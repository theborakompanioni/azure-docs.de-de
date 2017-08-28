---
title: "Virtuelle Azure-Computer: Hinzufügen oder Entfernen von Netzwerkschnittstellenkarten | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Netzwerkschnittstellen zu virtuellen Computern hinzufügen oder aus diesen entfernen können."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: jdial
ms.translationtype: HT
ms.sourcegitcommit: b309108b4edaf5d1b198393aa44f55fc6aca231e
ms.openlocfilehash: 57f95b765b1b116814683a6643db16091c3041f6
ms.contentlocale: de-de
ms.lasthandoff: 08/15/2017

---

# <a name="add-network-interfaces-to-or-remove-from-virtual-machines"></a>Virtuelle Computer: Hinzufügen oder Entfernen von Netzwerkschnittstellenkarten

Erfahren Sie, wie eine vorhandene Netzwerkschnittstellenkarte hinzugefügt wird, wenn Sie einen virtuellen Computer erstellen, oder wie Netzwerkschnittstellen einem virtuellen Computer mit dem Zustand „Beendet (Zuordnung aufgehoben)“ hinzugefügt oder von diesem entfernt werden. Eine Netzwerkschnittstelle ermöglicht einem virtuellen Azure-Computer (VM) die Kommunikation mit dem Internet, Azure und lokalen Ressourcen. Ein virtueller Computer kann eine oder mehrere Netzwerkschnittstellen haben. 

Weitere Informationen zum Hinzufügen, Ändern oder Entfernen von IP-Adressen für eine Netzwerkschnittstelle finden Sie im Artikel [Hinzufügen, Ändern oder Entfernen von IP-Adressen für Azure-Netzwerkschnittstellen](virtual-network-network-interface-addresses.md). Informationen zum Erstellen, Ändern oder Löschen von Netzwerkschnittstellen finden Sie im Artikel [Erstellen, Ändern oder Löschen von Netzwerkschnittstellen](virtual-network-network-interface.md).

