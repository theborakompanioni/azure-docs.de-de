<properties
   pageTitle="Konfigurieren einer VNet-zu-VNet-Verbindung für das klassische Bereitstellungsmodell | Microsoft Azure"
   description="Informationen zum Herstellen von Verbindungen zwischen virtuellen Azure-Netzwerken mithilfe von PowerShell und dem klassischen Azure-Portal."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/03/2016"
   ms.author="cherylmc"/>


# Konfigurieren einer VNet-zu-VNet-Verbindung für das klassische Bereitstellungsmodell

> [AZURE.SELECTOR]
- [Klassisches Azure-Portal](virtual-networks-configure-vnet-to-vnet-connection.md)
- [PowerShell – Azure Resource Manager](vpn-gateway-vnet-vnet-rm-ps.md)


Dieser Artikel führt Sie durch die Schritte zum Erstellen und Verbinden virtueller Netzwerke mithilfe des klassischen Bereitstellungsmodells (auch als Dienstverwaltung bezeichnet). In den folgenden Schritten wird das klassische Azure-Portal verwendet, um die VNets und Gateways zu erstellen, und PowerShell wird zum Konfigurieren der VNet-zu-VNet-Verbindung verwendet. Sie können die VNet-zu-VNet-Verbindung nicht im Portal konfigurieren.

**Informationen zu Azure-Bereitstellungsmodellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

**Bereitstellungsmodelle und Tools für VNet-zu-VNet**

Eine VNET-zu-VNET-Verbindung kann in beiden Bereitstellungsmodellen und mit unterschiedlichen Tools konfiguriert werden. Weitere Informationen finden Sie in der folgenden Tabelle. Wir aktualisieren diese Tabelle, wenn neue Artikel, neue Bereitstellungsmodelle und weitere Tools für diese Konfiguration verfügbar werden. Wenn ein Artikel verfügbar ist, fügen wir in der Tabelle einen direkten Link dazu ein.

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## Über VNet-zu-VNet-Verbindungen

Das Verbinden eines virtuellen Netzwerks mit einem anderen virtuellen Netzwerk (VNet-zu-VNet) ähnelt dem Verbinden eines virtuellen Netzwerks mit einem lokalen Standort. Beide Verbindungstypen verwenden ein VPN-Gateway, um einen sicheren Tunnel mit IPSec/IKE bereitzustellen.

Die VNets, die Sie verbinden, können sich in verschiedenen Abonnements und Regionen befinden. Sie können die VNet-zu-VNet-Kommunikation mit Konfigurationen für mehrere Standorte kombinieren. Auf diese Weise können Sie Netzwerktopologien einrichten, die standortübergreifende Konnektivität mit Konnektivität zwischen virtuellen Netzwerken kombinieren, wie in der folgenden Abbildung dargestellt:

![VNet-zu-VNet-Konnektivitätsdiagramm](./media/virtual-networks-configure-vnet-to-vnet-connection/vnet2vnet.png)

### Gründe für Verbindungen zwischen virtuellen Netzwerken

Aus den folgenden Gründen empfiehlt sich das Herstellen von Verbindungen zwischen virtuellen Netzwerken:

- **Regionsübergreifende Georedundanz und Geopräsenz**
	- Sie können Ihre eigene Georeplikation oder -synchronisierung mit sicheren Verbindungen einrichten, ohne Endpunkte mit Internetzugriff verwenden zu müssen.
	- Mit dem Azure Load Balancer und Clustertechnologie von Microsoft oder Drittanbietern können Sie Workloads mit hoher Verfügbarkeit mit Georedundanz über mehrere Azure-Regionen hinweg einrichten. Ein wichtiges Beispiel ist die Einrichtung von SQL Always On mit Verfügbarkeitsgruppen, die sich über mehrere Azure-Regionen erstrecken.

- **Regionale Anwendungen mit mehreren Ebenen mit starker Isolierungsgrenze**
	- Innerhalb der gleichen Region können Sie Anwendungen mit mehreren Ebenen mit mehreren verbundenen virtuellen Netzwerken einrichten, die starke Isolierung und sichere Kommunikation zwischen den Ebenen aufweisen.

