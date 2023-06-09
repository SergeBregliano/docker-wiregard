# Docker - Wireguard

Docker Compose permettant de lancer un VPN [Wireguard](https://www.wireguard.com/) en mode serveur.
Cette stack utilise l'[image mise à disposition par linuxserver.io](https://hub.docker.com/r/linuxserver/wireguard).

## Configuration serveur

Renommer le fichier ``sample.env`` en ``.env``.

Configurer le fichier ``.env`` avec les valeurs souhaitées.

Préciser notamment :

- VIRTUAL_HOST : nom de domaine de Wireguard.
- DATA_PATH : chemin pour le stockage des données.
- PEERS : noms des fichiers de configuration client en majuscule, séparés par une virgule.

Afin d'afficher facilement les fichiers de configuration, il est utile de paramétrer l'utilisateur avec lequel Wireguard fonctionnera. Pour se faire, il faut renseigner les champs :

- PUID
- PGID

Ces deux éléments correspondent au ``uid`` et ``gid`` qui sont affichés lors de de la commande :

```shell
id
```



## Configuration client

Installer le [client Wireguard](https://www.wireguard.com/install/).

Ajouter une entrée PEERS dans le fichier d'environnement ``.env`` sur le serveur. Chaque entrée PEERS correspond à un appareil (téléphone, ordinateur, etc.).
**Attention** : les entrées PEERS ne peuvent contenir que des caractères alphanumériques.

Par exemple, pour un simple téléphone, l'entrée ressemblerait à ça :

```shell
PEERS=TEL
```

Pour un téléphone, un PC et un Mac, l'entrée ressemblerait à :

```shell
PEERS=TEL,PC,MAC
```

Relancer le containter pour prendre en compte la nouvelle configuration :

```sh
Docker compose down
Docker compose up -d
```

Dans le dossier ``{DATA_PATH}/config/peer_{NOMDUPEER}/`` récupérer le fichier ``peer_{NOMDUPEER}.conf`` et l'importer dans le client Wireguard.

## Limitations

**Attention**, la version de **l'image linuxserver/wireguard n'est pas fixée** pour pemettre de prendre en comptes les évolutions lors des mises à jours.
