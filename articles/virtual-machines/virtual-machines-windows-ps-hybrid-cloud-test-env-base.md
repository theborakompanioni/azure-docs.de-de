<properties 
	pageTitle="Hybrid Cloud-Testumgebung | Microsoft Azure" 
	description="Erfahren Sie, wie Sie eine Hybrid Cloud-Umgebung für IT-Experten oder Entwicklungstests erstellen; vollständig mit einem vereinfachten lokalen Netzwerk." 
	services="virtual-machines-windows" 
	documentationCenter="" 
	authors="JoeDavies-MSFT" 
	manager="timlt" 
	editor=""
	tags="azure-resource-manager"/>

<tags 
	ms.service="virtual-machines-windows" 
	ms.workload="infrastructure-services" 
	ms.tgt_pltfrm="vm-windows" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/19/2016" 
	ms.author="josephd"/>

# Einrichten einer Hybrid Cloud-Umgebung zu Testzwecken
 
Dieses Thema führt Sie durch die Erstellung einer Hybrid Cloud-Umgebung mit Microsoft Azure für Tests. Die resultierende Konfiguration sieht folgendermaßen aus.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-base/virtual-machines-windows-ps-hybrid-cloud-test-env-base-ph5.png)

Damit wird eine echte hybride Produktionsumgebung von Ihrem Standort im Internet simuliert. Sie besteht aus:

-  einem vereinfachten lokalen Netzwerk (dem Corpnet-Subnetz).
-  Einem standortübergreifenden virtuellen in Azure gehosteten Netzwerk (TestVNET)
-  einer Site-to-Site-VPN-Verbindung.
-  Einem sekundären Domänencontroller im virtuellen Netzwerk TestVNET.

Diese Konfiguration bietet eine Grundlage und einen allgemeinen Ausgangspunkt für Folgendes:

-  Entwickeln und Testen von Anwendungen in einer Hybrid Cloud-Umgebung
-  Erstellen von Testkonfigurationen der Computer im Subnetz „Corpnet“ und im virtuellen Netzwerk TestVNET für Hybrid Cloud-basierte IT-Workloads

Das Einrichten dieser Hybrid Cloud-Testumgebung umfasst folgenden fünf Phasen:

1.	Konfigurieren der Computer im Subnetz „Corpnet“
2.	Konfigurieren von RRAS1
3.	Erstellen des standortübergreifenden virtuellen Azure-Netzwerks
4.	Erstellen der Site-to-Site-VPN-Verbindung
5.	Konfigurieren von DC2

