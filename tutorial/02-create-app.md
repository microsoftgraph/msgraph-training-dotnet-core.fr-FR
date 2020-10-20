<!-- markdownlint-disable MD002 MD041 -->

Commencez par créer un projet de console principale .NET à l’aide de l' [interface CLI .net Core](/dotnet/core/tools/).

1. Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet. Exécutez la commande suivante.

    ```Shell
    dotnet new console -o GraphTutorial
    ```

1. Une fois le projet créé, vérifiez qu’il fonctionne en remplaçant le répertoire actuel par le répertoire **GraphTutorial** et en exécutant la commande suivante dans votre CLI.

    ```Shell
    dotnet run
    ```

    Si elle fonctionne, l’application doit produire une sortie `Hello World!` .

## <a name="install-dependencies"></a>Installer les dépendances

Avant de poursuivre, ajoutez des dépendances supplémentaires que vous utiliserez plus tard.

- [Microsoft.Extensions.Configuration. UserSecrets](https://github.com/aspnet/extensions) pour lire la configuration de l’application à partir du [magasin de secrets de développement .net](https://docs.microsoft.com/aspnet/core/security/app-secrets).
- [Bibliothèque d’authentification Microsoft (MSAL) pour .net](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) pour authentifier l’utilisateur et acquérir des jetons d’accès.
- [Bibliothèque cliente .NET Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) pour effectuer des appels à Microsoft Graph.
- [TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) pour la traduction des identificateurs de fuseau horaire Windows en identificateurs IANA.

Exécutez les commandes suivantes dans votre interface CLI pour installer les dépendances.

```Shell
dotnet add package Microsoft.Extensions.Configuration.UserSecrets --version 3.1.8
dotnet add package Microsoft.Identity.Client --version 4.19.0
dotnet add package Microsoft.Graph --version 3.15.0
dotnet add package TimeZoneConverter
```

## <a name="design-the-app"></a>Concevoir l’application

Dans cette section, vous allez créer un menu simple basé sur la console.

Ouvrez **./Program.cs** dans un éditeur de texte (tel que [Visual Studio code](https://code.visualstudio.com/)) et remplacez l’intégralité de son contenu par le code suivant.

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

Cela implémente un menu de base et lit le choix de l’utilisateur à partir de la ligne de commande.
