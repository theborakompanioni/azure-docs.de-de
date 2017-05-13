---
title: "Azure Resource Manager-Vorlagen – Aktualisieren einer Ressource | Microsoft-Dokumentation"
description: "Beschreibt das Erweitern der Funktionalität der Azure Resource Manager-Vorlagen für das Aktualisieren einer Ressource."
services: azure-resource-manager, guidance
documentationcenter: na
author: petertay
manager: christb
editor: 
ms.service: guidance
ms.topic: article
ms.date: 05/02/2017
ms.author: mspnp
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 7807b3f6135ede80cd7e7e6486364482cde72e29
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---

# <a name="patterns-for-extending-the-functionality-of-azure-resource-manager-templates---updating-a-resource"></a>Muster zum Erweitern der Funktionalität von Azure Resource Manager-Vorlagen – Aktualisieren einer Ressource

Es gibt einige Szenarien, in denen Sie eine Ressource während der Bereitstellung aktualisieren müssen. Dieses Szenario tritt auf, wenn Sie nicht alle Eigenschaften für eine Ressource angeben können, solange nicht andere, abhängige Ressourcen erstellt werden. Wenn Sie z.B einen Back-End-Adresspool für einen Load Balancer erstellen, könnten Sie vielleicht die Netzwerkschnittstellen (NICs) auf Ihren virtuellen Computern (VMs) aktualisieren, um sie in den Back-End-Pool einzubeziehen. Resource Manager unterstützt das Aktualisieren von Ressourcen während der Bereitstellung, aber Sie müssen die Vorlage ordnungsgemäß entwerfen, um Fehler zu vermeiden und sicherzustellen, dass die Bereitstellung als Update behandelt wird.

## <a name="understand-the-pattern"></a>Grundlegendes zum Muster

Zunächst müssen Sie einmal in der Vorlage auf die Ressource verweisen, um sie zu erstellen, aber dann müssen Sie mit dem gleichen Namen auf die Ressource verweisen, um sie zu einem späteren Zeitpunkt zu aktualisieren. Wenn jedoch zwei Ressourcen in einer Vorlage den gleichen Namen besitzen, löst der Resource Manager eine Ausnahme aus. Um diesen Fehler zu vermeiden, geben Sie die aktualisierte Ressource mit dem Ressourcentyp `Microsoft.Resources/deployments` in einer zweiten Vorlage an, die entweder verknüpft oder als Untervorlage enthalten ist.

Zweitens müssen Sie in der zweiten Vorlage entweder den Namen der vorhandenen zu ändernden Eigenschaft oder einen neuen Namen für eine hinzuzufügende Eigenschaft angeben. Anschließend müssen Sie auch die ursprünglichen Eigenschaften und ihre ursprünglichen Werte angeben. Wenn Sie die ursprünglichen Eigenschaften und Werte nicht angeben, setzt der Resource Manager voraus, dass Sie eine neue Ressource erstellen möchten, und löscht die ursprüngliche Ressource. Er ersetzt die ursprüngliche Ressource durch eine neue Ressource, die nur die neuen Eigenschaften enthält, die Sie angegeben haben.

Schließlich müssen Sie die Ressource von allen zugehörigen Ressourcen abhängig machen, die Sie bereitstellen möchten. Diese Abhängigkeit stellt sicher, dass die Ressourcen in der richtigen Reihenfolge erstellt werden. Die Reihenfolge ist:

1. Ressource erstellt
2. Abhängige Ressourcen erstellt
3. Ressource (aus Schritt 1) mit Werten von abhängigen Ressourcen (Schritt 2) aktualisiert

## <a name="example-template"></a>Beispielvorlage

Die folgende Beispielvorlage veranschaulicht dieses Muster. Sie stellt ein virtuelles Netzwerk (VNet) mit dem Namen `firstVNet` bereit, das ein Subnetz mit dem Namen `firstSubnet` besitzt. Anschließend stellt sie eine virtuelle Netzwerkschnittstelle (NIC) mit dem Namen `nic1` bereit und ordnet sie dem Subnetz zu. Dann schließt eine Bereitstellungsressource mit dem Namen `updateVNet` eine geschachtelte Vorlage ein, die auf den Namen der `firstVNet`-Ressource verweist. 

