---
name: oci-manager
description: |
  使用 oci CLI 管理 Oracle Cloud Infrastructure (OCI) 虚拟机与云网络资源。
metadata:
  opencode:
    emoji: "☁️"
---

# Oracle Cloud Infrastructure (OCI) 资源管理指南

## 🔑 凭证初始化与依赖准备
请确保已配置 `~/.oci/config` 及对应的 API 私钥文件（如 `~/.oci/oci_api_key.pem`，权限 `600`）。

## 🔒 安全审计与高危红线
1. **【读操作放行，写操作请示】**：可以自由执行 `list`、`get` 等资源查询命令。
2. **【高危操作红线】**：涉及到任何 OCI 资源的**创建**、**更新**、**重启**或**删除**，**必须事先获得用户的明确批准**，绝对禁止私自操作！

## 🛠️ OCI 常用指令模版

### 1. Compute Instances (云主机)
```bash
# 列出 Compartment 下的虚拟机实例
oci compute instance list --compartment-id <YOUR_COMPARTMENT_ID>

# 获取单台虚拟机的详细信息
oci compute instance get --instance-id <YOUR_INSTANCE_ID>
```

### 2. Virtual Cloud Networks (虚拟云网络)
```bash
# 列出 VCN
oci network vcn list --compartment-id <YOUR_COMPARTMENT_ID>
```
