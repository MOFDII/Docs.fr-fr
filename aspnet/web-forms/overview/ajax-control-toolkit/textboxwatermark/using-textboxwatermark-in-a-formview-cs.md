---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
title: À l’aide de TextBoxWatermark dans un FormView (c#) | Documents Microsoft
author: wenz
description: Le contrôle TextBoxWatermark dans la boîte à outils de contrôle AJAX étend une zone de texte afin qu’un texte s’affiche dans la zone. Lorsqu’un utilisateur clique dans la zone, il vous...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6ee90bf-32a5-4987-a384-15cc7dd30c8a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
msc.type: authoredcontent
ms.openlocfilehash: 90e532d33756a995a85994254c48889517c5bb8d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870776"
---
<a name="using-textboxwatermark-in-a-formview-c"></a>À l’aide de TextBoxWatermark dans un FormView (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1CS.pdf)

> Le contrôle TextBoxWatermark dans la boîte à outils de contrôle AJAX étend une zone de texte afin qu’un texte s’affiche dans la zone. Lorsqu’un utilisateur clique dans la zone, elle est vidée. Si l’utilisateur quitte la zone de sans saisie de texte, le texte prérempli s’affiche de nouveau. Cela est également possible dans un contrôle FormView.


## <a name="overview"></a>Vue d'ensemble

Le `TextBoxWatermark` contrôle dans la boîte à outils de contrôle AJAX étend une zone de texte afin qu’un texte s’affiche dans la zone. Lorsqu’un utilisateur clique dans la zone, elle est vidée. Si l’utilisateur quitte la zone de sans saisie de texte, le texte prérempli s’affiche de nouveau. Cela est également possible dans un `FormView` contrôle.

## <a name="steps"></a>Étapes

Tout d’abord, une source de données est requise. Cet exemple utilise la base de données AdventureWorks et Microsoft SQL Server 2005 Express Edition. La base de données est une partie facultative d’une installation de Visual Studio (y compris l’édition expresse) et est également disponible en tant que téléchargement séparé sous [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). La base de données AdventureWorks fait partie des exemples de SQL Server 2005 et exemples de bases de données (téléchargement à l’adresse [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Pour configurer la base de données le plus simple consiste à utiliser le Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = fr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) et d’attacher le `AdventureWorks.mdf` le fichier de base de données.

Pour cet exemple, nous partons du principe que l’instance de SQL Server 2005 Express Edition est appelée `SQLEXPRESS` et réside sur le même ordinateur que le serveur web ; il s’agit également de la configuration par défaut. Si votre programme d’installation est différente, vous devez adapter les informations de connexion pour la base de données.

Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample1.aspx)]

Ensuite, ajoutez une source de données à la page qui prend en charge la `DELETE`, `INSERT` et `UPDATE` instructions SQL. Si vous utilisez l’assistant de Visual Studio pour créer la source de données, à l’esprit qu’un bogue dans la version actuelle ne précède pas le nom de table (`Vendor`) avec `Purchasing`. Le balisage suivant illustre la syntaxe correcte :

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample2.aspx)]

Mémoriser le nom (`ID`) de la source de données, car il est utilisé dans le `DataSourceID` propriété de la `FormView` contrôle. Le `<InsertItemTemplate>` section de la `FormView` contient une zone de texte qui est étendu par le `TextBoxWatermarkExtender` contrôle. Assurez-vous que l’extendeur réside dans le modèle et pas en dehors de celle-ci.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample3.aspx)]

Lorsque l’utilisateur s’affiche désormais dans le mode d’insertion de la `FormView` contrôler, le champ de texte pour le nouveau fournisseur est prérempli grâce à la `TextBoxWatermarkExtender` contrôle. Un clic à l’intérieur de la zone de texte vous permet du texte de remplissage disparaissent.


[![Le filigrane dans le champ provient de l’extendeur](using-textboxwatermark-in-a-formview-cs/_static/image2.png)](using-textboxwatermark-in-a-formview-cs/_static/image1.png)

Le filigrane dans le champ provient de l’extendeur ([cliquez pour afficher l’image en taille réelle](using-textboxwatermark-in-a-formview-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-textboxwatermark-with-validation-controls-cs.md)
