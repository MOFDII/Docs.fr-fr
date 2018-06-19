---
title: Inférieur, Sass et la police impressionnant dans ASP.NET Core
author: ardalis
description: Découvrez comment utiliser inférieur, Sass et police impressionnant dans les applications ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/less-sass-fa
ms.openlocfilehash: 3bb1c9006f8633485a420b52b5fa9b91b1875cc5
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30073521"
---
# <a name="less-sass-and-font-awesome-in-aspnet-core"></a><span data-ttu-id="1627d-103">Inférieur, Sass et la police impressionnant dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1627d-103">Less, Sass, and Font Awesome in ASP.NET Core</span></span>

<span data-ttu-id="1627d-104">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="1627d-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1627d-105">Les utilisateurs d’applications web ont des exigences plus élevées lorsqu’il s’agit du style et expérience globale.</span><span class="sxs-lookup"><span data-stu-id="1627d-105">Users of web applications have increasingly high expectations when it comes to style and overall experience.</span></span> <span data-ttu-id="1627d-106">Les applications web modernes fréquemment tirer parti des outils riches et infrastructures pour définir et gérer leur apparence et la convivialité de façon cohérente.</span><span class="sxs-lookup"><span data-stu-id="1627d-106">Modern web applications frequently leverage rich tools and frameworks for defining and managing their look and feel in a consistent manner.</span></span> <span data-ttu-id="1627d-107">Comme les infrastructures [Bootstrap](http://getbootstrap.com/) peuvent accéder permettent de définir un ensemble commun des styles et options de disposition pour les sites web.</span><span class="sxs-lookup"><span data-stu-id="1627d-107">Frameworks like [Bootstrap](http://getbootstrap.com/) can go a long way toward defining a common set of styles and layout options for web sites.</span></span> <span data-ttu-id="1627d-108">Toutefois, la plupart des sites non trivial également bénéficier de pouvoir définir et mettre à jour les styles et les fichiers de feuille de style en cascade de manière efficace, mais aussi ayant un accès facile aux icônes non-image qui permettent d’interface du site plus intuitive.</span><span class="sxs-lookup"><span data-stu-id="1627d-108">However, most non-trivial sites also benefit from being able to effectively define and maintain styles and cascading style sheet (CSS) files, as well as having easy access to non-image icons that help make the site's interface more intuitive.</span></span> <span data-ttu-id="1627d-109">C’est là langages et outils qui prennent en charge [moins](http://lesscss.org/) et [Sass](http://sass-lang.com/), et les bibliothèques telles que [police impressionnant](http://fontawesome.io/), sont disponibles dans.</span><span class="sxs-lookup"><span data-stu-id="1627d-109">That's where languages and tools that support [Less](http://lesscss.org/) and [Sass](http://sass-lang.com/), and libraries like [Font Awesome](http://fontawesome.io/), come in.</span></span>

## <a name="css-preprocessor-languages"></a><span data-ttu-id="1627d-110">Langues de préprocesseur CSS</span><span class="sxs-lookup"><span data-stu-id="1627d-110">CSS preprocessor languages</span></span>

<span data-ttu-id="1627d-111">Les langues qui sont compilés dans d’autres langues, afin d’améliorer l’expérience de l’utilisation de la langue sous-jacent, sont appelés Préprocesseurs.</span><span class="sxs-lookup"><span data-stu-id="1627d-111">Languages that are compiled into other languages, in order to improve the experience of working with the underlying language, are referred to as preprocessors.</span></span> <span data-ttu-id="1627d-112">Il existe deux Préprocesseurs populaires pour CSS : inférieur et Sass.</span><span class="sxs-lookup"><span data-stu-id="1627d-112">There are two popular preprocessors for CSS: Less and Sass.</span></span>  <span data-ttu-id="1627d-113">Ces Préprocesseurs ajouter des fonctionnalités à CSS, telles que la prise en charge pour les variables et les règles imbriquées, qui améliorent la facilité de maintenance de feuilles de style volumineuses et complexes.</span><span class="sxs-lookup"><span data-stu-id="1627d-113">These preprocessors add features to CSS, such as support for variables and nested rules, which improve the maintainability of large, complex stylesheets.</span></span> <span data-ttu-id="1627d-114">CSS en tant que langage est très simple, ne dispose pas de prise en charge de la même pour un élément simple en tant que variables, et cela a tendance à rendre les fichiers CSS répétitives et trop importants.</span><span class="sxs-lookup"><span data-stu-id="1627d-114">CSS as a language is very basic, lacking support even for something as simple as variables, and this tends to make CSS files repetitive and bloated.</span></span> <span data-ttu-id="1627d-115">Ajout de fonctionnalités de langage de programmation réel via Préprocesseurs peut aider à réduire la duplication et fournir une meilleure organisation des règles de style.</span><span class="sxs-lookup"><span data-stu-id="1627d-115">Adding real programming language features via preprocessors can help reduce duplication and provide better organization of styling rules.</span></span> <span data-ttu-id="1627d-116">Visual Studio fournit une prise en charge intégrée pour les deux moins et Sass, ainsi que les extensions qui peuvent améliorer l’expérience de développement lorsque vous travaillez avec ces langages.</span><span class="sxs-lookup"><span data-stu-id="1627d-116">Visual Studio provides built-in support for both Less and Sass, as well as extensions that can further improve the development experience when working with these languages.</span></span>

<span data-ttu-id="1627d-117">Prenons un exemple de comment Préprocesseurs peuvent améliorer la lisibilité et facilité de gestion des informations de style, CSS :</span><span class="sxs-lookup"><span data-stu-id="1627d-117">As a quick example of how preprocessors can improve readability and maintainability of style information, consider this CSS:</span></span>

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

<span data-ttu-id="1627d-118">Utilisation moindre, cela peut être réécrit pour éliminer tous les doublons, à l’aide un *mixin* (ainsi nommé, car il permet de « mix » propriétés d’une classe ou d’ensemble de règles dans un autre) :</span><span class="sxs-lookup"><span data-stu-id="1627d-118">Using Less, this can be rewritten to eliminate all of the duplication, using a *mixin* (so named because it allows you to "mix in" properties from one class or rule-set into another):</span></span>

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a><span data-ttu-id="1627d-119">Moins</span><span class="sxs-lookup"><span data-stu-id="1627d-119">Less</span></span>

<span data-ttu-id="1627d-120">Le moins CSS préprocesseur s’exécute à l’aide de Node.js.</span><span class="sxs-lookup"><span data-stu-id="1627d-120">The Less CSS preprocessor runs using Node.js.</span></span> <span data-ttu-id="1627d-121">Pour installer inférieure, utilisez le Gestionnaire de Package de nœud (npm) à partir d’une invite de commandes (-g signifie « globaux ») :</span><span class="sxs-lookup"><span data-stu-id="1627d-121">To install Less, use Node Package Manager (npm) from a command prompt (-g means "global"):</span></span>

```console
npm install -g less
```

<span data-ttu-id="1627d-122">Si vous utilisez Visual Studio, vous pouvez commencer à utiliser moins de ressources en ajoutant un ou plusieurs fichiers inférieure à votre projet, puis en configurant Gulp (ou Grunt) pour les traiter au moment de la compilation.</span><span class="sxs-lookup"><span data-stu-id="1627d-122">If you're using Visual Studio, you can get started with Less by adding one or more Less files to your project, and then configuring Gulp (or Grunt) to process them at compile-time.</span></span> <span data-ttu-id="1627d-123">Ajouter un *Styles* dossier à votre projet, puis ajoutez un nouveau moins un fichier nommé *main.less* dans ce dossier.</span><span class="sxs-lookup"><span data-stu-id="1627d-123">Add a *Styles* folder to your project, and then add a new Less file named *main.less* to this folder.</span></span>

![Ajouter le fichier](less-sass-fa/_static/add-less-file.png)

<span data-ttu-id="1627d-125">Une fois ajoutée, la structure des dossiers doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="1627d-125">Once added, your folder structure should look something like this:</span></span>

![Structure de dossiers](less-sass-fa/_static/folder-structure.png)

<span data-ttu-id="1627d-127">Maintenant, vous pouvez ajouter des styles de base pour le fichier, qui sera compilé en CSS et déployé dans le dossier wwwroot par Gulp.</span><span class="sxs-lookup"><span data-stu-id="1627d-127">Now you can add some basic styling to the file, which will be compiled into CSS and deployed to the wwwroot folder by Gulp.</span></span>

<span data-ttu-id="1627d-128">Modifier *main.less* pour inclure le contenu suivant, qui crée une palette de couleurs simple à partir d’une couleur de base unique.</span><span class="sxs-lookup"><span data-stu-id="1627d-128">Modify *main.less* to include the following content, which creates a simple color palette from a single base color.</span></span>

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

<span data-ttu-id="1627d-129">`@base` et l’autre @-prefixed éléments sont des variables.</span><span class="sxs-lookup"><span data-stu-id="1627d-129">`@base` and the other @-prefixed items are variables.</span></span> <span data-ttu-id="1627d-130">Chacun d’eux représente une couleur.</span><span class="sxs-lookup"><span data-stu-id="1627d-130">Each of them represents a color.</span></span> <span data-ttu-id="1627d-131">À l’exception de `@base`, ils sont définis à l’aide des fonctions de couleur : éclaircir assombrissement et de rotation.</span><span class="sxs-lookup"><span data-stu-id="1627d-131">Except for `@base`, they're set using color functions: lighten, darken, and spin.</span></span> <span data-ttu-id="1627d-132">Éclaircir et assombrissement faire à peu près ce que vous attendez ; rotation ajuste la teinte d’une couleur d’un nombre de degrés (autour de la roulette de la couleur).</span><span class="sxs-lookup"><span data-stu-id="1627d-132">Lighten and darken do pretty much what you would expect; spin adjusts the hue of a color by a number of degrees (around the color wheel).</span></span> <span data-ttu-id="1627d-133">Moins le processeur est assez intelligent pour ignorer les variables qui ne sont pas utilisées, donc pour illustrer le fonctionnement de ces variables, vous devez les utiliser quelque part.</span><span class="sxs-lookup"><span data-stu-id="1627d-133">The Less processor is smart enough to ignore variables that aren't used, so to demonstrate how these variables work, we need to use them somewhere.</span></span> <span data-ttu-id="1627d-134">Les classes `.baseColor`, etc. va vous montrer les valeurs calculées de chacune des variables dans le fichier CSS qui est généré.</span><span class="sxs-lookup"><span data-stu-id="1627d-134">The classes `.baseColor`, etc. will demonstrate the calculated values of each of the variables in the CSS file that's produced.</span></span>

### <a name="get-started"></a><span data-ttu-id="1627d-135">Prise en main</span><span class="sxs-lookup"><span data-stu-id="1627d-135">Get started</span></span>

<span data-ttu-id="1627d-136">Créer un **fichier de Configuration npm** (*package.json*) dans votre dossier de projet et le modifier pour faire référence à `gulp` et `gulp-less`:</span><span class="sxs-lookup"><span data-stu-id="1627d-136">Create an **npm Configuration File** (*package.json*) in your project folder and edit it to reference `gulp` and `gulp-less`:</span></span>

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

<span data-ttu-id="1627d-137">Installer les dépendances au niveau d’une invite de commandes dans votre dossier de projet, ou dans Visual Studio **l’Explorateur de solutions** (**dépendances > npm > Restaurer les packages**).</span><span class="sxs-lookup"><span data-stu-id="1627d-137">Install the dependencies either at a command prompt in your project folder, or in Visual Studio **Solution Explorer** (**Dependencies > npm > Restore packages**).</span></span>

```console
npm install
```

![Restauration des packages VS](less-sass-fa/_static/restore-packages.png)

<span data-ttu-id="1627d-139">Dans le dossier du projet, créez un **Gulp un fichier de Configuration** (*gulpfile.js*) pour définir le processus automatisé.</span><span class="sxs-lookup"><span data-stu-id="1627d-139">In the project folder, create a **Gulp Configuration File** (*gulpfile.js*) to define the automated process.</span></span>  <span data-ttu-id="1627d-140">Ajoutez une variable en haut du fichier pour représenter inférieur et une tâche à exécuter inférieur :</span><span class="sxs-lookup"><span data-stu-id="1627d-140">Add a variable at the top of the file to represent Less, and a task to run Less:</span></span>

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

<span data-ttu-id="1627d-141">Ouvrez le **Explorateur d’exécuteur de tâches** (**vue > autres fenêtres > Explorateur d’exécuteur de tâches**).</span><span class="sxs-lookup"><span data-stu-id="1627d-141">Open the **Task Runner Explorer** (**View > Other Windows > Task Runner Explorer**).</span></span> <span data-ttu-id="1627d-142">Parmi les tâches, vous devez voir une nouvelle tâche nommée `less`.</span><span class="sxs-lookup"><span data-stu-id="1627d-142">Among the tasks, you should see a new task named `less`.</span></span> <span data-ttu-id="1627d-143">Vous devrez peut-être actualiser la fenêtre.</span><span class="sxs-lookup"><span data-stu-id="1627d-143">You might have to refresh the window.</span></span>

<span data-ttu-id="1627d-144">Exécutez le `less` tâche et que vous consultez une sortie similaire à celui indiqué ici :</span><span class="sxs-lookup"><span data-stu-id="1627d-144">Run the `less` task, and you see output similar to what is shown here:</span></span>

![moins exécuteur de tâches](less-sass-fa/_static/less-task-runner.png)

<span data-ttu-id="1627d-146">Le *wwwroot/css* dossier contient désormais un nouveau fichier, *main.css*:</span><span class="sxs-lookup"><span data-stu-id="1627d-146">The *wwwroot/css* folder now contains a new file, *main.css*:</span></span>

![css principal créé](less-sass-fa/_static/main-css-created.png)

<span data-ttu-id="1627d-148">Ouvrez *main.css* et vous voyez quelque chose comme suit :</span><span class="sxs-lookup"><span data-stu-id="1627d-148">Open *main.css* and you see something like the following:</span></span>

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

<span data-ttu-id="1627d-149">Ajouter une page HTML simple à la *wwwroot* dossier et référence *main.css* pour afficher la palette de couleurs en action.</span><span class="sxs-lookup"><span data-stu-id="1627d-149">Add a simple HTML page to the *wwwroot* folder, and reference *main.css* to see the color palette in action.</span></span>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

<span data-ttu-id="1627d-150">Vous pouvez voir que le degré de 180 lancer sur `@base` utilisé pour produire `@background` a entraîné la roue opposées couleur de `@base`:</span><span class="sxs-lookup"><span data-stu-id="1627d-150">You can see that the 180 degree spin on `@base` used to produce `@background` resulted in the color wheel opposing color of `@base`:</span></span>

![exemple de test inférieur](less-sass-fa/_static/less-test-screenshot.png)

<span data-ttu-id="1627d-152">Inférieur prend également en charge pour les règles imbriquées, ainsi que les requêtes de média imbriquée.</span><span class="sxs-lookup"><span data-stu-id="1627d-152">Less also provides support for nested rules, as well as nested media queries.</span></span> <span data-ttu-id="1627d-153">Par exemple, la définition des hiérarchies imbriquées telles que les menus peuvent entraîner des règles CSS verbose comme ceux-ci :</span><span class="sxs-lookup"><span data-stu-id="1627d-153">For example, defining nested hierarchies like menus can result in verbose CSS rules like these:</span></span>

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

<span data-ttu-id="1627d-154">Dans l’idéal, toutes les règles de style associées sont placés entre eux dans le fichier CSS, mais dans la pratique il n’y a rien appliquer cette règle à l’exception de la convention et éventuellement des commentaires de bloc.</span><span class="sxs-lookup"><span data-stu-id="1627d-154">Ideally all of the related style rules will be placed together within the CSS file, but in practice there's nothing enforcing this rule except convention and perhaps block comments.</span></span>

<span data-ttu-id="1627d-155">Définition de ces règles mêmes utilisation moindre ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="1627d-155">Defining these same rules using Less looks like this:</span></span>

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

<span data-ttu-id="1627d-156">Notez que dans ce cas, tous les éléments subordonnés de `nav` contenus dans son étendue.</span><span class="sxs-lookup"><span data-stu-id="1627d-156">Note that in this case, all of the subordinate elements of `nav` are contained within its scope.</span></span> <span data-ttu-id="1627d-157">Il n’est plus toute répétition des éléments parents (`nav`, `li`, `a`), et le nombre total de la ligne a été supprimé également (bien que certaines des qui est un résultat de l’utilisation de valeurs sur les mêmes lignes dans le deuxième exemple).</span><span class="sxs-lookup"><span data-stu-id="1627d-157">There's no longer any repetition of parent elements (`nav`, `li`, `a`), and the total line count has dropped as well (though some of that's a result of putting values on the same lines in the second example).</span></span> <span data-ttu-id="1627d-158">Il peut être très utile, Oui, pour afficher toutes les règles pour un élément d’interface utilisateur donné au sein d’une portée limitée explicitement, dans ce cas définie du reste du fichier par des accolades.</span><span class="sxs-lookup"><span data-stu-id="1627d-158">It can be very helpful, organizationally, to see all of the rules for a given UI element within an explicitly bounded scope, in this case set off from the rest of the file by curly braces.</span></span>

