---
title: "Erstellen, Ändern und Löschen virtueller Azure-Netzwerke | Microsoft-Dokumentation"
description: "Informationen zum Erstellen, Ändern oder Löschen virtueller Netzwerke."
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
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: b577891afc52013b712634dd51e7e28efcfc4482
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="create-change-or-delete-virtual-networks"></a>Erstellen, Ändern oder Löschen virtueller Netzwerke

Erfahren Sie, wie Sie virtuelle Netzwerke (VNets) erstellen und löschen und wie Sie Einstellungen für vorhandene VNets wie DNS-Server und IP-Adressräume ändern. Ein VNet ist eine Darstellung Ihres eigenen Netzwerks in der Cloud. Bei einem VNet handelt es sich um eine logische Isolation von der dedizierten Azure-Cloud für Ihr Abonnement. Für jedes VNet ist Folgendes möglich:
- Wählen Sie den zuzuweisenden Adressraum aus. Ein Adressraum besteht aus mindestens einem Adressbereich, der mithilfe der CIDR-Notation definiert wird, z.B. 10.0.0.0/16.
- Wählen Sie für jedes VNet entweder den von Azure bereitgestellten DNS-Server oder Ihren eigenen. Alle Ressourcen, die mit dem VNet verbunden sind, werden diesem DNS-Server zum Auflösen von Namen innerhalb des VNet zugewiesen.
- Unterteilen Sie das VNet in Subnetze, die im Adressraum des VNet jeweils ihren eigenen Adressbereich haben. Informationen zum Erstellen, Ändern und Löschen von Subnetzen finden Sie im Artikel [Hinzufügen, Ändern und Löschen von Subnetzen](virtual-network-manage-subnet.md).

In diesem Artikel wird beschrieben, wie Sie über das Azure Resource Manager-Bereitstellungsmodell erstellte VNets erstellen, ändern und löschen.
 
