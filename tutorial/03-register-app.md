<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="fdff8-101">Dans cet exercice, vous allez créer une nouvelle application Azure AD à l’aide du centre d’administration Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fdff8-101">In this exercise you will create a new Azure AD application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="fdff8-102">Ouvrez un navigateur, accédez au [Centre d’administration Azure Active Directory ](https://aad.portal.azure.com) et connectez-vous à l’aide d’un **compte personnel** (ou compte Microsoft) ou d’un **compte professionnel ou scolaire**.</span><span class="sxs-lookup"><span data-stu-id="fdff8-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="fdff8-103">Sélectionnez **Azure Active Directory** dans le volet de navigation gauche, puis sélectionnez **Inscriptions d’applications** sous **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="fdff8-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="fdff8-104">Une capture d’écran des inscriptions d’applications</span><span class="sxs-lookup"><span data-stu-id="fdff8-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="fdff8-105">Sélectionnez **Nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="fdff8-105">Select **New registration**.</span></span> <span data-ttu-id="fdff8-106">Sur la page **Inscrire une application**, définissez les valeurs comme suit.</span><span class="sxs-lookup"><span data-stu-id="fdff8-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="fdff8-107">Définissez le **Nom** sur `.NET Core Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="fdff8-107">Set **Name** to `.NET Core Graph Tutorial`.</span></span>
    - <span data-ttu-id="fdff8-108">Définissez les **Types de comptes pris en charge** sur **Comptes dans un annuaire organisationnel et comptes personnels Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="fdff8-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="fdff8-109">Sous **URI de redirection**, remplacez la liste déroulante par **client Public (mobile & Desktop)** et définissez `https://login.microsoftonline.com/common/oauth2/nativeclient`la valeur sur.</span><span class="sxs-lookup"><span data-stu-id="fdff8-109">Under **Redirect URI**, change the dropdown to **Public client (mobile & desktop)**, and set the value to `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span></span>

    ![Capture d’écran de la page Inscrire une application](./images/aad-register-an-app.png)

1. <span data-ttu-id="fdff8-111">Sélectionner **Inscription**.</span><span class="sxs-lookup"><span data-stu-id="fdff8-111">Select **Register**.</span></span> <span data-ttu-id="fdff8-112">Sur la page **didacticiel .net Core Graph** , copiez la valeur de l' **ID d’application (client)** et enregistrez-la, vous en aurez besoin à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="fdff8-112">On the **.NET Core Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Une capture d’écran de l’ID d’application de la nouvelle inscription d'application](./images/aad-application-id.png)

1. <span data-ttu-id="fdff8-114">Sous **Gérer**, sélectionnez **Authentification**.</span><span class="sxs-lookup"><span data-stu-id="fdff8-114">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="fdff8-115">Recherchez la section **Paramètres avancés** et modifiez le paramètre **traiter l’application en tant que client public** sur **Oui**, puis choisissez **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="fdff8-115">Locate the **Advanced settings** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![Capture d’écran de la section type de client par défaut](./images/aad-default-client-type.png)
