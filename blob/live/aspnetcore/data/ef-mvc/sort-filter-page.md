---
title: "Cœur de ASP.NET MVC avec EF Core - trier, filtrer, la pagination - 3 sur 10"
author: tdykstra
description: "Dans ce didacticiel, vous allez ajouter le tri, filtrage et d’échange vers la page à l’aide d’ASP.NET Core et Entity Framework Core."
ms.author: tdykstra
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 6da2073b18f6fff9738808c84441e59240caefe3
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a><span data-ttu-id="0a965-103">Le tri, le filtrage, la pagination et le regroupement - Core EF avec le didacticiel ASP.NET Core MVC (partie 3 sur 10)</span><span class="sxs-lookup"><span data-stu-id="0a965-103">Sorting, filtering, paging, and grouping - EF Core with ASP.NET Core MVC tutorial (3 of 10)</span></span>

<span data-ttu-id="0a965-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0a965-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0a965-105">L’exemple d’application web Contoso University montre comment créer des applications web ASP.NET MVC de base à l’aide d’Entity Framework Core et Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0a965-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="0a965-106">Pour plus d’informations sur la série de didacticiels, consultez [le premier didacticiel de la série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="0a965-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="0a965-107">Dans le didacticiel précédent, vous implémenté un ensemble de pages web pour les opérations CRUD de base pour les entités de Student.</span><span class="sxs-lookup"><span data-stu-id="0a965-107">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="0a965-108">Dans ce didacticiel, vous allez ajouter le tri, filtrage et la fonctionnalité de pagination à la page d’Index d’étudiants.</span><span class="sxs-lookup"><span data-stu-id="0a965-108">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="0a965-109">Vous allez également créer une page qui effectue le regroupement simple.</span><span class="sxs-lookup"><span data-stu-id="0a965-109">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="0a965-110">L’illustration suivante montre à quoi ressemblera la page une lorsque vous avez terminé.</span><span class="sxs-lookup"><span data-stu-id="0a965-110">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="0a965-111">Les en-têtes de colonne sont des liens que l’utilisateur peut cliquer pour trier par colonne.</span><span class="sxs-lookup"><span data-stu-id="0a965-111">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="0a965-112">En cliquant sur un en-tête à plusieurs reprises de colonne bascule entre croissant et décroissant d’ordre de tri.</span><span class="sxs-lookup"><span data-stu-id="0a965-112">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Page d’index les étudiants](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="0a965-114">Ajouter des liens de tri de colonne à la Page d’Index les étudiants</span><span class="sxs-lookup"><span data-stu-id="0a965-114">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="0a965-115">Pour ajouter le tri à la page d’Index de l’étudiant, vous allez modifier le `Index` méthode du contrôleur étudiants et ajouter du code à la vue de l’Index de l’étudiant.</span><span class="sxs-lookup"><span data-stu-id="0a965-115">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="0a965-116">Ajouter les fonctionnalités de tri pour l’Index (méthode)</span><span class="sxs-lookup"><span data-stu-id="0a965-116">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="0a965-117">Dans *StudentsController.cs*, remplacez le `Index` méthode avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0a965-117">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="0a965-118">Ce code reçoit un `sortOrder` paramètre à partir de la chaîne de requête dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="0a965-118">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="0a965-119">La valeur de chaîne de requête est fournie par ASP.NET MVC de base en tant que paramètre à la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="0a965-119">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="0a965-120">Le paramètre sera une chaîne qui est « Name » ou « Date », éventuellement suivie d’un trait de soulignement et de la chaîne « desc » pour spécifier l’ordre décroissant.</span><span class="sxs-lookup"><span data-stu-id="0a965-120">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="0a965-121">L'ordre de tri par défaut est le tri croissant.</span><span class="sxs-lookup"><span data-stu-id="0a965-121">The default sort order is ascending.</span></span>

<span data-ttu-id="0a965-122">La première fois que la page d’Index est demandée, il n’aucune chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="0a965-122">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="0a965-123">Les étudiants sont affichés dans l’ordre croissant par nom, qui est la valeur par défaut établies par le cas de passage dans le `switch` instruction.</span><span class="sxs-lookup"><span data-stu-id="0a965-123">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="0a965-124">Lorsque l’utilisateur clique sur un colonne titre lien hypertexte, approprié `sortOrder` valeur est fournie dans la chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="0a965-124">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="0a965-125">Les deux `ViewData` éléments (NameSortParm et DateSortParm) sont utilisés par la vue pour configurer des liens hypertexte du titre de colonne avec les valeurs de chaîne de requête appropriée.</span><span class="sxs-lookup"><span data-stu-id="0a965-125">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="0a965-126">Il s’agit d’instructions ternaires.</span><span class="sxs-lookup"><span data-stu-id="0a965-126">These are ternary statements.</span></span> <span data-ttu-id="0a965-127">La première condition spécifie que si le `sortOrder` paramètre est null ou vide, NameSortParm doit être défini sur « name_desc » ; sinon, il doit être défini sur une chaîne vide.</span><span class="sxs-lookup"><span data-stu-id="0a965-127">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="0a965-128">Ces deux instructions activer l’affichage définir la colonne des liens hypertexte du titre comme suit :</span><span class="sxs-lookup"><span data-stu-id="0a965-128">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="0a965-129">Ordre de tri en cours</span><span class="sxs-lookup"><span data-stu-id="0a965-129">Current sort order</span></span>  | <span data-ttu-id="0a965-130">Lien hypertexte du nom du dernier</span><span class="sxs-lookup"><span data-stu-id="0a965-130">Last Name Hyperlink</span></span> | <span data-ttu-id="0a965-131">Lien hypertexte de date</span><span class="sxs-lookup"><span data-stu-id="0a965-131">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="0a965-132">Dernier nom par ordre croissant</span><span class="sxs-lookup"><span data-stu-id="0a965-132">Last Name ascending</span></span>  | <span data-ttu-id="0a965-133">descending</span><span class="sxs-lookup"><span data-stu-id="0a965-133">descending</span></span>          | <span data-ttu-id="0a965-134">ascending</span><span class="sxs-lookup"><span data-stu-id="0a965-134">ascending</span></span>      |
| <span data-ttu-id="0a965-135">Dernier nom décroissant</span><span class="sxs-lookup"><span data-stu-id="0a965-135">Last Name descending</span></span> | <span data-ttu-id="0a965-136">ascending</span><span class="sxs-lookup"><span data-stu-id="0a965-136">ascending</span></span>           | <span data-ttu-id="0a965-137">ascending</span><span class="sxs-lookup"><span data-stu-id="0a965-137">ascending</span></span>      |
| <span data-ttu-id="0a965-138">Date de l’ordre croissant</span><span class="sxs-lookup"><span data-stu-id="0a965-138">Date ascending</span></span>       | <span data-ttu-id="0a965-139">ascending</span><span class="sxs-lookup"><span data-stu-id="0a965-139">ascending</span></span>           | <span data-ttu-id="0a965-140">descending</span><span class="sxs-lookup"><span data-stu-id="0a965-140">descending</span></span>     |
| <span data-ttu-id="0a965-141">Date décroissant</span><span class="sxs-lookup"><span data-stu-id="0a965-141">Date descending</span></span>      | <span data-ttu-id="0a965-142">ascending</span><span class="sxs-lookup"><span data-stu-id="0a965-142">ascending</span></span>           | <span data-ttu-id="0a965-143">ascending</span><span class="sxs-lookup"><span data-stu-id="0a965-143">ascending</span></span>      |

<span data-ttu-id="0a965-144">La méthode utilise LINQ to Entities pour spécifier la colonne à trier.</span><span class="sxs-lookup"><span data-stu-id="0a965-144">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="0a965-145">Le code crée un `IQueryable` variable avant l’instruction switch, il modifie dans l’instruction switch et appelle le `ToListAsync` méthode après le `switch` instruction.</span><span class="sxs-lookup"><span data-stu-id="0a965-145">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="0a965-146">Lorsque vous créez et modifiez `IQueryable` variables, aucune requête n’est envoyée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="0a965-146">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="0a965-147">La requête n’est pas exécutée jusqu'à ce que vous convertissez la `IQueryable` objet dans une collection en appelant une méthode comme `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="0a965-147">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="0a965-148">Par conséquent, ce code génère une requête unique qui n’est pas exécutée tant que la `return View` instruction.</span><span class="sxs-lookup"><span data-stu-id="0a965-148">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

<span data-ttu-id="0a965-149">Ce code peut obtenir documentée avec un grand nombre de colonnes.</span><span class="sxs-lookup"><span data-stu-id="0a965-149">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="0a965-150">[Le didacticiel dernière de cette série](advanced.md#dynamic-linq) montre comment écrire du code qui vous permet de passer le nom de la `OrderBy` colonne dans une variable de chaîne.</span><span class="sxs-lookup"><span data-stu-id="0a965-150">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="0a965-151">Ajouter des liens hypertexte du titre de colonne à la vue de l’Index de l’étudiant</span><span class="sxs-lookup"><span data-stu-id="0a965-151">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="0a965-152">Remplacez le code dans *Views/Students/Index.cshtml*, avec le code suivant pour ajouter des liens hypertexte du titre de colonne.</span><span class="sxs-lookup"><span data-stu-id="0a965-152">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="0a965-153">Les lignes modifiées sont mises en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="0a965-153">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="0a965-154">Ce code utilise les informations contenues dans `ViewData` les valeurs de chaîne de propriétés pour configurer des liens hypertexte avec la requête appropriée.</span><span class="sxs-lookup"><span data-stu-id="0a965-154">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="0a965-155">Exécuter l’application, sélectionnez le **étudiants** onglet, puis cliquez sur le **nom** et **Date d’inscription** des en-têtes de colonne pour vérifier que le tri fonctionne.</span><span class="sxs-lookup"><span data-stu-id="0a965-155">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Page d’index étudiants dans l’ordre de nom](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="0a965-157">Ajouter une zone de recherche à la page d’Index des étudiants</span><span class="sxs-lookup"><span data-stu-id="0a965-157">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="0a965-158">Pour ajouter le filtrage sur la page d’Index des étudiants, vous allez ajouter une zone de texte et un bouton d’envoi à la vue et apporter les modifications correspondantes dans le `Index` (méthode).</span><span class="sxs-lookup"><span data-stu-id="0a965-158">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="0a965-159">La zone de texte vous permet d’entrer une chaîne à rechercher dans le prénom et le champ.</span><span class="sxs-lookup"><span data-stu-id="0a965-159">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="0a965-160">Ajouter des fonctionnalités de filtrage à l’Index (méthode)</span><span class="sxs-lookup"><span data-stu-id="0a965-160">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="0a965-161">Dans *StudentsController.cs*, remplacez le `Index` méthode avec le code suivant (les modifications sont mises en surbrillance).</span><span class="sxs-lookup"><span data-stu-id="0a965-161">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="0a965-162">Vous avez ajouté un `searchString` paramètre à la `Index` (méthode).</span><span class="sxs-lookup"><span data-stu-id="0a965-162">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="0a965-163">La valeur de chaîne de recherche est reçue à partir d’une zone de texte que vous ajouterez à la vue Index.</span><span class="sxs-lookup"><span data-stu-id="0a965-163">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="0a965-164">Vous avez également ajouté à l’instruction LINQ where clause qui sélectionne uniquement les étudiants dont prénom ou le nom contient la chaîne de recherche.</span><span class="sxs-lookup"><span data-stu-id="0a965-164">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="0a965-165">L’instruction qui ajoute where clause est exécutée uniquement s’il existe une valeur à rechercher.</span><span class="sxs-lookup"><span data-stu-id="0a965-165">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="0a965-166">Ici vous appelez le `Where` méthode sur une `IQueryable` objet et le filtre seront traités sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="0a965-166">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="0a965-167">Dans certains scénarios vous pouvez appeler la `Where` méthode comme méthode d’extension sur une collection en mémoire.</span><span class="sxs-lookup"><span data-stu-id="0a965-167">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="0a965-168">(Par exemple, supposons que vous modifiez la référence à `_context.Students` ainsi qu’au lieu d’un FE `DbSet` il fait référence à une méthode de référentiel qui retourne un `IEnumerable` collection.) Le résultat serait normalement le même, mais dans certains cas, peut être différent.</span><span class="sxs-lookup"><span data-stu-id="0a965-168">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="0a965-169">Par exemple, l’implémentation du .NET Framework de la `Contains` méthode effectue une comparaison respectant la casse par défaut, mais dans SQL Server, cela est déterminé par le paramètre de classement de l’instance de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0a965-169">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="0a965-170">Ce paramètre par défaut à la casse.</span><span class="sxs-lookup"><span data-stu-id="0a965-170">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="0a965-171">Vous pouvez appeler la `ToUpper` méthode le test doit être explicitement pas la casse : *où (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*.</span><span class="sxs-lookup"><span data-stu-id="0a965-171">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="0a965-172">Qui s’assurent que que résultats restent le même si vous modifiez le code ultérieurement pour utiliser un référentiel qui retourne un `IEnumerable` collection au lieu d’un `IQueryable` objet.</span><span class="sxs-lookup"><span data-stu-id="0a965-172">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="0a965-173">(Lorsque vous appelez le `Contains` méthode sur une `IEnumerable` collection, vous obtenez l’implémentation du .NET Framework ; lorsque vous appelez sur un `IQueryable` de l’objet, vous obtenez l’implémentation du fournisseur de base de données.) Toutefois, il est d’une baisse des performances de cette solution.</span><span class="sxs-lookup"><span data-stu-id="0a965-173">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there is a performance penalty for this solution.</span></span> <span data-ttu-id="0a965-174">Le `ToUpper` code place une fonction dans la clause WHERE de l’instruction SELECT de TSQL.</span><span class="sxs-lookup"><span data-stu-id="0a965-174">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="0a965-175">Qui empêche l’optimiseur d’à l’aide d’un index.</span><span class="sxs-lookup"><span data-stu-id="0a965-175">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="0a965-176">Étant donné que SQL est installé principalement sans respecter la casse, il est préférable d’éviter le `ToUpper` code jusqu'à ce que vous migrez vers un magasin de données qui respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="0a965-176">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="0a965-177">Ajouter une zone de recherche à la vue d’Index étudiant</span><span class="sxs-lookup"><span data-stu-id="0a965-177">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="0a965-178">Dans *Views/Student/Index.cshtml*, ajoutez le code en surbrillance immédiatement avant l’ouverture de balise table afin de créer une légende, une zone de texte et un **recherche** bouton.</span><span class="sxs-lookup"><span data-stu-id="0a965-178">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="0a965-179">Ce code utilise le `<form>` [d’assistance de balise](xref:mvc/views/tag-helpers/intro) pour ajouter la zone de texte de recherche et le bouton.</span><span class="sxs-lookup"><span data-stu-id="0a965-179">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="0a965-180">Par défaut, le `<form>` application d’assistance de balise envoie des données de formulaire avec une publication, ce qui signifie que les paramètres sont passés dans le corps du message HTTP et non dans l’URL sous forme de chaînes de requête.</span><span class="sxs-lookup"><span data-stu-id="0a965-180">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="0a965-181">Lorsque vous spécifiez HTTP GET, les données du formulaire sont passées dans l’URL sous forme de chaînes de requête, ce qui permet aux utilisateurs de l’URL de signet.</span><span class="sxs-lookup"><span data-stu-id="0a965-181">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="0a965-182">W3C instructions il est conseillé que vous devez utiliser obtenir lors de l’action n’entraîne pas une mise à jour.</span><span class="sxs-lookup"><span data-stu-id="0a965-182">The W3C guidelines recommend that you should use GET when the action does not result in an update.</span></span>

<span data-ttu-id="0a965-183">Exécuter l’application, sélectionnez le **étudiants** onglet, entrez une chaîne de recherche, puis cliquez sur Rechercher pour vérifier que le filtrage fonctionne.</span><span class="sxs-lookup"><span data-stu-id="0a965-183">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![Page d’index de stagiaires de filtrage](sort-filter-page/_static/filtering.png)

<span data-ttu-id="0a965-185">Notez que l’URL contient la chaîne de recherche.</span><span class="sxs-lookup"><span data-stu-id="0a965-185">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="0a965-186">Si vous créez un signet cette page, vous obtenez la liste filtrée lorsque vous utilisez le signet.</span><span class="sxs-lookup"><span data-stu-id="0a965-186">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="0a965-187">Ajout de `method="get"` à la `form` balise est ce qui a provoqué la chaîne de requête à générer.</span><span class="sxs-lookup"><span data-stu-id="0a965-187">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="0a965-188">À ce stade, si vous cliquez sur un lien de tri de titre de colonne vous perdez la valeur de filtre que vous avez entré dans le **recherche** boîte.</span><span class="sxs-lookup"><span data-stu-id="0a965-188">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="0a965-189">Vous allez résoudre cela dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="0a965-189">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="0a965-190">Ajouter la fonctionnalité de pagination à la page d’Index des étudiants</span><span class="sxs-lookup"><span data-stu-id="0a965-190">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="0a965-191">Pour ajouter la pagination à la page d’Index des étudiants, vous allez créer un `PaginatedList` classe utilise `Skip` et `Take` instructions pour filtrer les données sur le serveur au lieu de toujours récupérer toutes les lignes de la table.</span><span class="sxs-lookup"><span data-stu-id="0a965-191">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="0a965-192">Ensuite, vous allez apporter des modifications supplémentaires dans le `Index` (méthode) et ajouter des boutons de la pagination à la `Index` vue.</span><span class="sxs-lookup"><span data-stu-id="0a965-192">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="0a965-193">L’illustration suivante montre les boutons de la pagination.</span><span class="sxs-lookup"><span data-stu-id="0a965-193">The following illustration shows the paging buttons.</span></span>

![Page avec des liens de pagination d’index les étudiants](sort-filter-page/_static/paging.png)

<span data-ttu-id="0a965-195">Dans le dossier du projet, créez `PaginatedList.cs`, puis remplacez le code du modèle par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="0a965-195">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="0a965-196">Le `CreateAsync` méthode dans ce code prend la taille de la page et le numéro de page et applique `Skip` et `Take` instructions pour le `IQueryable`.</span><span class="sxs-lookup"><span data-stu-id="0a965-196">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="0a965-197">Lorsque `ToListAsync` est appelée sur le `IQueryable`, il retourne une liste contenant uniquement la page demandée.</span><span class="sxs-lookup"><span data-stu-id="0a965-197">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="0a965-198">Les propriétés `HasPreviousPage` et `HasNextPage` peut être utilisé pour activer ou désactiver **précédent** et **suivant** boutons de pagination.</span><span class="sxs-lookup"><span data-stu-id="0a965-198">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="0a965-199">A `CreateAsync` méthode est utilisée au lieu d’un constructeur pour créer le `PaginatedList<T>` de l’objet, car les constructeurs ne peuvent pas exécuter du code asynchrone.</span><span class="sxs-lookup"><span data-stu-id="0a965-199">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="0a965-200">Ajouter la fonctionnalité de pagination pour le Index (méthode)</span><span class="sxs-lookup"><span data-stu-id="0a965-200">Add paging functionality to the Index method</span></span>

<span data-ttu-id="0a965-201">Dans *StudentsController.cs*, remplacez le `Index` méthode avec le code suivant.</span><span class="sxs-lookup"><span data-stu-id="0a965-201">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="0a965-202">Ce code ajoute un paramètre de numéro de page, un paramètre de commande de tri actuelle et un paramètre de filtre actuel pour la signature de méthode.</span><span class="sxs-lookup"><span data-stu-id="0a965-202">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="0a965-203">La première fois que la page s’affiche, ou si l’utilisateur n’a pas cliqué sur un échange ou le lien de tri, tous les paramètres sont null.</span><span class="sxs-lookup"><span data-stu-id="0a965-203">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="0a965-204">Si un lien de la pagination est activé, la variable de page contient le numéro de page à afficher.</span><span class="sxs-lookup"><span data-stu-id="0a965-204">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="0a965-205">Le `ViewData` élément nommé CurrentSort fournit la vue de l’ordre de tri actuel, car il doit être incluse dans les liens de la pagination afin de conserver l’ordre de tri lors de la pagination.</span><span class="sxs-lookup"><span data-stu-id="0a965-205">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="0a965-206">Le `ViewData` élément nommé FiltreActif fournit la vue avec la chaîne de filtre actuel.</span><span class="sxs-lookup"><span data-stu-id="0a965-206">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="0a965-207">Cette valeur doit être incluse dans les liens de la pagination afin de conserver les paramètres de filtre lors de la pagination, et elle doit être restaurée à la zone de texte lorsque la page s’affiche de nouveau.</span><span class="sxs-lookup"><span data-stu-id="0a965-207">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="0a965-208">Si la chaîne de recherche est modifiée au cours de la pagination, la page doit être réinitialisé à 1, parce que le nouveau filtre pour afficher des données différentes.</span><span class="sxs-lookup"><span data-stu-id="0a965-208">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="0a965-209">La chaîne de recherche est modifiée quand une valeur est entrée dans la zone de texte et le bouton d’envoi est enfoncé.</span><span class="sxs-lookup"><span data-stu-id="0a965-209">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="0a965-210">Dans ce cas, le `searchString` paramètre n’est pas null.</span><span class="sxs-lookup"><span data-stu-id="0a965-210">In that case, the `searchString` parameter is not null.</span></span>

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

<span data-ttu-id="0a965-211">À la fin de la `Index` (méthode), la `PaginatedList.CreateAsync` méthode convertit la requête de student à une seule page d’étudiants dans un type de collection qui prend en charge la pagination.</span><span class="sxs-lookup"><span data-stu-id="0a965-211">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="0a965-212">Cette page unique des étudiants est ensuite passée à la vue.</span><span class="sxs-lookup"><span data-stu-id="0a965-212">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="0a965-213">Le `PaginatedList.CreateAsync` méthode prend un numéro de page.</span><span class="sxs-lookup"><span data-stu-id="0a965-213">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="0a965-214">Les deux points d’interrogation représenter l’opérateur de fusion null.</span><span class="sxs-lookup"><span data-stu-id="0a965-214">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="0a965-215">L’opérateur de fusion null définit une valeur par défaut pour un type nullable ; l’expression `(page ?? 1)` signifie retourne la valeur de `page` si elle a une valeur, ou retourne 1 si `page` a la valeur null.</span><span class="sxs-lookup"><span data-stu-id="0a965-215">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="0a965-216">Ajouter des liens de pagination à la vue de l’Index de l’étudiant</span><span class="sxs-lookup"><span data-stu-id="0a965-216">Add paging links to the Student Index view</span></span>

<span data-ttu-id="0a965-217">Dans *Views/Students/Index.cshtml*, remplacez le code existant par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="0a965-217">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="0a965-218">Les modifications sont mises en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="0a965-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="0a965-219">Le `@model` instruction en haut de la page spécifie que la vue obtient désormais une `PaginatedList<T>` de l’objet au lieu d’un `List<T>` objet.</span><span class="sxs-lookup"><span data-stu-id="0a965-219">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="0a965-220">Les liens d’en-tête de colonne permet de passer la chaîne de recherche en cours au contrôleur afin que l’utilisateur peut trier les résultats de filtre de la chaîne de requête :</span><span class="sxs-lookup"><span data-stu-id="0a965-220">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="0a965-221">Les boutons de pagination sont affichés par les programmes d’assistance de balise :</span><span class="sxs-lookup"><span data-stu-id="0a965-221">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="0a965-222">Exécutez l’application et accédez à la page d’étudiants.</span><span class="sxs-lookup"><span data-stu-id="0a965-222">Run the app and go to the Students page.</span></span>

![Page avec des liens de pagination d’index les étudiants](sort-filter-page/_static/paging.png)

<span data-ttu-id="0a965-224">Cliquez sur les liens de la pagination dans différents ordres de tri s’assurer que la pagination fonctionne.</span><span class="sxs-lookup"><span data-stu-id="0a965-224">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="0a965-225">Entrez une chaîne de recherche, puis recommencez la pagination pour vérifier que la pagination également fonctionne correctement avec le tri et de filtrage.</span><span class="sxs-lookup"><span data-stu-id="0a965-225">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="0a965-226">Créer une page à propos qui affiche les statistiques de l’étudiant</span><span class="sxs-lookup"><span data-stu-id="0a965-226">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="0a965-227">Pour le site de Web Contoso University **sur** page, vous afficherez le nombre d’étudiants inscrits pour chaque date d’inscription.</span><span class="sxs-lookup"><span data-stu-id="0a965-227">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="0a965-228">Cela nécessite de regroupement et simples des calculs sur les groupes.</span><span class="sxs-lookup"><span data-stu-id="0a965-228">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="0a965-229">Pour ce faire, vous devez procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="0a965-229">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="0a965-230">Créer une classe de modèle d’affichage pour les données dont vous avez besoin pour passer à la vue.</span><span class="sxs-lookup"><span data-stu-id="0a965-230">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="0a965-231">Modifiez la méthode à propos du contrôleur Home.</span><span class="sxs-lookup"><span data-stu-id="0a965-231">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="0a965-232">Modifier la vue à propos de.</span><span class="sxs-lookup"><span data-stu-id="0a965-232">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="0a965-233">Créer le modèle d’affichage</span><span class="sxs-lookup"><span data-stu-id="0a965-233">Create the view model</span></span>

<span data-ttu-id="0a965-234">Créer un *SchoolViewModels* dossier dans le *modèles* dossier.</span><span class="sxs-lookup"><span data-stu-id="0a965-234">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="0a965-235">Dans le nouveau dossier, ajoutez un fichier de classe *EnrollmentDateGroup.cs* et remplacez le code de modèle par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0a965-235">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="0a965-236">Modifier le contrôleur Home</span><span class="sxs-lookup"><span data-stu-id="0a965-236">Modify the Home Controller</span></span>

<span data-ttu-id="0a965-237">Dans *HomeController.cs*, ajoutez le code suivant à l’aide d’instructions en haut du fichier :</span><span class="sxs-lookup"><span data-stu-id="0a965-237">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="0a965-238">Ajouter une variable de classe pour le contexte de base de données immédiatement après l’accolade ouvrante de la classe et en obtenir une instance du contexte ASP.NET Core DI :</span><span class="sxs-lookup"><span data-stu-id="0a965-238">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="0a965-239">Remplacez la méthode `About` par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0a965-239">Replace the `About` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="0a965-240">L’instruction LINQ regroupe les entités étudiant par date d’inscription, calcule le nombre d’entités dans chaque groupe et stocke les résultats dans une collection de `EnrollmentDateGroup` afficher les objets de modèle.</span><span class="sxs-lookup"><span data-stu-id="0a965-240">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE] 
> <span data-ttu-id="0a965-241">Dans la 1.0 version de Entity Framework Core, le jeu de résultats entier est retourné au client, et de regroupement est effectué sur le client.</span><span class="sxs-lookup"><span data-stu-id="0a965-241">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="0a965-242">Dans certains scénarios, cela pourrait créer des problèmes de performances.</span><span class="sxs-lookup"><span data-stu-id="0a965-242">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="0a965-243">Veillez à tester les performances avec les volumes de données de production et si nécessaire utilisez brute SQL pour effectuer le regroupement sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="0a965-243">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="0a965-244">Pour plus d’informations sur l’utilisation de SQL brute, consultez [le dernier didacticiel de cette série](advanced.md).</span><span class="sxs-lookup"><span data-stu-id="0a965-244">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="0a965-245">Modifier le sur la vue</span><span class="sxs-lookup"><span data-stu-id="0a965-245">Modify the About View</span></span>

<span data-ttu-id="0a965-246">Remplacez le code dans le *Views/Home/About.cshtml* fichier avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="0a965-246">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="0a965-247">Exécutez l’application et accédez à la page à propos de.</span><span class="sxs-lookup"><span data-stu-id="0a965-247">Run the app and go to the About page.</span></span> <span data-ttu-id="0a965-248">Le nombre d’étudiants pour chaque date d’inscription s’affiche dans une table.</span><span class="sxs-lookup"><span data-stu-id="0a965-248">The count of students for each enrollment date is displayed in a table.</span></span>

![Sur la page](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="0a965-250">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="0a965-250">Summary</span></span>

<span data-ttu-id="0a965-251">Dans ce didacticiel, vous avez vu comment effectuer le tri, le filtrage, la pagination et regroupement.</span><span class="sxs-lookup"><span data-stu-id="0a965-251">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="0a965-252">Dans l’étape suivante du didacticiel, vous allez apprendre à gérer les modifications du modèle de données à l’aide des migrations.</span><span class="sxs-lookup"><span data-stu-id="0a965-252">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0a965-253">[Précédent](crud.md)
[Suivant](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="0a965-253">[Previous](crud.md)
[Next](migrations.md)</span></span>  
