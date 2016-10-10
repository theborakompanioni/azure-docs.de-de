<properties
    pageTitle="Referenz zur Azure Active Directory-Überwachungs-API | Microsoft Azure"
    description="Vorgehensweise zum Einstieg in die Azure Active Directory-Überwachungs-API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# Referenz zur Azure Active Directory-Überwachungs-API

Dieses Thema ist Bestandteil einer Sammlung von Themen zur Azure Active Directory-Berichterstellungs-API. Die Azure AD-Berichterstellung bietet eine API, mit der Sie unter Verwendung von Code oder zugehörigen Tools auf Überwachungsdaten zugreifen können. In diesem Thema erhalten Sie Referenzinformationen zur **Überwachungs-API**.

Siehe:

- Unter [Überwachungsprotokolle](active-directory-reporting-azure-portal.md#audit-logs) erhalten Sie konzeptionelle Informationen.
- Im Artikel [Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](active-directory-reporting-api-getting-started.md) finden Sie weitere Informationen zur Berichterstellungs-API.

Bei Fragen, Problemen oder zum Senden von Feedback wenden Sie sich an das [Hilfeteam für die AAD-Berichterstellung](mailto:aadreportinghelp@microsoft.com).


## Wer kann auf die Daten zugreifen?

- Benutzer mit den Rollen „Sicherheitsadministrator“ oder der Berechtigung „Sicherheit lesen“

- Globale Administratoren

- Jede App mit Autorisierung zum Zugriff auf die API (die App-Autorisierung kann nur basierend auf der Berechtigung eines globalen Administrators eingerichtet werden)



## Voraussetzungen

Um über die Berichterstellungs-API auf diesen Bericht zugreifen zu können, müssen folgende Bedingungen erfüllt sein:

- Sie verfügen über die [Free Edition von Azure Active Directory oder eine höhere Edition](active-directory-editions.md).

- Die [Voraussetzungen zum Zugriff auf die Azure AD-Berichterstellungs-API](active-directory-reporting-api-prerequisites.md) sind erfüllt.
 

##Zugriff auf die API

Sie können entweder über den [Graph-Tester](https://graphexplorer2.cloudapp.net) oder programmgesteuert, z.B. unter Verwendung von PowerShell, auf diese API zugreifen. Damit PowerShell die in AAD Graph-REST-Aufrufen verwendete OData-Filtersyntax richtig interpretiert, müssen Sie das Graviszeichen als Escapezeichen für das $-Zeichen verwenden. Das Graviszeichen dient als [Escapezeichen in PowerShell](https://technet.microsoft.com/library/hh847755.aspx) und erlaubt PowerShell eine zeichengetreue Interpretation von $, statt dieses als PowerShell-Variablenname (z.B. $filer) zu betrachten.

Der Schwerpunkt in diesem Thema liegt auf dem Graph-Tester. Ein PowerShell-Beispiel finden Sie in diesem [PowerShell-Skript](active-directory-reporting-api-audit-samples.md#powershell-script).

 
 

## API-Endpunkt


Sie können auf diese API mithilfe des folgenden URI zugreifen:

	https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Es gibt keine Beschränkung für die Anzahl der von der Azure AD-Berichterstellungs-API (mithilfe der OData-Paginierung) zurückgegebenen Datensätze. Informationen zu den Beschränkungen für die Aufbewahrung von Berichtsdaten finden Sie unter [Aufbewahrungsrichtlinien für Azure Active Directory-Berichte](active-directory-reporting-retention.md).

Dieser Aufruf gibt die Daten in Batches zurück. Jeder Batch umfasst maximal 1.000 Datensätze. Um den nächsten Batch an Datensätzen abzurufen, verwenden Sie den Link „Weiter“. Rufen Sie die skiptoken-Informationen für den ersten Satz an zurückgegebenen Datensätzen ab. Die skiptoken-Informationen befinden sich am Ende des Resultsets.

	https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## Unterstützte Filter

Sie können die Anzahl von zurückgegeben Datensätzen eingrenzen, indem Sie einen API-Aufruf in Form eines Filters ausführen. Für anmeldebezogene API-Daten werden die folgenden Filter unterstützt:

- **$top=<Anzahl zurückzugebender Datensätze>**: Zum Einschränken der Anzahl von Datensätzen, die zurückgegeben werden. Dies ist ein kostenintensiver Vorgang. Sie sollten diesen Filter nicht verwenden, wenn Tausende von Objekten zurückgegeben werden müssen.
- **$filter=<Ihre Filteranweisung>**: Legt basierend auf den unterstützten Filterfeldern fest, an welcher Art von Datensätzen Sie interessiert sind.



## Unterstützte Filterfelder und Operatoren

Um festzulegen, an welcher Art von Datensätzen Sie interessiert sind, können Sie eine Filteranweisung erstellen, die entweder eines der folgenden Filterfelder oder eine Kombination daraus enthält:

- [activityDate](#activitydate): Definiert ein Datum oder einen Datumsbereich.
- [activityType](#activitytype): Definiert den Typ einer Aktivität.
- [activity](#activity): Definiert die Aktivität als Zeichenfolge.
- [actor/name](#actorname): Definiert den Akteur in Form des Akteurnamens.
- [actor/objectid](#actorobjectid): Definiert den Akteur in Form der ID des Akteurs.
- [actor/upn](#actorupn): Definiert den Akteur als Benutzerprinzipalname (UPN) des Akteurs.
- [actor/name](#targetname): Definiert das Ziel in Form des Akteurnamens.
- [target/objectid](#targetobjectid): Definiert das Ziel in Form der ID des Ziels.
- [target/upn](#targetupn): Definiert das Ziel als Benutzerprinzipalname (UPN) des Akteurs.




----------

### activityDate

**Unterstützte Operatoren**: eq, ge, le, gt, lt

**Beispiel**:

	$filter=activityDate ge 2016-01-01T00:00:00Z and activityDate le 2016-01-02T00:00:00Z	

**Hinweise**:

datetime muss im UTC-Format angegeben werden.

----------

### activityType

**Unterstützte Operatoren**: eq

**Beispiel**:

	$filter=activityType eq 'User'	

**Hinweise**:

Erfordert eine Beachtung der Groß-/Kleinschreibung.

----------

### Aktivität

**Unterstützte Operatoren**: eq, contains, startsWith

**Beispiel**:

	$filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')	

**Hinweise**:

Erfordert eine Beachtung der Groß-/Kleinschreibung.

----------

### actor/name

**Unterstützte Operatoren**: eq, contains, startsWith

**Beispiel**:

	$filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')	

**Hinweise**:

Groß-/Kleinschreibung muss nicht beachtet werden.

	

----------
### actor/objectId

**Unterstützte Operatoren**: eq

**Beispiel**:

	$filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'	

----------
### target/name

**Unterstützte Operatoren**: eq, contains, startsWith

**Beispiel**:

	$filter=target/name eq 'test' or contains(target/name, 'test') or startswith(target/name, 'test')	

**Hinweise**:

Groß-/Kleinschreibung nicht beachten

----------

### target/upn

**Unterstützte Operatoren**: eq, startsWith

**Beispiel**:

	$filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc'))	

**Hinweise**:

- Groß-/Kleinschreibung nicht beachten
- Sie müssen bei der Abfrage von Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity den vollständigen Namespace hinzufügen.

----------

### target/objectId

**Unterstützte Operatoren**: eq

**Beispiel**:

	$filter=target/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'	

----------

### actor/upn

**Unterstützte Operatoren**: eq, startsWith

**Beispiel**:

	$filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')	

**Hinweise**:

- Groß-/Kleinschreibung nicht beachten
- Sie müssen bei der Abfrage von Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity den vollständigen Namespace hinzufügen.

----------




## Nächste Schritte

- Möchten Sie Beispiele für gefilterte Systemaktivitäten anzeigen? Sehen Sie sich die [Beispiele zur Azure Active Directory-Überwachungs-API](active-directory-reporting-api-audit-samples.md) an.

- Sie möchten mehr über die Azure AD-Berichterstellungs-API erfahren? Lesen Sie den Artikel [Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](active-directory-reporting-api-getting-started.md).

<!---HONumber=AcomDC_0928_2016-->