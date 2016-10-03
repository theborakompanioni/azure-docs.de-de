<properties
   pageTitle="Konfigurieren eines virtuellen Netzwerks und Gateways für ExpressRoute im klassischen Portal | Microsoft Azure"
   description="Dieser Artikel beschreibt die Einrichtung eines virtuellen Netzwerks für ExpressRoute unter Verwendung des klassischen Bereitstellungsmodells und des klassischen Portals."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/20/2016"
   ms.author="cherylmc"/>

# Erstellen eines virtuellen Netzwerks für ExpressRoute im klassischen Portal

In diesem Artikel werden Sie Schritt für Schritt durch die Konfiguration eines virtuellen Netzwerks und eines Gateways für virtuelle Netzwerke zur Verwendung mit ExpressRoute geführt. Dabei werden das klassische Bereitstellungsmodell und das klassische Portal verwendet.

Anweisungen für das Resource Manager-Bereitstellungsmodell finden Sie in den folgenden Artikeln: [Erstellen eines virtuellen Netzwerks über PowerShell](../virtual-network/virtual-networks-create-vnet-arm-ps.md) und [Konfigurieren eines Gateways für virtuelle Netzwerke für ExpressRoute mit Resource Manager und PowerShell](expressroute-howto-add-gateway-resource-manager.md).

**Informationen zu Azure-Bereitstellungsmodellen**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## Erstellen eines klassischen VNet und eines Gateways

