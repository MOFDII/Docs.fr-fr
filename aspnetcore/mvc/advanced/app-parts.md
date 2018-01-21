---
title: "Composants d’application ASP.NET Core"
author: ardalis
description: "Découvrez comment utiliser des parties de l’application, qui sont abstrations sur les ressources d’une application, pour configurer votre application pour découvrir ou d’éviter les fonctionnalités de chargement à partir d’un assembly."
ms.author: riande
manager: wpickett
ms.date: 01/04/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 12c34b7a9521835533998c5609870bc712a6d48c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="53d00-103">Composants d’application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53d00-103">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="53d00-104">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="53d00-104">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="53d00-105">Un *partie Application* est une abstraction sur les ressources d’une application, à partir de laquelle les fonctionnalités de MVC tels que les contrôleurs, les composants de la vue, ou programmes d’assistance de balise peuvent être découvertes.</span><span class="sxs-lookup"><span data-stu-id="53d00-105">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="53d00-106">Un exemple d’une partie de l’application est un AssemblyPart, qui encapsule une référence d’assembly et expose les types et les références de compilation.</span><span class="sxs-lookup"><span data-stu-id="53d00-106">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="53d00-107">*Fournisseurs de fonctionnalités* fonctionnent avec les composants d’application pour remplir les fonctionnalités d’une application ASP.NET MVC de base.</span><span class="sxs-lookup"><span data-stu-id="53d00-107">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="53d00-108">Cas d’usage principal pour les parties de l’application est à vous permettent de configurer votre application pour découvrir (ou éviter le chargement) fonctionnalités MVC à partir d’un assembly.</span><span class="sxs-lookup"><span data-stu-id="53d00-108">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="53d00-109">Présentation des composants d’Application</span><span class="sxs-lookup"><span data-stu-id="53d00-109">Introducing Application Parts</span></span>

