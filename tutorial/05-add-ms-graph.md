<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="cc889-101">In dieser Übung werden Sie das Microsoft Graph in die Anwendung integrieren.</span><span class="sxs-lookup"><span data-stu-id="cc889-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="cc889-102">Für diese Anwendung verwenden Sie die [Microsoft Graph-Clientbibliothek für .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) , um Anrufe an Microsoft Graph zu tätigen.</span><span class="sxs-lookup"><span data-stu-id="cc889-102">For this application, you will use the [Microsoft Graph Client Library for .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="cc889-103">Abrufen von Kalenderereignissen von Outlook</span><span class="sxs-lookup"><span data-stu-id="cc889-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="cc889-104">Fügen Sie eine neue Seite für die Kalenderansicht hinzu.</span><span class="sxs-lookup"><span data-stu-id="cc889-104">Add a new page for the calendar view.</span></span> <span data-ttu-id="cc889-105">Klicken Sie mit der rechten Maustaste auf das **GraphTutorial** -Projekt im Projektmappen-Explorer, und wählen Sie **> neues Element hinzufügen** aus. Wählen Sie **leere Seite** aus, geben Sie `CalendarPage.xaml` in das Feld **Name** ein, und wählen Sie **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="cc889-105">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Blank Page**, enter `CalendarPage.xaml` in the **Name** field, and select **Add**.</span></span>

1. <span data-ttu-id="cc889-106">Öffnen `CalendarPage.xaml` Sie und fügen Sie die folgende im vorhandenen `<Grid>` Element hinzu.</span><span class="sxs-lookup"><span data-stu-id="cc889-106">Open `CalendarPage.xaml` and add the following line inside the existing `<Grid>` element.</span></span>

    ```xaml
    <TextBlock x:Name="Events" TextWrapping="Wrap"/>
    ```

1. <span data-ttu-id="cc889-107">Öffnen `CalendarPage.xaml.cs` Sie und fügen Sie die folgenden `using` Anweisungen am Anfang der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="cc889-107">Open `CalendarPage.xaml.cs` and add the following `using` statements at the top of the file.</span></span>

    ```csharp
    using Microsoft.Graph;
    using Microsoft.Toolkit.Graph.Providers;
    using Microsoft.Toolkit.Uwp.UI.Controls;
    using Newtonsoft.Json;
    ```

