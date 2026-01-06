Il est important, pour que l'agent écrive du code pertinent, que nous lui passions un prompt système pour définir des guidelines générales qui ne doivent pas être répétées dans chaque requête utilisateur.

Si vous avez des coding rules c'est ici qu'il faut les écrire.

Vous pouvez donner les détails qui orienteront l'agent dans la rédaction du code:

* Quel langage de programmation utilisez vous ?
* Quel framework utilisez vous ?
* Quels sont les patterns de code que vous utilisez habituellement ?
* Y a-t-il des contraintes particulières à respecter (performance, sécurité, etc.) ?

Si vous avez des exemples de code, c'est aussi le moment de les fournir à l'agent pour qu'il puisse s'en inspirer.

De même, si vous avez des règles de nommage de commits, des structures de projet, des arborescences, des spécificités de test etc... 

Avec Claude code, c'est le fichier CLAUDE.md qui peut être pré-créé avec la commande /init dans le chat.

Avec Github Copilot, c'est dans .github/copilot-instructions.md qu'il faut écrire ces instructions.
