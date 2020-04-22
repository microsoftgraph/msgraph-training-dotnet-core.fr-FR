<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="15724-101">Commencez par créer un projet de console principale .NET à l’aide de l' [interface CLI .net Core](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="15724-101">Begin by creating a new .NET Core console project using the [.NET Core CLI](/dotnet/core/tools/).</span></span>

1. <span data-ttu-id="15724-102">Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet.</span><span class="sxs-lookup"><span data-stu-id="15724-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="15724-103">Exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="15724-103">Run the following command.</span></span>

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. <span data-ttu-id="15724-104">Une fois le projet créé, vérifiez qu’il fonctionne en remplaçant le répertoire actuel par le répertoire **GraphTutorial** et en exécutant la commande suivante dans votre CLI.</span><span class="sxs-lookup"><span data-stu-id="15724-104">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

    <span data-ttu-id="15724-105">Si elle fonctionne, l’application doit produire `Hello World!`une sortie.</span><span class="sxs-lookup"><span data-stu-id="15724-105">If it works, the app should output `Hello World!`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="15724-106">Installer les dépendances</span><span class="sxs-lookup"><span data-stu-id="15724-106">Install dependencies</span></span>

<span data-ttu-id="15724-107">Avant de poursuivre, ajoutez des dépendances supplémentaires que vous utiliserez plus tard.</span><span class="sxs-lookup"><span data-stu-id="15724-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="15724-108">[Microsoft. extensions. Configuration. UserSecrets](https://github.com/aspnet/extensions) pour lire la configuration de l’application à partir du [magasin de secrets de développement .net](https://docs.microsoft.com/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="15724-108">[Microsoft.Extensions.Configuration.UserSecrets](https://github.com/aspnet/extensions) to read application configuration from the [.NET development secret store](https://docs.microsoft.com/aspnet/core/security/app-secrets).</span></span>
- <span data-ttu-id="15724-109">[Bibliothèque d’authentification Microsoft (MSAL) pour .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) pour authentifier l’utilisateur et acquérir des jetons d’accès.</span><span class="sxs-lookup"><span data-stu-id="15724-109">[Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="15724-110">[Bibliothèque cliente .NET Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) pour effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="15724-110">[Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to the Microsoft Graph.</span></span>

<span data-ttu-id="15724-111">Exécutez les commandes suivantes dans votre interface CLI pour installer les dépendances.</span><span class="sxs-lookup"><span data-stu-id="15724-111">Run the following commands in your CLI to install the dependencies.</span></span>

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 3.1.2
dotnet add package Microsoft.Identity.Client --version 4.10.0
dotnet add package Microsoft.Graph --version 3.0.1
```

## <a name="design-the-app"></a><span data-ttu-id="15724-112">Concevoir l’application</span><span class="sxs-lookup"><span data-stu-id="15724-112">Design the app</span></span>

<span data-ttu-id="15724-113">Dans cette section, vous allez créer un menu simple basé sur la console.</span><span class="sxs-lookup"><span data-stu-id="15724-113">In this section you will create a simple console-based menu.</span></span>

<span data-ttu-id="15724-114">Ouvrez **Program.cs** dans un éditeur de texte (tel que [Visual Studio code](https://code.visualstudio.com/)) et remplacez l’intégralité de son contenu par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="15724-114">Open **Program.cs** in a text editor (such as [Visual Studio Code](https://code.visualstudio.com/)) and replace its entire contents with the following code.</span></span>

```csharp
using Microsoft.Extensions.Configuration;
using System;

namespace GraphTutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(".NET Core Graph Tutorial\n");

            int choice = -1;

            while (choice != 0) {
                Console.WriteLine("Please choose one of the following options:");
                Console.WriteLine("0. Exit");
                Console.WriteLine("1. Display access token");
                Console.WriteLine("2. List calendar events");

                try
                {
                    choice = int.Parse(Console.ReadLine());
                }
                catch (System.FormatException)
                {
                    // Set to invalid value
                    choice = -1;
                }

                switch(choice)
                {
                    case 0:
                        // Exit the program
                        Console.WriteLine("Goodbye...");
                        break;
                    case 1:
                        // Display access token
                        break;
                    case 2:
                        // List the calendar
                        break;
                    default:
                        Console.WriteLine("Invalid choice! Please try again.");
                        break;
                }
            }
        }
    }
}
```

<span data-ttu-id="15724-115">Cela implémente un menu de base et lit le choix de l’utilisateur à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="15724-115">This implements a basic menu and reads the user's choice from the command line.</span></span>
