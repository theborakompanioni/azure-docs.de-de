---
title: "Erstellen, Ändern oder Löschen eines Peerings virtueller Azure-Netzwerke | Microsoft-Dokumentation"
description: "In diesem Artikel erfahren Sie, wie Sie ein Peering virtueller Netzwerke erstellen, ändern oder löschen."
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
ms.date: 07/26/2017
ms.author: jdial
ms.translationtype: HT
ms.sourcegitcommit: 349fe8129b0f98b3ed43da5114b9d8882989c3b2
ms.openlocfilehash: 61bff8af221b9155dc3fa4def5d9e7cdcff9a6f6
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="create-change-or-delete-a-virtual-network-peering"></a>Erstellen, Ändern oder Löschen eines Peerings virtueller Netzwerke

In diesem Artikel erfahren Sie, wie Sie ein Peering virtueller Netzwerke erstellen, ändern oder löschen. Das Peering virtueller Netzwerke ist ein Mechanismus, mit dem zwei virtuelle Netzwerke am selben Azure-Standort über das Azure-Backbonenetzwerk miteinander verbunden werden können. Nach dem Peering werden die beiden virtuellen Netzwerke weiterhin als separate Ressourcen verwaltet. Ressourcen kommunizieren in jedem virtuellen Netzwerk mit derselben Latenz und Bandbreite, als ob sie sich in demselben virtuellen Netzwerk befinden würden. Wenn Sie nicht mit dem Peering virtueller Netzwerke vertraut sind, sollten Sie vor der Durchführung der Aufgaben den Artikel [Peering virtueller Netzwerke – Überblick](virtual-network-peering-overview.md) lesen und das Tutorial unter [Erstellen eines Peerings virtueller Netzwerke](virtual-network-create-peering.md) absolvieren.

