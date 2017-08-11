---
title: "Log Analytics-Datenmodell für Azure Backup"
description: "In diesem Artikel werden die Details des Log Analytics-Datenmodells für Azure Backup-Daten vorgestellt."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: dfd5c73d-0d34-4d48-959e-1936986f9fc0
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/24/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: 041a8835a1dd185739b23d4073fd5811bb4490b5
ms.contentlocale: de-de
ms.lasthandoff: 07/25/2017

---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Log Analytics-Datenmodell für Azure Backup-Daten
Dieser Artikel beschreibt das Datenmodell, das für die Pushübertragung von Berichtsdaten an Log Analytics verwendet wird. Mit diesem Datenmodell können Sie benutzerdefinierte Abfragen und Dashboards erstellen und in OMS verwenden. 

## <a name="using-azure-backup-data-model"></a>Verwenden des Azure Backup-Datenmodells
Sie können die folgenden im Rahmen des Datenmodells bereitgestellten Felder verwenden, um visuelle Elemente, benutzerdefinierte Abfragen und ein Dashboard gemäß Ihren Anforderungen zu erstellen.

### <a name="alert"></a>Warnung
Diese Tabelle enthält Details zu warnungsbezogenen Feldern.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| AlertUniqueId_s |Text |Eindeutige ID der generierten Warnung |
| AlertType_s |Text |Typ der generierten Warnung, z.B. Sicherung |
| AlertStatus_s |Text |Status der Warnung, z.B. „Aktiv“ |
| AlertOccurenceDateTime_s |Datum/Uhrzeit |Datum und Uhrzeit der Warnungserstellung |
| AlertSeverity_s |Text |Schweregrad der Warnung, z.B. „Kritisch“ |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |
| BackupItemUniqueId_s |Text |Eindeutige ID des Sicherungselements, zu dem diese Warnung gehört |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Warnungsobjekts, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für die Sicherung, zu der diese Warnung gehört, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: Warnung |
| Kategorie |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| ProtectedServerUniqueId_s |Text |Eindeutige ID der geschützten Ressource, zu der diese Warnung gehört |
| VaultUniqueId_s |Text |Eindeutige ID der geschützten Ressource, zu der diese Warnung gehört |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Dieses Feld repräsentiert die ID der Ressource, für die Daten erfasst werden; zeigt die Recovery Services-Tresor-ID an |
| SubscriptionId |Text |Dieses Feld repräsentiert die Abonnement-ID der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceGroup |Text |Dieses Feld repräsentiert die Ressourcengruppe der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceProvider |Text |Dieses Feld repräsentiert den Ressourcenanbieter, für den Daten erfasst werden: Microsoft.RecoveryServices |
| ResourceType |Text |Dieses Feld repräsentiert den Typ der Ressource, für die Daten erfasst werden: Tresore |

### <a name="backupitem"></a>BackupItem
Diese Tabelle enthält Details zu Feldern in Bezug auf Sicherungselemente.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |  
| BackupItemUniqueId_s |Text |Eindeutige ID des Sicherungselements |
| BackupItemId_s |Text |ID des Sicherungselements |
| BackupItemName_s |Text |Name des Sicherungselements |
| BackupItemFriendlyName_s |Text |Anzeigename des Sicherungselements |
| BackupItemType_s |Text |Typ des Sicherungselements, z.B. VM, FileFolder |
| ProtectedServerName_s |Text |Name des geschützten Servers, zu dem das Sicherungselement gehört |
| ProtectionState_s |Text |Aktueller Schutzstatus des Sicherungselements, z.B. „Geschützt“, „Schutz beendet“ |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Sicherungselementobjekts, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für die Sicherung, zu der dieses Sicherungselement gehört, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: BackupItem |
| Kategorie |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Dieses Feld repräsentiert die ID der Ressource, für die Daten erfasst werden; zeigt die Recovery Services-Tresor-ID an |
| SubscriptionId |Text |Dieses Feld repräsentiert die Abonnement-ID der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceGroup |Text |Dieses Feld repräsentiert die Ressourcengruppe der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceProvider |Text |Dieses Feld repräsentiert den Ressourcenanbieter, für den Daten erfasst werden: Microsoft.RecoveryServices |
| ResourceType |Text |Dieses Feld repräsentiert den Typ der Ressource, für die Daten erfasst werden: Tresore |

