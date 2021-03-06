---
title: Verwenden von Azure Backup als Ersatz für Ihre Bandinfrastruktur | Microsoft Docs
description: Hier erfahren Sie, wie Azure Backup eine Semantik ähnlich wie bei Bändern bereitstellt, damit Sie Ihre Daten in Azure sichern und wiederherstellen können.
services: backup
documentationcenter: ''
author: trinadhk
manager: vijayts
editor: ''

ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/27/2016
ms.author: jimpark;trinadhk;markgal

---
# Verwenden von Azure Backup als Ersatz für Ihre Bandinfrastruktur
Kunden von Azure Backup und System Center Data Protection Manager haben folgende Möglichkeiten:

* Sichern von Daten gemäß Zeitplänen, die den Anforderungen ihrer Organisation am besten entsprechen
* Beibehalten der Sicherungsdaten für längere Zeiträume
* Integrieren von Azure in ihre Anforderungen an die langfristige Aufbewahrung (anstelle von Bändern)

In diesem Artikel wird erläutert, wie Kunden Sicherungs- und Aufbewahrungsrichtlinien aktivieren können. Kunden, die für ihre Anforderungen an eine langfristige Aufbewahrung Bänder verwenden, steht jetzt dank dieses Features eine leistungsstarke und geeignete Alternative zur Verfügung. Das Feature ist in der neuesten Version von Azure Backup aktiviert (die [hier](http://aka.ms/azurebackup_agent) erhältlich ist). System Center DPM-Kunden müssen mindestens auf DPM 2012 R2 UR5 aktualisieren, bevor sie DPM mit dem Azure Backup-Dienst verwenden können.

## Was ist ein Sicherungszeitplan?
Der Sicherungszeitplan gibt die Häufigkeit des Sicherungsvorgangs an. Die Einstellungen auf dem folgenden Bildschirm geben beispielsweise an, dass Sicherungen täglich um 18:00 Uhr und um Mitternacht ausgeführt werden.

![Täglicher Zeitplan](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

Kunden können auch eine wöchentliche Sicherung planen. Die Einstellungen auf dem folgenden Bildschirm geben beispielsweise an, dass Sicherungen jeden zweiten Sonntag und Mittwoch um 9:30 Uhr und um 1 Uhr morgens erstellt werden.

![Wöchentlicher Zeitplan](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## Worum handelt es sich bei der Aufbewahrungsrichtlinie?
Die Aufbewahrungsrichtlinie gibt an, wie lange die Sicherung gespeichert werden muss. Anstatt nur eine "flache Richtlinie" für alle Sicherungspunkte anzugeben, können Kunden je nach Sicherungszeitpunkt verschiedene Aufbewahrungsrichtlinien festlegen. Zum Beispiel kann der täglich erstellte Sicherungspunkt, der als Wiederherstellungspunkt für den alltäglichen Betrieb dient, 90 Tage lang aufbewahrt werden. Der Sicherungspunkt, der zu Überwachungszwecken am Ende jedes Quartals erstellt wird, kann wesentlich länger aufbewahrt werden.

![Aufbewahrungsrichtlinie](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

Die Gesamtanzahl der in dieser Richtlinie angegebenen "Aufbewahrungspunkte" ist 90 (tägliche Punkte) + 40 (einer pro Quartal für 10 Jahre) = 130.

## Beispiel – Kombination aus beidem
![Beispielbildschirm](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. **Tägliche Aufbewahrungsrichtlinie**: Täglich erstellte Sicherungen werden sieben Tage lang gespeichert.
2. **Wöchentliche Aufbewahrungsrichtlinie**: Sicherungen, die jeden Tag um Mitternacht und jeden Samstag um 18 Uhr erstellt werden, werden vier Wochen lang aufbewahrt.
3. **Monatliche Aufbewahrungsrichtlinie**: Sicherungen, die am letzten Samstag im Monat um Mitternacht und um 18:00 Uhr erstellt werden, werden zwölf Monate lang aufbewahrt.
4. **Jährliche Aufbewahrungsrichtlinie**: Sicherungen, die jedes Jahr am letzten Samstag im März um Mitternacht erstellt werden, werden zehn Jahre lang aufbewahrt.

Die Gesamtanzahl der „Aufbewahrungspunkte“ (Punkte, von denen ein Kunde Daten wiederherstellen kann) in der obigen Abbildung wird wie folgt berechnet:

* Zwei Punkte pro Tag, sieben Tage lang = 14 Wiederherstellungspunkte
* Zwei Punkte pro Woche, vier Wochen lang = 8 Wiederherstellungspunkte
* Zwei Punkte pro Monat, 12 Monate lang = 24 Wiederherstellungspunkte
* Ein Punkt pro Jahr, 10 Jahre lang = 10 Wiederherstellungspunkte

Die Gesamtzahl der Wiederherstellungspunkte beträgt 56.

> [!NOTE]
> In Azure Backup gibt es keine Beschränkung der Anzahl der Wiederherstellungspunkte.
> 
> 

## Erweiterte Konfiguration
Durch Klicken auf **Ändern** im Bildschirm oben können Kunden noch flexiblere Aufbewahrungszeitpläne angeben.

![Ändern](./media/backup-azure-backup-cloud-as-tape/modify.png)

## Nächste Schritte
Weitere Informationen zu Azure Backup finden Sie hier:

* [Einführung in Azure Backup](backup-introduction-to-azure-backup.md)
* [Azure Backup testen](backup-try-azure-backup-in-10-mins.md)

<!---HONumber=AcomDC_0928_2016-->