---
title: Intergiciel (middleware) ASP.NET Core
author: rick-anderson
description: "En savoir plus sur ASP.NET Core intergiciel (middleware) et le pipeline de requête."
ms.author: riande
manager: wpickett
ms.date: 01/22/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: ef130e736e2f32fa134156d979ce5bfbedcae828
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2018
---
# <a name="aspnet-core-middleware-fundamentals"></a><span data-ttu-id="d9512-103">Notions de base ASP.NET Core intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="d9512-103">ASP.NET Core Middleware Fundamentals</span></span>

<a name="fundamentals-middleware"></a>

<span data-ttu-id="d9512-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="d9512-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d9512-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d9512-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="d9512-106">Nouveautés d’intergiciel (middleware) ?</span><span class="sxs-lookup"><span data-stu-id="d9512-106">What is middleware?</span></span>

<span data-ttu-id="d9512-107">Intergiciel (middleware) est un logiciel qui est intégré à un pipeline d’application pour gérer les demandes et réponses.</span><span class="sxs-lookup"><span data-stu-id="d9512-107">Middleware is software that is assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="d9512-108">Chaque composant :</span><span class="sxs-lookup"><span data-stu-id="d9512-108">Each component:</span></span>

* <span data-ttu-id="d9512-109">Peut décider de transmettre la demande au composant suivant dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="d9512-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="d9512-110">Peut effectuer un travail avant et après que le composant suivant dans le pipeline est appelé.</span><span class="sxs-lookup"><span data-stu-id="d9512-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="d9512-111">Les délégués de demande sont utilisés pour construire le pipeline de requête.</span><span class="sxs-lookup"><span data-stu-id="d9512-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="d9512-112">Les délégués de la demande gérer chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="d9512-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="d9512-113">Demander les délégués sont configurés à l’aide de [exécuter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [carte](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), et [utilisation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) les méthodes d’extension.</span><span class="sxs-lookup"><span data-stu-id="d9512-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="d9512-114">Un délégué de demande individuelle peut être spécifié en ligne en tant qu’une méthode anonyme (appelée intergiciel (middleware) en ligne), ou elle peut être définie dans une classe réutilisable.</span><span class="sxs-lookup"><span data-stu-id="d9512-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="d9512-115">Ces classes réutilisables et des méthodes anonymes d’en ligne sont *intergiciel (middleware)*, ou *composants d’intergiciel (middleware)*.</span><span class="sxs-lookup"><span data-stu-id="d9512-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="d9512-116">Chaque composant d’intergiciel (middleware) dans le pipeline de demande est responsable de l’appel de composant suivant dans le pipeline ou la chaîne de court-circuit si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="d9512-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="d9512-117">[Migration de Modules HTTP à un intergiciel (middleware)](../migration/http-modules.md) explique la différence entre la demande des pipelines dans ASP.NET Core et les versions précédentes et fournit d’autres exemples d’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="d9512-117">[Migrating HTTP Modules to Middleware](../migration/http-modules.md) explains the difference between request pipelines in ASP.NET Core and the previous versions and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="d9512-118">Création d’un pipeline de l’intergiciel (middleware) avec IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="d9512-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="d9512-119">Le pipeline de demande ASP.NET Core se compose d’une séquence de délégués de la demande, appelé une après l’autre, comme le montre ce diagramme (le thread d’exécution suit les flèches noires) :</span><span class="sxs-lookup"><span data-stu-id="d9512-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Modèle de traitement de requête montrant une demande qui arrivent, via trois middlewares et la réponse en laissant l’application de traitement.](middleware/_static/request-delegate-pipeline.png)

<span data-ttu-id="d9512-123">Chaque délégué peut effectuer des opérations avant et après le délégué suivant.</span><span class="sxs-lookup"><span data-stu-id="d9512-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="d9512-124">Un délégué peut également décider de ne pas transmettre une demande pour le délégué suivant, qui est appelé le pipeline de demande de court-circuit.</span><span class="sxs-lookup"><span data-stu-id="d9512-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="d9512-125">Court-circuit est souvent souhaitable car elle évite le travail inutile.</span><span class="sxs-lookup"><span data-stu-id="d9512-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="d9512-126">Par exemple, l’intergiciel (middleware) de fichiers statiques peut retourner une demande pour un fichier statique et le reste du pipeline de court-circuit.</span><span class="sxs-lookup"><span data-stu-id="d9512-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="d9512-127">Gestion des exceptions délégués doivent être appelés tôt dans le pipeline, afin qu’ils peuvent intercepter les exceptions qui se produisent dans les phases ultérieures du pipeline.</span><span class="sxs-lookup"><span data-stu-id="d9512-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="d9512-128">L’application de ASP.NET Core la plus simple possible définit un délégué de requête unique qui gère toutes les demandes.</span><span class="sxs-lookup"><span data-stu-id="d9512-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="d9512-129">Ce cas n’inclut pas un pipeline de demande réelle.</span><span class="sxs-lookup"><span data-stu-id="d9512-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="d9512-130">Au lieu de cela, une fonction anonyme unique est appelée en réponse à chaque requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="d9512-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

