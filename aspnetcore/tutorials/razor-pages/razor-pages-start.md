---
title: "Bien démarrer avec des pages Razor dans ASP.NET Core"
author: rick-anderson
description: "Bien démarrer avec des pages Razor dans ASP.NET Core"
keywords: ASP.NET Core,pages Razor,Razor,MVC
ms.author: riande
manager: wpickett
ms.date: 12/22/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 12685d6a391c127d78399408291c26caa0192ee2
ms.sourcegitcommit: 44a62f59d4db39d685c4487a0345a486be18d7c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2017
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>Bien démarrer avec des pages Razor dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce didacticiel décrit les principes fondamentaux liés à la génération d’une application web de pages Razor dans ASP.NET Core. L’utilisation de pages Razor est la méthode recommandée pour générer l’interface utilisateur d’applications web dans ASP.NET Core.

Il existe trois versions de ce didacticiel :

* Windows : ce didacticiel
* Mac OS : [Bien démarrer avec les pages Razor avec Visual Studio pour Mac](xref:tutorials/razor-pages-mac/razor-pages-start)
* Mac OS, Linux et Windows : [Bien démarrer avec les pages Razor dans ASP.NET Core avec Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prérequis

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a>Créer une application web Razor

* Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.
* Créez une application web ASP.NET Core. Nommez le projet **RazorPagesMovie**. Il est important de nommer le projet *RazorPagesMovie* pour que les espaces de noms correspondent quand vous copiez/collez du code.
  ![Nouvelle application web ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)
* Sélectionnez **ASP.NET Core 2.0** dans la liste déroulante, puis sélectionnez **Application web**.

> [!NOTE]
> Si vous souhaitez utiliser ASP.NET Core sur le .NET Framework, vous devez d’abord sélectionner **.NET Framework** dans la liste déroulante la plus à gauche dans la boîte de dialogue, puis la version d’ASP.NET Core souhaitée.

  ![Application web (pages Razor)](razor-pages-start/_static/np2.png)

Le modèle Visual Studio crée un projet de démarrage :

![Explorateur de solutions](razor-pages-start/_static/se.png)

Appuyez sur **F5** pour exécuter l’application en mode débogage ou sur **Ctrl+F5** pour l’exécuter sans attachement du débogueur

![Page d’accueil ou page d’index](razor-pages-start/_static/home.png)

* Visual Studio démarre [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) et exécute votre application. La barre d’adresses affiche `localhost:port#` au lieu de quelque chose qui ressemble à `example.com`. En effet, `localhost` est le nom d’hôte standard de votre ordinateur local. Localhost traite uniquement les requêtes web de l’ordinateur local. Quand Visual Studio crée un projet web, un port aléatoire est utilisé pour le serveur web. Dans l’image précédente, le numéro de port est 5 000. Quand vous exécutez l’application, vous voyez un autre numéro de port.
* Si vous lancez l’application avec **Ctrl+F5** (mode sans débogage), vous pouvez effectuer des modifications du code, enregistrer le fichier, actualiser le navigateur et afficher les modifications du code. De nombreux développeurs préfèrent utiliser le mode sans débogage pour lancer rapidement l’application et afficher les changements.

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[Suivant : Ajout d’un modèle](xref:tutorials/razor-pages/model)
