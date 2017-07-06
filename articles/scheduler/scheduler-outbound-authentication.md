---
title: Ausgehende Authentifizierung von Scheduler
description: Ausgehende Authentifizierung von Scheduler
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 6707f82b-7e32-401b-a960-02aae7bb59cc
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/15/2016
ms.author: deli
ms.translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 4f2fcfecf0d888997b0b0061dc9ed2a8f862d78b
ms.contentlocale: de-de
ms.lasthandoff: 11/17/2016


---
# <a name="scheduler-outbound-authentication"></a>Ausgehende Authentifizierung von Scheduler
Scheduler-Aufträge müssen unter Umständen Dienste kontaktieren, die eine Authentifizierung erfordern. Dadurch kann ein aufgerufener Dienst ermitteln, ob dem Scheduler-Auftrag Zugriff auf die Ressourcen gewährt werden soll. Zu diesen Diensten zählen etwa andere Azure-Dienste, Salesforce.com, Facebook und sichere benutzerdefinierten Websites.

## <a name="adding-and-removing-authentication"></a>Hinzufügen und Entfernen der Authentifizierung
Ein Scheduler-Auftrag lässt sich ganz einfach mit einer Authentifizierung versehen: Fügen Sie dem Element `request` beim Erstellen oder Aktualisieren eines Auftrags ein untergeordnetes JSON-Element vom Typ `authentication` hinzu. Geheime Informationen, die in einer PUT-, PATCH- oder POST-Anforderung (als Teil des Objekts `authentication` ) an den Scheduler-Dienst übergeben werden, werden niemals in Antworten zurückgegeben. In Antworten werden geheime Informationen auf NULL festgelegt, oder sie besitzen unter Umständen ein öffentliches Token, das die authentifizierte Entität darstellt.

Wenn Sie die Authentifizierung entfernen möchten, führen Sie eine explizite PUT- oder PATCH-Anforderung für den Auftrag aus, und legen Sie dabei das Objekt `authentication` auf NULL fest. In der Antwort sind keinerlei Authentifizierungseigenschaften enthalten.

Derzeit werden als Authentifizierungstypen nur das Modell `ClientCertificate` (für die Verwendung von SSL-/TLS-Clientzertifikaten), das Modell `Basic` (für die Standardauthentifizierung) und das Modell `ActiveDirectoryOAuth` (für die Active Directory-OAuth-Authentifizierung) unterstützt.

## <a name="request-body-for-clientcertificate-authentication"></a>Anforderungstext für die ClientCertificate-Authentifizierung
Wenn Sie die Authentifizierung mithilfe des Modells `ClientCertificate` hinzufügen, müssen Sie im Anforderungstext zusätzlich folgende Elemente angeben:  

| Element | Beschreibung |
|:--- |:--- |
| *authentication (übergeordnetes Element)* |Das Authentifizierungsobjekt für die Verwendung eines SSL-Clientzertifikats. |
| *type* |Erforderlich. Die Art der Authentifizierung. Für SSL-Clientzertifikate muss der Wert auf `ClientCertificate` festgelegt werden. |
| *pfx* |Erforderlich. Base64-codierte Inhalte der PFX-Datei. |
| *password* |Erforderlich. Kennwort für den Zugriff auf die PFX-Datei. |

## <a name="response-body-for-clientcertificate-authentication"></a>Antworttext für die ClientCertificate-Authentifizierung
Wenn eine Anforderung mit Authentifizierungsinformationen gesendet wird, enthält die Antwort die folgenden authentifizierungsbezogenen Elemente:

| Element | Beschreibung |
|:--- |:--- |
| *authentication (übergeordnetes Element)* |Das Authentifizierungsobjekt für die Verwendung eines SSL-Clientzertifikats. |
| *type* |Die Art der Authentifizierung. Für SSL-Clientzertifikate lautet der Wert `ClientCertificate`. |
| *certificateThumbprint* |Der Fingerabdruck des Zertifikats. |
| *certificateSubjectName* |Der Distinguished Name des Antragstellers für das Zertifikat. |
| *certificateExpiration* |Das Ablaufdatum des Zertifikats. |

## <a name="sample-rest-request-for-clientcertificate-authentication"></a>REST-Beispielanforderung für die ClientCertificate-Authentifizierung
```
PUT https://management.azure.com/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-clientcertificate-authentication"></a>REST-Beispielantwort für die ClientCertificate-Authentifizierung
```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="request-body-for-basic-authentication"></a>Anforderungstext für die Standardauthentifizierung
Wenn Sie die Authentifizierung mithilfe des Modells `Basic` hinzufügen, müssen Sie im Anforderungstext zusätzlich folgende Elemente angeben:

| Element | Beschreibung |
|:--- |:--- |
| *authentication (übergeordnetes Element)* |Das Authentifizierungsobjekt für die Verwendung der Standardauthentifizierung. |
| *type* |Erforderlich. Die Art der Authentifizierung. Für die Standardauthentifizierung muss der Wert `Basic`lauten. |
| *username* |Erforderlich. Der zu authentifizierende Benutzername. |
| *password* |Erforderlich. Das zu authentifizierende Kennwort. |

## <a name="response-body-for-basic-authentication"></a>Antworttext für die Standardauthentifizierung
Wenn eine Anforderung mit Authentifizierungsinformationen gesendet wird, enthält die Antwort die folgenden authentifizierungsbezogenen Elemente:

| Element | Beschreibung |
|:--- |:--- |
| *authentication (übergeordnetes Element)* |Das Authentifizierungsobjekt für die Verwendung der Standardauthentifizierung. |
| *type* |Die Art der Authentifizierung. Für die Standardauthentifizierung lautet der Wert `Basic`. |
| *username* |Der authentifizierte Benutzername. |

## <a name="sample-rest-request-for-basic-authentication"></a>REST-Beispielanforderung für die Standardauthentifizierung
```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-basic-authentication"></a>REST-Beispielantwort für die Standardauthentifizierung
```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="request-body-for-activedirectoryoauth-authentication"></a>Anforderungstext für die ActiveDirectoryOAuth-Authentifizierung
Wenn Sie die Authentifizierung mithilfe des Modells `ActiveDirectoryOAuth` hinzufügen, müssen Sie im Anforderungstext zusätzlich folgende Elemente angeben:

| Element | Beschreibung |
|:--- |:--- |
| *authentication (übergeordnetes Element)* |Das Authentifizierungsobjekt für die Verwendung der ActiveDirectoryOAuth-Authentifizierung. |
| *type* |Erforderlich. Die Art der Authentifizierung. Für die ActiveDirectoryOAuth-Authentifizierung muss der Wert `ActiveDirectoryOAuth`lauten. |
| *tenant* |Erforderlich. Die Mandanten-ID für den Azure AD-Mandanten. |
| *audience* |Erforderlich. Dies wird auf „https://management.core.windows.net/“ festgelegt. |
| *clientId* |Erforderlich. Geben Sie die Client-ID für die Azure AD-Anwendung an. |
| *secret* |Erforderlich. Der geheime Schlüssel des Clients, der das Token anfordert. |

### <a name="determining-your-tenant-identifier"></a>Ermitteln der Mandanten-ID
Durch Ausführen von `Get-AzureAccount` in Azure PowerShell können Sie die Mandanten-ID für den Azure AD-Mandanten ermitteln.

## <a name="response-body-for-activedirectoryoauth-authentication"></a>Antworttext für die ActiveDirectoryOAuth-Authentifizierung
Wenn eine Anforderung mit Authentifizierungsinformationen gesendet wird, enthält die Antwort die folgenden authentifizierungsbezogenen Elemente:

| Element | Beschreibung |
|:--- |:--- |
| *authentication (übergeordnetes Element)* |Das Authentifizierungsobjekt für die Verwendung der ActiveDirectoryOAuth-Authentifizierung. |
| *type* |Die Art der Authentifizierung. Für die ActiveDirectoryOAuth-Authentifizierung lautet der Wert `ActiveDirectoryOAuth`. |
| *tenant* |Die Mandanten-ID für den Azure AD-Mandanten. |
| *audience* |Dies wird auf „https://management.core.windows.net/“ festgelegt. |
| *clientId* |Die Client-ID für die Azure AD-Anwendung. |

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a>REST-Beispielanforderung für die ActiveDirectoryOAuth-Authentifizierung
```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a>REST-Beispielantwort für die ActiveDirectoryOAuth-Authentifizierung
```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "tenant":"microsoft.onmicrosoft.com",
               "audience":"https://management.core.windows.net/",
               "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type":"ActiveDirectoryOAuth"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "lastExecutionTime":"2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime":"2016-03-16T19:11:00Z",
         "executionCount":5,
         "failureCount":5,
         "faultedCount":1
      }
   }
}
```

## <a name="see-also"></a>Weitere Informationen
 [Was ist Azure Scheduler?](scheduler-intro.md)

 [Konzepte, Terminologie und Entitätshierarchie für Azure Scheduler](scheduler-concepts-terms.md)

 [Erste Schritte mit dem Scheduler im Azure-Portal](scheduler-get-started-portal.md)

 [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)

 [Azure Scheduler-REST-API – Referenz](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler – PowerShell-Cmdlets-Referenz](scheduler-powershell-reference.md)

 [Hochverfügbarkeit und Zuverlässigkeit von Azure Scheduler](scheduler-high-availability-reliability.md)

 [Einschränkungen, Standardwerte und Fehlercodes für Azure Scheduler](scheduler-limits-defaults-errors.md)