<span data-ttu-id="d9512-131">Le premier [application. Exécutez](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) délégué termine le pipeline.</span><span class="sxs-lookup"><span data-stu-id="d9512-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="d9512-132">Vous pouvez chaîner plusieurs délégués demande avec [application. Utilisez](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="d9512-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="d9512-133">Le `next` paramètre représente le délégué suivant dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="d9512-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="d9512-134">(Souvenez-vous que vous pouvez court-circuit le pipeline par *pas* appelant le *suivant* paramètre.) Vous pouvez généralement effectuer actions avant et après le délégué suivant, comme illustré dans cet exemple :</span><span class="sxs-lookup"><span data-stu-id="d9512-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="d9512-135">N’appelez pas `next.Invoke` une fois que la réponse a été envoyée au client.</span><span class="sxs-lookup"><span data-stu-id="d9512-135">Do not call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="d9512-136">Modifications apportées à `HttpResponse` après le démarrage de la réponse lève une exception.</span><span class="sxs-lookup"><span data-stu-id="d9512-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="d9512-137">Par exemple, les modifications telles que la définition des en-têtes, code d’état, etc., lève une exception.</span><span class="sxs-lookup"><span data-stu-id="d9512-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="d9512-138">Écrire dans le corps de réponse après avoir appelé `next`:</span><span class="sxs-lookup"><span data-stu-id="d9512-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="d9512-139">Peut entraîner une violation du protocole.</span><span class="sxs-lookup"><span data-stu-id="d9512-139">May cause a protocol violation.</span></span> <span data-ttu-id="d9512-140">Par exemple, l’écriture de plus de la manière indiquée `content-length`.</span><span class="sxs-lookup"><span data-stu-id="d9512-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="d9512-141">Peut altérer le format du corps.</span><span class="sxs-lookup"><span data-stu-id="d9512-141">May corrupt the body format.</span></span> <span data-ttu-id="d9512-142">Par exemple, l’écriture d’un pied de page HTML dans un fichier CSS.</span><span class="sxs-lookup"><span data-stu-id="d9512-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="d9512-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) est un indicateur utile pour indiquer si les en-têtes ont été envoyés et/ou le corps a été écrit.</span><span class="sxs-lookup"><span data-stu-id="d9512-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="d9512-144">Classement</span><span class="sxs-lookup"><span data-stu-id="d9512-144">Ordering</span></span>

<span data-ttu-id="d9512-145">Composants d’intergiciel (middleware) sont ajoutés dans l’ordre de la `Configure` méthode définit l’ordre dans lequel ils sont appelés sur les demandes et l’ordre inverse pour la réponse.</span><span class="sxs-lookup"><span data-stu-id="d9512-145">The order that middleware components are added in the `Configure` method defines the order in which they are invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="d9512-146">Ce classement est critique de sécurité, performances et fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="d9512-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="d9512-147">La méthode de configuration (voir ci-dessous) ajoute les composants d’intergiciel (middleware) suivant :</span><span class="sxs-lookup"><span data-stu-id="d9512-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="d9512-148">Gestion d’erreur d’exception</span><span class="sxs-lookup"><span data-stu-id="d9512-148">Exception/error handling</span></span>
2. <span data-ttu-id="d9512-149">Serveur de fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="d9512-149">Static file server</span></span>
3. <span data-ttu-id="d9512-150">Authentification</span><span class="sxs-lookup"><span data-stu-id="d9512-150">Authentication</span></span>
4. <span data-ttu-id="d9512-151">MVC</span><span class="sxs-lookup"><span data-stu-id="d9512-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d9512-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d9512-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d9512-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d9512-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

