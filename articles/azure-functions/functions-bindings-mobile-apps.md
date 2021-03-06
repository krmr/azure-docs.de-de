---
title: Mobile Apps-Bindungen in Azure Functions | Microsoft Docs
description: Erfahren Sie, wie Azure Mobile Apps-Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: ''
tags: ''
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur

ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/30/2016
ms.author: glenga

---
# Mobile Apps-Bindungen in Azure Functions
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren und Codieren von Azure Mobile Apps-Bindungen in Azure Functions.

[!INCLUDE [Einführung](../../includes/functions-bindings-intro.md)]

Mit mobilen Azure App Service-Apps können Sie Tabellenendpunkt-Daten auf mobilen Clients verfügbar machen. Dieselben Tabellendaten können sowohl in Eingabe- als auch in Ausgabebindungen in Azure Functions verwendet werden. Da ein dynamisches Schema unterstützt wird, eignet sich eine mobile Node.js-Back-End-App besonders zum Verfügbarmachen von Tabellendaten für die Verwendung mit Ihren Funktionen. Das dynamische Schema ist standardmäßig aktiviert und sollte in einer mobilen Produktions-App deaktiviert werden. Weitere Informationen zu Tabellenendpunkten in einem Node.js-Back-End finden Sie unter [Übersicht: Tabellenvorgänge](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations). In Mobile Apps unterstützt das Node.js-Back-End das portalinterne Durchsuchen und Bearbeiten von Tabellen. Weitere Informationen finden Sie im Abschnitt zur [portalinternen Bearbeitung](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#in-portal-editing) im Thema „Node.js SDK“. Beim Verwenden einer mobilen .NET-Back-End-App mit Azure Functions müssen Sie das Datenmodell entsprechend den Anforderungen der Funktion manuell aktualisieren. Weitere Informationen zu Tabellenendpunkten in einer mobilen .NET-Back-End-App finden Sie unter [Vorgehensweise: Definieren eines Tabellencontrollers](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller) im Thema „.NET-Back-End-SDK“.

## Erstellen einer Umgebungsvariablen für Ihre Back-End-URL für mobile Apps
Für Mobile Apps-Bindungen müssen Sie derzeit eine Umgebungsvariable erstellen, die die URL des Back-Ends der mobilen App selbst zurückgibt. Diese URL finden Sie im [Azure-Portal](https://portal.azure.com), wenn Sie Ihre mobile App suchen und das Blatt öffnen.

![Mobile Apps-Blatt im Azure-Portal](./media/functions-bindings-mobile-apps/mobile-app-blade.png)

So legen Sie diese URL als Umgebungsvariable in Ihrer Funktions-App fest:

1. Klicken Sie in Ihrer Funktionen-App im [Azure Functions-Portal](https://functions.azure.com/signin) auf **Function app settings** (Funktionen-App-Einstellungen) > **Go to App Service settings** (Zu App Service-Einstellungen wechseln).
   
    ![Funktions-App-Einstellungenblatt](./media/functions-bindings-mobile-apps/functions-app-service-settings.png)
2. Klicken Sie in Ihrer Funktionen-App auf **Alle Einstellungen**, scrollen Sie nach unten zu **Anwendungseinstellungen**, und geben Sie dann unter **App-Einstellungen** einen neuen **Namen** für die Umgebungsvariable ein, fügen Sie die URL in **Wert** ein, wobei Sie das HTTPS-Schema verwenden, klicken Sie dann auf **Speichern**, und schließen Sie das Funktionen-App-Blatt im Functions-Portal.
   
    ![Hinzufügen einer App-Einstellung zu einer Umgebungsvariablen](./media/functions-bindings-mobile-apps/functions-app-add-app-setting.png)

Nun können Sie diese neue Umgebungsvariable als *Verbindungsfeld* in Ihren Bindungen festlegen.

## <a id="mobiletablesapikey"></a> Verwenden Sie einen API-Schlüssel, um den Zugriff auf Ihre Mobile Apps-Tabellenendpunkte zu schützen.
In Azure Functions ermöglichen Ihnen Bindungen mobiler Tabellen, einen API-Schlüssel anzugeben. Dabei handelt es sich um einen gemeinsamen geheimen Schlüssel für Ihre Funktionen. Der unbefugte Zugriff aus anderen Apps wird so verhindert. Mobile Apps bieten keine integrierte Unterstützung für die Authentifizierung von API-Schlüsseln. Sie können jedoch einen API-Schlüssel in der mobilen Node.js-Back-End-App implementieren, indem Sie die Beispiele unter [Azure App Service Mobile Apps backend implementing an API key](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key) (Implementieren eines API-Schlüssels mit dem Azure Mobile App Service-Apps-Back-End) befolgen. In einer [mobilen .NET-Back-End-App](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key) können Sie auf ähnliche Weise einen API-Schlüssel implementieren.

> [!IMPORTANT]
> Dieser API-Schlüssel darf nicht mit Ihren mobilen App-Clients verteilt werden, er sollte nur sicher an dienstseitige Clients wie Azure Functions verteilt werden.
> 
> 

## <a id="mobiletablesinput"></a> Azure Mobile Apps-Eingabebindung
Eingabebindungen können einen Datensatz aus einem mobilen Tabellenendpunkt laden und direkt an Ihre Bindung übergeben. Die Datensatz-ID wird anhand des Triggers ermittelt, der die Funktion aufgerufen hat. In einer C#-Funktion werden alle Änderungen am Datensatz automatisch wieder an die Tabelle zurückgesendet, wenn die Funktion erfolgreich beendet wird.

#### „function.json“ für Mobile Apps-Eingabebindung
Die Datei *function.json* unterstützt die folgenden Eigenschaften:

* `name`: Variablenname, der im Funktionscode für den neuen Datensatz verwendet wird.
* `type`: Der Bindungstyp muss auf *mobileTable* festgelegt werden.
* `tableName`: Tabelle, in der der neue Datensatz erstellt wird.
* `id`: ID des abzurufenden Datensatzes. Diese Eigenschaft unterstützt Bindungen wie `{queueTrigger}`, die den Zeichenfolgenwert der Warteschlangennachricht als Datensatz-ID verwenden.
* `apiKey`: Zeichenfolge, die der Anwendungseinstellung entspricht, die den optionalen API-Schlüssel für die mobile App angibt. Dies ist erforderlich, wenn Ihre mobile App einen API-Schlüssel verwendet, um den Clientzugriff einzuschränken.
* `connection`: Zeichenfolge, die dem Namen der Umgebungsvariablen in den Anwendungseinstellungen entspricht, der die URL Ihres Back-Ends für mobile Apps angibt.
* `direction`: Bindungsrichtung, die auf *in* festgelegt werden muss.

Beispiel für die Datei *function.json*:

    {
      "bindings": [
        {
          "name": "record",
          "type": "mobileTable",
          "tableName": "MyTable",
          "id" : "{queueTrigger}",
          "connection": "My_MobileApp_Url",
          "apiKey": "My_MobileApp_Key",
          "direction": "in"
        }
      ],
      "disabled": false
    }

#### Azure Mobile Apps-Codebeispiel für einen C#-Warteschlangentrigger
Basierend auf dem vorherigen Beispiel für „function.json“ ruft die Eingabebindung den Datensatz von einem Mobile Apps-Tabellenendpunkt mit der ID ab, die der Zeichenfolge der Warteschlangennachricht entspricht, und übergibt ihn an den *record*-Parameter. Wenn der Datensatz nicht gefunden wird, ist der Parameter NULL. Der Datensatz wird dann mit dem neuen *Text*-Wert aktualisiert, wenn die Funktion beendet wird.

    #r "Newtonsoft.Json"    
    using Newtonsoft.Json.Linq;

    public static void Run(string myQueueItem, JObject record)
    {
        if (record != null)
        {
            record["Text"] = "This has changed.";
        }    
    }

#### Azure Mobile Apps-Codebeispiel für einen Node.js-Warteschlangentrigger
Basierend auf dem vorherigen Beispiel für „function.json“ ruft die Eingabebindung den Datensatz von einem Mobile Apps-Tabellenendpunkt mit der ID ab, die der Zeichenfolge der Warteschlangennachricht entspricht, und übergibt ihn an den *record*-Parameter. In Node.js-Funktionen werden aktualisierte Datensätze nicht an die Tabelle zurückgesendet. Dieses Codebeispiel schreibt den abgerufenen Datensatz in das Protokoll.

    module.exports = function (context, input) {    
        context.log(context.bindings.record);
        context.done();
    };


## <a id="mobiletablesoutput"></a>Azure Mobile Apps-Ausgabebindung
Ihre Funktion kann einen Datensatz mithilfe einer Ausgabebindung in einen Mobile Apps-Tabellenendpunkt schreiben.

#### „function.json“ für Mobile Apps-Ausgabebindung
Die Datei „function.json“ unterstützt die folgenden Eigenschaften:

* `name`: Variablenname, der im Funktionscode für den neuen Datensatz verwendet wird.
* `type`: Bindungstyp, der auf *mobileTable* festgelegt werden muss.
* `tableName`: Tabelle, in der der neue Datensatz erstellt wird.
* `apiKey`: Zeichenfolge, die der Anwendungseinstellung entspricht, die den optionalen API-Schlüssel für die mobile App angibt. Dies ist erforderlich, wenn Ihre mobile App einen API-Schlüssel verwendet, um den Clientzugriff einzuschränken.
* `connection`: Zeichenfolge, die dem Namen der Umgebungsvariablen in den Anwendungseinstellungen entspricht, der die URL Ihres Back-Ends für mobile Apps angibt.
* `direction`: Bindungsrichtung, die auf *out* festgelegt werden muss.

Beispiel für „function.json“:

    {
      "bindings": [
        {
          "name": "record",
          "type": "mobileTable",
          "tableName": "MyTable",
          "connection": "My_MobileApp_Url",
          "apiKey": "My_MobileApp_Key",
          "direction": "out"
        }
      ],
      "disabled": false
    }

#### Azure Mobile Apps-Codebeispiel für einen C#-Warteschlangentrigger
Dieses C#-Codebeispiel fügt einen neuen Datensatz mit einer *Text*-Eigenschaft in den Mobile Apps-Tabellenendpunkt in die Tabelle ein, die in der obigen Bindung angegeben ist.

    public static void Run(string myQueueItem, out object record)
    {
        record = new {
            Text = $"I'm running in a C# function! {myQueueItem}"
        };
    }

#### Azure Mobile Apps-Codebeispiel für einen Node.js-Warteschlangentrigger
Dieses Node.js-Codebeispiel fügt einen neuen Datensatz mit einer *text*-Eigenschaft in den Mobile Apps-Tabellenendpunkt in die Tabelle ein, die in der obigen Bindung angegeben ist.

    module.exports = function (context, input) {

        context.bindings.record = {
            text : "I'm running in a Node function! Data: '" + input + "'"
        }   

        context.done();
    };

## Nächste Schritte
[!INCLUDE [Nächste Schritte](../../includes/functions-bindings-next-steps.md)]

<!---HONumber=AcomDC_0907_2016-->