---
title: Erstellen von Azure Resource Manager-Vorlagen | Microsoft Docs
description: Erstellen Sie Azure-Ressourcen-Manager-Vorlagen mithilfe von deklarativer JSON-Syntax, um Anwendungen für Azure bereitzustellen.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn

ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2016
ms.author: tomfitz

---
# Erstellen von Azure Resource Manager-Vorlagen
In diesem Thema wird die Struktur einer Azure Resource Manager-Vorlage beschrieben. Er zeigt die verschiedenen Abschnitte einer Vorlage und die Eigenschaften, die in diesen Abschnitten verfügbar sind. Die Vorlage besteht aus JSON und Ausdrücken, mit denen Sie Werte für die Bereitstellung erstellen können.

Ein Leitfaden zum Erstellen einer Vorlage finden Sie unter [Resource Manager-Vorlage – Exemplarische Vorgehensweise](resource-manager-template-walkthrough.md). Empfehlungen zum Erstellen von Vorlagen finden Sie unter [Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen](resource-manager-template-best-practices.md).

Ein guter JSON-Editor kann das Erstellen von Vorlagen vereinfachen. Informationen zum Verwenden von Visual Studio mit Vorlagen finden Sie unter [Erstellen und Bereitstellen von Azure-Ressourcengruppen über Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). Informationen zur Verwendung von VS Code finden Sie unter [Verwenden von Azure Resource Manager-Vorlagen in Visual Studio Code](resource-manager-vs-code.md).

Sie müssen die Größe der Vorlage auf 1 MB und jede Parameterdatei auf 64 KB beschränken. Die 1-MB-Beschränkung gilt für den endgültigen Status der Vorlage, nachdem sie durch iterative Ressourcendefinitionen und Werte für Variablen und Parametern erweitert wurde.

## Vorlagenformat
In der einfachsten Struktur enthält eine Vorlage die folgenden Elemente.

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

| Elementname | Erforderlich | Beschreibung |
|:---:|:---:|:--- |
| $schema |Ja |Speicherort der JSON-Schemadatei, die die Version der Vorlagensprache beschreibt. Sie sollten die oben angegebene URL verwenden. |
| contentVersion |Ja |Version der Vorlage (z. B. 1.0.0.0). Sie können einen beliebigen Wert für dieses Element angeben. Bei der Bereitstellung von Ressourcen mithilfe der Vorlage kann mit diesem Wert sichergestellt werden, dass die richtige Vorlage verwendet wird. |
| parameters |Nein |Werte, die bei der Bereitstellung angegeben werden, um die Bereitstellung der Ressourcen anpassen. |
| Variablen |Nein |Werte, die als JSON-Fragmente in der Vorlage verwendet werden, um Vorlagensprachausdrücke zu vereinfachen. |
| angeben |Ja |Ressourcentypen, die in einer Ressourcengruppe bereitgestellt oder aktualisiert werden. |
| outputs |Nein |Werte, die nach der Bereitstellung zurückgegeben werden. |

Wir werden die Abschnitte der Vorlage weiter unten in diesem Thema ausführlich untersuchen. Jetzt prüfen wir den Teil der Syntax, aus dem sich die Vorlage zusammensetzt.