- **Abonnementübergreifende Kommunikation zwischen Organisationen in Azure**
	- Wenn Sie über mehrere Azure-Abonnements verfügen, können Sie Workloads aus verschiedenen Abonnements sicher zwischen virtuellen Netzwerken verbinden.
	- Für Unternehmen und Dienstanbieter können Sie die organisationsübergreifende Kommunikation mit sicherer VPN-Technologie in Azure aktivieren.

### Häufig gestellte Fragen zu VNet-zu-VNet-Verbindungen für klassische VNets

- Die virtuellen Netzwerke können sich im gleichen Abonnement oder in verschiedenen Abonnements befinden.

- Die virtuellen Netzwerke können sich in der gleichen Azure-Region oder in verschiedenen Azure-Regionen (Standorte) befinden.

- Ein Clouddienst oder Endpunkt mit Lastenausgleich darf auch dann nicht mehrere virtuelle Netzwerke umfassen, wenn diese verbunden sind.

- Für das Verbinden mehrerer virtueller Netzwerke sind keine VPN-Geräte erforderlich. Diese werden nur benötigt, wenn standortübergreifende Konnektivität erforderlich ist.

- VNet-zu-VNet unterstützt das Verbinden virtueller Azure-Netzwerke. Nicht unterstützt werden hingegen Verbindungen virtueller Computer oder Clouddienste, die nicht in einem virtuellen Netzwerk bereitgestellt wurden.

- Für VNet-zu-VNet sind Gateways mit dynamischem Routing erforderlich. Azure-Gateways mit statischem Routing werden nicht unterstützt.

- Virtuelle Netzwerkverbindungen können gleichzeitig mit VPNs mit mehreren Standorten und maximal 10 VPN-Tunneln für ein VPN-Gateway des virtuellen Netzwerks verwendet werden, das Verbindungen mit anderen virtuellen Netzwerken oder lokalen Standorten herstellt.

- Die Adressräume der virtuellen Netzwerke und der lokalen Netzwerkstandorte dürfen sich nicht überschneiden. Wenn sich Adressbereiche überschneiden, tritt beim Erstellen eines virtuellen Netzwerks oder beim Hochladen von NETCFG-Konfigurationsdateien ein Fehler auf.

- Redundante Tunnel zwischen einem Paar virtueller Netzwerke werden nicht unterstützt.

- Alle VPN-Tunnel des virtuellen Netzwerks (einschließlich P2S-VPNs) verwenden die verfügbare Bandbreite auf dem Azure-VPN-Gateway und die gleiche SLA für die Verfügbarkeit des VPN-Gateways in Azure gemeinsam.

- VNet-zu-VNet-Datenverkehr wird über den Azure-Backbone übertragen.


## <a name="step1"></a>Schritt 1: Planen der IP-Adressbereiche

Eine wichtige Entscheidung betrifft die Bereiche, die Sie zum Konfigurieren Ihrer virtuellen Netzwerke verwenden. Bedenken Sie, dass Sie bei dieser Konfiguration sicherstellen müssen, dass die VNet-Bereiche untereinander oder mit lokalen Netzwerken, mit denen sie eine Verbindung herstellen, nicht überlappen.

Die folgende Tabelle zeigt ein Beispiel, wie VNets definiert werden. Verwenden Sie die unten aufgeführten Bereiche nur als Richtlinie. Notieren Sie sich die Bereiche, die Sie für Ihre virtuellen Netzwerke verwenden. Sie benötigen diese Informationen in den späteren Schritten.

**Beispieleinstellungen**

|Virtual Network |Adressraum |Region |Verbindung mit dem lokalen Netzwerkstandort|
|:----------------|:---------------------------|:-----------|:-----------------------------|
|VNet1 |VNet1 (10.1.0.0/16) |USA, Westen |VNet2Local (10.2.0.0/16) |
|VNet2 |VNet2 (10.2.0.0/16) |Japan Ost |VNet1Local (10.1.0.0/16) |
  
