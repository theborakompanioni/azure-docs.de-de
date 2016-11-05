---
title: Auswählen von Linux-VM-Images mit der Azure-CLI | Microsoft Docs
description: Erfahren Sie, wie Sie den Herausgeber, das Angebot und die SKU für Images ermitteln, wenn Sie mit dem Resource Manager-Bereitstellungsmodell einen virtuellen Linux-Computer erstellen.
services: virtual-machines-linux
documentationcenter: ''
author: squillace
manager: timlt
editor: ''
tags: azure-resource-manager

ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/23/2016
ms.author: rasquill

---
# Auswählen von Linux-VM-Images mit der Azure-CLI
In diesem Thema wird beschrieben wie Sie Herausgeber, Angebote, SKUs und Versionen für jeden Ort finden, an dem Sie etwas bereitstellen möchten. Beispielsweise sind einige der häufig verwendeten Linux-VM-Images die folgenden:

**Tabelle mit häufig verwendeten Linux-Images**

| PublisherName | Angebot | Sku |
|:--- |:--- |:--- |:--- |
| RedHat |RHEL |7,2 |
| credativ |Debian |8 |
| SUSE |openSUSE |13\.2 |
| SUSE |SLES |12-SP1 |
| OpenLogic |CentOS |7,1 |
| Canonical |UbuntuServer |14\.04.4-LTS |
| CoreOS |CoreOS |Stable |

[!INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]

<!---HONumber=AcomDC_0824_2016-->