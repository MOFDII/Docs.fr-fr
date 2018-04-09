---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: La gestion des publications (postback) à partir d’un contrôle Popup avec un UpdatePanel (VB) | Documents Microsoft
author: wenz
description: L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé. Une attention particulière doit être portée...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 312927e01911ea705d0614eac462cd034820c7b4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a><span data-ttu-id="11e4a-104">La gestion des publications (postback) à partir d’un contrôle Popup avec un UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="11e4a-104">Handling Postbacks from A Popup Control With an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="11e4a-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="11e4a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="11e4a-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="11e4a-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span></span>

> <span data-ttu-id="11e4a-107">L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé.</span><span class="sxs-lookup"><span data-stu-id="11e4a-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="11e4a-108">Une attention particulière est à entreprendre lorsqu’une publication (postback) se produit au sein de ce type une fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="11e4a-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="11e4a-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="11e4a-109">Overview</span></span>

<span data-ttu-id="11e4a-110">L’extendeur PopupControl dans la boîte à outils de contrôle AJAX offre un moyen simple de déclencher une fenêtre contextuelle lorsque n’importe quel autre contrôle est activé.</span><span class="sxs-lookup"><span data-stu-id="11e4a-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="11e4a-111">Une attention particulière est à entreprendre lorsqu’une publication (postback) se produit au sein de ce type une fenêtre contextuelle.</span><span class="sxs-lookup"><span data-stu-id="11e4a-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="11e4a-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="11e4a-112">Steps</span></span>

<span data-ttu-id="11e4a-113">Lorsque vous utilisez un `PopupControl` avec une publication, un `UpdatePanel` peut empêcher l’actualisation de la page a provoqué par la publication (postback).</span><span class="sxs-lookup"><span data-stu-id="11e4a-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="11e4a-114">Le balisage suivant définit deux éléments importants :</span><span class="sxs-lookup"><span data-stu-id="11e4a-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="11e4a-115">A `ScriptManager` contrôle afin que les outils de contrôle ASP.NET AJAX fonctionne</span><span class="sxs-lookup"><span data-stu-id="11e4a-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="11e4a-116">Deux `TextBox` contrôles, ce qui déclencheront à la fois une fenêtre contextuelle</span><span class="sxs-lookup"><span data-stu-id="11e4a-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="11e4a-117">A `Panel` contrôle qui servira de la fenêtre contextuelle</span><span class="sxs-lookup"><span data-stu-id="11e4a-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="11e4a-118">Dans le panneau de configuration, un `Calendar` contrôle est incorporé dans un `UpdatePanel` contrôle</span><span class="sxs-lookup"><span data-stu-id="11e4a-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="11e4a-119">Deux `PopupControlExtender` contrôles qui affectent le panneau de configuration pour les zones de texte</span><span class="sxs-lookup"><span data-stu-id="11e4a-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="11e4a-120">Notez que la `OnSelectionChanged` attribut de la `Calendar` contrôle est défini.</span><span class="sxs-lookup"><span data-stu-id="11e4a-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="11e4a-121">Donc lorsque l’utilisateur sélectionne une date dans le calendrier, une publication (postback) se produit et la méthode côté serveur `c1_SelectionChanged()` est exécutée.</span><span class="sxs-lookup"><span data-stu-id="11e4a-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="11e4a-122">Dans cette méthode, la date actuelle doit être récupérée et mises à jour dans la zone de texte.</span><span class="sxs-lookup"><span data-stu-id="11e4a-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="11e4a-123">La syntaxe est la suivante : tout d’abord, un proxy de l’objet pour le `PopupControlExtender` sur la page doit être généré.</span><span class="sxs-lookup"><span data-stu-id="11e4a-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="11e4a-124">Les outils de contrôle ASP.NET AJAX offre le `GetProxyForCurrentPopup()` (méthode).</span><span class="sxs-lookup"><span data-stu-id="11e4a-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="11e4a-125">Prend en charge de l’objet de cette méthode retourne la `Commit()` méthode qui envoie une valeur au contrôle qui a déclenché la fenêtre contextuelle (pas le contrôle qui a déclenché l’appel de méthode !).</span><span class="sxs-lookup"><span data-stu-id="11e4a-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="11e4a-126">Le code suivant fournit la date sélectionnée comme argument pour la `Commit()` méthode, à l’origine du code à réécrire la date sélectionnée dans la zone de texte :</span><span class="sxs-lookup"><span data-stu-id="11e4a-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="11e4a-127">Lorsque vous cliquez sur une date du calendrier, la date sélectionnée s’affiche dans la zone de texte correspondante, création d’un contrôle de sélecteur de dates qui permettre actuellement se trouve désormais sur de nombreux sites Web.</span><span class="sxs-lookup"><span data-stu-id="11e4a-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="11e4a-128">[![Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="11e4a-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="11e4a-129">Le calendrier s’affiche lorsque l’utilisateur clique dans la zone de texte ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="11e4a-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="11e4a-130">[![En cliquant sur une date place dans la zone de texte](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="11e4a-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="11e4a-131">En cliquant sur une date place dans la zone de texte ([cliquez pour afficher l’image en taille réelle](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="11e4a-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="11e4a-132">[Précédent](using-multiple-popup-controls-vb.md)
> [Suivant](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="11e4a-132">[Previous](using-multiple-popup-controls-vb.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span></span>
