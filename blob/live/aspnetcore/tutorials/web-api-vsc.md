---
title: "Créer une API web avec ASP.NET Core et VS Code"
description: "Générer une API web sur macOS, Linux ou Windows avec ASP.NET Core MVC et Visual Studio Code"
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
keywords: ASP.NET Core, APIweb, API web, REST, Mac, Linux, HTTP, Service, Service HTTP, VS Code
manager: wpickett
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
uid: tutorials/web-api-vsc
ms.openlocfilehash: 40f9259101e5d006378562a27e97948641e29450
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/03/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="252f8-104">Créer une API web avec ASP.NET Core MVC et Visual Studio Code sur Linux, macOS et Windows</span><span class="sxs-lookup"><span data-stu-id="252f8-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="252f8-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="252f8-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="252f8-106">Dans ce didacticiel, vous allez générer une API web pour la gestion d’une liste de tâches.</span><span class="sxs-lookup"><span data-stu-id="252f8-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="252f8-107">Vous ne générerez pas d’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="252f8-107">You won’t build a UI.</span></span>

<span data-ttu-id="252f8-108">Il existe trois versions de ce didacticiel :</span><span class="sxs-lookup"><span data-stu-id="252f8-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="252f8-109">macOS, Linux, Windows : API web avec Visual Studio Code (le présent didacticiel)</span><span class="sxs-lookup"><span data-stu-id="252f8-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="252f8-110">macOS : [API web avec Visual Studio pour Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="252f8-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="252f8-111">Windows : [API web avec Visual Studio pour Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="252f8-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="252f8-112">Configurer votre environnement de développement</span><span class="sxs-lookup"><span data-stu-id="252f8-112">Set up your development environment</span></span>

<span data-ttu-id="252f8-113">Téléchargez et installez :</span><span class="sxs-lookup"><span data-stu-id="252f8-113">Download and install:</span></span>
- <span data-ttu-id="252f8-114">[SDK .NET Core 2.0.0](https://www.microsoft.com/net/core) ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="252f8-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="252f8-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="252f8-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="252f8-116">[Extension C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="252f8-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="252f8-117">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="252f8-117">Create the project</span></span>

<span data-ttu-id="252f8-118">À partir d’une console, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="252f8-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="252f8-119">Ouvrez le dossier *TodoApi* dans Visual Studio Code (VS Code), puis sélectionnez le fichier *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="252f8-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="252f8-120">Sélectionnez **Oui** quand une boîte de dialogue **Avertissement** s’affiche en indiquant le message suivant : « Les composants nécessaires à la build et au débogage sont manquants dans 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="252f8-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="252f8-121">Faut-il les ajouter ? »</span><span class="sxs-lookup"><span data-stu-id="252f8-121">Add them?"</span></span>
- <span data-ttu-id="252f8-122">Sélectionnez **Restaurer** quand une boîte de dialogue **Informations** s’affiche en indiquant qu’il existe des dépendances non résolues.</span><span class="sxs-lookup"><span data-stu-id="252f8-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code avec le message d’avertissement indiquant « Les composants nécessaires à la build et au débogage sont manquants dans 'TodoApi'.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="252f8-126">Appuyez sur **Déboguer** (F5) pour générer et exécuter le programme.</span><span class="sxs-lookup"><span data-stu-id="252f8-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="252f8-127">Dans un navigateur, accédez à http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="252f8-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="252f8-128">Le résultat suivant s’affiche :</span><span class="sxs-lookup"><span data-stu-id="252f8-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="252f8-129">Pour obtenir des conseils sur l’utilisation de VS Code, consultez [Aide de Visual Studio Code](#visual-studio-code-help).</span><span class="sxs-lookup"><span data-stu-id="252f8-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="252f8-130">Ajouter la prise en charge d’Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="252f8-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="252f8-131">La création d’un projet dans .NET Core 2.0 ajoute le fournisseur 'Microsoft.AspNetCore.All' au fichier *TodoApi.csproj*.</span><span class="sxs-lookup"><span data-stu-id="252f8-131">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="252f8-132">Il est inutile d’installer le fournisseur de base de données [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) séparément.</span><span class="sxs-lookup"><span data-stu-id="252f8-132">There is no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="252f8-133">Ce fournisseur de base de données permet d’utiliser Entity Framework Core avec une base de données en mémoire.</span><span class="sxs-lookup"><span data-stu-id="252f8-133">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="252f8-134">Ajouter une classe de modèle</span><span class="sxs-lookup"><span data-stu-id="252f8-134">Add a model class</span></span>

<span data-ttu-id="252f8-135">Un modèle est un objet qui représente les données dans votre application.</span><span class="sxs-lookup"><span data-stu-id="252f8-135">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="252f8-136">Dans le cas présent, le seul modèle est une tâche.</span><span class="sxs-lookup"><span data-stu-id="252f8-136">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="252f8-137">Ajoutez un dossier nommé *Models*.</span><span class="sxs-lookup"><span data-stu-id="252f8-137">Add a folder named *Models*.</span></span> <span data-ttu-id="252f8-138">Vous pouvez placer des classes de modèle n’importe où dans votre projet, mais le dossier *Models* est utilisé par convention.</span><span class="sxs-lookup"><span data-stu-id="252f8-138">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="252f8-139">Ajouter une classe `TodoItem` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="252f8-139">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="252f8-140">La base de données génère la valeur `Id` quand un `TodoItem` est créé.</span><span class="sxs-lookup"><span data-stu-id="252f8-140">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="252f8-141">Créer le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="252f8-141">Create the database context</span></span>

<span data-ttu-id="252f8-142">Le *contexte de base de données* est la classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données spécifié.</span><span class="sxs-lookup"><span data-stu-id="252f8-142">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="252f8-143">Vous créez cette classe en dérivant de la classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="252f8-143">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="252f8-144">Ajoutez une classe `TodoContext` dans le dossier *Models* :</span><span class="sxs-lookup"><span data-stu-id="252f8-144">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="252f8-145">Ajouter un contrôleur</span><span class="sxs-lookup"><span data-stu-id="252f8-145">Add a controller</span></span>

<span data-ttu-id="252f8-146">Dans le dossier *Contrôleurs*, créez une classe nommée `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="252f8-146">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="252f8-147">Ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="252f8-147">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="252f8-148">Lancer l’application</span><span class="sxs-lookup"><span data-stu-id="252f8-148">Launch the app</span></span>

<span data-ttu-id="252f8-149">Dans VS Code, appuyez sur F5 pour lancer l’application.</span><span class="sxs-lookup"><span data-stu-id="252f8-149">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="252f8-150">Accédez à http://localhost:5000/api/todo (le contrôleur `Todo` que nous venons de créer).</span><span class="sxs-lookup"><span data-stu-id="252f8-150">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="252f8-151">Aide de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="252f8-151">Visual Studio Code help</span></span>

- [<span data-ttu-id="252f8-152">Prise en main</span><span class="sxs-lookup"><span data-stu-id="252f8-152">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="252f8-153">Débogage</span><span class="sxs-lookup"><span data-stu-id="252f8-153">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="252f8-154">Terminal intégré</span><span class="sxs-lookup"><span data-stu-id="252f8-154">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="252f8-155">Raccourcis clavier</span><span class="sxs-lookup"><span data-stu-id="252f8-155">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="252f8-156">Raccourcis clavier Mac</span><span class="sxs-lookup"><span data-stu-id="252f8-156">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="252f8-157">Raccourcis clavier Linux</span><span class="sxs-lookup"><span data-stu-id="252f8-157">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="252f8-158">Raccourcis clavier Windows</span><span class="sxs-lookup"><span data-stu-id="252f8-158">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