## <a name="before"></a>Voraussetzungen

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Weitere Informationen zur unterstützten Anzahl von Netzwerkschnittstellen der einzelnen Größen von Linux- und Windows-VMs finden Sie im entsprechenden Artikel für [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Melden Sie sich mit einem Azure-Konto beim [Azure-Portal](https://portal.azure.com), der Azure-Befehlszeilenschnittstelle (CLI) oder bei Azure PowerShell an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Wenn Sie PowerShell-Befehle verwenden, um Aufgaben in diesem Artikel durchzuführen, [installieren und konfigurieren Sie Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure PowerShell-Cmdlets installiert haben. Hilfe und Beispiele für PowerShell-Befehle erhalten Sie durch Eingabe von `get-help <command> -full`.
- Wenn Sie Befehle der Azure-Befehlszeilenschnittstelle (CLI) verwenden, um Aufgaben in diesem Artikel durchzuführen, [installieren und konfigurieren Sie Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure CLI installiert haben. Hilfe zu den Befehlen der Befehlszeilenschnittstelle erhalten Sie durch Eingabe von `az <command> --help`. Anstatt die CLI und ihre Voraussetzungen zu installieren, können Sie die Azure Cloud Shell verwenden. Azure Cloud Shell ist eine kostenlose Bash-Shell, die Sie direkt im Azure-Portal ausführen können. Die Azure CLI ist vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Wenn Sie die Cloud Shell verwenden möchten, klicken Sie oben im [Portal](https://portal.azure.com) auf die Cloud Shell-Schaltfläche **>_**.

## <a name="about"></a>Netzwerkschnittstellen und virtuelle Computer

Sie können eine vorhandene Netzwerkschnittstelle einer VM hinzufügen (bzw. an diese anfügen), wenn Sie die VM erstellen. Voraussetzung ist, dass die Netzwerkschnittstelle derzeit nicht zu einem anderen virtuellen Computer hinzugefügt wurde. Sie können eine Netzwerkschnittstelle einer vorhandenen VM hinzufügen oder von dieser entfernen (was auch als Trennen bezeichnet wird). Voraussetzung ist, dass die VM den Zustand „Beendet (Zuordnung aufgehoben)“ hat. Wenn Sie im Azure-Portal eine VM erstellen, generiert das Portal für Sie eine Netzwerkschnittstelle mit Standardeinstellungen. Das Portal lässt Folgendes nicht zu:

- Angeben einer vorhandenen Netzwerkschnittstelle, um sie beim Erstellen des virtuellen Computers hinzuzufügen
- Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellen
- Angeben eines Namens für die Netzwerkschnittstelle (wird vom Portal mit einem Standardnamen erstellt)

Sie können Azure PowerShell oder die CLI verwenden, um eine Netzwerkschnittstelle oder VM mit den zuvor erwähnten Attributen zu erstellen, für die das Portal nicht verwendet werden kann. Berücksichtigen Sie bei den Aufgaben in den folgenden Abschnitten die folgenden Einschränkungen und Verhaltensweisen:

- Alle VM-Größen unterstützen mindestens zwei Netzwerkschnittstellen, doch einige VM-Größen unterstützen mehr als zwei Netzwerkschnittstellen. In der Vergangenheit unterstützten einige VM-Größen nur eine Netzwerkschnittstelle. Weitere Informationen zur unterstützten Anzahl an Netzwerkschnittstellen der einzelnen VM-Größen finden Sie im entsprechenden Artikel für [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 
- In der Vergangenheit konnten Netzwerkschnittstellen nur zu virtuellen Computern hinzugefügt werden, die mehrere Netzwerkschnittstellen unterstützten und mit mindestens zwei Netzwerkschnittstellen erstellt wurden. Es konnte keine Netzwerkschnittstelle zu einem virtuellen Computer hinzugefügt werden, der mit einer Netzwerkschnittstelle erstellt wurde, auch wenn die Größe des virtuellen Computers mehrere Netzwerkschnittstellen unterstützte. Im Gegenzug konnten Sie nur Netzwerkschnittstellen von einem virtuellen Computer mit mindestens drei Netzwerkschnittstellen entfernen, da virtuelle Computer, die mit zwei Netzwerkschnittstellen erstellt wurden, immer mindestens zwei Netzwerkschnittstellen haben mussten. Keine dieser Einschränkungen trifft noch zu. Sie können jetzt einen virtuellen Computer mit einer beliebigen Anzahl von Netzwerkschnittstellen erstellen (bis zu der Anzahl, die von der VM-Größe unterstützt wird) und eine beliebige Anzahl von Netzwerkschnittstellen hinzufügen oder entfernen (bei VMs im Zustand „Beendet [Zuordnung aufgehoben]“), solange der virtuelle Computer immer mindestens eine Netzwerkschnittstelle aufweist.
- Standardmäßig wird die erste Netzwerkschnittstelle eines virtuellen Computers als *primäre* Netzwerkschnittstelle definiert. Alle anderen Netzwerkschnittstellen auf dem virtuellen Computer sind *sekundäre* Netzwerkschnittstellen.
- Primären Netzwerkschnittstellen wird von Azure-DHCP-Server ein Standardgateway zugewiesen, sekundären Netzwerkschnittstellen hingegen nicht. Da sekundären Netzwerkschnittstellen kein Standardgateway zugewiesen wird, können sie standardmäßig nicht mit Ressourcen außerhalb ihres Subnetzes kommunizieren. Um sekundäre Netzwerkschnittstellen in einer Windows-VM für die Kommunikation mit Ressourcen außerhalb ihres Subnetzes zu aktivieren, fügen Sie mit dem über eine Windows-Befehlszeile eingegebenen Befehl `route add` dem Betriebssystem Routen hinzu. Da Linux-VMs standardmäßig „schwaches“ Hostrouting verwenden, wird empfohlen, den Datenverkehr für sekundäre Netzwerkschnittstellen auf ein einzelnes Subnetz zu beschränken. Wenn für sekundäre Netzwerkschnittstellen Verbindungen mit Adressen außerhalb des Subnetzes erforderlich sind, aktivieren Sie das richtlinienbasierte Routing, um sicherzustellen, dass für ein- und ausgehenden Datenverkehr dieselbe Netzwerkschnittstelle verwendet wird.
- Standardmäßig wird sämtlicher ausgehende Datenverkehr des virtuellen Computers über die IP-Adresse gesendet, die der primären IP-Konfiguration der primären Netzwerkschnittstelle zugewiesen ist. Sie können im Betriebssystem des virtuellen Computers steuern, welche IP-Adresse für ausgehenden Datenverkehr verwendet wird. Doch standardmäßig erfolgt dies über die primäre Netzwerkschnittstelle.
- In der Vergangenheit mussten alle virtuellen Computer in der gleichen Verfügbarkeitsgruppe über eine einzelne Netzwerkschnittstelle (oder über mehrere Netzwerkschnittstellen) verfügen. In einer Verfügbarkeitsgruppe können nun VMs mit bis zu der von der VM-Größe unterstützten Anzahl an Netzwerkschnittstellen vertreten sein. Sie können eine VM erst einer Verfügbarkeitsgruppe hinzufügen, nachdem sie erstellt wurde. Weitere Informationen zu Verfügbarkeitsgruppen finden Sie im Artikel [Verwalten der Verfügbarkeit von VMs in Azure](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy).
- Netzwerkschnittstellen in derselben VM können zwar mit unterschiedlichen Subnetzen eines VNet verbunden sein, die Netzwerkschnittstellen müssen jedoch alle mit dem gleichen VNet verbunden sein.
- Sie können eine beliebige IP-Adresse für eine beliebige IP-Konfiguration einer beliebigen primären oder sekundären Netzwerkschnittstelle einem Back-End-Pool von Azure Load Balancer hinzufügen. Bisher konnte nur die primäre IP-Adresse für die primäre Netzwerkschnittstelle einem Back-End-Pool hinzugefügt werden. Weitere Informationen zu IP-Adressen und -Konfigurationen finden Sie im Artikel [Hinzufügen, Ändern oder Entfernen von IP-Adressen](virtual-network-network-interface-addresses.md).
- Beim Löschen eines virtuellen Computers werden seine Netzwerkschnittstellen nicht gelöscht. Wenn ein virtueller Computer gelöscht wird, werden die Netzwerkschnittstellen von dem virtuellen Computer getrennt. Sie können die Netzwerkschnittstellen an andere virtuelle Computer anfügen oder löschen.
- Wenn eine Netzwerkschnittstelle über private IPv6-Adressen verfügt, können Sie sie an einen virtuellen Computer anhängen, wenn Sie einen erstellen. Sie können keine Netzwerkschnittstelle mit einer zugewiesenen IPv6-Adresse einem virtuellen Computer zuweisen, nachdem dieser erstellt wurde. Wenn Sie eine Netzwerkschnittstelle mit einer zugewiesenen privaten IPv6-Adresse anfügen, wenn Sie einen virtuellen Computer erstellen, können Sie nur diese Netzwerkschnittstelle dem virtuellen Computer anfügen, ungeachtet davon, wie viele Netzwerkschnittstellen die VM-Größe unterstützt. Weitere Informationen zum Zuweisen von IP-Adressen an Netzwerkschnittstellen finden Sie unter [IP-Adressen für Netzwerkschnittstellen](virtual-network-network-interface-addresses.md).

## <a name="vm-create"></a>Hinzufügen vorhandener Netzwerkschnittstellen zu einer neuen VM

Bei der Erstellung eines virtuellen Computers im Portal wird eine Netzwerkschnittstelle mit Standardeinstellungen generiert, die für Sie zu dem virtuellen Computer hinzugefügt wird. Über das Azure-Portal ist es nicht möglich, vorhandene Netzwerkschnittstellen an einen neuen virtuellen Computer anzufügen oder einen virtuellen Computer mit mehreren Netzwerkschnittstellen zu erstellen. Verwenden Sie hierfür die CLI oder PowerShell. Sie können einer VM beliebig viele Netzwerkschnittstellen entsprechend der von der erstellten VM-Größe unterstützten Anzahl hinzufügen. Weitere Informationen zur unterstützten Anzahl an Netzwerkschnittstellen der einzelnen VM-Größen finden Sie im entsprechenden Artikel für [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Die Netzwerkschnittstellen, die Sie zu einem virtuellen Computer hinzufügen, können derzeit zu keinem anderen virtuellen Computer hinzugefügt werden. Weitere Informationen zum Erstellen von Netzwerkschnittstellen finden Sie im Artikel [Erstellen, Ändern oder Löschen von Netzwerkschnittstellen](virtual-network-network-interface.md#create-a-network-interface).

> [!WARNING]
> Wenn einer Netzwerkschnittstelle eine private IPv6-Adresse zugewiesen ist, können Sie diese Netzwerkschnittstelle nur dem virtuellen Computer hinzufügen, wenn Sie diesen erstellen. Beim Erstellen des virtuellen Computer oder nach dem Erstellen des virtuellen Computers können Sie nicht mehrere Netzwerkschnittstellen an den virtuellen Computer anfügen, solange eine IPv6-Adresse einer Netzwerkschnittstelle zugewiesen ist, die an einen virtuellen Computer angefügt ist. Weitere Informationen zum Zuweisen von IP-Adressen an Netzwerkschnittstellen finden Sie unter [IP-Adressen für Netzwerkschnittstellen](virtual-network-network-interface-addresses.md).

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az vm create](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Hinzufügen einer Netzwerkschnittstelle zu einem vorhandenen virtuellen Computer

Sie können einem virtuellen Computer so viele Netzwerkschnittstellen hinzufügen, wie entsprechend der jeweiligen Größe des virtuellen Computers unterstützt werden. Weitere Informationen zur unterstützten Anzahl an Netzwerkschnittstellen der einzelnen VM-Größen finden Sie im entsprechenden Artikel für [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Die VM, zu der Sie eine Netzwerkschnittstelle hinzufügen möchten, muss die Anzahl der Netzwerkschnittstellen unterstützen, die Sie hinzufügen möchten, und sich im Zustand „Beendet (Zuordnung aufgehoben)“ befinden. Die gewünschten Netzwerkschnittstellen können derzeit zu keinem anderen virtuellen Computer hinzugefügt werden. Netzwerkschnittstellen können zu einem vorhandenen virtuellen Computer nicht über das Azure-Portal hinzugefügt werden. Um Netzwerkschnittstellen einem vorhandenen virtuellen Computer hinzuzufügen, müssen Sie die CLI oder PowerShell verwenden. 

> [!WARNING]
> Eine Netzwerkschnittstelle verfügt über eine zugewiesene private IPv6-Adresse und kann nicht einem vorhandenen virtuellen Computer angefügt werden. Sie können nur eine Netzwerkschnittstelle mit einer zugewiesenen privaten IPv6-Adresse an einen virtuellen Computer anfügen, wenn Sie einen virtuellen Computer erstellen. Weitere Informationen zum Zuweisen von IP-Adressen an Netzwerkschnittstellen finden Sie unter [IP-Adressen für Netzwerkschnittstellen](virtual-network-network-interface-addresses.md).

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az vm nic add](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#add) (Verweis) or [detaillierte Schritte](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (Verweis) oder [detaillierte Schritte](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="vm-view-nic"></a>Anzeigen der Netzwerkschnittstellen für einen virtuellen Computer

Sie können die derzeit zu einem virtuellen Computer hinzugefügten Netzwerkschnittstellen anzeigen, um mehr über die Konfiguration der einzelnen Netzwerkschnittstellen und die diesen zugewiesenen IP-Adressen zu erfahren. 

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement die Rolle „Besitzer“, „Mitwirkender“ oder „Netzwerkmitwirkender“ zugewiesen ist, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen zu Konten finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Virtuelle Computer* ein. Wenn **Virtuelle Computer** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Virtuelle Computer** auf den Namen des virtuellen Computers, für den Sie die Netzwerkschnittstellen anzeigen möchten.
4. Klicken Sie auf dem Blatt „Virtueller Computer“, das für den ausgewählten virtuellen Computer angezeigt wird, im Abschnitt **EINSTELLUNGEN** auf **Netzwerkschnittstellen**. Informationen über Netzwerkschnittstellen finden Sie im Artikel [Erstellen, Ändern oder Löschen von Netzwerkschnittstellen](virtual-network-network-interface.md). Weitere Informationen zum Hinzufügen, Ändern und Entfernen von IP-Adressen, die einer Netzwerkschnittstelle zugewiesen sind, finden Sie im Artikel [Manage IP addresses (Verwalten von IP-Adressen)](virtual-network-network-interface-addresses.md).

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az vm show](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a> Entfernen einer Netzwerkschnittstelle von einem virtuellen Computer

Der virtuelle Computer, von dem Sie eine Netzwerkschnittstelle entfernen (trennen) möchten, muss sich im Zustand „Beendet (Zuordnung aufgehoben)“ befinden und über mindestens zwei angefügte Netzwerkschnittstellen verfügen. Sie können jede beliebige Netzwerkschnittstelle entfernen, dem virtuellen Computer muss jedoch immer mindestens eine Netzwerkschnittstelle hinzugefügt sein. Wenn Sie eine primäre Netzwerkschnittstelle entfernen, weist Azure das Attribut „Primär“ derjenigen Netzwerkschnittstelle zu, die am längsten zu dem virtuellen Computer hinzugefügt ist. 

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement die Rolle „Besitzer“, „Mitwirkender“ oder „Netzwerkmitwirkender“ zugewiesen ist, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen zu Konten finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Virtuelle Computer* ein. Wenn **Virtuelle Computer** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem daraufhin angezeigten Blatt **Virtuelle Computer** auf den Namen des virtuellen Computers, von dem Sie eine Netzwerkschnittstelle entfernen möchten.
4. Klicken Sie auf dem Blatt „Virtueller Computer“, das für den ausgewählten virtuellen Computer angezeigt wird, im Abschnitt **EINSTELLUNGEN** auf **Netzwerkschnittstellen**. Informationen über Netzwerkschnittstellen finden Sie im Artikel [Erstellen, Ändern oder Löschen von Netzwerkschnittstellen](virtual-network-network-interface.md). Weitere Informationen zum Hinzufügen, Ändern und Entfernen von IP-Adressen, die einer Netzwerkschnittstelle zugewiesen sind, finden Sie im Artikel [Manage IP addresses (Verwalten von IP-Adressen)](virtual-network-network-interface-addresses.md).
5. Klicken Sie auf dem angezeigten Netzwerkschnittstellenblatt rechts neben der Netzwerkschnittstelle, die Sie trennen möchten, auf die Auslassungspunkte (**...**).
6. Klicken Sie auf **Trennen**. Falls an den virtuellen Computer nur eine einzelne Netzwerkschnittstelle angefügt ist, steht die Option **Trennen** nicht zur Verfügung. Klicken Sie im angezeigten Bestätigungsfeld auf **Ja** .

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az vm nic remove](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#remove) (Verweis) or [detaillierte Schritte](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Remove-AzureRMVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (Verweis) oder [detaillierte Schritte](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Sie virtuelle Computer mit mehreren Netzwerkschnittstellen oder IP-Adressen erstellen:

**Befehle**

|Task|Tool|
|---|---|
|Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellenkarten (NICs)|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Erstellen eines virtuellen Computers mit einer NIC und mehreren IPv4-Adressen|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Erstellen eines virtuellen Computers mit einer NIC und einer IPv6-Adresse (mit Azure Load Balancer)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-Vorlage](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|

