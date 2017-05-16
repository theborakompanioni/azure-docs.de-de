---
title: "Verbinden von Computern mit der OMS über das OMS-Gateway | Microsoft-Dokumentation"
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
ms.date: 04/20/2017
ms.author: magoedte;banders
ms.translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: ce2f9311775389366c66323070254f721f0896ab
ms.contentlocale: de-de
ms.lasthandoff: 04/22/2017

---

# <a name="connect-computers-without-internet-access-to-oms-using-the-oms-gateway"></a>Verbinden von Computern ohne Internetzugriff mit der OMS über das OMS-Gateway

In diesem Dokument erfahren Sie, wie Ihre von OMS verwalteten und von System Center Operations Manager überwachten Computer ohne Internetzugriff Daten an den OMS-Dienst senden können. Beim OMS-Gateway handelt es sich um einen HTTP-Weiterleitungsproxy, der HTTP-Tunnel mit dem Befehl „HTTP CONNECT“ unterstützt. Das Gateway kann Daten sammeln und im Auftrag der Computer an den OMS senden.  

Das OMS-Gateway unterstützt Folgendes:

* Azure Automation – Hybrid-Runbook-Worker  
* Direkt mit einem OMS-Arbeitsbereich verbundene Windows-Computer mit Microsoft Monitoring Agent
* Linux-Computer, bei denen der OMS-Agent für Linux direkt mit einem OMS-Arbeitsbereich verbunden ist  
* System Center Operations Manager 2012 SP1 mit UR7, Operations Manager 2012 R2 mit UR3 oder Operations Manager 2016-Verwaltungsgruppe mit OMS-Integration  

Falls Computer in Ihrem Netzwerk aufgrund von IT-Sicherheitsrichtlinien keine Verbindung mit dem Internet herstellen können (etwa im Fall von POS-Geräten oder von Servern, die IT-Dienste unterstützen) und Sie zu deren Verwaltung und Überwachung eine Verbindung mit OMS herstellen möchten, können sie so konfiguriert werden, dass sie direkt mit dem OMS-Gateway kommunizieren, um in ihrem Auftrag Konfigurations- und Weiterleitungsdaten zu empfangen.  Wenn diese Computer mit dem OMS-Agent konfiguriert werden, um eine direkte Verbindung mit einem OMS-Arbeitsbereich herzustellen, kommunizieren alle Computer stattdessen mit dem OMS-Gateway.  Das Gateway überträgt Daten von den Agents direkt an OMS, ohne sie bei der Übertragung zu analysieren.

Wenn eine Operations Manager-Verwaltungsgruppe in OMS integriert wird, können die Verwaltungsserver so konfiguriert werden, dass sie eine Verbindung mit dem OMS-Gateway herstellen, um Konfigurationsinformationen zu empfangen und gesammelte Daten abhängig von der aktivierten Lösung zu senden.  Operations Manager-Agents senden einige Daten wie etwa Operations Manager-Warnungen, Konfigurationsbewertung, Speicherplatz der Instanz und Kapazitätsdaten an den Verwaltungsserver. Andere umfangreiche Daten wie etwa IIS-Protokolle, Leistungsinformationen und Sicherheitsereignisse werden direkt an das OMS-Gateway gesendet.  Falls Sie zur Überwachung nicht vertrauenswürdiger Systeme einen oder mehrere Operations Manager-Gatewayserver in einer DMZ oder in einem anderen isolierten Netzwerk bereitgestellt haben, können diese nicht mit einem OMS-Gateway kommunizieren.  Operations Manager-Gatewayservers können nur mit einem Verwaltungsserver kommunizieren.  Wenn eine Operations Manager-Verwaltungsgruppe für die Kommunikation mit dem OMS-Gateway konfiguriert ist, werden die Proxykonfigurationsinformationen automatisch an alle per Agent verwalteten Computer verteilt, die zum Sammeln von Daten für Log Analytics konfiguriert sind. Das gilt auch, wenn die Einstellung leer ist.    

Zur Gewährleistung der hohen Verfügbarkeit für direkt verbundene Gruppen oder Operations Management-Gruppen, die über das Gateway mit OMS kommunizieren, können Sie den Datenverkehr mithilfe des Netzwerklastenausgleichs umleiten und auf mehrere Gatewayserver verteilen.  Wenn ein Gatewayserver ausfällt, wird der Datenverkehr an einen anderen verfügbaren Knoten umgeleitet.  

