---
title: "Communitytools – Verschieben klassischer Ressourcen zu Azure Resource Manager | Microsoft-Dokumentation"
description: "In diesem Artikel werden die Tools katalogisiert, die von der Community zur Unterstützung der Migration von IaaS-Ressourcen vom klassischen zum Azure Resource Manager-Bereitstellungsmodell bereitgestellt wurden."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 228b697b-3950-49f5-84bb-283bb56621b1
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 1/23/2017
ms.author: kasing
translationtype: Human Translation
ms.sourcegitcommit: 2deaf44de51a989463826d6e4815f9ce9f0bbee5
ms.openlocfilehash: 4b1fd3527140bc40299c008071753a91d264d567


---
# <a name="community-tools-to-migrate-iaas-resources-from-classic-to-azure-resource-manager"></a>Migrieren von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von Communitytools
In diesem Artikel werden die Tools katalogisiert, die von der Community zur Unterstützung der Migration von IaaS-Ressourcen vom klassischen zum Azure Resource Manager-Bereitstellungsmodell bereitgestellt wurden.

> [!NOTE]
> Diese Tools werden vom Microsoft-Support nicht offiziell unterstützt. Daher sind sie auf GitHub als Open-Source-Versionen verfügbar, und wir sind jederzeit offen für Vorschläge in Bezug auf Fixes oder zusätzliche Szenarien. Wenn Sie ein Problem melden möchten, verwenden Sie das Github-Problemfeature.
> 
> Die Migration mit diesen Tools führt bei Ihrem klassischen virtuellen Computer zu Ausfallzeiten. Informationen zur plattformgestützten Migration finden Sie hier: 
> 
> * [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zum Azure Resource Manager-Stapel](virtual-machines-windows-migration-classic-resource-manager.md)
> * [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
> * [Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von Azure PowerShell](virtual-machines-windows-ps-migration-classic-resource-manager.md)
> 
> 

## <a name="asm2arm"></a>ASM2ARM
Dies ist ein PowerShell-Skriptmodul für die Migration Ihres **einzelnen** virtuellen Computers vom klassischen zum Azure Resource Manager-Bereitstellungsmodell. 

[Link zur Tooldokumentation](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/asm2arm)

## <a name="migaz"></a>migAz
migAz ist eine zusätzliche Option zum Migrieren eines vollständigen Satzes klassischen IaaS-Ressourcen zu Azure Resource Manager-IaaS-Ressourcen. Die Migration kann im gleichen Abonnement oder zwischen verschiedenen Abonnements und Abonnementtypen (Ausnahme: CSP-Abonnements) durchgeführt werden.

[Link zur Tooldokumentation](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/migaz)




<!--HONumber=Jan17_HO4-->


