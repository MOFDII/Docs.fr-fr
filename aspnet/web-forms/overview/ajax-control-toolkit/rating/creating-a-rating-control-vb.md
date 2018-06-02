---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Création d’un contrôle d’évaluation (VB) | Documents Microsoft
author: wenz
description: De nombreux sites Web, de commerce électronique pour les sites de Communauté, offrent à leurs utilisateurs aux articles de taux ou aux éléments. Cela nécessite généralement des efforts de codage, mais nous avons le...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c19f36dfe1b72a3954db61ff1845e99c02e47c14
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-rating-control-vb"></a>Création d’un contrôle d’évaluation (VB)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)

> De nombreux sites Web, de commerce électronique pour les sites de Communauté, offrent à leurs utilisateurs aux articles de taux ou aux éléments. Cela nécessite généralement des efforts de codage, mais nous avons les outils de contrôle à notre disposition.


## <a name="overview"></a>Vue d'ensemble

De nombreux sites Web, de commerce électronique pour les sites de Communauté, offrent à leurs utilisateurs aux articles de taux ou aux éléments. Cela nécessite généralement des efforts de codage, mais nous avons les outils de contrôle à notre disposition.

## <a name="steps"></a>Étapes

Tout d’abord, vous avez besoin (au moins) deux types d’images : un pour remplie à l’élément de contrôle d’accès et pour un élément de contrôle d’accès vide. Un élément de contrôle d’accès est généralement une étoile ou une émoticône. Pour ce scénario, vous trouver trois fichiers, smiley.png et empty.png et émoticône-done.png dans le cadre des téléchargements de code source pour ce didacticiel.

Ensuite, créez un nouveau fichier ASP.NET et commencez par ajouter un `ScriptManager` contrôle à celui-ci :

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

Ensuite, ajoutez le `Rating` contrôle à partir de la boîte à outils de contrôle ASP.NET AJAX. Les attributs suivants doivent être définis pour cet exemple :

- `CurrentRating` l’évaluation initiale à utiliser
- `MaxRating` la classification maximale
- `EmptyStarCssClass` la classe CSS à utiliser lorsqu’un élément de contrôle d’accès (en étoile) est vide
- `FilledStarCssClass` la classe CSS à utiliser lorsqu’un élément de contrôle d’accès (en étoile) est renseignée.
- `StarCssClass` la classe CSS à utiliser pour un état visible
- `WaitingStarCssClass` la classe CSS à utiliser pendant un nombre d’étoiles est envoyé sur le serveur

Et Voici le balisage qui crée un contrôle d’évaluation avec cinq éléments (smileys) dont aucun n’est remplie initialement :

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

Les trois classes CSS référencés devant maintenant afficher les fichiers d’image appropriée, qui est facile à effectuer à l’aide de CSS :

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

Assurez-vous que vous fournissez la largeur et la hauteur des trois images, sinon l’affichage peut sembler un peu tordue.

Enfin, le résultat de l’évaluation doit être affiché à l’utilisateur (ou, au moins enregistré dans une base de données). Par conséquent, ajoutez une étiquette pour la sortie d’un message texte et un bouton Envoyer le formulaire d’évaluation sur le serveur de publication :

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

Dans le code côté serveur, accéder au contrôle de l’évaluation via son `ID` , puis accédez à son `CurrentRating` propriété qui correspond au nombre des éléments de contrôle d’accès sélectionné, dans notre exemple, une valeur comprise entre 0 et 5.

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

Enregistrez la page et chargez-le dans votre navigateur. Lorsque vous pointez sur les éléments de contrôle d’accès (vide), un effet JavaScript se produit : les modifications de contrôle d’accès. Lorsque vous cliquez sur l’ensemble des étoiles, l’évaluation actuelle est conservée. Enfin, lorsque vous envoyez le formulaire, le code côté serveur génère le classement sélectionné.


[![Création d’un système d’évaluation avec un minimum de code](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)

Création d’un système d’évaluation avec un minimum de code ([cliquez pour afficher l’image en taille réelle](creating-a-rating-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Précédent](creating-a-rating-control-cs.md)
