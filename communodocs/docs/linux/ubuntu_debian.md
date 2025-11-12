### Résolution de l'erreur NO_PUBEY lors d'un apt update

Pour les Ubunut >= 20.04 et Debian >= 11

Si l'erreur est de type :

```bash
The following signatures couldn't be verified because the public key is not available: NO_PUBKEY [KEY_ID]
```

On récupère la clé:
```bash
sudo gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys [KEY_ID]
```

Il faut ensuite déposer cette clé dans /etc/apt/trusted.gpg.d/ ou dans le répertoire spécifié dans le fichier source apt

```
Types: deb
URIs: https://repo.truc.com/debian
Suites: stable
Components: main
Signed-By: /usr/share/keyrings/[KEY_FILE].gpg
```

Au cas où, on peut faire une sauvegarde de l'ancienne clé, mais en théorie elle n'est plus d'actualité.

On dépose la clé au bon endroit
```bash
sudo gpg --export EDA3E22630349F1C > /usr/share/keyrings/[KEY_FILE].gpg
```

On peut sensuite relancer
``` bash
sudo apt update && sudo apt upgrade
```