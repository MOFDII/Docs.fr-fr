---
title: Choisir entre ASP.NET et ASP.NET Core
author: rick-anderson
description: "Découvrez comment choisir entre ASP.NET et ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 232e82ed66ff2363230ff09d435db1074c02b53b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="61b08-103">Choisir entre ASP.NET et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61b08-103">Choose between ASP.NET and ASP.NET Core</span></span> 

<span data-ttu-id="61b08-104">Quelle que soit l’application web à créer, ASP.NET a une solution pour vous : depuis les applications web d’entreprise ciblant Windows Server aux microservices ciblant des conteneurs Linux, et tout ce qui se trouve entre les deux.</span><span class="sxs-lookup"><span data-stu-id="61b08-104">No matter the web application you are creating, ASP.NET has a solution for you: from enterprise web applications targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="61b08-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61b08-105">ASP.NET Core</span></span>

<span data-ttu-id="61b08-106">ASP.NET Core est un framework open source et multiplateforme pour générer des applications web cloud modernes sur Windows, macOS ou Linux.</span><span class="sxs-lookup"><span data-stu-id="61b08-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web applications on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="61b08-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="61b08-107">ASP.NET</span></span>

<span data-ttu-id="61b08-108">ASP.NET est un framework mature qui offre tous les services nécessaires pour générer sur Windows des applications web d’entreprise basées sur des serveurs.</span><span class="sxs-lookup"><span data-stu-id="61b08-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web applications on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="61b08-109">Laquelle des deux est adaptée à mes besoins ?</span><span class="sxs-lookup"><span data-stu-id="61b08-109">Which one is right for me?</span></span>

| <span data-ttu-id="61b08-110">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61b08-110">ASP.NET Core</span></span> | <span data-ttu-id="61b08-111">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="61b08-111">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="61b08-112">Générer pour Windows, macOS ou Linux</span><span class="sxs-lookup"><span data-stu-id="61b08-112">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="61b08-113">Générer pour Windows</span><span class="sxs-lookup"><span data-stu-id="61b08-113">Build for Windows</span></span>|
|<span data-ttu-id="61b08-114">Les [pages Razor](xref:mvc/razor-pages/index) constituent l’approche recommandée pour créer une interface utilisateur web avec ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="61b08-114">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span> <span data-ttu-id="61b08-115">Consultez aussi [MVC](xref:mvc/overview) et [API web](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="61b08-115">See also [MVC](xref:mvc/overview) and [Web API](xref:tutorials/first-web-api)</span></span>|<span data-ttu-id="61b08-116">Utiliser [Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [API Web](https://docs.microsoft.com/aspnet/web-api/), ou [Pages Web](https://docs.microsoft.com/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="61b08-116">Use [Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [Web API](https://docs.microsoft.com/aspnet/web-api/), or [Web Pages](https://docs.microsoft.com/aspnet/web-pages)</span></span>|
|<span data-ttu-id="61b08-117">Plusieurs versions par machine</span><span class="sxs-lookup"><span data-stu-id="61b08-117">Multiple versions per machine</span></span>|<span data-ttu-id="61b08-118">Une seule version par machine</span><span class="sxs-lookup"><span data-stu-id="61b08-118">One version per machine</span></span>|
|<span data-ttu-id="61b08-119">Développer avec Visual Studio, [Visual Studio pour Mac](https://www.visualstudio.com/vs/visual-studio-mac/) ou [Visual Studio Code](https://code.visualstudio.com/) en utilisant C# ou F#</span><span class="sxs-lookup"><span data-stu-id="61b08-119">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="61b08-120">Développer avec Visual Studio en utilisant C#, VB ou F#</span><span class="sxs-lookup"><span data-stu-id="61b08-120">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="61b08-121">Performances supérieures à celles d’ASP.NET</span><span class="sxs-lookup"><span data-stu-id="61b08-121">Higher performance than ASP.NET</span></span>|<span data-ttu-id="61b08-122">Bonnes performances</span><span class="sxs-lookup"><span data-stu-id="61b08-122">Good performance</span></span>|
|[<span data-ttu-id="61b08-123">Choisir le runtime .NET Framework ou .NET Core</span><span class="sxs-lookup"><span data-stu-id="61b08-123">Choose .NET Framework or .NET Core runtime</span></span>](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="61b08-124">Utiliser le runtime .NET Framework</span><span class="sxs-lookup"><span data-stu-id="61b08-124">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="61b08-125">Scénarios ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61b08-125">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="61b08-126">Les [pages Razor](xref:mvc/razor-pages/index) constituent l’approche recommandée pour créer une interface utilisateur web avec ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="61b08-126">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span>
* [<span data-ttu-id="61b08-127">Sites web</span><span class="sxs-lookup"><span data-stu-id="61b08-127">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="61b08-128">API</span><span class="sxs-lookup"><span data-stu-id="61b08-128">APIs</span></span>](xref:tutorials/first-web-api)

## <a name="aspnet-scenarios"></a><span data-ttu-id="61b08-129">Scénarios ASP.NET</span><span class="sxs-lookup"><span data-stu-id="61b08-129">ASP.NET scenarios</span></span>

* [<span data-ttu-id="61b08-130">Sites web</span><span class="sxs-lookup"><span data-stu-id="61b08-130">Websites</span></span>](https://docs.microsoft.com/aspnet/mvc)
* [<span data-ttu-id="61b08-131">API</span><span class="sxs-lookup"><span data-stu-id="61b08-131">APIs</span></span>](https://docs.microsoft.com/aspnet/web-api)
* [<span data-ttu-id="61b08-132">Temps réel</span><span class="sxs-lookup"><span data-stu-id="61b08-132">Real-time</span></span>](https://docs.microsoft.com/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="61b08-133">Ressources</span><span class="sxs-lookup"><span data-stu-id="61b08-133">Resources</span></span>

* [<span data-ttu-id="61b08-134">Présentation d’ASP.NET</span><span class="sxs-lookup"><span data-stu-id="61b08-134">Introduction to ASP.NET</span></span>](https://docs.microsoft.com/aspnet/overview)
* [<span data-ttu-id="61b08-135">Présentation d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="61b08-135">Introduction to ASP.NET Core</span></span>](xref:index)
