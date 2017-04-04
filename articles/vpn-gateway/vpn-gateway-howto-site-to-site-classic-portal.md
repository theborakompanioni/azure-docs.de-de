---
title: 'Herstellen einer Verbindung zwischen Ihrem lokalen Netzwerk und einem virtuellen Azure-Netzwerk: Site-to-Site-VPN (klassisch): Portal | Microsoft-Dokumentation'
description: "Schritte zum Erstellen einer IPsec-Verbindung zwischen Ihrem lokalen Netzwerk und einem virtuellen Azure-Netzwerk über das öffentliche Internet. Anhand dieser Schritte können Sie über das Portal eine standortübergreifende Site-to-Site-VPN Gateway-Verbindung erstellen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 619ea430b13c16e8e4338413613d5798f36458ba
ms.lasthandoff: 03/25/2017


---
# <a name="create-a-site-to-site-connection-using-the-azure-portal-classic"></a>Erstellen einer Site-to-Site-Verbindung über das Azure-Portal (klassisch)
> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Klassisch – Azure-Portal](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klassisch – Klassisches Portal](vpn-gateway-site-to-site-create.md)
>
>


Eine VPN Gateway-S2S-Verbindung (Site-to-Site) ist eine Verbindung über einen VPN-Tunnel vom Typ „IPsec/IKE“ (IKEv1 oder IKEv2). Für diese Art von Verbindung wird ein lokales VPN-Gerät benötigt, dem eine öffentliche IP-Adresse zugewiesen ist und das nicht hinter einer NAT-Einheit angeordnet ist. Site-to-Site-Verbindungen können für standortübergreifende Konfigurationen und Hybridkonfigurationen verwendet werden.

In diesem Artikel werden das Erstellen eines virtuellen Netzwerks und das Herstellen einer Site-to-Site-VPN-Gatewayverbindung mit Ihrem lokalen Netzwerk mithilfe des klassischen Bereitstellungsmodells und des Azure-Portals beschrieben. 

