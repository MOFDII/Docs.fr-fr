---
title: Mise en cache dans ASP.NET Core
author: rick-anderson
description: "Découvrez comment mettre en cache les données en mémoire dans ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/memory
ms.openlocfilehash: 64635235c11b55818da02d63d044334f4b2cdb08
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2018
---
# <a name="in-memory-caching-in-aspnet-core"></a>Mise en cache dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), et [Steve Smith](https://ardalis.com/)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="caching-basics"></a>Principes fondamentaux de la mise en cache

La mise en cache peut améliorer considérablement les performances et l’évolutivité d’une application en réduisant le travail requis pour générer le contenu. La mise en cache fonctionne mieux avec les données qui sont rarement modifiées. La mise en cache permet une copie des données qui peuvent être renvoyées beaucoup plus rapidement qu’à partir de la source d’origine. Vous devez écrire et tester votre application pour ne jamais dépendre des données mises en cache.

ASP.NET Core prend en charge plusieurs caches différents. Le cache de la plus simple est basé sur le [IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache), qui représente un cache stocké dans la mémoire du serveur web. Les applications qui s’exécutent sur une batterie de serveurs de plusieurs serveurs devraient vous assurer que les sessions rémanentes lors de l’utilisation du cache en mémoire. Sessions rémanentes Assurez-vous que les requêtes ultérieures à partir d’un client tous les dirigé vers le même serveur. Par exemple, les utilisation d’applications Web Azure [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) pour router toutes les demandes ultérieures au même serveur.

Sessions non persistantes dans une batterie de serveurs web nécessitent un [cache distribué](distributed.md) pour éviter les problèmes de cohérence du cache. Pour certaines applications, un cache distribué peut prendre en charge la plus élevée de montée en charge qu’un cache en mémoire. À l’aide d’un cache distribué permet de décharger la mémoire cache pour un processus externe. 

Le `IMemoryCache` cache supprimez les entrées du cache mémoire insuffisante, sauf si le [cache priorité](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheitempriority) a la valeur `CacheItemPriority.NeverRemove`. Vous pouvez définir le `CacheItemPriority` pour ajuster la priorité avec laquelle le cache supprime les éléments de sollicitation de la mémoire.

Le cache en mémoire permettre stocker n’importe quel objet ; l’interface de cache distribué est limité à `byte[]`.

## <a name="using-imemorycache"></a>À l’aide de IMemoryCache

La mise en cache en mémoire est un *service* qui est référencé à partir de votre application à l’aide de [Injection de dépendance](../../fundamentals/dependency-injection.md). Appelez `AddMemoryCache` dans `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=8)] 

Demander le `IMemoryCache` instance dans le constructeur :

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor&highlight=3,5-999)] 

`IMemoryCache` nécessite le package NuGet « Microsoft.Extensions.Caching.Memory ».

Le code suivant utilise [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) pour vérifier si une heure est dans le cache. Si une heure n’est pas mis en cache, une nouvelle entrée est créée et ajoutée au cache avec [définir](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

L’heure actuelle et l’heure de mise en cache sont affichent :

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

La mise en cache `DateTime` valeur reste dans le cache, bien qu’il existe des demandes dans le délai d’expiration (et aucune suppression en raison d’une sollicitation de la mémoire). L’illustration suivante montre l’heure actuelle et une heure antérieure récupérés du cache :

![Vue d’index avec deux fois différentes affichées](memory/_static/time.png)

Le code suivant utilise [GetOrCreate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) et [GetOrCreateAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) en cache des données. 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Le code suivant appelle [obtenir](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) pour extraire l’heure de mise en cache :

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

Consultez [IMemoryCache méthodes](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache) et [CacheExtensions méthodes](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions) pour obtenir une description des méthodes du cache.

## <a name="using-memorycacheentryoptions"></a>À l’aide de MemoryCacheEntryOptions

L’exemple suivant :

- Définit l’heure d’expiration absolue. Ceci est la durée maximale que l’entrée peut être mis en cache et empêche l’élément de devenir trop périmées lors de l’expiration décalée est constamment renouvelée.
- Définit un délai d’expiration décalée. Les requêtes qui accèdent à cet élément de mise en cache réinitialise l’horloge d’expiration décalée.
- Définit la priorité de cache `CacheItemPriority.NeverRemove`. 
- Définit un [PostEvictionDelegate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.postevictiondelegate) qui est appelée après la suppression de l’entrée du cache. Le rappel est exécuté sur un thread différent du code qui supprime l’élément à partir du cache.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-20)]

## <a name="cache-dependencies"></a>Dépendances de cache

L’exemple suivant montre comment le point d’expirer une entrée de cache si une entrée de dépendance expire. A `CancellationChangeToken` est ajouté à l’élément mis en cache. Lorsque `Cancel` est appelée sur le `CancellationTokenSource`, les deux entrées du cache sont supprimées. 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

À l’aide un `CancellationTokenSource` permet à plusieurs entrées de cache à supprimer en tant que groupe. Avec la `using` modèle dans le code ci-dessus, les entrées de cache créées à l’intérieur du `using` bloc hériteront des déclencheurs et les paramètres d’expiration.

## <a name="additional-notes"></a>Remarques supplémentaires

- Lorsque vous utilisez un rappel pour remplir un élément de cache à :

  - La valeur de clé mise en cache peuvent trouver vide plusieurs demandes étant donné que le rappel n’est pas terminée. 
  - Cela peut entraîner le remplissage de l’élément mis en cache de plusieurs threads.

- Lorsqu’une entrée de cache est utilisée pour créer un autre, l’enfant copie de l’entrée parente des jetons d’expiration et les paramètres d’expiration basés sur le temps. L’enfant n’est pas expiré par la suppression manuelle ou mise à jour de l’entrée du parent.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Utilisation d’un cache distribué](xref:performance/caching/distributed)
* [Détecter les modifications à l’aide de jetons de modification](xref:fundamentals/primitives/change-tokens)
* [Mise en cache des réponses](xref:performance/caching/response)
* [Intergiciel de mise en cache des réponses](xref:performance/caching/middleware)
* [Tag Helper de cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Tag Helper de cache distribué](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
