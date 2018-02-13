---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: "Quelles sont les nouveautés dans ASP.NET Web API 2.1 | Documents Microsoft"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-21"></a><span data-ttu-id="98219-102">Quelles sont les nouveautés dans ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="98219-102">What's New in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="98219-103">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="98219-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="98219-104">Cette rubrique décrit les nouveautés de ASP.NET Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="98219-104">This topic describes what's new for ASP.NET Web API 2.1.</span></span>

- [<span data-ttu-id="98219-105">Télécharger</span><span class="sxs-lookup"><span data-stu-id="98219-105">Download</span></span>](#download)
- [<span data-ttu-id="98219-106">Documentation</span><span class="sxs-lookup"><span data-stu-id="98219-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="98219-107">Nouvelles fonctionnalités dans ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="98219-107">New Features in ASP.NET Web API 2.1</span></span>](#new-features)

    - [<span data-ttu-id="98219-108">Gestion des erreurs global</span><span class="sxs-lookup"><span data-stu-id="98219-108">Global Error Handling</span></span>](#global-error)
    - [<span data-ttu-id="98219-109">Améliorations du routage d’attribut</span><span class="sxs-lookup"><span data-stu-id="98219-109">Attribute Routing Improvements</span></span>](#attribute-routing)
    - [<span data-ttu-id="98219-110">Améliorations de la Page</span><span class="sxs-lookup"><span data-stu-id="98219-110">Help Page Improvements</span></span>](#help-page)
    - [<span data-ttu-id="98219-111">Prise en charge IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="98219-111">IgnoreRoute Support</span></span>](#ignoreroute)
    - [<span data-ttu-id="98219-112">Formateur de Type de média BSON</span><span class="sxs-lookup"><span data-stu-id="98219-112">BSON Media-Type Formatter</span></span>](#bson)
    - [<span data-ttu-id="98219-113">Meilleure prise en charge pour les filtres de Async</span><span class="sxs-lookup"><span data-stu-id="98219-113">Better Support for Async Filters</span></span>](#async-filters)
    - [<span data-ttu-id="98219-114">Requête d’analyse pour le Client de mise en forme de la bibliothèque</span><span class="sxs-lookup"><span data-stu-id="98219-114">Query Parsing for the Client Formatting Library</span></span>](#query-parsing)
- [<span data-ttu-id="98219-115">Problèmes connus et les modifications avec rupture</span><span class="sxs-lookup"><span data-stu-id="98219-115">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="98219-116">Correctifs de bogues</span><span class="sxs-lookup"><span data-stu-id="98219-116">Bug Fixes</span></span>](#bug-fixes)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="98219-117">Téléchargement</span><span class="sxs-lookup"><span data-stu-id="98219-117">Download</span></span>

<span data-ttu-id="98219-118">Les fonctions d’exécution sont publiées en tant que packages NuGet lors de la galerie NuGet.</span><span class="sxs-lookup"><span data-stu-id="98219-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="98219-119">Tous les packages de runtime suivent le [contrôle de version sémantique](http://semver.org/) spécification.</span><span class="sxs-lookup"><span data-stu-id="98219-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="98219-120">Le dernier package RTM d’ASP.NET Web API 2.1 a la version suivante : « 5.1.2 ».</span><span class="sxs-lookup"><span data-stu-id="98219-120">The latest ASP.NET Web API 2.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="98219-121">Vous pouvez installer ou mettre à jour ces packages via [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="98219-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="98219-122">Cette version inclut également des packages localisés correspondants sur NuGet.</span><span class="sxs-lookup"><span data-stu-id="98219-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="98219-123">Vous pouvez installer ou mettre à jour pour les packages NuGet publiés à l’aide de la Console du Gestionnaire de Package NuGet :</span><span class="sxs-lookup"><span data-stu-id="98219-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="98219-124">Documentation</span><span class="sxs-lookup"><span data-stu-id="98219-124">Documentation</span></span>

<span data-ttu-id="98219-125">Didacticiels et autres informations sur ASP.NET Web API 2.1 RTM sont disponibles à partir du site web ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="98219-125">Tutorials and other information about ASP.NET Web API 2.1 RTM are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a><span data-ttu-id="98219-126">Nouvelles fonctionnalités dans ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="98219-126">New Features in ASP.NET Web API 2.1</span></span>

<a id="global-error"></a>
### <a name="global-error-handling"></a><span data-ttu-id="98219-127">Gestion des erreurs global</span><span class="sxs-lookup"><span data-stu-id="98219-127">Global Error Handling</span></span>

<span data-ttu-id="98219-128">Toutes les exceptions non gérées peuvent maintenant être consignées via un mécanisme central, et le comportement des exceptions non gérées peut être personnalisé.</span><span class="sxs-lookup"><span data-stu-id="98219-128">All unhandled exceptions can now be logged through one central mechanism, and the behavior for unhandled exceptions can be customized.</span></span>

<span data-ttu-id="98219-129">Le framework prend en charge plusieurs exceptions enregistreurs d’événements, qui consultez l’exception non gérée et des informations sur le contexte dans lequel elle s’est produite, la demande en cours de traitement en temps.</span><span class="sxs-lookup"><span data-stu-id="98219-129">The framework supports multiple exception loggers, which all see the unhandled exception and information about the context in which it occurred, such as the request being processed at the time.</span></span>

<span data-ttu-id="98219-130">Par exemple, le code suivant utilise System.Diagnostics.TraceSource pour se connecter à toutes les exceptions non gérées :</span><span class="sxs-lookup"><span data-stu-id="98219-130">For example, the following code uses System.Diagnostics.TraceSource to log all unhandled exceptions:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

<span data-ttu-id="98219-131">Vous pouvez également remplacer le Gestionnaire d’exceptions par défaut, afin que vous puissiez personnaliser entièrement le message de réponse HTTP envoyé lorsqu’une exception non gérée se produit.</span><span class="sxs-lookup"><span data-stu-id="98219-131">You can also replace the default exception handler, so that you can fully customize the HTTP response message that is sent when an unhandled exception occurs.</span></span>

<span data-ttu-id="98219-132">Nous vous avons fourni un [exemple](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) qui consigne non prise en charge toutes les exceptions via l’infrastructure ELMAH populaires.</span><span class="sxs-lookup"><span data-stu-id="98219-132">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) that logs all unhandled exceptions via the popular ELMAH framework.</span></span>

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="98219-133">Améliorations du routage d’attribut</span><span class="sxs-lookup"><span data-stu-id="98219-133">Attribute Routing Improvements</span></span>

<span data-ttu-id="98219-134">Attribut routage maintenant prend en charge les contraintes, l’activation de contrôle de version et la sélection des itinéraires basés sur l’en-tête.</span><span class="sxs-lookup"><span data-stu-id="98219-134">Attribute routing now supports constraints, enabling versioning and header-based route selection.</span></span> <span data-ttu-id="98219-135">En outre, de nombreux aspects d’itinéraires d’attribut sont maintenant personnalisables via la **IDirectRouteFactory** interface et **RouteFactoryAttribute** classe.</span><span class="sxs-lookup"><span data-stu-id="98219-135">Also, many aspects of attribute routes are now customizable via the **IDirectRouteFactory** interface and **RouteFactoryAttribute** class.</span></span> <span data-ttu-id="98219-136">Le préfixe d’itinéraire est désormais extensible via la **IRoutePrefix** interface et **RoutePrefixAttribute** classe.</span><span class="sxs-lookup"><span data-stu-id="98219-136">The route prefix is now extensible via the **IRoutePrefix** interface and **RoutePrefixAttribute** class.</span></span>

<span data-ttu-id="98219-137">Nous vous avons fourni un [exemple](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) qui utilise les contraintes pour filtrer dynamiquement des contrôleurs par un en-tête HTTP de « api-version ».</span><span class="sxs-lookup"><span data-stu-id="98219-137">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) that uses constraints to dynamically filter controllers by an 'api-version' HTTP header.</span></span>

<a id="help-page"></a>
### <a name="help-page-improvements"></a><span data-ttu-id="98219-138">Améliorations de la Page</span><span class="sxs-lookup"><span data-stu-id="98219-138">Help Page Improvements</span></span>

<span data-ttu-id="98219-139">Web API 2.1 inclut les améliorations suivantes à [Pages d’aide API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span><span class="sxs-lookup"><span data-stu-id="98219-139">Web API 2.1 includes the following enhancements to [API Help Pages](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span></span>

- <span data-ttu-id="98219-140">Documentation des propriétés individuelles de paramètres ou types de retour des actions.</span><span class="sxs-lookup"><span data-stu-id="98219-140">Documentation of individual properties of parameters or return types of actions.</span></span>
- <span data-ttu-id="98219-141">Documentation des annotations de modèle de données.</span><span class="sxs-lookup"><span data-stu-id="98219-141">Documentation of data model annotations.</span></span>

<span data-ttu-id="98219-142">La conception de l’interface utilisateur des pages d’aide a été également mis à jour, pour prendre en compte ces modifications.</span><span class="sxs-lookup"><span data-stu-id="98219-142">The UI design of the help pages was also updated, to accomodate these changes.</span></span>

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a><span data-ttu-id="98219-143">Prise en charge IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="98219-143">IgnoreRoute Support</span></span>

<span data-ttu-id="98219-144">Web prend en charge l’API 2.1 en ignorant des modèles d’URL de routage d’API Web, via un ensemble de **IgnoreRoute** méthodes d’extension sur **HttpRouteCollection**.</span><span class="sxs-lookup"><span data-stu-id="98219-144">Web API 2.1 supports ignoring URL patterns in Web API routing, through a set of **IgnoreRoute** extension methods on **HttpRouteCollection**.</span></span> <span data-ttu-id="98219-145">Ces méthodes provoquent des API Web ignorer toutes les URL qui correspondent à un modèle spécifié et permettent à l’hôte appliquer un traitement supplémentaire si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="98219-145">These methods cause Web API to ignore any URLs that match a specified template, and allow the host to apply additional processing if appropriate.</span></span>

<span data-ttu-id="98219-146">L’exemple suivant ignore les URI qui commencent par un &quot;contenu&quot; segment :</span><span class="sxs-lookup"><span data-stu-id="98219-146">The following example ignores URIs that start with a &quot;content&quot; segment:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a><span data-ttu-id="98219-147">Formateur de Type de média BSON</span><span class="sxs-lookup"><span data-stu-id="98219-147">BSON Media-Type Formatter</span></span>

<span data-ttu-id="98219-148">Web API prend en charge la [BSON](http://bsonspec.org/) format de transmission, à la fois sur le client et sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="98219-148">Web API now supports the [BSON](http://bsonspec.org/) wire format, both on the client and on the server.</span></span>

<span data-ttu-id="98219-149">Pour activer BSON côté serveur, ajoutez le **BsonMediaTypeFormatter** à la collection de formateurs :</span><span class="sxs-lookup"><span data-stu-id="98219-149">To enable BSON on the server side, add the **BsonMediaTypeFormatter** to the formatters collection:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

<span data-ttu-id="98219-150">Voici comment un client .NET peut utiliser le format de BSON :</span><span class="sxs-lookup"><span data-stu-id="98219-150">Here is how a .NET client can consume BSON format:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

<span data-ttu-id="98219-151">Nous vous avons fourni un [exemple](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) qui affiche à la fois le client et côté serveur.</span><span class="sxs-lookup"><span data-stu-id="98219-151">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) that shows both the client and server side.</span></span>

<span data-ttu-id="98219-152">Pour plus d’informations, consultez [prise en charge BSON Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span><span class="sxs-lookup"><span data-stu-id="98219-152">For more information, see [BSON Support in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span></span>

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a><span data-ttu-id="98219-153">Meilleure prise en charge pour les filtres de Async</span><span class="sxs-lookup"><span data-stu-id="98219-153">Better Support for Async Filters</span></span>

<span data-ttu-id="98219-154">API Web prend désormais en charge un moyen simple pour créer des filtres qui s’exécutent de façon asynchrone.</span><span class="sxs-lookup"><span data-stu-id="98219-154">Web API now supports an easy way to create filters that execute asynchronously.</span></span> <span data-ttu-id="98219-155">Cette fonctionnalité est utile est votre filtre a besoin d’exécuter une action asynchrone, tels que l’accès une base de données.</span><span class="sxs-lookup"><span data-stu-id="98219-155">This feature is useful is your filter needs to perform an async action, such as access a database.</span></span> <span data-ttu-id="98219-156">Auparavant, pour créer un filtre async, vous deviez implémenter vous-même, l’interface de filtre, car les classes de base du filtre exposée uniquement les méthodes synchrones.</span><span class="sxs-lookup"><span data-stu-id="98219-156">Previously, to create an async filter, you had to implement the filter interface yourself, because the filter base classes only exposed synchronous methods.</span></span> <span data-ttu-id="98219-157">Maintenant vous pouvez remplacer le serveur virtuel `On*Async` méthodes du filtre de classe de base.</span><span class="sxs-lookup"><span data-stu-id="98219-157">Now you can override the virtual `On*Async` methods of the filter base class.</span></span>

<span data-ttu-id="98219-158">Exemple :</span><span class="sxs-lookup"><span data-stu-id="98219-158">For example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

<span data-ttu-id="98219-159">Le **AuthorizationFilterAttribute**, **ActionFilterAttribute**, et **ExceptionFilterAttribute** toutes les classes de prise en charge async Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="98219-159">The **AuthorizationFilterAttribute**, **ActionFilterAttribute**, and **ExceptionFilterAttribute** classes all support async in Web API 2.1.</span></span>

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a><span data-ttu-id="98219-160">Requête d’analyse pour le Client de mise en forme de la bibliothèque</span><span class="sxs-lookup"><span data-stu-id="98219-160">Query Parsing for the Client Formatting Library</span></span>

<span data-ttu-id="98219-161">Auparavant, **System.Net.Http.Formatting** prise en charge de l’analyse et de mise à jour des requêtes d’URI pour le code côté serveur, mais la bibliothèque portable équivalente n’a pas cette fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="98219-161">Previously, **System.Net.Http.Formatting** supported parsing and updating URI queries for server-side code, but the equivalent portable library was missing this feature.</span></span> <span data-ttu-id="98219-162">2.1 d’API Web, une application cliente peut désormais facilement analyser et mettre à jour une chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="98219-162">In Web API 2.1, a client application can now easily parse and update a query string.</span></span>

<span data-ttu-id="98219-163">Les exemples suivants montrent comment analyser, modifier et générer des requêtes d’URI.</span><span class="sxs-lookup"><span data-stu-id="98219-163">The following examples show how to parse, modify, and generate URI queries.</span></span> <span data-ttu-id="98219-164">(Les exemples montrent une application de console par souci de simplicité.)</span><span class="sxs-lookup"><span data-stu-id="98219-164">(The examples show a console application for simplicity.)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="98219-165">Problèmes connus et les modifications avec rupture</span><span class="sxs-lookup"><span data-stu-id="98219-165">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="98219-166">Cette section décrit les problèmes connus et les modifications avec rupture dans ASP.NET Web API 2.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="98219-166">This section describes known issues and breaking changes in the ASP.NET Web API 2.1 RTM.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="98219-167">Routage d’attributs</span><span class="sxs-lookup"><span data-stu-id="98219-167">Attribute Routing</span></span>

<span data-ttu-id="98219-168">Ambiguïtés dans les correspondances de routage attribut indiquons désormais d’une erreur plutôt que de choisir la première correspondance.</span><span class="sxs-lookup"><span data-stu-id="98219-168">Ambiguities in attribute routing matches now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="98219-169">Itinéraires d’attribut sont interdit d’utiliser le *{controller}* paramètre et d’utiliser le *{action}* paramètre itinéraires placés sur les actions.</span><span class="sxs-lookup"><span data-stu-id="98219-169">Attribute routes are prohibited from using the *{controller}* parameter, and from using the *{action}* parameter on routes placed on actions.</span></span> <span data-ttu-id="98219-170">Ces paramètres sont très probablement provoquer des ambiguïtés.</span><span class="sxs-lookup"><span data-stu-id="98219-170">These parameters would very likely cause ambiguities.</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="98219-171">API Web/MVC de génération de modèles automatique dans un projet avec des résultats packages 5.1 dans les 5.0 packages pour ceux qui n’existent pas déjà dans le projet</span><span class="sxs-lookup"><span data-stu-id="98219-171">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="98219-172">Mise à jour les packages NuGet pour RTM d’ASP.NET Web API 2.1 ne met pas à jour les outils de Visual Studio, tels que de génération de modèles automatique ASP.NET ou le modèle de projet d’Application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="98219-172">Updating NuGet packages for ASP.NET Web API 2.1 RTM does not update the Visual Studio tools, such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="98219-173">Ils utilisent la version précédente des packages de runtime ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="98219-173">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="98219-174">Par conséquent, la génération de modèles automatique ASP.NET installera la version précédente (5.0.0.0) des packages requis, si elles ne sont pas déjà disponibles dans vos projets.</span><span class="sxs-lookup"><span data-stu-id="98219-174">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="98219-175">Toutefois, la génération de modèles automatique ASP.NET dans Visual Studio 2013 RTM ou une mise à jour 1 ne remplace pas les packages plus récentes dans vos projets.</span><span class="sxs-lookup"><span data-stu-id="98219-175">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span>

<span data-ttu-id="98219-176">Si vous utilisez la génération de modèles automatique ASP.NET après la mise à jour les packages Web API 2.1 ou ASP.NET MVC 5.1, vérifiez que les versions des API Web MVC sont cohérentes.</span><span class="sxs-lookup"><span data-stu-id="98219-176">If you use ASP.NET scaffolding after updating the packages to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and MVC are consistent.</span></span>

### <a name="type-renames"></a><span data-ttu-id="98219-177">Changements de noms de type</span><span class="sxs-lookup"><span data-stu-id="98219-177">Type Renames</span></span>

<span data-ttu-id="98219-178">Parmi les types utilisés pour l’extensibilité de routage attribut ayant été renommés à partir de la version RC à la version RTM 2.1.</span><span class="sxs-lookup"><span data-stu-id="98219-178">Some of the types used for attribute routing extensibility were renamed from the RC to the 2.1 RTM.</span></span>

| <span data-ttu-id="98219-179">Ancien nom de type (2.1 RC)</span><span class="sxs-lookup"><span data-stu-id="98219-179">Old type name (2.1 RC)</span></span> | <span data-ttu-id="98219-180">Nouveau Type de nom (RTM 2.1)</span><span class="sxs-lookup"><span data-stu-id="98219-180">New Type Name (2.1 RTM)</span></span> |
| --- | --- |
| <span data-ttu-id="98219-181">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="98219-181">IDirectRouteProvider</span></span> | <span data-ttu-id="98219-182">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="98219-182">IDirectRouteFactory</span></span> |
| <span data-ttu-id="98219-183">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="98219-183">RouteProviderAttribute</span></span> | <span data-ttu-id="98219-184">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="98219-184">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="98219-185">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="98219-185">DirectRouteProviderContext</span></span> | <span data-ttu-id="98219-186">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="98219-186">DirectRouteFactoryContext</span></span> |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a><span data-ttu-id="98219-187">Filtres d’exception unwrap pas agréger des exceptions levées dans les actions asynchrones</span><span class="sxs-lookup"><span data-stu-id="98219-187">Exception filters do not unwrap aggregate exceptions thrown in async actions</span></span>

<span data-ttu-id="98219-188">Auparavant, si une action asynchrone a levé une **AggregateException**, un filtre d’exception serait désencapsuler l’exception, et **OnException** obtiendriez l’exception de base.</span><span class="sxs-lookup"><span data-stu-id="98219-188">Previously, if an async action threw an **AggregateException**, an exception filter would unwrap the exception, and **OnException** would get the base exception.</span></span> <span data-ttu-id="98219-189">2.1, le filtre d’exception ne pas unwrap, et **OnException** Obtient la version d’origine **AggregateException**.</span><span class="sxs-lookup"><span data-stu-id="98219-189">In 2.1, the exception filter does not unwrap it, and **OnException** gets the original **AggregateException**.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="98219-190">Correctifs de bogues</span><span class="sxs-lookup"><span data-stu-id="98219-190">Bug Fixes</span></span>

<span data-ttu-id="98219-191">Cette version inclut également plusieurs résolutions de bogue.</span><span class="sxs-lookup"><span data-stu-id="98219-191">This release also includes several bug fixes.</span></span> <span data-ttu-id="98219-192">Vous trouverez la liste complète ici :</span><span class="sxs-lookup"><span data-stu-id="98219-192">You can find the complete list here:</span></span>

- [<span data-ttu-id="98219-193">5.1.0 package de</span><span class="sxs-lookup"><span data-stu-id="98219-193">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="98219-194">5.1.1 package de</span><span class="sxs-lookup"><span data-stu-id="98219-194">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<span data-ttu-id="98219-195">Le 5.1.2 package contient des mises à jour IntelliSense, mais aucune des correctifs de bogues.</span><span class="sxs-lookup"><span data-stu-id="98219-195">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>