---
title: "Office 365-Lösung in der Operations Management Suite (OMS) | Microsoft-Dokumentation"
description: "Dieser Artikel bietet Einzelheiten zur Konfiguration und Verwendung der Office 365-Lösung in OMS.  Er enthält eine ausführliche Beschreibung der Office 365-Datensätze, die in Log Analytics erstellt werden."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: dcc44986acbb76eafc3cfacb79acf237802de021
ms.contentlocale: de-de
ms.lasthandoff: 07/25/2017

---
# <a name="office-365-solution-in-operations-management-suite-oms"></a>Office 365-Lösung in der Operations Management Suite (OMS)

![Office 365-Logo](media/oms-solution-office-365/icon.png)

Mit der Office 365-Lösung für die Operations Management Suite (OMS) können Sie Ihre Office 365-Umgebung in Log Analytics überwachen.  

- Überwachen Sie Benutzeraktivitäten mit Ihren Office 365-Konten, um die Verwendungsmuster zu analysieren und Trends beim Verhalten zu identifizieren. Beispielsweise können Sie spezielle Verwendungsszenarien extrahieren, wie z.B. Dateien, die außerhalb Ihrer Organisation freigegeben werden, oder die am häufigsten verwendeten SharePoint-Websites.
- Überwachen Sie Administratoraktivitäten, um Konfigurationsänderungen oder Vorgänge mit erhöhten Rechten nachzuverfolgen.
- Ermitteln und untersuchen Sie unerwünschtes Benutzerverhalten. Dies kann an die Anforderungen Ihrer Organisation angepasst werden.
- Demonstrieren Sie Überwachung und Compliance. Beispielsweise können Sie Dateizugriffe auf vertrauliche Dateien überwachen und so den Überwachungs- und Complianceprozess unterstützen.
- Führen Sie eine operative Problembehandlung mithilfe der OMS-Suche in den Office 365-Aktivitätsdaten Ihrer Organisation durch.

## <a name="prerequisites"></a>Voraussetzungen
Folgendes ist erforderlich, bevor diese Lösung installiert und konfiguriert wird.

