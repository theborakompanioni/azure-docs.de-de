<properties
	pageTitle="Verbinden von Windows-Computern mit Log Analytics | Microsoft Azure"
	description="Dieser Artikel beschreibt die Schritte, mit denen Sie die Windows-Computer in Ihrer lokalen Infrastruktur direkt mit OMS verbinden können, indem Sie eine benutzerdefinierte Version des Microsoft Monitoring Agent (MMA) verwenden."
	services="log-analytics"
	documentationCenter=""
	authors="bandersmsft"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="log-analytics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/28/2016"
	ms.author="banders"/>


# Verbinden von Windows-Computern mit Log Analytics

Dieser Artikel beschreibt die Schritte, mit denen Sie die Windows-Computer in Ihrer lokalen Infrastruktur direkt mit OMS verbinden können, indem Sie eine benutzerdefinierte Version des Microsoft Monitoring Agent (MMA) verwenden. Sie müssen Agents für alle Computer, die in OMS aufgenommen werden sollen, installieren und verbinden, damit diese an OMS Daten übermitteln und um diese Daten im OMS-Portal anzuzeigen und darauf zu reagieren.

Sie können Agents über das Setup, eine Befehlszeile oder durch eine Konfiguration des gewünschten Zustands in Azure Automation installieren.

Auf Computern mit Internetanschluss nutzt der Agent die Internetverbindung, um Daten an OMS zu senden. Bei Computern ohne Internetverbindung können Sie einen Proxy oder den OMS Log Analytics Forwarder verwenden.

Sie können Ihre Windows-Computer ganz einfach mit OMS verbinden, indem Sie die drei folgenden Schritte ausführen:

1. Laden Sie die Setup-Datei für den Agent herunter.
2. Installieren Sie den Agent mit der von Ihnen bevorzugten Methode.
3. Konfigurieren Sie ggf. den Agent.

Das folgende Diagramm zeigt die Beziehung zwischen Ihren Windows-Computern und OMS nach der Installation und Konfiguration der Agents.

![oms-direct-agent-diagram](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)


## Systemanforderungen und erforderliche Konfiguration
Prüfen Sie vor der Installation oder Bereitstellung der Agents die folgenden Details, um sicherzustellen, dass die diesbezüglichen Anforderungen erfüllt sind.

- Sie können den OMS MMA nur auf Computern installieren, auf denen Windows Server 2008 SP1 oder höher oder Windows 7 SP1 oder höher ausgeführt wird.
- Sie benötigen ein OMS-Abonnement. Weitere Informationen finden Sie unter [Get started with Log Analytics](log-analytics-get-started.md) (Erste Schritte mit Log Analytics).
- Jeder Windows-Computer muss eine Verbindung mit dem Internet herstellen können. Diese Verbindung kann direkt, über einen Proxy oder über den OMS Log Analytics Forwarder hergestellt werden.
- Sie können den OMS MMA auf eigenständigen Computern, Servern und virtuellen Computern installieren. Informationen zum Verbinden von in Azure gehosteten virtuellen Computern mit OMS finden Sie unter [Connect Azure storage to Log Analytics](log-analytics-azure-storage.md) (Herstellen einer Verbindung zwischen Azure-Speichern und Log Analytics).
- Der Agent muss den TCP-Port 443 für verschiedene Ressourcen verwenden. Weitere Informationen finden Sie unter [Configure proxy and firewall settings in Log Analytics](log-analytics-proxy-firewall.md) (Konfigurieren von Proxy- und Firewalleinstellungen in Log Analytics).

