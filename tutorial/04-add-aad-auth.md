<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="bc37c-101">Dans cet exercice, vous allez étendre l’application de l’exercice précédent pour prendre en charge l’authentification avec Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bc37c-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="bc37c-102">Cela est nécessaire pour obtenir le jeton d’accès OAuth nécessaire pour appeler Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="bc37c-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="bc37c-103">Dans cette étape, vous allez intégrer la [bibliothèque d’authentification Microsoft (MSAL) pour .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) dans l’application.</span><span class="sxs-lookup"><span data-stu-id="bc37c-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) into the application.</span></span>

<span data-ttu-id="bc37c-104">Créez un fichier dans le répertoire **GraphTutorial** nommé **appSettings. JSON**.</span><span class="sxs-lookup"><span data-stu-id="bc37c-104">Create a new file in the **GraphTutorial** directory named **appsettings.json**.</span></span> <span data-ttu-id="bc37c-105">Ajoutez le texte suivant dans ce fichier.</span><span class="sxs-lookup"><span data-stu-id="bc37c-105">Add the following text in that file.</span></span>

```json
{
  "appId": "YOUR_APP_ID_HERE",
  "scopes": [
    "User.Read",
    "Calendars.Read"
  ]
}
```

<span data-ttu-id="bc37c-106">Remplacez `YOUR_APP_ID_HERE` par l’ID d’application que vous avez créé dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="bc37c-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bc37c-107">Si vous utilisez un contrôle de code source tel que git, il est maintenant recommandé d’exclure le fichier **appSettings. JSON** du contrôle de code source afin d’éviter une fuite accidentelle de votre ID d’application.</span><span class="sxs-lookup"><span data-stu-id="bc37c-107">If you're using source control such as git, now would be a good time to exclude the **appsettings.json** file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="bc37c-108">Mettre en œuvre la connexion</span><span class="sxs-lookup"><span data-stu-id="bc37c-108">Implement sign-in</span></span>

