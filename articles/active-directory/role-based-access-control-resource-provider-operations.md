---
title: "Vorgänge für Azure Resource Manager-Anbieter | Microsoft-Dokumentation"
description: "Hier finden Sie ausführliche Informationen zu den verfügbaren Vorgängen für Microsoft Azure Resource Manager-Ressourcenanbieter."
services: active-directory
documentationcenter: 
author: jboeshart
manager: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: jaboes
ms.translationtype: Human Translation
ms.sourcegitcommit: 7f8b63c22a3f5a6916264acd22a80649ac7cd12f
ms.openlocfilehash: 3d32ded77382fe852682d63c96a66aacd141fdf6
ms.contentlocale: de-de
ms.lasthandoff: 05/01/2017


---
# <a name="azure-resource-manager-resource-provider-operations"></a>Vorgänge für Azure Resource Manager-Ressourcenanbieter

In diesem Dokument werden die Vorgänge aufgeführt, die jedem Microsoft Azure Resource Manager-Ressourcenanbieter zur Verfügung stehen. Sie können in benutzerdefinierten Rollen verwendet werden, um präzise RBAC-Berechtigungen (Role-Based Access Control; rollenbasierte Zugriffssteuerung) für Ressourcen in Azure bereitzustellen. Hinweis: Hierbei handelt es sich nicht um eine umfassende Liste, und bei Anbieterupdates werden unter Umständen Vorgänge hinzugefügt oder entfernt. Vorgangszeichenfolgen weisen das Format `Microsoft.<ProviderName>/<ChildResourceType>/<action>` auf. Verwenden Sie zum Anzeigen einer umfassenden und aktuellen Liste entweder `Get-AzureRmProviderOperation` (PowerShell) oder `azure provider operations show` (Azure-Befehlszeilenschnittstelle), um Vorgänge von Azure-Ressourcenanbietern aufzulisten.

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

| Vorgang | Beschreibung |
|---|---|
|/configuration/action|Aktualisiert die Mandantenkonfiguration.|
|/services/action|Aktualisiert eine Dienstinstanz im Mandanten.|
|/configuration/write|Erstellt eine Mandantenkonfiguration.|
|/configuration/read|Liest die Mandantenkonfiguration.|
|/services/write|Erstellt eine Dienstinstanz im Mandanten.|
|/services/read|Liest die Dienstinstanz im Mandanten.|
|/services/delete|Löscht eine Dienstinstanz im Mandanten.|
|/services/servicemembers/action|Erstellt eine Dienstmemberinstanz im Dienst.|
|/services/servicemembers/read|Liest die Dienstmemberinstanz im Dienst.|
|/services/servicemembers/delete|Löscht eine Dienstmemberinstanz im Dienst.|
|/services/servicemembers/alerts/read|Liest die Warnungen für einen Dienstmember.|
|/services/alerts/read|Liest die Warnungen für einen Dienst.|
|/services/alerts/read|Liest die Warnungen für einen Dienst.|

## <a name="microsoftadvisor"></a>Microsoft.Advisor

| Vorgang | Beschreibung |
|---|---|
|/generateRecommendations/action|Generiert Empfehlungen.|
|/suppressions/action|Erstellt/Aktualisiert Unterdrückungen.|
|/register/action|Registriert das Abonnement für den Microsoft-Ratgeber.|
|/generateRecommendations/read|Ruft den Status der Empfehlungsgenerierung ab.|
|/recommendations/read|Liest Empfehlungen.|
|/suppressions/read|Ruft Unterdrückungen ab.|
|/suppressions/delete|Löscht Unterdrückungen.|

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

| Vorgang | Beschreibung |
|---|---|
|/servers/read|Ruft die Informationen des angegebenen Analysis-Servers ab.|
|/servers/write|Erstellt oder aktualisiert den angegebenen Analysis-Server.|
|/servers/delete|Löscht den Analysis-Server.|
|/servers/suspend/action|Hält den Analysis-Server an.|
|/servers/resume/action|Setzt den Analysis-Server fort.|
|/servers/checkNameAvailability<br>/action|Prüft, ob der angegebene Analysis-Servername gültig ist oder bereits verwendet wird.|

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

| Vorgang | Beschreibung |
|---|---|
|/checkNameAvailability/action|Prüft, ob der angegebene Dienstname verfügbar ist.|
|/register/action|Dient zum Registrieren des Microsoft.ApiManagement-Ressourcenanbieters.|
|/unregister/action|Dient zum Aufheben der Registrierung des Microsoft.ApiManagement-Ressourcenanbieters.|
|/service/write|Dient zum Erstellen einer neuen Instanz des API Management-Diensts.|
|/service/read|Dient zum Lesen der Metadaten für eine API Management-Dienstinstanz.|
|/service/delete|Dient zum Löschen einer API Management-Dienstinstanz.|
|/service/updatehostname/action|Dient zum Einrichten, Aktualisieren oder Entfernen benutzerdefinierter Domänennamen für einen API Management-Dienst.|
|/service/uploadcertificate/action|Dient zum Hochladen eines SSL-Zertifikats für einen API Management-Dienst.|
|/service/backup/action|Dient zum Sichern des API Management-Diensts im angegebenen Container in einem vom Benutzer bereitgestellten Speicherkonto.|
|/service/restore/action|Dient zum Wiederherstellen des API Management-Diensts aus dem angegebenen Container in einem vom Benutzer bereitgestellten Speicherkonto.|
|/service/managedeployments/action|Dient zum Ändern der SKU/Einheiten sowie zum Hinzufügen/Entfernen regionaler Bereitstellungen des API Management-Diensts.|
|/service/getssotoken/action|Ruft ein SSO-Token ab, das verwendet werden kann, um sich als Administrator beim Legacyportal des API Management-Diensts anzumelden.|
|/service/applynetworkconfigurationupdates/action|Aktualisiert die in Virtual Network ausgeführten Microsoft.ApiManagement-Ressourcen, um aktualisierte Netzwerkeinstellungen zu verwenden.|
|/service/operationresults/read|Ruft den aktuellen Status eines Vorgangs mit langer Ausführungsdauer ab.|
|/service/networkStatus/read|Ruft die Netzwerkzugriffsstatus von Ressourcen ab.|
|/service/loggers/read|Dient zum Abrufen einer Liste mit Protokollierungen oder zum Abrufen von Protokollierungsdetails.|
|/service/loggers/write|Dient zum Hinzufügen einer neuen Protokollierung oder zum Aktualisieren der Details einer bereits vorhandenen Protokollierung.|
|/service/loggers/delete|Dient zum Entfernen vorhandener Protokollierungen.|
|/service/users/read|Dient zum Abrufen einer Liste mit registrierten Benutzern oder zum Abrufen der Kontodetails eines Benutzers.|
|/service/users/write|Dient zum Registrieren eines neuen Benutzers oder zum Aktualisieren der Kontodetails eines vorhandenen Benutzers.|
|/service/users/delete|Dient zum Entfernen von Benutzerkonten.|
|/service/users/generateSsoUrl/action|Dient zum Generieren der SSO-URL. Die URL kann für den Zugriff auf das Verwaltungsportal verwendet werden.|
|/service/users/subscriptions/read|Dient zum Abrufen einer Liste mit Benutzerabonnements.|
|/service/users/keys/read|Dient zum Abrufen einer Liste mit Benutzerschlüsseln.|
|/service/users/groups/read|Dient zum Abrufen einer Liste mit Benutzergruppen.|
|/service/tenant/operationResults/read|Dient zum Abrufen einer Liste mit Vorgangsergebnissen oder zum Abrufen des Ergebnisses eines bestimmten Vorgangs.|
|/service/tenant/policy/read|Dient zum Abrufen der Richtlinienkonfiguration für den Mandanten.|
|/service/tenant/policy/write|Dient zum Festlegen der Richtlinienkonfiguration für den Mandanten.|
|/service/tenant/policy/delete|Dient zum Entfernen der Richtlinienkonfiguration für den Mandanten.|
|/service/tenant/configuration/save/action|Erstellt Commit mit Konfigurationsmomentaufnahme in der angegebenen Verzweigung im Repository.|
|/service/tenant/configuration/deploy/action|Führt eine Bereitstellungsaufgabe aus, um Änderungen aus der angegebenen Git-Verzweigung auf die Konfiguration in der Datenbank anzuwenden.|
|/service/tenant/configuration/validate/action|Überprüft Änderungen aus der angegebenen Git-Verzweigung.|
|/service/tenant/configuration/operationResults/read|Dient zum Abrufen einer Liste mit Vorgangsergebnissen oder zum Abrufen des Ergebnisses eines bestimmten Vorgangs.|
|/service/tenant/configuration/syncState/read|Dient zum Abrufen des Status der letzten Git-Synchronisierung.|
|/service/tenant/access/read|Dient zum Abrufen der Zugriffsinformationsdetails für den Mandanten.|
|/service/tenant/access/write|Dient zum Aktualisieren der Zugriffsinformationsdetails für den Mandanten.|
|/service/tenant/access/regeneratePrimaryKey/action|Dient zum erneuten Generieren des primären Zugriffsschlüssels.|
|/service/tenant/access/regenerateSecondaryKey/action|Dient zum erneuten Generieren des sekundären Zugriffsschlüssels.|
|/service/identityProviders/read|Dient zum Abrufen der Identitätsanbieterliste oder zum Abrufen von Details zum Identitätsanbieter.|
|/service/identityProviders/write|Dient zum Erstellen eines neuen Identitätsanbieters oder zum Aktualisieren der Details eines bereits vorhandenen Identitätsanbieters.|
|/service/identityProviders/delete|Dient zum Entfernen bereits vorhandener Identitätsanbieter.|
|/service/subscriptions/read|Dient zum Abrufen einer Liste mit Produktabonnements oder zum Abrufen von Produktabonnementdetails.|
|/service/subscriptions/write|Dient dazu, für einen vorhandenen Benutzer ein Abonnement für ein vorhandenes Produkt zu erstellen oder die Details eines vorhandenen Abonnements zu aktualisieren. Dieser Vorgang kann zum Verlängern von Abonnements verwendet werden.|
|/service/subscriptions/delete|Dient zum Löschen von Abonnements. Dieser Vorgang kann zum Löschen von Abonnements verwendet werden.|
|/service/subscriptions/regeneratePrimaryKey/action|Dient zum erneuten Generieren des primären Abonnementschlüssels.|
|/service/subscriptions/regenerateSecondaryKey/action|Dient zum erneuten Generieren des sekundären Abonnementschlüssels.|
|/service/backends/read|Dient zum Abrufen einer Liste mit Back-Ends oder zum Abrufen von Back-End-Details.|
|/service/backends/write|Dient zum Hinzufügen eines neuen Back-Ends oder zum Aktualisieren bereits vorhandener Back-End-Details.|
|/service/backends/delete|Dient zum Entfernen vorhandener Back-Ends.|
|/service/apis/read|Dient zum Abrufen einer Liste mit allen registrierten APIs oder zum Abrufen von API-Details.|
|/service/apis/write|Dient zum Erstellen einer neuen API oder zum Aktualisieren bereits vorhandener API-Details.|
|/service/apis/delete|Dient zum Entfernen vorhandener APIs.|
|/service/apis/policy/read|Dient zum Abrufen von Richtlinienkonfigurationsdetails für APIs.|
|/service/apis/policy/write|Dient zum Festlegen von Richtlinienkonfigurationsdetails für APIs.|
|/service/apis/policy/delete|Dient zum Entfernen der Richtlinienkonfiguration von APIs.|
|/service/apis/operations/read|Dient zum Abrufen einer Liste mit vorhandenen API-Vorgängen oder zum Abrufen von Details zu API-Vorgängen.|
|/service/apis/operations/write|Dient zum Erstellen eines neuen API-Vorgangs oder zum Aktualisieren vorhandener API-Vorgänge.|
|/service/apis/operations/delete|Dient zum Entfernen vorhandener API-Vorgänge.|
|/service/apis/operations/policy/read|Dient zum Abrufen von Richtlinienkonfigurationsdetails für Vorgänge.|
|/service/apis/operations/policy/write|Dient zum Festlegen von Richtlinienkonfigurationsdetails für Vorgänge.|
|/service/apis/operations/policy/delete|Dient zum Entfernen der Richtlinienkonfiguration von Vorgängen.|
|/service/products/read|Dient zum Abrufen einer Liste mit Produkten oder zum Abrufen von Produktdetails.|
|/service/products/write|Dient zum Erstellen eines neuen Produkts oder zum Aktualisieren vorhandener Produktdetails.|
|/service/products/delete|Dient zum Entfernen vorhandener Produkt.|
|/service/products/subscriptions/read|Dient zum Abrufen einer Liste mit Produktabonnements.|
|/service/products/apis/read|Dient zum Abrufen einer Liste mit APIs, die einem vorhandenen Produkt hinzugefügt wurden.|
|/service/products/apis/write|Dient zum Hinzufügen von vorhandenen APIs zu vorhandenen Produkten.|
|/service/products/apis/delete|Dient dazu, vorhandene APIs von vorhandenen Produkten zu entfernen.|
|/service/products/policy/read|Dient zum Abrufen der Richtlinienkonfiguration vorhandener Produkte.|
|/service/products/policy/write|Dient zum Festlegen der Richtlinienkonfiguration für vorhandene Produkte.|
|/service/products/policy/delete|Dient zum Entfernen der Richtlinienkonfiguration von vorhandenen Produkten.|
|/service/products/groups/read|Dient zum Abrufen einer Liste mit Entwicklergruppen, die einem Produkt zugeordnet sind.|
|/service/products/groups/write|Dient zum Zuordnen von vorhandenen Entwicklergruppen zu vorhandenen Produkten.|
|/service/products/groups/delete|Dient zum Löschen der Zuordnung von vorhandenen Entwicklergruppen zu vorhandenen Produkten.|
|/service/openidConnectProviders/read|Dient zum Abrufen einer Liste mit OpenID Connect-Anbietern oder zum Abrufen von Details zu OpenID Connect-Anbietern.|
|/service/openidConnectProviders/write|Dient zum Erstellen eines neuen OpenID Connect-Anbieters oder zum Aktualisieren der Details eines vorhandenen OpenID Connect-Anbieters.|
|/service/openidConnectProviders/delete|Dient zum Entfernen vorhandener OpenID Connect-Anbieter.|
|/service/certificates/read|Dient zum Abrufen einer Liste mit Zertifikaten oder zum Abrufen von Zertifikatdetails.|
|/service/certificates/write|Dient zum Hinzufügen eines neuen Zertifikats.|
|/service/certificates/delete|Dient zum Entfernen vorhandener Zertifikate.|
|/service/properties/read|Dient zum Abrufen einer Liste mit allen Eigenschaften oder zum Abrufen von Details zur angegebenen Eigenschaft.|
|/service/properties/write|Erstellt eine neue Eigenschaft oder aktualisiert den Wert für die angegebene Eigenschaft.|
|/service/properties/delete|Entfernt vorhandene Eigenschaften.|
|/service/groups/read|Dient zum Abrufen einer Liste mit Gruppen oder zum Abrufen von Details einer Gruppe.|
|/service/groups/write|Dient zum Erstellen einer neuen Gruppe oder zum Aktualisieren vorhandener Gruppendetails.|
|/service/groups/delete|Dient zum Entfernen vorhandener Gruppen.|
|/service/groups/users/read|Dient zum Abrufen einer Liste mit Gruppenbenutzern.|
|/service/groups/users/write|Dient zum Hinzufügen von vorhandenen Benutzern zu vorhandenen Gruppen.|
|/service/groups/users/delete|Dient zum Entfernen von vorhandenen Benutzern aus vorhandenen Gruppen.|
|/service/authorizationServers/read|Dient zum Abrufen einer Liste mit Autorisierungsservern oder zum Abrufen von Autorisierungsserverdetails.|
|/service/authorizationServers/write|Dient zum Erstellen eines neuen Autorisierungsservers oder zum Aktualisieren der Details eines bereits vorhandenen Autorisierungsservers.|
|/service/authorizationServers/delete|Dient zum Entfernen vorhandener Autorisierungsserver.|
|/service/reports/bySubscription/read|Dient zum Abrufen von nach Abonnement aggregierten Berichten.|
|/service/reports/byRequest/read|Dient zum Abrufen von Berichtsdaten für Anforderungen.|
|/service/reports/byOperation/read|Dient zum Abrufen von nach Vorgängen aggregierten Berichten.|
|/service/reports/byGeo/read|Dient zum Abrufen von nach geografischer Region aggregierten Berichten.|
|/service/reports/byUser/read|Dient zum Abrufen von nach Entwicklern aggregierten Berichten.|
|/service/reports/byTime/read|Dient zum Abrufen von nach Zeiträumen aggregierten Berichten.|
|/service/reports/byApi/read|Dient zum Abrufen von nach APIs aggregierten Berichten.|
|/service/reports/byProduct/read|Dient zum Abrufen von nach Produkten aggregierten Berichten.|

## <a name="microsoftappservice"></a>Microsoft.AppService

| Vorgang | Beschreibung |
|---|---|
|/appidentities/Read|Gibt die beim Gateway registrierte Ressource (Website) zurück.|
|/appidentities/Write|Erstellt eine neue App-Identität.|
|/appidentities/Delete|Löscht eine vorhandene App-Identität.|
|/deploymenttemplates/listMetadata/Action|Listet UI-Metadaten im Zusammenhang mit dem API-App-Paket auf.|
|/deploymenttemplates/generate/Action|Gibt eine Bereitstellungsvorlage zum Bereitstellen von API-App-Instanzen zurück.|
|/gateways/Read|Gibt die Gatewayinstanz zurück.|
|/gateways/Write|Erstellt ein neues Gateway oder aktualisiert ein bereits vorhandenes.|
|/gateways/Delete|Löscht eine vorhandene Gatewayinstanz.|
|/gateways/listLoginUris/Action|Füllt den Tokenspeicher auf und gibt OAuth-Anmelde-URIs zurück.|
|/gateways/listKeys/Action|Gibt Gatewaygeheimnisse zurück.|
|/gateways/tokens/Write|Erstellt ein neues Zumo-Token mit dem angegebenen Namen.|
|/gateways/registrations/Read|Gibt die beim Gateway registrierte Ressource (Website) zurück.|
|/gateways/registrations/Write|Registriert eine Ressource (Website) beim Gateway.|
|/gateways/registrations/Delete|Hebt die Registrierung einer Ressource (Website) beim Gateway auf.|
|/apiapps/Read|Gibt die API-App-Instanz zurück.|
|/apiapps/Write|Erstellt eine neue API-App oder aktualisiert eine bereits vorhandene.|
|/apiapps/Delete|Löscht eine vorhandene API-App-Instanz.|
|/apiapps/listStatus/Action|Gibt den API-App-Status zurück.|
|/apiapps/listKeys/Action|Gibt API-App-Geheimnisse zurück.|
|/apiapps/apidefinitions/Read|Gibt die API-Definition der API-App zurück.|

## <a name="microsoftauthorization"></a>Microsoft.Authorization

| Vorgang | Beschreibung |
|---|---|
|/elevateAccess/action|Gewährt dem Aufrufer Zugriff vom Typ „Benutzerzugriffsadministrator“ auf der Mandantenebene.|
|/classicAdministrators/read|Liest die Administratoren für das Abonnement.|
|/classicAdministrators/write|Dient zum Hinzufügen oder Ändern von Administratoren für ein Abonnement.|
|/classicAdministrators/delete|Entfernt den Administrator aus dem Abonnement.|
|/locks/read|Ruft Sperren im angegebenen Bereich ab.|
|/locks/write|Dient zum Hinzufügen von Sperren im angegebenen Bereich.|
|/locks/delete|Dient zum Löschen von Sperren im angegebenen Bereich.|
|/policyAssignments/read|Dient zum Abrufen von Informationen zu einer Richtlinienzuweisung.|
|/policyAssignments/write|Dient zum Erstellen einer Richtlinienzuweisung im angegebenen Bereich.|
|/policyAssignments/delete|Dient zum Löschen einer Richtlinienzuweisung im angegebenen Bereich.|
|/permissions/read|Listet alle Berechtigungen auf, über die der Aufrufer in einem bestimmten Bereich verfügt.|
|/roleDefinitions/read|Dient zum Abrufen von Informationen zu einer Rollendefinition.|
|/roleDefinitions/write|Dient zum Erstellen oder Aktualisieren einer benutzerdefinierten Rollendefinition mit angegebenen Berechtigungen und zuweisbaren Bereichen.|
|/roleDefinitions/delete|Dient zum Löschen der angegebenen benutzerdefinierten Rollendefinition.|
|/providerOperations/read|Dient zum Abrufen von Vorgängen für alle Ressourcenanbieter, die in Rollendefinitionen verwendet werden können.|
|/policyDefinitions/read|Dient zum Abrufen von Informationen zu einer Richtliniendefinition.|
|/policyDefinitions/write|Dient zum Erstellen einer benutzerdefinierten Richtliniendefinition.|
|/policyDefinitions/delete|Dient zum Löschen einer Richtliniendefinition.|
|/roleAssignments/read|Dient zum Abrufen von Informationen zu einer Rollenzuweisung.|
|/roleAssignments/write|Dient zum Erstellen einer Rollenzuweisung im angegebenen Bereich.|
|/roleAssignments/delete|Dient zum Löschen einer Rollenzuweisung im angegebenen Bereich.|

## <a name="microsoftautomation"></a>Microsoft.Automation

| Vorgang | Beschreibung |
|---|---|
|/automationAccounts/read|Ruft ein Azure Automation-Konto ab.|
|/automationAccounts/write|Erstellt oder aktualisiert ein Azure Automation-Konto.|
|/automationAccounts/delete|Löscht ein Azure Automation-Konto.|
|/automationAccounts/configurations/readContent/action|Ruft den Inhalt einer Azure Automation DSC-Instanz ab.|
|/automationAccounts/hybridRunbookWorkerGroups/read|Liest Hybrid Runbook Worker-Ressourcen.|
|/automationAccounts/hybridRunbookWorkerGroups/delete|Löscht Hybrid Runbook Worker-Ressourcen.|
|/automationAccounts/jobSchedules/read|Ruft einen Azure Automation-Auftragszeitplan ab.|
|/automationAccounts/jobSchedules/write|Erstellt einen Azure Automation-Auftragszeitplan.|
|/automationAccounts/jobSchedules/delete|Löscht einen Azure Automation-Auftragszeitplan.|
|/automationAccounts/connectionTypes/read|Ruft ein Azure Automation-Verbindungstypasset ab.|
|/automationAccounts/connectionTypes/write|Erstellt ein Azure Automation-Verbindungstypasset.|
|/automationAccounts/connectionTypes/delete|Löscht ein Azure Automation-Verbindungstypasset.|
|/automationAccounts/modules/read|Ruft ein Azure Automation-Modul ab.|
|/automationAccounts/modules/write|Erstellt oder aktualisiert ein Azure Automation-Modul.|
|/automationAccounts/modules/delete|Löscht ein Azure Automation-Modul.|
|/automationAccounts/credentials/read|Ruft ein Azure Automation-Anmeldeinformationsasset ab.|
|/automationAccounts/credentials/write|Erstellt oder aktualisiert ein Azure Automation-Anmeldeinformationsasset.|
|/automationAccounts/credentials/delete|Löscht ein Azure Automation-Anmeldeinformationsasset.|
|/automationAccounts/certificates/read|Ruft ein Azure Automation-Zertifikatasset ab.|
|/automationAccounts/certificates/write|Erstellt oder aktualisiert ein Azure Automation-Zertifikatasset.|
|/automationAccounts/certificates/delete|Löscht ein Azure Automation-Zertifikatasset.|
|/automationAccounts/schedules/read|Ruft ein Azure Automation-Zeitplanasset ab.|
|/automationAccounts/schedules/write|Erstellt oder aktualisiert ein Azure Automation-Zeitplanasset.|
|/automationAccounts/schedules/delete|Löscht ein Azure Automation-Zeitplanasset.|
|/automationAccounts/jobs/read|Ruft einen Azure Automation-Auftrag ab.|
|/automationAccounts/jobs/write|Erstellt einen Azure Automation-Auftrag.|
|/automationAccounts/jobs/stop/action|Beendet einen Azure Automation-Auftrag.|
|/automationAccounts/jobs/suspend/action|Hält einen Azure Automation-Auftrag an.|
|/automationAccounts/jobs/resume/action|Setzt einen Azure Automation-Auftrag fort.|
|/automationAccounts/jobs/runbookContent/action|Ruft den Inhalt des Azure Automation-Runbooks zum Zeitpunkt der Auftragsausführung ab.|
|/automationAccounts/jobs/output/action|Ruft die Ausgabe eines Auftrags ab.|
|/automationAccounts/jobs/read|Ruft einen Azure Automation-Auftrag ab.|
|/automationAccounts/jobs/write|Erstellt einen Azure Automation-Auftrag.|
|/automationAccounts/jobs/stop/action|Beendet einen Azure Automation-Auftrag.|
|/automationAccounts/jobs/suspend/action|Hält einen Azure Automation-Auftrag an.|
|/automationAccounts/jobs/resume/action|Setzt einen Azure Automation-Auftrag fort.|
|/automationAccounts/jobs/streams/read|Ruft einen Azure Automation-Auftragsdatenstrom ab.|
|/automationAccounts/connections/read|Ruft ein Azure Automation-Verbindungsasset ab.|
|/automationAccounts/connections/write|Erstellt oder aktualisiert ein Azure Automation-Verbindungsasset.|
|/automationAccounts/connections/delete|Löscht ein Azure Automation-Verbindungsasset.|
|/automationAccounts/variables/read|Liest ein Azure Automation-Variablenasset.|
|/automationAccounts/variables/write|Erstellt oder aktualisiert ein Azure Automation-Variablenasset.|
|/automationAccounts/variables/delete|Löscht ein Azure Automation-Variablenasset.|
|/automationAccounts/runbooks/readContent/action|Ruft den Inhalt eines Azure Automation-Runbooks ab.|
|/automationAccounts/runbooks/read|Ruft ein Azure Automation-Runbook ab.|
|/automationAccounts/runbooks/write|Erstellt oder aktualisiert ein Azure Automation-Runbook.|
|/automationAccounts/runbooks/delete|Löscht ein Azure Automation-Runbook.|
|/automationAccounts/runbooks/draft/readContent/action|Ruft den Inhalt eines Azure Automation-Runbookentwurfs ab.|
|/automationAccounts/runbooks/draft/writeContent/action|Erstellt den Inhalt eines Azure Automation-Runbookentwurfs.|
|/automationAccounts/runbooks/draft/read|Ruft einen Azure Automation-Runbookentwurf ab.|
|/automationAccounts/runbooks/draft/publish/action|Veröffentlicht einen Azure Automation-Runbookentwurf.|
|/automationAccounts/runbooks/draft/undoEdit/action|Macht die Bearbeitung eines Azure Automation-Runbookentwurfs rückgängig.|
|/automationAccounts/runbooks/draft/testJob/read|Ruft einen Testauftrag für einen Azure Automation-Runbookentwurf ab.|
|/automationAccounts/runbooks/draft/testJob/write|Erstellt einen Testauftrag für einen Azure Automation-Runbookentwurf.|
|/automationAccounts/runbooks/draft/testJob/stop/action|Beendet einen Testauftrag für einen Azure Automation-Runbookentwurf.|
|/automationAccounts/runbooks/draft/testJob/suspend/action|Hält einen Testauftrag für einen Azure Automation-Runbookentwurf an.|
|/automationAccounts/runbooks/draft/testJob/resume/action|Setzt einen Testauftrag für einen Azure Automation-Runbookentwurf fort.|
|/automationAccounts/webhooks/read|Liest einen Azure Automation-Webhook.|
|/automationAccounts/webhooks/write|Erstellt oder aktualisiert einen Azure Automation-Webhook.|
|/automationAccounts/webhooks/delete|Löscht einen Azure Automation-Webhook. |
|/automationAccounts/webhooks/generateUri/action|Generiert einen URI für einen Azure Automation-Webhook.|

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

Dieser Anbieter ist kein vollwertiger ARM-Anbieter und stellt keine ARM-Vorgänge bereit.

## <a name="microsoftbatch"></a>Microsoft.Batch

