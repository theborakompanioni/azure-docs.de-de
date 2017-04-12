---
title: "Verwenden von C# mit Hive und Pig für Hadoop in HDInsight | Microsoft Docs"
description: Erfahren Sie, wie Sie benutzerdefinierte C#-Funktionen (User-Defined Functions, UDFs) mit Hive- und Pig-Streaming in Azure HDInsight verwenden.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d83def76-12ad-4538-bb8e-3ba3542b7211
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/02/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: f9ea34229e2f6e498c2826be04c09fb2da6393e3
ms.lasthandoff: 04/12/2017


---
# <a name="use-c-user-defined-functions-with-hive-and-pig-streaming-on-hadoop-in-hdinsight"></a>Verwenden benutzerdefinierter C#-Funktionen mit Hive- und Pig-Streaming für Hadoop in HDInsight

Hive und Pig sind hervorragend für die Arbeit mit Daten in Azure HDInsight geeignet. Manchmal benötigt man aber eine allgemeinere Sprache. Sowohl Hive als auch Pig ermöglichen es Ihnen, externen Code über benutzerdefinierte Funktionen (User-Defined Functions, UDFs) oder per Streaming aufzurufen.

In diesem Dokument erfahren Sie, wie Sie C# mit Hive und Pig verwenden.

> [!IMPORTANT]
> Die Schritte in diesem Dokument erfordern einen HDInsight-Cluster, der Windows als Betriebssystem nutzt. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

## <a name="prerequisites"></a>Voraussetzungen

* Mindestens Windows 7.

