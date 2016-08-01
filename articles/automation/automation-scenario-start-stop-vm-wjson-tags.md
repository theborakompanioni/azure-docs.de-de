<properties
   pageTitle="Verwendung von JSON-formatierten Tags zum Erstellen eines Zeitplans für Starten und Herunterfahren einer Azure-VM | Microsoft Azure"
   description="Dieser Artikel beschreibt die Verwendung von JSON-Zeichenfolgen für Tags zum Automatisieren der Planung des Startens und Herunterfahrens einer VM."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/18/2016"
   ms.author="magoedte;paulomarquesc" />

# Azure Automation-Szenario – Verwendung von JSON-formatierten Tags zum Erstellen eines Zeitplans für Starten und Herunterfahren einer Azure-VM

Häufig möchten Kunden das Starten und Herunterfahren virtueller Computer planen, um Abonnementkosten zu senken oder geschäftliche und technische Anforderungen zu unterstützen.

Im folgenden Szenario können Sie das automatisierte Starten und Herunterfahren der virtuellen Computer mithilfe eines Tags namens „Zeitplan“ auf der Ebene der Ressourcengruppe oder des virtuellen Computers in Azure einrichten. Dieser Zeitplan kann mit einer Startzeit und Endzeit von Sonntag bis Samstag konfiguriert werden. Einige Optionen sind zwar bereits einsatzbereit, z.B. [VM-Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) mit Einstellungen für automatische Skalierung, die das horizontale Herunter- oder Hochskalieren ermöglichen, und der Dienst [DevTest Labs](../devtest-lab/devtest-lab-overview.md) mit der integrierten Funktion zur Planung von Start- und Herunterfahrvorgängen, doch diese Optionen unterstützen nur bestimmte Szenarien und können nicht IaaS-VMs zugeordnet werden.

Wenn das Tag „Zeitplan“ auf eine Ressourcengruppe angewendet wird, wird es auf alle virtuellen Computer innerhalb dieser Ressourcengruppe angewendet. Wenn ein Zeitplan auch direkt auf einen virtuellen Computer angewendet wird, hat der letzte Zeitplan in der folgenden Reihenfolge Vorrang:

1.  Zeitplan wird auf eine Ressourcengruppe angewendet.
2.  Zeitplan wird auf eine Ressourcengruppe und einen virtuellen Computer in der Ressourcengruppe angewendet.
3.  Zeitplan wird auf einen virtuellen Computer angewendet.

In diesem Szenario wird im Wesentlichen eine JSON-Zeichenfolge mit einem angegebenen Format übernommen und als Wert für ein Tag namens „Zeitplan“ hinzugefügt. Dann listet ein Runbook alle Ressourcengruppen und virtuellen Computer auf und identifiziert die Zeitpläne für jeden virtuellen Computer basierend auf den oben genannten Szenarien, und dann durchläuft es diese VMs mit angefügten Zeitplänen und bewertet, welche Aktion ausgeführt werden soll, welche angehalten, heruntergefahren oder ignoriert werden kann.

Diese Runbooks verwenden zur Authentifizierung das [ausführende Konto von Azure](../automation/automation-sec-configure-azure-runas-account.md).

## Abrufen des Szenarios

Dieses Szenario besteht aus vier PowerShell-Workflow-Runbooks, die Sie aus dem [TechNet-Katalog](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) oder dem [GitHub](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup)-Repository für dieses Projekt herunterladen können.

Runbook | Beschreibung 
----------|----------
Test-ResourceSchedule | Überprüft den Zeitplan jedes virtuellen Computers (führt Herunterfahren oder Starten virtueller Computer je nach Zeitplan aus)
Add-ResourceSchedule | Fügt das Tag „Schedule“ einer VM oder Ressourcengruppe hinzu
Update-ResourceSchedule | Ändert ein vorhandenes Tag „Zeitplan“, indem es durch ein neues ersetzet wird
Remove-ResourceSchedule | Entfernt das Tag „Zeitplan“ aus einer VM oder Ressourcengruppe 


## Installieren und Konfigurieren dieses Szenarios

### Installieren und Veröffentlichen der Runbooks

Nach dem Herunterladen der Runbooks können Sie sie mit dem in [Erstellen oder Importieren eines Runbooks in Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-Azure-Automation) beschriebenen Verfahren importieren. Veröffentlichen Sie jedes Runbook, nachdem sie die Runbooks in Ihr Automation-Konto importiert haben.


