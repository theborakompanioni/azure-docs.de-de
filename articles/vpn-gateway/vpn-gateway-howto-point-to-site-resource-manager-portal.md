---
title: 'Herstellen einer Point-to-Site-Verbindung zwischen einem Computer und einem virtuellen Netzwerk unter Verwendung der Zertifikatauthentifizierung: Azure-Portal | Microsoft-Dokumentation'
description: "Stellen Sie zwischen einem Computer und Ihrer Azure Virtual Network-Instanz unter Verwendung der Zertifikatauthentifizierung eine sichere P2S-VPN-Gatewayverbindung her. Dieser Artikel gilt für das Resource Manager-Bereitstellungsmodell und für die Verwendung des Azure-Portals."
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
ms.date: 08/10/2017
ms.author: cherylmc
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 5c8e99f3ba52ef5d6f9f99ac24891c38e8970fff
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-certificate-authentication-azure-portal"></a>Konfigurieren einer Point-to-Site-Verbindung mit einem VNET unter Verwendung der Zertifikatauthentifizierung: Azure-Portal

In diesem Artikel wird beschrieben, wie Sie ein VNET mit einer P2S-Verbindung im Resource Manager-Bereitstellungsmodell mithilfe des Azure-Portals erstellen. Bei dieser Konfiguration werden Clients, die eine Verbindung herstellen, mithilfe von Zertifikaten authentifiziert. Sie können diese Konfiguration auch mit einem anderen Bereitstellungstool oder -modell erstellen. Wählen Sie hierzu in der folgenden Liste eine andere Option:

