---
title: Deaktivieren oder Aktivieren eines Traffic Manager-Endpunkts | Microsoft Docs
description: Dieser Artikel hilft Ihnen, die Endpunkte Ihres Traffic Manager-Profils zu aktivieren oder zu deaktivieren.
services: traffic-manager
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: tysonn

ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/10/2016
ms.author: sewhee

---
# Deaktivieren oder Aktivieren eines Traffic Manager-Endpunkts
Sie können auch einzelne Endpunkte deaktivieren, die Teil eines Traffic Manager-Profils sind. Endpunkte umfassen Cloud-Dienste und Websites. Bei Deaktivieren eines Endpunkts bleibt er Teil des Profils, doch das Profil verhält sich so, als wäre der Endpunkt nicht vorhanden. Diese Aktion ist äußerst nützlich zum zeitweiligen Entfernen eines Endpunkts, der sich im Wartungsmodus befindet oder erneut bereitgestellt werden soll. Sobald der Endpunkt wieder betriebsbereit ist, kann er aktiviert werden.

> [!NOTE]
> **Das Deaktivieren eines Endpunkts hat nichts mit dessen Bereitstellungsstatus in Azure zu tun. Ein fehlerfrei funktionierender Endpunkt bleibt in Betrieb und kann Datenverkehr empfangen, selbst wenn er in Traffic Manager deaktiviert ist. Das Deaktivieren eines Endpunkts in einem Profil wirkt sich darüber hinaus nicht auf seinen Status in einem anderen Profil aus.**
> 
> 

## So deaktivieren Sie einen Endpunkt
1. Suchen Sie im klassischen Azure-Portal im Bereich „Traffic Manager“ das Traffic Manager-Profil, das die Endpunkteinstellungen enthält, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben dem Profilnamen. Die Einstellungsseite für das Profil wird geöffnet.
2. Klicken Sie oben auf der Seite auf **Endpunkte**, um die Endpunkte anzuzeigen, die in der Konfiguration enthalten sind.
3. Klicken Sie auf den Endpunkt, den Sie deaktivieren möchten, und dann am unteren Rand der Seite auf **Deaktivieren**.
4. Datenverkehr wird basierend auf der DNS-Gültigkeitsdauer, die für den Traffic Manager-Domänennamen konfiguriert ist, nicht mehr zum Endpunkt geleitet. Sie können die Gültigkeitsdauer auf der Seite "Konfiguration" des Traffic Manager-Profils ändern.

## So aktivieren Sie einen Endpunkt
1. Suchen Sie im klassischen Azure-Portal im Bereich „Traffic Manager“ das Traffic Manager-Profil, das die Endpunkteinstellungen enthält, die Sie ändern möchten, und klicken Sie dann auf den Pfeil rechts neben dem Profilnamen. Die Einstellungsseite für das Profil wird geöffnet.
2. Klicken Sie oben auf der Seite auf **Endpunkte**, um die Endpunkte anzuzeigen, die in der Konfiguration enthalten sind.
3. Klicken Sie auf den Endpunkt, den Sie aktivieren möchten, und dann am unteren Rand der Seite auf **Aktivieren**.
4. Datenverkehr wird wieder entsprechend dem Profil zum Dienst geleitet.

## Nächste Schritte
[Deaktivieren, Aktivieren oder Löschen eines Traffic Manager-Profils](disable-enable-or-delete-a-profile.md)

[Problembehandlung beim Status „Heruntergestuft“ in Traffic Manager](traffic-manager-troubleshooting-degraded.md)

[Leistungsüberlegungen zu Traffic Manager](traffic-manager-performance-considerations.md)

<!---HONumber=AcomDC_0824_2016-->