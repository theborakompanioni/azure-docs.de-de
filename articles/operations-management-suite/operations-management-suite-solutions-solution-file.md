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
ms.date: 03/17/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 742405395a3da792d0df01d7393a6e7fc7a55842
ms.lasthandoff: 03/22/2017


---
# <a name="creating-a-management-solution-file-in-operations-management-suite-oms-preview"></a>Erstellen einer Verwaltungslösungsdatei in der Operations Management Suite (OMS) (Vorschau)
> [!NOTE]
> Dies ist die vorläufige Dokumentation für das Erstellen von Verwaltungslösungen in der OMS, die sich derzeit in der Vorschau befinden. Jedes unten beschriebene Schema kann sich ändern.  

Verwaltungslösungen in Operations Management Suite (OMS) werden als [Resource Manager-Vorlagen](../azure-resource-manager/resource-manager-template-walkthrough.md) implementiert.  Wenn Sie lernen möchten, wie Sie Verwaltungslösungen erstellen, ist der wichtigste Punkt das [Erstellen einer Vorlage](../azure-resource-manager/resource-group-authoring-templates.md).  In diesem Artikel erhalten Sie wichtige Informationen zu Vorlagen, die für Lösungen verwendet werden, und erfahren, wie Sie typische Lösungsressourcen konfigurieren.


## <a name="tools"></a>Tools

Können Sie einen beliebigen Texteditor zum Arbeiten mit Lösungsdateien verwenden, aber es wird empfohlen, die in Visual Studio oder Visual Studio Code bereitgestellten Funktionen zu nutzen, wie in den folgenden Artikeln beschrieben.

- [Erstellen und Bereitstellen von Azure-Ressourcengruppen mit Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)
- [Verwenden von Azure Resource Manager-Vorlagen in Visual Studio Code](../azure-resource-manager/resource-manager-vs-code.md)




## <a name="structure"></a>Strukturdefinition
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

