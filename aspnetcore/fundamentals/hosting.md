---
title: "Hébergement dans ASP.NET Core"
author: guardrex
description: "En savoir plus sur l’hôte web dans ASP.NET Core, qui est responsable de la gestion de démarrage et la durée de vie des applications."
keywords: "ASP.NET Core, web hôte, IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime"
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: 8adc58d67f103e8d1fc8fe197cf392752bdaf660
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="40876-104">Hébergement dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40876-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="40876-105">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="40876-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="40876-106">Applications ASP.NET Core configurer et lancer une *hôte*.</span><span class="sxs-lookup"><span data-stu-id="40876-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="40876-107">L’hôte est responsable de la gestion de démarrage et la durée de vie des applications.</span><span class="sxs-lookup"><span data-stu-id="40876-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="40876-108">Au minimum, l’hôte configure un serveur et un pipeline de traitement de la requête.</span><span class="sxs-lookup"><span data-stu-id="40876-108">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="40876-109">Configuration d’un ordinateur hôte</span><span class="sxs-lookup"><span data-stu-id="40876-109">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40876-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40876-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="40876-111">Créer un hôte à l’aide d’une instance de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="40876-111">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="40876-112">Cette opération est généralement effectuée dans le point d’entrée de l’application, le `Main` (méthode).</span><span class="sxs-lookup"><span data-stu-id="40876-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="40876-113">Dans les modèles de projet, `Main` se trouve dans *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="40876-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="40876-114">Par défaut *Program.cs* appelle [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) pour lancer l’installation d’un ordinateur hôte :</span><span class="sxs-lookup"><span data-stu-id="40876-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="40876-115">`CreateDefaultBuilder`effectue les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="40876-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="40876-116">Configure [Kestrel](servers/kestrel.md) que le serveur web.</span><span class="sxs-lookup"><span data-stu-id="40876-116">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="40876-117">Pour les options par défaut Kestrel, consultez [le Kestrel options section d’implémentation du serveur web Kestrel ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="40876-117">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="40876-118">Définit le chemin d’accès retourné par la racine de contenu [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="40876-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="40876-119">Configuration facultative de charge à partir de :</span><span class="sxs-lookup"><span data-stu-id="40876-119">Loads optional configuration from:</span></span>
  * <span data-ttu-id="40876-120">*appSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="40876-120">*appsettings.json*.</span></span>
  * <span data-ttu-id="40876-121">*appSettings. {Environnement} .json*.</span><span class="sxs-lookup"><span data-stu-id="40876-121">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="40876-122">[Les secrets utilisateur](xref:security/app-secrets) lorsque l’application s’exécute le `Development` environnement.</span><span class="sxs-lookup"><span data-stu-id="40876-122">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="40876-123">Variables d'environnement.</span><span class="sxs-lookup"><span data-stu-id="40876-123">Environment variables.</span></span>
  * <span data-ttu-id="40876-124">Arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="40876-124">Command-line arguments.</span></span>
