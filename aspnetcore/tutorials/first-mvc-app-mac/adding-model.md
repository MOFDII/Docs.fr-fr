---
title: Ajouter un modèle à une application ASP.NET Core MVC avec Visual Studio pour Mac
author: rick-anderson
description: Ajoutez un modèle à une application ASP.NET Core simple.
ms.author: riande
ms.date: 09/22/2017
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 53d63cd554f6a3ec958f27ed35b0a30b1833f84c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276111"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app-with-visual-studio-for-mac"></a><span data-ttu-id="4e4c0-103">Ajouter un modèle à une application ASP.NET Core MVC avec Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="4e4c0-103">Add a model to an ASP.NET Core MVC app with Visual Studio for Mac</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="4e4c0-104">Cliquez avec le bouton droit sur le dossier *Models*, puis sélectionnez **Ajouter** > **Nouveau fichier**.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-104">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span> 
* <span data-ttu-id="4e4c0-105">Dans la boîte de dialogue **Nouveau fichier** :</span><span class="sxs-lookup"><span data-stu-id="4e4c0-105">In the **New File** dialog:</span></span>

  * <span data-ttu-id="4e4c0-106">Dans le volet gauche, sélectionnez **Général**.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-106">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="4e4c0-107">Dans le volet central, sélectionnez **Classe vide**.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-107">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="4e4c0-108">Nommez la classe **Movie**, puis sélectionnez **Nouveau**.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-108">Name the class **Movie** and select **New**.</span></span>

<span data-ttu-id="4e4c0-109">Ajoutez les propriétés suivantes à la classe `Movie` :</span><span class="sxs-lookup"><span data-stu-id="4e4c0-109">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="4e4c0-110">Le champ `ID` est nécessaire à la base de données pour la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-110">The `ID` field is required by the database for the primary key.</span></span>

<span data-ttu-id="4e4c0-111">Générez le projet pour vérifier qu’il ne comporte aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-111">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="4e4c0-112">Vous avez désormais un **M**odèle dans votre application **M**VC.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-112">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="4e4c0-113">Préparer le projet pour la génération de modèles automatique</span><span class="sxs-lookup"><span data-stu-id="4e4c0-113">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="4e4c0-114">Cliquez avec le bouton droit sur le fichier projet, puis sélectionnez **Outils > Modifier le fichier**.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-114">Right click on the project file, and then select **Tools > Edit File**.</span></span>

  ![affichage de l’étape ci-dessus](adding-model/_static/1.png)