| Vorgang | Beschreibung |
|---|---|
|/register/action|Registriert das Abonnement für den Batch-Ressourcenanbieter und ermöglicht die Erstellung von Batch-Konten.|
|/batchAccounts/write|Erstellt ein neues Batch-Konto oder aktualisiert ein vorhandenes Batch-Konto.|
|/batchAccounts/read|Listet Batch-Konten auf oder ruft die Eigenschaften eines Batch-Kontos ab.|
|/batchAccounts/delete|Löscht ein Batch-Konto.|
|/batchAccounts/listkeys/action|Listet Zugriffsschlüssel für ein Batch-Konto auf.|
|/batchAccounts/regeneratekeys/action|Generiert Zugriffsschlüssel für ein Batch-Konto neu.|
|/batchAccounts/syncAutoStorageKeys/action|Synchronisiert Zugriffsschlüssel für das automatische Speicherkonto, das für ein Batch-Konto konfiguriert ist.|
|/batchAccounts/applications/read|Listet Anwendungen auf oder ruft die Eigenschaften einer Anwendung ab.|
|/batchAccounts/applications/write|Erstellt eine neue Anwendung oder aktualisiert eine vorhandene Anwendung.|
|/batchAccounts/applications/delete|Löscht eine Anwendung.|
|/batchAccounts/applications/versions/read|Ruft die Eigenschaften eines Anwendungspakets ab.|
|/batchAccounts/applications/versions/write|Erstellt ein neues Anwendungspaket oder aktualisiert ein vorhandenes Anwendungspaket.|
|/batchAccounts/applications/versions/activate/action|Aktiviert ein Anwendungspaket.|
|/batchAccounts/applications/versions/delete|Löscht ein Anwendungspaket.|
|/locations/quotas/read|Ruft Batch-Kontingente des angegebenen Abonnements in der angegebenen Azure-Region ab.|

## <a name="microsoftbilling"></a>Microsoft.Billing

| Vorgang | Beschreibung |
|---|---|
|/invoices/read|Listet verfügbare Rechnungen auf.|

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

| Vorgang | Beschreibung |
|---|---|
|/mapApis/Read|Lesevorgang|
|/mapApis/Write|Schreibvorgang|
|/mapApis/Delete|Löschvorgang|
|/mapApis/regenerateKey/action|Generiert den Schlüssel neu.|
|/mapApis/listSecrets/action|Dient zum Auflisten der Geheimnisse.|
|/mapApis/listSingleSignOnToken/action|Dient zum Lesen des SSO-Autorisierungstokens für die Ressource.|
|/Operations/read|Beschreibung des Vorgangs.|

## <a name="microsoftcache"></a>Microsoft.Cache

| Vorgang | Beschreibung |
|---|---|
|/checknameavailability/action|Prüft, ob ein Name für die Verwendung mit einer neuen Redis Cache-Instanz verfügbar ist.|
|/register/action|Registriert den Ressourcenanbieter „Microsoft.Cache“ bei einem Abonnement.|
|/unregister/action|Hebt die Registrierung des Ressourcenanbieters „Microsoft.Cache“ bei einem Abonnement auf.|
|/redis/write|Dient zum Ändern der Redis Cache-Einstellungen und -Konfiguration im Verwaltungsportal.|
|/redis/read|Dient zum Anzeigen der Redis Cache-Einstellungen und -Konfiguration im Verwaltungsportal.|
|/redis/delete|Dient zum Löschen der gesamten Redis Cache-Instanz.|
|/redis/listKeys/action|Dient zum Anzeigen des Werts von Redis Cache-Zugriffsschlüsseln im Verwaltungsportal.|
|/redis/regenerateKey/action|Dient zum Ändern des Werts von Redis Cache-Zugriffsschlüsseln im Verwaltungsportal.|
|/redis/import/action|Dient zum Importieren von Daten eines angegebenen Formats aus mehreren Blobs in Redis.|
|/redis/export/action|Dient dazu, Redis-Daten im angegebenen Format in Speicherblobs mit Präfix zu exportieren.|
|/redis/forceReboot/action|Dient zum Erzwingen des Neustarts einer Cache-Instanz (ggf. mit Datenverlust).|
|/redis/stop/action|Dient zum Beenden einer Cache-Instanz.|
|/redis/start/action|Dient zum Starten einer Cache-Instanz.|
|/redis/metricDefinitions/read|Ruft die verfügbaren Metriken für eine Redis Cache-Instanz ab.|
|/redis/firewallRules/read|Dient zum Abrufen der IP-Firewallregeln einer Redis Cache-Instanz.|
|/redis/firewallRules/write|Dient zum Bearbeiten der IP-Firewallregeln einer Redis Cache-Instanz.|
|/redis/firewallRules/delete|Dient zum Löschen der IP-Firewallregeln einer Redis Cache-Instanz.|
|/redis/listUpgradeNotifications/read|Dient zum Auflisten der neuesten Upgradebenachrichtigungen für den Cache-Mandanten.|
|/redis/linkedservers/read|Dient zum Abrufen von Verbindungsservern, die einer Redis Cache-Instanz zugeordnet sind.|
|/redis/linkedservers/write|Dient zum Hinzufügen von Verbindungsservern zu einer Redis Cache-Instanz.|
|/redis/linkedservers/delete|Dient zum Löschen von Verbindungsservern aus einer Redis Cache-Instanz.|
|/redis/patchSchedules/read|Ruft den Patchzeitplan einer Redis Cache-Instanz ab.|
|/redis/patchSchedules/write|Dient zum Ändern des Patchzeitplans einer Redis Cache-Instanz.|
|/redis/patchSchedules/delete|Dient zum Löschen des Patchzeitplans einer Redis Cache-Instanz.|

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

| Vorgang | Beschreibung |
|---|---|
|/provisionGlobalAppServicePrincipalInUserTenant/Action|Dient zum Bereitstellen eines Dienstprinzipals für einen Dienst-App-Prinzipal.|
|/validateCertificateRegistrationInformation/Action|Dient zum Überprüfen eines Zertifikaterwerbsobjekts, ohne es zu übermitteln.|
|/register/action|Dient zum Registrieren des Microsoft-Zertifikatressourcenanbieters für das Abonnement.|
|/certificateOrders/Write|Dient zum Hinzufügen eines neuen certificateOrder-Elements oder zum Aktualisieren eines bereits vorhandenen.|
|/certificateOrders/Delete|Dient zum Löschen eines vorhandenen AppServiceCertificate-Elements.|
|/certificateOrders/Read|Dient zum Abrufen der CertificateOrders-Liste.|
|/certificateOrders/reissue/Action|Dient zum erneuten Ausstellen einer vorhandenen Zertifikatbestellung.|
|/certificateOrders/renew/Action|Dient zum Verlängern einer vorhandenen Zertifikatbestellung.|
|/certificateOrders/retrieveCertificateActions/Action|Dient zum Abrufen der Liste mit Zertifikataktionen.|
|/certificateOrders/retrieveEmailHistory/Action|Dient zum Abrufen des Zertifikat-E-Mail-Verlaufs.|
|/certificateOrders/resendEmail/Action|Dient zum erneuten Senden von Zertifikat-E-Mails.|
|/certificateOrders/verifyDomainOwnership/Action|Überprüfen des Domänenbesitzes|
|/certificateOrders/resendRequestEmails/Action|Dient zum erneuten Senden von Anforderungs-E-Mails an eine andere E-Mail-Adresse.|
|/certificateOrders/resendRequestEmails/Action|Dient zum Abrufen eines Websitesiegels für ein ausgestelltes App Service-Zertifikat.|
|/certificateOrders/certificates/Write|Dient zum Hinzufügen eines neuen Zertifikats oder zum Aktualisieren eines bereits vorhandenen.|
|/certificateOrders/certificates/Delete|Dient zum Löschen eines vorhandenen Zertifikats.|
|/certificateOrders/certificates/Read|Dient zum Abrufen der Zertifikatliste.|

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

| Vorgang | Beschreibung |
|---|---|
|/register/action|Dient zum Registrieren für klassische Computeressourcen.|
|/checkDomainNameAvailability/action|Prüft die Verfügbarkeit eines angegebenen Domänennamens.|
|/moveSubscriptionResources/action|Dient zum Verschieben aller klassischen Ressourcen in ein anderes Abonnement.|
|/validateSubscriptionMoveAvailability/action|Dient zum Überprüfen der Verfügbarkeit des Abonnements für einen klassischen Verschiebevorgang.|
|/operatingSystemFamilies/read|Listet die in Microsoft Azure verfügbaren Gastbetriebssystemfamilien sowie die jeweils verfügbaren Betriebssystemversionen auf.
|/capabilities/read|Zeigt die Funktionen an.|
|/operatingSystems/read|Listet die Versionen des Gastbetriebssystems auf, die derzeit in Microsoft Azure verfügbar sind.|
|/resourceTypes/skus/read|Ruft die SKU-Liste für unterstützte Ressourcentypen ab.|
|/domainNames/read|Dient zum Zurückgeben der Domänennamen für Ressourcen.|
|/domainNames/write|Dient zum Hinzufügen oder Ändern der Domänennamen für Ressourcen.|
|/domainNames/delete|Dient zum Entfernen der Domänennamen für Ressourcen.|
|/domainNames/swap/action|Vertauscht Stagingslot und Produktionsslot.|
|/domainNames/serviceCertificates/read|Gibt die verwendeten Dienstzertifikate zurück.|
|/domainNames/serviceCertificates/write|Dient zum Hinzufügen oder Ändern der verwendeten Dienstzertifikate.|
|/domainNames/serviceCertificates/delete|Dient zum Löschen der verwendeten Dienstzertifikate.|
|/domainNames/serviceCertificates/operationStatuses/read|Liest den Vorgangsstatus für die Dienstzertifikate der Domänennamen.|
|/domainNames/capabilities/read|Zeigt die Domänennamenfunktionen an.|
|/domainNames/extensions/read|Gibt die Domänennamenerweiterungen zurück.|
|/domainNames/extensions/write|Dient zum Hinzufügen der Domänennamenerweiterungen.|
|/domainNames/extensions/delete|Dient zum Entfernen der Domänennamenerweiterungen.|
|/domainNames/extensions/operationStatuses/read|Liest den Vorgangsstatus für die Domänennamenerweiterungen.|
|/domainNames/active/write|Legt den aktiven Domänennamen fest.|
|/domainNames/slots/read|Zeigt die Bereitstellungsslots an.|
|/domainNames/slots/write|Erstellt oder aktualisiert die Bereitstellung.|
|/domainNames/slots/delete|Löscht einen angegebenen Bereitstellungsslot.|
|/domainNames/slots/start/action|Startet einen Bereitstellungsslot.|
|/domainNames/slots/stop/action|Hält den Bereitstellungsslot an.|
|/domainNames/slots/operationStatuses/read|Liest den Vorgangsstatus für die Domänennamenslots.|
|/domainNames/slots/roles/read|Dient zum Abrufen der Rolle für den Bereitstellungsslot.|
|/domainNames/slots/roles/extensionReferences/read|Gibt den Erweiterungsverweis für die Rolle „Bereitstellungsslot“ zurück.|
|/domainNames/slots/roles/extensionReferences/write|Dient zum Hinzufügen oder Ändern des Erweiterungsverweises für die Rolle „Bereitstellungsslot“.|
|/domainNames/slots/roles/extensionReferences/delete|Dient zum Entfernen des Erweiterungsverweises für die Rolle „Bereitstellungsslot“.|
|/domainNames/slots/roles/extensionReferences/operationStatuses/read|Liest den Vorgangsstatus für die Slotrollen-Erweiterungsverweise der Domänennamen.|
|/domainNames/slots/roles/roleInstances/read|Dient zum Abrufen der Rolleninstanz.|
|/domainNames/slots/roles/roleInstances/restart/action|Startet Rolleninstanzen neu.|
|/domainNames/slots/roles/roleInstances/reimage/action|Führt ein Reimaging der Rolleninstanz aus.|
|/domainNames/slots/roles/roleInstances/operationStatuses/read|Liest den Vorgangsstatus für die Slotrollen-Rolleninstanzen der Domänennamen.|
|/domainNames/slots/state/start/write|Ändert den Zustand des Bereitstellungsslots in „Beendet“.|
|/domainNames/slots/state/stop/write|Ändert den Zustand des Bereitstellungsslots in „Gestartet“.|
|/domainNames/slots/upgradeDomain/write|Dient zum Aufrüsten der Domäne.|
|/domainNames/internalLoadBalancers/read|Ruft die internen Lastenausgleichsmodule ab.|
|/domainNames/internalLoadBalancers/write|Erstellt ein neues internes Lastenausgleichsmodul.|
|/domainNames/internalLoadBalancers/delete|Dient zum Entfernen eines neuen internen Lastenausgleichsmoduls.|
|/domainNames/internalLoadBalancers/operationStatuses/read|Liest den Vorgangsstatus für die internen Lastenausgleichsmodule der Domänennamen.|
|/domainNames/loadBalancedEndpointSets/read|Zeigt die Endpunktgruppen mit Lastenausgleich an.|
|/domainNames/loadBalancedEndpointSets/operationStatuses/read|Liest den Vorgangsstatus für die Endpunktgruppen mit Lastenausgleich der Domänennamen.|
|/domainNames/availabilitySets/read|Dient zum Anzeigen der Verfügbarkeitsgruppe für die Ressource.|
|/quotas/read|Dient zum Abrufen des Kontingents für das Abonnement.|
|/virtualMachines/read|Ruft eine Liste mit virtuellen Computern ab.|
|/virtualMachines/write|Dient zum Hinzufügen oder Ändern virtueller Computer.|
|/virtualMachines/delete|Entfernt virtuelle Computer.|
|/virtualMachines/start/action|Starten Sie den virtuellen Computer.|
|/virtualMachines/redeploy/action|Stellt den virtuellen Computer erneut bereit.|
|/virtualMachines/restart/action|Startet virtuelle Computer neu.|
|/virtualMachines/stop/action|Beendet den virtuellen Computer.|
|/virtualMachines/shutdown/action|Fährt den virtuellen Computer herunter.|
|/virtualMachines/attachDisk/action|Fügt einem virtuellen Computer einen Datenträger an.|
|/virtualMachines/detachDisk/action|Trennt einen Datenträger von einem virtuellen Computer.|
|/virtualMachines/downloadRemoteDesktopConnectionFile/action|Lädt die RDP-Datei für einen virtuellen Computer herunter.|
|/virtualMachines/networkInterfaces/<br>associatedNetworkSecurityGroups/read|Ruft die Netzwerksicherheitsgruppe ab, die der Netzwerkschnittstelle zugeordnet ist.|
|/virtualMachines/networkInterfaces/<br>associatedNetworkSecurityGroups/write|Fügt eine Netzwerksicherheitsgruppe hinzu, die der Netzwerkschnittstelle zugeordnet ist.|
|/virtualMachines/networkInterfaces/<br>associatedNetworkSecurityGroups/delete|Löscht die Netzwerksicherheitsgruppe, die der Netzwerkschnittstelle zugeordnet ist.|
|/virtualMachines/networkInterfaces/<br>associatedNetworkSecurityGroups/operationStatuses/read|Liest den Vorgangsstatus für die den virtuellen Computern zugeordneten Netzwerksicherheitsgruppen.|
|/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read|Ruft die Metrikdefinitionen ab.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/read|Dient zum Abrufen der Diagnoseeinstellungen.|
|/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/write|Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen.|
|/virtualMachines/metrics/read|Ruft die Metriken ab.|
|/virtualMachines/operationStatuses/read|Liest den Vorgangsstatus für die virtuellen Computer.|
|/virtualMachines/extensions/read|Ruft die VM-Erweiterung ab.|
|/virtualMachines/extensions/write|Legt die VM-Erweiterung fest.|
|/virtualMachines/extensions/operationStatuses/read|Liest den Vorgangsstatus für die VM-Erweiterungen.|
|/virtualMachines/asyncOperations/read|Ruft die möglichen asynchronen Vorgänge ab.|
|/virtualMachines/disks/read|Ruft die Liste mit den Datenträgern ab.|
|/virtualMachines/associatedNetworkSecurityGroups/read|Ruft die Netzwerksicherheitsgruppe ab, die dem virtuellen Computer zugeordnet ist.|
|/virtualMachines/associatedNetworkSecurityGroups/write|Fügt eine Netzwerksicherheitsgruppe hinzu, die dem virtuellen Computer zugeordnet ist.|
|/virtualMachines/associatedNetworkSecurityGroups/delete|Löscht die Netzwerksicherheitsgruppe, die dem virtuellen Computer zugeordnet ist.|
|/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/read|Liest den Vorgangsstatus für die den virtuellen Computern zugeordneten Netzwerksicherheitsgruppen.|

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

| Vorgang | Beschreibung |
|---|---|
|/register/action|Dient zum Registrieren für klassische Netzwerkressourcen.|
|/gatewaySupportedDevices/read|Ruft die Liste mit unterstützten Geräten ab.|
|/reservedIps/read|Ruft die reservierten IP-Adressen ab.|
|/reservedIps/write|Dient zum Hinzufügen einer neuen reservierten IP-Adresse.|
|/reservedIps/delete|Dient zum Löschen einer reservierten IP-Adresse.|
|/reservedIps/link/action|Dient zum Verknüpfen einer reservierten IP-Adresse.|
|/reservedIps/join/action|Dient zum Einbinden einer reservierten IP-Adresse.|
|/reservedIps/operationStatuses/read|Liest den Vorgangsstatus für die reservierten IP-Adressen.|
|/virtualNetworks/read|Dient zum Abrufen des virtuellen Netzwerks.|
|/virtualNetworks/write|Dient zum Hinzufügen eines neuen virtuellen Netzwerks.|
|/virtualNetworks/delete|Löscht das virtuelle Netzwerk.|
|/virtualNetworks/peer/action|Richtet ein virtuelles Netzwerk als Peer für ein anderes virtuelles Netzwerk ein.|
|/virtualNetworks/join/action|Führt zum Beitritt zum virtuellen Netzwerk.|
|/virtualNetworks/checkIPAddressAvailability/action|Prüft die Verfügbarkeit einer bestimmten IP-Adresse in einem virtuellen Netzwerk.|
|/virtualNetworks/capabilities/read|Zeigt die Funktionen an.|
|/virtualNetworks/subnets/<br>associatedNetworkSecurityGroups/read|Ruft die Netzwerksicherheitsgruppe ab, die dem Subnetz zugeordnet ist.|
|/virtualNetworks/subnets/<br>associatedNetworkSecurityGroups/write|Fügt die Netzwerksicherheitsgruppe hinzu, die dem Subnetz zugeordnet ist.|
|/virtualNetworks/subnets/<br>associatedNetworkSecurityGroups/delete|Löscht die Netzwerksicherheitsgruppe, die dem Subnetz zugeordnet ist.|
|/virtualNetworks/subnets/<br>associatedNetworkSecurityGroups/operationStatuses/read|Liest den Vorgangsstatus für die dem Subnetz des virtuellen Netzwerks zugeordnete Netzwerksicherheitsgruppe.|
|/virtualNetworks/operationStatuses/read|Liest den Vorgangsstatus für die virtuellen Netzwerke.|
|/virtualNetworks/gateways/read|Ruft die virtuellen Netzwerkgateways ab.|
|/virtualNetworks/gateways/write|Fügt ein virtuelles Netzwerkgateway hinzu.|
|/virtualNetworks/gateways/delete|Löscht das virtuelle Netzwerkgateway.|
|/virtualNetworks/gateways/startDiagnostics/action|Startet die Diagnose für das virtuelle Netzwerkgateway.|
|/virtualNetworks/gateways/stopDiagnostics/action|Beendet die Diagnose für das virtuelle Netzwerkgateway.|
|/virtualNetworks/gateways/downloadDiagnostics/action|Lädt die Gatewaydiagnose herunter.|
|/virtualNetworks/gateways/listCircuitServiceKey/action|Ruft den Schlüssel des Verbindungsdiensts ab.|
|/virtualNetworks/gateways/downloadDeviceConfigurationScript/action|Lädt das Gerätekonfigurationsskript herunter.|
|/virtualNetworks/gateways/listPackage/action|Listet das Paket für das virtuelle Netzwerkgateway auf.|
|/virtualNetworks/gateways/operationStatuses/read|Liest den Vorgangsstatus für die virtuellen Netzwerkgateways.|
|/virtualNetworks/gateways/packages/read|Ruft das Paket für das virtuelle Netzwerkgateway ab.|
|/virtualNetworks/gateways/connections/read|Ruft die Verbindungsliste ab.|
|/virtualNetworks/gateways/connections/connect/action|Stellt eine Site-to-Site-Gatewayverbindung her.|
|/virtualNetworks/gateways/connections/disconnect/action|Trennt eine Site-to-Site-Gatewayverbindung.|
|/virtualNetworks/gateways/connections/test/action|Testet eine Site-to-Site-Gatewayverbindung.|
|/virtualNetworks/gateways/clientRevokedCertificates/read|Dient zum Lesen der gesperrten Clientzertifikate.|
|/virtualNetworks/gateways/clientRevokedCertificates/write|Sperrt ein Clientzertifikat.|
|/virtualNetworks/gateways/clientRevokedCertificates/delete|Hebt die Sperrung eines Clientzertifikats auf.|
|/virtualNetworks/gateways/clientRootCertificates/read|Dient zum Suchen nach den Clientstammzertifikaten.|
|/virtualNetworks/gateways/clientRootCertificates/write|Lädt ein neues Clientstammzertifikat hoch.|
|/virtualNetworks/gateways/clientRootCertificates/delete|Löscht das Clientzertifikat des virtuellen Netzwerkgateways.|
|/virtualNetworks/gateways/clientRootCertificates/download/action|Lädt ein Zertifikat nach Fingerabdruck herunter.|
|/virtualNetworks/gateways/clientRootCertificates/listPackage/action|Listet das Zertifikatpaket für das virtuelle Netzwerkgateway auf.|
|/networkSecurityGroups/read|Ruft die Netzwerksicherheitsgruppe ab.|
|/networkSecurityGroups/write|Fügt eine neue Netzwerksicherheitsgruppe hinzu.|
|/networkSecurityGroups/delete|Löscht die Netzwerksicherheitsgruppe.|
|/networkSecurityGroups/operationStatuses/read|Liest den Vorgangsstatus für die Netzwerksicherheitsgruppe.|
|/networkSecurityGroups/securityRules/read|Ruft die Sicherheitsregel ab.|
|/networkSecurityGroups/securityRules/write|Fügt eine Sicherheitsregel hinzu oder aktualisiert sie.|
|/networkSecurityGroups/securityRules/delete|Löscht die Sicherheitsregel.|
|/networkSecurityGroups/securityRules/operationStatuses/read|Liest den Vorgangsstatus für die Sicherheitsregeln der Netzwerksicherheitsgruppe.|
|/quotas/read|Dient zum Abrufen des Kontingents für das Abonnement.|

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

| Vorgang | Beschreibung |
|---|---|
|/register/action|Dient zum Registrieren für klassischen Speicher.|
|/checkStorageAccountAvailability/action|Prüft die Verfügbarkeit eines Speicherkontos.|
|/capabilities/read|Zeigt die Funktionen an.|
|/publicImages/read|Ruft das öffentliche VM-Image ab.|
|/images/read|Gibt das Image zurück.|
|/storageAccounts/read|Dient zum Zurückgeben des Speicherkontos mit dem angegebenen Konto.|
|/storageAccounts/write|Fügt ein neues Speicherkonto hinzu.|
|/storageAccounts/delete|Dient zum Löschen des Speicherkontos.|
|/storageAccounts/listKeys/action|Listet die Zugriffsschlüssel für die Speicherkonten auf.|
|/storageAccounts/regenerateKey/action|Generiert die vorhandenen Zugriffsschlüssel für das Speicherkonto neu.|
|/storageAccounts/operationStatuses/read|Liest den Vorgangsstatus für die Ressource.|
|/storageAccounts/images/read|Gibt das Speicherkontoimage zurück.|
|/storageAccounts/images/delete|Löscht das angegebene Speicherkontoimage.|
|/storageAccounts/disks/read|Gibt den Speicherkontodatenträger zurück.|
|/storageAccounts/disks/write|Fügt einen Speicherkontodatenträger hinzu.|
|/storageAccounts/disks/delete|Löscht den angegebenen Speicherkontodatenträger.|
|/storageAccounts/disks/operationStatuses/read|Liest den Vorgangsstatus für die Ressource.|
|/storageAccounts/osImages/read|Gibt das Betriebssystemimage für das Speicherkonto zurück.|
|/storageAccounts/osImages/delete|Löscht das angegebene Betriebssystemimage für das Speicherkonto.|
|/storageAccounts/services/read|Dient zum Abrufen der verfügbaren Dienste.|
|/storageAccounts/services/metricDefinitions/read|Ruft die Metrikdefinitionen ab.|
|/storageAccounts/services/metrics/read|Ruft die Metriken ab.|
|/storageAccounts/services/diagnosticSettings/read|Dient zum Abrufen der Diagnoseeinstellungen.|
|/storageAccounts/services/diagnosticSettings/write|Dient zum Hinzufügen oder Ändern der Diagnoseeinstellungen.|
|/disks/read|Gibt den Speicherkontodatenträger zurück.|
|/osImages/read|Gibt das Betriebssystemimage zurück.|
|/quotas/read|Dient zum Abrufen des Kontingents für das Abonnement.|

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

| Vorgang | Beschreibung |
|---|---|
|/accounts/read|Liest API-Konten.|
|/accounts/write|Schreibt API-Konten.|
|/accounts/delete|Löscht API-Konten.|
|/accounts/listKeys/action|Dient zum Auflisten von Schlüsseln.|
|/accounts/regenerateKey/action|Dient zum erneuten Generieren eines Schlüssels.|
|/accounts/skus/read|Liest verfügbare SKUs für eine vorhandene Ressource.|
|/accounts/usages/read|Dient zum Abrufen der Kontingentnutzung für eine vorhandene Ressource.|
|/Operations/read|Beschreibung des Vorgangs.|

## <a name="microsoftcommerce"></a>Microsoft.Commerce

| Vorgang | Beschreibung |
|---|---|
|/RateCard/read|Gibt Angebotsdaten, Metadaten für Ressourcen/Verbrauchseinheiten und Preise für das angegebene Abonnement zurück.|
|/UsageAggregates/read|Ruft die Nutzung von Microsoft Azure für ein Abonnement ab. Das Ergebnis enthält aggregierte Nutzungsdaten sowie abonnement- und ressourcenbezogene Informationen für einen bestimmten Zeitbereich.|

## <a name="microsoftcompute"></a>Microsoft.Compute

