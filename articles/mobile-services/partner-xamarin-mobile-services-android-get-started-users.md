---
title: Erste Schritte mit der Authentifizierung (Xamarin.Android) - Mobile Services
description: Erfahren Sie mehr über die Verwendung der Authentifizierung in Ihrer Azure Mobile Services-App für Xamarin.Android.
services: mobile-services
documentationcenter: xamarin
manager: dwrede
author: lindydonna
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 07/21/2016
ms.author: donnam

---
# Hinzufügen von Authentifizierung zur Mobile Services-App
[!INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

&nbsp;

[!INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

> Informationen zur entsprechenden Mobile Apps-Version dieses Themas finden Sie unter [Hinzufügen der Authentifizierung zu Ihrer Xamarin.Android-App](../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md).
> 
> 

<p>Dieses Thema beschreibt die Authentifizierung von Benutzern in Azure Mobile Services aus Ihrer Xamarin.Android-App. In diesem Lernprogramm fügen Sie eine Authentifizierung zu dem Schnellstartprojekt hinzu. Sie verwenden dazu einen Identitätsanbieter, der von Mobile Services unterstützt wird. Nach der erfolgreichen Authentifizierung und Autorisierung durch Mobile Services wird der Benutzer-ID-Wert angezeigt.</p>

Dieses Lernprogramm zeigt Ihnen die grundlegenden Schritte zur Aktivierung von Authentifizierung in Ihrer App:

1. [Registrieren Ihrer App für Authentifizierung und Konfigurieren von Mobile Services]
2. [Einschränken von Tabellenberechtigungen für authentifizierte Benutzer]
3. [Hinzufügen von Authentifizierung zur App]

Dieses Lernprogramm baut auf dem Mobile Services-Schnellstart auf. Sie müssen zunächst das Lernprogramm [Erste Schritte mit Mobile Services] abschließen.

Zum Abschließen dieses Tutorials ist Visual Studio mit Xamarin unter Windows oder Xamarin Studio unter Mac OS X erforderlich. Vollständige Installationsanweisungen finden Sie unter [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).

## <a name="register"></a>Registrieren Ihrer App für Authentifizierung und Konfigurieren von Mobile Services
[!INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

## <a name="permissions"></a>Einschränken von Berechtigungen für authentifizierte Benutzer
[!INCLUDE [mobile-services-restrict-permissions-javascript-backend](../../includes/mobile-services-restrict-permissions-javascript-backend.md)]

1. Öffnen Sie in Xamarin Studio das Projekt, das Sie beim Ausführen des Tutorials [Erste Schritte mit Mobile Services] erstellt haben.
2. Klicken Sie im Menü **Ausführen** auf **Debuggen starten**, um die App zu starten. Vergewissern Sie sich, dass nach dem Start der App ein Ausnahmefehler mit dem Statuscode 401 (nicht autorisiert) ausgelöst wird.
   
     Dies liegt daran, dass die App als nicht authentifizierter Benutzer auf den mobilen Dienst zugreift und die *TodoItem*-Tabelle nun eine Authentifizierung verlangt.

Als Nächstes werden Sie die App aktualisieren, um Benutzer zu authentifizieren, bevor diese Ressourcen vom Mobile Service anfordern.

## <a name="add-authentication"></a>Hinzufügen von Authentifizierung zur App
1. Fügen Sie der **TodoActivity**-Klasse die folgende Eigenschaft hinzu:
   
        private MobileServiceUser user;
2. Fügen Sie der **ToDoActivity**-Klasse die folgende Methode hinzu:
   
        private async Task Authenticate()
        {
            try
            {
                user = await client.LoginAsync(this, MobileServiceAuthenticationProvider.MicrosoftAccount);
                CreateAndShowDialog(string.Format("you are now logged in - {0}", user.UserId), "Logged in!");
            }
            catch (Exception ex)
            {
                CreateAndShowDialog(ex, "Authentication failed");
            }
        }
   
    Auf diese Weise wird eine neue Methode zur Verarbeitung des Authentifizierungsprozesses erstellt. Der Benutzer wird mithilfe einer Microsoft-Konto-Anmeldung authentifiziert. Ein Dialogfeld mit der ID des authentifizierten Benutzers wird eingeblendet. Ohne erfolgreiche Authentifizierung können Sie nicht fortfahren.
   
   > [!NOTE]
   > Falls Sie einen anderen Identitätsanbieter als Microsoft verwenden, ändern Sie den an die **login**-Methode übergebenen Wert auf einen der folgenden Werte: *Facebook*, *Google*, *Twitter* oder *WindowsAzureActiveDirectory*.
   > 
   > 
3. Fügen Sie in der **onCreate**-Methode die folgende Codezeile im Anschluss an den Code hinzu, der das `MobileServiceClient`-Objekt instanziiert.
   
        await Authenticate();
   
    Dieser Aufruf startet den Authentifizierungsprozess und wartet asynchron auf eine Antwort.
4. Verschieben Sie den verbleibenden Code nach `await Authenticate();` in der **OnCreate**-Methode in eine neue **CreateTable**-Methode mit dem folgenden Code:
   
        private async Task CreateTable()
        {
   
            await InitLocalStoreAsync();
   
            // Get the Mobile Service Table instance to use
            toDoTable = client.GetSyncTable<ToDoItem>();
   
            textNewToDo = FindViewById<EditText>(Resource.Id.textNewToDo);
   
            // Create an adapter to bind the items with the view
            adapter = new ToDoItemAdapter(this, Resource.Layout.Row_List_To_Do);
            var listViewTodo = FindViewById<ListView>(Resource.Id.listViewToDo);
            listViewTodo.Adapter = adapter;
   
            // Load the items from the Mobile Service
            await RefreshItemsFromTableAsync();
        }
5. Rufen Sie anschließend die neue **CreateTable**-Methode in **OnCreate** nach dem in Schritt 2 hinzugefügten **Authenticate**-Aufruf auf:
   
        await CreateTable();
6. Klicken Sie im Menü **Ausführen** auf **Debuggen starten**, um die App zu starten und sich mit dem Identitätsanbieter Ihrer Wahl anzumelden.
   
       Wenn Sie sich erfolgreich angemeldet haben, sollte die App fehlerfrei ausgeführt werden, und Sie sollten Mobile Services abfragen und Daten aktualisieren können.

## Abgeschlossenes Beispiel abrufen
Laden Sie das [abgeschlossene Beispielprojekt] herunter. Stellen Sie sicher, dass Sie die Variablen **applicationURL** und **applicationKey** mit Ihren eigenen Azure-Einstellungen aktualisieren.

## <a name="next-steps"></a>Nächste Schritte
Im nächsten Lernprogramm [Autorisieren von Benutzern mit Skripts] werden Sie den von Mobile Services auf Basis eines authentifizierten Benutzers bereitgestellten Benutzer-ID-Wert verwenden, um von Mobile Services zurückgegebene Daten zu filtern.

<!-- Anchors. -->
[Registrieren Ihrer App für Authentifizierung und Konfigurieren von Mobile Services]: #register
[Einschränken von Tabellenberechtigungen für authentifizierte Benutzer]: #permissions
[Hinzufügen von Authentifizierung zur App]: #add-authentication
[Next Steps]: #next-steps

<!-- Images. -->
[4]: ./media/partner-xamarin-mobile-services-android-get-started-users/mobile-services-selection.png
[5]: ./media/partner-xamarin-mobile-services-android-get-started-users/mobile-service-uri.png

[13]: ./media/partner-xamarin-mobile-services-android-get-started-users/mobile-identity-tab.png
[14]: ./media/partner-xamarin-mobile-services-android-get-started-users/mobile-portal-data-tables.png
[15]: ./media/partner-xamarin-mobile-services-android-get-started-users/mobile-portal-change-table-perms.png

<!-- URLs. -->
[Erste Schritte mit Mobile Services]: partner-xamarin-mobile-services-android-get-started.md
[Autorisieren von Benutzern mit Skripts]: mobile-services-javascript-backend-service-side-authorization.md
[abgeschlossene Beispielprojekt]: http://go.microsoft.com/fwlink/p/?LinkId=331328

<!---HONumber=AcomDC_0727_2016-->