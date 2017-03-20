---
title: 'Herstellen einer Point-to-Site-Verbindung zwischen einem Computer und einem virtuellen Azure-Netzwerk: klassisches Portal | Microsoft-Dokumentation'
description: "Stellen Sie über das klassische Azure-Portal eine sichere P2S-VPN Gateway-Verbindung mit Ihrem klassischen Azure Virtual Network her."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 4f5668a5-9b3d-4d60-88bb-5d16524068e0
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: ffd5690853a7344c7122940ad9676d1903ba2d4a
ms.lasthandoff: 03/09/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-classic-portal-classic"></a>Konfigurieren einer Point-to-Site-Verbindung mit einem VNet über das klassische Azure-Portal (klassisch)
> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Klassisch – Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> * [Klassisch – Klassisches Portal](vpn-gateway-point-to-site-create.md)
> 
> 

Mit einer P2S-Konfiguration (Point-to-Site) können Sie von einem einzelnen Clientcomputer eine sichere Verbindung mit einem virtuellen Netzwerk herstellen. P2S ist eine VPN-Verbindung über SSTP (Secure Socket Tunneling-Protokoll). Point-to-Site-Verbindungen sind nützlich, wenn Sie von einem Remotestandort, z.B. von zu Hause oder in einer Konferenz, eine Verbindung mit Ihrem VNet herstellen möchten. Diese Methode eignet sich auch, wenn Sie nur wenige Clients besitzen, die mit einem virtuellen Netzwerk verbunden werden müssen. Für P2S-Verbindungen ist kein VPN-Gerät und keine öffentliche IP-Adresse erforderlich. Sie stellen die VPN-Verbindung über den Clientcomputer her.

In diesem Artikel erfahren Sie Schritt für Schritt, wie Sie im Rahmen des klassischen Bereitstellungsmodells ein VNet mit einer Point-to-Site-Verbindung (P2S) über das klassische Portal erstellen. Weitere Informationen zu Point-to-Site-Verbindungen finden Sie unter [Point-to-Site – Häufig gestellte Fragen](#faq) am Ende dieses Artikels.



### <a name="deployment-models-and-methods-for-p2s-connections"></a>Bereitstellungsmodelle und -methoden für P2S-Verbindungen
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Die folgende Tabelle zeigt die beiden Bereitstellungsmodelle und die verfügbaren Bereitstellungsmethoden für P2S-Konfigurationen. Falls ein Artikel mit Konfigurationsschritten verfügbar ist, steht in der Tabelle ein direkter Link zur Verfügung.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Grundlegender Workflow
![Point-to-Site-Diagramm](./media/vpn-gateway-point-to-site-create/p2sclassic.png "Point-to-Site")

Im Folgenden werden die Schritte zum Erstellen einer sicheren P2S-Verbindung mit einem virtuellen Netzwerk beschrieben. Die Konfiguration einer P2S-Verbindung ist in vier Abschnitte unterteilt. Dabei ist die Reihenfolge wichtig, in der Sie die einzelnen Abschnitte konfigurieren. Überspringen Sie keine Schritte, und halten Sie die Reihenfolge ein.

* **Abschnitt 1** : Erstellen eines virtuellen Netzwerks und eines VPN-Gateways
* **Abschnitt 2** : Erstellen und Hochladen der Zertifikate für die Authentifizierung
* **Abschnitt 3** : Exportieren und Installieren der Clientzertifikate
* **Abschnitt 4** : Konfigurieren des VPN-Clients



## <a name="vnetvpn"></a>Abschnitt 1: Erstellen eines virtuellen Netzwerks und eines VPN-Gateways

Stellen Sie zunächst sicher, dass Sie über ein Azure-Abonnement verfügen. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial) registrieren.

