---
title: "Azure Automation – Hybrid Runbook Worker | Microsoft Docs"
description: "In diesem Artikel werden Informationen zum Installieren und Verwenden der Azure Automation-Funktion „Hybrid Runbook Worker“ bereitgestellt, mit der Sie Runbooks auf Computern in Ihrem lokalen Rechenzentrum oder Cloudanbieter ausführen können."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 06227cda-f3d1-47fe-b3f8-436d2b9d81ee
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/05/2017
ms.author: magoedte;bwren
ms.translationtype: HT
ms.sourcegitcommit: f9003c65d1818952c6a019f81080d595791f63bf
ms.openlocfilehash: 783fb22b0154915f2e3d8574ab95538dbd646705
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---

# <a name="automate-resources-in-your-data-center-or-cloud-with-hybrid-runbook-worker"></a>Automatisieren von Ressourcen im Rechenzentrum oder in der Cloud mit Hybrid Runbook Worker
Runbooks in Azure Automation können nicht auf Ressourcen in anderen Clouds oder in Ihrer lokalen Umgebung zugreifen, da sie in der Azure-Cloud ausgeführt werden.  Die Azure Automation-Funktion „Hybrid Runbook Worker“ ermöglicht das Ausführen von Runbooks direkt auf dem Computer, der die Rolle hostet, und außerhalb für Ressourcen in der Umgebung zur Verwaltung dieser lokalen Ressourcen. Runbooks werden in Azure Automation gespeichert und verwaltet und an einen oder mehrere dafür vorgesehene Computer übermittelt.  

Diese Funktionalität wird in der folgenden Abbildung veranschaulicht:<br>  

