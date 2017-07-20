---
title: "Erstellen, Ändern und Löschen von Subnetzen virtueller Azure-Netzwerke | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Subnetze virtueller Netzwerke in Azure erstellen, ändern oder löschen."
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
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: f3b8a5cc0c85e56e535871f1f7fcfd72bde65a03
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="create-change-or-delete-a-virtual-network-subnet"></a>Erstellen, Ändern oder Löschen von Subnetzen virtueller Netzwerke

Erfahren Sie, wie Sie Subnetze virtueller Netzwerke erstellen, ändern oder löschen. 

Wenn Sie mit virtuellen Netzwerken nicht vertraut sind, sollten Sie vor dem Erstellen, Ändern oder Löschen eines Subnetzes die Artikel [Übersicht über Azure Virtual Network](virtual-networks-overview.md) und [Erstellen, Ändern oder Löschen von virtuellen Netzwerken](virtual-network-manage-network.md) lesen. Alle Azure-Ressourcen, die eine Verbindung mit einem virtuellen Netzwerk herstellen können, sind in einem virtuellen Netzwerk mit einem Subnetz verbunden. In der Regel werden innerhalb eines virtuellen Netzwerks mehrere Subnetze erstellt:
- **Filtern von Datenverkehr zwischen Subnetzen**. Sie können Netzwerksicherheitsgruppen auf Subnetzen anwenden, um den ein- und ausgehenden Netzwerkdatenverkehr für alle Ressourcen, die mit dem virtuellen Netzwerk verbunden sind (z.B. virtuelle Computer), zu filtern. Weitere Informationen zum Erstellen von Netzwerksicherheitsgruppen finden Sie unter [Erstellen von Netzwerksicherheitsgruppen](virtual-networks-create-nsg-arm-pportal.md).
- **Steuern der Routings zwischen Subnetzen**. Azure erstellt Standardrouten, damit Datenverkehr automatisch zwischen Subnetzen weitergeleitet wird. Sie können die Azure-Standardrouten überschreiben, indem Sie benutzerdefinierte Routen erstellen. Weitere Informationen zu benutzerdefinierten Routen finden Sie unter [Benutzerdefinierte Routen](virtual-network-create-udr-arm-ps.md). 

In diesem Artikel wird beschrieben, wie Sie für über das Azure Resource Manager-Bereitstellungsmodell erstellte virtuelle Netzwerke Subnetze erstellen, ändern und löschen.
 
## <a name="before"></a>Voraussetzungen

Bevor Sie mit den Aufgaben, die in diesem Artikel beschrieben werden, beginnen, müssen die folgenden Voraussetzungen erfüllt sein:

