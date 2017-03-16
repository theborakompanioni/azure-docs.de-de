---
title: "Verbinden von Computern und Geräten mit der OMS mithilfe des OMS-Gateways | Microsoft-Dokumentation"
description: "Verbinden Sie Ihre OMS-verwalteten Geräte und Operations Manager-überwachten Computer mit dem OMS-Gateway, um Daten an den OMS-Dienst zu senden, wenn sie nicht über Internetzugriff verfügen."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: ae9a1623-d2ba-41d3-bd97-36e65d3ca119
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: a0c8af30fbed064001c3fd393bf0440aa1cb2835
ms.openlocfilehash: de2e6d201ba54774697356e1cd55c4881664a401
ms.lasthandoff: 02/28/2017


---
# <a name="connect-computers-and-devices-without-internet-access-to-oms-using-the-oms-gateway"></a>Verbinden von Computern und Geräten ohne Internetzugriff mit der OMS mithilfe des OMS-Gateways

Dieses Dokument beschreibt, wie Ihre OMS-verwalteten Geräte und Ihre von System Center Operations Manager überwachten Computer Daten an den OMS-Dienst senden können, wenn sie nicht über Internetzugriff verfügen. Das OMS-Gateway kann Daten sammeln und in seinem Namen an den OMS-Dienst senden.

Das Gateway ist ein HTTP-Forwardproxy, der HTTP-Tunnel mit dem Befehl HTTP CONNECT unterstützt. Das Gateway kann bis zu 2000 gleichzeitig verbundene OMS-Geräte bei Ausführung auf einem 16-GB-Server mit 4-Core-CPU unter Windows verarbeiten.

Beispielsweise können Sie in Ihrem Unternehmen oder in einer großen Organisation Server mit Netzwerkkonnektivität, aber ohne Internetkonnektivität haben. Oder aber Sie haben zwar zahlreiche Point-of-Sale- (POS-)Geräte, verfügen aber nicht über die Mittel,diese direkt zu überwachen. Und in einem anderen Beispiel kann der Operations Manager das OMS-Gateway als Proxyserver verwenden. In diesen Beispielen kann das OMS-Gateway Daten von den Agents, die auf diesen Servern oder POS-Geräten installiert sind, an die OMS übertragen.

Anstatt dass jeder einzelne Agent Daten direkt an das OMS-Gerät sendet und eine direkte Internetverbindung benötigt, werden alle Agentdaten über einen einzelnen Computer mit Internetverbindung gesendet. Auf diesem Computer installieren und verwenden Sie das Gateway. In diesem Szenario können Sie Agents auf allen Computern installieren, auf denen Sie Daten sammeln möchten. Das Gateway überträgt Daten dann direkt von den Agents an die OMS; das Gateway analysiert die Daten nicht, die übertragen werden.

Der OMS-Agent muss auf dem Computer installiert werden, auf dem auch das Gateway installiert ist. Dadurch können Sie das OMS-Gateway überwachen und die Leistungs- oder Ereignisdaten für den Server analysieren, auf dem es installiert ist. Außerdem unterstützt der Agent das OMS-Gateway beim Ermitteln der Dienstendpunkte, mit denen es kommunizieren muss.

Das Gateway muss Zugriff auf das Internet haben, um Daten zur OMS hochzuladen. Jeder Agent benötigt außerdem auch Netzwerkkonnektivität zu seinen Gateway, damit Agents automatisch Daten zu und vom Gateway übertragen können. Für ein optimales Ergebnis installieren Sie das Gateway auf einem Computer, der auch als Domänencontroller dient.

Das folgende Diagramm veranschaulicht den Datenfluss von Direkt-Agents zur OMS.

![Direkt-Agent-Diagramm](./media/log-analytics-oms-gateway/direct-agent-diagram.png)

Das folgende Diagramm veranschaulicht den Datenfluss von Operations Manager zur OMS.

![Operations Manager-Diagramm](./media/log-analytics-oms-gateway/scom-mgt-server.png)

## <a name="language-availability"></a>Verfügbare Sprachen

Das OMS-Gateway ist in folgenden Sprachen verfügbar:

- Chinesisch (vereinfacht)
- Chinesisch (traditionell)
- Tschechisch
- Niederländisch
- Deutsch
- Französisch
- Deutsch
- Ungarisch
- Italienisch
- Japanisch
- Koreanisch
- Polnisch
- Portugiesisch (Brasilien)
- Portugiesisch (Portugal)
- Russisch
- Spanisch (international)

