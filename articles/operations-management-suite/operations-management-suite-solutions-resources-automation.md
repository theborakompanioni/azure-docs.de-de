---
title: "Azure Automation-Ressourcen in OMS-Lösungen | Microsoft Docs"
description: "Lösungen in der OMS enthalten üblicherweise Runbooks in Azure Automation zum Automatisieren von Prozessen, wie z.B. das Sammeln und Verarbeiten von Überwachungsdaten.  Dieser Artikel beschreibt, wie Sie Runbooks und dazugehörige Ressourcen in eine Lösung integrieren können."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 5281462e-f480-4e5e-9c19-022f36dce76d
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
ms.openlocfilehash: a86a20e1e83f412a06f54bb195180b9d2af98ca6
ms.lasthandoff: 03/22/2017


---
# <a name="adding-azure-automation-resources-to-an-oms-management-solution-preview"></a>Hinzufügen von Azure Automation-Ressourcen zu einer OMS-Verwaltungslösung (Vorschau)
> [!NOTE]
> Dies ist die vorläufige Dokumentation für das Erstellen von Verwaltungslösungen in der OMS, die sich derzeit in der Vorschau befinden. Jedes unten beschriebene Schema kann sich ändern.   
> 
> 

[Verwaltungslösungen in OMS](operations-management-suite-solutions.md) enthalten üblicherweise Runbooks in Azure Automation zum Automatisieren von Prozessen, wie z.B. das Sammeln und Verarbeiten von Überwachungsdaten.  Zusätzlich zu Runbooks beinhalten Automation-Konten Assets, wie z.B. Variablen und Zeitpläne, die die Runbooks unterstützen, die in der Lösung verwendet werden.  Dieser Artikel beschreibt, wie Sie Runbooks und dazugehörige Ressourcen in eine Lösung integrieren können.

> [!NOTE]
> Die Beispiele in diesem Artikel verwenden Parameter und Variablen, die entweder erforderlich sind oder für Verwaltungslösungen gelten und unter [Creating management solutions in Operations Management Suite (OMS) (Erstellen von Verwaltungslösungen in der Operations Management Suite (OMS))](operations-management-suite-solutions-creating.md) beschrieben sind. 


## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie bereits mit den folgenden Informationen vertraut sind.

- [Erstellen einer Verwaltungslösung](operations-management-suite-solutions-creating.md)
- Struktur einer [Lösungsdatei](operations-management-suite-solutions-solution-file.md)
- [Erstellen von Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md)

## <a name="automation-account"></a>Automation-Konto
Alle Ressourcen in Azure Automation befinden sich einem [Automation-Konto](../automation/automation-security-overview.md#automation-account-overview).  Wie unter [OMS workspace and Automation account (OMS-Arbeitsbereich und Automation-Konto)](operations-management-suite-solutions.md#oms-workspace-and-automation-account) beschrieben, ist das Automation-Konto nicht in der Verwaltungslösung enthalten, muss aber vor der Installation der Lösung bestehen.  Ist es nicht verfügbar, schlägt die Installation der Lösung fehl.

Im Namen jeder Automation-Ressource ist der Name des Automation-Kontos enthalten.  Dafür sorgt,wie in diesem Beispiel eines Runbooks zu sehen, der Parameter **accountName** in der Lösung.

    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbooks
Sie sollten alle von der Lösung verwendeten Runbooks zur Lösungsdatei hinzufügen, sodass sie bei der Installation der Lösung erstellt werden.  Den Textkörper des Runbooks können Sie jedoch nicht zur Vorlage hinzufügen, daher sollten Sie das Runbook an einem öffentlichen Speicherort veröffentlichen, auf den alle Benutzer, die Ihre Lösung installieren, zugreifen können.

[Azure Automation Runbookressourcen](../automation/automation-runbook-types.md) sind vom Typ **Microsoft.Automation/automationAccounts/runbooks** und weisen die folgende Struktur auf. Dies schließt allgemeine Variablen und Parameter ein, sodass Sie diesen Codeausschnitt kopieren, in Ihre Lösungsdatei einfügen und die Parameternamen ändern können. 

    {
        "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
        "type": "Microsoft.Automation/automationAccounts/runbooks",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "location": "[parameters('regionId')]",
        "tags": { },
        "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
                "uri": "[variables('Runbook').Uri]",
                "version": [variables('Runbook').Version]"
            }
        }
    }


