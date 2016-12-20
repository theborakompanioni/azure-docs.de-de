---
title: "Automation-Ressourcen in OMS-Lösungen | Microsoft Docs"
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
ms.date: 10/19/2016
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e115fd3b5f29dbcbfd9f85ebb215d950539cc1e3


---
# <a name="automation-resources-in-oms-solutions-preview"></a>Automation-Ressourcen in OMS-Lösungen (Preview)
> [!NOTE]
> Dies ist die vorläufige Dokumentation für das Erstellen von Verwaltungslösungen in der OMS, die sich derzeit in der Vorschau befinden. Jedes unten beschriebene Schema kann sich ändern.   
> 
> 

[Verwaltungslösungen in OMS](operations-management-suite-solutions.md) enthalten üblicherweise Runbooks in Azure Automation zum Automatisieren von Prozessen, wie z.B. das Sammeln und Verarbeiten von Überwachungsdaten.  Zusätzlich zu Runbooks beinhalten Automation-Konten Assets, wie z.B. Variablen und Zeitpläne, die die Runbooks unterstützen, die in der Lösung verwendet werden.  Dieser Artikel beschreibt, wie Sie Runbooks und dazugehörige Ressourcen in eine Lösung integrieren können.

> [!NOTE]
> Die Beispiele in diesem Artikel verwenden Parameter und Variablen, die entweder erforderlich sind oder für Verwaltungslösungen gelten und unter [Creating management solutions in Operations Management Suite (OMS) (Erstellen von Verwaltungslösungen in der Operations Management Suite (OMS))](operations-management-suite-solutions-creating.md) beschrieben sind. 
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie schon mit der [Erstellung einer Verwaltungslösung](operations-management-suite-solutions-creating.md) und der Struktur einer Lösungsdatei vertraut sind.

