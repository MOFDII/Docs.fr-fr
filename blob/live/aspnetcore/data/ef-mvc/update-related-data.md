---
title: "Données - 7 sur 10 inhérentes à cœur d’ASP.NET MVC avec EF Core - mise à jour"
author: tdykstra
description: "Dans ce didacticiel vous allez mettre à jour les données associées en mettant à jour les propriétés de navigation et les champs de clé étrangère."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: 0e4df407a1ca15aa5baa2b7226be1cf91902a583
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="updating-related-data---ef-core-with-aspnet-core-mvc-tutorial-7-of-10"></a><span data-ttu-id="665da-103">Mise à jour des données connexes - Core EF avec le didacticiel d’ASP.NET MVC de base (7 sur 10)</span><span class="sxs-lookup"><span data-stu-id="665da-103">Updating related data - EF Core with ASP.NET Core MVC tutorial (7 of 10)</span></span>

<span data-ttu-id="665da-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="665da-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="665da-105">L’exemple d’application web Contoso University montre comment créer des applications web ASP.NET MVC de base à l’aide d’Entity Framework Core et Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="665da-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="665da-106">Pour plus d’informations sur la série de didacticiels, consultez [le premier didacticiel de la série](intro.md).</span><span class="sxs-lookup"><span data-stu-id="665da-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="665da-107">Dans le didacticiel précédent, vous avez affiché les données associées ; Dans ce didacticiel vous allez mettre à jour les données associées en mettant à jour les propriétés de navigation et les champs de clé étrangère.</span><span class="sxs-lookup"><span data-stu-id="665da-107">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="665da-108">Les illustrations suivantes montrent certaines des pages que vous allez utiliser.</span><span class="sxs-lookup"><span data-stu-id="665da-108">The following illustrations show some of the pages that you'll work with.</span></span>

![Page de modification du cours](update-related-data/_static/course-edit.png)

