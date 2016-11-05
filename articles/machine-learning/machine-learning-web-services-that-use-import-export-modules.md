---
title: Bereitstellen von Azure ML-Webdiensten, die Module zum Importieren und Exportieren von Daten verwenden | Microsoft Docs
description: Erfahren Sie, wie Sie die Module „Import Data“ und „Export Data“ verwenden, um Daten von einem Webdienst zu senden und zu empfangen.
services: machine-learning
documentationcenter: ''
author: vDonGlover
manager: raymondlaghaeian
editor: ''

ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2016
ms.author: v-donglo

---
# Bereitstellen von Azure ML-Webdiensten, die Module zum Importieren und Exportieren von Daten verwenden
Bei der Erstellung eines Vorhersageexperiments fügen Sie in der Regel eine Webdiensteingabe und -ausgabe hinzu. Wenn Sie das Experiment bereitstellen, können Nutzer Daten über die Webdiensteingabe bzw. -ausgabe senden und empfangen. Für einige Anwendungen sind die Daten eines Nutzers möglicherweise über einen Datenfeed verfügbar oder befinden sich bereits in einer externen Datenquelle, z.B. Azure-Blobspeicher. In diesen Fällen müssen sie nicht mithilfe von Webdiensteingabe und -ausgabe gelesen und geschrieben werden. Sie können stattdessen mit dem Stapelausführungsdienst (Batch Execution Service, BES) mit einem Import Data-Modul aus der Datenquelle gelesen werden, und die Bewertungsergebnisse können mit einem Export Data-Modul an einen anderen Datenspeicherort geschrieben werden.

Import Data-Modul und Export Data-Modul können aus einer Reihe von Datenspeicherorten wie Web-URL über HTTP, Hive-Abfrage, einer Azure SQL-Datenbank, Azure-Tabellenspeicher und Azure-Blobspeicher, einem bereitgestellten Datenfeed oder einer lokalen SQL-Datenbank lesen und darin schreiben.

In diesem Thema wird das Beispiel „Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset“ (Beispiel 5: Trainieren, Testen, Evaluieren für binäre Klassifizierung: Adult-Dataset) verwendet und vorausgesetzt, dass das Dataset bereits in eine Azure SQL-Tabelle mit dem Namen „censusdata“ geladen wurde.

## Erstellen des Trainingsexperiments
Beim Öffnen von „Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset“ wird das Beispieldataset „Adult Census Income Binary Classification“ verwendet. Dabei ähnelt das Experiment im Experimentbereich der folgenden Abbildung.

![Erstkonfiguration des Experiments.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)

So lesen Sie die Daten aus der Azure SQL-Tabelle:

1. Löschen Sie das Datasetmodul.
2. Geben Sie im Komponentensuchfeld „import“ ein.
3. Fügen Sie aus der Ergebnisliste ein Modul *Import Data* im Experimentbereich hinzu.
4. Verbinden Sie die Ausgabe des Moduls *Import Data* mit der Eingabe des Moduls *Clean Missing Data*.
5. Wählen Sie im Bereich „Properties“ **Azure SQL Database** in der Dropdownliste **Data Source** aus.
6. Geben Sie in den Feldern **Database server name**, **Database name**, **User name** und **Password** die entsprechenden Informationen für Ihre Datenbank ein.
7. Geben Sie im Abfragefeld „Database“ die folgende Abfrage ein.
   
     select [age],
   
        [workclass],
        [fnlwgt],
        [education],
        [education-num],
        [marital-status],
        [occupation],
        [relationship],
        [race],
        [sex],
        [capital-gain],
        [capital-loss],
        [hours-per-week],
        [native-country],
        [income]
     from dbo.censusdata;
8. Klicken Sie am unteren Rand des Experimentbereichs auf **Run**.

## Erstellen des Vorhersageexperiments
Als Nächstes richten Sie das Vorhersageexperiment ein, von dem aus Sie Ihren Webdienst bereitstellen.

1. Klicken Sie am unteren Rand des Experimentbereichs auf **Set Up Web Service**, und wählen Sie **Predictive Web Service [Recommended]**.
2. Entfernen Sie die Module *Web Service Input* und *Web Service Output* aus dem Vorhersageexperiment.
3. Geben Sie im Komponentensuchfeld „export“ ein.
4. Fügen Sie aus der Ergebnisliste ein Modul *Export Data* im Experimentbereich hinzu.
5. Verbinden Sie die Ausgabe des Moduls *Score Model* mit der Eingabe des Moduls *Export Data*.
6. Wählen Sie im Bereich „Properties“ **Azure SQL Database** in der Datenziel-Dropdownliste aus.
7. Geben Sie in den Feldern **Database server name**, **Database name**, **Server user account name** und **Server user account password** die entsprechenden Informationen für Ihre Datenbank ein.
8. Geben Sie in das Feld **Comma separated list of columns to be saved** „Scored Labels“ ein.
9. Geben Sie in das Feld **Data table name** „dbo.ScoredLabels“ ein. Wenn die Tabelle nicht vorhanden ist, wird sie erstellt, wenn das Experiment ausgeführt oder der Webdienst aufgerufen wird.
10. Geben Sie in das Feld **Comma separated list of datatable columns** „Scored Labels“ ein.

