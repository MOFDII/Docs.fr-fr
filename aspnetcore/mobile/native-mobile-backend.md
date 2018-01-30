---
title: "Création de Services principaux pour les Applications mobiles natives"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mobile/native-mobile-backend
ms.openlocfilehash: ff09f331cff5cca7b42fa89bff55c0ed5c7d82f4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="creating-backend-services-for-native-mobile-applications"></a><span data-ttu-id="a6170-102">Création de Services principaux pour les Applications mobiles natives</span><span class="sxs-lookup"><span data-stu-id="a6170-102">Creating Backend Services for Native Mobile Applications</span></span>

<span data-ttu-id="a6170-103">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a6170-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a6170-104">Les applications mobiles peuvent communiquer facilement avec les services principaux de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a6170-104">Mobile apps can easily communicate with ASP.NET Core backend services.</span></span>

[<span data-ttu-id="a6170-105">Afficher ou télécharger l’exemple de code de services principaux</span><span class="sxs-lookup"><span data-stu-id="a6170-105">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="a6170-106">L’exemple d’application Mobile natif</span><span class="sxs-lookup"><span data-stu-id="a6170-106">The Sample Native Mobile App</span></span>

<span data-ttu-id="a6170-107">Ce didacticiel montre comment créer des services principaux à l’aide d’ASP.NET MVC de base pour prendre en charge des applications mobiles natives.</span><span class="sxs-lookup"><span data-stu-id="a6170-107">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="a6170-108">Elle utilise le [Xamarin Forms ToDoRest application](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) en tant que son client natif, qui inclut des clients natifs distincts pour les appareils Android, iOS, Windows universel et Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="a6170-108">It uses the [Xamarin Forms ToDoRest app](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="a6170-109">Vous pouvez suivre le didacticiel lié pour créer l’application native (et installer les outils Xamarin libres nécessaires), ainsi que télécharger l’exemple de solution Xamarin.</span><span class="sxs-lookup"><span data-stu-id="a6170-109">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="a6170-110">L’exemple de Xamarin inclut un projet de services ASP.NET Web API 2, qui remplace les applications ASP.NET Core de cet article (sans aucune modification n’est requise par le client).</span><span class="sxs-lookup"><span data-stu-id="a6170-110">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![Pour l’application de faire le reste en cours d’exécution sur un smartphone Android](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="a6170-112">Fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="a6170-112">Features</span></span>

<span data-ttu-id="a6170-113">L’application ToDoRest prend en charge l’affichage, ajout, la suppression et la mise à jour des éléments de tâche.</span><span class="sxs-lookup"><span data-stu-id="a6170-113">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="a6170-114">Chaque élément possède un ID, un nom, Notes et une propriété qui indique si elle est été encore terminé.</span><span class="sxs-lookup"><span data-stu-id="a6170-114">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="a6170-115">La vue principale des éléments, comme indiqué ci-dessus, répertorie le nom de chaque élément et indique si elle est effectuée avec une coche.</span><span class="sxs-lookup"><span data-stu-id="a6170-115">The main view of the items, as shown above, lists each item's name and indicates if it's done with a checkmark.</span></span>

<span data-ttu-id="a6170-116">Si vous appuyez sur la `+` icône ouvre une boîte de dialogue Ajouter élément :</span><span class="sxs-lookup"><span data-stu-id="a6170-116">Tapping the `+` icon opens an add item dialog:</span></span>

![Élément de boîte de dialogue Ajouter](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="a6170-118">Un élément sur l’écran de liste principale ouvre une boîte de dialogue Modifier où nom de l’élément, les notes de publication et les paramètres terminés peuvent être modifiés ou que l’élément peut être supprimé :</span><span class="sxs-lookup"><span data-stu-id="a6170-118">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![Élément de boîte de dialogue Modifier](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="a6170-120">Cet exemple est configuré par défaut pour utiliser les services principaux hébergés sur developer.xamarin.com, qui autorisent des opérations en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="a6170-120">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="a6170-121">Pour tester vous-même par rapport à l’application ASP.NET Core créée dans la section suivante, en cours d’exécution sur votre ordinateur, vous devez mettre à jour de l’application `RestUrl` constante.</span><span class="sxs-lookup"><span data-stu-id="a6170-121">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="a6170-122">Accédez à la `ToDoREST` de projet et ouvrez le *Constants.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="a6170-122">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="a6170-123">Remplacez le `RestUrl` avec une URL qui inclut IP votre ordinateur adresse (pas localhost ou 127.0.0.1, étant donné que cette adresse est utilisée à partir de l’émulateur d’appareil, pas à partir de votre ordinateur).</span><span class="sxs-lookup"><span data-stu-id="a6170-123">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="a6170-124">Inclut également le numéro de port (5000).</span><span class="sxs-lookup"><span data-stu-id="a6170-124">Include the port number as well (5000).</span></span> <span data-ttu-id="a6170-125">Afin de vérifier que vos services fonctionnent avec un périphérique, assurez-vous de que vous n’avez pas un pare-feu actif bloque l’accès à ce port.</span><span class="sxs-lookup"><span data-stu-id="a6170-125">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="a6170-126">Création du projet ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a6170-126">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="a6170-127">Créer une Application Web ASP.NET Core dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a6170-127">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="a6170-128">Choisissez le modèle de l’API Web et aucune authentification.</span><span class="sxs-lookup"><span data-stu-id="a6170-128">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="a6170-129">Nommez le projet *ToDoApi*.</span><span class="sxs-lookup"><span data-stu-id="a6170-129">Name the project *ToDoApi*.</span></span>

![Boîte de dialogue nouvelle Application Web ASP.NET avec le modèle de projet d’API Web sélectionné](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="a6170-131">L’application doit répondre à toutes les demandes adressées au port 5000.</span><span class="sxs-lookup"><span data-stu-id="a6170-131">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="a6170-132">Mise à jour *Program.cs* à inclure `.UseUrls("http://*:5000")` d’effectuer cette opération :</span><span class="sxs-lookup"><span data-stu-id="a6170-132">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> <span data-ttu-id="a6170-133">Assurez-vous que vous exécutez l’application directement, plutôt que derrière IIS Express, qui ignore les demandes non locaux par défaut.</span><span class="sxs-lookup"><span data-stu-id="a6170-133">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="a6170-134">Exécutez `dotnet run` à partir d’une invite de commandes, ou choisissez le profil de nom d’application dans la liste déroulante de cible de débogage dans la barre d’outils de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a6170-134">Run `dotnet run` from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="a6170-135">Ajouter une classe de modèle pour représenter des éléments de tâche.</span><span class="sxs-lookup"><span data-stu-id="a6170-135">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="a6170-136">Marque requis des champs à l’aide de la `[Required]` attribut :</span><span class="sxs-lookup"><span data-stu-id="a6170-136">Mark required fields using the `[Required]` attribute:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

<span data-ttu-id="a6170-137">Les méthodes d’API requièrent un moyen d’utiliser des données.</span><span class="sxs-lookup"><span data-stu-id="a6170-137">The API methods require some way to work with data.</span></span> <span data-ttu-id="a6170-138">Utiliser le même `IToDoRepository` l’exemple de Xamarin d’origine utilise l’interface :</span><span class="sxs-lookup"><span data-stu-id="a6170-138">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

<span data-ttu-id="a6170-139">Pour cet exemple, l’implémentation utilise seulement une collection privée d’éléments :</span><span class="sxs-lookup"><span data-stu-id="a6170-139">For this sample, the implementation just uses a private collection of items:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

<span data-ttu-id="a6170-140">Configurer la mise en oeuvre dans *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a6170-140">Configure the implementation in *Startup.cs*:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

<span data-ttu-id="a6170-141">À ce stade, vous êtes prêt à créer le *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="a6170-141">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="a6170-142">En savoir plus sur la création web API dans [construction votre API Web premier avec ASP.NET MVC de base et de Visual Studio](../tutorials/first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="a6170-142">Learn more about creating web APIs in [Building Your First Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="a6170-143">Création du contrôleur</span><span class="sxs-lookup"><span data-stu-id="a6170-143">Creating the Controller</span></span>

<span data-ttu-id="a6170-144">Ajoutez un nouveau contrôleur pour le projet, *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="a6170-144">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="a6170-145">Il doit hériter de Microsoft.AspNetCore.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="a6170-145">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="a6170-146">Ajouter un `Route` attribut pour indiquer que le contrôleur gère les demandes effectuées aux chemins d’accès commençant par `api/todoitems`.</span><span class="sxs-lookup"><span data-stu-id="a6170-146">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="a6170-147">Le `[controller]` jeton dans l’itinéraire est remplacé par le nom du contrôleur (l’omission de la `Controller` suffixe) et s’avère particulièrement utile pour les itinéraires globales.</span><span class="sxs-lookup"><span data-stu-id="a6170-147">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="a6170-148">En savoir plus sur [routage](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="a6170-148">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="a6170-149">Le contrôleur a besoin d’un `IToDoRepository` à fonction ; demander une instance de ce type via le constructeur du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="a6170-149">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="a6170-150">Lors de l’exécution, cette instance est fournie à l’aide de la prise en charge de l’infrastructure de [injection de dépendance](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="a6170-150">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

<span data-ttu-id="a6170-151">Cette API prend en charge quatre verbes HTTP différents pour effectuer des opérations CRUD (création, lecture, mise à jour, suppression) sur la source de données.</span><span class="sxs-lookup"><span data-stu-id="a6170-151">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="a6170-152">La plus simple d'entre eux est l’opération de lecture, qui correspond à une requête HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="a6170-152">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="a6170-153">Lecture des éléments</span><span class="sxs-lookup"><span data-stu-id="a6170-153">Reading Items</span></span>

<span data-ttu-id="a6170-154">Demande d’une liste d’éléments est effectuée avec une demande GET pour le `List` (méthode).</span><span class="sxs-lookup"><span data-stu-id="a6170-154">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="a6170-155">Le `[HttpGet]` de l’attribut le `List` méthode indique que cette action doit gérer uniquement les demandes GET.</span><span class="sxs-lookup"><span data-stu-id="a6170-155">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="a6170-156">L’itinéraire pour cette action est l’itinéraire spécifié sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="a6170-156">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="a6170-157">Vous n’avez pas nécessairement besoin d’utiliser le nom d’action dans le cadre de l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="a6170-157">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="a6170-158">Il vous suffit de vous assurer de que chaque action comporte un itinéraire unique et non équivoque.</span><span class="sxs-lookup"><span data-stu-id="a6170-158">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="a6170-159">Attributs de routage peuvent être appliqués au contrôleur et à des niveaux de méthode pour créer des itinéraires spécifiques.</span><span class="sxs-lookup"><span data-stu-id="a6170-159">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

<span data-ttu-id="a6170-160">Le `List` méthode retourne un code de réponse OK 200 et tous les éléments de tâche, sérialisées au format JSON.</span><span class="sxs-lookup"><span data-stu-id="a6170-160">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="a6170-161">Vous pouvez tester votre nouvelle méthode d’API à l’aide de divers outils, tels que [Postman](https://www.getpostman.com/docs/), illustrée ici :</span><span class="sxs-lookup"><span data-stu-id="a6170-161">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![Affichage d’une demande GET todoitems et le corps de la réponse de JSON pour les trois éléments renvoyés dans la console postman](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="a6170-163">Création d’éléments</span><span class="sxs-lookup"><span data-stu-id="a6170-163">Creating Items</span></span>

<span data-ttu-id="a6170-164">Par convention, la création de nouveaux éléments de données est mappé pour le verbe HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="a6170-164">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="a6170-165">Le `Create` méthode a une `[HttpPost]` attribut appliqué et accepte un `ToDoItem` instance.</span><span class="sxs-lookup"><span data-stu-id="a6170-165">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="a6170-166">Étant donné que la `item` argument sera passé dans le corps de la publication, ce paramètre est décoré avec le `[FromBody]` attribut.</span><span class="sxs-lookup"><span data-stu-id="a6170-166">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="a6170-167">À l’intérieur de la méthode, l’élément est activé pour la validité et l’existence préalable dans le magasin de données, et si aucun problème se produit, il est ajouté à l’aide de l’espace de stockage.</span><span class="sxs-lookup"><span data-stu-id="a6170-167">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it's added using the repository.</span></span> <span data-ttu-id="a6170-168">La vérification de la `ModelState.IsValid` effectue [validation des modèles](../mvc/models/validation.md)et doit être effectuée dans chaque méthode d’API qui accepte une entrée d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="a6170-168">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

<span data-ttu-id="a6170-169">L’exemple utilise une énumération qui contient les codes d’erreur qui sont transmis sur le client mobile :</span><span class="sxs-lookup"><span data-stu-id="a6170-169">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

<span data-ttu-id="a6170-170">Ajout de nouveaux éléments à l’aide de Postman en choisissant le verbe POST en fournissant le nouvel objet au format JSON dans le corps de la demande de test.</span><span class="sxs-lookup"><span data-stu-id="a6170-170">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="a6170-171">Vous devez également ajouter un en-tête de demande spécifiant un `Content-Type` de `application/json`.</span><span class="sxs-lookup"><span data-stu-id="a6170-171">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![Affichage d’une publication et une réponse dans la console postman](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="a6170-173">La méthode retourne l’élément qui vient d’être créé dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="a6170-173">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="a6170-174">Mise à jour des éléments</span><span class="sxs-lookup"><span data-stu-id="a6170-174">Updating Items</span></span>

<span data-ttu-id="a6170-175">Modification des enregistrements s’effectue à l’aide de requêtes HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="a6170-175">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="a6170-176">Outre cette modification, le `Edit` méthode est presque identique à `Create`.</span><span class="sxs-lookup"><span data-stu-id="a6170-176">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="a6170-177">Notez que si l’enregistrement n’est trouvé, le `Edit` action retournera un `NotFound` réponse (404).</span><span class="sxs-lookup"><span data-stu-id="a6170-177">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

<span data-ttu-id="a6170-178">Pour tester avec Postman, remplacez le verbe PUT.</span><span class="sxs-lookup"><span data-stu-id="a6170-178">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="a6170-179">Spécifiez les données de l’objet mis à jour dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="a6170-179">Specify the updated object data in the Body of the request.</span></span>

![Affichage d’un PUT et réponse de la console postman](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="a6170-181">Cette méthode retourne un `NoContent` réponse (204) lors de la réussite, par souci de cohérence avec l’API préexistant.</span><span class="sxs-lookup"><span data-stu-id="a6170-181">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="a6170-182">Suppression d’éléments</span><span class="sxs-lookup"><span data-stu-id="a6170-182">Deleting Items</span></span>

<span data-ttu-id="a6170-183">La suppression d’enregistrements est effectuée par des demandes de suppression du service et en passant l’ID de l’élément à supprimer.</span><span class="sxs-lookup"><span data-stu-id="a6170-183">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="a6170-184">Mise à jour, recevoir des demandes pour les éléments qui n’existent pas dans `NotFound` réponses.</span><span class="sxs-lookup"><span data-stu-id="a6170-184">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="a6170-185">Sinon, une demande réussie obtiennent un `NoContent` réponse (204).</span><span class="sxs-lookup"><span data-stu-id="a6170-185">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

<span data-ttu-id="a6170-186">Notez que lorsque vous testez les fonctionnalités de suppression, rien n’est requis dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="a6170-186">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![Affichage d’une suppression et une réponse de la console postman](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="a6170-188">Conventions d’API Web courantes</span><span class="sxs-lookup"><span data-stu-id="a6170-188">Common Web API Conventions</span></span>

<span data-ttu-id="a6170-189">Lorsque vous développez les services principaux pour votre application, vous devez élaborer un jeu cohérent de conventions ou les stratégies de gestion des problèmes transversaux.</span><span class="sxs-lookup"><span data-stu-id="a6170-189">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="a6170-190">Par exemple, dans le service illustré ci-dessus, les demandes des enregistrements spécifiques qui n’ont pas été trouvés a reçu un `NotFound` réponse, plutôt qu’un `BadRequest` réponse.</span><span class="sxs-lookup"><span data-stu-id="a6170-190">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="a6170-191">De même, les commandes envoyées à ce service passé dans les types de modèle lié toujours vérifiées `ModelState.IsValid` et a retourné un `BadRequest` pour les types de modèle non valide.</span><span class="sxs-lookup"><span data-stu-id="a6170-191">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="a6170-192">Une fois que vous avez identifié une stratégie commune pour votre API, vous pouvez encapsuler généralement dans un [filtre](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="a6170-192">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="a6170-193">En savoir plus sur [comment encapsuler des politiques d’API communes dans les applications ASP.NET MVC de base](https://msdn.microsoft.com/magazine/mt767699.aspx).</span><span class="sxs-lookup"><span data-stu-id="a6170-193">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>