Wenn Benutzer Ihre Verwaltungslösung über den [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-management-solutions) oder über [Azure-Schnellstartvorlagen](operations-management-suite-solutions.md#finding-and-installing-management-solutions) installieren, werden sie aufgefordert, einen [OMS-Arbeitsbereich und ein Automation-Konto](operations-management-suite-solutions.md#oms-workspace-and-automation-account) auszuwählen.  Diese werden verwendet, um die Werte jedes Standardparameters aufzufüllen.  Der Benutzer wird nicht dazu aufgefordert, direkt Werte für die Standardparameter bereitzustellen, jedoch wird er aufgefordert, Werte für jeden zusätzlichen Parameter bereitzustellen.

Wenn der Benutzer Ihrer Lösung [mit einer anderen Methode](operations-management-suite-solutions.md#finding-and-installing-management-solutions) installiert, muss er einen Wert für alle Standardparameter und alle zusätzlichen Parameter angeben.

Ein Beispielparameter ist unten dargestellt.  

    "startTime": {
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
| solutionName |string |Name der Lösung  Wenn Sie die Lösung über Schnellstartvorlagen bereitstellen, sollten Sie „solutionName“ als Parameter definieren, damit Sie eine Zeichenfolge definieren können und dies nicht durch den Benutzer erfolgen muss. |
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
[Variablen](../azure-resource-manager/resource-group-authoring-templates.md#variables) sind Werte, die Sie in der restlichen Verwaltungslösung verwenden.  Diese Werte werden dem Benutzer, der die Lösung installiert, nicht verfügbar gemacht.  Sie stellen dem Ersteller einen zentralen Ort bereit, an dem dieser Werte verwalten kann, die in der Lösung möglicherweise mehrfach verwendet werden. Platzieren Sie beliebige Werte für Ihre Lösung in Variablen anstatt sie im **resources**-Element hartzucodieren.  Dadurch ist der Code besser lesbar und Sie können diese Werte einfach in späteren Versionen ändern.

Im Folgenden finden Sie ein Beispiel eines **variables**-Elements mit typischen Parametern, die in Lösungen verwendet werden.

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Sie verweisen in der Lösung mit der Syntax **variables('variable name')** auf Variablenwerte.  Um beispielsweise auf die SolutionName-Variable zuzugreifen, verwenden Sie **variables('SolutionName')**.

Sie können auch komplexe Variablen mit mehreren Gruppen von Werten definieren.  Diese sind in Verwaltungslösungen besonders nützlich, wenn Sie mehrere Eigenschaften für verschiedene Arten von Ressourcen definieren.  Sie könnten beispielsweise die oben aufgeführten Lösungsvariablen die folgendermaßen umstrukturieren.

    "variables": {
        "Solution": {
          "Version": "1.1",
          "Publisher": "Contoso",
          "Name": "My Solution"
        },
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Sie verweisen in diesem Fall in der Lösung mit der Syntax **variables('variable name').property** auf Variablenwerte.  Um beispielsweise auf die SolutionName-Variable zuzugreifen, verwenden Sie **variables('Solution').Name**.

## <a name="resources"></a>Ressourcen
[Ressourcen](../azure-resource-manager/resource-group-authoring-templates.md#resources) definieren die verschiedenen Ressourcen, die von Ihrer Verwaltungslösung installiert und konfiguriert werden.  Dies ist der größte und komplexeste Teil der Vorlage.  Sie erhalten die Struktur und die vollständige Beschreibung der Ressourcenelemente unter [Erstellen von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md#resources).  Verschiedene Ressourcen, die Sie in der Regel definieren, werden in anderen Artikeln in dieser Dokumentation beschrieben. 


### <a name="dependencies"></a>Abhängigkeiten
Die **dependsOn**-Elemente geben eine [Abhängigkeit](../azure-resource-manager/resource-group-define-dependencies.md) auf einer anderen Ressource an.  Wenn die Lösung installiert ist, wird keine Ressource erstellt, bis nicht alle Abhängigkeiten erstellt wurden.  Beispielsweise kann Ihre Lösung möglicherweise [ein Runbook starten](operations-management-suite-solutions-resources-automation.md#runbooks), wenn sie mithilfe einer [Auftragsressource](operations-management-suite-solutions-resources-automation.md#automation-jobs) erstellt wurde.  Die Auftragsressource würde dann von der Runbookressource abhängig sein, um sicherzustellen, dass das Runbook erstellt wird, bevor der Auftrag erstellt wird.

### <a name="oms-workspace-and-automation-account"></a>OMS-Arbeitsbereich und Automation-Konto
Verwaltungslösungen erfordern einen [OMS-Arbeitsbereich](../log-analytics/log-analytics-manage-access.md) für Ansichten und ein [Automation-Konto](../automation/automation-security-overview.md#automation-account-overview) für Runbooks und zugehörige Ressourcen.  Diese müssen verfügbar sein, bevor die Ressourcen in der Lösung erstellt werden und sie sollten nicht in der Lösung selbst definiert werden.  Der Benutzer wird [einen Arbeitsbereich und ein Konto angeben](operations-management-suite-solutions.md#oms-workspace-and-automation-account), wenn dieser Ihre Lösung bereitstellt. Als Autor sollten Sie aber die nachstehenden Punkte beachten.

## <a name="solution-resource"></a>Lösungsressource
Jede Lösung erfordert einen Ressourceneintrag im Element **resources**, der die Lösung selbst definiert.  Der Eintrag weist den Typ **Microsoft.OperationsManagement/solutions** und die folgende Struktur auf. Dies schließt [Standardparameter](#parameters) und [Variablen](#variables) ein, die in der Regel verwendet werden, um Eigenschaften der Lösung zu definieren.


    {
      "name": "[concat(variables('Solution').Name, '[' ,parameters('workspacename'), ']')]",
      "location": "[parameters('workspaceRegionId')]",
      "tags": { },
      "type": "Microsoft.OperationsManagement/solutions",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
        <list-of-resources>
      ],
      "properties": {
        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
        "referencedResources": [
            <list-of-referenced-resources>
        ],
        "containedResources": [
            <list-of-contained-resources>
        ]
      },
      "plan": {
        "name": "[concat(variables('Solution').Name, '[' ,parameters('workspaceName'), ']')]",
        "Version": "[variables('Solution').Version]",
        "product": "[variables('ProductName')]",
        "publisher": "[variables('Solution').Publisher]",
        "promotionCode": ""
      }
    }




### <a name="dependencies"></a>Abhängigkeiten
Die Lösungsressource muss über eine [Abhängigkeit](../azure-resource-manager/resource-group-define-dependencies.md) auf jeder anderen Ressource in der Lösung verfügen, da sie vorhanden sein müssen, bevor die Lösung erstellt werden kann.  Hierzu fügen Sie im Element **dependsOn** einen Eintrag für jede Ressource hinzu.

### <a name="properties"></a>Eigenschaften
Die Lösungsressource weist die Eigenschaften in der folgenden Tabelle auf.  Dies umfasst die Ressourcen, die in der Lösung enthalten sind oder von dieser referenziert werden. Dadurch wird definiert, wie die Ressource nach Installation der Lösung verwaltet wird.  Jede Ressource in der Lösung muss entweder in der Eigenschaft **referencedResources** oder in der Eigenschaft **containedResources** aufgelistet werden.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| workspaceResourceId |ID des Log Analytics-Arbeitsbereichs im Format *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Workspace Name\>*. |
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



## <a name="sample"></a>Beispiel
Beispiele für Lösungsdateien mit einer Lösungsressource finden Sie an folgenden Speicherorten.

- [Artikel zu Automation-Ressourcen](operations-management-suite-solutions-resources-automation.md#sample)
- [Ressourcen für Suchvorgänge und Warnungen](operations-management-suite-solutions-resources-searches-alerts.md#sample)


## <a name="next-steps"></a>Nächste Schritte
* [Hinzufügen gespeicherter Suchvorgänge und Warnungen](operations-management-suite-solutions-resources-searches-alerts.md) zu Ihrer Verwaltungslösung
* [Hinzufügen von Ansichten](operations-management-suite-solutions-resources-views.md) zu Ihrer Verwaltungslösung
* [Hinzufügen von Runbooks und anderen Automation-Ressourcen](operations-management-suite-solutions-resources-automation.md) zu Ihrer Verwaltungslösung
* Erfahren Sie Näheres zum [Erstellen von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).
* Durchsuchen Sie die [Azure-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates), um Beispiele für verschiedene Resource Manager-Vorlagen zu finden.

