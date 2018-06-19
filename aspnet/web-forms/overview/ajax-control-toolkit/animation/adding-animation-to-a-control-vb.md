---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Ajout d’une Animation à un contrôle (VB) | Documents Microsoft
author: wenz
description: Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Ce didacticiel montre comment...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3da98e478c45213875b3829e51351d03571a05b8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870633"
---
<a name="adding-animation-to-a-control-vb"></a>Ajout d’une Animation à un contrôle (VB)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Ce didacticiel montre comment configurer une animation de ce type.


## <a name="overview"></a>Vue d'ensemble

Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Ce didacticiel montre comment configurer une animation de ce type.

## <a name="steps"></a>Étapes

La première étape consiste comme d’habitude pour inclure la `ScriptManager` dans la page afin que la bibliothèque ASP.NET AJAX est chargée et les outils de contrôle peut être utilisé :

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

L’animation dans ce scénario sera appliquée à un volet de texte qui ressemble à ceci :

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

La classe CSS associée pour le panneau de configuration définit une couleur d’arrière-plan et une largeur :

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

Ensuite, nous devons le `AnimationExtender`. Après avoir entré un `ID` et classiques `runat="server"`, le `TargetControlID` attribut doit être défini pour le contrôle à animer dans notre cas, le panneau de configuration :

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

L’animation entière est appliquée de façon déclarative, en utilisant une syntaxe XML, malheureusement n’est pas entièrement pris en charge par IntelliSense de Visual Studio. Le nœud racine est `<Animations>;` au sein de ce nœud, plusieurs événements sont autorisés pour déterminer quand l’ou les animations prennent place :

- `OnClick` (clic de souris)
- `OnHoverOut` (lorsque la souris quitte un contrôle)
- `OnHoverOver` (lorsque la souris pointe sur un contrôle, l’arrêt du `OnHoverOut` animation)
- `OnLoad` (lorsque la page a été chargée)
- `OnMouseOut` (lorsque la souris quitte un contrôle)
- `OnMouseOver` (lorsque la souris pointe sur un contrôle, ne pas l’arrêt du `OnMouseOut` animation)

Le framework est fourni avec un jeu d’animations, chacun d’eux représenté par son propre élément XML. Voici une sélection :

- `<Color>` (modification d’une couleur)
- `<FadeIn>` (fondu dans)
- `<FadeOut>` (fondu out)
- `<Property>` (modification de propriété d’un contrôle)
- `<Pulse>` (impulsions)
- `<Resize>` (modification de la taille)
- `<Scale>` (modification de la taille proportionnelle)

Dans cet exemple, le panneau de configuration doit disparaître. L’animation prennent 1,5 seconde (`Duration` attribut), affichage des 24 images (étapes de l’animation) par seconde (`Fps` attributs). Voici le balisage complet pour le `AnimationExtender` contrôle :

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

Lorsque vous exécutez ce script, le panneau de configuration s’affiche et fondu dans un et demi secondes.


[![Le panneau est atténuant progressivement](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

Le panneau est atténuant progressivement ([cliquez pour afficher l’image en taille réelle](adding-animation-to-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](dynamically-controlling-updatepanel-animations-cs.md)
> [Suivant](executing-several-animations-at-the-same-time-vb.md)
