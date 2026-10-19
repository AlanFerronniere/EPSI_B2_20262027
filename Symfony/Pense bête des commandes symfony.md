# Création d’un projet web

Lancer la commande `symfony new --webapp my_project`

# Création d'une API

Lancer la commande `symfony new my_api`

# Lancement du serveur de debug

Symfony fourni un serveur intégré pour travailler. Pour le lancer, lancez la commande

`symfony server:start`

 à partir du dossier de votre projet.

Le serveur se lance par défaut sur [http://127.0.0.1:8000](http://127.0.0.1:8000)

Si votre app tourne dans wsl, l’url sera [http://wsl.localhost:8000](http://wsl.localhost:8000)
# Commandes symfony

Symfony est installé en dev avec un bundle « Maker » qui permet de gagner du temps en générant du code. Les commandes suivantes sont celles qui ont servi dans le cours :

## Créer un controller

```sh
symfony console make:controller
```
## Créer une entité

```sh
symfony console make:entity
```
## Créer une migration

```sh
symfony console make:migration
```

## Appliquer les migrations

```sh
symfony console doctrine:migrations:migrate
```
## Créer un formulaire type

```sh
symfony console make:form
```

