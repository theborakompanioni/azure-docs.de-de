<properties 
   pageTitle="Rollenbasierte Zugriffsteuerung in Azure Automation | Microsoft Azure"
   description="Die rollenbasierte Zugriffssteuerung (RBAC) ermöglicht eine präzise Zugriffsverwaltung für Azure-Ressourcen. Dieser Artikel beschreibt, wie eine rollenbasierte Zugriffssteuerung in Azure Automation eingerichtet wird."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="Automation RBAC, rollenbasierte Zugriffssteuerung, Azure RBAC" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/12/2016"
   ms.author="magoedte;sngun"/>

# Rollenbasierte Zugriffssteuerung in Azure Automation

## Rollenbasierte Zugriffssteuerung

Die rollenbasierte Zugriffssteuerung (RBAC) ermöglicht eine präzise Zugriffsverwaltung für Azure-Ressourcen. Mithilfe der [rollenbasierten Zugriffssteuerung](../active-directory/role-based-access-control-configure.md) können Sie Aufgaben innerhalb Ihres Teams verteilen und Benutzern, Gruppen und Anwendungen nur den Zugriff gewähren, den diese zur Ausführung ihrer Aufgaben benötigen. Den Benutzern kann die rollenbasierte Zugriffssteuerung über das Azure-Portal, Azure Befehlszeilenwerkzeuge oder Azure Management APIs zugewiesen werden.

## Rollenbasierte Zugriffssteuerung in Automation-Konten

Der Zugriff wird in Azure Automation erteilt, indem den Benutzern, Gruppen und Anwendungen im Automation-Konten-Bereich die passende RBAC-Rolle zugewiesen wird. Die folgenden vordefinierten Rollen werden von Automation-Konten unterstützt:

|**Rolle** | **Beschreibung** |
|:--- |:---|
| Besitzer | Die Rolle „Besitzer“ erlaubt den Zugriff auf alle Ressourcen und Aktionen innerhalb des Automation-Kontos, z.B. den Zugriff auf andere Benutzer, Gruppen und Anwendungen, um das Automation-Konto zu verwalten. |
| Mitwirkender | Die Rolle „Mitwirkender“ erlaubt Ihnen, fast alles zu verwalten. Das Einzige, was Sie nicht können, ist das Ändern der Zugriffsberechtigungen für ein Automation-Konto anderer Benutzer. |
| Leser | Die Rolle „Leser“ erlaubt Ihnen, alle Ressourcen im Automation-Konto zu betrachten, aber Sie können keine Änderungen vornehmen.|
| Operator für Automation | Die Rolle „Operator für Automation“ erlaubt Ihnen operative Aufgaben wie das Starten, Anhalten, Unterbrechen, Fortführen und Einplanen von Aufgaben. Diese Rolle ist hilfreich, wenn Sie Ihre Automation-Konten-Ressourcen wie Anmeldeinformationsobjekte und Runbooks vor Einblicken und Änderungen schützen wollen, es Mitgliedern Ihrer Organisation aber dennoch ermöglichen wollen, diese Runbooks auszuführen. |
| Benutzerzugriffsadministrator | Mit der Rolle „Benutzerzugriffsadministrator“ können Sie den Benutzerzugriff auf Azure Automation-Konten verwalten. |

>[AZURE.NOTE] Es ist nicht möglich, Zugriffsrechte bestimmten Runbooks zu gewähren, sondern nur für die Ressourcen und Aktionen im Automation-Konto.

In diesem Artikel zeigen wir Ihnen Schritt für Schritt, wie Sie eine rollenbasierte Zugriffssteuerung in Azure Automation einrichten. Zuerst sehen wir uns aber die einzelnen Berechtigungen genauer an, die für die Rollen „Mitwirkender“, „Leser“, „Operator für Automation“ und „Benutzerzugriffsadministrator“ gewährt werden. Das Ziel ist ein gutes Verständnis der Berechtigungen, bevor wir Benutzern Rechte für das Automation-Konto gewähren. Andernfalls kann dies unbeabsichtigte oder unerwünschte Konsequenzen haben.