### Hinzufügen eines Zeitplans zu Test-ResourceSchedule

Führen Sie diese Schritte aus, um den Zeitplan im Test-ResourceSchedule-Runbook zu aktivieren. Dieses Runbook überprüft, welche virtuellen Computer gestartet, heruntergefahren oder unverändert gelassen werden.

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto, und klicken Sie auf die Kachel **Runbooks**.
2. Klicken Sie auf dem Blatt **Test-ResourceSchedule** auf die Kachel **Zeitpläne**.
3. Klicken Sie auf dem Blatt **Zeitpläne** auf **Zeitplan hinzufügen**.
4. Wählen Sie auf dem Blatt **Zeitpläne** die Option **Zeitplan mit Runbook verknüpfen** und auf dem Blatt **Zeitplan** die Option **Neuen Zeitplan erstellen** aus.
5.  Geben Sie auf dem Blatt **Neuer Zeitplan** den Namen dieses Zeitplans ein. Beispiel: HourlyExecution.
6. Legen Sie für den **Start** des Zeitplans die Startzeit auf einen gerundeten Stundenwert fest.
7. Wählen Sie **Wiederholung** und für das **Erneut alle**-Intervall **1 Stunde** aus.
8. Vergewissern Sie sich, dass **Ablaufdatum festlegen** auf **Nein** festgelegt ist, und klicken Sie dann auf **Erstellen**, um den neuen Zeitplan zu speichern.
9. Wählen Sie auf dem Optionenblatt **Runbook planen** die Option **Parameter und Ausführungseinstellungen** aus, und geben Sie auf dem Test-ResourceSchedule-Blatt **Parameter** den Namen Ihres Abonnements in das Feld **SubscriptionName** ein. Dies ist der einzige für das Runbook erforderliche Parameter. Klicken Sie zum Abschließend des Vorgangs auf **OK**.
   

Der fertige Runbookzeitplan sollte wie folgt aussehen:<br> ![Konfiguriertes Test-ResourceSchedule-Runbook](./media/automation-scenario-start-stop-vm-wjson-tags/automation-schedule-config.png)<br>

## Zeitplan-JSON-Format

Diese Lösung übernimmt im Wesentlichen eine JSON-Zeichenfolge mit einem angegebenen Format und fügt sie als Wert für ein Tag namens „Zeitplan“ hinzu. Dann listet ein Runbook alle Ressourcengruppen und virtuellen Computer auf, identifiziert die Zeitpläne für jeden virtuellen Computer, durchläuft diese virtuellen Computer mit angefügten Zeitpläne und überprüft, welche Aktion ausgeführt werden soll. Im Folgenden sehen Sie ein Beispiel für die Formatierung.

    {
       "TzId": "Eastern Standard Time", 
        "0": {  
           "S": "11",
           "E": "17" 
        },
        "1": {
           "S": "9",
           "E": "19"
        },
        "2": {
           "S": "9",
           "E": "19"
        },
    }

Ausführliche Informationen zu dieser Struktur:

1. Das Format dieser JSON-Struktur ist für die Umgehung der Beschränkung auf 256 Zeichen für einen einzelnen Tagwert in Azure optimiert.

2. *TzId* stellt die Zeitzone des virtuellen Computers dar. Diese ID kann mithilfe der .NET TimeZoneInfo-Klasse in einer PowerShell-Sitzung abgerufen werden: **[System.TimeZoneInfo]::GetSystemTimeZones()**.<br>

    ![GetSystemTimeZones in PowerShell](./media/automation-scenario-start-stop-vm-wjson-tags/automation-get-timzone-powershell.png)

    - Wochentage werden mit einem numerischen Wert von 0 bis 6 dargestellt. Der Wert 0 entspricht Sonntag.
    - Die Startzeit wird mit dem **S**-Attribut dargestellt, und ihr Wert hat das 24-Stunden-Format.
    - Die End- oder Herunterfahrzeit wird mit dem **E**-Attribut dargestellt, und ihr Wert hat das 24-Stunden-Format.

    Wenn S- und E-Attribut den Wert null (0) haben, wird der virtuelle Computer in dem zum Zeitpunkt der Bewertung bestehenden Zustand belassen.

