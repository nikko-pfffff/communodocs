J'avais déjà lu des articles et expérimenté un peu sur la façon de rédiger des prompts...
Mais ce qui a complété le plus ce que j'avais déjà noté, est un article de newsletter de Generation IA: "[Anatomie d'un prompt](https://generationia.flint.media/p/anatomie-un-prompt)" et c'est d'ailleurs ce qui constitue la majeure partie de ce document.

Voici ce que je garde en tête suite à ces expérimentations et à cet article:

### Principes généraux

- Etre clair, précis, succin
- Utiliser des listes, des balises
- Utiliser le Markdown qui est très bien géré par les LLMs
- Ne pas oublier les règles dans le style guardrails

### Concepts fondamentaux

**Auto-régression** :<br />
Les LLMs génèrent le texte mot par mot en se basant sur tous les mots précédents : la qualité de la génération dépend de ce qui a été écrit précédemment.

### Techniques de prompting

**La complétion** :<br />
Rédiger soi-même le début dans le style souhaité, puis laisser le LLM continuer naturellement.

**L'approche indirecte** :<br />
Au lieu de demander directement le résultat, faire parler le modèle d'abord (ex: méthode des 5W pour un résumé).

**Chain of Thought (chaîne de pensée)** :<br />
Guider le modèle étape par étape plutôt que demander le résultat final.

**Décomposer les tâches complexes en tâches simples** :<br />
Simplifier les problèmes en sous-problèmes plus gérables.

**Préférer plusieurs prompts simples à un seul prompt complexe (Keep It Simple)** :<br />
Faciliter la compréhension et la gestion des réponses.

### Bonnes pratiques pour une réponse efficace

**Human in the loop** :<br />
Placer l'expertise humaine au cœur du processus.

**Exploitation de la capacité générative** :<br />
Demander plusieurs options et laisser l'humain choisir.

**Structure claire** :<br />
Organiser le prompt en sections (notes, structure, contraintes, exemples).

**Instructions précises** :<br />
Donner des règles détaillées sur le style, la structure et les contraintes.

**Interaction itérative** :<br />
Valider les étapes intermédiaires avant de continuer.
