---
title: Erfahren Sie mehr über die BizTalk-Regeln API-App und erstellen Sie diese in Ihrer Logik-App | Microsoft Docs
description: Dieses Thema behandelt die Funktionen des BizTalk-Regel-Connectors und bietet Anleitungen zu dessen Verwendung
services: logic-apps
documentationcenter: .net,nodejs,java
author: anuragdalmia
manager: erikre
editor: ''

ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 04/20/2016
ms.author: andalmia

---
# BizTalk-Regeln
[!INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Geschäftsregeln umfassen die Richtlinien und Entscheidungen, welche die Geschäftsprozesse steuern. Diese Richtlinien können formal in Verfahrenshandbüchern, Verträgen oder Vereinbarungen festgelegt werden oder als Wissen oder Erfahrung der Mitarbeiter vorliegen. Diese Richtlinien sind dynamisch und verändern sich eventuell im Laufe der Zeit aufgrund von Änderungen an Geschäftsplänen, Vorschriften oder aus anderen Gründen.

Die Implementierung dieser Richtlinien in herkömmlichen Programmiersprachen erfordert viel Zeit und Koordinationsaufwand, und Benutzer ohne Programmierkenntnisse können an der Erstellung und Wartung von Geschäftsrichtlinien nicht teilnehmen. BizTalk-Geschäftsregeln bieten eine Möglichkeit, diese Richtlinien schnell zu implementieren und den Rest des Geschäftsprozesses zu entkoppeln. Auf diese Weise können die erforderlichen Änderungen an den Geschäftsrichtlinien ohne Auswirkungen auf den übrigen Geschäftsprozesses vorgenommen werden.

## Gründe für Regeln
Es gibt drei wichtige Gründe für die Verwendung von BizTalk-Geschäftsregeln im Geschäftsprozess:

* Entkoppeln der Geschäftslogik vom Anwendungscode
* Mehr Kontrolle über die Verwaltung der Geschäftslogik für Business Analysts
* Schnellere Integration von Änderungen an der Geschäftslogik in die Produktion

## Regelkonzepte
## Vokabular
Ein *Vokabular* ist eine Auflistung von Definitionen, die aus Anzeigenamen für die in Regelbedingungen und Aktionen verwendeten IT-Objekte besteht. Vokabulardefinitionen erleichtern das Lesen, Verstehen und die gemeinsame Nutzung der Regeln von Personen in einem bestimmten Fachbereich.

Vokabulare sollen die Lücke zwischen Geschäftssemantik und Implementierung schließen. Beispielsweise könnte eine Datenbindung für einen Genehmigungsstatus auf eine bestimmte Spalte in einer bestimmten Zeile in einer bestimmten Datenbank verweisen, die als SQL-Abfrage dargestellt wird. Anstatt diese Art der komplexen Darstellung in eine Regel einzufügen, können Sie stattdessen eine Vokabulardefinition mit dem Anzeigenamen "Status" erstellen, die dieser Datenbindung zugeordnet ist. Anschließend können Sie "Status" in einer beliebigen Anzahl von Regeln verwenden, und das Regelmodul kann die entsprechenden Daten aus der Tabelle abrufen.

## Regel
Geschäftsregeln sind deklarative Anweisungen, die die Durchführung von Geschäftsprozessen steuern. Eine Regel besteht aus einer Bedingung und Aktionen. Die Bedingung wird ausgewertet, und wenn sie auf "true" ausgewertet wird, initiiert das Regelmodul eine oder mehrere Aktionen. Regeln im Geschäftsregel-Framework werden im folgenden Format definiert:

*IF* *Bedingung* *THEN* *Aktion*

Betrachten Sie das folgende Beispiel:

*IF Menge kleiner gleich verfügbare Mittel* *THEN Transaktion durchführen und Beleg drucken*

Diese Regel bestimmt, ob eine Transaktion durchgeführt wird, indem sie Geschäftslogik in Form eines Vergleichs zwischen zwei monetären Werten anwendet, einem Transaktionsbetrag und den zur Verfügung stehenden Mitteln. 
Sie können anhand der Geschäftsregel Geschäftsregeln erstellen, ändern und bereitstellen. Alternativ können Sie die vorherigen Aufgaben programmgesteuert ausführen.

### Bedingungen
Eine Bedingung ist ein boolescher Ausdruck (true/false), der aus einem oder mehreren Prädikaten besteht. 
In diesem Beispiel wird das Prädikat kleiner gleich auf die Menge und die verfügbaren Mittel angewendet. Diese Bedingung wird immer entweder als "true" oder "false" ausgewertet. 
Prädikate können mit den logischen Operatoren AND, OR und NOT zu einem logischen Ausdruck kombiniert werden, der möglicherweise sehr umfangreich ist, aber immer als "true" oder "false" ausgewertet wird.

### Aktionen
Aktionen sind die funktionalen Folgen der Bedingungsauswertung. Wenn eine Bedingung erfüllt ist, wird mindestens eine entsprechende Aktion initiiert. 
In unserem Beispiel sind "Transaktion durchführen" und "Beleg drucken" Aktionen, die nur dann durchgeführt werden, wenn die Bedingung (in diesem Fall "Betrag kleiner gleich verfügbare Mittel") wahr, also "true", ist. 
Aktionen werden im Geschäftsregel-Framework dargestellt, indem bestimmte Vorgänge an XML-Dokumenten durchgeführt werden.

## Richtlinie
Eine Richtlinie ist eine logische Gruppierung von Regeln. Sie erstellen eine Richtlinie, speichern sie, testen sie und verwenden sie in einer Produktionsumgebung, wenn Sie mit den Ergebnissen zufrieden sind.

### Richtlinienerstellung
Sie können Richtlinien im Regelportal verfassen. Eine Richtlinie kann einen beliebig großen Satz von Regeln enthalten. In der Regel erstellen Sie eine Richtlinie jedoch aus Regeln, die zu einer bestimmten Geschäftsdomäne innerhalb des Kontexts der Anwendung gehören, die die Richtlinie verwenden soll.

### Testen der Richtlinie
Bevor die Richtlinie in einer Produktionsumgebung verwendet wird, können Sie einen effektiven Testlauf ausführen. Das Regelportal ermöglicht Ihnen das Bereitstellen von Eingaben für eine Richtlinie, das Ausführen der Richtlinie und das Anzeigen der Ausgabe. Die Ausgabe umfasst Protokolle, die Ausführung der Regel, die Bedingungsauswertung und die daraus resultierenden Ausgaben.

## Beispielszenario - Versicherungsansprüche
Sehen wir uns ein Beispielszenario an, während wir dafür die Geschäftslogik erstellen.

![Alt text][1]

In einem sehr einfachen Versicherungsszenario übermittelt der Antragsteller seinen Versicherungsanspruch (über einen beliebigen Client wie Website, Telefon-App usw.). Diese Anspruchserhebung wird an die Einheit zur Schadenbearbeitung gesendet. Basierend auf dem Ergebnis der Bearbeitung wird der Anspruch entweder genehmigt, abgewiesen oder zur weiteren manuellen Verarbeitung weitergeleitet. Die Einheit zur Schadenbearbeitung wäre in unserem Szenario für die Geschäftslogik für das System zuständig. Wenn wir diese Einheit näher betrachten, können wir Folgendes erkennen:

![Alt text][2]

Verwenden wir jetzt Geschäftsregeln zum Implementieren dieser Geschäftslogik.

## Erstellen der Regel-API-App
1. Melden Sie sich beim Azure-Portal an.
2. Wählen Sie „Neu“ -> „Marketplace“ aus, und suchen Sie dann nach *BizTalk Rules*.
3. Wählen Sie „BizTalk Rules“ in der Ergebnisliste aus. Das Blatt „BizTalk Rules“ wird geöffnet.
4. Wählen Sie die Schaltfläche *Erstellen* ![Alt text][3].
5. Geben Sie auf dem neuen Blatt, das geöffnet wird, Folgendes ein:
   1. Name – Geben Sie einen Namen für die Regel-API-App ein.
   2. App Service-Plan – Wählen Sie einen App Service-Plan aus, oder erstellen Sie einen neuen.
   3. Preisstufe – Wählen Sie die Preisstufe, der diese App angehören soll.
   4. Ressourcengruppe – Wählen oder erstellen Sie die Ressourcengruppe, der die App angehören soll.
   5. Abonnement – Wählen Sie das zu verwendende Abonnement aus.
   6. Standort – Wählen Sie den geografischen Standort aus, an dem Sie die App bereitstellen möchten.
6. Klicken Sie auf *Erstellen*. Ihre BizTalk-Regel-API-App würde innerhalb weniger Minuten erstellt.

## Vokabularerstellung
Nach dem Erstellen einer BizTalk-Regel-API-App besteht der nächste Schritt im Erstellen von Vokabularen. Diese Übung wird erwartungsgemäß eher vom Entwickler durchgeführt. Dazu müssen folgende Schritte ausgeführt werden:

1. Starten Sie die BizTalk-Regel-API-App über das Portal, indem Sie „Durchsuchen“ -> „API-Apps“ -> <Ihre Regel-API-App> öffnen. Das Dashboard der Regel-API-App wird angezeigt und sieht etwa folgendermaßen aus:
   
   ![Alt text][4]

2\. Wählen Sie „Vocabulary definitions“ aus. Der Bildschirm zum Verfassen des Vokabulars wird angezeigt.  
3\. Wählen Sie „Hinzufügen“ aus, um neue Vokabulardefinitionen hinzuzufügen. Derzeit werden zwei Arten von Vokabulardefinitionen unterstützt – literal und XML.

## Literale Definition
1. Nach dem Klicken auf "Hinzufügen" wird ein neues Blatt "Definition hinzufügen" geöffnet. Geben Sie die folgenden Werte ein:
   1. Name – Es werden nur alphanumerische Zeichen ohne Sonderzeichen erwartet. Dieser Name sollte auch in Ihrer Liste vorhandener Vokabulardefinitionen eindeutig sein.
   2. Beschreibung – Optionales Feld.
   3. Definitionstyp – Es werden 2 Typen unterstützt. Wählen Sie in diesem Beispiel "Literal".
   4. Datentyp – Ermöglicht Benutzern das Auswählen des Datentyps der Definition. Derzeit werden 4 Datentypen unterstützt: 
      i. String – Diese Werte müssen in doppelte Anführungszeichen (“Beispielzeichenfolge”) eingeschlossen werden. 
      ii. Boolean – Kann entweder "true" oder "false" sein. 
      iii. Number – Kann jede Dezimalzahl sein. 
      iv. DateTime – Die Definition ist vom Typ Datum/Uhrzeit. Daten müssen in diesem Format eingegeben werden: mm/tt/jjjj hh:mm:ss AM\\PM.  
   5. Input – Hier geben Sie den Wert Ihrer Definition ein. Die hier eingegebenen Werte müssen dem ausgewählten Datentyp entsprechen. Sie können einen einzelnen Wert, einen durch Kommas getrennten Satz von Werten oder einen Bereich von Werten mit dem Schlüsselwort *to* eingeben. Beispielsweise können Sie den eindeutigen Wert 1, den Satz 1, 2, 3 oder einen Bereich von 1 bis 5 angeben. Beachten Sie, dass Bereiche nur für Zahlen unterstützt werden.
   6. Klicken Sie auf *OK*.

![Alt text][5]

## XML-Definition
Wenn als Vokabulartyp XML ausgewählt wird, muss folgende Eingabe bereitgestellt werden:  
  a.     Schema – Durch Klicken auf diese Option wird ein neues Blatt geöffnet, in dem der Benutzer entweder aus einer Liste bereits hochgeladener Schemas auswählen oder ein neues Schema hochladen kann.  
  b.     XPATH – Diese Eingabe wird erst freigegeben, wenn Sie im vorherigen Schritt ein Schema ausgewählt haben. Durch Klicken auf diese Option wird das ausgewählte Schema angezeigt. Der Benutzer kann den Knoten auswählen, für den eine Vokabulardefinition erstellt werden soll.  
  c.     FACT – Diese Eingabe legt fest, welches Datenobjekt dem Regelmodul zur Verarbeitung vorgelegt wird. Dies ist eine erweiterte Eigenschaft, die standardmäßig auf das übergeordnete Element des ausgewählten XPATH festgelegt ist. FACT ist für die Verkettung und Auflistung von Szenarien besonders wichtig.

![Alt text][6]

### Massenhinzufügen
In den oben genannten Schritten wurde die Erstellung von Vokabulardefinitionen behandelt. Nach ihrer Erstellung werden die Vokabulardefinitionen in Listenform angezeigt. Es gibt die Anforderung, mehrere Definitionen aus demselben Schema zu generieren, statt die obigen Schritte jedes Mal zu wiederholen. 
An dieser Stelle bietet sich die Funktion zur Massenhinzufügung an. Durch Auswählen von „Massenhinzufügen“ gelangen Sie zu einem neuen Blatt. Im ersten Schritt wählen Sie das Schema aus, für das mehrere Definitionen erstellt werden sollen. Durch Auswählen dieser Option wird ein neues Blatt geöffnet, in dem Sie entweder ein Schema in einer Liste bereits hochgeladener Schemas auswählen oder ein neues Schema hochladen können. 
Jetzt wird die XPATHS-Eigenschaft entsperrt. Durch Auswählen dieser Option wird der Schema-Viewer geöffnet, in dem mehrere Knoten ausgewählt werden können. 
Die Namen für die erstellten Definitionen entsprechen standardmäßig dem Namen des ausgewählten Knotens. Sie können nach der Erstellung jederzeit geändert werden.

![Alt text][7]

## Richtlinienerstellung
Nachdem der Entwickler die erforderlichen Vokabulare erstellt hat, ist der Business Analyst erwartungsgemäß derjenige, der Geschäftsrichtlinien über das Azure-Portal erstellt.  

    1. In der erstellten Regel-App gibt es eine Richtlinienlupe. Durch Klicken gelangt der Benutzer auf die Seite zur Richtlinienerstellung.  
    2. Auf dieser Seite wird die Liste der Richtlinien angezeigt, die für diese spezielle Regel-App gelten. Der Analyst kann eine neue Richtlinie hinzufügen, indem er einfach einen Richtliniennamen eingibt und die TAB-Taste zweimal drückt. In einer einzigen Regel-API-App können sich mehrere Richtlinien befinden.  
    3. Durch Auswählen der erstellten Richtlinie gelangt der Benutzer zur Richtliniendetailseite, auf der die in der Richtlinie enthaltenen Regeln angezeigt werden.  
    ![Alt text][8]  
    4. Klicken Sie auf „Hinzufügen“, um eine neue Regel hinzuzufügen. Sie gelangen zu einem neuen Blatt.

## Regelerstellung
Eine Regel ist eine Auflistung von Bedingungs- und Aktionsanweisungen. Die Aktionen werden ausgeführt, wenn die Bedingung als wahr („true“) ausgewertet wird. Geben Sie auf dem Blatt zur Regelerstellung einen eindeutigen Regelnamen (für diese Richtlinie) und eine Beschreibung (optional) ein. 
Das Bedingungsfeld („IF“) kann zum Erstellen komplexer Bedingungsanweisungen verwendet werden. Folgende Schlüsselwörter werden unterstützt:  

1. Und – bedingter Operator  
2. Oder – bedingter Operator  
3. does\_not\_exist  
4. exists  
5. false  
6. is\_equal\_to  
7. is\_greater\_than  
8. is\_greater\_than\_equal\_to  
9. is\_in  
10. is\_less\_than  
11. is\_less\_than\_equal\_to  
12. is\_not\_in  
13. is\_not\_equal\_to  
14. mod  
15. true  

Das Aktionsfeld („THEN“) kann mehrere Anweisungen enthalten, eine pro Zeile, um Aktionen zu erstellen, die ausgeführt werden sollen. Folgende Schlüsselwörter werden unterstützt:  

1. equals  
2. false  
3. true  
4. halt  
5. mod  
6. null  
7. aktualisieren  

Das Bedingungs- und das Aktionsfeld bieten Intellisense, damit Sie schnell eine Regel erstellen können. Dies kann durch Drücken von STRG+LEERTASTE oder einfach durch den Beginn der Eingabe ausgelöst werden. Schlüsselwörter, die den eingegebenen Zeichen entsprechen, werden automatisch gefiltert und dargestellt. Das Intellisense-Fenster zeigt alle Schlüsselwörter und Vokabulardefinitionen an.  
![Alt text][9]

## Explizite Vorwärtsverkettung
BizTalk-Regeln unterstützen die explizite Vorwärtsverkettung, sodass Benutzer, die Regeln auf bestimmte Aktionen hin erneut auswerten möchten, diesen Vorgang anhand von bestimmten Schlüsselwörtern auslösen können. Folgende Schlüsselwörter werden unterstützt:  

1. <Vokabulardefinition> aktualisieren – Durch dieses Schlüsselwort werden alle Regeln neu ausgewertet, die die angegebene Vokabulardefinition in ihrer Bedingung verwenden.  
2. Halt – Dieses Schlüsselwort beendet alle Regelausführungen.

## Aktivieren/Deaktivieren von Regeln
Jede Regel in der Richtlinie kann aktiviert oder deaktiviert werden. Standardmäßig sind alle Regeln aktiviert. Deaktivierte Regeln werden während der Richtlinienausführung nicht ausgeführt. Das Aktivieren/Deaktivieren von Regeln kann entweder direkt vom Regelblatt – die Befehle stehen in der Befehlsleiste im oberen Blattbereich zur Verfügung - oder über das Kontextmenü der Richtlinie erfolgen (klicken Sie mit der rechten Maustaste auf eine Regel).

## Regelpriorität
Alle Regeln einer Richtlinie werden in ihrer Reihenfolge ausgeführt. Die Priorität der Ausführung wird durch die Reihenfolge bestimmt, in der die Regeln in der Richtlinie vorkommen. Diese Priorität kann durch einfaches Ziehen und Ablegen der Regel geändert werden.

## Testen von Richtlinien
Über den Befehl „Richtlinie testen“ auf dem Blatt „Richtlinie testen“ können Sie Ihre Richtlinien testen. In diesem Blatt wird eine Liste der Vokabulardefinitionen angezeigt, die in der Richtlinie verwendet werden, die eine Benutzereingabe erfordert. Benutzer können die Werte für diese Eingaben für ihre Testszenarien manuell hinzufügen. Alternativ können Benutzer auch den Import von Test-XMLs als Eingabe auswählen. Sobald alle Eingaben vorliegen, kann der Test ausgeführt werden, und die Ausgaben für die einzelnen Vokabulardefinitionen werden zum einfachen Vergleich in der Ausgabespalte angezeigt. Um benutzerfreundliche Protokolle für Business Analysts anzuzeigen, klicken Sie zum Anzeigen der Ausführungsprotokolle auf "Protokolle anzeigen". Zum Speichern der Protokolle steht die Option "Ausgabe speichern" zur Verfügung, um alle mit dem Test verknüpften Daten zur unabhängigen Analyse zu speichern.

## Verwenden von Regeln in Logik-Apps
Nachdem die Richtlinie erstellt und getestet wurde, steht sie jetzt zum Gebrauch bereit. Sie können eine neue Logik-App erstellen, indem Sie links auf der Startseite des Portals die Option „Logik-Apps“ auswählen. Nachdem die Logik-App erstellt wurde, starten Sie sie, und wählen Sie dann *Triggers and Actions* aus. Sie können dann die Vorlage *Create from Scratch* auswählen. Führen Sie die Schritte aus, um Ihre BizTalk-Regel-API-App der Logik-App hinzuzufügen. Anschließend steht eine Option zur Verfügung, mit der die Ziel-Regel-API-App (Aktion) ausgewählt werden kann. Zu den Aktionen gehört die Liste der Richtlinien, die ausgeführt werden sollen. Wählen Sie eine bestimmte Richtlinie aus. Anschließend muss die für die Richtlinie erforderliche Eingabe angegeben werden. Benutzer können die nachfolgende Ausgabe der Regel-API-App für weitere Entscheidungen nutzen.

## Verwenden von Regeln über APIs
Die Regel-API-App kann auch über einen umfangreichen Satz von APIs aufgerufen werden. Auf diese Weise sind Benutzer nicht auf die einfache Verwendung von Logik-Apps beschränkt und können Regeln in einer beliebigen Anwendung durch REST-Aufrufe verwenden. Die genau verfügbaren REST-APIs können durch Klicken auf die Lupe "API-Definition" im Regeldashboard angezeigt werden.

![Alt text][10]

Das folgende Beispiel zeigt, wie diese API in C# verwendet werden könnte.

               // Constructing the JSON message to use in API call to Rules API App

            // xmlInstance is the XML message instance to be passed as input to our Policy
            string xmlInstance = "<ns0:Patient xmlns:ns0="http://tempuri.org/XMLSchema.xsd">  <ns0:Name>Name_0</ns0:Name>  <ns0:Email>Email_0</ns0:Email>  <ns0:PatientID>PatientID_0</ns0:PatientID>  <ns0:Age>10.4</ns0:Age>  <ns0:Claim>    <ns0:ClaimDate>2012-05-31T13:20:00.000-05:00</ns0:ClaimDate>    <ns0:ClaimID>10</ns0:ClaimID>    <ns0:TreatmentID>12</ns0:TreatmentID>    <ns0:ClaimAmount>10000.0</ns0:ClaimAmount>    <ns0:ClaimStatus>ClaimStatus_0</ns0:ClaimStatus>    <ns0:ClaimStatusReason>ClaimStatusReason_0</ns0:ClaimStatusReason>  </ns0:Claim></ns0:Patient>";

            JObject input = new JObject();

            // The JSON object is to be of form {"<XMLSchemName>_<RootNodeName>":"<XML Instance String>".
            // In the below case, we are using XML Schema - "insruanceclaimsschema" and the root node is "Patient".
            // This is CASE SENSITIVE.
            input.Add("insuranceclaimschema_Patient", xmlInstance);
            string stringContent = JsonConvert.SerializeObject(input);


            // Making REST call to Rules API App
            HttpClient httpClient = new HttpClient();

            // The url is the Host URL of the Rules API App
            httpClient.BaseAddress = new Uri("https://rulesservice77492755b7b54c3f9e1df8ba0b065dc6.azurewebsites.net/");
            HttpContent httpContent = new StringContent(stringContent);
            httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json");

            // Invoking API "Execute" on policy "InsruranceClaimPolicy" and getting response JSON object. The url can be gotten from the API Definition Lens
            var postReponse = httpClient.PostAsync("api/Policies/InsuranceClaimPolicy?comp=Execute", httpContent).Result;

Beachten Sie, dass in der oben genannten Regel-API-App die Sicherheitseinstellungen auf "Public Anon" festgelegt wurden. Dies kann innerhalb der API-App über – „Alle Einstellungen“ -> „Anwendungseinstellungen“ -> „Zugriffsebene“ festgelegt werden.

![Alt text][11]

## Bearbeiten von Vokabular und Richtlinie
Einer der Hauptvorteile der Verwendung von Geschäftsregeln ist, dass Änderungen an der Geschäftslogik sehr viel schneller in die Produktion übertragen werden können. Jede Änderung an Vokabular und Richtlinien wird sofort in der Produktionsumgebung angewendet. Der Benutzer muss lediglich zur jeweiligen Vokabulardefinition oder Richtlinie navigieren und eine Änderung vornehmen, damit sie wirksam wird.

<!--Image references-->
[1]: ./media/app-service-logic-use-biztalk-rules/InsuranceScenario.PNG
[2]: ./media/app-service-logic-use-biztalk-rules/InsuranceBusinessLogic.png
[3]: ./media/app-service-logic-use-biztalk-rules/CreateRuleApiApp.png
[4]: ./media/app-service-logic-use-biztalk-rules/RulesDashboard.png
[5]: ./media/app-service-logic-use-biztalk-rules/LiteralVocab.PNG
[6]: ./media/app-service-logic-use-biztalk-rules/XMLVocab.PNG
[7]: ./media/app-service-logic-use-biztalk-rules/BulkAdd.PNG
[8]: ./media/app-service-logic-use-biztalk-rules/PolicyList.PNG
[9]: ./media/app-service-logic-use-biztalk-rules/RuleCreate.PNG
[10]: ./media/app-service-logic-use-biztalk-rules/APIDef.PNG
[11]: ./media/app-service-logic-use-biztalk-rules/PublicAnon.PNG

<!---HONumber=AcomDC_0803_2016-->
