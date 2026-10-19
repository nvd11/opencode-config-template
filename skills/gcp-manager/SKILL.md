---
name: gcp-manager
description: |
  使用 gcloud CLI 管理 Google Cloud Platform (GCP) 资源。适用于查询、列出、监控或调用 Compute Engine 实例、Cloud Storage 存储桶、BigQuery 视图、GKE 容器集群以及 Dataflow 任务。
metadata:
  opencode:
    emoji: "☁️"
---

# Google Cloud Platform (GCP) 资源管理指南

## ⚠️ 核心环境与执行路径
请确保本地环境已配置好 `gcloud` SDK 及对应的 Service Account 或 ADC 凭证：

- **默认服务账户/账号**：`<YOUR_GCP_ACCOUNT>`
- **默认工作项目 (Project)**：`<YOUR_GCP_PROJECT_ID>`

---

## 🔒 安全审计与高危红线
1. **【读操作放行，写操作请示】**：可以自由执行 `list`、`describe`、`get`、`ls` 等资源查询与审计命令。
2. **【高危操作红线】**：涉及到任何资源的**创建 (`create`)、更新 (`update`/`patch`)、重启 (`restart` / `rebuild`) 或是删除 (`delete`/`remove`)**，**必须事先获得用户的明确批准**，绝对禁止私自篡改线上生产/测试环境！

---

## 🛠️ GCP 常用资源控制指令模版

### 1. Compute Engine (虚拟机主机)
```bash
gcloud compute instances list
gcloud compute instances describe [INSTANCE_NAME] --zone=[ZONE]
```

### 2. Cloud Storage (GCS 云存储)
```bash
gcloud storage ls
gcloud storage ls gs://[BUCKET_NAME]/
```

### 3. BigQuery (大数据分析)
```bash
bq ls
bq show [PROJECT_ID]:[DATASET_ID]
```

### 4. GKE (Google Kubernetes Engine)
```bash
gcloud container clusters list
gcloud container clusters get-credentials [CLUSTER_NAME] --zone=[ZONE]
```

### 5. Dataflow (Apache Beam ETL 管道)
```bash
gcloud dataflow jobs list
```
