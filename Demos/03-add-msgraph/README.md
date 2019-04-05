# <a name="how-to-run-the-completed-project"></a>Ausführen des abgeschlossenen Projekts

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen des abgeschlossenen Projekts in diesem Ordner benötigen Sie Folgendes:

- [Visual Studio](https://visualstudio.microsoft.com/vs/) auf dem Entwicklungscomputer installiert. Wenn Sie nicht über Visual Studio verfügen, besuchen Sie den vorherigen Link für Downloadoptionen. (**Hinweis:** dieses Lernprogramm wurde mit der Version 15,81 von Visual Studio 2017 geschrieben. Die Schritte in diesem Handbuch funktionieren möglicherweise mit anderen Versionen, die jedoch nicht getestet wurden.)
- Entweder ein persönliches Microsoft-Konto mit einem Postfach auf Outlook.com oder ein Microsoft-Geschäfts-oder Schulkonto.

Wenn Sie nicht über ein Microsoft-Konto verfügen, gibt es eine Reihe von Optionen, um ein kostenloses Konto zu erhalten:

- Sie können [sich für ein neues persönliches Microsoft-Konto registrieren](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).
- Sie können [sich für das office 365-Entwicklerprogramm anmelden](https://developer.microsoft.com/office/dev-program) , um ein kostenloses Office 365-Abonnement zu erhalten.

## <a name="register-a-native-application-with-the-azure-active-directory-admin-center"></a>Registrieren einer systemeigenen Anwendung im Azure Active Directory Admin Center

1. Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com) , und melden Sie sich mit einem **persönlichen Konto** (aka: Microsoft-Konto) oder einem Geschäfts- **oder Schulkonto**an.

1. Wählen Sie **Azure Active Directory** in der linken Navigationsleiste aus, und wählen Sie dann **App-Registrierungen (Vorschau)** unter **Manage**aus.

    ![Screenshot der APP-Registrierungen ](/tutorial/images/aad-portal-app-registrations.png)

1. Wählen Sie **neue Registrierung**aus. Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.

    - Legen **** Sie Name `UWP Graph Tutorial`auf fest.
    - Legen Sie **unterstützte Kontotypen** auf **Konten in einem beliebigen Organisations Verzeichnis und persönlichen Microsoft-Konten**fest.
    - Lassen Sie den umLeitungs- **URI** leer.

    ![Screenshot der Seite "Registrieren einer Anwendung"](/tutorial/images/aad-register-an-app.png)

1. Wählen Sie **registrieren**aus. Kopieren Sie auf der Seite **UWP Graph Tutorial** den Wert der **Anwendungs-ID (Client)** , und speichern Sie ihn, dann benötigen Sie ihn im nächsten Schritt.

    ![Screenshot der Anwendungs-ID der neuen App-Registrierung](/tutorial/images/aad-application-id.png)

1. Wählen Sie den Link umLeitungs- **URI hinzufügen** aus. Suchen Sie auf der Seite **URIs umleiten** nach dem Abschnitt Empfohlene Umleitungs- **URIs für öffentliche Clients (Mobil, Desktop)** . Wählen Sie `urn:ietf:wg:oauth:2.0:oob` den URI aus, und klicken Sie dann auf **Speichern**.

    ![Screenshot der Seite "umLeitungs-URIs"](/tutorial/images/aad-redirect-uris.png)

## <a name="configure-the-sample"></a>Konfigurieren des Beispiels

1. Benennen Sie `OAuth.resw.example` die Datei `OAuth.resw`in um.
1. Öffnen `graph-tutorial.sln` Sie in Visual Studio.
1. Bearbeiten Sie `OAuth.resw` die Datei in Visual Studio. Ersetzen `YOUR_APP_ID_HERE` Sie durch die **Anwendungs-ID** , die Sie im App-Registrierungs Portal erhalten haben.
1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die **Graph-Tutorial-** Lösung, und wählen Sie **NuGet-Pakete wiederherstellen**.

## <a name="run-the-sample"></a>Ausführen des Beispiels

Drücken Sie in Visual Studio **F5** , oder klicken Sie auf **Debuggen > Start Debugging**.