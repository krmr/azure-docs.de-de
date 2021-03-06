---
title: .NET-Anwendungen mit mehreren Ebenen | Microsoft Docs
description: "Ein .NET-Lernprogramm, das Ihnen hilft, eine Anwendung mit mehreren Ebenen in Azure zu erstellen, die Service Bus-Warteschlangen für die Kommunikation zwischen Ebenen verwendet."
services: service-bus
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1b8608ca-aa5a-4700-b400-54d65b02615c
ms.service: service-bus
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 60ed71eac1a233a08414edb8c89b196401895cd8


---
# <a name="net-multitier-application-using-azure-service-bus-queues"></a>.NET-Anwendungen mit mehreren Ebenen unter Verwendung von Azure Service Bus-Warteschlangen
## <a name="introduction"></a>Einführung
Mit Visual Studio und dem kostenlosen Azure SDK für .NET können Sie schnell und einfach für Microsoft Azure entwickeln. In diesem Tutorial werden Sie durch die Schritte zum Erstellen einer Anwendung geführt, von der mehrere in der lokalen Umgebung ausgeführte Azure-Ressourcen verwendet werden. In den Schritten wird davon ausgegangen, dass Sie noch keine Erfahrung mit Azure haben.

Sie lernen Folgendes:

* Vorbereitung Ihres Computers für die Azure-Entwicklung mit einem einzigen Download und einer einzigen Installation
* Einsatz von Visual Studio für die Azure-Entwicklung
* Erstellung von Anwendungen mit mehreren Ebenen in Azure mit Web- und Arbeiterrollen
* Kommunikation zwischen Ebenen mithilfe von Service Bus-Warteschlangen

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

