<properties 
   pageTitle="Debuggen von U-SQL-Aufträgen | Microsoft Azure" 
   description="Erfahren Sie, wie Sie die Vertexdaten fehlerhafter U-SQL-Aufträge mithilfe von Visual Studio debuggen. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>



#Debuggen von C#-Code in U-SQL für Data Lake Analytics-Aufträge 

Hier erfahren Sie, wie Sie mithilfe von Visual Studio-Tools für Azure Data Lake U-SQL-Aufträge debuggen, die aufgrund von Fehlern im Benutzercode nicht erfolgreich ausgeführt wurden.

Mithilfe der Visual Studio-Tools können Sie kompilierten Code und erforderliche Vertexdaten aus dem Cluster herunterladen, um fehlerhafte Aufträge nachzuverfolgen und zu debuggen.

Big Data-Systeme bieten in der Regel ein Erweiterbarkeitsmodell über Sprachen wie Java, C#, Python usw. Viele dieser Systeme stellen nur eingeschränkte Informationen zum Laufzeit-Debugging zur Verfügung, was das Debuggen von Laufzeitfehlern in benutzerdefiniertem Code erschwert. Die neuesten Visual Studio-Tools verfügen über ein Feature zum Debuggen fehlerhafter Vertexdaten. Mithilfe dieses Features können Sie die Laufzeitdaten von Azure auf Ihre lokale Arbeitsstation herunterladen und den fehlerhaften benutzerdefinierten C#-Code mit der gleichen Laufzeit und den exakten Eingabedaten aus der Cloud debuggen. Nach der Problembehebung können Sie den überarbeiteten Code über die Tools erneut in Azure ausführen.

