J'ai eu a tester Microsoft Copilot Studio récemment pour un projet interne.<br><br>
Le but du projet est de créer un agent conversationnel dans Microsoft Teams qui puisse répondre aux questions de type RUN de niveau 1 des employés en utilisant :

- les ressources internes (Confluence, JIRA, etc...)
- des documentations techniques officielles (AWS, Terraform, etc...)
- un LLM
<br>
<br>

Voici mes premiers retours d'expérience:

**Points positifs:**

- Intégration simple dans Microsoft Teams
- Utilisation des topics pour les réponses/flows pré définies avec possibilité de ne pas appeler de LLM
- Interface pour les non dev
- Sûrement pratique avec les produits saas (mais dans mon cas d'usage nous utilisions des services non saas)

**Contournements**

- Pour gérer le non accès direct aux services internes, il a fallu :<br>

    - Passer par Microsoft Power Automate pour effectuer des requêtes HTTP sur l'API du produit hébergé en interne (JIRA, Confluence, etc...)
    - Créer un Custom Connector pour accéder à Confluence depuis Power Automate
    - Être dans un environnement avec des IPs whitelistées au niveau du WAF (attention aux changements d'IPs)
    - Gérer l'authentification
        - Passage de tokens dans les headers ou body (nécessité d'un job power automate intermédiaire si la localisation du token change entre header et body)
        - Avoir une solution de gestion des secrets pour stocker les tokens.
        - Activer secure inputs et secure outputs dans les flows power automate pour éviter que les tokens soient visibles dans les logs. (A voir lorsqu'il seront stockés en tant que secrets)
        - Avoir un compte de service avec une clé API JIRA.

**Points négatifs / limites rencontrées:**
 
- Il y a eu une mise à jour, avec des caractères nouvellement refusés dans le prompt, j'ai du refaire tout le prompt.

- Il y a eu une mise à jour des IPs de l'environnement, tout a été bloqué, perte de l'accès jusqu'au nouveau whitelistage.

- Les debugs des flows power automate sont faisables mais pas toujours évidents.

- Le debug de l'agent dans copilot studio est bien plus compliqué.

- Les LLMs disponibles sont limités.

**Ma conclusion:**

C'est un produit assez fermé dans une boite opaque, mais c'est normal c'est du low-code.<br>
L'accès aux services internes est compliqué.<br>
Dans notre cas, les outils qu'on fournit à l'agent sont limités car très/trop précis dans les requêtes et les paramètres qu'on passe car on doit tout écrire dans un flow détaillé, on perd le bénéfice du LLM.<br>
Il est possible d'accéder à des serveurs MCP hébergés en interne, moyennant une gestion d'authentification et de sécurité.<br>
Le prompt n'est pas simple à rédiger pour avoir un comportement reproductible constant.<br>