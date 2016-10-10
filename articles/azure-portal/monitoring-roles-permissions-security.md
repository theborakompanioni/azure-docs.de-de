<properties
	pageTitle="Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor | Microsoft Azure"
	description="Erfahren Sie mehr über die Verwendung der vordefinierten Rollen und Berechtigungen in Azure Monitor, um den Zugriff auf Überwachungsressourcen zu beschränken."
	authors="johnkemnetz"
	manager="rboucher"
	editor=""
	services="monitoring-and-diagnostics"
	documentationCenter="monitoring-and-diagnostics"/>

<tags
	ms.service="monitoring-and-diagnostics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/26/2016"
	ms.author="johnkem"/>

# Erste Schritte mit Rollen, Berechtigungen und Sicherheit in Azure Monitor

Viele Teams müssen den Zugriff auf Überwachungsdaten und -einstellungen streng regulieren. Wenn einige Ihrer Teammitglieder beispielsweise ausschließlich an der Überwachung arbeiten (Supporttechniker, DevOps-Techniker) oder wenn Sie einen verwalteten Dienstanbieter verwenden, sollten Sie diesen nur Zugriff auf Überwachungsdaten erteilen und deren Möglichkeit zum Erstellen, Ändern oder Löschen von Ressourcen einschränken. Dieser Artikel beschreibt, wie Sie schnell eine integrierte RBAC-Rolle zur Überwachung auf einen Benutzer in Azure anwenden oder Ihre eigene benutzerdefinierte Rolle für einen Benutzer erstellen, der eingeschränkte Überwachungsberechtigungen benötigt. Anschließend werden Sicherheitsaspekte für Ihre Azure Monitor-Ressourcen erörtert, und es wird beschrieben, wie Sie den Zugriff auf die darin enthaltenen Daten beschränken können.

## Integrierte Überwachungsrollen

Die in Azure Monitor integrierten Rollen unterstützen Sie dabei, den Zugriff auf Ressourcen in einem Abonnement einzuschränken und gleichzeitig den Personen, die für die Überwachung der Infrastruktur verantwortlich sind, das Abrufen und Konfigurieren der benötigten Daten zu ermöglichen. Azure Monitor bietet zwei vorkonfigurierte Rollen: eine für das Lesen von Überwachungsdaten und eine für Mitwirkende an der Überwachung.

### Überwachungsleser

Personen, denen die Überwachungsleserrolle zugewiesen wird, können alle Überwachungsdaten in einem Abonnement anzeigen, aber keine Ressourcen ändern oder Einstellungen im Zusammenhang mit der Ressourcenüberwachung bearbeiten. Diese Rolle eignet sich für Benutzer in einer Organisation, beispielsweise Support- oder Betriebstechniker, die folgende Aufgaben erfüllen müssen:

- Anzeigen von Überwachungsdashboards im Portal und Erstellen ihrer eigenen privaten Überwachungsdashboards.
- Abfragen von Metriken über die [Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931930.aspx), [PowerShell-Cmdlets](insights-powershell-samples.md) oder die [plattformübergreifende Befehlszeilenschnittstelle](insights-cli-samples.md).
- Abfragen des Aktivitätsprotokolls über das Portal, die Azure Monitor-REST-API, PowerShell-Cmdlets oder die plattformübergreifende Befehlszeilenschnittstelle.
- Anzeigen der [Diagnoseeinstellungen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) für eine Ressource.
- Anzeigen des [Protokollprofils](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) für ein Abonnement.
- Anzeigen von Einstellungen für die automatische Skalierung.
- Anzeigen von Warnaktivitäten und -einstellungen.
- Zugreifen auf Application Insights-Daten und Anzeigen von Daten in der AI-Analyse.
- Durchsuchen von Arbeitsbereichsdaten von Log Analytics (OMS), einschließlich Nutzungsdaten für den Arbeitsbereich.
- Anzeigen von Verwaltungsgruppen in Log Analytics (OMS).
- Abrufen des Suchschemas von Log Analytics (OMS).
- Auflisten von Intelligence Packs zu Log Analytics (OMS).
- Abrufen und Ausführen gespeicherter Suchvorgänge von Log Analytics (OMS).
- Abrufen der Speicherkonfiguration von Log Analytics (OMS).