3. Wenn Sie die Bewertung für einen bestimmten Tag der Woche überspringen möchten, fügen Sie den Abschnitt des entsprechenden Wochentages nicht hinzu. Im folgenden Beispiel wird nur der Montag bewertet, und die anderen Tage der Woche werden ignoriert.
   
        {
          "TzId": "Eastern Standard Time",
           "1": {
             "S": "11",
             "E": "17"
           }
        }

## Markieren von Ressourcengruppen oder virtuellen Computern

Zum Herunterfahren virtueller Computer müssen Sie diese oder die Ressourcengruppen, in denen sie sich befinden, markieren. Virtuelle Computer, die nicht über ein Tag „Zeitplan“ verfügen, werden nicht bewertet und daher auch nicht gestartet oder heruntergefahren. Es gibt zwei Möglichkeiten, Ressourcengruppen oder virtuelle Computer mit dieser Lösung zu markieren: direkt über das Portal oder mithilfe der **Add-ResourceSchedule**-, **Update-ResourceSchedule**- und **Remove-ResourceSchedule**-Runbooks.

### Tags über das Portal erstellen

Führen Sie diese Schritte aus, um einen virtuellen Computer oder eine Ressourcengruppe im Portal zu kennzeichnen.

1. Vereinfachen Sie die JSON-Zeichenfolge, und stellen Sie sicher, dass sie keine Leerzeichen enthält. Die JSON-Zeichenfolge sollte wie folgt aussehen:

        {"TzId":"Eastern Standard Time","0":{"S":"11","E":"17"},"1":{"S":"9","E":"19"},"2": {"S":"9","E":"19"},"3":{"S":"9","E":"19"},"4":{"S":"9","E":"19"},"5":{"S":"9","E":"19"},"6":{"S":"11","E":"17"}}
   

2. Wählen Sie einen virtuellen Computer oder eine Ressourcengruppe aus, um diesen Zeitplan durch Auswählen des Tag-Symbols anzuwenden.<br>![VM-Tagoption](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-tag-option.png)
3. Tags werden auf ein Schlüssel/Wert-Paar folgend definiert. Geben Sie **Zeitplan** in das Feld **Schlüssel** ein, fügen Sie die JSON-Zeichenfolge in das Feld **Wert** ein, und klicken Sie dann auf **Speichern**. Ihr neues Tag sollte jetzt in der Liste der Tags für die Ressource angezeigt werden.<br>![VM-Zeitplan-Tag](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-schedule-tag.png)


### Markierung über PowerShell

Der Anfang des Skripts jedes importierten Runbooks enthält Hilfeinformationen, die beschreiben, wie Sie die Runbooks direkt über PowerShell ausführen. Die Runbooks Add-ScheduleResource und Update-ScheduleResource können durch Übergeben der erforderlichen Parameter aus PowerShell aufgerufen werden, sodass Sie das Tag „Zeitplan“ auf einem virtuellen Computer oder einer Ressourcengruppe außerhalb des Portals erstellen oder aktualisieren können.

Zum Erstellen, Hinzufügen und Löschen von Tags über PowerShell müssen Sie zunächst Ihre [PowerShell-Umgebung für Azure](../powershell-install-configure.md) einrichten. Nachdem Sie das Setup abgeschlossen haben, können Sie mit den folgenden Schritten fortfahren.

### Erstellen eines Tags „Zeitplan“ mit PowerShell

1. Öffnen Sie eine PowerShell-Sitzung, und führen Sie Folgendes aus, um sich mit Ihrem ausführenden Konto zu authentifizieren, und geben Sie ein Abonnement an:
    
        Conn = Get-AutomationConnection -Name AzureRunAsConnection
        Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
        -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
        Select-AzureRmSubscription -SubscriptionName "MySubscription"
   
2. Definieren Sie eine Zeitplanhashtabelle. Hier ist ein Beispiel, wie erstellt werden sollte:
    
        $schedule= @{ "TzId"="Eastern Standard Time"; "0"= @{"S"="11";"E"="17"};"1"= @{"S"="9";"E"="19"};"2"= @{"S"="9";"E"="19"};"3"= @{"S"="9";"E"="19"};"4"= @{"S"="9";"E"="19"};"5"= @{"S"="9";"E"="19"};"6"= @{"S"="11";"E"="17"}}

