---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Bien démarrer avec Entity Framework 6 Code First avec MVC 5 | Microsoft Docs
author: tdykstra
description: 'Une version plus récente de cette série de didacticiels est disponible : bien démarrer avec ASP.NET Core et Entity Framework Core à l’aide de Visual Studio 2015. Le Contoso Universi...'
ms.author: aspnetcontent
ms.date: 10/22/2015
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f03ddcf7dcc8b5d20c5459a7fb0015ab20f340c5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837167"
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>Bien démarrer avec Entity Framework 6 Code First en utilisant MVC 5
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [télécharger le PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > Une version plus récente de cette série de didacticiels est disponible : [bien démarrer avec ASP.NET Core et Entity Framework Core à l’aide de Visual Studio 2015](https://docs.asp.net/en/latest/data/ef-mvc/intro.html).
> 
> 
> L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de l’Entity Framework 6 et Visual Studio 2013. Ce didacticiel utilise le flux de travail Code First. Pour plus d’informations sur le choix entre Code First, Model First et Database First, consultez [Workflows de développement Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> L’exemple d’application est un site web pour une université Contoso fictive. Il inclut des fonctionnalités telles que l'admission d’étudiant, la création de cours et les affectations de formateur. Cette série de didacticiels explique comment générer l’exemple d’application Contoso University. Vous pouvez [Téléchargez l’application terminée](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
> 
> Une version de Visual Basic traduite par Mike Brind est disponible : [MVC 5 avec EF 6 en Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) sur le site Mikesdotnetting.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Entity Framework 6 (EntityFramework 6.1.0 NuGet package)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (facultatif)
>   
> 
> Le didacticiel doit également fonctionner avec [Visual Studio 2013 Express pour Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) ou Visual Studio 2012. Le [version de Visual Studio 2012 de Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323511) est requis pour le déploiement de Windows Azure avec Visual Studio 2012.
> 
> 
> ## <a name="tutorial-versions"></a>Versions de didacticiels
> 
> Pour les versions précédentes de ce didacticiel, consultez [le 4.1 EF / MVC 3 e-book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) et [bien démarrer avec EF 5 avec MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="questions-and-comments"></a>Questions et commentaires
> 
> Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), le [Entity Framework et LINQ au forum d’entités](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), ou [ StackOverflow.com](http://stackoverflow.com/).
> 
> Si vous rencontrez un problème que vous ne pouvez pas résoudre, vous trouverez généralement la solution au problème en comparant votre code pour le projet achevé que vous pouvez télécharger. Pour certaines erreurs courantes et comment les résoudre, consultez [les erreurs courantes et différents types de solutions pour eux.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>L’Application Web Contoso University

L’application que vous allez générer dans ces didacticiels est un site web simple d’université.

Les utilisateurs peuvent afficher et mettre à jour les informations relatives aux étudiants, aux cours et aux formateurs. Voici quelques écrans que vous allez créer.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Modifier un étudiant](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Le style d’interface utilisateur de ce site a été maintenu proche de ce qui est généré par les modèles intégrés, afin que le didacticiel puisse principalement se concentrer sur la façon d’utiliser Entity Framework.

## <a name="prerequisites"></a>Prérequis

Consultez **Versions logicielles** en haut de la page. Entity Framework 6 n’est pas requis, car vous installez le package NuGet d’Entity Framework en tant que partie du didacticiel.

## <a name="create-an-mvc-web-application"></a>Créer une Application Web MVC

Ouvrez Visual Studio et créez un projet Web c# nommé « ContosoUniversity ».

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

Dans le **nouveau projet ASP.NET** boîte de dialogue Sélectionnez le **MVC** modèle.

Si le **hôte dans le cloud** case à cocher dans la **Microsoft Azure** section est activée, désactivez-la.

Cliquez sur **modifier l’authentification**.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

Dans le **modifier l’authentification** boîte de dialogue, sélectionnez **aucune authentification**, puis cliquez sur **OK**. Pour ce didacticiel vous n’être obliger les utilisateurs à se connecter ou restreindre l’accès en fonction de qui est connecté.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Dans la boîte de dialogue Nouveau projet ASP.NET, cliquez sur **OK** pour créer le projet.

## <a name="set-up-the-site-style"></a>Configurer le Style du Site

Quelques changements simples configureront le menu, la disposition et la page d’accueil du site.

Ouvrez *Views\Shared\\_Layout.cshtml*et apportez les modifications suivantes :

- Remplacez chaque occurrence de « My Application ASP.NET » et « Nom de l’Application » à « Contoso University ».
- Ajouter des entrées de menu pour les étudiants, des cours, des formateurs et des services et supprimez l’entrée de Contact.

Les modifications apparaissent en surbrillance.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

Dans *Views\Home\Index.cshtml*, remplacez le contenu du fichier par le code suivant pour remplacer le texte sur ASP.NET et MVC avec le texte de cette application :

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Appuyez sur CTRL + F5 pour exécuter le site. Vous consultez la page d’accueil avec le menu principal.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Installer Entity Framework 6

À partir de la **outils** menu, cliquez sur **Gestionnaire de Package NuGet** puis cliquez sur **Console du Gestionnaire de Package**.

Dans le **Console du Gestionnaire de Package** fenêtre Entrez la commande suivante :

`Install-Package EntityFramework`

![EF installé](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

L’illustration montre 6.0.0 en cours d’installation, mais NuGet installe la dernière version d’Entity Framework (à l’exclusion des versions préliminaires), qui, à compter de la mise à jour plus récente pour le didacticiel est 6.1.1.

Cette étape est une des quelques étapes que ce didacticiel vous a à faire manuellement, mais qui aurait pu être fait automatiquement par la fonctionnalité de génération de modèles automatique ASP.NET MVC. Vous êtes les effectuer manuellement afin que vous puissiez voir les étapes requises pour utiliser Entity Framework. Vous utiliserez la structure ultérieurement pour créer le contrôleur MVC et les vues. Une alternative consiste à laisser la structure automatiquement installer le package NuGet d’EF, créer la classe de contexte de base de données et créer la chaîne de connexion. Lorsque vous êtes prêt à le faire de cette façon, il vous suffit est ignorer ces étapes et de structurer votre contrôleur MVC après avoir créé vos classes d’entité.

## <a name="create-the-data-model"></a>Créer le modèle de données

Ensuite, vous allez créer des classes d’entités pour l’application Contoso University. Vous commencerez par les trois entités suivantes :

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Il existe une relation un-à-plusieurs entre les entités `Student` et `Enrollment`, et une relation un-à-plusieurs entre les entités `Course` et `Enrollment`. En d’autres termes, un étudiant peut être inscrit dans un nombre quelconque de cours et un cours peut avoir un nombre quelconque d’élèves inscrits.

Dans les sections suivantes, vous allez créer une classe pour chacune de ces entités.

> [!NOTE]
> Si vous essayez de compiler le projet avant d’avoir créé toutes ces classes d’entité, vous obtiendrez des erreurs de compilateur.


### <a name="the-student-entity"></a>L’entité Student

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Dans le *modèles* dossier, créez un fichier de classe nommé *Student.cs* et remplacez le code du modèle par le code suivant :

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

La propriété `ID` devient la colonne de clé primaire de la table de base de données qui correspond à cette classe. Par défaut, Entity Framework interprète une propriété nommée `ID` ou *classname* `ID` comme clé primaire.

Le `Enrollments` propriété est un *propriété de navigation*. Les propriétés de navigation contiennent d’autres entités qui sont associées à cette entité. Dans ce cas, le `Enrollments` propriété d’un `Student` entité contiendra tous le `Enrollment` entités associées à ce `Student` entité. En d’autres termes, si une donnée `Student` ligne dans la base de données comporte deux liées `Enrollment` lignes (valeur des lignes qui contiennent la clé primaire de cette étudiant dans leurs `StudentID` colonne clé étrangère), qui `Student` l’entité `Enrollments` propriété de navigation contiendra ces deux `Enrollment` entités.

Propriétés de navigation sont généralement définies en tant que `virtual` afin qu’ils peuvent tirer parti de certaines fonctionnalités Entity Framework comme *le chargement différé*. (Le chargement différé est expliqué plus loin, dans le [lecture de données connexes](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) didacticiel plus loin dans cette série.)

Si une propriété de navigation peut contenir plusieurs entités (comme dans des relations plusieurs à plusieurs ou un -à-plusieurs), son type doit être une liste dans laquelle les entrées peuvent être ajoutées, supprimées et mises à jour, telle que `ICollection`.

### <a name="the-enrollment-entity"></a>L’entité Enrollment

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

Dans le dossier *Models*, créez *Enrollment.cs* et remplacez le code existant par le code suivant :

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

Le `EnrollmentID` propriété sera la clé primaire ; cette entité utilise le *classname* `ID` de modèle au lieu de `ID` par lui-même, comme vous l’avez vu dans la `Student` entité. En général, vous choisissez un modèle et l’utilisez dans tout votre modèle de données. Ici, la variante illustre que vous pouvez utiliser l’un ou l’autre modèle. Dans un prochain didacticiel, vous verrez comment l’utilisation `ID` sans `classname` rend plus facile à implémenter l’héritage dans le modèle de données.

Le `Grade` propriété est un [enum](https://msdn.microsoft.com/data/hh859576.aspx). Le point d’interrogation après la `Grade` déclaration de type indique que le `Grade` propriété est [nullable](https://msdn.microsoft.com/library/2cf62fcy.aspx). Un niveau qui a la valeur null est différent à partir d’une note égale à zéro, cela signifie qu’une note n’est pas connue ou n’a pas encore été affectée.

La propriété `StudentID` est une clé étrangère et la propriété de navigation correspondante est `Student`. Une entité `Enrollment` est associée à une entité `Student`, donc la propriété peut contenir uniquement une entité `Student` unique (contrairement à la propriété de navigation `Student.Enrollments` que vous avez vue précédemment, qui peut contenir plusieurs entités `Enrollment`).

La propriété `CourseID` est une clé étrangère et la propriété de navigation correspondante est `Course`. Une entité `Enrollment` est associée à une entité `Course`.

Entity Framework interprète une propriété comme une propriété de clé étrangère si elle se nomme *&lt;nom de propriété de navigation&gt;&lt;nom de propriété de clé primaire&gt;* (par exemple, `StudentID`pour le `Student` propriété de navigation depuis le `Student` clé primaire de l’entité est `ID`). Propriétés de clé étrangère peuvent également être nommées les mêmes simplement *&lt;nom de propriété de clé primaire&gt;* (par exemple, `CourseID` dans la mesure où le `Course` clé primaire de l’entité est `CourseID`).

### <a name="the-course-entity"></a>L’entité Course

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

Dans le *modèles* dossier, créez *Course.cs*, en remplaçant le code du modèle par le code suivant :

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

La propriété `Enrollments` est une propriété de navigation. Une entité `Course` peut être associée à un nombre quelconque d’entités `Enrollment`.

Nous fournirons plus de détails sur la [DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx) attribut dans un didacticiel plus loin dans cette série. En fait, cet attribut vous permet d’entrer la clé primaire pour le cours plutôt que de laisser la base de données la générer.

## <a name="create-the-database-context"></a>Créer le contexte de base de données

La classe principale qui coordonne les fonctionnalités d’Entity Framework pour un modèle de données spécifié est le *contexte de base de données* classe. Vous créez cette classe en dérivant le [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) classe. Dans votre code, vous spécifiez les entités qui sont incluses dans le modèle de données. Vous pouvez également personnaliser un certain comportement d’Entity Framework. Dans ce projet, la classe est nommée `SchoolContext`.

Pour créer un dossier dans le projet ContosoUniversity, cliquez sur le projet dans **l’Explorateur de solutions** et cliquez sur **ajouter**, puis cliquez sur **nouveau dossier**. Nommez le nouveau dossier *DAL* (pour la couche d’accès aux données). Dans ce dossier, créez un nouveau fichier de classe nommé *SchoolContext.cs*et remplacez le code du modèle par le code suivant :

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>En spécifiant les jeux d’entités

Ce code crée un [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) propriété pour chaque jeu d’entités. Dans la terminologie Entity Framework, un *jeu d’entités* correspond généralement à une table de base de données et un *entité* correspond à une ligne dans la table.

> [!NOTE] 
> 
> Vous pouvez avoir omis les `DbSet<Enrollment>` et `DbSet<Course>` instructions et elle fonctionnerait de la même. Entity Framework les inclurait implicitement, car l’entité `Student` référence l’entité `Enrollment`, et l’entité `Enrollment` référence l’entité `Course`.


### <a name="specifying-the-connection-string"></a>Spécification de la chaîne de connexion

Le nom de la chaîne de connexion (que vous allez ajouter au fichier Web.config plus loin) est transmis au constructeur.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Vous pouvez également transmettre la chaîne de connexion elle-même au lieu du nom d’un qui est stocké dans le fichier Web.config. Pour plus d’informations sur les options permettant de spécifier la base de données à utiliser, consultez [Entity Framework - connexions et des modèles](https://msdn.microsoft.com/data/jj592674).

Si vous ne spécifiez pas une chaîne de connexion ou le nom d’une manière explicite, Entity Framework suppose que le nom de chaîne de connexion est le même que le nom de classe. Le nom de chaîne de connexion par défaut dans cet exemple serait alors `SchoolContext`, identique à ce que vous spécifiez explicitement.

### <a name="specifying-singular-table-names"></a>Spécification des noms de tables au singulier

Le `modelBuilder.Conventions.Remove` instruction dans le [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) méthode empêche l’en cours mis au pluriel des noms de table. Si vous n’avez pas le faire, les tables générées dans la base de données seraient nommés `Students`, `Courses`, et `Enrollments`. Au lieu de cela, les noms de table seront `Student`, `Course`, et `Enrollment`. Les développeurs ne sont pas tous d’accord sur la nécessité d’utiliser des noms de table au pluriel. Ce didacticiel utilise la forme au singulier, mais le point important est que vous pouvez sélectionner quelle que soit la forme que vous préférez en incluant ou l’omission de cette ligne de code.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Configuration d’EF pour initialiser la base de données de test

Entity Framework peut automatiquement créer (ou supprimer et recréer) une base de données pour vous lors de l’application s’exécute. Vous pouvez spécifier qu’il doit être effectuée chaque fois que votre application s’exécute ou seulement lorsque le modèle est désynchronisé avec la base de données existante. Vous pouvez également écrire un `Seed` méthode qu’Entity Framework appelle automatiquement après avoir créé la base de données pour la remplir avec les données de test.

Le comportement par défaut consiste à créer une base de données uniquement si elle n’existe pas (et lever une exception si le modèle a changé et que la base de données existe déjà). Dans cette section, vous allez spécifier que la base de données doit être supprimée et recréée chaque fois que le modèle change. Suppression de la base de données entraîne la perte de toutes vos données. Il s’agit généralement OK pendant le développement, car la `Seed` méthode s’exécute lorsque la base de données est recréée et recrée vos données de test. Mais en production vous souhaitez généralement perdre toutes vos données chaque fois que vous devez modifier le schéma de base de données. Plus tard, vous verrez comment gérer les modifications apportées au modèle à l’aide des Migrations Code First pour modifier le schéma de base de données au lieu de devoir supprimer et recréer la base de données.

Dans le dossier de la couche DAL, créez un nouveau fichier de classe nommé *SchoolInitializer.cs* et remplacez le code du modèle avec le  
suivant le code, ce qui entraîne une base de données doit être créé lorsque nécessaire et charge les données de test dans la base de données.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

Le `Seed` méthode prend l’objet de contexte de base de données comme paramètre d’entrée, et le code dans la méthode utilise cet objet pour ajouter de nouvelles entités à la base de données. Pour chaque type d’entité, le code crée une collection de nouvelles entités, les ajoute à approprié `DbSet` propriété, puis enregistre les modifications apportées à la base de données. Il n’est pas nécessaire d’appeler le `SaveChanges` méthode après chaque groupe d’entités, comme le fait ici, mais cela vous permet de localiser la source d’un problème si une exception se produit pendant que le code écrit pour la base de données.

Pour indiquer à Entity Framework à utiliser votre classe d’initialiseur, ajoutez un élément à la `entityFramework` élément dans l’application *Web.config* fichier (l’un dans le dossier racine du projet), comme indiqué dans l’exemple suivant :

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

Le `context type` Spécifie le nom de classe de contexte complet et l’assembly, il se trouve dans, et le `databaseinitializer type` Spécifie le nom qualifié complet de la classe de l’initialiseur et de l’assembly. (Lorsque vous ne souhaitez pas EF d’utiliser l’initialiseur, vous pouvez définir un attribut sur le `context` élément : `disableDatabaseInitialization="true"`.) Pour plus d’informations, consultez [Entity Framework - paramètres du fichier Config](https://msdn.microsoft.com/data/jj556606).

Comme alternative à la définition de l’initialiseur dans le *Web.config* fichier consiste à faire dans le code en ajoutant un `Database.SetInitializer` instruction à la `Application_Start` méthode dans le *Global.asax.cs* fichier. Pour plus d’informations, consultez [compréhension des initialiseurs de base de données dans Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

L’application est maintenant définie jusqu'à afin que lorsque vous accédez à la base de données pour la première fois dans une exécution donnée de la  
application, Entity Framework compare la base de données au modèle (votre `SchoolContext` et classes d’entité). S’il existe une différence, l’application supprime et recrée la base de données.

> [!NOTE]
> Lorsque vous déployez une application sur un serveur web de production, vous devez supprimer ou désactiver le code qui supprime et recrée la base de données. Vous pouvez utiliser dans un didacticiel plus loin dans cette série.


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Configuration d’EF pour utiliser une base de données SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) est une version légère du moteur de base de données SQL Server Express. Il est facile à installer et configurer démarre à la demande et s’exécute en mode utilisateur. Base de données locale s’exécute dans un mode spécial de l’exécution de SQL Server Express qui vous permet de travailler avec les bases de données en tant que *.mdf* fichiers. Vous pouvez placer les fichiers de base de données de base de données locale dans le *application\_données* dossier d’un projet web si vous souhaitez être en mesure de copier la base de données avec le projet. La fonctionnalité d’instance utilisateur dans SQL Server Express vous permet également de travailler avec *.mdf* fichiers, mais la fonctionnalité d’instance utilisateur est déconseillé ; par conséquent, il est recommandé LocalDB pour travailler avec *.mdf* fichiers. Dans Visual Studio 2012 et versions ultérieures, la base de données locale est installé par défaut avec Visual Studio.

SQL Server Express est généralement pas utilisé pour les applications web de production. Base de données locale en particulier est déconseillé pour la production avec une application web, car il n’est pas conçu pour fonctionner avec IIS.

Dans ce didacticiel, vous allez utiliser avec la base de données locale. Ouvrez l’application *Web.config* fichier, puis ajoutez un `connectionStrings` élément précédant le `appSettings` élément, comme indiqué dans l’exemple suivant. (Assurez-vous que vous mettez à jour le *Web.config* fichier dans le dossier racine du projet. Il existe également un *Web.config* fichier se trouve dans le *vues* sous-dossier que vous n’avez pas besoin de mettre à jour.)

Si vous utilisez Visual Studio 2015, remplacez « v11.0 » dans la chaîne de connexion par « MSSQLLocalDB », car le nom d’instance de SQL Server par défaut a été modifié.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

La chaîne de connexion que vous avez ajouté Spécifie que Entity Framework utilise une base de données de base de données locale nommée *ContosoUniversity1.mdf*. (La base de données n’existe pas encore ; EF crée.) Si vous souhaitez que la base de données doit être créé dans votre *application\_données* dossier, vous pouvez ajouter `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` à la chaîne de connexion. Pour plus d’informations sur les chaînes de connexion, consultez [chaînes de connexion SQL Server pour les Applications Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Vous n’êtes pas obligé en fait avoir une chaîne de connexion dans le *Web.config* fichier. Si vous ne fournissez pas une chaîne de connexion, Entity Framework utilise une basée sur votre classe de contexte par défaut. Pour plus d’informations, consultez [Code First pour une base de données](https://msdn.microsoft.com/data/jj193542).

## <a name="creating-a-student-controller-and-views"></a>Création d’un contrôleur d’étudiants et des vues

Maintenant, vous allez créer une page web pour afficher des données, et le processus de demande les données déclenche automatiquement  
la création de la base de données. Vous commencerez par créer un nouveau contrôleur. Mais avant cela, générez le projet pour rendre les classes de modèle et au contexte disponible pour la structure de contrôleur MVC.

1. Avec le bouton droit le **contrôleurs** dossier **l’Explorateur de solutions**, sélectionnez **ajouter**, puis cliquez sur **nouvel élément structuré**.
2. Dans le **ajouter une structure** boîte de dialogue, sélectionnez **contrôleur MVC 5 avec vues, utilisant Entity Framework**.

     ![Ajouter une structure](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
3. Dans la boîte de dialogue Ajouter un contrôleur, effectuez les sélections suivantes, puis cliquez sur **ajouter**:

   - Classe de modèle : **étudiant (ContosoUniversity.Models)**. (Si vous ne voyez pas cette option dans la liste déroulante, générez le projet et réessayez.)
   - Classe de contexte de données : **SchoolContext (ContosoUniversity.DAL)**.
   - Nom du contrôleur : **StudentController** (pas StudentsController).
   - Conservez les valeurs par défaut pour les autres champs.

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

     Lorsque vous cliquez sur **ajouter**, le Générateur de modèles automatique crée un fichier StudentController.cs et un ensemble de vues (fichiers .cshtml) qui fonctionnent avec le contrôleur. Dans l’avenir, lorsque vous créez des projets qui utilisent Entity Framework vous pouvez également tirer parti des fonctionnalités supplémentaires de la génération de modèles automatique : simplement créer votre première classe de modèle, ne créez pas une chaîne de connexion, puis dans le **ajouter un contrôleur** boîte spécifier la nouvelle classe de contexte. Le Générateur de modèles automatique crée votre `DbContext` votre connexion et la classe de chaîne, ainsi que le contrôleur et des vues.
4. Visual Studio ouvre le *Controllers\StudentController.cs* fichier. Vous voyez qu'une variable de classe a été créée qui instancie un objet de contexte de base de données :

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     Le `Index` méthode d’action Obtient une liste d’étudiants à partir de la *étudiants* entité définie en lisant le `Students` propriété de l’instance de contexte de base de données :

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     Le *Student\Index.cshtml* affiche cette liste dans une table :

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Appuyez sur CTRL+F5 pour exécuter le projet. (Si vous obtenez une erreur « Impossible de créer le cliché instantané », fermez le navigateur et réessayez.)

     Cliquez sur le **étudiants** onglet pour afficher les données de test qui le `Seed` méthode inséré. Selon l’étroitesse ce champ est la fenêtre du navigateur, vous verrez le lien de l’onglet étudiant dans la barre d’adresses principales, ou vous devrez cliquez sur le coin supérieur droit pour afficher le lien.

     ![Bouton de menu](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

     ![Page d’Index pour les étudiants](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Afficher la base de données

Lorsque vous avez exécuté la page des étudiants et que l’application a essayé d’accéder à la base de données, EF a vu qu’il n’y a aucune base de données et donc il créé un, puis il a été exécuté la méthode seed pour remplir la base de données.

Vous pouvez utiliser **Explorateur de serveurs** ou **Explorateur d’objets SQL Server** (SSOX) pour afficher la base de données dans Visual Studio. Pour ce didacticiel, vous allez utiliser **Explorateur de serveurs**. (Dans les éditions de Visual Studio Express antérieure à 2013, **Explorateur de serveurs** est appelée **Database Explorer**.)

1. Fermez le navigateur.
2. Dans **Explorateur de serveurs**, développez **des connexions de données**, développez **contexte School (ContosoUniversity)**, puis développez **Tables** pour voir les tables dans votre nouvelle base de données.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. Avec le bouton droit le **étudiant** de table et cliquez sur **afficher les données de Table** pour afficher les colonnes qui ont été créés et les lignes qui ont été insérées dans la table.

    ![Table Student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. Fermer le **Explorateur de serveurs** connexion.

Le *ContosoUniversity1.mdf* et *.ldf* les fichiers de base de données se trouvent dans le `C:\Users\<yourusername>` dossier.

Car vous utilisez le `DropCreateDatabaseIfModelChanges` initialiseur, vous pouvez maintenant apporter une modification apportée à la `Student` classe, exécutez à nouveau l’application, et la base de données serait automatiquement recréée conformément à vos modifications. Par exemple, si vous ajoutez un `EmailAddress` propriété le `Student` classe, réexécutez la page des étudiants, et examinez la table à nouveau, vous verrez un nouveau `EmailAddress` colonne.

## <a name="conventions"></a>Conventions

La quantité de code que vous deviez écrire dans l’ordre pour Entity Framework pouvoir créer une base de données complète pour vous est minime en raison de l’utilisation de *conventions*, ou hypothèses qui rend l’Entity Framework. Certains d'entre eux ont déjà été observées ou ont été utilisées sans votre connaître les :

- Les formulaires pluralisées des noms de classes d’entité sont utilisés comme noms de tables.
- Les noms de propriété d’entité sont utilisées pour les noms de colonne.
- Propriétés de l’entité qui sont nommées `ID` ou *classname* `ID` sont reconnus comme des propriétés de clé primaire.
- Une propriété est interprétée comme une propriété de clé étrangère si elle se nomme *&lt;nom de propriété de navigation&gt;&lt;nom de propriété de clé primaire&gt;* (par exemple, `StudentID` pour la `Student` propriété de navigation depuis le `Student` clé primaire de l’entité est `ID`). Propriétés de clé étrangère peuvent également être nommées les mêmes simplement &lt;nom de propriété de clé primaire&gt; (par exemple, `EnrollmentID` dans la mesure où le `Enrollment` clé primaire de l’entité est `EnrollmentID`).

Vous avez vu que les conventions peuvent être substituées. Par exemple, vous avez spécifié que les noms de table ne doit pas être mis au pluriel, et vous verrez plus tard comment marquer explicitement une propriété comme une propriété de clé étrangère. Vous en apprendrez davantage sur les conventions et comment les remplacer dans les [création d’un modèle de données plus complexes](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) didacticiel plus loin dans cette série. Pour plus d’informations sur les conventions, consultez [Conventions Code First](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Récapitulatif

Vous avez maintenant créé une application simple qui utilise Entity Framework et SQL Server Express LocalDB pour stocker et afficher des données. Dans ce didacticiel, vous allez apprendre à effectuer des CRUD de base (créer, lire, mettre à jour, supprimer) les opérations.

Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer. Vous pouvez également demander de nouvelles rubriques à [afficher les leçons de Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Vous trouverez des liens vers d’autres ressources Entity Framework dans [accès aux données ASP.NET - ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Next](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
