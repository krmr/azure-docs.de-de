---
title: Installieren von MySQL auf einer OpenSUSE-VM | Microsoft Docs
description: Erfahren Sie, wie Sie MySQL auf einem virtuellen OpenSUSE Linux-Computer in Azure installieren.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: timlt
editor: ''
tags: azure-service-management

ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/19/2016
ms.author: cynthn

---
# Installieren von MySQL auf einem virtuellen Computer mit OpenSUSE Linux in Azure
[MySQL][MySQL] ist eine beliebte Open-Source-SQL-Datenbank. In diesem Tutorial erfahren Sie, wie Sie einen virtuellen Computer mit OpenSUSE Linux erstellen und anschließend MySQL installieren.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

<br>

## Erstellen eines virtuellen Computers mit OpenSUSE Linux
[!INCLUDE [create-and-configure-opensuse-vm-in-portal](../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## Installieren und Ausführen von MySQL auf dem virtuellen Computer
[!INCLUDE [install-and-run-mysql-on-opensuse-vm](../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## Nächste Schritte
Einzelheiten zu MySQL finden Sie in der [MySQL-Dokumentation][MySQLDocs].

[MySQLDocs]: http://dev.mysql.com/doc/index-topic.html
[MySQL]: http://www.mysql.com

<!---HONumber=AcomDC_0727_2016-->