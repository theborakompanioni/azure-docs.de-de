---
title: "Konfigurieren eines ausführenden Kontos in Azure | Microsoft Docs"
description: "Mit diesem Tutorial werden Sie schrittweise durch die Erstellung, das Testen und eine Beispielverwendung der Authentifizierung per Sicherheitsprinzipal in Azure Automation geführt."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
keywords: Dienstprinzipalname, setspn, Azure-Authentifizierung
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/10/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: 15cbf897f3f67b9d1bee0845b4d287fdabe63ba8
ms.lasthandoff: 03/11/2017


---
# <a name="authenticate-runbooks-with-azure-run-as-account"></a>Authentifizieren von Runbooks mit der Azure-Option „Ausführendes Konto“
In diesem Thema wird erläutert, wie Sie im Azure-Portal ein Automation-Konto mit dem Feature „Als Konto ausführen“ konfigurieren, um Runbooks zur Verwaltung von Ressourcen in Azure Resource Manager oder Azure Service Management zu authentifizieren.

Wenn Sie ein Automation-Konto im Azure-Portal erstellen, wird automatisch Folgendes erstellt:

* Ein ausführendes Konto, das einen Dienstprinzipal in Azure Active Directory sowie ein Zertifikat erstellt und die zum Verwalten von Resource Manager-Ressourcen mit Runbooks verwendete Rolle „Mitwirkender“ der rollenbasierten Zugriffssteuerung (RBAC) zuweist.   
* Ein klassisches ausführendes Konto, indem ein Verwaltungszertifikat hochgeladen wird, das zum Verwalten von Azure Service Management-Ressourcen oder klassischen Ressourcen mit Runbooks verwendet wird.  

Dies vereinfacht den Prozess für Sie und ermöglicht Ihnen das schnelle Erstellen und Bereitstellen von Runbooks als Unterstützung für Ihre Anforderungen an die Automation.      

Ein ausführendes Konto und ein klassisches ausführendes Konto ermöglichen Ihnen Folgendes:

* Bereitstellen eines Standardverfahrens für die Authentifizierung bei Azure, wenn Azure Resource Manager- oder Azure Service Management-Ressourcen mithilfe von Runbooks im Azure-Portal verwaltet werden  
* Automatisieren der Verwendung globaler Runbooks, die in Azure Alerts konfiguriert sind

> [!NOTE]
> Für das Azure-Feature zum [Integrieren von Warnungen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) mit globalen Automation-Runbooks ist ein Automation-Konto erforderlich, das mit einem ausführenden Konto und einem klassischen ausführenden Konto konfiguriert ist. Sie können entweder ein Automation-Konto auswählen, für das bereits ein ausführendes Konto und ein klassisches ausführendes Konto definiert sind, oder ein neues Konto erstellen.
>  

Hier erfahren Sie, wie Sie das Automation-Konto über das Azure-Portal erstellen, ein Automation-Konto mit PowerShell aktualisieren, die Kontokonfiguration verwalten und die Authentifizierung in Ihren Runbooks durchführen.

Beachten Sie Folgendes, bevor Sie fortfahren.

