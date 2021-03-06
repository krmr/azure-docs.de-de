---
title: Überwachen und Verwalten von Azure Data Factory-Pipelines
description: Erfahren Sie, wie Sie von Ihnen erstellte Azure Data Factorys und Pipelines mithilfe des Azure-Portals und Azure PowerShell überwachen und verwalten.
services: data-factory
documentationcenter: ''
author: spelluru
manager: jhubbard
editor: monicar

ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: spelluru

---
# Überwachen und Verwalten von Azure Data Factory-Pipelines
> [!div class="op_single_selector"]
> * [Verwenden von Azure-Portal/Azure PowerShell](data-factory-monitor-manage-pipelines.md)
> * [Verwenden der App „Überwachung und Verwaltung“](data-factory-monitor-manage-app.md)
> 
> 

Der Data Factory-Dienst bietet eine zuverlässige und umfassende Ansicht der Speicherungs-, Verarbeitungs- und Datenverschiebungsdienste. Der Dienst stellt für Sie ein Überwachungsdashboard bereit, das Sie für folgende Schritte verwenden können:

* Schnelles Bewerten der Integrität einer End-to-End-Datenpipeline
* Erkennen von Problemen und, falls erforderlich, Einleiten von Korrekturmaßnahmen
* Nachverfolgen der Datenherkunft
* Nachverfolgen von quellenübergreifenden Beziehungen zwischen Daten
* Anzeigen des vollständigen Verlaufs von Auftragsausführung, Systemintegrität und Abhängigkeiten

In diesem Artikel wird das Überwachen, Verwalten und Debuggen Ihrer Pipelines beschrieben. Ferner wird erläutert, wie Warnungen erstellt und Benachrichtigungen bei Fehlern eingerichtet werden.

## Grundlegendes zu Pipelines und Aktivitätsstatus
Im Azure-Portal haben Sie folgende Möglichkeiten:

* Anzeigen Ihrer Data Factory als Diagramm
* Anzeigen von Aktivitäten in einer Pipeline
* Anzeigen von Datasets für Eingabe und Ausgabe
* und vieles mehr...

In diesem Abschnitt wird auch erklärt, wie ein Slice von einem Status in einen anderen wechselt.

