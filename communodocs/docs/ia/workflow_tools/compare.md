J'ai testé flowise.ai, n8n.io et Microsoft Copilot Studio comme outils de workflow pour orchestrer des LLMs et d'autres outils.<br>
Mes tests se sont faits en containers pour flowise et n8n.<br>
[FlowiseIA](https://github.com/FlowiseAI/Flowise)<br>
[n8n](https://github.com/n8n-io/n8n)<br>

## MCP ##
Entre flowise, n8n et copilot studio, flowise se démarque car on peut utiliser des MCP en stdio.<br>
Pour n8n et copilot studio on ne peut y accéder qu'en HTTP.<br>
L'accès en stdio permet de pouvoir utiliser des MCP locaux ou par exemple dans flowise avoir un container server MCP démarré et accessible dans le container flowise.

Je vais continuer de chercher pour voir s'il y a une solution pour n8n.

## OpenAPI Doc ##
Je n'ai pas encore trouvé non plus côté copilot studio et N8N un bloc qui nous permet de transformer une open API doc en Tools.<br>
Flowise le propose à travers les blocs langchain.