> [AZURE.NOTE] Diese Rolle erteilt keinen Lesezugriff für Protokolldaten, die an einen Event Hub gestreamt oder in einem Speicherkonto gespeichert wurden. [Unten](#security-considerations-for-monitoring-data) finden Sie Informationen zum Konfigurieren des Zugriffs auf diese Ressourcen.

### Überwachungsmitwirkender

Personen, denen die Rolle für Überwachungsmitwirkende zugewiesen wird, können alle Überwachungsdaten in einem Abonnement anzeigen sowie Überwachungseinstellungen erstellen und ändern, aber keine weiteren Ressourcen ändern. Diese Rolle ist eine Obermenge der Überwachungsleserrolle und eignet sich für Mitglieder des Überwachungsteams einer Organisation oder für Anbieter verwalteter Dienste, die neben den oben genannten Berechtigungen auch folgende Aufgaben ausführen müssen:

- Veröffentlichen von Überwachungsdashboards als freigegebenes Dashboard.
- Festlegen von [Diagnoseeinstellungen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) für eine Ressource.*
- Festlegen des [Protokollprofils](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) für ein Abonnement.*
- Festlegen von Warnaktivitäten und -einstellungen.
- Erstellen von Application Insights-Webtests und -Komponenten.
- Auflisten vorinstallierter Arbeitsbereichsschlüssel aus Log Analytics (OMS).
- Aktivieren oder Deaktivieren von Intelligence Packs zu Log Analytics (OMS).
- Erstellen, Löschen und Ausführen gespeicherter Suchvorgänge von Log Analytics (OMS).
- Erstellen und Löschen der Speicherkonfiguration von Log Analytics (OMS).

*Dem Benutzer muss außerdem gesondert die ListKeys-Berechtigung für die Zielressource (Speicherkonto oder Event Hub-Namespace) erteilt werden, um ein Protokollprofil oder eine Diagnoseeinstellung festzulegen.

> [AZURE.NOTE] Diese Rolle erteilt keinen Lesezugriff für Protokolldaten, die an einen Event Hub gestreamt oder in einem Speicherkonto gespeichert wurden. [Unten](#security-considerations-for-monitoring-data) finden Sie Informationen zum Konfigurieren des Zugriffs auf diese Ressourcen.

## Überwachen von Berechtigungen und benutzerdefinierte RBAC-Rollen
Wenn die oben genannten vordefinierten Rollen nicht den genauen Anforderungen Ihres Teams entsprechen, können Sie eine [benutzerdefinierte RBAC-Rolle](../active-directory/role-based-access-control-custom-roles.md) mit detaillierteren Berechtigungen erstellen. Im Folgenden sind die allgemeinen Azure Monitor-RBAC-Vorgänge mit ihren Beschreibungen aufgeführt.

| Vorgang | Beschreibung |
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Microsoft.Insights/AlertRules/[Read, Write, Delete] | Lesen/Schreiben/Löschen von Warnungsregeln. |
| Microsoft.Insights/AlertRules/Incidents/Read | Auflisten von Incidents (Verlauf der ausgelösten Warnungsregel) für Warnungsregeln. Dies gilt nur für das Portal. |
| Microsoft.Insights/AutoscaleSettings/[Read, Write, Delete] | Lesen/Schreiben/Löschen von Einstellungen für die automatische Skalierung. |
| Microsoft.Insights/DiagnosticSettings/[Read, Write, Delete] | Lesen/Schreiben/Löschen von Diagnoseeinstellungen. |
| Microsoft.Insights/eventtypes/digestevents/Read | Diese Berechtigung ist für Benutzer notwendig, die über das Portal auf Aktivitätsprotokolle zugreifen müssen. |
| Microsoft.Insights/eventtypes/values/Read | Auflisten von Aktivitätsprotokollereignissen (Verwaltungsereignissen) in einem Abonnement. Diese Berechtigung gilt sowohl für den programmgesteuerten als auch für den Portalzugriff auf das Aktivitätsprotokoll. |
| Microsoft.Insights/LogDefinitions/Read | Diese Berechtigung ist für Benutzer notwendig, die über das Portal auf Aktivitätsprotokolle zugreifen müssen. |
| Microsoft.Insights/MetricDefinitions/Read | Lesen von Metrikdefinitionen (Liste der verfügbaren Metriktypen für eine Ressource). |
| Microsoft.Insights/Metrics/Read | Lesen von Metriken für eine Ressource. |

> [AZURE.NOTE] Für den Zugriff auf Warnungen, Diagnoseeinstellungen und Metriken für eine Ressource ist es erforderlich, dass der Benutzer für den Ressourcentyp und den Bereich der jeweiligen Ressource über Lesezugriff verfügt. Für das Erstellen („Schreiben“) einer Diagnoseeinstellung oder eines Protokollprofils, bei der bzw. dem Daten in einem Speicherkonto archiviert oder an Event Hubs gestreamt werden, muss der Benutzer zudem die ListKeys-Berechtigung für die Zielressource besitzen.

Beispielsweise können Sie anhand der oben dargestellten Tabelle wie folgt eine benutzerdefinierte RBAC-Rolle für einen „Aktivitätsprotokollleser“ erstellen:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzureRmRoleDefinition -Role $role 
```

## Sicherheitsaspekte für Überwachungsdaten
Überwachungsdaten – besonders Protokolldateien – können vertrauliche Informationen wie IP-Adressen oder Benutzernamen enthalten. Überwachungsdaten aus Azure werden in drei grundlegenden Varianten bereitgestellt:
1. Das Aktivitätsprotokoll, in dem alle Aktionen auf Steuerungsebene in Ihrem Azure-Abonnement beschrieben werden.
2. Diagnoseprotokolle, die von einer Ressource ausgegeben werden.
3. Metriken, die von Ressourcen ausgegeben werden.

Alle drei Datentypen können in einem Speicherkonto gespeichert oder an Event Hub gestreamt werden. Bei beidem handelt es sich um allgemeine Azure-Ressourcen. Weil es sich um allgemeine Ressourcen handelt, sind das Erstellen, das Löschen und der Zugriff darauf privilegierte Vorgänge, die normalerweise einem Administrator vorbehalten sind. Wir empfehlen, die folgenden Methoden für überwachungsbezogene Ressourcen anzuwenden, um Missbrauch zu verhindern:

- Verwenden Sie ein einzelnes dediziertes Speicherkonto für die Überwachung von Daten. Wenn Sie Überwachungsdaten auf mehrere Speicherkonten aufteilen müssen, nutzen Sie niemals ein Speicherkonto sowohl für Überwachungs- als auch für Nicht-Überwachungsdaten, da Personen, die nur auf Überwachungsdaten zugreifen müssen (z.B. eine Drittanbieter-SIEM-Lösung), dadurch unbeabsichtigterweise Zugriff auf Nicht-Überwachungsdaten erhalten.
- Verwenden Sie aus demselben Grund einen einzelnen dedizierten Service Bus- oder Event Hub-Namespace über alle Diagnoseeinstellungen hinweg.
- Schränken Sie den Zugriff auf überwachungsbezogene Speicherkonten oder Event Hubs ein, indem Sie sie in einer separaten Ressourcengruppe verwalten, und verwenden Sie [Bereiche](../active-directory/role-based-access-control-what-is.md#basics-of-access-management-in-azure) für Ihre Überwachungsrollen, um den Zugriff ausschließlich auf diese Ressourcengruppe zu beschränken.
- Erteilen Sie niemals die ListKeys-Berechtigung für Speicherkonten oder Event Hubs im Abonnementbereich, wenn ein Benutzer nur auf Überwachungsdaten zugreifen muss. Erteilen Sie dem Benutzer diese Berechtigungen stattdessen im Ressourcen- oder Ressourcengruppenbereich (wenn Sie eine dedizierte Überwachungsressourcengruppe verwenden).

### Einschränken des Zugriffs auf überwachungsbezogene Speicherkonten
Wenn ein Benutzer oder eine Anwendung Zugriff auf Überwachungsdaten in einem Speicherkonto benötigt, sollten Sie für das Speicherkonto, das Überwachungsdaten enthält, [eine Konto-SAS](https://msdn.microsoft.com/library/azure/mt584140.aspx) mit Nur-Lese-Zugriff auf Dienstebene für Blobspeicher erstellen. In PowerShell kann dies wie folgt aussehen:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Sie können das Token an die Entität übergeben, die aus dem jeweiligen Speicherkonto lesen muss, damit sie alle Blobs in diesem Speicherkonto auflisten und lesen kann.

Wenn Sie diese Berechtigung mit RBAC steuern müssen, können Sie dieser Entität alternativ die Berechtigung „Microsoft.Storage/storageAccounts/listkeys/action“ für das jeweilige Speicherkonto erteilen. Dies ist für Benutzer notwendig, die eine Diagnoseeinstellung oder ein Protokollprofil für die Archivierung in einem Speicherkonto festlegen müssen. Beispielsweise könnten Sie die folgende benutzerdefinierte RBAC-Rolle für Benutzer oder Anwendungen erstellen, die nur aus einem einzigen Speicherkonto lesen müssen:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzureRmRoleDefinition -Role $role 
```

> [AZURE.WARNING] Die ListKeys-Berechtigung ermöglicht dem Benutzer das Auflisten des primären und des sekundären Speicherkontoschlüssels. Diese Schlüssel gewähren dem Benutzer alle signierten Berechtigungen (Lesen, Schreiben, Erstellen von Blobs, Löschen von Blobs usw.) für alle signierten Dienste (Blob, Warteschlange, Tabelle, Datei) in diesem Speicherkonto. Wir empfehlen nach Möglichkeit die Verwendung einer Konto-SAS, wie oben beschriebenen.

### Einschränken des Zugriffs auf überwachungsbezogene Event Hubs
Für Event Hubs können Sie einem ähnlichen Muster folgen, jedoch müssen Sie zunächst eine dedizierte Autorisierungsregel für Lauschvorgänge erstellen. Wenn Sie einer Anwendung Zugriff erteilen möchten, die nur auf überwachungsbezogene Event Hubs lauschen muss, gehen Sie folgendermaßen vor:

1. Erstellen Sie eine SAS-Richtlinie für die Event Hubs, die für das Streamen von Überwachungsdaten mit ausschließlich Lauschansprüchen erstellt haben. Dies kann im Portal erfolgen. Beispielsweise könnten Sie die Richtlinie „MonitoringReadOnly“ nennen. Wenn möglich, sollten Sie diesen Schlüssel direkt an den Consumer übergeben und den nächsten Schritt überspringen.
2. Wenn der Consumer den Schlüssel ad hoc abrufen können muss, gewähren Sie dem Benutzer die ListKeys-Aktion für diesen Event Hub. Dies ist auch für Benutzer notwendig, die eine Diagnoseeinstellung oder ein Protokollprofil für das Streamen in Event Hubs festlegen müssen. Beispielsweise können Sie eine RBAC-Regel erstellen:

   ```powershell
   $role = Get-AzureRmRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzureRmRoleDefinition -Role $role 
   ```


## Nächste Schritte
- [Weitere Informationen zu RBAC und Berechtigungen in Resource Manager](../active-directory/role-based-access-control-what-is.md)
- [Übersicht über die Überwachung in Microsoft Azure](monitoring-overview.md)

<!---HONumber=AcomDC_0928_2016-->