## <a name="samples"></a>Beispiele
Resource Manager-Beispieltemplates für Automation-Ressourcen erhalten Sie über die [Schnellstartvorlagen auf GitHub](https://github.com/azureautomation/automation-packs/tree/master/101-sample-automation-resource-templates).

## <a name="automation-account"></a>Automation-Konto
Alle Ressourcen in Azure Automation befinden sich einem [Automation-Konto](../automation/automation-security-overview.md#automation-account-overview).  Wie unter [OMS workspace and Automation account (OMS-Arbeitsbereich und Automation-Konto)](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account) beschrieben, ist das Automation-Konto nicht in der Verwaltungslösung enthalten, muss aber vor der Installation der Lösung bestehen.  Ist es nicht verfügbar, schlägt die Installation der Lösung fehl.

Der Name des Automation-Kontos ist im Namen jeder Automation-Ressource enthalten.  Dafür sorgt,wie in diesem Beispiel eines Runbooks zu sehen, der Parameter **accountName** in der Lösung.

    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbooks
[Azure Automation runbook](../automation/automation-runbook-types.md)-Ressourcen sind vom Typ **Microsoft.Automation/automationAccounts/runbooks** und verfügen über die in der folgenden Tabelle aufgelisteten Eigenschaften.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| runbookType |Gibt den Typ des Runbooks an. <br><br> Script – PowerShell-Skript <br>PowerShell – PowerShell-Workflow <br> GraphPowerShell – Grafisches PowerShell-Skript-Runbook <br> GraphPowerShellWorkflow – Grafisches PowerShell-Workflow-Runbook |
| logProgress |Gibt an, ob für das Runbook [Datensätze zum Status](../automation/automation-runbook-output-and-messages.md) erstellt werden sollen |
| logVerbose |Gibt an, ob für das Runbook [ausführliche Datensätze](../automation/automation-runbook-output-and-messages.md) erstellt werden sollen |
| Beschreibung |Optionale Beschreibung für das Runbook |
| publishContentLink |Gibt den Inhalt des Runbooks an <br><br>uri – Gibt den URI für den Inhalt des Runbooks an.  Dies wird eine PS1-Datei für PowerShell und Skript-Runbooks sowie eine exportierte grafische Runbook-Datei für ein grafisches Runbook sein.  <br> version – Version des Runbooks für Ihre eigene Nachverfolgung |

Nachstehend finden Sie ein Beispiel für eine Runbook-Ressource.  In diesem Fall wird das Runbook aus dem [PowerShell-Katalog](https://www.powershellgallery.com) abgerufen.

    "name": "[concat(parameters('accountName'), '/Start-AzureV2VMs'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]"
    ],
    "tags": { },
    "properties": {
        "runbookType": "PowerShell",
        "logProgress": "true",
        "logVerbose": "true",
        "description": "",
        "publishContentLink": {
            "uri": "https://www.powershellgallery.com/api/v2/items/psscript/package/Update-ModulesInAutomationToLatestVersion/1.0/Start-AzureV2VMs.ps1",
            "version": "1.0"
        }
    }

### <a name="starting-a-runbook"></a>Starten eines Runbooks
In einer Verwaltungslösung haben Sie zwei Möglichkeiten, ein Runbook zu starten.

* Um das Runbook zu starten, wenn die Lösung installiert ist, erstellen Sie wie unten beschrieben eine [Auftragsressource](#automation-jobs).
* Um das Runbook nach einem Zeitplan zu starten, erstellen Sie wie unten beschrieben eine [Zeitplanressource](#schedules). 

## <a name="automation-jobs"></a>Automation-Aufträge
Um ein Runbook zu starten, wenn die Verwaltungslösung installiert ist, erstellen Sie eine **Auftragsressource**.  Auftragsressourcen sind vom Typ **Microsoft.Automation/automationAccounts/jobs** und verfügen über die in der folgenden Tabelle aufgelisteten Eigenschaften.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| runbook |Einzelne **name**-Entität mit dem Namen des Runbooks |
| parameters |Vom Runbook benötigte Entität für jeden Parameter |

Der Auftrag umfasst das Senden des Runbook-Namens und aller Parameterwerte an das Runbook.  Der Auftrag muss von dem Runbook [abhängen](operations-management-suite-solutions-creating.md#resources), das es startet, da das Runbook vor dem Auftrag erstellt werden muss.  Außerdem erstellen Sie Abhängigkeiten von anderen Aufträgen für Runbooks, die vor dem aktuellen ausgeführt werden sollten.

Der Name einer Auftragsressource muss eine GUID enthalten, die in der Regel von einem Parameter zugeordnet wird.  Weitere Informationen zu GUID-Parametern finden Sie unter [Creating management solutions in Operations Management Suite (OMS) (Erstellen von Verwaltungslösungen in der Operations Management Suite (OMS))](operations-management-suite-solutions-creating.md#parameters).  

Nachstehend finden Sie ein Beispiel einer Auftragsressource, die ein Runbook startet, wenn die Verwaltungslösung installiert ist.  Ein anderes Runbook muss abgeschlossen werden, bevor dieses gestartet wird, damit für dieses Runbook Abhängigkeiten von den Aufträgen bestehen.  Die Details für den Auftrag werden über Parameter und Variablen bereitgestellt, anstatt direkt angegeben zu werden.

    {
        "name": "[concat(parameters('accountName'), '/', parameters('startVmsRunbookGuid'))]",
        "type": "Microsoft.Automation/automationAccounts/jobs",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "location": "[parameters('regionId')]",
        "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]",
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/jobs/', parameters('initialPrepRunbookGuid'))]"
        ],
        "tags": { },
        "properties": {
            "runbook": {
                "name": "[variables('startVmsRunbookName')]"
            },
            "parameters": {
                "AzureConnectionAssetName": "[variables('AzureConnectionAssetName')]",
                "ResourceGroupName": "[variables('ResourceGroupName')]"
            }
        }
    }


## <a name="certificates"></a>Zertifikate
[Azure Automation-Zertifikate](../automation/automation-certificates.md) sind vom Typ **Microsoft.Automation/automationAccounts/certificates** und verfügen über die in der folgenden Tabelle aufgelisteten Eigenschaften.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| base64Value |Base-64-Wert für das Zertifikat |
| thumbprint |Fingerabdruck für das Zertifikat |

Nachstehend finden Sie ein Beispiel für eine Zertifikatsressource.

    "name": "[concat(parameters('accountName'), '/MyCertificate')]",
    "type": "certificates",
    "apiVersion": "2015-01-01-preview",
    "location": "[parameters('regionId')]",
    "tags": {},
    "dependsOn": [
    ],
    "properties": {
        "base64Value": "MIIC1jCCAA8gAwIAVgIYJY4wXCXH/YAHMtALh7qEFzANAgkqhkiG5w0AAQUFGDAUMRIwEBYDVQQDEwlsA3NhAGevc3QqHhcNMTZwNjI5MHQxMjAsWhcNOjEwNjI5NKWwMDAwWjAURIwEAYDVQQBEwlsA2NhAGhvc3QwggEiMA0GCSqGSIA3DQEAAQUAA4IADwAwggEKAoIAAQDIyzv2A0RUg1/AAryI9W1DGAHAqqGdlFfTkUSDfv+hEZTAwKv0p8daqY6GroT8Du7ctQmrxJsy8JxIpDWxUaWwXtvv1kR9eG9Vs5dw8gqhjtOwgXvkOcFdKdQwA82PkcXoHlo+NlAiiPPgmHSELGvcL1uOgl3v+UFiiD1ro4qYqR0ITNhSlq5v2QJIPnka8FshFyPHhVtjtKfQkc9G/xDePW8dHwAhfi8VYRmVMmJAEOLCAJzRjnsgAfznP8CZ/QUczPF8LuTZ/WA/RaK1/Arj6VAo1VwHFY4AZXAolz7xs2sTuHplVO7FL8X58UvF7nlxq48W1Vu0l8oDi2HjvAgMAAAGjJDAiMAsGA1UdDwREAwIEsDATAgNVHSUEDDAKAggrAgEFNQcDATANAgkqhkiG9w0AAQUFAAOCAQEAk8ak2A5Ug4Iay2v0uXAk95qdAthJQN5qIVA13Qay8p4MG/S5+aXOVz4uMXGt18QjGds1A7Q8KDV4Slnwn95sVgA5EP7akvoGXhgAp8Dm90sac3+aSG4fo1V7Y/FYgAgpEy4C/5mKFD1ATeyyhy3PmF0+ZQRJ7aLDPAXioh98LrzMZr1ijzlAAKfJxzwZhpJamAwjZCYqiNZ54r4C4wA4QgX9sVfQKd5e/gQnUM8gTQIjQ8G2973jqxaVNw9lZnVKW3C8/QyLit20pNoqX2qQedwsqg3WCUcPRUUqZ4NpQeHL/AvKIrt158zAfU903yElAEm2Zr3oOUR4WfYQ==",
        "thumbprint": "F485CBE5569F7A5019CB68D7G6D987AC85124B4C"
    }

## <a name="credentials"></a>Anmeldeinformationen
[Azure Automation-Anmeldeinformationen](../automation/automation-credentials.md) sind vom Typ **Microsoft.Automation/automationAccounts/credentials** und verfügen über die in der folgenden Tabelle aufgelisteten Eigenschaften.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| userName |Benutzername für die Anmeldeinformationen |
| password |Kennwort für die Anmeldeinformationen |

Nachstehend finden Sie ein Beispiel für eine Anmeldeinformationsressource.

    "name": "[concat(parameters('accountName'), '/', variables('credentialName'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks/credentials",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "userName": "User01",
        "password": "password"
    }


## <a name="schedules"></a>Zeitpläne
[Azure Automation-Zeitpläne](../automation/automation-schedules.md) sind vom Typ **Microsoft.Automation/automationAccounts/schedules** und verfügen über die in der folgenden Tabelle aufgelisteten Eigenschaften.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Beschreibung |Optionale Beschreibung für den Zeitplan |
| startTime |Gibt die Startzeit eines Zeitplans als DateTime-Objekt an. Eine Zeichenfolge kann bereitgestellt werden, wenn sie in ein gültiges DateTime-Objekt konvertiert werden kann. |
| isEnabled |Gibt an, ob der Zeitplan aktiviert ist |
| interval |Der Typ des Intervalls für den Zeitplan<br><br>day<br>hour |
| frequency |Häufigkeit, mit der der Zeitplan ausgelöst werden soll, in Anzahl von Tagen oder Stunden |

Nachstehend finden Sie ein Beispiel für eine Zeitplanressource.

    "name": "[concat(parameters('accountName'), '/', variables('variableName'))]",
    "type": "microsoft.automation/automationAccounts/schedules",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Schedule for StartByResourceGroup runbook",
        "startTime": "10/30/2016 12:00:00",
        "isEnabled": "true",
        "interval": "day",
        "frequency": "1"
    }


## <a name="variables"></a>Variablen
[Azure Automation-Variablen](../automation/automation-variables.md) sind vom Typ **Microsoft.Automation/automationAccounts/variables** und verfügen über die in der folgenden Tabelle aufgelisteten Eigenschaften.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Beschreibung |Optionale Beschreibung für die Variable |
| isEncrypted |Gibt an, ob die Variable verschlüsselt werden soll |
| type |Datentyp für die Variable |
| value |Wert für die Variable |

Nachstehend finden Sie ein Beispiel für eine Variablenressource.

    "name": "[concat(parameters('accountName'), '/', variables('StartTargetResourceGroupsAssetName')) ]",
    "type": "microsoft.automation/automationAccounts/variables",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Description for the variable.",
        "isEncrypted": "true",
        "type": "string",
        "value": "'This is a string value.'"
    }


## <a name="modules"></a>Module
Ihre Verwaltungslösung muss keine [globalen Module](../automation/automation-integration-modules.md) definieren, die von Ihren Runbooks verwendet werden, da sie immer verfügbar sein werden.  Sie müssen für jedes andere Modul, das von Ihrem Runbook verwendet wird, eine Ressource einschließen, und das Runbook sollte von der Modulressource abhängen, um sicherzustellen, dass sie vor dem Runbook erstellt wird.

[Integrationsmodule](../automation/automation-integration-modules.md) sind vom Typ **Microsoft.Automation/automationAccounts/modules** und verfügen über die in der folgenden Tabelle aufgelisteten Eigenschaften.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| contentLink |Gibt den Inhalt des Moduls an <br><br>uri – Gibt den URI für den Inhalt des Runbooks an.  Dies wird eine PS1-Datei für PowerShell und Skript-Runbooks sowie eine exportierte grafische Runbook-Datei für ein grafisches Runbook sein.  <br> version – Version des Runbooks für Ihre eigene Nachverfolgung |

Nachstehend finden Sie ein Beispiel für eine Modulressource.

    {        
        "name": "[concat(parameters('accountName'), '/', variables('OMSIngestionModuleName'))]",
        "type": "Microsoft.Automation/automationAccounts/modules",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "properties": {
            "contentLink": {
                "uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
            }
        }
    }

### <a name="updating-modules"></a>Aktualisieren von Modulen
Wenn Sie eine Verwaltungslösung aktualisieren, die ein Runbook einschließt, das einen Zeitplan verwendet, und die neue Version Ihrer Lösung über ein neues Modul verfügt, das von diesem Runbook verwendet wird, verwendet das Runbook möglicherweise die alte Version des Moduls.  Sie sollten die folgenden Runbooks in Ihre Lösung einschließen und einen Auftrag erstellen, um sie vor anderen Runbooks auszuführen.  Dadurch wird sichergestellt, dass alle Module nach Bedarf aktualisiert werden, bevor die Runbooks geladen werden.

* Mithilfe von [Update-ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) wird sichergestellt, dass es sich bei allen von Runbooks verwendeten Modulen in Ihrer Lösung um die aktuelle Version handelt.  
* [ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) registriert alle Zeitplanressourcen erneut, um sicherzustellen, dass die mit ihnen verknüpften Runbooks die aktuellen Module verwenden.

Nachstehend finden Sie ein Beispiel der für die Unterstützung der Modulaktualisierung erforderlichen Elemente einer Lösung.

    "parameters": {
        "ModuleImportGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        },
        "ReRegisterRunbookGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        }
    },

    "variables": {
        "ModuleImportRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ModuleImportRunbookUri": "https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/Content/Update-ModulesInAutomationToLatestVersion.ps1",
        "ModuleImportRunbookDescription": "Imports modules and also updates all Azure dependent modules that are in the same Automation Account.",
        "ReRegisterSchedulesRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ReRegisterSchedulesRunbookUri": "https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/Content/ReRegisterAutomationSchedule-MS-Mgmt.ps1",
        "ReRegisterSchedulesRunbookDescription": "Reregisters schedules to ensure that they use latest modules."
    },

    "resources": [
        {
            "name": "[concat(parameters('accountName'), '/', variables('ModuleImportRunbookName'))]",
            "type": "Microsoft.Automation/automationAccounts/runbooks",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "dependsOn": [
            ],
            "location": "[parameters('regionId')]",
            "tags": { },
            "properties": {
                "runbookType": "PowerShell",
                "logProgress": "true",
                "logVerbose": "true",
                "description": "[variables('ModuleImportRunbookDescription')]",
                "publishContentLink": {
                    "uri": "[variables('ModuleImportRunbookUri')]",
                    "version": "1.0.0.0"
                }
            }
        },
        {
            "name": "[concat(parameters('accountName'), '/', parameters('ModuleImportGuid'))]",
            "type": "Microsoft.Automation/automationAccounts/jobs",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "location": "[parameters('regionId')]",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ModuleImportRunbookName'))]"
            ],
            "tags": { },
            "properties": {
                "runbook": {
                    "name": "[variables('ModuleImportRunbookName')]"
                },
                "parameters": {
                    "ResourceGroupName": "[resourceGroup().name]",
                    "AutomationAccountName": "[parameters('accountName')]",
                    "NewModuleName": "AzureRM.Insights"
                }
            }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('ReRegisterSchedulesRunbookName'))]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "PowerShell",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('ReRegisterSchedulesRunbookDescription')]",
            "publishContentLink": {
              "uri": "[variables('ReRegisterSchedulesRunbookUri')]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', parameters('reRegisterSchedulesGuid'))]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ReRegisterSchedulesRunbookName'))]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('ReRegisterSchedulesRunbookName')]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
        }
    ]


## <a name="next-steps"></a>Nächste Schritte
* [Hinzufügen einer Sicht zu Ihrer Lösung](operations-management-suite-solutions-resources-views.md), um gesammelte Daten zu visualisieren.




<!--HONumber=Nov16_HO3-->


