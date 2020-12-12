<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung werden Sie das Microsoft Graph in die Anwendung integrieren. Für diese Anwendung verwenden Sie die [Microsoft Graph-Clientbibliothek für .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) , um Anrufe an Microsoft Graph zu tätigen.

## <a name="get-calendar-events-from-outlook"></a>Abrufen von Kalenderereignissen von Outlook

1. Fügen Sie eine neue Seite für die Kalenderansicht hinzu. Klicken Sie mit der rechten Maustaste auf das **GraphTutorial** -Projekt im Projektmappen-Explorer, und wählen Sie **> neues Element hinzufügen** aus. Wählen Sie **leere Seite** aus, geben Sie `CalendarPage.xaml` in das Feld **Name** ein, und wählen Sie **Hinzufügen** aus.

1. Öffnen `CalendarPage.xaml` Sie und fügen Sie die folgende im vorhandenen `<Grid>` Element hinzu.

    ```xaml
    <TextBlock x:Name="Events" TextWrapping="Wrap"/>
    ```

1. Öffnen `CalendarPage.xaml.cs` Sie und fügen Sie die folgenden `using` Anweisungen am Anfang der Datei hinzu.

    ```csharp
    using Microsoft.Graph;
    using Microsoft.Toolkit.Graph.Providers;
    using Microsoft.Toolkit.Uwp.UI.Controls;
    using Newtonsoft.Json;
    ```

1. Fügen Sie der-Klasse die folgenden Funktionen hinzu `CalendarPage` .

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

    Verwenden Sie den Code in `OnNavigatedTo` ist doing.

    - Die URL, die aufgerufen wird, lautet `/me/calendarview`.
        - Die `startDateTime` `endDateTime` Parameter und definieren den Anfang und das Ende der Kalenderansicht.
        - Der `Prefer: outlook.timezone` Header bewirkt `start` , dass die und der `end` Ereignisse in der Zeitzone des Benutzers zurückgegeben werden.
        - Die `Select` Funktion schränkt die für jedes Ereignis zurückgegebenen Felder auf diejenigen ein, die von der APP tatsächlich verwendet werden.
        - Die `OrderBy` -Funktion sortiert die Ergebnisse nach Startdatum und-Uhrzeit.
        - Die `Top` Funktion fordert die meisten 50-Ereignisse an.

1. Ändern `NavView_ItemInvoked` Sie die-Methode in der `MainPage.xaml.cs` Datei, um die vorhandene `switch` Anweisung durch Folgendes zu ersetzen.

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

Sie können nun die app ausführen, sich anmelden und im Menü auf der linken Seite auf das Navigationselement **Kalender** klicken. Es sollte ein JSON-Dump der Ereignisse im Kalender des Benutzers angezeigt werden.

## <a name="display-the-results"></a>Anzeigen der Ergebnisse

1. Ersetzen Sie den gesamten Inhalt `CalendarPage.xaml` durch Folgendes.

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

1. Öffnen Sie `CalendarPage.xaml.cs` die-und ersetzen Sie die- `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` Reihe durch Folgendes.

    ```csharp
    EventList.ItemsSource = events.CurrentPage.ToList();
    ```

    Wenn Sie die APP jetzt ausführen und den Kalender auswählen, sollten Sie eine Liste der Ereignisse in einem Datenraster abrufen. Die **Start** **-und** Endwerte werden jedoch auf nicht benutzerfreundliche Weise angezeigt. Sie können steuern, wie diese Werte mithilfe eines [Wertkonverters](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter)angezeigt werden.

1. Klicken Sie mit der rechten Maustaste auf das **GraphTutorial** -Projekt im Projektmappen-Explorer, und wählen Sie **> Klasse hinzufügen** aus. Nennen Sie die Klasse `GraphDateTimeTimeZoneConverter.cs` , und wählen Sie **Hinzufügen** aus. Ersetzen Sie den gesamten Inhalt der Datei durch den folgenden Code.

    :::code language="csharp" source="../demo/GraphTutorial/GraphDateTimeTimeZoneConverter.cs" id="ConverterSnippet":::

    Dieser Code verwendet die von Microsoft Graph zurückgegebene [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) -Struktur und analysiert sie in ein `DateTimeOffset` Objekt. Anschließend wird der Wert in die Zeitzone des Benutzers konvertiert und der formatierte Wert zurückgegeben.

1. Öffnen `CalendarPage.xaml` Sie, und fügen Sie Folgendes **vor** dem- `<Grid>` Element hinzu.

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="ResourcesSnippet":::

1. Ersetzen Sie die letzten beiden `DataGridTextColumn` Elemente durch Folgendes.

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="BindingSnippet" highlight="4,9":::

1. Führen Sie die APP aus, melden Sie sich an, und klicken Sie auf das Navigationselement **Kalender** . Die Liste der Ereignisse sollte angezeigt werden, wobei die Werte **Start** und **End** formatiert sind.

    ![Ein Screenshot der Tabelle mit Ereignissen](./images/add-msgraph-01.png)