Mit den folgenden Schritten werden ein klassisches VNet und ein Gateway für virtuelle Netzwerke erstellt. Wenn Sie bereits über ein klassisches VNet verfügen, lesen Sie den Abschnitt [Konfigurieren eines vorhandenen klassischen VNet](#config) in diesem Artikel.

1. Melden Sie sich beim [klassischen Azure-Portal](http://manage.windowsazure.com) an.

2. Klicken Sie unten links auf dem Bildschirm auf **Neu**. Klicken Sie im Navigationsbereich auf **Netzwerkdienste** und dann auf **Virtuelles Netzwerk**. Klicken Sie auf **Custom Create**, um den Konfigurationsassistenten zu starten.

3. Geben Sie auf der Seite **Details zu Virtual Network** Folgendes ein:

	- **Name** – Der Name Ihres virtuellen Netzwerks. Sie verwenden den hier festgelegten Namen bei der Bereitstellung Ihrer VMs und PaaS-Instanzen, deshalb sollte der Name nicht zu kompliziert sein.
	- **Speicherort** – Der Speicherort steht in direkter Beziehung zu dem physischen Standort (Region), an dem sich Ihre Ressourcen (VMs) befinden sollen. Wenn Sie z. B. möchten, dass sich die virtuellen Computer, die Sie für Ihr virtuelles Netzwerk bereitstellen, physisch in "USA West" befinden, wählen Sie diesen Speicherort aus. Sie können die Ihrem virtuellen Netzwerk zugeordnete Region nach dem Erstellen nicht mehr ändern.

4. Geben Sie auf der Seite **DNS Server und VPN-Konnektivität** die folgenden Informationen ein, und klicken Sie dann in der unteren rechten Ecke auf den Weiter-Pfeil.

	- **DNS-Server** – Geben Sie den Namen und die IP-Adresse des DNS-Servers ein, oder wählen Sie im Kontextmenü einen zuvor registrierten DNS-Server aus. Mit dieser Einstellung wird kein DNS-Server erstellt. Sie bietet die Möglichkeit, den DNS-Server anzugeben, den Sie zur Namensauflösung für dieses virtuelle Netzwerk verwenden möchten.
	- **Site-to-Site-Konnektivität** – Aktivieren Sie das Kontrollkästchen für **Ein Site-to-Site-VPN konfigurieren**.
	- **ExpressRoute** – Aktivieren Sie das Kontrollkästchen **ExpressRoute verwenden**. Diese Option wird nur angezeigt, wenn Sie **Ein Site-to-Site-VPN konfigurieren** ausgewählt haben.
	- **Lokales Netzwerk** – Sie müssen für ExpressRoute auch einen lokalen Netzwerkstandort besitzen. Allerdings werden im Fall einer ExpressRoute-Verbindung die Adresspräfixe ignoriert, die für den lokalen Netzwerkstandort angegeben werden. Stattdessen werden die Adresspräfixe, die Microsoft über die ExpressRoute-Verbindung angekündigt werden, zu Routingzwecken verwendet.<BR>Wenn Sie bereits ein lokales Netzwerk für Ihre ExpressRoute-Verbindung erstellt haben, können Sie es aus der Dropdownliste auswählen. Wählen Sie andernfalls **Ein neues lokales Netzwerk angeben** aus.

5. Die Seite **Site-to-Site-Konnektivität** wird angezeigt, wenn Sie im vorherigen Schritt die Angabe eines neuen lokalen Netzwerks ausgewählt haben. Geben Sie zur Konfiguration des lokalen Netzwerks die folgenden Informationen ein, und klicken Sie dann auf den Pfeil "Weiter".

	- **Name** – Der Name, den Sie Ihrem lokalen Netzwerkstandort geben möchten.
	- **Adressraum** – Umfasst Start-IP und CIDR (Anzahl der Adressen). Sie können einen beliebigen Adressbereich angeben, solange er sich nicht mit dem Adressbereich Ihres virtuellen Netzwerks überschneidet. Normalerweise werden die Adressbereiche für Ihre lokalen Netzwerke angegeben, aber im Fall von ExpressRoute werden diese Einstellungen nicht verwendet. Diese Einstellung ist jedoch erforderlich, um das lokale Netzwerk zu erstellen, wenn Sie das klassische Portal verwenden.
	- **Adressraum hinzufügen** – Diese Einstellung ist für ExpressRoute nicht relevant.


6. Geben Sie auf der Seite **Virtual Network Address Spaces** die folgenden Informationen ein, und klicken Sie dann in der unteren rechten Ecke auf das Häkchen, um das Netzwerk zu konfigurieren.

	- **Adressraum** – Umfasst Start-IP und die Anzahl der Adressen. Stellen Sie sicher, dass sich die angegebenen Adressräume und die Adressräume im lokalen Netzwerk nicht überschneiden.
	- **Subnetz hinzufügen** – Umfasst Start-IP und Anzahl Adressen. Zusätzliche Subnetze sind nicht erforderlich.
	- **Gatewaysubnetz hinzufügen** – Klicken Sie auf diese Option, um das Gatewaysubnetz hinzuzufügen. Das Gatewaysubnetz wird nur für das Gateway des virtuellen Netzwerks verwendet, und es ist für diese Konfiguration erforderlich.<BR>Das Gatewaysubnetz-CIDR (Anzahl von Adressen) für ExpressRoute muss /28 oder größer sein (/ 27, / 26 usw.). Dadurch stehen in diesem Subnetz genügend IP-Adressen zur Verfügung, damit die Konfiguration funktioniert. Wenn Sie im klassischen Portal das Kontrollkästchen zur Verwendung von ExpressRoute aktiviert haben, wird im Portal ein Gatewaysubnetz mit „/28“ angegeben. Die Anzahl von CIDR-Adressen kann im klassischen Portal nicht angepasst werden. Das Gatewaysubnetz wird im klassischen Portal als **Gateway** angezeigt, obwohl der tatsächliche Name des erstellten Gatewaysubnetzes eigentlich **GatewaySubnet** lautet. Sie können diesen Namen mithilfe von PowerShell oder im Azure-Portal anzeigen.

7. Klicken Sie auf das Häkchen am rechten unteren Rand der Seite, damit das virtuelle Netzwerk erstellt wird. Sobald der Vorgang abgeschlossen ist, wird im klassischen Portal auf der Seite **Netzwerke** unter **Status** der Eintrag **Erstellt** angezeigt.

## <a name="gw"></a>Erstellen des Gateways

1. Klicken Sie auf der Seite **Netzwerke** auf das eben erstellte virtuelle Netzwerk, und klicken Sie dann oben auf der Seite auf **Dashboard**.

2. Klicken Sie unten auf der Seite **Dashboard** auf **Gateway erstellen**, und wählen Sie dann **Dynamisches Routing** aus. Klicken Sie auf **Ja**, um zu bestätigen, dass Sie ein Gateway erstellen möchten.

3. Bei Beginn der Gatewayerstellung erhalten Sie eine entsprechende Nachricht. Die Gatewayerstellung kann bis zu 45 Minuten dauern.

4. Verknüpfen Sie Ihr Netzwerk mit einer Verbindung. Gehen Sie gemäß den Anweisungen im Artikel [Verknüpfen von virtuellen Netzwerken mit ExpressRoute-Verbindungen](expressroute-howto-linkvnet-classic.md) vor.

## <a name="config"></a>Konfigurieren eines vorhandenen klassischen VNet für ExpressRoute

Wenn Sie bereits über ein klassisches VNet verfügen, können Sie es im klassischen Portal zum Herstellen einer Verbindung mit ExpressRoute konfigurieren. Die Einstellungen sind mit denen der obigen Abschnitte identisch. Lesen Sie also diese Abschnitte, um sich mit den erforderlichen Einstellungen vertraut zu machen. Wenn Sie parallel bestehende ExpressRoute- und Standort-zu-Standort-Verbindungen erstellen möchten, finden Sie die erforderlichen Schritte [in diesem Artikel](expressroute-howto-coexist-classic.md). Sie unterscheiden sich von den Schritten in diesem Artikel.
 
1. Das lokale Netzwerk muss vor dem Aktualisieren der übrigen VNet-Einstellungen erstellt werden. Um ein neues lokales Netzwerk zu erstellen, was beim Konfigurieren von ExpressRoute über das klassische Portal erforderlich ist, klicken Sie auf **Neu** **>** **Network Services** **>** **Virtuelles Netzwerk** **>** **Lokales Netzwerk hinzufügen**. Folgen Sie den Schritten des Assistenten zum Erstellen des lokalen Netzwerks.

2. Verwenden Sie die Seite **Konfigurieren** zum Aktualisieren der restlichen Einstellungen für das VNet und zum Zuordnen des VNet zum lokalen Netzwerk.

3. Lesen Sie nach dem Konfigurieren der Einstellungen den Abschnitt [Erstellen des Gateways](#gw) dieses Artikels, um das Gateway zu erstellen.


## Nächste Schritte

- Informationen zum Hinzufügen virtueller Computer zu Ihrem virtuellen Netzwerk finden Sie unter [Virtual Machines-Lernpfade](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
- Weitere Informationen zu ExpressRoute finden Sie unter [ExpressRoute – Technische Übersicht](expressroute-introduction.md).


 

<!---HONumber=AcomDC_0921_2016-->