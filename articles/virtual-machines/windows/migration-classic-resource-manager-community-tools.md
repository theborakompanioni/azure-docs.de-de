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
ms.date: 03/30/2017
ms.author: kasing
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: d3fc0246088eddeb345bea0ffbd2c5247b218006
ms.contentlocale: de-de
ms.lasthandoff: 07/27/2017

---
# <a name="community-tools-to-migrate-iaas-resources-from-classic-to-azure-resource-manager"></a>Migrieren von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von Communitytools
In diesem Artikel werden die Tools katalogisiert, die von der Community zur Unterstützung der Migration von IaaS-Ressourcen vom klassischen zum Azure Resource Manager-Bereitstellungsmodell bereitgestellt wurden.

> [!NOTE]
> Diese Tools werden vom Microsoft-Support nicht offiziell unterstützt. Daher sind sie auf GitHub als Open-Source-Versionen verfügbar, und wir sind jederzeit offen für Vorschläge in Bezug auf Fixes oder weitere Szenarien. Wenn Sie ein Problem melden möchten, verwenden Sie das Feature für GitHub-Probleme.
> 
> Die Migration mit diesen Tools führt bei Ihrem klassischen virtuellen Computer zu Ausfallzeiten. Informationen zur plattformgestützten Migration finden Sie hier: 
> 
>   * [Plattformgestützte Migration von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zum Azure Resource Manager-Stapel](migration-classic-resource-manager-overview.md)
>   * [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](migration-classic-resource-manager-deep-dive.md)
>   * [Migrieren von IaaS-Ressourcen aus dem klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von Azure PowerShell](migration-classic-resource-manager-ps.md)
> 
> 

## <a name="asmmetadataparser"></a>AsmMetadataParser
Hierbei handelt es sich um eine Sammlung von Hilfsprogrammen, die im Rahmen von Enterprise-Migrationen von der Azure-Dienstverwaltung zu Azure Resource Manager erstellt wird. Dieses Tool ermöglicht Ihnen das Replizieren Ihrer Infrastruktur in ein anderes Abonnement, mit dem Sie die Migration testen und etwaige Probleme beheben können, bevor Sie die Migration für Ihr Produktionsabonnement ausführen.

[Link zur Tooldokumentation](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

## <a name="migaz"></a>migAz
migAz ist eine zusätzliche Option zum Migrieren eines vollständigen Satzes klassischen IaaS-Ressourcen zu Azure Resource Manager-IaaS-Ressourcen. Die Migration kann im gleichen Abonnement oder zwischen verschiedenen Abonnements und Abonnementtypen (Ausnahme: CSP-Abonnements) durchgeführt werden.

[Link zur Tooldokumentation](https://github.com/Azure/migAz)

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über die plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Ausführliche technische Informationen zur plattformgestützten Migration vom klassischen Bereitstellungsmodell zu Azure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Planen der Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Migrieren von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe von PowerShell](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Migrieren von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager mithilfe der Befehlszeilenschnittstelle](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Überprüfen der häufigsten Fehler bei der Migration](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Antworten auf die am häufigsten gestellten Fragen zum Migrieren von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)


