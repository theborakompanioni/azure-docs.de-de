---
title: Analysieren und Verarbeiten von JSON-Dokumenten mit Hive in HDInsight | Microsoft Docs
description: Informationen zum Verwenden und Analysieren von JSON-Dokumenten mit der Struktur in HDInsight
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: e17794e8-faae-4264-9434-67f61ea78f13
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/06/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: 763bc597bdfc40395511cdd9d797e5c7aaad0fdf
ms.openlocfilehash: ee7d40d2ff0ae1ac10b54f4c1f1dd704a70eb70c
ms.contentlocale: de-de
ms.lasthandoff: 09/06/2017

---
# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Verarbeiten und Analysieren von JSON-Dokumenten mit Hive in HDInsight

Erfahren Sie, wie JSON-Dateien mit Hive in HDInsight verarbeitet und analysiert werden. In diesem Tutorial wird das folgende JSON-Dokument verwendet:

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

Die Datei finden Sie unter wasb://processjson@hditutorialdata.blob.core.windows.net/. Weitere Informationen zur Verwendung von Azure-Blobspeicher mit HDInsight finden Sie unter [Verwenden des HDFS-kompatiblen Azure-Blobspeichers mit Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md). Sie können die Datei in den Standardcontainer des Clusters kopieren.

In diesem Tutorial verwenden Sie die Hive-Konsole.  Anweisungen zum Öffnen der Hive-Konsole finden Sie unter [Verwenden von Hive mit Hadoop in HDInsight über den Remotedesktop](hdinsight-hadoop-use-hive-remote-desktop.md).

## <a name="flatten-json-documents"></a>Vereinfachen von JSON-Dokumenten
Die im nächsten Abschnitt aufgeführten Methoden erfordern das JSON-Dokument in einer einzelnen Zeile. Sie müssen also das JSON-Dokument zu einer Zeichenfolge vereinfachen. Wenn Ihr JSON-Dokument bereits vereinfacht wurde, können Sie diesen Schritt überspringen und direkt mit dem nächsten Abschnitt fortfahren und die JSON-Daten analysieren.

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

Die unformatierte JSON-Datei befindet sich unter **wasb://processjson@hditutorialdata.blob.core.windows.net/**. Die Hive-Tabelle *StudentsRaw* verweist auf das unformatierte, nicht vereinfachte JSON-Dokument.

