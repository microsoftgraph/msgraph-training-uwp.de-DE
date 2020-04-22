<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="e6cc3-101">In diesem Abschnitt erstellen Sie eine neue UWP-app.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-101">In this section you'll create a new UWP app.</span></span>

1. <span data-ttu-id="e6cc3-102">Öffnen Sie Visual Studio, und wählen Sie **Neues Projekt erstellen**.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-102">Open Visual Studio, and select **Create a new project**.</span></span> <span data-ttu-id="e6cc3-103">Wählen Sie die Option **leere app (universelle Windows)** aus, die C# verwendet, und wählen Sie dann **weiter**aus.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-103">Choose the **Blank App (Universal Windows)** option that uses C#, then select **Next**.</span></span>

    ![Visual Studio 2019 Dialogfeld "Neues Projekt erstellen"](./images/vs-create-new-project.png)

1. <span data-ttu-id="e6cc3-105">Geben Sie im Dialogfeld **Neues Projekt konfigurieren** in `GraphTutorial` das Feld **Projektname** ein, und wählen Sie dann **Erstellen**aus.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-105">In the **Configure your new project** dialog, enter `GraphTutorial` in the **Project name** field and select **Create**.</span></span>

    ![Visual Studio 2019 Dialogfeld "Neues Projekt konfigurieren"](./images/vs-configure-new-project.png)

    > [!IMPORTANT]
    > <span data-ttu-id="e6cc3-107">Stellen Sie sicher, dass Sie genau den gleichen Namen für das Visual Studio Projekt eingeben, das in diesen Übungseinheiten angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-107">Ensure that you enter the exact same name for the Visual Studio Project that is specified in these lab instructions.</span></span> <span data-ttu-id="e6cc3-108">Der Visual Studio-Projektname wird Teil des Namespace im Code.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-108">The Visual Studio Project name becomes part of the namespace in the code.</span></span> <span data-ttu-id="e6cc3-109">Der Code in diesen Anweisungen hängt vom Namespace ab, der mit dem in diesen Anweisungen angegebenen Visual Studio-Projektnamen übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-109">The code inside these instructions depends on the namespace matching the Visual Studio Project name specified in these instructions.</span></span> <span data-ttu-id="e6cc3-110">Wenn Sie einen anderen Projektnamen verwenden, wird der Code nicht kompiliert, es sei denn, Sie passen alle Namespaces so an, dass Sie dem von Ihnen beim Erstellen des Projekts eingegebenen Visual Studio-Projektnamen entsprechen.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-110">If you use a different project name the code will not compile unless you adjust all the namespaces to match the Visual Studio Project name you enter when you create the project.</span></span>

1. <span data-ttu-id="e6cc3-111">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-111">Select **OK**.</span></span> <span data-ttu-id="e6cc3-112">Stellen Sie im Dialogfeld **Neues universelles Windows-Platt Form Projekt** sicher, dass die `Windows 10, Version 1809 (10.0; Build 17763)` **minimale Version** auf oder höher festgelegt ist, und wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-112">In the **New Universal Windows Platform Project** dialog, ensure that the **Minimum version** is set to `Windows 10, Version 1809 (10.0; Build 17763)` or later and select **OK**.</span></span>

## <a name="install-nuget-packages"></a><span data-ttu-id="e6cc3-113">Installieren der NuGet-Pakete</span><span class="sxs-lookup"><span data-stu-id="e6cc3-113">Install NuGet packages</span></span>

<span data-ttu-id="e6cc3-114">Bevor Sie fortfahren, installieren Sie einige zusätzliche NuGet-Pakete, die Sie später verwenden werden.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-114">Before moving on, install some additional NuGet packages that you will use later.</span></span>

- <span data-ttu-id="e6cc3-115">[Microsoft. Toolkit. UWP. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls/) zum Hinzufügen einiger UI-Steuerelemente für in-App-Benachrichtigungen und lade Indikatoren.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-115">[Microsoft.Toolkit.Uwp.Ui.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls/) to add some UI controls for in-app notifications and loading indicators.</span></span>
- <span data-ttu-id="e6cc3-116">[Microsoft. Toolkit. UWP. UI. Controls. DataGrid](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid/) zum Anzeigen der von Microsoft Graph zurückgegebenen Informationen.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-116">[Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid/) to display the information returned by Microsoft Graph.</span></span>
- <span data-ttu-id="e6cc3-117">[Microsoft. Toolkit. Graph. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Graph.Controls) zum Behandeln von Anmelde-und Zugriffstoken abrufen.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-117">[Microsoft.Toolkit.Graph.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Graph.Controls) to handle login and access token retrieval.</span></span>

1. <span data-ttu-id="e6cc3-118">Wählen Sie **Extras > NuGet-Paket-Manager > Paket-Manager-Konsole**aus.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-118">Select **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="e6cc3-119">Geben Sie in der Paket-Manager-Konsole die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="e6cc3-119">In the Package Manager Console, enter the following commands.</span></span>

    ```powershell
    Install-Package Microsoft.Toolkit.Uwp.Ui.Controls -Version 6.0.0
    Install-Package Microsoft.Toolkit.Uwp.Ui.Controls.DataGrid -Version 6.0.0
    Install-Package Microsoft.Toolkit.Graph.Controls -IncludePrerelease
    ```

## <a name="design-the-app"></a><span data-ttu-id="e6cc3-120">Entwerfen der App</span><span class="sxs-lookup"><span data-stu-id="e6cc3-120">Design the app</span></span>

<span data-ttu-id="e6cc3-121">In diesem Abschnitt erstellen Sie die Benutzeroberfläche für die app.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-121">In this section you'll create the UI for the app.</span></span>