| Vorgang | Beschreibung |
|---|---|
|/register/action|Registriert ein Abonnement beim Microsoft.Compute-Ressourcenanbieter.|
|/restorePointCollections/read|Dient zum Abrufen der Eigenschaften einer Wiederherstellungspunktsammlung.|
|/restorePointCollections/write|Erstellt eine neue Wiederherstellungspunktsammlung oder aktualisiert eine bereits vorhandene.|
|/restorePointCollections/delete|Löscht die Wiederherstellungspunktsammlung und die darin enthaltenen Wiederherstellungspunkte.|
|/restorePointCollections/restorePoints/read|Dient zum Abrufen der Eigenschaften eines Wiederherstellungspunkts.|
|/restorePointCollections/restorePoints/write|Erstellt einen neuen Wiederherstellungspunkt.|
|/restorePointCollections/restorePoints/delete|Löscht den Wiederherstellungspunkt.|
|/restorePointCollections/restorePoints/retrieveSasUris/action|Dient zum Abrufen der Eigenschaften eines Wiederherstellungspunkts sowie der Blob-SAS-URIs.|
|/virtualMachineScaleSets/read|Dient zum Abrufen der Eigenschaften einer VM-Skalierungsgruppe.|
|/virtualMachineScaleSets/write|Erstellt eine neue VM-Skalierungsgruppe oder aktualisiert eine bereits vorhandene.|
|/virtualMachineScaleSets/delete|Löscht die VM-Skalierungsgruppe.|
|/virtualMachineScaleSets/start/action|Startet die Instanzen der VM-Skalierungsgruppe.|
|/virtualMachineScaleSets/powerOff/action|Schaltet die Instanzen der VM-Skalierungsgruppe aus.|
|/virtualMachineScaleSets/restart/action|Startet die Instanzen der VM-Skalierungsgruppe neu.|
|/virtualMachineScaleSets/deallocate/action|Schaltet die Computeressourcen für die Instanzen der VM-Skalierungsgruppe aus und gibt sie frei. |
|/virtualMachineScaleSets/manualUpgrade/action|Aktualisiert Instanzen manuell auf das neueste Modell der VM-Skalierungsgruppe.|
|/virtualMachineScaleSets/scale/action|Dient zum horizontalen Hoch-/Herunterskalieren der Instanzanzahl einer vorhandenen VM-Skalierungsgruppe.|
|/virtualMachineScaleSets/instanceView/read|Ruft die Instanzansicht der VM-Skalierungsgruppe ab.|
|/virtualMachineScaleSets/skus/read|Listet die gültigen SKUs für eine vorhandene VM-Skalierungsgruppe auf.|
|/virtualMachineScaleSets/virtualMachines/read|Ruft die Eigenschaften eines virtuellen Computers in einer VM-Skalierungsgruppe ab.|
|/virtualMachineScaleSets/virtualMachines/delete|Dient zum Löschen eines bestimmten virtuellen Computers in einer VM-Skalierungsgruppe.|
|/virtualMachineScaleSets/virtualMachines/start/action|Startet eine Instanz eines virtuellen Computers in einer VM-Skalierungsgruppe.|
|/virtualMachineScaleSets/virtualMachines/powerOff/action|Schaltet eine Instanz eines virtuellen Computers in einer VM-Skalierungsgruppe aus.|
|/virtualMachineScaleSets/virtualMachines/restart/action|Startet eine Instanz eines virtuellen Computers in einer VM-Skalierungsgruppe neu.|
|/virtualMachineScaleSets/virtualMachines/deallocate/action|Schaltet die Computeressourcen für einen virtuellen Computer in einer VM-Skalierungsgruppe aus und gibt sie frei.|
|/virtualMachineScaleSets/virtualMachines/instanceView/read|Ruft die Instanzansicht eines virtuellen Computers in einer VM-Skalierungsgruppe ab.|
|/images/read|Dient zum Abrufen der Eigenschaften des Images.|
|/images/write|Erstellt ein neues Image oder aktualisiert ein bereits vorhandenes.|
|/images/delete|Löscht das Image.|
|/operations/read|Listet verfügbare Vorgänge für den Microsoft.Compute-Ressourcenanbieter auf.|
|/disks/read|Dient zum Abrufen der Eigenschaften eines Datenträgers.|
|/disks/write|Erstellt einen neuen Datenträger oder aktualisiert einen bereits vorhandenen.|
|/disks/delete|Löscht den Datenträger.|
|/disks/beginGetAccess/action|Dient zum Abrufen des SAS-URIs des Datenträgers für den Blobzugriff.|
|/disks/endGetAccess/action|Dient zum Sperren des SAS-URIs des Datenträgers.|
|/snapshots/read|Dient zum Abrufen der Eigenschaften einer Momentaufnahme.|
|/snapshots/write|Dient zum Erstellen einer neuen Momentaufnahme oder zum Aktualisieren einer bereits vorhandenen.|
|/snapshots/delete|Dient zum Löschen einer Momentaufnahme.|
|/availabilitySets/read|Dient zum Abrufen der Eigenschaften einer Verfügbarkeitsgruppe.|
|/availabilitySets/write|Erstellt eine neue Verfügbarkeitsgruppe oder aktualisiert eine bereits vorhandene.|
|/availabilitySets/delete|Löscht die Verfügbarkeitsgruppe.|
|/availabilitySets/vmSizes/read|Dient zum Auflisten der Größen, die zum Erstellen oder Aktualisieren eines virtuellen Computers in der Verfügbarkeitsgruppe zur Verfügung stehen.|
|/virtualMachines/read|Dient zum Abrufen der Eigenschaften eines virtuellen Computers.|
|/virtualMachines/write|Erstellt einen neuen virtuellen Computer oder aktualisiert einen vorhandenen virtuellen Computer.|
|/virtualMachines/delete|Löscht den virtuellen Computer.|
|/virtualMachines/start/action|Startet den virtuellen Computer.|
|/virtualMachines/powerOff/action|Schaltet den virtuellen Computer aus. Der virtuelle Computer wird weiterhin in Rechnung gestellt.|
|/virtualMachines/redeploy/action|Stellt den virtuellen Computer erneut bereit.|
|/virtualMachines/restart/action|Startet den virtuellen Computer neu.|
|/virtualMachines/deallocate/action|Schaltet den virtuellen Computer aus und gibt die Computeressourcen frei.|
|/virtualMachines/generalize/action|Legt den Zustand des virtuellen Computers auf „Generalisiert“ fest und bereitet den virtuellen Computer auf die Erfassung vor.|
|/virtualMachines/capture/action|Erfasst den virtuellen Computer, indem die virtuellen Datenträger kopiert werden und eine Vorlage generiert wird, die dann zum Erstellen ähnlicher virtueller Computer verwendet werden kann.|
|/virtualMachines/convertToManagedDisks/action|Konvertiert blobbasierte Datenträger des virtuellen Computers in verwaltete Datenträger.|
|/virtualMachines/vmSizes/read|Listet die verfügbaren Größen auf, auf die der virtuelle Computer aktualisiert werden kann.|
|/virtualMachines/instanceView/read|Ruft den detaillierten Laufzeitstatus des virtuellen Computers und seiner Ressourcen ab.|
|/virtualMachines/extensions/read|Dient zum Abrufen der Eigenschaften einer VM-Erweiterung.|
|/virtualMachines/extensions/write|Erstellt eine neue VM-Erweiterung oder aktualisiert eine bereits vorhandene.|
|/virtualMachines/extensions/delete|Löscht die VM-Erweiterung.|
|/locations/vmSizes/read|Listet die verfügbaren VM-Größen für einen Standort auf.|
|/locations/usages/read|Ruft Dienstlimits und aktuelle Nutzungsmengen für die Computeressourcen des Abonnements an einem Standort ab.|
|/locations/operations/read|Ruft den Status eines asynchronen Vorgangs ab.|

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

| Vorgang | Beschreibung |
|---|---|
|/register/action|Registriert das Abonnement für den Containerregistrierungs-Ressourcenanbieter und ermöglicht die Erstellung von Containerregistrierungen.|
|/checknameavailability/read|Prüft, ob der Registrierungsname gültig ist und noch nicht verwendet wird.|
|/registries/read|Gibt die Liste mit Containerregistrierungen zurück oder ruft die Eigenschaften für die angegebene Containerregistrierung ab.|
|/registries/write|Erstellt eine Containerregistrierung mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags für die angegebene Containerregistrierung.|
|/registries/delete|Löscht eine vorhandene Containerregistrierung.|
|/registries/listCredentials/action|Listet die Anmeldeinformationen für die angegebene Containerregistrierung auf.|
|/registries/regenerateCredential/action|Generiert die Anmeldeinformationen für die angegebene Containerregistrierung neu.|

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

| Vorgang | Beschreibung |
|---|---|
|/containerServices/subscriptions/read|Dient zum Abrufen der angegebenen Containerdienste auf der Grundlage des Abonnements.|
|/containerServices/resourceGroups/read|Dient zum Abrufen der angegebenen Containerdienste auf der Grundlage der Ressourcengruppe.|
|/containerServices/resourceGroups/ContainerServiceName/read|Ruft den angegebenen Containerdienst ab.|
|/containerServices/resourceGroups/ContainerServiceName/write|Legt den angegebenen Containerdienst fest oder aktualisiert ihn.|
|/containerServices/resourceGroups/ContainerServiceName/delete|Löscht den angegebenen Containerdienst.|

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

| Vorgang | Beschreibung |
|---|---|
|/updateCommunicationPreference/action|Dient zum Aktualisieren der Kommunikationseinstellungen.|
|/listCommunicationPreference/action|Dient zum Auflisten der Kommunikationseinstellungen.|
|/applications/read|Lesevorgang|
|/applications/write|Schreibvorgang|
|/applications/write|Schreibvorgang|
|/applications/delete|Löschvorgang|
|/applications/listSecrets/action|Dient zum Auflisten von Geheimnissen.|
|/applications/listSingleSignOnToken/action|Dient zum Lesen von Token für einmaliges Anmelden.|
|/operations/read|Dient zum Lesen von Vorgängen.|

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

| Vorgang | Beschreibung |
|---|---|
|/hubs/read|Dient zum Lesen eines beliebigen Azure Customer Insights-Hubs.|
|/hubs/write|Dient zum Erstellen oder Aktualisieren eines beliebigen Azure Customer Insights-Hubs.|
|/hubs/delete|Dient zum Löschen eines beliebigen Azure Customer Insights-Hubs.|
|/hubs/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für die Ressource ab.|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/hubs/providers/Microsoft.Insights/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/hubs/providers/Microsoft.Insights/logDefinitions/read|Ruft die verfügbaren Protokolle für die Ressource ab.|
|/hubs/authorizationPolicies/read|Dient zum Lesen einer beliebigen SAS-Richtlinie für Azure Customer Insights.|
|/hubs/authorizationPolicies/write|Dient zum Erstellen oder Aktualisieren einer beliebigen SAS-Richtlinie für Azure Customer Insights.|
|/hubs/authorizationPolicies/delete|Dient zum Löschen einer beliebigen SAS-Richtlinie für Azure Customer Insights.|
|/hubs/authorizationPolicies/regeneratePrimaryKey/action|Dient zum erneuten Generieren des primären Schlüssels einer SAS-Richtlinie für Azure Customer Insights.|
|/hubs/authorizationPolicies/regenerateSecondaryKey/action|Dient zum erneuten Generieren des sekundären Schlüssels einer SAS-Richtlinie für Azure Customer Insights.|
|/hubs/profiles/read|Dient zum Lesen eines beliebigen Azure Customer Insights-Profils.|
|/hubs/profiles/write|Dient zum Schreiben eines beliebigen Azure Customer Insights-Profils.|
|/hubs/kpi/read|Dient zum Lesen eines beliebigen Key Performance Indicators für Azure Customer Insights.|
|/hubs/kpi/write|Dient zum Erstellen oder Aktualisieren eines beliebigen Key Performance Indicators für Azure Customer Insights.|
|/hubs/kpi/delete|Dient zum Löschen eines beliebigen Key Performance Indicators für Azure Customer Insights.|
|/hubs/views/read|Dient zum Lesen einer beliebigen Azure Customer Insights-App-Ansicht.|
|/hubs/views/write|Dient zum Erstellen oder Aktualisieren einer beliebigen Azure Customer Insights-App-Ansicht.|
|/hubs/views/delete|Dient zum Löschen einer beliebigen Azure Customer Insights-App-Ansicht.|
|/hubs/interactions/read|Dient zum Lesen einer beliebigen Azure Customer Insights-Interaktion.|
|/hubs/interactions/write|Dient zum Erstellen oder Aktualisieren einer beliebigen Azure Customer Insights-Interaktion.|
|/hubs/roleAssignments/read|Dient zum Lesen einer beliebigen Azure Customer Insights-RBAC-Zuweisung.|
|/hubs/roleAssignments/write|Dient zum Erstellen oder Aktualisieren einer beliebigen Azure Customer Insights-RBAC-Zuweisung.|
|/hubs/roleAssignments/delete|Dient zum Löschen einer beliebigen Azure Customer Insights-RBAC-Zuweisung.|
|/hubs/connectors/read|Dient zum Lesen eines beliebigen Azure Customer Insights-Connectors.|
|/hubs/connectors/write|Dient zum Erstellen oder Aktualisieren eines beliebigen Azure Customer Insights-Connectors.|
|/hubs/connectors/delete|Dient zum Löschen eines beliebigen Azure Customer Insights-Connectors.|
|/hubs/connectors/mappings/read|Dient zum Lesen einer beliebigen Azure Customer Insights-Connectorzuordnung.|
|/hubs/connectors/mappings/write|Dient zum Erstellen oder Aktualisieren einer beliebigen Azure Customer Insights-Connectorzuordnung.|
|/hubs/connectors/mappings/delete|Dient zum Löschen einer beliebigen Azure Customer Insights-Connectorzuordnung.|

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

| Vorgang | Beschreibung |
|---|---|
|/checkNameAvailability/action|Prüft die Verfügbarkeit von Katalognamen für Mandanten.|
|/catalogs/read|Dient zum Abrufen von Eigenschaften für einen Katalog oder für mehrere Kataloge unter einem Abonnement oder einer Ressourcengruppe.|
|/catalogs/write|Erstellt einen Katalog oder aktualisiert die Tags und Eigenschaften für den Katalog.|
|/catalogs/delete|Löscht den Katalog.|

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

| Vorgang | Beschreibung |
|---|---|
|/datafactories/read|Liest Data Factorys.|
|/datafactories/write|Dient zum Erstellen oder Aktualisieren von Data Factorys.|
|/datafactories/delete|Löscht Data Factorys.|
|/datafactories/datapipelines/read|Liest Pipelines.|
|/datafactories/datapipelines/delete|Löscht Pipelines.|
|/datafactories/datapipelines/pause/action|Hält Pipelines an.|
|/datafactories/datapipelines/resume/action|Setzt Pipelines fort.|
|/datafactories/datapipelines/update/action|Aktualisiert Pipelines.|
|/datafactories/datapipelines/write|Dient zum Erstellen oder Aktualisieren von Pipelines.|
|/datafactories/linkedServices/read|Liest verknüpfte Dienste.|
|/datafactories/linkedServices/delete|Löscht verknüpfte Dienste.|
|/datafactories/linkedServices/write|Dient zum Erstellen oder Aktualisieren verknüpfter Dienste.|
|/datafactories/tables/read|Liest Tabellen.|
|/datafactories/tables/delete|Löscht Tabellen.|
|/datafactories/tables/write|Dient zum Erstellen oder Aktualisieren von Tabellen.|

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

| Vorgang | Beschreibung |
|---|---|
|/accounts/read|Dient zum Abrufen von Informationen zum DataLakeAnalytics-Konto.|
|/accounts/write|Dient zum Erstellen oder Aktualisieren des DataLakeAnalytics-Kontos.|
|/accounts/delete|Dient zum Löschen des DataLakeAnalytics-Kontos.|
|/accounts/firewallRules/read|Dient zum Abrufen von Informationen zu einer Firewallregel.|
|/accounts/firewallRules/write|Dient zum Erstellen oder Aktualisieren einer Firewallregel.|
|/accounts/firewallRules/delete|Dient zum Löschen einer Firewallregel.|
|/accounts/storageAccounts/read|Dient zum Abrufen verknüpfter Speicherkonten für das DataLakeAnalytics-Konto.|
|/accounts/storageAccounts/write|Dient zum Verknüpfen eines Speicherkontos mit dem DataLakeAnalytics-Konto.|
|/accounts/storageAccounts/delete|Dient zum Aufheben der Verknüpfung eines Speicherkontos mit dem DataLakeAnalytics-Konto.|
|/accounts/storageAccounts/Containers/read|Dient zum Abrufen von Containern unter dem Speicherkonto.|
|/accounts/storageAccounts/Containers/listSasTokens/action|Dient zum Auflisten von SAS-Token für den Speichercontainer.|
|/accounts/dataLakeStoreAccounts/read|Dient zum Abrufen verknüpfter DataLakeStore-Konten für das DataLakeAnalytics-Konto.|
|/accounts/dataLakeStoreAccounts/write|Dient zum Verknüpfen eines DataLakeStore-Kontos mit dem DataLakeAnalytics-Konto.|
|/accounts/dataLakeStoreAccounts/delete|Dient zum Aufheben der Verknüpfung eines DataLakeStore-Kontos mit dem DataLakeAnalytics-Konto.|

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

| Vorgang | Beschreibung |
|---|---|
|/accounts/read|Dient zum Abrufen von Informationen zu einem vorhandenen DataLakeStore-Konto.|
|/accounts/write|Dient zum Erstellen eines neuen DataLakeStore-Kontos oder zum Aktualisieren eines vorhandenen DataLakeStore-Kontos.|
|/accounts/delete|Dient zum Löschen eines vorhandenen DataLakeStore-Kontos.|
|/accounts/firewallRules/read|Dient zum Abrufen von Informationen zu einer Firewallregel.|
|/accounts/firewallRules/write|Dient zum Erstellen oder Aktualisieren einer Firewallregel.|
|/accounts/firewallRules/delete|Dient zum Löschen einer Firewallregel.|
|/accounts/trustedIdProviders/read|Dient zum Abrufen von Informationen zu einem vertrauenswürdigen Identitätsanbieter.|
|/accounts/trustedIdProviders/write|Dient zum Erstellen oder Aktualisieren eines vertrauenswürdigen Identitätsanbieters.|
|/accounts/trustedIdProviders/delete|Dient zum Löschen eines vertrauenswürdigen Identitätsanbieters.|

## <a name="microsoftdevices"></a>Microsoft.Devices

| Vorgang | Beschreibung |
|---|---|
|/register/action|Dient zum Registrieren des Abonnements für den IotHub-Ressourcenanbieter und ermöglicht die Erstellung von IotHub-Ressourcen.|
|/checkNameAvailability/Action|Dient zum Prüfen, ob ein IotHub-Name verfügbar ist.|
|/usages/Read|Dient zum Abrufen von Details zur Abonnementnutzung für diesen Anbieter.|
|/operations/Read|Dient zum Abrufen aller ResourceProvider-Vorgänge.|
|/iotHubs/Read|Ruft die IotHub-Ressourcen ab.|
|/iotHubs/Write|Dient zum Erstellen oder Aktualisieren von IotHub-Ressourcen.|
|/iotHubs/Delete|Dient zum Löschen von IotHub-Ressourcen.|
|/iotHubs/listkeys/Action|Dient zum Abrufen aller IotHub-Schlüssel.|
|/iotHubs/exportDevices/Action|Dient zum Exportieren von Geräten.|
|/iotHubs/importDevices/Action|Dient zum Importieren von Geräten.|
|/IotHubs/metricDefinitions/read|Ruft die verfügbaren Metriken für den IotHub-Dienst ab.|
|/iotHubs/iotHubKeys/listkeys/Action|Dient zum Abrufen des IotHub-Schlüssels für den angegebenen Namen.|
|/iotHubs/iotHubStats/Read|Dient zum Abrufen der IotHub-Statistik.|
|/iotHubs/quotaMetrics/Read|Dient zum Abrufen von Kontingentmetriken.|
|/iotHubs/eventHubEndpoints/consumerGroups/Write|Dient zum Erstellen von EventHub-Consumergruppen.|
|/iotHubs/eventHubEndpoints/consumerGroups/Read|Dient zum Abrufen von EventHub-Consumergruppen.|
|/iotHubs/eventHubEndpoints/consumerGroups/Delete|Dient zum Löschen von EventHub-Consumergruppen.|
|/iotHubs/routing/routes/$testall/Action|Dient zum Testen einer Nachricht mit allen vorhandenen Routen.|
|/iotHubs/routing/routes/$testnew/Action|Dient zum Testen einer Nachricht mit einer angegebenen Testroute.|
|/IotHubs/diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/IotHubs/diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/iotHubs/skus/Read|Dient zum Abrufen gültiger IotHub-SKUs.|
|/iotHubs/jobs/Read|Dient zum Abrufen von Auftragsdetails, die für eine bestimmte IotHub-Instanz übermittelt wurden.|
|/iotHubs/routingEndpointsHealth/Read|Ruft die Integrität aller Routingendpunkte für eine IotHub-Instanz ab.|

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

| Vorgang | Beschreibung |
|---|---|
|/Subscription/register/action|Registriert das Abonnement.|
|/labs/delete|Dient zum Löschen von Labs.|
|/labs/read|Dient zum Lesen von Labs.|
|/labs/write|Dient zum Hinzufügen oder Ändern von Labs.|
|/labs/ListVhds/action|Dient zum Auflisten von Datenträgerimages, die für die Erstellung benutzerdefinierter Images verfügbar sind.|
|/labs/GenerateUploadUri/action|Dient zum Generieren eines URI für das Hochladen benutzerdefinierter Datenträgerimages in ein Lab.|
|/labs/CreateEnvironment/action|Dient zum Erstellen von virtuellen Computern in einem Lab.|
|/labs/ClaimAnyVm/action|Dient zum Anfordern eines beliebigen anforderbaren virtuellen Computers im Lab.|
|/labs/ExportResourceUsage/action|Exportiert die Labressourcenverwendung in ein Speicherkonto.|
|/labs/users/delete|Dient zum Löschen von Benutzerprofilen.|
|/labs/users/read|Dient zum Lesen von Benutzerprofilen.|
|/labs/users/write|Dient zum Hinzufügen oder Ändern von Benutzerprofilen.|
|/labs/users/secrets/delete|Dient zum Löschen von Geheimnissen.|
|/labs/users/secrets/read|Dient zum Lesen von Geheimnissen.|
|/labs/users/secrets/write|Dient zum Hinzufügen oder Ändern von Geheimnissen.|
|/labs/users/environments/delete|Dient zum Löschen von Umgebungen.|
|/labs/users/environments/read|Dient zum Lesen von Umgebungen.|
|/labs/users/environments/write|Dient zum Hinzufügen oder Ändern von Umgebungen.|
|/labs/users/disks/delete|Dient zum Löschen von Datenträgern.|
|/labs/users/disks/read|Dient zum Lesen von Datenträgern.|
|/labs/users/disks/write|Dient zum Hinzufügen oder Ändern von Datenträgern.|
|/labs/users/disks/Attach/action|Dient zum Anfügen des Datenträgers an den virtuellen Computer sowie zum Erstellen der Datenträgerlease für den virtuellen Computer.|
|/labs/users/disks/Detach/action|Dient zum Trennen des an den virtuellen Computer angefügten Datenträgers sowie zum Beenden der Datenträgerlease für den virtuellen Computer.|
|/labs/customImages/delete|Dient zum Löschen benutzerdefinierter Images.|
|/labs/customImages/read|Dient zum Lesen benutzerdefinierter Images.|
|/labs/customImages/write|Dient zum Hinzufügen oder Ändern benutzerdefinierter Images.|
|/labs/serviceRunners/delete|Dient zum Löschen von Dienstausführern.|
|/labs/serviceRunners/read|Dient zum Lesen von Dienstausführern.|
|/labs/serviceRunners/write|Dient zum Hinzufügen oder Ändern von Dienstausführern.|
|/labs/artifactSources/delete|Dient zum Löschen von Artefaktquellen.|
|/labs/artifactSources/read|Dient zum Lesen von Artefaktquellen.|
|/labs/artifactSources/write|Dient zum Hinzufügen oder Ändern von Artefaktquellen.|
|/labs/artifactSources/artifacts/read|Dient zum Lesen von Artefakten.|
|/labs/artifactSources/artifacts/GenerateArmTemplate/action|Generiert eine ARM-Vorlage für das angegebene Artefakt, lädt die erforderlichen Dateien in ein Speicherkonto hoch und überprüft das generierte Artefakt.|
|/labs/artifactSources/armTemplates/read|Dient zum Lesen von Azure Resource Manager-Vorlagen.|
|/labs/costs/read|Dient zum Lesen von Kosten.|
|/labs/costs/write|Dient zum Hinzufügen oder Ändern von Kosten.|
|/labs/virtualNetworks/delete|Dient zum Löschen virtueller Netzwerke.|
|/labs/virtualNetworks/read|Dient zum Lesen virtueller Netzwerke.|
|/labs/virtualNetworks/write|Dient zum Hinzufügen oder Ändern virtueller Netzwerke.|
|/labs/formulas/delete|Dient zum Löschen von Formeln.|
|/labs/formulas/read|Dient zum Lesen von Formeln.|
|/labs/formulas/write|Dient zum Hinzufügen oder Ändern von Formeln.|
|/labs/schedules/delete|Dient zum Löschen von Zeitplänen.|
|/labs/schedules/read|Dient zum Lesen von Zeitplänen.|
|/labs/schedules/write|Dient zum Hinzufügen oder Ändern von Zeitplänen.|
|/labs/schedules/Execute/action|Dient zum Ausführen eines Zeitplans.|
|/labs/schedules/ListApplicable/action|Listet alle anwendbaren Zeitpläne auf.|
|/labs/galleryImages/read|Dient zum Lesen von Katalogimages.|
|/labs/policySets/EvaluatePolicies/action|Wertet Labrichtlinien aus.|
|/labs/policySets/policies/delete|Dient zum Löschen von Richtlinien.|
|/labs/policySets/policies/read|Dient zum Lesen von Richtlinien.|
|/labs/policySets/policies/write|Dient zum Hinzufügen oder Ändern von Richtlinien.|
|/labs/virtualMachines/delete|Dient zum Löschen virtueller Computer.|
|/labs/virtualMachines/read|Dient zum Lesen virtueller Computer.|
|/labs/virtualMachines/write|Dient zum Hinzufügen oder Ändern virtueller Computer.|
|/labs/virtualMachines/Start/action|Dient zum Starten eines virtuellen Computers.|
|/labs/virtualMachines/Stop/action|Dient zum Beenden eines virtuellen Computers.|
|/labs/virtualMachines/ApplyArtifacts/action|Dient zum Anwenden von Artefakten auf virtuelle Computer.|
|/labs/virtualMachines/AddDataDisk/action|Dient zum Anfügen eines neuen oder vorhandenen Datenträgers an virtuelle Computer.|
|/labs/virtualMachines/DetachDataDisk/action|Dient zum Trennen des angegebenen Datenträgers vom virtuellen Computer.|
|/labs/virtualMachines/Claim/action|Dient dazu, einen vorhandenen virtuellen Computer in Besitz zu nehmen.|
|/labs/virtualMachines/ListApplicableSchedules/action|Listet alle anwendbaren Zeitpläne auf.|
|/labs/virtualMachines/schedules/delete|Dient zum Löschen von Zeitplänen.|
|/labs/virtualMachines/schedules/read|Dient zum Lesen von Zeitplänen.|
|/labs/virtualMachines/schedules/write|Dient zum Hinzufügen oder Ändern von Zeitplänen.|
|/labs/virtualMachines/schedules/Execute/action|Dient zum Ausführen eines Zeitplans.|
|/labs/notificationChannels/delete|Dient zum Löschen von Benachrichtigungskanälen.|
|/labs/notificationChannels/read|Dient zum Lesen von Benachrichtigungskanälen.|
|/labs/notificationChannels/write|Dient zum Hinzufügen oder Ändern von Benachrichtigungskanälen.|
|/labs/notificationChannels/Notify/action|Dient zum Senden einer Benachrichtigung an einen angegebenen Kanal.|
|/schedules/delete|Dient zum Löschen von Zeitplänen.|
|/schedules/read|Dient zum Lesen von Zeitplänen.|
|/schedules/write|Dient zum Hinzufügen oder Ändern von Zeitplänen.|
|/schedules/Execute/action|Dient zum Ausführen eines Zeitplans.|
|/schedules/Retarget/action|Aktualisiert die Zielressourcen-ID eines Zeitplans.|
|/locations/operations/read|Dient zum Lesen von Vorgängen.|

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

| Vorgang | Beschreibung |
|---|---|
|/databaseAccountNames/read|Prüft die Verfügbarkeit des Namens.|
|/databaseAccounts/read|Liest ein Datenbankkonto.|
|/databaseAccounts/write|Dient zum Aktualisieren von Datenbankkonten.|
|/databaseAccounts/listKeys/action|Dient zum Auflisten von Schlüsseln eines Datenbankkontos.|
|/databaseAccounts/regenerateKey/action|Dient zum Rotieren von Schlüsseln eines Datenbankkontos.|
|/databaseAccounts/listConnectionStrings/action|Dient zum Abrufen der Verbindungszeichenfolgen für ein Datenbankkonto.|
|/databaseAccounts/changeResourceGroup/action|Dient zum Ändern der Ressourcengruppe eines Datenbankkontos.|
|/databaseAccounts/failoverPriorityChange/action|Dient zum Ändern der Failoverprioritäten von Regionen eines Datenbankkontos. Wird für manuelle Failovervorgänge verwendet.|
|/databaseAccounts/delete|Löscht die Datenbankkonten.|
|/databaseAccounts/metricDefinitions/read|Liest die Metrikdefinitionen für Datenbankkonten.|
|/databaseAccounts/metrics/read|Liest die Datenbankkontometriken.|
|/databaseAccounts/usages/read|Liest Informationen zur Datenbankkontoverwendung.|
|/databaseAccounts/databases/collections/metricDefinitions/read|Liest die Sammlungsmetrikdefinitionen.|
|/databaseAccounts/databases/collections/metrics/read|Liest die Sammlungsmetriken.|
|/databaseAccounts/databases/collections/usages/read|Liest Informationen zur Sammlungsverwendung.|
|/databaseAccounts/databases/metricDefinitions/read|Liest die Datenbankmetrikdefinitionen.|
|/databaseAccounts/databases/metrics/read|Liest die Datenbankmetriken.|
|/databaseAccounts/databases/usages/read|Liest Informationen zur Datenbankverwendung.|
|/databaseAccounts/readonlykeys/read|Liest die schreibgeschützten Schlüssel für Datenbankkonten.|

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

| Vorgang | Beschreibung |
|---|---|
|/generateSsoRequest/Action|Dient zum Generieren einer Anforderung für die Anmeldung beim Domänensteuerungszentrum.|
|/validateDomainRegistrationInformation/Action|Dient zum Überprüfen eines Domänenerwerbsobjekts, ohne es zu übermitteln.|
|/checkDomainAvailability/Action|Dient zum Prüfen, ob eine Domäne erworben werden kann.|
|/listDomainRecommendations/Action|Dient zum Abrufen der Liste mit Domänenempfehlungen auf der Grundlage von Schlüsselwörtern.|
|/register/action|Dient zum Registrieren des Microsoft-Domänenressourcenanbieters für das Abonnement.|
|/domains/Read|Dient zum Abrufen der Domänenliste.|
|/domains/Write|Dient zum Hinzufügen einer neuen Domäne oder zum Aktualisieren einer bereits vorhandenen.|
|/domains/Delete|Dient zum Löschen einer vorhandenen Domäne.|
|/domains/operationresults/Read|Dient zum Abrufen eines Domänenvorgangs.|

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

