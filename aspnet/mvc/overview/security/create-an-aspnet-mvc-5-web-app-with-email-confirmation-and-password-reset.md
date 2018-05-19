---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Créer une application web ASP.NET MVC 5 sécurisée avec identification, confirmation par courrier électronique et réinitialisation de mot de passe (C#) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment créer une application web ASP.NET MVC 5 avec confirmation par courrier électronique et réinitialisation de mot de passe en utilisant le système d’appartenance d'ASP.NET Identity.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: d55b34135d5bab98ab8de31cc4b12dcc272cbc0a
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/20/2018
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Créer une application web ASP.NET MVC 5 sécurisée avec identification, confirmation par courrier électronique et réinitialisation de mot de passe (C#)
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> Ce didacticiel vous montre comment créer une application de web ASP.NET MVC 5 avec confirmation par courrier électronique et réinitialisé de mot de passe en utilisant le système d’appartenance d'ASP.NET Identity. Vous pouvez télécharger l’application terminée [ici](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Le téléchargement contient les programmes d’assistance de débogage qui vous permettent de tester une confirmation par courrier électronique et SMS sans configurer un fournisseur  courrier électronique ou de SMS.
> Ce didacticiel a été écrit par [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter : [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Créer une application ASP.NET MVC

Commencez par installer et exécuter [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure.

> [!NOTE]
> Avertissement : Vous devez installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure pour effectuer ce didacticiel.


1. Créez un projet Web ASP.NET et sélectionnez le modèle MVC. Le modèle Web Forms prend également en charge ASP.NET Identity, afin de pouvoir suivre des étapes similaires dans une application web forms.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Laissez l’authentification par défaut à **Comptes d’utilisateur individuels**. Si vous souhaitez héberger l’application dans Azure, laissez la case à cocher activée. Plus loin dans ce didacticiel, nous déploierons sur Azure. Vous pouvez [ouvrir un compte Azure gratuitement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Définir le [projet pour utiliser SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Exécuter l’application, cliquez sur le lien **S'inscrire** et inscrire un utilisateur. À ce stade, la seule validation de l’adresse de messagerie est avec l'attribut [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx).
5. Dans l’Explorateur de serveurs, accédez à **Data Connections\DefaultConnection\Tables\AspNetUsers**, cliquez avec le bouton droit et sélectionnez **Ouvrir la définition de la table**.

    L’illustration suivante montre le schéma `AspNetUsers` :

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Cliquez avec le bouton droit sur la table **AspNetUsers** et sélectionnez **Afficher les données de la table**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 À ce stade l’adresse de messagerie n’a pas été confirmée.
7. Cliquez sur la ligne et sélectionnez Supprimer. Vous allez ajouter cette adresse de messagerie à nouveau à l’étape suivante et envoyer un message électronique de confirmation.

## <a name="email-confirmation"></a>E-mail de confirmation

Il est recommandé de confirmer l’adresse de messagerie d’un nouvel enregistrement d’utilisateur pour vérifier qu’ils n’empruntent pas l'identité d'une autre personne (autrement dit, qu'ils ne se sont pas inscrits avec par l'adresse de messagerie de quelqu'un d’autre). Supposons que vous aviez un forum de discussion, vous voudriez empêcher `"bob@example.com"` de s'inscrire en tant que `"joe@contoso.com"`. Sans la confirmation par courrier électronique, `"joe@contoso.com"` pourrait recevoir du courrier indésirable à partir de votre application. Supposons que Bob se soit accidentellement inscrit en tant que `"bib@example.com"` et ne l'ait pas remarqué, il ne serait pas en mesure d’utiliser le mot de passe de récupération, car l’application n’a pas son e-mail correcte. l'email de confirmation offre uniquement une protection limitée contre les robots et ne fournit de protection à partir des expéditeurs déterminés comme spammers, ils ont plusieurs alias de messagerie de travail qu’ils peuvent utiliser pour s'inscrire.

En règle générale, vous souhaitez empêcher les nouveaux utilisateurs d'envoyer des données à votre site web avant qu’ils soient confirmés par courrier électronique, un sms ou un autre mécanisme. <a id="build"></a>Dans les sections ci-dessous, nous allons activer la confirmation de courrier électronique et modifier le code pour empêcher les utilisateurs qui viennent d’être inscrits de se connecter jusqu'à ce que leur courrier électronique ait été confirmé.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Se connecter à SendGrid

Bien que ce didacticiel montre uniquement comment ajouter la notification par courrier électronique via [SendGrid](http://sendgrid.com/), vous pouvez envoyer un e-mail à l’aide de SMTP et d'autres mécanismes (consultez [les ressources additionnelles](#addRes)).

1. Dans la Console du Gestionnaire de Packages, entrez ce qui suit la commande suivante : 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Accédez à la [page d’inscription d'Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) et inscrivez-vous à un compte SendGrid gratuit. Configurer SendGrid en ajoutant du code semblable au suivant dans *App_Start/IdentityConfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Vous devrez ajouter les instructions suivantes :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Pour simplifier cet exemple, nous allons stocker les paramètres d’application dans le fichier *web.config* :

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Sécurité - Ne jamais stocker des données sensibles dans votre code source. Le compte et les informations d’identification sont stockées dans l’appSetting. Sur Azure, vous pouvez stocker en toute sécurité ces valeurs sur l'onglet **[Configurer](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** dans le portail Azure. Consultez [les meilleures pratiques pour le déploiement des mots de passe et autres données sensibles sur ASP.NET et Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>Activer la confirmation par courrier électronique dans le contrôleur de compte

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Vérifiez que le fichier *Views\Account\ConfirmEmail.cshtml* possède la syntaxe razor correcte. (Le caractère @ dans la première ligne peut être manquant.)

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Exécutez l’application et cliquez sur le lien Register. Une fois que vous envoyez le formulaire d’inscription, vous êtes connecté.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Vérifiez votre compte de messagerie, puis cliquez sur le lien pour confirmer votre adresse de messagerie.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Exigez une confirmation par courrier électronique avant de se connecter

Actuellement une fois qu’un utilisateur termine le formulaire d’inscription, il est connecté. En règle générale, vous souhaitez confirmer leur adresse de messagerie avant de les enregistrer. Dans la section ci-dessous, nous allons modifier le code pour demander aux nouveaux utilisateurs un message électronique de confirmation avant d’être connecté (authentifié). Mettez à jour la méthode `HttpPost Register` avec les modifications en surbrillance suivantes :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

En supprimant la méthode `SignInAsync`, l’utilisateur ne sera pas authentifié lors de l’inscription. La ligne `TempData["ViewBagLink"] = callbackUrl;` peut être utilisée pour [déboguer l’application](#dbg) et tester l’inscription sans envoyer de courrier électronique. `ViewBag.Message` permet d’afficher les instructions de confirmation. L'[exemple à télécharger](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contient du code pour tester l’e-mail de confirmation sans configurer la messagerie et peut également être utilisé pour déboguer l’application.

Créez un `Views\Shared\Info.cshtml` et ajoutez le code razor suivant :

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Ajoutez l'[attribut Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) à la méthode d’action `Contact` du contrôleur Home. Vous pouvez cliquer sur le lien **Contact** pour vérifier que les utilisateurs anonymes n’ont pas accès et les utilisateurs authentifiés ont accès.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Vous devez également mettre à jour la méthode d’action `HttpPost Login` :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Mettez à jour la vue *Views\Shared\Error.cshtml* pour afficher le message d’erreur :

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Supprimez tous les comptes dans la table **AspNetUsers** qui contient l’alias de messagerie électronique que vous souhaitez tester. Exécutez l’application et vérifiez que vous ne pouvez pas vous connecter avant d’avoir confirmé votre adresse de messagerie. Après avoir confirmé votre adresse de messagerie, cliquez sur le **Contact** lien.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Récupération/réinitialisation de mot de passe

Supprimez les caractères de commentaire de la `HttpPost ForgotPassword` méthode d’action dans le contrôleur de compte :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Supprimez les caractères de commentaire de la méthode `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) dans le fichier de vue razor *Views\Account\Login.cshtml* :

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

La page de connexion affiche maintenant un lien pour réinitialiser le mot de passe.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Renvoyer le lien de confirmation par courrier électronique

Une fois qu’un utilisateur crée un nouveau compte local, on envoie par courrier électronique un lien de confirmation qu’il doit utiliser avant de se connecter. Si l’utilisateur supprime accidentellement le message de confirmation, ou si le message électronique n’arrive jamais, ils doivent envoyer à nouveau le lien de confirmation. Les modifications de code suivants montrent comment activer cette option.

Ajoutez la méthode d’assistance suivante en bas du fichier *Controllers\AccountController.cs* :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Mettez à jour la méthode Register pour utiliser la nouvelle méthode :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Mettez à jour la méthode de connexion pour renvoyer le mot de passe si le compte d’utilisateur n’a pas été confirmé :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Combiner des comptes de connexion de réseaux sociaux et local

Vous pouvez combiner les comptes locaux et de réseaux sociaux en cliquant sur le lien de votre messagerie. Dans l’ordre suivant  **RickAndMSFT@gmail.com**  est tout d’abord créé en tant qu’une connexion locale, mais vous pouvez créer le compte en tant que login social d'abord, puis ajouter une connexion locale.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Cliquez sur le lien **Gérer**. Notez la valeur 0 login externe (social) associé à ce compte.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Cliquez sur le lien vers un autre service de login et acceptez les demandes d’application. Les deux comptes ont été combinés, vous pourrez vous connecter avec l'un ou l'autre compte. Vous pourriez vouloir que vos utilisateurs ajoutent des comptes locaux au cas où leur service d’authentification de réseau social est arrêté, ou plus probable qu’ils ont perdu l’accès à leur compte social.

Dans l’image suivante, Tom est un login social (comme l'indique le **Logins externes : 1** affiché sur la page).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Cliquez sur **Choisir un mot de passe** vous permet d’ajouter un login local avec le même compte.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>E-mail de confirmation de manière plus approfondie

Mon didacticiel [Confirmation du compte et récupération de mot de passe avec ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) aborde ce sujet avec plus de détails.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Débogage de l’application

Si vous n’obtenez pas un message électronique contenant le lien :

- Vérifiez votre dossier courrier indésirable.
- Connectez-vous à votre compte SendGrid, puis cliquez sur le [lien de l’activité de messagerie](https://sendgrid.com/logs/index).

Pour tester le lien de vérification sans courrier électronique, vous devez télécharger l'[exemple terminé](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Le lien de confirmation et les codes de confirmation seront afficher sur la page.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Des liens vers les ressources recommandées sur ASP.NET Identity](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Confirmation de compte et récupération de mot de passe avec ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) des informations plus détaillées sur la confirmation de compte et de récupération du mot de passe.
- [Application MVC 5 avec Facebook, Twitter, LinkedIn et authentification Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) ce didacticiel vous montre comment écrire une application ASP.NET MVC 5 avec autorisation Facebook et Google OAuth 2. Il montre également comment ajouter des données supplémentaires à la base de données d’identité.
- [Déployer une application sécurisée ASP.NET MVC avec l’appartenance, OAuth et base de données SQL vers Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Ce didacticiel ajoute le déploiement d’Azure, la sécurisation de votre application avec les rôles, l’utilisation de l’API d’appartenance pour ajouter des utilisateurs et des rôles et des fonctionnalités de sécurité supplémentaires.
- [Création d’une application pour Google OAuth 2 et la connexion de l’application au projet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Création de l’application de Facebook et connexion de l’application au projet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configuration de SSL dans le projet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
