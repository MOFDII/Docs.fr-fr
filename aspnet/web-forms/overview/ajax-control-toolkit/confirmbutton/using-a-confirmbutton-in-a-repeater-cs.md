---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
title: À l’aide d’un ConfirmButton dans un répéteur (c#) | Documents Microsoft
author: wenz
description: L’extendeur ConfirmButton dans la boîte à outils de contrôle AJAX crée Oui/pas de fenêtre contextuelle lorsque l’utilisateur clique sur un bouton (y compris contrôle LinkButton). Uniquement si Oui est en cours...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a973ed3e-400c-4925-ace2-0b086b479301
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 7a0717083a3c1e285f0c8c4cb8503d2bbe42153b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871517"
---
<a name="using-a-confirmbutton-in-a-repeater-c"></a><span data-ttu-id="c3ca6-104">À l’aide d’un ConfirmButton dans un répéteur (c#)</span><span class="sxs-lookup"><span data-stu-id="c3ca6-104">Using a ConfirmButton In a Repeater (C#)</span></span>
====================
<span data-ttu-id="c3ca6-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c3ca6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c3ca6-106">[Télécharger le Code](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c3ca6-106">[Download Code](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)</span></span>

> <span data-ttu-id="c3ca6-107">L’extendeur ConfirmButton dans la boîte à outils de contrôle AJAX crée Oui/pas de fenêtre contextuelle lorsque l’utilisateur clique sur un bouton (y compris contrôle LinkButton).</span><span class="sxs-lookup"><span data-stu-id="c3ca6-107">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="c3ca6-108">Uniquement si l’utilisateur est cliqué sur Oui, l’action du bouton est exécutée, sinon annulée.</span><span class="sxs-lookup"><span data-stu-id="c3ca6-108">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="c3ca6-109">Cela est également possible dans un répéteur.</span><span class="sxs-lookup"><span data-stu-id="c3ca6-109">This is also possible in a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="c3ca6-110">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="c3ca6-110">Overview</span></span>

<span data-ttu-id="c3ca6-111">L’extendeur ConfirmButton dans la boîte à outils de contrôle AJAX crée Oui/pas de fenêtre contextuelle lorsque l’utilisateur clique sur un bouton (y compris contrôle LinkButton).</span><span class="sxs-lookup"><span data-stu-id="c3ca6-111">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="c3ca6-112">Uniquement si l’utilisateur est cliqué sur Oui, l’action du bouton est exécutée, sinon annulée.</span><span class="sxs-lookup"><span data-stu-id="c3ca6-112">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="c3ca6-113">Cela est également possible dans un répéteur.</span><span class="sxs-lookup"><span data-stu-id="c3ca6-113">This is also possible in a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="c3ca6-114">Étapes</span><span class="sxs-lookup"><span data-stu-id="c3ca6-114">Steps</span></span>

<span data-ttu-id="c3ca6-115">Tout d’abord, une source de données est requise.</span><span class="sxs-lookup"><span data-stu-id="c3ca6-115">First of all, a data source is required.</span></span> <span data-ttu-id="c3ca6-116">Cet exemple utilise la base de données AdventureWorks et Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="c3ca6-116">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="c3ca6-117">La base de données est une partie facultative d’une installation de Visual Studio (y compris l’édition expresse) et est également disponible en tant que téléchargement séparé sous [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="c3ca6-117">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="c3ca6-118">La base de données AdventureWorks fait partie des exemples de SQL Server 2005 et exemples de bases de données (téléchargement à l’adresse [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="c3ca6-118">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="c3ca6-119">Pour configurer la base de données le plus simple consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) et d’attacher le `AdventureWorks.mdf` le fichier de base de données.</span><span class="sxs-lookup"><span data-stu-id="c3ca6-119">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="c3ca6-120">Pour cet exemple, nous partons du principe que l’instance de SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et réside sur le même ordinateur que le serveur web ; il s’agit également de la configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="c3ca6-120">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="c3ca6-121">Si votre programme d’installation est différente, vous devez adapter les informations de connexion pour la base de données.</span><span class="sxs-lookup"><span data-stu-id="c3ca6-121">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="c3ca6-122">Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :</span><span class="sxs-lookup"><span data-stu-id="c3ca6-122">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample1.aspx)]

<span data-ttu-id="c3ca6-123">Ensuite, une source de données est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="c3ca6-123">Then, a data source is required.</span></span> <span data-ttu-id="c3ca6-124">Par souci de simplicité, uniquement les cinq premières entrées dans la table de fournisseurs AdventureWorks sont récupérées.</span><span class="sxs-lookup"><span data-stu-id="c3ca6-124">For the sake of simplicity, only the first five entries in AdventureWorks' Vendors table are retrieved.</span></span> <span data-ttu-id="c3ca6-125">Notez que lors de l’utilisation de l’Assistant Visual Studio pour créer la source de données, le nom de table (`Vendors`) est actuellement pas correctement préfixés avec `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="c3ca6-125">Note that when using the Visual Studio wizard to create the data source, the table name (`Vendors`) is currently not correctly prefixed with `Purchasing`.</span></span> <span data-ttu-id="c3ca6-126">Le balisage suivant est correct :</span><span class="sxs-lookup"><span data-stu-id="c3ca6-126">The following markup is the correct one:</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample2.aspx)]

<span data-ttu-id="c3ca6-127">Cette source de données peut ensuite servir dans un répéteur.</span><span class="sxs-lookup"><span data-stu-id="c3ca6-127">This data source can then be used within a repeater.</span></span> <span data-ttu-id="c3ca6-128">Comme d’habitude, le `DataBinder.Eval()` méthode récupère des données à partir de la source de données.</span><span class="sxs-lookup"><span data-stu-id="c3ca6-128">As usual, the `DataBinder.Eval()` method retrieves data from the data source.</span></span> <span data-ttu-id="c3ca6-129">Le `ConfirmButtonExtender` contrôle puis doit être placé dans la `<ItemTemplate>` section du répéteur afin qu’elle s’affiche pour chaque entrée de la source de données.</span><span class="sxs-lookup"><span data-stu-id="c3ca6-129">The `ConfirmButtonExtender` control must then be placed within the `<ItemTemplate>` section of the repeater so that it appears for every entry in the data source.</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample3.aspx)]


<span data-ttu-id="c3ca6-130">[![Le bouton de confirmation s’affiche en regard de chaque entrée de la source de données](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c3ca6-130">[![The confirm button appears next to each entry from the data source](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)</span></span>

<span data-ttu-id="c3ca6-131">Le bouton de confirmation s’affiche en regard de chaque entrée de la source de données ([cliquez pour afficher l’image en taille réelle](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c3ca6-131">The confirm button appears next to each entry from the data source ([Click to view full-size image](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c3ca6-132">Next</span><span class="sxs-lookup"><span data-stu-id="c3ca6-132">Next</span></span>](using-a-confirmbutton-in-a-repeater-vb.md)
