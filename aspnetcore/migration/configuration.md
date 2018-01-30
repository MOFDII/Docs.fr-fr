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
ms.openlocfilehash: 23b96ad11201f9b82cbd9fb832757d905407d228
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-configuration"></a><span data-ttu-id="c3bff-102">Migration de la Configuration</span><span class="sxs-lookup"><span data-stu-id="c3bff-102">Migrating Configuration</span></span>

<span data-ttu-id="c3bff-103">Par [Steve Smith](https://ardalis.com/) et [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="c3bff-103">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="c3bff-104">Dans l’article précédent, nous avons commencé [vers un projet ASP.NET MVC ASP.NET MVC de base](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="c3bff-104">In the previous article, we began [migrating an ASP.NET MVC project to ASP.NET Core MVC](mvc.md).</span></span> <span data-ttu-id="c3bff-105">Dans cet article, nous migrer la configuration.</span><span class="sxs-lookup"><span data-stu-id="c3bff-105">In this article, we migrate configuration.</span></span>

<span data-ttu-id="c3bff-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c3bff-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="c3bff-107">Configuration de l’installation</span><span class="sxs-lookup"><span data-stu-id="c3bff-107">Setup Configuration</span></span>

<span data-ttu-id="c3bff-108">ASP.NET Core n’utilise plus le *Global.asax* et *web.config* fichiers utilisées par les versions précédentes d’ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c3bff-108">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="c3bff-109">Dans les versions antérieures d’ASP.NET, la logique de démarrage d’application a été placée dans un `Application_StartUp` méthode *Global.asax*.</span><span class="sxs-lookup"><span data-stu-id="c3bff-109">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="c3bff-110">Ensuite, dans ASP.NET MVC, un *Startup.cs* fichier a été inclus dans la racine du projet ; et, elle a été appelée au démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="c3bff-110">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="c3bff-111">ASP.NET Core a arrêté complètement cette approche, en plaçant toute logique de démarrage dans le *Startup.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="c3bff-111">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="c3bff-112">Le *web.config* fichier a également été remplacé dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3bff-112">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="c3bff-113">Configuration proprement dite peut maintenant être configurée, dans le cadre de la procédure de démarrage d’application décrite dans *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c3bff-113">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="c3bff-114">Configuration peut toujours utiliser des fichiers XML, mais en général, les projets ASP.NET Core place les valeurs de configuration dans un fichier au format JSON, tel que *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c3bff-114">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="c3bff-115">Système de configuration d’ASP.NET Core peut accéder aussi facilement les variables d’environnement, ce qui peuvent fournir un emplacement plus sécurisé et fiable pour les valeurs spécifiques à l’environnement.</span><span class="sxs-lookup"><span data-stu-id="c3bff-115">ASP.NET Core's configuration system can also easily access environment variables, which can provide a more secure and robust location for environment-specific values.</span></span> <span data-ttu-id="c3bff-116">Cela est particulièrement vrai pour les clés secrètes comme des chaînes de connexion et les clés API qui ne doivent pas être archivés dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="c3bff-116">This is especially true for secrets like connection strings and API keys that shouldn't be checked into source control.</span></span> <span data-ttu-id="c3bff-117">Consultez [Configuration](xref:fundamentals/configuration/index) pour en savoir plus sur la configuration dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c3bff-117">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="c3bff-118">Pour cet article, nous avons commencé avec le projet ASP.NET Core partiellement migré à partir de [l’article précédent](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="c3bff-118">For this article, we are starting with the partially-migrated ASP.NET Core project from [the previous article](mvc.md).</span></span> <span data-ttu-id="c3bff-119">Pour installer la configuration, ajoutez le constructeur et la propriété suivante le *Startup.cs* fichier situé à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="c3bff-119">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]

<span data-ttu-id="c3bff-120">Notez qu’à ce stade, le *Startup.cs* fichier ne sera pas compilé, qu’il faut toujours ajouter les éléments suivants `using` instruction :</span><span class="sxs-lookup"><span data-stu-id="c3bff-120">Note that at this point, the *Startup.cs* file won't compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="c3bff-121">Ajouter un *appsettings.json* fichier à la racine du projet à l’aide du modèle d’élément approprié :</span><span class="sxs-lookup"><span data-stu-id="c3bff-121">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![Ajouter AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="c3bff-123">Migrer les paramètres de Configuration à partir de web.config</span><span class="sxs-lookup"><span data-stu-id="c3bff-123">Migrate Configuration Settings from web.config</span></span>

<span data-ttu-id="c3bff-124">Notre projet ASP.NET MVC incluse de la chaîne de connexion de base de données requises dans *web.config*, dans le `<connectionStrings>` élément.</span><span class="sxs-lookup"><span data-stu-id="c3bff-124">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="c3bff-125">Dans notre projet ASP.NET Core, nous allons stocker ces informations dans le *appsettings.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="c3bff-125">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="c3bff-126">Ouvrez *appsettings.json*et notez qu’il contient déjà les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="c3bff-126">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]


<span data-ttu-id="c3bff-127">Dans la ligne en surbrillance mentionnée ci-dessus, remplacez le nom de la base de données à partir de **_CHANGE_ME** au nom de votre base de données.</span><span class="sxs-lookup"><span data-stu-id="c3bff-127">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="c3bff-128">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="c3bff-128">Summary</span></span>

<span data-ttu-id="c3bff-129">ASP.NET Core place toute logique de démarrage de l’application dans un seul fichier dans lequel les services nécessaires et les dépendances peuvent être définis et configurés.</span><span class="sxs-lookup"><span data-stu-id="c3bff-129">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="c3bff-130">Il remplace le *web.config* fichier avec une fonctionnalité de configuration souples qui peut tirer parti des divers formats de fichier, tels que JSON, ainsi que les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="c3bff-130">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
