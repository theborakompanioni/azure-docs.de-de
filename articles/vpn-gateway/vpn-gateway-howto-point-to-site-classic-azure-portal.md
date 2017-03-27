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
ms.date: 03/20/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 92e573d7f3ebfbe41c8012068a8262d6fc324da8
ms.lasthandoff: 03/21/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal-classic"></a>Konfigurieren einer Point-to-Site-Verbindung mit einem VNet über das Azure-Portal (klassisch)
> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klassisch – Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Mit einer P2S-Konfiguration (Point-to-Site) können Sie von einem einzelnen Clientcomputer eine sichere Verbindung mit einem virtuellen Netzwerk herstellen. P2S ist eine VPN-Verbindung über SSTP (Secure Socket Tunneling-Protokoll). Point-to-Site-Verbindungen sind nützlich, wenn Sie von einem Remotestandort, z.B. von zu Hause oder bei einer Konferenz, eine Verbindung mit Ihrem VNet herstellen möchten. Diese Methode eignet sich auch, wenn Sie nur wenige Clients besitzen, die mit einem virtuellen Netzwerk verbunden werden müssen. Für P2S-Verbindungen ist kein VPN-Gerät und auch keine öffentliche IP-Adresse erforderlich. Sie stellen die VPN-Verbindung über den Clientcomputer her.

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie über das Azure-Portal im Rahmen des klassischen Bereitstellungsmodells ein VNet mit einer P2S-Verbindung (Point-to-Site) erstellen. Weitere Informationen zu Point-to-Site-Verbindungen finden Sie unter [Point-to-Site – Häufig gestellte Fragen](#faq) am Ende dieses Artikels.


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
* **Adressraum: 192.168.0.0/16**<br>In diesem Beispiel verwenden wir nur einen einzelnen Adressraum. Sie können für Ihr VNet aber auch mehrere Adressräume verwenden.
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



## <a name="vnetvpn"></a>Abschnitt 1: Erstellen eines virtuellen Netzwerks und eines VPN-Gateways

Stellen Sie zunächst sicher, dass Sie über ein Azure-Abonnement verfügen. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial) registrieren.
### <a name="createvnet"></a>Teil 1: Erstellen eines virtuellen Netzwerks
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