1. Die hier beschriebenen Schritte haben keine Auswirkung auf vorhandene Automation-Konten, die bereits im klassischen Bereitstellungsmodell oder Resource Manager-Bereitstellungsmodell erstellt wurden.  
2. Die hier beschriebenen Schritte funktionieren nur für über das Azure-Portal erstellte Automation-Konten.  Wenn Sie versuchen, ein Konto über das klassische Portal zu erstellen, wird die Konfiguration des ausführenden Kontos nicht repliziert.
3. Wenn Sie momentan über Runbooks und Assets (Zeitpläne, Variablen usw.) verfügen, die zuvor für die Verwaltung klassischer Ressourcen erstellt wurden, und diese Runbooks mit dem neuen klassischen ausführenden Konto authentifiziert werden sollen, gehen Sie wie folgt vor: Erstellen Sie wie unter „Verwalten eines ausführenden Azure-Kontos“ beschrieben ein klassisches ausführendes Konto, oder aktualisieren Sie Ihr vorhandenes Konto mithilfe des weiter unten bereitgestellten PowerShell-Skripts.  
4. Zur Authentifizierung mit dem neuen ausführenden Konto und klassischen ausführenden Automation-Konto müssen Sie Ihre vorhandenen Runbooks mit dem Beispielcode ändern, der im Abschnitt [Beispiele für Authentifizierungscode](#authentication-code-examples) angegeben ist.  
   
    >[!NOTE] 
    >Das ausführende Konto dient zur Authentifizierung gegenüber Resource Manager-Ressourcen anhand des zertifikatbasierten Dienstprinzipals, und das klassische ausführende Konto ist für die Authentifizierung gegenüber Service Management-Ressourcen mit einem Verwaltungszertifikat bestimmt.     

## <a name="create-a-new-automation-account-from-the-azure-portal"></a>Erstellen eines neuen Automation-Kontos über das Azure-Portal
In diesem Abschnitt führen Sie die folgenden Schritte aus, um im Azure-Portal ein neues Azure Automation-Konto zu erstellen.  Dadurch werden sowohl das ausführende als auch das klassische ausführende Konto erstellt.  

> [!NOTE]
> Der Benutzer, der diese Schritte ausführt, muss ein Mitglied der Rolle „Dienstadministratoren“ oder Co-Administrator des Abonnements sein, das dem Benutzer Zugriff auf das Abonnement gewährt. Zudem muss der Benutzer der Active Directory-Standardinstanz dieses Abonnements als Benutzer hinzugefügt werden. Das Konto muss keiner privilegierten Rolle zugewiesen werden. Benutzer, die vor dem Hinzufügen zur Rolle des Co-Administrators kein Mitglied der Active Directory-Instanz des Abonnements waren, werden der Active Directory-Instanz als Gast hinzugefügt, und ihnen wird auf dem Blatt **Automation-Konto hinzufügen** die Warnung „Sie haben keine Berechtigungen zum Erstellen…“ angezeigt. Benutzer, die zuerst der Rolle des Co-Administrators hinzugefügt wurden, können aus der Active Directory-Instanz des Abonnements entfernt und erneut hinzugefügt werden, um sie als Vollbenutzer in Active Directory einzurichten. Dies kann im Azure-Portal im Bereich **Azure Active Directory** überprüft werden. Wählen Sie dazu **Benutzer und Gruppen**, **Alle Benutzer** und nach der Auswahl des bestimmten Benutzers die Option **Profil**.  Als Wert des Attributs **Benutzertyp** im Benutzerprofil darf nicht **Gast** angegeben sein.  
> 

1. Melden Sie sich mit einem Konto, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist, beim Azure-Portal an.
2. Wählen Sie die Option **Automation-Konten**.
3. Klicken Sie auf dem Blatt „Automation-Konten“ auf **Hinzufügen**.<br>![Automation-Konto hinzufügen](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)
   
   > [!NOTE]
   > Wenn auf dem Blatt **Automation-Konto hinzufügen** die folgende Warnung angezeigt wird, ist Ihr Konto kein Mitglied der Rolle „Abonnement-Administratoren“ und Co-Admin des Abonnements.<br>![Automation-Konto hinzufügen – Warnung](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   > 
   > 
4. Geben Sie auf dem Blatt **Automation-Konto hinzufügen** im Feld **Name** einen Namen für das neue Automation-Konto ein.
5. Wenn Sie über mehrere Abonnements verfügen, geben Sie ein Abonnement für das neue Konto sowie eine neue oder vorhandene **Ressourcengruppe** und den **Standort** eines Azure-Datencenters an.
6. Stellen Sie sicher, dass **Ja** für die Option **Ausführendes Azure-Konto erstellen** ausgewählt ist, und klicken Sie auf die Schaltfläche **Erstellen**.  
   
   > [!NOTE]
   > Wenn Sie **Nein** auswählen und das ausführende Konto nicht erstellen, wird auf dem Blatt **Automation-Konto hinzufügen** eine Warnmeldung angezeigt.  Das Konto wird zwar im Azure-Portal erstellt, verfügt aber nicht über eine entsprechende Authentifizierungsidentität in Ihrem klassischen oder Resource Manager-Abonnementverzeichnisdienst und hat somit keinen Zugriff auf Ressourcen in Ihrem Abonnement.  Runbooks, die auf dieses Konto verweisen, können sich daher nicht authentifizieren und keine Aufgaben für Ressourcen in diesen Bereitstellungsmodellen ausführen.
   > 
   > ![Automation-Konto hinzufügen – Warnung](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)<br>
   > Wenn der Dienstprinzipal nicht erstellt wird, wird die Rolle „Mitwirkender“ nicht zugewiesen.
   > 

7. Während das Automation-Konto in Azure erstellt wird, können Sie den Status unter **Benachrichtigungen** im Menü nachverfolgen.

### <a name="resources-included"></a>Enthaltene Ressourcen
Nach der erfolgreichen Erstellung des Automation-Kontos werden automatisch verschiedene Ressourcen erstellt.  In der folgenden Tabelle sind die Ressourcen für das ausführende Konto zusammengefasst.<br>

| Ressource | Beschreibung |
| --- | --- |
| AzureAutomationTutorial-Runbook |Ein grafisches Beispielrunbook, das die Authentifizierung mithilfe des ausführenden Kontos veranschaulicht und alle Resource Manager-Ressourcen abruft. |
| AzureAutomationTutorialScript-Runbook |Ein PowerShell-Beispielrunbook, das die Authentifizierung mithilfe des ausführenden Kontos veranschaulicht und alle Resource Manager-Ressourcen abruft. |
| AzureRunAsCertificate |Ein Zertifikatasset, das während der Erstellung des Automation-Kontos automatisch oder mit dem folgenden PowerShell-Skript für ein vorhandenes Konto erstellt wird.  Das Zertifikat ermöglicht die Authentifizierung bei Azure, sodass Sie Azure Resource Manager-Ressourcen über Runbooks verwalten können.  Dieses Zertifikat ist ein Jahr lang gültig. |
| AzureRunAsConnection |Ein Verbindungsasset, das während der Erstellung des Automation-Kontos automatisch oder mit dem unten stehenden PowerShell-Skript für ein vorhandenes Konto erstellt wird. |

In der folgenden Tabelle sind die Ressourcen für das klassische ausführende Konto zusammengefasst.<br>

| Ressource | Beschreibung |
| --- | --- |
| AzureClassicAutomationTutorial-Runbook |Ein grafisches Beispielrunbook, das alle klassischen virtuellen Computer in einem Abonnement mithilfe des klassischen ausführenden Kontos (Zertifikat) abruft und anschließend den VM-Namen und -Status ausgibt. |
| AzureClassicAutomationTutorialScript-Runbook |Ein PowerShell-Beispielrunbook, das alle klassischen virtuellen Computer in einem Abonnement mithilfe des klassischen ausführenden Kontos (Zertifikat) abruft und anschließend den VM-Namen und -Status ausgibt. |
| AzureClassicRunAsCertificate |Ein automatisch erstelltes Zertifikatasset, das zur Authentifizierung bei Azure verwendet wird, sodass Sie klassische Azure-Ressourcen über Runbooks verwalten können.  Dieses Zertifikat ist ein Jahr lang gültig. |
| AzureClassicRunAsConnection |Ein automatisch erstelltes Verbindungsasset, das zur Authentifizierung bei Azure verwendet wird, sodass Sie klassische Azure-Ressourcen über Runbooks verwalten können. |

## <a name="verify-run-as-authentication"></a>Überprüfen der Authentifizierung mit dem ausführenden Konto
Als Nächstes führen wir einen kleinen Test durch, um sicherzustellen, dass Sie sich mit dem neuen ausführenden Konto authentifizieren können.     

1. Öffnen Sie im Azure-Portal das zuvor erstellte Automation-Konto.  
2. Klicken Sie auf die Kachel **Runbooks** , um die Liste mit den Runbooks zu öffnen.
3. Wählen Sie das Runbook **AzureAutomationTutorialScript** aus, und klicken Sie auf **Starten**, um das Runbook zu starten.  Sie werden gefragt, ob Sie das Runbook starten möchten.
4. Es wird ein [Runbook-Auftrag](automation-runbook-execution.md) erstellt, das Blatt „Auftrag“ geöffnet und der Auftragsstatus auf der Kachel **Auftragszusammenfassung** angezeigt.  
5. Der Auftrag besitzt zunächst den Status *In Warteschlange* , um anzugeben, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Wird der Auftrag von einem Worker übernommen, wechselt der Status zu *Wird gestartet* und anschließend zu *Wird ausgeführt*, wenn die Ausführung des Runbooks tatsächlich gestartet wurde.  
6. Wenn der Runbook-Auftrag abgeschlossen ist, sollte der Status **Abgeschlossen** lauten.<br> ![Sicherheitsprinzipal-Runbooktest](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)<br>
7. Um die detaillierten Ergebnisse des Runbooks anzuzeigen, klicken Sie auf die Kachel **Ausgabe** .
8. Im Blatt **Ausgabe** sollte angezeigt werden, dass die Authentifizierung erfolgreich war und eine Liste aller verfügbaren Ressourcen in der Ressourcengruppe zurückgegeben wurde.
9. Schließen Sie das Blatt **Ausgabe**, um zum Blatt **Auftragszusammenfassung** zurückzukehren.
10. Schließen Sie das Blatt **Auftragszusammenfassung** und das entsprechende Runbook-Blatt **AzureAutomationTutorialScript**.

## <a name="verify-classic-run-as-authentication"></a>Überprüfen der Authentifizierung mit dem klassischen ausführenden Konto
Als Nächstes führen wir einen kleinen Test durch, um sicherzustellen, dass Sie sich mit dem neuen klassischen ausführenden Konto authentifizieren können.     

1. Öffnen Sie im Azure-Portal das zuvor erstellte Automation-Konto.  
2. Klicken Sie auf die Kachel **Runbooks** , um die Liste mit den Runbooks zu öffnen.
3. Wählen Sie das Runbook **AzureClassicAutomationTutorialScript** aus, und klicken Sie auf **Starten**, um das Runbook zu starten.  Sie werden gefragt, ob Sie das Runbook starten möchten.
4. Es wird ein [Runbook-Auftrag](automation-runbook-execution.md) erstellt, das Blatt „Auftrag“ geöffnet und der Auftragsstatus auf der Kachel **Auftragszusammenfassung** angezeigt.  
5. Der Auftrag besitzt zunächst den Status *In Warteschlange* , um anzugeben, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Wird der Auftrag von einem Worker übernommen, wechselt der Status zu *Wird gestartet* und anschließend zu *Wird ausgeführt*, wenn die Ausführung des Runbooks tatsächlich gestartet wurde.  
6. Wenn der Runbook-Auftrag abgeschlossen ist, sollte der Status **Abgeschlossen** lauten.<br> ![Sicherheitsprinzipal-Runbooktest](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
7. Um die detaillierten Ergebnisse des Runbooks anzuzeigen, klicken Sie auf die Kachel **Ausgabe** .
8. Auf dem Blatt **Ausgabe** sollte angezeigt werden, dass die Authentifizierung erfolgreich war und eine Liste mit allen klassischen VMs im Abonnement zurückgegeben wurde.
9. Schließen Sie das Blatt **Ausgabe**, um zum Blatt **Auftragszusammenfassung** zurückzukehren.
10. Schließen Sie das Blatt **Auftragszusammenfassung** und das entsprechende Runbookblatt **AzureClassicAutomationTutorialScript**.

## <a name="managing-azure-run-as-account"></a>Verwalten eines ausführenden Azure-Kontos
Während der Lebensdauer Ihres Automation-Kontos müssen Sie das Zertifikat erneuern, bevor es abläuft. Falls der Verdacht besteht, dass das Konto kompromittiert wurde, können Sie das ausführende Konto löschen und neu erstellen.  In diesem Abschnitt erfahren Sie, wie Sie dazu vorgehen müssen.  

### <a name="self-signed-certificate-renewal"></a>Erneuerung eines selbstsignierten Zertifikats
Das für das ausführende Azure-Konto erstellte selbstsignierte Zertifikat ist ab dem Erstellungsdatum ein Jahr lang gültig und kann innerhalb des Gültigkeitszeitraums jederzeit erneuert werden.  Wenn Sie das Zertifikat erneuern, bleibt das alte gültige Zertifikat erhalten, um Runbooks, die sich ggf. noch in der Warteschlange befinden oder aktiv ausgeführt werden und das ausführende Konto für die Authentifizierung verwenden, nicht zu beeinträchtigen.  Das Zertifikat bleibt bis zum Ablaufdatum erhalten.    

> [!NOTE]
> Wenn Sie Ihr ausführendes Automation-Konto für die Verwendung eines Zertifikats konfiguriert haben, das von Ihrer Unternehmenszertifizierungsstelle ausgestellt wurde, und diese Option verwenden, wird dieses Zertifikat durch ein selbstsigniertes Zertifikat ersetzt.  

1. Öffnen Sie das Automation-Konto im Azure-Portal.  
2. Wählen Sie auf dem Blatt des Automation-Kontos im Bereich mit den Kontoeigenschaften unter dem Abschnitt **Kontoeinstellungen** die Option **Ausführende Konten** aus.<br><br> ![Eigenschaftenbereich des Automation-Kontos](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)<br><br>
3. Wählen Sie auf dem Eigenschaftenblatt **Ausführende Konten** entweder das ausführende Konto oder das klassische ausführende Konto aus, für das Sie das Zertifikat erneuern möchten, und klicken Sie anschließend auf dem Eigenschaftenblatt für das ausgewählte Konto auf **Zertifikat erneuern**.<br><br> ![Erneuern des Zertifikats für das ausführende Konto](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)<br><br> Sie werden gefragt, ob Sie fortfahren möchten.  
4. Während das Zertifikat erneuert wird, können Sie den Status im Menü unter **Benachrichtigungen** verfolgen.

### <a name="delete-run-as-account"></a>Löschen des ausführenden Azure-Kontos
Die folgende Anleitung beschreibt, wie Sie Ihr ausführendes Azure oder Ihr klassisches ausführendes Konto löschen und neu erstellen.  Das Automation-Konto bleibt bei dieser Aktion erhalten.  Nach dem Löschen des ausführenden Kontos oder des klassischen ausführenden Kontos können Sie es über das Portal neu erstellen.  

1. Öffnen Sie das Automation-Konto im Azure-Portal.  
2. Wählen Sie auf dem Blatt des Automation-Kontos im Bereich mit den Kontoeigenschaften unter dem Abschnitt **Kontoeinstellungen** die Option **Ausführende Konten** aus.
3. Wählen Sie auf dem Eigenschaftenblatt **Ausführende Konten** entweder das ausführende Konto oder das klassische ausführende Konto aus, das Sie löschen möchten, und klicken Sie anschließend auf dem Eigenschaftenblatt für das ausgewählte Konto auf **Löschen**.<br><br> ![Löschen des ausführenden Azure-Kontos](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)<br><br>  Sie werden gefragt, ob Sie fortfahren möchten.
4. Während das Konto gelöscht wird, können Sie den Status im Menü unter **Benachrichtigungen** verfolgen.  Nach Abschluss des Löschvorgangs können Sie das Konto neu erstellen, indem Sie auf dem Eigenschaftenblatt **Ausführende Konten** die Erstellungsoption für **Ausführendes Azure-Konto** auswählen.<br><br> ![Neuerstellen des ausführenden Automation-Kontos](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)<br> 

### <a name="misconfiguration"></a>Fehlkonfiguration
Damit das ausführende Konto bzw. das klassische ausführende Konto einwandfrei funktioniert, dürfen bestimmte erforderliche Konfigurationselemente nicht gelöscht bzw. müssen bei der anfänglichen Einrichtung richtig erstellt werden. Beispiele für solche Elemente oder unzulässige Vorgänge wären etwa:

* Zertifikatasset 
* Verbindungsasset 
* Entfernung des ausführenden Kontos aus der Rolle „Mitwirkender“
* Dienstprinzipal oder Anwendung in Azure AD

Automation erkennt diese Änderungen und weist Sie mit dem Status **Unvollständig** auf dem Eigenschaftenblatt **Ausführende Konten** für das Konto darauf hin.<br><br> ![Statusmeldung mit Hinweis auf ein unvollständiges ausführendes Konto](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)<br><br>Wenn Sie das ausführende Konto auswählen, wird im Eigenschaftenbereich des Kontos der folgende Fehler angezeigt:<br><br> ![Warnmeldung mit Hinweis auf ein unvollständiges ausführendes Konto](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png)<br>  
Falls Ihr ausführendes Konto falsch konfiguriert ist, können Sie dieses Problem schnell beheben, indem Sie das ausführende Konto löschen und neu erstellen.   

## <a name="update-an-automation-account-using-powershell"></a>Aktualisieren eines Automation-Kontos mit PowerShell
Wenn Folgendes zutrifft, können Sie Ihr vorhandenes Automation-Konto mit PowerShell aktualisieren:

1. Sie haben ein Automation-Konto erstellt, die Erstellung des ausführenden Kontos jedoch abgelehnt. 
2. Sie haben bereits ein Automation-Konto zum Verwalten von Resource Manager-Ressourcen und möchten es aktualisieren, um das ausführende Konto für die Runbook-Authentifizierung einzuschließen.
4. Sie haben bereits ein Automation-Konto zum Verwalten klassischer Ressourcen und möchten es zur Verwendung des klassischen ausführenden Kontos aktualisieren, anstatt ein neues Konto zu erstellen und Ihre Runbooks und Assets zu diesem Konto zu migrieren.   
5. Sie möchten ein ausführendes Azure-Konto und ein klassisches ausführendes Konto unter Verwendung eines Zertifikats erstellen, das von Ihrer Unternehmenszertifizierungsstelle ausgestellt wurde.

Für dieses Skript ist Folgendes erforderlich:

1. Zum Ausführen dieses Skripts wird Windows 10 oder Windows Server 2016 benötigt, und die Azure Resource Manager-Module ab Version 2.01 müssen installiert sein.  Unter älteren Windows-Versionen wird das Skript nicht unterstützt.  
2. Azure PowerShell ab Version 1.0. Informationen zu dieser Version und ihrer Installation finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).
3. Sie haben ein Automation-Konto erstellt.  Dieses Konto wird unten im Skript als Wert für die Parameter -AutomationAccountName und -ApplicationDisplayName angegeben.

Wählen Sie zum Ermitteln der Werte für *SubscriptionID*, *ResourceGroup* und *AutomationAccountName* (erforderliche Parameter für die Skripts) im Azure-Portal auf dem Blatt **Automation-Konto** Ihr Automation-Konto und anschließend die Option **Alle Einstellungen** aus.  Wählen Sie auf dem Blatt **Alle Einstellungen** unter **Kontoeinstellungen** die Option **Eigenschaften** aus.  Die Werte werden auf dem Blatt **Eigenschaften** angezeigt.<br><br> ![Automation-Konto – Eigenschaften](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-run-as-account-powershell-script"></a>PowerShell-Skript zum Erstellen des ausführenden Kontos
Dieses PowerShell-Skript unterstützt folgende Konfigurationen: 

* Erstellen eines ausführenden Azure-Kontos unter Verwendung eines selbstsignierten Zertifikats
* Erstellen eines ausführenden Azure-Kontos und eines klassischen ausführenden Azure-Kontos unter Verwendung eines selbstsignierten Zertifikats
* Erstellen eines ausführenden Azure-Kontos und eines klassischen ausführenden Azure-Kontos unter Verwendung eines Unternehmenszertifikats
* Erstellen eines ausführenden Azure-Kontos und eines klassischen ausführenden Azure-Kontos unter Verwendung eines selbstsignierten Zertifikats in der Azure Government-Cloud

Je nach ausgewählter Konfigurationsoption wird Folgendes erstellt:

* Eine Azure AD-Anwendung, die entweder mit dem öffentlichen Schlüssel des selbstsignierten Zertifikats oder des Unternehmenszertifikats exportiert wird, und ein Dienstprinzipalkonto für diese Anwendung in Azure AD. Außerdem wird die Rolle „Mitwirkender“ (kann auch in „Besitzer“ oder eine andere Rolle geändert werden) für dieses Konto in Ihrem aktuellen Abonnement zugewiesen.  Weitere Informationen finden Sie im Artikel [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md).
* Ein Automation-Zertifikatsset im angegebenen Automation-Konto mit dem Namen **AzureRunAsCertificate**, in dem der von der Azure AD-Anwendung verwendete private Schlüssel des Zertifikats enthalten ist.
* Ein Automation-Verbindungsasset im angegebenen Automation-Konto mit dem Namen **AzureRunAsConnection**, das die Anwendungs-ID (applicationId), Mandanten-ID (tenantId), Abonnement-ID (subscriptionId) und den Zertifikatfingerabdruck enthält.    

Klassisches ausführendes Konto:

* Ein Automation-Zertifikatsset im angegebenen Automation-Konto mit dem Namen **AzureClassicRunAsCertificate**, in dem der vom Verwaltungszertifikat verwendete private Schlüssel des Zertifikats enthalten ist.  
* Ein Automation-Verbindungsasset im angegebenen Automation-Konto mit dem Namen **AzureClassicRunAsConnection**, das den Abonnementnamen, die Abonnement-ID (subscriptionId) und den Namen des Zertifikatassets enthält.

Wenn Sie die Option zum Erstellen eines klassischen ausführenden Kontos wählen, müssen Sie nach der Skriptausführung das öffentliche Zertifikat (CER-Format) in den Verwaltungsspeicher für das Abonnement hochladen, unter dem das Automation-Konto erstellt wurde. Folgen Sie zum Ausführen des Skripts und Hochladen des Zertifikats den unten stehenden Schritten.    

1. Speichern Sie das folgende Skript auf dem Computer.  In diesem Beispiel speichern Sie es mit dem Dateinamen **New-RunAsAccount.ps1**.  
   
         #Requires -RunAsAdministrator
         Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory=$true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory=$true)]
        [String] $SelfSignedCertPlainPassword,
 
        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$EnvironmentName="AzureCloud",

        [Parameter(Mandatory=$false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
        )

        function CreateSelfSignedCertificate([string] $keyVaultName, [string] $certificateName, [string] $selfSignedCertPlainPassword, 
                                      [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
           -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
           -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired)

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose 
        }

        function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $CurrentDate = Get-Date
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $KeyId = (New-Guid).Guid 

        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= [DateTime]$PfxCert.GetExpirationDateString()
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use Key credentials and create AAD Application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId 
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id
   
        # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
        Sleep -s 15
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           Sleep -s 10
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        }
           return $Application.ApplicationId.ToString();
        }

        function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName,[string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
        }

        function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues 
        }

        Import-Module AzureRM.Profile
        Import-Module AzureRM.Resources

        $AzureRMProfileVersion= (Get-Module AzureRM.Profile).Version
        if (!(($AzureRMProfileVersion.Major -ge 2 -and $AzureRMProfileVersion.Minor -ge 1) -or ($AzureRMProfileVersion.Major -gt 2)))
        {
           Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
           return
        }
 
        Login-AzureRmAccount -EnvironmentName $EnvironmentName
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

        # Create Run As Account using Service Principal
        $CertifcateAssetName = "AzureRunAsCertificate"
        $ConnectionAssetName = "AzureRunAsConnection"
        $ConnectionTypeName = "AzureServicePrincipal"
 
        if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
        } else {
          $CertificateName = $AutomationAccountName+$CertifcateAssetName
          $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
          $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
          $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
          CreateSelfSignedCertificate $KeyVaultName $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired 
        }

        # Create Service Principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

        # Create the automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate the ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId} 

        # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create Run As Account using Service Principal
            $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
            $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
            $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
            $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
                    "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                    "Then click Upload and upload the .cer format of #CERT#" 
 
             if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
             $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
             $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
             $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        } else {
             $ClassicRunAsAccountCertificateName = $AutomationAccountName+$ClassicRunAsAccountCertifcateAssetName
             $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
             $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
             $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
             $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
             CreateSelfSignedCertificate $KeyVaultName $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired 
        }

        # Create the automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. Starten Sie auf Ihrem Computer **Windows PowerShell** über den **Startbildschirm** mit erhöhten Benutzerrechten.
