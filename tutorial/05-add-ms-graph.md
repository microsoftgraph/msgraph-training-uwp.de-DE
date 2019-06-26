<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="76c5e-101">In dieser Übung werden Sie das Microsoft Graph in die Anwendung integrieren.</span><span class="sxs-lookup"><span data-stu-id="76c5e-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="76c5e-102">Für diese Anwendung verwenden Sie die [Microsoft Graph-Clientbibliothek für .net](https://github.com/microsoftgraph/msgraph-sdk-dotnet) , um Anrufe an Microsoft Graph zu tätigen.</span><span class="sxs-lookup"><span data-stu-id="76c5e-102">For this application, you will use the [Microsoft Graph Client Library for .NET](https://github.com/microsoftgraph/msgraph-sdk-dotnet) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="76c5e-103">Abrufen von Kalenderereignissen aus Outlook</span><span class="sxs-lookup"><span data-stu-id="76c5e-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="76c5e-104">Beginnen Sie mit dem Hinzufügen einer neuen Seite für die Kalenderansicht.</span><span class="sxs-lookup"><span data-stu-id="76c5e-104">Start by adding a new page for the calendar view.</span></span> <span data-ttu-id="76c5e-105">Klicken Sie mit der rechten Maustaste auf das **Graph-Tutorial-** Projekt im Projektmappen-Explorer, und wählen Sie **#a0 neues Element hinzufügen aus...**. Wählen Sie **leere Seite**aus `CalendarPage.xaml` , geben Sie in das Feld **Name** ein, und wählen Sie **Hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="76c5e-105">Right-click the **graph-tutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Blank Page**, enter `CalendarPage.xaml` in the **Name** field, and select **Add**.</span></span>

<span data-ttu-id="76c5e-106">Öffnen `CalendarPage.xaml` Sie und fügen Sie die folgende im vorhandenen `<Grid>` Element hinzu.</span><span class="sxs-lookup"><span data-stu-id="76c5e-106">Open `CalendarPage.xaml` and add the following line inside the existing `<Grid>` element.</span></span>

```xml
<TextBlock x:Name="Events" TextWrapping="Wrap"/>
```

<span data-ttu-id="76c5e-107">Öffnen `CalendarPage.xaml.cs` Sie und fügen Sie `using` die folgenden Anweisungen am Anfang der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="76c5e-107">Open `CalendarPage.xaml.cs` and add the following `using` statements at the top of the file.</span></span>

```cs
using Microsoft.Toolkit.Services.MicrosoftGraph;
using Microsoft.Toolkit.Uwp.UI.Controls;
using Newtonsoft.Json;
```

<span data-ttu-id="76c5e-108">Fügen Sie dann die folgenden Funktionen zur `CalendarPage` Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="76c5e-108">Then add the following functions to the `CalendarPage` class.</span></span>

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

<span data-ttu-id="76c5e-109">Verwenden Sie den Code in `OnNavigatedTo` ist doing.</span><span class="sxs-lookup"><span data-stu-id="76c5e-109">Consider with the code in `OnNavigatedTo` is doing.</span></span>

- <span data-ttu-id="76c5e-110">Die URL, die aufgerufen wird `/v1.0/me/events`.</span><span class="sxs-lookup"><span data-stu-id="76c5e-110">The URL that will be called is `/v1.0/me/events`.</span></span>
- <span data-ttu-id="76c5e-111">Die `Select` -Funktion schränkt die für die einzelnen Ereignisse zurückgegebenen Felder auf genau diejenigen ein, die die Ansicht tatsächlich verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="76c5e-111">The `Select` function limits the fields returned for each events to just those the view will actually use.</span></span>
- <span data-ttu-id="76c5e-112">Die `OrderBy` -Funktion sortiert die Ergebnisse nach dem Datum und der Uhrzeit, zu der Sie erstellt wurden, wobei das letzte Element zuerst angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="76c5e-112">The `OrderBy` function sorts the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="76c5e-113">Bevor Sie die app ausführen, müssen Sie, um zu dieser Kalender Seite navigieren zu können, die `NavView_ItemInvoked` Methode in der `MainPage.xaml.cs` Datei so ändern, dass `throw new NotImplementedException();` Sie die folgende Stelle ersetzt:</span><span class="sxs-lookup"><span data-stu-id="76c5e-113">Just before running the app, in order to be able to navigate to this calendar page, modify the `NavView_ItemInvoked` method in the `MainPage.xaml.cs` file to replace the `throw new NotImplementedException();` line with as follows.</span></span>

```cs
case "calendar":
    RootFrame.Navigate(typeof(CalendarPage));
    break;
```

<span data-ttu-id="76c5e-114">Sie können nun die app ausführen, sich anmelden und im Menü auf der linken Seite auf das Navigationselement **Kalender** klicken.</span><span class="sxs-lookup"><span data-stu-id="76c5e-114">You can now run the app, sign in, and click the **Calendar** navigation item in the left-hand menu.</span></span> <span data-ttu-id="76c5e-115">Es sollte ein JSON-Dump der Ereignisse im Kalender des Benutzers angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="76c5e-115">You should see a JSON dump of the events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="76c5e-116">Anzeigen der Ergebnisse</span><span class="sxs-lookup"><span data-stu-id="76c5e-116">Display the results</span></span>

