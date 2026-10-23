# Manipulation du DOM avec jQuery

Ce cours présente la manipulation du **DOM (Document Object Model)** à l'aide de la bibliothèque **jQuery**, son fonctionnement interne basé sur l'enveloppe (*wrapper*) et le chaînage, ainsi que les parallèles avec le **JavaScript moderne (Vanilla ES6+)**.

---

## 1. Installation et Intégration

Il existe plusieurs façons d'intégrer jQuery dans un projet selon l'environnement de développement (projet simple, build tool ou framework full-stack comme Symfony).

### 1.1 Avec un gestionnaire de paquets (npm, yarn, pnpm)

Dans un environnement de développement moderne (utilisant un bundler comme Vite, Webpack ou Webpack Encore dans Symfony), vous pouvez installer jQuery directement depuis le registre **npm** :

```bash
# Avec npm
npm install jquery

# Avec yarn
yarn add jquery

# Avec pnpm
pnpm add jquery
```

#### Importation dans votre code JavaScript :
```javascript
// Import standard ES Module (ES6+)
import $ from 'jquery';

// Utilisation
$(function() {
    console.log("jQuery chargé via npm/bundler !");
});
```

> [!TIP] Rendre jQuery disponible globalement (`window.$`)
> Par défaut, un import modulaire n'expose pas `$` à la portée globale `window`. Si vous utilisez d'anciens plugins jQuery ou des scripts inline dans vos templates Twig, exposez-le manuellement dans votre point d'entrée JS :
> ```javascript
> import $ from 'jquery';
> window.$ = window.jQuery = $;
> ```

---

### 1.2 Intégration dans un projet Symfony

Puisque vous travaillez avec Symfony en cours parallèle, voici comment intégrer jQuery selon la méthode choisie côté backend :

#### Option A : Avec Symfony AssetMapper (Symfony 6.3+ / 7 sans Node.js)
AssetMapper permet de gérer des paquets npm sans avoir besoin d'installer Node.js ni Webpack :
```bash
php bin/console importmap:require jquery
```
Puis dans `assets/app.js` :
```javascript
import $ from 'jquery';
```

#### Option B : Avec Symfony Webpack Encore
1. Installez le paquet : `npm install jquery --save-dev`
2. Si vous avez besoin de jQuery globalement dans vos vues Twig, activez-le dans `webpack.config.js` :
   ```javascript
   Encore.autoProvidejQuery();
   ```

---

### 1.3 Via un CDN (Content Delivery Network)

Idéal pour les maquettes, les TP rapides ou les projets statiques sans étape de compilation (*build*). Il suffit d'ajouter la balise `<script>` dans le `<head>` ou juste avant la fermeture de `</body>` :

```html
<!-- CDN officiel jQuery (version minifiée de production) -->
<script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>

<!-- Alternative CDN fiable (cdnjs / jsDelivr) -->
<!-- <script src="https://cdn.jsdelivr.net/npm/jquery@3.7.1/dist/jquery.min.js"></script> -->
```

- **Avantages :** Zéro configuration, mise en cache possible par le navigateur si le visiteur a déjà visité un site utilisant le même CDN.
- **Inconvénients :** Nécessite une connexion Internet active lors du développement, et crée une dépendance envers un serveur tiers.

---

### 1.4 Téléchargement en local (Autonome)