Es wird empfohlen, den OMS-Agent auf dem Computer zu installieren, auf dem die OMS-Gatewaysoftware ausgeführt wird, um das OMS-Gateway zu überwachen und die Leistungs- oder Ereignisdaten zu analysieren. Außerdem unterstützt der Agent das OMS-Gateway beim Ermitteln der Dienstendpunkte, mit denen es kommunizieren muss.

Jeder Agent benötigt eine Netzwerkverbindung mit dem entsprechenden Gateway, damit Agents automatisch Daten an das und vom Gateway übertragen können. Das Gateway sollte nicht auf einem Domänencontroller installiert werden.

Das folgende Diagramm zeigt den Datenfluss von direkten Agents an OMS unter Verwendung des Gatewayservers.  In der Proxykonfiguration der Agents muss der Port verwendet, mit dem das OMS-Gateway für die Kommunikation mit OMS konfiguriert ist.  

![Diagramm für die Kommunikation direkter Agents mit OMS](./media/log-analytics-oms-gateway/oms-omsgateway-agentdirectconnect.png)

Das folgende Diagramm zeigt den Datenfluss von einer Operations Manager-Verwaltungsgruppe an OMS.   

![Diagramm für die Kommunikation von Operations Manager mit OMS](./media/log-analytics-oms-gateway/oms-omsgateway-opsmgrconnect.png)

## <a name="prerequisites"></a>Voraussetzungen

Ein Computer, der für die Ausführung des OMS-Gateways vorgesehen ist, muss über Folgendes verfügen:

* Windows 10, Windows 8.1, Windows 7
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008
* .NET Framework 4.5
* Vierkernprozessor und 8 GB Arbeitsspeicher (mindestens) 

### <a name="language-availability"></a>Verfügbare Sprachen

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

Die neueste Version der Setupdatei für das OMS-Gateway kann auf drei Arten bezogen werden:

1. Herunterladen über das [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=54443).

2. Herunterladen über das OMS-Portal.  Navigieren Sie nach der Anmeldung bei Ihrem OMS-Arbeitsbereich zu **Einstellungen** > **Verbundene Quellen** > **Windows-Server**, und klicken Sie auf **OMS-Gateway herunterladen**.

