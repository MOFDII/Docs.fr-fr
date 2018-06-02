# <a name="prefix-key-vault-configuration-provider-sample-application-aspnet-core-1x"></a>Application d’exemple de fournisseur de Configuration de coffre de clés du préfixe (ASP.NET Core 1.x)

Cet exemple illustre l’utilisation du fournisseur de Configuration Azure Key Vault pour ASP.NET Core 1.x à l’aide de préfixes d’espaces de nom de la clé. Pour l’exemple 2.x ASP.NET Core, consultez [application d’exemple de fournisseur de Configuration du coffre de clé du préfixe (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x).

> [!NOTE]
> Le fournisseur de configuration n’est pas disponible pour la version 1.0 de ASP.NET Core. Si vous souhaitez implémenter le fournisseur de configuration et l’application est une application ASP.NET Core 1.0, mettez à niveau l’application d’abord 1.1 ou version ultérieure.

Pour plus d’informations sur le fonctionne de l’exemple, consultez la [fournisseur de configuration d’Azure Key Vault](xref:security/key-vault-configuration) rubrique.

## <a name="using-the-sample"></a>Utilisation de l'exemple
1. Créer un coffre de clés et configurer Azure Active Directory (Azure AD) pour l’application en suivant les instructions de [prise en main d’Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Ajouter des clés secrètes dans le coffre de clés à l’aide du PowerShell Module de Azure, l’API de gestion Azure ou le portail Azure. Les clés secrètes créées sont de type *Manual* ou *Certificate*. Les clés secrètes *Certificate* sont des certificats pour les applications et les services, mais elles ne sont pas prises en charge par le fournisseur de configuration. Vous devez utiliser l'option *Manual* afin de créer des clés secrètes de paire nom-valeur pour le fournisseur de configuration.
     * Pour des valeurs hiérarchiques (sections de configuration), utilisez `--` (deux tirets) comme séparateur.
     * Pour l’exemple d’application, créez deux *manuel* secrets avec les paires nom-valeur suivantes :
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Inscrire l’exemple d’application dans Azure Active Directory.
   * Autoriser l’application à accéder au coffre de clés. Lorsque vous utilisez l’applet de commande PowerShell `Set-AzureRmKeyVaultAccessPolicy` pour autoriser l’application à accéder au coffre de clés, vous pouvez obtenir la liste des clés secrètes avec `List` et `Get` avec `-PermissionsToSecrets list,get`.

2. Mettre à jour le fichier *appsettings.json* de l’application avec les valeurs de `Vault`, `ClientId`, et `ClientSecret`.
3. Exécutez l’exemple d’application, qui obtient ses valeurs de configuration à partir de `IConfigurationRoot` avec le même nom que le nom secret. Dans cet exemple, le préfixe est la version de l’application que vous avez fournie pour `PrefixKeyVaultSecretManager` quand vous avez ajouté le fournisseur de configuration du coffre de clés Azure. La valeur de `AppSecret` est obtenu avec `config["AppSecret"]`.
4. Modifier la version de l’assembly de l’application dans le fichier de projet à partir de `5.0.0.0` à `5.1.0.0` et réexécutez l’application. Cette fois, la valeur secrète retournée est `5.1.0.0_secret_value`.
