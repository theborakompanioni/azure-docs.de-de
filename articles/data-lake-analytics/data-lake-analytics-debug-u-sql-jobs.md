---
title: "Debuggen von U-SQL-Aufträgen | Microsoft Docs"
description: Hier erfahren Sie, wie Sie mithilfe von Visual Studio einen U-SQL-Vertexfehler debuggen.
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/02/2016
ms.author: saveenr
ms.translationtype: HT
ms.sourcegitcommit: 2812039649f7d2fb0705220854e4d8d0a031d31e
ms.openlocfilehash: 2a77c72d3062272305208934d6406d040266c753
ms.contentlocale: de-de
ms.lasthandoff: 07/22/2017

---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a>Debuggen von benutzerdefiniertem C#-Code für fehlerhafte U-SQL-Aufträge

U-SQL bietet ein C#-basiertes Erweiterungsmodell, sodass Sie mit eigenem Code Funktionen wie etwa eine benutzerdefinierte Extraktions- oder Reduzierungskomponente hinzufügen können. Weitere Informationen finden Sie im [U-SQL-Programmierbarkeitshandbuch](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf). In der Praxis können für jeden Code Debugmaßnahmen erforderlich sein, und Big Data-Systeme stellen unter Umständen nur eingeschränkte Laufzeit-Debuginformationen wie etwa Protokolldateien bereit.

Azure Data Lake Tools für Visual Studio bietet ein Feature namens **Debuggen von Vertexfehlern**, mit dem Sie einen fehlerhaften Auftrag aus der Cloud auf Ihrem lokalen Computer klonen können, um ihn zu debuggen. Der lokale Klon umfasst die gesamte Cloudumgebung einschließlich Eingabedaten und Benutzercode.

Das folgende Video zeigt die Verwendung des Features „Debuggen von Vertexfehlern“ in Azure Data Lake Tools für Visual Studio.

