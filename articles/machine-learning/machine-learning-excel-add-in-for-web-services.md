<properties
	pageTitle="Excel-Add-In für Machine Learning-Webdienste | Microsoft Azure"
	description="Hier wird erläutert, wie Sie Azure Machine Learning-Webdienste direkt in Excel ohne Erstellung von Code verwenden."
	services="machine-learning"
	documentationCenter=""
	authors="tedway"
	manager="paulettm"
	editor="cgronlun"
    tags=""/>

<tags
	ms.service="machine-learning"
    ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="data-services"
	ms.date="07/06/2016"
	ms.author="tedway;garye" />

# Excel-Add-In für Azure Machine Learning-Webdienste

Mit Excel können Webdienste auf einfache Weise direkt aufgerufen werden, ohne dafür Code erstellen zu müssen.

## Schritte zum Verwenden eines vorhandenen Webdiensts in eine Arbeitsmappe

1. Öffnen Sie die [Excel-Beispieldatei](http://aka.ms/amlexcel-sample-2), die das Excel-Add-In und Daten zu den Passagieren auf der Titanic enthält.
2. Wählen Sie den Webdienst aus, indem Sie darauf klicken – in diesem Beispiel ist das „Titanic Survivor Predictor (Excel-Add-In-Beispiel) [Ergebnis]“.

    ![Webdienst auswählen][01]

3. Dadurch gelangen Sie zum Abschnitt **Vorhersagen**. Diese Arbeitsmappe enthält bereits Beispieldaten. In einer leeren Arbeitsmappe können Sie jedoch auch eine Zelle in Excel auswählen und auf **Use sample data** klicken.
4. Wählen Sie die Daten mit Überschriften aus, und klicken Sie auf das Symbol für den Eingabedatenbereich. Stellen Sie sicher, dass das Kontrollkästchen „Daten haben Überschriften“ aktiviert ist.
5. Geben Sie unter **Ausgabe** die Zellennummer ein, in der die Ausgabe erfolgen soll. In diesem Beispiel ist das „H1“.
6. Klicken Sie auf **Vorhersagen**.

	![Abschnitt „Predict“][02]

## Schritte zum Hinzufügen eines neuen Webdiensts

Veröffentlichen Sie einen Webdienst (auf [dieser Seite](machine-learning-walkthrough-5-publish-web-service.md) erfahren Sie, wie das geht), oder verwenden Sie einen vorhandenen Webdienst.

Rufen Sie den API-Schlüssel Ihres Webdiensts ab. Wo dies erfolgt, hängt davon ab, ob Sie einen klassischen oder neuen Webdienst veröffentlicht haben.

**Klassischer Webdienst**

1. Klicken Sie in Machine Learning Studio im linken Bereich auf den Abschnitt **WEB SERVICES**, und wählen Sie den Webdienst aus.

	![Studio – Webdienst auswählen][04]

2. Kopieren Sie den API-Schlüssel für den Webdienst.

	![Studio-API-Schlüssel][05]

3. Klicken Sie auf der Registerkarte **DASHBOARD** für den Webdienst auf die Verknüpfung **REQUEST/RESPONSE**.
4. Wechseln Sie zum Abschnitt **Request URI**. Kopieren und speichern Sie die URL.

**Neuer Webdienst**

1. Klicken Sie im Azure Machine Learning Web Services-Portal auf **WEB SERVICES**, und wählen Sie Ihren Webdienst aus.
2. Klicken Sie auf **Consume**.
3. Wechseln Sie zum Abschnitt **Basic consumption info**. Kopieren und speichern Sie den **Primary Key** und die URL für **Request-Response**.


## Schritte zum Hinzufügen eines neuen Webdiensts

1. Veröffentlichen Sie einen Webdienst (auf [dieser Seite](machine-learning-walkthrough-5-publish-web-service.md) erfahren Sie, wie das geht), oder verwenden Sie einen vorhandenen Webdienst.
2. Navigieren Sie in Excel zum Abschnitt **Webdienste** (wenn Sie sich im Abschnitt **Vorhersagen** befinden, und klicken Sie auf den Zurück-Pfeil, um zur Liste der Webdienste zu gelangen).

	![Webdienstauswahl aufrufen][03]
3. Klicken Sie auf **Webdienst hinzufügen**.
4. Fügen Sie die URL in das Textfeld des Excel-Add-Ins mit der Beschriftung **URL** ein.
5. Fügen Sie die API/den Primärschlüssel in das Textfeld mit der Beschriftung **API key** ein.
6. Klicken Sie auf **Hinzufügen**.

	![URL und API-Schlüssel für einen klassischen Webdienst.][06]

10.	Folgen Sie den Anweisungen „Schritte zum Verwenden eines vorhandenen Webdiensts“ oben, um den Webdienst zu verwenden.

## Freigeben Ihrer Arbeitsmappe

Wenn Sie Ihre Arbeitsmappe speichern, werden auch die API/der Primärschlüssel der hinzugefügten Webdienste gespeichert. Das bedeutet, dass Sie die Arbeitsmappe nur für Personen freigeben sollten, denen Sie vertrauen.

Weiter unten oder in unserem [Forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409) können Sie Fragen stellen.

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png

<!---HONumber=AcomDC_0720_2016-->