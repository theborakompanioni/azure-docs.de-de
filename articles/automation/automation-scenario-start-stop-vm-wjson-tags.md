<properties
   pageTitle="Erstellen eines Zeitplans für das Starten und Herunterfahren virtueller Azure-Computer mithilfe von Tags im JSON-Format | Microsoft Azure"
   description="Dieser Artikel beschreibt die Verwendung von JSON-Zeichenfolgen für Tags zum Automatisieren der Planung des Startens und Herunterfahrens virtueller Computer."
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

# Azure Automation-Szenario: Erstellen eines Zeitplans für das Starten und Herunterfahren virtueller Azure-Computer mithilfe von Tags im JSON-Format

Kunden möchten häufig das Starten und Herunterfahren virtueller Computer planen, um Abonnementkosten zu senken oder geschäftliche und technische Anforderungen zu unterstützen.

Im folgenden Szenario können Sie das automatisierte Starten und Herunterfahren der virtuellen Computer mithilfe eines Tags namens „Zeitplan“ auf der Ebene der Ressourcengruppe oder des virtuellen Computers in Azure einrichten. Dieser Zeitplan kann mit einer Startzeit und Endzeit von Sonntag bis Samstag konfiguriert werden.

Es stehen einige sofort verwendungsbereite Optionen zur Verfügung. Diese umfassen:
-  [Skalierungsgruppen für virtuelle Computer](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) mit Einstellungen für automatisches horizontales Herunter- oder Hochskalieren.
- [DevTest Labs](../devtest-lab/devtest-lab-overview.md)-Dienst mit integrierter Planungsfunktion für Start- und Herunterfahrvorgänge.

Diese Optionen unterstützen jedoch nur bestimmte Szenarien und können nicht auf virtuelle IaaS-Computer (Infrastructure-as-a-Service) angewendet werden.

Wenn das Tag „Zeitplan“ auf eine Ressourcengruppe angewendet wird, wird es auf alle virtuellen Computer innerhalb dieser Ressourcengruppe angewendet. Wenn ein Zeitplan auch direkt auf einen virtuellen Computer angewendet wird, hat der letzte Zeitplan in der folgenden Reihenfolge Vorrang:

1.  Zeitplan wird auf eine Ressourcengruppe angewendet.
2.  Zeitplan wird auf eine Ressourcengruppe und einen virtuellen Computer in der Ressourcengruppe angewendet.
3.  Zeitplan wird auf einen virtuellen Computer angewendet.

In diesem Szenario wird im Wesentlichen eine JSON-Zeichenfolge mit einem angegebenen Format verwendet und als Wert für ein Tag namens „Zeitplan“ hinzugefügt. Anschließend listet ein Runbook alle Ressourcengruppen und virtuellen Computer auf und identifiziert die Zeitpläne für die einzelnen virtuellen Computer auf der Grundlage der zuvor aufgeführten Szenarien. Danach werden die virtuellen Computer durchlaufen, die über angefügte Zeitpläne verfügen, und die auszuführende Aktion wird ermittelt. So wird beispielsweise bestimmt, welche virtuellen Computer beendet, heruntergefahren oder ignoriert werden sollen.

Diese Runbooks verwenden zur Authentifizierung das [ausführende Konto von Azure](../automation/automation-sec-configure-azure-runas-account.md).

## Herunterladen der Runbooks für das Szenario

Dieses Szenario besteht aus vier PowerShell-Workflow-Runbooks, die Sie aus dem [TechNet-Katalog](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) oder dem [GitHub](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup)-Repository für dieses Projekt herunterladen können.

Runbook | Beschreibung
----------|----------
Test-ResourceSchedule | Überprüft den Zeitplan der einzelnen virtuellen Computer und führt auf der Grundlage des Zeitplans einen Herunterfahr- oder Startvorgang aus.
Add-ResourceSchedule | Fügt das Zeitplan-Tag einem virtuellen Computer oder einer Ressourcengruppe hinzu.
Update-ResourceSchedule | Ersetzt das vorhandene Zeitplan-Tag durch ein neues.
Remove-ResourceSchedule | Entfernt das Zeitplan-Tag von einem virtuellen Computer oder aus einer Ressourcengruppe.


## Installieren und Konfigurieren des Szenarios

### Installieren und Veröffentlichen der Runbooks

