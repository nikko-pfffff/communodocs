## Objectif

Définir un cadre, des bonnes pratiques, des règles de nommage et d'utilisation pour permettre à chacun de s'y retrouver en codant et en lisant le code des autres membres de l'équipe.

## Bonnes pratiques

### Pensez au relecteur / mainteneur en codant, soyez le plus clair possible

* Noms de variables parlantes, pas de lettre seule
* Pas plus de 3 tests sur une seule ligne (and, or, =, > etc...)
* Pas de ligne à rallonge (lire sans scroller c'est mieux 😊)
    + Taille maximum 79 chars, caractère de changement de ligne : back slash `\`
* Pas de ternaire ou structure complexe, se limiter aux expressions simples
* Utiliser les espaces pour indenter
* Une ligne par import, pas de `import sys, os, json, pouet`

### Utilisez des linters et des formatters

Utiliser des linters pour vérifier le code

#### Bash

Passez *Shellcheck* sur les scripts Shell

#### Python

Utilisez :
* *pylint* pour vérifier le code (activable dans VSCode)
* black pour formater le code Python

#### Terraform

Utilisez :
* `terraform validate` pour valider la syntaxe
* `terraform fmt` pour formater le code
* `tflint` pour vérifier les bonnes pratiques
* `terrascan` pour vérifier la sécurité

### Ne pas stocker de secrets dans le code

!!! Attention
    Aucun secret ne doit se trouver dans le code, utiliser un espace dédié pour les stocker (Vault, AWS Secrets Manager, etc...).

## VSCode

### Extensions utiles

#### Système

* Ansible
* Remote Development
* Remote - SSH
* WSL
* Docker
* DevContainers
* RPM Spec
* systemd-unit-file
* XML
* YAML

#### Python

* Python
* Pylance
* Pylint

#### Bash

* ShellCheck
* Bash Beautify

#### Git

* Gitlab Workflow

#### Hashicorp

* Hashicorp HCL
* Hashicorp Terraform
* Hashicorp Vault

#### Hébergements

* AWS Toolkit
* AWS Boto3
* Kubernetes

### Utilisez Dev Container

Pour un projet Python standard, vous pouvez créer le fichier suivant :

```json title=".devcontainer/devcontainer.json"
// For format details, see https://aka.ms/devcontainer.json. For config options, see the
// README at: https://github.com/devcontainers/templates/tree/main/src/python
{
    "name": "Python 3",
    // Or use a Dockerfile or Docker Compose file. More info: https://containers.dev/guide/dockerfile
    "image": "mcr.microsoft.com/devcontainers/python:0-3.13",
    // "build": {
    //     // Path is relataive to the devcontainer.json file.
    //     "dockerfile": "Dockerfile"
    // },
    // Features to add to the dev container. More info: https://containers.dev/features.
    // "features": {},
    // Use 'forwardPorts' to make a list of ports inside the container available locally.
    // "forwardPorts": [],
    // Use 'postCreateCommand' to run commands after the container is created.
    "postCreateCommand": "export PIP_INDEX_URL=<nom de l'index interne>; pip install --upgrade pip; pip3 install --user -r requirements.txt",
    // Configure tool-specific properties.
    // "customizations": {},
    // Uncomment to connect as root instead. More info: https://aka.ms/dev-containers-non-root.
    // "remoteUser": "root"
    "remoteUser": "vscode",
    "mounts": [
        "source=${localEnv:HOME}/.aws,target=/home/vscode/.aws,type=bind,consistency=cached"
    ],
    "customizations": {
        "vscode": {
            "extensions": [
                "njpwerner.autodocstring",
                "oderwat.indent-rainbow",
                "GitHub.copilot",
                "ms-python.pylint"
            ]
        }
    }
}
```

Il faudra un `requirements.txt` à la racine du projet, contenant au minimum les lignes suivantes (+ les modules propres à votre code) :

```txt title="requirements.txt"
boto3==1.16.51
botocore==1.19.51
loguru==0.7.0
autopep8
```
