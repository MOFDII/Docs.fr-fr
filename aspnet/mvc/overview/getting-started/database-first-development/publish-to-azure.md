---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Publier le site MVC Database First sur Azure | Microsoft Docs
author: tfitzmac
description: À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Ce didacticiel seri...
ms.author: riande
ms.date: 12/22/2014
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 45dd2c127e3ba0644e8168e293006fa9eadd776d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823689"
---
<a name="publish-mvc-database-first-site-to-azure"></a>Publier le site MVC Database First sur Azure
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> À l’aide de la structure ASP.NET MVC et Entity Framework, vous pouvez créer une application web qui fournit une interface à une base de données existante. Cette série de didacticiels vous montre comment générer du code qui permet aux utilisateurs d’afficher, modifier, créer et supprimer automatiquement les données qui résident dans une table de base de données. Le code généré correspond aux colonnes dans la table de base de données.
> 
> Cette partie de la série se concentre sur la publication de l’application web et la base de données sur Azure. Vous pouvez lire cette rubrique pour en savoir plus sur la publication d’une application web et la base de données, mais pour effectuer les étapes que vous devez commencer au début du didacticiel. Consultez [mise en route](setting-up-database.md).


## <a name="deploy-your-web-app-on-azure"></a>Déployer votre application web sur Azure

Vous avez besoin d’un compte Azure pour suivre ce didacticiel :

