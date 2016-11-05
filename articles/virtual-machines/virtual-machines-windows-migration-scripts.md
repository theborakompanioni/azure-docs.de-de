---
title: Communitytools für die Migration von der Azure-Dienstverwaltung zum Azure Resource Manager
description: In diesem Artikel werden die Tools katalogisiert, die von der Community zur Unterstützung der Migration von IaaS-Ressourcen von der Azure-Dienstverwaltung zum Azure Resource Manager-Stapel bereitgestellt wurden.
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: timlt
editor: ''
tags: azure-resource-manager

ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/29/2016
ms.author: singhkay

---
# Communitytools für die Migration von der Azure-Dienstverwaltung zum Azure Resource Manager
In diesem Artikel werden die Tools katalogisiert, die von der Community zur Unterstützung der Migration von IaaS-Ressourcen von der Azure-Dienstverwaltung zum Azure Resource Manager-Stapel bereitgestellt wurden.

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

## ASM2ARM
Dies ist ein PowerShell-Skriptmodul für die Migration Ihres **einzelnen** virtuellen Computers (VM) vom Azure-Dienstverwaltungsstapel zum Azure Resource Manager-Stapel.

[Link zur Tooldokumentation](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/asm2arm)

## migAz
migAz ist eine zusätzliche Option zum Migrieren eines vollständigen Satzes von Azure-Dienstverwaltungs-IaaS-Ressourcen zu Azure Resource Manager-IaaS-Ressourcen. Die Migration kann im gleichen Abonnement oder zwischen verschiedenen Abonnements und Abonnementtypen (Ausnahme: CSP-Abonnements) durchgeführt werden.

[Link zur Tooldokumentation](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/migaz)

<!---HONumber=AcomDC_0831_2016-->