### <a name="backupitemassociation"></a>BackupItemAssociation
Diese Tabelle enthält Details zur Zuordnung von Sicherungselementen zu verschiedenen Entitäten.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |  
| BackupItemUniqueId_s |Text |Eindeutige ID des Sicherungselements |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Zuordnungsobjekts für das Sicherungselement, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für die Sicherung, zu der dieses Sicherungselement gehört, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: BackupItemAssociation |
| Kategorie |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| PolicyUniqueId_g |Text |Eindeutige ID zur Identifizierung der Richtlinie, der das Sicherungselement zugeordnet ist |
| ProtectedServerUniqueId_s |Text |Eindeutige ID des geschützten Servers, zu dem dieses Sicherungselement gehört |
| VaultUniqueId_s |Text |Eindeutige ID des Tresors, zu dem dieses Sicherungselement gehört |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Dieses Feld repräsentiert die ID der Ressource, für die Daten erfasst werden; zeigt die Recovery Services-Tresor-ID an |
| SubscriptionId |Text |Dieses Feld repräsentiert die Abonnement-ID der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceGroup |Text |Dieses Feld repräsentiert die Ressourcengruppe der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceProvider |Text |Dieses Feld repräsentiert den Ressourcenanbieter, für den Daten erfasst werden: Microsoft.RecoveryServices |
| ResourceType |Text |Dieses Feld repräsentiert den Typ der Ressource, für die Daten erfasst werden: Tresore |

### <a name="job"></a>Auftrag
Diese Tabelle enthält Details zu auftragsbezogenen Feldern.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |
| BackupItemUniqueId_s |Text |Eindeutige ID des Sicherungselements, zu dem dieser Auftrag gehört |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Auftragsobjekts, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für die Sicherung, zu der dieser Auftrag gehört, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: Auftrag |
| Kategorie |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| ProtectedServerUniqueId_s |Text |Eindeutige ID der geschützten Ressource, zu der dieser Auftrag gehört |
| VaultUniqueId_s |Text |Eindeutige ID der geschützten Ressource, zu der dieser Auftrag gehört |
| JobOperation_s |Text |Vorgang, für den Auftrag ausgeführt wird, z.B. Sicherung, Wiederherstellung, Sicherungskonfiguration |
| JobStatus_s |Text |Status des beendeten Auftrags, z.B. „Abgeschlossen“, „Fehler“ |
| JobFailureCode_s |Text |Zeichenfolge mit dem Fehlercode zum Angeben des Grunds des Auftragsfehlers |
| JobStartDateTime_s |Datum/Uhrzeit |Datum und Uhrzeit des Starts des Auftrags |
| BackupStorageDestination_s |Text |Ziel des Sicherungsspeichers, z.B. Cloud, Datenträger  |
| JobDurationInSecs_s | Number |Gesamtdauer des Auftrags in Sekunden |
| DataTransferredInMB_s | Number |Für diesen Auftrag übertragene Daten in MB|
| JobUniqueId_g |Text |Eindeutige ID zur Bezeichnung des Auftrags |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Dieses Feld repräsentiert die ID der Ressource, für die Daten erfasst werden; zeigt die Recovery Services-Tresor-ID an |
| SubscriptionId |Text |Dieses Feld repräsentiert die Abonnement-ID der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceGroup |Text |Dieses Feld repräsentiert die Ressourcengruppe der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceProvider |Text |Dieses Feld repräsentiert den Ressourcenanbieter, für den Daten erfasst werden: Microsoft.RecoveryServices |
| ResourceType |Text |Dieses Feld repräsentiert den Typ der Ressource, für die Daten erfasst werden: Tresore |

