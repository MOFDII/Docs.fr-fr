---
title: "Configurer la localisation d’un objet portable"
author: sebastienros
description: "Cet article présente les fichiers objets Portable et décrit les étapes permettant de les utiliser dans une application ASP.NET Core avec l’infrastructure de base de Orchard."
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/portable-object-localization
ms.openlocfilehash: dfdd86b4706a1fb8e313c24ba830ec996fe09225
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="configure-portable-object-localization-with-orchard-core"></a><span data-ttu-id="018c8-103">Configurer la localisation d’un objet portable avec Orchard de base</span><span class="sxs-lookup"><span data-stu-id="018c8-103">Configure portable object localization with Orchard Core</span></span>

<span data-ttu-id="018c8-104">Par [Sébastien Ros](https://github.com/sebastienros) et [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="018c8-104">By [Sébastien Ros](https://github.com/sebastienros) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="018c8-105">Cet article décrit les étapes pour l’utilisation des fichiers d’objet Portable (PO) dans une application ASP.NET Core avec la [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span><span class="sxs-lookup"><span data-stu-id="018c8-105">This article walks through the steps for using Portable Object (PO) files in an ASP.NET Core application with the [Orchard Core](https://github.com/OrchardCMS/OrchardCore) framework.</span></span>

<span data-ttu-id="018c8-106">**Remarque :** Orchard Core n’est pas un produit Microsoft.</span><span class="sxs-lookup"><span data-stu-id="018c8-106">**Note:** Orchard Core is not a Microsoft product.</span></span> <span data-ttu-id="018c8-107">Par conséquent, Microsoft ne fournit aucune prise en charge pour cette fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="018c8-107">Consequently, Microsoft provides no support for this feature.</span></span>

<span data-ttu-id="018c8-108">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="018c8-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/POLocalization) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-po-file"></a><span data-ttu-id="018c8-109">Qu’est un fichier de bon de commande ?</span><span class="sxs-lookup"><span data-stu-id="018c8-109">What is a PO file?</span></span>

<span data-ttu-id="018c8-110">Les fichiers de bon de commande sont distribués sous forme de fichiers texte contenant les chaînes traduites pour une langue donnée.</span><span class="sxs-lookup"><span data-stu-id="018c8-110">PO files are distributed as text files containing the translated strings for a given language.</span></span> <span data-ttu-id="018c8-111">Certains avantages de l’utilisation de fichiers de bon de commande à la place *.resx* fichiers incluent :</span><span class="sxs-lookup"><span data-stu-id="018c8-111">Some advantages of using PO files instead *.resx* files include:</span></span>
- <span data-ttu-id="018c8-112">Fichiers de bon de commande prennent en charge la pluralisation ; *.resx* fichiers ne prennent pas en charge pluralisation.</span><span class="sxs-lookup"><span data-stu-id="018c8-112">PO files support pluralization; *.resx* files don't support pluralization.</span></span>
- <span data-ttu-id="018c8-113">Fichiers de bon de commande ne sont pas compilés comme *.resx* fichiers.</span><span class="sxs-lookup"><span data-stu-id="018c8-113">PO files aren't compiled like *.resx* files.</span></span> <span data-ttu-id="018c8-114">Par conséquent, des étapes de build et outils que ceux spécialisées ne sont pas nécessaires.</span><span class="sxs-lookup"><span data-stu-id="018c8-114">As such, specialized tooling and build steps aren't required.</span></span>
- <span data-ttu-id="018c8-115">Fichiers de bon de commande fonctionnent correctement avec les outils d’édition en ligne de collaboration.</span><span class="sxs-lookup"><span data-stu-id="018c8-115">PO files work well with collaborative online editing tools.</span></span>

### <a name="example"></a><span data-ttu-id="018c8-116">Exemple</span><span class="sxs-lookup"><span data-stu-id="018c8-116">Example</span></span>

<span data-ttu-id="018c8-117">Voici un exemple de fichier de bon de commande contenant la traduction de deux chaînes en Français, y compris l’un avec sa forme au pluriel :</span><span class="sxs-lookup"><span data-stu-id="018c8-117">Here is a sample PO file containing the translation for two strings in French, including one with its plural form:</span></span>

<span data-ttu-id="018c8-118">*fr.po*</span><span class="sxs-lookup"><span data-stu-id="018c8-118">*fr.po*</span></span>

```text
#: Services/EmailService.cs:29
msgid "Enter a comma separated list of email addresses."
msgstr "Entrez une liste d'emails séparés par une virgule."

#: Views/Email.cshtml:112
msgid "The email address is \"{0}\"."
msgid_plural "The email addresses are \"{0}\"."
msgstr[0] "L'adresse email est \"{0}\"."
msgstr[1] "Les adresses email sont \"{0}\""
```

<span data-ttu-id="018c8-119">Cet exemple utilise la syntaxe suivante :</span><span class="sxs-lookup"><span data-stu-id="018c8-119">This example uses the following syntax:</span></span>

- <span data-ttu-id="018c8-120">`#:`: Un commentaire qui indique le contexte de la chaîne à convertir.</span><span class="sxs-lookup"><span data-stu-id="018c8-120">`#:`: A comment indicating the context of the string to be translated.</span></span> <span data-ttu-id="018c8-121">La même chaîne peut-être être traduite différemment selon l’endroit où il est utilisé.</span><span class="sxs-lookup"><span data-stu-id="018c8-121">The same string might be translated differently depending on where it is being used.</span></span>
- <span data-ttu-id="018c8-122">`msgid`: Chaîne non traduite.</span><span class="sxs-lookup"><span data-stu-id="018c8-122">`msgid`: The untranslated string.</span></span>
- <span data-ttu-id="018c8-123">`msgstr`: La chaîne traduite.</span><span class="sxs-lookup"><span data-stu-id="018c8-123">`msgstr`: The translated string.</span></span>

<span data-ttu-id="018c8-124">Dans le cas de prise en charge de la pluralisation, plusieurs entrées peuvent être définies.</span><span class="sxs-lookup"><span data-stu-id="018c8-124">In the case of pluralization support, more entries can be defined.</span></span>

- <span data-ttu-id="018c8-125">`msgid_plural`: La chaîne au pluriel non traduite.</span><span class="sxs-lookup"><span data-stu-id="018c8-125">`msgid_plural`: The untranslated plural string.</span></span>
- <span data-ttu-id="018c8-126">`msgstr[0]`: La chaîne traduite pour le cas de 0.</span><span class="sxs-lookup"><span data-stu-id="018c8-126">`msgstr[0]`: The translated string for the case 0.</span></span>
- <span data-ttu-id="018c8-127">`msgstr[N]`: La chaîne traduite pour le n de cas.</span><span class="sxs-lookup"><span data-stu-id="018c8-127">`msgstr[N]`: The translated string for the case N.</span></span>

<span data-ttu-id="018c8-128">La spécification de fichier de bon de commande se trouve [ici](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span><span class="sxs-lookup"><span data-stu-id="018c8-128">The PO file specification can be found [here](https://www.gnu.org/savannah-checkouts/gnu/gettext/manual/html_node/PO-Files.html).</span></span>

## <a name="configuring-po-file-support-in-aspnet-core"></a><span data-ttu-id="018c8-129">Configuration de prise en charge du fichier de bon de commande dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="018c8-129">Configuring PO file support in ASP.NET Core</span></span>

<span data-ttu-id="018c8-130">Cet exemple est basé sur une application ASP.NET MVC de base générée à partir d’un modèle de projet Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="018c8-130">This example is based on an ASP.NET Core MVC application generated from a Visual Studio 2017 project template.</span></span>

### <a name="referencing-the-package"></a><span data-ttu-id="018c8-131">Référencement du package</span><span class="sxs-lookup"><span data-stu-id="018c8-131">Referencing the package</span></span>

<span data-ttu-id="018c8-132">Ajoutez une référence à la `OrchardCore.Localization.Core` package NuGet.</span><span class="sxs-lookup"><span data-stu-id="018c8-132">Add a reference to the `OrchardCore.Localization.Core` NuGet package.</span></span> <span data-ttu-id="018c8-133">Il est disponible sur [MyGet](https://www.myget.org/) à la source du package suivant : https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span><span class="sxs-lookup"><span data-stu-id="018c8-133">It is available on [MyGet](https://www.myget.org/) at the following package source: https://www.myget.org/F/orchardcore-preview/api/v3/index.json</span></span>

<span data-ttu-id="018c8-134">Le *.csproj* fichier contient maintenant une ligne semblable à celle-ci (numéro de version peut varier) :</span><span class="sxs-lookup"><span data-stu-id="018c8-134">The *.csproj* file now contains a line similar to the following (version number may vary):</span></span>

[!code-xml[Main](localization/sample/POLocalization/POLocalization.csproj?range=9)]

### <a name="registering-the-service"></a><span data-ttu-id="018c8-135">Inscription du service</span><span class="sxs-lookup"><span data-stu-id="018c8-135">Registering the service</span></span>

<span data-ttu-id="018c8-136">Ajouter les services requis pour le `ConfigureServices` méthode *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="018c8-136">Add the required services to the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_ConfigureServices&highlight=4-21)]

<span data-ttu-id="018c8-137">Ajouter l’intergiciel (middleware) requis pour le `Configure` méthode *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="018c8-137">Add the required middleware to the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[Main](localization/sample/POLocalization/Startup.cs?name=snippet_Configure&highlight=15)]

<span data-ttu-id="018c8-138">Ajoutez le code suivant à votre affichage Razor de choix.</span><span class="sxs-lookup"><span data-stu-id="018c8-138">Add the following code to your Razor view of choice.</span></span> <span data-ttu-id="018c8-139">*About.cshtml* est utilisé dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="018c8-139">*About.cshtml* is used in this example.</span></span>

[!code-cshtml[Main](localization/sample/POLocalization/Views/Home/About.cshtml)]

<span data-ttu-id="018c8-140">Un `IViewLocalizer` instance est injecté et utilisé pour convertir le texte « Hello world ! ».</span><span class="sxs-lookup"><span data-stu-id="018c8-140">An `IViewLocalizer` instance is injected and used to translate the text "Hello world!".</span></span>

### <a name="creating-a-po-file"></a><span data-ttu-id="018c8-141">Création d’un fichier de bon de commande</span><span class="sxs-lookup"><span data-stu-id="018c8-141">Creating a PO file</span></span>

<span data-ttu-id="018c8-142">Créez un fichier nommé  *<culture code>.po* dans votre dossier racine.</span><span class="sxs-lookup"><span data-stu-id="018c8-142">Create a file named *<culture code>.po* in your application root folder.</span></span> <span data-ttu-id="018c8-143">Dans cet exemple, le nom de fichier est *fr.po* , car la langue Français est utilisée :</span><span class="sxs-lookup"><span data-stu-id="018c8-143">In this example, the file name is *fr.po* because the French language is used:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

<span data-ttu-id="018c8-144">Ce fichier contient la chaîne à traduire et la chaîne traduite en Français.</span><span class="sxs-lookup"><span data-stu-id="018c8-144">This file stores both the string to translate and the French-translated string.</span></span> <span data-ttu-id="018c8-145">Traductions rétablir leur culture parent, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="018c8-145">Translations revert to their parent culture, if necessary.</span></span> <span data-ttu-id="018c8-146">Dans cet exemple, le *fr.po* fichier est utilisé si la culture demandée est `fr-FR` ou `fr-CA`.</span><span class="sxs-lookup"><span data-stu-id="018c8-146">In this example, the *fr.po* file is used if the requested culture is `fr-FR` or `fr-CA`.</span></span>

### <a name="testing-the-application"></a><span data-ttu-id="018c8-147">Test de l'application</span><span class="sxs-lookup"><span data-stu-id="018c8-147">Testing the application</span></span>

<span data-ttu-id="018c8-148">Exécutez votre application et accédez à l’URL `/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="018c8-148">Run your application, and navigate to the URL `/Home/About`.</span></span> <span data-ttu-id="018c8-149">Le texte **Hello world !**</span><span class="sxs-lookup"><span data-stu-id="018c8-149">The text **Hello world!**</span></span> <span data-ttu-id="018c8-150">s’affiche.</span><span class="sxs-lookup"><span data-stu-id="018c8-150">is displayed.</span></span>

<span data-ttu-id="018c8-151">Accédez à l’URL `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="018c8-151">Navigate to the URL `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="018c8-152">Le texte **Bonjour le monde !**</span><span class="sxs-lookup"><span data-stu-id="018c8-152">The text **Bonjour le monde!**</span></span> <span data-ttu-id="018c8-153">s’affiche.</span><span class="sxs-lookup"><span data-stu-id="018c8-153">is displayed.</span></span>

## <a name="pluralization"></a><span data-ttu-id="018c8-154">Pluralisation</span><span class="sxs-lookup"><span data-stu-id="018c8-154">Pluralization</span></span>

<span data-ttu-id="018c8-155">Fichiers de bon de commande prend en charge les formulaires de pluralisation, ce qui est utile lorsque la même chaîne doit être traduites différemment selon une cardinalité.</span><span class="sxs-lookup"><span data-stu-id="018c8-155">PO files support pluralization forms, which is useful when the same string needs to be translated differently based on a cardinality.</span></span> <span data-ttu-id="018c8-156">Cette tâche est effectuée compliquée par le fait que chaque langage définit des règles personnalisées pour sélectionner la chaîne à utiliser en fonction de la cardinalité.</span><span class="sxs-lookup"><span data-stu-id="018c8-156">This task is made complicated by the fact that each language defines custom rules to select which string to use based on the cardinality.</span></span>

<span data-ttu-id="018c8-157">Le package Orchard localisation fournit une API pour appeler ces différentes formes au pluriel automatiquement.</span><span class="sxs-lookup"><span data-stu-id="018c8-157">The Orchard Localization package provides an API to invoke these different plural forms automatically.</span></span>

### <a name="creating-pluralization-po-files"></a><span data-ttu-id="018c8-158">Création de fichiers de bon de commande de pluralisation</span><span class="sxs-lookup"><span data-stu-id="018c8-158">Creating pluralization PO files</span></span>

<span data-ttu-id="018c8-159">Ajoutez le contenu suivant à mentionnées précédemment *fr.po* fichier :</span><span class="sxs-lookup"><span data-stu-id="018c8-159">Add the following content to the previously mentioned *fr.po* file:</span></span>

```text
msgid "There is one item."
msgid_plural "There are {0} items."
msgstr[0] "Il y a un élément."
msgstr[1] "Il y a {0} éléments."
```

<span data-ttu-id="018c8-160">Consultez [qu’est un fichier de bon de commande ?](#what-is-a-po-file) pour une explication de ce que représente chaque entrée dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="018c8-160">See [What is a PO file?](#what-is-a-po-file) for an explanation of what each entry in this example represents.</span></span>

### <a name="adding-a-language-using-different-pluralization-forms"></a><span data-ttu-id="018c8-161">Ajout d’une langue à l’aide de formulaires de pluralisation différents</span><span class="sxs-lookup"><span data-stu-id="018c8-161">Adding a language using different pluralization forms</span></span>

<span data-ttu-id="018c8-162">Les chaînes en anglais et Français ont été utilisés dans l’exemple précédent.</span><span class="sxs-lookup"><span data-stu-id="018c8-162">English and French strings were used in the previous example.</span></span> <span data-ttu-id="018c8-163">Anglais et le Français ont uniquement deux formes de pluralisation et partagent les mêmes règles de formulaire, c'est-à-dire qu’une cardinalité de 1 est mappée à la première forme au pluriel.</span><span class="sxs-lookup"><span data-stu-id="018c8-163">English and French have only two pluralization forms and share the same form rules, which is that a cardinality of one is mapped to the first plural form.</span></span> <span data-ttu-id="018c8-164">N’importe quel autre cardinalité est mappée à la deuxième forme au pluriel.</span><span class="sxs-lookup"><span data-stu-id="018c8-164">Any other cardinality is mapped to the second plural form.</span></span>

<span data-ttu-id="018c8-165">Pas toutes les langues partagent les mêmes règles.</span><span class="sxs-lookup"><span data-stu-id="018c8-165">Not all languages share the same rules.</span></span> <span data-ttu-id="018c8-166">Cela est illustré par la langue tchèque, ce qui a trois formes au pluriel.</span><span class="sxs-lookup"><span data-stu-id="018c8-166">This is illustrated with the Czech language, which has three plural forms.</span></span>

<span data-ttu-id="018c8-167">Créer le `cs.po` de fichiers comme suit, puis notez comment la pluralisation a besoin de trois des traductions différentes :</span><span class="sxs-lookup"><span data-stu-id="018c8-167">Create the `cs.po` file as follows, and note how the pluralization needs three different translations:</span></span>

[!code-text[Main](localization/sample/POLocalization/cs.po)]

<span data-ttu-id="018c8-168">Pour accepter les localisations tchèques, ajoutez `"cs"` à la liste des cultures prises en charge dans les `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="018c8-168">To accept Czech localizations, add `"cs"` to the list of supported cultures in the `ConfigureServices` method:</span></span>

```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("cs")
};
```

<span data-ttu-id="018c8-169">Modifier la *Views/Home/About.cshtml* fichier pour restituer des chaînes localisées, au pluriel pour plusieurs cardinalités :</span><span class="sxs-lookup"><span data-stu-id="018c8-169">Edit the *Views/Home/About.cshtml* file to render localized, plural strings for several cardinalities:</span></span>

```cshtml
<p>@Localizer.Plural(1, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(2, "There is one item.", "There are {0} items.")</p>
<p>@Localizer.Plural(5, "There is one item.", "There are {0} items.")</p>
```

<span data-ttu-id="018c8-170">**Remarque :** dans un scénario réel, une variable est utilisée pour représenter le nombre.</span><span class="sxs-lookup"><span data-stu-id="018c8-170">**Note:** In a real world scenario, a variable would be used to represent the count.</span></span> <span data-ttu-id="018c8-171">Ici, nous répéter le même code avec trois valeurs différentes pour exposer un cas très spécifique.</span><span class="sxs-lookup"><span data-stu-id="018c8-171">Here, we repeat the same code with three different values to expose a very specific case.</span></span>

<span data-ttu-id="018c8-172">En basculant les cultures, vous consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="018c8-172">Upon switching cultures, you see the following:</span></span>

<span data-ttu-id="018c8-173">Pour `/Home/About`:</span><span class="sxs-lookup"><span data-stu-id="018c8-173">For `/Home/About`:</span></span>

```html
There is one item.
There are 2 items.
There are 5 items.
```

<span data-ttu-id="018c8-174">Pour `/Home/About?culture=fr`:</span><span class="sxs-lookup"><span data-stu-id="018c8-174">For `/Home/About?culture=fr`:</span></span>

```html
Il y a un élément.
Il y a 2 éléments.
Il y a 5 éléments.
```

<span data-ttu-id="018c8-175">Pour `/Home/About?culture=cs`:</span><span class="sxs-lookup"><span data-stu-id="018c8-175">For `/Home/About?culture=cs`:</span></span>

```html
Existuje jedna položka.
Existují 2 položky.
Existuje 5 položek.
```

<span data-ttu-id="018c8-176">Notez que pour la culture en tchèque, les trois traductions sont différentes.</span><span class="sxs-lookup"><span data-stu-id="018c8-176">Note that for the Czech culture, the three translations are different.</span></span> <span data-ttu-id="018c8-177">Les cultures anglais et Français partagent la même construction pour les deux dernières chaînes traduites.</span><span class="sxs-lookup"><span data-stu-id="018c8-177">The French and English cultures share the same construction for the two last translated strings.</span></span>

## <a name="advanced-tasks"></a><span data-ttu-id="018c8-178">Tâches avancées</span><span class="sxs-lookup"><span data-stu-id="018c8-178">Advanced tasks</span></span>

### <a name="contextualizing-strings"></a><span data-ttu-id="018c8-179">CONTEXTUALISATION des chaînes</span><span class="sxs-lookup"><span data-stu-id="018c8-179">Contextualizing strings</span></span>

<span data-ttu-id="018c8-180">Les applications contiennent souvent les chaînes doivent être converties à plusieurs endroits.</span><span class="sxs-lookup"><span data-stu-id="018c8-180">Applications often contain the strings to be translated in several places.</span></span> <span data-ttu-id="018c8-181">La même chaîne peut-être avoir une traduction différents dans certains emplacements au sein d’une application (vues Razor ou des fichiers de classe).</span><span class="sxs-lookup"><span data-stu-id="018c8-181">The same string may have a different translation in certain locations within an app (Razor views or class files).</span></span> <span data-ttu-id="018c8-182">Un fichier de bon de commande prend en charge la notion d’un contexte de fichier, qui peut être utilisé pour classer la chaîne qui est représentée.</span><span class="sxs-lookup"><span data-stu-id="018c8-182">A PO file supports the notion of a file context, which can be used to categorize the string being represented.</span></span> <span data-ttu-id="018c8-183">À l’aide d’un contexte de fichier, une chaîne peut être traduite différemment, selon le contexte du fichier (ou l’absence d’un contexte de fichier).</span><span class="sxs-lookup"><span data-stu-id="018c8-183">Using a file context, a string can be translated differently, depending on the file context (or lack of a file context).</span></span>

<span data-ttu-id="018c8-184">Les services de localisation de bon de commande utilisent le nom de la classe complète ou la vue qui est utilisée lors de la conversion d’une chaîne.</span><span class="sxs-lookup"><span data-stu-id="018c8-184">The PO localization services use the name of the full class or the view that is used when translating a string.</span></span> <span data-ttu-id="018c8-185">Cela est accompli en définissant la valeur sur la `msgctxt` entrée.</span><span class="sxs-lookup"><span data-stu-id="018c8-185">This is accomplished by setting the value on the `msgctxt` entry.</span></span>

<span data-ttu-id="018c8-186">Considérez un ajout mineur précédemment *fr.po* exemple.</span><span class="sxs-lookup"><span data-stu-id="018c8-186">Consider a minor addition to the previous *fr.po* example.</span></span> <span data-ttu-id="018c8-187">Une vue Razor situé *Views/Home/About.cshtml* peut être défini comme le contexte du fichier en définissant la réservé `msgctxt` valeur de l’écriture :</span><span class="sxs-lookup"><span data-stu-id="018c8-187">A Razor view located at *Views/Home/About.cshtml* can be defined as the file context by setting the reserved `msgctxt` entry's value:</span></span>

```text
msgctxt "Views.Home.About"
msgid "Hello world!"
msgstr "Bonjour le monde!"
```

<span data-ttu-id="018c8-188">Avec la `msgctxt` définie en tant que tel, la traduction de texte se produit lorsque vous naviguerez vers `/Home/About?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="018c8-188">With the `msgctxt` set as such, text translation occurs when navigating to `/Home/About?culture=fr-FR`.</span></span> <span data-ttu-id="018c8-189">La conversion ne se produit pas lorsque vous naviguerez vers `/Home/Contact?culture=fr-FR`.</span><span class="sxs-lookup"><span data-stu-id="018c8-189">The translation won't occur when navigating to `/Home/Contact?culture=fr-FR`.</span></span>

<span data-ttu-id="018c8-190">Lorsqu’aucune entrée spécifique n’est mis en correspondance avec un contexte de fichier donné, mécanisme de secours de noyaux Orchard recherche un fichier de bon de commande approprié sans contexte.</span><span class="sxs-lookup"><span data-stu-id="018c8-190">When no specific entry is matched with a given file context, Orchard Core's fallback mechanism looks for an appropriate PO file without a context.</span></span> <span data-ttu-id="018c8-191">En supposant qu’il n’existe aucun contexte de fichier spécifique définie pour *Views/Home/Contact.cshtml*, la navigation vers `/Home/Contact?culture=fr-FR` charge un fichier de bon de commande tels que :</span><span class="sxs-lookup"><span data-stu-id="018c8-191">Assuming there is no specific file context defined for *Views/Home/Contact.cshtml*, navigating to `/Home/Contact?culture=fr-FR` loads a PO file such as:</span></span>

[!code-text[Main](localization/sample/POLocalization/fr.po)]

### <a name="changing-the-location-of-po-files"></a><span data-ttu-id="018c8-192">Modification de l’emplacement des fichiers de bon de commande</span><span class="sxs-lookup"><span data-stu-id="018c8-192">Changing the location of PO files</span></span>

<span data-ttu-id="018c8-193">L’emplacement par défaut des fichiers de bon de commande peut être modifié dans `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="018c8-193">The default location of PO files can be changed in `ConfigureServices`:</span></span>

```csharp
services.AddPortableObjectLocalization(options => options.ResourcesPath = "Localization");
```

<span data-ttu-id="018c8-194">Dans cet exemple, les fichiers de bon de commande sont chargés à partir de la *localisation* dossier.</span><span class="sxs-lookup"><span data-stu-id="018c8-194">In this example, the PO files are loaded from the *Localization* folder.</span></span>

### <a name="implementing-a-custom-logic-for-finding-localization-files"></a><span data-ttu-id="018c8-195">Implémenter une logique personnalisée pour rechercher des fichiers de localisation</span><span class="sxs-lookup"><span data-stu-id="018c8-195">Implementing a custom logic for finding localization files</span></span>

<span data-ttu-id="018c8-196">Lorsqu’une logique plus complexe est nécessaire pour localiser les fichiers de bon de commande, le `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface peut être implémentée et enregistré en tant que service.</span><span class="sxs-lookup"><span data-stu-id="018c8-196">When more complex logic is needed to locate PO files, the `OrchardCore.Localization.PortableObject.ILocalizationFileLocationProvider` interface can be implemented and registered as a service.</span></span> <span data-ttu-id="018c8-197">Cela est utile lorsque les fichiers de bon de commande peuvent être stockés dans des emplacements différents ou lorsque les fichiers doivent se trouver dans une hiérarchie de dossiers.</span><span class="sxs-lookup"><span data-stu-id="018c8-197">This is useful when PO files can be stored in varying locations or when the files have to be found within a hierarchy of folders.</span></span>

### <a name="using-a-different-default-pluralized-language"></a><span data-ttu-id="018c8-198">À l’aide d’une autre langue par défaut pluralisé</span><span class="sxs-lookup"><span data-stu-id="018c8-198">Using a different default pluralized language</span></span>

<span data-ttu-id="018c8-199">Le package inclut un `Plural` méthode d’extension qui est spécifique à deux formes au pluriel.</span><span class="sxs-lookup"><span data-stu-id="018c8-199">The package includes a `Plural` extension method that is specific to two plural forms.</span></span> <span data-ttu-id="018c8-200">Pour les langues nécessitant plus pluriel, créez une méthode d’extension.</span><span class="sxs-lookup"><span data-stu-id="018c8-200">For languages requiring more plural forms, create an extension method.</span></span> <span data-ttu-id="018c8-201">Avec une méthode d’extension, vous n’aurez pas à fournir n’importe quel fichier de localisation pour la langue par défaut &mdash; les chaînes d’origine sont déjà disponibles directement dans le code.</span><span class="sxs-lookup"><span data-stu-id="018c8-201">With an extension method, you won't need to provide any localization file for the default language &mdash; the original strings are already available directly in the code.</span></span>

<span data-ttu-id="018c8-202">Vous pouvez utiliser le plus générique `Plural(int count, string[] pluralForms, params object[] arguments)` surcharge qui accepte un tableau de chaînes de traductions.</span><span class="sxs-lookup"><span data-stu-id="018c8-202">You can use the more generic `Plural(int count, string[] pluralForms, params object[] arguments)` overload which accepts a string array of translations.</span></span>