Wenn Sie noch kein Azure-Abonnement besitzen, können Sie sich unter [Azure ausprobieren](https://azure.microsoft.com/pricing/free-trial/) für ein kostenloses Konto registrieren. Wenn Sie ein MSDN- oder Visual Studio-Abonnement besitzen, finden Sie weitere Informationen unter [Monatliche Azure-Gutschrift für Visual Studio-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

>[AZURE.NOTE] Für virtuelle Computer und virtuelle Netzwerkgateways in Azure fallen laufende Kosten an, wenn sie ausgeführt werden. Ein Azure-VPN-Gateway wird als Gruppe von zwei virtuellen Computern in Azure implementiert. Nähere Informationen hierzu finden Sie unter [Preise – Virtuelles Netzwerk](https://azure.microsoft.com/pricing/details/virtual-network/). Erstellen Sie die Testumgebung zum Verringern der Kosten für das Ausführen des VPN Gateways, und führen Sie die erforderlichen Tests und Demonstrationen so schnell wie möglich aus.

Diese Konfiguration erfordert ein Testsubnetz von bis zu vier Computern, die über eine öffentliche IP-Adresse direkt mit dem Internet verbunden sind. Wenn Sie nicht über diese Ressourcen verfügen, können Sie auch [eine simulierte Hybrid Cloud-Testumgebung einrichten](virtual-machines-windows-ps-hybrid-cloud-test-env-sim.md). Für die simulierte Hybrid Cloud-Testumgebung ist nur ein Azure-Abonnement erforderlich.

## Phase 1: Konfigurieren der Computer im Subnetz "Corpnet"

Befolgen Sie die Anweisungen im Abschnitt "Steps for Configuring the Corpnet Subnet" (Schritte zum Konfigurieren des Subnetzes Corpnet) im [Test Lab Guide: Base Configuration for Windows Server 2012 R2](http://www.microsoft.com/download/details.aspx?id=39638), um die Computer DC1, APP1 und CLIENT1 in einem Subnetz namens Corpnet zu konfigurieren. **Dieses Subnetz muss von Ihrem Organisationsnetzwerk isoliert werden, da es direkt über den RRAS1-Computer mit dem Internet verbunden wird.**

Melden Sie sich zunächst bei DC1 mit den Anmeldeinformationen von CORP\\User1 an. Führen Sie die folgenden Befehle von einer Windows PowerShell-Eingabeaufforderung aus, um die CORP-Domäne so zu konfigurieren, dass Computer und Benutzer ihren lokalen Domänencontroller zur Authentifizierung verwenden.

	New-ADReplicationSite -Name "TestLab" 
	New-ADReplicationSite -Name "TestVNET"
	New-ADReplicationSubnet “Name "10.0.0.0/8" “Site "TestLab"
	New-ADReplicationSubnet “Name "192.168.0.0/16" “Site "TestVNET

Die aktuelle Konfiguration sieht folgendermaßen aus.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-base/virtual-machines-windows-ps-hybrid-cloud-test-env-base-ph1.png)
 
## Phase 2: Konfigurieren von RRAS1

RRAS1 bietet Datenverkehrs-Routingdienste und VPN-Gerätedienste zwischen den Computern im Subnetz „Corpnet“ und im virtuellen Netzwerk TestVNET. Für RRAS1 müssen zwei Netzwerkkarten installiert sein.

Installieren Sie zunächst das Betriebssystem für RRAS1.

1.	Starten Sie die Installation von Windows Server 2012 R2.
2.	Befolgen Sie die Anweisungen zum Abschließen der Installation und geben Sie ein sicheres Kennwort für das lokale Administratorkonto an. Melden Sie sich mit dem lokalen Administratorkonto an.
3.	Stellen Sie eine Verbindung zwischen RRAS1 und einem Netzwerk her, das über Internetzugriff verfügt, und führen Sie Windows Update aus, um die neuesten Updates für Windows Server 2012 R2 zu installieren.
4.	Verbinden Sie eine Netzwerkkarte mit dem Subnetz „Corpnet“ und die andere direkt mit dem Internet. RRAS1 darf sich hinter einer Internetfirewall befinden, jedoch nicht hinter einer Netzwerkadressenübersetzung (network address translator, NAT).

Als Nächstes konfigurieren Sie die TCP/IP-Eigenschaften von RRAS1. Sie benötigen eine öffentliche IP-Adresskonfiguration, einschließlich einer Adresse, Subnetzmaske (oder Präfixlänge), und das Standardgateway und DNS-Server von Ihrem Internet-Service Provider (ISP). Sie benötigen die öffentliche IP-Adresse für Phase 3.

Verwenden Sie diese Befehle in einer Windows PowerShell-Eingabeaufforderung auf Administratorebene für RRAS1. Füllen Sie vor dem Ausführen dieser Befehle die Variablenwerte aus, und entfernen Sie die Zeichen < und >. Sie können die aktuellen Namen der Netzwerkkarten der Anzeige des Befehls **Get-NetAdapter** entnehmen.

	$corpnetAdapterName="<Name of the adapter attached to the Corpnet subnet>"
	$internetAdapterName="<Name of the adapter attached to the Internet>"
	[Ipaddress]$publicIP="<Your public IP address>"
	$publicIPpreflength=<Prefix length of your public IP address>
	[IPAddress]$publicDG="<Your ISP default gateway>"
	[IPAddress]$publicDNS="<Your ISP DNS server(s)>"
	Rename-NetAdapter -Name $corpnetAdapterName -NewName Corpnet
	Rename-NetAdapter -Name $internetAdapterName -NewName Internet
	New-NetIPAddress -InterfaceAlias "Internet" -IPAddress $publicIP -PrefixLength $publicIPpreflength “DefaultGateway $publicDG
	Set-DnsClientServerAddress -InterfaceAlias Internet -ServerAddresses $publicDNS
	New-NetIPAddress -InterfaceAlias "Corpnet" -IPAddress 10.0.0.2 -AddressFamily IPv4 -PrefixLength 24
	Set-DnsClientServerAddress -InterfaceAlias "Corpnet" -ServerAddresses 10.0.0.1
	Set-DnsClient -InterfaceAlias "Corpnet" -ConnectionSpecificSuffix corp.contoso.com
	New-NetFirewallRule -DisplayName "Allow ICMPv4-Input" -Protocol ICMPv4
	New-NetFirewallRule -DisplayName "Allow ICMPv4-Output" -Protocol ICMPv4 -Direction Outbound
	Disable-NetAdapterBinding -Name "Internet" -ComponentID ms_msclient
	Disable-NetAdapterBinding -Name "Internet" -ComponentID ms_server
	ping dc1.corp.contoso.com

Stellen Sie für den letzten Befehl sicher, dass es vier Antworten von der IP-Adresse 10.0.0.1 gibt.

Die aktuelle Konfiguration sieht folgendermaßen aus.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-base/virtual-machines-windows-ps-hybrid-cloud-test-env-base-ph2.png)

## Phase 3: Erstellen des standortübergreifenden virtuellen Azure-Netzwerks

Starten Sie eine Azure PowerShell-Eingabeaufforderung.

> [AZURE.NOTE] Die folgenden Befehlssätze verwenden Azure PowerShell 1.0 und höher. Weitere Informationen finden Sie unter [Azure PowerShell 1.0](https://azure.microsoft.com/blog/azps-1-0/) (in englischer Sprache).

Melden Sie sich bei Ihrem Konto an.

	Login-AzureRMAccount

Rufen Sie Ihren Abonnementnamen mit dem folgenden Befehl ab.

	Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Legen Sie Ihr Azure-Abonnement fest. Verwenden Sie das gleiche Abonnement, das Sie zum Erstellen der grundlegenden Konfiguration verwendet haben. Ersetzen Sie alles in den Anführungszeichen, einschließlich der Zeichen < und >, durch die korrekten Namen.

	$subscr="<subscription name>"
	Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Erstellen Sie als Nächstes eine neue Ressourcengruppe für die Hybridtestumgebung.

Um einen eindeutigen Namen für die Ressourcengruppe zu finden, verwenden Sie diesen Befehl zum Auflisten der vorhandenen Ressourcengruppen.

	Get-AzureRMResourceGroup | Sort ResourceGroupName | Select ResourceGroupName

Erstellen Sie die neue Ressourcengruppe mit diesen Befehlen.

	$rgName="<resource group name>"
	$locName="<an Azure location, such as West US>"
	New-AzureRMResourceGroup -Name $rgName -Location $locName

Virtuelle Computer auf Ressourcen-Manager-Basis benötigen ein Speicherkonto auf Ressourcen-Manager-Basis. Sie müssen für jedes Speicherkonto einen global eindeutigen Namen angeben, der nur aus Kleinbuchstaben und Zahlen besteht. Mit diesem Befehl können Sie die vorhandenen Speicherkonten auflisten.

	Get-AzureRMStorageAccount | Sort Name | Select Name

Erstellen Sie ein neues Speicherkonto mit diesen Befehlen.

	$rgName="<your new resource group name>"
	$locName="<the location of your new resource group>"
	$saName="<unique storage account name >"
	New-AzureRMStorageAccount -Name $saName -ResourceGroupName $rgName –Type Standard_LRS -Location $locName

Erstellen Sie als Nächstes das virtuelle Azure-Netzwerk, das Ihre Hybrid Cloud-Umgebung hosten wird, und schützen Sie es mit einer Netzwerksicherheitsgruppe.

	$locShortName="<the location of your new resource group in lowercase with spaces removed, example: westus>"
	$gwSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "192.168.255.248/29"
	$vmSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix "192.168.0.0/24"
	New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix "192.168.0.0/16" -Subnet $gwSubnet,$vmSubnet -DNSServer "10.0.0.1"
	$rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP to all VMs on the subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
	New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
	$vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name "TestVNET"
	$nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
	Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name TestSubnet -AddressPrefix "192.168.0.0/24" -NetworkSecurityGroup $nsg

Verwenden Sie diese Befehle, um die Gateways für die Site-to-Site-VPN-Verbindung zu erstellen. Sie benötigen die öffentliche IP-Adresse der Internetschnittstelle von RRAS1 aus Phase 2.

	$localGatewayIP="<the public IP address assigned to the Internet interface of RRAS1>"
	$vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
	
	# Attach a virtual network gateway to a public IP address and the gateway subnet
	$publicGatewayVipName="HybCloudPublicIPAddress"
	$vnetGatewayIpConfigName="HybCloudPublicIPConfig"
	New-AzureRMPublicIpAddress -Name $vnetGatewayIpConfigName -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
	$publicGatewayVip=Get-AzureRMPublicIpAddress -Name $vnetGatewayIpConfigName -ResourceGroupName $rgName
	$vnetGatewayIpConfig=New-AzureRMVirtualNetworkGatewayIpConfig -Name $vnetGatewayIpConfigName -PublicIpAddressId $publicGatewayVip.Id -SubnetId $vnet.Subnets[0].Id
	
	# Create the Azure gateway
	$vnetGatewayName="HybCloudAzureGateway"
	$vnetGateway=New-AzureRMVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $rgName -Location $locName -GatewayType Vpn -VpnType RouteBased -IpConfigurations $vnetGatewayIpConfig
	
	# Create the gateway for the local network
	$localGatewayName="HybCloudLocalNetGateway"
	$localNetworkPrefix="10.0.0.0/8"
	$localGateway=New-AzureRMLocalNetworkGateway -Name $localGatewayName -ResourceGroupName $rgName -Location $locName -GatewayIpAddress $localGatewayIP -AddressPrefix $localNetworkPrefix

Bedenken Sie, dass die Erstellung neuer Gateways 20 Minuten oder mehr in Anspruch nehmen kann.

Als Nächstes verwenden Sie den folgenden Befehl zum Ermitteln der öffentlichen IP-Adresse des Azure VPN Gateways für Ihr virtuelles Netzwerk.

	Get-AzureRMPublicIpAddress -Name $vnetGatewayIpConfigName -ResourceGroupName $rgName

Notieren Sie sich die IP-Adresse im Feld **IPAddress** der Anzeige. Sie benötigen diese für Phase 4.

Besorgen Sie sich als Nächstes einen zufällig generierten vorinstallierten Schlüssel mit 32 Zeichen und starker Kryptografie von Ihrem Netzwerk- oder Systemadministrator. Verwenden Sie zum Erhalten eines vorinstallierten Schlüssels alternativ die Informationen im Artikel [Create a random string for an IPsec preshared key](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) (Erstellen einer zufälligen Zeichenfolge für einen vorinstallierten IPsec-Schlüssel).

Verwenden Sie diese Befehle zum Erstellen der Site-to-Site-VPN-Verbindung in Azure.

	# Define the Azure virtual network VPN connection
	$vnetConnectionKey="<32-character pre-shared key>"
	$vnetConnectionName="HybCloudS2SConnection"
	$vnetConnection=New-AzureRMVirtualNetworkGatewayConnection -Name $vnetConnectionName -ResourceGroupName $rgName -Location $locName -ConnectionType IPsec -SharedKey $vnetConnectionKey -VirtualNetworkGateway1 $vnetGateway -LocalNetworkGateway2 $localGateway

Die aktuelle Konfiguration sieht folgendermaßen aus.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-base/virtual-machines-windows-ps-hybrid-cloud-test-env-base-ph3.png)
 
## Phase 4: Erstellen der Site-to-Site-VPN-Verbindung

Konfigurieren Sie zunächst RRAS1 mit dem Routing- und RAS-Dienst als VPN-Gerät für das Subnetz „Corpnet“. Melden Sie sich als lokaler Administrator bei RRAS1 an, und führen Sie folgende Befehle in einer Windows PowerShell-Eingabeaufforderung aus.

	Import-Module ServerManager
	Install-WindowsFeature RemoteAccess -IncludeManagementTools
	Add-WindowsFeature -name Routing -IncludeManagementTools

Als Nächstes konfigurieren Sie RRAS1, um die Site-to-Site-VPN-Verbindung vom Azure-VPN-Gateway zu erhalten. Starten Sie RRAS1 neu, melden Sie sich als lokaler Administrator an, und führen Sie folgende Befehle in einer Windows PowerShell-Eingabeaufforderung aus. Sie müssen die öffentliche IP-Adresse des Azure VPN Gateways und den Wert des vorinstallierten Schlüssels aus Phase 3 angeben.

	$PresharedKey="<32-character pre-shared key>"
	Import-Module RemoteAccess
	Install-RemoteAccess -VpnType VpnS2S
	Add-VpnS2SInterface -Protocol IKEv2 -AuthenticationMethod PSKOnly -Persistent -NumberOfTries 3 -ResponderAuthenticationMethod PSKOnly -Name S2StoTestVNET -Destination "<public IP address of the Azure VPN gateway>" -IPv4Subnet @("192.168.0.0/24:100") -SharedSecret $PresharedKey
	Set-VpnServerIPsecConfiguration -EncryptionType MaximumEncryption
	Set-VpnServerIPsecConfiguration -SADataSizeForRenegotiationKilobytes 33553408
	New-ItemProperty -Path HKLM:\System\CurrentControlSet\Services\RemoteAccess\Parameters\IKEV2 -Name SkipConfigPayload -PropertyType DWord -Value 1
	Restart-Service RemoteAccess

Warten Sie einige Minuten, bis die Verbindung zwischen RRAS1 und dem Azure VPN Gateway hergestellt ist.

Als Nächstes konfigurieren Sie RRAS1 zur Unterstützung von übersetztem Datenverkehr für Internetspeicherorte. Führen Sie für RRAS1 folgende Schritte aus:

1.	Geben Sie im Startbildschirm **rras** ein, und klicken Sie dann auf **Routing und Remotezugriff**.
2.	Öffnen Sie in der Konsolenstruktur den Servernamen, und klicken Sie dann auf **IPv4**.
3.	Klicken Sie mit der rechten Maustaste auf **Allgemein**, und klicken Sie dann auf **Neues Routingprotokoll**.
4.	Klicken Sie auf **NAT** und anschließend auf **OK**.
5.	Klicken Sie in der Konsolenstruktur mit der rechten Maustaste auf **NAT**, klicken Sie dann auf **Neue Schnittstelle**, anschließend auf **Corpnet**, und klicken Sie dann zwei Mal auf **OK**.
6.	Klicken Sie mit der rechten Maustaste auf **NAT**, dann auf **Neue Schnittstelle**, anschließend auf **Internet**, und klicken Sie dann auf **OK**.
7.	Klicken Sie auf der Registerkarte **NAT** auf **An das Internet angeschlossene, öffentliche Schnittstelle**, wählen Sie **NAT auf dieser Schnittstelle aktivieren**, und klicken Sie dann auf **OK**.

Als Nächstes konfigurieren Sie DC1, APP1 und CLIENT1, um RRAS1 als Standardgateway zu verwenden.
 
Führen Sie für DC1 folgende Befehle in der Windows PowerShell-Eingabeaufforderung auf Administratorebene aus.

	New-NetRoute -DestinationPrefix "0.0.0.0/0" -InterfaceAlias "Ethernet" -NextHop 10.0.0.2
	Set-DhcpServerv4OptionValue -Router 10.0.0.2

Wenn der Name der Schnittstelle nicht Ethernet ist, verwenden Sie den **Get-NetAdapter**-Befehl, um den Schnittstellennamen zu bestimmen.

Führen Sie für APP1 folgenden Befehl in der Windows PowerShell-Eingabeaufforderung auf Administratorebene aus.

	New-NetRoute -DestinationPrefix "0.0.0.0/0" -InterfaceAlias "Ethernet" -NextHop 10.0.0.2

Führen Sie für CLIENT1 den folgenden Befehl in der Windows PowerShell-Eingabeaufforderung auf Administratorebene aus.

	ipconfig /renew

Die aktuelle Konfiguration sieht folgendermaßen aus.
 

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-base/virtual-machines-windows-ps-hybrid-cloud-test-env-base-ph4.png)


