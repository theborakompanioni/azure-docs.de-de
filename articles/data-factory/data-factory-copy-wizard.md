<properties 
	pageTitle="Assistent zum Kopieren in Data Factory | Microsoft Azure" 
	description="Erfahren Sie, wie Sie den Assistenten zum Kopieren in Data Factory verwenden, um Daten aus unterstützten Datenquellen in Senken zu kopieren." 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/27/2016" 
	ms.author="spelluru"/>

# Assistent zum Kopieren in Data Factory
Der **Assistent zum Kopieren in Data Factory** ermöglicht das Erstellen einer Pipeline, mit der Sie Daten aus unterstützten Quellen an Ziele kopieren können, ohne JSON-Definitionen für verknüpfte Dienste, Datasets und Pipelines zu schreiben. Klicken Sie zum Starten des Assistenten zum Kopieren auf der Startseite Ihrer Data Factory auf die Kachel **Daten kopieren**.

![Assistent zum Kopieren von Daten](./media/data-factory-copy-wizard/copy-data-wizard.png)

## Features

### Ein intuitiver und unkomplizierter Assistent zum Kopieren von Daten 
Mit diesem Assistenten können Sie problemlos innerhalb weniger Minuten Daten von einer Quelle zu einem Ziel verschieben. Führen Sie dazu die folgenden einfachen Schritte aus:

1.	Wählen Sie die **Quelle** aus.
2.	Wählen Sie das **Ziel** aus.
3.	Konfigurieren Sie die **Einstellungen**.

![Auswählen einer Datenquelle](./media/data-factory-copy-wizard/select-data-source-page.png)

### Umfangreiche Datensuche und Schemazuordnungen
Sie können innerhalb des Assistenten Tabellen/Ordner durchsuchen, eine Datenvorschau anzeigen, Schemata zuordnen, Ausdrücke überprüfen und einfache Datentransformationen durchführen.

**Tabellen/Ordner durchsuchen** ![Durchsuchen von Tabellen und Ordnern](./media/data-factory-copy-wizard/browse-tables-folders.png)

### Skalierbare Funktionalität für unterschiedliche Daten- und Objekttypen
Das Szenario ist von Beginn an für Big Data ausgelegt. Es ist einfach und effizient, Data Factory-Pipelines zu erstellen, die hunderte von Ordnern, Dateien oder Tabellen verschieben können.

**Datenvorschau anzeigen, Schema zuordnen und einfache Transformationen durchführen** ![Dateiformateinstellungen](./media/data-factory-copy-wizard/file-format-settings.png) ![Schemazuordnung](./media/data-factory-copy-wizard/schema-mapping.png) ![Überprüfen von Ausdrücken](./media/data-factory-copy-wizard/validate-expressions.png)

### Skalierbare Funktionalität für unterschiedliche Daten- und Objekttypen
Das Szenario ist von Beginn an für Big Data ausgelegt. Mithilfe des Assistenten zum Kopieren ist es einfach und effizient, hunderte von Ordnern, Dateien oder Tabellen zu verschieben.

![Auswählen von Tabellen zum Kopieren von Daten](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

### Umfangreichere Planungsoptionen
Sie können den Kopiervorgang nur einmal oder nach einem Zeitplan (stündlich, täglich etc.) ausführen. Beide Optionen können für die verschiedensten Connectors über die lokale Kopie, Cloudkopie und die Kopie auf dem lokalen Desktop hinweg verwendet werden. Das einmalige Kopieren aktiviert eine einmalige Datenverschiebung von einer Quelle zu einem Ziel und wird auf Daten jeglicher Größe und jeglichen unterstützten Formats angewandt. Das Kopieren nach einem Zeitplan aktiviert das Kopieren von Daten nach einem vorgeschriebenen Wiederholungszyklus. Sie können umfangreiche Einstellungen (wie Wiederholen, Timeout, Warnungen etc.) nutzen, um das Kopieren nach einem Zeitplan zu konfigurieren.

![Planen von Eigenschaften](./media/data-factory-copy-wizard/scheduling-properties.png)


## Lernprogramm:
Eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mit einer Kopieraktivität unter Verwendung des **Assistenten zum Kopieren in Data Factory** finden Sie unter [Tutorial: Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md).


## Variablen im Azure Blob-Ordnerpfad
Sie können Variablen im Ordnerpfad verwenden, um Daten aus einem Ordner zu kopieren, der zur Laufzeit auf der Grundlage der [WindowStart-Systemvariablen](data-factory-functions-variables.md#data-factory-system-variables) bestimmt wird. Folgende Variablen werden unterstützt: **year**, **month**, **day**, **hour**, **minute** und **{custom}**. Beispiel: Eingabeordner/{year}/{month}/{day}.

Angenommen, Sie haben Eingabeordner im folgenden Format:
	
	2016/03/01/01
	2016/03/01/02
	2016/03/01/03
	...

Klicken Sie auf die Schaltfläche **Durchsuchen** für **die Datei oder den Ordner**, navigieren Sie zu einem der Ordner (Beispiel: 2016>03>01>02), und klicken Sie auf **Auswählen**. Das Textfeld sollte jetzt **2016/03/01/02** enthalten. Ersetzen Sie nun **2016** durch **{year}**, **03** durch **{month}**, **01** durch **{day}**, **02** durch **{hour}**, und drücken Sie die **TAB-TASTE**. Daraufhin sollten wie nachfolgend dargestellt Dropdownlisten zum Auswählen des **Formats** für diese vier Variablen angezeigt werden:

![Verwenden von Systemvariablen](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)

Sie können auch, wie nachfolgend dargestellt, eine **benutzerdefinierte** Variable und beliebige [unterstützte Formatzeichenfolgen](https://msdn.microsoft.com/library/8kb3ddd4.aspx) verwenden. Wählen Sie einen Ordner mit dieser Struktur aus, indem Sie zunächst die Schaltfläche „Durchsuchen“ verwenden. Ersetzen Sie einen Wert durch **{custom}**, und drücken Sie dann die **TAB-TASTE**, um das Textfeld anzuzeigen, in das Sie die Formatzeichenfolge eingeben können.

![Verwenden von benutzerdefinierten Variablen](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## Nächste Schritte
- [Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe des Data Factory-Kopier-Assistenten](data-factory-copy-data-wizard-tutorial.md)

<!---HONumber=AcomDC_0810_2016-->