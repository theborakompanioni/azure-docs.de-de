---
title: "Resource Manager-Vorlage für Speicher | Microsoft Docs"
description: "Zeigt das Ressourcen-Manager-Schema für die Bereitstellung von Speicherkonten über eine Vorlage."
services: azure-resource-manager,storage
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 25d35683-fe99-4a11-8b1a-b80accab58e7
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/05/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 4f541e34e7c0696e4074613c4ab0734a096c6d12
ms.openlocfilehash: a9850299610aae6b4d9fdaf5abdf81ff6b361b8b


---
# <a name="storage-account-template-schema"></a>Vorlagenschema für Speicherkonten
Erstellt ein Speicherkonto.

## <a name="schema-format"></a>Schemaformat
Fügen Sie zum Erstellen eines Speicherkontos das folgende Schema im Ressourcenabschnitt der Vorlage hinzu.

    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2015-06-15",
        "name": string,
        "location": string,
        "properties": 
        {
            "accountType": string
        }
    }

## <a name="values"></a>Werte
In den folgenden Tabellen sind die Werte beschrieben, die Sie im Schema festlegen müssen.

| Name | Wert |
| --- | --- |
| type |Enum<br />Erforderlich<br />**Microsoft.Storage/storageAccounts**<br /><br />Der zu erstellende Ressourcentyp. |
| apiVersion |Enum<br />Erforderlich<br />**2015-06-15** oder **2015-05-01-preview**<br /><br />Die zum Erstellen der Ressource zu verwendende API-Version. |
| Name |String<br />Erforderlich<br />Zwischen 3 und 24 Zeichen, nur Ziffern und Kleinbuchstaben.<br /><br />Der Name des zu erstellenden Speicherkontos. Der Name muss in Azure in allen Fällen eindeutig sein. Verwenden Sie die Funktion [uniqueString](resource-group-template-functions.md#uniquestring) mit Ihrer Benennungskonvention wie im folgenden Beispiel. |
| location |String<br />Erforderlich<br />Eine Region, die Speicherkonten unterstützt. Informationen zum Bestimmen gültiger Regionen finden Sie unter [Unterstützte Regionen](resource-manager-supported-services.md#supported-regions).<br /><br />Die Region, die das Speicherkonto hosten soll. |
| Eigenschaften |Objekt<br />Erforderlich<br />[properties-Objekt](#properties)<br /><br />Ein Objekt, das den Typ des zu erstellenden Speicherkontos angibt. |

<a id="properties" />

### <a name="properties-object"></a>properties-Objekt
| Name | Wert |
| --- | --- |
| accountType |String<br />Erforderlich<br />**Standard_LRS**, **Standard_ZRS**, **Standard_GRS**, **Standard_RAGRS** oder **Premium_LRS**<br /><br />Der Speicherkontotyp. Die zulässigen Werte sind: lokal redundanter Standardspeicher, zonenredundanter Standardspeicher, georedundanter Standardspeicher, georedundanter Standardspeicher mit Lesezugriff und lokal redundanter Premiumspeicher. Informationen zu diesen Kontotypen finden Sie unter [Azure Storage-Replikation](../storage/storage-redundancy.md). |

## <a name="examples"></a>Beispiele
Im folgenden Beispiel wird ein lokal redundantes Standardspeicherkonto mit einem eindeutigen Namen basierend auf der Ressourcen-Gruppen-Id bereitgestellt.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2015-06-15",
                "name": "[concat('storage', uniqueString(resourceGroup().id))]",
                "location": "[resourceGroup().location]",
                "properties": 
            {
                    "accountType": "Standard_LRS"
            }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Schnellstartvorlagen
Es gibt viele Schnellstartvorlagen, die ein Speicherkonto enthalten. In den folgenden Vorlagen werden einige häufige Szenarien veranschaulicht:

* [Erstellen eines Standardspeicherkontos](https://azure.microsoft.com/documentation/templates/101-storage-account-create)
* [Einfache Bereitstellung einer Windows-VM](https://azure.microsoft.com/documentation/templates/101-vm-simple-windows)
* [Einfache Bereitstellung einer Linux-VM](https://azure.microsoft.com/documentation/templates/101-vm-simple-linux)
* [Erstellen eines CDN-Profils, eines CDN-Endpunkts mit einem Speicherkonto als Ursprung](https://azure.microsoft.com/documentation/templates/201-cdn-with-storage-account)
* [Erstellen einer hoch verfügbaren SharePoint-Farm mit neun virtuellen Computern mithilfe der Powershell DSC-Erweiterung](https://azure.microsoft.com/documentation/templates/sharepoint-server-farm-ha)
* [Einfache Bereitstellung eines sicheren Service Fabric-Clusters mit fünf Knoten und aktivierter WAD](https://azure.microsoft.com/documentation/templates/service-fabric-secure-cluster-5-node-1-nodetype-wad)
* [Erstellen eines virtuellen Computers aus einem Windows-Image mit vier leeren Datenträgern](https://azure.microsoft.com/documentation/templates/101-vm-multiple-data-disk)

## <a name="next-steps"></a>Nächste Schritte
* Allgemeine Informationen zu Speichern finden Sie unter [Einführung in Microsoft Azure Storage](../storage/storage-introduction.md).
* Vorlagen, die ein neues Speicherkonto mit einem virtuellen Computer verwenden, finden Sie z.B. unter [Bereitstellen eines einfachen virtuellen Linux-Computers](https://azure.microsoft.com/documentation/templates/101-simple-linux-vm/) oder [Bereitstellen eines einfachen virtuellen Windows-Computers](https://azure.microsoft.com/documentation/templates/101-simple-windows-vm/).




<!--HONumber=Nov16_HO3-->


