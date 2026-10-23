# 1. Les bases de la syntaxe JavaScript

Ce cours couvre les fondations du langage JavaScript moderne (norme ECMAScript / ES6+), son modèle de typage, ses structures de contrôle et les mécanismes clés de son moteur d'exécution (portée, hoisting, closures).

---

## 1. Introduction et Environnement d'exécution

JavaScript est un langage :
- **Interprété / JIT-compilé** (moteurs V8, SpiderMonkey, JavaScriptCore)
- **À typage dynamique et faible**
- **Monothreadé** avec une boucle d'événements (*Event Loop*)
- **Multi-paradigme** (impératif, fonctionnel, orienté objet par prototype)

Pour tester du code en direct :
- Dans le navigateur : Ouvrez les Outils de développement (`F12` ou `Ctrl+Shift+I` > Onglet **Console**).
- Dans le terminal : `node` pour ouvrir le REPL, ou `node script.js`.

> [!TIP] Le mode strict (`"use strict";`)
> Il est fortement recommandé d'activer le mode strict en tête de fichier ou de fonction (activé par défaut dans les modules ES6). Il empêche les erreurs silencieuses (comme l'assignation accidentelle à une variable non déclarée).

---

## 2. Les Variables : `var`, `let` et `const`

Historiquement, JavaScript ne disposait que du mot-clé `var`. Depuis ES6 (2015), `let` et `const` sont les standards absolus.

### 2.1 Comparatif des déclarations

| Critère | `var` (Legacy) | `let` (Moderne) | `const` (Moderne) |
| :--- | :--- | :--- | :--- |
| **Portée (*Scope*)** | Fonction ou Globale | **Bloc `{ ... }`** | **Bloc `{ ... }`** |
| **Réassignation** | Oui | Oui | **Non (valeur fixe)** |
| **Redéclaration** | Oui (source de bugs !) | Non (erreur) | Non (erreur) |
| **Hoisting** | Initialisé à `undefined` | Non initialisé (TDZ) | Non initialisé (TDZ) |

### 2.2 Exemples de portée de bloc

```javascript
// Avec let / const : portée limitée aux accolades {}
if (true) {
    let message = "Bonjour";
    const pi = 3.14159;
}
// console.log(message); // ReferenceError: message is not defined

// Avec var : fuite en dehors du bloc !
if (true) {
    var fuite = "Je m'échappe du if !";
}
console.log(fuite); // Affiche "Je m'échappe du if !"
```

### 2.3 `const` et la mutabilité des objets

`const` empêche la **réassignation** de la variable (son adresse mémoire), mais n'empêche pas la **mutation** du contenu d'un tableau ou d'un objet :

```javascript
const user = { nom: "Alice", role: "Dev" };
user.nom = "Bob"; // VALIDE : on modifie une propriété
// user = { nom: "Charlie" }; // ERREUR : TypeError (réassignation interdite)

const notes = [12, 15];
notes.push(18); // VALIDE
// notes = [10, 20]; // ERREUR
```

---

## 3. Le Mécanisme de Hoisting (Remontée)

Le moteur JavaScript analyse le code en deux phases :
1. **Phase de création / compilation :** Le moteur alloue la mémoire pour les variables et fonctions.
2. **Phase d'exécution :** Le code est exécuté ligne par ligne.

### Hoisting avec `var`
Les déclarations avec `var` sont remontées au sommet de leur contexte et initialisées à `undefined` :

```javascript
console.log(prenom); // Affiche 'undefined' (pas d'erreur !)
var prenom = "Lucas";
console.log(prenom); // Affiche 'Lucas'
```

### Hoisting avec `let` / `const` et la TDZ (Temporal Dead Zone)
Les déclarations avec `let` et `const` sont bien reconnues par le moteur, mais restent inaccessibles jusqu'à leur ligne de déclaration :

```javascript
console.log(age); // ReferenceError: Cannot access 'age' before initialization
let age = 22;     // Fin de la Temporal Dead Zone
```

