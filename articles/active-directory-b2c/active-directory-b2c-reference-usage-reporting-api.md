---
title: "Azure AD B2C-Berichterstattungs-API – Beispiele und Definitionen | Microsoft-Dokumentation"
description: "Anleitung und Beispiel zum Abrufen von Berichten über B2C-Mandantenbenutzer, Authentifizierungen und MFA."
services: active-directory-b2c
documentationcenter: dev-center-name
author: rojasja
manager: mbaldwin
ms.service: active-directory-b2c
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/08/2017
ms.author: joroja
translationtype: Human Translation
ms.sourcegitcommit: 4b2cbf5f62ee63bada42b2a06506b793d4349fdb
ms.openlocfilehash: 7db9a45a4c80ea8d01937837dfa7a15c171fb66b
ms.lasthandoff: 02/10/2017


---
# <a name="accessing-usage-reports-in-azure-ad-b2c-via-the-reporting-api"></a>Zugriff auf Verwendungsberichte in Azure AD B2C über die Berichterstattungs-API

Azure Active Directory B2C bietet Identitätsanbieter übergreifende Anmeldung und MFA-basierte Authentifizierung für alle Endbenutzer Ihrer Anwendungenfamilie.  Wenn die Anzahl der im Mandanten registrierten Benutzer, die Anbieter, die sie zur Registrierung verwendet haben und die Anzahl der Authentifizierungen nach Typ bekannt sind, können Fragen wie die folgenden beantwortet werden:
* Wie viele Benutzer jedes Identitätsanbietertyps (z.B. Microsoft Account, LinkedIn) haben sich in den letzten 10 Tagen registriert?
* Wie viele Multi-Factor-Authentifizierungen wurden im vergangenen Monat erfolgreich abgeschlossen?
* Wie viele anmeldebasierten Authentifizierungen wurden in diesem Monats abgeschlossen? Pro Tag? Pro Anwendung?
* Wie kann ich die erwarteten monatlichen Kosten für meine B2C-Mandantenaktivitäten ermitteln?

Der Schwerpunkt dieses Artikels liegt auf Berichten, die eng mit Abrechnungsaktivität, Benutzerzahl, Anzahl abrechenbarer, anmeldebasierter Authentifizierungen und Anzahl von Multi-Factor-Authentifizierungen verbunden sind.


## <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Voraussetzungen zum Zugriff auf die Azure AD-Berichterstellungs-API
Bevor Sie beginnen, müssen Sie die [Voraussetzungen zum Zugriff auf die Azure AD-Berichterstellungs-API](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/)erfüllen.  Erstellen Sie eine Anwendung, beschaffen Sie sich einen geheimen Schlüssel für sie, und gewähren Sie ihr Zugriffsrechte für die Berichte Ihres Azure AD B2C-Mandanten. *Bash-Skript*- und *Python-Skript*-Beispiele werden auch hier zur Verfügung gestellt.

## <a name="powershell-script"></a>PowerShell-Skript
Dieses Skript veranschaulicht die vier Verwendungsberichte mithilfe des Parameters **TimeStamp** und des Filters **-ApplicationId**.

```
# This script will require the Web Application and permissions setup in Azure Active Directory

# Constants
$ClientID      = "your-client-application-id-here"  
$ClientSecret  = "your-client-application-secret-here"
$loginURL      = "https://login.windows.net"
$tenantdomain  = "your-b2c-tenant-domain.onmicrosoft.com"  
# Get an Oauth 2 access token based on client id, secret and tenant domain
$body          = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth         = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
if ($oauth.access_token -ne $null) {
    $headerParams  = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    Write-host Data from the tenantUserCount report
    Write-host ====================================================
     # Returns a JSON document for the report
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the tenantUserCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?%24filter=TimeStamp+gt+2016-10-15&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCountSummary report
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=TimeStamp+gt+2016-09-20+and+TimeStamp+lt+2016-10-03&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCount report with ApplicationId filter
    Write-host ====================================================
    # Returns a JSON document for the " " report
        $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCountSummary
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCount?%24filter=TimeStamp+gt+2016-09-10+and+TimeStamp+lt+2016-10-04&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCount report with ApplicationId filter
    Write-host ====================================================
       $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
     Write-host $myReport.Content

} else {
    Write-Host "ERROR: No Access Token"
    }
```


