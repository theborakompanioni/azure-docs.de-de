<properties 
    pageTitle="Protokollierung und Fehlerbehandlung in Logik-Apps | Microsoft Azure" 
    description="Enthält einen echten Anwendungsfall mit erweiterter Fehlerbehandlung und Protokollierung mit Logik-Apps." 
    keywords=""
    services="logic-apps" 
    authors="hedidin" 
    manager="" 
    editor="" 
    documentationCenter=""/>
    
<tags 
    ms.service="logic-apps" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/29/2016" 
    ms.author="b-hoedid"/>
    
# Protokollierung und Fehlerbehandlung in Logik-Apps 

In diesem Artikel wird ausführlich beschrieben, wie Sie eine Logik-App erweitern können, um eine bessere Unterstützung der Ausnahmebehandlung zu erzielen. Es handelt sich um einen echten Anwendungsfall und unsere Antwort auf die Frage **Unterstützen Logik-Apps die Ausnahme- und Fehlerbehandlung?**

>[AZURE.NOTE] Die aktuelle Version der Logik-Apps enthält eine Standardvorlage für Antworten auf Aktionen. Dies umfasst sowohl die interne Überprüfung als auch Fehlerantworten, die von einer API-App zurückgegeben werden.

## Übersicht über den Anwendungsfall und das Szenario

Die folgende Geschichte ist der Anwendungsfall für diesen Artikel.

> Wir wurden von einem bekannten Unternehmen aus dem Gesundheitswesen damit beauftragt, eine Azure-Lösung für ein Patientenportal mit Dynamics CRM Online (CRMOL) zu erstellen. Das Unternehmen nutzte zum damaligen Zeitpunkt Salesforce, und die Aufgabe bestand darin, Termindatensätze zwischen dem CRMOL-Patientenportal und Salesforce zu übertragen. Außerdem sollte die Norm [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) für alle Patientendatensätze verwendet werden.

> Die folgenden beiden wichtigen Anforderungen mussten integriert werden:
> -  Protokollierung der vom CRMOL-Portal gesendeten Datensätze
> -  Schaffung einer Möglichkeit zum Anzeigen von Fehlern, die im Workflow auftreten


## Lösung des Problems

