---
title: Vergleich zwischen von Azure IoT Hub und Event Hubs | Microsoft Docs
description: Hier finden Sie einen Vergleich der Azure IoT Hub- und Azure Event Hubs-Dienste, wobei besonders auf Unterschiede bei den Funktionen und Anwendungsbeispiele eingegangen wird.
services: iot-hub
documentationcenter: ''
author: fsautomata
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2016
ms.author: elioda

---
# <a name="comparison-of-iot-hub-and-event-hubs"></a>Vergleich von IoT Hub und Event Hubs
Eine der wichtigsten Anwendungsmöglichkeiten für Azure IoT Hub ist die Erfassung der Telemetriedaten von Geräten. Aus diesem Grund wird IoT Hub häufig mit [Azure Event Hubs][Azure Event Hubs]verglichen. Ebenso wie IoT Hub ist Event Hubs ein Dienst zur Ereignisverarbeitung, der riesige Mengen von Ereignis- und Telemetriedaten erfassen kann und gleichzeitig eine niedrige Latenz und hohe Zuverlässigkeit bietet.

Allerdings bestehen bei den Diensten viele Unterschiede, die in der folgenden Tabelle beschrieben werden.

| Bereich | IoT Hub | Event Hubs |
| --- | --- | --- |
| Kommunikationsmuster |Ermöglicht D2C- und C2D-Messaging. |Ermöglicht nur Ereigniseingang (meist für D2C-Szenarien). |
| Unterstützung für Geräteprotokolle |Unterstützt MQTT, AMQP, AMQP über WebSockets und HTTP/1. Darüber hinaus arbeitet IoT Hub mit dem [Azure IoT-Protokollgateway][lnk-azure-protocol-gateway] zusammen, einer anpassbaren Protokollgateway-Implementierung, die benutzerdefinierte Protokolle unterstützt. |Unterstützt AMQP, AMQP über WebSockets und HTTP/1. |
| Sicherheit |Ermöglicht Identität pro Gerät und widerrufbare Zugriffssteuerung. Siehe [Abschnitt über Sicherheit im IoT Hub-Entwicklerleitfaden]. |Ermöglicht Event Hubs-weite [SAS-Richtlinien][Event Hub – Sicherheit] mit begrenzter Widerrufsunterstützung über [Richtlinien des Herausgebers][Event Hub-Herausgeberrichtlinien]. Bei IoT-Lösungen ist es häufig erforderlich, eine benutzerdefinierte Lösung zur Unterstützung von Anmeldeinformationen pro Gerät und Maßnahmen zum Schutz vor Spoofing zu implementieren. |
| Vorgangsüberwachung |Ermöglicht IoT-Lösungen, einen umfangreichen Satz von Geräte-Identitätsverwaltungs- und -verbindungsereignissen zu abonnieren, z. B. individuelle Authentifizierungsfehler, Drosselung und Ausnahmen bei ungültigen Formaten. Diese Ereignisse ermöglichen Ihnen das rasche Bestimmen von Verbindungsproblemen auf Einzelgerätebene. |Macht nur aggregierte Metriken verfügbar. |
| Skalieren |Ist für die Unterstützung von Millionen von gleichzeitig verbundenen Geräten optimiert. |Kann eine eingeschränktere Anzahl von gleichzeitigen Verbindungen unterstützen – bis zu 5.000 AMQP-Verbindungen gemäß [Azure Service Bus-Kontingenten][]. Jedoch können Sie mit Event Hubs die Partition für jede gesendete Nachricht angeben. |
| Geräte-SDKs |Stellt [Geräte-SDKs][Azure IoT Hub-SDKs] für viele verschiedene Plattformen und Sprachen bereit. |Wird unter .NET und C unterstützt und verfügt außerdem über AMQP- und HTTP-Sendeschnittstellen. |
| Dateiupload |Ermöglicht IoT-Lösungen zum Hochladen von Dateien von Geräten in die Cloud. Enthält einen Dateibenachrichtigungs-Endpunkt für die Workflowintegration und eine Vorgangsüberwachungskategorie für die Debugunterstützung. |Verwendet ein Anspruchsprüfungsmuster zur manuellen Anforderung von Dateien bei Geräten, und um Geräten einen Speicherschlüssel für die Transaktion bereitzustellen. |

Zusammengefasst bedeutet dies: Auch wenn nur der D2C-Telemetrieeingang als Anwendungsfall vorhanden ist, bietet IoT Hub einen Dienst, der speziell für die Verbindung von IoT-Geräten ausgelegt ist. Außerdem werden die Wertbeiträge für diese Szenarien mit IoT-spezifischen Features weiter ausgebaut. Event Hubs sind für den Ereigniseingang in großem Umfang ausgelegt, sowohl im Kontext von Szenarien im Rechenzentrum als auch außerhalb davon.

Es ist nicht ungewöhnlich, dass IoT Hub und Event Hubs in derselben Lösung verwendet werden. Dabei wird IoT Hub für die D2C-Kommunikation eingesetzt und mit Event Hubs der nachgelagerte Ereigniseingang in Echtzeitverarbeitungsmodule geregelt.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Planen Ihrer IoT Hub-Bereitstellung finden Sie unter [Skalierung, hohe Verfügbarkeit und Notfallwiederherstellung][lnk-scaling].

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Entwicklerhandbuch][lnk-devguide]
* [Simulieren eines Geräts mit dem Gateway SDK][lnk-gateway]

[Azure Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[Abschnitt über Sicherheit im IoT Hub-Entwicklerhandbuch]: iot-hub-devguide-security.md
[Event Hub – Sicherheit]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hub-Herausgeberrichtlinien]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks
[Azure Service Bus-Kontingente]: ../service-bus-messaging/service-bus-quotas.md
[Azure IoT Hub-SDKs]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md



<!--HONumber=Oct16_HO2-->


