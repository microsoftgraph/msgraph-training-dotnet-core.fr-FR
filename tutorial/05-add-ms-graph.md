<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application. Pour cette application, vous allez utiliser la bibliothèque [cliente Microsoft Graph .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) pour effectuer des appels à Microsoft Graph.

## <a name="get-user-details"></a>Obtenir les détails de l’utilisateur

1. Créez un répertoire dans le **répertoire GraphTutorial** nommé **Graph**.
1. Créez un fichier dans le répertoire **Graph** nommé **GraphHelper.cs** et ajoutez le code suivant à ce fichier.

    ```csharp
    using Microsoft.Graph;
    using System;
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using TimeZoneConverter;

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

1. Ajoutez le code suivant dans ./Program.cs juste après l’appel pour obtenir l’utilisateur et obtenir le nom complet `Main` de  `GetAccessToken` l’utilisateur.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

Si vous exécutez l’application maintenant, une fois que vous vous connectez, l’application vous reçoit par son nom.

## <a name="get-a-calendar-view"></a>Obtenir un affichage Calendrier

1. Ajoutez la fonction suivante à la `GraphHelper` classe pour obtenir des événements à partir du calendrier de l’utilisateur.

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

Que fait ce code ?

- L’URL qui sera appelée est `/me/calendarview`.
- Les `startDateTime` `endDateTime` paramètres définissent le début et la fin de l’affichage Calendrier.
- `Prefer: outlook.timezone`L’en-tête entraîne le retour des événements dans `start` le fuseau horaire de `end` l’utilisateur.
- La `Top` fonction demande au maximum 50 événements.
- La fonction limite les champs renvoyés pour chaque événement à ceux que `Select` l’application utilisera réellement.
- La `OrderBy` fonction trie les résultats par date et heure de début.

## <a name="display-the-results"></a>Afficher les résultats

1. Ajoutez la fonction suivante à la classe pour mettre en forme les `Program` [propriétés dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph dans un format convivial.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. Ajoutez la fonction suivante à la classe pour obtenir les événements `Program` de l’utilisateur et les sortir dans la console.

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. Ajoutez ce qui suit juste après `// List the calendar` le commentaire dans la `Main` fonction.

    ```csharp
    ListCalendarEvents(
        user.MailboxSettings.TimeZone,
        $"{user.MailboxSettings.DateFormat} {user.MailboxSettings.TimeFormat}"
    );
    ```

1. Enregistrez toutes vos modifications et exécutez l’application. Choisissez **l’option Afficher le calendrier de cette semaine** pour afficher la liste des événements de l’utilisateur.

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
