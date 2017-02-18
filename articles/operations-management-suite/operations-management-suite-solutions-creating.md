---
title: "Erstellen von Verwaltungslösungen in der Operations Management Suite (OMS) | Microsoft Docs"
description: "Verwaltungslösungen erweitern die Funktionalität der Operations Management Suite (OMS), indem sie Pakete mit Verwaltungsszenarien bereitstellen, die Kunden zu ihrem OMS-Arbeitsbereich hinzufügen können.  Diese Artikel beschreibt, wie Sie Verwaltungslösungen erstellen, die Sie in Ihrer Umgebung verwenden oder Ihren Kunden zur Verfügung stellen möchten."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: fc8b76bf996060e226ac3f508a1ecffca6fc3c98
ms.openlocfilehash: caa2f96d452174ebb13c5cbf67737f20e2a2134d


---
# <a name="creating-management-solutions-in-operations-management-suite-oms-preview"></a>Erstellen von Verwaltungslösungen in der Operations Management Suite (OMS) (Vorschau)
> [!NOTE]
> Dies ist die vorläufige Dokumentation für das Erstellen von Verwaltungslösungen in der OMS, die sich derzeit in der Vorschau befinden. Jedes unten beschriebene Schema kann sich ändern.  
>
>

Verwaltungslösungen erweitern die Funktionalität der Operations Management Suite (OMS), indem sie Pakete mit Verwaltungsszenarien bereitstellen, die Kunden zu ihrem OMS-Arbeitsbereich hinzufügen können.  Dieser Artikel enthält Informationen zum Erstellen eigener Verwaltungslösungen, die Sie in Ihrer eigenen Umgebung verwenden können, oder für Kunden über die Community verfügbar machen können.

## <a name="planning-your-management-solution"></a>Planen Ihrer Verwaltungslösung
Verwaltungslösungen in der OMS umfassen mehrere Ressourcen, die ein bestimmtes Verwaltungsszenario unterstützen.  Konzentrieren Sie sich beim Planen Ihrer Lösung auf das Szenario, das Sie erreichen möchten, sowie auf alle erforderlichen Ressourcen, um es zu unterstützen.  Jede Lösung sollte eigenständig sein und jede erforderliche Ressource definieren, auch wenn eine oder mehrere Ressourcen auch durch andere Lösungen definiert werden.  Wenn eine Verwaltungslösung installiert wird, werden die einzelnen Ressourcen erstellt, sofern sie noch nicht vorhanden sind, und Sie können definieren, was mit den Ressourcen passieren soll, wenn eine Lösung entfernt wird.  

Eine Verwaltungslösung kann beispielsweise ein [Azure Automation-Runbook](../automation/automation-intro.md) enthalten, das basierend auf einem [Zeitplan](../automation/automation-schedules.md) Daten für das Log Analytics-Repository sammelt und eine [Ansicht](../log-analytics/log-analytics-view-designer.md) verwendet, um verschiedene Visualisierungen der gesammelten Daten bereitzustellen.  Der gleiche Zeitplan wird möglicherweise auch von einer anderen Lösung verwendet.  Als Ersteller der Verwaltungslösung definieren Sie alle drei Ressourcen, geben aber an, dass das Runbook und die Ansicht automatisch entfernt werden sollen, wenn die Lösung entfernt wird.    Sie definieren auch den Zeitplan, geben aber an, dass dieser beim Entfernen der Lösung beibehalten werden soll, für den Fall, dass der Plan noch von der anderen Lösung verwendet wird.

## <a name="management-solution-files"></a>Dateien der Verwaltungslösung
Verwaltungslösungen werden als [Resource Manager-Vorlagen](../azure-resource-manager/resource-manager-template-walkthrough.md) implementiert.  Wenn Sie lernen möchten, wie Sie Verwaltungslösungen erstellen, ist der wichtigste Punkt das [Erstellen einer Vorlage](../azure-resource-manager/resource-group-authoring-templates.md).  In diesem Artikel erhalten Sie wichtige Informationen zu Vorlagen, die für Lösungen verwendet werden, und erfahren, wie Sie typische Lösungsressourcen definieren.

