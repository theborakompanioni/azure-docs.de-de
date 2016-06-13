<properties
	pageTitle="RBAC: integrierte Rollen | Microsoft Azure"
	description="Dieses Thema beschreibt die integrierten Rollen der rollenbasierten Zugriffssteuerung (RBAC)."
	services="active-directory"
	documentationCenter=""
	authors="kgremban"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="identity"
	ms.date="05/20/2016"
	ms.author="kgremban"/>

#RBAC: Integrierte Rollen

Die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) von Azure umfasst die folgenden integrierten Rollen, die Benutzern, Gruppen und Diensten zugewiesen werden können. Die Definitionen integrierter Rollen können nicht geändert werden. Sie können jedoch [benutzerdefinierte Rollen in Azure RBAC](role-based-access-control-custom-roles.md) zur Anpassung an die spezifischen Anforderungen Ihrer Organisation erstellen.

## Rollen in Azure

Die folgende Tabelle enthält kurze Beschreibungen der integrierten Rollen. Klicken Sie auf den Rollennamen, um eine detaillierte Liste der **actions** und **notactions** für die Rolle anzuzeigen. Die **Aktions**-Eigenschaft gibt die zulässigen Aktionen für Azure-Ressourcen an. Für Aktionszeichenfolgen dürfen Platzhalter verwendet werden. Die **notactions**-Eigenschaft gibt die Aktionen an, die von den zulässigen Aktionen ausgeschlossen sind.

>[AZURE.NOTE] Die Definitionen von Azure-Rollen werden ständig weiterentwickelt. Dieser Artikel wird so aktuell wie möglich gehalten. Die aktuellsten Definitionen von Rollen finden Sie jedoch immer in Azure PowerShell. Verwenden Sie je nachdem das Cmdlet `(get-azurermroledefinition "<role name>").actions` oder `(get-azurermroledefinition "<role name>").notactions`.

