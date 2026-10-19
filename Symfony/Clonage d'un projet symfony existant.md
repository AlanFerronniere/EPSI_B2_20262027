
Quand vous clonez un projet existant, vous devez suivre quelques étapes pour le configurer correctement sur votre machine locale. Voici les étapes à suivre :

1. **Cloner le dépôt** : Utilisez la commande `git clone <url-du-depot>` pour cloner le projet sur votre machine.
2. **Installer les dépendances** : Naviguez dans le répertoire du projet cloné et exécutez `composer install` pour installer toutes les dépendances PHP nécessaires.
3. **Configurer les variables d'environnement** : Copiez le fichier `.env.example` en `.env` et modifiez les variables d'environnement selon vos besoins (base de données, mailer, etc.).
4. **Générer la clé de l'application** : Exécutez `symfony console secret:generate` pour générer une clé secrète pour l'application.
5. **Mettre à jour la base de données** : Si le projet utilise une base de données, exécutez les migrations avec `symfony console doctrine:migrations:migrate` pour créer les tables nécessaires.
6. **Charger les fixtures (si nécessaire)** : Si le projet utilise des fixtures pour peupler la base de données, exécutez `symfony console doctrine:fixtures:load`.
