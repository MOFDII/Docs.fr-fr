---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Remplissage d’une liste à l’aide de CascadingDropDown (c#) | Documents Microsoft
author: wenz
description: Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à une charge de DropDownList associés à des valeurs dans anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: c9e47f6484e49013004bf15084f98440ee67558e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="filling-a-list-using-cascadingdropdown-c"></a>Remplissage d’une liste à l’aide de CascadingDropDown (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à une charge de DropDownList associés à des valeurs dans une autre DropDownList. (Par exemple, une seule liste fournit une liste des États, et la liste suivante est ensuite remplie de grandes villes dans cet état.) Le premier test pour résoudre est réellement remplir une liste déroulante à l’aide de ce contrôle.


## <a name="overview"></a>Vue d'ensemble

Le contrôle CascadingDropDown dans la boîte à outils de contrôle AJAX étend un contrôle DropDownList afin que les modifications apportées à une charge de DropDownList associés à des valeurs dans une autre DropDownList. (Par exemple, une seule liste fournit une liste des États, et la liste suivante est ensuite remplie de grandes villes dans cet état.) Le premier test pour résoudre est réellement remplir une liste déroulante à l’aide de ce contrôle.

## <a name="steps"></a>Étapes

Pour activer la fonctionnalité d’ASP.NET AJAX et les outils de contrôle, le `ScriptManager` contrôle doit être placé n’importe où sur la page (mais dans le `<form>` élément) :

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

Ensuite, un contrôle DropDownList est nécessaire :

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

Pour cette liste, un extendeur CascadingDropDown est ajouté. Il envoie une demande asynchrone à un service web qui retourne ensuite une liste d’entrées à afficher dans la liste. Pour ce faire, les attributs CascadingDropDown suivants doivent être définis :

- `ServicePath`: URL du service web fournir les entrées de liste
- `ServiceMethod`: Méthode web fournir les entrées de liste
- `TargetControlID`: ID de la liste déroulante
- `Category`: Les informations de catégorie qui sont soumises à la méthode web lorsqu’elle est appelée
- `PromptText`: Texte à afficher lorsque le chargement asynchrone des données de liste à partir du serveur

Voici le balisage de la `CascadingDropDown` élément. La seule différence entre c# et VB est le nom du service web associé :

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

Le code JavaScript en provenance de le `CascadingDropDown` extendeur appelle une méthode de service web avec la signature suivante :

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

Par conséquent, l’aspect important est que la méthode doit retourner un tableau de type `CascadingDropDownNameValue` (définie par les outils de contrôle ASP.NET AJAX). Dans le `CascadingDropDownNameValue` constructeur, le premier texte de l’entrée liste, puis sa valeur doivent être fournies, tout comme `<option value="VALUE">NAME</option>` dans HTML. Voici quelques exemples de données :

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

Chargement de la page dans le navigateur déclenche la liste à remplir avec les trois fournisseurs.


[![La liste est remplie automatiquement](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

La liste est remplie automatiquement ([cliquez pour afficher l’image en taille réelle](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-cascadingdropdown-with-a-database-cs.md)