<span data-ttu-id="d9512-154">Dans le code ci-dessus, `UseExceptionHandler` est le premier composant d’intergiciel (middleware) ajouté au pipeline, par conséquent, elle intercepte toutes les exceptions qui se produisent dans les appels ultérieurs.</span><span class="sxs-lookup"><span data-stu-id="d9512-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="d9512-155">L’intergiciel (middleware) fichier statique est appelée très tôt dans le pipeline afin de gérer les demandes et de court-circuit sans passer par les autres composants.</span><span class="sxs-lookup"><span data-stu-id="d9512-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="d9512-156">Fournit de l’intergiciel (middleware) de fichiers statiques **aucune** vérifications d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="d9512-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="d9512-157">Tous les fichiers pris en charge par, notamment ceux de *wwwroot*, sont accessibles.</span><span class="sxs-lookup"><span data-stu-id="d9512-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="d9512-158">Consultez [utilisation des fichiers statiques](xref:fundamentals/static-files) pour une approche de sécuriser les fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="d9512-158">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d9512-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d9512-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="d9512-160">Si la demande n’est pas gérée par l’intergiciel (middleware) fichier statique, il est transmis à l’intergiciel (middleware) identité (`app.UseAuthentication`), qui effectue l’authentification.</span><span class="sxs-lookup"><span data-stu-id="d9512-160">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="d9512-161">Identité n’effectue pas de court-circuit les demandes non authentifiées.</span><span class="sxs-lookup"><span data-stu-id="d9512-161">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="d9512-162">Bien que l’identité authentifie les requêtes, d’autorisation (et rejet) se produit uniquement après que MVC sélectionne une Page Razor ou un contrôleur et une action spécifique.</span><span class="sxs-lookup"><span data-stu-id="d9512-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d9512-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d9512-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d9512-164">Si la demande n’est pas gérée par l’intergiciel (middleware) fichier statique, il est transmis à l’intergiciel (middleware) identité (`app.UseIdentity`), qui effectue l’authentification.</span><span class="sxs-lookup"><span data-stu-id="d9512-164">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="d9512-165">Identité n’effectue pas de court-circuit les demandes non authentifiées.</span><span class="sxs-lookup"><span data-stu-id="d9512-165">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="d9512-166">Bien que l’identité authentifie les requêtes, d’autorisation (et rejet) se produit uniquement après que MVC sélectionne un contrôleur spécifique et une action.</span><span class="sxs-lookup"><span data-stu-id="d9512-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="d9512-167">L’exemple suivant montre un intergiciel (middleware) classement où les demandes de fichiers statiques sont gérées par l’intergiciel (middleware) de fichiers statiques avant l’intergiciel de compression de la réponse.</span><span class="sxs-lookup"><span data-stu-id="d9512-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="d9512-168">Fichiers statiques ne sont pas compressés avec ce classement de l’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="d9512-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="d9512-169">Les réponses MVC de [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) peuvent être compressées.</span><span class="sxs-lookup"><span data-stu-id="d9512-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a><span data-ttu-id="d9512-170">Utilisez, exécuter et carte</span><span class="sxs-lookup"><span data-stu-id="d9512-170">Use, Run, and Map</span></span>