- Wenn Sie noch nicht mit der Arbeit mit virtuellen Netzwerken vertraut sind, sollten Sie die Übung in [Erstellen Ihres ersten virtuellen Azure-Netzwerks](virtual-network-get-started-vnet-subnet.md) durcharbeiten. Diese Übung hilft Ihnen dabei, sich mit virtuellen Netzwerken vertraut zu machen.
- Weitere Informationen zu Grenzwerten für virtuelle Netzwerke finden Sie in [Azure-Grenzwerte](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Melden Sie sich mit Ihrem Azure-Konto beim Azure-Portal, der Azure-Befehlszeilenschnittstelle (Azure CLI) oder bei Azure PowerShell an. Falls Sie noch nicht über ein Azure-Konto verfügen, melden Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) an.
- Wenn Sie PowerShell-Befehle verwenden möchten, um die Aufgaben in diesem Artikel auszuführen, müssen Sie zunächst [Azure PowerShell installieren und konfigurieren](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie über die neueste Version der Azure PowerShell-Cmdlets verfügen. Hilfe zu den Beispielen für PowerShell-Befehle erhalten Sie durch Eingabe von `get-help <command> -full`.
- Wenn Sie Azure CLI-Befehle verwenden möchten, um die Aufgaben in diesem Artikel auszuführen, müssen Sie zunächst [Azure-Befehlszeilenschnittstelle installieren und konfigurieren](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie über die neueste Version der Azure-Befehlszeilenschnittstelle verfügen. Um Hilfe zu Azure CLI-Befehlen zu erhalten, geben Sie `az <command> --help` ein.

## <a name="create-subnet"></a>Erstellen eines Subnetzes

So erstellen Sie ein Subnetz

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im Portal in das Suchfeld **virtuelle Netzwerke** ein. Klicken Sie in den Suchergebnissen auf **Virtuelle Netzwerke**.
3. Klicken Sie auf dem Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, dem Sie ein Subnetz hinzufügen möchten.
4. Klicken Sie auf dem Blatt „Virtuelles Netzwerk“ auf **Subnetze**.
5. Klicken Sie auf **+ Subnetz**.
6. Geben Sie auf dem Blatt **Subnetz hinzufügen** Werte für die folgenden Parameter ein:
    - **Name:** Der Name muss innerhalb des virtuellen Netzwerks eindeutig sein.
    - **Adressbereich:** Der Bereich muss innerhalb des Adressraums für das virtuelle Netzwerk eindeutig sein. Der Bereich darf keine Überschneidungen mit anderen Subnetzadressbereichen innerhalb des virtuellen Netzwerks aufweisen. Der Adressraum muss mithilfe der CIDR-Notation (Classless Inter-Domain Routing, klassenloses domänenübergreifendes Routing) angegeben werden. In einem virtuellen Netzwerk mit dem Adressraum 10.0.0.0/16 können Sie beispielsweise für ein Subnetz den Adressraum 10.0.0.0/24 definieren. Der kleinste Bereich, den Sie angeben können, ist /29, der acht IP-Adressen für das Subnetz bereitstellt. Azure reserviert die erste und letzte Adresse in jedem Subnetz aus Gründen der Protokollkonformität. Drei weitere Adressen sind für die Nutzung durch Azure-Dienste reserviert. Demzufolge führt das Einrichten eines Subnetzes mit dem Adressbereich /29 zu drei nutzbaren IP-Adressen im Subnetz. Wenn Sie ein virtuelles Netzwerk mit einem VPN-Gateway verbinden möchten, müssen Sie ein Gatewaysubnetz erstellen. Weitere Informationen finden Sie unter [Informationen zu VPN Gateway-Einstellungen](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#a-namegwsubagateway-subnet) im Abschnitt „Gatewaysubnetz“. Unter bestimmten Bedingungen können Sie den Adressbereich ändern, nachdem das Subnetz erstellt wurde. Informationen zum Ändern des Adressbereichs eines Subnetzes finden Sie im Abschnitt [Ändern der Subnetzeinstellungen](#change-subnet) dieses Artikels.
    - **Netzwerksicherheitsgruppe:** Optional können Sie dem Subnetz eine vorhandene Netzwerksicherheitsgruppe zum Steuern der Filterung von ein- und ausgehendem Netzwerkdatenverkehr für das Subnetz zuordnen. Die Netzwerksicherheitsgruppe muss demselben Abonnement und Standort wie das virtuelle Netzwerk angehören. Sie muss darüber hinaus mit dem Resource Manager-Bereitstellungsmodell erstellt worden sein. Weitere Informationen zum Erstellen von Netzwerksicherheitsgruppen finden Sie unter [Netzwerksicherheitsgruppen](virtual-networks-create-nsg-arm-pportal.md).
    - **Routingtabelle:** Optional können Sie dem Subnetz eine vorhandene Routingtabelle zur Steuerung der Weiterleitung von Netzwerkdatenverkehr zu anderen Netzwerken zuordnen. Die Routingtabelle muss demselben Abonnement und Standort wie das virtuelle Netzwerk angehören. Sie muss darüber hinaus mit dem Resource Manager-Bereitstellungsmodell erstellt worden sein. Weitere Informationen zum Erstellen von Routingtabellen finden Sie unter [Benutzerdefinierte Routen](virtual-network-create-udr-arm-ps.md).
    - **Benutzer:** Sie können den Zugriff auf das Subnetz über integrierte Rollen oder eigene benutzerdefinierte Rollen steuern. Weitere Informationen zum Zuweisen von Rollen und Benutzern für den Zugriff auf das Subnetz finden Sie unter [Verwenden von Rollenzuweisungen zum Verwalten des Zugriffs auf Ihre Azure-Ressourcen](../active-directory/role-based-access-control-configure.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-access).
7. Um das Subnetz dem ausgewählten virtuellen Netzwerk hinzuzufügen, klicken Sie auf **OK**.

**Befehle**

|Tool|Befehl|
|---|---|
|Azure-Befehlszeilenschnittstelle|[az network vnet subnet create](/cli/azure/network/vnet/subnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?view=azurermps-3.8.0?toc=%2fazure%2fvirtual-network%2ftoc.json), [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig?view=azurermps-3.8.0?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-subnet"></a>Ändern von Subnetzeinstellungen

Sie ändern Netzwerksicherheitsgruppen, Routingtabellen und den Benutzerzugriff auf ein Subnetz über die Verwaltung von Ressourcen, die mit einem Subnetz verbunden sind. Informationen zu diesen Einstellungen finden Sie unter [Erstellen von Subnetzen](#create-subnet) in Schritt 6. Wenn Sie den Adressbereich eines Subnetzes ändern möchten, müssen Sie zunächst alle Ressourcen löschen, die mit dem Subnetz verbunden sind. Die Schritte zum Löschen einer Ressource variieren abhängig von der Ressource. Informationen zum Löschen von Ressourcen, die mit Subnetzen verbunden sind, finden Sie in der Dokumentation zum jeweiligen Ressourcentyp, den Sie löschen möchten. So ändern Sie den Adressbereich für ein Subnetz

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im Portal in das Suchfeld **virtuelle Netzwerke** ein. Klicken Sie in den Suchergebnissen auf **Virtuelle Netzwerke**.
3. Klicken Sie auf dem Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, für dessen Subnetz Sie den Adressbereich ändern möchten.
4. Klicken Sie auf das Subnetz, dessen Adressbereich geändert werden soll.
5. Geben Sie auf dem Blatt „Subnetz“ im Feld **Adressbereich** den neuen Adressbereich ein. Der Bereich muss innerhalb des Adressraums für das virtuelle Netzwerk eindeutig sein. Der Bereich darf keine Überschneidungen mit anderen Subnetzadressbereichen innerhalb des virtuellen Netzwerks aufweisen. Der Adressraum muss in der CIDR-Notation angegeben werden. In einem virtuellen Netzwerk mit dem Adressraum 10.0.0.0/16 können Sie beispielsweise für ein Subnetz den Adressraum 10.0.0.0/24 definieren. Der kleinste Bereich, den Sie angeben können, ist /29, der acht IP-Adressen für das Subnetz bereitstellt. Azure reserviert die erste und letzte Adresse in jedem Subnetz aus Gründen der Protokollkonformität. Drei weitere Adressen sind für die Nutzung durch Azure-Dienste reserviert. Als Ergebnis ist ein Subnetz mit einem Adressbereich des Typs „/29“ mit drei verwendbaren IP-Adressen verfügbar. Wenn Sie ein virtuelles Netzwerk mit einem VPN-Gateway verbinden möchten, müssen Sie ein Gatewaysubnetz erstellen. Weitere Informationen finden Sie unter [Informationen zu VPN Gateway-Einstellungen](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#a-namegwsubagateway-subnet) im Abschnitt „Gatewaysubnetz“. Unter bestimmten Bedingungen können Sie den Adressbereich ändern, nachdem das Subnetz erstellt wurde. Informationen zum Ändern des Adressbereichs eines Subnetzes finden Sie im Abschnitt [Ändern der Subnetzeinstellungen](#change-subnet) dieses Artikels.
6. Klicken Sie auf **Speichern**.

**Befehle**

|Tool|Befehl|
|---|---|
|Azure-Befehlszeilenschnittstelle|[az network vnet subnet update](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig?view=azurermps-3.8.0?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-subnet"></a>Löschen eines Subnetzes

Sie können ein Subnetz erst löschen, wenn keine Ressourcen mehr damit verbunden sind. Falls Ressourcen mit dem Subnetz verbunden sind, müssen Sie diese Ressourcen zunächst löschen, bevor das Subnetz gelöscht werden kann. Die Schritte zum Löschen einer Ressource variieren abhängig von der Ressource. Informationen zum Löschen von Ressourcen, die mit Subnetzen verbunden sind, finden Sie in der Dokumentation zum jeweiligen Ressourcentyp, den Sie löschen möchten. So löschen Sie ein Subnetz

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im Portal in das Suchfeld **virtuelle Netzwerke** ein. Klicken Sie in den Suchergebnissen auf **Virtuelle Netzwerke**.
3. Klicken Sie auf dem Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, aus dem Sie ein Subnetz löschen möchten.
4. Klicken Sie auf dem Blatt des virtuellen Netzwerks unter **Einstellungen** auf **Subnetze**.
5. Klicken Sie auf dem eingeblendeten Blatt „Subnetze“ in der Liste der Subnetze mit der rechten Maustaste auf das Subnetz, das Sie löschen möchten. Klicken Sie dann erst auf **Löschen** und danach auf **Ja**, um das Subnetz zu löschen.

**Befehle**

|Tool|Befehl|
|---|---|
|Azure-Befehlszeilenschnittstelle|[az network vnet delete](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?view=azurermps-3.8.0?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Nächste Schritte

Informationen dazu, wie Sie eine VM erstellen und mit einem Subnetz verbinden, finden Sie unter [Erstellen virtueller Netzwerke und Verbinden virtueller Computer](virtual-network-get-started-vnet-subnet.md#a-namecreate-vmsacreate-virtual-machines).