| Vorgang | Beschreibung |
|---|---|
|/lcsprojects/read|Dient zum Anzeigen von Microsoft Dynamics Lifecycle Services-Projekten, die zu einem Benutzer gehören.|
|/lcsprojects/write|Dient zum Erstellen oder Aktualisieren von Microsoft Dynamics Lifecycle Services-Projekten, die zum Benutzer gehören. Nur die Eigenschaften für Name und Beschreibung können aktualisiert werden. Abonnement und Standort für das Projekt können nach der Erstellung nicht mehr aktualisiert werden.|
|/lcsprojects/delete|Dient zum Löschen von Microsoft Dynamics Lifecycle Services-Projekten, die zum Benutzer gehören.|
|/lcsprojects/clouddeployments/read|Dient zum Anzeigen von Microsoft Dynamics AX 2012 R3 Evaluation-Bereitstellungen in einem Microsoft Dynamics Lifecycle Services-Projekt, die zu einem Benutzer gehören.|
|/lcsprojects/clouddeployments/write|Dient zum Erstellen von Microsoft Dynamics AX 2012 R3 Evaluation-Bereitstellungen in einem Microsoft Dynamics Lifecycle Services-Projekt, die zu einem Benutzer gehören. Bereitstellungen können über das Azure-Verwaltungsportal verwaltet werden.|
|/lcsprojects/connectors/read|Dient zum Lesen von Connectors, die zu einem Microsoft Dynamics Lifecycle Services-Projekt gehören.|
|/lcsprojects/connectors/write|Dient zum Erstellen und Aktualisieren von Connectors, die zu einem Microsoft Dynamics Lifecycle Services-Projekt gehören.|

## <a name="microsofteventhub"></a>Microsoft.EventHub

| Vorgang | Beschreibung |
|---|---|
|/checkNameAvailability/action|Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement.|
|/register/action|Registriert das Abonnement für den EventHub-Ressourcenanbieter und ermöglicht die Erstellung von EventHub-Ressourcen.|
|/namespaces/write|Dient zum Erstellen einer Namespaceressource und zum Aktualisieren ihrer Eigenschaften. Die Eigenschaften für Tags und Status des Namespace können aktualisiert werden.|
|/namespaces/read|Dient zum Abrufen der Liste mit Beschreibungen für Namespaceressourcen.|
|/namespaces/Delete|Dient zum Löschen von Namespaceressourcen.|
|/namespaces/metricDefinitions/read|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacemetriken.|
|/namespaces/authorizationRules/read|Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln.|
|/namespaces/authorizationRules/write|Dient zum Erstellen von Autorisierungsregeln auf Namespaceebene und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel.|
|/namespaces/authorizationRules/delete|Dient zum Löschen von Namespace-Autorisierungsregeln. Die standardmäßige Namespace-Autorisierungsregel kann nicht gelöscht werden. |
|/namespaces/authorizationRules/listkeys/action|Dient zum Abrufen der Verbindungszeichenfolge für den Namespace.|
|/namespaces/authorizationRules/regenerateKeys/action|Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource.|
|/namespaces/eventhubs/write|Dient zum Erstellen oder Aktualisieren von EventHub-Eigenschaften.|
|/namespaces/eventhubs/read|Dient zum Abrufen einer Liste mit EventHub-Ressourcenbeschreibungen.|
|/namespaces/eventhubs/Delete|Vorgang zum Löschen von EventHub-Ressourcen.|
|/namespaces/eventHubs/consumergroups/write|Dient zum Erstellen oder Aktualisieren von ConsumerGroup-Eigenschaften.|
|/namespaces/eventHubs/consumergroups/read|Dient zum Abrufen einer Liste mit ConsumerGroup-Ressourcenbeschreibungen.|
|/namespaces/eventHubs/consumergroups/Delete|Vorgang zum Löschen von ConsumerGroup-Ressourcen.|
|/namespaces/eventhubs/authorizationRules/read| Dient zum Abrufen der Liste mit EventHub-Autorisierungsregeln.|
|/namespaces/eventhubs/authorizationRules/write|Dient zum Erstellen von EventHub-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel.|
|/namespaces/eventhubs/authorizationRules/delete|Vorgang zum Löschen von EventHub-Autorisierungsregeln.|
|/namespaces/eventhubs/authorizationRules/listkeys/action|Dient zum Abrufen der EventHub-Verbindungszeichenfolge.|
|/namespaces/eventhubs/authorizationRules/regenerateKeys/action|Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource.|
|/namespaces/diagnosticSettings/read|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen.|
|/namespaces/diagnosticSettings/write|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen.|
|/namespaces/logDefinitions/read|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespaceprotokolle.|

## <a name="microsoftfeatures"></a>Microsoft.Features

| Vorgang | Beschreibung |
|---|---|
|/providers/features/read|Ruft das Feature eines Abonnements in einem angegebenen Ressourcenanbieter ab.|
|/providers/features/register/action|Registriert das Feature für ein Abonnement in einem angegebenen Ressourcenanbieter.|
|/features/read|Ruft die Features eines Abonnements ab.|

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

| Vorgang | Beschreibung |
|---|---|
|/clusters/write|Dient zum Erstellen oder Aktualisieren von HDInsight-Clustern.|
|/clusters/read|Dient zum Abrufen von Details zu HDInsight-Clustern.|
|/clusters/delete|Dient zum Löschen eines HDInsight-Clusters.|
|/clusters/changerdpsetting/action|Dient zum Ändern der RDP-Einstellung für HDInsight-Cluster.|
|/clusters/configurations/action|Dient zum Aktualisieren der Konfiguration von HDInsight-Clustern.|
|/clusters/configurations/read|Dient zum Abrufen der Konfigurationen von HDInsight-Clustern.|
|/clusters/roles/resize/action|Dient zum Anpassen der Größe eines HDInsight-Clusters.|
|/locations/capabilities/read|Dient zum Abrufen von Abonnementfunktionen.|
|/locations/checkNameAvailability/read|Dient zum Prüfen der Verfügbarkeit des Namens.|

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

| Vorgang | Beschreibung |
|---|---|
|/register/action|Registriert das Abonnement für den Import/Export-Ressourcenanbieter und ermöglicht die Erstellung von Import/Exportaufträgen.|
|/jobs/write|Erstellt einen Auftrag mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags für den angegebenen Auftrag.|
|/jobs/read|Ruft die Eigenschaften für den angegebenen Auftrag ab oder gibt die Auftragsliste zurück.|
|/jobs/listBitLockerKeys/action|Ruft die BitLocker-Schlüssel für den angegebenen Auftrag ab.|
|/jobs/delete|Löscht einen vorhandenen Auftrag.|
|/locations/read|Ruft die Eigenschaften für den angegebenen Standort ab oder gibt die Standortliste zurück.|

## <a name="microsoftinsights"></a>Microsoft.Insights

| Vorgang | Beschreibung |
|---|---|
|/Register/Action|Dient zum Registrieren des Microsoft Insights-Anbieters.|
|/AlertRules/Write|Dient zum Schreiben in eine Warnungsregelkonfiguration.|
|/AlertRules/Delete|Dient zum Löschen einer Warnungsregelkonfiguration.|
|/AlertRules/Read|Dient zum Lesen einer Warnungsregelkonfiguration.|
|/AlertRules/Activated/Action|Warnungsregel aktiviert|
|/AlertRules/Resolved/Action|Warnungsregel aufgelöst|
|/AlertRules/Throttled/Action|Warnungsregel gedrosselt|
|/AlertRules/Incidents/Read|Dient zum Lesen einer Warnungsregel-Incidentkonfiguration.|
|/MetricDefinitions/Read|Dient zum Lesen von Metrikdefinitionen.|
|/eventtypes/values/Read|Dient zum Lesen von Verwaltungsereignistyp-Werten.|
|/eventtypes/digestevents/Read|Dient zum Lesen des Verwaltungsereignistyp-Digests.|
|/Metrics/Read|Dient zum Lesen von Metriken.|
|/LogProfiles/Write|Dient zum Schreiben in eine Protokollprofilkonfiguration.|
|/LogProfiles/Delete|Dient zum Löschen von Protokollprofilkonfigurationen.|
|/LogProfiles/Read|Dient zum Lesen von Protokollprofilen.|
|/AutoscaleSettings/Write|Dient zum Schreiben in eine Autoskalierungseinstellungskonfiguration.|
|/AutoscaleSettings/Delete|Dient zum Löschen einer Autoskalierungseinstellungskonfiguration.|
|/AutoscaleSettings/Read|Dient zum Lesen einer Autoskalierungseinstellungskonfiguration.|
|/AutoscaleSettings/Scaleup/Action|Automatisches zentrales Hochskalieren|
|/AutoscaleSettings/Scaledown/Action|Automatisches zentrales Herunterskalieren|
|/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read|Dient zum Lesen von Metrikdefinitionen.|
|/ActivityLogAlerts/Activated/Action|Aktivitätsprotokollwarnung ausgelöst|
|/DiagnosticSettings/Write|Dient zum Schreiben in die Diagnoseeinstellungskonfiguration.|
|/DiagnosticSettings/Delete|Dient zum Löschen der Diagnoseeinstellungskonfiguration.|
|/DiagnosticSettings/Read|Dient zum Lesen einer Diagnoseeinstellungskonfiguration.|
|/LogDefinitions/Read|Dient zum Lesen von Protokolldefinitionen.|
|/ExtendedDiagnosticSettings/Write|Dient zum Schreiben in die erweiterte Diagnoseeinstellungskonfiguration.|
|/ExtendedDiagnosticSettings/Delete|Dient zum Löschen der erweiterten Diagnoseeinstellungskonfiguration.|
|/ExtendedDiagnosticSettings/Read|Dient zum Lesen einer erweiterten Diagnoseeinstellungskonfiguration.|

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

| Vorgang | Beschreibung |
|---|---|
|/register/action|Registriert ein Abonnement.|
|/checkNameAvailability/read|Prüft, ob ein Schlüsseltresorname gültig ist und noch nicht verwendet wird.|
|/vaults/read|Dient zum Anzeigen der Eigenschaften eines Schlüsseltresors.|
|/vaults/write|Dient zum Erstellen eines neuen Schlüsseltresors oder zum Aktualisieren der Eigenschaften einer vorhandenen Schlüsseltresors.|
|/vaults/delete|Dient zum Löschen eines Schlüsseltresors.|
|/vaults/deploy/action|Ermöglicht den Zugriff auf Geheimnisse in einem Schlüsseltresor beim Bereitstellen von Azure-Ressourcen.|
|/vaults/secrets/read|Dient zum Anzeigen der Eigenschaften (aber nicht des Werts) eines Geheimnisses.|
|/vaults/secrets/write|Dient zum Erstellen eines neuen Geheimnisses oder zum Aktualisieren des Werts eines vorhandenen Geheimnisses.|
|/vaults/accessPolicies/write|Dient zum Aktualisieren einer vorhandenen Zugriffsrichtlinie durch Zusammenführen oder Ersetzen oder zum Hinzufügen einer neuen Zugriffsrichtlinie zu einem Tresor.|
|/deletedVaults/read|Dient zum Anzeigen der Eigenschaften vorläufig gelöschter Schlüsseltresore.|
|/locations/operationResults/read|Dient zum Überprüfen des Ergebnisses eines Vorgangs mit langer Ausführungszeit.|
|/locations/deletedVaults/read|Dient zum Anzeigen der Eigenschaften eines vorläufig gelöschten Schlüsseltresors.|
|/locations/deletedVaults/purge/action|Dient zum endgültigen Löschen eines vorläufig gelöschten Schlüsseltresors.|

## <a name="microsoftlogic"></a>Microsoft.Logic

| Vorgang | Beschreibung |
|---|---|
|/workflows/read|Liest den Workflow.|
|/workflows/write|Erstellt oder aktualisiert den Workflow.|
|/workflows/delete|Löscht den Workflow.|
|/workflows/run/action|Startet eine Ausführung des Workflows.|
|/workflows/disable/action|Deaktiviert den Workflow.|
|/workflows/enable/action|Aktiviert den Workflow.|
|/workflows/validate/action|Überprüft den Workflow.|
|/workflows/move/action|Verschiebt den Workflow von der vorhandenen Abonnement-ID, der Ressourcengruppe und/oder dem Namen in eine andere Abonnement-ID, eine andere Ressourcengruppe und/oder einen anderen Namen.|
|/workflows/listSwagger/action|Ruft die Swagger-Definitionen für den Workflow ab.|
|/workflows/regenerateAccessKey/action|Generiert die Zugriffsschlüsselgeheimnisse neu.|
|/workflows/listCallbackUrl/action|Ruft die Rückruf-URL für Workflows ab.|
|/workflows/versions/read|Liest die Workflowversion.|
|/workflows/versions/triggers/listCallbackUrl/action|Ruft die Rückruf-URL für Trigger ab.|
|/workflows/runs/read|Liest die Workflowausführung.|
|/workflows/runs/cancel/action|Bricht die Ausführung eines Workflows ab.|
|/workflows/runs/actions/read|Liest die Workflowausführungsaktion.|
|/workflows/runs/operations/read|Liest den Status von Workflowausführungsaktionen.|
|/workflows/triggers/read|Liest den Trigger.|
|/workflows/triggers/run/action|Führt den Trigger aus.|
|/workflows/triggers/listCallbackUrl/action|Ruft die Rückruf-URL für Trigger ab.|
|/workflows/triggers/histories/read|Liest die Triggerverläufe.|
|/workflows/triggers/histories/resubmit/action|Übermittelt den Workflowtrigger erneut.|
|/workflows/accessKeys/read|Liest den Zugriffsschlüssel.|
|/workflows/accessKeys/write|Erstellt oder aktualisiert den Zugriffsschlüssel.|
|/workflows/accessKeys/delete|Löscht den Zugriffsschlüssel.|
|/workflows/accessKeys/list/action|Listet die Zugriffsschlüsselgeheimnisse auf.|
|/workflows/accessKeys/regenerate/action|Generiert die Zugriffsschlüsselgeheimnisse neu.|
|/locations/workflows/validate/action|Überprüft den Workflow.|

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

| Vorgang | Beschreibung |
|---|---|
|/register/action|Registriert das Abonnement für den Machine Learning-Webdienst-Ressourcenanbieter und ermöglicht die Erstellung von Webdiensten.|
|/webServices/action|Dient zum Erstellen regionaler Webdiensteigenschaften für unterstützte Regionen.|
|/commitmentPlans/read|Dient zum Lesen beliebiger Machine Learning-Vertragspläne.|
|/commitmentPlans/write|Dient zum Erstellen oder Aktualisieren beliebiger Machine Learning-Vertragspläne.|
|/commitmentPlans/delete|Dient zum Löschen beliebiger Machine Learning-Vertragspläne.|
|/commitmentPlans/join/action|Dient zum Beitreten zu beliebigen Machine Learning-Vertragsplänen.|
|/commitmentPlans/commitmentAssociations/read|Dient zum Lesen beliebiger Machine Learning-Vertragsplanzuordnungen.|
|/commitmentPlans/commitmentAssociations/move/action|Dient zum Verschieben beliebiger Machine Learning-Vertragsplanzuordnungen.|
|/Workspaces/read|Dient zum Lesen beliebiger Machine Learning-Arbeitsbereiche.|
|/Workspaces/write|Dient zum Erstellen oder Aktualisieren beliebiger Machine Learning-Arbeitsbereiche.|
|/Workspaces/delete|Dient zum Löschen beliebiger Machine Learning-Arbeitsbereiche.|
|/Workspaces/listworkspacekeys/action|Dient zum Auflisten von Schlüsseln für einen Machine Learning-Arbeitsbereich.|
|/Workspaces/resyncstoragekeys/action|Dient zum erneuten Synchronisieren von Schlüsseln eines für einen Machine Learning-Arbeitsbereich konfigurierten Speicherkontos.|
|/webServices/read|Dient zum Lesen beliebiger Machine Learning-Webdienste.|
|/webServices/write|Dient zum Erstellen oder Aktualisieren beliebiger Machine Learning-Webdienste.|
|/webServices/delete|Dient zum Löschen beliebiger Machine Learning-Webdienste.|

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

| Vorgang | Beschreibung |
|---|---|
|/agreements/offers/plans/read|Dient zum Zurückgeben einer Vereinbarung für ein angegebenes Marketplace-Element.|
|/agreements/offers/plans/sign/action|Dient zum Unterzeichnen einer Vereinbarung für ein angegebenes Marketplace-Element.|
|/agreements/offers/plans/cancel/action|Dient zum Stornieren einer Vereinbarung für ein angegebenes Marketplace-Element.|

## <a name="microsoftmedia"></a>Microsoft.Media

| Vorgang | Beschreibung |
|---|---|
|/mediaservices/read||
|/mediaservices/write||
|/mediaservices/delete||
|/mediaservices/regenerateKey/action||
|/mediaservices/listKeys/action||
|/mediaservices/syncStorageKeys/action||

## <a name="microsoftnetwork"></a>Microsoft.Network

| Vorgang | Beschreibung |
|---|---|
|/register/action|Registriert das Abonnement.|
|/unregister/action|Hebt die Registrierung des Abonnements auf.|
|/checkTrafficManagerNameAvailability/action|Prüft die Verfügbarkeit eines relativen Traffic Manager-DNS-Namens.|
|/dnszones/read|Dient zum Abrufen der DNS-Zone im JSON-Format. Zu den Zoneneigenschaften zählen „tags“, „etag“, „numberOfRecordSets“ und „maxNumberOfRecordSets“. Die in der Zone enthaltenen Ressourceneintragssätze werden durch diesen Befehl nicht abgerufen.|
|/dnszones/write|Dient zum Erstellen oder Aktualisieren einer DNS-Zone innerhalb einer Ressourcengruppe.  Wird zum Aktualisieren der Tags für eine DNS-Zonenressource verwendet. Mit diesem Befehl können keine Ressourceneintragssätze innerhalb der Zone erstellt oder aktualisiert werden.|
|/dnszones/delete|Dient zum Löschen der DNS-Zone im JSON-Format. Zu den Zoneneigenschaften zählen „tags“, „etag“, „numberOfRecordSets“ und „maxNumberOfRecordSets“.|
|/dnszones/MX/read|Dient zum Abrufen des Ressourceneintragssatzes vom Typ „MX“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag.|
|/dnszones/MX/write|Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „MX“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt.|
|/dnszones/MX/delete|Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „MX“ aus einer DNS-Zone.|
|/dnszones/NS/read|Ruft DNS-Ressourceneintragssätze vom Typ „NS“ ab.|
|/dnszones/NS/write|Erstellt oder aktualisiert DNS-Ressourceneintragssätze vom Typ „NS“.|
|/dnszones/NS/delete|Löscht den DNS-Ressourceneintragssatz vom Typ „NS“.|
|/dnszones/AAAA/read|Dient zum Abrufen des Ressourceneintragssatzes vom Typ „AAAA“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag.|
|/dnszones/AAAA/write|Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „AAAA“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt.|
|/dnszones/AAAA/delete|Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „AAAA“ aus einer DNS-Zone.|
|/dnszones/CNAME/read|Dient zum Abrufen des Ressourceneintragssatzes vom Typ „CNAME“ im JSON-Format. Der Ressourceneintragssatz enthält Gültigkeitsdauer, Tags und ETag.|
|/dnszones/CNAME/write|Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „CNAME“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt.|
|/dnszones/CNAME/delete|Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „CNAME“ aus einer DNS-Zone.|
|/dnszones/SOA/read|Ruft DNS-Ressourceneintragssätze vom Typ „SOA“ ab.|
|/dnszones/SOA/write|Erstellt oder aktualisiert DNS-Ressourceneintragssätze vom Typ „SOA“.|
|/dnszones/SRV/read|Dient zum Abrufen des Ressourceneintragssatzes vom Typ „SRV“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag.|
|/dnszones/SRV/write|Dient zum Erstellen oder Aktualisieren von Ressourceneintragssätzen vom Typ „SRV“.|
|/dnszones/SRV/delete|Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „SRV“ aus einer DNS-Zone.|
|/dnszones/PTR/read|Dient zum Abrufen des Ressourceneintragssatzes vom Typ „PTR“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag.|
|/dnszones/PTR/write|Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „PTR“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt.|
|/dnszones/PTR/delete|Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „PTR“ aus einer DNS-Zone.|
|/dnszones/A/read|Dient zum Abrufen des Ressourceneintragssatzes vom Typ „A“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag.|
|/dnszones/A/write|Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „A“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt.|
|/dnszones/A/delete|Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „A“ aus einer DNS-Zone.|
|/dnszones/TXT/read|Dient zum Abrufen des Ressourceneintragssatzes vom Typ „TXT“ im JSON-Format. Der Ressourceneintragssatz enthält eine Liste mit Einträgen sowie Gültigkeitsdauer, Tags und ETag.|
|/dnszones/TXT/write|Dient zum Erstellen oder Aktualisieren eines Ressourceneintragssatzes vom Typ „TXT“ innerhalb einer DNS-Zone. Die aktuellen Ressourceneinträge des Ressourceneintragssatzes werden durch die angegebenen Einträge ersetzt.|
|/dnszones/TXT/delete|Dient zum Entfernen des Ressourceneintragssatzes mit einem bestimmten Namen und dem Typ „TXT“ aus einer DNS-Zone.|
|/dnszones/recordsets/read|Ruft DNS-Ressourceneintragssätze typenübergreifend ab.|
|/networkInterfaces/read|Ruft eine Netzwerkschnittstellendefinition ab. |
|/networkInterfaces/write|Erstellt eine Netzwerkschnittstelle oder aktualisiert eine vorhandene Netzwerkschnittstelle. |
|/networkInterfaces/join/action|Verknüpft einen virtuellen Computer mit einer Netzwerkschnittstelle.|
|/networkInterfaces/delete|Löscht eine Netzwerkschnittstelle.|
|/networkInterfaces/effectiveRouteTable/action|Dient zum Abrufen der Routingtabelle, die für die Netzwerkschnittstelle des virtuellen Computers konfiguriert ist.|
|/networkInterfaces/effectiveNetworkSecurityGroups/action|Dient zum Abrufen der Netzwerksicherheitsgruppen, die für die Netzwerkschnittstelle des virtuellen Computers konfiguriert sind.|
|/networkInterfaces/loadBalancers/read|Ruft alle Lastenausgleichsmodule ab, denen die Netzwerkschnittstelle angehört.|
|/networkInterfaces/ipconfigurations/read|Ruft eine IP-Konfigurationsdefinition für Netzwerkschnittstellen ab. |
|/publicIPAddresses/read|Ruft eine Definition für eine öffentliche IP-Adresse ab.|
|/publicIPAddresses/write|Erstellt eine öffentliche IP-Adresse oder aktualisiert eine vorhandene öffentliche IP-Adresse. |
|/publicIPAddresses/delete|Löscht eine öffentliche IP-Adresse.|
|/publicIPAddresses/join/action|Verknüpft eine öffentliche IP-Adresse.|
|/routeFilters/read|Ruft eine Routenfilterdefinition ab.|
|/routeFilters/join/action|Verknüpft einen Routenfilter.|
|/routeFilters/delete|Löscht eine Routenfilterdefinition.|
|/routeFilters/write|Erstellt einen Routenfilter oder aktualisiert einen vorhandenen Routenfilter.|
|/routeFilters/rules/read|Ruft eine Routenfilterregel-Definition ab.|
|/routeFilters/rules/write|Erstellt eine Routenfilterregel oder aktualisiert eine vorhandene Routenfilterregel.|
|/routeFilters/rules/delete|Löscht eine Routenfilterregel-Definition.|
|/networkWatchers/read|Dient zum Abrufen der Network Watcher-Definition.|
|/networkWatchers/write|Erstellt eine Network Watcher-Instanz oder aktualisiert eine vorhandene Network Watcher-Instanz.|
|/networkWatchers/delete|Löscht eine Network Watcher-Instanz.|
|/networkWatchers/configureFlowLog/action|Konfiguriert die Datenflussprotokollierung für eine Zielressource.|
|/networkWatchers/ipFlowVerify/action|Gibt zurück, ob das Paket an oder von einem bestimmten Ziel zugelassen oder abgelehnt wird.|
|/networkWatchers/nextHop/action|Gibt für ein angegebenes Ziel und eine Ziel-IP-Adresse die Art des nächsten Hops und die IP-Adresse des nächsten Hops zurück.|
|/networkWatchers/queryFlowLogStatus/action|Ruft den Status der Datenflussprotokollierung für eine Ressource ab.|
|/networkWatchers/queryTroubleshootResult/action|Ruft das Ergebnis des zuvor oder momentan ausgeführten Problembehandlungsvorgangs ab.|
|/networkWatchers/securityGroupView/action|Dient zum Anzeigen der konfigurierten und effektiven Netzwerksicherheitsgruppen-Regeln, die auf einen virtuellen Computer angewendet wurden.|
|/networkWatchers/topology/action|Ruft eine Netzwerkebenenansicht mit Ressourcen und deren Beziehungen in einer Ressourcengruppe ab.|
|/networkWatchers/troubleshoot/action|Startet die Problembehandlung für eine Netzwerkressource in Azure.|
|/networkWatchers/packetCaptures/queryStatus/action|Ruft Informationen zu Eigenschaften und zum Status einer Paketerfassungsressource ab.|
|/networkWatchers/packetCaptures/stop/action|Dient zum Beenden der laufenden Paketerfassungssitzung.|
|/networkWatchers/packetCaptures/read|Dient zum Abrufen der Paketerfassungsdefinition.|
|/networkWatchers/packetCaptures/write|Erstellt eine Paketerfassung.|
|/networkWatchers/packetCaptures/delete|Löscht eine Paketerfassung.|
|/loadBalancers/read|Ruft eine Lastenausgleichsdefinition ab.|
|/loadBalancers/write|Erstellt einen Lastenausgleich oder aktualisiert einen vorhandenen Lastenausgleich.|
|/loadBalancers/delete|Löscht einen Lastenausgleich.|
|/loadBalancers/networkInterfaces/read|Ruft Verweise auf alle Netzwerkschnittstellen unter einem Lastenausgleich ab.|
|/loadBalancers/loadBalancingRules/read|Ruft eine Lastenausgleichsregel-Definition für den Lastenausgleich ab.|
|/loadBalancers/backendAddressPools/read|Ruft eine Back-End-Adresspooldefinition für den Lastenausgleich ab.|
|/loadBalancers/backendAddressPools/join/action|Verknüpft einen Back-End-Adresspool für den Lastenausgleich.|
|/loadBalancers/inboundNatPools/read|Ruft eine eingehende NAT-Pooldefinition für den Lastenausgleich ab.|
|/loadBalancers/inboundNatPools/join/action|Verknüpft einen eingehenden NAT-Pool für den Lastenausgleich.|
|/loadBalancers/inboundNatRules/read|Ruft eine eingehende NAT-Regeldefinition für den Lastenausgleich ab.|
|/loadBalancers/inboundNatRules/write|Erstellt eine eingehende NAT-Regel für den Lastenausgleich oder aktualisiert eine vorhandene eingehende NAT-Regel für den Lastenausgleich.|
|/loadBalancers/inboundNatRules/delete|Löscht eine eingehende NAT-Regel für den Lastenausgleich.|
|/loadBalancers/inboundNatRules/join/action|Verknüpft eine eingehende NAT-Regel für den Lastenausgleich.|
|/loadBalancers/outboundNatRules/read|Ruft eine ausgehende NAT-Regeldefinition für den Lastenausgleich ab.|
|/loadBalancers/probes/read|Ruft einen Lastenausgleichstest ab.|
|/loadBalancers/virtualMachines/read|Ruft Verweise auf alle virtuellen Computer unter einem Lastenausgleich ab.|
|/loadBalancers/frontendIPConfigurations/read|Ruft eine Front-End-IP-Konfigurationsdefinition für den Lastenausgleich ab.|
|/trafficManagerGeographicHierarchies/read|Ruft die geografische Traffic Manager-Hierarchie mit Regionen ab, die mit der geografischen Routingmethode für Datenverkehr verwendet werden können.|
|/bgpServiceCommunities/read|Dient zum Abrufen von BGP Service Communities.|
|/applicationGatewayAvailableWafRuleSets/read|Ruft verfügbare WAF-Regelsätze für Application Gateway ab.|
|/virtualNetworks/read|Dient zum Abrufen der Definition des virtuellen Netzwerks.|
|/virtualNetworks/write|Erstellt ein virtuelles Netzwerk oder aktualisiert ein vorhandenes virtuelles Netzwerk.|
|/virtualNetworks/delete|Löscht ein virtuelles Netzwerk.|
|/virtualNetworks/peer/action|Richtet ein virtuelles Netzwerk als Peer für ein anderes virtuelles Netzwerk ein.|
|/virtualNetworks/virtualNetworkPeerings/read|Ruft eine Definition für das Peering virtueller Netzwerke ab.|
|/virtualNetworks/virtualNetworkPeerings/write|Erstellt ein Peering virtueller Netzwerke oder aktualisiert ein vorhandenes Peering virtueller Netzwerke.|
|/virtualNetworks/virtualNetworkPeerings/delete|Löscht ein Peering virtueller Netzwerke.|
|/virtualNetworks/subnets/read|Ruft eine Subnetzdefinition für virtuelle Netzwerke ab.|
|/virtualNetworks/subnets/write|Erstellt ein Subnetz für virtuelle Netzwerke oder aktualisiert ein vorhandenes Subnetz für virtuelle Netzwerke.|
|/virtualNetworks/subnets/delete|Löscht ein Subnetz für virtuelle Netzwerke.|
|/virtualNetworks/subnets/join/action|Verknüpft ein virtuelles Netzwerk.|
|/virtualNetworks/subnets/joinViaServiceTunnel/action|Verknüpft Ressourcen wie etwa ein Speicherkonto oder eine SQL-Datenbank mit einem Diensttunneling-fähigen Subnetz.|
|/virtualNetworks/subnets/virtualMachines/read|Ruft Verweise auf alle virtuellen Computer im Subnetz eines virtuellen Netzwerks ab.|
|/virtualNetworks/checkIpAddressAvailability/read|Prüft, ob eine IP-Adresse im angegebenen virtuellen Netzwerk verfügbar ist.|
|/virtualNetworks/virtualMachines/read|Ruft Verweise auf alle virtuellen Computer in einem virtuellen Netzwerk ab.|
|/expressRouteServiceProviders/read|Ruft ExpressRoute-Dienstanbieter ab.|
|/dnsoperationresults/read|Ruft Ergebnisse eines DNS-Vorgangs ab.|
|/localnetworkgateways/read|Ruft lokale Netzwerkgateways ab.|
|/localnetworkgateways/write|Erstellt ein lokales Netzwerkgateway oder aktualisiert ein vorhandenes lokales Netzwerkgateway.|
|/localnetworkgateways/delete|Löscht lokale Netzwerkgateways.|
|/trafficManagerProfiles/read|Dient zum Abrufen der Traffic Manager-Profilkonfiguration. Dazu zählen DNS-Einstellungen, Routingeinstellungen für Datenverkehr, Endpunktüberwachungseinstellungen und die Liste mit den Endpunkten, die durch dieses Traffic Manager-Profil geroutet werden.|
|/trafficManagerProfiles/write|Dient zum Erstellen eines Traffic Manager-Profils oder zum Ändern der Konfiguration eines vorhandenen Traffic Manager-Profils. Dies schließt das Aktivieren oder Deaktivieren eines Profils sowie das Ändern von DNS-Einstellungen, Routingeinstellungen für Datenverkehr oder Endpunktüberwachungseinstellungen ein. Durch das Traffic Manager-Profil geroutete Endpunkte können hinzugefügt, entfernt, aktiviert oder deaktiviert werden.|
|/trafficManagerProfiles/delete|Dient zum Löschen des Traffic Manager-Profils. Alle dem Traffic Manager-Profil zugeordneten Einstellungen gehen verloren, und das Profil kann nicht mehr zum Routen von Datenverkehr verwendet werden.|
|/dnsoperationstatuses/read|Ruft den Status eines DNS-Vorgangs ab. |
|/operations/read|Dient zum Abrufen verfügbarer Vorgänge.|
|/expressRouteCircuits/read|Dient zum Abrufen eines ExpressRouteCircuit-Elements.|
|/expressRouteCircuits/write|Erstellt ein ExpressRouteCircuit-Element oder aktualisiert ein vorhandenes ExpressRouteCircuit-Element.|
|/expressRouteCircuits/delete|Löscht ein ExpressRouteCircuit-Element.|
|/expressRouteCircuits/stats/read|Ruft eine ExpressRouteCircuit-Statistik ab.|
|/expressRouteCircuits/peerings/read|Ruft ein ExpressRouteCircuit-Peering ab.|
|/expressRouteCircuits/peerings/write|Erstellt ein ExpressRouteCircuit-Peering oder aktualisiert ein vorhandenes ExpressRouteCircuit-Peering.|
|/expressRouteCircuits/peerings/delete|Löscht ein ExpressRouteCircuit-Peering.|
|/expressRouteCircuits/peerings/arpTables/action|Ruft ein ArpTable-Element für ein ExpressRouteCircuit-Peering ab.|
|/expressRouteCircuits/peerings/routeTables/action|Ruft ein RouteTable-Element für ein ExpressRouteCircuit-Peering ab.|
|/expressRouteCircuits/peerings/<br>routeTablesSummary/action|Ruft eine RouteTable-Zusammenfassung für ein ExpressRouteCircuit-Peering ab.|
|/expressRouteCircuits/peerings/stats/read|Ruft eine ExpressRouteCircuit-Peeringstatistik ab.|
|/expressRouteCircuits/authorizations/read|Ruft eine ExpressRouteCircuit-Autorisierung ab.|
|/expressRouteCircuits/authorizations/write|Erstellt eine ExpressRouteCircuit-Autorisierung oder aktualisiert eine vorhandene ExpressRouteCircuit-Autorisierung.|
|/expressRouteCircuits/authorizations/delete|Löscht eine ExpressRouteCircuit-Autorisierung.|
|/connections/read|Ruft VirtualNetworkGatewayConnection-Elemente ab.|
|/connections/write|Erstellt ein VirtualNetworkGatewayConnection-Element oder aktualisiert ein vorhandenes VirtualNetworkGatewayConnection-Element.|
|/connections/delete|Löscht VirtualNetworkGatewayConnection-Elemente.|
|/connections/sharedKey/read|Ruft SharedKey-Elemente für „VirtualNetworkGatewayConnection“ ab.|
|/connections/sharedKey/write|Erstellt ein SharedKey-Element für „VirtualNetworkGatewayConnection“ oder aktualisiert ein vorhandenes SharedKey-Element für „VirtualNetworkGatewayConnection“.|
|/networkSecurityGroups/read|Ruft eine Netzwerksicherheitsgruppen-Definition ab.|
|/networkSecurityGroups/write|Erstellt eine Netzwerksicherheitsgruppe oder aktualisiert eine vorhandene Netzwerksicherheitsgruppe.|
|/networkSecurityGroups/delete|Löscht eine Netzwerksicherheitsgruppe.|
|/networkSecurityGroups/join/action|Verknüpft eine Netzwerksicherheitsgruppe.|
|/networkSecurityGroups/defaultSecurityRules/read|Ruft eine Standardsicherheitsregel-Definition ab.|
|/networkSecurityGroups/securityRules/read|Ruft eine Sicherheitsregeldefinition ab.|
|/networkSecurityGroups/securityRules/write|Erstellt eine Sicherheitsregel oder aktualisiert eine vorhandene Sicherheitsregel.|
|/networkSecurityGroups/securityRules/delete|Löscht eine Sicherheitsregel.|
|/applicationGateways/read|Ruft ein Anwendungsgateway ab.|
|/applicationGateways/write|Erstellt ein Anwendungsgateway oder aktualisiert ein vorhandenes Anwendungsgateway.|
|/applicationGateways/delete|Löscht ein Anwendungsgateway.|
|/applicationGateways/backendhealth/action|Ruft die Back-End-Integrität eines Anwendungsgateways ab.|
|/applicationGateways/start/action|Startet ein Anwendungsgateway.|
|/applicationGateways/stop/action|Beendet ein Anwendungsgateway.|
|/applicationGateways/backendAddressPools/join/action|Verknüpft einen Back-End-Adresspool für ein Anwendungsgateway.|
|/routeTables/read|Ruft eine Routingtabellendefinition ab.|
|/routeTables/write|Erstellt eine Routingtabelle oder aktualisiert eine vorhandene Routingtabelle.|
|/routeTables/delete|Löscht eine Routingtabellendefinition.|
|/routeTables/join/action|Verknüpft eine Routingtabelle.|
|/routeTables/routes/read|Ruft eine Routendefinition ab.|
|/routeTables/routes/write|Erstellt eine Route oder aktualisiert eine vorhandene Route.|
|/routeTables/routes/delete|Löscht eine Routendefinition.|
|/locations/operationResults/read|Ruft das Ergebnis eines asynchronen POST- oder DELETE-Vorgangs ab.|
|/locations/checkDnsNameAvailability/read|Prüft, ob eine DNS-Bezeichnung am angegebenen Standort verfügbar ist.|
|/locations/usages/read|Ruft die Ressourcenverwendungsmetriken ab.|
|/locations/operations/read|Ruft die Vorgangsressource ab, die den Status eines asynchronen Vorgangs darstellt.|

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

