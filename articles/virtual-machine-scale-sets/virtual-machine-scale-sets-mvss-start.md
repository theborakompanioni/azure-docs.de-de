---
title: "Informationen zu Vorlagen für VM-Skalierungsgruppen | Microsoft-Dokumentation"
description: "Informationen zum Erstellen einer Vorlage für eine kleinstmögliche Skalierungsgruppe für VM-Skalierungsgruppen"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/14/2017
ms.author: negat
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 3b978f5448c2cfbba4d02e3efd730dea7c7813c3
ms.lasthandoff: 03/31/2017

---

# <a name="learn-about-virtual-machine-scale-set-templates"></a>Informationen zu Vorlagen für VM-Skalierungsgruppen
[Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) sind eine hervorragende Möglichkeit, Gruppen aufeinander bezogener Ressourcen bereitzustellen. In dieser Reihe von Tutorials wird gezeigt, wie Sie eine Vorlage für eine kleinstmögliche Skalierungsgruppe erstellen und für verschiedene Szenarios anpassen. Alle Beispiele stammen aus diesem [GitHub-Repository](https://github.com/gatneil/mvss). 

Hier wird eine einfache Vorlage verwendet. Umfassendere Beispiele für Skalierungsgruppenvorlagen finden Sie, indem Sie im [GitHub-Repository mit Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates) nach Ordnern suchen, die die Zeichenfolge `vmss` enthalten.

Wenn Sie bereits mit dem Erstellen von Vorlagen vertraut sind, können Sie gleich mit dem Abschnitt „Nächste Schritte“ fortfahren, um zu erfahren, wie diese Vorlage geändert wird.

## <a name="review-the-template"></a>Überprüfen der Vorlage

Verwenden Sie GitHub, um unsere Vorlage für eine kleinstmögliche Skalierungsgruppe [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json) zu überprüfen.

In diesem Tutorial untersuchen wir die Diff (`git diff master minimum-viable-scale-set`), um die Vorlage für eine kleinstmögliche Skalierungsgruppe Stück für Stück zu erstellen.

## <a name="define-schema-and-contentversion"></a>Definieren von $schema und contentVersion
Zuerst definieren wir `$schema` und `contentVersion` in der Vorlage. Das Element `$schema` definiert die Version der Vorlagensprache und wird für die Visual Studio-Syntaxhervorhebung und ähnliche Validierungsfunktionen verwendet. Das Element `contentVersion` wird nicht von Azure verwendet. Stattdessen können Sie damit die Vorlagenversion verfolgen.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
```
## <a name="define-parameters"></a>Definieren von Parametern
Als Nächstes definieren wir die beiden Parameter `adminUsername` und `adminPassword`. Parameter sind Werte, die Sie zum Zeitpunkt der Bereitstellung angeben. Der Parameter `adminUsername` hat den Typ `string`, aber da `adminPassword` geheim ist, erhält er den Typ `securestring`. Später werden diese Parameter in die Skalierungsgruppenkonfiguration übergeben.

```json
  "parameters": {
    "adminUsername": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    }
  },
```
## <a name="define-variables"></a>Definieren von Variablen
Mit Resource Manager-Vorlagen können Sie außerdem Variablen für die spätere Verwendung in der Vorlage definieren. In diesem Beispiel werden keine Variablen verwendet, deshalb ist das JSON-Objekt leer.

```json
  "variables": {},
