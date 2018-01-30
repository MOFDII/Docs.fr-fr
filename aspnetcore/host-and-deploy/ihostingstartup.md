---
title: "Ajouter des fonctionnalités de l’application à partir d’un assembly externe à l’aide de IHostingStartup dans ASP.NET Core"
author: guardrex
description: "Découvrez comment ajouter des fonctionnalités à une application ASP.NET Core à partir d’un assembly externe à l’aide d’une implémentation IHostingStartup."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/ihostingstartup
ms.openlocfilehash: bd2446d6133e0c06dc14509271c2d17be4c95b63
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="add-app-features-from-an-external-assembly-using-ihostingstartup-in-aspnet-core"></a><span data-ttu-id="76621-103">Ajouter des fonctionnalités de l’application à partir d’un assembly externe à l’aide de IHostingStartup dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="76621-103">Add app features from an external assembly using IHostingStartup in ASP.NET Core</span></span>

<span data-ttu-id="76621-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="76621-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="76621-105">Un [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implémentation permet d’ajouter des fonctionnalités à une application au démarrage à partir de l’application en dehors de `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="76621-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding features to an app at startup from outside of the app's `Startup` class.</span></span> <span data-ttu-id="76621-106">Par exemple, une bibliothèque d’outils externes permettre utiliser un `IHostingStartup` l’implémentation pour fournir des services à une application ou fournisseurs de configuration supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="76621-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="76621-107">`IHostingStartup`*est disponible dans ASP.NET Core 2.0 et versions ultérieures.*</span><span class="sxs-lookup"><span data-stu-id="76621-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="76621-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="76621-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="76621-109">Détecter les assemblys de démarrage hébergement chargés</span><span class="sxs-lookup"><span data-stu-id="76621-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="76621-110">Pour découvrir les assemblys de démarrage d’hébergement chargé par l’application ou par des bibliothèques, activez la journalisation, puis vérifiez les journaux des applications.</span><span class="sxs-lookup"><span data-stu-id="76621-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="76621-111">Les erreurs qui se produisent lors du chargement d’assemblys sont enregistrées.</span><span class="sxs-lookup"><span data-stu-id="76621-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="76621-112">Assemblys de démarrage hébergement chargés sont enregistrées au niveau du débogage, et toutes les erreurs sont journalisées.</span><span class="sxs-lookup"><span data-stu-id="76621-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="76621-113">Les lectures d’application exemple le le [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) dans un `string` de tableau et affiche le résultat dans la page d’Index de l’application :</span><span class="sxs-lookup"><span data-stu-id="76621-113">The sample app reads the the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[Main](ihostingstartup/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="76621-114">Désactiver le chargement automatique des assemblys de démarrage d’hébergement</span><span class="sxs-lookup"><span data-stu-id="76621-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="76621-115">Il existe deux méthodes pour désactiver le chargement automatique des assemblys de démarrage d’hébergement :</span><span class="sxs-lookup"><span data-stu-id="76621-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="76621-116">Définir le [empêcher le démarrage d’hébergement](xref:fundamentals/hosting#prevent-hosting-startup) héberger le paramètre de configuration.</span><span class="sxs-lookup"><span data-stu-id="76621-116">Set the [Prevent Hosting Startup](xref:fundamentals/hosting#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="76621-117">Définir le `ASPNETCORE_preventHostingStartup` variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="76621-117">Set the `ASPNETCORE_preventHostingStartup` environment variable.</span></span>

<span data-ttu-id="76621-118">Lorsque le paramètre d’hôte ou de la variable d’environnement est définie sur `true` ou `1`, l’hébergement des assemblys de démarrage ne sont pas chargés automatiquement.</span><span class="sxs-lookup"><span data-stu-id="76621-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="76621-119">Si les deux sont définis, le paramètre de l’hôte détermine le comportement.</span><span class="sxs-lookup"><span data-stu-id="76621-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="76621-120">Désactivation des assemblys de démarrage hébergement à l’aide de la variable d’environnement ou le paramètre hôte les désactive globalement et peut désactiver plusieurs fonctionnalités d’une application.</span><span class="sxs-lookup"><span data-stu-id="76621-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several features of an app.</span></span> <span data-ttu-id="76621-121">Il n’est pas encore possible de désactiver de manière sélective un hébergement assembly de démarrage ajouté par une bibliothèque, à moins que la bibliothèque propose sa propre option de configuration.</span><span class="sxs-lookup"><span data-stu-id="76621-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="76621-122">Une version ultérieure propose la possibilité de désactiver de manière sélective les assemblys de démarrage hébergement (consultez [GitHub émettre aspnet/hébergement # entré 1243](https://github.com/aspnet/Hosting/pull/1243)).</span><span class="sxs-lookup"><span data-stu-id="76621-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup-features"></a><span data-ttu-id="76621-123">Implémenter des fonctionnalités de IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="76621-123">Implement IHostingStartup features</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="76621-124">Créer l’assembly</span><span class="sxs-lookup"><span data-stu-id="76621-124">Create the assembly</span></span>

<span data-ttu-id="76621-125">Un `IHostingStartup` déployée en tant qu’assembly basé sur une application console sans point d’entrée.</span><span class="sxs-lookup"><span data-stu-id="76621-125">An `IHostingStartup` feature is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="76621-126">Les références d’assembly le [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package :</span><span class="sxs-lookup"><span data-stu-id="76621-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[Main](ihostingstartup/snapshot_sample/StartupFeature.csproj)]

<span data-ttu-id="76621-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribut identifie une classe en tant qu’implémentation de `IHostingStartup` pour le chargement et l’exécution lors de la génération du [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="76621-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="76621-128">Dans l’exemple suivant, l’espace de noms est `StartupFeature`, et la classe est `StartupFeatureHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="76621-128">In the following example, the namespace is `StartupFeature`, and the class is `StartupFeatureHostingStartup`:</span></span>

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet1)]

