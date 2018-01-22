---
title: OWIN (Open Web Interface pour .NET)
author: ardalis
description: "Découvrez comment ASP.NET Core prend en charge l’Interface Web ouverte pour .NET (OWIN), ce qui permet aux applications web à être dissocié de serveurs web."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e819037e2ebd1566c778879516e20de8dc7603ea
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="7d7b3-103">Introduction à ouvrir l’Interface Web pour .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-103">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="7d7b3-104">Par [Steve Smith](https://ardalis.com/) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-104">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7d7b3-105">ASP.NET Core prend en charge l’interface Open Web pour .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="7d7b3-105">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="7d7b3-106">OWIN permet que les applications web soient dissociées des serveurs web.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-106">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="7d7b3-107">Il définit une manière standard pour un intergiciel (middleware) à utiliser dans un pipeline pour gérer les demandes et réponses associées.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-107">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="7d7b3-108">Intergiciel (middleware) et les applications ASP.NET Core peuvent interagir avec intergiciel (middleware), les serveurs et les applications basées sur OWIN.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-108">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="7d7b3-109">OWIN fournit une couche de découplage qui permet à deux infrastructures avec les modèles d’objets disparates à être utilisés ensemble.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-109">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="7d7b3-110">Le `Microsoft.AspNetCore.Owin` package fournit deux implémentations d’adaptateur :</span><span class="sxs-lookup"><span data-stu-id="7d7b3-110">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="7d7b3-111">ASP.NET Core pour OWIN</span><span class="sxs-lookup"><span data-stu-id="7d7b3-111">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="7d7b3-112">OWIN à ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7d7b3-112">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="7d7b3-113">Cela permet à ASP.NET Core être hébergée sur un serveur/hôte compatible OWIN, ou pour d’autres composants compatibles OWIN à exécuter sur ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-113">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="7d7b3-114">Remarque : À l’aide de ces adaptateurs assortie d’une baisse des performances.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-114">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="7d7b3-115">Les applications utilisant uniquement des composants ASP.NET Core ne devraient pas utiliser l’ou les adaptateurs Owin.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-115">Applications using only ASP.NET Core components should not use the Owin package or adapters.</span></span>

<span data-ttu-id="7d7b3-116">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7d7b3-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="7d7b3-117">Intergiciel (middleware) OWIN en cours d’exécution dans le pipeline ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7d7b3-117">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="7d7b3-118">Prise en charge OWIN de ASP.NET Core est déployé dans le cadre de la `Microsoft.AspNetCore.Owin` package.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-118">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="7d7b3-119">Vous pouvez importer la prise en charge OWIN dans votre projet en installant ce package.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-119">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="7d7b3-120">Intergiciel (middleware) OWIN est conforme à la [spécification de OWIN](http://owin.org/spec/spec/owin-1.0.0.html), ce qui nécessite un `Func<IDictionary<string, object>, Task>` interface et clés spécifiques avec la valeur (tel que `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="7d7b3-120">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="7d7b3-121">L’intergiciel OWIN de simple suivante affiche « Hello World » :</span><span class="sxs-lookup"><span data-stu-id="7d7b3-121">The following simple OWIN middleware displays "Hello World":</span></span>

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

