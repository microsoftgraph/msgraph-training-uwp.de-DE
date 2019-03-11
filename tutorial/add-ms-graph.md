<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung integrieren Sie Microsoft Graph in die Anwendung. Für diese Anwendung verwenden Sie die [Microsoft Graph-Client Bibliothek für .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) , um Aufrufe von Microsoft Graph zu tätigen.

## <a name="get-calendar-events-from-outlook"></a>Abrufen von Kalenderereignissen aus Outlook

Beginnen Sie mit dem Hinzufügen einer neuen Seite für die Kalenderansicht. Klicken Sie im projektMappen-Explorer mit der rechten Maustaste auf das **Graph-Tutorial-** Projekt, und wählen Sie **_GT_ neues Element hinzufügen...**. Wählen Sie **leere Seite**aus `CalendarPage.xaml` , geben Sie in das Feld **Name** ein, und wählen Sie **Hinzufügen**aus.

Öffnen `CalendarPage.xaml` Sie, und fügen Sie die folgende Codezeile `<Grid>` innerhalb des vorhandenen Elements hinzu.

```xml
<TextBlock x:Name="Events" TextWrapping="Wrap"/>
```

Öffnen `CalendarPage.xaml.cs` Sie und fügen Sie `using` die folgenden Anweisungen am Anfang der Datei hinzu.

```cs
using Microsoft.Toolkit.Services.MicrosoftGraph;
using Microsoft.Toolkit.Uwp.UI.Controls;
using Newtonsoft.Json;
```

Fügen Sie dann der `CalendarPage` Klasse die folgenden Funktionen hinzu.

```cs
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
    // Get the Graph client from the service
    var graphClient = MicrosoftGraphService.Instance.GraphProvider;

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

Berücksichtigen Sie mit dem Code `OnNavigatedTo` in is doing.

- Die URL, die aufgerufen wird, `/v1.0/me/events`lautet.
- Die `Select` Funktion schränkt die für jedes Ereignis zurückgegebenen Felder auf diejenigen ein, die die Ansicht tatsächlich verwendet.
- Die `OrderBy` Funktion sortiert die Ergebnisse nach dem Datum und der Uhrzeit, zu denen Sie erstellt wurden, wobei das neueste Element zuerst angezeigt wird.

Bevor Sie die app ausführen, müssen Sie in der Lage sein, zu dieser Kalender Seite zu navigieren, `NavView_ItemInvoked` indem Sie die `MainPage.xaml.cs` -Methode in der `throw new NotImplementedException();` Datei so ändern, dass Sie die folgende ersetzt:

```cs
case "calendar":
    RootFrame.Navigate(typeof(CalendarPage));
    break;
```

Sie können jetzt die app ausführen, sich anmelden und im linken Menü auf das Navigationselement **Kalender** klicken. Es sollte ein JSON-Dump der Ereignisse im Kalender des Benutzers angezeigt werden.

## <a name="display-the-results"></a>Anzeigen der Ergebnisse

Jetzt können Sie den JSON-Dump durch einen anderen ersetzen, um die Ergebnisse auf benutzerfreundliche Weise anzuzeigen. Ersetzen Sie den gesamten Inhalt `CalendarPage.xaml` von durch Folgendes.

```xml
<Page
    x:Class="graph_tutorial.CalendarPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:graph_tutorial"
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

Dadurch wird die `TextBlock` durch eine `DataGrid`ersetzt. Öffnen `CalendarPage.xaml.cs` Sie nun die- `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` und ersetzen Sie die folgende.

```cs
EventList.ItemsSource = events.CurrentPage.ToList();
```

Wenn Sie die APP jetzt ausführen und den Kalender auswählen, sollten Sie eine Liste der Ereignisse in einem Datenraster abrufen. Die **Start** -und Endwerte werden jedoch auf nicht benutzerfreundliche Weise angezeigt. **** Sie können steuern, wie diese Werte mit einem [Wertkonverter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter)angezeigt werden.

Klicken Sie im projektMappen-Explorer mit der rechten Maustaste auf das **Graph-Tutorial-** Projekt, und wählen Sie **_GT_-Klasse hinzufügen...**. Benennen Sie die `GraphDateTimeTimeZoneConverter.cs` Klasse, und wählen Sie **Hinzufügen**. Ersetzen Sie den gesamten Inhalt der Datei durch Folgendes.

```cs
using Microsoft.Graph;
using System;

namespace graph_tutorial
{
    class GraphDateTimeTimeZoneConverter : Windows.UI.Xaml.Data.IValueConverter
    {
        public object Convert(object value, Type targetType, object parameter, string language)
        {
            DateTimeTimeZone date = value as DateTimeTimeZone;

            if (date != null)
            {
                // Resolve the time zone
                var timezone = TimeZoneInfo.FindSystemTimeZoneById(date.TimeZone);
                // Parse method assumes local time, which may not be the case
                var parsedDateAsLocal = DateTimeOffset.Parse(date.DateTime);
                // Determine the offset from UTC time for the specific date
                // Making this call adjusts for DST as appropriate
                var tzOffset = timezone.GetUtcOffset(parsedDateAsLocal.DateTime);
                // Create a new DateTimeOffset with the specific offset from UTC
                var correctedDate = new DateTimeOffset(parsedDateAsLocal.DateTime, tzOffset);
                // Return the local date time string
                return correctedDate.LocalDateTime.ToString();
            }

            return string.Empty;
        }

        public object ConvertBack(object value, Type targetType, object parameter, string language)
        {
            throw new NotImplementedException();
        }
    }
}
```

Dieser Code verwendet die von Microsoft Graph zurückgegebene [dateTimeTimeZone](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/datetimetimezone) -Struktur und analysiert `DateTimeOffset` Sie in ein Objekt. Anschließend wird der Wert in die Zeitzone des Benutzers konvertiert und der formatierte Wert zurückgegeben.

Öffnen `CalendarPage.xaml` Sie, und fügen **** Sie Folgendes `<Grid>` vor dem-Element hinzu.

```xml
<Page.Resources>
    <local:GraphDateTimeTimeZoneConverter x:Key="DateTimeTimeZoneValueConverter" />
</Page.Resources>
```

Ersetzen Sie dann die `Binding="{Binding Start.DateTime}"` -Codezeile durch Folgendes.

```xml
Binding="{Binding Start, Converter={StaticResource DateTimeTimeZoneValueConverter}}"
```

Ersetzen Sie `Binding="{Binding End.DateTime}"` die-Reihe durch Folgendes.

```xml
Binding="{Binding End, Converter={StaticResource DateTimeTimeZoneValueConverter}}"
```

Führen Sie die APP aus, melden Sie sich an, und klicken Sie auf das Navigationselement **Kalender** . Es sollte eine Liste der Ereignisse angezeigt werden, deren **anfangs** -und Endwerte formatiert sind. ****

![Screenshot der Ereignistabelle](./images/add-msgraph-01.png)