```

## <a name="define-resources"></a>Definieren von Ressourcen
Als Nächstes wird der Ressourcenabschnitt der Vorlage bearbeitet. Hier definieren Sie, was bereitgestellt werden soll. Im Gegensatz zu `parameters` und `variables` (die JSON-Objekte sind), ist `resources` eine JSON-Liste von JSON-Objekten.

```json
   "resources": [
```

Alle Ressourcen erfordern die Eigenschaften `type`, `name`, `apiVersion` und `location`. Die erste Ressource in diesem Beispiel weist den Typ `Microsft.Network/virtualNetwork`, den Namen `myVnet` und apiVersion `2016-03-30` auf. (Die neueste API-Version für einen Ressourcentyp finden Sie in der [Azure-REST-API-Dokumentation](https://docs.microsoft.com/rest/api/).)

```json
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "apiVersion": "2016-12-01",
```

## <a name="specify-location"></a>Angeben des Speicherorts
Zum Angeben des Speicherorts für das virtuelle Netzwerk verwenden wir eine [Resource Manager-Vorlagenfunktion](../azure-resource-manager/resource-group-template-functions.md). Diese Funktion muss wie folgt in Anführungszeichen und eckige Klammern eingeschlossen werden: `"[<template-function>]"`. In diesem Fall verwenden wir die Funktion `resourceGroup`. Sie nimmt keine Argumente an und gibt ein JSON-Objekt mit Metadaten über die Ressourcengruppe zurück, in der diese Bereitstellung bereitgestellt wird. Die Ressourcengruppe wird zum Zeitpunkt der Bereitstellung vom Benutzer festgelegt. Anschließend wenden wir Index mit `.location` auf dieses JSON-Objekt an, um den Speicherort aus dem JSON-Objekt abzurufen.

```json
       "location": "[resourceGroup().location]",
```

## <a name="specify-virtual-network-properties"></a>Angeben von Eigenschaften des virtuellen Netzwerks
Jede Resource Manager-Ressource hat einen eigenen `properties`-Abschnitt für Konfigurationen, die für die Ressource spezifisch sind. In diesem Fall geben wir an, dass das virtuelle Netzwerk ein Subnetz haben soll, das den privaten IP-Adressbereich `10.0.0.0/16` verwendet. Eine Skalierungsgruppe ist immer in einem Subnetz enthalten. Sie kann sich nicht auf Subnetze erstrecken.

```json
       "properties": {
         "addressSpace": {
           "addressPrefixes": [
             "10.0.0.0/16"
           ]
         },
         "subnets": [
           {
             "name": "mySubnet",
             "properties": {
               "addressPrefix": "10.0.0.0/16"
             }
           }
         ]
       }
     },
```

## <a name="add-dependson-list"></a>Hinzufügen der dependsOn-Liste
Zusätzlich zu den erforderlichen Eigenschaften `type`, `name`, `apiVersion` und `location` kann jede Ressource über eine optionale `dependsOn`-Liste von Zeichenfolgen verfügen. Diese Liste gibt an, welche anderen Ressourcen aus dieser Bereitstellung beendet werden müssen, bevor diese Ressource bereitgestellt wird.

In diesem Fall gibt es nur ein Element in der Liste: das virtuelle Netzwerk aus dem vorherigen Beispiel. Wir geben diese Abhängigkeit an, da das Netzwerk vorhanden sein muss, bevor die Skalierungsgruppe VMs erstellen kann. Auf diese Weise kann die Skalierungsgruppe an diese VMs private IP-Adressen aus dem IP-Adressbereich vergeben, der zuvor in den Netzwerkeigenschaften angegeben wurde. Das Format der Zeichenfolgen in der dependsOn-Liste ist `<type>/<name>`. Verwenden Sie die gleichen Werte für `type` und `name` wie zuvor in der Definition der Ressourcen des virtuellen Netzwerks.

```json
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "apiVersion": "2016-04-30-preview",
       "location": "[resourceGroup().location]",
       "dependsOn": [
         "Microsoft.Network/virtualNetworks/myVnet"
       ],
```
## <a name="specify-scale-set-properties"></a>Angeben von Eigenschaften der Skalierungsgruppe
Skalierungsgruppen verfügen über viele Eigenschaften zum Anpassen der virtuellen Computer in der Skalierungsgruppe. Eine vollständige Liste dieser Eigenschaften finden Sie in der [REST-API-Dokumentation für Skalierungsgruppen](https://docs.microsoft.com/en-us/rest/api/virtualmachinescalesets/create-or-update-a-set). In diesem Tutorial werden nur einige häufig verwendete Eigenschaften festgelegt.
### <a name="supply-vm-size-and-capacity"></a>Angeben der Größe und der Kapazität der virtuellen Computer
Für die Skalierungsgruppe muss festgelegt werden, welche Größe der virtuelle Computer haben soll („SKU-Name“) und wie viele solcher virtueller Computer erstellt werden sollen („SKU-Kapazität“). Welche Größen für virtuelle Computer verfügbar sind, erfahren Sie unter [Größen für virtuelle Windows-Computer](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).

```json
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
       },
