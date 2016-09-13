<properties 
   pageTitle="Konfigurieren einer Point-to-Site-VPN-Gatewayverbindung mit einem virtuellen Netzwerk mithilfe des Resource Manager-Bereitstellungsmodells | Microsoft Azure"
   description="Stellen Sie eine sichere Verbindung mit Ihrem Azure Virtual Network her, indem Sie eine P2S-VPN-Gatewayverbindung herstellen."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc" />

# Konfigurieren einer Point-to-Site-VPN-Verbindung (P2S) mit einem VNet mithilfe von PowerShell

> [AZURE.SELECTOR]
- [PowerShell – Resource Manager](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Portal – klassisch](vpn-gateway-point-to-site-create.md)

Mit einer P2S-Konfiguration (Point-to-Site) können Sie von einem einzelnen Clientcomputer eine sichere Verbindung mit einem virtuellen Netzwerk herstellen. Eine P2S-Verbindung ist nützlich, wenn Sie von einem Remotestandort, z.B. von zu Hause oder in einer Konferenz, eine Verbindung mit Ihrem VNet herstellen möchten. Diese Methode eignet sich auch, wenn Sie nur wenige Clients besitzen, die mit einem virtuellen Netzwerk verbunden werden müssen.

Dieser Artikel führt Sie durch die Erstellung eines VNet mit einer P2S-Verbindung im **Resource Manager-Bereitstellungsmodell**. Für diese Schritte ist PowerShell erforderlich. Derzeit ist es nicht möglich, diese Lösung komplett über das Azure-Portal zu erstellen.

Damit P2S-Verbindungen funktionieren, ist kein VPN-Gerät und keine öffentliche IP-Adresse erforderlich. Eine VPN-Verbindung wird hergestellt, indem Sie die Verbindung vom Clientcomputer aus starten. Weitere Informationen zu P2S-Verbindungen finden Sie unter [Häufig gestellte Fragen zum VPN Gateway](vpn-gateway-vpn-faq.md#point-to-site-connections) und [Planung und Entwurf](vpn-gateway-plan-design.md).



**Informationen zu Azure-Bereitstellungsmodellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

**Bereitstellungsmodelle und Tools für Punkt-zu-Standort-Verbindungen**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)]

![P2S-Diagramm](./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "Point-to-Site")


## Informationen zu dieser Konfiguration.

In diesem Szenario erstellen Sie ein virtuelles Netzwerk mit einer P2S-Verbindung. Diese Anweisungen helfen Ihnen auch beim Erstellen von Zertifikaten, die für diese Konfiguration erforderlich sind. Eine P2S-Verbindung besteht aus folgenden Elementen: einem VNet mit einem VPN-Gateway, einer CER-Stammzertifikatdatei (öffentlicher Schlüssel), einem Clientzertifikat und der VPN-Konfiguration auf dem Client.

Für diese Konfiguration verwenden wir die folgenden Werte: Wir legen die Variablen im Abschnitt [1](#declare) des Artikels fest. Sie können diese Schritte entweder als exemplarische Vorgehensweise durchlaufen und die Werte nicht ändern, oder Sie können die Werte Ihrer Umgebung entsprechend ändern.

- Name: **VNet1**, mit den Adressbereichen **192.168.0.0/16** und **10.254.0.0/16**. Beachten Sie, dass Sie für ein VNet mehrere Adressräume verwenden können.
- Subnetzname: **FrontEnd**, verwendet **192.168.1.0/24**
- Subnetzname: **BackEnd**, verwendet **10.254.1.0/24**
- Subnetzname: **GatewaySubnet**, verwendet **192.168.200.0/24**. Der Subnetzname *GatewaySubnet* ist erforderlich, damit das Gateway funktioniert.
- VPN-Clientadresspool: **172.16.201.0/24**. VPN-Clients, die sich über diese Punkt-zu-Standort-Verbindung mit dem VNet verbinden, erhalten eine IP-Adresse aus diesem Pool.
- Abonnement: Überprüfen Sie, ob Sie das richtige Abonnement verwenden, falls Sie mehrere besitzen.
- Ressourcengruppe: **TestRG**
- Standort: **USA, Osten**
- DNS-Server: **IP-Adresse** des DNS-Servers, der für die Namensauflösung verwendet werden soll.
- GW-Name: **GW**
- Name der öffentlichen IP: **GWIP**
- VpnType: **RouteBased**


## Vorbereitungen

- Stellen Sie sicher, dass Sie über ein Azure-Abonnement verfügen. Wenn Sie noch kein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) registrieren.
	