### <a name="part-1-create-a-virtual-network"></a>Teil 1: Erstellen eines virtuellen Netzwerks
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an. Bei diesen Schritten wird das klassische Portal und nicht das Azure-Portal verwendet. Derzeit ist es nicht möglich, eine P2S-Verbindung mit dem Azure-Portal zu erstellen.
2. Klicken Sie unten links auf dem Bildschirm auf **Neu**. Klicken Sie im Navigationsbereich auf **Network Services** und dann auf **Virtual Network**. Klicken Sie auf **Custom Create** , um den Konfigurationsassistenten zu starten.
3. Geben Sie auf dem Bildschirm **Details des virtuellen Netzwerks** die folgenden Informationen ein, und klicken Sie dann auf den Vorwärtspfeil unten rechts.
   
   * **Name**: Name des virtuellen Netzwerks. Beispiel: „VNet1“. Dies ist der Name, den Sie beim Bereitstellen von VMs für dieses VNet angeben.
   * **Speicherort**: Der Speicherort steht in direkter Beziehung zu dem physischen Standort (Region), an dem sich Ihre Ressourcen (VMs) befinden sollen. Wenn Sie z. B. möchten, dass sich die virtuellen Computer, die Sie für Ihr virtuelles Netzwerk bereitstellen, physisch in "USA West" befinden, wählen Sie diesen Speicherort aus. Sie können die Ihrem virtuellen Netzwerk zugeordnete Region nach dem Erstellen nicht mehr ändern.
4. Geben Sie auf der Seite **DNS Server und VPN-Konnektivität** die folgenden Informationen ein, und klicken Sie dann in der unteren rechten Ecke auf den Weiter-Pfeil.
   
   * **DNS-Server**: Geben Sie den Namen und die IP-Adresse des DNS-Servers ein, oder wählen Sie einen zuvor registrierten DNS-Server im Kontextmenü aus. Mit dieser Einstellung wird kein DNS-Server erstellt. Sie bietet die Möglichkeit, den DNS-Server anzugeben, den Sie zur Namensauflösung für dieses virtuelle Netzwerk verwenden möchten. Wenn Sie den Standarddienst für die Namensauflösung in Azure verwenden möchten, lassen Sie diesen Abschnitt leer.
   * **Standort-zu-Standort-VPNs konfigurieren**: Aktivieren Sie dieses Kontrollkästchen.
5. Geben Sie auf der Seite **Point-to-Site-Konnektivität** den IP-Adressbereich an, aus dem die VPN-Clients eine IP-Adresse erhalten, wenn eine Verbindung besteht. Es gibt einige Regeln bezüglich der Adressbereiche, die Sie angeben können. Sie müssen unbedingt sicherstellen, dass der angegebene Bereich keine anderen Bereiche überlappt, die sich in Ihrem lokalen Netzwerk befinden.
6. Geben Sie die folgenden Informationen ein, und klicken Sie dann auf den Pfeil "Weiter".
   
   * **Adressraum**: Umfasst Start-IP und CIDR (Adressenanzahl).
   * **Adressraum hinzufügen**: Fügen Sie einen Adressraum nur hinzu, wenn es für den Netzwerkentwurf erforderlich ist.
7. Geben Sie auf der Seite **Adressräume des virtuellen Netzwerks** die Adressräume an, die für das virtuelle Netzwerk verwendet werden sollen. Dies sind die dynamischen IP-Adressen (DIPS), die den virtuellen Computern und anderen Rolleninstanzen zugewiesen werden, die Sie für dieses virtuelle Netzwerk bereitstellen.<br><br>Es ist besonders wichtig, einen Bereich auszuwählen, der sich nicht mit den anderen Bereichen überschneidet, die für Ihr lokales Netzwerk verwendet werden. Sie müssen sich mit Ihrem Netzwerkadministrator abstimmen, der ggf. einen Bereich von IP-Adressen aus dem Adressraum Ihres lokalen Netzwerks reservieren muss, den Sie für Ihr virtuelles Netzwerk verwenden können.
8. Geben Sie die folgenden Informationen ein, und klicken Sie dann auf das Häkchen, um das virtuelle Netzwerk zu erstellen.
   
   * **Adressraum**: Fügen Sie den internen IP-Adressbereich hinzu, den Sie für dieses virtuelle Netzwerk verwenden möchten, einschließlich Start-IP und Anzahl. Es ist wichtig, einen Bereich auszuwählen, der sich nicht mit den anderen Bereichen überschneidet, die für Ihr lokales Netzwerk verwendet werden. 
   * **Subnetz hinzufügen**: Zusätzliche Subnetze sind nicht erforderlich, aber Sie können ein getrenntes Subnetz für virtuelle Computer erstellen, die über statische DIPs verfügen sollen. Vielleicht möchten Sie jedoch auch Ihre virtuellen Computer in einem Subnetz zusammenfassen, das von anderen Rolleninstanzen getrennt ist.
   * **Gatewaysubnetz hinzufügen**: Das Gatewaysubnetz ist für ein P2S-VPN erforderlich. Klicken Sie auf diese Option, um das Gatewaysubnetz hinzuzufügen. Das Gatewaysubnetz wird nur für das Gateway des virtuellen Netzwerks verwendet.
