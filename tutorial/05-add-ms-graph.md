<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung werden Sie das Microsoft Graph in die Anwendung integrieren. Für diese Anwendung verwenden Sie die [Microsoft Graph-Clientbibliothek für .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) , um Anrufe an Microsoft Graph zu tätigen.

## <a name="get-calendar-events-from-outlook"></a>Abrufen von Kalenderereignissen von Outlook

1. Fügen Sie eine neue Seite für die Kalenderansicht hinzu. Klicken Sie mit der rechten Maustaste auf das **GraphTutorial** -Projekt im Projektmappen-Explorer, und wählen Sie **> neues Element hinzufügen**aus. Wählen Sie **leere Seite**aus `CalendarPage.xaml` , geben Sie in das Feld **Name** ein, und wählen Sie **Hinzufügen**aus.

1. Öffnen `CalendarPage.xaml` Sie und fügen Sie die folgende im vorhandenen `<Grid>` Element hinzu.

    ```xaml
    <TextBlock x:Name="Events" TextWrapping="Wrap"/>
    ```

1. Öffnen `CalendarPage.xaml.cs` Sie und fügen Sie `using` die folgenden Anweisungen am Anfang der Datei hinzu.

    ```csharp
    using Microsoft.Toolkit.Graph.Providers;
    using Microsoft.Toolkit.Uwp.UI.Controls;
    using Newtonsoft.Json;
    ```

1. Fügen Sie der `CalendarPage` -Klasse die folgenden Funktionen hinzu.

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
            // Get the events
            var events = await graphClient.Me.Events.Request()
                .Select("subject,organizer,start,end")
                .OrderBy("createdDateTime DESC")
                .GetAsync();

            // TEMPORARY: Show the results as JSON
            Events.Text = JsonConvert.SerializeObject(events.CurrentPage);
        }
        catch(Microsoft.Graph.ServiceException ex)
        {
            ShowNotification($"Exception getting events: {ex.Message}");
        }

        base.OnNavigatedTo(e);
    }
    ```

    Verwenden Sie den Code in `OnNavigatedTo` ist doing.

    - Die URL, die aufgerufen wird, lautet `/v1.0/me/events`.
    - Die `Select`-Funktion beschränkt die Felder, die für jedes Ereignis zurückgegeben werden, auf nur diejenigen, die von der Ansicht tatsächlich verwendet werden.
    - Die `OrderBy`-Funktion sortiert die Ergebnisse nach Datum und Uhrzeit ihrer Erstellung, wobei das aktuellste Element zuerst angezeigt wird.

1. Ändern Sie `NavView_ItemInvoked` die-Methode `MainPage.xaml.cs` in der Datei, um `switch` die vorhandene Anweisung durch Folgendes zu ersetzen.

    :::code language="csharp" source="../demo/GraphTutorial/MainPage.xaml.cs" id="SwitchStatementSnippet" highlight="4":::

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

1. Öffnen `CalendarPage.xaml.cs` Sie die- `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` und ersetzen Sie die-Reihe durch Folgendes.

    ```csharp
    EventList.ItemsSource = events.CurrentPage.ToList();
    ```

    Wenn Sie die APP jetzt ausführen und den Kalender auswählen, sollten Sie eine Liste der Ereignisse in einem Datenraster abrufen. Die **Start** **-und** Endwerte werden jedoch auf nicht benutzerfreundliche Weise angezeigt. Sie können steuern, wie diese Werte mithilfe eines [Wertkonverters](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter)angezeigt werden.

1. Klicken Sie mit der rechten Maustaste auf das **GraphTutorial** -Projekt im Projektmappen-Explorer, und wählen Sie **> Klasse hinzufügen**aus. Nennen Sie die `GraphDateTimeTimeZoneConverter.cs` Klasse, und wählen Sie **Hinzufügen**aus. Ersetzen Sie den gesamten Inhalt der Datei durch den folgenden Code.

    :::code language="csharp" source="../demo/GraphTutorial/GraphDateTimeTimeZoneConverter.cs" id="ConverterSnippet":::

    Dieser Code verwendet die von Microsoft Graph zurückgegebene [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) -Struktur und analysiert `DateTimeOffset` Sie in ein Objekt. Anschließend wird der Wert in die Zeitzone des Benutzers konvertiert und der formatierte Wert zurückgegeben.

1. Öffnen `CalendarPage.xaml` Sie, und fügen **before** Sie Folgendes `<Grid>` vor dem-Element hinzu.

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="ResourcesSnippet":::

1. Ersetzen Sie die letzten `DataGridTextColumn` beiden Elemente durch Folgendes.

    :::code language="xaml" source="../demo/GraphTutorial/CalendarPage.xaml" id="BindingSnippet" highlight="4,9":::

1. Führen Sie die APP aus, melden Sie sich an, und klicken Sie auf das Navigationselement **Kalender** . Die Liste der Ereignisse sollte angezeigt werden, wobei die Werte **Start** und **End** formatiert sind.

    ![Ein Screenshot der Tabelle mit Ereignissen](./images/add-msgraph-01.png)
