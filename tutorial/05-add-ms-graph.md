<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application. Pour cette application, vous allez utiliser la [bibliothèque cliente .NET Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) pour passer des appels à Microsoft Graph.

## <a name="get-user-details"></a>Obtenir les détails de l’utilisateur

1. Créez un répertoire dans le répertoire **GraphTutorial** nommé **Graph**.
1. Créez un fichier dans le répertoire **Graph** nommé **GraphHelper.cs** et ajoutez le code suivant à ce fichier.

    ```csharp
    using Microsoft.Graph;
    using System;
    using System.Collections.Generic;
    using System.Threading.Tasks;

    namespace GraphTutorial
    {
        public class GraphHelper
        {
            private static GraphServiceClient graphClient;
            public static void Initialize(IAuthenticationProvider authProvider)
            {
                graphClient = new GraphServiceClient(authProvider);
            }

            public static async Task<User> GetMeAsync()
            {
                try
                {
                    // GET /me
                    return await graphClient.Me.Request().GetAsync();
                }
                catch (ServiceException ex)
                {
                    Console.WriteLine($"Error getting signed-in user: {ex.Message}");
                    return null;
                }
            }
        }
    }
    ```

1. Ajoutez le code `Main` suivant dans **Program.cs** juste après l' `GetAccessToken` appel pour obtenir l’utilisateur et en sortie le nom d’affichage de l’utilisateur.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

Si vous exécutez l’application maintenant, une fois que vous vous êtes en train de vous reconnecter à l’application, vous vous félicitez par son nom.

## <a name="get-calendar-events-from-outlook"></a>Récupérer les événements de calendrier à partir d’Outlook

1. Ajoutez la fonction suivante à la `GraphHelper` classe pour obtenir des événements à partir du calendrier de l’utilisateur.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

Que fait ce code ?

- L’URL qui sera appelée est `/me/events`.
- La `Select` fonction limite les champs renvoyés pour chaque événement à ceux que l’application utilisera réellement.
- La fonction `OrderBy` trie les résultats en fonction de la date et de l’heure de création, avec l’élément le plus récent en premier.

## <a name="display-the-results"></a>Afficher les résultats

1. Ajoutez la fonction suivante à la `Program` classe pour mettre en forme les propriétés [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph dans un format convivial.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. Ajoutez la fonction suivante à la `Program` classe pour obtenir les événements de l’utilisateur et les sortir dans la console.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. Ajoutez les éléments suivants juste après `// List the calendar` le commentaire dans `Main` la fonction.

    ```csharp
    ListCalendarEvents();
    ```

1. Enregistrez toutes vos modifications et exécutez l’application. Choisissez l’option **liste des événements de calendrier** pour afficher la liste des événements de l’utilisateur.

    ```Shell
    Welcome Adele Vance

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. List calendar events
    2
    Events:
    Subject: Team meeting
      Organizer: Adele Vance
      Start: 5/22/19, 3:00 PM
      End: 5/22/19, 4:00 PM
    Subject: Team Lunch
      Organizer: Adele Vance
      Start: 5/24/19, 6:30 PM
      End: 5/24/19, 8:00 PM
    Subject: Flight to Redmond
      Organizer: Adele Vance
      Start: 5/26/19, 4:30 PM
      End: 5/26/19, 7:00 PM
    Subject: Let's meet to discuss strategy
      Organizer: Patti Fernandez
      Start: 5/27/19, 10:00 PM
      End: 5/27/19, 10:30 PM
    Subject: All-hands meeting
      Organizer: Adele Vance
      Start: 5/28/19, 3:30 PM
      End: 5/28/19, 5:00 PM
    ```
