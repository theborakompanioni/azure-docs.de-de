---
title: Problembehandlung bei allgemeinen Azure Automation-Problemen | Microsoft-Dokumentation
description: "Dieser Artikel enthält Informationen zum Behandeln und Beheben von häufig auftretenden Azure Automation-Fehlern."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
tags: top-support-issue
keywords: Automation-Fehler, Problembehandlung, Problem
ms.assetid: 5f3cfe61-70b0-4e9c-b892-d02daaeee07d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2017
ms.author: sngun; v-reagie
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: 64548d91e98754210cc5185d9d759141cc0621d3
ms.contentlocale: de-de
ms.lasthandoff: 06/26/2017


---
# <a name="troubleshooting-common-issues-in-azure-automation"></a>Problembehandlung für allgemeine Probleme in Azure Automation 
Dieser Artikel bietet Hilfe für die Problembehandlung häufiger Fehler, die in Azure Automation auftreten können, und enthält mögliche Lösungen zum Beheben dieser Fehler.

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Authentifizierungsfehler beim Verwenden von Azure Automation-Runbooks
### <a name="scenario-sign-in-to-azure-account-failed"></a>Szenario: Fehler beim Anmelden am Azure-Konto
**Fehler**: Sie erhalten beim Verwenden des Add-AzureAccount- oder Login-AzureRmAccount-Cmdlets den Fehler „Unknown_user_type: Unbekannter Benutzertyp“.

**Ursache des Fehlers:** Dieser Fehler tritt auf, wenn der Assetname für die Anmeldeinformationen ungültig ist oder Benutzername und Kennwort, die Sie zur Einrichtung des Automation-Anmeldeinformationsassets verwendet haben, nicht gültig sind.

**Tipps zur Problembehandlung:** Führen Sie die folgenden Schritte aus, um zu ermitteln, wo der Fehler liegt:  

1. Stellen Sie sicher, dass keine Sonderzeichen (einschließlich des Zeichens **@** ) im Namen des Assets mit den Automation-Anmeldeinformationen enthalten sind, die Sie zum Herstellen der Verbindung mit Azure verwenden.  
2. Überprüfen Sie, ob Sie den Benutzernamen und das Kennwort aus den Azure Automation-Anmeldeinformationen in Ihrem lokalen PowerShell ISE-Editor verwenden können. Hierfür können Sie die folgenden Cmdlets in der PowerShell ISE ausführen:  

        $Cred = Get-Credential  
        #Using Azure Service Management   
        Add-AzureAccount –Credential $Cred  
        #Using Azure Resource Manager  
        Login-AzureRmAccount –Credential $Cred
3. Wenn die Authentifizierung lokal fehlschlägt, bedeutet dies, dass Sie Ihre Azure Active Directory-Anmeldeinformationen nicht richtig eingerichtet haben. Informationen zur richtigen Einrichtung des Azure Active Directory-Kontos finden Sie im Blogbeitrag [Authenticating to Azure using Azure Active Directory](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) (Authentifizieren in Azure mit Azure Active Directory).  

### <a name="scenario-unable-to-find-the-azure-subscription"></a>Szenario: Azure-Abonnement nicht gefunden
**Fehler:** Sie erhalten beim Arbeiten mit dem Cmdlet „Select-AzureSubscription“ oder „Select-AzureRmSubscription“ die Fehlermeldung „Das Abonnement mit dem Namen ``<subscription name>`` wurde nicht gefunden“.

**Ursache des Fehlers:** Dieser Fehler tritt auf, wenn der Name des Abonnements ungültig ist, oder wenn der Azure Active Directory-Benutzer, der versucht, die Abonnementdetails abzurufen, nicht als Administrator des Abonnements konfiguriert ist.

**Tipps zur Problembehandlung:** Führen Sie die folgenden Schritte aus, um zu ermitteln, ob die Authentifizierung in Azure richtig durchgeführt wurde, und ob Sie Zugriff auf das Abonnement haben, das Sie auswählen möchten:  

