---
title: "Activation de l’authentification à l’aide de Facebook, Google et d’autres fournisseurs externes"
author: rick-anderson
description: "Ce didacticiel montre comment générer une application ASP.NET Core 2.x à l’aide d’OAuth 2.0 avec des fournisseurs d’authentification externes."
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/social/index
ms.openlocfilehash: 7d03998c82bf13976ec6157acb5c56c28e5c0d52
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a><span data-ttu-id="c67fc-103">Activation de l’authentification à l’aide de Facebook, Google et d’autres fournisseurs externes</span><span class="sxs-lookup"><span data-stu-id="c67fc-103">Enabling authentication using Facebook, Google, and other external providers</span></span>

<a name="security-authentication-social-logins"></a>

<span data-ttu-id="c67fc-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c67fc-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c67fc-105">Ce didacticiel montre comment créer une application ASP.NET Core 2.x qui permet aux utilisateurs de se connecter à l’aide d’OAuth 2.0 avec des informations d’identification provenant de fournisseurs d’authentification externes.</span><span class="sxs-lookup"><span data-stu-id="c67fc-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="c67fc-106">Les fournisseurs [Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md) et [Microsoft](microsoft-logins.md) sont traités dans les sections qui suivent.</span><span class="sxs-lookup"><span data-stu-id="c67fc-106">[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), and [Microsoft](microsoft-logins.md) providers are covered in the following sections.</span></span> <span data-ttu-id="c67fc-107">D’autres fournisseurs sont disponibles dans des packages tiers, comme [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) et [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="c67fc-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Icônes de réseau social pour Facebook, Twitter, Google plus et Windows](index/_static/social.png)