Die Eigenschaften für Runbooks werden in der folgenden Tabelle beschrieben.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| runbookType |Gibt den Typ des Runbooks an. <br><br> Script – PowerShell-Skript <br>PowerShell – PowerShell-Workflow <br> GraphPowerShell – Grafisches PowerShell-Skript-Runbook <br> GraphPowerShellWorkflow – Grafisches PowerShell-Workflow-Runbook |
| logProgress |Gibt an, ob für das Runbook [Datensätze zum Status](../automation/automation-runbook-output-and-messages.md) erstellt werden sollen |
| logVerbose |Gibt an, ob für das Runbook [ausführliche Datensätze](../automation/automation-runbook-output-and-messages.md) erstellt werden sollen |
| Beschreibung |Optionale Beschreibung für das Runbook |
| publishContentLink |Gibt den Inhalt des Runbooks an <br><br>uri – Gibt den URI für den Inhalt des Runbooks an.  Dies wird eine PS1-Datei für PowerShell und Skript-Runbooks sowie eine exportierte grafische Runbook-Datei für ein grafisches Runbook sein.  <br> version – Version des Runbooks für Ihre eigene Nachverfolgung |


## <a name="automation-jobs"></a>Automation-Aufträge
Wenn Sie ein Runbook in Azure Automation starten, wird ein Automatisierungsauftrag erstellt.  Sie können eine Automatisierungsauftragressource zu Ihrer Lösung hinzufügen, um ein Runbook bei der Installation der Verwaltungslösung automatisch zu starten.  Diese Methode wird in der Regel verwendet, um Runbooks zu starten, die zur Erstkonfiguration der Lösung dienen.  Zum Starten eines Runbooks in regelmäßigen Abständen erstellen Sie einen [Zeitplan](#schedules) und einen [Auftragszeitplan](#job-schedules)

Auftragsressourcen sind vom Typ **Microsoft.Automation/automationAccounts/jobs** und weisen die folgende Struktur auf.  Dies schließt allgemeine Variablen und Parameter ein, sodass Sie diesen Codeausschnitt kopieren, in Ihre Lösungsdatei einfügen und die Parameternamen ändern können. 

    {
      "name": "[concat(parameters('accountName'), '/', parameters('Runbook').JobGuid)]",
      "type": "Microsoft.Automation/automationAccounts/jobs",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
      ],
      "tags": { },
      "properties": {
        "runbook": {
          "name": "[variables('Runbook').Name]"
        },
        "parameters": {
          "Parameter1": "[[variables('Runbook').Parameter1]",
          "Parameter2": "[[variables('Runbook').Parameter2]"
        }
      }
    }

Die Eigenschaften für Automatisierungsaufträge werden in der folgenden Tabelle beschrieben.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| runbook |Einzelne name-Entität mit dem Namen des zu startenden Runbooks |
| parameters |Vom Runbook benötigte Entität für jeden Parameterwert. |

Der Auftrag umfasst das Senden des Runbook-Namens und aller Parameterwerte an das Runbook.  Der Auftrag sollte von dem Runbook [abhängen](operations-management-suite-solutions-solution-file.md#resources), von dem er gestartet wird, da das Runbook vor dem Auftrag erstellt werden muss.  Wenn Sie über mehrere Runbooks verfügen, die gestartet werden sollen, können Sie die Reihenfolge festlegen, indem ein Auftrag von allen anderen Aufträgen abhängt, die zuerst ausgeführt werden sollen.

Der Name einer Auftragsressource muss eine GUID enthalten, die in der Regel von einem Parameter zugeordnet wird.  Weitere Informationen zu GUID-Parametern finden Sie unter [Creating management solutions in Operations Management Suite (OMS) (Erstellen von Verwaltungslösungen in der Operations Management Suite (OMS))](operations-management-suite-solutions-solution-file.md#parameters).  


## <a name="certificates"></a>Zertifikate
[Azure Automation-Zertifikate](../automation/automation-certificates.md) sind vom Typ **Microsoft.Automation/automationAccounts/certificates** und weisen die folgende Struktur auf. Dies schließt allgemeine Variablen und Parameter ein, sodass Sie diesen Codeausschnitt kopieren, in Ihre Lösungsdatei einfügen und die Parameternamen ändern können. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
      "type": "Microsoft.Automation/automationAccounts/certificates",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "base64Value": "[variables('Certificate').Base64Value]",
        "thumbprint": "[variables('Certificate').Thumbprint]"
      }
    }



Die Eigenschaften für Zertifikatressourcen werden in der folgenden Tabelle beschrieben.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| base64Value |Base-64-Wert für das Zertifikat |
| thumbprint |Fingerabdruck für das Zertifikat |



