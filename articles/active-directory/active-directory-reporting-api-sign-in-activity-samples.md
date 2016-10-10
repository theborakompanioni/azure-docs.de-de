<properties
    pageTitle="Beispiele zur Azure Active Directory-Anmeldeaktivitätsbericht-API | Microsoft Azure"
    description="Vorgehensweise zum Einstieg in die Azure Active Directory Berichterstellungs-API"
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

# Beispiele zur Azure Active Directory-Anmeldeaktivitätsbericht-API

Dieses Thema ist Bestandteil einer Sammlung von Themen zur Azure Active Directory-Berichterstellungs-API. Die Azure AD-Berichterstellung bietet eine API, mit der Sie unter Verwendung von Code oder zugehörigen Tools auf Daten zur Anmeldeaktivität zugreifen können. In diesem Thema werden Codebeispiele für die **Anmeldeaktivitätsbericht-API** bereitgestellt.

Siehe:

- Unter [Überwachungsprotokolle](active-directory-reporting-azure-portal.md#audit-logs) erhalten Sie konzeptionelle Informationen.
- Im Artikel [Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](active-directory-reporting-api-getting-started.md) finden Sie weitere Informationen zur Berichterstellungs-API.

Bei Fragen, Problemen oder zum Senden von Feedback wenden Sie sich an das [Hilfeteam für die AAD-Berichterstellung](mailto:aadreportinghelp@microsoft.com).


## Voraussetzungen
Damit Sie die Beispielen in diesem Thema verwenden können, müssen die [Voraussetzungen zum Zugriff auf die Azure AD-Berichterstellungs-API](active-directory-reporting-api-prerequisites.md) erfüllt sein.


##PowerShell-Skript

	# This script will require the Web Application and permissions setup in Azure Active Directory
	$ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
	$ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
	$loginURL       = "https://login.windows.net/"
	$tenantdomain   = "<tenantDomain>"
	$ daterange            # For example, contoso.onmicrosoft.com

	$7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
	# or, AddMinutes(-5)

	Write-Output $7daysago

	# Get an Oauth 2 access token based on client id, secret and tenant domain
	$body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

	$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

	if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"
	
	$i=0
  	
	Do{
		Write-Output "Fetching data using Uri: $url"
		$myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
		Write-Output "---------------------------------------------"
		$myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
		$url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
		$i = $i+1
	} while($url -ne $null)

	} else {
    
		Write-Host "ERROR: No Access Token"
	}




## Ausführen des Skripts
Wenn Sie die Bearbeitung des Skripts abgeschlossen haben, führen Sie es aus, und prüfen Sie, ob die erwarteten Daten aus dem Bericht zu den Überwachungsprotokollen zurückgegeben werden.

Das Skript gibt die Ausgabe des Anmeldeberichts im JSON-Format zurück. Darüber hinaus wird eine Datei `SigninActivities.json` mit derselben Ausgabe erstellt. Sie können damit experimentieren, indem Sie das Skript so verändern, dass es Daten aus anderen Berichten zurückgibt, und nicht benötigte Ausgabeformate auskommentieren.



## Nächste Schritte

- Möchten Sie die Beispiele in diesem Thema anpassen? Dann sehen Sie sich die [Referenz zur Azure Active Directory-Anmeldeaktivitätsbericht-API](active-directory-reporting-api-sign-in-activity-reference.md) an.

- Eine vollständige Übersicht zur Verwendung der Azure Active Directory-Berichterstellungs-API finden Sie im Artikel [Erste Schritte mit der Azure Active Directory-Berichterstellungs-API](active-directory-reporting-api-getting-started.md).

- Wenn Sie weitere Informationen zur Azure Active Directory-Berichterstellung benötigen, finden Sie diese im [Leitfaden zur Azure Active Directory-Berichterstellung](active-directory-reporting-guide.md).

<!---HONumber=AcomDC_0928_2016-->