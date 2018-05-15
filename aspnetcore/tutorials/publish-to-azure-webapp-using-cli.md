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
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="fa4a1-103">Publier une application ASP.NET Core sur Azure avec des outils en ligne de commande</span><span class="sxs-lookup"><span data-stu-id="fa4a1-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="fa4a1-104">Auteur : [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="fa4a1-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="fa4a1-105">Ce didacticiel vous montrera comment générer et déployer une application ASP.NET Core sur Microsoft Azure App Service à l’aide d’outils en ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-105">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="fa4a1-106">Lorsque vous aurez terminé, vous disposerez d’une application web créée dans ASP.NET MVC Core et hébergée comme une application Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-106">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="fa4a1-107">Ce didacticiel est écrit avec des outils en ligne de commande Windows, mais il peut également s’appliquer à des environnements macOS et Linux.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="fa4a1-108">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="fa4a1-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fa4a1-109">créer un site web Azure App Service à l’aide de l’interface Azure CLI ;</span><span class="sxs-lookup"><span data-stu-id="fa4a1-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="fa4a1-110">déployer une application ASP.NET Core sur Azure App Service à l’aide de l’outil en ligne de commande Git.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-110">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fa4a1-111">Prérequis</span><span class="sxs-lookup"><span data-stu-id="fa4a1-111">Prerequisites</span></span>

<span data-ttu-id="fa4a1-112">Pour suivre ce didacticiel, vous aurez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="fa4a1-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="fa4a1-113">Un [abonnement Microsoft Azure](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="fa4a1-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="fa4a1-114">Un client de ligne de commande [Git](https://www.git-scm.com/)</span><span class="sxs-lookup"><span data-stu-id="fa4a1-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="fa4a1-115">Créer une application web</span><span class="sxs-lookup"><span data-stu-id="fa4a1-115">Create a web application</span></span>

<span data-ttu-id="fa4a1-116">Créez un nouveau répertoire pour l’application web, créez une nouvelle application ASP.NET Core MVC, puis exécutez le site web en local.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-116">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="fa4a1-117">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="fa4a1-117">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="fa4a1-118">Autre</span><span class="sxs-lookup"><span data-stu-id="fa4a1-118">Other</span></span>](#tab/other)
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

<span data-ttu-id="fa4a1-120">Testez l’application en accédant à http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-120">Test the application by browsing to http://localhost:5000.</span></span>

![Le site web en cours d’exécution en local](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="fa4a1-122">Créer l’instance Azure App Service</span><span class="sxs-lookup"><span data-stu-id="fa4a1-122">Create the Azure App Service instance</span></span>

<span data-ttu-id="fa4a1-123">À l’aide [d’Azure Cloud Shell](/azure/cloud-shell/quickstart), créez un groupe de ressources, un plan App Service et une application web App Service.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-123">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="fa4a1-124">Avant le déploiement, définissez les identifiants de déploiement au niveau du compte à l’aide de la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="fa4a1-124">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="fa4a1-125">Une URL de déploiement est nécessaire pour déployer l’application à l’aide de Git.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-125">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="fa4a1-126">Récupérez l’URL ainsi.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-126">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="fa4a1-127">Notez l’URL qui s’affiche ; elle se termine par `.git`.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-127">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="fa4a1-128">Elle est utilisée dans l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-128">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="fa4a1-129">Déployer l’application avec Git</span><span class="sxs-lookup"><span data-stu-id="fa4a1-129">Deploy the application using Git</span></span>

<span data-ttu-id="fa4a1-130">Vous pouvez maintenant effectuer le déploiement avec Git à partir de votre ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-130">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="fa4a1-131">Il est possible d’ignorer sans risque les avertissements de Git sur les fins de ligne.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-131">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="fa4a1-132">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="fa4a1-132">Windows</span></span>](#tab/windows)
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

# <a name="othertabother"></a>[<span data-ttu-id="fa4a1-133">Autre</span><span class="sxs-lookup"><span data-stu-id="fa4a1-133">Other</span></span>](#tab/other)
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

<span data-ttu-id="fa4a1-134">Git vous demande les identifiants de déploiement définis précédemment.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-134">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="fa4a1-135">Après l’authentification, l’application sera envoyée sur l’emplacement distant, générée et déployée.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-135">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Sortie du déploiement Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="fa4a1-137">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="fa4a1-137">Test the application</span></span>

<span data-ttu-id="fa4a1-138">Testez l’application en accédant à `https://<web app name>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-138">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="fa4a1-139">Pour afficher l’adresse dans Cloud Shell (ou Azure CLI), utilisez les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="fa4a1-139">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![L’application en cours d’exécution dans Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="fa4a1-141">Nettoyer</span><span class="sxs-lookup"><span data-stu-id="fa4a1-141">Clean up</span></span>

<span data-ttu-id="fa4a1-142">Lorsque vous avez terminé de tester l’application et d’inspecter le code et les ressources, supprimez l’application web et le plan en supprimant le groupe de ressources.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-142">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="fa4a1-143">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="fa4a1-143">Next steps</span></span>

<span data-ttu-id="fa4a1-144">Dans ce didacticiel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="fa4a1-144">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fa4a1-145">créer un site web Azure App Service à l’aide de l’interface Azure CLI ;</span><span class="sxs-lookup"><span data-stu-id="fa4a1-145">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="fa4a1-146">déployer une application ASP.NET Core sur Azure App Service à l’aide de l’outil en ligne de commande Git.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-146">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="fa4a1-147">Apprenez maintenant à utiliser la ligne de commande pour déployer une application web existante qui utilise CosmosDB.</span><span class="sxs-lookup"><span data-stu-id="fa4a1-147">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fa4a1-148">Déployer sur Azure en ligne de commande avec .NET Core</span><span class="sxs-lookup"><span data-stu-id="fa4a1-148">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