>[AZURE.TIP] Auf der Website der [Integration User Group](http://www.integrationusergroup.com/do-logic-apps-support-error-handling/ "Integration User Group") können Sie sich ein allgemeines Video zu diesem Thema ansehen.

Wir haben uns für die Verwendung von **[Azure DocumentDB](https://azure.microsoft.com/services/documentdb/ "Azure DocumentDB")** als Repository für die Protokoll- und Fehlerdatensätze entschieden. (In DocumentDB werden Datensätze als Dokumente bezeichnet.) Da Logik-Apps über eine Standardvorlage für alle Antworten verfügen, mussten wir kein benutzerdefiniertes Schema erstellen. Wir konnten eine API-App zum Einfügen und Abfragen für Fehler- und Protokolldatensätze erstellen. Außerdem konnten wir jeweils ein Schema in der API-App definieren.

Weiterhin bestand die Anforderung, Datensätze nach einem bestimmten Datum endgültig zu löschen. DocumentDB verfügt über die Eigenschaft [Time-To-Live (TTL)](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "Gültigkeitsdauer") (Gültigkeitsdauer), mit der wir einen **Time-To-Live**-Wert für jeden Datensatz bzw. eine gesamte Sammlung festlegen konnten. Es ist dann nicht mehr erforderlich, Datensätze in DocumentDB manuell zu löschen.

### Unser Ansatz

Der erste Schritt ist die Erstellung unserer Logik-App und das Laden in den Designer. In diesem Beispiel verwenden wir übergeordnete und untergeordnete Logik-Apps. Wir setzen voraus, dass wir die übergeordnete Logik-App bereits erstellt haben. Wir erstellen also eine untergeordnete Logik-App.

Da wir den aus CRMOL stammenden Datensatz protokollieren möchten, beginnen wir oben. Wir müssen einen Anforderungstrigger verwenden, da die übergeordnete Logik-App dieses untergeordnete Element auslöst.

> [AZURE.IMPORTANT] Zur Durchführung dieses Tutorials müssen Sie eine DocumentDB-Datenbank und zwei Sammlungen (Protokollierung und Fehler) erstellen.

### Logik-App-Trigger 

**Wir verwenden wie unten dargestellt einen Anforderungstrigger.**

```` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

````


### Schritte 

Wir beginnen mit der Protokollierung. Wir müssen die Quelle (Anforderung) des Patientendatensatzes aus dem CRMOL-Portal protokollieren.

1. Zuerst müssen wir aus CRMOL **einen neuen Termindatensatz abrufen**. Der Trigger aus CRM liefert uns die folgenden Informationen: **CRM PatentId**, **Datensatztyp**, **Neuer oder aktualisierter Datensatz** (neuer oder aktualisierter boolescher Wert) und **SalesforceId**. Die „SalesforceId“ kann auch ein Nullwert sein, da sie nur für Updates verwendet wird. Wir erhalten den CRM-Datensatz, indem wir die CRM-PatientID und den Datensatztyp verwenden.
1. Als Nächstes müssen wir den **InsertLogEntry**-Vorgang der DocumentDB-API-App wie in den folgenden Abbildungen hinzufügen:


#### Protokolleintrag einfügen – Designeransicht

![Protokolleintrag einfügen](./media/app-service-logic-scenario-error-and-exception-handling/lognewpatient.png)

#### Fehlereintrag einfügen – Designeransicht
![Protokolleintrag einfügen](./media/app-service-logic-scenario-error-and-exception-handling/insertlogentry.png)

#### Bedingung – Auf Fehler beim Erstellen des Datensatzes überprüfen 

![Bedingung](./media/app-service-logic-scenario-error-and-exception-handling/condition.png)


## Logik-App-Quellcode 

>[AZURE.NOTE]  Bei den folgenden Angaben handelt es sich nur um Beispiele. Da das Tutorial auf einer tatsächlichen Implementierung basiert, die gerade in der Produktion verwendet wird, werden für den Wert eines **Quellknotens** unter Umständen keine Eigenschaften angezeigt, die sich auf das Planen eines Termins beziehen.

### Protokollierung
Im folgenden Logik-App-Codebeispiel wird veranschaulicht, wie Sie die Protokollierung behandeln.

#### Einfügen eines Protokolleintrags
Dies ist der Logik-App-Quellcode zum Einfügen eines Protokolleintrags.

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### Protokollanforderung

Dies ist die Nachricht zur Protokollanforderung für die API-App.

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
	    "date": "Fri, 10 Jun 2016 22:31:56 GMT",
	    "operation": "New Patient",
	    "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
	    "providerId": "",
	    "source": "{"Pragma":"no-cache","x-ms-request-id":"e750c9a9-bd48-44c4-bbba-1688b6f8a132","OData-Version":"4.0","Cache-Control":"no-cache","Date":"Fri, 10 Jun 2016 22:31:56 GMT","Set-Cookie":"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1","Server":"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0","X-AspNet-Version":"4.0.30319","X-Powered-By":"ASP.NET","Content-Length":"1935","Content-Type":"application/json; odata.metadata=minimal; odata.streaming=true","Expires":"-1"}"
    	}
    }
    
```


#### Protokollantwort

Dies ist die Nachricht zur Protokollantwort aus der API-App.

``` json
{
    "statusCode": 200,
    "headers": {
	    "Pragma": "no-cache",
	    "Cache-Control": "no-cache",
	    "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
	    "Server": "Microsoft-IIS/8.0",
	    "X-AspNet-Version": "4.0.30319",
	    "X-Powered-By": "ASP.NET",
	    "Content-Length": "964",
	    "Content-Type": "application/json; charset=utf-8",
	    "Expires": "-1"
    },
    "body": {
	    "ttl": 2592000,
	    "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
	    "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
	    "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
	    "_ts": 1465597936,
	    "_etag": ""0400fc2f-0000-0000-0000-575b3ff00000"",
	    "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
	    "timestamp": "2016-06-10T22:31:56Z",
	    "source": "{"Pragma":"no-cache","x-ms-request-id":"e750c9a9-bd48-44c4-bbba-1688b6f8a132","OData-Version":"4.0","Cache-Control":"no-cache","Date":"Fri, 10 Jun 2016 22:31:56 GMT","Set-Cookie":"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1","Server":"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0","X-AspNet-Version":"4.0.30319","X-Powered-By":"ASP.NET","Content-Length":"1935","Content-Type":"application/json; odata.metadata=minimal; odata.streaming=true","Expires":"-1"}",
	    "operation": "New Patient",
	    "salesforceId": "",
	    "expired": false
    }
}
    