## <a name="usage-report-definitions"></a>Definitionen von Verwendungsberichten
**TenantUserCount**: Die Anzahl der Benutzer im Mandanten nach Typ des Identitätsanbieters pro Tag in den letzten 30 Tagen. (Optional liefert ein TimeStamp-Filter Benutzerzahlen ab einem angegebenen Datum bis zum aktuellen Datum.) Der Bericht enthält:
 * TotalUserCount = Anzahl aller Benutzerobjekte
 * OtherUserCount = Anzahl der AAD-Verzeichnisbenutzer (Nicht-B2C-Benutzer)
 * LocalUserCount = Anzahl der B2C-Benutzerkonten, die mit Anmeldeinformationen lokal auf dem B2C-Mandanten erstellt wurden.

**AlternateIdUserCount** = Anzahl der mit externen Identitätsanbietern (z.B. Facebook, Microsoft Account und anderen AAD-Mandanten – auch bekannt als OrgId) registrierten B2C-Benutzer

**b2cAuthenticationCountSummary** = Summe der täglichen Anzahl abrechenbarer Authentifizierungen in den letzten 30 Tagen nach Tag und nach Typ des Authentifizierungsablaufs

**b2cAuthenticationCount** = Anzahl der Authentifizierungen innerhalb eines Zeitraums. Standard sind die letzten 30 Tage.  (Optional: TimeStamp[s] zu Beginn und Ende definieren einen bestimmten gewünschten Zählungszeitraum). Die Ausgabe enthält einen StartTimeStamp (frühestes Datum der Aktivität für diesen Mandanten) und EndTimeStamp (letztes Update).

**b2cMfaRequestCountSummary** = Summe der täglichen Anzahl an Multi-Factor-Authentifizierungen nach Tag und nach MFA-Typ (SMS oder Spracheingabe)


## <a name="limitations"></a>Einschränkungen
* Die Benutzeranzahl betreffende Daten werden alle 24 bis 48 Stunden aktualisiert.  Authentifizierungen werden mehrmals täglich aktualisiert.
* Bei Verwendung des Filters ApplicationId kann ein leerer Bericht als Antwort auf eine der folgenden Bedingungen zurückzuführen sein:
 * Die Anwendungs-ID ist im Mandanten nicht vorhanden. Stellen Sie sicher, dass sie korrekt ist.
 * Die Anwendungs-ID ist zwar vorhanden, aber im Berichtszeitraum wurden keine Daten gefunden. Überprüfen Sie die Datum-Uhrzeit-Parameter.


## <a name="next-steps"></a>Nächste Schritte
### <a name="estimating-your-azure-ad-monthly-bill"></a>Schätzen Sie Ihre monatliche Rechnung für Azure AD.
In Kombination mit [den aktuellsten verfügbaren Azure AD B2C-Preisen](https://azure.microsoft.com/pricing/details/active-directory-b2c/) können Sie tägliche, wöchentliche und monatliche Azure-Nutzung schätzen.  Eine Schätzung ist besonders hilfreich, wenn Sie Änderungen im Mandantenverhalten planen, die sich auf die Gesamtkosten auswirken können.  Istkosten können Sie unter Ihrem [verknüpften Azure-Abonnement](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-how-to-enable-billing) überprüfen.

### <a name="options-for-other-output-formats"></a>Optionen für andere Ausgabeformate
```
# to output to JSON use following line in the PowerShell sample
$myReport.Content | Out-File -FilePath b2cUserJourneySummaryEvents.json -Force

# to output the content to a name value list
($myReport.Content | ConvertFrom-Json).value | Out-File -FilePath name-your-file.txt -Force

# to output the content in XML use the following line
(($myReport.Content | ConvertFrom-Json).value | ConvertTo-Xml).InnerXml | Out-File -FilePath name-your-file.xml -Force
```


<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/        
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/    