## Berechtigungen für die Rolle „Mitwirkender“

Die folgende Tabelle enthält die speziellen Aktionen, die von der Rolle „Mitwirkender“ in Automation durchgeführt werden können.

| **Ressourcentyp** | **Lesen** | **Schreiben** | **Löschen** | **Andere Aktionen** |
|:--- |:---|:--- |:---|:--- |
| Azure Automation-Konto | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | 
| Automation-Zertifikatobjekt | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation-Verbindungsobjekt | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | 
| Automation-Verbindungstypasset | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | 
| Automation-Anmeldeinformationsobjekt | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation-Zeitplanasset | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation-Variablenressource | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation-Konfiguration für den gewünschten Zustand | | | | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) |
| Hybrid Runbook Worker-Ressourcentyp | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | 
| Azure Automation-Auftrag | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | 
| Automation-Auftragsdatenstrom | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | | 
| Automation-Auftragszeitplan | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation-Modul | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | |
| Azure Automation-Runbook | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) |
| Automation-Runbookentwurf | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) |
| Testauftrag für Automation-Runbookentwurf | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | 
| Automation-Webhook | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) |

## Berechtigungen für die Rolle „Leser“

Die folgende Tabelle enthält die speziellen Aktionen, die von der Rolle „Leser“ in Automation durchgeführt werden können.

| **Ressourcentyp** | **Lesen** | **Schreiben** | **Löschen** | **Andere Aktionen** |
|:--- |:---|:--- |:---|:--- |
| Administrator für klassisches Abonnement | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | 
| Verwaltungssperre | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | 
| Berechtigung | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | |
| Anbietervorgänge | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | 
| Rollenzuweisung | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | 
| Rollendefinition | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | 

## Berechtigungen für die Rolle „Operator für Automation“

Die folgende Tabelle enthält die speziellen Aktionen, die von der Rolle „Operator für Automation“ in Automation durchgeführt werden können.

| **Ressourcentyp** | **Lesen** | **Schreiben** | **Löschen** | **Andere Aktionen** |
|:--- |:---|:--- |:---|:--- |
| Azure Automation-Konto | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | 
| Automation-Zertifikatobjekt | | | |
| Automation-Verbindungsobjekt | | | |
| Automation-Verbindungstypasset | | | |
| Automation-Anmeldeinformationsobjekt | | | |
| Automation-Zeitplanasset | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | |
| Automation-Variablenressource | | | |
| Automation-Konfiguration für den gewünschten Zustand | | | | |
| Hybrid Runbook Worker-Ressourcentyp | | | | | 
| Azure Automation-Auftrag | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | 
| Automation-Auftragsdatenstrom | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | |  
| Automation-Auftragszeitplan | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | |
| Automation-Modul | | | |
| Azure Automation-Runbook | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-Runbookentwurf | | | |
| Testauftrag für Automation-Runbookentwurf | | | |  
| Automation-Webhook | | | |

