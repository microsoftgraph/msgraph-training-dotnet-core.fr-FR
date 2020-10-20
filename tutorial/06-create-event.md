<!-- markdownlint-disable MD002 MD041 -->

Dans cette section, vous allez ajouter la possibilité de créer des événements dans le calendrier de l’utilisateur.

1. Ouvrez **./Graph/GraphHelper.cs** et ajoutez la fonction suivante à la classe **GraphHelper** .

    :::code language="csharp" source="../demo/GraphTutorial/Graph/GraphHelper.cs" id="CreateEventSnippet":::

    Ce code initialise un objet **Event** et utilise le kit de développement logiciel Graph pour l’ajouter au calendrier de l’utilisateur.

1. Ouvrez **./Program.cs** et ajoutez les fonctions suivantes à la classe **Program** .

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="UserInputSnippet":::

    Ces fonctions sont des fonctions d’assistance pour la lecture des données entrées par l’utilisateur.

1. Ajoutez la fonction suivante à la classe **Program** .

    :::code language="csharp" source="../demo/GraphTutorial/Program.cs" id="CreateEventSnippet":::

    Cette fonction invite l’utilisateur à indiquer l’objet, les participants, le début, la fin et le corps, puis utilise ces valeurs pour appeler `GraphHelper.CreateEvent` .

1. Ajoutez les éléments suivants juste après le `// Create a new event` commentaire dans la `Main` fonction.

    ```csharp
    CreateEvent(user.MailboxSettings.TimeZone);
    ```

1. Enregistrez toutes vos modifications et exécutez l’application. Choisissez l’option **Ajouter un événement** . Répondez aux invites pour créer un événement dans le calendrier de l’utilisateur.
