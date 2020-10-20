# <a name="how-to-run-the-completed-project"></a>Exécution du projet terminé

## <a name="prerequisites"></a>Conditions préalables

Pour exécuter le projet terminé dans ce dossier, vous avez besoin des éléments suivants :

- Le [Kit de développement logiciel .net Core](https://dotnet.microsoft.com/download) installé sur votre ordinateur de développement. (**Remarque :** ce didacticiel a été écrit avec .net Core SDK version 3.1.402. Les étapes de ce guide peuvent fonctionner avec d’autres versions, mais cela n’a pas été testé.)
- Un compte professionnel ou scolaire Microsoft.

Si vous n’avez pas de compte Microsoft, vous pouvez vous [inscrire au programme pour les développeurs office 365](https://developer.microsoft.com/office/dev-program) pour obtenir un abonnement gratuit à Office 365.

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Enregistrer une application Web avec le centre d’administration Azure Active Directory

1. Ouvrez un navigateur, accédez au [Centre d’administration Azure Active Directory ](https://aad.portal.azure.com) et connectez-vous à l’aide d’un **compte personnel** (ou compte Microsoft) ou d’un **compte professionnel ou scolaire**.

1. Sélectionnez **Azure Active Directory** dans le volet de navigation gauche, puis sélectionnez **Inscriptions d’applications** sous **Gérer**.

    ![Une capture d’écran des inscriptions d’applications ](/tutorial/images/aad-portal-app-registrations.png)

1. Sélectionnez **Nouvelle inscription**. Sur la page **Inscrire une application**, définissez les valeurs comme suit.

    - Définissez le **Nom** sur `.NET Core Graph Tutorial`.
    - Définissez les types de comptes **pris en charge** sur **les comptes dans n’importe quel annuaire d’organisation**.
    - Laissez **Redirect URI** vide.

    ![Capture d’écran de la page Inscrire une application](/tutorial/images/aad-register-an-app.png)

1. Sélectionner **Inscription**. Sur la page **didacticiel .net Core Graph** , copiez la valeur de l' **ID d’application (client)** et enregistrez-la, vous en aurez besoin à l’étape suivante.

    ![Une capture d’écran de l’ID d’application de la nouvelle inscription d'application](/tutorial/images/aad-application-id.png)

1. Sélectionnez le lien **Ajouter un URI de redirection** . Sur la page **URI de redirection** , recherchez la section **URI de redirection suggérée pour les clients publics (mobile, bureau)** . Sélectionnez l' `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.

    ![Capture d’écran de la page des URI de redirection](/tutorial/images/aad-redirect-uris.png)

1. Recherchez la section **type de client par défaut** et modifiez le paramètre **traiter l’application comme un client public** sur **Oui**, puis cliquez sur **Enregistrer**.

    ![Capture d’écran de la section type de client par défaut](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a>Configurer l’exemple

1. Initialisez le [magasin de secrets de développement .net](https://docs.microsoft.com/aspnet/core/security/app-secrets) en ouvrant votre interface CLI dans le répertoire qui contient **GraphTutorial. csproj** et en exécutant la commande suivante.

    ```Shell
    dotnet user-secrets init
    ```

1. Ajoutez votre ID d’application et une liste d’étendues requises au magasin secret à l’aide des commandes suivantes. Remplacez `YOUR_APP_ID_HERE` par l’ID d’application que vous avez créé dans le portail Azure.

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;Calendars.Read"
    ```

## <a name="build-and-run-the-sample"></a>Création et exécution de l’exemple

Dans votre interface de ligne de commande (CLI), accédez au répertoire du projet et exécutez les commandes suivantes.

```Shell
dotnet restore
dotnet build
dotnet run
```
