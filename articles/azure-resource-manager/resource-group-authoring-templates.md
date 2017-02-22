---
title: "Erstellen von Vorlagen für Azure-Bereitstellungen | Microsoft-Dokumentation"
description: Beschreibt die Struktur und die Eigenschaften der Azure Resource Manager-Vorlagen mithilfe deklarativer JSON-Syntax.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 2a9075f4c9f10d05df3b275a39b3629d4ffd095f
ms.openlocfilehash: 52fe8e3ce0c9c94c918818784fd735b5a6486ed8


---
# <a name="authoring-azure-resource-manager-templates"></a>Erstellen von Azure Resource Manager-Vorlagen
In diesem Thema wird die Struktur einer Azure Resource Manager-Vorlage beschrieben. Er zeigt die verschiedenen Abschnitte einer Vorlage und die Eigenschaften, die in diesen Abschnitten verfügbar sind. Die Vorlage besteht aus JSON-Code und Ausdrücken, mit denen Sie Werte für Ihre Bereitstellung erstellen können. 

Informationen zum Anzeigen der Vorlage für Ressourcen, die Sie bereits bereitgestellt haben, finden Sie unter [Exportieren einer Azure Resource Manager-Vorlage aus vorhandenen Ressourcen](resource-manager-export-template.md). Ein Leitfaden zum Erstellen einer Vorlage finden Sie unter [Resource Manager-Vorlage – Exemplarische Vorgehensweise](resource-manager-template-walkthrough.md). Empfehlungen zum Erstellen von Vorlagen finden Sie unter [Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen](resource-manager-template-best-practices.md).

Ein guter JSON-Editor kann das Erstellen von Vorlagen vereinfachen. Informationen zum Verwenden von Visual Studio mit Vorlagen finden Sie unter [Erstellen und Bereitstellen von Azure-Ressourcengruppen über Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). Informationen zur Verwendung von VS Code finden Sie unter [Verwenden von Azure Resource Manager-Vorlagen in Visual Studio Code](resource-manager-vs-code.md).

Begrenzen Sie die Größe der Vorlage auf 1 MB und die jeder Parameterdatei auf 64 KB. Die 1-MB-Beschränkung gilt für den endgültigen Status der Vorlage, nachdem sie durch iterative Ressourcendefinitionen und Werte für Variablen und Parameter erweitert wurde. 

## <a name="template-format"></a>Vorlagenformat
In der einfachsten Struktur enthält eine Vorlage die folgenden Elemente:

```json
{
   "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "",
   "parameters": {  },
   "variables": {  },
   "resources": [  ],
   "outputs": {  }
}
```

| Elementname | Erforderlich | Beschreibung |
|:--- |:--- |:--- |
| $schema |Ja |Speicherort der JSON-Schemadatei, die die Version der Vorlagensprache beschreibt. Verwenden Sie die im vorherigen Beispiel gezeigte URL. |
| contentVersion |Ja |Version der Vorlage (z. B. 1.0.0.0). Sie können einen beliebigen Wert für dieses Element angeben. Bei der Bereitstellung von Ressourcen mithilfe der Vorlage kann mit diesem Wert sichergestellt werden, dass die richtige Vorlage verwendet wird. |
| parameters |Nein |Werte, die bei der Bereitstellung angegeben werden, um die Bereitstellung der Ressourcen anpassen. |
| variables |Nein |Werte, die als JSON-Fragmente in der Vorlage verwendet werden, um Vorlagensprachausdrücke zu vereinfachen. |
| resources |Ja |Ressourcentypen, die in einer Ressourcengruppe bereitgestellt oder aktualisiert werden. |
| outputs |Nein |Werte, die nach der Bereitstellung zurückgegeben werden. |

Weiter unten in diesem Thema untersuchen wir die Abschnitte der Vorlage ausführlich.

