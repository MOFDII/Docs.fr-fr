---
title: "Ajout d’un modèle dans une application ASP.NET Core MVC."
author: rick-anderson
description: "Ajoutez un modèle à une application ASP.NET Core simple."
ms.author: riande
ms.date: 09/18/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
manager: wpickett
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 32677b8232e907e8431e05a3727fe7a2e5717ec4
ms.sourcegitcommit: 83b5a4715fd25e4eb6f7c8427c0ef03850a7fa07
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2018
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="7f3db-103">Ajoutez une classe au dossier *Modèles* nommé *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="7f3db-103">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="7f3db-104">Ajoutez le code suivant au fichier *Models/Movie.cs* :</span><span class="sxs-lookup"><span data-stu-id="7f3db-104">Add the following code to the *Models/Movie.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="7f3db-105">Le champ `ID` est nécessaire à la base de données pour la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="7f3db-105">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="7f3db-106">Générez l’application pour vérifier que vous n’avez aucune erreur, et que vous avez enfin ajouté un **M**odèle à votre application **M**VC.</span><span class="sxs-lookup"><span data-stu-id="7f3db-106">Build the app to verify you don't have any errors, and you've finally added a **M**odel to your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="7f3db-107">Préparer le projet pour la génération de modèles automatique</span><span class="sxs-lookup"><span data-stu-id="7f3db-107">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="7f3db-108">Ajoutez les packages NuGet en surbrillance suivants au fichier *MvcMovie.csproj* :</span><span class="sxs-lookup"><span data-stu-id="7f3db-108">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="7f3db-109">Enregistrez le fichier et sélectionnez **Restaurer** quand une boîte de dialogue **Informations** s’affiche en indiquant qu’il existe des dépendances non résolues.</span><span class="sxs-lookup"><span data-stu-id="7f3db-109">Save the file and select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>
- <span data-ttu-id="7f3db-110">Créez un fichier *Models/MvcMovieContext.cs*, puis ajoutez la classe `MvcMovieContext` suivante :</span><span class="sxs-lookup"><span data-stu-id="7f3db-110">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- <span data-ttu-id="7f3db-111">Ouvrez le fichier *Startup.cs*, puis ajoutez deux instructions using :</span><span class="sxs-lookup"><span data-stu-id="7f3db-111">Open the *Startup.cs* file and add two usings:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- <span data-ttu-id="7f3db-112">Ajoutez le contexte de base de données au fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="7f3db-112">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="7f3db-113">Cela permet d’indiquer à Entity Framework quelles sont les classes de modèles incluses dans le modèle de données.</span><span class="sxs-lookup"><span data-stu-id="7f3db-113">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="7f3db-114">Vous définissez un *jeu d’entités* d’objets Movies, lesquels sont représentés dans la base de données sous forme de table Movie.</span><span class="sxs-lookup"><span data-stu-id="7f3db-114">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="7f3db-115">Générez le projet pour vérifier qu’il n’existe aucune erreur.</span><span class="sxs-lookup"><span data-stu-id="7f3db-115">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="7f3db-116">Effectuer une génération de modèles automatique pour MovieController</span><span class="sxs-lookup"><span data-stu-id="7f3db-116">Scaffold the MovieController</span></span>

<span data-ttu-id="7f3db-117">Ouvrez une fenêtre de terminal dans le dossier de projet, puis exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="7f3db-117">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="7f3db-118">Le moteur de génération de modèles automatique crée les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="7f3db-118">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="7f3db-119">contrôleur de films (*Controllers/MoviesController.cs*) ;</span><span class="sxs-lookup"><span data-stu-id="7f3db-119">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="7f3db-120">fichiers de vues Razor pour les pages Create, Delete, Details, Edit et Index (*Views/Movies/\*.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="7f3db-120">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="7f3db-121">La création automatique de méthodes d’action et de vues [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (créer, lire, mettre à jour et supprimer) s’appelle la *génération de modèles automatique*.</span><span class="sxs-lookup"><span data-stu-id="7f3db-121">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="7f3db-122">Vous aurez bientôt une application web entièrement opérationnelle qui vous permettra de gérer une base de données de films.</span><span class="sxs-lookup"><span data-stu-id="7f3db-122">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="7f3db-123">Vous disposez maintenant d’une base de données et de pages pour afficher, modifier, mettre à jour et supprimer les données.</span><span class="sxs-lookup"><span data-stu-id="7f3db-123">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="7f3db-124">Dans le prochain didacticiel, nous allons utiliser la base de données.</span><span class="sxs-lookup"><span data-stu-id="7f3db-124">In the next tutorial, we'll work with the database.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="7f3db-125">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7f3db-125">Additional resources</span></span>

* [<span data-ttu-id="7f3db-126">Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="7f3db-126">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="7f3db-127">Globalisation et localisation</span><span class="sxs-lookup"><span data-stu-id="7f3db-127">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="7f3db-128">[Précédent - Ajouter une vue](adding-view.md)
[Suivant - Utilisation de SQLite](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="7f3db-128">[Previous - Add a view](adding-view.md)
[Next - Working with SQLite](working-with-sql.md)</span></span>
