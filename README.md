<img src="images/Gravix Layer.png" width="100%"/>

<!-- Links -->
<p align="center">
  <a href="https://gravixlayer.com/" style="color: #06b6d4;">Website</a> •
  <a href="https://platform.gravixlayer.com/signin?callbackUrl=%2F" style="color: #06b6d4;">Platform</a> •
  <a href="https://docs.gravixlayer.com/docs/getting-started/" style="color: #06b6d4;">Docs</a> •
  <a href="#" style="color: #06b6d4;">Discord</a>
</p>

# Gravix Layer Code Recipes

Welcome to the ultimate collection of AI-powered code recipes for developers.

The Gravix Layer Code Recipes is your go-to resource for building cutting-edge AI applications with [Gravix Layer](https://gravixlayer.com/). Whether you're a seasoned developer or just starting your AI journey, these modular recipes will accelerate your development process.

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

### 3. Explore the Recipes
Browse the recipes below, copy the code, and integrate into your projects.

## Code Recipes

> **Note**: Each recipe is designed to be modular.

| Recipe | Description | Open |
| ------ | ----------- | ---- |
| [**MCP Integration**](recipes/mcp/model_context_protocol.ipynb) | Implement Model Context Protocol for standardized AI data access and tool integration. | [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/gravix-layer/code-recipes/blob/main/recipes/mcp/model_context_protocol.ipynb) |


## Explore Further

Ready to level up your AI development? Check out these essential resources:

<p>
  • <a href="https://docs.gravixlayer.com/docs/getting-started/" style="color: #06b6d4;"><strong>Developer Documentation</strong></a> - Complete guides and tutorials<br>
  • <a href="https://docs.gravixlayer.com/docs/supported-models" style="color: #06b6d4;"><strong>Supported Models</strong></a> - Browse our model catalog<br>
  • <a href="https://platform.gravixlayer.com/" style="color: #06b6d4;"><strong>Platform Dashboard</strong></a> - Manage your deployments
</p>

## Contributing

We welcome contributions from the developer community. If you have a recipe that could help other developers, we want to hear from you!

**Ways to contribute:**
- Submit new recipes
- Report bugs or issues
- Suggest improvements
- Improve documentation

Ready to contribute? Open a pull request or reach out to us!

---

<div align="center">

### Gravix Layer

[**Start Building**](https://platform.gravixlayer.com/signin?callbackUrl=%2F) • [**Join Community**](https://discord.gg/gravixlayer)

</div>


