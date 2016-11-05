---
title: Konfigurieren eines VPN-Gateways im klassischen Azure-Portal | Microsoft Docs
description: Dieser Artikel führt Sie durch die Schritte, die erforderlich sind, um das VPN-Gateway Ihres virtuellen Netzwerks zu konfigurieren und den VPN-Routingtyp eines Gateways zu ändern.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: ''
tags: azure-service-management

ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/11/2016
ms.author: cherylmc

---
# Konfigurieren eines VPN Gateways für das klassische Bereitstellungsmodell
Wenn Sie eine sichere standortübergreifende Verbindung zwischen Azure und Ihrem lokalen Standort erstellen möchten, müssen Sie eine VPN-Gatewayverbindung konfigurieren. Im klassischen Bereitstellungsmodell kann ein Gateway einen der beiden VPN-Routingtypen aufweisen: statisch oder dynamisch. Welchen Typ Sie auswählen, richtet sich zum einen nach Ihrem Netzwerkentwurf und zum anderen nach dem lokalen VPN-Gerät, das Sie verwenden möchten.

Einige Konnektivitätsoptionen (wie etwa Punkt-zu-Standort-Verbindungen) setzen ein Gateway mit dynamischem Routing voraus. Wenn Sie Ihr Gateway so konfigurieren möchten, dass es sowohl Punkt-zu-Standort-Verbindungen als auch eine Standort-zu-Standort-Verbindung unterstützt, müssen Sie ein Gateway mit dynamischem Routing konfigurieren, auch wenn für die Standort-zu-Standort-Verbindung eigentlich beide Routingtypen für das VPN-Gateway konfiguriert werden können.

Darüber hinaus müssen Sie sicherstellen, dass das Gerät, das Sie für die Verbindung verwenden möchten, den gewünschten VPN-Routingtyp unterstützt. Weitere Informationen finden Sie unter [Informationen zu VPN-Geräten](vpn-gateway-about-vpn-devices.md).

**Informationen zum Artikel**