3. Navigieren Sie über die PowerShell-Befehlszeilenshell mit erhöhten Rechten zu dem Ordner, der das Skript enthält, das Sie in Schritt 1 erstellt haben, und führen Sie das Skript aus. Legen Sie dabei die erforderlichen Parameterwerte für die gewünschte Konfiguration fest.  

    **Erstellen eines ausführenden Azure-Kontos unter Verwendung eines selbstsignierten Zertifikats**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false` 

    **Erstellen eines ausführenden Azure-Kontos und eines klassischen ausführenden Azure-Kontos unter Verwendung eines selbstsignierten Zertifikats**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Erstellen eines ausführenden Azure-Kontos und eines klassischen ausführenden Azure-Kontos unter Verwendung eines Unternehmenszertifikats**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Erstellen eines ausführenden Azure-Kontos und eines klassischen ausführenden Azure-Kontos unter Verwendung eines selbstsignierten Zertifikats in der Azure Government-Cloud**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`
 
    > [!NOTE]
    > Nach dem Ausführen des Skripts werden Sie aufgefordert, sich gegenüber Azure zu authentifizieren. Sie müssen sich mit einem Konto anmelden, das ein Mitglied der Rolle „Abonnement-Administratoren“ und Co-Admin des Abonnements ist.
    > 
    > 

