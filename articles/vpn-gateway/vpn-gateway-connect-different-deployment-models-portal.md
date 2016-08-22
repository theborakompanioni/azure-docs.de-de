<properties 
   pageTitle="Verbinden von klassischen virtuellen Netzwerken mit virtuellen Resource Manager-Netzwerken im Portal | Microsoft Azure"
   description="Hier erfahren Sie, wie Sie mithilfe von VPN Gateway und des Portals eine VPN-Verbindung zwischen klassischen VNets und Resource Manager-VNets erstellen."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/09/2016"
   ms.author="cherylmc" />

# Verbinden von virtuellen Netzwerken aus unterschiedlichen Bereitstellungsmodellen im Portal

> [AZURE.SELECTOR]
- [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)


In Azure sind zurzeit zwei Verwaltungsmodelle verfügbar: klassisches Modell und RM-Modell (Resource Manager). Wenn Sie Azure seit einiger Zeit verwenden, verfügen Sie wahrscheinlich über Azure-VMs und Instanzrollen, die in einem klassischen VNet ausgeführt werden. Ihre neueren VMs und Rolleninstanzen werden möglicherweise in einem in Resource Manager erstellten VNet ausgeführt. In diesem Artikel wird Schritt für Schritt erläutert, wie Sie klassische VNets mit Resource Manager-VNets verbinden, damit die Ressourcen in den separaten Bereitstellungsmodellen über eine Gatewayverbindung miteinander kommunizieren können.