| Vorgang | Beschreibung |
|---|---|
|/register/action|Registriert das Abonnement für den NotificiationHubs-Ressourcenanbieter und ermöglicht die Erstellung von Namespaces und Notification Hubs.|
|/CheckNamespaceAvailability/action|Prüft, ob ein bestimmter Name einer Namespaceressource innerhalb des NotificationHub-Diensts verfügbar ist.|
|/Namespaces/write|Dient zum Erstellen einer Namespaceressource und zum Aktualisieren ihrer Eigenschaften. Die Eigenschaften für Tags und Status des Namespace können aktualisiert werden.|
|/Namespaces/read|Dient zum Abrufen der Liste mit Beschreibungen für Namespaceressourcen.|
|/Namespaces/Delete|Dient zum Löschen von Namespaceressourcen.|
|/Namespaces/authorizationRules/action|Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln.|
|/Namespaces/CheckNotificationHubAvailability/action|Prüft, ob ein bestimmter NotificationHub-Name innerhalb eines Namespaces verfügbar ist.|
|/Namespaces/authorizationRules/write|Dient zum Erstellen von Autorisierungsregeln auf Namespaceebene und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel.|
|/Namespaces/authorizationRules/read|Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln.|
|/Namespaces/authorizationRules/delete|Dient zum Löschen von Namespace-Autorisierungsregeln. Die standardmäßige Namespace-Autorisierungsregel kann nicht gelöscht werden. |
|/Namespaces/authorizationRules/listkeys/action|Dient zum Abrufen der Verbindungszeichenfolge für den Namespace.|
|/Namespaces/authorizationRules/regenerateKeys/action|Namespace-Autorisierungsregel zum erneuten Generieren des primären/sekundären Schlüssels. Geben Sie den Schlüssel an, der neu generiert werden soll.|
|/Namespaces/NotificationHubs/write|Dient zum Erstellen eines Notification Hubs und zum Aktualisieren seiner Eigenschaften. Zu diesen Einstellungen zählen in erster Linie PNS-Anmeldeinformationen, Autorisierungsregeln und Gültigkeitsdauer.|
|/Namespaces/NotificationHubs/read|Dient zum Abrufen einer Liste mit Notification Hub-Ressourcenbeschreibungen.|
|/Namespaces/NotificationHubs/Delete|Dient zum Löschen von Notification Hub-Ressourcen.|
|/Namespaces/NotificationHubs/authorizationRules/action|Dient zum Abrufen der Liste mit Notification Hub-Autorisierungsregeln.|
|/Namespaces/NotificationHubs/pnsCredentials/action|Dient zum Abrufen aller Notification Hub-PNS-Anmeldeinformationen. Hierzu zählen Anmeldeinformationen für WNS, MPNS, APNS, GCM und Baidu.|
|/Namespaces/NotificationHubs/debugSend/action|Dient zum Senden einer Test-Pushbenachrichtigung.|
|/Namespaces/NotificationHubs/metricDefinitions/read|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacemetriken.|
|/Namespaces/NotificationHubs/<br>authorizationRules/write|Dient zum Erstellen von Notification Hub-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel.|
|/Namespaces/NotificationHubs/<br>authorizationRules/read|Dient zum Abrufen der Liste mit Notification Hub-Autorisierungsregeln.|
|/Namespaces/NotificationHubs/<br>authorizationRules/delete|Dient zum Löschen von Notification Hub-Autorisierungsregeln.|
|/Namespaces/NotificationHubs/<br>authorizationRules/listkeys/action|Dient zum Abrufen der Notification Hub-Verbindungszeichenfolge.|
|/Namespaces/NotificationHubs/<br>authorizationRules/regenerateKeys/action|Notification Hub-Autorisierungsregel zum erneuten Generieren des primären/sekundären Schlüssels. Geben Sie den Schlüssel an, der neu generiert werden soll.|

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

| Vorgang | Beschreibung |
|---|---|
|/register/action|Dient zum Registrieren eines Abonnements bei einem Ressourcenanbieter.|
|/linkTargets/read|Listet vorhandene Konten auf, die keinem Azure-Abonnement zugeordnet sind. Verwenden Sie eine Kunden-ID, die von diesem Vorgang in der Eigenschaft für die Kunden-ID des Vorgangs „Arbeitsbereich erstellen“ zurückgegeben wird, um dieses Azure-Abonnement mit einem Arbeitsbereich zu verknüpfen.|
|/workspaces/write|Erstellt einen neuen Arbeitsbereich oder stellt eine Verknüpfung mit einem vorhandenen Arbeitsbereich her, indem die Kunden-ID für den vorhandenen Arbeitsbereich bereitgestellt wird.|
|/workspaces/read|Ruft einen vorhandenen Arbeitsbereich ab.|
|/workspaces/delete|Löscht einen Arbeitsbereich. Wenn der Arbeitsbereich bei seiner Erstellung mit einem vorhandenen Arbeitsbereich verknüpft war, wird der Arbeitsbereich, mit dem er verknüpft war, nicht gelöscht.|
|/workspaces/generateregistrationcertificate/action|Generiert ein Registrierungszertifikat für den Arbeitsbereich. Dieses Zertifikat wird verwendet, um Microsoft System Center Operation Manager mit dem Arbeitsbereich zu verbinden.|
|/workspaces/sharedKeys/action|Ruft die gemeinsam verwendeten Schlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden.|
|/workspaces/search/action|Führt eine Suchabfrage aus.|
|/workspaces/datasources/read|Dient zum Abrufen von Datenquellen unter einem Arbeitsbereich.|
|/workspaces/datasources/write|Dient zum Erstellen/Aktualisieren von Datenquellen unter einem Arbeitsbereich.|
|/workspaces/datasources/delete|Dient zum Löschen von Datenquellen unter einem Arbeitsbereich.|
|/workspaces/managementGroups/read|Ruft die Namen und Metadaten für über System Center Operations Manager verwaltete Gruppen ab, die mit diesem Arbeitsbereich verbunden sind.|
|/workspaces/schema/read|Ruft das Suchschema für den Arbeitsbereich ab.  Ein Suchschema umfasst die verfügbar gemachten Felder und die dazugehörigen Typen.|
|/workspaces/usages/read|Ruft Nutzungsdaten für einen Arbeitsbereich ab, einschließlich der durch den Arbeitsbereich gelesenen Datenmenge.|
|/workspaces/intelligencepacks/read|Listet alle für einen angegebenen Arbeitsbereich sichtbaren Intelligence Packs auf und gibt an, ob das Pack für diesen Arbeitsbereich aktiviert oder deaktiviert ist.|
|/workspaces/intelligencepacks/enable/action|Aktiviert ein Intelligence Pack für einen bestimmten Arbeitsbereich.|
|/workspaces/intelligencepacks/disable/action|Deaktiviert ein Intelligence Pack für einen bestimmten Arbeitsbereich.|
|/workspaces/sharedKeys/read|Ruft die gemeinsam verwendeten Schlüssel für den Arbeitsbereich ab. Diese Schlüssel werden verwendet, um Microsoft Operational Insights-Agents mit dem Arbeitsbereich zu verbinden.|
|/workspaces/savedSearches/read|Ruft eine gespeicherte Suchabfrage ab.|
|/workspaces/savedSearches/write|Erstellt eine gespeicherte Suchabfrage.|
|/workspaces/savedSearches/delete|Löscht eine gespeicherte Suchabfrage.|
|/workspaces/storageinsightconfigs/write|Erstellt eine neue Speicherkonfiguration. Diese Konfigurationen werden dazu verwendet, Daten von einem Speicherort in einem vorhandenen Speicherkonto abzurufen.|
|/workspaces/storageinsightconfigs/read|Ruft eine Speicherkonfiguration ab.|
|/workspaces/storageinsightconfigs/delete|Löscht eine Speicherkonfiguration. Daraufhin werden von Microsoft Operational Insights keine Daten mehr aus dem Speicherkonto gelesen.|
|/workspaces/configurationScopes/read|Dient zum Abrufen von Konfigurationsbereichen.|
|/workspaces/configurationScopes/write|Dient zum Festlegen von Konfigurationsbereichen.|
|/workspaces/configurationScopes/delete|Dient zum Löschen von Konfigurationsbereichen.|

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

| Vorgang | Beschreibung |
|---|---|
|/register/action|Dient zum Registrieren eines Abonnements bei einem Ressourcenanbieter.|
|/solutions/write|Dient zum Erstellen neuer OMS-Lösungen.|
|/solutions/read|Dient zum Abrufen vorhandener OMS-Lösungen.|
|/solutions/delete|Dient zum Löschen vorhandener OMS-Lösungen.|

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

| Vorgang | Beschreibung |
|---|---|
|/Vaults/backupJobsExport/action|Dient zum Exportieren von Aufträgen.|
|/Vaults/write|Der Vorgang „Tresor erstellen“ erstellt eine Azure-Ressource vom Typ „Tresor“.|
|/Vaults/read|Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt.|
|/Vaults/delete|Der Vorgang „Tresor löschen“ löscht die angegebene Azure-Ressource vom Typ „Tresor“.|
|/Vaults/refreshContainers/read|Aktualisiert die Containerliste.|
|/Vaults/backupJobsExport/operationResults/read|Gibt das Ergebnis des Vorgangs „Auftrag exportieren“ zurück.|
|/Vaults/backupOperationResults/read|Gibt das Ergebnis eines Sicherungsvorgangs für Recovery Services-Tresore zurück.|
|/Vaults/monitoringAlerts/read|Ruft die Warnungen für den Recovery Services-Tresor ab.|
|/Vaults/monitoringAlerts/{Eindeutige Warnungs-ID}/read|Ruft die Warnungsdetails ab.|
|/Vaults/backupSecurityPIN/read|Gibt Informationen zur Sicherheits-PIN für Recovery Services-Tresore zurück.|
|/vaults/replicationEvents/read|Dient zum Lesen beliebiger Ereignisse.|
|/Vaults/backupProtectableItems/read|Gibt die Liste mit allen schützbaren Elementen zurück.|
|/vaults/replicationFabrics/read|Dient zum Lesen beliebiger Fabrics.|
|/vaults/replicationFabrics/write|Dient zum Erstellen oder Aktualisieren beliebiger Fabrics.|
|/vaults/replicationFabrics/remove/action|Dient zum Entfernen von Fabrics.|
|/vaults/replicationFabrics/checkConsistency/action|Prüft die Konsistenz des Fabrics.|
|/vaults/replicationFabrics/delete|Dient zum Löschen beliebiger Fabrics.|
|/vaults/replicationFabrics/renewcertificate/action||
|/vaults/replicationFabrics/deployProcessServerImage/action|Dient zum Bereitstellen von Prozessserverimages.|
|/vaults/replicationFabrics/reassociateGateway/action|Dient zum Neuzuordnen des Gateways.|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/<br>read|Dient zum Lesen beliebiger Recovery Services-Anbieter.|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/<br>remove/action|Dient zum Entfernen von Recovery Services-Anbietern.|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/<br>delete|Dient zum Löschen beliebiger Recovery Services-Anbieter.|
|/vaults/replicationFabrics/replicationRecoveryServicesProviders/<br>refreshProvider/action|Dient zum Aktualisieren des Anbieters.|
|/vaults/replicationFabrics/replicationStorageClassifications/read|Dient zum Lesen beliebiger Speicherklassifizierungen.|
|/vaults/replicationFabrics/replicationStorageClassifications/<br>replicationStorageClassificationMappings/read|Dient zum Lesen beliebiger Speicherklassifizierungszuordnungen.|
|/vaults/replicationFabrics/replicationStorageClassifications/<br>replicationStorageClassificationMappings/write|Dient zum Erstellen oder Aktualisieren beliebiger Speicherklassifizierungszuordnungen.|
|/vaults/replicationFabrics/replicationStorageClassifications/<br>replicationStorageClassificationMappings/delete|Dient zum Löschen beliebiger Speicherklassifizierungszuordnungen.|
|/vaults/replicationFabrics/replicationvCenters/read|Dient zum Lesen beliebiger Aufträge.|
|/vaults/replicationFabrics/replicationvCenters/write|Dient zum Erstellen oder Aktualisieren beliebiger Aufträge.|
|/vaults/replicationFabrics/replicationvCenters/delete|Dient zum Löschen beliebiger Aufträge.|
|/vaults/replicationFabrics/replicationNetworks/read|Dient zum Lesen beliebiger Netzwerke.|
|/vaults/replicationFabrics/replicationNetworks/<br>replicationNetworkMappings/read|Dient zum Lesen beliebiger Netzwerkzuordnungen.|
|/vaults/replicationFabrics/replicationNetworks/<br>replicationNetworkMappings/write|Dient zum Erstellen oder Aktualisieren beliebiger Netzwerkzuordnungen.|
|/vaults/replicationFabrics/replicationNetworks/<br>replicationNetworkMappings/delete|Dient zum Löschen beliebiger Netzwerkzuordnungen.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>read|Dient zum Lesen beliebiger Schutzcontainer.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>discoverProtectableItem/action|Dient zum Ermitteln des schützbaren Elements.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>write|Dient zum Erstellen oder Aktualisieren beliebiger Schutzcontainer.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>remove/action|Dient zum Entfernen von Schutzcontainern.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>switchprotection/action|Dient zum Wechseln von Schutzcontainern.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectableItems/read|Dient zum Lesen beliebiger schützbarer Elemente.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectionContainerMappings/read|Dient zum Lesen beliebiger Schutzcontainerzuordnungen.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectionContainerMappings/write|Dient zum Erstellen oder Aktualisieren beliebiger Schutzcontainerzuordnungen.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectionContainerMappings/remove/action|Dient zum Entfernen von Schutzcontainerzuordnungen.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectionContainerMappings/delete|Dient zum Löschen beliebiger Schutzcontainerzuordnungen.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectedItems/read|Dient zum Lesen beliebiger geschützter Elemente.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectedItems/write|Dient zum Erstellen oder Aktualisieren beliebiger geschützter Elemente.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectedItems/delete|Dient zum Löschen beliebiger geschützter Elemente.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectedItems/remove/action|Dient zum Entfernen geschützter Elemente.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectedItems/plannedFailover/action|Geplantes Failover|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectedItems/unplannedFailover/action|Failover|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectedItems/testFailover/action|Testfailover|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectedItems/testFailoverCleanup/action|Testfailoverbereinigung|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectedItems/failoverCommit/action|Failovercommit|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectedItems/reProtect/action|Dient zum erneuten Schützen geschützter Elemente.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectedItems/updateMobilityService/action|Dient zum Aktualisieren von Mobility Service.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectedItems/repairReplication/action|Dient zum Reparieren der Replikation.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectedItems/applyRecoveryPoint/action|Dient zum Anwenden des Wiederherstellungspunkts.|
|/vaults/replicationFabrics/replicationProtectionContainers/<br>replicationProtectedItems/recoveryPoints/read|Dient zum Lesen beliebiger Wiederherstellungspunkte für die Replikation.|
|/vaults/replicationPolicies/read|Dient zum Lesen beliebiger Richtlinien.|
|/vaults/replicationPolicies/write|Dient zum Erstellen oder Aktualisieren beliebiger Richtlinien.|
|/vaults/replicationPolicies/delete|Dient zum Löschen beliebiger Richtlinien.|
|/vaults/replicationRecoveryPlans/read|Dient zum Lesen beliebiger Wiederherstellungspläne.|
|/vaults/replicationRecoveryPlans/write|Dient zum Erstellen oder Aktualisieren beliebiger Wiederherstellungspläne.|
|/vaults/replicationRecoveryPlans/delete|Dient zum Löschen beliebiger Wiederherstellungspläne.|
|/vaults/replicationRecoveryPlans/plannedFailover/action|Wiederherstellungsplan für geplantes Failover|
|/vaults/replicationRecoveryPlans/unplannedFailover/action|Wiederherstellungsplan für Failover|
|/vaults/replicationRecoveryPlans/testFailover/action|Wiederherstellungsplan für Testfailover|
|/vaults/replicationRecoveryPlans/testFailoverCleanup/action|Wiederherstellungsplan für Testfailoverbereinigung|
|/vaults/replicationRecoveryPlans/failoverCommit/action|Wiederherstellungsplan für Failovercommit|
|/vaults/replicationRecoveryPlans/reProtect/action|Wiederherstellungsplan für erneutes Schützen|
|/Vaults/extendedInformation/read|Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt.|
|/Vaults/extendedInformation/write|Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt.|
|/Vaults/extendedInformation/delete|Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt.|
|/Vaults/backupManagementMetaData/read|Gibt Sicherungsverwaltungs-Metadaten für Recovery Services-Tresore zurück.|
|/Vaults/backupProtectionContainers/read|Gibt alle zum Abonnement gehörenden Container zurück.|
|/Vaults/backupFabrics/operationResults/read|Gibt den Status des Vorgangs zurück.|
|/Vaults/backupFabrics/protectionContainers/read|Gibt alle registrierten Container zurück.|
|/Vaults/backupFabrics/protectionContainers/<br>operationResults/read|Ruft das Ergebnis eines Vorgangs ab, der für den Schutzcontainer ausgeführt wurde.|
|/Vaults/backupFabrics/protectionContainers/<br>protectedItems/read|Gibt Objektdetails des geschützten Elements zurück.|
|/Vaults/backupFabrics/protectionContainers/<br>protectedItems/write|Dient zum Erstellen eines geschützten Elements für die Sicherung.|
|/Vaults/backupFabrics/protectionContainers/<br>protectedItems/delete|Löscht geschützte Elemente.|
|/Vaults/backupFabrics/protectionContainers/<br>protectedItems/backup/action|Führt eine Sicherung für geschützte Elemente aus.|
|/Vaults/backupFabrics/protectionContainers/<br>protectedItems/operationResults/read|Ruft das Ergebnis eines Vorgangs ab, der für geschützte Elemente ausgeführt wurde.|
|/Vaults/backupFabrics/protectionContainers/<br>protectedItems/operationStatus/read|Gibt den Status eines Vorgangs zurück, der für geschützte Elemente ausgeführt wurde.|
|/Vaults/backupFabrics/protectionContainers/<br>protectedItems/recoveryPoints/read|Dient zum Abrufen von Wiederherstellungspunkten für geschützte Elemente.|
|/Vaults/backupFabrics/protectionContainers/<br>protectedItems/recoveryPoints/<br>restore/action|Dient zum Wiederherstellen von Wiederherstellungspunkten für geschützte Elemente.|
|/Vaults/backupFabrics/protectionContainers/<br>protectedItems/recoveryPoints/<br>provisionInstantItemRecovery/action|Dient zum Bereitstellen der sofortigen Elementwiederherstellung für geschützte Elemente.|
|/Vaults/backupFabrics/protectionContainers/<br>protectedItems/recoveryPoints/<br>revokeInstantItemRecovery/action|Dient zum Widerrufen der sofortigen Elementwiederherstellung für geschützte Elemente.|
|/Vaults/usages/read|Gibt Nutzungsdetails für einen Recovery Services-Tresor zurück.|
|/vaults/usages/read|Dient zum Lesen beliebiger Tresorverwendungen.|
|/Vaults/certificates/write|Der Vorgang „Ressourcenzertifikat aktualisieren“ aktualisiert das Zertifikat für die Ressourcen-/Tresoranmeldeinformationen.|
|/Vaults/tokenInfo/read|Gibt Tokeninformationen für Recovery Services-Tresore zurück.|
|/vaults/replicationAlertSettings/read|Dient zum Lesen beliebiger Warnungseinstellungen.|
|/vaults/replicationAlertSettings/write|Dient zum Erstellen oder Aktualisieren beliebiger Warnungseinstellungen.|
|/Vaults/backupOperations/read|Gibt den Status eines Sicherungsvorgangs für Recovery Services-Tresore zurück.|
|/Vaults/storageConfig/read|Gibt die Speicherkonfiguration für Recovery Services-Tresore zurück.|
|/Vaults/storageConfig/write|Aktualisiert Speicherkonfiguration für Recovery Services-Tresore.|
|/Vaults/backupUsageSummaries/read|Gibt Zusammenfassungen für geschützte Elemente und geschützte Server für einen Recovery Services-Tresor zurück.|
|/Vaults/backupProtectedItems/read|Gibt die Liste mit allen geschützten Elementen zurück.|
|/Vaults/backupconfig/vaultconfig/read|Gibt die Konfiguration für Recovery Services-Tresore zurück.|
|/Vaults/backupconfig/vaultconfig/write|Aktualisiert die Konfiguration für Recovery Services-Tresore.|
|/Vaults/registeredIdentities/write|Der Vorgang „Dienstcontainer registrieren“ kann zum Registrieren eines Containers beim Wiederherstellungsdienst verwendet werden.|
|/Vaults/registeredIdentities/read|Der Vorgang „Container abrufen“ kann zum Abrufen der für eine Ressource registrierten Container verwendet werden.|
|/Vaults/registeredIdentities/delete|Der Vorgang „Registrierung des Containers aufheben“ kann zum Aufheben der Registrierung eines Containers verwendet werden.|
|/Vaults/registeredIdentities/operationResults/read|Mit dem Vorgang „Vorgangsergebnisse abrufen“ können der Vorgangsstatus und das Ergebnis für den asynchron übermittelten Vorgang abgerufen werden.|
|/vaults/replicationJobs/read|Dient zum Lesen beliebiger Aufträge.|
|/vaults/replicationJobs/cancel/action|Dient zum Abbrechen von Aufträgen.|
|/vaults/replicationJobs/restart/action|Dient zum Neustarten von Aufträgen.|
|/vaults/replicationJobs/resume/action|Dient zum Fortsetzen von Aufträgen.|
|/Vaults/backupPolicies/read|Gibt alle Schutzrichtlinien zurück.|
|/Vaults/backupPolicies/write|Erstellt Schutzrichtlinien.|
|/Vaults/backupPolicies/delete|Dient zum Löschen einer Schutzrichtlinie.|
|/Vaults/backupPolicies/operationResults/read|Dient zum Abrufen der Ergebnisse von Richtlinienvorgängen.|
|/Vaults/backupPolicies/operationStatus/read|Dient zum Abrufen des Status von Richtlinienvorgängen.|
|/Vaults/vaultTokens/read|Der Vorgang „Tresortoken“ kann zum Abrufen des Tresortokens für Back-End-Vorgänge auf Tresorebene verwendet werden.|
|/Vaults/monitoringConfigurations/notificationConfiguration/read|Ruft die Konfiguration von Recovery Services-Tresorbenachrichtigungen ab.|
|/Vaults/backupJobs/read|Gibt alle Auftragsobjekte zurück.|
|/Vaults/backupJobs/cancel/action|Dient zum Abbrechen des Auftrags.|
|/Vaults/backupJobs/operationResults/read|Gibt das Ergebnis von Auftragsvorgängen zurück.|
|/locations/allocateStamp/action|„AllocateStamp“ ist ein interner Vorgang des Diensts.|
|/locations/allocatedStamp/read|„GetAllocatedStamp“ ist ein interner Vorgang des Diensts.|