## Phase 5: Konfigurieren von DC2

Führen Sie als Erstes auf dem lokalen Computer die folgenden Befehle in der Azure PowerShell-Eingabeaufforderung aus, um einen virtuellen Azure-Computer für DC2 zu erstellen.

	$rgName="<your resource group name>"
	$locName="<your Azure location, such as West US>"
	$saName="<your storage account name>"
	
	$vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
	$subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
	$pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
	$nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip -PrivateIpAddress 192.168.0.4
	$vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
	$storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
	$vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
	Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
	$cred=Get-Credential -Message "Type the name and password of the local administrator account for DC2."
	$vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
	$vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
	$vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
	$osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
	$vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
	New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Verwenden Sie als Nächstes das Azure-Portal zum Herstellen einer Verbindung mit dem neuen DC2 (virtueller Computer) mithilfe der Anmeldeinformationen des lokalen Administratorkontos.

Konfigurieren Sie anschließend eine Windows-Firewall-Regel, um Datenverkehr zum Testen der allgemeinen Konnektivität zuzulassen. Führen Sie auf DC2 in einer Windows PowerShell-Eingabeaufforderung auf Administratorebene den folgenden Befehl aus:

	Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
	ping dc1.corp.contoso.com

Der Ping-Befehl sollte zu vier erfolgreichen Antworten von der IP-Adresse 10.0.0.1 führen. Falls Sie die *simulierte Hybrid Cloud-Konfiguration* verwenden, sollten Sie vier erfolgreiche Antworten von der IP-Adresse 10.0.0.4 sehen. Dies ist ein Test des Datenverkehrs über die Site-to-Site-VPN- oder die VNet-zu-VNet-Verbindung.