Nach dem Herunterladen der Runbooks können Sie sie mithilfe des unter [Erstellen oder Importieren eines Runbooks in Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-Azure-Automation) beschriebenen Verfahrens importieren. Veröffentlichen Sie die einzelnen Runbooks, nachdem Sie sie in Ihr Automation-Konto importiert haben.


### Hinzufügen eines Zeitplans zum Runbook „Test-ResourceSchedule“

Führen Sie die folgenden Schritte aus, um den Zeitplan für das Runbook „Test-ResourceSchedule“ zu aktivieren. Dieses Runbook überprüft, welche virtuellen Computer gestartet, heruntergefahren oder ignoriert werden sollen.

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto, und klicken Sie auf die Kachel **Runbooks**.
2. Klicken Sie auf dem Blatt **Test-ResourceSchedule** auf die Kachel **Zeitpläne**.
3. Klicken Sie auf dem Blatt **Zeitpläne** auf **Zeitplan hinzufügen**.
4. Wählen Sie auf dem Blatt **Zeitpläne** die Option **Zeitplan mit Runbook verknüpfen** aus. Wählen Sie anschließend **Neuen Zeitplan erstellen** aus.
5.  Geben Sie auf dem Blatt **Neuer Zeitplan** den Namen des Zeitplans ein (beispielsweise *HourlyExecution*).
6. Legen Sie für den **Start** des Zeitplans die Startzeit auf einen Stundenwert fest.
7. Wählen Sie **Wiederholung** und als Wiederholungsintervall die Option **1 Stunde** aus.
8. Vergewissern Sie sich, dass **Ablaufdatum festlegen** auf **Nein** festgelegt ist, und klicken Sie dann auf **Erstellen**, um den neuen Zeitplan zu speichern.
9. Wählen Sie auf dem Optionsblatt **Runbook planen** die Option **Parameter und Ausführungseinstellungen** aus. Geben Sie auf dem Test-ResourceSchedule-Blatt **Parameter** im Feld **SubscriptionName** den Namen Ihres Abonnements ein. Dies ist der einzige erforderliche Parameter für das Runbook. Klicken Sie abschließend auf **OK**.


Der fertige Runbookzeitplan sollte wie folgt aussehen:

![Konfiguriertes Test-ResourceSchedule-Runbook](./media/automation-scenario-start-stop-vm-wjson-tags/automation-schedule-config.png)<br>

## Formatieren der JSON-Zeichenfolge

Diese Lösung verwendet im Wesentlichen eine JSON-Zeichenfolge mit einem angegebenen Format und fügt sie als Wert für ein Tag namens „Zeitplan“ hinzu. Anschließend listet ein Runbook alle Ressourcengruppen und virtuellen Computer auf und identifiziert die Zeitpläne für die einzelnen virtuellen Computer.

Das Runbook durchläuft die virtuellen Computer mit angefügten Zeitplänen und prüft, welche Aktionen ausgeführt werden sollen. Im Anschluss sehen Sie ein Beispiel für die Formatierung der Lösungen:

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

Einige ausführliche Informationen zu dieser Struktur:

1. Das Format dieser JSON-Struktur ist für die Umgehung der Beschränkung auf 256 Zeichen für einen einzelnen Tagwert in Azure optimiert.

2. *TzId* stellt die Zeitzone des virtuellen Computers dar. Diese ID kann mithilfe der .NET -Klasse „TimeZoneInfo“ in einer PowerShell-Sitzung abgerufen werden: **[System.TimeZoneInfo]::GetSystemTimeZones()**.

    ![GetSystemTimeZones in PowerShell](./media/automation-scenario-start-stop-vm-wjson-tags/automation-get-timzone-powershell.png)

    - Wochentage werden mit einem numerischen Wert von 0 bis 6 dargestellt. Der Wert 0 entspricht Sonntag.
    - Die Startzeit wird mit dem Attribut **S** dargestellt, und ihr Wert hat das 24-Stunden-Format.
    - Die Beendigungs- oder Herunterfahrzeit wird mit dem Attribut **E** dargestellt, und ihr Wert hat das 24-Stunden-Format.

    Wenn die Attribute **S** und **E** jeweils den Wert 0 besitzen, wird der virtuelle Computer in dem zum Zeitpunkt der Auswertung bestehenden Zustand belassen.

