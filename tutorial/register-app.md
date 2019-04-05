<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung erstellen Sie eine neue Azure AD native-Anwendung mithilfe des Azure Active Directory Admin Center.

1. Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com) , und melden Sie sich mit einem **persönlichen Konto** (aka: Microsoft-Konto) oder einem Geschäfts- **oder Schulkonto**an.

1. Wählen Sie **Azure Active Directory** in der linken Navigationsleiste aus, und wählen Sie dann **App-Registrierungen (Vorschau)** unter **Manage**aus.

    ![Screenshot der APP-Registrierungen ](./images/aad-portal-app-registrations.png)

1. Wählen Sie **neue Registrierung**aus. Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.

    - Legen **** Sie Name `UWP Graph Tutorial`auf fest.
    - Legen Sie **unterstützte Kontotypen** auf **Konten in einem beliebigen Organisations Verzeichnis und persönlichen Microsoft-Konten**fest.
    - Lassen Sie den umLeitungs- **URI** leer.

    ![Screenshot der Seite "Registrieren einer Anwendung"](./images/aad-register-an-app.png)

1. Wählen Sie **registrieren**aus. Kopieren Sie auf der Seite **UWP Graph Tutorial** den Wert der **Anwendungs-ID (Client)** , und speichern Sie ihn, dann benötigen Sie ihn im nächsten Schritt.

    ![Screenshot der Anwendungs-ID der neuen App-Registrierung](./images/aad-application-id.png)

1. Wählen Sie den Link umLeitungs- **URI hinzufügen** aus. Suchen Sie auf der Seite **URIs umleiten** nach dem Abschnitt Empfohlene Umleitungs- **URIs für öffentliche Clients (Mobil, Desktop)** . Wählen Sie `urn:ietf:wg:oauth:2.0:oob` den URI aus, und klicken Sie dann auf **Speichern**.

    ![Screenshot der Seite "umLeitungs-URIs"](./images/aad-redirect-uris.png)