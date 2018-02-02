---
title: "Confirmation du compte et récupération de mot de passe dans ASP.NET Core"
author: rick-anderson
description: "Montre comment créer une application ASP.NET Core à la réinitialisation par courrier électronique de confirmation et le mot de passe."
manager: wpickett
ms.author: riande
ms.date: 12/1/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: 8aeb04f772fa687706bd8080b4306ff0040a159f
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="ab6f5-103">Confirmation du compte et récupération de mot de passe dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab6f5-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="ab6f5-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="ab6f5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="ab6f5-105">Ce didacticiel vous montre comment créer une application ASP.NET Core à la réinitialisation par courrier électronique de confirmation et le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="ab6f5-106">Créer un projet ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab6f5-106">Create a New ASP.NET Core Project</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab6f5-107">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ab6f5-107">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ab6f5-108">Cette étape s’applique à Visual Studio sous Windows.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-108">This step applies to Visual Studio on Windows.</span></span> <span data-ttu-id="ab6f5-109">Consultez la section suivante pour obtenir des instructions de l’interface CLI.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-109">See the next section for CLI instructions.</span></span>

<span data-ttu-id="ab6f5-110">Le didacticiel requiert Visual Studio 2017 (Preview) 2 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-110">The tutorial requires Visual Studio 2017 Preview 2 or later.</span></span>

* <span data-ttu-id="ab6f5-111">Dans Visual Studio, créez un nouveau projet d’Application Web.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-111">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="ab6f5-112">Sélectionnez **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-112">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="ab6f5-113">L’illustration suivante show **.NET Core** sélectionné, mais vous pouvez sélectionner **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-113">The following image show **.NET Core** selected, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="ab6f5-114">Sélectionnez **modifier l’authentification** et la valeur **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-114">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="ab6f5-115">Conservez la valeur par défaut **dans l’application des comptes d’utilisateurs de magasin**.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-115">Keep the default **Store user accounts in-app**.</span></span>

