---
title: Automatische Erweiterung der Azure Event Hub-Durchsatzeinheiten | Microsoft-Dokumentation
description: "Aktivieren der automatischen Vergrößerung in einem Namespace zur automatischen Erweiterung der Durchsatzeinheiten"
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: shvija;sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 5bbeb9d4516c2b1be4f5e076a7f63c35e4176b36
ms.openlocfilehash: b085091ea7bfd601efb0eee84144ddd091422d6e
ms.contentlocale: de-de
ms.lasthandoff: 06/13/2017


---

# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>Automatische Erweiterung der Azure Event Hub-Durchsatzeinheiten

## <a name="overview"></a>Übersicht

Azure Event Hubs ist eine extrem skalierbare Datenstreamingplattform. Daher erweitern Event Hubs-Kunden ihre Nutzung häufig nach der Integration des Dienstes. Für solche Erweiterungen müssen die vordefinierten Durchsatzeinheiten (TUs, Throughput Units) zur Skalierung von Event Hubs und zur Verarbeitung größerer Übertragungsraten erweitert werden. Mit der Funktion *Automatische Vergrößerung* von Event Hubs wird die Anzahl von TUs automatisch an die Nutzungsanforderungen angepasst. Durch die Erweiterung von TUs werden Drosselungsszenarios verhindert, bei denen Folgendes vorliegt:

* Die Datenerfassungsrate überschreitet die festgelegten TUs.
* Die Rate für ausgehende Datenanforderungen überschreitet die festgelegten TUs.

## <a name="how-auto-inflate-works"></a>Funktionsweise der automatischen Vergrößerung

Der Datenverkehr von Event Hubs wird von Durchsatzeinheiten gesteuert. Eine einzelne TU lässt eingehenden Datenverkehr von 1 MB/s und die doppelte Menge an ausgehendem Datenverkehr zu. Standard Event Hubs kann mit 1 bis 20 Durchsatzeinheiten konfiguriert werden. Dank der automatischen Vergrößerung können Sie zunächst mit den mindestens erforderlichen Durchsatzeinheiten beginnen. Je nach der Zunahme des Datenverkehrs passt die Funktion die Durchsatzeinheiten automatisch bis zur maximal erforderlichen Anzahl an. Die automatische Vergrößerung hat folgende Vorteile:

- Effizienter Skalierungsmechanismus, der eine Vergrößerung entsprechend der Zunahme des Datenverkehrs ermöglicht
- Automatische Skalierung bis zur festgelegten Obergrenze ohne Probleme aufgrund einer Drosselung
- Mehr Kontrolle über die Skalierung, da Sie bestimmen, wann in welchem Umfang skaliert wird

## <a name="enable-auto-inflate-on-a-namespace"></a>Aktivieren der automatischen Vergrößerung in einem Namespace

Sie können die automatische Vergrößerung in einem Namespace mit einer der folgenden Methoden aktivieren oder deaktivieren:

1. [Azure-Portal](https://portal.azure.com)
2. Azure Resource Manager-Vorlage

### <a name="enable-auto-inflate-through-the-portal"></a>Aktivieren der automatischen Vergrößerung über das Portal

Sie können die Funktion zur automatischen Vergrößerung in einem Namespace beim Erstellen eines Event Hubs-Namespace aktivieren:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

Wenn diese Option aktiviert ist, können Sie mit wenigen Durchsatzeinheiten beginnen und die Anzahl mit zunehmenden Nutzungsanforderungen anpassen. Die Obergrenze für die Vergrößerung hat keine Auswirkungen auf den Preis. Dieser hängt von der Anzahl von genutzten TUs pro Stunde ab.

Sie können die automatische Vergrößerung auch über die Option **Skalieren** auf dem Blatt für Einstellungen im Portal aktivieren:
 
![](./media/event-hubs-auto-inflate/event-hubs-auto-inflate2.png)

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a>Aktivieren der automatischen Vergrößerung mithilfe einer Azure Resource Manager-Vorlage

Sie können die automatische Vergrößerung bei der Bereitstellung einer Azure Resource Manager-Vorlage aktivieren. Legen Sie beispielsweise die Eigenschaft `isAutoInflateEnabled` auf **TRUE** und `maximumThroughputUnits` auf 10 fest.

```json
"resources": [
        {
            "apiVersion": "2017-04-01",
            "name": "[parameters('namespaceName')]",
            "type": "Microsoft.EventHub/Namespaces",
            "location": "[variables('location')]",
            "sku": {
                "name": "Standard",
                "tier": "Standard"
            },
            "properties": {
                "isAutoInflateEnabled": true,
                "maximumThroughputUnits": 10
            },
            "resources": [
                {
                    "apiVersion": "2017-04-01",
                    "name": "[parameters('eventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
                    ],
                    "properties": {},
                    "resources": [
                        {
                            "apiVersion": "2017-04-01",
                            "name": "[parameters('consumerGroupName')]",
                            "type": "ConsumerGroups",
                            "dependsOn": [
                                "[parameters('eventHubName')]"
                            ],
                            "properties": {}
                        }
                    ]
                }
            ]
        }
    ]
```

Die vollständige Vorlage finden Sie unter [Create Event Hubs namespace and enable inflate (Erstellen eines Event Hubs-Namespace und aktivieren der Vergrößerung)](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) auf GitHub.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

* [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)
* [Erstellen eines Event Hubs](event-hubs-create.md)