- Installieren Sie die Azure Resource Manager PowerShell-Cmdlets (1.0.2 oder höher). Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Bei der Verwendung von PowerShell für diese Konfiguration müssen Sie PowerShell als Administrator ausführen.

## <a name="declare"></a>Teil 1 – Anmelden und Festlegen von Variablen

In diesem Abschnitt melden Sie sich an und deklarieren die für diese Konfiguration verwendeten Werte. Die deklarierten Werte werden in den Beispielskripts verwendet. Ändern Sie die Werte Ihrer Umgebung entsprechend. Alternativ können Sie diese Schritte mit den deklarierten Werten als Übung durchlaufen.

1. Melden Sie sich in der PowerShell-Konsole bei Ihrem Azure-Konto an. Sie werden von diesem Cmdlet zur Eingabe der Anmeldeinformationen für Ihr Azure-Konto aufgefordert. Nach dem Anmelden werden Ihre Kontoeinstellungen heruntergeladen, damit sie Azure PowerShell zur Verfügung stehen.

		Login-AzureRmAccount 

2. Rufen Sie eine Liste Ihrer Azure-Abonnements ab.

		Get-AzureRmSubscription

3. Geben Sie das Abonnement an, das Sie verwenden möchten.

		Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. Deklarieren Sie die gewünschten Variablen. Verwenden Sie das unten gezeigte Beispiel, und ersetzen Sie die Werte nach Bedarf durch Ihre eigenen.

		$VNetName  = "VNet1"
		$FESubName = "FrontEnd"
		$BESubName = "Backend"
		$GWSubName = "GatewaySubnet"
		$VNetPrefix1 = "192.168.0.0/16"
		$VNetPrefix2 = "10.254.0.0/16"
		$FESubPrefix = "192.168.1.0/24"
		$BESubPrefix = "10.254.1.0/24"
		$GWSubPrefix = "192.168.200.0/26"
		$VPNClientAddressPool = "172.16.201.0/24"
		$RG = "TestRG"
		$Location = "East US"
		$DNS = "8.8.8.8"
		$GWName = "GW"
		$GWIPName = "GWIP"
		$GWIPconfName = "gwipconf"
    	$P2SRootCertName = "ARMP2SRootCert.cer"


## Teil 2 – Konfigurieren eines VNET 


1. Erstellen Sie eine Ressourcengruppe.

		New-AzureRmResourceGroup -Name $RG -Location $Location

2. Erstellen Sie die Subnetzkonfigurationen für das virtuelle Netzwerk, und benennen Sie sie mit *FrontEnd*, *BackEnd* und *GatewaySubnet*. Diese Präfixe müssen im oben deklarierten VNet-Adressraum enthalten sein.

		$fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
		$besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
		$gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix

3. Erstellen des virtuellen Netzwerks Der angegebene DNS-Server muss dazu in der Lage sein, die Namen für die Ressourcen aufzulösen, mit denen Sie eine Verbindung herstellen möchten. In diesem Beispiel verwenden wir eine öffentliche IP-Adresse. Achten Sie darauf, dass Sie Ihre eigenen Werte verwenden.
	
		New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS

4. Geben Sie die Variablen für das virtuelle Netzwerk an, das Sie erstellt haben.

		$vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
		$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

5. Fordern Sie eine dynamisch zugewiesene öffentliche IP-Adresse an. Diese IP-Adresse ist erforderlich, damit das Gateway ordnungsgemäß funktioniert. Später verbinden Sie das Gateway mit der Gateway-IP-Konfiguration.
		
		$pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
		$ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

