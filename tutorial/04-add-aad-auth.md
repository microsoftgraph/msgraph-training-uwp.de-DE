<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="102b8-101">In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="102b8-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="102b8-102">Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken zum Aufrufen von Microsoft Graph zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="102b8-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="102b8-103">In diesem Schritt werden Sie das **LoginButton** -Steuerelement aus den [Windows Graph-Steuerelementen](https://github.com/windows-toolkit/Graph-Controls) in die Anwendung integrieren.</span><span class="sxs-lookup"><span data-stu-id="102b8-103">In this step you will integrate the **LoginButton** control from the [Windows Graph Controls](https://github.com/windows-toolkit/Graph-Controls) into the application.</span></span>

1. <span data-ttu-id="102b8-104">Klicken Sie mit der rechten Maustaste auf das **GraphTutorial** -Projekt im Projektmappen-Explorer, und wählen Sie **> neues Element hinzufügen** aus. Wählen Sie **Ressourcendatei (. resw)**, benennen Sie die Datei, `OAuth.resw` und wählen Sie **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="102b8-104">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Resources File (.resw)**, name the file `OAuth.resw` and select **Add**.</span></span> <span data-ttu-id="102b8-105">Wenn die neue Datei in Visual Studio geöffnet wird, erstellen Sie zwei Ressourcen wie folgt.</span><span class="sxs-lookup"><span data-stu-id="102b8-105">When the new file opens in Visual Studio, create two resources as follows.</span></span>

    - <span data-ttu-id="102b8-106">**Name:** `AppId` , **Wert:** die APP-ID, die Sie im Anwendungs Registrierungs Portal generiert haben.</span><span class="sxs-lookup"><span data-stu-id="102b8-106">**Name:** `AppId`, **Value:** the app ID you generated in Application Registration Portal</span></span>
    - <span data-ttu-id="102b8-107">**Name:** `Scopes` , **Wert:**`User.Read User.ReadBasic.All People.Read MailboxSettings.Read Calendars.ReadWrite`</span><span class="sxs-lookup"><span data-stu-id="102b8-107">**Name:** `Scopes`, **Value:** `User.Read User.ReadBasic.All People.Read MailboxSettings.Read Calendars.ReadWrite`</span></span>

    ![Ein Screenshot der Datei OAuth. resw im Visual Studio-Editor](./images/edit-resources-01.png)

    > [!IMPORTANT]
    > <span data-ttu-id="102b8-109">Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es jetzt ein guter Zeitpunkt, die `OAuth.resw` Datei aus der Quellcodeverwaltung auszuschließen, um unbeabsichtigtes Auslaufen ihrer APP-ID zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="102b8-109">If you're using source control such as git, now would be a good time to exclude the `OAuth.resw` file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="configure-the-loginbutton-control"></a><span data-ttu-id="102b8-110">Konfigurieren des LoginButton-Steuerelements</span><span class="sxs-lookup"><span data-stu-id="102b8-110">Configure the LoginButton control</span></span>

1. <span data-ttu-id="102b8-111">Öffnen `MainPage.xaml.cs` Sie und fügen Sie die folgende `using` Anweisung am Anfang der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="102b8-111">Open `MainPage.xaml.cs` and add the following `using` statement to the top of the file.</span></span>

    ```csharp
    using Microsoft.Toolkit.Graph.Providers;
    ```

1. <span data-ttu-id="102b8-112">Ersetzen Sie den vorhandenen Konstruktor durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="102b8-112">Replace the existing constructor with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="ConstructorSnippet":::

    <span data-ttu-id="102b8-113">Dieser Code lädt die Einstellungen aus `OAuth.resw` und initialisiert den MSAL-Anbieter mit diesen Werten.</span><span class="sxs-lookup"><span data-stu-id="102b8-113">This code loads the settings from `OAuth.resw` and initializes the MSAL provider with those values.</span></span>

1. <span data-ttu-id="102b8-114">Fügen Sie nun einen Ereignishandler für das `ProviderUpdated` Ereignis auf der hinzu `ProviderManager` .</span><span class="sxs-lookup"><span data-stu-id="102b8-114">Now add an event handler for the `ProviderUpdated` event on the `ProviderManager`.</span></span> <span data-ttu-id="102b8-115">Fügen Sie die folgende Funktion zur `MainPage`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="102b8-115">Add the following function to the `MainPage` class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="ProviderUpdatedSnippet":::

    <span data-ttu-id="102b8-116">Dieses Ereignis wird ausgelöst, wenn sich der Anbieter ändert oder wenn sich der Anbieterstatus ändert.</span><span class="sxs-lookup"><span data-stu-id="102b8-116">This event triggers when the provider changes, or when the provider state changes.</span></span>

1. <span data-ttu-id="102b8-117">Erweitern Sie im Projektmappen-Explorer **Homepage. XAML** , und öffnen Sie `HomePage.xaml.cs` .</span><span class="sxs-lookup"><span data-stu-id="102b8-117">In Solution Explorer, expand **HomePage.xaml** and open `HomePage.xaml.cs`.</span></span> <span data-ttu-id="102b8-118">Ersetzen Sie den vorhandenen Konstruktor durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="102b8-118">Replace the existing constructor with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/HomePage.xaml.cs" id="ConstructorSnippet":::

1. <span data-ttu-id="102b8-119">Starten Sie die APP neu, und klicken Sie oben in der APP auf das **Anmelde** Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="102b8-119">Restart the app and click the **Sign In** control at the top of the app.</span></span> <span data-ttu-id="102b8-120">Nachdem Sie sich angemeldet haben, sollte die Benutzeroberfläche geändert werden, um anzugeben, dass Sie sich erfolgreich angemeldet haben.</span><span class="sxs-lookup"><span data-stu-id="102b8-120">Once you've signed in, the UI should change to indicate that you've successfully signed-in.</span></span>

    ![Ein Screenshot der APP nach der Anmeldung](./images/add-aad-auth-01.png)

    > [!NOTE]
    > <span data-ttu-id="102b8-122">Das `ButtonLogin` Steuerelement implementiert die Logik des Speicherns und Aktualisierens des Zugriffstokens für Sie.</span><span class="sxs-lookup"><span data-stu-id="102b8-122">The `ButtonLogin` control implements the logic of storing and refreshing the access token for you.</span></span> <span data-ttu-id="102b8-123">Die Token werden im sicheren Speicher gespeichert und bei Bedarf aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="102b8-123">The tokens are stored in secure storage and refreshed as needed.</span></span>