Fügen Sie anschließend einen zusätzlichen Datenträger als neues Volume mit dem Laufwerkbuchstaben F: hinzu.

1.	Klicken Sie im linken Bereich des Server-Managers auf **Datei- und Speicherdienste** und anschließend auf **Datenträger**.
2.	Klicken Sie im Inhaltsbereich in der Gruppe **Datenträger** auf **Datenträger 2** (wobei **Partition** die Einstellung **Unbekannt** aufweisen muss).
3.	Klicken Sie auf **Aufgaben** und anschließend auf **Neues Volume**.
4.	Klicken Sie auf der Seite „Voraussetzungen“ des Assistenten für neue Volumes auf **Weiter**.
5.	Klicken Sie auf der Seite „Server und Datenträger auswählen“ auf **Datenträger 2** und anschließend auf **Weiter**. Wenn Sie dazu aufgefordert werden, klicken Sie auf **OK**.
6.	Klicken Sie auf der Seite „Geben Sie die Größe des Volumes an“ auf **Weiter**.
7.	Klicken Sie auf der Seite „Einem Laufwerkbuchstaben oder Ordner zuweisen“ auf **Weiter**.
8.	Klicken Sie auf der Seite „Dateisystemeinstellungen auswählen“ auf **Weiter**.
9.	Klicken Sie auf der Seite „Auswahl bestätigen“ auf **Erstellen**.
10.	Wenn Sie fertig sind, klicken Sie auf **Schließen**.

