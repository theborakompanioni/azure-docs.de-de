<properties
	pageTitle="Verwalten eines neuen Machine Learning-Webdiensts | Microsoft Azure"
	description="Verwalten des Zugriffs auf Azure Machine Learning-Arbeitsbereiche sowie Bereitstellen und Verwalten von ML-API-Webdiensten"
	services="machine-learning"
	documentationCenter=""
	authors="vDonGlover"
	manager="jhubbard"
	editor="cgronlun"/>

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/06/2016"
	ms.author="v-donglo"/>


# Verwalten eines neuen Machine Learning-Webdiensts

> [AZURE.NOTE] Die Verfahren in diesem Artikel sind für neue Azure Machine Learning-Webdienste relevant. Informationen zum Verwalten klassischer Webdienste finden Sie unter [Verwalten eines Azure Machine Learning-Arbeitsbereichs](machine-learning-manage-workspace.md).

Im Microsoft Azure Machine Learning Web Services-Portal können Sie Ihre neuen Machine Learning-Webdienste verwalten. Ihre Möglichkeiten:

- Überwachen der Nutzung des Webdiensts
- Konfigurieren der Beschreibung, Aktualisieren der Schlüssel für den Webdienst und des Schlüssel Ihres Speicherkontos und Aktivieren oder Deaktivieren von Beispieldaten
- Löschen des Webdiensts
- Erstellen, Löschen und Aktualisieren von Abrechnungsplänen

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> [AZURE.TIP] In Azure Machine Learning Studio können Sie auf der Registerkarte **WEBDIENSTE** Machine-Learning-Webdienste hinzufügen, aktualisieren oder löschen.

So verwalten Sie Ihre Webdienste

1.	Melden Sie sich mit Ihrem Microsoft Azure-Konto beim [Microsoft Azure Machine Learning Web Service-Portal](https://services.azureml.net/quickstart) an. Verwenden Sie hierzu das Konto, das dem Azure-Abonnement zugeordnet ist.
2.	Klicken Sie im Menü auf **Web Services**.

Eine Liste der für Ihr Abonnement bereitgestellten Webdienste wird angezeigt. Um einen Webdienst zu verwalten, klicken Sie auf den Namen in der Liste, um die Webdienstseite zu öffnen.

Auf der Seite „Web Services“ können Sie folgende Aktionen ausführen:

- Auf den Webdienst klicken, um ihn zu verwalten
- Auf den Abrechnungsplan des Webdiensts klicken, um ihn zu aktualisieren
- einen Webdienst löschen
- einen Webdienst in eine andere Region kopieren
- einen Webdienst kopieren und in einer anderen Region bereitstellen

Wenn Sie auf einen Webdienst klicken, wird die Seite „Quickstart“ des Webdiensts geöffnet. Die Seite „Quickstart“ des Webdienst bietet zwei Menüoption, die Ihnen das Verwalten des Webdiensts ermöglichen:

- **DASHBOARD**: Hier können Sie die Nutzung des Webdiensts anzeigen.
- **CONFIGURE**: Ermöglicht das Hinzufügen von beschreibendem Text, das Ein- und Ausschalten der Fehlerprotokollierung, das Aktualisieren des Schlüssels des Speicherkontos, das dem Webdienst zugeordnet ist, sowie das Aktivieren und Deaktivieren von Beispieldaten.

## Überwachen der Nutzung des Webdiensts

Klicken Sie auf die Registerkarte **DASHBOARD**.

Im Dashboard können Sie die Gesamtnutzung des Webdiensts über einen Zeitraum anzeigen. In der Dropdownliste „Period“ rechts oben in den Nutzungsdiagrammen können Sie den Zeitraum auswählen. Im Dashboard werden die folgenden Informationen angezeigt:

- **Requests Over Time** zeigt ein Diagramm der Anzahl der Anforderungen im ausgewählten Zeitraum an. Es dient zum Bestimmen von Nutzungsspitzen.
- **Request-Response Requests** zeigt die Gesamtanzahl der „Request-Response“-Aufrufe, die der Dienst im ausgewählten Zeitraum empfangen hat, und die Anzahl der Fehler an.
- **Average Request-Response Compute Time** zeigt die Durchschnittszeit an, die zum Ausführen der empfangenen Anforderungen benötigt wurde.
- **Batch Requests** zeigt die Gesamtanzahl der Batchanforderungen, die der Dienst im ausgewählten Zeitraum empfangen hat, und die Anzahl der Fehler an.
- **Average Job Latency** zeigt die Durchschnittszeit an, die zum Ausführen der empfangenen Anforderungen benötigt wurde.
- **Errors** zeigt die kumulierte Anzahl der Fehler, die bei Aufrufen des Webdiensts aufgetreten sind.
- **Services Costs** zeigt die Kosten für den Abrechnungsplan, der dem Dienst zugeordnet ist.

## Konfigurieren des Webdiensts ##

Klicken Sie auf die Menüoption **CONFIGURE**.

Sie können die folgenden Eigenschaften aktualisieren:

* In **Description** können Sie eine Beschreibung des Webdiensts eingeben.
* In **Title** können Sie einen Titel für den Webdienst eingeben.
* In **Keys** können Sie den primären und die sekundären API-Schlüssel rotieren lassen.
* In **Storage account key** können Sie den Schlüssel des Speicherkontos aktualisieren, das dem Webdienst zugeordnet ist.
* **Enable Sample data** ermöglicht das Bereitstellen von Beispieldaten, die Sie mit Ihrem Request-Response-Dienst testen können. Wenn Sie den Webdienst in Machine Learning Studio erstellt haben, stammen die Beispieldaten aus den Daten, mit denen Sie Ihr Modell trainiert haben. Wenn Sie den Dienst programmgesteuert erstellt haben, stammen die Daten aus den Beispieldaten, die Sie als Teil des JSON-Pakets bereitgestellt haben.

## Verwalten von Abrechnungsplänen

Klicken Sie auf der Seite „Quickstart“ des Webdiensts auf die Menüoption **Plans**. Sie können auch auf den Plan klicken, der einem bestimmten Webdienst zugeordnet ist, um diesen Plan zu verwalten.

* **New** ermöglicht das Erstellen eines neuen Plans.
* Über **Add/Remove Plan instance** können Sie einen vorhandenen Plan horizontal hochskalieren, um Kapazität hinzuzufügen.
* Über **Upgrade/Downgrade** können Sie einen vorhandenen Plan zentral hochskalieren, um Kapazität hinzuzufügen.
* **Delete** ermöglicht das Löschen eines Plans.

Klicken Sie auf einen Plan, um dessen Dashboard anzuzeigen. Das Dashboard zeigt eine Momentaufnahme der Plannutzung in einem ausgewählten Zeitraum. Klicken Sie rechts oben in den Dashboardinformationen auf die Dropdownliste **Period**, um den Zeitraum auszuwählen.

Das Dashboard des Plans enthält die folgenden Informationen:

* **Plan Description** zeigt Informationen zu den Kosten und zur Kapazität des Plans.
* **Plan Usage** zeigt die Anzahl der Transaktionen und Computestunden, die für den Plan in Rechnung gestellt wurden.
* **Web Services** zeigt die Anzahl der Webdienste, die den Plan verwenden.
* **Top Web Service By Calls** zeigt die vier Webdienste, die die meisten Aufrufe vornehmen, die für den Plan in Rechnung gestellt werden.
* **Top Web Services by Compute Hrs** zeigt die vier Webdienste, die die meisten Computeressourcen nutzen, die für den Plan in Rechnung gestellt werden.

<!---HONumber=AcomDC_0914_2016-->