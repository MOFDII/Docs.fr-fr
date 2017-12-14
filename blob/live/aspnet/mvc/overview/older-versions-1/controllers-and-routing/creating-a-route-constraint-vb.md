---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: "Création d’une contrainte d’itinéraire (VB) | Documents Microsoft"
author: StephenWalther
description: "Dans ce didacticiel, Stephen Walther montre comment vous pouvez contrôler la façon dont le navigateur demande itinéraires à la correspondance en créant des contraintes d’itinéraire avec des expressions régulières."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 67ff2666f4558abd4f8d9bddffd7aef8bb68d7bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-route-constraint-vb"></a><span data-ttu-id="50754-103">Création d’une contrainte d’itinéraire (VB)</span><span class="sxs-lookup"><span data-stu-id="50754-103">Creating a Route Constraint (VB)</span></span>
====================
<span data-ttu-id="50754-104">par [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="50754-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="50754-105">Dans ce didacticiel, Stephen Walther montre comment vous pouvez contrôler la façon dont le navigateur demande itinéraires à la correspondance en créant des contraintes d’itinéraire avec des expressions régulières.</span><span class="sxs-lookup"><span data-stu-id="50754-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="50754-106">Contraintes d’itinéraire vous permet de restreindre les demandes du navigateur qui correspond à un itinéraire particulier.</span><span class="sxs-lookup"><span data-stu-id="50754-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="50754-107">Vous pouvez utiliser une expression régulière pour spécifier une contrainte d’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="50754-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="50754-108">Par exemple, imaginez que vous avez défini l’itinéraire dans la liste 1 dans le fichier Global.asax.</span><span class="sxs-lookup"><span data-stu-id="50754-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="50754-109">**La liste 1 - Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="50754-109">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="50754-110">Le listing 1 contient un itinéraire nommé produit.</span><span class="sxs-lookup"><span data-stu-id="50754-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="50754-111">Vous pouvez utiliser la gamme de produits pour mapper les demandes du navigateur à la ProductController contenu dans la liste 2.</span><span class="sxs-lookup"><span data-stu-id="50754-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="50754-112">**Listing 2 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="50754-112">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="50754-113">Notez que l’action Details() exposée par le contrôleur produit accepte un seul paramètre nommé productId.</span><span class="sxs-lookup"><span data-stu-id="50754-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="50754-114">Ce paramètre est un paramètre de type entier.</span><span class="sxs-lookup"><span data-stu-id="50754-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="50754-115">L’itinéraire défini dans la liste 1 correspond à une des URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="50754-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="50754-116">/ Produit/23</span><span class="sxs-lookup"><span data-stu-id="50754-116">/Product/23</span></span>
- <span data-ttu-id="50754-117">/ Produit/7</span><span class="sxs-lookup"><span data-stu-id="50754-117">/Product/7</span></span>

<span data-ttu-id="50754-118">Malheureusement, l’itinéraire correspond également les URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="50754-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="50754-119">/ Produits/texte</span><span class="sxs-lookup"><span data-stu-id="50754-119">/Product/blah</span></span>
- <span data-ttu-id="50754-120">/ Produits/apple</span><span class="sxs-lookup"><span data-stu-id="50754-120">/Product/apple</span></span>

<span data-ttu-id="50754-121">Étant donné que l’action Details() attend un paramètre de type entier, une demande qui contient l’autre chose qu’une valeur entière provoque une erreur.</span><span class="sxs-lookup"><span data-stu-id="50754-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="50754-122">Par exemple, si vous tapez l’URL /Product/apple dans votre navigateur vous obtenez la page d’erreur dans la Figure 1.</span><span class="sxs-lookup"><span data-stu-id="50754-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


<span data-ttu-id="50754-123">[![La boîte de dialogue Nouveau projet](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="50754-123">[![The New Project dialog box](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)</span></span>

<span data-ttu-id="50754-124">**Figure 01**: voir une page éclater ([cliquez pour afficher l’image en taille réelle](creating-a-route-constraint-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="50754-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-vb/_static/image2.png))</span></span>


<span data-ttu-id="50754-125">Vous souhaitez vraiment faire est uniquement en correspondance les URL qui contiennent un productId entier approprié.</span><span class="sxs-lookup"><span data-stu-id="50754-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="50754-126">Vous pouvez utiliser une contrainte lors de la définition d’un itinéraire pour restreindre les URL qui correspondent à l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="50754-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="50754-127">La gamme de produit modifiée dans la liste 3 contient une contrainte d’expression régulière qui correspond uniquement à des entiers.</span><span class="sxs-lookup"><span data-stu-id="50754-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="50754-128">**La liste 3 - Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="50754-128">**Listing 3 - Global.asax.vb**</span></span>

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="50754-129">L’expression régulière \d+ correspond à un ou plusieurs des entiers.</span><span class="sxs-lookup"><span data-stu-id="50754-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="50754-130">Cette contrainte entraîne la gamme de produits faire correspondre les URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="50754-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="50754-131">/ Produit/3</span><span class="sxs-lookup"><span data-stu-id="50754-131">/Product/3</span></span>
- <span data-ttu-id="50754-132">/ Produits/8999</span><span class="sxs-lookup"><span data-stu-id="50754-132">/Product/8999</span></span>

<span data-ttu-id="50754-133">Mais pas les URL suivantes :</span><span class="sxs-lookup"><span data-stu-id="50754-133">But not the following URLs:</span></span>

- <span data-ttu-id="50754-134">/ Produits/apple</span><span class="sxs-lookup"><span data-stu-id="50754-134">/Product/apple</span></span>
- <span data-ttu-id="50754-135">/ Produit</span><span class="sxs-lookup"><span data-stu-id="50754-135">/Product</span></span>

<span data-ttu-id="50754-136">Ces demandes du navigateur seront gérées par un autre itinéraire ou, si aucun itinéraire correspondant, un *Impossible de trouver la ressource* erreur est renvoyée.</span><span class="sxs-lookup"><span data-stu-id="50754-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="50754-137">[Précédent](creating-custom-routes-vb.md)
[Suivant](creating-a-custom-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="50754-137">[Previous](creating-custom-routes-vb.md)
[Next](creating-a-custom-route-constraint-vb.md)</span></span>