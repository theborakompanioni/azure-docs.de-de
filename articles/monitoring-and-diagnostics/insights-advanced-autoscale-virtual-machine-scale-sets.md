---
title: Erweiterte Autoskalierung mit Azure Virtual Machines | Microsoft-Dokumentation
description: Hier werden Resource Manager und VM-Skalierungsgruppen mit mehreren Regeln und Profilen verwendet, die mit Skalierungsaktionen E-Mails senden und Webhook-URLs aufrufen.
author: kamathashwin
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 7e3576e2-4a2b-4736-b5ae-98c4689cdd2b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2016
ms.author: ashwink
translationtype: Human Translation
ms.sourcegitcommit: b521c3bb99c9a6980df46eacec4e1ebe72fb01a1
ms.openlocfilehash: 00f6a7af021029d1795071c930e05f200578fa14
ms.lasthandoff: 02/23/2017


---

# <a name="advanced-autoscale-configuration-using-resource-manager-templates-for-vm-scale-sets"></a>Konfiguration der erweiterten automatischen Skalierung mithilfe von Resource Manager-Vorlagen für VM-Skalierungsgruppen
Sie können VM-Skalierungsgruppen basierend auf Leistungsmetrik-Schwellenwerten horizontal herunter- und hochskalieren – entweder nach einem sich wiederholenden Zeitplan oder zu einem bestimmten Datum. Außerdem können Sie E-Mail- und Webhookbenachrichtigungen für Skalierungsaktionen konfigurieren. Diese exemplarische Vorgehensweise zeigt ein Beispiel für die Konfiguration der oben genannten Objekte mithilfe einer Resource Manager-Vorlage für eine VM-Skalierungsgruppe.

> [!NOTE]
> Zwar werden in dieser exemplarischen Vorgehensweise die Schritte für VM-Skalierungsgruppen erläutert, die Informationen gelten aber auch für die automatische Skalierung von Cloud Services und Web-Apps.
> Wenn Sie eine einfache Einstellung für horizontales Herunterskalieren/Hochskalieren für eine VM-Skalierungsgruppe konfigurieren möchten, die auf einer einfachen Leistungsmetrik wie CPU basiert, finden Sie in den Dokumenten zu [Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) und [Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) weitere Informationen.
>
>

