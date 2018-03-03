---
title: Migration de la Configuration
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/configuration
ms.openlocfilehash: e1ee582072c88542565c5cb860e157afe137f9f0
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/02/2018
---
# <a name="migrating-configuration"></a>Migration de la Configuration

Par [Steve Smith](https://ardalis.com/) et [Scott Addie](https://scottaddie.com)

Dans l’article précédent, nous avons commencé [vers un projet ASP.NET MVC ASP.NET MVC de base](mvc.md). Dans cet article, nous migrer la configuration.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="setup-configuration"></a>Configuration de l’installation

ASP.NET Core n’utilise plus le *Global.asax* et *web.config* fichiers utilisées par les versions précédentes d’ASP.NET. Dans les versions antérieures d’ASP.NET, la logique de démarrage d’application a été placée dans un `Application_StartUp` méthode *Global.asax*. Ensuite, dans ASP.NET MVC, un *Startup.cs* fichier a été inclus dans la racine du projet ; et, elle a été appelée au démarrage de l’application. ASP.NET Core a arrêté complètement cette approche, en plaçant toute logique de démarrage dans le *Startup.cs* fichier.

Le *web.config* fichier a également été remplacé dans ASP.NET Core. Configuration proprement dite peut maintenant être configurée, dans le cadre de la procédure de démarrage d’application décrite dans *Startup.cs*. Configuration peut toujours utiliser des fichiers XML, mais en général, les projets ASP.NET Core place les valeurs de configuration dans un fichier au format JSON, tel que *appsettings.json*. Système de configuration d’ASP.NET Core également facilement accessible variables d’environnement, ce qui peuvent fournir un [plus un emplacement sécurisé et robuste](xref:security/app-secrets) pour valeurs spécifiques à l’environnement. Cela est particulièrement vrai pour les clés secrètes comme des chaînes de connexion et les clés API qui ne doivent pas être archivés dans le contrôle de code source. Consultez [Configuration](xref:fundamentals/configuration/index) pour en savoir plus sur la configuration dans ASP.NET Core.

Pour cet article, nous avons commencé avec le projet ASP.NET Core partiellement migré à partir de [l’article précédent](mvc.md). Pour installer la configuration, ajoutez le constructeur et la propriété suivante le *Startup.cs* fichier situé à la racine du projet :

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

Notez qu’à ce stade, le *Startup.cs* fichier ne sera pas compilé, qu’il faut toujours ajouter les éléments suivants `using` instruction :

```csharp
using Microsoft.Extensions.Configuration;
```

Ajouter un *appsettings.json* fichier à la racine du projet à l’aide du modèle d’élément approprié :

![Ajouter AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Migrer les paramètres de Configuration à partir de web.config

Notre projet ASP.NET MVC incluse de la chaîne de connexion de base de données requises dans *web.config*, dans le `<connectionStrings>` élément. Dans notre projet ASP.NET Core, nous allons stocker ces informations dans le *appsettings.json* fichier. Ouvrez *appsettings.json*et notez qu’il contient déjà les éléments suivants :

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


Dans la ligne en surbrillance mentionnée ci-dessus, remplacez le nom de la base de données à partir de **_CHANGE_ME** au nom de votre base de données.

## <a name="summary"></a>Récapitulatif

ASP.NET Core place toute logique de démarrage de l’application dans un seul fichier dans lequel les services nécessaires et les dépendances peuvent être définis et configurés. Il remplace le *web.config* fichier avec une fonctionnalité de configuration souples qui peut tirer parti des divers formats de fichier, tels que JSON, ainsi que les variables d’environnement.
