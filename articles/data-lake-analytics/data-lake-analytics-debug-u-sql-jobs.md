---
title: "Debuggen von U-SQL-Aufträgen | Microsoft Docs"
description: "Erfahren Sie, wie Sie die Vertexdaten fehlerhafter U-SQL-Aufträge mithilfe von Visual Studio debuggen. "
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: jhubbard
editor: cgronlun
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/02/2016
ms.author: yanacai
translationtype: Human Translation
ms.sourcegitcommit: a5bb452582f05981a17c2514e0e40db0571bf61d
ms.openlocfilehash: f9b485bfbfbeb8a95ae1908ef6b1733b9cc6999a


---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a>Debuggen von benutzerdefiniertem C#-Code für fehlerhafte U-SQL-Aufträge

Erfahren Sie, wie Sie mithilfe der Azure Data Lake Tools für Visual Studio U-SQL-Aufträge debuggen, bei denen Fehler im benutzerdefinierten Code aufgetreten sind.

## <a name="background"></a>Hintergrund

U-SQL bietet ein Erweiterbarkeitsmodell über C#: Benutzer können benutzerdefinierten C#-Code schreiben, z.B. Extrahierungs- und Reduzierungsfunktionen, um eine bessere Erweiterbarkeit zu erzielen (weitere Informationen finden Sie unter [Benutzerdefinierter Code in U-SQL](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#user-defined-functions---udf)). Allerdings lassen sich Fehler im Code nicht immer vermeiden, und das Debuggen in großen Datensystemen ist schwierig, weil viele Systeme nur wenig Debuginformationen zur Laufzeit bereitstellen, wie z.B. Protokolle usw. 

Die ADL Tools für Visual Studio bieten eine Funktion namens **Debuggen von Vertexfehlern**, mit der Sie fehlerhafte Umgebungen (einschließlich vorübergehender Eingabedaten, Benutzercode usw.) ganz einfach aus der Cloud auf Ihren lokalen Computer klonen können. So können Sie fehlerhafte Aufträge mit der gleichen Laufzeit und den gleichen Eingabedaten wie in der Cloud nachverfolgen und debuggen.

Das folgende Video zeigt die Verwendung von **Debuggen von Vertexfehlern** in den ADL Tools für Video Studio.