```

Nun sehen wir uns die Schritte der Fehlerbehandlung an:

----------    
    
### Fehlerbehandlung

Im folgenden Logik-App-Codebeispiel wird veranschaulicht, wie Sie die Fehlerbehandlung implementieren können.

#### Erstellen des Fehlerdatensatzes

Dies ist der Logik-App-Quellcode zum Erstellen eines Fehlerdatensatzes.

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter": 
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}  	       
```

#### Einfügen eines Fehlers in DocumentDB – Anforderung

``` json
  
{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{"Account_Class_vod__c":"PRAC","Account_Status_MED__c":"I","CRM_HUB_ID__c":"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0","Credentials_vod__c","DTC_ID_MED__c":"","Fax":"","FirstName":"A","Gender_vod__c":"","IMS_ID__c":"","LastName":"BAILEY","MasterID_mp__c":"","C_ID_MED__c":"851588","Middle_vod__c":"","NPI_vod__c":"","PDRP_MED__c":false,"PersonDoNotCall":false,"PersonEmail":"","PersonHasOptedOutOfEmail":false,"PersonHasOptedOutOfFax":false,"PersonMobilePhone":"","Phone":"","Practicing_Specialty__c":"FM - FAMILY MEDICINE","Primary_City__c":"","Primary_State__c":"","Primary_Street_Line2__c":"","Primary_Street__c":"","Primary_Zip__c":"","RecordTypeId":"012U0000000JaPWIA0","Request_Date__c":"2016-06-10T22:31:55.9647467Z","ONY_ID__c":"","Specialty_1_vod__c":"","Suffix_vod__c":"","Website":""}",
        "statusCode": "400"
    }
}
```

#### Einfügen eines Fehlers in DocumentDB – Antwort


``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": ""0c00eaac-0000-0000-0000-575b3fdc0000"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{"Account_Class_vod__c":"PRAC","Account_Status_MED__c":"I","CRM_HUB_ID__c":"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0","Credentials_vod__c":"DO - Degree level is DO","DTC_ID_MED__c":"","Fax":"","FirstName":"A","Gender_vod__c":"","IMS_ID__c":"","LastName":"BAILEY","MterID_mp__c":"","Medicis_ID_MED__c":"851588","Middle_vod__c":"","NPI_vod__c":"","PDRP_MED__c":false,"PersonDoNotCall":false,"PersonEmail":"","PersonHasOptedOutOfEmail":false,"PersonHasOptedOutOfFax":false,"PersonMobilePhone":"","Phone":"","Practicing_Specialty__c":"FM - FAMILY MEDICINE","Primary_City__c":"","Primary_State__c":"","Primary_Street_Line2__c":"","Primary_Street__c":"","Primary_Zip__c":"","RecordTypeId":"012U0000000JaPWIA0","Request_Date__c":"2016-06-10T22:31:55.9647467Z","XXXXXXX":"","Specialty_1_vod__c":"","Suffix_vod__c":"","Website":""}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### Salesforce-Fehlerantwort

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}
    
```

### Zurückgeben der Antwort an die übergeordnete Logik-App

Nachdem Sie die Antwort erhalten haben, können Sie sie zurück an die übergeordnete Logik-App übergeben.

#### Zurückgeben der Antwort „Erfolg“ an die übergeordnete Logik-App

``` json
"SuccessResponse": {
    "runAfter": 
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "	Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### Zurückgeben der Antwort „Fehler“ an die übergeordnete Logik-App

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}
  
