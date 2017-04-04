---
title: "Konfigurieren eines ausführenden Kontos in Azure | Microsoft-Dokumentation"
description: "In diesem Tutorial werden Sie schrittweise durch die Erstellung, das Testen und eine Beispielverwendung der Authentifizierung per Sicherheitsprinzipal in Azure Automation geführt."
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
ms.date: 03/27/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: fbca3d195290551d37606e231b997a40a602351f
ms.lasthandoff: 03/28/2017


---

# <a name="authenticate-runbooks-with-an-azure-run-as-account"></a>Authentifizieren von Runbooks mit der Azure-Option „Ausführendes Konto“
In diesem Artikel wird veranschaulicht, wie Sie ein Azure Automation-Konto im Azure-Portal konfigurieren. Hierbei verwenden Sie das Feature „Ausführendes Konto“ zum Authentifizieren von Runbooks, mit denen Ressourcen im Azure Resource Manager oder per Azure-Dienstverwaltung authentifiziert werden.

Wenn Sie ein Automation-Konto im Azure-Portal erstellen, werden automatisch zwei Konten erstellt:

* Ein ausführendes Konto. Für dieses Konto werden ein Dienstprinzipal in Azure Active Directory (Azure AD) und ein Zertifikat erstellt. Außerdem wird die Rolle „Mitwirkender“ der rollenbasierten Zugriffssteuerung (RBAC) zugewiesen, die zum Verwalten von Resource Manager-Ressourcen mit Runbooks verwendet wird.
* Ein klassisches ausführendes Konto. Für dieses Konto wird ein Verwaltungszertifikat hochgeladen, das zum Verwalten der Dienstverwaltung oder von klassischen Ressourcen mit Runbooks verwendet wird.

Die Erstellung eines Automation-Kontos vereinfacht den Prozess für Sie und ermöglicht Ihnen das schnelle Erstellen und Bereitstellen von Runbooks als Unterstützung für Ihre Anforderungen an die Automation.

Mit einem ausführenden Konto und einem klassischen ausführenden Konto haben Sie folgende Möglichkeiten:

* Bereitstellen eines Standardverfahrens für die Authentifizierung bei Azure, wenn Resource Manager-Ressourcen oder Ressourcen der Dienstverwaltung mithilfe von Runbooks im Azure-Portal verwaltet werden
* Automatisieren der Nutzung von globalen Runbooks, die Sie in Azure-Warnungen konfigurieren können

> [!NOTE]
> Für das Azure-Feature zum [Integrieren von Warnungen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) in globale Automation-Runbooks ist ein Automation-Konto erforderlich, das mit einem ausführenden Konto und einem klassischen ausführenden Konto konfiguriert ist. Sie können ein Automation-Konto auswählen, für das bereits ein ausführendes Konto und ein klassisches ausführendes Konto definiert sind, oder Sie können ein neues Automation-Konto erstellen.
>  

In diesem Artikel erfahren Sie, wie Sie ein Automation-Konto über das Azure-Portal erstellen, ein Automation-Konto mit Azure PowerShell aktualisieren, die Kontokonfiguration verwalten und die Authentifizierung in Ihren Runbooks durchführen.

Bevor Sie mit der Erstellung eines Automation-Kontos beginnen, ist es ratsam, Folgendes zu verstehen und zu berücksichtigen:

* Die Erstellung eines Automation-Kontos wirkt sich nicht auf Automation-Konten aus, die Sie mit dem klassischen oder Resource Manager-Bereitstellungsmodell ggf. bereits erstellt haben.
* Der Prozess funktioniert nur für Automation-Konten, die Sie im Azure-Portal erstellen. Wenn Sie versuchen, ein Konto über das klassische Azure-Portal zu erstellen, wird die Konfiguration des ausführenden Kontos nicht repliziert.
* Falls Sie bereits über Runbooks und Assets verfügen (z.B. Zeitpläne oder Variablen), um klassische Ressourcen zu verwalten, und Runbooks zum Authentifizieren mit dem neuen klassischen ausführenden Konto verwenden möchten, haben Sie folgende Möglichkeiten:

  * Befolgen Sie zum Erstellen eines klassischen ausführenden Kontos die Anleitung im Abschnitt „Verwalten des ausführenden Kontos“. 
  * Verwenden Sie zum Aktualisieren Ihres vorhandenen Kontos das PowerShell-Skript im Abschnitt „Aktualisieren des Automation-Kontos mit PowerShell“.
