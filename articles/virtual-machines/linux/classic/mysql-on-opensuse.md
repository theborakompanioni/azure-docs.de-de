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
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: 01b798a25575b66f89057315ce80d6cc0cde53b5
ms.lasthandoff: 03/27/2017


---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>Installieren von MySQL auf einem virtuellen Computer mit OpenSUSE Linux in Azure
[MySQL][MySQL] ist eine beliebte Open-Source-SQL-Datenbank. In diesem Tutorial erfahren Sie, wie Sie einen virtuellen Computer mit OpenSUSE Linux erstellen und anschließend MySQL installieren.

> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../../../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.

<br>

## <a name="create-a-virtual-machine-running-opensuse-linux"></a>Erstellen eines virtuellen Computers mit OpenSUSE Linux
[!INCLUDE [create-and-configure-opensuse-vm-in-portal](../../../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-the-virtual-machine"></a>Installieren und Ausführen von MySQL auf dem virtuellen Computer
[!INCLUDE [install-and-run-mysql-on-opensuse-vm](../../../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a>Nächste Schritte
Einzelheiten zu MySQL finden Sie in der [MySQL-Dokumentation][MySQLDocs].

[MySQLDocs]:http://dev.mysql.com/doc/index-topic.html
[MySQL]:http://www.mysql.com