```

----------
  
## DocumentDB-Repository und Portal

Mit unserer Lösung wurden [DocumentDB](https://azure.microsoft.com/services/documentdb) zusätzliche Funktionen hinzugefügt.

### Fehlerverwaltungsportal

Sie können eine MVC-Web-App erstellen, mit der die Fehlerdatensätze aus DocumentDB angezeigt werden können. Für die aktuelle Version sind die Vorgänge **Liste**, **Details**, **Bearbeiten** und **Löschen** enthalten.

> [AZURE.NOTE] Vorgang „Bearbeiten“: DocumentDB führt eine Ersetzung des gesamten Dokuments durch. Die Datensätze, die in der Listen- und Detailansicht angezeigt werden, sind lediglich Beispiele. Es handelt sich nicht um echte Datensätze mit Patiententerminen.
        
Unten sind Beispiele für unsere MVC-App-Details unter Verwendung des obigen Ansatzes angegeben.

#### Fehlerverwaltung – Liste

![Fehlerliste](./media/app-service-logic-scenario-error-and-exception-handling/errorlist.png)
        
#### Fehlerverwaltung – Detailansicht

![Fehlerdetails](./media/app-service-logic-scenario-error-and-exception-handling/errordetails.png)

### Protokollverwaltungsportal

Zum Anzeigen der Protokolle haben wir auch eine MVC-Web-App erstellt. Unten sind Beispiele für unsere MVC-App-Details unter Verwendung des obigen Ansatzes angegeben.

#### Beispielprotokoll – Detailansicht

![Protokoll – Detailansicht](./media/app-service-logic-scenario-error-and-exception-handling/samplelogdetail.png)
    
### API-App-Details

#### Ausnahmeverwaltungs-API für Logik-Apps

Mit unserer Ausnahmeverwaltungs-API-App für Logik-Apps (Open Source) wurden die folgenden Funktionen bereitgestellt.

Zwei Controller
- ErrorController: Fügt einen Fehlerdatensatz (Dokument) in eine DocumentDB-Sammlung ein.
- LogController: Fügt einen Protokolldatensatz (Dokument) in eine DocumentDB-Sammlung ein.

> [AZURE.TIP] Für beide Controller werden `async Task<dynamic>`-Vorgänge verwendet. So können Vorgänge zur Laufzeit aufgelöst werden, damit wir das DocumentDB-Schema im Text des Vorgangs erstellen können.

Jedes Dokument in DocumentDB muss über eine eindeutige ID verfügen. Wir verwenden die `PatientId` und fügen einen Zeitstempel hinzu, der in einen Unix-Zeitstempelwert (double) konvertiert wird. Wir schneiden ihn ab, um den Bruchteil zu entfernen.

Sie können den Quellcode unserer Fehlercontroller-API [hier bei GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs) anzeigen.

Wir rufen die API aus einer Logik-App mit der folgenden Syntax auf:

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

Mit dem Ausdruck im obigen Codebeispiel wird geprüft, ob *Create\_NewPatientRecord* den Status **Failed** aufweist.

## Zusammenfassung 

- Sie können die Protokollierung und Fehlerbehandlung in einer Logik-App leicht implementieren.
- DocumentDB kann als Repository für Protokoll- und Fehlerdatensätze (Dokumente) genutzt werden.
- Sie können MVC verwenden, um ein Portal zum Anzeigen von Protokoll- und Fehlerdatensätzen zu erstellen.

### Quellcode
Der Quellcode für die API-Anwendung zur Logik-App-Ausnahmeverwaltung ist in [diesem GitHub-Repository](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Ausnahmeverwaltungs-API für Logik-App") verfügbar.


## Nächste Schritte 
- [Anzeigen weiterer Logik-App-Beispiele und -Szenarien](app-service-logic-examples-and-scenarios.md)
- [Informationen zu Überwachungstools für Logik-Apps](app-service-logic-monitor-your-logic-apps.md)
- [Erstellen einer automatisierten Bereitstellungsvorlage für Logik-Apps](app-service-logic-create-deploy-template.md)

<!---HONumber=AcomDC_0803_2016-->