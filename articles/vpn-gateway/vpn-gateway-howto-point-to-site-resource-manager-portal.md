---
title: 'Herstellen einer Point-to-Site-Verbindung zwischen einem Computer und einem virtuellen Azure-Netzwerk: Portal | Microsoft-Dokumentation'
description: Stellen Sie unter Verwendung von Resource Manager und Azure-Portal eine sichere P2S-VPN-Gatewayverbindung mit Ihrem Azure Virtual Network her.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a15ad327-e236-461f-a18e-6dbedbf74943
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 6703df0f64534ed638e570342eef7fbda2a74d2e
ms.lasthandoff: 04/03/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Konfigurieren einer Point-to-Site-Verbindung mit einem VNet über das Azure-Portal
> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klassisch – Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Mit einer P2S-Konfiguration (Point-to-Site) können Sie von einem einzelnen Clientcomputer eine sichere Verbindung mit einem virtuellen Netzwerk herstellen. P2S ist eine VPN-Verbindung über SSTP (Secure Socket Tunneling-Protokoll). Point-to-Site-Verbindungen sind nützlich, wenn Sie von einem Remotestandort, z.B. von zu Hause oder bei einer Konferenz, eine Verbindung mit Ihrem VNet herstellen möchten. Diese Methode eignet sich auch, wenn Sie nur wenige Clients besitzen, die mit einem virtuellen Netzwerk verbunden werden müssen. Für P2S-Verbindungen ist kein VPN-Gerät und auch keine öffentliche IP-Adresse erforderlich. Sie stellen die VPN-Verbindung über den Clientcomputer her.

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie über das Azure-Portal ein VNet mit einer P2S-Verbindung (Point-to-Site) erstellen. Weitere Informationen zu Point-to-Site-Verbindungen finden Sie unter [Point-to-Site – Häufig gestellte Fragen](#faq) am Ende dieses Artikels.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Bereitstellungsmodelle und -methoden für P2S-Verbindungen
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Die folgende Tabelle zeigt die beiden Bereitstellungsmodelle und die verfügbaren Bereitstellungsmethoden für P2S-Konfigurationen. Falls ein Artikel mit Konfigurationsschritten verfügbar ist, steht in der Tabelle ein direkter Link zur Verfügung.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Grundlegender Workflow
![P2S-Diagramm](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

### <a name="example"></a>Beispielwerte
* **Name: VNet1**
* **Adressraum: 192.168.0.0/16**<br>In diesem Beispiel verwenden wir nur einen einzelnen Adressraum. Sie können für Ihr VNet aber auch mehrere Adressräume verwenden.
* **Subnetzname: FrontEnd**
* **Subnetzadressbereich: 192.168.1.0/24**
* **Abonnement:** Falls Sie über mehrere Abonnements verfügen, vergewissern Sie sich, dass Sie das richtige Abonnement verwenden.
* **Ressourcengruppe: TestRG**
* **Standort: USA, Osten**
* **GatewaySubnet: 192.168.200.0/24**
* **Name des virtuellen Netzwerkgateways: VNet1GW**
* **Gatewaytyp: VPN**
* **VPN-Typ: Routenbasiert**
* **Öffentliche IP-Adresse: VNet1GWpip**
* **Verbindungstyp: Point-to-Site**
* **Clientadresspool: 172.16.201.0/24**<br>VPN-Clients, die über diese Point-to-Site-Verbindung eine Verbindung mit dem VNet herstellen, erhalten eine IP-Adresse aus dem Clientadresspool.

## <a name="createvnet"></a>Teil 1: Erstellen eines virtuellen Netzwerks
Stellen Sie zunächst sicher, dass Sie über ein Azure-Abonnement verfügen. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial) registrieren. Falls Sie diese Konfiguration zu Übungszwecken erstellen, können Sie die [Beispielwerte](#example) verwenden.

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="address"></a>Teil 2: Angeben von Adressräumen und Subnetzen
Sie können dem VNet nach dem Erstellen weitere Adressräume und Subnetze hinzufügen.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="gatewaysubnet"></a>Teil 3: Hinzufügen eines Gatewaysubnetzes

Bevor Sie das virtuelle Netzwerk mit einem Gateway verbinden, müssen Sie das Gatewaysubnetz für das virtuelle Netzwerk erstellen, mit dem Sie eine Verbindung herstellen möchten. Für die Gatewaydienste werden die im Gatewaysubnetz angegebenen IP-Adressen verwendet. Erstellen Sie nach Möglichkeit ein Gatewaysubnetz mit einem CIDR-Block vom Typ „/28“ oder „/27“, damit genügend IP-Adressen für zukünftige zusätzliche Konfigurationsanforderungen zur Verfügung stehen.

Die Screenshots in diesem Abschnitt werden als Referenzbeispiel bereitgestellt. Verwenden Sie den GatewaySubnet-Adressbereich mit den Werten für Ihre Konfiguration.

###<a name="to-create-a-gateway-subnet"></a>So erstellen Sie ein Gatewaysubnetz

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="dns"></a>Teil 4: Angeben eines DNS-Servers (optional)

DNS ist für eine Point-to-Site-Verbindung nicht erforderlich. Wenn Sie für Ressourcen, die in Ihren virtuellen Netzwerken bereitgestellt werden, die Namensauflösung verwenden möchten, müssen Sie aber einen DNS-Server angeben. Diese Einstellung bietet die Möglichkeit, den DNS-Server anzugeben, den Sie zur Namensauflösung für dieses virtuelle Netzwerk verwenden möchten. Mit dieser Einstellung wird kein DNS-Server erstellt.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>Teil 5: Erstellen eines Gateways für das virtuelle Netzwerk
Für Point-to-Site-Verbindungen werden folgende Einstellungen benötigt:

* Gatewaytyp: VPN
* VPN-Typ: Routenbasiert

### <a name="to-create-a-virtual-network-gateway"></a>So erstellen Sie ein Gateway für das virtuelle Netzwerk
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>Teil 6: Generieren von Zertifikaten

Zertifikate werden von Azure zur Authentifizierung von VPN-Clients für Point-to-Site-VPNs verwendet. Nach dem Erstellen des Stammzertifikats exportieren Sie die Daten des öffentlichen Zertifikats (nicht den privaten Schlüssel) als Base64-codierte X.509-CER-Datei. Anschließend laden Sie die Daten des öffentlichen Zertifikats aus dem Stammzertifikat nach Azure hoch.

Auf jedem Clientcomputer, der per Point-to-Site eine Verbindung mit einem VNet herstellt, muss ein Clientzertifikat installiert sein. Das Clientzertifikat wird über das Stammzertifikat generiert und auf jedem Clientcomputer installiert. Wenn kein gültiges Clientzertifikat installiert ist und der Client versucht, eine Verbindung mit dem VNet herzustellen, tritt bei der Authentifizierung ein Fehler auf.

### <a name="getcer"></a>Schritt 1: Beschaffen der CER-Datei für das Stammzertifikat

####<a name="enterprise-certificate"></a>Unternehmenszertifikat
 
Bei einer Unternehmenslösung können Sie Ihre vorhandene Zertifikatkette verwenden. Rufen Sie die CER-Datei für das Stammzertifikat ab, das Sie verwenden möchten.

####<a name="self-signed-root-certificate"></a>Selbstsigniertes Stammzertifikat

Wenn Sie keine Unternehmenszertifikatlösung verwenden, müssen Sie ein selbstsigniertes Stammzertifikat generieren. Sie können PowerShell verwenden, um ein selbstsigniertes Stammzertifikat zu erstellen, das die erforderlichen Felder für die P2S-Authentifizierung enthält. Unter [Arbeiten mit selbstsignierten Stammzertifikaten für Punkt-zu-Standort-Verbindungen](vpn-gateway-certificates-point-to-site.md) werden die Schritte zum Erstellen eines selbstsignierten Stammzertifikats beschrieben.

> [!NOTE]
> Bisher war „makecert“ die empfohlene Methode, um selbstsignierte Zertifikate zu erstellen und Clientzertifikate für Point-to-Site-Verbindungen zu generieren. Jetzt können Sie diese Zertifikate mit PowerShell erstellen. Ein Vorteil bei der Verwendung von PowerShell ist, dass Sie SHA-2-Zertifikate erstellen können. Die erforderlichen Werte finden Sie unter [Arbeiten mit selbstsignierten Stammzertifikaten für Punkt-zu-Standort-Verbindungen](vpn-gateway-certificates-point-to-site.md).
>
>


#### <a name="to-export-the-public-key-for-a-self-signed-root-certificate"></a>So exportieren Sie den öffentlichen Schlüssel für ein selbstsigniertes Stammzertifikat

Für Point-to-Site-Verbindungen muss der öffentliche Schlüssel (CER-Format) nach Azure hochgeladen werden. Die folgenden Schritte dienen Ihnen als Hilfe beim Exportieren der CER-Datei für Ihr selbstsigniertes Stammzertifikat.

1. Um eine CER-Datei aus dem Zertifikat zu erhalten, öffnen Sie **certmgr.msc**. Suchen Sie das selbstsignierte Stammzertifikat (in der Regel in „Zertifikate – Aktueller Benutzer\Eigene Zertifikate\Zertifikate“), und klicken Sie mit der rechten Maustaste darauf. Klicken Sie auf **Alle Aufgaben** und anschließend auf **Exportieren**. Dadurch wird der **Zertifikatexport-Assistent**geöffnet.
2. Klicken Sie im Assistenten auf **Weiter**. Wählen Sie **Nein, privaten Schlüssel nicht exportieren** aus, und klicken Sie dann auf **Weiter**.
3. Wählen Sie auf der Seite **Dateiformat für den Export** die Option **Base-64-codiert X.509 (.CER)** aus, und klicken Sie dann auf **Weiter**. 
4. Wählen Sie unter **Zu exportierende Datei** die Option **Durchsuchen** aus, um zu dem Speicherort zu wechseln, an den das Zertifikat exportiert werden soll. Geben Sie unter **Dateiname**einen Namen für die Zertifikatdatei ein. Klicken Sie auf **Weiter**.
5. Klicken Sie auf **Fertig stellen** , um das Zertifikat zu exportieren. Es wird **Der Export war erfolgreich** angezeigt. Klicken Sie auf **OK**, um den Assistenten zu schließen.

### <a name="generateclientcert"></a>Schritt 2: Generieren eines Clientzertifikats
Sie können entweder ein eindeutiges Zertifikat für jeden Client generieren, mit dem eine Verbindung mit dem virtuellen Netzwerk hergestellt wird, oder Sie können dasselbe Zertifikat für mehrere Clients verwenden. Der Vorteil beim Generieren von eindeutigen Clientzertifikaten besteht darin, dass Sie bei Bedarf ein einzelnes Zertifikat widerrufen können. Falls überall das gleiche Clientzertifikat verwendet wird und Sie das Zertifikat für einen Client sperren müssen, müssen Sie sonst neue Zertifikate für alle Clients generieren und installieren, die das Zertifikat für die Authentifizierung verwenden.

####<a name="enterprise-certificate"></a>Unternehmenszertifikat
- Generieren Sie bei Verwendung einer Unternehmenszertifikatlösung ein Clientzertifikat mit dem gängigen Name-Wert-Format „name@yourdomain.com“ (anstatt des Formats „Domänenname\Benutzername“).
- Stellen Sie sicher, dass das von Ihnen ausgestellte Clientzertifikat auf der Zertifikatvorlage „User“ basiert, das als ersten Eintrag in der Nutzungsliste „Client Authentication“ enthält, anstatt „Smart Card Logon“ usw. Sie können das Zertifikat überprüfen, indem Sie auf das Clientzertifikat doppelklicken und **Details > Erweiterte Schlüsselverwendung** anzeigen.

####<a name="self-signed-root-certificate"></a>Selbstsigniertes Stammzertifikat 
Wenn Sie ein selbstsigniertes Stammzertifikat verwenden, helfen Ihnen die Schritte zum [Generieren eines Clientzertifikats mit PowerShell](vpn-gateway-certificates-point-to-site.md#clientcert) weiter. Hiermit können Sie ein Clientzertifikat generieren, das mit Point-to-Site-Verbindungen kompatibel ist.


### <a name="exportclientcert"></a>Schritt 3: Exportieren des Clientzertifikats
Wenn Sie ein Clientzertifikat über ein selbstsigniertes Stammzertifikat mithilfe der [PowerShell](vpn-gateway-certificates-point-to-site.md#clientcert)-Anleitung generieren, wird es automatisch auf dem Computer installiert, den Sie für die Generierung verwendet haben. Falls Sie ein Clientzertifikat auf einem anderen Clientcomputer installieren möchten, müssen Sie es exportieren.

1. Um ein Clientzertifikat zu exportieren, öffnen Sie **certmgr.msc**. Klicken Sie mit der rechten Maustaste auf das Clientzertifikat, das Sie exportieren möchten, klicken Sie auf **Alle Aufgaben** und anschließend auf **Exportieren**. Dadurch wird der **Zertifikatexport-Assistent**geöffnet.
2. Klicken Sie im Assistenten auf **Weiter**, wählen Sie **Ja, privaten Schlüssel exportieren** aus, und klicken Sie dann auf **Weiter**.
3. Übernehmen Sie auf der Seite **Format der zu exportierenden Datei** die Standardwerte. Stellen Sie sicher, dass die Option **Wenn möglich, alle Zertifikate im Zertifizierungspfad einbeziehen** aktiviert ist. Klicken Sie auf **Weiter**. 
4. Auf der Seite **Sicherheit** müssen Sie den privaten Schlüssel schützen. Wenn Sie ein Kennwort verwenden möchten, müssen Sie sich das für dieses Zertifikat festgelegte Kennwort unbedingt merken oder notieren. Klicken Sie auf **Weiter**.
5. Wählen Sie unter **Zu exportierende Datei** die Option **Durchsuchen** aus, um zu dem Speicherort zu wechseln, an den das Zertifikat exportiert werden soll. Geben Sie unter **Dateiname**einen Namen für die Zertifikatdatei ein. Klicken Sie auf **Weiter**.
6. Klicken Sie auf **Fertig stellen** , um das Zertifikat zu exportieren.   

## <a name="addresspool"></a>Teil 7: Hinzufügen des Clientadresspools
1. Navigieren Sie nach Erstellung des virtuellen Netzwerkgateways auf dem Blatt des virtuellen Netzwerkgateways zum Abschnitt **Einstellungen**. Klicken Sie im Abschnitt **Einstellungen** auf **Punkt-zu-Standort-Konfiguration**, um das Blatt **Konfiguration** zu öffnen.
   
    ![Point-to-Site-Blatt](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png)
2. **Adresspool** ist der IP-Adresspool, aus dem Clients, die eine Verbindung herstellen, eine IP-Adresse erhalten. Fügen Sie den Adresspool hinzu, und klicken Sie auf **Speichern**.
   
    ![Clientadresspool](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>Teil 8: Hochladen der CER-Datei des Stammzertifikats
Nach Erstellung des Gateways können Sie die CER-Datei für ein vertrauenswürdiges Stammzertifikat in Azure hochladen. Sie können Dateien für bis zu 20 Stammzertifikate hochladen. Der private Schlüssel für das Stammzertifikat wird nicht in Azure hochgeladen. Die hochgeladene CER-Datei wird von Azure zur Authentifizierung von Clients verwendet, die eine Verbindung mit dem virtuellen Netzwerk herstellen.

1. Die Zertifikate werden auf dem Blatt **Point-to-Site-Konfiguration** im Abschnitt **Stammzertifikat** hinzugefügt.  
2. Vergewissern Sie sich, dass Sie das Stammzertifikat als X.509-CER-Datei mit Base-64-Codierung exportiert haben. Das Zertifikat muss in diesem Format exportiert werden, um es mit einem Text-Editor öffnen zu können.
3. Öffnen Sie das Zertifikat mit einem Text-Editor (beispielsweise mit dem Windows-Editor). Kopieren Sie nur den folgenden Abschnitt als fortlaufende Zeile:
   
    ![Zertifikatdaten](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)

    > [!NOTE]
    > Stellen Sie beim Kopieren der Zertifikatsdaten sicher, dass Sie den Text als fortlaufende Zeile ohne Wagenrückläufe oder Zeilenvorschübe kopieren. Es kann erforderlich sein, Ihre Ansicht im Text-Editor so zu ändern, dass die Symbole bzw. alle Zeichen angezeigt werden, damit die Wagenrückläufe bzw. Zeilenvorschübe sichtbar sind.                                                                                                                                                                            
    >
    >

4. Fügen Sie die Zertifikatdaten im Feld **Daten des öffentlichen Zertifikats** ein. Geben Sie dem Zertifikat einen **Namen**, und klicken Sie dann auf **Speichern**. Sie können bis zu 20 vertrauenswürdige Stammzertifikate hinzufügen.
   
    ![Zertifikatupload](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="clientconfig"></a>Teil 9: Herunterladen und Installieren des Konfigurationspakets für VPN-Clients
Clients, die mithilfe von P2S eine Verbindung mit Azure herstellen, benötigen ein Clientzertifikat sowie ein installiertes Konfigurationspaket für VPN-Clients. Konfigurationspakete für VPN-Clients stehen für Windows-Clients zur Verfügung. 

Das VPN-Clientpaket enthält Informationen zum Konfigurieren der in Windows integrierten VPN-Clientsoftware. Die Konfiguration ist VPN-spezifisch. Das Paket installiert keine zusätzlichen Software.

Sie können auf jedem Clientcomputer das gleiche VPN-Clientkonfigurationspaket verwenden – vorausgesetzt, es handelt sich dabei um die passende Version für die Architektur des jeweiligen Clients.

### <a name="step-1---download-the-client-configuration-package"></a>Schritt 1: Herunterladen des Clientkonfigurationspakets

1. Klicken Sie auf dem Blatt **Punkt-zu-Standort-Konfiguration** auf **Download VPN client** (VPN-Client herunterladen), um das Blatt **Download VPN client** (VPN-Client herunterladen) zu öffnen. Die Paketgenerierung kann etwas dauern.
   
    ![Herunterladen des VPN-Clients 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. Wählen Sie das passende Paket für Ihren Client aus, und klicken Sie auf **Herunterladen**. Speichern Sie die Konfigurationspaketdatei. Diese muss auf jedem Clientcomputer installiert werden, der eine Verbindung mit dem virtuellen Netzwerk herstellt.

    ![Herunterladen des VPN-Clients 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpnclient.png)

### <a name="step-2---install-the-client-configuration-package"></a>Schritt 2: Installieren des Clientkonfigurationspakets

1. Kopieren Sie die Konfigurationsdatei lokal auf den Computer, für den Sie eine Verbindung mit dem virtuellen Netzwerk herstellen möchten. 
2. Doppelklicken Sie auf die EXE-Datei, um das Paket auf dem Clientcomputer zu installieren. Da Sie das Konfigurationspaket erstellt haben, ist es nicht signiert. Daher wird unter Umständen eine Warnung angezeigt. Klicken Sie im ggf. angezeigten Windows SmartScreen-Popup auf **Weitere Informationen** (links) und dann auf **Trotzdem ausführen**, um das Paket zu installieren.
3. Navigieren Sie auf dem Clientcomputer zu **Netzwerkeinstellungen**, und klicken Sie auf **VPN**. Die Verbindung wird angezeigt. Sie zeigt den Namen des virtuellen Netzwerks an, mit dem eine Verbindung hergestellt wird. Das sieht dann in etwa so aus:
3. Installieren Sie das Paket auf dem Clientcomputer. Klicken Sie im ggf. angezeigten Windows SmartScreen-Popup auf **Weitere Informationen** (links) und dann auf **Trotzdem ausführen**, um das Paket zu installieren.
4. Navigieren Sie auf dem Clientcomputer zu **Netzwerkeinstellungen**, und klicken Sie auf **VPN**. Die Verbindung wird angezeigt. Sie zeigt den Namen des virtuellen Netzwerks an, mit dem eine Verbindung hergestellt wird. Dies sieht in etwa wie in diesem Beispiel aus: 
   
    ![VPN-Client](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png)


## <a name="installclientcert"></a>Teil 10: Installieren eines exportierten Clientzertifikats

Wenn Sie eine P2S-Verbindung mit einem anderen Clientcomputer als dem für die Generierung der Clientzertifikate verwendeten Computer herstellen möchten, müssen Sie ein Clientzertifikat installieren. Beim Installieren eines Clientzertifikats benötigen Sie das Kennwort, das beim Exportieren des Clientzertifikats erstellt wurde. 

1. Suchen Sie die *PFX* -Datei, und kopieren Sie sie auf den Clientcomputer. Doppelklicken Sie auf dem Clientcomputer auf die *PFX* -Datei, um sie zu installieren. Lassen Sie den **Speicherort** auf **Aktueller Benutzer** eingestellt, und klicken Sie dann auf **Weiter**.
2. Nehmen Sie auf der Seite **Zu importierende Datei** keine Änderungen vor. Klicken Sie auf **Weiter**.
3. Geben Sie auf der Seite **Schutz für den privaten Schlüssel** das Kennwort für das Zertifikat ein (sofern Sie ein Kennwort festgelegt haben), oder überprüfen Sie die Richtigkeit des Sicherheitsprinzipals, der das Zertifikat installiert, und klicken Sie dann auf **Weiter**.
4. Behalten Sie auf der Seite **Zertifikatspeicher** den Standardspeicherort bei, und klicken Sie dann auf **Weiter**.
5. Klicken Sie auf **Fertig stellen**. Klicken Sie in der **Sicherheitswarnung** für die Zertifikatinstallation auf **Ja**. Sie können beruhigt auf „Ja“ klicken, da Sie das Zertifikat generiert haben. Das Zertifikat wird nun erfolgreich importiert.

## <a name="connect"></a>Teil 11: Herstellen einer Verbindung mit Azure
1. Um eine Verbindung mit Ihrem VNet herzustellen, navigieren Sie auf dem Clientcomputer zu „VPN-Verbindungen“ und suchen nach der VPN-Verbindung, die Sie erstellt haben. Sie hat den gleichen Namen wie das virtuelle Netzwerk. Klicken Sie auf **Verbinden**. Möglicherweise wird eine Popupmeldung angezeigt, die sich auf die Verwendung des Zertifikats bezieht. Klicken Sie in diesem Fall auf **Weiter** , um erhöhte Rechte zu verwenden. 
2. Klicken Sie auf der Statusseite **Verbindung** auf **Verbinden**, um die Verbindung herzustellen. Wenn der Bildschirm **Zertifikat auswählen** angezeigt wird, vergewissern Sie sich, dass das angezeigte Clientzertifikat dem Zertifikat entspricht, die Sie zum Herstellen der Verbindung verwenden möchten. Wenn dies nicht der Fall ist, verwenden Sie den Dropdownpfeil, um das richtige Zertifikat auszuwählen, und klicken Sie dann auf **OK**.
   
    ![Herstellen der Verbindung vom VPN-Client mit Azure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)

    
3. Die Verbindung sollte jetzt eingerichtet werden.
   
    ![Mit Azure verbundener VPN-Client](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)
                                                                                                                                                                           

> [!NOTE]
> Wenn Sie bei Verwendung eines Zertifikats, das mit einer Lösung einer Unternehmenszertifizierungsstelle ausgestellt wurde, Probleme mit der Authentifizierung haben, ist es ratsam, die Authentifizierungsreihenfolge des Clientzertifikats zu überprüfen. Sie können die Reihenfolge der Authentifizierungsliste überprüfen, indem Sie auf das Clientzertifikat doppelklicken und zu **Details > Erweiterte Schlüsselverwendung** navigieren. Achten Sie darauf, dass in der Liste „Client Authentication“ als erster Eintrag aufgeführt ist. Wenn nicht, müssen Sie ein Clientzertifikat basierend auf der User-Vorlage ausstellen, die als ersten Eintrag in der Liste „Client Authentication“ enthält. 
>
>

## <a name="verify"></a>Teil 12: Überprüfen der Verbindung
1. Um sicherzustellen, dass die VPN-Verbindung aktiv ist, öffnen Sie eine Eingabeaufforderung mit Administratorrechten, und führen Sie *Ipconfig/all*aus.
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

## <a name="add"></a>Hinzufügen oder Entfernen vertrauenswürdiger Stammzertifikate
Sie können vertrauenswürdige Stammzertifikate hinzufügen und aus Azure entfernen. Wenn Sie ein vertrauenswürdiges Zertifikat entfernen, kann für die Clientzertifikate, die mit dem Stammzertifikat generiert wurden, per Punkt-zu-Standort-Verfahren keine Verbindung mehr mit Azure hergestellt werden. Wenn Sie Clients verbinden möchten, muss dafür jeweils ein neues Clientzertifikat installiert werden, das mit einem für Azure vertrauenswürdigen Zertifikat generiert wird.

### <a name="to-add-a-trusted-root-certificate"></a>So fügen Sie ein vertrauenswürdiges Stammzertifikat hinzu

Sie können Azure bis zu 20 vertrauenswürdige CER-Stammzertifikatdateien hinzufügen. Eine entsprechende Anleitung finden Sie in diesem Artikel im Abschnitt zum [Hochladen eines vertrauenswürdigen Stammzertifikats](#uploadfile).

### <a name="to-remove-a-trusted-root-certificate"></a>Entfernen eines vertrauenswürdigen Stammzertifikats

1. Navigieren Sie zum Entfernen eines vertrauenswürdigen Zertifikats zum Blatt **Punkt-zu-Standort-Konfiguration** für Ihr virtuelles Netzwerkgateway.
2. Suchen Sie auf dem Blatt im Abschnitt **Stammzertifikat** nach dem Zertifikat, das Sie entfernen möchten.
3. Klicken Sie neben dem Zertifikat auf die Auslassungspunkte und anschließend auf „Entfernen“.

## <a name="revokeclient"></a>Sperren eines Clientzertifikats
Sie können Clientzertifikate sperren. Anhand der Zertifikatsperrliste können Sie basierend auf einzelnen Clientzertifikaten selektiv Punkt-zu-Standort-Verbindungen verweigern. Das ist nicht dasselbe wie das Entfernen eines vertrauenswürdigen Stammzertifikats. Wenn Sie ein vertrauenswürdiges Stammzertifikat (CER-Datei) aus Azure entfernen, wird der Zugriff für alle Clientzertifikate gesperrt, die mit dem gesperrten Stammzertifikat generiert oder signiert wurden. Wenn Sie anstelle des Stammzertifikats ein Clientzertifikat sperren, können die anderen Zertifikate, die auf der Grundlage des Stammzertifikats generiert wurden, weiterhin zur Authentifizierung für die P2S-Verbindung verwendet werden.

Üblicherweise wird das Stammzertifikat zum Verwalten des Zugriffs auf Team- oder Organisationsebene verwendet. Eine genauer abgestufte Steuerung des Zugriffs für einzelne Benutzer erfolgt hingegen mit gesperrten Clientzertifikaten.

### <a name="to-revoke-a-client-certificate"></a>So sperren Sie ein Clientzertifikat

Sie können ein Clientzertifikat sperren, indem Sie den Fingerabdruck der Sperrliste hinzufügen.

1. Rufen Sie den Fingerabdruck des Clientzertifikats ab. Weitere Informationen finden Sie unter [Vorgehensweise: Abrufen des Fingerabdrucks eines Zertifikats](https://msdn.microsoft.com/library/ms734695.aspx).
2. Kopieren Sie ihn in einen Text-Editor, und entfernen Sie alle Leerzeichen, sodass eine fortlaufende Zeichenfolge entsteht.
3. Navigieren Sie zum Blatt **Punkt-zu-Standort-Konfiguration** des virtuellen Netzwerkgateways. Dieses Blatt haben Sie zum [Hochladen eines vertrauenswürdigen Stammzertifikats](#uploadfile) verwendet.
4. Geben Sie im Abschnitt **Gesperrte Zertifikate** einen Anzeigenamen für das Zertifikat ein. (Dabei muss es sich nicht um den allgemeinen Namen des Zertifikats handeln.)
5. Kopieren Sie die Fingerabdruck-Zeichenfolge, und fügen Sie sie in das Feld **Fingerabdruck** ein.
6. Der Fingerabdruck wird überprüft und automatisch der Sperrliste hinzugefügt. Auf dem Bildschirm erscheint eine Meldung mit dem Hinweis, dass die Liste aktualisiert wird. 
7. Nach Abschluss der Aktualisierung kann das Zertifikat nicht mehr für die Verbindungsherstellung verwendet werden. Clients, die versuchen, unter Verwendung dieses Zertifikats eine Verbindung herzustellen, erhalten eine Meldung mit dem Hinweis, dass das Zertifikat nicht mehr gültig ist.

## <a name="faq"></a>Point-to-Site – Häufig gestellte Fragen

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Nächste Schritte
Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter [Virtuelle Computer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) . Weitere Informationen zu Netzwerken und virtuellen Computern finden Sie unter [Azure- und Linux-VM-Netzwerke (Übersicht)](../virtual-machines/linux/azure-vm-network-overview.md).