> [!div class="op_single_selector"]
> * [Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Azure-Portal (klassisch)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Mit einem P2S-VPN-Gateway (Point-to-Site) können Sie von einem einzelnen Clientcomputer aus eine sichere Verbindung mit Ihrem virtuellen Netzwerk herstellen. Eine P2S-VPN-Verbindung ist nützlich, wenn Sie an einem Remotestandort (beispielsweise bei der Telearbeit zu Hause oder in einer Konferenz) eine Verbindung mit Ihrem VNET herstellen möchten. Wenn nur einige wenige Clients eine Verbindung mit einem VNET herstellen müssen, ist ein P2S-VPN (und nicht ein S2S-VPN) ebenfalls eine nützliche Lösung. 

P2S nutzt das Secure Socket Tunneling-Protokoll (SSTP), ein SSL-basiertes VPN-Protokoll. Eine P2S-Verbindung wird hergestellt, indem Sie die Verbindung vom Clientcomputer aus starten.

![P2S-Diagramm](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/point-to-site-connection-diagram.png)

Für Point-to-Site-Verbindungen mit Zertifikatauthentifizierung wird Folgendes benötigt:

* Ein RouteBased-VPN-Gateway.
* Der öffentliche Schlüssel (CER-Datei) für ein Stammzertifikat (in Azure hochgeladen). Sobald das Zertifikat hochgeladen wurde, wird es als vertrauenswürdiges Zertifikat betrachtet und für die Authentifizierung verwendet.
* Ein Clientzertifikat, das über das Stammzertifikat generiert und auf jedem Clientcomputer installiert wurde, der eine Verbindung mit dem VNET herstellen wird. Dieses Zertifikat wird für die Clientauthentifizierung verwendet.
* Ein VPN-Clientkonfigurationspaket. Das VPN-Clientkonfigurationspaket enthält die erforderlichen Informationen, damit der Client eine Verbindung mit dem VNET herstellen kann. Das Paket konfiguriert den vorhandenen nativen VPN-Client des Windows-Betriebssystems. Jeder Client, der eine Verbindung herstellt, muss mit dem Konfigurationspaket konfiguriert werden.

Point-to-Site-Verbindungen erfordern weder ein VPN-Gerät noch eine lokale öffentliche IP-Adresse. Die VPN-Verbindung wird über das Secure Socket Tunneling-Protokoll (SSTP) hergestellt. Auf Serverseite werden die SSTP-Versionen 1.0, 1.1 und 1.2 unterstützt. Der Client entscheidet, welche Version verwendet wird. Unter Windows 8.1 und höher wird standardmäßig SSTP 1.2 verwendet.

Weitere Informationen zu Point-to-Site-Verbindungen finden Sie unter [Point-to-Site – Häufig gestellte Fragen](#faq) am Ende dieses Artikels.

#### <a name="example"></a>Beispielwerte

Sie können die folgenden Werte zum Erstellen einer Testumgebung oder zum besseren Verständnis der Beispiele in diesem Artikel nutzen:

* **VNET-Name:** VNet1
* **Adressraum:** 192.168.0.0/16<br>In diesem Beispiel verwenden wir nur einen einzelnen Adressraum. Sie können für Ihr VNet aber auch mehrere Adressräume verwenden.
* **Subnetzname:** FrontEnd
* **Subnetzadressbereich:** 192.168.1.0/24
* **Abonnement:** Falls Sie über mehrere Abonnements verfügen, sollten Sie sich vergewissern, dass Sie das richtige Abonnement verwenden.
* **Ressourcengruppe:** TestRG
* **Standort:** USA, Osten
* **GatewaySubnet:** 192.168.200.0/24<br>
* **DNS-Server:** (optional) IP-Adresse des DNS-Servers, der für die Namensauflösung verwendet werden soll
* **Name des Gateways für virtuelle Netzwerke:** VNet1GW
* **Gatewaytyp:** VPN
* **VPN-Typ:** Routenbasiert
* **Öffentliche IP-Adresse:** VNet1GWpip
* **Verbindungstyp:** Point-to-Site
* **Clientadresspool:** 172.16.201.0/24<br>VPN-Clients, die über diese Point-to-Site-Verbindung eine Verbindung mit dem VNet herstellen, erhalten eine IP-Adresse aus dem Clientadresspool.

## <a name="createvnet"></a>1. Erstellen eines virtuellen Netzwerks

Stellen Sie zunächst sicher, dass Sie über ein Azure-Abonnement verfügen. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial) registrieren.

[!INCLUDE [Basic Point-to-Site VNet](../../includes/vpn-gateway-basic-p2s-vnet-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>2. Hinzufügen eines Gatewaysubnetzes

Bevor Sie das virtuelle Netzwerk mit einem Gateway verbinden, müssen Sie das Gatewaysubnetz für das virtuelle Netzwerk erstellen, mit dem Sie eine Verbindung herstellen möchten. Für die Gatewaydienste werden die im Gatewaysubnetz angegebenen IP-Adressen verwendet. Erstellen Sie nach Möglichkeit ein Gatewaysubnetz mit einem CIDR-Block vom Typ „/28“ oder „/27“, damit genügend IP-Adressen für zukünftige zusätzliche Konfigurationsanforderungen zur Verfügung stehen.

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-p2s-rm-portal-include.md)]

## <a name="dns"></a>3. Angeben eines DNS-Servers (optional)

Nach Erstellung des virtuellen Netzwerks können Sie für die Namensauflösung die IP-Adresse eines DNS-Servers hinzufügen. Der DNS-Server ist für diese Konfiguration optional, für die Namensauflösung jedoch erforderlich. Wenn ein Wert angegeben wird, wird kein neuer DNS-Server erstellt. Die IP-Adresse des angegebenen DNS-Servers muss dazu in der Lage sein, die Namen für die Ressourcen aufzulösen, mit denen Sie eine Verbindung herstellen möchten. In diesem Beispiel wurde eine private IP-Adresse verwendet. Bei dieser IP-Adresse handelt es sich aber wahrscheinlich nicht um die IP-Adresse Ihres DNS-Servers. Achten Sie darauf, dass Sie Ihre eigenen Werte verwenden.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>4. Erstellen eines Gateways für das virtuelle Netzwerk

[!INCLUDE [create-gateway](../../includes/vpn-gateway-add-gw-p2s-rm-portal-include.md)]

## <a name="generatecert"></a>5. Generieren von Zertifikaten

Zertifikate werden von Azure zum Authentifizieren von Clients verwendet, die eine Verbindung mit einem VNET über eine Point-to-Site-VPN-Verbindung herstellen. Nachdem Sie ein Stammzertifikat abgerufen haben, müssen Sie die Informationen des öffentlichen Schlüssels in Azure [hochladen](#uploadfile). Das Stammzertifikat wird dann von Azure als „vertrauenswürdig“ für die Verbindung mit dem virtuellen Netzwerk über P2S betrachtet. Sie generieren darüber hinaus Clientzertifikate aus dem vertrauenswürdigen Stammzertifikat und installieren sie dann auf den einzelnen Clientcomputern. Mit dem Clientzertifikat wird der Client authentifiziert, wenn er eine Verbindung mit dem VNET initiiert. 

### <a name="getcer"></a>1. Beschaffen der CER-Datei für ein Stammzertifikat

[!INCLUDE [root-certificate](../../includes/vpn-gateway-p2s-rootcert-include.md)]

### <a name="generateclientcert"></a>2. Generieren eines Clientzertifikats

[!INCLUDE [generate-client-cert](../../includes/vpn-gateway-p2s-clientcert-include.md)]

## <a name="addresspool"></a>6. Hinzufügen des Clientadresspools

Der Clientadresspool ist ein Bereich privater IP-Adressen, die Sie angeben. Die Clients, die eine Verbindung über ein P2S-VPN herstellen, erhalten eine IP-Adresse aus diesem Bereich. Verwenden Sie einen privaten IP-Adressbereich, der sich nicht mit dem lokalen Standort überschneidet, aus dem Sie Verbindungen herstellen möchten. Der Bereich darf sich auch nicht mit dem VNET überschneiden, mit dem Sie Verbindungen herstellen möchten.

1. Navigieren Sie nach Erstellung des Gateways für virtuelle Netzwerke auf dem Blatt des Gateways für virtuelle Netzwerke zum Abschnitt **Einstellungen**. Klicken Sie im Abschnitt **Einstellungen** auf **Punkt-zu-Standort-Konfiguration**, um die Seite **Konfiguration** zu öffnen.

  ![Seite „Point-to-Site“](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/gatewayblade.png)
2. Auf der Seite **Point-to-Site-Konfiguration** können Sie den automatisch ausgefüllten Bereich löschen und dann den privaten IP-Adressbereich hinzufügen, den Sie verwenden möchten. Klicken Sie auf **Speichern**, um die Einstellung zu überprüfen und zu speichern.

  ![Clientadresspool](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/ipaddresspool.png)

## <a name="uploadfile"></a>7. Hochladen der Daten des öffentlichen Zertifikats für das Stammzertifikat

Nachdem das Gateway erstellt wurde, laden Sie die Informationen des öffentlichen Schlüssels für das Stammzertifikat in Azure hoch. Nach dem Hochladen der Daten des öffentlichen Zertifikats kann Azure sie verwenden, um Clients zu authentifizieren, auf denen ein aus dem vertrauenswürdigen Stammzertifikat generiertes Clientzertifikat installiert ist. Sie können weitere vertrauenswürdige Stammzertifikate hochladen – bis zu 20 insgesamt.

1. Die Zertifikate werden auf der Seite **Point-to-Site-Konfiguration** im Abschnitt **Stammzertifikat** hinzugefügt.  
2. Vergewissern Sie sich, dass Sie das Stammzertifikat als X.509-CER-Datei mit Base-64-Codierung exportiert haben. Das Zertifikat muss in diesem Format exportiert werden, damit Sie es mit einem Text-Editor öffnen können.
3. Öffnen Sie das Zertifikat mit einem Text-Editor (beispielsweise mit dem Windows-Editor). Stellen Sie beim Kopieren der Zertifikatsdaten sicher, dass Sie den Text als fortlaufende Zeile ohne Wagenrückläufe oder Zeilenvorschübe kopieren. Es kann erforderlich sein, Ihre Ansicht im Text-Editor so zu ändern, dass die Symbole bzw. alle Zeichen angezeigt werden, damit die Wagenrückläufe bzw. Zeilenvorschübe sichtbar sind. Kopieren Sie nur den folgenden Abschnitt als fortlaufende Zeile:

  ![Zertifikatdaten](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png)
4. Fügen Sie die Zertifikatdaten im Feld **Daten des öffentlichen Zertifikats** ein. Geben Sie dem Zertifikat einen **Namen**, und klicken Sie dann auf **Speichern**. Sie können bis zu 20 vertrauenswürdige Stammzertifikate hinzufügen.

  ![Zertifikatupload](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcertupload.png)

## <a name="clientconfig"></a>8. Generieren und Installieren des Konfigurationspakets für VPN-Clients

Zum Herstellen einer Verbindung mit einem VNET über ein P2S-VPN muss jeder Client ein Clientkonfigurationspaket installieren, das den nativen VPN-Client mit den Einstellungen und Dateien konfiguriert, die für das Herstellen einer Verbindung mit dem virtuellen Netzwerk erforderlich sind. Das VPN-Clientkonfigurationspaket konfiguriert den nativen Windows-VPN-Client, es installiert keinen neuen oder anderen VPN-Client.

Sie können auf jedem Clientcomputer das gleiche VPN-Clientkonfigurationspaket verwenden – vorausgesetzt, es handelt sich dabei um die passende Version für die Architektur des jeweiligen Clients. Die Liste mit den unterstützten Clientbetriebssystemen finden Sie unter [Point-to-Site – Häufig gestellte Fragen](#faq) am Ende dieses Artikels.

### <a name="step-1---generate-and-download-the-client-configuration-package"></a>Schritt 1: Generieren und Herunterladen des Clientkonfigurationspakets

1. Klicken Sie auf der Seite **Punkt-zu-Standort-Konfiguration** auf **VPN-Client herunterladen**, um die Seite **VPN-Client herunterladen** zu öffnen. Die Paketgenerierung kann etwas dauern.

  ![Herunterladen des VPN-Clients 1](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadvpnclient1.png)
2. Wählen Sie das passende Paket für Ihren Client aus, und klicken Sie auf **Herunterladen**. Speichern Sie die Konfigurationspaketdatei. Sie installieren das VPN-Clientkonfigurationspaket auf jedem Clientcomputer, der eine Verbindung mit dem virtuellen Netzwerk herstellt.

  ![Herunterladen des VPN-Clients 2](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpnclient.png)

### <a name="step-2---install-the-client-configuration-package"></a>Schritt 2: Installieren des Clientkonfigurationspakets

1. Kopieren Sie die Konfigurationsdatei lokal auf den Computer, für den Sie eine Verbindung mit dem virtuellen Netzwerk herstellen möchten. 
2. Doppelklicken Sie auf die EXE-Datei, um das Paket auf dem Clientcomputer zu installieren. Da Sie das Konfigurationspaket erstellt haben, ist es nicht signiert, und unter Umständen wird eine Warnung angezeigt. Klicken Sie im ggf. angezeigten Windows SmartScreen-Popup auf **Weitere Informationen** (links) und dann auf **Trotzdem ausführen**, um das Paket zu installieren.
3. Installieren Sie das Paket auf dem Clientcomputer. Klicken Sie im ggf. angezeigten Windows SmartScreen-Popup auf **Weitere Informationen** (links) und dann auf **Trotzdem ausführen**, um das Paket zu installieren.
4. Navigieren Sie auf dem Clientcomputer zu **Netzwerkeinstellungen**, und klicken Sie auf **VPN**. Die VPN-Verbindung zeigt den Namen des virtuellen Netzwerks an, mit dem eine Verbindung hergestellt wird.

## <a name="installclientcert"></a>9. Installieren eines exportierten Clientzertifikats

Wenn Sie eine P2S-Verbindung mit einem anderen Clientcomputer als dem für die Generierung der Clientzertifikate verwendeten Computer herstellen möchten, müssen Sie ein Clientzertifikat installieren. Beim Installieren eines Clientzertifikats benötigen Sie das Kennwort, das beim Exportieren des Clientzertifikats erstellt wurde. Normalerweise müssen Sie hierfür nur auf das Zertifikat doppelklicken und dann die Installation durchführen.

Stellen Sie sicher, dass das Clientzertifikat zusammen mit der gesamten Zertifikatkette als PFX-Datei exportiert wurden (Standardeinstellung). Andernfalls sind die Stammzertifikatinformationen nicht auf dem Clientcomputer vorhanden, und der Client kann nicht ordnungsgemäß authentifiziert werden. Weitere Informationen finden Sie unter [Installieren eines exportierten Clientzertifikats](vpn-gateway-certificates-point-to-site.md#install).

## <a name="connect"></a>10. Herstellen einer Verbindung mit Azure

1. Um eine Verbindung mit Ihrem VNet herzustellen, navigieren Sie auf dem Clientcomputer zu „VPN-Verbindungen“ und suchen nach der VPN-Verbindung, die Sie erstellt haben. Sie hat den gleichen Namen wie das virtuelle Netzwerk. Klicken Sie auf **Verbinden**. Möglicherweise wird eine Popupmeldung angezeigt, die sich auf die Verwendung des Zertifikats bezieht. Klicken Sie auf **Weiter**, um erhöhte Rechte zu verwenden.

2. Klicken Sie auf der Statusseite **Verbindung** auf **Verbinden**, um die Verbindung herzustellen. Wenn der Bildschirm **Zertifikat auswählen** angezeigt wird, vergewissern Sie sich, dass das angezeigte Clientzertifikat dem Zertifikat entspricht, die Sie zum Herstellen der Verbindung verwenden möchten. Wenn dies nicht der Fall ist, verwenden Sie den Dropdownpfeil, um das richtige Zertifikat auszuwählen, und klicken Sie dann auf **OK**.

  ![Verbindungsherstellung zwischen VPN-Client und Azure](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/clientconnect.png)
3. Die Verbindung wurde hergestellt.

  ![Verbindung hergestellt](./media/vpn-gateway-howto-point-to-site-resource-manager-portal/connected.png)

#### <a name="troubleshooting-p2s-connections"></a>Problembehandlung für P2S-Verbindungen

[!INCLUDE [verifies client certificates](../../includes/vpn-gateway-certificates-verify-client-cert-include.md)]

## <a name="verify"></a>11. Überprüfen der Verbindung

1. Um sicherzustellen, dass die VPN-Verbindung aktiv ist, öffnen Sie eine Eingabeaufforderung mit Administratorrechten, und führen Sie *Ipconfig/all*aus.
2. Zeigen Sie die Ergebnisse an. Beachten Sie, dass die erhaltene IP-Adresse eine der Adressen aus dem Clientadresspool des Punkt-zu-Standort-VPN ist, den Sie in Ihrer Konfiguration angegeben haben. Die Ergebnisse sehen in etwa wie in diesem Beispiel aus:

  ```
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
  ```

## <a name="connectVM"></a>Herstellen einer Verbindung mit einem virtuellem Computer

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-p2s-include.md)]

## <a name="add"></a>Hinzufügen oder Entfernen vertrauenswürdiger Stammzertifikate

Sie können vertrauenswürdige Stammzertifikate hinzufügen und aus Azure entfernen. Wenn Sie ein Stammzertifikat entfernen, können Clients, für die über diesen Stamm ein Zertifikat generiert wurde, nicht authentifiziert werden und somit auch keine Verbindung herstellen. Wenn Sie für einen Client die Authentifizierung und Verbindungsherstellung durchführen möchten, müssen Sie ein neues Clientzertifikat installieren, das aus einem für Azure vertrauenswürdigen (hochgeladenen) Stammzertifikat generiert wurde.

### <a name="to-add-a-trusted-root-certificate"></a>So fügen Sie ein vertrauenswürdiges Stammzertifikat hinzu

Sie können Azure bis zu 20 vertrauenswürdige CER-Stammzertifikatdateien hinzufügen. Eine entsprechende Anleitung finden Sie in diesem Artikel im Abschnitt zum [Hochladen eines vertrauenswürdigen Stammzertifikats](#uploadfile).

### <a name="to-remove-a-trusted-root-certificate"></a>Entfernen eines vertrauenswürdigen Stammzertifikats

1. Navigieren Sie zum Entfernen eines vertrauenswürdigen Zertifikats zur Seite **Punkt-zu-Standort-Konfiguration** für Ihr Gateway für virtuelle Netzwerke.
2. Suchen Sie auf der Seite im Abschnitt **Stammzertifikat** nach dem Zertifikat, das Sie entfernen möchten.
3. Klicken Sie neben dem Zertifikat auf die Auslassungspunkte und anschließend auf „Entfernen“.

## <a name="revokeclient"></a>Sperren eines Clientzertifikats

Sie können Clientzertifikate sperren. Anhand der Zertifikatsperrliste können Sie basierend auf einzelnen Clientzertifikaten selektiv Punkt-zu-Standort-Verbindungen verweigern. Das ist nicht dasselbe wie das Entfernen eines vertrauenswürdigen Stammzertifikats. Wenn Sie ein vertrauenswürdiges Stammzertifikat (CER-Datei) aus Azure entfernen, wird der Zugriff für alle Clientzertifikate gesperrt, die mit dem gesperrten Stammzertifikat generiert oder signiert wurden. Wenn Sie anstelle des Stammzertifikats ein Clientzertifikat sperren, können die anderen Zertifikate, die auf der Grundlage des Stammzertifikats generiert wurden, weiterhin für die Authentifizierung verwendet werden.

Üblicherweise wird das Stammzertifikat zum Verwalten des Zugriffs auf Team- oder Organisationsebene verwendet. Eine genauer abgestufte Steuerung des Zugriffs für einzelne Benutzer erfolgt hingegen mit gesperrten Clientzertifikaten.

### <a name="to-revoke-a-client-certificate"></a>So sperren Sie ein Clientzertifikat

Sie können ein Clientzertifikat sperren, indem Sie den Fingerabdruck der Sperrliste hinzufügen.

1. Rufen Sie den Fingerabdruck des Clientzertifikats ab. Weitere Informationen finden Sie unter [Vorgehensweise: Abrufen des Fingerabdrucks eines Zertifikats](https://msdn.microsoft.com/library/ms734695.aspx).
2. Kopieren Sie ihn in einen Text-Editor, und entfernen Sie alle Leerzeichen, sodass eine fortlaufende Zeichenfolge entsteht.
3. Navigieren Sie zur Seite **Punkt-zu-Standort-Konfiguration** des Gateways für virtuelle Netzwerke. Dies ist die Seite, die Sie zum [Hochladen eines vertrauenswürdigen Stammzertifikats](#uploadfile) verwendet haben.
4. Geben Sie im Abschnitt **Gesperrte Zertifikate** einen Anzeigenamen für das Zertifikat ein. (Dabei muss es sich nicht um den allgemeinen Namen des Zertifikats handeln.)
5. Kopieren Sie die Fingerabdruck-Zeichenfolge, und fügen Sie sie in das Feld **Fingerabdruck** ein.
6. Der Fingerabdruck wird überprüft und automatisch der Sperrliste hinzugefügt. Auf dem Bildschirm wird eine Meldung mit dem Hinweis angezeigt, dass die Liste aktualisiert wird. 
7. Nach Abschluss der Aktualisierung kann das Zertifikat nicht mehr für die Verbindungsherstellung verwendet werden. Clients, die versuchen, unter Verwendung dieses Zertifikats eine Verbindung herzustellen, erhalten eine Meldung mit dem Hinweis, dass das Zertifikat nicht mehr gültig ist.

## <a name="faq"></a>Point-to-Site – Häufig gestellte Fragen

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Nächste Schritte
Sobald die Verbindung hergestellt ist, können Sie Ihren virtuellen Netzwerken virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter [Virtuelle Computer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute) . Weitere Informationen zu Netzwerken und virtuellen Computern finden Sie unter [Azure- und Linux-VM-Netzwerke (Übersicht)](../virtual-machines/linux/azure-vm-network-overview.md).