## <a name="before-you-begin"></a>Voraussetzungen

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Lesen Sie den Artikel [Azure-Grenzwerte](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits), um mehr über die Einschränkungen von Peerings zu erfahren.
- Melden Sie sich mit einem Azure-Konto beim Azure-Portal, der Azure-Befehlszeilenschnittstelle (CLI) oder bei Azure PowerShell an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Wenn Sie PowerShell-Befehle verwenden, um Aufgaben in diesem Artikel durchzuführen, [installieren und konfigurieren Sie Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure PowerShell-Cmdlets installiert haben. Hilfe und Beispiele für PowerShell-Befehle erhalten Sie durch Eingabe von `get-help <command> -full`.
- Wenn Sie Befehle der Azure-Befehlszeilenschnittstelle (CLI) verwenden, um Aufgaben in diesem Artikel durchzuführen, [installieren und konfigurieren Sie die Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure CLI installiert haben. Hilfe zu den Befehlen der Befehlszeilenschnittstelle erhalten Sie durch Eingabe von `az <command> --help`. Anstatt die CLI und ihre Voraussetzungen zu installieren, können Sie die Azure Cloud Shell verwenden. Azure Cloud Shell ist eine kostenlose Bash-Shell, die Sie direkt im Azure-Portal ausführen können. Die Azure-Befehlszeilenschnittstelle ist bei der Cloud Shell vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Wenn Sie die Cloud Shell verwenden möchten, klicken Sie oben im [Portal](https://portal.azure.com) auf die Cloud Shell-Schaltfläche **>_**. 

## <a name="create-a-peering"></a>Erstellen eines Peerings

>[!NOTE]
>Es gibt verschiedene Anforderungen, Einschränkungen und Überlegungen zur erfolgreichen Erstellung eines Peerings eines virtuellen Netzwerks zu berücksichtigen. Lesen Sie vor der Erstellung eines Peerings sicher, dass Sie mit den [Anforderungen und Einschränkungen](#requirements-and-constraints) sowie den [erforderlichen Berechtigungen](#permissions) vertraut sind.
>

1. Melden Sie sich im [Portal](https://portal.azure.com) mit einem Konto an, das der erforderlichen [Rolle oder Berechtigung](#permissions) zugewiesen ist.
2. Geben Sie oben im Azure-Portal im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Virtuelle Netzwerke* ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, klicken Sie darauf. Aktivieren Sie nicht **Virtuelle Netzwerke (klassisch)**, wenn diese Option in der Liste angezeigt wird, da über das klassische Bereitstellungsmodell kein Peering von einem bereitgestellten virtuellen Netzwerk aus erstellt werden kann.
3. Klicken Sie auf dem angezeigten Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, für das Sie ein Peering erstellen möchten.
4. Klicken Sie im Bereich, der für das von Ihnen gewählte virtuelle Netzwerk angezeigt wird, im Abschnitt **EINSTELLUNGEN** auf **Peerings**.
5. Klicken Sie auf **+ Hinzufügen**. 
6. <a name="add-peering"></a>Geben Sie auf dem Blatt **Peering hinzufügen** Werte für folgende Einstellungen ein, oder wählen Sie sie aus:
    - **Name**: Der Name für das Peering muss innerhalb des virtuellen Netzwerks eindeutig sein.
    - **Bereitstellungsmodell für das virtuelle Netzwerk**: Wählen Sie aus, über welches Bereitstellungsmodell das mittels Peering verknüpfte virtuelle Netzwerk bereitgestellt wurde.
    - **Ich kenne meine Ressourcen-ID**: Wenn Sie Lesezugriff auf das virtuelle Netzwerk haben, das mittels Peering verknüpft werden sollen, lassen Sie dieses Kontrollkästchen deaktiviert. Wenn Sie keinen Lesezugriff auf das virtuelle Netzwerk oder das Abonnement besitzen, das mittels Peering verknüpft werden soll, aktivieren Sie dieses Kontrollkästchen. Geben Sie in das Feld **Ressourcen-ID**, das bei Aktivierung des Kontrollkästchens angezeigt wird, die vollständige Ressourcen-ID des mittels Peering verknüpften virtuellen Netzwerks ein. Die von Ihnen eingegebene Ressourcen-ID muss für ein virtuelles Netzwerk bestimmt sein, das sich am selben Azure-[Standort](https://azure.microsoft.com/regions) wie dieses virtuelle Netzwerk befindet. Die vollständige Ressourcen-ID sieht in etwa wie folgt aus: „/subscriptions/<Id>/resourceGroups/<Ressourcengruppenname>/providers/Microsoft.Network/virtualNetworks/<Name des virtuellen Netzwerks>“. Sie können die Ressourcen-ID für ein virtuelles Netzwerk abrufen, indem Sie die Eigenschaften eines virtuellen Netzwerks anzeigen. Informationen zum Anzeigen der Eigenschaften eines virtuellen Netzwerks finden Sie im Artikel [Verwalten virtueller Netzwerke](virtual-network-manage-network.md#view-vnet).
    - **Abonnement**: Wählen Sie das [Abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) des virtuellen Netzwerks aus, das mittels Peering verknüpft werden soll. Je nachdem, auf wie viele Abonnements Ihr Konto Lesezugriff verfügt, werden ein oder mehrere Abonnements aufgeführt. Wenn Sie das Kontrollkästchen **Ressourcen-ID** aktivieren, ist diese Einstellung ist verfügbar. Sie können virtuelle Netzwerke in verschiedenen Abonnements mittels Peering verknüpfen, solange beide virtuelle Netzwerke über Resource Manager erstellt wurden. Das Peering von Netzwerken über verschiedene Abonnements hinweg, die über unterschiedliche Bereitstellungsmodelle erstellt wurden, ist mit der Vorschauversion möglich. Führen Sie für die Vorschau eine Registrierung durch, bevor Sie ein Peering zwischen virtuellen Netzwerken erstellen, die über verschiedene in den Abonnements vorhandenen Bereitstellungsmodellen vorhanden sind. Weitere Informationen finden Sie in den zu den Abschnitten zum Registrieren für die Vorschau und [Peering von virtuellen Netzwerken, die in verschiedenen Bereitstellungsmodellen erstellt wurden und zu unterschiedlichen Abonnements gehören](create-peering-different-deployment-models-subscriptions.md).
    - **Virtuelles Netzwerk**: Wählen Sie das virtuelle Netzwerk aus, das mittels Peering verknüpft werden soll. Sie können ein virtuelles Netzwerk auswählen, das über ein Azure-Bereitstellungsmodell erstellt wurde. Das virtuelle Netzwerk muss sich jedoch am selben Standort wie das des virtuellen Netzwerks befinden, in dem das Peering initiiert wird. Sie benötigen Lesezugriff auf das virtuelle Netzwerk, damit es in der Liste angezeigt wird. Wenn ein virtuelles Netzwerk aufgeführt wird, jedoch ausgegraut ist, überschneidet sich der Adressraum des virtuellen Netzwerks möglicherweise mit dem Adressraum dieses virtuellen Netzwerks. Wenn sich die Adressräume des virtuellen Netzwerks überschneidet, können diese nicht mittels Peering verknüpft werden. Wenn Sie das Kontrollkästchen **Ressourcen-ID** aktivieren, ist diese Einstellung ist verfügbar.
    - **Zugriff auf virtuelles Netzwerk zulassen**: Wählen Sie **Aktiviert** (Standard), wenn Kommunikation zwischen beiden virtuellen Netzwerken aktiviert werden soll. Durch die Aktivierung der Kommunikation zwischen virtuellen Netzwerken können Ressourcen, die mit einem der beiden virtuellen Netzwerke verbunden sind, mit derselben Bandbreite und Latenz kommunizieren, als ob sie mit demselben virtuellen Netzwerk verbunden wären. Die gesamte Kommunikation zwischen Ressourcen in den beiden virtuellen Netzwerken erfolgt über das private Azure-Netzwerk. Das Standardtag **VirtualNetwork** für Netzwerksicherheitsgruppen umfasst das virtuelle Netzwerk und das mittels Peering verknüpfte virtuelle Netzwerk. Weitere Informationen zu Standardtags von Netzwerksicherheitsgruppen finden Sie im Artikel [Netzwerksicherheitsgruppen – Übersicht](virtual-networks-nsg.md#default-tags).  Wählen Sie **Deaktiviert**, wenn der Datenverkehr nicht über das mittels Peering verknüpfte virtuelle Netzwerk weitergeleitet werden soll. Sie können **Deaktiviert** wählen, wenn Sie ein virtuelles Netzwerk mittels Peering mit einem anderen virtuellen Netzwerk verknüpft haben, jedoch die Option haben möchten, den Datenverkehr zwischen den beiden virtuellen Netzwerken gelegentlich zu deaktivieren. Es kann u.U. praktischer sein, die Option zu aktivieren bzw. zu deaktivieren statt Peerings zu löschen und neu zu erstellen. Wenn diese Einstellung deaktiviert ist, wird der Datenverkehr nicht zwischen den mittels Peering verknüpften virtuellen Netzwerken weitergeleitet.
    - **Weitergeleiteten Datenverkehr zulassen**: Aktivieren Sie dieses Kontrollkästchen, um zuzulassen, dass der an das mittels Peering verknüpfte virtuelle Netzwerk weitergeleitete Datenverkehr (Datenverkehr, der nicht in dem mittels Peering verknüpften virtuellen Netzwerk generiert wird) über dieses virtuelle Netzwerk erfolgt. Die Weiterleitung von Datenverkehr ist üblich, wenn Sie ein virtuelles Netzwerkgerät im virtuellen Netzwerk bereitgestellt haben, mit dem Sie über Peering eine Verbindung herstellen, und zur Weiterleitung von Datenverkehr benutzerdefinierte Routen über das virtuelle Netzwerkgerät erstellt haben. Wenn Sie dieses Kontrollkästchen deaktiviert (Standardeinstellung) lassen, kann Datenverkehr, der vom mittels Peering verknüpften virtuellen Netzwerk weitergeleitet wurde, nicht an dieses virtuelle Netzwerk weitergeleitet werden. Durch die Aktivierung dieser Funktion wird zwar die Weiterleitung des Datenverkehrs mittels Peering ermöglicht, es werden jedoch keine benutzerdefinierten Routen oder virtuellen Netzwerkgeräte erstellt. Benutzerdefinierte Routen und virtuelle Netzwerkgeräte werden separat erstellt. Weitere Informationen hierzu finden Sie unter [Benutzerdefinierte Routen](virtual-networks-udr-overview.md).
    - **Gatewaytransit zulassen**: Aktivieren Sie dieses Kontrollkästchen, wenn ein Gateway für virtuelle Netzwerke mit diesem virtuellen Netzwerk verbunden ist und Sie zulassen möchten, dass Datenverkehr von mittels Peering verknüpften virtuellen Netzwerken über das Gateway geleitet werden soll. Dieses virtuelle Netzwerk kann z.B. über ein Gateway für virtuelle Netzwerke mit einem lokalen Netzwerk verbunden werden. Wird dieses Kontrollkästchen aktiviert, kann Datenverkehr aus dem mittels Peering verknüpften virtuellen Netzwerk über das verbundene Gateway an dieses virtuelle Netzwerk weitergeleitet werden. Wenn Sie dieses Kontrollkästchen aktivieren, darf für das mittels Peering verknüpfte virtuelle Netzwerk kein Gateway konfiguriert sein. Wenn Sie das Peering vom anderen virtuellen Netzwerk mit diesem virtuellen Netzwerk einrichten, muss für das mittels Peering verknüpfte virtuelle Netzwerk das Kontrollkästchen **Remotegateway verwenden** aktiviert sein. Wenn Sie dieses Kontrollkästchen deaktiviert (Standard) lassen, wird der Datenverkehr trotzdem von dem mittels Peering verknüpften Netzwerk an dieses virtuelle Netzwerk weitergeleitet, kann jedoch nicht über ein mit diesem virtuellen Netzwerk verbundenes Gateway für virtuelle Netzwerke erfolgen. Weitere Informationen hierzu finden Sie unter [Gateways für virtuelle Netzwerke](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#site-to-site-and-multi-site-ipsecike-vpn-tunnel). 
    
    Diese Option kann nicht aktiviert werden, wenn Sie ein virtuelles Netzwerk (Resource Manager) mittels Peering mit einem virtuellen Netzwerk (klassisch) verbinden. Trotz des Datenflusses zwischen zwei virtuellen Netzwerken kann der Datenverkehr des virtuellen Netzwerks (klassisch) nicht über ein Netzwerkgateway erfolgen, das mit dem virtuellen Netzwerk (Resource Manager) verbunden ist.
    - **Remotegateways verwenden**: Aktivieren Sie dieses Kontrollkästchen, wenn Sie zulassen möchten, dass Datenverkehr von diesen virtuellen Netzwerken über ein Gateway für virtuelle Netzwerke geleitet werden soll, das mit dem mittels Peering verknüpften virtuellen Netzwerk verbunden ist. Beispielsweise ist an das virtuelle Netzwerk, mit dem Sie mittels Peering eine Verbindung herstellen, ein VPN-Gateway verbunden, dass Kommunikation mit einem lokalen Netzwerk ermöglicht.  Wird dieses Kontrollkästchen aktiviert, kann Datenverkehr aus diesem virtuellen Netzwerk über das verbundene VPN-Gateway an dieses mittels Peering verknüpfte virtuelle Netzwerk weitergeleitet werden. Wenn Sie dieses Kontrollkästchen aktivieren, muss ein Gateway für virtuelle Netzwerke mit dem mittels Peering verknüpften virtuellen Netzwerk verbunden sein. Zudem muss das Kontrollkästchen **Gatewaytransit zulassen** aktiviert sein. Wenn Sie dieses Kontrollkästchen deaktiviert (Standard) lassen, kann der Datenverkehr trotzdem von dem mittels Peering verknüpften Netzwerk an dieses virtuelle Netzwerk weitergeleitet werden, kann jedoch nicht über ein mit diesem virtuellen Netzwerk verbundenes Gateway für virtuelle Netzwerke erfolgen. 
    
    Diese Option kann nicht aktiviert werden, wenn Sie ein virtuelles Netzwerk (Resource Manager) mittels Peering mit einem virtuellen Netzwerk (klassisch) verbinden. Trotz des Datenverkehrsflusses zwischen zwei virtuellen Netzwerken kann der Datenverkehr des virtuellen Netzwerks (Resource Manager) nicht über ein Netzwerkgateway erfolgen, das mit dem virtuellen Netzwerk (klassisch) verbunden ist.
7. Klicken Sie auf die Schaltfläche **OK**, um das Subnetz dem ausgewählten virtuellen Netzwerk hinzuzufügen.

### <a name="commands"></a>Befehle

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network vnet peering create](/cli/azure/network/vnet/peering#create?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Add-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json)|


### <a name="scenarios"></a>Szenarien

Ein Peering für virtuelle Netzwerke wird zwischen virtuellen Netzwerken erstellt, die mit dem gleichen oder unterschiedlichen Bereitstellungsmodellen unter demselben oder unterschiedlichen Abonnements erstellt wurden. Arbeiten Sie ein ausführliches Tutorial für eines der folgenden Szenarios durch:
 
|Azure-Bereitstellungsmodell  | Abonnement  |
|---------|---------|
|Beide mit Resource Manager |[Gleich](virtual-network-create-peering.md)|
| |[Unterschiedlich](create-peering-different-subscriptions.md)|
|Einmal Resource Manager, einmal klassisch     |[Gleich](create-peering-different-deployment-models.md)|
| |[Unterschiedlich](create-peering-different-deployment-models-subscriptions.md)|

## <a name="view-or-change-peering-settings"></a>Anzeigen oder Ändern von Peeringeinstellungen

1. Melden Sie sich im [Portal](https://portal.azure.com) mit einem Konto an, das der erforderlichen [Rolle oder Berechtigung](#permissions) zugewiesen ist.
2. Geben Sie oben im Azure-Portal im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Virtuelle Netzwerke* ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem angezeigten Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, für das Sie ein Peering erstellen möchten.
4. Klicken Sie im Bereich, der für das von Ihnen gewählte virtuelle Netzwerk angezeigt wird, im Abschnitt **EINSTELLUNGEN** auf **Peerings**.
5. Klicken Sie auf das Peering, für das die Einstellungen angezeigt oder geändert werden sollen.
6. Ändern Sie die entsprechende Einstellung. Informationen zu den Optionen für jede Einstellung in [Schritt 6](#add-peering) finden Sie im Abschnitt „Erstellen eines Peerings“ dieses Artikels. 

    >[!NOTE]
    >Es gibt verschiedene Anforderungen, Einschränkungen und Überlegungen zur erfolgreichen Erstellung eines Peerings eines virtuellen Netzwerks zu berücksichtigen. Lesen Sie vor der Erstellung eines Peerings sicher, dass Sie mit den [Anforderungen und Einschränkungen](#requirements-and-constraints) sowie den [erforderlichen Berechtigungen](#permissions) vertraut sind.
    >

7. Klicken Sie auf **Speichern**.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network vnet peering list](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#list) zum Auflisten von Peerings für ein virtuelles Netzwerk, [az network vnet peering show](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#show) zum Anzeigen von Einstellungen für ein bestimmtes Peering und [az network vnet peering update](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#update) zum Ändern von Peeringeinstellungen|
|PowerShell|[Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json) zum Abrufen der Ansichtspeeringeinstellungen und [Set-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/set-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json) zum Ändern der Einstellungen|

## <a name="delete-a-peering"></a>Löschen eines Peerings
Wenn ein Peering gelöscht wird, wird der Datenverkehr von einem virtuellen Netzwerk nicht mehr an das mittels Peering verknüpfte virtuelle Netzwerk weitergeleitet. Wenn bereitgestellte virtuelle Netzwerke mittels Peering über Resource Manager verbunden werden, weist jedes virtuelle Netzwerk ein Peering mit dem anderen virtuellen Netzwerk auf. Obwohl durch das Löschen des Peerings von einem virtuellen Netzwerk die Kommunikation zwischen den virtuellen Netzwerken deaktiviert wird, wird das Peering vom anderen virtuellen Netzwerk nicht gelöscht. Der Peeringstatus für das Peering, das im anderen virtuellen Netzwerk vorhanden ist, lautet **Getrennt**. Sie können das Peering erst neu erstellen, wenn Sie das Peering im ersten virtuellen Netzwerk neu erstellen, und der Peeringstatus bei beiden virtuellen Netzwerken zu *Verbunden* wechselt. 

Wenn Sie möchten, dass virtuelle Netzwerke gelegentlich, jedoch nicht immer, kommunizieren sollen, können Sie die Einstellung **Zugriff auf virtuelles Netzwerk zulassen** auf **Deaktiviert** festlegen, statt das Peering zu löschen. Informationen hierzu finden Sie in Schritt 6 des Abschnitts [Erstellen eines Peerings](#create-peering) dieses Artikels. Es kann ggf. einfacher sein, den Netzwerkzugriff zu deaktivieren und zu aktivieren, statt die Peerings zu löschen und neu zu erstellen.

1. Melden Sie sich im [Portal](https://portal.azure.com) mit einem Konto an, das der erforderlichen [Rolle oder Berechtigung](#permissions) zugewiesen ist.
2. Geben Sie oben im Azure-Portal im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Virtuelle Netzwerke* ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem angezeigten Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, für das Sie ein Peering löschen möchten.
4. Klicken Sie auf dem gewählten Blatt, das für das virtuelle Netzwerk angezeigt wird, unter **Einstellungen** auf **Peerings**.
5. Klicken Sie in der Liste der Peerings auf dem Blatt „Peerings“ mit der rechten Maustaste auf das Peering, das Sie löschen möchten. Klicken Sie anschließend auf **Löschen** und dann auf **Ja**, um das Peering des ersten virtuellen Netzwerks zu löschen.
6. Führen Sie die vorherigen Schritte durch, um das Peering aus dem anderen virtuellen Netzwerk im Peering zu löschen.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network vnet peering delete](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Remove-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/remove-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="requirements-and-constraints"></a>Anforderungen und Einschränkungen 

- Die mittels Peering verknüpften virtuellen Netzwerke dürfen keine sich überschneidenden IP-Adressräume aufweisen.
- Sie können Adressräume zu einem virtuellen Netzwerk hinzufügen oder aus diesem löschen, nachdem ein virtuelles Netzwerk per Peering mit einem anderen virtuellen Netzwerk verknüpft wurde. Um Adressräume hinzuzufügen oder zu entfernen, das Peering zu löschen oder Adressräume hinzuzufügen oder zu entfernen, erstellen Sie erneut das Peering. Um Adressräume zu virtuellen Netzwerken hinzuzufügen oder aus diesen zu entfernen, lesen Sie den Artikel [Erstellen, Ändern oder Löschen virtueller Netzwerke](virtual-network-manage-network.md#add-address-spaces). 
- Sie können zwei über Resource Manager bereitgestellte virtuelle Netzwerke oder ein über Resource Manager bereitgestelltes virtuelles Netzwerk per Peering mit einem virtuellen Netzwerk verknüpfen, das über das klassische Bereitstellungsmodell bereitgestellt wurde. Sie können nicht zwei virtuelle Netzwerke, die mit dem klassischen Bereitstellungsmodell erstellt wurden, mittels Peering miteinander verknüpfen. Wenn Sie mit den Azure-Bereitstellungsmodellen nicht vertraut sind, lesen Sie den Artikel [Grundlegendes zu Azure-Bereitstellungsmodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Sie können ein [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) verwenden, um zwei virtuelle Netzwerke zu verbinden, die mit dem klassischen Bereitstellungsmodell erstellt wurden.
- Wenn zwei virtuelle Netzwerke, die mit dem Resource Manager erstellt wurden, mittels Peering miteinander verknüpft werden, muss für jedes virtuelle Netzwerk im Peering ein Peering konfiguriert werden. Wenn Sie das Peering vom ersten virtuellen Netzwerk mit dem zweiten virtuellen Netzwerk erstellen, lautet der Peeringstatus *Initiiert*.  Wenn Sie das Peering vom zweiten virtuellen Netzwerk mit dem ersten virtuellen Netzwerk erstellen, lautet der Peeringstatus *Verbunden*. Wenn Sie sich den Peeringstatus für das erste virtuelle Netzwerk ansehen, erkennen Sie, dass der Status von *Initiiert* zu *Verbunden* gewechselt ist. Das Peering wurde erst erfolgreich erstellt, wenn der Peeringstatus für beide Peerings virtueller Netzwerk *Verbunden* lautet. 
- Wenn ein über Resource Manager erstelltes virtuelles Netzwerk mittels Peering mit einem über das klassische Bereitstellungsmodell erstellten virtuellen Netzwerk verbunden wird, konfigurieren Sie nur ein Peering für das virtuelle Netzwerk, das über Resource Manager bereitgestellt wurde. Es ist jedoch nicht möglich, ein Peering für ein virtuelles Netzwerk (klassisch) oder zwischen zwei virtuellen Netzwerken zu konfigurieren, die über das klassische Bereitstellungsmodell bereitgestellt wurden. Wenn Sie das virtuelle Netzwerk (Resource Manager) mittels Peering mit dem virtuellen Netzwerk (klassisch) verbinden, lautet der Peeringstatus *Wird aktualisiert* und wechselt anschließend gleich zu *Verbunden*.   
- Ein Peering wird zwischen zwei virtuellen Netzwerken eingerichtet. Peerings sind nicht transitiv. Wenn Sie Peerings zwischen Folgendem erstellen:
    - VirtualNetwork1 & VirtualNetwork2
    - VirtualNetwork2 & VirtualNetwork3

  Es erfolgt kein Peering zwischen VirtualNetwork1 und VirtualNetwork3 über VirtualNetwork2. Wenn Sie ein Peering zwischen virtuellen Netzwerken für VirtualNetwork1 und VirtualNetwork3 erstellen möchten, müssen Sie ein Peering zwischen VirtualNetwork1 und VirtualNetwork3 erstellen.
- Über die standardmäßige Azure-Namensauflösung können keine Namen in virtuellen Netzwerken aufgelöst werden, die mittels Peering verbunden sind. Um Namen in anderen virtuellen Netzwerken aufzulösen, müssen Sie einen benutzerdefinierten DNS-Server verwenden. Weitere Informationen zum Einrichten Ihres DNS-Servers finden Sie im Artikel [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
- Ressourcen in beiden virtuellen Netzwerken im Peering können mit derselben Bandbreite und Latenz miteinander kommunizieren, als ob sie sich im selben virtuellen Netzwerk befinden würden. Für die Größe jedes virtuellen Computers liegt jedoch eine eigene maximale Netzwerkbandbreite. Weitere Informationen zur Bandbreitenobergrenze für verschiedene VM-Größen finden Sie in den Artikeln zu den Größen von virtuellen Computern unter [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Sie können virtuelle Netzwerke, die über Resource Manager bereitgestellt werden, mittels Peering miteinander verbinden, die dasselbe Abonnement oder unterschiedliche Abonnements sind.
- Sie können virtuelle Netzwerke, die über andere Bereitstellungsmodelle bereitgestellt werden, mittels Peering miteinander verbinden, die dasselbe Abonnement oder unterschiedliche Abonnements aufweisen (Vorschauversion). 
- Die Abonnements, denen beide virtuelle Netzwerke zugewiesen sind, müssen demselben Azure Active Directory-Mandanten zugeordnet sein. Wenn Sie noch keinen AD-Mandanten besitzen, [erstellen Sie schnell einen](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). Sie können ein [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) verwenden, um zwei virtuelle Netzwerke in unterschiedlichen Abonnements zu verknüpfen, die verschiedenen Active Directory-Mandanten zugewiesen sind.
- Ein virtuelles Netzwerk kann mittels Peering mit einem anderen virtuellen Netzwerk verbunden werden; dies gilt auch für ein anderes virtuelles Netzwerk mit einem virtuellen Gateway für virtuelle Azure-Netzwerke. Wenn die virtuellen Netzwerke sowohl mittels Peering als auch mithilfe eines Gateways verbunden sind, wird der Datenverkehr zwischen den virtuellen Netzwerken über die Peeringkonfiguration statt über das Gateway geleitet.
- Für ein- und ausgehenden Datenverkehr, der VNET-Peering verwendet, fällt eine Gebühr an. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/virtual-network).


## <a name="permissions"></a>Berechtigungen

Die Konten, mit denen Sie das Peering in virtuellen Netzwerken erstellt haben, müssen die erforderliche Rolle oder die erforderlichen Berechtigungen haben. Wenn Sie beispielsweise ein Peering zwischen den virtuellen Netzwerken myVnetA und myVnetB erstellen, muss Ihr Konto mindestens die folgende Rolle oder die folgenden Berechtigungen für jedes virtuelle Netzwerk haben:
    
|Virtuelles Netzwerk|Bereitstellungsmodell|Rolle|Berechtigungen|
|---|---|---|---|
|myVnetA|Ressourcen-Manager|[Mitwirkender von virtuellem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klassisch|[Mitwirkender von klassischem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|–|
|myVnetB|Ressourcen-Manager|[Mitwirkender von virtuellem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klassisch|[Mitwirkender von klassischem Netzwerk](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Erfahren Sie mehr über [integrierte Rollen](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) und das Zuweisen bestimmter Berechtigungen zu [benutzerdefinierten Rollen](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (nur für Resource Manager).

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das Erstellen einer [Hub-and-Spoke-Netzwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering). 

