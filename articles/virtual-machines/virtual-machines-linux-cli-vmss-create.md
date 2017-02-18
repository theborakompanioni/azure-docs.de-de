---
title: Erstellen von Azure-VM-Skalierungsgruppen mit der Azure-Befehlszeilenschnittstelle | Microsoft-Dokumentation
description: "Erfahren Sie mehr über VM-Skalierungsgruppen."
keywords: "virtueller Linux-Computer, Skalierungsgruppen für virtuelle Computer"
services: virtual-machines-linux
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: ba1aedb6-49cb-4546-8b8b-da97aba8e42d
ms.service: virtual-machine-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/24/2016
ms.author: negat
translationtype: Human Translation
ms.sourcegitcommit: 5b8950619da3f42bc3c92443a7b9e9f2a97067f9
ms.openlocfilehash: 8d274127a4e1947e42b5544c3c1f62c4e48dcee9


---
# <a name="what-are-virtual-machine-scale-sets"></a>Was sind Skalierungsgruppen für virtuelle Computer?
Skalierungsgruppen virtueller Computer ermöglichen es Ihnen, mehrere VMs als Satz zu verwalten. Auf hoher Ebene haben Skalierungsgruppen die folgenden Vor- und Nachteile:

Vorteile:

1. Hohe Verfügbarkeit. Jede Skalierungsgruppe platziert ihre virtuellen Computer in eine Verfügbarkeitsgruppe mit fünf Fehlerdomänen (FD) und fünf Updatedomänen (UD), um die Verfügbarkeit sicherzustellen (weitere Informationen zu FDs und UDs finden Sie unter [VM-Verfügbarkeit](virtual-machines-linux-manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). 
2. Einfache Integration mit dem Azure Load Balancer und App Gateway
3. Leichtere Integration mit Azure Autoscale
4. Vereinfachte Bereitstellung, Verwaltung und Bereinigung von virtuellen Computern
5. Unterstützung häufiger Windows- und Linux-Varianten sowie benutzerdefinierter Images

Nachteile:

1. Kann in einer Skalierungsgruppe keine Datenträger an VM-Instanzen anfügen. Muss stattdessen Azure Blob Storage, Azure Files Storage, Azure Table Storage oder andere Speicherlösungen verwenden.

## <a name="quick-create-using-azure-cli"></a>Schnellerfassung mithilfe der Azure-Befehlszeilenschnittstelle 
[!INCLUDE [cli-vmss-quick-create](../../includes/virtual-machines-linux-cli-vmss-quick-create-include.md)]

## <a name="next-steps"></a>Nächste Schritte
Allgemeine Informationen finden Sie auf der [Hauptseite für Skalierungsgruppen](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

Weitere Dokumente finden Sie auf der [Hauptdokumentationsseite für Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Wenn Sie Beispiele für Resource Manager-Vorlagen ansehen möchten, die Skalierungsgruppen verwenden, suchen Sie im [Github-Repository für Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates)nach „vmss“.




<!--HONumber=Jan17_HO4-->