<span data-ttu-id="c67fc-109">Permettre aux utilisateurs de se connecter avec leurs informations d’identification existantes est pratique pour eux et transfère une grande partie de la complexité de la gestion du processus de connexion sur un tiers.</span><span class="sxs-lookup"><span data-stu-id="c67fc-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="c67fc-110">Pour obtenir des exemples de la façon dont les connexions des réseaux sociaux peuvent amener du trafic et des conversions de clients, consultez les études de cas par [Facebook](https://www.facebook.com/unsupportedbrowser) et [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="c67fc-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="c67fc-111">Remarque : Les packages présentés font abstraction d’une grande partie de la complexité du flux d’authentification OAuth, mais en comprendre les détails peut devenir nécessaire pour la résolution des problèmes.</span><span class="sxs-lookup"><span data-stu-id="c67fc-111">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="c67fc-112">De nombreuses ressources sont disponibles. Vous pouvez par exemple consulter [Introduction à OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) ou [Présentation d’OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span><span class="sxs-lookup"><span data-stu-id="c67fc-112">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="c67fc-113">Certains problèmes peuvent être résolus en examinant le [code source d’ASP.NET Core pour les packages de fournisseur](https://github.com/aspnet/Security/tree/dev/src).</span><span class="sxs-lookup"><span data-stu-id="c67fc-113">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="c67fc-114">Créer un projet ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c67fc-114">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="c67fc-115">Dans Visual Studio 2017, créez un projet à partir de la page de démarrage, ou via **Fichier > Nouveau > Projet**.</span><span class="sxs-lookup"><span data-stu-id="c67fc-115">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="c67fc-116">Sélectionnez le modèle **Application web ASP.NET Core** disponible dans la catégorie **Visual C# > .NET Core** :</span><span class="sxs-lookup"><span data-stu-id="c67fc-116">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![Boîte de dialogue Nouveau projet](index/_static/new-project.png)

* <span data-ttu-id="c67fc-118">Appuyez sur **Application Web** et vérifiez que **Authentification** est défini sur **Comptes d’utilisateur individuels** :</span><span class="sxs-lookup"><span data-stu-id="c67fc-118">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Boîte de dialogue Nouvelle application web](index/_static/select-project.png)

<span data-ttu-id="c67fc-120">Remarque : Ce didacticiel s’applique à la version ASP.NET Core 2.0 SDK, qui peut être sélectionnée en haut de l’Assistant.</span><span class="sxs-lookup"><span data-stu-id="c67fc-120">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="c67fc-121">Appliquer des migrations</span><span class="sxs-lookup"><span data-stu-id="c67fc-121">Apply migrations</span></span>

* <span data-ttu-id="c67fc-122">Exécutez l’application et sélectionnez le lien **Se connecter**.</span><span class="sxs-lookup"><span data-stu-id="c67fc-122">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="c67fc-123">Sélectionnez le lien **S’inscrire comme nouvel utilisateur**.</span><span class="sxs-lookup"><span data-stu-id="c67fc-123">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="c67fc-124">Entrez l’adresse e-mail et le mot de passe du nouveau compte, puis sélectionnez **S’inscrire**.</span><span class="sxs-lookup"><span data-stu-id="c67fc-124">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="c67fc-125">Suivez les instructions pour appliquer des migrations.</span><span class="sxs-lookup"><span data-stu-id="c67fc-125">Follow the instructions to apply migrations.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="c67fc-126">Exiger SSL</span><span class="sxs-lookup"><span data-stu-id="c67fc-126">Require SSL</span></span>

<span data-ttu-id="c67fc-127">OAuth 2.0 nécessite l’utilisation de SSL pour l’authentification avec le protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c67fc-127">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="c67fc-128">Remarque : Les projets créés à partir des modèles de projet **Application web** ou **API web** pour ASP.NET Core 2.x sont configurés automatiquement pour activer SSL, et pour se lancer avec des URL HTTPS si l’option **Comptes d’utilisateur individuels** a été sélectionnée dans la boîte de dialogue **Modifier l’authentification** dans l’Assistant Projet, comme indiqué ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="c67fc-128">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="c67fc-129">Exigez SSL sur votre site en suivant les étapes de la rubrique [Exiger SSL dans une application ASP.NET Core](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="c67fc-129">Require SSL on your site by following the steps in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="c67fc-130">Utilisez SecretManager pour stocker les jetons affectés par les fournisseurs de connexion</span><span class="sxs-lookup"><span data-stu-id="c67fc-130">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="c67fc-131">Les fournisseurs de connexion de réseaux sociaux affectent des jetons **ID d’application** et **Secret de l’application** lors du processus d’inscription (le nom exact varie selon le fournisseur).</span><span class="sxs-lookup"><span data-stu-id="c67fc-131">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process (exact naming varies by provider).</span></span>

<span data-ttu-id="c67fc-132">Ces valeurs sont en réalité le *nom d’utilisateur* et le *mot de passe* que votre application utilise pour accéder à leur API, et constituent les « secrets » qui peuvent être liés à la configuration de votre application à l’aide de **Secret Manager**, au lieu de les stocker directement dans des fichiers de configuration ou de les coder en dur.</span><span class="sxs-lookup"><span data-stu-id="c67fc-132">These values are effectively the *user name* and *password* your application uses to access their API, and constitute the "secrets" that can be linked to your application configuration with the help of **Secret Manager** instead of storing them in configuration files directly or hard-coding them.</span></span>

<span data-ttu-id="c67fc-133">Suivez les étapes de la rubrique [Stockage sécurisé des secrets d’application lors du développement dans ASP.NET Core](xref:security/app-secrets) pour stocker les jetons affectés par chaque fournisseur de connexion ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="c67fc-133">Follow the steps in [Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets) topic so that you can store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="c67fc-134">Configurer les fournisseurs de connexion nécessaires à votre application</span><span class="sxs-lookup"><span data-stu-id="c67fc-134">Setup login providers required by your application</span></span>

<span data-ttu-id="c67fc-135">Utilisez les rubriques suivantes pour configurer votre application pour utiliser ces différents fournisseurs :</span><span class="sxs-lookup"><span data-stu-id="c67fc-135">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="c67fc-136">Instructions pour [Facebook](facebook-logins.md)</span><span class="sxs-lookup"><span data-stu-id="c67fc-136">[Facebook](facebook-logins.md) instructions</span></span>
* <span data-ttu-id="c67fc-137">Instructions pour [Twitter](twitter-logins.md)</span><span class="sxs-lookup"><span data-stu-id="c67fc-137">[Twitter](twitter-logins.md) instructions</span></span>
* <span data-ttu-id="c67fc-138">Instructions pour [Google](google-logins.md)</span><span class="sxs-lookup"><span data-stu-id="c67fc-138">[Google](google-logins.md) instructions</span></span>
* <span data-ttu-id="c67fc-139">Instructions pour [Microsoft](microsoft-logins.md)</span><span class="sxs-lookup"><span data-stu-id="c67fc-139">[Microsoft](microsoft-logins.md) instructions</span></span>
* <span data-ttu-id="c67fc-140">Instructions pour les [autres fournisseurs](other-logins.md)</span><span class="sxs-lookup"><span data-stu-id="c67fc-140">[Other provider](other-logins.md) instructions</span></span>

## <a name="optionally-set-password"></a><span data-ttu-id="c67fc-141">Définition facultative d’un mot de passe</span><span class="sxs-lookup"><span data-stu-id="c67fc-141">Optionally set password</span></span>

<span data-ttu-id="c67fc-142">Quand vous vous inscrivez auprès d’un fournisseur de connexion externe, vous n’avez pas de mot de passe inscrit auprès de l’application.</span><span class="sxs-lookup"><span data-stu-id="c67fc-142">When you register with an external login provider, you do not have a password registered with the app.</span></span> <span data-ttu-id="c67fc-143">Ceci vous évite de devoir créer et mémoriser un mot de passe pour le site, mais vous rend aussi dépendant du fournisseur de connexion externe.</span><span class="sxs-lookup"><span data-stu-id="c67fc-143">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="c67fc-144">Si le fournisseur de connexion externe est indisponible, vous ne pouvez pas vous connecter au site web.</span><span class="sxs-lookup"><span data-stu-id="c67fc-144">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="c67fc-145">Pour créer un mot de passe et vous connecter à l’aide de l’e-mail que vous avez défini lors du processus de connexion avec des fournisseurs externes :</span><span class="sxs-lookup"><span data-stu-id="c67fc-145">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="c67fc-146">Appuyez sur le lien **Bonjour <email alias>** en haut à droite pour accéder à la vue **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="c67fc-146">Tap the **Hello <email alias>** link at the top right corner to navigate to the **Manage** view.</span></span>

![Vue Gérer de l’application web](index/_static/pass1a.png)

* <span data-ttu-id="c67fc-148">Appuyez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="c67fc-148">Tap **Create**</span></span>

![Page Définir votre mot de passe](index/_static/pass2a.png)

* <span data-ttu-id="c67fc-150">Définissez un mot de passe valide à utiliser pour vous connecter avec votre e-mail.</span><span class="sxs-lookup"><span data-stu-id="c67fc-150">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c67fc-151">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="c67fc-151">Next steps</span></span>

* <span data-ttu-id="c67fc-152">Cet article a présenté l’authentification externe et expliqué les prérequis nécessaires pour ajouter des connexions externes à votre application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c67fc-152">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="c67fc-153">Référencez les pages spécifiques au fournisseur pour configurer les connexions pour les fournisseurs nécessaires à votre application.</span><span class="sxs-lookup"><span data-stu-id="c67fc-153">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
