---
title: "Verweisen auf ein benutzerdefiniertes Image in einer Vorlage für eine Azure-Skalierungsgruppe | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie einer vorhandenen Vorlage für eine Azure-VM-Skalierungsgruppe ein benutzerdefiniertes Image hinzufügen."
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
ms.date: 5/10/2017
ms.author: negat
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: cf52fc9e95267c4bc5c0106aadf626685ddd5c24
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017

---

# <a name="add-a-custom-image-to-an-azure-scale-set-template"></a>Hinzufügen eines benutzerdefinierten Images zu einer Azure-Skalierungsgruppenvorlage

In diesem Artikel erfahren Sie, wie die [Vorlage für eine kleinstmögliche Skalierungsgruppe](./virtual-machine-scale-sets-mvss-start.md) geändert wird, um die Bereitstellung über ein benutzerdefiniertes Image durchzuführen.

## <a name="change-the-template-definition"></a>Ändern der Vorlagendefinition

Unsere Vorlage für die kleinstmögliche Skalierungsgruppe ist [hier](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json) zu finden, und unsere Vorlage für die Bereitstellung der Skalierungsgruppe aus einem benutzerdefinierten Image ist [hier](https://raw.githubusercontent.com/gatneil/mvss/custom-image/azuredeploy.json) zu finden. Sehen wir uns die Diff zum Erstellen dieser Vorlage (`git diff minimum-viable-scale-set custom-image`) Stück für Stück an:

### <a name="creating-a-managed-disk-image"></a>Erstellen eines Images für einen verwalteten Datenträger

Wenn bereits ein benutzerdefiniertes Image für einen verwalteten Datenträger (eine Ressource vom Typ `Microsoft.Compute/images`) vorhanden ist, können Sie diesen Abschnitt überspringen.

Wir fügen zunächst einen Parameter `sourceImageVhdUri` hinzu – den URI für das generalisierte Blob in Azure Storage, das das benutzerdefinierte Image für die Bereitstellung enthält.


```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "sourceImageVhdUri": {
+      "type": "string",
+      "metadata": {
+        "description": "The source of the generalized blob containing the custom image"
+      }
     }
   },
   "variables": {},
```

Als Nächstes fügen wir eine Ressource vom Typ `Microsoft.Compute/images` hinzu – das Image für den verwalteten Datenträger, basierend auf dem generalisierten Blob unter dem URI `sourceImageVhdUri`. Dieses Image muss sich in derselben Region befinden wie die Skalierungsgruppe, die es verwendet. In den Eigenschaften des Images legen wir den Betriebssystemtyp, den Speicherort des Blobs (über den Parameter `sourceImageVhdUri`) und den Speicherkontotyp fest:

```diff
   "resources": [
     {
+      "type": "Microsoft.Compute/images",
+      "apiVersion": "2016-04-30-preview",
+      "name": "myCustomImage",
+      "location": "[resourceGroup().location]",
+      "properties": {
+        "storageProfile": {
+          "osDisk": {
+            "osType": "Linux",
+            "osState": "Generalized",
+            "blobUri": "[parameters('sourceImageVhdUri')]",
+            "storageAccountType": "Standard_LRS"
+          }
+        }
+      }
+    },
+    {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "location": "[resourceGroup().location]",

```

In der Skalierungsgruppenressource fügen wir eine `dependsOn`-Klausel mit Verweis aus das benutzerdefinierte Image hinzu, um sicherzustellen, dass das Image erstellt wird, bevor die Skalierungsgruppe versucht, eine Bereitstellung aus ihm durchzuführen:

```diff
       "location": "[resourceGroup().location]",
       "apiVersion": "2016-04-30-preview",
       "dependsOn": [
-        "Microsoft.Network/virtualNetworks/myVnet"
+        "Microsoft.Network/virtualNetworks/myVnet",
+        "Microsoft.Compute/images/myCustomImage"
       ],
       "sku": {
         "name": "Standard_A1",

```

### <a name="changing-scale-set-properties-to-use-the-managed-disk-image"></a>Ändern der Eigenschaften der Skalierungsgruppe für die Verwendung des Images für den verwalteten Datenträger

In der Option `imageReference` der Skalierungsgruppe `storageProfile` geben wir nicht Herausgeber, Angebot, SKU und Version eines Plattformimages an, sondern die `id` der Ressource `Microsoft.Compute/images`:

```diff
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
-              "publisher": "Canonical",
-              "offer": "UbuntuServer",
-              "sku": "16.04-LTS",
-              "version": "latest"
+              "id": "[resourceId('Microsoft.Compute/images', 'myCustomImage')]"
             }
           },
           "osProfile": {
```

In diesem Beispiel verwenden wir die `resourceId`-Funktion zum Abrufen der Ressourcen-ID des in der gleichen Vorlage erstellten Images. Wenn Sie das Image für den verwalteten Datenträger zuvor erstellt haben, geben Sie stattdessen die ID dieses Images an. Diese ID muss folgendes Format aufweisen: `/subscriptions/<subscription-id>resourceGroups/<resource-group-name>/providers/Microsoft.Compute/images/<image-name>`.


## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]