## <a name="download-the-oms-gateway"></a>Herunterladen des OMS-Gateways

Die Setupdatei für das OMS-Gateway kann auf drei Arten bezogen werden:

### <a name="microsoft-download-center"></a>Microsoft Download Center

- Laden Sie die neueste Version des OMS-Gateways vom [Microsoft Download Center](http://download.microsoft.com/download/2/5/C/25CF992A-0347-4765-BD7D-D45D5B27F92C/OMS%20Gateway.msi) herunter.

### <a name="oms-portal"></a>OMS-Portal

1.    Melden Sie sich bei Ihrem OMS-Arbeitsbereich an.
2.    Wählen Sie **Einstellungen** > **Verbundene Quellen** > **Windows-Server** aus.
3.    Klicken Sie auf **Download OMS Gateway** (OMS-Gateway herunterladen).


### <a name="azure-portal"></a>Azure-Portal

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com), und melden Sie sich an. Durchsuchen Sie die Liste mit den Diensten, und wählen Sie **Log Analytics** aus.
2. Wählen Sie einen Arbeitsbereich aus.
3. Klicken Sie auf dem Blatt für Ihren Arbeitsbereich unter **Allgemein** auf **Schnellstart**.
4. Klicken Sie unter **Choose a data source to connect to the workspace** (Datenquelle auswählen, die mit dem Arbeitsbereich verbunden werden soll) auf **Computer**.
4. Klicken Sie auf dem Blatt **Direct Agent** auf **Download OMS Gateway** (OMS-Gateway herunterladen).  
    ![Download OMS Gateway](./media/log-analytics-oms-gateway/download-gateway.png) (OMS-Gateway herunterladen)


## <a name="install-the-oms-gateway"></a>Installieren des OMS-Gateways
Durch Installieren des Gateways werden frühere Versionen des Gateways, die Sie installiert haben, ersetzt (Log Analytics Forwarder).

Voraussetzungen: .Net Framework 4.5, Windows Server 2012 R2 SP1 und höher


1. Starten Sie die Installation, indem Sie auf **OMS Gateway.msi** doppelklicken.
2. Klicken Sie auf der Seite Willkommen auf **Weiter**.  
    ![Gateway-Setup-Assistent](./media/log-analytics-oms-gateway/gateway-wizard01.png)
3. Wählen Sie auf der Seite Lizenzvertrag **Ich akzeptiere die Bestimmungen des Lizenzvertrags**, um den Lizenzbestimmungen zuzustimmen, und anschließend **Weiter** aus.
4. Auf der Port- und Proxy-Adressseite:
   1. Geben Sie die für das Gateway zu verwendende TCP-Portnummer ein. Setup öffnet diese Portnummer über die Windows-Firewall. Der Standardwert ist 8080.
      Der gültige Bereich der Portnummer ist 1 bis 65535. Wenn die Eingabe außerhalb dieses Bereichs liegt, wird eine Fehlermeldung angezeigt.
   2. Falls der Server, auf dem das Gateway installiert ist, einen Proxy verwenden muss, geben Sie die Proxyadresse ein, an der das Gateway eine Verbindung herstellen muss. Beispiel `http://myorgname.corp.contoso.com:80`. Wenn leer,versucht das Gateway, eine direkte Verbindung mit dem Internet herzustellen. Andernfalls stellt das Gateway eine Verbindung mit dem Proxy her. Wenn der Proxyserver eine Authentifizierung erfordert, geben Sie den Benutzernamen und das Kennwort ein.  
       ![Gateway-Assistent-Proxykonfiguration](./media/log-analytics-oms-gateway/gateway-wizard02.png)  
   3. Klicken Sie auf **Weiter**
5. Wenn Sie Microsoft-Updates nicht aktiviert haben, wird die Seite „Microsoft-Update“ angezeigt, auf der Sie angeben können, dass Microsoft-Updates aktiviert werden sollen. Treffen Sie eine Auswahl, und klicken Sie dann auf **Weiter**. Fahren Sie andernfalls mit dem nächsten Schritt fort.
6. Übernehmen Sie auf der Seite „Zielordner“ den Standardordner „c:\ProgramFiles\OMS Gateway“, oder geben Sie den Speicherort zum Installieren des Gateways ein, und klicken Sie dann auf **Weiter**.
7. Klicken Sie auf der Seite „Bereit zur Installation“ auf **Installieren**. Möglicherweise wird eine Benutzerkontensteuerung angezeigt, die die Berechtigung zum Installieren anfordert. Ist dies der Fall, klicken Sie auf **Ja**.
8. Nachdem Setup abgeschlossen ist, klicken Sie auf **Fertig stellen**. Sie können überprüfen, ob der Dienst ausgeführt wird, indem Sie das Snap-In services.msc öffnen und überprüfen, ob **OMS-Gateway** in der Liste der Dienste angezeigt wird.  
    ![Dienste – OMS-Gateway](./media/log-analytics-oms-gateway/gateway-service.png)