## <a name="microsoftrelay"></a>Microsoft.Relay

| Vorgang | Beschreibung |
|---|---|
|/checkNamespaceAvailability/action|Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement.|
|/register/action|Registriert das Abonnement für den Relayressourcenanbieter und ermöglicht die Erstellung von Relayressourcen.|
|/namespaces/write|Dient zum Erstellen einer Namespaceressource und zum Aktualisieren ihrer Eigenschaften. Die Eigenschaften für Tags und Status des Namespace können aktualisiert werden.|
|/namespaces/read|Dient zum Abrufen der Liste mit Beschreibungen für Namespaceressourcen.|
|/namespaces/Delete|Dient zum Löschen von Namespaceressourcen.|
|/namespaces/authorizationRules/write|Dient zum Erstellen von Autorisierungsregeln auf Namespaceebene und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel.|
|/namespaces/authorizationRules/delete|Dient zum Löschen von Namespace-Autorisierungsregeln. Die standardmäßige Namespace-Autorisierungsregel kann nicht gelöscht werden. |
|/namespaces/authorizationRules/listkeys/action|Dient zum Abrufen der Verbindungszeichenfolge für den Namespace.|
|/namespaces/HybridConnections/write|Dient zum Erstellen oder Aktualisieren von HybridConnection-Eigenschaften.|
|/namespaces/HybridConnections/read|Dient zum Abrufen einer Liste mit HybridConnection-Ressourcenbeschreibungen.|
|/namespaces/HybridConnections/Delete|Vorgang zum Löschen von HybridConnection-Ressourcen.|
|/namespaces/HybridConnections/authorizationRules/write|Dient zum Erstellen von HybridConnection-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel.|
|/namespaces/HybridConnections/authorizationRules/delete|Vorgang zum Löschen von HybridConnection-Autorisierungsregeln.|
|/namespaces/HybridConnections/authorizationRules/listkeys/action|Dient zum Abrufen der HybridConnection-Verbindungszeichenfolge.|
|/namespaces/WcfRelays/write|Dient zum Erstellen oder Aktualisieren von WcfRelay-Eigenschaften.|
|/namespaces/WcfRelays/read|Dient zum Abrufen einer Liste mit WcfRelay-Ressourcenbeschreibungen.|
|/namespaces/WcfRelays/Delete|Vorgang zum Löschen von WcfRelay-Ressourcen.|
|/namespaces/WcfRelays/authorizationRules/write|Dient zum Erstellen von WcfRelay-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel.|
|/namespaces/WcfRelays/authorizationRules/delete|Vorgang zum Löschen von WcfRelay-Autorisierungsregeln.|
|/namespaces/WcfRelays/authorizationRules/listkeys/action|Dient zum Abrufen der WcfRelay-Verbindungszeichenfolge.|

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

| Vorgang | Beschreibung |
|---|---|
|/AvailabilityStatuses/read|Ruft den Verfügbarkeitsstatus für alle Ressourcen im angegebenen Bereich ab.|
|/AvailabilityStatuses/current/read|Ruft den Verfügbarkeitsstatus für die angegebene Ressource ab.|

## <a name="microsoftresources"></a>Microsoft.Resources

| Vorgang | Beschreibung |
|---|---|
|/checkResourceName/action|Dient zum Überprüfen der Gültigkeit des Ressourcennamens.|
|/providers/read|Dient zum Abrufen der Anbieterliste.|
|/subscriptions/read|Ruft die Abonnementliste ab.|
|/subscriptions/operationresults/read|Dient zum Abrufen der Ergebnisse des Abonnementvorgangs.|
|/subscriptions/providers/read|Ruft Ressourcenanbieter ab oder listet sie auf.|
|/subscriptions/tagNames/read|Ruft Abonnementtags ab oder listet sie auf.|
|/subscriptions/tagNames/write|Fügt ein Abonnementtag hinzu.|
|/subscriptions/tagNames/delete|Löscht ein Abonnementtag.|
|/subscriptions/tagNames/tagValues/read|Ruft Abonnementtagwerte ab oder listet sie auf.|
|/subscriptions/tagNames/tagValues/write|Fügt einen Abonnementtagwert hinzu.|
|/subscriptions/tagNames/tagValues/delete|Löscht einen Abonnementtagwert.|
|/subscriptions/resources/read|Ruft die Ressourcen eines Abonnements ab.|
|/subscriptions/resourceGroups/read|Ruft Ressourcengruppen ab oder listet sie auf.|
|/subscriptions/resourceGroups/write|Erstellt oder aktualisiert eine Ressourcengruppe.|
|/subscriptions/resourceGroups/delete|Löscht eine Ressourcengruppe und alle dazugehörigen Ressourcen.|
|/subscriptions/resourceGroups/moveResources/action|Verschiebt Ressourcen aus einer Ressourcengruppe in eine andere.|
|/subscriptions/resourceGroups/validateMoveResources/action|Überprüft das Verschieben von Ressourcen aus einer Ressourcengruppe in eine andere.|
|/subscriptions/resourcegroups/resources/read|Ruft die Ressourcen für die Ressourcengruppe ab.|
|/subscriptions/resourcegroups/deployments/read|Ruft Bereitstellungen ab oder listet sie auf.|
|/subscriptions/resourcegroups/deployments/write|Erstellt oder aktualisiert eine Bereitstellung.|
|/subscriptions/resourcegroups/deployments/operationstatuses/read|Ruft den Status von Bereitstellungsvorgängen ab oder listet ihn auf.|
|/subscriptions/resourcegroups/deployments/operations/read|Ruft Bereitstellungsvorgänge ab oder listet sie auf.|
|/subscriptions/locations/read|Ruft die Liste mit den unterstützten Standorten ab.|
|/links/read|Ruft Ressourcenlinks ab oder listet sie auf.|
|/links/write|Erstellt oder aktualisiert einen Ressourcenlink.|
|/links/delete|Löscht einen Ressourcenlink.|
|/tenants/read|Ruft die Mandantenliste ab.|
|/resources/read|Dient zum Abrufen einer gefilterten Ressourcenliste.|
|/deployments/read|Ruft Bereitstellungen ab oder listet sie auf.|
|/deployments/write|Erstellt oder aktualisiert eine Bereitstellung.|
|/deployments/delete|Löscht eine Bereitstellung.|
|/deployments/cancel/action|Bricht eine Bereitstellung ab.|
|/deployments/validate/action|Überprüft eine Bereitstellung.|
|/deployments/operations/read|Ruft Bereitstellungsvorgänge ab oder listet sie auf.|

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

| Vorgang | Beschreibung |
|---|---|
|/jobcollections/read|Dient zum Abrufen von Auftragssammlungen.|
|/jobcollections/write|Erstellt oder aktualisiert Auftragssammlungen.|
|/jobcollections/delete|Löscht Auftragssammlungen.|
|/jobcollections/enable/action|Aktiviert Auftragssammlungen.|
|/jobcollections/disable/action|Deaktiviert Auftragssammlungen.|
|/jobcollections/jobs/read|Ruft Aufträge ab.|
|/jobcollections/jobs/write|Erstellt oder aktualisiert Aufträge.|
|/jobcollections/jobs/delete|Löscht Aufträge.|
|/jobcollections/jobs/run/action|Führt Aufträge aus.|
|/jobcollections/jobs/generateLogicAppDefinition/action|Generiert Logik-App-Definitionen auf der Grundlage eines Scheduler-Auftrags.|
|/jobcollections/jobs/jobhistories/read|Ruft den Auftragsverlauf ab.|

## <a name="microsoftsearch"></a>Microsoft.Search

| Vorgang | Beschreibung |
|---|---|
|/register/action|Registriert das Abonnement für den Search-Ressourcenanbieter und ermöglicht die Erstellung von Suchdiensten.|
|/checkNameAvailability/action|Prüft die Verfügbarkeit des Dienstnamens.|
|/searchServices/write|Erstellt oder aktualisiert den Suchdienst.|
|/searchServices/read|Liest den Suchdienst.|
|/searchServices/delete|Löscht den Suchdienst.|
|/searchServices/start/action|Startet den Suchdienst.|
|/searchServices/stop/action|Beendet den Suchdienst.|
|/searchServices/listAdminKeys/action|Liest die Administratorschlüssel.|
|/searchServices/regenerateAdminKey/action|Generiert die Administratorschlüssel neu.|
|/searchServices/createQueryKey/action|Erstellt den Abfrageschlüssel.|
|/searchServices/queryKey/read|Liest die Abfrageschlüssel.|
|/searchServices/queryKey/delete|Löscht den Abfrageschlüssel.|

## <a name="microsoftsecurity"></a>Microsoft.Security

| Vorgang | Beschreibung |
|---|---|
|/jitNetworkAccessPolicies/read|Ruft die JIT-Netzwerkzugriffsrichtlinien ab.|
|/jitNetworkAccessPolicies/write|Erstellt eine neue JIT-Netzwerkzugriffsrichtlinie oder aktualisiert eine bereits vorhandene.|
|/jitNetworkAccessPolicies/initiate/action|Initiiert eine JIT-Netzwerkzugriffsrichtlinie.|
|/securitySolutionsReferenceData/read|Ruft die Verweisdaten für Sicherheitslösungen ab.|
|/securityStatuses/read|Ruft den Sicherheitsintegritätsstatus für Azure-Ressourcen ab.|
|/webApplicationFirewalls/read|Ruft die Web Application Firewalls ab.|
|/webApplicationFirewalls/write|Erstellt eine neue Web Application Firewall oder aktualisiert eine bereits vorhandene.|
|/webApplicationFirewalls/delete|Löscht eine Web Application Firewall.|
|/securitySolutions/read|Ruft die Sicherheitslösungen ab.|
|/securitySolutions/write|Erstellt eine neue Sicherheitslösung oder aktualisiert eine bereits vorhandene.|
|/securitySolutions/delete|Löscht eine Sicherheitslösung.|
|/tasks/read|Ruft alle verfügbaren Sicherheitsempfehlungen ab.|
|/tasks/dismiss/action|Dient zum Verwerfen einer Sicherheitsempfehlung.|
|/tasks/activate/action|Dient zum Aktivieren einer Sicherheitsempfehlung.|
|/policies/read|Ruft die Sicherheitsrichtlinie ab.|
|/policies/write|Aktualisiert die Sicherheitsrichtlinie.|
|/applicationWhitelistings/read|Ruft die Anwendungswhitelists ab.|
|/applicationWhitelistings/write|Erstellt eine neue Anwendungswhitelist oder aktualisiert eine bereits vorhandene.|

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

| Vorgang | Beschreibung |
|---|---|
|/subscriptions/write|Erstellt oder aktualisiert ein Abonnement.|
|/gateways/write|Erstellt oder aktualisiert ein Gateway.|
|/gateways/delete|Löscht ein Gateway.|
|/gateways/read|Ruft ein Gateway ab.|
|/gateways/regenerateprofile/action|Generiert das Gatewayprofil neu.|
|/gateways/upgradetolatest/action|Aktualisiert das Gateway auf die neueste Version.|
|/nodes/write|Erstellt oder aktualisiert einen Knoten.|
|/nodes/delete|Löscht einen Knoten.|
|/nodes/read|Ruft einen Knoten ab.|
|/sessions/write|Erstellt oder aktualisiert eine Sitzung.|
|/sessions/read|Ruft eine Sitzung ab.|
|/sessions/delete|Löscht eine Sitzung.|

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

| Vorgang | Beschreibung |
|---|---|
|/checkNameAvailability/action|Prüft die Verfügbarkeit des Namespaces unter dem angegebenen Abonnement.|
|/register/action|Registriert das Abonnement für den ServiceBus-Ressourcenanbieter und ermöglicht die Erstellung von ServiceBus-Ressourcen.|
|/namespaces/write|Dient zum Erstellen einer Namespaceressource und zum Aktualisieren ihrer Eigenschaften. Die Eigenschaften für Tags und Status des Namespace können aktualisiert werden.|
|/namespaces/read|Dient zum Abrufen der Liste mit Beschreibungen für Namespaceressourcen.|
|/namespaces/Delete|Dient zum Löschen von Namespaceressourcen.|
|/namespaces/metricDefinitions/read|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacemetriken.|
|/namespaces/authorizationRules/write|Dient zum Erstellen von Autorisierungsregeln auf Namespaceebene und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel.|
|/namespaces/authorizationRules/read|Dient zum Abrufen der Liste mit Beschreibungen für Namespace-Autorisierungsregeln.|
|/namespaces/authorizationRules/delete|Dient zum Löschen von Namespace-Autorisierungsregeln. Die standardmäßige Namespace-Autorisierungsregel kann nicht gelöscht werden. |
|/namespaces/authorizationRules/listkeys/action|Dient zum Abrufen der Verbindungszeichenfolge für den Namespace.|
|/namespaces/authorizationRules/regenerateKeys/action|Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource.|
|/namespaces/diagnosticSettings/read|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen.|
|/namespaces/diagnosticSettings/write|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespacediagnoseeinstellungen.|
|/namespaces/queues/write|Dient zum Erstellen oder Aktualisieren von Warteschlangeneigenschaften.|
|/namespaces/queues/read|Dient zum Abrufen einer Liste mit Warteschlangen-Ressourcenbeschreibungen.|
|/namespaces/queues/Delete|Vorgang zum Löschen von Warteschlangenressourcen.|
|/namespaces/queues/authorizationRules/write|Dient zum Erstellen von Warteschlangen-Autorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel.|
|/namespaces/queues/authorizationRules/read| Dient zum Abrufen der Liste mit Warteschlangen-Autorisierungsregeln.|
|/namespaces/queues/authorizationRules/delete|Vorgang zum Löschen von Warteschlangen-Autorisierungsregeln.|
|/namespaces/queues/authorizationRules/listkeys/action|Dient zum Abrufen der Warteschlangen-Verbindungszeichenfolge.|
|/namespaces/queues/authorizationRules/regenerateKeys/action|Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource.|
|/namespaces/logDefinitions/read|Dient zum Abrufen der Liste mit Ressourcenbeschreibungen für Namespaceprotokolle.|
|/namespaces/topics/write|Dient zum Erstellen oder Aktualisieren von Themaeigenschaften.|
|/namespaces/topics/read|Dient zum Abrufen einer Liste mit Themaressourcenbeschreibungen.|
|/namespaces/topics/Delete|Vorgang zum Löschen von Themaressourcen.|
|/namespaces/topics/authorizationRules/write|Dient zum Erstellen von Themaautorisierungsregeln und zum Aktualisieren der dazugehörigen Eigenschaften. Aktualisiert werden können die Zugriffsrechte für Autorisierungsregeln sowie der primäre und der sekundäre Schlüssel.|
|/namespaces/topics/authorizationRules/read| Dient zum Abrufen der Liste mit Themaautorisierungsregeln.|
|/namespaces/topics/authorizationRules/delete|Vorgang zum Löschen von Themaautorisierungsregeln.|
|/namespaces/topics/authorizationRules/listkeys/action|Dient zum Abrufen der Themaverbindungszeichenfolge.|
|/namespaces/topics/authorizationRules/regenerateKeys/action|Dient zum erneuten Generieren des primären oder sekundären Schlüssels für die Ressource.|
|/namespaces/topics/subscriptions/write|Dient zum Erstellen oder Aktualisieren von TopicSubscription-Eigenschaften.|
|/namespaces/topics/subscriptions/read|Dient zum Abrufen einer Liste mit TopicSubscription-Ressourcenbeschreibungen.|
|/namespaces/topics/subscriptions/Delete|Vorgang zum Löschen von TopicSubscription-Ressourcen.|
|/namespaces/topics/subscriptions/rules/write|Dient zum Erstellen oder Aktualisieren von Aktualisierungsregeleigenschaften.|
|/namespaces/topics/subscriptions/rules/read|Dient zum Abrufen einer Liste mit Regelressourcenbeschreibungen.|
|/namespaces/topics/subscriptions/rules/Delete|Vorgang zum Löschen von Regelressourcen.|

## <a name="microsoftsql"></a>Microsoft.Sql

| Vorgang | Beschreibung |
|---|---|
|/servers/read|Dient zum Zurückgeben einer Liste mit Servern in einer Ressourcengruppe für ein Abonnement.|
|/servers/write|Dient zum Erstellen eines neuen Servers oder zum Ändern der Eigenschaften vorhandener Server in einer Ressourcengruppe für ein Abonnement.|
|/servers/delete|Dient zum Löschen eines Servers und aller enthaltenen Datenbanken und Pools für elastische Datenbanken.|
|/servers/import/action|Dient zum Erstellen einer neuen Datenbank auf dem Server und zum Bereitstellen von Schema und Daten aus einem DACPAC-Paket.|
|/servers/upgrade/action|Dient zum Aktivieren neuer Funktionen aus der neuesten Version des Servers und zum Angeben der Konvertierungszuordnung für die Datenbankedition.|
|/servers/VulnerabilityAssessmentScans/action|Dient zum Ausführen einer Serverüberprüfung zur Bewertung von Sicherheitsrisiken.|
|/servers/operationResults/read|Vorgang zum Nachverfolgen des Status eines Serverupgrades von einer niedrigeren auf eine höhere Version.|
|/servers/operationResults/delete|Dient zum Abbrechen eines laufenden Serverversionsupgrades.|
|/servers/securityAlertPolicies/read|Dient zum Abrufen von Details zur Richtlinie für die Serverbedrohungserkennung, die für einen bestimmten Server konfiguriert ist.|
|/servers/securityAlertPolicies/write|Dient zum Ändern der Serverbedrohungserkennung für einen bestimmten Server.|
|/servers/securityAlertPolicies/operationResults/read|Dient zum Abrufen von Ergebnissen des Set-Vorgangs der Richtlinie für die Serverbedrohungserkennung.|
|/servers/administrators/read|Dient zum Abrufen von Serveradministratordetails.|
|/servers/administrators/write|Dient zum Erstellen oder Aktualisieren von Serveradministratoren.|
|/servers/administrators/delete|Dient zum Löschen von Serveradministratoren vom Server.|
|/servers/recoverableDatabases/read|Vorgang für die Notfallwiederherstellung der Livedatenbank, um die Datenbank auf der Grundlage des letzten als funktionierend bekannten Sicherungspunkts wiederherzustellen. Gibt Informationen zur letzten als funktionierend bekannten Sicherung zurück, stellt die Datenbank aber nicht wieder her.|
|/servers/serviceObjectives/read|Dient zum Abrufen einer Liste mit verfügbaren Servicelevelzielen (auch Leistungsstufen genannt) für einen bestimmten Server.|
|/servers/firewallRules/read|Dient zum Abrufen von Details zu Serverfirewallregeln.|
|/servers/firewallRules/write|Dient zum Erstellen oder Aktualisieren der Serverfirewallregel, die den zulässigen IP-Adressbereich für die Verbindungsherstellung mit dem Server steuert.|
|/servers/firewallRules/delete|Dient zum Löschen von Firewallregeln vom Server.|
|/servers/administratorOperationResults/read|Dient zum Abrufen von Ergebnissen von Serveradministratorvorgängen.|
|/servers/recommendedElasticPools/read|Dient zum Abrufen von Empfehlungen für Pools für elastische Datenbanken, um Kosten zu senken oder die Leistung zu verbessern. Die Empfehlungen basieren auf der Ressourcenverwendung in der Vergangenheit.|
|/servers/recommendedElasticPools/metrics/read|Dient zum Abrufen von Metriken für empfohlene Pools für elastische Datenbanken für einen bestimmten Server.|
|/servers/recommendedElasticPools/databases/read|Dient zum Abrufen von Datenbanken, die empfohlenen Pools für elastische Datenbanken für einen bestimmten Server hinzugefügt werden sollen.|
|/servers/elasticPools/read|Dient zum Abrufen von Details zu Pools für elastische Datenbanken auf einem bestimmten Server.|
|/servers/elasticPools/write|Dient zum Erstellen eines neuen Pools für elastische Datenbanken oder zum Ändern von Eigenschaften vorhandener Pools für elastische Datenbanken.|
|/servers/elasticPools/delete|Dient zum Löschen vorhandener Pools für elastische Datenbanken.|
|/servers/elasticPools/operationResults/read|Dient zum Abrufen von Details zu einem bestimmten Vorgang für Pools für elastische Datenbanken.|
|/servers/elasticPools/providers/Microsoft.Insights/<br>metricDefinitions/read|Dient zum Zurückgeben verfügbarer Metriktypen für Pools für elastische Datenbanken.|
|/servers/elasticPools/providers/Microsoft.Insights/<br>diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/servers/elasticPools/providers/Microsoft.Insights/<br>diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/servers/elasticPools/metrics/read|Dient zum Zurückgeben von Ressourcenverwendungsmetriken für Pools für elastische Datenbanken.|
|/servers/elasticPools/elasticPoolDatabaseActivity/read|Dient zum Abrufen von Aktivitäten und Details zu einer bestimmten Datenbank, die einem Pool für elastische Datenbanken angehört.|
|/servers/elasticPools/advisors/read|Gibt eine Liste mit verfügbaren Ratgebern für den Pool für elastische Datenbanken zurück.|
|/servers/elasticPools/advisors/write|Dient zum Aktualisieren des Status der automatischen Ausführung eines Ratgebers auf der Ebene von Pools für elastische Datenbanken.|
|/servers/elasticPools/advisors/recommendedActions/read|Gibt eine Liste mit empfohlenen Aktionen des angegebenen Ratgebers für den Pool für elastische Datenbanken zurück.|
|/servers/elasticPools/advisors/recommendedActions/write|Dient zum Anwenden der empfohlenen Aktion auf den Pool für elastische Datenbanken.|
|/servers/elasticPools/elasticPoolActivity/read|Dient zum Abrufen von Aktivitäten und Details zu einem bestimmten Pool für elastische Datenbanken.|
|/servers/elasticPools/databases/read|Dient zum Abrufen der Liste und Details zu Datenbanken, die einem Pool für elastische Datenbanken auf einem bestimmten Server angehören.|
|/servers/auditingPolicies/read|Dient zum Abrufen von Details zur Richtlinie für die Standardservertabellen-Überwachung, die für einen bestimmten Server konfiguriert ist.|
|/servers/auditingPolicies/write|Dient zum Ändern der Standardservertabellen-Überwachung für einen bestimmten Server.|
|/servers/disasterRecoveryConfiguration/operationResults/read|Dient zum Abrufen der Ergebnisse des Konfigurationsvorgangs für die Notfallwiederherstellung.|
|/servers/advisors/read|Gibt eine Liste mit verfügbaren Ratgebern für den Server zurück.|
|/servers/advisors/write|Aktualisiert den Status der automatischen Ausführung eines Ratgebers auf der Serverebene.|
|/servers/advisors/recommendedActions/read|Gibt eine Liste mit empfohlenen Aktionen des angegebenen Ratgebers für den Server zurück.|
|/servers/advisors/recommendedActions/write|Dient zum Anwenden der empfohlenen Aktion auf den Server.|
|/servers/usages/read|Dient zum Zurückgeben des Server-DTU-Kontingents und des aktuellen DTU-Verbrauchs durch alle Datenbanken auf dem Server.|
|/servers/elasticPoolEstimates/read|Gibt die Liste mit Schätzwerten für den Pool für elastische Datenbanken zurück, die bereits für diesen Server erstellt wurden.|
|/servers/elasticPoolEstimates/write|Erstellt für die Liste der bereitgestellten Datenbanken neue Schätzwerte für den Pool für elastische Datenbanken.|
|/servers/auditingSettings/read|Dient zum Abrufen von Details zur Richtlinie für die Serverblobüberwachung, die für einen bestimmten Server konfiguriert ist.|
|/servers/auditingSettings/write|Dient zum Ändern der Serverblobüberwachung für einen bestimmten Server.|
|/servers/auditingSettings/operationResults/read|Dient zum Abrufen des Ergebnisses des Set-Vorgangs der Richtlinie für die Serverblobüberwachung.|
|/servers/backupLongTermRetentionVaults/read|Vorgang zum Abrufen eines Sicherungstresors für die langfristige Aufbewahrung. Gibt Informationen zu dem Tresor zurück, der bei diesem Server registriert ist.|
|/servers/backupLongTermRetentionVaults/write|Dient zum Registrieren eines Sicherungstresors für die langfristige Aufbewahrung.|
|/servers/restorableDroppedDatabases/read|Dient zum Abrufen einer Liste mit Datenbanken, die auf einem bestimmten Server verworfen wurden und weiterhin in einer Aufbewahrungsrichtlinie enthalten sind. Dieser Vorgang gibt eine Liste mit Datenbanken und zugehörigen Metadaten (wie etwa das Löschdatum) zurück.|
|/servers/databases/read|Dient zum Zurückgeben einer Liste mit Servern in einer Ressourcengruppe für ein Abonnement.|
|/servers/databases/write|Dient zum Erstellen eines neuen Servers oder zum Ändern der Eigenschaften vorhandener Server in einer Ressourcengruppe für ein Abonnement.|
|/servers/databases/delete|Dient zum Löschen eines Servers und aller enthaltenen Datenbanken und Pools für elastische Datenbanken.|
|/servers/databases/export/action|Dient zum Erstellen einer neuen Datenbank auf dem Server und zum Bereitstellen von Schema und Daten aus einem DACPAC-Paket.|
|/servers/databases/VulnerabilityAssessmentScans/action|Dient zum Ausführen einer Datenbanküberprüfung zur Bewertung von Sicherheitsrisiken.|
|/servers/databases/pause/action|Dient zum Anhalten einer Datenbank der DataWarehouse-Edition.|
|/servers/databases/resume/action|Dient zum Fortsetzen einer Datenbank der DataWarehouse-Edition.|
|/servers/databases/operationResults/read|Vorgang zum Nachverfolgen des Status von Datenbankvorgängen mit langer Ausführungsdauer (beispielsweise Skalierungsvorgänge).|
|/servers/databases/replicationLinks/read|Dient zum Zurückgeben von Details zu Replikationslinks, die für eine bestimmte Datenbank eingerichtet wurden.|
|/servers/databases/replicationLinks/delete|Dient zum Erzwingen der Beendigung der Replikationsbeziehung mit potenziellem Datenverlust.|
|/servers/databases/replicationLinks/unlink/action|Dient zum Beenden der Replikationsbeziehung – entweder erzwungen oder nach der Synchronisierung mit dem Partner.|
|/servers/databases/replicationLinks/failover/action|Dient zum Ausführen eines Failovers nach der Synchronisierung aller Änderungen der primären Datenbank. Dadurch wird diese Datenbank zur primären Datenbank der Replikationsbeziehung, und die primäre Remotedatenbank wird zu einer sekundären Datenbank.|
|/servers/databases/replicationLinks/forceFailoverAllowDataLoss/action|Dient zum Ausführen eines sofortigen Failovers mit potenziellem Datenverlust. Dadurch wird diese Datenbank zur primären Datenbank der Replikationsbeziehung, und die primäre Remotedatenbank wird zu einer sekundären Datenbank.|
|/servers/databases/replicationLinks/updateReplicationMode/action|Dient zum Aktualisieren des Replikationsmodus für die Verknüpfung auf den synchronen oder asynchronen Modus.|
|/servers/databases/replicationLinks/operationResults/read|Dient zum Abrufen des Status von Vorgängen mit langer Ausführungsdauer für Datenbankreplikationslinks.|
|/servers/databases/dataMaskingPolicies/read|Dient zum Abrufen von Details zur Datenmaskierungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist.|
|/servers/databases/dataMaskingPolicies/write|Dient zum Ändern der Datenmaskierungsrichtlinie für eine bestimmte Datenbank.|
|/servers/databases/dataMaskingPolicies/rules/read|Dient zum Abrufen von Details zur Datenmaskierungsrichtlinien-Regel, die für eine bestimmte Datenbank konfiguriert ist.|
|/servers/databases/dataMaskingPolicies/rules/write|Dient zum Ändern von Datenmaskierungsrichtlinen-Regeln für eine bestimmte Datenbank.|
|/servers/databases/securityAlertPolicies/read|Dient zum Abrufen von Details zur Bedrohungserkennungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist.|
|/servers/databases/securityAlertPolicies/write|Dient zum Ändern der Bedrohungserkennungsrichtlinie für eine bestimmte Datenbank.|
|/servers/databases/providers/Microsoft.Insights/<br>metricDefinitions/read|Dient zum Zurückgeben verfügbarer Metriktypen für Datenbanken.|
|/servers/databases/providers/Microsoft.Insights/<br>diagnosticSettings/read|Ruft die Diagnoseeinstellung für die Ressource ab.|
|/servers/databases/providers/Microsoft.Insights/<br>diagnosticSettings/write|Erstellt oder aktualisiert die Diagnoseeinstellung für die Ressource.|
|/servers/databases/providers/Microsoft.Insights/<br>logDefinitions/read|Ruft die verfügbaren Protokolle für Datenbanken ab.|
|/servers/databases/topQueries/read|Gibt aggregierte Laufzeitstatistiken für die ausgewählte Abfrage im ausgewählten Zeitraum zurück.|
|/servers/databases/topQueries/queryText/read|Gibt den Transact-SQL-Text für die ausgewählte Abfrage-ID zurück.|
|/servers/databases/topQueries/statistics/read|Gibt aggregierte Laufzeitstatistiken für die ausgewählte Abfrage im ausgewählten Zeitraum zurück.|
|/servers/databases/connectionPolicies/read|Dient zum Abrufen von Details zur Verbindungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist.|
|/servers/databases/connectionPolicies/write|Dient zum Ändern der Verbindungsrichtlinie für eine bestimmte Datenbank.|
|/servers/databases/metrics/read|Dient zum Zurückgeben von Ressourcenverwendungsmetriken für Datenbanken.|
|/servers/databases/auditRecords/read|Dient zum Abrufen der Datensätze für die Datenbankblobüberwachung.|
|/servers/databases/transparentDataEncryption/read|Dient zum Abrufen von Status und Details des Transparent Data Encryption-Sicherheitsfeatures für eine bestimmte Datenbank.|
|/servers/databases/transparentDataEncryption/write|Dient zum Aktivieren oder Deaktivieren von Transparent Data Encryption für eine bestimmte Datenbank.|
|/servers/databases/transparentDataEncryption/operationResults/read|Dient zum Abrufen von Status und Details des Transparent Data Encryption-Sicherheitsfeatures für eine bestimmte Datenbank.|
|/servers/databases/auditingPolicies/read|Dient zum Abrufen von Details zur Tabellenüberwachungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist.|
|/servers/databases/auditingPolicies/write|Dient zum Ändern der Tabellenüberwachungsrichtlinie für eine bestimmte Datenbank.|
|/servers/databases/dataWarehouseQueries/read|Gibt die Data Warehouse-Verteilungsabfrageinformationen für die ausgewählte Abfrage-ID zurück.|
|/servers/databases/dataWarehouseQueries/<br>dataWarehouseQuerySteps/read|Gibt die verteilten Abfrageschrittinformationen von Data Warehouse-Abfragen für die ausgewählte Schritt-ID zurück.|
|/servers/databases/serviceTierAdvisors/read|Gibt Vorschläge zum zentralen Hoch- oder Herunterskalieren von Datenbanken auf der Grundlage von Abfrageausführungsstatistiken zurück, um die Leistung zu verbessern oder die Kosten zu verringern.|
|/servers/databases/advisors/read|Gibt eine Liste mit verfügbaren Ratgebern für die Datenbank zurück.|
|/servers/databases/advisors/write|Dient zum Aktualisieren des Status der automatischen Ausführung eines Ratgebers auf der Datenbankebene.|
|/servers/databases/advisors/recommendedActions/read|Gibt eine Liste mit empfohlenen Aktionen des angegebenen Ratgebers für die Datenbank zurück.|
|/servers/databases/advisors/recommendedActions/write|Dient zum Anwenden der empfohlenen Aktion auf die Datenbank.|
|/servers/databases/usages/read|Dient zum Zurückgeben der maximal erreichbaren Datenbankgröße und der aktuellen Belegung durch Daten.|
|/servers/databases/queryStore/read|Gibt die aktuellen Werte von Abfragespeichereinstellungen für die Datenbank zurück.|
|/servers/databases/queryStore/write|Aktualisiert die Abfragespeichereinstellung für die Datenbank.|
|/servers/databases/auditingSettings/read|Dient zum Abrufen von Details zur Blobüberwachungsrichtlinie, die für eine bestimmte Datenbank konfiguriert ist.|
|/servers/databases/auditingSettings/write|Dient zum Ändern der Blobüberwachungsrichtlinie für eine bestimmte Datenbank.|
|/servers/databases/schemas/tables/recommendedIndexes/read|Dient zum Abrufen einer Liste mit Indexempfehlungen für eine Datenbank.|
|/servers/databases/schemas/tables/recommendedIndexes/write|Dient zum Anwenden von Indexempfehlungen.|
|/servers/databases/schemas/tables/columns/read|Dient zum Abrufen einer Liste mit Tabellenspalten.|
|/servers/databases/missingindexes/read|Dient zum Zurückgeben von Erstellungs-, Änderungs- oder Löschvorschlägen für Indizes zur Verbesserung der Abfrageleistung.|
|/servers/databases/missingindexes/write|Dient zum Verwenden von Datenbankindexempfehlungen in einer bestimmten Datenbank.|
|/servers/databases/importExportOperationResults/read|Dient zum Zurückgeben von Details zu Datenbankimportvorgängen oder -exportvorgängen aus DACPAC im Speicherkonto.|
|/servers/importExportOperationResults/read|Dient zum Zurückgeben der Liste mit Details zu Datenbankimportvorgängen aus einem Speicherkonto auf einem bestimmten Server.|

