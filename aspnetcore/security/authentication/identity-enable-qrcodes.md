---
title: "Activer la génération de Code QR pour les applications d’authentification dans ASP.NET Core"
author: rick-anderson
description: "Activer la génération de Code QR pour les applications d’authentification dans ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 09/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 87a6d3f17216625e0f7ce206dddd72cb2f371e9a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a>Activer la génération de Code QR pour les applications d’authentification dans ASP.NET Core

Remarque : Cette rubrique s’applique à ASP.NET Core 2.x

ASP.NET Core est livré avec prise en charge pour les applications de l’authentificateur pour l’authentification individuelle. Deux applications-Factor authentication (2FA) l’authentificateur, à l’aide d’une durée à usage unique mot de passe algorithme (TOTP), sont l’approche pour 2FA recommandée. 2FA à l’aide de TOTP est préférée à 2FA SMS. Une application d’authentification fournit un code de 6 à 8 chiffres que les utilisateurs doivent entrer après confirmation de leur nom d’utilisateur et un mot de passe. En règle générale, une application d’authentification est installée sur un Smartphone.

Les modèles d’application web ASP.NET Core prennent en charge des authentificateurs, mais ne fournissent pas de prise en charge pour la génération de QRCode. Les générateurs de QRCode facilitent le programme d’installation de 2FA. Ce document va vous guider ajout [Code QR](https://wikipedia.org/wiki/QR_code) génération à la page de configuration 2FA.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Ajout des Codes QR à la page de configuration 2FA

Ces instructions utilisent *qrcode.js* du référentiel https://davidshimjs.github.io/qrcodejs/.

* Téléchargez le [bibliothèque javascript pour qrcode.js](https://davidshimjs.github.io/qrcodejs/) à la `wwwroot\lib` dossier dans votre projet.

* Dans *Pages\Account\Manage\EnableAuthenticator.cshtml* (Pages Razor) ou *Views\Manage\EnableAuthenticator.cshtml* (MVC), recherchez le `Scripts` section à la fin du fichier :

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Mise à jour la `Scripts` section pour ajouter une référence à la `qrcodejs` bibliothèque que vous avez ajouté et un appel pour générer le Code QR. Il doit se présenter comme suit :

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="~/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* Supprimer le paragraphe qui propose des liens vers ces instructions.

Exécuter votre application et vous assurer que vous pouvez analyser le code QR et valider le code que destiné à prouver l’authentificateur.

## <a name="change-the-site-name-in-the-qr-code"></a>Modifier le nom du site dans le Code QR

Le nom du site dans le Code QR provient du nom du projet que vous sélectionnez lorsque vous créez votre projet. Vous pouvez le modifier en recherchant le `GenerateQrCodeUri(string email, string unformattedKey)` méthode dans le *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* les fichiers (Pages Razor) ou le *Controllers\ManageController.cs* fichier (MVC). 

Le code à partir du modèle par défaut se présente comme suit :

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

Le deuxième paramètre dans l’appel à `string.Format` est le nom de votre site, obtenue à partir de votre nom de la solution. Il peut être modifié par n’importe quelle valeur, mais il doit toujours être codé URL.

## <a name="using-a-different-qr-code-library"></a>À l’aide d’une autre bibliothèque de Code QR

Vous pouvez remplacer la bibliothèque de Code QR avec la bibliothèque par défaut. Le code HTML contient un `qrCode` élément dans lequel vous pouvez placer un Code QR par le mécanisme de votre bibliothèque fournit.

L’URL correcte pour le Code QR est disponible dans le :

* `AuthenticatorUri`propriété du modèle.
* `data-url`propriété dans le `qrCodeData` élément. 

Utilisez `@Html.Raw` pour accéder à la propriété de modèle dans une vue (sinon les signes & dans l’url doit être encodés en double et le paramètre d’étiquette du Code QR est ignoré).

## <a name="totp-client-and-server-time-skew"></a>Décalage horaire TOTP client et serveur

Authentification de TOTP dépend du périphérique à la fois le serveur et un authentificateur ayant une heure précise. Les jetons ne durent que 30 secondes. Si des connexions de 2FA TOTP échouent, vérifiez que l’heure du serveur est précises et, de préférence, synchronisées avec un service NTP précis.
