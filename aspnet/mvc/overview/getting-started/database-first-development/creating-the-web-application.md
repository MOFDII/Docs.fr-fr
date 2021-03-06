---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'EF Database First avec ASP.NET MVC : création de l’Application Web et les modèles de données | Microsoft Docs'
author: Rick-Anderson
description: À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Ce didacticiel seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 6679b61326bd016481d96a4b5d58ec006f86b633
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020795"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a>EF Database First avec ASP.NET MVC : création de l’Application Web et les modèles de données
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données. Le code généré correspond aux colonnes dans la table de base de données.
> 
> Cette partie de la série se concentre sur la création de l’application web et la génération de modèles de données basés sur vos tables de base de données.


## <a name="create-a-new-aspnet-web-application"></a>Créez une Application Web ASP.NET

Dans une nouvelle solution ou la même solution que le projet de base de données, créez un nouveau projet dans Visual Studio et sélectionnez le **Application Web ASP.NET** modèle. Nommez le projet **ContosoSite**.

![créer le projet](creating-the-web-application/_static/image1.png)

Cliquez sur **OK**.

Dans la fenêtre Nouveau projet ASP.NET, sélectionnez le **MVC** modèle. Vous pouvez effacer le **hôte dans le cloud** option pour l’instant, car vous allez déployer l’application vers le cloud plus tard. Cliquez sur **OK** pour créer l’application.

![Sélectionnez le modèle mvc](creating-the-web-application/_static/image2.png)

Le projet est créé avec les fichiers par défaut et les dossiers.

Dans ce didacticiel, vous allez utiliser Entity Framework 6. Vous pouvez vérifier la version d’Entity Framework dans votre projet via la fenêtre Gérer les Packages NuGet. Si nécessaire, mettez à jour votre version d’Entity Framework.

![Affiche la version](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Générer les modèles

Vous allez maintenant créer des modèles Entity Framework à partir des tables de base de données. Ces modèles sont des classes que vous allez utiliser pour travailler avec les données. Chaque modèle reflète une table dans la base de données et contient des propriétés qui correspondent aux colonnes dans la table.

Avec le bouton droit le **modèles** dossier, puis sélectionnez **ajouter** et **un nouvel élément**.

![Ajouter un nouvel élément](creating-the-web-application/_static/image4.png)

Dans la fenêtre Ajouter un nouvel élément, sélectionnez **données** dans le volet gauche et **ADO.NET Entity Data Model** parmi les options dans le volet central. Nommez le nouveau fichier de modèle **ContosoModel**.

![créer le modèle](creating-the-web-application/_static/image5.png)

Cliquez sur **Ajouter**.

Dans l’Assistant Entity Data Model, sélectionnez **Entity Framework Designer à partir de la base de données**.

![générer à partir de la base de données](creating-the-web-application/_static/image6.png)

Cliquez sur **Suivant**.

Si vous avez des connexions de base de données définies dans votre environnement de développement, vous pouvez voir une de ces connexions présélectionnées. Toutefois, vous souhaitez créer une nouvelle connexion à la base de données que vous avez créé dans la première partie de ce didacticiel. Cliquez sur le **nouvelle connexion** bouton.

![se connecter à la base de données](creating-the-web-application/_static/image7.png)

Dans la fenêtre Propriétés de connexion, indiquez le nom du serveur local où votre base de données a été créé (dans ce cas **(localdb) \ProjectsV12**). Après avoir entré le nom du serveur, sélectionnez le ContosoUniversityData dans les bases de données disponibles.

![définir les propriétés de connexion](creating-the-web-application/_static/image8.png)

Cliquez sur **OK**.

Les propriétés de connexion correctes sont maintenant affichées. Vous pouvez utiliser le nom par défaut pour la connexion dans le fichier Web.Config

![paramètres de connexion](creating-the-web-application/_static/image9.png)

Cliquez sur **Suivant**.

Sélectionnez **Tables** pour générer des modèles pour les trois tables.

![sélectionner des tables](creating-the-web-application/_static/image10.png)

Cliquez sur **Terminer**.

Si vous recevez un avertissement de sécurité, sélectionnez **OK** pour continuer l’exécution du modèle.

Les modèles sont générés à partir des tables de base de données, et un diagramme s’affiche et montre les propriétés et les relations entre les tables.

![diagramme du modèle](creating-the-web-application/_static/image11.png)

Le dossier de modèles inclut désormais les nombreux nouveaux fichiers associés pour les modèles qui ont été générés à partir de la base de données.

![afficher les nouveaux fichiers de modèle](creating-the-web-application/_static/image12.png)

Le **ContosoModel.Context.cs** fichier contient une classe qui dérive de la **DbContext** classe et fournit une propriété pour chaque classe de modèle qui correspond à une table de base de données. Le **Course.cs**, **Enrollment.cs**, et **Student.cs** fichiers contiennent les classes de modèle qui représentent les tables de bases de données. Lorsque vous travaillez avec la structure, vous allez utiliser la classe de contexte et les classes de modèle.

Avant de poursuivre ce didacticiel, générez le projet. Dans la section suivante, vous allez générer du code basé sur les modèles de données, mais cette section ne fonctionnera pas si le projet n’a pas été créé.

> [!div class="step-by-step"]
> [Précédent](setting-up-database.md)
> [Suivant](generating-views.md)