## <a name="expressions-and-functions"></a>Ausdrücke und Funktionen
Die grundlegende Syntax der Vorlage ist JSON. Allerdings erweitern Ausdrücke und Funktionen den JSON-Code, der in der Vorlage zur Verfügung steht. Mit Ausdrücken erstellen Sie Werte, die keine strikten Literalwerte sind. Ausdrücke werden in Klammern eingeschlossen (`[` und`]`) und beim Bereitstellen der Vorlage ausgewertet. Ausdrücke können an beliebiger Stelle in einem JSON-Zeichenfolgenwert auftreten und geben immer einen anderen JSON-Wert zurück. Wenn Sie ein Zeichenfolgenliteral verwenden müssen, das mit einer eckigen Klammer (`[`) beginnt, müssen Sie zwei eckige Klammern verwenden: `[[`.

In der Regel verwenden Sie Ausdrücke mit Funktionen, um Vorgänge zum Konfigurieren der Bereitstellung durchzuführen. Genau wie in JavaScript haben Funktionsaufrufe das Format **functionName(arg1,arg2,arg3)**. Auf Eigenschaften verweisen Sie mithilfe der Operatoren Punkt und [Index].

Das folgende Beispiel zeigt, wie Sie verschiedene Funktionen beim Erstellen von Werten verwenden:

```json
"variables": {
   "location": "[resourceGroup().location]",
   "usernameAndPassword": "[concat(parameters('username'), ':', parameters('password'))]",
   "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
}
```

Unter [Funktionen von Azure-Ressourcen-Manager-Vorlagen](resource-group-template-functions.md)finden Sie eine vollständige Liste der Vorlagenfunktionen. 

## <a name="parameters"></a>Parameter
Im Abschnitt „Parameter“ der Vorlage geben Sie an, welche Werte Sie beim Bereitstellen der Ressourcen eingeben können. Mit diesen Parameterwerten können Sie die Bereitstellung anpassen, indem Sie Werte angeben, die für eine bestimmte Umgebung (z. B. Entwicklung, Testing oder Produktion) maßgeschneidert sind. Sie müssen in der Vorlage nicht unbedingt Parameter angeben, aber ohne Parameter stellt Ihre Vorlage immer dieselben Ressourcen mit den gleichen Namen, Speicherorten und Eigenschaften bereit.

Sie können diese Parameterwerte in der Vorlage zum Festlegen von Werten für die bereitgestellten Ressourcen verwenden. Nur die im Parameterabschnitt deklarierten Parameter können in anderen Abschnitten der Vorlage verwendet werden.

Sie definieren Parameter mit der folgenden Struktur:

```json
"parameters": {
   "<parameter-name>" : {
     "type" : "<type-of-parameter-value>",
     "defaultValue": "<default-value-of-parameter>",
     "allowedValues": [ "<array-of-allowed-values>" ],
     "minValue": <minimum-value-for-int>,
     "maxValue": <maximum-value-for-int>,
     "minLength": <minimum-length-for-string-or-array>,
     "maxLength": <maximum-length-for-string-or-array-parameters>,
     "metadata": {
         "description": "<description-of-the parameter>" 
     }
   }
}
```

| Elementname | Erforderlich | Beschreibung |
|:--- |:--- |:--- |
| parameterName |Ja |Der Name des Parameters. Es muss sich um einen gültigen JavaScript-Bezeichner handeln. |
| Typ |Ja |Der Typ des Parameterwerts. Informationen finden Sie nach dieser Tabelle in der Liste der zulässigen Typen. |
| defaultValue |Nein |Der Standardwert für den Parameter, wenn kein Wert für den Parameter angegeben wird. |
| allowedValues |Nein |Ein Array der zulässigen Werte für den Parameter, um sicherzustellen, dass der richtige Wert angegeben wird. |
| minValue |Nein |Der Mindestwert für Parameter vom Typ "int", einschließlich des angegebenen Werts. |
| maxValue |Nein |Der Höchstwert für Parameter vom Typ "int", einschließlich des angegebenen Werts. |
| minLength |Nein |Die Mindestlänge der Parameter „string“, „secureString“ und „array“, einschließlich des angegebenen Werts. |
| maxLength |Nein |Die Höchstlänge der Parameter „string“, „secureString“ und „array“, einschließlich des angegebenen Werts. |
| Beschreibung |Nein |Beschreibung des Parameters, der Benutzern der Vorlage über die benutzerdefinierte Vorlagenschnittstelle des Portals angezeigt wird. |

