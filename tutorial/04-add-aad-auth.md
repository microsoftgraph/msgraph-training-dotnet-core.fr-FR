<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="2f3f8-101">Dans cet exercice, vous allez étendre l’application de l’exercice précédent pour prendre en charge l’authentification avec Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="2f3f8-102">Cela est nécessaire pour obtenir le jeton d’accès OAuth nécessaire pour appeler Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="2f3f8-103">Dans cette étape, vous allez intégrer la [bibliothèque d’authentification Microsoft (MSAL) pour .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) dans l’application.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) into the application.</span></span>

1. <span data-ttu-id="2f3f8-104">Initialisez le [magasin de secrets de développement .net](/aspnet/core/security/app-secrets) en ouvrant votre interface CLI dans le répertoire qui contient **GraphTutorial. csproj** et en exécutant la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-104">Initialize the [.NET development secret store](/aspnet/core/security/app-secrets) by opening your CLI in the directory that contains **GraphTutorial.csproj** and running the following command.</span></span>

    ```Shell
    dotnet user-secrets init
    ```

1. <span data-ttu-id="2f3f8-105">Ajoutez votre ID d’application et une liste d’étendues requises au magasin secret à l’aide des commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-105">Add your application ID and a list of required scopes to the secret store using the following commands.</span></span> <span data-ttu-id="2f3f8-106">Remplacez `YOUR_APP_ID_HERE` par l’ID d’application que vous avez créé dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;Calendars.Read"
    ```

## <a name="implement-sign-in"></a><span data-ttu-id="2f3f8-107">Implémentation de la connexion</span><span class="sxs-lookup"><span data-stu-id="2f3f8-107">Implement sign-in</span></span>

<span data-ttu-id="2f3f8-108">Dans cette section, vous allez créer un fournisseur d’authentification qui peut être utilisé avec le kit de développement logiciel (SDK) Graph et qui peut également être utilisé pour demander explicitement un jeton d’accès à l’aide du [flux de code du périphérique](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span><span class="sxs-lookup"><span data-stu-id="2f3f8-108">In this section you will create an authentication provider that can be used with the Graph SDK and can also be used to explicitly request an access token by using the [device code flow](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code).</span></span>

### <a name="create-an-authentication-provider"></a><span data-ttu-id="2f3f8-109">Créer un fournisseur d’authentification</span><span class="sxs-lookup"><span data-stu-id="2f3f8-109">Create an authentication provider</span></span>

1. <span data-ttu-id="2f3f8-110">Créez un répertoire dans le répertoire **GraphTutorial** nommé **Authentication**.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-110">Create a new directory in the **GraphTutorial** directory named **Authentication**.</span></span>
1. <span data-ttu-id="2f3f8-111">Créez un fichier dans le répertoire **d’authentification** nommé **DeviceCodeAuthProvider.cs** et ajoutez le code suivant à ce fichier.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-111">Create a new file in the **Authentication** directory named **DeviceCodeAuthProvider.cs** and add the following code to that file.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Authentication/DeviceCodeAuthProvider.cs" id="AuthProviderSnippet":::

<span data-ttu-id="2f3f8-112">Examinez ce que fait ce code.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-112">Consider what this code does.</span></span>

- <span data-ttu-id="2f3f8-113">Il utilise l’implémentation `IPublicClientApplication` MSAL pour demander et gérer des jetons.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-113">It uses the MSAL `IPublicClientApplication` implementation to request and manage tokens.</span></span>
- <span data-ttu-id="2f3f8-114">La `GetAccessToken` fonction :</span><span class="sxs-lookup"><span data-stu-id="2f3f8-114">The `GetAccessToken` function:</span></span>
  - <span data-ttu-id="2f3f8-115">Se connecte à l’utilisateur s’il n’est pas déjà connecté à l’aide du flux de code du périphérique.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-115">Signs in the user if they are not already signed in using the device code flow.</span></span>
  - <span data-ttu-id="2f3f8-116">Garantit que le jeton renvoyé est toujours actualisé à l’aide `AcquireTokenSilent` de la fonction, qui renvoie le jeton mis en cache s’il n’a pas expiré et actualise le jeton s’il a expiré.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-116">Ensures that the token returned is always fresh by using the `AcquireTokenSilent` function, which returns the cached token if it not expired, and refreshes the token if it is expired.</span></span>
- <span data-ttu-id="2f3f8-117">Elle implémente l' `IAuthenticationProvider` interface de manière à ce que le kit de développement logiciel Graph puisse utiliser la classe pour authentifier les appels Graph.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-117">It implements the `IAuthenticationProvider` interface so that the Graph SDK can use the class to authenticate Graph calls.</span></span>

## <a name="sign-in-and-display-the-access-token"></a><span data-ttu-id="2f3f8-118">Se connecter et afficher le jeton d’accès</span><span class="sxs-lookup"><span data-stu-id="2f3f8-118">Sign in and display the access token</span></span>

<span data-ttu-id="2f3f8-119">Dans cette section, vous allez mettre à jour l’application `GetAccessToken` pour appeler la fonction, qui se connecte à l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-119">In this section you will update the application to call the `GetAccessToken` function, which will sign in the user.</span></span> <span data-ttu-id="2f3f8-120">Vous ajouterez également du code pour afficher le jeton.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-120">You will also add code to display the token.</span></span>

1. <span data-ttu-id="2f3f8-121">Ajoutez la fonction suivante à la classe `Program`.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-121">Add the following function to the `Program` class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="LoadAppSettingsSnippet":::

1. <span data-ttu-id="2f3f8-122">Ajoutez le code suivant à la `Main` fonction immédiatement après la `Console.WriteLine(".NET Core Graph Tutorial\n");` ligne.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-122">Add the following code to the `Main` function immediately after the `Console.WriteLine(".NET Core Graph Tutorial\n");` line.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="InitializationSnippet":::

1. <span data-ttu-id="2f3f8-123">Ajoutez le code suivant à la `Main` fonction immédiatement après la `// Display access token` ligne.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-123">Add the following code to the `Main` function immediately after the `// Display access token` line.</span></span>

    ```csharp
    Console.WriteLine($"Access token: {accessToken}\n");
    ```

1. <span data-ttu-id="2f3f8-124">Générez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-124">Build and run the app.</span></span> <span data-ttu-id="2f3f8-125">L’application affiche une URL et un code de périphérique.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-125">The application displays a URL and device code.</span></span>

    ```Shell
    PS C:\Source\GraphTutorial> dotnet run
    .NET Core Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

    > [!TIP]
    > <span data-ttu-id="2f3f8-126">Si vous rencontrez des erreurs, comparez votre **Program.cs** à l' [exemple sur GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="2f3f8-126">If you encounter errors, compare your **Program.cs** with the [example on GitHub](https://github.com/microsoftgraph/msgraph-training-dotnet-core/blob/master/demo/GraphTutorial/Program.cs).</span></span>

1. <span data-ttu-id="2f3f8-127">Ouvrez un navigateur et accédez à l’URL affichée.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-127">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="2f3f8-128">Entrez le code fourni et connectez-vous.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-128">Enter the provided code and sign in.</span></span> <span data-ttu-id="2f3f8-129">Une fois terminé, revenez à l’application et choisissez le **1. Afficher** l’option de jeton d’accès pour afficher le jeton d’accès.</span><span class="sxs-lookup"><span data-stu-id="2f3f8-129">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
