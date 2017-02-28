---
title: Verwenden von Managed Disks mit Azure-VM-Skalierungsgruppen | Microsoft-Dokumentation
description: "Es wird beschrieben, wann Sie verwaltete Datenträger mit VM-Skalierungsgruppen verwenden sollten und wie Sie dabei vorgehen."
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/21/2017
ms.author: negat
translationtype: Human Translation
ms.sourcegitcommit: db84d2b03ad1542a898c2c452e62a3f7ef7e6af8
ms.openlocfilehash: 4824a8a24a7e43bc8e8112303f20d916e67b6aff
ms.lasthandoff: 02/17/2017


---
# <a name="azure-vm-scale-sets-and-managed-disks"></a>Azure-VM-Skalierungsgruppen und verwaltete Datenträger

Azure-[VM-Skalierungsgruppen](/azure/virtual-machine-scale-sets/) unterstützen jetzt virtuelle Computer mit verwalteten Datenträgern. Die Verwendung von verwalteten Datenträgern mit Skalierungsgruppen hat mehrere Vorteile, z.B.:

* Es ist nicht mehr erforderlich, Speicherkonten vorab zu erstellen und zu verwalten, um die Betriebssystemdatenträger für die Skalierungsgruppen-VMs zu speichern.

* Sie können verwaltete Datenträger für Daten an die Skalierungsgruppe anfügen.

* Mit einem verwalteten Datenträger kann eine Skalierungsgruppe eine Kapazität von bis zu 1.000 VMs (basierend auf einem Plattformimage) oder 100 VMs (basierend auf einem benutzerdefinierten Image) haben.

## <a name="get-started"></a>Erste Schritte

Eine einfache Möglichkeit für den Einstieg in die Nutzung von Skalierungsgruppen mit verwalteten Datenträgern ist die Bereitstellung über das Azure-Portal. [hier finden Sie weitere Informationen](./virtual-machine-scale-sets-portal-create.md) Eine weitere einfache Möglichkeit ist die Verwendung von [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) zum Bereitstellen einer Skalierungsgruppe. Im folgenden Beispiel wird veranschaulicht, wie Sie eine Ubuntu-basierte Skalierungsgruppe mit 10 VMs erstellen, die jeweils Datenträger mit 50 GB und 100 GB aufweisen:

```azurecli
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

Alternativ dazu können Sie im [GitHub-Repository mit den Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates) nach Ordnern suchen, die `vmss` enthalten. Darin befinden sich fertige Beispiele für Vorlagen zum Bereitstellen von Skalierungsgruppen. Informationen dazu, für welche Vorlagen bereits verwaltete Datenträger verwendet werden, finden Sie in [dieser Liste](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).

## <a name="api-versions"></a>API-Versionen

Die aktuelle allgemein verfügbare API-Version für Skalierungsgruppen mit verwalteten Datenträgern ist `2016-04-30-preview`. Skalierungsgruppen mit nicht verwalteten Datenträgern funktionieren auch für neue API-Versionen, die über Unterstützung für verwaltete Datenträger verfügen, weiter wie bisher. Für Skalierungsgruppen mit nicht verwalteten Datenträgern gelten die Vorteile von verwalteten Datenträgern aber auch dann nicht, wenn diese neuen API-Versionen verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere allgemeine Informationen zu verwalteten Datenträgern finden Sie in [diesem Artikel](../storage/storage-managed-disks-overview.md).

Informationen dazu, wie Sie eine Resource Manager-Vorlage zum Bereitstellen von Skalierungsgruppen mit verwalteten Datenträgern konvertieren, finden Sie in [diesem Artikel](./virtual-machine-scale-sets-convert-template-to-md.md). Die gleichen Änderungen an den Resource Manager-Vorlagen gelten auch für die Azure-REST-API.

Weitere Informationen zur Verwendung von verwalteten Datenträgern mit Skalierungsgruppen finden Sie in [diesem Artikel](./virtual-machine-scale-sets-attached-disks.md).

Informationen zur Verwendung von großen Skalierungsgruppen finden Sie in [diesem Artikel](./virtual-machine-scale-sets-placement-groups.md).



