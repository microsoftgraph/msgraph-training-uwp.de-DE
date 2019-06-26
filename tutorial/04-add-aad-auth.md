<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen. Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken zum Aufrufen von Microsoft Graph zu erhalten. In diesem Schritt werden Sie das [AadLogin](https://docs.microsoft.com/dotnet/api/microsoft.toolkit.uwp.ui.controls.graph.aadlogin?view=win-comm-toolkit-dotnet-stable) -Steuerelement aus dem [Windows Community Toolkit](https://github.com/Microsoft/WindowsCommunityToolkit) in die Anwendung integrieren.

Klicken Sie mit der rechten Maustaste auf das **Graph-Tutorial-** Projekt im Projektmappen-Explorer, und wählen Sie **#a0 neues Element hinzufügen aus...**. Wählen Sie **Ressourcendatei (. resw)**, benennen Sie `OAuth.resw` die Datei, und wählen Sie **Hinzufügen**aus. Wenn die neue Datei in Visual Studio geöffnet wird, erstellen Sie zwei Ressourcen wie folgt.

- **Name:** `AppId`, **Wert:** die APP-ID, die Sie im Anwendungs Registrierungs Portal generiert haben.
- **Name:** `Scopes`, **Wert:**`User.Read Calendars.Read`

![Ein Screenshot der Datei OAuth. resw im Visual Studio-Editor](./images/edit-resources-01.png)

> [!IMPORTANT]
> Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es jetzt ein guter Zeitpunkt, die Datei `OAuth.resw` aus der Quellcodeverwaltung auszuschließen, um unbeabsichtigtes Auslaufen ihrer APP-ID zu vermeiden.

## <a name="configure-the-aadlogin-control"></a>Konfigurieren des AadLogin-Steuerelements

Beginnen Sie mit dem Hinzufügen von Code, um die Werte aus der Ressourcendatei zu lesen. Öffnen `MainPage.xaml.cs` Sie und fügen Sie `using` die folgende Anweisung am Anfang der Datei hinzu.

```cs
using Microsoft.Toolkit.Services.MicrosoftGraph;
```

Ersetzen Sie die Zeile `RootFrame.Navigate(typeof(HomePage));` durch den folgenden Code.

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

Dieser Code lädt die Einstellungen aus `OAuth.resw` und initialisiert die globale Instanz von `MicrosoftGraphService` mit diesen Werten.

Fügen Sie nun einen Ereignishandler für `SignInCompleted` das Ereignis für `AadLogin` das Steuerelement hinzu. Öffnen Sie `MainPage.xaml` die Datei, und ersetzen `<graphControls:AadLogin>` Sie das vorhandene Element durch Folgendes.

```xml
<graphControls:AadLogin x:Name="Login"
    HorizontalAlignment="Left"
    View="SmallProfilePhotoLeft"
    AllowSignInAsDifferentUser="False"
    SignInCompleted="Login_SignInCompleted"
    SignOutCompleted="Login_SignOutCompleted"
    />
```

Fügen Sie dann die folgenden Funktionen zur `MainPage` Klasse in `MainPage.xaml.cs`hinzu.

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

Erweitern Sie schließlich im Projektmappen-Explorer **Homepage. XAML** , und öffnen `HomePage.xaml.cs`Sie. Fügen Sie den folgenden Code nach `this.InitializeComponent();` der Codezeile hinzu.

```cs
if ((App.Current as App).IsAuthenticated)
{
    HomePageMessage.Text = "Welcome! Please use the menu to the left to select a view.";
}
```

Starten Sie die APP neu, und klicken Sie oben in der APP auf das **Anmelde** Steuerelement. Nachdem Sie sich angemeldet haben, sollte die Benutzeroberfläche geändert werden, um anzugeben, dass Sie sich erfolgreich angemeldet haben.

![Ein Screenshot der APP nach der Anmeldung](./images/add-aad-auth-01.png)

> [!NOTE]
> Das `AadLogin` Steuerelement implementiert die Logik des Speicherns und Aktualisierens des Zugriffstokens für Sie. Die Token werden im sicheren Speicher gespeichert und bei Bedarf aktualisiert.
