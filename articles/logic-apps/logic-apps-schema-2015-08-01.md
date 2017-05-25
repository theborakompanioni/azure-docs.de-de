---
title: "Schemaaktualisierungen für Azure Logic Apps – 1. August 2015 (Vorschau) | Microsoft-Dokumentation"
description: "Erstellen von JSON-Definitionen für Azure Logic Apps mit der Schemaversion „2015-08-01-preview“."
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 0d03a4d4-e8a8-4c81-aed5-bfd2a28c7f0c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 05/31/2016
ms.author: LADocs; stepsic
ms.translationtype: Human Translation
ms.sourcegitcommit: a3ae5ce7d16ac9ed3a06c313ff3c48e8cbf79c54
ms.openlocfilehash: f1bf19e6dfbcf187635730a53c93162244c17c6a
ms.contentlocale: de-de
ms.lasthandoff: 03/01/2017


---
# <a name="schema-updates-for-azure-logic-apps---august-1-2015-preview"></a>Schemaaktualisierungen für Azure Logic Apps – 1. August 2015 (Vorschau)

Diese neue Schema- und API-Version für Azure Logic Apps enthält wichtige Verbesserungen, mit denen Logik-Apps zuverlässiger und einfacher zu verwenden sind:

*    Der **APIApp**-Aktionstyp wird auf einen neuen [**APIConnection**](#api-connections)-Aktionstyp aktualisiert.
*    **Repeat** wird in [**Foreach**](#foreach) umbenannt.
*    Die [**HTTP-Listener**-API-App](#http-listener) ist nicht mehr erforderlich.
*    Für den Aufruf untergeordneter Workflows wird ein [neues Schema](#child-workflows) verwendet.

<a name="api-connections"></a>
## <a name="move-to-api-connections"></a>Wechsel zu API-Verbindungen

Die größte Änderung besteht darin, dass Sie zur Verwendung von APIs keine API-Apps in Ihrem Azure-Abonnement bereitstellen müssen. Hier sind die Verwendungsmöglichkeiten für APIs:

* Verwaltete APIs
* Ihre benutzerdefinierten Web-APIs

Jede dieser Möglichkeiten wird etwas anders gehandhabt, da sich ihre Verwaltungs- und Hostingmodelle unterscheiden. Ein Vorteil dieses Modells besteht darin, dass Sie nicht mehr auf in Ihrer Azure-Ressourcengruppe bereitgestellte Ressourcen beschränkt sind. 

### <a name="managed-apis"></a>Verwaltete APIs

Microsoft verwaltet einige APIs in Ihrem Namen, z.B. Office 365, Salesforce, Twitter und FTP. Sie können einige dieser verwalteten APIs ohne weitere Bearbeitung verwenden, etwa Bing Translate, wohingegen andere APIs konfiguriert werden müssen. Diese Konfiguration wird als *Verbindung* (Connection) bezeichnet.

Wenn Sie Office 365 verwenden, müssen Sie beispielsweise eine Verbindung erstellen, die Ihr Office 365-Anmeldetoken enthält. Dieses Token wird sicher gespeichert und aktualisiert, damit Ihre Logik-App jederzeit die Office 365-API aufrufen kann. Wenn Sie eine Verbindung mit Ihrer SQL Server-Instanz oder dem FTP-Server herstellen möchten, müssen Sie eine Verbindung erstellen, die die Verbindungszeichenfolge enthält. 

Innerhalb dieser Definition werden diese Aktionen als `APIConnection` bezeichnet. Hier sehen Sie ein Beispiel für eine Verbindung, die Office 365 zum Senden einer E-Mail aufruft:

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

Das `host`-Objekt stellt den Teil der Eingaben dar, der nur für API-Verbindungen gilt. Es besteht aus zwei Teilen: `api` und `connection`.

`api` enthält die Laufzeit-URL, unter der die verwaltete API gehostet wird. Durch den Aufruf von `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview` können Sie alle verfügbaren verwalteten APIs anzeigen.

Wenn Sie eine API verwenden, müssen für die API nicht unbedingt *Verbindungsparameter* definiert sein. Sind für die API keine Parameter definiert, ist keine *Verbindung* erforderlich. Sind für die API Parameter definiert, müssen Sie eine Verbindung erstellen. Die erstellte Verbindung hat den Namen, den Sie auswählen. Sie verweisen dann im `connection`-Objekt innerhalb des `host`-Objekts auf den Namen. Rufen Sie zum Erstellen einer Verbindung in einer Ressourcengruppe Folgendes auf:

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

Verwenden Sie folgenden Text:

```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues": {
        "accountName": "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location": "{Logic app's location}"
}
```

### <a name="deploy-managed-apis-in-an-azure-resource-manager-template"></a>Bereitstellen von verwalteten APIs in einer Azure Resource Manager-Vorlage

Sie können eine vollständige Anwendung in einer Azure Resource Manager-Vorlage erstellen, solange keine interaktive Anmeldung erforderlich ist.
Ist eine Anmeldung erforderlich, können Sie alle Elemente mit der Azure Resource Manager-Vorlage einrichten, müssen jedoch zum Autorisieren der Verbindungen das Portal aufrufen. 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": ["[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/', parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:, Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

Sie sehen in diesem Beispiel, dass es sich bei den Verbindungen lediglich um Ressourcen in der Ressourcengruppe handelt. Sie verweisen auf die verwalteten APIs, die Ihnen in Ihrem Abonnement zur Verfügung stehen.

### <a name="your-custom-web-apis"></a>Ihre benutzerdefinierten Web-APIs

Wenn Sie eigene (also nicht von Microsoft verwaltete) APIs einsetzen, verwenden Sie für das Aufrufen die integrierte **HTTP**-Aktion. Für die optimale Nutzung sollten Sie einen Swagger-Endpunkt für Ihre API verfügbar machen. Durch diesen Endpunkt kann der Logik-App-Designer die Ein- und Ausgaben für Ihre API rendern. Ohne Swagger kann der Designer die Ein- und Ausgaben nur als nicht transparente JSON-Objekte anzeigen.

Hier sehen Sie ein Beispiel für die neue `metadata.apiDefinitionUrl` -Eigenschaft:

```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata": {
              "apiDefinitionUrl": "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method": "GET"
            }
        }
    }
}
```

Wenn Sie Ihre Web-API in Azure App Service hosten, wird die Web-API automatisch in der Liste der im Designer verfügbaren Aktionen angezeigt. Andernfalls müssen Sie die URL direkt einfügen. Der Swagger-Endpunkt darf nicht authentifiziert sein, damit er im Logik-App-Designer verwendet werden kann, aber Sie können die API selbst mit den von Swagger unterstützten Methoden sichern.

### <a name="call-deployed-api-apps-with-2015-08-01-preview"></a>Aufrufen bereitgestellter API-Apps mit „2015-08-01-preview“

Wenn Sie zuvor eine API-App bereitgestellt haben, können Sie sie über die **HTTP**-Aktion aufrufen.

Beispiel: Wenn Sie Dropbox zum Auflisten von Dateien verwenden, ist in der Schemaversionsdefinition **2014-12-01-preview** etwa Folgendes enthalten:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Sie können die entsprechende HTTP-Aktion gemäß diesem Beispiel erstellen, während der Parameterabschnitt der Logik-App-Definition unverändert bleibt.

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata": {
              "apiDefinitionUrl": "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method": "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Informationen zu den einzelnen Eigenschaften:

| Aktionseigenschaft | Beschreibung |
| --- | --- |
| `type` |`Http` anstelle von `APIapp` |
| `metadata.apiDefinitionUrl` |Wenn Sie diese Aktion im Logik-App-Designer verwenden möchten, sollten Sie den Metadatenendpunkt aufnehmen. Er wird erstellt aus: `{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` |Erstellt aus: `{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` |Immer `POST` |
| `inputs.body` |Identisch mit den API-App-Parametern |
| `inputs.authentication` |Identisch mit der API-App-Authentifizierung |

Dieser Ansatz sollte bei allen API-App-Aktionen funktionieren. Beachten Sie jedoch, dass diese früheren API-Apps nicht mehr unterstützt werden. Daher sollten Sie auf eine der beiden anderen vorherigen Optionen umsteigen, eine verwaltete API oder Hosten Ihrer benutzerdefinierten Web-API.

<a name="foreach"></a>
## <a name="renamed-repeat-to-foreach"></a>„Repeat“ umbenannt in „Foreach“

Für die vorherige Schemaversion haben wir umfangreiches Kundenfeedback mit dem Hinweis erhalten, dass bei **Repeat** nicht ganz klar sei, dass es sich bei **Repeat** tatsächlich um eine For-Each-Schleife handelt. Daher wurde `repeat` in `foreach` umbenannt. Beispielsweise hätten Sie zuvor geschrieben:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

Jetzt würden Sie schreiben:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

Zuvor wurde mit der Funktion `@repeatItem()` auf das aktuelle Element verwiesen, das durchlaufen wurde. Diese Funktion ist nun zu `@item()` vereinfacht. 

### <a name="reference-outputs-from-foreach"></a>Referenzausgaben von „foreach“

Zur Vereinfachung sind Ausgaben von `foreach`-Aktionen nicht in ein Objekt namens `repeatItems` eingeschlossen. Die Ausgaben des vorherigen `repeat`-Beispiels waren:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Jetzt lauten diese Ausgaben:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Bisher mussten Sie zum Aufrufen des Texts der Aktion beim Verweisen auf diese Ausgaben Folgendes verwenden:

```
{
    "actions": {
        "secondAction": {
            "type": "Http",
            "repeat": "@outputs('pingBing').repeatItems",
            "inputs": {
                "method": "POST",
                "uri": "http://www.example.com",
                "body": "@repeatItem().outputs.body"
            }
        }
    }
}
```

Nun führen Sie stattdessen Folgendes aus:

```
{
    "actions": {
        "secondAction": {
            "type": "Http",
            "foreach": "@outputs('pingBing')",
            "inputs": {
                "method": "POST",
                "uri": "http://www.example.com",
                "body": "@item().outputs.body"
            }
        }
    }
}
```

Infolge dieser Änderungen werden die Funktionen `@repeatItem()`, `@repeatBody()` und `@repeatOutputs()` entfernt.

<a name="http-listener"></a>
## <a name="native-http-listener"></a>Nativer HTTP-Listener

Die HTTP-Listener-Funktionen sind jetzt integriert, daher müssen Sie keine HTTP-Listener-API-App mehr bereitstellen. Ausführliche Informationen dazu, wie Sie den Logik-App-Endpunkt aufrufbar machen, finden Sie [hier](../logic-apps/logic-apps-http-endpoint.md). 

Im Rahmen dieser Änderungen haben wir die `@accessKeys()`-Funktion entfernt und durch die `@listCallbackURL()`-Funktion zum Abrufen des Endpunkts (wenn erforderlich) ersetzt. Außerdem müssen Sie jetzt mindestens einen Trigger in Ihrer Logik-App definieren. Wenn Sie für den Workflow `/run` ausführen möchten, benötigen Sie einen der folgenden Trigger: `manual`, `apiConnectionWebhook` oder `httpWebhook`.

<a name="child-workflows"></a>
## <a name="call-child-workflows"></a>Aufrufen von untergeordneten Workflows

Zuvor mussten Sie zum Aufrufen von untergeordneten Workflows den gewünschten Workflow aufrufen, das Zugriffstoken abrufen und dieses Token in die Definition der Logik-App einfügen, die den untergeordneten Workflow aufrufen sollte. Mit dem neuen Schema generiert das Logic Apps-Modul zur Laufzeit automatisch eine SAS für den untergeordneten Workflow, sodass Sie keine geheimen Schlüssel in die Definition einfügen müssen. Beispiel:

```
"mynestedwf": {
    "type": "workflow",
    "inputs": {
        "host": {
            "id": "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName": "myendpointtrigger"
        },
        "queries": {
            "extrafield": "specialValue"
        },
        "headers": {
            "x-ms-date": "@utcnow()",
            "Content-type": "application/json"
        },
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        }
    },
    "conditions": []
}
```

Eine zweite Verbesserung ist, dass die untergeordneten Workflows vollen Zugriff auf die eingehende Anforderung erhalten. Dies bedeutet, dass Sie Parameter im Abschnitt *queries* und im Objekt *headers* übergeben und den gesamten Text vollständig festlegen können.

Schließlich müssen Änderungen am untergeordneten Workflow vorgenommen werden. Zuvor konnten Sie einen untergeordneten Workflow direkt aufrufen. Nun müssen Sie einen Triggerendpunkt im Workflow festlegen, den der übergeordnete Workflow aufruft. Im Allgemeinen fügen Sie einen Trigger vom Typ `manual` hinzu und verwenden ihn dann in der übergeordneten Definition. Beachten Sie, dass die `host`-Eigenschaft ein spezielles `triggerName`-Element enthält, da Sie immer angeben müssen, welchen Trigger Sie aufrufen.

## <a name="other-changes"></a>Weitere Änderungen

### <a name="new-queries-property"></a>Neue „queries“-Eigenschaft

Alle Aktionstypen unterstützen jetzt eine neue Eingabe namens `queries`. Diese Eingabe kann ein strukturiertes Objekt sein, sodass Sie die Zeichenfolge nicht manuell zusammensetzen müssen.

### <a name="renamed-parse-function-to-json"></a>Funktion „parse()“ umbenannt in „json()“

Wir fügen bald weitere Inhaltstypen hinzu, daher wurde die `parse()`-Funktion in `json()` umbenannt.

## <a name="coming-soon-enterprise-integration-apis"></a>In Kürze verfügbar: Enterprise Integration-APIs

Wir haben noch keine verwalteten Versionen der Enterprise Integration-APIs wie AS2. In der Zwischenzeit können Sie Ihre vorhandenen bereitgestellten BizTalk-APIs über die HTTP-Aktion verwenden. Ausführliche Informationen finden Sie in der [Integrationsroadmap](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/) unter „Verwenden der bereits bereitgestellten API-Apps“. 