1. <span data-ttu-id="e6cc3-122">Fügen Sie zunächst eine Variable auf Anwendungsebene hinzu, um den Authentifizierungsstatus nachzuverfolgen.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-122">Start by adding an application-level variable to track authentication state.</span></span> <span data-ttu-id="e6cc3-123">Erweitern Sie im Projektmappen-Explorer den Knoten **app. XAML** , und öffnen Sie **app.XAML.cs**.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-123">In Solution Explorer, expand **App.xaml** and open **App.xaml.cs**.</span></span> <span data-ttu-id="e6cc3-124">Fügen Sie der `App`-Klasse die folgende Eigenschaft hinzu.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-124">Add the following property to the `App` class.</span></span>

    ```csharp
    public bool IsAuthenticated { get; set; }
    ```

1. <span data-ttu-id="e6cc3-125">Definieren Sie das Layout für die Hauptseite.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-125">Define the layout for the main page.</span></span> <span data-ttu-id="e6cc3-126">Öffnen `MainPage.xaml` Sie den gesamten Inhalt, und ersetzen Sie ihn durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-126">Open `MainPage.xaml` and replace its entire contents with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/MainPage.xaml" id="MainPageXamlSnippet":::

    <span data-ttu-id="e6cc3-127">Dies definiert eine grundlegende [NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview) mit **Start** -und **Kalender** Navigationslinks, die als Hauptansicht der APP fungieren.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-127">This defines a basic [NavigationView](/uwp/api/windows.ui.xaml.controls.navigationview) with **Home** and **Calendar** navigation links to act as the main view of the app.</span></span> <span data-ttu-id="e6cc3-128">Außerdem wird ein [LoginButton](https://github.com/windows-toolkit/Graph-Controls) -Steuerelement in der Kopfzeile der Ansicht hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-128">It also adds a [LoginButton](https://github.com/windows-toolkit/Graph-Controls) control in the header of the view.</span></span> <span data-ttu-id="e6cc3-129">Mit diesem Steuerelement kann sich der Benutzer an-und abmelden. Das Steuerelement ist noch nicht vollständig aktiviert, Sie werden es in einer späteren Übung konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-129">That control will allow the user to sign in and out. The control isn't fully enabled yet, you will configure it in a later exercise.</span></span>

1. <span data-ttu-id="e6cc3-130">Klicken Sie mit der rechten Maustaste auf das **Graph-Tutorial-** Projekt im Projektmappen-Explorer, und wählen Sie **> neues Element hinzufügen aus...**. Wählen Sie **leere Seite**aus `HomePage.xaml` , geben Sie in das Feld **Name** ein, und wählen Sie **Hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-130">Right-click the **graph-tutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Blank Page**, enter `HomePage.xaml` in the **Name** field, and select **Add**.</span></span> <span data-ttu-id="e6cc3-131">Ersetzen Sie das `<Grid>` vorhandene Element in der Datei durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-131">Replace the existing `<Grid>` element in the file with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/HomePage.xaml" id="HomePageGridSnippet" highlight="2-5":::

1. <span data-ttu-id="e6cc3-132">Erweitern Sie Haupt **. XAML** im Projektmappen `MainPage.xaml.cs`-Explorer, und öffnen Sie.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-132">Expand **MainPage.xaml** in Solution Explorer and open `MainPage.xaml.cs`.</span></span> <span data-ttu-id="e6cc3-133">Fügen Sie der `MainPage` Klasse die folgende Funktion hinzu, um den Authentifizierungsstatus zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-133">Add the following function to the `MainPage` class to manage authentication state.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="SetAuthStateSnippet":::

1. <span data-ttu-id="e6cc3-134">Fügen Sie dem `MainPage()` Konstruktor **nach** der `this.InitializeComponent();` -Verbindung den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-134">Add the following code to the `MainPage()` constructor **after** the `this.InitializeComponent();` line.</span></span>

    ```csharp
    // Initialize auth state to false
    SetAuthState(false);

    // Configure MSAL provider
    // TEMPORARY
    MsalProvider.ClientId = "11111111-1111-1111-1111-111111111111";

    // Navigate to HomePage.xaml
    RootFrame.Navigate(typeof(HomePage));
    ```

    <span data-ttu-id="e6cc3-135">Wenn die APP erstmalig gestartet wird, wird der Authentifizierungsstatus in `false` initialisiert und zur Startseite navigiert.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-135">When the app first starts, it will initialize the authentication state to `false` and navigate to the home page.</span></span>

1. <span data-ttu-id="e6cc3-136">Fügen Sie den folgenden Ereignishandler hinzu, um die angeforderte Seite zu laden, wenn der Benutzer ein Element aus der Navigationsansicht auswählt.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-136">Add the following event handler to load the requested page when the user selects an item from the navigation view.</span></span>

    ```csharp
    private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
    {
        var invokedItem = args.InvokedItem as string;

        switch (invokedItem.ToLower())
        {
            case "calendar":
                throw new NotImplementedException();
                break;
            case "home":
            default:
                RootFrame.Navigate(typeof(HomePage));
                break;
        }
    }
    ```

1. <span data-ttu-id="e6cc3-137">Speichern Sie alle Änderungen, drücken Sie **F5** , oder wählen Sie **Debug > starten des Debuggings** in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e6cc3-137">Save all of your changes, then press **F5** or select **Debug > Start Debugging** in Visual Studio.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e6cc3-138">Stellen Sie sicher, dass Sie die entsprechende Konfiguration für Ihren Computer auswählen (Arm, x64, x86).</span><span class="sxs-lookup"><span data-stu-id="e6cc3-138">Make sure you select the appropriate configuration for your machine (ARM, x64, x86).</span></span>

    ![Screenshot der Homepage](./images/create-app-01.png)