<span data-ttu-id="1627d-159">Le `&` syntaxe est une fonctionnalité moins sélecteur et qui représente le parent de sélection en cours.</span><span class="sxs-lookup"><span data-stu-id="1627d-159">The `&` syntax is a Less selector feature, with & representing the current selector parent.</span></span> <span data-ttu-id="1627d-160">Par conséquent, dans l’un {...}</span><span class="sxs-lookup"><span data-stu-id="1627d-160">So, within the a {...}</span></span> <span data-ttu-id="1627d-161">bloc, `&` représente un `a` balise et par conséquent `&:link` équivaut à `a:link`.</span><span class="sxs-lookup"><span data-stu-id="1627d-161">block, `&` represents an `a` tag, and thus `&:link` is equivalent to `a:link`.</span></span>

<span data-ttu-id="1627d-162">Requêtes de média, extrêmement utiles pour la création d’une conception réactive, peuvent également contribuer fortement à répétition et de la complexité de CSS.</span><span class="sxs-lookup"><span data-stu-id="1627d-162">Media queries, extremely useful in creating responsive designs, can also contribute heavily to repetition and complexity in CSS.</span></span> <span data-ttu-id="1627d-163">Inférieur permet les requêtes de média être imbriqués dans les classes, afin que la définition de classe entière ne doit être répété dans différents niveau supérieur `@media` éléments.</span><span class="sxs-lookup"><span data-stu-id="1627d-163">Less allows media queries to be nested within classes, so that the entire class definition doesn't need to be repeated within different top-level `@media` elements.</span></span> <span data-ttu-id="1627d-164">Par exemple, voici CSS pour un menu réactif :</span><span class="sxs-lookup"><span data-stu-id="1627d-164">For example, here is CSS for a responsive menu:</span></span>

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