9. Nachdem das virtuelle Netzwerk erstellt wurde, wird im klassischen Azure-Portal auf der Seite mit den Netzwerken unter **Status** der Eintrag **Erstellt** angezeigt. Nachdem Ihr virtuelles Netzwerk erstellt wurde, können Sie das Gateway mit dynamischem Routing erstellen.

### <a name="part-2-create-a-dynamic-routing-gateway"></a>Teil 2: Erstellen eines Gateways mit dynamischem Routing
Der Gatewaytyp muss als dynamisch konfiguriert werden. Gateways mit statischem Routing sind mit diesem Feature nicht einsetzbar.

1. Klicken Sie im klassischen Azure-Portal auf der Seite **Netzwerke** auf das virtuelle Netzwerk, das Sie erstellt haben, und navigieren Sie zur Seite **Dashboard**.
2. Klicken Sie unten auf der Seite **Dashboard** auf **Gateway erstellen**. Es wird eine Meldung mit der folgenden Frage angezeigt: **Möchten Sie ein Gateway für das virtuelle Netzwerk „VNet1“ erstellen?**. Klicken Sie auf **Ja** , um mit der Erstellung des Gateways zu beginnen. Die Gatewayerstellung kann bis zu 45 Minuten dauern.

## <a name="generate"></a>Abschnitt 2: Generieren und Hochladen von Zertifikaten
Zertifikate werden von Azure zur Authentifizierung von VPN-Clients für Point-to-Site-VPNs verwendet. Nach dem Erstellen des Stammzertifikats exportieren Sie die Daten des öffentlichen Zertifikats (nicht den privaten Schlüssel) als Base64-codierte X.509-CER-Datei. Anschließend laden Sie die Daten des öffentlichen Zertifikats aus dem Stammzertifikat nach Azure hoch.

Auf jedem Clientcomputer, der per Point-to-Site eine Verbindung mit einem VNet herstellt, muss ein Clientzertifikat installiert sein. Das Clientzertifikat wird über das Stammzertifikat generiert und auf jedem Clientcomputer installiert. Wenn kein gültiges Clientzertifikat installiert ist und der Client versucht, eine Verbindung mit dem VNet herzustellen, tritt bei der Authentifizierung ein Fehler auf.

In diesem Abschnitt führen Sie Folgendes aus:

* Beschaffen der CER-Datei für ein Stammzertifikat: Dies kann entweder ein selbstsigniertes Stammzertifikat sein, oder Sie können Ihr Unternehmenszertifikatsystem verwenden.
* Hochladen der CER-Datei nach Azure
* Generieren von Clientzertifikaten

### <a name="root"></a>Teil 1: Beschaffen der CER-Datei für ein Stammzertifikat


Bei einer Unternehmenslösung können Sie Ihre vorhandene Zertifikatkette verwenden. Rufen Sie die CER-Datei für das Stammzertifikat ab, das Sie verwenden möchten.