## <a name="credentials"></a>Anmeldeinformationen
[Azure Automation-Anmeldeinformationen](../automation/automation-credentials.md) sind vom Typ **Microsoft.Automation/automationAccounts/credentials** und weisen die folgende Struktur auf.  Dies schließt allgemeine Variablen und Parameter ein, sodass Sie diesen Codeausschnitt kopieren, in Ihre Lösungsdatei einfügen und die Parameternamen ändern können. 


    {
      "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
      "type": "Microsoft.Automation/automationAccounts/credentials",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "userName": "[parameters('credentialUsername')]",
        "password": "[parameters('credentialPassword')]"
      }
    }

Die Eigenschaften für Anmeldeinformationsressourcen werden in der folgenden Tabelle beschrieben.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| userName |Benutzername für die Anmeldeinformationen |
| password |Kennwort für die Anmeldeinformationen |


## <a name="schedules"></a>Zeitpläne
[Azure Automation-Zeitpläne](../automation/automation-schedules.md) sind vom Typ **Microsoft.Automation/automationAccounts/schedules** und weisen die folgende Struktur auf. Dies schließt allgemeine Variablen und Parameter ein, sodass Sie diesen Codeausschnitt kopieren, in Ihre Lösungsdatei einfügen und die Parameternamen ändern können. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
      "type": "microsoft.automation/automationAccounts/schedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Schedule').Description]",
        "startTime": "[parameters('scheduleStartTime')]",
        "timeZone": "[parameters('scheduleTimeZone')]",
        "isEnabled": "[variables('Schedule').IsEnabled]",
        "interval": "[variables('Schedule').Interval]",
        "frequency": "[variables('Schedule').Frequency]"
      }
    }

Die Eigenschaften für Zeitplanressourcen werden in der folgenden Tabelle beschrieben.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Beschreibung |Optionale Beschreibung für den Zeitplan |
| startTime |Gibt die Startzeit eines Zeitplans als DateTime-Objekt an. Eine Zeichenfolge kann bereitgestellt werden, wenn sie in ein gültiges DateTime-Objekt konvertiert werden kann. |
| isEnabled |Gibt an, ob der Zeitplan aktiviert ist |
| interval |Der Typ des Intervalls für den Zeitplan<br><br>day<br>hour |
| frequency |Häufigkeit, mit der der Zeitplan ausgelöst werden soll, in Anzahl von Tagen oder Stunden |

Zeitpläne benötigen eine Startzeit mit einem Wert größer als die aktuelle Zeit.  Sie können diesen Wert nicht mit einer Variablen angeben, da Sie nicht ermitteln könnten, wann die Installation erfolgt.

Wenden Sie eine der folgenden zwei Strategien an, wenn Sie Zeitplanressourcen in einer Lösung verwenden.

- Verwenden Sie einen Parameter für die Startzeit des Zeitplans.  Dadurch wird der Benutzer beim Installieren der Lösung aufgefordert, einen Wert anzugeben.  Wenn Sie über mehrere Zeitpläne verfügen, können Sie für mehrere Zeitpläne einen einzigen Parameterwert verwenden.
- Erstellen Sie die Zeitpläne unter Verwendung eines Runbooks, das beim Installieren der Lösung gestartet wird.  Dadurch braucht der Benutzer keinen Zeitpunkt mehr anzugeben, allerdings können Sie den Zeitplan auch nicht mehr in der Lösung einschließen. Er wird daher mit der Lösung entfernt.


### <a name="job-schedules"></a>Auftragszeitpläne
Auftragszeitplan-Ressourcen verknüpfen ein Runbook mit einem Zeitplan.  Sie sind vom Typ **Microsoft.Automation/automationAccounts/jobSchedules** und weisen die folgende Struktur auf.  Dies schließt allgemeine Variablen und Parameter ein, sodass Sie diesen Codeausschnitt kopieren, in Ihre Lösungsdatei einfügen und die Parameternamen ändern können. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
      "type": "microsoft.automation/automationAccounts/jobSchedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
        "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
      ],
      "tags": {
      },
      "properties": {
        "schedule": {
          "name": "[variables('Schedule').Name]"
        },
        "runbook": {
          "name": "[variables('Runbook').Name]"
        }
      }
    }


Die Eigenschaften für Auftragszeitpläne werden in der folgenden Tabelle beschrieben.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Zeitplanname |Einzelne **name**-Entität mit dem Namen des Zeitplans |
| Runbookname  |Einzelne **name**-Entität mit dem Namen des Runbooks  |