<span data-ttu-id="1627d-165">Cela peut être mieux défini dans un délai inférieur en tant que :</span><span class="sxs-lookup"><span data-stu-id="1627d-165">This can be better defined in Less as:</span></span>

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

<span data-ttu-id="1627d-166">Une autre fonctionnalité de moins que nous avons déjà vu est sa prise en charge des opérations mathématiques, ce qui permet des attributs de style doit être construite à partir de variables prédéfinies.</span><span class="sxs-lookup"><span data-stu-id="1627d-166">Another feature of Less that we have already seen is its support for mathematical operations, allowing style attributes to be constructed from pre-defined variables.</span></span> <span data-ttu-id="1627d-167">Cela facilite la mise à jour de styles connexes beaucoup plus faciles, étant donné que la variable de base peut être modifiée et toutes les valeurs dépendantes changent automatiquement.</span><span class="sxs-lookup"><span data-stu-id="1627d-167">This makes updating related styles much easier, since the base variable can be modified and all dependent values change automatically.</span></span>

<span data-ttu-id="1627d-168">Les fichiers CSS, en particulier pour les sites de grande taille (et en particulier si les requêtes de média sont utilisés), ont tendance à devenir très volumineux au fil du temps, ce qui leur utilisation lourde.</span><span class="sxs-lookup"><span data-stu-id="1627d-168">CSS files, especially for large sites (and especially if media queries are being used), tend to get quite large over time, making working with them unwieldy.</span></span> <span data-ttu-id="1627d-169">Moins de fichiers peuvent être définies séparément, puis extraites à l’aide de `@import` directives.</span><span class="sxs-lookup"><span data-stu-id="1627d-169">Less files can be defined separately, then pulled together using `@import` directives.</span></span> <span data-ttu-id="1627d-170">Inférieur peut également être utilisé pour importer CSS des fichiers individuels, de même, si vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="1627d-170">Less can also be used to import individual CSS files, as well, if desired.</span></span>