| Rollenname | Beschreibung |
| --------- | ----------- |
| [Mitwirkender des API-Verwaltungsdienstes](#api-management-service-contributor) | Kann API-Verwaltungsdienste verwalten |
| [Mitwirkender der Application Insights-Komponente](#application-insights-component-contributor) | Kann Application Insights-Komponenten verwalten |
| [Operator für Automation](#automation-operator) | Kann Aufträge starten, unterbrechen und fortsetzen |
| [Mitwirkender von BizTalk](#biztalk-contributor) | Kann BizTalk-Dienste verwalten |
| [Mitwirkender von ClearDB-MySQL-DB](#cleardb-mysql-db-contributor) | Kann ClearDB MySQL-Datenbanken verwalten |
| [Mitwirkender](#contributor) | Kann alles außer den Zugriff verwalten |
| [Mitwirkender von Data Factory](#data-factory-contributor) | Kann Data Factorys verwalten |
| [DevTest Labs-Benutzer](#devtest-labs-user) | Kann alles anzeigen sowie virtuelle Maschinen verbinden, starten, neu starten und herunterfahren |
| [Mitwirkender von DocumentDB-Konto](#documentdb-account-contributor) | Kann DocumentDB-Konten verwalten |
| [Mitwirkender von Intelligent Systems-Konto](#intelligent-systems-account-contributor) | Kann Intelligent Systems-Konten verwalten |
| [Mitwirkender von virtuellem Netzwerk](#network-contributor) | Kann alle Netzwerkressourcen verwalten |
| [Mitwirkender von New Relic APM-Konto](#new-relic-apm-account-contributor) | Kann New Relic Application Performance Management-Konten und -Anwendungen verwalten |
| [Besitzer](#owner) | Kann alles verwalten, einschließlich des Zugriffs |
| [Leser](#reader) | Kann alles anzeigen, jedoch keine Änderungen vornehmen |
| [Mitwirkender von Redis-Cache](#redis-cache-contributor]) | Kann Redis-Caches verwalten |
| [Mitwirkender von Zeitplanungsauftragssammlung](#scheduler-job-collections-contributor) | Kann Zeitplanungsauftragssammlungen verwalten |
| [Mitwirkender von Suchdienst](#search-service-contributor) | Kann Suchdienste verwalten |
| [Sicherheits-Manager](#security-manager) | Kann Sicherheitskomponenten, Sicherheitsrichtlinien und virtuelle Maschinen verwalten |
| [Mitwirkender von SQL DB](#sql-db-contributor) | Kann SQL-Datenbanken verwalten, jedoch nicht die zugehörigen sicherheitsbezogenen Richtlinien |
| [SQL-Sicherheits-Manager](#sql-security-manager) | Kann die sicherheitsbezogenen Richtlinien von SQL-Servern und SQL-Datenbanken verwalten |
| [Mitwirkender von SQL Server](#sql-server-contributor) | Kann SQL-Server und SQL-Datenbanken verwalten, jedoch nicht die zugehörigen sicherheitsbezogenen Richtlinien |
| [Mitwirkender von klassischem Speicherkonto](#classic-storage-account-contributor) | Kann klassische Speicherkonten verwalten |
| [Mitwirkender von Speicherkonto](#storage-account-contributor) | Kann Speicherkonten verwalten |
| [Benutzerzugriffsadministrator](#user-access-administrator) | Kann den Benutzerzugriff auf Azure-Ressourcen verwalten |
| [Mitwirkender von klassischen virtuellen Computern](#classic-virtual-machine-contributor) | Kann klassische virtuelle Computer verwalten, jedoch nicht das virtuelle Netzwerk oder Speicherkonto, mit dem sie verbunden sind |
| [Mitwirkender von virtuellen Computern](#virtual-machine-contributor) | Kann virtuelle Computer verwalten, jedoch nicht das virtuelle Netzwerk oder Speicherkonto, mit dem sie verbunden sind |
| [Mitwirkender von klassischem Netzwerk](#classic-network-contributor) | Kann klassische virtuelle Netzwerke und reservierte IP-Adressen verwalten |
| [Mitwirkender von Webplan](#web-plan-contributor) | Kann Webpläne verwalten |
| [Mitwirkender von Website](#website-contributor) | Kann Websites verwalten, jedoch nicht die Webpläne, mit denen sie verbunden sind |

## Rollenberechtigungen
Die folgenden Tabellen beschreiben die spezifischen Berechtigungen der einzelnen Rollen. Dazu gehören **actions**, die Berechtigungen erteilen, und **notactions**, die sie einschränken.

### Mitwirkender des API-Verwaltungsdienstes
Kann API-Verwaltungsdienste verwalten

| **Aktionen** | |
| ------- | ------ |
| Microsoft.ApiManagement/Service/* | Erstellen und Verwalten von API-Verwaltungsdiensten |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Mitwirkender der Application Insights-Komponente
Kann Application Insights-Komponenten verwalten

| **Aktionen** | |
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.Insights/components/* | Erstellen und Verwalten von Insights-Komponenten |
| Microsoft.Insights/webtests/* | Erstellen und Verwalten von Webtests |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Operator für Automation
Kann Aufträge starten, unterbrechen und fortsetzen

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Automation/automationAccounts/jobs/read | Lesen von Aufträgen für Automation-Konten |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Fortsetzen eines Auftrags für Automation-Konten |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Beenden eines Auftrags für Automation-Konten |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Lesen von Auftragsdatenströmen für Automation-Konten |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Unterbrechen eines Auftrags für Automation-Konten |
| Microsoft.Automation/automationAccounts/jobs/write | Schreiben von Aufträgen für Automation-Konten |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Lesen eines Auftragszeitplans für Automation-Konten |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Lesen eines Auftragszeitplans für Automation-Konten |
| Microsoft.Automation/automationAccounts/read | Lesen von Automation-Konten |
| Microsoft.Automation/automationAccounts/runbooks/read | Lesen von Automation-Runbooks |
| Microsoft.Automation/automationAccounts/schedules/read | Lesen von Zeitplänen für Automation-Konten |
| Microsoft.Automation/automationAccounts/schedules/write | Schreiben von Zeitplänen für Automation-Konten |
| Microsoft.Insights/components/* | Erstellen und Verwalten von Insights-Komponenten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Mitwirkender von BizTalk
Kann BizTalk-Dienste verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.BizTalkServices/BizTalk/* | Erstellen und Verwalten von BizTalk-Diensten |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Mitwirkender von ClearDB-MySQL-DB
Kann ClearDB MySQL-Datenbanken verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
| successbricks.cleardb/databases/* | Erstellen und Verwalten von ClearDB MySQL-Datenbanken |

### Mitwirkender
Kann alles außer den Zugriff verwalten

| **Aktionen** ||
| ------- | ------ |
| * | Erstellen und Verwalten von Ressourcen aller Typen |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Authorization/*/Write | Rollen und Rollenzuweisungen können nicht erstellt werden. |
| Microsoft.Authorization/*/Delete | Rollen und Rollenzuweisungen können nicht gelöscht werden. |

### Mitwirkender von Data Factory
Kann Data Factorys verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.DataFactory/dataFactories/* | Verwalten von Data Factorys |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### DevTest Labs-Benutzer
Kann alles anzeigen sowie virtuelle Maschinen verbinden, starten, neu starten und herunterfahren

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Compute/availabilitySets/read | Lesen der Eigenschaften von Verfügbarkeitsgruppen |
| Microsoft.Compute/virtualMachines/*/read | Lesen der Eigenschaften eines virtuellen Computers (VM-Größen, Laufzeitstatus, VM-Erweiterungen usw.) |
| Microsoft.Compute/virtualMachines/deallocate/action | Aufhebung der Zuordnung virtueller Computer |
| Microsoft.Compute/virtualMachines/read | Lesen der Eigenschaften eines virtuellen Computers |
| Microsoft.Compute/virtualMachines/restart/action | Neustarten virtueller Computer |
| Microsoft.Compute/virtualMachines/start/action | Starten virtueller Computer |
| Microsoft.DevTestLab/*/read | Lesen der Eigenschaften eines Labs |
| Microsoft.DevTestLab/labs/createEnvironment/action | Erstellen einer Lab-Umgebung |
| Microsoft.DevTestLab/labs/formulas/delete | Löschen von Formeln |
| Microsoft.DevTestLab/labs/formulas/read | Lesen von Formeln |
| Microsoft.DevTestLab/labs/formulas/write | Hinzufügen oder Ändern von Formeln |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Auswerten von Labrichtlinien |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Verknüpfen eines Back-End-Adresspools für Load Balancer |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Verknüpfen einer eingehenden NAT-Regel für Load Balancer |
| Microsoft.Network/networkInterfaces/*/read | Lesen der Eigenschaften einer Netzwerkschnittstelle (z. B. alle Load Balancer, zu denen die Netzwerkschnittstelle gehört) |
| Microsoft.Network/networkInterfaces/join/action | Verknüpfen eines virtuellen Computers mit einer Netzwerkschnittstelle |
| Microsoft.Network/networkInterfaces/read | Lesen von Netzwerkschnittstellen |
| Microsoft.Network/networkInterfaces/write | Schreiben in Netzwerkschnittstellen |
| Microsoft.Network/publicIPAddresses/*/read | Lesen der Eigenschaften einer öffentlichen IP-Adresse |
| Microsoft.Network/publicIPAddresses/join/action | Verknüpfen einer öffentlichen IP-Adresse |
| Microsoft.Network/publicIPAddresses/read | Lesen von öffentlichen IP-Adressen im Netzwerk |
| Microsoft.Network/virtualNetworks/subnets/join/action | Verknüpfen eines virtuellen Netzwerks |
| Microsoft.Resources/deployments/operations/read | Lesen von Bereitstellungsvorgängen |
| Microsoft.Resources/deployments/read | Lesen von Bereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Storage/storageAccounts/listKeys/action | Auflisten von Speicherkontoschlüsseln |

### Mitwirkender von DocumentDB-Konto
Kann DocumentDB-Konten verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.DocumentDb/databaseAccounts/* | Erstellen und Verwalten von DocumentDB-Konten |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Mitwirkender von Intelligent Systems-Konto
Kann Intelligent Systems-Konten verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.IntelligentSystems/accounts/* | Erstellen und Verwalten von Intelligent Systems-Konten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Mitwirkender von virtuellem Netzwerk
Kann alle Netzwerkressourcen verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.Network/* | Erstellen und Verwalten von Netzwerken |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Mitwirkender von New Relic APM-Konto
Kann New Relic Application Performance Management-Konten und -Anwendungen verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
| NewRelic.APM/accounts/* | Erstellen und Verwalten von New Relic Application Performance Management-Konten |

### Besitzer
Kann alles verwalten, einschließlich des Zugriffs

| **Aktionen** ||
| ------- | ------ |
| * | Erstellen und Verwalten von Ressourcen aller Typen |

### Leser
Kann alles anzeigen, jedoch keine Änderungen vornehmen

| **Aktionen** ||
| ------- | ------ |
| **/read | Lesen von Ressourcen aller Typen mit Ausnahme geheimer Schlüssel |

### Mitwirkender von Redis-Cache
Kann Redis-Caches verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Cache/redis/* | Erstellen und Verwalten von Redis-Caches |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Mitwirkender von Zeitplanungsauftragssammlung
Kann Zeitplanungsauftragssammlungen verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen | Microsoft.Scheduler/jobcollections/* | Erstellen und Verwalten von Auftragssammlungen |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Mitwirkender von Suchdienst
Kann Suchdienste verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen | Microsoft.Search/searchServices/* | Erstellen und Verwalten von Suchdiensten |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Sicherheits-Manager
Kann Sicherheitskomponenten, Sicherheitsrichtlinien und virtuelle Computer verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.ClassicCompute/*/read | Lesen von Konfigurationsinformationen zu klassischen virtuellen Compute-Computern |
| Microsoft.ClassicCompute/virtualMachines/*/write | Schreiben der Konfiguration für virtuelle Computer |
| Microsoft.ClassicNetwork/*/read | Lesen von Konfigurationsinformationen zu klassischem Netzwerk |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen | Microsoft.Security/* | Erstellen und Verwalten von Sicherheitskomponenten und -richtlinien |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Mitwirkender von SQL DB
Kann SQL-Datenbanken verwalten, jedoch nicht die zugehörigen sicherheitsbezogenen Richtlinien

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Rollen und Rollenzuweisungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen | Microsoft.Sql/servers/databases/* | Erstellen und Verwalten von SQL-Datenbanken |
| Microsoft.Sql/servers/read | Lesen von SQL Server |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Kann keine Überwachungsrichtlinien bearbeiten |
| Microsoft.Sql/servers/databases/auditingSettings/* | Überwachungsrichtlinien können nicht bearbeiten werden |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Kann keine Verbindungsrichtlinien bearbeiten |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Kann keine Datenmaskierungsrichtlinien bearbeiten |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Richtlinien für Sicherheitswarnungen können nicht bearbeitet werden |
| Microsoft.Sql/servers/databases/securityMetrics/* | Kann keine Sicherheitsmetriken bearbeiten |

### SQL-Sicherheits-Manager
Kann die sicherheitsbezogenen Richtlinien von SQL-Servern und SQL-Datenbanken verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen der Microsoft-Autorisierung |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen | Microsoft.Sql/servers/auditingPolicies/* | Erstellen und Verwalten von SQL Server-Überwachungsrichtlinien |
| Microsoft.Sql/servers/auditingSettings/* | Erstellen und Verwalten von SQL Server-Überwachungseinstellungen |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Erstellen und Verwalten von Überwachungsrichtlinien von SQL Server-Datenbanken |
| Microsoft.Sql/servers/databases/auditingSettings/* | Erstellen und Verwalten von Überwachungseinstellungen von SQL Server-Datenbanken |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Erstellen und Verwalten von Verbindungsrichtlinien von SQL Server-Datenbanken |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Erstellen und Verwalten von Datenmaskierungsrichtlinien von SQL Server-Datenbanken |
| Microsoft.Sql/servers/databases/read | Lesen von SQL-Datenbanken |
| Microsoft.Sql/servers/databases/schemas/read | Lesen von Datenbankschemas von SQL-Servern |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Lesen von Spalten der Datenbanktabellen von SQL-Servern |
| Microsoft.Sql/servers/databases/schemas/tables/read | Lesen von Datenbanktabellen von SQL-Servern |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Erstellen und Verwalten von Richtlinien für Sicherheitswarnungen von SQL Server-Datenbanken |
| Microsoft.Sql/servers/databases/securityMetrics/* | Erstellen und Verwalten von Sicherheitsmetriken von SQL Server-Datenbanken |
| Microsoft.Sql/servers/read | Lesen von SQL Server |
| Microsoft.Sql/servers/securityAlertPolicies/* | Erstellen und Verwalten von Richtlinien für Sicherheitswarnungen von SQL Server |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Mitwirkender von SQL Server
Kann SQL-Server und SQL-Datenbanken verwalten, jedoch nicht die zugehörigen sicherheitsbezogenen Richtlinien

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen|
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen | Microsoft.Sql/servers/* | Erstellen und Verwalten von SQL-Servern |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/auditingPolicies/* | Kann keine SQL Server-Überwachungsrichtlinien bearbeiten |
| Microsoft.Sql/servers/auditingSettings/* | SQL Server-Überwachungseinstellungen können nicht bearbeitet werden |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Kann keine Überwachungsrichtlinien von SQL Server-Datenbanken bearbeiten |
| Microsoft.Sql/servers/databases/auditingSettings/* | Überwachungseinstellungen von SQL Server-Datenbanken können nicht bearbeitet werden |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Kann keine Verbindungsrichtlinien von SQL Server-Datenbanken bearbeiten |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Kann keine Datenmaskierungsrichtlinien von SQL Server-Datenbanken bearbeiten |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Richtlinien für Sicherheitswarnungen von SQL Server-Datenbanken können nicht bearbeitet werden |
| Microsoft.Sql/servers/databases/securityMetrics/* | Kann keine Sicherheitsmetriken von SQL Server-Datenbanken bearbeiten |
| Microsoft.Sql/servers/securityAlertPolicies/* | Richtlinien für Sicherheitswarnungen von SQL Server können nicht bearbeitet werden |

### Mitwirkender von klassischem Speicherkonto
Kann klassische Speicherkonten verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.ClassicStorage/storageAccounts/* | Erstellen und Verwalten von Speicherkonten |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Mitwirkender von Speicherkonto
Ermöglicht die Verwaltung von Speicherkonten, aber nicht den Zugriff darauf.

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen aller Autorisierungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Insights/diagnosticSettings/* | Verwalten der Diagnoseeinstellungen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen | Microsoft.Storage/storageAccounts/* | Erstellen und Verwalten von Speicherkonten |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Benutzerzugriffsadministrator
Kann den Benutzerzugriff auf Azure-Ressourcen verwalten

| **Aktionen** ||
| ------- | ------ |
| */read | Lesen von Ressourcen aller Typen, mit Ausnahme geheimer Schlüssel | 
| Microsoft.Authorization/* | Verwalten der Autorisierung |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Mitwirkender von klassischen virtuellen Computern
Kann klassische virtuelle Computer verwalten, jedoch nicht das virtuelle Netzwerk oder Speicherkonto, mit dem sie verbunden sind

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.ClassicCompute/domainNames/* | Erstellen und Verwalten von klassischen Compute-Domänennamen |
| Microsoft.ClassicCompute/virtualMachines/* | Erstellen und Verwalten von virtuellen Computern |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | Verknüpfen von Netzwerksicherheitsgruppen |
| Microsoft.ClassicNetwork/reservedIps/link/action | Verknüpfen von reservierten IP-Adressen |
| Microsoft.ClassicNetwork/reservedIps/read | Lesen von reservierten IP-Adressen |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Verknüpfen von virtuellen Netzwerken |
| Microsoft.ClassicNetwork/virtualNetworks/read | Lesen von virtuellen Netzwerken |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Lesen von Speicherkontodatenträgern |
| Microsoft.ClassicStorage/storageAccounts/images/read | Lesen von Speicherkontoimages |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Auflisten von Speicherkontoschlüsseln |
| Microsoft.ClassicStorage/storageAccounts/read | Lesen von klassischen Speicherkonten |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Mitwirkender von virtuellen Computern
Kann virtuelle Computer verwalten, jedoch nicht das virtuelle Netzwerk oder Speicherkonto, mit dem sie verbunden sind

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.Compute/availabilitySets/* | Erstellen und Verwalten von Compute-Verfügbarkeitsgruppen |
| Microsoft.Compute/locations/* | Erstellen und Verwalten von Compute-Speicherorten |
| Microsoft.Compute/virtualMachines/* | Erstellen und Verwalten von virtuellen Computern |
| Microsoft.Compute/virtualMachineScaleSets | Erstellen und Verwalten von Skalierungsgruppen für virtuelle Computer |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Verknüpfen von Back-End-Adresspools für Netzwerkanwendungsgateway |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Verknüpfen von Back-End-Adresspools für Load Balancer |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Verknüpfen von eingehenden NAT-Pools für Load Balancer |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Verknüpfen von eingehenden NAT-Regeln für Load Balancer |
| Microsoft.Network/loadBalancers/read | Lesen von Load Balancern |
| Microsoft.Network/locations/* | Erstellen und Verwalten von Netzwerkspeicherorten |
| Microsoft.Network/networkInterfaces/* | Erstellen und Verwalten von Netzwerkschnittstellen |
| Microsoft.Network/networkSecurityGroups/join/action | Verknüpfen von Netzwerksicherheitsgruppen |
| Microsoft.Network/networkSecurityGroups/read | Lesen von Netzwerksicherheitsgruppen |
| Microsoft.Network/publicIPAddresses/join/action | Verknüpfen von öffentlichen IP-Adressen im Netzwerk |
| Microsoft.Network/publicIPAddresses/read | Lesen von öffentlichen IP-Adressen im Netzwerk |
| Microsoft.Network/virtualNetworks/read | Lesen von virtuellen Netzwerken |
| Microsoft.Network/virtualNetworks/subnets/join/action | Verknüpfen von Subnetzen in virtuellen Netzwerken |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen | Microsoft.Storage/storageAccounts/listKeys/action | Auflisten von Speicherkontoschlüsseln |
| Microsoft.Storage/storageAccounts/read | Lesen von Speicherkonten |
| Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Mitwirkender von klassischem Netzwerk
Kann klassische virtuelle Netzwerke und reservierte IP-Adressen verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.ClassicNetwork/* | Erstellen und Verwalten von klassischen Netzwerken |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |

### Mitwirkender von Webplan
Kann Webpläne verwalten

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
| Microsoft.Web/serverFarms/* | Erstellen und Verwalten von Serverfarmen |

### Mitwirkender von Website
Kann Websites verwalten, jedoch nicht die Webpläne, mit denen sie verbunden sind

| **Aktionen** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lesen von Autorisierungen |
| Microsoft.Insights/alertRules/* | Erstellen und Verwalten von Insights-Warnungsregeln |
| Microsoft.Insights/components/* | Erstellen und Verwalten von Insights-Komponenten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lesen des Status der Ressourcen |
| Microsoft.Resources/deployments/* | Erstellen und Verwalten von Ressourcengruppenbereitstellungen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lesen von Ressourcengruppen | Microsoft.Support/* | Erstellen und Verwalten von Support-Tickets |
| Microsoft.Web/certificates/* | Erstellen und Verwalten von Websitezertifikaten |
| Microsoft.Web/listSitesAssignedToHostName/read | Lesen von Websites, die einem Hostnamen zugewiesen sind |
| Microsoft.Web/serverFarms/join/action | Verknüpfen von Serverfarmen |
| Microsoft.Web/serverFarms/read | Lesen von Serverfarmen |
| Microsoft.Web/sites/* | Erstellen und Verwalten von Websites |

## Weitere Informationen
- [Rollenbasierte Zugriffssteuerung:](role-based-access-control-configure.md) Erste Schritte mit RBAC im Azure-Portal.
- [Benutzerdefinierte Rollen in Azure RBAC:](role-based-access-control-custom-roles.md) Erfahren Sie, wie Sie benutzerdefinierte Rollen entsprechend Ihren Zugriffsanforderungen erstellen.
- [Erstellen eines Verlaufsberichts zu Zugriffsänderungen:](role-based-access-control-access-change-history-report.md) Verfolgen Sie das Ändern von Rollenzuweisungen in RBAC nach.
- [Problembehandlung bei rollenbasierter Zugriffssteuerung:](role-based-access-control-troubleshooting.md) Sehen Sie sich Vorschläge zur Behebung häufig auftretender Probleme an.

<!---HONumber=AcomDC_0601_2016-->