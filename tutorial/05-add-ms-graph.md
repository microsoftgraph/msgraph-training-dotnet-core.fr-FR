<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="94ef1-101">Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application.</span><span class="sxs-lookup"><span data-stu-id="94ef1-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="94ef1-102">Pour cette application, vous allez utiliser la [bibliothèque cliente .NET Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-dotnet) pour passer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="94ef1-102">For this application, you will use the [Microsoft Graph .NET Client Library](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-user-details"></a><span data-ttu-id="94ef1-103">Obtenir les détails de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="94ef1-103">Get user details</span></span>

1. <span data-ttu-id="94ef1-104">Créez un répertoire dans le répertoire **GraphTutorial** nommé **Graph**.</span><span class="sxs-lookup"><span data-stu-id="94ef1-104">Create a new directory in the **GraphTutorial** directory named **Graph**.</span></span>
1. <span data-ttu-id="94ef1-105">Créez un fichier dans le répertoire **Graph** nommé **GraphHelper.cs** et ajoutez le code suivant à ce fichier.</span><span class="sxs-lookup"><span data-stu-id="94ef1-105">Create a new file in the **Graph** directory named **GraphHelper.cs** and add the following code to that file.</span></span>

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

1. <span data-ttu-id="94ef1-106">Ajoutez le code `Main` suivant dans **Program.cs** juste après l' `GetAccessToken` appel pour obtenir l’utilisateur et en sortie le nom d’affichage de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="94ef1-106">Add the following code in `Main` in **Program.cs** just after the `GetAccessToken` call to get the user and output the user's display name.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="GetUserSnippet":::

<span data-ttu-id="94ef1-107">Si vous exécutez l’application maintenant, une fois que vous vous êtes en train de vous reconnecter à l’application, vous vous félicitez par son nom.</span><span class="sxs-lookup"><span data-stu-id="94ef1-107">If you run the app now, after you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="94ef1-108">Récupérer les événements de calendrier à partir d’Outlook</span><span class="sxs-lookup"><span data-stu-id="94ef1-108">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="94ef1-109">Ajoutez la fonction suivante à la `GraphHelper` classe pour obtenir des événements à partir du calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="94ef1-109">Add the following function to the `GraphHelper` class to get events from the user's calendar.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="GetEventsSnippet":::

<span data-ttu-id="94ef1-110">Que fait ce code ?</span><span class="sxs-lookup"><span data-stu-id="94ef1-110">Consider what this code is doing.</span></span>

- <span data-ttu-id="94ef1-111">L’URL qui sera appelée est `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="94ef1-111">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="94ef1-112">La `Select` fonction limite les champs renvoyés pour chaque événement à ceux que l’application utilisera réellement.</span><span class="sxs-lookup"><span data-stu-id="94ef1-112">The `Select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="94ef1-113">La fonction `OrderBy` trie les résultats en fonction de la date et de l’heure de création, avec l’élément le plus récent en premier.</span><span class="sxs-lookup"><span data-stu-id="94ef1-113">The `OrderBy` function sorts the results by the date and time they were created, with the most recent item being first.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="94ef1-114">Afficher les résultats</span><span class="sxs-lookup"><span data-stu-id="94ef1-114">Display the results</span></span>

1. <span data-ttu-id="94ef1-115">Ajoutez la fonction suivante à la `Program` classe pour mettre en forme les propriétés [DateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) de Microsoft Graph dans un format convivial.</span><span class="sxs-lookup"><span data-stu-id="94ef1-115">Add the following function to the `Program` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="FormatDateSnippet":::

1. <span data-ttu-id="94ef1-116">Ajoutez la fonction suivante à la `Program` classe pour obtenir les événements de l’utilisateur et les sortir dans la console.</span><span class="sxs-lookup"><span data-stu-id="94ef1-116">Add the following function to the `Program` class to get the user's events and output them to the console.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="ListEventsSnippet":::

1. <span data-ttu-id="94ef1-117">Ajoutez les éléments suivants juste après `// List the calendar` le commentaire dans `Main` la fonction.</span><span class="sxs-lookup"><span data-stu-id="94ef1-117">Add the following just after the `// List the calendar` comment in the `Main` function.</span></span>

    ```csharp
    ListCalendarEvents();
    ```

1. <span data-ttu-id="94ef1-118">Enregistrez toutes vos modifications et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="94ef1-118">Save all of your changes and run the app.</span></span> <span data-ttu-id="94ef1-119">Choisissez l’option **liste des événements de calendrier** pour afficher la liste des événements de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="94ef1-119">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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