<span data-ttu-id="d9512-171">Vous configurez le pipeline HTTP à l’aide `Use`, `Run`, et `Map`.</span><span class="sxs-lookup"><span data-stu-id="d9512-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="d9512-172">Le `Use` méthode pouvez court-circuit le pipeline (autrement dit, si elle n’appelle pas une `next` délégué de la demande).</span><span class="sxs-lookup"><span data-stu-id="d9512-172">The `Use` method can short-circuit the pipeline (that is, if it does not call a `next` request delegate).</span></span> <span data-ttu-id="d9512-173">`Run`est une convention, et certains composants d’intergiciel (middleware) peuvent exposer `Run[Middleware]` méthodes qui s’exécutent à la fin du pipeline.</span><span class="sxs-lookup"><span data-stu-id="d9512-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="d9512-174">`Map*`les extensions sont utilisées comme une convention pour créer une branche le pipeline.</span><span class="sxs-lookup"><span data-stu-id="d9512-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="d9512-175">[Carte](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches le pipeline de requête en fonction des correspondances de chemin de la demande donnée.</span><span class="sxs-lookup"><span data-stu-id="d9512-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="d9512-176">Si le chemin d’accès de requête commence par le chemin d’accès donné, la branche est exécutée.</span><span class="sxs-lookup"><span data-stu-id="d9512-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="d9512-177">Le tableau suivant présente les demandes et réponses de `http://localhost:1234` en utilisant le code précédent :</span><span class="sxs-lookup"><span data-stu-id="d9512-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="d9512-178">Demande</span><span class="sxs-lookup"><span data-stu-id="d9512-178">Request</span></span> | <span data-ttu-id="d9512-179">Réponse</span><span class="sxs-lookup"><span data-stu-id="d9512-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="d9512-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="d9512-180">localhost:1234</span></span> | <span data-ttu-id="d9512-181">Bonjour à partir de la table non délégué.</span><span class="sxs-lookup"><span data-stu-id="d9512-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="d9512-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="d9512-182">localhost:1234/map1</span></span> | <span data-ttu-id="d9512-183">Test de mappage 1</span><span class="sxs-lookup"><span data-stu-id="d9512-183">Map Test 1</span></span> |
| <span data-ttu-id="d9512-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="d9512-184">localhost:1234/map2</span></span> | <span data-ttu-id="d9512-185">Test de mappage 2</span><span class="sxs-lookup"><span data-stu-id="d9512-185">Map Test 2</span></span> |
| <span data-ttu-id="d9512-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="d9512-186">localhost:1234/map3</span></span> | <span data-ttu-id="d9512-187">Bonjour à partir de la table non délégué.</span><span class="sxs-lookup"><span data-stu-id="d9512-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="d9512-188">Lorsque `Map` est utilisé, les segments de chemin d’accès de mise en correspondance sont supprimés de `HttpRequest.Path` et ajouté à `HttpRequest.PathBase` pour chaque demande.</span><span class="sxs-lookup"><span data-stu-id="d9512-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="d9512-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches le pipeline de requête en fonction du résultat du prédicat donné.</span><span class="sxs-lookup"><span data-stu-id="d9512-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="d9512-190">Un prédicat de type `Func<HttpContext, bool>` peut être utilisé pour mapper les requêtes avec une nouvelle branche du pipeline.</span><span class="sxs-lookup"><span data-stu-id="d9512-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="d9512-191">Dans l’exemple suivant, un prédicat est utilisé pour détecter la présence d’une variable de chaîne de requête `branch`:</span><span class="sxs-lookup"><span data-stu-id="d9512-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="d9512-192">Le tableau suivant présente les demandes et réponses de `http://localhost:1234` en utilisant le code précédent :</span><span class="sxs-lookup"><span data-stu-id="d9512-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="d9512-193">Demande</span><span class="sxs-lookup"><span data-stu-id="d9512-193">Request</span></span> | <span data-ttu-id="d9512-194">Réponse</span><span class="sxs-lookup"><span data-stu-id="d9512-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="d9512-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="d9512-195">localhost:1234</span></span> | <span data-ttu-id="d9512-196">Bonjour à partir de la table non délégué.</span><span class="sxs-lookup"><span data-stu-id="d9512-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="d9512-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="d9512-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="d9512-198">Branche utilisé = principale</span><span class="sxs-lookup"><span data-stu-id="d9512-198">Branch used = master</span></span>|

<span data-ttu-id="d9512-199">`Map`prend en charge l’imbrication, par exemple :</span><span class="sxs-lookup"><span data-stu-id="d9512-199">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

<span data-ttu-id="d9512-200">`Map`peut également correspondre à plusieurs segments à la fois, par exemple :</span><span class="sxs-lookup"><span data-stu-id="d9512-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="d9512-201">Intergiciel (middleware) intégré</span><span class="sxs-lookup"><span data-stu-id="d9512-201">Built-in middleware</span></span>