* Visual Studio mit den folgenden Versionen:

  * Visual Studio 2012 Professional/Premium/Ultimate mit [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)

  * Visual Studio 2013 Community/Professional/Premium/Ultimate mit [Update 4](https://www.microsoft.com/download/details.aspx?id=44921)

  * Visual Studio 2015

  * Visual Studio 2017

* Hadoop in HDInsight-Cluster – unter [Bereitstellen eines HDInsight-Clusters](hdinsight-provision-clusters.md) werden die Schritte zum Erstellen eines Clusters beschrieben

* Hadoop Tools für Visual Studio oder Data Lake Tools für Visual Studio. unter [Erste Schritte mit HDInsight Hadoop-Tools für Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) werden die Schritte zum Installieren und Konfigurieren der Tools beschrieben

    > [!NOTE]
    > Data Lake-Tools sind in Visual Studio 2017 installiert, durch Auswählen der Workload __Azure-Entwicklung__ während der Installation.

## <a name="net-on-hdinsight"></a>.NET in HDInsight

Die .NET CLR (Common Language Runtime) und Frameworks sind standardmäßig auf Windows-basierten HDInsight-Clustern installiert. Die .NET CLR ermöglicht Ihnen die Verwendung von C#-Anwendungen mit Hive- und Pig-Streaming (Daten werden zwischen Hive/Pig und der C#-Anwendung über „stdout/stdin“ übergeben).

> [!NOTE]
> Die Ausführung von .NET Framework-UDFs auf Linux-basierten HDInsight-Clustern wird derzeit nicht unterstützt.


## <a name="net-and-streaming"></a>.NET und Streaming

Beim Streaming übergeben Hive und Pig Daten über "stdout" an externe Anwendungen und empfangen die Ergebnisse über "stdin". Verwenden Sie für C#-Anwendungen `Console.ReadLine()` und `Console.WriteLine()`.

Da Hive und Pig die Anwendung zur Laufzeit aufrufen, sollte die Vorlage **Konsolen-App (.NET Framework)** für Ihre C#-Projekte verwendet werden.

## <a name="hive-and-c35"></a>Hive and C&#35;

### <a name="create-the-c-project"></a>Erstellen des C#-Projekts

1. Öffnen Sie Visual Studio, und erstellen Sie eine Lösung. Wählen Sie als Projekttyp **Konsolen-App (.NET Framework)** aus, und geben Sie dem neuen Projekt den Namen **HiveCSharp**.

2. Ersetzen Sie den Inhalt von **Program.cs** durch den folgenden Code:

    ```csharp
    using System;
    using System.Security.Cryptography;
    using System.Text;
    using System.Threading.Tasks;

    namespace HiveCSharp
    {
        class Program
        {
            static void Main(string[] args)
            {
                string line;
                // Read stdin in a loop
                while ((line = Console.ReadLine()) != null)
                {
                    // Parse the string, trimming line feeds
                    // and splitting fields at tabs
                    line = line.TrimEnd('\n');
                    string[] field = line.Split('\t');
                    string phoneLabel = field[1] + ' ' + field[2];
                    // Emit new data to stdout, delimited by tabs
                    Console.WriteLine("{0}\t{1}\t{2}", field[0], phoneLabel, GetMD5Hash(phoneLabel));
                }
            }
            /// <summary>
            /// Returns an MD5 hash for the given string
            /// </summary>
            /// <param name="input">string value</param>
            /// <returns>an MD5 hash</returns>
            static string GetMD5Hash(string input)
            {
                // Step 1, calculate MD5 hash from input
                MD5 md5 = System.Security.Cryptography.MD5.Create();
                byte[] inputBytes = System.Text.Encoding.ASCII.GetBytes(input);
                byte[] hash = md5.ComputeHash(inputBytes);

                // Step 2, convert byte array to hex string
                StringBuilder sb = new StringBuilder();
                for (int i = 0; i < hash.Length; i++)
                {
                    sb.Append(hash[i].ToString("x2"));
                }
                return sb.ToString();
            }
        }
    }
    ```

3. Erstellen Sie das Projekt.

### <a name="upload-to-storage"></a>Hochladen in den Speicher

1. Öffnen Sie den **Server-Explorer**in Visual Studio.

2. Erweitern Sie erst **Azure** und dann **HDInsight**.

3. Geben Sie bei Aufforderung Ihre Azure-Anmeldeinformationen ein, und klicken Sie auf **Anmelden**.

4. Erweitern Sie den HDInsight-Cluster, in dem Sie diese Anwendung bereitstellen möchten, und erweitern dann **Standardmäßiges Speicherkonto**.

    ![Anzeige des Speicherkontos für den Cluster im Server-Explorer ](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

5. Doppelklicken Sie auf **Standardcontainer** für den Cluster, um den Inhalt des Standardcontainers anzuzeigen.
6. Klicken Sie auf das Symbol für den Upload, und navigieren Sie dann zum Ordner **bin\debug** für das Projekt **HiveCSharp**. Wählen Sie anschließend die Datei **HiveCSharp.exe** aus, und klicken Sie auf **OK**.

    ![Symbol für den Upload](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)

7. Nachdem der Upload abgeschlossen wurde, können Sie die Anwendung aus einer Hive-Abfrage verwenden.

### <a name="hive-query"></a>Hive-Abfrage

1. Öffnen Sie den **Server-Explorer**in Visual Studio.

2. Erweitern Sie erst **Azure** und dann **HDInsight**.

3. Klicken Sie mit der rechten Maustaste auf den Cluster, in dem Sie die Anwendung **HiveCSharp** bereitgestellt haben, und wählen Sie dann **Hive-Abfrage schreiben**.

4. Verwenden Sie für die Hive-Abfrage den folgenden Text:

    ```hiveql
    add file wasbs:///HiveCSharp.exe;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'HiveCSharp.exe' AS
    (clientid string, phoneLabel string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;
    ```

    Mit dieser Abfrage werden die Felder `clientid`, `devicemake` und `devicemodel` aus `hivesampletable` ausgewählt und an die Anwendung HiveCSharp.exe übergeben. Die Abfrage erwartet, dass die Anwendung drei Felder zurückgibt, die als `clientid`, `phoneLabel` und `phoneHash` gespeichert werden. Darüber hinaus wird bei der Abfrage davon ausgegangen, dass sich "HiveCSharp.exe" im Stamm des standardmäßigen Speichercontainers (`add file wasbs:///HiveCSharp.exe`) befindet.

5. Klicken Sie auf **Senden**, um den Auftrag an den HDInsight-Cluster zu übermitteln. Das Fenster **Hive-Auftrag – Zusammenfassung** wird geöffnet.

6. Klicken Sie auf **Aktualisieren**, um die Zusammenfassung zu aktualisieren, bis sich der **Auftragsstatus** in **Abgeschlossen** ändert. Klicken Sie auf **Auftragsausgabe**, um die Auftragsausgabe anzuzeigen.

## <a name="pig-and-c35"></a>Pig und C&#35;

### <a name="create-the-c-project"></a>Erstellen des C#-Projekts

1. Öffnen Sie Visual Studio, und erstellen Sie eine Lösung. Wählen Sie als Projekttyp **Konsolenanwendung** aus, und geben Sie dem neuen Projekt den Namen **PigUDF**.

2. Ersetzen Sie den Inhalt der Datei **Program.cs** durch den folgenden Code:

    ```csharp
    using System;

    namespace PigUDF
    {
        class Program
        {
            static void Main(string[] args)
            {
                string line;
                // Read stdin in a loop
                while ((line = Console.ReadLine()) != null)
                {
                    // Fix formatting on lines that begin with an exception
                    if(line.StartsWith("java.lang.Exception"))
                    {
                        // Trim the error info off the beginning and add a note to the end of the line
                        line = line.Remove(0, 21) + " - java.lang.Exception";
                    }
                    // Split the fields apart at tab characters
                    string[] field = line.Split('\t');
                    // Put fields back together for writing
                    Console.WriteLine(String.Join("\t",field));
                }
            }
        }
    }
    ```

    Diese Anwendung analysiert mit Pig gesendete Zeilen und formatiert Zeilen neu, die mit `java.lang.Exception` beginnen.

3. Speichern Sie **Program.cs**, und erstellen Sie dann das Projekt neu.

### <a name="upload-the-application"></a>Hochladen der Anwendung

1. Beim Pig-Streaming wird vorausgesetzt, dass die Anwendung lokal im Clusterdateisystem vorliegt. Aktivieren Sie Remotedesktop für den HDInsight-Cluster, und stellen Sie anschließend mithilfe der Anweisungen unter [Verbinden mit HDInsight-Clustern über RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)eine Verbindung her.

2. Kopieren Sie nach der Verbindungsherstellung **PigUDF.exe** aus dem Verzeichnis **bin/debug** für das PigUDF-Projekt auf Ihren lokalen Computer, und fügen Sie die Datei im Verzeichnis **%PIG_HOME%** des Clusters ein.

### <a name="use-the-application-from-pig-latin"></a>Verwenden der Anwendung aus Pig Latin

1. Starten Sie aus der Remotedesktopsitzung die Hadoop-Befehlszeile über das Symbol **Hadoop-Befehlszeile** auf dem Desktop.

2. Starten Sie die Pig-Befehlszeile wie folgt:

        cd %PIG_HOME%
        bin\pig

    Eine `grunt>`-Eingabeaufforderung wird angezeigt.

3. Geben Sie folgendes ein, um einen Pig-Auftrag über die .NET Framework-Anwendung auszuführen:

        DEFINE streamer `pigudf.exe` SHIP('pigudf.exe');
        LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = STREAM LOG through streamer as (col1, col2, col3, col4, col5);
        DUMP DETAILS;

    Die `DEFINE`-Anweisung erstellt einen Alias von `streamer` für die Anwendung "pigudf.exe", und `SHIP` verteilt sie über die Knoten im Cluster. Später wird `streamer` mit dem `STREAM`-Operator verwendet, um die einzelnen Zeilen in LOG zu verarbeiten und die Daten als Serie von Spalten zurückzugeben.

    > [!NOTE]
    > Der Anwendungsname, der für das Streaming verwendet wird, muss bei der Aliaserstellung mit \` (Gravis) und bei der Verwendung mit `SHIP` von ' (einfaches Anführungszeichen) umschlossen werden.

4. Nach Eingabe der letzten Zeile sollte der Auftrag gestartet werden. Eine Ausgabe ähnlich folgendem Text wird zurückgegeben:

        (2012-02-03 20:11:56 SampleClass5 [WARN] problem finding id 1358451042 - java.lang.Exception)
        (2012-02-03 20:11:56 SampleClass5 [DEBUG] detail for id 1976092771)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1317358561)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1737534798)
        (2012-02-03 20:11:56 SampleClass7 [DEBUG] detail for id 1475865947)

## <a name="summary"></a>Zusammenfassung

In diesem Dokument haben Sie erfahren, wie Sie eine .NET Framework-Anwendung über Hive und Pig in HDInsight verwenden. Informationen zur Verwendung von Python mit Hive und Pig finden Sie unter [Verwenden von Python mit Hive und Pig in HDInsight](hdinsight-python.md).

Informationen zu anderen Möglichkeiten der Verwendung von Pig und Hive sowie Informationen zur Verwendung von MapReduce finden Sie in diesen Dokumenten:

* [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

