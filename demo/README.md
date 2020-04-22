# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="d9d8d-101">Exécution du projet terminé</span><span class="sxs-lookup"><span data-stu-id="d9d8d-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d9d8d-102">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="d9d8d-102">Prerequisites</span></span>

<span data-ttu-id="d9d8d-103">Pour exécuter le projet terminé dans ce dossier, vous avez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="d9d8d-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="d9d8d-104">Le [Kit de développement logiciel .net Core](https://dotnet.microsoft.com/download) installé sur votre ordinateur de développement.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-104">The [.NET Core SDK](https://dotnet.microsoft.com/download) installed on your development machine.</span></span> <span data-ttu-id="d9d8d-105">(**Remarque :** ce didacticiel a été écrit avec .net Core SDK version 2.2.401.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-105">(**Note:** This tutorial was written with .NET Core SDK version 2.2.401.</span></span> <span data-ttu-id="d9d8d-106">Les étapes de ce guide peuvent fonctionner avec d’autres versions, mais cela n’a pas été testé.)</span><span class="sxs-lookup"><span data-stu-id="d9d8d-106">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="d9d8d-107">Un compte professionnel ou scolaire Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-107">A Microsoft work or school account.</span></span>

<span data-ttu-id="d9d8d-108">Si vous n’avez pas de compte Microsoft, vous pouvez vous [inscrire au programme pour les développeurs office 365](https://developer.microsoft.com/office/dev-program) pour obtenir un abonnement gratuit à Office 365.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-108">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="d9d8d-109">Enregistrer une application Web avec le centre d’administration Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d9d8d-109">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="d9d8d-110">Ouvrez un navigateur, accédez au [Centre d’administration Azure Active Directory ](https://aad.portal.azure.com) et connectez-vous à l’aide d’un **compte personnel** (ou compte Microsoft) ou d’un **compte professionnel ou scolaire**.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-110">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="d9d8d-111">Sélectionnez **Azure Active Directory** dans le volet de navigation gauche, puis sélectionnez **Inscriptions d’applications** sous **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-111">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="d9d8d-112">Une capture d’écran des inscriptions d’applications</span><span class="sxs-lookup"><span data-stu-id="d9d8d-112">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="d9d8d-113">Sélectionnez **Nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-113">Select **New registration**.</span></span> <span data-ttu-id="d9d8d-114">Sur la page **Inscrire une application**, définissez les valeurs comme suit.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-114">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="d9d8d-115">Définissez le **Nom** sur `.NET Core Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-115">Set **Name** to `.NET Core Graph Tutorial`.</span></span>
    - <span data-ttu-id="d9d8d-116">Définissez les types de comptes **pris en charge** sur **les comptes dans n’importe quel annuaire d’organisation**.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-116">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="d9d8d-117">Laissez **Redirect URI** vide.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-117">Leave **Redirect URI** empty.</span></span>

    ![Capture d’écran de la page Inscrire une application](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="d9d8d-119">Sélectionner **Inscription**.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-119">Select **Register**.</span></span> <span data-ttu-id="d9d8d-120">Sur la page **didacticiel .net Core Graph** , copiez la valeur de l' **ID d’application (client)** et enregistrez-la, vous en aurez besoin à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-120">On the **.NET Core Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Une capture d’écran de l’ID d’application de la nouvelle inscription d'application](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="d9d8d-122">Sélectionnez le lien **Ajouter un URI de redirection** .</span><span class="sxs-lookup"><span data-stu-id="d9d8d-122">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="d9d8d-123">Sur la page **URI de redirection** , recherchez la section **URI de redirection suggérée pour les clients publics (mobile, bureau)** .</span><span class="sxs-lookup"><span data-stu-id="d9d8d-123">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="d9d8d-124">Sélectionnez l' `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-124">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![Capture d’écran de la page des URI de redirection](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="d9d8d-126">Recherchez la section **type de client par défaut** et modifiez le paramètre **traiter l’application comme un client public** sur **Oui**, puis cliquez sur **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-126">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![Capture d’écran de la section type de client par défaut](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="d9d8d-128">Configurer l’exemple</span><span class="sxs-lookup"><span data-stu-id="d9d8d-128">Configure the sample</span></span>

1. <span data-ttu-id="d9d8d-129">Initialisez le [magasin de secrets de développement .net](https://docs.microsoft.com/aspnet/core/security/app-secrets) en ouvrant votre interface CLI dans le répertoire qui contient **GraphTutorial. csproj** et en exécutant la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-129">Initialize the [.NET development secret store](https://docs.microsoft.com/aspnet/core/security/app-secrets) by opening your CLI in the directory that contains **GraphTutorial.csproj** and running the following command.</span></span>

    ```Shell
    dotnet user-secrets init
    ```

1. <span data-ttu-id="d9d8d-130">Ajoutez votre ID d’application et une liste d’étendues requises au magasin secret à l’aide des commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-130">Add your application ID and a list of required scopes to the secret store using the following commands.</span></span> <span data-ttu-id="d9d8d-131">Remplacez `YOUR_APP_ID_HERE` par l’ID d’application que vous avez créé dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-131">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    ```Shell
    dotnet user-secrets set appId "YOUR_APP_ID_HERE"
    dotnet user-secrets set scopes "User.Read;Calendars.Read"
    ```

## <a name="build-and-run-the-sample"></a><span data-ttu-id="d9d8d-132">Création et exécution de l’exemple</span><span class="sxs-lookup"><span data-stu-id="d9d8d-132">Build and run the sample</span></span>

<span data-ttu-id="d9d8d-133">Dans votre interface de ligne de commande (CLI), accédez au répertoire du projet et exécutez les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="d9d8d-133">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
dotnet restore
dotnet build
dotnet run
```