1. <span data-ttu-id="cc889-108">Fügen Sie der-Klasse die folgenden Funktionen hinzu `CalendarPage` .</span><span class="sxs-lookup"><span data-stu-id="cc889-108">Add the following functions to the `CalendarPage` class.</span></span>

    ```csharp
    private void ShowNotification(string message)
    {
        // Get the main page that contains the InAppNotification
        var mainPage = (Window.Current.Content as Frame).Content as MainPage;

        // Get the notification control
        var notification = mainPage.FindName("Notification") as InAppNotification;

        notification.Show(message);
    }

    protected override async void OnNavigatedTo(NavigationEventArgs e)
    {
        // Get the Graph client from the provider
        var graphClient = ProviderManager.Instance.GlobalProvider.Graph;

        try
        {
            // Get the user's mailbox settings to determine
            // their time zone
            var user = await graphClient.Me.Request()
                .Select(u => new { u.MailboxSettings })
                .GetAsync();

            var startOfWeek = GetUtcStartOfWeekInTimeZone(DateTime.Today, user.MailboxSettings.TimeZone);
            var endOfWeek = startOfWeek.AddDays(7);

            var queryOptions = new List<QueryOption>
            {
                new QueryOption("startDateTime", startOfWeek.ToString("o")),
                new QueryOption("endDateTime", endOfWeek.ToString("o"))
            };

            // Get the events
            var events = await graphClient.Me.CalendarView.Request(queryOptions)
                .Header("Prefer", $"outlook.timezone=\"{user.MailboxSettings.TimeZone}\"")
                .Select(ev => new
                {
                    ev.Subject,
                    ev.Organizer,
                    ev.Start,
                    ev.End
                })
                .OrderBy("start/dateTime")
                .Top(50)
                .GetAsync();

            // TEMPORARY: Show the results as JSON
            Events.Text = JsonConvert.SerializeObject(events.CurrentPage);
        }
        catch (ServiceException ex)
        {
            ShowNotification($"Exception getting events: {ex.Message}");
        }

        base.OnNavigatedTo(e);
    }

    private static DateTime GetUtcStartOfWeekInTimeZone(DateTime today, string timeZoneId)
    {
        TimeZoneInfo userTimeZone = TimeZoneInfo.FindSystemTimeZoneById(timeZoneId);

        // Assumes Sunday as first day of week
        int diff = System.DayOfWeek.Sunday - today.DayOfWeek;

        // create date as unspecified kind
        var unspecifiedStart = DateTime.SpecifyKind(today.AddDays(diff), DateTimeKind.Unspecified);

        // convert to UTC
        return TimeZoneInfo.ConvertTimeToUtc(unspecifiedStart, userTimeZone);
    }
    ```

    <span data-ttu-id="cc889-109">Verwenden Sie den Code in `OnNavigatedTo` ist doing.</span><span class="sxs-lookup"><span data-stu-id="cc889-109">Consider with the code in `OnNavigatedTo` is doing.</span></span>

    - <span data-ttu-id="cc889-110">Die URL, die aufgerufen wird, lautet `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="cc889-110">The URL that will be called is `/me/calendarview`.</span></span>
        - <span data-ttu-id="cc889-111">Die `startDateTime` `endDateTime` Parameter und definieren den Anfang und das Ende der Kalenderansicht.</span><span class="sxs-lookup"><span data-stu-id="cc889-111">The `startDateTime` and `endDateTime` parameters define the start and end of the calendar view.</span></span>
        - <span data-ttu-id="cc889-112">Der `Prefer: outlook.timezone` Header bewirkt `start` , dass die und der `end` Ereignisse in der Zeitzone des Benutzers zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="cc889-112">The `Prefer: outlook.timezone` header causes the `start` and `end` of the events to be returned in the user's time zone.</span></span>
        - <span data-ttu-id="cc889-113">Die `Select` Funktion schränkt die für jedes Ereignis zurückgegebenen Felder auf diejenigen ein, die von der APP tatsächlich verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cc889-113">The `Select` function limits the fields returned for each event to just those the app will actually use.</span></span>
        - <span data-ttu-id="cc889-114">Die `OrderBy` -Funktion sortiert die Ergebnisse nach Startdatum und-Uhrzeit.</span><span class="sxs-lookup"><span data-stu-id="cc889-114">The `OrderBy` function sorts the results by the start date and time.</span></span>
        - <span data-ttu-id="cc889-115">Die `Top` Funktion fordert die meisten 50-Ereignisse an.</span><span class="sxs-lookup"><span data-stu-id="cc889-115">The `Top` function requests at most 50 events.</span></span>

1. <span data-ttu-id="cc889-116">Ändern `NavView_ItemInvoked` Sie die-Methode in der `MainPage.xaml.cs` Datei, um die vorhandene `switch` Anweisung durch Folgendes zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="cc889-116">Modify the `NavView_ItemInvoked` method in the `MainPage.xaml.cs` file to replace the existing `switch` statement with the following.</span></span>

    ```csharp
    switch (invokedItem.ToLower())
    {
        case "new event":
            throw new NotImplementedException();
            break;
        case "calendar":
            RootFrame.Navigate(typeof(CalendarPage));
            break;
        case "home":
        default:
            RootFrame.Navigate(typeof(HomePage));
            break;
    }
    ```

<span data-ttu-id="cc889-117">Sie können nun die app ausführen, sich anmelden und im Menü auf der linken Seite auf das Navigationselement **Kalender** klicken.</span><span class="sxs-lookup"><span data-stu-id="cc889-117">You can now run the app, sign in, and click the **Calendar** navigation item in the left-hand menu.</span></span> <span data-ttu-id="cc889-118">Es sollte ein JSON-Dump der Ereignisse im Kalender des Benutzers angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="cc889-118">You should see a JSON dump of the events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="cc889-119">Anzeigen der Ergebnisse</span><span class="sxs-lookup"><span data-stu-id="cc889-119">Display the results</span></span>

1. <span data-ttu-id="cc889-120">Ersetzen Sie den gesamten Inhalt `CalendarPage.xaml` durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="cc889-120">Replace the entire contents of `CalendarPage.xaml` with the following.</span></span>

    ```xaml
    <Page
        x:Class="GraphTutorial.CalendarPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:GraphTutorial"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:controls="using:Microsoft.Toolkit.Uwp.UI.Controls"
        mc:Ignorable="d"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <Grid>
            <controls:DataGrid x:Name="EventList" Grid.Row="1"
                    AutoGenerateColumns="False">
                <controls:DataGrid.Columns>
                    <controls:DataGridTextColumn
                            Header="Organizer"
                            Width="SizeToCells"
                            Binding="{Binding Organizer.EmailAddress.Name}"
                            FontSize="20" />
                    <controls:DataGridTextColumn
                            Header="Subject"
                            Width="SizeToCells"
                            Binding="{Binding Subject}"
                            FontSize="20" />
                    <controls:DataGridTextColumn
                            Header="Start"
                            Width="SizeToCells"
                            Binding="{Binding Start.DateTime}"
                            FontSize="20" />
                    <controls:DataGridTextColumn
                            Header="End"
                            Width="SizeToCells"
                            Binding="{Binding End.DateTime}"
                            FontSize="20" />
                </controls:DataGrid.Columns>
            </controls:DataGrid>
        </Grid>
    </Page>
    ```

1. <span data-ttu-id="cc889-121">Öffnen Sie `CalendarPage.xaml.cs` die-und ersetzen Sie die- `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` Reihe durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="cc889-121">Open `CalendarPage.xaml.cs` and replace the `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` line with the following.</span></span>

    ```csharp
    EventList.ItemsSource = events.CurrentPage.ToList();
    ```

    <span data-ttu-id="cc889-122">Wenn Sie die APP jetzt ausführen und den Kalender auswählen, sollten Sie eine Liste der Ereignisse in einem Datenraster abrufen.</span><span class="sxs-lookup"><span data-stu-id="cc889-122">If you run the app now and select the calendar, you should get a list of events in a data grid.</span></span> <span data-ttu-id="cc889-123">Die **Start** **-und** Endwerte werden jedoch auf nicht benutzerfreundliche Weise angezeigt.</span><span class="sxs-lookup"><span data-stu-id="cc889-123">However, the **Start** and **End** values are displayed in a non-user-friendly manner.</span></span> <span data-ttu-id="cc889-124">Sie können steuern, wie diese Werte mithilfe eines [Wertkonverters](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter)angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="cc889-124">You can control how those values are displayed by using a [value converter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).</span></span>

1. <span data-ttu-id="cc889-125">Klicken Sie mit der rechten Maustaste auf das **GraphTutorial** -Projekt im Projektmappen-Explorer, und wählen Sie **> Klasse hinzufügen** aus. Nennen Sie die Klasse `GraphDateTimeTimeZoneConverter.cs` , und wählen Sie **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="cc889-125">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > Class...**. Name the class `GraphDateTimeTimeZoneConverter.cs` and select **Add**.</span></span> <span data-ttu-id="cc889-126">Ersetzen Sie den gesamten Inhalt der Datei durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="cc889-126">Replace the entire contents of the file with the following.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/GraphDateTimeTimeZoneConverter.cs" id="ConverterSnippet":::

    <span data-ttu-id="cc889-127">Dieser Code verwendet die von Microsoft Graph zurückgegebene [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) -Struktur und analysiert sie in ein `DateTimeOffset` Objekt.</span><span class="sxs-lookup"><span data-stu-id="cc889-127">This code takes the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) structure returned by Microsoft Graph and parses it into a `DateTimeOffset` object.</span></span> <span data-ttu-id="cc889-128">Anschließend wird der Wert in die Zeitzone des Benutzers konvertiert und der formatierte Wert zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="cc889-128">It then converts the value into the user's time zone and returns the formatted value.</span></span>

1. <span data-ttu-id="cc889-129">Öffnen `CalendarPage.xaml` Sie, und fügen Sie Folgendes **vor** dem- `<Grid>` Element hinzu.</span><span class="sxs-lookup"><span data-stu-id="cc889-129">Open `CalendarPage.xaml` and add the following **before** the `<Grid>` element.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="ResourcesSnippet":::

1. <span data-ttu-id="cc889-130">Ersetzen Sie die letzten beiden `DataGridTextColumn` Elemente durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="cc889-130">Replace the last two `DataGridTextColumn` elements with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="BindingSnippet" highlight="4,9":::

1. <span data-ttu-id="cc889-131">Führen Sie die APP aus, melden Sie sich an, und klicken Sie auf das Navigationselement **Kalender** .</span><span class="sxs-lookup"><span data-stu-id="cc889-131">Run the app, sign in, and click the **Calendar** navigation item.</span></span> <span data-ttu-id="cc889-132">Die Liste der Ereignisse sollte angezeigt werden, wobei die Werte **Start** und **End** formatiert sind.</span><span class="sxs-lookup"><span data-stu-id="cc889-132">You should see the list of events with the **Start** and **End** values formatted.</span></span>

    ![Ein Screenshot der Tabelle mit Ereignissen](./images/add-msgraph-01.png)