### <a name="policy"></a>Richtlinie
Diese Tabelle enthält Details zu richtlinienbezogenen Feldern.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Richtlinienobjekts, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für die Sicherung, zu der diese Richtlinie gehört, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: Richtlinie |
| Kategorie |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| PolicyUniqueId_g |Text |Eindeutige ID zur Bezeichnung der Richtlinie |
| PolicyName_s |Text |Name der definierten Richtlinie |
| BackupFrequency_s |Text |Häufigkeit, mit der Sicherungen erfolgen, z.B. täglich, wöchentlich |
| BackupTimes_s |Text |Datum und Uhrzeit geplanter Sicherungen |
| BackupDaysOfTheWeek_s |Text |Tag der Woche, für den Sicherungen geplant sind |
| RetentionDuration_s |Ganze Zahl |Beibehaltungsdauer für konfigurierte Sicherungen |
| DailyRetentionDuration_s |Ganze Zahl |Gesamte Beibehaltungsdauer in Tagen für konfigurierte Sicherungen |
| DailyRetentionTimes_s |Text |Konfiguration von Datum und Uhrzeit der täglichen Beibehaltung |
| WeeklyRetentionDuration_s |Dezimalzahl |Gesamte wöchentliche Beibehaltungsdauer in Wochen für konfigurierte Sicherungen |
| WeeklyRetentionTimes_s |Text |Konfiguration von Datum und Uhrzeit der wöchentlichen Beibehaltung |
| WeeklyRetentionDaysOfTheWeek_s |Text |Tage der Woche, die für die wöchentliche Beibehaltung ausgewählt sind |
| MonthlyRetentionDuration_s |Dezimalzahl |Gesamte Beibehaltungsdauer in Monaten für konfigurierte Sicherungen |
| MonthlyRetentionTimes_s |Text |Konfiguration von Datum und Uhrzeit der monatlichen Beibehaltung |
| MonthlyRetentionFormat_s |Text |Typ der Konfiguration für die monatliche Beibehaltung, z.B. „Täglich“ für tagesbasiert, „Wöchentlich“ für wochenbasiert |
| MonthlyRetentionDaysOfTheWeek_s |Text |Tage der Woche, die für die monatliche Beibehaltung ausgewählt sind |
| MonthlyRetentionWeeksOfTheMonth_s |Text |Wochen des Monats, in denen die monatliche Beibehaltung konfiguriert ist, z.B. Erste, Letzte usw. |
| YearlyRetentionDuration_s |Dezimalzahl |Gesamte Beibehaltungsdauer in Jahren für konfigurierte Sicherungen |
| YearlyRetentionTimes_s |Text |Konfiguration von Datum und Uhrzeit der jährlichen Beibehaltung |
| YearlyRetentionMonthsOfTheYear_s |Text |Monate des Jahres, die für die jährliche Beibehaltung ausgewählt sind |
| YearlyRetentionFormat_s |Text |Typ der Konfiguration für die jährliche Beibehaltung, z.B. „Täglich“ für tagesbasiert, „Wöchentlich“ für wochenbasiert |
| YearlyRetentionDaysOfTheMonth_s |Text |Datumsangaben des Monats, die für die jährliche Beibehaltung ausgewählt sind |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Dieses Feld repräsentiert die ID der Ressource, für die Daten erfasst werden; zeigt die Recovery Services-Tresor-ID an |
| SubscriptionId |Text |Dieses Feld repräsentiert die Abonnement-ID der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceGroup |Text |Dieses Feld repräsentiert die Ressourcengruppe der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceProvider |Text |Dieses Feld repräsentiert den Ressourcenanbieter, für den Daten erfasst werden: Microsoft.RecoveryServices |
| ResourceType |Text |Dieses Feld repräsentiert den Typ der Ressource, für die Daten erfasst werden: Tresore |