<span data-ttu-id="1627d-171">*Mixins* peut accepter des paramètres, et inférieur prend en charge une logique conditionnelle dans l’écran de protecteurs mixin, qui fournissent un moyen déclaratif de définir certaines mixins prise d’effet.</span><span class="sxs-lookup"><span data-stu-id="1627d-171">*Mixins* can accept parameters, and Less supports conditional logic in the form of mixin guards, which provide a declarative way to define when certain mixins take effect.</span></span> <span data-ttu-id="1627d-172">Une utilisation courante pour les protecteurs de mixin est pour ajuster les couleurs en fonction de la lumière ou sombre la couleur source.</span><span class="sxs-lookup"><span data-stu-id="1627d-172">A common use for mixin guards is to adjust colors based on how light or dark the source color is.</span></span> <span data-ttu-id="1627d-173">Étant donné un mixin qui accepte un paramètre de couleur, un garde mixin peut servir à modifier le mixin en fonction de cette couleur :</span><span class="sxs-lookup"><span data-stu-id="1627d-173">Given a mixin that accepts a parameter for color, a mixin guard can be used to modify the mixin based on that color:</span></span>

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

<span data-ttu-id="1627d-174">Étant donné notre actuel `@base` valeur `#663333`, moins ce script génère le code CSS suivant :</span><span class="sxs-lookup"><span data-stu-id="1627d-174">Given our current `@base` value of `#663333`, this Less script will produce the following CSS:</span></span>

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

