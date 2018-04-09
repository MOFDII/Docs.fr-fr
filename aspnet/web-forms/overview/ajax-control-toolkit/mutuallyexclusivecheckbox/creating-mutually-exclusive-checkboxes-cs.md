---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Création de cases à cocher s’excluent mutuellement (c#) | Documents Microsoft
author: wenz
description: 'Lorsque seul un ensemble d’options peut être sélectionné, cases d’option sont généralement utilisées. Il existe cependant un inconvénient, : une fois qu’une case d’option dans un groupe est sélectionnée,...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: c3a5abe7d02ace16f4aaad8d4adfbd0cba8e84ef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="f2b37-104">Création de cases à cocher s’excluent mutuellement (c#)</span><span class="sxs-lookup"><span data-stu-id="f2b37-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>
====================
<span data-ttu-id="f2b37-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f2b37-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f2b37-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f2b37-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="f2b37-107">Lorsque seul un ensemble d’options peut être sélectionné, cases d’option sont généralement utilisées.</span><span class="sxs-lookup"><span data-stu-id="f2b37-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="f2b37-108">Il existe cependant un inconvénient, : une fois qu’une case d’option dans un groupe est sélectionnée, il n’est pas possible de désactiver toutes les cases d’option.</span><span class="sxs-lookup"><span data-stu-id="f2b37-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="f2b37-109">Cases à cocher ne peut être désactivés à tout moment, toutefois, ne sont pas mutuellement exclusives.</span><span class="sxs-lookup"><span data-stu-id="f2b37-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="f2b37-110">Ce didacticiel offre le meilleur des deux approches : les cases à cocher qui s’excluent mutuellement.</span><span class="sxs-lookup"><span data-stu-id="f2b37-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="f2b37-111">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="f2b37-111">Overview</span></span>

<span data-ttu-id="f2b37-112">Lorsque seul un ensemble d’options peut être sélectionné, cases d’option sont généralement utilisées.</span><span class="sxs-lookup"><span data-stu-id="f2b37-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="f2b37-113">Il existe cependant un inconvénient, : une fois qu’une case d’option dans un groupe est sélectionnée, il n’est pas possible de désactiver toutes les cases d’option.</span><span class="sxs-lookup"><span data-stu-id="f2b37-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="f2b37-114">Cases à cocher ne peut être désactivés à tout moment, toutefois, ne sont pas mutuellement exclusives.</span><span class="sxs-lookup"><span data-stu-id="f2b37-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="f2b37-115">Ce didacticiel offre le meilleur des deux approches : les cases à cocher qui s’excluent mutuellement.</span><span class="sxs-lookup"><span data-stu-id="f2b37-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="f2b37-116">Étapes</span><span class="sxs-lookup"><span data-stu-id="f2b37-116">Steps</span></span>

<span data-ttu-id="f2b37-117">Les outils de contrôle ASP.NET AJAX contient l’extendeur MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="f2b37-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="f2b37-118">Cela permet aux programmeurs d’affecter les case à cocher à un nom de groupe (`Key` attribut).</span><span class="sxs-lookup"><span data-stu-id="f2b37-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="f2b37-119">À partir de toutes les cases à cocher dans le même groupe, une seule peut être sélectionnée à la fois.</span><span class="sxs-lookup"><span data-stu-id="f2b37-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="f2b37-120">Commençons par placer les deux cases à cocher sur une page ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f2b37-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="f2b37-121">Il peut y avoir plusieurs, mais deux d'entre eux suffit pour illustrer le principe :</span><span class="sxs-lookup"><span data-stu-id="f2b37-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="f2b37-122">Pour les deux cases à cocher, un contrôle MutuallyExclusiveCheckBoxExtender doit être mis sur la page.</span><span class="sxs-lookup"><span data-stu-id="f2b37-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="f2b37-123">Les deux attributs de clé doivent avoir la même valeur, tout comme la valeur d’attributs d’éléments de bouton radio HTML doivent être identiques pour indiquer le groupe qu'auquel ils appartiennent.</span><span class="sxs-lookup"><span data-stu-id="f2b37-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="f2b37-124">La propriété TargetControlID de l’extendeur pointe vers l’ID de la case à cocher.</span><span class="sxs-lookup"><span data-stu-id="f2b37-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="f2b37-125">Enfin, inclure ASP.NET AJAX `ScriptManager` requis par tous les éléments de la boîte à outils de contrôle ASP.NET AJAX :</span><span class="sxs-lookup"><span data-stu-id="f2b37-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="f2b37-126">Enregistrez et exécutez la page : vous pouvez vérifier et désactivez les cases à cocher, toutefois à aucun moment peut les deux cases à cocher est activées.</span><span class="sxs-lookup"><span data-stu-id="f2b37-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="f2b37-127">[![Case à cocher qu’une seule peut être vérifiée à la fois](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f2b37-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="f2b37-128">Case à cocher qu’une seule peut être vérifiée à la fois ([cliquez pour afficher l’image en taille réelle](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f2b37-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f2b37-129">Next</span><span class="sxs-lookup"><span data-stu-id="f2b37-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
