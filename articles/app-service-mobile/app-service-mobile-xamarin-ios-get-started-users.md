---
title: Erste Schritte mit der Authentifizierung für mobile Apps in Xamarin iOS
description: Erfahren Sie, wie Sie Mobile Apps zum Authentifizieren Ihrer Xamarin iOS-App über eine Vielzahl von Identitätsanbietern nutzen können, darunter AAD, Google, Facebook, Twitter und Microsoft.
services: app-service\mobile
documentationcenter: xamarin
author: mattchenderson
manager: dwrede
editor: ''

ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 06/28/2016
ms.author: mahender

---
# Hinzufügen der Authentifizierung zu Ihrer Xamarin.iOS-App
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In diesem Thema wird die Authentifizierung von Benutzern einer mobilen App Service-App über Ihre Clientanwendung veranschaulicht. In diesem Lernprogramm fügen Sie dem Xamarin.iOS-Schnellstartprojekt durch Verwenden eines von App Service unterstützten Identitätsanbieters eine Authentifizierungsfunktion hinzu. Nach der erfolgreichen Authentifizierung und Autorisierung durch Ihre mobile App wird die Benutzer-ID angezeigt und Sie erhalten Zugriff auf beschränkte Tabellendaten.

Sie müssen zunächst das Lernprogramm [Erstellen einer Xamarin.iOS-App] abschließen. Wenn Sie das heruntergeladene Schnellstart-Serverprojekt nicht verwenden, müssen Sie Ihrem Projekt das Authentifizierungs-Erweiterungspaket hinzufügen. Weitere Informationen zu Servererweiterungspaketen finden Sie unter [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) (in englischer Sprache).

## Registrieren Ihrer App für die Authentifizierung und Konfigurieren von App Services
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## Einschränken von Berechtigungen für authentifizierte Benutzer
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. Führen Sie das Clientprojekt in Visual Studio oder Xamarin Studio auf einem Gerät oder Emulator aus. Stellen Sie sicher, dass ein Ausnahmefehler mit dem Statuscode 401 (Nicht autorisiert) angezeigt wird, nachdem die App gestartet wurde. Der Fehler wird in der Konsole des Debuggers protokolliert. Daher sollte in Visual Studio der Fehler im Ausgabefenster angezeigt werden.

&nbsp;&nbsp;Der nicht autorisierte Fehler kommt vor, weil die App versucht, als nicht authentifizierter Benutzer auf Ihr Mobile App-Back-End zuzugreifen. Die Tabelle *TodoItem* erfordert jetzt eine Authentifizierung.

Als Nächstes aktualisieren Sie die Client-App, um Ressourcen vom mobilen App-Back-End mit einem authentifizierten Benutzer zu aktualisieren.

## Hinzufügen von Authentifizierung zur App
In diesem Abschnitt modifizieren Sie die App, sodass vor der Anzeige von Daten ein Anmeldebildschirm angezeigt wird. Wenn die App gestartet wird, baut sie keine Verbindung zu App Service auf und zeigt keinerlei Daten an. Nachdem der Benutzer die Aktualisierungsgeste durchführt, wird der Anmeldebildschirm angezeigt, nach der erfolgreichen Anmeldung dann die Liste von To-Do-Objekten.

1. Öffnen Sie im Clientprojekt die Datei **QSTodoService.cs**, und fügen Sie der QSTodoService-Klasse nachstehende using-Anweisung und `MobileServiceUser` with-Accessor hinzu:
   
    ```
        using UIKit;
    ```
   
        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }
2. Fügen Sie die neue Methode **Authenticate** mit der folgenden Definition zu **QSTodoService** hinzu:

        public async Task Authenticate(UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Falls Sie einen anderen Identitätsanbieter als ein Facebook verwenden, ändern Sie den an **LoginAsync** übergebenen Wert auf einen der folgenden Werte: _MicrosoftAccount_, _Twitter_, _Google_ oder _WindowsAzureActiveDirectory_.

1. Öffnen Sie **QSTodoListViewController.cs**. Ändern Sie die Methodendefinition von **ViewDidLoad** und entfernen Sie den Aufruf von **RefreshAsync()** in der Nähe des Endes:
   
        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();
   
            todoService = QSTodoService.DefaultService;
           await todoService.InitializeStoreAsync ();
   
           RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync ();
           }
   
            // Comment out the call to RefreshAsync
            // await RefreshAsync ();
        }
2. Ändern Sie die Methode **RefreshAsync** für die Authentifizierung, wenn die Eigenschaft **Benutzer** null ist. Fügen Sie folgenden Code oben in der Methodendefinition hinzu:
   
        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method
3. Führen Sie in Visual Studio oder Xamarin Studio mit Verbindung zum Xamarin Build Host auf Ihrem Mac das Clientprojekt auf einem Gerät oder Emulator aus. Überprüfen Sie, dass die App keine Daten anzeigt.
   
    Führen Sie die Aktualisierungsgeste durch Herunterziehen der Objektliste, was die Anzeige des Anmeldebildschirms auslöst. Nachdem Sie erfolgreich gültige Anmeldeinformationen eingegeben haben, zeigt die App die Liste der To-Do-Objekte an, und Sie können die Daten ändern.

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Erstellen einer Xamarin.iOS-App]: app-service-mobile-xamarin-ios-get-started.md

<!---HONumber=AcomDC_0706_2016-->