<span data-ttu-id="1627d-175">Inférieur fournit un nombre de fonctionnalités supplémentaires, mais cela doit vous donner une idée de la puissance de ce langage de prétraitement.</span><span class="sxs-lookup"><span data-stu-id="1627d-175">Less provides a number of additional features, but this should give you some idea of the power of this preprocessing language.</span></span>

## <a name="sass"></a><span data-ttu-id="1627d-176">Sass</span><span class="sxs-lookup"><span data-stu-id="1627d-176">Sass</span></span>

<span data-ttu-id="1627d-177">Sass est similaire à une valeur inférieure, en fournissant la prise en charge pour la plupart de ces fonctionnalités, mais avec une syntaxe légèrement différente.</span><span class="sxs-lookup"><span data-stu-id="1627d-177">Sass is similar to Less, providing support for many of the same features, but with slightly different syntax.</span></span> <span data-ttu-id="1627d-178">Il est construit à l’aide de Ruby, plutôt que JavaScript, ainsi que les exigences de configuration différents.</span><span class="sxs-lookup"><span data-stu-id="1627d-178">It's built using Ruby, rather than JavaScript, and so has different setup requirements.</span></span> <span data-ttu-id="1627d-179">Le langage Sass d’origine n’a pas d’utiliser des accolades ou des points-virgules, mais au lieu de cela défini étendue à l’aide d’un espace blanc et mise en retrait.</span><span class="sxs-lookup"><span data-stu-id="1627d-179">The original Sass language didn't use curly braces or semicolons, but instead defined scope using white space and indentation.</span></span> <span data-ttu-id="1627d-180">Dans la version 3 de Sass, introduite une nouvelle syntaxe, **SCSS** (« CSS Sassy »).</span><span class="sxs-lookup"><span data-stu-id="1627d-180">In version 3 of Sass, a new syntax was introduced, **SCSS** ("Sassy CSS").</span></span> <span data-ttu-id="1627d-181">SCSS est similaire à CSS dans la mesure où il ignore les espaces blancs et les niveaux de mise en retrait et utilise à la place des points-virgules et des accolades.</span><span class="sxs-lookup"><span data-stu-id="1627d-181">SCSS is similar to CSS in that it ignores indentation levels and whitespace, and instead uses semicolons and curly braces.</span></span>