### <a name="policyassociation"></a>PolicyAssociation
Diese Tabelle enthält Details zur Zuordnung von Richtlinien zu verschiedenen Entitäten.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Richtlinienobjekts, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für die Sicherung, zu der diese Richtlinie gehört, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: PolicyAssociation |
| Kategorie |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| PolicyUniqueId_g |Text |Eindeutige ID zur Bezeichnung der Richtlinie |
| VaultUniqueId_s |Text |Eindeutige ID des Tresors, zu dem diese Richtlinie gehört |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Dieses Feld repräsentiert die ID der Ressource, für die Daten erfasst werden; zeigt die Recovery Services-Tresor-ID an |
| SubscriptionId |Text |Dieses Feld repräsentiert die Abonnement-ID der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceGroup |Text |Dieses Feld repräsentiert die Ressourcengruppe der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceProvider |Text |Dieses Feld repräsentiert den Ressourcenanbieter, für den Daten erfasst werden: Microsoft.RecoveryServices |
| ResourceType |Text |Dieses Feld repräsentiert den Typ der Ressource, für die Daten erfasst werden: Tresore |

### <a name="protectedserver"></a>ProtectedServer
Diese Tabelle enthält Details zu Feldern in Bezug auf geschützte Server.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |
| ProtectedServerName_s |Text |Name des geschützten Servers |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Objekts des geschützten Servers, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für die Sicherung, zu der dieser geschützte Server gehört, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: ProtectedServer |
| Kategorie |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| ProtectedServerUniqueId_s |Text |Eindeutige ID des geschützten Servers |
| RegisteredContainerId_s |Text |ID des Containers, der für die Sicherung registriert ist |
| ProtectedServerType_s |Text |Typ des geschützten Servers, der gesichert wird, z.B. Windows |
| ProtectedServerFriendlyName_s |Text |Anzeigename des geschützten Servers |
| AzureBackupAgentVersion_s |Text |Nummer der Backup-Agent-Version |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Dieses Feld repräsentiert die ID der Ressource, für die Daten erfasst werden; zeigt die Recovery Services-Tresor-ID an |
| SubscriptionId |Text |Dieses Feld repräsentiert die Abonnement-ID der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceGroup |Text |Dieses Feld repräsentiert die Ressourcengruppe der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceProvider |Text |Dieses Feld repräsentiert den Ressourcenanbieter, für den Daten erfasst werden: Microsoft.RecoveryServices |
| ResourceType |Text |Dieses Feld repräsentiert den Typ der Ressource, für die Daten erfasst werden: Tresore |

### <a name="protectedserverassociation"></a>ProtectedServerAssociation
Diese Tabelle enthält Details zur Zuordnung von geschützten Servern zu anderen Entitäten.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Objekts der Zuordnung für den geschützten Server, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für die Sicherung, zu der dieser geschützte Server gehört, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: ProtectedServerAssociation |
| Kategorie |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| ProtectedServerUniqueId_s |Text |Eindeutige ID des geschützten Servers |
| VaultUniqueId_s |Text |Eindeutige ID des Tresors, zu dem dieser geschützte Server gehört |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Dieses Feld repräsentiert die ID der Ressource, für die Daten erfasst werden; zeigt die Recovery Services-Tresor-ID an |
| SubscriptionId |Text |Dieses Feld repräsentiert die Abonnement-ID der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceGroup |Text |Dieses Feld repräsentiert die Ressourcengruppe der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceProvider |Text |Dieses Feld repräsentiert den Ressourcenanbieter, für den Daten erfasst werden: Microsoft.RecoveryServices |
| ResourceType |Text |Dieses Feld repräsentiert den Typ der Ressource, für die Daten erfasst werden: Tresore |

