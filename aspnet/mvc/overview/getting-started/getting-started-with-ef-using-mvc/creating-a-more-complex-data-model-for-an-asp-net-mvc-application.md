---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: Création d’un modèle de données plus complexe pour une Application ASP.NET MVC | Documents Microsoft
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de l’Entity Framework 6 Code First et Visual Studio en cours...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: fd8bf6502b0dd261505a86a2ed86d4c3f42e8755
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877097"
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application"></a>Création d’un modèle de données plus complexe pour une Application ASP.NET MVC
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [télécharger le PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de l’Entity Framework 6 Code First et Visual Studio 2013. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Dans les didacticiels précédents, vous avez travaillé avec un modèle de données simple composé de trois entités. Dans ce didacticiel, vous ajouterez des entités et relations, et vous allez personnaliser le modèle de données en spécifiant la mise en forme, la validation et les règles de mappage de base de données. Vous verrez deux façons de personnaliser le modèle de données : en ajoutant des attributs aux classes d’entité et en ajoutant du code à la classe de contexte de base de données.

Lorsque vous aurez terminé, les classes d’entité composeront le modèle de données complet indiqué dans l’illustration suivante :

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Personnaliser le modèle de données à l’aide d’attributs

Dans cette section, vous allez apprendre à personnaliser le modèle de données en utilisant des attributs qui spécifient des règles de mise en forme, de validation et de mappage de base de données. Ensuite, dans plusieurs des sections suivantes, vous allez créer l’intégralité `School` modèle de données en ajoutant des attributs aux classes déjà créées et création de nouvelles classes pour les types d’entités restants dans le modèle.

### <a name="the-datatype-attribute"></a>L’attribut de type de données

Pour les dates d’inscription des étudiants, toutes les pages web affichent l’heure avec la date, alors que seule la date vous intéresse dans ce champ. Vous pouvez avoir recours aux attributs d’annotation de données pour apporter une modification au code, permettant de corriger le format d’affichage dans chaque vue qui affiche ces données. Pour voir un exemple de la procédure à suivre, vous allez ajouter un attribut à la propriété `EnrollmentDate` dans la classe `Student`.

Dans *Models\Student.cs*, ajouter un `using` instruction pour le `System.ComponentModel.DataAnnotations` espace de noms et ajoutez `DataType` et `DisplayFormat` des attributs à la `EnrollmentDate` la propriété, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

Le [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attribut est utilisé pour spécifier un type de données qui est plus spécifique que le type intrinsèque de la base de données. Dans le cas présent, nous voulons uniquement effectuer le suivi de la date, pas de la date et de l’heure. Le [énumération DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fournit de nombreux types de données, tels que *Date, heure, numéro de téléphone, devise, EmailAddress* et bien plus encore. L’attribut `DataType` peut également permettre à l’application de fournir automatiquement des fonctionnalités propres au type. Par exemple, un `mailto:` lien peut être créé pour [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), et un sélecteur de date peut être fourni pour [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) dans les navigateurs qui prennent en charge [HTML5](http://html5.org/). Le [type de données](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) émet des attributs HTML 5 [données -](http://ejohn.org/blog/html-5-data-attributes/) (prononcé *data tiret*) les attributs que les navigateurs HTML 5 peuvent comprendre. Le [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attributs ne fournissent pas de validation.

`DataType.Date` ne spécifie pas le format de la date qui s’affiche. Par défaut, le champ de données s’affiche selon les formats par défaut basés sur le serveur [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

L’attribut `DisplayFormat` est utilisé pour spécifier explicitement le format de date :


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


Le `ApplyFormatInEditMode` paramètre spécifie que la mise en forme spécifiée doit également être appliquée lorsque la valeur est affichée dans une zone de texte pour la modification. (Vous pouvez que pour certains champs, par exemple, pour les valeurs de devise, vous pouvez le symbole de devise dans la zone de texte pour le modifier.)

Vous pouvez utiliser la [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribut par lui-même, mais il est généralement judicieux d’utiliser le [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) également d’attribut. Le `DataType` attribut transmet le *sémantique* des données en opposition à comment effectuer le rendu sur un écran et offre les avantages suivants que vous n’obtenez pas avec `DisplayFormat`:

- Le navigateur peut activer des fonctionnalités HTML5 (par exemple pour afficher un contrôle de calendrier, le symbole monétaire correspondant aux paramètres régionaux, des liens de messagerie, une certaine validation des entrées côté client, etc.).
- Par défaut, le navigateur renvoie les données en utilisant le format approprié en fonction de votre [paramètres régionaux](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- Le [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attribut peut activer MVC choisir le modèle de champ de droite pour afficher les données (la [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) utilise le modèle de chaîne). Pour plus d’informations, consultez de Brad Wilson [modèles ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Bien que rédigées pour MVC 2, cet article toujours s’applique à la version actuelle d’ASP.NET MVC.)

Si vous utilisez la `DataType` attribut avec un champ de date, vous devez spécifier le `DisplayFormat` attribut également afin de garantir que le champ s’affiche correctement dans les navigateurs de Chrome. Pour plus d’informations, consultez [ce thread StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Pour plus d’informations sur la gestion d’autres formats de date dans MVC, accédez à [MVC 5 Introduction : examen des méthodes de modification et modifier la vue](../introduction/examining-the-edit-methods-and-edit-view.md) et recherche dans la page pour &quot;internationalisation&quot;.

Réexécutez la page d’Index de l’étudiant et notez que fois ne sont plus affichés pour les dates d’inscription. Le même aura la valeur true pour n’importe quelle vue qui utilise le `Student` modèle.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

Vous pouvez également spécifier les règles de validation de données et les messages d’erreur de validation à l’aide d’attributs. Le [StringLength attribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) définit la longueur maximale de la base de données et fournit le côté client et côté serveur validation pour ASP.NET MVC. Vous pouvez également spécifier la longueur de chaîne minimale dans cet attribut, mais la valeur minimale n’a aucun impact sur le schéma de base de données.

Supposons que vous voulez garantir que les utilisateurs n’entrent pas plus de 50 caractères pour un nom. Pour ajouter cette limitation, ajoutez [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) des attributs à la `LastName` et `FirstMidName` propriétés, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

Le [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribut ne sera pas empêcher un utilisateur d’entrer un espace blanc pour un nom. Vous pouvez utiliser la [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attribut à appliquer des restrictions à l’entrée. Par exemple, le code suivant requiert le premier caractère en majuscules et les autres caractères alphabétique :

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

Le [MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) attribut offre une fonctionnalité similaire à la [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) d’attribut, mais ne fournit pas côté client validation.

Exécutez l’application et cliquez sur le **étudiants** onglet. Vous obtenez l’erreur suivante :

*Le modèle de sauvegarde le contexte 'SchoolContext' a changé depuis la création de la base de données. Envisagez d’utiliser les Migrations Code First pour mettre à jour la base de données ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Le modèle de base de données a changé d’une manière qui nécessite une modification dans le schéma de base de données et Entity Framework a détecté que. Migrations vous permet de mettre à jour le schéma sans perdre de données que vous avez ajouté à la base de données à l’aide de l’interface utilisateur. Si vous avez modifié les données qui a été créées par le `Seed` (méthode), qui est modifiée à son état d’origine car le [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) méthode que vous utilisez dans le `Seed` (méthode). ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) équivaut à une opération « upsert » à partir de la terminologie de base de données.)

Dans la console du Gestionnaire de package, entrez les commandes suivantes :

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

Le `add-migration` commande crée un fichier nommé  *&lt;timeStamp&gt;\_MaxLengthOnNames.cs*. Ce fichier contient du code dans la méthode `Up` qui met à jour la base de données pour qu’elle corresponde au modèle de données actuel. La commande `update-database` a exécuté ce code.

L’horodatage qui précède le nom de fichier migrations est utilisée par Entity Framework pour classer les migrations. Vous pouvez créer plusieurs migrations avant d’exécuter le `update-database` commande, puis toutes les migrations sont appliquées dans l’ordre dans lequel ils ont été créés.

Exécutez le **créer** page et entrez le nom de plus de 50 caractères. Lorsque vous cliquez sur **Create**, la validation côté client affiche un message d’erreur.

![Erreur de val de côté client](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>L’attribut de colonne

Vous pouvez également utiliser des attributs pour contrôler la façon dont les classes et les propriétés sont mappées à la base de données. Supposons que vous aviez utilisé le nom `FirstMidName` pour le champ de prénom, car le champ peut également contenir un deuxième prénom. Mais vous souhaitez que la colonne de base de données soit nommée `FirstName`, car les utilisateurs qui écriront des requêtes ad-hoc par rapport à la base de données sont habitués à ce nom. Pour effectuer ce mappage, vous pouvez utiliser l’attribut `Column`.

L’attribut `Column` spécifie que lorsque la base de données sera créée, la colonne de la table `Student` qui est mappée sur la propriété `FirstMidName` sera nommée `FirstName`. En d’autres termes, lorsque votre code fait référence à `Student.FirstMidName`, les données proviennent de la colonne `FirstName` de la table `Student` ou y sont mises à jour. Si vous ne spécifiez pas les noms de colonnes, elles reçoivent le même nom que le nom de propriété.

Dans le *Student.cs* , ajoutez un `using` instruction pour [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) et ajoutez l’attribut de nom de colonne à la `FirstMidName` propriété, comme indiqué dans les code en surbrillance suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

L’ajout de la [attribut de colonne](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) modifie le modèle de sauvegarde le SchoolContext, donc il ne correspond pas à la base de données. Entrez les commandes suivantes dans le PMC pour créer une autre migration :

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

Dans **l’Explorateur de serveurs**, ouvrez le *Student* Concepteur de tables en double-cliquant sur le *étudiant* table.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

L’illustration suivante montre le nom de colonne d’origine tel qu’il était avant d’appliquer les deux premières migrations. En plus du nom de colonne de `FirstMidName` à `FirstName`, les deux colonnes de nom ont changé depuis `MAX` longueur à 50 caractères.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Vous pouvez également modifier base de données de mappage à l’aide de la [API Fluent](https://msdn.microsoft.com/data/jj591617), comme vous le verrez plus loin dans ce didacticiel.

> [!NOTE]
> Si vous essayez de compiler avant d’avoir fini de créer toutes les classes d’entité dans les sections suivantes, vous pouvez obtenir des erreurs de compilation.


## <a name="complete-changes-to-the-student-entity"></a>Terminer les modifications apportées à l’entité Student

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Dans *Models\Student.cs*, remplacez le code que vous avez ajouté précédemment par le code suivant. Les modifications apparaissent en surbrillance.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>L’attribut requis

Le [attribut requis](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) rend les champs obligatoires de propriétés de nom. Le `Required attribute` n’est pas nécessaire pour les types valeur tels que DateTime, int, double et float. Types valeur ne peut pas avoir une valeur null, et sont par nature traitées en tant que champs obligatoires. Vous pouvez supprimer la [attribut requis](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) et remplacez-le par un paramètre de longueur minimale pour le `StringLength` attribut :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

### <a name="the-display-attribute"></a>L’attribut d’affichage

L’attribut `Display` spécifie que la légende pour les zones de texte doit être « First Name », « Last Name », « Full Name » et « Enrollment Date », au lieu du nom de propriété dans chaque instance (qui n’a pas d’espace pour séparer les mots).

### <a name="the-fullname-calculated-property"></a>Le nom complet de propriété calculée

`FullName` est une propriété calculée qui retourne une valeur créée par concaténation de deux autres propriétés. Par conséquent, il a uniquement un `get` accesseur et non `FullName` colonne sera générée dans la base de données.

## <a name="create-the-instructor-entity"></a>Créer l’entité Instructor

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Créer *Models\Instructor.cs*, en remplaçant le code du modèle avec le code suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

Notez que plusieurs propriétés sont identiques dans les entités `Student` et `Instructor`. Dans le didacticiel [Implémentation de l’héritage](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) plus loin dans cette série, vous allez refactoriser ce code pour éliminer la redondance.

Vous pouvez placer plusieurs attributs sur une seule ligne, donc vous pouvez également écrire la classe de formateur comme suit :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Les cours et les propriétés de Navigation OfficeAssignment

Les propriétés `Courses` et `OfficeAssignment` sont des propriétés de navigation. Comme expliqué précédemment, ils sont généralement définis en tant que [virtuels](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) afin qu’ils peuvent tirer parti d’une fonction d’Entity Framework appelée [chargement différé](https://msdn.microsoft.com/magazine/hh205756.aspx). En outre, si une propriété de navigation peut contenir plusieurs entités, son type doit implémenter la [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx) Interface. Par exemple [IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx) qualifie mais pas [IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx) car `IEnumerable<T>` n’implémente pas [ajouter ](https://msdn.microsoft.com/library/63ywd54z.aspx).

Formateur pouvez animer n’importe quel nombre de cours, de sorte que `Courses` est défini comme une collection de `Course` entités.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

Règles d’entreprise un formateur ne peut avoir au plus un bureau, par conséquent, l’état `OfficeAssignment` est défini comme un seul `OfficeAssignment` entité (qui peut être `null` si aucun office n’est affectée).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>Créer l’entité OfficeAssignment

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Créer *Models\OfficeAssignment.cs* avec le code suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Générez le projet, qui enregistre les modifications et vérifie que vous n’avez pas effectué une copie et collez le compilateur peut intercepter des erreurs.

### <a name="the-key-attribute"></a>L’attribut de clé

Il existe une relation un-à-zéro-ou-un entre le `Instructor` et `OfficeAssignment` entités. Une attribution existe uniquement en relation avec le formateur, il est assigné à, et par conséquent sa clé primaire est également sa clé étrangère dans la `Instructor` entité. Mais Entity Framework ne peut pas reconnaître automatiquement `InstructorID` en tant que principal clés de cette entité, car son nom ne respecte pas la `ID` ou *classname* `ID` convention d’affectation de noms. Par conséquent, l’attribut `Key` est utilisé pour l’identifier comme clé :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Vous pouvez également utiliser le `Key` attribut si l’entité n’a pas sa propre clé primaire, mais que vous voulez nommer la propriété autre chose que `classnameID` ou `ID`. Par défaut EF traite la touche non généré à la base de données, car la colonne est pour une relation d’identification.

### <a name="the-foreignkey-attribute"></a>L’attribut ForeignKey

Lorsqu’il existe une relation un-à-zéro-ou-un ou une relation entre deux entités (par exemple, entre `OfficeAssignment` et `Instructor`), EF ne peut pas fonctionner à l’extrémité de la relation est le principal et l’extrémité est dépendante. Relations ont une propriété de navigation de référence dans chaque classe vers l’autre classe. Le [ForeignKey attribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) peut être appliqué à la classe dépendante pour établir la relation. Si vous omettez le [ForeignKey attribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), vous obtenez l’erreur suivante lorsque vous essayez de créer la migration :

*Impossible de déterminer l’extrémité principale d’une association entre les types 'ContosoUniversity.Models.OfficeAssignment' et 'ContosoUniversity.Models.Instructor'. La terminaison principale de cette association doit être configurée de manière explicite à l’aide de la relation des API fluent ou annotations de données.*

Plus loin dans ce didacticiel, vous allez apprendre à configurer cette relation avec l’API fluent.

### <a name="the-instructor-navigation-property"></a>La propriété de Navigation de formateur

Le `Instructor` entité a un nullable `OfficeAssignment` propriété de navigation (parce qu’un formateur n’est peut-être pas une assignation office) et le `OfficeAssignment` entité a non-nullable `Instructor` propriété de navigation (car une attribution ne peut pas exister sans un formateur-- `InstructorID` non-null). Lorsqu’un `Instructor` entité associée à une `OfficeAssignment` entité, chaque entité a une référence à l’autre dans sa propriété de navigation.

Vous pouvez placer un `[Required]` attribut sur la propriété de navigation du formateur pour spécifier qu’il doit y avoir un formateur connexe, mais vous n’êtes pas obligé de le faire, car la clé étrangère InstructorID (qui est également la clé pour cette table) est non nullable.

## <a name="modify-the-course-entity"></a>Modifier l’entité Course

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Dans *Models\Course.cs*, remplacez le code que vous avez ajouté précédemment par le code suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

L’entité de cours possède une propriété de clé étrangère `DepartmentID` qui pointe vers le `Department` entité et contient un `Department` propriété de navigation. Entity Framework ne vous demande pas d’ajouter une propriété de clé étrangère à votre modèle de données lorsque vous avez une propriété de navigation pour une entité associée. EF crée automatiquement des clés étrangères dans la base de données là où elles sont requises. Mais le fait d’avoir la clé étrangère dans le modèle de données peut rendre les mises à jour plus simples et plus efficaces. Par exemple, lorsque vous lisez une entité de cours à modifier, le `Department` entité a la valeur null si vous ne le charge pas, par conséquent, lorsque vous mettez à jour l’entité de cours, vous devez tout d’abord récupérer le `Department` entité. Lorsque la propriété de clé étrangère `DepartmentID` est inclus dans le modèle de données, vous n’avez pas besoin extraire le `Department` entité avant de mettre à jour.

### <a name="the-databasegenerated-attribute"></a>L’attribut DatabaseGenerated

Le [DatabaseGenerated attribut](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) avec la [aucun](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) paramètre sur le `CourseID` propriété spécifie que les valeurs de clé primaire sont fournies par l’utilisateur au lieu de générés par la base de données.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Par défaut, Entity Framework suppose que les valeurs de clés primaires sont générées par la base de données. C’est ce que vous souhaitez dans la plupart des scénarios. Toutefois, pour `Course` des entités, vous devez utiliser un numéro spécifié par l’utilisateur comme une série de 1000 pour un service, une série de 2000 pour un autre service et ainsi de suite.

### <a name="foreign-key-and-navigation-properties"></a>Propriétés de Navigation et de clé étrangère

Les propriétés de clé étrangère et les propriétés de navigation dans les `Course` entité reflète les relations suivantes :

- Un cours est affecté à un seul département, donc il existe une clé étrangère `DepartmentID` et une propriété de navigation `Department` pour les raisons mentionnées ci-dessus. 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Un cours peut avoir un nombre quelconque d’étudiants inscrits, si bien que la propriété de navigation `Enrollments` est une collection : 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Un cours pouvant être animé par plusieurs formateurs, la propriété de navigation `Instructors` est une collection : 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>Créer l’entité du service

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

Créer *Models\Department.cs* avec le code suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>L’attribut de colonne

Précédemment, vous avez utilisé le [attribut de colonne](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) pour modifier le mappage de nom de colonne. Dans le code de la `Department` entité, le `Column` attribut sert à modifier SQL type correspondance de données afin que la colonne sera définie à l’aide de SQL Server [money](https://msdn.microsoft.com/library/ms179882.aspx) type dans la base de données :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Mappage de colonnes n’est généralement pas nécessaire, car l’Entity Framework choisit généralement le type de données SQL Server approprié en fonction du type CLR que vous définissez pour la propriété. Le type CLR `decimal` est mappé à un type SQL Server `decimal`. Mais dans ce cas, vous savez que la colonne organiserons des montants en devise et le [money](https://msdn.microsoft.com/library/ms179882.aspx) type de données est plus adapté. Pour plus d’informations sur les types de données CLR et comment ils correspondent aux types de données SQL Server, consultez [SqlClient pour Entity FrameworkTypes](https://msdn.microsoft.com/library/bb896344.aspx).

### <a name="foreign-key-and-navigation-properties"></a>Propriétés de Navigation et de clé étrangère

Les propriétés de clé étrangère et de navigation reflètent les relations suivantes :

- Un département peut ou non avoir un administrateur, et un administrateur est toujours un formateur. Par conséquent la `InstructorID` propriété n’est incluse en tant que clé étrangère à la `Instructor` entité et un point d’interrogation est ajouté après le `int` désignation pour marquer la propriété Nullable de type. La propriété de navigation est nommée `Administrator` mais contient un `Instructor` entité : 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Un service peut avoir plusieurs cours, donc il est un `Courses` propriété de navigation : 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Par convention, Entity Framework permet la suppression en cascade pour les clés étrangères non nullables et pour les relations plusieurs à plusieurs. Cela peut entraîner des règles de suppression en cascade circulaires, qui provoqueront une exception lorsque vous essaierez d’ajouter une migration. Par exemple, si vous n’avez pas défini le `Department.InstructorID` propriété Nullable, vous obtenez le message d’exception suivant : « la relation référentielle entraîne une référence cyclique n’est pas autorisée. » Si vos règles d’entreprise nécessaire `InstructorID` propriété soit non nullable, vous devez utiliser l’instruction API fluent suivante pour désactiver la suppression en cascade sur la relation : 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modify-the-enrollment-entity"></a>Modifier l’entité de l’inscription

![Enrollment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

 Dans *Models\Enrollment.cs*, remplacez le code que vous avez ajouté précédemment par le code suivant

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>Propriétés de Navigation et de clé étrangère

Les propriétés de clé étrangère et de navigation reflètent les relations suivantes :

- Un enregistrement d’inscription est utilisé pour un cours unique, si bien qu’il existe une propriété de clé étrangère `CourseID` et une propriété de navigation `Course` : 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- Un enregistrement d’inscription est utilisé pour un étudiant unique, si bien qu’il existe une propriété de clé étrangère `StudentID` et une propriété de navigation `Student` : 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>Relations plusieurs-à-plusieurs

Il existe une relation plusieurs-à-plusieurs entre la `Student` et `Course` entités et le `Enrollment` entité fonctionne comme une table de jointure plusieurs-à-plusieurs *avec une charge utile* dans la base de données. Cela signifie que la `Enrollment` table contient des données supplémentaires en plus des clés étrangères des tables jointes (dans ce cas, une clé primaire et un `Grade` propriété).

L’illustration suivante montre à quoi ressemblent ces relations dans un diagramme d’entité. (Ce diagramme a été généré à l’aide de la [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); création du diagramme ne fait pas partie de ce didacticiel, il est uniquement utilisé ici à titre d’illustration.)

![Student-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Chaque ligne de relation a une 1 à une extrémité et un astérisque (\*) à l’autre, qui indique une relation un-à-plusieurs.

Si le `Enrollment` table n’a pas inclure les informations de catégorie, il doit uniquement contenir les deux clés étrangères `CourseID` et `StudentID`. Dans ce cas, il correspond à une table de jointure plusieurs-à-plusieurs *sans la charge utile* (ou un *table de jonction pure*) dans la base de données, et vous ne devez créer une classe de modèle pour qu’il tout. Le `Instructor` et `Course` entités avoir ce type de relation plusieurs-à-plusieurs, et comme vous pouvez le voir, il n’existe aucune classe d’entité entre eux :

![Instructor-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Une table de jointure est requis dans la base de données, cependant, comme indiqué dans le schéma de base de données suivantes :

![Instructor-Course_many-to-many_relationship_tables](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Entity Framework crée automatiquement le `CourseInstructor` table et que vous lisez et indirectement mettre à jour par la lecture et mise à jour la `Instructor.Courses` et `Course.Instructors` propriétés de navigation.

## <a name="entity-diagram-showing-relationships"></a>Diagramme des entités montrant les relations

L’illustration suivante montre le diagramme que les outils Entity Framework Power Tools créent pour le modèle School complet.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

Outre les lignes de relation plusieurs-à-plusieurs (\* à \*) et les lignes de relation un-à-plusieurs (1 à \*), vous pouvez voir ici la ligne de relation d’un-à-zéro-ou-un (1 à 0.. 1) entre la `Instructor` et `OfficeAssignment` entités et la ligne de zéro-ou-relation un-à-plusieurs (0.. 1 à \*) entre les entités du formateur et de service.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Personnaliser le modèle de données en ajoutant du Code dans le contexte de base de données

Vous allez ensuite ajouter les nouvelles entités à la `SchoolContext` de classe et de personnaliser le mappage à l’aide des [API fluent](https://msdn.microsoft.com/data/jj591617) appels. L’API est « fluent », car il est souvent utilisée par tirage une série d’appels de méthode dans une instruction unique, comme dans l’exemple suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

Dans ce didacticiel, vous utiliserez l’API fluent uniquement pour le mappage de base de données que vous ne pouvez pas faire avec des attributs. Toutefois, vous pouvez également utiliser l’API Fluent pour spécifier la majorité des règles de mise en forme, de validation et de mappage que vous pouvez spécifier à l’aide d’attributs. Certains attributs, tels que `MinimumLength`, ne peuvent pas être appliqués avec l’API Fluent. Comme mentionné précédemment, `MinimumLength` ne change pas le schéma, elle s’applique uniquement une règle de validation côté client et serveur

Certains développeurs préfèrent utiliser exclusivement l’API Fluent afin de conserver des classes d’entité « propres ». Vous pouvez combiner les attributs et l’API Fluent si vous le voulez, et il existe quelques personnalisations qui peuvent être effectuées uniquement à l’aide de l’API Fluent, mais en général la pratique recommandée consiste à choisir l’une de ces deux approches et à l’utiliser constamment, autant que possible.

Pour ajouter de nouvelles entités pour les données de modèle et effectuez un mappage de base de données que vous n’avez pas à l’aide des attributs, remplacez le code dans *DAL\SchoolContext.cs* avec le code suivant :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

La nouvelle instruction de la [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) méthode configure la table de jointure plusieurs-à-plusieurs :

- Pour la relation plusieurs-à-plusieurs entre la `Instructor` et `Course` entités, le code spécifie les noms de table et de colonne pour la table de jointure. Code d’abord peut configurer la relation plusieurs-à-plusieurs pour vous sans ce code, mais si vous ne l’appelez, vous obtiendrez comme noms par défaut `InstructorInstructorID` pour la `InstructorID` colonne.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Le code suivant illustre la façon dont vous pouvez avoir utilisé API fluent au lieu d’attributs pour spécifier la relation entre la `Instructor` et `OfficeAssignment` entités :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Pour plus d’informations sur ce que font les instructions « API fluent » en arrière-plan, consultez le [API Fluent](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) billet de blog.

## <a name="seed-the-database-with-test-data"></a>Peupler la base de données avec des données de test

Remplacez le code dans le *Migrations\Configuration.cs* fichier avec le code suivant afin de fournir des données de la valeur de départ pour les nouvelles entités que vous avez créé.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Comme vous l’avez vu dans le premier didacticiel, la majeure partie de ce code simplement des mises à jour ou crée des objets d’entité et charge les exemples de données dans les propriétés requises pour le test. Toutefois, notez comment le `Course` entité, qui a une relation plusieurs-à-plusieurs avec la `Instructor` entité, est gérée :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Lorsque vous créez un `Course` de l’objet, vous initialisez le `Instructors` propriété de navigation comme une collection vide en utilisant le code `Instructors = new List<Instructor>()`. Il est ainsi possible d’ajouter `Instructor` entités qui sont associées à ce `Course` à l’aide de la `Instructors.Add` (méthode). Si vous n’avez pas créé une liste vide, vous ne pourrez ajouter ces relations, car la `Instructors` propriété serait null et n’avez pas un `Add` (méthode). Vous pouvez également ajouter l’initialisation de la liste au constructeur.

## <a name="add-a-migration-and-update-the-database"></a>Ajouter une Migration et mise à jour de la base de données

À partir de la PMC, entrez le `add-migration` commande (ne le faites pas la `update-database` commande encore) :

`add-Migration ComplexDataModel`

Si vous tentiez d’exécuter la commande `update-database` à ce stade (ne le faites pas encore), vous obtiendriez l’erreur suivante :

*L’instruction ALTER TABLE est en conflit avec la contrainte de clé étrangère « FK\_dbo. Cours\_dbo. Service\_DepartmentID ». Le conflit s’est produite dans la base de données « ContosoUniversity », table « dbo. Département », colonne « DepartmentID ».*

Parfois, lorsque vous exécutez des migrations avec des données existantes, vous devez insérer des données de stub dans la base de données pour répondre aux contraintes de clé étrangère, et c’est ce que vous devez faire maintenant. Le code généré dans le ComplexDataModel `Up` méthode ajoute non nullable `DepartmentID` clé étrangère vers le `Course` table. Car il existe déjà des lignes dans le `Course` table lorsque le code s’exécute, le `AddColumn` opération échoue parce que SQL Server ne sait pas quelle valeur à placer dans la colonne ne peut pas être null. Par conséquent avoir à modifier le code pour permettre à la nouvelle colonne à une valeur par défaut et créer un service stub nommé « Temp » en tant que le service par défaut. En conséquence, existant `Course` lignes sont liées au service « Temp » après le `Up` s’exécute la méthode. Vous pouvez les associer aux départements corrects dans le `Seed` (méthode).

Modifier la &lt; *timestamp&gt;\_ComplexDataModel.cs* , commentez la ligne de code qui ajoute la colonne DepartmentID de la table Course et ajoutez le code en surbrillance suivant (les commentaires ligne est également mis en surbrillance) :

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

Lorsque le `Seed` exécution de la méthode, il insère des lignes dans le `Department` table et elle seront liée existant `Course` lignes pour ces nouvelles `Department` lignes. Si vous n’avez pas ajouté à n’importe quel cours dans l’interface utilisateur, vous ne sont plus devez alors le service « Temp » ou la valeur par défaut sur le `Course.DepartmentID` colonne. Pour permettre la possibilité qu’une personne avez peut-être ajouté cours à l’aide de l’application, vous pouvez également mettre à jour le `Seed` code de la méthode pour vous assurer que tous les `Course` lignes (pas seulement ceux qui sont insérées par les exécutions antérieures de la `Seed` méthode) ont valide `DepartmentID` valeurs avant de supprimer la valeur par défaut à partir de la colonne et supprimer le service « Temp ».

Une fois que vous avez terminé de modifier le &lt; *timestamp&gt;\_ComplexDataModel.cs* de fichier, entrez le `update-database` dans le CFP à l’exécution de la migration.

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> Il est possible d’obtenir d’autres erreurs lors de la migration des données et apporter des modifications de schéma. Si vous obtenez des erreurs de migration que vous ne pouvez pas résoudre, vous pouvez changer le nom de la base de données dans la chaîne de connexion ou supprimer la base de données. L’approche la plus simple consiste à renommer la base de données *Web.config* fichier. L’exemple suivant affiche le nom modifié en CU\_Test :
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
> 
> Avec une base de données, il n’existe pas de données à migrer et le `update-database` commande est beaucoup plus susceptible de se terminer sans erreur. Pour obtenir des instructions sur la suppression de la base de données, consultez [le déplacement d’une base de données à partir de Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).
> 
> En cas d’échec, un autre chose que vous pouvez essayer est réinitialiser la base de données en entrant la commande suivante dans le PMC :
> 
> `update-database -TargetMigration:0`


Ouvrez la base de données **l’Explorateur de serveurs** comme vous le faisait auparavant, puis développez le **Tables** nœud pour voir que toutes les tables ont été créés. (Si vous avez toujours **l’Explorateur de serveurs** ouvrir à partir de l’état antérieur, cliquez sur le **Actualiser** bouton.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

Vous n’avez pas créé une classe de modèle pour le `CourseInstructor` table. Comme expliqué précédemment, il s’agit d’une table de jointure pour la relation plusieurs-à-plusieurs entre la `Instructor` et `Course` entités.

Avec le bouton droit le `CourseInstructor` de table et sélectionnez **afficher les données de Table** pour vérifier qu’il comporte des données à la suite de la `Instructor` entités que vous avez ajouté à la `Course.Instructors` propriété de navigation.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="summary"></a>Récapitulatif

Vous avez maintenant un modèle de données plus complexe et une base de données correspondante. Dans ce didacticiel vous en apprendrez davantage sur les différentes façons d’accéder aux données associées.

Veuillez laisser des commentaires sur la façon dont vous avez aimé ce didacticiel et nous pouvons améliorer. Vous pouvez également demander de nouvelles rubriques à [afficher Me comment avec le Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Vous trouverez des liens vers d’autres ressources Entity Framework dans le [ASP.NET Data Access - ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Précédent](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Suivant](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