![Page de modification du formateur](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="665da-111">Personnaliser le créer et modifier des Pages pour les cours</span><span class="sxs-lookup"><span data-stu-id="665da-111">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="665da-112">Lorsqu’une nouvelle entité de cours est créée, il doit avoir une relation à un service existant.</span><span class="sxs-lookup"><span data-stu-id="665da-112">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="665da-113">Pour faciliter ce processus, le code de modèle généré automatiquement inclut des méthodes de contrôleur et de créer et modifier des vues qui incluent une liste déroulante pour sélectionner le service.</span><span class="sxs-lookup"><span data-stu-id="665da-113">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="665da-114">Les jeux de liste déroulante, le `Course.DepartmentID` une propriété de clé étrangère, et c’est tout Entity Framework a besoin pour charger le `Department` propriété de navigation avec l’entité de service appropriée.</span><span class="sxs-lookup"><span data-stu-id="665da-114">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="665da-115">Vous allez utiliser le code de modèle généré automatiquement, mais modifier légèrement pour ajouter la gestion des erreurs et de trier la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="665da-115">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="665da-116">Dans *CoursesController.cs*, supprimez les quatre méthodes de créer et modifier et remplacez-les par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="665da-116">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

<span data-ttu-id="665da-117">Après le `Edit` HttpPost (méthode), créez une méthode qui charge les informations de service pour obtenir la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="665da-117">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

<span data-ttu-id="665da-118">Le `PopulateDepartmentsDropDownList` Obtient une liste de tous les départements sont triés par nom de méthode, crée un `SelectList` collection pour obtenir la liste déroulante et passe la collection à la vue dans `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="665da-118">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="665da-119">La méthode accepte le paramètre facultatif `selectedDepartment` paramètre qui permet au code appelant spécifier l’élément est sélectionné lors du rendu de la liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="665da-119">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="665da-120">La vue passe le nom « DepartmentID » pour le `<select>` d’assistance de balise et l’application d’assistance puis sache qu’il peut pour rechercher dans le `ViewBag` de l’objet pour un `SelectList` nommé « DepartmentID ».</span><span class="sxs-lookup"><span data-stu-id="665da-120">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="665da-121">Le HttpGet `Create` les appels de méthode le `PopulateDepartmentsDropDownList` méthode sans définir l’élément sélectionné, car de formation le service n’est pas établi encore :</span><span class="sxs-lookup"><span data-stu-id="665da-121">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department is not established yet:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

<span data-ttu-id="665da-122">Le HttpGet `Edit` méthode définit l’élément sélectionné, en fonction de l’ID du service qui est déjà affecté au cours en cours de modification :</span><span class="sxs-lookup"><span data-stu-id="665da-122">The HttpGet `Edit` method sets the selected item, based on the ID of the department that is already assigned to the course being edited:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

<span data-ttu-id="665da-123">Pour les deux méthodes HttpPost `Create` et `Edit` également inclure du code qui définit l’élément sélectionné quand ils réafficher la page après une erreur.</span><span class="sxs-lookup"><span data-stu-id="665da-123">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="665da-124">Cela garantit que lorsque la page s’affiche de nouveau pour afficher le message d’erreur, quel que soit le service a été sélectionné reste sélectionné.</span><span class="sxs-lookup"><span data-stu-id="665da-124">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="665da-125">Ajouter. AsNoTracking pour plus d’informations et de méthodes de suppression</span><span class="sxs-lookup"><span data-stu-id="665da-125">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="665da-126">Pour optimiser les performances des détails du cours et supprimer des pages, ajouter `AsNoTracking` appelle le `Details` et HttpGet `Delete` méthodes.</span><span class="sxs-lookup"><span data-stu-id="665da-126">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a><span data-ttu-id="665da-127">Modifier les vues de cours</span><span class="sxs-lookup"><span data-stu-id="665da-127">Modify the Course views</span></span>

<span data-ttu-id="665da-128">Dans *Views/Courses/Create.cshtml*, ajouter une option « Sélectionner département » pour le **service** déroulante liste, de modifier la légende de **DepartmentID** à  **Service**et ajouter un message de validation.</span><span class="sxs-lookup"><span data-stu-id="665da-128">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

<span data-ttu-id="665da-129">Dans *Views/Courses/Edit.cshtml*, apporter la même modification pour le champ de service que vous venez de faire dans *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="665da-129">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="665da-130">Également dans *Views/Courses/Edit.cshtml*, ajoutez un champ de numéro de cours avant la **titre** champ.</span><span class="sxs-lookup"><span data-stu-id="665da-130">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="665da-131">Étant donné que le nombre de cours est la clé primaire, il est affiché, mais il ne peut pas être modifié.</span><span class="sxs-lookup"><span data-stu-id="665da-131">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

<span data-ttu-id="665da-132">Il existe déjà un champ masqué (`<input type="hidden">`) pour le nombre de cours dans la vue Edition.</span><span class="sxs-lookup"><span data-stu-id="665da-132">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="665da-133">Ajout d’un `<label>` application d’assistance de balise n’élimine la nécessité pour le champ masqué, car elle n’entraîne pas le nombre de cours à inclure dans les données publiées lorsque l’utilisateur clique sur **enregistrer** sur la **modifier** page.</span><span class="sxs-lookup"><span data-stu-id="665da-133">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="665da-134">Dans *Views/Courses/Delete.cshtml*, ajoutez un champ du numéro de cours en haut et modifier l’ID de service pour le nom du service.</span><span class="sxs-lookup"><span data-stu-id="665da-134">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

<span data-ttu-id="665da-135">Dans *Views/Courses/Details.cshtml*, apporter la même modification que vous venez de *Delete.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="665da-135">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="665da-136">Tester les pages de cours</span><span class="sxs-lookup"><span data-stu-id="665da-136">Test the Course pages</span></span>

<span data-ttu-id="665da-137">Exécuter l’application, sélectionnez le **cours** , cliquez sur **créer un nouveau**et entrez les données d’un nouveau cours :</span><span class="sxs-lookup"><span data-stu-id="665da-137">Run the app, select the **Courses** tab, click **Create New**, and enter data for a new course:</span></span>

![Page de création de cours](update-related-data/_static/course-create.png)

<span data-ttu-id="665da-139">Cliquez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="665da-139">Click **Create**.</span></span> <span data-ttu-id="665da-140">La page d’Index de cours s’affiche avec le cours de nouveau ajouté à la liste.</span><span class="sxs-lookup"><span data-stu-id="665da-140">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="665da-141">Le nom du service dans la liste de page d’Index provient de la propriété de navigation, indiquant que la relation a été établie correctement.</span><span class="sxs-lookup"><span data-stu-id="665da-141">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="665da-142">Cliquez sur **modifier** sur un cours dans la page d’Index de cours.</span><span class="sxs-lookup"><span data-stu-id="665da-142">Click **Edit** on a course in the Courses Index page.</span></span>

![Page de modification du cours](update-related-data/_static/course-edit.png)

<span data-ttu-id="665da-144">Modifier les données sur la page, cliquez sur **enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="665da-144">Change data on the page and click **Save**.</span></span> <span data-ttu-id="665da-145">La page d’Index de cours s’affiche avec les données de cours mis à jour.</span><span class="sxs-lookup"><span data-stu-id="665da-145">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-an-edit-page-for-instructors"></a><span data-ttu-id="665da-146">Ajouter une Page de modification pour les formateurs</span><span class="sxs-lookup"><span data-stu-id="665da-146">Add an Edit Page for Instructors</span></span>

<span data-ttu-id="665da-147">Lorsque vous modifiez un enregistrement du formateur, que vous souhaitez être en mesure de mettre à jour l’attribution du formateur office.</span><span class="sxs-lookup"><span data-stu-id="665da-147">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="665da-148">Entité Instructor a une relation un-à-zéro-ou-un avec l’entité OfficeAssignment, ce qui signifie que votre code doit gérer les situations suivantes :</span><span class="sxs-lookup"><span data-stu-id="665da-148">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="665da-149">Si l’utilisateur efface l’attribution d’office et il avait une valeur, supprimez l’entité OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="665da-149">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="665da-150">Si l’utilisateur entre une valeur de l’attribution d’office et il a été initialement vide, créez une nouvelle entité OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="665da-150">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="665da-151">Si l’utilisateur modifie la valeur d’une assignation d’office, modifiez la valeur dans une entité OfficeAssignment existante.</span><span class="sxs-lookup"><span data-stu-id="665da-151">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="665da-152">Mettre à jour le contrôleur instructeurs</span><span class="sxs-lookup"><span data-stu-id="665da-152">Update the Instructors controller</span></span>

<span data-ttu-id="665da-153">Dans *InstructorsController.cs*, modifiez le code dans le HttpGet `Edit` méthode afin qu’elle charge de l’entité Instructor `OfficeAssignment` propriété de navigation et les appels `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="665da-153">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

<span data-ttu-id="665da-154">Remplacez le HttpPost `Edit` méthode avec le code suivant pour gérer les mises à jour de l’attribution d’office :</span><span class="sxs-lookup"><span data-stu-id="665da-154">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

<span data-ttu-id="665da-155">Le code effectue les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="665da-155">The code does the following:</span></span>

-  <span data-ttu-id="665da-156">Modifie le nom de la méthode `EditPost` , car la signature est désormais le même que le HttpGet `Edit` (méthode) (le `ActionName` attribut spécifie que le `/Edit/` URL est toujours utilisé).</span><span class="sxs-lookup"><span data-stu-id="665da-156">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

-  <span data-ttu-id="665da-157">Obtient l’entité Instructor actuelle à partir de la base de données à l’aide pour le chargement hâtif le `OfficeAssignment` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="665da-157">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="665da-158">Il est identique à ce que vous l’avez fait dans le HttpGet `Edit` (méthode).</span><span class="sxs-lookup"><span data-stu-id="665da-158">This is the same as what you did in the HttpGet `Edit` method.</span></span>

-  <span data-ttu-id="665da-159">Met à jour l’entité Instructor récupérée avec des valeurs dans le classeur de modèles.</span><span class="sxs-lookup"><span data-stu-id="665da-159">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="665da-160">Le `TryUpdateModel` surcharge vous permet à la liste blanche les propriétés que vous souhaitez inclure.</span><span class="sxs-lookup"><span data-stu-id="665da-160">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="665da-161">Cela empêche la validation excessive, comme expliqué dans la [deuxième didacticiel](crud.md).</span><span class="sxs-lookup"><span data-stu-id="665da-161">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets do not play well with <ul> [!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   <span data-ttu-id="665da-162">Si l’emplacement du bureau est vide, définit la propriété Instructor.OfficeAssignment NULL afin que la ligne correspondante dans la table OfficeAssignment est supprimée.</span><span class="sxs-lookup"><span data-stu-id="665da-162">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets do not play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- <span data-ttu-id="665da-163">Enregistre les modifications dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="665da-163">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="665da-164">Mettre à jour la vue Modifier le formateur</span><span class="sxs-lookup"><span data-stu-id="665da-164">Update the Instructor Edit view</span></span>

<span data-ttu-id="665da-165">Dans *Views/Instructors/Edit.cshtml*, ajouter un nouveau champ pour la modification de l’emplacement du bureau, à la fin avant du **enregistrer** bouton :</span><span class="sxs-lookup"><span data-stu-id="665da-165">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

<span data-ttu-id="665da-166">Exécuter l’application, sélectionnez le **instructeurs** onglet, puis cliquez sur **modifier** sur un formateur.</span><span class="sxs-lookup"><span data-stu-id="665da-166">Run the app, select the **Instructors** tab, and then click **Edit** on an instructor.</span></span> <span data-ttu-id="665da-167">Modifier la **bureaux** et cliquez sur **enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="665da-167">Change the **Office Location** and click **Save**.</span></span>

![Page de modification du formateur](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="665da-169">Ajouter des affectations de cours à la page Modifier le formateur</span><span class="sxs-lookup"><span data-stu-id="665da-169">Add Course assignments to the Instructor Edit page</span></span>

<span data-ttu-id="665da-170">Instructeurs enseigner à n’importe quel nombre de cours.</span><span class="sxs-lookup"><span data-stu-id="665da-170">Instructors may teach any number of courses.</span></span> <span data-ttu-id="665da-171">Maintenant, vous allez améliorer la page Modifier le formateur en ajoutant la possibilité de modifier les affectations de cours à l’aide d’un groupe de cases à cocher, comme indiqué dans la capture d’écran suivante :</span><span class="sxs-lookup"><span data-stu-id="665da-171">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Page de modification du formateur en cours](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="665da-173">La relation entre les entités en cours et le formateur est plusieurs-à-plusieurs.</span><span class="sxs-lookup"><span data-stu-id="665da-173">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="665da-174">Pour ajouter et supprimer des relations, vous ajoutez et supprimez des entités vers et depuis le jeu d’entités CourseAssignments jointure.</span><span class="sxs-lookup"><span data-stu-id="665da-174">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="665da-175">Formateur de l’interface utilisateur qui permet de modifier les cours est assigné à est un groupe de cases à cocher.</span><span class="sxs-lookup"><span data-stu-id="665da-175">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="665da-176">Une case à cocher pour chaque cours dans la base de données s’affiche, et celles qui le formateur est actuellement attribué aux sont sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="665da-176">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="665da-177">L’utilisateur peut sélectionner ou désactivez les cases à cocher pour modifier les affectations de cours.</span><span class="sxs-lookup"><span data-stu-id="665da-177">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="665da-178">Si le nombre de cours ont été beaucoup plus important, vous souhaiterez probablement utiliser une autre méthode de présentation des données dans la vue, mais que vous utiliseriez la même méthode de manipulation d’une entité de jointure pour créer ou supprimer des relations.</span><span class="sxs-lookup"><span data-stu-id="665da-178">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="665da-179">Mettre à jour le contrôleur instructeurs</span><span class="sxs-lookup"><span data-stu-id="665da-179">Update the Instructors controller</span></span>

<span data-ttu-id="665da-180">Pour fournir des données à la vue pour obtenir la liste de cases à cocher, vous allez utiliser une classe de modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="665da-180">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="665da-181">Créer *AssignedCourseData.cs* dans les *SchoolViewModels* dossier et remplacez le code existant par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="665da-181">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="665da-182">Dans *InstructorsController.cs*, remplacez le HttpGet `Edit` méthode avec le code suivant.</span><span class="sxs-lookup"><span data-stu-id="665da-182">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="665da-183">Les modifications sont mises en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="665da-183">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

<span data-ttu-id="665da-184">Le code ajoute un chargement hâtif pour le `Courses` propriété de navigation et appelle la nouvelle `PopulateAssignedCourseData` méthode pour fournir des informations du tableau de case à cocher à l’aide de la `AssignedCourseData` afficher la classe de modèle.</span><span class="sxs-lookup"><span data-stu-id="665da-184">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="665da-185">Le code dans la `PopulateAssignedCourseData` méthode lit toutes les entités de cours pour charger une liste de cours à l’aide de la classe de modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="665da-185">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="665da-186">Pour chaque cours, le code vérifie l’existence de cours dans le formateur `Courses` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="665da-186">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="665da-187">Pour créer l’efficacité des recherches lors de la vérification si un cours est affecté pour le formateur, les cours affectés pour le formateur sont placés dans un `HashSet` collection.</span><span class="sxs-lookup"><span data-stu-id="665da-187">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="665da-188">Le `Assigned` est définie sur true pour les cours le formateur est assigné à.</span><span class="sxs-lookup"><span data-stu-id="665da-188">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="665da-189">La vue utilise cette propriété pour déterminer les zones doivent être affichées en tant que vérification sélectionné.</span><span class="sxs-lookup"><span data-stu-id="665da-189">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="665da-190">Enfin, la liste est passée à la vue dans `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="665da-190">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="665da-191">Ensuite, ajoutez le code qui est exécuté lorsque l’utilisateur clique sur **enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="665da-191">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="665da-192">Remplacez le `EditPost` méthode avec les éléments suivants de code et ajoutez une nouvelle méthode qui met à jour le `Courses` propriété de navigation d’entité Instructor.</span><span class="sxs-lookup"><span data-stu-id="665da-192">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

<span data-ttu-id="665da-193">La signature de méthode diffère maintenant le HttpGet `Edit` méthode, de sorte que si le nom de la méthode change de `EditPost` à `Edit`.</span><span class="sxs-lookup"><span data-stu-id="665da-193">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="665da-194">Étant donné que la vue ne dispose d’une collection d’entités de cours, le classeur de modèles ne peut pas mettre à jour automatiquement le `CourseAssignments` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="665da-194">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="665da-195">Au lieu d’utiliser le classeur de modèles pour mettre à jour le `CourseAssignments` propriété de navigation, c’est dans la nouvelle `UpdateInstructorCourses` (méthode).</span><span class="sxs-lookup"><span data-stu-id="665da-195">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="665da-196">Par conséquent, vous devez exclure le `CourseAssignments` propriété à partir de la liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="665da-196">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="665da-197">Cela ne nécessite aucune modification pour le code qui appelle `TryUpdateModel` , car vous utilisez la surcharge de la création de listes autorisées et `CourseAssignments` n’est pas dans la liste d’inclusion.</span><span class="sxs-lookup"><span data-stu-id="665da-197">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="665da-198">Si aucune vérification de zones ont été sélectionnés, le code dans `UpdateInstructorCourses` initialise le `CourseAssignments` propriété de navigation avec une collection vide et retourne :</span><span class="sxs-lookup"><span data-stu-id="665da-198">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

<span data-ttu-id="665da-199">Ensuite, le code effectue une itération sur tous les cours dans la base de données et vérifie chaque cours par rapport à celles actuellement affectées au formateur par rapport à ceux qui ont été sélectionnés dans la vue.</span><span class="sxs-lookup"><span data-stu-id="665da-199">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="665da-200">Pour faciliter les recherches efficaces, les deux collections ce dernier sont stockées dans `HashSet` objets.</span><span class="sxs-lookup"><span data-stu-id="665da-200">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="665da-201">Si la case à cocher pour un cours a été sélectionnée mais que le cours n’est pas dans le `Instructor.CourseAssignments` propriété de navigation, le cours est ajouté à la collection dans la propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="665da-201">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

<span data-ttu-id="665da-202">Si la case à cocher pour un cours n’a pas été activée, mais le cours se trouve dans le `Instructor.CourseAssignments` propriété de navigation, le cours est supprimée de la propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="665da-202">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a><span data-ttu-id="665da-203">Mettre à jour les vues de formateur</span><span class="sxs-lookup"><span data-stu-id="665da-203">Update the Instructor views</span></span>

<span data-ttu-id="665da-204">Dans *Views/Instructors/Edit.cshtml*, ajouter un **cours** champ avec un tableau de cases à cocher en ajoutant le code suivant immédiatement après le code le `div` éléments pour le **Office**  champ et avant la `div` , élément pour les **enregistrer** bouton.</span><span class="sxs-lookup"><span data-stu-id="665da-204">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE] 
> <span data-ttu-id="665da-205">Lorsque vous collez le code dans Visual Studio, les sauts de ligne changera d’une manière qui interrompt le code.</span><span class="sxs-lookup"><span data-stu-id="665da-205">When you paste the code in Visual Studio, line breaks will be changed in a way that breaks the code.</span></span>  <span data-ttu-id="665da-206">Appuyez une fois sur Ctrl + Z pour annuler la mise en forme automatique.</span><span class="sxs-lookup"><span data-stu-id="665da-206">Press Ctrl+Z one time to undo the automatic formatting.</span></span>  <span data-ttu-id="665da-207">Cela permet de résoudre les sauts de ligne afin qu’elles apparaîtront comme vous le voyez ici.</span><span class="sxs-lookup"><span data-stu-id="665da-207">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="665da-208">La mise en retrait ne doit pas nécessairement être parfait, mais la `@</tr><tr>`, `@:<td>`, `@:</td>`, et `@:</tr>` lignes doivent être chacun sur une ligne unique comme illustré, ou vous obtiendrez une erreur d’exécution.</span><span class="sxs-lookup"><span data-stu-id="665da-208">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="665da-209">Avec le bloc de code nouveau sélectionné, appuyez sur Tab trois fois pour aligner le nouveau code avec le code existant.</span><span class="sxs-lookup"><span data-stu-id="665da-209">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="665da-210">Vous pouvez vérifier l’état de ce problème [ici](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="665da-210">You can check the status of this problem [here](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

<span data-ttu-id="665da-211">Ce code crée une table HTML qui possède trois colonnes.</span><span class="sxs-lookup"><span data-stu-id="665da-211">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="665da-212">Dans chaque colonne est une case à cocher suivie d’une légende qui est constitué par le numéro et le titre.</span><span class="sxs-lookup"><span data-stu-id="665da-212">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="665da-213">Toutes les cases à cocher ont le même nom (« selectedCourses »), qui informe le classeur de modèles, ils doivent être traités en tant que groupe.</span><span class="sxs-lookup"><span data-stu-id="665da-213">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they are to be treated as a group.</span></span> <span data-ttu-id="665da-214">L’attribut de valeur de chaque case à cocher est défini à la valeur de `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="665da-214">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="665da-215">Lorsque la page est publiée, le classeur de modèles passe un tableau au contrôleur qui se compose de la `CourseID` valeurs pour seulement les cases à cocher qui sont sélectionnées.</span><span class="sxs-lookup"><span data-stu-id="665da-215">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="665da-216">Lorsque les cases à cocher sont restitués initialement, ceux qui sont pour les cours affectés pour le formateur archivées attributs, qui sélectionne les (affiche les archivés).</span><span class="sxs-lookup"><span data-stu-id="665da-216">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="665da-217">Exécuter l’application, sélectionnez le **instructeurs** onglet, puis cliquez sur **modifier** sur un formateur pour voir les **modifier** page.</span><span class="sxs-lookup"><span data-stu-id="665da-217">Run the app, select the **Instructors** tab, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![Page de modification du formateur en cours](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="665da-219">Modifier des attributions de cours et cliquez sur Enregistrer.</span><span class="sxs-lookup"><span data-stu-id="665da-219">Change some course assignments and click Save.</span></span> <span data-ttu-id="665da-220">Les modifications que vous apportez sont répercutées sur la page d’Index.</span><span class="sxs-lookup"><span data-stu-id="665da-220">The changes you make are reflected on the Index page.</span></span>

> [!NOTE] 
> <span data-ttu-id="665da-221">L’approche adoptée ici pour modifier les données de cours formateur fonctionne bien quand un nombre limité de cours.</span><span class="sxs-lookup"><span data-stu-id="665da-221">The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="665da-222">Pour les collections qui sont beaucoup plus volumineux, une interface utilisateur différente et une autre méthode de mise à jour serait nécessaires.</span><span class="sxs-lookup"><span data-stu-id="665da-222">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="665da-223">Mise à jour de la page de suppression</span><span class="sxs-lookup"><span data-stu-id="665da-223">Update the Delete page</span></span>

<span data-ttu-id="665da-224">Dans *InstructorsController.cs*, supprimez le `DeleteConfirmed` méthode et insérer le code suivant à la place.</span><span class="sxs-lookup"><span data-stu-id="665da-224">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

<span data-ttu-id="665da-225">Ce code rend les modifications suivantes :</span><span class="sxs-lookup"><span data-stu-id="665da-225">This code makes the following changes:</span></span>

* <span data-ttu-id="665da-226">Eager fait de chargement pour le `CourseAssignments` propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="665da-226">Does eager loading for the `CourseAssignments` navigation property.</span></span>  <span data-ttu-id="665da-227">Vous devez inclure ou EF ne sont pas informés sur connexes `CourseAssignment` entités et ne sont pas les supprimer.</span><span class="sxs-lookup"><span data-stu-id="665da-227">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span>  <span data-ttu-id="665da-228">Pour éviter d’avoir à les lire ici vous pouvez configurer suppression en cascade dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="665da-228">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="665da-229">Si le formateur à supprimer est attribué en tant qu’administrateur d’un service, supprime l’attribution de formateur de ces services.</span><span class="sxs-lookup"><span data-stu-id="665da-229">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="665da-230">Ajouter des bureaux et les cours à la page Créer</span><span class="sxs-lookup"><span data-stu-id="665da-230">Add office location and courses to the Create page</span></span>

<span data-ttu-id="665da-231">Dans *InstructorsController.cs*, supprimez le HttpGet et HttpPost `Create` méthodes, puis ajoutez le code suivant à leur place :</span><span class="sxs-lookup"><span data-stu-id="665da-231">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

<span data-ttu-id="665da-232">Ce code est similaire à ce que vous avez vu pour la `Edit` méthodes à l’exception qui initialement aucun cours sont sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="665da-232">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="665da-233">Le HttpGet `Create` les appels de méthode le `PopulateAssignedCourseData` méthode pas, car il peut y avoir des cours sélectionnés mais, dans commande pour fournir une collection vide pour le `foreach` boucle dans la vue (sinon l’afficher le code lève une exception de référence null).</span><span class="sxs-lookup"><span data-stu-id="665da-233">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="665da-234">HttpPost `Create` méthode ajoute chaque cours sélectionné pour le `CourseAssignments` propriété de navigation avant qu’il vérifie les erreurs de validation et ajoute le nouveau formateur pour la base de données.</span><span class="sxs-lookup"><span data-stu-id="665da-234">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="665da-235">Cours sont ajoutés même s’il existe des erreurs de modèle afin que lorsque des erreurs de modèle (par exemple, l’utilisateur indexé une date non valide), et la page s’affiche de nouveau avec un message d’erreur, toutes les sélections de cours qui ont été apportées sont automatiquement restaurées.</span><span class="sxs-lookup"><span data-stu-id="665da-235">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="665da-236">Notez que pour pouvoir ajouter des cours à la `CourseAssignments` propriété de navigation que vous avez pour initialiser la propriété comme une collection vide :</span><span class="sxs-lookup"><span data-stu-id="665da-236">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="665da-237">Comme alternative à cette opération dans le code du contrôleur, vous pouvez l’effectuer dans le modèle de formateur en modifiant l’accesseur Get de propriété pour créer automatiquement la collection si elle n’existe pas, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="665da-237">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

```csharp
private ICollection<CourseAssignment> _courseAssignments;
public ICollection<CourseAssignment> CourseAssignments
{
    get
    {
        return _courseAssignments ?? (_courseAssignments = new List<CourseAssignment>());
    }
    set
    {
        _courseAssignments = value;
    }
}
```

<span data-ttu-id="665da-238">Si vous modifiez le `CourseAssignments` propriété de cette façon, vous pouvez supprimer le code d’initialisation explicite de propriété dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="665da-238">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="665da-239">Dans *Views/Instructor/Create.cshtml*, ajoutez une zone de texte emplacement office et cochez les cases pour les cours avant le bouton Envoyer.</span><span class="sxs-lookup"><span data-stu-id="665da-239">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="665da-240">Comme dans le cas de la page de modification, [corriger la mise en forme si Visual Studio remet en forme le code lorsque vous le collez](#notepad).</span><span class="sxs-lookup"><span data-stu-id="665da-240">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

<span data-ttu-id="665da-241">Testez en exécutant l’application et la création d’un formateur.</span><span class="sxs-lookup"><span data-stu-id="665da-241">Test by running the app and creating an instructor.</span></span> 

## <a name="handling-transactions"></a><span data-ttu-id="665da-242">La gestion des Transactions</span><span class="sxs-lookup"><span data-stu-id="665da-242">Handling Transactions</span></span>

<span data-ttu-id="665da-243">Comme expliqué dans la [didacticiel CRUD](crud.md), Entity Framework implémente implicitement des transactions.</span><span class="sxs-lookup"><span data-stu-id="665da-243">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="665da-244">Pour les scénarios où vous avez besoin de plus contrôler--par exemple, si vous souhaitez inclure des opérations effectuées en dehors d’Entity Framework dans une transaction, consultez [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span><span class="sxs-lookup"><span data-stu-id="665da-244">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="summary"></a><span data-ttu-id="665da-245">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="665da-245">Summary</span></span>

<span data-ttu-id="665da-246">Vous avez maintenant terminé l’introduction à l’utilisation des données associées.</span><span class="sxs-lookup"><span data-stu-id="665da-246">You have now completed the introduction to working with related data.</span></span> <span data-ttu-id="665da-247">Dans l’étape suivante du didacticiel, vous verrez comment gérer les conflits d’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="665da-247">In the next tutorial you'll see how to handle concurrency conflicts.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="665da-248">[Précédent](read-related-data.md)
[Suivant](concurrency.md)</span><span class="sxs-lookup"><span data-stu-id="665da-248">[Previous](read-related-data.md)
[Next](concurrency.md)</span></span>  
