---
title: "Erstellen, Ändern und Löschen von Subnetzen virtueller Azure-Netzwerke | Microsoft-Dokumentation"
description: "Informationen zum Erstellen, Ändern oder Löschen von Subnetzen virtueller Netzwerke."
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
ms.date: 05/10/2017
ms.author: jdial
ms.translationtype: Human Translation
ms.sourcegitcommit: 17c4dc6a72328b613f31407aff8b6c9eacd70d9a
ms.openlocfilehash: 38dcdf2f313c236a507e6a418c39812da16c6345
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="create-change-or-delete-virtual-network-subnets"></a>Erstellen, Ändern oder Löschen von Subnetzen virtueller Netzwerke

Informationen zum Erstellen, Ändern oder Löschen von Subnetzen virtueller Netzwerke (VNets). Wenn Sie nicht mit VNets vertraut sind, empfiehlt sich vor dem Erstellen, Ändern oder Löschen von Subnetzen die Lektüre der Artikel [Virtuelles Azure-Netzwerk](virtual-networks-overview.md) und [Erstellen, Ändern und Löschen virtueller Netzwerke](virtual-network-manage-network.md). Azure-Ressourcen, die mit einem VNet eine Verbindung herstellen können, werden mit einem Subnetz in einem VNet verbunden. In einem VNet werden in der Regel mehrere Subnetze für folgende Zwecke erstellt:
- **Filtern von Datenverkehr zwischen Subnetzen:** Netzwerksicherheitsgruppen (NSG) können auf Subnetze zum Filtern des ein- und ausgehenden Netzwerkdatenverkehrs für alle Ressourcen (z.B. VMs) angewendet werden, die mit dem VNet verbunden sind. Weitere Informationen zum Erstellen von NSGs finden Sie im Artikel [Erstellen von Netzwerksicherheitsgruppen](virtual-networks-create-nsg-arm-pportal.md).
- **Steuern des Routings zwischen Subnetzen:** Azure erstellt Standardrouten, damit Datenverkehr automatisch zwischen Subnetzen weitergeleitet wird. Sie können die Azure-Standardrouten überschreiben, indem Sie benutzerdefinierte Routen erstellen. Weitere Informationen zu benutzerdefinierten Routen finden Sie im Artikel [Erstellen von benutzerdefinierten Routen](virtual-network-create-udr-arm-ps.md). 

In diesem Artikel wird beschrieben, wie Sie für über das Azure Resource Manager-Bereitstellungsmodell erstellte VNets Subnetze erstellen, ändern und löschen.
 
