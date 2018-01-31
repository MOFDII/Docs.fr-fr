---
title: "Implémentations de serveurs web dans ASP.NET Core"
author: tdykstra
description: "Présente les serveurs web Kestrel et WebListener pour ASP.NET Core. Vous aide à déterminer quand les choisir et à quel moment les utiliser avec un serveur proxy inverse."
manager: wpickett
ms.author: tdykstra
ms.date: 08/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/index
ms.openlocfilehash: 9e2bea396e50615bd02affad93f0ee55255d299f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="950be-104">Implémentations de serveurs web dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="950be-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="950be-105">De [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73) et [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="950be-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="950be-106">Une application ASP.NET Core s’exécute avec une implémentation de serveur HTTP in-process.</span><span class="sxs-lookup"><span data-stu-id="950be-106">An ASP.NET Core application runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="950be-107">L’implémentation du serveur écoute les requêtes HTTP et les expose à l’application sous forme de [fonctionnalités de requêtes](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) composées dans `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="950be-107">The server implementation listens for HTTP requests and surfaces them to the application as sets of [request features](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) composed into an `HttpContext`.</span></span>

<span data-ttu-id="950be-108">ASP.NET Core est livré avec deux implémentations de serveurs :</span><span class="sxs-lookup"><span data-stu-id="950be-108">ASP.NET Core ships two server implementations:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="950be-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="950be-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="950be-110">[Kestrel](kestrel.md) est un serveur HTTP multiplateforme basé sur [libuv](https://github.com/libuv/libuv), une bibliothèque d’E/S asynchrones multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="950be-110">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="950be-111">[HTTP.sys](httpsys.md) est un serveur HTTP exclusivement Windows basé sur le [pilote de noyau Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="950be-111">[HTTP.sys](httpsys.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="950be-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="950be-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="950be-113">[Kestrel](kestrel.md) est un serveur HTTP multiplateforme basé sur [libuv](https://github.com/libuv/libuv), une bibliothèque d’E/S asynchrones multiplateforme.</span><span class="sxs-lookup"><span data-stu-id="950be-113">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="950be-114">[WebListener](weblistener.md) est un serveur HTTP exclusivement Windows basé sur le [pilote de noyau Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="950be-114">[WebListener](weblistener.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

---

## <a name="kestrel"></a><span data-ttu-id="950be-115">Kestrel</span><span class="sxs-lookup"><span data-stu-id="950be-115">Kestrel</span></span>

<span data-ttu-id="950be-116">Kestrel est le serveur web inclus par défaut dans les modèles de nouveaux projets ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="950be-116">Kestrel is the web server that's included by default in ASP.NET Core new-project templates.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="950be-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="950be-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="950be-118">Vous pouvez utiliser uniquement Kestrel ou l’associer à un *serveur proxy inverse*, par exemple IIS, Nginx ou Apache.</span><span class="sxs-lookup"><span data-stu-id="950be-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="950be-119">Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire.</span><span class="sxs-lookup"><span data-stu-id="950be-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel communique directement avec Internet sans serveur proxy inverse](kestrel/_static/kestrel-to-internet2.png)

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="950be-122">L’une ou l’autre des configurations, &mdash;avec ou sans serveur proxy inverse&mdash;, peut également être utilisée si Kestrel est exposé uniquement à un réseau interne.</span><span class="sxs-lookup"><span data-stu-id="950be-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

<span data-ttu-id="950be-123">Pour plus d’informations sur l’utilisation de Kestrel avec un proxy inverse, consultez [Présentation de Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="950be-123">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="950be-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="950be-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="950be-125">Si votre application accepte uniquement les requêtes d’un réseau interne, vous pouvez utiliser Kestrel de manière autonome.</span><span class="sxs-lookup"><span data-stu-id="950be-125">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel communique directement avec votre réseau interne](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="950be-127">Si vous exposez votre application à Internet, vous devez utiliser IIS, Nginx ou Apache en tant que *serveur proxy inverse*.</span><span class="sxs-lookup"><span data-stu-id="950be-127">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="950be-128">Un serveur proxy inverse reçoit les requêtes HTTP en provenance d’Internet et les transmet à Kestrel après un traitement préliminaire, comme illustré dans le diagramme suivant.</span><span class="sxs-lookup"><span data-stu-id="950be-128">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram.</span></span>

![Kestrel communique indirectement avec Internet via un serveur proxy inverse, par exemple IIS, Nginx ou Apache](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="950be-130">La raison la plus importante qui justifie l’utilisation d’un proxy inverse pour les déploiements Edge (exposés au trafic en provenance d’Internet) est la sécurité.</span><span class="sxs-lookup"><span data-stu-id="950be-130">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="950be-131">Les versions 1.x de Kestrel n’ont pas un ensemble complet de mesures de défense contre les attaques.</span><span class="sxs-lookup"><span data-stu-id="950be-131">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="950be-132">Cela inclut, entre autres choses, les délais d’attente appropriés, les limites de taille et les limites de connexions simultanées.</span><span class="sxs-lookup"><span data-stu-id="950be-132">This includes, but isn't limited to, appropriate timeouts, size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="950be-133">Pour plus d’informations sur l’utilisation de Kestrel avec un proxy inverse, consultez [Présentation de Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="950be-133">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

---

<span data-ttu-id="950be-134">Vous ne pouvez pas utiliser IIS, Nginx ou Apache sans Kestrel ou une [implémentation de serveur personnalisée](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="950be-134">You can't use IIS, Nginx, or Apache without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="950be-135">ASP.NET Core a été conçu pour s’exécuter dans son propre processus et se comporter de manière cohérente entre les plateformes.</span><span class="sxs-lookup"><span data-stu-id="950be-135">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="950be-136">IIS, Nginx et Apache dictent leur propre processus et environnement de démarrage. Pour les utiliser directement, ASP.NET Core doit s’adapter aux besoins de chacun d’eux.</span><span class="sxs-lookup"><span data-stu-id="950be-136">IIS, Nginx, and Apache dictate their own startup process and environment; to use them directly, ASP.NET Core would have to adapt to the needs of each one.</span></span> <span data-ttu-id="950be-137">L’utilisation d’une implémentation de serveur web telle que celle de Kestrel donne à ASP.NET Core un contrôle du processus et de l’environnement de démarrage.</span><span class="sxs-lookup"><span data-stu-id="950be-137">Using a web server implementation such as Kestrel gives ASP.NET Core control over the startup process and environment.</span></span> <span data-ttu-id="950be-138">Ainsi, plutôt que d’essayer d’adapter ASP.NET Core à IIS, Nginx ou Apache, configurez simplement ces serveurs web en serveur proxy des requêtes vers Kestrel.</span><span class="sxs-lookup"><span data-stu-id="950be-138">So rather than trying to adapt ASP.NET Core to IIS, Nginx, or Apache, you just set up those web servers to proxy requests to Kestrel.</span></span> <span data-ttu-id="950be-139">Grâce à cette solution, vos classes `Program.Main` et `Startup` restent essentiellement les mêmes, quel que soit l’emplacement de déploiement.</span><span class="sxs-lookup"><span data-stu-id="950be-139">This arrangement allows your `Program.Main` and `Startup` classes to be essentially the same no matter where you deploy.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="950be-140">IIS avec Kestrel</span><span class="sxs-lookup"><span data-stu-id="950be-140">IIS with Kestrel</span></span>

<span data-ttu-id="950be-141">Quand vous utilisez IIS ou IIS Express en tant que proxy inverse pour ASP.NET Core, l’application ASP.NET Core s’exécute dans un processus distinct du processus de travail IIS.</span><span class="sxs-lookup"><span data-stu-id="950be-141">When you use IIS or IIS Express as a reverse proxy for ASP.NET Core, the ASP.NET Core application runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="950be-142">Dans le processus IIS, un module IIS spécial s’exécute pour coordonner la relation du proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="950be-142">In the IIS process, a special IIS module runs to coordinate the reverse proxy relationship.</span></span>  <span data-ttu-id="950be-143">Il s’agit du *module ASP.NET Core*.</span><span class="sxs-lookup"><span data-stu-id="950be-143">This is the *ASP.NET Core Module*.</span></span> <span data-ttu-id="950be-144">Les principales fonctions du module ASP.NET Core consistent à démarrer l’application ASP.NET Core, la redémarrer quand elle se bloque et lui transférer le trafic HTTP.</span><span class="sxs-lookup"><span data-stu-id="950be-144">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core application, restart it when it crashes, and forward HTTP traffic to it.</span></span> <span data-ttu-id="950be-145">Pour plus d’informations, consultez [Module ASP.NET Core](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="950be-145">For more information, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="950be-146">Nginx avec Kestrel</span><span class="sxs-lookup"><span data-stu-id="950be-146">Nginx with Kestrel</span></span>

<span data-ttu-id="950be-147">Pour plus d’informations sur l’utilisation de Nginx sur Linux en tant que serveur proxy inverse pour Kestrel, consultez [Héberger sur Linux avec Nginx](xref:host-and-deploy/linux-nginx).</span><span class="sxs-lookup"><span data-stu-id="950be-147">For information about how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="950be-148">Apache avec Kestrel</span><span class="sxs-lookup"><span data-stu-id="950be-148">Apache with Kestrel</span></span>

<span data-ttu-id="950be-149">Pour plus d’informations sur l’utilisation d’Apache sur Linux en tant que serveur proxy inverse pour Kestrel, consultez [Héberger sur Linux avec Apache](xref:host-and-deploy/linux-apache).</span><span class="sxs-lookup"><span data-stu-id="950be-149">For information about how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="950be-150">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="950be-150">HTTP.sys</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="950be-151">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="950be-151">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="950be-152">Si vous exécutez votre application ASP.NET Core sur Windows, HTTP.sys est une alternative à Kestrel.</span><span class="sxs-lookup"><span data-stu-id="950be-152">If you run your ASP.NET Core app on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="950be-153">Vous pouvez utiliser HTTP.sys dans les scénarios où vous exposez votre application à Internet et où vous avez besoin des fonctionnalités HTTP.sys que Kestrel ne prend pas en charge.</span><span class="sxs-lookup"><span data-stu-id="950be-153">You can use HTTP.sys for scenarios where you expose your app to the Internet and you need HTTP.sys features that Kestrel doesn't support.</span></span> 

![HTTP.sys communique directement avec Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="950be-155">HTTP.sys peut également être utilisé pour les applications qui sont exposées uniquement à un réseau interne.</span><span class="sxs-lookup"><span data-stu-id="950be-155">HTTP.sys can also be used for applications that are exposed only to an internal network.</span></span> 

![HTTP.sys communique directement avec votre réseau interne](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="950be-157">Dans le cas des réseaux internes, Kestrel est généralement recommandé pour obtenir les meilleures performances. Toutefois, dans certains scénarios, vous pouvez être amené à utiliser une fonctionnalité uniquement offerte par HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="950be-157">For internal network scenarios, Kestrel is generally recommended for best performance; but in some scenarios, you might want to use a feature that only HTTP.sys offers.</span></span> <span data-ttu-id="950be-158">Pour plus d’informations sur les fonctionnalités de HTTP.sys, consultez [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="950be-158">For information about HTTP.sys features, see [HTTP.sys](httpsys.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="950be-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="950be-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="950be-160">HTTP.sys se nomme WebListener dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="950be-160">HTTP.sys is named WebListener in ASP.NET Core 1.x.</span></span> <span data-ttu-id="950be-161">Si vous exécutez votre application ASP.NET Core sur Windows, WebListener est une alternative que vous pouvez utiliser dans les scénarios où vous souhaitez exposer votre application à Internet, mais où vous ne pouvez pas recourir à IIS.</span><span class="sxs-lookup"><span data-stu-id="950be-161">If you run your ASP.NET Core app on Windows, WebListener is an alternative that you can use for scenarios where you want to expose your app to the Internet but you can't use IIS.</span></span>

![WebListener communique directement avec Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="950be-163">WebListener peut également être utilisé à la place de Kestrel pour les applications exposées uniquement à un réseau interne, si vous avez besoin de fonctionnalités WebListener non prises en charge par Kestrel.</span><span class="sxs-lookup"><span data-stu-id="950be-163">WebListener can also be used in place of Kestrel for applications that are exposed only to an internal network, if you need WebListener features that Kestrel doesn't support.</span></span> 

![WebListener communique directement avec votre réseau interne](weblistener/_static/weblistener-to-internal.png)

<span data-ttu-id="950be-165">Dans le cas des réseaux internes, Kestrel est généralement recommandé pour obtenir les meilleures performances. Toutefois, dans certains scénarios, vous pouvez être amené à utiliser une fonctionnalité uniquement offerte par WebListener.</span><span class="sxs-lookup"><span data-stu-id="950be-165">For internal network scenarios, Kestrel is generally recommended for best performance, but in some scenarios you might want to use a feature that only WebListener offers.</span></span> <span data-ttu-id="950be-166">Pour plus d’informations sur les fonctionnalités de WebListener, consultez [WebListener](weblistener.md).</span><span class="sxs-lookup"><span data-stu-id="950be-166">For information about WebListener features, see [WebListener](weblistener.md).</span></span>

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a><span data-ttu-id="950be-167">Remarques sur l’infrastructure serveur d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="950be-167">Notes about ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="950be-168">Le [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) disponible dans la méthode `Configure` de la classe `Startup` expose la propriété `ServerFeatures` de type [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="950be-168">The [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup` class `Configure` method exposes the `ServerFeatures` property of type [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="950be-169">Kestrel et WebListener exposent uniquement la fonctionnalité [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) mais les autres implémentations de serveurs peuvent exposer des fonctionnalités supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="950be-169">Kestrel and WebListener both expose only a single feature, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="950be-170">`IServerAddressesFeature` permet de déterminer quel est le port lié à l’implémentation de serveur au moment de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="950be-170">`IServerAddressesFeature` can be used to find out which port the server implementation has bound to at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="950be-171">Serveurs personnalisés</span><span class="sxs-lookup"><span data-stu-id="950be-171">Custom servers</span></span>

<span data-ttu-id="950be-172">Si les serveurs intégrés ne répondent pas à vos besoins, vous pouvez créer une implémentation de serveur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="950be-172">If the built-in servers don't meet your needs, you can create a custom server implementation.</span></span> <span data-ttu-id="950be-173">Le [guide OWIN (Open Web Interface pour .NET)](../owin.md) montre comment écrire une implémentation [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) basée sur [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="950be-173">The [Open Web Interface for .NET (OWIN) guide](../owin.md) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="950be-174">Vous êtes libre d’implémenter uniquement les interfaces de fonctionnalités dont votre application a besoin. Toutefois, vous devez assurer au minimum la prise en charge de [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) et [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="950be-174">You're free to implement just the feature interfaces your application needs, though at a minimum you must support [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span></span>

## <a name="next-steps"></a><span data-ttu-id="950be-175">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="950be-175">Next steps</span></span>

<span data-ttu-id="950be-176">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="950be-176">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="950be-177">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="950be-177">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

- [<span data-ttu-id="950be-178">Kestrel</span><span class="sxs-lookup"><span data-stu-id="950be-178">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="950be-179">Kestrel avec IIS</span><span class="sxs-lookup"><span data-stu-id="950be-179">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="950be-180">Héberger sur Linux avec Nginx</span><span class="sxs-lookup"><span data-stu-id="950be-180">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="950be-181">Héberger sur Linux avec Apache</span><span class="sxs-lookup"><span data-stu-id="950be-181">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="950be-182">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="950be-182">HTTP.sys</span></span>](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="950be-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="950be-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

- [<span data-ttu-id="950be-184">Kestrel</span><span class="sxs-lookup"><span data-stu-id="950be-184">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="950be-185">Kestrel avec IIS</span><span class="sxs-lookup"><span data-stu-id="950be-185">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="950be-186">Héberger sur Linux avec Nginx</span><span class="sxs-lookup"><span data-stu-id="950be-186">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="950be-187">Héberger sur Linux avec Apache</span><span class="sxs-lookup"><span data-stu-id="950be-187">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="950be-188">WebListener</span><span class="sxs-lookup"><span data-stu-id="950be-188">WebListener</span></span>](weblistener.md)

---
