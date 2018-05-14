---
title: Publier une application ASP.NET Core sur Azure avec des outils en ligne de commande
author: camsoper
description: Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide du client de ligne de commande Git.
manager: wpickett
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
ms.devlang: dotnet
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 0462a4cf18bba23643ed3b1b4e6b76bdbceb24a8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a>Publier une application ASP.NET Core sur Azure avec des outils en ligne de commande

Auteur : [Cam Soper](https://twitter.com/camsoper)

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

Ce didacticiel vous montrera comment générer et déployer une application ASP.NET Core sur Microsoft Azure App Service à l’aide d’outils en ligne de commande.  Lorsque vous aurez terminé, vous disposerez d’une application web créée dans ASP.NET MVC Core et hébergée comme une application Azure App Service Web Apps.  Ce didacticiel est écrit avec des outils en ligne de commande Windows, mais il peut également s’appliquer à des environnements macOS et Linux.  

Dans ce didacticiel, vous apprendrez à :

> [!div class="checklist"]
> * créer un site web Azure App Service à l’aide de l’interface Azure CLI ;
> * déployer une application ASP.NET Core sur Azure App Service à l’aide de l’outil en ligne de commande Git.

## <a name="prerequisites"></a>Prérequis

Pour suivre ce didacticiel, vous aurez besoin des éléments suivants :

* Un [abonnement Microsoft Azure](https://azure.microsoft.com/free/)
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* Un client de ligne de commande [Git](https://www.git-scm.com/)

## <a name="create-a-web-application"></a>Créer une application web

Créez un nouveau répertoire pour l’application web, créez une nouvelle application ASP.NET Core MVC, puis exécutez le site web en local.

# <a name="windowstabwindows"></a>[Fenêtres](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[Autre](#tab/other)
```bash
# Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the application
dotnet run
```
---

![Sortie de la ligne de commande](publish-to-azure-webapp-using-cli/_static/new_prj.png)

Testez l’application en accédant à http://localhost:5000.

![Le site web en cours d’exécution en local](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a>Créer l’instance Azure App Service

À l’aide [d’Azure Cloud Shell](/azure/cloud-shell/quickstart), créez un groupe de ressources, un plan App Service et une application web App Service.

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

Avant le déploiement, définissez les identifiants de déploiement au niveau du compte à l’aide de la commande suivante :

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

Une URL de déploiement est nécessaire pour déployer l’application à l’aide de Git.  Récupérez l’URL ainsi.

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
Notez l’URL qui s’affiche ; elle se termine par `.git`. Elle est utilisée dans l’étape suivante.

## <a name="deploy-the-application-using-git"></a>Déployer l’application avec Git

Vous pouvez maintenant effectuer le déploiement avec Git à partir de votre ordinateur local.

> [!NOTE]
> Il est possible d’ignorer sans risque les avertissements de Git sur les fins de ligne.

# <a name="windowstabwindows"></a>[Fenêtres](#tab/windows)
```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[Autre](#tab/other)
```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```
---

Git vous demande les identifiants de déploiement définis précédemment. Après l’authentification, l’application sera envoyée sur l’emplacement distant, générée et déployée.

![Sortie du déploiement Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a>Tester l’application

Testez l’application en accédant à `https://<web app name>.azurewebsites.net`.  Pour afficher l’adresse dans Cloud Shell (ou Azure CLI), utilisez les éléments suivants :

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![L’application en cours d’exécution dans Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a>Nettoyer

Lorsque vous avez terminé de tester l’application et d’inspecter le code et les ressources, supprimez l’application web et le plan en supprimant le groupe de ressources.

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à :

> [!div class="checklist"]
> * créer un site web Azure App Service à l’aide de l’interface Azure CLI ;
> * déployer une application ASP.NET Core sur Azure App Service à l’aide de l’outil en ligne de commande Git.

Apprenez maintenant à utiliser la ligne de commande pour déployer une application web existante qui utilise CosmosDB.

> [!div class="nextstepaction"]
> [Déployer sur Azure en ligne de commande avec .NET Core](/dotnet/azure/dotnet-quickstart-xplat)
