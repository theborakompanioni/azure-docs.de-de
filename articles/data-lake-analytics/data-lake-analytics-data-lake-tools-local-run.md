---
title: "Testen und Debuggen von U-SQL-Aufträgen mit lokalen Testläufen und dem Azure Data Lake-U-SQL-SDK | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Azure Data Lake-Tools für Visual Studio und das Azure Data Lake-U-SQL-SDK zum Testen und Debuggen von U-SQL-Aufträgen auf der lokalen Arbeitsstation verwendet werden."
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/15/2016
ms.author: yanacai
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: 771a96df5cc66bac46e7144785be8cc072b57b31
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---
# <a name="test-and-debug-u-sql-jobs-by-using-local-run-and-the-azure-data-lake-u-sql-sdk"></a>Testen und Debuggen von U-SQL-Aufträgen mit lokalen Testläufen und dem Azure Data Lake-U-SQL-SDK

Sie können Azure Data Lake-Tools für Visual Studio und das Azure Data Lake-U-SQL-SDK zum Ausführen von U-SQL-Aufträgen auf der Arbeitsstation verwenden, so wie es auch im Azure Data Lake-Dienst möglich ist. Mit diesen beiden Funktionen für lokale Testläufe sparen Sie beim Testen und Debuggen Ihrer U-SQL-Aufträge Zeit.

## <a name="understand-the-data-root-folder-and-the-file-path"></a>Grundlegendes zum Datenstammordner und zum Dateipfad

Sowohl für lokale Testläufe als auch für das U-SQL-SDK ist ein Datenstammordner erforderlich. Der Datenstammordner ist ein „lokaler Speicher“ für das lokale Computerkonto. Dies entspricht dem Azure Data Lake Store-Konto eines Data Lake Analytics-Kontos. Der Wechsel zu einem anderen Datenstammordner ähnelt dem Wechsel zu einem anderen Speicherkonto. Wenn Sie auf normalerweise freigegebene Daten mit unterschiedlichen Datenstammordnern zugreifen möchten, müssen Sie in Ihren Skripts absolute Pfade verwenden. Alternativ können Sie symbolische Verknüpfungen im Dateisystem (z.B. **mklink** auf NTFS) unterhalb des Datenstammordners erstellen, die auf die freigegebenen Daten verweisen.

Der Datenstammordner wird für Folgendes verwendet:

- Speichern von Metadaten, einschließlich Datenbanken, Tabellen, Tabellenwertfunktionen (Table-Valued Functions, TVFs) und Assemblys.
- Suchen nach Eingabe- und Ausgabepfaden, die als relative Pfade in U-SQL definiert sind. Das Verwenden relativer Pfade erleichtert das Bereitstellen von U-SQL-Projekten in Azure.

Sie können einen relativen Pfad und einen lokalen absoluten Pfad in U-SQL-Skripts verwenden. Der relative Pfad ist relativ zum angegebenen Pfad des Datenstammordners. Wir empfehlen, „/“ als Pfadtrennzeichen zu verwenden, damit Ihre Skripts mit dem Server kompatibel sind. Hier sind einige Beispiele für relative Pfade und ihre entsprechenden absoluten Pfade. In diesen Beispielen ist „C:\LocalRunDataRoot“ der Datenstammordner.

|Relativer Pfad|Absoluter Pfad|
|-------------|-------------|
|/abc/def/input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|abc/def/input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/abc/def/input.csv |D:\abc\def\input.csv|

## <a name="use-local-run-from-visual-studio"></a>Verwenden von lokalen Testläufen in Visual Studio

Die Data Lake-Tools für Visual Studio ermöglichen lokale U-SQL-Testläufe in Visual Studio. Mit dieser Funktion können Sie die folgenden Schritte ausführen:

- Ein U-SQL-Skript zusammen mit C#-Assemblys lokal ausführen.
- Eine C#-Assembly lokal debuggen.
- U-SQL-Kataloge (lokale Datenbanken, Assemblys, Schemas und Tabellen) über Server-Explorer erstellen, anzeigen und löschen. Sie finden den lokalen Katalog auch über den Server-Explorer.

    ![Lokaler Katalog für lokale Testläufe der Data Lake-Tools für Visual Studio](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-local-catalog.png)