Die Hive-Tabelle *StudentsOneLine* speichert die Daten im HDInsight-Standarddateisystem unter dem Pfad */json/students/*.

Die INSERT-Anweisung füllt die Tabelle „StudentOneLine“ mit den vereinfachten JSON-Daten.

Die SELECT-Anweisung gibt nur eine Zeile zurück.

Dies ist die Ausgabe der SELECT-Anweisung:

![Vereinfachen (Flattening) des JSON-Dokuments][image-hdi-hivejson-flatten]

## <a name="analyze-json-documents-in-hive"></a>Analysieren von JSON-Dokumenten in Hive
In der Struktur sind drei verschiedene Mechanismen zum Ausführen von Abfragen bei JSON-Dokumenten verfügbar:

* Verwenden Sie die UDF (User Defined Function, benutzerdefinierte Funktion) GET\_JSON\_OBJECT.
* Verwenden Sie die JSON_TUPLE-UDF.
* Verwenden Sie ein benutzerdefiniertes Serialisierungs-/Deserialisierungsprogramm (SerDe).
* Schreiben Sie mit Python oder anderen Sprachen eine eigene UDF. In [diesem Artikel][hdinsight-python] finden Sie weitere Informationen zum Ausführen von Python-Code mit Hive.

### <a name="use-the-getjsonobject-udf"></a>Verwenden der GET\_JSON_OBJECT-UDF
In Hive ist eine integrierte UDF namens [get json object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) verfügbar, mit der JSON-Abfragen während der Laufzeit ausgeführt werden können. Von dieser Methode werden zwei Argumente akzeptiert: der Tabellenname/Methodenname mit dem vereinfachten JSON-Dokument sowie das JSON-Feld, das analysiert werden muss. An einem Beispiel soll verdeutlicht werden, wie diese UDF funktioniert.

Abrufen der Vor- und Nachnamen aller Studierenden

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Dies ist die Ausgabe, wenn diese Abfrage im Konsolenfenster ausgeführt wurde:

![UDF „get_json_object“][image-hdi-hivejson-getjsonobject]

Für die get-json_object-UDF gibt es einige Einschränkungen.

* Da für jedes Feld in der Abfrage eine erneute Analyse die Abfrage erforderlich ist, wird die Leistung beeinträchtigt.
* GET\_JSON_OBJECT() gibt die Zeichenfolgendarstellung eines Arrays zurück. Zum Umwandeln dieses Arrays in ein Hive-Array müssen Sie reguläre Ausdrücke verwenden, um die eckigen Klammern "[" und "]" zu ersetzen, und Sie müssen die Funktion "Call Split" verwenden, um das Array zu erhalten.

Deshalb wird im Hive-Wiki die Verwendung von "json_tuple" empfohlen.  

### <a name="use-the-jsontuple-udf"></a>Verwenden der JSON_TUPLE-UDF
Eine andere in Hive verfügbare UDF ist [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple). Diese Funktion ist leistungsfähiger als [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Bei dieser Methode wird mit einer einzigen Funktion ein Satz von Schlüsseln sowie eine JSON-Zeichenfolge verwendet, um ein Tupel Werte zurückzugeben.  Die folgende Abfrage gibt die ID der Studierenden und die Klasse aus dem JSON-Dokument zurück:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

Die Ausgabe des Skripts in der Hive-Konsole:

![UDF „json_tuple“][image-hdi-hivejson-jsontuple]

JSON\_TUPLE verwendet die Syntax [Lateral View](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) in Hive, mit der von „json\_tuple“ eine virtuelle Tabelle erstellt wird, indem die UDT-Funktion auf jede Zeile der Originaltabelle angewendet wird.  Komplexe JSONs werden durch die wiederholte Verwendung von LATERAL VIEW zu unhandlich. Darüber hinaus kann JSON_TUPLE keine geschachtelten JSONs verarbeiten.

### <a name="use-custom-serde"></a>Verwenden eines benutzerdefinierten Serialisierungs-/Deserialisierungsprogramms (SerDe)
SerDe ist die beste Wahl für die Analyse von geschachtelten JSON-Dokumenten. Sie können damit das JSON-Schema definieren und das Schema verwenden, um die Dokumente zu analysieren. Anweisungen hierzu finden Sie unter [How to use a Custom JSON Serde with Microsoft Azure HDInsight](https://blogs.msdn.microsoft.com/bigdatasupport/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight/) (Verwenden eines benutzerdefinierten JSON-SerDe mit Microsoft Azure HDInsight).

## <a name="summary"></a>Zusammenfassung
Es lässt sich zusammenfassend feststellen, dass der JSON-Operatortyp in der Struktur, den Sie auswählen, von Ihrem Szenario abhängt. Wenn in einem einfachen JSON-Dokument nur ein einziges Feld durchsucht werden soll, können Sie die Hive-UDF „get\_json\_object“ verwenden. Wenn mehrere Suchschlüssel vorliegen, können Sie die UDF „json_tuple“ verwenden. Bei geschachtelten Dokumenten müssen Sie das JSON-SerDe verwenden.

## <a name="next-steps"></a>Nächste Schritte

Verwandte Artikel

* [Verwenden von Hive und HiveQL mit Hadoop in HDInsight zum Analysieren einer Apache Log4j-Beispieldatei](hdinsight-use-hive.md)
* [Analysieren von Flugverspätungsdaten mit Hive in HDInsight](hdinsight-analyze-flight-delay-data.md)
* [Analysieren von Twitter-Daten mit Hive in HDInsight](hdinsight-analyze-twitter-data.md)
* [Ausführen eines Hadoop-Auftrags mit Azure Cosmos DB und HDInsight](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png