<span data-ttu-id="d9512-202">ASP.NET Core est fourni avec les composants d’intergiciel (middleware) suivant, ainsi qu’une description de l’ordre dans lequel ils doivent être ajoutés :</span><span class="sxs-lookup"><span data-stu-id="d9512-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="d9512-203">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="d9512-203">Middleware</span></span> | <span data-ttu-id="d9512-204">Description</span><span class="sxs-lookup"><span data-stu-id="d9512-204">Description</span></span> | <span data-ttu-id="d9512-205">Trier</span><span class="sxs-lookup"><span data-stu-id="d9512-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="d9512-206">Authentification</span><span class="sxs-lookup"><span data-stu-id="d9512-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="d9512-207">Fournit la prise en charge de l’authentification.</span><span class="sxs-lookup"><span data-stu-id="d9512-207">Provides authentication support.</span></span> | <span data-ttu-id="d9512-208">Avant de `HttpContext.User` est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="d9512-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="d9512-209">Terminal Server pour les rappels d’OAuth.</span><span class="sxs-lookup"><span data-stu-id="d9512-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="d9512-210">CORS</span><span class="sxs-lookup"><span data-stu-id="d9512-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="d9512-211">Configure le partage de ressources Cross-Origin.</span><span class="sxs-lookup"><span data-stu-id="d9512-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="d9512-212">Avant les composants qui utilisent les CORS.</span><span class="sxs-lookup"><span data-stu-id="d9512-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="d9512-213">Diagnostics</span><span class="sxs-lookup"><span data-stu-id="d9512-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="d9512-214">Configure les diagnostics.</span><span class="sxs-lookup"><span data-stu-id="d9512-214">Configures diagnostics.</span></span> | <span data-ttu-id="d9512-215">Avant les composants qui génèrent des erreurs.</span><span class="sxs-lookup"><span data-stu-id="d9512-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="d9512-216">ForwardedHeaders/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="d9512-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="d9512-217">Transfère les en-têtes traitées sur la requête actuelle.</span><span class="sxs-lookup"><span data-stu-id="d9512-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="d9512-218">Avant des composants qui consomment les champs mis à jour (exemples : schéma, hôte, Ipclient, méthode).</span><span class="sxs-lookup"><span data-stu-id="d9512-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="d9512-219">Mise en cache des réponses</span><span class="sxs-lookup"><span data-stu-id="d9512-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="d9512-220">Prend en charge la mise en cache des réponses.</span><span class="sxs-lookup"><span data-stu-id="d9512-220">Provides support for caching responses.</span></span> | <span data-ttu-id="d9512-221">Avant les composants qui nécessitent la mise en cache.</span><span class="sxs-lookup"><span data-stu-id="d9512-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="d9512-222">Compression de la réponse</span><span class="sxs-lookup"><span data-stu-id="d9512-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="d9512-223">Prend en charge la compression des réponses.</span><span class="sxs-lookup"><span data-stu-id="d9512-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="d9512-224">Avant les composants qui requièrent la compression.</span><span class="sxs-lookup"><span data-stu-id="d9512-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="d9512-225">RequestLocalization</span><span class="sxs-lookup"><span data-stu-id="d9512-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="d9512-226">Prend en charge la localisation.</span><span class="sxs-lookup"><span data-stu-id="d9512-226">Provides localization support.</span></span> | <span data-ttu-id="d9512-227">Avant de composants sensibles de la localisation.</span><span class="sxs-lookup"><span data-stu-id="d9512-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="d9512-228">Routage</span><span class="sxs-lookup"><span data-stu-id="d9512-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="d9512-229">Définit et contraint les itinéraires de la demande.</span><span class="sxs-lookup"><span data-stu-id="d9512-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="d9512-230">Terminal Server pour les itinéraires correspondants.</span><span class="sxs-lookup"><span data-stu-id="d9512-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="d9512-231">Session</span><span class="sxs-lookup"><span data-stu-id="d9512-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="d9512-232">Prend en charge la gestion des sessions utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d9512-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="d9512-233">Avant les composants qui nécessitent la Session.</span><span class="sxs-lookup"><span data-stu-id="d9512-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="d9512-234">Fichiers statiques</span><span class="sxs-lookup"><span data-stu-id="d9512-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="d9512-235">Fournit la prise en charge pour traiter les fichiers statiques et l’exploration des répertoires.</span><span class="sxs-lookup"><span data-stu-id="d9512-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="d9512-236">Terminal Server, si une demande correspond aux fichiers.</span><span class="sxs-lookup"><span data-stu-id="d9512-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="d9512-237">Réécriture d’URL</span><span class="sxs-lookup"><span data-stu-id="d9512-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="d9512-238">Prend en charge la réécriture d’URL et la redirection des demandes.</span><span class="sxs-lookup"><span data-stu-id="d9512-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="d9512-239">Avant des composants qui consomment l’URL.</span><span class="sxs-lookup"><span data-stu-id="d9512-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="d9512-240">WebSockets</span><span class="sxs-lookup"><span data-stu-id="d9512-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="d9512-241">Active le protocole WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d9512-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="d9512-242">Avant des composants qui sont requis pour accepter les demandes WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d9512-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="d9512-243">Intergiciel (middleware) de l’écriture</span><span class="sxs-lookup"><span data-stu-id="d9512-243">Writing middleware</span></span>

