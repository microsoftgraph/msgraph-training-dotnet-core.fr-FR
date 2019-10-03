<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c4f8e-101">Commencez par créer un projet de console principale .NET à l’aide de l' [interface CLI .net Core](/dotnet/core/tools/?tabs=netcore2x).</span><span class="sxs-lookup"><span data-stu-id="c4f8e-101">Begin by creating a new .NET Core console project using the [.NET Core CLI](/dotnet/core/tools/?tabs=netcore2x).</span></span>

1. <span data-ttu-id="c4f8e-102">Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet.</span><span class="sxs-lookup"><span data-stu-id="c4f8e-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="c4f8e-103">Exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="c4f8e-103">Run the following command.</span></span>

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. <span data-ttu-id="c4f8e-104">Une fois le projet créé, vérifiez qu’il fonctionne en remplaçant le répertoire actuel par le répertoire **GraphTutorial** et en exécutant la commande suivante dans votre CLI.</span><span class="sxs-lookup"><span data-stu-id="c4f8e-104">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

    <span data-ttu-id="c4f8e-105">Si elle fonctionne, l’application doit produire `Hello World!`une sortie.</span><span class="sxs-lookup"><span data-stu-id="c4f8e-105">If it works, the app should output `Hello World!`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="c4f8e-106">Installer les dépendances</span><span class="sxs-lookup"><span data-stu-id="c4f8e-106">Install dependencies</span></span>

<span data-ttu-id="c4f8e-107">Avant de poursuivre, ajoutez des dépendances supplémentaires que vous utiliserez plus tard.</span><span class="sxs-lookup"><span data-stu-id="c4f8e-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="c4f8e-108">[Microsoft. extensions. Configuration](https://github.com/aspnet/Extensions) pour lire la configuration de l’application à partir d’un fichier JSON.</span><span class="sxs-lookup"><span data-stu-id="c4f8e-108">[Microsoft.Extensions.Configuration](https://github.com/aspnet/Extensions) to read application configuration from a JSON file.</span></span>
- <span data-ttu-id="c4f8e-109">[Bibliothèque d’authentification Microsoft (MSAL) pour .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) pour authentifier l’utilisateur et acquérir des jetons d’accès.</span><span class="sxs-lookup"><span data-stu-id="c4f8e-109">[Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="c4f8e-110">[Bibliothèque cliente .NET Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) pour effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="c4f8e-110">[Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="c4f8e-111">[Fournisseurs d’authentification pour le kit de développement logiciel (SDK) Microsoft Graph .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet-auth) pour permettre à la bibliothèque cliente Graph de demander automatiquement des jetons lors de la réalisation d’appels d’API.</span><span class="sxs-lookup"><span data-stu-id="c4f8e-111">[Authentication Providers for Microsoft Graph .NET SDK](https://github.com/microsoftgraph/msgraph-sdk-dotnet-auth) to enable the Graph client library to request tokens automatically when making API calls.</span></span>

<span data-ttu-id="c4f8e-112">Exécutez les commandes suivantes dans votre interface CLI pour installer les dépendances.</span><span class="sxs-lookup"><span data-stu-id="c4f8e-112">Run the following commands in your CLI to install the dependencies.</span></span>

```Shell
dotnet add package Microsoft.Extensions.Configuration --version 2.2.0
dotnet add package Microsoft.Extensions.Configuration.FileExtensions --version 2.2.0
dotnet add package Microsoft.Extensions.Configuration.Json --version 2.2.0
dotnet add package Microsoft.Extensions.Configuration.Binder --version 2.2.0
dotnet add package Microsoft.Identity.Client --version 4.3.1
dotnet add package Microsoft.Graph --version 1.17.0
```

## <a name="design-the-app"></a><span data-ttu-id="c4f8e-113">Concevoir l’application</span><span class="sxs-lookup"><span data-stu-id="c4f8e-113">Design the app</span></span>

<span data-ttu-id="c4f8e-114">Dans cette section, vous allez créer un menu simple basé sur la console.</span><span class="sxs-lookup"><span data-stu-id="c4f8e-114">In this section you will create a simple console-based menu.</span></span>

<span data-ttu-id="c4f8e-115">Ouvrez **Program.cs** dans un éditeur de texte (tel que [Visual Studio code](https://code.visualstudio.com/)) et remplacez l’intégralité de son contenu par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="c4f8e-115">Open **Program.cs** in a text editor (such as [Visual Studio Code](https://code.visualstudio.com/)) and replace its entire contents with the following code.</span></span>

[!code-csharp[](../demos/01-create-app/GraphTutorial/Program.cs)]

<span data-ttu-id="c4f8e-116">Cela implémente un menu de base et lit le choix de l’utilisateur à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="c4f8e-116">This implements a basic menu and reads the user's choice from the command line.</span></span>