## <a name="variables"></a>Variablen
[Azure Automation-Variablen](../automation/automation-variables.md) sind vom Typ **Microsoft.Automation/automationAccounts/variables** und weisen die folgende Struktur auf.  Dies schließt allgemeine Variablen und Parameter ein, sodass Sie diesen Codeausschnitt kopieren, in Ihre Lösungsdatei einfügen und die Parameternamen ändern können.

    {
      "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
      "type": "microsoft.automation/automationAccounts/variables",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Variable').Description]",
        "isEncrypted": "[variables('Variable').Encrypted]",
        "type": "[variables('Variable').Type]",
        "value": "[variables('Variable').Value]"
      }
    }

Die Eigenschaften für Variablenressourcen werden in der folgenden Tabelle beschrieben.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Beschreibung | Optionale Beschreibung für die Variable |
| isEncrypted | Gibt an, ob die Variable verschlüsselt werden soll |
| type | Datentyp für die Variable |
| value | Wert für die Variable |

## <a name="modules"></a>Module
Ihre Verwaltungslösung muss keine [globalen Module](../automation/automation-integration-modules.md) definieren, die von Ihren Runbooks verwendet werden, da diese in Ihrem Automation-Konto immer verfügbar sind.  Für jedes andere von Ihren Runbooks verwendete Modul müssen Sie jedoch eine Ressource hinzufügen.

[Integrationsmodule](../automation/automation-integration-modules.md) sind vom Typ **Microsoft.Automation/automationAccounts/modules** und weisen die folgende Struktur auf.  Dies schließt allgemeine Variablen und Parameter ein, sodass Sie diesen Codeausschnitt kopieren, in Ihre Lösungsdatei einfügen und die Parameternamen ändern können.

    {
      "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
      "type": "Microsoft.Automation/automationAccounts/modules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "dependsOn": [
      ],
      "properties": {
        "contentLink": {
          "uri": "[variables('Module').Uri]"
        }
      }
    }


Die Eigenschaften für Modulressourcen werden in der folgenden Tabelle beschrieben.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| contentLink |Gibt den Inhalt des Moduls an <br><br>uri – Gibt den URI für den Inhalt des Moduls an.  Dies wird eine PS1-Datei für PowerShell und Skript-Runbooks sowie eine exportierte grafische Runbook-Datei für ein grafisches Runbook sein.  <br> version – Version des Moduls für Ihre eigene Nachverfolgung. |

Das Runbook sollte von der Modulressourcen abhängen, um sicherzustellen, dass diese vor dem Runbook erstellt wird.

### <a name="updating-modules"></a>Aktualisieren von Modulen
Wenn Sie eine Verwaltungslösung aktualisieren, die ein Runbook einschließt, das einen Zeitplan verwendet, und die neue Version Ihrer Lösung über ein neues Modul verfügt, das von diesem Runbook verwendet wird, verwendet das Runbook möglicherweise die alte Version des Moduls.  Sie sollten die folgenden Runbooks in Ihre Lösung einschließen und einen Auftrag erstellen, um sie vor anderen Runbooks auszuführen.  Dadurch wird sichergestellt, dass alle Module nach Bedarf aktualisiert werden, bevor die Runbooks geladen werden.

* Mithilfe von [Update-ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) wird sichergestellt, dass es sich bei allen von Runbooks verwendeten Modulen in Ihrer Lösung um die aktuelle Version handelt.  
* [ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) registriert alle Zeitplanressourcen erneut, um sicherzustellen, dass die mit ihnen verknüpften Runbooks die aktuellen Module verwenden.




## <a name="sample"></a>Beispiel
Es folgt ein Beispiel für eine Lösung, die die folgenden Ressourcen enthält:

- Runbook.  Dies ist ein Beispiel für ein in einem öffentlichen GitHub-Repository gespeichertes Runbook.
- Automation-Auftrag, der das Runbook startet, wenn die Lösung installiert wird.
- Zeitplan und Auftragszeitplan zum Starten des Runbooks in regelmäßigen Abständen.
- Zertifikat
- Anmeldeinformation
- Variable
- Modul  Dies ist das [OMSIngestionAPI-Modul](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) zum Schreiben von Daten in Log Analytics. 

