---
title: Installieren von MySQL auf einer OpenSUSE-VM | Microsoft Docs
description: Erfahren Sie, wie Sie MySQL auf einem virtuellen OpenSUSE Linux-Computer in Azure installieren.
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1594e10e-c314-455a-9efb-a89441de364b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/19/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: f6537e4ebac76b9f3328223ee30647885ee15d3e
ms.openlocfilehash: ff9e23e9324cc47ae1a5d7cb52f13920000b8557


---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>Installieren von MySQL auf einem virtuellen Computer mit OpenSUSE Linux in Azure
[MySQL][MySQL] ist eine beliebte Open-Source-SQL-Datenbank. In diesem Tutorial erfahren Sie, wie Sie einen virtuellen Computer mit OpenSUSE Linux erstellen und anschließend MySQL installieren.

> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.

<br>

## <a name="create-a-virtual-machine-running-opensuse-linux"></a>Erstellen eines virtuellen Computers mit OpenSUSE Linux
[!INCLUDE [create-and-configure-opensuse-vm-in-portal](../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-the-virtual-machine"></a>Installieren und Ausführen von MySQL auf dem virtuellen Computer
[!INCLUDE [install-and-run-mysql-on-opensuse-vm](../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a>Nächste Schritte
Einzelheiten zu MySQL finden Sie in der [MySQL-Dokumentation][MySQLDocs].

[MySQLDocs]: http://dev.mysql.com/doc/index-topic.html
[MySQL]: http://www.mysql.com




<!--HONumber=Feb17_HO3-->