3. Wenn Sie die Auswertung für einen bestimmten Tag der Woche überspringen möchten, fügen Sie für den entsprechenden Wochentag keinen Abschnitt hinzu. Im folgenden Beispiel wird nur der Montag ausgewertet; die anderen Wochentage werden ignoriert:

        {
          "TzId": "Eastern Standard Time",
           "1": {
             "S": "11",
             "E": "17"
           }
        }

## Markieren von Ressourcengruppen oder virtuellen Computern

Wenn Sie virtuelle Computer herunterfahren möchten, müssen Sie entweder die virtuellen Computer oder die Ressourcengruppen, in denen sie sich befinden, mit einem Tag versehen. Virtuelle Computer ohne Zeitplan-Tag werden nicht ausgewertet. Sie werden also weder gestartet noch heruntergefahren.

Ressourcengruppen oder virtuelle Computer können mit dieser Lösung auf zwei Arten mit einem Tag versehen werden: Entweder direkt über das Azure-Portal oder mithilfe der Runbooks „Add-ResourceSchedule“, „Update-ResourceSchedule“ und „Remove-ResourceSchedule“.

### Markieren über das Portal

Führen Sie die folgenden Schritte aus, um einen virtuellen Computer oder eine Ressourcengruppe über das Portal zu markieren:

1. Vereinfachen Sie die JSON-Zeichenfolge, und stellen Sie sicher, dass sie keine Leerzeichen enthält. Die JSON-Zeichenfolge sollte wie folgt aussehen:

        {"TzId":"Eastern Standard Time","0":{"S":"11","E":"17"},"1":{"S":"9","E":"19"},"2": {"S":"9","E":"19"},"3":{"S":"9","E":"19"},"4":{"S":"9","E":"19"},"5":{"S":"9","E":"19"},"6":{"S":"11","E":"17"}}


2. Wählen Sie für einen virtuellen Computer oder eine Ressourcengruppe das Symbol **Tag** aus, um den Zeitplan anzuwenden.

![VM-Tagoption](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-tag-option.png)
3. Tags werden auf der Grundlage eines Schlüssel-Wert-Paars definiert. Geben Sie **Zeitplan** in das Feld **Schlüssel** ein, und fügen Sie die JSON-Zeichenfolge in das Feld **Wert** ein. Klicken Sie auf **Speichern**. Ihr neues Tag sollte jetzt in der Liste der Tags für die Ressource angezeigt werden.

![VM-Zeitplan-Tag](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-schedule-tag.png)


### Markieren über PowerShell

Der Anfang des Skripts jedes importierten Runbooks enthält Hilfeinformationen, die beschreiben, wie Sie die Runbooks direkt über PowerShell ausführen. Die Runbooks „Add-ScheduleResource“ und „Update-ScheduleResource“ können über PowerShell aufgerufen werden. Übergeben Sie hierzu die erforderlichen Parameter, um das Zeitplan-Tag für einen virtuellen Computer oder eine Ressourcengruppe außerhalb des Portals zu erstellen oder zu aktualisieren.

Zum Erstellen, Hinzufügen und Löschen von Tags über PowerShell müssen Sie zunächst Ihre [PowerShell-Umgebung für Azure einrichten](../powershell-install-configure.md). Nach der Einrichtung können Sie mit den folgenden Schritten fortfahren:

### Erstellen eines Zeitplan-Tags mit PowerShell

1. Öffnen Sie eine PowerShell-Sitzung. Verwenden Sie das folgende Beispiel, um sich mit Ihrem ausführenden Konto zu authentifizieren und ein Abonnement anzugeben:

        Conn = Get-AutomationConnection -Name AzureRunAsConnection
        Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
        -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
        Select-AzureRmSubscription -SubscriptionName "MySubscription"

2. Definieren Sie eine Zeitplanhashtabelle. Hier ist ein Beispiel, wie erstellt werden sollte:

        $schedule= @{ "TzId"="Eastern Standard Time"; "0"= @{"S"="11";"E"="17"};"1"= @{"S"="9";"E"="19"};"2"= @{"S"="9";"E"="19"};"3"= @{"S"="9";"E"="19"};"4"= @{"S"="9";"E"="19"};"5"= @{"S"="9";"E"="19"};"6"= @{"S"="11";"E"="17"}}

