<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="941fc-101">Commencez par créer un projet de console .NET Core à l’aide [de .NET Core CLI](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="941fc-101">Begin by creating a new .NET Core console project using the [.NET Core CLI](/dotnet/core/tools/).</span></span>

1. <span data-ttu-id="941fc-102">Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet.</span><span class="sxs-lookup"><span data-stu-id="941fc-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="941fc-103">Exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="941fc-103">Run the following command.</span></span>

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. <span data-ttu-id="941fc-104">Une fois le projet créé, vérifiez qu’il fonctionne en modifiant le répertoire actuel en répertoire **GraphTutorial** et en exécutant la commande suivante dans votre CLI.</span><span class="sxs-lookup"><span data-stu-id="941fc-104">Once the project is created, verify that it works by changing the current directory to the **GraphTutorial** directory and running the following command in your CLI.</span></span>

    ```Shell
    dotnet run
    ```

    <span data-ttu-id="941fc-105">Si elle fonctionne, l’application doit être `Hello World!` sortie.</span><span class="sxs-lookup"><span data-stu-id="941fc-105">If it works, the app should output `Hello World!`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="941fc-106">Installer les dépendances</span><span class="sxs-lookup"><span data-stu-id="941fc-106">Install dependencies</span></span>

<span data-ttu-id="941fc-107">Avant de passer à autre chose, ajoutez des dépendances supplémentaires que vous utiliserez ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="941fc-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="941fc-108">[Microsoft.Extensions.Config'uration. UserSecrets pour lire](https://github.com/aspnet/extensions) la configuration de l’application à partir du magasin de secrets [de développement .NET](https://docs.microsoft.com/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="941fc-108">[Microsoft.Extensions.Configuration.UserSecrets](https://github.com/aspnet/extensions) to read application configuration from the [.NET development secret store](https://docs.microsoft.com/aspnet/core/security/app-secrets).</span></span>
- <span data-ttu-id="941fc-109">[Microsoft Authentication Library (MSAL) pour .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) authentifier l’utilisateur et acquérir des jetons d’accès.</span><span class="sxs-lookup"><span data-stu-id="941fc-109">[Microsoft Authentication Library (MSAL) for .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="941fc-110">[Bibliothèque cliente Microsoft Graph .NET pour](https://github.com/microsoftgraph/msgraph-sdk-dotnet) appeler Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="941fc-110">[Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="941fc-111">[TimeZoneConverter pour](https://github.com/mj1856/TimeZoneConverter) la traduction des identificateurs de fuseau horaire Windows en identificateurs IANA.</span><span class="sxs-lookup"><span data-stu-id="941fc-111">[TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) for translating Windows time zone identifiers to IANA identifiers.</span></span>

<span data-ttu-id="941fc-112">Exécutez les commandes suivantes dans votre CLI pour installer les dépendances.</span><span class="sxs-lookup"><span data-stu-id="941fc-112">Run the following commands in your CLI to install the dependencies.</span></span>

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 5.0.0
dotnet add package Microsoft.Identity.Client --version 4.25.0
dotnet add package Microsoft.Graph --version 3.22.0
dotnet add package TimeZoneConverter
```

## <a name="design-the-app"></a><span data-ttu-id="941fc-113">Concevoir l’application</span><span class="sxs-lookup"><span data-stu-id="941fc-113">Design the app</span></span>

<span data-ttu-id="941fc-114">Dans cette section, vous allez créer un menu simple basé sur la console.</span><span class="sxs-lookup"><span data-stu-id="941fc-114">In this section you will create a simple console-based menu.</span></span>

<span data-ttu-id="941fc-115">Ouvrez **./Program.cs** dans un éditeur de texte (par [exemple, Visual Studio Code)](https://code.visualstudio.com/)et remplacez tout son contenu par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="941fc-115">Open **./Program.cs** in a text editor (such as [Visual Studio Code](https://code.visualstudio.com/)) and replace its entire contents with the following code.</span></span>

```csharp
using Microsoft.Extensions.Configuration;
using System;
using System.Collections.Generic;

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
                Console.WriteLine("2. View this week's calendar");
                Console.WriteLine("3. Add an event");

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
                    case 3:
                        // Create a new event
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

<span data-ttu-id="941fc-116">Cela implémente un menu de base et lit le choix de l’utilisateur à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="941fc-116">This implements a basic menu and reads the user's choice from the command line.</span></span>
