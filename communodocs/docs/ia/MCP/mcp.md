**A Retenir :**

1. Les capacités d'un serveur sont des tools, ressources, prompts qui seront exposés via le protocol MCP.
2. La sortie d'un server MCP c'est 25000 tokens max, donc gérez la pagination lorsque vous l'écrivez.
3. Les erreurs doivent être retournées dans la structure json de la réponse pour permettre au LLM d'interpréter l'erreur et de la gérer.
4. Par défault un serveur MCP exécute ses commandes dans son répertoire d'installation, il faut récupérer le working dir dans le context.

```python
    context = mcp.get_context()
    roots_result = await context.session.list_roots()
    working_dir = roots_result.roots[0].uri.path
```
