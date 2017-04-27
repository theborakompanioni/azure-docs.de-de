---
title: Herstellen einer Verbindung zwischen Offlinecomputern und der Operations Management Suite mithilfe des OMS-Gateways | Microsoft-Dokumentation
description: "Stellen Sie eine Verbindung mit Offlinecomputern her, die von der Operations Management Suite verwaltet und von System Center Operations Manager überwacht werden, und verwenden Sie dabei das OMS-Gateway, um Daten an den Operations Management Suite-Dienst zu senden."
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
ms.date: 03/27/2017
ms.author: magoedte; banders
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 19bf9b3d65210458e4f018dc591e35e59287cd8e
ms.lasthandoff: 04/03/2017

---

# <a name="connect-offline-computers-to-operations-management-suite-by-using-oms-gateway"></a>Herstellen einer Verbindung zwischen Offlinecomputern und der Operations Management Suite mithilfe des OMS-Gateways

Computer, die von der Operations Management Suite (OMS) verwaltet und von System Center Operations Manager (Operations Manager) überwacht werden, können Daten an den Operations Management Suite-Dienst senden, wenn sie nicht über Internetzugriff verfügen. Das OMS-Gateway ist ein HTTP-Forwardproxy, der HTTP-Tunnel mit dem Befehl „HTTP CONNECT“ unterstützt. Das OMS-Gateway kann Daten sammeln und im Auftrag von Offlinecomputern an den Operations Management Suite-Dienst senden.  

Sie können das OMS-Gateway mit Folgendem verwenden:

* Azure Automation Hybrid Runbook Worker.  
* Windows-Computer, die über den Microsoft Monitoring Agent verfügen und direkt mit einem Operations Management Suite-Arbeitsbereich verbunden sind.
* System Center Operations Manager 2012 SP1 mit Updaterollup (UR) 7, System Center Operations Manager 2012 R2 mit UR3 oder eine in die Operations Management Suite integrierte System Center Operations Manager 2016-Verwaltungsgruppe.  

Einige IT-Sicherheitsrichtlinien lassen nicht zu, dass vernetzte Computer eine Verbindung mit dem Internet herstellen. Das kann beispielsweise bei POS-Geräten (Point Of Sale) oder bei Servern der Fall sein, die IT-Dienste unterstützen. Die Computer müssen zur Verwaltung und Überwachung jedoch mit der Operations Management Suite verbunden werden. Sie können Offlinecomputer so einrichten, dass sie direkt mit dem OMS-Gateway kommunizieren. Das Gateway empfängt Konfigurations- und Weiterleitungsdaten im Auftrag der Offlinecomputer.  

Wenn Sie Computer mit dem Operations Management Suite-Agent einrichten, um eine direkte Verbindung mit einem Operations Management Suite-Arbeitsbereich herzustellen, kommunizieren alle Computer stattdessen über das OMS-Gateway. Das Gateway überträgt Daten von den Agents direkt an die Operations Management Suite. Die übertragenen Daten werden vom Gateway nicht analysiert.

Wenn Sie eine Operations Manager-Verwaltungsgruppe in die Operations Management Suite integrieren, können Sie Verwaltungsserver für die Verbindungsherstellung mit dem OMS-Gateway einrichten. Die Server empfangen Konfigurationsinformationen und senden dann die gesammelten Daten abhängig von der eingerichteten Lösung. Operations Manager-Agents senden einige Daten wie etwa Operations Manager-Warnungen, Konfigurationsbewertung, Speicherplatz der Instanz und Kapazitätsdaten an den Verwaltungsserver. Andere, umfangreiche Daten wie etwa IIS-Protokolle (Internet Information Services, Internetinformationsdienste), Leistungsinformationen und Sicherheitsereignisse werden direkt an das OMS-Gateway gesendet. 