Dieser Artikel wurde für das klassische Bereitstellungsmodell und das [klassische Portal](https://manage.windowsazure.com) (nicht das Azure-Portal) geschrieben.

**Informationen zu Azure-Bereitstellungsmodellen**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## Konfigurationsübersicht
Die folgenden Schritte führen Sie durch die Konfiguration des VPN-Gateways im klassischen Azure-Portal. Diese Schritte gelten für Gateways für virtuelle Netzwerke, die mit dem klassischen Bereitstellungsmodell erstellt wurden. Zurzeit sind im Azure-Portal nicht alle Konfigurationseinstellungen für Gateways verfügbar. Sobald dies der Fall ist, erstellen wir neue Anweisungen, die für das Azure-Portal gelten.

1. [Erstellen eines VPN Gateways für Ihr VNET](#create-a-vpn-gateway)
2. [Sammeln von Konfigurationsinformationen für Ihr VPN-Gerät](#gather-information-for-your-vpn-device-configuration)
3. [Konfigurieren des VPN-Geräts](#configure-your-vpn-device)
4. [Überprüfen der lokalen Netzwerkbereiche und der IP-Adresse des VPN-Gateways](#verify-your-local-network-ranges-and-vpn-gateway-ip-address)

### Voraussetzungen
Vor dem Konfigurieren Ihres Gateways müssen Sie zuerst Ihr virtuelles Netzwerk erstellen. Die Schritte zum Erstellen eines virtuellen Netzwerks für standortübergreifende Verbindungen finden Sie unter [Konfigurieren eines virtuellen Netzwerks mit einer Standort-zu-Standort-VPN-Verbindung](vpn-gateway-site-to-site-create.md) bzw. unter [Konfigurieren eines virtuellen Netzwerks mit einer Punkt-zu-Standort-VPN-Verbindung](vpn-gateway-point-to-site-create.md). Führen Sie anschließend die folgenden Schritte durch, um das VPN-Gateway zu konfigurieren und die Informationen zu sammeln, die Sie zum Konfigurieren des VPN-Geräts benötigen.

Wenn Sie bereits über ein VPN-Gateway verfügen und den VPN-Routingtyp ändern möchten, finden Sie weitere Informationen unter [Gewusst wie: Ändern des VPN-Routingtyps für Ihr Gateway](#how-to-change-the-vpn-routing-type-for-your-gateway).

## Erstellen eines VPN-Gateways
1. Vergewissern Sie sich, dass im [klassischen Azure-Portal](https://manage.windowsazure.com) auf der Seite **Netzwerke** in der Statusspalte für Ihr virtuelles Netzwerk **Erstellt** angezeigt wird.
2. Klicken Sie in der Spalte **Name** auf den Namen Ihres virtuellen Netzwerks.
3. Beachten Sie auf der Seite **Dashboard**, dass für dieses VNet noch kein Gateway konfiguriert wurde. Dieser Status wird angezeigt, wenn Sie die Konfigurationsschritte für Ihr Gateway durchführen.

![Gateway nicht erstellt](./media/vpn-gateway-configure-vpn-gateway-mp/IC717025.png)

Klicken Sie anschließend am unteren Seitenrand auf **Gateway erstellen**. Sie haben die Wahl zwischen *Statisches Routing* und *Dynamisches Routing*. Bei der Wahl des VPN-Routingtyps müssen einige Faktoren berücksichtigt werden. Dazu zählt beispielsweise die Frage, was das VPN-Gerät unterstützt und ob Punkt-zu-Standort-Verbindungen unterstützt werden müssen. Unter [Informationen zu VPN-Geräten für VPN-Gatewayverbindungen zwischen Standorten](vpn-gateway-about-vpn-devices.md) erfahren Sie, welchen VPN-Routingtyp Sie benötigen. Wenn Sie die VPN-Routingtypen nach der Erstellung des Gateways ändern möchten, müssen Sie das Gateway löschen und neu erstellen. Wenn Sie zum Bestätigen der Gatewayerstellung aufgefordert werden, klicken Sie auf **Ja**.

![VPN-Routingtyp des Gateways](./media/vpn-gateway-configure-vpn-gateway-mp/IC717026.png)

Während der Gatewayerstellung wechselt die Gatewaygrafik auf der Seite zu Gelb und zeigt *Gateway wird erstellt* an. Die Gatewayerstellung kann bis zu 45 Minuten dauern. Warten Sie, bis die Gatewayerstellung abgeschlossen ist, bevor Sie mit anderen Konfigurationseinstellungen fortfahren.

![Gatewayerstellung](./media/vpn-gateway-configure-vpn-gateway-mp/IC717027.png)

Wenn der Gatewaystatus zu *Verbindung wird hergestellt* wechselt, können Sie die für das VPN-Gerät benötigten Informationen sammeln.

![Gatewayverbindungsherstellung](./media/vpn-gateway-configure-vpn-gateway-mp/IC717028.png)

## Sammeln von Konfigurationsinformationen für Ihr VPN-Gerät
Sammeln Sie nach der Gatewayerstellung die Konfigurationsinformationen für Ihr VPN-Gerät. Diese Informationen finden Sie auf der Dashboardseite für Ihr virtuelles Netzwerk:

1. **Gateway-IP-Adresse**: Die IP-Adresse befindet sich auf der Dashboardseite. Sie wird erst nach Abschluss der Gatewayerstellung angezeigt.
2. **Gemeinsam verwendeter Schlüssel**: Klicken Sie am unteren Bildschirmrand auf **Schlüssel verwalten**. Klicken Sie auf das Symbol neben dem Schlüssel, um ihn in die Zwischenablage zu kopieren. Fügen Sie den Schlüssel anschließend ein, und speichern Sie ihn. Diese Schaltfläche funktioniert nur, wenn ein einzelner Standort-zu-Standort-VPN-Tunnel vorhanden ist. Sind mehrere Standort-zu-Standort-VPN-Tunnel vorhanden, muss die API oder das PowerShell-Cmdlet *Get Virtual Network Gateway Shared Key* verwendet werden.

![Schlüssel verwalten](./media/vpn-gateway-configure-vpn-gateway-mp/IC717029.png)

## Konfigurieren des VPN-Geräts
Nach Abschluss der vorherigen Schritte müssen Sie oder Ihr Netzwerkadministrator das VPN-Gerät konfigurieren, um die Verbindung zu erstellen. Weitere Informationen zu VPN-Geräten finden Sie unter [Informationen zu VPN-Geräten und Gateways für virtuelle Netzwerkverbindungen](vpn-gateway-about-vpn-devices.md).

Wenn das VPN-Gerät konfiguriert wurde, können Sie Ihre aktualisierten Verbindungsinformationen auf der Dashboardseite für Ihr VNet anzeigen.

Mithilfe eines der folgenden Befehle können Sie Ihre Verbindung testen:

|  | Cisco ASA | Cisco ISR/ASR | Juniper SSG/ISG | Juniper SRX/J |
| --- | --- | --- | --- | --- |
| **SAs im Hauptmodus prüfen** |show crypto isakmp sa |show crypto isakmp sa |get ike cookie |show security ike security-association |
| **SAs im schnellen Modus prüfen** |show crypto ipsec sa |show crypto ipsec sa |get sa |show security ipsec security-association |

## Überprüfen der lokalen Netzwerkbereiche und der IP-Adresse des VPN-Gateways
### Überprüfen der IP-Adresse Ihres VPN-Gateways
Damit ein Gateway ordnungsgemäß eine Verbindung herstellen kann, muss die IP-Adresse für Ihr VPN-Gerät korrekt für das lokale Netzwerk konfiguriert sein, das Sie für Ihre standortübergreifende Konfiguration angegeben haben. In der Regel wird dies beim Festlegen der Standort-zu-Standort-Konfiguration erledigt. Wenn Sie dieses lokale Netzwerk allerdings bereits mit einem anderen Gerät verwendet haben oder sich die IP-Adresse für dieses lokale Netzwerk geändert hat, ändern Sie die Einstellungen, und geben Sie die richtige Gateway-IP-Adresse an.

1. Klicken Sie zum Überprüfen der Gateway-IP-Adresse im linken Bereich des Portals auf **Netzwerke**, und wählen Sie dann am oberen Seitenrand die Option **Lokale Netzwerke** aus. Die VPN-Gateway-Adressen für die einzelnen lokalen Netzwerke, die Sie erstellt haben, werden angezeigt. Wählen Sie zum Bearbeiten der IP-Adresse das entsprechende VNet aus, und klicken Sie am unteren Seitenrand auf **Bearbeiten**.
2. Bearbeiten Sie auf der Seite **Details zum lokalen Netzwerk angeben** die IP-Adresse, und klicken Sie anschließend am unteren Seitenrand auf den Weiter-Pfeil.
3. Klicken Sie rechts unten auf der Seite **Den Adressraum angeben** auf das Häkchen, um die Einstellungen zu speichern.

### Überprüfen der Adressbereiche für Ihre lokalen Netzwerke
Damit der richtige Datenverkehr über das Gateway zu Ihrem lokalen Standort geleitet wird, müssen Sie überprüfen, ob jeder IP-Adressbereich angegeben wurde. Jeder Bereich muss in Ihrer Azure-Konfiguration für **lokale Netzwerke** aufgeführt werden. Je nach Netzwerkkonfiguration Ihres lokalen Standorts kann dies eine recht aufwendige Aufgabe sein. Datenverkehr für eine IP-Adresse, die in den aufgeführten Bereichen enthalten ist, wird über das VPN-Gateway des virtuellen Netzwerks gesendet. Bei den aufgeführten Bereichen muss es sich nicht um private Bereiche handeln. Es empfiehlt sich jedoch zu überprüfen, ob mit der lokalen Konfiguration eingehender Datenverkehr empfangen werden kann.

Führen Sie zum Bearbeiten oder Hinzufügen der Bereiche für ein lokales Netzwerk folgende Schritte aus.

1. Klicken Sie zum Bearbeiten der IP-Bereiche für ein lokales Netzwerk im linken Bereich des Portals auf **Netzwerke**, und wählen Sie dann am oberen Seitenrand die Option **Lokale Netzwerke** aus. Im Portal lassen sich die aufgeführten Bereiche am einfachsten auf der Seite **Bearbeiten** anzeigen. Wählen Sie zum Anzeigen der Bereiche das entsprechende VNet aus, und klicken Sie am unteren Seitenrand auf **Bearbeiten**.
2. Nehmen Sie auf der Seite **Details zum lokalen Netzwerk angeben** keine Änderungen vor. Klicken Sie am unteren Seitenrand auf den Weiter-Pfeil.
3. Nehmen Sie auf der Seite **Den Adressraum angeben** die gewünschten Änderungen vor. Klicken Sie anschließend auf das Häkchen, um die Konfiguration zu speichern.

## Anzeigen des Gatewaydatenverkehrs
Das Gateway und den Gatewaydatenverkehr können Sie auf der Dashboardseite von Virtual Network anzeigen.

Auf der Dashboardseite wird Folgendes angezeigt:

* Die ein- und ausgehende Datenmenge, die über das Gateway abgewickelt wird.
* Die Namen der DNS-Server, die für Ihr virtuelles Netzwerk angegeben sind.
* Die Verbindung zwischen Gateway und VPN-Gerät.
* Der gemeinsam verwendete Schlüssel, mit dem die Gatewayverbindung mit Ihrem VPN-Gerät konfiguriert wurde.

## Gewusst wie: Ändern des VPN-Routingtyps für Ihr Gateway
Da einige Konnektivitätskonfigurationen nur für bestimmte Gatewayroutingtypen verfügbar sind, kann es vorkommen, dass Sie den VPN-Routingtyp eines vorhandenen VPN Gateways ändern müssen. Ein Beispiel: Sie möchten eine bereits vorhandene Standort-zu-Standort-Verbindung mit statischem Gateway um Punkt-zu-Standort-Verbindungen erweitern. Punkt-zu-Standort-Verbindungen erfordern ein dynamisches Gateway. Das bedeutet: Um eine Punkt-zu-Standort-Verbindung zu konfigurieren, müssen Sie den VPN-Routingtyp Ihres Gateways von „statisch“ zu „dynamisch“ ändern.

Zum Ändern des VPN-Routingtyps eines Gateways müssen Sie das vorhandene Gateway löschen und anschließend ein neues Gateway mit dem neuen Routingtyp erstellen. Sie müssen nicht das gesamte virtuelle Netzwerk löschen, um den Routingtyp für das Gateway zu ändern.

Bevor Sie den VPN-Routingtyp Ihres Gateways ändern, vergewissern Sie sich, dass Ihr VPN-Gerät den gewünschten Routingtyp unterstützt. Unter [Informationen zu VPN-Geräten und Gateways für virtuelle Netzwerkverbindungen](vpn-gateway-about-vpn-devices.md) können Sie neue Routingkonfigurationsbeispiele herunterladen und sich über die Anforderungen von VPN-Geräten informieren.

> [!IMPORTANT]
> Wenn Sie ein VPN-Gateway für das virtuelle Netzwerk löschen, wird die dem Gateway zugewiesene VIP freigegeben. Wenn Sie das Gateway neu erstellen, wird ihm eine neue VIP zugewiesen.
> 
> 

1. **Löschen Sie das vorhandene VPN-Gateway.**
   
    Klicken Sie am unteren Rand der Dashboardseite für Ihr virtuelles Netzwerk auf **Gateway löschen**. Warten Sie, bis eine Benachrichtigung mit dem Hinweis erscheint, dass das Gateway gelöscht wurde. Sobald die Benachrichtigung angezeigt wird, dass das Gateway gelöscht wurde, können Sie ein neues Gateway erstellen.
2. **Erstellen Sie ein neues VPN-Gateway.**
   
    Führen Sie das unter [Erstellen eines VPN-Gateways](#create-a-vpn-gateway) beschriebene Verfahren aus.

## Nächste Schritte
Sie können dem virtuellen Netzwerk virtuelle Computer hinzufügen. Weitere Informationen finden Sie unter [Erstellen eines benutzerdefinierten virtuellen Computers](../virtual-machines/virtual-machines-windows-classic-createportal.md).

Wenn Sie eine Punkt-zu-Standort-VPN-Verbindung konfigurieren möchten, helfen Ihnen die Informationen unter [Konfigurieren einer Punkt-zu-Standort-VPN-Verbindung](vpn-gateway-point-to-site-create.md).

<!---HONumber=AcomDC_0817_2016-->