---
title: Analytics, das leistungsfähige Suchtool von Application Insights | Microsoft Docs
description: 'Übersicht über Analytics, das leistungsfähige Diagnosesuchtool von Application Insights. '
services: application-insights
documentationcenter: ''
author: alancameronwills
manager: douge

ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/25/2016
ms.author: awills

---
# Analytics in Application Insights
[Analytics](app-insights-analytics.md) ist die leistungsfähige Suchfunktion von [Application Insights](app-insights-overview.md). Auf diesen Seiten wird die Analytics-Abfragesprache beschrieben.

* **[Sehen Sie sich das Einführungsvideo an](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Testen Sie Analytics mit unseren simulierten Daten](https://analytics.applicationinsights.io/demo)**, wenn Ihre App noch keine Daten an Application Insights sendet.

## Abfragen in Analytics
Eine typische Abfrage ist eine *Quelltabelle*, gefolgt von einer Reihe von *Operatoren*, die durch `|` getrennt sind.

Finden wir also heraus, zu welcher Tageszeit die Einwohner von Hyderabad unsere Web-App testen. Und während wir damit beschäftigt sind, sehen wir, welche Ergebniscodes an ihre HTTP-Anfragen zurückgegeben werden.

```AIQL

    requests      // Table of events that log HTTP requests.
    | where timestamp > ago(7d) and client_City == "Hyderabad"
    | summarize clients = dcount(client_IP) 
      by tod_UTC=bin(timestamp % 1d, 1h), resultCode
    | extend local_hour = (tod_UTC + 5h + 30min) % 24h + datetime("2001-01-01") 
```

Wir zählen verschiedene Client-IP-Adressen der letzten 7 Tage auf und gruppieren sie nach Uhrzeit.

Wir zeigen die Ergebnisse mit der Balkendiagramm-Präsentation und listen die Ergebnisse von verschiedenen Antwortcodes auf.

![Wählen Sie das Balkendiagramm, die X-Achsen und Y-Achsen, dann die Segmentierung](./media/app-insights-analytics/020.png)

Anscheinend ist unserer App zur Mittagszeit und zur Schlafenszeit in Hyderabad am beliebtesten. (Und wir sollten wir diese 500 Codes untersuchen.)

Es gibt auch leistungsstarke statistische Vorgänge:

![](./media/app-insights-analytics/025.png)

Die Sprache verfügt über viele attraktive Features:

* [Filtern](app-insights-analytics-reference.md#where-operator) der Rohdaten Ihrer App-Telemetrie nach beliebigen Feldern, einschließlich Ihrer benutzerdefinierten Eigenschaften und Metriken.
* [Verbinden](app-insights-analytics-reference.md#join-operator) mehrerer Tabellen – Korrelation von Anforderungen mit Seitenansichten, Aufrufen von Abhängigkeiten, Ausnahmen und Protokollablaufverfolgungen.
* Leistungsstarke statistische [Aggregationen](app-insights-analytics-reference.md#aggregations).
* Genauso leistungsstark wie SQL, aber viel einfacher für komplexe Abfragen: anstelle der Schachtelung von Anweisungen übergeben Sie die Daten aus einem elementaren Vorgang an den nächsten.
* Sofortige und leistungsfähige Visualisierungen.

## Verbinden mit Ihren Application Insights-Daten
Öffnen Sie Analytics über das [Blatt „Übersicht“](app-insights-dashboards.md) Ihrer App in Application Insights:

![Öffnen Sie unter „portal.azure.com“ die Application Insights-Ressource, und wählen Sie „Analytics“.](./media/app-insights-analytics/001.png)

## Grenzen
Derzeit sind Abfrageergebnisse auf eine Woche alte Daten beschränkt.

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

## Nächste Schritte
* Es wird empfohlen, mit der [Einführung in die Abfragesprache](app-insights-analytics-tour.md) zu beginnen.

<!---HONumber=AcomDC_0831_2016-->