Wenn Sie eine Anwendung schreiben, die den endgültigen Webdienst aufruft, möchten Sie vielleicht zur Laufzeit eine andere Eingabeabfrage oder Zieltabelle angeben. Um diese Eingaben und Ausgaben zu konfigurieren, können Sie mit dem Feature „Web Service Parameters“ das Modul *Import Data*, die Eigenschaft *Data source* und die Modusdatenziel-Eigenschaft *Export Data* festlegen. Weitere Informationen zu „Web Service Parameters“ finden Sie unter dem Eintrag [AzureML Web Service Parameters](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) im „Cortana Intelligence and Machine Learning Blog“.

So konfigurieren Sie die Webdienstparameter für die Importabfrage und die Zieltabelle:

1. Klicken Sie im Eigenschaftenbereich für das *Import Data*-Modul auf das Symbol des Felds **Database query** oben rechts, und wählen Sie **Set as web service parameter**.
2. Klicken Sie im Eigenschaftenbereich für das *Export Data*-Modul auf das Symbol des Felds **Data table name** oben rechts, und wählen Sie **Set as web service parameter**.
3. Klicken Sie am unteren Rand des Eigenschaftenbereichs für das *Export Data*-Modul im Abschnitt **Web Service Parameters** auf „Database query“, und benennen Sie sie in „Query“ um.
4. Klicken Sie auf **Data table name**, und benennen Sie die Tabelle in **Table** um.

Wenn Sie fertig sind, sollte das Experiment der folgenden Abbildung ähneln.

![Endgültige Form des Experiments.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

Jetzt können Sie das Experiment als Webdienst bereitstellen.

## Bereitstellen des Webdiensts
Sie können entweder einen klassischen oder einen neuen Webdienst bereitstellen.

### Bereitstellen eines klassischen Webdiensts
So stellen Sie einen klassischen Webdienst bereit und erstellen eine Anwendung, die ihn verwendet:

1. Klicken Sie am unteren Rand des Experimentbereichs auf „Run“.
2. Wenn die Ausführung abgeschlossen ist, klicken Sie auf **Deploy Web Service** und wählen **Deploy Web Service [Classic]**.
3. Suchen Sie Ihren API-Schlüssel auf dem Webdienst-Dashboard. Kopieren Sie ihn und speichern Sie ihn zur späteren Verwendung.
4. Klicken Sie in der Tabelle **Default Endpoint** auf den Link **Batch Execution**, um die API-Hilfeseite zu öffnen.
5. Erstellen Sie in Visual Studio eine C#-Konsolenanwendung.
6. Am unteren Rand der API-Hilfeseite finden Sie den Abschnitt **Sample Code**.
7. Kopieren Sie den C#-Beispielcode, fügen Sie ihn in die Datei „Program.cs“ ein, und entfernen Sie alle Verweise auf den Blobspeicher.
8. Aktualisieren Sie den Wert der Variablen *apiKey* mit dem zuvor gespeicherten API-Schlüssel.
9. Suchen Sie die Anforderungsdeklaration, und aktualisieren Sie die Werte der Webdienstparameter, die den Modulen *Import Data* und *Export Data* übergeben werden. In diesem Fall verwenden Sie die ursprüngliche Abfrage, definieren aber einen neuen Tabellennamen.
   
     var request = new BatchExecutionRequest() 
     {    
   
         GlobalParameters = new Dictionary<string, string>() {
         { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
         { "Table", "dbo.ScoredTable2" },
         }
     };
10. Führen Sie die Anwendung aus.

Nach Abschluss der Ausführung wird der Datenbank eine neue Tabelle mit den Bewertungsergebnissen hinzugefügt.

### Bereitstellen eines neuen Webdiensts
So stellen Sie einen neuen Webdienst bereit und erstellen eine Anwendung, die ihn verwendet:

1. Klicken Sie am unteren Rand des Experimentbereichs auf **Run**.
2. Wenn die Ausführung abgeschlossen ist, klicken Sie auf **Deploy Web Service** und wählen **Deploy Web Service [New]**.
3. Geben Sie auf der Seite „Deploy Experiment“ einen Namen für den Webdienst ein, wählen Sie einen Tarif aus, und klicken Sie auf **Deploy**.
4. Klicken Sie auf der Seite **Quickstart** auf **Consume**.
5. Klicken Sie im Abschnitt **Sample Code** auf **Batch**.
6. Erstellen Sie in Visual Studio eine C#-Konsolenanwendung.
7. Kopieren Sie den C#-Beispielcode, und fügen Sie ihn in die Datei „Program.cs“ ein.
8. Aktualisieren Sie den Wert der Variablen *apiKey* mit dem **Primary Key** im Abschnitt **Basic consumption info**.
9. Suchen Sie die Deklaration *scoreRequest*, und aktualisieren Sie die Werte der Webdienstparameter, die den Modulen *Import Data* und *Export Data* übergeben werden. In diesem Fall verwenden Sie die ursprüngliche Abfrage, definieren aber einen neuen Tabellennamen.
   
     var scoreRequest = new
     {
   
         Inputs = new Dictionary<string, StringTable>()
         {
         },
         GlobalParameters = new Dictionary<string, string>() {
              { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
             { "Table", "dbo.ScoredTable3" },
         }
     };
10. Führen Sie die Anwendung aus.

<!---HONumber=AcomDC_0817_2016-->