---
title: "Referenz zur Azure Active Directory-Anmeldeaktivitätsbericht-API | Microsoft Docs"
description: "Referenz zur Azure Active Directory-Anmeldeaktivitätsbericht-API"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: ddcd9ae0-f6b7-4f13-a5e1-6cbf51a25634
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2016
ms.author: dhanyahk;markvi
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 4c60c0fd364400c881f1b8b96598725f6bd11d26


---
# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Referenz zur Azure Active Directory-Anmeldeaktivitätsbericht-API
Dieses Thema ist Bestandteil einer Sammlung von Themen zur Azure Active Directory-Berichterstellungs-API.  
Die Azure AD-Berichterstellung bietet eine API, mit der Sie unter Verwendung von Code oder zugehörigen Tools auf Überwachungsdaten zugreifen können.
In diesem Thema erhalten Sie Referenzinformationen zur **Berichterstellungs-API zur Anmeldeaktivität**.

Siehe:

* [Anmeldeaktivität](active-directory-reporting-azure-portal.md#sign-in-activities) erhalten Sie konzeptionelle Informationen.
* [Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](active-directory-reporting-api-getting-started.md) finden Sie weitere Informationen zur Berichterstellungs-API.

Bei Fragen, Problemen oder zum Senden von Feedback wenden Sie sich an das [Hilfeteam für die AAD-Berichterstellung](mailto:aadreportinghelp@microsoft.com).

## <a name="who-can-access-the-api-data"></a>Wer kann auf die API-Daten zugreifen?
* Benutzer mit den Rollen „Sicherheitsadministrator“ oder der Berechtigung „Sicherheit lesen“
* Globale Administratoren
* Jede App mit Autorisierung zum Zugriff auf die API (die App-Autorisierung kann nur basierend auf der Berechtigung eines globalen Administrators eingerichtet werden)

## <a name="prerequisites"></a>Voraussetzungen
Um über die Berichterstellungs-API auf diesen Bericht zugreifen zu können, müssen folgende Bedingungen erfüllt sein:

* Sie verfügen über die [Azure Active Directory Premium P1 oder P2 Edition](active-directory-editions.md)
* Die [Voraussetzungen zum Zugriff auf die Azure AD-Berichterstellungs-API](active-directory-reporting-api-prerequisites.md)sind erfüllt. 

## <a name="accessing-the-api"></a>Zugriff auf die API
Sie können entweder über den [Graph-Tester](https://graphexplorer2.cloudapp.net) oder programmgesteuert, z.B. unter Verwendung von PowerShell, auf diese API zugreifen. Damit PowerShell die in AAD Graph-REST-Aufrufen verwendete OData-Filtersyntax richtig interpretiert, müssen Sie das Graviszeichen als Escapezeichen für das $-Zeichen verwenden. Das Graviszeichen dient als [Escapezeichen in PowerShell](https://technet.microsoft.com/library/hh847755.aspx) und erlaubt PowerShell eine zeichengetreue Interpretation des Zeichens „$“, statt dieses Zeichen als PowerShell-Variablenname (z.B. „$filter“) zu betrachten.

Der Schwerpunkt in diesem Thema liegt auf dem Graph-Tester. Ein PowerShell-Beispiel finden Sie in diesem [PowerShell-Skript](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).

## <a name="api-endpoint"></a>API-Endpunkt
Sie können auf diese API mithilfe des folgenden Basis-URI zugreifen:  

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Aufgrund der Menge an Daten gilt für diese API ein Limit von einer Million zurückgegebener Datensätze. 

Dieser Aufruf gibt die Daten in Batches zurück. Jeder Batch umfasst maximal 1.000 Datensätze.  
Um den nächsten Batch an Datensätzen abzurufen, verwenden Sie den Link „Weiter“. Rufen Sie die [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) -Informationen für den ersten Satz an zurückgegebenen Datensätzen ab. Die skiptoken-Informationen befinden sich am Ende des Resultsets.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Unterstützte Filter
Sie können die Anzahl von zurückgegeben Datensätzen eingrenzen, indem Sie einen API-Aufruf in Form eines Filters ausführen.  
Für anmeldebezogene API-Daten werden die folgenden Filter unterstützt:

* **$top=\<Anzahl zurückzugebender Datensätze\>**: Zum Einschränken der Anzahl von Datensätzen, die zurückgegeben werden. Dies ist ein kostenintensiver Vorgang. Sie sollten diesen Filter nicht verwenden, wenn Tausende von Objekten zurückgegeben werden müssen.  
* **$filter=\<Ihre Filteranweisung\>**: Legt basierend auf den unterstützten Filterfeldern fest, an welcher Art von Datensätzen Sie interessiert sind.

## <a name="supported-filter-fields-and-operators"></a>Unterstützte Filterfelder und Operatoren
Um festzulegen, an welcher Art von Datensätzen Sie interessiert sind, können Sie eine Filteranweisung erstellen, die entweder eines der folgenden Filterfelder oder eine Kombination daraus enthält:

* [signinDateTime](#signindatetime) : Definiert ein Datum oder einen Datumsbereich.
* [userId](#userid) : Definiert einen bestimmten Benutzer basierend auf der Benutzer-ID.
* [userPrincipalName](#userprincipalname) : Definiert einen bestimmten Benutzer basierend auf dem Benutzerprinzipalnamen (UPN) des Benutzers.
* [appId](#appid) : Definiert eine bestimmte App basierend auf der App-ID.
* [appDisplayName](#appdisplayname) : Definiert eine bestimmte App basierend auf dem Anzeigenamen der App.
* [loginStatus](#loginStatus) : Definiert den Status der Anmeldungen (erfolgreich/fehlerhaft).

> [!NOTE]
> Bei Verwendung des Graph-Testers müssen Sie die Groß-/Kleinschreibung für jeden Buchstaben in Ihren Filterfeldern berücksichtigen.
> 
> 

Um den Bereich der zurückgegebenen Daten einzuschränken, können Sie Kombinationen aus den unterstützten Filtern und Filterfeldern erstellen. Beispielsweise werden mit der folgenden Anweisung die ersten 10 Datensätze zwischen dem 1. Juli 2016 und dem 6. Juli 2016 zurückgegeben:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


- - -
### <a name="signindatetime"></a>signinDateTime
**Unterstützte Operatoren**: eq, ge, le, gt, lt

**Beispiel**:

Verwendung eines bestimmten Datums

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z    



Verwendung eines Datumsbereichs    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Hinweise**:

Der datetime-Parameter muss im UTC-Format angegeben werden. 

- - -
### <a name="userid"></a>userId
**Unterstützte Operatoren**: eq

**Beispiel**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Hinweise**:

Der Wert von userId ist ein Zeichenfolgenwert.

- - -
### <a name="userprincipalname"></a>userPrincipalName
**Unterstützte Operatoren**: eq

**Beispiel**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Hinweise**:

Der Wert von userPrincipalName ist ein Zeichenfolgenwert.

- - -
### <a name="appid"></a>appId
**Unterstützte Operatoren**: eq

**Beispiel**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Hinweise**:

Der Wert von appId ist ein Zeichenfolgenwert.

- - -
### <a name="appdisplayname"></a>appDisplayName
**Unterstützte Operatoren**: eq

**Beispiel**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Hinweise**:

Der Wert von appDisplayName ist ein Zeichenfolgenwert.

- - -
### <a name="loginstatus"></a>loginStatus
**Unterstützte Operatoren**: eq

**Beispiel**:

    $filter=loginStatus+eq+'1'  


**Hinweise**:

Es gibt zwei Optionen für den loginStatus: 0 = erfolgreich, 1 = fehlerhaft.

- - -
## <a name="next-steps"></a>Nächste Schritte
* Möchten Sie Beispiele für gefilterte Anmeldeaktivitäten anzeigen? Dann sehen Sie sich die [Beispiele zur Azure Active Directory-Anmeldeaktivitätsbericht-API](active-directory-reporting-api-sign-in-activity-samples.md)an.
* Sie möchten mehr über die Azure AD-Berichterstellungs-API erfahren? Lesen Sie den Artikel [Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](active-directory-reporting-api-getting-started.md).




<!--HONumber=Nov16_HO3-->


