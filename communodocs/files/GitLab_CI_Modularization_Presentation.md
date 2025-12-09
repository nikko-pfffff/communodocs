---
marp: true
theme: default
class: invert

---
# Modularisation GitLab CI : Include, Reference & Extends
## Include, Reference & Extends
### Réduire la duplication de code et améliorer la maintenabilité

---

**Modularisation GitLab CI**
*Rendre vos Pipelines DRY avec Include, Reference & Extends*

- Durée : 10 minutes
- Focus : Techniques de modularisation pratiques
- Objectif : Réduire la duplication de code et améliorer la maintenabilité

---

## Le Problème - Fichiers CI Monolithiques
### Avant la Modularisation
```yaml
stages:
  - validate
  - deploy

terraform-validate-dev:
  stage: validate
  script:
    - terraform version
    - mkdir -p "$CI_PROJECT_DIR/.terraform.d/plugin-cache"
    - terraform init -input=false -backend-config=config/config.dev.s3.tfbackend         
    - terraform init -backend=false
    - terraform validate
  rules:
    - if: $CI_COMMIT_BRANCH =~ /^(develop|main)$/
    changes:
      - $DIRECTORY/**/*
  # ... repeated for staging, prod
```
---

**Problèmes :**
- Duplication de code
- Difficile à maintenir

---

## La Solution - Architecture Modulaire
### Après Modularisation
```yaml
# .gitlab-ci.yml (Clean & Simple)
include:
  - local: 'templates/rules-check.yml'
  - local: 'templates/terraform.yml'

terraform-validate:
  extends: .terraform-validate
  variables:
    DIRECTORY: "infrastructure"
  rules:
  - !reference [.rules-check, rules]                                                     
```
---

**Avantages :**
- DRY (Don't Repeat Yourself)
- Composants réutilisables
- Meilleure organisation

---

## Méthode 1 - Include
### Objectif : Importer des Fichiers YAML Externes

**Options de Syntaxe :**
```yaml
include:
  # Local files
  - local: 'templates/common.yml'
  
  # Remote files
  - remote: 'https://example.com/ci-templates/base.yml'                                  
  
  # Project files
  - project: 'group/ci-templates'
    file: '/templates/docker.yml'
  
  # Template files
  - template: Security/SAST.gitlab-ci.yml
```
---

```yaml
Les includes intègrent le contenu des fichiers YAML en paramètre
(les variables et directives globales en les concaténant ainsi que les hidden jobs)      
```
<br>
<br>

**Cas d'Usage :**
- Partager des templates entre projets
- Organiser de grandes configurations CI
- Créer des définitions de jobs réutilisables

---

## Méthode 2 - Reference (!reference)
### Objectif : Réutiliser des Sections Spécifiques

```yaml
.base-rules:
  - if: $CI_PIPELINE_SOURCE == "merge_request_event"
    when: never
  - if: $RUN_JOB && ($RUN_JOB == $CI_JOB_STAGE)
    when: always

.change-rules:
  - if: $CI_COMMIT_BRANCH =~ /^(develop|main)$/ && $RUN_JOB == null                      
    changes:
      - $DIRECTORY/**/*

  rules:
    - !reference [.base-rules]
    - !reference [.change-rules]
```

---
```yaml
Les références permettent de composer des listes et d’importer des parties de contenu    
sans le problème de surcharge et précédence.
```
<br>
<br>

**Fonctionnalités Clés :**
- Copie le contenu exact
- Ajout d'élément à une liste
- Réutilisation granulaire

---

## Méthode 3 - Extends
### Objectif : Hériter et Surcharger les Configurations de Jobs

```yaml
.base-terraform:
  before_script:
    - terraform version
    - mkdir -p "$CI_PROJECT_DIR/.terraform.d/plugin-cache"

.terraform-validate:
  extends: .base-terraform  # ← Inherits all properties
  script:
    - terraform init -backend=false
    - terraform validate

.terraform-plan:
  extends: .base-terraform
  script:
    - terraform plan -var-file=config/terraform.$ENV.tfvars                              
```
---
```yaml
Les extends permettent d’importer des blocs complets avec les balises. 

Ils permettent de concaténer les variables, token, secrets etc…  

Les before_script, script, rules seront écrasés selon la précédence                      
(parent, enfant, ordre d’écriture, déclaration job local)
```

<br>
<br>

**Règles d'Héritage :**
- L'enfant surcharge le parent
- Les tableaux sont remplacés (pas fusionnés)
- Les objets sont fusionnés

---

## Exemple Concret - Notre Structure de Templates
### Organisation du Projet

```
templates/
├── aws.yml           # AWS-specific jobs and authentication
├── datadog.yml       # Datadog-specific jobs and API keys
├── default.yml       # General vars and default configuration
├── docker.yml        # Docker-specific jobs and push repository
├── gitlab.yml        # GitLab-specific jobs and authentication
├── kubernetes.yml    # Kubernetes-specific jobs and authentication
├── terraform.yml     # Terraform-specific jobs
├── vault.yml         # Vault specific configuration to get secrets
└── README.md         # Documentation

```
---

**Avantages en Pratique :**
- Beaucoup de réduction de code
- Comportement cohérent entre environnements
- Facile à mettre à jour et maintenir

---

## Pattern Avancé - Combiner les Trois
### Le Trio de Puissance en Action

```yaml
include:
  - local: 'templates/default.yml'                                                       
  - local: 'templates/terraform.yml'
  - local: 'templates/rules-check.yml'

terraform-plan:
  extends: .terraform-plan
  variables:
    DIRECTORY: "infrastructure"
  rules:
    - !reference [.rules-check, rules]

terraform-apply:
  extends: .terraform-apply
  variables:
    DIRECTORY: "infrastructure"
  rules:
    - !reference [.rules-check, rules]
```
---

```yaml
.base-rules:
  - if: $CI_PIPELINE_SOURCE == "merge_request_event"
    when: never
  - if: $RUN_JOB && (($RUN_JOB != $CI_JOB_STAGE) && ($TRIGGER != $RUN_JOB) && ($RUN_JOB !~ $TRIGGER_LIST))
    when: never
  - if: $RUN_JOB && ($RUN_JOB =~ $TRIGGER_LIST)
    when: always
  - if: $RUN_JOB && ($RUN_JOB == $TRIGGER)
    when: always
  - if: $RUN_JOB && ($RUN_JOB == $CI_JOB_STAGE)
    when: always

.change-rules:
  - if: $CI_COMMIT_BRANCH =~ /^(develop|main)$/ && $RUN_JOB == null
    changes:
      - $DIRECTORY/**/*

.rules-check:
  before_script:
    - echo "RUN_JOB - $RUN_JOB"
    - echo "TRIGGER - $TRIGGER"
    - echo "TRIGGER_LIST - $TRIGGER_LIST"

  rules:
    - !reference [.base-rules]
    - !reference [.change-rules]
```


---

## Bonnes Pratiques et Conseils


**✅ À FAIRE :**
- Utilisez des hidden job avec des nom parlants (`.base-terraform`)
- Gardez les templates concentrées sur 1 seul sujet
- Documentez vos templates
- Utilisez des variables pour factoriser
- Testez les changements de templates

---

**❌ À NE PAS FAIRE :**
- Ne créez pas des chaînes d’héritage complexes
- Ne mélangez pas les sujets dans vos templates
- Ne hardcodez pas de variables d’environnement

---


# Questions ?