Ausführlichere Informationen finden Sie unter [Operator für Automation – Aktionen](../active-directory/role-based-access-built-in-roles.md#automation-operator). Hier sind die von der Rolle „Operator für Automation“ im Automation-Konto unterstützten Aktionen sowie dessen Ressourcen aufgelistet.

## Berechtigungen für die Rolle „Benutzerzugriffsadministrator“

Die folgende Tabelle enthält die speziellen Aktionen, die von der Rolle „Benutzerzugriffsadministrator“ in Automation durchgeführt werden können.

| **Ressourcentyp** | **Lesen** | **Schreiben** | **Löschen** | **Andere Aktionen** |
|:--- |:---|:--- |:---|:--- |
| Azure Automation-Konto | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-Zertifikatobjekt | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-Verbindungsobjekt | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-Verbindungstypasset | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-Anmeldeinformationsobjekt | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-Zeitplanasset | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-Variablenressource | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-Konfiguration für den gewünschten Zustand | | | | |
| Hybrid Runbook Worker-Ressourcentyp | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | | 
| Azure Automation-Auftrag | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | | 
| Automation-Auftragsdatenstrom | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | | 
| Automation-Auftragszeitplan | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-Modul | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Azure Automation-Runbook | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation-Runbookentwurf | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Testauftrag für Automation-Runbookentwurf | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | | | 
| Automation-Webhook | ![Grüner Status](media/automation-role-based-access-control/green-checkmark.png) | | |

## Konfigurieren einer rollenbasierten Zugriffssteuerung für Automation-Konten mittels Azure-Portal

1.	Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und öffnen Sie auf dem Blatt „Automation-Konten“ Ihr Automation-Konto.

2.	Klicken Sie oben rechts auf die **Zugriffssteuerung**. Dadurch wird das Blatt **Benutzer** geöffnet, auf dem Sie neue Benutzer, Gruppen und Anwendungen hinzufügen können, um Ihr Automation-Konto zu verwalten. Des Weiteren können Sie die vorhandenen Rollen anzeigen, die für das Automation-Konto konfiguriert werden können.

    ![Zugriffsschaltfläche](media/automation-role-based-access-control/automation-01-access-button.png)

>[AZURE.NOTE] **Abonnement-Administratoren** ist als Standardbenutzer bereits vorhanden. Die Active Directory-Gruppe der Abonnementadministratoren umfasst die Dienstadministratoren und Co-Administratoren für Ihr Azure-Abonnement. Der Dienstadministrator ist der Besitzer Ihres Azure-Abonnements und dessen Ressourcen und wird auch für die Automation-Konten die Rolle des Besitzers erben. Dies bedeutet, dass der Zugriff für **Dienstadministratoren und Co-Administratoren** eines Abonnements **geerbt** wird, während er für alle anderen Benutzer **zugewiesen** wird. Klicken Sie auf **Abonnement-Administratoren**, um mehr über deren Rechte zu erfahren.

### Einen neuen Benutzer hinzufügen und eine Rolle zuweisen

1.	Klicken Sie im Benutzerblatt auf **Hinzufügen**, um das Blatt **Zugriff hinzufügen** zu öffnen, auf dem Sie einen Benutzer, eine Gruppe oder eine Anwendung hinzufügen und ihnen Rollen zuweisen können.

    ![Benutzer hinzufügen](media/automation-role-based-access-control/automation-02-add-user.png)

2.	Wählen Sie eine Rolle aus der Liste der verfügbaren Rollen aus. Wir wählen die Rolle **Leser** aus. Sie können aber jede der verfügbaren, integrierten Rollen, die von Automation-Konten unterstützt werden, oder auch eine eigene, benutzerdefinierte Rolle auswählen.

    ![Rolle wählen](media/automation-role-based-access-control/automation-03-select-role.png)

3.	Klicken Sie auf **Benutzer hinzufügen**, um das Blatt **Benutzer hinzufügen** zu öffnen. Falls Sie bereits Benutzer, Gruppen oder Anwendungen hinzugefügt haben, um Ihr Abonnement zu verwalten, werden diese Benutzer aufgelistet, und Sie können sie auswählen, um ihnen den Zugriff zu erteilen. Falls keine Benutzer aufgeführt werden oder falls der Benutzer, den Sie hinzufügen möchten, nicht auf der Liste steht, klicken Sie auf **Einladen**, um das Blatt **Einen Gast einladen** zu öffnen. Von dort können Sie jeden Benutzer mit einer gültigen Microsoft-Konto-E-Mail-Adresse wie Outlook.com, OneDrive oder Xbox Live ID einladen. Wenn Sie die E-Mail-Adresse des Benutzers eingegeben haben, klicken Sie auf **Auswählen**, um den Benutzer hinzuzufügen, und dann auf **OK**.

    ![Hinzufügen von Benutzern](media/automation-role-based-access-control/automation-04-add-users.png)
 
    Sie sollten jetzt den Benutzer auf dem Blatt **Benutzer** mit seiner zugewiesenen Rolle als **Leser** sehen.

    ![Benutzer auflisten](media/automation-role-based-access-control/automation-05-list-users.png)

    Sie können dem Benutzer die Rolle über das Blatt **Rollen** zuweisen.

1. Klicken Sie auf dem Blatt „Benutzer“ auf **Rollen**, um das Blatt **Rollen** zu öffnen. In diesem Blatt können Sie den Namen der Rolle sowie die Anzahl der Benutzer und Gruppen sehen, denen diese Rolle zugewiesen wurde.

    ![Rollen vom Benutzerblatt zuweisen](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)
   
    >[AZURE.NOTE] Die rollenbasierte Zugriffssteuerung kann nur auf der Automation-Konto-Ebene eingerichtete werden und nicht bei einer Ressource unter dem Automation-Konto.

    Sie können einem Benutzer, einer Gruppe oder einer Anwendung mehr als eine Rolle zuweisen. Falls wir dem Benutzer beispielsweise die Rolle **Operator für Automation** zusammen mit der Rolle **Leser** zuweisen, kann er alle Automation-Ressourcen anzeigen und die Runbookaufträge ausführen. Sie können das Dropdown-Menü erweitern, um eine Liste mit allen dem Benutzer zugewiesenen Rollen anzeigen zu lassen.

    ![Mehrere Rollen anzeigen](media/automation-role-based-access-control/automation-07-view-multiple-roles.png)
 
### Benutzer entfernen

Sie können die Zugriffserlaubnisse eines Benutzers, der selbst nicht das Automation-Konto verwaltet oder nicht mehr für die Organisation arbeitet, entfernen. Folgende Schritte müssen Sie durchführen, um einen Benutzer zu entfernen:

1.	Wählen Sie auf dem Blatt **Benutzer** die Rollenzuweisung aus, die Sie entfernen möchten.

2.	Klicken Sie auf dem Blatt mit den Zuweisungsdetails auf die Schaltfläche **Entfernen**.

3.	Klicken Sie auf **Ja**, um die Entfernung zu bestätigen.

    ![Benutzer entfernen](media/automation-role-based-access-control/automation-08-remove-users.png)

## Benutzer mit zugewiesener Rolle

Wenn sich Benutzer, denen eine Rolle zugewiesen ist, bei ihrem Automation-Konto anmelden, wird ihnen das Konto des Besitzers in der Liste mit den **Standardverzeichnissen** angezeigt. Um das Automation-Konto, dem sie hinzugefügt wurden, sehen zu können, müssen sie das Standardverzeichnis auf das Standardverzeichnis des Besitzers ändern.

![Standardverzeichnis](media/automation-role-based-access-control/automation-09-default-directory-in-role-assigned-user.png)

### Die Benutzererfahrung mit der Rolle „Operator für Automation“

Wenn Benutzer, denen die Rolle „Operator für Automation“ zugewiesen wurde, das ihnen zugewiesene Automation-Konto anzeigen, können sie lediglich die Liste mit den Runbooks, Runbookaufgaben und Zeitplänen sehen, die im Automation-Konto erstellt wurden. Dies gilt aber nicht für deren Definition. Sie können die Runbookaufgabe starten, anhalten, unterbrechen, fortführen oder einplanen. Der Benutzer wird auf die anderen Automation-Ressourcen wie Konfigurationen, Hybrid-Worker-Gruppen oder DSC-Knoten nicht zugreifen können.

![Kein Zugriff auf Ressourcen](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)

Wenn der Benutzer das Runbook anklickt, werden ihm die Befehle, die Quelle anzuzeigen oder das Runbook zu bearbeiten, nicht angeboten, da die Rolle „Operator für Automation“ diese Zugriffe nicht erlaubt.

![Kein Änderungszugriff auf Runbook](media/automation-role-based-access-control/automation-11-no-access-to-edit-runbook.png)

Der Benutzer wird einen Lese- und Erstellungszugriff für die Zeitpläne haben, aber keinen Zugriff auf andere Assettypen.

![Kein Zugriff auf Assets](media/automation-role-based-access-control/automation-12-no-access-to-assets.png)

Dieser Benutzer hat auch keine Zugriffsrechte, um die einem Runbook zugeordneten Webhooks zu sehen.

![Kein Zugriff auf Webhooks](media/automation-role-based-access-control/automation-13-no-access-to-webhooks.png)

## Rollenbasierte Zugriffssteuerung für das Automation-Konto mit Azure PowerShell konfigurieren

Rollenbasierter Zugriff kann für ein Automation-Konto auch mit den folgenden [Azure PowerShell-Cmdlets](../active-directory/role-based-access-control-manage-access-powershell.md) konfiguriert werden.

• [Get-AzureRmRoleDefinition](https://msdn.microsoft.com/library/mt603792.aspx) listet alle in Azure Active Directory verfügbaren RBAC-Rollen auf. Sie können diesen Befehl zusammen mit der **Name**-Eigenschaft verwenden, um alle Aktionen aufzulisten, die von einer bestimmten Rolle durchgeführt werden können. **Beispiel:** ![Beziehe Rollendefinition](media/automation-role-based-access-control/automation-14-get-azurerm-role-definition.png)

• [Get-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt619413.aspx) listet die Rollenzuweisungen durch Azure AD RBAC für den angegebenen Bereich auf. Ohne einschränkende Parameter gibt dieser Befehl alle Rollenzuweisungen zurück, die in diesem Abonnement erstellt wurden. Verwenden Sie den **ExpandPrincipalGroups**-Parameter, um die Zugriffszuweisungen für den festgelegten Benutzer sowie die Gruppen, denen der Benutzer angehört, aufzulisten. **Beispiel:** Verwenden Sie den folgenden Befehl, um alle Benutzer innerhalb eines Automation-Kontos mit ihren Rollen aufzulisten.

    Get-AzureRMRoleAssignment -scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>” 

![Beziehe Rollenzuweisung](media/automation-role-based-access-control/automation-15-get-azurerm-role-assignment.png)

• Verwenden Sie [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603580.aspx), um Benutzern, Gruppen und Anwendungen die Zugriffsberechtigung für einen bestimmten Bereich zuzuweisen. **Beispiel:** Verwenden Sie den folgenden Befehl, um die Rolle „Operator für Automation“ für einen Benutzer im Bereich des Automation-Kontos zuzuweisen.

    New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName "Automation operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”  

![Neue Rollenzuweisung](media/automation-role-based-access-control/automation-16-new-azurerm-role-assignment.png)

• Verwenden Sie [Remove-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603781.aspx), um den Zugriff eines angegebenen Benutzers, der Gruppe oder der Anwendung für einen bestimmten Bereich zu entfernen. **Beispiel:** Verwenden Sie den folgenden Befehl, um den Benutzer aus der Rolle „Operator für Automation“ im Bereich des Automation-Kontos zu entfernen.

    Remove-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to remove> -RoleDefinitionName "Automation Operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”

Ersetzen Sie in den obigen Beispielen die **Anmelde-ID**, die **Abonnement-ID**, den **Namen der Ressourcengruppe** und den **Namen des Automation-Kontos** durch die entsprechenden Werte Ihres Kontos. Wählen Sie **Ja**, wenn Sie zum Bestätigen aufgefordert werden, bevor Sie mit dem Entfernen von Benutzerrollenzuweisungen fortfahren.


## Nächste Schritte
-  Weitere Informationen zu den verschiedenen Möglichkeiten, die rollenbasierte Zugriffsteuerung für Azure Automation zu konfigurieren, finden Sie unter [Verwalten der rollenbasierten Zugriffssteuerung mit Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md).
- Weitere Informationen zu verschiedenen Methoden zum Starten eines Runbooks finden Sie unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md).
- Weitere Informationen zu verschiedenen Runbooktypen finden Sie unter [Azure Automation-Runbooktypen](automation-runbook-types.md).

<!---HONumber=AcomDC_0914_2016-->