3. Herunterladen über das [Azure-Portal](https://portal.azure.com).  Gehen Sie nach der Anmeldung wie folgt vor:  

   1. Durchsuchen Sie die Liste mit den Diensten, und wählen Sie **Log Analytics** aus.  
   2. Wählen Sie einen Arbeitsbereich aus.
   3. Klicken Sie auf dem Blatt für Ihren Arbeitsbereich unter **Allgemein** auf **Schnellstart**.
   4. Klicken Sie unter **Choose a data source to connect to the workspace** (Datenquelle auswählen, die mit dem Arbeitsbereich verbunden werden soll) auf **Computer**.
   5. Klicken Sie auf dem Blatt **Direct Agent** auf **Download OMS Gateway** (OMS-Gateway herunterladen).<br><br> ![Download OMS Gateway](./media/log-analytics-oms-gateway/download-gateway.png) (OMS-Gateway herunterladen)


## <a name="install-the-oms-gateway"></a>Installieren des OMS-Gateways

Führen Sie die folgenden Schritte aus, um ein Gateway zu installieren.  Falls eine ältere Version (ehemals *Log Analytics Forwarder*) installiert ist, wird ein Upgrade auf die aktuelle Version ausgeführt.  

1. Doppelklicken Sie im Zielordner auf **OMS Gateway.msi**.
2. Klicken Sie auf der Seite **Willkommen**auf **Weiter**.<br><br> ![Gateway-Setup-Assistent](./media/log-analytics-oms-gateway/gateway-wizard01.png)<br> 
3. Wählen Sie auf der Seite **Lizenzvertrag** die Option **Ich stimme den Bedingungen des Lizenzvertrags zu.** aus, um den Lizenzbestimmungen zuzustimmen, und wählen Sie anschließend **Weiter** aus.
4. Gehen Sie auf der Seite für die Port- und Proxyadresse**** wie folgt vor:
   1. Geben Sie die für das Gateway zu verwendende TCP-Portnummer ein. Das Setup konfiguriert eine Eingangsregel mit dieser Portnummer für die Windows-Firewall.  Der Standardwert ist 8080.
      Der gültige Bereich der Portnummer ist 1 bis 65535. Wenn die Eingabe außerhalb dieses Bereichs liegt, wird eine Fehlermeldung angezeigt.
   2. Optional: Falls der Server, auf dem das Gateway installiert ist, über einen Proxy kommunizieren muss, geben Sie die Proxyadresse ein, mit der das Gateway eine Verbindung herstellen muss. Beispiel: `http://myorgname.corp.contoso.com:80`.  Ohne Angabe versucht das Gateway, eine direkte Verbindung mit dem Internet herzustellen.  Falls der Proxyserver eine Authentifizierung erfordert, geben Sie einen Benutzernamen und ein Kennwort ein.<br><br> ![Gateway-Assistent-Proxykonfiguration](./media/log-analytics-oms-gateway/gateway-wizard02.png)<br>   
   3. Klicken Sie auf **Weiter**.
5. Falls Microsoft Update nicht aktiviert ist, wird die Seite „Microsoft Update“ angezeigt, auf der Sie Microsoft Update aktivieren können. Treffen Sie eine Auswahl, und klicken Sie dann auf **Weiter**. Fahren Sie andernfalls mit dem nächsten Schritt fort.
6. Übernehmen Sie auf der Seite **Zielordner** entweder den Standardordner „C:\Programme\OMS Gateway“, oder geben Sie den Speicherort zum Installieren des Gateways ein, und klicken Sie anschließend auf **Weiter**.
7. Klicken Sie auf der Seite **Bereit zur Installation** auf **Installieren**. Unter Umständen fordert die Benutzerkontensteuerung eine Installationsberechtigung an. Ist dies der Fall, klicken Sie auf **Ja**.
8. Nachdem Setup abgeschlossen ist, klicken Sie auf **Fertig stellen**. Sie können überprüfen, ob der Dienst ausgeführt wird. Öffnen Sie dazu das Snap-In „services.msc“, und überprüfen Sie, ob **OMS-Gateway** in der Liste mit den Diensten angezeigt wird und den Status **Wird ausgeführt** besitzt.<br><br> ![Dienste – OMS-Gateway](./media/log-analytics-oms-gateway/gateway-service.png)  

## <a name="configure-network-load-balancing"></a>Konfigurieren des Netzwerklastenausgleichs 
Für hohe Verfügbarkeit können Sie das Gateway mit Netzwerklastenausgleich (Network Load Balancing, NLB) konfigurieren. Hierzu können Sie entweder den Microsoft-Netzwerklastenausgleich (NLB) oder einen hardwarebasierten Lastenausgleich verwenden.  Der Lastenausgleich leitet die angeforderten Verbindungen von OMS-Agents oder Operations Manager-Verwaltungsservern zur Verwaltung des Datenverkehrs für alle Knoten um. Wenn ein Gatewayserver ausfällt, wird der Datenverkehr zu anderen Knoten umgeleitet.

Informationen zum Entwerfen und Bereitstellen eines Netzwerklastenausgleichs-Clusters unter Windows Server 2016 finden Sie unter [Netzwerklastenausgleich](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  Die folgenden Schritte beschreiben, wie Sie einen Cluster mit Microsoft-Netzwerklastenausgleich konfigurieren.  

1.  Melden Sie sich mit einem Administratorkonto bei dem Windows-Server an, der dem NLB-Cluster angehört.  
2.  Öffnen Sie im Server-Manager den Netzwerklastenausgleich-Manager, klicken Sie auf **Tools**, und klicken Sie anschließend auf **Netzwerklastenausgleich-Manager**.
3. Um einen OMS-Gatewayserver mit dem installierten Microsoft Monitoring Agent zu verbinden, klicken Sie mit der rechten Maustaste auf die IP-Adresse des Clusters, und klicken Sie dann auf **Host dem Cluster hinzufügen**.<br><br> ![Netzwerklastenausgleich-Manager – Host dem Cluster hinzufügen](./media/log-analytics-oms-gateway/nlb02.png)<br> 
4. Geben Sie die IP-Adresse des Gatewayservers ein, mit dem Sie eine Verbindung herstellen möchten.<br><br> ![Netzwerklastenausgleich-Manager – Host dem Cluster hinzufügen: Verbinden](./media/log-analytics-oms-gateway/nlb03.png) 
    
## <a name="configure-oms-agent-and-operations-manager-management-group"></a>Konfigurieren von OMS-Agent und Operations Manager-Verwaltungsgruppe
Im folgenden Abschnitt erfahren Sie, wie Sie direkt verbundene OMS-Agents, eine Operations Manager-Verwaltungsgruppe oder Azure Automation Hybrid Runbook Worker mit dem OMS-Gateway konfigurieren, um die Kommunikation mit OMS zu ermöglichen.  

Informationen zu den Anforderungen und Schritten zum Installieren des OMS-Agents auf Windows-Computern mit OMS-Direktverbindung finden Sie unter [Verbinden von Windows-Computern mit dem Log Analytics-Dienst in Azure](log-analytics-windows-agents.md). Entsprechende Informationen für Linux-Computer finden Sie unter [Verbinden Ihrer Linux-Computer mit Log Analytics](log-analytics-linux-agents.md). 

### <a name="configuring-the-oms-agent-and-operations-manager-to-use-the-oms-gateway-as-a-proxy-server"></a>Konfigurieren von OMS-Agent und Operations Manager für die Verwendung des OMS-Gateways als Proxyserver

### <a name="configure-standalone-oms-agent"></a>Konfigurieren eines eigenständigen OMS-Agents
Informationen zum Konfigurieren eines Agents für die Verwendung eines Proxyservers (in diesem Fall: das Gateway) finden Sie unter [Konfigurieren von Proxy- und Firewalleinstellungen mit dem Microsoft Monitoring Agent](log-analytics-proxy-firewall.md).  Wenn Sie mehrere Gatewayserver hinter einem Netzwerklastenausgleich bereitgestellt haben, handelt es sich bei der OMS-Agent-Proxykonfiguration um die virtuelle IP-Adresse des NLBs:<br><br> ![Microsoft Monitoring Agenteigenschaften – Proxyeinstellungen](./media/log-analytics-oms-gateway/nlb04.png)

### <a name="configure-operations-manager---all-agents-use-the-same-proxy-server"></a>Konfigurieren von Operations Manager (alle Agents verwenden den gleichen Proxyserver)
Sie konfigurieren Operations Manager, um den Gatewayserver hinzuzufügen.  Die Operations Manager-Proxykonfiguration wird automatisch auf alle Agents angewendet, die Operations Manager unterstellt sind. Das gilt auch, wenn die Einstellung leer ist.

Voraussetzungen für die Unterstützung von Operations Manager mithilfe des Gateways:

* Microsoft Monitoring Agent (Agentversion – **8.0.10900.0** und höher) auf dem Gateway-Server installiert und für die OMS-Arbeitsbereiche konfiguriert, mit denen Sie kommunizieren möchten.
* Das Gateway benötigt Internetkonnektivität oder muss mit einem Proxyserver verbunden sein, bei dem dies der Fall ist.

> [!NOTE]
> Wenn Sie keinen Wert für das Gateway angeben, werden mithilfe von Push leere Werte an alle Agents übertragen.


1. Öffnen Sie die Operations Manager-Konsole, und klicken Sie unter **Operations Management Suite**, auf **Verbindung** und dann auf **Proxyserver konfigurieren**.<br><br> ![Operations Manager – Proxyserver konfigurieren](./media/log-analytics-oms-gateway/scom01.png)<br> 
2. Wählen Sie **Proxyserver für Zugriff auf den Operations Management Suite-Dienst verwenden** aus, und geben Sie die IP-Adresse des OMS-Gatewayservers oder die virtuelle IP-Adresse des NLBs ein. Beginnen Sie mit dem Präfix `http://`.<br><br> ![Operations Manager – Proxyserveradresse](./media/log-analytics-oms-gateway/scom02.png)<br> 
3. Klicken Sie auf **Fertig stellen**. Der Operations Manager-Server ist mit dem OMS-Arbeitsbereich verbunden.

### <a name="configure-operations-manager---specific-agents-use-proxy-server"></a>Konfigurieren von Operations Manager (bestimmte Agents verwenden einen Proxyserver)
Bei großen oder komplexen Umgebungen soll der OMS-Gatewayserver unter Umständen nur von bestimmten Servern (oder Gruppen) verwendet werden.  Für diese Server können Sie den Operations Manager-Agent nicht direkt aktualisieren, da dieser Wert durch den globalen Wert für die Verwaltungsgruppe überschrieben wird.  Stattdessen müssen Sie die Regel außer Kraft setzen, die verwendet wird, um diese Werte mithilfe von Push zu übertragen.

> [!NOTE] 
> Mit der gleichen Konfigurationsmethode können Sie die Verwendung mehrerer OMS-Gatewayserver in Ihrer Umgebung ermöglichen.  So können beispielsweise spezifische OMS-Gatewayserver für einzelne Regionen angegeben werden.

1. Öffnen Sie die Operations Manager-Konsole, und wählen Sie den Arbeitsbereich **Erstellen** aus.  
2. Wählen Sie im Erstellungsarbeitsbereich die Option **Regeln** aus, und klicken Sie auf der Operations Manager-Symbolleiste auf die Schaltfläche **Bereich**. Sollte diese Schaltfläche nicht verfügbar sein, vergewissern Sie sich, dass im Überwachungsbereich kein Ordner, sondern ein Objekt ausgewählt ist. Im Dialogfeld **Management Pack-Objekte in Bereiche einteilen** wird eine Liste mit allgemeinen Zielklassen, Gruppen oder Objekten angezeigt. 
3. Geben Sie in das Suchfeld**** den Suchbegriff **Integritätsdienst** ein, und wählen Sie den Dienst in der Liste aus.  Klicken Sie auf **OK**.  
4. Suchen Sie nach der Regel **Advisor Proxy Setting Rule** (Proxyeinstellungsregel für Ratgeber), und klicken Sie auf der Symbolleiste der Operations-Konsole auf **Außerkraftsetzungen**. Zeigen Sie anschließend auf **Regel außer Kraft setzen\Für ein bestimmtes Objekt der Klasse: Integritätsdienst**, und wählen Sie in der Liste ein bestimmtes Objekt aus.  Optional können Sie auch eine benutzerdefinierte Gruppe mit dem Integritätsdienstobjekt der Server erstellen, auf die Sie diese Außerkraftsetzung anwenden möchten, und dann die Außerkraftsetzung auf diese Gruppe anwenden.
5. Klicken Sie im Dialogfeld **Außerkraftsetzungseigenschaften**, um in der Spalte **Außerkraftsetzung** neben dem Parameter **WebProxyAddress** ein Häkchen zu setzen.  Geben Sie im Feld **Außerkraftsetzungswert** die URL des OMS-Gatewayservers ein. Beginnen Sie dabei mit dem Präfix `http://`.
   >[!NOTE]
   > Die Regel muss nicht aktiviert werden, da sie bereits automatisch mit einer Außerkraftsetzung aus dem Microsoft System Center Advisor-Management Pack für die Außerkraftsetzung von sicheren Verweisen verwaltet wird, das auf die Überwachungsservergruppe von Microsoft System Center Advisor ausgerichtet ist.
   > 
6. Wählen Sie in der Liste **Ziel-Management Pack auswählen** ein Management Pack aus, oder klicken Sie auf **Neu**, um ein neues, nicht versiegeltes Management Pack zu erstellen. 
7. Klicken Sie nach Abschluss der Änderungen auf **OK**. 

### <a name="configure-for-automation-hybrid-workers"></a>Konfigurieren für Automation Hybrid Worker
Für den Fall, dass Ihre Umgebung Automation Hybrid Runbook Worker enthält, finden Sie im Anschluss manuelle, temporäre Problemumgehungen, mit denen Sie das Gateway für deren Unterstützung konfigurieren können.

In den folgenden Schritten müssen Sie die Azure-Region kennen, in der sich das Automation-Konto befindet. So finden Sie den Speicherort

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie den Azure Automation-Dienst.
3. Wählen Sie das entsprechende Azure Automation-Konto aus.
4. Zeigen Sie seine Region unter **Speicherort** an.<br><br> ![Azure-Portal – Speicherort des Automation-Kontos](./media/log-analytics-oms-gateway/location.png)  

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

Wenn Ihr Computer automatisch als Hybrid Runbook Worker für das Patchen mithilfe der Update Management-Lösung registriert wird, gehen Sie wie folgt vor:

1. Fügen Sie die Auftragslaufzeit-Datendienst-URLs der Liste zulässiger Hosts auf dem OMS-Gateway hinzu. Beispiel:  `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. Starten Sie den OMS-Gatewaydienst mithilfe des folgenden PowerShell-Cmdlets neu: `Restart-Service OMSGatewayService`

Wenn das Onboarding Ihres Computers bei Azure Automation mithilfe des Cmdlets für die Hybrid Runbook Worker-Registrierung durchgeführt wird, gehen Sie wie folgt vor:

1. Fügen Sie die Agent-Dienstregistrierungs-URL der Liste zulässiger Hosts auf dem OMS-Gateway hinzu. Beispiel: `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. Fügen Sie die Auftragslaufzeit-Datendienst-URLs der Liste zulässiger Hosts auf dem OMS-Gateway hinzu. Beispiel:  `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. Starten Sie den OMS-Gatewaydienst neu.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Nützliche PowerShell-cmdlets
Cmdlets können Ihnen helfen, Aufgaben durchzuführen, die für die Aktualisierung der Konfigurationseinstellungen des OMS-Gateways erforderlich sind. Bevor Sie sie verwenden, müssen folgende Schritte durchgeführt worden sein:

1. Installieren des OMS-Gateways (MSI).
2. Öffnen Sie ein PowerShell-Konsolenfenster.
3. Geben Sie zum Importieren des Moduls folgenden Befehl ein: `Import-Module OMSGateway`
4. Wenn im vorherigen Schritt kein Fehler aufgetreten ist, wurde das Modul erfolgreich importiert, und die Cmdlets können verwendet werden. Geben Sie `Get-Module OMSGateway` ein
5. Nachdem Sie die Cmdlets geändert haben, stellen Sie sicher, dass Sie den Gatewaydienst neu starten.

Wenn in Schritt 3 ein Fehler auftritt, wurde das Modul nicht importiert. Der Fehler kann auftreten, wenn PowerShell das Modul nicht finden kann. Sie finden es im Gateway-Installationspfad: *C:\Programme\Microsoft OMS Gateway\PowerShell*.

| **Cmdlet** | **Parameter** | **Beschreibung** | **Beispiel** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Schlüssel |Ruft die Konfiguration des Diensts ab |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Schlüssel (erforderlich) <br> Wert |Ändert die Konfiguration des Diensts |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Ruft die Adresse des Relays (Upstreamproxy) ab |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Adresse<br> Benutzername<br> Kennwort |Legt die Adresse (und Anmeldeinformationen) des Relays (Upstreamproxy) fest |1. Legen Sie einen Relay-Proxy und Anmeldeinformationen fest:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Legen Sie einen Relay-Proxy fest, der keine Authentifizierung erfordert: `Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Löschen Sie die Relay-Proxyeinstellung:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |Ruft den derzeit zulässigen Host ab. (Gilt nur für den lokal konfigurierten zulässigen Host. Automatisch heruntergeladene zulässige Hosts werden nicht berücksichtigt.) |`Get-OMSGatewayAllowedHost` | 
| `Add-OMSGatewayAllowedHost` |Host (erforderlich) |Fügt den Host der Liste zulässiger Hosts hinzu |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Host (erforderlich) |Entfernt den Host aus der Liste zulässiger Hosts |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Antragsteller (erforderlich) |Fügt den Clientzertifikatantragsteller der Liste zulässiger Hosts hinzu |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Antragsteller (erforderlich) |Entfernt den Clientzertifikatantragsteller aus der Liste zulässiger Hosts |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Ruft die derzeit zulässigen Clientzertifikatantragsteller ab. (Gilt nur für lokal konfigurierte zulässige Antragsteller. Automatisch heruntergeladene zulässige Antragsteller werden nicht berücksichtigt.) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Problembehandlung
Um vom Gateway protokollierte Ereignisse zu erfassen, muss auch der OMS-Agent installiert sein.<br><br> ![Ereignisanzeige – OMS-Gatewayprotokoll](./media/log-analytics-oms-gateway/event-viewer.png)

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
| 105 |FEHLER TcpConnection – Ungültiges Clientzertifikat: CN=Gateway <br><br> Stellen Sie Folgendes sicher: <br>    <br> &#149; Sie verwenden ein Gateway mit der Versionsnummer 1.0.395.0 oder höher. <br> &#149; Der MMA-Agent auf dem Gatewayserver und die Agents, die mit dem Gateway kommunizieren, sind mit dem gleichen Log Analytics-Arbeitsbereich verbunden. |
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