- Office 365-Organisationsabonnement.
- Anmeldeinformationen für ein Benutzerkonto, das ein globaler Administrator ist.
- Um Überwachungsdaten zu erhalten, müssen Sie [die Überwachung](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&rs=en-US&ad=US#PickTab=Before_you_begin) in Ihrem Office 365-Abonnement konfigurieren.  Beachten Sie, dass die [Postfachüberwachung](https://technet.microsoft.com/library/dn879651.aspx) separat konfiguriert wird.  Sie können die Lösung auch installieren und andere Daten sammeln, wenn die Überwachung nicht konfiguriert ist.
 


## <a name="management-packs"></a>Management Packs
Bei dieser Lösung werden keine Management Packs in verbundenen Verwaltungsgruppen installiert.
  

## <a name="configuration"></a>Konfiguration
Nachdem Sie [die Office 365-Lösung zu Ihrem Abonnement hinzugefügt haben](../log-analytics/log-analytics-add-solutions.md), müssen Sie sie mit Ihrem Office 365-Abonnement verbinden.

1. Fügen Sie die Alert Management-Lösung dem OMS-Arbeitsbereich hinzu, indem Sie den unter [Hinzufügen von Lösungen](../log-analytics/log-analytics-add-solutions.md)beschriebenen Prozess verwenden.
2. Navigieren Sie im OMS-Portal zu **Einstellungen**.
3. Wählen Sie unter **Verbundene Datenquellen** die Option **Office 365** aus.
4. Klicken Sie auf **Office 365 verbinden**.<br>![Office 365 verbinden](media/oms-solution-office-365/configure.png)
5. Melden Sie sich bei Office 365 mit einem Konto an, das ein globaler Administrator für Ihr Abonnement ist. 
6. Das Abonnement wird mit den Workloads aufgeführt, die die Lösung überwacht.<br>![Office 365 verbinden](media/oms-solution-office-365/connected.png) 


## <a name="data-collection"></a>Datensammlung
### <a name="supported-agents"></a>Unterstützte Agents
Die Office 365-Lösung ruft keine Daten von den [OMS-Agents](../log-analytics/log-analytics-data-sources.md) ab.  Sie ruft Daten direkt aus Office 365 ab.

### <a name="collection-frequency"></a>Sammlungshäufigkeit
Office 365 sendet immer, wenn ein Datensatz erstellt wird, eine [Webhookbenachrichtigung](https://msdn.microsoft.com/office-365/office-365-management-activity-api-reference#receiving-notifications) mit detaillierten Daten an Log Analytics.

## <a name="using-the-solution"></a>Verwenden der Lösung
Wenn Sie die Office 365-Lösung dem OMS-Arbeitsbereich hinzufügen, wird Ihrem OMS-Dashboard die Kachel **Office 365** hinzugefügt. Auf dieser Kachel werden ein Zahlenwert und eine grafische Darstellung der Anzahl von Computern in Ihrer Umgebung und jeweils die Updatekonformität angezeigt.<br><br>
![Kachel mit der Office 365-Zusammenfassung](media/oms-solution-office-365/tile.png)  

Klicken Sie auf die Kachel **Office 365**, um das Dashboard **Office 365** zu öffnen.

![Office 365-Dashboard](media/oms-solution-office-365/dashboard.png)  

Das Dashboard enthält die Spalten, die in der folgenden Tabelle angegeben sind. In jeder Spalte sind die zehn wichtigsten Warnungen nach ihrer Anzahl aufgeführt. Dies richtet sich jeweils nach den Spaltenkriterien für den angegebenen Bereich und Zeitraum. Sie können eine Protokollsuche durchführen, mit der die gesamte Liste ausgegeben wird, indem Sie unten in der Spalte auf „Alle anzeigen“ oder indem Sie auf die Spaltenüberschrift klicken.

| Column | Beschreibung |
|:--|:--|
| Vorgänge | Bietet Informationen über die aktiven Benutzer aller Ihrer überwachten Office 365-Abonnements. Sie können auch die Anzahl der Aktivitäten sehen, die im Verlauf aufgetreten sind.
| Exchange | Zeigt die Aufschlüsselung der Exchange Server-Aktivitäten, z.B. Berechtigungen zum Hinzufügen oder Festlegen von Postfächern. |
| SharePoint | Zeigt die wichtigsten Aktivitäten, die Benutzer in SharePoint-Dokumenten ausführen. Wenn Sie auf dieser Kachel einen Drilldown ausführen, zeigt die Seite „Suche“ die Details dieser Aktivitäten, z.B. das Zieldokument und den Speicherort dieser Aktivität. Beispiel: Für ein Ereignis „Auf Datei zugegriffen“ werden das Dokument, auf das zugegriffen wurde, der zugeordnete Kontoname und die IP-Adresse angezeigt. |
| Azure Active Directory | Enthält die wichtigsten Benutzeraktivitäten, z.B. Versuche, das Benutzerkennwort zurückzusetzen, und Anmeldeversuche. Wenn Sie einen Drilldown ausführen, können Sie die Details dieser Aktivitäten wie den Ergebnisstatus anzeigen. Dies ist hilfreich, wenn Sie verdächtige Aktivitäten in Azure Active Directory überwachen möchten. |




## <a name="log-analytics-records"></a>Log Analytics-Datensätze

Alle im Log Analytics-Arbeitsbereich von der Office 365-Lösung erstellten Datensätze weisen **OfficeActivity** als **Typ** auf.  Die Eigenschaft **OfficeWorkload** bestimmt, zu welchem Office 365-Dienst der Datensatz gehört: Exchange, AzureActiveDirectory, SharePoint oder OneDrive.  Die Eigenschaft **RecordType** gibt den Typ des Vorgangs an.  Die Eigenschaften für jeden Vorgangstyp variieren. Sie sind in den folgenden Tabellen dargestellt.

### <a name="common-properties"></a>Allgemeine Eigenschaften
Die folgenden Eigenschaften gelten für alle Office 365-Datensätze.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Typ | *OfficeActivity* |
| ClientIP | Die IP-Adresse des Geräts, das verwendet wurde, als die Aktivität protokolliert wurde. Die IP-Adresse wird im IPv4- oder IPv6-Adressformat angezeigt. |
| OfficeWorkload | Office 365-Dienst, auf den sich der Datensatz bezieht.<br><br>AzureActiveDirectory<br>Exchange<br>SharePoint|
| Vorgang | Der Name der Benutzer- oder Administratoraktivität.  |
| OrganizationId | Die GUID des Office 365-Mandanten Ihrer Organisation. Dieser Wert ist für Ihre Organisation immer gleich, unabhängig vom Office 365-Dienst, in dem er auftritt. |
| RecordType | Der Typ des ausgeführten Vorgangs. |
| ResultStatus | Gibt an, ob die Aktion (angegeben in der Eigenschaft „Operation“) erfolgreich war oder nicht. Mögliche Werte sind „Succeeded“, „PartiallySucceded“ oder „Failed“. Bei Exchange-Administratoraktivitäten ist der Wert entweder „True“ oder „False“. |
| UserId | Der Benutzerprinzipalname (User Principal Name, UPN) des Benutzers, der die Aktion ausgeführt hat, die zum Protokollieren des Datensatzes geführt hat, beispielsweise my_name@my_domain_name. Beachten Sie, dass auch Datensätze für die von Systemkonten ausgeführten Aktivitäten (z.B. „SHAREPOINT\system“ oder „NTAUTHORITY\SYSTEM“) enthalten sind. | 
| UserKey | Eine alternative ID für den Benutzer, der in der Eigenschaft „UserId“ identifiziert wird.  Beispiel: Diese Eigenschaft wird für Ereignisse, die von Benutzern in SharePoint, OneDrive for Business und Exchange ausgeführt werden, mit der eindeutigen Passport-ID (PUID) aufgefüllt. Diese Eigenschaft kann für Ereignisse, die in anderen Diensten und Ereignissen von Systemkonten ausgeführt werden, auch den gleichen Wert wie die Eigenschaft „UserID“ angeben.|
| UserType | Der Typ des Benutzers, der den Vorgang ausgeführt hat.<br><br>Administrator<br>Anwendung<br>DcAdmin<br>Regulär <br>Reserved<br>ServicePrincipal<br>System |


### <a name="azure-active-directory-base"></a>Azure Active Directory-Basis
Die folgenden Eigenschaften gelten für alle Azure Active Directory-Datensätze.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AzureActiveDirectory_EventType | Der Typ des Azure AD-Ereignisses. |
| ExtendedProperties | Die erweiterten Eigenschaften des Azure AD-Ereignisses. |


### <a name="azure-active-directory-account-logon"></a>Azure Active Directory-Kontoanmeldung
Diese Datensätze werden erstellt, wenn ein Active Directory-Benutzer versucht, sich anzumelden.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectoryAccountLogon |
| Anwendung | Die Anwendung, die das Kontoanmeldeereignis auslöst, z.B. Office 15. |
| Client- | Details zum Clientgerät, Betriebssystem des Geräts und Gerätebrowser, das bzw. der für das Kontoanmeldeereignis verwendet wurde. |
| LoginStatus | Diese Eigenschaft stammt direkt von „OrgIdLogon.LoginStatus“. Die Zuordnung von verschiedenen interessanten Anmeldefehlern könnte durch Warnungsalgorithmen erfolgen. |
| UserDomain | Die Informationen zur Mandantenidentität (Tenant Identity Information, TII). | 


### <a name="azure-active-directory"></a>Azure Active Directory
Diese Datensätze werden erstellt, wenn Änderungen oder Erweiterungen an Azure Active Directory-Objekten vorgenommen werden.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AADTarget | Der Benutzer, von dem die Aktion (identifiziert durch die Eigenschaft „Operation“) ausgeführt wurde. |
| Akteur | Der Benutzer oder Dienstprinzipal, der die Aktion ausgeführt hat. |
| ActorContextId | Die GUID der Organisation, zu der der Akteur gehört. |
| ActorIpAddress | Der IP-Adresse des Akteurs im IPV4- oder IPV6-Adressformat. |
| InterSystemsId | Die GUID, die die Aktionen für Komponenten im Office 365-Dienst überwacht. |
| IntraSystemId |   Die GUID, die von Azure Active Directory zum Nachverfolgen der Aktion generiert wird. |
| SupportTicketId | Die Kundensupportticket-ID für die Aktion in Vertretungssituationen. |
| TargetContextId | Die GUID der Organisation, zu der der Zielbenutzer gehört. |


### <a name="data-center-security"></a>Sicherheit von Rechenzentren
Diese Datensätze werden aus Überwachungsdaten für die Sicherheit von Rechenzentren erstellt.  

| Eigenschaft | Beschreibung |
|:--- |:--- |
| EffectiveOrganization | Der Name des Mandanten, der Ziel der Erhöhung der Rechte/des Cmdlets war. |
| ElevationApprovedTime | Der Zeitstempel des Zeitpunkts, als die Erhöhung der Rechte genehmigt wurde. |
| ElevationApprover | Der Name eines Microsoft-Managers. |
| ElevationDuration | Die Dauer, für die die Erhöhung der Rechte aktiv war. |
| ElevationRequestId |  Ein eindeutiger Bezeichner für die Anforderung zur Erhöhung der Rechte. |
| ElevationRole | Die Rolle, für die die Erhöhung der Rechte angefordert wurde. |
| ElevationTime | Die Startzeit für die Erhöhung der Rechte. |
| Start_Time | Die Startzeit für die Cmdlet-Ausführung. |


### <a name="exchange-admin"></a>Exchange-Verwaltung
Diese Datensätze werden erstellt, wenn Änderungen an der Exchange-Konfiguration vorgenommen werden.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeAdmin |
| ExternalAccess |  Gibt an, ob das Cmdlet von einem Benutzer in Ihrer Organisation, von Mitarbeitern eines Microsoft-Rechenzentrums oder vom Dienstkonto eines Rechenzentrums oder von einem delegierten Administrator ausgeführt wurde. Der Wert „False“ gibt an, dass das Cmdlet von einem Benutzer in Ihrer Organisation ausgeführt wurde. Der Wert „True“ gibt an, dass das Cmdlet von Mitarbeitern eines Rechenzentrums, einem Dienstkonto eines Rechenzentrums oder einem delegierten Administrator ausgeführt wurde. |
| ModifiedObjectResolvedName |  Dies ist der benutzerfreundliche Name des Objekts, das vom Cmdlet geändert wurde. Dies wird nur protokolliert, wenn das Cmdlet das Objekt ändert. |
| OrganizationName | Der Name des Mandanten. |
| OriginatingServer | Der Name des Servers, über den das Cmdlet ausgeführt wurde. |
| Parameter | Der Name und der Wert für alle Parameter, die mit dem Cmdlet verwendet wurden, das in der Eigenschaft „Operations“ identifiziert wird. |


### <a name="exchange-mailbox"></a>Exchange-Postfach
Diese Datensätze werden erstellt, wenn Änderungen oder Erweiterungen an Exchange-Postfächern vorgenommen werden.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeItem |
| ClientInfoString | Informationen zum E-Mail-Client, der zum Ausführen des Vorgangs verwendet wurde, z.B. eine Browserversion, eine Outlook-Version und Informationen zu mobilen Geräten. |
| Client_IPAddress | Die IP-Adresse des Geräts, das verwendet wurde, als der Vorgang protokolliert wurde. Die IP-Adresse wird im IPv4- oder IPv6-Adressformat angezeigt. |
| ClientMachineName | Der Computername, der den Outlook-Client hostet. |
| ClientProcessName | Der E-Mail-Client, der verwendet wurde, um auf das Postfach zuzugreifen. |
| ClientVersion | Die Version des E-Mail-Clients. |
| InternalLogonType | Für die interne Verwendung reserviert. |
| Logon_Type | Gibt den Typ des Benutzers an, der auf das Postfach zugegriffen und den Vorgang ausgeführt hat, der protokolliert wurde. |
| LogonUserDisplayName |    Der benutzerfreundliche Name des Benutzers, der den Vorgang ausgeführt hat. |
| LogonUserSid | Die SID des Benutzers, der den Vorgang ausgeführt hat. |
| MailboxGuid | Die Exchange-GUID des Postfachs, auf das zugegriffen wurde. |
| MailboxOwnerMasterAccountSid | Die Hauptkonto-SID des Kontos des Postfachbesitzers. |
| MailboxOwnerSid | Die SID des Postfachbesitzers. |
| MailboxOwnerUPN | Die E-Mail-Adresse der Person, die das Postfach besitzt, auf das zugegriffen wurde. |


### <a name="exchange-mailbox-audit"></a>Exchange-Postfachüberwachung
Diese Datensätze werden erstellt, wenn ein Eintrag zur Postfachüberwachung erstellt wird.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeItem |
| Element | Stellt das Element dar, für das der Vorgang ausgeführt wurde. | 
| SendAsUserMailboxGuid | Die Exchange-GUID des Postfachs, auf das zugegriffen wurde, um die E-Mail zu senden. |
| SendAsUserSmtp | Die SMTP-Adresse des Benutzers, dessen Identität angenommen wird. |
| SendonBehalfOfUserMailboxGuid | Die Exchange-GUID des Postfachs, auf das zugegriffen wurde, um die E-Mail im Namen eines anderen Benutzers zu senden. |
| SendOnBehalfOfUserSmtp | Die SMTP-Adresse des Benutzers, in dessen Namen die E-Mail gesendet wird. |


### <a name="exchange-mailbox-audit-group"></a>Exchange-Postfachüberwachung – Gruppen
Diese Datensätze werden erstellt, wenn Änderungen oder Erweiterungen an Exchange-Gruppen vorgenommen werden.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | Exchange |
| OfficeWorkload | ExchangeItemGroup |
| AffectedItems | Informationen über alle Elemente in der Gruppe. |
| CrossMailboxOperations | Gibt an, ob mehr als ein Postfach am Vorgang beteiligt war. |
| DestMailboxId | Wird nur festgelegt, wenn der Parameter „CrossMailboxOperations“ auf „True“ festgelegt ist. Gibt die Zielpostfach-GUID an. |
| DestMailboxOwnerMasterAccountSid | Wird nur festgelegt, wenn der Parameter „CrossMailboxOperations“ auf „True“ festgelegt ist. Gibt die SID für die Hauptkonto-SID des Ziel-Postfachbesitzers an. |
| DestMailboxOwnerSid | Wird nur festgelegt, wenn der Parameter „CrossMailboxOperations“ auf „True“ festgelegt ist. Gibt die SID des Zielpostfachs an. |
| DestMailboxOwnerUPN | Wird nur festgelegt, wenn der Parameter „CrossMailboxOperations“ auf „True“ festgelegt ist. Gibt den UPN des Besitzers des Zielpostfachs an. |
| DestFolder | Der Zielordner für Vorgänge wie „Verschieben“. |
| Ordner | Der Ordner, in dem sich eine Gruppe von Elementen befindet. |
| Ordner |     Informationen zu den Quellordnern, die an einem Vorgang beteiligt waren, z.B. ob Ordner ausgewählt und dann gelöscht wurden. |


### <a name="sharepoint-base"></a>SharePoint-Basis
Diese Eigenschaften gelten für alle SharePoint-Datensätze.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| EventSource | Gibt an, dass in SharePoint ein Ereignis aufgetreten ist. Mögliche Werte sind „SharePoint“ oder „ObjectModel“. |
| ItemType | Der Typ des Objekts, auf das zugegriffen wurde oder das geändert wurde. Details zu den Typen von Objekten finden Sie in der Tabelle zu „ItemType“. |
| MachineDomainInfo | Informationen zu Gerätesynchronisierungsvorgängen. Diese Informationen werden nur gemeldet, wenn sie in der Anforderung vorhanden sind. |
| MachineId |   Informationen zu Gerätesynchronisierungsvorgängen. Diese Informationen werden nur gemeldet, wenn sie in der Anforderung vorhanden sind. |
| Site_ | Die GUID der Website, in der sich die Datei oder der Ordner befindet, auf die bzw. den der Benutzer zugegriffen hat. |
| Source_Name | Die Entität, die den überwachten Vorgang ausgelöst hat. Mögliche Werte sind „SharePoint“ oder „ObjectModel“. |
| UserAgent | Informationen zum Client oder Browser des Benutzers. Diese Informationen werden vom Client oder Browser bereitgestellt. |


### <a name="sharepoint-schema"></a>SharePoint-Schema
Diese Datensätze werden erstellt, wenn Konfigurationsänderungen an SharePoint vorgenommen werden.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| CustomEvent | Optionale Zeichenfolge für benutzerdefinierte Ereignisse. |
| Event_Data |  Optionale Nutzlast für benutzerdefinierte Ereignisse. |
| ModifiedProperties | Die Eigenschaft bezieht sich auf Verwaltungsereignisse, z.B. das Hinzufügen eines Benutzers als Mitglied einer Website oder der Administratorgruppe einer Websitesammlung. Die Eigenschaft enthält den Namen der Eigenschaft, die geändert wurde (z.B. die Gruppe der Websiteadministratoren), den neuen Wert der geänderten Eigenschaft (z.B. den Benutzer, der als Websiteadministrator hinzugefügt wurde) und den vorherigen Wert des geänderten Objekts. |


### <a name="sharepoint-file-operations"></a>SharePoint-Dateivorgänge
Diese Datensätze werden als Reaktion auf Dateivorgänge in SharePoint erstellt.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePointFileOperation |
| DestinationFileExtension | Die Dateierweiterung einer Datei, die kopiert oder verschoben wird. Diese Eigenschaft wird nur für die Ereignisse „FileCopied“ und „FileMoved“ angezeigt. |
| DestinationFileName | Der Name der Datei, die kopiert oder verschoben wird. Diese Eigenschaft wird nur für die Ereignisse „FileCopied“ und „FileMoved“ angezeigt. |
| DestinationRelativeUrl | Die URL des Zielordners, in den eine Datei kopiert oder verschoben wird. Die Kombination der Werte für die Parameter „SiteURL“, „DestinationRelativeURL“ und „DestinationFileName“ ist identisch mit dem Wert für die Eigenschaft „ObjectID“, die den vollständigen Pfadnamen für die kopierte Datei darstellt. Diese Eigenschaft wird nur für die Ereignisse „FileCopied“ und „FileMoved“ angezeigt. |
| SharingType | Der Typ der Freigabeberechtigungen, die dem Benutzer zugewiesen wurden, für den die Ressource freigegeben wurde. Dieser Benutzer wird durch den Parameter „UserSharedWith“ identifiziert. |
| Site_Url | Die URL der Website, in der sich die Datei oder der Ordner befindet, auf die bzw. den der Benutzer zugegriffen hat. |
| SourceFileExtension | Die Dateierweiterung der Datei, auf die der Benutzer zugegriffen hat. Diese Eigenschaft ist leer, wenn das Objekt, auf das zugegriffen wurde, ein Ordner ist. |
| SourceFileName |  Der Name der Datei oder des Ordners, auf die bzw. den der Benutzer zugegriffen hat. |
| SourceRelativeUrl | Die URL des Ordners, der die Datei enthält, auf die der Benutzer zugegriffen hat. Die Kombination der Werte für die Parameter „SiteURL“, „SourceRelativeURL“ und „SourceFileName“ ist identisch mit dem Wert für die Eigenschaft „ObjectID“, die den vollständigen Pfadnamen für die Datei darstellt, auf die der Benutzer zugegriffen hat. |
| UserSharedWith |  Der Benutzer, für den eine Ressource freigegeben wurde. |




## <a name="sample-log-searches"></a>Beispiele für Protokollsuchen
Die folgende Tabelle enthält Beispiele für Protokollsuchen für Updatedatensätze, die mit dieser Lösung erfasst wurden.

| Abfrage | Beschreibung |
| --- | --- |
|Anzahl aller Vorgänge in Ihrem Office 365-Abonnement |Type = OfficeActivity | measure count() by Operation |
|Verwendung von SharePoint-Websites|Type=OfficeActivity OfficeWorkload=sharepoint | measure count() as Count by SiteUrl | sort Count asc|
|Dateizugriffsvorgänge nach Benutzertyp|Type=OfficeActivity OfficeWorkload=sharepoint Operation=FileAccessed | measure count() by UserType|
|Suche mit einem bestimmten Schlüsselwort|`Type=OfficeActivity OfficeWorkload=azureactivedirectory "MyTest"`|
|Überwachung externer Aktionen für Exchange|`Type=OfficeActivity OfficeWorkload=exchange ExternalAccess = true`|



## <a name="troubleshooting"></a>Problembehandlung

Wenn Ihre Office 365-Lösung nicht wie erwartet Daten erfasst, überprüfen Sie den Status im OMS-Portal unter **Einstellungen** -> **Verbundene Datenquellen** -> **Office 365**. In der folgenden Tabelle werden die einzelnen Statuswerte beschrieben.

| Status | Beschreibung |
|:--|:--|
| Aktiv | Das Office 365-Abonnement ist aktiv, und die Workload wurde erfolgreich mit Ihrem OMS-Arbeitsbereich verbunden. |
| Ausstehend | Das Office 365-Abonnement ist aktiv, aber die Workload wurde noch nicht erfolgreich mit Ihrem OMS-Arbeitsbereich verbunden. Wenn Sie sich zum ersten Mal mit dem Office 365-Abonnement verbinden, weisen alle Workloads diesen Status auf, bis sie erfolgreich verbunden werden. Warten Sie 24 Stunden, bis alle Workloads auf „Aktiv“ wechseln. |
| Inaktiv | Das Office 365-Abonnement weist einen inaktiven Status auf. Details finden Sie auf Ihrer Office 365-Verwaltungsseite. Nachdem Sie Ihr Office 365-Abonnement aktiviert haben, trennen Sie die Verbindung mit Ihrem OMS-Arbeitsbereich, und stellen Sie sie erneut her, um mit dem Empfang von Daten zu beginnen. |



## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie die Protokollsuche in [Log Analytics](../log-analytics/log-analytics-log-searches.md), um ausführliche Daten zu Updates anzuzeigen.
* [Erstellen Sie eigene Dashboards](../log-analytics/log-analytics-dashboards.md), um Ihre bevorzugten Office 365-Suchabfragen anzuzeigen.
* [Erstellen Sie Warnungen](../log-analytics/log-analytics-alerts.md), um proaktiv über wichtige Office 365-Aktivitäten benachrichtigt zu werden.  

