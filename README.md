# OpenCode Configuration Template (Cindy Persona & Higress AI Gateway) 🌸🚀

这是一份开箱即用、**100% 脱敏（零个人隐私、零明文密钥）** 的 [OpenCode](https://opencode.ai) 配置模板仓库，内置了 **Higress AI Gateway** 多模型配置（支持 `gemini-4-argon`、`gemini-3.8-flash`、`kimi-k3`、`glm-5.3`、`gpt-5.6-luna` 等）、**Cindy 贴身技术秘书人设框架**、图片视觉预分析插件以及实用运维 Skills。

---

## 📂 目录结构说明

```text
~/.config/opencode/
├── opencode.jsonc                  # 核心模型与网关提供商配置 (Higress / LiteLLM)
├── agent/
│   └── build.md                    # Primary Agent 启动仪式与上下文自动挂载配置
├── Cindy/
│   ├── SOUL.md                     # Agent 核心性格与人设定义
│   ├── system_prompt.txt           # 系统提示词模板
│   ├── USER.md                     # 用户背景与技术栈偏好模板 (自行填写)
│   ├── MEMORY.md                   # 开发红线守则 (防死循环、防越权、防泄密)
│   ├── TOOLS.md                    # 本地工具与资产目录索引
│   └── projects/
│       └── example_project.md      # 项目架构知识库模板
├── plugins/
│   └── vision-translator.ts.bak    # 纯文本模型图片转文字预分析插件 (按需去掉 .bak 启用)
├── skills/
│   ├── session-history-review/     # 本地 SQLite 会话历史回溯技能
│   ├── gcp-manager/                # GCP 资源只读审计与运维规范模板
│   ├── oci-manager/                # OCI 云主机运维规范模板
│   └── messaging-platforms-feishu/ # 飞书原生大图与文档卡片防折叠指南
└── users-it-assests/               # 个人服务器拓扑、数据库与服务列表模板
```

---

## 🚀 快速上手指南 (3 步搞定)

### 第一步：克隆模板到本地 OpenCode 配置目录

- **Linux / macOS**：
  ```bash
  git clone https://github.com/nvd11/opencode-config-template.git ~/.config/opencode
  ```
- **Windows (PowerShell)**：
  ```powershell
  git clone https://github.com/nvd11/opencode-config-template.git "$env:USERPROFILE\.config\opencode"
  ```

### 第二步：填入您的 API Key

打开 `~/.config/opencode/opencode.jsonc`，将 `YOUR_HIGRESS_API_KEY_HERE` 替换为您专属的 Higress API Key：

```jsonc
"higress": {
  "name": "Higress AI Gateway",
  "options": {
    "baseURL": "https://higress.jppwl.asia/v1",
    "apiKey": "sk-your-exclusive-api-key"
  },
  ...
}
```

### 第三步：自定义个人背景（可选）

编辑 `Cindy/USER.md` 填入您的称呼与常用技术栈，然后运行 `opencode` 启动即可！在 OpenCode 中可随时按 `Ctrl + A` 或输入 `/model` 在 `Gemini 4 Argon`、`Gemini 3.8 Flash`、`Kimi K3`、`GLM 5.3` 等模型间自由切换。