### <a name="storage"></a>Speicher
Diese Tabelle enthält Details zu speicherbezogenen Feldern.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| CloudStorageInBytes_s |Dezimalzahl |Von Sicherungen belegter Sicherungsspeicher in der Cloud, berechnet anhand des letzten Werts |
| ProtectedInstances_s |Dezimalzahl |Anzahl der geschützten Instanzen, die zum Berechnen von Front-End-Speicher in der Abrechnung verwendet werden, berechnet anhand des letzten Werts |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Speicherobjekts, z.B. „Aktiv“, „Gelöscht“ |
| BackupManagementType_s |Text |Anbietertyp für die Sicherung, zu der dieser Speicher gehört, z.B. IaaSVM, FileFolder |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: Speicher |
| Kategorie |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| ProtectedServerUniqueId_s |Text |Eindeutige ID des geschützten Servers, für den der Speicher berechnet wird |
| VaultUniqueId_s |Text |Eindeutige ID des Tresors, für den der Speicher berechnet wird |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Dieses Feld repräsentiert die ID der Ressource, für die Daten erfasst werden; zeigt die Recovery Services-Tresor-ID an |
| SubscriptionId |Text |Dieses Feld repräsentiert die Abonnement-ID der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceGroup |Text |Dieses Feld repräsentiert die Ressourcengruppe der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceProvider |Text |Dieses Feld repräsentiert den Ressourcenanbieter, für den Daten erfasst werden: Microsoft.RecoveryServices |
| ResourceType |Text |Dieses Feld repräsentiert den Typ der Ressource, für die Daten erfasst werden: Tresore |

### <a name="vault"></a>Tresor
Diese Tabelle enthält Details zu tresorbezogenen Feldern.

| Feld | Datentyp | Beschreibung |
| --- | --- | --- |
| EventName_s |Text |Dieses Feld stellt den Namen des Ereignisses dar, es lautet immer „AzureBackupCentralReport“ |
| SchemaVersion_s |Text |Dieses Feld gibt die aktuelle Version des Schemas an: **V1** |
| State_s |Text |Aktueller Status des Tresorobjekts, z.B. „Aktiv“, „Gelöscht“ |
| NameVorgang |Text |Dieses Feld repräsentiert den Namen des aktuellen Vorgangs: Tresor |
| Kategorie |Text |Dieses Feld repräsentiert die Kategorie der Diagnosedaten, die an Log Analytics übermittelt werden: AzureBackupReport |
| Ressource |Text |Die Ressource, für die Daten erfasst werden; zeigt den Recovery Services-Tresornamen an |
| VaultUniqueId_s |Text |Eindeutige ID des Tresors |
| VaultName_s |Text |Name des Tresors |
| AzureDataCenter_s |Text |Rechenzentrum, in dem sich der Tresor befindet |
| StorageReplicationType_s |Text |Typ der Speicherreplikation für den Tresor, z.B. GeoRedundant |
| SourceSystem |Text |Quellsystem der aktuellen Daten: Azure |
| ResourceId |Text |Dieses Feld repräsentiert die ID der Ressource, für die Daten erfasst werden; zeigt die Recovery Services-Tresor-ID an |
| SubscriptionId |Text |Dieses Feld repräsentiert die Abonnement-ID der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceGroup |Text |Dieses Feld repräsentiert die Ressourcengruppe der Ressource (RS-Tresor), für die Daten erfasst werden |
| ResourceProvider |Text |Dieses Feld repräsentiert den Ressourcenanbieter, für den Daten erfasst werden: Microsoft.RecoveryServices |
| ResourceType |Text |Dieses Feld repräsentiert den Typ der Ressource, für die Daten erfasst werden: Tresore |

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie das Datenmodell zum Erstellen von Azure Backup-Berichten überprüft haben, können Sie mit dem [Erstellen von Dashboards](../log-analytics/log-analytics-dashboards.md) in Log Analytics und OMS beginnen.

