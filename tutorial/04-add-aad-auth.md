<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez étendre l’application de l’exercice précédent pour prendre en charge l’authentification avec Azure AD. Cela est nécessaire pour obtenir le jeton d’accès OAuth nécessaire pour appeler Microsoft Graph. Dans cette étape, vous allez intégrer la [bibliothèque d’authentification Microsoft (MSAL) pour .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) dans l’application.

1. Initialisez le [magasin de secrets de développement .net](/aspnet/core/security/app-secrets) en ouvrant votre interface CLI dans le répertoire qui contient **GraphTutorial. csproj** et en exécutant la commande suivante.

    ```Shell
    dotnet user-secrets init
    ```

1. Ajoutez votre ID d’application et une liste d’étendues requises au magasin secret à l’aide des commandes suivantes. Remplacez `YOUR_APP_ID_HERE` par l’ID d’application que vous avez créé dans le portail Azure.

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;MailboxSettings.Read;Calendars.ReadWrite"
    ```

    Examinons les étendues d’autorisation que vous venez de définir.

    - **User. Read** permet à l’application de lire le profil de l’utilisateur connecté pour obtenir des informations telles que le nom d’affichage et l’adresse de messagerie.
    - **MailboxSettings. Read** permet à l’application de lire le fuseau horaire, le format de la date et le format d’heure préférés de l’utilisateur.
    - **Calendars. ReadWrite** permet à l’application de lire les événements existants sur le calendrier de l’utilisateur et d’ajouter de nouveaux événements.

## <a name="implement-sign-in"></a>Implémentation de la connexion

Dans cette section, vous allez créer un fournisseur d’authentification qui peut être utilisé avec le kit de développement logiciel (SDK) Graph et qui peut également être utilisé pour demander explicitement un jeton d’accès à l’aide du [flux de code du périphérique](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).

### <a name="create-an-authentication-provider"></a>Créer un fournisseur d’authentification

1. Créez un répertoire dans le répertoire **GraphTutorial** nommé **Authentication**.
1. Créez un fichier dans le répertoire **d’authentification** nommé **DeviceCodeAuthProvider.cs** et ajoutez le code suivant à ce fichier.

    :::code language="csharp" source="../demo/GraphTutorial/Authentication/DeviceCodeAuthProvider.cs" id="AuthProviderSnippet":::

Examinez ce que fait ce code.

- Il utilise l' `IPublicClientApplication` implémentation MSAL pour demander et gérer des jetons.
- La `GetAccessToken` fonction :
  - Se connecte à l’utilisateur s’il n’est pas déjà connecté à l’aide du flux de code du périphérique.
  - Garantit que le jeton renvoyé est toujours actualisé à l’aide de la `AcquireTokenSilent` fonction, qui renvoie le jeton mis en cache s’il n’a pas expiré et actualise le jeton s’il a expiré.
- Elle implémente l' `IAuthenticationProvider` interface de manière à ce que le kit de développement logiciel Graph puisse utiliser la classe pour authentifier les appels Graph.

## <a name="sign-in-and-display-the-access-token"></a>Se connecter et afficher le jeton d’accès

Dans cette section, vous allez mettre à jour l’application pour appeler la `GetAccessToken` fonction, qui se connecte à l’utilisateur. Vous ajouterez également du code pour afficher le jeton.

1. Ajoutez la fonction suivante à la classe `Program`.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="LoadAppSettingsSnippet":::

1. Ajoutez le code suivant à la `Main` fonction immédiatement après la `Console.WriteLine(".NET Core Graph Tutorial\n");` ligne.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="InitializationSnippet":::

1. Ajoutez le code suivant à la `Main` fonction immédiatement après la `// Display access token` ligne.

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. Générez et exécutez l’application. L’application affiche une URL et un code de périphérique.

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > Si vous rencontrez des erreurs, comparez votre **Program.cs** à l' [exemple sur GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).

1. Ouvrez un navigateur et accédez à l’URL affichée. Entrez le code fourni et connectez-vous. Une fois terminé, revenez à l’application et choisissez le **1. Afficher** l’option de jeton d’accès pour afficher le jeton d’accès.
