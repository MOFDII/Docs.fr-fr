---
title: Gestion de clés de Protection des données et la durée de vie dans ASP.NET Core
author: rick-anderson
description: En savoir plus sur la gestion de clés de Protection des données et la durée de vie dans ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 54259b1e2f37cdbbd551038e80f2b0fa1d77f196
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277803"
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>Gestion de clés de Protection des données et la durée de vie dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>Gestion de clés

L’application tente de détecter son environnement d’exploitation et de gérer la configuration de la clé sur son propre.

1. Si l’application est hébergée dans [applications Azure](https://azure.microsoft.com/services/app-service/), les clés sont rendues persistantes dans le *%HOME%\ASP.NET\DataProtection-Keys* dossier. Ce dossier est alimenté par le stockage réseau et synchronisé sur tous les ordinateurs hébergeant l’application.
   * Les clés ne sont pas protégées au repos.
   * Le *DataProtection-clés* dossier fournit l’anneau de clé à toutes les instances d’une application dans un emplacement de déploiement.
   * Les emplacements de déploiement distincts, tels que Préproduction et Production, ne partagent pas de porte-clés. Lorsque vous échangez entre les emplacements de déploiement, par exemple le remplacement intermédiaire en Production ou à l’aide de / B test, n’importe quelle application à l’aide de la Protection des données ne pourra plus être déchiffrer les données stockées à l’aide de l’anneau de clé à l’intérieur de l’emplacement précédent. Cela conduit aux utilisateurs est enregistrés en dehors d’une application qui utilise l’authentification de cookie ASP.NET Core standard, car elle utilise la Protection des données à protéger ses cookies. Si vous le souhaitez, indépendant de l’emplacement de clé anneaux utilisent un fournisseur de l’anneau de clé externe, telles que le stockage Blob Azure, Azure Key Vault, un magasin SQL, ou le cache Redis.

1. Si le profil utilisateur est disponible, les clés sont rendues persistantes dans le *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* dossier. Si le système d’exploitation est Windows, les clés sont chiffrées au repos à l’aide de DPAPI.

1. Si l’application est hébergée dans IIS, les clés sont conservées dans le Registre HKLM dans une clé de Registre spéciale qui est l’ACL sur uniquement pour le compte de processus de travail. Les clés sont chiffrées au repos à l’aide de DPAPI.

1. Si aucune de ces conditions, les clés ne sont pas rendues persistantes en dehors du processus en cours. Lorsque le processus s’arrête, tous les généré les clés sont perdues.

Le développeur est toujours dans un contrôle total et peut remplacer comment et où les clés sont stockées. Les trois premières options ci-dessus doivent fournir des valeurs par défaut corrects pour la plupart des applications similaires à la manière dont ASP.NET  **\<machineKey >** les routines de la génération automatique a fonctionné dans le passé. L’option de secours, finale est le seul scénario qui nécessite le développeur à spécifier [configuration](xref:security/data-protection/configuration/overview) initial s’ils veulent persistance des clés, mais ce basculement se produit uniquement dans les rares cas.

Lorsque vous hébergez dans un conteneur Docker, clés doivent être persistante dans un dossier qui est un volume Docker (un volume partagé ou un volume monté hôte qui persiste au-delà de la durée de vie du conteneur) ou dans un fournisseur externe, tel que [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ou [Redis](https://redis.io/). Un fournisseur externe est également utile dans les scénarios de batterie de serveurs web si les applications ne peut pas accéder à un volume partagé de réseau (voir [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) pour plus d’informations).

> [!WARNING]
> Si le développeur remplace les règles décrites ci-dessus et le système de Protection des données dans un dépôt de clé spécifique de points, un chiffrement de clés à l’arrêt automatique est désactivé. Une protection à l’arrêt peut être réactivée via [configuration](xref:security/data-protection/configuration/overview).

## <a name="key-lifetime"></a>Durée de vie

Par défaut, les clés ont une durée de vie de 90 jours. Lorsqu’une clé expire, l’application génère une nouvelle clé et définit la nouvelle clé comme clé active automatiquement. Tant que clés supprimées restent sur le système, votre application peut déchiffrer les données protégées avec eux. Consultez [gestion de clés](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) pour plus d’informations.

## <a name="default-algorithms"></a>Algorithmes par défaut

L’algorithme de protection de charge utile par défaut utilisé est AES-256-CBC pour la confidentialité et HMACSHA256 authenticité. Une clé principale de 512 bits, modifiée tous les 90 jours, est utilisée pour dériver les deux sous-clés utilisés pour ces algorithmes sur une base par charge. Consultez [des sous-clés de dérivation](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) pour plus d’informations.

## <a name="see-also"></a>Voir aussi

* [Extensibilité de la gestion de clés](xref:security/data-protection/extensibility/key-management)