- Vous pouvez [ouvrir un compte Azure gratuit](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -vous bénéficiez de crédits que vous pouvez utiliser pour tester les services Azure payants et même lorsqu’ils sont épuisés, vous pouvez conserver le compte et utiliser les services Azure gratuits.
- Vous pouvez [activer les avantages d’abonnement MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -votre abonnement MSDN vous donne des crédits chaque mois que vous pouvez utiliser pour les services Azure payants.

Pour publier votre application web, cliquez sur le projet et sélectionnez **publier**.

![Publier le site](publish-to-azure/_static/image1.png)

Sélectionnez les sites Web Microsoft Azure.

![Sélectionnez Azure](publish-to-azure/_static/image2.png)

Si vous n’êtes pas connecté Azure, fournissez vos informations d’identification de compte Azure. Ensuite, sélectionnez Nouveau pour créer une nouvelle application web.

![nouveau site](publish-to-azure/_static/image3.png)

Créer un nom unique pour votre application web. Vous saurez que le nom est unique si vous voyez une coche verte à droite du champ name. Sélectionnez une région pour votre application web. Sélectionnez **créer un serveur** pour la base de données et fournir un nom d’utilisateur et le mot de passe pour ce nouveau serveur de base de données. Lorsque vous avez terminé, cliquez sur **créer**.

![créer un site](publish-to-azure/_static/image4.png)

Vos valeurs de connexion sont maintenant prêt. Vous pouvez laisser ces valeurs inchangées.

![connection](publish-to-azure/_static/image5.png)

Cliquez sur **Suivant**.

Pour les paramètres, notez que deux connexions de base de données sont spécifiés - ApplicationDBContext et ContosoUniversityDataEntities. ApplicationDBContext correspond à la connexion pour les tables de compte d’utilisateur. Ces valeurs indiquent uniquement les chaînes de connexion pour les bases de données. Cela ne signifie pas que ces bases de données seront publiées lorsque vous publiez votre site. Vous allez publier votre projet de base de données une fois que vous avez terminé la publication de l’application web.

![](publish-to-azure/_static/image6.png)

Les points de suspension (...) en regard de la connexion de base de données affiche les détails de la chaîne de connexion. Cliquez sur les points de suspension en regard de ContosoUniversityDataEntities.

![](publish-to-azure/_static/image7.png)

Notez le nom du serveur de base de données et la base de données. Le nom du serveur généré de façon aléatoire. Le nom de la base de données est simple le nom de votre site avec  **\_db** ajouté. Vous aurez besoin plus tard les deux noms lorsque vous publiez votre base de données.

Cliquez sur **OK** pour fermer la fenêtre de chaîne de connexion de base de données.

Dans la fenêtre publier le site Web, cliquez sur **suivant** pour afficher l’aperçu.

![](publish-to-azure/_static/image8.png)

Vous pouvez cliquer sur Démarrer l’aperçu pour afficher la liste des fichiers à publier. Dans la mesure où il s’agit de la première fois que vous avez publié ce site, la liste est tous les fichiers pertinents dans le projet.

Cliquez sur **Publier**.

Le volet de sortie affiche le résultat de votre publication.

![](publish-to-azure/_static/image9.png)

Après la publication, le site est immedialely lancé dans un navigateur web. Votre site a été déployé et vous pouvez inscrire un nouvel utilisateur sur le site ; Toutefois, vos tables dans le projet ContosoUniversityData n’ont pas encore été publiées. Si vous cliquez sur la liste des étudiants lien, vous recevrez une erreur.

## <a name="publish-database-to-sql-azure"></a>Publier la base de données pour SQL Azure

Avant de publier votre base de données, il se peut que vous devez vous assurer que votre ordinateur local peut se connecter au serveur de base de données. Le pare-feu pour votre serveur de base de données restreint qui machines peuvent se connecter à la base de données. Vous devez ajouter l’adresse IP de votre ordinateur pour les adresses IP autorisées pour le pare-feu.

Connectez-vous à votre compte Azure via le portail Azure.

Sélectionnez votre nouvelle base de données, puis sélectionnez **gérer**.

![gérer](publish-to-azure/_static/image10.png)

Vous devez configurer votre serveur de base de données pour autoriser les connexions à partir de votre ordinateur. Lorsque vous sélectionnez Gérer, vous êtes invité à ajouter l’adresse IP actuelle comme autorisée pour le serveur de base de données. Sélectionnez Oui.

![Ajouter une adresse ip](publish-to-azure/_static/image11.png)

Il est probable que l’adresse IP que vous avez ajouté à l’étape précédente n’est pas la seule adresse IP, que vous devez configurer pour les connexions. Vous pouvez tenter de se connecter à la base de données pour voir si les connexions ont été configurées correctement. Fournir l’utilisateur et le mot de passe que vous avez créée.

![connexion](publish-to-azure/_static/image12.png)

Si vous recevez un message d’erreur, vous devez ajouter une autre adresse IP. Cliquez sur le message d’erreur pour afficher plus de détails sur l’erreur. Dans les détails, vous verrez l’adresse IP que vous devez ajouter. Notez cette adresse IP.

![non autorisé](publish-to-azure/_static/image13.png)

Fermer cette fenêtre de connexion, revenez au portail Azure.

Accédez au tableau de bord pour votre base de données. Cliquez sur **gérer les adresses IP autorisée**.

![gérer les adresses ip](publish-to-azure/_static/image14.png)

Vous devez maintenant ajouter l’adresse IP du message d’erreur. Modifier la plage d’adresses IP autorisées à inclure de celui du message d’erreur, soit ajouter cette adresse IP comme une entrée distincte.

![Ajouter une nouvelle adresse](publish-to-azure/_static/image15.png)

Enregistrez la modification à des adresses IP autorisées.

Cliquez sur Gérer et essayez de vous connecter à nouveau à la base de données. Vous devrez peut-être attendre quelques minutes avant que les adresses IP autorisées sont correctement configurés pour le pare-feu. Lorsque vous pouvez vous connecter avec succès dans la base de données, vous avez fini de configurer votre connexion à la base de données.

Vous pouvez laisser cette fenêtre de gestion ouverte, car vous allez vérifier le résultat de votre déploiement de base de données peu de temps.

Revenez à votre projet de base de données. Cliquez sur le projet et sélectionnez **publier**.

![publier la base de données](publish-to-azure/_static/image16.png)

Dans la fenêtre de la base de données de publication, sélectionnez **modifier**.

![modifier](publish-to-azure/_static/image17.png)

Indiquez le nom du serveur de base de données et vos informations d’identification d’authentification pour le serveur. Après avoir entré les informations d’identification, sélectionnez la base de données que vous avez créé dans la liste des bases de données disponibles. Par défaut, Visual Studio définit le nom du champ de base de données sur le nom de votre projet qui ne peut pas être identique à la base de données que vous avez créé.

![](publish-to-azure/_static/image18.png)

Cliquez sur OK.

Vous souhaiterez probablement enregistrer ce profil pour pouvoir publier des mises à jour à l’avenir sans entrer à nouveau toutes les informations de connexion. Sélectionnez **Créer un profil**.

![enregistrer le profil](publish-to-azure/_static/image19.png)

Il créera un fichier dans votre projet nommé **ContosoUniversityData.publish.xml**. La prochaine fois que vous souhaitez publier la base de données vers Azure, il suffit de recharger ce profil.

Maintenant, cliquez sur **publier** pour créer la base de données sur Azure.

![publish](publish-to-azure/_static/image20.png)

Après l’exécution pendant un certain temps, les publication des résultats sont affichés.

![résultats](publish-to-azure/_static/image21.png)

Maintenant, vous pouvez revenir au portail de gestion pour votre base de données. Actualiser l’affichage de conception et notez que 3 tables avec des données préremplies ont été déployés.

![nouvelles tables](publish-to-azure/_static/image22.png)

Vous êtes maintenant prêt à tester l’application web qui est déployée sur Azure. Accédez à l’application web sur Azure (tel que http://contosositeexample.azurewebsites.net/). Cliquez sur le lien pour la liste des étudiants et vous devriez voir la vue index pour les étudiants.

![vue](publish-to-azure/_static/image23.png)

Parfois, la base de données et la connexion besoin un peu de temps pour être correctement configurés. Si vous recevez une erreur, patientez quelques minutes et réessayez.

## <a name="conclusion"></a>Conclusion

Cette série fourni un exemple simple de la génération de code à partir d’une base de données existante qui permet aux utilisateurs de modifier, mettre à jour, créer et supprimer des données. Il utilisé ASP.NET MVC 5, Entity Framework et génération de modèles automatique ASP.NET pour créer le projet.

Pour obtenir un exemple d’introduction du développement Code First, consultez [mise en route avec ASP.NET MVC 5](../introduction/getting-started.md).

Pour un exemple plus avancé, consultez [création d’un modèle de données Entity Framework pour une application ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Notez que l’API DbContext que vous utilisez pour l’utilisation des données dans la première base de données est identique à l’API que vous utilisez pour travailler avec des données dans le Code First. Même si vous envisagez d’utiliser la première base de données, vous pouvez apprendre à gérer des scénarios plus complexes tels que la lecture et la mise à jour des données connexes, gestion des conflits d’accès concurrentiel, et ainsi de suite à partir d’un didacticiel de Code First. La seule différence est dans la base de données, classe de contexte, les classes d’entité sont créés.

> [!div class="step-by-step"]
> [Précédent](enhancing-data-validation.md)
