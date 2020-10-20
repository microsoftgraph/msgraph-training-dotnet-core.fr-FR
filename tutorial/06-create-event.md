<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b63f3-101">Dans cette section, vous allez ajouter la possibilité de créer des événements dans le calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b63f3-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="b63f3-102">Ouvrez **./Graph/GraphHelper.cs** et ajoutez la fonction suivante à la classe **GraphHelper** .</span><span class="sxs-lookup"><span data-stu-id="b63f3-102">Open **./Graph/GraphHelper.cs** and add the following function to the **GraphHelper** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="CreateEventSnippet":::

    <span data-ttu-id="b63f3-103">Ce code initialise un objet **Event** et utilise le kit de développement logiciel Graph pour l’ajouter au calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b63f3-103">This code initializes an **Event** object and uses the Graph SDK to add it to the user's calendar.</span></span>

1. <span data-ttu-id="b63f3-104">Ouvrez **./Program.cs** et ajoutez les fonctions suivantes à la classe **Program** .</span><span class="sxs-lookup"><span data-stu-id="b63f3-104">Open **./Program.cs** and add the following functions to the **Program** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="UserInputSnippet":::

    <span data-ttu-id="b63f3-105">Ces fonctions sont des fonctions d’assistance pour la lecture des données entrées par l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b63f3-105">These functions are helper functions for reading user input.</span></span>

1. <span data-ttu-id="b63f3-106">Ajoutez la fonction suivante à la classe **Program** .</span><span class="sxs-lookup"><span data-stu-id="b63f3-106">Add the following function to the **Program** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="CreateEventSnippet":::

    <span data-ttu-id="b63f3-107">Cette fonction invite l’utilisateur à indiquer l’objet, les participants, le début, la fin et le corps, puis utilise ces valeurs pour appeler `GraphHelper.CreateEvent` .</span><span class="sxs-lookup"><span data-stu-id="b63f3-107">This function prompts the user for subject, attendees, start, end, and body, then uses those values to call `GraphHelper.CreateEvent`.</span></span>

1. <span data-ttu-id="b63f3-108">Ajoutez les éléments suivants juste après le `// Create a new event` commentaire dans la `Main` fonction.</span><span class="sxs-lookup"><span data-stu-id="b63f3-108">Add the following just after the `// Create a new event` comment in the `Main` function.</span></span>

    ```csharp
    CreateEvent(user.MailboxSettings.TimeZone);
    ```

1. <span data-ttu-id="b63f3-109">Enregistrez toutes vos modifications et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="b63f3-109">Save all of your changes and run the app.</span></span> <span data-ttu-id="b63f3-110">Choisissez l’option **Ajouter un événement** .</span><span class="sxs-lookup"><span data-stu-id="b63f3-110">Choose the **Add an event** option.</span></span> <span data-ttu-id="b63f3-111">Répondez aux invites pour créer un événement dans le calendrier de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b63f3-111">Respond to the prompts to create a new event on the user's calendar.</span></span>
