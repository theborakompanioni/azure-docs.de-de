---
title: 'Gewusst wie: Markieren eines virtuellen Linux-Computers | Microsoft Docs'
description: Informieren Sie sich über das Markieren eines virtuellen Linux-Computers mit Tags, der in Azure mithilfe des Resource Manager-Bereitstellungsmodells erstellt wurde.
services: virtual-machines-linux
documentationcenter: ''
author: mmccrory
manager: timlt
editor: tysonn
tags: azure-resource-manager

ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2016
ms.author: memccror

---
# Gewusst wie: Markieren eines virtuellen Linux-Computers in Azure
In diesem Artikel werden verschiedene Methoden zum Markieren eines virtuellen Linux-Computers in Azure mithilfe des Resource Manager-Bereitstellungsmodells beschrieben. Tags sind benutzerdefinierte Schlüssel-Wert-Paare, die direkt auf einer Ressource oder einer Ressourcengruppe platziert werden können. Azure unterstützt derzeit bis zu 15 Tags pro Ressource und Ressourcengruppe. Tags können zum Zeitpunkt der Erstellung auf einer Ressource platziert werden oder zu einer vorhandenen Ressource hinzugefügt werden. Beachten Sie, dass Tags nur für Ressourcen unterstützt werden, die über das Resource Manager-Bereitstellungsmodell erstellt wurden.

[!INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## Erstellen von Tags mit der Azure-Befehlszeilenschnittstelle
Zunächst [installieren und konfigurieren Sie die Azure-Befehlszeilenschnittstelle](../xplat-cli-azure-resource-manager.md) und stellen sicher, dass Sie sich im Resource Manager-Modus (`azure config mode arm`) befinden.

Sie können alle Eigenschaften für einen bestimmten virtuellen Computer einschließlich der Tags anzeigen, indem Sie den folgenden Befehl verwenden:

        azure vm show -g MyResourceGroup -n MyTestVM

Zum Hinzufügen eines neuen VM-Tags über die Azure-Befehlszeilenschnittstelle können Sie den `azure vm set`-Befehl zusammen mit dem Tag-Parameter **-t** verwenden:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

Um alle Tags zu entfernen, können Sie den **–T**-Parameter im `azure vm set`-Befehl verwenden.

        azure vm set – g MyResourceGroup –n MyTestVM -T


Nun, da wir unseren Ressourcen über die Azure-Befehlszeilenschnittstelle und das Portal Tags zugewiesen haben, werfen wir einen Blick auf die Nutzungsdetails, um die Tags im Abrechnungsportal anzuzeigen.

[!INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## Nächste Schritte
* Weitere Informationen zum Markieren Ihrer Azure-Ressourcen finden Sie unter [Übersicht über den Azure Resource Manager][Übersicht über den Azure Resource Manager] und [Verwenden von Tags zum Organisieren von Azure-Ressourcen][Verwenden von Tags zum Organisieren von Azure-Ressourcen].
* Informationen dazu, wie Tags Ihnen helfen können, die Verwendung Ihrer Azure-Ressourcen zu verwalten, finden Sie unter [Informationen zu Ihrer Rechnung für Microsoft Azure][Informationen zu Ihrer Rechnung für Microsoft Azure] und [Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure][Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure].

[Azure CLI environment]: ./xplat-cli-azure-resource-manager.md
[Übersicht über den Azure Resource Manager]: ../resource-group-overview.md
[Verwenden von Tags zum Organisieren von Azure-Ressourcen]: ../resource-group-using-tags.md
[Informationen zu Ihrer Rechnung für Microsoft Azure]: ../billing-understand-your-bill.md
[Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure]: ../billing-usage-rate-card-overview.md

<!---HONumber=AcomDC_0706_2016-->