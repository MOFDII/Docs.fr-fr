---
title: Autorisation basée sur les revendications dans ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter des vérifications de revendications d’autorisation dans une application ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/claims
ms.openlocfilehash: 6b60ae5515819b017ab577f655ed91ee4d8ed0dd
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275225"
---
# <a name="claims-based-authorization-in-aspnet-core"></a>Autorisation basée sur les revendications dans ASP.NET Core

<a name="security-authorization-claims-based"></a>

Lors de la création d’une identité qu’il peut être affecté à une ou plusieurs revendications émises par une partie de confiance. Une revendication est une paire nom-valeur qui représente le sujet est, pas le sujet peut le faire. Par exemple, peut avoir conduire un permis de, émis par une autorité de licence conduite local. Conduire votre permis d’a votre date de naissance. Dans ce cas le nom de la revendication serait `DateOfBirth`, la valeur de revendication est votre date de naissance, par exemple `8th June 1970` et l’émetteur est l’autorité déterminant de la licence. Autorisation basée sur les revendications, à son la plus simple, vérifie la valeur de revendication et autorise l’accès à une ressource en fonction de cette valeur. Pour exemple, si vous souhaitez que l’accès à un club nuit le processus d’autorisation peut être :

Le responsable de la sécurité porte serait évaluer la valeur de votre date de naissance revendication et qu’elles s’approuvent l’émetteur (l’autorité de licence conduite) avant de qui que vous donne accès.

Une identité peut contenir plusieurs revendications avec plusieurs valeurs et peut contenir plusieurs revendications du même type.

## <a name="adding-claims-checks"></a>Ajout de contrôles de revendications

Revendication les vérifications d’autorisations déclaratives ; le développeur les incorpore dans leur code, par rapport à un contrôleur ou une action dans un contrôleur, en spécifiant les revendications qui l’utilisateur actuel doit posséder et éventuellement la valeur de la revendication doit détenir pour accéder à la ressource demandée. Configuration requise est basée sur la stratégie de revendications, le développeur doit créer et enregistrer une stratégie d’exprimer les exigences de revendications.

Le type le plus simple de stratégie recherche la présence d’une revendication de revendication et ne vérifie pas la valeur.

Vous devez d’abord créer et enregistrer la stratégie. Cette opération a lieu dans le cadre de la configuration du service d’autorisation, qui dure généralement partie dans `ConfigureServices()` dans votre *Startup.cs* fichier.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

Dans ce cas le `EmployeeOnly` stratégie vérifie la présence d’un `EmployeeNumber` de revendication de l’identité actuelle.

Vous appliquez ensuite la stratégie à l’aide de la `Policy` propriété sur le `AuthorizeAttribute` attribut pour spécifier le nom de la stratégie ;

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

Le `AuthorizeAttribute` attribut peut être appliqué à un contrôleur entier, dans cette instance uniquement la stratégie de correspondance des identités peut accéder à une Action sur le contrôleur.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

Si vous disposez d’un contrôleur qui est protégé par le `AuthorizeAttribute` d’attribut, mais souhaitez autoriser l’accès anonyme aux actions particulières que vous appliquez le `AllowAnonymousAttribute` attribut.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

La plupart des revendications sont fournies avec une valeur. Vous pouvez spécifier une liste de valeurs autorisées lors de la création de la stratégie. L’exemple suivant aboutirait uniquement pour les employés dont le numéro employé a été 1, 2, 3, 4 ou 5.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

### <a name="add-a-generic-claim-check"></a>Ajouter une vérification de la revendication générique

Si la valeur de revendication n’est pas une valeur unique ou une transformation est requise, utilisez [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion). Pour plus d’informations, consultez [à l’aide d’une func pour répondre à une stratégie](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).

## <a name="multiple-policy-evaluation"></a>Évaluation des stratégies

Si vous appliquez plusieurs stratégies à un contrôleur ou d’action, toutes les stratégies doivent passer avant que l’accès est accordé. Exemple :

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

Dans l’exemple ci-dessus n’importe quelle identité ce qui répond à la `EmployeeOnly` stratégie peut accéder à la `Payslip` action en tant que cette stratégie est appliquée sur le contrôleur. Toutefois afin d’appeler le `UpdateSalary` action doit répondre à l’identité *les deux* le `EmployeeOnly` stratégie et le `HumanResources` stratégie.

Si vous souhaitez que les stratégies plus complexes, tels que prend une date de naissance revendication, calculer un âge à partir de celui-ci, puis la vérification de la durée de vie est 21 ou antérieure, vous devez écrire [gestionnaires de stratégie personnalisée](xref:security/authorization/policies).
