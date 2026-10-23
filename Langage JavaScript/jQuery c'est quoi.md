# jQuery c'est quoi ?

## Ressources
- Site jQuery : https://jquery.com/
- jQuery UI : https://jqueryui.com/

## Qu'est-ce que jQuery ?
jQuery est une bibliothèque JavaScript rapide, petite et riche en fonctionnalités. Elle simplifie des tâches courantes en JavaScript, telles que la manipulation du DOM, la gestion des événements, les animations et les requêtes AJAX, avec une syntaxe plus concise et facile à utiliser.
jQuery c'est anciennement très populaire, mais avec l'évolution des navigateurs et l'arrivée de frameworks modernes comme React, Angular et Vue.js, son utilisation a diminué. Cependant, il reste encore largement utilisé dans de nombreux projets existants. C'est notamment très utilisé dans WordPress, qui est un CMS très populaire.

## Pourquoi utiliser jQuery ?
- **Facilité d'utilisation** : jQuery simplifie la syntaxe JavaScript, ce qui permet aux développeurs de réaliser des tâches complexes avec moins de code.
- **Compatibilité entre navigateurs** : jQuery gère les différences entre les navigateurs, ce qui permet aux développeurs de créer des applications web qui fonctionnent de manière cohérente sur différents navigateurs.
- **Large communauté et support** : jQuery dispose d'une vaste communauté de développeurs, ce qui signifie qu'il existe de nombreuses ressources, plugins et extensions disponibles pour étendre ses fonctionnalités.
- **Plugins et extensions** : jQuery dispose d'un large éventail de plugins qui permettent d'ajouter facilement des fonctionnalités supplémentaires à vos projets web.
- **Animations et effets** : jQuery offre des méthodes intégrées pour créer des animations et des effets visuels, ce qui permet d'améliorer l'expérience utilisateur sur les sites web.
- **Requêtes AJAX simplifiées** : jQuery facilite l'envoi de requêtes AJAX, ce qui permet de charger des données dynamiques sans avoir à recharger la page entière.
- **Documentation complète** : jQuery dispose d'une documentation complète et bien organisée, ce qui facilite l'apprentissage et la mise en œuvre de ses fonctionnalités.

## Exemple de code jQuery
Voici un exemple simple de code jQuery qui change le texte d'un élément HTML lorsqu'on clique
    sur un bouton :
    
```html
<!DOCTYPE html>
<html lang="fr">
<head><head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Exemple jQuery</title>
	<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
</head>
<body>
	<h1 id="titre">Bonjour, monde !</h1>
	<button id="changerTexte">Changer le texte</button>

	<script>
		$(document).ready(function() {
			$("#changerTexte").click(function() {
				$("#titre").text("Texte changé !");
			});
		});
	</script>
</body>
</html>
```

Ce code crée une page web avec un titre et un bouton. Lorsque l'utilisateur clique sur le bouton, le texte du titre change grâce à jQuery.
Notez la syntaxe pour sélectionner le bouton et gérer l'événement de clic, ainsi que la méthode `.text()` pour modifier le contenu de l'élément sélectionné.

