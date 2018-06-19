---
uid: web-api/overview/advanced/dependency-injection
title: Injection de dépendances dans ASP.NET Web API 2 | Documents Microsoft
author: MikeWasson
description: Ce didacticiel montre comment injecter des dépendances dans votre contrôleur de l’API Web ASP.NET. Versions des logiciels utilisées dans le bloc d’Application Unity didacticiel Web API 2...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 7f64cc83e36c80b0ffd53edfc629557c0847b200
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036513"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="c3ca7-104">Injection de dépendances dans ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="c3ca7-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="c3ca7-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c3ca7-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c3ca7-106">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="c3ca7-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="c3ca7-107">Ce didacticiel montre comment injecter des dépendances dans votre contrôleur de l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c3ca7-108">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="c3ca7-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c3ca7-109">API Web 2</span><span class="sxs-lookup"><span data-stu-id="c3ca7-109">Web API 2</span></span>
> - [<span data-ttu-id="c3ca7-110">Bloc d’Application Unity</span><span class="sxs-lookup"><span data-stu-id="c3ca7-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="c3ca7-111">Entity Framework 6 (version 5 fonctionne aussi)</span><span class="sxs-lookup"><span data-stu-id="c3ca7-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="c3ca7-112">Nouveautés d’Injection de dépendance ?</span><span class="sxs-lookup"><span data-stu-id="c3ca7-112">What is Dependency Injection?</span></span>

<span data-ttu-id="c3ca7-113">A *dépendance* est n’importe quel objet nécessitant un autre objet.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="c3ca7-114">Par exemple, il est courant de définir un [référentiel](http://martinfowler.com/eaaCatalog/repository.html) qui gère l’accès aux données.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="c3ca7-115">Nous allons illustrer avec un exemple.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-115">Let's illustrate with an example.</span></span> <span data-ttu-id="c3ca7-116">Tout d’abord, nous allons définir un modèle de domaine :</span><span class="sxs-lookup"><span data-stu-id="c3ca7-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="c3ca7-117">Voici une classe de référentiel simple qui stocke des éléments dans une base de données, à l’aide d’Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="c3ca7-118">Maintenant nous allons définir un contrôleur d’API Web qui prend en charge des demandes GET pour `Product` entités.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="c3ca7-119">(Je suis en laissant les POST et les autres méthodes par souci de simplicité.) Voici une première tentative de :</span><span class="sxs-lookup"><span data-stu-id="c3ca7-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="c3ca7-120">Notez que la classe de contrôleur dépend `ProductRepository`, et nous allons laisser le contrôleur de créer le `ProductRepository` instance.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="c3ca7-121">Toutefois, il est déconseillé de coder en dur la dépendance de cette façon, pour plusieurs raisons.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="c3ca7-122">Si vous souhaitez remplacer `ProductRepository` avec une implémentation différente, vous devez également modifier la classe de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="c3ca7-123">Si le `ProductRepository` possède des dépendances, vous devez configurer dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="c3ca7-124">Pour un grand projet avec plusieurs contrôleurs, votre code de configuration devienne dispersée dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="c3ca7-125">Il est difficile de test unitaire, étant donné que le contrôleur est codé en dur pour interroger la base de données.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="c3ca7-126">Pour un test unitaire, vous devez utiliser un référentiel fictifs ou stub, ce qui n’est pas possible avec la conception faites-en.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="c3ca7-127">Nous pouvons résoudre ces problèmes en *injection* le référentiel dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="c3ca7-128">Tout d’abord, refactorisez la `ProductRepository` classe dans une interface :</span><span class="sxs-lookup"><span data-stu-id="c3ca7-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="c3ca7-129">Puis indiquez le `IProductRepository` comme paramètre de constructeur :</span><span class="sxs-lookup"><span data-stu-id="c3ca7-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="c3ca7-130">Cet exemple utilise [injection de constructeur](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="c3ca7-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="c3ca7-131">Vous pouvez également utiliser *injection de setter*, où vous définissez la dépendance via une méthode setter ou une propriété.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="c3ca7-132">Mais maintenant il existe un problème, car votre application ne crée pas directement le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="c3ca7-133">API Web crée le contrôleur lorsqu’il achemine la demande et API Web ne sait rien sur `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="c3ca7-134">Il s’agit là qu’intervient le résolveur de dépendance d’API Web.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="c3ca7-135">Le résolveur de dépendance d’API Web</span><span class="sxs-lookup"><span data-stu-id="c3ca7-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="c3ca7-136">API Web définit les **IDependencyResolver** interface pour la résolution des dépendances.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="c3ca7-137">Voici la définition de l’interface :</span><span class="sxs-lookup"><span data-stu-id="c3ca7-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="c3ca7-138">Le **IDependencyScope** interface possède deux méthodes :</span><span class="sxs-lookup"><span data-stu-id="c3ca7-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="c3ca7-139">**GetService** crée une instance d’un type.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="c3ca7-140">**GetServices** crée une collection d’objets d’un type spécifié.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="c3ca7-141">Le **IDependencyResolver** hérite de la méthode **IDependencyScope** et ajoute les **BeginScope** (méthode).</span><span class="sxs-lookup"><span data-stu-id="c3ca7-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="c3ca7-142">Je vous parlerai étendues plus loin dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="c3ca7-143">Lors de l’API Web crée une instance de contrôleur, il appelle d’abord **IDependencyResolver.GetService**, en passant le type du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="c3ca7-144">Vous pouvez utiliser ce point d’extensibilité pour créer le contrôleur, résolution des dépendances.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="c3ca7-145">Si **GetService** retourne null, recherche un constructeur sans paramètre sur la classe de contrôleur Web API.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="c3ca7-146">Résolution de dépendance avec le conteneur Unity</span><span class="sxs-lookup"><span data-stu-id="c3ca7-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="c3ca7-147">Bien que vous pouvez écrire un **IDependencyResolver** implémentation à partir de zéro, l’interface est vraiment conçue pour agir en tant que pont entre les conteneurs d’inversion de contrôle existants et les API Web.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="c3ca7-148">Un conteneur inversion de contrôle est un composant logiciel qui est chargé de gérer les dépendances.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="c3ca7-149">Vous inscrivez les types avec le conteneur et ensuite utilisez le conteneur pour créer des objets.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="c3ca7-150">Le conteneur effectue automatiquement les relations de dépendance.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="c3ca7-151">De nombreux conteneurs d’inversion de contrôle vous permettent également de contrôler les éléments tels que la durée de vie et la portée.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="c3ca7-152">« IoC » signifie « d’inversion de contrôle », qui est un modèle général où une infrastructure appelle du code d’application.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="c3ca7-153">Un conteneur IoC construit vos objets, lequel « inverse » le flux habituel de contrôle.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="c3ca7-154">Pour ce didacticiel, nous allons utiliser [Unity](https://msdn.microsoft.com/library/ff647202.aspx) à partir de Microsoft Patterns &amp; pratiques.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="c3ca7-155">(Incluent d’autres bibliothèques populaires [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), et [StructureMap ](http://docs.structuremap.net/).) Vous pouvez utiliser le Gestionnaire de Package NuGet pour installer Unity.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://docs.structuremap.net/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="c3ca7-156">À partir de la **outils** menu dans Visual Studio, sélectionnez **Gestionnaire de Package de bibliothèque**, puis sélectionnez **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-156">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="c3ca7-157">Dans la fenêtre de Console du Gestionnaire de Package, tapez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="c3ca7-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="c3ca7-158">Voici une implémentation de **IDependencyResolver** qui encapsule un conteneur Unity.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="c3ca7-159">Si le **GetService** méthode ne peut pas résoudre un type, elle doit retourner **null**.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="c3ca7-160">Si le **GetServices** méthode ne peut pas résoudre un type, elle doit retourner un objet de collection vide.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="c3ca7-161">Ne pas lever d’exceptions pour les types inconnus.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="c3ca7-162">Configurer le résolveur de dépendance</span><span class="sxs-lookup"><span data-stu-id="c3ca7-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="c3ca7-163">Définir le résolveur de dépendance sur le **DependencyResolver** propriété global **HttpConfiguration** objet.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="c3ca7-164">Le code suivant inscrit le `IProductRepository` de l’interface avec Unity, puis crée un `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="c3ca7-165">Portée des dépendances et la durée de vie de contrôleur</span><span class="sxs-lookup"><span data-stu-id="c3ca7-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="c3ca7-166">Contrôleurs sont créés par la demande.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-166">Controllers are created per request.</span></span> <span data-ttu-id="c3ca7-167">Pour gérer la durée de vie des objets, **IDependencyResolver** utilise le concept d’un *étendue*.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="c3ca7-168">Le résolveur de dépendance attachée à la **HttpConfiguration** objet a une portée globale.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="c3ca7-169">Lors de l’API Web crée un contrôleur, il appelle **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="c3ca7-170">Cette méthode retourne un **IDependencyScope** qui représente une étendue enfant.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="c3ca7-171">API Web appelle ensuite **GetService** sur l’étendue enfant pour créer le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="c3ca7-172">Lors de la demande est terminée, les API Web appelle **Dispose** sur l’étendue enfant.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="c3ca7-173">Utilisez le **Dispose** méthode pour supprimer les dépendances du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="c3ca7-174">La procédure d’implémentation **BeginScope** varie selon le conteneur inversion de contrôle.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="c3ca7-175">Pour Unity, la portée correspond à un conteneur enfant :</span><span class="sxs-lookup"><span data-stu-id="c3ca7-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="c3ca7-176">La plupart des conteneurs d’inversion de contrôle ont des équivalents similaire.</span><span class="sxs-lookup"><span data-stu-id="c3ca7-176">Most IoC containers have similar equivalents.</span></span>
