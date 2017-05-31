---
title: "Virtuelle Azure-Computer: Hinzufügen oder Entfernen von Netzwerkschnittstellenkarten | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Netzwerkschnittstellenkarten (NICs) virtuellen Computer hinzugefügt oder von diesen entfernt werden."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: jdial
ms.translationtype: Human Translation
ms.sourcegitcommit: afa23b1395b8275e72048bd47fffcf38f9dcd334
ms.openlocfilehash: 0d609b20040572e3fb371a277603109d64a0ba37
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---

# <a name="add-network-interfaces-to-or-remove-from-virtual-machines"></a>Virtuelle Computer: Hinzufügen oder Entfernen von Netzwerkschnittstellenkarten

Erfahren Sie, wie eine vorhandene Netzwerkschnittstellenkarte (NIC) hinzugefügt wird, wenn Sie einen virtuellen Computer erstellen, oder wie NICs einem virtuellen Computer mit dem Zustand „Beendet (Zuordnung aufgehoben)“ hinzugefügt oder von diesem entfernt werden. Eine NIC ermöglicht einem virtuellen Azure-Computer (VM) die Kommunikation mit dem Internet, Azure und lokalen Ressourcen. Ein virtueller Computer hat ein oder mehrere NICs. 

Informationen zum Hinzufügen, Ändern oder Entfernen von IP-Adressen für eine NIC finden Sie im Artikel [Hinzufügen, Ändern oder Entfernen von IP-Adressen](virtual-network-network-interface-addresses.md). Informationen zum Erstellen, Ändern oder Löschen von NICs finden Sie im Artikel zu [Erstellen, Ändern oder Löschen von Netzwerkschnittstellenkarten](virtual-network-network-interface.md).