## <a name="microsoftstorage"></a>Microsoft.Storage

| Vorgang | Beschreibung |
|---|---|
|/register/action|Registriert das Abonnement für den Speicherressourcenanbieter und ermöglicht die Erstellung von Speicherkonten.|
|/checknameavailability/read|Prüft, ob der Kontoname gültig ist und noch nicht verwendet wird.|
|/storageAccounts/write|Erstellt ein Speicherkonto mit den angegebenen Parametern oder aktualisiert die Eigenschaften oder Tags oder fügt eine benutzerdefinierte Domäne zum angegebenen Speicherkonto hinzu.|
|/storageAccounts/delete|Löscht ein vorhandenes Speicherkonto.|
|/storageAccounts/listkeys/action|Gibt die Zugriffsschlüssel für das angegebene Speicherkonto zurück.|
|/storageAccounts/regeneratekey/action|Generiert die Zugriffsschlüssel für das angegebene Speicherkonto neu.|
|/storageAccounts/read|Gibt die Liste mit Speicherkonten zurück oder ruft die Eigenschaften für das angegebene Speicherkonto ab.|
|/storageAccounts/listAccountSas/action|Gibt das Konto-SAS-Token für das angegebene Speicherkonto zurück.|
|/storageAccounts/listServiceSas/action|Speicherdienst-SAS-Token|
|/storageAccounts/services/diagnosticSettings/write|Dient zum Erstellen/Aktualisieren von Speicherkonto-Diagnoseeinstellungen.|
|/skus/read|Listet die von Microsoft.Storage unterstützten SKUs auf.|
|/usages/read|Gibt den Grenzwert und den aktuellen Verwendungszähler für Ressourcen im angegebenen Abonnement zurück.|
|/operations/read|Fragt den Status eines asynchronen Vorgangs ab.|
|/locations/deleteVirtualNetworkOrSubnets/action|Benachrichtigt Microsoft.Storage darüber, dass das virtuelle Netzwerk oder Subnetz gelöscht wird.|

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

| Vorgang | Beschreibung |
|---|---|
|/managers/clearAlerts/action|Dient zum Deaktivieren aller Warnungen im Zusammenhang mit dem Geräte-Manager.|
|/managers/getActivationKey/action|Dient zum Abrufen des Aktivierungsschlüssels für den Geräte-Manager.|
|/managers/regenerateActivationKey/action|Dient zum erneuten Generieren des Aktivierungsschlüssels für den Geräte-Manager.|
|/managers/regenarateRegistationCertificate/action|Dient zum erneuten Generieren des Registrierungszertifikats für den Geräte-Manager.|
|/managers/getEncryptionKey/action|Dient zum Abrufen des Verschlüsselungsschlüssels für den Geräte-Manager.|
|/managers/read|Listet die Geräte-Manager auf oder ruft sie ab.|
|/managers/delete|Löscht die Geräte-Manager.|
|/managers/write|Dient zum Erstellen oder Aktualisieren der Geräte-Manager.|
|/managers/configureDevice/action|Konfiguriert ein Gerät.|
|/managers/listActivationKey/action|Ruft den Aktivierungsschlüssel des StorSimple-Geräte-Managers ab.|
|/managers/listPublicEncryptionKey/action|Dient zum Auflisten öffentlicher Verschlüsselungsschlüssel eines StorSimple-Geräte-Managers.|
|/managers/listPrivateEncryptionKey/action|Ruft den privaten Verschlüsselungsschlüssel für einen StorSimple-Geräte-Manager ab.|
|/managers/provisionCloudAppliance/action|Dient zum Erstellen eines neuen Cloudgeräts.|
|/Managers/write|Der Vorgang „Tresor erstellen“ erstellt eine Azure-Ressource vom Typ „Tresor“.|
|/Managers/read|Der Vorgang „Tresor abrufen“ ruft ein Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt.|
|/Managers/delete|Der Vorgang „Tresor löschen“ löscht die angegebene Azure-Ressource vom Typ „Tresor“.|
|/managers/storageAccountCredentials/write|Dient zum Erstellen oder Aktualisieren der Speicherkonto-Anmeldedaten.|
|/managers/storageAccountCredentials/read|Listet die Speicherkonto-Anmeldedaten auf oder ruft sie ab.|
|/managers/storageAccountCredentials/delete|Löscht die Speicherkonto-Anmeldedaten.|
|/managers/storageAccountCredentials/listAccessKey/action|Dient zum Auflisten von Zugriffsschlüsseln der Speicherkonto-Anmeldedaten.|
|/managers/accessControlRecords/read|Listet die Access Control-Datensätze auf oder ruft sie ab.|
|/managers/accessControlRecords/write|Dient zum Erstellen oder Aktualisieren der Access Control-Datensätze.|
|/managers/accessControlRecords/delete|Löscht die Access Control-Datensätze.|
|/managers/metrics/read|Listet die Metriken auf oder ruft sie ab.|
|/managers/bandwidthSettings/read|Listet die Bandbreiteneinstellungen auf (nur 8000er Serie).|
|/managers/bandwidthSettings/write|Erstellt eine neue Bandbreiteneinstellung oder aktualisiert Bandbreiteneinstellungen (nur 8000er Serie).|
|/managers/bandwidthSettings/delete|Löscht vorhandene Bandbreiteneinstellungen (nur 8000er Serie).|
|/Managers/extendedInformation/read|Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt.|
|/Managers/extendedInformation/write|Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt.|
|/Managers/extendedInformation/delete|Der Vorgang „Ausführliche Informationen abrufen“ ruft die ausführlichen Informationen zu einem Objekt ab, das die Azure-Ressource vom Typ „Tresor“ darstellt.|
|/managers/alerts/read|Listet die Warnungen auf oder ruft sie ab.|
|/managers/storageDomains/read|Listet die Speicherdomänen auf oder ruft sie ab.|
|/managers/storageDomains/write|Dient zum Erstellen oder Aktualisieren der Speicherdomänen.|
|/managers/storageDomains/delete|Löscht die Speicherdomänen.|
|/managers/devices/scanForUpdates/action|Dient zum Suchen nach Updates auf einem Gerät.|
|/managers/devices/download/action|Dient zum Herunterladen von Updates für ein Gerät.|
|/managers/devices/install/action|Dient zum Installieren von Updates auf einem Gerät.|
|/managers/devices/read|Listet die Geräte auf oder ruft sie ab.|
|/managers/devices/write|Dient zum Erstellen oder Aktualisieren der Geräte.|
|/managers/devices/delete|Löscht die Geräte.|
|/managers/devices/deactivate/action|Deaktiviert ein Gerät.|
|/managers/devices/publishSupportPackage/action|Dient zum Veröffentlichen des Supportpakets eines Geräts für die Problembehandlung durch den Microsoft-Support.|
|/managers/devices/failover/action|Dient zum Ausführen eines Failovers für das Gerät.|
|/managers/devices/sendTestAlertEmail/action|Dient zum Senden einer Testwarnungs-E-Mail an konfigurierte E-Mail-Empfänger.|
|/managers/devices/installUpdates/action|Installiert Updates auf den Geräten.|
|/managers/devices/listFailoverSets/action|Dient zum Auflisten der Failovergruppen für ein vorhandenes Gerät.|
|/managers/devices/listFailoverTargets/action|Dient zum Auflisten von Failoverzielen der Geräte.|
|/managers/devices/publicEncryptionKey/action|Dient zum Auflisten öffentlicher Verschlüsselungsschlüssel des Geräte-Managers.|
|/managers/devices/hardwareComponentGroups/<br>read|Dient zum Auflisten der Hardwarekomponentengruppen.|
|/managers/devices/hardwareComponentGroups/<br>changeControllerPowerState/action|Dient zum Ändern des Controllerenergiezustands von Hardwarekomponentengruppen.|
|/managers/devices/metrics/read|Listet die Metriken auf oder ruft sie ab.|
|/managers/devices/chapSettings/write|Dient zum Erstellen oder Aktualisieren der CHAP-Einstellungen.|
|/managers/devices/chapSettings/read|Listet die CHAP-Einstellungen auf oder ruft sie ab.|
|/managers/devices/chapSettings/delete|Löscht die CHAP-Einstellungen.|
|/managers/devices/backupScheduleGroups/read|Listet die Sicherungszeitplangruppen auf oder ruft sie ab.|
|/managers/devices/backupScheduleGroups/write|Dient zum Erstellen oder Aktualisieren der Sicherungszeitplangruppen.|
|/managers/devices/backupScheduleGroups/delete|Löscht die Sicherungszeitplangruppen.|
|/managers/devices/updateSummary/read|Listet die Updatezusammenfassung auf oder ruft sie ab.|
|/managers/devices/migrationSourceConfigurations/<br>import/action|Dient zum Importieren von Quellkonfigurationen für die Migration.|
|/managers/devices/migrationSourceConfigurations/<br>startMigrationEstimate/action|Dient zum Starten eines Auftrags zur Ermittlung der voraussichtlichen Dauer des Migrationsvorgangs.|
|/managers/devices/migrationSourceConfigurations/<br>startMigration/action|Dient zum Starten der Migration unter Verwendung von Quellkonfigurationen.|
|/managers/devices/migrationSourceConfigurations/<br>confirmMigration/action|Bestätigt eine erfolgreiche Migration und committet sie.|
|/managers/devices/migrationSourceConfigurations/<br>fetchMigrationEstimate/action|Dient zum Abrufen des Status für den Migrationsschätzauftrag.|
|/managers/devices/migrationSourceConfigurations/<br>fetchMigrationStatus/action|Dient zum Abrufen des Status für die Migration.|
|/managers/devices/migrationSourceConfigurations/<br>fetchConfirmMigrationStatus/action|Dient zum Abrufen des Migrationsbestätigungsstatus.|
|/managers/devices/alertSettings/read|Listet die Warnungseinstellungen auf oder ruft sie ab.|
|/managers/devices/alertSettings/write|Dient zum Erstellen oder Aktualisieren der Warnungseinstellungen.|
|/managers/devices/networkSettings/read|Listet die Netzwerkeinstellungen auf oder ruft sie ab.|
|/managers/devices/networkSettings/write|Erstellt eine neue Netzwerkeinstellung oder aktualisiert Netzwerkeinstellungen.|
|/managers/devices/jobs/read|Listet die Aufträge auf oder ruft sie ab.|
|/managers/devices/jobs/cancel/action|Dient zum Abbrechen eines ausgeführten Auftrags.|
|/managers/devices/metricsDefinitions/read|Listet die Metrikdefinitionen auf oder ruft sie ab.|
|/managers/devices/volumeContainers/write|Erstellt einen neuen Volumecontainer oder aktualisiert Volumecontainer (nur 8000er Serie).|
|/managers/devices/volumeContainers/read|Dient zum Auflisten der Volumecontainer (nur 8000er Serie).|
|/managers/devices/volumeContainers/delete|Löscht vorhandene Volumecontainer (nur 8000er Serie).|
|/managers/devices/volumeContainers/listEncryptionKeys/action|Dient zum Auflisten von Verschlüsselungsschlüsseln von Volumecontainern.|
|/managers/devices/volumeContainers/rolloverEncryptionKey/action|Dient zum Ausführen eines Verschlüsselungsschlüssel-Rollovers für Volumecontainer.|
|/managers/devices/volumeContainers/metrics/read|Dient zum Auflisten der Metriken.|
|/managers/devices/volumeContainers/volumes/read|Dient zum Auflisten der Volumes.|
|/managers/devices/volumeContainers/volumes/write|Erstellt ein neues Volume oder aktualisiert Volumes.|
|/managers/devices/volumeContainers/volumes/delete|Löscht vorhandene Volumes.|
|/managers/devices/volumeContainers/volumes/metrics/read|Dient zum Auflisten der Metriken.|
|/managers/devices/volumeContainers/volumes/metricsDefinitions/read|Dient zum Auflisten der Metrikdefinitionen.|
|/managers/devices/volumeContainers/metricsDefinitions/read|Dient zum Auflisten der Metrikdefinitionen.|
|/managers/devices/iscsiservers/read|Listet die iSCSI-Server auf oder ruft sie ab.|
|/managers/devices/iscsiservers/write|Dient zum Erstellen oder Aktualisieren der iSCSI-Server.|
|/managers/devices/iscsiservers/delete|Löscht die iSCSI-Server.|
|/managers/devices/iscsiservers/backup/action|Dient zum Erstellen einer Sicherung eines iSCSI-Servers.|
|/managers/devices/iscsiservers/metrics/read|Listet die Metriken auf oder ruft sie ab.|
|/managers/devices/iscsiservers/disks/read|Listet die Datenträger auf oder ruft sie ab.|
|/managers/devices/iscsiservers/disks/write|Dient zum Erstellen oder Aktualisieren der Datenträger.|
|/managers/devices/iscsiservers/disks/delete|Löscht die Datenträger.|
|/managers/devices/iscsiservers/disks/metrics/read|Listet die Metriken auf oder ruft sie ab.|
|/managers/devices/iscsiservers/disks/metricsDefinitions/read|Listet die Metrikdefinitionen auf oder ruft sie ab.|
|/managers/devices/iscsiservers/metricsDefinitions/read|Listet die Metrikdefinitionen auf oder ruft sie ab.|
|/managers/devices/backups/read|Listet den Sicherungssatz auf oder ruft ihn ab.|
|/managers/devices/backups/delete|Löscht den Sicherungssatz.|
|/managers/devices/backups/restore/action|Dient zum Wiederherstellen aller Volumes aus einem Sicherungssatz.|
|/managers/devices/backups/elements/clone/action|Dient zum Klonen einer Freigabe oder eines Volumes unter Verwendung eines Sicherungselements.|
|/managers/devices/backupPolicies/write|Erstellt eine neue Sicherungsrichtlinie oder aktualisiert vorhandene Sicherungsrichtlinien (nur 8000er Serie).|
|/managers/devices/backupPolicies/read|Dient zum Auflisten der Sicherungsrichtlinien (nur 8000er Serie).|
|/managers/devices/backupPolicies/delete|Löscht vorhandene Sicherungsrichtlinien (nur 8000er Serie).|
|/managers/devices/backupPolicies/backup/action|Dient zum Erstellen einer manuellen Sicherung, um eine bedarfsgesteuerte Sicherung aller durch die Richtlinie geschützten Volumes zu erstellen.|
|/managers/devices/backupPolicies/schedules/write|Erstellt einen neuen Zeitplan oder aktualisiert Zeitpläne.|
|/managers/devices/backupPolicies/schedules/read|Dient zum Auflisten der Zeitpläne.|
|/managers/devices/backupPolicies/schedules/delete|Löscht vorhandene Zeitpläne.|
|/managers/devices/securitySettings/update/action|Dient zum Aktualisieren der Sicherheitseinstellungen.|
|/managers/devices/securitySettings/read|Dient zum Auflisten der Sicherheitseinstellungen.|
|/managers/devices/securitySettings/<br>syncRemoteManagementCertificate/action|Dient zum Synchronisieren des Remoteverwaltungszertifikats für ein Gerät.|
|/managers/devices/securitySettings/write|Erstellt eine neue Sicherheitseinstellung oder aktualisiert Sicherheitseinstellungen.|
|/managers/devices/fileservers/read|Listet die Dateiserver auf oder ruft sie ab.|
|/managers/devices/fileservers/write|Dient zum Erstellen oder Aktualisieren der Dateiserver.|
|/managers/devices/fileservers/delete|Löscht die Dateiserver.|
|/managers/devices/fileservers/backup/action|Dient zum Erstellen einer Sicherung eines Dateiservers.|
|/managers/devices/fileservers/metrics/read|Listet die Metriken auf oder ruft sie ab.|
|/managers/devices/fileservers/shares/write|Dient zum Erstellen oder Aktualisieren der Freigaben.|
|/managers/devices/fileservers/shares/read|Listet die Freigaben auf oder ruft sie ab.|
|/managers/devices/fileservers/shares/delete|Löscht die Freigaben.|
|/managers/devices/fileservers/shares/metrics/read|Listet die Metriken auf oder ruft sie ab.|
|/managers/devices/fileservers/shares/metricsDefinitions/read|Listet die Metrikdefinitionen auf oder ruft sie ab.|
|/managers/devices/fileservers/metricsDefinitions/read|Listet die Metrikdefinitionen auf oder ruft sie ab.|
|/managers/devices/timeSettings/read|Listet die Uhrzeiteinstellungen auf oder ruft sie ab.|
|/managers/devices/timeSettings/write|Erstellt eine neue Uhrzeiteinstellung oder aktualisiert Uhrzeiteinstellungen.|
|/Managers/certificates/write|Der Vorgang „Ressourcenzertifikat aktualisieren“ aktualisiert das Zertifikat für die Ressourcen-/Tresoranmeldeinformationen.|
|/managers/cloudApplianceConfigurations/read|Dient zum Auflisten der vom Cloudgerät unterstützten Konfigurationen.|
|/managers/metricsDefinitions/read|Listet die Metrikdefinitionen auf oder ruft sie ab.|
|/managers/encryptionSettings/read|Listet die Verschlüsselungseinstellungen auf oder ruft sie ab.|

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

| Vorgang | Beschreibung |
|---|---|
|/streamingjobs/Start/action|Dient zum Starten von Stream Analytics-Aufträgen.|
|/streamingjobs/Stop/action|Dient zum Beenden von Stream Analytics-Aufträgen.|
|/streamingjobs/Read|Dient zum Lesen von Stream Analytics-Aufträgen.|
|/streamingjobs/Write|Dient zum Schreiben von Stream Analytics-Aufträgen.|
|/streamingjobs/Delete|Dient zum Löschen von Stream Analytics-Aufträgen.|
|/streamingjobs/providers/Microsoft.Insights/metricDefinitions/read|Ruft die verfügbaren Metriken für Streamingaufträge ab.|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/read|Dient zum Lesen der Diagnoseeinstellung.|
|/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/write|Dient zum Schreiben der Diagnoseeinstellung.|
|/streamingjobs/providers/Microsoft.Insights/logDefinitions/read|Ruft die verfügbaren Protokolle für Streamingaufträge ab.|
|/streamingjobs/transformations/Read|Dient zum Lesen von Stream Analytics-Auftragstransformationen.|
|/streamingjobs/transformations/Write|Dient zum Schreiben von Stream Analytics-Auftragstransformationen.|
|/streamingjobs/transformations/Delete|Dient zum Löschen von Stream Analytics-Auftragstransformationen.|
|/streamingjobs/inputs/Read|Dient zum Lesen von Stream Analytics-Auftragseingaben.|
|/streamingjobs/inputs/Write|Dient zum Schreiben von Stream Analytics-Auftragseingaben.|
|/streamingjobs/inputs/Delete|Dient zum Löschen von Stream Analytics-Auftragseingaben.|
|/streamingjobs/outputs/Read|Dient zum Lesen von Stream Analytics-Auftragsausgaben.|
|/streamingjobs/outputs/Write|Dient zum Schreiben von Stream Analytics-Auftragsausgaben.|
|/streamingjobs/outputs/Delete|Dient zum Löschen von Stream Analytics-Auftragsausgaben.|

## <a name="microsoftsupport"></a>Microsoft.Support

| Vorgang | Beschreibung |
|---|---|
|/register/action|Führt die Registrierung beim Supportressourcenanbieter durch.|
|/supportTickets/read|Ruft Details zu Supporttickets (einschließlich Status, Schweregrad, Kontaktdetails und Kommunikation) oder die Liste mit Supporttickets (abonnementübergreifend) ab.|
|/supportTickets/write|Erstellt oder aktualisiert ein Supportticket. Sie können ein Supportticket zu technischen Problemen bzw. zu Problemen im Zusammenhang mit Abrechnung, Kontingenten oder Abonnementverwaltung erstellen. Der Schweregrad, die Kontaktinformationen und die Kommunikation für vorhandene Supporttickets können aktualisiert werden.|

## <a name="microsoftweb"></a>Microsoft.Web