Konfigurieren Sie als Nächstes DC2 als replizierten Domänencontroller für die corp.contoso.com-Domäne. Führen Sie in der Windows PowerShell-Eingabeaufforderung von DC2 die folgenden Befehle aus.

	Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
	Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

Beachten Sie, dass Sie zur Angabe des Kennworts für CORP\\User1 und eines Kennworts für den Verzeichnisdienst-Wiederherstellungsmodus (DSRM) und zum Neustart von DC2 aufgefordert werden.

Da das virtuelle Netzwerk TestVNET nun über einen eigenen DNS-Server (DC2) verfügt, müssen Sie das virtuelle Netzwerk TestVNET zur Verwendung dieses DNS-Servers konfigurieren.

1.	Klicken Sie im linken Bereich des Azure-Portals auf das Symbol für virtuelle Netzwerke, und klicken Sie dann auf **TestVNET**.
2.	Klicken Sie auf der Registerkarte **Einstellungen** auf **DNS-Server**.
3.	Geben Sie unter **Primärer DNS-Server** die Adresse **192.168.0.4** ein, und ersetzen Sie dadurch die Adresse „10.0.0.4“.
4.	Klicken Sie auf Speichern.

Starten Sie als Nächstes den DC2, damit dieser die neue DNS-Server-Konfiguration mit diesen Befehlen bei der Azure PowerShell-Eingabeaufforderung verwendet.

	Stop-AzureRmVM -ResourceGroupName $rgName -Name DC2
	Start-AzureRmVM -ResourceGroupName $rgName -Name DC2

Die aktuelle Konfiguration sieht folgendermaßen aus.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-base/virtual-machines-windows-ps-hybrid-cloud-test-env-base-ph5.png)

Die Hybrid Cloud-Umgebung kann nun getestet werden.
 
## Nächste Schritte

- Richten Sie in dieser Umgebung eine [SharePoint-Intranetfarm](virtual-machines-windows-ps-hybrid-cloud-test-env-sp.md), eine [webbasierte Branchenanwendung](virtual-machines-windows-ps-hybrid-cloud-test-env-lob.md) oder einen [Office 365-Verzeichnissynchronisierungsserver (DirSync)](virtual-machines-windows-ps-hybrid-cloud-test-env-dirsync.md) ein.

<!---HONumber=AcomDC_0720_2016-->