1. Achten Sie darauf, **Add-AzureAccount** auszuführen, bevor Sie das Cmdlet **Select-AzureSubscription** ausführen.  
2. Wenn diese Fehlermeldung weiterhin angezeigt wird, ändern Sie Ihren Code, indem Sie das Cmdlet **Get-AzureSubscription** nach dem Cmdlet **Add-AzureAccount** hinzufügen und dann den Code ausführen.  Überprüfen Sie jetzt, ob die Ausgabe von Get-AzureSubscription Ihre Abonnementdetails enthält.  

   * Falls in der Ausgabe keine Abonnementdetails angezeigt werden, bedeutet dies, dass das Abonnement noch nicht initialisiert wurde.  
   * Wenn Sie die Abonnementdetails in der Ausgabe angezeigt werden, bestätigen Sie mithilfe des Cmdlets **Select-AzureSubscription**, dass Sie den richtigen Abonnementnamen bzw. die richtige ID verwenden.   

### <a name="scenario-authentication-to-azure-failed-because-multi-factor-authentication-is-enabled"></a>Szenario: Fehler bei der Authentifizierung gegenüber Azure, da Multi-Factor Authentication aktiviert ist
**Fehler:** Der Fehler „Add-AzureAccount: AADSTS50079: Starke Authentifizierungsregistrierung (Nachweis) erforderlich“ wird ausgegeben, wenn Sie sich für Azure mit Ihrem Azure-Benutzernamen und -Kennwort authentifizieren.

**Ursache des Fehlers:** Falls Sie für Ihr Azure-Konto über Multi-Factor Authentication verfügen, können Sie für die Authentifizierung gegenüber Azure keinen Azure Active Directory-Benutzer verwenden.  Stattdessen müssen Sie ein Zertifikat oder einen Dienstprinzipal für die Authentifizierung gegenüber Azure verwenden.

**Tipps zur Problembehandlung**: Informationen zum Verwenden eines Zertifikats mit den Azure Service Management-Cmdlets finden Sie unter [Managing Azure Services with the Microsoft Azure Automation Preview Service](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) (Verwalten von Azure Services mit dem Microsoft Azure Automation Preview-Dienst). Informationen zum Verwenden eines Dienstprinzipals mit Azure Resource Manager-Cmdlets finden Sie unter [Erstellen eines Dienstprinzipals mit dem Azure-Portal](../azure-resource-manager/resource-group-create-service-principal-portal.md) und [Authentifizieren eines Dienstprinzipals mit dem Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md).

## <a name="common-errors-when-working-with-runbooks"></a>Häufige Fehler beim Verwenden von Runbooks
### <a name="scenario-the-runbook-job-start-was-attempted-three-times-but-it-failed-to-start-each-time"></a>Szenario: Es wurde dreimal versucht, den Runbookauftrag zu starten, jedoch schlugen die Versuche jedes Mal fehl.
**Fehler:** Your runbook fails with the error "The job was tried three times but it failed." (Ihr Runbook schlägt mit dem Fehler „Es wurde dreimal erfolglos versucht, den Auftrag auszuführen.“)

**Ursache des Fehlers:** Dieser Fehler kann durch folgende Gründe verursacht werden:  

