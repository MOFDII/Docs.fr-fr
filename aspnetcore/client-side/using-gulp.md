---
title: Utilisation de Gulp dans ASP.NET Core
author: rick-anderson
description: "Découvrez comment utiliser Gulp dans ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-gulp
ms.openlocfilehash: 0a3443e8187d46992f55dc537d0f400c6771c50c
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/02/2018
---
# <a name="introduction-to-using-gulp-in-aspnet-core"></a><span data-ttu-id="2b46a-103">Introduction à l’utilisation de Gulp dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2b46a-103">Introduction to using Gulp in ASP.NET Core</span></span> 

<span data-ttu-id="2b46a-104">Par [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Michel Roth](https://github.com/danroth27), et [Shayne Boyer](https://twitter.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="2b46a-104">By [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), and [Shayne Boyer](https://twitter.com/spboyer)</span></span>

<span data-ttu-id="2b46a-105">Dans une application web moderne classique, le processus de génération peut :</span><span class="sxs-lookup"><span data-stu-id="2b46a-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="2b46a-106">Regroupement et minimiser les fichiers JavaScript et CSS.</span><span class="sxs-lookup"><span data-stu-id="2b46a-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="2b46a-107">Exécuter les outils pour appeler les tâches de regroupement et de minimisation avant chaque build.</span><span class="sxs-lookup"><span data-stu-id="2b46a-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="2b46a-108">Compilez inférieur ou SASS fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="2b46a-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="2b46a-109">Compiler des fichiers JavaScript CoffeeScript ou TypeScript.</span><span class="sxs-lookup"><span data-stu-id="2b46a-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="2b46a-110">A *exécuteur de tâches* est un outil qui automatise les tâches de développement de routine et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="2b46a-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="2b46a-111">Visual Studio fournit la prise en charge intégrée pour deux exécuteurs de tâches de base de JavaScript populaires : [Gulp](https://gulpjs.com/) et [Grunt](using-grunt.md).</span><span class="sxs-lookup"><span data-stu-id="2b46a-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="2b46a-112">gulp</span><span class="sxs-lookup"><span data-stu-id="2b46a-112">Gulp</span></span>

<span data-ttu-id="2b46a-113">Gulp est un toolkit basées JavaScript de build en continu pour le code côté client.</span><span class="sxs-lookup"><span data-stu-id="2b46a-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="2b46a-114">Il est couramment utilisé pour diffuser des fichiers côté client via une série de processus de déclenchement d’un événement spécifique dans un environnement de génération.</span><span class="sxs-lookup"><span data-stu-id="2b46a-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="2b46a-115">Par exemple, Gulp permettre être utilisé pour automatiser [groupement et la minimisation](bundling-and-minification.md) ou le nettoyage d’un environnement de développement avant une nouvelle build.</span><span class="sxs-lookup"><span data-stu-id="2b46a-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleansing of a development environment before a new build.</span></span>

<span data-ttu-id="2b46a-116">Un ensemble de tâches de Gulp est défini dans *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="2b46a-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="2b46a-117">Le code JavaScript suivant inclut les modules Gulp et spécifie les chemins d’accès des fichiers référencés dans les tâches à venir :</span><span class="sxs-lookup"><span data-stu-id="2b46a-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

```javascript
/// <binding Clean='clean' />
"use strict";

var gulp = require("gulp"),
  rimraf = require("rimraf"),
  concat = require("gulp-concat"),
  cssmin = require("gulp-cssmin"),
  uglify = require("gulp-uglify");

var paths = {
  webroot: "./wwwroot/"
};

paths.js = paths.webroot + "js/**/*.js";
paths.minJs = paths.webroot + "js/**/*.min.js";
paths.css = paths.webroot + "css/**/*.css";
paths.minCss = paths.webroot + "css/**/*.min.css";
paths.concatJsDest = paths.webroot + "js/site.min.js";
paths.concatCssDest = paths.webroot + "css/site.min.css";
```

<span data-ttu-id="2b46a-118">Le code ci-dessus spécifie quels modules de nœud sont requises.</span><span class="sxs-lookup"><span data-stu-id="2b46a-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="2b46a-119">Le `require` fonction importe chaque module afin que les tâches dépendantes peuvent utiliser leurs fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="2b46a-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="2b46a-120">Chaque module importé est assigné à une variable.</span><span class="sxs-lookup"><span data-stu-id="2b46a-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="2b46a-121">Les modules peuvent être situés par nom ou chemin d’accès.</span><span class="sxs-lookup"><span data-stu-id="2b46a-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="2b46a-122">Dans cet exemple, les modules nommé `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, et `gulp-uglify` sont récupérés par nom.</span><span class="sxs-lookup"><span data-stu-id="2b46a-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="2b46a-123">En outre, une série de chemins d’accès sont créés afin que les emplacements des fichiers CSS et JavaScript peuvent être réutilisées et référencées dans les tâches.</span><span class="sxs-lookup"><span data-stu-id="2b46a-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="2b46a-124">Le tableau suivant fournit des descriptions des modules inclus dans *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="2b46a-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

| <span data-ttu-id="2b46a-125">Nom du module</span><span class="sxs-lookup"><span data-stu-id="2b46a-125">Module Name</span></span> | <span data-ttu-id="2b46a-126">Description</span><span class="sxs-lookup"><span data-stu-id="2b46a-126">Description</span></span> |
| ----------- | ----------- |
| <span data-ttu-id="2b46a-127">gulp</span><span class="sxs-lookup"><span data-stu-id="2b46a-127">gulp</span></span>        | <span data-ttu-id="2b46a-128">Gulp de diffusion en continu de système de génération.</span><span class="sxs-lookup"><span data-stu-id="2b46a-128">The Gulp streaming build system.</span></span> <span data-ttu-id="2b46a-129">Pour plus d’informations, consultez [gulp](https://www.npmjs.com/package/gulp).</span><span class="sxs-lookup"><span data-stu-id="2b46a-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span> |
| <span data-ttu-id="2b46a-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="2b46a-130">rimraf</span></span>      | <span data-ttu-id="2b46a-131">Un module de suppression de nœud.</span><span class="sxs-lookup"><span data-stu-id="2b46a-131">A Node deletion module.</span></span> <span data-ttu-id="2b46a-132">Pour plus d’informations, consultez [rimraf](https://www.npmjs.com/package/rimraf).</span><span class="sxs-lookup"><span data-stu-id="2b46a-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span> |
| <span data-ttu-id="2b46a-133">gulp-concat</span><span class="sxs-lookup"><span data-stu-id="2b46a-133">gulp-concat</span></span> | <span data-ttu-id="2b46a-134">Un module qui concatène des fichiers en fonction de caractère de saut de ligne du système d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="2b46a-134">A module that concatenates files based on the operating system's newline character.</span></span> <span data-ttu-id="2b46a-135">Pour plus d’informations, consultez [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span><span class="sxs-lookup"><span data-stu-id="2b46a-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span> |
| <span data-ttu-id="2b46a-136">gulp-cssmin</span><span class="sxs-lookup"><span data-stu-id="2b46a-136">gulp-cssmin</span></span> | <span data-ttu-id="2b46a-137">Un module qui minimise les fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="2b46a-137">A module that minifies CSS files.</span></span> <span data-ttu-id="2b46a-138">Pour plus d’informations, consultez [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span><span class="sxs-lookup"><span data-stu-id="2b46a-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span> |
| <span data-ttu-id="2b46a-139">gulp-uglify</span><span class="sxs-lookup"><span data-stu-id="2b46a-139">gulp-uglify</span></span> | <span data-ttu-id="2b46a-140">Un module qui minimise *.js* fichiers.</span><span class="sxs-lookup"><span data-stu-id="2b46a-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="2b46a-141">Pour plus d’informations, consultez [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span><span class="sxs-lookup"><span data-stu-id="2b46a-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span> |

<span data-ttu-id="2b46a-142">Une fois que les modules requis sont importés, les tâches peuvent être spécifiés.</span><span class="sxs-lookup"><span data-stu-id="2b46a-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="2b46a-143">Ici, il existe six tâches inscrit, représenté par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="2b46a-143">Here there are six tasks registered, represented by the following code:</span></span>

```javascript
gulp.task("clean:js", function (cb) {
  rimraf(paths.concatJsDest, cb);
});

gulp.task("clean:css", function (cb) {
  rimraf(paths.concatCssDest, cb);
});

gulp.task("clean", ["clean:js", "clean:css"]);

gulp.task("min:js", function () {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", function () {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", ["min:js", "min:css"]);
```

<span data-ttu-id="2b46a-144">Le tableau suivant fournit une explication des tâches spécifié dans le code ci-dessus :</span><span class="sxs-lookup"><span data-stu-id="2b46a-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="2b46a-145">Nom de la tâche</span><span class="sxs-lookup"><span data-stu-id="2b46a-145">Task Name</span></span>|<span data-ttu-id="2b46a-146">Description</span><span class="sxs-lookup"><span data-stu-id="2b46a-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="2b46a-147">nettoyer : js</span><span class="sxs-lookup"><span data-stu-id="2b46a-147">clean:js</span></span>|<span data-ttu-id="2b46a-148">Une tâche qui utilise le module de suppression de nœud rimraf pour supprimer la version réduite du fichier site.js.</span><span class="sxs-lookup"><span data-stu-id="2b46a-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="2b46a-149">nettoyer : css</span><span class="sxs-lookup"><span data-stu-id="2b46a-149">clean:css</span></span>|<span data-ttu-id="2b46a-150">Une tâche qui utilise le module de suppression de nœud rimraf pour supprimer la version réduite du fichier site.css.</span><span class="sxs-lookup"><span data-stu-id="2b46a-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="2b46a-151">Nettoyer</span><span class="sxs-lookup"><span data-stu-id="2b46a-151">clean</span></span>|<span data-ttu-id="2b46a-152">Une tâche qui appelle le `clean:js` tâche, suivi par le `clean:css` tâche.</span><span class="sxs-lookup"><span data-stu-id="2b46a-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="2b46a-153">min:js</span><span class="sxs-lookup"><span data-stu-id="2b46a-153">min:js</span></span>|<span data-ttu-id="2b46a-154">Une tâche qui minimise et concatène tous les fichiers .js dans le dossier js.</span><span class="sxs-lookup"><span data-stu-id="2b46a-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="2b46a-155">Le. les fichiers min.js sont exclus.</span><span class="sxs-lookup"><span data-stu-id="2b46a-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="2b46a-156">min:CSS</span><span class="sxs-lookup"><span data-stu-id="2b46a-156">min:css</span></span>|<span data-ttu-id="2b46a-157">Une tâche qui minimise et concatène tous les fichiers .css dans le dossier css.</span><span class="sxs-lookup"><span data-stu-id="2b46a-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="2b46a-158">Le. les fichiers min.css sont exclus.</span><span class="sxs-lookup"><span data-stu-id="2b46a-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="2b46a-159">min</span><span class="sxs-lookup"><span data-stu-id="2b46a-159">min</span></span>|<span data-ttu-id="2b46a-160">Une tâche qui appelle le `min:js` tâche, suivi par le `min:css` tâche.</span><span class="sxs-lookup"><span data-stu-id="2b46a-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="2b46a-161">Exécution des tâches par défaut</span><span class="sxs-lookup"><span data-stu-id="2b46a-161">Running default tasks</span></span>

<span data-ttu-id="2b46a-162">Si vous n’avez pas déjà créé une application Web, créez un nouveau projet d’Application Web ASP.NET dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2b46a-162">If you haven't already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1.  <span data-ttu-id="2b46a-163">Ajouter un nouveau fichier JavaScript à votre projet et nommez-le *gulpfile.js*, puis copiez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="2b46a-163">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    var gulp = require("gulp"),
      rimraf = require("rimraf"),
      concat = require("gulp-concat"),
      cssmin = require("gulp-cssmin"),
      uglify = require("gulp-uglify");
    
    var paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", function (cb) {
      rimraf(paths.concatJsDest, cb);
    });
    
    gulp.task("clean:css", function (cb) {
      rimraf(paths.concatCssDest, cb);
    });
    
    gulp.task("clean", ["clean:js", "clean:css"]);
    
    gulp.task("min:js", function () {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min:css", function () {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min", ["min:js", "min:css"]);
    ```

2.  <span data-ttu-id="2b46a-164">Ouvrez le *package.json* fichier (ajoutez si ce n’est pas il) et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="2b46a-164">Open the *package.json* file (add if not there) and add the following.</span></span>

    ```json
    {
      "devDependencies": {
        "gulp": "3.9.1",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.1.7",
        "gulp-uglify": "2.0.1",
        "rimraf": "2.6.1"
      }
    }
    ```

3.  <span data-ttu-id="2b46a-165">Dans **l’Explorateur de solutions**, avec le bouton droit *gulpfile.js*, puis sélectionnez **Task Runner Explorer**.</span><span class="sxs-lookup"><span data-stu-id="2b46a-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>
    
    ![Ouvrez l’Explorateur d’exécuteur de tâche à partir de l’Explorateur de solutions](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    <span data-ttu-id="2b46a-167">**Explorateur d’exécuteur de tâches** affiche la liste des tâches de Gulp.</span><span class="sxs-lookup"><span data-stu-id="2b46a-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="2b46a-168">(Vous devrez peut-être cliquez sur le **Actualiser** bouton qui apparaît à gauche du nom du projet.)</span><span class="sxs-lookup"><span data-stu-id="2b46a-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>
    
    ![Explorateur d’exécuteur de tâche](using-gulp/_static/03-TaskRunnerExplorer.png)

4.  <span data-ttu-id="2b46a-170">Sous **tâches** dans **Task Runner Explorer**, avec le bouton droit **propre**, puis sélectionnez **exécuter** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="2b46a-170">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![Tâche de nettoyage d’Explorateur d’exécuteur de tâche](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="2b46a-172">**Explorateur d’exécuteur de tâches** créera un nouvel onglet nommé **propre** et exécuter la tâche clean telle qu’elle est définie dans *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="2b46a-172">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5.  <span data-ttu-id="2b46a-173">Avec le bouton droit le **propre** de tâches, puis sélectionnez **liaisons** > **avant de générer**.</span><span class="sxs-lookup"><span data-stu-id="2b46a-173">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![Liaison BeforeBuild Task Runner Explorer](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="2b46a-175">Le **avant de générer** liaison configure la tâche clean à s’exécuter automatiquement avant chaque génération du projet.</span><span class="sxs-lookup"><span data-stu-id="2b46a-175">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="2b46a-176">Les liaisons défini avec **Task Runner Explorer** sont stockés sous la forme d’un commentaire en haut de votre *gulpfile.js* et sont effectifs seulement dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2b46a-176">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="2b46a-177">Une alternative qui ne nécessite pas Visual Studio consiste à configurer l’exécution automatique des tâches de gulp dans votre *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="2b46a-177">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="2b46a-178">Par exemple, placez cela votre *.csproj* fichier :</span><span class="sxs-lookup"><span data-stu-id="2b46a-178">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="2b46a-179">Maintenant la tâche de nettoyage est exécutée lorsque vous exécutez le projet dans Visual Studio ou à partir d’une invite de commandes à l’aide de la [dotnet exécuter](/dotnet/core/tools/dotnet-run) commande (exécuter `npm install` premier).</span><span class="sxs-lookup"><span data-stu-id="2b46a-179">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the [dotnet run](/dotnet/core/tools/dotnet-run) command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="2b46a-180">Définition et une nouvelle tâche en cours d’exécution</span><span class="sxs-lookup"><span data-stu-id="2b46a-180">Defining and running a new task</span></span>

<span data-ttu-id="2b46a-181">Pour définir une nouvelle tâche de choses, modifier *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="2b46a-181">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1.  <span data-ttu-id="2b46a-182">Ajoutez le code JavaScript suivant à la fin de *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="2b46a-182">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    <span data-ttu-id="2b46a-183">Cette tâche est nommée `first`, et il affiche simplement une chaîne.</span><span class="sxs-lookup"><span data-stu-id="2b46a-183">This task is named `first`, and it simply displays a string.</span></span>

2.  <span data-ttu-id="2b46a-184">Enregistrer *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="2b46a-184">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="2b46a-185">Dans **l’Explorateur de solutions**, avec le bouton droit *gulpfile.js*, puis sélectionnez *Task Runner Explorer*.</span><span class="sxs-lookup"><span data-stu-id="2b46a-185">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4.  <span data-ttu-id="2b46a-186">Dans **Task Runner Explorer**, avec le bouton droit **première**, puis sélectionnez **exécuter**.</span><span class="sxs-lookup"><span data-stu-id="2b46a-186">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![Exécutez la première tâche Task Runner Explorer](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="2b46a-188">Vous verrez que le texte de sortie s’affiche.</span><span class="sxs-lookup"><span data-stu-id="2b46a-188">You’ll see that the output text is displayed.</span></span> <span data-ttu-id="2b46a-189">Si vous êtes intéressé par exemples basés sur un scénario courant, consultez les recettes de Gulp.</span><span class="sxs-lookup"><span data-stu-id="2b46a-189">If you are interested in examples based on a common scenario, see Gulp Recipes.</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="2b46a-190">Définition et l’exécution de tâches dans une série</span><span class="sxs-lookup"><span data-stu-id="2b46a-190">Defining and running tasks in a series</span></span>

<span data-ttu-id="2b46a-191">Lorsque vous exécutez plusieurs tâches, les tâches exécutées simultanément par défaut.</span><span class="sxs-lookup"><span data-stu-id="2b46a-191">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="2b46a-192">Toutefois, si vous avez besoin exécuter des tâches dans un ordre spécifique, vous devez spécifier que chaque tâche est terminé, ainsi que les tâches qui dépendent de l’achèvement d’une autre tâche.</span><span class="sxs-lookup"><span data-stu-id="2b46a-192">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1.  <span data-ttu-id="2b46a-193">Pour définir une série de tâches à exécuter dans l’ordre, remplacez le `first` les tâches que vous avez ajouté ci-dessus *gulpfile.js* avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="2b46a-193">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    <span data-ttu-id="2b46a-194">Vous disposez maintenant de trois tâches : `series:first`, `series:second`, et `series`.</span><span class="sxs-lookup"><span data-stu-id="2b46a-194">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="2b46a-195">Le `series:second` tâche inclut un deuxième paramètre qui spécifie un tableau de tâches à exécuter et sont terminés avant du `series:second` tâche exécutera.</span><span class="sxs-lookup"><span data-stu-id="2b46a-195">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="2b46a-196">Comme indiqué dans le code ci-dessus, seule la `series:first` tâche doit être réalisée avant le `series:second` tâche exécutera.</span><span class="sxs-lookup"><span data-stu-id="2b46a-196">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2.  <span data-ttu-id="2b46a-197">Enregistrer *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="2b46a-197">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="2b46a-198">Dans **l’Explorateur de solutions**, avec le bouton droit *gulpfile.js* et sélectionnez **Task Runner Explorer** si elle n’est pas déjà ouverte.</span><span class="sxs-lookup"><span data-stu-id="2b46a-198">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4.  <span data-ttu-id="2b46a-199">Dans **Task Runner Explorer**, avec le bouton droit **série** et sélectionnez **exécuter**.</span><span class="sxs-lookup"><span data-stu-id="2b46a-199">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Explorateur d’exécuteur de tâches exécuter la tâche de série](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="2b46a-201">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="2b46a-201">IntelliSense</span></span>

<span data-ttu-id="2b46a-202">IntelliSense fournit l’exécution de code, des descriptions de paramètre et d’autres fonctionnalités pour améliorer la productivité et réduire les erreurs.</span><span class="sxs-lookup"><span data-stu-id="2b46a-202">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="2b46a-203">Les tâches de gulp sont écrites en JavaScript ; Par conséquent, IntelliSense peut fournir une assistance lors du développement.</span><span class="sxs-lookup"><span data-stu-id="2b46a-203">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="2b46a-204">Lorsque vous travaillez avec JavaScript, IntelliSense répertorie les objets, les fonctions, les propriétés et paramètres qui sont disponibles en fonction de votre contexte actuel.</span><span class="sxs-lookup"><span data-stu-id="2b46a-204">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="2b46a-205">Sélectionnez une option de programmation dans la liste contextuelle fournie par IntelliSense pour compléter le code.</span><span class="sxs-lookup"><span data-stu-id="2b46a-205">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="2b46a-207">Pour plus d’informations sur IntelliSense, consultez [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).</span><span class="sxs-lookup"><span data-stu-id="2b46a-207">For more information about IntelliSense, see [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="2b46a-208">Environnements de développement, intermédiaire et de production</span><span class="sxs-lookup"><span data-stu-id="2b46a-208">Development, staging, and production environments</span></span>

<span data-ttu-id="2b46a-209">Lorsque Gulp est utilisé pour optimiser les fichiers côté client pour intermédiaire et de production, les fichiers traités sont enregistrés dans un emplacement intermédiaire et de production local.</span><span class="sxs-lookup"><span data-stu-id="2b46a-209">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="2b46a-210">Le *_Layout.cshtml* fichier utilise le **environnement** balise d’assistance pour fournir deux versions différentes de fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="2b46a-210">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="2b46a-211">Une version des fichiers CSS est pour le développement et l’autre version est optimisée pour la préparation et de production.</span><span class="sxs-lookup"><span data-stu-id="2b46a-211">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="2b46a-212">Dans Visual Studio 2017, lorsque vous modifiez le **ASPNETCORE_ENVIRONMENT** variable d’environnement `Production`, Visual Studio génère l’application Web et un lien vers les fichiers CSS sous forme réduites.</span><span class="sxs-lookup"><span data-stu-id="2b46a-212">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="2b46a-213">La balise suivante montre la **environnement** programmes d’assistance qui contient des balises de liens à la balise la `Development` CSS le réduite et fichiers `Staging, Production` fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="2b46a-213">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery"
            crossorigin="anonymous"
            integrity="sha384-K+ctZQ+LL8q6tP7I94W+qzQsfRV2a+AfHIi9k8z8l9ggpc8X+Ytst4yBo/hH+8Fk">
    </script>
    <script src="https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js"
            asp-fallback-src="~/lib/bootstrap/dist/js/bootstrap.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.fn && window.jQuery.fn.modal"
            crossorigin="anonymous"
            integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa">
    </script>
    <script src="~/js/site.min.js" asp-append-version="true"></script>
</environment>
```

## <a name="switching-between-environments"></a><span data-ttu-id="2b46a-214">Basculer entre les environnements</span><span class="sxs-lookup"><span data-stu-id="2b46a-214">Switching between environments</span></span>

<span data-ttu-id="2b46a-215">Pour basculer entre la compilation pour des environnements différents, vous devez modifier le **ASPNETCORE_ENVIRONMENT** valeur de la variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="2b46a-215">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1.  <span data-ttu-id="2b46a-216">Dans **Task Runner Explorer**, vérifiez que le **min** tâche a été défini pour exécuter **avant de générer**.</span><span class="sxs-lookup"><span data-stu-id="2b46a-216">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2.  <span data-ttu-id="2b46a-217">Dans **l’Explorateur de solutions**, cliquez sur le nom du projet et sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="2b46a-217">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="2b46a-218">La feuille de propriétés de l’application Web s’affiche.</span><span class="sxs-lookup"><span data-stu-id="2b46a-218">The property sheet for the Web app is displayed.</span></span>

3.  <span data-ttu-id="2b46a-219">Cliquez sur l’onglet **Déboguer**.</span><span class="sxs-lookup"><span data-stu-id="2b46a-219">Click the **Debug** tab.</span></span>

4.  <span data-ttu-id="2b46a-220">Définir la valeur de la **: environnement d’hébergement** variable d’environnement `Production`.</span><span class="sxs-lookup"><span data-stu-id="2b46a-220">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5.  <span data-ttu-id="2b46a-221">Appuyez sur **F5** pour exécuter l’application dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="2b46a-221">Press **F5** to run the application in a browser.</span></span>

6.  <span data-ttu-id="2b46a-222">Dans la fenêtre du navigateur, avec le bouton droit de la page, puis sélectionnez **afficher la Source** pour afficher le code HTML de la page.</span><span class="sxs-lookup"><span data-stu-id="2b46a-222">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="2b46a-223">Notez que les liens de la feuille de style pointent vers les fichiers CSS réduites.</span><span class="sxs-lookup"><span data-stu-id="2b46a-223">Notice that the stylesheet links point to the minified CSS files.</span></span>

7.  <span data-ttu-id="2b46a-224">Fermez le navigateur pour arrêter l’application Web.</span><span class="sxs-lookup"><span data-stu-id="2b46a-224">Close the browser to stop the Web app.</span></span>

8.  <span data-ttu-id="2b46a-225">Dans Visual Studio, revenir à la feuille de propriétés de l’application Web et modifier la **: environnement d’hébergement** variable d’environnement pour revenir `Development`.</span><span class="sxs-lookup"><span data-stu-id="2b46a-225">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9.  <span data-ttu-id="2b46a-226">Appuyez sur **F5** réexécuter l’application dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="2b46a-226">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="2b46a-227">Dans la fenêtre du navigateur, avec le bouton droit de la page, puis sélectionnez **afficher la Source** pour afficher le code HTML de la page.</span><span class="sxs-lookup"><span data-stu-id="2b46a-227">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="2b46a-228">Notez que les liens de la feuille de style pointent vers les versions unminified des fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="2b46a-228">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="2b46a-229">Pour plus d’informations sur les environnements dans ASP.NET Core, consultez [fonctionne avec plusieurs environnements](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="2b46a-229">For more information related to environments in ASP.NET Core, see [Working with Multiple Environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="2b46a-230">Détails des tâches et de module</span><span class="sxs-lookup"><span data-stu-id="2b46a-230">Task and module details</span></span>

<span data-ttu-id="2b46a-231">Une tâche Gulp est inscrit avec un nom de fonction.</span><span class="sxs-lookup"><span data-stu-id="2b46a-231">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="2b46a-232">Vous pouvez spécifier des dépendances si d’autres tâches doivent être exécutées avant que la tâche actuelle.</span><span class="sxs-lookup"><span data-stu-id="2b46a-232">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="2b46a-233">Les fonctions supplémentaires permettent à exécuter et surveiller les tâches de choses, ainsi que de définir la source (*src*) et de destination (*dest*) des fichiers en cours de modification.</span><span class="sxs-lookup"><span data-stu-id="2b46a-233">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="2b46a-234">Les fonctions API de Gulp principales sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="2b46a-234">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="2b46a-235">Gulp (fonction)</span><span class="sxs-lookup"><span data-stu-id="2b46a-235">Gulp Function</span></span>|<span data-ttu-id="2b46a-236">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="2b46a-236">Syntax</span></span>|<span data-ttu-id="2b46a-237">Description</span><span class="sxs-lookup"><span data-stu-id="2b46a-237">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="2b46a-238">task</span><span class="sxs-lookup"><span data-stu-id="2b46a-238">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="2b46a-239">Le `task` fonction crée une tâche.</span><span class="sxs-lookup"><span data-stu-id="2b46a-239">The `task` function creates a task.</span></span> <span data-ttu-id="2b46a-240">Le `name` paramètre définit le nom de la tâche.</span><span class="sxs-lookup"><span data-stu-id="2b46a-240">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="2b46a-241">Le `deps` paramètre contient un tableau de tâches à effectuer avant d’exécuter cette tâche.</span><span class="sxs-lookup"><span data-stu-id="2b46a-241">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="2b46a-242">Le `fn` paramètre représente une fonction de rappel qui effectue les opérations de la tâche.</span><span class="sxs-lookup"><span data-stu-id="2b46a-242">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="2b46a-243">Espion</span><span class="sxs-lookup"><span data-stu-id="2b46a-243">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="2b46a-244">Le `watch` fonction surveille les fichiers et exécute des tâches lorsqu’une modification de fichier se produit.</span><span class="sxs-lookup"><span data-stu-id="2b46a-244">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="2b46a-245">Le `glob` paramètre est un `string` ou `array` qui détermine les fichiers à surveiller.</span><span class="sxs-lookup"><span data-stu-id="2b46a-245">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="2b46a-246">Le `opts` paramètre fournit la surveillance des options de fichiers supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="2b46a-246">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="2b46a-247">src</span><span class="sxs-lookup"><span data-stu-id="2b46a-247">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="2b46a-248">Le `src` fonction fournit des fichiers qui correspondent aux valeurs glob.</span><span class="sxs-lookup"><span data-stu-id="2b46a-248">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="2b46a-249">Le `glob` paramètre est un `string` ou `array` qui détermine les fichiers à lire.</span><span class="sxs-lookup"><span data-stu-id="2b46a-249">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="2b46a-250">Le `options` paramètre fournit des options de fichiers supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="2b46a-250">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="2b46a-251">destination</span><span class="sxs-lookup"><span data-stu-id="2b46a-251">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="2b46a-252">Le `dest` fonction définit un emplacement dans lequel les fichiers peuvent être écrites.</span><span class="sxs-lookup"><span data-stu-id="2b46a-252">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="2b46a-253">Le `path` paramètre est une chaîne ou une fonction qui détermine le dossier de destination.</span><span class="sxs-lookup"><span data-stu-id="2b46a-253">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="2b46a-254">Le `options` paramètre est un objet qui spécifie les options de dossier de sortie.</span><span class="sxs-lookup"><span data-stu-id="2b46a-254">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="2b46a-255">Pour plus d’informations de référence API de Gulp supplémentaires, consultez [Gulp des API de documents](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span><span class="sxs-lookup"><span data-stu-id="2b46a-255">For additional Gulp API reference information, see [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="2b46a-256">Gulp recettes</span><span class="sxs-lookup"><span data-stu-id="2b46a-256">Gulp recipes</span></span>

<span data-ttu-id="2b46a-257">La Communauté Gulp fournit Gulp [recettes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span><span class="sxs-lookup"><span data-stu-id="2b46a-257">The Gulp community provides Gulp [recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="2b46a-258">Ces recettes sont constituées des tâches de Gulp pour résoudre des scénarios courants.</span><span class="sxs-lookup"><span data-stu-id="2b46a-258">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2b46a-259">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="2b46a-259">Additional resources</span></span>

* [<span data-ttu-id="2b46a-260">Documentation de gulp</span><span class="sxs-lookup"><span data-stu-id="2b46a-260">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="2b46a-261">Groupement et la minimisation dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2b46a-261">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="2b46a-262">À l’aide de Grunt dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2b46a-262">Using Grunt in ASP.NET Core</span></span>](using-grunt.md)