| Vorgang | Beschreibung |
|---|---|
|/unregister/action|Dient zum Aufheben der Registrierung des Microsoft.Web-Ressourcenanbieters für das Abonnement.|
|/validate/action|Dient zur Überprüfung.|
|/register/action|Dient zum Registrieren des Microsoft.Web-Ressourcenanbieters für das Abonnement.|
|/hostingEnvironments/Read|Dient zum Abrufen der Eigenschaften einer App Service-Umgebung.|
|/hostingEnvironments/Write|Dient zum Erstellen einer neuen App Service-Umgebung oder zum Aktualisieren einer bereits vorhandenen.|
|/hostingEnvironments/Delete|Dient zum Löschen einer App Service-Umgebung.|
|/hostingEnvironments/reboot/Action|Dient zum Neustarten aller Computer in einer App Service-Umgebung.|
|/hostingenvironments/resume/action|Dient zum Fortsetzen von Hostingumgebungen.|
|/hostingenvironments/suspend/action|Dient zum Anhalten von Hostingumgebungen.|
|/hostingenvironments/metricdefinitions/read|Dient zum Abrufen von Metrikdefinitionen für Hostingumgebungen.|
|/hostingEnvironments/workerPools/Read|Dient zum Abrufen der Eigenschaften eines Workerpools in einer App Service-Umgebung.|
|/hostingEnvironments/workerPools/Write|Dient zum Erstellen eines neuen Workerpools in einer App Service-Umgebung oder zum Aktualisieren eines bereits vorhandenen.|
|/hostingenvironments/workerpools/metricdefinitions/read|Dient zum Abrufen von Workerpool-Metrikdefinitionen für Hostingumgebungen.|
|/hostingenvironments/workerpools/metrics/read|Dient zum Abrufen von Workerpoolmetriken für Hostingumgebungen.|
|/hostingenvironments/workerpools/skus/read|Dient zum Abrufen von Workerpool-SKUs für Hostingumgebungen.|
|/hostingenvironments/workerpools/usages/read|Dient zum Abrufen von Workerpoolverwendungen für Hostingumgebungen.|
|/hostingenvironments/sites/read|Dient zum Abrufen von Web-Apps für Hostingumgebungen.|
|/hostingenvironments/serverfarms/read|Dient zum Abrufen von App Service-Plänen für Hostingumgebungen.|
|/hostingenvironments/usages/read|Dient zum Abrufen von Hostingumgebungsverwendungen.|
|/hostingenvironments/capacities/read|Dient zum Abrufen von Hostingumgebungskapazitäten.|
|/hostingenvironments/operations/read|Dient zum Abrufen von Hostingumgebungsvorgängen.|
|/hostingEnvironments/multiRolePools/Read|Dient zum Abrufen der Eigenschaften eines FrontEnd-Pools in einer App Service-Umgebung.|
|/hostingEnvironments/multiRolePools/Write|Dient zum Erstellen eines neuen FrontEnd-Pools in einer App Service-Umgebung oder zum Aktualisieren eines bereits vorhandenen.|
|/hostingenvironments/multirolepools/metricdefinitions/read|Dient zum Abrufen von MultiRole-Poolmetrikdefinitionen für Hostingumgebungen.|
|/hostingenvironments/multirolepools/metrics/read|Dient zum Abrufen von MultiRole-Poolmetriken für Hostingumgebungen.|
|/hostingenvironments/multirolepools/skus/read|Dient zum Abrufen von MultiRole-Pool-SKUs für Hostingumgebungen.|
|/hostingenvironments/multirolepools/usages/read|Dient zum Abrufen von MultiRole-Poolverwendungen für Hostingumgebungen.|
|/hostingenvironments/diagnostics/read|Dient zum Abrufen von Hostingumgebungsdiagnosen.|
|/publishingusers/read|Dient zum Abrufen von Veröffentlichungsbenutzern.|
|/publishingusers/write|Dient zum Aktualisieren von Veröffentlichungsbenutzern.|
|/checknameavailability/read|Dient zum Prüfen, ob ein Ressourcenname verfügbar ist.|
|/geoRegions/Read|Ruft die Liste mit geografischen Regionen ab.|
|/sites/Read|Dient zum Abrufen der Eigenschaften einer Web-App.|
|/sites/Write|Dient zum Erstellen einer neuen Web-App oder zum Aktualisieren einer bereits vorhandenen.|
|/sites/Delete|Dient zum Löschen einer vorhandenen Web-App.|
|/sites/backup/Action|Dient zum Erstellen einer neuen Web-App-Sicherung.|
|/sites/publishxml/Action|Dient zum Abrufen des Veröffentlichungsprofils (XML) für eine Web-App.|
|/sites/publish/Action|Dient zum Veröffentlichen einer Web-App.|
|/sites/restart/Action|Dient zum Neustarten einer Web-App.|
|/sites/start/Action|Dient zum Starten einer Web-App.|
|/sites/stop/Action|Dient zum Beenden einer Web-App.|
|/sites/slotsswap/Action|Dient zum Austauschen von Web-App-Bereitstellungsslots.|
|/sites/slotsdiffs/Action|Dient zum Abrufen von Konfigurationsunterschieden zwischen Web-App und Slots.|
|/sites/applySlotConfig/Action|Dient zum Anwenden der Web-App-Slotkonfiguration aus dem Zielslot auf die aktuelle Web-App.|
|/sites/resetSlotConfig/Action|Dient zum Zurücksetzen der Web-App-Konfiguration.|
|/sites/functions/action|Functions-Web-Apps|
|/sites/listsyncfunctiontriggerstatus/action|Web-Apps zum Auflisten des Triggerstatus der Synchronisierungsfunktion|
|/sites/networktrace/action|Netzwerkablaufverfolgungs-Web-Apps|
|/sites/newpassword/action|NewPassword-Web-Apps|
|/sites/sync/action|Synchronisierungs-Web-Apps|
|/sites/operationresults/read|Dient zum Abrufen der Ergebnisse von Web-App-Vorgängen.|
|/sites/webjobs/read|Dient zum Abrufen von Webaufträgen für Web-Apps.|
|/sites/backup/read|Dient zum Abrufen der Web-App-Sicherung.|
|/sites/backup/write|Dient zum Aktualisieren der Web-App-Sicherung.|
|/sites/metricdefinitions/read|Dient zum Abrufen von Metrikdefinitionen für Web-Apps.|
|/sites/metrics/read|Dient zum Abrufen von Web-App-Metriken.|
|/sites/continuouswebjobs/delete|Dient zum Löschen fortlaufender Webaufträge für Web-Apps.|
|/sites/continuouswebjobs/read|Dient zum Abrufen fortlaufender Webaufträge für Web-Apps.|
|/sites/continuouswebjobs/start/action|Dient zum Starten fortlaufender Webaufträge für Web-Apps.|
|/sites/continuouswebjobs/stop/action|Dient zum Beenden fortlaufender Webaufträge für Web-Apps.|
|/sites/domainownershipidentifiers/read|Dient zum Abrufen von Domänenbesitz-IDs für Web-Apps.|
|/sites/domainownershipidentifiers/write|Dient zum Aktualisieren von Domänenbesitz-IDs für Web-Apps.|
|/sites/premieraddons/delete|Dient zum Löschen von Premier-Add-Ons für Web-Apps.|
|/sites/premieraddons/read|Dient zum Abrufen von Premier-Add-Ons für Web-Apps.|
|/sites/premieraddons/write|Dient zum Aktualisieren von Premier-Add-Ons für Web-Apps.|
|/sites/triggeredwebjobs/delete|Dient zum Löschen ausgelöster Webaufträge für Web-Apps.|
|/sites/triggeredwebjobs/read|Dient zum Abrufen ausgelöster Webaufträge für Web-Apps.|
|/sites/triggeredwebjobs/run/action|Dient zum Ausführen ausgelöster Webaufträge für Web-Apps.|
|/sites/hostnamebindings/delete|Dient zum Löschen von Hostnamenbindungen für Web-Apps.|
|/sites/hostnamebindings/read|Dient zum Abrufen von Hostnamenbindungen für Web-Apps.|
|/sites/hostnamebindings/write|Dient zum Aktualisieren von Hostnamenbindungen für Web-Apps.|
|/sites/virtualnetworkconnections/delete|Dient zum Löschen von Verbindungen mit virtuellen Netzwerken für Web-Apps.|
|/sites/virtualnetworkconnections/read|Dient zum Abrufen von Verbindungen mit virtuellen Netzwerken für Web-Apps.|
|/sites/virtualnetworkconnections/write|Dient zum Aktualisieren von Verbindungen mit virtuellen Netzwerken für Web-Apps.|
|/sites/virtualnetworkconnections/gateways/read|Dient zum Abrufen von Gateways für Verbindungen mit virtuellen Netzwerken für Web-Apps.|
|/sites/virtualnetworkconnections/gateways/write|Dient zum Aktualisieren von Gateways für Verbindungen mit virtuellen Netzwerken für Web-Apps.|
|/sites/publishxml/read|Dient zum Abrufen des XML-Codes für die Veröffentlichung von Web-Apps.|
|/sites/hybridconnectionrelays/read|Dient zum Abrufen von Hybridverbindungsrelays für Web-Apps.|
|/sites/perfcounters/read|Dient zum Abrufen von Leistungsindikatoren für Web-Apps.|
|/sites/usages/read|Dient zum Abrufen von Web-App-Verwendungen.|
|/sites/slots/Write|Dient zum Erstellen eines neuen Web-App-Slots oder zum Aktualisieren eines bereits vorhandenen.|
|/sites/slots/Delete|Dient zum Löschen eines vorhandenen Web-App-Slots.|
|/sites/slots/backup/Action|Dient zum Erstellen einer neuen Web-App-Slotsicherung.|
|/sites/slots/publishxml/Action|Dient zum Abrufen des Veröffentlichungsprofils (XML) für einen Web-App-Slot.|
|/sites/slots/publish/Action|Dient zum Veröffentlichen eines Web-App-Slots.|
|/sites/slots/restart/Action|Dient zum Neustarten eines Web-App-Slots.|
|/sites/slots/start/Action|Dient zum Starten eines Web-App-Slots.|
|/sites/slots/stop/Action|Dient zum Beenden eines Web-App-Slots.|
|/sites/slots/slotsswap/Action|Dient zum Austauschen von Web-App-Bereitstellungsslots.|
|/sites/slots/slotsdiffs/Action|Dient zum Abrufen von Konfigurationsunterschieden zwischen Web-App und Slots.|
|/sites/slots/applySlotConfig/Action|Dient zum Anwenden der Web-App-Slotkonfiguration aus dem Zielslot auf den aktuellen Slot.|
|/sites/slots/resetSlotConfig/Action|Dient zum Zurücksetzen der Web-App-Slotkonfiguration.|
|/sites/slots/Read|Dient zum Abrufen der Eigenschaften eines Web-App-Bereitstellungsslots.|
|/sites/slots/newpassword/action|NewPassword-Web-App-Slots|
|/sites/slots/sync/action|Dient zum Synchronisieren von Web-App-Slots.|
|/sites/slots/operationresults/read|Dient zum Abrufen der Ergebnisse von Web-App-Slotvorgängen.|
|/sites/slots/webjobs/read|Dient zum Abrufen von Webaufträgen für Web-App-Slots.|
|/sites/slots/backup/write|Dient zum Aktualisieren der Web-App-Slotsicherung.|
|/sites/slots/metricdefinitions/read|Dient zum Abrufen von Metrikdefinitionen für Web-App-Slots.|
|/sites/slots/metrics/read|Dient zum Abrufen von Metriken für Web-App-Slots.|
|/sites/slots/continuouswebjobs/delete|Dient zum Löschen fortlaufender Webaufträge für Web-App-Slots.|
|/sites/slots/continuouswebjobs/read|Dient zum Abrufen fortlaufender Webaufträge für Web-App-Slots.|
|/sites/slots/continuouswebjobs/start/action|Dient zum Starten fortlaufender Webaufträge für Web-App-Slots.|
|/sites/slots/continuouswebjobs/stop/action|Dient zum Beenden fortlaufender Webaufträge für Web-App-Slots.|
|/sites/slots/premieraddons/delete|Dient zum Löschen von Premier-Add-Ons für Web-App-Slots.|
|/sites/slots/premieraddons/read|Dient zum Abrufen von Premier-Add-Ons für Web-App-Slots.|
|/sites/slots/premieraddons/write|Dient zum Aktualisieren von Premier-Add-Ons für Web-App-Slots.|
|/sites/slots/triggeredwebjobs/delete|Dient zum Löschen ausgelöster Webaufträge für Web-App-Slots.|
|/sites/slots/triggeredwebjobs/read|Dient zum Abrufen ausgelöster Webaufträge für Web-App-Slots.|
|/sites/slots/triggeredwebjobs/run/action|Dient zum Ausführen ausgelöster Webaufträge für Web-App-Slots.|
|/sites/slots/hostnamebindings/delete|Dient zum Löschen von Hostnamenbindungen für Web-App-Slots.|
|/sites/slots/hostnamebindings/read|Dient zum Abrufen von Hostnamenbindungen für Web-App-Slots.|
|/sites/slots/hostnamebindings/write|Dient zum Aktualisieren von Hostnamenbindungen für Web-App-Slots.|
|/sites/slots/phplogging/read|Dient zum Abrufen der PHP-Protokollierung für Web-App-Slots.|
|/sites/slots/virtualnetworkconnections/delete|Dient zum Löschen von Verbindungen mit virtuellen Netzwerken für Web-App-Slots.|
|/sites/slots/virtualnetworkconnections/read|Dient zum Abrufen von Verbindungen mit virtuellen Netzwerken für Web-App-Slots.|
|/sites/slots/virtualnetworkconnections/write|Dient zum Aktualisieren von Verbindungen mit virtuellen Netzwerken für Web-App-Slots.|
|/sites/slots/virtualnetworkconnections/gateways/write|Dient zum Aktualisieren von Gateways für Verbindungen mit virtuellen Netzwerken für Web-App-Slots.|
|/sites/slots/usages/read|Dient zum Abrufen von Verwendungen für Web-App-Slots.|
|/sites/slots/hybridconnection/delete|Dient zum Löschen der Hybridverbindung für Web-App-Slots.|
|/sites/slots/hybridconnection/read|Dient zum Abrufen der Hybridverbindung für Web-App-Slots.|
|/sites/slots/hybridconnection/write|Dient zum Aktualisieren der Hybridverbindung für Web-App-Slots.|
|/sites/slots/config/Read|Dient zum Abrufen der Konfigurationseinstellungen des Web-App-Slots.|
|/sites/slots/config/list/Action|Dient zum Auflisten der sicherheitsrelevanten Einstellungen des Web-App-Slots. Hierzu zählen etwa Veröffentlichungsanmeldeinformationen, App-Einstellungen und Verbindungszeichenfolgen.|
|/sites/slots/config/Write|Dient zum Aktualisieren der Konfigurationseinstellungen des Web-App-Slots.|
|/sites/slots/config/delete|Dient zum Löschen der Konfiguration von Web-App-Slots.|
|/sites/slots/instances/read|Dient zum Abrufen von Web-App-Slotinstanzen.|
|/sites/slots/instances/processes/read|Dient zum Abrufen von Prozessen für Web-App-Slotinstanzen.|
|/sites/slots/instances/deployments/read|Dient zum Abrufen von Bereitstellungen für Web-App-Slotinstanzen.|
|/sites/slots/sourcecontrols/Read|Dient zum Abrufen der Konfigurationseinstellungen für die Quellcodeverwaltung des Web-App-Slots.|
|/sites/slots/sourcecontrols/Write|Dient zum Aktualisieren der Konfigurationseinstellungen für die Quellcodeverwaltung des Web-App-Slots.|
|/sites/slots/sourcecontrols/Delete|Dient zum Löschen der Konfigurationseinstellungen für die Quellcodeverwaltung des Web-App-Slots.|
|/sites/slots/restore/read|Dient zum Abrufen der Wiederherstellung für Web-App-Slots.|
|/sites/slots/analyzecustomhostname/read|Dient zum Abrufen des benutzerdefinierten Hostnamens für die Analyse von Web-Apps-Slots.|
|/sites/slots/backups/Read|Dient zum Abrufen der Eigenschaften einer Sicherung von Web-App-Slots.|
|/sites/slots/backups/list/action|Dient zum Auflisten von Web-App-Slotsicherungen.|
|/sites/slots/backups/restore/action|Dient zum Wiederherstellen von Web-App-Slotsicherungen.|
|/sites/slots/deployments/delete|Dient zum Löschen von Web-App-Slotbereitstellungen.|
|/sites/slots/deployments/read|Dient zum Abrufen von Web-App-Slotbereitstellungen.|
|/sites/slots/deployments/write|Dient zum Aktualisieren von Web-App-Slotbereitstellungen.|
|/sites/slots/deployments/log/read|Dient zum Abrufen des Protokolls für Web-App-Slotbereitstellungen.|
|/sites/hybridconnection/delete|Dient zum Löschen der Hybridverbindung für Web-Apps.|
|/sites/hybridconnection/read|Dient zum Abrufen der Hybridverbindung für Web-Apps.|
|/sites/hybridconnection/write|Dient zum Aktualisieren der Hybridverbindung für Web-Apps.|
|/sites/recommendationhistory/read|Dient zum Abrufen des Empfehlungsverlaufs für Web-Apps.|
|/sites/recommendations/Read|Dient zum Abrufen der Liste mit Empfehlungen für Web-Apps.|
|/sites/recommendations/disable/action|Dient zum Deaktivieren von Web-App-Empfehlungen.|
|/sites/config/Read|Dient zum Abrufen von Web-App-Konfigurationseinstellungen.|
|/sites/config/list/Action|Dient zum Auflisten der sicherheitsrelevanten Einstellungen der Web-App. Hierzu zählen etwa Veröffentlichungsanmeldeinformationen, App-Einstellungen und Verbindungszeichenfolgen.|
|/sites/config/Write|Dient zum Aktualisieren der Konfigurationseinstellungen der Web-App.|
|/sites/config/delete|Dient zum Löschen der Web-App-Konfiguration.|
|/sites/instances/read|Dient zum Abrufen von Web-App-Instanzen.|
|/sites/instances/processes/delete|Dient zum Löschen von Prozessen für Web-App-Instanzen.|
|/sites/instances/processes/read|Dient zum Abrufen von Prozessen für Web-App-Instanzen.|
|/sites/instances/deployments/read|Dient zum Abrufen von Prozessen für Web-App-Bereitstellungen.|
|/sites/sourcecontrols/Read|Dient zum Abrufen der Konfigurationseinstellungen für die Quellcodeverwaltung der Web-App.|
|/sites/sourcecontrols/Write|Dient zum Aktualisieren der Konfigurationseinstellungen für die Quellcodeverwaltung der Web-App.|
|/sites/sourcecontrols/Delete|Dient zum Löschen der Konfigurationseinstellungen für die Quellcodeverwaltung der Web-App.|
|/sites/restore/read|Dient zum Abrufen der Web-App-Wiederherstellung.|
|/sites/analyzecustomhostname/read|Dient zum Analysieren des benutzerdefinierten Hostnamens.|
|/sites/backups/Read|Dient zum Abrufen der Eigenschaften einer Web-App-Sicherung.|
|/sites/backups/list/action|Dient zum Auflisten von Web-App-Sicherungen.|
|/sites/backups/restore/action|Dient zum Wiederherstellen von Web-App-Sicherungen.|
|/sites/snapshots/read|Dient zum Abrufen von Web-App-Momentaufnahmen.|
|/sites/functions/delete|Dient zum Löschen von Web-App-Funktionen.|
|/sites/functions/listsecrets/action|Dient zum Auflisten der Geheimnisse für Web-App-Funktionen.|
|/sites/functions/read|Dient zum Abrufen von Web-App-Funktionen.|
|/sites/functions/write|Dient zum Aktualisieren von Web-App-Funktionen.|
|/sites/deployments/delete|Dient zum Löschen von Web-App-Bereitstellungen.|
|/sites/deployments/read|Dient zum Abrufen von Web-App-Bereitstellungen.|
|/sites/deployments/write|Dient zum Aktualisieren von Web-App-Bereitstellungen.|
|/sites/deployments/log/read|Dient zum Abrufen des Web-App-Bereitstellungsprotokolls.|
|/sites/diagnostics/read|Dient zum Abrufen von Web-App-Diagnosen.|
|/sites/diagnostics/workerprocessrecycle/read|Dient zum Abrufen der Workerprozesswiederverwendung für Web-App-Diagnosen.|
|/sites/diagnostics/workeravailability/read|Dient zum Abrufen der Workerverfügbarkeit für Web-App-Diagnosen.|
|/sites/diagnostics/runtimeavailability/read|Dient zum Abrufen der Laufzeitverfügbarkeit für Web-App-Diagnosen.|
|/sites/diagnostics/cpuanalysis/read|Dient zum Abrufen der CPU-Analyse von Web-App-Diagnosen.|
|/sites/diagnostics/servicehealth/read|Dient zum Abrufen der Dienstintegrität von Web-App-Diagnosen.|
|/sites/diagnostics/frebanalysis/read|Dient zum Abrufen der FREB-Analyse von Web-App-Diagnosen.|
|/availablestacks/read|Dient zum Abrufen verfügbarer Stapel.|
|/isusernameavailable/read|Dient zum Prüfen, ob der Benutzername verfügbar ist.|
|/Microsoft.Web/apiManagementAccounts/<br>apis/Read|Dient zum Abrufen der API-Liste.|
|/Microsoft.Web/apiManagementAccounts/<br>apis/Write|Dient zum Hinzufügen einer neuen API oder zum Aktualisieren einer bereits vorhandenen.|
|/Microsoft.Web/apiManagementAccounts/<br>apis/Delete|Dient zum Löschen einer vorhandenen API.|
|/Microsoft.Web/apiManagementAccounts/<br>apis/connections/Read|Dient zum Abrufen der Verbindungsliste.|
|/Microsoft.Web/apiManagementAccounts/<br>apis/connections/Write|Dient zum Speichern einer neuen Verbindung oder zum Aktualisieren einer bereits vorhandenen.|
|/Microsoft.Web/apiManagementAccounts/<br>apis/connections/Delete|Dient zum Löschen einer vorhandenen Verbindung.|
|/Microsoft.Web/apiManagementAccounts/<br>apis/connections/connectionAcls/Read|Dient zum Lesen von Verbindungs-ACLs.|
|/Microsoft.Web/apiManagementAccounts/<br>apis/connections/connectionAcls/Write|Dient zum Hinzufügen oder Aktualisieren von Verbindungs-ACLs.|
|/Microsoft.Web/apiManagementAccounts/<br>apis/connections/connectionAcls/Delete|Dient zum Löschen von Verbindungs-ACLs.|
|/Microsoft.Web/apiManagementAccounts/<br>apis/connectionAcls/Read|Dient zum Lesen von Verbindungs-ACLs für APIs.|
|/Microsoft.Web/apiManagementAccounts/<br>apis/apiAcls/Read|Dient zum Lesen von Verbindungs-ACLs.|
|/Microsoft.Web/apiManagementAccounts/<br>apis/apiAcls/Write|Dient zum Erstellen oder Aktualisieren von API-ACLs.|
|/Microsoft.Web/apiManagementAccounts/<br>apis/apiAcls/Delete|Dient zum Löschen von API-ACLs.|
|/serverfarms/Read|Dient zum Abrufen der Eigenschaften für einen App Service-Plan.|
|/serverfarms/Write|Dient zum Erstellen eines neuen App Service-Plans oder zum Aktualisieren eines bereits vorhandenen.|
|/serverfarms/Delete|Löschen eines vorhandenen App Service-Plans|
|/serverfarms/restartSites/Action|Dient zum Neustarten aller Web-Apps in einem App Service-Plan.|
|/serverfarms/operationresults/read|Dient zum Abrufen von Vorgangsergebnissen für App Service-Pläne.|
|/serverfarms/capabilities/read|Dient zum Abrufen von Funktionen für App Service-Pläne.|
|/serverfarms/metricdefinitions/read|Dient zum Abrufen von Metrikdefinitionen für App Service-Pläne.|
|/serverfarms/metrics/read|Dient zum Abrufen von Metriken für App Service-Pläne.|
|/serverfarms/hybridconnectionplanlimits/read|Dient zum Abrufen von Hybridverbindungsplan-Grenzwerten für App Service-Pläne.|
|/serverfarms/virtualnetworkconnections/read|Dient zum Abrufen von Verbindungen mit virtuellen Netzwerken für App Service-Pläne.|
|/serverfarms/virtualnetworkconnections/routes/delete|Dient zum Löschen von Routen für Verbindungen mit virtuellen Netzwerken für App Service-Pläne.|
|/serverfarms/virtualnetworkconnections/routes/read|Dient zum Abrufen von Routen für Verbindungen mit virtuellen Netzwerken für App Service-Pläne.|
|/serverfarms/virtualnetworkconnections/routes/write|Dient zum Aktualisieren von Routen für Verbindungen mit virtuellen Netzwerken für App Service-Pläne.|
|/serverfarms/virtualnetworkconnections/gateways/write|Dient zum Aktualisieren von Gateways für Verbindungen mit virtuellen Netzwerken für App Service-Pläne.|
|/serverfarms/firstpartyapps/settings/delete|Dient zum Löschen von Erstanbieter-App-Einstellungen für App Service-Pläne.|
|/serverfarms/firstpartyapps/settings/read|Dient zum Abrufen von Erstanbieter-App-Einstellungen für App Service-Pläne.|
|/serverfarms/firstpartyapps/settings/write|Dient zum Aktualisieren von Erstanbieter-App-Einstellungen für App Service-Pläne.|
|/serverfarms/sites/read|Dient zum Abrufen von Web-Apps für App Service-Pläne.|
|/serverfarms/workers/reboot/action|Dient zum Neustarten von Workern für App Service-Pläne.|
|/serverfarms/hybridconnectionrelays/read|Dient zum Abrufen von Hybridverbindungsrelays für App Service-Pläne.|
|/serverfarms/skus/read|Dient zum Abrufen von SKUs für App Service-Pläne.|
|/serverfarms/usages/read|Dient zum Abrufen von Verwendungen für App Service-Pläne.|
|/serverfarms/hybridconnectionnamespaces/relays/sites/read|Dient zum Abrufen von Hybridverbindungsnamespace-Relay-Web-Apps für App Service-Pläne.|
|/ishostnameavailable/read|Dient zum Prüfen, ob der Hostname verfügbar ist.|
|/connectionGateways/Read|Dient zum Abrufen der Verbindungsgatewayliste.|
|/connectionGateways/Write|Erstellt oder aktualisiert ein Verbindungsgateway.|
|/connectionGateways/Delete|Löscht ein Verbindungsgateway.|
|/connectionGateways/Join/Action|Verknüpft ein Verbindungsgateway.|
|/classicmobileservices/read|Dient zum Abrufen von Mobile Services (klassisch).|
|/skus/read|Dient zum Abrufen von SKUs.|
|/certificates/Read|Dient zum Abrufen der Zertifikatliste.|
|/certificates/Write|Dient zum Hinzufügen eines neuen Zertifikats oder zum Aktualisieren eines bereits vorhandenen.|
|/certificates/Delete|Dient zum Löschen eines vorhandenen Zertifikats.|
|/operations/read|Dient zum Abrufen von Vorgängen.|
|/recommendations/Read|Dient zum Abrufen der Liste mit Empfehlungen für Abonnements.|
|/ishostingenvironmentnameavailable/read|Dient zum Abrufen, ob der Hostingumgebungsname verfügbar ist.|
|/apiManagementAccounts/Read|Dient zum Abrufen der Liste mit API-Verwaltungskonten.|
|/apiManagementAccounts/Write|Dient zum Hinzufügen eines neuen API-Verwaltungskontos oder zum Aktualisieren eines bereits vorhandenen.|
|/apiManagementAccounts/Delete|Dient zum Löschen eines vorhandenen API-Verwaltungskontos.|
|/apiManagementAccounts/connectionAcls/Read|Dient zum Abrufen der Liste mit Verbindungs-ACLs.|
|/apiManagementAccounts/apiAcls/Read|Dient zum Lesen von Verbindungs-ACLs.|
|/connections/Read|Dient zum Abrufen der Verbindungsliste.|
|/connections/Write|Erstellt oder aktualisiert eine Verbindung.|
|/connections/Delete|Löscht eine Verbindung.|
|/connections/Join/Action|Verknüpft eine Verbindung.|
|/connections/confirmconsentcode/action|Dient zum Bestätigen des Genehmigungscodes für Verbindungen.|
|/connections/listconsentlinks/action|Dient zum Auflisten von Genehmigungslinks für Verbindungen.|
|/deploymentlocations/read|Dient zum Abrufen von Bereitstellungsorten.|
|/sourcecontrols/read|Dient zum Abrufen von Quellcodeverwaltungen.|
|/sourcecontrols/write|Dient zum Aktualisieren von Quellcodeverwaltungen.|
|/managedhostingenvironments/read|Dient zum Abrufen verwalteter Hostingumgebungen.|
|/managedhostingenvironments/sites/read|Dient zum Abrufen von Web-Apps für verwaltete Hostingumgebungen.|
|/managedhostingenvironments/serverfarms/read|Dient zum Abrufen von App Service-Plänen für verwaltete Hostingumgebungen.|
|/locations/managedapis/read|Dient zum Abrufen verwalteter APIs für Standorte.|
|/locations/apioperations/read|Dient zum Abrufen von Standorten für API-Vorgänge.|
|/locations/connectiongatewayinstallations/read|Dient zum Abrufen von Standorten für Verbindungsgatewayinstallationen.|
|/listSitesAssignedToHostName/Read|Dient zum Abrufen der Namen von Websites, die dem Hostnamen zugewiesen sind.|

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Erstellen einer benutzerdefinierten Rolle](role-based-access-control-custom-roles.md).

- Machen Sie sich mit den [integrierten RBAC-Rollen](role-based-access-built-in-roles.md) vertraut.

- Informieren Sie sich über das Verwalten von Zugriffszuweisungen [nach Benutzer](role-based-access-control-manage-assignments.md) oder [nach Ressource](role-based-access-control-configure.md). 

