<properties
   pageTitle="Erste Schritte mit der Azure AD Reporting-API | Microsoft Azure"
   description="Vorgehensweise zum Einstieg in die Azure Active Directory Reporting-API"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/28/2016"
   ms.author="dhanyahk"/>

# Erste Schritte mit der Reporting-API von Azure Active Directory

*Diese Dokumentation ist Teil des [Handbuchs für Azure Active Directory Reporting](active-directory-reporting-guide.md).*

Azure Active Directory (AD) bietet eine Vielzahl von Aktivitäts-, Sicherheits- und Überwachungsberichten. Diese Daten können über das klassische Azure-Portal genutzt werden, sind aber auch in vielen anderen Anwendungen sehr nützlich, z.B. in SIEM-Systemen, Überwachungs- und Business Intelligence-Tools.

Die [Azure AD Reporting-APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) ermöglichen den programmgesteuerten Zugriff auf diese Daten über verschiedene REST-basierte APIs, die in einer Vielzahl von Programmiersprachen und Tools aufgerufen werden können.

Dieser Artikel begleitet Sie durch das Aufrufen der Azure AD Reporting-APIs über PowerShell. Sie können das PowerShell-Beispielskript ändern, um Ihrem Szenario entsprechend auf Daten in einem beliebigen der verfügbaren Berichte im JSON-, XML- oder Textformat zuzugreifen.

Zur Verwendung dieses Beispiels benötigen Sie einen [Azure Active Directory](active-directory-whatis.md)-Mandanten.

## Erstellen einer Azure AD-Anwendung für den Zugriff auf die API

Die Reporting-API verwendet [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) zum Autorisieren des Zugriffs auf die Web-APIs. Für den Zugriff auf Informationen in Ihrem Verzeichnis müssen Sie eine Anwendung in Ihrem Azure AD-Mandanten erstellen und ihr entsprechende Berechtigungen für den Zugriff auf die Azure AD-Daten erteilen.


### Registrieren einer Azure AD-Anwendung
Zum Registrieren der Azure AD-Anwendung müssen Sie sich beim klassischen Azure-Portal mit einem Azure-Abonnementadministratorkonto anmelden, das auch der Verzeichnisrolle „Globaler Administrator“ Ihres Azure AD-Mandanten angehört. Dies ist erforderlich, weil Sie die Azure AD-Anwendung mit Berechtigungen registrieren, die eine Registrierung/Zustimmung mit einem Konto mit globalen Administratorberechtigungen erfordern.

> [AZURE.IMPORTANT] Clientanwendungen, die wie hier unter Anmeldeinformationen mit Administratorberechtigungen ausgeführt werden, können äußerst einflussreich sein. Sorgen Sie also unbedingt dafür, dass die ID und geheimen Anmeldeinformationen der Anwendung stets gut geschützt sind.


1. Navigieren Sie zum [klassischen Azure-Portal](https://manage.windowsazure.com/).
2. Navigieren Sie im linken Bereich in der Erweiterung **Active Directory** zu Ihrem Azure AD-Mandanten.
3. Navigieren Sie zur Registerkarte **Anwendungen**.
4. Klicken Sie auf der unteren Leiste auf **Hinzufügen**.
	- Klicken Sie auf „Eine von meinem Unternehmen entwickelte Anwendung hinzufügen“.
	- **Name**: Hier haben Sie die freie Wahl Ein Eintrag wie "Reporting-API-Anwendung" wird empfohlen.
	- **Typ**: Wählen Sie "Webanwendung und/oder Web-API".
	- Klicken Sie auf den Pfeil, um zur nächsten Seite zu gelangen.
	- **Anmelde-URL**: ```https://localhost```.
	- **App-ID-URI**: ```https://localhost/ReportingApiApp```.
	- Klicken Sie auf das Häkchen, um das Hinzufügen der Anwendung abzuschließen.

### Gewähren Ihrer Anwendung die Berechtigung zum Verwenden der API
1. Navigieren Sie zur Registerkarte **Anwendungen**.
2. Navigieren Sie zu Ihrer neu erstellten Anwendung.
3. Klicken Sie auf die Registerkarte **Konfigurieren**.
4. Im Abschnitt "Berechtigungen für andere Anwendungen":
	- Klicken Sie für die Ressource „Windows Azure Active Directory“ (Berechtigungen für die Azure AD Graph-API) auf die Dropdownliste „Anwendungsberechtigungen“, und wählen Sie **Verzeichnisdaten lesen** aus.

        > [AZURE.IMPORTANT] Achten Sie darauf, die richtigen Berechtigungen anzugeben. Verwenden Sie „Berechtigungen“ (nicht „Delegierte Berechtigungen“). Andernfalls erhält die Anwendung nicht die Berechtigungsstufe, die sie für den Zugriff auf die Reporting-API benötigt, und für Ihr Skript tritt ein Fehler vom Typ *Unable to check Directory Read access for appId* (Der Verzeichnislesezugriff konnte für die App-ID nicht geprüft werden.) auf.


5. Klicken Sie auf der unteren Leiste auf **Speichern**.

### Abrufen der Verzeichnis-ID, der Client-ID und des geheimen Clientschlüssels

Die folgenden Schritte begleiten Sie durch das Abrufen der Client-ID Ihrer Anwendung und des geheimen Clientschlüssels. Sie müssen auch den Namen Ihres Mandanten kennen. Dieser kann entweder "*.onmicrosoft.com" oder ein benutzerdefinierter Domänenname sein. Kopieren Sie diese Werte an einen separaten Ort, da Sie sie später benötigen, um das Skript zu ändern.

#### Abrufen des Domänennamens Ihres Azure AD-Mandanten
1. Navigieren Sie im linken Bereich in der Erweiterung **Active Directory** zu Ihrem Azure AD-Mandanten.
2. Navigieren Sie zur Registerkarte **Domänen**.
4. Der Domänenname Ihres Mandanten (<Mandantenname>.onmicrosoft.com) wird angezeigt. Falls Sie benutzerdefinierte Domänennamen konfiguriert haben, werden diese ebenfalls angezeigt.

#### Abrufen der Client-ID der Anwendung
1. Navigieren Sie zur Registerkarte **Anwendungen**.
2. Navigieren Sie zu Ihrer neu erstellten Anwendung.
3. Navigieren Sie zur Registerkarte **Konfigurieren**.
4. Die Client-ID Ihrer Anwendung ist im Feld **Client-ID** aufgeführt.

#### Abrufen des geheimen Clientschlüssels der Anwendung
1. Navigieren Sie zur Registerkarte **Anwendungen**.
2. Navigieren Sie zu Ihrer neu erstellten Anwendung.
3. Navigieren Sie zur Registerkarte **Konfigurieren**.
4. Generieren Sie einen neuen geheimen Schlüssel für die Anwendung, indem Sie im Abschnitt "Schlüssel" eine Dauer auswählen.
5. Der Schlüssel wird beim Speichern angezeigt. Kopieren Sie ihn, und fügen Sie ihn unbedingt an einem sicheren Ort ein, da es später keine Möglichkeit des Abrufs gibt.

## Ändern des Skripts
Bearbeiten Sie eines der folgenden Skripts so, dass es Ihr Verzeichnis verwendet. Ersetzen Sie dazu „$ClientID“, „$ClientSecret“ und „$tenantdomain“ durch die korrekten Werte aus den obigen Abschnitten.

### PowerShell-Skript
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

    # Parse auditEvents report items, save output to file(s): auditEventsX.json, where X = 0 thru n for number of nextLink pages
    if ($oauth.access_token -ne $null) {   
        $i=0
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
        $url = 'https://graph.windows.net/$tenantdomain/reports/auditEvents?api-version=beta&$filter=eventTime gt ' + $7daysago

        # loop through each query page (1 through n)
        Do{
            # display each event on the console window
            Write-Output "Fetching data using Uri: $url"
            $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
            foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
                Write-Output ($event | ConvertTo-Json)
            }
        
            # save the query page to an output file
            Write-Output "Save the output to a file auditEvents$i.json"
            $myReport.Content | Out-File -FilePath auditEvents$i.json -Force
            $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
            $i = $i+1
        } while($url -ne $null)
    } else {
        Write-Host "ERROR: No Access Token"
        }

    Write-Host "Press any key to continue ..."
    $x = $host.UI.RawUI.ReadKey("NoEcho,IncludeKeyDown")

