# Feishu Media & Images (飞书发送图片防吞指南)

## 问题背景 (The Problem)
在飞书（尤其是移动端）发送图片时，如果将图片放在 `attachments` 数组中，或者将图片与文字（`message` 字段）在同一个 API 调用中混合发送，飞书底层 API 会将其强制转换为“富文本 (Rich Text)”或普通“文件 (File)”卡片。

## 解决方案：纯媒体发送姿势 (The Solution)
为了确保图片在飞书聊天窗口中被渲染为**原生的大图气泡 (Native Image Bubble)**，请遵循以下两条铁律：

### 1. 使用顶层的 `media` 字段
直接使用 `message` 工具最外层的 `media` 参数指定图片路径或 URL。

### 2. 绝对不能与文字混发 (Zero Text Mix)
如果需要配文说明，**必须发起两次独立的工具调用**：
- 第一次调用：只传 `message` 发送纯文字。
- 第二次调用：只传 `media` 发送纯图片（**完全省略 `message` 和 `attachments` 字段**）。

## 代码示例 (Tool Call Example)

```json
{
  "action": "send",
  "channel": "feishu",
  "target": "chat:oc_xxxxx",
  "media": "/tmp/mockup.jpg"
}
```
