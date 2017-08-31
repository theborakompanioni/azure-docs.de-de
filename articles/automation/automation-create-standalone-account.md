---
title: "Erstellen eines eigenständigen Azure Automation-Kontos | Microsoft-Dokumentation"
description: "Mit diesem Tutorial werden Sie schrittweise durch die Erstellung, das Testen und eine Beispielverwendung der Authentifizierung per Sicherheitsprinzipal in Azure Automation geführt."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/18/2017
ms.author: magoedte
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: 6eadfb0c3f91c1f2c7783d70604b45d5dc9912a3
ms.contentlocale: de-de
ms.lasthandoff: 08/19/2017

---

# <a name="create-a-standalone-azure-automation-account"></a>Erstellen eines eigenständigen Azure Automation-Kontos
In diesem Thema wird veranschaulicht, wie Sie im Azure-Portal ein Automation-Konto für folgende Zwecke erstellen: Sie möchten Azure Automation evaluieren und kennenlernen, ohne die zusätzlichen Verwaltungslösungen oder die Integration in OMS Log Analytics zur erweiterten Überwachung von Runbookaufträgen zu verwenden.  Sie können diese Verwaltungslösungen später jederzeit hinzufügen oder in Log Analytics integrieren.  Mit dem Automation-Konto können Sie Runbooks authentifizieren, mit denen Ressourcen unter einer Azure Resource Manager-Bereitstellung oder einer klassischen Azure-Bereitstellung verwaltet werden.

Wenn Sie ein Automation-Konto im Azure-Portal erstellen, wird automatisch Folgendes erstellt:

* Ausführendes Konto, das einen neuen Dienstprinzipal in Azure Active Directory und ein Zertifikat erstellt und die Rolle „Mitwirkender“ der rollenbasierten Zugriffssteuerung (RBAC) zuweist, die zum Verwalten von Resource Manager-Ressourcen mit Runbooks verwendet wird.   
* Ein klassisches ausführendes Konto, indem ein Verwaltungszertifikat hochgeladen wird, das zum Verwalten von klassischen Ressourcen mit Runbooks verwendet wird.  

Dies vereinfacht den Prozess für Sie und ermöglicht Ihnen das schnelle Erstellen und Bereitstellen von Runbooks als Unterstützung für Ihre Anforderungen an die Automation.  

## <a name="permissions-required-to-create-automation-account"></a>Erforderliche Berechtigungen zum Erstellen eines Automation-Kontos
Zum Erstellen oder Aktualisieren eines Automation-Kontos müssen Sie über die folgenden spezifischen Berechtigungen verfügen, um das Thema abschließen zu können.   
 
* Damit Sie ein Automation-Konto erstellen können, muss Ihr AD-Benutzerkonto zu einer Rolle mit Berechtigungen hinzugefügt werden, die der Rolle „Besitzer“ für Microsoft.Automation-Ressourcen entspricht. Dies wird im Artikel [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md) erläutert.  
* Wenn für die App-Registrierungseinstellung **Ja** festgelegt ist, können Benutzer ohne Administratorrechte im Azure AD-Mandanten [AD-Anwendungen registrieren](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions).  Wenn für die App-Registrierungen **Nein** festgelegt ist, muss der Benutzer, der diese Aktion ausführt, ein globaler Administrator in Azure AD sein. 

Wenn Sie kein Mitglied der Active Directory-Instanz des Abonnements sind, bevor Sie der Rolle „Globaler Administrator/Co-Administrator“ des Abonnements hinzugefügt werden, werden Sie Active Directory als Gast hinzugefügt. In diesem Fall erhalten Sie eine Warnung der Art „Sie haben keine Berechtigungen zum Erstellen von…“. auf dem Blatt **Automation-Konto hinzufügen**. Benutzer, die zuerst der Rolle des globalen Administrators/Co-Administrators hinzugefügt wurden, können aus der Active Directory-Instanz des Abonnements entfernt und erneut hinzugefügt werden, um sie als Vollbenutzer in Active Directory einzurichten. Sie können dies im Azure-Portal im Bereich **Azure Active Directory** überprüfen. Wählen Sie hierzu **Benutzer und Gruppen**, **Alle Benutzer** und nach Auswahl des jeweiligen Benutzers die Option **Profil**. Als Wert des Attributs **Benutzertyp** im Benutzerprofil darf nicht **Gast** angegeben sein.

## <a name="create-a-new-automation-account-from-the-azure-portal"></a>Erstellen eines neuen Automation-Kontos über das Azure-Portal
In diesem Abschnitt führen Sie die folgenden Schritte aus, um im Azure-Portal ein Azure Automation-Konto zu erstellen.    