Die zulässigen Typen und Werte lauten folgendermaßen:

* **string**
* **secureString**
* **int**
* **bool**
* **object** 
* **secureObject**
* **array**

Um einen Parameter als optional anzugeben, geben Sie einen Standardwert (kann eine leere Zeichenfolge sein) an. 

Wenn Sie einen Parameternamen in der Vorlage angeben, die einem Parameter im Befehl zum Bereitstellen der Vorlage entspricht, sind die bereitgestellten Werte möglicherweise mehrdeutig. Resource Manager löst diese Probleme durch Hinzufügen des Postfix-Elements **FromTemplate** zum Vorlagenparameter. Beispiel: Falls Sie einen Parameter namens **ResourceGroupName** in Ihrer Vorlage einfügen, wird ein Konflikt mit dem Parameter **ResourceGroupName** im Cmdlet [New-AzureRmResourceGroupDeployment][deployment2cmdlet] verursacht. Sie werden während der Bereitstellung zur Eingabe eines Werts für **ResourceGroupNameFromTemplate** aufgefordert. Im Allgemeinen sollten Sie diese Verwirrung vermeiden, indem Sie Parametern nicht dieselben Namen wie Parametern für Bereitstellungsvorgänge geben.

> [!NOTE]
> Für Kennwörter, Schlüssel und andere geheime Informationen sollte der Typ **secureString** verwendet werden. Wenn Sie vertrauliche Daten an ein JSON-Objekt übergeben, verwenden Sie den Typ **secureObject**. Vorlagenparameter des Typs „secureString“ oder „secureObject“ können nach der Bereitstellung der Ressource nicht mehr gelesen werden. 
> 
> Im folgenden Eintrag im Bereitstellungsverlauf werden beispielsweise die Werte für eine Zeichenfolge und ein Objekt, jedoch nicht für „secureString“ und „secureObject“ angezeigt.
>
> ![Anzeigen der Bereitstellungswerte](./media/resource-group-authoring-templates/show-parameters.png)  
>