<span data-ttu-id="76621-129">Une classe implémente `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="76621-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="76621-130">La classe [configurer](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) utilise un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) pour ajouter des fonctionnalités à une application :</span><span class="sxs-lookup"><span data-stu-id="76621-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add features to an app:</span></span>

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="76621-131">Lors de la génération un `IHostingStartup` de projet, le fichier de dépendances (*\*. deps.json*) définit les `runtime` l’emplacement de l’assembly à la *bin* dossier :</span><span class="sxs-lookup"><span data-stu-id="76621-131">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="76621-132">Seule une partie du fichier est affichée.</span><span class="sxs-lookup"><span data-stu-id="76621-132">Only part of the file is shown.</span></span> <span data-ttu-id="76621-133">Le nom de l’assembly dans l’exemple est `StartupFeature`.</span><span class="sxs-lookup"><span data-stu-id="76621-133">The assembly name in the example is `StartupFeature`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="76621-134">Mettre à jour le fichier de dépendances</span><span class="sxs-lookup"><span data-stu-id="76621-134">Update the dependencies file</span></span>

<span data-ttu-id="76621-135">L’emplacement d’exécution est spécifié dans le  *\*. deps.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="76621-135">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="76621-136">Pour active la fonctionnalité, le `runtime` élément doit spécifier l’emplacement de l’assembly du runtime de la fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="76621-136">To active the feature, the `runtime` element must specify the location of the feature's runtime assembly.</span></span> <span data-ttu-id="76621-137">Préfixe la `runtime` emplacement avec `lib/netcoreapp2.0/`:</span><span class="sxs-lookup"><span data-stu-id="76621-137">Prefix the `runtime` location with `lib/netcoreapp2.0/`:</span></span>

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="76621-138">Dans l’exemple d’application, la modification de la  *\*. deps.json* fichier est effectué par un [PowerShell](/powershell/scripting/powershell-scripting) script.</span><span class="sxs-lookup"><span data-stu-id="76621-138">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="76621-139">Le script PowerShell est automatiquement déclenché par une cible de génération dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="76621-139">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="feature-activation"></a><span data-ttu-id="76621-140">Activation de fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="76621-140">Feature activation</span></span>

<span data-ttu-id="76621-141">**Placez le fichier d’assembly**</span><span class="sxs-lookup"><span data-stu-id="76621-141">**Place the assembly file**</span></span>

<span data-ttu-id="76621-142">Le `IHostingStartup` fichier d’assembly de l’implémentation doit être *bin*-déployés dans l’application ou placé dans le [magasin runtime](/dotnet/core/deploying/runtime-store):</span><span class="sxs-lookup"><span data-stu-id="76621-142">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="76621-143">Pour une utilisation par l’utilisateur, placez l’assembly dans le magasin de runtime du profil utilisateur à :</span><span class="sxs-lookup"><span data-stu-id="76621-143">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="76621-144">Dans le monde entier, placez l’assembly dans le magasin d’exécution de l’installation de .NET Core :</span><span class="sxs-lookup"><span data-stu-id="76621-144">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="76621-145">Lors du déploiement de l’assembly dans le magasin de l’exécution, le fichier de symboles peut-être être déployé également, mais n’est pas nécessaire pour la fonctionnalité soit opérationnelle.</span><span class="sxs-lookup"><span data-stu-id="76621-145">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the feature to work.</span></span>

<span data-ttu-id="76621-146">**Placez le fichier de dépendances**</span><span class="sxs-lookup"><span data-stu-id="76621-146">**Place the dependencies file**</span></span>

<span data-ttu-id="76621-147">La mise en oeuvre  *\*. deps.json* fichier doit être dans un emplacement accessible.</span><span class="sxs-lookup"><span data-stu-id="76621-147">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="76621-148">Pour une utilisation par l’utilisateur, placez le fichier dans le `additonalDeps` dossier du profil utilisateur `.dotnet` paramètres :</span><span class="sxs-lookup"><span data-stu-id="76621-148">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span> 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="76621-149">Dans le monde entier, placez le fichier dans le `additonalDeps` dossier de l’installation de .NET Core :</span><span class="sxs-lookup"><span data-stu-id="76621-149">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="76621-150">Notez le numéro de version, `2.0.0`, reflète la version du runtime partagé utilisé par l’application cible.</span><span class="sxs-lookup"><span data-stu-id="76621-150">Note the version, `2.0.0`, reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="76621-151">Le runtime partagé est indiqué dans le  *\*. runtimeconfig.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="76621-151">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="76621-152">Dans l’exemple d’application, le runtime partagé est spécifié dans le *HostingStartupSample.runtimeconfig.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="76621-152">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="76621-153">**Jeu de variables d’environnement**</span><span class="sxs-lookup"><span data-stu-id="76621-153">**Set environment variables**</span></span>

<span data-ttu-id="76621-154">Définissez les variables d’environnement suivantes dans le contexte de l’application qui utilise la fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="76621-154">Set the following environment variables in the context of the app that uses the feature.</span></span>

<span data-ttu-id="76621-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="76621-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="76621-156">Seuls les assemblys démarrage hébergement sont analysés pour la `HostingStartupAttribute`.</span><span class="sxs-lookup"><span data-stu-id="76621-156">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="76621-157">Le nom de l’assembly de l’implémentation est fourni dans cette variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="76621-157">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="76621-158">L’exemple d’application définit cette valeur sur `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="76621-158">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="76621-159">La valeur peut également être définie à l’aide de la [hébergeant les assemblys de démarrage](xref:fundamentals/hosting#hosting-startup-assemblies) héberger le paramètre de configuration.</span><span class="sxs-lookup"><span data-stu-id="76621-159">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/hosting#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="76621-160">DOTNET\_SUPPLÉMENTAIRES\_DÉP.</span><span class="sxs-lookup"><span data-stu-id="76621-160">DOTNET\_ADDITIONAL\_DEPS</span></span>

<span data-ttu-id="76621-161">L’emplacement de la mise en oeuvre  *\*. deps.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="76621-161">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="76621-162">Si le fichier est placé dans le profil d’utilisateur *.dotnet* dossier pour une utilisation de l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="76621-162">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="76621-163">Si le fichier est placé dans l’installation de .NET Core dans le monde entier, fournissez le chemin d’accès complet au fichier :</span><span class="sxs-lookup"><span data-stu-id="76621-163">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="76621-164">L’exemple d’application définit cette valeur sur :</span><span class="sxs-lookup"><span data-stu-id="76621-164">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="76621-165">Pour obtenir des exemples montrant comment définir des variables d’environnement pour les différents systèmes d’exploitation, consultez [fonctionne avec plusieurs environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="76621-165">For examples of how to set environment variables for various operating systems, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="76621-166">Exemple d’application</span><span class="sxs-lookup"><span data-stu-id="76621-166">Sample app</span></span>

<span data-ttu-id="76621-167">Le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)) utilise `IHostingStartup` pour créer un outil de diagnostic.</span><span class="sxs-lookup"><span data-stu-id="76621-167">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="76621-168">L’outil ajoute deux middlewares à l’application au démarrage, qui fournissent des informations de diagnostic :</span><span class="sxs-lookup"><span data-stu-id="76621-168">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="76621-169">Services enregistrés</span><span class="sxs-lookup"><span data-stu-id="76621-169">Registered services</span></span>
* <span data-ttu-id="76621-170">Adresse : schéma, hôte, chemin d’accès de base, chemin d’accès, chaîne de requête</span><span class="sxs-lookup"><span data-stu-id="76621-170">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="76621-171">Connexion : adresse IP distante, port distant, adresse IP locale, port local, le certificat client</span><span class="sxs-lookup"><span data-stu-id="76621-171">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="76621-172">En-têtes de demande</span><span class="sxs-lookup"><span data-stu-id="76621-172">Request headers</span></span>
* <span data-ttu-id="76621-173">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="76621-173">Environment variables</span></span>

