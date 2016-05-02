<properties
	pageTitle="Vertikales Skalieren von virtuellen Azure-Computern mit Azure Automation | Microsoft Azure"
	description="Erfahren Sie, wie Sie einen virtuellen Windows-Computer als Reaktion auf die Überwachung von Warnungen mit Azure Automation vertikal skalieren."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="singhkay"
	manager="drewm"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/29/2016"
	ms.author="singhkay"/>

# Vertikales Skalieren von virtuellen Azure-Computern mit Azure Automation

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassisches Bereitstellungsmodell

Als vertikale Skalierung wird der Vorgang bezeichnet, die Ressourcen eines Computers als Reaktion auf die Workload zu erhöhen oder zu verringern. In Azure kann dies erreicht werden, indem die Größe des virtuellen Computers geändert wird. Dies kann in folgenden Szenarien hilfreich sein:

- Wenn der virtuelle Computer nicht häufig verwendet wird, können Sie ihn auf eine kleinere Größe skalieren, um die monatlichen Kosten zu senken.
- Wenn auf dem virtuellen Computer eine Lastspitze auftritt, kann seine Größe geändert werden, um die Kapazität zu erhöhen.

Nachfolgend werden die Schritte aufgelistet, die hierzu erforderlich sind:

1. Einrichten von Azure Automation für den Zugriff auf Ihre virtuellen Computer
2. Importieren der Azure Automation-Runbooks für die vertikale Skalierung in Ihr Abonnement
3. Hinzufügen eines Webhooks zu Ihrem Runbook
4. Hinzufügen einer Warnung zu Ihrem virtuellen Computer

> [AZURE.NOTE] Die Größen, auf die der virtuelle Computer skaliert werden kann, hängen einerseits von der Größe des ersten virtuellen Computers sowie von der Verfügbarkeit anderer Größen im Cluster ab, in dem der virtuelle Computer bereitgestellt wurde. In den veröffentlichten Automation-Runbooks, die in diesem Artikel verwendet werden, wird dies berücksichtigt und lediglich eine Skalierung innerhalb der nachstehenden VM-Größenpaare durchgeführt. Dies bedeutet, dass ein virtueller Standard\_D1v2-Computer nicht plötzlich auf Standard\_G5 hochskaliert oder auf Basic\_A0 herunterskaliert werden kann.

>| VM-Größenpaare für die Skalierung | |
|---|---|
| Basic\_A0 | Basic\_A4 |
| Standard\_A0 | Standard\_A4 |
| Standard\_A5 | Standard\_A7 |
| Standard\_A8 | Standard\_A9 |
| Standard\_A10 | Standard\_A11 |
| Standard\_D1 | Standard\_D4 |
| Standard\_D11 | Standard\_D14 |
| Standard\_DS1 | Standard\_DS4 |
| Standard\_DS11 | Standard\_DS14 |
| Standard\_D1v2 | Standard\_D5v2 |
| Standard\_D11v2 | Standard\_D14v2 |
| Standard\_G1 | Standard\_G5 |
| Standard\_GS1 | Standard\_GS5 |

## Einrichten von Azure Automation für den Zugriff auf Ihre virtuellen Computer

Als Erstes müssen Sie lediglich ein Azure Automation-Konto erstellen, in dem die Runbooks gehostet werden, mit denen die Instanzen der VM-Skalierungsgruppen skaliert werden. Vor Kurzem wurde für den Automation-Dienst die Funktion des ausführenden Kontos eingeführt, wodurch die Einrichtung des Dienstprinzipals für die automatische Ausführung der Runbooks im Auftrag des Benutzers stark vereinfacht wird. Weitere Informationen darüber finden Sie im unten stehenden Artikel:

* [Authentifizieren von Runbooks mit der Azure-Option „Ausführendes Konto“](../automation/automation-sec-configure-azure-runas-account.md)

## Importieren der Azure Automation-Runbooks für die vertikale Skalierung in Ihr Abonnement

Die Runbooks, die zur vertikalen Skalierung Ihres virtuellen Computers benötigt werden, sind bereits im Azure Automation-Runbookkatalog veröffentlicht. Sie müssen diese in Ihr Abonnement importieren. Informationen dazu, wie Sie Runbooks importieren, finden Sie im folgenden Artikel:

* [Runbook und Modulkataloge für Azure Automation](../automation/automation-runbook-gallery.md)

Die nachfolgende Abbildung zeigt, welche Runbooks importiert werden müssen:

![Importieren von Runbooks](./media/virtual-machines-vertical-scaling-automation/scale-runbooks.png)

## Hinzufügen eines Webhooks zu Ihrem Runbook

Nachdem Sie die Runbooks importiert haben, müssen Sie einen Webhook zum Runbook hinzufügen, damit es über eine Warnung von einem virtuellen Computer ausgelöst werden kann. Ausführliche Informationen zum Erstellen eines Webhooks für Ihr Runbook finden Sie hier:

* [Azure Automation-Webhooks](../automation/automation-webhooks.md)

Stellen Sie sicher, dass Sie den Webhook kopieren, bevor Sie das Dialogfeld für Webhooks schließen, da sie den Webhook im nächsten Abschnitt benötigen.

## Hinzufügen einer Warnung zu Ihrem virtuellen Computer

1. Wählen Sie die VM-Einstellungen aus.
2. Wählen Sie „Warnungsregeln“ aus.
3. Wählen Sie „Warnung hinzufügen“ aus.
4. Wählen Sie die Metrik aus, bei der die Warnung ausgelöst wird.
5. Wählen Sie die Bedingung aus, bei deren Erfüllung die Warnung ausgelöst wird.
6. Wählen Sie einen Schwellenwert aus, bei dem die Bedingung aus Schritt 5 erfüllt ist.
7. Wählen Sie den Zeitraum aus, in dem der Überwachungsdienst auf die Bedingung und die Schwellenwerte aus den Schritten 5 und 6 prüft.
8. Fügen Sie den Webhook ein, den Sie im vorherigen Abschnitt kopiert haben.

![Hinzufügen einer Warnung zu VM 1](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-1.png)

![Hinzufügen einer Warnung zu VM 2](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-2.png)

<!---HONumber=AcomDC_0420_2016-->