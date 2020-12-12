<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d30fe-101">In diesem Abschnitt können Sie die Möglichkeit zum Erstellen von Ereignissen im Kalender des Benutzers hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d30fe-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="d30fe-102">Fügen Sie eine neue Seite für die neue Ereignisansicht hinzu.</span><span class="sxs-lookup"><span data-stu-id="d30fe-102">Add a new page for the new event view.</span></span> <span data-ttu-id="d30fe-103">Klicken Sie mit der rechten Maustaste auf das **GraphTutorial** -Projekt im Projektmappen-Explorer, und wählen Sie **> neues Element hinzufügen** aus. Wählen Sie **leere Seite** aus, geben Sie `NewEventPage.xaml` in das Feld **Name** ein, und wählen Sie **Hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="d30fe-103">Right-click the **GraphTutorial** project in Solution Explorer and select **Add > New Item...**. Choose **Blank Page**, enter `NewEventPage.xaml` in the **Name** field, and select **Add**.</span></span>

1. <span data-ttu-id="d30fe-104">Öffnen Sie **NewEventPage. XAML** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="d30fe-104">Open **NewEventPage.xaml** and replace its contents with the following.</span></span>

    :::code language="xaml" source="../demo/GraphTutorial/NewEventPage.xaml" id="NewEventPageXamlSnippet":::

1. <span data-ttu-id="d30fe-105">Öffnen Sie **NewEventPage.XAML.cs** , und fügen Sie die folgenden `using` Anweisungen am Anfang der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="d30fe-105">Open **NewEventPage.xaml.cs** and add the following `using` statements to the top of the file.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/NewEventPage.xaml.cs" id="UsingStatementsSnippet":::

1. <span data-ttu-id="d30fe-106">Fügen Sie die **INotifyPropertyChange** -Schnittstelle zur **NewEventPage** -Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="d30fe-106">Add the **INotifyPropertyChange** interface to the **NewEventPage** class.</span></span> <span data-ttu-id="d30fe-107">Ersetzen Sie die vorhandene Klassendeklaration durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="d30fe-107">Replace the existing class declaration with the following.</span></span>

    ```csharp
    public sealed partial class NewEventPage : Page, INotifyPropertyChanged
    {
        public NewEventPage()
        {
            this.InitializeComponent();
            DataContext = this;
        }
    }
    ```

1. <span data-ttu-id="d30fe-108">Fügen Sie der **NewEventPage** -Klasse die folgenden Eigenschaften hinzu.</span><span class="sxs-lookup"><span data-stu-id="d30fe-108">Add the following properties to the **NewEventPage** class.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/NewEventPage.xaml.cs" id="PropertiesSnippet":::

1. <span data-ttu-id="d30fe-109">Fügen Sie den folgenden Code hinzu, um die Zeitzone des Benutzers aus Microsoft Graph abzurufen, wenn die Seite geladen wird.</span><span class="sxs-lookup"><span data-stu-id="d30fe-109">Add the following code to get the user's time zone from Microsoft Graph when the page loads.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/NewEventPage.xaml.cs" id="LoadTimeZoneSnippet":::

1. <span data-ttu-id="d30fe-110">Fügen Sie den folgenden Code hinzu, um das Ereignis zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d30fe-110">Add the following code to create the event.</span></span>

    :::code language="csharp" source="../demo/GraphTutorial/NewEventPage.xaml.cs" id="CreateEventSnippet":::

1. <span data-ttu-id="d30fe-111">Ändern `NavView_ItemInvoked` Sie die-Methode in der **MainPage.XAML.cs** -Datei, um die vorhandene `switch` Anweisung durch Folgendes zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="d30fe-111">Modify the `NavView_ItemInvoked` method in the **MainPage.xaml.cs** file to replace the existing `switch` statement with the following.</span></span>

    ```csharp
    switch (invokedItem.ToLower())
    {
        case "new event":
            RootFrame.Navigate(typeof(NewEventPage));
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

1. <span data-ttu-id="d30fe-112">Speichern Sie die Änderungen, und führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="d30fe-112">Save your changes and run the app.</span></span> <span data-ttu-id="d30fe-113">Melden Sie sich an, wählen Sie das Menüelement **neuer Ereignis** aus, füllen Sie das Formular aus, und wählen Sie **Erstellen** aus, um dem Kalender des Benutzers ein Ereignis hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="d30fe-113">Sign in, select the **New event** menu item, fill in the form, and select **Create** to add an event to the user's calendar.</span></span>

    ![Screenshot der neuen Ereignisseite](images/create-event-01.png)
