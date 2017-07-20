---
title: Verbinden von Windows-Computern mit Azure Log Analytics | Microsoft Docs
description: "Dieser Artikel beschreibt die Schritte, mit denen Sie die Windows-Computer in Ihrer lokalen Infrastruktur mit dem Log Analytics-Dienst verbinden können, indem Sie eine benutzerdefinierte Version des Microsoft Monitoring Agents (MMA) verwenden."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 932f7b8c-485c-40c1-98e3-7d4c560876d2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 48a0eaeb10d406d551c9e5870edde06809bd7544
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017


---
# <a name="connect-windows-computers-to-the-log-analytics-service-in-azure"></a>Verbinden von Windows-Computern mit dem Log Analytics-Dienst in Azure

Dieser Artikel beschreibt die Schritte, mit denen Sie Windows-Computer in Ihrer lokalen Infrastruktur mit OMS-Arbeitsbereichen verbinden können, indem Sie eine benutzerdefinierte Version des Microsoft Monitoring Agents (MMA) verwenden. Sie müssen Agents für alle Computer, die aufgenommen werden sollen, installieren und verbinden, damit diese Daten an den Log Analytics-Dienst übermitteln und um diese Daten anzuzeigen und zu bearbeiten. Jeder Agent kann Berichte zu mehreren Arbeitsbereichen erstellen.

Sie können Agents über das Setup, eine Befehlszeile oder durch eine Konfiguration des gewünschten Zustands in Azure Automation installieren.  

>[!NOTE]
Für virtuelle Computer in Azure können Sie die Installation vereinfachen, indem Sie die [VM-Erweiterung](log-analytics-azure-vm-extension.md) verwenden.

Auf Computern mit Internetverbindung nutzt der Agent die Internetverbindung, um Daten an OMS zu senden. Bei Computern ohne Internetverbindung können Sie einen Proxy oder das [OMS-Gateway](log-analytics-oms-gateway.md) verwenden.

Sie können Ihre Windows-Computer ganz einfach mit OMS verbinden, indem Sie die drei folgenden Schritte ausführen:

1. Laden Sie OMS-Portal die Setup-Datei für den Agent herunter.
2. Installieren Sie den Agent mit der von Ihnen bevorzugten Methode.
3. Konfigurieren Sie den Agent, oder fügen Sie bei Bedarf zusätzliche Arbeitsbereiche hinzu.

Das folgende Diagramm zeigt die Beziehung zwischen Ihren Windows-Computern und OMS nach der Installation und Konfiguration der Agents.

![oms-direct-agent-diagram](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)

Wenn laut Ihren IT-Sicherheitsrichtlinien Computer in Ihrem Netzwerk keine Verbindung mit dem Internet herstellen dürfen, können Sie Ihre Computer so konfigurieren, dass eine Verbindung zum OMS-Gateway hergestellt wird. Weitere Informationen und Schritte zum Konfigurieren Ihrer Server für die Kommunikation mit dem OMS-Dienst über ein OMS-Gateway finden Sie unter [Verbinden von Computern mit der OMS über das OMS-Gateway](log-analytics-oms-gateway.md).

## <a name="system-requirements-and-required-configuration"></a>Systemanforderungen und erforderliche Konfiguration
Prüfen Sie vor der Installation oder Bereitstellung der Agents die folgenden Details, um sicherzustellen, dass die Anforderungen erfüllt sind.

- Sie können den OMS MMA nur auf Computern installieren, auf denen Windows Server 2008 SP1 oder höher oder Windows 7 SP1 oder höher ausgeführt wird.
- Sie benötigen ein Azure-Abonnement.  Weitere Informationen finden Sie unter [Erste Schritte mit Log Analytics](log-analytics-get-started.md).
- Jeder Windows-Computer muss über HTTPS oder das OMS-Gateway eine Verbindung mit dem Internet herstellen können. Diese Verbindung kann direkt, über einen Proxy oder über das OMS-Gateway hergestellt werden.
- Sie können den OMS MMA auf eigenständigen Computern, Servern und virtuellen Computern installieren. Informationen zum Verbinden von in Azure gehosteten virtuellen Computern mit OMS finden Sie unter [Connect Azure virtual machines to Log Analytics](log-analytics-azure-vm-extension.md) (Herstellen einer Verbindung zwischen Azure-VMs und Log Analytics).
- Der Agent muss den TCP-Port 443 für verschiedene Ressourcen verwenden.

### <a name="network"></a>Netzwerk

