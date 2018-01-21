---
title: "Distributed d’assistance de balise de Cache | Documents Microsoft"
author: pkellner
description: "Montre comment travailler avec l’application d’assistance de balise de Cache"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: f5844dade218fdba1169a55fe3ce251a9cc03db2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="distributed-cache-tag-helper"></a><span data-ttu-id="a76ad-103">Application d’assistance de balise de Cache distribué</span><span class="sxs-lookup"><span data-stu-id="a76ad-103">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="a76ad-104">Par [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="a76ad-104">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="a76ad-105">L’application d’assistance de balise de Cache distribué permet d’améliorer considérablement les performances de votre application ASP.NET Core en mettant en cache de son contenu à une source de cache distribué.</span><span class="sxs-lookup"><span data-stu-id="a76ad-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="a76ad-106">L’assistance de balise de Cache distribué hérite de la même classe de base que l’application d’assistance de balise de Cache.</span><span class="sxs-lookup"><span data-stu-id="a76ad-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="a76ad-107">Tous les attributs associés à l’application d’assistance de balise de Cache fonctionne également sur l’assistance de balise distribué.</span><span class="sxs-lookup"><span data-stu-id="a76ad-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="a76ad-108">L’assistance de balise de Cache distribué suit le **principe de dépendances explicites** appelé **Injection de constructeur**.</span><span class="sxs-lookup"><span data-stu-id="a76ad-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="a76ad-109">Plus précisément, le `IDistributedCache` conteneur d’interface est passée dans le constructeur de la de Distributed balise Cache l’assistance.</span><span class="sxs-lookup"><span data-stu-id="a76ad-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="a76ad-110">Si aucune implémentation concrète de `IDistributedCache` a été créé dans `ConfigureServices`, se trouve généralement dans startup.cs, puis l’assistance de balise de Cache distribué utilisera le même fournisseur en mémoire pour le stockage des données mises en cache en tant que l’application d’assistance de balise de Cache de base.</span><span class="sxs-lookup"><span data-stu-id="a76ad-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="a76ad-111">Distributed des attributs de programme d’assistance de balise de Cache</span><span class="sxs-lookup"><span data-stu-id="a76ad-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="a76ad-112">activé expire sur expire après expiration coulissant varient en fonction de l’en-tête varient par requête varient par itinéraire varient par cookie varient par utilisateur varient-par priorité</span><span class="sxs-lookup"><span data-stu-id="a76ad-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="a76ad-113">Consultez d’assistance de balise de Cache pour les définitions.</span><span class="sxs-lookup"><span data-stu-id="a76ad-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="a76ad-114">Application d’assistance de balise de Cache distribué hérite de la même classe que l’application d’assistance de balise de Cache tous ces attributs sont communs à partir de l’application d’assistance de balise de Cache.</span><span class="sxs-lookup"><span data-stu-id="a76ad-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="a76ad-115">nom (obligatoire)</span><span class="sxs-lookup"><span data-stu-id="a76ad-115">name (required)</span></span>

| <span data-ttu-id="a76ad-116">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="a76ad-116">Attribute Type</span></span>    | <span data-ttu-id="a76ad-117">Exemple de valeur</span><span class="sxs-lookup"><span data-stu-id="a76ad-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="a76ad-118">chaîne</span><span class="sxs-lookup"><span data-stu-id="a76ad-118">string</span></span>    | <span data-ttu-id="a76ad-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="a76ad-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="a76ad-120">Requis `name` attribut est utilisé comme une clé pour que le cache stocké pour chaque instance d’une assistance de balise de Cache distribué.</span><span class="sxs-lookup"><span data-stu-id="a76ad-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="a76ad-121">Contrairement à la base du Cache balise d’assistance qui affecte une clé à chaque instance d’assistance de balise de Cache basée sur le nom de la page Razor et l’emplacement de l’application d’assistance de balise dans la page razor, l’assistance de balise de Cache distribué base uniquement sa clé sur l’attribut`name`</span><span class="sxs-lookup"><span data-stu-id="a76ad-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="a76ad-122">Exemple d’utilisation :</span><span class="sxs-lookup"><span data-stu-id="a76ad-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="a76ad-123">Distributed Cache balise d’assistance IDistributedCache implémentations</span><span class="sxs-lookup"><span data-stu-id="a76ad-123">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="a76ad-124">Il existe deux implémentations de `IDistributedCache` intégrée à ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a76ad-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="a76ad-125">Une est basée sur **Sql Server** et l’autre est basée sur **Redis**.</span><span class="sxs-lookup"><span data-stu-id="a76ad-125">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="a76ad-126">Vous trouverez des détails de ces implémentations à la ressource référencée ci-dessous nommée « utilisation d’un cache distribué ».</span><span class="sxs-lookup"><span data-stu-id="a76ad-126">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="a76ad-127">Les deux implémentations impliquent la définition d’une instance de `IDistributedCache` dans du ASP.NET Core **startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="a76ad-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="a76ad-128">Il n’existe aucun attribut de balise associés spécifiquement à l’aide d’une implémentation spécifique de `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="a76ad-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="a76ad-129">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a76ad-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