- <span data-ttu-id="4e4c0-116">Ajoutez les packages NuGet en surbrillance suivants au fichier *MvcMovie.csproj* :</span><span class="sxs-lookup"><span data-stu-id="4e4c0-116">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
  [!code-csharp[](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="4e4c0-117">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-117">Save the file.</span></span>

- <span data-ttu-id="4e4c0-118">Créez un fichier *Models/MvcMovieContext.cs*, puis ajoutez la classe `MvcMovieContext` suivante : [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="4e4c0-118">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:  [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="4e4c0-119">Ouvrez le fichier *Startup.cs*, puis ajoutez deux instructions using : [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="4e4c0-119">Open the *Startup.cs* file and add two usings:  [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="4e4c0-120">Ajoutez le contexte de base de données au fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="4e4c0-120">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="4e4c0-121">Cela permet d’indiquer à Entity Framework quelles sont les classes de modèles incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-121">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="4e4c0-122">Vous définissez un *jeu d’entités* d’objets Movies, lesquels sont représentés dans la base de données sous forme de table Movie.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-122">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="4e4c0-123">Générez le projet pour vérifier qu’il n’existe aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-123">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="4e4c0-124">Effectuer une génération de modèles automatique pour MovieController</span><span class="sxs-lookup"><span data-stu-id="4e4c0-124">Scaffold the MovieController</span></span>

<span data-ttu-id="4e4c0-125">Ouvrez une fenêtre de terminal dans le dossier de projet, puis exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="4e4c0-125">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="4e4c0-126">Si vous obtenez l’erreur `No executable found matching command "dotnet-aspnet-codegenerator", verify` :</span><span class="sxs-lookup"><span data-stu-id="4e4c0-126">If you get the error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span></span>

 * <span data-ttu-id="4e4c0-127">Vous êtes dans le répertoire du projet.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-127">You are in the project directory.</span></span> <span data-ttu-id="4e4c0-128">Le répertoire du projet contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-128">The project directory has the *Program.cs*, *Startup.cs* and *.csproj* files.</span></span>
 * <span data-ttu-id="4e4c0-129">Votre version de dotnet est la 1.1 ou une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-129">Your dotnet version is 1.1 or higher.</span></span> <span data-ttu-id="4e4c0-130">Exécutez `dotnet` pour obtenir la version souhaitée.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-130">Run `dotnet` to get the version.</span></span>
 * <span data-ttu-id="4e4c0-131">Vous avez ajouté l’élément `<DotNetCliToolReference>` au [fichier MvcMovie.csproj](#prepare-the-project-for-scaffolding).</span><span class="sxs-lookup"><span data-stu-id="4e4c0-131">You have added the `<DotNetCliToolReference>` element to the [MvcMovie.csproj file](#prepare-the-project-for-scaffolding).</span></span>
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

<span data-ttu-id="4e4c0-132">Le moteur de génération de modèles automatique crée les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="4e4c0-132">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="4e4c0-133">contrôleur de films (*Controllers/MoviesController.cs*) ;</span><span class="sxs-lookup"><span data-stu-id="4e4c0-133">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="4e4c0-134">fichiers de vues Razor pour les pages Create, Delete, Details, Edit et Index (*Views/Movies/\*.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="4e4c0-134">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="4e4c0-135">La création automatique de méthodes d’action et de vues [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (créer, lire, mettre à jour et supprimer) s’appelle la *génération de modèles automatique*.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-135">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="4e4c0-136">Vous aurez bientôt une application web entièrement opérationnelle qui vous permettra de gérer une base de données de films.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-136">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

### <a name="add-the-files-to-visual-studio"></a><span data-ttu-id="4e4c0-137">Ajouter les fichiers à Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e4c0-137">Add the files to Visual Studio</span></span>

* <span data-ttu-id="4e4c0-138">Ajoutez le fichier *MovieController.cs* au projet Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="4e4c0-138">Add the *MovieController.cs* file to the Visual Studio project:</span></span>

  * <span data-ttu-id="4e4c0-139">Cliquez avec le bouton droit sur le dossier *Controllers*, puis sélectionnez **Ajouter > Ajouter des fichiers**.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-139">Right-click on the *Controllers* folder and select **Add > Add Files**.</span></span>
  * <span data-ttu-id="4e4c0-140">Sélectionnez le fichier *MovieController.cs*.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-140">Select the *MovieController.cs* file.</span></span>

* <span data-ttu-id="4e4c0-141">Ajoutez le dossier *Movies* et les vues :</span><span class="sxs-lookup"><span data-stu-id="4e4c0-141">Add the *Movies* folder and views:</span></span>

  * <span data-ttu-id="4e4c0-142">Cliquez avec le bouton droit sur le dossier *Views*, puis sélectionnez **Ajouter > Ajouter un dossier existant**.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-142">Right-click on the *Views* folder and select **Add > Add Existing Folder**.</span></span>
  * <span data-ttu-id="4e4c0-143">Accédez au dossier *Views*, sélectionnez *Views\Movies*, puis **Ouvrir**.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-143">Navigate to the *Views* folder, select *Views\Movies*, and then select **Open**.</span></span>
  * <span data-ttu-id="4e4c0-144">Dans la boîte de dialogue **Sélectionnez les fichiers à ajouter à partir de Movies**, sélectionnez **Inclure tout**, puis **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-144">In the **Select files to add from Movies** dialog, select **Include All**, and then **OK**.</span></span>

[!INCLUDE [adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="4e4c0-145">Vous disposez maintenant d’une base de données et de pages pour afficher, modifier, mettre à jour et supprimer les données.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-145">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="4e4c0-146">Dans le prochain didacticiel, nous allons utiliser la base de données.</span><span class="sxs-lookup"><span data-stu-id="4e4c0-146">In the next tutorial, we'll work with the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4e4c0-147">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="4e4c0-147">Additional resources</span></span>

* [<span data-ttu-id="4e4c0-148">Les Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="4e4c0-148">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="4e4c0-149">Globalisation et localisation</span><span class="sxs-lookup"><span data-stu-id="4e4c0-149">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="4e4c0-150">[Précédent : Ajout d’une vue](adding-view.md)
> [Suivant : Utilisation de SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="4e4c0-150">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
