---
name: session-history-review
description: |
  回顾 opencode 历史会话消息，支持回溯最近 N 个会话。适用于提问"我们上一个 session 聊了什么"、"列出之前的会话内容"、"查找历史对话"、"回顾最近 N 个 session"等需要复盘旧 session、找回之前的聊天记录、任务上下文或决策细节的场景。
---

# Session 历史消息回顾指南

当用户要求回顾历史会话（上一个或多个）时，使用本技能从 opencode 的 SQLite 数据库（`~/.local/share/opencode/opencode.db`）中提取会话内容和消息文本。

## 🗄️ 核心情报：数据存储结构

所有会话数据存储在 SQLite 数据库：`~/.local/share/opencode/opencode.db`

**关键表**：

| 表 | 作用 | 关键字段 |
|---|---|---|
| `session` | 会话元数据 | `id`（**截断版**，只有前 ~16 字符）、`title`、`directory`、`time_created`、`time_updated` |
| `message` | 消息**元数据**（角色/模型/成本） | `session_id`（**完整版**，带随机后缀）、`time_created`、`data`(JSON: role/model/tokens) |
| `part` | 消息**实际内容**（text/tool 等） | `message_id`、`session_id`、`data`(JSON: type=text/tool/...) |

## ⚠️ 注意事项

1. **session_id 前缀不同**：
   - `session` 表的 `id` 是**截断版**（如 `ses_02cb5ec52ffe`）
   - `message` / `part` 表的 `session_id` 是**完整版**（如 `ses_02cb5ec52ffehpS1F4FYtJd2bK`）
   - 查询时**必须用 `LIKE 'ses_xxx%'`** 匹配，否则查不到！

2. **消息文本存在 `part` 表**，不在 `message` 表：
   - `message.data` 只有元数据（role、model、tokens、summary），**没有正文**
   - 正文在 `part.data` 中，`type=text` 时取 `text` 字段

## 🔍 标准查询流程

### 第一步：列出所有会话（按时间倒序）

```bash
python3 -c "
import os, sqlite3, datetime
db_path = os.path.expanduser('~/.local/share/opencode/opencode.db')
conn = sqlite3.connect(db_path)
cur = conn.cursor()
cur.execute('SELECT id, title, directory, time_updated FROM session ORDER BY time_updated DESC LIMIT 20')
for r in cur.fetchall():
    print(r[0][:16], '|', r[1][:50], '|', r[2][:60], '|', datetime.datetime.fromtimestamp(r[3]/1000))
"
```

### 第二步：用 LIKE 匹配完整 session_id 批量读取消息文本

```bash
python3 -c "
import os, sqlite3, json, datetime
db_path = os.path.expanduser('~/.local/share/opencode/opencode.db')
conn = sqlite3.connect(db_path)
cur = conn.cursor()
N = 3  # 回溯 N 个会话
cur.execute('SELECT id, title, time_updated FROM session ORDER BY time_updated DESC LIMIT ?', (N+1,))
sessions = cur.fetchall()[1:]  # 跳过当前会话
for sid, title, ts in sessions:
    print(f'\n########## 会话: {title} | {datetime.datetime.fromtimestamp(ts/1000).strftime(\"%m-%d %H:%M\")} ##########')
    cur.execute('SELECT p.data, m.time_created, m.data FROM part p JOIN message m ON p.message_id = m.id WHERE p.session_id LIKE ? ORDER BY m.time_created ASC, p.time_created ASC', (sid + '%',))
    for pdata, ts2, mdata in cur.fetchall():
        d = json.loads(pdata)
        m = json.loads(mdata)
        if d.get('type') == 'text' and d.get('text', '').strip():
            role = m.get('role', '?')
            dt = datetime.datetime.fromtimestamp(ts2/1000).strftime('%H:%M')
            print(f'===== [{dt}] {role.upper()} =====')
            print(d['text'])
"
```
