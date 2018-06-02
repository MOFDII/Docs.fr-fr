---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: Présentation du routage ASP.NET MVC (c#) | Documents Microsoft
author: StephenWalther
description: Dans ce didacticiel, Stephen Walther montre comment l’infrastructure ASP.NET MVC mappe les demandes du navigateur aux actions de contrôleur.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: fa565d2ef253539844f5224df00bdcdc047bb3f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-routing-overview-c"></a>Présentation du routage ASP.NET MVC (c#)
====================
par [Stephen Walther](https://github.com/StephenWalther)

> Dans ce didacticiel, Stephen Walther montre comment l’infrastructure ASP.NET MVC mappe les demandes du navigateur aux actions de contrôleur.


Dans ce didacticiel, vous sont présentés dans une fonctionnalité importante de toutes les applications ASP.NET MVC appelée *le routage ASP.NET*. Le module de routage ASP.NET est chargé pour le mappage des demandes entrantes de navigateur pour les actions de contrôleur MVC particuliers. À la fin de ce didacticiel, vous saurez comment la table de routage standard mappe les demandes aux actions de contrôleur.

## <a name="using-the-default-route-table"></a>À l’aide de la Table de l’itinéraire par défaut

Lorsque vous créez une application ASP.NET MVC, l’application est déjà configurée pour utiliser le routage ASP.NET. Routage d’ASP.NET est configuré dans deux emplacements.

Tout d’abord, le routage ASP.NET est activé dans le fichier de configuration de votre application Web (fichier Web.config). Il existe quatre sections dans le fichier de configuration qui sont pertinentes pour le routage : la section system.web.httpModules, la section system.web.httpHandlers, la section system.webserver.modules et la section system.webserver.handlers. Veillez à ne pas supprimer ces sections, car sans ces sections routage ne fonctionnera plus.

Ensuite et plus important encore, une table de routage est créée dans le fichier Global.asax de l’application. Le fichier Global.asax est un fichier spécial qui contient les gestionnaires d’événements pour les événements de cycle de vie des applications ASP.NET. La table de routage est créée lors de l’événement de démarrer l’Application.

Le fichier dans la liste 1 contient le fichier Global.asax de valeur par défaut pour une application ASP.NET MVC.

**La liste 1 - Global.asax.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

Quand une application MVC commence, l’Application\_méthode Start() est appelée. Cette méthode appelle à son tour, la méthode RegisterRoutes(). La méthode RegisterRoutes() crée la table d’itinéraires.

La table d’itinéraires par défaut contient un seul itinéraire (nommé par défaut). L’itinéraire par défaut mappe le premier segment d’URL pour un nom de contrôleur, le deuxième segment d’URL à une action de contrôleur et le troisième segment dans un paramètre nommé **id**.

Imaginez que vous entrez l’URL suivante dans la barre d’adresse de votre navigateur web :

/Home/Index/3

L’itinéraire par défaut est mappé à cette URL pour les paramètres suivants :

- contrôleur = Home

- action = Index

- id = 3

Lorsque vous demandez l’URL /Home/Index/3, le code suivant est exécuté :

HomeController.Index(3)

L’itinéraire par défaut inclut les valeurs par défaut pour tous les trois paramètres. Si vous ne fournissez pas un contrôleur, puis le paramètre contrôleur par défaut la valeur **accueil**. Si vous ne fournissez pas une action, le paramètre d’action par défaut la valeur **Index**. Enfin, si vous ne fournissez pas un id, le paramètre id par défaut est une chaîne vide.

Examinons quelques exemples de la façon dont l’itinéraire par défaut mappe aux actions de contrôleur. Imaginez que vous entrez l’URL suivante dans la barre d’adresse de votre navigateur :

/ Home

En raison de l’itinéraire par défaut, paramètres par défaut entrer cette URL entraîne la méthode Index() de la classe HomeController dans la liste de 2 à appeler.

**Listing 2 - HomeController.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

Dans la liste 2, la classe HomeController inclut une méthode nommée Index() qui accepte un paramètre unique nommé ID. L’URL /Home entraîne la méthode Index() être appelée avec une chaîne vide comme valeur du paramètre Id.

En raison de la manière que l’infrastructure MVC appelle les actions de contrôleur, l’URL /Home correspond également à la méthode Index() de la classe HomeController dans la liste 3.

**La liste 3 - HomeController.cs (action Index sans paramètre)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

La méthode Index() dans la liste 3 n’accepte pas les paramètres. L’URL /Home provoquera cette méthode Index() à appeler. L’URL /Home/Index/3 appelle également cette méthode (l’Id est ignoré).

L’URL /Home correspond également à la méthode Index() de la classe HomeController sur la liste 4.

**La liste 4 - HomeController.cs (action Index avec le paramètre accepte les valeurs NULL)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

Dans la liste 4, la méthode Index() a un paramètre de type entier. Étant donné que le paramètre est un paramètre nullable (peut avoir pas la valeur Null), le Index() peut être appelé sans déclencher d’une erreur.

Enfin, l’appel à la méthode Index() dans la liste de 5 avec l’URL /Home entraîne une exception depuis le paramètre Id *n’est pas* un paramètre accepte les valeurs NULL. Si vous tentez d’appeler la méthode Index() puis vous obtenez l’erreur affichée dans la Figure 1.

**La liste 5 - HomeController.cs (action Index avec le paramètre Id)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


[![Appel d’une action de contrôleur qui attend une valeur de paramètre](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)

**Figure 01**: appel d’une action de contrôleur qui attend une valeur de paramètre ([cliquez pour afficher l’image en taille réelle](asp-net-mvc-routing-overview-cs/_static/image2.png))


L’URL /Home/Index/3, quant à eux, fonctionne parfaitement avec l’action de contrôleur d’Index dans la liste 5. La demande /Home/Index/3 entraîne la méthode Index() être appelé avec un paramètre d’Id qui a la valeur 3.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel est de vous fournir une brève introduction au routage ASP.NET. Nous avons examiné la table d’itinéraires par défaut que vous obtenez avec une application ASP.NET MVC. Vous avez appris comment l’itinéraire par défaut mappe des URL aux actions de contrôleur.

> [!div class="step-by-step"]
> [Next](understanding-action-filters-cs.md)
