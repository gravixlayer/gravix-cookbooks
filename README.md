<img src="images/Gravix Layer.png" width="100%"/>

<!-- Links -->
<p align="center">
  <a href="https://gravixlayer.com/" style="color: #06b6d4;">Website</a> •
  <a href="https://platform.gravixlayer.com/signin?callbackUrl=%2F" style="color: #06b6d4;">Platform</a> •
  <a href="https://docs.gravixlayer.com/docs/getting-started/" style="color: #06b6d4;">Docs</a> •
  <a href="https://discord.gg/dTsjM3YTxz" style="color: #06b6d4;">Discord</a>
</p>

# Gravix Layer Cookbooks

Welcome to the ultimate collection of AI-powered cookbooks for developers.

The Gravix Layer Cookbooks is your go-to resource for building cutting-edge AI applications with [Gravix Layer](https://gravixlayer.com/). Whether you're a seasoned developer or just starting your AI journey, these modular cookbooks will accelerate your development process.

## Why Gravix Layer?

- **Lightning Fast**: Sub-second inference speeds
- **Global Scale**: Multi-region deployment ready
- **Developer First**: Simple APIs, powerful results
- **Cost Effective**: Pay only for what you use.
- **OpenAI Compatible**: Drop-in replacement for OpenAI API

## Quick Start

Get started with Gravix Layer in minutes using our OpenAI-compatible API:

### 1. Get Your API Key
Sign up for free at [Gravix Layer Platform](https://platform.gravixlayer.com/signin?callbackUrl=%2F) and generate your API key from the dashboard.

### 2. Make Your First Request

**Click to view code for your preferred language:**

<details>
<summary><b>cURL</b></summary>

```bash
curl -X POST https://api.gravixlayer.com/v1/inference/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $GRAVIXLAYER_API_KEY" \
  -d '{
  "model": "llama3.1:8b-instruct-fp16",
  "messages": [
    {
      "role": "user",
      "content": "Hello"
    }
  ]
}'
```

</details>

<details>
<summary><b>Python</b></summary>

```python
import os
from openai import OpenAI

client = OpenAI(
  api_key = os.environ.get("GRAVIXLAYER_API_KEY"),
  base_url = "https://api.gravixlayer.com/v1/inference"
)

completion = client.chat.completions.create(
  model="llama3.1:8b-instruct-fp16",
  messages=[
    {"role": "user", "content": "Hello!"}
  ]
)

print(completion.choices[0].message)
```

</details>

<details>
<summary><b>JavaScript</b></summary>

```javascript
import OpenAI from "openai";

const openai = new OpenAI({
  apiKey: process.env.GRAVIXLAYER_API_KEY,
  baseURL: "https://api.gravixlayer.com/v1/inference"
});

async function ChatCompletion() {
  const completion = await openai.chat.completions.create({
    messages: [{ role: "user", content: "Hello" }],
    model: "llama3.1:8b-instruct-fp16",
  });
  console.log(completion.choices[0]);
}

ChatCompletion();
```

</details>

### 3. Explore the Cookbooks
Browse the cookbooks below, copy the code, and integrate into your projects.

## Cookbooks

> **Note**: Each cookbook is designed to be modular.

| Cookbook | Description | Open |
| ------ | ----------- | ---- |
| [**MCP Implementation**](Model%20Context%20Protocol.ipynb) | Build MCP server and client using a working to do application example. | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/gravixlayer/Code-Recipes/blob/main/Model%20Context%20Protocol.ipynb) |
| [**Automated GitHub Changelog**](Automated%20Changelog%20Generator_N8N.md) | Effortlessly generate and maintain a professional, AI-powered changelog in your GitHub repository after every push, using n8n. | [View Cookbook](Automated%20Changelog%20Generator_N8N.md) |
| [**Text-to-SQL Multi-Agent System**](Text-to-SQL.ipynb) | Multi-agent AI system for converting natural language to SQL and querying the database. | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/gravixlayer/Code-Recipes/blob/main/Text-to-SQL.ipynb) |
| [**Stock Analysis using LangChain Orchestration**](Agentic%20Stock%20Analysis.ipynb) | Analyze stocks with financial data, technical indicators, news sentiment, and LLM-powered insights in a streamlined workflow. | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/gravixlayer/Code-Recipes/blob/main/Agentic%20Stock%20Analysis.ipynb) |
| [**Agentic Email Triage Workflow**](Email%20Triage%20Agent.ipynb) | Automated email processing with intelligent prioritization, summarization, and multi-format reporting using agent orchestration. | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/gravixlayer/Code-Recipes/blob/main/Email%20Triage%20Agent.ipynb) |
| [**Simple RAG**](Simple%20RAG.ipynb) | Naive retrieval-augmented generation example: ingest docs, index vectors, and run basic retrieval. | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/gravixlayer/Code-Recipes/blob/main/Simple%20RAG.ipynb) |
| [**Agentic RAG Cookbook**](Agentic%20RAG%20Cookbook.ipynb) | Multi Agents that ingest PDFs, index vectors, retrieve context, and generate sourced answers. | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/gravixlayer/Code-Recipes/blob/main/Agentic%20RAG%20Cookbook.ipynb) |
| [**Contextual RAG**](Contextual%20RAG.ipynb) | Combine retrieval with dynamic prompts and context windows for precise answers. | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/gravixlayer/Code-Recipes/blob/main/Contextual%20RAG.ipynb) |

## Explore Further

Ready to level up your AI development? Check out these essential resources:

<p>
  • <a href="https://docs.gravixlayer.com/docs/getting-started/" style="color: #06b6d4;"><strong>Developer Documentation</strong></a> - Complete guides and tutorials<br>
  • <a href="https://docs.gravixlayer.com/docs/supported-models" style="color: #06b6d4;"><strong>Supported Models</strong></a> - Browse our model catalog<br>
  • <a href="https://platform.gravixlayer.com/" style="color: #06b6d4;"><strong>Platform Dashboard</strong></a> - Manage your deployments
</p>

## Contributing

We welcome contributions from the developer community. If you have a cookbook that could help other developers, we want to hear from you!

**Ways to contribute:**
- Submit new cookbooks
- Report bugs or issues
- Suggest improvements
- Improve documentation

Ready to contribute? Open a pull request or reach out to us!

---

<div align="center">

### Gravix Layer

[**Start Building**](https://platform.gravixlayer.com/signin?callbackUrl=%2F) • [**Join Community**](https://discord.gg/dTsjM3YTxz)

</div>


