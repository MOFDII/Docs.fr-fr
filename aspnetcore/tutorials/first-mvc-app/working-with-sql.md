---
title: Utilisation de SQL Server LocalDB
author: rick-anderson
description: Utilisation de SQL Server LocalDB avec une application MVC simple
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 14b0f04ce904b34a03b3c5160e189e2e6d045734
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="working-with-sql-server-localdb"></a><span data-ttu-id="00383-103">Utilisation de SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="00383-103">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="00383-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="00383-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="00383-105">L’objet `MvcMovieContext` gère la tâche de connexion à la base de données et de mappage d’objets `Movie` à des enregistrements de la base de données.</span><span class="sxs-lookup"><span data-stu-id="00383-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="00383-106">Le contexte de base de données est inscrit auprès du conteneur [Injection de dépendances](xref:fundamentals/dependency-injection) dans la méthode `ConfigureServices` du fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="00383-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

<span data-ttu-id="00383-107">Le système de [configuration](xref:fundamentals/configuration/index) d’ASP.NET Core lit `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="00383-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="00383-108">Pour un développement local, il obtient la chaîne de connexion à partir du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="00383-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[Main](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="00383-109">Quand vous déployez l’application sur un serveur de test ou de production, vous pouvez utiliser une variable d’environnement ou une autre approche pour définir un serveur SQL Server réel comme chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="00383-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="00383-110">Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="00383-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="00383-111">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="00383-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="00383-112">LocalDB est une version allégée du moteur de base de données SQL Server Express qui est ciblée pour le développement de programmes.</span><span class="sxs-lookup"><span data-stu-id="00383-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that is targeted for program development.</span></span> <span data-ttu-id="00383-113">LocalDB démarre à la demande et s’exécute en mode utilisateur, ce qui n’implique aucune configuration complexe.</span><span class="sxs-lookup"><span data-stu-id="00383-113">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="00383-114">Par défaut, la base de données LocalDB crée des fichiers « \*.mdf » dans le répertoire *C:/Users/\<utilisateur\>*.</span><span class="sxs-lookup"><span data-stu-id="00383-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="00383-115">Dans le menu **Affichage**, ouvrez **l’Explorateur d’objets SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="00383-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu View](working-with-sql/_static/ssox.png)

* <span data-ttu-id="00383-117">Cliquez avec le bouton droit sur la table `Movie` **> Concepteur de vue**.</span><span class="sxs-lookup"><span data-stu-id="00383-117">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menu contextuel ouvert sur la table Movie](working-with-sql/_static/design.png)

  ![Table Movie ouverte dans le Concepteur](working-with-sql/_static/dv.png)

<span data-ttu-id="00383-120">Notez l’icône de clé en regard de `ID`.</span><span class="sxs-lookup"><span data-stu-id="00383-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="00383-121">Par défaut, EF fait d’une propriété nommée `ID` la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="00383-121">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="00383-122">Cliquez avec le bouton droit sur la table `Movie` **> Afficher les données**.</span><span class="sxs-lookup"><span data-stu-id="00383-122">Right click on the `Movie` table **> View Data**</span></span>

  ![Menu contextuel ouvert sur la table Movie](working-with-sql/_static/ssox2.png)

  ![Table Movie ouverte, affichant des données de table](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="00383-125">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="00383-125">Seed the database</span></span>

<span data-ttu-id="00383-126">Créez une classe nommée `SeedData` dans l’espace de noms *Modèles*.</span><span class="sxs-lookup"><span data-stu-id="00383-126">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="00383-127">Remplacez le code généré par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="00383-127">Replace the generated code with the following:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="00383-128">Si la base de données contient des films, l’initialiseur de valeur initiale retourne une valeur et aucun film n’est ajouté.</span><span class="sxs-lookup"><span data-stu-id="00383-128">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="00383-129">Ajouter l’initialiseur de valeur initiale</span><span class="sxs-lookup"><span data-stu-id="00383-129">Add the seed initializer</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="00383-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="00383-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="00383-131">Ajoutez l’initialiseur de valeur initiale à la méthode `Main` dans le fichier *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="00383-131">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="00383-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="00383-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="00383-133">Ajoutez l’initialiseur de valeur initiale à la fin de la méthode `Configure` dans le fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="00383-133">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---

<span data-ttu-id="00383-134">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="00383-134">Test the app</span></span>

* <span data-ttu-id="00383-135">Supprimez tous les enregistrements de la base de données.</span><span class="sxs-lookup"><span data-stu-id="00383-135">Delete all the records in the DB.</span></span> <span data-ttu-id="00383-136">Pour ce faire, utilisez les liens de suppression disponibles dans le navigateur ou à partir de SSOX.</span><span class="sxs-lookup"><span data-stu-id="00383-136">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="00383-137">Forcez l’application à s’initialiser (appelez les méthodes de la classe `Startup`) pour que la méthode seed s’exécute.</span><span class="sxs-lookup"><span data-stu-id="00383-137">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="00383-138">Pour forcer l’initialisation, IIS Express doit être arrêté et redémarré.</span><span class="sxs-lookup"><span data-stu-id="00383-138">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="00383-139">Pour cela, adoptez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="00383-139">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="00383-140">Cliquez avec le bouton droit sur l’icône de barre d’état système IIS Express dans la zone de notification, puis appuyez sur **Quitter** ou sur **Arrêter le site**.</span><span class="sxs-lookup"><span data-stu-id="00383-140">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Icône de la barre d’état système IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menu contextuel](working-with-sql/_static/stopIIS.png)

   * <span data-ttu-id="00383-143">Si vous exécutiez Visual Studio en mode non-débogage, appuyez sur F5 pour l’exécuter en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="00383-143">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
   * <span data-ttu-id="00383-144">Si vous exécutiez Visual Studio en mode débogage, arrêtez le débogueur et appuyez sur F5.</span><span class="sxs-lookup"><span data-stu-id="00383-144">If you were running VS in debug mode, stop the debugger and press F5</span></span>
   
<span data-ttu-id="00383-145">L’application affiche les données de départ.</span><span class="sxs-lookup"><span data-stu-id="00383-145">The app shows the seeded data.</span></span>

![Application MVC Movie ouverte dans Microsoft Edge, affichant les données relatives aux films](working-with-sql/_static/m55.png)

>[!div class="step-by-step"]
<span data-ttu-id="00383-147">[Précédent](adding-model.md)
[Suivant](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="00383-147">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