Betrachten Sie die `addressSpace`- und `subnets`-Eigenschaft dieser Ressource. Beachten Sie, dass der `addressSpace`-Wert des `firstVNet`-Ressourcenbereitstellungsobjekts auf den gleichen Eigenschaftswert festgelegt ist. Im `subnets`-Array ist der Wert für `firstSubnet` gleichermaßen festgelegt. Da alle ursprünglichen `firstVNet`-Eigenschaften angegeben wurden, aktualisiert der Resource Manager die Ressource in Azure. In diesem Fall besteht das Update im Hinzufügen eines neuen Subnetzes namens `secondSubnet`.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "resources": [
      {
      "apiVersion": "2016-03-30",
      "name": "firstVNet",
      "location":"[resourceGroup().location]",
      "type": "Microsoft.Network/virtualNetworks",
      "properties": {
          "addressSpace":{"addressPrefixes": [
              "10.0.0.0/22"
          ]},
          "subnets":[              
              {
                  "name":"firstSubnet",
                  "properties":{
                    "addressPrefix":"10.0.0.0/24"
                  }
              }
            ]
      }
    },
    {
        "apiVersion": "2015-06-15",
        "type":"Microsoft.Network/networkInterfaces",
        "name":"nic1",
        "location":"[resourceGroup().location]",
        "dependsOn": [
            "firstVNet"
        ],
        "properties": {
            "ipConfigurations":[
                {
                    "name":"ipconfig1",
                    "properties": {
                        "privateIPAllocationMethod":"Dynamic",
                        "subnet": {
                            "id": "[concat(resourceId('Microsoft.Network/virtualNetworks','firstVNet'),'/subnets/firstSubnet')]"
                        }
                    }
                }
            ]
        }
    },
    {
      "apiVersion": "2015-01-01",
      "type": "Microsoft.Resources/deployments",
      "name": "updateVNet",
      "dependsOn": [
          "nic1"
      ],
      "properties": {
        "mode": "Incremental",
        "parameters": {},
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [
              {
                  "apiVersion": "2016-03-30",
                  "name": "firstVNet",
                  "location":"[resourceGroup().location]",
                  "type": "Microsoft.Network/virtualNetworks",
                  "properties": {
                      "addressSpace": "[reference('firstVNet').addressSpace]",
                      "subnets":[
                          {
                              "name":"[reference('firstVNet').subnets[0].name]",
                              "properties":{
                                  "addressPrefix":"[reference('firstVNet').subnets[0].properties.addressPrefix]"
                                  }
                          },
                          {
                              "name":"secondSubnet",
                              "properties":{
                                  "addressPrefix":"10.0.1.0/24"
                                  }
                          }
                     ]
                  }
              }
          ],
          "outputs": {}
          }
        }
    }
  ],
  "outputs": {}
}
```

## <a name="try-the-template"></a>Testen der Vorlage

Befolgen Sie die nachfolgenden Schritte, wenn Sie diese Vorlage testen möchten:

1.    Wechseln Sie zum Azure-Portal, wählen Sie das Pluszeichen „+“ aus, und suchen Sie nach dem Ressourcentyp „Vorlagenbereitstellung“. Wenn Sie ihn in den Suchergebnissen finden, wählen Sie ihn aus.
2.    Wenn Sie zur Seite „Vorlagenbereitstellung“ gelangen, wählen Sie die Schaltfläche **Erstellen** aus, sodass das Blatt „Benutzerdefinierte Bereitstellung“ geöffnet wird.
3.    Wählen Sie das Symbol **Bearbeiten** aus.
4.    Löschen Sie die leere Vorlage.
5.    Kopieren Sie die vorherige Beispielvorlage, und fügen Sie sie im rechten Bereich ein.
6.    Wählen Sie die Schaltfläche **Speichern** aus.
7.    Sie kehren zum Bereich „Benutzerdefinierte Bereitstellung“ zurück, aber dieses Mal sind einige Dropdownfelder verfügbar. Wählen Sie Ihr Abonnement aus, erstellen Sie eine neue oder vorhandene Ressourcengruppe, und wählen Sie dann einen Speicherort aus. Lesen Sie die Nutzungsbedingungen, und wählen Sie die Schaltfläche **Ich stimme zu**.
8.    Wählen Sie die Schaltfläche **Kaufen** aus.

Nachdem die Bereitstellung abgeschlossen ist, öffnen Sie die Ressourcengruppe, die Sie im Verwaltungsportal angegeben haben. Sie sehen ein VNet mit dem Namen `firstVNet` und eine NIC mit dem Namen `nic1`. Klicken Sie auf `firstVNet` und dann auf `subnets`. Sie sehen das ursprünglich erstellte `firstSubnet` und das `secondSubnet`, das in der `updateVNet`-Ressource hinzugefügt wurde. 

![Ursprüngliches Subnetz und aktualisiertes Subnetz](./media/resource-manager-update/firstVNet-subnets.png)

Gehen Sie dann zurück zur Ressourcengruppe, und klicken Sie auf `nic1` und dann auf `IP configurations`. Im `IP configurations`-Abschnitt ist `Subnet` auf `firstSubnet (10.0.0.0/24)` festgelegt. 

![IP-Konfigurationseinstellungen für nic1](./media/resource-manager-update/nic1-ipconfigurations.png)

Das ursprüngliche `firstVNet` wurde nicht neu erstellt, sondern aktualisiert. Wäre `firstVNet` neu erstellt worden, wäre `nic1` nicht `firstVNet` zugeordnet.

## <a name="next-steps"></a>Nächste Schritte

Sie können dieses Muster in Ihren Vorlagen verwenden, um die ursprünglichen Eigenschaften der Ressource, die Sie aktualisieren möchten, neu festzulegen. Geben Sie die Updateressource mithilfe des `Microsoft.Resources/deployments`-Ressourcentyps entweder in einer verknüpften oder geschachtelten Vorlage an.

* Eine Einführung zur `reference()`-Funktion finden Sie unter [Vorlagenfunktionen im Azure Resource Manager](resource-group-template-functions.md).
* Dieses Muster ist auch im [Vorlagenbaustein-Projekt](https://github.com/mspnp/template-building-blocks) und in den [Azure-Referenzarchitekturen](/azure/architecture/reference-architectures/) implementiert.