![Hybrid-Runbook-Worker – Übersicht](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Eine technische Übersicht über die Hybrid Runbook Worker-Rolle und Berücksichtigungen bei der Bereitstellung erhalten Sie unter [Übersicht über die Automation-Architektur](automation-offering-get-started.md#automation-architecture-overview).    

## <a name="hybrid-runbook-worker-groups"></a>Hybrid-Runbook-Workergruppen
Jeder Hybrid-Runbook-Worker ist ein Mitglied einer Hybrid-Runbook-Workergruppe, die Sie beim Installieren des Agents angeben.  Eine Gruppe kann einen einzelnen Agent umfassen, aber für eine höhere Verfügbarkeit können Sie mehrere Agents installieren.

Wenn Sie ein Runbook auf einen Hybrid Runbook Worker starten, geben Sie die Gruppe an, in der das Runbook ausgeführt werden soll.  Die Mitglieder der Gruppe legen fest, welcher Worker die Anforderung verarbeitet.  Sie können keinen bestimmten Worker angeben.

## <a name="relationship-to-service-management-automation"></a>Beziehung zu Service Management Automation (SMA)
Mit [Service Management Automation (SMA)](https://technet.microsoft.com/library/dn469260.aspx) können Sie dieselben Runbooks, die von Azure Automation unterstützt werden, in Ihrem lokalen Rechenzentrum ausführen. SMA wird in der Regel mit Windows Azure- Pack bereitgestellt, da Windows Azure Pack eine grafische Benutzeroberfläche für die SMA-Verwaltung enthält. Im Gegensatz zu Azure Automation erfordert SMA eine lokale Installation, die Webserver zum Hosten der API, eine Datenbank für Runbooks und SMA-Konfiguration sowie Runbook Worker zum Ausführen von Aufträgen enthält. Azure Automation stellt diese Dienste in der Cloud bereit, Sie müssen lediglich die Hybrid-Runbook-Worker in Ihrer lokalen Umgebung verwalten.

Wenn Sie SMA bereits verwenden, können Sie Ihre Runbooks in Azure Automation verschieben, damit diese ohne Änderungen mit Hybrid Runbook Worker verwendet werden. Hierbei wird vorausgesetzt, dass eine eigene Authentifizierung gegenüber Ressourcen durchgeführt wird, wie unter [Running runbooks on a Hybrid Runbook Worker (Ausführen von Runbooks auf einem Hybrid Runbook Worker)](automation-hrw-run-runbooks.md) beschrieben.  Runbooks werden in SMA im Kontext des Dienstkontos auf dem Workerserver ausgeführt, der möglicherweise die Authentifizierung für die Runbooks bereitstellt.

Sie können anhand der folgenden Kriterien prüfen, ob Azure Automation mit Hybrid-Runbook-Worker oder Service Management Automation die bessere Wahl für Ihre Anforderungen ist.

* SMA erfordert eine lokale Installation von zugrunde liegenden Komponenten, die mit Windows Azure Pack verbunden sind, wenn eine grafische Verwaltungsschnittstelle erforderlich ist. Darüber hinaus benötigt SMA mehr lokale Ressourcen mit höheren Wartungskosten als Azure Automation, bei dem nur ein Agent auf den lokalen Runbookworkern installiert werden muss. Die Agents werden von Operations Management Suite verwaltet, wodurch sich Ihre Wartungskosten verringern.
* Azure Automation speichert Runbooks in der Cloud und übermittelt diese an lokale Hybrid-Runbook-Worker. Wenn Ihre Sicherheitsrichtlinie dieses Verhalten nicht zulässt, sollten Sie SMA verwenden.
* SMA ist in System Center enthalten. und erfordert daher eine System Center 2012 R2-Lizenz erforderlich. Azure Automation basiert auf einem mehrstufigen Abonnementmodell.
* Azure Automation bietet erweiterte Funktionen wie z. B. grafische Runbooks, die in SMA nicht verfügbar sind.

## <a name="installing-the-windows-hybrid-runbook-worker"></a>Installieren der Windows Hybrid Runbook Worker 

Zum Installieren und Konfigurieren eines Windows Hybrid Runbook Worker stehen zwei Methoden zur Verfügung.  Die empfohlene Methode besteht darin, ein Automation-Runbook zu verwenden, um den Prozess, der zum Konfigurieren eines Windows-Computers erforderlich ist, vollständig zu automatisieren.  Bei der zweiten Methode wird die Rolle Schritt für Schritt manuell installiert und konfiguriert.  

> [!NOTE]
> Um die Konfiguration der Server zu verwalten, die die Hybrid Runbook Worker-Rolle mit Desired State Configuration (DSC) unterstützen, müssen Sie die Server als DSC-Knoten hinzufügen.  Weitere Informationen zu ihrer Integration für die Verwaltung durch DSC finden Sie unter [Integrieren von Computern für die Verwaltung durch Azure Automation DSC](automation-dsc-onboarding.md).           
><br>
>Wenn Sie die [Lösung für die Updateverwaltung](../operations-management-suite/oms-solution-update-management.md) aktivieren, werden alle mit dem OMS-Arbeitsbereich verbundenen Windows-Computer automatisch als Hybrid Runbook Worker konfiguriert, um Runbooks zu unterstützen, die in dieser Lösung enthalten sind.  Es wird aber keine Registrierung für Hybrid Worker-Gruppen durchgeführt, die in Ihrem Automation-Konto bereits definiert wurden.  Der Computer kann einer Hybrid Runbook Worker-Gruppe in Ihrem Automation-Konto hinzugefügt werden, um Automation-Runbooks zu unterstützen, solange Sie sowohl für die Lösung als auch die Mitgliedschaft in der Hybrid Runbook Worker-Gruppe dasselbe Konto verwenden.  Diese Funktionalität wurde Version 7.2.12024.0 des Hybrid Runbook Worker hinzugefügt.  

Lesen Sie sich vor dem Bereitstellen eines Hybrid Runbook Worker die folgenden Informationen zu [Hardware- und Softwareanforderungen](automation-offering-get-started.md#hybrid-runbook-worker) und [Informationen zur Vorbereitung Ihres Netzwerks](automation-offering-get-started.md#network-planning) durch.  Lesen Sie sich nach dem erfolgreichen Bereitstellen eines Runbook Workers [Running runbooks on a Hybrid Runbook Worker (Ausführen von Runbooks auf einem Hybrid Runbook Worker)](automation-hrw-run-runbooks.md) durch, um zu erfahren, wie Sie Ihre Runbooks für die Automatisierung von Prozessen in Ihrem lokalen Rechenzentrum oder in einer anderen Cloudumgebung konfigurieren.  
 
### <a name="automated-deployment"></a>Automatisierte Bereitstellung

Führen Sie die folgenden Schritte aus, um die Installation und Konfiguration der Windows Hybrid Worker-Rolle zu automatisieren.  

1. Laden Sie das Skript *New-OnPremiseHybridWorker.ps1* aus dem [PowerShell-Katalog](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker/1.0/DisplayScript) direkt über den Computer herunter, auf dem die Hybrid Runbook Worker-Rolle ausgeführt wird, oder laden Sie es über einen anderen Computer in Ihrer Umgebung herunter, und kopieren sie es auf den Worker.  

    Das Skript *New-OnPremiseHybridWorker.ps1* erfordert während der Ausführung folgende Parameter:

  * *AutomationAccountName* (obligatorisch): Der Name Ihres Automation-Kontos.  
  * *ResourceGroupName* (obligatorisch): Der Name der Ressourcengruppe, die Ihrem Automation-Konto zugeordnet ist.  
  * *HybridGroupName* (obligatorisch): Der Name einer Hybrid Runbook Worker-Gruppe, die Sie als Ziel für die Runbooks angeben, die dieses Szenario unterstützen. 
  *  *SubscriptionID* (obligatorisch): Die Azure-Abonnement-ID Ihres Automation-Kontos.
  *  *WorkspaceName* (optional): Der Name des OMS-Arbeitsbereichs.  Falls kein OMS-Arbeitsbereich vorhanden ist, wird dieser durch das Skript erstellt und konfiguriert.  

     > [!NOTE]
     > Derzeit werden für die Automation-OMS-Integration lediglich folgende Regionen unterstützt: **Australien, Südosten**, **USA, Osten 2**, **Asien, Südosten** und **Europa, Westen**.  Falls sich Ihr Automation-Konto nicht in einer dieser Regionen befindet, erstellt das Skript zwar einen OMS-Arbeitsbereich, weist Sie aber in einer Warnung darauf hin, dass keine Verknüpfung möglich ist.
     > 
2. Starten Sie auf Ihrem Computer **Windows PowerShell** über den **Startbildschirm** im Administratormodus.  
3. Navigieren Sie über die PowerShell-Befehlszeilenshell zu dem Ordner mit dem heruntergeladenen Skript, und führen Sie es aus. Ändern Sie dabei die Werte für die Parameter *-AutomationAccountName*, *-ResourceGroupName*, *-HybridGroupName*, *-SubscriptionId* und *-WorkspaceName*.

     > [!NOTE] 
     > Nach dem Ausführen des Skripts werden Sie aufgefordert, sich bei Azure zu authentifizieren.  Sie **müssen** sich mit einem Konto anmelden, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist.  
     >  
    
        .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> `
        -ResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
        -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfOMSWorkspace>

4. Sie werden aufgefordert, der Installation von **NuGet** zuzustimmen und sich mit Ihren Azure-Anmeldeinformationen zu authentifizieren.<br><br> ![Ausführen des Skripts „New-OnPremiseHybridWorker“](media/automation-hybrid-runbook-worker/new-onpremisehybridworker-scriptoutput.png)

5. Nach vollständiger Ausführung des Skripts werden auf dem Blatt „Hybrid Worker-Gruppen“ die neue Gruppe und die Anzahl von Mitgliedern angezeigt. Im Falle einer bereits vorhandenen Gruppe wird die Anzahl der Mitglieder erhöht.  Sie können die Gruppe aus der Liste auf dem Blatt **Hybrid Worker-Gruppen** auswählen und dann die Kachel **Hybrid Worker** auswählen.  Auf dem Blatt **Hybrid Worker** werden die einzelnen Mitglieder der Gruppe aufgelistet.  

### <a name="manual-deployment"></a>Manuelle Bereitstellung 
Führen Sie die ersten beiden Schritte einmal für Ihre Automation-Umgebung durch, und wiederholen Sie dann die verbleibenden Schritte für jeden Workercomputer.

#### <a name="1-create-operations-management-suite-workspace"></a>1. Erstellen eines Operations Management Suite-Arbeitsbereichs
Sofern Sie noch nicht über einen Operations Management Suite-Arbeitsbereich verfügen, erstellen Sie diesen mithilfe der Anweisungen unter [Verwalten von Arbeitsbereichen](../log-analytics/log-analytics-manage-access.md). Wenn Sie bereits über einen Arbeitsbereich verfügen, können Sie diesen verwenden.

#### <a name="2-add-automation-solution-to-operations-management-suite-workspace"></a>2. Hinzufügen der Automation-Lösung zum Operations Management Suite-Arbeitsbereich
Die Funktionalität von Operations Management Suite kann durch Lösungen erweitert werden.  Die Automation-Lösung fügt Azure Automation Funktionalität hinzu, einschließlich der Unterstützung für Hybrid Runbook Worker.  Wenn Sie die Lösung Ihrem Arbeitsbereich hinzufügen, werden Workerkomponenten automatisch per Push auf den Agent-Computer übertragen, den Sie im nächsten Schritt installieren werden.

Befolgen Sie die Anweisungen unter [Hinzufügen einer Lösung über den Lösungskatalog](../log-analytics/log-analytics-add-solutions.md) , um Ihrem Operations Management Suite-Arbeitsbereich die Lösung **Automation** hinzuzufügen.

#### <a name="3-install-the-microsoft-monitoring-agent"></a>3. Installieren von Microsoft Monitoring Agent
Microsoft Monitoring Agent verbindet Computer mit Operations Management Suite.  Wenn Sie den Agent auf dem lokalen Computer installieren und mit Ihrem Arbeitsbereich verbinden, werden automatisch die erforderlichen Komponenten für Hybrid Runbook Worker heruntergeladen.

Befolgen Sie die Anweisungen unter [Verbinden von Windows-Computern mit Log Analytics](../log-analytics/log-analytics-windows-agents.md) , um den Agent auf dem lokalen Computer zu installieren.  Sie können diesen Vorgang für mehrere Computer wiederholen, um Ihrer Umgebung mehrere Worker hinzuzufügen.

Wenn der Agent erfolgreich mit Operations Management Suite verbunden ist, wird er im Operations Management Suite-Bereich **Einstellungen** auf der Registerkarte **Verbundene Datenquellen** aufgeführt.  Sie können sicherstellen, dass der Agent die Automation-Lösung ordnungsgemäß heruntergeladen hat, indem Sie prüfen, ob unter „C:\Programme\Microsoft Monitoring Agent\Agent“ ein Ordner namens **AzureAutomationFiles** vorhanden ist.  Unter „C:\Programme\Microsoft Monitoring Agent\Agent\AzureAutomation\“ im Unterordner \\*version* können Sie die Hybrid Runbook Worker-Version überprüfen.   

#### <a name="4-install-the-runbook-environment-and-connect-to-azure-automation"></a>4. Installieren der Runbookumgebung und Verbindungsherstellung mit Azure Automation
Wenn Sie der Operations Management Suite einen Agent hinzufügen, lädt die Automation-Lösung per Push das PowerShell-Modul **HybridRegistration** herunter, in dem das Cmdlet **Add-HybridRunbookWorker** enthalten ist.  Sie verwenden dieses Cmdlet zum Installieren der Runbookumgebung auf dem Computer und registrieren diesen bei Azure Automation.

Öffnen Sie eine PowerShell-Sitzung im Administratormodus, und führen Sie die folgenden Befehle zum Importieren des Moduls aus.

    cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
    Import-Module HybridRegistration.psd1

Führen Sie dann das Cmdlet **Add-HybridRunbookWorker** mit der folgenden Syntax aus:

    Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>

Sie finden die für dieses Cmdlet erforderlichen Informationen auf dem Blatt **Schlüssel verwalten** im Azure-Portal.  Öffnen Sie dieses Blatt, indem Sie in Ihrem Automation-Konto auf dem Blatt **Einstellungen** die Option **Schlüssel** auswählen.

![Hybrid-Runbook-Worker – Übersicht](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* **GroupName** ist der Name der Hybrid Runbook Worker-Gruppe. Wenn die Gruppe bereits im Automation-Konto vorhanden ist, wird der aktuelle Computer der Gruppe hinzugefügt.  Ist die Gruppe nicht vorhanden, wird sie hinzugefügt.
* **EndPoint** ist das Feld **URL** auf dem Blatt **Schlüssel verwalten**.
* **Token** ist der **Primäre Zugriffsschlüssel** im Blatt **Schlüssel verwalten**.  

Verwenden Sie den Schalter **-Verbose** mit **Add-HybridRunbookWorker**, um detaillierte Informationen über die Installation zu erhalten.

#### <a name="5-install-powershell-modules"></a>5. Installieren von PowerShell-Modulen
Runbooks können beliebige Aktivitäten und Cmdlets der Module verwenden, die Sie in Ihrer Azure Automation-Umgebung installiert haben.  Diese Module werden nicht automatisch auf den lokalen Computern bereitgestellt, sie müssen manuell installiert werden.  Eine Ausnahme stellt das standardmäßig installierte Azure-Modul dar. Es bietet Zugriff auf Cmdlets für alle Azure-Dienste und -Aktivitäten für Azure Automation.

Da der primäre Zweck der Hybrid Runbook Worker-Funktion darin besteht, lokale Ressourcen zu verwalten, müssen Sie sehr wahrscheinlich die Module zur Unterstützung dieser Ressourcen installieren.  Unter [Installieren von Modulen](http://msdn.microsoft.com/library/dd878350.aspx) finden Sie Informationen zum Installieren von PowerShell-Modulen.  Installierte Module müssen sich in einem Speicherort befinden, auf den von der PSModulePath-Umgebungsvariable verwiesen wird, damit sie vom Hybrid Worker automatisch importiert werden.  Weitere Informationen finden Sie unter [Modifying the PSModulePath Installation Path](https://msdn.microsoft.com/library/dd878326%28v=vs.85%29.aspx) (Ändern des PSModulePath-Installationspfads). 

## <a name="installing-linux-hybrid-runbook-worker"></a>Installieren eines Linux Hybrid Runbook Worker
Die Installation eines Hybrid Runbook Worker unter Linux ist sehr unkomplizierter Vorgang zur manuellen Installation und Konfiguration der Rolle.  Dazu ist die Aktivierung der Lösung **Automation Hybrid Worker** in Ihrem OMS-Arbeitsbereich erforderlich sowie das Ausführen einiger Befehle, um den Computer als Worker zu registrieren und ihn zu einer neuen oder vorhandenen Gruppe hinzuzufügen. 

1.  Aktivieren Sie die Lösung „Automation Hybrid Worker“ in OMS. Dies kann durch Folgendes geschehen:

   1. Aktivieren Sie die Lösung **Automation Hybrid Worker** im Lösungskatalog des [OMS-Portals](https://mms.microsoft.com).
   2. Führen Sie das folgende Cmdlet aus:

        ```$null = Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName "AzureAutomation" -Enabled $true
        ```
2.  Run the following command with the proper parameters (endpoint and key can be taken from the portal from the automation account linked to the workspace used in the steps above):
sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <OMSworkspaceId> -k <automationsharedkey> --groupname <hybridgroupname> -e <automationendpoint>


## Removing Hybrid Runbook Worker 
You can remove one or more Hybrid Runbook Workers from a group or you can remove the group, depending on your requirements.  To remove a Hybrid Runbook Worker from an on-premises computer, perform the following steps.

1. In the Azure portal, navigate to your Automation account.  
2. From the **Settings** blade, select **Keys** and note the values for field **URL** and **Primary Access Key**.  You need this information for the next step.
3. Open a PowerShell session in Administrator mode and run the following command - `Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>`.  Use the **-Verbose** switch for a detailed log of the removal process.

> [!NOTE]
> This does not remove the Microsoft Monitoring Agent from the computer, only the functionality and configuration of the Hybrid Runbook Worker role.  

## Remove Hybrid Worker groups
To remove a group, you first need to remove the Hybrid Runbook Worker from every computer that is a member of the group using the procedure shown earlier, and then you perform the following steps to remove the group.  

1. Open the Automation account in the Azure portal.
2. Select the **Hybrid Worker Groups** tile and in the **Hybrid Worker Groups** blade, select the group you wish to delete.  After selecting the specific group, the **Hybrid worker group** properties blade is displayed.<br> ![Hybrid Runbook Worker Group Blade](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)   
3. On the properties blade for the selected group, click **Delete**.  A message appears asking you to confirm this action, select **Yes** if you are sure you want to proceed.<br> ![Delete Group Confirmation Dialog](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)<br> This process can take several seconds to complete and you can track its progress under **Notifications** from the menu.  

## Troubleshooting 
The Hybrid Runbook Worker depends on the Microsoft Monitoring Agent to communicate with your Automation account to register the worker, receive runbook jobs, and report status. If  registration of the worker fails, here are some possible causes for the error:  

1. The hybrid worker is behind a proxy or firewall.  
    Verify the computer has outbound access to *.azure-automation.net on port 443.  

2. The computer the hybrid worker is running on has less than the minimum hardware [requirements](automation-offering-get-started.md#hybrid-runbook-worker).  
    Computers running the Hybrid Runbook Worker should meet the minimum hardware requirements before designating it to host this feature. Otherwise, depending on the resource utilization of other background processes and contention caused by runbooks during execution, the computer will become over utilized and cause runbook job delays or timeouts.
    Confirm the computer designated to run the Hybrid Runbook Worker feature meets the minimum hardware requirements.  If it does, monitor CPU and memory utilization to determine any correlation between the performance of Hybrid Runbook Worker processes and Windows.  If there is memory or CPU pressure, this may indicate the need to upgrade or add additional processors, or increase memory to address the resource bottleneck and resolve the error. Alternatively, select a different compute resource that can support the minimum requirements and scale when workload demands indicate an increase is necessary.
    
3. The Microsoft Monitoring Agent service is not running.  
    If the Microsoft Monitoring Agent Windows service is not running, this prevents the Hybrid Runbook Worker from communicating with Azure Automation.  Verify the agent is running by entering the following command in PowerShell: `get-service healthservice`.  If the service is stopped, enter the following command in PowerShell to start the service: `start-service healthservice`.  

4. In the **Application and Services Logs\Operations Manager** event log, you see event 4502  and EventMessage containing **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent** with the following description:  *The certificate presented by the service <wsid>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Please contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication. The article KB3126513 has additional troubleshooting information for connectivity issues.*
    This can be caused by your proxy or network firewall blockking communication to Microsoft Azure.  Verify the computer has outbound access to *.azure-automation.net on ports 443.

Logs are stored locally on each hybrid worker at C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.  You can check if there are any warning or error events written to the **Application and Services Logs\Microsoft-SMA\Operations** and **Application and Services Logs\Operations Manager** event log that would indicate a connectivity or other issue affecting onboarding of the role to Azure Automation or issue while performing normal operations.  

## Next steps
Review [run runbooks on a Hybrid Runbook Worker](automation-hrw-run-runbooks.md) to learn how to configure your runbooks to automate processes in your on-premises datacenter or other cloud environment.