## Schritt 2: Erstellen von VNet1

In diesem Schritt erstellen wir VNet1. Wenn Sie eines der Beispiele verwenden, achten Sie darauf, dass Sie Ihre eigenen Werte einsetzen. Wenn Ihr VNet bereits vorhanden ist, müssen Sie diesen Schritt nicht ausführen. Aber Sie müssen sicherstellen, dass die IP-Adressbereiche nicht mit den Bereichen für das zweite VNet oder mit einem der anderen VNets überlappen, mit denen Sie eine Verbindung herstellen möchten.

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) an. Beachten Sie, dass diese Schritte nicht das neuere Azure-Portal verwenden.

2. Klicken Sie in der unteren linken Ecke des Bildschirms auf **Neu** > **Netzwerkdienste** > **Virtuelles Netzwerk** > **Benutzerdefiniert erstellen**, um den Konfigurations-Assistenten zu starten. Während Sie den Assistenten bearbeiten, fügen Sie die unten angegebenen Werte auf jeder Seite hinzu.

### Details zum virtuellen Netzwerk

Geben Sie auf der Seite „Details zum virtuellen Netzwerk“ die folgenden Informationen ein.

  ![Details zum virtuellen Netzwerk](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736055.png)

  - **Name** – Der Name des virtuellen Netzwerks. Beispiel: VNet1
  - **Standort** – Wenn Sie ein virtuelles Netzwerk erstellen, ordnen Sie dieses einem Azure-Standort (einer Region) zu. Wenn Sie z. B. wünschen, dass Ihre virtuellen Computer, die Sie für Ihr virtuelles Netzwerk bereitgestellt haben, physisch in "USA West" vorhanden sind, wählen Sie diesen Standort aus. Sie können den Ihrem virtuellen Netzwerk zugeordnete Standort nach dem Erstellen nicht mehr ändern.

### DNS-Server und VPN-Konnektivität

Geben Sie auf der Seite DNS Servers and VPN Connectivity die folgenden Informationen ein, und klicken Sie dann in der unteren rechten Ecke auf den Weiter-Pfeil.

  ![DNS-Server und VPN-Konnektivität](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736056.jpg)

- **DNS-Server** – Geben Sie den Namen und die IP-Adresse des DNS-Servers ein, oder wählen Sie einen zuvor registrierten DNS-Server in der Dropdown-Liste aus. Durch diese Einstellung wird kein DNS-Server erstellt. Sie bietet die Möglichkeit, die DNS-Server anzugeben, die Sie zur Namensauflösung für dieses virtuelle Netzwerk verwenden möchten. Wenn Sie Namensauflösung zwischen Ihren virtuellen Netzwerken wünschen, müssen Sie Ihren eigenen DNS-Server konfigurieren, anstatt die Namensauflösung zu verwenden, die von Azure bereitgestellt wird.
- Aktivieren Sie keines der Kontrollkästchen für die P2S- oder S2S-Verbindungen. Klicken Sie nur unten rechts auf den Pfeil, um zum nächsten Bildschirm zu gelangen.

### Adressräume des virtuellen Netzwerks

Geben Sie auf der Seite „Adressräume des virtuellen Netzwerks“ die Adressbereiche an, die für das virtuelle Netzwerk verwendet werden sollen. Dies sind die dynamischen IP-Adressen (DIPS), die den virtuellen Computern und anderen Rolleninstanzen zugewiesen werden, die Sie für dieses virtuelle Netzwerk bereitstellen.