Die grundlegende Struktur einer Verwaltungslösung entspricht einer [Resource Manager-Vorlage](../azure-resource-manager/resource-group-authoring-templates.md#template-format), die wie folgt aussieht.  Jeder der folgenden Abschnitte beschreibt die Elemente auf oberster Ebene und die zugehörigen Inhalte in einer Lösung.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parameter
[Parameter](../azure-resource-manager/resource-group-authoring-templates.md#parameters) sind Werte, die Sie von den Benutzern benötigen, wenn diese die Verwaltungslösung installieren.  Es gibt Standardparameter, über die alle Lösungen verfügen werden, und Sie können nach Bedarf zusätzliche Parameter zu Ihrer bestimmten Lösung hinzufügen.  Wie Benutzer die Parameterwerte bei der Installation Ihrer Lösung bereitstellen, hängt vom bestimmten Parameter ab und wie die Lösung installiert wird.

Wenn Benutzer Ihre Verwaltungslösung über den [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-management-solutions) oder über [Azure-Schnellstartvorlagen](operations-management-suite-solutions.md#finding-and-installing-management-solutions) installieren, werden sie aufgefordert, einen [OMS-Arbeitsbereich und ein Automation-Konto](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account) auszuwählen.  Diese werden verwendet, um die Werte jedes Standardparameters aufzufüllen.  Der Benutzer wird nicht dazu aufgefordert, direkt Werte für die Standardparameter bereitzustellen, jedoch wird er aufgefordert, Werte für jeden zusätzlichen Parameter bereitzustellen.

Wenn der Benutzer Ihrer Lösung [mit einer anderen Methode](operations-management-suite-solutions.md#finding-and-installing-management-solutions) installiert, muss er einen Wert für alle Standardparameter und alle zusätzlichen Parameter angeben.

Ein Beispielparameter ist unten dargestellt.

    "Daily Start Time": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

In der folgenden Tabelle werden die Attribute eines Parameters beschrieben.

| Attribut | Beschreibung |
|:--- |:--- |
| type |Der Datentyp für den Parameter. Das für den Benutzer angezeigte Eingabesteuerelement hängt vom Datentyp ab.<br><br>bool – Dropdownfeld<br>string – Textfeld<br>int – Textfeld<br>securestring – Kennwortfeld<br> |
| category |Optionale Kategorie für den Parameter.  Parameter in der gleichen Kategorie werden gruppiert. |
| control |Zusätzliche Funktionen für Abfrageparameter<br><br>datetime – die Datetime-Steuerung wird angezeigt<br>guid – der GUID-Wert wird automatisch generiert, und er Parameter wird nicht angezeigt. |
| Beschreibung |Optionale Beschreibung für den Parameter.  Wird in einer Informationssprechblase neben dem Parameter angezeigt. |

### <a name="standard-parameters"></a>Standardparameter
In der folgenden Tabelle werden die Standardparameter für alle Verwaltungslösungen aufgelistet.  Diese Werte werden für den Benutzer aufgefüllt, es gibt keine Aufforderung, wenn Ihre Lösung vom Azure Marketplace oder von Schnellstartvorlagen installiert wird.  Der Benutzer muss Werte für diese bereitstellen, wenn die Lösung mit einer anderen Methode installiert wurde.

> [!NOTE]
> Die Benutzeroberfläche im Azure Marketplace und für Schnellstartvorlagen erwartet in der Tabelle die Namen der Parameter.  Wenn Sie unterschiedliche Parameternamen verwenden, dann wird der Benutzer für diese aufgefordert, und sie werden nicht automatisch aufgefüllt.
>
>

| Parameter | Typ | Beschreibung |
|:--- |:--- |:--- |
| accountName |string |Azure Automation-Kontoname |
| pricingTier |string |Der Tarif für den Log Analytics-Arbeitsbereich und das Azure Automation-Konto |
| regionId |string |Region des Azure Automation-Kontos |
| solutionName |string |Name der Lösung |
| workspaceName |string |Name des Log Analytics-Arbeitsbereichs. |
| workspaceRegionId |string |Region des Azure Log Analytics-Arbeitsbereichs |

### <a name="sample"></a>Beispiel
Es folgt eine Beispielparameterentität für eine Lösung.  Dies umfasst alle Standardparameter und zwei zusätzliche Parameter in der gleichen Kategorie.

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
               "type": "string",
               "metadata": {
                   "description": "A valid Azure Automation account name"
               }
        },
        "workspaceRegionId": {
               "type": "string",
               "metadata": {
                   "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        },
        "jobIdGuid": {
        "type": "string",
            "metadata": {
                "description": "GUID for a runbook job",
                "control": "guid",
                "category": "Schedule"
            }
        },
        "startTime": {
            "type": "string",
            "metadata": {
                "description": "Time for starting the runbook.",
                "control": "datetime",
                "category": "Schedule"
            }
        }


Sie verweisen in anderen Elementen der Lösung mit der Syntax **parameters('parameter name')** auf die Parameterwerte.  Um z.B. auf den Namen des Arbeitsbereichs zuzugreifen, verwenden Sie **parameters('workspaceName')**.

## <a name="variables"></a>Variablen
Das Element **Variables** enthält Werte, die Sie in der restlichen Verwaltungslösung verwenden.  Diese Werte werden dem Benutzer, der die Lösung installiert, nicht verfügbar gemacht.  Sie stellen dem Ersteller einen zentralen Ort bereit, an dem dieser Werte verwalten kann, die in der Lösung möglicherweise mehrfach verwendet werden. Platzieren Sie beliebige Werte für Ihre Lösung in Variablen anstatt sie im **resources**-Element hartzucodieren.  Dadurch ist der Code besser lesbar und Sie können diese Werte einfach in späteren Versionen ändern.

Im Folgenden finden Sie ein Beispiel eines **variables**-Elements mit typischen Parametern, die in Lösungen verwendet werden.

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Sie verweisen in der Lösung mit der Syntax **variables('variable name')** auf Variablenwerte.  Um beispielsweise auf die SolutionName-Variable zuzugreifen, verwenden Sie **variables('solutionName')**.

## <a name="resources"></a>Ressourcen
Das Element **resources** definiert die verschiedenen Ressourcen, die in Ihrer Verwaltungslösung enthalten sind.  Dies ist der größte und komplexeste Teil der Vorlage.  Ressourcen werden mit der folgenden Struktur definiert.  

    "resources": [
        {
            "name": "<name-of-the-resource>",            
            "apiVersion": "<api-version-of-resource>",
            "type": "<resource-provider-namespace/resource-type-name>",        
            "location": "<location-of-resource>",
            "tags": "<name-value-pairs-for-resource-tagging>",
            "comments": "<your-reference-notes>",
            "dependsOn": [
                "<array-of-related-resource-names>"
            ],
            "properties": "<unique-settings-for-the-resource>",
            "resources": [
                "<array-of-child-resources>"
            ]
        }
    ]

### <a name="dependencies"></a>Abhängigkeiten
Die **dependsOn**-Elemente geben eine [Abhängigkeit](../azure-resource-manager/resource-group-define-dependencies.md) auf einer anderen Ressource an.  Wenn die Lösung installiert ist, wird keine Ressource erstellt, bis nicht alle Abhängigkeiten erstellt wurden.  Beispielsweise kann Ihre Lösung möglicherweise [ein Runbook starten](operations-management-suite-solutions-resources-automation.md#runbooks), wenn sie mithilfe einer [Auftragsressource](operations-management-suite-solutions-resources-automation.md#automation-jobs) erstellt wurde.  Die Auftragsressource würde dann von der Runbookressource abhängig sein, um sicherzustellen, dass das Runbook erstellt wird, bevor der Auftrag erstellt wird.

### <a name="oms-workspace-and-automation-account"></a>OMS-Arbeitsbereich und Automation-Konto
Verwaltungslösungen erfordern einen [OMS-Arbeitsbereich](../log-analytics/log-analytics-manage-access.md) für Ansichten und ein [Automation-Konto](../automation/automation-security-overview.md#automation-account-overview) für Runbooks und zugehörige Ressourcen.  Diese müssen verfügbar sein, bevor die Ressourcen in der Lösung erstellt werden und sie sollten nicht in der Lösung selbst definiert werden.  Der Benutzer wird [einen Arbeitsbereich und ein Konto angeben](operations-management-suite-solutions.md#oms-workspace-and-automation-account), wenn dieser Ihre Lösung bereitstellt. Als Autor sollten Sie aber die nachstehenden Punkte beachten.

## <a name="solution-resource"></a>Lösungsressource
Jede Lösung erfordert einen Ressourceneintrag im Element **resources**, der die Lösung selbst definiert.  Der Eintrag weist den Typ **Microsoft.OperationsManagement/solutions** auf.  Es folgt ein Beispiel für eine Lösungsressource.  Die anderen Elemente werden in den folgenden Abschnitten beschrieben.

    "name": "[concat(variables('SolutionName'), '[ ' ,parameters('workspacename'), ' ]')]",
    "location": "[parameters('workspaceRegionId')]",
    "tags": { },
    "type": "Microsoft.OperationsManagement/solutions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]",
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
    ]
    "properties": {
        "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
        "referencedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]"
        ],
        "containedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
        ]
    },
    "plan": {
        "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
        "Version": "[variables('SolutionVersion')]",
        "product": "AzureSQLAnalyticSolution",
        "publisher": "[variables('SolutionPublisher')]",
        "promotionCode": ""
    }

### <a name="solution-name"></a>Projektmappenname
Der Name der Lösung muss im Azure-Abonnement eindeutig sein. Der empfohlene Wert ist der folgende.  Dadurch wird eine Variable namens **SolutionName** für den Basisnamen und der **workspaceName**-Parameter verwendet, um sicherzustellen, dass der Namen eindeutig ist.

    [concat(variables('SolutionName'), ' [' ,parameters('workspaceName'), ']')]

Dies würde einen Namen wie folgt auflösen.

    My Solution Name [MyWorkspace]


### <a name="dependencies"></a>Abhängigkeiten
Die Lösungsressource muss über eine [Abhängigkeit](../azure-resource-manager/resource-group-define-dependencies.md) auf jeder anderen Ressource in der Lösung verfügen, da sie vorhanden sein müssen, bevor die Lösung erstellt werden kann.  Hierzu fügen Sie im Element **dependsOn** einen Eintrag für jede Ressource hinzu.

### <a name="properties"></a>Eigenschaften
Die Lösungsressource weist die Eigenschaften in der folgenden Tabelle auf.  Dies umfasst die Ressourcen, die in der Lösung enthalten sind oder von dieser referenziert werden. Dadurch wird definiert, wie die Ressource nach Installation der Lösung verwaltet wird.  Jede Ressource in der Lösung muss entweder in der Eigenschaft **referencedResources** oder in der Eigenschaft **containedResources** aufgelistet werden.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| workspaceResourceId |ID des OMS-Arbeitsbereichs im Format *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Workspace Name\>*. |
| referencedResources |Liste der Ressourcen in der Lösung, die nicht entfernt werden sollen, wenn die Lösung entfernt wird. |
| containedResources |Liste der Ressourcen in der Lösung, die entfernt werden sollen, wenn die Lösung entfernt wird. |

Das obige Beispiel ist für eine Lösung mit einem Runbook, einem Zeitplan und einer Anzeige vorgesehen.  Zeitplan und Runbook werden in im Element **properties** (Eigenschaften) referenziert (*referenced*) und daher beim Entfernen der Lösung nicht entfernt.  Die Ansicht ist enthalten (*contained*) und wird daher beim Entfernen der Lösung ebenfalls entfernt.

### <a name="plan"></a>Plan
Die Entität **Plan** der Lösungsressource weist die Eigenschaften in der folgenden Tabelle auf.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Name |Name der Lösung |
| Version |Die Version der Lösung, wie vom Autor festgelegt. |
| product |Eindeutige Zeichenfolge zum Identifizieren der Lösung. |
| Herausgeber |Herausgeber der Lösung |

## <a name="other-resources"></a>Weitere Ressourcen
Details und Beispiele zu Ressourcen, die für alle Verwaltungslösungen in den folgenden Artikeln gelten.

* [Artikel zu Ansichten und Dashboards](operations-management-suite-solutions-resources-views.md)
* [Artikel zu Automation-Ressourcen](operations-management-suite-solutions-resources-automation.md)

## <a name="testing-a-management-solution"></a>Testen einer Verwaltungslösung
Bevor Sie Ihre Verwaltungslösung bereitstellen, empfiehlt es sich, diese mithilfe von [Test-AzureRmResourceGroupDeployment](../azure-resource-manager/resource-group-template-deploy.md#deploy) zu testen.  Dieser Befehl überprüft Ihre Lösungsdatei und hilft Ihnen dabei, mögliche Probleme zu identifizieren, bevor Sie die Lösung bereitstellen.

## <a name="next-steps"></a>Nächste Schritte
* [Hinzufügen gespeicherter Suchvorgänge und Warnungen](operations-management-suite-solutions-resources-searches-alerts.md) zu Ihrer Verwaltungslösung
* [Hinzufügen von Ansichten](operations-management-suite-solutions-resources-views.md) zu Ihrer Verwaltungslösung
* [Hinzufügen von Automation-Runbooks und anderen Ressourcen ](operations-management-suite-solutions-resources-automation.md) zu Ihrer Verwaltungslösung
* Erfahren Sie Näheres zum [Erstellen von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).
* Durchsuchen Sie die [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates), um Beispiele für verschiedene Resource Manager-Vorlagen zu finden.



<!--HONumber=Jan17_HO4-->


