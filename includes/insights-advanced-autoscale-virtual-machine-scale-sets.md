# Erweiterte Konfiguration der automatischen Skalierung mithilfe von Resource Manager-Vorlagen für VM-Skalierungsgruppen

Sie können VM-Skalierungsgruppen basierend auf Leistungsmetrikschwellenwerten horizontal hoch- und herunterskalieren – entweder nach einem sich wiederholenden Zeitplan oder an einem bestimmten Datum. Außerdem können Sie E-Mail- und Webhookbenachrichtigungen für Skalierungsaktionen konfigurieren. Diese exemplarische Vorgehensweise beinhaltet ein Beispiel für die Konfiguration der oben genannten Elemente mithilfe einer Resource Manager-Vorlage für eine VM-Skalierungsgruppe.

>[AZURE.NOTE] Zwar werden in dieser exemplarischen Vorgehensweise die Schritte für VM-Skalierungsgruppen erläutert, Sie können diese aber auch für die automatische Skalierung von Cloud Services und Web-Apps anwenden. Wenn Sie eine einfache Einstellung für horizontales Herunterskalieren/Hochskalieren für eine VM-Skalierungsgruppe konfigurieren möchten, die auf einer einfachen Leistungsmetrik wie CPU basiert, finden Sie in Dokumenten zu [Linux](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) und [Windows](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md) weitere Informationen.



