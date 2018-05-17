---
title: Programme d’installation de Facebook connexion externe dans ASP.NET Core
author: rick-anderson
description: Ce didacticiel illustre l’intégration de l’authentification d’utilisateur de compte Facebook dans une application ASP.NET Core existante.
manager: wpickett
ms.author: riande
ms.date: 08/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/facebook-logins
ms.openlocfilehash: a0b96f480aaa3941cf63b25780c5a1d9d4b2dbb0
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="9dc81-103">Programme d’installation de Facebook connexion externe dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9dc81-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="9dc81-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9dc81-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9dc81-105">Ce didacticiel vous montre comment permettre aux utilisateurs de se connecter avec leur compte Facebook à l’aide d’un exemple de projet ASP.NET Core 2.0 créé sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="9dc81-105">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="9dc81-106">L’authentification Facebook requiert le [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="9dc81-106">Facebook authentication requires the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package.</span></span> <span data-ttu-id="9dc81-107">Nous commençons par créer un ID d’application Facebook en suivant le [étapes officiels](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="9dc81-107">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="9dc81-108">Créer l’application en Facebook</span><span class="sxs-lookup"><span data-stu-id="9dc81-108">Create the app in Facebook</span></span>

*  <span data-ttu-id="9dc81-109">Accédez à la [aux développeurs de Facebook application](https://developers.facebook.com/apps/) page et vous connecter.</span><span class="sxs-lookup"><span data-stu-id="9dc81-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="9dc81-110">Si vous n’avez pas déjà un compte Facebook, utilisez le **souscrire à Facebook** sur la page de connexion pour créer un lien.</span><span class="sxs-lookup"><span data-stu-id="9dc81-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="9dc81-111">Appuyez sur la **ajouter une nouvelle application** bouton dans le coin supérieur droit pour créer un nouvel ID d’application.</span><span class="sxs-lookup"><span data-stu-id="9dc81-111">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Facebook pour le portail des développeurs ouvrir dans Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="9dc81-113">Remplissez le formulaire, puis appuyez sur la **ID d’application créer** bouton.</span><span class="sxs-lookup"><span data-stu-id="9dc81-113">Fill out the form and tap the **Create App ID** button.</span></span>

   ![Créer un formulaire de nouvel ID d’application](index/_static/FBNewAppId.png)

* <span data-ttu-id="9dc81-115">Sur le **sélectionner un produit** , cliquez sur **Set Up** sur la **connexion Facebook** carte.</span><span class="sxs-lookup"><span data-stu-id="9dc81-115">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

   ![Page de configuration de produit](index/_static/FBProductSetup.png)

* <span data-ttu-id="9dc81-117">Le **Quickstart** Assistant lancera avec **choisir une plate-forme** en tant que la première page.</span><span class="sxs-lookup"><span data-stu-id="9dc81-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="9dc81-118">Ignorer l’Assistant pour l’instant, en cliquant sur le **paramètres** lien dans le menu de gauche :</span><span class="sxs-lookup"><span data-stu-id="9dc81-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

   ![Démarrage rapide de Skip](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="9dc81-120">Vous voyez s’afficher la **les paramètres Client OAuth** page :</span><span class="sxs-lookup"><span data-stu-id="9dc81-120">You are presented with the **Client OAuth Settings** page:</span></span>

![Page de paramètres OAuth du client](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="9dc81-122">Entrez votre développement URI avec */signin-facebook* ajoutées dans le **URI de redirection OAuth valide** champ (par exemple : `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="9dc81-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="9dc81-123">L’authentification Facebook configurée plus loin dans ce didacticiel va gérer automatiquement les demandes à */signin-facebook* itinéraire pour implémenter le flux OAuth.</span><span class="sxs-lookup"><span data-stu-id="9dc81-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="9dc81-124">Cliquez sur **enregistrer les modifications**.</span><span class="sxs-lookup"><span data-stu-id="9dc81-124">Click **Save Changes**.</span></span>

* <span data-ttu-id="9dc81-125">Cliquez sur le **tableau de bord** lien dans le volet de navigation gauche.</span><span class="sxs-lookup"><span data-stu-id="9dc81-125">Click the **Dashboard** link in the left navigation.</span></span> 

    <span data-ttu-id="9dc81-126">Dans cette page, prenez note de votre `App ID` et votre `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="9dc81-126">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="9dc81-127">Vous allez ajouter à la fois dans votre application ASP.NET Core dans la section suivante :</span><span class="sxs-lookup"><span data-stu-id="9dc81-127">You will add both into your ASP.NET Core application in the next section:</span></span>

   ![Tableau de bord Facebook Developer](index/_static/FBDashboard.png)

* <span data-ttu-id="9dc81-129">Lorsque vous déployez le site vous devez revoir la **connexion Facebook** page Installation et inscrire un URI public.</span><span class="sxs-lookup"><span data-stu-id="9dc81-129">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="9dc81-130">Stocker les ID d’application Facebook et Secret d’application</span><span class="sxs-lookup"><span data-stu-id="9dc81-130">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="9dc81-131">Lier les paramètres sensibles tels que Facebook `App ID` et `App Secret` à votre configuration d’application à l’aide du [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="9dc81-131">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="9dc81-132">Pour les besoins de ce didacticiel, nommez les jetons `Authentication:Facebook:AppId` et `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="9dc81-132">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="9dc81-133">Exécutez les commandes suivantes pour stocker en toute sécurité `App ID` et `App Secret` à l’aide du Gestionnaire de Secret :</span><span class="sxs-lookup"><span data-stu-id="9dc81-133">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="9dc81-134">Configurer l’authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="9dc81-134">Configure Facebook Authentication</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9dc81-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9dc81-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="9dc81-136">Ajoutez le service de Facebook dans le `ConfigureServices` méthode dans le *Startup.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="9dc81-136">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9dc81-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9dc81-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="9dc81-138">Installer le [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span><span class="sxs-lookup"><span data-stu-id="9dc81-138">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="9dc81-139">Pour installer ce package avec Visual Studio 2017, cliquez sur le projet et sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9dc81-139">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="9dc81-140">Pour installer avec l’interface CLI de .NET Core, exécutez le code suivant dans votre répertoire de projet :</span><span class="sxs-lookup"><span data-stu-id="9dc81-140">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="9dc81-141">Ajouter l’intergiciel (middleware) Facebook dans le `Configure` méthode dans *Startup.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="9dc81-141">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="9dc81-142">Consultez le [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) référence des API pour plus d’informations sur les options de configuration prises en charge par l’authentification Facebook.</span><span class="sxs-lookup"><span data-stu-id="9dc81-142">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="9dc81-143">Options de configuration peuvent être utilisées pour :</span><span class="sxs-lookup"><span data-stu-id="9dc81-143">Configuration options can be used to:</span></span>

* <span data-ttu-id="9dc81-144">Demander des informations différentes sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9dc81-144">Request different information about the user.</span></span>
* <span data-ttu-id="9dc81-145">Ajoutez des arguments de chaîne de requête pour personnaliser l’expérience de connexion.</span><span class="sxs-lookup"><span data-stu-id="9dc81-145">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="9dc81-146">Se connecter avec Facebook</span><span class="sxs-lookup"><span data-stu-id="9dc81-146">Sign in with Facebook</span></span>

<span data-ttu-id="9dc81-147">Exécutez votre application et cliquez sur **connecter**.</span><span class="sxs-lookup"><span data-stu-id="9dc81-147">Run your application and click **Log in**.</span></span> <span data-ttu-id="9dc81-148">Vous voyez une option pour vous connecter avec Facebook.</span><span class="sxs-lookup"><span data-stu-id="9dc81-148">You see an option to sign in with Facebook.</span></span>

![Application Web : utilisateur non authentifié](index/_static/DoneFacebook.png)

<span data-ttu-id="9dc81-150">Lorsque vous cliquez sur **Facebook**, vous êtes redirigé vers Facebook pour l’authentification :</span><span class="sxs-lookup"><span data-stu-id="9dc81-150">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Page d’authentification Facebook](index/_static/FBLogin.png)

<span data-ttu-id="9dc81-152">Les demandes d’authentification de Facebook publique profil et adresse de messagerie par défaut :</span><span class="sxs-lookup"><span data-stu-id="9dc81-152">Facebook authentication requests public profile and email address by default:</span></span>

![Page d’authentification Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="9dc81-154">Une fois que vous entrez vos informations d’identification de Facebook, vous êtes redirigé vers votre site où vous pouvez définir votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="9dc81-154">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="9dc81-155">Vous êtes désormais connecté à l’aide de vos informations d’identification de Facebook :</span><span class="sxs-lookup"><span data-stu-id="9dc81-155">You are now logged in using your Facebook credentials:</span></span>

![Application Web : utilisateur authentifié](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="9dc81-157">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="9dc81-157">Troubleshooting</span></span>

* <span data-ttu-id="9dc81-158">**ASP.NET Core 2.x uniquement :** si identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, une tentative d’authentification entraîne *ArgumentException : l’option 'SignInScheme' doit être fournie*.</span><span class="sxs-lookup"><span data-stu-id="9dc81-158">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="9dc81-159">Le modèle de projet utilisé dans ce didacticiel permet de s’assurer que cette opération est effectuée.</span><span class="sxs-lookup"><span data-stu-id="9dc81-159">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="9dc81-160">Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtenez *une opération de base de données a échoué lors du traitement de la demande* erreur.</span><span class="sxs-lookup"><span data-stu-id="9dc81-160">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="9dc81-161">Appuyez sur **s’appliquent les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.</span><span class="sxs-lookup"><span data-stu-id="9dc81-161">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9dc81-162">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="9dc81-162">Next steps</span></span>

* <span data-ttu-id="9dc81-163">Cet article a montré comment vous pouvez vous authentifier avec Facebook.</span><span class="sxs-lookup"><span data-stu-id="9dc81-163">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="9dc81-164">Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="9dc81-164">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="9dc81-165">Une fois que vous publiez votre site web à l’application web Azure, vous devez réinitialiser le `AppSecret` dans le portail des développeurs Facebook.</span><span class="sxs-lookup"><span data-stu-id="9dc81-165">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="9dc81-166">Définir le `Authentication:Facebook:AppId` et `Authentication:Facebook:AppSecret` en tant que paramètres de l’application dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="9dc81-166">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="9dc81-167">Le système de configuration est configuré pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="9dc81-167">The configuration system is set up to read keys from environment variables.</span></span>