### <a name="gateway"></a>Teil 2: Erstellen eines Gatewaysubnetzes und eines Gateways mit dynamischem Routing
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
8. Wählen Sie die Gatewaygröße****aus. Hierbei handelt es sich um die Gateway-SKU, die Sie bei der Erstellung Ihres virtuellen Netzwerkgateways verwenden. Im Portal ist standardmäßig die SKU **Basic** ausgewählt. Weitere Informationen zu Gateway-SKUs finden Sie unter [Informationen zu VPN Gateway-Einstellungen](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![Gatewaygröße](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
9. Wählen Sie den **Routingtyp** für Ihr Gateway aus. Für P2S-Konfigurationen wird der Routingtyp **Dynamisch** benötigt. Klicken Sie abschließend auf **OK**.

    ![Konfigurieren des Routingtyps](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png)
10. Klicken Sie am unteren Rand des Blatts **Neue VPN-Verbindung** auf **OK**, um mit der Erstellung Ihres virtuellen Netzwerkgateways zu beginnen. Dies kann bis zu 45 Minuten dauern.

## <a name="generatecerts"></a>Abschnitt 2: Erstellen von Zertifikaten
Zertifikate werden von Azure zur Authentifizierung von VPN-Clients für Point-to-Site-VPNs verwendet. Nach dem Erstellen des Stammzertifikats exportieren Sie die Daten des öffentlichen Zertifikats (nicht den privaten Schlüssel) als Base64-codierte X.509-CER-Datei. Anschließend laden Sie die Daten des öffentlichen Zertifikats aus dem Stammzertifikat nach Azure hoch.

Auf jedem Clientcomputer, der per Point-to-Site eine Verbindung mit einem VNet herstellt, muss ein Clientzertifikat installiert sein. Das Clientzertifikat wird über das Stammzertifikat generiert und auf jedem Clientcomputer installiert. Wenn kein gültiges Clientzertifikat installiert ist und der Client versucht, eine Verbindung mit dem VNet herzustellen, tritt bei der Authentifizierung ein Fehler auf.

### <a name="cer"></a>Teil 1: Beschaffen des öffentlichen Schlüssels (CER-Format) für das Stammzertifikat

####<a name="enterprise-certificate"></a>Unternehmenszertifikat
 
Bei einer Unternehmenslösung können Sie Ihre vorhandene Zertifikatkette verwenden. Rufen Sie die CER-Datei für das Stammzertifikat ab, das Sie verwenden möchten.

####<a name="self-signed-root-certificate"></a>Selbstsigniertes Stammzertifikat

Wenn Sie keine Unternehmenszertifikatlösung verwenden, müssen Sie ein selbstsigniertes Stammzertifikat generieren. Sie können PowerShell verwenden, um ein selbstsigniertes Zertifikat zu erstellen, das die erforderlichen Felder für die P2S-Authentifizierung enthält. Unter [Arbeiten mit selbstsignierten Zertifikaten für Punkt-zu-Standort-Verbindungen](vpn-gateway-certificates-point-to-site.md) werden die Schritte zum Erstellen eines selbstsignierten Stammzertifikats beschrieben.

> [!NOTE]
> Bisher war „makecert“ die empfohlene Methode, um selbstsignierte Zertifikate zu erstellen und Clientzertifikate für Point-to-Site-Verbindungen zu generieren. Jetzt können Sie diese Zertifikate mit PowerShell erstellen. Ein Vorteil bei der Verwendung von PowerShell ist, dass Sie SHA-2-Zertifikate erstellen können. Die erforderlichen Werte finden Sie unter [Arbeiten mit selbstsignierten Zertifikaten für Punkt-zu-Standort-Verbindungen](vpn-gateway-certificates-point-to-site.md).
>
>

#### <a name="to-export-the-public-key-for-a-self-signed-root-certificate"></a>So exportieren Sie den öffentlichen Schlüssel für ein selbstsigniertes Stammzertifikat

Für Point-to-Site-Verbindungen muss der öffentliche Schlüssel (CER-Format) nach Azure hochgeladen werden. Die folgenden Schritte dienen Ihnen als Hilfe beim Exportieren der CER-Datei für Ihr selbstsigniertes Stammzertifikat.

1. Um eine CER-Datei aus dem Zertifikat zu erhalten, öffnen Sie **certmgr.msc**. Suchen Sie das selbstsignierte Stammzertifikat (in der Regel in „Zertifikate – Aktueller Benutzer\Eigene Zertifikate\Zertifikate“), und klicken Sie mit der rechten Maustaste darauf. Klicken Sie auf **Alle Aufgaben** und anschließend auf **Exportieren**. Dadurch wird der **Zertifikatexport-Assistent**geöffnet.
2. Klicken Sie im Assistenten auf **Weiter**. Wählen Sie **Nein, privaten Schlüssel nicht exportieren** aus, und klicken Sie dann auf **Weiter**.
3. Wählen Sie auf der Seite **Dateiformat für den Export** die Option **Base-64-codiert X.509 (.CER)** aus, und klicken Sie dann auf **Weiter**. 
4. Wählen Sie unter **Zu exportierende Datei** die Option **Durchsuchen** aus, um zu dem Speicherort zu wechseln, an den das Zertifikat exportiert werden soll. Geben Sie unter **Dateiname**einen Namen für die Zertifikatdatei ein. Klicken Sie auf **Weiter**.
5. Klicken Sie auf **Fertig stellen** , um das Zertifikat zu exportieren. Es wird **Der Export war erfolgreich** angezeigt. Klicken Sie auf **OK**, um den Assistenten zu schließen.

### <a name="genclientcert"></a>Teil 2: Generieren eines Clientzertifikats

Sie können entweder ein eindeutiges Zertifikat für jeden Client generieren, mit dem eine Verbindung hergestellt wird, oder Sie können dasselbe Zertifikat für mehrere Clients verwenden. Der Vorteil beim Generieren von eindeutigen Clientzertifikaten besteht darin, dass Sie bei Bedarf ein einzelnes Zertifikat widerrufen können. Falls überall das gleiche Clientzertifikat verwendet wird und Sie das Zertifikat für einen Client sperren müssen, müssen Sie sonst neue Zertifikate für alle Clients generieren und installieren, die das Zertifikat für die Authentifizierung verwenden.

####<a name="enterprise-certificate"></a>Unternehmenszertifikat
- Generieren Sie bei Verwendung einer Unternehmenszertifikatlösung ein Clientzertifikat mit dem gängigen Name-Wert-Format „name@yourdomain.com“ (anstatt des Formats „Domänenname\Benutzername“).
- Stellen Sie sicher, dass das von Ihnen ausgestellte Clientzertifikat auf der Zertifikatvorlage „User“ basiert, das als ersten Eintrag in der Nutzungsliste „Client Authentication“ enthält, anstatt „Smart Card Logon“ usw. Sie können das Zertifikat überprüfen, indem Sie auf das Clientzertifikat doppelklicken und **Details > Erweiterte Schlüsselverwendung** anzeigen.

####<a name="self-signed-root-certificate"></a>Selbstsigniertes Stammzertifikat 
Wenn Sie ein selbstsigniertes Stammzertifikat verwenden, helfen Ihnen die Schritte zum [Generieren eines Clientzertifikats mit PowerShell](vpn-gateway-certificates-point-to-site.md#clientcert) weiter. Hiermit können Sie ein Clientzertifikat generieren, das mit Point-to-Site-Verbindungen kompatibel ist.

### <a name="exportclientcert"></a>Teil 3: Exportieren des Clientzertifikats
Wenn Sie ein Clientzertifikat über ein selbstsigniertes Stammzertifikat mithilfe der [PowerShell](vpn-gateway-certificates-point-to-site.md#clientcert)-Anleitung generieren, wird es automatisch auf dem Computer installiert, den Sie für die Generierung verwendet haben. Falls Sie ein Clientzertifikat auf einem anderen Clientcomputer installieren möchten, müssen Sie es exportieren.

1. Um ein Clientzertifikat zu exportieren, öffnen Sie **certmgr.msc**. Klicken Sie mit der rechten Maustaste auf das Clientzertifikat, das Sie exportieren möchten, klicken Sie auf **Alle Aufgaben** und anschließend auf **Exportieren**. Dadurch wird der **Zertifikatexport-Assistent**geöffnet.
2. Klicken Sie im Assistenten auf **Weiter**, wählen Sie **Ja, privaten Schlüssel exportieren** aus, und klicken Sie dann auf **Weiter**.
3. Übernehmen Sie auf der Seite **Format der zu exportierenden Datei** die Standardwerte. Stellen Sie sicher, dass die Option **Wenn möglich, alle Zertifikate im Zertifizierungspfad einbeziehen** aktiviert ist. Klicken Sie auf **Weiter**.
4. Auf der Seite **Sicherheit** müssen Sie den privaten Schlüssel schützen. Wenn Sie ein Kennwort verwenden möchten, müssen Sie sich das für dieses Zertifikat festgelegte Kennwort unbedingt merken oder notieren. Klicken Sie auf **Weiter**.
5. Wählen Sie unter **Zu exportierende Datei** die Option **Durchsuchen** aus, um zu dem Speicherort zu wechseln, an den das Zertifikat exportiert werden soll. Geben Sie unter **Dateiname**einen Namen für die Zertifikatdatei ein. Klicken Sie auf **Weiter**.
6. Klicken Sie auf **Fertig stellen** , um das Zertifikat zu exportieren.

## <a name="upload"></a>Abschnitt 3: Hochladen der CER-Datei des Stammzertifikats
Nach Erstellung des Gateways können Sie die CER-Datei für ein vertrauenswürdiges Stammzertifikat in Azure hochladen. Sie können Dateien für bis zu 20 Stammzertifikate hochladen. Der private Schlüssel für das Stammzertifikat wird nicht in Azure hochgeladen. Die hochgeladene CER-Datei wird von Azure zur Authentifizierung von Clients verwendet, die eine Verbindung mit dem virtuellen Netzwerk herstellen.

1. Klicken Sie auf dem Blatt für Ihr VNet im Abschnitt **VPN-Verbindungen** auf die****Clientgrafik, um das Blatt **Punkt-zu-Standort-VPN-Verbindung** zu öffnen.

    ![Clients](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. Klicken Sie auf dem Blatt **Punkt-zu-Standort-Verbindung** auf **Zertifikate verwalten**, um das Blatt **Zertifikate** zu öffnen.<br>

    ![Blatt „Zertifikate“](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. Klicken Sie auf dem Blatt **Zertifikate** auf **Hochladen**, um das Blatt **Zertifikat hochladen** zu öffnen.<br>

    ![Blatt zum Hochladen von Zertifikaten](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png)<br>
4. Klicken Sie auf die Ordnergrafik, um zur CER-Datei zu navigieren. Wählen Sie die Datei aus, und klicken Sie anschließend auf **OK**. Aktualisieren Sie die Seite, damit das hochgeladene Zertifikat auf dem Blatt **Zertifikate** angezeigt wird.

    ![Hochladen des Zertifikats](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png)<br>

## <a name="vpnclientconfig"></a>Abschnitt 4: Generieren des VPN-Clientkonfigurationspakets
Zum Herstellen einer Verbindung mit dem virtuellen Netzwerk müssen Sie auch einen VPN-Client konfigurieren. Der Clientcomputer benötigt sowohl ein Clientzertifikat als auch das richtige VPN-Clientkonfigurationspaket, um eine Verbindung herstellen zu können.

Das VPN-Clientpaket enthält Konfigurationsinformationen zum Konfigurieren der VPN-Client-Software, die in Windows integriert. Das Paket installiert keine zusätzlichen Software. Die Einstellungen gelten speziell für das virtuelle Netzwerk, mit dem Sie eine Verbindung herstellen möchten. Die Liste mit den unterstützten Clientbetriebssystemen finden Sie unter [Point-to-Site – Häufig gestellte Fragen](#faq) am Ende dieses Artikels.

### <a name="to-generate-the-vpn-client-configuration-package"></a>So generieren Sie das VPN-Clientkonfigurationspaket
1. Klicken Sie im Azure-Portal auf dem Übersichtsblatt****für Ihr VNet im Abschnitt **VPN-Verbindungen** auf die Clientgrafik, um das Blatt **Punkt-zu-Standort-VPN-Verbindung** zu öffnen.
2. Wählen Sie im oberen Bereich des Blatts **Punkt-zu-Standort-VPN-Verbindung** das Downloadpaket für das Clientbetriebssystem aus, unter dem es installiert wird:

   * Wählen Sie für 64-Bit-Clients das Paket **VPN-Client (64 Bit)** aus.
   * Wählen Sie für 32-Bit-Clients das Paket **VPN-Client (32 Bit)** aus.

     ![Herunterladen des VPN-Clientkonfigurationspakets](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png)<br>
3. Daraufhin erscheint eine Meldung mit dem Hinweis, dass Azure das VPN-Clientkonfigurationspaket für das virtuelle Netzwerk generiert. Wenn das Paket nach einigen Minuten generiert wurde, erscheint auf Ihrem lokalen Computer eine Meldung mit dem Hinweis, dass das Paket heruntergeladen wurde. Speichern Sie die Konfigurationspaketdatei. Diese muss auf jedem Clientcomputer installiert werden, der eine P2S-Verbindung mit dem virtuellen Netzwerk herstellt.

## <a name="clientconfiguration"></a>Abschnitt 5: Konfigurieren des Clientcomputers
### <a name="part-1-install-an-exported-client-certificate"></a>Teil 1: Installieren eines exportierten Clientzertifikats

Wenn Sie eine P2S-Verbindung mit einem anderen Clientcomputer als dem für die Generierung der Clientzertifikate verwendeten Computer herstellen möchten, müssen Sie ein Clientzertifikat installieren. Beim Installieren eines Clientzertifikats benötigen Sie das Kennwort, das beim Exportieren des Clientzertifikats erstellt wurde.

1. Suchen Sie die *PFX* -Datei, und kopieren Sie sie auf den Clientcomputer. Doppelklicken Sie auf dem Clientcomputer auf die *PFX* -Datei, um sie zu installieren. Lassen Sie den **Speicherort** auf **Aktueller Benutzer** eingestellt, und klicken Sie dann auf **Weiter**.
2. Nehmen Sie auf der Seite **Zu importierende Datei** keine Änderungen vor. Klicken Sie auf **Weiter**.
3. Geben Sie auf der Seite **Schutz für den privaten Schlüssel** das Kennwort für das Zertifikat ein (sofern Sie ein Kennwort festgelegt haben), oder überprüfen Sie die Richtigkeit des Sicherheitsprinzipals, der das Zertifikat installiert, und klicken Sie dann auf **Weiter**.
4. Behalten Sie auf der Seite **Zertifikatspeicher** den Standardspeicherort bei, und klicken Sie dann auf **Weiter**.
5. Klicken Sie auf **Fertig stellen**. Klicken Sie in der **Sicherheitswarnung** für die Zertifikatinstallation auf **Ja**. Sie können beruhigt auf „Ja“ klicken, da Sie das Zertifikat generiert haben. Das Zertifikat wird nun erfolgreich importiert.

### <a name="part-2-install-the-vpn-client-configuration-package"></a>Teil 2: Installieren des VPN-Clientkonfigurationspakets
Sie können auf jedem Clientcomputer das gleiche VPN-Clientkonfigurationspaket verwenden – vorausgesetzt, es handelt sich dabei um die passende Version für die Architektur des jeweiligen Clients.

1. Kopieren Sie die Konfigurationsdatei lokal auf den Computer, für den Sie eine Verbindung mit dem virtuellen Netzwerk herstellen möchten. 
2. Doppelklicken Sie auf die EXE-Datei, um das Paket auf dem Clientcomputer zu installieren. Das Konfigurationspaket ist nicht signiert, da Sie es erstellt haben. Daher wird unter Umständen eine Warnung angezeigt. Klicken Sie im ggf. angezeigten Windows SmartScreen-Popup auf **Weitere Informationen** (links) und dann auf **Trotzdem ausführen**, um das Paket zu installieren.
3. Navigieren Sie auf dem Clientcomputer zu **Netzwerkeinstellungen**, und klicken Sie auf **VPN**. Die Verbindung wird angezeigt. Sie zeigt den Namen des virtuellen Netzwerks an, mit dem eine Verbindung hergestellt wird. Das sieht dann in etwa so aus:

    ![VPN-Client](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png)

## <a name="connect"></a>Abschnitt 6: Herstellen der Verbindung mit Azure
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

## <a name="add"></a>Hinzufügen oder Entfernen vertrauenswürdiger Stammzertifikate

Sie können vertrauenswürdige Stammzertifikate hinzufügen und aus Azure entfernen. Wenn Sie ein vertrauenswürdiges Zertifikat entfernen, kann für die Clientzertifikate, die mit dem Stammzertifikat generiert wurden, per Punkt-zu-Standort-Verfahren keine Verbindung mehr mit Azure hergestellt werden. Wenn Sie Clients verbinden möchten, muss dafür jeweils ein neues Clientzertifikat installiert werden, das mit einem für Azure vertrauenswürdigen Zertifikat generiert wird.

### <a name="to-add-a-trusted-root-certificate"></a>So fügen Sie ein vertrauenswürdiges Stammzertifikat hinzu

Sie können Azure bis zu 20 vertrauenswürdige CER-Stammzertifikatdateien hinzufügen. Eine entsprechende Anleitung finden Sie in [Abschnitt 3: Hochladen der CER-Datei des Stammzertifikats](#upload).

### <a name="to-remove-a-trusted-root-certificate"></a>Entfernen eines vertrauenswürdigen Stammzertifikats


1. Klicken Sie auf dem Blatt für Ihr VNet im Abschnitt **VPN-Verbindungen** auf die****Clientgrafik, um das Blatt **Punkt-zu-Standort-VPN-Verbindung** zu öffnen.

    ![Clients](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. Klicken Sie auf dem Blatt **Punkt-zu-Standort-Verbindung** auf **Zertifikate verwalten**, um das Blatt **Zertifikate** zu öffnen.<br>

    ![Blatt „Zertifikate“](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. Klicken Sie auf dem Blatt **Zertifikate** neben dem Zertifikat, das Sie entfernen möchten, auf die Auslassungspunkte, und klicken Sie anschließend auf **Löschen**.

     ![Löschen eines Stammzertifikats](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deleteroot.png)<br>


## <a name="revokeclient"></a>Sperren eines Clientzertifikats
Sie können Clientzertifikate sperren. Anhand der Zertifikatsperrliste können Sie basierend auf einzelnen Clientzertifikaten selektiv Punkt-zu-Standort-Verbindungen verweigern. Das ist nicht dasselbe wie das Entfernen eines vertrauenswürdigen Stammzertifikats. Wenn Sie ein vertrauenswürdiges Stammzertifikat (CER-Datei) aus Azure entfernen, wird der Zugriff für alle Clientzertifikate gesperrt, die mit dem gesperrten Stammzertifikat generiert oder signiert wurden. Wenn Sie anstelle des Stammzertifikats ein Clientzertifikat sperren, können die anderen Zertifikate, die auf der Grundlage des Stammzertifikats generiert wurden, weiterhin zur Authentifizierung für die P2S-Verbindung verwendet werden.

Üblicherweise wird das Stammzertifikat zum Verwalten des Zugriffs auf Team- oder Organisationsebene verwendet. Eine genauer abgestufte Steuerung des Zugriffs für einzelne Benutzer erfolgt hingegen mit gesperrten Clientzertifikaten.

### <a name="to-revoke-a-client-certificate"></a>So sperren Sie ein Clientzertifikat

Sie können ein Clientzertifikat sperren, indem Sie den Fingerabdruck der Sperrliste hinzufügen.

1. Rufen Sie den Fingerabdruck des Clientzertifikats ab. Weitere Informationen finden Sie unter [Vorgehensweise: Abrufen des Fingerabdrucks eines Zertifikats](https://msdn.microsoft.com/library/ms734695.aspx).
2. Kopieren Sie ihn in einen Text-Editor, und entfernen Sie alle Leerzeichen, sodass eine fortlaufende Zeichenfolge entsteht.
3. Navigieren Sie zu **„Name des klassischen virtuellen Netzwerks“ > Punkt-zu-Standort-VPN-Verbindung > Zertifikate**, und klicken Sie anschließend auf **Sperrliste**, um das Blatt mit der Sperrliste zu öffnen. 
4. Klicken Sie auf dem Blatt **Sperrliste** auf **+Zertifikat hinzufügen**, um das Blatt **Zertifikat der Sperrliste hinzufügen** zu öffnen.
5. Fügen Sie auf dem Blatt **Zertifikat der Sperrliste hinzufügen** den Zertifikatfingerabdruck als durchgehende Textzeile (ohne Leerzeichen) ein. Klicken Sie unten auf dem Blatt auf **OK**.
6. Nach Abschluss der Aktualisierung kann das Zertifikat nicht mehr für die Verbindungsherstellung verwendet werden. Clients, die versuchen, unter Verwendung dieses Zertifikats eine Verbindung herzustellen, erhalten eine Meldung mit dem Hinweis, dass das Zertifikat nicht mehr gültig ist.


## <a name="faq"></a>Point-to-Site – Häufig gestellte Fragen

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Nächste Schritte
Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter [Virtuelle Computer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) . Weitere Informationen zu Netzwerken und virtuellen Computern finden Sie unter [Azure- und Linux-VM-Netzwerke (Übersicht)](../virtual-machines/virtual-machines-linux-azure-vm-network-overview.md).

