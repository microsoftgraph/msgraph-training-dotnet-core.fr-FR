<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez étendre l’application de l’exercice précédent pour prendre en charge l’authentification avec Azure AD. Cela est nécessaire pour obtenir le jeton d’accès OAuth nécessaire pour appeler Microsoft Graph. Dans cette étape, vous allez intégrer la [bibliothèque d’authentification Microsoft (MSAL) pour .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) dans l’application.

Créez un fichier dans le répertoire **GraphTutorial** nommé **appSettings. JSON**. Ajoutez le texte suivant dans ce fichier.

```json
{
  "appId": "YOUR_APP_ID_HERE",
  "scopes": [
    "User.Read",
    "Calendars.Read"
  ]
}
```

Remplacez `YOUR_APP_ID_HERE` par l’ID d’application que vous avez créé dans le portail Azure.

> [!IMPORTANT]
> Si vous utilisez un contrôle de code source tel que git, il est maintenant recommandé d’exclure le fichier **appSettings. JSON** du contrôle de code source afin d’éviter une fuite accidentelle de votre ID d’application.

## <a name="implement-sign-in"></a>Mettre en œuvre la connexion

Dans cette section, vous allez créer un fournisseur d’authentification qui peut être utilisé avec le kit de développement logiciel (SDK) Graph et qui peut également être utilisé pour demander explicitement un jeton d’accès à l’aide du [flux de code du périphérique](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).

### <a name="create-an-authentication-provider"></a>Créer un fournisseur d’authentification

1. Créez un répertoire dans le répertoire **GraphTutorial** nommé **Authentication**.
1. Créez un fichier dans le répertoire **d’authentification** nommé **DeviceCodeAuthProvider.cs** et ajoutez le code suivant à ce fichier.

    ```csharp
    using Microsoft.Graph;
    using Microsoft.Identity.Client;
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace GraphTutorial
    {
        public class DeviceCodeAuthProvider : IAuthenticationProvider
        {
            private IPublicClientApplication _msalClient;
            private string[] _scopes;
            private IAccount _userAccount;

            public DeviceCodeAuthProvider(string appId, string[] scopes)
            {
                _scopes = scopes;

                _msalClient = PublicClientApplicationBuilder
                    .Create(appId)
                    // Set the tenant ID to "organizations" to disable personal accounts
                    // Azure OAuth does not support device code flow for personal accounts
                    // See https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code
                    .WithTenantId("organizations")
                    .Build();
            }

            public async Task<string> GetAccessToken()
            {
                // If there is no saved user account, the user must sign-in
                if (_userAccount == null)
                {
                    try
                    {
                        // Invoke device code flow so user can sign-in with a browser
                        var result = await _msalClient.AcquireTokenWithDeviceCode(_scopes, callback => {
                            Console.WriteLine(callback.Message);
                            return Task.FromResult(0);
                        }).ExecuteAsync();

                        _userAccount = result.Account;
                        return result.AccessToken;
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"Error getting access token: {exception.Message}");
                        return null;
                    }
                }
                else
                {
                    // If there is an account, call AcquireTokenSilent
                    // By doing this, MSAL will refresh the token automatically if
                    // it is expired. Otherwise it returns the cached token.

                        var result = await _msalClient
                            .AcquireTokenSilent(_scopes, _userAccount)
                            .ExecuteAsync();

                       return result.AccessToken;
                }
            }

            // This is the required function to implement IAuthenticationProvider
            // The Graph SDK will call this function each time it makes a Graph
            // call.
            public async Task AuthenticateRequestAsync(HttpRequestMessage requestMessage)
            {
                requestMessage.Headers.Authorization =
                    new AuthenticationHeaderValue("bearer", await GetAccessToken());
            }
        }
    }
    ```

Examinez ce que fait ce code.

- Il utilise l’implémentation `IPublicClientApplication` MSAL pour demander et gérer des jetons.
- La `GetAccessToken` fonction :
  - Se connecte à l’utilisateur s’il n’est pas déjà connecté à l’aide du flux de code du périphérique.
  - Garantit que le jeton renvoyé est toujours actualisé à l’aide `AcquireTokenSilent` de la fonction, qui renvoie le jeton mis en cache s’il n’a pas expiré et actualise le jeton s’il a expiré.
- Elle implémente l' `IAuthenticationProvider` interface de manière à ce que le kit de développement logiciel Graph puisse utiliser la classe pour authentifier les appels Graph.

## <a name="sign-in-and-display-the-access-token"></a>Se connecter et afficher le jeton d’accès

Dans cette section, vous allez mettre à jour l’application `GetAccessToken` pour appeler la fonction, qui se connecte à l’utilisateur. Vous ajouterez également du code pour afficher le jeton.

1. Ouvrez **Program.cs** et ajoutez les instructions `using` suivantes en haut du fichier.

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. Ajoutez la fonction suivante à la classe `Program`.

    ```csharp
    static IConfigurationRoot LoadAppSettings()
    {
        var appConfig = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("appsettings.json", false, true)
            .Build();

        // Check for required settings
        if (string.IsNullOrEmpty(appConfig["appId"]) ||
            // Make sure there's at least one value in the scopes array
            string.IsNullOrEmpty(appConfig["scopes:0"]))
        {
            return null;
        }

        return appConfig;
    }
    ```

1. Ajoutez le code suivant à la `Main` fonction immédiatement après la `Console.WriteLine(".NET Core Graph Tutorial\n");` ligne.

    ```csharp
    var appConfig = LoadAppSettings();

    if (appConfig == null)
    {
        Console.WriteLine("Missing or invalid appsettings.json...exiting");
        return;
    }

    var appId = appConfig["appId"];
    var scopes = appConfig.GetSection("scopes").Get<string[]>();

    // Initialize the auth provider with values from appsettings.json
    var authProvider = new DeviceCodeAuthProvider(appId, scopes);

    // Request a token to sign in the user
    var accessToken = authProvider.GetAccessToken().Result;
    ```

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
    > Si vous rencontrez des erreurs, comparez votre **Program.cs** à l' [exemple sur GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demos/01-create-app/GraphTutorial/Program.cs).

1. Ouvrez un navigateur et accédez à l’URL affichée. Entrez le code fourni et connectez-vous. Une fois terminé, revenez à l’application et choisissez le **1. Afficher** l’option de jeton d’accès pour afficher le jeton d’accès.
