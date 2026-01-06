# Processus de review et merge request

## Les règles pour review et MR

### Prérequis

!!! info "Voici les prérequis"
    - La personne qui code n'est pas la personne qui merge (sauf pour les cas d'auto merge précisés en bas de la page)
    - Les MR/review sont prioritaires si elles font partie du sprint courant ou urgence immédiate
    - Les MR/review doivent faire partir d'un ticket du sprint, sinon elles ne sont pas prioritaires
    - Les MR/review doivent avoir été testées avant
    - Les Review ne sont pas faites pour corriger ce que vous n'avez pas pris le temps de relire

### Fil conducteur pour dérouler la Review

!!! info "Mémo"
    La confiance n'exclue pas la vérification !

**L'essentiel (on lit vraiment et la totalité) :** &#128522;

- Est-ce que c'est compréhensible / lisible ?
- Est-ce que ça respecte les coding rules ?
- Est-ce qu'il y a des erreurs ?
- Est-ce qu'on a des tips pour optimiser (mais sans amoindrir la compréhension) ?
- Est-ce que je me porte garant de ce qui va être proddé ?

Si toutes ces questions sont validées on peut cliquer sur approuve 😁 👍

### Incompatible avec le process de review

!!! danger "Voici les ce qu'il faut éviter avec le process de review"
    ❌ C'est bon je te fais confiance... click
    
    ❌ On fait la review ensemble, je vous explique vite fait... et hop (ça c'est plutôt la présentation à l'équipe)
    
    ❌ J'ai la flemme de relire, les copains feront pour moi

---

## Tips pour se faire valider plus vite

### Faire des petites MR

- 1 sujet/ticket = 1 MR (pas de mélange, de refacto, de regroupement... "tiens en passant je vais ajouter ça")
- Si les MR sont grosses elles sont moins bien relues
- Si les MR embarquent plusieurs sujets, lorsqu'il faut revert c'est beaucoup moins facile


### Se relire avant d'envoyer aux autres

- On envoie sa MR en review, et on est le premier à la parcourir, comme si on relisait quelqu'un d'autre
- Si on charge le relecteur parce qu'il doit mettre 36000 commentaires, il va passer à côté de points importants
- Si le relecteur a l'impression de vous corriger tout le temps parce que ce n'est pas relu, il finira par baisser les bras

---

## Comment

### En ligne avec le codeur c'est plus rapide

- Pour poser les questions en live
- Pour échanger sur les tips

### Et sur JIRA

- Toute question et réponse (même posée et répondue en live) doit être écrite
    - Pour l'historique
    - Pour que les autres membres de l'équipe ou dev qui lisent le code soient informés