### Navigieren zu Ihrer Data Factory
1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Alle durchsuchen**, und wählen Sie **Data Factorys** aus.
   
   ![Alle durchsuchen -> Data Factorys](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
   
   Auf dem Blatt **Data Factorys** sollten alle Data Factorys angezeigt sein.
3. Wählen Sie auf dem Blatt "Data Factorys" die Data Factory aus, die Sie interessiert. Anschließend sollte die Startseite (das Blatt **Data Factory**) für die Data Factory angezeigt werden.
   
    ![Blatt "Data Factory"](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### Diagrammansicht Ihrer Data Factory
Die Diagrammansicht einer Data Factory bietet eine zentrale Konsole zum Überwachen und Verwalten der Data Factory und ihrer Ressourcen.

Klicken Sie auf der Data Factory-Startseite auf **Diagramm**, um die Diagrammansicht Ihrer Data Factory anzuzeigen.

![Diagrammansicht](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

Sie können die Optionen „Vergrößern“, „Verkleinern“, „Mit Zoom anpassen“, „Auf 100 % vergrößern“ verwenden und das Layout des Diagramms sperren sowie Pipelines und Tabellen automatisch positionieren. Außerdem können Sie die Informationen zur Datenherkunft anzeigen (vor- und nachgeschaltete Elemente von ausgewählten Elementen).

### Aktivitäten innerhalb einer Pipeline
1. Klicken Sie mit der rechten Maustaste auf die Pipeline, und klicken Sie auf **Pipeline öffnen**, um alle Aktivitäten in der Pipeline sowie Ein- und Ausgabedatasets für die Aktivitäten anzuzeigen. Diese Funktion ist hilfreich, wenn Ihre Pipeline mehr als eine Aktivität umfasst und Sie die operative Herkunft einer einzelnen Pipeline verstehen möchten.
   
    ![Menü "Pipeline öffnen"](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)
2. Im folgenden Beispiel sehen Sie zwei Aktivitäten in der Pipeline mit ihren Ein- und Ausgaben. Die Aktivitäten **JoinData** vom Typ "HDInsight Hive-Aktivität" und **EgressDataAzure** vom Typ "Kopieraktivität" sind in dieser Beispielpipeline enthalten.
   
    ![Aktivitäten innerhalb einer Pipeline](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png)
3. Sie können zurück zur Data Factory-Startseite navigieren, indem Sie in der Brotkrümelnavigation links oben auf den Link „Data Factory“ klicken.
   
    ![Zurück zur Data Factory navigieren](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### Anzeigen des Status jeder Aktivität innerhalb einer Pipeline
Sie können den aktuellen Status einer Aktivität anzeigen, indem Sie sich den Status eines der Datasets ansehen, das von der Aktivität erzeugt wird.

Im folgenden Beispiel wurde die **BlobPartitionHiveActivity** erfolgreich ausgeführt, und es wurde ein Dataset mit dem Namen **PartitionedProductsUsageTable** erzeugt, das den Status **Ready** (Bereit) hat.

![Status der Pipeline](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

Durch Doppelklicken auf **PartitionedProductsUsageTable** in der Diagrammansicht werden alle Slices angezeigt, die innerhalb einer Pipeline von verschiedenen Aktivitätsausführungen erzeugt wurden. Sie sehen, dass die **BlobPartitionHiveActivity** in den letzten acht Monaten jeden Monat erfolgreich ausgeführt wurde und Slices mit dem Status **Ready** erzeugt hat.

Die Datasetslices in Data Factory können einen der folgenden Status haben:

<table>
<tr>
    <th align="left">Zustand</th><th align="left">Unterzustand</th><th align="left">Beschreibung</th>
</tr>
<tr>
    <td rowspan="8">Warten</td><td>ScheduleTime</td><td>Der Zeitpunkt für die Sliceausführung ist noch nicht erreicht.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Die Upstream-Abhängigkeiten sind nicht bereit.</td>
</tr>
<tr>
<td>ComputeResources</td><td>Die Compute-Ressourcen sind nicht verfügbar.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Alle Aktivitätsinstanzen führen andere Slices aus.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Die Aktivität wurde angehalten und kann den Slice erst ausführen, wenn sie fortgesetzt wird.</td>
</tr>
<tr>
<td>Wiederholen</td><td>Es wird versucht, die Ausführung der Aktivität zu wiederholen.</td>
</tr>
<tr>
<td>Überprüfen</td><td>Die Überprüfung wurde noch nicht gestartet.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Es wird auf die Wiederholung der Überprüfung gewartet.</td>
</tr>
<tr>
&lt; tr
<td rowspan="2">In Bearbeitung</td><td>Die Überprüfen erfolgt.</td><td>Die Überprüfung wird ausgeführt.</td>
</tr>
<td></td>
<td>Der Slice wird verarbeitet.</td>
</tr>
<tr>
<td rowspan="4">Fehler</td><td>TimedOut</td><td>Die Ausführung dauerte länger, als für die Aktivität zulässig ist.</td>
</tr>
<tr>
<td>Canceled</td><td>Durch den Benutzer abgebrochen.</td>
</tr>
<tr>
<td>Überprüfen</td><td>Fehler bei der Überprüfung.</td>
</tr>
<tr>
<td></td><td>Der Slice konnte nicht generiert und/oder überprüft werden.</td>
</tr>
<td>Bereit</td><td></td><td>Der Slice ist für die Verwendung bereit.</td>
</tr>
<tr>
<td>Übersprungen</td><td></td><td>Der Slice wird nicht verarbeitet.</td>
</tr>
<tr>
<td>Keine</td><td></td><td>Ein Slice, der zuvor einen anderen Status hatte, aber zurückgesetzt wurde.</td>
</tr>
</table>



Sie können die Details zu einem Slice anzeigen, indem Sie auf dem Blatt **Zuletzt aktualisierte Slices** auf den Eintrag eines Slices klicken.

![Slicedetails](./media/data-factory-monitor-manage-pipelines/slice-details.png)

Wenn der Slice mehrere Male ausgeführt wurde, enthält die Liste **Aktivitätsausführungen** mehrere Zeilen.

![Aktivitätsausführungen für einen Slice](./media/data-factory-monitor-manage-pipelines/activity-runs-for-a-slice.png)

Sie können Details zu einer Aktivitätsausführung anzeigen, indem Sie in der Liste **Aktivitätsausführungen** auf einen Ausführungseintrag klicken. In der Liste werden alle Protokolldateien zusammen mit einer Fehlermeldung, falls vorhanden, angezeigt. Diese Funktion ist nützlich zum Anzeigen und Debuggen von Protokollen, ohne dass Sie Ihre Data Factory verlassen müssen.

![Aktivitätsausführung – Details](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

Wenn der Slice nicht den Status **Bereit** hat, sehen Sie die vorgelagerten Slices, die nicht bereit sind und das Ausführen des aktuellen Slices blockieren, in der Liste **Vorgelagerte Slices, die nicht bereit sind**. Diese Funktion ist nützlich, wenn der Slice den Status **Warten** hat und Sie die vorgelagerten Abhängigkeiten verstehen möchten, auf die der Slice wartet.

![Vorgelagerte Slices nicht bereit](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### Statusdiagramm für Datasets
Sobald Sie eine Data Factory bereitstellen und die Pipelines einen gültigen aktiven Zeitraum haben, gehen die Datasetslices von einem Status in einen anderen über. Derzeit bildet das folgende Statusdiagramm die Status von Slices ab:

![Statusdiagramm](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

In Data Factory gibt es die folgenden Übergänge beim Status von Datasets: Warten -> In Bearbeitung/In Bearbeitung (Überprüfung) -> Bereit/Fehler

Die Slices starten im Status **Warten**, in dem Vorbedingungen für die Ausführung erfüllt werden müssen. Anschließend beginnt die Ausführung der Aktivität, und der Slice wechselt in den Status **In Bearbeitung**. Die Ausführung der Aktivität kann erfolgreich oder nicht erfolgreich sein. Der Slice wird basierend auf dem Ergebnis der Ausführung als **Bereit** oder **Fehler** markiert.

Sie können den Slice vom Status **Bereit** oder **Fehler** in den Status **Warten** zurücksetzen. Außerdem können Sie als Status für den Slice auch **Überspringen** angeben, wodurch die Aktivität nicht ausgeführt und der Slice nicht verarbeitet wird.

## Verwalten von Pipelines
Sie können Ihre Pipelines mit Azure PowerShell verwalten. Sie können z. B. mit Azure PowerShell-Cmdlets Pipelines anhalten und fortsetzen.

### Anhalten und Fortsetzen von Pipelines
Sie können Pipelines mit dem PowerShell-Cmdlet **Suspend-AzureRmDataFactoryPipeline** anhalten. Dieses Cmdlet ist nützlich, wenn Sie Ihre Pipelines nicht ausführen möchten, bis ein Problem behoben wurde.

Im folgenden Screenshot wurde z.B. ein Problem in der **PartitionProductsUsagePipeline** in der Data Factory **productrecgamalbox1dev** festgestellt, weshalb die Pipeline angehalten werden soll.

![Pipeline, die angehalten werden soll](./media/data-factory-monitor-manage-pipelines/pipeline-to-be-suspended.png)

Führen Sie den folgenden PowerShell-Befehl aus, um eine Pipeline anzuhalten.

    Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>

Beispiel:

    Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline 

Nach der Behebung des Problems in der **PartitionProductsUsagePipeline** kann die angehaltene Pipeline mit dem folgenden PowerShell-Befehl fortgesetzt werden.

    Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>

Beispiel:

    Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline 


## Debuggen von Pipelines
Azure Data Factory bietet über das Azure-Portal und Azure PowerShell umfangreiche Funktionen zum Debuggen von Problemen bei Pipelines.

### Suchen von Fehlern in einer Pipeline
Wenn eine Aktivitätsausführung in einer Pipeline nicht erfolgreich ist, hat das von der Pipeline erstellte Dataset aufgrund des Fehlers den Status "Fehler". Sie können Fehler in Azure Data Factory mit den folgenden Verfahren debuggen und beheben.

#### Debuggen eines Fehlers im Azure-Portal:
1. Klicken Sie auf der Startseite der Data Factory auf der Kachel **Datasets** auf **Mit Fehlern**.
   
   ![Kachel "Datasets" mit Fehler](./media/data-factory-monitor-manage-pipelines/datasets-tile-with-errors.png)
2. Klicken Sie auf dem Blatt **Datasets mit Fehlern** auf die gewünschte Tabelle.
   
   ![Blatt "Datasets mit Fehlern"](./media/data-factory-monitor-manage-pipelines/datasets-with-errors-blade.png)
3. Klicken Sie auf dem Blatt **TABELLE** auf den Problemslice, bei dem **STATUS** auf **Fehler** festgelegt ist.
   
   ![Blatt "Tabelle" mit Problemslice](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
4. Klicken Sie auf dem Blatt **DATENSLICE** auf die fehlerhafte Aktivitätsausführung.
   
   ![Datenslice mit einem Fehler](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
5. Auf dem Blatt **AKTIVITÄTSAUSFÜHRUNG – DETAILS** können Sie die mit der Verarbeitung von HDInsight verknüpften Dateien herunterladen. Klicken Sie auf Download für Status/stderr, um die Fehlerprotokolldatei herunterzuladen, die Einzelheiten zu dem Fehler enthält.
   
   ![Blatt "Aktivitätsausführung – Details" mit Fehler](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)

#### Verwenden von PowerShell zum Debuggen eines Fehlers
1. Starten Sie **Azure PowerShell**.
2. Führen Sie den Befehl **Get-AzureRmDataFactorySlice** aus, um die Slices und deren Status anzuzeigen. Ein Slice mit dem Status **Fehler** sollte angezeigt werden.
   
     Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-TableName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
   
   Beispiel:

        Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -TableName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00

    Ersetzen Sie **StartDateTime** durch den für „Set-AzureRmDataFactoryPipelineActivePeriod“ festgelegten StartDateTime-Wert.
1. Führen Sie nun das Cmdlet **Get-AzureRmDataFactoryRun** zum Abrufen von Details zur Aktivitätsausführung für den Slice aus.
   
        Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-
        DataFactoryName] <String> [-TableName] <String> [-StartDateTime] 
        <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
   
    Beispiel:

        Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -TableName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"

    Der Wert von "StartDateTime" ist die Startzeit für den Fehler-/Problemslice, den Sie im vorherigen Schritt notiert haben. Datum und Uhrzeit sollte in doppelte Anführungszeichen eingeschlossen werden.
1. Die Ausgabe mit Einzelheiten zu dem Fehler (ähnlich der folgenden) sollte angezeigt werden:
   
    Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
    ResourceGroupName       : ADF
    DataFactoryName         : LogProcessingFactory3
    TableName               : EnrichedGameEventsTable
    ProcessingStartTime     : 10/10/2014 3:04:52 AM
    ProcessingEndTime       : 10/10/2014 3:06:49 AM
    PercentComplete         : 0
    DataSliceStart          : 5/5/2014 12:00:00 AM
    DataSliceEnd            : 5/6/2014 12:00:00 AM
    Status                  : FailedExecution
    Timestamp               : 10/10/2014 3:04:52 AM
    RetryAttempt            : 0
    Properties              : {}
    ErrorMessage            : Pig script failed with exit code '5'. See wasb://        adfjobs@spestore.blob.core.windows.net/PigQuery
   
                                    Jobs/841b77c9-d56c-48d1-99a3-
                8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                more details.
    ActivityName            : PigEnrichLogs
    PipelineName            : EnrichGameLogsPipeline
    Type                    :
2. Sie können das Cmdlet **Save-AzureRmDataFactoryLog** mit dem ID-Wert ausführen, den Sie in der Ausgabe finden, und die Protokolldateien mithilfe der Option **-DownloadLogs** für das Cmdlet herunterladen.
   
   Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\\Test"

## Wiederholen nach Fehlern in einer Pipeline
### Verwenden des Azure-Portals
Nachdem Sie eine Problembehandlung und ein Debugging für Fehler in einer Pipeline ausgeführt haben, können Sie Fehler wiederholen, indem Sie zum fehlerhaften Slice navigieren und dann auf der Befehlsleiste auf die Schaltfläche **Ausführen** klicken.

![Wiederholen eines fehlerhaften Slices](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

Für den Fall, dass der Slice die Überprüfung aufgrund eines Richtlinienfehlers (z. B. Daten nicht verfügbar) nicht besteht, können Sie den Fehler korrigieren und die Überprüfung erneut ausführen, indem Sie auf der Befehlsleiste auf **Überprüfen** klicken. ![Beheben von Fehlern und Überprüfen](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### Verwenden von Azure PowerShell
Sie können Ausführungen, bei denen Fehler aufgetreten sind, mit dem Cmdlet „Set-AzureRmDataFactorySliceStatus“ wiederholen. Im Thema [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) finden Sie die Syntax und ausführliche Informationen zum Cmdlet.

**Beispiel:** Im folgenden Beispiel wird der Status aller Slices für die Tabelle „DAWikiAggregatedData“ in der Azure Data Factory „WikiADF“ auf „Waiting“ festgelegt.

„UpdateType“ ist auf „UpstreamInPipeline“ festgelegt. Dies bedeutet, dass die Status der einzelnen Slices für die Tabelle und alle abhängigen (vorgeschalteten) Tabellen auf „Waiting“ festgelegt sind. Ein anderer möglicher Wert für diesen Parameter ist „Individual“.

    Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -TableName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00


## Erstellen von Warnungen
Azure protokolliert Benutzerereignisse, wenn eine Azure-Ressource (z.B. eine Data Factory) erstellt, aktualisiert oder gelöscht wird. Sie können Warnungen für diese Ereignisse erstellen. Data Factory erlaubt das Erfassen verschiedener Metriken und Erstellen von Warnungen zu Metriken. Es wird empfohlen, Ereignisse für die Echtzeitüberwachung und Metriken für Nachverfolgungszwecke zu verwenden.

### Warnungen zu Ereignissen
Azure-Ereignisse bieten hilfreiche Einblicke in die Aktivitäten in Ihren Azure-Ressourcen. Azure protokolliert Benutzerereignisse, wenn eine Azure-Ressource (z.B. eine Data Factory) erstellt, aktualisiert oder gelöscht wird. Bei Verwendung von Azure Data Factory werden Ereignisse in folgenden Fällen generiert:

* Azure Data Factory wird erstellt/aktualisiert/gelöscht.
* Die Datenverarbeitung (als Ausführungen bezeichnet) wurde gestartet/abgeschlossen.
* Ein bedarfsbezogener HDInsight-Cluster wird erstellt und entfernt.

Sie können Warnungen zu diesen Benutzerereignissen erstellen und sie so konfigurieren, dass E-Mail-Benachrichtigungen an den Administrator und an Co-Administratoren des Abonnements gesendet werden. Darüber hinaus können Sie zusätzliche E-Mail-Adressen von Benutzern angeben, die E-Mail-Benachrichtigungen erhalten sollen, wenn bestimmte Bedingungen erfüllt werden. Diese Funktion ist nützlich, wenn Sie sich bei Fehlern benachrichtigen lassen und Ihre Data Factory nicht kontinuierlich überwachen möchten.

> [!NOTE]
> Das Portal zeigt derzeit keine Warnungen für Ereignisse an. Verwenden Sie die [App „Überwachung und Verwaltung“](data-factory-monitor-manage-app.md), um alle Warnungen anzuzeigen.
> 
> 

#### Angeben einer Warnungsdefinition:
Um eine Warnungsdefinition anzugeben, erstellen Sie eine JSON-Datei mit einer Beschreibung der Vorgänge, zu denen Sie benachrichtigt werden möchten. Im folgenden Beispiel sendet die Warnung eine E-Mail-Benachrichtigung für den RunFinished-Vorgang. Genauer gesagt: Es wird eine E-Mail-Benachrichtigung gesendet, wenn eine Ausführung in der Data Factory abgeschlossen wurde und ein Fehler aufgetreten ist (Status = FailedExecution).

    {
        "contentVersion": "1.0.0.0",
         "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "parameters": {},
        "resources": 
        [
            {
                "name": "ADFAlertsSlice",
                "type": "microsoft.insights/alertrules",
                "apiVersion": "2014-04-01",
                "location": "East US",
                "properties": 
                {
                    "name": "ADFAlertsSlice",
                    "description": "One or more of the data slices for the Azure Data Factory has failed processing.",
                    "isEnabled": true,
                    "condition": 
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.ManagementEventRuleCondition",
                        "dataSource": 
                        {
                            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleManagementEventDataSource",
                            "operationName": "RunFinished",
                            "status": "Failed",
                            "subStatus": "FailedExecution"   
                        }
                    },
                    "action": 
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "customEmails": [ "<your alias>@contoso.com" ]
                    }
                }
            }
        ]
    }

Aus der JSON-Definition kann **subStatus** entfernt werden, wenn Sie zu einem bestimmten Fehler nicht informiert werden möchten.

In diesem Beispiel wird die Warnung für alle Data Factorys in Ihrem Abonnement eingerichtet. Wenn die Warnung für eine bestimmte Data Factory eingerichtet werden soll, können Sie das **resourceUri**-Element der Data Factory im **dataSource**-Block angeben:

    "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"

Die folgende Tabelle enthält eine Liste der verfügbaren Vorgänge und Status (samt Unterstatus).

| Vorgangsname | Status | Unterstatus |
| --- | --- | --- |
| RunStarted |Gestartet |Starting |
| RunFinished |Failed / Succeeded |FailedResourceAllocation<br/><br/>Succeeded<br/><br/>FailedExecution<br/><br/>TimedOut<br/><br/><Canceled<br/><br/>FailedValidation<br/><br/>Abandoned |
| OnDemandClusterCreateStarted |Gestartet | |
| OnDemandClusterCreateSuccessful |Erfolgreich | |
| OnDemandClusterDeleted |Erfolgreich | |

Unter [Benachrichtigungsregel erstellen](https://msdn.microsoft.com/library/azure/dn510366.aspx) finden Sie ausführliche Informationen zu JSON-Elementen, die im Beispiel verwendet werden.

#### Bereitstellen der Warnung
Verwenden Sie zum Bereitstellen der Warnung das Azure PowerShell-Cmdlet **New-AzureRmResourceGroupDeployment**, wie im folgenden Beispiel gezeigt:

    New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  

Nachdem die Ressourcengruppenbereitstellung erfolgreich abgeschlossen wurde, werden die folgenden Meldungen angezeigt:

    VERBOSE: 7:00:48 PM - Template is valid.
    WARNING: 7:00:48 PM - The StorageAccountName parameter is no longer used and will be removed in a future release.
    Please update scripts to remove this parameter.
    VERBOSE: 7:00:49 PM - Create template deployment 'ADFAlertFailedSlice'.
    VERBOSE: 7:00:57 PM - Resource microsoft.insights/alertrules 'ADFAlertsSlice' provisioning status is succeeded

    DeploymentName    : ADFAlertFailedSlice
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 10/11/2014 2:01:00 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           :

> [!NOTE]
> Sie können die REST-API [Warnungsregel erstellen](https://msdn.microsoft.com/library/azure/dn510366.aspx) verwenden, um eine Regel für Warnungen zu erstellen. Die JSON-Nutzlast ähnelt dem JSON-Beispiel.
> 
> 

#### Abrufen der Liste von Azure-Ressourcengruppenbereitstellungen
Um die Liste mit den bereitgestellten Azure-Ressourcengruppen abzurufen, verwenden Sie das Cmdlet **Get-AzureRmResourceGroupDeployment** wie im folgenden Beispiel gezeigt:

    Get-AzureRmResourceGroupDeployment -ResourceGroupName adf

    DeploymentName    : ADFAlertFailedSlice
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 10/11/2014 2:01:00 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           :


#### Problembehandlung bei Benutzerereignissen
* Sie können alle erzeugten Ereignisse anzeigen, indem Sie auf die Kachel **Vorgänge** klicken. Für alle auf dem Blatt **Ereignisse** angezeigten Vorgänge können Warnungen eingerichtet werden:
  
    ![Vorgänge](./media/data-factory-monitor-manage-pipelines/operations.png)
* Der Artikel [Azure Insight Cmdlets](https://msdn.microsoft.com/library/mt282452.aspx) (in englischer Sprache) erörtert PowerShell-Cmdlets, mit deren Hilfe sich Warnungen hinzufügen/empfangen/entfernen lassen. Hier sind einige Beispiele für die Verwendung des Cmdlets **Get-AlertRule**:

        PS C:\> get-alertrule -res $resourceGroup -n ADFAlertsSlice -det

                Properties :
                Action      : Microsoft.Azure.Management.Insights.Models.RuleEmailAction
                Condition   :
                DataSource :
                EventName             :
                Category              :
                Level                 :
                OperationName         : RunFinished
                ResourceGroupName     :
                ResourceProviderName  :
                ResourceId            :
                Status                : Failed
                SubStatus             : FailedExecution
                Claims                : Microsoft.Azure.Management.Insights.Models.RuleManagementEventClaimsDataSource
                Condition      :
                Description : One or more of the data slices for the Azure Data Factory has failed processing.
                Status      : Enabled
                Name:       : ADFAlertsSlice
                Tags       :
                $type          : Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage
                Id: /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/ADFAlertsSlice
                Location   : West US
                Name       : ADFAlertsSlice

        PS C:\> Get-AlertRule -res $resourceGroup

                Properties : Microsoft.Azure.Management.Insights.Models.Rule
                Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
                Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
                Location   : West US
                Name       : FailedExecutionRunsWest0

                Properties : Microsoft.Azure.Management.Insights.Models.Rule
                Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
                Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest3
                Location   : West US
                Name       : FailedExecutionRunsWest3

        PS C:\> Get-AlertRule -res $resourceGroup -Name FailedExecutionRunsWest0

                Properties : Microsoft.Azure.Management.Insights.Models.Rule
                Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
                Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
                Location   : West US
                Name       : FailedExecutionRunsWest0

    Führen Sie die folgenden get-Help-Befehle aus, wenn Ihnen das Cmdlet „Get-AlertRule“ im Detail und anhand von Beispiele vorgeführt werden soll.

        get-help Get-AlertRule -detailed 
        get-help Get-AlertRule -examples


* Wenn Sie die Ereignisse für die Warnungserzeugung auf dem Blatt "Portal" sehen, aber keine E-Mail-Benachrichtigungen erhalten, prüfen Sie, ob die angegebene E-Mail-Adresse für den Empfang von E-Mails externer Absender eingerichtet ist. Möglicherweise wurden die Warnungs-E-Mails durch Ihre E-Mail-Einstellungen blockiert.

### Warnungen zu Metriken
Data Factory erlaubt das Erfassen verschiedener Metriken und Erstellen von Warnungen zu Metriken. Sie können Warnungen für die folgenden Metriken für die Slices in Ihrer Data Factory erstellen und überwachen.

* Fehlerhafte Ausführungen
* Erfolgreiche Ausführungen

Diese Metriken sind nützlich und ermöglichen es Ihnen, sich einen Überblick über die gesamten fehlerhaften und erfolgreichen Ausführungen in ihrer Data Factory zu verschaffen. Metriken werden bei jeder Ausführung eines Slices ausgegeben. Zu jeder vollen Stunde werden diese Metriken aggregiert und in Ihr Speicherkonto übertragen. Richten Sie daher ein Speicherkonto ein, um Metriken zu aktivieren.

#### Aktivieren von Metriken:
Klicken Sie auf dem Blatt „Data Factory“ auf Folgendes, um Metriken zu aktivieren:

**Überwachung** -> **Metrik** -> **Diagnoseeinstellungen** -> **Diagnose**

Klicken Sie auf dem Blatt **Diagnose** auf **Ein**, wählen Sie das Speicherkonto aus, und speichern Sie.

![Aktivieren von Metriken](./media/data-factory-monitor-manage-pipelines/enable-metrics.png)

Nach dem Speichern kann es bis zu eine Stunde dauern, bis die Metriken auf dem Blatt „Überwachung“ angezeigt werden, da die Aggregation von Metriken stündlich erfolgt.

### Einrichten von Warnungen zu Metriken:
Zum Einrichten von Warnungen zu Metriken klicken Sie auf dem Blatt „Data Factory“ auf Folgendes: **Überwachung** > **Metrik** > **Warnung hinzufügen** > **Warnungsregel hinzufügen**.

Geben Sie die Details für die Warnungsregel ein, geben Sie E-Mail-Adressen an, und klicken Sie auf **OK**.

![Einrichten von Warnungen zu Metriken](./media/data-factory-monitor-manage-pipelines/setting-up-alerts-on-metrics.png)

Anschließend sollten Sie eine neue Warnregel sehen, die auf der Kachel "Warnungsregeln" aktiviert ist:

![Warnungsregeln aktiviert](./media/data-factory-monitor-manage-pipelines/alert-rule-enabled.png)

Glückwunsch! Sie haben Ihre erste Warnung zu Metriken eingerichtet. Jetzt sollten Sie Benachrichtigungen jedes Mal erhalten, wenn eine Warnungsregel im angegebenen Zeitfenster erfüllt wird.

### Warnbenachrichtigungen:
Sobald die Warnungsregel der Bedingung entspricht, sollten Sie eine durch eine Warnung ausgelöste E-Mail erhalten. Sobald das Problem behoben wurde und die Warnbedingung nicht mehr zutrifft, erhalten Sie eine E-Mail mit einem Hinweis zur Behebung der Warnung.

Dieses Verhalten ist anders als bei Ereignissen, bei denen eine Benachrichtigung bei jedem Fehler gesendet wird, für den die Warnungsregel zutrifft.

### Bereitstellen von Warnungen mithilfe von PowerShell
Sie können Warnungen zu Metriken auf die gleiche Weise wie zu Ereignissen bereitstellen.

**Warnungsdefinition:**

    {
        "contentVersion" : "1.0.0.0",
        "$schema" : "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "parameters" : {},
        "resources" : [
        {
                "name" : "FailedRunsGreaterThan5",
                "type" : "microsoft.insights/alertrules",
                "apiVersion" : "2014-04-01",
                "location" : "East US",
                "properties" : {
                    "name" : "FailedRunsGreaterThan5",
                    "description" : "Failed Runs greater than 5",
                    "isEnabled" : true,
                    "condition" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                        "dataSource" : {
                            "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
                            "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                            "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName
    >/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>",
                            "metricName" : "FailedRuns"
                        },
                        "threshold" : 5.0,
                        "windowSize" : "PT3H",
                        "timeAggregation" : "Total"
                    },
                    "action" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "customEmails" : ["abhinav.gpt@live.com"]
                    }
                }
            }
        ]
    }

Ersetzen Sie „subscriptionId“, „resourceGroupName“ und „dataFactoryName“ im obigen Beispiel durch die entsprechenden Werte.

*metricName* unterstützt ab jetzt zwei Werte:

* FailedRuns
* SuccessfulRuns

**Bereitstellen der Warnung:**

Verwenden Sie zum Bereitstellen der Warnung das Azure PowerShell-Cmdlet **New-AzureRmResourceGroupDeployment**, wie im folgenden Beispiel gezeigt:

    New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json

Folgende Meldung sollte nach erfolgreicher Bereitstellung angezeigt werden:

    VERBOSE: 12:52:47 PM - Template is valid.
    VERBOSE: 12:52:48 PM - Create template deployment 'FailedRunsGreaterThan5'.
    VERBOSE: 12:52:55 PM - Resource microsoft.insights/alertrules 'FailedRunsGreaterThan5' provisioning status is succeeded


    DeploymentName    : FailedRunsGreaterThan5
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 7/27/2015 7:52:56 PM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           


Zum Bereitstellen einer Warnungsregel können Sie auch das Cmdlet **Add-AlertRule** verwenden. Ausführliche Informationen und Beispiele finden Sie im Thema [Add-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx).

## Verschieben von Data Factory in eine andere Ressourcengruppe oder ein anderes Abonnement
Sie können eine Data Factory mithilfe der Schaltfläche **Verschieben** in der Befehlsleiste auf der Homepage Ihrer Data Factory in eine andere Ressourcengruppe oder ein anderes Abonnement verschieben.

![Data Factory verschieben](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

Sie können auch alle zugehörigen Ressourcen (z. B. mit der Data Factory verknüpfte Warnungen) zusammen mit der Data Factory verschieben.

![Dialogfeld „Ressourcen verschieben“](./media/data-factory-monitor-manage-pipelines/MoveResources.png)

<!---HONumber=AcomDC_0907_2016-->