* <span data-ttu-id="40876-125">Configure [journalisation](xref:fundamentals/logging/index) pour la sortie de console et de débogage.</span><span class="sxs-lookup"><span data-stu-id="40876-125">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="40876-126">La journalisation inclut [filtrage de journal](xref:fundamentals/logging/index#log-filtering) les règles spécifiées dans une section de configuration de journalisation d’un *appsettings.json* ou *appsettings. {} Environnement} .json* fichier.</span><span class="sxs-lookup"><span data-stu-id="40876-126">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="40876-127">En cas de retard IIS, Active [intégration des services Internet](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="40876-127">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="40876-128">Configure le chemin d’accès de base et le port écouté par le serveur lors de l’utilisation du [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="40876-128">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="40876-129">Le module crée un proxy inverse entre IIS et Kestrel.</span><span class="sxs-lookup"><span data-stu-id="40876-129">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="40876-130">Configure également l’application de [capturer les erreurs de démarrage](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="40876-130">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="40876-131">Pour les options par défaut d’IIS, consultez [IIS options de section de l’hôte ASP.NET Core sur Windows avec IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="40876-131">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="40876-132">Le *racine du contenu* détermine où l’hôte recherche les fichiers de contenu, tels que les fichiers de vue MVC.</span><span class="sxs-lookup"><span data-stu-id="40876-132">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="40876-133">Lorsque l’application est lancée à partir du dossier racine du projet, le dossier du projet racine est utilisé en tant que la racine du contenu.</span><span class="sxs-lookup"><span data-stu-id="40876-133">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="40876-134">Ceci est la valeur par défaut utilisée dans [Visual Studio](https://www.visualstudio.com/) et [dotnet nouveaux modèles](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="40876-134">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="40876-135">Pour plus d’informations sur la configuration de l’application, consultez [Configuration dans ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="40876-135">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="40876-136">Comme alternative à l’aide de la méthode statique `CreateDefaultBuilder` méthode, la création d’un hôte à partir de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) est une approche pris en charge avec ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="40876-136">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="40876-137">Pour plus d’informations, consultez l’onglet 1.x ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="40876-137">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40876-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40876-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="40876-139">Créer un hôte à l’aide d’une instance de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="40876-139">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="40876-140">Création d’un hôte est généralement effectuée dans le point d’entrée de l’application, le `Main` (méthode).</span><span class="sxs-lookup"><span data-stu-id="40876-140">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="40876-141">Dans les modèles de projet, `Main` se trouve dans *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="40876-141">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="40876-142">`WebHostBuilder`nécessite un [serveur qui implémente IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="40876-142">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="40876-143">Serveurs intégrés sont [Kestrel](servers/kestrel.md) et [HTTP.sys](servers/httpsys.md) (avant la version d’ASP.NET 2.0 de noyaux, HTTP.sys a été appelé [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="40876-143">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="40876-144">Dans cet exemple, le [méthode d’extension UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) Spécifie le serveur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="40876-144">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="40876-145">Le *racine du contenu* détermine où l’hôte recherche les fichiers de contenu, tels que les fichiers de vue MVC.</span><span class="sxs-lookup"><span data-stu-id="40876-145">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="40876-146">La racine de contenu par défaut est obtenue pour `UseContentRoot` par [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="40876-146">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="40876-147">Lorsque l’application est lancée à partir du dossier racine du projet, le dossier du projet racine est utilisé en tant que la racine du contenu.</span><span class="sxs-lookup"><span data-stu-id="40876-147">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="40876-148">Ceci est la valeur par défaut utilisée dans [Visual Studio](https://www.visualstudio.com/) et [dotnet nouveaux modèles](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="40876-148">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="40876-149">Pour utiliser IIS comme un proxy inverse, appelez [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) dans le cadre de la création de l’ordinateur hôte.</span><span class="sxs-lookup"><span data-stu-id="40876-149">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="40876-150">`UseIISIntegration`ne configure pas un *server*, comme [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) est.</span><span class="sxs-lookup"><span data-stu-id="40876-150">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="40876-151">`UseIISIntegration`Configure le chemin d’accès de base et le port écouté par le serveur lors de l’utilisation du [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) pour créer un proxy inverse entre Kestrel et IIS.</span><span class="sxs-lookup"><span data-stu-id="40876-151">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="40876-152">Utilisation d’IIS avec ASP.NET Core, `UseKestrel` et `UseIISIntegration` doit être spécifié.</span><span class="sxs-lookup"><span data-stu-id="40876-152">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="40876-153">`UseIISIntegration`Active uniquement lors de l’exécution derrière IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="40876-153">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="40876-154">Pour plus d’informations, consultez [Introduction à ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) et [référence de configuration ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="40876-154">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="40876-155">Une implémentation minimale qui configure un ordinateur hôte (et une application ASP.NET Core) consiste à spécifier un serveur et la configuration du pipeline de demande de l’application :</span><span class="sxs-lookup"><span data-stu-id="40876-155">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

<span data-ttu-id="40876-156">Lorsque vous configurez un ordinateur hôte, [configurer](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) et [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) méthodes peuvent être fournies.</span><span class="sxs-lookup"><span data-stu-id="40876-156">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="40876-157">Si un `Startup` classe est spécifié, il doit définir un `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="40876-157">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="40876-158">Pour plus d’informations, consultez [démarrage de l’Application dans ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="40876-158">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="40876-159">Appels multiples à `ConfigureServices` ajouter à un autre.</span><span class="sxs-lookup"><span data-stu-id="40876-159">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="40876-160">Appels multiples à `Configure` ou `UseStartup` sur la `WebHostBuilder` remplacer les paramètres précédents.</span><span class="sxs-lookup"><span data-stu-id="40876-160">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="40876-161">Valeurs de configuration d’hôte</span><span class="sxs-lookup"><span data-stu-id="40876-161">Host configuration values</span></span>

<span data-ttu-id="40876-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) s’appuie sur des approches suivantes pour définir des valeurs de configuration de l’ordinateur hôte :</span><span class="sxs-lookup"><span data-stu-id="40876-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="40876-163">Configuration de générateur de l’hôte, qui inclut des variables d’environnement avec le format `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="40876-163">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="40876-164">Par exemple, `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="40876-164">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="40876-165">Méthodes explicites, tel que `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="40876-165">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="40876-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) et la clé associée.</span><span class="sxs-lookup"><span data-stu-id="40876-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="40876-167">Lors de la définition d’une valeur avec `UseSetting`, la valeur est définie en tant que chaîne, quelle que soit le type.</span><span class="sxs-lookup"><span data-stu-id="40876-167">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="40876-168">L’hôte utilise l’option qui définit une valeur en dernier.</span><span class="sxs-lookup"><span data-stu-id="40876-168">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="40876-169">Pour plus d’informations, consultez [configuration de remplacement](#overriding-configuration) dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="40876-169">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="40876-170">Capturer les erreurs de démarrage</span><span class="sxs-lookup"><span data-stu-id="40876-170">Capture Startup Errors</span></span>

<span data-ttu-id="40876-171">Ce paramètre contrôle la capture des erreurs de démarrage.</span><span class="sxs-lookup"><span data-stu-id="40876-171">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="40876-172">**Clé**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="40876-172">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="40876-173">**Type**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="40876-173">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="40876-174">**Par défaut**: valeur par défaut est `false` , sauf si l’application s’exécute avec Kestrel derrière IIS, où la valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="40876-174">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="40876-175">**À l’aide de**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="40876-175">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="40876-176">**Variable d’environnement**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="40876-176">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="40876-177">Lorsque `false`, erreurs de résultat de démarrage de l’ordinateur hôte en cours de fermeture.</span><span class="sxs-lookup"><span data-stu-id="40876-177">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="40876-178">Lorsque `true`, capture des exceptions lors du démarrage de l’ordinateur hôte et tente de démarrer le serveur.</span><span class="sxs-lookup"><span data-stu-id="40876-178">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40876-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40876-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40876-180">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40876-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="40876-181">Racine du contenu</span><span class="sxs-lookup"><span data-stu-id="40876-181">Content Root</span></span>

<span data-ttu-id="40876-182">Ce paramètre détermine où ASP.NET Core commence à rechercher les fichiers de contenu, tels que les vues MVC.</span><span class="sxs-lookup"><span data-stu-id="40876-182">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="40876-183">**Clé**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="40876-183">**Key**: contentRoot</span></span>  
<span data-ttu-id="40876-184">**Type**: *chaîne*</span><span class="sxs-lookup"><span data-stu-id="40876-184">**Type**: *string*</span></span>  
<span data-ttu-id="40876-185">**Par défaut**: le dossier où réside l’application par défaut.</span><span class="sxs-lookup"><span data-stu-id="40876-185">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="40876-186">**À l’aide de**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="40876-186">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="40876-187">**Variable d’environnement**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="40876-187">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="40876-188">La racine du contenu est également utilisée comme le chemin d’accès de base pour le [paramètre de la racine Web](#web-root).</span><span class="sxs-lookup"><span data-stu-id="40876-188">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="40876-189">Si le chemin d’accès n’existe pas, l’hôte ne peut pas démarrer.</span><span class="sxs-lookup"><span data-stu-id="40876-189">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40876-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40876-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40876-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40876-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="40876-192">Erreurs détaillées</span><span class="sxs-lookup"><span data-stu-id="40876-192">Detailed Errors</span></span>

<span data-ttu-id="40876-193">Détermine si les détails les erreurs doivent être capturés.</span><span class="sxs-lookup"><span data-stu-id="40876-193">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="40876-194">**Clé**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="40876-194">**Key**: detailedErrors</span></span>  
<span data-ttu-id="40876-195">**Type**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="40876-195">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="40876-196">**Par défaut**: false</span><span class="sxs-lookup"><span data-stu-id="40876-196">**Default**: false</span></span>  
<span data-ttu-id="40876-197">**À l’aide de**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="40876-197">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="40876-198">**Variable d’environnement**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="40876-198">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="40876-199">Lorsque activé (ou lorsque la <a href="#environment">environnement</a> a la valeur `Development`), l’application capture les exceptions détaillées.</span><span class="sxs-lookup"><span data-stu-id="40876-199">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40876-200">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40876-200">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40876-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40876-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="40876-202">Environnement</span><span class="sxs-lookup"><span data-stu-id="40876-202">Environment</span></span>

<span data-ttu-id="40876-203">Définit l’environnement de l’application.</span><span class="sxs-lookup"><span data-stu-id="40876-203">Sets the app's environment.</span></span>

<span data-ttu-id="40876-204">**Clé**: environnement</span><span class="sxs-lookup"><span data-stu-id="40876-204">**Key**: environment</span></span>  
<span data-ttu-id="40876-205">**Type**: *chaîne*</span><span class="sxs-lookup"><span data-stu-id="40876-205">**Type**: *string*</span></span>  
<span data-ttu-id="40876-206">**Par défaut**: Production</span><span class="sxs-lookup"><span data-stu-id="40876-206">**Default**: Production</span></span>  
<span data-ttu-id="40876-207">**À l’aide de**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="40876-207">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="40876-208">**Variable d’environnement**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="40876-208">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="40876-209">L’environnement peut être défini à n’importe quelle valeur.</span><span class="sxs-lookup"><span data-stu-id="40876-209">The environment can be set to any value.</span></span> <span data-ttu-id="40876-210">Les valeurs définies par l’infrastructure sont `Development`, `Staging`, et `Production`.</span><span class="sxs-lookup"><span data-stu-id="40876-210">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="40876-211">Les valeurs ne sont pas respecter la casse.</span><span class="sxs-lookup"><span data-stu-id="40876-211">Values aren't case sensitive.</span></span> <span data-ttu-id="40876-212">Par défaut, le *environnement* est lu depuis le `ASPNETCORE_ENVIRONMENT` variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="40876-212">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="40876-213">Lorsque vous utilisez [Visual Studio](https://www.visualstudio.com/), variables d’environnement peuvent être définies dans le *launchSettings.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="40876-213">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="40876-214">Pour plus d’informations, consultez [Utilisation de plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="40876-214">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40876-215">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40876-215">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40876-216">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40876-216">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="40876-217">Hébergement des assemblys de démarrage</span><span class="sxs-lookup"><span data-stu-id="40876-217">Hosting Startup Assemblies</span></span>

<span data-ttu-id="40876-218">Définit les assemblys de démarrage de l’application hôte.</span><span class="sxs-lookup"><span data-stu-id="40876-218">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="40876-219">**Clé**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="40876-219">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="40876-220">**Type**: *chaîne*</span><span class="sxs-lookup"><span data-stu-id="40876-220">**Type**: *string*</span></span>  
<span data-ttu-id="40876-221">**Par défaut**: une chaîne vide</span><span class="sxs-lookup"><span data-stu-id="40876-221">**Default**: Empty string</span></span>  
<span data-ttu-id="40876-222">**À l’aide de**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="40876-222">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="40876-223">**Variable d’environnement**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="40876-223">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="40876-224">Une chaîne délimitée par des points-virgules d’héberger des assemblys de démarrage à charger au démarrage.</span><span class="sxs-lookup"><span data-stu-id="40876-224">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="40876-225">Cette fonctionnalité est une nouveauté dans ASP.NET 2.0 de base.</span><span class="sxs-lookup"><span data-stu-id="40876-225">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="40876-226">Bien que la valeur de configuration par défaut est une chaîne vide, les assemblys de démarrage hébergement toujours incluent assembly de l’application.</span><span class="sxs-lookup"><span data-stu-id="40876-226">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="40876-227">Lors de l’hébergement des assemblys de démarrage sont fournies, elles sont ajoutées à l’assembly de l’application pour le chargement lorsque l’application génère ses services courants lors du démarrage.</span><span class="sxs-lookup"><span data-stu-id="40876-227">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40876-228">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40876-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40876-229">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40876-229">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="40876-230">Cette fonctionnalité n’est pas disponible dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="40876-230">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="40876-231">Préférez l’hébergement d’URL</span><span class="sxs-lookup"><span data-stu-id="40876-231">Prefer Hosting URLs</span></span>

<span data-ttu-id="40876-232">Indique si l’ordinateur hôte doit écouter les URL configurées avec le `WebHostBuilder` au lieu de ceux configurés avec la `IServer` implémentation.</span><span class="sxs-lookup"><span data-stu-id="40876-232">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="40876-233">**Clé**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="40876-233">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="40876-234">**Type**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="40876-234">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="40876-235">**Par défaut**: true</span><span class="sxs-lookup"><span data-stu-id="40876-235">**Default**: true</span></span>  
<span data-ttu-id="40876-236">**À l’aide de**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="40876-236">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="40876-237">**Variable d’environnement**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="40876-237">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="40876-238">Cette fonctionnalité est une nouveauté dans ASP.NET 2.0 de base.</span><span class="sxs-lookup"><span data-stu-id="40876-238">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40876-239">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40876-239">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40876-240">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40876-240">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="40876-241">Cette fonctionnalité n’est pas disponible dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="40876-241">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="40876-242">Empêcher le démarrage d’hébergement</span><span class="sxs-lookup"><span data-stu-id="40876-242">Prevent Hosting Startup</span></span>

<span data-ttu-id="40876-243">Empêche le chargement automatique des assemblys de démarrage, y compris les assemblys de démarrage configurés par l’assembly de l’application d’hébergement d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="40876-243">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="40876-244">Consultez [ajouter des fonctionnalités de l’application à partir d’un assembly externe à l’aide de IHostingStartup](xref:host-and-deploy/ihostingstartup) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="40876-244">See [Add app features from an external assembly using IHostingStartup](xref:host-and-deploy/ihostingstartup) for more information.</span></span>

<span data-ttu-id="40876-245">**Clé**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="40876-245">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="40876-246">**Type**: *bool* (`true` ou `1`)</span><span class="sxs-lookup"><span data-stu-id="40876-246">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="40876-247">**Par défaut**: false</span><span class="sxs-lookup"><span data-stu-id="40876-247">**Default**: false</span></span>  
<span data-ttu-id="40876-248">**À l’aide de**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="40876-248">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="40876-249">**Variable d’environnement**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="40876-249">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="40876-250">Cette fonctionnalité est une nouveauté dans ASP.NET 2.0 de base.</span><span class="sxs-lookup"><span data-stu-id="40876-250">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40876-251">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40876-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40876-252">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40876-252">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="40876-253">Cette fonctionnalité n’est pas disponible dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="40876-253">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="40876-254">URL du serveur</span><span class="sxs-lookup"><span data-stu-id="40876-254">Server URLs</span></span>

<span data-ttu-id="40876-255">Indique les adresses IP ou les adresses d’hôte avec les ports et protocoles que le serveur doit écouter les demandes.</span><span class="sxs-lookup"><span data-stu-id="40876-255">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="40876-256">**Clé**: URL</span><span class="sxs-lookup"><span data-stu-id="40876-256">**Key**: urls</span></span>  
<span data-ttu-id="40876-257">**Type**: *chaîne*</span><span class="sxs-lookup"><span data-stu-id="40876-257">**Type**: *string*</span></span>  
<span data-ttu-id="40876-258">**Par défaut**: http://localhost : 5000</span><span class="sxs-lookup"><span data-stu-id="40876-258">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="40876-259">**À l’aide de**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="40876-259">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="40876-260">**Variable d’environnement**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="40876-260">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="40876-261">La valeur séparées par des points-virgules ( ;) des préfixes de la liste des URL à laquelle le serveur doit répondre.</span><span class="sxs-lookup"><span data-stu-id="40876-261">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="40876-262">Par exemple, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="40876-262">For example, `http://localhost:123`.</span></span> <span data-ttu-id="40876-263">Utilisez «\*» pour indiquer que le serveur doit écouter les demandes sur des adresses IP ou le nom d’hôte en utilisant le port spécifié et le protocole (par exemple, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="40876-263">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="40876-264">Le protocole (`http://` ou `https://`) doivent être incluses avec chaque URL.</span><span class="sxs-lookup"><span data-stu-id="40876-264">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="40876-265">Formats pris en charge varient entre les serveurs.</span><span class="sxs-lookup"><span data-stu-id="40876-265">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40876-266">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40876-266">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="40876-267">Kestrel a sa propre API de configuration du point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="40876-267">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="40876-268">Pour plus d’informations, consultez [Implémentation du serveur web Kestrel dans ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="40876-268">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40876-269">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40876-269">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="40876-270">Délai d’arrêt</span><span class="sxs-lookup"><span data-stu-id="40876-270">Shutdown Timeout</span></span>

<span data-ttu-id="40876-271">Spécifie la quantité de temps à attendre avant l’arrêt hôte web.</span><span class="sxs-lookup"><span data-stu-id="40876-271">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="40876-272">**Clé**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="40876-272">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="40876-273">**Type**: *int*</span><span class="sxs-lookup"><span data-stu-id="40876-273">**Type**: *int*</span></span>  
<span data-ttu-id="40876-274">**Par défaut**: 5</span><span class="sxs-lookup"><span data-stu-id="40876-274">**Default**: 5</span></span>  
<span data-ttu-id="40876-275">**À l’aide de**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="40876-275">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="40876-276">**Variable d’environnement**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="40876-276">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="40876-277">Bien que la clé accepte un *int* avec `UseSetting` (par exemple, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), la `UseShutdownTimeout` méthode d’extension prend un `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="40876-277">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="40876-278">Cette fonctionnalité est une nouveauté dans ASP.NET 2.0 de base.</span><span class="sxs-lookup"><span data-stu-id="40876-278">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40876-279">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40876-279">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40876-280">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40876-280">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="40876-281">Cette fonctionnalité n’est pas disponible dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="40876-281">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="40876-282">Assembly de démarrage</span><span class="sxs-lookup"><span data-stu-id="40876-282">Startup Assembly</span></span>

<span data-ttu-id="40876-283">Détermine l’assembly à rechercher la `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="40876-283">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="40876-284">**Clé**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="40876-284">**Key**: startupAssembly</span></span>  
<span data-ttu-id="40876-285">**Type**: *chaîne*</span><span class="sxs-lookup"><span data-stu-id="40876-285">**Type**: *string*</span></span>  
<span data-ttu-id="40876-286">**Par défaut**: assembly de l’application</span><span class="sxs-lookup"><span data-stu-id="40876-286">**Default**: The app's assembly</span></span>  
<span data-ttu-id="40876-287">**À l’aide de**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="40876-287">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="40876-288">**Variable d’environnement**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="40876-288">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="40876-289">L’assembly par nom (`string`) ou de type (`TStartup`) peuvent être référencés.</span><span class="sxs-lookup"><span data-stu-id="40876-289">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="40876-290">Si plusieurs `UseStartup` méthodes sont appelées, le dernier est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="40876-290">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40876-291">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40876-291">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40876-292">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40876-292">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a><span data-ttu-id="40876-293">Racine du site Web</span><span class="sxs-lookup"><span data-stu-id="40876-293">Web Root</span></span>

<span data-ttu-id="40876-294">Définit le chemin d’accès relatif aux ressources statique de l’application.</span><span class="sxs-lookup"><span data-stu-id="40876-294">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="40876-295">**Clé**: webroot</span><span class="sxs-lookup"><span data-stu-id="40876-295">**Key**: webroot</span></span>  
<span data-ttu-id="40876-296">**Type**: *chaîne*</span><span class="sxs-lookup"><span data-stu-id="40876-296">**Type**: *string*</span></span>  
<span data-ttu-id="40876-297">**Par défaut**: si non spécifié, la valeur par défaut est "(Content Root)/wwwroot », si le chemin d’accès existe.</span><span class="sxs-lookup"><span data-stu-id="40876-297">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="40876-298">Si le chemin d’accès n’existe pas, un fournisseur de fichiers de l’absence d’opération est utilisé.</span><span class="sxs-lookup"><span data-stu-id="40876-298">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="40876-299">**À l’aide de**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="40876-299">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="40876-300">**Variable d’environnement**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="40876-300">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40876-301">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40876-301">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40876-302">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40876-302">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="40876-303">Substitution de configuration</span><span class="sxs-lookup"><span data-stu-id="40876-303">Overriding configuration</span></span>

<span data-ttu-id="40876-304">Utilisez [Configuration](xref:fundamentals/configuration/index) pour configurer l’hôte.</span><span class="sxs-lookup"><span data-stu-id="40876-304">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="40876-305">Dans l’exemple suivant, configuration de l’hôte est éventuellement spécifiée dans un *hosting.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="40876-305">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="40876-306">Toute configuration chargée à partir de la *hosting.json* fichier peut être remplacé par des arguments de ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="40876-306">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="40876-307">La configuration intégrée (dans `config`) est utilisé pour configurer l’ordinateur hôte avec `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="40876-307">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40876-308">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40876-308">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="40876-309">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="40876-309">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="40876-310">Substitution de la configuration fournie par `UseUrls` avec *hosting.json* config du premier argument de ligne de commande config deuxième :</span><span class="sxs-lookup"><span data-stu-id="40876-310">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40876-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40876-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="40876-312">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="40876-312">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="40876-313">Substitution de la configuration fournie par `UseUrls` avec *hosting.json* config du premier argument de ligne de commande config deuxième :</span><span class="sxs-lookup"><span data-stu-id="40876-313">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> <span data-ttu-id="40876-314">Le [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) méthode d’extension n’est pas actuellement capable d’analyser une section de configuration retournée par `GetSection` (par exemple, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="40876-314">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="40876-315">Le `GetSection` méthode filtre les clés de configuration à la section demandée, mais laisse le nom de la section sur les clés (par exemple, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="40876-315">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="40876-316">Le `UseConfiguration` méthode attend les clés pour faire correspondre le `WebHostBuilder` clés (par exemple, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="40876-316">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="40876-317">La présence d’un nom de la section sur les clés empêche des valeurs de la section à partir de la configuration de l’hôte.</span><span class="sxs-lookup"><span data-stu-id="40876-317">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="40876-318">Ce problème sera résolu dans une prochaine mise en production.</span><span class="sxs-lookup"><span data-stu-id="40876-318">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="40876-319">Pour plus d’informations et des solutions de contournement, consultez [en passant de la section de configuration dans WebHostBuilder.UseConfiguration utilise des clés complètes](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="40876-319">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="40876-320">Pour spécifier l’hôte s’exécutent sur une URL particulière, la valeur souhaitée peut être passée dans à partir d’une invite de commandes lors de l’exécution `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="40876-320">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="40876-321">L’argument de ligne de commande remplace le `urls` valeur à partir de la *hosting.json* fichier et que le serveur écoute sur le port 8080 :</span><span class="sxs-lookup"><span data-stu-id="40876-321">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="40876-322">Démarrage de l’ordinateur hôte</span><span class="sxs-lookup"><span data-stu-id="40876-322">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="40876-323">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="40876-323">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="40876-324">**Exécuter**</span><span class="sxs-lookup"><span data-stu-id="40876-324">**Run**</span></span>

<span data-ttu-id="40876-325">Le `Run` méthode démarre l’application web et bloque le thread appelant jusqu'à ce que l’ordinateur hôte est arrêtée :</span><span class="sxs-lookup"><span data-stu-id="40876-325">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="40876-326">**Start**</span><span class="sxs-lookup"><span data-stu-id="40876-326">**Start**</span></span>

<span data-ttu-id="40876-327">Exécuter l’hôte de façon non bloquant en appelant son `Start` méthode :</span><span class="sxs-lookup"><span data-stu-id="40876-327">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="40876-328">Si une liste d’URL est passée à la `Start` (méthode), il est à l’écoute sur les URL spécifiées :</span><span class="sxs-lookup"><span data-stu-id="40876-328">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="40876-329">L’application peut initialiser et démarrer un nouvel hôte à l’aide des valeurs par défaut préconfigurés de `CreateDefaultBuilder` à l’aide d’une méthode pratique statique.</span><span class="sxs-lookup"><span data-stu-id="40876-329">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="40876-330">Ces méthodes de démarrer le serveur sans la sortie de console et avec [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) attendre un saut (Ctrl-C/SIGINT ou SIGTERM) :</span><span class="sxs-lookup"><span data-stu-id="40876-330">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="40876-331">**Démarrer (application RequestDelegate)**</span><span class="sxs-lookup"><span data-stu-id="40876-331">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="40876-332">Démarrer avec un `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="40876-332">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="40876-333">Effectuer une demande dans le navigateur pour `http://localhost:5000` pour recevoir la réponse « Hello World ! »</span><span class="sxs-lookup"><span data-stu-id="40876-333">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="40876-334">`WaitForShutdown`bloque jusqu'à ce qu’un saut (Ctrl-C/SIGINT ou SIGTERM) est émis.</span><span class="sxs-lookup"><span data-stu-id="40876-334">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="40876-335">L’application s’affiche la `Console.WriteLine` message et attend d’une touche quitter.</span><span class="sxs-lookup"><span data-stu-id="40876-335">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="40876-336">**Démarrer (string url RequestDelegate application)**</span><span class="sxs-lookup"><span data-stu-id="40876-336">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="40876-337">Démarrer avec une URL et `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="40876-337">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="40876-338">Produit le même résultat en tant que **début (application RequestDelegate)**, à l’exception de l’application répond à `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="40876-338">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="40876-339">**Démarrer (Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="40876-339">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="40876-340">Utiliser une instance de `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) à utiliser le routage intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="40876-340">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="40876-341">Utilisez les demandes suivantes du navigateur avec l’exemple :</span><span class="sxs-lookup"><span data-stu-id="40876-341">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="40876-342">Demande</span><span class="sxs-lookup"><span data-stu-id="40876-342">Request</span></span>                                    | <span data-ttu-id="40876-343">Réponse</span><span class="sxs-lookup"><span data-stu-id="40876-343">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="40876-344">Hello, Martin !</span><span class="sxs-lookup"><span data-stu-id="40876-344">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="40876-345">Buenos dias, Catrina !</span><span class="sxs-lookup"><span data-stu-id="40876-345">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="40876-346">Lève une exception avec la chaîne « ooops ! »</span><span class="sxs-lookup"><span data-stu-id="40876-346">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="40876-347">Lève une exception avec la chaîne « Uh oh ! »</span><span class="sxs-lookup"><span data-stu-id="40876-347">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="40876-348">Santé, Kevin !</span><span class="sxs-lookup"><span data-stu-id="40876-348">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="40876-349">Hello World!</span><span class="sxs-lookup"><span data-stu-id="40876-349">Hello World!</span></span>                             |

<span data-ttu-id="40876-350">`WaitForShutdown`bloque jusqu'à ce qu’un saut (Ctrl-C/SIGINT ou SIGTERM) est émis.</span><span class="sxs-lookup"><span data-stu-id="40876-350">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="40876-351">L’application s’affiche la `Console.WriteLine` message et attend d’une touche quitter.</span><span class="sxs-lookup"><span data-stu-id="40876-351">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="40876-352">**Démarrer (string url, Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="40876-352">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="40876-353">Utiliser une URL et une instance de `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="40876-353">Use a URL and an instance of `IRouteBuilder`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="40876-354">Produit le même résultat en tant que **Démarrer (Action<IRouteBuilder> routeBuilder)**, à l’exception de l’application répond à `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="40876-354">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="40876-355">**StartWith (Action<IApplicationBuilder> application)**</span><span class="sxs-lookup"><span data-stu-id="40876-355">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="40876-356">Fournissez un délégué pour configurer un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="40876-356">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="40876-357">Effectuer une demande dans le navigateur pour `http://localhost:5000` pour recevoir la réponse « Hello World ! »</span><span class="sxs-lookup"><span data-stu-id="40876-357">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="40876-358">`WaitForShutdown`bloque jusqu'à ce qu’un saut (Ctrl-C/SIGINT ou SIGTERM) est émis.</span><span class="sxs-lookup"><span data-stu-id="40876-358">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="40876-359">L’application s’affiche la `Console.WriteLine` message et attend d’une touche quitter.</span><span class="sxs-lookup"><span data-stu-id="40876-359">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="40876-360">**StartWith (string url, Action<IApplicationBuilder> application)**</span><span class="sxs-lookup"><span data-stu-id="40876-360">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="40876-361">Fournir une URL et un délégué pour configurer un `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="40876-361">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="40876-362">Produit le même résultat en tant que **StartWith (Action<IApplicationBuilder> app)**, à l’exception de l’application répond à `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="40876-362">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="40876-363">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="40876-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="40876-364">**Exécuter**</span><span class="sxs-lookup"><span data-stu-id="40876-364">**Run**</span></span>

<span data-ttu-id="40876-365">Le `Run` méthode démarre l’application web et bloque le thread appelant jusqu'à l’arrêt de l’ordinateur hôte :</span><span class="sxs-lookup"><span data-stu-id="40876-365">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="40876-366">**Start**</span><span class="sxs-lookup"><span data-stu-id="40876-366">**Start**</span></span>

<span data-ttu-id="40876-367">Exécuter l’hôte de façon non bloquant en appelant son `Start` méthode :</span><span class="sxs-lookup"><span data-stu-id="40876-367">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="40876-368">Si une liste d’URL est passée à la `Start` (méthode), il est à l’écoute sur les URL spécifiées :</span><span class="sxs-lookup"><span data-stu-id="40876-368">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="40876-369">Interface de IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="40876-369">IHostingEnvironment interface</span></span>

<span data-ttu-id="40876-370">Le [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) fournit des informations sur l’environnement d’hébergement web de l’application.</span><span class="sxs-lookup"><span data-stu-id="40876-370">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="40876-371">Utilisez [injection de constructeur](xref:fundamentals/dependency-injection) pour obtenir le `IHostingEnvironment` afin d’utiliser ses propriétés et les méthodes d’extension :</span><span class="sxs-lookup"><span data-stu-id="40876-371">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

<span data-ttu-id="40876-372">A [approche basée sur une convention](xref:fundamentals/environments#startup-conventions) peut être utilisé pour configurer l’application au démarrage sur l’environnement.</span><span class="sxs-lookup"><span data-stu-id="40876-372">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="40876-373">Vous pouvez également injecter le `IHostingEnvironment` dans le `Startup` constructeur à utiliser pour `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="40876-373">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> <span data-ttu-id="40876-374">Outre la `IsDevelopment` méthode d’extension, `IHostingEnvironment` offre `IsStaging`, `IsProduction`, et `IsEnvironment(string environmentName)` méthodes.</span><span class="sxs-lookup"><span data-stu-id="40876-374">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="40876-375">Consultez [fonctionne avec plusieurs environnements](xref:fundamentals/environments) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="40876-375">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="40876-376">Le `IHostingEnvironment` service peut également être injecté directement dans le `Configure` méthode pour configurer le pipeline de traitement :</span><span class="sxs-lookup"><span data-stu-id="40876-376">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

<span data-ttu-id="40876-377">`IHostingEnvironment`peuvent être injectées dans le `Invoke` méthode lors de la création personnalisée [intergiciel (middleware)](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="40876-377">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="40876-378">Interface de IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="40876-378">IApplicationLifetime interface</span></span>

<span data-ttu-id="40876-379">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) permet postérieur au démarrage et arrêt des activités.</span><span class="sxs-lookup"><span data-stu-id="40876-379">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="40876-380">Trois propriétés sur l’interface sont des jetons d’annulation utilisés pour inscrire `Action` méthodes qui définissent des événements de démarrage et d’arrêt.</span><span class="sxs-lookup"><span data-stu-id="40876-380">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="40876-381">Il existe également un `StopApplication` (méthode).</span><span class="sxs-lookup"><span data-stu-id="40876-381">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="40876-382">Jeton d’annulation</span><span class="sxs-lookup"><span data-stu-id="40876-382">Cancellation Token</span></span>    | <span data-ttu-id="40876-383">Déclenché lors de le &#8230;</span><span class="sxs-lookup"><span data-stu-id="40876-383">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="40876-384">L’ordinateur hôte a entièrement démarré.</span><span class="sxs-lookup"><span data-stu-id="40876-384">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="40876-385">L’hôte effectue un arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="40876-385">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="40876-386">Les demandes peuvent être encore en cours.</span><span class="sxs-lookup"><span data-stu-id="40876-386">Requests may still be processing.</span></span> <span data-ttu-id="40876-387">Blocs d’arrêt jusqu'à la fin de cet événement.</span><span class="sxs-lookup"><span data-stu-id="40876-387">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="40876-388">L’ordinateur hôte est la fin de l’arrêt approprié.</span><span class="sxs-lookup"><span data-stu-id="40876-388">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="40876-389">Toutes les demandes doivent être traités.</span><span class="sxs-lookup"><span data-stu-id="40876-389">All requests should be processed.</span></span> <span data-ttu-id="40876-390">Blocs d’arrêt jusqu'à la fin de cet événement.</span><span class="sxs-lookup"><span data-stu-id="40876-390">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="40876-391">Méthode</span><span class="sxs-lookup"><span data-stu-id="40876-391">Method</span></span>            | <span data-ttu-id="40876-392">Action</span><span class="sxs-lookup"><span data-stu-id="40876-392">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="40876-393">Arrêt de demandes de l’application actuelle.</span><span class="sxs-lookup"><span data-stu-id="40876-393">Requests termination of the current application.</span></span> |

```csharp
public class Startup 
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime) 
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="40876-394">Résolution des problèmes de System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="40876-394">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="40876-395">**S’applique à ASP.NET Core 2.0 uniquement**</span><span class="sxs-lookup"><span data-stu-id="40876-395">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="40876-396">Un ordinateur hôte peut-être être créé en injectant `IStartup` directement dans le conteneur d’injection de dépendance au lieu d’appeler `UseStartup` ou `Configure`:</span><span class="sxs-lookup"><span data-stu-id="40876-396">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="40876-397">Si l’hôte est construit de cette façon, l’erreur suivante peut se produire :</span><span class="sxs-lookup"><span data-stu-id="40876-397">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="40876-398">Cela se produit car le [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (l’assembly actuel) est requis pour analyser pour `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="40876-398">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="40876-399">Si l’application manuellement injecte `IStartup` dans le conteneur d’injection de dépendance, ajoutez l’appel suivant à `WebHostBuilder` avec le nom de l’assembly spécifié :</span><span class="sxs-lookup"><span data-stu-id="40876-399">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="40876-400">Vous pouvez également ajouter un fantôme `Configure` à la `WebHostBuilder`, qui affecte le `applicationName`(`ApplicationKey`) automatiquement :</span><span class="sxs-lookup"><span data-stu-id="40876-400">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="40876-401">**Remarque**: cela est uniquement nécessaire avec la version de ASP.NET Core 2.0 et uniquement lorsque l’application n’appelle pas `UseStartup` ou `Configure`.</span><span class="sxs-lookup"><span data-stu-id="40876-401">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="40876-402">Pour plus d’informations, consultez [annonces : Microsoft.Extensions.PlatformAbstractions a été supprimé (commentaire)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) et [StartupInjection exemple](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="40876-402">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="40876-403">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="40876-403">Additional resources</span></span>

* [<span data-ttu-id="40876-404">Héberger sur Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="40876-404">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="40876-405">Héberger sur Linux avec Nginx</span><span class="sxs-lookup"><span data-stu-id="40876-405">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="40876-406">Héberger sur Linux avec Apache</span><span class="sxs-lookup"><span data-stu-id="40876-406">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="40876-407">Ordinateur hôte dans un Service Windows</span><span class="sxs-lookup"><span data-stu-id="40876-407">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