In diesem Lernprogramm werden Sie eine Anwendung mit mehreren Ebenen in einem Azure-Cloud-Dienst erstellen und ausführen. Als Front-End dient eine ASP.NET MVC-Webrolle, und als Back-End eine Workerrolle, die eine Service Bus-Warteschlange nutzt. Dieselbe Anwendung mit mehreren Ebenen kann auch mit einem Webprojekt als Front-End erstellt und auf einer Azure-Website anstelle eines Cloud-Diensts bereitgestellt werden. Hinweise zu den Unterschieden für Front-Ends mit Azure-Websites finden Sie im Abschnitt [Nächste Schritte](#nextsteps). Sie können auch das Tutorial [Hybride lokale/Cloud-.NET-Anwendung](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) ausprobieren.

Der folgende Screenshot zeigt die fertige Anwendung.

![][0]

## <a name="scenario-overview-interrole-communication"></a>Szenario-Übersicht: Kommunikation zwischen Rollen
Um eine Bestellung zur Verarbeitung zu übermitteln, muss die Front-End-GUI in ihrer Funktion als Webrolle mit der Logikkomponente in der mittleren Ebene interagieren, die eine Workerrolle erfüllt. Dieses Beispiel verwendet die verwaltete Servicebus-Messaging-Funktion für die Kommunikation zwischen Ebenen.

Durch die verwaltete Messaging-Funktion zwischen Web- und mittlerer Ebene werden die Komponenten voneinander entkoppelt. Im Gegensatz zur direkten Kommunikation (z. B. per TCP oder HTTP) verbindet sich die Webebene nicht direkt mit der mittleren Ebene, sondern schiebt Arbeitseinheiten in Form von Nachrichten in Service Bus, wo diese zuverlässig aufbewahrt werden, bis die mittlere Ebene diese konsumieren und verarbeiten kann.

Service Bus bietet zwei Entitäten für das Brokermessaging: Warteschlangen und Themen. Mit Warteschlangen wird jede Nachricht von einem einzelnen Empfänger konsumiert. Themen unterstützen das Veröffentlichungs- und Abonnementmuster, mit dem jede veröffentlichte Nachricht den für das entsprechende Thema registrierten Abonnements zugänglich gemacht wird. Jedes Abonnement pflegt eine eigene Nachrichten-Warteschlange. Abonnements können mit Filterregeln konfiguriert werden. Diese sorgen dafür, dass nur Nachrichten in der Abonnement-Warteschlange landen, welche die Filterregeln erfüllen. Das folgende Beispiel verwendet Service Bus-Warteschlangen.

![][1]

Dieser Kommunikationsmechanismus bietet verschiedene Vorteile gegenüber direkten Nachrichten:

* **Zeitliche Entkopplung:** Dank des asynchronen Nachrichtenmusters müssen Producer und Consumer nicht gleichzeitig online sein. Der Servicebus speichert die Nachrichten zuverlässig, bis der Consumer diese entgegennehmen kann. Auf diese Weise können die Komponenten verteilter Anwendungen voneinander entkoppelt werden, z. B. zu Wartungszwecken oder bei einem Komponentenausfall, ohne das Gesamtsystem zu beeinträchtigen. Außerdem genügt es unter Umständen, wenn die konsumierende Anwendung nur zu bestimmten Tageszeiten online ist.
* **Belastungsausgleich:** In vielen Anwendungen schwankt die Systemlast mit der Zeit, während die Bearbeitungszeit pro Arbeitseinheit normalerweise konstant ist. Durch die Einführung einer Warteschlange zwischen Nachrichtenproducer und Consumer muss der Consumer (Arbeiter) anstatt der Spitzenlast nur die durchschnittliche Last verarbeiten können. Die Tiefe der Warteschlange erhöht und verringert sich mit der eingehenden Last. Dies ermöglicht direkte Einsparungen bei der Infrastruktur, die zur Bearbeitung der Anwendungslast benötigt wird.
* **Lastenausgleich:** Mit zunehmender Last können zusätzliche Arbeitsprozesse zur Verarbeitung der Warteschlange eingesetzt werden. Jede Nachricht wird nur von einem der Arbeitsprozesse verarbeitet. Außerdem ermöglicht dieser entnahmebasierte Lastenausgleich eine optimale Auslastung der Workercomputer, selbst wenn sich deren Rechenleistung stark unterscheidet, da jeder Workercomputer die Nachrichten mit seinem eigenen Maximaldurchsatz aus der Warteschlange entnimmt. Dieses Schema hat auch die Bezeichnung *Konkurrierende Consumer*.
  
  ![][2]

In den folgenden Abschnitten wird der Code für die Implementierung dieser Architektur behandelt.

## <a name="set-up-the-development-environment"></a>Einrichten der Entwicklungsumgebung
Bevor Sie mit der Entwicklung von Azure-Anwendungen beginnen können, müssen Sie über die entsprechenden Tools verfügen und die Entwicklungsumgebung einrichten.

1. Installieren Sie das Azure-SDK für .NET unter [Tools und SDK herunterladen][Tools und SDK herunterladen].
2. Klicken Sie für Ihre Visual Studio-Version auf **Installieren des SDK**. Für die Schritte in diesem Tutorial wird Visual Studio 2015 verwendet.
3. Wenn Sie aufgefordert werden, das Installationsprogramm auszuführen oder zu speichern, klicken Sie auf **Ausführen**.
4. Klicken Sie im **Webplattform-Installer** auf **Installieren**, und setzen Sie die Installation fort.
5. Nach Abschluss der Installation haben Sie alles zur Hand, was Sie benötigen, um mit der Entwicklung der App zu beginnen. Das SDK enthält Tools, mit denen Sie ganz leicht Azure-Anwendungen in Visual Studio entwickeln können. Wenn Visual Studio nicht installiert ist, wird außerdem die kostenlose Version Visual Studio Express vom SDK installiert.

## <a name="create-a-namespace"></a>Erstellen eines Namespace
Der nächste Schritt ist die Einrichtung des Dienstnamespace und das Abrufen eines SAS (Shared Access Signature)-Schlüssels. Ein Namespace stellt eine Anwendungsgrenze für jede Anwendung bereit, die über Service Bus zur Verfügung steht. Das System generiert einen SAS-Schlüssel, wenn ein Namespace erstellt wird. Namespace und SAS-Schlüssel bilden gemeinsam die Anmeldeinformationen, mit denen sich der Service Bus bei der Anwendung authentifiziert.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Erstellen einer Webrolle
In diesem Abschnitt lernen Sie, wie Sie das Front-End Ihrer Anwendung erstellen. Zunächst erstellen Sie die Seiten, aus denen Ihre Anwendung besteht.
Anschließend fügen Sie Code hinzu, mit dem Elemente an eine Service Bus-Warteschlange übermittelt und Statusinformationen zur Warteschlange angezeigt werden.

### <a name="create-the-project"></a>Erstellen des Projekts
1. Starten Sie Microsoft Visual Studio mit Administratorrechten. Klicken Sie mit der rechten Maustaste auf das Programmsymbol von **Visual Studio**, und klicken Sie anschließend auf **Als Administrator ausführen**, um Visual Studio mit Administratorrechten zu starten. Für den ebenfalls in diesem Artikel behandelten Azure-Serveremulator muss Visual Studio mit Administratorrechten gestartet werden.
   
   Klicken Sie in Visual Studio im Menü **Datei** auf **Neu** und anschließend auf **Projekt**.
2. Klicken Sie im Menü **Installierte Vorlagen** unter **Visual C#** auf **Cloud** und anschließend auf **Azure-Clouddienst**. Geben Sie dem Projekt den Namen **MultiTierApp**. Klicken Sie dann auf **OK**.
   
   ![][9]
3. Doppelklicken Sie in den **.NET Framework 4.5**-Rollen auf **ASP.NET-Webrolle**.
   
   ![][10]
4. Zeigen Sie auf **WebRole1** unter **Azure-Clouddienst-Lösung**, klicken Sie auf das Stiftsymbol, und geben Sie der Webrolle den Namen **FrontendWebRole**. Klicken Sie dann auf **OK**. (Achten Sie darauf „Frontend“ mit kleinem „e“ einzugeben, nicht als „FrontEnd“.)
   
   ![][11]
5. Klicken Sie im Dialogfeld **Neues ASP.NET-Projekt** in der Liste **Vorlage auswählen** auf **MVC**.
   
   ![][12]
6. Klicken Sie ebenfalls im Dialogfeld **Neues ASP.NET-Projekt** auf die Schaltfläche **Authentifizierung ändern**. Klicken Sie im Dialogfeld **Authentifizierung ändern** auf **Keine Authentifizierung** und dann auf **OK**. Für dieses Tutorial stellen Sie eine App bereit, für die keine Benutzeranmeldung erforderlich ist.
   
    ![][16]
7. Klicken Sie im Dialogfeld **Neues ASP.NET-Projekt** auf **OK**, um das Projekt zu erstellen.
8. Klicken Sie im **Projektmappen-Explorer** im Projekt **FrontendWebRole** mit der rechten Maustaste auf **Verweise**, und klicken Sie dann auf **NuGet-Pakete verwalten**.
9. Klicken Sie auf die Registerkarte **Durchsuchen**, und suchen Sie nach `Microsoft Azure Service Bus`. Klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen.
   
   ![][13]
   
   Beachten Sie, dass die benötigten Clientassemblys nun referenziert sind und einige neue Codedateien hinzugefügt wurden.
10. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Modelle**, und klicken Sie anschließend auf **Hinzufügen** und dann auf **Klasse**. Geben Sie in das Feld **Name** den Namen **OnlineOrder.cs** ein. Klicken Sie anschließend auf **Hinzufügen**.

### <a name="write-the-code-for-your-web-role"></a>Schreiben des Codes für Ihre Webrolle
In diesem Abschnitt erstellen Sie die verschiedenen Seiten, aus denen Ihre Anwendung besteht.

1. Ersetzen Sie in der Datei "OnlineOrder.cs" in Visual Studio die bestehende Namespacedefinition durch den folgenden Code:
   
   ```
   namespace FrontendWebRole.Models
   {
       public class OnlineOrder
       {
           public string Customer { get; set; }
           public string Product { get; set; }
       }
   }
   ```
2. Doppelklicken Sie im **Projektmappen-Explorer** auf **Controllers\HomeController.cs**. Fügen Sie die folgenden **using**-Anweisungen am Anfang Ihrer Datei hinzu, um die Namespaces für Ihr neu erstelltes Modell sowie Service Bus einzuschließen.
   
   ```
   using FrontendWebRole.Models;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   ```
3. Ersetzen Sie in der Datei "HomeController.cs" in Visual Studio die bestehende Namespacedefinition ebenfalls durch den folgenden Code. Dieser Code enthält die Methoden für die Übermittlung von Elementen in die Warteschlange.
   
   ```
   namespace FrontendWebRole.Controllers
   {
       public class HomeController : Controller
       {
           public ActionResult Index()
           {
               // Simply redirect to Submit, since Submit will serve as the
               // front page of this application.
               return RedirectToAction("Submit");
           }
   
           public ActionResult About()
           {
               return View();
           }
   
           // GET: /Home/Submit.
           // Controller method for a view you will create for the submission
           // form.
           public ActionResult Submit()
           {
               // Will put code for displaying queue message count here.
   
               return View();
           }
   
           // POST: /Home/Submit.
           // Controller method for handling submissions from the submission
           // form.
           [HttpPost]
           // Attribute to help prevent cross-site scripting attacks and
           // cross-site request forgery.  
           [ValidateAntiForgeryToken]
           public ActionResult Submit(OnlineOrder order)
           {
               if (ModelState.IsValid)
               {
                   // Will put code for submitting to queue here.
   
                   return RedirectToAction("Submit");
               }
               else
               {
                   return View(order);
               }
           }
       }
   }
   ```
4. Klicken Sie im Menü **Build** auf **Projektmappe erstellen**, um die Richtigkeit Ihrer bisherigen Arbeit zu überprüfen.
5. Erstellen Sie anschließend die Ansicht für die zuvor erstellte `Submit()`-Methode. Klicken Sie mit der rechten Maustaste in die `Submit()`-Methode (Überladung von `Submit()` ohne Parameter), und wählen Sie **Ansicht hinzufügen**.
   
   ![][14]
6. Ein Dialogfeld zum Erstellen der Ansicht wird geöffnet. Wählen Sie in der Liste **Vorlage** die Option **Erstellen** aus. Klicken Sie in der Liste **Modellklasse** auf die **OnlineOrder**-Klasse.
   
   ![][15]
7. Klicken Sie auf **Hinzufügen**.
8. Ändern Sie nun den angezeigten Namen Ihrer Anwendung. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei **Views\Shared\\_Layout.cshtml**, um sie im Visual Studio-Editor zu öffnen.
9. Ersetzen Sie alle Vorkommnisse von **My ASP.NET Application** durch **LITWARE's Products**.
10. Entfernen Sie die Links **Home**, **About** und **Contact**. Löschen Sie den hervorgehobenen Code:
    
    ![][28]
11. Erweitern Sie anschließend die Übermittlungsseite um einige Informationen zur Warteschlange. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei **Views\Home\Submit.cshtml**, um sie im Visual Studio-Editor zu öffnen. Fügen Sie nach `<h2>Submit</h2>` die folgende Zeile hinzu. Derzeit ist `ViewBag.MessageCount` leer. Sie werden diesen Bereich später ausfüllen.
    
    ```
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```
12. Sie haben nun Ihre GUI implementiert. Drücken Sie F5****, um Ihre Anwendung auszuführen und zu prüfen, ob diese korrekt angezeigt wird.
    
    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a>Schreiben des Codes für die Übermittlung von Elementen an die Service Bus-Warteschlange
Sie fügen nun den Code für die Übermittlung von Elementen in die Warteschlange hinzu. Zuerst erstellen Sie die Klasse mit den Verbindungsinformationen für die Service Bus-Warteschlange. Anschließend initialisieren Sie Ihre Verbindung in „Global.aspx.cs“. Zuletzt aktualisieren Sie den zuvor in „HomeController.cs“ erstellten Übermittlungscode, um die Elemente an die Service Bus-Warteschlange zu übermitteln.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **FrontendWebRole** (klicken Sie auf das Projekt, nicht auf die Rolle). Klicken Sie auf **Hinzufügen** und anschließend auf **Klasse**.
2. Geben Sie der Klasse den Namen **QueueConnector.cs**. Klicken Sie auf **Hinzufügen**, um die Klasse zu erstellen.
3. Sie fügen nun den Code hinzu, der Ihre Verbindungsinformationen kapselt und die Verbindung zur Service Bus-Warteschlange initialisiert. Ersetzen Sie den gesamten Inhalt von „QueueConnector.cs“ durch den folgenden Code, und geben Sie Werte für `your Service Bus namespace` (Ihr Namespacename) und `yourKey` ein. Letzterer ist der **Primärschlüssel**, den Sie über das Azure-Portal abgerufen haben.
   
   ```
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Web;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   
   namespace FrontendWebRole
   {
       public static class QueueConnector
       {
           // Thread-safe. Recommended that you cache rather than recreating it
           // on every request.
           public static QueueClient OrdersQueueClient;
   
           // Obtain these values from the portal.
           public const string Namespace = "your Service Bus namespace";
   
           // The name of your queue.
           public const string QueueName = "OrdersQueue";
   
           public static NamespaceManager CreateNamespaceManager()
           {
               // Create the namespace manager which gives you access to
               // management operations.
               var uri = ServiceBusEnvironment.CreateServiceUri(
                   "sb", Namespace, String.Empty);
               var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                   "RootManageSharedAccessKey", "yourKey");
               return new NamespaceManager(uri, tP);
           }
   
           public static void Initialize()
           {
               // Using Http to be friendly with outbound firewalls.
               ServiceBusEnvironment.SystemConnectivity.Mode =
                   ConnectivityMode.Http;
   
               // Create the namespace manager which gives you access to
               // management operations.
               var namespaceManager = CreateNamespaceManager();
   
               // Create the queue if it does not exist already.
               if (!namespaceManager.QueueExists(QueueName))
               {
                   namespaceManager.CreateQueue(QueueName);
               }
   
               // Get a client to the queue.
               var messagingFactory = MessagingFactory.Create(
                   namespaceManager.Address,
                   namespaceManager.Settings.TokenProvider);
               OrdersQueueClient = messagingFactory.CreateQueueClient(
                   "OrdersQueue");
           }
       }
   }
   ```
4. Stellen Sie nun sicher, dass Ihre **Initialize**-Methode aufgerufen wird. Doppelklicken Sie im **Projektmappen-Explorer** auf **Global.asax\Global.asax.cs**.
5. Fügen Sie am Ende der **Application_Start**-Methode die folgende Codezeile hinzu.
   
   ```
   FrontendWebRole.QueueConnector.Initialize();
   ```
6. Zuletzt aktualisieren Sie den zuvor erstellten Webcode, um die Elemente an die Warteschlange zu übermitteln. Doppelklicken Sie im **Projektmappen-Explorer** auf **Controllers\HomeController.cs**.
7. Aktualisieren Sie die `Submit()`-Methode (Überladung ohne Parameter) wie folgt, um die Nachrichtenanzahl für die Warteschlange abzurufen.
   
   ```
   public ActionResult Submit()
   {
       // Get a NamespaceManager which allows you to perform management and
       // diagnostic operations on your Service Bus queues.
       var namespaceManager = QueueConnector.CreateNamespaceManager();
   
       // Get the queue, and obtain the message count.
       var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
       ViewBag.MessageCount = queue.MessageCount;
   
       return View();
   }
   ```
8. Aktualisieren Sie die `Submit(OnlineOrder order)`-Methode (Überladung mit einem Parameter) wie folgt, um Bestellinformationen an die Warteschlange zu übermitteln.
   
   ```
   public ActionResult Submit(OnlineOrder order)
   {
       if (ModelState.IsValid)
       {
           // Create a message from the order.
           var message = new BrokeredMessage(order);
   
           // Submit the order.
           QueueConnector.OrdersQueueClient.Send(message);
           return RedirectToAction("Submit");
       }
       else
       {
           return View(order);
       }
   }
   ```
9. Führen Sie die Anwendung nun erneut aus. Bei jeder Übermittlung einer Bestellung steigt der Nachrichtenzähler an.
   
   ![][18]

## <a name="create-the-worker-role"></a>Erstellen einer Workerrolle
Sie werden nun die Workerrolle zur Verarbeitung der übermittelten Nachrichten erstellen. In diesem Beispiel wird die Visual Studio-Projektvorlage **Workerrolle mit Service Bus-Warteschlange** verwendet. Sie haben die erforderlichen Anmeldeinformationen bereits aus dem Portal abgerufen.

1. Stellen Sie sicher, dass Sie Visual Studio mit Ihrem Azure-Konto verbunden haben.
2. Klicken Sie in Visual Studio im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Rollen** im Projekt **MultiTierApp**.
3. Klicken Sie auf **Hinzufügen** und anschließend auf **Neues Workerrollenprojekt**. Das Dialogfeld **Neues Rollenprojekt hinzufügen** wird geöffnet.
   
   ![][26]
4. Klicken Sie im Dialogfeld **Neues Rollenprojekt hinzufügen** auf **Workerrolle mit Service Bus-Warteschlange**.
   
   ![][23]
5. Geben Sie in das Feld **Name** den Namen **OrderProcessingRole** für das Projekt ein. Klicken Sie anschließend auf **Hinzufügen**.
6. Kopieren Sie die Verbindungszeichenfolge, die Sie in Schritt 9 des Abschnitts „Erstellen eines Service Bus-Namespaces“ abgerufen haben, in die Zwischenablage.
7. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die in Schritt 5 erstellte **OrderProcessingRole** (klicken Sie auf **OrderProcessingRole** unter **Rollen**, nicht auf die Klasse). Klicken Sie anschließend auf **Eigenschaften**.
8. Klicken Sie auf der Registerkarte **Einstellungen** im Dialogfeld **Eigenschaften** in das Textfeld **Wert** für **Microsoft.ServiceBus.ConnectionString**, und fügen Sie den in Schritt 6 kopierten Endpunktwert ein.
   
   ![][25]
9. Erstellen Sie die **OnlineOrder**-Klasse, um die Nachrichten abzubilden, während diese aus der Warteschlange verarbeitet werden. Sie können dabei eine zuvor erstellte Klasse wiederverwenden. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die **OrderProcessingRole**-Klasse (Rechtsklick auf die Klasse, nicht die Rolle). Klicken Sie auf **Hinzufügen** und anschließend auf **Vorhandenes Element**.
10. Durchsuchen Sie den Unterordner nach **FrontendWebRole\Models**, und doppelklicken Sie dann auf **OnlineOrder.cs**, um die Klasse dem Projekt hinzuzufügen.
11. Ändern Sie wie im folgenden Code gezeigt in **WorkerRole.cs** den Wert der Variablen **QueueName** von `"ProcessingQueue"` in `"OrdersQueue"`.
    
    ```
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```
12. Fügen Sie die folgende using-Anweisung am Anfang der Datei "WorkerRole.cs" hinzu.
    
    ```
    using FrontendWebRole.Models;
    ```
13. Ersetzen Sie in der Funktion `Run()` im `OnMessage()`-Aufruf den Inhalt der `try`-Klausel durch den folgenden Code.
    
    ```
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```
14. Ihre Anwendung ist nun fertig. Sie können die vollständige Anwendung testen, indem Sie im Projektmappen-Explorer mit der rechten Maustaste auf das MultiTierApp-Projekt klicken und dann **Set as Startup Project** auswählen und F5 drücken. Beachten Sie, dass die Nachrichtenzahl nicht ansteigt, da die Workerrolle die Elemente in der Warteschlange verarbeitet und als abgeschlossen markiert. Sie können die Ausgabe Ihrer Workerrolle in der Azure-Serveremulator-GUI anzeigen. Klicken Sie dazu mit der rechten Maustaste in den Infobereich Ihrer Taskleiste, und wählen Sie **Serveremulator-GUI anzeigen**.
    
    ![][19]
    
    ![][20]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Servicebus finden Sie in den folgenden Ressourcen:  

* [Azure Service Bus][sbmsdn]  
* [Seite für Service Bus-Dienst][sbwacom]  
* [Einsatz von Service Bus-Warteschlangen][sbwacomqhowto]  

Weitere Informationen zu Szenarien mit mehreren Ebenen finden Sie unter:  

* [.NET-Anwendungen mit mehreren Ebenen mithilfe von Speichertabellen, Warteschlangen und Blobs][mutitierstorage]  

[0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-01.png
[1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
[2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
[Tools und SDK herunterladen]: http://go.microsoft.com/fwlink/?LinkId=271920


[GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
[Microsoft.WindowsAzure.Configuration.CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx
[NamespaceMananger]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx

[QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx

[TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx

[EventHubClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx

[9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
[10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
[11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
[12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
[13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
[14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
[15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
[16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
[17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-36.png
[18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-37.png

[19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
[20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
[23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
[25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
[26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
[28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

[sbmsdn]: http://msdn.microsoft.com/library/azure/ee732537.aspx  
[sbwacom]: /documentation/services/service-bus/  
[sbwacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
[mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36



<!--HONumber=Nov16_HO2-->


