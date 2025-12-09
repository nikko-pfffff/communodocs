Marp est un outil pour faire des présentations en utilisant Markdown. Il permet de créer des diapositives rapidement et facilement, avec une syntaxe simple.<br>
:right_arrow: [marp.app](https://marp.app/) :left_arrow:

J'ai préféré ne rien installé et utiliser docker pour lancer la conversion de markdown vers une présentation.

```bash
docker run --rm -v $PWD:/home/marp/app/ -e LANG=$LANG -e MARP_USER="$(id -u):$(id -g)" marpteam/marp-cli GitLab_CI_Modularization_Presentation.md
```

Cette commande convertit le fichier markdown en slides HTML. Vous pouvez aussi générer un PDF en ajoutant l'option `--pdf` ou un PPTX avec `--pptx` etc...

La version HTML est light et peut être joué n'importe où.

Voici une présentation que j'ai fait sur la modularisation des pipelines GitLab CI.

[Version markdown](https://github.com/nikko-pfffff/communodocs/blob/main/communodocs/files/GitLab_CI_Modularization_Presentation.md)<br>
[Version HTML](https://github.com/nikko-pfffff/communodocs/blob/main/communodocs/files/GitLab_CI_Modularization_Presentation.html)