---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Modification des Animations à partir du serveur (c#) | Documents Microsoft
author: wenz
description: Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent également...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 6946875552c885ffb1f2a2eb7e728b85d7dd3973
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="modifying-animations-from-the-server-side-c"></a>Modification des Animations à partir du serveur (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent également être modifiées sur le côté serveur


## <a name="overview"></a>Vue d'ensemble

Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. Les animations peuvent également être modifiées sur le côté serveur

## <a name="steps"></a>Étapes

Tout d’abord, incluez le `ScriptManager` dans la page ; ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser les outils de contrôle :

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

L’animation sera appliquée à un volet de texte qui ressemble à ceci :

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

Dans la classe CSS associée pour le panneau de configuration, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau de configuration :

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

Le reste du code s’exécute sur le côté serveur et n’utilise pas de balisage ; au lieu de cela, il utilise le code pour créer le `AnimationExtender` contrôle :

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

Toutefois, les outils de contrôle actuellement ne fournit pas un accès à l’API pour créer des animations. Il est toutefois possible de définir la `AnimationExtender`propriété Animations en une chaîne qui contient le balisage XML utilisé lors de l’attribution de façon déclarative les animations. Pour créer le code XML qui ne doit pas contenir le `<Animations>` élément que vous pouvez utiliser les XML du .NET Framework prend en charge ou, comme dans le code suivant, juste fournir la chaîne :

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

Enfin, ajoutez le `AnimationExtender` dans le contrôle à la page active, le `<form runat="server">` élément, en veillant à ce que l’animation est incluse et s’exécute :

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![L’animation est créée à l’aide de code en C# /Visual Basic côté serveur](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

L’animation est créée à l’aide de code en C# /Visual Basic côté serveur ([cliquez pour afficher l’image en taille réelle](modifying-animations-from-the-server-side-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](triggering-an-animation-in-another-control-cs.md)
> [Suivant](executing-animations-using-client-side-code-cs.md)
