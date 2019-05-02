<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7bcfb-101">In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung zur Unterstützung der Authentifizierung mit Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="7bcfb-102">Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken für den Aufruf von Microsoft Graph abzurufen.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="7bcfb-103">In diesem Schritt integrieren Sie das [AadLogin](https://docs.microsoft.com/dotnet/api/microsoft.toolkit.uwp.ui.controls.graph.aadlogin?view=win-comm-toolkit-dotnet-stable) -Steuerelement aus dem [Windows-Community-Toolkit](https://github.com/Microsoft/WindowsCommunityToolkit) in die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-103">In this step you will integrate the [AadLogin](https://docs.microsoft.com/dotnet/api/microsoft.toolkit.uwp.ui.controls.graph.aadlogin?view=win-comm-toolkit-dotnet-stable) control from the [Windows Community Toolkit](https://github.com/Microsoft/WindowsCommunityToolkit) into the application.</span></span>

<span data-ttu-id="7bcfb-104">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das **Graph-Tutorial-** Projekt, und wählen Sie **> neues Element hinzufügen...**. Wählen Sie **Ressourcendatei (. resw)** aus, benennen `OAuth.resw` Sie die Datei, und wählen Sie **Hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-104">Right-click the **graph-tutorial** project in Solution Explorer and choose **Add > New Item...**. Choose **Resources File (.resw)**, name the file `OAuth.resw` and choose **Add**.</span></span> <span data-ttu-id="7bcfb-105">Wenn die neue Datei in Visual Studio geöffnet wird, erstellen Sie zwei Ressourcen wie folgt.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-105">When the new file opens in Visual Studio, create two resources as follows.</span></span>

- <span data-ttu-id="7bcfb-106">**Name:** `AppId`, **Value:** die APP-ID, die Sie im Anwendungs Registrierungs Portal generiert haben</span><span class="sxs-lookup"><span data-stu-id="7bcfb-106">**Name:** `AppId`, **Value:** the app ID you generated in Application Registration Portal</span></span>
- <span data-ttu-id="7bcfb-107">**Name:** `Scopes`, **Wert:**`User.Read Calendars.Read`</span><span class="sxs-lookup"><span data-stu-id="7bcfb-107">**Name:** `Scopes`, **Value:** `User.Read Calendars.Read`</span></span>

![Screenshot der OAuth. resw-Datei im Visual Studio-Editor](./images/edit-resources-01.png)

> [!IMPORTANT]
> <span data-ttu-id="7bcfb-109">Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es jetzt ein guter Zeitpunkt, die Datei `OAuth.resw` aus der Quellcodeverwaltung auszuschließen, um versehentlich Ihre APP-ID zu vernichten.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-109">If you're using source control such as git, now would be a good time to exclude the `OAuth.resw` file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="configure-the-aadlogin-control"></a><span data-ttu-id="7bcfb-110">Konfigurieren des AadLogin-Steuerelements</span><span class="sxs-lookup"><span data-stu-id="7bcfb-110">Configure the AadLogin control</span></span>

<span data-ttu-id="7bcfb-111">Beginnen Sie mit dem Hinzufügen von Code, um die Werte aus der Ressourcendatei zu lesen.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-111">Start by adding code to read the values out of the resources file.</span></span> <span data-ttu-id="7bcfb-112">Öffnen `MainPage.xaml.cs` Sie und fügen Sie `using` die folgende Anweisung am Anfang der Datei.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-112">Open `MainPage.xaml.cs` and add the following `using` statement to the top of the file.</span></span>

```cs
using Microsoft.Toolkit.Services.MicrosoftGraph;
```

<span data-ttu-id="7bcfb-113">Ersetzen Sie die Zeile `RootFrame.Navigate(typeof(HomePage));` durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-113">Replace the `RootFrame.Navigate(typeof(HomePage));` line with the following code.</span></span>

```cs
// Load OAuth settings
var oauthSettings = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("OAuth");
var appId = oauthSettings.GetString("AppId");
var scopes = oauthSettings.GetString("Scopes");

if (string.IsNullOrEmpty(appId) || string.IsNullOrEmpty(scopes))
{
    Notification.Show("Could not load OAuth Settings from resource file.");
}
else
{
    // Initialize Graph
    MicrosoftGraphService.Instance.AuthenticationModel = MicrosoftGraphEnums.AuthenticationModel.V2;
    MicrosoftGraphService.Instance.Initialize(appId,
        MicrosoftGraphEnums.ServicesToInitialize.UserProfile,
        scopes.Split(' '));

    // Navigate to HomePage.xaml
    RootFrame.Navigate(typeof(HomePage));
}
```

<span data-ttu-id="7bcfb-114">Dieser Code lädt die Einstellungen aus `OAuth.resw` und initialisiert die globale Instanz der `MicrosoftGraphService` mit diesen Werten.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-114">This code loads the settings from `OAuth.resw` and initializes the global instance of the `MicrosoftGraphService` with those values.</span></span>

<span data-ttu-id="7bcfb-115">Fügen Sie nun einen Ereignishandler für `SignInCompleted` das Ereignis im `AadLogin` Steuerelement hinzu.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-115">Now add an event handler for the `SignInCompleted` event on the `AadLogin` control.</span></span> <span data-ttu-id="7bcfb-116">Öffnen Sie `MainPage.xaml` die Datei, und ersetzen `<graphControls:AadLogin>` Sie das vorhandene Element durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-116">Open the `MainPage.xaml` file and replace the existing `<graphControls:AadLogin>` element with the following.</span></span>

```xml
<graphControls:AadLogin x:Name="Login"
    HorizontalAlignment="Left"
    View="SmallProfilePhotoLeft"
    AllowSignInAsDifferentUser="False"
    SignInCompleted="Login_SignInCompleted"
    SignOutCompleted="Login_SignOutCompleted"
    />
```

<span data-ttu-id="7bcfb-117">Fügen Sie dann die folgenden Funktionen zur `MainPage` -Klasse `MainPage.xaml.cs`in hinzu.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-117">Then add the following functions to the `MainPage` class in `MainPage.xaml.cs`.</span></span>

```cs
private void Login_SignInCompleted(object sender, Microsoft.Toolkit.Uwp.UI.Controls.Graph.SignInEventArgs e)
{
    // Set the auth state
    SetAuthState(true);
    // Reload the home page
    RootFrame.Navigate(typeof(HomePage));
}

private void Login_SignOutCompleted(object sender, EventArgs e)
{
    // Set the auth state
    SetAuthState(false);
    // Reload the home page
    RootFrame.Navigate(typeof(HomePage));
}
```

<span data-ttu-id="7bcfb-118">Erweitern Sie schließlich im Projektmappen-Explorer die Option **Homepage. XAML** und öffnen `HomePage.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-118">Finally, in Solution Explorer, expand **HomePage.xaml** and open `HomePage.xaml.cs`.</span></span> <span data-ttu-id="7bcfb-119">Fügen Sie den folgenden Code nach `this.InitializeComponent();` der Leitung hinzu.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-119">Add the following code after the `this.InitializeComponent();` line.</span></span>

```cs
if ((App.Current as App).IsAuthenticated)
{
    HomePageMessage.Text = "Welcome! Please use the menu to the left to select a view.";
}
```

<span data-ttu-id="7bcfb-120">Starten Sie die APP neu, und klicken Sie oben in der APP auf das **Anmelde** Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-120">Restart the app and click the **Sign In** control at the top of the app.</span></span> <span data-ttu-id="7bcfb-121">Nachdem Sie sich angemeldet haben, sollte sich die Benutzeroberfläche ändern, um anzugeben, dass Sie sich erfolgreich angemeldet haben.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-121">Once you've signed in, the UI should change to indicate that you've successfully signed-in.</span></span>

![Screenshot der APP nach der Anmeldung](./images/add-aad-auth-01.png)

> [!NOTE]
> <span data-ttu-id="7bcfb-123">Das `AadLogin` Steuerelement implementiert die Logik zum Speichern und Aktualisieren des Zugriffstokens für Sie.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-123">The `AadLogin` control implements the logic of storing and refreshing the access token for you.</span></span> <span data-ttu-id="7bcfb-124">Die Token werden in Secure Storage gespeichert und bei Bedarf aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="7bcfb-124">The tokens are stored in secure storage and refreshed as needed.</span></span>