## Teil 3 – Hinzufügen vertrauenswürdiger Zertifikate

Azure authentifiziert Clients, die eine Verbindung über P2S herstellen möchten, mithilfe von Zertifikaten. Importieren Sie die CER-Datei (den öffentlichen Schlüssel) für ein Stammzertifikat in Azure. Wenn Sie Azure eine Base64-codierte x.509-Datei (CER-Datei) hinzufügen, weisen Sie Azure dadurch an, dem Stammzertifikat zu vertrauen, das die Datei darstellt.

Bei einer Unternehmenslösung können Sie Ihre vorhandene Zertifikatkette verwenden. Wenn Sie keine Lösung einer Unternehmenszertifizierungsstelle verwenden, können Sie ein selbstsigniertes Stammzertifikat erstellen. Makecert ist eine Methode zum Erstellen eines selbstsignierten Zertifikats. Eine Anleitung zum Erstellen eines selbstsignierten Stammzertifikats mithilfe von *makecert* finden Sie unter [Arbeiten mit selbstsignierten Stammzertifikaten für Punkt-zu-Standort-Konfigurationen](vpn-gateway-certificates-point-to-site.md).

Unabhängig davon, wie Sie das Zertifikat abrufen, laden Sie die CER-Datei für das Zertifikat in Azure hoch und erstellen Clientzertifikate, die auf den Clients installiert werden, welche eine Verbindung herstellen möchten. Das selbstsignierte Zertifikat wird nicht direkt auf dem Client installiert. Weiter unten in diesem Artikel im Abschnitt zur [Clientkonfiguration](#cc) erfahren Sie, wie Sie Clientzertifikate erstellen.
		
Sie können Azure bis zu 20 vertrauenswürdige Zertifikate hinzufügen. Exportieren Sie das Zertifikat als Base64-codierte X.509-Datei (CER-Datei), um den öffentlichen Schlüssel abzurufen. Eine Möglichkeit zum Exportieren in eine CER-Datei besteht darin, **certmger.msc** zu öffnen und das Zertifikat unter „Persönlich/Zertifikate“ zu suchen. Klicken Sie mit der rechten Maustaste darauf, und exportieren Sie es ohne den privaten Schlüssel als „Base-64-codiert X.509 (.CER)“. Notieren Sie sich den Pfad der exportierten CER-Datei. Dies ist ein Beispiel für das Abrufen der Base64-Zeichenfolgendarstellung Ihres Zertifikats.

Fügen Sie das vertrauenswürdige Zertifikat zu Azure hinzu. Für diesen Schritt müssen Sie Ihren eigenen CER-Dateipfad verwenden. Achten Sie besonders auf die Variable „$P2SRootCertName = "ARMP2SRootCert.cer"“, die Sie in Teil 1 dieses Artikels festgelegt haben. Wenn der Name Ihres Zertifikats davon abweicht, passen Sie die Variable entsprechend an.
    
		$filePathForCert = "pasteYourCerFilePathHere"
		$cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
		$CertBase64 = [system.convert]::ToBase64String($cert.RawData)
		$p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## Teil 4 – Erstellen des VPN-Gateways

Konfigurieren und erstellen Sie das virtuelle Netzwerkgateway für Ihr VNet. *-GatewayType* muss **Vpn** sein, und *-VpnType* muss **RouteBased** lauten. Dies kann bis zu 45 Minuten dauern.

		New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
		-Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
		-VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
		-VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## Teil 5 – Herunterladen des Konfigurationspakets für VPN-Clients

Clients, die mithilfe von P2S eine Verbindung mit Azure herstellen, benötigen ein Clientzertifikat sowie ein installiertes VPN-Clientkonfigurationspaket. Konfigurationspakete für VPN-Clients stehen für Windows-Clients zur Verfügung. Das VPN-Clientpaket enthält Informationen zur Konfiguration der VPN-Clientsoftware, die in Windows integriert ist und speziell für das VPN gilt, mit dem Sie eine Verbindung herstellen möchten. Das Paket installiert keine zusätzlichen Software. Weitere Informationen finden Sie unter [Häufig gestellte Fragen zum VPN-Gateway](vpn-gateway-vpn-faq.md#point-to-site-connections).

1. Nachdem Sie das Gateway erstellt haben, verwenden Sie das folgende Beispiel zum Herunterladen des Clientkonfigurationspakets:

		Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
		-VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

2. Das PowerShell-Cmdlet gibt einen URL-Link zurück. Dies ist ein Beispiel dafür, wie die zurückgegebene URL aussieht:

    	"https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"

3. Kopieren Sie den zurückgegebenen Link, und fügen Sie ihn in einen Webbrowser ein, um das Paket herunterzuladen. Installieren Sie anschließend das Paket auf dem Clientcomputer.

4. Navigieren Sie auf dem Clientcomputer zu **Netzwerkeinstellungen**, und klicken Sie auf **VPN**. Die Verbindung wird angezeigt. Sie zeigt den Namen des virtuellen Netzwerks an, mit dem eine Verbindung hergestellt wird. Dies sieht etwa so aus:

	![VPN-Client](./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png "VPN-Client")

## <a name="cc"></a>Teil 6: Generieren des Clientzertifikats

Generieren Sie als Nächstes die Clientzertifikate. Sie können entweder ein eindeutiges Zertifikat für jeden Client generieren, mit dem eine Verbindung hergestellt wird, oder Sie können dasselbe Zertifikat für mehrere Clients verwenden. Der Vorteil beim Generieren von eindeutigen Clientzertifikaten besteht darin, dass Sie bei Bedarf ein einzelnes Zertifikat widerrufen können. Falls überall dasselbe Clientzertifikat verwendet wird und Sie das Zertifikat für einen Client widerrufen müssen, müssen Sie sonst neue Zertifikate für alle Clients generieren und installieren, die das Zertifikat für die Authentifizierung verwenden.

- Generieren Sie bei Verwendung einer Unternehmenszertifikatlösung ein Clientzertifikat mit dem gängigen Namenswertformat „name@ihredomäne.com“, und verwenden Sie nicht das NetBIOS-Format „DOMÄNE\\Benutzername“.

- Wenn Sie ein selbstsigniertes Zertifikat verwenden, helfen Ihnen die Informationen zum Generieren eines Clientzertifikats unter [Arbeiten mit selbstsignierten Stammzertifikaten für P2S-Konfigurationen](vpn-gateway-certificates-point-to-site.md) weiter.

## Teil 7: Installieren des Clientzertifikats

Installieren Sie auf jedem Computer, den Sie mit dem virtuellen Netzwerk verbinden möchten, ein Clientzertifikat. Ein Clientzertifikat wird für die Authentifizierung benötigt. Sie können die Installation des Clientzertifikats automatisieren oder manuell durchführen. In den folgenden Schritten wird das manuelle Exportieren und Installieren des Clientzertifikats beschrieben.

1. Sie können *certmgr.msc* zum Exportieren eines Clientzertifikats verwenden. Klicken Sie mit der rechten Maustaste auf das Clientzertifikat, das Sie exportieren möchten, klicken Sie auf **Alle Aufgaben** und anschließend auf **Exportieren**.
2. Exportieren Sie das Clientzertifikat mit dem privaten Schlüssel. Dies ist eine *PFX*-Datei. Vergessen Sie nicht, sich das Kennwort (Schlüssel) zu notieren oder zu merken, das Sie für dieses Zertifikat festgelegt haben.
3. Kopieren Sie die *PFX*-Datei auf den Clientcomputer. Doppelklicken Sie auf dem Clientcomputer auf die *PFX*-Datei, um sie zu installieren. Geben Sie das Kennwort ein, wenn Sie dazu aufgefordert werden. Ändern Sie den Speicherort der Installation nicht.


## Teil 8: Verbinden mit Azure

1. Um eine Verbindung mit Ihrem VNet herzustellen, navigieren Sie auf dem Clientcomputer zu „VPN-Verbindungen“ und suchen nach der VPN-Verbindung, die Sie erstellt haben. Sie hat den gleichen Namen wie das virtuelle Netzwerk. Klicken Sie auf **Verbinden**. Möglicherweise wird eine Popupmeldung angezeigt, die sich auf die Verwendung des Zertifikats bezieht. Klicken Sie in diesem Fall auf **Weiter**, um erhöhte Rechte zu verwenden.

2. Klicken Sie auf der Statusseite **Verbindung** auf **Verbinden**, um die Verbindung herzustellen. Wenn der Bildschirm **Zertifikat auswählen** angezeigt wird, vergewissern Sie sich, dass das angezeigte Clientzertifikat dem Zertifikat entspricht, die Sie zum Herstellen der Verbindung verwenden möchten. Wenn dies nicht der Fall ist, verwenden Sie den Dropdownpfeil, um das richtige Zertifikat auszuwählen, und klicken Sie dann auf **OK**.

	![VPN-Client 2](./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN-Clientverbindung")

3. Die Verbindung sollte jetzt eingerichtet werden.

	![VPN-Client 3](./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "VPN-Clientverbindung 2")

## Teil 9: Überprüfen der Verbindung

1. Um sicherzustellen, dass die VPN-Verbindung aktiv ist, öffnen Sie eine Eingabeaufforderung mit Administratorrechten, und führen Sie *Ipconfig/all* aus.

2. Zeigen Sie die Ergebnisse an. Beachten Sie, dass die erhaltene IP-Adresse eine der Adressen aus dem Clientadresspool des Punkt-zu-Standort-VPN ist, den Sie in Ihrer Konfiguration angegeben haben. Das Ergebnis sollte etwa wie folgt aussehen:
    
		PPP adapter VNet1:
			Connection-specific DNS Suffix .:
			Description.....................: VNet1
			Physical Address................:
			DHCP Enabled....................: No
			Autoconfiguration Enabled.......: Yes
			IPv4 Address....................: 172.16.201.3(Preferred)
			Subnet Mask.....................: 255.255.255.255
			Default Gateway.................:
			NetBIOS over Tcpip..............: Enabled

## So können Sie ein vertrauenswürdiges Stammzertifikat hinzufügen oder entfernen

Zertifikate werden zur Authentifizierung von VPN-Clients für Punkt-zu-Standort-VPNs verwendet. In den folgenden Schritten wird das Hinzufügen und Entfernen von Stammzertifikaten beschrieben. Wenn Sie Azure eine Base64-codierte x.509-Datei (CER-Datei) hinzufügen, weisen Sie Azure dadurch an, dem Stammzertifikat zu vertrauen, das die Datei darstellt.

Sie können vertrauenswürdige Stammzertifikate mithilfe von PowerShell oder im Azure-Portal hinzufügen oder entfernen. Im Azure-Portal ist dies unter **Virtuelles Netzwerkgateway > Einstellungen > Point-to-Site-Konfiguration > Stammzertifikate** möglich. In den folgenden Schritten ist beschrieben, wie Sie diese Aufgaben mithilfe von PowerShell ausführen.

### Hinzufügen eines vertrauenswürdigen Stammzertifikats

Sie können Azure bis zu 20 vertrauenswürdige CER-Stammzertifikatdateien hinzufügen. Gehen Sie folgendermaßen vor, um ein Stammzertifikat hinzuzufügen.

1. Erstellen Sie das neue Stammzertifikat, das Sie Azure hinzufügen möchten, und bereiten Sie es vor. Exportieren Sie den öffentlichen Schlüssel als Base64-codierte X.509-Datei (.CER), und öffnen Sie diese mit einem Text-Editor. Kopieren Sie anschließend nur den unten gezeigten Teil.
 
	Kopieren Sie die Werte wie im folgenden Beispiel gezeigt.

	![Zertifikat](./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png "Zertifikat")
	
2. Legen Sie im folgenden Beispiel den Zertifikatnamen und die Schlüsselinformationen als Variable fest. Ersetzen Sie die Informationen mit Ihren eigenen Informationen.

		$P2SRootCertName2 = "ARMP2SRootCert2.cer"
		$MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"


3. Fügen Sie das neue Stammzertifikat hinzu. Sie können immer nur jeweils ein Zertifikat hinzufügen.

		Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2


4. Mit dem folgenden Cmdlet können Sie überprüfen, ob das neue Zertifikat ordnungsgemäß hinzugefügt wurde:

		Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
		-VirtualNetworkGatewayName "GW"

### Entfernen eines vertrauenswürdigen Stammzertifikats

Sie können ein vertrauenswürdiges Stammzertifikat aus Azure entfernen. Wenn Sie ein vertrauenswürdiges Zertifikat entfernen, kann für die Clientzertifikate, die mit dem Stammzertifikat generiert wurden, per Punkt-zu-Standort-Verfahren keine Verbindung mehr mit Azure hergestellt werden. Wenn Sie Clients verbinden möchten, muss dafür jeweils ein neues Clientzertifikat installiert werden, das mit einem für Azure vertrauenswürdigen Zertifikat generiert wird.

1. Ändern Sie das folgende Beispiel, um ein vertrauenswürdiges Stammzertifikat zu entfernen:

	Deklarieren Sie die Variablen.

		$P2SRootCertName2 = "ARMP2SRootCert2.cer"
		$MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Entfernen Sie das Zertifikat.

		Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
 
3. Überprüfen Sie mit dem folgenden Cmdlet, ob das Zertifikat erfolgreich entfernt wurde.

		Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
		-VirtualNetworkGatewayName "GW"

## Verwalten der Liste gesperrter Clientzertifikate

Sie können Clientzertifikate sperren. Anhand der Zertifikatsperrliste können Sie basierend auf einzelnen Clientzertifikaten selektiv Punkt-zu-Standort-Verbindungen verweigern. Wenn Sie ein CER-Stammzertifikat aus Azure entfernen, wird der Zugriff für alle Clientzertifikate widerrufen, die mit dem widerrufenen Stammzertifikat generiert oder signiert wurden. Falls Sie nicht das Stammzertifikat widerrufen möchten, sondern ein bestimmtes Clientzertifikat, ist dies auch möglich. Die anderen Zertifikate, die mit dem Stammzertifikat generiert wurden, sind dann weiterhin gültig. Üblicherweise wird das Stammzertifikat zum Verwalten des Zugriffs auf Team- oder Organisationsebene verwendet. Eine genauer abgestufte Steuerung des Zugriffs für einzelne Benutzer erfolgt hingegen mit gesperrten Clientzertifikaten.

### Sperren eines Clientzertifikats

1. Rufen Sie den Fingerabdruck des zu sperrenden Clientzertifikats ab.

		$RevokedClientCert1 = "ClientCert1"
		$RevokedThumbprint1 = "‎ef2af033d0686820f5a3c74804d167b88b69982f"

2. Fügen Sie den Fingerabdruck zur Liste der gesperrten Fingerabdrücke hinzu.

		Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
		-VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

3. Überprüfen Sie, ob der Fingerabdruck zur Zertifikatsperrliste hinzugefügt wurde. Sie müssen die Fingerabdrücke nacheinander hinzufügen.

		Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### Reaktivieren eines Clientzertifikats

Sie können ein Clientzertifikat reaktivieren, indem Sie den Fingerabdruck aus der Liste der gesperrten Clientzertifikate entfernen.

1.  Entfernen Sie den Fingerabdruck aus der Liste der gesperrten Clientzertifikate.

		Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
		-VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

2. Überprüfen Sie, ob der Fingerabdruck aus der Sperrliste entfernt wurde.

		Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## Nächste Schritte

Sie können Ihrem virtuellen Netzwerk einen virtuellen Computer hinzufügen. Für diese Schritte finden Sie Informationen unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

<!---HONumber=AcomDC_0907_2016-->