<span data-ttu-id="1627d-182">Pour installer Sass, en général, vous installez d’abord Ruby (préinstallé sur macOS) et puis exécutez :</span><span class="sxs-lookup"><span data-stu-id="1627d-182">To install Sass, typically you would first install Ruby (pre-installed on macOS), and then run:</span></span>

```console
gem install sass
```

<span data-ttu-id="1627d-183">Toutefois, si vous utilisez Visual Studio, vous pouvez commencer à utiliser Sass à peu près la même façon comme vous le feriez avec moins.</span><span class="sxs-lookup"><span data-stu-id="1627d-183">However, if you're running Visual Studio, you can get started with Sass in much the same way as you would with Less.</span></span> <span data-ttu-id="1627d-184">Ouvrez *package.json* et ajouter le package « gulp-sass » à `devDependencies`:</span><span class="sxs-lookup"><span data-stu-id="1627d-184">Open *package.json* and add the "gulp-sass" package to `devDependencies`:</span></span>

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

<span data-ttu-id="1627d-185">Ensuite, modifiez *gulpfile.js* pour ajouter une variable sass et une tâche pour compiler vos fichiers Sass et de placer les résultats dans le dossier wwwroot :</span><span class="sxs-lookup"><span data-stu-id="1627d-185">Next, modify *gulpfile.js* to add a sass variable and a task to compile your Sass files and place the results in the wwwroot folder:</span></span>

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

<span data-ttu-id="1627d-186">Vous pouvez maintenant ajouter le fichier Sass *main2.scss* à la *Styles* dossier à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="1627d-186">Now you can add the Sass file *main2.scss* to the *Styles* folder in the root of the project:</span></span>

![ajouter les fichiers scss](less-sass-fa/_static/add-scss-file.png)

<span data-ttu-id="1627d-188">Ouvrez *main2.scss* et ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="1627d-188">Open *main2.scss* and add the following:</span></span>

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

<span data-ttu-id="1627d-189">Enregistrez tous vos fichiers.</span><span class="sxs-lookup"><span data-stu-id="1627d-189">Save all of your files.</span></span> <span data-ttu-id="1627d-190">Désormais, lorsque vous actualisez **Task Runner Explorer**, vous voyez un `sass` tâche.</span><span class="sxs-lookup"><span data-stu-id="1627d-190">Now when you refresh **Task Runner Explorer**, you see a `sass` task.</span></span> <span data-ttu-id="1627d-191">Exécutez-le, puis en regardant dans la */wwwroot/css* dossier.</span><span class="sxs-lookup"><span data-stu-id="1627d-191">Run it, and look in the */wwwroot/css* folder.</span></span> <span data-ttu-id="1627d-192">Il existe désormais un *main2.css* fichier, avec ce contenu :</span><span class="sxs-lookup"><span data-stu-id="1627d-192">There's now a *main2.css* file, with these contents:</span></span>

```css
body {
    background-color: #CC0000;
}
```

<span data-ttu-id="1627d-193">Sass prend en charge l’imbrication à peu près le même a été qu’inférieur est le cas, en fournissant des avantages similaires.</span><span class="sxs-lookup"><span data-stu-id="1627d-193">Sass supports nesting in much the same was that Less does, providing similar benefits.</span></span> <span data-ttu-id="1627d-194">Peut être fractionnées en fonction de fichiers et inclus à l’aide de la `@import` directive :</span><span class="sxs-lookup"><span data-stu-id="1627d-194">Files can be split up by function and included using the `@import` directive:</span></span>

```sass
@import 'anotherfile';
```