Führen Sie bei Erstellung eines klassischen ausführenden Kontos nach Abschluss des Skripts die Schritte aus, mit denen das [Zertifikat für die Verwaltungs-API](../azure-api-management-certs.md) in das klassische Azure-Portal hochgeladen wird.  Falls Sie ein klassisches ausführendes Konto mit einem selbstsignierten öffentlichen Zertifikat (CER-Format) erstellt haben, befindet sich eine Kopie des erstellten Zertifikats auf Ihrem Computer im Ordner mit den temporären Dateien (unter dem Benutzerprofil, das zum Ausführen der PowerShell-Sitzung verwendet wurde: *%USERPROFILE%\AppData\Local\Temp*).  Wenn Sie das klassische ausführende Konto dagegen so konfiguriert haben, dass ein von der Zertifizierungsstelle Ihres Unternehmens generiertes Zertifikat (CER-Format) verwendet wird, müssen Sie dieses Zertifikat nutzen.  Sehen Sie sich nach dem Hochladen des Zertifikats den [Beispielcode](#sample-code-to-authenticate-with-service-management-resources) an, mit dem die Konfiguration der Anmeldeinformationen mit Ressourcen der Dienstverwaltung überprüft wird.  

Falls Sie kein klassisches ausführendes Konto erstellt haben, können Sie den unten angegebenen [Beispielcode](#sample-code-to-authenticate-with-resource-manager-resources) zum Authentifizieren gegenüber Resource Manager-Ressourcen und Überprüfen der Anmeldeinformationen verwenden.   

##  <a name="authentication-code-examples"></a>Beispiele für Authentifizierungscode

Mit den folgenden Beispielen soll veranschaulicht werden, wie Sie Ihre Runbooks gegenüber dem Resource Manager oder klassischen Ressourcen mit einem ausführenden Konto authentifizieren.

### <a name="authenticate-with-resource-manager-resources"></a>Durchführen der Authentifizierung mit Resource Manager-Ressourcen
Sie können den unten stehenden aktualisierten Beispielcode aus dem Beispielrunbook **AzureAutomationTutorialScript** verwenden, um sich zum Verwalten von Resource Manager-Ressourcen mit Ihren Runbooks mit dem ausführenden Konto zu authentifizieren.   

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get the connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Signing in to Azure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
       "Setting context to a specific subscription"     
       Set-AzureRmContext -SubscriptionId $SubId              
    }
    catch {
        if (!$servicePrincipalConnection)
        {
           $ErrorMessage = "Connection $connectionName not found."
           throw $ErrorMessage
         } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
         }
    }