### Bash-Skript

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

    URL="https://graph.windows.net/$TENANT_DOMAIN/reports/auditEvents?api-version=beta&$filter=eventTime%20gt%20$YESTERDAY"


    REPORT=$(curl -s --header "Authorization: $TOKEN_TYPE $ACCESS_TOKEN" $URL)

    echo $REPORT | ./jq-win64.exe -r '.value' | ./jq-win64.exe -r ".[]"

### Python
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
	request_string = 'https://graph.windows.net/' + tenant_domain + '/reports/auditEvents?api-version=beta&$filter=eventTime%20gt%20' + yesterday   
	response = requests.get(request_string, headers = header_params)

	if response.status_code is 200:
	    print response.content
	else:
	    print 'ERROR: API request failed'


## Ausführen des Skripts
Wenn Sie die Bearbeitung des Skripts abgeschlossen haben, führen Sie es aus, und prüfen Sie, ob die erwarteten Daten aus dem Bericht "AuditEvents" zurückgegeben werden.

Das Skript gibt Ausgaben des Berichts „auditEvents“ im JSON-Format zurück. Außerdem erstellt es eine Datei vom Typ `auditEvents.json` mit der gleichen Ausgabe. Sie können damit experimentieren, indem Sie das Skript so verändern, dass es Daten aus anderen Berichten zurückgibt, und nicht benötigte Ausgabeformate auskommentieren.

## Hinweise

- Es gibt keine Beschränkung für die Anzahl der von der Azure AD Reporting-API (mithilfe von OData-Paginierung) zurückgegebenen Ereignisse.
- Informationen zu den Beschränkungen für die Aufbewahrung von Berichtsdaten finden Sie unter [Aufbewahrungsrichtlinien für Azure Active Directory-Berichte](active-directory-reporting-retention.md).


## Nächste Schritte
- Möchten Sie wissen, welche Sicherheits-, Überwachungs- und Aktivitätsberichte zur Verfügung stehen? Lesen Sie [Sicherheits-, Überwachungs- und Aktivitätsberichte](active-directory-view-access-usage-reports.md). Eine Liste mit allen verfügbaren Azure AD Graph-API-Endpunkten finden Sie unter [https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta](https://graph.windows.net/tenant-name/reports/$metadata?api-version=beta). Die entsprechende Dokumentation befindet sich im Artikel [Azure AD Reports and Events (Preview)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-reports-and-events-preview) (Azure AD-Berichte und-Ereignisse (Vorschau)).
- Unter [Azure AD-Überwachungsberichtsereignisse](active-directory-reporting-audit-events.md) finden Sie weitere Informationen zum Überwachungsbericht.
- Unter [Azure AD Reports and Events (Preview)](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) (Azure AD-Berichte und-Ereignisse (Vorschau)) finden Sie weitere Informationen zum REST-Dienst der Azure AD Graph-API.

<!---HONumber=AcomDC_0629_2016-->