1. Arbeitsspeicherlimit.  Wir haben dokumentierte Beschränkungen, wie viel Speicherplatz den [Automatisierungsdienstgrenzwerten](../azure-subscription-service-limits.md#automation-limits) einer Sandbox zugeordnet sind. Ein Auftrag schlägt also möglicherweise fehl, wenn mehr als 400 MB Arbeitsspeicher verwendet werden. 

2. Modul inkompatibel.  Dies kann auftreten, wenn Modulabhängigkeiten nicht korrekt sind. Wenn dies der Fall ist, gibt Ihr Runbook in der Regel die Benachrichtigung „Befehl wurde nicht gefunden“ oder „Der Parameter kann nicht gebunden werden“. 

**Tipps zur Problembehandlung:** Sie können dieses Problem wie folgt beheben:  

* Empfohlene Methoden, die innerhalb des Arbeitsspeichergrenzwerts funktionieren, sollen die Workload zwischen mehreren Runbooks aufteilen und nicht so viel Daten im Speicher verarbeiten, keine unnötige Ausgabe aus den Runbooks schreiben oder ermitteln, wie viele Prüfpunkt Sie in Ihre PowerShell-Workflow-Runbooks schreiben.  

* Sie müssen Ihre Azure-Module aktualisieren, indem Sie die Schritte unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) befolgen.  


### <a name="scenario-runbook-fails-because-of-deserialized-object"></a>Szenario: Runbookfehler aufgrund eines deserialisierten Objekts
**Fehler**: Für Ihr Runbook tritt folgender Fehler auf: „Der Parameter ``<ParameterName>`` kann nicht gebunden werden. Der Wert ``<ParameterType>`` des Typs ``<ParameterType>`` ‚Deserialisiert‘ kann nicht in den Typ ``<ParameterType>`` konvertiert werden.“

**Ursache des Fehlers:** Wenn es sich bei Ihrem Runbook um einen PowerShell-Workflow handelt, werden komplexe Objekte in einem deserialisierten Format gespeichert, um den Runbookstatus beizubehalten, wenn der Workflow angehalten wird.  

**Tipps zur Problembehandlung:**  
Sie können dieses Problem mit einer der folgenden drei Lösungen beheben:

1. Wenn Sie komplexe Objekte von einem Cmdlet an ein anderes übergeben, sollten Sie diese Cmdlets mit einem InlineScript umschließen.  
2. Übergeben Sie den Namen oder Wert, den Sie aus dem komplexen Objekt benötigen, anstatt das gesamte Objekt zu übergeben.  
3. Verwenden Sie anstelle eines PowerShell-Workflow-Runbooks ein PowerShell-Runbook.  

### <a name="scenario-runbook-job-failed-because-the-allocated-quota-exceeded"></a>Szenario: Fehler beim Runbookauftrag, da das zugeordnete Kontingent überschritten wurde
**Fehler:** Ihr Runbookauftrag schlägt mit dem Fehler „Das Kontingent für die monatliche Gesamtausführungsdauer des Auftrags wurde für dieses Abonnement erreicht“ fehl.

**Ursache des Fehlers**: Dieser Fehler tritt auf, wenn die Auftragsausführung das kostenlose Kontingent von 500 Minuten für Ihr Konto überschreitet. Dieses Kontingent gilt für alle Arten von Auftragsausführungsaufgaben, z. B. Testen eines Auftrags, Starten eines Auftrags im Portal, Ausführen eines Auftrags per Webhook und Planen der Ausführung eines Auftrags per Azure-Portal oder in Ihrem Rechenzentrum. Weitere Informationen zu den Preisen für Automation finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/).

**Tipps zur Problembehandlung:** Wenn Sie mehr als 500 Minuten an Verarbeitungszeit pro Monat nutzen möchten, müssen Sie Ihr Abonnement vom Tarif „Free“ auf den Tarif „Basic“ umstellen. Sie können das Upgrade auf den Tarif „Basic“ mit den folgenden Schritten durchführen:  

1. Melden Sie sich bei Ihrem Azure-Abonnement an.  
2. Wählen Sie das Automation-Konto aus, das Sie aktualisieren möchten.  
3. Klicken Sie auf **Einstellungen** > **Tarif und Nutzung** > **Tarif**.  
4. Wählen Sie auf dem Blatt **Tarif wählen** die Option **Basic** aus.    