## <a name="before"></a>Voraussetzungen

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Weitere Informationen zur unterstützten NIC-Anzahl der einzelnen Größen von Linux- und Windows-VMs finden Sie im entsprechenden Artikel für [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Melden Sie sich mit einem Azure-Konto beim Azure-Portal, der Azure-Befehlszeilenschnittstelle (CLI) oder bei Azure PowerShell an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Bei Verwenden von PowerShell zum Ausführen der Aufgaben in diesem Artikel installieren und konfigurieren Sie Azure PowerShell gemäß den Anweisungen im Artikel [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure PowerShell-Cmdlets installiert haben. Hilfe und Beispiele für PowerShell-Befehle erhalten Sie durch Eingabe von `get-help <command> -full`.
- Bei Verwenden der Azure CLI zum Ausführen der Aufgaben in diesem Artikel installieren und konfigurieren Sie die Azure CLI gemäß den Anweisungen im Artikel zum [Installieren und Konfigurieren der Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure CLI installiert haben. Hilfe zu den CLI-Befehlen erhalten Sie durch Eingabe von `az <command> --help`.

## <a name="about"></a>Informationen zu NICs und VMs

Sie können eine vorhandene NIC einer VM hinzufügen (bzw. an diese anfügen), wenn Sie die VM erstellen. Voraussetzung ist, dass die NIC derzeit nicht an eine andere VM angefügt ist. Sie können eine NIC einer vorhandenen VM hinzufügen oder von dieser entfernen (was auch als Trennen bezeichnet wird). Voraussetzung ist, dass die VM den Zustand „Beendet (Zuordnung aufgehoben)“ hat. Wenn Sie im Azure-Portal eine VM erstellen, generiert das Portal für Sie eine NIC mit Standardeinstellungen. Das Portal lässt Folgendes nicht zu:

- Angeben einer vorhandenen NIC, um sie beim Erstellen des virtuellen Computers hinzuzufügen
- Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellenkarten (NICs)
- Angeben eines Namens für die NIC (wird vom Portal mit einem Standardnamen erstellt)

Sie können Azure PowerShell oder die CLI verwenden, um eine NIC oder VM mit den zuvor erwähnten Attributen zu erstellen, für die das Portal nicht verwendet werden kann. Berücksichtigen Sie bei den Aufgaben in den folgenden Abschnitten die folgenden Einschränkungen und Verhaltensweisen:

- Alle VM-Größen unterstützen mindestens zwei NICs, doch einige VM-Größen unterstützen mehr als zwei NICs. In der Vergangenheit unterstützt einige VM-Größen nur eine NIC. Weitere Informationen zur unterstützten NIC-Anzahl der einzelnen VM-Größen finden Sie im entsprechenden Artikel für [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 
- In der Vergangenheit konnten NICs nur zu virtuellen Computern hinzugefügt werden, die mehrere NICs unterstützt haben und mit mindestens zwei NICs erstellt wurden. Es konnte keine NIC zu einem virtuellen Computer hinzugefügt werden, der mit einer NIC erstellt wurde, auch wenn die Größe des virtuellen Computers mehrere NICs unterstützte. Im Gegenzug konnten Sie nur NICs von einem virtuellen Computer mit mindestens drei NICs entfernen, da virtuelle Computer, die mit zwei NICs erstellt wurden, immer mindestens zwei NICs haben mussten. Keine dieser Einschränkungen trifft noch zu. Sie können jetzt einen virtuellen Computer mit einer beliebigen Anzahl von NICs erstellen (bis zu der Anzahl, die von der VM-Größe unterstützt wird) und eine beliebige Anzahl von NICs hinzufügen oder entfernen (bei VMs im Zustand „Beendet [Zuordnung aufgehoben]“), solange der virtuelle Computer immer mindestens eine NIC aufweist.
- Standardmäßig wird die erste NIC eines virtuellen Computers als *primäre* NIC definiert. Alle anderen NICs der VM sind *sekundäre* NICs.
- Primären NICs wird von Azure-DHCP-Server ein Standardgateway zugewiesen, sekundären NICs dagegen nicht. Da sekundären NICs kein Standardgateway zugewiesen wird, können Sie standardmäßig nicht mit Ressourcen außerhalb ihres Subnetzes kommunizieren. Um sekundäre NICs in einer Windows-VM für die Kommunikation mit Ressourcen außerhalb ihres Subnetzes zu aktivieren, fügen Sie mit dem über eine Windows-Befehlszeile eingegebenen Befehl `route add` dem Betriebssystem Routen hinzu. Da Linux-VMs standardmäßig „schwaches“ Hostrouting verwenden, wird empfohlen, den Datenverkehr für sekundäre NICs auf ein einzelnes Subnetz zu beschränken. Wenn für sekundäre NICs Verbindungen mit Adressen außerhalb des Subnetzes erforderlich sind, aktivieren Sie das richtlinienbasierte Routing, um sicherzustellen, dass für ein- und ausgehenden Datenverkehr dieselbe NIC verwendet wird.
- Standardmäßig wird sämtlicher ausgehende Datenverkehr des virtuellen Computers über die IP-Adresse gesendet, die der primären IP-Konfiguration der primären NIC zugewiesen ist. Sie können im Betriebssystem des virtuellen Computers steuern, welche IP-Adresse für ausgehenden Datenverkehr verwendet wird. Doch standardmäßig erfolgt dies über die primäre NIC.
- In der Vergangenheit mussten alle virtuellen Computer in der gleichen Verfügbarkeitsgruppe über eine einzelne NIC (oder über mehrere NICs) verfügen. In einer Verfügbarkeitsgruppe können nun VMs mit bis zu der Anzahl enthalten sein, die von der VM-Größe unterstützt wird. Sie können eine VM erst einer Verfügbarkeitsgruppe hinzufügen, nachdem sie erstellt wurde. Weitere Informationen zu Verfügbarkeitsgruppen finden Sie im Artikel [Verwalten der Verfügbarkeit von VMs in Azure](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy).
- NICs in derselben VM können zwar mit unterschiedlichen Subnetzen eines VNet verbunden sein, die NICs müssen jedoch alle mit dem gleichen VNet verbunden sein.
- Sie können eine beliebige IP-Adresse für eine beliebige IP-Konfiguration einer beliebigen primären oder sekundären NIC einem Back-End-Pool von Azure Load Balancer hinzufügen. Bisher konnte nur die primäre IP-Adresse für die primäre NIC einem Back-End-Pool hinzugefügt werden. Weitere Informationen zu IP-Adressen und -Konfigurationen finden Sie im Artikel [Hinzufügen, Ändern oder Entfernen von IP-Adressen](virtual-network-network-interface-addresses.md).
- Beim Löschen eines virtuellen Computers werden die an ihn angefügten NICs nicht gelöscht. Wenn ein virtueller Computer gelöscht wird, werden die NICs vom virtuellen Computer getrennt. Sie können die NICs an andere virtuelle Computer anfügen oder löschen.

## <a name="vm-create"></a>Hinzufügen vorhandener NICs zu einer neuen VM
Bei der Erstellung einer VM im Portal wird eine NIC mit Standardeinstellungen generiert, die für Sie an die VM angefügt wird. Über das Azure-Portal ist es nicht möglich, vorhandene NICs an einen neuen virtuellen Computer anzufügen oder einen virtuellen Computer mit mehreren NICs zu erstellen. Verwenden Sie hierfür die CLI oder PowerShell. Sie können einer VM beliebig viele NICs entsprechend der von der erstellten VM-Größe unterstützten Anzahl hinzufügen. Weitere Informationen zur unterstützten NIC-Anzahl der einzelnen VM-Größen finden Sie im entsprechenden Artikel für [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Die NICs, die Sie einer VM hinzufügen, können derzeit an keine andere VM angefügt werden. Weitere Informationen zum Erstellen von NICs finden Sie im Artikel [Erstellen, Ändern oder Löschen von Netzwerkschnittstellenkarten](virtual-network-network-interface.md#create-nic).

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az vm create](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[New-AzureRmVM](/powershell/resourcemanager/azurerm.compute/v2.5.0/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Hinzufügen vorhandener NICs zu einer vorhandenen VM

Sie können einer VM beliebig viele NICs entsprechend der von der VM-Größe unterstützten Anzahl hinzufügen. Weitere Informationen zur unterstützten NIC-Anzahl der einzelnen VM-Größen finden Sie im entsprechenden Artikel für [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Die VM, an der Sie eine NIC hinzufügen möchten, muss mehrere NICs unterstützen und sich im Zustand „Beendet (Zuordnung aufgehoben)“ befinden. Die NICs, die Sie hinzufügen, können derzeit an keine andere VM angefügt werden. NICs können einem vorhandenen virtuellen Computer nicht über das Azure-Portal hinzugefügt werden. Um NICs einer vorhandenen VM hinzuzufügen, müssen Sie die CLI oder PowerShell verwenden.

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az vm nic add](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#add)|
|PowerShell|[Add-AzureRmVMNetworkInterface](/powershell/resourcemanager/azurerm.compute/v2.5.0/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-view-nic"></a> Anzeigen von NICs für eine VM

Sie können die derzeit an eine VM angefügten NICs anzeigen, um mehr über die Konfiguration der einzelnen NICs und die diesen zugewiesenen IP-Adressen zu erfahren. 

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement die Rolle „Besitzer“, „Mitwirkender“ oder „Netzwerkmitwirkender“ zugewiesen ist, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Virtuelle Computer* ein. Wenn **Virtuelle Computer** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Virtuelle Computer** auf den Namen des virtuellen Computers, für den Sie NICs anzeigen möchten.
4. Klicken Sie auf dem Blatt „Virtueller Computer“, das für den ausgewählten virtuellen Computer angezeigt wird, im Abschnitt **EINSTELLUNGEN** auf **Netzwerkschnittstellen**. Weitere Informationen zu NIC-Einstellungen und ihrer Änderung finden Sie im Artikel [Erstellen, Ändern oder Löschen von Netzwerkschnittstellenkarten](virtual-network-network-interface.md). Weitere Informationen zum Hinzufügen, Ändern und Entfernen von IP-Adressen, die einer NIC zugewiesen sind, finden Sie im Artikel [Hinzufügen, Ändern oder Entfernen von IP-Adressen](virtual-network-network-interface-addresses.md).

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az vm show](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVM](/powershell/resourcemanager/azurerm.compute/v1.3.4/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a>Entfernen von NICs von einer VM

Die VM, von der Sie eine NIC entfernen möchten, muss sich im Zustand „Beendet (Zuordnung aufgehoben)“ befinden und über mindestens zwei angefügte NICs verfügen. Sie können jede beliebige NIC entfernen, doch an den virtuellen Computer muss immer mindestens eine NIC angefügt sein. Wenn Sie eine primäre NIC entfernen, weist Azure das Attribut „Primär“ einer NIC zu, die am längsten an den virtuellen Computer angefügt ist. Sie können auch selbst eine beliebige NIC als die primäre festlegen. Über das Azure-Portal können Sie weder NICs von einer VM entfernen noch das Attribut „Primär“ für eine NIC festlegen. Diese beiden Aktionen können jedoch über die CLI oder PowerShell erfolgen. 

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az vm nic remove](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#remove)|
|PowerShell|[Remove-AzureRMVMNetworkInterface](/powershell/resourcemanager/azurerm.compute/v2.5.0/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Sie virtuelle Computer mit mehreren NICs oder IP-Adressen erstellen:

**Befehle**

|Task|Tool|
|---|---|
|Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellenkarten (NICs)|[BEFEHLSZEILENSCHNITTSTELLE (CLI)](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
||[PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Erstellen eines virtuellen Computers mit einer NIC und mehreren IP-Adressen|[BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-multiple-ip-addresses-cli.md)|
||[PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|