## <a name="install-an-agent-on-devices"></a>Installieren eines Agents auf Geräten
Informationen über das Installieren direkt verbundener Agents finden Sie bei Bedarf unter [Verbinden von Windows-Computern mit Log Analytics](log-analytics-windows-agents.md). Dieser Artikel beschreibt, wie Sie den Agent mithilfe eines Setup-Assistenten oder über die Befehlszeile installieren können.

## <a name="configure-oms-agents"></a>Konfigurieren von OMS-Agents
Informationen über das Konfigurieren eines Agents, sodass er einen Proxyserver – in diesem Fall das Gateway – verwendet, finden Sie unter [Konfigurieren von Proxy- und Firewalleinstellungen mit dem Microsoft Monitoring Agent](log-analytics-proxy-firewall.md).

Operations Manager-Agents senden einige Daten, z. B. Operations Manager-Warnungen, Konfigurationsbewertung, Instance Space und Kapazitätsdaten, über den Verwaltungsserver. Andere hohe Datenvolumen, z. B. IIS-Protokolle, Leistung und Sicherheit, werden direkt an das OMS-Gateway gesendet. Unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) finden Sie eine vollständige Liste der Daten, die über jeden Kanal gesendet werden.

> [!NOTE]
> Wenn Sie das Gateway mit Netzwerklastenausgleich verwenden möchten, lesen Sie [Optionally configure network load balancing (Optionales Konfigurieren des Netzwerklastenausgleichs)](#optionally-configure-network-load-balancing).
>
>

## <a name="configure-a-scom-proxy-server"></a>Konfigurieren eines SCOM-Proxyservers
Sie können Operations Manager so konfigurieren, dass das Gateway hinzugefügt wird und als Proxyserver fungiert. Wenn Sie die Proxykonfiguration aktualisieren, wird die Proxykonfiguration automatisch auf alle Agents angewendet, die an Operations Manager melden.

Um das Gateway zur Unterstützung von Operations Manager zu verwenden, benötigen Sie:

* Microsoft Monitoring Agent (Agentversion – **8.0.10900.0** und höher) auf dem Gateway-Server installiert und für die OMS-Arbeitsbereiche konfiguriert, mit denen Sie kommunizieren möchten.
* Das Gateway benötigt Internetkonnektivität oder muss mit einem Proxyserver verbunden sein, bei dem dies der Fall ist.

### <a name="to-configure-scom-for-the-gateway"></a>So konfigurieren Sie SCOM für das Gateway
1. Öffnen Sie die Operations Manager-Konsole, und klicken Sie unter **Operations Management Suite**, auf **Verbindung** und dann auf **Proxyserver konfigurieren**:  
    ![Operations Manager – Proxyserver konfigurieren](./media/log-analytics-oms-gateway/scom01.png)
2. Wählen Sie **Proxyserver für Zugriff auf den Operations Management Suite-Dienst verwenden** aus, und geben Sie dann die IP-Adresse des OMS-Gatewayservers ein. Stellen Sie sicher, dass Sie mir dem Präfix `http://` beginnen:  
    ![Operations Manager – Proxyserveradresse](./media/log-analytics-oms-gateway/scom02.png)
3. Klicken Sie auf **Fertig stellen**. Der Operations Manager-Server ist mit dem OMS-Arbeitsbereich verbunden.

## <a name="configure-network-load-balancing"></a>Konfigurieren des Netzwerklastenausgleichs
Sie können das Gateway für hohe Verfügbarkeit mithilfe des Netzwerklastenausgleichs konfigurieren, indem Sie einen Cluster erstellen. Der Cluster verwaltet Datenverkehr von den Agents, indem die angeforderten Verbindungen von den Microsoft Monitoring-Agents knotenübergreifend umgeleitet werden. Wenn ein Gatewayserver ausfällt, wird der Datenverkehr zu anderen Knoten umgeleitet.

1. Öffnen Sie den Netzwerklastenausgleich-Manager, und erstellen Sie einen Cluster.
2. Klicken Sie mit der rechten Maustaste auf den Cluster, bevor Sie Gateways hinzufügen, und wählen Sie **Clustereigenschaften** aus. Konfigurieren Sie den Cluster so, dass er über eine eigene IP-Adresse verfügt:  
    ![Netzwerklastenausgleich-Manager – Cluster-IP-Adressen](./media/log-analytics-oms-gateway/nlb01.png)
3. Um einen OMS-Gatewayserver mit dem installierten Microsoft Monitoring Agent zu verbinden, klicken Sie mit der rechten Maustaste auf die IP-Adresse des Clusters, und klicken Sie dann auf **Host dem Cluster hinzufügen**.  
    ![Netzwerklastenausgleich-Manager – Host dem Cluster hinzufügen](./media/log-analytics-oms-gateway/nlb02.png)
4. Geben Sie die IP-Adresse des Servers des Gateways ein, mit dem Sie eine Verbindung herstellen möchten:  
    ![Netzwerklastenausgleich-Manager – Host dem Cluster hinzufügen: Verbinden](./media/log-analytics-oms-gateway/nlb03.png)
5. Auf Computern, die nicht über Internetkonnektivität verfügen, müssen Sie die IP-Adresse des Clusters verwenden, wenn Sie die **Microsoft Monitoring Agenteigenschaften** konfigurieren:  
    ![Microsoft Monitoring Agenteigenschaften – Proxyeinstellungen](./media/log-analytics-oms-gateway/nlb04.png)

## <a name="configure-for-automation-hybrid-workers"></a>Konfigurieren für Automation Hybrid Worker
Wenn Sie Automation Hybrid Worker in Ihrer Umgebung haben, werden von den folgenden Schritten manuelle, temporäre Problemumgehungen bereitgestellt, um das Gateway für deren Unterstützung zu konfigurieren.

In den folgenden Schritten müssen Sie die Azure-Region kennen, in der sich das Automation-Konto befindet. So finden Sie den Speicherort

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie den Azure Automation-Dienst.
3. Wählen Sie das entsprechende Azure Automation-Konto aus.
4. Zeigen Sie seine Region unter **Speicherort** an.  
    ![Azure-Portal – Speicherort des Automation-Kontos](./media/log-analytics-oms-gateway/location.png)

Verwenden Sie die folgenden Tabellen, um die URL für jeden Speicherort zu identifizieren:

**Auftragslaufzeit-Datendienst-URLs**

| **location** | **URL** |
| --- | --- |
| USA (Mitte/Norden) |ncus-jobruntimedata-prod-su1.azure-automation.net |
| Europa, Westen |we-jobruntimedata-prod-su1.azure-automation.net |
| USA, Süden-Mitte |scus-jobruntimedata-prod-su1.azure-automation.net |
| USA (Ost) 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Kanada, Mitte |cc-jobruntimedata-prod-su1.azure-automation.net |
| Nordeuropa |ne-jobruntimedata-prod-su1.azure-automation.net |
| Südostasien |sea-jobruntimedata-prod-su1.azure-automation.net |
| Indien (Mitte) |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japan |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Australien |ase-jobruntimedata-prod-su1.azure-automation.net |

**Agent-Dienst-URLs**

| **location** | **URL** |
| --- | --- |
| USA (Mitte/Norden) |ncus-agentservice-prod-1.azure-automation.net |
| Europa, Westen |we-agentservice-prod-1.azure-automation.net |
| USA, Süden-Mitte |scus-agentservice-prod-1.azure-automation.net |
| USA (Ost) 2 |eus2-agentservice-prod-1.azure-automation.net |
| Kanada, Mitte |cc-agentservice-prod-1.azure-automation.net |
| Nordeuropa |ne-agentservice-prod-1.azure-automation.net |
| Südostasien |sea-agentservice-prod-1.azure-automation.net |
| Indien (Mitte) |cid-agentservice-prod-1.azure-automation.net |
| Japan |jpe-agentservice-prod-1.azure-automation.net |
| Australien |ase-agentservice-prod-1.azure-automation.net |

Wenn Ihr Computer automatisch als Hybrid Worker für das Patchen mithilfe der Update Management-Lösung registriert ist, gehen Sie wie folgt vor:

1. Fügen Sie die Auftragslaufzeit-Datendienst-URLs der Liste zulässiger Hosts auf dem OMS-Gateway hinzu. Beispiel:  `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. Starten Sie den OMS-Gatewaydienst mithilfe des folgenden PowerShell Cmdlets neu: `Restart-Service OMSGatewayService`

Wenn das Onboarding Ihres Computers bei Azure Automation mithilfe des Hybrid Worker-Registrierungs-Cmdlets durchgeführt wird, gehen Sie wie folgt vor:

1. Fügen Sie die Agent-Dienstregistrierungs-URL der Liste zulässiger Hosts auf dem OMS-Gateway hinzu. Beispiel: `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. Fügen Sie die Auftragslaufzeit-Datendienst-URLs der Liste zulässiger Hosts auf dem OMS-Gateway hinzu. Beispiel:  `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. Starten Sie den OMS-Gatewaydienst neu.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Nützliche PowerShell-cmdlets
Cmdlets können Ihnen helfen, Aufgaben durchzuführen, die für die Aktualisierung der Konfigurationseinstellungen des OMS-Gateways erforderlich sind. Bevor Sie sie verwenden, müssen folgende Schritte durchgeführt worden sein:

1. Installieren des OMS-Gateways (MSI).
2. Öffnen des Azure PowerShell-Fensters.
3. Geben Sie zum Importieren des Moduls folgenden Befehl ein: `Import-Module OMSGateway`
4. Wenn im vorherigen Schritt kein Fehler aufgetreten ist, wurde das Modul erfolgreich importiert, und die Cmdlets können verwendet werden. Geben Sie `Get-Module OMSGateway` ein
5. Nachdem Sie die Cmdlets geändert haben, stellen Sie sicher, dass Sie den Gatewaydienst neu starten.

Wenn in Schritt 3 ein Fehler auftritt, wurde das Modul nicht importiert. Der Fehler kann auftreten, wenn PowerShell das Modul nicht finden kann. Sie finden es im Gateway-Installationspfad: C:\Programme\Microsoft OMS Gateway\PowerShell.

| **Cmdlet** | **Parameter** | **Beschreibung** | **Beispiele** |
| --- | --- | --- | --- |
| `Set-OMSGatewayConfig` |Schlüssel (erforderlich) <br> Wert |Ändert die Konfiguration des Diensts |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |
| `Get-OMSGatewayConfig` |Schlüssel |Ruft die Konfiguration des Diensts ab |`Get-OMSGatewayConfig` <br> <br> `Get-OMSGatewayConfig -Name ListenPort` |
| `Set-OMSGatewayRelayProxy` |Adresse <br> Benutzername <br> Kennwort |Legt die Adresse (und Anmeldeinformationen) des Relays (Upstreamproxy) fest |1. Legen Sie ein Antwortproxy und die Anmeldeinformationen fest: `Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080 -Username user1 -Password 123` <br> <br> 2. Legen Sie ein Antwortproxy fest, das keine Authentifizierung voraussetzt: `Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080` <br> <br> 3. Deaktivieren Sie die Antwortproxyeinstellung, d. h. ein Antwortproxy ist nicht erforderlich: `Set-OMSGatewayRelayProxy -Address ""` |
| `Get-OMSGatewayRelayProxy` | |Ruft die Adresse des Relays (Upstreamproxy) ab |`Get-OMSGatewayRelayProxy` |
| `Add-OMSGatewayAllowedHost` |Host (erforderlich) |Fügt den Host der Liste zulässiger Hosts hinzu |`Add-OMSGatewayAllowedHost -Host www.test.com` |
| `Remove-OMSGatewayAllowedHost` |Host (erforderlich) |Entfernt den Host aus der Liste zulässiger Hosts |`Remove-OMSGatewayAllowedHost -Host www.test.com` |
| `Get-OMSGatewayAllowedHost` | |Ruft den derzeit zulässigen Host ab (nur lokal konfigurierter zulässiger Host, schließen Sie automatisch heruntergeladene zulässige Hosts nicht ein) |`Get-OMSGatewayAllowedHost` |
| `Add-OMSGatewayAllowedClientCertificate` |Antragsteller (erforderlich) |Fügt den Clientzertifikatantragsteller der Liste zulässiger Hosts hinzu |`Add-OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Remove-OMSGatewayAllowedClientCertificate` |Antragsteller (erforderlich) |Entfernt den Clientzertifikatantragsteller aus der Liste zulässiger Hosts |`Remove- OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Get-OMSGatewayAllowedClientCertificate` | |Ruft die derzeit zulässigen Clientzertifikatantragsteller ab (nur lokal konfigurierte zulässiger Antragsteller, schließen Sie automatisch heruntergeladene zulässige Antragsteller nicht ein) |`Get-OMSGatewayAllowedClientCertificate` |

## <a name="troubleshoot"></a>Problembehandlung
Installieren Sie den OMS-Agent auf Computern, auf denen das Gateway installiert ist. Sie können mithilfe des Agents die Ereignisse sammeln, die vom Gateway protokolliert werden.

![Ereignisanzeige – OMS-Gatewayprotokoll](./media/log-analytics-oms-gateway/event-viewer.png)

**OMS-Gatewayereignis-IDs und Beschreibungen**

Die folgende Tabelle enthält die Ereignis-IDs und Beschreibungen für OMS Gatewayprotokollereignisse.

| **ID** | **Beschreibung** |
| --- | --- |
| 400 |Alle Anwendungsfehler, die keine bestimmte ID haben |
| 401 |Falsche Konfiguration. Beispiel: listenPort = "text" anstelle eines Integers |
| 402 |Fehler bei der Analyse von TLS-Handshakenachrichten |
| 403 |Netzwerkfehler. Beispiel: Verbindung mit dem Zielserver kann nicht hergestellt werden |
| 100 |Allgemeine Informationen |
| 101 |Dienst wurde gestartet |
| 102 |Dienst wurde beendet |
| 103 |HTTP CONNECT-Befehl vom Client empfangen |
| 104 |Kein HTTP CONNECT-Befehl |
| 105 |Zielserver ist nicht in der Liste zulässiger Elemente enthalten, oder der Zielport ist kein sicherer Port (443) <br> <br> Stellen Sie sicher, dass der MMA-Agent auf dem Gatewayserver und die Agents, die mit dem Gateway kommunizieren, mit dem gleichen Log Analytics-Arbeitsbereich verbunden sind. |
| 105 |FEHLER TcpConnection – Ungültiges Clientzertifikat: CN=Gateway <br><br> Stellen Sie Folgendes sicher: <br>    <br> & #149; Sie verwenden ein Gateway mit der Versionsnummer 1.0.395.0 oder höher. <br> &#149; Der MMA-Agent auf dem Gatewayserver und die Agents, die mit dem Gateway kommunizieren, sind mit dem gleichen Log Analytics-Arbeitsbereich verbunden. |
| 106 |Beliebiger Grund dafür, dass die TLS-Sitzung verdächtig ist und abgelehnt wird |
| 107 |Die TLS-Sitzung wurde überprüft |

**Zu sammelnde Leistungsindikatoren**

Die folgende Tabelle zeigt die verfügbaren Leistungsindikatoren für das OMS-Gateway. Sie können die Indikatoren mit dem Systemmonitor hinzufügen.

| **Name** | **Beschreibung** |
| --- | --- |
| OMS-Gateway/Aktive Clientverbindung |Anzahl der aktiven Clientnetzwerkverbindungen (TCP) |
| OMS-Gateway/Fehleranzahl |Anzahl von Fehlern |
| OMS-Gateway/Verbundener Client |Anzahl verbundener Clients |
| OMS-Gateway/Ablehnungsanzahl |Anzahl von Ablehnungen aufgrund von TLS-Überprüfungsfehlern |

![OMS-Gateway-Leistungsindikatoren](./media/log-analytics-oms-gateway/counters.png)

## <a name="get-assistance"></a>Unterstützung
Wenn Sie sich am Azure-Portal angemeldet haben, können Sie eine Anfrage zwecks Unterstützung im Zusammenhang mit dem OMS-Gateway oder anderen Azure-Diensten oder Leistungsmerkmalen eines Diensts erstellen.
Um Unterstützung anzufordern, klicken Sie auf das Fragezeichen in der oberen rechten Ecke des Portals, und klicken Sie dann auf **Neue Supportanfrage**. Füllen Sie das neue Formular für die Supportanfrage aus.

![Neue Supportanfrage](./media/log-analytics-oms-gateway/support.png)

Sie können auch Feedback zur OMS oder Log Analytics im [Microsoft Azure-Feedbackforum](https://feedback.azure.com/forums/267889) hinterlassen.

## <a name="next-steps"></a>Nächste Schritte
* [Fügen Sie Datenquellen hinzu](log-analytics-data-sources.md), um Daten aus verbundenen Quellen im OMS-Arbeitsbereich zu sammeln und im OMS-Repository zu speichern.

