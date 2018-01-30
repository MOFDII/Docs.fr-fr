---
title: Hachage de mot de passe
author: rick-anderson
description: "Ce document explique comment le hachage des mots de passe à l’aide de l’API de protection des données ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 536499687865b4e4d5b1d9c4076623b5ac1ffdbe
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="password-hashing"></a>Hachage de mot de passe

La base de code de protection de données inclut un package *Microsoft.AspNetCore.Cryptography.KeyDerivation* qui contient les fonctions de dérivation de clé de chiffrement. Ce package est un composant autonome et n’a aucune dépendance sur le reste du système de protection des données. Il peut être utilisé totalement indépendants. La source existe en même temps que le code de protection des données base pour des raisons pratiques.

Le package actuellement propose une méthode `KeyDerivation.Pbkdf2` qui permet de hachage d’un mot de passe à l’aide de la [PBKDF2 algorithme](https://tools.ietf.org/html/rfc2898#section-5.2). Cette API méthode est très similaire à .NET Framework existant [Rfc2898DeriveBytes type](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), mais il existe trois des différences importantes :

1. Le `KeyDerivation.Pbkdf2` méthode prend en charge la consommation de plusieurs PRF (actuellement `HMACSHA1`, `HMACSHA256`, et `HMACSHA512`), alors que le `Rfc2898DeriveBytes` prend uniquement en charge de type `HMACSHA1`.

2. Le `KeyDerivation.Pbkdf2` méthode détecte le système d’exploitation actuel et tente de choisir l’implémentation plus optimisée de la routine, en fournissant de bien meilleures performances dans certains cas. (Sous Windows 8, il offre environ 10 x le débit de `Rfc2898DeriveBytes`.)

3. Le `KeyDerivation.Pbkdf2` méthode exige de l’appelant spécifier tous les paramètres (SEL, PRF et nombre d’itérations). Le `Rfc2898DeriveBytes` type fournit les valeurs par défaut pour ces.

[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]

Consultez le code source pour ASP.NET Core Identity `PasswordHasher` cas d’utilisation de type pour un monde réel.
