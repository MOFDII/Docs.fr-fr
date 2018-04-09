---
title: Migration de l’authentification et identité vers ASP.NET Core
author: ardalis
description: En savoir plus sur la migration de l’authentification et identité à partir d’un projet ASP.NET MVC à un projet ASP.NET MVC de base.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: 320f5e079316114832e639d62c780a0639df0c61
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Migration de l’authentification et identité vers ASP.NET Core

<a name="migration-identity"></a>

Par [Steve Smith](https://ardalis.com/)

Dans le précédent article nous [migré la configuration à partir d’un projet ASP.NET MVC à ASP.NET MVC de base](configuration.md). Dans cet article, nous migrer les fonctionnalités de gestion d’inscription et connexion utilisateur.

## <a name="configure-identity-and-membership"></a>Configurez l’identité et l’appartenance

Dans ASP.NET MVC, les fonctionnalités d’authentification et identité sont configurées à l’aide d’ASP.NET Identity dans Startup.Auth.cs et IdentityConfig.cs, situé dans le dossier App_Start. Dans ASP.NET MVC de base, ces fonctionnalités sont configurées dans *Startup.cs*.

Installez les packages NuGet `Microsoft.AspNetCore.Identity.EntityFrameworkCore` et `Microsoft.AspNetCore.Authentication.Cookies`.

Ensuite, ouvrez Startup.cs et mettre à jour le `ConfigureServices()` méthode à utiliser les services d’Entity Framework et de l’identité :

```csharp
public void ConfigureServices(IServiceCollection services)
{
  // Add EF services to the services container.
  services.AddEntityFramework(Configuration)
    .AddSqlServer()
    .AddDbContext<ApplicationDbContext>();

  // Add Identity services to the services container.
  services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
    .AddEntityFrameworkStores<ApplicationDbContext>();

  services.AddMvc();
}
```

À ce stade, il existe des deux types référencés dans le code ci-dessus, nous n’avons pas encore été migrés à partir du projet ASP.NET MVC : `ApplicationDbContext` et `ApplicationUser`. Créer un nouveau *modèles* dossier dans le noyau ASP.NET le projet, puis ajoutez les deux classes lui correspondant à ces types. Vous trouverez le ASP.NET MVC versions de ces classes dans `/Models/IdentityModels.cs`, mais nous allons utiliser un fichier par la classe dans le projet migré car il s’agit plus claire.

ApplicationUser.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

ApplicationDbContext.cs:

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvc6Project.Models
{
  public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
  {
    public ApplicationDbContext()
    {
      Database.EnsureCreated();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
      options.UseSqlServer();
    }
  }
}
```

Personnalisation des utilisateurs ou le ApplicationDbContext n’inclut pas le projet Web de Starter MVC ASP.NET Core. Lorsque vous migrez une application réelle, vous devrez également migrer toutes les propriétés personnalisées et les méthodes de l’utilisateur de votre application et les classes de DbContext, ainsi que d’autres classes de modèle, que votre application utilise (par exemple, si votre DbContext a un DbSet<Album>, vous devrez bien entendu migrer la classe Album).

Ces fichiers en place, le fichier Startup.cs est possible pour compiler à la mise à jour à l’aide de ses instructions :

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

Notre application est maintenant prête à prendre en charge les services d’authentification et identité - il suffit d’activer ces fonctionnalités exposées aux utilisateurs.

## <a name="migrate-registration-and-login-logic"></a>Migrer l’inscription et la logique de connexion

Avec les services d’identité configurés pour l’accès aux applications et données configuré à l’aide d’Entity Framework et SQL Server, nous êtes maintenant prêts à ajouter la prise en charge pour l’inscription et connexion à l’application. N’oubliez pas que [plus haut dans le processus de migration](mvc.md#migrate-layout-file) nous commenté une référence à _LoginPartial dans _Layout.cshtml. Il est maintenant temps pour revenir à ce code, supprimez les commentaires et l’ajouter dans les contrôleurs nécessaires et les vues pour prendre en charge les fonctionnalités de connexion.

Mise à jour _Layout.cshtml ; ne pas commenter la @Html.Partial ligne :

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Maintenant, ajoutez une nouvelle Page de vue MVC appelé _LoginPartial dans le dossier Views/Shared :

Mettre à jour de _LoginPartial.cshtml avec le code suivant (Remplacez tout son contenu) :

```cshtml
@inject SignInManager<User> SignInManager
@inject UserManager<User> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="LogOff" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log off</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

À ce stade, vous devez être en mesure d’actualiser le site dans votre navigateur.

## <a name="summary"></a>Récapitulatif

ASP.NET Core introduit des modifications pour les fonctionnalités d’identité ASP.NET. Dans cet article, vous avez vu comment migrer les fonctionnalités de gestion de l’authentification et l’utilisateur d’une identité ASP.NET vers ASP.NET Core.