### <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a>Szenario: Cmdlet wird beim Ausführen eines Runbooks nicht erkannt
**Fehler**: Für Ihren Runbookauftrag tritt der folgende Fehler auf: ``<cmdlet name>``„Die Benennung ``<cmdlet name>`` wurde nicht als Name eines Cmdlets, einer Funktion, einer Skriptdatei oder eines ausführbaren Programms erkannt.“

**Ursache des Fehlers:** Dieser Fehler wird verursacht, wenn das PowerShell-Modul das Cmdlet nicht findet, das Sie in Ihrem Runbook verwenden.  Dies kann der Fall sein, weil das Modul mit dem Cmdlet unter dem Konto nicht vorhanden ist, ein Namenskonflikt mit einem Runbooknamen besteht oder das Cmdlet auch in einem anderen Modul vorhanden ist und Automation den Namen nicht auflösen kann.

**Tipps zur Problembehandlung:** Sie können dieses Problem wie folgt beheben:  

* Überprüfen Sie, ob Sie den Cmdlet-Namen richtig eingegeben haben.  
* Stellen Sie sicher, dass das Cmdlet unter Ihrem Automation-Konto vorhanden ist und dass keine Konflikte bestehen. Überprüfen Sie wie folgt, ob das Cmdlet vorhanden ist: Öffnen Sie ein Runbook im Bearbeitungsmodus, und suchen Sie in der Bibliothek nach dem gewünschten Cmdlet, oder führen Sie **Get-Command``<CommandName>``** aus.  Nachdem Sie überprüft haben, dass das Cmdlet für das Konto verfügbar ist und dass keine Namenskonflikte mit anderen Cmdlets oder Runbooks bestehen, sollten Sie es der Canvas hinzufügen und sicherstellen, dass Sie in Ihrem Runbook einen gültigen Parametersatz verwenden.  
* Falls ein Namenskonflikt vorliegt und das Cmdlet in zwei unterschiedlichen Modulen verfügbar ist, können Sie dies beheben, indem Sie den vollqualifizierten Namen für das Cmdlet verwenden. Sie können beispielsweise **ModuleName\CmdletName** verwenden.  
* Wenn Sie das Runbook lokal in einer Hybrid Worker-Gruppe ausführen, stellen Sie sicher, dass das Modul/Cmdlet auf dem Computer installiert ist, auf dem der Hybrid Worker gehostet wird.

### <a name="scenario-a-long-running-runbook-consistently-fails-with-the-exception-the-job-cannot-continue-running-because-it-was-repeatedly-evicted-from-the-same-checkpoint"></a>Szenario: Ein lange ausgeführtes Runbook schlägt regelmäßig mit der Ausnahme „Der Auftrag kann nicht fortgesetzt werden, da er wiederholt am gleichen Prüfpunkt entfernt wurde“ fehl.
**Ursache des Fehlers:** Dies ist das vorgesehene Verhalten aufgrund der Überwachung der gleichmäßigen Auslastung der Prozesse in Azure Automation, die ein Runbook automatisch beendet, wenn es länger als 3 Stunden ausgeführt wird. Die zurückgegebene Fehlermeldung bietet jedoch keine Optionen für weitere Schritte an. Ein Runbook kann aus verschiedenen Gründen ausgesetzt werden. Meistens wird das Beenden durch Fehler verursacht. Beispielsweise führen eine nicht abgefangene Ausnahme in einem Runbook, ein Netzwerkfehler oder der Absturz des Runbook Workers, der das Runbook ausführt, dazu, dass das Runbook angehalten und dann vom letzten Prüfpunkt an fortgesetzt wird.