---

## 4. Les Types de Données

JavaScript distingue les **types primitifs** (immuables, passés par valeur) et les **objets** (mutables, passés par référence).

### 4.1 Les 7 types primitifs
```javascript
// 1. String
const titre = "Inception";

// 2. Number (entiers et flottants sont tous des 64-bit float IEEE 754)
const annee = 2010;
const prix = 12.99;

// 3. Boolean
const disponible = true;

// 4. Undefined (variable déclarée sans valeur attribuée)
let note; // typeof note === "undefined"

// 5. Null (absence intentionnelle de valeur)
const realisateur = null; // typeof null === "object" (anomalie historique de JS !)

// 6. BigInt (entiers de taille arbitraire)
const grandNombre = 9007199254740991n;

// 7. Symbol (identifiant unique immuable)
const cleUnique = Symbol("id");
```

### 4.2 Les types référence (Objets)
Tout ce qui n'est pas primitif hérite de `Object` :
```javascript
const film = { titre: "Dune", annee: 2021 }; // Objet littéral
const genres = ["Sci-Fi", "Aventure"];       // Tableau (Array)
const afficher = function() { return 42; };  // Fonction
```

> [!WARNING] Vérification de types avec `typeof`
> ```javascript
> typeof "texte";       // "string"
> typeof 42;            // "number"
> typeof true;          // "boolean"
> typeof undefined;     // "undefined"
> typeof null;          // "object"  <-- Piège classique !
> typeof [1, 2, 3];     // "object"  (Utiliser Array.isArray(arr) pour tester un tableau)
> typeof function(){};  // "function"
> ```

---

## 5. Les Opérateurs et Comparaisons

### 5.1 Égalité stricte (`===`) vs Égalité faible (`==`)

Toujours privilégier l'égalité stricte `===` qui compare la valeur **ET** le type sans conversion implicite (*coercion*).

```javascript
5 == "5";   // true  (Danger : JS convertit le string en number)
5 === "5";  // false (Recommandé : types différents)

null == undefined;   // true
null === undefined;  // false

0 == false;   // true
0 === false;  // false
```

### 5.2 Valeurs "Falsy" et "Truthy"
Dans une condition, JavaScript convertit automatiquement une valeur en booléen :

- **Falsy :** `false`, `0`, `-0`, `""` (chaîne vide), `null`, `undefined`, `NaN`.
- **Truthy :** Tout le reste ! (y compris `[]`, `{}` et `"0"`).

### 5.3 Opérateurs modernes indispensables (ES2020+)

```javascript
// 1. Nullish Coalescing (??) : Retourne la droite UNIQUEMENT si la gauche est null ou undefined
const noteUtilisateur = 0;
const affichage1 = noteUtilisateur || "Pas de note"; // "Pas de note" (Car 0 est falsy !)
const affichage2 = noteUtilisateur ?? "Pas de note"; // 0 (Car 0 n'est ni null ni undefined)

// 2. Chaînage optionnel (?.) : Évite l'erreur "Cannot read properties of undefined"
const commande = { client: { nom: "Marc" } };
console.log(commande.client?.nom);     // "Marc"
console.log(commande.livraison?.ville); // undefined (ne plante pas !)

// 3. Opérateur ternaire
const statut = annee >= 2000 ? "Récent" : "Classique";
```

---

## 6. Structures Conditionnelles

### 6.1 `if / else if / else`
```javascript
const note = 14;

if (note >= 16) {
    console.log("Très bien");
} else if (note >= 10) {
    console.log("Validé");
} else {
    console.log("Rattrapage");
}
```

### 6.2 `switch`
```javascript
const role = "ROLE_ADMIN";

switch (role) {
    case "ROLE_ADMIN":
        console.log("Accès complet");
        break;
    case "ROLE_USER":
        console.log("Accès limité");
        break;
    default:
        console.log("Accès refusé");
}
```

---

## 7. Itérations et Boucles