![Nouvelle boîte de dialogue projet indiquant « Radio de comptes d’utilisateur individuels » sélectionnée](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab6f5-117">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ab6f5-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ab6f5-118">Le didacticiel requiert Visual Studio 2017 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-118">The tutorial requires Visual Studio 2017 or later.</span></span>

* <span data-ttu-id="ab6f5-119">Dans Visual Studio, créez un nouveau projet d’Application Web.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-119">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="ab6f5-120">Sélectionnez **modifier l’authentification** et la valeur **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-120">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>

![Nouvelle boîte de dialogue projet indiquant « Radio de comptes d’utilisateur individuels » sélectionnée](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a><span data-ttu-id="ab6f5-122">Création du projet .NET core CLI pour macOS et Linux</span><span class="sxs-lookup"><span data-stu-id="ab6f5-122">.NET Core CLI project creation for macOS and Linux</span></span>

<span data-ttu-id="ab6f5-123">Si vous utilisez l’interface CLI ou SQLite, exécutez la commande suivante dans une fenêtre de commande :</span><span class="sxs-lookup"><span data-stu-id="ab6f5-123">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="ab6f5-124">`--auth Individual`Spécifie le modèle de comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-124">`--auth Individual` specifies the Individual User Accounts template.</span></span>
* <span data-ttu-id="ab6f5-125">Sur Windows, ajoutez le `-uld` option.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-125">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="ab6f5-126">Le `-uld` option permet de créer une chaîne de connexion de base de données locale plutôt que dans une base de données SQLite.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-126">The `-uld` option creates a LocalDB connection string rather than a SQLite DB.</span></span>
* <span data-ttu-id="ab6f5-127">Exécutez `new mvc --help` pour obtenir de l’aide sur cette commande.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-127">Run `new mvc --help` to get help on this command.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="ab6f5-128">Nouvelle inscription de l’utilisateur de test</span><span class="sxs-lookup"><span data-stu-id="ab6f5-128">Test new user registration</span></span>

<span data-ttu-id="ab6f5-129">Exécuter l’application, sélectionnez le **inscrire** lier et inscrire un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="ab6f5-130">Suivez les instructions pour exécuter les migrations d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-130">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="ab6f5-131">À ce stade, la seule validation de l’adresse de messagerie est avec le [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribut.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-131">At this  point, the only validation on the email is with the [[EmailAddress]](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="ab6f5-132">Une fois que vous soumettez l’inscription, vous êtes connecté à l’application.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-132">After you submit the registration, you are logged into the app.</span></span> <span data-ttu-id="ab6f5-133">Plus loin dans ce didacticiel, nous allons modifier cela pour les nouveaux utilisateurs ne peuvent pas se connecter jusqu'à ce que leur courrier électronique a été validée.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-133">Later in the tutorial, we'll change this so new users cannot log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="ab6f5-134">Afficher la base de données d’identité</span><span class="sxs-lookup"><span data-stu-id="ab6f5-134">View the Identity database</span></span>

# <a name="sql-servertabsql-server"></a>[<span data-ttu-id="ab6f5-135">SQL Server</span><span class="sxs-lookup"><span data-stu-id="ab6f5-135">SQL Server</span></span>](#tab/sql-server)

* <span data-ttu-id="ab6f5-136">À partir de la **vue** menu, sélectionnez **l’Explorateur d’objets SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="ab6f5-136">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span> 
* <span data-ttu-id="ab6f5-137">Accédez à **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-137">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="ab6f5-138">Avec le bouton droit sur **dbo. AspNetUsers** > **afficher les données**:</span><span class="sxs-lookup"><span data-stu-id="ab6f5-138">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu contextuel sur la table AspNetUsers dans l’Explorateur d’objets SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="ab6f5-140">Remarque la `EmailConfirmed` champ est `False`.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-140">Note the `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="ab6f5-141">Vous pouvez souhaiter réutiliser ce courrier électronique à l’étape suivante lors de l’application envoie un message électronique de confirmation.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-141">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="ab6f5-142">Avec le bouton droit sur la ligne, puis sélectionnez **supprimer**.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-142">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="ab6f5-143">Suppression de l’adresse de messagerie alias maintenant facilitera dans les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-143">Deleting the email alias now will make it easier in the following steps.</span></span>

# <a name="sqlitetabsqlite"></a>[<span data-ttu-id="ab6f5-144">SQLite</span><span class="sxs-lookup"><span data-stu-id="ab6f5-144">SQLite</span></span>](#tab/sqlite)

<span data-ttu-id="ab6f5-145">Consultez [utilisation de SQLite dans un projet ASP.NET MVC de base](xref:tutorials/first-mvc-app-xplat/working-with-sql) pour obtenir des instructions sur l’affichage de la base de données SQLite.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-145">See [Working with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite DB.</span></span> 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a><span data-ttu-id="ab6f5-146">Exiger le protocole SSL et le programme d’installation d’IIS Express pour le protocole SSL</span><span class="sxs-lookup"><span data-stu-id="ab6f5-146">Require SSL and setup IIS Express for SSL</span></span>

<span data-ttu-id="ab6f5-147">Consultez [en appliquant SSL](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="ab6f5-147">See [Enforcing SSL](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="ab6f5-148">Demander confirmation de courrier électronique</span><span class="sxs-lookup"><span data-stu-id="ab6f5-148">Require email confirmation</span></span>

<span data-ttu-id="ab6f5-149">Il est recommandé de confirmer l’adresse de messagerie d’un nouvel enregistrement d’utilisateur pour vérifier qu’ils empruntez pas une autre personne (autrement dit, ils n’ont pas inscrit avec par quelqu'un d’autre adresse de messagerie).</span><span class="sxs-lookup"><span data-stu-id="ab6f5-149">It's a best practice to confirm the email of a new user registration to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="ab6f5-150">Supposons que vous aviez un forum de discussion, et vous souhaitez empêcher «yli@example.com« lors de l’inscription en tant que »nolivetto@contoso.com. »</span><span class="sxs-lookup"><span data-stu-id="ab6f5-150">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="ab6f5-151">Sans la confirmation par courrier électronique, «nolivetto@contoso.com« Impossible d’obtenir le courrier indésirable à partir de votre application.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-151">Without email confirmation, "nolivetto@contoso.com" could get unwanted email from your app.</span></span> <span data-ttu-id="ab6f5-152">Supposons que l’utilisateur inscrit par inadvertance en tant que «ylo@example.com» et vous n’aviez pas remarqué la faute d’orthographe de « yli », ils semblent ne pas pouvoir à utiliser la récupération de mot de passe, car l’application n’a pas leur courrier électronique correct.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-152">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli," they wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="ab6f5-153">E-mail de confirmation offre uniquement une protection limitée de robots et ne fournit la protection à partir des expéditeurs déterminés ayant plusieurs alias de messagerie de travail qu’ils peuvent utiliser pour enregistrer.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-153">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers who have many working email aliases they can use to register.</span></span>

<span data-ttu-id="ab6f5-154">En règle générale, vous souhaitez empêcher les nouveaux utilisateurs à partir de la validation des données à votre site web avant d’avoir un message électronique de confirmation.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-154">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span> 

<span data-ttu-id="ab6f5-155">Mise à jour `ConfigureServices` pour exiger un message électronique de confirmation :</span><span class="sxs-lookup"><span data-stu-id="ab6f5-155">Update `ConfigureServices` to require a confirmed email:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab6f5-156">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ab6f5-156">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab6f5-157">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ab6f5-157">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
<span data-ttu-id="ab6f5-158">La ligne précédente empêche les utilisateurs enregistrés d’étant connecté en tant que leur adresse de messagerie est confirmée.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-158">The preceding line prevents registered users from being logged in until their email is confirmed.</span></span> <span data-ttu-id="ab6f5-159">Toutefois, cette ligne n’empêche les nouveaux utilisateurs ne soient enregistrées dans après leur inscrivent.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-159">However, that line doesn't prevent new users from being logged in after they register.</span></span> <span data-ttu-id="ab6f5-160">Le code par défaut se connecte un utilisateur après leur inscrivent.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-160">The default code logs in a user after they register.</span></span> <span data-ttu-id="ab6f5-161">Une fois qu’ils se déconnecter, ils ne pourront pas se connecter à nouveau jusqu'à ce qu’ils s’inscrivent.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-161">Once they log out, they won't be able to log in again until they register.</span></span> <span data-ttu-id="ab6f5-162">Plus loin dans ce didacticiel, nous allons modifier l’utilisateur de code donc nouvellement inscrit sont **pas** connecté.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-162">Later in the tutorial we'll change the code so newly registered user are **not** logged in.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="ab6f5-163">Configurer le fournisseur de messagerie</span><span class="sxs-lookup"><span data-stu-id="ab6f5-163">Configure email provider</span></span>

<span data-ttu-id="ab6f5-164">Dans ce didacticiel, SendGrid est utilisé pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="ab6f5-165">Vous avez besoin d’un compte SendGrid et une clé pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="ab6f5-166">Vous pouvez utiliser d’autres fournisseurs de messagerie.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-166">You can use other email providers.</span></span> <span data-ttu-id="ab6f5-167">ASP.NET Core 2.x inclut `System.Net.Mail`, qui vous permet d’envoyer par courrier électronique à partir de votre application.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="ab6f5-168">Nous vous recommandons de qu'utiliser SendGrid ou un autre service de messagerie pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-168">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="ab6f5-169">SMTP est difficile de sécuriser et correctement configuré.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-169">SMTP is difficult to to secure and set up correctly.</span></span>

<span data-ttu-id="ab6f5-170">Le [modèle d’Options](xref:fundamentals/configuration/options) est utilisé pour accéder aux paramètres de compte et une clé utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-170">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="ab6f5-171">Pour plus d’informations, consultez [configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="ab6f5-171">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="ab6f5-172">Créez une classe pour extraire la clé de sécuriser la messagerie électronique.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-172">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="ab6f5-173">Pour cet exemple, le `AuthMessageSenderOptions` classe est créée dans le *Services/AuthMessageSenderOptions.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-173">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="ab6f5-174">Définir le `SendGridUser` et `SendGridKey` avec la [outil Gestionnaire de secret](../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="ab6f5-174">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](../app-secrets.md).</span></span> <span data-ttu-id="ab6f5-175">Exemple :</span><span class="sxs-lookup"><span data-stu-id="ab6f5-175">For example:</span></span>

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="ab6f5-176">Sous Windows, Gestionnaire de secret principal stocke vos paires de clés/valeur dans un *secrets.json* fichier dans le répertoire %APPDATA%/Microsoft/UserSecrets/ < WebAppName-userSecretsId >.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-176">On Windows, Secret Manager stores your keys/value pairs in a *secrets.json* file in the %APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId> directory.</span></span>

<span data-ttu-id="ab6f5-177">Le contenu de la *secrets.json* fichier ne sont pas chiffrés.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-177">The contents of the *secrets.json* file are not encrypted.</span></span> <span data-ttu-id="ab6f5-178">Le *secrets.json* fichier est présenté ci-dessous (le `SendGridKey` valeur a été supprimée.)</span><span class="sxs-lookup"><span data-stu-id="ab6f5-178">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="ab6f5-179">Configurer le démarrage pour utiliser AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="ab6f5-179">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="ab6f5-180">Ajouter `AuthMessageSenderOptions` au conteneur de service à la fin de la `ConfigureServices` méthode dans le *Startup.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="ab6f5-180">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab6f5-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ab6f5-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab6f5-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ab6f5-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="ab6f5-183">Configurer la classe AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="ab6f5-183">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="ab6f5-184">Ce didacticiel montre comment ajouter des notifications par courrier électronique via [SendGrid](https://sendgrid.com/), mais vous pouvez envoyer par courrier électronique à l’aide de SMTP et autres mécanismes.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-184">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

* <span data-ttu-id="ab6f5-185">Installer le `SendGrid` package NuGet.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-185">Install the `SendGrid` NuGet package.</span></span> <span data-ttu-id="ab6f5-186">À partir de la Console du Gestionnaire de Package, entrez ce qui suit la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="ab6f5-186">From the Package Manager Console,  enter the following the following command:</span></span>

  `Install-Package SendGrid`

* <span data-ttu-id="ab6f5-187">Consultez [prise en main SendGrid gratuitement](https://sendgrid.com/free/) s’inscrire pour un compte SendGrid gratuit.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-187">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="ab6f5-188">Configurer SendGrid</span><span class="sxs-lookup"><span data-stu-id="ab6f5-188">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab6f5-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ab6f5-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="ab6f5-190">Ajoutez le code dans *Services/EmailSender.cs* similaire à ce qui suit pour configurer SendGrid :</span><span class="sxs-lookup"><span data-stu-id="ab6f5-190">Add code in *Services/EmailSender.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab6f5-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ab6f5-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
* <span data-ttu-id="ab6f5-192">Ajoutez le code dans *Services/MessageServices.cs* similaire à ce qui suit pour configurer SendGrid :</span><span class="sxs-lookup"><span data-stu-id="ab6f5-192">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="ab6f5-193">Activer la récupération de confirmation et le mot de passe du compte</span><span class="sxs-lookup"><span data-stu-id="ab6f5-193">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="ab6f5-194">Le modèle a le code pour la récupération de confirmation et le mot de passe du compte.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-194">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="ab6f5-195">Rechercher les `[HttpPost] Register` méthode dans le *AccountController.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-195">Find the `[HttpPost] Register` method in the  *AccountController.cs* file.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab6f5-196">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ab6f5-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ab6f5-197">Interdire nouvellement inscrit qui est automatiquement connecté en supprimant la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="ab6f5-197">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="ab6f5-198">La méthode complète s’affiche avec la ligne modifiée mis en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="ab6f5-198">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]

<span data-ttu-id="ab6f5-199">Remarque : Le code précédent échoue si vous implémentez `IEmailSender` et envoyez un courrier électronique en texte brut.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-199">Note: The previous code will fail if you implement `IEmailSender` and send a plain text email.</span></span> <span data-ttu-id="ab6f5-200">Consultez [ce problème](https://github.com/aspnet/Home/issues/2152) pour plus d’informations et une solution de contournement.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-200">See [this issue](https://github.com/aspnet/Home/issues/2152) for more information and a workaround.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab6f5-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ab6f5-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ab6f5-202">Ne pas commenter le code pour activer la confirmation du compte.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-202">Uncomment the code to enable account confirmation.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="ab6f5-203">Remarque : Nous avons également empêche un utilisateur qui vient d’être inscrit qui est automatiquement connecté en supprimant la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="ab6f5-203">Note: We're also preventing a newly-registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="ab6f5-204">Activer la récupération de mot de passe par le code de commentaire de la `ForgotPassword` action dans le *Controllers/AccountController.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-204">Enable password recovery by uncommenting the code in the `ForgotPassword` action in the *Controllers/AccountController.cs* file.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="ab6f5-205">Supprimez les commentaires de l’élément form *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-205">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="ab6f5-206">Vous souhaiterez peut-être supprimer le `<p> For more information on how to enable reset password ... </p>` élément qui contient un lien vers cet article.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-206">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element which contains a link to this article.</span></span>

[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="ab6f5-207">Inscrire, par courrier électronique de confirmation et réinitialiser le mot de passe</span><span class="sxs-lookup"><span data-stu-id="ab6f5-207">Register, confirm email, and reset password</span></span>

<span data-ttu-id="ab6f5-208">Exécuter l’application web et testez la confirmation du compte et le flux de récupération de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-208">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="ab6f5-209">Exécutez l’application et inscrire un nouvel utilisateur</span><span class="sxs-lookup"><span data-stu-id="ab6f5-209">Run the app and register a new user</span></span>

 ![Affichage du livre de comptes application Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="ab6f5-211">Vérifiez votre adresse de messagerie pour le lien de confirmation du compte.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-211">Check your email for the account confirmation link.</span></span> <span data-ttu-id="ab6f5-212">Consultez [déboguer messagerie](#debug) si vous n’obtenez pas le message électronique.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-212">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="ab6f5-213">Cliquez sur le lien pour confirmer votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-213">Click the link to confirm your email.</span></span>
* <span data-ttu-id="ab6f5-214">Connectez-vous avec votre adresse électronique et un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-214">Log in with your email and password.</span></span>
* <span data-ttu-id="ab6f5-215">Fermez la session.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-215">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="ab6f5-216">Afficher la page de gestion</span><span class="sxs-lookup"><span data-stu-id="ab6f5-216">View the manage page</span></span>

<span data-ttu-id="ab6f5-217">Sélectionnez votre nom d’utilisateur dans le navigateur : ![fenêtre de navigateur avec le nom d’utilisateur](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="ab6f5-217">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="ab6f5-218">Vous devrez peut-être développer la barre de navigation pour afficher le nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-218">You might need to expand the navbar to see user name.</span></span>

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab6f5-220">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ab6f5-220">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ab6f5-221">La page de gestion s’affiche avec le **profil** onglet sélectionné.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-221">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="ab6f5-222">Le **messagerie** affiche une case à cocher indiquant l’adresse de messagerie a été confirmée.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-222">The **Email** shows a check box indicating the email has been confirmed.</span></span> 

![page Gérer](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab6f5-224">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ab6f5-224">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ab6f5-225">Nous parlerons de cette page plus tard dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-225">We'll talk about this page later in the tutorial.</span></span>
<span data-ttu-id="ab6f5-226">![page Gérer](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="ab6f5-226">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="ab6f5-227">Réinitialisation de mot de passe de test</span><span class="sxs-lookup"><span data-stu-id="ab6f5-227">Test password reset</span></span>

* <span data-ttu-id="ab6f5-228">Si vous êtes connecté, sélectionnez **déconnexion**.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-228">If you're logged in, select **Logout**.</span></span>  
* <span data-ttu-id="ab6f5-229">Sélectionnez le **connecter** lien et sélectionnez le **vous avez oublié votre mot de passe ?** lien.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-229">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="ab6f5-230">Entrez l’e-mail que vous avez utilisé pour enregistrer le compte.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-230">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="ab6f5-231">Un e-mail avec un lien pour réinitialiser votre mot de passe sera envoyé.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-231">An email with a link to reset your password will be sent.</span></span> <span data-ttu-id="ab6f5-232">Vérifiez votre adresse de messagerie et cliquez sur le lien pour réinitialiser votre mot de passe.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-232">Check your email and click the link to reset your password.</span></span>  <span data-ttu-id="ab6f5-233">Une fois votre mot de passe a été réinitialisé avec succès, vous pouvez vous connecter avec votre adresse électronique et un nouveau mot de passe.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-233">After your password has been successfully reset, you can login with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="ab6f5-234">Déboguer le courrier électronique</span><span class="sxs-lookup"><span data-stu-id="ab6f5-234">Debug email</span></span>

<span data-ttu-id="ab6f5-235">Si vous ne parvenez le travail de la messagerie :</span><span class="sxs-lookup"><span data-stu-id="ab6f5-235">If you can't get email working:</span></span>

* <span data-ttu-id="ab6f5-236">Examinez le [activité de messagerie](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-236">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="ab6f5-237">Vérifiez votre dossier courrier indésirable.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-237">Check your spam folder.</span></span>
* <span data-ttu-id="ab6f5-238">Essayez un autre alias de messagerie électronique sur un fournisseur de messagerie différents (Microsoft, Yahoo, Gmail, etc.).</span><span class="sxs-lookup"><span data-stu-id="ab6f5-238">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="ab6f5-239">Créer un [application console pour envoyer un courrier électronique](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="ab6f5-239">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="ab6f5-240">Essayez d’envoyer à des comptes de messagerie différents.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-240">Try sending to different email accounts.</span></span>

<span data-ttu-id="ab6f5-241">**Remarque :** meilleure pratique de sécurité est de ne pas utiliser les secrets de fabrication dans le développement et de test.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-241">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="ab6f5-242">Si vous publiez l’application sur Azure, vous pouvez définir les clés secrètes SendGrid en tant que paramètres de l’application dans le portail de l’application Web Azure.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-242">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="ab6f5-243">Le système de configuration est configuré pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-243">The configuration system is setup to read keys from environment variables.</span></span>

## <a name="prevent-login-at-registration"></a><span data-ttu-id="ab6f5-244">Empêcher la connexion lors de l’inscription</span><span class="sxs-lookup"><span data-stu-id="ab6f5-244">Prevent login at registration</span></span>

<span data-ttu-id="ab6f5-245">Avec les modèles en cours, une fois qu’un utilisateur termine le formulaire d’inscription, qu’ils soient connectés (authentifié).</span><span class="sxs-lookup"><span data-stu-id="ab6f5-245">With the current templates, once a user completes the registration form, they're logged in (authenticated).</span></span> <span data-ttu-id="ab6f5-246">En règle générale, vous souhaitez confirmer leur adresse de messagerie avant de les enregistrer.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-246">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="ab6f5-247">Dans la section ci-dessous, nous allons modifier le code pour exiger des utilisateurs ont un message électronique de confirmation avant qu’ils soient connectés.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-247">In the section below, we will modify the code to require new users have a confirmed email before they're logged in.</span></span> <span data-ttu-id="ab6f5-248">Mise à jour la `[HttpPost] Login` action dans le *AccountController.cs* fichier avec les modifications en surbrillance suivantes.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-248">Update the `[HttpPost] Login` action in the *AccountController.cs* file with the following highlighted changes.</span></span>

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]

<span data-ttu-id="ab6f5-249">**Remarque :** meilleure pratique de sécurité est de ne pas utiliser les secrets de fabrication dans le développement et de test.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-249">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="ab6f5-250">Si vous publiez l’application sur Azure, vous pouvez définir les clés secrètes SendGrid en tant que paramètres de l’application dans le portail de l’application Web Azure.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-250">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="ab6f5-251">Le système de configuration est configuré pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-251">The configuration system is setup to read keys from environment variables.</span></span>


## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="ab6f5-252">Combiner des comptes de connexion de réseaux sociaux et local</span><span class="sxs-lookup"><span data-stu-id="ab6f5-252">Combine social and local login accounts</span></span>

<span data-ttu-id="ab6f5-253">Remarque : Cette section s’applique uniquement aux ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-253">Note: This section applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="ab6f5-254">Pour ASP.NET 2.x de base, consultez [cela](https://github.com/aspnet/Docs/issues/3753) problème.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-254">For ASP.NET Core 2.x, see [this](https://github.com/aspnet/Docs/issues/3753) issue.</span></span>

<span data-ttu-id="ab6f5-255">Pour terminer cette section, vous devez d’abord activer un fournisseur d’authentification externe.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-255">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="ab6f5-256">Consultez [activation de l’authentification à l’aide de Facebook, Google et autres fournisseurs externes](social/index.md).</span><span class="sxs-lookup"><span data-stu-id="ab6f5-256">See [Enabling authentication using Facebook, Google and other external providers](social/index.md).</span></span>

<span data-ttu-id="ab6f5-257">Vous pouvez combiner les comptes locaux et réseaux sociaux en cliquant sur le lien de votre messagerie.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-257">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="ab6f5-258">Dans l’ordre suivant, «RickAndMSFT@gmail.com» est tout d’abord créé en tant qu’une connexion locale ; Toutefois, vous pouvez créer le compte en tant qu’une connexion sociale tout d’abord, puis ajouter une connexion locale.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-258">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Application Web : RickAndMSFT@gmail.com utilisateur authentifié](accconfirm/_static/rick.png)

<span data-ttu-id="ab6f5-260">Cliquez sur le **gérer** lien.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-260">Click on the **Manage** link.</span></span> <span data-ttu-id="ab6f5-261">Notez la valeur 0 externe (connexions sociales) associé à ce compte.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-261">Note the 0 external (social logins) associated with this account.</span></span>

![Gérer les affichages](accconfirm/_static/manage.png)

<span data-ttu-id="ab6f5-263">Cliquez sur le lien vers un autre service de connexion et d’accepter les demandes d’application.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-263">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="ab6f5-264">Dans l’image ci-dessous, Facebook est le fournisseur d’authentification externes :</span><span class="sxs-lookup"><span data-stu-id="ab6f5-264">In the image below, Facebook is the external authentication provider:</span></span>

![Gestion de votre vue de logins externes répertoriant Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="ab6f5-266">Les deux comptes ont été combinés.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-266">The two accounts have been combined.</span></span> <span data-ttu-id="ab6f5-267">Vous ne pourrez pas vous connecter avec un compte.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-267">You will be able to log on with either account.</span></span> <span data-ttu-id="ab6f5-268">Vous pourriez vos utilisateurs pour ajouter des comptes locaux au cas où le journal des réseaux sociaux dans le service d’authentification est arrêté, ou plus probable qu’ils avez perdu l’accès à leur compte sociaux.</span><span class="sxs-lookup"><span data-stu-id="ab6f5-268">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they've lost access to their social account.</span></span>