Im folgenden Beispiel wird veranschaulicht, wie Sie Parameter definieren:

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]"
  },
  "skuName": {
    "type": "string",
    "defaultValue": "F1",
    "allowedValues": [
      "F1",
      "D1",
      "B1",
      "B2",
      "B3",
      "S1",
      "S2",
      "S3",
      "P1",
      "P2",
      "P3",
      "P4"
    ]
  },
  "skuCapacity": {
    "type": "int",
    "defaultValue": 1,
    "minValue": 1
  }
}
```

Informationen zum Eingeben der Parameterwerte während der Bereitstellung finden Sie unter [Bereitstellen einer Anwendung mit einer Azure Resource Manager-Vorlage](resource-group-template-deploy.md). 

## <a name="variables"></a>Variablen
Im Abschnitt „Variablen“ erstellen Sie Werte, die in der ganzen Vorlage verwendet werden können. Variablen basieren zumeist auf Werten, die von den Parametern stammen. Sie müssen nicht unbedingt Variablen definieren, aber häufig bewirken sie eine Vereinfachung Ihrer Vorlage, indem komplexe Ausdrücke reduziert werden.

Sie definieren Variablen mit der folgenden Struktur:

```json
"variables": {
   "<variable-name>": "<variable-value>",
   "<variable-name>": { 
       <variable-complex-type-value> 
   }
}
```

Das folgende Beispiel zeigt, wie Sie eine Variable definieren, die aus zwei Parameterwerten erstellt wird:

```json
"variables": {
    "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
}
```

Das nächste Beispiel zeigt eine Variable mit einem komplexen JSON-Typ sowie Variablen, die aus anderen Variablen erstellt werden:

```json
"parameters": {
   "environmentName": {
     "type": "string",
     "allowedValues": [
       "test",
       "prod"
     ]
   }
},
"variables": {
   "environmentSettings": {
     "test": {
       "instancesSize": "Small",
       "instancesCount": 1
     },
     "prod": {
       "instancesSize": "Large",
       "instancesCount": 4
     }
   },
   "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
   "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
   "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
}
```

## <a name="resources"></a>angeben
Im Ressourcenabschnitt definieren Sie die Ressourcen, die bereitgestellt oder aktualisiert werden. Dieser Abschnitt kann komplizierter werden, da Sie die Typen, die sie bereitstellen, verstehen müssen, um die richtigen Werte angeben zu können. 

Sie definieren Ressourcen mit der folgenden Struktur:

```json
"resources": [
   {
     "apiVersion": "<api-version-of-resource>",
     "type": "<resource-provider-namespace/resource-type-name>",
     "name": "<name-of-the-resource>",
     "location": "<location-of-resource>",
     "tags": "<name-value-pairs-for-resource-tagging>",
     "comments": "<your-reference-notes>",
     "dependsOn": [
       "<array-of-related-resource-names>"
     ],
     "properties": "<settings-for-the-resource>",
     "copy": {
       "name": "<name-of-copy-loop>",
       "count": "<number-of-iterations>"
     },
     "resources": [
       "<array-of-child-resources>"
     ]
   }
]
```

| Elementname | Erforderlich | Beschreibung |
|:--- |:--- |:--- |
| apiVersion |Ja |Version der REST-API zum Erstellen der Ressource. |
| type |Ja |Der Typ der Ressource. Dieser Wert ist eine Kombination aus dem Namespace des Ressourcenanbieters und dem Ressourcentyp (z.B. **Microsoft.Storage/storageAccounts**). |
| Name |Ja |Der Name der Ressource. Der Name muss die Einschränkungen für URI-Komponenten laut Definition in RFC3986 erfüllen. Darüber hinaus überprüfen Azure-Dienste, die externen Parteien den Ressourcennamen verfügbar machen, den Namen, um sicherzustellen, dass es sich nicht um einen Versuch handelt, eine andere Identität vorzutäuschen. Siehe [Überprüfen des Ressourcennamens](https://msdn.microsoft.com/library/azure/mt219035.aspx). |
| location |Variabel |Unterstützte Standorte der angegebenen Ressource Wählen Sie einen der verfügbaren Standorte. In der Regel ist es jedoch sinnvoll, einen in der Nähe der Benutzer zu wählen. Normalerweise ist es auch sinnvoll, Ressourcen, die miteinander interagieren, in der gleichen Region zu platzieren. eine Rollenzuweisung) jedoch nicht. |
| tags |Nein |Markierungen, die der Ressource zugeordnet sind |
| Kommentare |Nein |Ihre Notizen zur Dokumentierung der Ressourcen in Ihrer Vorlage |
| dependsOn |Nein |Ressourcen, die bereitgestellt werden müssen, bevor diese Ressource bereitgestellt wird. Resource Manager wertet die Abhängigkeiten zwischen den Ressourcen aus und stellt sie in der richtigen Reihenfolge bereit. Wenn Ressourcen nicht voneinander abhängig sind, werden sie parallel bereitgestellt. Der Wert kann eine durch Trennzeichen getrennte Liste von Ressourcennamen oder eindeutigen Ressourcenbezeichnern sein. Es werden nur Ressourcen aufgelistet, die in dieser Vorlage bereitgestellt werden. Ressourcen, die nicht in dieser Vorlage definiert sind, müssen bereits vorhanden sein. Vermeiden Sie das Hinzufügen unnötiger Abhängigkeiten, da diese die Bereitstellung verlangsamen und Ringabhängigkeiten schaffen können. Tipps für das Festlegen von Abhängigkeiten finden Sie unter [Definieren von Abhängigkeiten in Azure Resource Manager-Vorlagen](resource-group-define-dependencies.md). |
| Eigenschaften |Nein |Ressourcenspezifische Konfigurationseinstellungen. Die Werte für die Eigenschaften sind mit den Werten identisch, die Sie im Anforderungstext für den REST-API-Vorgang (PUT-Methode) angegeben haben, um die Ressource zu erstellen. Links zur Ressourcenschemadokumentation oder REST-API finden Sie unter [Anbieter, Regionen, API-Versionen und Schemas für Resource Manager](resource-manager-supported-services.md). |
| copy |Nein |Wenn mehr als eine Instanz erforderlich ist, die Anzahl der zu erstellenden Ressourcen. Weitere Informationen finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen in Azure Resource Manager](resource-group-create-multiple.md). |
| resources |Nein |Untergeordnete Ressourcen, die von der definierten Ressource abhängig sind. Stellen Sie nur Ressourcentypen bereit, die laut Schema der übergeordneten Ressource zulässig sind. Der vollqualifizierte Typ der untergeordneten Ressource enthält den übergeordneten Ressourcentyp, z.B. **Microsoft.Web/sites/extensions**. Eine Abhängigkeit von der übergeordneten Ressource ist nicht impliziert. Sie müssen diese Abhängigkeit explizit definieren. |

Zu wissen, welche Werte für **apiVersion**, **type** und **location** anzugeben sind, ist nicht sofort ersichtlich. Glücklicherweise können Sie diese Werte mithilfe von Azure PowerShell oder der Azure-Befehlszeilenschnittstelle (CLI) bestimmen.

Zum Abrufen aller Ressourcenanbieter mit **PowerShell** geben Sie Folgendes an:

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

Suchen Sie in der zurückgegebenen Liste die Ressourcenanbieter, die Sie interessieren. Verwenden Sie zum Abrufen der Ressourcentypen für einen Ressourcenanbieter (z.B. Speicher) Folgendes:

```powershell
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes
```

Verwenden Sie zum Abrufen der API-Versionen für einen Ressourcentyp (z.B. Speicherkonten) Folgendes:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).ApiVersions
```

