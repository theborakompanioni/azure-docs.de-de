<properties
	pageTitle="Vertikales Skalieren von Azure VM-Skalierungsgruppen | Microsoft Azure"
	description="Erfahren Sie, wie Sie einen virtuellen Computer als Reaktion auf die Überwachung von Warnungen mit Azure Automation vertikal skalieren."
	services="virtual-machine-scale-sets"
	documentationCenter=""
	authors="gbowerman"
	manager="madhana"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machine-scale-sets"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-multiple"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/03/2016"
	ms.author="guybo"/>

# Vertikale automatische Skalierung mit VM-Skalierungsgruppen

In diesem Artikel wird beschrieben, wie Sie Azure [VM-Skalierungsgruppen](https://azure.microsoft.com/services/virtual-machine-scale-sets/) mit oder ohne erneute Bereitstellung vertikal skalieren können. Für die vertikale Skalierung von virtuellen Computern, die sich nicht in Skalierungsgruppen befinden, finden Sie weitere Informationen unter [Vertikales Skalieren von virtuellen Azure-Computern mit Azure Automation](../virtual-machines/virtual-machines-windows-vertical-scaling-automation.md).

Bei der vertikalen Skalierung, auch bekannt als _zentrales Hochskalieren_ und _zentrales Herunterskalieren_, wird die Größe eines virtuellen Computers (VM) als Antwort auf eine Workload vergrößert oder verringert. Vergleichen Sie dies mit der [horizontale Skalierung](./virtual-machine-scale-sets-autoscale-overview.md), die auch als _horizontales Hochskalieren_ und _horizontales Herunterskalieren_ bezeichnet wird, bei der die Anzahl der virtuellen Computer je nach Workload geändert wird.

Bei der erneuten Bereitstellung wird ein vorhandener virtueller Computer entfernt und durch einen neuen ersetzt. Wenn Sie die Größe der virtuellen Computer in einer VM-Skalierungsgruppe erhöhen oder verringern, ist es in einigen Fällen angebracht, die Größe von vorhandenen virtuellen Computern zu ändern und Ihre Daten beizubehalten. In anderen Fällen müssen Sie neue virtuelle Computer der neuen Größe bereitstellen. In diesem Dokument werden beide Fälle behandelt.

Die vertikale Skalierung kann in folgenden Fällen nützlich sein:

- Ein Dienst basierend auf virtuellen Computern wird zu wenig verwendet (beispielsweise am Wochenende). Das Reduzieren der Größe des virtuellen Computers kann die monatlichen Kosten reduzieren.
- Beim Erhöhen der Größe des virtuellen Computers, um den größeren Bedarf zu bewältigen, ohne zusätzliche virtuelle Computer zu erstellen.

Sie können die vertikale Skalierung so einrichten, dass sie auf der Grundlage von metrikbasierten Warnungen aus Ihrer VM-Skalierungsgruppe ausgelöst wird. Bei Aktivierung der Warnung wird ein Webhook ausgelöst, der wiederum ein Runbook auslöst, das Ihre Skalierungsgruppe hoch- oder herunterskalieren kann. Die vertikale Skalierung kann wie folgt konfiguriert werden:

1. Erstellen Sie ein Azure Automation-Konto mit der Funktion „Ausführen als“.
2. Importieren Sie Azure Automation-Runbooks für vertikale Skalierungsgruppen in Ihr Abonnement.
3. Fügen Sie Ihrem Runbook einen Webhook hinzu.
4. Fügen Sie Ihrer VM-Skalierungsgruppe mithilfe einer Webhook-Benachrichtigung eine Warnung hinzu.

> [AZURE.NOTE] Die vertikale automatische Skalierung kann nur innerhalb bestimmter VM-Größenbereiche stattfinden. Vergleichen Sie die Spezifikationen der einzelnen Größen, bevor Sie sich für die Skalierung entscheiden (eine höhere Zahl bedeutet nicht immer, dass der virtuelle Computer größer ist). Sie können zwischen den folgenden Größenpaaren skalieren:

>| VM-Größenpaare für die Skalierung | |
|---|---|
| Standard\_A0 | Standard\_A11 |
| Standard\_D1 | Standard\_D14 |
| Standard\_DS1 | Standard\_DS14 |
| Standard\_D1v2 | Standard\_D15v2 |
| Standard\_G1 | Standard\_G5 |
| Standard\_GS1 | Standard\_GS5 |

## Erstellen eines Azure Automation-Kontos mit der Funktion „Ausführen als“

Als Erstes müssen Sie lediglich ein Azure Automation-Konto erstellen, in dem die Runbooks gehostet werden, mit denen die Instanzen der VM-Skalierungsgruppen skaliert werden. Vor kurzem wurde in [Azure Automation](https://azure.microsoft.com/services/automation/) die Funktion des ausführenden Kontos eingeführt, wodurch die Einrichtung des Dienstprinzipals für die automatische Ausführung der Runbooks im Auftrag des Benutzers stark vereinfacht wird. Weitere Informationen darüber finden Sie im unten stehenden Artikel:

* [Authentifizieren von Runbooks mit der Azure-Option „Ausführendes Konto“](../automation/automation-sec-configure-azure-runas-account.md)

## Importieren der Azure Automation-Runbooks für die vertikale Skalierung in Ihr Abonnement

Die Runbooks, die zur vertikalen Skalierung Ihrer VM-Skalierungsgruppen benötigt werden, sind bereits im Azure Automation-Runbookkatalog veröffentlicht. Befolgen Sie die Schritte in diesem Artikel, um sie in Ihr Abonnement zu importieren:

* [Runbook und Modulkataloge für Azure Automation](../automation/automation-runbook-gallery.md)

Wählen Sie die Option zum Durchsuchen des Katalogs im Menü „Runbooks“ aus:

![Zu importierende Runbooks][runbooks]

Es werden die zu importierenden Runbooks angezeigt: Wählen Sie das Runbook aus, basierend darauf, ob Sie eine vertikale Skalierung mit oder ohne erneute Bereitstellung durchführen möchten:

![Runbooks-Katalog][gallery]

## Hinzufügen eines Webhooks zu Ihrem Runbook

Nachdem Sie die Runbooks importiert haben, müssen Sie einen Webhook zum Runbook hinzufügen, damit es über eine Warnung von einer VM-Skalierungsgruppe ausgelöst werden kann. Ausführliche Informationen zum Erstellen eines Webhooks für Ihr Runbook finden Sie in dem folgenden Artikel:

* [Azure Automation-Webhooks](../automation/automation-webhooks.md)

> [AZURE.NOTE] Stellen Sie sicher, dass Sie den Webhook-URI kopieren, bevor Sie das Dialogfeld für Webhooks schließen, da sie den Webhook im nächsten Abschnitt benötigen.

## Hinzufügen einer Warnung zur VM-Skalierungsgruppe

Im Folgenden finden Sie ein PowerShell-Skript, das zeigt, wie Sie einer VM-Skalierungsgruppe eine Warnung hinzufügen können. Der folgende Artikel enthält Informationen zum Abrufen des Namens der Metrik, um die Warnung auszugeben: [Allgemeine Metriken für die automatische Skalierung in Azure Insights](../azure-portal/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [AZURE.NOTE] Es wird empfohlen, ein angemessenes Zeitfenster für die Warnung zu konfigurieren, um das allzu häufige Auslösen einer vertikale Skalierung und damit verbundener Dienstunterbrechungen zu vermeiden. Ziehen Sie ein Fenster von mindestens 20 bis 30 Minuten in Betracht. Ziehen Sie die horizontale Skalierung in Betracht, wenn Sie Unterbrechungen vermeiden müssen.

Weitere Informationen zum Erstellen von Warnungen finden Sie in den folgenden Artikeln:

* [Azure Insights – PowerShell-Schnellstartbeispiele](../azure-portal/insights-powershell-samples.md)
* [Azure Insights – Schnellstartbeispiele für plattformübergreifende Befehlszeilenschnittstelle](../azure-portal/insights-cli-samples.md)

## Zusammenfassung

In diesem Artikel wurden einige einfache Beispiele der vertikalen Skalierung gezeigt. Mit diesen Bausteinen – Automation-Konto, Runbooks, Webhooks, Warnungen – können Sie eine Vielzahl von Ereignissen mit einem benutzerdefinierten Satz von Aktionen verbinden.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png

<!---HONumber=AcomDC_0810_2016-->