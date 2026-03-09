---
title: Oh My API
emoji: 🐾
colorFrom: pink
colorTo: purple
sdk: docker
pinned: true
license: mit
---

<div align="center">
<br/>

```
 ██████╗ ██╗  ██╗      ███╗   ███╗██╗   ██╗      █████╗ ██████╗ ██╗
██╔═══██╗██║  ██║      ████╗ ████║╚██╗ ██╔╝     ██╔══██╗██╔══██╗██║
██║   ██║███████║      ██╔████╔██║ ╚████╔╝      ███████║██████╔╝██║
██║   ██║██╔══██║      ██║╚██╔╝██║  ╚██╔╝       ██╔══██║██╔═══╝ ██║
╚██████╔╝██║  ██║      ██║ ╚═╝ ██║   ██║        ██║  ██║██║     ██║
 ╚═════╝ ╚═╝  ╚═╝      ╚═╝     ╚═╝   ╚═╝        ╚═╝  ╚═╝╚═╝     ╚═╝
```

**统一 AI API 代理网关**

[![Version](https://img.shields.io/badge/CLIProxyAPI-v6.8.37-0f172a?style=flat-square&labelColor=0f172a&color=38bdf8)](.)
[![SDK](https://img.shields.io/badge/SDK-Docker-0f172a?style=flat-square&labelColor=0f172a&color=6366f1)](.)
[![Status](https://img.shields.io/badge/Status-Running-0f172a?style=flat-square&labelColor=0f172a&color=22c55e)](.)
[![License](https://img.shields.io/badge/License-MIT-0f172a?style=flat-square&labelColor=0f172a&color=f59e0b)](.)

<br/>

> 一个运行在 HuggingFace Space 上的高性能 AI API 代理，<br/>
> 统一接入 Claude · Gemini · OpenAI · Codex · Vertex AI

<br/>

</div>

---

## ⚡ 支持的模型

| 提供商 | 支持模型 | 接入方式 |
|--------|----------|----------|
| 🟣 **Anthropic Claude** | Claude 4.5 / 4.6 全系列 | API Key |
| 🔵 **Google Gemini** | Gemini 3.1 Pro / Flash | API Key |
| 🟢 **OpenAI** | GPT-5.1 / GPT-5.2 | API Key |
| 🟡 **Vertex AI** | Gemini on Vertex | 服务账号 |
| ⚪ **OpenAI-compat** | 任意兼容 OpenAI 格式的服务 | API Key |

---

## 🚀 快速接入

将你的 API Base URL 替换为本 Space 地址即可：

```bash
# 原始请求
curl https://api.anthropic.com/v1/messages ...

# 替换为本代理
curl https://[user_name]-[space_name].hf.space/v1/messages \
  -H "x-api-key: 你的访问密钥" \
  -H "Content-Type: application/json" \
  -d '{"model": "claude-4-6-sonnet", "max_tokens": 1024, "messages": [{"role": "user", "content": "Hello"}]}'
```

### Python 示例

```python
import anthropic

client = anthropic.Anthropic(
    api_key="你的访问密钥",
    base_url="https://[user_name]-[space_name].hf.space",
)

message = client.messages.create(
    model="claude-4-6-sonnet",
    max_tokens=1024,
    messages=[{"role": "user", "content": "你好！"}]
)
print(message.content)
```

### OpenAI SDK 兼容

```python
from openai import OpenAI

client = OpenAI(
    api_key="你的访问密钥",
    base_url="https://[user_name]-[space_name].hf.space/openai",
)

response = client.chat.completions.create(
    model="gpt-5.1",
    messages=[{"role": "user", "content": "你好！"}]
)
```

---

## 🏗️ 技术架构

```
┌─────────────────────────────────────────────────────┐
│                HuggingFace Space                     │
│                                                     │
│   Client Request                                    │
│        │                                            │
│        ▼                                            │
│   ┌─────────────┐        ┌──────────────────────┐   │
│   │             │        │   Supabase PostgreSQL │   │
│   │ CLIProxyAPI │◄──────►│   Token Store         │   │
│   │  :7860      │        │   Config Store        │   │
│   │             │        └──────────────────────┘   │
│   └──────┬──────┘                                   │
│          │                                          │
└──────────┼──────────────────────────────────────────┘
           │
    ┌──────┴───────┐
    │              │
    ▼              ▼
 Anthropic      Google
 Claude API     Gemini API
```

---

## ⚙️ 部署配置

### 必需环境变量

在 HuggingFace Space **Settings → Secrets** 中配置：

| 变量名 | 说明 | 示例 |
|--------|------|------|
| `PGSTORE_DSN` | PostgreSQL 连接字符串 | `postgresql://user:pass@host:6543/postgres?sslmode=require` |
| `MANAGEMENT_PASSWORD` | 管理后台密码 | `your-secure-password` |

## 🔑 添加 API Key

服务启动后，通过管理接口添加你的 API Key：

```bash
# 添加 Claude Key
curl -X POST https://[user_name]-[space_name].hf.space/management/clients \
  -H "Authorization: Bearer $MANAGEMENT_PASSWORD" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "my-client",
    "claude-keys": ["sk-ant-api03-xxxxx"]
  }'

# 添加 Gemini Key  
curl -X POST https://[user_name]-[space_name].hf.space/management/clients \
  -H "Authorization: Bearer $MANAGEMENT_PASSWORD" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "my-client",
    "gemini-keys": ["AIzaSy-xxxxx"]
  }'
```

---

<div align="center">
<br/>

Built with ❤️ · Powered by [CLIProxyAPI](https://github.com/eceasy/cli-proxy-api) · Hosted on [HuggingFace](https://huggingface.co)

<br/>

</div>