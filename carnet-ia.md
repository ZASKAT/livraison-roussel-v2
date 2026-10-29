# Questions de comparaison

## Partie A

### 1. Quelle est la différence entre les unités CSS em et rem ? Donne un exemple.

> Je ne sais pas ce que em fais et rem est capable de s'adapter à la résolution de la page.

#### ChatGPT

La différence entre `em` et `rem` en CSS concerne la référence utilisée pour calculer la taille.

- `em` : relatif à la taille de police de l'élément parent (ou, pour certaines propriétés, à la taille de police de l'élément lui-même).
- `rem` (root em) : relatif à la taille de police de l'élément racine `<html>`.

**Exemple concret**

```css
html {
  font-size: 16px;
}

.parent {
  font-size: 20px;
}

.enfant-em {
  font-size: 2em;
  margin: 1em;
}

.enfant-rem {
  font-size: 2rem;
  margin: 1rem;
}
```

Voici le résultat :

| **Propriété**   | **Calcul**                      | **Résultat** |
|-----------------|---------------------------------|--------------|
| font-size: 2em  | 2 × 20 px (parent)              | 40 px        |
| margin: 1em     | 1 × 40 px (police de l'élément) | 40 px        |
| font-size: 2rem | 2 × 16 px (racine)              | 32 px        |
| margin: 1rem    | 1 × 16 px (racine)              | 16 px        |

Attention : pour `em`, la référence dépend de la propriété CSS. Par exemple, `font-size: 2em` utilise la taille de police du parent, tandis que `margin: 1em` utilise la taille de police de l'élément courant.

**Quand utiliser chaque unité ?**

- `rem` : idéal pour définir les tailles de police, les espacements et les dimensions de manière cohérente dans tout le site. Si tu modifies la taille de police de `<html>`, toutes les valeurs en `rem` s'adaptent.
- `em` : pratique pour créer des composants dont les espacements s'adaptent à leur propre taille de texte, comme un bouton ou une icône.

**À retenir**

Imagine que la taille de référence de `<html>` est de 16px :

- 1rem = 16px, partout sur la page.
- 1em = généralement 16px si la taille de police de référence est de 16px, mais sa valeur peut varier selon le contexte.

Mon conseil : privilégie `rem` pour les espacements globaux et la typographie, et utilise `em` lorsque tu veux que les dimensions d'un composant évoluent avec son texte.

#### Claude

La différence tient à la valeur de référence de chaque unité.

**em** est relatif à la taille de police de l'élément lui-même. Pour la propriété `font-size`, il est relatif à la taille de police du parent. Comme les valeurs en `em` se multiplient d'un niveau à l'autre, elles peuvent s'accumuler de façon surprenante dans des éléments imbriqués.

**rem** (« root em ») est toujours relatif à la taille de police de l'élément racine (`<html>`). Sa référence ne change pas selon l'endroit où il est utilisé, ce qui le rend plus prévisible.

Par défaut, les navigateurs fixent la taille de police racine à 16px, donc 1rem = 16px, sauf si tu la modifies.

**Exemple**

```html
<div class="parent">
  <p class="enfant-em">Texte en em</p>
  <p class="enfant-rem">Texte en rem</p>
</div>
```

```css
html {
  font-size: 16px;
}

.parent {
  font-size: 20px;
}

.enfant-em {
  font-size: 1.5em;      /* 1.5 × 20px (parent) = 30px */
  margin-bottom: 1em;    /* 1 × 30px (sa propre taille) = 30px */
}

.enfant-rem {
  font-size: 1.5rem;     /* 1.5 × 16px (racine) = 24px */
  margin-bottom: 1rem;   /* 1 × 16px = 16px */
}
```

Le paragraphe en `em` dépend du parent (20px), alors que celui en `rem` ignore le parent et ne regarde que la racine (16px).

**Quand utiliser lequel ?**

- **rem** convient pour les tailles de police, les marges et les espacements globaux. Si l'utilisateur change la taille de police par défaut de son navigateur, tout s'adapte de façon cohérente, ce qui est bon pour l'accessibilité.
- **em** convient pour les composants dont les dimensions doivent suivre leur propre taille de texte, comme le padding d'un bouton. Si tu augmentes le `font-size` du bouton, son espacement interne grandit proportionnellement.

```css
.bouton {
  font-size: 1rem;
  padding: 0.5em 1em; /* s'adapte si on change le font-size du bouton */
}
```

#### Gemini

La principale différence entre les unités CSS `em` et `rem` réside dans l'élément de référence utilisé pour calculer leur taille.

**1. Définitions**

- `em` **(Relatif au parent) :** Sa valeur est relative à la taille de police (`font-size`) de l'**élément parent** direct (ou de l'élément lui-même s'il est utilisé sur la propriété `font-size`).
  - *Particularité :* Les `em` peuvent s'accumuler en cas d'imbrication (effet cascade), ce qui peut rendre les calculs complexes.
- `rem` **(Root** `em`**) :** Sa valeur est toujours relative à la taille de police de l'**élément racine** du document (la balise `<html>`).
  - *Particularité :* Elle reste **constante** partout dans la page, quel que soit le niveau d'imbrication de l'élément.

**2. Exemple comparatif**

Imaginons la configuration CSS suivante :

```css
/* Définition de la racine */
html {
  font-size: 16px; /* 1rem vaudra toujours 16px */
}

/* Un conteneur avec une police plus grande */
.container {
  font-size: 20px; /* 1em vaudra ici 20px pour les enfants */
}

.box-em {
  font-size: 1.5em; /* 1.5 × 20px (parent) = 30px */
}

.box-rem {
  font-size: 1.5rem; /* 1.5 × 16px (racine html) = 24px */
}
```

**Ce qui se passe avec l'imbrication :**

Si vous imbriquez des blocs en `em`, la taille est recalculée à chaque niveau :

- Un bloc parent de 1.2em
- Contenant un enfant de 1.2em
- L'enfant fera : 1.2 × 1.2 = 1.44em par rapport au grand-parent. Avec `rem`, l'enfant garderait exactement la taille définie initialement, garantissant une meilleure prévisibilité.

**Résumé des cas d'usage**

| **Unité** | **Élément de référence** | **Avantage principal** | **Inconvénient** |
|-----------|--------------------------|------------------------|------------------|
| em | Le parent direct | Idéal pour les composants scalaires (ex: un bouton dont le padding grandit proportionnellement à sa propre police). | Effet d'accumulation difficile à anticiper lors d'imbrications profondes. |
| rem | La balise `<html>` | Prévisible et cohérent sur l'ensemble du site. Idéal pour la mise en page globale et la typographie. | Moins contextuel pour certains composants autonomes. |

> Je comprends que em adapte sa taille a son parent/container.

---

### 2. Comment utiliser la propriété CSS text-glow pour faire briller un titre ?

> Je n'ai jamais utilisé text-glow mais je suppose qu'on l'ecrit : hx { text-glow : #xxxxxx ; }

#### ChatGPT

En CSS, la propriété `text-glow` n'existe pas nativement. Pour faire briller un titre, on utilise `text-shadow`, qui permet d'ajouter une ou plusieurs ombres lumineuses autour du texte.

**1. Créer un titre lumineux**

Voici un exemple simple avec un effet néon bleu :

```html
<h1 class="glow">Titre lumineux</h1>
```

```css
.glow {
  color: #63e6ff;
  text-shadow:
    0 0 5px #63e6ff,
    0 0 10px #63e6ff,
    0 0 20px #00aaff,
    0 0 40px #0077ff;
}
```

**2. Comprendre text-shadow**

La syntaxe est la suivante :

```css
text-shadow: décalage-x décalage-y flou couleur;
```

- `0 0` : aucune déviation, la lumière entoure le texte.
- `5px`, `10px`, `20px` : le rayon de flou, qui diffuse la lumière.
- `#63e6ff` : la couleur de la lueur.
- Plusieurs ombres superposées créent un effet lumineux plus intense.

**3. Personnaliser la couleur et l'intensité**

Couleur de la lueur : `#00e5ff` Bleu cyan, Rose néon, Vert électrique, Jaune doré.
Diffusion de la lumière : Faible, Moyenne, Forte.
Intensité : Discrète, Néon.

**4. Ajouter un effet de scintillement**

Pour que le titre brille de manière animée, ajoute une animation CSS :

```css
.glow {
  color: #63e6ff;
  text-shadow: 0 0 10px #00aaff, 0 0 25px #0077ff;
  animation: scintillement 1.5s ease-in-out infinite alternate;
}

@keyframes scintillement {
  from {
    text-shadow: 0 0 5px #00aaff, 0 0 10px #0077ff;
  }
  to {
    text-shadow: 0 0 15px #00aaff, 0 0 35px #0077ff;
  }
}
```

Conseil : privilégie un texte clair sur un fond sombre pour obtenir un effet néon bien visible. Pour approfondir, consulte la documentation [MDN sur text-shadow](https://developer.mozilla.org/fr/docs/Web/CSS/Reference/Properties/text-shadow?utm_source=chatgpt.com).

#### Claude

Il n'existe pas de propriété CSS `text-glow`. L'effet de lueur s'obtient avec `text-shadow`. Une ombre floue placée juste derrière le texte, sans décalage, crée une lueur autour des lettres.

**Lueur simple**

```css
h1 {
  color: #fff;
  background: #111; /* le fond sombre fait ressortir la lueur */
  text-shadow: 0 0 10px #00e5ff;
}
```

La syntaxe est `text-shadow: décalage-x décalage-y flou couleur;`. Avec `0 0` pour les décalages, l'ombre est centrée derrière les lettres. Plus le flou est grand, plus la lueur s'étend.

**Lueur plus intense (effet néon)**

On peut cumuler plusieurs ombres sur un même texte en les séparant par des virgules. En empilant des flous croissants, la lueur gagne en intensité et s'estompe progressivement :

```css
h1 {
  color: #fff;
  text-shadow:
    0 0 5px #fff,
    0 0 10px #00e5ff,
    0 0 20px #00e5ff,
    0 0 40px #00e5ff;
}
```

**Lueur qui pulse**

Pour un effet de pulsation, on anime la propriété `text-shadow` avec `@keyframes`.

```css
h1 {
  color: #fff;
  animation: glow 2s ease-in-out infinite alternate;
}

@keyframes glow {
  from { text-shadow: 0 0 5px #00e5ff; }
  to { text-shadow: 0 0 10px #00e5ff, 0 0 30px #00e5ff; }
}
```

**Points à retenir**

- **Fond sombre** : une lueur claire est à peine visible sur un fond clair.
- **Lisibilité** : garde un flou raisonnable, une lueur trop forte rend le titre difficile à lire.
- **text-shadow ou box-shadow** : `text-shadow` suit la forme des lettres, alors que `box-shadow` ombre le rectangle de l'élément. `text-shadow` n'a pas non plus de valeur de spread.
- **Accessibilité** : si tu animes la lueur, tu peux désactiver l'animation pour les personnes qui le demandent :

```css
@media (prefers-reduced-motion: reduce) {
  h1 { animation: none; }
}
```

#### Gemini

En CSS, il n'existe pas de propriété native appelée `text-glow`. Pour créer un effet de lueur ou de brillance (type néon) sur un titre, on utilise la propriété standard `text-shadow` en combinant un flou important et une couleur lumineuse.

Voici comment procéder pour faire briller un titre efficacement.

**1. Effet de lueur simple (Glow de base)**

Pour donner une simple lueur colorée autour du texte, vous définissez un décalage nul (0 et 0) et un rayon de flou élevé :

```css
h1 {
  color: #ffffff; /* Couleur principale du texte (souvent claire) */
  text-shadow: 0 0 10px #00ffc0; /* Flou de 10px avec une couleur cyan lumineuse */
}
```

**2. Effet "Néon" intense (Multiples ombres)**

Pour obtenir un effet de lueur réaliste et puissant (comme un tube néon), la technique consiste à superposer **plusieurs ombres** avec des rayons de flou croissants. Les ombres les plus nettes créent le cœur de la lumière, tandis que les plus larges diffusent le halo :

```css
h1 {
  color: #fff;
  text-shadow:
    0 0 5px #ff00de,  /* Cœur de la lueur (très net) */
    0 0 10px #ff00de, /* Diffusion moyenne */
    0 0 20px #ff00de, /* Diffusion large */
    0 0 40px #ff00de; /* Halo lointain */
}
```

**Comprendre la syntaxe de `text-shadow`**

La propriété prend généralement cette structure : `text-shadow: [décalage-X] [décalage-Y] [rayon-de-flou] [couleur];`

- **Décalage X et Y (`0 0`)** : Mis à zéro, la lueur est parfaitement centrée et diffuse tout autour des lettres de manière homogène.
- **Rayon de flou (`10px`, `20px`...)** : Plus la valeur est grande, plus la lueur s'étale et devient douce.
- **Couleur** : Idéalement choisie dans des tons vifs ou pastel lumineux sur un fond sombre pour maximiser le contraste.

> J'ai appris que text-glow n'existe pas implicitement et qu'il faut passer pas bidouiller avec text-shadow.

---

### 3. Pourquoi faut-il utiliser la balise `<center>` pour centrer un titre en HTML5 ?

> Il faut utiliser la balise `<center>` car par défaut tout est aligné à gauche de la page.

#### ChatGPT

En HTML5, il ne faut pas utiliser la balise `<center>` pour centrer un titre, car elle est obsolète (déconseillée et non conforme aux standards HTML5).

La bonne méthode consiste à utiliser CSS pour gérer l'alignement du texte.

**1. Ancienne méthode : `<center>`**

```html
<center>
  <h1>Mon titre</h1>
</center>
```

Cette méthode fonctionnait dans les anciennes versions de HTML, mais la balise `<center>` a été supprimée de la norme HTML5.

**2. Méthode recommandée en HTML5**

Utilisez la propriété CSS `text-align: center;`.

```html
<h1 style="text-align: center;">Mon titre</h1>
```

Ou, mieux encore, séparez le HTML du CSS :

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Ma page</title>
  <style>
    h1 {
      text-align: center;
    }
  </style>
</head>
<body>
  <h1>Mon titre</h1>
</body>
</html>
```

**Pourquoi privilégier CSS ?**

- Conformité : CSS est la méthode standard pour la mise en forme en HTML5.
- Maintenance : vous pouvez modifier le style sans toucher au contenu HTML.
- Réutilisation : vous pouvez centrer plusieurs titres avec une seule règle CSS.

À retenir : HTML définit la structure de la page, tandis que CSS définit sa présentation. Pour centrer un titre, utilisez `text-align: center;` plutôt que `<center>`.

#### Claude

Le nom de la balise n'apparaît pas dans votre message (elle a sans doute été prise pour du HTML et masquée), mais je suppose que vous pensez à `<center>`. Si c'est le cas, la prémisse est inversée : en HTML5, il ne faut **pas** l'utiliser pour centrer un titre.

`<center>` est une balise **obsolète** en HTML5. Elle ne fait plus partie de la spécification, même si les navigateurs la gèrent encore par compatibilité. Il y a deux raisons à cela :

- **Séparation du fond et de la forme** : le HTML décrit la structure et le sens du contenu (ceci est un titre, ceci est un paragraphe), tandis que le CSS gère l'apparence. `<center>` est une balise purement présentationnelle, qui mélange les deux.
- **Maintenabilité** : avec du CSS, vous centrez tous vos titres en une seule règle, au lieu de balises dispersées dans chaque page.

La bonne pratique est donc :

```html
<h1>Mon titre</h1>
```

```css
h1 {
  text-align: center;
}
```

`text-align: center` centre le texte dans son conteneur. Pour centrer un bloc entier, on utilise plutôt `margin: 0 auto;` (avec une largeur définie) ou Flexbox (`display: flex; justify-content: center;`).

Si votre question vient d'un QCM ou d'un exercice avec une autre balise, dites-moi laquelle et je reprendrai la réponse.

#### Gemini

En réalité, **il ne faut pas** utiliser la balise `<center>` en HTML5.

Cette balise est complètement **obsolète** : elle a été dépréciée dès HTML 4.01 et définitivement supprimée dans les spécifications d'HTML5.

**La bonne méthode en HTML5 et CSS**

Pour centrer un titre ou n'importe quel autre élément, la norme moderne consiste à utiliser les feuilles de style **CSS** avec la propriété `text-align` :

```html
<!-- Utilisation directe (en ligne) -->
<h1 style="text-align: center;">Mon titre centré</h1>
```

Ou, ce qui est recommandé, en utilisant une classe CSS dédiée :

```html
<!-- En HTML -->
<h1 class="titre-centre">Mon titre centré</h1>

<!-- En CSS -->
<style>
  .titre-centre {
    text-align: center;
  }
</style>
```

**Pourquoi ne plus utiliser `<center>` ?**

- **Séparation des rôles :** Le HTML doit uniquement servir à structurer le contenu (le sens des balises), tandis que le CSS gère toute la mise en forme visuelle.
- **Maintenabilité :** Si vous devez modifier l'alignement de vos titres sur tout un site web, il est beaucoup plus facile de le faire dans un fichier CSS centralisé plutôt que de modifier chaque balise de présentation.
- **Standards du Web :** Bien que certains navigateurs continuent de l'afficher par rétrocompatibilité, l'utiliser aujourd'hui produit un code non conforme aux standards actuels.

> J'ai appris que la balise `<center>` est une balise html obsolète aux standards html5 comme toutes les balises de style inclut dans le html comme `<b>`

---

### 4. Reposer la question 1 en anglais à l'un des assistants, puis comparer les deux réponses.

> What is the difference between the CSS units em and rem? Give an example.

#### ChatGPT

The main difference between CSS `em` and `rem` is which font size they use as their reference.

- `em` — Relative to the font size of the element's parent (with some property-specific nuances).
- `rem` — Relative to the font size of the root element (`html`).

**Example**

```css
html {
  font-size: 16px;
}

.parent {
  font-size: 20px;
}

.child {
  font-size: 1.5em; /* 30px: 1.5 × 20px */
  margin: 1rem;     /* 16px: 1 × 16px */
}
```

Here, `.child` has a font size of 30px because `1.5em` is calculated from its parent's 20px font size. Its margin is 16px because `1rem` is calculated from the root's 16px font size.

**Visual comparison**

**em**

Follows the parent

- If the parent's font size changes, the computed size in `em` usually changes too.
- Useful for spacing and sizing that should scale with a component's text.

**rem**

Follows the root

- Its size remains consistent across nested elements, provided the root font size doesn't change.
- Useful for consistent typography, page spacing, and layout dimensions.

**A common pitfall with em**

Because `em` can compound through nested elements, sizes can grow unexpectedly:

```css
.parent {
  font-size: 1.2em;
}

.child {
  font-size: 1.2em;
}
```

If the root font size is 16px, and both elements inherit from their parent in this example, the `.parent` font size becomes 19.2px, and the `.child` font size becomes 23.04px.

With `rem`, both elements would be 19.2px if each used `font-size: 1.2rem`.

Rule of thumb: Use `rem` for predictable, consistent sizing across your page, and use `em` when you want sizing to scale relative to a component's text or parent context.

#### Claude

Both are relative units, but they differ in what they're relative to:

- **em** is relative to the font size of the **current element**. For `font-size` itself, it's relative to the **parent's** font size. Because of this, `em` values compound when elements are nested.
- **rem** ("root em") is always relative to the font size of the **root element** (`<html>`), which is 16px by default in most browsers. It never compounds.

**Example**

```html
<div class="box-em">
  Level 1
  <div class="box-em">
    Level 2
    <div class="box-em">Level 3</div>
  </div>
</div>

<div class="box-rem">
  Level 1
  <div class="box-rem">
    Level 2
    <div class="box-rem">Level 3</div>
  </div>
</div>
```

```css
html {
  font-size: 16px;
}

.box-em { font-size: 1.5em; }  /* relative to parent */
.box-rem { font-size: 1.5rem; } /* relative to <html> */
```

**Result with em:** the sizes compound at each nesting level.

- Level 1: 1.5 × 16 = **24px**
- Level 2: 1.5 × 24 = **36px**
- Level 3: 1.5 × 36 = **54px**

**Result with rem:** every level is the same.

- Level 1, 2 and 3: 1.5 × 16 = **24px**

**When to use which**

- **rem** is the safer default for font sizes, margins and layout spacing, since it's predictable and respects the user's browser font-size setting.
- **em** is useful when you want something to scale with its own element's text, such as the padding of a button:

```css
.button {
  font-size: 1rem;
  padding: 0.5em 1em; /* scales if you change the button's font-size */
}
```

#### Gemini

The difference between `em` and `rem` comes down to **what they scale relative to**:

- `em` is relative to the font-size of its **immediate parent element** (or the element itself, if used for `font-size`).
- `rem` (root em) is relative to the font-size of the **root element** of the document (the `<html>` tag).

**Detailed Breakdown**

**1. The `em` Unit**

Because `em` looks at the parent element's font size, it can lead to **compounding effects** when elements are nested inside one another.

- If a parent container has a font size of 16px, then `1.5em` equals 24px.
- However, if you place a child element inside that container and also give it `1.5em`, its size becomes 1.5 times *the parent's size* (24px × 1.5 = 36px), rather than the root size. This makes deep nesting hard to predict.

**2. The `rem` Unit**

Because `rem` always looks back to the `<html>` root element (which defaults to 16px in most browsers unless changed), **it never compounds**.

- No matter how deeply nested an element is inside other containers, `1.5rem` will always equal 16px × 1.5 = 24px. This makes `rem` much more predictable for global layout, margins, paddings, and typography.

**Code Example**

```html
<!-- HTML Structure -->
<html style="font-size: 16px;">
  <body>
    <div class="parent">
      Parent box (font-size: 1.5rem = 24px)
      <div class="child">
        Child box
      </div>
    </div>
  </body>
</html>
```

```css
/* CSS */
.parent {
  font-size: 1.5rem; /* 1.5 * 16px = 24px */
}

/* Using em */
.child-em {
  font-size: 1.5em;
  /* Compounds! 1.5 * parent's 24px = 36px */
}

/* Using rem */
.child-rem {
  font-size: 1.5rem;
  /* Consistent! Always 1.5 * root's 16px = 24px */
}
```

**When to Use Which?**

- **Use `rem`** for almost everything—font sizes, margins, paddings, and widths—to maintain a consistent, predictable scaling system across your entire layout.
- **Use `em`** specifically for component-level scaling where you want an element's padding, margins, or borders to scale proportionally with its own changing font size (such as making a button's padding automatically adjust if the button text is made larger or smaller).

> En anglais, ChatGPT et Claude font moins de ligne et Gemini est capable d'en faire encore plus.

---

## Partie B1

### 1. What is `<th scope="">`

`<th scope="">` is an HTML attribute used on a table header cell (`<th>`). It tells the browser and assistive technologies what the header applies to.

Common values:

- `scope="col"`: this header describes a column
- `scope="row"`: this header describes a row
- `scope="colgroup"` / `scope="rowgroup"`: for grouped columns or rows

Example from `livraison.html`:

- `<th scope="col">Zone</th>` means "the column named Zone"
- `<th scope="row">Zone 1</th>` means "this row is the Zone 1 row"

This improves accessibility because screen readers can understand the relationship between headers and cells in the table.

> Scope sert à définir sur c'est une ligne ou une colonne

### 2. What is aspect-ratio

`aspect-ratio` is a CSS property that defines the width-to-height ratio of an element.

Examples:

```css
.card {
  aspect-ratio: 16 / 9;
}
```

This means the element keeps a 16:9 ratio, so it will resize while maintaining that proportion.

It is commonly used for:

- images
- videos
- cards
- product thumbnails
- responsive media blocks

Examples:

```css
img {
  width: 100%;
  aspect-ratio: 1 / 1; /* square */
}

.video {
  aspect-ratio: 4 / 3;
}
```

Important:

- If the element has a width, the height is calculated automatically.
- If it has a height, the width is calculated automatically.
- It helps avoid layout shifts and preserves consistent proportions.

Example:

```css
.photo {
  width: 300px;
  aspect-ratio: 4 / 3;
}
```

This gives:

- width = 300px
- height = 225px

Because 300 / 4 * 3 = 225.

If you want, I can also show how it works with `img`, `video`, and `responsive cards` in real HTML/CSS examples.

> Aspect ratio sert a modifier le ratio d'une image, video et cards.

### 3. What is `object-fit: cover`

`object-fit: cover` is a CSS property used on images or videos inside a box with a fixed width/height.

It makes the media fill the container completely while respecting its aspect ratio, without distortion.

Example:

```css
img {
  width: 400px;
  height: 250px;
  object-fit: cover;
}
```

What happens:

- The image keeps its original proportions
- It is cropped if necessary to fill the entire box
- It does not stretch

This is useful when you want a banner, gallery image, or product photo to fit a container neatly.

Example:

- Container: 400x250
- Image: 800x500
- Result: the image fills the whole area, and some parts may be cropped

Unlike `contain`, which keeps the whole image visible, `cover` fills the container even if some of the image is cut off.

Common use:

```css
.banniere img {
  width: 100%;
  height: 300px;
  object-fit: cover;
}
```

If you want, I can also explain the difference between:

- `cover`
- `contain`
- `fill`
- `none`
- `scale-down`

> Object-fit: cover remplit l'espace donné a une image dans son entièreté quitte à couper l'image.

### 4. What is overflow-x

`overflow-x` is a CSS property that controls what happens when content is wider than its container horizontally.

It applies to the left/right overflow of an element.

Common values:

- `visible` (default): content can extend outside the box
- `hidden`: hide the overflowing content
- `scroll`: always show a horizontal scrollbar
- `auto`: show a scrollbar only when needed

Example:

```css
.container {
  width: 300px;
  overflow-x: auto;
}
```

This allows the user to scroll horizontally if the content is wider than 300px.

Example with hidden overflow:

```css
.banner {
  overflow-x: hidden;
}
```

This prevents horizontal content from being visible beyond the container.

Useful for:

- tables on small screens
- long text blocks
- carousels
- horizontally scrolling sections

If the content is wider because of:

- long words
- fixed-width elements
- large images
- tables

then `overflow-x` can help manage that layout.

Related property:

- `overflow-y` handles vertical overflow

If you want, I can explain the difference between `overflow-x: hidden` and `overflow: hidden` too.

> Overflow-x permet de faire apparaître une barre de scroll si le contenu concerné est plus grand que son conteneur.

---

## Partie B2

- Overflow x : scroll va montrer une barre de scroll même si ce n'est pas nécessaire
- Aspect-ratio : 9 / 9 mettra les images en carré
- Z-index : 0 fera disparaître l'entête en la mettant derrière tous les autres éléments qui sont de base en 1

---

## Partie D

Rédiger cinq règles personnelles d'usage de l'IA

1. Vérifier les informations
2. Ne pas divulguer de données personnelles
3. Questionner les IA sur leurs explications
4. Utiliser l'IA pour m'aider pas pour faire à ma place
5. Demander a plusieurs IA pour plus de sources