Wenn Sie keine Unternehmenszertifikatlösung verwenden, müssen Sie ein selbstsigniertes Stammzertifikat generieren. Sie können PowerShell verwenden, um ein selbstsigniertes Stammzertifikat zu erstellen, das die erforderlichen Felder für die P2S-Authentifizierung enthält. Unter [Arbeiten mit selbstsignierten Stammzertifikaten für Punkt-zu-Standort-Verbindungen](vpn-gateway-certificates-point-to-site.md) werden die Schritte zum Erstellen eines selbstsignierten Stammzertifikats beschrieben.

> [!NOTE]
> Bisher war „makecert“ die empfohlene Methode, um selbstsignierte Zertifikate zu erstellen und Clientzertifikate für Point-to-Site-Verbindungen zu generieren. Jetzt können Sie PowerShell zum Erstellen dieser Zertifikate verwenden. Ein Vorteil bei der Verwendung von PowerShell ist, dass Sie SHA-2-Zertifikate erstellen können. Die erforderlichen Werte finden Sie unter [Arbeiten mit selbstsignierten Stammzertifikaten für Punkt-zu-Standort-Verbindungen](vpn-gateway-certificates-point-to-site.md).
>
>


#### <a name="to-export-the-public-key-for-a-self-signed-root-certificate"></a>So exportieren Sie den öffentlichen Schlüssel für ein selbstsigniertes Stammzertifikat

Für Point-to-Site-Verbindungen muss der öffentliche Schlüssel (CER-Format) nach Azure hochgeladen werden. Die folgenden Schritte dienen Ihnen als Hilfe beim Exportieren der CER-Datei für Ihr selbstsigniertes Stammzertifikat.

1. Um eine CER-Datei aus dem Zertifikat zu erhalten, öffnen Sie **certmgr.msc**. Suchen Sie das selbstsignierte Stammzertifikat (in der Regel in „Zertifikate – Aktueller Benutzer\Eigene Zertifikate\Zertifikate“), und klicken Sie mit der rechten Maustaste darauf. Klicken Sie auf **Alle Aufgaben** und anschließend auf **Exportieren**. Dadurch wird der **Zertifikatexport-Assistent**geöffnet.
2. Klicken Sie im Assistenten auf **Weiter**. Wählen Sie **Nein, privaten Schlüssel nicht exportieren** aus, und klicken Sie dann auf **Weiter**.
3. Wählen Sie auf der Seite **Dateiformat für den Export** die Option **Base-64-codiert X.509 (.CER)** aus, und klicken Sie dann auf **Weiter**. 
4. Wählen Sie unter **Zu exportierende Datei** die Option **Durchsuchen** aus, um zu dem Speicherort zu wechseln, an den das Zertifikat exportiert werden soll. Geben Sie unter **Dateiname**einen Namen für die Zertifikatdatei ein. Klicken Sie auf **Weiter**.
5. Klicken Sie auf **Fertig stellen** , um das Zertifikat zu exportieren. Es wird **Der Export war erfolgreich** angezeigt. Klicken Sie auf **OK**, um den Assistenten zu schließen.

### <a name="upload"></a>Teil 2: Hochladen der CER-Stammzertifikatdatei in das klassische Azure-Portal

Fügen Sie Azure ein vertrauenswürdiges Zertifikat hinzu. Wenn Sie Azure eine Base64-codierte x.509-Datei (CER-Datei) hinzufügen, weisen Sie Azure dadurch an, dem Stammzertifikat zu vertrauen, das die Datei darstellt. Sie können Dateien für bis zu 20 Stammzertifikate hochladen. Der private Schlüssel für das Stammzertifikat wird nicht in Azure hochgeladen. Die hochgeladene CER-Datei wird von Azure zur Authentifizierung von Clients verwendet, die eine Verbindung mit dem virtuellen Netzwerk herstellen.

1. Klicken Sie im klassischen Azure-Portal auf der Seite **Zertifikate** für Ihr virtuelles Netzwerk auf **Ein Stammzertifikat hochladen**.
2. Suchen Sie auf der Seite **Zertifikat hochladen** nach der CER-Datei für das Stammzertifikat, und klicken Sie dann auf das Häkchen.

