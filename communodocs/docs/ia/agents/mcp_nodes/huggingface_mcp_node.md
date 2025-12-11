Comment faire plus simple ?

En utilsant la classe Agent du module huggingface_hub :
```python
import os

from huggingface_hub import Agent

agent = Agent(
    model="Qwen/Qwen2.5-72B-Instruct",
    provider="nebius",
    servers=[
        {
            "command": "npx",
            "args": [
                "mcp-remote",
                "http://localhost:7860/gradio_api/mcp/sse"  # Your Gradio MCP server
            ]
        }
    ],
)
```