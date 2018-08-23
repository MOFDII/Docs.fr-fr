---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Quelles sont les nouveautés dans ASP.NET Web API 2.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 12/25/2014
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: ef08a3bb397ff54795ca6c70cdcc35206cf122f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826595"
---
<a name="whats-new-in-aspnet-web-api-22"></a>Quelles sont les nouveautés dans ASP.NET Web API 2.2
====================
par [Microsoft](https://github.com/microsoft)

Cette rubrique décrit les nouveautés de ASP.NET Web API 2.2.

- [Télécharger](#download)
- [Documentation](#documentation)
- [Nouvelles fonctionnalités dans ASP.NET Web API 2.2](#newf)

    - [OData v4](#OData)
    - [Améliorations du routage d’attribut](#ARI)
    - [Prise en charge de Client de l’API Web pour Windows Phone 8.1](#phone)
- [Problèmes connus et les modifications avec rupture](#known-issues)
- [Correctifs de bogues](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 bêta](#523)

<a id="download"></a>
## <a name="download"></a>Téléchargement

Les fonctionnalités de runtime sont publiées sous forme de packages NuGet dans la galerie NuGet. Tous les packages de runtime suivent le [Semver](http://semver.org/) spécification. Le dernier package ASP.NET Web API 2.2 a la version suivante : « 5.2.0 ». Vous pouvez installer ou mettre à jour ces packages via [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). La version inclut également des packages localisés correspondants sur NuGet.

Vous pouvez installer ou mettre à jour vers les packages NuGet publiées à l’aide de la Console du Gestionnaire de Package NuGet :

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentation

Didacticiels et autres informations sur ASP.NET Web API 2.2 sont disponibles à partir du site web ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>Nouvelles fonctionnalités dans ASP.NET Web API 2.2

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

Cette version ajoute la prise en charge pour le protocole OData v4. Pour plus d’informations, consultez le [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Voici quelques-unes des principales fonctionnalités et modifications pour OData v4 :

- [Prise en charge pour les propriétés d’alias dans le modèle d’OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [Prise en charge de ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute et ConcurrencyCheckAttribute dans ODataConventionModelBuilder](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Fournir la possibilité de fournir un titre convivial pour les actions](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- Intégrer avec ODL UriParser
- Prise en charge de [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [relation contenant-contenu](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) et [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- Prise en charge de cast pour les types primitifs
- [Prise en charge OData (fonction)](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Alias de paramètre de prise en charge pour les appels de fonction](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Prend en charge la convention d’affectation de noms de casse mixte dans le modèle](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- Prise en charge de cast() dans $filter
- Prise en charge de type complexe ouvert
- Suppression de EntitySetController et AsyncEntitySetController
- [Modifiées $link pour $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Attribut routage prise en charge](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- Utilise des bibliothèques OData Core 6.4.0

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>Améliorations du routage d’attribut

Routage par attributs maintenant fournit un point d’extensibilité appelé IDirectRouteProvider, ce qui permet un contrôle total sur la façon dont les routes d’attribut sont découverts et configurés. Un IDirectRouteProvider est chargé de fournir une liste des actions et les contrôleurs, ainsi que les informations de routage associé pour spécifier exactement quelle configuration de routage est souhaitée pour ces actions. Une implémentation IDirectRouteProvider peut être spécifiée lors de l’appel MapAttributes/MapHttpAttributeRoutes.

Personnalisation IDirectRouteProvider sera le plus simple, en étendant notre implémentation par défaut, DefaultDirectRouteProvider. Cette classe fournit des méthodes virtuelles substituables distincts pour modifier la logique de détection des attributs, création d’entrées d’itinéraire et découvrir le préfixe d’itinéraire et le préfixe de zone.

Voici quelques exemples sur ce que vous pourriez faire avec ce nouveau point d’extensibilité :

1. Prend en charge l’héritage des attributs de Route

    Exemple :

    Ici une requête like « / api/valeurs/10 » avec succès renverrait « Réussite : 10 »

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. Fournir un nom d’itinéraire par défaut pour vos routes d’attribut en suivant certaines convention que vous le souhaitez. Par défaut, le routage par attributs ne crée pas automatiquement les noms de routes d’attribut.
3. Modifier le modèle de routage de routes d’attribut à un seul endroit central avant qu’ils se retrouvent dans la table de routage.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Prise en charge de Client de l’API Web pour Windows Phone 8.1

Vous pouvez maintenant utiliser le package NuGet de Client d’API Web pour implémenter votre logique de client d’API Web lorsque vous ciblez Windows Phone 8.1 ou à partir d’une application universelle.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problèmes connus et les modifications avec rupture

Cette section décrit les problèmes connus et les modifications avec rupture dans ASP.NET Web API 2.2.

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>Générateur de modèles

Problème : Les fonctions surchargées ne peuvent pas être exposées en tant que FunctionImport

S’il existe 2 fonctions surchargées, et ils sont également FunctionImport comme indiqué ci-dessous puis de demander des résultats ~/GetAllConventionCustomers(CustomerName={customerName}) dans System.InvalidOperationException.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Solution de contournement : La solution de contournement pour ce problème consiste à ajouter à la fois les surcharges de fonction en tant que FunctionImports.

#### <a name="odata-routing"></a>Routage d’OData

Les littéraux de chaîne qui incluent l’URL encodée barre oblique (% 2F) et backslash(%5C) provoquer une erreur 404 lorsqu’ils sont utilisés dans les chemins d’accès de la ressource OData.

Par exemple, les littéraux de chaîne peuvent être utilisés dans les chemins d’accès de la ressource OData en tant que paramètres de fonctions ou des valeurs de clés de jeux d’entités.

/Employees/total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Lorsque les services reçoivent ces demandes la hôtes va annuler-séquence d’échappement les séquences d’échappement avant leur transmission à l’exécution de l’API Web. Cela protège contre les attaques comme suit :  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

Cela entraîne la pile de l’API Web OData retourner une erreur 404 (introuvable). Pour éviter cette erreur, votre client doit utiliser les séquences d’échappement double pour une barre oblique (% 252F) et une barre oblique inverse (% C de 255). Cela ne se produit pas pour les chaînes de requête tels que /Employees ? $filter = Name eq 'Nom % 2F'

**Notez les barres obliques sans séquence d’échappement (« / ») et les barres obliques inverses (") ne sont pas autorisées dans les littéraux de chaîne de chemin d’accès de ressource OData. Barres obliques doivent apparaître uniquement comme séparateurs de chemin d’accès et les barres obliques inverses ne doit pas apparaître dans le chemin d’accès de la ressource OData du tout. (Les deux sont utilisables dans certaines parties d’une chaîne de requête OData).**

Solution de contournement : Vous pourriez substituer la méthode Parse de DefaultODataPathHandler pour échapper la barre oblique et une barre oblique inverse dans les littéraux de chaîne avant de les analyser en fait. Vous trouverez un exemple de cette approche ici.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Queryable]

L’attribut [Queryable] est déconseillé. Nouvelles OData v3 applications doivent utiliser **System.Web.Http.OData.EnableQueryAttribute**.

Le **ODataHttpConfigurationExtensions.EnableQuerySupport** méthode d’extension ajoute à présent un **EnableQueryAttribute** à la collection de filtres globaux. Si tous les contrôleurs ont le **[Queryable]** d’attribut, l’appel `config.EnableQuerySupport()` entraîne la **[Queryable]** attribut Échec

La méthode recommandée pour résoudre ce problème consiste à remplacer toutes les instances de **QueryableAttribute** avec **System.Web.Http.OData.EnableQueryAttribute**.

Une autre solution consiste à utiliser le code suivant dans votre configuration de l’API Web :

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Routage par attributs

Problème : Liaison de modèle de type complexe qui est décorée avec l’attribut de FromUri se comporte différemment lorsque vous utilisez le routage par attributs.

Lien suivant consiste à suivre le problème et aussi des informations sur une solution de contournement.  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Problème : Génération de modèles automatique MVC/API Web dans un projet avec 5.2.0 résultats packages dans 5.1.2 packages pour ceux qui n’existent pas déjà dans le projet

La mise à jour des packages NuGet pour ASP.NET MVC 5.2 n’actualise pas les outils de Visual Studio telles que la structure ASP.NET ou le modèle de projet d’Application Web ASP.NET. Ils utilisent la version précédente des packages de runtime ASP.NET (par exemple, 5.1.2 dans Update 2). Par conséquent, la génération de modèles automatique ASP.NET installera la version précédente (par exemple, 5.1.2 dans Update 2) des packages requis, si elles ne sont pas déjà disponibles dans vos projets. Toutefois, la génération de modèles automatique ASP.NET dans Visual Studio 2013 RTM ou 1 de mise à jour ne remplace pas les derniers packages dans vos projets. Si vous utilisez la génération de modèles automatique ASP.NET après la mise à jour les packages de vos projets Web API 2.2 ou ASP.NET MVC 5.2, assurez-vous que les versions des API Web et ASP.NET MVC sont cohérentes.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Correctifs de bogues et mises à jour de fonctionnalité secondaire

Cette version inclut également plusieurs correctifs de bogues et fonctionnalités mineures mises à jour. Vous trouverez la liste complète ici :

- [package 5.2](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

Le package Microsoft.AspNet.OData 5.2.1 contient des mises à jour des dépendances NuGet, mais aucune des correctifs de bogues. Avec cette mise à jour, il n’est plus une dépendance stricte Microsoft.OData.Core 6.4.0, mais un peut mettre à niveau vers toute version entre 6.4.0 et 7.0.0.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

Dans cette version, nous avons effectué une dépendance de changement pour `Json.Net 6.0.4`. Pour plus d’informations sur les nouveautés introduite dans cette version de `Json.NET`, consultez [Json.NET 6.0 version 4 - fusion de JSON, l’Injection de dépendances](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). Cette version ne comporte aucune autre nouvelle fonctionnalité ou des correctifs de bogues dans l’API Web. Nous avons par la suite mis à jour tous les autres packages dépendants que nous possédons afin qu’ils dépendent de cette nouvelle version de l’API Web.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 bêta

Vous pouvez consulter la version [ici](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Cette version contient uniquement les correctifs de bogues. Vous pouvez utiliser [cette requête](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) pour afficher la liste des problèmes résolus dans cette version.