### 7.1 Boucles classiques
```javascript
// for classique (quand on a besoin de l'index)
for (let i = 0; i < 5; i++) {
    console.log(i);
}

// while
let compteur = 0;
while (compteur < 3) {
    compteur++;
}
```

### 7.2 `for...of` (Pour les valeurs des tableaux et itérables)
```javascript
const films = ["Matrix", "Avatar", "Tenet"];

for (const film of films) {
    console.log(film); // "Matrix", "Avatar", "Tenet"
}
```

### 7.3 `for...in` (Pour énumérer les propriétés d'un objet)
```javascript
const film = { titre: "Gladiator", annee: 2000, realisateur: "Ridley Scott" };

for (const cle in film) {
    console.log(`${cle} : ${film[cle]}`);
}
```

> [!CAUTION] Ne pas utiliser `for...in` sur les tableaux
> `for...in` itère sur les clés énumérables (qui sont des chaînes de caractères), dans un ordre non garanti. Pour un tableau, utilisez `for...of` ou les méthodes fonctionnelles (`forEach`, `map`).

---

## 8. Les Fonctions

### 8.1 Déclaration vs Expression de fonction

```javascript
// Déclaration classique (Bénéficie du hoisting : appelable avant sa ligne)
function saluer(nom) {
    return `Bonjour ${nom}`;
}

// Expression de fonction (Stockée dans une variable, pas de hoisting)
const additionner = function(a, b) {
    return a + b;
};
```

### 8.2 Fonctions fléchées (*Arrow Functions* ES6)
Syntaxe concise, idéale pour les callbacks. **Attention : elles n'ont pas leur propre `this` !**

```javascript
// Syntaxe complète
const multiplier = (a, b) => {
    return a * b;
};

// Retour implicite sur une seule expression
const doubler = x => x * 2;
```

### 8.3 Paramètres par défaut et Rest Parameter (`...`)

```javascript
// Paramètre avec valeur par défaut
function creerFilm(titre, annee = 2026) {
    return { titre, annee };
}

// Rest parameter (regroupe les arguments restants dans un vrai tableau)
function faireSomme(...nombres) {
    return nombres.reduce((acc, curr) => acc + curr, 0);
}
console.log(faireSomme(2, 4, 6, 8)); // 20
```

---

## 9. Portée Lexicale et Closures (Fermetures)

> [!IMPORTANT] Définition d'une Closure
> Une **closure** est une fonction qui se "souvient" de son environnement lexical (les variables déclarées autour d'elle au moment de sa création), même après que la fonction parente a terminé son exécution.

### 9.1 Exemple fondamental : La fabrique de fonctions
```javascript
function creerCompteur(valeurInitiale = 0) {
    let count = valeurInitiale; // Variable privée protégée

    return {
        incrementer() {
            count++;
            return count;
        },
        obtenirValeur() {
            return count;
        }
    };
}

const compteur1 = creerCompteur(10);
console.log(compteur1.incrementer());   // 11
console.log(compteur1.incrementer());   // 12
console.log(compteur1.obtenirValeur()); // 12
// console.log(compteur1.count);        // undefined (impossible d'y accéder directement !)
```

### 9.2 Le piège historique du `var` dans une boucle asynchrone

Ce problème illustre parfaitement la différence entre la portée de fonction de `var` et la portée de bloc de `let` avec les closures :

```javascript
// Problème avec var :
for (var i = 0; i < 3; i++) {
    setTimeout(() => {
        console.log("var i:", i);
    }, 100);
}
// Résultat dans la console : 3, 3, 3 (car var n'a qu'une seule instance partagée)

// Solution avec let :
for (let j = 0; j < 3; j++) {
    setTimeout(() => {
        console.log("let j:", j);
    }, 100);
}
// Résultat dans la console : 0, 1, 2 (chaque itération crée une nouvelle variable j capturée par la closure)
```

---


### Prochaine étape :
➡️ **[[Objets, this et Héritage prototypal]]**
