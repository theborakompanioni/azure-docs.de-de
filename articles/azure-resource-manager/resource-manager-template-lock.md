---
title: "Resource Manager-Vorlage für Ressourcensperren | Microsoft Docs"
description: "Zeigt das Ressourcen-Manager-Schema für die Bereitstellung von Ressourcensperren über eine Vorlage."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: de6c0c57-e33a-4960-98e0-900901592003
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/03/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: e841c21a15c47108cbea356172bffe766003a145
ms.openlocfilehash: 16f87b8859a5d98c2328388ef12b13ef0e261861


---
# <a name="resource-locks-template-schema"></a>Vorlagenschema für Ressourcensperren
Erstellt eine Sperre für eine Ressource und ihre untergeordneten Ressourcen.

## <a name="schema-format"></a>Schemaformat
Fügen Sie zum Erstellen einer Sperre das folgende Schema im Ressourcenabschnitt der Vorlage hinzu.

    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "level": enum,
            "notes": string
        }
    }



## <a name="values"></a>Werte
In den folgenden Tabellen sind die Werte beschrieben, die Sie im Schema festlegen müssen.

| Name | Erforderlich | Beschreibung |
| --- | --- | --- |
| Typ |Ja |Der zu erstellende Ressourcentyp.<br /><br />Für Ressourcen:<br />**{namespace}/{type}/providers/locks**<br /><br/>Für Ressourcengruppen:<br />**Microsoft.Authorization/locks** |
| apiVersion |Ja |Die zum Erstellen der Ressource zu verwendende API-Version.<br /><br />Verwendung:<br />**2015-01-01**<br /><br /> |
| Name |Ja |Ein Wert, der sowohl die zu sperrende Ressource als auch einen Namen für die Sperre angibt. Kann bis zu 64 Zeichen lang sein, und <, >, %, &, ? oder Steuerzeichen dürfen nicht enthalten sein.<br /><br />Für Ressourcen:<br />**{resource}/Microsoft.Authorization/{lockname}**<br /><br />Für Ressourcengruppen:<br />**{lockname}** |
| dependsOn |Nein |Eine durch Komma getrennte Liste von Ressourcennamen oder eindeutigen Ressourcenbezeichnern.<br /><br />Die Sammlung von Ressourcen, von denen diese Sperre abhängt. Wenn die Ressource, die Sie sperren, in derselben Vorlage bereitgestellt wird, fügen Sie den Ressourcennamen in diesem Element hinzu, um sicherzustellen, dass die Ressource zuerst bereitgestellt wird. |
| Eigenschaften |Ja |Ein Objekt, das den Sperrentyp und Informationen zu der Sperre identifiziert.<br /><br />Siehe [properties-Objekt](#properties-object). |

### <a name="properties-object"></a>properties-Objekt
| Name | Erforderlich | Beschreibung |
| --- | --- | --- |
| Einstellung |Ja |Der Sperrentyp, der auf den Bereich angewendet wird.<br /><br />**CannotDelete**: Benutzer können die Ressource ändern, aber nicht löschen.<br />**ReadOnly**: Benutzer können aus einer Ressource lesen, sie aber nicht löschen oder Aktionen in ihr ausführen. |
| notes |Nein |Beschreibung der Sperre. Kann bis zu 512 Zeichen enthalten. |

## <a name="how-to-use-the-lock-resource"></a>Verwenden der gesperrten Ressource
Sie fügen diese Ressource Ihrer Vorlage hinzu, um zu verhindern, dass bestimmte Aktionen für eine Ressource ausgeführt werden. Die Sperre gilt für alle Benutzer und Gruppen.

Zum Erstellen oder Löschen von Verwaltungssperren müssen Sie Zugriff auf Aktionen vom Typ **Microsoft.Authorization/*** oder **Microsoft.Authorization/locks/*** haben. Von den integrierten Rollen verfügen nur **Besitzer** und **Benutzerzugriffsadministrator** über Berechtigungen für diese Aktionen. Informationen zur rollenbasierten Zugriffssteuerung finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-control-configure.md).

Die Sperre wird auf die angegebene Ressource und alle untergeordneten Ressourcen angewendet.

Sie können eine Sperre mit dem PowerShell-Befehl **Remove-AzureRmResourceLock** oder mit dem [Löschvorgang](https://msdn.microsoft.com/library/azure/mt204562.aspx) der REST-API entfernen.

## <a name="examples"></a>Beispiele
Im folgenden Beispiel wird ein Löschschutz für eine Web-App angewendet.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "hostingPlanName": {
                  "type": "string"
            }
        },
        "variables": {
            "siteName": "[concat('site',uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                },
            },
            {
                "type": "Microsoft.Web/sites/providers/locks",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
             }
        ],
        "outputs": {}
    }

Im nächsten Beispiel wird ein Löschschutz auf die Ressourcengruppe angewendet.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Authorization/locks",
                "apiVersion": "2015-01-01",
                "name": "MyGroupLock",
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
            }
        ],
        "outputs": {}
    }

## <a name="next-steps"></a>Nächste Schritte
* Informationen zur Vorlagenstruktur finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](resource-group-authoring-templates.md).
* Weitere Informationen zu Sperren finden Sie unter [Sperren von Ressourcen mit dem Azure-Ressourcen-Manager](resource-group-lock-resources.md).




<!--HONumber=Nov16_HO3-->


