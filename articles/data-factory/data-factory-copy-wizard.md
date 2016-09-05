<properties
	pageTitle="Assistent zum Kopieren in Data Factory | Microsoft Azure"
	description="Erfahren Sie, wie Sie den Data Factory-Kopier-Assistenten verwenden, um Daten aus unterstützten Datenquellen in Senken zu kopieren."
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
Der Azure Data Factory-Kopier-Assistent ermöglicht das Erstellen einer Pipeline, mit der Sie Daten aus unterstützten Quellen an Ziele kopieren können, ohne JSON-Definitionen für verknüpfte Dienste, Datasets und Pipelines zu schreiben. Klicken Sie zum Starten des Kopier-Assistenten auf der Startseite Ihrer Data Factory auf die Kachel **Daten kopieren**.

![Kopier-Assistent](./media/data-factory-copy-wizard/copy-data-wizard.png)

## Features

### Ein intuitiver und unkomplizierter Assistent zum Kopieren von Daten
Mit diesem Assistenten können Sie problemlos innerhalb weniger Minuten Daten von einer Quelle zu einem Ziel verschieben. Führen Sie dazu die folgenden einfachen Schritte aus:

1.	Wählen Sie die Quelle aus.
2.	Wählen Sie das Ziel aus.
3.	Konfigurieren Sie die Einstellungen.

![Auswählen einer Datenquelle](./media/data-factory-copy-wizard/select-data-source-page.png)

### Umfangreiche Datensuche und Schemazuordnungen
Sie können innerhalb des Assistenten Tabellen und Ordner durchsuchen, eine Datenvorschau anzeigen, Schemata zuordnen, Ausdrücke überprüfen und einfache Datentransformationen durchführen.

**Tabellen/Ordner durchsuchen** ![Durchsuchen von Tabellen und Ordnern](./media/data-factory-copy-wizard/browse-tables-folders.png)

### Skalierbare Funktionalität für unterschiedliche Daten- und Objekttypen
Das Szenario ist von Beginn an für Big Data ausgelegt. Es ist einfach und effizient, Data Factory-Pipelines zu erstellen, die Hunderte von Ordnern, Dateien oder Tabellen verschieben können.

**Datenvorschau** ![Dateiformateinstellungen](./media/data-factory-copy-wizard/file-format-settings.png)

**Zuordnen eines Schemas** ![Schemazuordnung](./media/data-factory-copy-wizard/schema-mapping.png)

**Ausführen einfacher Transformationen** ![Überprüfen von Ausdrücken](./media/data-factory-copy-wizard/validate-expressions.png)

### Skalierbare Funktionalität für unterschiedliche Daten- und Objekttypen
Indem Sie den Kopier-Assistenten verwenden, können Sie Hunderte von Ordner, Dateien oder Tabellen einfach und effizient verschieben.

![Wählen Sie die Tabellen, aus denen Daten kopiert werden sollen.](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

### Umfangreichere Planungsoptionen
Sie können den Kopiervorgang einmal oder nach einem Zeitplan (stündlich, täglich usw.) ausführen. Beide Optionen können für die verschiedensten Connectors für die lokale Kopie, Cloudkopie und die Kopie auf dem lokalen Desktop verwendet werden.

Ein einmaliger Kopiervorgang ermöglicht nur einmal das Verschieben von Daten aus einer Quelle in ein Ziel. Er gilt für Daten jeder Größe in jedem unterstützten Format. Das Kopieren nach einem Zeitplan ermöglicht Ihnen das Kopieren von Daten nach einem vorgeschriebenen Wiederholungszyklus. Sie können umfangreiche Einstellungen (wie Wiederholen, Timeout, Warnungen usw.) nutzen, um das Kopieren nach einem Zeitplan zu konfigurieren.

![Planen von Eigenschaften](./media/data-factory-copy-wizard/scheduling-properties.png)

## Variablen im Azure-Blobordnerpfad
Sie können Variablen im Ordnerpfad verwenden, um Daten aus einem Ordner zu kopieren, der zur Laufzeit auf Grundlage der [WindowStart-Systemvariablen](data-factory-functions-variables.md#data-factory-system-variables) bestimmt wird. Folgende Variablen werden unterstützt: **{year}**, **{month}**, **{day}**, **{hour}**, **{minute}** und **{custom}**. Beispiel: Eingabeordner/{year}/{month}/{day}.

Angenommen, Sie haben Eingabeordner im folgenden Format:

	2016/03/01/01
	2016/03/01/02
	2016/03/01/03
	...

Klicken Sie auf die Schaltfläche **Durchsuchen** für **Datei oder Ordner**, navigieren Sie zu einem der Ordner (z.B. 2016->03->01->02), und klicken Sie auf **Auswählen**. Das Textfeld sollte jetzt **2016/03/01/02** enthalten. Ersetzen Sie nun **2016** durch **{year}**, **03** durch **{month}**, **01** durch **{day}**, **02** durch **{hour}**, und drücken Sie die TAB-Taste. Daraufhin werden Dropdownlisten zum Auswählen des Formats für diese vier Variablen angezeigt:

![Verwenden von Systemvariablen](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)

Sie können auch eine **benutzerdefinierte** Variable und beliebige [unterstützte Formatzeichenfolgen](https://msdn.microsoft.com/library/8kb3ddd4.aspx) verwenden (siehe Screenshot unten). Um einen Ordner mit dieser Struktur auszuwählen, verwenden Sie zunächst die Schaltfläche **Durchsuchen**. Ersetzen Sie dann einen Wert mit **{custom}**, und betätigen Sie die TAB-Taste, um das Textfeld anzuzeigen, in dem Sie die Formatzeichenfolge eingeben können.

![Verwenden von benutzerdefinierten Variablen](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## Nächste Schritte
Eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mit einer Kopieraktivität unter Verwendung des Data Factory-Kopier-Assistenten finden Sie im [Tutorial: Erstellen einer Pipeline mithilfe des Kopier-Assistenten](data-factory-copy-data-wizard-tutorial.md).

<!---HONumber=AcomDC_0824_2016-->