---
title: "Erstellen von Application Insights-Ressourcen, Warnungen und Verfügbarkeitstests in PowerShell | Microsoft Docs"
description: Automatisieren Sie die Verwaltung von Application Insights-Ressourcen mithilfe einer Azure Resource Manager-Vorlage.
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 9f73b87f-be63-4847-88c8-368543acad8b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: fc2d3c3fee5abbab0eab16c106c88c8753e703cc
ms.openlocfilehash: f6302d7d11691635c286164f11be74ff3a1ad8dd


---
# <a name="create-application-insights-resources-using-powershell"></a>Erstellen von Application Insights-Ressourcen mithilfe von PowerShell
In diesem Artikel wird beschrieben, wie eine [Application Insights](app-insights-overview.md)-Ressource automatisch in Azure erstellt wird. Dies kann z. B. als Teil eines Buildvorgangs erfolgen. Zusammen mit der grundlegenden Application Insights-Ressource können Sie [Verfügbarkeitswebtests](app-insights-monitor-web-app-availability.md) erstellen, [Warnungen einrichten](app-insights-alerts.md) und andere Azure-Ressourcen erstellen.

Im Wesentlichen werden diese Ressourcen mit JSON-Vorlagen für den [Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md) erstellt. Die Vorgehensweise lässt sich wie folgt zusammenfassen: Sie laden die JSON-Definitionen vorhandener Ressourcen herunter, parametrisieren bestimmte Werte, z. B. Namen, und führen dann die Vorlage immer aus, wenn Sie eine neue Ressource erstellen möchten. Sie können mehrere Ressourcen zusammenfassen und in einem Durchgang erstellen, z. B. einen App-Monitor mit Verfügbarkeitstests, Warnungen und Speicher für fortlaufenden Export. Einige Parametrisierungen weisen Besonderheiten auf, die hier erläutert werden.

## <a name="one-time-setup"></a>Einmalige Konfiguration
Wenn Sie PowerShell noch nicht mit Ihrem Azure-Abonnement verwendet haben:

Installieren Sie das Azure PowerShell-Modul auf dem Computer, auf dem die Skripts ausgeführt werden sollen:

1. Installieren Sie [Microsoft-Webplattform-Installer (Version 5 oder höher)](http://www.microsoft.com/web/downloads/platform.aspx).
2. Installieren Sie hiermit Microsoft Azure PowerShell.

## <a name="copy-the-json-for-existing-resources"></a>Kopieren der JSON-Definition für vorhandene Ressourcen
1. Richten Sie [Application Insights](app-insights-overview.md) für ein Projekt ein, das den Projekten ähnelt, die automatisch generiert werden sollen. Fügen Sie bei Bedarf Webtests und Warnungen hinzu.
2. Erstellen Sie eine neue JSON-Datei, in diesem Beispiel die Datei `template1.json` . Kopieren Sie den folgenden Inhalt in die Datei:

    ```JSON

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "appName": { "type": "string" },
            "webTestName": { "type": "string" },
            "url": { "type": "string" },
            "text": { "type" : "string" }
          },
          "variables": {
            "testName": "[concat(parameters('webTestName'), 
               '-', toLower(parameters('appName')))]"
            "alertRuleName": "[concat(parameters('webTestName'), 
               '-', toLower(parameters('appName')), 
               '-', subscription().subscriptionId)]"
          },
          "resources": [
            {
              // component JSON file contents
            },
            {
              //web test JSON file contents
            },
            {
              //alert rule JSON file contents
            }

            // Any other resources go here
          ]
        }

    ```

    Mit dieser Vorlage wird zusätzlich zur Hauptressource ein Verfügbarkeitstest eingerichtet.


1. Öffnen Sie den [Azure-Ressourcen-Manager](https://resources.azure.com/). Navigieren Sie über `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components` nach unten zu Ihrer Anwendungsressource. 
   
    ![Navigation im Azure-Ressourcen-Explorer](./media/app-insights-powershell/01.png)
   
    *Komponenten* sind die grundlegenden Application Insights-Ressourcen zum Anzeigen von Anwendungen. Für die zugeordneten Warnungsregeln und Verfügbarkeitswebtests liegen separate Ressourcen vor.
2. Kopieren Sie die JSON-Definition der Komponente an die entsprechende Stelle in der Datei `template1.json`.
3. Löschen Sie folgende Eigenschaften:
   
   * `id`
   * `InstrumentationKey`
   * `CreationDate`
   * `TenantId`
4. Öffnen Sie die Abschnitte „webtests“ und „alertrules“, und kopieren Sie die JSON-Definition für einzelne Elemente in die Vorlage. (Kopieren Sie die Definition nicht aus den Knoten „webtests“ oder „alertrules“, sondern aus den Elementen unter diesen Knoten.)
   
    Jeder Webtest weist eine zugeordnete Warnungsregel auf, die Sie auch kopieren müssen.
   
    Jeder Webtest verfügt über eine entsprechende Warnungsregel. Der Webtest sollte zuerst aufgeführt sein.
   
    Sie können auch Warnungen für Metriken hinzufügen. [Metriknamen](app-insights-powershell-alerts.md#metric-names).
5. Fügen Sie diese Zeile in jede Ressource ein:
   
    `"apiVersion": "2015-05-01",`

## <a name="parameterize-the-template"></a>Parametrisieren der Vorlage
Nun müssen Sie die Namen durch Parameter ersetzen. Zum [Parametrisieren einer Vorlage](../azure-resource-manager/resource-group-authoring-templates.md) schreiben Sie Ausdrücke mithilfe einer [Reihe von Hilfsfunktionen](../azure-resource-manager/resource-group-template-functions.md). 

Sie können nicht einen Teil einer Zeichenfolge parametrisieren. Verwenden Sie daher `concat()` zum Erstellen von Zeichenfolgen.

Es folgen einige Beispiele der Ersetzungen, die Sie vornehmen können. Es gibt mehrere Vorkommen der einzelnen Ersetzungen. In Ihrer Vorlage müssen Sie unter Umständen andere Ersetzungen vornehmen. In diesen Beispielen werden die Parameter und Variablen verwendet, die oben in der Vorlage definiert wurden.

| Suchen | Ersetzen durch |
| --- | --- |
| `"hidden-link:/subscriptions/.../components/MyAppName"` |`"[concat('hidden-link:',`<br/>` resourceId('microsoft.insights/components',` <br/> ` parameters('appName')))]"` |
| `"/subscriptions/.../alertrules/myAlertName-myAppName-subsId",` |`"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",` |
| `"/subscriptions/.../webtests/myTestName-myAppName",` |`"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",` |
| `"myWebTest-myAppName"` |`"[variables(testName)]"'` |
| `"myTestName-myAppName-subsId"` |`"[variables('alertRuleName')]"` |
| `"myAppName"` |`"[parameters('appName')]"` |
| `"myappname"` (Kleinbuchstaben) |`"[toLower(parameters('appName'))]"` |
| `"<WebTest Name=\"myWebTest\" ...`<br/>` Url=\"http://fabrikam.com/home\" ...>"` |`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]"`<br/>Löschen Sie „Guid“ und „Id“. |

## <a name="set-dependencies-between-the-resources"></a>Festlegen von Abhängigkeiten zwischen den Ressourcen
Die Ressourcen sollten in Azure in strikter Reihenfolge eingerichtet werden. Um sicherzustellen, dass eine Einrichtung abgeschlossen ist, bevor die nächste beginnt, fügen Sie Abhängigkeitszeilen hinzu:

* In der Ressource für Verfügbarkeitstests:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`
* In der Ressource für Warnungen:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`

Beachten Sie, dass ein Verfügbarkeitstest tatsächlich aus zwei Teilen besteht: dem Test selbst und einer Warnungsregel, die abhängig von den Testergebnissen ausgelöst wird.

## <a name="create-application-insights-resources"></a>Erstellen von Application Insights-Ressourcen
1. Melden Sie sich in PowerShell bei Azure an.
   
    `Login-AzureRmAccount`
2. Führen Sie beispielsweise den folgenden Befehl aus:
   
    ```PS
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -templateFile .\template1.json `
               -appName myNewApp `
               -webTestName aWebTest `
               -url http://myapp.com `
               -text "Welcome!"
   
    ``` 
   
   * -ResourceGroupName: die Gruppe, in der Sie die neuen Ressourcen erstellen möchten.
   * -templateFile: muss vor den benutzerdefinierten Parametern angegeben werden.
   * -appName: der Name der zu erstellenden Ressource.
   * -webTestName: der Name des zu erstellenden Webtests.
   * -Url: die URL der Web-App.
   * -text: eine Zeichenfolge, die auf der Webseite angezeigt wird.

## <a name="to-get-the-instrumentation-key"></a>So rufen Sie den Instrumentierungsschlüssel ab
Nach der Erstellung einer Anwendungsressource, benötigen Sie den iKey: 

```PS

    $resource = Get-AzureRmResource -ResourceId "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<YOUR RESOURCE GROUP>/providers/Microsoft.Insights/components/<YOUR APP NAME>"

    $resource.Properties.InstrumentationKey
```


## <a name="an-example"></a>Beispiel
Hier ist die vollständige Vorlage, die ich erstellt habe. Sie weist die Anwendungskomponente, den Verfügbarkeitstest, die Warnung zum Verfügbarkeitstest und eine Warnung für die Antwortzeitmetrik auf.

``` JSON

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "webTestName": { "type": "string" },
    "appName": { "type": "string" },
    "URL": { "type": "string" },
    "text": { "type": "string" }
  },
  "variables": {
    "alertRuleName": "[concat(parameters('webTestName'), '-', toLower(parameters('appName')), '-', subscription().subscriptionId)]",
    "testName": "[concat(parameters('webTestName'), '-', toLower(parameters('appName')))]",
    "responseAlertName": "[concat('ResponseTime-', toLower(parameters('appName')))]"
  },
  "resources": [
    {
      //
      // App resource
      //
      "name": "[parameters('appName')]",
      "type": "Microsoft.Insights/components",
      "apiVersion": "2014-04-01",
      "kind": "web",
      "location": "East US", // Set to preferred location 
      "properties": {
        "Application_Type": "web",
        "Flow_Type": "Brownfield",
        "Request_Source": "VSIX3.3.1.0",
        "Name": "[parameters('appName')]",
        "PackageId": null,
        "ApplicationId": "[parameters('appName')]"
      },
      "tags": { "applicationType": "web" }
    },
    {
      //
      // Availability test
      //
      "name": "[variables('testName')]",
      "type": "Microsoft.Insights/webtests",
      "apiVersion": "2014-04-01",
      "location": "East US", // Set to preferred location
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "Name": "[parameters('webTestName')]",
        "Description": "n",
        "Enabled": true,
        "Frequency": 900, // 15 minutes
        "Timeout": 120, // 2 minutes
        "Kind": "ping", // single URL test
        "RetryEnabled": true,
        "Locations": [
          {
            "Id": "us-va-ash-azr"
          },
          {
            "Id": "emea-nl-ams-azr"
          },
          {
            "Id": "apac-jp-kaw-edge"
          }
        ],
        "Configuration": {
          "WebTest": "[concat('<WebTest   Name=\"', parameters('webTestName'), '\"   Enabled=\"True\"         CssProjectStructure=\"\"    CssIteration=\"\"  Timeout=\"120\"  WorkItemIds=\"\"         xmlns=\"http://microsoft.com/schemas/VisualStudio/TeamTest/2010\"         Description=\"\"  CredentialUserName=\"\"  CredentialPassword=\"\"         PreAuthenticate=\"True\"  Proxy=\"default\"  StopOnError=\"False\"         RecordedResultFile=\"\"  ResultsLocale=\"\">  <Items>  <Request Method=\"GET\"    Version=\"1.1\"  Url=\"', parameters('Url'),   '\" ThinkTime=\"0\"  Timeout=\"300\" ParseDependentRequests=\"True\"         FollowRedirects=\"True\" RecordResult=\"True\" Cache=\"False\"         ResponseTimeGoal=\"0\"  Encoding=\"utf-8\"  ExpectedHttpStatusCode=\"200\"         ExpectedResponseUrl=\"\" ReportingName=\"\" IgnoreHttpStatusCode=\"False\" />        </Items>  <ValidationRules> <ValidationRule  Classname=\"Microsoft.VisualStudio.TestTools.WebTesting.Rules.ValidationRuleFindText, Microsoft.VisualStudio.QualityTools.WebTestFramework, Version=10.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a\" DisplayName=\"Find Text\"         Description=\"Verifies the existence of the specified text in the response.\"         Level=\"High\"  ExectuionOrder=\"BeforeDependents\">  <RuleParameters>        <RuleParameter Name=\"FindText\" Value=\"',   parameters('text'), '\" />  <RuleParameter Name=\"IgnoreCase\" Value=\"False\" />  <RuleParameter Name=\"UseRegularExpression\" Value=\"False\" />  <RuleParameter Name=\"PassIfTextFound\" Value=\"True\" />  </RuleParameters> </ValidationRule>  </ValidationRules>  </WebTest>')]"
        },
        "SyntheticMonitorId": "[variables('testName')]"
      }
    },
    {
      //
      // Alert rule for the availability test
      //
      "name": "[variables('alertRuleName')]",
      "type": "Microsoft.Insights/alertrules",
      "apiVersion": "2014-04-01",
      "location": "East US", // Must be East US at present
      "dependsOn": [
        "[resourceId('Microsoft.Insights/webtests', variables('testName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource",
        "[concat('hidden-link:', resourceId('Microsoft.Insights/webtests', variables('testName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('alertRuleName')]",
        "description": "alert for web test",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.LocationThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.LocationThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/webtests', variables('testName'))]",
            "metricName": "GSMT_AvRaW"
          },
          "windowSize": "PT15M", // Take action if changed state for 15 minutes
          "failedLocationCount": 2
        },
        "actions": [
          {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
            "sendToServiceOwners": true,
            "customEmails": []
          }
        ]
      }

    },
    {
      //
      // Metric alert on response time
      //
      "name": "[variables('responseAlertName')]",
      "type": "Microsoft.Insights/alertrules",
      "apiVersion": "2014-04-01",
      "location": "East US", // Must be East US at present
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]",
        "[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('responseAlertName')]",
        "description": "response time alert",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/components', parameters('appName'))]",
            "metricName": "request.duration"
          },
          "threshold": 3, //seconds
          "windowSize": "PT15M" // Take action if changed state for 15 minutes
        },
        "actions": [
          {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
            "sendToServiceOwners": true,
            "customEmails": []
          }
        ]
      }
    }
  ]
}


```


## <a name="next-steps"></a>Nächste Schritte
Andere Artikel zu Automation:

* [Erstellen einer Application Insights-Ressource](app-insights-powershell-script-create-resource.md) : Schnellverfahren ohne Verwendung einer Vorlage.
* [Einrichten von Warnungen](app-insights-powershell-alerts.md)
* [Erstellen von Webtests](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [Senden von Azure-Diagnosedaten an Application Insights](app-insights-powershell-azure-diagnostics.md)
* [Bereitstellen in Azure aus GitHub](http://blogs.msdn.com/b/webdev/archive/2015/09/16/deploy-to-azure-from-github-with-application-insights.aspx)
* [Erstellen von Versionsanmerkungen](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)




<!--HONumber=Nov16_HO4-->


