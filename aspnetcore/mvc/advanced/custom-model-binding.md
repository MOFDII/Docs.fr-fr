---
title: "Liaison de modèle personnalisé"
author: ardalis
description: "Personnalisation de la liaison de modèle dans ASP.NET MVC de base."
ms.author: riande
manager: wpickett
ms.date: 04/10/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: d8b94f53954c5ab63ccf3aab4eb7a7a7dbea487b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="custom-model-binding"></a><span data-ttu-id="f2f90-103">Liaison de modèle personnalisé</span><span class="sxs-lookup"><span data-stu-id="f2f90-103">Custom Model Binding</span></span>

<span data-ttu-id="f2f90-104">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f2f90-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f2f90-105">Liaison de modèle autorise les actions de contrôleur travailler directement avec les types de modèle (transmis en tant qu’arguments de méthode), plutôt que les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="f2f90-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="f2f90-106">Mappage entre les modèles de données et votre application demande entrantes est gérée par les classeurs de modèles.</span><span class="sxs-lookup"><span data-stu-id="f2f90-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="f2f90-107">Les développeurs peuvent étendre les fonctionnalités de liaison de modèle intégrée en implémentant des classeurs de modèles personnalisés (même si en règle générale, vous n’avez pas besoin d’écrire votre propre fournisseur).</span><span class="sxs-lookup"><span data-stu-id="f2f90-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="f2f90-108">Afficher ou télécharger l’exemple à partir de GitHub</span><span class="sxs-lookup"><span data-stu-id="f2f90-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="f2f90-109">Limitations de classeur de modèle par défaut</span><span class="sxs-lookup"><span data-stu-id="f2f90-109">Default model binder limitations</span></span>

<span data-ttu-id="f2f90-110">Les classeurs de modèles par défaut prennent en charge la plupart des types de données .NET Core courants et doivent répondre aux besoins de la plupart des développeurs.</span><span class="sxs-lookup"><span data-stu-id="f2f90-110">The default model binders support most of the common .NET Core data types and should meet most developers\` needs.</span></span> <span data-ttu-id="f2f90-111">Ils s’attendre à lier des données texte à partir de la demande directement à des types de modèle.</span><span class="sxs-lookup"><span data-stu-id="f2f90-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="f2f90-112">Vous devrez peut-être transformer l’entrée avant la liaison.</span><span class="sxs-lookup"><span data-stu-id="f2f90-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="f2f90-113">Par exemple, si vous avez une clé qui peut être utilisée pour rechercher des données de modèle.</span><span class="sxs-lookup"><span data-stu-id="f2f90-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="f2f90-114">Vous pouvez utiliser un classeur de modèles personnalisés pour extraire des données en fonction de la clé.</span><span class="sxs-lookup"><span data-stu-id="f2f90-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="f2f90-115">Vérification de liaison de modèle</span><span class="sxs-lookup"><span data-stu-id="f2f90-115">Model binding review</span></span>

<span data-ttu-id="f2f90-116">Liaison de modèle utilise des définitions spécifiques pour les types qu’il opère sur.</span><span class="sxs-lookup"><span data-stu-id="f2f90-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="f2f90-117">A *type simple* est convertie à partir d’une chaîne unique dans l’entrée.</span><span class="sxs-lookup"><span data-stu-id="f2f90-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="f2f90-118">A *type complexe* est convertie à partir de plusieurs valeurs d’entrée.</span><span class="sxs-lookup"><span data-stu-id="f2f90-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="f2f90-119">L’infrastructure détermine la différence en fonction de l’existence d’un `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="f2f90-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="f2f90-120">Il est recommandé que vous créez un convertisseur de type, si vous disposez d’un simple `string`  ->  `SomeType` mappage ne nécessitant pas de ressources externes.</span><span class="sxs-lookup"><span data-stu-id="f2f90-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="f2f90-121">Avant de créer votre propre classeur de modèles personnalisés, il est important de consulter des modèles existants classeurs sont implémentées.</span><span class="sxs-lookup"><span data-stu-id="f2f90-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="f2f90-122">Prendre en compte la [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) qui peut être utilisé pour convertir des chaînes codées en base64 en tableaux d’octets.</span><span class="sxs-lookup"><span data-stu-id="f2f90-122">Consider the [ByteArrayModelBinder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="f2f90-123">Les tableaux d’octets sont souvent stockées en tant que fichiers ou des champs de base de données BLOB.</span><span class="sxs-lookup"><span data-stu-id="f2f90-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="f2f90-124">Utilisation de la ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="f2f90-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="f2f90-125">Chaînes encodées en Base64 peuvent être utilisés pour représenter les données binaires.</span><span class="sxs-lookup"><span data-stu-id="f2f90-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="f2f90-126">Par exemple, l’image suivante peut être encodé sous forme de chaîne.</span><span class="sxs-lookup"><span data-stu-id="f2f90-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="f2f90-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span><span class="sxs-lookup"><span data-stu-id="f2f90-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="f2f90-128">Une petite partie de la chaîne encodée est indiquée dans l’image suivante :</span><span class="sxs-lookup"><span data-stu-id="f2f90-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="f2f90-129">![bot dotnet codé](custom-model-binding/images/encoded-bot.png "bot dotnet codée")</span><span class="sxs-lookup"><span data-stu-id="f2f90-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="f2f90-130">Suivez les instructions de la [fichier Lisezmoi de l’exemple](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) pour convertir la chaîne codée en base 64 dans un fichier.</span><span class="sxs-lookup"><span data-stu-id="f2f90-130">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="f2f90-131">ASP.NET MVC de base peut prendre une chaînes codées en base64 et utiliser un `ByteArrayModelBinder` pour le convertir en un tableau d’octets.</span><span class="sxs-lookup"><span data-stu-id="f2f90-131">ASP.NET Core MVC can take a base64-encoded strings and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="f2f90-132">Le [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) qui implémente [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) mappe `byte[]` arguments à `ByteArrayModelBinder`:</span><span class="sxs-lookup"><span data-stu-id="f2f90-132">The [ByteArrayModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