## <a name="before"></a>Voraussetzungen

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Falls Sie noch nicht mit VNets und Subnetzen in Azure vertraut sind, empfiehlt es sich, vor der Lektüre dieses Artikels zunächst die Übung unter [Erstellen Ihres ersten virtuellen Netzwerks](virtual-network-get-started-vnet-subnet.md) zu absolvieren. In dieser Übung werden Sie an VNets und Subnetze herangeführt.
- Lesen Sie den Artikel zu [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits), um mehr über Einschränkungen von Subnetzen und VNets zu erfahren.
- Melden Sie sich mit einem Azure-Konto beim Azure-Portal, der Azure-Befehlszeilenschnittstelle (CLI) oder bei Azure PowerShell an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Wenn Sie Azure PowerShell-Befehle verwenden, um Aufgaben in diesem Artikel auszuführen, müssen zuerst [Azure PowerShell installieren und konfigurieren](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure PowerShell-Cmdlets installiert haben. Hilfe und Beispiele für PowerShell-Befehle erhalten Sie durch Eingabe von `get-help <command> -full`.
- Wenn Sie Befehle der Azure-Befehlszeilenschnittstelle (CLI) verwenden, um Aufgaben in diesem Artikel auszuführen, müssen zuerst die [Azure CLI installieren und konfigurieren](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure CLI installiert haben. Hilfe zu den Befehlen der Befehlszeilenschnittstelle erhalten Sie durch Eingabe von `az <command> --help`.


## <a name="create-subnet"></a>Erstellen eines Subnetzes

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie oben im Azure-Portal im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Virtuelle Netzwerke* ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem eingeblendeten Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, dem Sie ein Subnetz hinzufügen möchten.
4. Klicken Sie im Bereich, der für das von Ihnen gewählte virtuelle Netzwerk angezeigt wird, auf **Subnetze**.
5. Klicken Sie auf **+ Subnetz**.
6. Geben Sie auf dem Blatt **Subnetz hinzufügen** Werte für die folgenden Parameter ein:
    - **Name:** Der Name muss innerhalb des virtuellen Netzwerks eindeutig sein.
    - **Adressbereich:** Der Bereich muss innerhalb des Adressraums für das VNet eindeutig sein und darf sich nicht mit anderen Subnetzadressbereichen innerhalb des virtuellen Netzwerks überlappen. Der Adressraum muss in der CIDR-Notation angegeben werden. In einem VNet mit dem Adressraum 10.0.0.0/16 können Sie beispielsweise für ein Subnetz den Adressraum 10.0.0.0/24 definieren. Der kleinste Bereich, den Sie angeben können, ist /29, der acht IP-Adressen für das Subnetz bereitstellt. Azure reserviert die erste und letzte Adresse in jedem Subnetz aus Gründen der Protokollkonformität. Drei weitere Adressen sind für die Nutzung durch Azure-Dienste reserviert. Demzufolge führt das Einrichten eines Subnetzes mit dem Adressbereich /29 zu drei nutzbaren IP-Adressen im Subnetz. Wenn Sie vorhaben, ein VNet mit einem VPN-Gateway zu verbinden, muss ein Gatewaysubnetz erstellt werden. Weitere Informationen finden Sie unter [Informationen zu VPN Gateway-Einstellungen](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#a-namegwsubagateway-subnet) im Abschnitt „Gatewaysubnetz“. Unter bestimmten Bedingungen können Sie den Adressbereich ändern, nachdem das Subnetz erstellt wurde. Informationen zum Ändern des Adressbereichs eines Subnetzes finden Sie im Abschnitt [Ändern des Subnetzes](#change-subnet) dieses Artikels.
    - **Netzwerksicherheitsgruppe (NSG):** Optional können Sie eine vorhandene NSG dem Subnetz zum Steuern der Filterung von ein- und ausgehendem Netzwerkdatenverkehr für das Subnetz zuordnen. Die NSG muss zum selben Abonnement und Standort wie das VNet gehören und über das Ressourcen-Manager-Bereitstellungsmodell bereitgestellt worden sein. Weitere Informationen zum Erstellen von NSGs finden Sie im Artikel [Erstellen von Netzwerksicherheitsgruppen](virtual-networks-create-nsg-arm-pportal.md).
    - **Routingtabelle:** Sie können dem Subnetz optional eine vorhandene Routingtabelle zur Steuerung der Weiterleitung von Netzwerkdatenverkehr zu anderen Netzwerken zuordnen. Die Routingtabelle muss zum selben Abonnement und Standort wie das VNet gehören und über das Ressourcen-Manager-Bereitstellungsmodell bereitgestellt worden sein. Weitere Informationen zum Erstellen von Routingtabellen finden Sie im Artikel [Benutzerdefinierte Routen und IP-Weiterleitung](virtual-network-create-udr-arm-ps.md).
    - **Benutzer**: Sie können den Zugriff auf das Subnetz über integrierte Rollen oder eigene benutzerdefinierte Rollen steuern. Weitere Informationen zum Zuweisen von Rollen und Benutzern für den Zugriff das Subnetz finden Sie im Artikel [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen](../active-directory/role-based-access-control-configure.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-access).
7. Klicken Sie auf die Schaltfläche **OK**, um das Subnetz dem ausgewählten VNet hinzuzufügen.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network vnet subnet create](/cli/azure/network/vnet/subnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?view=azurermps-3.8.0?toc=%2fazure%2fvirtual-network%2ftoc.json), [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig?view=azurermps-3.8.0?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-subnet"></a>Ändern von Subnetzeinstellungen

Sie können die NSG, Routingtabellen und den Benutzerzugriff auf das Subnetz mithilfe von Ressourcen ändern, die mit einem Subnetz verbunden sind. Weitere Informationen zu diesen Einstellungen finden Sie in Schritt 6 [Erstellen eines Subnetzes](#create-subnet) in diesem Artikel. Wenn der Adressraum eines Subnetzes geändert werden soll, dürfen keine Ressourcen mit dem Subnetz verbunden sein. Falls Ressourcen mit dem Subnetz verbunden sind, müssen Sie diese Ressourcen erst löschen, ehe der Adressbereich geändert werden kann. Die Anweisungen zum Löschen einer Ressource variieren abhängig von der Ressource. Informationen zum Löschen von Ressourcen, die mit Subnetzen verbunden sind, finden Sie in der Dokumentation zu den einzelnen Ressourcentypen, die Sie löschen möchten. Führen Sie die folgenden Schritte aus, um den Adressbereich für ein Subnetz zu ändern:

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie oben im Portal im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Virtuelle Netzwerke* ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem eingeblendeten Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, für dessen Subnetz Sie den Adressbereich ändern möchten.
4. Klicken Sie auf das Subnetz, dessen Adressbereich geändert werden soll.
5. Geben Sie auf dem für das Subnetz eingeblendeten Blatt den neuen Adressbereich in das Feld **Adressbereich** ein. Der Bereich muss innerhalb des Adressraums für das VNet eindeutig sein und darf sich nicht mit anderen Subnetzadressbereichen innerhalb des virtuellen Netzwerks überlappen. Der Adressraum muss in der CIDR-Notation angegeben werden. In einem VNet mit dem Adressraum 10.0.0.0/16 können Sie beispielsweise für ein Subnetz den Adressraum 10.0.0.0/24 definieren. Der kleinste Bereich, den Sie angeben können, ist /29, der acht IP-Adressen für das Subnetz bereitstellt. Azure reserviert die erste und letzte Adresse in jedem Subnetz aus Gründen der Protokollkonformität. Drei weitere Adressen sind für die Nutzung durch Azure-Dienste reserviert. Als Ergebnis ist ein Subnetz mit einem Adressbereich des Typs „/29“ mit drei verwendbaren IP-Adressen verfügbar. Wenn Sie vorhaben, ein VNet mit einem VPN-Gateway zu verbinden, muss ein Gatewaysubnetz erstellt werden. Weitere Informationen finden Sie unter [Informationen zu VPN Gateway-Einstellungen](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#a-namegwsubagateway-subnet) im Abschnitt „Gatewaysubnetz“. Unter bestimmten Bedingungen können Sie den Adressbereich ändern, nachdem das Subnetz erstellt wurde. Informationen zum Ändern des Adressbereichs eines Subnetzes finden Sie im Abschnitt [Ändern des Subnetzes](#change-subnet) dieses Artikels.
6. Klicken Sie auf **Speichern**.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network vnet subnet update](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig?view=azurermps-3.8.0?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-subnet"></a>Löschen eines Subnetzes

Sie können ein Subnetz erst löschen, sobald keine Ressourcen mehr damit verbunden sind. Falls Ressourcen mit dem Subnetz verbunden sind, müssen Sie diese Ressourcen zunächst löschen. Die Anweisungen zum Löschen einer Ressource variieren abhängig von der Ressource. Informationen zum Löschen von Ressourcen, die mit Subnetzen verbunden sind, finden Sie in der Dokumentation zu den einzelnen Ressourcentypen, die Sie löschen möchten.

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie oben im Azure-Portal im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Virtuelle Netzwerke* ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem eingeblendeten Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, aus dem Sie ein Subnetz löschen möchten.
4. Klicken Sie auf dem Blatt, das für das ausgewählte VNet angezeigt wird, unter **Einstellungen** auf **Subnetze**.
5. Klicken Sie auf dem eingeblendeten Blatt „Subnetze“ in der Liste der Subnetze mit der rechten Maustaste auf das Subnetz, das Sie löschen möchten. Klicken Sie dann erst auf **Löschen** und danach auf **Ja**, um das Subnetz zu löschen.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network vnet delete](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?view=azurermps-3.8.0?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Nächste Schritte

Informationen dazu, wie Sie eine VM erstellen und mit einem Subnetz verbinden, finden Sie im Artikel [Erstellen Ihres ersten virtuellen Netzwerks](virtual-network-get-started-vnet-subnet.md#a-namecreate-vmsacreate-virtual-machines).
