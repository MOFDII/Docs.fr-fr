---
title: "Ajout d’une fonction de recherche"
author: rick-anderson
description: "Montre comment ajouter une fonction de recherche à une application ASP.NET MVC simple"
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: 2d8a18365a0d46d6468d708e1cd02def071309b7
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

Remarque : SQLite respecte la casse, vous devez donc rechercher « Ghost » et non pas « ghost ».

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

Changez la balise `<form>` dans la vue Razor *Views\movie\Index.cshtml* pour spécifier `method="get"` :

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
[Précédent - Méthodes et vues du contrôleur](controller-methods-views.md)
[Suivant - Ajouter un champ](new-field.md)  
