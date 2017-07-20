---
title: Python-Skript zum Abrufen von Daten aus Azure Log Analytics | Microsoft-Dokumentation
description: "Die Log Analytics-Protokollsuch-API ermöglicht einem beliebigem REST-API-Client den Abruf von Daten aus einem Log Analytics-Arbeitsbereich.  Dieser Artikel enthält ein Python-Beispielskript mit der Protokollsuch-API."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/28/2017
ms.author: bwren
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: 56d7c6dc648a01e7b0efc167cb65c94bac5468ec
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017


---

# <a name="retrieve-data-from-log-analytics-with-a-python-script"></a>Abrufen von Daten aus Log Analytics mit einem Python-Skript
Die [Log Analytics-Protokollsuch-API](log-analytics-log-search-api.md) ermöglicht einem beliebigem REST-API-Client den Abruf von Daten aus einem Log Analytics-Arbeitsbereich.  In diesem Artikel wird ein Python-Beispielskript gezeigt, das die Log Analytics-Protokollsuch-API verwendet.  

## <a name="authentication"></a>Authentifizierung
Dieses Skript verwendet zur Authentifizierung beim Arbeitsbereich einen Dienstprinzipal in Azure Active Directory.  Durch Dienstprinzipale kann eine Clientanwendung anfordern, dass der Dienst selbst dann ein Konto authentifiziert, wenn der Client keinen Kontonamen aufweist. Vor der Ausführung dieses Skripts müssen Sie einen Dienstprinzipal über den Prozess erstellen, der unter [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](../azure-resource-manager/resource-group-create-service-principal-portal.md) beschrieben wird.  Sie müssen die Anwendungs-ID, die Mandanten-ID und den Authentifizierungsschlüssel für das Skript angeben. 

> [!NOTE]
> Wenn Sie [ein Azure Automation-Konto erstellen](../automation/automation-create-standalone-account.md), wird ein Dienstprinzipal für die Verwendung mit diesem Skript erstellt.  Wenn Sie bereits einen Dienstprinzipal mit Azure Automation erstellt haben, können Sie diesen verwenden, anstatt einen neuen zu erstellen. Sie müssen jedoch möglicherweise [einen Authentifizierungsschlüssel erstellen](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key), falls noch keiner vorhanden ist.

## <a name="script"></a>Skript
``` python
import adal
import requests
import json
import datetime
from pprint import pprint

# Details of workspace.  Fill in details for your workspace.
resource_group = 'xxxxxxxx'
workspace = 'xxxxxxxx'

# Details of query.  Modify these to your requirements.
query = "Type=Event"
end_time = datetime.datetime.utcnow()
start_time = end_time - datetime.timedelta(hours=24)
num_results = 100  # If not provided, a default of 10 results will be used.

# IDs for authentication.  Fill in values for your service principal.
subscription_id = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'
tenant_id = 'xxxxxxxx-xxxx-xxxx-xxx-xxxxxxxxxxxx'
application_id = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx'
application_key = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'

# URLs for authentication
authentication_endpoint = 'https://login.microsoftonline.com/'
resource  = 'https://management.core.windows.net/'

# Get access token
context = adal.AuthenticationContext('https://login.microsoftonline.com/' + tenant_id)
token_response = context.acquire_token_with_client_credentials('https://management.core.windows.net/', application_id, application_key)
access_token = token_response.get('accessToken')

# Add token to header
headers = {
    "Authorization": 'Bearer ' + access_token,
    "Content-Type":'application/json'
}

# URLs for retrieving data
uri_base = 'https://management.azure.com'
uri_api = 'api-version=2015-11-01-preview'
uri_subscription = 'https://management.azure.com/subscriptions/' + subscription_id
uri_resourcegroup = uri_subscription + '/resourcegroups/'+ resource_group
uri_workspace = uri_resourcegroup + '/providers/Microsoft.OperationalInsights/workspaces/' + workspace
uri_search = uri_workspace + '/search'

# Build search parameters from query details
search_params = {
        "query": query,
        "top": num_results,
        "start": start_time.strftime('%Y-%m-%dT%H:%M:%S'),
        "end": end_time.strftime('%Y-%m-%dT%H:%M:%S')
        }

# Build URL and send post request
uri = uri_search + '?' + uri_api
response = requests.post(uri,json=search_params,headers=headers)

# Response of 200 if successful
if response.status_code == 200:

    # Parse the response to get the ID and status
    data = response.json()
    search_id = data["id"].split("/")
    id = search_id[len(search_id)-1]
    status = data["__metadata"]["Status"]

    # If status is pending, then keep checking until complete
    while status == "Pending":

        # Build URL to get search from ID and send request
        uri_search = uri_search + '/' + id
        uri = uri_search + '?' + uri_api
        response = requests.get(uri,headers=headers)

        # Parse the response to get the status
        data = response.json()
        status = data["__metadata"]["Status"]

else:

    # Request failed
    print (response.status_code)
    response.raise_for_status()

print ("Total records:" + str(data["__metadata"]["total"]))
print ("Returned top:" + str(data["__metadata"]["top"]))
pprint (data["value"])
```
## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über die [Log Analytics-Protokollsuch-API](log-analytics-log-search-api.md).