<span data-ttu-id="bc37c-109">Dans cette section, vous allez créer un fournisseur d’authentification qui peut être utilisé avec le kit de développement logiciel (SDK) Graph et qui peut également être utilisé pour demander explicitement un jeton d’accès à l’aide du [flux de code du périphérique](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span><span class="sxs-lookup"><span data-stu-id="bc37c-109">In this section you will create an authentication provider that can be used with the Graph SDK and can also be used to explicitly request an access token by using the [device code flow](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span></span>

### <a name="create-an-authentication-provider"></a><span data-ttu-id="bc37c-110">Créer un fournisseur d’authentification</span><span class="sxs-lookup"><span data-stu-id="bc37c-110">Create an authentication provider</span></span>

1. <span data-ttu-id="bc37c-111">Créez un répertoire dans le répertoire **GraphTutorial** nommé **Authentication**.</span><span class="sxs-lookup"><span data-stu-id="bc37c-111">Create a new directory in the **GraphTutorial** directory named **Authentication**.</span></span>
1. <span data-ttu-id="bc37c-112">Créez un fichier dans le répertoire **d’authentification** nommé **DeviceCodeAuthProvider.cs** et ajoutez le code suivant à ce fichier.</span><span class="sxs-lookup"><span data-stu-id="bc37c-112">Create a new file in the **Authentication** directory named **DeviceCodeAuthProvider.cs** and add the following code to that file.</span></span>

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
                    .WithAuthority(AadAuthorityAudience.AzureAdAndPersonalMicrosoftAccount, true)
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

<span data-ttu-id="bc37c-113">Examinez ce que fait ce code.</span><span class="sxs-lookup"><span data-stu-id="bc37c-113">Consider what this code does.</span></span>

- <span data-ttu-id="bc37c-114">Il utilise l’implémentation `IPublicClientApplication` MSAL pour demander et gérer des jetons.</span><span class="sxs-lookup"><span data-stu-id="bc37c-114">It uses the MSAL `IPublicClientApplication` implementation to request and manage tokens.</span></span>
- <span data-ttu-id="bc37c-115">La `GetAccessToken` fonction :</span><span class="sxs-lookup"><span data-stu-id="bc37c-115">The `GetAccessToken` function:</span></span>
  - <span data-ttu-id="bc37c-116">Se connecte à l’utilisateur s’il n’est pas déjà connecté à l’aide du flux de code du périphérique.</span><span class="sxs-lookup"><span data-stu-id="bc37c-116">Signs in the user if they are not already signed in using the device code flow.</span></span>
  - <span data-ttu-id="bc37c-117">Garantit que le jeton renvoyé est toujours actualisé à l’aide `AcquireTokenSilent` de la fonction, qui renvoie le jeton mis en cache s’il n’a pas expiré et actualise le jeton s’il a expiré.</span><span class="sxs-lookup"><span data-stu-id="bc37c-117">Ensures that the token returned is always fresh by using the `AcquireTokenSilent` function, which returns the cached token if it not expired, and refreshes the token if it is expired.</span></span>
- <span data-ttu-id="bc37c-118">Elle implémente l' `IAuthenticationProvider` interface de manière à ce que le kit de développement logiciel Graph puisse utiliser la classe pour authentifier les appels Graph.</span><span class="sxs-lookup"><span data-stu-id="bc37c-118">It implements the `IAuthenticationProvider` interface so that the Graph SDK can use the class to authenticate Graph calls.</span></span>

## <a name="sign-in-and-display-the-access-token"></a><span data-ttu-id="bc37c-119">Se connecter et afficher le jeton d’accès</span><span class="sxs-lookup"><span data-stu-id="bc37c-119">Sign in and display the access token</span></span>

<span data-ttu-id="bc37c-120">Dans cette section, vous allez mettre à jour l’application `GetAccessToken` pour appeler la fonction, qui se connecte à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="bc37c-120">In this section you will update the application to call the `GetAccessToken` function, which will sign in the user.</span></span> <span data-ttu-id="bc37c-121">Vous ajouterez également du code pour afficher le jeton.</span><span class="sxs-lookup"><span data-stu-id="bc37c-121">You will also add code to display the token.</span></span>

1. <span data-ttu-id="bc37c-122">Ouvrez **Program.cs** et ajoutez les instructions `using` suivantes en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="bc37c-122">Open **Program.cs** and add the following `using` statements to the top of the file.</span></span>

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. <span data-ttu-id="bc37c-123">Ajoutez la fonction suivante à la classe `Program`.</span><span class="sxs-lookup"><span data-stu-id="bc37c-123">Add the following function to the `Program` class.</span></span>

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

1. <span data-ttu-id="bc37c-124">Ajoutez le code suivant à la `Main` fonction immédiatement après la `Console.WriteLine(".NET Core Graph Tutorial\n");` ligne.</span><span class="sxs-lookup"><span data-stu-id="bc37c-124">Add the following code to the `Main` function immediately after the `Console.WriteLine(".NET Core Graph Tutorial\n");` line.</span></span>

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

1. <span data-ttu-id="bc37c-125">Ajoutez le code suivant à la `Main` fonction immédiatement après la `// Display access token` ligne.</span><span class="sxs-lookup"><span data-stu-id="bc37c-125">Add the following code to the `Main` function immediately after the `// Display access token` line.</span></span>

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. <span data-ttu-id="bc37c-126">Générez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="bc37c-126">Build and run the app.</span></span> <span data-ttu-id="bc37c-127">L’application affiche une URL et un code de périphérique.</span><span class="sxs-lookup"><span data-stu-id="bc37c-127">The application displays a URL and device code.</span></span>

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > <span data-ttu-id="bc37c-128">Si vous rencontrez des erreurs, comparez votre **Program.cs** à l' [exemple sur GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demos/01-create-app/GraphTutorial/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="bc37c-128">If you encounter errors, compare your **Program.cs** with the [example on GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demos/01-create-app/GraphTutorial/Program.cs).</span></span>

1. <span data-ttu-id="bc37c-129">Ouvrez un navigateur et accédez à l’URL affichée.</span><span class="sxs-lookup"><span data-stu-id="bc37c-129">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="bc37c-130">Entrez le code fourni et connectez-vous.</span><span class="sxs-lookup"><span data-stu-id="bc37c-130">Enter the provided code and sign in.</span></span> <span data-ttu-id="bc37c-131">Une fois terminé, revenez à l’application et choisissez le **1. Afficher** l’option de jeton d’accès pour afficher le jeton d’accès.</span><span class="sxs-lookup"><span data-stu-id="bc37c-131">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
