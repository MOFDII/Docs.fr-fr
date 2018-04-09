---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Création d’une Action (VB) | Documents Microsoft
author: microsoft
description: Découvrez comment ajouter une nouvelle action à un contrôleur ASP.NET MVC. En savoir plus sur la configuration requise pour une méthode d’action.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: c77e4738444c61d60bdd78a50b36f98be41fc271
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="creating-an-action-vb"></a>Création d’une Action (VB)
====================
par [Microsoft](https://github.com/microsoft)

> Découvrez comment ajouter une nouvelle action à un contrôleur ASP.NET MVC. En savoir plus sur la configuration requise pour une méthode d’action.


L’objectif de ce didacticiel est d’expliquer comment vous pouvez créer une nouvelle action de contrôleur. Vous en savoir plus sur la configuration requise d’une méthode d’action. Vous apprenez également à empêcher une méthode d’être exposé en tant qu’action.

## <a name="adding-an-action-to-a-controller"></a>Ajout d’une Action à un contrôleur

Vous ajoutez une nouvelle action à un contrôleur en ajoutant une nouvelle méthode au contrôleur. Par exemple, le contrôleur dans la liste 1 contient une action nommée Index() et qu’une action nommée SayHello(). Les deux méthodes sont exposées en tant qu’actions.

**Listing 1 - Controllers\HomeController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

Pour pouvoir être exposés à l’univers en tant qu’action, une méthode doit remplir certaines conditions :

- La méthode doit être publique.
- La méthode ne peut pas être une méthode statique.
- La méthode ne peut pas être une méthode d’extension.
- La méthode ne peut pas être un constructeur, getter ou setter.
- La méthode ne peut pas avoir de types génériques ouverts.
- La méthode n’est pas une méthode de la classe de base du contrôleur.
- La méthode ne peut pas contenir **ref** ou **hors** paramètres.

Notez qu’il n’existe aucune restriction sur le type de retour d’une action de contrôleur. Une action de contrôleur peut retourner une chaîne, une valeur DateTime, une instance de la classe Random, ou void. L’infrastructure ASP.NET MVC convertit tout type de retour n’est pas un résultat d’action dans une chaîne et restituer la chaîne dans le navigateur.

Lorsque vous ajoutez une méthode qui ne viole pas ces exigences à un contrôleur, la méthode est exposée comme une action de contrôleur. Soyez prudent ici. Une action de contrôleur peut être appelée par toute personne connectée à Internet. Ne créez pas le cas, par exemple, une action de contrôleur DeleteMyWebsite().

## <a name="preventing-a-public-method-from-being-invoked"></a>Empêche une méthode publique à partir de l’appelé

Si vous devez créer une méthode publique dans une classe de contrôleur et vous ne souhaitez pas exposer la méthode comme une action de contrôleur, vous pouvez empêcher l’appelé à l’aide de la méthode la &lt;NonAction&gt; attribut. Par exemple, le contrôleur dans la liste 2 contient une méthode publique nommée CompanySecrets() est décorée avec le &lt;NonAction&gt; attribut.

**Listing 2 - Controllers\WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Si vous tentez d’appeler l’action du contrôleur CompanySecrets() en tapant /Work/CompanySecrets dans la barre d’adresses de votre navigateur, vous allez récupérer le message d’erreur dans la Figure 1.


[![Appel d’une méthode NonAction](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Figure 01**: appeler une méthode NonAction ([cliquez pour afficher l’image en taille réelle](creating-an-action-vb/_static/image2.png))

> [!div class="step-by-step"]
> [Précédent](creating-a-controller-vb.md)
> [Suivant](aspnet-mvc-controllers-overview-cs.md)