Eine Videopräsentation dieses Features finden Sie unter [Debug your custom code in Azure Data Lake Analytics](https://mix.office.com/watch/1bt17ibztohcb) (Debuggen von benutzerdefiniertem Code in Azure Data Lake Analytics).

>[AZURE.NOTE] Visual Studio reagiert möglicherweise nicht mehr oder stürzt ab, wenn die beiden folgenden Windows-Updates nicht installiert sind: [Microsoft Visual C++ 2015 Redistributable Update 2](https://www.microsoft.com/download/details.aspx?id=51682), [Universelle Windows 10-C-Runtime](https://www.microsoft.com/download/details.aspx?id=50410&wa=wsignin1.0).


##Voraussetzungen
-	Lektüre des Artikels [Erste Schritte](data-lake-analytics-data-lake-tools-get-started.md).

## Erstellen und Konfigurieren von Debugprojekten

Wenn Sie einen fehlerhaften Auftrag im Visual Studio-Tool für Data Lake öffnen, erhalten Sie eine Warnung. Die detaillierte Fehlerinformationen werden auf der Registerkarte „Fehler“ und der gelben Warnungsleiste oben im Fenster angezeigt.

![Azure Data Lake Analytics, Debuggen von U-SQL-Aufträgen mit Visual Studio, Herunterladen von Vertexdaten](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

**So können Sie die Vertexdaten herunterladen und eine Debugprojektmappe erstellen**

1.	Öffnen Sie in Visual Studio einen fehlerhaften U-SQL-Auftrag.
2.	Klicken Sie auf **Herunterladen**, um alle erforderlichen Ressourcen und Eingabedatenströme herunterzuladen. Klicken Sie auf **Wiederholen**, wenn der Download nicht erfolgreich war.
3.	Klicken Sie nach Abschluss des Downloads auf **Öffnen**, um ein lokales Debugprojekt zu erstellen. Eine neue Visual Studio-Projektmappe namens **VertexDebug** mit einem leeren Projekt namens **LocalVertexHost** wird erstellt.

Wenn in der U-SQL-CodeBehind-Datei (Script.usql.cs) benutzerdefinierte Operatoren verwendet werden, müssen Sie ein C#-Projekt mit einer Klassenbibliothek und dem Code der benutzerdefinierten Operatoren erstellen und das Projekt der Projektmappe „VertexDebug“ hinzufügen.

Wenn es DLL-Assemblys gibt, die bei Ihrer Data Lake Analytics-Datenbank registriert sind, müssen Sie den Quellcode der Assemblys der Projektmappe „VertexDebug“ hinzufügen.
 
Wenn Sie eine separate C#-Klassenbibliothek für Ihren U-SQL-Code erstellt und DLL-Assemblys in Ihrer Data Lake Analytics-Datenbank registriert haben, müssen Sie das C#-Quellprojekt der Assemblys der Projektmappe „VertexDebug“ hinzufügen.

In seltenen Fällen werden in der ursprünglichen Projektmappe benutzerdefinierte Operatoren in der U-SQL-CodeBehind-Datei (Script.usql.cs) verwendet. In diesem Fall müssen Sie eine C#-Bibliothek mit dem Quellcode erstellen und den Assemblynamen auf den im Cluster registrierten Namen festlegen. Den im Cluster registrierten Assemblynamen können Sie in dem Skript ermitteln, das im Cluster ausgeführt wurde. Öffnen Sie hierzu den U-SQL-Auftrag, und klicken Sie im Auftragsbereich auf „Skript“.

**So konfigurieren Sie die Projektmappe**

1.	Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das soeben erstellte C#-Projekt, und klicken Sie anschließend auf **Eigenschaften**.
2.	Legen Sie den Ausgabepfad auf den des Arbeitsverzeichnisses des Projekts „LocalVertexHost“ fest. Sie können den Pfad des Arbeitsverzeichnisses des Projekts „LocalVertexHost“ aus den Eigenschaften von „LocalVertexHost“ abrufen.
3.	Erstellen Sie Ihr C#-Projekt so, dass die PDB-Datei im Arbeitsverzeichnis des Projekts „LocalVertexHost“ abgelegt wird. Sie können die PDB-Datei auch manuell in diesen Ordner kopieren.
4.	Überprüfen Sie in **Ausnahmeeinstellungen** die Common Language Runtime-Ausnahmen:

![Azure Data Lake Analytics, Debuggen von U-SQL-Aufträgen mit Visual Studio, Einstellung](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)
 
##Debuggen des Auftrags

Nachdem Sie durch Herunterladen der Vertexdaten eine Debugprojektmappe erstellt und die Umgebung konfiguriert haben, können Sie mit dem Debuggen Ihres U-SQL-Codes beginnen.

1.	Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das soeben erstellte Projekt **LocalVertexHost**, zeigen Sie auf **Debuggen**, und klicken Sie anschließend auf **Neue Instanz starten**. „LocalVertexHost“ muss als Startprojekt festgelegt werden. Ggf. wird zunächst die folgende Meldung angezeigt, die Sie ignorieren können. Es kann bis zu einer Minute dauern, bis der Debugbildschirm angezeigt wird.
 
    ![Azure Data Lake Analytics, Debuggen von U-SQL-Aufträgen mit Visual Studio, Warnung](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

4.	Verwenden Sie die Visual Studio-Debugfunktionen (Überwachung, Variablen usw.), um das Problem zu beheben.
5.	Nachdem Sie ein Problem ausgemacht haben, korrigieren Sie den Code, und erstellen Sie das C#-Projekt neu, bevor Sie es erneut testen, bis alle Probleme gelöst sind. Nachdem das Debuggen letztendlich erfolgreich war, wird im Ausgabefenster die folgende Meldung gezeigt.

        The Program ‘LocalVertexHost.exe’ has exited with code 0 (0x0).
 
##Erneutes Übermitteln des Auftrags

Nachdem Sie das Debuggen des U-SQL-Codes abgeschlossen haben, können Sie den fehlerhaften Auftrag erneut übermitteln.

1. Registrieren Sie die neuen DLL-Assemblys in Ihrer ADLA-Datenbank.

    1.	Erweitern Sie im Server-Explorer/Cloud-Explorer in den Visual Studio-Tools für Data Lake den Knoten **Datenbanken**.
    2.	Klicken Sie mit der rechten Maustaste auf „Assemblys“, um Assemblys zu registrieren.
    3.	Registrieren Sie Ihre neuen DLL-Assemblys in der ADLA-Datenbank.
 
2.	Oder kopieren Sie den C#-Code in die C#-CodeBehind-Datei „script.usql.cs“.
3.	Übermitteln Sie den Auftrag erneut.

##Nächste Schritte

- [Tutorial: Erste Schritte mit Azure Data Lake Analytics-U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md)
- [Tutorial: Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Entwickeln von benutzerdefinierten U-SQL-Operatoren für Azure Data Lake Analytics-Aufträge](data-lake-analytics-u-sql-develop-user-defined-operators.md)

<!---HONumber=AcomDC_0914_2016-->