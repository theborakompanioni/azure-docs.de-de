<properties 
   pageTitle="Accelerated Networking für einen virtuellen Computer – PowerShell | Microsoft Azure"
   description="Erfahren Sie etwas über das Konfigurieren von Accelerated Networking für einen virtuellen Azure-Computer mithilfe von PowerShell."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="jdial" />

# Accelerated Networking für virtuelle Computer

Accelerated Networking ermöglicht die E/A-Virtualisierung mit Einzelstamm (Single Root I/O Virtualization, SR-IOV) in einem virtuellen Computer (VM) und somit eine erhebliche Steigerung der Netzwerkleistung. Über diesen Hochleistungspfad wird der Host des Datenpfads umgangen, um Latenzen, Jitter und CPU-Auslastung zu verringern. So können auf unterstützten VM-Typen die anspruchsvollsten Netzwerkworkloads genutzt werden. Dieser Artikel erläutert die Verwendung von PowerShell für das Konfigurieren von Accelerated Networking im Azure Resource Manager-Bereitstellungsmodell.

Die folgende Abbildung zeigt die Kommunikation zwischen zwei virtuellen Computern (VM) mit und ohne Accelerated Networking:

![Vergleich](./media/virtual-network-accelerated-networking-powershell/image1.png)

Ohne Accelerated Networking muss sämtlicher Netzwerkdatenverkehr zur und von der VM den Host und den virtuellen Switch passieren. Der virtuelle Switch setzt alle Richtlinien für den Netzwerkdatenverkehr um, z.B. Netzwerksicherheitsgruppen, Zugriffssteuerungslisten, Isolation und andere virtualisierte Netzwerkdienste. Weitere Informationen finden Sie im Artikel [Hyper-V Network Virtualization and Virtual Switch](https://technet.microsoft.com/library/jj945275.aspx) (Hyper-V-Netzwerkvirtualisierung und virtueller Switch).

Mit Accelerated Networking gelangt der Netzwerkdatenverkehr zur Netzwerkkarte (NIC) und wird dann an den virtuellen Computer weitergeleitet. Alle Netzwerkrichtlinien, die ohne Accelerated Networking vom virtuellen Switch angewendet werden, werden ausgelagert und in der Hardware angewendet. Durch das Anwenden von Richtlinien in der Hardware kann die NIC Netzwerkdatenverkehr direkt an den virtuellen Computer weiterleiten und dabei den Host und den virtuellen Switch umgehen, während gleichzeitig sämtliche Richtlinien umgesetzt werden, die sonst auf dem Host angewendet worden wären.

Die Vorteile des Accelerated Networking gelten nur für den virtuellen Computer, auf dem dieses Feature aktiviert ist. Die besten Ergebnisse erzielen Sie, wenn Sie dieses Feature auf mindestens zwei virtuellen Computern aktivieren, die mit demselben VNET verbunden sind. Bei der Kommunikation über VNETs oder bei lokalen Verbindungen hat dieses Feature nur minimale Auswirkungen auf die Latenz.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

##Vorteile

- **Geringere Latenz/mehr Pakete pro Sekunde (pps):** Durch das Entfernen des virtuellen Switchs aus dem Datenpfad wird die Zeit reduziert, die Pakete auf dem Host für die Verarbeitung von Richtlinien warten mussten. Gleichzeitig wird die Anzahl der Pakete erhöht, die im virtuellen Computer verarbeitet werden können.
- **Reduzierte Jitter:** Die Verarbeitung im virtuellen Switch hängt vom Umfang der anzuwendenden Richtlinien ab und von der Workload der CPU, die die Verarbeitung durchführt. Das Auslagern der Richtlinienerzwingung auf die Hardware entfernt diesen Umweg, indem die Pakete direkt an den virtuellen Computer gesendet werden. Damit werden der Host und alle Softwareinterrupts und Kontextwechsel aus der VM-Kommunikation entfernt.
- **Verringerte CPU-Auslastung:** Das Umgehen des virtuellen Switchs auf dem Host führt zu weniger CPU-Auslastung für die Verarbeitung des Netzwerkdatenverkehrs.

## Einschränkungen

Die folgenden Einschränkungen gelten für die Verwendung dieser Funktion:
 
- **Erstellung von Netzwerkschnittstellen:** Accelerated Networking kann nur für eine neue Netzwerkschnittstelle aktiviert werden. Auf einer vorhandenen Netzwerkschnittstelle kann die Funktion nicht aktiviert werden.
- **VM-Erstellung:** Eine Netzwerkschnittstelle, auf der Accelerated Networking aktiviert ist, kann nur an eine VM angefügt werden, während diese VM erstellt wird. Die Netzwerkschnittstelle kann nicht an vorhandene virtuelle Computer angefügt werden.
- **Regionen:** Dieses Angebot gilt nur in den Regionen USA, Westen-Mitte und Europa, Westen. Die Anzahl der Regionen wird in Zukunft noch erweitert.
- **Unterstützte Betriebssysteme:** Microsoft Windows Server 2012 R2 und Windows Server 2016 Technical Preview 5. Die Unterstützung für Linux und Windows Server 2012 wird in Kürze hinzugefügt.
- **VM-Größe:** Es werden nur die VM-Instanzgrößen Standard\_D15\_v2 und Standard\_DS15\_v2 unterstützt. Weitere Informationen finden Sie im Artikel [Windows-VM-Größen](../virtual-machines/virtual-machines-windows-sizes.md). Die Anzahl von unterstützten VM-Instanzgrößen wird in Zukunft erweitert werden.

Änderungen an diesen Einschränkungen werden auf der Seite [Azure-Updates für Virtual Network](https://azure.microsoft.com/updates/?product=virtual-network&updatetype=&platform=) bekannt gemacht.

## Erstellen einer Windows-VM mit Accelerated Networking

1. Öffnen Sie eine PowerShell-Eingabeaufforderung, und führen Sie die restlichen Schritte in diesem Abschnitt innerhalb einer einzelnen PowerShell-Sitzung aus. Wenn Sie PowerShell nicht installiert und konfiguriert haben, befolgen Sie die Anweisungen im Artikel [Gewusst wie: Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).
2. Wenn Sie sich für die Vorschau registrieren möchten, senden Sie eine E-Mail an [Accelerated Networking-Abonnements](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e), und geben Sie Ihre Abonnement-ID und die beabsichtigte Verwendung an.
3. Sie registrieren die Funktion mit Ihrem Abonnement, indem Sie die folgenden Befehle eingeben:

		Register-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingFeature -ProviderNamespace Microsoft.Network
		Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network

4. Ersetzen Sie ggf. *westcentralus* durch den Namen eines anderen Standorts, der im Abschnitt [Einschränkungen](#limitations) dieses Artikels als unterstützte Region aufgeführt wird. Geben Sie den folgenden Befehl ein, um eine Variable für den Standort festzulegen:

		$locName = "westcentralus"

5. Ersetzen Sie *RG1* durch einen Namen für die Ressourcengruppe, die die neue Netzwerkschnittstelle enthalten wird, und geben Sie die folgenden Befehle ein, um sie zu erstellen:

		$rgName = "RG1"
		New-AzureRmResourceGroup -Name $rgName -Location $locName

6. Ändern Sie *VM1 NIC1* in den neuen Namen der Netzwerkschnittstelle aus, und geben dann den folgenden Befehl ein:

		$NICName = "VM1-NIC1"

7. Die Netzwerkschnittstelle muss über eine Verbindung mit einem Subnetz innerhalb einer vorhandenen Azure Virtual Network-Instanz (VNET) am gleichen Standort und im gleichen [Abonnement](../azure-glossary-cloud-terminology.md#subscription) wie die Netzwerkschnittstelle verfügen. Sofern Sie nicht mit VNETs vertraut sind, finden Sie weitere Informationen im Artikel [Übersicht über virtuelle Netzwerke](virtual-networks-overview.md). Wenn Sie ein VNET erstellen, führen Sie die Schritte im Artikel [Erstellen eines VNET](virtual-networks-create-vnet-arm-ps.md) aus. Ändern Sie die Werte der folgenden Variablen in den Namen des VNET und des Subnetzes, mit dem Sie die Netzwerkschnittstelle verbinden möchten, und den Namen der Ressourcengruppe, zu der das VNET gehört.

		$VNetName   = "VNet1"
		$SubnetName = "Subnet1"

	Wenn Ihnen kein Name eines VNET an dem in Schritt 3 ausgewählten Standort bekannt ist, geben Sie die folgenden Befehle ein:
		
		$VirtualNetworks = Get-AzureRmVirtualNetwork
		$VirtualNetworks | Where-Object {$_.Location -eq $locName} | Format-Table Name, Location
		
	Ist die zurückgegebene Liste leer, müssen Sie am Standort ein VNET erstellen. Wenn Sie ein VNET erstellen, führen Sie die Schritte im Artikel [Erstellen eines virtuellen Netzwerks](virtual-networks-create-vnet-arm-ps.md) aus.

	Um den Namen der Subnetze im VNET abzurufen, geben Sie die folgenden Befehle ein. Ersetzen Sie dabei *Subnet1* oben durch den Namen eines Subnetzes:
		
		$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
		$VNet.Subnets | Format-Table Name, AddressPrefix

8. Geben Sie die folgenden Befehle ein, um das VNET und das Subnetz abzurufen, und weisen Sie sie Variablen zu.

		$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
		$Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

9. Ermitteln Sie eine vorhandene öffentliche IP-Adressressource, die der Netzwerkschnittstelle zugeordnet werden kann, damit Sie über das Internet eine Verbindung herstellen können. Wenn Sie nicht über das Internet auf den virtuellen Computer mit der Netzwerkschnittstelle zugreifen möchten, können Sie diesen Schritt überspringen. Ohne eine öffentliche IP-Adresse müssen Sie die Verbindung mit diesem virtuellen Computer von einem anderen virtuellen Computer im gleichen VNET aus herstellen.

	Ändern Sie *PIP1* in den Namen einer vorhandenen öffentlichen IP-Adressressource, die an dem Standort, an dem Sie die Netzwerkschnittstelle erstellen, vorhanden und zurzeit keiner anderen Netzwerkschnittstelle zugeordnet ist. Ändern Sie ggf. $rgName in den Namen der Ressourcengruppe, zu der die öffentliche IP-Adressressource gehört, und geben Sie den folgenden Befehl ein:

		$PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $rgName

	Geben Sie die folgenden Befehle ein, wenn Sie den Namen einer vorhandenen öffentlichen IP-Adressressource nicht kennen:

		$PublicIPAddresses = Get-AzureRmPublicIPAddress
		$PublicIPAddresses | Where-Object { $_.Location -eq $locName } |Format-Table Name, Location, IPAddress, IpConfiguration

	Wenn die Spalte **IPConfiguration** in der zurückgegebenen Ausgabe keinen Wert enthält, ist die öffentliche IP-Adressressource keiner vorhandenen Netzwerkschnittstelle zugeordnet und kann daher verwendet werden. Wenn die Liste leer ist oder keine öffentlichen IP-Adressressourcen verfügbar sind, können Sie mit dem Befehl New-AzureRmPublicIPAddress eine erstellen.

	>[AZURE.NOTE] Für öffentliche IP-Adressen fällt eine geringe Gebühr an. Weitere Informationen zu den Preisen finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses).
10. Wenn Sie der Schnittstelle keine öffentliche IP-Adressressource hinzufügen möchten, entfernen Sie *-PublicIPAddress $PIP1* am Ende des folgenden Befehls. Sie erstellen die Netzwerkschnittstelle mit Accelerated Networking, indem Sie den folgenden Befehl eingeben:

		$nic = New-AzureRmNetworkInterface -Location $locName -Name $NICName -ResourceGroupName $rgName -Subnet $Subnet -EnableAcceleratedNetworking -PublicIpAddress $PIP1 

11. Sie weisen die Netzwerkschnittstelle einem virtuellen Computer beim Erstellen hinzu, indem Sie die Schritte 3 und 6 im Artikel [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-ps-create.md) befolgen. Ersetzen Sie in Schritt 6-2 *Standard\_A1* durch eine der VM-Größen, die im Abschnitt [Einschränkungen](#limitations) dieses Artikels angegeben werden.

	>[AZURE.NOTE] Wenn Sie den *Namen* der Variablen $locName, $rgName oder $nic in diesem Artikel geändert haben, schlägt Schritt 6 im Artikel „Erstellen eines virtuellen Computers“ fehl. Sie können die *Werte* der Variablen jedoch ändern.

12. Laden Sie nach dem Erstellen des virtuellen Computers den [Accelerated Networking-Treiber](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84) herunter, stellen Sie eine Verbindung mit dem virtuellen Computer her, und führen Sie das Treiberinstallationsprogramm innerhalb des virtuellen Computers aus.

13. Klicken Sie mit der rechten Maustaste auf die Windows-Schaltfläche, und klicken Sie auf **Geräte-Manager**. Vergewissern Sie sich, dass der **Mellanox ConnectX-3 Virtual Function Ethernet Adapter** wie in der folgenden Abbildung gezeigt unterhalb der Option **Netzwerkadapter** angezeigt wird, wenn Sie diese erweitern:

	![Geräte-Manager](./media/virtual-network-accelerated-networking-powershell/image2.png)

<!---HONumber=AcomDC_0928_2016-->