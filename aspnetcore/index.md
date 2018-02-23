---
title: "Présentation d’ASP.NET Core"
author: rick-anderson
description: "Offre une présentation d’ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 12/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: index
ms.openlocfilehash: 42bf005a29a8723233a0d9e5532827f7522f0e75
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2018
---
# <a name="aspnet-core"></a>ASP.NET Core

Par [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) et [Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core est un framework multiplateforme à hautes performances et [open source](https://github.com/aspnet/home) pour créer des applications cloud modernes et connectées à Internet. Avec ASP.NET Core, vous pouvez :

* Créer des applications et des services web, des applications [IoT](https://www.microsoft.com/internet-of-things/) et des back-ends mobiles.
* Utilisez vos outils de développement préférés sur Windows, macOS et Linux.
* Déployer dans le cloud ou localement.
* Exécuter sur [.NET Core ou .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).

## <a name="why-use-aspnet-core"></a>Pourquoi utiliser ASP.NET Core ?

Des millions de développeurs ont utilisé (et continuent d’utiliser) [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) pour créer des applications web. ASP.NET Core est une refonte d’ASP.NET 4.x, avec des modifications d’architecture qui aboutissent à un framework plus léger et modulaire.

ASP.NET Core offre les avantages suivants :

* Un scénario unifié pour créer une interface utilisateur web et des API web.
* L’intégration de [frameworks modernes, côté client](xref:client-side/index) et des workflows de développement.
* Un environnement prêt pour le cloud et basée sur des fichiers de [configuration système](xref:fundamentals/configuration/index).
* [Injection de dépendances](xref:fundamentals/dependency-injection) intégrée.
* Un pipeline de requête HTTP léger, [haute performance](https://github.com/aspnet/benchmarks) et modulaire.
* La capacité à être hébergé sur [IIS](xref:host-and-deploy/iis/index), [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache), [Docker](xref:host-and-deploy/docker/index), ou sur votre propre hébergement.
* La gestion de version des applications côte à côte lorsque la cible est [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
* Outillage qui simplifie le développement web moderne.
* Capacité à générer et à exécuter sur Windows, macOS et Linux.
* Open source et [centré sur la communauté](https://live.asp.net/).

ASP.NET Core est fourni entièrement sous forme de packages [NuGet](https://www.nuget.org/). Ceci vous permet d’optimiser votre application pour y inclure seulement les packages NuGet nécessaires. En fait, les applications ASP.NET Core 2.x ciblant .NET Core ne nécessitent qu’un [seul package NuGet](xref:fundamentals/metapackage). Les avantages des applications ayant une surface d’exposition plus petite incluent une sécurité accrue, une maintenance réduite et des performances améliorées.

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>Créer des API web et une interface utilisateur web en utilisant le modèle MVC d’ASP.NET Core

Le modèle MVC d’ASP.NET Core fournit des fonctionnalités pour créer des [API web](xref:tutorials/index#build-web-apis) et des [applications web](xref:tutorials/index#build-web-apps) :

* Le [modèle MVC (Modèle-Vue-Contrôleur)](xref:mvc/overview) permet de rendre vos API web et vos applications web [testables](testing/index.md).
* [Razor Pages](xref:mvc/razor-pages/index) (nouveauté dans ASP.NET Core 2.0) est un modèle de programmation basé sur les pages qui rend plus facile et plus productive la création d’une interface utilisateur web.
* Le [balisage Razor](xref:mvc/views/razor) fournit une syntaxe efficace pour [Razor Pages](xref:mvc/razor-pages/index) et les [vues MVC](xref:mvc/views/overview).
* Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor.
* La prise en charge intégrée de [plusieurs formats de données et de la négociation de contenu](mvc/models/formatting.md) permet à vos API web d’être utilisées par un large éventail de clients, notamment des navigateurs et des appareils mobiles.
* Le principe de la [liaison de modèle](xref:mvc/models/model-binding) permet le mappage automatiquement des données des requêtes HTTP aux paramètres des méthodes d’action.
* La [validation de modèle](xref:mvc/models/validation) effectue automatiquement la validation côté client et côté serveur.

## <a name="client-side-development"></a>Développement côté client

ASP.NET Core s’intègre parfaitement avec les frameworks et les bibliothèques populaires côté client, notamment [Angular](xref:spa/angular), [React](xref:spa/react) et [Bootstrap](xref:client-side/bootstrap). Pour plus d’informations, consultez [Développement côté client](xref:client-side/index).

## <a name="next-steps"></a>Étapes suivantes

Pour plus d'informations, reportez-vous aux ressources suivantes :

* [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start)
* [Didacticiels ASP.NET Core](xref:tutorials/index)
* [Notions de base d’ASP.NET Core](xref:fundamentals/index)
* [Le point hebdomadaire de la communauté ASP.NET](https://live.asp.net/) couvre l’avancement et les plans des équipes de développement. Il comprend de nouveaux blogs et des logiciels de tiers.