<span data-ttu-id="7d7b3-122">La signature de l’exemple retourne un `Task` et accepte un `IDictionary<string, object>` comme exigé par le OWIN.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-122">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="7d7b3-123">Le code suivant montre comment ajouter la `OwinHello` intergiciel (middleware) (ci-dessus) pour le pipeline ASP.NET avec la `UseOwin` méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-123">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="7d7b3-124">Vous pouvez configurer d’autres actions pour avoir lieu dans le pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-124">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="7d7b3-125">En-têtes de réponse doivent uniquement être modifiées avant la première écriture dans le flux de réponse.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-125">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="7d7b3-126">Appels multiples à `UseOwin` est déconseillée pour des raisons de performances.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-126">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="7d7b3-127">Composants OWIN fonctionnera mieux si regroupés.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-127">OWIN components will operate best if grouped together.</span></span>

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="7d7b3-128">À l’aide d’hébergement ASP.NET sur un serveur OWIN</span><span class="sxs-lookup"><span data-stu-id="7d7b3-128">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="7d7b3-129">Les serveurs OWIN peuvent héberger des applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-129">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="7d7b3-130">Un tel serveur est [Nowin](https://github.com/Bobris/Nowin), un serveur web de .NET OWIN.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-130">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="7d7b3-131">Dans l’exemple de cet article, j’ai inclus un projet qui fait référence à Nowin et l’utilise pour créer un `IServer` capable d’auto-hébergement ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-131">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="7d7b3-132">`IServer`est une interface qui nécessite une `Features` propriété et un `Start` (méthode).</span><span class="sxs-lookup"><span data-stu-id="7d7b3-132">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="7d7b3-133">`Start`est chargé de configurer et démarrer le serveur, qui dans ce cas est effectué via une série d’appels d’API fluent que définir des adresses analysés à partir de la IServerAddressesFeature.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-133">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="7d7b3-134">Notez que la configuration fluent de la `_builder` variable Spécifie que les demandes sont traitées par le `appFunc` définis précédemment dans la méthode.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-134">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="7d7b3-135">Cela `Func` est appelée sur chaque demande de traiter les demandes entrantes.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-135">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="7d7b3-136">Nous allons également ajouter un `IWebHostBuilder` extension pour faciliter la tâche Ajouter et configurer le serveur Nowin.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-136">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

<span data-ttu-id="7d7b3-137">Tout cela en place, tout ce qui est requis pour exécuter une application ASP.NET à l’aide de ce serveur personnalisé pour appeler l’extension de *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="7d7b3-137">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}

```

<span data-ttu-id="7d7b3-138">En savoir plus sur ASP.NET [serveurs](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="7d7b3-138">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="7d7b3-139">Exécutez ASP.NET Core sur un serveur OWIN et utiliser sa prise en charge de WebSocket</span><span class="sxs-lookup"><span data-stu-id="7d7b3-139">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="7d7b3-140">Un autre exemple de fonctionnalités de serveurs basée sur les OWIN peut être exploité par ASP.NET Core est aux fonctionnalités telles que les WebSockets.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-140">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="7d7b3-141">Le serveur web .NET OWIN utilisé dans l’exemple précédent a prise en charge de WebSocket intégrées, qui peuvent être exploitées par une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-141">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="7d7b3-142">L’exemple ci-dessous montre une application web simple qui prend en charge de WebSocket et renvoie tous les éléments envoyés au serveur via le protocole WebSocket.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-142">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

<span data-ttu-id="7d7b3-143">Cela [exemple](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) est configuré à l’aide de la même `NowinServer` que la précédente - la seule différence réside dans la configuration de l’application dans son `Configure` (méthode).</span><span class="sxs-lookup"><span data-stu-id="7d7b3-143">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="7d7b3-144">Un test à l’aide de [un client websocket simple](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) montre l’application :</span><span class="sxs-lookup"><span data-stu-id="7d7b3-144">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Client de Test Web Socket](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="7d7b3-146">Environnement OWIN</span><span class="sxs-lookup"><span data-stu-id="7d7b3-146">OWIN environment</span></span>

<span data-ttu-id="7d7b3-147">Vous pouvez construire un environnement OWIN à l’aide du `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-147">You can construct a OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="7d7b3-148">Clés OWIN</span><span class="sxs-lookup"><span data-stu-id="7d7b3-148">OWIN keys</span></span>

