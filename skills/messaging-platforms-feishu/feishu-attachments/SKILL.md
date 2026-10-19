# Feishu File Attachments (飞书发文件防吞指南)

## 问题背景 (The Problem)
在飞书（尤其是移动端）发送非图片类文件（如 `.pdf`, `.docx`, `.zip`）时，如果将文字与文件混合在同一条消息中发送，或者使用了默认的媒体格式，飞书底层的 API 会将其识别并转换为“富文本”或“图片缩略图”卡片，导致手机端用户看到一个不可点击、无法下载的图标。

## 解决方案 (The Solution)
为了确保发送的文件在飞书手机端能被正确渲染为**原生的、带有下载按钮和文件大小显示的文件卡片**（Native File Card），请遵循以下规范：

1. **必须携带 `forceDocument: true` 参数**：在调用 `message` 工具时，强制开启原生文件/文档模式。
2. **使用极简的附件参数**：直接在 `attachments` 中使用 `path` 和 `type: "file"`。
3. **文本与文件分离发送**：先发一段解释性文字，然后再单独调用一次 `message` 专门发送纯文件附件。

## 代码示例 (Tool Call Example)

```json
{
  "action": "send",
  "channel": "feishu",
  "target": "chat:oc_xxxxx",
  "forceDocument": true,
  "attachments": [
    {
      "path": "/path/to/workspace/example.docx",
      "type": "file"
    }
  ]
}
```