3. Definieren Sie die Parameter, die das Runbook benötigt. Im folgenden Beispiel ist ein virtueller Computer das Ziel:

        $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; `
        "VmName"="VM01";"Schedule"=$schedule}

    Wenn Sie eine Ressourcengruppe markieren, entfernen Sie einfach wie folgt den Parameter *VMName* aus der $params-Hashtabelle:

        $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; `
        "Schedule"=$schedule}

4. Führen Sie das **Add-ResourceSchedule**-Runbook mit den folgenden Parametern aus, um das Tag „Zeitplan“ zu erstellen:

        Start-AzureRmAutomationRunbook -Name "Add-ResourceSchedule" -Parameters $params `
        -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"

5. Wenn Sie das Tag einer Ressourcengruppe oder eines virtuellen Computers aktualisieren möchten, führen Sie das **Update-ResourceSchedule**-Runbook mit den folgenden Parametern aus:

        Start-AzureRmAutomationRunbook -Name "Update-ResourceSchedule" -Parameters $params `
        -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"

### Entfernen eines Tags „Zeitplan“ mit PowerShell

1. Öffnen Sie eine PowerShell-Sitzung, und führen Sie Folgendes aus, um sich mit Ihrem ausführenden Konto zu authentifizieren, wählen Sie ein Abonnement aus, und geben Sie es an:
    
        Conn = Get-AutomationConnection -Name AzureRunAsConnection
        Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
        -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
        Select-AzureRmSubscription -SubscriptionName "MySubscription"

2. Definieren Sie die Parameter, die das Runbook benötigt. Im folgenden Beispiel ist ein virtueller Computer das Ziel:

        $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01" ` 
        ;"VmName"="VM01"}

    Wenn Sie ein Tag von einer Ressourcengruppe entfernen, entfernen Sie einfach wie folgt den Parameter *VMName* aus der $params-Hashtabelle:

        $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"}

3. Führen Sie das **Remove-ResourceSchedule**-Runbook aus, um das Tag „Zeitplan“ zu entfernen:

        Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
        -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"

4. Wenn Sie das Tag einer Ressourcengruppe oder eines virtuellen Computers aktualisieren möchten, führen Sie das **Remove-ResourceSchedule**-Runbook mit den folgenden Parametern aus:

        Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
        -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"


>[AZURE.NOTE] Sie sollten diese Runbooks (und den Zustand des virtuellen Computers) proaktiv überwachen, um sicherzustellen, dass die virtuellen Computer entsprechend heruntergefahren und gestartet werden.

Sie können die Details des **Test-ResourceSchedule**-Runbookauftrags im Azure-Portal durch Auswählen der Kachel „Aufträge“ des Runbooks anzeigen. Die Zusammenfassung des Auftrags zeigt die Eingabeparameter und den Ausgabedatenstrom sowie allgemeine Informationen zum Auftrag und alle aufgetretenen Ausnahmen an. Der Verlauf enthält Meldungen aus dem Ausgabedatenstrom sowie Warnungs- und Fehlerdatenströmen. Wählen Sie die Kachel „Ausgabe“ aus, um detaillierte Ergebnisse der Runbookausführung anzuzeigen.<br> ![Test-ResourceSchedule-Ausgabe](./media/automation-scenario-start-stop-vm-wjson-tags/automation-job-output.png)

## Nächste Schritte

-  Die ersten Schritte mit PowerShell-Workflow-Runbooks sind unter [Mein erstes PowerShell-Workflow-Runbook](automation-first-runbook-textual.md) beschrieben.
-  Informationen zu den verschiedenen Runbooktypen sowie zu ihren Vorteilen und Einschränkungen finden Sie unter [Azure Automation-Runbooktypen](automation-runbook-types.md).
-  Weitere Informationen zur PowerShell-Skriptunterstützung finden Sie unter [Announcing Native PowerShell Script Support in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/) (Ankündigung der nativen PowerShell-Skriptunterstützung in Azure Automation).
-  Weitere Informationen zu Runbookprotokollierung und -ausgabe finden Sie unter [Runbookausgabe und -meldungen in Azure Automation](automation-runbook-output-and-messages.md).
-  Weitere Informationen zum ausführenden Konto von Azure sowie zum Authentifizieren von Runbooks, die dieses Konto verwenden, finden Sie unter [Authentifizieren von Runbooks mit der Azure-Option „Ausführendes Konto“](../automation/automation-sec-configure-azure-runas-account.md).

<!---HONumber=AcomDC_0720_2016-->