Vous pouvez télécharger le fichier `jquery-3.7.1.min.js` depuis le site officiel [jquery.com/download](https://jquery.com/download/) et le placer dans votre dossier `public/js/` :

```html
<script src="/js/jquery-3.7.1.min.js"></script>
```
Cette méthode garantit que votre application fonctionne à 100% hors-ligne.

---

## 2. Le Cœur de jQuery : La fonction `$()` et l'Objet jQuery

Le symbole `$` est un alias vers la fonction globale `jQuery`.

### 2.1 Que produit `$(selecteur)` ?
Lorsque vous écrivez `$('#titre')` ou `$('.film')`, jQuery ne retourne **pas** un élément HTML natif, mais un **Objet jQuery** (une collection / tableau-like enveloppant les éléments DOM correspondants) :

```javascript
// Sélection de tous les éléments .film
const $films = $('.film');

console.log($films); 
// Résultat : un objet jQuery [div.film, div.film, ...] doté de toutes les méthodes jQuery
```

> [!TIP] Convention de nommage
> Pour distinguer facilement une variable contenant un élément DOM natif d'un objet jQuery, on préfixe traditionnellement ces dernières par un `$` :
> ```javascript
> const btnEl = document.querySelector('#btn'); // Élément natif
> const $btn = $('#btn');                       // Objet jQuery
> ```

### 2.2 Passerelle entre DOM natif et jQuery

- **De DOM natif vers jQuery :** `$(elementDOM)` (on "enveloppe" l'élément).
- **De jQuery vers DOM natif :** `$element[0]` ou `$element.get(0)` (on extrait le premier élément sous-jacent).

```javascript
const elNatif = document.getElementById('mon-id');
const $elJquery = $(elNatif); // Converti en objet jQuery

const boutonNatif = $elJquery[0]; // Extrait l'élément natif
```

### 2.3 Attendre le chargement du DOM : `ready()`

Pour s'assurer que le script s'exécute une fois le document HTML totalement parsé :

```javascript
// Forme classique
$(document).ready(function() {
    console.log("Le DOM est prêt !");
});

// Forme raccourcie recommandée
$(function() {
    console.log("Le DOM est prêt !");
});
```

---

## 3. Sélection et Traversée du DOM (*DOM Traversal*)

### 3.1 Sélecteurs CSS standards
jQuery supporte l'ensemble des sélecteurs CSS (identifiants, classes, attributs, combinateurs) :
```javascript
$('#page-title');             // Par ID
$('.btn-danger');             // Par classe
$('article > p.intro');       // Enfants directs
$('input[type="email"]');     // Par attribut
$('li:first');                // Premier élément
$('li:last');                 // Dernier élément
$('tr:even');                 // Éléments pairs (index 0, 2, 4...)
```

### 3.2 Méthodes de parcours de l'arbre DOM

Une grande force de jQuery est de naviguer intuitivement d'un élément à un autre :

| Direction | Méthodes jQuery | Description |
| :--- | :--- | :--- |
| **Vers le haut (Parents)** | `.parent()` | Parent direct |
| | `.parents('.conteneur')` | Tous les ancêtres (filtrables) |
| | `.closest('.carte')` | Le premier ancêtre correspondant (très utilisé !) |
| **Vers le bas (Enfants)** | `.children()` | Enfants directs |
| | `.find('.btn')` | Tous les descendants correspondant au sélecteur |
| **Latéral (Frères / Sœurs)** | `.next()` / `.prev()` | Élément suivant / précédent |
| | `.siblings()` | Tous les éléments frères |
| **Filtrage** | `.eq(index)` | Sélectionne l'élément à l'index donné (ex: `.eq(2)`) |
| | `.filter('.actif')` | Garde uniquement ceux qui ont la classe |
| | `.not('.exclu')` | Exclut ceux qui ont la classe |

```javascript
// Exemple : trouver le bouton de suppression à l'intérieur de la carte du film cliqué
$('.carte-film').find('.btn-supprimer').css('color', 'red');
```

---

## 4. Manipulation du Contenu, des Attributs et des Styles

### 4.1 Règle "Getter / Setter"
La majorité des méthodes jQuery fonctionnent selon une règle simple :
- **Sans argument :** Elle lit la valeur (Getter).
- **Avec argument(s) :** Elle modifie la valeur (Setter) et s'applique à **tous** les éléments de la sélection !

```javascript
// 1. Contenu textuel
const texte = $('#titre').text();          // GET
$('#titre').text('Nouveau titre');         // SET

// 2. Contenu HTML
const html = $('.description').html();     // GET
$('.description').html('<strong>OK</strong>'); // SET

// 3. Valeur de formulaire (input, textarea, select)
const email = $('#user-email').val();      // GET
$('#user-email').val('alice@epsi.fr');     // SET
```

### 4.2 Manipulation des classes CSS

```javascript
$element.addClass('actif surbrillance'); // Ajoute une ou plusieurs classes
$element.removeClass('chargement');     // Supprime une classe
$element.toggleClass('ouvert');         // Ajoute si absente, supprime si présente
const estActif = $element.hasClass('actif'); // Renvoie true ou false
```

### 4.3 Attributs et Propriétés : `.attr()` vs `.prop()`

> [!WARNING] Piège classique : `attr()` vs `prop()`
> - `.attr()` manipule les **attributs HTML** textuels (tels qu'écrits dans le code source).
> - `.prop()` manipule les **propriétés JavaScript du DOM** en direct (notamment les états booléens : `disabled`, `checked`, `selected`).

```javascript
// Pour modifier un état de formulaire, toujours privilégier .prop() :
$('#btn-valider').prop('disabled', true);  // Désactiver
$('#case-rgpd').prop('checked', true);     // Cocher

// Pour les attributs standards (src, href, title, etc.) :
$('img#logo').attr('src', 'images/nouveau-logo.png');
$('a#lien').attr('target', '_blank');

// Lecture d'un attribut data-* HTML5 :
const filmId = $('#carte-1').data('id'); // Lit data-id="1"
```

### 4.4 Styles en ligne avec `.css()`

```javascript
// Modifier une seule propriété :
$('#alerte').css('background-color', '#fee2e2');

// Modifier plusieurs propriétés d'un coup (via un objet JS) :
$('#alerte').css({
    'border': '1px solid #ef4444',
    'color': '#991b1b',
    'padding': '12px'
});
```

---

## 5. Création, Insertion et Suppression d'éléments

### 5.1 Créer un élément HTML
En passant une chaîne HTML complète à `$()`, jQuery crée l'élément en mémoire :
```javascript
const $nouveauFilm = $('<li class="item-film">Inception (2010)</li>');

// Ou avec un objet d'attributs :
const $btn = $('<button>', {
    text: 'Supprimer',
    class: 'btn-danger',
    click: function() { $(this).parent().remove(); }
});
```

### 5.2 Insérer des éléments dans le DOM

```javascript
const $liste = $('#ma-liste');

// À l'intérieur :
$liste.append($nouveauFilm);    // Ajoute à la fin (dernier enfant)
$liste.prepend($nouveauFilm);   // Ajoute au début (premier enfant)

// À l'extérieur (frères) :
$liste.after('<p>Fin de liste</p>');   // Juste après la liste
$liste.before('<h3>Catalogue</h3>');   // Juste avant la liste
```

### 5.3 Supprimer ou Vider des éléments

```javascript
$('#ma-liste').empty();   // Vide tous les enfants de la liste, mais conserve le <ul>
$('#element-inutile').remove(); // Supprime complètement l'élément du DOM
```

---

## 6. Le Chaînage (*Method Chaining*)

Puisque les méthodes jQuery "Setter" renvoient l'objet jQuery d'origine (`return this;`), on peut enchaîner plusieurs opérations à la suite sans répéter le sélecteur :

```javascript
$('#notification')
    .text('Opération réussie !')
    .removeClass('erreur')
    .addClass('succes')
    .css('display', 'block')
    .fadeOut(3000);
```

---

## 7. Gestion des Événements & Délégation

### 7.1 Écoute simple avec `.on()`
```javascript
$('#btn-envoyer').on('click', function(event) {
    event.preventDefault(); // Annule l'action par défaut (ex: soumission formulaire)
    console.log("Bouton cliqué !");
});

// Écouteurs de raccourcis (équivalents) :
$('#input-recherche').on('input', function() { ... });
$('#form-film').on('submit', function(e) { ... });
```

> [!NOTE] Le mot-clé `this` dans un écouteur jQuery
> Dans une fonction de callback classique `function() {}`, `this` représente l'**élément DOM natif** qui a déclenché l'événement. Pour lui appliquer des méthodes jQuery, on l'enveloppe : `$(this)`.

### 7.2 La Délégation d'événements (Essentielle avec les API / AJAX)

Lorsqu'on charge des données depuis une API Symfony, les nouveaux éléments HTML sont injectés **après** le chargement initial de la page. Un écouteur classique `$('.btn-delete').on('click', ...)` ne fonctionnerait pas sur eux !

**La solution : écouter sur le parent existant grâce au paramètre sélecteur de `.on()` :**

```javascript
// Syntaxe : $(parentExistant).on(evenement, selecteurCible, callback)
$('#liste-films').on('click', '.btn-delete', function() {
    // $(this) est le .btn-delete cliqué, même s'il vient d'être créé !
    const filmId = $(this).data('id');
    $(this).closest('li').remove();
    console.log(`Film #${filmId} retiré du DOM.`);
});
```

---

## 8. Effets et Animations intégrées

jQuery intègre des méthodes d'animation visuelle fluides :

```javascript
// Affichage / Masquage instantané ou avec durée (en ms)
$element.hide(400);
$element.show(400);
$element.toggle(); // Alterne entre affiché et masqué

// Fondu (Fade)
$element.fadeIn(300);
$element.fadeOut(300);
$element.fadeToggle();

// Déroulement (Slide) - Très utilisé pour les accordéons et menus
$element.slideDown();
$element.slideUp();
$element.slideToggle();
```

---

## 9. Synthèse Comparative : jQuery vs Vanilla JS (ES6+)

| Opération | jQuery | Vanilla JS Moderne |
| :--- | :--- | :--- |
| **Sélection unique** | `$('#id')` | `document.querySelector('#id')` |
| **Sélection multiple** | `$('.item')` | `document.querySelectorAll('.item')` |
| **Attente du DOM** | `$(function() { ... })` | `document.addEventListener('DOMContentLoaded', ...)` |
| **Texte / HTML** | `$el.text('...')` / `$el.html('...')` | `el.textContent = '...'` / `el.innerHTML = '...'` |
| **Valeur d'input** | `$input.val()` | `input.value` |
| **Ajout/Retrait classe** | `$el.addClass('c')` / `.removeClass('c')` | `el.classList.add('c')` / `.remove('c')` |
| **Basculer une classe** | `$el.toggleClass('active')` | `el.classList.toggle('active')` |
| **Propriété disabled** | `$btn.prop('disabled', true)` | `btn.disabled = true;` |
| **Attributs** | `$el.attr('href', '...')` | `el.setAttribute('href', '...')` |
| **Style CSS** | `$el.css('color', 'blue')` | `el.style.color = 'blue'` |
| **Créer un élément** | `$('<li>')` | `document.createElement('li')` |
| **Ajouter un enfant** | `$parent.append($enfant)` | `parent.append(enfant)` |
| **Supprimer un élément** | `$el.remove()` | `el.remove()` |
| **Événement simple** | `$el.on('click', fn)` | `el.addEventListener('click', fn)` |
| **Délégation d'événements** | `$parent.on('click', '.cible', fn)` | `parent.addEventListener('click', e => { if (e.target.closest('.cible')) ... })` |