Wenn Sie ein VNet erstellen, das auch mit Ihrem lokalen Netzwerk verbunden ist, ist es besonders wichtig, einen Bereich auszuwählen, der nicht mit den Bereichen überlappt, die für das lokale Netzwerk verwendet werden. Sie müssen sich in diesem Fall mit Ihrem Netzwerkadministrator abstimmen, der ggf. einen Bereich von IP-Adressen aus dem Adressraum Ihres lokalen Netzwerks reservieren muss, den Sie für Ihr virtuelles Netzwerk verwenden können.

  ![Seite "Adressräume des virtuellen Netzwerks"](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736057.jpg)

  - **Adressraum** – Umfasst Start-IP und die Anzahl Adressen. Stellen Sie sicher, dass die angegebenen Adressräume und die Adressräume im lokalen Netzwerk nicht überlappen. In diesem Beispiel verwenden Sie 10.1.0.0/16 für VNet1.
  - **Subnetz hinzufügen** – Umfasst Start-IP und Anzahl Adressen. Zusätzliche Subnetze sind nicht erforderlich, aber Sie können eine getrenntes Subnetz für virtuelle Computer erstellen, die über statische DIPs verfügen sollen. Vielleicht möchten Sie jedoch auch Ihre virtuellen Computer in einem Subnetz zusammenfassen, das von Ihren anderen Rolleninstanzen getrennt ist.
 
**Klicken Sie auf das Häkchen** rechts unten auf der Seite, damit das virtuelle Netzwerk erstellt wird. Sobald der Vorgang abgeschlossen ist, wird auf der Seite „Netzwerke“ unter „Status“ der Eintrag „Erstellt“ angezeigt.

## Schritt 3: Erstellen von VNet2

