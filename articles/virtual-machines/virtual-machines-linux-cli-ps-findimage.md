---
title: "Auswählen von Linux-VM-Images mit der Azure-CLI | Microsoft Docs"
description: "Erfahren Sie, wie Sie den Herausgeber, das Angebot und die SKU für Images ermitteln, wenn Sie mit dem Resource Manager-Bereitstellungsmodell einen virtuellen Linux-Computer erstellen."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7a858e38-4f17-4e8e-a28a-c7f801101721
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/23/2016
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 732767bcece6d892aee7a8e6976f527fc66bb48d


---
# <a name="select-linux-vm-images-with-the-azure-cli"></a>Auswählen von Linux-VM-Images mit der Azure-CLI
In diesem Thema wird beschrieben wie Sie Herausgeber, Angebote, SKUs und Versionen für jeden Ort finden, an dem Sie etwas bereitstellen möchten. Beispielsweise sind einige der häufig verwendeten Linux-VM-Images die folgenden:

**Tabelle mit häufig verwendeten Linux-Images**

| PublisherName | Angebot | Sku |
|:--- |:--- |:--- |:--- |
| RedHat |RHEL |7,2 |
| credativ |Debian |8 |
| SUSE |openSUSE |13.2 |
| SUSE |SLES |12-SP1 |
| OpenLogic |CentOS |7,1 |
| Canonical |UbuntuServer |14.04.4-LTS |
| CoreOS |CoreOS |Stable |

[!INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]




<!--HONumber=Nov16_HO3-->