## Ausdrücke und Funktionen
Die grundlegende Syntax der Vorlage ist JSON. Allerdings erweitern Ausdrücke und Funktionen die JSON-Anfragen, die in der Vorlage verfügbar sind, und ermöglichen Ihnen das Erstellen von Werten, die keine strikten Literalwerte sind. Ausdrücke werden in Klammern eingeschlossen („[“ und „]“) und beim Bereitstellen der Vorlage ausgewertet. Ausdrücke können an beliebiger Stelle in einem JSON-Zeichenfolgenwert auftreten und geben immer einen anderen JSON-Wert zurück. Wenn Sie ein Zeichenfolgenliteral verwenden müssen, das mit einer eckigen Klammer [ beginnt, müssen Sie zwei eckige Klammern verwenden [[.

In der Regel verwenden Sie Ausdrücke mit Funktionen, um Vorgänge zum Konfigurieren der Bereitstellung durchzuführen. Genau wie in JavaScript haben Funktionsaufrufe das Format **functionName(arg1,arg2,arg3)**. Auf Eigenschaften verweisen Sie mithilfe der Operatoren Punkt und [Index].

Das folgende Beispiel zeigt, wie Sie verschiedene Funktionen beim Erstellen von Werten verwenden:

    "variables": {
       "location": "[resourceGroup().location]",
       "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
       "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

Unter [Funktionen von Azure-Ressourcen-Manager-Vorlagen](resource-group-template-functions.md) finden Sie eine vollständige Liste der Vorlagenfunktionen.

## Parameter
Im Abschnitt „Parameter“ der Vorlage geben Sie an, welche Werte Sie beim Bereitstellen der Ressourcen eingeben können. Mit diesen Parameterwerten können Sie die Bereitstellung anpassen, indem Sie Werte angeben, die für eine bestimmte Umgebung (z. B. Entwicklung, Testing oder Produktion) maßgeschneidert sind. Sie müssen in der Vorlage nicht unbedingt Parameter angeben, aber ohne Parameter stellt Ihre Vorlage immer dieselben Ressourcen mit den gleichen Namen, Speicherorten und Eigenschaften bereit.

Sie können diese Parameterwerte in der Vorlage zum Festlegen von Werten für die bereitgestellten Ressourcen verwenden. Nur die im Parameterabschnitt deklarierten Parameter können in anderen Abschnitten der Vorlage verwendet werden.

Sie definieren Parameter mit der folgenden Struktur:

    "parameters": {
       "<parameterName>" : {
         "type" : "<type-of-parameter-value>",
         "defaultValue": "<optional-default-value-of-parameter>",
         "allowedValues": [ "<optional-array-of-allowed-values>" ],
         "minValue": <optional-minimum-value-for-int-parameters>,
         "maxValue": <optional-maximum-value-for-int-parameters>,
         "minLength": <optional-minimum-length-for-string-secureString-array-parameters>,
         "maxLength": <optional-maximum-length-for-string-secureString-array-parameters>,
         "metadata": {
             "description": "<optional-description-of-the parameter>" 
         }
       }
    }

| Elementname | Erforderlich | Beschreibung |
|:---:|:---:|:--- |
| parameterName |Ja |Der Name des Parameters. Es muss sich um einen gültigen JavaScript-Bezeichner handeln. |
| Typ |Ja |Der Typ des Parameterwerts. Die nachstehende Liste zeigt die zulässigen Typen. |
| defaultValue |Nein |Der Standardwert für den Parameter, wenn kein Wert für den Parameter angegeben wird. |
| allowedValues |Nein |Ein Array der zulässigen Werte für den Parameter, um sicherzustellen, dass der richtige Wert angegeben wird. |
| minValue |Nein |Der Mindestwert für Parameter vom Typ "int", einschließlich des angegebenen Werts. |
| maxValue |Nein |Der Höchstwert für Parameter vom Typ "int", einschließlich des angegebenen Werts. |
| minLength |Nein |Die Mindestlänge für Parameter vom Typ "string", "secureString" und "array", einschließlich des angegebenen Werts. |
| maxLength |Nein |Die Höchstlänge für Parameter vom Typ "string", "secureString" und "array", einschließlich des angegebenen Werts. |
| description |Nein |Beschreibung des Parameters, der Benutzern der Vorlage über die benutzerdefinierte Vorlagenschnittstelle des Portals angezeigt wird. |

Die zulässigen Typen und Werte lauten folgendermaßen:

* "string" oder "secureString" – eine beliebige gültige JSON-Zeichenfolge
* "int" – eine gültige JSON-Ganzzahl
* "bool" – ein gültiger boolescher JSON-Wert
* "object" oder "secureObject" – ein gültiges JSON-Objekt
* "array" – ein gültige JSON-Array

Um einen Parameter als optional anzugeben, geben Sie einen Standardwert (kann eine leere Zeichenfolge sein) an.

Wenn Sie einen Parameternamen angeben, der einem Parameter im Befehl zum Bereitstellen der Vorlage entspricht (z.B. den Namen **ResourceGroupName** in Ihrer Vorlage, der dem **ResourceGroupName**-Parameter im Cmdlet [New-AzureRmResourceGroupDeployment][deployment2cmdlet] entspricht), werden Sie aufgefordert, einen Wert für einen Parameter mit dem Postfix **FromTemplate** anzugeben (z.B. **ResourceGroupNameFromTemplate**). Im Allgemeinen sollten Sie diese Verwirrung vermeiden, indem Sie Parametern nicht dieselben Namen wie Parametern für Bereitstellungsvorgänge geben.

> [!NOTE]
> Für Kennwörter, Schlüssel und andere geheime Informationen sollte der Typ **secureString** verwendet werden. Vorlagenparameter des Typs secureString können nach der Bereitstellung der Ressource nicht mehr gelesen werden.
> 
> 

Im folgenden Beispiel wird veranschaulicht, wie Sie Parameter definieren:

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

Informationen zum Eingeben der Parameterwerte während der Bereitstellung finden Sie unter [Bereitstellen einer Anwendung mit einer Azure Resource Manager-Vorlage](resource-group-template-deploy.md#parameter-file).

## Variablen
Im Abschnitt „Variablen“ erstellen Sie Werte, die in der ganzen Vorlage verwendet werden können. Diese Variablen basieren i. d. R. auf Werten, die von den Parametern stammen. Sie müssen nicht unbedingt Variablen definieren, aber häufig bewirken sie eine Vereinfachung Ihrer Vorlage, indem komplexe Ausdrücke reduziert werden.

Sie definieren Variablen mit der folgenden Struktur:

    "variables": {
       "<variable-name>": "<variable-value>",
       "<variable-name>": { 
           <variable-complex-type-value> 
       }
    }

Das folgende Beispiel zeigt, wie Sie eine Variable definieren, die aus zwei Parameterwerten erstellt wird:

     "variables": {
       "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
    }

Das nächste Beispiel zeigt eine Variable mit einem komplexen JSON-Typ sowie Variablen, die aus anderen Variablen erstellt werden:

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

## Ressourcen
Im Ressourcenabschnitt definieren Sie die Ressourcen, die bereitgestellt oder aktualisiert werden. In diesem Bereich kann Ihre Vorlage komplizierter werden, da Sie die Typen für die Bereitstellung kennen müssen, um die richtigen Werte angeben zu können. Viele hilfreiche Informationen zu Ressourcenanbietern finden Sie unter [Anbieter, Regionen, API-Versionen und Schemas für den Ressourcen-Manager](resource-manager-supported-services.md).

Sie definieren Ressourcen mit der folgenden Struktur:

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
         "resources": [
           "<array-of-child-resources>"
         ]
       }
    ]

| Elementname | Erforderlich | Beschreibung |
|:---:|:---:|:--- |
| apiVersion |Ja |Version der REST-API zum Erstellen der Ressource. Wie Sie die verfügbaren Versionsnummern für einen bestimmten Ressourcentyp ermitteln, erfahren Sie unter [Unterstützte API-Versionen](resource-manager-supported-services.md#supported-api-versions). |
| type |Ja |Der Typ der Ressource. Dieser Wert ist eine Kombination aus dem Namespace des Ressourcenanbieters und dem Ressourcentyp, den der Ressourcenanbieter unterstützt. |
| Name |Ja |Der Name der Ressource. Der Name muss die Einschränkungen für URI-Komponenten laut Definition in RFC3986 erfüllen. Darüber hinaus überprüfen Azure-Dienste, die externen Parteien den Ressourcennamen verfügbar machen, den Namen, um sicherzustellen, dass es sich nicht um einen Versuch handelt, eine andere Identität vorzutäuschen. Siehe [Überprüfen des Ressourcennamens](https://msdn.microsoft.com/library/azure/mt219035.aspx). |
| location |Variabel |Unterstützte Standorte der angegebenen Ressource Die verfügbaren Standorte finden Sie unter [Unterstützte Regionen](resource-manager-supported-services.md#supported-regions). Die meisten Ressourcentypen benötigen einen Standort, einige (wie z.B. eine Rollenzuweisung) jedoch nicht. |
| tags |Nein |Markierungen, die der Ressource zugeordnet sind |
| Kommentare |Nein |Ihre Notizen zur Dokumentierung der Ressourcen in Ihrer Vorlage |
| dependsOn |Nein |Ressourcen, von denen die definierte Ressource abhängig ist. Die Abhängigkeiten zwischen den Ressourcen werden ausgewertet, und die Ressourcen werden in ihrer Abhängigkeitsreihenfolge bereitgestellt. Wenn Ressourcen nicht voneinander abhängig sind, wird versucht, sie parallel bereitzustellen. Der Wert kann eine durch Trennzeichen getrennte Liste von Ressourcennamen oder eindeutigen Ressourcenbezeichnern sein. |
| Eigenschaften |Nein |Ressourcenspezifische Konfigurationseinstellungen Die Werte für die Eigenschaften sind mit den Werten identisch, die Sie im Anforderungstext für den REST-API-Vorgang (PUT-Methode) angegeben haben, um die Ressource zu erstellen. Links zur Ressourcenschemadokumentation oder REST-API finden Sie unter [Anbieter, Regionen, API-Versionen und Schemas für den Ressourcen-Manager](resource-manager-supported-services.md). |
| Ressourcen |Nein |Untergeordnete Ressourcen, die von der definierten Ressource abhängig sind. Sie können nur Ressourcentypen bereitstellen, die laut Schema der übergeordneten Ressource zulässig sind. Der vollqualifizierte Name des untergeordneten Ressourcentyps enthält den übergeordneten Ressourcentyp, z. B. **Microsoft.Web/sites/extensions**. Die Abhängigkeit von der übergeordneten Ressource ist nicht implizit. Sie müssen diese Abhängigkeit explizit definieren. |

Wenn der Ressourcenname nicht eindeutig ist, können Sie mit der **resourceId**-Hilfsfunktion (weiter unten beschrieben) den eindeutigen Bezeichner für eine Ressource abrufen.

Der Abschnitt „Ressourcen“ enthält ein Array mit den bereitzustellenden Ressourcen. Innerhalb jeder Ressource können Sie für diese Ressourcen wiederum ein Array mit untergeordneten Ressourcen definieren. Der Abschnitt „Ressourcen“ kann daher beispielsweise folgende Struktur aufweisen:

    "resources": [
       {
           "name": "resourceA",
           ...
       },
       {
           "name": "resourceB",
           ...
           "resources": [
               {
                   "name": "firstChildResourceB",
                   ...
               },
               {   
                   "name": "secondChildResourceB",
                   ...
               }
           ]
       },
       {
           "name": "resourceC",
           ...
       }
    ]



Das folgende Beispiel enthält eine **Microsoft.Web/serverfarms**-Ressource und eine **Microsoft.Web/Sites**-Ressource mit einer untergeordneten **Extensions**-Ressource. Beachten Sie, dass die Website als abhängig von der Serverfarm markiert ist, da die Serverfarm vorhanden sein muss, bevor die Website bereitgestellt werden kann. Sie sehen auch, dass die **Extensions**-Ressource ein untergeordnetes Element der Website ist.

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
          "team": "ARM"
        },
        "dependsOn": [
          "[concat('Microsoft.Web/serverFarms/', parameters('hostingPlanName'))]"
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


## Ausgaben
Im Ausgabenabschnitt legen Sie Werte fest, die von der Bereitstellung zurückgegeben werden. Sie könnten z. B. den URI für den Zugriff auf eine bereitgestellte Ressource zurückgeben.

Das folgende Beispiel zeigt die Struktur einer Ausgabedefinition:

    "outputs": {
       "<outputName>" : {
         "type" : "<type-of-output-value>",
         "value": "<output-value-expression>"
       }
    }

| Elementname | Erforderlich | Beschreibung |
|:---:|:---:|:--- |
| outputName |Ja |Name des Ausgabewerts. Es muss sich um einen gültigen JavaScript-Bezeichner handeln. |
| Typ |Ja |Der Typ des Ausgabewerts. Ausgabewerte unterstützen dieselben Typen wie Vorlagen-Eingabeparameter. |
| value |Ja |Ein Vorlagensprachausdruck, der ausgewertet und als Ausgabewert zurückgegeben wird. |

Das folgende Beispiel zeigt einen Wert, der im Ausgabeabschnitt zurückgegeben wird.

    "outputs": {
       "siteUri" : {
         "type" : "string",
         "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
       }
    }

Weitere Informationen zum Arbeiten mit Vorlagen finden Sie unter [Freigeben des Status in Azure-Ressourcen-Manager-Vorlagen](best-practices-resource-manager-state.md).

## Nächste Schritte
* Komplette Vorlagen für viele verschiedene Lösungstypen finden Sie unter [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/).
* Ausführliche Informationen zu den Funktionen, die Sie innerhalb einer Vorlage nutzen können, finden Sie unter [Funktionen von Azure Resource Manager-Vorlagen](resource-group-template-functions.md).
* Informationen zum Zusammenführen mehrerer Vorlagen während der Bereitstellung finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure Resource Manager](resource-group-linked-templates.md).
* Informationen dazu, wie Sie beim Erstellen eines Ressourcentyps eine bestimmte Anzahl von Durchläufen ausführen, finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md).
* Möglicherweise müssen Sie Ressourcen verwenden, die in einer anderen Ressourcengruppe enthalten sind. Dies geschieht normalerweise bei der Arbeit mit Speicherkonten oder virtuellen Netzwerken, die in mehreren Ressourcengruppen gemeinsam verwendet werden. Weitere Informationen finden Sie unter der [resourceId-Funktion](resource-group-template-functions.md#resourceid).

[deployment2cmdlet]: https://msdn.microsoft.com/library/mt740620(v=azure.200).aspx

<!---HONumber=AcomDC_0928_2016-->