1. Melden Sie sich mit einem Konto, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist, beim Azure-Portal an.
2. Klicken Sie auf **Neu**.<br><br> ![Auswählen der Option „Neu“ im Azure-Portal](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  
3. Suchen Sie nach **Automation**, und wählen Sie dann in den Suchergebnissen die Option **Automation + Control**.<br><br> ![Suchen nach und Auswählen von Automation über den Marketplace](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)<br> 
3. Klicken Sie auf dem Blatt „Automation-Konten“ auf **Hinzufügen**.<br><br>![Automation-Konto hinzufügen](media/automation-create-standalone-account/automation-create-automationacct-properties.png)
   
   > [!NOTE]
   > Wenn auf dem Blatt **Automation-Konto hinzufügen** die folgende Warnung angezeigt wird, ist Ihr Konto kein Mitglied der Rolle „Abonnement-Administratoren“ und kein Co-Administrator des Abonnements.<br><br>![Automation-Konto hinzufügen – Warnung](media/automation-create-standalone-account/create-account-without-perms.png)
   > 
   > 
4. Geben Sie auf dem Blatt **Automation-Konto hinzufügen** im Feld **Name** einen Namen für das neue Automation-Konto ein.
5. Wenn Sie über mehrere Abonnements verfügen, geben Sie ein Abonnement für das neue Konto und eine neue oder vorhandene **Ressourcengruppe** sowie den **Standort** eines Azure-Datencenters an.
6. Stellen Sie sicher, dass **Ja** für die Option **Ausführendes Azure-Konto erstellen** ausgewählt ist, und klicken Sie auf die Schaltfläche **Erstellen**.  
   
   > [!NOTE]
   > Wenn Sie **Nein** auswählen und das ausführende Konto nicht erstellen, wird auf dem Blatt **Automation-Konto hinzufügen** eine Warnmeldung angezeigt.  Das Konto wird zwar im Azure-Portal erstellt, verfügt aber nicht über eine entsprechende Authentifizierungsidentität in Ihrem klassischen oder Resource Manager-Abonnementverzeichnisdienst und hat somit keinen Zugriff auf Ressourcen in Ihrem Abonnement.  Runbooks, die auf dieses Konto verweisen, können sich daher nicht authentifizieren und keine Aufgaben für Ressourcen in diesen Bereitstellungsmodellen durchführen.
   > 
   > ![Automation-Konto hinzufügen – Warnung](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)<br>
   > Wenn der Dienstprinzipal nicht erstellt wird, wird die Rolle „Mitwirkender“ nicht zugewiesen.
   > 

7. Während das Automation-Konto in Azure erstellt wird, können Sie den Status unter **Benachrichtigungen** im Menü nachverfolgen.

### <a name="resources-included"></a>Enthaltene Ressourcen
Nach der erfolgreichen Erstellung des Automation-Kontos werden automatisch verschiedene Ressourcen erstellt.  In der folgenden Tabelle sind die Ressourcen für das ausführende Konto zusammengefasst.<br>

| Ressource | Beschreibung |
| --- | --- |
| AzureAutomationTutorial-Runbook |Ein grafisches Beispielrunbook, das die Authentifizierung mithilfe des ausführenden Kontos veranschaulicht und alle Resource Manager-Ressourcen abruft. |
| AzureAutomationTutorialScript-Runbook |Ein PowerShell-Beispielrunbook, das die Authentifizierung mithilfe des ausführenden Kontos veranschaulicht und alle Resource Manager-Ressourcen abruft. |
| AzureRunAsCertificate |Ein Zertifikatasset, das während der Erstellung des Automation-Kontos automatisch oder mit dem unten stehenden PowerShell-Skript für ein vorhandenes Konto erstellt wird.  Das Zertifikat ermöglicht die Authentifizierung bei Azure, sodass Sie Azure Resource Manager-Ressourcen über Runbooks verwalten können.  Dieses Zertifikat ist ein Jahr lang gültig. |
| AzureRunAsConnection |Ein Verbindungsasset, das während der Erstellung des Automation-Kontos automatisch oder mit dem unten stehenden PowerShell-Skript für ein vorhandenes Konto erstellt wird. |

In der folgenden Tabelle sind die Ressourcen für das klassische ausführende Konto zusammengefasst.<br>

| Ressource | Beschreibung |
| --- | --- |
| AzureClassicAutomationTutorial-Runbook |Ein grafisches Beispielrunbook, das alle klassischen virtuellen Computer in einem Abonnement mithilfe des klassischen ausführenden Kontos (Zertifikat) abruft und anschließend den VM-Namen und -Status ausgibt. |
| AzureClassicAutomationTutorialScript-Runbook |Ein PowerShell-Beispielrunbook, das alle klassischen virtuellen Computer in einem Abonnement mithilfe des klassischen ausführenden Kontos (Zertifikat) abruft und anschließend den VM-Namen und -Status ausgibt. |
| AzureClassicRunAsCertificate |Ein automatisch erstelltes Zertifikatasset, das zur Authentifizierung bei Azure verwendet wird, sodass Sie klassische Azure-Ressourcen über Runbooks verwalten können.  Dieses Zertifikat ist ein Jahr lang gültig. |
| AzureClassicRunAsConnection |Ein automatisch erstelltes Verbindungsasset, das zur Authentifizierung bei Azure verwendet wird, sodass Sie klassische Azure-Ressourcen über Runbooks verwalten können. |


## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zur grafischen Inhaltserstellung finden Sie unter [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md).
* Erste Schritte mit PowerShell-Runbooks werden unter [Mein erstes PowerShell-Runbook](automation-first-runbook-textual-powershell.md) beschrieben.
* Die ersten Schritte mit PowerShell-Workflow-Runbooks sind unter [Mein erstes PowerShell-Workflow-Runbook](automation-first-runbook-textual.md)beschrieben.