<span data-ttu-id="f2f90-133">Lorsque vous créez votre propre classeur de modèles personnalisés, vous pouvez implémenter votre propre `IModelBinderProvider` tapez, ou utilisez le [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="f2f90-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="f2f90-134">L’exemple suivant montre comment utiliser `ByteArrayModelBinder` pour convertir une chaîne codée en base 64 pour un `byte[]` et enregistrer le résultat dans un fichier :</span><span class="sxs-lookup"><span data-stu-id="f2f90-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="f2f90-135">Vous pouvez valider une chaîne codée en base64 à cette méthode d’api à l’aide d’un outil tel que [Postman](https://www.getpostman.com/):</span><span class="sxs-lookup"><span data-stu-id="f2f90-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="f2f90-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="f2f90-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="f2f90-137">Tant que le classeur peut lier des données de demande aux propriétés correctement nommées ou d’arguments, la liaison de modèle réussit.</span><span class="sxs-lookup"><span data-stu-id="f2f90-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="f2f90-138">L’exemple suivant montre comment utiliser `ByteArrayModelBinder` avec un modèle d’affichage :</span><span class="sxs-lookup"><span data-stu-id="f2f90-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="f2f90-139">Exemple de classeur de modèle personnalisé</span><span class="sxs-lookup"><span data-stu-id="f2f90-139">Custom model binder sample</span></span>

<span data-ttu-id="f2f90-140">Dans cette section, nous allons implémenter un classeur de modèles personnalisés qui :</span><span class="sxs-lookup"><span data-stu-id="f2f90-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="f2f90-141">Convertit les données de demandes entrantes en arguments clés fortement typées.</span><span class="sxs-lookup"><span data-stu-id="f2f90-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="f2f90-142">Utilise Entity Framework Core pour extraire de l’entité associée.</span><span class="sxs-lookup"><span data-stu-id="f2f90-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="f2f90-143">Passe l’entité associée en tant qu’argument à la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="f2f90-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="f2f90-144">L’exemple suivant utilise le `ModelBinder` de l’attribut le `Author` modèle :</span><span class="sxs-lookup"><span data-stu-id="f2f90-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="f2f90-145">Dans le code précédent, le `ModelBinder` attribut spécifie le type de `IModelBinder` qui doit être utilisé pour lier `Author` paramètres d’action.</span><span class="sxs-lookup"><span data-stu-id="f2f90-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="f2f90-146">Le `AuthorEntityBinder` est utilisée pour lier une `Author` paramètre par extraction de l’entité à partir d’une source de données à l’aide d’Entity Framework Core et une `authorId`:</span><span class="sxs-lookup"><span data-stu-id="f2f90-146">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="f2f90-147">Le code suivant montre comment utiliser le `AuthorEntityBinder` dans une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="f2f90-147">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="f2f90-148">Le `ModelBinder` attribut peut être utilisé pour appliquer la `AuthorEntityBinder` aux paramètres qui n’utilisent pas les conventions utilisées par défaut :</span><span class="sxs-lookup"><span data-stu-id="f2f90-148">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that do not use default conventions:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="f2f90-149">Dans cet exemple, étant donné que le nom de l’argument n’est pas la valeur par défaut `authorId`, il est spécifié sur le paramètre à l’aide `ModelBinder` attribut.</span><span class="sxs-lookup"><span data-stu-id="f2f90-149">In this example, since the name of the argument is not the default `authorId`, it's specified on the parameter using `ModelBinder` attribute.</span></span> <span data-ttu-id="f2f90-150">Notez que le contrôleur et l’action de méthode sont simplifiées par rapport à la recherche de l’entité dans la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="f2f90-150">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="f2f90-151">La logique pour extraire l’auteur à l’aide d’Entity Framework Core est déplacée vers le classeur de modèles.</span><span class="sxs-lookup"><span data-stu-id="f2f90-151">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="f2f90-152">Cela peut être considérable simplification lorsque vous disposez de plusieurs méthodes qui lier au modèle auteur et peuvent vous aider à suivre le [principe sec](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="f2f90-152">This can be considerable simplification when you have several methods that bind to the author model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="f2f90-153">Vous pouvez appliquer la `ModelBinder` d’attributs aux propriétés de modèle individuel (tel que sur un viewmodel) ou aux paramètres de méthode d’action pour spécifier un classeur de modèles ou un nom de modèle pour simplement ce type ou l’action.</span><span class="sxs-lookup"><span data-stu-id="f2f90-153">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="f2f90-154">Implémentation d’un ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="f2f90-154">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="f2f90-155">Au lieu d’appliquer un attribut, vous pouvez implémenter `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="f2f90-155">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="f2f90-156">Il s’agit d’implémentation des classeurs de la nouvelle infrastructure intégrée.</span><span class="sxs-lookup"><span data-stu-id="f2f90-156">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="f2f90-157">Lorsque vous spécifiez le type de votre classeur opère sur, vous spécifiez le type d’argument, il génère, **pas** accepte une entrée votre classeur.</span><span class="sxs-lookup"><span data-stu-id="f2f90-157">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="f2f90-158">Le fournisseur de classeurs suivant fonctionne avec les `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="f2f90-158">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="f2f90-159">Lorsqu’il est ajouté à la collection de MVC de fournisseurs, vous n’avez pas besoin d’utiliser le `ModelBinder` de l’attribut `Author` ou `Author` des paramètres typés.</span><span class="sxs-lookup"><span data-stu-id="f2f90-159">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="f2f90-160">Remarque : Le code précédent renvoie un `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="f2f90-160">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="f2f90-161">`BinderTypeModelBinder`sert de fabrique pour les classeurs de modèles et fournit l’injection de dépendance (DI).</span><span class="sxs-lookup"><span data-stu-id="f2f90-161">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="f2f90-162">Le `AuthorEntityBinder` requiert DI accéder aux EF Core.</span><span class="sxs-lookup"><span data-stu-id="f2f90-162">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="f2f90-163">Utilisez `BinderTypeModelBinder` si votre classeur de modèles exige des services de DI.</span><span class="sxs-lookup"><span data-stu-id="f2f90-163">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="f2f90-164">Pour utiliser un fournisseur de classeurs de modèles personnalisé, ajoutez-le dans `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f2f90-164">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="f2f90-165">Lors de l’évaluation de classeurs de modèles, la collection de fournisseurs est examinée dans l’ordre.</span><span class="sxs-lookup"><span data-stu-id="f2f90-165">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="f2f90-166">Le premier fournisseur qui retourne un classeur est utilisé.</span><span class="sxs-lookup"><span data-stu-id="f2f90-166">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="f2f90-167">L’illustration suivante montre la valeur par défaut de classeurs de modèles à partir du débogueur.</span><span class="sxs-lookup"><span data-stu-id="f2f90-167">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="f2f90-168">![par défaut de classeurs de modèles](custom-model-binding/images/default-model-binders.png "par défaut de classeurs de modèles")</span><span class="sxs-lookup"><span data-stu-id="f2f90-168">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="f2f90-169">Ajout de votre fournisseur à la fin de la collection peut entraîner un classeur de modèles intégrés appelé avant que votre classeur personnalisé a un risque.</span><span class="sxs-lookup"><span data-stu-id="f2f90-169">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="f2f90-170">Dans cet exemple, le fournisseur personnalisé est ajouté au début de la collection pour vous assurer qu’il est utilisé pour `Author` arguments de l’action.</span><span class="sxs-lookup"><span data-stu-id="f2f90-170">In this example, the custom provider is added to the beginning of the collection to ensure it is used for `Author` action arguments.</span></span>

[!code-csharp[Main](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="f2f90-171">Recommandations et meilleures pratiques</span><span class="sxs-lookup"><span data-stu-id="f2f90-171">Recommendations and best practices</span></span>

<span data-ttu-id="f2f90-172">Classeurs de modèles personnalisé :</span><span class="sxs-lookup"><span data-stu-id="f2f90-172">Custom model binders:</span></span>
- <span data-ttu-id="f2f90-173">Déconseillé définir les codes d’état ou de retourner des résultats (par exemple, 404 introuvable).</span><span class="sxs-lookup"><span data-stu-id="f2f90-173">Should not attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="f2f90-174">En cas de liaison de modèle, un [filtre d’action](xref:mvc/controllers/filters) ou logique dans la méthode d’action doit gérer l’échec.</span><span class="sxs-lookup"><span data-stu-id="f2f90-174">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="f2f90-175">Sont particulièrement utiles pour la suppression du code répétitif et problèmes transversaux à partir des méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="f2f90-175">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="f2f90-176">Ne doit généralement pas être utilisé pour convertir une chaîne en un type personnalisé, un [ `TypeConverter` ](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) est généralement une meilleure option.</span><span class="sxs-lookup"><span data-stu-id="f2f90-176">Typically should not be used to convert a string into a custom type, a [`TypeConverter`](https://docs.microsoft.com//dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
