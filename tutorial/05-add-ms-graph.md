<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7f91c-101">Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application.</span><span class="sxs-lookup"><span data-stu-id="7f91c-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="7f91c-102">Pour cette application, vous allez utiliser la bibliothèque [cliente Microsoft Graph .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) pour effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="7f91c-102">For this application, you will use the [Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-user-details"></a><span data-ttu-id="7f91c-103">Obtenir les détails de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="7f91c-103">Get user details</span></span>

1. <span data-ttu-id="7f91c-104">Créez un répertoire dans le **répertoire GraphTutorial** nommé **Graph**.</span><span class="sxs-lookup"><span data-stu-id="7f91c-104">Create a new directory in the **GraphTutorial** directory named **Graph**.</span></span>
1. <span data-ttu-id="7f91c-105">Créez un fichier dans le répertoire **Graph** nommé **GraphHelper.cs** et ajoutez le code suivant à ce fichier.</span><span class="sxs-lookup"><span data-stu-id="7f91c-105">Create a new file in the **Graph** directory named **GraphHelper.cs** and add the following code to that file.</span></span>

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

1. <span data-ttu-id="7f91c-106">Ajoutez le code suivant dans ./Program.cs juste après l’appel pour obtenir l’utilisateur et obtenir le nom complet `Main` de  `GetAccessToken` l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7f91c-106">Add the following code in `Main` in **./Program.cs** just after the `GetAccessToken` call to get the user and output the user's display name.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

<span data-ttu-id="7f91c-107">Si vous exécutez l’application maintenant, une fois que vous vous connectez, l’application vous reçoit par son nom.</span><span class="sxs-lookup"><span data-stu-id="7f91c-107">If you run the app now, after you log in the app welcomes you by name.</span></span>

## <a name="get-a-calendar-view"></a><span data-ttu-id="7f91c-108">Obtenir un affichage Calendrier</span><span class="sxs-lookup"><span data-stu-id="7f91c-108">Get a calendar view</span></span>

1. <span data-ttu-id="7f91c-109">Ajoutez la fonction suivante à la `GraphHelper` classe pour obtenir des événements à partir du calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7f91c-109">Add the following function to the `GraphHelper` class to get events from the user's calendar.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

<span data-ttu-id="7f91c-110">Que fait ce code ?</span><span class="sxs-lookup"><span data-stu-id="7f91c-110">Consider what this code is doing.</span></span>

- <span data-ttu-id="7f91c-111">L’URL qui sera appelée est `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="7f91c-111">The URL that will be called is `/me/calendarview`.</span></span>
- <span data-ttu-id="7f91c-112">Les `startDateTime` `endDateTime` paramètres définissent le début et la fin de l’affichage Calendrier.</span><span class="sxs-lookup"><span data-stu-id="7f91c-112">The `startDateTime` and `endDateTime` parameters define the start and end of the calendar view.</span></span>
- <span data-ttu-id="7f91c-113">`Prefer: outlook.timezone`L’en-tête entraîne le retour des événements dans `start` le fuseau horaire de `end` l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7f91c-113">The `Prefer: outlook.timezone` header causes the `start` and `end` of the events to be returned in the user's time zone.</span></span>
- <span data-ttu-id="7f91c-114">La `Top` fonction demande au maximum 50 événements.</span><span class="sxs-lookup"><span data-stu-id="7f91c-114">The `Top` function requests at most 50 events.</span></span>
- <span data-ttu-id="7f91c-115">La fonction limite les champs renvoyés pour chaque événement à ceux que `Select` l’application utilisera réellement.</span><span class="sxs-lookup"><span data-stu-id="7f91c-115">The `Select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="7f91c-116">La `OrderBy` fonction trie les résultats par date et heure de début.</span><span class="sxs-lookup"><span data-stu-id="7f91c-116">The `OrderBy` function sorts the results by the start date and time.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="7f91c-117">Afficher les résultats</span><span class="sxs-lookup"><span data-stu-id="7f91c-117">Display the results</span></span>

1. <span data-ttu-id="7f91c-118">Ajoutez la fonction suivante à la classe pour mettre en forme les `Program` [propriétés dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph dans un format convivial.</span><span class="sxs-lookup"><span data-stu-id="7f91c-118">Add the following function to the `Program` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. <span data-ttu-id="7f91c-119">Ajoutez la fonction suivante à la classe pour obtenir les événements `Program` de l’utilisateur et les sortir dans la console.</span><span class="sxs-lookup"><span data-stu-id="7f91c-119">Add the following function to the `Program` class to get the user's events and output them to the console.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. <span data-ttu-id="7f91c-120">Ajoutez ce qui suit juste après `// List the calendar` le commentaire dans la `Main` fonction.</span><span class="sxs-lookup"><span data-stu-id="7f91c-120">Add the following just after the `// List the calendar` comment in the `Main` function.</span></span>

    ```csharp
    ListCalendarEvents(
        user.MailboxSettings.TimeZone,
        $"{user.MailboxSettings.DateFormat} {user.MailboxSettings.TimeFormat}"
    );
    ```

1. <span data-ttu-id="7f91c-121">Enregistrez toutes vos modifications et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="7f91c-121">Save all of your changes and run the app.</span></span> <span data-ttu-id="7f91c-122">Choisissez **l’option Afficher le calendrier de cette semaine** pour afficher la liste des événements de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7f91c-122">Choose the **View this week's calendar** option to see a list of the user's events.</span></span>

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