<span data-ttu-id="1627d-195">Sass prend en charge mixins également, à l’aide de la `@mixin` mot clé pour les définir et `@include` pour les inclure, comme dans cet exemple à partir de [sass-lang.com](http://sass-lang.com):</span><span class="sxs-lookup"><span data-stu-id="1627d-195">Sass supports mixins as well, using the `@mixin` keyword to define them and `@include` to include them, as in this example from [sass-lang.com](http://sass-lang.com):</span></span>

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

<span data-ttu-id="1627d-196">En plus de mixins, Sass prend également en charge le concept d’héritage, ce qui permet une classe étendre un autre.</span><span class="sxs-lookup"><span data-stu-id="1627d-196">In addition to mixins, Sass also supports the concept of inheritance, allowing one class to extend another.</span></span> <span data-ttu-id="1627d-197">Il est conceptuellement semblable à un mixin, mais entraîne moins de code CSS.</span><span class="sxs-lookup"><span data-stu-id="1627d-197">It's conceptually similar to a mixin, but results in less CSS code.</span></span> <span data-ttu-id="1627d-198">Il est réalisé en utilisant la `@extend` (mot clé).</span><span class="sxs-lookup"><span data-stu-id="1627d-198">It's accomplished using the `@extend` keyword.</span></span> <span data-ttu-id="1627d-199">Pour tester mixins, ajoutez le code suivant à votre *main2.scss* fichier :</span><span class="sxs-lookup"><span data-stu-id="1627d-199">To try out mixins, add the following to your *main2.scss* file:</span></span>

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

<span data-ttu-id="1627d-200">Examinez la sortie dans *main2.css* après l’exécution de la `sass` de tâches dans **Task Runner Explorer**:</span><span class="sxs-lookup"><span data-stu-id="1627d-200">Examine the output in *main2.css* after running the `sass` task in **Task Runner Explorer**:</span></span>

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

<span data-ttu-id="1627d-201">Notez que toutes les propriétés courantes de l’alerte mixin sont répétés dans chaque classe.</span><span class="sxs-lookup"><span data-stu-id="1627d-201">Notice that all of the common properties of the alert mixin are repeated in each class.</span></span> <span data-ttu-id="1627d-202">Le mixin fait un bon de travail de la déduplication au moment du développement, mais il est toujours créant un fichier CSS avec un grand nombre de duplication, entraînant plus grands que les fichiers CSS nécessaires - un problème de performances.</span><span class="sxs-lookup"><span data-stu-id="1627d-202">The mixin did a good job of helping eliminate duplication at development time, but it's still creating CSS with a lot of duplication in it, resulting in larger than necessary CSS files - a potential performance issue.</span></span>

<span data-ttu-id="1627d-203">Maintenant remplacer l’alerte mixin avec un `.alert` classe et remplacez `@include` à `@extend` (sans oublier d’étendre `.alert`, et non `alert`) :</span><span class="sxs-lookup"><span data-stu-id="1627d-203">Now replace the alert mixin with a `.alert` class, and change `@include` to `@extend` (remembering to extend `.alert`, not `alert`):</span></span>

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

<span data-ttu-id="1627d-204">Exécuter une fois de plus de Sass et examinez le code CSS qui en résulte :</span><span class="sxs-lookup"><span data-stu-id="1627d-204">Run Sass once more, and examine the resulting CSS:</span></span>

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

<span data-ttu-id="1627d-205">Désormais, les propriétés sont définies uniquement autant que nécessaire, et mieux CSS est généré.</span><span class="sxs-lookup"><span data-stu-id="1627d-205">Now the properties are defined only as many times as needed, and better CSS is generated.</span></span>

<span data-ttu-id="1627d-206">Sass inclut également des fonctions et les opérations de la logique conditionnelle, similaires à une valeur inférieure.</span><span class="sxs-lookup"><span data-stu-id="1627d-206">Sass also includes functions and conditional logic operations, similar to Less.</span></span> <span data-ttu-id="1627d-207">En fait, les fonctionnalités de ces deux langages sont très similaires.</span><span class="sxs-lookup"><span data-stu-id="1627d-207">In fact, the two languages' capabilities are very similar.</span></span>

## <a name="less-or-sass"></a><span data-ttu-id="1627d-208">Inférieur ou Sass ?</span><span class="sxs-lookup"><span data-stu-id="1627d-208">Less or Sass?</span></span>

<span data-ttu-id="1627d-209">Il n’existe encore aucune consensus que s’il s’agit en général préférable d’utiliser inférieur et Sass (ou même s’il convient de préférer les Sass d’origine ou la nouvelle syntaxe SCSS dans Sass).</span><span class="sxs-lookup"><span data-stu-id="1627d-209">There's still no consensus as to whether it's generally better to use Less or Sass (or even whether to prefer the original Sass or the newer SCSS syntax within Sass).</span></span> <span data-ttu-id="1627d-210">La décision la plus importante consiste probablement à **utiliser un de ces outils**, par opposition à simplement à codage manuel vos fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="1627d-210">Probably the most important decision is to **use one of these tools**, as opposed to just hand-coding your CSS files.</span></span> <span data-ttu-id="1627d-211">Une fois que vous avez apportées qui inférieurs, de décision et Sass sont de bons choix.</span><span class="sxs-lookup"><span data-stu-id="1627d-211">Once you've made that decision, both Less and Sass are good choices.</span></span>

## <a name="font-awesome"></a><span data-ttu-id="1627d-212">Police impressionnant</span><span class="sxs-lookup"><span data-stu-id="1627d-212">Font Awesome</span></span>

<span data-ttu-id="1627d-213">Outre les Préprocesseurs CSS, une excellente ressource pour les applications de style web moderne est impressionnant de police.</span><span class="sxs-lookup"><span data-stu-id="1627d-213">In addition to CSS preprocessors, another great resource for styling modern web applications is Font Awesome.</span></span> <span data-ttu-id="1627d-214">Police Awesome est un kit de ressources qui fournit plus de 500 icônes vecteur évolutive qui peuvent être utilisés librement dans vos applications web.</span><span class="sxs-lookup"><span data-stu-id="1627d-214">Font Awesome is a toolkit that provides over 500 scalable vector icons that can be freely used in your web applications.</span></span> <span data-ttu-id="1627d-215">Il a été conçu pour fonctionner avec les données d’amorçage, mais il n’a aucune dépendance sur cette infrastructure ou sur toutes les bibliothèques JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1627d-215">It was originally designed to work with Bootstrap, but it has no dependency on that framework or on any JavaScript libraries.</span></span>

<span data-ttu-id="1627d-216">Prise en main impressionnant de police, la plus simple consiste à ajouter une référence à celui-ci, à l’aide de son emplacement réseau (CDN) de diffusion de contenu public :</span><span class="sxs-lookup"><span data-stu-id="1627d-216">The easiest way to get started with Font Awesome is to add a reference to it, using its public content delivery network (CDN) location:</span></span>

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

<span data-ttu-id="1627d-217">Vous pouvez également l’ajouter à votre projet Visual Studio en l’ajoutant aux section « dépendances » dans *bower.json*:</span><span class="sxs-lookup"><span data-stu-id="1627d-217">You can also add it to your Visual Studio project by adding it to the "dependencies" in *bower.json*:</span></span>

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

<span data-ttu-id="1627d-218">Une fois que vous avez une référence à la police impressionnant sur une page, vous pouvez ajouter des icônes à votre application en appliquant des classes police impressionnant, généralement précédés de « fa- », à vos éléments HTML inline (tel que `<span>` ou `<i>`).</span><span class="sxs-lookup"><span data-stu-id="1627d-218">Once you have a reference to Font Awesome on a page, you can add icons to your application by applying Font Awesome classes, typically prefixed with "fa-", to your inline HTML elements (such as `<span>` or `<i>`).</span></span>  <span data-ttu-id="1627d-219">Par exemple, vous pouvez ajouter des icônes aux listes simples et des menus à l’aide de code similaire à celui-ci :</span><span class="sxs-lookup"><span data-stu-id="1627d-219">For example, you can add icons to simple lists and menus using code like this:</span></span>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

<span data-ttu-id="1627d-220">Cela génère les éléments suivants dans le navigateur, remarquez l’icône en regard de chaque élément :</span><span class="sxs-lookup"><span data-stu-id="1627d-220">This produces the following in the browser - note the icon beside each item:</span></span>

![icônes de la liste](less-sass-fa/_static/list-icons-screenshot.png)

<span data-ttu-id="1627d-222">Vous pouvez afficher la liste complète des icônes disponibles ici :</span><span class="sxs-lookup"><span data-stu-id="1627d-222">You can view a complete list of the available icons here:</span></span>

http://fontawesome.io/icons/

## <a name="summary"></a><span data-ttu-id="1627d-223">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="1627d-223">Summary</span></span>

<span data-ttu-id="1627d-224">Les applications web modernes exigent plus en plus réactives, fluide conceptions qui sont propres, intuitive et facile à utiliser à partir de différents périphériques.</span><span class="sxs-lookup"><span data-stu-id="1627d-224">Modern web applications increasingly demand responsive, fluid designs that are clean, intuitive, and easy to use from a variety of devices.</span></span> <span data-ttu-id="1627d-225">Gérer la complexité des feuilles de style CSS requis pour atteindre ces objectifs est mieux effectuée à l’aide d’un type de préprocesseur inférieur ou Sass.</span><span class="sxs-lookup"><span data-stu-id="1627d-225">Managing the complexity of the CSS stylesheets required to achieve these goals is best done using a preprocessor like Less or Sass.</span></span> <span data-ttu-id="1627d-226">En outre, les kits comme police impressionnant fournissent rapidement des icônes bien connus pour les menus de navigation textuelle et expérience des boutons, l’amélioration de l’utilisateur globale de votre application.</span><span class="sxs-lookup"><span data-stu-id="1627d-226">In addition, toolkits like Font Awesome quickly provide well-known icons to textual navigation menus and buttons, improving the overall user experience of your application.</span></span>