## <a name="walkthrough"></a>Exemplarische Vorgehensweise
In dieser exemplarischen Vorgehensweise verwenden wir den [Azure-Ressourcen-Explorer](https://resources.azure.com/) zum Konfigurieren und Aktualisieren der Einstellung für das automatische Skalieren einer Skalierungsgruppe. Der Azure-Ressourcen-Explorer bietet eine einfache Möglichkeit zum Verwalten von Azure-Ressourcen über Resource Manager-Vorlagen. Wenn Sie den Azure-Ressourcen-Explorer noch nicht kennen, lesen Sie zunächst [diese Einführung](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/).

1. Stellen Sie eine neue Skalierungsgruppe mit grundlegenden Einstellungen für die automatischen Skalierung bereit. In diesem Artikel wird die Windows-Skalierungsgruppe aus dem Azure-Schnellstartkatalog mit einer Standardvorlage für die automatische Skalierung verwendet. Die Linux-Skalierung erfolgt in gleicher Weise.
2. Navigieren Sie nach dem Erstellen der Skalierungsgruppe mithilfe des Azure-Ressourcen-Explorers zur Ressource der Skalierungsgruppe. Unter dem Microsoft.Insights-Knoten wird Folgendes angezeigt:

    ![Azure Explorer](./media/insights-advanced-autoscale-vmss/azure_explorer_navigate.png)

    Durch die Ausführung der Vorlage wurde eine Standardeinstellung für die automatische Skalierung mit dem Namen **autoscalewad** erstellt. Auf der rechten Seite wird die vollständige Definition dieser Einstellung für die automatische Skalierung angezeigt. In diesem Fall verfügt die Standardeinstellung für die automatische Skalierung über eine CPU%-basierte Regel für horizontales Hoch- und Herunterskalieren.  

3. Sie können nun basierend auf dem Zeitplan oder bestimmten Anforderungen weitere Profile und Regeln hinzufügen. Wir erstellen eine Einstellung für die automatische Skalierung mit drei Profilen. Weitere Informationen zu Profilen und Regeln bei der automatischen Skalierung finden Sie unter [Empfohlene Methoden für die automatische Skalierung](insights-autoscale-best-practices.md).  

    | Profile und Regeln | Beschreibung |
    |--- | --- |
    | **Profil** |**Basierend auf Leistung/Metrik** |
    | Regel |Anzahl von Service Bus-Warteschlangennachrichten > x |
    | Regel |Anzahl von Service Bus-Warteschlangennachrichten < y |
    | Regel |CPU % > n |
    | Regel |CPU% < p |
    | **Profil** |**Wochentage morgens (keine Regeln)** |
    | **Profil** |**Tag der Produkteinführung (keine Regeln)** |

4. Dies ist ein hypothetisches Skalierungsszenario, das wir für diese exemplarische Vorgehensweise verwenden.

    * **Lastbasiert:** Ich möchte je nach Last meiner Anwendung, die in meiner Skalierungsgruppe gehostet wird, horizontal hoch- oder herunterskalieren.*
    * **Länge der Nachrichtenwarteschlange:** Ich verwende eine Service Bus-Warteschlange für die eingehenden Nachrichten, die an meine Anwendung gerichtet sind. Ich verwende die Anzahl von Warteschlangennachrichten und die CPU-Auslastung in Prozent. Außerdem konfiguriere ich ein Standardprofil zum Auslösen einer Skalierungsaktion, wenn entweder die Nachrichtenanzahl oder die CPU-Auslastung den Schwellenwert erreicht.*
    * **Zeitpunkt innerhalb einer Woche und Tageszeit:** Ich möchte ein sich wöchentlich wiederholendes, auf der Tageszeit basierendes Profil namens „Wochentage, morgens“ verwenden. Den Verlaufsdaten habe ich entnommen, dass es besser ist, über eine bestimmte Anzahl von VM-Instanzen zu verfügen, damit meine Anwendungslast während dieser Zeit bewältigt werden kann.*
    * **Sondertermine:** Ich habe ein Profil vom Typ „Tag der Produkteinführung“ hinzugefügt. Ich plane für bestimmte Tage im Voraus, damit meine Anwendung die Last bewältigen kann, die durch Marketingankündigungen und die Einführung neuer Produkte verursacht wird.*
    * *Die letzten zwei Profile können auch andere auf Leistungsmetriken basierende Regeln umfassen. In diesem Fall werden jedoch stattdessen die auf Leistungsmetriken basierenden Standardregeln verwendet. Regeln sind für sich wiederholende und datumsbasierte Profile optional.*

    Die Profil- und Regelpriorisierung des Moduls für die automatische Skalierung wird auch im Artikel [Empfohlene Methoden für die automatische Skalierung](insights-autoscale-best-practices.md) erläutert.
    Eine Liste mit gängigen Metriken für die automatische Skalierung finden Sie unter [Allgemeine Metriken für die automatische Skalierung](insights-autoscale-common-metrics.md).

5. Stellen Sie sicher, dass Sie sich im Ressourcen-Explorer im Modus **Lesen/Schreiben** befinden.

    ![Autoscalewad, Standardeinstellung für die automatische Skalierung](./media/insights-advanced-autoscale-vmss/autoscalewad.png)

6. Klicken Sie auf "Bearbeiten". **Ersetzen** Sie das profiles-Element in der Einstellung für die automatische Skalierung durch folgende Konfiguration:

    ![Profile](./media/insights-advanced-autoscale-vmss/profiles.png)

    ```
    {
            "name": "Perf_Based_Scale",
            "capacity": {
              "minimum": "2",
              "maximum": "12",
              "default": "2"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "MessageCount",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 10
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "MessageCount",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 3
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "Percentage CPU",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/<this_vmss_name>",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT30M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 85
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              },
              {
                "metricTrigger": {
                  "metricName": "Percentage CPU",
                  "metricNamespace": "",
                  "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/<this_vmss_name>",
                  "timeGrain": "PT5M",
                  "statistic": "Average",
                  "timeWindow": "PT30M",
                  "timeAggregation": "Average",
                  "operator": "LessThan",
                  "threshold": 60
                },
                "scaleAction": {
                  "direction": "Decrease",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          },
          {
            "name": "Weekday_Morning_Hours_Scale",
            "capacity": {
              "minimum": "4",
              "maximum": "12",
              "default": "4"
            },
            "rules": [],
            "recurrence": {
              "frequency": "Week",
              "schedule": {
                "timeZone": "Pacific Standard Time",
                "days": [
                  "Monday",
                  "Tuesday",
                  "Wednesday",
                  "Thursday",
                  "Friday"
                ],
                "hours": [
                  6
                ],
                "minutes": [
                  0
                ]
              }
            }
          },
          {
            "name": "Product_Launch_Day",
            "capacity": {
              "minimum": "6",
              "maximum": "20",
              "default": "6"
            },
            "rules": [],
            "fixedDate": {
              "timeZone": "Pacific Standard Time",
              "start": "2016-06-20T00:06:00Z",
              "end": "2016-06-21T23:59:00Z"
            }
          }
    ```
    Die unterstützten Felder und ihre Werte finden Sie in der [Dokumentation zur REST-API für die automatische Skalierung](https://msdn.microsoft.com/en-us/library/azure/dn931928.aspx). Ihre Einstellung für die automatische Skalierung enthält jetzt die drei zuvor erläuterten Profile.

7. Betrachten Sie abschließend den Abschnitt **notification** der automatischen Skalierung. Mit Benachrichtigungen für die automatische Skalierung können Sie drei Aufgaben ausführen, wenn eine Aktion zum horizontalen Hoch- oder Herunterskalieren erfolgreich ausgelöst wurde.
   - Sie können den Administrator und Co-Administratoren Ihres Abonnements benachrichtigen.
   - Sie können eine E-Mail an eine Benutzergruppe senden.
   - Sie können einen Webhookaufruf auslösen. Wenn dieser Webhook ausgelöst wird, sendet er Metadaten zur Bedingung für die automatische Skalierung sowie zur Ressource der Skalierungsgruppe. Weitere Informationen über die Nutzlast des Webhooks für die automatische Skalierung finden Sie unter [Konfigurieren von Webhook- und E-Mail-Benachrichtigungen für die automatische Skalierung](insights-autoscale-to-webhook-email.md).

   Ergänzen Sie die Einstellung für die automatische Skalierung wie folgt, und ersetzen Sie dabei Ihr **notification**-Element, dessen Wert NULL ist.

   ```
   "notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": true,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]

   ```

   Klicken Sie im Ressourcen-Explorer auf die Schaltfläche **Put**, um die Einstellung für die automatische Skalierung zu aktualisieren.

Sie haben eine Einstellung für die automatische Skalierung für eine VM-Skalierungsgruppe aktualisiert, um mehrere Skalierungsprofile und Skalierungsbenachrichtigungen aufzunehmen.

## <a name="next-steps"></a>Nächste Schritte
Über die folgenden Links können Sie auf weitere Informationen zur automatischen Skalierung zugreifen.

[Beheben von Problemen bei der automatischen Skalierung von VM-Skalierungsgruppen](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)

[Allgemeine Metriken für die automatische Skalierung](insights-autoscale-common-metrics.md)

[Empfohlene Methoden für die automatische Skalierung](insights-autoscale-best-practices.md)

[Verwalten der automatischen Skalierung mit PowerShell](insights-powershell-samples.md#create-and-manage-autoscale-settings)

[Verwalten der automatischen Skalierung über die CLI](insights-cli-samples.md#autoscale)

[Konfigurieren von Webhook- und E-Mail-Benachrichtigungen für die automatische Skalierung](insights-autoscale-to-webhook-email.md)

