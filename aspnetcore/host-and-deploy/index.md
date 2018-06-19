---
title: Héberger et déployer ASP.NET Core
author: rick-anderson
description: Découvrez comment configurer des environnements d’hébergement et déployer des applications ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/index
ms.openlocfilehash: 1ffc7f9f2dc2a06dddb629d2d2553964b56cec05
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2018
ms.locfileid: "34472932"
---
# <a name="host-and-deploy-aspnet-core"></a>Héberger et déployer ASP.NET Core

En général, pour déployer une application ASP.NET Core sur un environnement d’hébergement :

* Publier l’application dans un dossier sur le serveur d’hébergement.
* Configurer un gestionnaire de processus qui démarre l’application à l’arrivée des requêtes et redémarre l’application en cas de blocage ou quand le serveur redémarre.
* Si la configuration d’un proxy inverse est souhaitée, configurer un proxy inverse qui transfère les demandes à l’application.

## <a name="publish-to-a-folder"></a>Publier dans un dossier

La commande CLI [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) compile le code d’application et copie les fichiers nécessaires pour exécuter l’application dans un dossier *publish*. Dans le cadre d’un déploiement à partir de Visual Studio, l’étape [dotnet publish](/dotnet/core/tools/dotnet-publish) est effectuée automatiquement avant que les fichiers ne soient copiés dans la destination du déploiement.

### <a name="folder-contents"></a>Contenu du dossier

Le dossier *publish* contient des fichiers *.exe* et *.dll* pour l’application, ses dépendances et éventuellement le runtime .NET.

Une application .NET Core peut être publiée en tant qu’application *autonome* ou *dépendante du framework*. Si l’application est autonome, les fichiers *.dll* qui contiennent le runtime .NET sont inclus dans le dossier *publish*. Si l’application dépend du framework, les fichiers du runtime .NET ne sont pas inclus, car l’application a une référence à une version de .NET installée sur le serveur. Le modèle de déploiement par défaut est « dépendante du framework ». Pour plus d’informations, consultez [Déploiement d’applications .NET Core](/dotnet/articles/core/deploying/index).

En plus des fichiers *.exe* et *.dll*, le dossier *publish* d’une application ASP.NET Core contient généralement des fichiers de configuration, des ressources statiques et des vues MVC. Pour plus d’informations, consultez [Structure des répertoires](xref:host-and-deploy/directory-structure).

## <a name="set-up-a-process-manager"></a>Configurer un gestionnaire de processus

Une application ASP.NET Core est une application console qui doit être démarrée quand un serveur démarre, et redémarrée si elle se bloque. Pour automatiser le démarrage et le redémarrage, un gestionnaire de processus est nécessaire. Les gestionnaires de processus les plus courants pour ASP.NET Core sont les suivants :

* Linux
  * [Nginx](xref:host-and-deploy/linux-nginx)
  * [Apache](xref:host-and-deploy/linux-apache)
* Windows
  * [IIS](xref:host-and-deploy/iis/index)
  * [Service Windows](xref:host-and-deploy/windows-service)

## <a name="set-up-a-reverse-proxy"></a>Configurer un proxy inverse

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Si l’application utilise le serveur web [Kestrel](xref:fundamentals/servers/kestrel), alors [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) ou [IIS](xref:host-and-deploy/iis/index) peut être utilisé comme serveur proxy inverse. Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire.

Les deux configurations&mdash;avec ou sans serveur proxy inverse&mdash;sont des configurations d’hébergement valides et prises en charge pour les applications ASP.NET Core versions 2.0 ou ultérieures. Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Si l’application utilise le serveur web [Kestrel](xref:fundamentals/servers/kestrel) et qu’elle est exposée à Internet, utilisez [Nginx](xref:host-and-deploy/linux-nginx), [Apache](xref:host-and-deploy/linux-apache) ou [IIS](xref:host-and-deploy/iis/index) comme serveur proxy inverse. Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire. La principale raison pour utiliser un proxy inverse est la sécurité. Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy).

---

## <a name="proxy-server-and-load-balancer-scenarios"></a>Scénarios avec un serveur proxy et un équilibreur de charge

Une configuration supplémentaire peut être nécessaire pour les applications hébergées derrière des serveurs proxy et des équilibreurs de charge. Sans configuration supplémentaire, une application peut ne pas avoir accès au schéma (HTTP/HTTPS) et à l’adresse IP distante d’où provient une requête. Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Utilisation de Visual Studio et MSBuild pour automatiser le déploiement

Le déploiement nécessite souvent des tâches supplémentaires en plus de la copie de la sortie de [dotnet publish](/dotnet/core/tools/dotnet-publish) vers un serveur. Par exemple, des fichiers supplémentaires peuvent être requis ou exclus du dossier *publish*. Visual Studio utilise MSBuild pour le déploiement web, et MSBuild peut être personnalisé pour effectuer de nombreuses autres tâches pendant le déploiement. Pour plus d’informations, consultez [Profils de publication dans Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) et l’ouvrage [Using MSBuild and Team Foundation Build](http://msbuildbook.com/) (Utilisation de MSBuild et Team Foundation Build).

Les applications peuvent être déployées directement à partir de Visual Studio sur Azure App Service à l’aide de la [fonctionnalité de publication web](xref:tutorials/publish-to-azure-webapp-using-vs) ou de la [prise en charge intégrée de Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment). Visual Studio Team Services prend en charge le [déploiement continu sur Azure App Service](/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?tabs=vsts).

## <a name="publishing-to-azure"></a>Publication sur Azure

Consultez la page [Publier une application web ASP.NET Core sur Azure App Service à l’aide de Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pour savoir comment publier une application sur Azure à l’aide de Visual Studio. Il est également possible de la publier sur Azure à partir de la [ligne de commande](xref:tutorials/publish-to-azure-webapp-using-cli).

## <a name="additional-resources"></a>Ressources supplémentaires

Pour plus d’informations sur l’utilisation de Docker comme environnement d’hébergement, consultez [Applications ASP.NET Core hôtes dans Docker](xref:host-and-deploy/docker/index).