## <a name="before"></a>Voraussetzungen

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Falls Sie noch nicht mit VNets vertraut sind, empfiehlt es sich, vor der Lektüre dieses Artikels zunächst die Übung unter [Erstellen Ihres ersten virtuellen Netzwerks](virtual-network-get-started-vnet-subnet.md) zu absolvieren. In dieser Übung werden Sie an VNets herangeführt.
- Lesen Sie den Artikel zu [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits), um mehr über Einschränkungen von VNets zu erfahren.
- Melden Sie sich mit einem Azure-Konto beim Azure-Portal, der Azure-Befehlszeilenschnittstelle (CLI) oder bei Azure PowerShell an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Wenn Sie Azure PowerShell-Befehle verwenden, um Aufgaben in diesem Artikel auszuführen, müssen zuerst [Azure PowerShell installieren und konfigurieren](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure PowerShell-Cmdlets installiert haben. Hilfe und Beispiele für PowerShell-Befehle erhalten Sie durch Eingabe von `get-help <command> -full`.
- Wenn Sie Befehle der Azure-Befehlszeilenschnittstelle (CLI) verwenden, um Aufgaben in diesem Artikel auszuführen, müssen zuerst die [Azure CLI installieren und konfigurieren](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure CLI installiert haben. Hilfe zu den Befehlen der Befehlszeilenschnittstelle erhalten Sie durch Eingabe von `az <command> --help`.


## <a name="create-vnet"></a>Erstellen eines virtuellen Netzwerks

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Klicken Sie im Azure-Portal auf **+ Neu**. Klicken Sie auf dem eingeblendeten Blatt **Neu** auf **Netzwerk**. Klicken Sie auf dem eingeblendeten Blatt **Netzwerk** auf **Virtuelles Netzwerk**.
3. Lassen Sie auf dem eingeblendeten Blatt **Virtuelles Netzwerk** unter **Bereitstellungsmodell auswählen** die Option *Resource Manager* ausgewählt, und klicken Sie auf **Erstellen**.
4. Das Blatt **Virtuelles Netzwerk erstellen** wird angezeigt. Geben Sie hier Werte für die folgenden Einstellungen an, und klicken Sie anschließend auf **Erstellen**:
    - **Name**: Der Name muss in der ausgewählten [Ressourcengruppe](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), in der Sie das VNet erstellen möchten, eindeutig sein. Nach Erstellung des VNet kann der Name nicht mehr geändert werden. Sie können mit der Zeit mehrere VNets erstellen. Lesen Sie den Artikel [Benennungskonventionen](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions), in dem Sie Benennungsvorschläge zum Vereinfachen der Verwaltung mehrerer VNets finden.
    - **Adressraum**: Geben Sie diesen in der CIDR-Notation an. Der von Ihnen definierte Adressraum kann öffentlich oder privat (RFC 1918) sein. Unabhängig davon, ob Sie einen öffentlichen oder privaten Adressraum definieren, ist der Adressraum nur innerhalb des VNet, miteinander verbundener VNets und lokaler Netzwerke erreichbar, die Sie mit dem VNet verbunden haben. Sie können die folgenden Adressräume nicht hinzufügen:
        - 224.0.0.0/4 (Multicast)
        - 255.255.255.255/32 (Übertragung)
        - 127.0.0.0/8 (Loopback)
        - 169.254.0.0/16 (verbindungslokal)
        - 168.63.129.16/32 (Internes DNS)
    
      Wenngleich Sie bei der Erstellung des virtuellen Netzwerks nur einen Adressraum definieren können, können Sie nach seiner Erstellung zusätzliche Adressräume hinzufügen. Die erforderlichen Schritte werden im Abschnitt [Hinzufügen und Entfernen von Adressräumen](#add-address-spaces) in diesem Artikel erläutert.

      >[!WARNING]
      >Wenn sich der Adressraum eines VNet mit dem eines anderen virtuellen oder lokalen Netzwerks überlappt, können sie nicht verbunden werden. Berücksichtigen Sie andere virtuelle oder lokale Netzwerke, die Sie künftig ggf. mit dem VNet verbinden möchten, ehe Sie den Adressraum festlegen.
      >
      >
    
    - **Subnetzname:** Der Name muss innerhalb des VNet eindeutig sein. Nach Erstellung des Subnetzes kann der Name nicht mehr geändert werden. Das Portal fordert an, dass Sie ein Subnetz definieren, wenn Sie ein VNet erstellen, obwohl ein VNet nicht unbedingt Subnetze haben muss. Das Portal lässt beim Erstellen des VNet nur das Festlegen eines Subnetzes zu. Im Anschluss können Sie dem VNet allerdings weitere Subnetze hinzufügen. Informationen zum Hinzufügen eines Subnetzes zu einem VNet finden Sie im Abschnitt [Erstellen eines Subnetzes](virtual-network-manage-subnet.md#create-subnet) des Artikels [Erstellen, Ändern und Löschen von Subnetzen](virtual-network-manage-subnet.md). Sie können über die CLI oder PowerShell ein VNet mit mehreren Subnetzen erstellen.

      >[!TIP]
      >Unterschiedliche Subnetze werden häufig erstellt, um die Weiterleitung von Datenverkehr zwischen ihnen zu filtern oder zu steuern. Erwägen Sie vor dem Definieren von Subnetzen, wie Sie Datenverkehr zwischen Subnetzen filtern und weiterleiten möchten. Weitere Informationen zum Filtern von Datenverkehr zwischen Subnetzen finden Sie im Artikel [Steuern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](virtual-networks-nsg.md). Azure leitet Datenverkehr zwischen Subnetzen automatisch weiter, doch Sie können diese Standardrouten von Azure überschreiben. Wie das geht, erfahren Sie im Artikel [Benutzerdefinierte Routen und IP-Weiterleitung](virtual-networks-udr-overview.md).
      >

    - **Subnetzadressbereich:** Dieser muss sich im **Adressraum** befinden, den Sie für das VNet eingegeben haben. Der kleinste Bereich, den Sie angeben können, ist /29, der acht IP-Adressen für das Subnetz bereitstellt. Azure reserviert die erste und letzte Adresse in jedem Subnetz aus Gründen der Protokollkonformität. Drei weitere Adressen sind für die Nutzung durch Azure-Dienste reserviert. Als Ergebnis hat ein VNet mit dem Subnetzadressbereich „/29“ drei verwendbare IP-Adressen. Wenn Sie vorhaben, ein VNet mit einem VPN-Gateway zu verbinden, muss ein Gatewaysubnetz erstellt werden. Weitere Informationen finden Sie unter [Informationen zu VPN Gateway-Einstellungen](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#a-namegwsubagateway-subnet) im Abschnitt „Gatewaysubnetz“. Unter bestimmten Bedingungen können Sie den Adressbereich ändern, nachdem das Subnetz erstellt wurde. Informationen zum Ändern des Adressbereichs eines Subnetzes finden Sie im Abschnitt [Ändern des Subnetzes](#change-subnet) im Artikel [Hinzufügen, Ändern und Löschen von Subnetzen](virtual-network-manage-subnet.md).
    - **Abonnement**: Wählen Sie ein [Abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) aus. Ein VNet kann sich nicht über mehrere Abonnements erstrecken. Es kann jedoch mithilfe eines Azure VPN Gateways oder VNet-Peerings mit VNets in anderen Abonnements verbunden werden. Azure-Ressourcen, die Sie mit dem VNet verbinden, müssen zum gleichen Abonnement gehören.
    - **Ressourcengruppe**: Wählen Sie eine vorhandene [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) aus, oder erstellen Sie eine neue Ressourcengruppe. Azure-Ressourcen, die Sie mit dem VNet verbinden, können in der gleichen oder verschiedenen Ressourcengruppen vorhanden sein.
    - **Standort:** Wählen Sie einen Azure-[Standort](https://azure.microsoft.com/regions/) (auch als Region bezeichnet). Ein VNet kann sich nicht über mehrere Azure-Standorte erstrecken. Sie können jedoch über ein Azure VPN Gateway ein VNet an einem Standort mit einem VNet an einem anderen Standort verbinden. Azure-Ressourcen, die Sie mit dem VNet verbinden, müssen zum gleichen Standort gehören.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network vnet create](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name = "view-vnet"></a>Anzeigen von virtuellen Netzwerken und Einstellungen

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie oben im Portal im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Virtuelle Netzwerke* ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem eingeblendeten Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, dessen Einstellungen Sie anzeigen möchten.
4. Auf dem Blatt, das für das ausgewählte VNet angezeigt wird, stehen folgende Einstellungen zur Verfügung:
    - **Übersicht:** Enthält Informationen zum VNet, z.B. zum Adressraum und den DNS-Servern. Die folgende Abbildung zeigt die Übersicht über die Einstellungen eines VNet mit dem Namen **MyVNet**:
    
        ![Übersicht über die Netzwerkschnittstelle](./media/virtual-network-manage-network/vnet-overview.png)

      Sie können auf diesem Blatt ein VNet in ein anderes Abonnement oder eine andere Ressourcengruppe verschieben. Informationen zum Verschieben eines VNet finden Sie im Artikel [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Hier erfahren Sie, welche Voraussetzungen erfüllt sein müssen und wie Sie Ressourcen über das Azure-Portal, mithilfe von PowerShell oder unter Verwendung der Azure CLI verschieben. Zusammen mit dem VNet müssen auch alle damit verbundenen Ressourcen verschoben werden. 
    - **Adressraum:** Die dem VNet zugewiesenen Adressräume werden aufgelistet. Die erforderlichen Schritte zum Hinzufügen und Entfernen von Adressräumen werden im Abschnitt [Hinzufügen und Entfernen von Adressräumen](#address-spaces) in diesem Artikel erläutert.
    - **Verbundene Geräte:** Alle mit dem VNet verbundenen Ressourcen werden angezeigt. Das Beispiel in der vorherigen Abbildung zeigt drei Netzwerkschnittstellen und ein Lastenausgleichsmodul, die mit dem VNet verbunden sind. Neue Ressourcen, die Sie erstellen und mit dem VNet verbinden, werden aufgeführt. Wenn Sie eine mit dem VNet verbundene Ressource löschen, wird sie nicht mehr in der Liste angezeigt.
    - **Subnetze:** Eine Liste mit Subnetzen im VNet. Weitere Informationen zum Hinzufügen und Entfernen von Subnetzen finden Sie in den Abschnitten [Hinzufügen eines Subnetzes](virtual-network-manage-subnet.md#create-subnet) und [Löschen eines Subnetzes](virtual-network-manage-subnet.md#delete-subnet) im Artikel [Hinzufügen, Ändern oder Löschen von Subnetzen](virtual-network-manage-subnet.md).
    - **DNS-Server:** Sie können angeben, ob der interne DNS-Server von Azure oder benutzerdefinierte DNS-Server die Namensauflösung für Geräte übernimmt, die mit dem VNet verbunden sind. Wenn Sie ein VNet im Azure-Portal erstellen, werden standardmäßig die DNS-Server von Azure für die Namensauflösung in einem VNet verwendet. Führen Sie zum Ändern der DNS-Server die Schritte im Abschnitt [Ändern, Hinzufügen und Entfernen von DNS-Servern](#dns-servers) dieses Artikels aus. 
    - **Peerings:** Wenn Peerings im Abonnement vorhanden sind, werden sie hier aufgeführt. Sie können die Einstellungen für vorhandene Peerings anzeigen oder Peerings erstellen, ändern oder löschen. Weitere Informationen zu Peerings finden Sie im Artikel mit der [Übersicht über das Peering in virtuellen Netzwerken](virtual-network-peering-overview.md).
    - **Eigenschaften:** Zeigt Einstellungen des VNet, wie z.B. seine Ressourcen-ID und das zugehörige Abonnement.
    - **Diagramm:** Das Diagramm bietet eine grafische Darstellung aller mit dem VNet verbundenen Geräte sowie wichtige Informationen zu den Geräten. Sie können auf ein Gerät klicken, um es direkt in dieser Ansicht zu verwalten.
    - **Allgemeine Azure-Einstellungen:** Weitere Informationen zu allgemeinen Azure-Einstellungen finden Sie in den Artikeln zu [Aktivitätsprotokollen](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs), [Zugriffssteuerung](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control), [Tags](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags), [Sperren](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json) und [Automatisierungsskripts](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group).

**Befehle**

|**Tool**|**Befehl**|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network vnet show](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVirtualNetwork](/powershell/resourcemanager/azurerm.network/v3.8.0/get-azurermvirtualnetwork/?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="address-spaces"></a>Hinzufügen und Entfernen von Adressräumen

Sie können einem VNet Adressräume hinzufügen und diese entfernen. Die Adressräume müssen in der CIDR-Notation angegeben werden und dürfen sich nicht innerhalb des gleichen VNet überlappen. Die von Ihnen definierten Adressräume können öffentlich oder privat (RFC 1918) sein. Unabhängig davon, ob Sie öffentliche oder private Adressräume definieren, sind diese Adressräume nur innerhalb des VNet, miteinander verbundener VNets und lokaler Netzwerke erreichbar, die Sie mit dem VNet verbunden haben. Sie können die folgenden Adressräume nicht hinzufügen:
    - 224.0.0.0/4 (Multicast)
    - 255.255.255.255/32 (Übertragung)
    - 127.0.0.0/8 (Loopback)
    - 169.254.0.0/16 (verbindungslokal)
    - 168.63.129.16/32 (Internes DNS)

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie oben im Azure-Portal im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Virtuelle Netzwerke* ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem eingeblendeten Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, dem Sie einen Adressraum hinzufügen bzw. aus dem Sie einen Adressraum entfernen möchten.
4. Klicken Sie auf dem Blatt, das für das ausgewählte VNet angezeigt wird, im Abschnitt **EINSTELLUNGEN** auf **Adressraum**.
5. Führen Sie eine der folgenden Aufgaben auf dem Blatt aus, das für Adressräume angezeigt wird:
    - **Adressraum hinzufügen:** Geben Sie den neuen Adressraum in das Feld ein. Der Adressraum darf sich nicht mit einem vorhandenen Adressraum überlappen, der für das VNet definiert ist.
    - **Adressraum entfernen:** Klicken Sie mit der rechten Maustaste auf einen Adressraum, und klicken Sie dann auf **Entfernen**. Wenn ein Subnetz im Adressraum vorhanden ist, können Sie den Adressraum nicht entfernen. Ehe Sie einen Adressraum entfernen können, müssen zunächst alle im Adressraum enthaltenen Subnetze (und mit diesen verbundene Ressourcen) gelöscht werden.
6. Klicken Sie auf **Speichern**.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|Nur Ressourcen-Manager|[az network vnet update](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?view=azurermps-3.8.0?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="dns-servers"></a>Hinzufügen, Ändern und Entfernen von DNS-Servern

Alle mit dem VNet verbundenen VMs werden bei den DNS-Servern registriert, die für das VNet angegeben sind, und nutzen den DNS-Server für die Namensauflösung. Jede Netzwerkschnittstellenkarte (NIC) einer VM hat eigene DNS-Servereinstellungen. Wenn eine NIC eigene DNS-Servereinstellungen hat, überschreiben diese die DNS-Servereinstellungen für das VNet. Weitere Informationen zu DNS-Einstellungen von NICs finden Sie im Artikel [Erstellen, Ändern oder Löschen von Netzwerkschnittstellenkarten](virtual-network-network-interface.md#dns). Weitere Informationen zur Namensauflösung für VMs und Cloud Services-Rolleninstanzen finden Sie im Artikel [Namensauflösung für virtuelle Computer und Rolleninstanzen](virtual-networks-name-resolution-for-vms-and-role-instances.md).

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie oben im Azure-Portal im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Virtuelle Netzwerke* ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, klicken Sie darauf. 
3. Klicken Sie auf dem eingeblendeten Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, dessen DNS-Einstellungen Sie ändern möchten.
4. Klicken Sie auf dem Blatt, das für das ausgewählte VNet angezeigt wird, im Abschnitt **EINSTELLUNGEN** auf **DNS-Server**.
5. Wählen Sie eine der folgenden Optionen auf dem Blatt aus, das für DNS-Server angezeigt wird:
    - **Standard (von Azure bereitgestellt):** Alle Ressourcennamen und privaten IP-Adressen werden automatisch bei den Azure-DNS-Servern registriert. Sie können Namen zwischen allen Ressource auflösen, die im gleichen VNet miteinander verbunden sind. Diese Option eignet sich nicht zum VNet-übergreifenden Auflösen von Namen. Hierfür müssen Sie einen benutzerdefinierten DNS-Server verwenden.
    - **Benutzerdefiniert:** Sie können einen oder mehrere Server (bis zum Azure-Grenzwert für ein VNet) hinzufügen. Weitere Informationen zu Einschränkungen für DNS-Server finden Sie im Artikel [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-networking-limits-classic). Folgende Optionen stehen zur Auswahl:
        - **Adresse hinzufügen:** Hierüber können Sie einen DNS-Server der Liste „DNS-Server“ Ihres virtuellen Netzwerks hinzufügen und den DNS-Server auch bei Azure registrieren. Wenn Sie zuvor einen DNS-Server bei Azure registriert haben, können Sie ihn in der angezeigten Liste auswählen. 
        - **Adresse entfernen:** Klicken Sie auf das **X** neben dem Server, den Sie entfernen möchten. Bei Löschen des Servers wird dieser nur aus dieser VNet-Liste entfernt. Der DNS-Server bleibt in Azure registriert und kann von Ihren anderen VNets verwendet werden. 
        - **Reihenfolge von DNS-Serveradressen ändern**: Beim Angeben von DNS-Servern müssen Sie darauf achten, dass Sie sie in der richtigen Reihenfolge für Ihre Umgebung auflisten. DNS-Serverlisten werden nicht per Roundrobin verarbeitet. Die DNS-Server werden in der Reihenfolge verwendet, in der sie angegeben sind. Wenn der erste DNS-Server in der Liste erreicht werden kann, verwendet der Client diesen DNS-Server unabhängig davon, ob der DNS-Server ordnungsgemäß funktioniert. Entfernen Sie alle aufgeführten DNS-Server, und fügen Sie sie in der gewünschten Reihenfolge wieder hinzu.
        - **Adresse ändern**: Markieren Sie den DNS-Server in der Liste, und geben Sie den neuen Namen ein.
6. Klicken Sie auf **Speichern**.
7. Starten Sie die mit dem VNet verbundenen VMs, damit ihnen die neuen DNS-Servereinstellungen zugewiesen werden. VMs nutzen ihre aktuellen DNS-Einstellungen weiter, bis sie neu gestartet werden.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network vnet update](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?view=azurermps-3.8.0?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="delete-vnet"></a>Löschen eines virtuellen Netzwerks

Sie können ein VNet erst löschen, sobald keine Ressourcen mehr damit verbunden sind. Falls Ressourcen mit den Subnetzen im VNet verbunden sind, müssen Sie diese Ressourcen zunächst löschen. Die Anweisungen zum Löschen einer Ressource variieren abhängig von der Ressource. Informationen zum Löschen von Ressourcen, die mit Subnetzen verbunden sind, finden Sie in der Dokumentation zu den einzelnen Ressourcentypen, die Sie löschen möchten. Führen Sie die folgenden Schritte durch, um ein VNet zu löschen:

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie oben im Azure-Portal im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Virtuelle Netzwerke* ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem eingeblendeten Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, das Sie löschen möchten.
4. Bestätigen Sie, dass keine Geräte mit dem VNet verbunden sind, indem Sie im Abschnitt **EINSTELLUNGEN** des Blatts, das für das ausgewählte VNet angezeigt wird, auf **Verbundene Geräte** klicken. Wenn verbundene Geräte vorhanden sind, müssen Sie diese zuerst löschen, ehe Sie das VNet löschen können.  Wenn keine verbundenen Geräte vorhanden sind, klicken Sie auf dem Blatt auf **Übersicht**.
5. Klicken Sie oben auf dem Blatt auf das Symbol **Löschen**. 
6. Klicken Sie auf **Ja**, um das Löschen des VNet zu bestätigen.


**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[azure network vnet delete](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmVirtualNetwork](/powershell/module/azurerm.network/remove-azurermvirtualnetwork?view=azurermps-3.8.0?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="next-steps"></a>Nächste Schritte

- Informationen dazu, wie Sie eine VM erstellen und mit einem VNet verbinden, finden Sie im Artikel [Erstellen Ihres ersten virtuellen Netzwerks](virtual-network-get-started-vnet-subnet.md#a-namecreate-vmsacreate-virtual-machines).
- Informationen dazu, wie Sie Netzwerkdatenverkehr zwischen Subnetzen in einem VNet filtern, finden Sie im Artikel [Erstellen von Netzwerksicherheitsgruppen](virtual-networks-create-nsg-arm-pportal.md).
- Informationen zum Erstellen eines Peerings zwischen zwei VNets finden Sie im Artikel [Erstellen eines VNet-Peerings](virtual-networks-create-vnetpeering-arm-portal.md#peering-vnets-in-the-same-subscription).
- Weitere Informationen zu Optionen zum Verbinden eines VNet mit einem lokalen Netzwerk finden Sie im Artikel [Informationen zu VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#a-namediagramsaconnection-topology-diagrams).

