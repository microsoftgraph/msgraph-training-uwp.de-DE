<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="17aaf-101">In dieser Übung erstellen Sie eine neue Azure AD native-Anwendung mithilfe des Azure Active Directory Admin Center.</span><span class="sxs-lookup"><span data-stu-id="17aaf-101">In this exercise you will create a new Azure AD native application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="17aaf-102">Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com) , und melden Sie sich mit einem **persönlichen Konto** (aka: Microsoft-Konto) oder einem Geschäfts- **oder Schulkonto**an.</span><span class="sxs-lookup"><span data-stu-id="17aaf-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="17aaf-103">Wählen Sie **Azure Active Directory** in der linken Navigationsleiste aus, und wählen Sie dann **App-Registrierungen (Vorschau)** unter **Manage**aus.</span><span class="sxs-lookup"><span data-stu-id="17aaf-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="17aaf-104">Screenshot der APP-Registrierungen</span><span class="sxs-lookup"><span data-stu-id="17aaf-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="17aaf-105">Wählen Sie **neue Registrierung**aus.</span><span class="sxs-lookup"><span data-stu-id="17aaf-105">Select **New registration**.</span></span> <span data-ttu-id="17aaf-106">Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.</span><span class="sxs-lookup"><span data-stu-id="17aaf-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="17aaf-107">Legen \*\*\*\* Sie Name `UWP Graph Tutorial`auf fest.</span><span class="sxs-lookup"><span data-stu-id="17aaf-107">Set **Name** to `UWP Graph Tutorial`.</span></span>
    - <span data-ttu-id="17aaf-108">Legen Sie **unterstützte Kontotypen** auf **Konten in einem beliebigen Organisations Verzeichnis und persönlichen Microsoft-Konten**fest.</span><span class="sxs-lookup"><span data-stu-id="17aaf-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="17aaf-109">Lassen Sie den umLeitungs- **URI** leer.</span><span class="sxs-lookup"><span data-stu-id="17aaf-109">Leave **Redirect URI** empty.</span></span>

    ![Screenshot der Seite "Registrieren einer Anwendung"](./images/aad-register-an-app.png)

1. <span data-ttu-id="17aaf-111">Wählen Sie **registrieren**aus.</span><span class="sxs-lookup"><span data-stu-id="17aaf-111">Choose **Register**.</span></span> <span data-ttu-id="17aaf-112">Kopieren Sie auf der Seite **UWP Graph Tutorial** den Wert der **Anwendungs-ID (Client)** , und speichern Sie ihn, dann benötigen Sie ihn im nächsten Schritt.</span><span class="sxs-lookup"><span data-stu-id="17aaf-112">On the **UWP Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Screenshot der Anwendungs-ID der neuen App-Registrierung](./images/aad-application-id.png)

1. <span data-ttu-id="17aaf-114">Wählen Sie den Link umLeitungs- **URI hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="17aaf-114">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="17aaf-115">Suchen Sie auf der Seite **URIs umleiten** nach dem Abschnitt Empfohlene Umleitungs- **URIs für öffentliche Clients (Mobil, Desktop)** .</span><span class="sxs-lookup"><span data-stu-id="17aaf-115">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="17aaf-116">Wählen Sie `urn:ietf:wg:oauth:2.0:oob` den URI aus, und klicken Sie dann auf **Speichern**.</span><span class="sxs-lookup"><span data-stu-id="17aaf-116">Select the `urn:ietf:wg:oauth:2.0:oob` URI, then choose **Save**.</span></span>

    ![Screenshot der Seite "umLeitungs-URIs"](./images/aad-redirect-uris.png)