---
title: Was passiert, wenn Sie Mobile Services mithilfe von Visual Studio Verbundene Dienste zu einer Javascript-App hinzufügen? | Microsoft Docs
description: Beschreibt, was mit Ihrem Azure Mobile Services-Projekt in Visual Studio passiert ist.
services: mobile-services
documentationcenter: ''
author: mlhoop
manager: douge
editor: ''

ms.service: mobile-services
ms.workload: mobile
ms.tgt_pltfrm: NA
ms.devlang: JavaScript
ms.topic: article
ms.date: 07/21/2016
ms.author: mlearned

---
# Was passiert in meinem Javascript-Projekt, wenn ich Azure Mobile Services über verbundene Visual Studio-Dienste hinzugefügt habe?
## NuGet-Paket hinzugefügt
Das NuGet-Paket **WindowsAzure.MobileServices.WinJS** wurde installiert, einschließlich der Azure Mobile Service-Bibliothek in der Datei `js\MobileServices.js`.

## Werte für die Verbindungszeichenfolge für Mobile Services
Im Ordner `services\mobileServices\settings` wurde eine neue JavaScript-Datei (JS-Datei) mit einem **MobileServiceClient** generiert. Sie enthält die Anwendungs-URL des ausgewählten mobilen Diensts sowie seinen Anwendungsschlüssel.

## Verweise zu "default.html" hinzugefügt
Verweise auf `MobileServices.js` und die Einstellungsdatei wurden zu `default.html` hinzugefügt.

## Dateien für verbundene Dienste hinzugefügt
Im Ordner "Dienste" wurden Konfigurationsdateien für verbundene Dienste hinzugefügt.

<!---HONumber=AcomDC_0727_2016-->