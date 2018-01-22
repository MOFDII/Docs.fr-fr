---
title: "Fournisseurs de protection des données éphémères"
author: rick-anderson
description: "Ce document explique les détails d’implémentation des fournisseurs de protection des données éphémères ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 9c1d03373c9d7fb6dffb3583c58aa593fd3875f4
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="ephemeral-data-protection-providers"></a><span data-ttu-id="a0a26-103">Fournisseurs de protection des données éphémères</span><span class="sxs-lookup"><span data-stu-id="a0a26-103">Ephemeral data protection providers</span></span>

<a name="data-protection-implementation-key-storage-ephemeral"></a>

<span data-ttu-id="a0a26-104">Il existe des scénarios où une application a besoin d’un throwaway `IDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="a0a26-104">There are scenarios where an application needs a throwaway `IDataProtectionProvider`.</span></span> <span data-ttu-id="a0a26-105">Par exemple, le développeur peut simplement être expérimentation dans une application console uniques, ou l’application elle-même est transitoire (elle est basée sur un script ou un test unitaire projet).</span><span class="sxs-lookup"><span data-stu-id="a0a26-105">For example, the developer might just be experimenting in a one-off console application, or the application itself is transient (it's scripted or a unit test project).</span></span> <span data-ttu-id="a0a26-106">Pour prendre en charge ces scénarios le [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) package inclut un type `EphemeralDataProtectionProvider`.</span><span class="sxs-lookup"><span data-stu-id="a0a26-106">To support these scenarios the [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) package includes a type `EphemeralDataProtectionProvider`.</span></span> <span data-ttu-id="a0a26-107">Ce type fournit une implémentation de base de `IDataProtectionProvider` clé dont le référentiel est maintenu en mémoire uniquement et n’est pas écrit dans n’importe quel magasin de stockage.</span><span class="sxs-lookup"><span data-stu-id="a0a26-107">This type provides a basic implementation of `IDataProtectionProvider` whose key repository is held solely in-memory and isn't written out to any backing store.</span></span>

<span data-ttu-id="a0a26-108">Chaque instance de `EphemeralDataProtectionProvider` utilise sa propre clé principale unique.</span><span class="sxs-lookup"><span data-stu-id="a0a26-108">Each instance of `EphemeralDataProtectionProvider` uses its own unique master key.</span></span> <span data-ttu-id="a0a26-109">Par conséquent, si un `IDataProtector` enracinée à un `EphemeralDataProtectionProvider` génère une charge utile protégée, cette charge utile peut uniquement être protégée par un équivalent `IDataProtector` (les données sont identiques [objectif](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) chaîne) à la même racine `EphemeralDataProtectionProvider` instance.</span><span class="sxs-lookup"><span data-stu-id="a0a26-109">Therefore, if an `IDataProtector` rooted at an `EphemeralDataProtectionProvider` generates a protected payload, that payload can only be unprotected by an equivalent `IDataProtector` (given the same [purpose](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) chain) rooted at the same `EphemeralDataProtectionProvider` instance.</span></span>

<span data-ttu-id="a0a26-110">L’exemple suivant montre comment instancier un `EphemeralDataProtectionProvider` et son utilisation pour protéger et déprotéger les données.</span><span class="sxs-lookup"><span data-stu-id="a0a26-110">The following sample demonstrates instantiating an `EphemeralDataProtectionProvider` and using it to protect and unprotect data.</span></span>

```csharp
using System;
using Microsoft.AspNetCore.DataProtection;

public class Program
{
    public static void Main(string[] args)
    {
        const string purpose = "Ephemeral.App.v1";

        // create an ephemeral provider and demonstrate that it can round-trip a payload
        var provider = new EphemeralDataProtectionProvider();
        var protector = provider.CreateProtector(purpose);
        Console.Write("Enter input: ");
        string input = Console.ReadLine();

        // protect the payload
        string protectedPayload = protector.Protect(input);
        Console.WriteLine($"Protect returned: {protectedPayload}");

        // unprotect the payload
        string unprotectedPayload = protector.Unprotect(protectedPayload);
        Console.WriteLine($"Unprotect returned: {unprotectedPayload}");

        // if I create a new ephemeral provider, it won't be able to unprotect existing
        // payloads, even if I specify the same purpose
        provider = new EphemeralDataProtectionProvider();
        protector = provider.CreateProtector(purpose);
        unprotectedPayload = protector.Unprotect(protectedPayload); // THROWS
    }
}

/*
* SAMPLE OUTPUT
*
* Enter input: Hello!
* Protect returned: CfDJ8AAAAAAAAAAAAAAAAAAAAA...uGoxWLjGKtm1SkNACQ
* Unprotect returned: Hello!
* << throws CryptographicException >>
*/
```