<span data-ttu-id="53d00-110">Les applications MVC chargement leurs fonctionnalités à partir de [parties de l’application](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span><span class="sxs-lookup"><span data-stu-id="53d00-110">MVC apps load their features from [application parts](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="53d00-111">En particulier, la [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) classe représente une partie de l’application qui est sauvegardée par un assembly.</span><span class="sxs-lookup"><span data-stu-id="53d00-111">In particular, the [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that is backed by an assembly.</span></span> <span data-ttu-id="53d00-112">Vous pouvez utiliser ces classes pour découvrir et charger des fonctionnalités MVC, telles que les contrôleurs, affichage des composants, les programmes d’assistance de balise et sources de compilation razor.</span><span class="sxs-lookup"><span data-stu-id="53d00-112">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="53d00-113">Le [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) est chargé de suivre les parties de l’application et les fournisseurs de fonctionnalités disponibles pour l’application MVC.</span><span class="sxs-lookup"><span data-stu-id="53d00-113">The [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="53d00-114">Vous pouvez interagir avec les `ApplicationPartManager` dans `Startup` lorsque vous configurez MVC :</span><span class="sxs-lookup"><span data-stu-id="53d00-114">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => p.ApplicationParts.Add(part));
```

<span data-ttu-id="53d00-115">Par défaut MVC recherche l’arborescence des dépendances et rechercher des contrôleurs (même dans d’autres assemblys).</span><span class="sxs-lookup"><span data-stu-id="53d00-115">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="53d00-116">Pour charger un assembly arbitraire (par exemple, à partir d’un plug-in qui n’est pas référencé au moment de la compilation), vous pouvez utiliser une partie de l’application.</span><span class="sxs-lookup"><span data-stu-id="53d00-116">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="53d00-117">Vous pouvez utiliser les composants d’application pour *éviter* pour les contrôleurs dans un assembly particulier ou un emplacement de la recherche.</span><span class="sxs-lookup"><span data-stu-id="53d00-117">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="53d00-118">Vous pouvez contrôler (ou les assemblys) sont disponibles pour l’application en modifiant le `ApplicationParts` collection de la `ApplicationPartManager`.</span><span class="sxs-lookup"><span data-stu-id="53d00-118">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="53d00-119">L’ordre des entrées dans le `ApplicationParts` collection n’est pas importante.</span><span class="sxs-lookup"><span data-stu-id="53d00-119">The order of the entries in the `ApplicationParts` collection is not important.</span></span> <span data-ttu-id="53d00-120">Il est important de configurer complètement le `ApplicationPartManager` avant de l’utiliser pour configurer les services dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="53d00-120">It is important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="53d00-121">Par exemple, vous devez entièrement configurer le `ApplicationPartManager` avant d’appeler `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="53d00-121">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="53d00-122">Ne parvient pas à le faire, signifie que les contrôleurs dans les composants d’application ajoutés après que l’appel de méthode n’est pas affecté (ne sera pas obtenir inscrit en tant que services) qui peut entraîner une bevavior incorrecte de votre application.</span><span class="sxs-lookup"><span data-stu-id="53d00-122">Failing to do so, will mean that controllers in application parts added after that method call will not be affected (will not get registered as services) which might result in incorrect bevavior of your application.</span></span>

<span data-ttu-id="53d00-123">Si vous avez un assembly qui contient des contrôleurs que vous ne souhaitez pas être utilisé, supprimez-le de la `ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="53d00-123">If you have an assembly that contains controllers you do not want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p =>
    {
        var dependentLibrary = p.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="53d00-124">En plus de l’assembly du projet et ses assemblys dépendants, la `ApplicationPartManager` inclura des composants pour `Microsoft.AspNetCore.Mvc.TagHelpers` et `Microsoft.AspNetCore.Mvc.Razor` par défaut.</span><span class="sxs-lookup"><span data-stu-id="53d00-124">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="53d00-125">Fournisseurs de fonctionnalités d’application</span><span class="sxs-lookup"><span data-stu-id="53d00-125">Application Feature Providers</span></span>

<span data-ttu-id="53d00-126">Fournisseurs de fonctionnalités d’application examiner des parties de l’application et fournissent des fonctionnalités pour les parties.</span><span class="sxs-lookup"><span data-stu-id="53d00-126">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="53d00-127">Il existe des fournisseurs de fonctionnalités intégrées pour les composants MVC suivants :</span><span class="sxs-lookup"><span data-stu-id="53d00-127">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="53d00-128">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="53d00-128">Controllers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="53d00-129">Référence de métadonnées</span><span class="sxs-lookup"><span data-stu-id="53d00-129">Metadata Reference</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="53d00-130">Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="53d00-130">Tag Helpers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="53d00-131">Affichage des composants</span><span class="sxs-lookup"><span data-stu-id="53d00-131">View Components</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="53d00-132">Héritent des fournisseurs de fonctionnalités `IApplicationFeatureProvider<T>`, où `T` est le type de la fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="53d00-132">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="53d00-133">Vous pouvez implémenter votre propre fonction pour un des types de fonctions de MVC, les fournisseurs répertoriés ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="53d00-133">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="53d00-134">L’ordre des fournisseurs de fonctionnalités dans le `ApplicationPartManager.FeatureProviders` collection peut être importante, étant donné que les fournisseurs ultérieure peuvent réagir aux actions effectuées par les fournisseurs précédents.</span><span class="sxs-lookup"><span data-stu-id="53d00-134">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="53d00-135">Exemple : Fonctionnalité du contrôleur générique</span><span class="sxs-lookup"><span data-stu-id="53d00-135">Sample: Generic controller feature</span></span>

<span data-ttu-id="53d00-136">Par défaut, ASP.NET Core MVC ignore les contrôleurs génériques (par exemple, `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="53d00-136">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="53d00-137">Cet exemple utilise un fournisseur de fonctionnalité du contrôleur qui s’exécute lorsque le fournisseur par défaut et ajoute des instances de contrôleur générique pour une liste spécifiée de types (défini dans `EntityTypes.Types`) :</span><span class="sxs-lookup"><span data-stu-id="53d00-137">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="53d00-138">Les types d’entités :</span><span class="sxs-lookup"><span data-stu-id="53d00-138">The entity types:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="53d00-139">Le fournisseur de fonctionnalités est ajouté dans `Startup`:</span><span class="sxs-lookup"><span data-stu-id="53d00-139">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="53d00-140">Par défaut, les noms de contrôleur générique utilisés pour le routage serait sous la forme *GenericController'1 [Widget]* au lieu de *Widget*.</span><span class="sxs-lookup"><span data-stu-id="53d00-140">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="53d00-141">L’attribut suivant est utilisé pour modifier le nom afin qu’ils correspondent au type générique utilisé par le contrôleur :</span><span class="sxs-lookup"><span data-stu-id="53d00-141">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="53d00-142">La `GenericController` classe :</span><span class="sxs-lookup"><span data-stu-id="53d00-142">The `GenericController` class:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="53d00-143">Le résultat, lorsqu’un itinéraire correspondant est demandé :</span><span class="sxs-lookup"><span data-stu-id="53d00-143">The result, when a matching route is requested:</span></span>

![Exemple de sortie à partir de l’exemple d’application lit, « Bonjour à partir d’un contrôleur Sproket générique ».](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="53d00-145">Exemple : Fonctionnalités d’affichage disponibles</span><span class="sxs-lookup"><span data-stu-id="53d00-145">Sample: Display available features</span></span>

<span data-ttu-id="53d00-146">Vous pouvez parcourir les fonctionnalités remplies disponibles à votre application en demandant une `ApplicationPartManager` via [injection de dépendance](../../fundamentals/dependency-injection.md) et l’utiliser pour remplir les instances des fonctionnalités appropriées :</span><span class="sxs-lookup"><span data-stu-id="53d00-146">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="53d00-147">Exemple de sortie :</span><span class="sxs-lookup"><span data-stu-id="53d00-147">Example output:</span></span>

![Exemple de sortie de l’exemple d’application](app-parts/_static/available-features.png)
