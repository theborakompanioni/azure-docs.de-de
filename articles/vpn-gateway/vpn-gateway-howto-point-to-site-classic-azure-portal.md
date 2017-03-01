---
title: 'Herstellen einer Point-to-Site-Verbindung zwischen einem Computer und einem virtuellen Azure-Netzwerk: Azure-Portal: klassisch | Microsoft-Dokumentation'
description: "Stellen Sie über das klassische Azure-Portal eine sichere P2S-VPN Gateway-Verbindung mit Ihrem Azure Virtual Network her."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 65e14579-86cf-4d29-a6ac-547ccbd743bd
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/17/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: cf72197aba2c6e6c7a51f96d1161cf1fbe88a0c5
ms.openlocfilehash: eb56c702224ab4503051cbee0c3678a7e8833d2b
ms.lasthandoff: 02/18/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Konfigurieren einer Point-to-Site-Verbindung mit einem VNet über das Azure-Portal
> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klassisch – Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Mit einer P2S-Konfiguration (Point-to-Site) können Sie von einem einzelnen Clientcomputer eine sichere Verbindung mit einem virtuellen Netzwerk herstellen. Eine P2S-Verbindung ist nützlich, wenn Sie von einem Remotestandort, z.B. von zu Hause oder in einer Konferenz, eine Verbindung mit Ihrem VNet herstellen möchten. Diese Methode eignet sich auch, wenn Sie nur wenige Clients besitzen, die mit einem virtuellen Netzwerk verbunden werden müssen.