<span data-ttu-id="76621-174">Pour exécuter l’exemple :</span><span class="sxs-lookup"><span data-stu-id="76621-174">To run the sample:</span></span>

1. <span data-ttu-id="76621-175">Le projet de démarrage du Diagnostic utilise [PowerShell](/powershell/scripting/powershell-scripting) pour modifier son *StartupDiagnostics.deps.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="76621-175">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="76621-176">PowerShell est installé par défaut sur le système d’exploitation Windows depuis Windows 7 SP1 et Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="76621-176">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="76621-177">Pour obtenir de PowerShell sur d’autres plateformes, consultez [installation de Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="76621-177">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="76621-178">Générez le projet de démarrage du Diagnostic.</span><span class="sxs-lookup"><span data-stu-id="76621-178">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="76621-179">Une cible de génération dans le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="76621-179">A build target in the project file:</span></span>
   * <span data-ttu-id="76621-180">Déplace l’assembly et des symboles des fichiers au magasin de runtime du profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="76621-180">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="76621-181">Déclenche le script PowerShell pour modifier la *StartupDiagnostics.deps.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="76621-181">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="76621-182">Déplace le *StartupDiagnostics.deps.json* fichier pour le profil d’utilisateur `additionalDeps` dossier.</span><span class="sxs-lookup"><span data-stu-id="76621-182">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="76621-183">Définissez les variables d’environnement :</span><span class="sxs-lookup"><span data-stu-id="76621-183">Set the environment variables:</span></span>
    * <span data-ttu-id="76621-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="76621-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="76621-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="76621-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="76621-186">Exécutez l’exemple d’application.</span><span class="sxs-lookup"><span data-stu-id="76621-186">Run the sample app.</span></span>
5. <span data-ttu-id="76621-187">Demander le `/services` point de terminaison pour voir l’application inscrit des services.</span><span class="sxs-lookup"><span data-stu-id="76621-187">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="76621-188">Demander le `/diag` point de terminaison pour afficher les informations de diagnostic.</span><span class="sxs-lookup"><span data-stu-id="76621-188">Request the `/diag` endpoint to see the diagnostic information.</span></span>
