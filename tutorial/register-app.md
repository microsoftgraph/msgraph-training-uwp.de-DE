<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="def60-101">In dieser Übung erstellen Sie eine neue Azure AD native-Anwendung mithilfe des Azure Active Directory Admin Center.</span><span class="sxs-lookup"><span data-stu-id="def60-101">In this exercise you will create a new Azure AD native application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="def60-102">Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com). Melden Sie sich mit einem **persönlichen Konto** (auch: Microsoft-Konto) oder einem **Geschäfts- oder Schulkonto** an.</span><span class="sxs-lookup"><span data-stu-id="def60-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="def60-103">Wählen Sie **Azure Active Directory** in der linken Navigationsleiste aus, und wählen Sie dann **App** -Registrierungen unter **Manage**aus.</span><span class="sxs-lookup"><span data-stu-id="def60-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="def60-104">Screenshot der APP-Registrierungen</span><span class="sxs-lookup"><span data-stu-id="def60-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="def60-105">Wählen Sie **Neue Registrierung** aus.</span><span class="sxs-lookup"><span data-stu-id="def60-105">Select **New registration**.</span></span> <span data-ttu-id="def60-106">Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.</span><span class="sxs-lookup"><span data-stu-id="def60-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="def60-107">Legen Sie **Name** auf `UWP Graph Tutorial` fest.</span><span class="sxs-lookup"><span data-stu-id="def60-107">Set **Name** to `UWP Graph Tutorial`.</span></span>
    - <span data-ttu-id="def60-108">Legen Sie **Unterstützte Kontotypen** auf **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** fest.</span><span class="sxs-lookup"><span data-stu-id="def60-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="def60-109">Lassen Sie **URI umleiten** leer.</span><span class="sxs-lookup"><span data-stu-id="def60-109">Leave **Redirect URI** empty.</span></span>

    ![Screenshot der Seite "Registrieren einer Anwendung"](./images/aad-register-an-app.png)

1. <span data-ttu-id="def60-111">Wählen Sie **Registrieren** aus.</span><span class="sxs-lookup"><span data-stu-id="def60-111">Choose **Register**.</span></span> <span data-ttu-id="def60-112">Kopieren Sie auf der Seite **UWP Graph Tutorial** den Wert der **Anwendungs-ID (Client)** , und speichern Sie ihn, dann benötigen Sie ihn im nächsten Schritt.</span><span class="sxs-lookup"><span data-stu-id="def60-112">On the **UWP Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Screenshot der Anwendungs-ID der neuen App-Registrierung](./images/aad-application-id.png)

1. <span data-ttu-id="def60-114">Wählen Sie den Link Umleitungs- **URI hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="def60-114">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="def60-115">Suchen Sie auf der Seite **URIs umleiten** nach dem Abschnitt Empfohlene Umleitungs- **URIs für öffentliche Clients (Mobil, Desktop)** .</span><span class="sxs-lookup"><span data-stu-id="def60-115">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="def60-116">Wählen Sie `urn:ietf:wg:oauth:2.0:oob` den URI aus, und klicken Sie dann auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="def60-116">Select the `urn:ietf:wg:oauth:2.0:oob` URI, then choose **Save**.</span></span>

    ![Screenshot der Seite "Umleitungs-URIs"](./images/aad-redirect-uris.png)