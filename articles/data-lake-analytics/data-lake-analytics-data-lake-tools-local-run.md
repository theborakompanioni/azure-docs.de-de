---
title: "Testen und Debuggen von U-SQL-Aufträgen mit lokalen Testläufen und dem Azure Data Lake-U-SQL-SDK | Microsoft Docs"
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
ms.sourcegitcommit: 9d088a3afd8830c80462099cc869f1493d09fb41
ms.openlocfilehash: e0385315434ac7a10e121f88469aaaec45160b86


---
# <a name="test-and-debug-u-sql-jobs-using-local-run-and-the-azure-data-lake-u-sql-sdk"></a>Testen und Debuggen von U-SQL-Aufträgen mit lokalen Testläufen und dem Azure Data Lake-U-SQL-SDK

Hier erfahren Sie, wie Azure Data Lake-Tools für Visual Studio und das Azure Data Lake-U-SQL-SDK zum Testen und Debuggen von U-SQL-Aufträgen auf der lokalen Arbeitsstation verwendet werden.  Durch diese beiden Funktionen für lokale Testläufe können Sie U-SQL-Aufträge genau wie im Azure Data Lake-Dienst auf Ihrer Arbeitsstation ausführen. Mit diesen Funktionen sparen Sie beim Testen und Debuggen Ihrer U-SQL-Aufträge Zeit.

Voraussetzungen: 

- Ein Azure Data Lake Analytics-Konto. Siehe [Erste Schritte mit Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md). 
- Die Azure Data Lake-Tools für Visual Studio.  Siehe [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md). 
- Die U-SQL-Skriptentwicklungsumgebung. Siehe [Erste Schritte mit Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md). 


## <a name="understand-data-root-and-file-path"></a>Grundlegendes zu Datenstamm und Dateipfad

Sowohl für lokale Testläufe als auch für das U-SQL-SDK ist ein Datenstammordner erforderlich. Der Datenstamm ist ein „lokaler Speicher“ für das lokale Computerkonto. Dies entspricht dem Data Lake Store-Konto eines Data Lake Analytics-Kontos in Azure. Der Wechsel zu einem anderen Datenstammordner ähnelt dem Wechsel zu einem anderen Speicherkonto. Wenn Sie häufig auf freigegebene Daten mit unterschiedlichen Datenstammordnern zugreifen möchten, müssen Sie in Ihren Skripts absolute Pfade verwenden oder symbolische Verknüpfungen im Dateisystem unter diesen Datenstammordnern erstellen (z.B. „mklink“ in NTFS), die auf die freigegebenen Daten verweisen. 

Der Datenstammordner wird für Folgendes verwendet:

- Speichern von Metadaten, einschließlich Datenbanken, Tabellen, TVFs, Assemblys usw.
- Suchen nach Eingabe- und Ausgabepfaden, die als relative Pfade in U-SQL definiert sind. Das Verwenden relativer Pfade erleichtert das Bereitstellen von U-SQL-Projekten in Azure.

Sie können relative Pfade und lokale absolute Pfade in U-SQL-Skripts verwenden, und der relative Pfad ist relativ zum Pfad des angegebenen Datenstammordners. Es wird empfohlen, „/“ als Pfadtrennzeichen zu verwenden, damit Ihre Skripts mit dem Server kompatibel sind. Hier sind einige Beispiele für relative Pfade und ihre entsprechenden absoluten Pfade. In diesen Beispielen ist „C:\LocalRunDataRoot“ das Datenstammverzeichnis:

|Relativer Pfad|Absoluter Pfad|
|-------------|-------------|
|/abc/def/input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|abc/def/input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/abc/def/input.csv |D:\abc\def\input.csv|

## <a name="use-local-run-from-visual-studio"></a>Verwenden von lokalen Testläufen in Visual Studio

Die Data Lake-Tools für Visual Studio ermöglichen lokale U-SQL-Testläufe in Visual Studio. Mit dieser Funktion können Sie die folgenden Schritte ausführen:

- U-SQL-Skripts zusammen mit C#-Assemblys lokal ausführen.
- C#-Assemblys lokal debuggen.
- U-SQL-Kataloge (lokale Datenbanken, Assemblys, Schemas und Tabellen) über Server-Explorer erstellen, anzeigen und löschen. Sie finden den lokalen Katalog auch über den Server-Explorer.

    ![Lokaler Katalog für lokale Testläufe der Data Lake-Tools für Visual Studio](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-local-catalog.png)

