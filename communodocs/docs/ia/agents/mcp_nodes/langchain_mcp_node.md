J'ai écrit un exemple de noeud qu'on pourrait utiliser dans une flotte.<br>
Pour l'instant je suis parti sur Bedrock parce que c'était dans mon environnement technique.

Les variables d'environnement sont une contrainte qui peut être contournée avec l'utilisation de AWS STS pour récupérer les crédentials en live.

Un exemple de fichier de configuration JSON pour le MCP est donné dans le repo communodocs [ici](https://github.com/nikko-pfffff/communodocs/blob/main/communodocs/files/aws_doc_mcp.json).

```python
import os

from mcp import ClientSession, StdioServerParameters
from mcp.client.stdio import stdio_client
from langchain_mcp_adapters.tools import load_mcp_tools
from langgraph.prebuilt import create_react_agent
from langchain_aws import ChatBedrock

from dotenv import load_dotenv

import json
import asyncio

class McpAgentLangChain:

    def __init__(self, configuration_file: str, model):
        self.configuration_file = configuration_file
        self.model = model

    async def initialize(self):

        with open(self.configuration_file, "r") as file:
            json_configuration = json.load(file)

        server_params = StdioServerParameters(
            command=json_configuration["command"],
            args=json_configuration["args"]
        )

        async with stdio_client(server_params) as (read, write):

            async with ClientSession(read, write) as session:

                await session.initialize()
                print("Session Initialized.")

                tools = await load_mcp_tools(session)
                print(f"Loaded tools: {[tool.name for tool in tools]}")

                mcp_agent_langchain = create_react_agent(self.model, tools)

                self.agent = mcp_agent_langchain
                return mcp_agent_langchain

    async def run(self, messages):

        response = await self.agent.ainvoke({
            "messages": messages
        })

        return response

if __name__ == "__main__":

    load_dotenv()
    model = ChatBedrock(
        model_id=<mon infrerence profile>,
        provider="anthropic",
        temperature=0,
        aws_access_key_id=os.getenv("AWS_ACCESS_KEY_ID"),
        aws_secret_access_key=os.getenv("AWS_SECRET_ACCESS_KEY"),
        aws_session_token=os.getenv("AWS_SESSION_TOKEN")
    )

    agent = McpAgentLangChain("aws_doc_mcp.json", model)
    asyncio.run(agent.initialize())
    messages = [
        {"role": "user", "content": "Can you tell me how to create an EC2 instance ?"},
    ]

    response = asyncio.run(agent.run(messages))
    print("Agent Response:\n", response["messages"][-1].content)
```