> [!VIDEO https://e0d1.wpc.azureedge.net/80E0D1/OfficeMixProdMediaBlobStorage/asset-d3aeab42-6149-4ecc-b044-aa624901ab32/b0fc0373c8f94f1bb8cd39da1310adb8.mp4?sv=2012-02-12&sr=c&si=a91fad76-cfdd-4513-9668-483de39e739c&sig=K%2FR%2FdnIi9S6P%2FBlB3iLAEV5pYu6OJFBDlQy%2FQtZ7E7M%3D&se=2116-07-19T09:27:30Z&rscd=attachment%3B%20filename%3DDebugyourcustomcodeinUSQLADLA.mp4]
>
>

> [!NOTE]
> Visual Studio reagiert möglicherweise nicht mehr oder stürzt ab, wenn die beiden folgenden Windows-Updates nicht installiert sind: [Microsoft Visual C++ 2015 Redistributable Update 2](https://www.microsoft.com/download/details.aspx?id=51682), [Universelle C-Runtime für Windows](https://www.microsoft.com/download/details.aspx?id=50410&wa=wsignin1.0).
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
* Lektüre des Artikels [Erste Schritte](data-lake-analytics-data-lake-tools-get-started.md) .

## <a name="download-failed-vertex-to-local"></a>Herunterladen eines fehlerhaften Vertex auf das lokale System

Wenn Sie einen fehlerhaften Auftrag in den ADL Tools für Visual Studio öffnen, erhalten Sie eine Warnung. Die detaillierten Fehlermeldungen werden auf der Registerkarte „Fehler“ und der gelben Warnungsleiste oben im Fenster angezeigt.

1. Klicken Sie auf **Herunterladen** , um alle erforderlichen Ressourcen und Eingabedatenströme herunterzuladen. Klicken Sie auf **Wiederholen** , wenn der Download nicht erfolgreich war. 

2. Klicken Sie nach erfolgreichem Download auf **Öffnen**, um eine lokale Debugumgebung zu generieren. Es wird eine neue Visual Studio-Instanz mit einer Debugprojektmappe erstellt und automatisch geöffnet. 

3. Die Debugschritte weichen bei fehlerhaften Codebehindaufträgen und Assemblys ein wenig ab.

    - [Debuggen von fehlerhaften Codebehindaufträgen](#debug-job-failed-with-code-behind)
    - [Debuggen von fehlerhaften Aufträgen mit Assemblys](#debug-job-failed-with-assemblies)

![Azure Data Lake Analytics, Debuggen von U-SQL-Aufträgen mit Visual Studio, Herunterladen von Vertexdaten](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

## <a name="debug-job-failed-with-code-behind"></a>Debuggen von fehlerhaften Codebehindaufträgen

Wenn Sie benutzerdefinierten Code in einer Codebehinddatei (im U-SQL-Projekt üblicherweise als „Script.usql.cs“ bezeichnet) geschrieben haben und dann beim U-SQL-Auftrag ein Fehler auftritt, wird der Quellcode automatisch in die generierte Debugprojektmappe importiert. So können Sie ganz einfach Visual Studio-basierte Debugfunktionen (Überwachung, Variablen usw.) verwenden, um das Problem zu beheben. 

Stellen Sie vor dem Debuggen sicher, dass Sie im Fenster mit den Ausnahmeeinstellungen die Option **Common Language Runtime-Ausnahmen** aktiviert haben (**STRG + ALT + E**).

![Azure Data Lake Analytics, Debuggen von U-SQL-Aufträgen mit Visual Studio, Einstellung](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

1. Drücken Sie **F5**. Der Codebehindcode wird automatisch ausgeführt, bis er von einer Ausnahme angehalten wird.

2. Öffnen Sie **ADLTool_Codebehind.usql.cs** im Projekt, legen Sie Haltepunkte fest, und drücken Sie dann F5, um den Code schrittweise zu debuggen.

    ![Azure Data Lake Analytics-U-SQL-Debugausnahme](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

## <a name="debug-job-failed-with-assemblies"></a>Debuggen von fehlerhaften Aufträgen mit Assemblys

Wenn Sie im U-SQL-Skript registrierte Assemblys verwenden, kann das System den Quellcode nicht automatisch abrufen. Sie müssen vor dem Debuggen von benutzerdefiniertem Code einige Konfigurationsänderungen vornehmen, d.h., Sie müssen den Quellcode der Assemblys zur automatisch generierten Projektmappe hinzufügen.

### <a name="configure-the-solution"></a>Konfigurieren der Projektmappe

1. Klicken Sie mit der rechten Maustaste auf die **Projektmappe „VertexDebug“** > **Hinzufügen** > **Vorhandenes Projekt...**, um den Quellcode der Assemblys zu suchen und das Projekt zur Debugprojektmappe hinzuzufügen.

    ![Azure Data Lake Analytics-U-SQL-Debugprojekt hinzufügen](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. Klicken Sie mit der rechten Maustaste in der Projektmappe auf **LocalVertexHost** > **Eigenschaften**, und kopieren Sie den Pfad **Arbeitsverzeichnis**.

3. Klicken Sie mit der rechten Maustaste auf das **Projekt mit dem Assemblyquellcode** > **Eigenschaften**, wählen Sie links die Registerkarte **Erstellen** aus, und fügen Sie den in Schritt 2 kopierten Pfad in **Ausgabe** > **Ausgabepfad** ein.  

    ![Azure Data Lake Analytics-U-SQL-Debugprojekt, PDB-Pfad einrichten](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

4. Drücken Sie **STRG + ALT + E**, und aktivieren Sie im Fenster mit den Ausnahmeeinstellungen die Option **Common Language Runtime-Ausnahmen**.

### <a name="start-debug"></a>Starten des Debuggens

1. Klicke Sie mit der rechten Maustaste auf das **Projekt mit dem Assemblyquellcode** > **Erneut erstellen**, um PDB-Dateien in das LocalVertexHost-Arbeitsverzeichnis auszugeben.

2. Drücken Sie **F5**. Das Projekt wird automatisch ausgeführt, bis es von einer Ausnahme angehalten wird. Ggf. wird zunächst die folgende Meldung angezeigt, die Sie ignorieren können. Es kann bis zu einer Minute dauern, bis der Debugbildschirm angezeigt wird.

    ![Azure Data Lake Analytics, Debuggen von U-SQL-Aufträgen mit Visual Studio, Warnung](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

3. Öffnen Sie den Quellcode, und legen Sie Haltepunkte fest. Drücken Sie dann **F5**, um den Code schrittweise zu debuggen.

Sie können auch andere Visual Studio-basierte Debugfunktionen (Überwachung, Variablen usw.) verwenden, um das Problem zu beheben. 

**Beachten Sie Folgendes:** Sie müssen das Projekt mit dem Assemblyquellcode nach jeder Änderung des Codes erneut erstellen, damit die neuen PDB-Dateien generiert werden.

Nachdem das Debuggen erfolgreich abgeschlossen wurde, wird im Ausgabefenster die folgende Meldung angezeigt:

    The Program ‘LocalVertexHost.exe’ has exited with code 0 (0x0).

![Azure Data Lake Analytics-U-SQL-Debugging erfolgreich](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

## <a name="resubmit-the-job"></a>Erneutes Übermitteln des Auftrags
Nachdem Sie das Debuggen abgeschlossen haben, können Sie den fehlerhaften Auftrag erneut übermitteln.

1. Registrieren Sie die neuen DLL-Assemblys in Ihrer ADLA-Datenbank.

    1. Erweitern Sie im Server-Explorer bzw. Cloud-Explorer den Knoten **ADLA-Konto** > **Datenbanken**.
    2. Klicken Sie mit der rechten Maustaste auf **Assemblys**, um Assemblys zu registrieren. 
    3. Registrieren Sie Ihre neuen DLL-Assemblys in der ADLA-Datenbank.
    ![Azure Data Lake Analytics-U-SQL-Debugging, Assembly registrieren](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)
2. Oder kopieren Sie den C#-Code wieder in die C#-Codebehinddatei „script.usql.cs“.
3. Übermitteln Sie den Auftrag erneut.

## <a name="next-steps"></a>Nächste Schritte

* [U-SQL-Programmierbarkeitshandbuch](data-lake-analytics-u-sql-programmability-guide.md)
* [Entwickeln von benutzerdefinierten U-SQL-Operatoren für Azure Data Lake Analytics-Aufträge](data-lake-analytics-u-sql-develop-user-defined-operators.md)
* [Tutorial: Erste Schritte mit Azure Data Lake Analytics-U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md)
* [Tutorial: Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)





<!--HONumber=Jan17_HO1-->


