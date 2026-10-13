# Environnement de travail

## WSL

Sous windows, on peut faire avec WSL
Ici je vais créer une instance de debian nommée B220262027

```sh
wsl --update
wsl --install Debian --name B220262027
#pour le lancer si vous l'avez déjà
wsl -d B220262027
```

Ensuite vous spécifiez un nom d'utilisateur et un mot de passe DONT VOUS VOUS SOUVENNEZ

## Installation de PHP et mariaDB

```
sudo apt update
sudo apt upgrade
sudo apt install curl composer php-xml php-intl php-gd php-curl php-fpm
sudo apt install mariadb-server php-mysql

#on configure git
git config --global user.email "ferronniere@hotmail.com"
git config --global user.name "Alan Ferronnière"
```

## Installation de Symfony CLI

```sh
curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | sudo -E bash
```
puis
```sh
sudo apt install symfony-cli
```

## Pour créer une BDD quand on en aura besoin
```
sudo /etc/init.d/mariadb start
sudo mysql
CREATE DATABASE chatonsbdd;
GRANT ALL ON chatonsbdd.* TO 'chatonsbdd'@'localhost' IDENTIFIED BY 'epsi';
FLUSH PRIVILEGES;
EXIT;
# pour vérifier : sudo mysql -uchatonsbdd -pepsi chatonsbdd
```