* Zur Authentifizierung mit dem neuen ausführenden Konto und klassischen ausführenden Automation-Konto müssen Sie Ihre vorhandenen Runbooks mit dem Beispielcode ändern, der im Abschnitt [Beispiele für Authentifizierungscode](#authentication-code-examples) angegeben ist.

    >[!NOTE]
    >Das ausführende Konto ist für die Authentifizierung bei Resource Manager-Ressourcen mit dem zertifikatbasierten Dienstprinzipal bestimmt. Das klassische ausführende Konto ist für die Authentifizierung bei Dienstverwaltungsressourcen mit einem Verwaltungszertifikat bestimmt.

## <a name="create-an-automation-account-from-the-azure-portal"></a>Erstellen eines Automation-Kontos über das Azure-Portal
In diesem Abschnitt erstellen Sie ein Azure Automation-Konto über das Azure-Portal, mit dem wiederum ein ausführendes Konto und ein klassisches ausführendes Konto erstellt werden.

>[!NOTE]
>Zur Erstellung eines Automation-Kontos müssen Sie Mitglied der Rolle „Dienstadministratoren“ oder Co-Administrator des Abonnements sein, worüber Zugriff auf das Abonnement gewährt wird. Außerdem müssen Sie der Active Directory-Standardinstanz dieses Abonnements als Benutzer hinzugefügt worden sein. Dem Konto muss dabei keine privilegierte Rolle zugewiesen sein.
>
>Wenn Sie kein Mitglied der Active Directory-Instanz des Abonnements sind, bevor Sie der Rolle „Co-Administrator“ des Abonnements hinzugefügt werden, werden Sie Active Directory als Gast hinzugefügt. In diesem Fall erhalten Sie eine Warnung der Art „Sie haben keine Berechtigungen zum Erstellen von…“ auf dem Blatt **Automation-Konto hinzufügen**.
>
>Benutzer, die zuerst der Rolle des Co-Administrators hinzugefügt wurden, können aus der Active Directory-Instanz des Abonnements entfernt und erneut hinzugefügt werden, um sie als Vollbenutzer in Active Directory einzurichten. Sie können dies im Azure-Portal im Bereich **Azure Active Directory** überprüfen. Wählen Sie hierzu **Benutzer und Gruppen**, **Alle Benutzer** und nach Auswahl des jeweiligen Benutzers die Option **Profil**. Als Wert des Attributs **Benutzertyp** im Benutzerprofil darf nicht **Gast** angegeben sein.
>

1. Melden Sie sich mit einem Konto, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist, beim Azure-Portal an.

2. Wählen Sie die Option **Automation-Konten**.

3. Klicken Sie auf dem Blatt **Automation-Konten** auf **Hinzufügen**.
Das Blatt **Automation-Konto hinzufügen** wird geöffnet.

 ![Blatt „Automation-Konto hinzufügen“](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)

   > [!NOTE]
   > Wenn Ihr Konto kein Mitglied der Rolle „Abonnement-Administratoren“ und kein Co-Administrator des Abonnements ist, wird auf dem Blatt **Automation-Konto hinzufügen** die folgende Warnung angezeigt:
   >
   >![Automation-Konto hinzufügen – Warnung](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   >
   >

4. Geben Sie auf dem Blatt **Automation-Konto hinzufügen** im Feld **Name** einen Namen für das neue Automation-Konto ein.

5. Gehen Sie wie folgt vor, wenn Sie mehr als ein Abonnement verwenden:

    a. Geben Sie unter **Abonnement** ein Abonnement für das neue Konto an.

    b. Klicken Sie unter **Ressourcengruppe** auf **Neu erstellen** oder **Vorhandene verwenden**.

    c. Geben Sie unter **Standort** ein Azure-Datencenter an.

6. Wählen Sie unter **Ausführendes Azure-Konto erstellen** die Option **Ja**, und klicken Sie auf **Erstellen**.

   > [!NOTE]
   > Wenn Sie das ausführende Konto nicht erstellen, indem Sie **Nein** wählen, wird auf dem Blatt **Automation-Konto hinzufügen** eine Warnmeldung angezeigt. Das Konto wird zwar im Azure-Portal erstellt, aber es verfügt nicht über eine entsprechende Authentifizierungsidentität in Ihrem klassischen oder Resource Manager-Abonnementverzeichnisdienst. Das Konto hat somit keinen Zugriff auf die Ressourcen Ihres Abonnements. Runbooks, die auf dieses Konto verweisen, können sich daher nicht authentifizieren und keine Aufgaben für Ressourcen in diesen Bereitstellungsmodellen ausführen.
   >
   > ![Warnmeldung auf dem Blatt „Automation-Konto hinzufügen“](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)
   >
   > Da das Dienstprinzipal nicht erstellt wird, wird die Rolle „Mitwirkender“ nicht zugewiesen.
   >

7. Während das Automation-Konto in Azure erstellt wird, können Sie den Status unter **Benachrichtigungen** im Menü nachverfolgen.

### <a name="resources"></a>Ressourcen
Nach der erfolgreichen Erstellung des Automation-Kontos werden automatisch verschiedene Ressourcen erstellt. Die Ressourcen werden in den folgenden beiden Tabellen zusammengefasst:

#### <a name="run-as-account-resources"></a>Ressourcen des ausführenden Kontos

| Ressource | Beschreibung |
| --- | --- |
| AzureAutomationTutorial-Runbook | Ein grafisches Beispielrunbook, das die Authentifizierung mithilfe des ausführenden Kontos veranschaulicht und alle Resource Manager-Ressourcen abruft. |
| AzureAutomationTutorialScript-Runbook | Ein PowerShell-Beispielrunbook, das die Authentifizierung mithilfe des ausführenden Kontos veranschaulicht und alle Resource Manager-Ressourcen abruft. |
| AzureRunAsCertificate | Dies ist das Zertifikatasset, das automatisch erstellt wird, wenn Sie ein Automation-Konto erstellen oder das folgende PowerShell-Skript für ein vorhandenes Konto verwenden. Mit dem Zertifikat ist die Authentifizierung bei Azure möglich, sodass Sie Azure Resource Manager-Ressourcen über Runbooks verwalten können. Das Zertifikat ist ein Jahr lang gültig. |
| AzureRunAsConnection | Dies ist das Verbindungsasset, das automatisch erstellt wird, wenn Sie ein Automation-Konto erstellen oder das PowerShell-Skript für ein vorhandenes Konto verwenden. |

#### <a name="classic-run-as-account-resources"></a>Ressourcen des klassischen ausführenden Kontos

| Ressource | Beschreibung |
| --- | --- |
| AzureClassicAutomationTutorial-Runbook | Ein grafisches Beispielrunbook zum Abrufen aller VMs, die mit dem klassischen Bereitstellungsmodell eines Abonnements erstellt werden, indem das klassische ausführende Konto (Zertifikat) verwendet wird. Anschließend werden der VM-Name und der -Status geschrieben. |
| AzureClassicAutomationTutorialScript-Runbook | Ein PowerShell-Beispielrunbook, das alle klassischen virtuellen Computer in einem Abonnement mithilfe des klassischen ausführenden Kontos (Zertifikat) abruft und anschließend den VM-Namen und -Status schreibt. |
| AzureClassicRunAsCertificate | Das automatisch erstellte Zertifikatasset, das Sie zum Authentifizieren bei Azure verwenden, sodass Sie klassische Azure-Ressourcen über Runbooks verwalten können. Das Zertifikat ist ein Jahr lang gültig. |
| AzureClassicRunAsConnection | Das automatisch erstellte Verbindungsasset, das Sie zum Authentifizieren bei Azure verwenden, sodass Sie klassische Azure-Ressourcen über Runbooks verwalten können. |

## <a name="verify-run-as-authentication"></a>Überprüfen der Authentifizierung mit dem ausführenden Konto
Führen Sie einen kleinen Test durch, um sicherzustellen, dass Sie sich mit dem neuen ausführenden Konto authentifizieren können.

1. Öffnen Sie im Azure-Portal das zuvor erstellte Automation-Konto.

2. Klicken Sie auf die Kachel **Runbooks** , um die Liste mit den Runbooks zu öffnen.

3. Wählen Sie das Runbook **AzureAutomationTutorialScript** aus, und klicken Sie auf **Starten**, um das Runbook zu starten. Die folgenden Ereignisse treten auf:
 * Es wird ein [Runbookauftrag](automation-runbook-execution.md) erstellt, das Blatt **Auftrag** geöffnet und der Auftragsstatus auf der Kachel **Auftragszusammenfassung** angezeigt.
 * Der Auftrag besitzt zunächst den Status **In Warteschlange**. So wird angegeben, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird.
 * Der Status ändert sich in **Wird gestartet**, wenn ein Worker den Auftrag beansprucht.
 * Der Status ändert sich in **Wird ausgeführt**, wenn die Ausführung des Runbooks beginnt.
 * Nachdem der Runbookauftrag abgeschlossen ist, sollte der Status **Abgeschlossen** lauten.

       ![Security Principal Runbook Test](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)
4. Um die detaillierten Ergebnisse des Runbooks anzuzeigen, klicken Sie auf die Kachel **Ausgabe**.  
Das Blatt **Ausgabe** wird mit dem Hinweis angezeigt, dass die Authentifizierung des Runbooks erfolgreich war und eine Liste aller verfügbaren Ressourcen in der Ressourcengruppe zurückgegeben wurde.

5. Schließen Sie das Blatt **Ausgabe**, um zum Blatt **Auftragszusammenfassung** zurückzukehren.

6. Schließen Sie das Blatt **Auftragszusammenfassung** und das entsprechende Runbookblatt **AzureAutomationTutorialScript**.

## <a name="verify-classic-run-as-authentication"></a>Überprüfen der Authentifizierung mit dem klassischen ausführenden Konto
Führen Sie einen ähnlichen Test durch, um sicherzustellen, dass Sie sich mit dem neuen klassischen ausführenden Konto authentifizieren können.

1. Öffnen Sie im Azure-Portal das zuvor erstellte Automation-Konto.

2. Klicken Sie auf die Kachel **Runbooks** , um die Liste mit den Runbooks zu öffnen.

3. Wählen Sie das Runbook **AzureClassicAutomationTutorialScript** aus, und klicken Sie auf **Starten**, um das Runbook zu starten. Die folgenden Ereignisse treten auf:

 * Es wird ein [Runbookauftrag](automation-runbook-execution.md) erstellt, das Blatt **Auftrag** geöffnet und der Auftragsstatus auf der Kachel **Auftragszusammenfassung** angezeigt.
 * Der Auftrag besitzt zunächst den Status **In Warteschlange**. So wird angegeben, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird.
 * Der Status ändert sich in **Wird gestartet**, wenn ein Worker den Auftrag beansprucht.
 * Der Status ändert sich in **Wird ausgeführt**, wenn die Ausführung des Runbooks beginnt.
 * Nachdem der Runbookauftrag abgeschlossen ist, sollte der Status **Abgeschlossen** lauten.

    ![Sicherheitsprinzipal-Runbooktest](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
4. Um die detaillierten Ergebnisse des Runbooks anzuzeigen, klicken Sie auf die Kachel **Ausgabe**.  
Das Blatt **Ausgabe** wird mit dem Hinweis angezeigt, dass die Authentifizierung des Runbooks erfolgreich war und eine Liste mit allen klassischen VMs des Abonnements zurückgegeben wurde.

5. Schließen Sie das Blatt **Ausgabe**, um zum Blatt **Auftragszusammenfassung** zurückzukehren.

6. Schließen Sie das Blatt **Auftragszusammenfassung** und das entsprechende Runbookblatt **AzureAutomationTutorialScript**.

## <a name="managing-your-run-as-account"></a>Verwalten des ausführenden Kontos
Da Ihr Automation-Konto irgendwann abläuft, müssen Sie das Zertifikat erneuern. Wenn Sie der Meinung sind, dass das ausführende Konto kompromittiert wurde, können Sie es löschen und neu erstellen. In diesem Abschnitt wird beschrieben, wie Sie dabei vorgehen.

### <a name="self-signed-certificate-renewal"></a>Erneuerung eines selbstsignierten Zertifikats
Das selbstsignierte Zertifikat, das Sie für das ausführende Konto erstellt haben, läuft ein Jahr nach dem Erstellungsdatum ab. Sie können es vor dem Ablaufdatum jederzeit erneuern. Beim Erneuern wird das aktuelle gültige Zertifikat beibehalten, um sicherzustellen, dass sich keine negativen Auswirkungen auf Runbooks ergeben, die sich in der Warteschlange befinden oder aktiv ausgeführt werden und die mit dem ausführenden Konto authentifiziert werden. Das Zertifikat bleibt bis zum Ablaufdatum gültig.

> [!NOTE]
> Wenn Sie Ihr ausführendes Automation-Konto für die Verwendung eines Zertifikats konfiguriert haben, das von Ihrer Unternehmenszertifizierungsstelle ausgestellt wurde, und diese Option verwenden, wird das Unternehmenszertifikat durch ein selbstsigniertes Zertifikat ersetzt.

Führen Sie folgende Schritte aus, um das Zertifikat zu erneuern:

1. Öffnen Sie das Automation-Konto im Azure-Portal.

2. Wählen Sie auf dem Blatt **Automation-Konto** im Bereich **Kontoeigenschaften** unter **Kontoeinstellungen** die Option **Ausführende Konten**.

    ![Eigenschaftenbereich des Automation-Kontos](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)
3. Wählen Sie auf dem Blatt mit den Eigenschaft für **Ausführende Konten** entweder das ausführende Konto oder das klassische ausführende Konto aus, für das Sie das Zertifikat erneuern möchten.

4. Klicken Sie auf dem Blatt **Eigenschaften** für das ausgewählte Konto auf **Zertifikat erneuern**.

    ![Erneuern des Zertifikats für das ausführende Konto](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)

5. Während das Zertifikat erneuert wird, können Sie den Status im Menü unter **Benachrichtigungen** verfolgen.

### <a name="delete-a-run-as-or-classic-run-as-account"></a>Löschen eines ausführenden Kontos oder klassischen ausführenden Kontos
In diesem Abschnitt wird beschrieben, wie Sie ein ausführendes Konto oder klassisches ausführendes Konto löschen und neu erstellen. Bei dieser Aktion wird das Automation-Konto beibehalten. Nach dem Löschen eines ausführenden Kontos oder klassischen ausführenden Kontos können Sie es im Azure-Portal neu erstellen.

1. Öffnen Sie das Automation-Konto im Azure-Portal.

2. Wählen Sie auf dem Blatt **Automation-Konto** im Bereich mit den Kontoeigenschaften die Option **Ausführende Konten**.

3. Wählen Sie auf dem Blatt mit den Eigenschaft für **Ausführende Konten** entweder das ausführende Konto oder das klassische ausführende Konto aus, das Sie löschen möchten. Klicken Sie anschließend auf dem Blatt **Eigenschaften** für das ausgewählte Konto auf **Löschen**.

 ![Löschen des ausführenden Azure-Kontos](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)

4. Während das Konto gelöscht wird, können Sie den Status im Menü unter **Benachrichtigungen** verfolgen.

5. Nach dem Löschen des Kontos können Sie das Konto neu erstellen, indem Sie auf dem Eigenschaftenblatt **Ausführende Konten** die Erstellungsoption für **Ausführendes Azure-Konto** auswählen.

 ![Neuerstellen des ausführenden Automation-Kontos](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)

### <a name="misconfiguration"></a>Fehlkonfiguration
Einige Konfigurationselemente, die erforderlich sind, damit das ausführende Konto bzw. das klassische ausführende Konto richtig funktioniert, wurden ggf. gelöscht oder bei der anfänglichen Einrichtung nicht richtig erstellt. Beispiele für diese Elemente sind:

* Zertifikatasset
* Verbindungsasset
* Entfernung des ausführenden Kontos aus der Rolle „Mitwirkender“
* Dienstprinzipal oder Anwendung in Azure AD

In den obigen und anderen Fällen einer Fehlkonfiguration erkennt das Automation-Konto die Änderungen und zeigt auf dem Eigenschaftenblatt **Ausführende Konten** des Kontos den Status *Unvollständig* an.

![Konfigurationsstatus „Unvollständig“ für ausführendes Konto](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)

Wenn Sie das ausführende Konto auswählen, wird im Bereich **Eigenschaften** die folgende Fehlermeldung angezeigt:

![Warnmeldung mit Hinweis auf eine unvollständige Konfiguration des ausführenden Kontos](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png).

Sie können diese Probleme mit ausführenden Konten schnell beheben, indem Sie das Konto löschen und neu erstellen.

## <a name="update-your-automation-account-by-using-powershell"></a>Aktualisieren des Automation-Kontos mit PowerShell
Sie können PowerShell verwenden, um Ihr vorhandenes Automation-Konto zu aktualisieren, wenn Folgendes gegeben ist:

* Sie erstellen ein Automation-Konto, aber lehnen die Erstellung des ausführenden Kontos ab.
* Sie verwenden bereits ein Automation-Konto zum Verwalten von Resource Manager-Ressourcen und möchten das Konto aktualisieren, um das ausführende Konto für die Runbook-Authentifizierung einzuschließen.
* Sie verwenden bereits ein Automation-Konto zum Verwalten klassischer Ressourcen und möchten es zur Verwendung des klassischen ausführenden Kontos aktualisieren, anstatt ein neues Konto zu erstellen und Ihre Runbooks und Assets zu diesem Konto zu migrieren.   
* Sie möchten ein ausführendes Konto und ein klassisches ausführendes Konto erstellen, indem Sie ein Zertifikat nutzen, das von Ihrer Unternehmenszertifizierungsstelle ausgestellt wurde.

Für das Skript ist Folgendes erforderlich:

* Das Skript kann nur unter Windows 10 und Windows Server 2016 mit Azure Resource Manager-Modul 2.01 und höher ausgeführt werden. Unter älteren Windows-Versionen wird das Skript nicht unterstützt.
* Azure PowerShell 1.0 und höher. Informationen zur PowerShell 1.0-Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).
* Ein Automation-Konto, auf das im folgenden PowerShell-Skript als Wert für die Parameter *–AutomationAccountName* und *-ApplicationDisplayName* verwiesen wird.

Gehen Sie wie folgt vor, um die Werte für *SubscriptionID*, *ResourceGroup* und *AutomationAccountName* zu erhalten. Hierbei handelt es sich um erforderliche Parameter für die Skripts:
1. Wählen Sie im Azure-Portal auf dem Blatt **Automation-Konto** Ihr Automation-Konto aus, und wählen Sie dann die Option **Alle Einstellungen**. 
2. Wählen Sie auf dem Blatt **Alle Einstellungen** unter **Kontoeinstellungen** die Option **Eigenschaften**. 
3. Notieren Sie sich die Werte auf dem Blatt **Eigenschaften**.

 ![Blatt „Eigenschaften“ des Automation-Kontos](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-a-run-as-account-powershell-script"></a>Erstellen eines PowerShell-Skripts für ein ausführendes Konto
Dieses PowerShell-Skript unterstützt folgende Konfigurationen:

* Erstellen eines ausführenden Kontos mit einem selbstsignierten Zertifikat
* Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat
* Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem Unternehmenszertifikat
* Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat in der Azure Government-Cloud

Abhängig von der gewählten Konfigurationsoption werden mit dem Skript die folgenden Elemente erstellt.

**Für ausführende Konten:**

* Erstellt eine Azure AD-Anwendung, die entweder mit dem öffentlichen Schlüssel des selbstsignierten Zertifikats oder des Unternehmenszertifikats exportiert wird, und ein Dienstprinzipalkonto für die Anwendung in Azure AD. Außerdem wird die Rolle „Mitwirkender“ für das Konto in Ihrem aktuellen Abonnement zugewiesen. Sie können diese Einstellung auch in „Besitzer“ oder eine beliebige andere Rolle ändern. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md).
* Erstellt ein Automation-Zertifikatasset mit dem Namen *AzureRunAsCertificate* im angegebenen Automation-Konto. Das Zertifikatasset enthält den privaten Schlüssel des Zertifikats, der von der Azure AD-Anwendung verwendet wird.
* Erstellt ein Automation-Verbindungsasset mit dem Namen *AzureRunAsConnection* im angegebenen Automation-Konto. Das Verbindungsasset enthält die applicationId, tenantId, subscriptionId und den Zertifikatfingerabdruck.

**Für klassische ausführende Konten:**

* Erstellt ein Automation-Zertifikatasset mit dem Namen *AzureClassicRunAsCertificate* im angegebenen Automation-Konto. Das Zertifikatasset enthält den privaten Zertifikatschlüssel, der vom Verwaltungszertifikat verwendet wird.
* Erstellt ein Automation-Verbindungsasset mit dem Namen *AzureClassicRunAsConnection* im angegebenen Automation-Konto. Das Verbindungsasset enthält den Abonnementnamen, die subscriptionId und den Namen des Zertifikatassets.

>[!NOTE]
> Wenn Sie eine Option zum Erstellen eines klassischen ausführenden Kontos wählen, müssen Sie nach dem Ausführen des Skripts das öffentliche Zertifikat (Dateinamenerweiterung „.cer“) in den Verwaltungsspeicher für das Abonnement hochladen, unter dem das Automation-Konto erstellt wurde.
> 

Gehen Sie wie folgt vor, um das Skript auszuführen und das Zertifikat hochzuladen:

1. Speichern Sie das folgende Skript auf dem Computer. In diesem Beispiel speichern Sie es mit dem Dateinamen *New-RunAsAccount.ps1*.

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

        # Use key credentials and create an Azure AD application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
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

        # Create a Run As account by using a service principal
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

        # Create a service principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate the ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}


        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create a Run As account by using a service principal
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

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. Klicken Sie auf Ihrem Computer auf **Start**, und starten Sie **Windows PowerShell** mit erhöhten Benutzerrechten.

3. Wechseln Sie von der PowerShell-Befehlszeilenshell mit erhöhten Rechten zum Ordner, der das in Schritt 1 erstellte Skript enthält.

4. Führen Sie das Skript aus, indem Sie die Parameterwerte für die benötigte Konfiguration verwenden.

    **Erstellen eines ausführenden Kontos mit einem selbstsignierten Zertifikat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    **Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem Unternehmenszertifikat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat in der Azure Government-Cloud**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > Nach dem Ausführen des Skripts werden Sie aufgefordert, sich gegenüber Azure zu authentifizieren. Melden Sie sich mit einem Konto an, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist.
    >
    >

Beachten Sie nach dem erfolgreichen Ausführen des Skripts Folgendes:
* Falls Sie ein klassisches ausführendes Konto mit einem selbstsignierten öffentlichen Zertifikat (CER-Datei) erstellt haben, führt das Skript die Erstellung durch und speichert es im Ordner für temporäre Dateien auf Ihrem Computer unter dem Benutzerprofil *%USERPROFILE%\AppData\Local\Temp*, das Sie zum Ausführen der PowerShell-Sitzung verwendet haben.
* Wenn Sie ein klassisches ausführendes Konto mit einem öffentlichen Unternehmenszertifikat (CER-Datei) erstellt haben, sollten Sie dieses Zertifikat verwenden. Befolgen Sie die Anleitung zum [Hochladen eines Verwaltungs-API-Zertifikats in das klassische Azure-Portal](../azure-api-management-certs.md), und überprüfen Sie anschließend die Konfiguration der Anmeldeinformationen mit Service Management-Ressourcen, indem Sie den [Beispielcode für die Authentifizierung mit Service Management-Ressourcen](#sample-code-to-authenticate-with-service-management-resources) verwenden. 
* Falls Sie *kein* klassisches ausführendes Konto erstellt haben, können Sie die Authentifizierung mit Resource Manager-Ressourcen durchführen und die Konfiguration der Anmeldeinformationen überprüfen, indem Sie den [Beispielcode für die Authentifizierung mit Resource Manager-Ressourcen](#sample-code-to-authenticate-with-resource-manager-resources) verwenden.

## <a name="sample-code-to-authenticate-with-resource-manager-resources"></a>Beispielcode für die Authentifizierung mit Resource Manager-Ressourcen
Sie können den folgenden aktualisierten Beispielcode aus dem Beispielrunbook *AzureAutomationTutorialScript* verwenden, um sich zum Verwalten von Resource Manager-Ressourcen mit Ihren Runbooks mit dem ausführenden Konto zu authentifizieren.

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

Um die einfache Verwendung mehrerer Abonnements zu ermöglichen, enthält das Skript zwei zusätzliche Codezeilen, mit denen das Verweisen auf einen Abonnementkontext unterstützt wird. Ein Variablenasset mit dem Namen *SubscriptionId* enthält die ID des Abonnements. Nach der `Add-AzureRmAccount`-Cmdletanweisung wird das [`Set-AzureRmContext`](https://msdn.microsoft.com/library/mt619263.aspx)-Cmdlet mit dem Parametersatz *-SubscriptionId* angegeben. Wenn der Variablenname zu generisch ist, können Sie ihn ändern und ein Präfix hinzufügen oder eine andere Benennungskonvention nutzen, um die Identifizierung zu vereinfachen. Alternativ können Sie anstelle von *-SubscriptionId* auch den Parametersatz *-SubscriptionName* mit einem entsprechenden Variablenasset verwenden.

Für das Cmdlet, das Sie für die Authentifizierung im Runbook nutzen (`Add-AzureRmAccount`), wird der Parametersatz *ServicePrincipalCertificate* verwendet. Die Authentifizierung wird nicht mit Anmeldeinformationen vorgenommen, sondern mit dem Dienstprinzipalzertifikat.

## <a name="sample-code-to-authenticate-with-service-management-resources"></a>Beispielcode für die Authentifizierung mit Service Management-Ressourcen
Sie können den folgenden aktualisierten Beispielcode aus dem Beispielrunbook *AzureClassicAutomationTutorialScript* verwenden, um sich zum Verwalten von klassischen Ressourcen mit Ihren Runbooks mit dem klassischen ausführenden Konto zu authentifizieren.

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
* [Anwendungs- und Dienstprinzipalobjekte in Azure AD](../active-directory/active-directory-application-objects.md)
* [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md)
* [Übersicht über Zertifikate für Azure Cloud Services](../cloud-services/cloud-services-certs-create.md)