Das Installationsprogramm der Data Lake-Tools erstellt den Ordner „C:\LocalRunRoot“, der als standardmäßiger Datenstammordner verwendet wird. Die standardmäßige Parallelität für lokale Testläufe ist 1. 

### <a name="to-configure-local-run-in-visual-studio"></a>So konfigurieren Sie lokale Testläufe in Visual Studio

1. Öffnen Sie Visual Studio.
2. Öffnen Sie **Server-Explorer**.
3. Erweitern Sie **Azure**, **Data Lake Analytics**.
4. Klicken Sie im Menü **Data Lake** auf **Optionen und Einstellungen**. 
5. Erweitern Sie in der linken Struktur **Azure Data Lake** und dann **Allgemein**.

    ![Konfigurationseinstellungen für lokale Testläufe der Data Lake-Tools für Visual Studio](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-configure.png)

Ein Visual Studio-U-SQL-Projekt ist für lokale Testläufe erforderlich. Hier liegt der Unterschied zur Ausführung von U-SQL-Skripts über Azure.

### <a name="to-run-a-u-sql-script-locally"></a>So führen Sie ein U-SQL-Skript lokal aus
1. Öffnen Sie Ihr U-SQL-Projekt in Visual Studio.   
2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf ein U-SQL-Skript, und klicken Sie dann auf **Skript senden**. Wählen Sie „(Lokal)“ als Analytics-Konto für die lokale Ausführung des Skripts aus.
3. Sie können auch oben im Skriptfenster auf das Konto „(Lokal)“ klicken und dann auf **Senden** klicken (oder verwenden Sie die Tastenkombination **STRG+F5**).

    ![Senden von Aufträgen für lokale Testläufe mit den Data Lake-Tools für Visual Studio](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-submit-job.png)



## <a name="use-local-run-from-data-lake-u-sql-sdk"></a>Verwenden lokaler Testläufe über das Data Lake-U-SQL-SDK
 
Neben der lokalen Ausführung von U-SQL-Skripts mithilfe von Visual Studio können Sie auch das Azure Data Lake-U-SQL-SDK verwenden, um U-SQL-Skripts lokal über die Befehlszeile und Programmierschnittstellen auszuführen. Damit können Sie Ihre lokalen U-SQL-Test skalieren.

### <a name="install-the-sdk"></a>Installieren des SDKs

Für das Data Lake-U-SQL-SDK sind die folgenden Komponenten erforderlich:

- [Microsoft .NET Framework 4.6 oder höher](https://www.microsoft.com/en-us/download/details.aspx?id=17851).
- Microsoft Visual C++ 14 und Windows SDK 10.0.10240.0 oder höher. Es gibt zwei Möglichkeiten, diese Komponenten zu erhalten:

    - Installieren Sie Visual Studio ([Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou)). Im Ordner mit Programmdateien befindet sich der Ordner „\Windows Kits\10“, z.B. „C:\Programme (x86) \Windows Kits\10\"; Sie finden auch die Windows 10 SDK-Version unter „\Windows Kits\10\Lib“. Wenn diese Ordner nicht angezeigt werden, installieren Sie Visual Studio neu, und stellen Sie sicher, dass das Windows 10 SDK bei der Installation aktiviert ist. Auf diese Weise findet das lokale U-SQL-Compilerskript diese erforderlichen Komponenten automatisch.

    ![Windows 10 SDK für lokale Testläufe der Data Lake-Tools für Visual Studio](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)
 
    - Installieren Sie alternativ die [Data Lake-Tools für Visual Studio](http://aka.ms/adltoolsvs). Die vorkonfigurierten VC++- und Windows SDK-Dateien finden Sie unter „C:\Programme (x86) \Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK“. In diesem Fall findet der lokale U-SQL-Compiler die erforderlichen Komponenten nicht automatisch. Sie müssen deshalb den CppSDK-Pfad angeben. Sie können die Dateien an einen anderen Speicherort kopieren oder diese einfach ohne Änderungen verwenden. Anschließend können Sie auswählen, ob Sie die Umgebungsvariable „SCOPE_CPP_SDK“ auf das Verzeichnis festlegen oder das Argument „-CppSDK“ mit diesem Verzeichnis an der Befehlszeile des Hilfsprogramms für lokale Testläufe angeben. 

Nachdem Sie das SDK installiert haben, müssen Sie die folgenden Konfigurationsschritte ausführen:

- Legen Sie die Umgebungsvariable **SCOPE_CPP_SDK** fest.

    Wenn Sie Microsoft Visual C++ und das Windows SDK über die Installation der Data Lake-Tools für Visual Studio installieren, vergewissern Sie sich, dass der folgende Ordner vorhanden ist:

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    Definieren Sie eine neue Umgebungsvariable namens „SCOPE_CPP_SDK“, die auf dieses Verzeichnis verweist. Oder kopieren Sie den Ordner an einen anderen Speicherort, und geben Sie „SCOPE_CPP_SDK“ entsprechend an.

    Zusätzlich zum Festlegen der Umgebungsvariablen können Sie auch das Argument „-CppSDK“ angeben, wenn Sie die Befehlszeile verwenden. Dieses Argument überschreibt die standardmäßige Umgebungsvariable „CppSDK“. 

- Legen Sie die Umgebungsvariable **LOCALRUN_DATAROOT** fest.

    Definieren Sie eine neue Umgebungsvariable namens „LOCALRUN_DATAROOT“, die auf den Datenstamm verweist. 

    Zusätzlich zum Festlegen der Umgebungsvariablen können Sie auch das Argument „-DataRoot“ mit dem Datenstammpfad angeben, wenn Sie die Befehlszeile verwenden. Dieses Argument überschreibt die standardmäßige Datenstamm-Umgebungsvariable. Sie müssen dieses Argument jeder Befehlszeile hinzufügen, die Sie ausführen, damit Sie den Datenstamm für alle Vorgänge überschreiben können. 

### <a name="using-the-sdk-from-command-line"></a>Verwenden des SDK über die Befehlszeile

#### <a name="the-command-line-interface-of-the-helper-application"></a>Befehlszeilenschnittstelle des Hilfsprogramms

„LocalRunHelper.exe“ aus dem SDK ist das Befehlszeilen-Hilfsprogramm, das Schnittstellen für die meisten häufig verwendeten Funktionen für lokale Testläufe bereitstellt. Beachten Sie, dass für den Befehl und die Argumentoptionen die Groß-/Kleinschreibung beachtet wird. So rufen Sie es auf:

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
-  **Optional Argument** enthält Argumente, die optional sind, und die Standardwerte.  Optionale boolesches Argumente verfügen nicht über Parameter, und ihre Darstellung ist das Negativ ihres Standardwerts.

#### <a name="return-value-and-logging"></a>Rückgabewert und Protokollierung

Das Hilfsprogramm gibt **0** bei einem Erfolg und **-1** bei einem Fehler zurück. Standardmäßig gibt das Hilfsprogramm alle Meldungen in der aktuellen Konsole aus.  Die meisten Befehle unterstützen jedoch das optionale Argument **-MessageOut Pfad_zur_Protokolldatei**, das die Ausgaben in eine Protokolldatei umleitet.


### <a name="the-sdk-usage-samples"></a>SDK-Verwendungsbeispiele

#### <a name="compile-and-run"></a>Kompilieren und Ausführen

Der Befehl „run“ wird verwendet, um das Skript zu kompilieren und dann die kompilierten Ergebnisse auszuführen. Die Befehlszeilenargumente sind eine Kombination aus „compile“ und „run“.

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

Beispiel:

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

Neben der Kombination von „compile“ und „run“ können Sie die kompilierten ausführbaren Dateien auch separat kompilieren und ausführen. 

#### <a name="compile-u-sql-script"></a>Kompilieren eines U-SQL-Skripts

Der Befehl „compile“ wird verwendet, um ein U-SQL-Skript in ausführbaren Dateien zu kompilieren. 

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

Weitere optionale Argumente für die Kompilierung finden Sie im Abschnitt zum Arbeitsverzeichnis (-WorkDir) im Anhang.

|Argument|Beschreibung|
|--------|-----------|
|-CppSDK [Standardwert '']|CppSDK-Verzeichnis|
|-DataRoot [Standardwert '']|Datenstamm für Daten und Metadaten, der Standardwert ist die Umgebungsvariable „LOCALRUN_DATAROOT“|
|-MessageOut [Standardwert '']|Sichern von Meldungen der Konsole in einer Datei|
|-Shallow [Standardwert 'False']|Flache Kompilierung, nur eine Syntaxüberprüfung des Skripts und der Rückgabe wird ausgeführt.|
|-WorkDir [Standardwert 'D:\localrun\t\ScopeWorkDir']|Verzeichnis für die Verwendung und die Ausgaben des Compilers, weitere Informationen finden Sie im Anhang unter „Arbeitsverzeichnis“.|

Optionale Argumente für Assemblys und CodeBehind:

|Argument|Beschreibung|
|--------|-----------|
|-CodeBehind [Standardwert 'False']|Das Skript verfügt über eine CS-CodeBehind-Datei, die automatisch als UDO-Objekt kompiliert und registriert wird|
|-References [Standardwert '']|Liste mit Pfaden zu zusätzlichen Verweisassemblys oder Datendateien mit CodeBehind, getrennt durch „;“|
|-UseDatabase [Standardwert 'master']|Datenbank, die für die temporäre Assemblyregistrierung von CodeBehind verwendet wird|
|-UdoRedirect [Standardwert 'False']|Generieren einer UDO-Assemblyumleitungskonfiguration, die für .NET Runtime angibt, dass abhängige Assemblys aus dem kompilierten Ausgabeverzeichnis zuerst überprüft werden, wenn UDO aufgerufen wird|

Im Folgenden finden Sie einige Beispiele zur Verwendung:

Kompilieren Sie ein U-SQL-Skript:

    LocalRunHelper compile -Script d:\test\test1.usql

Kompilieren Sie ein U-SQL-Skript, und legen Sie den Datenstammordner fest. Beachten Sie, dass hiermit die festgelegte Umgebungsvariable überschrieben wird.

    LocalRunHelper compile -Script d:\test\test1.usql –DataRoot c:\DataRoot

Kompilieren Sie ein U-SQL-Skript, und legen Sie Arbeitsverzeichnis, Verweisassembly und Datenbank fest.

    LocalRunHelper compile -Script d:\test\test1.usql -WorkDir d:\test\bin -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB

#### <a name="execute-compiled-result"></a>Ausführen von kompilierten Ergebnissen

Der Befehl „execute“ wird zum Ausführen von kompilierten Ergebnissen verwendet.   

    LocalRunHelper execute -Algebra path_to_compiled_algebra_file [optional_arguments]

Optionale Argumente:

|Argument|Beschreibung|
|--------|-----------|
|-DataRoot [Standardwert '']|Datenstamm für die Metadatenausführung, der Standardwert ist die Umgebungsvariable „LOCALRUN_DATAROOT“|
|-MessageOut [Standardwert '']|Sichern von Meldungen der Konsole in einer Datei|
|-Parallel [Standardwert '1']|Ausführen der generierten Schritte für lokale Testläufe mit der angegebenen Parallelitätsebene|
|-Verbose [Standardwert 'False']|Anzeigen ausführlicher Ausgaben der Runtime|

Im Folgenden finden Sie ein Beispiel zur Verwendung:

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5

## <a name="using-the-sdk-with-programming-interface"></a>Verwenden des SDK mit Programmierschnittstelle

Die Programmierschnittstellen befinden sich alle in der Assembly „Microsoft.Analytics.LocalRun“. Darüber können Sie die Funktionen des U-SQL-SDK und des C#-Testframeworks integrieren, um lokale Tests mit U-SQL-Skripts zu skalieren. Ein entsprechendes neues Dokument ist in Kürze verfügbar. Im Anhang finden Sie weitere Informationen zu den Schnittstellen.

## <a name="appendix"></a>Anhang

### <a name="working-directory"></a>Arbeitsverzeichnis

Bei der lokalen Ausführung eines U-SQL-Skripts wird ein Arbeitsverzeichnis während der Kompilierung erstellt.  Zusätzlich zu den Ausgaben der Kompilierung werden auch die erforderlichen Runtime-Dateien für die lokale Ausführung als Schattenkopien in das Arbeitsverzeichnis aufgenommen.   Wenn das Argument **-WorkDir** an der Befehlszeile nicht angegeben wird, wird das Standardarbeitsverzeichnis „ScopeWorkDir“ im aktuellen Verzeichnis erstellt. Folgende Dateien sind im Arbeitsverzeichnis enthalten.

|Verzeichnis/Datei|Definition|Beschreibung|
|--------------|----------|-----------|
|ScopeWorkDir|Das Arbeitsverzeichnis|Stammordner|
|C6A101DDCB470506|Die Hashzeichenfolge der Runtime-Version|Schattenkopie der Runtime-Dateien, die für die lokale Ausführung benötigt werden|
|\.\Script_66AE4909AA0ED06C|Skriptname und Hashzeichenfolge des Skriptpfads|Ausgaben der Kompilierung und Protokollierung der Ausführungsschritte|
|\.\.\\_\_script\_\_.abr|Compilerausgabe|Die Algebradatei|
|\.\.\\_\_ScopeCodeGen\_\_.*|Compilerausgabe|Generierter verwalteter Code|
|\.\.\\_\_ScopeCodeGenEngine\_\_.*|Compilerausgabe|Generierter nativer Code|
|\.\.\referenced_assemblies|Assemblyverweis|Dateien für Verweisassemblys|
|\.\.\deployed_resources|Bereitstellung von Ressourcen|Dateien für die Bereitstellung von Ressourcen|
|\.\.\xxxxxxxx.xxx[1..n]_*.*|Ausführungsprotokoll|Protokoll der Ausführungsschritte|

### <a name="programming-interfaces-for-azure-data-lake-u-sql-sdk"></a>Programmierschnittstellen für das Azure Data Lake-U-SQL-SDK

Die Programmierschnittstellen befinden sich alle in der Assembly „Microsoft.Analytics.LocalRun“.

#### <a name="microsoftanalyticslocalrunconfiguration"></a>Microsoft.Analytics.LocalRun.Configuration
Klasse der Konfigurationsparameter für die Kompilierung

**Konstruktor**

public Configuration(string rootPath)

|Parameter|Typ|Beschreibung|
|---------|----|-----------|
|rootPath|System.String|Der Pfad zum aktuellen Verzeichnis des Arbeitskontexts. Wenn „WorkingDirectory“ nicht festgelegt ist, ist das standardmäßige Arbeitsverzeichnis „rootPath“ und „ScopeWorkDir“.|

**Eigenschaften**

|Name|Beschreibung|
|----|-----------|
|CppSDK|Speicherort des Cpp-SDK, standardmäßig wird die Systemstandardkonfiguration verwendet |
|DataDirectory|Speicherort von Tabellen, Assemblys und Eingabe-/Ausgabedaten, standardmäßig „ScopeWorkDir\DataDir“ |
|GenerateUdoRedirect|Gibt an, ob eine Konfiguration zum Überschreiben der Umleitung beim Laden von Assemblys generiert werden soll|
|WorkingDirectory|Arbeitsverzeichnis des Compilers, standardmäßig „ScopeWorkDir“, sofern kein anderes Verzeichnis festgelegt ist|


#### <a name="microsoftanalyticslocalrunlocalcompiler"></a>Microsoft.Analytics.LocalRun.LocalCompiler
Die lokale U-SQL-Compilerklasse

**Konstruktor**

public LocalCompiler(Configuration configuration)

|Parameter|Typ|Beschreibung|
|---------|----|-----------|
|Konfiguration|Microsoft.Analytics.LocalRun.Configuration||

**Methode**

public bool Compile( string script, string filePath, bool shallow, out CommonCompileResult result )

|Parameter|Typ|Beschreibung|
|---------|----|-----------|
|script|System.String|Zeichenfolge des Eingabeskripts|
|filePath|System.String|Pfad der Skriptdatei, bei null wird der Standardwert verwendet|
|shallow|System.Boolean|Flache Kompilierung (nur Überprüfung der Syntax) oder vollständige Kompilierung|
|result|Microsoft.Cosmos.ClientTools.Shared.CommonCompileResult|Detaillierte Kompilierungsergebnisse|
|Rückgabewert|System.Boolean|true: Kein schwerwiegender Fehler bei der Kompilierung. <br>false: Schwerwiegender Fehler bei der Kompilierung.|

#### <a name="microsoftanalyticslocalrunlocalrunner--idisposable"></a>Microsoft.Analytics.LocalRun.LocalRunner : IDisposable
Die lokale U-SQL-Ausführungsklasse

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
|algebraFilePath|System.String|Pfad zur Algebradatei|
|dataRoot|System.String|Pfad zu „DataRoot“|
|cachePath|System.String|Pfad zum Verzeichnis mit dem Kompilierungsergebnis, wird auf null festgelegt, um den Standardspeicherort der Algebradatei zu verwenden|
|runtimePath|System.String|Pfad zum Verzeichnis der Runtime-Schattenkopie, wird auf null festgelegt, um das Standardverzeichnis zu verwenden, in dem sich das übergeordnete Verzeichnis von „cachePath“ befindet|
|tempPath|System.String|Temporärer Speicherpfad, nur für die interne Verwendung, wird auf null festgelegt|
|logPath|System.String|Pfad, in den Ausführungsprotokolle geschrieben werden, wird auf null festgelegt, um „cachePath“ als Standard zu verwenden|
|execEventHandler|System. Action<Object, ExecutionStatusBase.ExecutionEventArgs>|Ereignisbenachrichtigungshandler für Ausführungsstatusänderungen|
|eventContext|System. Objekt|Kontext des Ereignishandlers|
|postMessage (optional)|System.Action<String>|Protokollierungshandler für den Status|

**Eigenschaften**

|Name|Beschreibung|
|----|-----------|
|AlgebraPath|Pfad zur Algebradatei|
|CachePath|Der Cachepfad der Compilerergebnisse, in dem generierte Binärdateien gespeichert werden|
|CompletedSteps|Anzahl der abgeschlossenen Schritte|
|DataRoot|„DataRoot“ für Metadaten|
|LastErrorMessage|(Geerbt von „ExecutionStatusBase“.)|
|LogPath|Speicherort der Protokolldatei, Setter erstellt das Verzeichnis, wenn es nicht vorhanden ist, zuvor erstellter Protokollpfad wird nicht bereinigt|
|OutputHeader|Sichern des Schemaheaders in Textausgaben|
|Parallelität|Parallelität, Standard für Logikprozessoren: 1. Ändern dieser Eigenschaft nach dem Start führt zu einer Ausnahme|
|Status|Ausführungsstatus in einer Skala von 0 bis 100 %|
|RuntimePath|Speicherort der Runtime-Dateien, muss ein Verzeichnis über „CachePath“ sein, wenn es die Schattenkopie vom Compiler ist|
|Status|Ausführungsstatus <br>enum ExecutionStatusBase.ExecutionStatus <br>{ <br>Initialized, // Initialisieren <br>Running, // Wird ausgeführt, „WaitOne“ überprüft das Ereignis nur in diesem Status <br>Success, // Wurde erfolgreich abgeschlossen <br>Error, // Fehler <br>}|
|TotalSteps|Gesamtzahl der auszuführenden Schritte, ein gültiger Wert ist erst nach der Erstellung des gerichteten azyklischen Graphen für den Scheitelpunkt verfügbar|
|Ausführlich|Ausführliche Informationen während der Ausführung|

**Methode**

|Methode|Beschreibung|
|------|-----------|
|Cancel()|Abbrechen der aktiven Algebra <br>Rückgabewert <br>Typ: Boolescher Wert <br>false: Fehler beim Abbrechen, überprüfen Sie „LastErrorMessage“ auf Details|
|Start()|Starten der Ausführung der Algebra <br>Rückgabewert <br>Typ: Boolescher Wert <br>false: Fehler beim Starten, überprüfen Sie „LastErrorMessage“ auf Details|
|WaitOne() <br>WaitOne(Int32) <br>WaitOne(TimeSpan) <br>WaitOne(Int32, Boolean) <br>WaitOne(TimeSpan, Boolean)|Warten auf den Abschluss, siehe „WaitHandle.WaitOne“|
|Dispose()||


## <a name="see-also"></a>Weitere Informationen

* Eine Übersicht über Data Lake Analytics finden Sie unter [Azure Data Lake Analytics – Übersicht](data-lake-analytics-overview.md).
* Informationen zu den ersten Schritten der Entwicklung von U-SQL-Anwendungen finden Sie unter [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* Informationen zum Erlernen von U-SQL finden Sie unter [Erste Schritte mit der Sprache U-SQL für Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informationen zu Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md).
* Informationen zum Protokollieren von Diagnoseinformationen finden Sie unter [Accessing diagnostic logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)
* Eine komplexere Abfrage finden Sie unter [Analysieren von Websiteprotokollen mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Zum Anzeigen von Auftragsdetails gehen Sie unter [Use Job Browser and Job View for Azure Data lake Analytics jobs (Verwenden des Auftragsbrowsers und der Auftragsansicht für Azure Data Lake Analytics-Aufträge)](data-lake-analytics-data-lake-tools-view-jobs.md).
* Weitere Informationen zur Verwendung der Scheitelpunktausführungsansicht finden Sie unter [Use the Vertex Execution View in Data Lake Tools for Visual Studio (Verwenden der Scheitelpunktausführungsansicht in Data Lake-Tools für Visual Studio)](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).


<!--HONumber=Nov16_HO4-->