Das Skript enthält zwei zusätzliche Codezeilen, um Verweise auf einen Abonnementkontext zu unterstützen und so die problemlose Verwendung mehrerer Abonnements zu ermöglichen. Ein Variablenasset namens „SubscriptionId“ enthält die ID des Abonnements, und nach der Cmdlet-Anweisung „Add-AzureRmAccount“ wird das [Set-AzureRmContext-Cmdlet](https://msdn.microsoft.com/library/mt619263.aspx) mit dem Parametersatz *-SubscriptionId*angegeben. Sollte der Variablenname zu allgemein sein, können Sie den Namen der Variablen mit einem Präfix versehen oder eine andere Namenskonvention anwenden, um einen aussagekräftigeren Variablennamen zu erhalten. Alternativ können Sie anstelle von „-SubscriptionId“ auch den Parametersatz „-SubscriptionName“ mit einem entsprechenden Variablenasset verwenden.    

Beachten Sie das zur Authentifizierung im Runbook verwendete Cmdlet – **Add-AzureRmAccount**verwendet den Parametersatz *ServicePrincipalCertificate* .  Die Authentifizierung wird nicht mit Anmeldeinformationen vorgenommen, sondern mit einem Dienstprinzipalzertifikat.  

### <a name="authenticate-with-service-management-resources"></a>Durchführen der Authentifizierung mit Ressourcen der Dienstverwaltung
Sie können den unten stehenden aktualisierten Beispielcode aus dem Beispielrunbook **AzureClassicAutomationTutorialScript** verwenden, um sich zum Verwalten von klassischen Ressourcen mit Ihren Runbooks mit dem klassischen ausführenden Konto zu authentifizieren.

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get the connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate to Azure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in the Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting the certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in the Automation account."
    }

    Write-Verbose "Authenticating to Azure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu Dienstprinzipalen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](../active-directory/active-directory-application-objects.md).
* Weitere Informationen zur rollenbasierten Zugriffssteuerung in Azure Automation finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md).
* Weitere Informationen zu Zertifikaten und Azure-Diensten finden Sie unter [Übersicht über Zertifikate für Azure Cloud Services](../cloud-services/cloud-services-certs-create.md)


