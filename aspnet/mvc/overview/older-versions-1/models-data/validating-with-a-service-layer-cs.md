---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Validation avec une couche de Service (c#) | Documents Microsoft
author: StephenWalther
description: Découvrez comment déplacer votre logique de validation en dehors de vos actions de contrôleur et dans une couche de service distinct. Dans ce didacticiel, Stephen Walther explique comment vous...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 06042ac197cc54da767a94a44c57eb09bb3db9fa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="validating-with-a-service-layer-c"></a>Validation avec une couche de Service (c#)
====================
par [Stephen Walther](https://github.com/StephenWalther)

> Découvrez comment déplacer votre logique de validation en dehors de vos actions de contrôleur et dans une couche de service distinct. Dans ce didacticiel, Stephen Walther explique comment vous pouvez maintenir une séparation AIGUE des problèmes en isolant votre couche de service à partir de votre couche de contrôleur.


L’objectif de ce didacticiel est de décrire une méthode d’effectuer la validation dans une application ASP.NET MVC. Dans ce didacticiel, vous allez apprendre à déplacer votre logique de validation en dehors de vos contrôleurs et dans une couche de service distinct.

## <a name="separating-concerns"></a>Séparation des problèmes

Lorsque vous générez une application ASP.NET MVC, vous ne devez pas placer votre logique de la base de données à l’intérieur de vos actions de contrôleur. Mélange de votre logique de la base de données et de contrôleur rend votre application plus difficile à gérer au fil du temps. Il est recommandé que vous placez toute logique de votre base de données dans une couche distincte de référentiel.

Par exemple, la liste 1 contient un référentiel simple nommé le ProductRepository. Le référentiel de produit contient toutes les le code d’accès aux données pour l’application. La liste inclut également l’interface IProductRepository implémentant le référentiel de produit.

**La liste 1--Models\ProductRepository.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

Le contrôleur dans la liste 2 utilise la couche de référentiels dans son Index() et Create() actions. Notez que ce contrôleur ne contient pas la logique de base de données. Création d’une couche de référentiel permet d’établir une séparation claire des préoccupations. Les contrôleurs sont responsables de la logique de contrôle de flux application et le référentiel est responsable de la logique d’accès aux données.

**Listing 2 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a>Création d’une couche de Service

Par conséquent, la logique de contrôle de flux application appartienne à un contrôleur d’et appartienne de logique d’accès aux données dans un référentiel. Dans ce cas, où placer votre logique de validation ? Une option consiste à placer votre logique de validation dans un *couche de service*.

Une couche de service est une couche supplémentaire dans une application ASP.NET MVC qui sert d’intermédiaire pour la communication entre un contrôleur et une couche de référentiel. La couche de service contient la logique métier. En particulier, il contient la logique de validation.

Par exemple, la couche de service de produit dans la liste 3 a une méthode CreateProduct(). La méthode CreateProduct() appelle la méthode de ValidateProduct() pour valider un nouveau produit avant de passer le produit dans le référentiel de produit.

**Listing 3 - Models\ProductService.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

Le contrôleur de produit a été mis à jour dans la liste de 4 à utiliser la couche de service au lieu de la couche de référentiels. La couche de contrôleur communique avec la couche de service. La couche de service communique avec la couche de référentiels. Chaque couche possède une responsabilité distincte.

**La liste 4 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

Notez que le service de produit est créé dans le constructeur de contrôleur du produit. Lorsque le service de produit est créé, le dictionnaire d’états de modèles est passé au service. Le service de produit utilise l’état du modèle pour transmettre les messages d’erreur de la validation vers le contrôleur.

## <a name="decoupling-the-service-layer"></a>Dissociation de la couche de Service

Nous avons a échoué isoler le contrôleur et les couches de service dans un point. Le contrôleur et les couches de service communiquent à l’état de modèle. En d’autres termes, la couche de service a une dépendance sur une fonctionnalité particulière de l’infrastructure ASP.NET MVC.

Vous souhaitez isoler la couche de service à partir de notre couche contrôleur autant que possible. En théorie, nous devons être en mesure d’utiliser la couche de service avec n’importe quel type d’application et non seulement une application ASP.NET MVC. Par exemple, à l’avenir, nous souhaitions générer un frontal pour notre application de WPF. Nous étudions trouver un moyen de supprimer la dépendance sur ASP.NET MVC état du modèle à partir de la couche de service.

Dans la liste 5, la couche de service a été mise à jour afin qu’il n’utilise plus le modèle d’état. Au lieu de cela, il utilise n’importe quelle classe qui implémente l’interface IValidationDictionary.

**Listing 5 - Models\ProductService.cs (decoupled)**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

L’interface IValidationDictionary est défini dans la liste 6. Cette interface simple possède une méthode unique et une seule propriété.

**La liste 6 - Models\IValidationDictionary.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

La classe dans la liste 7, la classe ModelStateWrapper, nommée implémente l’interface IValidationDictionary. Vous pouvez instancier la classe ModelStateWrapper en passant un dictionnaire d’états de modèle au constructeur.

**La liste 7 - Models\ModelStateWrapper.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

Enfin, le contrôleur de mise à jour dans la liste 8 utilise le ModelStateWrapper lors de la création de la couche de service dans son constructeur.

**Liste 8 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

À l’aide de la IValidationDictionary interface et la classe ModelStateWrapper permet d’isoler totalement notre couche de service à partir de la couche de notre contrôleur. La couche de service n’est plus dépendante de l’état du modèle. Vous pouvez passer n’importe quelle classe qui implémente l’interface IValidationDictionary à la couche de service. Par exemple, une application WPF peut implémenter l’interface IValidationDictionary avec une classe de collection simple.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel a pour présenter une approche pour effectuer la validation dans une application ASP.NET MVC. Dans ce didacticiel, vous avez appris comment déplacer tout votre logique de validation en dehors de vos contrôleurs et dans une couche de service distinct. Vous avez également appris comment isoler votre couche de service à partir de votre couche de contrôleur en créant une classe ModelStateWrapper.

> [!div class="step-by-step"]
> [Précédent](validating-with-the-idataerrorinfo-interface-cs.md)
> [Suivant](validation-with-the-data-annotation-validators-cs.md)
