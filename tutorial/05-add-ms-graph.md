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
                    return await graphClient.Me
                        .Request()
                        .Select(u => new{
                            u.DisplayName,
                            u.MailboxSettings
                        })
                        .GetAsync();
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

1. Ajoutez le code suivant dans `Main` in **./Program.cs** juste après l' `GetAccessToken` appel pour obtenir l’utilisateur et en sortie le nom d’affichage de l’utilisateur.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

Si vous exécutez l’application maintenant, une fois que vous vous êtes en train de vous reconnecter à l’application, vous vous félicitez par son nom.

## <a name="get-a-calendar-view"></a>Obtenir un affichage Calendrier

1. Ajoutez la fonction suivante à la `GraphHelper` classe pour obtenir des événements à partir du calendrier de l’utilisateur.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

Que fait ce code ?

- L’URL qui sera appelée est `/me/calendarview`.
- Les `startDateTime` `endDateTime` paramètres et définissent le début et la fin de l’affichage Calendrier.
- L' `Prefer: outlook.timezone` en-tête entraîne le `start` `end` renvoi des événements et dans le fuseau horaire de l’utilisateur.
- La `Top` fonction demande au plus 50 événements.
- La `Select` fonction limite les champs renvoyés pour chaque événement à ceux que l’application utilisera réellement.
- La `OrderBy` fonction trie les résultats en fonction de la date et de l’heure de début.

## <a name="display-the-results"></a>Afficher les résultats

1. Ajoutez la fonction suivante à la `Program` classe pour mettre en forme les propriétés [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph dans un format convivial.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. Ajoutez la fonction suivante à la `Program` classe pour obtenir les événements de l’utilisateur et les sortir dans la console.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. Ajoutez les éléments suivants juste après le `// List the calendar` commentaire dans la `Main` fonction.

    ```csharp
    ListCalendarEvents(
        user.MailboxSettings.TimeZone,
        $"{user.MailboxSettings.DateFormat} {user.MailboxSettings.TimeFormat}"
    );
    ```

1. Enregistrez toutes vos modifications et exécutez l’application. Sélectionnez l’option **afficher le calendrier de cette semaine** pour afficher la liste des événements de l’utilisateur.

    ```Shell
    Welcome Lynne Robbins!

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. View this week's calendar
    3. Add an event
    2
    Events:
    Subject: Meeting
      Organizer: Lynne Robbins
      Start: 9/28/2020 10:00 AM
      End: 9/28/2020 11:30 AM
    Subject: Weekly meeting
      Organizer: Lynne Robbins
      Start: 9/28/2020 2:00 PM
      End: 9/28/2020 3:00 PM
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 9/28/2020 4:00 PM
      End: 9/28/2020 5:30 PM
    Subject: Tailspin Toys Proposal Review + Lunch
      Organizer: Lidia Holloway
      Start: 9/29/2020 12:00 PM
      End: 9/29/2020 1:00 PM
    Subject: Weekly meeting
      Organizer: Lynne Robbins
      Start: 9/29/2020 2:00 PM
      End: 9/29/2020 3:00 PM
    Subject: Project Tailspin
    ```
