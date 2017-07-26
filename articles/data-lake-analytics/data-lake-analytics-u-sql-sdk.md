---
title: "Skalieren der lokalen Ausführung und Tests von U-SQL mit Azure Data Lake U-SQL SDK | Microsoft-Dokumentation"
description: "In diesem Artikel erfahren Sie, wie Sie Azure Data Lake U-SQL SDK verwenden, um die lokale Ausführung von U-SQL SDK-Aufträgen und Tests mit der Befehlszeile und den Programmierschnittstellen auf Ihrer lokalen Arbeitsstation skalieren."
services: data-lake-analytics
documentationcenter: 
author: 
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/01/2017
ms.author: yanacai
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: 55242bcf644ca0e7f30cfe7eada2130451c36e64
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---

# <a name="scale-u-sql-local-run-and-test-with-azure-data-lake-u-sql-sdk"></a>Skalieren der lokalen Ausführung und Tests von U-SQL mit Azure Data Lake U-SQL SDK

Beim Entwickeln von U-SQL-Skript führt man das U-SQL-Skript normalerweise lokal aus und testet es, bevor man es an die Cloud übermittelt. Azure Data Lake stellt ein NuGet-Paket namens „Azure Data Lake U-SQL SDK“ für dieses Szenario zur Verfügung, mit dem Sie U-SQL ganz einfach lokal ausführen und testen können. Es ist auch möglich, diesen U-SQL-Test mit einem CI-System (Continuous Integration) zu integrieren, um das Kompilieren und den Test zu automatisieren.

Wenn Sie sich dafür interessieren, wie man ein U-SQL-Skript mit GUI-Tools manuell lokal ausführt und debuggt, können Sie stattdessen Azure Data Lake Tools für Visual Studio verwenden. Weitere Informationen darüber finden Sie [hier](data-lake-analytics-data-lake-tools-local-run.md).

## <a name="install-azure-data-lake-u-sql-sdk"></a>Installieren von Azure Data Lake U-SQL SDK

Sie können Azure Data Lake U-SQL SDK [hier](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) auf Nuget.org erwerben. Bevor Sie mit der Verwendung beginnen, müssen Sie sicherstellen, dass Sie über die folgenden Abhängigkeiten verfügen.

### <a name="dependencies"></a>Abhängigkeiten

Für das Data Lake-U-SQL-SDK sind die folgenden Komponenten erforderlich:

- [Microsoft .NET Framework 4.6 oder höher](https://www.microsoft.com/download/details.aspx?id=17851).
- Microsoft Visual C++ 14 und Windows SDK 10.0.10240.0 oder höher (in diesem Artikel als „CppSDK“ bezeichnet). Es gibt zwei Möglichkeiten, „CppSDK“ zu erhalten:

    - Installieren Sie [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou). Im Ordner „Program Files“ befindet sich der Ordner „\Windows Kits\10“, z.B. „C:\Program Files (x86) \Windows Kits\10“. Zudem finden Sie die Windows 10 SDK-Version unter „\Windows Kits\10\Lib“. Wenn dieser Ordner nicht angezeigt wird, installieren Sie Visual Studio, und wählen Sie während der Installation das Windows 10 SDK aus. Wenn es mit Visual Studio installiert wurde, findet der lokale U-SQL-Compiler es automatisch.

    ![Windows 10 SDK für lokale Testläufe der Data Lake-Tools für Visual Studio](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)

    - Installieren Sie [Data Lake-Tools für Visual Studio](http://aka.ms/adltoolsvs). Die vorkonfigurierten Visual C++- und Windows SDK-Dateien finden Sie unter „C:\Programme (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK“. In diesem Fall findet das lokale U-SQL-Compilerskript diese erforderlichen Komponenten nicht automatisch. Sie müssen für das Skripts den CppSDK-Pfad angeben. Sie können die Dateien an einen anderen Speicherort kopieren oder ohne Änderungen verwenden.

## <a name="understand-basic-concepts"></a>Grundlegende Konzepte

### <a name="data-root"></a>Stammordner

Der Datenstammordner ist ein „lokaler Speicher“ für das lokale Computerkonto. Dies entspricht dem Azure Data Lake Store-Konto eines Data Lake Analytics-Kontos. Der Wechsel zu einem anderen Datenstammordner ähnelt dem Wechsel zu einem anderen Speicherkonto. Wenn Sie auf normalerweise freigegebene Daten mit unterschiedlichen Datenstammordnern zugreifen möchten, müssen Sie in Ihren Skripts absolute Pfade verwenden. Alternativ können Sie symbolische Verknüpfungen im Dateisystem (z.B. **mklink** auf NTFS) unterhalb des Datenstammordners erstellen, die auf die freigegebenen Daten verweisen.

Der Datenstammordner wird für Folgendes verwendet:

- Speichern von lokalen Metadaten, einschließlich Datenbanken, Tabellen, Tabellenwertfunktionen (Table-Valued Functions, TVFs) und Assemblys.
- Suchen nach Eingabe- und Ausgabepfaden, die als relative Pfade in U-SQL definiert sind. Das Verwenden relativer Pfade erleichtert das Bereitstellen von U-SQL-Projekten in Azure.

### <a name="file-path-in-u-sql"></a>Dateipfade in U-SQL

Sie können einen relativen Pfad und einen lokalen absoluten Pfad in U-SQL-Skripts verwenden. Der relative Pfad ist relativ zum angegebenen Pfad des Datenstammordners. Wir empfehlen, „/“ als Pfadtrennzeichen zu verwenden, damit Ihre Skripts mit dem Server kompatibel sind. Hier sind einige Beispiele für relative Pfade und ihre entsprechenden absoluten Pfade. In diesen Beispielen ist „C:\LocalRunDataRoot“ der Datenstammordner.

|Relativer Pfad|Absoluter Pfad|
|-------------|-------------|
|/abc/def/input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|abc/def/input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/abc/def/input.csv |D:\abc\def\input.csv|

### <a name="working-directory"></a>Arbeitsverzeichnis

Bei der lokalen Ausführung des U-SQL-Skripts wird während der Kompilierung ein Arbeitsverzeichnis unter dem aktuellen Startverzeichnis erstellt. Zusätzlich zu den Ausgaben der Kompilierung werden auch die erforderlichen Runtime-Dateien für die lokale Ausführung als Schattenkopien in das Arbeitsverzeichnis aufgenommen. Der Stammordner des Arbeitsverzeichnisses heißt „ScopeWorkDir“. Folgende Dateien sind im Arbeitsverzeichnis enthalten:

|Verzeichnis/Datei|Verzeichnis/Datei|Verzeichnis/Datei|Definition|Beschreibung|
|--------------|--------------|--------------|----------|-----------|
|C6A101DDCB470506| | |Die Hashzeichenfolge der Runtime-Version|Schattenkopie der Runtime-Dateien, die für die lokale Ausführung benötigt werden|
| |Script_66AE4909AA0ED06C| |Skriptname und Hashzeichenfolge des Skriptpfads|Ausgaben der Kompilierung und Protokollierung der Ausführungsschritte|
| | |\_script\_.abr|Compilerausgabe|Algebradatei|
| | |\_ScopeCodeGen\_.*|Compilerausgabe|Generierter verwalteter Code|
| | |\_ScopeCodeGenEngine\_.*|Compilerausgabe|Generierter nativer Code|
| | |referenced assemblies|Assemblyverweis|Dateien für Verweisassemblys|
| | |deployed_resources|Bereitstellung von Ressourcen|Dateien für die Bereitstellung von Ressourcen|
| | |xxxxxxxx.xxx[1..n]\_\*.*|Ausführungsprotokoll|Protokoll der Ausführungsschritte|


## <a name="use-the-sdk-from-the-command-line"></a>Verwenden des SDK über die Befehlszeile

### <a name="command-line-interface-of-the-helper-application"></a>Befehlszeilenschnittstelle des Hilfsprogramms

Unter SDK directory\build\runtime ist „LocalRunHelper.exe“ das Befehlszeilen-Hilfsprogramm, das Schnittstellen für die meisten häufig verwendeten Funktionen für lokale Testläufe bereitstellt. Beachten Sie, dass für den Befehl und die Argumentoptionen die Groß-/Kleinschreibung beachtet wird. So rufen Sie es auf:

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

### <a name="return-value-and-logging"></a>Rückgabewert und Protokollierung

Das Hilfsprogramm gibt **0** für Erfolg und **-1** für Fehler zurück. Standardmäßig sendet das Hilfsprogramm alle Meldungen an die aktuelle Konsole. Die meisten Befehle unterstützen jedoch das optionale Argument **-MessageOut Pfad_zur_Protokolldatei**, das die Ausgaben in eine Protokolldatei umleitet.

### <a name="environment-variable-configuring"></a>Konfigurieren der Umgebungsvariablen

Die lokale U-SQL-Ausführung benötigt einen angegebenen Stammordner als Speicherkonto, sowie einen angegebenen CppSDK-Pfad für Abhängigkeiten. Sie können das Argument in der Befehlszeile festlegen, oder dafür eine Umgebungsvariable festlegen.

- Legen Sie die Umgebungsvariable **SCOPE_CPP_SDK** fest.

    Wenn Sie Microsoft Visual C++ und das Windows SDK über die Installation der Data Lake-Tools für Visual Studio installieren, vergewissern Sie sich, dass der folgende Ordner vorhanden ist:

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    Definieren Sie eine neue Umgebungsvariable namens **SCOPE_CPP_SDK**, die auf dieses Verzeichnis verweist. Oder kopieren Sie den Ordner an einen anderen Speicherort, und geben Sie **SCOPE_CPP_SDK** entsprechend an.

    Zusätzlich zum Festlegen der Umgebungsvariablen können Sie das Argument **-CppSDK** angeben, wenn Sie die Befehlszeile verwenden. Dieses Argument überschreibt die standardmäßige Umgebungsvariable „CppSDK“.

- Legen Sie die Umgebungsvariable **LOCALRUN_DATAROOT** fest.

    Definieren Sie eine neue Umgebungsvariable namens **LOCALRUN_DATAROOT**, die auf den Datenstamm verweist.

    Zusätzlich zum Festlegen der Umgebungsvariablen können Sie auch das Argument **-DataRoot** mit dem Datenstammpfad angeben, wenn Sie die Befehlszeile verwenden. Dieses Argument überschreibt die standardmäßige Datenstamm-Umgebungsvariable. Sie müssen dieses Argument jeder Befehlszeile hinzufügen, die Sie ausführen, damit Sie die standardmäßige Datenstamm-Umgebungsvariable für alle Vorgänge überschreiben können.

### <a name="sdk-command-line-usage-samples"></a>Verwendungsbeispiele für die SDK-Befehlszeile

#### <a name="compile-and-run"></a>Kompilieren und Ausführen

Der Befehl **run** wird verwendet, um das Skript zu kompilieren und dann die kompilierten Ergebnisse auszuführen. Die Befehlszeilenargumente sind eine Kombination der Argumente für **compile** und **execute**.

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

Im Folgenden sind optionale Argumente für **run** aufgeführt:


|Argument|Standardwert|Beschreibung|
|--------|-------------|-----------|
|-CodeBehind|False|Das Skript verfügt über eine CS-CodeBehind-Datei|
|-CppSDK| |CppSDK-Verzeichnis|
|-DataRoot| Umgebungsvariable „DataRoot“|Datenstamm für die lokale Ausführung, der Standardwert ist die Umgebungsvariable „LOCALRUN_DATAROOT“|
|-MessageOut| |Sichern von Meldungen der Konsole in einer Datei|
|-Parallel|1|Ausführen des Plans mit der angegebenen Parallelität|
|-References| |Liste mit Pfaden zu zusätzlichen Verweisassemblys oder Datendateien mit CodeBehind, getrennt durch „;“|
|-UdoRedirect|False|Generieren einer UDO-Assemblyumleitungskonfiguration|
|-UseDatabase|master|Datenbank, die für die temporäre Assemblyregistrierung von CodeBehind verwendet wird|
|-Verbose|False|Anzeigen ausführlicher Ausgaben der Runtime|
|-WorkDir|Aktuelles Verzeichnis|Verzeichnis für die Compilernutzung und -ausgaben|
|-RunScopeCEP|0|Der zu verwendende ScopeCEP-Modus|
|-ScopeCEPTempPath|temp|Der zum Streamen von Daten zu verwendende Pfad|
|-OptFlags| |Durch Trennzeichen getrennte Liste von Flags für die Optimierung|


Hier sehen Sie ein Beispiel:

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

Neben der Kombination von **compile** und **execute** können Sie die kompilierten ausführbaren Dateien auch separat kompilieren und ausführen.

#### <a name="compile-a-u-sql-script"></a>Kompilieren eines U-SQL-Skripts

Der Befehl **compile** wird verwendet, um ein U-SQL-Skript in ausführbare Dateien zu kompilieren.

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

Im Folgenden sind optionale Argumente für **compile** aufgeführt:


|Argument|Beschreibung|
|--------|-----------|
| -CodeBehind [Standardwert 'False']|Das Skript verfügt über eine CS-CodeBehind-Datei|
| -CppSDK [Standardwert '']|CppSDK-Verzeichnis|
| -DataRoot [Standardwert 'DataRoot environment variable']|Datenstamm für die lokale Ausführung, der Standardwert ist die Umgebungsvariable „LOCALRUN_DATAROOT“|
| -MessageOut [Standardwert '']|Sichern von Meldungen der Konsole in einer Datei|
| -References [Standardwert '']|Liste mit Pfaden zu zusätzlichen Verweisassemblys oder Datendateien mit CodeBehind, getrennt durch „;“|
| -Shallow [Standardwert 'False']|Flache Kompilierung|
| -UdoRedirect [Standardwert 'False']|Generieren einer UDO-Assemblyumleitungskonfiguration|
| -UseDatabase [Standardwert 'master']|Datenbank, die für die temporäre Assemblyregistrierung von CodeBehind verwendet wird|
| -WorkDir [Standardwert 'Current Directory']|Verzeichnis für die Compilernutzung und -ausgaben|
| -RunScopeCEP [Standardwert '0']|Der zu verwendende ScopeCEP-Modus|
| -ScopeCEPTempPath [Standardwert 'temp']|Der zum Streamen von Daten zu verwendende Pfad|
| -OptFlags [Standardwert '']|Durch Trennzeichen getrennte Liste von Flags für die Optimierung|


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

Im Folgenden sind optionale Argumente für **execute** aufgeführt:

|Argument|Beschreibung|
|--------|-----------|
|-DataRoot [Standardwert '']|Stammordner für die Ausführung von Metadaten. Der Standardwert ist die Umgebungsvariable **LOCALRUN_DATAROOT**.|
|-MessageOut [Standardwert '']|Sichern von Meldungen der Konsole in einer Datei.|
|-Parallel [Standardwert '1']|Indikator dafür, dass die generierten Schritte für lokale Testläufe mit der angegebenen Parallelitätsebene ausgeführt werden.|
|-Verbose [Standardwert 'False']|Indikator dafür, dass ausführliche Ausgaben der Runtime angezeigt werden.|

Im Folgenden finden Sie ein Beispiel zur Verwendung:

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5


## <a name="use-the-sdk-with-programming-interfaces"></a>Verwenden des SDK mit Programmierschnittstellen

Die Programmierschnittstellen befinden sich alle in der Datei „LocalRunHelper.exe“. Sie können sie verwenden, um die Funktionalität des U-SQL-SDK und des C#-Testframeworks zu integrieren, sodass Sie Ihren lokalen U-SQL-Skripttest skalieren können. In diesem Artikel wird das Standard C#-Komponententestprojekt verwendet, um darzustellen, wie diese Schnittstellen für das Testen Ihres U-SQL-Skripts verwendet werden können.

### <a name="step-1-create-c-unit-test-project-and-configuration"></a>Schritt 1: Erstellen eines C#-Komponententestprojekts und Konfiguration

- Erstellen Sie über Datei > Neu > Projekt > Visual C# > Test > Komponententestprojekt ein C#-Komponententestprojekt.
- Fügen Sie „LocalRunHelper.exe“ als Verweis für das Projekt hinzu. „LocalRunHelper.exe“ finden Sie im NuGet-Paket unter \build\runtime\LocalRunHelper.exe.

    ![Azure Data Lake U-SQL SDK-Verweis hinzufügen](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-add-reference.png)

- Achten Sie darauf, dass Sie die Buildzielplattform als x64 einstellen, da U-SQL SDK **nur** x64 unterstützt. Sie können dies über Projekteigenschaft > Build > Zielplattform einstellen.

    ![Azure Data Lake U-SQL SDK x64-Projekt konfigurieren](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-x64.png)

- Stellen Sie sicher, dass Ihre Testumgebung auf x64 eingestellt ist. In Visual Studio können Sie dies über Test > Testeinstellungen > Standardmäßige Prozessorarchitektur > x64 einstellen.

    ![Azure Data Lake U-SQL SDK x64-Testumgebung](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-test-x64.png)

- Stellen Sie sicher, dass Sie alle Abhängigkeitsdateien aus NugetPackage\build\runtime\ in das Arbeitsverzeichnis des Projekts kopieren, das sich normalerweise unter ProjectFolder\bin\x64\Debug befindet.

### <a name="step-2-create-u-sql-script-test-case"></a>Schritt 2: Erstellen eines Testfalls für das U-SQL-Skript

Im folgenden finden Sie Beispielcode für den Test des U-SQL-Skripts. Für das Testen müssen Sie Skripts, Eingabedateien und erwartete Ausgabedateien vorbereiten.

    using System;
    using Microsoft.VisualStudio.TestTools.UnitTesting;
    using System.IO;
    using System.Text;
    using System.Security.Cryptography;
    using Microsoft.Analytics.LocalRun;

    namespace UnitTestProject1
    {
        [TestClass]
        public class USQLUnitTest
        {
            [TestMethod]
            public void TestUSQLScript()
            {
                //Specify the local run message output path
                StreamWriter MessageOutput = new StreamWriter("../../../log.txt");

                LocalRunHelper localrun = new LocalRunHelper(MessageOutput);

                //Configure the DateRoot path, Script Path and CPPSDK path
                localrun.DataRoot = "../../../";
                localrun.ScriptPath = "../../../Script/Script.usql";
                localrun.CppSdkDir = "../../../CppSDK";

                //Run U-SQL script
                localrun.DoRun();

                //Script output 
                string Result = Path.Combine(localrun.DataRoot, "Output/result.csv");

                //Expected script output
                string ExpectedResult = "../../../ExpectedOutput/result.csv";

                Test.Helpers.FileAssert.AreEqual(Result, ExpectedResult);

                //Don't forget to close MessageOutput to get logs into file
                MessageOutput.Close();
            }
        }
    }

    namespace Test.Helpers
    {
        public static class FileAssert
        {
            static string GetFileHash(string filename)
            {
                Assert.IsTrue(File.Exists(filename));

                using (var hash = new SHA1Managed())
                {
                    var clearBytes = File.ReadAllBytes(filename);
                    var hashedBytes = hash.ComputeHash(clearBytes);
                    return ConvertBytesToHex(hashedBytes);
                }
            }

            static string ConvertBytesToHex(byte[] bytes)
            {
                var sb = new StringBuilder();

                for (var i = 0; i < bytes.Length; i++)
                {
                    sb.Append(bytes[i].ToString("x"));
                }
                return sb.ToString();
            }

            public static void AreEqual(string filename1, string filename2)
            {
                string hash1 = GetFileHash(filename1);
                string hash2 = GetFileHash(filename2);

                Assert.AreEqual(hash1, hash2);
            }
        }
    }


### <a name="programming-interfaces-in-localrunhelperexe"></a>Programmierschnittstellen in „LocalRunHelper.exe“

„LocalRunHelper.exe“ bietet die Programmierschnittstellen für das lokale Kompilieren, Ausführen usw. von U-SQL. Die Schnittstellen sind wie folgt gelistet.

**Konstruktor**

public LocalRunHelper([System.IO.TextWriter messageOutput = null])

|-Parameter enthalten.|Typ|Beschreibung|
|---------|----|-----------|
|messageOutput|System.IO.TextWriter|für ausgehende Nachrichten, für die Verwendung von „Console“ auf NULL festgelegt|

**Eigenschaften**

|Eigenschaft|Typ|Beschreibung|
|--------|----|-----------|
|AlgebraPath|string|Der Pfad zur Algebradatei (die Algebradatei ist eines der Kompilierungsergebnisse)|
|CodeBehindReferences|string|Wenn sich im Skript zusätzlicher Code hinter Verweisen befindet, geben Sie die Pfade mit „;“ getrennt an|
|CppSdkDir|string|CppSDK-Verzeichnis|
|CurrentDir|string|Aktuelles Verzeichnis|
|DataRoot|string|Stammpfad der Daten|
|DebuggerMailPath|string|Der Pfad zum Debugger-Mailslot|
|GenerateUdoRedirect|bool|Gibt an, ob eine Konfiguration zum Überschreiben der Umleitung beim Laden von Assemblys generiert werden soll|
|HasCodeBehind|bool|Wenn sich hinter dem Skript Code befindet|
|InputDir|string|Verzeichnis für Eingabedaten|
|MessagePath|string|Dateipfad zum Sichern der Nachrichten|
|OutputDir|string|Verzeichnis für Ausgabedaten|
|Parallelität|int|Parallelität zur Ausführung der Algebra|
|ParentPid|int|PID des übergeordneten Elements auf dem der Dienst das Beenden, Einstellen auf 0 oder Negativ zum Ignorieren überwacht|
|ResultPath|string|Dateipfad zum Sichern der Ergebnisse|
|RuntimeDir|string|Runtime-Verzeichnis|
|scriptPath|string|Hier finden Sie das Skript|
|shallow|bool|Flache Kompilierung oder nicht|
|TempDir|string|Temp-Verzeichnis|
|UseDataBase|string|Geben Sie die Datenbank an, die für die temporäre Assemblyregistrierung von CodeBehind verwendet wird, standardmäßig „master“|
|WorkDir|string|Bevorzugtes Arbeitsverzeichnis|


**Methode**

|Methode|Beschreibung|return|-Parameter enthalten.|
|------|-----------|------|---------|
|public bool DoCompile()|Kompilieren des U-SQL-Skripts|Im Erfolgsfall TRUE| |
|public bool DoExec()|Ausführen des kompilierten Ergebnisses|Im Erfolgsfall TRUE| |
|public bool DoRun()|Ausführen des U-SQL-Skripts (Compile + Execute)|Im Erfolgsfall TRUE| |
|public bool IsValidRuntimeDir(string path)|Überprüfen Sie, ob der angegebene Pfad ein gültiges Runtime-Verzeichnis ist|Bei Gültigkeit TRUE|Der Pfad des Runtime-Verzeichnis|


## <a name="faq-about-common-issue"></a>Häufig gestellte Fragen zu häufigen Problemen

### <a name="error-1"></a>Fehler 1:
E_CSC_SYSTEM_INTERNAL: Internal error! Could not load file or assembly 'ScopeEngineManaged.dll' or one of its dependencies (Datei oder Assembly 'ScopeEngineManaged.dll' oder eine der Abhängigkeiten konnten nicht geladen werden). Das angegebene Modul wurde nicht gefunden.

Überprüfen Sie die folgenden Punkte:

- Stellen Sie sicher, dass Sie über eine x64-Umgebung verfügen. Die Buildzielplattform und die Testumgebung sollten auf x64 eingestellt sein, siehe **Schritt 1: Erstellen eines C#-Komponententestprojekts und Konfiguration**.
- Stellen Sie sicher, dass Sie alle Abhängigkeitsdateien aus NugetPackage\build\runtime\ in das Arbeitsverzeichnis des Projekts kopiert haben.


## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Erlernen von U-SQL finden Sie unter [Erste Schritte mit der Sprache U-SQL für Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informationen zum Protokollieren von Diagnoseinformationen finden Sie unter [Zugreifen auf Diagnoseprotokolle für Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).
* Eine komplexere Abfrage finden Sie unter [Analysieren von Websiteprotokollen mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Informationen zum Anzeigen von Auftragsdetails finden Sie unter [Verwenden des Auftragsbrowsers und der Auftragsansicht für Azure Data Lake Analytics-Aufträge](data-lake-analytics-data-lake-tools-view-jobs.md).
* Weitere Informationen zur Verwendung der Scheitelpunktausführungsansicht finden Sie unter [Verwenden der Scheitelpunktausführungsansicht in Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).

