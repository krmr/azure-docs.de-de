---
title: "Übersicht über Service Bus Relay | Microsoft Docs"
description: "Übersicht über Service Bus Relay"
services: service-bus
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1038a2d8-5def-4f48-8703-cb0070fc5f10
ms.service: service-bus
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0482096cbec6a5e4b7b13ea662a180cd9b96e85f


---
# <a name="overview-of-service-bus-relay"></a>Übersicht über Service Bus Relay
Eine wichtige Komponente von Service Bus ist ein zentralisierter *Relaydienst* (mit starkem Lastenausgleich), der Ihnen die Erstellung von Hybridanwendungen ermöglicht, die sowohl in einem Azure-Datencenter als auch in Ihrer eigenen lokalen Unternehmensumgebung ausgeführt werden.  Das Service Bus Relay unterstützt eine Vielzahl verschiedener Transportprotokolle und Webdienststandards. Dies schließt SOAP, WS-* und sogar REST ein. Der Relaydienst unterstützt Ihre Hybridanwendungen, indem er Ihnen die Möglichkeit bietet, WCF-Dienste (Windows Communication Foundation) in einem Unternehmensnetzwerk sicher in der öffentlichen Cloud bereitzustellen, ohne dass eine Firewallverbindung geöffnet werden muss oder tiefgreifende Änderungen an der unternehmensinternen Netzwerkinfrastruktur erforderlich werden. 

![WCF Relay-Konzepte](./media/service-bus-relay-overview/sb-relay-01.png)

Der Relaydienst unterstützt herkömmliches unidirektionales Messaging, Anforderungs-/Antwortmessaging und Peer-to-Peer-Messaging. Er unterstützt außerdem Ereignisverteilung im Internetumfang, um Veröffentlichen-/Abonnieren-Szenarios und bidirektionale Socketkommunikation für erhöhte Punkt-Effizienz zu ermöglichen. 

Im Relaymessaging stellt ein lokaler Dienst eine Verbindung zu dem Relaydienst über einen ausgehenden Port her und erstellt einen bidirektionalen Socket für die mit einer bestimmten Rendezvous-Adresse gekoppelte Kommunikation. Der Client kann dann mit dem lokalen Dienst durch das Senden von Nachrichten an den Relaydienst unter Verwendung der Rendezvousadresse kommunizieren. Der Relaydienst leitet dann Nachrichten an den lokalen Dienst über den bidirektionalen, bereits vorhandenen Socket weiter. Der Client benötigt keine direkte Verbindung zum lokalen Dienst, muss nicht wissen, wo der Dienst angesiedelt ist, und für den lokalen Dienst sind keine geöffneten eingehenden Ports in der Firewall notwendig.

Sie initiieren die Verbindung zwischen dem lokalen Dienst und dem Relaydienst mithilfe einer Sammlung von WCF-"Relay"-Bindungen. Im Prinzip werden die Relaybindungen neuen Transportbindungselementen zugeordnet, die entwickelt wurden, um WCF-Kanalkomponenten zu erstellen, die mit Service Bus in der Cloud integriert werden. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Service Bus Relay finden Sie in den folgenden Themen:

* [Azure Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* [Verwenden des Azure Service Bus WCF Relay-Diensts](service-bus-dotnet-how-to-use-relay.md)




<!--HONumber=Nov16_HO2-->


