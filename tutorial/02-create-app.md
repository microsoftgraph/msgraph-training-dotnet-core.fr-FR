<!-- markdownlint-disable MD002 MD041 -->

Commencez par créer un projet de console principale .NET à l’aide de l' [interface CLI .net Core](/dotnet/core/tools/?tabs=netcore2x).

1. Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet. Exécutez la commande suivante.

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. Une fois le projet créé, vérifiez qu’il fonctionne en remplaçant le répertoire actuel par le répertoire **GraphTutorial** et en exécutant la commande suivante dans votre CLI.

    ```Shell
    dotnet run
    ```

    Si elle fonctionne, l’application doit produire `Hello World!`une sortie.

## <a name="install-dependencies"></a>Installer les dépendances

Avant de poursuivre, ajoutez des dépendances supplémentaires que vous utiliserez plus tard.

- [Microsoft. extensions. Configuration. UserSecrets](https://github.com/aspnet/extensions) pour lire la configuration de l’application à partir du [magasin de secrets de développement .net](https://docs.microsoft.com/aspnet/core/security/app-secrets).
- [Bibliothèque d’authentification Microsoft (MSAL) pour .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) pour authentifier l’utilisateur et acquérir des jetons d’accès.
- [Bibliothèque cliente .NET Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) pour effectuer des appels à Microsoft Graph.

Exécutez les commandes suivantes dans votre interface CLI pour installer les dépendances.

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 3.1.0
dotnet add package Microsoft.Identity.Client --version 4.7.1
dotnet add package Microsoft.Graph --version 1.21.0
```

## <a name="design-the-app"></a>Concevoir l’application

Dans cette section, vous allez créer un menu simple basé sur la console.

Ouvrez **Program.cs** dans un éditeur de texte (tel que [Visual Studio code](https://code.visualstudio.com/)) et remplacez l’intégralité de son contenu par le code suivant.

[!code-csharp[](../demos/01-create-app/GraphTutorial/Program.cs)]

Cela implémente un menu de base et lit le choix de l’utilisateur à partir de la ligne de commande.
