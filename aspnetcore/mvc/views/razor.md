---
title: "Référence de la syntaxe Razor pour ASP.NET Core"
author: rick-anderson
description: "En savoir plus sur la syntaxe Razor balisage pour l’incorporation de code serveur dans les pages Web."
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: d932e28246998c60e2b3f9c77a2521fe55991e85
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="ff28e-103">Syntaxe Razor pour ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ff28e-103">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="ff28e-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), et [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="ff28e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="ff28e-105">Razor est une syntaxe de balisage pour l’incorporation de code serveur dans les pages Web.</span><span class="sxs-lookup"><span data-stu-id="ff28e-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="ff28e-106">La syntaxe Razor est constitué de Razor balisage, c# et HTML.</span><span class="sxs-lookup"><span data-stu-id="ff28e-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="ff28e-107">Les fichiers contenant Razor généralement ont un *.cshtml* extension de fichier.</span><span class="sxs-lookup"><span data-stu-id="ff28e-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="ff28e-108">Rendu HTML</span><span class="sxs-lookup"><span data-stu-id="ff28e-108">Rendering HTML</span></span>

<span data-ttu-id="ff28e-109">La langue de Razor par défaut est HTML.</span><span class="sxs-lookup"><span data-stu-id="ff28e-109">The default Razor language is HTML.</span></span> <span data-ttu-id="ff28e-110">HTML rendu à partir du balisage de Razor n’est pas différente de rendu HTML à partir d’un fichier HTML.</span><span class="sxs-lookup"><span data-stu-id="ff28e-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="ff28e-111">Balisage HTML dans *.cshtml* fichiers Razor est restitué par le serveur sans modification.</span><span class="sxs-lookup"><span data-stu-id="ff28e-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="ff28e-112">Syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="ff28e-112">Razor syntax</span></span>

<span data-ttu-id="ff28e-113">Razor prend en charge de c# et utilise le `@` symbole pour effectuer la transition du code HTML en c#.</span><span class="sxs-lookup"><span data-stu-id="ff28e-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="ff28e-114">Razor évalue les expressions c# et les affiche dans la sortie HTML.</span><span class="sxs-lookup"><span data-stu-id="ff28e-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="ff28e-115">Lorsqu’un `@` symbole est suivi d’un [Razor de mot clé réservé](#razor-reserved-keywords), transition vers le balisage spécifique Razor.</span><span class="sxs-lookup"><span data-stu-id="ff28e-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="ff28e-116">Sinon, il adopte brut c#.</span><span class="sxs-lookup"><span data-stu-id="ff28e-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="ff28e-117">Pour éviter un `@` de symboles dans le balisage de Razor, utiliser un deuxième `@` symbole :</span><span class="sxs-lookup"><span data-stu-id="ff28e-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="ff28e-118">Le code est rendu en HTML avec un seul `@` symbole :</span><span class="sxs-lookup"><span data-stu-id="ff28e-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="ff28e-119">Attributs HTML et contenu contenant les adresses de messagerie ne traitent pas les `@` symbole comme un caractère de la transition.</span><span class="sxs-lookup"><span data-stu-id="ff28e-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="ff28e-120">Dans l’exemple suivant, les adresses de messagerie sont conservées par Razor l’analyse :</span><span class="sxs-lookup"><span data-stu-id="ff28e-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="ff28e-121">Expressions implicites Razor</span><span class="sxs-lookup"><span data-stu-id="ff28e-121">Implicit Razor expressions</span></span>

<span data-ttu-id="ff28e-122">Les expressions implicites Razor commencer par `@` suivi par le code c# :</span><span class="sxs-lookup"><span data-stu-id="ff28e-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="ff28e-123">À l’exception de C# `await` (mot clé), les expressions implicites ne doivent pas contenir des espaces.</span><span class="sxs-lookup"><span data-stu-id="ff28e-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="ff28e-124">Si l’instruction c# a une fin claire, des espaces peuvent être mélangés :</span><span class="sxs-lookup"><span data-stu-id="ff28e-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="ff28e-125">Les expressions implicites **ne peut pas** contiennent les génériques c#, comme les caractères entre crochets (`<>`) sont interprétés comme une balise HTML.</span><span class="sxs-lookup"><span data-stu-id="ff28e-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="ff28e-126">Le code suivant est **pas** valide :</span><span class="sxs-lookup"><span data-stu-id="ff28e-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="ff28e-127">Le code précédent génère une erreur du compilateur semblable à un des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="ff28e-127">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="ff28e-128">L’élément « int » n’a pas été fermé.</span><span class="sxs-lookup"><span data-stu-id="ff28e-128">The "int" element was not closed.</span></span> <span data-ttu-id="ff28e-129">Tous les éléments doivent être à fermeture automatique ou concordent une balise de fin.</span><span class="sxs-lookup"><span data-stu-id="ff28e-129">All elements must be either self-closing or have a matching end tag.</span></span>
 * <span data-ttu-id="ff28e-130">Impossible de convertir le groupe de méthodes 'GenericMethod' au type 'object' non-délégué.</span><span class="sxs-lookup"><span data-stu-id="ff28e-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="ff28e-131">Souhaitiez-vous appeler la méthode ? »</span><span class="sxs-lookup"><span data-stu-id="ff28e-131">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="ff28e-132">Appels de méthode générique doivent être encapsulées dans un [expression Razor explicite](#explicit-razor-expressions) ou un [bloc de code Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="ff28e-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="ff28e-133">Expressions explicites Razor</span><span class="sxs-lookup"><span data-stu-id="ff28e-133">Explicit Razor expressions</span></span>

<span data-ttu-id="ff28e-134">Les expressions explicites Razor se composent d’un `@` symbole avec la parenthèse à charge équilibrée.</span><span class="sxs-lookup"><span data-stu-id="ff28e-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="ff28e-135">Pour afficher l’heure de la semaine dernière, le balisage de Razor suivant est utilisé :</span><span class="sxs-lookup"><span data-stu-id="ff28e-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="ff28e-136">Tout contenu dans le `@()` parenthèses sont évaluée et rendus à la sortie.</span><span class="sxs-lookup"><span data-stu-id="ff28e-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="ff28e-137">En général les expressions implicites, décrites dans la section précédente, ne peut pas contenir d’espaces.</span><span class="sxs-lookup"><span data-stu-id="ff28e-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="ff28e-138">Dans le code suivant, une semaine n’est pas soustraite de l’heure actuelle :</span><span class="sxs-lookup"><span data-stu-id="ff28e-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="ff28e-139">Le code restitue le code HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="ff28e-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="ff28e-140">Les expressions explicites peuvent être utilisées pour concaténer du texte avec un résultat de l’expression :</span><span class="sxs-lookup"><span data-stu-id="ff28e-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="ff28e-141">Sans l’expression explicite, `<p>Age@joe.Age</p>` est traité comme une adresse de messagerie, et `<p>Age@joe.Age</p>` est rendu.</span><span class="sxs-lookup"><span data-stu-id="ff28e-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="ff28e-142">Lors de l’écriture en tant qu’expression explicite `<p>Age33</p>` est rendu.</span><span class="sxs-lookup"><span data-stu-id="ff28e-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="ff28e-143">Les expressions explicites peuvent être utilisées pour afficher la sortie à partir des méthodes génériques dans *.cshtml* fichiers.</span><span class="sxs-lookup"><span data-stu-id="ff28e-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="ff28e-144">Dans une expression implicite, les caractères entre crochets (`<>`) sont interprétés comme une balise HTML.</span><span class="sxs-lookup"><span data-stu-id="ff28e-144">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="ff28e-145">Le balisage suivant est **pas** Razor valide :</span><span class="sxs-lookup"><span data-stu-id="ff28e-145">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="ff28e-146">Le code précédent génère une erreur du compilateur semblable à un des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="ff28e-146">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="ff28e-147">L’élément « int » n’a pas été fermé.</span><span class="sxs-lookup"><span data-stu-id="ff28e-147">The "int" element was not closed.</span></span> <span data-ttu-id="ff28e-148">Tous les éléments doivent être à fermeture automatique ou concordent une balise de fin.</span><span class="sxs-lookup"><span data-stu-id="ff28e-148">All elements must be either self-closing or have a matching end tag.</span></span>
 * <span data-ttu-id="ff28e-149">Impossible de convertir le groupe de méthodes 'GenericMethod' au type 'object' non-délégué.</span><span class="sxs-lookup"><span data-stu-id="ff28e-149">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="ff28e-150">Souhaitiez-vous appeler la méthode ? »</span><span class="sxs-lookup"><span data-stu-id="ff28e-150">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="ff28e-151">Le balisage suivant illustre l’écriture de façon correcte ce code.</span><span class="sxs-lookup"><span data-stu-id="ff28e-151">The following markup shows the correct way write this code.</span></span> <span data-ttu-id="ff28e-152">Le code est écrit en tant qu’expression explicite :</span><span class="sxs-lookup"><span data-stu-id="ff28e-152">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="ff28e-153">Encodage de l’expression</span><span class="sxs-lookup"><span data-stu-id="ff28e-153">Expression encoding</span></span>

<span data-ttu-id="ff28e-154">Les expressions c# qui correspondent à une chaîne sont encodées en HTML.</span><span class="sxs-lookup"><span data-stu-id="ff28e-154">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="ff28e-155">Les expressions c# qui correspondent aux `IHtmlContent` sont rendus directement via `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="ff28e-155">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="ff28e-156">Les expressions c# qui ne correspondent à `IHtmlContent` sont converties en une chaîne en `ToString` et codées avant leur rendus.</span><span class="sxs-lookup"><span data-stu-id="ff28e-156">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="ff28e-157">Le code restitue le code HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="ff28e-157">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="ff28e-158">Le code HTML est indiqué dans le navigateur en tant que :</span><span class="sxs-lookup"><span data-stu-id="ff28e-158">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="ff28e-159">`HtmlHelper.Raw`sortie n’est pas encodée mais sont rendue sous forme de balisage HTML.</span><span class="sxs-lookup"><span data-stu-id="ff28e-159">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="ff28e-160">À l’aide de `HtmlHelper.Raw` utilisateur unsanitized entrée est un risque de sécurité.</span><span class="sxs-lookup"><span data-stu-id="ff28e-160">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="ff28e-161">L’entrée d’utilisateur peut contenir malveillant JavaScript ou autres attaques.</span><span class="sxs-lookup"><span data-stu-id="ff28e-161">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="ff28e-162">Il est difficile d’expurgation de l’entrée d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ff28e-162">Sanitizing user input is difficult.</span></span> <span data-ttu-id="ff28e-163">Évitez d’utiliser `HtmlHelper.Raw` avec l’entrée d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ff28e-163">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="ff28e-164">Le code restitue le code HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="ff28e-164">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="ff28e-165">Blocs de code Razor</span><span class="sxs-lookup"><span data-stu-id="ff28e-165">Razor code blocks</span></span>

<span data-ttu-id="ff28e-166">Blocs de code Razor commencent par `@` et sont placés par `{}`.</span><span class="sxs-lookup"><span data-stu-id="ff28e-166">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="ff28e-167">Contrairement aux expressions, le code c# à l’intérieur des blocs de code n’est pas rendu.</span><span class="sxs-lookup"><span data-stu-id="ff28e-167">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="ff28e-168">Blocs de code et des expressions dans une vue de partagent la même portée et sont définies dans l’ordre :</span><span class="sxs-lookup"><span data-stu-id="ff28e-168">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

<span data-ttu-id="ff28e-169">Le code restitue le code HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="ff28e-169">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="ff28e-170">Transitions implicites</span><span class="sxs-lookup"><span data-stu-id="ff28e-170">Implicit transitions</span></span>

<span data-ttu-id="ff28e-171">La langue par défaut dans un bloc de code est c#, mais la Page Razor peut effectuer la transition en HTML :</span><span class="sxs-lookup"><span data-stu-id="ff28e-171">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="ff28e-172">Transition délimitée explicite</span><span class="sxs-lookup"><span data-stu-id="ff28e-172">Explicit delimited transition</span></span>

<span data-ttu-id="ff28e-173">Pour définir une sous-section d’un bloc de code qui doit être rendu HTML, entourer les caractères pour le rendu de Razor  **\<texte >** balise :</span><span class="sxs-lookup"><span data-stu-id="ff28e-173">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="ff28e-174">Utilisez cette approche pour le rendu HTML n’est pas entourée d’une balise HTML.</span><span class="sxs-lookup"><span data-stu-id="ff28e-174">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="ff28e-175">Sans une balise HTML ou Razor, une erreur d’exécution Razor se produit.</span><span class="sxs-lookup"><span data-stu-id="ff28e-175">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="ff28e-176">Le  **\<texte >** balise est utile pour contrôler l’espace blanc lors du rendu du contenu :</span><span class="sxs-lookup"><span data-stu-id="ff28e-176">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="ff28e-177">Seul le contenu entre le  **\<texte >** balise est affichée.</span><span class="sxs-lookup"><span data-stu-id="ff28e-177">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="ff28e-178">Aucun espace blanc avant ou après le  **\<texte >** étiquette s’affiche dans la sortie HTML.</span><span class="sxs-lookup"><span data-stu-id="ff28e-178">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="ff28e-179">Transition de ligne explicite avec @:</span><span class="sxs-lookup"><span data-stu-id="ff28e-179">Explicit Line Transition with @:</span></span>

<span data-ttu-id="ff28e-180">Pour rendre le reste d’une ligne entière à l’intérieur d’un bloc de code HTML, utilisez le `@:` syntaxe :</span><span class="sxs-lookup"><span data-stu-id="ff28e-180">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="ff28e-181">Sans le `@:` dans le code, une erreur d’exécution Razor est générée.</span><span class="sxs-lookup"><span data-stu-id="ff28e-181">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="ff28e-182">Avertissement : Extra `@` caractères dans un fichier Razor risque de provoquer des erreurs du compilateur au niveau des instructions plus loin dans le bloc.</span><span class="sxs-lookup"><span data-stu-id="ff28e-182">Warning: Extra `@` characters in a Razor file can cause cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="ff28e-183">Ces erreurs du compilateur peuvent être difficiles à comprendre, car l’erreur se produit avant l’erreur signalée.</span><span class="sxs-lookup"><span data-stu-id="ff28e-183">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="ff28e-184">Cette erreur est courant après la combinaison de plusieurs expressions implicite/explicite dans un bloc de code unique.</span><span class="sxs-lookup"><span data-stu-id="ff28e-184">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="ff28e-185">Structures de contrôle</span><span class="sxs-lookup"><span data-stu-id="ff28e-185">Control Structures</span></span>

<span data-ttu-id="ff28e-186">Structures de contrôle sont une extension des blocs de code.</span><span class="sxs-lookup"><span data-stu-id="ff28e-186">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="ff28e-187">Tous les aspects des blocs de code (transition au balisage, inline c#) également s’appliquent aux structures suivantes :</span><span class="sxs-lookup"><span data-stu-id="ff28e-187">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="ff28e-188">Instructions conditionnelles @if, sinon si, l’autre, et@switch</span><span class="sxs-lookup"><span data-stu-id="ff28e-188">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="ff28e-189">`@if`détermine à quel moment le code s’exécute :</span><span class="sxs-lookup"><span data-stu-id="ff28e-189">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="ff28e-190">`else`et `else if` ne nécessitent pas le `@` symbole :</span><span class="sxs-lookup"><span data-stu-id="ff28e-190">`else` and `else if` don't require the `@` symbol:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

<span data-ttu-id="ff28e-191">Le balisage suivant montre comment utiliser une instruction switch :</span><span class="sxs-lookup"><span data-stu-id="ff28e-191">The following markup shows how to use a switch statement:</span></span>

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="ff28e-192">Bouclage @for, @foreach, @while, et @do tandis que</span><span class="sxs-lookup"><span data-stu-id="ff28e-192">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="ff28e-193">Basé sur un modèle HTML peut être rendu avec des instructions de contrôle de boucle.</span><span class="sxs-lookup"><span data-stu-id="ff28e-193">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="ff28e-194">Pour afficher une liste de personnes :</span><span class="sxs-lookup"><span data-stu-id="ff28e-194">To render a list of people:</span></span>

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

<span data-ttu-id="ff28e-195">Les instructions de boucles suivantes sont prises en charge :</span><span class="sxs-lookup"><span data-stu-id="ff28e-195">The following looping statements are supported:</span></span>

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a><span data-ttu-id="ff28e-196">COMPOSÉS@using</span><span class="sxs-lookup"><span data-stu-id="ff28e-196">Compound @using</span></span>

<span data-ttu-id="ff28e-197">En c#, un `using` instruction est utilisée pour vérifier un objet est supprimé.</span><span class="sxs-lookup"><span data-stu-id="ff28e-197">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="ff28e-198">Dans Razor, le même mécanisme est utilisé pour créer des programmes d’assistance HTML qui contiennent du contenu supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="ff28e-198">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="ff28e-199">Dans le code suivant, les programmes d’assistance HTML restituer une balise form avec la `@using` instruction :</span><span class="sxs-lookup"><span data-stu-id="ff28e-199">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

<span data-ttu-id="ff28e-200">Actions au niveau de l’étendue peuvent être effectuées avec [programmes d’assistance de balise](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="ff28e-200">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="ff28e-201">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="ff28e-201">@try, catch, finally</span></span>

<span data-ttu-id="ff28e-202">La gestion des exceptions sont similaire à c# :</span><span class="sxs-lookup"><span data-stu-id="ff28e-202">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="ff28e-203">Razor a la possibilité de protéger les sections critiques avec des instructions de verrouillage :</span><span class="sxs-lookup"><span data-stu-id="ff28e-203">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="ff28e-204">Commentaires</span><span class="sxs-lookup"><span data-stu-id="ff28e-204">Comments</span></span>

<span data-ttu-id="ff28e-205">Razor prend en charge les commentaires HTML et c# :</span><span class="sxs-lookup"><span data-stu-id="ff28e-205">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="ff28e-206">Le code restitue le code HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="ff28e-206">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="ff28e-207">Commentaires de Razor sont supprimés par le serveur avant le rendu de la page Web.</span><span class="sxs-lookup"><span data-stu-id="ff28e-207">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="ff28e-208">Razor utilise `@*  *@` pour délimiter les commentaires.</span><span class="sxs-lookup"><span data-stu-id="ff28e-208">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="ff28e-209">Le code suivant est commenté pour le serveur n’effectue aucun rendu tout balisage :</span><span class="sxs-lookup"><span data-stu-id="ff28e-209">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="ff28e-210">Directives</span><span class="sxs-lookup"><span data-stu-id="ff28e-210">Directives</span></span>

<span data-ttu-id="ff28e-211">Directives de Razor sont représentées par des expressions implicites avec les éléments suivants de mots clés réservés du `@` symbole.</span><span class="sxs-lookup"><span data-stu-id="ff28e-211">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="ff28e-212">Une directive modifie la manière dont une vue est analysée ou active des fonctionnalités différentes.</span><span class="sxs-lookup"><span data-stu-id="ff28e-212">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="ff28e-213">Comprendre comment Razor génère du code pour une vue rend plus facile à comprendre comment fonctionnent les directives.</span><span class="sxs-lookup"><span data-stu-id="ff28e-213">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="ff28e-214">Le code génère une classe semblable au suivant :</span><span class="sxs-lookup"><span data-stu-id="ff28e-214">The code generates a class similar to the following:</span></span>

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

<span data-ttu-id="ff28e-215">Plus loin dans cet article, la section [affichage de la classe c# Razor générée pour une vue](#viewing-the-razor-c-class-generated-for-a-view) explique comment afficher cette classe générée.</span><span class="sxs-lookup"><span data-stu-id="ff28e-215">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="ff28e-216">Le `@using` directive ajoute c# `using` directive à la vue générée :</span><span class="sxs-lookup"><span data-stu-id="ff28e-216">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="ff28e-217">Le `@model` directive spécifie le type du modèle passé à une vue :</span><span class="sxs-lookup"><span data-stu-id="ff28e-217">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="ff28e-218">Dans une application ASP.NET MVC de base créée avec les comptes d’utilisateur individuels, le *Views/Account/Login.cshtml* affichage contient la déclaration de modèle suivante :</span><span class="sxs-lookup"><span data-stu-id="ff28e-218">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="ff28e-219">La classe générée hérite `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="ff28e-219">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="ff28e-220">Razor expose un `Model` propriété pour accéder au modèle passées à la vue :</span><span class="sxs-lookup"><span data-stu-id="ff28e-220">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="ff28e-221">Le `@model` directive spécifie le type de cette propriété.</span><span class="sxs-lookup"><span data-stu-id="ff28e-221">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="ff28e-222">La directive spécifie le `T` dans `RazorPage<T>` que généré classe que la vue dérive.</span><span class="sxs-lookup"><span data-stu-id="ff28e-222">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="ff28e-223">Si le `@model` directive n’est pas spécifié, le `Model` propriété est de type `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="ff28e-223">If the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="ff28e-224">La valeur du modèle est passée à la vue à partir du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="ff28e-224">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="ff28e-225">Pour plus d’informations, consultez [fortement typée de modèles et les @model (mot clé).</span><span class="sxs-lookup"><span data-stu-id="ff28e-225">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="ff28e-226">Le `@inherits` directive fournit un contrôle complet de la classe qui hérite de la vue :</span><span class="sxs-lookup"><span data-stu-id="ff28e-226">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="ff28e-227">Le code suivant est un type de page Razor personnalisé :</span><span class="sxs-lookup"><span data-stu-id="ff28e-227">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="ff28e-228">Le `CustomText` s’affiche dans une vue :</span><span class="sxs-lookup"><span data-stu-id="ff28e-228">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="ff28e-229">Le code restitue le code HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="ff28e-229">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="ff28e-230">`@model`et `@inherits` peut être utilisé dans la même vue.</span><span class="sxs-lookup"><span data-stu-id="ff28e-230">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="ff28e-231">`@inherits`peut être dans un *_ViewImports.cshtml* fichier importe de la vue :</span><span class="sxs-lookup"><span data-stu-id="ff28e-231">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="ff28e-232">Le code suivant est un exemple d’une vue fortement typée :</span><span class="sxs-lookup"><span data-stu-id="ff28e-232">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="ff28e-233">Si «rick@contoso.com» est passé dans le modèle, la vue génère le code HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="ff28e-233">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="ff28e-234">Le `@inject` directive permet à la Page Razor injecter un service à partir de la [conteneur de service](xref:fundamentals/dependency-injection) dans une vue.</span><span class="sxs-lookup"><span data-stu-id="ff28e-234">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="ff28e-235">Pour plus d’informations, consultez [injection de dépendances dans les vues](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ff28e-235">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="ff28e-236">Le `@functions` directive permet à une Page Razor ajouter le contenu au niveau des fonctions à une vue :</span><span class="sxs-lookup"><span data-stu-id="ff28e-236">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="ff28e-237">Exemple :</span><span class="sxs-lookup"><span data-stu-id="ff28e-237">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="ff28e-238">Le code génère le balisage HTML suivant :</span><span class="sxs-lookup"><span data-stu-id="ff28e-238">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="ff28e-239">Le code suivant est la classe générée Razor c# :</span><span class="sxs-lookup"><span data-stu-id="ff28e-239">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="ff28e-240">Le `@section` directive est utilisée conjointement avec la [disposition](xref:mvc/views/layout) pour activer des vues à restituer le contenu dans les différentes parties de la page HTML.</span><span class="sxs-lookup"><span data-stu-id="ff28e-240">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="ff28e-241">Pour plus d’informations, consultez [Sections](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="ff28e-241">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="ff28e-242">Programmes d’assistance de balise</span><span class="sxs-lookup"><span data-stu-id="ff28e-242">Tag Helpers</span></span>

<span data-ttu-id="ff28e-243">Il existe trois directives qui se rapportent à [programmes d’assistance de balise](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="ff28e-243">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="ff28e-244">Directive</span><span class="sxs-lookup"><span data-stu-id="ff28e-244">Directive</span></span> | <span data-ttu-id="ff28e-245">Fonction</span><span class="sxs-lookup"><span data-stu-id="ff28e-245">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="ff28e-246">Rend les programmes d’assistance de balise disponibles à une vue.</span><span class="sxs-lookup"><span data-stu-id="ff28e-246">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="ff28e-247">Supprime les programmes d’assistance de balise précédemment ajoutés à partir d’une vue.</span><span class="sxs-lookup"><span data-stu-id="ff28e-247">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="ff28e-248">Spécifie un préfixe de balise pour activer la prise en charge de l’application d’assistance de balise et de rendre l’utilisation du programme d’assistance de balise explicite.</span><span class="sxs-lookup"><span data-stu-id="ff28e-248">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="ff28e-249">Mots clés de Razor réservé</span><span class="sxs-lookup"><span data-stu-id="ff28e-249">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="ff28e-250">Mots clés de Razor</span><span class="sxs-lookup"><span data-stu-id="ff28e-250">Razor keywords</span></span>

* <span data-ttu-id="ff28e-251">page (nécessite un cœur d’ASP.NET 2.0 et versions ultérieur)</span><span class="sxs-lookup"><span data-stu-id="ff28e-251">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="ff28e-252">fonctions</span><span class="sxs-lookup"><span data-stu-id="ff28e-252">functions</span></span>
* <span data-ttu-id="ff28e-253">hérite</span><span class="sxs-lookup"><span data-stu-id="ff28e-253">inherits</span></span>
* <span data-ttu-id="ff28e-254">modèle</span><span class="sxs-lookup"><span data-stu-id="ff28e-254">model</span></span>
* <span data-ttu-id="ff28e-255">section</span><span class="sxs-lookup"><span data-stu-id="ff28e-255">section</span></span>
* <span data-ttu-id="ff28e-256">application d’assistance (non prises en charge par ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="ff28e-256">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="ff28e-257">Mots clés de Razor sont précédés de `@(Razor Keyword)` (par exemple, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="ff28e-257">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="ff28e-258">Mots clés du langage c# Razor</span><span class="sxs-lookup"><span data-stu-id="ff28e-258">C# Razor keywords</span></span>

* <span data-ttu-id="ff28e-259">casse</span><span class="sxs-lookup"><span data-stu-id="ff28e-259">case</span></span>
* <span data-ttu-id="ff28e-260">do</span><span class="sxs-lookup"><span data-stu-id="ff28e-260">do</span></span>
* <span data-ttu-id="ff28e-261">default</span><span class="sxs-lookup"><span data-stu-id="ff28e-261">default</span></span>
* <span data-ttu-id="ff28e-262">for</span><span class="sxs-lookup"><span data-stu-id="ff28e-262">for</span></span>
* <span data-ttu-id="ff28e-263">foreach</span><span class="sxs-lookup"><span data-stu-id="ff28e-263">foreach</span></span>
* <span data-ttu-id="ff28e-264">if</span><span class="sxs-lookup"><span data-stu-id="ff28e-264">if</span></span>
* <span data-ttu-id="ff28e-265">else</span><span class="sxs-lookup"><span data-stu-id="ff28e-265">else</span></span>
* <span data-ttu-id="ff28e-266">lock</span><span class="sxs-lookup"><span data-stu-id="ff28e-266">lock</span></span>
* <span data-ttu-id="ff28e-267">switch</span><span class="sxs-lookup"><span data-stu-id="ff28e-267">switch</span></span>
* <span data-ttu-id="ff28e-268">try</span><span class="sxs-lookup"><span data-stu-id="ff28e-268">try</span></span>
* <span data-ttu-id="ff28e-269">catch</span><span class="sxs-lookup"><span data-stu-id="ff28e-269">catch</span></span>
* <span data-ttu-id="ff28e-270">finally</span><span class="sxs-lookup"><span data-stu-id="ff28e-270">finally</span></span>
* <span data-ttu-id="ff28e-271">using</span><span class="sxs-lookup"><span data-stu-id="ff28e-271">using</span></span>
* <span data-ttu-id="ff28e-272">while</span><span class="sxs-lookup"><span data-stu-id="ff28e-272">while</span></span>

<span data-ttu-id="ff28e-273">Mots clés du langage c# Razor doivent être échappé en double avec `@(@C# Razor Keyword)` (par exemple, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="ff28e-273">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="ff28e-274">Le premier `@` remplace l’analyseur Razor.</span><span class="sxs-lookup"><span data-stu-id="ff28e-274">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="ff28e-275">La seconde `@` remplace l’analyseur c#.</span><span class="sxs-lookup"><span data-stu-id="ff28e-275">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="ff28e-276">Mots clés réservés non utilisées par Razor</span><span class="sxs-lookup"><span data-stu-id="ff28e-276">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="ff28e-277">namespace</span><span class="sxs-lookup"><span data-stu-id="ff28e-277">namespace</span></span>
* <span data-ttu-id="ff28e-278">class</span><span class="sxs-lookup"><span data-stu-id="ff28e-278">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="ff28e-279">Affichage de la classe c# Razor générée pour une vue</span><span class="sxs-lookup"><span data-stu-id="ff28e-279">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="ff28e-280">Ajoutez la classe suivante au projet ASP.NET MVC de base :</span><span class="sxs-lookup"><span data-stu-id="ff28e-280">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="ff28e-281">Remplacer la `RazorTemplateEngine` ajouté par MVC avec la `CustomTemplateEngine` classe :</span><span class="sxs-lookup"><span data-stu-id="ff28e-281">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="ff28e-282">Définir un point d’arrêt sur la `return csharpDocument` instruction de `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="ff28e-282">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="ff28e-283">Lorsque l’exécution du programme s’arrête au point d’arrêt, afficher la valeur de `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="ff28e-283">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Vue de generatedCode visualiseur de texte](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="ff28e-285">Recherches de vue et de respect de la casse</span><span class="sxs-lookup"><span data-stu-id="ff28e-285">View lookups and case sensitivity</span></span>

<span data-ttu-id="ff28e-286">Le moteur d’affichage Razor effectue des recherches respectant la casse pour les vues.</span><span class="sxs-lookup"><span data-stu-id="ff28e-286">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="ff28e-287">Toutefois, la recherche réelle est déterminée par le système de fichiers sous-jacent :</span><span class="sxs-lookup"><span data-stu-id="ff28e-287">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="ff28e-288">Source en fonction du fichier :</span><span class="sxs-lookup"><span data-stu-id="ff28e-288">File based source:</span></span> 
  * <span data-ttu-id="ff28e-289">Sur les systèmes d’exploitation avec les systèmes de fichiers de la casse (par exemple, Windows), les recherches de fournisseur de fichier physique sont insensible à la casse.</span><span class="sxs-lookup"><span data-stu-id="ff28e-289">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="ff28e-290">Par exemple, `return View("Test")` entraîne des correspondances pour */Views/Home/Test.cshtml*, */Views/home/test.cshtml*et n’importe quel autre variante de la casse.</span><span class="sxs-lookup"><span data-stu-id="ff28e-290">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="ff28e-291">Sur les systèmes de fichiers respectant la casse (par exemple, Linux, OS x et avec `EmbeddedFileProvider`), les recherches respectent la casse.</span><span class="sxs-lookup"><span data-stu-id="ff28e-291">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="ff28e-292">Par exemple, `return View("Test")` spécifiquement les correspondances */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ff28e-292">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="ff28e-293">Précompilés vues : avec le cœur d’ASP.NET 2.0 et versions ultérieur, de vues précompilés recherche respecte la casse sur tous les systèmes d’exploitation.</span><span class="sxs-lookup"><span data-stu-id="ff28e-293">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="ff28e-294">Le comportement est identique au comportement du fournisseur du fichier physique sous Windows.</span><span class="sxs-lookup"><span data-stu-id="ff28e-294">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="ff28e-295">Si deux vues précompilés diffèrent uniquement par la casse, le résultat de recherche est non déterministe.</span><span class="sxs-lookup"><span data-stu-id="ff28e-295">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="ff28e-296">Les développeurs sont encouragés à correspondre à la casse des noms de fichiers et de répertoires à la casse de :</span><span class="sxs-lookup"><span data-stu-id="ff28e-296">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="ff28e-297">Noms de zone, le contrôleur et action.</span><span class="sxs-lookup"><span data-stu-id="ff28e-297">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="ff28e-298">Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="ff28e-298">Razor Pages.</span></span>
    
<span data-ttu-id="ff28e-299">Casse garantit que les déploiements de rechercher leur point de vue, quelle que soit le système de fichiers sous-jacent.</span><span class="sxs-lookup"><span data-stu-id="ff28e-299">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