<span data-ttu-id="76c5e-117">Nun können Sie das JSON-Dump durch etwas ersetzen, um die Ergebnisse auf eine benutzerfreundliche Weise anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="76c5e-117">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="76c5e-118">Ersetzen Sie den gesamten Inhalt `CalendarPage.xaml` durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="76c5e-118">Replace the entire contents of `CalendarPage.xaml` with the following.</span></span>

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

<span data-ttu-id="76c5e-119">Dadurch wird die `TextBlock` durch a `DataGrid`ersetzt.</span><span class="sxs-lookup"><span data-stu-id="76c5e-119">This replaces the `TextBlock` with a `DataGrid`.</span></span> <span data-ttu-id="76c5e-120">Öffnen `CalendarPage.xaml.cs` Sie jetzt und ersetzen `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` Sie die-Reihe durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="76c5e-120">Now open `CalendarPage.xaml.cs` and replace the `Events.Text = JsonConvert.SerializeObject(events.CurrentPage);` line with the following.</span></span>

```cs
EventList.ItemsSource = events.CurrentPage.ToList();
```

<span data-ttu-id="76c5e-121">Wenn Sie die APP jetzt ausführen und den Kalender auswählen, sollten Sie eine Liste der Ereignisse in einem Datenraster abrufen.</span><span class="sxs-lookup"><span data-stu-id="76c5e-121">If you run the app now and select the calendar, you should get a list of events in a data grid.</span></span> <span data-ttu-id="76c5e-122">Die **Start** -und Endwerte werden jedoch auf nicht benutzerfreundliche Weise angezeigt. \*\*\*\*</span><span class="sxs-lookup"><span data-stu-id="76c5e-122">However, the **Start** and **End** values are displayed in a non-user-friendly manner.</span></span> <span data-ttu-id="76c5e-123">Sie können steuern, wie diese Werte mithilfe eines [Wertkonverters](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter)angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="76c5e-123">You can control how those values are displayed by using a [value converter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).</span></span>

<span data-ttu-id="76c5e-124">Klicken Sie mit der rechten Maustaste auf das **Graph-Tutorial-** Projekt im Projektmappen-Explorer, und wählen Sie **#a0 Klasse hinzufügen**aus. Nennen Sie die `GraphDateTimeTimeZoneConverter.cs` Klasse, und wählen Sie **Hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="76c5e-124">Right-click the **graph-tutorial** project in Solution Explorer and select **Add > Class...**. Name the class `GraphDateTimeTimeZoneConverter.cs` and select **Add**.</span></span> <span data-ttu-id="76c5e-125">Ersetzen Sie den gesamten Inhalt der Datei durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="76c5e-125">Replace the entire contents of the file with the following.</span></span>

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

<span data-ttu-id="76c5e-126">Dieser Code verwendet die von Microsoft Graph zurückgegebene [dateTimeTimeZone](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/datetimetimezone) -Struktur und analysiert `DateTimeOffset` Sie in ein Objekt.</span><span class="sxs-lookup"><span data-stu-id="76c5e-126">This code takes the [dateTimeTimeZone](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/datetimetimezone) structure returned by Microsoft Graph and parses it into a `DateTimeOffset` object.</span></span> <span data-ttu-id="76c5e-127">Anschließend wird der Wert in die Zeitzone des Benutzers konvertiert und der formatierte Wert zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="76c5e-127">It then converts the value into the user's time zone and returns the formatted value.</span></span>

<span data-ttu-id="76c5e-128">Öffnen `CalendarPage.xaml` Sie, und fügen \*\*\*\* Sie Folgendes `<Grid>` vor dem-Element hinzu.</span><span class="sxs-lookup"><span data-stu-id="76c5e-128">Open `CalendarPage.xaml` and add the following **before** the `<Grid>` element.</span></span>

```xml
<Page.Resources>
    <local:GraphDateTimeTimeZoneConverter x:Key="DateTimeTimeZoneValueConverter" />
</Page.Resources>
```

<span data-ttu-id="76c5e-129">Ersetzen Sie dann die `Binding="{Binding Start.DateTime}"` -Reihe durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="76c5e-129">Then, replace the `Binding="{Binding Start.DateTime}"` line with the following.</span></span>

```xml
Binding="{Binding Start, Converter={StaticResource DateTimeTimeZoneValueConverter}}"
```

<span data-ttu-id="76c5e-130">Ersetzen Sie `Binding="{Binding End.DateTime}"` die-Verbindung durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="76c5e-130">Replace the `Binding="{Binding End.DateTime}"` line with the following.</span></span>

```xml
Binding="{Binding End, Converter={StaticResource DateTimeTimeZoneValueConverter}}"
```

<span data-ttu-id="76c5e-131">Führen Sie die APP aus, melden Sie sich an, und klicken Sie auf das Navigationselement **Kalender** .</span><span class="sxs-lookup"><span data-stu-id="76c5e-131">Run the app, sign in, and click the **Calendar** navigation item.</span></span> <span data-ttu-id="76c5e-132">Die Liste der Ereignisse sollte angezeigt werden, wobei die Werte **Start** und **End** formatiert sind.</span><span class="sxs-lookup"><span data-stu-id="76c5e-132">You should see the list of events with the **Start** and **End** values formatted.</span></span>

![Ein Screenshot der Ereignistabelle](./images/add-msgraph-01.png)
