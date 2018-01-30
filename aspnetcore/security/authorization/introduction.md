---
title: "Introduction à l’autorisation"
author: rick-anderson
description: "Ce document fournit une explication de l’autorisation de base et explique comment l’autorisation est lié à ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: a6bd81a4e5796c1d0a0033c2b8d5a6ba9282564c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction"></a>Introduction

<a name="security-authorization-introduction"></a>

Autorisation désigne le processus qui détermine ce qu’un utilisateur est en mesure d’effectuer. Par exemple, un utilisateur administratif est autorisé à créer une bibliothèque de documents, ajoutez les documents, modifier des documents et les supprimer. Un utilisateur non-administrateur, utilisation de la bibliothèque est uniquement autorisé à lire les documents.

L’autorisation est orthogonales et indépendante de l’authentification, qui est le processus d’évaluation qui est un utilisateur. L’authentification peut créer une ou plusieurs identités pour l’utilisateur actuel.

## <a name="authorization-types"></a>Types d’autorisation

Autorisation de ASP.NET Core fournit un simple déclaratif [rôle](roles.md) et un [enrichie basée sur la stratégie](policies.md) modèle. L’autorisation est exprimée dans la configuration requise et gestionnaires d’évaluent les revendications d’un utilisateur par rapport aux exigences. Vérifications impératives peuvent reposer sur des stratégies simples ou qui évaluent l’identité de l’utilisateur et les propriétés de la ressource à laquelle l’utilisateur tente d’accéder.

## <a name="namespaces"></a>Espaces de noms

Les composants d’autorisation, y compris le `AuthorizeAttribute` et `AllowAnonymousAttribute` attributs sont trouvent dans le `Microsoft.AspNetCore.Authorization` espace de noms.