Sie können auch eine Verbindung zwischen VNets in unterschiedlichen Abonnements, Regionen und Bereitstellungsmodellen erstellen. Es ist auch möglich, VNets zu verbinden, die bereits über Verbindungen mit lokalen Netzwerken verfügen, sofern sie mit einem dynamischen oder routenbasierten Gateway konfiguriert wurden. Weitere Informationen zu VNet-zu-VNet-Verbindungen finden Sie am Ende dieses Artikels unter [Häufig gestellte Fragen zu VNet-zu-VNet-Verbindungen](#faq).

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## Vorbereitungen

Die folgenden Schritte führen Sie durch die erforderlichen Einstellungen zum Konfigurieren eines dynamischen oder routenbasierten Gateways für jedes VNet und die Erstellung einer VPN-Verbindung zwischen den Gateways. Diese Konfiguration unterstützt keine statischen oder richtlinienbasierten Gateways. In diesem Artikel verwenden wir das klassische Portal, das Azure-Portal und PowerShell. Derzeit ist es nicht möglich, diese Konfiguration nur im Azure-Portal zu erstellen.

Überprüfen Sie vor Beginn Folgendes:

 - Beide VNets wurden bereits erstellt.
 - Die Adressbereiche für die VNets überlappen weder einander noch die Bereiche für andere Verbindungen, mit denen die Gateways verbunden werden können.
 - Sie haben die aktuellen PowerShell-Cmdlets (1.0.2 oder höher) installiert. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Installieren Sie sowohl die SM-Cmdlets (Dienstverwaltung) als auch die RM-Cmdlets (Resource Manager).

### <a name="values"></a>Beispieleinstellungen

Sie können die Beispieleinstellungen als Referenz verwenden, wenn Sie in den folgenden Schritten die PowerShell-Cmdlets ausführen.

**Einstellungen für das klassische VNet**

VNet-Name = ClassicVNet <br> Standort = USA, Westen <br> Adressräume von Virtual Network = 10.0.0.0/8 <br> Subnetz-1 = 10.0.0.0/11 <br> Gatewaysubnetz = 10.32.0.0/29 <br> Name des lokalen Netzwerks = RMVNetLocal <br>

**Einstellungen für das Resource Manager-VNet**

VNet-Name = RMVNet <br> Ressourcengruppe = RG1 <br> IP-Adressräume von Virtual Network = 192.168.1.0/16 <br> Subnetz-1 = 192.168.1.0/24 <br> Gatewaysubnetz = 192.168.0.0/26 <br> Standort = USA, Osten <br> Name des virtuellen Netzwerkgateways = RMGateway <br> Öffentlicher IP-Name des Gateways = gwpip <br> Gatewaytyp = VPN <br> VPN-Typ = Routenbasiert <br> Lokales Netzwerkgateway = ClassicVNetLocal <br>

## <a name="createsmgw"></a>Abschnitt 1: Konfigurieren der Einstellungen für das klassische VNet

In diesem Abschnitt erstellen wir das lokale Netzwerk und das Gateway für Ihr klassisches VNet. In den Anweisungen in diesem Abschnitt wird das klassische Portal verwendet. Das Azure-Portal bietet derzeit nicht alle Einstellungen, die für ein klassisches VNet gelten.

### Teil 1 – Erstellen eines neuen lokalen Netzwerks

Öffnen Sie das [klassische Portal](https://manage.windowsazure.com), und melden Sie sich mit Ihrem Azure-Konto an.

1. Klicken Sie in der unteren linken Ecke des Bildschirms auf **NEU** > **Network Services** > **Virtual Network** > **Lokales Netzwerk hinzufügen**.

2. Geben Sie im Fenster **Details zum lokalen Netzwerk angeben** einen Namen für das RM-VNet ein, mit dem Sie eine Verbindung herstellen möchten. Geben Sie im Feld **IP-Adresse des VPN-Geräts (optional)** eine beliebige gültige öffentliche IP-Adresse ein. Dies ist nur ein temporärer Platzhalter. Sie ändern diese IP-Adresse später. Klicken Sie in der unteren rechten Ecke des Fensters auf den Pfeil.
 
3. Geben Sie auf der Seite **Adressraum angeben** im Textfeld **Start-IP** das Netzwerkpräfix und den CIDR-Block für das Resource Manager-VNet ein, mit dem Sie eine Verbindung herstellen möchten. Diese Einstellung wird verwendet, um den Adressraum für die Weiterleitung an das RM-VNet anzugeben.

### Teil 2 – Zuordnen des lokalen Netzwerks zum VNet

1. Klicken Sie oben auf der Seite auf **Virtual Networks**, um zum Bildschirm „Virtual Netzworks“ zu wechseln, und klicken Sie dann auf Ihr klassisches VNet. Klicken Sie auf der Seite für Ihr VNet auf **Konfigurieren**, um zur Konfigurationsseite zu wechseln.

2. Aktivieren Sie im Verbindungsabschnitt **Site-to-Site-Konnektivität** das Kontrollkästchen **Eine Verbindung mit dem lokalen Netzwerk herstellen**. Wählen Sie anschließend das von Ihnen erstellte lokale Netzwerk aus. Falls Sie mehrere lokale Netzwerke erstellt haben, müssen Sie in der Dropdownliste das Netzwerk auswählen, das Sie zum Darstellen Ihres Resource Manager-VNet erstellt haben.

3. Klicken Sie unten auf der Seite auf **Speichern**.

### Teil 3 – Erstellen des Gateways

1. Klicken Sie nach dem Speichern der Einstellungen oben auf der Seite auf **Dashboard**, um zur Seite „Dashboard“ zu wechseln. Klicken Sie unten auf der Seite „Dashboard“ auf **Gateway erstellen** und dann auf **Dynamisches Routing**. Klicken Sie auf **Ja**, um mit der Erstellung des Gateways zu beginnen. Für diese Konfiguration ist ein Gateway mit dynamischem Routing erforderlich.

2. Warten Sie, bis das Gateway erstellt wurde. Dieser Vorgang kann manchmal 45 Minuten oder sogar länger dauern.

### <a name="ip"></a>Teil 4 – Anzeigen der öffentlichen IP-Adresse des Gateways

Nachdem das Gateway erstellt wurde, können Sie die Gateway-IP-Adresse auf der Seite **Dashboard** anzeigen. Dies ist die öffentliche IP-Adresse Ihres Gateways. Notieren Sie die öffentliche IP-Adresse, oder kopieren Sie sie. Sie verwenden diese Adresse später beim Erstellen des lokalen Netzwerks für Ihre Resource Manager-VNet-Konfiguration.


## <a name="creatermgw"></a>Abschnitt 2: Konfigurieren der Einstellungen für das Resource Manager-VNet

In diesem Abschnitt erstellen wir das virtuelle Netzwerkgateway und das lokale Netzwerk für Ihr Resource Manager-VNet. Bevor Sie die folgenden Schritte ausführen, müssen Sie die öffentliche IP-Adresse für das Gateway des klassischen VNet abrufen.

Die Screenshots dienen als Beispiele. Achten Sie darauf, dass Sie die Werte durch Ihre eigenen Werte ersetzen. Falls Sie die Konfiguration zu Übungszwecken erstellen, können Sie [diese Werte](#values) verwenden.


### Teil 1 – Erstellen eines Gatewaysubnetzes

Bevor Sie das virtuelle Netzwerk mit einem Gateway verbinden, müssen Sie das Gatewaysubnetz für das virtuelle Netzwerk erstellen, mit dem Sie eine Verbindung herstellen möchten. Erstellen Sie ein Gatewaysubnetz mit einem CIDR-Wert von /28 oder einem größeren Wert (/27, /26 usw.).

Navigieren Sie in einem Browser zum [Azure-Portal](http://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### Teil 2 – Erstellen eines virtuellen Netzwerkgateways


[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


### Teil 3 – Erstellen eines lokalen Netzwerkgateways

Mit dem „lokalen Netzwerkgateway“ ist normalerweise Ihr lokaler Standort gemeint. Dieses Gateway teilt Azure die IP-Adressbereiche für die Weiterleitung an den Standort und die öffentliche IP-Adresse des Geräts für diesen Standort mit. In diesem Fall bezieht es sich jedoch auf den Adressbereich und die öffentliche IP-Adresse, die Ihrem klassischen VNet und virtuellen Netzwerkgateway zugeordnet sind.

Geben Sie für das lokale Netzwerkgateway einen Namen ein, anhand dessen Azure darauf verweisen kann. Sie können Ihr lokales Netzwerkgateway erstellen, während das virtuelle Netzwerkgateway erstellt wird. Für diese Konfiguration verwenden Sie die öffentliche IP-Adresse, die Ihrem klassischen VNet-Gateway im [vorherigen Abschnitt](#ip) zugewiesen wurde.

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]


### Teil 4 – Kopieren der öffentlichen IP-Adresse

Nachdem das virtuelle Netzwerkgateway erstellt wurde, kopieren Sie die öffentliche IP-Adresse, die dem Gateway zugeordnet ist. Diese Adresse verwenden Sie beim Konfigurieren der lokalen Netzwerkeinstellungen für Ihr klassisches VNet.


## Abschnitt 3: Ändern des lokalen Netzwerks für das klassische VNet

Öffnen Sie das [klassische Portal](https://manage.windowsazure.com).

2. Scrollen Sie im klassischen Portal auf der linken Seite nach unten, und klicken Sie auf **Netzwerke**. Klicken Sie oben auf der Seite **Netzwerke** auf **Lokale Netzwerke**.

3. Klicken Sie auf das lokale Netzwerk, das Sie in Teil 1 konfiguriert haben. Klicken Sie unten auf der Seite auf **Bearbeiten**.

4. Ersetzen Sie auf der Seite **Details zum lokalen Netzwerk angeben** die Platzhalter-IP-Adresse durch die öffentliche IP-Adresse für das Resource Manager-Gateway, das Sie im vorherigen Abschnitt erstellt haben. Klicken Sie auf den Pfeil, um zum nächsten Abschnitt zu gelangen. Überprüfen Sie, ob der **Adressraum** korrekt ist, und klicken Sie dann auf das Häkchen, um die Änderungen zu übernehmen.

## <a name="connect"></a>Abschnitt 4: Erstellen der Verbindung

In diesem Abschnitt erstellen wir die Verbindung zwischen den VNets. Für diese Schritte ist PowerShell erforderlich. Die Verbindung kann in keinem der Portale erstellen werden. Stellen Sie sicher, dass Sie sowohl die SM-PowerShell-Cmdlets (klassisch) als auch die RM-PowerShell-Cmdlets (Resource Manager) heruntergeladen und installiert haben.


1. Melden Sie sich in der PowerShell-Konsole bei Ihrem Azure-Konto an. Das folgende Cmdlet fordert Sie zur Eingabe der Anmeldeinformationen für Ihr Azure-Konto auf. Nachdem Sie sich angemeldet haben, werden Ihre Kontoeinstellungen heruntergeladen, sodass sie in Azure PowerShell verfügbar sind.

		Login-AzureRmAccount 

 	Falls Sie mehrere Abonnements haben, benötigen Sie eine Liste Ihrer Azure-Abonnements.

		Get-AzureRmSubscription

	Geben Sie das Abonnement an, das Sie verwenden möchten.

		Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2. Fügen Sie Ihr Azure-Konto hinzu, um die klassischen PowerShell-Cmdlets zu verwenden. Dazu können Sie den folgenden Befehl verwenden:

		Add-AzureAccount

3. Legen Sie Ihren gemeinsam verwendeten Schlüssel fest, indem Sie den folgenden Beispielcode ausführen. In diesem Beispiel ist `-VNetName` der Name des klassischen VNet und `-LocalNetworkSiteName` der Name, den Sie bei der Konfiguration des lokalen Netzwerks im klassischen Portal für das lokale Netzwerk angegeben haben. `-SharedKey` ist ein Wert, den Sie generieren und angeben können. Der hier angegebene Wert muss mit dem Wert identisch sein, den Sie im nächsten Schritt beim Erstellen der Verbindung angeben.

		Set-AzureVNetGatewayKey -VNetName ClassicVNet `
		-LocalNetworkSiteName RMVNetLocal -SharedKey abc123

4. Erstellen Sie die VPN-Verbindung, indem Sie die folgenden Befehle ausführen.
	
	**Festlegen der Variablen**

		$vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
		$vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

	**Erstellen der Verbindung**<br> Beachten Sie, dass `-ConnectionType` „IPsec“ ist, und nicht „Vnet2Vnet“. In diesem Beispiel ist `-Name` der Name, den Sie für die Verbindung verwenden möchten. Der folgende Beispielcode erstellt eine Verbindung mit dem Namen „*rm-to-classic-connection*“.
		
		New-AzureRmVirtualNetworkGatewayConnection -Name rm-to-classic-connection -ResourceGroupName RG1 `
		-Location "East US" -VirtualNetworkGateway1 `
		$vnet02gateway -LocalNetworkGateway2 `
		$vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## Überprüfen der Verbindung

Sie können die Verbindung im klassischen Portal, im Azure-Portal oder mit PowerShell überprüfen. Zum Überprüfen der Verbindung können Sie die folgenden Schritte ausführen. Ersetzen Sie die Werte dabei durch Ihre eigenen Werte.

[AZURE.INCLUDE [vpn-gateway-verify connection](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="faq"></a>Häufig gestellte Fragen zu VNet-zu-VNet-Verbindungen

Zeigen Sie die Details zu den häufig gestellten Fragen an, um zusätzliche Informationen zu VNet-zu-VNet-Verbindungen zu erhalten.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

<!---HONumber=AcomDC_0810_2016-->