> [!VIDEO https://e0d1.wpc.azureedge.net/80E0D1/OfficeMixProdMediaBlobStorage/asset-d3aeab42-6149-4ecc-b044-aa624901ab32/b0fc0373c8f94f1bb8cd39da1310adb8.mp4?sv=2012-02-12&sr=c&si=a91fad76-cfdd-4513-9668-483de39e739c&sig=K%2FR%2FdnIi9S6P%2FBlB3iLAEV5pYu6OJFBDlQy%2FQtZ7E7M%3D&se=2116-07-19T09:27:30Z&rscd=attachment%3B%20filename%3DDebugyourcustomcodeinUSQLADLA.mp4]
>

> [!NOTE]
> Für Visual Studio müssen die beiden folgenden Updates installiert sein: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) und die [universelle Windows 10-C-Runtime](https://www.microsoft.com/download/details.aspx?id=50410).

## <a name="download-failed-vertex-to-local-machine"></a>Herunterladen eines fehlerhaften Vertex auf den lokalen Computer

Wenn Sie in Azure Data Lake Tools für Visual Studio einen fehlerhaften Auftrag öffnen, wird eine gelbe Warnleiste mit ausführlichen Fehlermeldungen auf der Fehlerregisterkarte angezeigt.

1. Klicken Sie auf **Herunterladen** , um alle erforderlichen Ressourcen und Eingabedatenströme herunterzuladen. Sollte der Downloadvorgang nicht abgeschlossen werden, klicken Sie auf **Wiederholen**.

2. Klicken Sie nach Abschluss des Downloadvorgangs auf **Öffnen**, um eine lokale Debugumgebung zu generieren. Es wird automatisch eine neue Visual Studio-Instanz mit einer Debugprojektmappe erstellt und geöffnet.

![Azure Data Lake Analytics, Debuggen von U-SQL-Aufträgen mit Visual Studio, Herunterladen von Vertexdaten](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

Aufträge können CodeBehind-Quelldateien oder registrierte Assemblys enthalten. Beiden Typen haben jeweils unterschiedliche Debugszenarien.

- [Debuggen eines fehlerhaften Auftrags mit CodeBehind](#debug-job-failed-with-code-behind)
- [Debuggen eines fehlerhaften Auftrags mit Assemblys](#debug-job-failed-with-assemblies)


## <a name="debug-job-failed-with-code-behind"></a>Debuggen eines fehlerhaften Auftrags mit CodeBehind

Wenn bei einem U-SQL-Auftrag ein Fehler auftritt und der Auftrag Benutzercode enthält (üblicherweise mit der Bezeichnung `Script.usql.cs` in einem U-SQL-Projekt), wird der entsprechende Quellcode in die Debugprojektmappe importiert.  Dort können Sie das Problem mithilfe der Visual Studio-Debugtools (Überwachung, Variablen usw.) behandeln.

> [!NOTE]
> Vergewissern Sie sich vor dem Debuggen, dass im Fenster mit den Ausnahmeeinstellungen**** (STRG+ALT+E) die Option **Common Language Runtime-Ausnahmen** aktiviert ist.

![Azure Data Lake Analytics, Debuggen von U-SQL-Aufträgen mit Visual Studio, Einstellung](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

1. Drücken Sie**** F5, um den CodeBehind-Code auszuführen. Der Code wird ausgeführt, bis er durch eine Ausnahme beendet wird.

2. Öffnen Sie die Datei `ADLTool_Codebehind.usql.cs`, legen Sie Haltepunkte fest, und drücken Sie anschließend**** F5, um den Code schrittweise zu debuggen.

    ![Azure Data Lake Analytics-U-SQL-Debugausnahme](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

## <a name="debug-job-failed-with-assemblies"></a>Debuggen von fehlerhaften Aufträgen mit Assemblys

Wenn Ihr U-SQL-Skript registrierte Assemblys enthält, kann der Quellcode nicht automatisch abgerufen werden. In diesem Fall müssen die Quellcodedateien der Assemblys manuell zur Projektmappe hinzugefügt werden.

### <a name="configure-the-solution"></a>Konfigurieren der Projektmappe

1. Klicken Sie mit der rechten Maustaste auf die Projektmappe **VertexDebug**, und klicken Sie anschließend auf „Hinzufügen“ > „Vorhandenes Projekt...“, um den Quellcode der Assemblys zu suchen und das Projekt zur Debugprojektmappe hinzuzufügen.

    ![Azure Data Lake Analytics-U-SQL-Debugprojekt hinzufügen](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. Klicken Sie in der Projektmappe mit der rechten Maustaste auf **LocalVertexHost**, klicken Sie auf **Eigenschaften**, und kopieren Sie den Pfad des Arbeitsverzeichnisses.

3. Klicken Sie mit der rechten Maustaste auf das Projekt mit dem Assemblyquellcode, klicken Sie auf **Eigenschaften**, wählen Sie links die Registerkarte **Erstellen** aus, und fügen Sie den in Schritt 2 kopierten Pfad als **Ausgabe > Ausgabepfad** ein.

    ![Azure Data Lake Analytics-U-SQL-Debugprojekt, PDB-Pfad einrichten](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

4. Drücken Sie **STRG + ALT + E**, und aktivieren Sie im Fenster mit den Ausnahmeeinstellungen die Option **Common Language Runtime-Ausnahmen**.

### <a name="start-debug"></a>Starten des Debuggens

1. Klicke Sie mit der rechten Maustaste auf das Projekt mit dem Assemblyquellcode, klicken Sie auf **Erneut erstellen**, um PDB-Dateien im Arbeitsverzeichnis `LocalVertexHost` auszugeben.

2. Drücken Sie**** F5. Das Projekt wird ausgeführt, bis es durch eine Ausnahme beendet wird. Möglicherweise wird die folgende Warnmeldung angezeigt. Diese kann gefahrlos ignoriert werden. Es kann bis zu einer Minute dauern, bis der Debugbildschirm angezeigt wird.

    ![Azure Data Lake Analytics, Debuggen von U-SQL-Aufträgen mit Visual Studio, Warnung](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

3. Öffnen Sie Ihren Quellcode, legen Sie Haltepunkte fest, und drücken Sie anschließend**** F5, um den Code schrittweise zu debuggen.

Sie können das Problem auch mithilfe der Visual Studio-Debugtools (Überwachung, Variablen usw.) behandeln.

> [!NOTE]
> Das Projekt mit dem Assemblyquellcode muss nach jeder Änderung des Codes neu erstellt werden, um aktualisierte PDB-Dateien zu generieren.

Wenn das Projekt nach dem Debuggen erfolgreich abgeschlossen wird, erscheint im Ausgabefenster die folgende Meldung:

```
The Program 'LocalVertexHost.exe' has exited with code 0 (0x0).
```

![Azure Data Lake Analytics-U-SQL-Debugging erfolgreich](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

## <a name="resubmit-the-job"></a>Erneutes Übermitteln des Auftrags

Nach Abschluss des Debuggens muss der fehlerhafte Auftrag erneut übermittelt werden.

1. Kopieren Sie bei Aufträgen mit CodeBehind-Projektmappen Ihren C#-Code in die CodeBehind-Quelldatei (in der Regel `Script.usql.cs`).
2. Bei Aufträgen mit Assemblys müssen die aktualisierten DLL-Assemblys in der ADLA-Datenbank registriert werden:
    1. Erweitern Sie im Server- oder Cloud-Explorer den Knoten **ADLA-Konto > Datenbanken**.
    2. Klicken Sie mit der rechten Maustaste auf **Assemblys**, und registrieren Sie die neuen DLL-Assemblys bei der ADLA-Datenbank: ![Azure Data Lake Analytics – U-SQL-Debugging – Registrieren von Assemblys](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)
3. Übermitteln Sie den Auftrag erneut.

## <a name="next-steps"></a>Nächste Schritte

- [U-SQL-Programmierbarkeitshandbuch](data-lake-analytics-u-sql-programmability-guide.md)
- [Entwickeln von benutzerdefinierten U-SQL-Operatoren für Azure Data Lake Analytics-Aufträge](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [Tutorial: Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)