Ein Operations Manager-Gatewayserver, der zur Überwachung nicht vertrauenswürdiger Systeme in einem Umkreisnetzwerk (auch *DMZ*/*Demilitarized Zone* *genannt*) oder in einem anderen isolierten Netzwerk bereitgestellt wird, kann nicht mit dem OMS-Gateway kommunizieren. Operations Manager-Gatewayserver können nur mit einem Verwaltungsserver kommunizieren. Wenn Sie eine Operations Manager-Verwaltungsgruppe für die Kommunikation mit dem OMS-Gateway einrichten, werden die Proxykonfigurationsinformationen automatisch an alle per Agent verwalteten Computer verteilt, die zum Sammeln von Daten für Azure Log Analytics eingerichtet sind. Proxykonfigurationsinformationen werden auch dann automatisch verteilt, wenn die Einstellung leer ist.    

Zur Gewährleistung einer hohen Verfügbarkeit für direkt verbundene Agents oder Operations Management-Gruppen, die über das Gateway mit der Operations Management Suite kommunizieren, können Sie einen Netzwerklastenausgleich (Network Load Balancing, NLB) verwenden. NLB leitet Datenverkehr um und verteilt ihn auf mehrere Gatewayserver. Falls ein Gatewayserver ausfällt, wird der Datenverkehr an einen anderen verfügbaren Knoten umgeleitet.  

Es empfiehlt sich, den Operations Management Suite-Agent auf dem Computer zu installieren, auf dem die OMS-Gateway-Software ausgeführt wird. So können Sie das OMS-Gateway überwachen und Leistungs- oder Ereignisdaten analysieren. Der Agent unterstützt das OMS-Gateway außerdem beim Ermitteln der Dienstendpunkte, mit denen es kommunizieren muss.

Jeder Agent benötigt eine Netzwerkverbindung mit dem entsprechenden Gateway, damit Agents automatisch Daten an das und vom Gateway übertragen können. Wir raten davon ab, das Gateway auf einem Domänencontroller zu installieren.

Das folgende Diagramm zeigt den Datenfluss von direkten Agents an Operations Management Suite über den Gatewayserver. Die Proxykonfiguration für Agents muss dem Port entsprechen, über den das OMS-Gateway mit der Operations Management Suite kommuniziert.  

![Diagramm für die direkte Agent-Kommunikation mit der Operations Management Suite](./media/log-analytics-oms-gateway/oms-omsgateway-agentdirectconnect.png)

Das folgende Diagramm zeigt den Datenfluss von einer Operations Manager-Verwaltungsgruppe an die Operations Management Suite.   

![Diagramm für die Operations Manager-Kommunikation mit der Operations Management Suite](./media/log-analytics-oms-gateway/oms-omsgateway-opsmgrconnect.png)

## <a name="prerequisites"></a>Voraussetzungen

Ein Computer, auf dem das OMS-Gateway ausgeführt werden soll, muss über Folgendes verfügen:

* Betriebssystem: Windows 10, Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 oder Windows Server 2008
* Microsoft .NET Framework 4.5
* Vierkernprozessor (mindestens)
* 8 GB Arbeitsspeicher (mindestens) 

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

## <a name="download-oms-gateway"></a>Herunterladen des OMS-Gateways

Die neueste Version des OMS-Gateways kann auf drei Arten bezogen werden:

* Sie können es über das [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=54443) herunterladen.

* Sie können es über das Operations Management Suite-Portal herunterladen. Gehen Sie nach der Anmeldung bei Ihrem Operations Management Suite-Arbeitsbereich wie folgt vor:

    1. Navigieren Sie zu **Einstellungen** > **Verbundene Quellen** > **Windows-Server**. 
    2. Wählen Sie **OMS-Gateway herunterladen** aus.

* Sie können es über das [Azure-Portal](https://portal.azure.com) herunterladen. Gehen Sie nach der Anmeldung wie folgt vor:  

   1. Wählen Sie in der Liste mit den Diensten die Option **Log Analytics** aus.  
   2. Wählen Sie einen Arbeitsbereich aus.
   3. Wählen Sie auf dem Blatt für Ihren Arbeitsbereich unter **Allgemein** die Option **Schnellstart** aus.
   4. Wählen Sie unter **Datenquelle zum Herstellen einer Verbindung mit dem Arbeitsbereich auswählen** die Option **Computer** aus.
   5. Wählen Sie auf dem Blatt **Direkt-Agent** die Option **OMS-Gateway herunterladen** aus.<br><br> ![Download OMS Gateway](./media/log-analytics-oms-gateway/download-gateway.png) (OMS-Gateway herunterladen)


## <a name="install-oms-gateway"></a>Installieren des OMS-Gateways

Führen Sie zum Installieren des Gateways die folgenden Schritte aus. 

> [!NOTE]
> Falls eine frühere Version des Gateways (ehemals Log Analytics-Weiterleitung genannt) installiert ist, wird diese auf die aktuelle Version des OMS-Gateways aktualisiert.
>

1. Doppelklicken Sie im Zielordner auf **OMS Gateway.msi**.
2. Wählen Sie auf der Seite **Willkommen** die Option **Weiter** aus.<br><br> ![Gateway-Setup-Assistent](./media/log-analytics-oms-gateway/gateway-wizard01.png)<br> 
3. Wählen Sie auf der Seite **Lizenzvertrag** die Option **Ich stimme den Bedingungen des Lizenzvertrags zu.** aus, um den Lizenzbestimmungen zuzustimmen aus. 
4. Gehen Sie auf der Seite für die Port- und Proxyadresse**** wie folgt vor:

   1. Geben Sie die für das Gateway zu verwendende TCP-Portnummer ein. Das Setup konfiguriert eine Eingangsregel mit dieser Portnummer für die Windows-Firewall. Der Standardwert ist 8080.
      Zulässig sind Portnummern im Bereich zwischen 1 und 65535. Liegt die Eingabe außerhalb dieses Bereichs, erscheint eine Fehlermeldung.
   2. Optional: Falls der Server, auf dem das Gateway installiert ist, über einen Proxy kommunizieren muss, geben Sie die Proxyadresse ein, mit der das Gateway eine Verbindung herstellen muss. Beispiel: **http://myorgname.corp.contoso.com:80**. Wenn das Proxyadressfeld leer ist, versucht das Gateway, eine direkte Verbindung mit dem Internet herzustellen. Falls der Proxyserver eine Authentifizierung erfordert, geben Sie einen Benutzernamen und ein Kennwort ein.<br><br> ![Gateway-Assistent-Proxykonfiguration](./media/log-analytics-oms-gateway/gateway-wizard02.png)<br>   
5. Falls Microsoft Update auf dem Computer nicht aktiviert ist, erscheint die Seite **Microsoft Update**. Auf dieser Seite können Sie das Feature aktivieren. Fahren Sie andernfalls mit dem nächsten Schritt fort.
6. Übernehmen Sie auf der Seite **Zielordner** entweder den Standardordner „C:\Programme\OMS Gateway“, oder geben Sie den Speicherort ein, an dem Sie das Gateway installieren möchten.
7. Wählen Sie auf der Seite **Bereit zur Installation** die Option **Installieren** aus. Daraufhin erscheint unter Umständen ein Dialogfeld, in dem die Berechtigung zum Installieren angefordert wird. Wählen Sie **Ja** aus.
8. Wählen Sie **Fertig stellen** aus. Vergewissern Sie sich, dass der Dienst ausgeführt wird. Öffnen Sie hierzu das Snap-In „Services.msc“, und überprüfen Sie, ob **OMS-Gateway** in der Liste mit den Diensten angezeigt wird. Der Status sollte **Wird ausgeführt** lauten.<br><br> ![Überprüfen des OMS-Gateway-Diensts](./media/log-analytics-oms-gateway/gateway-service.png)  

## <a name="set-up-network-load-balancing"></a>Einrichten des Netzwerklastenausgleichs 
Sie können das OMS-Gateway mithilfe von NLB für hohe Verfügbarkeit einrichten. Dazu können Sie entweder das NLB-Feature (Network Load Balancing, Netzwerklastenausgleich) in Windows Server oder hardwarebasierte Lastenausgleichsmodule verwenden. Zur Verwaltung des Datenverkehrs leitet der Lastenausgleich angeforderte Verbindungen von Operations Management Suite-Agents oder Operations Manager-Verwaltungsservern über sämtliche Knoten hinweg um. Sollte ein Gatewayserver ausfallen, wird der Datenverkehr an andere Knoten umgeleitet.

Informationen zum Entwerfen und Bereitstellen eines Netzwerklastenausgleichs-Clusters unter Windows Server 2016 finden Sie unter [Netzwerklastenausgleich](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  

Gehen Sie zum Einrichten eines Netzwerklastenausgleich-Clusters unter Windows Server wie folgt vor:  

1. Melden Sie sich mit einem lokalen Administratorkonto bei dem Windows-Server an, der dem Netzwerklastenausgleich-Cluster angehört.  
2. Öffnen Sie im Server-Manager den Netzwerklastenausgleich-Manager.
3. Wählen Sie **Tools** und anschließend **Netzwerklastenausgleich-Manager** aus.
4. Um einen OMS-Gatewayserver mit dem installierten Microsoft Monitoring Agent zu verbinden, klicken Sie mit der rechten Maustaste auf die IP-Adresse des Clusters, und wählen Sie anschließend **Host dem Cluster hinzufügen** aus.<br><br> ![Netzwerklastenausgleich-Manager: Host dem Cluster hinzufügen](./media/log-analytics-oms-gateway/nlb02.png)<br> 
5. Geben Sie die IP-Adresse des Gatewayservers ein, mit dem Sie eine Verbindung herstellen möchten.<br><br> ![Netzwerklastenausgleich-Manager: Host dem Cluster hinzufügen und Verbindung herstellen](./media/log-analytics-oms-gateway/nlb03.png) 
    
## <a name="set-up-the-operations-management-suite-agent-and-an-operations-manager-management-group"></a>Einrichten des Operations Management Suite-Agents und einer Operations Manager-Verwaltungsgruppe
In diesem Abschnitt erfahren Sie, wie Sie Operations Management Suite-Agents mit Direktverbindung und eine Operations Manager-Verwaltungsgruppe einrichten. Sie können auch Azure Automation Hybrid Runbook Worker einrichten, um das OMS-Gateway für die Kommunikation mit der Operations Management Suite zu verwenden.  

Weitere Informationen zu Anforderungen und Schritten für die Installation des Operations Management Suite-Agents auf Windows-Computern mit direkter Operations Management Suite-Verbindung finden Sie unter [Verbinden von Windows-Computern mit dem Log Analytics-Dienst in Azure](log-analytics-windows-agents.md). Informationen für Linux-Computer finden Sie unter [Verbinden Ihrer Linux-Computer mit Log Analytics](log-analytics-linux-agents.md). 

### <a name="set-up-the-operations-management-suite-agent-and-operations-manager-to-use-the-oms-gateway-as-a-proxy-server"></a>Einrichten von Operations Management Suite-Agent und Operations Manager für die Verwendung des OMS-Gateways als Proxyserver

### <a name="set-up-a-standalone-operations-management-suite-agent"></a>Einrichten eines eigenständigen Operations Management Suite-Agents
Informationen zum Einrichten eines Agents für die Verwendung eines Proxyservers finden Sie unter [Konfigurieren von Proxy- und Firewalleinstellungen in Log Analytics](log-analytics-proxy-firewall.md). In diesem Fall ist der Proxyserver das Gateway. Wenn Sie mehrere Gatewayserver hinter einem Netzwerklastenausgleich bereitgestellt haben, handelt es sich bei der Proxykonfiguration des Operations Management Suite-Agents um die virtuelle IP-Adresse des Netzwerklastenausgleichs:<br><br> ![Microsoft Monitoring Agenteigenschaften: Proxyeinstellungen](./media/log-analytics-oms-gateway/nlb04.png)

### <a name="set-up-operations-manager-all-agents-use-the-same-proxy-server"></a>Einrichten von Operations Manager (gleicher Proxyserver für alle Agents)
Richten Sie Operations Manager ein, um den Gatewayserver hinzuzufügen. Die Operations Manager-Proxykonfiguration wird automatisch auf alle Agents angewendet, die Operations Manager unterstellt sind. Das gilt auch, wenn die Einstellung leer ist.

Für die Verwendung des OMS-Gateways mit Operations Manager müssen folgende Anforderungen erfüllt sein:

* Microsoft Monitoring Agent (ab Agent-Version **8.0.10900.0**) muss auf dem Gatewayserver installiert sein. Es muss für die Arbeitsbereiche des Operations Management Suite-Agents eingerichtet sein, mit denen Sie kommunizieren möchten.
* Das Gateway benötigt Internetkonnektivität oder muss mit einem Proxyserver verbunden sein, bei dem dies der Fall ist.

> [!NOTE]
> Wenn Sie keinen Wert für das Gateway angeben, werden mithilfe von Push leere Werte an alle Agents übertragen.

Gehen Sie wie folgt vor, um Ihren Operations Manager-Server mit dem Arbeitsbereich Ihres Operations Management Suite-Agents zu verbinden:

1. Navigieren Sie in der Operations Manager-Konsole zu **Operations Management Suite** > **Verbindung** > **Proxyserver konfigurieren**.<br><br> ![Operations Manager: Proxyserver konfigurieren](./media/log-analytics-oms-gateway/scom01.png)<br> 
2. Wählen Sie **Proxyserver für Zugriff auf Operations Management Suite verwenden** aus. Geben Sie die IP-Adresse des OMS-Gatewayservers oder die virtuelle IP-Adresse des Netzwerklastenausgleichs ein. Beginnen Sie mit dem Präfix **http://**.<br><br> ![Operations Manager: Proxyserveradresse](./media/log-analytics-oms-gateway/scom02.png)<br> 
3. Wählen Sie **Fertig stellen** aus. 

### <a name="set-up-operations-manager-specific-agents-use-the-proxy-server"></a>Einrichten von Operations Manager (Verwendung des Proxyservers durch bestimmte Agents)
In großen oder komplexen Umgebungen soll der OMS-Gatewayserver unter Umständen nur von bestimmten Servern (oder Gruppen) verwendet werden. Für diese Server können Sie den Operations Manager-Agent nicht direkt aktualisieren. Der Wert wird durch den globalen Wert für die Verwaltungsgruppe außer Kraft gesetzt. Setzen Sie stattdessen die Regel außer Kraft, die verwendet wird, um diese Werte mithilfe von Push zu übertragen.

> [!NOTE] 
> Mit der gleichen Konfigurationsmethode können Sie mehrere OMS-Gatewayserver in Ihrer Umgebung ausführen. So können beispielsweise spezifische OMS-Gatewayserver für einzelne Regionen angegeben werden.

1. Öffnen Sie die System Center Operations Manager-Konsole, und wählen Sie den Arbeitsbereich **Erstellen** aus.  
2. Wählen Sie **Regeln** und anschließend auf der Verwaltungssymbolleiste von System Center Operations die Schaltfläche **Bereich** aus. Sollte diese Schaltfläche nicht verfügbar sein, vergewissern Sie sich, dass im Bereich **Überwachung** kein Ordner, sondern ein Objekt ausgewählt ist. Im Dialogfeld **Management Pack-Objekte in Bereiche einteilen** wird eine Liste mit allgemeinen Zielklassen, Gruppen oder Objekten angezeigt. 
3. Geben Sie im Suchfeld**** den Suchbegriff **Integritätsdienst** ein. Wählen Sie ihn in der Liste aus. Klicken Sie auf **OK**.  
4. Suchen Sie auf der Symbolleiste der Operations Manager-Konsole nach der Regel **Advisor Proxy Setting Rule** (Proxyeinstellungsregel für Ratgeber). Wählen Sie **Außerkraftsetzungen** aus, und zeigen Sie anschließend auf **Regel außer Kraft setzen\Für ein bestimmtes Objekt der Klasse: Integritätsdienst**. Wählen Sie in der Liste ein bestimmtes Objekt aus. Optional können Sie auch eine benutzerdefinierte Gruppe mit dem Integritätsdienstobjekt der Server erstellen, auf die Sie diese Außerkraftsetzung anwenden möchten. Wenden Sie die Außerkraftsetzung anschließend auf diese Gruppe an.
5. Wählen Sie im Dialogfeld **Außerkraftsetzungseigenschaften** neben dem Parameter **WebProxyAddress** die Spalte **Außerkraftsetzung** aus. Geben Sie im Feld **Außerkraftsetzungswert** die URL des OMS-Gatewayservers ein. Beginnen Sie mit dem Präfix **http://**.

   >[!NOTE]
   > Sie müssen die Regel nicht aktivieren. Die Regel wird automatisch mit einer Außerkraftsetzung aus dem System Center Advisor-Management Pack für die Außerkraftsetzung von sicheren Verweisen verwaltet. Das Management Pack ist auf die System Center Advisor-Überwachungsservergruppe ausgerichtet.
   > 

6. Gehen Sie zum Angeben eines Management Packs wie folgt vor:
    * Wählen Sie in der Liste **Ziel-Management Pack auswählen** ein Management Pack aus.
    * Durch Auswählen von **Neu** können Sie ein neues, nicht versiegeltes Management Pack erstellen. 
7. Klicken Sie auf **OK**. 

### <a name="set-up-automation-hybrid-workers"></a>Einrichten von Automation-Hybrid Workern
Wenn Ihre Umgebung Azure Automation Hybrid Runbook Worker enthält, können Sie eine manuelle, temporäre Problemumgehung erstellen, damit sie vom Gateway unterstützt werden.

Für die folgenden Schritte müssen Sie die Azure-Region kennen, in der sich das Automation-Konto befindet. Gehen Sie zum Ermitteln des Standorts wie folgt vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie den Azure Automation-Dienst.
3. Wählen Sie das relevante Azure Automation-Konto aus.
4. Die Region wird unter **Standort** angezeigt.<br><br> ![Azure-Portal: Standort des Automation-Kontos](./media/log-analytics-oms-gateway/location.png)  

Verwenden Sie die folgenden Tabellen, um die URL für jeden Speicherort zu identifizieren:

**Auftragslaufzeit-Datendienst-URLs**

| Ort | URL |
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

| Ort | URL |
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

1. Fügen Sie die URLs des Auftragslaufzeit-Datendiensts auf dem OMS-Gateway der Liste zulässiger Hosts**** hinzu. Beispiel:  `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. Starten Sie den OMS-Gatewaydienst mithilfe des folgenden Azure PowerShell-Cmdlets neu: `Restart-Service OMSGatewayService`

Wenn das Onboarding Ihres Computers bei Azure Automation mithilfe des Cmdlets für die Hybrid Runbook Worker-Registrierung durchgeführt wird, gehen Sie wie folgt vor:

1. Fügen Sie die Agent-Dienstregistrierungs-URL auf dem OMS-Gateway der Liste zulässiger Hosts**** hinzu. Beispiel: `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. Fügen Sie die URLs des Auftragslaufzeit-Datendiensts auf dem OMS-Gateway der Liste zulässiger Hosts**** hinzu. Beispiel:  `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. Starten Sie den OMS-Gatewaydienst neu:  `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Nützliche PowerShell-cmdlets
Cmdlets können Sie beim Aktualisieren der OMS-Gatewayeinstellungen unterstützen. Führen Sie vor der Verwendung von PowerShell-Cmdlets folgende Schritte aus:

1. Installieren Sie das OMS-Gateway (MSI).
2. Öffnen Sie ein PowerShell-Konsolenfenster.
3. Geben Sie zum Importieren des Moduls den folgenden Befehl ein: `Import-Module OMSGateway`. Wenn kein Fehler auftritt, wurde das Modul erfolgreich importiert, und Sie können die Cmdlets verwenden. 
4. Geben Sie `Get-Module OMSGateway` ein.
5. Starten Sie den OMS-Gatewaydienst neu, nachdem Sie mithilfe von Cmdlets Änderungen vorgenommen haben.

Wenn in Schritt 3 ein Fehler auftritt, wurde das Modul nicht importiert. Dieser Fehler kann auftreten, wenn PowerShell das Modul nicht findet. Das Modul befindet sich am Installationspfad des Gateways: „C:\Programme\Microsoft OMS Gateway\PowerShell“.

| Cmdlet | Parameter | Beschreibung | Beispiele |
| --- | --- | --- | --- |
| `Set-OMSGatewayConfig` |Schlüssel (erforderlich) <br> Wert |Ändert die Dienstkonfiguration. |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |
| `Get-OMSGatewayConfig` |Schlüssel |Ruft die Dienstkonfiguration ab. |`Get-OMSGatewayConfig` <br> <br> `Get-OMSGatewayConfig -Name ListenPort` |
| `Set-OMSGatewayRelayProxy` |Adresse <br> Benutzername <br> Kennwort |Legt die Adresse (und die Anmeldeinformationen) des Relays (Upstreamproxy) fest. |1. Legen Sie ein Antwortproxy und die Anmeldeinformationen fest: `Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080 -Username user1 -Password 123` <br> <br> 2. Legen Sie ein Antwortproxy fest, das keine Authentifizierung voraussetzt: `Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080` <br> <br> 3. Löschen Sie die Antwortproxyeinstellung, um anzugeben, dass Sie keinen Antwortproxy benötigen: `Set-OMSGatewayRelayProxy -Address ""` |
| `Get-OMSGatewayRelayProxy` | |Ruft die Adresse des Relays (Upstreamproxy) ab. |`Get-OMSGatewayRelayProxy` |
| `Add-OMSGatewayAllowedHost` |Host (erforderlich) |Fügt den Host der Liste zulässiger Hosts hinzu |`Add-OMSGatewayAllowedHost -Host www.test.com` |
| `Remove-OMSGatewayAllowedHost` |Host (erforderlich) |Entfernt den Host aus der Liste zulässiger Hosts |`Remove-OMSGatewayAllowedHost -Host www.test.com` |
| `Get-OMSGatewayAllowedHost` | |Ruft den derzeit zulässigen Host ab. (Gilt nur für den lokal konfigurierten zulässigen Host. Automatisch heruntergeladene zulässige Hosts werden nicht berücksichtigt.) |`Get-OMSGatewayAllowedHost` |
| `Add-OMSGatewayAllowedClientCertificate` |Antragsteller (erforderlich) |Fügt den Clientzertifikatantragsteller der Liste zulässiger Hosts hinzu |`Add-OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Remove-OMSGatewayAllowedClientCertificate` |Antragsteller (erforderlich) |Entfernt den Clientzertifikatantragsteller aus der Liste zulässiger Hosts |`Remove- OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Get-OMSGatewayAllowedClientCertificate` | |Ruft die derzeit zulässigen Clientzertifikat-Antragsteller ab. (Gilt nur für lokal konfigurierte zulässige Antragsteller. Automatisch heruntergeladene zulässige Antragsteller werden nicht berücksichtigt.) |`Get-OMSGatewayAllowedClientCertificate` |

## <a name="troubleshooting"></a>Problembehandlung
Um vom Gateway protokollierte Ereignisse zu erfassen, muss der Operations Management Suite-Agent installiert sein.<br><br> ![Ereignisanzeige: OMS-Gatewayprotokoll](./media/log-analytics-oms-gateway/event-viewer.png)

**OMS-Gatewayereignis-IDs und Beschreibungen**

Die folgende Tabelle enthält die Ereignis-IDs und Beschreibungen für OMS Gatewayprotokollereignisse:

| ID | Beschreibung |
| --- | --- |
| 400 |Alle Anwendungsfehler, die keine bestimmte ID haben |
| 401 |Falsche Konfiguration. Beispiel: listenPort = "\<text\>" (anstelle einer ganzen Zahl) |
| 402 |Fehler bei der Analyse von TLS-Handshakenachrichten (Transport Layer Security). |
| 403 |Netzwerkfehler. Beispiel: Die Verbindung mit dem Zielserver kann nicht hergestellt werden. |
| 100 |Allgemeine Informationen |
| 101 |Dienst wurde gestartet |
| 102 |Dienst wurde beendet |
| 103 |HTTP CONNECT-Befehl vom Client empfangen |
| 104 |Kein HTTP CONNECT-Befehl |
| 105 |Der Zielserver ist nicht in der Liste zulässiger Elemente enthalten, oder der Zielport ist kein sicherer Port (443). <br> <br> Stellen Sie sicher, dass der Microsoft Monitoring Agent-Agent auf Ihrem Gatewayserver und die Agents, die mit dem Gateway kommunizieren, mit dem gleichen Log Analytics-Arbeitsbereich verbunden sind. |
| 105 |FEHLER TcpConnection – Ungültiges Clientzertifikat: CN=Gateway <br><br> Stellen Sie Folgendes sicher: <br>    <br> &#149; Sie verwenden mindestens die OMS-Gatewayversion 1.0.395.0. <br> &#149; Der Microsoft Monitoring Agent-Agent auf Ihrem Gatewayserver und die Agents, die mit dem Gateway kommunizieren, sind mit dem gleichen Log Analytics-Arbeitsbereich verbunden. |
| 106 |Beliebiger Grund dafür, dass die TLS-Sitzung verdächtig ist und abgelehnt wird |
| 107 |Die TLS-Sitzung wurde überprüft |

**Zu sammelnde Leistungsindikatoren**

Die folgende Tabelle enthält die verfügbaren Leistungsindikatoren für das OMS-Gateway. Die Indikatoren können über den Windows-Systemmonitor hinzugefügt werden.

| Name | Beschreibung |
| --- | --- |
| OMS-Gateway/Aktive Clientverbindung |Anzahl der aktiven Clientnetzwerkverbindungen (TCP) |
| OMS-Gateway/Fehleranzahl |Anzahl von Fehlern |
| OMS-Gateway/Verbundener Client |Anzahl verbundener Clients |
| OMS-Gateway/Ablehnungsanzahl |Anzahl von Ablehnungen aufgrund von TLS-Überprüfungsfehlern |

![OMS-Gateway-Leistungsindikatoren](./media/log-analytics-oms-gateway/counters.png)

## <a name="get-assistance"></a>Unterstützung
Nach der Anmeldung beim Azure-Portal können Sie Unterstützung für das OMS-Gateway oder für andere Azure-Dienste oder Dienstfeatures anfordern.
Wählen Sie zum Anfordern von Unterstützung das Fragezeichen in der rechten oberen Ecke des Portals und anschließend die Option **Neue Supportanfrage** aus. Füllen Sie das Formular für die neue Supportanfrage aus.

![Neue Supportanfrage](./media/log-analytics-oms-gateway/support.png)

Im [Microsoft Azure-Feedbackforum](https://feedback.azure.com/forums/267889) können Sie außerdem Feedback zur Operations Management Suite oder zu Log Analytics abgeben.

## <a name="next-steps"></a>Nächste Schritte
Sie können [Datenquellen hinzufügen](log-analytics-data-sources.md), um Daten aus den verbundenen Quellen in Ihrem Operations Management Suite-Arbeitsbereich zu erfassen. Die Daten können im Operations Management Suite-Repository gespeichert werden.

