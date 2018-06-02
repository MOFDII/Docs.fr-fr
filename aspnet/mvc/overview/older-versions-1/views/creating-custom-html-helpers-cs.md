---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Création de programmes d’assistance du code HTML personnalisé (c#) | Documents Microsoft
author: microsoft
description: L’objectif de ce didacticiel est de montrer comment vous pouvez créer des applications auxiliaires HTML personnalisé que vous pouvez utiliser dans vos vues MVC. En tirant parti du programme d’assistance HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: ebc9aa2aa8dbc02dc01833d671c3bfd19141ba74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="creating-custom-html-helpers-c"></a>Création des programmes d’assistance HTML personnalisés (c#)
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger le PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> L’objectif de ce didacticiel est de montrer comment vous pouvez créer des applications auxiliaires HTML personnalisé que vous pouvez utiliser dans vos vues MVC. En tirant parti des programmes d’assistance HTML, vous pouvez réduire la quantité de saisie fastidieux de balises HTML que vous devez effectuer pour créer une page HTML standard.


L’objectif de ce didacticiel est de montrer comment vous pouvez créer des applications auxiliaires HTML personnalisé que vous pouvez utiliser dans vos vues MVC. En tirant parti des programmes d’assistance HTML, vous pouvez réduire la quantité de saisie fastidieux de balises HTML que vous devez effectuer pour créer une page HTML standard.

Dans la première partie de ce didacticiel, je décris parmi les programmes d’assistance HTML existant, inclus avec l’infrastructure ASP.NET MVC. Ensuite, je décris deux méthodes de création des programmes d’assistance HTML personnalisé : vous expliquent comment créer des programmes d’assistance HTML personnalisé en créant une méthode statique et en créant une méthode d’extension.

## <a name="understanding-html-helpers"></a>Présentation des programmes d’assistance HTML

Une application d’assistance HTML est simplement une méthode qui retourne une chaîne. La chaîne peut représenter n’importe quel type de contenu que vous souhaitez. Par exemple, vous pouvez utiliser des programmes d’assistance HTML pour restituer des balises HTML standard tels que HTML `<input>` et `<img>` balises. Vous pouvez également utiliser des programmes d’assistance HTML pour restituer le contenu plus complexe telles que la bande d’onglets ou d’une table HTML de base de données.

L’infrastructure ASP.NET MVC inclut l’ensemble des programmes d’assistance de HTML standard (il ne s’agit pas d’une liste complète) suivantes :

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

Par exemple, considérez le formulaire dans la liste 1. Ce formulaire est affiché à l’aide de deux des programmes d’assistance HTML standard (voir Figure 1). Ce formulaire utilise la `Html.BeginForm()` et `Html.TextBox()` les méthodes d’assistance pour restituer un formulaire HTML simple.


[![Page rendue avec des programmes d’assistance HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Figure 01**: Page rendue avec des programmes d’assistance HTML ([cliquez pour afficher l’image en taille réelle](creating-custom-html-helpers-cs/_static/image3.png))


**La liste 1 : `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

La méthode d’assistance de Html.BeginForm() est utilisée pour créer l’ouverture et de fermeture HTML `<form>` balises. Notez que le `Html.BeginForm()` méthode est appelée au sein d’un à l’aide de déclaration. L’à l’aide d’instruction garantit que le `<form>` balise est fermé à la fin de l’utilisation bloc.

Si vous préférez, au lieu de créer un à l’aide de bloc, vous pouvez appeler la méthode d’assistance de Html.EndForm() pour fermer la `<form>` balise. Utiliser l’approche de création d’ouverture et fermeture `<form>` balise semble plus intuitif.

Le `Html.TextBox()` méthodes d’assistance sont utilisés dans la liste 1 pour le rendu HTML `<input>` balises. Si vous sélectionnez Afficher la source dans votre navigateur affiche la source HTML dans le Listing 2. Notez que la source contient des balises HTML standard.

> [!IMPORTANT]
> Notez que la `Html.TextBox()`-HTML rendue avec application d’assistance `<%= %>` balises au lieu de `<% %>` balises. Si vous n’incluez pas le signe égal, puis rien n’est restitué dans le navigateur.

L’infrastructure ASP.NET MVC contient un petit ensemble de programmes d’assistance. Très probablement, vous devez étendre l’infrastructure MVC avec des programmes d’assistance HTML personnalisé. Dans le reste de ce didacticiel, vous découvrez des deux méthodes de création des programmes d’assistance HTML personnalisé.

**Liste 2 : `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Création de programmes d’assistance HTML avec des méthodes statiques

Le moyen le plus simple pour créer un programme d’assistance HTML nouvelle est de créer une méthode statique qui retourne une chaîne. Par exemple, imaginez que vous décidez de créer un nouveau programme d’assistance HTML qui effectue le rendu HTML `<label>` balise. Vous pouvez utiliser la classe dans la liste 2 pour restituer un `<label>` .

**Liste 2 : `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

Il n’a rien de spécial sur la classe dans la liste 2. Le `Label()` méthode retourne simplement une chaîne.

La vue Index modifiée dans la liste 3 utilise le `LabelHelper` pour le rendu HTML `<label>` balises. Notez que la vue inclut une `<%@ imports %>` directive qui importe le `Application1.Helpers` espace de noms.

**Liste 2 : `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Création de programmes d’assistance HTML avec les méthodes d’Extension

Si vous souhaitez créer des programmes d’assistance HTML qui fonctionnent comme les programmes d’assistance HTML standard inclus dans l’infrastructure ASP.NET MVC, vous devez créer des méthodes d’extension. Méthodes d’extension permettent d’ajouter de nouvelles méthodes à une classe existante. Lorsque vous créez une méthode de programme d’assistance HTML, vous ajoutez de nouvelles méthodes à la classe HtmlHelper représentée par la propriété de Html d’un affichage.

La classe de liste 3 ajoute une méthode d’extension pour le `HtmlHelper` classe nommée `Label()`. Il existe plusieurs choses que vous devez remarquer sur cette classe. Tout d’abord, notez que la classe est une classe statique. Vous devez définir une méthode d’extension avec une classe statique.

En second lieu, notez que le premier paramètre de la `Label()` méthode est précédée par le mot clé `this`. Le premier paramètre d’une méthode d’extension indique la classe qui étend de la méthode d’extension.

**La liste 3 : `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Après avoir créé une méthode d’extension et que vous générez votre application avec succès, la méthode d’extension s’affiche dans Visual Studio Intellisense, telles que toutes les autres méthodes d’une classe (voir Figure 2). La seule différence est qu’extension méthodes apparaissent avec un symbole spécial en regard des (il s’agit d’une icône de flèche vers le bas).


[![À l’aide de la méthode d’extension Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Figure 02**: à l’aide de la méthode d’extension Html.Label() ([cliquez pour afficher l’image en taille réelle](creating-custom-html-helpers-cs/_static/image6.png))


La vue Index modifiée sur la liste 4 utilise la méthode d’extension Html.Label() à restituer tous ses `<label>` balises.

**La liste 4 – `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez appris à deux méthodes de création des programmes d’assistance HTML personnalisé. Tout d’abord, vous avez appris à créer un personnalisé `Label()` programme d’assistance HTML en créant une méthode statique qui retourne une chaîne. Ensuite, vous avez appris à créer un personnalisé `Label()` méthode du programme d’assistance HTML en créant une méthode d’extension sur la `HtmlHelper` classe.

Dans ce didacticiel, I se concentre sur la création d’une méthode de programme d’assistance HTML extrêmement simple. Notez qu’une application d’assistance HTML peuvent être aussi complexe que vous le souhaitez. Vous pouvez générer des programmes d’assistance HTML qui restituent contenu riche, tels que les vues de l’arborescence, les menus ou les tables de base de données.

> [!div class="step-by-step"]
> [Précédent](asp-net-mvc-views-overview-cs.md)
> [Suivant](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