<span data-ttu-id="d9512-244">Intergiciel (middleware) est généralement encapsulé dans une classe et exposée avec une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="d9512-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="d9512-245">Prenez en compte l’intergiciel (middleware) suivant, qui définit la culture pour la requête actuelle à partir de la chaîne de requête :</span><span class="sxs-lookup"><span data-stu-id="d9512-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="d9512-246">Remarque : L’exemple de code ci-dessus est utilisé pour illustrer la création d’un composant d’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="d9512-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="d9512-247">Consultez [ globalisation et localisation](xref:fundamentals/localization) pour la prise en charge de la localisation intégré d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d9512-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="d9512-248">Vous pouvez tester l’intergiciel (middleware) en passant dans la culture, par exemple `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="d9512-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="d9512-249">Le code suivant déplace le délégué de l’intergiciel (middleware) à une classe :</span><span class="sxs-lookup"><span data-stu-id="d9512-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="d9512-250">La méthode d’extension suivant expose l’intergiciel (middleware) via [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="d9512-250">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="d9512-251">Le code suivant appelle l’intergiciel (middleware) à partir de `Configure`:</span><span class="sxs-lookup"><span data-stu-id="d9512-251">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="d9512-252">Intergiciel (middleware) doit suivre le [principe de dépendances explicites](http://deviq.com/explicit-dependencies-principle/) en exposant ses dépendances dans son constructeur.</span><span class="sxs-lookup"><span data-stu-id="d9512-252">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="d9512-253">Intergiciel (middleware) est construit en une seule fois par *durée de vie application*.</span><span class="sxs-lookup"><span data-stu-id="d9512-253">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="d9512-254">Consultez *dépendances par demande* ci-dessous si vous avez besoin de partager des services avec un intergiciel (middleware) au sein d’une requête.</span><span class="sxs-lookup"><span data-stu-id="d9512-254">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="d9512-255">Composants d’intergiciel (middleware) peuvent résoudre leurs dépendances à partir de l’injection de dépendances via les paramètres du constructeur.</span><span class="sxs-lookup"><span data-stu-id="d9512-255">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="d9512-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)peut également accepter des paramètres supplémentaires directement.</span><span class="sxs-lookup"><span data-stu-id="d9512-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="d9512-257">Dépendances par demande</span><span class="sxs-lookup"><span data-stu-id="d9512-257">Per-request dependencies</span></span>

<span data-ttu-id="d9512-258">Étant donné que l’intergiciel (middleware) est créée au démarrage de l’application, pas à la demande, *étendue* les services de durée de vie utilisés par les constructeurs de l’intergiciel (middleware) ne sont pas partagés avec d’autres types d’injection de dépendance lors de chaque demande.</span><span class="sxs-lookup"><span data-stu-id="d9512-258">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="d9512-259">Si vous devez partager une *étendue* entre votre intergiciel (middleware) et d’autres types de service, ajoutez ces services pour le `Invoke` signature de la méthode.</span><span class="sxs-lookup"><span data-stu-id="d9512-259">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="d9512-260">Le `Invoke` méthode peut accepter des paramètres supplémentaires sont remplies par injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="d9512-260">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="d9512-261">Exemple :</span><span class="sxs-lookup"><span data-stu-id="d9512-261">For example:</span></span>

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a><span data-ttu-id="d9512-262">Ressources</span><span class="sxs-lookup"><span data-stu-id="d9512-262">Resources</span></span>

* [<span data-ttu-id="d9512-263">Exemple de code utilisé dans ce document.</span><span class="sxs-lookup"><span data-stu-id="d9512-263">Sample code used in this doc</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [<span data-ttu-id="d9512-264">Migration des modules HTTP vers les intergiciels (middleware)</span><span class="sxs-lookup"><span data-stu-id="d9512-264">Migrating HTTP Modules to Middleware</span></span>](../migration/http-modules.md)
* [<span data-ttu-id="d9512-265">Démarrage d’une application</span><span class="sxs-lookup"><span data-stu-id="d9512-265">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="d9512-266">Fonctionnalités de requête</span><span class="sxs-lookup"><span data-stu-id="d9512-266">Request Features</span></span>](request-features.md)
