---
title: "Développement d’applications ASP.NET Core à l’aide de dotnet watch"
author: rick-anderson
description: "Ce didacticiel montre comment installer et utiliser l’outil Observateur de fichiers (dotnet watch) de l’interface de ligne de commande .NET Core dans une application ASP.NET Core."
keywords: "ASP.NET Core, à l’aide de dotnet watch"
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 1b26beaa41f4b38e0cfd2c8300cb521a3dcce47d
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/03/2018
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="5ec7d-104">Développement d’applications ASP.NET Core à l’aide de dotnet watch</span><span class="sxs-lookup"><span data-stu-id="5ec7d-104">Developing ASP.NET Core apps using dotnet watch</span></span>

<span data-ttu-id="5ec7d-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="5ec7d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="5ec7d-106">`dotnet watch` est un outil qui exécute une commande [.NET Core CLI](/dotnet/core/tools) quand des fichiers sources changent.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-106">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="5ec7d-107">Par exemple, un changement de fichier peut déclencher la compilation, l’exécution de tests ou le déploiement.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-107">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="5ec7d-108">Dans ce didacticiel, nous utilisons une application API web existante avec deux points de terminaison : un qui retourne une somme et un qui retourne un produit.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-108">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="5ec7d-109">La méthode product contient un bogue que nous allons résoudre dans le cadre de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="5ec7d-110">Téléchargez l’[application exemple](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span><span class="sxs-lookup"><span data-stu-id="5ec7d-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="5ec7d-111">Elle contient deux projets : *WebApp* (API web ASP.NET Core) et *WebAppTests* (API de tests unitaires pour le web).</span><span class="sxs-lookup"><span data-stu-id="5ec7d-111">It contains two projects: *WebApp* (an ASP.NET Core Web API) and *WebAppTests* (unit tests for the Web API).</span></span>

<span data-ttu-id="5ec7d-112">Dans une interface de commande, accédez au dossier *WebApp* et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="5ec7d-112">In a command shell, navigate to the *WebApp* folder and run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="5ec7d-113">La sortie de la console affiche des messages semblables à ce qui suit (indiquant que l’application est en cours d’exécution et en attente de demandes) :</span><span class="sxs-lookup"><span data-stu-id="5ec7d-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="5ec7d-114">Dans un navigateur web, accédez à `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="5ec7d-115">Vous devez voir le résultat `9`.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-115">You should see the result of `9`.</span></span>