**Tipps zur Problembehandlung:** Zur Vermeidung dieses Problems wird empfohlen, Prüfpunkte in einem Workflow zu verwenden.  Weitere Informationen finden Sie unter [Grundlagen des Windows PowerShell-Workflows](automation-powershell-workflow.md#checkpoints).  Eine ausführlichere Erläuterung der gleichmäßigen Auslastung und von Prüfpunkten finden Sie in diesem Blogbeitrag zum [Verwenden von Prüfpunkten in Runbooks](https://azure.microsoft.com/en-us/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).

## <a name="common-errors-when-importing-modules"></a>Häufige Fehler beim Importieren von Modulen
### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a>Szenario: Fehler beim Modulimport oder Cmdlets können nach dem Importieren nicht ausgeführt werden
**Fehler:** Ein Modul kann nicht importiert werden, oder der Import ist erfolgreich, aber es werden keine Cmdlets extrahiert.

**Ursache des Fehlers:** Einige häufige Ursachen, warum ein Modul nicht erfolgreich nach Azure Automation importiert wird, lauten:  

* Die Struktur stimmt nicht mit der Struktur überein, die für Automation erforderlich ist.  
* Das Modul ist von einem anderen Modul abhängig, das für Ihr Automation-Konto nicht bereitgestellt wurde.  
* Für das Modul fehlen die Abhängigkeiten im Ordner.  
* Das Cmdlet **New-AzureRmAutomationModule** wird zum Hochladen des Moduls verwendet, und Sie haben nicht den vollständigen Speicherpfad angegeben oder das Modul nicht mit einer öffentlich zugänglichen URL geladen.  

**Tipps zur Problembehandlung:**  
Sie können dieses Problem wie folgt beheben:  

* Stellen Sie sicher, dass das Modul das folgende Format beachtet:  
  Modulname.Zip **->** Modulname oder Versionsnummer **->** (Modulname.psm1, Modulname.psd1)
* Öffnen Sie die PSD1-Datei, und prüfen Sie, ob für das Modul Abhängigkeiten bestehen.  Wenn ja, laden Sie diese Module in das Automation-Konto hoch.  
* Stellen Sie sicher, dass alle referenzierten DLLs im Modulordner vorhanden sind.  

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Häufige Fehler beim Verwenden der Konfiguration des gewünschten Zustands (Desired State Configuration, DSC)
### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a>Szenario: Der Knoten hat den Fehlerstatus „Nicht gefunden“
**Fehler**: Der Knoten weist den Status **Fehler** mit der Meldung „Fehler beim Versuch, die Aktion vom Server ‚https://``<url>``//accounts/``<account-id>``/Nodes(AgentId=``<agent-id>``)/GetDscAction‘ abzurufen, weil keine gültige ``<guid>``-Konfiguration gefunden wurde.“ auf.

**Ursache des Fehlers:** Dieser Fehler tritt normalerweise auf, wenn der Knoten einem Konfigurationsnamen (z.B. ABC) anstatt einem Knotenkonfigurationsnamen (z.B. ABC.WebServer) zugewiesen ist.  

**Tipps zur Problembehandlung:**  

* Stellen Sie sicher, dass Sie den Knoten mit dem „Knotenkonfigurationsnamen“ und nicht mit dem „Konfigurationsnamen“ zuweisen.  
* Einem Knoten können Sie über das Azure-Portal oder mit einem PowerShell-Cmdlet eine Knotenkonfiguration zuweisen.

  * Um über das Azure-Portal einem Knoten eine Knotenkonfiguration zuzuweisen, öffnen Sie das Blatt **DSC-Knoten**, wählen Sie dann einen Knoten aus, und klicken Sie auf die Schaltfläche **Knotenkonfiguration** zuweisen.  
  * Um einem Knoten mit einem PowerShell-Cmdlet eine Knotenkonfiguration zuzuweisen, verwenden Sie das Cmdlet **Set-AzureRmAutomationDscNode** .

### <a name="scenario--no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a>Szenario: Bei der Kompilierung einer Konfiguration wurden keine Knotenkonfigurationen (MOF-Dateien) erstellt.
**Fehler:** Der DSC-Kompilierungsauftrag wird mit dem folgenden Fehler angehalten: „Kompilierung erfolgreich abgeschlossen, ohne dass MOF-Dateien mit Knotenkonfigurationen erstellt wurden“.

**Ursache des Fehlers:** Wenn der Ausdruck nach dem Schlüsselwort **Node** in der DSC-Konfiguration mit „$null“ ausgewertet wird, werden keine Knotenkonfigurationen erstellt.    

**Tipps zur Problembehandlung:**  
Sie können dieses Problem wie folgt beheben:  

* Stellen Sie sicher, dass der Ausdruck neben dem Schlüsselwort **Node** in der Konfigurationsdefinition nicht mit „$null“ ausgewertet wird.  
* Wenn Sie bei der Kompilierung der Konfiguration ConfigurationData übergeben, stellen Sie sicher, dass Sie die erwarteten Werte übergeben, die für die Konfiguration aus [ConfigurationData](automation-dsc-compile.md#configurationdata)erforderlich sind.

### <a name="scenario--the-dsc-node-report-becomes-stuck-in-progress-state"></a>Szenario: Der DSC-Knotenbericht bleibt mit dem Status „In Bearbeitung“ hängen.
**Fehler:** DSC-Agent gibt die Meldung „Keine Instanz mit den angegebenen Eigenschaftswerten gefunden“ aus.

**Ursache des Fehlers:** Sie haben Ihre WMF-Version aktualisiert und WMI beschädigt.  

**Tipps zur Problembehandlung:** Befolgen Sie die Anweisungen im Blogbeitrag zu [bekannten Problemen und Einschränkungen in DSC](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc), um das Problem zu beheben.

### <a name="scenario--unable-to-use-a-credential-in-a-dsc-configuration"></a>Szenario: In einer DSC-Konfiguration können keine Anmeldeinformationen verwendet werden.
**Fehler:** Der DSC-Kompilierungsauftrag wurde mit dem folgenden Fehler angehalten: Fehler „System.InvalidOperationException“ beim Verarbeiten der Credential-Eigenschaft vom Typ ``<some resource name>``: Das Konvertieren und Speichern eines verschlüsselten Kennworts als Klartext ist nur zulässig, wenn PSDscAllowPlainTextPassword auf „true“ festgelegt ist.

**Ursache des Fehlers:** Sie haben Anmeldeinformationen in einer Konfiguration verwendet, aber nicht die ordnungsgemäßen **ConfigurationData** angegeben, über die **PSDscAllowPlainTextPassword** für jede Knotenkonfiguration auf „true“ festgelegt wird.  

**Tipps zur Problembehandlung:**  

* Stellen Sie sicher, dass Sie die ordnungsgemäßen **ConfigurationData** übergeben, über die **PSDscAllowPlainTextPassword** für jede Knotenkonfiguration auf „true“ festgelegt wird. Weitere Informationen finden Sie im Abschnitt zu den [Assets in Azure Automation DSC](automation-dsc-compile.md#assets).

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie die oben genannten Schritte zur Problembehandlung befolgt und keine Lösung gefunden haben, gibt es folgende Möglichkeiten zur weiteren Unterstützung.

* Wenden Sie sich an Azure-Experten. Posten Sie Ihr Problem in den [MSDN Azure- oder Stack Overflow-Foren](https://azure.microsoft.com/support/forums/).
* Erstellen Sie einen Azure-Supportfall. Klicken Sie auf der [Azure-Support-Website](https://azure.microsoft.com/support/options/) unter **Technischer und Abrechnungssupport** auf **Support erhalten**.
* Senden Sie im [Script Center](https://azure.microsoft.com/documentation/scripts/) eine Skriptanforderung, wenn Sie nach einer Azure Automation-Runbooklösung oder einem Integrationsmodul suchen.
* Veröffentlichen Sie Feedback oder Vorschläge zu Features für Azure Automation unter [User Voice](https://feedback.azure.com/forums/34192--general-feedback)(Aussagen von Benutzern).

