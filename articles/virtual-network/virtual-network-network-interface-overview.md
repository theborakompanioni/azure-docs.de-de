---
title: Netzwerkschnittstellen in Azure | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu Azure-Netzwerkschnittstellen im Azure Resource Manager-Bereitstellungsmodell.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f58b503f-18bf-4377-aa63-22fc8a96e4be
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 3244d5b52785d820698bf26f9bf189de93ef64e4
ms.openlocfilehash: 691b79d7739246dad7191195fa049fd58340c8ff


---
# <a name="network-interfaces-in-azure"></a>Netzwerkschnittstellen in Azure
Eine Netzwerkschnittstelle (NIC) ist eine Verbindung zwischen einem virtuellen Computer (VM) und dem zugrunde liegenden Softwarenetzwerk. In diesem Artikel wird erläutert, was eine Netzwerkschnittstelle ist und wie sie im Azure Resource Manager-Bereitstellungsmodell verwendet wird.

Microsoft empfiehlt die Bereitstellung neuer Ressourcen mit dem Resource Manager-Bereitstellungsmodell. Sie können aber auch virtuelle Computer mit Netzwerkkonnektivität im [klassischen](virtual-network-ip-addresses-overview-classic.md) Bereitstellungsmodell bereitstellen. Wenn Sie mit dem klassischen Modell vertraut sind, gibt es einige wichtige Unterschiede bei VM-Netzwerken im Resource Manager-Bereitstellungsmodell. Weitere Informationen zu den Unterschieden finden Sie im Artikel [Netzwerke mit virtuellen Computern – klassisch](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments).

In Azure gilt für Netzwerkschnittstellen Folgendes:

1. Sie sind Ressourcen, die erstellt und gelöscht werden können und eigene konfigurierbare Einstellungen aufweisen.
2. Sie müssen bei der Erstellung mit einem Subnetz in einem virtuellen Azure-Netzwerk (VNET) verbunden werden. Sofern Sie nicht mit VNETs vertraut sind, finden Sie weitere Informationen im Artikel [Übersicht über virtuelle Netzwerke](virtual-networks-overview.md) . Die Netzwerkschnittstelle (NIC) muss mit einem VNET verbunden werden, das in derselben Azure-[Region](https://azure.microsoft.com/regions) und unter demselben [Abonnement](../azure-glossary-cloud-terminology.md#subscription) wie die NIC vorhanden ist. Nachdem eine NIC erstellt wurde, können Sie zwar noch das Subnetz ändern, mit dem sie verbunden ist, jedoch nicht mehr das VNET, mit dem sie verbunden ist.
3. Ihnen ist ein Name zugewiesen, der nach dem Erstellen der NIC nicht mehr geändert werden kann. Der Name muss innerhalb einer Azure-[Ressourcengruppe](../azure-resource-manager/resource-group-overview.md#resource-groups) eindeutig sein. Eindeutigkeit innerhalb des Abonnements oder des Azure-Standorts, an dem sie erstellt wurde, oder innerhalb der verbundenen VNETs ist jedoch nicht erforderlich. In einem Azure-Abonnement werden häufig mehrere NICs erstellt. Es wird empfohlen, dass Sie eine Namenskonvention anwenden, die das Verwalten mehrerer NICs gegenüber der Verwendung von Standardnamen vereinfacht. Anregungen finden Sie im Artikel [Empfohlene Namenskonventionen für Azure-Ressourcen](../guidance/guidance-naming-conventions.md).
4. Sie können einem virtuellen Computer angefügt werden – aber nur einem einzelnen virtuellen Computer am gleichen Standort wie die NIC.
5. Sie verfügen über eine MAC-Adresse, die für NICs so lange beibehalten wird, wie diese mit einem virtuellen Computer verbunden sind. Die MAC-Adresse wird auch beibehalten, wenn die VM neu gestartet (aus dem Betriebssystem heraus) oder beendet (aufgehobene Zuordnung) und über das Azure-Portal, Azure PowerShell oder die Azure-Befehlszeilenschnittstelle gestartet wird. Wird die Verbindung mit einem virtuellen Computer getrennt und die NIC anschließend einer anderen VM angefügt, erhält die NIC eine andere MAC-Adresse. Wenn die NIC gelöscht wird, wird die MAC-Adresse anderen NICs zugewiesen.
6. Ihnen muss eine primäre **private** *IPv4* -IP-Adresse zugewiesen werden.
7. Ihnen kann eine öffentliche IP-Adressressource zugeordnet werden.
8. Sie unterstützen Accelerated Networking mit E/A-Virtualisierung mit Einzelstamm (Single-Root I/O Virtualization, SR-IOV) für bestimmte VM-Größen, auf denen bestimmte Versionen des Betriebssystems Microsoft Windows Server ausgeführt werden muss. Weitere Informationen zu diesem Vorschaufeature finden Sie im Artikel [Accelerated Networking für virtuelle Computer](virtual-network-accelerated-networking-powershell.md) .
9. Sie können Datenverkehr empfangen, der nicht an die ihnen zugewiesenen privaten IP-Adressen gerichtet ist, wenn für die NIC die IP-Weiterleitung aktiviert wurde. Wenn auf einem virtuellen Computer z.B. Firewallsoftware ausgeführt wird, leiten sie auch Pakete weiter, die nicht an die eigenen IP-Adressen gerichtet sind. Der virtuelle Computer muss weiterhin Software für das Routing oder die Weiterleitung von Datenverkehr ausführen, dazu muss jedoch die IP-Weiterleitung für eine NIC aktiviert sein.
10. Sie werden oft in derselben Ressourcengruppe wie die virtuellen Computer erstellt, denen sie zugeordnet sind, oder in demselben virtuellen Netzwerk, mit dem sie verbunden sind – auch wenn dies keine Voraussetzungen sind.

## <a name="vms-with-multiple-network-interfaces"></a>Virtuelle Computer mit mehreren Netzwerkschnittstellen
Sie können auch mehrere Netzwerkkarten (NICs) an eine VM anfügen, sollten dabei aber Folgendes beachten:  

* Die VM-Größe muss mehrere NICs unterstützen. Weitere Informationen zu den VM-Größen, die mehrere NICs unterstützen, finden Sie in den Artikeln [Windows Server-VM-Größen](../virtual-machines/virtual-machines-windows-sizes.md) und [Linux-VM-Größen](../virtual-machines/virtual-machines-linux-sizes.md).
* Die VM muss mit mindestens zwei NICs erstellt werden. Wenn die VM mit nur einer NIC erstellt wird, können Sie nach ihrer Erstellung keine zusätzlichen NICs an die VM anfügen. Dies gilt auch, wenn die VM-Größe mehr als eine NIC unterstützt. Sofern die VM mit mindestens zwei NICs erstellt wurde, können Sie nach der Erstellung weitere NICs an die VM anfügen, wenn die VM-Größe mehr als zwei NICs unterstützt.  
* Sie können sekundäre NICs von einer VM trennen (die primäre NIC kann nicht getrennt werden), wenn mindestens drei NICs angefügt sind. Das Trennen von NICs ist nicht möglich, wenn nicht mehr als zwei NICs an die VM angefügt sind.  
* Die interne Reihenfolge der Netzwerkkarten im virtuellen Computer ist zufällig und kann sich auch zwischen Azure-Infrastrukturaktualisierungen ändern. Die IP-Adressen und die zugehörigen Ethernet-MAC-Adressen ändern sich aber nicht. Nehmen wir beispielsweise an, das Betriebssystem identifiziert Azure NIC1 als Eth1. Eth1 hat die IP-Adresse 10.1.0.100 und die MAC-Adresse 00-0D-3A-B0-39-0D. Nach einem Update der Azure-Infrastruktur und einem Neustart identifiziert das Betriebssystem Azure NIC1 jetzt ggf. als Eth2. Die IP- und die MAC-Adresse sind aber unverändert, und das Betriebssystem führt Azure NIC1 weiterhin als Eth1. Wenn der Neustart vom Benutzer initiiert wird, bleibt die NIC-Reihenfolge im Betriebssystem unverändert.  
* Falls die VM Mitglied einer [Verfügbarkeitsgruppe](../azure-glossary-cloud-terminology.md#availability-set) ist, müssen alle VMs in der Verfügbarkeitsgruppe entweder nur eine NIC oder mehrere NICs aufweisen. Falls die VMs über mehrere NICs verfügen, muss die jeweilige Anzahl nicht unbedingt gleich sein, solange jede VM mindestens zwei NICs aufweist.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie im Artikel [Erstellen einer VM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) , wie Sie eine VM mit einer einzigen NIC erstellen.
* Erfahren Sie im Artikel [Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellenkarten (NICs)](virtual-network-deploy-multinic-arm-ps.md) , wie Sie einen virtuellen Computer mit mehreren NICs erstellen.
* Erfahren Sie im Artikel [Mehrere IP-Adressen für virtuelle Azure-Computer](virtual-network-multiple-ip-addresses-powershell.md) , wie Sie eine NIC mit mehreren IP-Konfigurationen erstellen.




<!--HONumber=Jan17_HO5-->


