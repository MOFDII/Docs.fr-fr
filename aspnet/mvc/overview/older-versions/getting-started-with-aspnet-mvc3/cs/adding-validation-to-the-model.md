---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: Ajout d’une Validation pour le modèle (c#) | Documents Microsoft
author: Rick-Anderson
description: Création d’un contrôleur
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: cdfe76f440e34ca7356af193186f90d2231b9db6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="adding-validation-to-the-model-c"></a>Ajout d’une Validation pour le modèle (c#)
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../../getting-started/introduction/getting-started.md) qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et illustre plusieurs fonctionnalités.
> 
> 
> Ce didacticiel, vous allez apprendre les principes fondamentaux de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis répertoriés ci-dessous. Vous pouvez installer tous les en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les conditions préalables à l’aide des liens suivants :
> 
> - [Conditions préalables requises de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mettre à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + outils prennent en charge)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les composants requis en cliquant sur le lien suivant : [composants requis de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet de Visual Web Developer avec code source c# est disponible pour accompagner cette rubrique. [Télécharger la version c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez Visual Basic, basculez vers le [version Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de ce didacticiel.


Dans cette section, vous allez ajouter la logique de validation pour la `Movie` modèle et vous devez vous assurer que les règles de validation sont appliquées à tout moment, un utilisateur tente de créer ou modifier un film à l’aide de l’application.

## <a name="keeping-things-dry"></a>Garder des données sec

Les principes de conception d’ASP.NET MVC est sec (« ne pas se répéter »). ASP.NET MVC vous encourage pour spécifier les fonctionnalités ou les comportements qu’une seule fois, puis l’avez partout dans une application. Cela réduit la quantité de code, que vous devez écrire et rend le code que vous écrivez beaucoup plus facile à gérer.

La prise en charge de validation fournie par ASP.NET MVC et Entity Framework Code First est un bon exemple du principe sec en action. Vous pouvez spécifier de façon déclarative des règles de validation dans un seul emplacement (dans la classe de modèle) et ensuite ces règles sont appliquées partout dans l’application.

Examinons à présent comment bénéficier de cette prise en charge de la validation de l’application de film.

## <a name="adding-validation-rules-to-the-movie-model"></a>Ajouter des règles de Validation pour le modèle de film

Vous allez commencer en ajoutant une logique de validation à la `Movie` classe.

Ouvrez le fichier *Movie.cs*. Ajouter un `using` instruction au début du fichier qui fait référence à la [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espace de noms :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

L’espace de noms fait partie du .NET Framework. Il fournit un ensemble intégré d’attributs de validation que vous pouvez appliquer de façon déclarative à une classe ou une propriété.

Mettre à jour le `Movie` classe pour tirer parti de la fonction intégrée [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), et [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) attributs de validation . Utilisez le code suivant comme exemple dans lequel appliquer les attributs.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

Les attributs de validation spécifient le comportement que vous souhaitez appliquer sur les propriétés du modèle sur lesquels ils sont appliqués. Le `Required` attribut indique qu’une propriété doit avoir une valeur ; dans cet exemple, un film doit avoir des valeurs pour le `Title`, `ReleaseDate`, `Genre`, et `Price` propriétés afin de pouvoir être valide. L’attribut `Range` contraint une valeur à une plage spécifiée. L’attribut `StringLength` vous permet de définir la longueur maximale d’une propriété de chaîne, et éventuellement sa longueur minimale.

Code vérifie tout d’abord que vous spécifiez sur une classe de modèle de règles de validation sont appliquées avant que l’application enregistre les modifications dans la base de données. Par exemple, le code suivant lève une exception lorsque la `SaveChanges` méthode est appelée, car plusieurs requis `Movie` valeurs de propriété sont manquants et le prix est égal à zéro (ce qui est en dehors de la plage valide).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

Avoir des règles de validation appliquées automatiquement par le .NET Framework permet de rendre votre application plus fiable. Cela garantit également que vous n’oublierez pas de valider un élément et que vous n’autoriserez pas par inadvertance l’insertion de données incorrectes dans la base de données.

Voici un code complet pour la mise à jour *Movie.cs* fichier :

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Erreur de validation de l’interface utilisateur dans ASP.NET MVC

Exécuter à nouveau l’application et accédez à la */Movies* URL.

Cliquez sur le **créer un film** pour ajouter un nouveau film. Remplissez le formulaire avec des valeurs non valides, puis cliquez sur le **créer** bouton.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Notez comment le formulaire a automatiquement utilisé une couleur d’arrière-plan pour mettre en évidence les zones de texte qui contiennent des données non valides et a émis un message d’erreur de validation appropriées en regard de chacun d’eux. Les messages d’erreur correspondent les chaînes d’erreur que vous avez spécifié lorsque vous annoté la `Movie` classe. Les erreurs sont appliquées à la fois (à l’aide de JavaScript) côté client et côté serveur (au cas où un utilisateur a désactivé JavaScript).

Un avantage est que vous n’avez pas devez modifier une seule ligne de code dans le `MoviesController` classe ou dans le *Create.cshtml* vue afin d’activer cette validation de l’interface utilisateur. Le contrôleur et les vues créées précédemment dans ce didacticiel sélectionnées jusqu'à la validation des règles que vous avez spécifié l’utilisation d’attributs sur la `Movie` classe de modèle.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>La Validation se produit de la créer afficher et créer la méthode d’Action

Vous vous demandez peut-être comment l’interface utilisateur de validation a été générée sans aucune mise à jour du code dans le contrôleur ou dans les vues. La liste suivante indique ce que le `Create` méthodes dans le `MovieController` ressemble de classe. Ils sont identiques à la façon dont vous les avez créés précédemment dans ce didacticiel.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

La première méthode d’action affiche le formulaire de création initial. La seconde gère la publication de formulaire. La seconde `Create` les appels de méthode `ModelState.IsValid` pour vérifier si le film a des erreurs de validation. L’appel de cette méthode évalue tous les attributs de validation qui ont été appliqués à l’objet. Si l’objet comporte des erreurs de validation, la `Create` méthode réaffiche le formulaire. S’il n’y a pas d’erreur, la méthode enregistre le nouveau film dans la base de données.

Voici le *Create.cshtml* afficher le modèle que vous structuré précédemment dans le didacticiel. Il est utilisé par les méthodes d’action ci-dessus à la fois pour afficher le formulaire initial et pour le réafficher en cas d’erreur.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Notez comment le code utilise un `Html.EditorFor` application d’assistance pour générer le `<input>` élément pour chaque `Movie` propriété. En regard de ce programme d’assistance est un appel à la `Html.ValidationMessageFor` méthode d’assistance. Ces deux méthodes d’assistance de travail avec l’objet de modèle qui est passé par le contrôleur à la vue (dans ce cas, un `Movie` objet). Ils recherchent automatiquement des attributs de validation spécifiés sur les messages d’erreur de modèle et l’affichage comme il convient.

Très intéressant, cette approche est que le contrôleur, ni le modèle créer une vue sait rien sur les règles de validation réelle appliquées ou sur les messages d’erreur affichés. Les règles de validation et les chaînes d’erreur sont spécifiées uniquement dans la classe `Movie`.

Si vous souhaitez modifier la logique de validation par la suite, vous pouvez le faire dans exactement un seul emplacement. Vous n’aurez pas à vous soucier des différentes parties de l’application qui pourraient être incohérentes avec la façon dont les règles sont appliquées. Toute la logique de validation sera définie à un seul emplacement et utilisée partout. Le code est ainsi très propre, facile à mettre à jour et évolutif. Et cela signifie que vous respecterez entièrement le principe DRY.

## <a name="adding-formatting-to-the-movie-model"></a>Ajout de mise en forme pour le modèle de film

Ouvrez le fichier *Movie.cs*. Le [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espace de noms fournit des attributs de mise en forme en plus de l’ensemble intégré d’attributs de validation. Vous devez appliquer le [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribut et un [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valeur d’énumération pour la date de publication et les champs de prix. Le code suivant illustre la `ReleaseDate` et `Price` propriétés appropriés [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribut.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

Ou bien, vous pouvez explicitement définir un [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) valeur. Le code suivant montre la propriété de date de version avec une chaîne de format de date (à savoir, « d »). Vous utilisez cela pour spécifier que vous ne souhaitez temps dans le cadre de la date de publication.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

Le code suivant formats le `Price` propriété sous forme de devise.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

Le texte complet `Movie` classe est illustrée ci-dessous.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Exécutez l’application et accédez à la `Movies` contrôleur.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

Dans la partie suivante de la série, nous allons examiner l’application et apporter des améliorations aux méthodes `Details` et `Delete` générées automatiquement.

> [!div class="step-by-step"]
> [Précédent](adding-a-new-field.md)
> [Suivant](improving-the-details-and-delete-methods.md)
