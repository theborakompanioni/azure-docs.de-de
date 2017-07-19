---
title: "Verweisen auf ein vorhandenes virtuelles Netzwerk in einer Vorlage für eine Azure-Skalierungsgruppe | Microsoft-Dokumentation"
description: "Informationen zum Hinzufügen eines virtuellen Netzwerks zu einer vorhandenen Vorlage für eine Azure-VM-Skalierungsgruppe"
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
ms.date: 06/27/2017
ms.author: negat
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 28117d467b491704aed8d45e5eba42530579dfa2
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017

---

# <a name="add-reference-to-an-existing-virtual-network-in-an-azure-scale-set-template"></a>Hinzufügen eines Verweises auf ein vorhandenes virtuelles Netzwerk in einer Vorlage für eine Azure-Skalierungsgruppe

In diesem Artikel wird gezeigt, wie die [Vorlage für eine kleinstmögliche Skalierungsgruppe](./virtual-machine-scale-sets-mvss-start.md) geändert wird, um die Bereitstellung in einem vorhandenen virtuellen Netzwerk durchzuführen, statt ein neues zu erstellen.

## <a name="change-the-template-definition"></a>Ändern der Vorlagendefinition

Unsere Vorlage für die kleinstmögliche Skalierungsgruppe kann [hier](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json) angezeigt werden, und unsere Vorlage für die Bereitstellung der Skalierungsgruppe in einem vorhandenen virtuellen Netzwerk kann [hier](https://raw.githubusercontent.com/gatneil/mvss/existing-vnet/azuredeploy.json) angezeigt werden. Sehen wir uns die Diff zum Erstellen dieser Vorlage (`git diff minimum-viable-scale-set existing-vnet`) Stück für Stück an:

Wir fügen zunächst einen `subnetId`-Parameter hinzu. Diese Zeichenfolge wird an die Skalierungsgruppenkonfiguration übergeben und ermöglicht der Skalierungsgruppe das Identifizieren des vorab erstellten Subnetzes, in dem virtuelle Computer bereitgestellt werden sollen. Diese Zeichenfolge muss folgendes Format aufweisen: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>/subnets/<subnet-name>`. Beispiel: Zum Bereitstellen der Skalierungsgruppe in einem vorhandenen virtuellen Netzwerk mit dem Namen `myvnet`, dem Subnetz `mysubnet`, der Ressourcengruppe `myrg`, und dem Abonnement `00000000-0000-0000-0000-000000000000`, wäre „subnetId“: `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myrg/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/mysubnet`.

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "subnetId": {
+      "type": "string"
     }
   },
```

Als Nächstes können wir die Ressource für das virtuelle Netzwerk aus dem `resources`-Array löschen, da wir ein vorhandenes virtuelles Netzwerk verwenden und kein neues bereitstellen müssen.

```diff
   "variables": {},
   "resources": [
-    {
-      "type": "Microsoft.Network/virtualNetworks",
-      "name": "myVnet",
-      "location": "[resourceGroup().location]",
-      "apiVersion": "2016-12-01",
-      "properties": {
-        "addressSpace": {
-          "addressPrefixes": [
-            "10.0.0.0/16"
-          ]
-        },
-        "subnets": [
-          {
-            "name": "mySubnet",
-            "properties": {
-              "addressPrefix": "10.0.0.0/16"
-            }
-          }
-        ]
-      }
-    },
```

Das virtuelle Netzwerk ist bereits vorhanden, bevor die Vorlage bereitgestellt wird, daher besteht keine Notwendigkeit, eine dependsOn-Klausel in der Skalierungsgruppe für das virtuelle Netzwerk anzugeben. Deshalb löschen wir diese Zeilen:

```diff
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
-      "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
-      ],
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
```

Schließlich übergeben wir den `subnetId`-Parameter, der vom Benutzer festgelegt wird (statt `resourceId`, um die ID des VNET in der gleichen Bereitstellung abzurufen, was von der Vorlage für die kleinstmögliche Skalierungsgruppe ausgeführt wird).

```diff
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
-                          "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
+                          "id": "[parameters('subnetId')]"
                         }
                       }
                     }
```




## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]