<span data-ttu-id="5ec7d-116">Accédez à l’API du produit (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span><span class="sxs-lookup"><span data-stu-id="5ec7d-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="5ec7d-117">Elle retourne `9`, et non pas `20` comme prévu.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="5ec7d-118">Nous allons corriger cela ultérieurement dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-118">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="5ec7d-119">Ajouter `dotnet watch` à un projet</span><span class="sxs-lookup"><span data-stu-id="5ec7d-119">Add `dotnet watch` to a project</span></span>

1. <span data-ttu-id="5ec7d-120">Ajoutez une référence de package `Microsoft.DotNet.Watcher.Tools` dans le fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="5ec7d-120">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. <span data-ttu-id="5ec7d-121">Installez le package `Microsoft.DotNet.Watcher.Tools` en exécutant la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="5ec7d-121">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="5ec7d-122">Exécution des commandes de l’interface CLI de .NET Core avec `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="5ec7d-122">Running .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="5ec7d-123">Toutes les [commandes de l’interface CLI de .NET Core](/dotnet/core/tools#cli-commands) peuvent être exécutées avec `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-123">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="5ec7d-124">Exemple :</span><span class="sxs-lookup"><span data-stu-id="5ec7d-124">For example:</span></span>

| <span data-ttu-id="5ec7d-125">Commande</span><span class="sxs-lookup"><span data-stu-id="5ec7d-125">Command</span></span> | <span data-ttu-id="5ec7d-126">Commande avec watch</span><span class="sxs-lookup"><span data-stu-id="5ec7d-126">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="5ec7d-127">dotnet run</span><span class="sxs-lookup"><span data-stu-id="5ec7d-127">dotnet run</span></span> | <span data-ttu-id="5ec7d-128">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="5ec7d-128">dotnet watch run</span></span> |
| <span data-ttu-id="5ec7d-129">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="5ec7d-129">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="5ec7d-130">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="5ec7d-130">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="5ec7d-131">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="5ec7d-131">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="5ec7d-132">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="5ec7d-132">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="5ec7d-133">dotnet test</span><span class="sxs-lookup"><span data-stu-id="5ec7d-133">dotnet test</span></span> | <span data-ttu-id="5ec7d-134">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="5ec7d-134">dotnet watch test</span></span> |

<span data-ttu-id="5ec7d-135">Exécutez `dotnet watch run` dans le dossier *WebApp*.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-135">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="5ec7d-136">La sortie de la console indique que `watch` a démarré.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-136">The console output indicates `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="5ec7d-137">Apport de modifications avec `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="5ec7d-137">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="5ec7d-138">Vérifiez que `dotnet watch` est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-138">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="5ec7d-139">Corrigez le bogue présent dans la méthode `Product` de *MathController.cs* afin qu’elle retourne le produit et non la somme :</span><span class="sxs-lookup"><span data-stu-id="5ec7d-139">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="5ec7d-140">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-140">Save the file.</span></span> <span data-ttu-id="5ec7d-141">La sortie de la console indique que `dotnet watch` a détecté un changement de fichier et a redémarré l’application.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-141">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="5ec7d-142">Vérifiez que `http://localhost:<port number>/api/math/product?a=4&b=5` retourne le résultat correct.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-142">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="5ec7d-143">Exécution de tests à l’aide de `dotnet watch`</span><span class="sxs-lookup"><span data-stu-id="5ec7d-143">Running tests using `dotnet watch`</span></span>

1. <span data-ttu-id="5ec7d-144">Modifiez la méthode `Product` de *MathController.cs* pour qu’elle retourne à nouveau la somme, puis enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-144">Change the `Product` method of *MathController.cs* back to returning the sum and save the file.</span></span>
1. <span data-ttu-id="5ec7d-145">Dans une interface de commande, accédez au dossier *WebAppTests*.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-145">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="5ec7d-146">Exécutez `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-146">Run `dotnet restore`.</span></span>
1. <span data-ttu-id="5ec7d-147">Exécutez `dotnet watch test`.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-147">Run `dotnet watch test`.</span></span> <span data-ttu-id="5ec7d-148">Sa sortie indique qu’un test a échoué et que l’observateur est en attente de changement de fichier :</span><span class="sxs-lookup"><span data-stu-id="5ec7d-148">Its output indicates that a test failed and that watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="5ec7d-149">Corrigez le code de la méthode `Product` afin qu’elle retourne le produit.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-149">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="5ec7d-150">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-150">Save the file.</span></span>

<span data-ttu-id="5ec7d-151">`dotnet watch` détecte le changement de fichier et réexécute les tests.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-151">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="5ec7d-152">La sortie de la console indique que les tests ont réussi.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-152">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="5ec7d-153">dotnet-watch dans GitHub</span><span class="sxs-lookup"><span data-stu-id="5ec7d-153">dotnet-watch in GitHub</span></span>

<span data-ttu-id="5ec7d-154">dotnet-watch fait partie du [dépôt DotNetTools](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch) GitHub.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-154">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>

<span data-ttu-id="5ec7d-155">La [section MSBuild](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) du fichier [Lisez-moi dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explique comment configurer dotnet-watch à partir du fichier projet MSBuild surveillé.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-155">The [MSBuild section](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="5ec7d-156">Le fichier [Lisez-moi dotnet-watch](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contient des informations sur dotnet-watch non traitées dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="5ec7d-156">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
