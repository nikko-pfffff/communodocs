# Tester son MCP 

## En mode debug standlalone

### Si on developpe des SDK
comme FastMCP en python ou McpServer en javascript, on a la possibilité de lancer inspector afin de tester le server.

```python
from mcp.server.fastmcp import FastMCP
```
```bash
mcp dev server.py
```

```javascript
import {
  McpServer,
  ResourceTemplate,
} from "@modelcontextprotocol/sdk/server/mcp.js";
```
```bash
npx @modelcontextprotocol/inspector node ./index.mjs
```

On a ainsi une page web qui nous permet de tester les méthodes (tools, ressources, prompts...) de notre server MCP

### Si on developpe son MCP avec Gradio

Gradio:

- converti les fonctions en tools
- fait le mapping des  arguments
- met en place une communication HTTP+SSE
- créé 1 interface web pour tester et un MCP server endpoint

## En mode debug avec un LLM

Il existe un client MCP light `Tiny Agent` qui permet facilement de connecter son MCP à un agent pour tester l'utilisation du server.<br>
Cela utilise des models sur HuggingFace, il faut donc être connecté et avoir un token API avec les permissions `Make calls to Inference Providers`.

Un petit json de configuration (my-agent/agent.json)
```json
{
    "name": "playwright-agent",
    "description": "Agent with Playwright MCP server",
    "model": <le model de votre choix>,
    "provider": <le provider qui va bien>,
    "servers": [
        {
            "type": "stdio",
            "command": "npx",
            "args": ["@playwright/mcp@latest"]
        }
    ]
}
```
et
```bash
npx @huggingface/tiny-agents run ./my-agent
```