---
title: "Dérivation de sous-clés et de chiffrement authentifié"
author: rick-anderson
description: "Ce document explique les détails d’implémentation de la protection des données ASP.NET Core dérivation des sous-clés et authentifié de chiffrement."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: 4b905bbc7bb064b6ba1741557bd694c8c67ccfa8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="subkey-derivation-and-authenticated-encryption"></a>Dérivation de sous-clés et de chiffrement authentifié

<a name="data-protection-implementation-subkey-derivation"></a>

La plupart des clés dans l’anneau de clé contiendra une certaine forme d’entropie et contient des informations algorithmiques indiquant « le chiffrement en mode CBC + validation de HMAC » ou « chiffrement de GCM + validation ». Dans ce cas, nous nous référons à l’entropie incorporé en tant que le support de gestion (ou KM) pour cette clé, et que nous effectuons une fonction de dérivation de clé pour dériver les clés qui seront utilisés pour les opérations de chiffrement réelles.

> [!NOTE]
> Les clés sont abstraites, et une implémentation personnalisée ne peut-être pas se comporter comme indiqué ci-dessous. Si la clé fournit sa propre implémentation de `IAuthenticatedEncryptor` au lieu d’utiliser une des usines intégrées, le mécanisme décrit dans cette section ne s’applique plus.

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>Données authentifiées supplémentaires et la dérivation de la sous-clé

Le `IAuthenticatedEncryptor` interface sert d’interface de base pour toutes les opérations de chiffrement authentifié. Son `Encrypt` méthode prend deux mémoires tampons : texte en clair et additionalAuthenticatedData (AAD). Le flux de contenu de texte en clair sans modification de l’appel à `IDataProtector.Protect`, mais le AAD est généré par le système et se compose de trois composants :

1. L’en-tête magic 32 bits 09 F0 C9 F0 qui identifie cette version du système de protection des données.

2. L’id de clé 128 bits.

3. Une chaîne de longueur variable formée à partir de la chaîne de l’objectif qui a créé le `IDataProtector` qui effectue cette opération.

Étant donné que le AAD est unique pour le tuple de tous les trois composants, nous pouvons l’utiliser pour dériver les nouvelles clés à partir du Gestionnaire de clés au lieu d’utiliser KM lui-même dans tous nos opérations cryptographiques. Pour chaque appel à `IAuthenticatedEncryptor.Encrypt`, le processus de dérivation de clé suivant a lieu :

( K_E, K_H ) = SP800_108_CTR_HMACSHA512(K_M, AAD, contextHeader || keyModifier)

Ici, nous appelons NIST SP800-108 KDF en Mode de compteur (consultez [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), s 5.1) avec les paramètres suivants :

* Clé de dérivation de clé (KDK) = K_M

* PRF = HMACSHA512

* étiquette = additionalAuthenticatedData

* contexte = contextHeader || keyModifier

L’en-tête de contexte est de longueur variable et sert essentiellement une empreinte numérique des algorithmes pour lequel nous allons dérivation K_E et K_H. Le modificateur de clé est une chaîne de 128 bits générée de façon aléatoire pour chaque appel à `Encrypt` et sert à garantir avec surcharger la probabilité que KE et KH sont uniques pour cette opération de chiffrement d’authentification spécifique, même si toutes les autres entrées au KDF sont constante.

Pour le chiffrement en mode CBC + des opérations de validation HMAC, | K_E | est la longueur de la clé de chiffrement symétrique, et | K_H | est la taille du résumé de la routine HMAC. Pour le chiffrement de GCM + des opérations de validation, | K_H | = 0.

## <a name="cbc-mode-encryption--hmac-validation"></a>Le chiffrement en mode CBC + validation de HMAC

Une fois que K_E est généré par le biais du mécanisme ci-dessus, nous générer un vecteur d’initialisation aléatoire et exécuter l’algorithme de chiffrement symétrique pour chiffrer le texte en clair. Le vecteur d’initialisation et le texte chiffré sont ensuite exécutées via la routine HMAC initialisée avec la clé K_H pour produire le Mac. Ce processus et la valeur de retour est représenté sous forme graphique ci-dessous.

![Retour et les processus en mode CBC](subkeyderivation/_static/cbcprocess.png)

*output:= keyModifier || iv || E_cbc (K_E,iv,data) || HMAC(K_H, iv || E_cbc (K_E,iv,data))*

> [!NOTE]
> Le `IDataProtector.Protect` implémentation sera [ajouter l’en-tête magic et l’id de clé](authenticated-encryption-details.md) de sortie avant de le renvoyer à l’appelant. Étant donné que l’en-tête magic et l’id de clé sont implicitement dans le cadre du [AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad), et étant donné que le modificateur de clé est acheminé comme entrée au KDF, cela signifie que chaque octet de la charge utile de retourné finale est authentifié par le Mac.

## <a name="galoiscounter-mode-encryption--validation"></a>Le chiffrement du Mode de compteur/GALOIS + validation

Une fois que K_E est généré par le biais du mécanisme ci-dessus, nous générer une valeur à usage unique à 96 bits aléatoire et exécuter l’algorithme de chiffrement symétrique pour chiffrer le texte en clair et de produire la balise d’authentification de 128 bits.

![Retour et les processus en mode GCM](subkeyderivation/_static/galoisprocess.png)

*sortie : = keyModifier || valeur à usage unique || E_gcm (K_E, valeur à usage unique, de données) || authTag*

> [!NOTE]
> Même si GCM prend nativement en charge le concept d’AAD, nous sommes toujours à l’alimentation AAD uniquement KDF d’origine, pour passer une chaîne vide en GCM pour son paramètre AAD. La raison à cela est double. Tout d’abord, [pour prendre en charge agilité](context-headers.md#data-protection-implementation-context-headers) que nous ne souhaitons utiliser K_M directement en tant que la clé de chiffrement. En outre, GCM impose des exigences très strictes unicité sur ses entrées. La probabilité que la routine de chiffrement GCM n’est jamais appelée sur deux ou plus distincte des jeux de données d’entrée avec le même (clé, valeur à usage unique) paire ne doit pas dépasser 2 ^ 32. Si nous corriger K_E nous ne pouvons pas effectuer plus de 2 ^ 32 opérations de chiffrement avant d’exécuter que nous afoul of le 2 ^ -32 limiter. Cela peut sembler un très grand nombre d’opérations, mais un serveur web de trafic élevé peut subir de 4 milliards de demandes en jours simples, dans la durée de vie normale pour ces clés. Pour rester conformes de 2 ^ limite de la probabilité de-32, que nous continuons à utiliser un modificateur de clé de 128 bits et les 96 bits valeur à usage unique, qui étend radicalement le nombre d’opérations utilisable pour n’importe quel K_M donné. Par souci de simplicité de conception, nous partageons le chemin d’accès du code KDF entre les opérations CBC et GCM et AAD est déjà pris en compte dans le KDF il est inutile de transférer à la routine GCM.