## Exemplarische Vorgehensweise
In dieser exemplarischen Vorgehensweise verwenden wir den [Azure-Ressourcen-Explorer](https://resources.azure.com/) zum Konfigurieren und Aktualisieren der Einstellung für das automatische Skalieren einer Skalierungsgruppe. Der Azure-Ressourcen-Explorer bietet eine einfache Möglichkeit zum Verwalten von Azure-Ressourcen über Resource Manager-Vorlagen. Wenn Sie den Azure-Ressourcen-Explorer noch nicht kennen, lesen Sie zunächst [diese Einführung](https://azure.microsoft.com/blog/azure-resource-explorer-a-new-tool-to-discover-the-azure-api/).

1. Stellen Sie eine neue Skalierungsgruppe mit grundlegenden Einstellungen für die automatischen Skalierung bereit. In diesem Artikel wird die Windows-Skalierungsgruppe aus dem Azure-Schnellstartkatalog mit einer Standardvorlage für die automatische Skalierung verwendet. Die Linux-Skalierung erfolgt in gleicher Weise.

2. Navigieren Sie nach dem Erstellen der Skalierungsgruppe mithilfe des Azure-Ressourcen-Explorers zur Ressource der Skalierungsgruppe. Unter dem Microsoft.Insights-Knoten wird Folgendes angezeigt:

	![Azure Explorer](./media/insights-advanced-autoscale-vmss/azure_explorer_navigate.png)

	Durch die Ausführung der Vorlage wurde eine Standardeinstellung für die automatische Skalierung mit dem Namen **autoscalewad** erstellt. Auf der rechten Seite wird die vollständige Definition dieser Einstellung für die automatische Skalierung angezeigt. In diesem Fall verfügt die Standardeinstellung für die automatische Skalierung über eine CPU%-basierte Regel für horizontales Hoch- und Herunterskalieren.

3. Sie können nun basierend auf dem Zeitplan oder bestimmten Anforderungen weitere Profile und Regeln hinzufügen. Wir erstellen eine Einstellung für die automatische Skalierung mit drei Profilen. Weitere Informationen zu Profilen und Regeln bei der automatischen Skalierung finden Sie unter [Empfohlene Methoden für die automatische Skalierung in Azure Insights](../articles/azure-portal/insights-autoscale-best-practices.md).

    | Profile und Regeln | Beschreibung |
	|---------|-------------------------------------|
	| **Profil** | **Basierend auf Leistung/Metrik** |
	| Regel | Anzahl von Service Bus-Warteschlangennachrichten > x |
	| Regel | Anzahl von Service Bus-Warteschlangennachrichten < y |
	| Regel | CPU% < n |
	| Regel | CPU% < p |
	| **Profil** | **Wochentage morgens, (keine Regeln)** |
	| **Profil** | **Tag der Produkteinführung (keine Regeln)** |

4. Dies ist ein hypothetisches Skalierungsszenario, das wir für diese exemplarische Vorgehensweise verwenden.
	- _**Lastbasiert**: Ich möchte je nach Last meiner Anwendung, die in meiner Skalierungsgruppe gehostet wird, horizontal hoch- oder herunterskalieren._
	- _**Länge der Nachrichtenwarteschlange**: Ich verwende eine Service Bus-Warteschlange für die eingehenden Nachrichten, die von meiner Anwendung empfangen werden. Ich verwende die Anzahl von Warteschlangennachrichten und CPU%. Außerdem konfiguriere ich ein Standardprofil zum Auslösen einer Skalierungsaktion, wenn entweder die Nachrichtenanzahl oder die CPU-Nutzung in Prozent den Schwellenwert erreicht._
	- _**Zeitpunkt innerhalb einer Woche und Tageszeit**: Ich möchte ein sich wöchentlich wiederholendes, auf der Tageszeit basierendes Profil namens „Wochentage, morgens“. Den Verlaufsdaten habe ich entnommen, dass es besser ist, über eine bestimmte Anzahl von VM-Instanzen zu verfügen, damit meine Anwendungslast während dieser Zeit bewältigt werden kann._
	- _**Sondertermine**: Ich habe ein Profil „Tag der Produkteinführung“ hinzugefügt. Ich plane für bestimmte Tage im Voraus, damit meine Anwendung die Last verarbeiten kann, die durch Marketingankündigungen und die Einführung neuer Produkte verursacht wird._
	- _Die letzten zwei Profile können auch andere auf Leistungsmetriken basierende Regeln umfassen. In diesem Fall werden jedoch stattdessen die auf Leistungsmetriken basierenden Standardregeln verwendet. Regeln sind für sich wiederholende und datumsbasierte Profile optional._

	Die Profil- und Regelpriorisierung des Moduls für die automatische Skalierung wird auch im Artikel [Empfohlene Methoden für die automatische Skalierung in Azure Insights](../articles/azure-portal/insights-autoscale-best-practices.md) erläutert. Eine Liste mit gängigen Metriken für die automatische Skalierung finden Sie unter [Allgemeine Metriken für die automatische Skalierung](../articles/azure-portal/insights-autoscale-common-metrics.md).

5. Stellen Sie sicher, dass Sie sich im Ressourcen-Explorer im Modus **Lesen/Schreiben** befinden.

	![Autoscalewad, Standardeinstellung für die automatische Skalierung](./media/insights-advanced-autoscale-vmss/autoscalewad.png)

6. Klicken Sie auf "Bearbeiten". **Ersetzen** Sie das profiles-Element in der Einstellung für die automatische Skalierung durch Folgendes:

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
	              "metricName": "\\Processor(_Total)\\% Processor Time",
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
	              "metricName": "\\Processor(_Total)\\% Processor Time",
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
	              "direction": "Increase",
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
	Die unterstützten Felder und ihre Werte finden Sie in der [Dokumentation zur REST-API für die automatische Skalierung](https://msdn.microsoft.com/de-DE/library/azure/dn931928.aspx).

	Ihre Einstellung für die automatische Skalierung enthält jetzt die drei zuvor erläuterten Profile.

7. 	Abschließend betrachten wir den **notification**-Abschnitt für die automatische Skalierung. Mit Benachrichtigungen für die automatische Skalierung können Sie drei Aufgaben ausführen, wenn eine Aktion zum horizontalen Hoch- oder Herunterskalieren erfolgreich ausgelöst wurde.

	1. Sie können den Administrator und Co-Administratoren Ihres Abonnements benachrichtigen.

	2. Sie können eine E-Mail an eine Benutzergruppe senden.

	3. Sie können einen Webhookaufruf auslösen. Wenn dieser Webhook ausgelöst wird, sendet er Metadaten zur Bedingung für die automatische Skalierung sowie zur Ressource der Skalierungsgruppe. Weitere Informationen über die Nutzlast des Webhooks für die automatische Skalierung finden Sie unter [Konfigurieren von Webhook- und E-Mail-Benachrichtigungen für die automatische Skalierung](../articles/azure-portal/insights-autoscale-to-webhook-email.md).

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

## Nächste Schritte

Über die folgenden Links können Sie auf weitere Informationen zur automatischen Skalierung zugreifen.

[Allgemeine Metriken für die automatische Skalierung](../articles/azure-portal/insights-autoscale-common-metrics.md)

[Empfohlene Methoden für die automatische Skalierung](../articles/azure-portal/insights-autoscale-best-practices.md)

[Verwalten der automatischen Skalierung mit PowerShell](../articles/azure-portal/insights-powershell-samples.md#create-and-manage-autoscale-settings)

[Verwalten der automatischen Skalierung über die CLI](../articles/azure-portal/insights-cli-samples.md#autoscale)

[Konfigurieren von Webhook- und E-Mail-Benachrichtigungen für die automatische Skalierung](../articles/azure-portal/insights-autoscale-to-webhook-email.md)

<!---HONumber=AcomDC_0817_2016-->