Wiederholen Sie anschließend die oben aufgeführten Schritte, um ein weiteres virtuelles Netzwerk zu erstellen. Im weiteren Verlauf stellen Sie eine Verbindung zwischen diesen beiden virtuellen Netzwerken her. Sie können auf die [Beispieleinstellungen](#step1) in Schritt 1 zurückgreifen. Wenn Ihr VNet bereits vorhanden ist, müssen Sie diesen Schritt nicht ausführen. Aber Sie müssen sicherstellen, dass die IP-Adressbereiche nicht mit den Bereichen der anderen VNets oder lokalen Netzwerke überlappen, mit denen Sie eine Verbindung herstellen möchten.

## Schritt 4: Hinzufügen der lokalen Netzwerkstandorte

Wenn Sie eine VNet-zu-VNet-Konfiguration erstellen, müssen Sie lokale Netzwerkstandorte konfigurieren, die auf der Seite **Lokale Netzwerke** des Portals angezeigt werden. Azure verwendet die für die einzelnen lokalen Netzwerkstandorte angegebenen Einstellungen, um zu bestimmen, wie Datenverkehr zwischen den VNets weitergeleitet wird. Sie legen den Namen fest, den Sie zum Verweisen auf die einzelnen lokalen Netzwerkstandorte verwenden möchten. Es empfiehlt sich, einen aussagekräftigen Namen zu verwenden, da Sie den Wert aus einer Dropdownliste auswählen.

Beispielsweise stellt in dieser Übung VNet1 eine Verbindung mit einem lokalen Netzwerkstandort her, den Sie mit dem Namen „VNet2Local“ erstellt haben. Die Einstellungen für VNet2Local enthalten die Adresspräfixe für VNet2 und eine öffentliche IP-Adresse für das VNet2-Gateway. VNet2 stellt eine Verbindung mit einem lokalen Netzwerkstandort her, den Sie mit dem Namen „VNet1Local“ erstellt haben und der die Adresspräfixe für VNet1 und die öffentliche IP-Adresse für das VNet1-Gateway enthält.

### <a name="localnet"></a>Hinzufügen des lokalen Netzwerkstandorts VNet1Local

1. Klicken Sie in der unteren linken Ecke des Bildschirms auf **Neu** > **Netzwerkdienste** > **Virtuelles Netzwerk** > **Lokales Netzwerk hinzufügen**.

2. Geben Sie auf der Seite **Details zum lokalen Netzwerk angeben** unter **Name** einen Namen ein, den Sie für das virtuelle Netzwerk verwenden möchten, mit dem Sie eine Verbindung herstellen möchten. In diesem Beispiel können Sie „VNet1Local“ verwenden, um auf die IP-Adressbereiche und das Gateway für VNet1 zu verweisen.

3. Geben Sie in **IP-Adresse des VPN-Geräts (optional)** eine beliebige gültige öffentliche IP-Adresse an. Normalerweise verwenden Sie die tatsächliche externe IP-Adresse für ein VPN-Gerät. Für VNet-zu-VNet-Konfigurationen verwenden Sie die öffentliche IP-Adresse, die dem Gateway für das VNet zugewiesen ist. Wenn Sie das Gateway jedoch noch nicht erstellt haben, geben Sie eine beliebige gültige öffentliche IP-Adresse als Platzhalter ein. Lassen Sie dieses Feld nicht leer, es ist für diese Konfiguration nicht optional. Anschließend kehren Sie zu diesen Einstellungen zurück und konfigurieren sie dann mit den entsprechenden Gateway-IP-Adressen, nachdem Azure diese generiert hat. Klicken Sie auf den Pfeil, um zum nächsten Bildschirm zu gelangen.

4. Auf der Seite **Adresse angeben** fügen Sie den tatsächlichen IP-Adressbereich und die Adressenanzahl für VNet1 ein. Diese Angaben müssen genau dem Bereich entsprechen, den Sie für VNet1 konfiguriert haben. Azure verwendet den IP-Adressbereich, den Sie angeben, um den Datenverkehr für VNet1 weiterzuleiten. Klicken Sie auf das Häkchen, um das lokale Netzwerk zu erstellen.

### Hinzufügen des lokalen Netzwerkstandorts VNet2Local

Verwenden Sie die oben aufgeführten Schritte, um den lokalen Netzwerkstandort „VNet2Local“ zu erstellen. Sie können bei Bedarf auf die Werte in den [Beispieleinstellungen](#step1) in Schritt 1 zurückgreifen.

### Konfigurieren der einzelnen VNets, sodass sie auf ein lokales Netzwerk verweisen

Jedes VNet muss auf das entsprechende lokale Netzwerk verweisen, an das Sie Datenverkehr weiterleiten möchten.

#### Für VNet1

1. Navigieren Sie zur Seite **Konfigurieren** für das virtuelle Netzwerk **VNet1**.
2. Wählen Sie unter „Site-to-Site-Konnektivität“ die Option „Eine Verbindung mit dem lokalen Netzwerk herstellen“ aus, und wählen Sie dann **VNet2Local** als lokales Netzwerk aus der Dropdownliste aus.
3. Speichern Sie die Einstellungen.

#### Für VNet2

1. Navigieren Sie zur Seite **Konfigurieren** für das virtuelle Netzwerk **VNet2**.
2. Wählen Sie unter „Site-to-Site-Konnektivität“ die Option „Eine Verbindung mit dem lokalen Netzwerk herstellen“ aus, und wählen Sie dann **VNet1Local** als lokales Netzwerk aus der Dropdownliste aus.
3. Speichern Sie die Einstellungen.

## Schritt 5: Konfigurieren eines Gateways für jedes VNet

Konfigurieren Sie ein Gateway mit dynamischem Routing für jedes virtuelle Netzwerk. Diese Konfiguration unterstützt keine Gateways mit statischem Routing. Wenn Sie VNets verwenden, die zuvor konfiguriert wurden und die bereits über Gateways mit dynamischem Routing verfügen, müssen Sie diesen Schritt nicht ausführen. Wenn die Gateways statisch sind, müssen Sie diese löschen und dann erneut als Gateways mit dynamischem Routing erstellen. Beachten Sie, dass beim Löschen eines Gateways die zugewiesene öffentliche IP-Adresse freigegeben wird. Sie müssen dann lokale Netzwerke und VPN-Geräte mit der neuen öffentlichen IP-Adresse für das neue Gateway konfigurieren.

1. Stellen Sie auf der Seite **Netzwerke** sicher, dass in der Statusspalte für Ihr virtuelles Netzwerk **Erstellt** angezeigt wird.

2. Klicken Sie in der Spalte **Name** auf den Namen Ihres virtuellen Netzwerks. In diesem Beispiel verwenden wir VNet1.

3. Vergewissern Sie sich auf der Seite **Dashboard**, dass dieses VNet noch kein konfiguriertes Gateway besitzt. Diese Statusänderung wird angezeigt, wenn Sie die Konfigurationsschritte für Ihr Gateway absolvieren.

4. Klicken Sie unten auf der Seite auf **Gateway erstellen** und auf **Dynamisches Routing**. Wenn Sie aufgefordert werden zu bestätigen, dass das Gateway erstellt werden soll, klicken Sie auf "Ja".

  	![Gatewaytyp](./media/virtual-networks-configure-vnet-to-vnet-connection/IC717026.png)

5. Während der Gatewayerstellung wechselt die Gatewaygrafik auf der Seite zu gelb und zeigt „Gateway wird erstellt“ an. Die Erstellung des Gateways nimmt normalerweise 30 Minuten in Anspruch.

6. Wiederholen Sie die gleichen Schritte für VNet2. Das erste VNet-Gateway muss nicht fertiggestellt sein, wenn Sie mit dem Erstellen des Gateways für das andere VNet beginnen.

7. Wenn sich der Gatewaystatus in „Verbinden“ ändert, wird die öffentliche IP-Adresse für jedes Gateway im Dashboard angezeigt. Notieren Sie sich die IP-Adressen, die für jedes VNet gelten. Gehen Sie dabei sorgfältig vor, um sie nicht zu verwechseln. Dies sind die IP-Adressen, die verwendet werden, wenn Sie die IP-Platzhalteradressen für das VPN-Gerät für jedes lokale Netzwerk bearbeiten.

## Schritt 6: Bearbeiten des lokalen Netzwerks

1. Klicken Sie auf der Seite **Lokale Netzwerke** auf den Namen des lokalen Netzwerks, den Sie bearbeiten möchten, und klicken Sie dann unten auf der Seite auf **Bearbeiten**. Geben Sie als **IP-Adresse des VPN-Geräts** die IP-Adresse des Gateways ein, das dem VNet entspricht. Geben Sie z.B. für VNet1Local die Gateway-IP-Adresse ein, die VNet1 zugewiesen ist. Klicken Sie unten auf der Seite auf den Pfeil.

2. Klicken Sie auf der Seite **Den Adressraum angeben** unten rechts auf das Häkchen, ohne Änderungen vorzunehmen.

## Schritt 7: Erstellen der VPN-Verbindung

Nachdem alle zuvor beschriebenen Schritte abgeschlossen wurden, legen Sie die vorinstallierten IPsec-/IKE-Schlüssel fest. Hierfür verwenden wir PowerShell. Weitere Informationen zur Installation der PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Stellen Sie sicher, dass Sie die neueste Version der Dienstverwaltungs-Cmdlets (SM) herunterladen.

1. Öffnen Sie Windows PowerShell, und melden Sie sich an.

		Add-AzureAccount

2. Erstellen Sie die Verbindungen. Beachten Sie in den folgenden Beispielen, dass der Wert für „SharedKey“ identisch ist. Dieser Wert muss immer übereinstimmen.


	VNet1-zu-VNet2-Verbindung

		Set-AzureVNetGatewayKey -VNetName VNet1 -LocalNetworkSiteName VNet2Local -SharedKey A1b2C3D4

	VNet2-zu-VNet1-Verbindung

		Set-AzureVNetGatewayKey -VNetName VNet2 -LocalNetworkSiteName VNet1Local -SharedKey A1b2C3D4

3. Warten Sie, bis die Verbindung initialisiert wird. Nachdem das Gateway initialisiert wurde, sieht es wie in der Abbildung unten gezeigt aus, und Ihre virtuellen Netzwerke sind verbunden.

	![Gatewaystatus – verbunden](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736059.jpg)

	[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

## Nächste Schritte

Sie können Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter [Dokumentation zu Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/).



[1]: ../hdinsight-hbase-geo-replication-configure-vnets.md
[2]: http://channel9.msdn.com/Series/Getting-started-with-Windows-Azure-HDInsight-Service/Configure-the-VPN-connectivity-between-two-Azure-virtual-networks
 

<!---HONumber=AcomDC_0803_2016-->