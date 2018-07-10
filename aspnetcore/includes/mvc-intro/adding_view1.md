# <a name="add-a-view-to-an-aspnet-core-mvc-app"></a>Ajouter une vue à une application ASP.NET Core MVC

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Dans cette section, vous modifiez la classe `HelloWorldController` de façon à utiliser les fichiers de modèle de vue Razor pour encapsuler proprement le processus de génération des réponses HTML à un client.

Vous créez un fichier de modèle de vue à l’aide de Razor. Les modèles de vue Razor ont l’extension de fichier *.cshtml*. Ils offrent un moyen élégant pour créer une sortie HTML avec C#.

Actuellement, la méthode `Index` retourne une chaîne avec un message qui est codé en dur dans la classe du contrôleur. Dans la classe `HelloWorldController`, remplacez la méthode `Index` par le code suivant :

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

Le code précédent retourne un objet `View`. Il utilise un modèle de vue pour générer une réponse HTML au navigateur. Les méthodes du contrôleur (également appelées méthodes d’action), comme la méthode `Index` ci-dessus, retournent généralement un [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) (ou une classe dérivée de `ActionResult`), et non une chaîne ou un type similaire.