Um die unterstützten Speicherorte für einen Ressourcentyp abzurufen, verwenden Sie Folgendes:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).Locations
```

Zum Abrufen aller Ressourcenanbieter mit der **Azure CLI** geben Sie Folgendes an:

```azurecli
azure provider list
```

Suchen Sie in der zurückgegebenen Liste die Ressourcenanbieter, die Sie interessieren. Verwenden Sie zum Abrufen der Ressourcentypen für einen Ressourcenanbieter (z.B. Speicher) Folgendes:

```azurecli
azure provider show Microsoft.Storage
```

Um unterstützte Speicherorte und API-Versionen abzurufen, verwenden Sie Folgendes:

```azurecli
azure provider show Microsoft.Storage --details --json
```

Weitere Informationen zu den Ressourcenanbietern finden Sie unter [Anbieter, Regionen, API-Versionen und Schemas für Resource Manager](resource-manager-supported-services.md).

Der Abschnitt „Ressourcen“ enthält ein Array mit den bereitzustellenden Ressourcen. Innerhalb jeder Ressource können Sie auch ein Array untergeordneter Ressourcen definieren. Der Abschnitt „Ressourcen“ kann daher beispielsweise folgende Struktur aufweisen:

```json
"resources": [
   {
       "name": "resourceA",
   },
   {
       "name": "resourceB",
       "resources": [
           {
               "name": "firstChildResourceB",
           },
           {   
               "name": "secondChildResourceB",
           }
       ]
   },
   {
       "name": "resourceC",
   }
]
```      

Das folgende Beispiel enthält die Ressource **Microsoft.Web/serverfarms** und die Ressource **Microsoft.Web/Sites** mit der untergeordneten Ressource **Extensions**. Beachten Sie, dass die Website als abhängig von der Serverfarm markiert ist, da die Serverfarm vorhanden sein muss, bevor die Website bereitgestellt werden kann. Sie sehen auch, dass die **Extensions** -Ressource ein untergeordnetes Element der Website ist.

```json
"resources": [
  {
    "apiVersion": "2015-08-01",
    "name": "[parameters('hostingPlanName')]",
    "type": "Microsoft.Web/serverfarms",
    "location": "[resourceGroup().location]",
    "tags": {
      "displayName": "HostingPlan"
    },
    "sku": {
      "name": "[parameters('skuName')]",
      "capacity": "[parameters('skuCapacity')]"
    },
    "properties": {
      "name": "[parameters('hostingPlanName')]",
      "numberOfWorkers": 1
    }
  },
  {
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[parameters('siteName')]",
    "location": "[resourceGroup().location]",
    "tags": {
      "environment": "test",
      "team": "Web"
    },
    "dependsOn": [
      "[concat(parameters('hostingPlanName'))]"
    ],
    "properties": {
      "name": "[parameters('siteName')]",
      "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
    },
    "resources": [
      {
        "apiVersion": "2015-08-01",
        "type": "extensions",
        "name": "MSDeploy",
        "dependsOn": [
          "[concat('Microsoft.Web/sites/', parameters('siteName'))]"
        ],
        "properties": {
          "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
          "dbType": "None",
          "connectionString": "",
          "setParameters": {
            "Application Path": "[parameters('siteName')]"
          }
        }
      }
    ]
  }
]
```

## <a name="outputs"></a>Ausgaben
Im Ausgabenabschnitt legen Sie Werte fest, die von der Bereitstellung zurückgegeben werden. Sie könnten z. B. den URI für den Zugriff auf eine bereitgestellte Ressource zurückgeben.

Das folgende Beispiel zeigt die Struktur einer Ausgabedefinition:

```json
"outputs": {
   "<outputName>" : {
     "type" : "<type-of-output-value>",
     "value": "<output-value-expression>"
   }
}
```

| Elementname | Erforderlich | Beschreibung |
|:--- |:--- |:--- |
| outputName |Ja |Name des Ausgabewerts. Es muss sich um einen gültigen JavaScript-Bezeichner handeln. |
| Typ |Ja |Der Typ des Ausgabewerts. Ausgabewerte unterstützen dieselben Typen wie Vorlagen-Eingabeparameter. |
| value |Ja |Vorlagensprachausdruck, der ausgewertet und als Ausgabewert zurückgegeben wird. |

Das folgende Beispiel zeigt einen Wert, der im Ausgabeabschnitt zurückgegeben wird.

```json
"outputs": {
   "siteUri" : {
     "type" : "string",
     "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
   }
}
```

Weitere Informationen zum Arbeiten mit Vorlagen finden Sie unter [Freigeben des Status in Azure-Ressourcen-Manager-Vorlagen](best-practices-resource-manager-state.md).

## <a name="next-steps"></a>Nächste Schritte
* Komplette Vorlagen für viele verschiedene Lösungstypen finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/).
* Ausführliche Informationen zu den Funktionen, die Sie innerhalb einer Vorlage nutzen können, finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](resource-group-template-functions.md).
* Informationen zum Zusammenführen mehrerer Vorlagen während der Bereitstellung finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure Resource Manager](resource-group-linked-templates.md).
* Möglicherweise müssen Sie Ressourcen verwenden, die in einer anderen Ressourcengruppe enthalten sind. Dieses Szenario ist bei der Arbeit mit Speicherkonten oder virtuellen Netzwerken üblich, die in mehreren Ressourcengruppen gemeinsam verwendet werden. Weitere Informationen finden Sie unter der [resourceId-Funktion](resource-group-template-functions.md#resourceid).

[deployment2cmdlet]: https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.2.0/new-azurermresourcegroupdeployment



<!--HONumber=Jan17_HO4-->