Das Installationsprogramm der Data Lake-Tools erstellt den Ordner „C:\LocalRunRoot“, der als standardmäßiger Datenstammordner verwendet wird. Die standardmäßige Parallelität für lokale Testläufe ist 1.

### <a name="to-configure-local-run-in-visual-studio"></a>So konfigurieren Sie lokale Testläufe in Visual Studio

1. Öffnen Sie Visual Studio.
2. Öffnen Sie **Server-Explorer**.
3. Erweitern Sie **Azure** > **Data Lake Analytics**.
4. Klicken Sie im Menü **Data Lake** auf **Optionen und Einstellungen**.
5. Erweitern Sie in der linken Struktur **Azure Data Lake** und dann **Allgemein**.

    ![Konfigurationseinstellungen für lokale Testläufe der Data Lake-Tools für Visual Studio](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-configure.png)

Ein Visual Studio-U-SQL-Projekt ist für lokale Testläufe erforderlich. Hier liegt der Unterschied zur Ausführung von U-SQL-Skripts über Azure.

### <a name="to-run-a-u-sql-script-locally"></a>So führen Sie ein U-SQL-Skript lokal aus
1. Öffnen Sie Ihr U-SQL-Projekt in Visual Studio.   
2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf ein U-SQL-Skript, und klicken Sie dann auf **Skript senden**.
3. Wählen Sie **(Lokal)** als Analytics-Konto für die lokale Ausführung des Skripts aus.
Sie können auch oben im Skriptfenster auf das Konto **(Lokal)** klicken und dann auf **Senden** klicken (oder verwenden Sie die Tastenkombination STRG+F5).

    ![Senden von Aufträgen für lokale Testläufe mit den Data Lake-Tools für Visual Studio](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-submit-job.png)

### <a name="debug-scripts-and-c-assemblies-locally"></a>Lokales Debuggen von Skripts und C#-Assemblys

Sie können C#-Assemblys debuggen, ohne sie zu senden und beim Azure Data Lake Analytics-Dienst zu registrieren. Sie können sowohl in der Code-Behind-Datei als auch in einem referenzierten C#-Projekt Haltepunkte festlegen.

#### <a name="to-debug-local-code-in-code-behind-file"></a>So debuggen Sie lokalen Code in der CodeBehind-Datei:

1. Legen Sie Haltepunkte in der CodeBehind-Datei fest.
2. Drücken Sie F5, um das Skript lokal zu debuggen.

> [!NOTE]
   > Das folgende Verfahren funktioniert nur in Visual Studio 2015. In älteren Versionen von Visual Studio müssen Sie eventuell die PDB-Dateien manuell hinzufügen.  
   >
   >

#### <a name="to-debug-local-code-in-a-referenced-c-project"></a>So debuggen Sie lokalen Code in einem referenzierten C#-Projekt:

1. Erstellen Sie ein C#-Assemblyprojekt, und erstellen Sie es, um die Ausgabe-DLL zu generieren.
2. Registrieren Sie die DLL mithilfe einer U-SQL-Anweisung:

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. Legen Sie Haltepunkte im C#-Code fest.
4. Drücken Sie F5, um das Skript mit Verweis auf die C#-DLL lokal zu debuggen.

## <a name="use-local-run-from-the-data-lake-u-sql-sdk"></a>Verwenden lokaler Testläufe über das Data Lake-U-SQL-SDK

Neben der lokalen Ausführung von U-SQL-Skripts mithilfe von Visual Studio können Sie auch das Azure Data Lake-U-SQL-SDK verwenden, um U-SQL-Skripts lokal über die Befehlszeile und Programmierschnittstellen auszuführen. Damit können Sie Ihren lokalen U-SQL-Test skalieren.

Weitere Informationen zu [Azure Data Lake U-SQL SDK](data-lake-analytics-u-sql-sdk.md).


## <a name="next-steps"></a>Nächste Schritte

* Eine komplexere Abfrage finden Sie unter [Analysieren von Websiteprotokollen mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Informationen zum Anzeigen von Auftragsdetails finden Sie unter [Verwenden des Auftragsbrowsers und der Auftragsansicht für Azure Data Lake Analytics-Aufträge](data-lake-analytics-data-lake-tools-view-jobs.md).
* Weitere Informationen zur Verwendung der Scheitelpunktausführungsansicht finden Sie unter [Verwenden der Scheitelpunktausführungsansicht in Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).