3. Definieren Sie die erforderlichen Parameter für das Runbook. Im folgenden Beispiel ist ein virtueller Computer das Ziel:

        $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; `
        "VmName"="VM01";"Schedule"=$schedule}

    Wenn Sie eine Ressourcengruppe mit einem Tag versehen möchten, entfernen Sie den Parameter *VMName* aus der $params-Hashtabelle:

        $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; `
        "Schedule"=$schedule}

4. Führen Sie das Add-ResourceSchedule-Runbook mit den folgenden Parametern aus, um das Zeitplan-Tag zu erstellen:

        Start-AzureRmAutomationRunbook -Name "Add-ResourceSchedule" -Parameters $params `
        -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"

5. Wenn Sie das Tag einer Ressourcengruppe oder eines virtuellen Computers aktualisieren möchten, führen Sie das **Update-ResourceSchedule**-Runbook mit den folgenden Parametern aus:

        Start-AzureRmAutomationRunbook -Name "Update-ResourceSchedule" -Parameters $params `
        -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"

### Entfernen eines Zeitplan-Tags mit PowerShell

1. Öffnen Sie eine PowerShell-Sitzung, und führen Sie Folgendes aus, um sich mit Ihrem ausführenden Konto zu authentifizieren und ein Abonnement auszuwählen und anzugeben:

        Conn = Get-AutomationConnection -Name AzureRunAsConnection
        Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
        -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
        Select-AzureRmSubscription -SubscriptionName "MySubscription"

2. Definieren Sie die erforderlichen Parameter für das Runbook. Im folgenden Beispiel ist ein virtueller Computer das Ziel:

        $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01" `
        ;"VmName"="VM01"}

    Wenn Sie ein Tag von einer Ressourcengruppe entfernen möchten, entfernen Sie den Parameter *VMName* aus der $params-Hashtabelle:

        $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"}

3. Führen Sie das Remove-ResourceSchedule-Runbook aus, um das Zeitplan-Tag zu entfernen:

        Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
        -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"

4. Wenn Sie das Tag einer Ressourcengruppe oder eines virtuellen Computers aktualisieren möchten, führen Sie das Remove-ResourceSchedule-Runbook mit den folgenden Parametern aus:

        Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
        -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"


>[AZURE.NOTE] Es empfiehlt sich, die Runbooks (und den Zustand der virtuellen Computer) proaktiv zu überwachen, um sicherzustellen, dass die virtuellen Computer ordnungsgemäß heruntergefahren und gestartet werden.

Die Details des Test-ResourceSchedule-Runbookauftrags können im Azure-Portal durch Auswählen der Kachel **Aufträge** des Runbooks angezeigt werden. Die Auftragszusammenfassung zeigt die Eingabeparameter und den Ausgabedatenstrom sowie allgemeine Informationen zum Auftrag und alle ggf. aufgetretenen Ausnahmen an.

Die **Auftragszusammenfassung** beinhaltet Meldungen aus den Ausgabe-, Warnungs- und Fehlerdatenströmen. Wählen Sie die Kachel **Ausgabe** aus, um detaillierte Ergebnisse der Runbookausführung anzuzeigen.

![Test-ResourceSchedule-Ausgabe](./media/automation-scenario-start-stop-vm-wjson-tags/automation-job-output.png)

## Nächste Schritte

-  Die ersten Schritte mit PowerShell-Workflow-Runbooks sind unter [Mein erstes PowerShell-Workflow-Runbook](automation-first-runbook-textual.md) beschrieben.
-  Informationen zu den verschiedenen Runbooktypen sowie zu ihren Vorteilen und Einschränkungen finden Sie unter [Azure Automation-Runbooktypen](automation-runbook-types.md).
-  Weitere Informationen zu den Features der PowerShell-Skriptunterstützung finden Sie unter [Announcing Native PowerShell Script Support in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/) (Ankündigung der nativen PowerShell-Skriptunterstützung in Azure Automation).
-  Weitere Informationen zu Runbookprotokollierung und -ausgabe finden Sie unter [Runbookausgabe und -meldungen in Azure Automation](automation-runbook-output-and-messages.md).
-  Weitere Informationen zum ausführenden Konto von Azure sowie zum Authentifizieren von Runbooks mithilfe dieses Kontos finden Sie unter [Authentifizieren von Runbooks mit der Azure-Option „Ausführendes Konto“](../automation/automation-sec-configure-azure-runas-account.md).

<!---HONumber=AcomDC_0803_2016-->