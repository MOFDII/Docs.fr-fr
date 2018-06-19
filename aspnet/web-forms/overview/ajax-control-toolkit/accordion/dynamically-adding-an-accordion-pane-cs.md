---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Ajout dynamique d’un volet Accordéon (c#) | Documents Microsoft
author: wenz
description: Le contrôle Accordéon dans la boîte à outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur afficher l’un d'entre eux à la fois. Panneaux est généralement déclaré w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: ad2fc6ea3d527215c0226f3f594d781163d538b5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879463"
---
<a name="dynamically-adding-an-accordion-pane-c"></a>Ajout dynamique d’un volet Accordéon (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> Le contrôle Accordéon dans la boîte à outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur afficher l’un d'entre eux à la fois. Panneaux est généralement déclaré dans la page elle-même, mais le code côté serveur peut être utilisé pour obtenir le même résultat.


## <a name="overview"></a>Vue d'ensemble

Le contrôle Accordéon dans la boîte à outils de contrôle AJAX fournit plusieurs volets et permet à l’utilisateur afficher l’un d'entre eux à la fois. Panneaux est généralement déclaré dans la page elle-même, mais le code côté serveur peut être utilisé pour obtenir le même résultat.

## <a name="steps"></a>Étapes

Le contrôle Accordéon expose toutes les propriétés importantes pour le code côté serveur. Entre autres choses, le `Panes` propriété accorde l’accès à la collection de volets qui composent l’accordéon. Chaque volet est de type `AccordionPane`. Par conséquent, il est facile pour créer un volet de ce type :

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

Le `HeaderContainer` propriété du `AccordionPane` fournit l’accès aux contrôles ASP.NET dans la section d’en-tête du volet ; le `ContentContainer` propriété de `AccordionPane` fait de même pour la section de contenu du volet. Cela permet au code ASP.NET ajouter du contenu vers les volets :

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

Enfin, la pane(s) doit être ajouté à la `Panes` collection de l’accordéon :

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

Voici un code côté serveur complet qui ajoute deux volets à un contrôle Accordéon :

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

Le seul élément manquant est Accordéon lui-même, ce qui dépend de la présence de ASP.NET `ScriptManager` contrôle :

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

Pour terminer l’exemple, les deux classes CSS référencés dans le contrôle Accordéon fournissent des informations de style pour le navigateur :

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


[![Les données dans l’accordéon a été ajoutées dynamiquement par le code côté serveur](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

Les données dans l’accordéon a été ajoutées dynamiquement par le code côté serveur ([cliquez pour afficher l’image en taille réelle](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](databinding-to-an-accordion-cs.md)
> [Suivant](databinding-to-an-accordion-vb.md)