Das Beispiel verwendet Variablen für [Standardlösungsparameter](operations-management-suite-solutions-solution-file.md#parameters), die im Gegensatz zu hartcodierten Werten in Ressourcendefinitionen häufig in einer Lösung verwendet werden.


    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "workspaceName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Log Analytics workspace."
          }
        },
        "accountName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Automation account."
          }
        },
        "workspaceregionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Log Analytics workspace."
          }
        },
        "regionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Automation account."
          }
        },
        "pricingTier": {
          "type": "string",
          "metadata": {
            "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account."
          }
        },
        "certificateBase64Value": {
          "type": "string",
          "metadata": {
            "Description": "Base 64 value for certificate."
          }
        },
        "certificateThumbprint": {
          "type": "securestring",
          "metadata": {
            "Description": "Thumbprint for certificate."
          }
        },
        "credentialUsername": {
          "type": "string",
          "metadata": {
            "Description": "Username for credential."
          }
        },
        "credentialPassword": {
          "type": "securestring",
          "metadata": {
            "Description": "Password for credential."
          }
        },
        "scheduleStartTime": {
          "type": "string",
          "metadata": {
            "Description": "Start time for shedule."
          }
        },
        "scheduleTimeZone": {
          "type": "string",
          "metadata": {
            "Description": "Time zone for schedule."
          }
        },
        "scheduleLinkGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for the schedule link to runbook.",
            "control": "guid"
          }
        },
        "runbookJobGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for the runbook job.",
            "control": "guid"
          }
        }
      },
      "variables": {
        "SolutionName": "MySolution",
        "SolutionVersion": "1.0",
        "SolutionPublisher": "Contoso",
        "ProductName": "SampleSolution",
    
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31",
    
        "Runbook": {
          "Name": "MyRunbook",
          "Description": "Sample runbook",
          "Type": "PowerShell",
          "Uri": "https://raw.githubusercontent.com/user/myrepo/master/samples/MyRunbook.ps1",
          "JobGuid": "[parameters('runbookJobGuid')]"
        },
    
        "Certificate": {
          "Name": "MyCertificate",
          "Base64Value": "[parameters('certificateBase64Value')]",
          "Thumbprint": "[parameters('certificateThumbprint')]"
        },
    
        "Credential": {
          "Name": "MyCredential",
          "UserName": "[parameters('credentialUsername')]",
          "Password": "[parameters('credentialPassword')]"
        },
    
        "Schedule": {
          "Name": "MySchedule",
          "Description": "Sample schedule",
          "IsEnabled": "true",
          "Interval": "1",
          "Frequency": "hour",
          "StartTime": "[parameters('scheduleStartTime')]",
          "TimeZone": "[parameters('scheduleTimeZone')]",
          "LinkGuid": "[parameters('scheduleLinkGuid')]"
        },
    
        "Variable": {
          "Name": "MyVariable",
          "Description": "Sample variable",
          "Encrypted": 0,
          "Type": "string",
          "Value": "'This is my string value.'"
        },
    
        "Module": {
          "Name": "OMSIngestionAPI",
          "Uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
        }
      },
      "resources": [
        {
          "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
          "location": "[parameters('workspaceRegionId')]",
          "tags": { },
          "type": "Microsoft.OperationsManagement/solutions",
          "apiVersion": "[variables('LogAnalyticsApiVersion')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
            "referencedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
            ],
            "containedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]"
            ]
          },
          "plan": {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
            "Version": "[variables('SolutionVersion')]",
            "product": "[variables('ProductName')]",
            "publisher": "[variables('SolutionPublisher')]",
            "promotionCode": ""
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
              "uri": "[variables('Runbook').Uri]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').JobGuid)]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('Runbook').Name]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
          "type": "Microsoft.Automation/automationAccounts/certificates",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "Base64Value": "[variables('Certificate').Base64Value]",
            "Thumbprint": "[variables('Certificate').Thumbprint]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
          "type": "Microsoft.Automation/automationAccounts/credentials",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "userName": "[variables('Credential').UserName]",
            "password": "[variables('Credential').Password]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
          "type": "microsoft.automation/automationAccounts/schedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Schedule').Description]",
            "startTime": "[variables('Schedule').StartTime]",
            "timeZone": "[variables('Schedule').TimeZone]",
            "isEnabled": "[variables('Schedule').IsEnabled]",
            "interval": "[variables('Schedule').Interval]",
            "frequency": "[variables('Schedule').Frequency]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
          "type": "microsoft.automation/automationAccounts/jobSchedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
          ],
          "tags": {
          },
          "properties": {
            "schedule": {
              "name": "[variables('Schedule').Name]"
            },
            "runbook": {
              "name": "[variables('Runbook').Name]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
          "type": "microsoft.automation/automationAccounts/variables",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Variable').Description]",
            "isEncrypted": "[variables('Variable').Encrypted]",
            "type": "[variables('Variable').Type]",
            "value": "[variables('Variable').Value]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
          "type": "Microsoft.Automation/automationAccounts/modules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "properties": {
            "contentLink": {
              "uri": "[variables('Module').Uri]"
            }
          }
        }
    
      ],
      "outputs": { }
    }




## <a name="next-steps"></a>Nächste Schritte
* [Hinzufügen einer Sicht zu Ihrer Lösung](operations-management-suite-solutions-resources-views.md), um gesammelte Daten zu visualisieren.