### <a name="createclientcert"></a>Teil 3: Generieren eines Clientzertifikats
Sie können entweder ein eindeutiges Zertifikat für jeden Client generieren, mit dem eine Verbindung hergestellt wird, oder Sie können dasselbe Zertifikat für mehrere Clients verwenden. Der Vorteil beim Generieren von eindeutigen Clientzertifikaten besteht darin, dass Sie bei Bedarf ein einzelnes Zertifikat widerrufen können. Falls überall das gleiche Clientzertifikat verwendet wird und Sie das Zertifikat für einen Client sperren müssen, müssen Sie sonst neue Zertifikate für alle Clients generieren und installieren, die das Zertifikat für die Authentifizierung verwenden.

####<a name="enterprise-certificate"></a>Unternehmenszertifikat
- Generieren Sie bei Verwendung einer Unternehmenszertifikatlösung ein Clientzertifikat mit dem gängigen Name-Wert-Format „name@yourdomain.com“ (anstatt des Formats „Domänenname\Benutzername“).
- Stellen Sie sicher, dass das von Ihnen ausgestellte Clientzertifikat auf der Zertifikatvorlage „User“ basiert, das als ersten Eintrag in der Nutzungsliste „Client Authentication“ enthält, anstatt „Smart Card Logon“ usw. Sie können das Zertifikat überprüfen, indem Sie auf das Clientzertifikat doppelklicken und **Details > Erweiterte Schlüsselverwendung** anzeigen.

