---
title: "Beispiele zur Überwachungs-API für die Azure Active Directory-Berichterstellung | Microsoft Docs"
description: Vorgehensweise zum Einstieg in die Azure Active Directory Berichterstellungs-API
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: de8b8ec3-49b3-4aa8-93fb-e38f52c99743
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 665b613db31b15b6f6d2826a0795be6275c832ca
ms.openlocfilehash: 8216fa7ab092b2d0225d075d933fa56fbab56f40
ms.contentlocale: de-de
ms.lasthandoff: 12/28/2016


---
# <a name="azure-active-directory-reporting-audit-api-samples"></a>Beispiele zur Azure Active Directory-Überwachungs-API
Dieses Thema ist Bestandteil einer Sammlung von Themen zur Azure Active Directory-Berichterstellungs-API.  
Die Azure AD-Berichterstellung bietet eine API, mit der Sie unter Verwendung von Code oder zugehörigen Tools auf Überwachungsdaten zugreifen können.
In diesem Thema werden Codebeispiele für die **Überwachungs-API**bereitgestellt.

Siehe:

* [Überwachungsprotokolle](active-directory-reporting-azure-portal.md#audit-logs) erhalten Sie konzeptionelle Informationen.
* [Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](active-directory-reporting-api-getting-started.md) finden Sie weitere Informationen zur Berichterstellungs-API.

Bei Fragen, Problemen oder zum Senden von Feedback wenden Sie sich an das [Hilfeteam für die AAD-Berichterstellung](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Voraussetzungen
Damit Sie die Beispielen in diesem Thema verwenden können, müssen die [Voraussetzungen zum Zugriff auf die Azure AD-Berichterstellungs-API](active-directory-reporting-api-prerequisites.md)erfüllt sein.  

## <a name="known-issue"></a>Bekannte Probleme
Die App-Authentifizierung funktioniert nicht, wenn sich Ihr Mandant in der Region „Europa“ befindet. Verwenden Sie als Umgehung die Benutzerauthentifizierung zum Zugriff auf die Überwachungs-API, bis wir dieses Problem behoben haben. 

## <a name="powershell-script"></a>PowerShell-Skript
    # This script will require registration of a Web Application in Azure Active Directory (see https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/)

    # Constants
    $ClientID       = "your-client-application-id-here"       # Insert your application's Client ID, a Globally Unique ID (registered by Global Admin)
    $ClientSecret   = "your-client-application-secret-here"   # Insert your application's Client Key/Secret string
    $loginURL       = "https://login.microsoftonline.com"     # AAD Instance; for example https://login.microsoftonline.com
    $tenantdomain   = "your-tenant-domain.onmicrosoft.com"    # AAD Tenant; for example, contoso.onmicrosoft.com
    $resource       = "https://graph.windows.net"             # Azure AD Graph API resource URI
    $7daysago       = "{0:s}" -f (get-date).AddDays(-7) + "Z" # Use 'AddMinutes(-5)' to decrement minutes, for example
    Write-Output "Searching for events starting $7daysago"

    # Create HTTP header, get an OAuth2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    # Parse audit report items, save output to file(s): auditX.json, where X = 0 thru n for number of nextLink pages
    if ($oauth.access_token -ne $null) {   
        $i=0
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
        $url = 'https://graph.windows.net/' + $tenantdomain + '/activities/audit?api-version=beta&`$filter=activityDate gt ' + $7daysago

        # loop through each query page (1 through n)
        Do{
            # display each event on the console window
            Write-Output "Fetching data using Uri: $url"
            $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
            foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
                Write-Output ($event | ConvertTo-Json)
            }

            # save the query page to an output file
            Write-Output "Save the output to a file audit$i.json"
            $myReport.Content | Out-File -FilePath audit$i.json -Force
            $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
            $i = $i+1
        } while($url -ne $null)
    } else {
        Write-Host "ERROR: No Access Token"
        }

    Write-Host "Press any key to continue ..."
    $x = $host.UI.RawUI.ReadKey("NoEcho,IncludeKeyDown")


### <a name="executing-the-powershell-script"></a>Ausführen des PowerShell-Skripts
Wenn Sie die Bearbeitung des Skripts abgeschlossen haben, führen Sie es aus, und prüfen Sie, ob die erwarteten Daten aus dem Bericht zu den Überwachungsprotokollen zurückgegeben werden.

Das Skript gibt die Ausgabe des Überwachungsberichts im JSON-Format zurück. Darüber hinaus wird eine Datei `audit.json` mit derselben Ausgabe erstellt. Sie können damit experimentieren, indem Sie das Skript so verändern, dass es Daten aus anderen Berichten zurückgibt, und nicht benötigte Ausgabeformate auskommentieren.

## <a name="bash-script"></a>Bash-Skript
    #!/bin/bash

    # Author: Ken Hoff (kenhoff@microsoft.com)
    # Date: 2015.08.20
    # NOTE: This script requires jq (https://stedolan.github.io/jq/)

    CLIENT_ID="your-application-client-id-here"         # Should be a ~35 character string insert your info here
    CLIENT_SECRET="your-application-client-secret-here" # Should be a ~44 character string insert your info here
    LOGIN_URL="https://login.windows.net"
    TENANT_DOMAIN="your-directory-name-here.onmicrosoft.com"    # For example, contoso.onmicrosoft.com

    TOKEN_INFO=$(curl -s --data-urlencode "grant_type=client_credentials" --data-urlencode "client_id=$CLIENT_ID" --data-urlencode "client_secret=$CLIENT_SECRET" "$LOGIN_URL/$TENANT_DOMAIN/oauth2/token?api-version=1.0")

    TOKEN_TYPE=$(echo $TOKEN_INFO | ./jq-win64.exe -r '.token_type')
    ACCESS_TOKEN=$(echo $TOKEN_INFO | ./jq-win64.exe -r '.access_token')

    # get yesterday's date

    YESTERDAY=$(date --date='1 day ago' +'%Y-%m-%d')

    URL="https://graph.windows.net/$TENANT_DOMAIN/activities/audit?api-version=beta&$filter=activityDate%20gt%20$YESTERDAY"


    REPORT=$(curl -s --header "Authorization: $TOKEN_TYPE $ACCESS_TOKEN" $URL)

    echo $REPORT | ./jq-win64.exe -r '.value' | ./jq-win64.exe -r ".[]"

## <a name="python-script"></a>Python-Skript
    # Author: Michael McLaughlin (michmcla@microsoft.com)
    # Date: January 20, 2016
    # This requires the Python Requests module: http://docs.python-requests.org

    import requests
    import datetime
    import sys

    client_id = 'your-application-client-id-here'
    client_secret = 'your-application-client-secret-here'
    login_url = 'https://login.windows.net/'
    tenant_domain = 'your-directory-name-here.onmicrosoft.com'

    # Get an OAuth access token
    bodyvals = {'client_id': client_id,
                'client_secret': client_secret,
                'grant_type': 'client_credentials'}

    request_url = login_url + tenant_domain + '/oauth2/token?api-version=1.0'
    token_response = requests.post(request_url, data=bodyvals)

    access_token = token_response.json().get('access_token')
    token_type = token_response.json().get('token_type')

    if access_token is None or token_type is None:
        print "ERROR: Couldn't get access token"
        sys.exit(1)

    # Use the access token to make the API request
    yesterday = datetime.date.strftime(datetime.date.today() - datetime.timedelta(days=1), '%Y-%m-%d')

    header_params = {'Authorization': token_type + ' ' + access_token}
    request_string = 'https://graph.windows.net/' + tenant_domain + 'activities/audit?api-version=beta&$filter=activityDate%20gt%20' + yesterday   
    response = requests.get(request_string, headers = header_params)

    if response.status_code is 200:
        print response.content
    else:
        print 'ERROR: API request failed'





## <a name="next-steps"></a>Nächste Schritte
* Möchten Sie die Beispiele in diesem Thema anpassen? Dann sehen Sie sich die [Referenz zur Azure Active Directory-Überwachungs-API](active-directory-reporting-api-audit-reference.md)an. 
* Eine vollständige Übersicht zur Verwendung der Azure Active Directory-Berichterstellungs-API finden Sie im Artikel [Erste Schritte mit der Azure Active Directory-Berichterstellungs-API](active-directory-reporting-api-getting-started.md).
* Wenn Sie weitere Informationen zur Azure Active Directory-Berichterstellung benötigen, finden Sie diese im [Leitfaden zur Azure Active Directory-Berichterstellung](active-directory-reporting-guide.md).  


