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
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: c43fb7b513136bb3ea964419d68f58585bba48fd
ms.openlocfilehash: 348971e07169ae9c155753c7943124e96e6e9597


---
# <a name="test-and-debug-u-sql-jobs-by-using-local-run-and-the-azure-data-lake-u-sql-sdk"></a>Testen und Debuggen von U-SQL-Aufträgen mit lokalen Testläufen und dem Azure Data Lake-U-SQL-SDK

Sie können Azure Data Lake-Tools für Visual Studio und das Azure Data Lake-U-SQL-SDK zum Ausführen von U-SQL-Aufträgen auf der Arbeitsstation verwenden, so wie es auch im Azure Data Lake-Dienst möglich ist. Mit diesen beiden Funktionen für lokale Testläufe sparen Sie beim Testen und Debuggen Ihrer U-SQL-Aufträge Zeit.

Voraussetzungen:

- Ein Azure Data Lake Analytics-Konto. Siehe [Erste Schritte mit Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- Azure Data Lake-Tools für Visual Studio. Siehe [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Die U-SQL-Skriptentwicklungsumgebung. Siehe [Erste Schritte mit Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).


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



## <a name="use-local-run-from-the-data-lake-u-sql-sdk"></a>Verwenden lokaler Testläufe über das Data Lake-U-SQL-SDK

Neben der lokalen Ausführung von U-SQL-Skripts mithilfe von Visual Studio können Sie auch das Azure Data Lake-U-SQL-SDK verwenden, um U-SQL-Skripts lokal über die Befehlszeile und Programmierschnittstellen auszuführen. Damit können Sie Ihren lokalen U-SQL-Test skalieren.

### <a name="install-the-sdk"></a>Installieren des SDKs

Für das Data Lake-U-SQL-SDK sind die folgenden Komponenten erforderlich:

- [Microsoft .NET Framework 4.6 oder höher](https://www.microsoft.com/en-us/download/details.aspx?id=17851).
- Microsoft Visual C++ 14 und Windows SDK 10.0.10240.0 oder höher. Es gibt zwei Möglichkeiten, diese Komponente zu erhalten:

    - Installieren Sie [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou). Im Ordner „Programme“ befindet sich der Ordner „\Windows Kits\10“, z.B. „C:\Programme (x86) \Windows Kits\10“\. Sie finden die Windows 10 SDK-Version unter „\Windows Kits\10\Lib“. Wenn dieser Ordner nicht angezeigt wird, installieren Sie Visual Studio, und wählen Sie während der Installation das Windows 10 SDK aus. Das lokale U-SQL-Compilerskript findet diese erforderlichen Komponenten dann automatisch.

    ![Windows 10 SDK für lokale Testläufe der Data Lake-Tools für Visual Studio](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)

    - Installieren Sie [Data Lake-Tools für Visual Studio](http://aka.ms/adltoolsvs). Die vorkonfigurierten Visual C++- und Windows SDK-Dateien finden Sie unter „C:\Programme (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK“. In diesem Fall findet das lokale U-SQL-Compilerskript diese erforderlichen Komponenten nicht automatisch. Sie müssen für das Skripts den CppSDK-Pfad angeben. Sie können die Dateien an einen anderen Speicherort kopieren oder ohne Änderungen verwenden. Anschließend können Sie auswählen, ob Sie die Umgebungsvariable **SCOPE_CPP_SDK** auf das Verzeichnis festlegen oder das Argument **-CppSDK** mit diesem Verzeichnis an der Befehlszeile des Hilfsprogramms für lokale Testläufe angeben.

Nachdem Sie das SDK installiert haben, müssen Sie die folgenden Konfigurationsschritte ausführen:

- Legen Sie die Umgebungsvariable **SCOPE_CPP_SDK** fest.

    Wenn Sie Microsoft Visual C++ und das Windows SDK über die Installation der Data Lake-Tools für Visual Studio installieren, vergewissern Sie sich, dass der folgende Ordner vorhanden ist:

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    Definieren Sie eine neue Umgebungsvariable namens **SCOPE_CPP_SDK**, die auf dieses Verzeichnis verweist. Oder kopieren Sie den Ordner an einen anderen Speicherort, und geben Sie **SCOPE_CPP_SDK** entsprechend an.

    Zusätzlich zum Festlegen der Umgebungsvariablen können Sie das Argument **-CppSDK** angeben, wenn Sie die Befehlszeile verwenden. Dieses Argument überschreibt die standardmäßige Umgebungsvariable „CppSDK“.

- Legen Sie die Umgebungsvariable **LOCALRUN_DATAROOT** fest.

    Definieren Sie eine neue Umgebungsvariable namens **LOCALRUN_DATAROOT**, die auf den Datenstamm verweist.

    Zusätzlich zum Festlegen der Umgebungsvariablen können Sie auch das Argument **-DataRoot** mit dem Datenstammpfad angeben, wenn Sie die Befehlszeile verwenden. Dieses Argument überschreibt die standardmäßige Datenstamm-Umgebungsvariable. Sie müssen dieses Argument jeder Befehlszeile hinzufügen, die Sie ausführen, damit Sie die standardmäßige Datenstamm-Umgebungsvariable für alle Vorgänge überschreiben können.

### <a name="use-the-sdk-from-the-command-line"></a>Verwenden des SDK über die Befehlszeile

#### <a name="command-line-interface-of-the-helper-application"></a>Befehlszeilenschnittstelle des Hilfsprogramms

„LocalRunHelper.exe“ im SDK ist das Befehlszeilen-Hilfsprogramm, das Schnittstellen für die meisten häufig verwendeten Funktionen für lokale Testläufe bereitstellt. Beachten Sie, dass für den Befehl und die Argumentoptionen die Groß-/Kleinschreibung beachtet wird. So rufen Sie es auf:

    LocalRunHelper.exe <command> <Required-Command-Arguments> [Optional-Command-Arguments]

Führen Sie „LocalRunHelper.exe“ ohne Argumente oder mit der Option **help** aus, um die Hilfeinformationen anzuzeigen:

    > LocalRunHelper.exe help

        Command 'help' :  Show usage information
        Command 'compile' :  Compile the script
        Required Arguments :
            -Script param
                    Script File Path
        Optional Arguments :
            -Shallow [default value 'False']
                    Shallow compile

In den Hilfeinformationen:

-  **Command** gibt den Namen des Befehls an.  
-  **Required Argument** enthält Argumente, die angegeben werden müssen.  
-  **Optional Argument** enthält optionale Argumente und deren Standardwerte.  Optionale boolesche Argumente verfügen nicht über Parameter, und ihre Darstellung ist das Negativ ihres Standardwerts.

#### <a name="return-value-and-logging"></a>Rückgabewert und Protokollierung

Das Hilfsprogramm gibt **0** für Erfolg und **-1** für Fehler zurück. Standardmäßig sendet das Hilfsprogramm alle Meldungen an die aktuelle Konsole. Die meisten Befehle unterstützen jedoch das optionale Argument **-MessageOut Pfad_zur_Protokolldatei**, das die Ausgaben in eine Protokolldatei umleitet.


### <a name="sdk-usage-samples"></a>SDK-Verwendungsbeispiele

#### <a name="compile-and-run"></a>Kompilieren und Ausführen

Der Befehl **run** wird verwendet, um das Skript zu kompilieren und dann die kompilierten Ergebnisse auszuführen. Die Befehlszeilenargumente sind eine Kombination der Argumente für **compile** und **run**.

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

Hier sehen Sie ein Beispiel:

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

Neben der Kombination von **compile** und **run** können Sie die kompilierten ausführbaren Dateien auch separat kompilieren und ausführen.

#### <a name="compile-a-u-sql-script"></a>Kompilieren eines U-SQL-Skripts

Der Befehl **compile** wird verwendet, um ein U-SQL-Skript in ausführbare Dateien zu kompilieren.

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

Im Folgenden sind optionale Argumente für die Kompilierung aufgeführt:

|Argument|Beschreibung|
|--------|-----------|
|-CppSDK [Standardwert '']|CppSDK-Verzeichnis.|
|-DataRoot [Standardwert '']|Datenstammordner für Daten und Metadaten. Der Standardwert ist die Umgebungsvariable **LOCALRUN_DATAROOT**.|
|-MessageOut [Standardwert '']|Sichern von Meldungen der Konsole in einer Datei.|
|-Shallow [Standardwert 'False']|Flache Kompilierung. Nur eine Syntaxüberprüfung des Skripts und der Rückgabe wird ausgeführt.|
|-WorkDir [Standardwert 'D:\localrun\t\ScopeWorkDir']|Verzeichnis für die Compilernutzung und -ausgaben. Weitere Informationen finden Sie unter „Arbeitsverzeichnis“ im Anhang.|

Im Folgenden sind optionale Argumente für Assemblys und CodeBehind aufgeführt:

|Argument|Beschreibung|
|--------|-----------|
|-CodeBehind [Standardwert 'False']|Indikator dafür, dass das Skript über eine CS-CodeBehind-Datei verfügt, die automatisch als benutzerdefinierter Objekttyp (User-Defined Object, UDO) kompiliert und registriert wird|
|-References [Standardwert '']|Liste mit Pfaden zu zusätzlichen Verweisassemblys oder Datendateien einer CodeBehind, getrennt durch „;“|
|-UseDatabase [Standardwert 'master']|Datenbank, die für die temporäre Assemblyregistrierung von CodeBehind verwendet wird|
|-UdoRedirect [Standardwert 'False']|UDO-Assemblyumleitungskonfiguration, die für .NET Runtime angibt, dass abhängige Assemblys aus dem kompilierten Ausgabeverzeichnis zuerst überprüft werden müssen, wenn UDO aufgerufen wird|

Im Folgenden finden Sie einige Beispiele zur Verwendung.

Kompilieren eines U-SQL-Skripts:

    LocalRunHelper compile -Script d:\test\test1.usql

Kompilieren eines U-SQL-Skripts und Festlegen des Datenstammordners. Beachten Sie, dass damit die festgelegte Umgebungsvariable überschrieben wird.

    LocalRunHelper compile -Script d:\test\test1.usql –DataRoot c:\DataRoot

Kompilieren eines U-SQL-Skripts und Festlegen eines Arbeitsverzeichnisses, einer Verweisassembly und einer Datenbank:

    LocalRunHelper compile -Script d:\test\test1.usql -WorkDir d:\test\bin -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB

#### <a name="execute-compiled-results"></a>Ausführen von kompilierten Ergebnissen

Der Befehl **execute** wird zum Ausführen von kompilierten Ergebnissen verwendet.   

    LocalRunHelper execute -Algebra path_to_compiled_algebra_file [optional_arguments]

Im Folgenden sind optionale Argumente aufgeführt:

|Argument|Beschreibung|
|--------|-----------|
|-DataRoot [Standardwert '']|Stammordner für die Ausführung von Metadaten. Der Standardwert ist die Umgebungsvariable **LOCALRUN_DATAROOT**.|
|-MessageOut [Standardwert '']|Sichern von Meldungen der Konsole in einer Datei.|
|-Parallel [Standardwert '1']|Indikator dafür, dass die generierten Schritte für lokale Testläufe mit der angegebenen Parallelitätsebene ausgeführt werden.|
|-Verbose [Standardwert 'False']|Indikator dafür, dass ausführliche Ausgaben der Runtime angezeigt werden.|

Im Folgenden finden Sie ein Beispiel zur Verwendung:

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5

## <a name="use-the-sdk-with-programming-interfaces"></a>Verwenden des SDK mit Programmierschnittstellen

Die Programmierschnittstellen befinden sich alle in der Assembly „Microsoft.Analytics.LocalRun“. Sie können sie verwenden, um die Funktionalität des U-SQL-SDK und des C#-Testframeworks zu integrieren, sodass Sie Ihren lokalen U-SQL-Skripttest skalieren können. Im Anhang finden Sie weitere Informationen zu den Schnittstellen.

## <a name="appendix"></a>Anhang

### <a name="working-directory"></a>Arbeitsverzeichnis

Bei der lokalen Ausführung des U-SQL-Skripts wird ein Arbeitsverzeichnis während der Kompilierung erstellt. Zusätzlich zu den Ausgaben der Kompilierung werden auch die erforderlichen Runtime-Dateien für die lokale Ausführung als Schattenkopien in das Arbeitsverzeichnis aufgenommen. Wenn das Argument **-WorkDir** an der Befehlszeile nicht angegeben wird, wird das Standardarbeitsverzeichnis „ScopeWorkDir“ im aktuellen Verzeichnis erstellt. Folgende Dateien sind im Arbeitsverzeichnis enthalten:

|Verzeichnis/Datei|Definition|Beschreibung|
|--------------|----------|-----------|
|ScopeWorkDir|Arbeitsverzeichnis|Stammordner|
|C6A101DDCB470506|Die Hashzeichenfolge der Runtime-Version|Schattenkopie der Runtime-Dateien, die für die lokale Ausführung benötigt werden|
|\.\Script_66AE4909AA0ED06C|Skriptname und Hashzeichenfolge des Skriptpfads|Ausgaben der Kompilierung und Protokollierung der Ausführungsschritte|
|\.\.\\_\_script\_\_.abr|Compilerausgabe|Algebradatei|
|\.\.\\_\_ScopeCodeGen\_\_.*|Compilerausgabe|Generierter verwalteter Code|
|\.\.\\_\_ScopeCodeGenEngine\_\_.*|Compilerausgabe|Generierter nativer Code|
|\.\.\referenced_assemblies|Assemblyverweis|Dateien für Verweisassemblys|
|\.\.\deployed_resources|Bereitstellung von Ressourcen|Dateien für die Bereitstellung von Ressourcen|
|\.\.\xxxxxxxx.xxx[1..n]_*.*|Ausführungsprotokoll|Protokoll der Ausführungsschritte|

### <a name="programming-interfaces-for-the-azure-data-lake-u-sql-sdk"></a>Programmierschnittstellen für das Azure Data Lake-U-SQL-SDK

Die Programmierschnittstellen befinden sich alle in der Assembly „Microsoft.Analytics.LocalRun“.

#### <a name="microsoftanalyticslocalrunconfiguration"></a>Microsoft.Analytics.LocalRun.Configuration
„Microsoft.Analytics.LocalRun.Configuration“ ist die Klasse der Konfigurationsparameter für die Kompilierung.

**Konstruktor**

public Configuration(string rootPath)

|Parameter|Typ|Beschreibung|
|---------|----|-----------|
|rootPath|System.String|Der Pfad zum aktuellen Verzeichnis des Arbeitskontexts. Wenn „WorkingDirectory“ nicht festgelegt ist, ist das standardmäßige Arbeitsverzeichnis „rootPath“ und „ScopeWorkDir“.|

**Eigenschaften**

|Name|Beschreibung|
|----|-----------|
|CppSDK|Speicherort von CppSDK, wenn keine Systemstandardkonfiguration verwendet wird. |
|DataDirectory|Speicherort, in dem Tabellen, Assemblys und Eingabe-/Ausgabedaten gespeichert werden. Standardmäßig ist es „ScopeWorkDir\DataDir“. |
|GenerateUdoRedirect|Indikator, der angibt, ob eine Konfiguration zum Überschreiben der Umleitung beim Laden von Assemblys generiert werden soll.|
|WorkingDirectory|Arbeitsverzeichnis des Compilers. Standardmäßig ist es „ScopeWorkDir“.|


#### <a name="microsoftanalyticslocalrunlocalcompiler"></a>Microsoft.Analytics.LocalRun.LocalCompiler
„Microsoft.Analytics.LocalRun.LocalCompiler“ ist die lokale U-SQL-Compilerklasse.

**Konstruktor**

public LocalCompiler(Configuration configuration)

|Parameter|Typ|Beschreibung|
|---------|----|-----------|
|Konfiguration|Microsoft.Analytics.LocalRun.Configuration||

**Methode**

public bool Compile( string script, string filePath, bool shallow, out CommonCompileResult result )

|Parameter|Typ|Beschreibung|
|---------|----|-----------|
|script|System.String|Zeichenfolge des Eingabeskripts.|
|filePath|System.String|Pfad der Skriptdatei. Wird auf null festgelegt, um den Standardpfad zu verwenden.|
|shallow|System.Boolean|Flache Kompilierung (nur Überprüfung der Syntax) oder vollständige Kompilierung.|
|result|Microsoft.Cosmos.ClientTools.Shared.CommonCompileResult|Detaillierte Kompilierungsergebnisse.|
|Rückgabewert|System.Boolean|True: Kein schwerwiegender Fehler bei der Kompilierung. <br>False: Schwerwiegender Fehler bei der Kompilierung.|

#### <a name="microsoftanalyticslocalrunlocalrunner--idisposable"></a>Microsoft.Analytics.LocalRun.LocalRunner : IDisposable
„Microsoft.Analytics.LocalRun.LocalRunner : IDisposable“ ist die lokale U-SQL-Ausführungsklasse.

**Konstruktor**

public LocalRunner( string algebraFilePath, string dataRoot, Action<string> postMessage = null )

|Parameter|Typ|Beschreibung|
|---------|----|-----------|
|algebraFilePath|System.String|Pfad zur Algebradatei|
|dataRoot|System.String|Pfad zu „DataRoot“|
|postMessage (optional)|System.Action<String>|Protokollierungshandler für den Status|

public LocalRunner( string algebraFilePath, string dataRoot, string cachePath, string runtimePath, string tempPath, string logPath, Action<Object, ExecutionStatusBase. ExecutionEventArgs> execEventHandler, Object eventContext, Action<string> postMessage = null )

|Parameter|Typ|Beschreibung|
|---------|----|-----------|
|algebraFilePath|System.String|Pfad zur Algebradatei.|
|dataRoot|System.String|Pfad zu „DataRoot“.|
|cachePath|System.String|Pfad zum Verzeichnis mit dem Kompilierungsergebnis. Wird auf null festgelegt, um den Standardpfad zu verwenden, in dem sich die Algebradatei befindet.|
|runtimePath|System.String|Pfad zum Verzeichnis der Runtime-Schattenkopie. Wird auf null festgelegt, um den Standardpfad zu verwenden, in dem sich das übergeordnete Verzeichnis von „cachePath“ befindet.|
|tempPath|System.String|Temporärer Speicherpfad, nur für die interne Verwendung. Wird auf null festgelegt.|
|logPath|System.String|Pfad, in den Ausführungsprotokolle geschrieben werden. Wird auf null festgelegt, um „cachePath“ als Standardpfad zu verwenden.|
|execEventHandler|System. Action<Object, ExecutionStatusBase.ExecutionEventArgs>|Ereignisbenachrichtigungshandler für Ausführungsstatusänderungen.|
|eventContext|System. Objekt|Kontext des Ereignishandlers.|
|postMessage (optional)|System.Action<String>|Protokollierungshandler für den Status.|

**Eigenschaften**

|Name|Beschreibung|
|----|-----------|
|AlgebraPath|Pfad zur Algebradatei.|
|CachePath|Cachepfad der Compilerergebnisse, in dem generierte Binärdateien gespeichert werden.|
|CompletedSteps|Anzahl der abgeschlossenen Schritte.|
|DataRoot|„DataRoot“ für Metadaten.|
|LastErrorMessage|(Geerbt von „ExecutionStatusBase“.)|
|LogPath|Speicherort der Protokollierungsdatei. Der Setter erstellt das Verzeichnis, wenn es nicht vorhanden ist. Der zuvor erstellte Protokollpfad wird nicht bereinigt.|
|OutputHeader|Sichern des Schemaheaders in Textausgaben.|
|Parallelität|Parallelität. Standardmäßig die Logikprozessoren: 1. Das Ändern dieser Eigenschaft nach dem Start führt zu einer Ausnahme.|
|Status|Ausführungsstatus in einer Skala von 0 bis 100 %.|
|RuntimePath|Speicherort der Runtime-Dateien. Es muss ein Verzeichnis über „CachePath“ sein, wenn es die Schattenkopie vom Compiler ist.|
|Status|Ausführungsstatus <br><br>enum ExecutionStatusBase.ExecutionStatus <br>{ <br>Initialized, // Initialisiert. <br>Running,     // Wird ausgeführt.  In diesem Status überprüft „WaitOne“ nur das Ereignis. <br>Success,     // Erfolgreich abgeschlossen. <br>Error,       // Fehler. <br>}|
|TotalSteps|Gesamtzahl der auszuführenden Schritte. Ein gültiger Wert ist erst nach der Erstellung des gerichteten azyklischen Graphen für den Scheitelpunkt verfügbar.|
|Ausführlich|Ausführliche Informationen während der Ausführung.|

**Methode**

|Methode|Beschreibung|
|------|-----------|
|Cancel()|Abbrechen der aktiven Algebra. <br><br>Zurückgegeben wird ein boolescher Wert. <br><br>Rückgabewert „false“: Fehler beim Abbrechen, überprüfen Sie „LastErrorMessage“ auf Details.|
|Start()|Starten der Ausführung der Algebra. <br><br>Zurückgegeben wird ein boolescher Wert. <br><br>Rückgabewert „false“: Fehler beim Starten, überprüfen Sie „LastErrorMessage“ auf Details.|
|WaitOne() <br>WaitOne(Int32) <br>WaitOne(TimeSpan) <br>WaitOne(Int32, Boolean) <br>WaitOne(TimeSpan, Boolean)|Warten auf den Abschluss. Siehe „WaitHandle.WaitOne“.|
|Dispose()||


## <a name="next-steps"></a>Nächste Schritte

* Eine Übersicht über Data Lake Analytics finden Sie unter [Azure Data Lake Analytics – Übersicht](data-lake-analytics-overview.md).
* Informationen zu den ersten Schritten der Entwicklung von U-SQL-Anwendungen finden Sie unter [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* Informationen zum Erlernen von U-SQL finden Sie unter [Erste Schritte mit der Sprache U-SQL für Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informationen zu Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md).
* Informationen zum Protokollieren von Diagnoseinformationen finden Sie unter [Zugreifen auf Diagnoseprotokolle für Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).
* Eine komplexere Abfrage finden Sie unter [Analysieren von Websiteprotokollen mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Informationen zum Anzeigen von Auftragsdetails finden Sie unter [Verwenden des Auftragsbrowsers und der Auftragsansicht für Azure Data Lake Analytics-Aufträge](data-lake-analytics-data-lake-tools-view-jobs.md).
* Weitere Informationen zur Verwendung der Scheitelpunktausführungsansicht finden Sie unter [Verwenden der Scheitelpunktausführungsansicht in Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).



<!--HONumber=Dec16_HO1-->


