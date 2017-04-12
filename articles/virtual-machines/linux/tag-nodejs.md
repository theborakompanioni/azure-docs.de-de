---
title: Markieren eines virtuellen Linux-Computers in Azure | Microsoft-Dokumentation
description: "Informieren Sie sich über das Markieren eines virtuellen Linux-Computers unter Azure mit Tags, der in Azure mithilfe des Resource Manager-Bereitstellungsmodells erstellt wurde."
services: virtual-machines-linux
documentationcenter: 
author: mmccrory
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ca0e17e5-d78e-42e6-9dad-c1e8f1c58027
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: memccror
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: f643001c85e127ae39e9869ffdc689bcac232ccb
ms.lasthandoff: 04/03/2017


---
# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Gewusst wie: Markieren eines virtuellen Linux-Computers in Azure
In diesem Artikel werden verschiedene Methoden zum Markieren eines virtuellen Linux-Computers in Azure mithilfe des Resource Manager-Bereitstellungsmodells beschrieben. Tags sind benutzerdefinierte Schlüssel-Wert-Paare, die direkt auf einer Ressource oder einer Ressourcengruppe platziert werden können. Azure unterstützt derzeit bis zu 15 Tags pro Ressource und Ressourcengruppe. Tags können zum Zeitpunkt der Erstellung auf einer Ressource platziert werden oder zu einer vorhandenen Ressource hinzugefügt werden. Beachten Sie, dass Tags nur für Ressourcen unterstützt werden, die über das Resource Manager-Bereitstellungsmodell erstellt wurden.

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Erstellen von Tags mit der Azure-Befehlszeilenschnittstelle
Zunächst [installieren und konfigurieren Sie die Azure-Befehlszeilenschnittstelle](../../xplat-cli-azure-resource-manager.md) und stellen sicher, dass Sie sich im Resource Manager-Modus (`azure config mode arm`) befinden.

Sie können alle Eigenschaften für einen bestimmten virtuellen Computer einschließlich der Tags anzeigen, indem Sie den folgenden Befehl verwenden:

        azure vm show -g MyResourceGroup -n MyTestVM

Zum Hinzufügen eines neuen VM-Tags über die Azure-Befehlszeilenschnittstelle können Sie den `azure vm set` -Befehl zusammen mit dem Tag-Parameter **-t**verwenden:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

Um alle Tags zu entfernen, können Sie den **–T**-Parameter im `azure vm set`-Befehl verwenden.

        azure vm set – g MyResourceGroup –n MyTestVM -T


Nun, da wir unseren Ressourcen über die Azure-Befehlszeilenschnittstelle und das Portal Tags zugewiesen haben, werfen wir einen Blick auf die Nutzungsdetails, um die Tags im Abrechnungsportal anzuzeigen.

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Markieren Ihrer Azure-Ressourcen finden Sie unter [Übersicht über den Azure Resource Manager][Azure Resource Manager Overview] und [Verwenden von Tags zum Organisieren von Azure-Ressourcen][Using Tags to organize your Azure Resources].
* Informationen dazu, wie Tags Ihnen helfen können, Ihre Verwendung von Azure-Ressourcen zu verwalten, finden Sie unter [Informationen zu Ihrer Rechnung für Microsoft Azure][Understanding your Azure Bill] und [Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure][Gain insights into your Microsoft Azure resource consumption].

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md