![Diagramm für die standortübergreifende Site-to-Site-VPN Gateway-Verbindung](./media/vpn-gateway-howto-site-to-site-classic-portal/site-to-site-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Bereitstellungsmodelle und -methoden für Site-to-Site-Verbindungen
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Die folgende Tabelle zeigt die derzeit verfügbaren Bereitstellungsmodelle und -methoden für Site-to-Site-Konfigurationen. Falls ein Artikel mit Konfigurationsschritten verfügbar ist, steht in der Tabelle ein direkter Link zur Verfügung.

[!INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Zusätzliche Konfigurationen
Wenn Sie VNets miteinander verbinden möchten, aber keine Verbindung mit einem lokalen Standort erstellen, finden Sie unter [Konfigurieren einer VNet-zu-VNet-Verbindung](virtual-networks-configure-vnet-to-vnet-connection.md)entsprechende Informationen. Wenn Sie eine Site-to-Site-Verbindung mit einem VNET hinzufügen möchten, das bereits über eine Verbindung verfügt, lesen Sie unter [Hinzufügen einer S2S-Verbindung mit einem VNET mit einer vorhandenen VPN-Gatewayverbindung](vpn-gateway-multi-site.md) nach.

## <a name="before-you-begin"></a>Voraussetzungen
Vergewissern Sie sich vor Beginn der Konfiguration, dass Sie über Folgendes verfügen:

* Ein kompatibles VPN-Gerät (und eine Person, die es konfigurieren kann). Weitere Informationen finden Sie unter [Informationen zu VPN-Geräten](vpn-gateway-about-vpn-devices.md). Wenn Sie sich mit dem Konfigurieren des VPN-Geräts oder mit den IP-Adressbereichen Ihrer lokalen Netzwerkkonfiguration nicht auskennen, müssen Sie sich an eine Person wenden, die Ihnen diese Details liefern kann.
* Eine externe öffentliche IPv4-IP-Adresse für Ihr VPN-Gerät. Diese IP-Adresse darf sich nicht hinter einer NAT befinden.
* Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) aktivieren oder sich für ein [kostenloses Konto](http://azure.microsoft.com/pricing/free-trial) registrieren.
* Derzeit muss in PowerShell der gemeinsam verwendete Schlüssel angegeben und die VPN Gateway-Verbindung erstellt werden. Installieren Sie die aktuelle Version der PowerShell-Cmdlets der Azure-Dienstverwaltung. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs). Bei der Verwendung von PowerShell für diese Konfiguration müssen Sie PowerShell als Administrator ausführen. 

> [!NOTE]
> Beim Konfigurieren einer Standort-zu-Standort-Verbindung ist eine öffentliche IPv4-IP-Adresse für das VPN-Gerät erforderlich.                                                                                                                                                                               
>
>

### <a name="values"></a>Beispielkonfigurationswerte für diese Übung
Sie können die Beispielkonfigurationswerte nutzen, wenn Sie diese Schritte als Übung verwenden:

* **VNET-Name:** TestVNet1
* **Adressraum:** 
    * 10.11.0.0/16
    * 10.12.0.0/16 (für diese Übung optional)
* **Subnetze:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24 (für diese Übung optional)
* **GatewaySubnet:** 10.11.255.0/27
* **Ressourcengruppe:** TestRG1
* **Standort:** USA, Osten
* **DNS-Server:** 8.8.8.8 (für diese Übung optional)
* **Name des lokalen Standorts:** Site2

## <a name="CreatVNet"></a>1. Erstellen eines virtuellen Netzwerks

Wenn Sie ein virtuelles Netzwerk für eine S2S-Verbindung erstellen, müssen Sie Folgendes sicherstellen: Die von Ihnen angegebenen Adressräume dürfen sich nicht mit den Clientadressräumen für die lokalen Standorte überlappen, mit denen Sie eine Verbindung herstellen möchten. Bei überlappenden Subnetzen funktioniert die Verbindung nicht einwandfrei.

* Wenn Sie bereits über ein VNet verfügen, sollten Sie überprüfen, ob die Einstellungen mit Ihrem Entwurf des VPN Gateways kompatibel sind. Achten Sie besonders auf Subnetze, die sich unter Umständen mit anderen Netzwerken überlappen. 

* Falls Sie noch nicht über ein virtuelles Netzwerk verfügen, erstellen Sie eines. Die Screenshots dienen lediglich zur Veranschaulichung. Achten Sie darauf, dass Sie die Werte durch Ihre eigenen Werte ersetzen.

### <a name="to-create-a-virtual-network"></a>So erstellen Sie ein virtuelles Netzwerk

1. Navigieren Sie in einem Browser zum [Azure-Portal](http://portal.azure.com) , und melden Sie sich, falls erforderlich, mit Ihrem Azure-Konto an.
2. Klicken Sie auf **Neu**. Geben Sie im Feld **Marketplace durchsuchen** die Zeichenfolge „Virtual Network“ ein. Klicken Sie in der zurückgegebenen Liste auf **Virtual Network**, um das Blatt **Virtual Network** zu öffnen.

    ![Blatt für die Suche nach „Virtual Network“](./media/vpn-gateway-howto-site-to-site-classic-portal/newvnetportal700.png)
3. Wählen Sie im unteren Bereich des Blatts „Virtual Network“ in der Liste **Bereitstellungsmodell auswählen** die Option **Resource Manager** aus, und klicken Sie dann auf **Erstellen**.

    ![Bereitstellungsmodell auswählen](./media/vpn-gateway-howto-site-to-site-classic-portal/selectmodel.png)
4. Konfigurieren Sie auf dem Blatt **Virtuelles Netzwerk erstellen** die VNet-Einstellungen. Auf diesem Blatt fügen Sie Ihren ersten Adressraum und einen Adressbereich des Subnetzes hinzu. Nachdem Sie die Erstellung des VNet abgeschlossen haben, können Sie zurückgehen und weitere Subnetze und Adressräume hinzufügen.

    ![Blatt „Virtuelles Netzwerk erstellen“](./media/vpn-gateway-howto-site-to-site-classic-portal/createvnet.png "Blatt „Virtuelles Netzwerk erstellen“")
5. Überprüfen Sie, ob es sich um das richtige **Abonnement** handelt. Das Abonnement kann über die Dropdownliste geändert werden.
6. Klicken Sie auf **Ressourcengruppe** , und wählen Sie entweder eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue, indem Sie einen Namen für die neue Ressourcengruppe eingeben. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Wählen Sie als Nächstes für das VNet die Einstellungen für **Standort** aus. Der Standort gibt an, wo sich die in diesem VNet bereitgestellten Ressourcen befinden.
8. Wählen Sie **An Dashboard anheften** aus, wenn das VNet komfortabel auf dem Dashboard zur Verfügung stehen soll, und klicken Sie dann auf **Erstellen**.

    ![An Dashboard anheften](./media/vpn-gateway-howto-site-to-site-classic-portal/pintodashboard150.png "An Dashboard anheften")
9. Nach dem Klicken auf „Erstellen“ wird auf dem Dashboard eine Kachel angezeigt, mit der der Status des VNet angegeben wird. Die Kachel verändert sich, wenn das VNet erstellt wird.

    ![Kachel „Virtuelles Netzwerk wird erstellt“](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Virtuelles Netzwerk wird erstellt")

Nachdem das virtuelle Netzwerk erstellt wurde, wird im klassischen Azure-Portal auf der Seite mit den Netzwerken unter **Status** der Eintrag **Erstellt** angezeigt.

## <a name="additionaladdress"></a>2. Hinzufügen weiterer Adressräume

Nach dem Erstellen Ihres virtuellen Netzwerks können Sie weitere Adressräume hinzufügen. Das Hinzufügen von zusätzlichen Adressräumen ist für eine S2S-Konfiguration nicht obligatorisch, aber Sie können die folgenden Schritte ausführen, falls Sie mehrere Adressräume benötigen:

1. Suchen Sie im Portal nach den virtuellen Netzwerken.
2. Klicken Sie auf dem Blatt für Ihr virtuelles Netzwerk im Abschnitt **Einstellungen** auf **Adressraum**.
3. Klicken Sie auf dem Blatt „Adressraum“ auf **+Hinzufügen**, und geben Sie weitere Adressräume ein.
 
## <a name="dns"></a>3. Angeben eines DNS-Servers
Die DNS-Einstellungen sind für eine S2S-Konfiguration nicht obligatorisch, aber das DNS wird benötigt, wenn Sie die Namensauflösung nutzen möchten.

Nach Erstellung des virtuellen Netzwerks können Sie für die Namensauflösung die IP-Adresse eines DNS-Servers hinzufügen. Öffnen Sie die Einstellungen für Ihr virtuelles Netzwerk, klicken Sie auf „DNS-Server“, und fügen Sie die IP-Adresse des gewünschten DNS-Servers hinzu, den Sie für die Namensauflösung verwenden möchten. Mit dieser Einstellung wird kein DNS-Server erstellt. In den Beispieleinstellungen verwenden wir einen öffentlichen DNS-Server. Meist ist es ratsam, einen privaten DNS-Server zu nutzen. Achten Sie darauf, dass Sie einen DNS-Server hinzufügen, mit dem Ihre Ressourcen kommunizieren können.

1. Suchen Sie im Portal nach den virtuellen Netzwerken.
2. Klicken Sie auf dem Blatt für Ihr virtuelles Netzwerk im Abschnitt **Einstellungen** auf **DNS-Server**.
3. Fügen Sie einen DNS-Server hinzu.
4. Klicken Sie oben auf der Seite auf **Speichern**, um Ihre Einstellungen zu speichern.
 
## <a name="localsite"></a>4. Konfigurieren des lokalen Standorts

Mit dem lokalen Standort ist in der Regel Ihr lokaler Standort gemeint. Er enthält die IP-Adresse des VPN-Geräts, mit dem Sie eine Verbindung herstellen, und die IP-Adressbereiche, die über das VPN-Gateway an das VPN-Gerät weitergeleitet werden.

1. Navigieren Sie im Portal zu dem virtuellen Netzwerk, für das Sie ein Gateway erstellen möchten.
2. Klicken Sie auf dem Blatt für Ihr virtuelles Netzwerk auf dem Blatt **Übersicht** im Abschnitt mit den VPN-Verbindungen auf **Gateway**, um das Blatt **Neue VPN-Verbindung** zu öffnen.

    ![Klicken zum Konfigurieren der Gatewayeinstellungen](./media/vpn-gateway-howto-site-to-site-classic-portal/beforegw125.png "Klicken zum Konfigurieren der Gatewayeinstellungen")
3. Wählen Sie auf dem Blatt **Neue VPN-Verbindung** die Option **Standort-zu-Standort**.

    ![Klicken auf „Standort-zu-Standort“](./media/vpn-gateway-howto-site-to-site-classic-portal/site-to-site.png "Standort-zu-Standort")
4. Klicken Sie auf **Lokaler Standort – Erforderliche Einstellungen konfigurieren**, um das Blatt **Lokaler Standort** zu öffnen. Konfigurieren Sie die Einstellungen, und klicken Sie dann auf **OK**, um die Einstellungen zu speichern.
    - **Name:** Erstellen Sie einen Namen für Ihren lokalen Standort, damit Sie ihn leicht identifizieren können.
    - **IP-Adresse des VPN-Gateways:** Dies ist die öffentliche IP-Adresse des VPN-Geräts für Ihr lokales Netzwerk. Für das VPN-Gerät wird eine öffentliche IPv4-IP-Adresse benötigt. Geben Sie eine gültige öffentliche IP-Adresse für das VPN-Gerät an, mit dem Sie eine Verbindung herstellen möchten. Sie darf sich nicht hinter einer NAT befinden und muss für Azure erreichbar sein.
    - **Clientadressraum:** Listen Sie die IP-Adressbereiche auf, die über dieses Gateway an das lokale Netzwerk weitergeleitet werden sollen. Sie können mehrere Adressraumbereiche hinzufügen. Stellen Sie sicher, dass sich die hier angegebenen Bereiche nicht mit den Bereichen anderer Netzwerke, mit denen für Ihr virtuelles Netzwerk Verbindungen hergestellt werden, oder mit den Adressbereichen des virtuellen Netzwerks selbst überlappen.

    ![Lokaler Standort](./media/vpn-gateway-howto-site-to-site-classic-portal/localnetworksite.png "Konfigurieren des lokalen Standorts")

## <a name="gatewaysubnet"></a>5. Konfigurieren des Gatewaysubnetzes

Sie müssen für Ihr VPN-Gateway ein Gatewaysubnetz erstellen. Das Gatewaysubnetz enthält die IP-Adressen, die von den Diensten des VPN-Gateways verwendet werden.

1. Aktivieren Sie auf dem Blatt **Neue VPN-Verbindung** das Kontrollkästchen **Gateway sofort erstellen**. Das Blatt „Optionale Gatewaykonfiguration“ wird angezeigt. Wenn Sie das Kontrollkästchen nicht aktivieren, wird das Blatt zum Konfigurieren des Gatewaysubnetzes nicht angezeigt.

    ![Gatewaykonfiguration – Subnetz, Größe, Routingtyp](./media/vpn-gateway-howto-site-to-site-classic-portal/optional.png "Gatewaykonfiguration – Subnetz, Größe, Routingtyp")
2. Klicken Sie auf **Optionale Gatewaykonfiguration – Subnetz, Größe und Routingtyp**, um das Blatt **Gatewaykonfiguration** zu öffnen.
3. Klicken Sie auf dem Blatt **Gatewaykonfiguration** auf **Subnetz – Erforderliche Einstellungen konfigurieren**, um das Blatt **Subnetz hinzufügen** zu öffnen.

    ![Gatewaykonfiguration – Gatewaysubnetz](./media/vpn-gateway-howto-site-to-site-classic-portal/subnetrequired.png "Gatewaykonfiguration – Gatewaysubnetz")
4. Fügen Sie auf dem Blatt **Subnetz hinzufügen** das Gatewaysubnetz hinzu. Verwenden Sie beim Hinzufügen des Gatewaysubnetzes nach Möglichkeit einen CIDR-Block vom Typ /28 oder /27. So wird sichergestellt, dass Sie über genügend IP-Adressen verfügen, um zukünftige zusätzliche Konfigurationsanforderungen erfüllen zu können.  Klicken Sie auf **OK** , um die Einstellungen zu speichern.

    ![Hinzufügen eines Gatewaysubnetzes](./media/vpn-gateway-howto-site-to-site-classic-portal/addgwsubnet.png "Hinzufügen eines Gatewaysubnetzes")

## <a name="sku"></a>6. Angeben der SKU und des VPN-Typs
1. Wählen Sie die Gatewaygröße****aus. Hierbei handelt es sich um die Gateway-SKU, die Sie bei der Erstellung Ihres virtuellen Netzwerkgateways verwenden. Im Portal ist standardmäßig die SKU **Basic** ausgewählt. Weitere Informationen zu Gateway-SKUs finden Sie unter [Informationen zu VPN Gateway-Einstellungen](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![Auswählen der SKU und des VPN-Typs](./media/vpn-gateway-howto-site-to-site-classic-portal/sku.png "Auswählen der SKU und des VPN-Typs")
2. Wählen Sie den **Routingtyp** für Ihr Gateway aus. Dies wird auch als VPN-Typ bezeichnet. Es ist wichtig, den richtigen Gatewaytyp auszuwählen, weil es nicht möglich ist, das Gateway in einen anderen Typ zu konvertieren. Ihr VPN-Gerät muss mit dem Routingtyp kompatibel sein, den Sie auswählen. Weitere Informationen zum VPN-Typ finden Sie unter [Informationen zu VPN Gateway-Einstellungen](vpn-gateway-about-vpn-gateway-settings.md#vpntype). Es kann sein, dass in Artikeln die VPN-Typen „RouteBased“ und „PolicyBased“ vorkommen. „Dynamisch“ entspricht „RouteBased“, und „Statisch“ entspricht „PolicyBased“.
3. Klicken Sie auf **OK** , um die Einstellungen zu speichern.
4. Klicken Sie am unteren Rand des Blatts **Neue VPN-Verbindung** auf **OK**, um mit der Erstellung Ihres virtuellen Netzwerkgateways zu beginnen. Dies kann bis zu 45 Minuten dauern.

## <a name="vpndevice"></a>7. Konfigurieren des VPN-Geräts

Halten Sie mit Ihrem Gerätehersteller für spezifische Informationen zur Konfiguration Rücksprache, und konfigurieren Sie das Gerät. Weitere Informationen zu VPN-Geräten, die problemlos mit Azure eingesetzt werden können, finden Sie unter [VPN-Geräte](vpn-gateway-about-vpn-devices.md) . Führen Sie außerdem eine Überprüfung auf [bekannte Probleme mit der Gerätekompatibilität](vpn-gateway-about-vpn-devices.md#known) für das VPN-Gerät durch, das Sie verwenden möchten. 

Beim Konfigurieren Ihres VPN-Geräts benötigen Sie die IP-Adresse des von Ihnen erstellten VPN-Gateways. Sie können sie ermitteln, indem Sie zum Blatt **Übersicht** für Ihr virtuelles Netzwerk navigieren.

## <a name="CreateConnection"></a>8. Erstellen der Verbindung
In diesem Schritt legen Sie den gemeinsam verwendeten Schlüssel fest und erstellen die Verbindung. Der von Ihnen festgelegte Schlüssel muss der gleiche Schlüssel sein, der in Ihrer VPN-Gerätekonfiguration verwendet wurde.

> [!NOTE]
> Derzeit ist dieser Schritt im Azure-Portal nicht verfügbar. Sie müssen die Dienstverwaltungsversion der Azure PowerShell-Cmdlets verwenden.                                                                                                                                                                             
>
>

### <a name="step-1-connect-to-your-azure-account"></a>Schritt 1: Herstellen einer Verbindung mit Ihrem Azure-Konto

1. Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und stellen Sie eine Verbindung mit Ihrem Konto her. Verwenden Sie das folgende Beispiel, um eine Verbindung herzustellen:

        Login-AzureRmAccount
2. Überprüfen Sie die Abonnements für das Konto.

        Get-AzureRmSubscription
3. Wenn Sie über mehr als ein Abonnement verfügen, wählen Sie das Abonnement aus, das Sie verwenden möchten.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
4. Fügen Sie die Dienstverwaltungsversion der PowerShell-Cmdlets hinzu.

        Add-AzureAccount

### <a name="step-2-set-the-shared-key-and-create-the-connection"></a>Schritt 2: Festlegen des gemeinsam verwendeten Schlüssels und Erstellen der Verbindung

Beim Verwenden von PowerShell und des klassischen Bereitstellungsmodells sind die Namen der Ressourcen im Portal in einigen Fällen nicht die Namen, die Azure in Bezug auf die Nutzung von PowerShell erwartet. Mit den folgenden Schritten können Sie die Netzwerkkonfigurationsdatei exportieren, um die genauen Werte für die Namen zu erhalten.

1. Erstellen Sie auf Ihrem Computer ein Verzeichnis, und exportieren Sie die Netzwerkkonfigurationsdatei in das Verzeichnis. In diesem Beispiel wird die Netzwerkkonfigurationsdatei in das Verzeichnis „C:\AzureNet“ exportiert.

         Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
2. Öffnen Sie die Netzwerkkonfigurationsdatei mit einem XML-Editor, und überprüfen Sie die Werte für „LocalNetworkSite name“ und „VirtualNetworkSite name“. Ändern Sie das Beispiel, um Ihre Werte zu nutzen. Setzen Sie den Wert in einfache Anführungszeichen, wenn ein Name Leerzeichen enthält.

3. Legen Sie den gemeinsam verwendeten Schlüssel fest, und erstellen Sie die Verbindung. „-SharedKey“ ist ein Wert, den Sie generieren und angeben. Im Beispiel wurde „abc123“ verwendet, aber Sie können (und sollten) einen komplexeren Wert generieren und verwenden. Entscheidend ist Folgendes: Der Wert, den Sie hier angeben, muss dem Wert entsprechen, den Sie beim Konfigurieren Ihres VPN-Geräts angegeben haben.

        Set-AzureVNetGatewayKey -VNetName 'Group TestRG1 TestVNet1' `
        -LocalNetworkSiteName 'D1BFC9CB_Site2' -SharedKey abc123
Wenn die Verbindung erstellt wird, lautet das Ergebnis: **Status: Successful** (Status: Erfolgreich).

## <a name="verify"></a>9. Überprüfen der Verbindung

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="next-steps"></a>Nächste Schritte
Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter [Virtuelle Computer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) .


