---
title: "Cœur de ASP.NET MVC avec EF Core - modèle de données - 5, 10"
author: tdykstra
description: "Dans ce didacticiel, vous ajoutez des entités et relations et personnalisez le modèle de données en spécifiant la mise en forme, la validation et les règles de mappage de base de données."
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: ac30d9ae5531934ba5163a8d9114b11ac54af8d2
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a><span data-ttu-id="b82e3-103">Création d’un modèle de données complexes - Core EF avec le didacticiel d’ASP.NET MVC de base (5 sur 10)</span><span class="sxs-lookup"><span data-stu-id="b82e3-103">Creating a complex data model - EF Core with ASP.NET Core MVC tutorial (5 of 10)</span></span>

<span data-ttu-id="b82e3-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b82e3-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b82e3-105">L’exemple d’application web Contoso University montre comment créer des applications web ASP.NET MVC de base à l’aide d’Entity Framework Core et Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b82e3-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="b82e3-106">Pour plus d’informations sur la série de didacticiels, consultez [le premier didacticiel de la série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="b82e3-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="b82e3-107">Dans les didacticiels précédents, vous avez travaillé avec un modèle de données simple composé de trois entités.</span><span class="sxs-lookup"><span data-stu-id="b82e3-107">In the previous tutorials, you worked with a simple data model that was composed of three entities.</span></span> <span data-ttu-id="b82e3-108">Dans ce didacticiel, vous allez ajouter des entités et relations, et vous allez personnaliser le modèle de données en spécifiant la mise en forme, la validation et les règles de mappage de base de données.</span><span class="sxs-lookup"><span data-stu-id="b82e3-108">In this tutorial, you'll add more entities and relationships and you'll customize the data model by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="b82e3-109">Lorsque vous avez terminé, les classes d’entité composent le modèle de données qui est indiqué dans l’illustration suivante :</span><span class="sxs-lookup"><span data-stu-id="b82e3-109">When you're finished, the entity classes will make up the completed data model that's shown in the following illustration:</span></span>

![Diagramme d’entité](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a><span data-ttu-id="b82e3-111">Personnaliser le modèle de données à l’aide d’attributs</span><span class="sxs-lookup"><span data-stu-id="b82e3-111">Customize the Data Model by Using Attributes</span></span>

<span data-ttu-id="b82e3-112">Dans cette section, vous allez apprendre à personnaliser le modèle de données à l’aide des attributs qui spécifient la mise en forme, validation et les règles de mappage de base de données.</span><span class="sxs-lookup"><span data-stu-id="b82e3-112">In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="b82e3-113">Puis dans plusieurs des sections suivantes, vous allez créer le modèle de données School complète en ajoutant des attributs aux classes que vous avez déjà créé et créer de nouvelles classes pour les autres types d’entités dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="b82e3-113">Then in several of the following sections you'll create the complete School data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="b82e3-114">L’attribut de type de données</span><span class="sxs-lookup"><span data-stu-id="b82e3-114">The DataType attribute</span></span>

<span data-ttu-id="b82e3-115">Pour les dates d’inscription étudiant, toutes les pages web actuellement affichent l’heure, ainsi que la date, même si tout vous intéressent pour ce champ est la date.</span><span class="sxs-lookup"><span data-stu-id="b82e3-115">For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date.</span></span> <span data-ttu-id="b82e3-116">À l’aide des attributs d’annotations de données, vous pouvez apporter une modification qui permet de corriger le format d’affichage dans chaque vue qui affiche les données de code.</span><span class="sxs-lookup"><span data-stu-id="b82e3-116">By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data.</span></span> <span data-ttu-id="b82e3-117">Pour voir un exemple de procédure, vous allez ajouter un attribut à la `EnrollmentDate` propriété dans la `Student` classe.</span><span class="sxs-lookup"><span data-stu-id="b82e3-117">To see an example of how to do that, you'll add an attribute to the `EnrollmentDate` property in the `Student` class.</span></span>

<span data-ttu-id="b82e3-118">Dans *Models/Student.cs*, ajouter un `using` instruction pour le `System.ComponentModel.DataAnnotations` espace de noms et ajoutez `DataType` et `DisplayFormat` des attributs à la `EnrollmentDate` la propriété, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="b82e3-118">In *Models/Student.cs*, add a `using` statement for the `System.ComponentModel.DataAnnotations` namespace and add `DataType` and `DisplayFormat` attributes to the `EnrollmentDate` property, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="b82e3-119">L’attribut `DataType` sert à spécifier un type de données qui est plus spécifique que le type intrinsèque de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b82e3-119">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="b82e3-120">Dans ce cas, nous voulons uniquement le suivi de la date, pas la date et l’heure.</span><span class="sxs-lookup"><span data-stu-id="b82e3-120">In this case we only want to keep track of the date, not the date and time.</span></span> <span data-ttu-id="b82e3-121">Le `DataType` énumération fournit de nombreux types de données, telles que Date, Time, numéro de téléphone, devise, EmailAddress et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="b82e3-121">The  `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="b82e3-122">L’attribut `DataType` peut également permettre à l’application de fournir automatiquement des fonctionnalités propres au type.</span><span class="sxs-lookup"><span data-stu-id="b82e3-122">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="b82e3-123">Par exemple, vous pouvez créer un lien `mailto:` pour `DataType.EmailAddress`, et vous pouvez fournir un sélecteur de date pour `DataType.Date` dans les navigateurs qui prennent en charge HTML5.</span><span class="sxs-lookup"><span data-stu-id="b82e3-123">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="b82e3-124">Le `DataType` émet un attribut HTML 5 `data-` attributs (données prononcé tiret) qui peuvent de comprendre les navigateurs HTML 5.</span><span class="sxs-lookup"><span data-stu-id="b82e3-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="b82e3-125">Le `DataType` les attributs ne fournissent aucune validation.</span><span class="sxs-lookup"><span data-stu-id="b82e3-125">The `DataType` attributes don't provide any validation.</span></span>

<span data-ttu-id="b82e3-126">`DataType.Date` ne spécifie pas le format de la date qui s’affiche.</span><span class="sxs-lookup"><span data-stu-id="b82e3-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="b82e3-127">Par défaut, le champ de données s’affiche selon les formats par défaut en fonction CultureInfo du serveur.</span><span class="sxs-lookup"><span data-stu-id="b82e3-127">By default, the data field is displayed according to the default formats based on the server's CultureInfo.</span></span>

<span data-ttu-id="b82e3-128">L’attribut `DisplayFormat` est utilisé pour spécifier explicitement le format de date :</span><span class="sxs-lookup"><span data-stu-id="b82e3-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="b82e3-129">Le paramètre `ApplyFormatInEditMode` indique que la mise en forme doit également être appliquée quand la valeur est affichée dans une zone de texte à des fins de modification.</span><span class="sxs-lookup"><span data-stu-id="b82e3-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="b82e3-130">(Vous pouvez que pour certains champs--par exemple, pour les valeurs de devise, vous pouvez le symbole de devise dans la zone de texte pour le modifier.)</span><span class="sxs-lookup"><span data-stu-id="b82e3-130">(You might not want that for some fields -- for example, for currency values, you might not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="b82e3-131">Vous pouvez utiliser la `DisplayFormat` attribut par lui-même, mais il est généralement judicieux d’utiliser le `DataType` également d’attribut.</span><span class="sxs-lookup"><span data-stu-id="b82e3-131">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute also.</span></span> <span data-ttu-id="b82e3-132">Le `DataType` attribut donne la sémantique des données par opposition à comment effectuer le rendu sur un écran et qu’il offre les avantages suivants que vous n’obtenez pas avec `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="b82e3-132">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with `DisplayFormat`:</span></span>

* <span data-ttu-id="b82e3-133">Le navigateur peut activer des fonctionnalités HTML5 (par exemple pour afficher un contrôle de calendrier, le symbole monétaire approprié de paramètres régionaux, les liens de courrier électronique, certains côté client d’entrée validation, etc..).</span><span class="sxs-lookup"><span data-stu-id="b82e3-133">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.).</span></span>

* <span data-ttu-id="b82e3-134">Par défaut, le navigateur affiche les données à l’aide du format correspondant à vos paramètres régionaux.</span><span class="sxs-lookup"><span data-stu-id="b82e3-134">By default, the browser will render data using the correct format based on your locale.</span></span>

<span data-ttu-id="b82e3-135">Pour plus d’informations, consultez la [ \<d’entrée > documentation de l’application d’assistance de balise](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="b82e3-135">For more information, see the [\<input> tag helper documentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span></span>

<span data-ttu-id="b82e3-136">Exécuter l’application, accédez à la page d’Index des étudiants et notez que fois ne sont plus affichés pour les dates d’inscription.</span><span class="sxs-lookup"><span data-stu-id="b82e3-136">Run the app, go to the Students Index page and notice that times are no longer displayed for the enrollment dates.</span></span> <span data-ttu-id="b82e3-137">Le même aura la valeur true pour n’importe quelle vue qui utilise le modèle de Student.</span><span class="sxs-lookup"><span data-stu-id="b82e3-137">The same will be true for any view that uses the Student model.</span></span>

![Affichage des dates sans heures dans la page d’index les étudiants](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="b82e3-139">L’attribut StringLength</span><span class="sxs-lookup"><span data-stu-id="b82e3-139">The StringLength attribute</span></span>

<span data-ttu-id="b82e3-140">Vous pouvez également spécifier des règles de validation de données et les messages d’erreur de validation à l’aide d’attributs.</span><span class="sxs-lookup"><span data-stu-id="b82e3-140">You can also specify data validation rules and validation error messages using attributes.</span></span> <span data-ttu-id="b82e3-141">Le `StringLength` attribut définit la longueur maximale de la base de données et fournit des côté client et côté serveur validation pour ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b82e3-141">The `StringLength` attribute sets the maximum length  in the database and provides client side and server side validation for ASP.NET MVC.</span></span> <span data-ttu-id="b82e3-142">Vous pouvez également spécifier la longueur de chaîne minimale dans cet attribut, mais la valeur minimale n’a aucun impact sur le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="b82e3-142">You can also specify the minimum string length in this attribute, but the minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="b82e3-143">Supposons que vous souhaitez vous assurer que les utilisateurs n’entrent pas plus de 50 caractères pour un nom.</span><span class="sxs-lookup"><span data-stu-id="b82e3-143">Suppose you want to ensure that users don't enter more than 50 characters for a name.</span></span> <span data-ttu-id="b82e3-144">Pour ajouter cette limitation, ajoutez `StringLength` des attributs à la `LastName` et `FirstMidName` propriétés, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="b82e3-144">To add this limitation, add `StringLength` attributes to the `LastName` and `FirstMidName` properties, as shown in the following example:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="b82e3-145">Le `StringLength` attribut ne sera pas empêcher un utilisateur d’entrer un espace blanc pour un nom.</span><span class="sxs-lookup"><span data-stu-id="b82e3-145">The `StringLength` attribute won't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="b82e3-146">Vous pouvez utiliser la `RegularExpression` attribut à appliquer des restrictions à l’entrée.</span><span class="sxs-lookup"><span data-stu-id="b82e3-146">You can use the `RegularExpression` attribute to apply restrictions to the input.</span></span> <span data-ttu-id="b82e3-147">Par exemple, le code suivant requiert le premier caractère en majuscules et les autres caractères alphabétique :</span><span class="sxs-lookup"><span data-stu-id="b82e3-147">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="b82e3-148">Le `MaxLength` attribut fournit des fonctionnalités similaires à la `StringLength` d’attribut, mais ne fournit pas côté client validation.</span><span class="sxs-lookup"><span data-stu-id="b82e3-148">The `MaxLength` attribute provides functionality similar to the `StringLength` attribute but doesn't provide client side validation.</span></span>

<span data-ttu-id="b82e3-149">Le modèle de base de données a maintenant changé d’une manière qui nécessite une modification dans le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="b82e3-149">The database model has now changed in a way that requires a change in the database schema.</span></span> <span data-ttu-id="b82e3-150">Migrations vous permet de mettre à jour le schéma sans perdre de données que vous avez peut-être ajoutés à la base de données à l’aide de l’interface utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="b82e3-150">You'll use migrations to update the schema without losing any data that you may have added to the database by using the application UI.</span></span>

<span data-ttu-id="b82e3-151">Enregistrez vos modifications et générez le projet.</span><span class="sxs-lookup"><span data-stu-id="b82e3-151">Save your changes and build the project.</span></span> <span data-ttu-id="b82e3-152">Ouvrez la fenêtre de commande dans le dossier du projet, puis entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="b82e3-152">Then open the command window in the project folder and enter the following commands:</span></span>

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

<span data-ttu-id="b82e3-153">Le `migrations add` commande vous avertit qu’une perte de données peut-être se produire, car la modification la plus courte pour les deux colonnes de longueur maximale.</span><span class="sxs-lookup"><span data-stu-id="b82e3-153">The `migrations add` command warns that data loss may occur, because the change makes the maximum length shorter for two columns.</span></span>  <span data-ttu-id="b82e3-154">Migrations crée un fichier nommé  *\<timeStamp > _MaxLengthOnNames.cs*.</span><span class="sxs-lookup"><span data-stu-id="b82e3-154">Migrations creates a file named *\<timeStamp>_MaxLengthOnNames.cs*.</span></span> <span data-ttu-id="b82e3-155">Ce fichier contient le code dans la `Up` méthode qui met à jour la base de données correspond au modèle de données en cours.</span><span class="sxs-lookup"><span data-stu-id="b82e3-155">This file contains code in the `Up` method that will update the database to match the current data model.</span></span> <span data-ttu-id="b82e3-156">Le `database update` commande exécution de ce code.</span><span class="sxs-lookup"><span data-stu-id="b82e3-156">The `database update` command ran that code.</span></span>

<span data-ttu-id="b82e3-157">L’horodateur le préfixe au nom de fichier migrations est utilisée par Entity Framework pour classer les migrations.</span><span class="sxs-lookup"><span data-stu-id="b82e3-157">The timestamp prefixed to the migrations file name is used by Entity Framework to order the migrations.</span></span> <span data-ttu-id="b82e3-158">Vous pouvez créer plusieurs migrations avant d’exécuter la commande de base de données de mise à jour, puis toutes les migrations sont appliquées dans l’ordre dans lequel ils ont été créés.</span><span class="sxs-lookup"><span data-stu-id="b82e3-158">You can create multiple migrations before running the update-database command, and then all of the migrations are applied in the order in which they were created.</span></span>

<span data-ttu-id="b82e3-159">Exécuter l’application, sélectionnez le **étudiants** , cliquez sur **créer un nouveau**et entrez le nom de plus de 50 caractères.</span><span class="sxs-lookup"><span data-stu-id="b82e3-159">Run the app, select the **Students** tab, click **Create New**, and enter either name longer than 50 characters.</span></span> <span data-ttu-id="b82e3-160">Lorsque vous cliquez sur **créer**, validation côté client affiche un message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="b82e3-160">When you click **Create**, client side validation shows an error message.</span></span>

![Page affichant les erreurs de longueur de chaîne d’index les étudiants](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a><span data-ttu-id="b82e3-162">L’attribut de colonne</span><span class="sxs-lookup"><span data-stu-id="b82e3-162">The Column attribute</span></span>

<span data-ttu-id="b82e3-163">Vous pouvez également utiliser des attributs pour contrôler la façon dont les classes et les propriétés sont mappées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="b82e3-163">You can also use attributes to control how your classes and properties are mapped to the database.</span></span> <span data-ttu-id="b82e3-164">Supposons que vous aviez utilisé le nom `FirstMidName` pour le prénom, car le champ peut également contenir un deuxième prénom.</span><span class="sxs-lookup"><span data-stu-id="b82e3-164">Suppose you had used the name `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span> <span data-ttu-id="b82e3-165">Mais vous souhaitez que la colonne de base de données nommé `FirstName`, car les utilisateurs qui doit écrire des requêtes ad-hoc par rapport à la base de données sont habitués à ce nom.</span><span class="sxs-lookup"><span data-stu-id="b82e3-165">But you want the database column to be named `FirstName`, because users who will be writing ad-hoc queries against the database are accustomed to that name.</span></span> <span data-ttu-id="b82e3-166">Pour effectuer ce mappage, vous pouvez utiliser la `Column` attribut.</span><span class="sxs-lookup"><span data-stu-id="b82e3-166">To make this mapping, you can use the `Column` attribute.</span></span>

<span data-ttu-id="b82e3-167">Le `Column` attribut spécifie que lorsque la base de données est créé, la colonne de la `Student` table qui mappe à la `FirstMidName` propriété sera nommée `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="b82e3-167">The `Column` attribute specifies that when the database is created, the column of the `Student` table that maps to the `FirstMidName` property will be named `FirstName`.</span></span> <span data-ttu-id="b82e3-168">En d’autres termes, lorsque votre code fait référence à `Student.FirstMidName`, les données proviennent ou être mis à jour dans le `FirstName` colonne de la `Student` table.</span><span class="sxs-lookup"><span data-stu-id="b82e3-168">In other words, when your code refers to `Student.FirstMidName`, the data will come from or be updated in the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="b82e3-169">Si vous ne spécifiez pas les noms de colonnes, donnés le même nom que le nom de propriété.</span><span class="sxs-lookup"><span data-stu-id="b82e3-169">If you don't specify column names, they're given the same name as the property name.</span></span>

<span data-ttu-id="b82e3-170">Dans le *Student.cs* , ajoutez un `using` instruction pour `System.ComponentModel.DataAnnotations.Schema` et ajoutez l’attribut de nom de colonne à la `FirstMidName` la propriété, comme indiqué dans le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="b82e3-170">In the *Student.cs* file, add a `using` statement for `System.ComponentModel.DataAnnotations.Schema` and add the column name attribute to the `FirstMidName` property, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="b82e3-171">L’ajout de la `Column` attribut modifie la sauvegarde du modèle le `SchoolContext`, donc il ne correspond pas à la base de données.</span><span class="sxs-lookup"><span data-stu-id="b82e3-171">The addition of the `Column` attribute changes the model backing the `SchoolContext`, so it won't match the database.</span></span>

<span data-ttu-id="b82e3-172">Enregistrez vos modifications et générez le projet.</span><span class="sxs-lookup"><span data-stu-id="b82e3-172">Save your changes and build the project.</span></span> <span data-ttu-id="b82e3-173">Ouvrez la fenêtre de commande dans le dossier du projet, puis entrez les commandes suivantes pour créer une autre migration :</span><span class="sxs-lookup"><span data-stu-id="b82e3-173">Then open the command window in the project folder and enter the following commands to create another migration:</span></span>

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

<span data-ttu-id="b82e3-174">Dans **l’Explorateur d’objets SQL Server**, ouvrez le Concepteur de tables Student en double-cliquant sur le **étudiant** table.</span><span class="sxs-lookup"><span data-stu-id="b82e3-174">In **SQL Server Object Explorer**, open the Student table designer by double-clicking the **Student** table.</span></span>

![Table d’étudiants dans SSOX après la migration](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="b82e3-176">Avant d’appliquer les deux premières migrations, les colonnes de nom sont de type nvarchar (max).</span><span class="sxs-lookup"><span data-stu-id="b82e3-176">Before you applied the first two migrations, the name columns were of type nvarchar(MAX).</span></span> <span data-ttu-id="b82e3-177">Ils sont maintenant nvarchar (50) et le nom de colonne a été modifiée à partir de FirstMidName FirstName.</span><span class="sxs-lookup"><span data-stu-id="b82e3-177">They're now nvarchar(50) and the column name has changed from FirstMidName to FirstName.</span></span>

> [!Note]
> <span data-ttu-id="b82e3-178">Si vous essayez de compiler avant de terminer la création de toutes les classes d’entité dans les sections suivantes, vous pouvez obtenir les erreurs du compilateur.</span><span class="sxs-lookup"><span data-stu-id="b82e3-178">If you try to compile before you finish creating all of the entity classes in the following sections, you might get compiler errors.</span></span>

## <a name="final-changes-to-the-student-entity"></a><span data-ttu-id="b82e3-179">Modifications finales à l’entité Student</span><span class="sxs-lookup"><span data-stu-id="b82e3-179">Final changes to the Student entity</span></span>

![Entité Student](complex-data-model/_static/student-entity.png)

<span data-ttu-id="b82e3-181">Dans *Models/Student.cs*, remplacez le code que vous avez ajouté précédemment par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="b82e3-181">In *Models/Student.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="b82e3-182">Les modifications sont mises en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="b82e3-182">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="b82e3-183">L’attribut requis</span><span class="sxs-lookup"><span data-stu-id="b82e3-183">The Required attribute</span></span>

<span data-ttu-id="b82e3-184">Le `Required` attribut rend les champs obligatoires de propriétés de nom.</span><span class="sxs-lookup"><span data-stu-id="b82e3-184">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="b82e3-185">Le `Required` attribut n’est pas nécessaire pour les types non nullable tels que des types valeur (DateTime, int, double, float, etc..).</span><span class="sxs-lookup"><span data-stu-id="b82e3-185">The `Required` attribute isn't needed for non-nullable types such as value types (DateTime, int, double, float, etc.).</span></span> <span data-ttu-id="b82e3-186">Les types qui ne peut pas être null sont traitées automatiquement en tant que champs requis.</span><span class="sxs-lookup"><span data-stu-id="b82e3-186">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="b82e3-187">Vous pouvez supprimer la `Required` d’attribut et les remplacer par un paramètre de longueur minimale pour le `StringLength` attribut :</span><span class="sxs-lookup"><span data-stu-id="b82e3-187">You could remove the `Required` attribute and replace it with a minimum length parameter for the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="b82e3-188">L’attribut d’affichage</span><span class="sxs-lookup"><span data-stu-id="b82e3-188">The Display attribute</span></span>

<span data-ttu-id="b82e3-189">Le `Display` attribut spécifie que la légende pour les zones de texte doit être « Prénom », « Last Name », « Nom complet » et « Date d’inscription », au lieu du nom de propriété dans chaque instance (qui n’a pas les mots de la division d’espace).</span><span class="sxs-lookup"><span data-stu-id="b82e3-189">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="b82e3-190">La propriété FullName calculée</span><span class="sxs-lookup"><span data-stu-id="b82e3-190">The FullName calculated property</span></span>

<span data-ttu-id="b82e3-191">`FullName`est une propriété calculée qui retourne une valeur qui est obtenue par concaténation de deux autres propriétés.</span><span class="sxs-lookup"><span data-stu-id="b82e3-191">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="b82e3-192">Par conséquent, il a uniquement un accesseur get et non `FullName` colonne sera générée dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b82e3-192">Therefore it has only a get accessor, and no `FullName` column will be generated in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="b82e3-193">Créer l’entité Instructor</span><span class="sxs-lookup"><span data-stu-id="b82e3-193">Create the Instructor Entity</span></span>

![Entité Instructor](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="b82e3-195">Créer *Models/Instructor.cs*, en remplaçant le code du modèle avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b82e3-195">Create *Models/Instructor.cs*, replacing the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="b82e3-196">Notez que plusieurs propriétés sont identiques dans les entités Student et formateur.</span><span class="sxs-lookup"><span data-stu-id="b82e3-196">Notice that several properties are the same in the Student and Instructor entities.</span></span> <span data-ttu-id="b82e3-197">Dans le [implémentation de l’héritage](inheritance.md) didacticiel plus loin dans cette série, vous devez refactoriser ce code pour éliminer la redondance.</span><span class="sxs-lookup"><span data-stu-id="b82e3-197">In the [Implementing Inheritance](inheritance.md) tutorial later in this series, you'll refactor this code to eliminate the redundancy.</span></span>

<span data-ttu-id="b82e3-198">Vous pouvez placer plusieurs attributs sur une seule ligne, afin que vous pouvez également écrire le `HireDate` attributs comme suit :</span><span class="sxs-lookup"><span data-stu-id="b82e3-198">You can put multiple attributes on one line, so you could also write the `HireDate` attributes as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="b82e3-199">Les propriétés de navigation CourseAssignments et OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="b82e3-199">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="b82e3-200">Le `CourseAssignments` et `OfficeAssignment` propriétés sont des propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="b82e3-200">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="b82e3-201">Formateur pouvez animer n’importe quel nombre de cours, de sorte que `CourseAssignments` est défini comme une collection.</span><span class="sxs-lookup"><span data-stu-id="b82e3-201">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="b82e3-202">Si une propriété de navigation peut contenir plusieurs entités, son type doit être une liste dans laquelle les entrées peuvent être ajoutées, supprimées et mis à jour.</span><span class="sxs-lookup"><span data-stu-id="b82e3-202">If a navigation property can hold multiple entities, its type must be a list in which entries can be added, deleted, and updated.</span></span>  <span data-ttu-id="b82e3-203">Vous pouvez spécifier `ICollection<T>` ou un type tel que `List<T>` ou `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="b82e3-203">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="b82e3-204">Si vous spécifiez `ICollection<T>`, EF crée un `HashSet<T>` collection par défaut.</span><span class="sxs-lookup"><span data-stu-id="b82e3-204">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="b82e3-205">La raison pour laquelle ils sont `CourseAssignment` entités sont expliquées ci-dessous dans la section sur les relations plusieurs-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="b82e3-205">The reason why these are `CourseAssignment` entities is explained below in the section about many-to-many relationships.</span></span>

<span data-ttu-id="b82e3-206">L’état des règles d’entreprise Contoso University qu’un formateur peut avoir uniquement au plus un bureau, donc la `OfficeAssignment` propriété contient une seule entité OfficeAssignment (qui peut être null si aucune office n’est affectée).</span><span class="sxs-lookup"><span data-stu-id="b82e3-206">Contoso University business rules state that an instructor can only have at most one office, so the `OfficeAssignment` property holds a single OfficeAssignment entity (which may be null if no office is assigned).</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="b82e3-207">Créer l’entité OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="b82e3-207">Create the OfficeAssignment entity</span></span>

![Entité OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="b82e3-209">Créer *Models/OfficeAssignment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b82e3-209">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="b82e3-210">L’attribut de clé</span><span class="sxs-lookup"><span data-stu-id="b82e3-210">The Key attribute</span></span>

<span data-ttu-id="b82e3-211">Il existe une relation un-à-zéro-ou-un entre le formateur et les entités OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="b82e3-211">There's a one-to-zero-or-one relationship  between the Instructor and the OfficeAssignment entities.</span></span> <span data-ttu-id="b82e3-212">Une attribution existe uniquement en relation avec le formateur, à qu'il est assigné, et par conséquent sa clé primaire est également sa clé étrangère vers l’entité Instructor.</span><span class="sxs-lookup"><span data-stu-id="b82e3-212">An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the Instructor entity.</span></span> <span data-ttu-id="b82e3-213">Mais Entity Framework ne peut pas reconnaître automatiquement InstructorID comme clé primaire de cette entité, car son nom ne suit pas la convention d’affectation de noms ID ou classnameID.</span><span class="sxs-lookup"><span data-stu-id="b82e3-213">But the Entity Framework can't automatically recognize InstructorID as the primary key of this entity because its name doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="b82e3-214">Par conséquent, le `Key` attribut est utilisé pour identifier en tant que la clé :</span><span class="sxs-lookup"><span data-stu-id="b82e3-214">Therefore, the `Key` attribute is used to identify it as the key:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="b82e3-215">Vous pouvez également utiliser le `Key` si l’entité n’a pas sa propre clé primaire, mais que vous souhaitez la propriété donner un nom autre que classnameID ou ID d’attribut</span><span class="sxs-lookup"><span data-stu-id="b82e3-215">You can also use the `Key` attribute if the entity does have its own primary key but you want to name the property something other than classnameID or ID.</span></span>

<span data-ttu-id="b82e3-216">Par défaut, EF traite la touche non généré à la base de données, car la colonne est pour une relation d’identification.</span><span class="sxs-lookup"><span data-stu-id="b82e3-216">By default, EF treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="b82e3-217">La propriété de navigation de formateur</span><span class="sxs-lookup"><span data-stu-id="b82e3-217">The Instructor navigation property</span></span>

<span data-ttu-id="b82e3-218">Entité Instructor a nullable `OfficeAssignment` propriété de navigation (parce qu’un formateur n’est peut-être pas une assignation office), et l’entité OfficeAssignment a non-nullable `Instructor` propriété de navigation (car une attribution ne peut pas exister sans un formateur-- `InstructorID` non-null).</span><span class="sxs-lookup"><span data-stu-id="b82e3-218">The Instructor entity has a nullable `OfficeAssignment` navigation property (because an instructor might not have an office assignment), and the OfficeAssignment entity has a non-nullable `Instructor` navigation property (because an office assignment can't exist without an instructor -- `InstructorID` is non-nullable).</span></span> <span data-ttu-id="b82e3-219">Lorsqu’une entité Instructor possède une entité OfficeAssignment associée, chaque entité a une référence à l’autre dans sa propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="b82e3-219">When an Instructor entity has a related OfficeAssignment entity, each entity will have a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="b82e3-220">Vous pouvez placer un `[Required]` attribut sur la propriété de navigation du formateur pour spécifier qu’il doit y avoir un formateur connexe, mais vous n’êtes pas obligé de le faire, car le `InstructorID` clé étrangère (qui est également la clé pour cette table) est non nullable.</span><span class="sxs-lookup"><span data-stu-id="b82e3-220">You could put a `[Required]` attribute on the Instructor navigation property to specify that there must be a related instructor, but you don't have to do that because the `InstructorID` foreign key (which is also the key to this table) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="b82e3-221">Modifier l’entité de cours</span><span class="sxs-lookup"><span data-stu-id="b82e3-221">Modify the Course Entity</span></span>

![Entité de cours](complex-data-model/_static/course-entity.png)

<span data-ttu-id="b82e3-223">Dans *Models/Course.cs*, remplacez le code que vous avez ajouté précédemment par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="b82e3-223">In *Models/Course.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="b82e3-224">Les modifications sont mises en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="b82e3-224">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="b82e3-225">L’entité de cours possède une propriété de clé étrangère `DepartmentID` qui pointe vers l’entité de service associée et il a un `Department` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="b82e3-225">The course entity has a foreign key property `DepartmentID` which points to the related Department entity and it has a `Department` navigation property.</span></span>

<span data-ttu-id="b82e3-226">Entity Framework ne requiert pas vous permet d’ajouter une propriété de clé étrangère à votre modèle de données lorsque vous disposez d’une propriété de navigation pour une entité associée.</span><span class="sxs-lookup"><span data-stu-id="b82e3-226">The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity.</span></span>  <span data-ttu-id="b82e3-227">EF crée des clés étrangères dans la base de données partout où ils sont nécessaires et crée automatiquement [occulter les propriétés](https://docs.microsoft.com/ef/core/modeling/shadow-properties) pour eux.</span><span class="sxs-lookup"><span data-stu-id="b82e3-227">EF automatically creates foreign keys in the database wherever they're needed and creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for them.</span></span> <span data-ttu-id="b82e3-228">Mais ayant la clé étrangère dans le modèle de données peut rendre les mises à jour plus simple et plus efficace.</span><span class="sxs-lookup"><span data-stu-id="b82e3-228">But having the foreign key in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="b82e3-229">Par exemple, lorsque vous lisez une entité de cours à modifier, l’entité du service est null si vous ne le charge pas, par conséquent, lorsque vous mettez à jour l’entité de cours, vous devez tout d’abord récupérer l’entité du service.</span><span class="sxs-lookup"><span data-stu-id="b82e3-229">For example, when you fetch a course entity to edit, the  Department entity is null if you don't load it, so when you update the course entity, you would have to first fetch the Department entity.</span></span> <span data-ttu-id="b82e3-230">Lorsque la propriété de clé étrangère `DepartmentID` est inclus dans le modèle de données, vous n’avez pas besoin récupérer l’entité du service avant de mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="b82e3-230">When the foreign key property `DepartmentID` is included in the data model, you don't need to fetch the Department entity before you update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="b82e3-231">L’attribut DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="b82e3-231">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="b82e3-232">Le `DatabaseGenerated` d’attribut avec le `None` paramètre sur le `CourseID` propriété spécifie que les valeurs de clé primaire sont fournies par l’utilisateur au lieu de générés par la base de données.</span><span class="sxs-lookup"><span data-stu-id="b82e3-232">The `DatabaseGenerated` attribute with the `None` parameter on the `CourseID` property specifies that primary key values are provided by the user rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="b82e3-233">Par défaut, Entity Framework suppose que les valeurs de clés primaires sont générées par la base de données.</span><span class="sxs-lookup"><span data-stu-id="b82e3-233">By default, Entity Framework assumes that primary key values are generated by the database.</span></span> <span data-ttu-id="b82e3-234">C’est ce que vous souhaitez dans la plupart des scénarios.</span><span class="sxs-lookup"><span data-stu-id="b82e3-234">That's what you want in most scenarios.</span></span> <span data-ttu-id="b82e3-235">Toutefois, pour les entités de cours, vous allez utiliser un numéro spécifié par l’utilisateur comme une série de 1000 pour un service, une série de 2000 pour un autre service et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="b82e3-235">However, for Course entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.</span></span>

<span data-ttu-id="b82e3-236">Le `DatabaseGenerated` attribut peut également être utilisé pour générer des valeurs par défaut, dans le cas des colonnes de base de données utilisées pour enregistrer la date d’une ligne a été créée ou mis à jour.</span><span class="sxs-lookup"><span data-stu-id="b82e3-236">The `DatabaseGenerated` attribute can also be used to generate default values, as in the case of database columns used to record the date a row was created or updated.</span></span>  <span data-ttu-id="b82e3-237">Pour plus d’informations, consultez [généré des propriétés](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="b82e3-237">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="b82e3-238">Propriétés de navigation et de clé étrangère</span><span class="sxs-lookup"><span data-stu-id="b82e3-238">Foreign key and navigation properties</span></span>

<span data-ttu-id="b82e3-239">Les propriétés de clé étrangère et les propriétés de navigation dans l’entité de cours reflètent les relations suivantes :</span><span class="sxs-lookup"><span data-stu-id="b82e3-239">The foreign key properties and navigation properties in the Course entity reflect the following relationships:</span></span>

<span data-ttu-id="b82e3-240">Un cours est affecté à un service, donc il est un `DepartmentID` clé étrangère et un `Department` propriété de navigation pour les raisons mentionnées ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="b82e3-240">A course is assigned to one department, so there's a `DepartmentID` foreign key and a `Department` navigation property for the reasons mentioned above.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="b82e3-241">Un cours peut avoir n’importe quel nombre d’élèves inscrits, par conséquent, le `Enrollments` propriété de navigation est une collection :</span><span class="sxs-lookup"><span data-stu-id="b82e3-241">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="b82e3-242">Un cours peut être dirigée par des instructeurs plusieurs, donc la `CourseAssignments` propriété de navigation est une collection (le type `CourseAssignment` est expliquée [ultérieurement](#many-to-many-relationships)) :</span><span class="sxs-lookup"><span data-stu-id="b82e3-242">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection (the type `CourseAssignment` is explained [later](#many-to-many-relationships)):</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a><span data-ttu-id="b82e3-243">Créer l’entité du service</span><span class="sxs-lookup"><span data-stu-id="b82e3-243">Create the Department entity</span></span>

![Entité de service](complex-data-model/_static/department-entity.png)


<span data-ttu-id="b82e3-245">Créer *Models/Department.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b82e3-245">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="b82e3-246">L’attribut de colonne</span><span class="sxs-lookup"><span data-stu-id="b82e3-246">The Column attribute</span></span>

<span data-ttu-id="b82e3-247">Précédemment, vous avez utilisé le `Column` attribut pour modifier le mappage de nom de colonne.</span><span class="sxs-lookup"><span data-stu-id="b82e3-247">Earlier you used the `Column` attribute to change column name mapping.</span></span> <span data-ttu-id="b82e3-248">Dans le code de l’entité du service, le `Column` attribut sert à modifier SQL type correspondance de données afin que la colonne sera définie à l’aide du type de money SQL Server dans la base de données :</span><span class="sxs-lookup"><span data-stu-id="b82e3-248">In the code for the Department entity, the `Column` attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="b82e3-249">Mappage de colonnes n’est généralement pas nécessaire, car l’Entity Framework choisit le type de données SQL Server approprié en fonction du type CLR que vous définissez pour la propriété.</span><span class="sxs-lookup"><span data-stu-id="b82e3-249">Column mapping is generally not required, because the Entity Framework chooses the appropriate SQL Server data type based on the CLR type that you define for the property.</span></span> <span data-ttu-id="b82e3-250">Le CLR `decimal` type correspond à un serveur SQL Server `decimal` type.</span><span class="sxs-lookup"><span data-stu-id="b82e3-250">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="b82e3-251">Mais dans ce cas vous savez que la colonne organiserons des montants en devise et le type de données money est plus adapté.</span><span class="sxs-lookup"><span data-stu-id="b82e3-251">But in this case you know that the column will be holding currency amounts, and the money data type is more appropriate for that.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="b82e3-252">Propriétés de navigation et de clé étrangère</span><span class="sxs-lookup"><span data-stu-id="b82e3-252">Foreign key and navigation properties</span></span>

<span data-ttu-id="b82e3-253">Les propriétés de navigation et de clé étrangère reflètent les relations suivantes :</span><span class="sxs-lookup"><span data-stu-id="b82e3-253">The foreign key and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="b82e3-254">Un service peut ou ne peut pas avoir un administrateur, et un administrateur est toujours un formateur.</span><span class="sxs-lookup"><span data-stu-id="b82e3-254">A department may or may not have an administrator, and an administrator is always an instructor.</span></span> <span data-ttu-id="b82e3-255">Par conséquent la `InstructorID` propriété n’est incluse en tant que clé étrangère à l’entité Instructor, et un point d’interrogation est ajouté après le `int` désignation pour marquer la propriété Nullable de type.</span><span class="sxs-lookup"><span data-stu-id="b82e3-255">Therefore the `InstructorID` property is included as the foreign key to the Instructor entity, and a question mark is added after the `int` type designation to mark the property as nullable.</span></span> <span data-ttu-id="b82e3-256">La propriété de navigation est nommée `Administrator` , mais sa valeur est une entité Instructor :</span><span class="sxs-lookup"><span data-stu-id="b82e3-256">The navigation property is named `Administrator` but holds an Instructor entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="b82e3-257">Un service peut avoir de nombreux cours, est une propriété de navigation du cours :</span><span class="sxs-lookup"><span data-stu-id="b82e3-257">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> <span data-ttu-id="b82e3-258">Par convention, Entity Framework permet la suppression en cascade pour les clés étrangères non nullables et pour les relations plusieurs-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="b82e3-258">By convention, the Entity Framework enables cascade delete for non-nullable foreign keys and for many-to-many relationships.</span></span> <span data-ttu-id="b82e3-259">Cela peut entraîner des règles de suppression en cascade circulaire, ce qui provoquent une exception lorsque vous essayez d’ajouter une migration.</span><span class="sxs-lookup"><span data-stu-id="b82e3-259">This can result in circular cascade delete rules, which will cause an exception when you try to add a migration.</span></span> <span data-ttu-id="b82e3-260">Par exemple, si vous n’avez pas défini la propriété Department.InstructorID Nullable, EF serait configurer une règle de suppression en cascade pour supprimer le formateur lorsque vous supprimez le service, qui n’est pas de ce que vous voulez effectuer.</span><span class="sxs-lookup"><span data-stu-id="b82e3-260">For example, if you didn't define the Department.InstructorID property as nullable, EF would configure a cascade delete rule to delete the instructor when you delete the department, which isn't what you want to have happen.</span></span> <span data-ttu-id="b82e3-261">Si vos règles d’entreprise nécessaire le `InstructorID` propriété soit non nullable, vous devez utiliser l’instruction API fluent suivante pour désactiver la suppression en cascade sur la relation :</span><span class="sxs-lookup"><span data-stu-id="b82e3-261">If your business rules required the `InstructorID` property to be non-nullable, you would have to use the following fluent API statement to disable cascade delete on the relationship:</span></span>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a><span data-ttu-id="b82e3-262">Modifier l’entité de l’inscription</span><span class="sxs-lookup"><span data-stu-id="b82e3-262">Modify the Enrollment entity</span></span>

![Entité de l’inscription](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="b82e3-264">Dans *Models/Enrollment.cs*, remplacez le code que vous avez ajouté précédemment par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b82e3-264">In *Models/Enrollment.cs*, replace the code you added earlier with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="b82e3-265">Propriétés de navigation et de clé étrangère</span><span class="sxs-lookup"><span data-stu-id="b82e3-265">Foreign key and navigation properties</span></span>

<span data-ttu-id="b82e3-266">Les propriétés de clé étrangère et les propriétés de navigation reflètent les relations suivantes :</span><span class="sxs-lookup"><span data-stu-id="b82e3-266">The foreign key properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="b82e3-267">Un enregistrement d’inscription est un cours unique, donc il est un `CourseID` propriété de clé étrangère et un `Course` propriété de navigation :</span><span class="sxs-lookup"><span data-stu-id="b82e3-267">An enrollment record is for a single course, so there's a `CourseID` foreign key property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="b82e3-268">Un enregistrement d’inscription est pour un étudiant unique, donc il est un `StudentID` propriété de clé étrangère et un `Student` propriété de navigation :</span><span class="sxs-lookup"><span data-stu-id="b82e3-268">An enrollment record is for a single student, so there's a `StudentID` foreign key property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="b82e3-269">Relations plusieurs-à-plusieurs</span><span class="sxs-lookup"><span data-stu-id="b82e3-269">Many-to-Many Relationships</span></span>

<span data-ttu-id="b82e3-270">Il existe une relation plusieurs-à-plusieurs entre les entités étudiant et cours et l’entité d’inscription fonctionne comme une table de jointure plusieurs-à-plusieurs *avec une charge utile* dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b82e3-270">There's a many-to-many relationship between the Student and Course entities, and the Enrollment entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="b82e3-271">« Avec une charge utile » signifie que la table Enrollment contient des données supplémentaires en plus des clés étrangères des tables jointes (dans ce cas, une clé primaire et une propriété de niveau).</span><span class="sxs-lookup"><span data-stu-id="b82e3-271">"With payload" means that the Enrollment table contains additional data besides foreign keys for the joined tables (in this case, a primary key and a Grade property).</span></span>

<span data-ttu-id="b82e3-272">L’illustration suivante montre l’aspect de ces relations dans un diagramme d’entité.</span><span class="sxs-lookup"><span data-stu-id="b82e3-272">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="b82e3-273">(Ce diagramme a été généré à l’aide de l’Entity Framework Power Tools pour EF 6.x ; création du diagramme ne fait pas partie de ce didacticiel, il est uniquement utilisé ici à titre d’illustration.)</span><span class="sxs-lookup"><span data-stu-id="b82e3-273">(This diagram was generated using the Entity Framework Power Tools for EF 6.x; creating the diagram isn't part of the tutorial, it's just being used here as an illustration.)</span></span>

![Cours de l’étudiant plusieurs à plusieurs relations](complex-data-model/_static/student-course.png)

<span data-ttu-id="b82e3-275">Chaque ligne de relation comporte un 1 à une extrémité et un astérisque (\*) à l’autre, qui indique une relation un-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="b82e3-275">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="b82e3-276">Si la table Enrollment n’a pas été incluent des informations de catégorie, il faudrait uniquement contenir les deux clés étrangères CourseID et StudentID.</span><span class="sxs-lookup"><span data-stu-id="b82e3-276">If the Enrollment table didn't include grade information, it would only need to contain the two foreign keys CourseID and StudentID.</span></span> <span data-ttu-id="b82e3-277">Dans ce cas, il serait une table de jointure plusieurs-à-plusieurs sans la charge utile (ou une table de jonction pure) dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b82e3-277">In that case, it would be a many-to-many join table without payload (or a pure join table) in the database.</span></span> <span data-ttu-id="b82e3-278">Les entités du formateur et de cours ont ce type de relation plusieurs-à-plusieurs, et l’étape suivante consiste à créer une classe d’entité pour fonctionner comme une table de jonction sans la charge utile.</span><span class="sxs-lookup"><span data-stu-id="b82e3-278">The Instructor and Course entities have that kind of many-to-many relationship, and your next step is to create an entity class to function as a join table without payload.</span></span>

<span data-ttu-id="b82e3-279">(EF 6.x prend en charge les tables de jointure implicite pour les relations plusieurs-à-plusieurs, mais EF Core ne.</span><span class="sxs-lookup"><span data-stu-id="b82e3-279">(EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="b82e3-280">Pour plus d’informations, consultez la [discussion dans le référentiel GitHub de noyaux EF](https://github.com/aspnet/EntityFramework/issues/1368).)</span><span class="sxs-lookup"><span data-stu-id="b82e3-280">For more information, see the [discussion in the EF Core GitHub repository](https://github.com/aspnet/EntityFramework/issues/1368).)</span></span> 

## <a name="the-courseassignment-entity"></a><span data-ttu-id="b82e3-281">L’entité CourseAssignment</span><span class="sxs-lookup"><span data-stu-id="b82e3-281">The CourseAssignment entity</span></span>

![Entité de CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="b82e3-283">Créer *Models/CourseAssignment.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b82e3-283">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a><span data-ttu-id="b82e3-284">Joindre des noms d’entité</span><span class="sxs-lookup"><span data-stu-id="b82e3-284">Join entity names</span></span>

<span data-ttu-id="b82e3-285">Une table de jointure est requis dans la base de données pour la relation plusieurs-à-plusieurs de formateur à cours, et il doit être représenté par un jeu d’entités.</span><span class="sxs-lookup"><span data-stu-id="b82e3-285">A join table is required in the database for the Instructor-to-Courses many-to-many relationship, and it has to be represented by an entity set.</span></span> <span data-ttu-id="b82e3-286">Il est courant de nommer une entité de jointure `EntityName1EntityName2`, ce qui serait dans ce cas `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="b82e3-286">It's common to name a join entity `EntityName1EntityName2`, which in this case would be `CourseInstructor`.</span></span> <span data-ttu-id="b82e3-287">Toutefois, nous vous recommandons de choisir un nom qui décrit la relation.</span><span class="sxs-lookup"><span data-stu-id="b82e3-287">However, we recommend that you choose a name that describes the relationship.</span></span> <span data-ttu-id="b82e3-288">Modèles de données démarrent simple et la croissance, avec des jointures non-charge fréquemment mise en route charges utiles ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="b82e3-288">Data models start out simple and grow, with no-payload joins frequently getting payloads later.</span></span> <span data-ttu-id="b82e3-289">Si vous démarrez avec un nom descriptif d’entité, vous n’aurez pas à le modifier ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="b82e3-289">If you start with a descriptive entity name, you won't have to change the name later.</span></span> <span data-ttu-id="b82e3-290">Dans l’idéal, l’entité de jointure aurait son propre nom (word éventuellement unique) naturelle dans le domaine d’entreprise.</span><span class="sxs-lookup"><span data-stu-id="b82e3-290">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="b82e3-291">Par exemple, la documentation et les clients pu être liées par le biais des évaluations.</span><span class="sxs-lookup"><span data-stu-id="b82e3-291">For example, Books and Customers could be linked through Ratings.</span></span> <span data-ttu-id="b82e3-292">Pour cette relation, `CourseAssignment` est un meilleur choix que `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="b82e3-292">For this relationship, `CourseAssignment` is a better choice than `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="b82e3-293">Clé composite</span><span class="sxs-lookup"><span data-stu-id="b82e3-293">Composite key</span></span>

<span data-ttu-id="b82e3-294">Étant donné que les clés étrangères ne sont pas nullables et ensemble identifie de façon unique identifier chaque ligne de la table, il n’est pas nécessaire pour une clé primaire distincte.</span><span class="sxs-lookup"><span data-stu-id="b82e3-294">Since the foreign keys are not nullable and together uniquely identify each row of the table, there's no need for a separate primary key.</span></span> <span data-ttu-id="b82e3-295">Le *InstructorID* et *CourseID* propriétés doivent fonctionner comme une clé primaire composite.</span><span class="sxs-lookup"><span data-stu-id="b82e3-295">The *InstructorID* and *CourseID* properties should function as a composite primary key.</span></span> <span data-ttu-id="b82e3-296">La seule façon pour identifier les clés primaires composites EF est à l’aide de la *API fluent* (il ne peut pas être effectuée à l’aide d’attributs).</span><span class="sxs-lookup"><span data-stu-id="b82e3-296">The only way to identify composite primary keys to EF is by using the *fluent API* (it can't be done by using attributes).</span></span> <span data-ttu-id="b82e3-297">Vous allez apprendre à configurer la clé primaire composite dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="b82e3-297">You'll see how to configure the composite primary key in the next section.</span></span>

<span data-ttu-id="b82e3-298">La clé composite permet de s’assurer que vous pouvez avoir plusieurs lignes pour un cours et plusieurs lignes pour un formateur, vous ne peut pas avoir plusieurs lignes pour le même formateur et de cours.</span><span class="sxs-lookup"><span data-stu-id="b82e3-298">The composite key ensures that while you can have multiple rows for one course, and multiple rows for one instructor, you can't have multiple rows for the same instructor and course.</span></span> <span data-ttu-id="b82e3-299">Le `Enrollment` jointure entité définit sa propre clé primaire, les doublons de ce type sont possibles.</span><span class="sxs-lookup"><span data-stu-id="b82e3-299">The `Enrollment` join entity defines its own primary key, so duplicates of this sort are possible.</span></span> <span data-ttu-id="b82e3-300">Pour éviter ces doublons, vous pourriez ajouter un index unique sur les champs de clé étrangères, ou configurez `Enrollment` avec une clé primaire composite similaire à `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="b82e3-300">To prevent such duplicates, you could add a unique index on the foreign key fields, or configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="b82e3-301">Pour plus d’informations, consultez [index](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="b82e3-301">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="b82e3-302">Mettre à jour le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="b82e3-302">Update the database context</span></span>

<span data-ttu-id="b82e3-303">Ajoutez le code en surbrillance suivant à la *Data/SchoolContext.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="b82e3-303">Add the following highlighted code to the *Data/SchoolContext.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="b82e3-304">Ce code ajoute de nouvelles entités et configure la clé primaire de l’entité CourseAssignment composite.</span><span class="sxs-lookup"><span data-stu-id="b82e3-304">This code adds the new entities and configures the CourseAssignment entity's composite primary key.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="b82e3-305">Alternative d’API Fluent aux attributs</span><span class="sxs-lookup"><span data-stu-id="b82e3-305">Fluent API alternative to attributes</span></span>

<span data-ttu-id="b82e3-306">Le code dans le `OnModelCreating` méthode de la `DbContext` classe utilise le *API fluent* pour configurer le comportement EF.</span><span class="sxs-lookup"><span data-stu-id="b82e3-306">The code in the `OnModelCreating` method of the `DbContext` class uses the *fluent API* to configure EF behavior.</span></span> <span data-ttu-id="b82e3-307">L’API est appelée « fluent », car il est souvent utilisée par tirage une série d’appels de méthode ensemble dans une instruction unique, comme dans cet exemple à partir de la [EF documentations](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span><span class="sxs-lookup"><span data-stu-id="b82e3-307">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement, as in this example from the [EF Core documentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="b82e3-308">Dans ce didacticiel, vous utilisez l’API fluent uniquement pour le mappage de base de données que vous ne pouvez pas faire avec des attributs.</span><span class="sxs-lookup"><span data-stu-id="b82e3-308">In this tutorial, you're using the fluent API only for database mapping that you can't do with attributes.</span></span> <span data-ttu-id="b82e3-309">Toutefois, vous pouvez également utiliser l’API fluent pour spécifier la majeure partie de la mise en forme, les règles de validation et mappage que vous pouvez effectuer à l’aide d’attributs.</span><span class="sxs-lookup"><span data-stu-id="b82e3-309">However, you can also use the fluent API to specify most of the formatting, validation, and mapping rules that you can do by using attributes.</span></span> <span data-ttu-id="b82e3-310">Certains attributs, tels que `MinimumLength` ne peut pas être appliqué avec l’API fluent.</span><span class="sxs-lookup"><span data-stu-id="b82e3-310">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="b82e3-311">Comme mentionné précédemment, `MinimumLength` ne change pas le schéma, elle s’applique uniquement une règle de validation côté client et le serveur.</span><span class="sxs-lookup"><span data-stu-id="b82e3-311">As mentioned previously, `MinimumLength` doesn't change the schema, it only applies a client and server side validation rule.</span></span>

<span data-ttu-id="b82e3-312">Certains développeurs préfèrent utiliser l’API fluent exclusivement afin qu’ils peuvent conserver leurs classes d’entité « propre ».</span><span class="sxs-lookup"><span data-stu-id="b82e3-312">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="b82e3-313">Vous pouvez combiner des attributs et des API fluent si vous voulez, et il existe quelques personnalisations peuvent uniquement être effectuées à l’aide des API fluent, mais en général la pratique recommandée consiste à choisir l’une de ces deux approches et utilisez ce constamment autant que possible.</span><span class="sxs-lookup"><span data-stu-id="b82e3-313">You can mix attributes and fluent API if you want, and there are a few customizations that can only be done by using fluent API, but in general the recommended practice is to choose one of these two approaches and use that consistently as much as possible.</span></span> <span data-ttu-id="b82e3-314">Si vous n’utilisez pas les deux, notez que partout où il existe un conflit, API Fluent remplace les attributs.</span><span class="sxs-lookup"><span data-stu-id="b82e3-314">If you do use both, note that wherever there's a conflict, Fluent API overrides attributes.</span></span>

<span data-ttu-id="b82e3-315">Pour plus d’informations sur les attributs et l’API fluent, consultez [méthodes de configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="b82e3-315">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="b82e3-316">Entité diagramme montrant les relations</span><span class="sxs-lookup"><span data-stu-id="b82e3-316">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="b82e3-317">L’illustration suivante montre le diagramme Entity Framework Power Tools créer pour le modèle School terminé.</span><span class="sxs-lookup"><span data-stu-id="b82e3-317">The following illustration shows the diagram that the Entity Framework Power Tools create for the completed School model.</span></span>

![Diagramme d’entité](complex-data-model/_static/diagram.png)

<span data-ttu-id="b82e3-319">Outre les lignes de relation un-à-plusieurs (1 à \*), vous pouvez voir ici la ligne de relation d’un-à-zéro-ou-un (1 à 0.. 1) entre les entités du formateur et OfficeAssignment et la ligne zéro-ou-relation un-à-plusieurs (0.. 1 à *) entre le Entités du formateur et de service.</span><span class="sxs-lookup"><span data-stu-id="b82e3-319">Besides the one-to-many relationship lines (1 to \*), you can see here the one-to-zero-or-one relationship line (1 to 0..1) between the Instructor and OfficeAssignment entities and the zero-or-one-to-many relationship line (0..1 to *) between the Instructor and Department entities.</span></span>

## <a name="seed-the-database-with-test-data"></a><span data-ttu-id="b82e3-320">Valeur initiale de la base de données de Test</span><span class="sxs-lookup"><span data-stu-id="b82e3-320">Seed the Database with Test Data</span></span>

<span data-ttu-id="b82e3-321">Remplacez le code dans le *Data/DbInitializer.cs* fichier avec le code suivant afin de fournir des données de la valeur de départ pour les nouvelles entités que vous avez créé.</span><span class="sxs-lookup"><span data-stu-id="b82e3-321">Replace the code in the *Data/DbInitializer.cs* file with the following code in order to provide seed data for the new entities you've created.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="b82e3-322">Comme vous l’avez vu dans le premier didacticiel, la majeure partie de ce code crée des objets d’entité simplement et charge les exemples de données dans les propriétés requises pour le test.</span><span class="sxs-lookup"><span data-stu-id="b82e3-322">As you saw in the first tutorial, most of this code simply creates new entity objects and loads sample data into properties as required for testing.</span></span> <span data-ttu-id="b82e3-323">Notez la façon dont sont gérées les relations plusieurs-à-plusieurs : le code crée des relations en créant des entités dans le `Enrollments` et `CourseAssignment` joindre des jeux d’entités.</span><span class="sxs-lookup"><span data-stu-id="b82e3-323">Notice how the many-to-many relationships are handled: the code creates relationships by creating entities in the `Enrollments` and `CourseAssignment` join entity sets.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="b82e3-324">Ajouter une migration</span><span class="sxs-lookup"><span data-stu-id="b82e3-324">Add a migration</span></span>

<span data-ttu-id="b82e3-325">Enregistrez vos modifications et générez le projet.</span><span class="sxs-lookup"><span data-stu-id="b82e3-325">Save your changes and build the project.</span></span> <span data-ttu-id="b82e3-326">Ouvrez la fenêtre de commande dans le dossier du projet, puis entrez la `migrations add` commande (ne le faites pas la commande de base de données de mise à jour encore) :</span><span class="sxs-lookup"><span data-stu-id="b82e3-326">Then open the command window in the project folder and enter the `migrations add` command (don't do the update-database command yet):</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="b82e3-327">Vous obtenez un avertissement concernant les pertes de données possibles.</span><span class="sxs-lookup"><span data-stu-id="b82e3-327">You get a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="b82e3-328">Si vous avez essayé d’exécuter le `database update` commande à ce stade (ne le faites pas encore), vous obtenez l’erreur suivante :</span><span class="sxs-lookup"><span data-stu-id="b82e3-328">If you tried to run the `database update` command at this point (don't do it yet), you would get the following error:</span></span>

> <span data-ttu-id="b82e3-329">L’instruction ALTER TABLE est en conflit avec la contrainte de clé étrangère « FK_dbo. Course_dbo. Department_DepartmentID ».</span><span class="sxs-lookup"><span data-stu-id="b82e3-329">The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID".</span></span> <span data-ttu-id="b82e3-330">Le conflit s’est produite dans la base de données « ContosoUniversity », table « dbo. Département », colonne « DepartmentID ».</span><span class="sxs-lookup"><span data-stu-id="b82e3-330">The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.</span></span>

<span data-ttu-id="b82e3-331">Parfois, lorsque vous exécutez des migrations avec des données existantes, vous devez insérer des données de stub dans la base de données pour répondre aux contraintes de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="b82e3-331">Sometimes when you execute migrations with existing data, you need to insert stub data into the database to satisfy foreign key constraints.</span></span> <span data-ttu-id="b82e3-332">Le code généré dans le `Up` méthode ajoute une clé étrangère DepartmentID non nullable pour la table Course.</span><span class="sxs-lookup"><span data-stu-id="b82e3-332">The generated code in the `Up` method adds a non-nullable DepartmentID foreign key to the Course table.</span></span> <span data-ttu-id="b82e3-333">S’il existe déjà lignes dans la table Course lorsque le code s’exécute, le `AddColumn` opération échoue, car SQL Server ne sait pas quelle valeur à placer dans la colonne ne peut pas être null.</span><span class="sxs-lookup"><span data-stu-id="b82e3-333">If there are already rows in the Course table when the code runs, the `AddColumn` operation fails because SQL Server doesn't know what value to put in the column that can't be null.</span></span> <span data-ttu-id="b82e3-334">Pour ce didacticiel, vous allez exécuter la migration sur une base de données, mais dans une application de production, vous devez effectuer la migration de gérer les données existantes, afin des instructions suivantes montrent un exemple de procédure à suivre.</span><span class="sxs-lookup"><span data-stu-id="b82e3-334">For this tutorial you'll run the migration on a new database, but in a production application you'd have to make the migration handle existing data, so the following directions show an example of how to do that.</span></span>

<span data-ttu-id="b82e3-335">Pour rendre cette migration fonctionne avec des données existantes, que vous devez modifier le code pour permettre à la nouvelle colonne à une valeur par défaut et créer un service de stub nommé « Temp » en tant que le service par défaut.</span><span class="sxs-lookup"><span data-stu-id="b82e3-335">To make this migration work with existing data you have to change the code to give the new column a default value, and create a stub department named "Temp" to act as the default department.</span></span> <span data-ttu-id="b82e3-336">Par conséquent, cours lignes existantes sont tous les liées au service « Temp » après le `Up` s’exécute la méthode.</span><span class="sxs-lookup"><span data-stu-id="b82e3-336">As a result, existing Course rows will all be related to the "Temp" department after the `Up` method runs.</span></span>

* <span data-ttu-id="b82e3-337">Ouvrez le *{timestamp}_ComplexDataModel.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="b82e3-337">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span> 

* <span data-ttu-id="b82e3-338">Commentez la ligne de code qui ajoute la colonne DepartmentID de la table Course.</span><span class="sxs-lookup"><span data-stu-id="b82e3-338">Comment out the line of code that adds the DepartmentID column to the Course table.</span></span>

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* <span data-ttu-id="b82e3-339">Ajoutez le code en surbrillance suivant après le code qui crée la table Department :</span><span class="sxs-lookup"><span data-stu-id="b82e3-339">Add the following highlighted code after the code that creates the Department table:</span></span>

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="b82e3-340">Dans une application de production, vous devez écrire le code ou des scripts pour ajouter des lignes de service et de relations avec les lignes de cours pour les nouvelles lignes de service.</span><span class="sxs-lookup"><span data-stu-id="b82e3-340">In a production application, you would write code or scripts to add Department rows and relate Course rows to the new Department rows.</span></span> <span data-ttu-id="b82e3-341">Vous ne sont plus devez alors le service « Temp » ou la valeur par défaut sur la colonne Course.DepartmentID.</span><span class="sxs-lookup"><span data-stu-id="b82e3-341">You would then no longer need the "Temp" department or the default value on the Course.DepartmentID column.</span></span>

<span data-ttu-id="b82e3-342">Enregistrez vos modifications et générez le projet.</span><span class="sxs-lookup"><span data-stu-id="b82e3-342">Save your changes and build the project.</span></span>

## <a name="change-the-connection-string-and-update-the-database"></a><span data-ttu-id="b82e3-343">Modifier la chaîne de connexion et de mettre à jour de la base de données</span><span class="sxs-lookup"><span data-stu-id="b82e3-343">Change the connection string and update the database</span></span>

<span data-ttu-id="b82e3-344">Vous avez maintenant nouveau code la `DbInitializer` classe qui ajoute des données de départ pour les nouvelles entités à une base de données vide.</span><span class="sxs-lookup"><span data-stu-id="b82e3-344">You now have new code in the `DbInitializer` class that adds seed data for the new entities to an empty database.</span></span> <span data-ttu-id="b82e3-345">Pour rendre EF à créer une base de données vide, modifier le nom de la base de données dans la chaîne de connexion dans *appsettings.json* ContosoUniversity3 ou un autre nom que vous avez utilisé sur l’ordinateur que vous utilisez.</span><span class="sxs-lookup"><span data-stu-id="b82e3-345">To make EF create a new empty database, change the name of the database in the connection string in *appsettings.json* to ContosoUniversity3 or some other name that you haven't used on the computer you're using.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

<span data-ttu-id="b82e3-346">Enregistrer les modifications à votre *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b82e3-346">Save your change to *appsettings.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="b82e3-347">Comme alternative à la modification du nom de la base de données, vous pouvez supprimer la base de données.</span><span class="sxs-lookup"><span data-stu-id="b82e3-347">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="b82e3-348">Utilisez **l’Explorateur d’objets SQL Server** (SSOX) ou le `database drop` commande CLI :</span><span class="sxs-lookup"><span data-stu-id="b82e3-348">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```

<span data-ttu-id="b82e3-349">Une fois que vous avez modifié le nom de la base de données ou supprimés de la base de données, exécuter le `database update` dans la fenêtre de commande à exécuter les migrations.</span><span class="sxs-lookup"><span data-stu-id="b82e3-349">After you have changed the database name or deleted the database, run the `database update` command in the command window to execute the migrations.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="b82e3-350">Exécuter l’application pour provoquer le `DbInitializer.Initialize` méthode à exécuter et de remplir la nouvelle base de données.</span><span class="sxs-lookup"><span data-stu-id="b82e3-350">Run the app to cause the `DbInitializer.Initialize` method to run and populate the new database.</span></span>

<span data-ttu-id="b82e3-351">Ouvrez la base de données dans SSOX comme vous l’avez fait précédemment, puis développez le **Tables** nœud pour voir que toutes les tables ont été créés.</span><span class="sxs-lookup"><span data-stu-id="b82e3-351">Open the database in SSOX as you did earlier, and expand the **Tables** node to see that all of the tables have been created.</span></span> <span data-ttu-id="b82e3-352">(Si vous avez toujours SSOX ouvrir à partir de l’état antérieur, cliquez sur le **Actualiser** bouton.)</span><span class="sxs-lookup"><span data-stu-id="b82e3-352">(If you still have SSOX open from the earlier time, click the **Refresh** button.)</span></span>

![Tables de SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="b82e3-354">Exécutez l’application pour déclencher le code de l’initialiseur qui alimente la base de données.</span><span class="sxs-lookup"><span data-stu-id="b82e3-354">Run the app to trigger the initializer code that seeds the database.</span></span>

<span data-ttu-id="b82e3-355">Avec le bouton droit le **CourseAssignment** de table et sélectionnez **des données d’affichage** pour vérifier qu’il comporte des données.</span><span class="sxs-lookup"><span data-stu-id="b82e3-355">Right-click the **CourseAssignment** table and select **View Data** to verify that it has data in it.</span></span>

![Données CourseAssignment dans SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a><span data-ttu-id="b82e3-357">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="b82e3-357">Summary</span></span>

<span data-ttu-id="b82e3-358">Vous avez maintenant un modèle de données plus complexe et de la base de données correspondante.</span><span class="sxs-lookup"><span data-stu-id="b82e3-358">You now have a more complex data model and corresponding database.</span></span> <span data-ttu-id="b82e3-359">Dans ce didacticiel, vous en apprendrez davantage sur la façon d’accéder aux données associées.</span><span class="sxs-lookup"><span data-stu-id="b82e3-359">In the following tutorial, you'll learn more about how to access related data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b82e3-360">[Précédent](migrations.md)
[Suivant](read-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="b82e3-360">[Previous](migrations.md)
[Next](read-related-data.md)</span></span>  