Damit Punkt-zu-Standort-Verbindungen funktionieren, ist kein VPN-Gerät und keine öffentliche IP-Adresse erforderlich. Eine VPN-Verbindung wird hergestellt, indem Sie die Verbindung vom Clientcomputer aus starten. Weitere Informationen zu Point-to-Site-Verbindungen finden Sie unter [Point-to-Site – Häufig gestellte Fragen](#faq) am Ende dieses Artikels.

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie über das Azure-Portal im Rahmen des klassischen Bereitstellungsmodells ein VNet mit einer P2S-Verbindung (Point-to-Site) erstellen.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Bereitstellungsmodelle und -methoden für P2S-Verbindungen
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Die folgende Tabelle zeigt die beiden Bereitstellungsmodelle und die verfügbaren Bereitstellungsmethoden für P2S-Konfigurationen. Falls ein Artikel mit Konfigurationsschritten verfügbar ist, steht in der Tabelle ein direkter Link zur Verfügung.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Grundlegender Workflow
![P2S-Diagramm](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/point-to-site-connection-diagram.png)

In den folgenden Abschnitten werden die Schritte zum Erstellen einer sicheren P2S-Verbindung mit einem virtuellen Netzwerk beschrieben.

1. Erstellen eines virtuelles Netzwerks und eines VPN-Gateways
2. Generieren von Zertifikaten
3. Hochladen der CER-Datei
4. Generieren des VPN-Clientkonfigurationspakets
5. Konfigurieren des Clientcomputers
6. Herstellen einer Verbindung mit Azure

### <a name="example-settings"></a>Beispieleinstellungen
Beispieleinstellungen:

* **Name: VNet1**
* **Adressraum: 192.168.0.0/16**
* **Subnetzname: FrontEnd**
* **Subnetzadressbereich: 192.168.1.0/24**
* **Abonnement:** Falls Sie über mehrere Abonnements verfügen, vergewissern Sie sich, dass Sie das richtige Abonnement verwenden.
* **Ressourcengruppe: TestRG**
* **Standort: USA, Osten**
* **Verbindungstyp: Point-to-Site**
* **Clientadressraum: 172.16.201.0/24**. VPN-Clients, die über diese Point-to-Site-Verbindung eine Verbindung mit dem VNet herstellen, erhalten eine IP-Adresse aus dem angegebenen Pool.
* **GatewaySubnet: 192.168.200.0/24**. Das Gatewaysubnetz muss den Namen „GatewaySubnet“ haben.
* **Größe:** Wählen Sie die gewünschte Gateway-SKU aus.
* **Routingtyp: Dynamisch**

## <a name="a-namevnetvpnasection-1---create-a-virtual-network-and-a-vpn-gateway"></a><a name="vnetvpn"></a>Abschnitt 1: Erstellen eines virtuellen Netzwerks und eines VPN-Gateways
### <a name="a-namecreatevnetapart-1-create-a-virtual-network"></a><a name="createvnet"></a>Teil 1: Erstellen eines virtuellen Netzwerks
Falls Sie noch nicht über ein virtuelles Netzwerk verfügen, erstellen Sie eines. Die Screenshots dienen lediglich zur Veranschaulichung. Achten Sie darauf, dass Sie die Werte durch Ihre eigenen Werte ersetzen. Gehen Sie wie folgt vor, um ein VNet über das Azure-Portal zu erstellen:

1. Navigieren Sie in einem Browser zum [Azure-Portal](http://portal.azure.com) , und melden Sie sich, falls erforderlich, mit Ihrem Azure-Konto an.
2. Klicken Sie auf **Neu**. Geben Sie im Feld **Marketplace durchsuchen** die Zeichenfolge „Virtual Network“ ein. Klicken Sie in der zurückgegebenen Liste auf **Virtual Network**, um das Blatt **Virtual Network** zu öffnen.

    ![Blatt für die Suche nach „Virtual Network“](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png)
3. Wählen Sie im unteren Bereich des Blatts „Virtual Network“ in der Liste **Bereitstellungsmodell auswählen** die Option **Resource Manager** aus, und klicken Sie dann auf **Erstellen**.

    ![Bereitstellungsmodell auswählen](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png)
4. Konfigurieren Sie auf dem Blatt **Virtuelles Netzwerk erstellen** die VNet-Einstellungen. In diesem Blatt fügen Sie Ihren ersten Adressraum und einen Adressbereich des Subnetzes hinzu. Nachdem Sie die Erstellung des VNet abgeschlossen haben, können Sie zurückgehen und weitere Subnetze und Adressräume hinzufügen.

    ![Blatt "Erstellen eines virtuellen Netzwerks"](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png)
5. Überprüfen Sie, ob es sich um das richtige **Abonnement** handelt. Das Abonnement kann über die Dropdownliste geändert werden.
6. Klicken Sie auf **Ressourcengruppe** , und wählen Sie entweder eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue, indem Sie einen Namen für die neue Ressourcengruppe eingeben. Geben Sie der Ressourcengruppe beim Erstellen einer neuen Gruppe einen Namen, der zu Ihren geplanten Konfigurationswerten passt. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Wählen Sie als Nächstes für das VNet die Einstellungen für **Standort** aus. Der Standort gibt an, wo sich die in diesem VNet bereitgestellten Ressourcen befinden.
8. Wählen Sie **An Dashboard anheften** aus, wenn das VNet komfortabel auf dem Dashboard zur Verfügung stehen soll, und klicken Sie dann auf **Erstellen**.

    ![An Dashboard anheften](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png)
9. Nach dem Klicken auf „Erstellen“ wird auf dem Dashboard eine Kachel angezeigt, mit der der Status des VNet angegeben wird. Die Kachel verändert sich, wenn das VNet erstellt wird.

    ![Kachel "Erstellen eines virtuellen Netzwerks"](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png)
10. Nach Erstellung des virtuellen Netzwerks können Sie für die Namensauflösung die IP-Adresse eines DNS-Servers hinzufügen. Öffnen Sie die Einstellungen für Ihr virtuelles Netzwerk, klicken Sie auf „DNS-Server“, und fügen Sie die IP-Adresse des gewünschten DNS-Servers hinzu. Mit dieser Einstellung wird kein neuer DNS-Server erstellt. Achten Sie darauf, dass Sie einen DNS-Server hinzufügen, mit dem Ihre Ressourcen kommunizieren können.

Nachdem das virtuelle Netzwerk erstellt wurde, wird im klassischen Azure-Portal auf der Seite mit den Netzwerken unter **Status** der Eintrag **Erstellt** angezeigt.

### <a name="a-namegatewayapart-2-create-gateway-subnet-and-a-dynamic-routing-gateway"></a><a name="gateway"></a>Teil 2: Erstellen eines Gatewaysubnetzes und eines Gateways mit dynamischem Routing
In diesem Schritt erstellen Sie ein Gatewaysubnetz und ein Gateways mit dynamischem Routing. Im Azure-Portal für das klassische Bereitstellungsmodell können Gatewaysubnetz und Gateway über die gleichen Konfigurationsblätter erstellt werden.

1. Navigieren Sie im Portal zu dem virtuellen Netzwerk, für das Sie ein Gateway erstellen möchten.
2. Klicken Sie auf dem Blatt für Ihr virtuelles Netzwerk auf dem Blatt **Übersicht** im Abschnitt mit den VPN-Verbindungen auf **Gateway**.

    ![Klicken zum Erstellen eines Gateways](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png)
3. Wählen Sie auf dem Blatt **Neue VPN-Verbindung** die Option **Punkt-zu-Standort** aus.

    ![Point-to-Site-Verbindungstyp](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png)
4. Fügen Sie unter **Clientadressraum** den IP-Adressbereich hinzu. Hierbei handelt es sich um den Bereich, aus dem die VPN-Clients bei der Verbindungsherstellung eine IP-Adresse erhalten. Ersetzen Sie den automatisch ausgefüllten Bereich durch Ihren eigenen.

    ![Clientadressraum](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png)
5. Aktivieren Sie das Kontrollkästchen **Gateway sofort erstellen**.

    ![Gateway sofort erstellen](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png)
6. Klicken Sie auf **Optionale Gatewaykonfiguration**, um das Blatt **Gatewaykonfiguration** zu öffnen.

    ![Klicken Sie auf „Optionale Gatewaykonfiguration“.](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png)
7. Klicken Sie auf **Subnetz > Erforderliche Einstellungen konfigurieren**, um das **Gatewaysubnetz** hinzuzufügen. Es ist zwar möglich, ein Gatewaysubnetz von /29 zu erstellen, es wird jedoch empfohlen, ein größeres Subnetz mit mehr Adressen zu erstellen und mindestens /28 oder /27 auszuwählen. Dadurch steht eine ausreichend hohe Anzahl von Adressen für mögliche zusätzliche Konfigurationen zur Verfügung, die Sie zukünftig vielleicht benötigen.

   > [!IMPORTANT]
   > Vermeiden Sie bei der Verwendung von Gatewaysubnetzen die Zuordnung einer Netzwerksicherheitsgruppe (NSG) zum Gatewaysubnetz. Das Zuordnen einer Netzwerksicherheitsgruppe zu diesem Subnetz kann dazu führen, dass das VPN-Gateway nicht mehr wie erwartet funktioniert. Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie unter [Was ist eine Netzwerksicherheitsgruppe (NSG)?](../virtual-network/virtual-networks-nsg.md).
   >
   >

    ![Hinzufügen von „GatewaySubnet“](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png)
8. Wählen Sie die Gatewaygröße** **aus. Hierbei handelt es sich um die Gateway-SKU, die Sie bei der Erstellung Ihres virtuellen Netzwerkgateways verwenden. Im Portal ist standardmäßig die SKU **Basic** ausgewählt. Weitere Informationen zu Gateway-SKUs finden Sie unter [Informationen zu VPN Gateway-Einstellungen](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![Gatewaygröße](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
9. Wählen Sie den **Routingtyp** für Ihr Gateway aus. Für P2S-Konfigurationen wird der Routingtyp **Dynamisch** benötigt. Klicken Sie abschließend auf **OK**.

    ![Konfigurieren des Routingtyps](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png)
10. Klicken Sie am unteren Rand des Blatts **Neue VPN-Verbindung** auf **OK**, um mit der Erstellung Ihres virtuellen Netzwerkgateways zu beginnen. Dies kann bis zu 45 Minuten dauern.

## <a name="a-namegeneratecertsasection-2---generate-certificates"></a><a name="generatecerts"></a>Abschnitt 2: Generieren von Zertifikaten
Zertifikate werden von Azure zur Authentifizierung von VPN-Clients für Point-to-Site-VPNs verwendet. Die Daten des öffentlichen Zertifikats (nicht der private Schlüssel) werden als x.509-CER-Datei mit Base-64-Codierung exportiert – entweder aus einem mit einer Unternehmenszertifikatlösung generierten Stammzertifikat oder aus einem selbstsignierten Stammzertifikat. Anschließend werden die Daten des öffentlichen Zertifikats aus dem Stammzertifikat in Azure importiert. Darüber hinaus müssen Sie auf der Grundlage des Stammzertifikats für Clients ein Clientzertifikat generieren. Auf jedem Client, der eine P2S-Verbindung mit dem virtuellen Netzwerk herstellen möchte, muss ein Clientzertifikat installiert sein, das auf der Grundlage des Stammzertifikats generiert wurde.

### <a name="a-namecerapart-1-obtain-the-cer-file-for-the-root-certificate"></a><a name="cer"></a>Teil 1: Beschaffen der CER-Datei für ein Stammzertifikat
Bei einer Unternehmenslösung können Sie Ihre vorhandene Zertifikatkette verwenden. Wenn Sie keine Lösung einer Unternehmenszertifizierungsstelle verwenden, können Sie ein selbstsigniertes Stammzertifikat erstellen. Die empfohlene Methode zum Erstellen eines selbstsignierten Zertifikats für P2S-Verbindungen ist [makecert](vpn-gateway-certificates-point-to-site.md). Sie können zwar auch PowerShell zum Erstellen von selbstsignierten Zertifikaten verwenden, aber das von PowerShell generierte Zertifikat enthält nicht die für P2S-Verbindungen benötigten Felder.

* Wenn Sie eine Unternehmenszertifikatlösung verwenden, müssen Sie die CER-Datei für das gewünschte Stammzertifikat beschaffen.
* Wenn Sie keine Unternehmenszertifikatlösung verwenden, müssen Sie [mit makecert ein selbstsigniertes Stammzertifikat generieren](vpn-gateway-certificates-point-to-site.md).


1. Um eine CER-Datei auf der Grundlage eines Zertifikats zu generieren, öffnen Sie **certmgr.msc**, und suchen das Stammzertifikat. Klicken Sie mit der rechten Maustaste auf das selbstsignierte Stammzertifikat, klicken Sie auf **Alle Aufgaben** und dann auf **Exportieren**. Dadurch wird der **Zertifikatexport-Assistent**geöffnet.
2. Klicken Sie im Assistenten auf **Weiter**, wählen Sie **Nein, privaten Schlüssel nicht exportieren** aus, und klicken Sie dann auf **Weiter**.
3. Wählen Sie auf der Seite **Dateiformat für den Export** die Option **Base-64-codiert X.509 (.CER)** aus. Klicken Sie auf **Weiter**.
4. Wählen Sie unter **Zu exportierende Datei** die Option **Durchsuchen** aus, um zu dem Speicherort zu wechseln, an den das Zertifikat exportiert werden soll. Geben Sie unter **Dateiname**einen Namen für die Zertifikatdatei ein. Klicken Sie auf **Weiter**.
5. Klicken Sie auf **Fertig stellen** , um das Zertifikat zu exportieren.

### <a name="a-namegenclientcertapart-2-generate-a-client-certificate"></a><a name="genclientcert"></a>Teil 2: Generieren eines Clientzertifikats
Sie können entweder ein eindeutiges Zertifikat für jeden Client generieren, mit dem eine Verbindung hergestellt wird, oder Sie können dasselbe Zertifikat für mehrere Clients verwenden. Der Vorteil beim Generieren von eindeutigen Clientzertifikaten besteht darin, dass Sie bei Bedarf ein einzelnes Zertifikat widerrufen können. Falls überall das gleiche Clientzertifikat verwendet wird und Sie das Zertifikat für einen Client sperren müssen, müssen Sie sonst neue Zertifikate für alle Clients generieren und installieren, die das Zertifikat für die Authentifizierung verwenden.

####<a name="enterprise-certificate"></a>Unternehmenszertifikat
- Generieren Sie bei Verwendung einer Unternehmenszertifikatlösung ein Clientzertifikat mit dem gängigen Name-Wert-Format 'name@yourdomain.com', (anstatt des Formats „Domänenname\Benutzername“).
- Stellen Sie sicher, dass das von Ihnen ausgestellte Clientzertifikat auf der Zertifikatvorlage „User“ basiert, das als ersten Eintrag in der Nutzungsliste „Client Authentication“ enthält, anstatt „Smart Card Logon“ usw. Sie können das Zertifikat überprüfen, indem Sie auf das Clientzertifikat doppelklicken und **Details > Erweiterte Schlüsselverwendung** anzeigen.

####<a name="self-signed-certificate"></a>Selbstsigniertes Zertifikat 
Wenn Sie ein selbstsigniertes Zertifikat verwenden, helfen Ihnen die Informationen zum Generieren eines Clientzertifikats unter [Arbeiten mit selbstsignierten Stammzertifikaten für P2S-Konfigurationen](vpn-gateway-certificates-point-to-site.md) weiter.

### <a name="a-nameexportclientcertapart-3-export-the-client-certificate"></a><a name="exportclientcert"></a>Teil 3: Exportieren des Clientzertifikats
Installieren Sie auf jedem Computer, den Sie mit dem virtuellen Netzwerk verbinden möchten, ein Clientzertifikat. Ein Clientzertifikat wird für die Authentifizierung benötigt. Sie können die Installation des Clientzertifikats automatisieren oder das Zertifikat manuell installieren. In den folgenden Schritten wird das manuelle Exportieren und Installieren des Clientzertifikats beschrieben.

1. Sie können *certmgr.msc*zum Exportieren eines Clientzertifikats verwenden. Klicken Sie mit der rechten Maustaste auf das Clientzertifikat, das Sie exportieren möchten, klicken Sie auf **Alle Aufgaben** und anschließend auf **Exportieren**.
2. Exportieren Sie das Clientzertifikat mit dem privaten Schlüssel. Dies ist eine *PFX* -Datei. Vergessen Sie nicht, sich das Kennwort (Schlüssel) zu notieren oder zu merken, das Sie für dieses Zertifikat festgelegt haben.

## <a name="a-nameuploadasection-3---upload-the-root-certificate-cer-file"></a><a name="upload"></a>Abschnitt 3: Hochladen der CER-Datei des Stammzertifikats
Nach Erstellung des Gateways können Sie die CER-Datei für ein vertrauenswürdiges Stammzertifikat in Azure hochladen. Sie können Dateien für bis zu 20 Stammzertifikate hochladen. Der private Schlüssel für das Stammzertifikat wird nicht in Azure hochgeladen. Die hochgeladene CER-Datei wird von Azure zur Authentifizierung von Clients verwendet, die eine Verbindung mit dem virtuellen Netzwerk herstellen.

1. Klicken Sie auf dem Blatt für Ihr VNet im Abschnitt **VPN-Verbindungen** auf die** **Clientgrafik, um das Blatt **Punkt-zu-Standort-VPN-Verbindung** zu öffnen.

    ![Clients](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. Klicken Sie auf dem Blatt **Punkt-zu-Standort-Verbindung** auf **Zertifikate verwalten**, um das Blatt **Zertifikate** zu öffnen.<br>

    ![Blatt „Zertifikate“](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. Klicken Sie auf dem Blatt **Zertifikate** auf **Hochladen**, um das Blatt **Zertifikat hochladen** zu öffnen.<br>

    ![Blatt zum Hochladen von Zertifikaten](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png)<br>
4. Klicken Sie auf die Ordnergrafik, um zur CER-Datei zu navigieren. Wählen Sie die Datei aus, und klicken Sie anschließend auf **OK**. Aktualisieren Sie die Seite, damit das hochgeladene Zertifikat auf dem Blatt **Zertifikate** angezeigt wird.

    ![Hochladen des Zertifikats](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png)<br>

## <a name="a-namevpnclientconfigasection-4---generate-the-vpn-client-configuration-package"></a><a name="vpnclientconfig"></a>Abschnitt 4: Generieren des VPN-Clientkonfigurationspakets
Zum Herstellen einer Verbindung mit dem virtuellen Netzwerk müssen Sie auch einen VPN-Client konfigurieren. Der Clientcomputer benötigt sowohl ein Clientzertifikat als auch das richtige VPN-Clientkonfigurationspaket, um eine Verbindung herstellen zu können.

Das VPN-Clientpaket enthält Konfigurationsinformationen zum Konfigurieren der VPN-Client-Software, die in Windows integriert. Das Paket installiert keine zusätzlichen Software. Die Einstellungen gelten speziell für das virtuelle Netzwerk, mit dem Sie eine Verbindung herstellen möchten. Die Liste mit den unterstützten Clientbetriebssystemen finden Sie unter [Point-to-Site – Häufig gestellte Fragen](#faq) am Ende dieses Artikels.

### <a name="to-generate-the-vpn-client-configuration-package"></a>So generieren Sie das VPN-Clientkonfigurationspaket
1. Klicken Sie im Azure-Portal auf dem Übersichtsblatt** **für Ihr VNet im Abschnitt **VPN-Verbindungen** auf die Clientgrafik, um das Blatt **Punkt-zu-Standort-VPN-Verbindung** zu öffnen.
2. Wählen Sie im oberen Bereich des Blatts **Punkt-zu-Standort-VPN-Verbindung** das Downloadpaket für das Clientbetriebssystem aus, unter dem es installiert wird:

   * Wählen Sie für 64-Bit-Clients das Paket **VPN-Client (64 Bit)** aus.
   * Wählen Sie für 32-Bit-Clients das Paket **VPN-Client (32 Bit)** aus.

     ![Herunterladen des VPN-Clientkonfigurationspakets](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png)<br>
3. Daraufhin erscheint eine Meldung mit dem Hinweis, dass Azure das VPN-Clientkonfigurationspaket für das virtuelle Netzwerk generiert. Wenn das Paket nach einigen Minuten generiert wurde, erscheint auf Ihrem lokalen Computer eine Meldung mit dem Hinweis, dass das Paket heruntergeladen wurde. Speichern Sie die Konfigurationspaketdatei. Diese muss auf jedem Clientcomputer installiert werden, der eine P2S-Verbindung mit dem virtuellen Netzwerk herstellt.

## <a name="a-nameclientconfigurationasection-5---configure-the-client-computer"></a><a name="clientconfiguration"></a>Abschnitt 5: Konfigurieren des Clientcomputers
### <a name="part-1-install-the-client-certificate"></a>Teil 1: Installieren des Clientzertifikats
Jeder Clientcomputer muss über ein Clientzertifikat verfügen, um sich zu authentifizieren. Beim Installieren des Clientzertifikats benötigen Sie das Kennwort, das beim Exportieren des Clientzertifikats erstellt wurde.

1. Kopieren Sie die PFX-Datei auf den Clientcomputer.
2. Doppelklicken Sie auf die PFX-Datei, um sie zu installieren. Ändern Sie den Speicherort der Installation nicht.

### <a name="part-2-install-the-vpn-client-configuration-package"></a>Teil 2: Installieren des VPN-Clientkonfigurationspakets
Sie können auf jedem Clientcomputer das gleiche VPN-Clientkonfigurationspaket verwenden – vorausgesetzt, es handelt sich dabei um die passende Version für die Architektur des jeweiligen Clients.

1. Kopieren Sie die Konfigurationsdatei lokal auf den Computer, den Sie mit dem virtuellen Netzwerk verbinden möchten, und doppelklicken Sie auf die EXE-Datei.
2. Sobald das Paket installiert worden ist, können Sie die VPN-Verbindung starten. Das Konfigurationspaket ist nicht von Microsoft signiert. Sie können das Paket mithilfe des Signaturdiensts Ihres Unternehmens oder mit [SignTool](http://go.microsoft.com/fwlink/p/?LinkId=699327) selbst signieren. Sie können das Paket auch ohne Signatur verwenden. Wenn das Paket nicht signiert ist, wird bei der Installation des Pakets aber eine Warnung angezeigt.
3. Navigieren Sie auf dem Clientcomputer zu **Netzwerkeinstellungen**, und klicken Sie auf **VPN**. Die Verbindung wird angezeigt. Sie zeigt den Namen des virtuellen Netzwerks an, mit dem eine Verbindung hergestellt wird. Das sieht dann in etwa so aus:

    ![VPN-Client](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png)

## <a name="a-nameconnectasection-6---connect-to-azure"></a><a name="connect"></a>Abschnitt 6: Herstellen der Verbindung mit Azure
### <a name="connect-to-your-vnet"></a>Herstellen der Verbindung mit Ihrem VNet
1. Um eine Verbindung mit Ihrem VNet herzustellen, navigieren Sie auf dem Clientcomputer zu „VPN-Verbindungen“ und suchen nach der VPN-Verbindung, die Sie erstellt haben. Sie hat den gleichen Namen wie das virtuelle Netzwerk. Klicken Sie auf **Verbinden**. Möglicherweise wird eine Popupmeldung angezeigt, die sich auf die Verwendung des Zertifikats bezieht. Klicken Sie in diesem Fall auf **Weiter** , um erhöhte Rechte zu verwenden.
2. Klicken Sie auf der Statusseite **Verbindung** auf **Verbinden**, um die Verbindung herzustellen. Wenn der Bildschirm **Zertifikat auswählen** angezeigt wird, vergewissern Sie sich, dass das angezeigte Clientzertifikat dem Zertifikat entspricht, die Sie zum Herstellen der Verbindung verwenden möchten. Wenn dies nicht der Fall ist, verwenden Sie den Dropdownpfeil, um das richtige Zertifikat auszuwählen, und klicken Sie dann auf **OK**.

    ![VPN-Clientverbindung](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png)
3. Die Verbindung sollte jetzt eingerichtet werden.

    ![Verbindung hergestellt](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png)

> [!NOTE]
> Wenn Sie bei Verwendung eines Zertifikats, das mit einer Lösung einer Unternehmenszertifizierungsstelle ausgestellt wurde, Probleme mit der Authentifizierung haben, ist es ratsam, die Authentifizierungsreihenfolge des Clientzertifikats zu überprüfen. Sie können die Reihenfolge der Authentifizierungsliste überprüfen, indem Sie auf das Clientzertifikat doppelklicken und zu **Details > Erweiterte Schlüsselverwendung** navigieren. Achten Sie darauf, dass in der Liste „Client Authentication“ als erster Eintrag aufgeführt ist. Wenn nicht, müssen Sie ein Clientzertifikat basierend auf der User-Vorlage ausstellen, die als ersten Eintrag in der Liste „Client Authentication“ enthält. 
>
>

### <a name="verify-the-vpn-connection"></a>Überprüfen der VPN-Verbindung
1. Um sicherzustellen, dass die VPN-Verbindung aktiv ist, öffnen Sie eine Eingabeaufforderung mit Administratorrechten, und führen Sie *Ipconfig/all*aus.
2. Zeigen Sie die Ergebnisse an. Beachten Sie, dass die IP-Adresse, die Sie erhalten, eine Adresse aus dem Adressbereich der P2S-Verbindung ist, den Sie beim Erstellen des virtuellen Netzwerks angegeben haben. Das Ergebnis sollte etwa wie folgt aussehen:

Beispiel:

    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## <a name="a-namefaqapoint-to-site-faq"></a><a name="faq"></a>Point-to-Site – Häufig gestellte Fragen

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Nächste Schritte
Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter [Virtuelle Computer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) .

