<properties
    pageTitle="Automatisieren der Entfernung von Ressourcengruppen | Microsoft Azure"
    description="PowerShell-Workflow-Version eines Azure Automation-Szenarios mit Runbooks zum Entfernen aller Ressourcengruppen in Ihrem Abonnement."
    services="automation"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""
    />
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="magoedte"/>


# <a name="azure-automation-scenario---automate-removal-of-resource-groups"></a>Azure Automation-Szenario – Automatisieren der Entfernung von Ressourcengruppen

Viele Kunden erstellen mehr als eine Ressourcengruppe. Einige werden unter Umständen für die Verwaltung von Produktionsanwendungen verwendet, und andere werden vielleicht als Entwicklungs-, Test- und Stagingumgebungen genutzt. Dabei kann nicht nur die Bereitstellung dieser Ressourcen automatisiert werden, es ist auch möglich, eine Ressourcengruppe mit nur einem Klick außer Betrieb zu setzen. Mit Azure Automation können Sie diese häufig anfallende Verwaltungsaufgabe optimieren. Darüber hinaus ist dieses Szenario auch hilfreich, wenn Sie ein Azure-Abonnement verwenden, für das ein durch ein Mitgliedsangebot wie MSDN oder das Microsoft Partner Network Cloud Essentials-Programm bedingtes Ausgabenlimit gilt.

Dieses Szenario basiert auf einem PowerShell-Runbook und dient dazu, einzelne oder mehrere Ressourcengruppen aus Ihrem Abonnement zu entfernen. In der Standardeinstellung des Runbooks wird vor dem Fortfahren ein Test durchgeführt. So wird sichergestellt, dass Sie die Ressourcengruppe nicht versehentlich löschen, bevor Sie zum Abschließen dieses Verfahrens bereit sind.   

## <a name="getting-the-scenario"></a>Abrufen des Szenarios

Dieses Szenario umfasst ein PowerShell-Runbook, das Sie aus dem [PowerShell-Katalog](https://www.powershellgallery.com/packages/Remove-ResourceGroup/1.0/DisplayScript) herunterladen können. Sie können es auch direkt über das Azure-Portal aus dem [Runbookkatalog](automation-runbook-gallery.md) importieren.<br><br>

Runbook | Beschreibung|
----------|------------|
Remove-ResourceGroup | Entfernt eine oder mehrere Azure-Ressourcengruppen und die zugeordneten Ressourcen aus dem Abonnement.  
<br>
Für dieses Runbook sind folgende Eingabeparameter definiert:

Parameter | Beschreibung|
----------|------------|
NameFilter (erforderlich) | Ermöglicht die Angabe eines Namensfilters zur Einschränkung der zu löschenden Ressourcengruppen. Mehrere Werte können in Form einer kommagetrennten Liste übergeben werden.<br>Bei dem Filter wird die Groß-/Kleinschreibung nicht berücksichtigt, und jede Ressourcengruppe, die die Zeichenfolge enthält, wird als Übereinstimmung betrachtet.|
PreviewMode (optional) | Führt das Runbook aus, um zu ermitteln, welche Ressourcengruppen gelöscht werden, aber es wird keine Aktion ausgeführt.<br>Der Wert ist standardmäßig auf **true** festgelegt, um das versehentliche Löschen einzelner oder mehrerer Ressourcengruppen zu vermeiden, die an das Runbook übergeben wurden.  

## <a name="install-and-configure-this-scenario"></a>Installieren und Konfigurieren des Szenarios

### <a name="prerequisites"></a>Voraussetzungen

Dieses Runbook verwendet zur Authentifizierung das [ausführende Konto von Azure](automation-sec-configure-azure-runas-account.md).    

### <a name="install-and-publish-the-runbooks"></a>Installieren und Veröffentlichen der Runbooks

Nach dem Herunterladen des Runbooks können Sie es mithilfe des in [Erstellen oder Importieren eines Runbooks in Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-Azure-Automation) beschriebenen Verfahrens importieren. Veröffentlichen Sie das Runbook, nachdem Sie es in Ihr Automation-Konto importiert haben.


## <a name="using-the-runbook"></a>Verwenden des Runbooks

Die folgenden Schritte veranschaulichen die Ausführung dieses Runbooks, damit Sie sich mit dessen Funktionsweise vertraut machen können. Sie testen das Runbook in diesem Beispiel lediglich. Die Ressourcengruppe wird also nicht tatsächlich gelöscht.  

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto, und klicken Sie auf **Runbooks**.
2. Wählen Sie das Runbook **Remove-ResourceGroup** aus, und klicken Sie auf **Starten**.
3. Wenn Sie das Runbook starten, öffnet sich das Blatt **Runbook starten**, und Sie können die Parameter konfigurieren. Geben Sie die Namen von Ressourcengruppen in Ihrem Abonnement ein, die Sie für das Testing verwenden können und die keinen Schaden anrichten, falls sie versehentlich gelöscht werden.<br> ![Remove-ResouceGroup – Parameter](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-input-parameters.png)

    >[AZURE.NOTE] Stellen Sie sicher, dass **Previewmode** auf **true** festgelegt ist, um das Löschen der ausgewählten Ressourcengruppen zu vermeiden.  **Hinweis:** Die Ressourcengruppe mit dem Automation-Konto, unter dem das Runbook ausgeführt wird, wird von diesem Runbook nicht entfernt.  

4. Klicken Sie nach dem Konfigurieren aller Parameterwerte auf **OK**, um das Runbook der Ausführungswarteschlange hinzuzufügen.  

Die Details des Runbookauftrags **Remove-ResourceGroup** können Sie im Azure-Portal anzeigen, indem Sie im Runbook die Option **Aufträge** wählen. Die Auftragszusammenfassung zeigt die Eingabeparameter und den Ausgabedatenstrom sowie allgemeine Informationen zum Auftrag und alle aufgetretenen Ausnahmen an.<br> ![Remove-ResourceGroup – Status des Runbookauftrags](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-status.png).

Die **Auftragszusammenfassung** beinhaltet Meldungen aus den Ausgabe-, Warnungs- und Fehlerdatenströmen. Wählen Sie die Option **Ausgabe**, um detaillierte Ergebnisse der Runbookausführung anzuzeigen.<br> ![Remove-ResourceGroup – Ergebnisse der Runbookausgabe](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-output.png)

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Einstieg in die Erstellung eigener Runbooks finden Sie unter [Erstellen oder Importieren eines Runbooks in Azure Automation](automation-creating-importing-runbook.md).
- Informationen zu den ersten Schritten mit PowerShell-Workflow-Runbooks finden Sie unter [Mein erstes PowerShell-Workflow-Runbook](automation-first-runbook-textual.md).



<!--HONumber=Oct16_HO2-->


