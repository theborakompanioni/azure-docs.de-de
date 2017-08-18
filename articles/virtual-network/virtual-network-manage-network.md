---
title: "Erstellen, Ändern oder Löschen eines virtuellen Azure-Netzwerks | Microsoft-Dokumentation"
description: "In diesem Artikel erfahren Sie, wie Sie ein virtuelles Netzwerk in Azure erstellen, ändern oder löschen."
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
ms.translationtype: HT
ms.sourcegitcommit: b309108b4edaf5d1b198393aa44f55fc6aca231e
ms.openlocfilehash: 74aace2136136c25bc56327d38cfbab168265401
ms.contentlocale: de-de
ms.lasthandoff: 08/15/2017

---
# <a name="create-change-or-delete-a-virtual-network"></a>Erstellen, Ändern oder Löschen eines virtuellen Netzwerks

In diesem Artikel erfahren Sie, wie Sie ein virtuelles Netzwerk erstellen und löschen und Einstellungen für vorhandene virtuelle Netzwerke wie DNS-Server und IP-Adressräume ändern.

Ein virtuelles Netzwerk ist eine Darstellung Ihres eigenen Netzwerks in der Cloud. Bei einem virtuellen Netzwerk handelt es sich um eine logische Isolation der dedizierten Azure-Cloud für Ihr Azure-Abonnement. Für jedes virtuelle Netzwerk, das Sie erstellen, können Sie folgende Aktionen durchführen:
- Wählen Sie den zuzuweisenden Adressraum aus. Ein Adressraum besteht aus einem oder mehreren Adressbereichen, die mithilfe der CIDR-Notation (Classless Inter-Domain Routing) definiert sind, z.B. 10.0.0.0/16.
- Wählen Sie entweder den von Azure bereitgestellten DNS-Server, oder verwenden Sie Ihren eigenen. Alle Ressourcen, die mit dem virtuellen Netzwerk verbunden sind, werden diesem DNS-Server zum Auflösen der Namen innerhalb des virtuellen Netzwerks zugewiesen.
- Unterteilen Sie das virtuelle Netzwerk in Subnetze, die im Adressraum des virtuellen Netzwerks jeweils über einen eigenen Adressbereich verfügen. Informationen zum Erstellen, Ändern und Löschen von Subnetzen finden Sie unter [Hinzufügen, Ändern oder Löschen von Subnetzen](virtual-network-manage-subnet.md).

In diesem Artikel wird beschrieben, wie Sie über das Azure Resource Manager-Bereitstellungsmodell virtuelle Netzwerke erstellen, ändern und löschen.

## <a name="before"></a>Voraussetzungen

Bevor Sie mit den Aufgaben, die in diesem Artikel beschrieben werden, beginnen, müssen die folgenden Voraussetzungen erfüllt sein:

- Wenn Sie noch nicht mit der Arbeit mit virtuellen Netzwerken vertraut sind, sollten Sie die Übung unter [Erstellen Ihres ersten virtuellen Azure-Netzwerks](virtual-network-get-started-vnet-subnet.md) durchgehen. Diese Übung hilft Ihnen dabei, sich mit virtuellen Netzwerken vertraut zu machen.
- Weitere Informationen zu Grenzwerten für virtuelle Netzwerke finden Sie in [Azure-Grenzwerte](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Melden Sie sich mit Ihrem Azure-Konto beim Azure-Portal, der Azure-Befehlszeilenschnittstelle (Azure CLI) oder bei Azure PowerShell an. Falls Sie noch kein Azure-Konto besitzen, registrieren Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free).
- Wenn Sie PowerShell-Befehle verwenden möchten, um die Aufgaben in diesem Artikel auszuführen, müssen Sie zunächst [Azure PowerShell installieren und konfigurieren](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass die neueste Version der Azure PowerShell-Cmdlets installiert ist. Hilfe zu den Beispielen für PowerShell-Befehle erhalten Sie durch Eingabe von `get-help <command> -full`.
- Wenn Sie Azure CLI-Befehle verwenden möchten, um die Aufgaben in diesem Artikel auszuführen, müssen Sie zunächst [Azure-Befehlszeilenschnittstelle installieren und konfigurieren](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass die neueste Version der Azure CLI installiert ist. Um Hilfe zu Azure CLI-Befehlen zu erhalten, geben Sie `az <command> --help` ein.


## <a name="create-vnet"></a>Erstellen eines virtuellen Netzwerks

So erstellen Sie ein virtuelles Netzwerk:

1. Melden Sie sich beim [Portal](https://portal.azure.com) mit einem Konto an, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Klicken Sie auf **Neu** > **Netzwerk** > **Virtuelles Netzwerk**.
3. Lassen Sie auf dem Blatt **Virtuelles Netzwerk** unter **Bereitstellungsmodell auswählen** die Option **Resource Manager** aktiviert, und klicken Sie auf **Erstellen**.
4. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** die Werte für die folgenden Einstellungen ein, oder wählen Sie sie aus, und klicken Sie anschließend auf **Erstellen**:
    - **Name**: Der Name muss in der ausgewählten [Ressourcengruppe](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), in der Sie das virtuelle Netzwerk erstellen möchten, eindeutig sein. Sie können den Namen nicht ändern, nachdem das virtuelle Netzwerk erstellt wurde. Sie können im Laufe der Zeit mehrere virtuelle Netzwerke erstellen. Benennungsvorschläge finden Sie unter [Benennungskonventionen](/azure/architecture/best-practices/naming-conventions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions). Durch die Verwendung einer Benennungskonvention können Sie einfacher mehrere virtuelle Netzwerke verwalten.
    - **Adressraum**: Geben Sie den Adressraum in der CIDR-Notation an. Der von Ihnen definierte Adressraum kann öffentlich oder privat (RFC 1918) sein. Ganz gleich, ob Sie den Adressraum als öffentlich oder privat definieren – der Adressraum ist nur innerhalb des virtuellen Netzwerks, über miteinander verbundene virtuelle Netzwerke und über beliebige lokale Netzwerke erreichbar, die Sie mit dem virtuellen Netzwerk verbunden haben. Sie können die folgenden Adressräume nicht hinzufügen:
        - 224.0.0.0/4 (Multicast)
        - 255.255.255.255/32 (Übertragung)
        - 127.0.0.0/8 (Loopback)
        - 169.254.0.0/16 (verbindungslokal)
        - 168.63.129.16/32 (Internes DNS)

      Sie können beim Erstellen des virtuellen Netzwerks zwar nur einen Adressraum definieren, nach der Erstellung des virtuellen Netzwerks jedoch weitere Adressräume hinzufügen. Informationen zum Hinzufügen eines Adressraums zu einem vorhandenen virtuellen Netzwerk finden Sie unter [Hinzufügen oder Entfernen eines Adressraums](#add-address-spaces) in diesem Artikel.

      >[!WARNING]
      >Wenn ein virtuelles Netzwerk Adressräume aufweist, die sich mit denen eines anderen virtuellen Netzwerks oder eines lokalen Netzwerks überlappen, können die beiden Netzwerke nicht verbunden werden. Bevor Sie einen Adressraum definieren, sollten Sie das virtuelle Netzwerk zukünftig eventuell mit anderen virtuellen Netzwerken oder lokalen Netzwerken verbinden.
      >
      >

    - **Subnetzname**: Der Subnetzname muss innerhalb des virtuellen Netzwerks eindeutig sein. Sie können den Subnetznamen nicht ändern, nachdem das Subnetz erstellt wurde. Das Portal erfordert, dass Sie bei der Erstellung eines virtuellen Netzwerks ein Subnetz definieren, obwohl ein virtuelles Netzwerk nicht unbedingt Subnetze umfassen muss. Im Portal können Sie nur ein Subnetz definieren, wenn Sie ein virtuelles Netzwerk erstellen. Sie können später weitere Subnetze zum virtuellen Netzwerk hinzufügen, nachdem das virtuelle Netzwerk erstellt wurde. Informationen zum Hinzufügen eines Subnetzes zu einem virtuellen Netzwerk finden Sie unter [Erstellen eines Subnetzes](virtual-network-manage-subnet.md#create-subnet) im Artikel [Erstellen, Ändern oder Löschen von Subnetzen](virtual-network-manage-subnet.md). Sie können über die Azure CLI oder über PowerShell ein virtuelles Netzwerk mit mehreren Subnetzen erstellen.

      >[!TIP]
      >Gelegentlich erstellen Administratoren unterschiedliche Subnetze, um das Datenverkehrrouting zwischen ihnen zu filtern oder zu steuern. Erwägen Sie vor dem Definieren von Subnetzen, wie Sie Datenverkehr zwischen Ihren Subnetzen filtern und weiterleiten möchten. Weitere Informationen zum Filtern von Datenverkehr zwischen Subnetzen finden Sie unter [Netzwerksicherheitsgruppen](virtual-networks-nsg.md). Azure leitet Datenverkehr zwischen Subnetzen automatisch weiter, doch Sie können diese Standardrouten von Azure außer Kraft setzen. Informationen zum Außerkraftsetzen des standardmäßigen Datenverkehrrouting bei Azure-Subnetzen finden Sie unter [Benutzerdefinierte Routen](virtual-networks-udr-overview.md).
      >

    - **Subnetzadressbereich:** Der Bereich muss sich im Adressraum befinden, den Sie für das virtuelle Netzwerk eingegeben haben. Der kleinste Bereich, den Sie angeben können, ist /29, der acht IP-Adressen für das Subnetz bereitstellt. Azure reserviert die erste und letzte Adresse in jedem Subnetz aus Gründen der Protokollkonformität. Drei weitere Adressen sind für die Nutzung durch Azure-Dienste reserviert. Folglich weist ein virtuelles Netzwerk mit dem Subnetzadressbereich „/29“ drei verwendbare IP-Adressen auf. Wenn Sie ein virtuelles Netzwerk mit einem VPN-Gateway verbinden möchten, müssen Sie ein Gatewaysubnetz erstellen. Weitere Informationen finden Sie unter [Informationen zu VPN Gateway-Einstellungen](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub) im Abschnitt „Gatewaysubnetz“. Unter bestimmten Bedingungen können Sie den Adressbereich ändern, nachdem das Subnetz erstellt wurde. Informationen zum Ändern des Adressbereichs eines Subnetzes finden Sie im Abschnitt [Ändern der Subnetzeinstellungen](#change-subnet) des Artikels [Hinzufügen, Ändern oder Löschen von Subnetzen](virtual-network-manage-subnet.md).
    - **Abonnement**: Wählen Sie ein [Abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) aus. Sie können nicht dasselbe virtuelle Netzwerk in mehreren Azure-Abonnements verwenden. Allerdings können Sie ein virtuelles Netzwerk in einem Abonnement mit virtuellen Netzwerken in anderen Abonnements verbinden. Um virtuelle Netzwerke in verschiedenen Abonnements zu verbinden, verwenden Sie das Azure-VPN-Gateway oder das Peering virtueller Netzwerke. Alle Azure-Ressource, die Sie mit dem virtuellen Netzwerk verbinden, müssen sich im selben Abonnement wie das des virtuellen Netzwerks befinden.
    - **Ressourcengruppe**: Wählen Sie eine vorhandene [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) aus, oder erstellen Sie eine neue Ressourcengruppe. Azure-Ressourcen, die Sie mit dem virtuellen Netzwerk verbinden, können sich in derselben Ressourcengruppe wie die des virtuellen Netzwerks oder in einer anderen Ressourcengruppe befinden.
    - **Standort**: Wählen Sie einen Azure-[Standort](https://azure.microsoft.com/regions/) (auch als „Region“ bezeichnet). Ein virtuelles Netzwerk kann sich nur an einem Azure-Standort befinden. Allerdings können Sie ein virtuelles Netzwerk an einem Standort mithilfe eines VPN-Gateways mit einem virtuellen Netzwerk an einem anderen Standort verbinden. Alle Azure-Ressource, die Sie mit dem virtuellen Netzwerk verbinden, müssen sich am selben Standort wie dem des virtuellen Netzwerks befinden.

**Befehle**

|Tool|Befehl|
|---|---|
|Azure-Befehlszeilenschnittstelle|[az network vnet create](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name = "view-vnet"></a>Anzeigen von virtuellen Netzwerken und Einstellungen

So zeigen Sie virtuelle Netzwerke und Einstellungen an:

1. Melden Sie sich beim [Portal](https://portal.azure.com) mit einem Konto an, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im Portal in das Suchfeld **Virtuelle Netzwerke** ein. Klicken Sie in den Suchergebnissen auf **Virtuelle Netzwerke**.
3. Klicken Sie auf dem Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, dessen Einstellungen Sie anzeigen möchten.
4. Auf dem Blatt, das für das ausgewählte virtuelle Netzwerk angezeigt wird, stehen folgende Einstellungen zur Verfügung:
    - **Übersicht**: Enthält Informationen zum virtuellen Netzwerk, z.B. zum Adressraum und den DNS-Servern. Der folgende Screenshot zeigt die Übersicht über die Einstellungen eines virtuellen Netzwerks mit dem Namen **MyVNet** an:

        ![Übersicht über die Netzwerkschnittstelle](./media/virtual-network-manage-network/vnet-overview.png)

      Auf dem Blatt **Übersicht** können Sie ein virtuelles Netzwerk in ein anderes Abonnement oder eine andere Ressourcengruppe verschieben. Informationen zum Verschieben eines virtuellen Netzwerks finden Sie unter [Verschieben von Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json). In diesem Artikel erfahren Sie, welche Voraussetzungen erfüllt sein müssen und wie Sie Ressourcen über das Azure-Portal, mithilfe von PowerShell oder unter Verwendung der Azure CLI verschieben. Alle Ressourcen, die mit dem virtuellen Netzwerk verbunden sind, müssen mit dem virtuellen Netzwerk verschoben werden.
    - **Adressraum**: Die dem virtuellen Netzwerk zugewiesenen Adressräume werden aufgelistet. Die erforderlichen Schritte zum Hinzufügen und Entfernen eines Adressraums werden im Abschnitt [Hinzufügen oder Entfernen eines Adressraums](#address-spaces) in diesem Artikel erläutert.
    - **Verbundene Geräte**: Alle Ressourcen, die mit dem virtuellen Netzwerk verbunden sind, werden aufgeführt. Im vorhergehenden Screenshot werden drei Netzwerkschnittstellen und ein Load Balancer gezeigt, die mit dem virtuellen Netzwerk verbunden sind. Neue Ressourcen, die Sie erstellen und mit dem virtuellen Netzwerk verbinden, werden aufgeführt. Wenn Sie eine Ressource löschen, die mit dem virtuellen Netzwerk verbunden war, wird sie nicht mehr in der Liste angezeigt.
    - **Subnetze**: Eine Liste von Subnetzen im virtuellen Netzwerk wird angezeigt. Informationen zum Hinzufügen und Entfernen eines Subnetz, finden Sie unter [Erstellen eines Subnetzes](virtual-network-manage-subnet.md#create-subnet) und [Löschen eines Subnetzes](virtual-network-manage-subnet.md#delete-subnet) im Artikel [Hinzufügen, Ändern oder Löschen von Subnetzen](virtual-network-manage-subnet.md).
    - **DNS-Server**: Sie können angeben, ob der interne DNS-Server von Azure oder ein benutzerdefinierter DNS-Server die Namensauflösung für Geräte bereitstellt, die mit dem virtuellen Netzwerk verbunden sind. Wenn Sie ein virtuelles Netzwerk über das Azure-Portal erstellen, werden standardmäßig die DNS-Server von Azure für die Namensauflösung in einem virtuellen Netzwerk verwendet. Führen Sie zum Ändern der DNS-Server die Schritte im Abschnitt [Ändern, Hinzufügen oder Entfernen eines DNS-Servers](#dns-servers) in diesem Artikel durch.
    - **Peerings**: Wenn Peerings im Abonnement vorhanden sind, werden sie hier aufgeführt. Sie können die Einstellungen für vorhandene Peerings anzeigen oder Peerings erstellen, ändern oder löschen. Weitere Informationen zu Peerings finden Sie im Artikel [Peering virtueller Netzwerke](virtual-network-peering-overview.md).
    - **Eigenschaften**: Zeigt Einstellungen des virtuellen Netzwerks an, wie z.B. seine Ressourcen-ID und das zugehörige Azure-Abonnement.
    - **Diagramm**: Das Diagramm bietet eine visuelle Darstellung aller Geräte, die mit dem virtuellen Netzwerk verbunden sind. Das Diagramm enthält einige wichtige Informationen zu den Geräten. Um Geräte in dieser Ansicht zu verwalten, klicken Sie im Diagramm auf das Gerät.
    - **Allgemeine Azure-Einstellungen**: Weitere Informationen zu allgemeinen Azure-Einstellungen finden Sie in folgenden Artikeln:
        *   [Aktivitätsprotokoll](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
        *   [Zugriffssteuerung (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
        *   [Tags](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags)
        *   [Sperren](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
        *   [Automatisierungsskript](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Befehle**

|Tool|Befehl|
|---|---|
|Azure-Befehlszeilenschnittstelle|[az network vnet show](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork/?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="add-address-spaces"></a>Hinzufügen oder Entfernen eines Adressraums

Sie können Adressräume eines virtuellen Netzwerks hinzufügen und entfernen. Ein Adressraum muss in der CIDR-Notation angegeben werden und darf sich innerhalb desselben virtuellen Netzwerks nicht mit anderen Adressräumen überlappen. Die von Ihnen definierten Adressräume können öffentlich oder privat (RFC 1918) sein. Ganz gleich, ob Sie den Adressraum als öffentlich oder privat definieren – der Adressraum ist nur innerhalb des virtuellen Netzwerks, über miteinander verbundene virtuelle Netzwerke und über beliebige lokale Netzwerke erreichbar, die Sie mit dem virtuellen Netzwerk verbunden haben. Sie können die folgenden Adressräume nicht hinzufügen:

- 224.0.0.0/4 (Multicast)
- 255.255.255.255/32 (Übertragung)
- 127.0.0.0/8 (Loopback)
- 169.254.0.0/16 (verbindungslokal)
- 168.63.129.16/32 (Internes DNS)

So fügen Sie einen Adressraum hinzu oder entfernen diesen:

1. Melden Sie sich beim [Portal](https://portal.azure.com) mit einem Konto an, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im Portal in das Suchfeld **Virtuelle Netzwerke** ein. Wählen Sie in den Suchergebnissen **Virtuelle Netzwerke** aus.
3. Klicken Sie auf dem Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, für das Sie einen Adressraum hinzufügen oder entfernen möchten.
4. Klicken Sie auf dem Blatt „Virtuelles Netzwerk“ unter **EINSTELLUNGEN** auf **Adressraum**.
5. Wählen Sie eine der folgenden Optionen auf dem Blatt aus, das für den Adressraum angezeigt wird:
    - **Adressraum hinzufügen**: Geben Sie den neuen Adressraum ein. Der Adressraum darf sich nicht mit einem vorhandenen Adressraum überlappen, der für das virtuelle Netzwerk definiert ist.
    - **Adressraum entfernen**: Klicken Sie mit der rechten Maustaste auf einen Adressraum, und klicken Sie dann auf **Entfernen**. Wenn ein Subnetz im Adressraum vorhanden ist, können Sie den Adressraum nicht entfernen. Um einen Adressraum zu entfernen, müssen Sie zunächst alle im Adressraum enthaltenen Subnetze (und mit diesen verbundene Ressourcen) löschen.
6. Klicken Sie auf **Speichern**.

**Befehle**

|Tool|Befehl|
|---|---|
|Azure-Befehlszeilenschnittstelle|Nur Ressourcen-Manager|[az network vnet update](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="dns-servers"></a>Hinzufügen, Ändern oder Entfernen eines DNS-Servers

Alle mit dem virtuellen Netzwerk verbundenen VMs werden bei den DNS-Servern registriert, die Sie für das virtuelle Netzwerk angeben. Zudem wird der angegebene DNS-Server für die Namensauflösung verwendet. Jede Netzwerkschnittstellenkarte (NIC) einer VM hat eigene DNS-Servereinstellungen. Wenn eine Netzwerkkarte mit eigenen DNS-Servereinstellungen versehen ist, überschreiben diese die DNS-Servereinstellungen für das virtuelle Netzwerk. Weitere Informationen zu DNS-Einstellungen von Netzwerkkarten finden Sie unter [Aufgaben und Einstellungen für Netzwerkschnittstellen](virtual-network-network-interface.md#change-dns-servers). Weitere Informationen zur Namensauflösung für VMs und zu Rolleninstanzen in Azure Cloud Services finden Sie unter [Namensauflösung für VMs und Rolleninstanzen](virtual-networks-name-resolution-for-vms-and-role-instances.md). So fügen Sie einen DNS-Server hinzu oder ändern bzw. entfernen diesen:

1. Melden Sie sich beim [Portal](https://portal.azure.com) mit einem Konto an, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im Portal in das Suchfeld **Virtuelle Netzwerke** ein. Wählen Sie in den Suchergebnissen **Virtuelle Netzwerke** aus.
3. Klicken Sie auf dem Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, dessen DNS-Einstellungen Sie ändern möchten.
4. Klicken Sie auf dem Blatt „Virtuelles Netzwerk“ unter **EINSTELLUNGEN** auf **DNS-Server**.
5. Wählen Sie eine der folgenden Optionen auf dem Blatt aus, die für DNS-Server aufgeführt werden:
    - **Standard (von Azure bereitgestellt)**: Alle Ressourcennamen und privaten IP-Adressen werden automatisch bei den Azure-DNS-Servern registriert. Sie können Namen zwischen allen Ressourcen auflösen, die im selben virtuellen Netzwerk miteinander verbunden sind. Diese Option eignet sich nicht zum Auflösen von Namen über virtuelle Netzwerke hinweg. Hierfür müssen Sie einen benutzerdefinierten DNS-Server verwenden.
    - **Benutzerdefiniert**: Sie können einen oder mehrere Server bis zum Azure-Grenzwert für ein virtuelles Netzwerk hinzufügen. Weitere Informationen zu Einschränkungen für DNS-Server finden Sie unter [Azure-Grenzwerte](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-networking-limits-classic). Folgende Optionen stehen zur Auswahl:
        - **Adresse hinzufügen**: Hierüber können Sie den Server der DNS-Serverliste Ihres virtuellen Netzwerks hinzufügen. Durch diese Option wird der DNS-Server auch bei Azure registriert. Wenn Sie bereits einen DNS-Server bei Azure registriert haben, können Sie ihn in der Liste auswählen.
        - **Adresse entfernen**: Klicken Sie auf das **X** neben dem Server, den Sie entfernen möchten. Durch das Löschen des Servers wird dieser nur aus der Liste dieses virtuellen Netzwerks entfernt. Der DNS-Server bleibt in Azure registriert und kann von Ihren anderen virtuellen Netzwerken verwendet werden.
        - **Reihenfolge von DNS-Serveradressen ändern**: Beim Angeben von DNS-Servern müssen Sie darauf achten, dass Sie sie in der richtigen Reihenfolge für Ihre Umgebung auflisten. DNS-Serverlisten werden in der Reihenfolge verwendet, in der sie angegeben sind. Sie können nicht als Roundrobinsetup verwendet. Wenn der erste DNS-Server in der Liste erreicht werden kann, verwendet der Client diesen DNS-Server unabhängig davon, ob der DNS-Server ordnungsgemäß funktioniert. Entfernen Sie alle aufgeführten DNS-Server, und fügen Sie sie in der gewünschten Reihenfolge wieder hinzu.
        - **Adresse ändern**: Markieren Sie den DNS-Server in der Liste, und geben Sie den neuen Namen ein.
6. Klicken Sie auf **Speichern**.
7. Starten Sie die mit dem virtuellen Netzwerk verbundenen VMs, damit ihnen die neuen DNS-Servereinstellungen zugewiesen werden. VMs nutzen ihre aktuellen DNS-Einstellungen weiter, bis sie neu gestartet werden.

**Befehle**

|Tool|Befehl|
|---|---|
|Azure-Befehlszeilenschnittstelle|[az network vnet update](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="delete-vnet"></a>Löschen eines virtuellen Netzwerks

Sie können ein virtuelles Netzwerk erst löschen, wenn keine Ressourcen mehr damit verbunden sind. Falls Ressourcen mit den Subnetzen im virtuellen Netzwerk verbunden sind, müssen Sie zuerst die Ressourcen, die mit allen Subnetzen im virtuellen Netzwerk verbunden sind, löschen. Die Schritte zum Löschen einer Ressource variieren abhängig von der Ressource. Informationen zum Löschen von mit Subnetzen verbundenen Ressourcen finden Sie in der Dokumentation zum jeweiligen Ressourcentyp, den Sie löschen möchten. So löschen Sie ein virtuelles Netzwerk:

1. Melden Sie sich beim [Portal](https://portal.azure.com) mit einem Konto an, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im Portal in das Suchfeld **Virtuelle Netzwerke** ein. Klicken Sie in den Suchergebnissen auf **Virtuelle Netzwerke**.
3. Wählen Sie auf dem Blatt **Virtuelle Netzwerke** das virtuelle Netzwerk, das Sie löschen möchten.
4. Um sicherzustellen, dass keine Geräte mit dem virtuellen Netzwerk verbunden sind, klicken Sie auf dem Blatt „Virtuelles Netzwerk“ unter **EINSTELLUNGEN** auf **Verbundene Geräte**. Wenn verbundene Geräte vorhanden sind, müssen Sie diese löschen, ehe Sie das virtuelle Netzwerk löschen können. Wenn keine verbundenen Geräte vorhanden sind, klicken Sie auf **Übersicht**.
5. Klicken Sie oben auf dem Blatt auf das Symbol **Löschen**.
6. Klicken Sie auf **Ja**, um das Löschen des virtuellen Netzwerks zu bestätigen.


**Befehle**

|Tool|Befehl|
|---|---|
|Azure-Befehlszeilenschnittstelle|[azure network vnet delete](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmVirtualNetwork](/powershell/module/azurerm.network/remove-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="next-steps"></a>Nächste Schritte

- Informationen dazu, wie Sie eine VM erstellen und mit einem virtuellen Netzwerk verbinden, finden Sie unter [Erstellen virtueller Netzwerke und Verbinden von VMs](virtual-network-get-started-vnet-subnet.md#create-vms).
- Informationen dazu, wie Sie Netzwerkdatenverkehr zwischen Subnetzen in einem virtuellen Netzwerk filtern, finden Sie unter [Erstellen von Netzwerksicherheitsgruppen](virtual-networks-create-nsg-arm-pportal.md).
- Um ein virtuelles Netzwerk mittels Peering mit einem anderen virtuellen Netzwerk verbinden zu können, lesen Sie [Erstellen eines Peerings virtueller Netzwerke](virtual-network-create-peering.md#portal).
- Weitere Informationen zu Optionen zum Verbinden eines virtuellen Netzwerks mit einem lokalen Netzwerk finden Sie unter [Informationen zu VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams).

