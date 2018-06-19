---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Déclenchement d’une Animation dans un autre contrôle (c#) | Documents Microsoft
author: wenz
description: Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. En règle générale, lancer un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e94046ca70607e37c1b5ef57d5cedef67a236b94
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868111"
---
<a name="triggering-an-animation-in-another-control-c"></a>Déclenchement d’une Animation dans un autre contrôle (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. En règle générale, lancer une animation est déclenchée par l’utilisateur avec le même contrôle. Il est également possible d’interagir avec un contrôle et des animations d’un autre contrôle.


## <a name="overview"></a>Vue d'ensemble

Le contrôle de l’Animation dans la boîte à outils de contrôle ASP.NET AJAX n’est pas simplement un contrôle, mais une infrastructure entière pour ajouter des animations à un contrôle. En règle générale, lancer une animation est déclenchée par l’utilisateur avec le même contrôle. Il est également possible d’interagir avec un contrôle et des animations d’un autre contrôle.

## <a name="steps"></a>Étapes

Tout d’abord, incluez le `ScriptManager` dans la page ; ensuite, la bibliothèque ASP.NET AJAX est chargée, ce qui permet d’utiliser les outils de contrôle :

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

L’animation sera appliquée à un volet de texte qui ressemble à ceci :

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

Dans la classe CSS associée pour le panneau de configuration, définir une couleur d’arrière-plan agréable et également définir une largeur fixe pour le panneau de configuration :

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Pour démarrer l’animation du Panneau de configuration, un bouton HTML est utilisé. Notez que `<input type="button" />` est favorisée sur `<asp:Button />` , car nous ne voulons pas d’une publication (postback) lorsque l’utilisateur clique sur ce bouton.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Ensuite, ajoutez le `AnimationExtender` à la page, en fournissant une `ID`, le `TargetControlID` attribut et le texte obligatoire `runat="server"`. Il est important de définir `TargetControlID` à l’ID du bouton (l’élément déclenchant l’animation), pas à l’ID du panneau (l’élément en cours d’animation)

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

Dans la `<Animations>` nœud, place les animations comme d’habitude. Afin qu’elles soient de modifier le panneau de configuration, pas sur le bouton, définissez la `AnimationTarget` attribut pour chaque élément d’animation dans `AnimationExtender`. La valeur de `AnimationTarget` est l’ID du panneau, bien sûr. De cette façon, les animations se produisent avec le panneau de configuration, pas avec le bouton de déclenchement. Voici le `AnimationExtender` balisage pour ce scénario :

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Notez l’ordre spécial dans lequel les animations individuelles. Tout d’abord, le bouton est désactivé une fois l’animation s’exécute. Étant donné qu’aucun `AnimationTarget` d’attribut dans le `<EnableAction>` élément, cette animation est appliquée au contrôle d’origine : le bouton. Les étapes de le deux animation sont effectués en (`<Parallel>` élément). Ont tous deux leur `AnimationTarget` attributs `"Panel1"`, par conséquent, l’animation le panneau de configuration, et non sur le bouton.


[![Un clic de souris sur le bouton démarre l’animation du Panneau de configuration](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Un clic de souris sur le bouton démarre l’animation du Panneau de configuration ([cliquez pour afficher l’image en taille réelle](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](disabling-actions-during-animation-cs.md)
> [Suivant](modifying-animations-from-the-server-side-cs.md)
