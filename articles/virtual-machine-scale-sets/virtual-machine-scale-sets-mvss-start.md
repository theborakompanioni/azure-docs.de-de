---
title: "Azure-VM-Skalierungsgruppen: Kleinstmögliche Skalierungsgruppe | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine Vorlage für eine kleinstmögliche Skalierungsgruppe erstellen"
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
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: 65a400b63688c50fd97c34a5aa5dadc2f5dd286c
ms.lasthandoff: 03/06/2017


---

# <a name="about-this-tutorial"></a>Informationen zu diesem Lernprogramm

[Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) sind eine hervorragende Möglichkeit, Gruppen aufeinander bezogener Ressourcen bereitzustellen. In dieser Reihe von Tutorials wird gezeigt, wie Sie eine Vorlage für eine kleinstmögliche Skalierungsgruppe erstellen und für verschiedene Szenarios anpassen. Alle Beispiele stammen aus diesem [github repo (GitHub-Repository)](https://github.com/gatneil/mvss). Jede in dieser exemplarischen Vorgehensweise gezeigte Diff ist das Ergebnis eines `git diff` zwischen Verzweigungen in diesem Repository. Diese Vorlagen und Diffs sollen als einfache, unvollständige Beispiele dienen. Umfassendere Beispiele für Skalierungsgruppenvorlagen finden Sie, indem Sie im [Azure Quickstart Templates github repo (Schnellstartvorlagen-GitHub-Repository in Azure)](https://github.com/Azure/azure-quickstart-templates) nach Ordnern suchen, die die Zeichenfolge `vmss` enthalten.

## <a name="a-minimum-viable-scale-set"></a>Eine kleinstmögliche Skalierungsgruppe

Unsere Vorlage für eine kleinstmögliche Skalierungsgruppe finden Sie [hier](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json). Wenn Sie bereits mit Vorlagen vertraut sind, können Sie gleich zum Abschnitt „Nächste Schritte“ gehen, um zu erfahren, wie Sie diese Vorlage für andere Szenarios abändern. Wenn Sie jedoch mit Vorlagen noch nicht so sehr vertraut sind, kann diese Schritt-für-Schritt-Beschreibung hilfreich sein. Zunächst sehen wir uns die Diff zum Erstellen dieser Vorlage (`git diff master minimum-viable-scale-set`) Stück für Stück an:

Zuerst definieren wir `$schema` und `contentVersion` der Vorlage. `$schema` definiert die Version der Vorlagensprache und wird für die Visual Studio-Syntaxhervorhebung und ähnliche Validierungsfunktionen verwendet. `contentVersion` wird von Azure überhaupt nicht verwendet. Stattdessen können Sie damit feststellen, um welche Version der Vorlage es sich handelt.

```diff
+{
+  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
+  "contentVersion": "1.0.0.0",
```

Als Nächstes definieren wir die beiden Parameter `adminUsername` und `adminPassword`. Parameter sind Werte, die vom Benutzer zum Zeitpunkt der Bereitstellung angegeben werden. `adminUsername` ist vom Typ `string`, aber da `adminPassword` geheim ist, erhält er den Typ `securestring`. Wir werden später sehen, dass diese Parameter in die Skalierungsgruppenkonfiguration übergeben werden.

```diff
+  "parameters": {
+    "adminUsername": {
+      "type": "string"
+    },
+    "adminPassword": {
+      "type": "securestring"
+    }
+  },
```

Resource Manager-Vorlagen ermöglichen außerdem das Definieren von Variablen für die spätere Verwendung in der Vorlage. In diesem Beispiel verwenden wir keine Variablen, deshalb ist das JSON-Objekt leer.

```diff
+  "variables": {},
```

Als Nächstes gibt es die Ressourcen der Vorlage, mit denen wir definieren, was tatsächlich bereitgestellt werden soll. Im Gegensatz zu `parameters` und `variables` (die JSON-Objekte sind), ist `resources` eine JSON-Liste aus JSON-Objekten.

```diff
+  "resources": [
```

Alle Ressourcen erfordern `type`, `name`, `apiVersion` und `location`. Unsere erste Ressource ist vom Typ `Microsft.Network/virtualNetwork` mit dem Namen `myVnet` und der apiVersion `2016-03-30`. Die neueste API-Version für einen Ressourcentyp finden Sie über die [Azure REST API documentation (Azure-REST-API-Dokumentation)](https://docs.microsoft.com/rest/api/).

```diff
+    {
+      "type": "Microsoft.Network/virtualNetworks",
+      "name": "myVnet",
+      "apiVersion": "2016-12-01",
```

Zum Angeben des Speicherorts für das virtuelle Netzwerk verwenden wir eine [Resource Manager-Vorlagenfunktion](../azure-resource-manager/resource-group-template-functions.md), die wie folgt in Anführungszeichen und eckigen Klammern stehen muss: `"[<template-function>]"`. In diesem Fall verwenden wir die resourceGroup-Funktion, die keine Argumente annimmt und ein JSON-Objekt mit Metadaten über die Ressourcengruppe zurückgibt, an die diese Bereitstellung bereitgestellt wird. Die Ressourcengruppe wird zum Zeitpunkt der Bereitstellung vom Benutzer festgelegt. Anschließend wenden wir Index mit `.location` auf dieses JSON-Objekt an, um den Speicherort aus dem JSON-Objekt abzurufen.

```diff
+      "location": "[resourceGroup().location]",
```


Jede Resource Manager-Ressource hat einen eigenen `properties`-Abschnitt für Konfigurationen, die für die Ressource spezifisch sind. In diesem Fall geben wir an, dass das virtuelle Netzwerk ein Subnetz haben soll, das den privaten IP-Adressbereich `10.0.0.0/16` verwendet. Eine Skalierungsgruppe ist immer in einem Subnetz enthalten. Sie kann sich nicht auf Subnetze erstrecken.

```diff
+      "properties": {
+        "addressSpace": {
+          "addressPrefixes": [
+            "10.0.0.0/16"
+          ]
+        },
+        "subnets": [
+          {
+            "name": "mySubnet",
+            "properties": {
+              "addressPrefix": "10.0.0.0/16"
+            }
+          }
+        ]
+      }
+    },
```

Zusätzlich zu den erforderlichen Eigenschaften `type`, `name`, `apiVersion` und `location` kann jede Ressource optional über eine `dependsOn`-Liste von Zeichenfolgen verfügen, die angibt, welche anderen Ressourcen aus dieser Bereitstellung abgeschlossen sein müssen, bevor diese Ressource bereitgestellt wird. In diesem Fall gibt es nur ein Element in dieser Liste, das oben erwähnte virtuelle Netzwerk. Wir geben diese Abhängigkeit an, da das Netzwerk vorhanden sein muss, bevor die Skalierungsgruppe VMs erstellen kann. Auf diese Weise kann die Skalierungsgruppe an diese VMs private IP-Adressen aus dem IP-Adressbereich vergeben, der zuvor in den Netzwerkeigenschaften angegeben wurde. Das Format einer Zeichenfolge in der dependsOn-Liste ist `<type>/<name>` (dieselben `type` und `name`, die wir zuvor in der Ressourcendefinition des virtuellen Netzwerks verwendet haben).

```diff
+    {
+      "type": "Microsoft.Compute/virtualMachineScaleSets",
+      "name": "myScaleSet",
+      "apiVersion": "2016-04-30-preview",
+      "location": "[resourceGroup().location]",
+      "dependsOn": [
+        "Microsoft.Network/virtualNetworks/myVnet"
+      ],
```

Für die Skalierungsgruppe muss festgelegt werden, welche Größe die VM haben soll („SKU-Name“) und wie viele solcher VMs erstellt werden sollen („SKU-Kapazität“). Welche VM-Größen verfügbar sind, erfahren Sie unter [Größen für virtuelle Windows-Computer](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).

```diff
+      "sku": {
+        "name": "Standard_A1",
+        "capacity": 2
+      },
```

Für die Skalierungsgruppe muss auch festgelegt werden, wie mit Updates für die Skalierungsgruppe umgegangen werden soll. Derzeit gibt es die beiden Optionen `Manual` und `Automatic`. Weitere Informationen zu den Unterschieden zwischen den beiden Optionen finden Sie in der Dokumentation zum [Upgraden einer VM-Skalierungsgruppe](./virtual-machine-scale-sets-upgrade-scale-set.md).

```diff
+      "properties": {
+        "upgradePolicy": {
+          "mode": "Manual"
+        },
```

Für die Skalierungsgruppe muss auch festgelegt werden, welches Betriebssystem die VMs haben sollen. Hier erstellen wir die VMs mit einem vollständig gepatchten Ubuntu 16.04-LTS-Image.

```diff
+        "virtualMachineProfile": {
+          "storageProfile": {
+            "imageReference": {
+              "publisher": "Canonical",
+              "offer": "UbuntuServer",
+              "sku": "16.04-LTS",
+              "version": "latest"
+            }
+          },
```

Da die Skalierungsgruppe mehrere VMs bereitstellt, geben wir nicht für jede VM einen Namen an, sondern bestimmen ein `computerNamePrefix`. Die Skalierungsgruppe fügt für jede VM einen Index an dieses Präfix an, sodass die VM-Namen das Format `<computerNamePrefix>_<auto-generated-index>` haben. In diesem Codeausschnitt sehen wir auch, dass wir die Parameter von vorhin verwenden, um den Administratorbenutzernamen und das Kennwort für alle VMs in der Skalierungsgruppe festzulegen. Dies erfolgt mit der Vorlagenfunktion `parameters`, die eine Zeichenfolge annimmt, die den Parameter angibt, auf den wir verweisen möchten, und den Wert für diesen Parameter ausgibt.

```diff
+          "osProfile": {
+            "computerNamePrefix": "vm",
+            "adminUsername": "[parameters('adminUsername')]",
+            "adminPassword": "[parameters('adminPassword')]"
+          },
```

Abschließend müssen wir die Netzwerkkonfiguration für die VMs in der Skalierungsgruppe angeben. In diesem Fall müssen wir nur die ID des zuvor erstellten Subnetzes festlegen, damit die Skalierungsgruppe weiß, dass sie die Netzwerkschnittstellen in diesem Subnetz platzieren soll. Wir können die ID des virtuellen Netzwerks, das das Subnetz enthält, mithilfe der Vorlagenfunktion `resourceId` erhalten. Diese Funktion nimmt den Typ und den Namen einer Ressource an und gibt den vollqualifizierten Bezeichner dieser Ressource zurück (diese ID hat das Format `/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`). Allerdings ist der Bezeichner des virtuellen Netzwerks nicht ausreichend. Wir müssen das jeweilige Subnetz angeben, in dem sich die VMs der Skalierungsgruppe befinden sollen, also verknüpfen wir `/subnets/mySubnet` mit der ID des virtuellen Netzwerks. Das Ergebnis ist die vollqualifizierte ID des Subnetzes. Wir führen diese Verkettung mit der `concat`-Funktion durch, die eine Reihe von Zeichenfolgen annimmt und deren Verkettung zurückgibt.

```diff
+          "networkProfile": {
+            "networkInterfaceConfigurations": [
+              {
+                "name": "myNic",
+                "properties": {
+                  "primary": "true",
+                  "ipConfigurations": [
+                    {
+                      "name": "myIpConfig",
+                      "properties": {
+                        "subnet": {
+                          "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
+                        }
+                      }
+                    }
+                  ]
+                }
+              }
+            ]
+          }
+        }
+      }
+    }
+  ]
+}

```

## <a name="next-steps"></a>Nächste Schritte

Sie können die vorangehende Vorlage bereitstellen, indem Sie [diese Dokumentation](../azure-resource-manager/resource-group-template-deploy.md) beachten.

Weitere allgemeine Informationen zu Skalierungsgruppen finden Sie in der [Übersicht über Skalierungsgruppen](./virtual-machine-scale-sets-overview.md)