Damit Windows-Agents eine Verbindung mit dem OMS-Dienst herstellen und sich bei diesem registrieren können, müssen sie Zugriff auf Netzwerkressourcen, einschließlich der Portnummern und Domänen-URLs, haben.

- Für Proxy-Server müssen Sie sicherstellen, dass die entsprechenden Proxy-Serverressourcen in Agenteinstellungen konfiguriert sind.
- Für Firewalls, die den Zugriff auf das Internet einschränken, müssen Ihre Netzwerktechniker die Firewall so konfigurieren, dass OMS Zugriff erhält. In den Agenteinstellungen ist keine Aktion erforderlich.

Die folgende Tabelle zeigt die für die Kommunikation erforderlichen Ressourcen.

>[!NOTE]
>In einigen der folgenden Ressourcen wird Operational Insights erwähnt, wobei es sich um einen früheren Namen von Log Analytics handelt.

| Agent-Ressource | Ports | Umgehung der HTTPS-Überprüfung |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Ja |
| *.oms.opinsights.azure.com | 443 | Ja |
| *.blob.core.windows.net | 443 | Ja |
| *.azure-automation.net | 443 | Ja |



## <a name="download-the-agent-setup-file-from-oms"></a>Herunterladen der Setup-Datei von OMS
1. Klicken Sie im OMS-Portal auf der Seite **Übersicht** auf die Kachel **Einstellungen**.  Klicken Sie oben auf die Registerkarte **Verbundene Datenquellen** .  
    ![Registerkarte „Verbundene Datenquellen“](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. Klicken Sie auf **Windows-Server** und dann auf **Windows-Agent herunterladen** für den entsprechenden Prozessortyp, um die Setupdatei herunterzuladen.
3. Klicken Sie rechts von **Arbeitsbereichs-ID**auf das Kopiersymbol, und fügen Sie die ID in den Editor ein.
4. Klicken Sie rechts von **Primärschlüssel**auf das Kopiersymbol, und fügen Sie den Schlüssel in den Editor ein.     

## <a name="install-the-agent-using-setup"></a>Installieren des Agents über Setup
1. Führen Sie Setup aus, um den Agent auf einem Computer zu installieren, den Sie verwalten möchten.
2. Klicken Sie auf der Seite **Willkommen**auf Weiter.
3. Lesen Sie die Seite „Lizenzbedingungen“ durch, und klicken Sie anschließend auf **Ich stimme zu**.
4. Auf der Seite „Zielordner“ können Sie den Standardinstallationsordner entweder ändern oder beibehalten. Klicken Sie anschließend auf **Weiter**.
5. Wählen Sie auf der Seite „Agent-Setupoptionen“ aus, ob der Agent mit Azure Log Analytics (OMS) oder Operations Manager verbunden werden soll. Sie können die Auswahl aber auch leer lassen, wenn Sie den Agent später konfigurieren möchten. Klicken Sie auf **Weiter**.
   
    - Wenn Sie eine Verbindung mit Azure Log Analytics (OMS) herstellen möchten, fügen Sie die **Arbeitsbereichs-ID** und den **Arbeitsbereichsschlüssel (Primärschlüssel)** ein, die Sie im vorherigen Schritt in Editor kopiert haben, und klicken Sie auf **Weiter**.  
        ![Arbeitsbereichs-ID und Primärschlüssel einfügen](./media/log-analytics-windows-agents/connect-workspace.png)
    - Wenn Sie eine Verbindung mit Operations Manager herstellen möchten, geben Sie den **Namen der Verwaltungsgruppe**, den Namen des **Verwaltungsservers** und den **Verwaltungsserverport** ein, und klicken Sie anschließend auf **Weiter**. Wählen Sie auf der Seite „Agentaktionskonto“ entweder das lokale Systemkonto oder ein lokales Domänenkonto aus, und klicken Sie auf **Weiter**.  
        ![Verwaltungsgruppenkonfiguration](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![Agentaktionskonto](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Überprüfen Sie Ihre Auswahl auf der Seite „Bereit zum Installieren“, und klicken Sie dann auf **Installieren**.
7. Klicken Sie auf der Seite „Die Konfiguration wurde erfolgreich abgeschlossen“ auf **Fertig stellen**.
8. Nach Abschluss wird der **Microsoft Monitoring Agent** in der **Systemsteuerung** angezeigt. Dort können Sie Ihre Konfiguration überprüfen und sicherstellen, dass der Agent mit Operational Insights (OMS) verbunden ist. Wenn die Verbindung mit OMS hergestellt ist, zeigt der Agent die folgende Meldung an: **The Microsoft Monitoring Agent has successfully connected to the Microsoft Operations Management Suite service**

## <a name="configure-proxy-settings"></a>Konfigurieren von Proxyeinstellungen

Mit dem folgenden Verfahren können Sie Proxyeinstellungen für den Microsoft Monitoring Agent über die Systemsteuerung konfigurieren. Sie müssen diese Schritte für jeden Server ausführen. Wenn Sie viele Server konfigurieren müssen, ist es möglicherweise einfacher, diesen Prozess mithilfe eines Skripts zu automatisieren. Wenn dies der Fall ist, finden Sie entsprechende Anweisungen unter [So konfigurieren Sie Proxyeinstellungen für den Microsoft Monitoring Agent mithilfe eines Skripts](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>So konfigurieren Sie Proxyeinstellungen für den Microsoft Monitoring Agent über die Systemsteuerung
1. Öffnen Sie die **Systemsteuerung**.
2. Öffnen Sie **Microsoft Monitoring Agent**.
3. Klicken Sie auf die Registerkarte **Proxyeinstellungen** .  
    ![Registerkarte „Proxyeinstellungen“](./media/log-analytics-windows-agents/proxy-direct-agent-proxy.png)
4. Wählen Sie **Proxyserver verwenden** aus, und geben Sie die URL und gegebenenfalls die Portnummer ein (siehe dazu das Beispiel oben). Wenn der Proxyserver eine Authentifizierung erfordert, geben Sie den Benutzernamen und das Kennwort für den Zugriff auf den Proxyserver ein.


### <a name="verify-agent-connectivity-to-oms"></a>Überprüfen der Agentkonnektivität mit OMS

Sie können mithilfe der folgenden Schritte problemlos überprüfen, ob die Agents mit OMS kommunizieren:

1.  Öffnen Sie auf dem Computer mit dem Windows-Agent-die Systemsteuerung.
2.  Öffnen Sie Microsoft Monitoring Agent.
3.  Klicken Sie auf die Registerkarte „Azure Log Analytics (OMS)“.
4.  In der Statusspalte sollte angezeigt werden, dass der Agent erfolgreich mit dem Operations Management Suite-Dienst verbunden wurde.

![Agent verbunden](./media/log-analytics-windows-agents/mma-connected.png)


### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a>So konfigurieren Sie Proxyeinstellungen für den Microsoft Monitoring Agent mithilfe eines Skripts
Kopieren Sie das folgende Beispiel, aktualisieren Sie es mit den tatsächlichen Informationen für Ihre Umgebung, und speichern Sie es mit der Dateinamenerweiterung PS1. Führen Sie das Skript dann auf jedem Computer aus, der eine direkte Verbindung mit dem OMS-Dienst herstellt.

    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get the Health Service configuration object.  We need to determine if we
    #have the right update rollup with the API we need.  If not, no need to run the rest of the script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)



## <a name="install-the-agent-using-the-command-line"></a>Installieren des Agents über die Befehlszeile
- Ändern Sie das folgende Beispiel, und verwenden Sie es dann zur Installation des Agents über die Befehlszeile. Das Beispiel führt eine vollständig automatische Installation aus.

    >[!NOTE]
    Wenn Sie einen Agent aktualisieren möchten, müssen Sie die Skripting-API für Log Analytics verwenden. Im nächsten Abschnitt finden Sie Informationen zum Aktualisieren eines Agents.

    ```dos
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

Der Agent verwendet IExpress als sein Self-Extractor mithilfe des `/c` Befehls. Weitere Informationen zu den Befehlszeilenswitches finden Sie unter [Befehlszeilenoptionen für IExpress-Softwareupdatepakete](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages). Sie können anschließend das Beispiel aktualisieren, um es an Ihre Bedürfnisse anzupassen.

|MMA-spezifische Optionen                   |Hinweise         |
|---------------------------------------|--------------|
|ADD_OPINSIGHTS_WORKSPACE               | 1 = Konfigurieren des Agents, sodass Berichte an einen Arbeitsbereich übermittelt werden                |
|OPINSIGHTS_WORKSPACE_ID                | Arbeitsbereichs-ID (GUID) für den hinzuzufügenden Arbeitsbereich                    |
|OPINSIGHTS_WORKSPACE_KEY               | Arbeitsbereichsschlüssel, der für die erste Authentifizierung beim Arbeitsbereich verwendet wird |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Angabe der Cloudumgebung, in der sich der Arbeitsbereich befindet <br> 0 = Azure Commercial Cloud (Standard) <br> 1 = Azure Government |
|OPINSIGHTS_PROXY_URL               | URI für den zu verwendenden Proxy |
|OPINSIGHTS_PROXY_USERNAME               | Benutzername für den Zugriff auf einen authentifizierten Proxy |
|OPINSIGHTS_PROXY_PASSWORD               | Kennwort für den Zugriff auf einen authentifizierten Proxy |

>[!NOTE]
Um zu vermeiden, dass Sie die Längenbegrenzung der Befehlszeile von IExpress erreichen, installieren Sie den Agent ohne konfigurierten Arbeitsbereich, und verwenden Sie dann ein Skript, um die Konfiguration für den Arbeitsbereich festzulegen.

>[!NOTE]
Falls `Command line option syntax error.` angezeigt wird, wenn Sie den Parameter `OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE` verwenden, können Sie folgende Problemumgehung verwenden:
```dos
MMASetup-AMD64.exe /C /T:.\MMAExtract
cd .\MMAExtract
setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
```

## <a name="add-a-workspace-using-a-script"></a>Hinzufügen eines Arbeitsbereichs mithilfe eines Skripts
Fügen Sie einen Arbeitsbereich hinzu, indem Sie die Skripting-API des Log Analytics-Agents mit dem folgenden Beispiel verwenden:

```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

Verwenden Sie das folgende Beispielskript, um einen Arbeitsbereich in Azure für die US-Regierung hinzuzufügen:
```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
Wenn Sie bereits die Befehlszeile oder Skripts zum Installieren oder Konfigurieren des Agents verwendet haben, sollten Sie wissen, dass `EnableAzureOperationalInsights` durch `AddCloudWorkspace` ersetzt wurde.

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Installieren des Agents mithilfe von DSC in Azure Automation

Sie können das folgende Skriptbeispiel verwenden, um den Agent über DSC in Azure Automation zu installieren. Mit diesem Beispiel wird der vom `URI`-Wert identifizierte 64-Bit-Agent installiert. Sie können auch die 32-Bit-Version installieren, indem Sie den URI-Wert ersetzen. Die URIs für beide Versionen lauten:

- Windows-64-Bit-Agent: https://go.microsoft.com/fwlink/?LinkId=828603
- Windows-32-Bit-Agent: https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
Mit diesem Beispielverfahren und -skript wird kein vorhandener Agent aktualisiert.

1. Importieren Sie das DSC-Modul xPSDesiredStateConfiguration von [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) in Azure Automation.  
2.  Erstellen Sie Variablenassets in Azure Automation für *OPSINSIGHTS_WS_ID* und *OPSINSIGHTS_WS_KEY*. Legen Sie für *OPSINSIGHTS_WS_ID* Ihre OMS Log Analytics-Arbeitsbereichs-ID und für *OPSINSIGHTS_WS_KEY* den Primärschlüssel Ihres Arbeitsbereichs fest.
3.  Verwenden Sie das folgende Skript, und speichern Sie es als MMAgent.ps1
4.  Ändern Sie das folgende Beispiel, und verwenden Sie es dann zur Installation des Agents über DSC in Azure Automation. Importieren Sie „MMAgent.ps1“ in Azure Automation über die Azure Automation-Schnittstelle oder das Cmdlet.
5.  Weisen Sie der Konfiguration einen Knoten zu. Innerhalb von 15 Minuten prüft der Knoten die Konfiguration, und der MMA wird per Pushvorgang auf den Knoten übertragen.

```PowerShell
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
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}


```

### <a name="get-the-latest-productid-value"></a>Abrufen des aktuellen ProductId-Werts

Der `ProductId value` im Skript „MMAgent.ps1“ ist für jede Agent-Version eindeutig. Wenn eine aktualisierte Version eines Agents veröffentlicht wird, ändert sich der ProductId-Wert. Wenn sich der ProductId-Wert also künftig ändert, können Sie die Agent-Version mithilfe eines einfachen Skripts suchen. Nachdem Sie die aktuelle Version des Agents auf einem Testserver installiert haben, können Sie mit dem folgenden Skript den ProductId-Wert der Installation abrufen. Mithilfe des aktuellen ProductId-Werts können Sie den Wert im Skript „MMAgent.ps1“ aktualisieren.

```PowerShell
$InstalledApplications  = Get-ChildItem hklm:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall


foreach ($Application in $InstalledApplications)

{

     $Key = Get-ItemProperty $Application.PSPath

     if ($Key.DisplayName -eq "Microsoft Monitoring Agent")

     {

        $Key.DisplayName

        Write-Output ("Product ID is: " + $Key.PSChildName.Substring(1,$Key.PSChildName.Length -2))

     }

}  
```

## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Manuelles Konfigurieren eines Agents oder Hinzufügen zusätzlicher Arbeitsbereiche
Wenn Sie Agents installiert, aber noch nicht konfiguriert haben, oder wenn der Agent Berichte zu mehreren Arbeitsbereichen erstellen soll, können Sie die Agents mithilfe der folgenden Informationen aktivieren oder neu konfigurieren. Nachdem Sie den Agent konfiguriert haben, registriert dieser sich beim Agent-Dienst und ruft die erforderlichen Konfigurationsinformationen und Management Packs ab, die die Lösung enthalten.

1. Öffnen Sie nach der Installation des Microsoft Monitoring Agents die **Systemsteuerung**.
2. Öffnen Sie **Microsoft Monitoring Agent**, und klicken Sie dann auf die Registerkarte **Azure Log Analytics (OMS)**.   
3. Klicken Sie auf **Hinzufügen**, um das Feld **Log Analytics-Arbeitsbereich hinzufügen** zu öffnen.
4. Fügen Sie die **Arbeitsbereichs-ID** und den **Arbeitsbereichsschlüssel (Primärschlüssel)** des Arbeitsbereichs ein, den Sie hinzufügen möchten. (Sie hatten diese Informationen zuvor in Editor kopiert.) Klicken Sie anschließend auf **OK**.  
    ![Operational Insights konfigurieren](./media/log-analytics-windows-agents/add-workspace.png)

Nachdem die Daten von Computern erfasst wurden, die vom Agent überwacht werden, wird die Anzahl der von OMS überwachten Computer im OMS-Portal auf der Registerkarte **Verbundene Datenquellen** in den **Einstellungen** unter **Server verbunden** angezeigt.


## <a name="to-disable-an-agent"></a>So deaktivieren Sie einen Agent
1. Öffnen Sie nach der Installation des Agents die **Systemsteuerung**.
2. Öffnen Sie Microsoft Monitoring Agent, und klicken Sie dann auf die Registerkarte **Azure Log Analytics (OMS)** .
3. Wählen Sie einen Arbeitsbereich aus, und klicken Sie dann auf **Entfernen**. Wiederholen Sie diesen Schritt für alle anderen Arbeitsbereiche.


## <a name="optionally-configure-agents-to-report-to-an-operations-manager-management-group"></a>Konfigurieren von Agents zum Berichten an eine Operations Manager-Verwaltungsgruppe (optional)

Wenn Sie Operations Manager in Ihrer IT-Infrastruktur verwenden, können Sie auch den MMA-Agent als Operations Manager-Agent nutzen.

### <a name="to-configure-mma-agents-to-report-to-an-operations-manager-management-group"></a>So konfigurieren Sie MMA-Agents zum Berichten an eine Operations Manager-Verwaltungsgruppe
1.  Öffnen Sie auf dem Computer, auf dem der Agent installiert ist, die **Systemsteuerung**.  
2.  Öffnen Sie **Microsoft Monitoring Agent**, und klicken Sie dann auf die Registerkarte **Operations Manager**.  
    ![Microsoft Monitoring Agent – Registerkarte „Operations Manager“](./media/log-analytics-windows-agents/om-mg01.png)
3.  Wenn Ihre Operations Manager-Server über eine Integration mit Active Directory verfügen, klicken Sie auf **Verwaltungsgruppenzuweisungen aus AD DS automatisch aktualisieren**.
4.  Klicken Sie auf **Hinzufügen**, um das Dialogfeld **Verwaltungsgruppe hinzufügen** zu öffnen.  
    ![Microsoft Monitoring Agent – Verwaltungsgruppe hinzufügen](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  Geben Sie im Feld **Verwaltungsgruppenname** den Namen Ihrer Verwaltungsgruppe ein.
6.  Geben Sie im Feld **Primärer Verwaltungsserver** den Computernamen des primären Verwaltungsservers ein.
7.  Geben Sie im Feld **Verwaltungsserverport** die TCP-Portnummer ein.
8.  Wählen Sie unter **Agentaktionskonto**das lokale Systemkonto oder ein lokales Domänenkonto aus.
9.  Klicken Sie zum Schließen des Dialogfelds **Verwaltungsgruppe hinzufügen** auf **OK**. Klicken Sie anschließend zum Schließen des Dialogfelds **Eigenschaften von Microsoft Monitoring Agent** auf **OK**.


## <a name="next-steps"></a>Nächste Schritte

- [Fügen Sie Log Analytics-Lösungen aus dem Lösungskatalog hinzu](log-analytics-add-solutions.md) , um Funktionen hinzuzufügen und Daten zu sammeln.

