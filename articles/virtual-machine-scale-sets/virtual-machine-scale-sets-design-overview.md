---
title: "Entwerfen von Skalierungsgruppen virtueller Azure Computer für die Skalierung | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über das Entwerfen von Skalierungsgruppen virtueller Azure-Computer für die Skalierung"
keywords: "virtueller Linux-Computer, Skalierungsgruppen für virtuelle Computer"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: c27c6a59-a0ab-4117-a01b-42b049464ca1
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: negat
translationtype: Human Translation
ms.sourcegitcommit: e869b06935736fae72bd3b5407ebab7c3830098d
ms.openlocfilehash: de3687a1bf36bf49db400a5660ac631f20b629d0


---
# <a name="designing-vm-scale-sets-for-scale"></a>Entwerfen von VM-Skalierungsgruppen für die Skalierung
In diesem Thema werden Überlegungen zum Entwurf von Skalierungsgruppen für virtuelle Computer erörtert. Informationen darüber, was Skalierungsgruppen für virtuelle Computer sind, finden Sie unter [Übersicht über VM-Skalierungsgruppen](virtual-machine-scale-sets-overview.md).

## <a name="storage"></a>Speicher

### <a name="scale-sets-with-azure-managed-disks"></a>Skalierungsgruppen mit Azure Managed Disks
Skalierungsgruppen können jetzt mit [Azure Managed Disks](../storage/storage-managed-disks-overview.md) erstellt werden. Das Feature „Managed Disks“ (Verwaltete Datenträger) bietet die folgenden Vorteile:
- Sie müssen für die VMs in der Skalierungsgruppe nicht vorab eine Gruppe von Azure-Speicherkonten erstellen.
- Sie können in Ihrer Skalierungsgruppe [angefügte Datenträger](virtual-machine-scale-sets-attached-disks.md) für die virtuellen Computer definieren.
- Skalierungsgruppen können so konfiguriert werden, dass [bis zu 1.000 virtuelle Computer in einer Gruppe unterstützt werden](virtual-machine-scale-sets-placement-groups.md). 

Sie können Skalierungsgruppen mit Managed Disks ab Version „2016-04-30-preview“ der Azure Compute-API erstellen. Informationen zum Konvertieren einer Skalierungsgruppenvorlage zu Managed Disks finden Sie unter [Konvertieren einer Skalierungsgruppenvorlage in eine Skalierungsgruppenvorlage für verwaltete Datenträger](virtual-machine-scale-sets-convert-template-to-md.md).

### <a name="user-managed-storage"></a>Vom Benutzer verwalteter Speicher
Eine Skalierungsgruppe, die nicht mit Azure Managed Disks definiert ist, benötigt vom Benutzer erstellte Speicherkonten zum Speichern der Betriebssystem-Datenträger der VMs in der Gruppe. Pro Speicherkonto werden maximal 20 VMs empfohlen, um die maximale E/A-Leistung zu erzielen und von der _Überbereitstellung_ zu profitieren. Außerdem sollten Sie als Anfangszeichen der Speicherkontonamen verschiedene Buchstaben verwenden. Auf diese Weise lässt sich die Last leichter auf verschiedene interne Systeme verteilen. 

>[!NOTE]
>Die VM-Skalierungsgruppen-API mit der Version `2016-04-30-preview` unterstützt das Verwenden von Azure Managed Disks für den Betriebssystem-Datenträger und alle zusätzlichen Datenträger. Weitere Informationen finden Sie unter [Übersicht über Azure Managed Disks](../storage/storage-managed-disks-overview.md) und [Verwenden angefügter Datenträger](virtual-machine-scale-sets-attached-disks.md). 

## <a name="overprovisioning"></a>Überbereitstellung
Ab API-Version „2016-03-30“ erfolgt die „Überbereitstellung“ virtueller Computer bei VM-Skalierungsgruppen standardmäßig. Wenn die Überbereitstellung aktiviert ist, richtet die Skalierungsgruppe tatsächlich mehr VMs ein, als Sie angefordert haben, und löscht dann die zusätzlichen VMs, nachdem die angeforderte Anzahl von VMs erfolgreich bereitgestellt wurde. Die Überbereitstellung verbessert die Erfolgsquoten bei der Bereitstellung und verkürzt die Bereitstellungsdauer. Die zusätzlichen VMs werden Ihnen nicht berechnet und nicht auf Ihre Kontingentgrenzen angerechnet.

Während die Überbereitstellung die Erfolgsquoten bei der Bereitstellung verbessert, kann sie ein verwirrendes Verhalten bei Anwendungen verursachen, die nicht darauf ausgelegt sind, mit dem Vorhandensein und anschließenden Nichtvorhandensein von VMs umzugehen. Um die Überbereitstellung zu deaktivieren, stellen Sie sicher, dass Ihre Vorlage folgende Zeichenfolge enthält: "overprovision": "false". Weitere Einzelheiten finden Sie in der [REST-API-Dokumentation für VM-Skalierungsgruppen](https://msdn.microsoft.com/library/azure/mt589035.aspx).

Wenn die Skalierungsgruppe vom Benutzer verwalteten Speicher nutzt und Sie die Überbereitstellung deaktivieren, sind mehr als 20 VMs pro Speicherkonto möglich, doch aus Gründen der E/A-Leistung werden nur maximal 40 empfohlen. 

## <a name="limits"></a>Grenzen
Eine für ein Marketplace-Image (auch Plattformimage genannt) erstellte Skalierungsgruppe, die für die Verwendung von Azure Managed Disks konfiguriert ist, unterstützt eine Kapazität von bis zu 1.000 VMs. Wenn Sie Ihre Skalierungsgruppe für eine Unterstützung von mehr als 100 VMs konfiguriert haben, funktionieren nicht alle Szenarien identisch (z.B. der Lastenausgleich). Weitere Informationen finden Sie unter [Verwenden umfangreicher VM-Skalierungsgruppen](virtual-machine-scale-sets-placement-groups.md). 

Eine Skalierungsgruppe mit vom Benutzer verwalteten Speicherkonten ist derzeit auf 100 VMs begrenzt (und für diese Skalierung werden 5 Speicherkonten empfohlen).

Bei Konfiguration mit Azure Managed Disks kann eine auf einem benutzerdefinierten (oder von Ihnen erstellten) Image basierende Skalierungsgruppe bis zu 100 VMs aufweisen. Wenn die Skalierungsgruppe mit vom Benutzer verwalteten Speicherkonten konfiguriert ist, müssen alle Betriebssystem-Datenträger-VHDs innerhalb eines Speicherkontos erstellt werden. Folglich ist 20 die maximal empfohlene Anzahl von VMs in einer Skalierungsgruppe, die auf einem benutzerdefinierten Image und vom Benutzer verwalteten Speicher basiert. Wenn Sie die Überbereitstellung deaktivieren, können Sie bis zu 40 gehen.

Wenn die Anzahl benötigter virtueller Computer über diese Grenzwerte hinausgeht, müssen Sie mehrere Skalierungsgruppen bereitstellen, wie in dieser [Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale)gezeigt.




<!--HONumber=Feb17_HO2-->