## Herunterladen der Setup-Datei von OMS
1. Klicken Sie im OMS-Portal auf der Seite **Übersicht** auf die Kachel **Einstellungen**. Klicken Sie oben auf die Registerkarte **Verbundene Datenquellen**. ![Registerkarte „Verbundene Datenquellen“](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. Klicken Sie unter **Attach Computers Directly** (Computer direkt anfügen) auf **Windows-Agent herunterladen** für den entsprechenden Prozessortyp, um die Setup-Datei herunterzuladen.
3. Klicken Sie rechts von **Arbeitsbereichs-ID** auf das Kopiersymbol, und fügen Sie die ID in den Editor ein.
4. Klicken Sie rechts von **Primärschlüssel** auf das Kopiersymbol, und fügen Sie den Schlüssel in den Editor ein. ![Arbeitsbereichs-ID und Primärschlüssel kopieren](./media/log-analytics-windows-agents/oms-direct-agent-primary-key.png)

## Installieren des Agents über Setup
1. Führen Sie Setup aus, um den Agent auf einem Computer zu installieren, den Sie verwalten möchten.
2. Klicken Sie auf der Seite **Willkommen** auf Weiter.
3. Lesen Sie die Seite „Lizenzbedingungen“ durch, und klicken Sie anschließend auf **Ich stimme zu**.
4. Auf der Seite „Zielordner“ können Sie den Standardinstallationsordner entweder ändern oder beibehalten. Klicken Sie anschließend auf **Weiter**.
5. Wählen Sie auf der Seite „Agent-Setupoptionen“ aus, ob der Agent mit Operational Insights (OMS) oder Operations Manager verbunden werden soll. Sie können die Auswahl aber auch leer lassen, wenn Sie den Agent später konfigurieren möchten. Klicken Sie auf **Weiter**.   
    - Wenn Sie eine Verbindung mit Operational Insights (OMS) herstellen möchten, fügen Sie die **Arbeitsbereichs-ID** und den **Arbeitsbereichsschlüssel (Primärschlüssel)** ein, die Sie im vorherigen Schritt in den Editor kopiert haben, und klicken Sie auf **Weiter**. ![Arbeitsbereichs-ID und Primärschlüssel einfügen](./media/log-analytics-windows-agents/oms-mma-aoi-setup.png)
    - Wenn Sie eine Verbindung mit Operations Manager herstellen möchten, geben Sie den **Namen der Verwaltungsgruppe**, den Namen des **Verwaltungsservers** und den **Verwaltungsserverport** ein, und klicken Sie anschließend auf **Weiter**. Wählen Sie auf der Seite „Agentaktionskonto“ entweder das lokale Systemkonto oder ein lokales Domänenkonto aus, und klicken Sie auf **Weiter**. ![Verwaltungsgruppenkonfiguration](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![Agentaktionskonto](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Überprüfen Sie Ihre Auswahl auf der Seite „Bereit zum Installieren“, und klicken Sie dann auf **Installieren**.
7. Klicken Sie auf der Seite „Die Konfiguration wurde erfolgreich abgeschlossen“ auf **Fertig stellen**.
8. Nach Abschluss wird der **Microsoft Monitoring Agent** in der **Systemsteuerung** angezeigt. Dort können Sie Ihre Konfiguration überprüfen und sicherstellen, dass der Agent mit Operational Insights (OMS) verbunden ist. Wenn die Verbindung mit OMS hergestellt ist, zeigt der Agent die folgende Meldung an: **The Microsoft Monitoring Agent has successfully connected to the Azure Operational Insights (OMS) service.** (Microsoft Monitoring Agent wurde erfolgreich mit dem Azure Operational Insights (OMS)-Dienst verbunden.).

## Installieren des Agents über die Befehlszeile
- Ändern Sie das folgende Beispiel, und verwenden Sie es dann zur Installation des Agents über die Befehlszeile.

    ```
    MMASetup-AMD64.exe /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

## Installieren des Agents mithilfe von DSC in Azure Automation
1. Importieren Sie das xPSDesiredStateConfiguration DSC-Modul von [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) in Azure Automation.  

2.	Erstellen Sie variable Azure Automation-Objekte für *OPSINSIGHTS\_WS\_ID* und *OPSINSIGHTS\_WS\_KEY*. Legen Sie für *OPSINSIGHTS\_WS\_ID* Ihre OMS Log Analytics-Arbeitsbereichs-ID und für *OPSINSIGHTS\_WS\_KEY* den Primärschlüssel Ihres Arbeitsbereichs fest.
3.	Verwenden Sie das folgende Skript, und speichern Sie es als „MMAgent.ps1“.
4.	Ändern Sie das folgende Beispiel, und verwenden Sie es dann zur Installation des Agents über DSC in Azure Automation. Importieren Sie „MMAgent.ps1“ in Azure Automation über die Azure Automation-Schnittstelle oder das Cmdlet.
5.	Weisen Sie der Konfiguration einen Knoten zu. Innerhalb von 15 Minuten prüft der Knoten die Konfiguration, und der MMA wird per Pushvorgang auf den Knoten übertragen.

```
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
        }

        xRemoteFile OIPackage {
            Uri = "https://opsinsight.blob.core.windows.net/publicfiles/MMASetup-AMD64.exe"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "E854571C-3C01-4128-99B8-52512F44E5E9"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}  


```


## Manuelles Konfigurieren eines Agents
Wenn Sie Agents installiert, aber noch nicht konfiguriert haben, oder wenn Sie Änderungen an der Konfiguration vorgenommen haben, können Sie die Agents mithilfe der folgenden Informationen aktivieren oder neu konfigurieren. Nachdem Sie den Agent konfiguriert haben, registriert dieser sich beim Agent-Dienst und ruft die erforderlichen Konfigurationsinformationen und Management Packs ab, die die Lösung enthalten.

1. Öffnen Sie nach der Installation von Microsoft Monitoring Agent die **Systemsteuerung**.
2. Öffnen Sie **Microsoft Monitoring Agent**, und klicken Sie dann auf **Connect to Azure Operational Insights (OMS)** (Mit Azure Operational Insights (OMS) verbinden).   
3. Fügen Sie die **Arbeitsbereichs-ID** und den **Arbeitsbereichsschlüssel (Primärschlüssel)** ein, die Sie zuvor in den Editor kopiert haben, und klicken Sie auf **OK**. ![Operational Insights konfigurieren](./media/log-analytics-windows-agents/oms-mma-aoi.png)

Nachdem die Daten von Computern erfasst wurden, die vom Agent überwacht werden, erscheint die Anzahl der von OMS überwachten Computer im OMS-Portal auf der Registerkarte **Verbundene Datenquellen** in den **Einstellungen** unter **Servers Connected** (Verbundene Server).


### So deaktivieren Sie einen Agent
1. Öffnen Sie nach der Installation des Agents die **Systemsteuerung**.
2. Öffnen Sie Microsoft Monitoring Agent, und klicken Sie dann auf die Registerkarte **Azure Operational Insights (OMS)**.
3. Deaktivieren Sie **Verbinden mit Azure Operational Insights**.

## Konfigurieren eines Agents über die Befehlszeile

- Sie können Windows PowerShell mit dem folgenden Beispiel verwenden.
    ```
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
    $healthServiceSettings.EnableAzureOperationalInsights('workspacename', 'workspacekey')
    $healthServiceSettings.ReloadConfiguration()
    ```

## Konfigurieren von Agents zum Berichten an eine Operations Manager-Verwaltungsgruppe (optional)

Wenn Sie Operations Manager in Ihrer IT-Infrastruktur verwenden, können Sie auch den MMA-Agent als Operations Manager-Agent nutzen.

### So konfigurieren Sie MMA-Agents zum Berichten an eine Operations Manager-Verwaltungsgruppe
1.	Öffnen Sie auf dem Computer, auf dem der Agent installiert ist, die **Systemsteuerung**.
2.	Öffnen Sie **Microsoft Monitoring Agent**, und klicken Sie dann auf die Registerkarte **Operations Manager**. ![Microsoft Monitoring Agent – Registerkarte „Operations Manager“](./media/log-analytics-windows-agents/oms-mma-om01.png)
3.	Wenn Ihre Operations Manager-Server über eine Integration mit Active Directory verfügen, klicken Sie auf „Verwaltungsgruppenzuweisungen aus AD DS automatisch aktualisieren“.
4.	Klicken Sie auf **Hinzufügen**, um das Dialogfeld **Verwaltungsgruppe hinzufügen** zu öffnen. ![Microsoft Monitoring Agent – Verwaltungsgruppe hinzufügen](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.	Geben Sie im Feld **Verwaltungsgruppenname** den Namen Ihrer Verwaltungsgruppe ein.
6.	Geben Sie im Feld **Primärer Verwaltungsserver** den Computernamen des primären Verwaltungsservers ein.
7.	Geben Sie im Feld **Verwaltungsserverport** die TCP-Portnummer ein.
8.	Wählen Sie unter **Agentaktionskonto** entweder das lokale Systemkonto oder ein lokales Domänenkonto aus.
9.	Klicken Sie zum Schließen des Dialogfelds **Verwaltungsgruppe hinzufügen** auf **OK**. Klicken Sie anschließend zum Schließen des Dialogfelds **Microsoft Monitoring Agenteigenschaften** auf **OK**.

## Konfigurieren von Agents zum Verwenden des OMS Log Analytics Forwarder (optional)

Wenn Sie über Server oder Clients verfügen, die nicht mit dem Internet verbunden sind, können Sie den OMS Log Analytics Forwarder verwenden, um Daten an OMS zu übermitteln. Wenn Sie den Forwarder verwenden, werden alle Daten von Agents über einen einzelnen Server gesendet, der Zugriff auf das Internet hat. Der Forwarder überträgt Daten von den Agents direkt an OMS, ohne dass die übertragenen Daten analysiert werden.

Weitere Informationen zum Forwarder, einschließlich Setup und Konfiguration, finden Sie unter [OMS Log Analytics Forwarder](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder).

Informationen zur Konfiguration Ihrer Agents zur Verwendung eines Proxyservers (in diesem Fall der OMS Forwarder) finden Sie unter [Configure proxy and firewall settings in Log Analytics](log-analytics-proxy-firewall.md) (Konfigurieren von Proxy- und Firewalleinstellungen in Log Analytics).

## Konfigurieren von Proxy- und Firewall-Einstellungen (optional)
Wenn sich in Ihrer Umgebung Proxyserver oder Firewalls befinden, die den Zugriff auf das Internet einschränken, finden Sie unter [Configure proxy and firewall settings in Log Analytics](log-analytics-proxy-firewall.md) (Konfigurieren von Proxy- und Firewalleinstellungen in Log Analytics) Informationen dazu, wie Sie die Kommunikation mit dem OMS-Dienst für Ihre Agents ermöglichen können.

## Nächste Schritte

- [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md) (Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog) beschreibt das Hinzufügen von Funktionen und das Sammeln von Daten.
- [Configure proxy and firewall settings in Log Analytics](log-analytics-proxy-firewall.md) (Konfigurieren von Proxy- und Firewalleinstellungen in Log Analytics) beschreibt, wie Sie vorgehen müssen, damit die Agents mit dem Log Analytics-Dienst kommunizieren können, wenn Ihre Organisation einen Proxyserver oder eine Firewall verwendet.

<!---HONumber=AcomDC_0504_2016-->