####<a name="self-signed-root-root-certificate"></a>Selbstsigniertes Stammzertifikat 
Wenn Sie ein selbstsigniertes Stammzertifikat verwenden, helfen Ihnen die Schritte zum [Generieren eines Clientzertifikats mit PowerShell](vpn-gateway-certificates-point-to-site.md#clientcert) weiter. Hiermit können Sie ein Clientzertifikat generieren, das mit Point-to-Site-Verbindungen kompatibel ist.


## <a name="installclientcert"></a>Abschnitt 3: Exportieren und Installieren des Clientzertifikats

Nach dem Generieren eines Clientzertifikats müssen Sie das Zertifikat als PFX-Datei exportieren und auf dem Clientcomputer installieren. Jeder Clientcomputer muss über ein Clientzertifikat verfügen, um sich zu authentifizieren. Sie können die Installation des Clientzertifikats automatisieren oder manuell durchführen. In den folgenden Schritten wird das manuelle Exportieren und Installieren des Clientzertifikats beschrieben.

### <a name="export-the-client-certificate"></a>Exportieren des Clientzertifikats

1. Um ein Clientzertifikat zu exportieren, öffnen Sie **certmgr.msc**. Klicken Sie mit der rechten Maustaste auf das Clientzertifikat, das Sie exportieren möchten, klicken Sie auf **Alle Aufgaben** und anschließend auf **Exportieren**. Dadurch wird der **Zertifikatexport-Assistent**geöffnet.
2. Klicken Sie im Assistenten auf **Weiter**, wählen Sie **Ja, privaten Schlüssel exportieren** aus, und klicken Sie dann auf **Weiter**.
3. Auf der Seite **Format der zu exportierenden Datei** können Sie die Standardwerte übernehmen. Klicken Sie auf **Weiter**. 
4. Auf der Seite **Sicherheit** müssen Sie den privaten Schlüssel schützen. Wenn Sie ein Kennwort verwenden möchten, müssen Sie sich das für dieses Zertifikat festgelegte Kennwort unbedingt merken oder notieren. Klicken Sie auf **Weiter**.
5. Wählen Sie unter **Zu exportierende Datei** die Option **Durchsuchen** aus, um zu dem Speicherort zu wechseln, an den das Zertifikat exportiert werden soll. Geben Sie unter **Dateiname**einen Namen für die Zertifikatdatei ein. Klicken Sie auf **Weiter**.
6. Klicken Sie auf **Fertig stellen** , um das Zertifikat zu exportieren.

### <a name="install-the-client-certificate"></a>Installieren des Clientzertifikats

Beim Installieren des Clientzertifikats benötigen Sie das Kennwort, das beim Exportieren des Clientzertifikats erstellt wurde.

1. Suchen Sie die *PFX* -Datei, und kopieren Sie sie auf den Clientcomputer. Doppelklicken Sie auf dem Clientcomputer auf die *PFX* -Datei, um sie zu installieren. Lassen Sie den **Speicherort** auf **Aktueller Benutzer** eingestellt, und klicken Sie dann auf **Weiter**.
2. Nehmen Sie auf der Seite **Zu importierende Datei** keine Änderungen vor. Klicken Sie auf **Weiter**.
3. Geben Sie auf der Seite **Schutz für den privaten Schlüssel** das Kennwort für das Zertifikat ein (sofern Sie ein Kennwort festgelegt haben), oder überprüfen Sie die Richtigkeit des Sicherheitsprinzipals, der das Zertifikat installiert, und klicken Sie dann auf **Weiter**.
4. Behalten Sie auf der Seite **Zertifikatspeicher** den Standardspeicherort bei, und klicken Sie dann auf **Weiter**.
5. Klicken Sie auf **Fertig stellen**. Klicken Sie in der **Sicherheitswarnung** für die Zertifikatinstallation auf **Ja**. Sie können beruhigt auf „Ja“ klicken, da Sie das Zertifikat generiert haben. Das Zertifikat wird nun erfolgreich importiert.

## <a name="vpnclientconfig"></a>Abschnitt 4: Konfigurieren Ihres VPN-Clients
Zum Herstellen einer Verbindung mit dem virtuellen Netzwerk müssen Sie auch einen VPN-Client konfigurieren. Der Client benötigt sowohl ein Clientzertifikat als auch das richtige VPN-Clientkonfigurationspaket, um eine Verbindung herstellen zu können. Führen Sie die folgenden Schritte in der richtigen Reihenfolge aus, um einen VPN-Client zu konfigurieren.

### <a name="part-1-create-the-vpn-client-configuration-package"></a>Teil 1: Erstellen des VPN-Clientkonfigurationspakets
1. Navigieren Sie im klassischen Azure-Portal auf der Seite **Dashboard** für Ihr virtuelles Netzwerk zum Menü „Auf einen Blick“ in der rechten Ecke. Das VPN-Clientpaket enthält Konfigurationsinformationen zum Konfigurieren der VPN-Client-Software, die in Windows integriert. Das Paket installiert keine zusätzlichen Software. Die Einstellungen gelten speziell für das virtuelle Netzwerk, mit dem Sie eine Verbindung herstellen möchten. Die Liste mit den unterstützten Clientbetriebssystemen finden Sie unter [Point-to-Site – Häufig gestellte Fragen](#faq) am Ende dieses Artikels.<br><br>Wählen Sie das Downloadpaket für das Clientbetriebssystem aus, unter dem es installiert wird:
   
   * Wählen Sie für 32-Bit-Clients **32-Bit-Client-VPN-Paket herunterladen**aus.
   * Wählen Sie für 64-Bit-Clients **64-Bit-Client-VPN-Paket herunterladen**aus.
2. Die Erstellung des Clientpakets kann einige Minuten in Anspruch nehmen. Sobald das Paket fertiggestellt wurde, können Sie die Datei herunterladen. Die heruntergeladene *EXE* -Datei kann sicher auf dem lokalen Computer gespeichert werden.
3. Nachdem Sie das VPN-Clientpaket generiert und aus dem klassischen Azure-Portal heruntergeladen haben, können Sie das Clientpaket auf dem Clientcomputer installieren, von dem Sie eine Verbindung mit dem virtuellen Netzwerk herstellen möchten. Wenn Sie das VPN-Clientpaket auf mehreren Clientcomputern installieren möchten, stellen Sie sicher, dass auf diesen auch ein Clientzertifikat installiert worden ist.

### <a name="part-2-install-the-vpn-configuration-package-on-the-client"></a>Teil 2: Installieren des VPN-Konfigurationspakets auf dem Client
1. Kopieren Sie die Konfigurationsdatei lokal auf den Computer, den Sie mit dem virtuellen Netzwerk verbinden möchten, und doppelklicken Sie auf die EXE-Datei. 
2. Sobald das Paket installiert worden ist, können Sie die VPN-Verbindung starten. Das Konfigurationspaket ist nicht von Microsoft signiert. Sie können das Paket mithilfe des Signaturdiensts Ihres Unternehmens oder mit [SignTool](http://go.microsoft.com/fwlink/p/?LinkId=699327) selbst signieren. Sie können das Paket auch ohne Signatur verwenden. Wenn das Paket nicht signiert ist, wird bei der Installation des Pakets aber eine Warnung angezeigt.
3. Navigieren Sie auf dem Clientcomputer zu **Netzwerkeinstellungen**, und klicken Sie auf **VPN**. Die Verbindung wird angezeigt. Sie zeigt den Namen des virtuellen Netzwerks an, mit dem eine Verbindung hergestellt wird. Dies sieht etwa so aus: 
   
    ![VPN-Client](./media/vpn-gateway-point-to-site-create/vpn.png "VPN-Client")

### <a name="part-3-connect-to-azure"></a>Teil 3: Verbinden mit Azure
1. Um eine Verbindung mit Ihrem VNet herzustellen, navigieren Sie auf dem Clientcomputer zu „VPN-Verbindungen“ und suchen nach der VPN-Verbindung, die Sie erstellt haben. Sie hat den gleichen Namen wie das virtuelle Netzwerk. Klicken Sie auf **Verbinden**. Möglicherweise wird eine Popupmeldung angezeigt, die sich auf die Verwendung des Zertifikats bezieht. Klicken Sie in diesem Fall auf **Weiter** , um erhöhte Rechte zu verwenden. 
2. Klicken Sie auf der Statusseite **Verbindung** auf **Verbinden**, um die Verbindung herzustellen. Wenn der Bildschirm **Zertifikat auswählen** angezeigt wird, vergewissern Sie sich, dass das angezeigte Clientzertifikat dem Zertifikat entspricht, die Sie zum Herstellen der Verbindung verwenden möchten. Wenn dies nicht der Fall ist, verwenden Sie den Dropdownpfeil, um das richtige Zertifikat auszuwählen, und klicken Sie dann auf **OK**.
   
    ![VPN-Client 2](./media/vpn-gateway-point-to-site-create/clientconnect.png "VPN-Clientverbindung")
3. Die Verbindung sollte jetzt eingerichtet werden.
   
    ![VPN-Client 3](./media/vpn-gateway-point-to-site-create/connected.png "VPN-Clientverbindung 2")

> [!NOTE]
> Wenn Sie bei Verwendung eines Zertifikats, das mit einer Lösung einer Unternehmenszertifizierungsstelle ausgestellt wurde, Probleme mit der Authentifizierung haben, ist es ratsam, die Authentifizierungsreihenfolge des Clientzertifikats zu überprüfen. Sie können die Reihenfolge der Authentifizierungsliste überprüfen, indem Sie auf das Clientzertifikat doppelklicken und zu **Details > Erweiterte Schlüsselverwendung** navigieren. Achten Sie darauf, dass in der Liste „Client Authentication“ als erster Eintrag aufgeführt ist. Wenn nicht, müssen Sie ein Clientzertifikat basierend auf der User-Vorlage ausstellen, die als ersten Eintrag in der Liste „Client Authentication“ enthält. 
>
>

### <a name="part-4-verify-the-vpn-connection"></a>Teil 4: Überprüfen der VPN-Verbindung
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

## <a name="faq"></a>Point-to-Site – Häufig gestellte Fragen

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter [Virtuelle Computer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) . Weitere Informationen zu Netzwerken und virtuellen Computern finden Sie unter [Azure- und Linux-VM-Netzwerke (Übersicht)](../virtual-machines/virtual-machines-linux-azure-vm-network-overview.md).