<span data-ttu-id="7d7b3-149">Dépend de OWIN un `IDictionary<string,object>` objet pour communiquer des informations tout au long d’un échange de demande/réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-149">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="7d7b3-150">ASP.NET Core implémente les clés répertoriées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="7d7b3-150">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="7d7b3-151">Consultez le [principales spécifications, les extensions](http://owin.org/#spec), et [OWIN clé instructions et des clés communes](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="7d7b3-151">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="7d7b3-152">Données de la demande (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-152">Request Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="7d7b3-153">Touche</span><span class="sxs-lookup"><span data-stu-id="7d7b3-153">Key</span></span>               | <span data-ttu-id="7d7b3-154">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-154">Value (type)</span></span> | <span data-ttu-id="7d7b3-155">Description</span><span class="sxs-lookup"><span data-stu-id="7d7b3-155">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="7d7b3-156">owin.RequestScheme</span><span class="sxs-lookup"><span data-stu-id="7d7b3-156">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="7d7b3-157">owin.RequestMethod</span><span class="sxs-lookup"><span data-stu-id="7d7b3-157">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="7d7b3-158">owin.RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="7d7b3-158">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="7d7b3-159">owin.RequestPath</span><span class="sxs-lookup"><span data-stu-id="7d7b3-159">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="7d7b3-160">owin.RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="7d7b3-160">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="7d7b3-161">owin.RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="7d7b3-161">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="7d7b3-162">owin.RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="7d7b3-162">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="7d7b3-163">owin.RequestBody</span><span class="sxs-lookup"><span data-stu-id="7d7b3-163">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="7d7b3-164">Données de la demande (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-164">Request Data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="7d7b3-165">Touche</span><span class="sxs-lookup"><span data-stu-id="7d7b3-165">Key</span></span>               | <span data-ttu-id="7d7b3-166">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-166">Value (type)</span></span> | <span data-ttu-id="7d7b3-167">Description</span><span class="sxs-lookup"><span data-stu-id="7d7b3-167">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="7d7b3-168">owin.RequestId</span><span class="sxs-lookup"><span data-stu-id="7d7b3-168">owin.RequestId</span></span> | `String` | <span data-ttu-id="7d7b3-169">Facultatif</span><span class="sxs-lookup"><span data-stu-id="7d7b3-169">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="7d7b3-170">Données de réponse (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-170">Response Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="7d7b3-171">Touche</span><span class="sxs-lookup"><span data-stu-id="7d7b3-171">Key</span></span>               | <span data-ttu-id="7d7b3-172">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-172">Value (type)</span></span> | <span data-ttu-id="7d7b3-173">Description</span><span class="sxs-lookup"><span data-stu-id="7d7b3-173">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="7d7b3-174">owin.ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="7d7b3-174">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="7d7b3-175">Facultatif</span><span class="sxs-lookup"><span data-stu-id="7d7b3-175">Optional</span></span> |
| <span data-ttu-id="7d7b3-176">owin.ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="7d7b3-176">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="7d7b3-177">Facultatif</span><span class="sxs-lookup"><span data-stu-id="7d7b3-177">Optional</span></span> |
| <span data-ttu-id="7d7b3-178">owin.ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="7d7b3-178">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="7d7b3-179">owin.ResponseBody</span><span class="sxs-lookup"><span data-stu-id="7d7b3-179">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="7d7b3-180">Autres données (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-180">Other Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="7d7b3-181">Touche</span><span class="sxs-lookup"><span data-stu-id="7d7b3-181">Key</span></span>               | <span data-ttu-id="7d7b3-182">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-182">Value (type)</span></span> | <span data-ttu-id="7d7b3-183">Description</span><span class="sxs-lookup"><span data-stu-id="7d7b3-183">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="7d7b3-184">owin.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="7d7b3-184">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="7d7b3-185">owin.Version</span><span class="sxs-lookup"><span data-stu-id="7d7b3-185">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="7d7b3-186">Clés communes</span><span class="sxs-lookup"><span data-stu-id="7d7b3-186">Common Keys</span></span>

| <span data-ttu-id="7d7b3-187">Touche</span><span class="sxs-lookup"><span data-stu-id="7d7b3-187">Key</span></span>               | <span data-ttu-id="7d7b3-188">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-188">Value (type)</span></span> | <span data-ttu-id="7d7b3-189">Description</span><span class="sxs-lookup"><span data-stu-id="7d7b3-189">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="7d7b3-190">ssl.ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="7d7b3-190">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="7d7b3-191">ssl.LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="7d7b3-191">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="7d7b3-192">server.RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="7d7b3-192">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="7d7b3-193">server.RemotePort</span><span class="sxs-lookup"><span data-stu-id="7d7b3-193">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="7d7b3-194">server.LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="7d7b3-194">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="7d7b3-195">server.LocalPort</span><span class="sxs-lookup"><span data-stu-id="7d7b3-195">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="7d7b3-196">server.IsLocal</span><span class="sxs-lookup"><span data-stu-id="7d7b3-196">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="7d7b3-197">server.OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="7d7b3-197">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="7d7b3-198">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="7d7b3-198">SendFiles v0.3.0</span></span>

| <span data-ttu-id="7d7b3-199">Touche</span><span class="sxs-lookup"><span data-stu-id="7d7b3-199">Key</span></span>               | <span data-ttu-id="7d7b3-200">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-200">Value (type)</span></span> | <span data-ttu-id="7d7b3-201">Description</span><span class="sxs-lookup"><span data-stu-id="7d7b3-201">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="7d7b3-202">sendfile.SendAsync</span><span class="sxs-lookup"><span data-stu-id="7d7b3-202">sendfile.SendAsync</span></span> | <span data-ttu-id="7d7b3-203">Consultez [signature du délégué](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-203">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="7d7b3-204">Par demande</span><span class="sxs-lookup"><span data-stu-id="7d7b3-204">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="7d7b3-205">V0.3.0 opaque</span><span class="sxs-lookup"><span data-stu-id="7d7b3-205">Opaque v0.3.0</span></span>

| <span data-ttu-id="7d7b3-206">Touche</span><span class="sxs-lookup"><span data-stu-id="7d7b3-206">Key</span></span>               | <span data-ttu-id="7d7b3-207">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-207">Value (type)</span></span> | <span data-ttu-id="7d7b3-208">Description</span><span class="sxs-lookup"><span data-stu-id="7d7b3-208">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="7d7b3-209">opaque.Version</span><span class="sxs-lookup"><span data-stu-id="7d7b3-209">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="7d7b3-210">opaque.Upgrade</span><span class="sxs-lookup"><span data-stu-id="7d7b3-210">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="7d7b3-211">Consultez [signature du délégué](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-211">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="7d7b3-212">opaque.Stream</span><span class="sxs-lookup"><span data-stu-id="7d7b3-212">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="7d7b3-213">opaque.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="7d7b3-213">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="7d7b3-214">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="7d7b3-214">WebSocket v0.3.0</span></span>

| <span data-ttu-id="7d7b3-215">Touche</span><span class="sxs-lookup"><span data-stu-id="7d7b3-215">Key</span></span>               | <span data-ttu-id="7d7b3-216">Valeur (type)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-216">Value (type)</span></span> | <span data-ttu-id="7d7b3-217">Description</span><span class="sxs-lookup"><span data-stu-id="7d7b3-217">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="7d7b3-218">websocket.Version</span><span class="sxs-lookup"><span data-stu-id="7d7b3-218">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="7d7b3-219">websocket.Accept</span><span class="sxs-lookup"><span data-stu-id="7d7b3-219">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="7d7b3-220">Consultez [signature du délégué](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-220">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="7d7b3-221">websocket.AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="7d7b3-221">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="7d7b3-222">Non-spec</span><span class="sxs-lookup"><span data-stu-id="7d7b3-222">Non-spec</span></span> |
| <span data-ttu-id="7d7b3-223">websocket.SubProtocol</span><span class="sxs-lookup"><span data-stu-id="7d7b3-223">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="7d7b3-224">Consultez [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) étape 5.5</span><span class="sxs-lookup"><span data-stu-id="7d7b3-224">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="7d7b3-225">websocket.SendAsync</span><span class="sxs-lookup"><span data-stu-id="7d7b3-225">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="7d7b3-226">Consultez [signature du délégué](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-226">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="7d7b3-227">websocket.ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="7d7b3-227">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="7d7b3-228">Consultez [signature du délégué](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="7d7b3-229">websocket.CloseAsync</span><span class="sxs-lookup"><span data-stu-id="7d7b3-229">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="7d7b3-230">Consultez [signature du délégué](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="7d7b3-231">websocket.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="7d7b3-231">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="7d7b3-232">websocket.ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="7d7b3-232">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="7d7b3-233">Facultatif</span><span class="sxs-lookup"><span data-stu-id="7d7b3-233">Optional</span></span> |
| <span data-ttu-id="7d7b3-234">websocket.ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="7d7b3-234">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="7d7b3-235">Facultatif</span><span class="sxs-lookup"><span data-stu-id="7d7b3-235">Optional</span></span> |


## <a name="additional-resources"></a><span data-ttu-id="7d7b3-236">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7d7b3-236">Additional Resources</span></span>

* [<span data-ttu-id="7d7b3-237">Intergiciel (middleware)</span><span class="sxs-lookup"><span data-stu-id="7d7b3-237">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="7d7b3-238">Serveurs</span><span class="sxs-lookup"><span data-stu-id="7d7b3-238">Servers</span></span>](servers/index.md)