```

### <a name="choose-type-of-updates"></a>Auswählen des Updatetyps
Für die Skalierungsgruppe muss auch festgelegt werden, wie mit Updates für die Skalierungsgruppe umgegangen werden soll. Derzeit gibt es die beiden Optionen `Manual` und `Automatic`. Weitere Informationen zu den Unterschieden zwischen den beiden Optionen finden Sie in der Dokumentation zum [Upgraden einer VM-Skalierungsgruppe](./virtual-machine-scale-sets-upgrade-scale-set.md).

```json
       "properties": {
         "upgradePolicy": {
           "mode": "Manual"
         },
```

### <a name="choose-vm-operating-system"></a>Auswählen des Betriebssystems der virtuellen Computer
Für die Skalierungsgruppe muss festgelegt werden, welches Betriebssystem die virtuellen Computer haben sollen. Hier erstellen wir die virtuellen Computer mit einem vollständig gepatchten Ubuntu 16.04-LTS-Image.

```json
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
               "publisher": "Canonical",
               "offer": "UbuntuServer",
               "sku": "16.04-LTS",
               "version": "latest"
             }
           },
```

### <a name="specify-computernameprefix"></a>Angeben von computerNamePrefix
Die Skalierungsgruppe stellt mehrere virtuelle Computer bereit. Statt der Namen der einzelnen virtuellen Computer geben wir `computerNamePrefix` an. Die Skalierungsgruppe fügt für jeden virtuellen Computer einen Index an dieses Präfix an, sodass die Namen der virtuellen Computer das Format `<computerNamePrefix>_<auto-generated-index>` haben.

Im folgenden Codeausschnitt verwenden wir die bereits erwähnten Parameter, um den Administratorbenutzernamen und das Kennwort für alle virtuellen Computer in der Skalierungsgruppe festzulegen. Dies erfolgt mit der Vorlagenfunktion `parameters`. Diese Funktion akzeptiert eine Zeichenfolge, die angibt, auf welchen Parameter verwiesen werden soll, und gibt den Wert für diesen Parameter aus.

```json
           "osProfile": {
             "computerNamePrefix": "vm",
             "adminUsername": "[parameters('adminUsername')]",
             "adminPassword": "[parameters('adminPassword')]"
           },
```

### <a name="specify-vm-network-configuration"></a>Angeben der Netzwerkkonfiguration für die virtuellen Computer
Abschließend müssen wir die Netzwerkkonfiguration für die virtuellen Computer in der Skalierungsgruppe angeben. In diesem Fall müssen wir nur die ID des zuvor erstellten Subnetzes angeben. Dadurch fügt die Skalierungsgruppe die Netzwerkschnittstellen in dieses Subnetz ein.

Sie können die ID des virtuellen Netzwerks, das das Subnetz enthält, mithilfe der Vorlagenfunktion `resourceId` abrufen. Diese Funktion nimmt den Typ und den Namen einer Ressource an und gibt den vollqualifizierten Bezeichner dieser Ressource zurück. Diese ID hat das Format: `/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`

Allerdings ist der Bezeichner des virtuellen Netzwerks nicht ausreichend. Sie müssen das genaue Subnetz angeben, in dem sich die virtuellen Computer der Skalierungsgruppe befinden sollen. Verketten Sie dazu `/subnets/mySubnet` mit der ID des virtuellen Netzwerks. Das Ergebnis ist die vollqualifizierte ID des Subnetzes. Führen Sie diese Verkettung mit der Funktion `concat` durch, die eine Reihe von Zeichenfolgen annimmt und deren Verkettung zurückgibt.

```json
           "networkProfile": {
             "networkInterfaceConfigurations": [
               {
                 "name": "myNic",
                 "properties": {
                   "primary": "true",
                   "ipConfigurations": [
                     {
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
                           "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
                         }
                       }
                     }
                   ]
                 }
               }
             ]
           }
         }
       }
     }
   ]
}

```

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]

