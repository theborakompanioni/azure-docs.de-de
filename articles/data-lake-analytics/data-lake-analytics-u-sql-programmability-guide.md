---
title: "U-SQL-Programmierbarkeitshandbuch für Azure Data Lake Aanlytics | Microsoft Docs"
description: U-SQL-Programmierbarkeitshandbuch
services: data-lake-analytics
documentationcenter: 
author: MikeRys
manager: arindamc
ms.assetid: 63be271e-7c44-4d19-9897-c2913ee9599d
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/15/2016
ms.author: mrys
translationtype: Human Translation
ms.sourcegitcommit: cd2aafd80db337cadaa2217a6638d93186975b68
ms.openlocfilehash: 563a6821b4a3736ef1233aa67d86b9ba06565788


---
# <a name="u-sql-programmability-guide"></a>U-SQL-Programmierbarkeitshandbuch
## <a name="azure-data-lake"></a>Azure Data Lake
Azure Data Lake umfasst alle erforderlichen Funktionen, die Entwickler, Data Scientists und Analysten benötigen, um Daten problemlos speichern zu können – und zwar unabhängig von der Größe, vom Format und von der Geschwindigkeit der Daten. Darüber hinaus können die Daten mit Data Lake auf verschiedene Art, auf verschiedenen Plattformen und unter Verwendung verschiedener Sprachen verarbeitet und analysiert werden. Mit Data Lake gehört die Komplexität beim Erfassen und Speichern von Daten der Vergangenheit an, und mit Batch-, Streaming- und interaktiven Analysen können Sie Ihre Analysen jetzt noch schneller ausführen.

Azure Data Lake ist ein Satz von Diensten, die gemeinsam eine cloudbasierte Big Data-Plattform bereitstellen:

- HDInsight
- Azure Data Lake Store
- Azure Data Lake Analytics

U-SQL ist eine Abfragesprache, die speziell für Big Data-Workloads konzipiert ist. Eines der individuellen Features von U-SQL ist die Kombination der SQL-ähnlichen deklarativen Sprache mit dem Erweiterungs- und Programmierbarkeitspotenzial von C#. Darüber hinaus besteht die Möglichkeit, auf Schemametadaten zuzugreifen, sie zu bearbeiten und benutzerdefinierte Komponenten wie Datenprozessoren und Reducer zu erstellen.

In diesem Handbuch konzentrieren wir uns auf die durch C# ermöglichte Erweiterbarkeit und Programmierbarkeit der U-SQL-Sprache.

## <a name="requirements"></a>Anforderungen
Um mit der ADL-Entwicklung zu beginnen, müssen Sie [Azure Data Lake-Tools für Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504) herunterladen und installieren.

## <a name="starting-with-u-sql"></a>Einstieg mit U-SQL  
Die Beschreibung der U-SQL-Sprache wird in diesem Dokument nicht behandelt. Es werden jedoch grundlegende U-SQL-Konstrukte zur schrittweisen Einführung in U-SQL-Programmierbarkeitsfeatures beschrieben. Weitere Informationen finden Sie in der [U-SQL Language Reference](http://aka.ms/usql_reference) (Referenz zur U-SQL-Sprache).

Betrachten Sie das folgende Beispiel:

```sql
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        guid Guid,
        dt DateTime,
        user String,
        des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT
        MAX(guid) AS start_id,
        MIN(dt) AS start_time,
        user,
        des
    FROM @rs0
    GROUP BY user, des;

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

Im obigen Beispiel haben wir eine **Eingabedatei** – Datei „input_file.tsv“, definiert durch die **lokale Variable** @input_file.

Als Ergebnis der Ausführung des obigen U-SQL-Skripts werden die folgenden Aktionen ausgeführt:

* Zu Anfang lädt die **EXTRACT**-Anweisung Daten durch Konvertieren der Eingabedatei in das **Arbeitsspeicherrowset** in den Arbeitsspeicher.
* **SELECT** aggregiert Daten aus dem Datenrowset und bereitet sie zum Exportieren vor.
* Der **OUTPUT**-Befehl exportiert ein Datenrowset in die – externe – **Ausgabedatei**.

Zunächst betrachten wir einige Optionen zur direkten Verwendung eines C#-Ausdrucks in einem U-SQL-Skript.

## <a name="c-types-and-expressions-in-u-sql-script"></a>C#-Typen und -Ausdrücke in einem U-SQL-Skript
Ein U-SQL-C#-Ausdruck ist – ähnlich allgemeinem C# – eine Sequenz aus mindestens einem Operator, die zu einem einzelnen Wert, Objekt, einer einzelnen Methode oder einem einzelnen Namespace ausgewertet werden kann. Ausdrücke können aus einem Literalwert, einem Methodenaufruf, einem Operator oder einem einfachen Name bestehen. Einfache Namen können der Name einer Variablen, eines Typmembers, Methodenparameters, Namespaces oder Typs sein.

Wenn wir über U-SQL-C#-Ausdrücke sprechen, beziehen wir uns spezifisch auf die C#-Ausdrücke im U-SQL-Basisskript. Der im weiteren Verlauf dieses Dokuments erörterte zugrunde liegende C#-CodeBehind-Abschnitt kann auch C#-Ausdrücke als reguläres C#-Code-basiertes Element enthalten.

Ausdrücke können Operatoren verwenden, die andere Ausdrücke als Parameter verwenden, oder Methodenaufrufe, deren Parameter wiederum andere Methodenaufrufe sind. Beispiel eines Ausdrucks:  

```c#
    Convert.ToDateTime(Convert.ToDateTime(dt).ToString("yyyy-MM-dd"))
```

U-SQL-Sprache ermöglicht die Verwendung von standardmäßigen C#-Ausdrücken aus integrierten Namespaces.  
 
```c#
    Microsoft.Analytics.Interfaces;  
    Microsoft.Analytics.Types.Sql;  
    System;  
    System.Collections.Generic;  
    System.Linq;  
    System.Text;  
```

Allgemeine C#-Ausdrücke können in U-SQL in SELECT und EXTRACT verwendet werden.

Die C#-Ausdrücke können auch in DECLARE- oder IF-Anweisungen verwendet werden. Hier sehen Sie ein Beispiel eines solchen Ausdrucks:   

```c#
    DateTime.Today.Day   
    Convert.ToDateTime
```

Beispiel für ein Skript auf U-SQL-Basis:  

```sql
    DECLARE @default_dt DateTime = Convert.ToDateTime("06/01/2016");
```

C#-Ausdrücke können erweiterte Funktionalität zur Bearbeitung bieten, wenn Spalten Teil des Rowsets sind. Wenn wir z.B. eine DateTime-Spalte mit null Stunden in das Datum konvertieren möchten, können wir folgenden SELECT-Teil des U-SQL-Basisskripts verwenden:

```sql
@rs1 =
    SELECT
        MAX(guid) AS start_id,
     MIN(dt) AS start_time,
        MIN(Convert.ToDateTime(Convert.ToDateTime(dt).ToString("yyyy-MM-dd")))
AS start_zero_time,
        user,
        des
    FROM @rs0
    GROUP BY user, des;
```

Wie Sie sehen, verwenden wir die `System.Convert.ToDateTime`-Methode, um die Konvertierung auszuführen.

Das folgende Szenario veranschaulicht anhand eines etwas komplizierteren Anwendungsfalls die Verwendung einiger grundlegender C#-Operatoren. Betrachten Sie den folgenden Ausdruck.

```sql
@rs1 =
    SELECT
        MAX(guid) AS start_id,
        MIN(dt) AS start_time,
          MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
        user,
        des
    FROM @rs0
    GROUP BY user, des;
```

Er ist ein Beispiel für einen Ausdruck mit einem bedingten C#-Operator.

Die obigen Beispiele veranschaulichen die Verwendung von C#-Ausdrücken im U-SQL-Basisskript. U-SQL bietet jedoch noch umfangreichere Programmierbarkeitsfeatures, die im weiteren Verlauf dieses Dokuments behandelt werden.  

Vollständiges Skript:

```sql
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        guid Guid,
        dt DateTime,
        user String,
        des String
    FROM @input_file USING Extractors.Tsv();

DECLARE @default_dt DateTime = Convert.ToDateTime("06/01/2016");

@rs1 =
    SELECT
        MAX(guid) AS start_id,
    MIN(dt) AS start_time,
        MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
        user,
        des
    FROM @rs0
    GROUP BY user, des;

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

### <a name="using-c-expressions-for-data-type-conversions"></a>Verwendung von C#-Ausdrücken für Datentypkonvertierungen
Das folgende Beispiel aus dem obigen Muster veranschaulicht das Verfahren der DateTime-Datenkonvertierung mithilfe von C#-Ausdrücken. In diesem speziellen Szenario werden DateTime-Zeichenfolgendaten in das Standard-DateTime-Format mit der Zeitnotation „00:00:00“ für Mitternacht konvertiert.

```sql
DECLARE @dt String = "2016-07-06 10:23:15";
@rs1 =
    SELECT Convert.ToDateTime(Convert.ToDateTime(@dt).ToString("yyyy-MM-dd")) AS dt,
           dt AS olddt
    FROM @rs0;
OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

### <a name="using-c-expressions-for-todays-date"></a>Verwenden von C#-Ausdrücken für das heutige Datum
Um das heutige Datum zu extrahieren, können wir den folgenden C#-Ausdruck verwenden.

```c#
DateTime.Now.ToString("M/d/yyyy")
```

In einem Skript

```sql
@rs1 =
    SELECT
        MAX(guid) AS start_id,
        MIN(dt) AS start_time,
        MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
        MIN(USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)) AS start_fiscalperiod,
        DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
        user,
        des
    FROM @rs0
    GROUP BY user, des;
```

## <a name="in-line-c-function-expressions"></a>Inline-C#-Funktionsausdrücke
U-SQL ermöglicht die Inlinedefinition von Funktionsausdrücken als Teil von C#-Ausdrücken. Dies bietet zusätzliche Möglichkeiten der Verwendung von C#-Funktionen mit Ausgabeverweisparametern.

Allgemeine Inlinedefinition von Funktionsausdrücken

```c#
    (Func<type of param1, type of param2>)
    (param1 =>
         { … return param2}
    ) (Rowset Column)
```

Beispiel:

```c#
    (Func<string, DateTime?>)
    (input_p =>
         {
            DateTime dt_result;
            return DateTime.TryParse(input_p, out dt_result)
                   ? (DateTime?) dt_result : (DateTime?) null;
         }
    )
    ) (dt)
```

In diesem Beispiel wird die Inlinefunktion mit dem Zeichenfolgen-Eingabeparameter „input_p“ definiert. Innerhalb dieser Funktion überprüfen wir, ob die Eingabezeichenfolge ein gültiger DateTime-Wert ist. Falls ja, wird sie zurückgegeben; andernfalls wird NULL zurückgegeben.

Die Inlinefunktion ist in diesem Fall notwendig, da die Funktion DateTime.TryParse einen Ausgabeparameter enthält – `out dt_result`. Wir definieren ihn als `DateTime dt_result`.


## <a name="verifying-data-type-values"></a>Überprüfen von Datentypwerten
Einige C#-Funktionen können nicht direkt in U-SQL-Basisskripts als C#-Ausdruck verwendet werden. Dies gilt insbesondere für Funktionen, die einen Ausgabeverweisparameter erfordern. Diese Funktionen können jedoch, wie oben erläutert, als Teil eines Inlinefunktionsausdrucks definiert und verwendet werden.

### <a name="using-inline-function-expressions"></a>Verwenden von Inlinefunktionsausdrücken
Um die Gültigkeit des DateTime-Werts zu überprüfen, können wir `DateTime.TryParse` verwenden.

Hier ist das vollständige Beispiel der Verwendung von Inlinefunktionsausdrücken zum Überprüfen des Datentypwerts mit `DateTime.TryParse`.

In diesem Szenario überprüfen wir den DateTime-Datentypwert.

```sql
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        guid Guid,
        dt string,
        user String,
        des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT
        MAX(guid) AS start_id,
        MIN((
             (Func<string, DateTime?>)
             (input_parameter =>
                { DateTime dt_result;
                  return
                     DateTime.TryParse(input_parameter, out dt_result)
                       ?(DateTime?) dt_result : (DateTime?) null;
                }
             )
             ) (dt)) AS start_time,
        DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
        user,
        des
    FROM @rs0
    GROUP BY user, des;

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

### <a name="using-code-behind"></a>Verwendung von CodeBehind
Um die gleiche Funktionalität im CodeBehind-Abschnitt des U-SQL-Programms zu verwenden, definieren wir die C#-Funktion ToDateTime.

Hier ist der Abschnitt des U-SQL-Basisskripts, in dem wir die erforderlichen Änderungen vorgenommen haben:

```sql
     @rs1 =
        SELECT
          MAX(guid) AS start_id,
          MIN(USQL_Programmability.CustomFunctions.ToDateTime(dt)) AS start_time,
          DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
          user,
          des
          FROM @rs0
        GROUP BY user, des;
```

und die CodeBehind-Funktion

```c#
        static public DateTime? ToDateTime(string dt)
        {
            DateTime dtValue;

            if (!DateTime.TryParse(dt, out dtValue))
                return Convert.ToDateTime(dt);
            else
                return null;
        }
```

## <a name="using-code-behind"></a>Verwendung von CodeBehind
CodeBehind ist ein C#-Programmierbarkeitsabschnitt des U-SQL-Projekts. Grundsätzlich ist CodeBehind eine kompilierte Assembly (DLL), auf die im U-SQL-Skript verwiesen wird. Mit Visual Studio-Tools können Sie einen C#-Programmierbarkeitsabschnitt als Teil des U-SQL-Projekts verwalten und debuggen.

Ein typisches in Visual Studio erstelltes U-SQL-Projekt besteht aus zwei Teilen. Basisskript – Datei mit der Erweiterung **USQL**.

![a](./media/data-lake-analytics-u-sql-programmability-guide/base-script-file.png)


Typische Projektmappe   
![typical-solution-project](./media/data-lake-analytics-u-sql-programmability-guide/typical-solution-project.png)


Der zweite Teil des Projekts wird als CodeBehind bezeichnet – Datei „Script.usql.cs“.  
![code-behind](./media/data-lake-analytics-u-sql-programmability-guide/code-behind.png)

Diese Datei enthält eine Namespace-Standarddefinition für Programmierbarkeitsobjekte.

```c#
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQL_Programmability
{

}
```

Die obige CodeBehind-Vorlage wird automatisch generiert. Diese Datei enthält eine Namespace-Standarddefinition für Programmierbarkeitsobjekte. Während der Ausführung des Projekts wird sie kompiliert, und in einem U-SQL-Basisskript wird darauf verwiesen.

Um mit der Entwicklung von Programmierbarkeitsobjekten zu beginnen, müssen wir eine **öffentliche** Klasse erstellen.

```c#
namespace USQL_Programmability
{
    public class MyClass
    {
        public static string MyFunction(string param1)
        {
            return "my result";
        }

    }

}
```

Die Programmierbarkeitsobjekte können benutzerdefinierte Funktionen (User-Defined Functions) oder **UDF**, benutzerdefinierte Typen (User-Defined Types) oder **UDT, PROZESS, REDUCER** usw. sein.

## <a name="registering-u-sql-assemblies"></a>Registrieren von U-SQL-Assemblys
Das U-SQL-Erweiterbarkeitsmodell hängt stark von der Möglichkeit ab, dass Sie Ihren eigenen benutzerdefinierten Code hinzufügen können. U-SQL ermöglicht Ihnen derzeit, auf einfache Weise eigenen .Net-basierten Code hinzuzufügen – insbesondere C#, aber Sie können auch in anderen .Net-Sprachen wie VB.Net oder F# geschriebenen benutzerdefinierten Code hinzufügen. Sie können sogar eine eigene Runtime für andere Sprachen bereitstellen, obwohl Sie immer noch die Interoperabilität durch eine .Net-Ebene selbst bereitstellen müssen. Wenn Sie wünschen, dass wir eine bestimmte Sprache unterstützen, senden Sie bitte eine Featureanforderung, und/oder hinterlassen Sie hier einen Kommentar: http://aka.ms/adlfeedback.

### <a name="difference-between-code-behind-and-assembly-registration-through-adl-tools-in-visual-studio"></a>Unterschied zwischen CodeBehind und Assemblyregistrierung über ADL-Tools in Visual Studio
Die einfachste Möglichkeit, benutzerdefinierten Code zu nutzen, ist die Verwendung der ADL-Tools für die CodeBehind-Funktionen von Visual Studio.

Wie bereits erwähnt, tragen Sie den benutzerdefinierten Code für das Skript (z.B. „Script.usql“) in die zugehörige CodeBehind-Datei (z.B. „Script.usql.cs“) ein.

![code-behind-example](./media/data-lake-analytics-u-sql-programmability-guide/code-behind-example.jpg)
**Abbildung 1**: CodeBehind-Beispiel in ADL-Tools in VS (klicken Sie zum Vergrößern auf das Bild, Beispielcode finden Sie [hier](https://github.com/Azure/usql/tree/master/Examples/TweetAnalysis))
<br />

Der Vorteil von CodeBehind ist, dass das Tool die folgenden Schritte für Sie ausführt, wenn Sie Ihr Skript übermitteln:  

1. Sie erstellt die Assembly für die CodeBehind-Datei.  

1. Sie fügt dem Skript einen Prolog hinzu, in dem die [CREATE ASSEMBLY](https://msdn.microsoft.com/library/azure/mt763293.aspx)-Anweisung zum Registrieren der Assemblydatei verwendet wird, und verwendet [REFERENCE ASSEMBLY] (https://msdn.microsoft.com/library/azure/mt763294.aspx), um die Assembly in den Kontext des Skripts zu laden.

1. Sie fügt dem Skript einen Epilog hinzu, in dem [DROP ASSEMBLY](https://msdn.microsoft.com/library/azure/mt763295.aspx) verwendet wird, um die vorübergehend registrierte Assembly wieder zu entfernen.

Sie können den generierten Prolog und Epilog anzeigen, wenn Sie das Skript öffnen:

![generated-prologue](./media/data-lake-analytics-u-sql-programmability-guide/generated-prologue.png)

**Abbildung 2**: Automatisch für CodeBehind generierter Prolog und Epilog
<br />

Einige Nachteile von CodeBehind sind:

* Code wird für jede Skriptübermittlung hochgeladen.
* Funktionalität kann nicht mit anderen gemeinsam genutzt werden.

Daher können Sie der Projektmappe eine separate C#-Klassenbibliothek (für U-SQL) hinzufügen (siehe Abbildung 3), den Code entwickeln oder vorhandenen CodeBehind-Code überkopieren (am C#-Code sind keine Änderungen erforderlich, siehe Abbildung 4) und dann mit der Menüoption „Assembly registrieren“ des Projekts die Assembly registrieren (siehe Schritt 1 in Abbildung 5).

![creating-project](./media/data-lake-analytics-u-sql-programmability-guide/creating-project.png)
**Abbildung 3**: Erstellen eines U-SQL-C#-Codeprojekts  
<br />

![class-library](./media/data-lake-analytics-u-sql-programmability-guide/class-library.png)
**Abbildung 4**: U-SQL-C#-Klassenbibliothek neben der CodeBehind-Datei  
<br />

![register-code](./media/data-lake-analytics-u-sql-programmability-guide/register-code.png)
**Abbildung 5**: Registrieren des U-SQL-C#-Codeprojekts
<br />

Im Dialogfeld für die Registrierung (siehe Schritt 2 in Abbildung 5) können Sie angeben, wo Sie die Assembly registrieren möchten (Data Lake Analytics-Konto, Datenbank), und wie Sie sie benennen möchten (der lokale Assemblypfad wird vom Tool eingetragen). Es bietet auch eine Option zum erneuten Registrieren einer bereits registrierten Assembly und zwei Optionen zum Hinzufügen zusätzlicher Abhängigkeiten:

* *Verwaltete Abhängigkeiten*: Zeigt die ebenfalls benötigten verwalteten Assemblys an. Jede ausgewählte Assembly wird einzeln registriert, und Skripts können darauf verweisen. Sie verwenden diese für andere .Net-Assemblys.

* *Zusätzliche Dateien*: Sie können zusätzliche Ressourcendateien hinzufügen, die die Assembly benötigt. Diese werden zusammen mit der Assembly registriert und automatisch geladen, wenn auf die Assembly verwiesen wird. Sie verwenden diese für Konfigurationsdateien, native Assemblys, Runtimes in anderen Sprache und ihre Ressourcen usw.

Wir nutzen beide Optionen in den folgenden Beispielen. Der [neueste Blogeintrag zur Bildverarbeitung](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/18/introducing-image-processing-in-u-sql/) ist ein weiteres Beispiel für die Verwendung einer vordefinierten Assembly, die diese Optionen zur Registrierung verwenden kann.

Jetzt können Sie von einem beliebigen U-SQL-Skript aus, das über Berechtigungen für die Datenbank der registrierten Assemblys verfügt, auf die registrierten Assemblys verweisen (siehe Code im U-SQL-Skript in Abbildung 4). Sie müssen einen Verweis auf jede separat registrierte Assembly hinzufügen. Die zusätzlichen Ressourcendateien werden automatisch bereitgestellt. Dieses Skript sollte keine CodeBehind-Datei für den Code in referenzierten Assemblys mehr aufweisen, die CodeBehind-Datei kann jedoch immer noch anderen Code bereitstellen.

### <a name="registering-assemblies-via-adl-tools-in-visual-studio-and-in-u-sql-scripts"></a>Registrieren von Assemblys über ADL-Tools in Visual Studio und U-SQL-Skripts
So wie die ADL-Tools in Visual Studio das Registrieren einer Assembly leicht machen, ist dies auch mit einem Skript möglich (in gleicher Weise, wie die Tools es für Sie übernehmen), wenn Sie z.B. auf einer anderen Plattform entwickeln und bereits Assemblys kompiliert haben, die Sie hochladen und registrieren möchten. Im Wesentlichen befolgen Sie hierzu die folgenden Schritte:

1. Sie laden die Assembly-DLL und alle ebenfalls erforderlichen Nicht-System-DLLs und Ressourcendateien an einen Speicherort hoch, den Sie in Ihrem Azure Data Lake Storage-Konto auswählen, oder sogar in einem Microsoft Azure Blob Store-Konto, das mit Ihrem Azure Data Lake-Konto verknüpft ist. Sie können eines der vielen verfügbaren Uploadtools verwenden (z.B. Powershell-Befehle, den ADL Tool Data Lake Explorer von Visual Studio, Ihren bevorzugten SDK-Hochladebefehl oder Hochladen über das Azure-Portal).

1. Nachdem Sie die DLLs hochgeladen haben, registrieren Sie sie mit den [CREATE ASSEMBLY](https://msdn.microsoft.com/library/azure/mt763293.aspx)-Anweisungen.

Wir verwenden diesen Ansatz in dem folgenden räumlichen Beispiel.

### <a name="registering-assemblies-that-use-other-net-assemblies-based-on-the-json-and-xml-sample-library"></a>Registrieren von Assemblys, die andere .Net-Assemblys verwenden (basierend auf der JSON- und XML-Beispielbibliothek)
Unsere [U-SQL-Github-Website](https://github.com/Azure/usql/) bietet eine Reihe von freigegebenen Beispielassemblys, die Sie verwenden dürfen. Eine der Assemblys, aufgerufen mit [Microsoft.Analytics.Samples.Formats](https://github.com/Azure/usql/tree/master/Examples/DataFormats), bietet Extraktoren, Funktionen und Outputter zur Verarbeitung von JSON- und XML-Dokumenten. Die Microsoft.Analytics.Samples.Formats-Assembly ist von zwei vorhandenen domänenspezifischen Assemblys abhängig, um die JSON- bzw. XML-Verarbeitung durchzuführen. Sie verwendet die [Newtonsoft Json.Net](http://www.newtonsoft.com/)-Bibliothek für die Verarbeitung der JSON-Dokumente und die [System.Xml](https://msdn.microsoft.com/data/bb291078.aspx)-Assembly für die XML-Verarbeitung. Wir verwenden sie, um zu zeigen, wie sie registriert wird, und wie die Assemblys in unseren Skripts verwendet werden.

Erst laden wir das [Visual Studio-Projekt](https://github.com/Azure/usql/tree/master/Examples/DataFormats) in unsere lokale Entwicklungsumgebung herunter (z.B. mittels einer lokalen Kopie mit dem GitHub-Tool für Windows). Anschließend öffnen wir die Projektmappe in Visual Studio, klicken mit der rechten Maustaste wie oben erläutert auf das Projekt, um die Assembly zu registrieren. Diese Assembly besitzt zwei Abhängigkeiten, doch wir müssen nur die Abhängigkeit „Newtonsoft“ einbeziehen, da „System.xml“ bereits in Azure Data Lake verfügbar ist (es muss jedoch explizit darauf verwiesen werden). Abbildung 6 zeigt, wie wir die Assembly benennen (beachten Sie, dass Sie auch einen anderen Namen ohne Punkte auswählen können) und die Newtonsoft-DLL hinzufügen. Jede der beiden Assemblys wird nun einzeln in der angegebenen Datenbank (z.B. JSONBlog) registriert.

![register-assembly](./media/data-lake-analytics-u-sql-programmability-guide/register-assembly.png)

**Abbildung 6**: Registrieren der Microsoft.Analytics.Samples.Formats-Assembly aus Visual Studio
<br />

Wenn Sie oder andere, für die Sie die registrierten Assemblys durch Gewähren von Lesezugriff auf die Datenbank freigegeben haben, jetzt die JSON-Funktion in ihren eigenen Skripts verwenden möchten, fügen Sie die folgenden zwei Verweise Ihrem Skript hinzu:

```
REFERENCE ASSEMBLY JSONBlog.[NewtonSoft.Json];
REFERENCE ASSEMBLY JSONBlog.[Microsoft.Analytics.Samples.Formats];
```

Wenn Sie die XML-Funktionalität verwenden möchten, fügen Sie der registrierten Assembly einen Systemassemblyverweis und einen Verweis hinzu:

```
REFERENCE SYSTEM ASSEMBLY [System.Xml];
REFERENCE ASSEMBLY JSONBlog.[Microsoft.Analytics.Samples.Formats];
```

Weitere Informationen zur Verwendung der JSON-Funktionen finden Sie in [diesem Blogbeitrag](https://blogs.msdn.microsoft.com/mrys/?p=755).

### <a name="registering-assemblies-that-use-native-c-assemblies-using-the-sql-server-2016-spatial-type-assembly-from-the-feature-pack"></a>Registrieren von Assemblys, die native C++-Assemblys verwenden (unter Verwendung der SQL Server 2016 Spatial Type-Assembly aus dem Feature Pack)
Jetzt untersuchen wir ein etwas anderes Szenario. Nehmen wir an, dass die zu verwendende Assembly von Code abhängig ist, der nicht auf .Net basiert; insbesondere ist die Assembly von einer nativen C++-Assembly abhängig. Ein Beispiel einer solchen Assembly ist die Assembly [Microsoft.SqlServer.Types.dll](https://www.microsoft.com/download/details.aspx?id=52676) vom SQL Server-Typ. Sie stellt .Net-basierte Implementierungen der SQL Server- HierarchyID-, -Geometrie- und Geografietypen bereit, die von clientseitigen SQL Server-Anwendungen für die Behandlung der SQL Server-Typen verwendet werden (sie war auch ursprünglich – vor dem Release von SQL Server 2016 – die Assembly, die die Implementierung für die räumlichen SQL Server-Typen bereitstellte).

Sehen Sie, wie Sie diese Assembly in U-SQL registrieren!

Zuerst laden wir die Assembly aus dem [SQL Server 2016 FeaturePack](https://www.microsoft.com/download/details.aspx?id=52676) herunter und installieren sie. Wählen Sie die 64-Bit-Version des Installationsprogramms („ENU\x64\SQLSysClrTypes.msi“), denn wir möchten sicherstellen, dass wir die 64-Bit-Version der Bibliotheken haben.

Das Installationsprogramm installiert die verwaltete Assembly „Microsoft.SqlServer.Types.dll“ in „C:\Program Files (x86)\Microsoft SQL Server\130\SDK\Assemblies“ und die native Assembly „SqlServerSpatial130.dll“ in „\Windows\System32“\. Nun laden wir die Assemblys in unseren Azure Data Lake Store hoch (z.B. in einen Ordner mit dem Namen „/upload/asm/spatial“).

Da das Installationsprogramm die native Bibliothek im Systemordner „c:\Windows\System32“ installiert hat, müssen wir sicherstellen, dass wir entweder „SqlServerSpatial130.dll“ vor dem Hochladen aus diesem Ordner kopieren, oder dass das von uns verwendete Tool keine [Dateisystemumleitung](https://msdn.microsoft.com/library/windows/desktop/aa384187(v=vs.85).aspx) des Systemordners ausführt. Wenn Sie sie z.B. mit dem aktuellen ADL-Datei-Explorer von Visual Studio hochladen möchten, müssen Sie zuerst die Datei in ein anderes Verzeichnis kopieren, andernfalls – zum Zeitpunkt des Verfassens dieses Blog – wird die 32-Bit-Version hochgeladen (da Visual Studio eine 32-Bit-Anwendung ist, wo die Dateisystemumleitung in das ADL-Upload-Dateiauswahlfenster erfolgt), und beim Ausführen eines U-SQL-Skripts, das Aufrufe in native Assembly ausführt, wird folgender (interner) Fehler zur Runtime gemeldet:

*Interne Ausnahme von Benutzerausdruck: Es wurde versucht, ein Programm mit einem falschen Format zu laden. (Ausnahme von HRESULT:0x8007000B)*

Nach dem Hochladen der zwei Assemblydateien registrieren wir sie jetzt mit dem folgenden Skript in einem Datenbank-SQLSpatial:

```sql
DECLARE @ASSEMBLY_PATH string = "/upload/asm/spatial/";
DECLARE @SPATIAL_ASM string = @ASSEMBLY_PATH+"Microsoft.SqlServer.Types.dll";
DECLARE @SPATIAL_NATIVEDLL string = @ASSEMBLY_PATH+"SqlServerSpatial130.dll";

CREATE DATABASE IF NOT EXISTS SQLSpatial;
USE DATABASE SQLSpatial;

DROP ASSEMBLY IF EXISTS SqlSpatial;
CREATE ASSEMBLY SqlSpatial
FROM @SPATIAL_ASM
WITH ADDITIONAL_FILES =
     (
         @SPATIAL_NATIVEDLL
     );
```

In diesem Fall registrieren wir nur eine U-SQL-Assembly und beziehen die native Assembly als Zeichenfolgenabhängigkeit in die U-SQL-Assembly ein. Um die räumlichen Assemblys zu verwenden, müssen wir nur auf die U-SQL-Assembly verweisen, und die zusätzliche Datei wird automatisch der Assembly zur Verfügung gestellt. Hier ist ein einfaches Beispielskript, das die räumliche Assembly verwendet:

```sql
REFERENCE SYSTEM ASSEMBLY [System.Xml];
REFERENCE ASSEMBLY SQLSpatial.SqlSpatial;

USING Geometry = Microsoft.SqlServer.Types.SqlGeometry;
USING Geography = Microsoft.SqlServer.Types.SqlGeography;
USING SqlChars = System.Data.SqlTypes.SqlChars;

@spatial =
    SELECT * FROM (VALUES
                   // The following expression is not using the native DDL
                   ( Geometry.Point(1.0,1.0,0).ToString()),    
                   // The following expression is using the native DDL
                   ( Geometry.STGeomFromText(new SqlChars("LINESTRING (100 100, 20 180, 180 180)"), 0).ToString())
                  ) AS T(geom);

OUTPUT @spatial
TO "/output/spatial.csv"
USING Outputters.Csv();
```

Die SQL-Typen-Bibliothek hat eine Abhängigkeit von der System.XML-Assembly, daher müssen wir darauf verweisen. Darüber hinaus verwenden einige der Methoden die System.Data.SqlTypes-Typen anstelle der integrierten C#-Typen. Da „System.Data“ bereits standardmäßig enthalten ist, reicht es, auf den erforderlichen SQL-Typ zu verweisen. Den obigen Code finden Sie auf unserer [Github-Website](https://github.com/Azure/usql/tree/master/Examples/SQLSpatialExample).


### <a name="assembly-versioning-and-other-points"></a>Assemblyversionierung und weitere Punkte
U-SQL verwendet derzeit die .Net Framework-Version 4.5. Stellen Sie darum bitte sicher, dass Ihre eigenen Assemblys mit dieser Runtimeversion kompatibel sind!

Wie bereits oben erwähnt, führt U-SQL Code in einem 64-Bit-Format (x64) aus. Stellen Sie darum bitte sicher, dass der Code zur Ausführung unter x64 kompiliert wird. Andernfalls erhalten Sie die obige Fehlermeldung zu falschem Format!

Jede hochgeladene Assembly-DLL, Ressourcendatei wie z.B. eine andere Runtime, eine native Assembly oder Konfigurationsdateien usw. können maximal 400MB groß sein, und die Gesamtgröße der entweder über DEPLOY RESOURCE oder über Assemblys, auf die verwiesen wird, bereitgestellten Ressourcen und deren zusätzlicher Dateien darf 3GB nicht überschreiten.

Beachten Sie schließlich, dass jede U-SQL-Datenbank nur eine Version einer bestimmten angegebenen Assembly enthalten kann. Wenn Sie z.B. sowohl die Version 7 als auch 8 der NewtonSoft-Json.Net-Bibliothek benötigen, müssen Sie diese in zwei verschiedenen Datenbanken registrieren. Darüber hinaus kann jedes Skript nur auf eine Version einer bestimmten Assembly-DLL verweisen. In diesem Punkt folgt U-SQL der C#-Semantik zur Assemblyverwaltung und -versionierung!


## <a name="user-defined-functions---udf"></a>Benutzerdefinierte Funktionen – (UDF, User Defined Functions)
Benutzerdefinierte Funktionen bzw. UDF von U-SQL sind Programmierungsroutinen, die Parameter annehmen, eine Aktion wie z.B. eine komplexe Berechnung ausführen und das Ergebnis dieser Aktion als Wert zurückgeben. Der Rückgabewert der benutzerdefinierten Funktion kann nur ein einzelner Skalarwert sein. Eine U-SQL-UDF kann im U-SQL-Basisskript wie jede andere skalare Funktion in C# aufgerufen werden.

Benutzerdefinierte Funktionen in U-SQL müssen als öffentlich und **statisch** initialisiert werden.

```c#
        public static string MyFunction(string param1)
        {
            return "my result";
        }
```

Zuerst sehen wir uns das einfache Beispiel zum Erstellen einer UDF an.

In diesem Anwendungsfall müssen wir den Geschäftszeitraum bestimmen – Geschäftsquartal und Geschäftsmonat der ersten Anmeldung für den Benutzer. Der erste Geschäftsmonat des Jahres in unserem Szenario ist Juni.

Zum Berechnen der Geschäftszeitraums führen wir die folgende C#-Funktion ein.

```c#
        public static string GetFiscalPeriod(DateTime dt)
        {
            int FiscalMonth=0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter=0;
            if (FiscalMonth >=1 && FiscalMonth<=3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return "Q" + FiscalQuarter.ToString() + ":P" + FiscalMonth.ToString();
        }
```

Sie berechnet einfach Geschäftsmonat und -quartal und gibt einen Zeichenfolgenwert zurück. Für Juni – erster Monat des ersten Geschäftsquartals – „Q1:P1“, für Juli „Q1:P2“ usw.

Dies ist eine reguläre C#-Funktion, die wir für unser U-SQL-Projekt verwenden möchten.

So sieht der CodeBehind-Abschnitt in diesem Szenario aus:

```c#
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQL_Programmability
{
    public class CustomFunctions
    {
        public static string GetFiscalPeriod(DateTime dt)
        {
            int FiscalMonth=0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter=0;
            if (FiscalMonth >=1 && FiscalMonth<=3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return "Q" + FiscalQuarter.ToString() + ":" + FiscalMonth.ToString();
        }

    }

}
```

Jetzt rufen wir diese Funktion aus dem U-SQL-Basisskript heraus auf. Zu diesem Zweck müssen wir einen vollqualifizierten Namen der Funktion angeben, einschließlich Namespace – NameSpace.Class.Function(parameter).

```sql
    USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)
```

Hier ist das eigentlich U-SQL-Basisskript.

```sql
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        guid Guid,
        dt DateTime,
        user String,
        des String
    FROM @input_file USING Extractors.Tsv();

DECLARE @default_dt DateTime = Convert.ToDateTime("06/01/2016");

@rs1 =
    SELECT
        MAX(guid) AS start_id,
        MIN(dt) AS start_time,
        MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
        MIN(USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)) AS start_fiscalperiod,
        user,
        des
    FROM @rs0
    GROUP BY user, des;

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

Hier ist die Ausgabedatei der Ausführung des Skripts.

```
0d8b9630-d5ca-11e5-8329-251efa3a2941,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User1",""

20843640-d771-11e5-b87b-8b7265c75a44,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User2",""

301f23d2-d690-11e5-9a98-4b4f60a1836f,2016-02-11T09:01:33.9720000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User3",""
```

Dieses Beispiel zeigt eine einfache Verwendung einer Inline-UDF in U-SQL.

### <a name="keeping-state-between-udf-invocations"></a>Verwaltung des Zustands zwischen UDF-Aufrufen
U-SQL-C#-Programmierbarkeitsobjekte können bei Nutzung der Interaktivität über globale CodeBehind-Variablen noch ausgereifter sein. Betrachten Sie den folgenden Geschäftsanwendungsfall:

In großen Organisationen können Benutzer zwischen Varianten interner Anwendungen wechseln. Dies kann Microsoft Dynamics CRM, PowerBI usw. umfassen. Kunden möchten in einer Telemetrieanalyse ermitteln, wie Benutzer zwischen verschiedenen Anwendungen wechseln, was der Nutzungstrend ist usw. Das Endziel für das Geschäft ist die Optimierung der Anwendungsnutzung. Kombinieren Sie möglichst verschiedene Anwendungen oder bestimmte Anmeldungsroutinen.

Um dieses Ziel zu erreichen, müssen wir eine Sitzungs-IDs bestimmen und die Verzögerung zwischen den letzten Sitzungen.

Wir müssen eine vorherige Anmeldung finden und diese Anmeldung dann allen Sitzungen zuweisen, die bei der gleichen Anwendung generiert werden. Die erste Herausforderung besteht darin, dass das U-SQL-Basisskript uns nicht ermöglicht, Berechnungen auf eine bereits berechnete Spalte mit LAG-Funktion anzuwenden. Die zweite Herausforderung ist, dass wir die bestimmte Sitzung für alle Sitzungen im gleichen Zeitraum halten müssen.

Um dieses Problem zu beheben, verwenden wir eine globale Variable im CodeBehind-Abschnitt – `static public string globalSession;`.

Diese globale Variable wird während der Ausführung des Skripts auf das gesamte Rowset angewendet.

Hier ist der CodeBehind-Abschnitt unseres U-SQL-Programms.

```c#
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQLApplication21
{
    public class UserSession
    {
        static public string globalSession;
        static public string StampUserSession(string eventTime, string PreviousRow, string Session)
        {

            if (!string.IsNullOrEmpty(PreviousRow))
            {
                double timeGap = Convert.ToDateTime(eventTime).Subtract(Convert.ToDateTime(PreviousRow)).TotalMinutes;
                if (timeGap <= 60)
                    return Session;
                else
                    return Guid.NewGuid().ToString();
            }
            else
                return Guid.NewGuid().ToString();

        }

        static public string getStampUserSession(string Session)
        {
            if (Session != globalSession && !string.IsNullOrEmpty(Session))
            {
                globalSession = Session;
            }
            return globalSession;
        }

    }
}
```

Die globale Variable `static public string globalSession;` wird innerhalb der `getStampUserSession`-Funktion verwendet und immer dann erneut initialisiert, wenn der Parameter „Session“ geändert wird.

U-SQL-Basisskript

```sql
DECLARE @in string = @"\UserSession\test1.tsv";
DECLARE @out1 string = @"\UserSession\Out1.csv";
DECLARE @out2 string = @"\UserSession\Out2.csv";
DECLARE @out3 string = @"\UserSession\Out3.csv";

@records =
    EXTRACT DataId string,
            EventDateTime string,           
            UserName string,
            UserSessionTimestamp string

    FROM @in
    USING Extractors.Tsv();

@rs1 =
    SELECT EventDateTime,
           UserName,
LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC) AS prevDateTime,          
           string.IsNullOrEmpty(LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)) AS Flag,           
           USQLApplication21.UserSession.StampUserSession
           (
               EventDateTime,
               LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC),
               LAG(UserSessionTimestamp, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)
           )
           AS UserSessionTimestamp
    FROM @records;

@rs2 =
    SELECT EventDateTime,
           UserName,
           LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC) AS prevDateTime,
           string.IsNullOrEmpty(LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)) AS Flag,
           USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp) AS UserSessionTimestamp
    FROM @rs1
    WHERE UserName != "UserName";

OUTPUT @rs2
TO @out2
ORDER BY UserName, EventDateTime ASC
USING Outputters.Csv();
```

Funktion `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` wird hier während der zweiten Berechnung des Arbeitsspeicherrowsets aufgerufen. Sie übernimmt die `UserSessionTimestamp`-Spalte und gibt den Wert zurück, bis `UserSessionTimestamp` geändert wird.

Die Ausgabedatei

```
"2016-02-19T07:32:36.8420000-08:00","User1",,True,"72a0660e-22df-428e-b672-e0977007177f"
"2016-02-17T11:52:43.6350000-08:00","User2",,True,"4a0cd19a-6e67-4d95-a119-4eda590226ba"
"2016-02-17T11:59:08.8320000-08:00","User2","2016-02-17T11:52:43.6350000-08:00",False,"4a0cd19a-6e67-4d95-a119-4eda590226ba"
"2016-02-11T07:04:17.2630000-08:00","User3",,True,"51860a7a-1610-4f74-a9ea-69d5eef7cd9c"
"2016-02-11T07:10:33.9720000-08:00","User3","2016-02-11T07:04:17.2630000-08:00",False,"51860a7a-1610-4f74-a9ea-69d5eef7cd9c"
"2016-02-15T21:27:41.8210000-08:00","User3","2016-02-11T07:10:33.9720000-08:00",False,"4d2bc48d-bdf3-4591-a9c1-7b15ceb8e074"
"2016-02-16T05:48:49.6360000-08:00","User3","2016-02-15T21:27:41.8210000-08:00",False,"dd3006d0-2dcd-42d0-b3a2-bc03dd77c8b9"
"2016-02-16T06:22:43.6390000-08:00","User3","2016-02-16T05:48:49.6360000-08:00",False,"dd3006d0-2dcd-42d0-b3a2-bc03dd77c8b9"
"2016-02-17T16:29:53.2280000-08:00","User3","2016-02-16T06:22:43.6390000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-17T16:39:07.2430000-08:00","User3","2016-02-17T16:29:53.2280000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-17T17:20:39.3220000-08:00","User3","2016-02-17T16:39:07.2430000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-19T05:23:54.5710000-08:00","User3","2016-02-17T17:20:39.3220000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T05:48:37.7510000-08:00","User3","2016-02-19T05:23:54.5710000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T06:40:27.4830000-08:00","User3","2016-02-19T05:48:37.7510000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T07:27:37.7550000-08:00","User3","2016-02-19T06:40:27.4830000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T19:35:40.9450000-08:00","User3","2016-02-19T07:27:37.7550000-08:00",False,"3f385f0b-3e68-4456-ac74-ff6cef093674"
"2016-02-20T00:07:37.8250000-08:00","User3","2016-02-19T19:35:40.9450000-08:00",False,"685f76d5-ca48-4c58-b77d-bd3a9ddb33da"
"2016-02-11T09:01:33.9720000-08:00","User4",,True,"9f0cf696-c8ba-449a-8d5f-1ca6ed8f2ee8"
"2016-02-17T06:30:38.6210000-08:00","User4","2016-02-11T09:01:33.9720000-08:00",False,"8b11fd2a-01bf-4a5e-a9af-3c92c4e4382a"
"2016-02-17T22:15:26.4020000-08:00","User4","2016-02-17T06:30:38.6210000-08:00",False,"4e1cb707-3b5f-49c1-90c7-9b33b86ca1f4"
"2016-02-18T14:37:27.6560000-08:00","User4","2016-02-17T22:15:26.4020000-08:00",False,"f4e44400-e837-40ed-8dfd-2ea264d4e338"
"2016-02-19T01:20:31.4800000-08:00","User4","2016-02-18T14:37:27.6560000-08:00",False,"2136f4cf-7c7d-43c1-8ae2-08f4ad6a6e08"
```

Dieses Beispiel zeigt einen etwas komplizierteren Anwendungsfall, wenn wir eine globale Variable im CodeBehind-Abschnitt verwenden, die auf den gesamten Arbeitsspeicherrowset angewendet wird.

## <a name="using-user-defined-types---udt"></a>Verwenden benutzerdefinierter Typen – (UDTs, Using User-Defined Types)
Benutzerdefinierte Typen oder UDTs sind ein weiteres Programmierbarkeitsfeature von U-SQL. Ein U-SQL-UDT verhält sich wie ein regulärer benutzerdefinierter C#-Typ. C# ist eine stark typisierte Sprache, die die Verwendung integrierter und benutzerdefinierter Typen ermöglicht.

U-SQL kann beliebige UDTs nicht implizit serialisieren/deserialisieren, während diese zwischen Scheitelpunkten in Rowsets übergeben werden. Daher muss der Benutzer einen expliziten Formatierer bereitstellen, der die IFormatter-Schnittstelle verwendet. U-SQL erhält damit die Methoden zum Serialisieren und Deserialisieren des UDT. 

> [!NOTE]
> Die in U-SQL integrierten Extraktoren und Outputters können derzeit UDT-Daten nicht in/aus Dateien serialisieren/deserialisieren – selbst wenn der IFormatter festgelegt wurde.  Daher muss der Benutzer beim Schreiben von UDT-Daten in eine Datei mit der OUTPUT-Anweisung oder beim Lesen mit einem Extraktor diese als Zeichenfolgen-oder Bytearray übergeben und den Code zur Serialisierung und Deserialisierung explizit aufrufen (z.B. die ToString()-Methode des UDT). Benutzerdefinierte Extraktoren und Outputter können hingegen UDTs lesen und schreiben.

Wenn Sie versuchen, einen UDT in EXTRACTOR oder OUTPUTTER (außerhalb des vorherigen SELECT) zu verwenden,

```sql
@rs1 =
    SELECT MyNameSpace.Myfunction_Returning_UDT(filed1) AS myfield
    FROM @rs0;

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

erhalten Sie die folgende Fehlermeldung:

```
    Error    1    E_CSC_USER_INVALIDTYPEINOUTPUTTER: Outputters.Text was used to output column myfield of type
    MyNameSpace.Myfunction_Returning_UDT.

    Description:

    Outputters.Text only supports built-in types.

    Resolution:

    Implement a custom outputter that knows how to serialize this type or call a serialization method on the type in
    the preceding SELECT.    C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\
    USQL-Programmability\Types.usql    52    1    USQL-Programmability
```

Für die Zusammenarbeit von UDT und Outptutter müssen wir den UDT entweder mit der ToString()-Methode in eine Zeichenfolge serialisieren, oder wir erstellen einen benutzerdefinierten Outputter.

UDTs können derzeit nicht in der GROUP BY-Klausel verwendet werden. Wenn der UDT in der GROUP BY-Klausel verwendet wird, wird der folgende Fehler ausgelöst:

```
    Error    1    E_CSC_USER_INVALIDTYPEINCLAUSE: GROUP BY doesn't support type MyNameSpace.Myfunction_Returning_UDT
    for column myfield

    Description:

    GROUP BY doesn't support UDT or Complex types.

    Resolution:

    Add a SELECT statement where you can project a scalar column that you want to use with GROUP BY.
    C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\USQL-Programmability\Types.usql
    62    5    USQL-Programmability
```

Folgendermaßen definieren wir einen UDT:

* Hinzufügen der folgenden Namespaces

```c#
    using Microsoft.Analytics.Interfaces
    using System.IO;
```

* `Microsoft.Analytics.Interfaces` ist für die UDT-Schnittstellen erforderlich, und `System.IO` könnte möglicherweise erforderlich sein, um die IFormatter-Schnittstelle definieren.

* Definieren des benutzerdefinierten Typs mit dem SqlUserDefinedType-Attribut

**SqlUserDefinedType** wird verwendet, um in U-SQL die Definition eines Typs in einer Assembly als benutzerdefinierten Typ (UDT) zu markieren. Die Eigenschaften des Attributs spiegeln die physischen Merkmale des UDT wider. Diese Klasse kann nicht vererbt werden.

SqlUserDefinedType ist ein erforderliches Attribut für die UDT-Definition.

Der Konstruktor der Klasse  

* SqlUserDefinedTypeAttribute (Typformatierung)

* Typformatierung – erforderlicher Parameter zur Definition einer UDT-Formatierung. Ein spezifischer Typ der `IFormatter`-Schnittstelle muss hier übergeben werden.

```c#
    [SqlUserDefinedType(typeof(MyTypeFormatter))]
      public class MyType
              {
             …
           }
```

* Ein typischer UDT benötigt auch die Definition der IFormatter-Schnittstelle.

```c#
       public class MyTypeFormatter : IFormatter<MyType>
        {
            public void Serialize(MyType instance, IColumnWriter writer,
                           ISerializationContext context)
            {
        …
            }

            public MyType Deserialize(IColumnReader reader,
                                  ISerializationContext context)
            {
        …
            }
        }
```

`IFormatter`-Schnittstelle zur Serialisierung und Deserialisierung eines Objektgraphs mit dem Stammtyp von \<typeparamref Name = "T" >.

\<Typeparam Name = "T"> – der Stammtyp für Serialisierung und Deserialisierung des Objektgraphs.

* „Deserialize“ – Deserialisieren der Daten in dem angegebenen Datenstrom und Wiederherstellen des Graphs von Objekten.

* „Serialize“ – Serialisieren eines Objekts oder Graphs von Objekten mit dem angegebenen Stamm in den angegebenen Datenstrom.

`MyType`-Instanz – die Instanz des Typs  
`IColumnWriter`-Writer/`IColumnReader`-Reader – der zugrunde liegende Spaltendatenstrom.  
`ISerializationContext`-Kontext – Aufzählung, die eine Gruppe von Flags definiert, die den Quell- oder Zielkontext für den Datenstrom während der Serialisierung angibt. 
 
   * *Intermediate* – gibt an, dass der Quell- oder Zielkontext kein permanenter Speicher ist.

   * *Persistence* – gibt an, dass der Quell- oder Zielkontext ein permanenter Speicher ist.

Als regulärer C#-Typ kann die U-SQL-UDT-Definition u.a. Außerkraftsetzungen für Operatoren wie +/==/!=, usw. enthalten. Kann statische Methoden usw. enthalten. Wenn wir diesen UDT z.B. als Parameter für die U-SQL-MIN-Aggregatfunktion verwenden, müssen wir die Außerkraftsetzung des <-Operators definieren.

Weiter oben in dieser Anleitung veranschaulichten wir ein Beispiel für die Identifizierung des Geschäftszeitraums ab einem bestimmten Datum im Format Qn:Pn (Q1:P10). Das folgende Beispiel zeigt das Definieren eines benutzerdefinierten Typs für Geschäftszeitraumwerte.

CodeBehind-Abschnitt mit benutzerdefiniertem UDT und IFormatter-Schnittstelle dafür:

```c#
        [SqlUserDefinedType(typeof(FiscalPeriodFormatter))]
        public struct FiscalPeriod
        {
            public int Quarter { get; private set; }

            public int Month { get; private set; }

            public FiscalPeriod(int quarter, int month):this()
            {
                this.Quarter = quarter;
                this.Month = month;
            }

            public override bool Equals(object obj)
            {
                if (ReferenceEquals(null, obj))
                {
                    return false;
                }

                return obj is FiscalPeriod && Equals((FiscalPeriod)obj);
            }

            public bool Equals(FiscalPeriod other)
            {
return this.Quarter.Equals(other.Quarter) && this.Month.Equals(other.Month);
            }

            public bool GreaterThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) > 0 || this.Month.CompareTo(other.Month) > 0;
            }

            public bool LessThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) < 0 || this.Month.CompareTo(other.Month) < 0;
            }

            public override int GetHashCode()
            {
                unchecked
                {
                    return (this.Quarter.GetHashCode() * 397) ^ this.Month.GetHashCode();
                }
            }

            public static FiscalPeriod operator +(FiscalPeriod c1, FiscalPeriod c2)
            {
return new FiscalPeriod((c1.Quarter + c2.Quarter) > 4 ? (c1.Quarter + c2.Quarter)-4 : (c1.Quarter + c2.Quarter), (c1.Month + c2.Month) > 12 ? (c1.Month + c2.Month) - 12 : (c1.Month + c2.Month));
            }

            public static bool operator ==(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.Equals(c2);
            }

            public static bool operator !=(FiscalPeriod c1, FiscalPeriod c2)
            {
                return !c1.Equals(c2);
            }
            public static bool operator >(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.GreaterThan(c2);
            }
            public static bool operator <(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.LessThan(c2);
            }
            public override string ToString()
            {
                return (String.Format("Q{0}:P{1}", this.Quarter, this.Month));
            }

        }

        public class FiscalPeriodFormatter : IFormatter<FiscalPeriod>
        {
public void Serialize(FiscalPeriod instance, IColumnWriter writer, ISerializationContext context)
            {
                using (var binaryWriter = new BinaryWriter(writer.BaseStream))
                {
                    binaryWriter.Write(instance.Quarter);
                    binaryWriter.Write(instance.Month);
                    binaryWriter.Flush();
                }
            }

public FiscalPeriod Deserialize(IColumnReader reader, ISerializationContext context)
            {
                using (var binaryReader = new BinaryReader(reader.BaseStream))
                {
var result = new FiscalPeriod(binaryReader.ReadInt16(), binaryReader.ReadInt16());
                    return result;
                }
            }
        }
```

Der definierte Typ umfasst zwei Zahlen – „Quarter“ und „Month“. Die Operatoren ==/!=/>/< und die statische Methode ToString() sind hier definiert.

Wie bereits erwähnt, kann der UDT im SELECT-Ausdruck verwendet werden, aber ohne benutzerdefinierte Serialisierung nicht im OUTPUTTER/EXTRACTOR. Er muss entweder mit ToString() als Zeichenfolge serialisiert oder mit benutzerdefiniertem OUTPUTTER/EXTRACTOR verwendet werden.

Jetzt betrachten wird die Verwendung des UDT. Im CodeBehind-Abschnitt änderten wir unsere GetFiscalPeriod-Funktion in

```c#
        public static FiscalPeriod GetFiscalPeriodWithCustomType(DateTime dt)
        {
            int FiscalMonth = 0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter = 0;
            if (FiscalMonth >= 1 && FiscalMonth <= 3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return new FiscalPeriod(FiscalQuarter, FiscalMonth);
        }
```

Wie Sie sehen können, wird der Wert unseres FiscalPeriod-Typs zurückgegeben.

Im Folgenden sehen Sie, wie wir ihn im U-SQL-Basisskript weiterverwenden. Dieses Beispiel zeigt verschiedene Möglichkeiten des UDT-Aufrufs im U-SQL-Skript.

```sql
DECLARE @input_file string = @"c:\work\cosmos\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"c:\work\cosmos\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        guid string,
        dt DateTime,
        user String,
        des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT guid AS start_id,
           dt,
           DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).Quarter AS fiscalquarter,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).Month AS fiscalmonth,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt) + new USQL_Programmability.CustomFunctions.FiscalPeriod(1,7) AS fiscalperiod_adjusted,
           user,
           des
    FROM @rs0;

@rs2 =
    SELECT start_id,
           dt,
           DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
           fiscalquarter,
           fiscalmonth,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).ToString() AS fiscalperiod,
       
       // This user-defined type was created in the prior SELECT.  Passing the UDT to this subsequent SELECT would have failed if the UDT was not annotated with an IFormatter.
           fiscalperiod_adjusted.ToString() AS fiscalperiod_adjusted,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 TO @output_file USING Outputters.Text();
```

Vollständiger CodeBehind-Abschnitt:

```c#
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.IO;

namespace USQL_Programmability
{
    public class CustomFunctions
    {
        static public DateTime? ToDateTime(string dt)
        {
            DateTime dtValue;

            if (!DateTime.TryParse(dt, out dtValue))
                return Convert.ToDateTime(dt);
            else
                return null;
        }

        public static FiscalPeriod GetFiscalPeriodWithCustomType(DateTime dt)
        {
            int FiscalMonth = 0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter = 0;
            if (FiscalMonth >= 1 && FiscalMonth <= 3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return new FiscalPeriod(FiscalQuarter, FiscalMonth);
        }



        [SqlUserDefinedType(typeof(FiscalPeriodFormatter))]
        public struct FiscalPeriod
        {
            public int Quarter { get; private set; }

            public int Month { get; private set; }

            public FiscalPeriod(int quarter, int month):this()
            {
                this.Quarter = quarter;
                this.Month = month;
            }

            public override bool Equals(object obj)
            {
                if (ReferenceEquals(null, obj))
                {
                    return false;
                }

                return obj is FiscalPeriod && Equals((FiscalPeriod)obj);
            }

            public bool Equals(FiscalPeriod other)
            {
return this.Quarter.Equals(other.Quarter) &&    this.Month.Equals(other.Month);
            }

            public bool GreaterThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) > 0 || this.Month.CompareTo(other.Month) > 0;
            }

            public bool LessThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) < 0 || this.Month.CompareTo(other.Month) < 0;
            }

            public override int GetHashCode()
            {
                unchecked
                {
                    return (this.Quarter.GetHashCode() * 397) ^ this.Month.GetHashCode();
                }
            }

            public static FiscalPeriod operator +(FiscalPeriod c1, FiscalPeriod c2)
            {
return new FiscalPeriod((c1.Quarter + c2.Quarter) > 4 ? (c1.Quarter + c2.Quarter)-4 : (c1.Quarter + c2.Quarter), (c1.Month + c2.Month) > 12 ? (c1.Month + c2.Month) - 12 : (c1.Month + c2.Month));
            }

            public static bool operator ==(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.Equals(c2);
            }

            public static bool operator !=(FiscalPeriod c1, FiscalPeriod c2)
            {
                return !c1.Equals(c2);
            }
            public static bool operator >(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.GreaterThan(c2);
            }
            public static bool operator <(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.LessThan(c2);
            }
            public override string ToString()
            {
                return (String.Format("Q{0}:P{1}", this.Quarter, this.Month));
            }

        }

        public class FiscalPeriodFormatter : IFormatter<FiscalPeriod>
        {
public void Serialize(FiscalPeriod instance, IColumnWriter writer, ISerializationContext context)
            {
                using (var binaryWriter = new BinaryWriter(writer.BaseStream))
                {
                    binaryWriter.Write(instance.Quarter);
                    binaryWriter.Write(instance.Month);
                    binaryWriter.Flush();
                }
            }

public FiscalPeriod Deserialize(IColumnReader reader, ISerializationContext context)
            {
                using (var binaryReader = new BinaryReader(reader.BaseStream))
                {
var result = new FiscalPeriod(binaryReader.ReadInt16(), binaryReader.ReadInt16());
                    return result;
                }
            }
        }

    }
}
```

## <a name="user-defined-aggregates--udagg"></a>Benutzerdefinierte Aggregate – (UDAGG, User-Defined Aggregates)
Benutzerdefinierte Aggregate sind alle Funktionen im Zusammenhang mit Aggregation, die nicht standardmäßig in U-SQL verfügbar sind. Das Beispiel kann ein Aggregat zur Durchführung einer benutzerdefinierten mathematischen Berechnung, von Zeichenfolgenverkettungen oder Bearbeitungen mit Zeichenfolgen usw. sein.

Die Basisklassendefinition eines benutzerdefinierten Aggregats ist:

```c#
    [SqlUserDefinedAggregate]
    public abstract class IAggregate<T1, T2, TResult> : IAggregate
    {
        protected IAggregate();

        public abstract void Accumulate(T1 t1, T2 t2);
        public abstract void Init();
        public abstract TResult Terminate();
    }
```

**SqlUserDefinedAggregate** gibt an, dass der Typ als benutzerdefiniertes Aggregat registriert werden muss. Diese Klasse kann nicht vererbt werden.

Das SqlUserDefinedType-Attribut ist für die UDAGG-Definition **optional**.


Die Basisklasse ermöglicht Ihnen die Übergabe von drei abstrakten Parametern – zwei zur Eingabe, einer für das Ergebnis. Die Datentypen sind variabel und sollten während der Klassenvererbung definiert werden.

```c#
    public class GuidAggregate : IAggregate<string, string, string>
    {
        string guid_agg;

        public override void Init()
        {
        …
        }

        public override void Accumulate(string guid, string user)
        {
        …
        }

        public override string Terminate()
        {
        …
        }

    }
```

* „Init“ bietet eine Initialisierungsroutine für jede Aggregationsgruppe. Sie wird während der Berechnung einmal für jede Gruppe aufgerufen.
* „Accumulate“ – wird einmal für jeden Wert ausgeführt. Stellt die wichtigsten Funktionen für den Aggregationsalgorithmus bereit. Kann zum Aggregieren von Werten mit verschiedenen, während der Klassenvererbung definierten Datentypen verwendet werden. Kann zwei Parameter mit variablen Datentypen akzeptieren.
* „Terminate“ – wird am Ende der Verarbeitung einmal pro Aggregationsgruppe ausgeführt, um das Ergebnis für jede Gruppe auszugeben.

Um einen korrekten Eingabe- und Ausgabedatentyp zu deklarieren, verwenden Sie die Klassendefinition:

```c#
    public abstract class IAggregate<T1, T2, TResult> : IAggregate
```

* „T1“ – erster Parameter für „Accumulate“
* „T2“ – zweiter Parameter für „Accumulate“
* „TResult“ – Rückgabetyp von „Terminate“

für Beispiel

```c#
    public class GuidAggregate : IAggregate<string, int, int>
```

or

```c#
    public class GuidAggregate : IAggregate<string, string, string>
```

### <a name="using-udagg-in-u-sql"></a>Verwenden von UDAGGs in U-SQL
Um ein UDAGG verwenden zu können, definieren Sie es zuerst in CodeBehind, oder verweisen Sie, wie bereits erwähnt, aus der vorhandenen Programmierbarkeits-DLL darauf.

Verwenden Sie dann die folgende Syntax:

```c#
    AGG<UDAGG_functionname>(param1,param2)
```

Hier ist das Beispiel für ein UDAGG:

```c#
    public class GuidAggregate : IAggregate<string, string, string>
    {
        string guid_agg;

        public override void Init()
        {
            guid_agg = "";
        }

        public override void Accumulate(string guid, string user)
        {
            if (user.ToUpper()== "USER1")
            {
                guid_agg += "{" + guid + "}";
            }
        }

        public override string Terminate()
        {
            return guid_agg;
        }

    }
```

und das U-SQL-Basisskript:

```sql
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @" \usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        guid string,
        dt DateTime,
        user String,
        des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT
        user,
        AGG<USQL_Programmability.GuidAggregate>(guid,user) AS guid_list
    FROM @rs0
    GROUP BY user;

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

In diesem Anwendungsfall verketten wir Klassen-GUIDs für den bestimmten Benutzer.

## <a name="user-defined-objects--udo"></a>Benutzerdefinierte Objekte – (UDO, User-Defined Objects)
U-SQL bietet eine Möglichkeit zum Definieren benutzerdefinierter Programmierbarkeitsobjekte, die als benutzerdefinierte Objekte oder UDOs bezeichnet werden.

Liste von UDOs in U-SQL

* Benutzerdefinierte Extraktoren
    * Zeilenweise Extraktion
    * Wird verwendet, um die Datenextraktion aus benutzerdefinierten strukturierten Dateien zu implementieren.

* Benutzerdefinierte Outputter
    * Zeilenweise Ausgabe
    * Wird zur Ausgabe benutzerdefinierter Datentypen oder in das benutzerdefinierte Dateiformat verwendet.

* Benutzerdefinierte Prozessoren
    * Eine Zeile nehmen und eine Zeile erzeugen
    * Wird verwendet, um die Anzahl der Spalten zu reduzieren, oder eine neue Spalte mit Werten zu erzeugen, die vom vorhandenen Spaltensatz abgeleitet werden.

* Benutzerdefinierte Applier
    * Eine Zeile nehmen und 0 bis n Zeilen erzeugen
    * Wird mit OUTER/CROSS APPLY verwendet.

* Benutzerdefinierte Combiner
    * Kombiniert Rowsets – benutzerdefinierte JOINs.

* Benutzerdefinierte Reducer
    * N Zeilen nehmen und eine Zeile erzeugen
    * Wird verwendet, um die Anzahl der Zeilen zu reduzieren.

Das UDO wird im U-SQL-Skript in der Regel explizit als Teil der folgenden U-SQL-Anweisungen aufgerufen:

* EXTRACT
* OUTPUT
* PROCESS
* COMBINE
* REDUCE

## <a name="user-defined-extractor"></a>Benutzerdefinierter Extraktor
In U-SQL können Sie externe Daten mit der EXTRACT-Anweisung importieren. Die EXTRACT-Anweisung kann integrierte UDO-Extraktoren verwenden.  

* *Extractors.Text()*: Extraktion aus durch Trennzeichen getrennten Textdateien mit verschiedenen Codierungen.

* *Extractors.Csv()*: Extraktion aus durch Trennzeichen getrennten Dateien (CSV) mit verschiedenen Codierungen.

* *Extractors.Tsv()*: Extraktion aus durch Tabstopp getrennten Dateien (TSV) mit verschiedenen Codierungen.

Allerdings kann es hilfreich sein, einen benutzerdefinierten Extraktor zu entwickeln. Dies kann während des Datenimports zu folgenden Zwecken nützlich sein:

* Ändern von Eingabedaten durch Teilen von Spalten, Ändern einzelner Werte. Spalten lassen sich besser mit PROCESSOR-Funktionen kombinieren.
* Analysieren unstrukturierter Daten wie Webseiten/E-Mails oder teilweise unstrukturierter Daten wie z.B. XML/JSON.
* Analysieren von Daten in nicht unterstützter Codierung.

Um einen benutzerdefinierten Extraktor (User-Defined Extractor, UDE) zu definieren, müssen wir eine `IExtractor`-Schnittstelle erstellen. Alle Eingabeparameter des Extraktors, z.B. Spalten-/Zeilentrennzeichen, Codierung usw., müssen im Konstruktor der Klasse definiert werden. Die `IExtractor`-Schnittstelle sollte auch die Definition für `IEnumerable<IRow>`-Außerkraftsetzungen enthalten.

```c#
     [SqlUserDefinedExtractor]
     public class SampleExtractor : IExtractor
     {
         public SampleExtractor(string row_delimiter, char col_delimiter)
         {
            …

         }

         public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
         {
             …
         }
     }
```

Das **SqlUserDefinedExtractor**-Attribut gibt an, dass der Typ als benutzerdefinierter Extraktor registriert werden muss. Diese Klasse kann nicht vererbt werden.

SqlUserDefinedExtractor ist ein optionales Attribut für die UDE-Definition. Es wird verwendet, um die AtomicFileProcessing-Eigenschaft für das UDE-Objekt zu definieren.

* bool     AtomicFileProcessing   

* **true** = dieser Extraktor benötigt atomische Eingabedateien (JSON, XML, ...).
* **false** = dieser Extraktor kann geteilte/verteilte Dateien (CSV, SEQ, ...) behandeln.

Die wichtigsten UDE-Programmierbarkeitsobjekte sind *input* und *output*. Das input-Objekt dient als `IUnstructuredReader` zum Aufzählen von Eingabedaten, und *output*, um Ausgabedaten als Ergebnis der Extraktoraktivität festzulegen.

Der Zugriff auf die Eingabedaten erfolgt über `System.IO.Stream` und `System.IO.StreamReader`.

Für die Eingabespalten-Aufzählung teilen wir zuerst den Eingabedatenstrom mit Zeilentrennzeichen.

```c#
    foreach (Stream current in input.Split(my_row_delimiter))
    {
    …
    }
```

Dann teilen wir die Eingabezeile weiter in Spaltenteile auf.

```c#
    foreach (Stream current in input.Split(my_row_delimiter))
    {
    …
        string[] parts = line.Split(my_column_delimiter);
               foreach (string part in parts)
        {
        …
        }
    }
```

Zum Festlegen von Ausgabedaten verwenden wir die `output.Set`-Methode.

Beachten Sie unbedingt, dass der benutzerdefinierte Extraktor nur mit „output“ definierte Spalten/Werte ausgibt. Wir legen den Methodenaufruf fest.

```c#
    output.Set<string>(count, part);
```

Die tatsächliche Extraktorausgabe wird durch den Aufruf von `yield return output.AsReadOnly();` ausgelöst.

Hier ist das Extraktorbeispiel:

```c#
    [SqlUserDefinedExtractor(AtomicFileProcessing = true)]
    public class FullDescriptionExtractor : IExtractor
    {
         private Encoding _encoding;
         private byte[] _row_delim;
         private char _col_delim;

        public FullDescriptionExtractor(Encoding encoding, string row_delim = "\r\n", char col_delim = '\t')
        {
             this._encoding = ((encoding == null) ? Encoding.UTF8 : encoding);
             this._row_delim = this._encoding.GetBytes(row_delim);
             this._col_delim = col_delim;

        }

        public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
        {
             string line;
             //Read the input line by line
             foreach (Stream current in input.Split(_encoding.GetBytes("\r\n")))
             {
                using (System.IO.StreamReader streamReader = new StreamReader(current, this._encoding))
                 {
                     line = streamReader.ReadToEnd().Trim();
                     //Split the input by the column delimiter
                     string[] parts = line.Split(this._col_delim);
                     int count = 0; // start with first column
                     foreach (string part in parts)
                     {
    if (count == 0)
                         {  // for column “guid”, re-generated guid
                             Guid new_guid = Guid.NewGuid();
                             output.Set<Guid>(count, new_guid);
                         }
                         else if (count == 2)
                         {
                             // for column “user”, convert to UPPER case
                             output.Set<string>(count, part.ToUpper());

                         }
                         else
                         {
                             // keep the rest of the columns as-is
                             output.Set<string>(count, part);
                         }
                         count += 1;
                     }

                 }
                 yield return output.AsReadOnly();
             }
             yield break;
         }
     }
```

In diesem Anwendungsfall generiert der Extraktor GUID für die „guid“-Spalte neu und konvertiert die Werte der „user“-Spalte in Großbuchstaben. Benutzerdefinierte Extraktoren können durch Analysieren der Eingabedaten und deren Bearbeitung kompliziertere Ergebnisse hervorbringen.

U-SQL-Basisskript, das einen benutzerdefinierten Extraktor verwendet

```sql
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        guid Guid,
        dt String,
        user String,
        des String
    FROM @input_file
    USING new USQL_Programmability.FullDescriptionExtractor(Encoding.UTF8);

OUTPUT @rs0 TO @output_file USING Outputters.Text();
```

## <a name="user-defined-outputter"></a>Benutzerdefinierter Outputter
Der benutzerdefinierte Outputter ist ein anderes U-SQL-UDO, das die Erweiterung einer integrierten U-SQL-Funktionalität ermöglicht. Ähnlich wie beim Extraktor gibt es verschiedene integrierte Outputter.

* *Outputters.Text()*: schreibt Daten in durch Trennzeichen getrennte Textdateien mit verschiedenen Codierungen.
* *Outputters.Csv()*: schreibt Daten in durch Trennzeichen getrennte Textdateien (CSV) mit verschiedenen Codierungen.
* *Outputters.Tsv()*: schreibt Daten in durch Tabstopp getrennte Textdateien (TSV) mit verschiedenen Codierungen.

Benutzerdefinierte Outputter ermöglichen Ihnen, Daten in einem benutzerdefinierten Format zu schreiben. Dies kann in folgenden Fällen hilfreich sein:

* Schreiben von Daten in teilweise strukturierte oder unstrukturierte Dateien
* Schreiben von Daten in nicht unterstützten Codierungen
* Ändern von Ausgabedaten oder Hinzufügen von benutzerdefinierten Attributen

Um einen benutzerdefinierten Outputter zu definieren, müssen wir eine `IOutputter`-Schnittstelle erstellen.

Basis-`IOutputter`-Klassenimplementierung

```c#
    public abstract class IOutputter : IUserDefinedOperator
    {
        protected IOutputter();

        public virtual void Close();
        public abstract void Output(IRow input, IUnstructuredWriter output);
    }
```

Alle Eingabeparameter des Outputters, z.B. Spalten-/Zeilentrennzeichen, Codierung usw., müssen im Konstruktor der Klasse definiert werden. Die `IOutputter`-Schnittstelle sollte auch die Definition für `void Output`-Außerkraftsetzungen enthalten. Das Attribut `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` kann optional für die Verarbeitung atomischer Dateien festgelegt werden – bitte beachten Sie die Details im Folgenden.

```c#
        [SqlUserDefinedOutputter(AtomicFileProcessing = true)]
        public class MyOutputter : IOutputter
        {

            public MyOutputter(myparam1, myparam2)
            {
              …
            }

            public override void Close()
            {
              …
            }

            public override void Output(IRow row, IUnstructuredWriter output)
            {
              …
            }
        }
```

* `Output` wird für jede Eingabezeile aufgerufen. Gibt das `IUnstructuredWriter output`-Rowset zurück.
* Die Konstruktorklasse wird zum Übergeben von Parametern an den benutzerdefinierten Outputter verwendet.
* `Close` wird optional außer Kraft gesetzt, um den ressourcenintensiven Zustand aufzuheben oder zu wissen, wann die letzte Zeile geschrieben wurde.

Das **SqlUserDefinedOutputter**-Attribut gibt an, dass der Typ als benutzerdefinierter Outputter registriert werden muss. Diese Klasse kann nicht vererbt werden.

SqlUserDefinedOutputter ist ein optionales Attribut zur Definition eines benutzerdefinierten Outputters. Es wird verwendet, um die AtomicFileProcessing-Eigenschaft zu definieren.

* bool     AtomicFileProcessing   

* **true** = dieser Outputter benötigt atomische Ausgabedateien (JSON, XML, ...).
* **false** = dieser Outputter kann geteilte/verteilte Dateien (CSV, SEQ, ...) behandeln.

Die wichtigsten Programmierbarkeitsobjekte sind *row* und *output*. Das *row*-Objekt dient als `IRow`-Schnittstelle zum Aufzählen von Ausgabedaten, und *output*, um Ausgabedaten in die Zieldatei zu lenken.

Der Zugriff auf die Ausgabedaten erfolgt über die `IRow`-Schnittstelle. Ausgabedaten werden jeweils zeilenweise übergeben.

Die einzelnen Werte werden durch den Aufruf der Get-Methode der IRow-Schnittstelle aufgezählt.

```c#
    row.Get<string>("column_name")
```

Einzelne Spaltennamen können durch Aufrufen von `row.Schema` ermittelt werden.

```c#
    ISchema schema = row.Schema;
    var col = schema[i];
    string val = row.Get<string>(col.Name)
```

Dieser Ansatz ermöglicht Ihnen, einen flexiblen Outputter für alle Metadatenschemata zu erstellen.

Um die Ausgabedaten in die Datei zu schreiben, wird `System.IO.StreamWriter` verwendet, wobei der Datenstromparameter als Teil von `IUnstructuredWriter output` auf `output.BaseStrea` festgelegt wird.

Wichtige Anmerkung: Um den Datenpuffer nach jeder Zeileniteration in die Datei zu leeren, muss das `StreamWriter`-Objekt mit aktiviertem Disposable-Attribut (Standard) und dem Schlüsselwort *using* verwendet werden.

```c#
    using (StreamWriter streamWriter = new StreamWriter(output.BaseStream, this._encoding))
            {
        …
            }
```

Rufen Sie andernfalls die Flush()-Methode explizit nach jeder Iteration auf – dies wird im weiteren Verlauf dieses Beispiels veranschaulicht:

### <a name="setting-header-and-footer-for-user-defined-outputter"></a>Festlegen von Kopf- und Fußzeile für benutzerdefinierten Outputter
Verwenden Sie zum Festlegen einer Kopfzeile einen einzelnen Durchlauf der Iterationsausführung.

```c#
            public override void Output(IRow row, IUnstructuredWriter output)
            {
             …
                if (isHeaderRow)
                {
                    …                
                }

             …
                if (isHeaderRow)
                {
                    isHeaderRow = false;
                }
             …
            }
        }
```

Der Code im ersten `if (isHeaderRow)`-Block wird nur einmal ausgeführt.

Verwenden Sie für die Fußzeile den Verweis auf die Instanz des `System.IO.Stream`-Objekts (`output.BaseStream`), und schreiben Sie die Fußzeile in die Close()-Methode der `IOutputter`-Schnittstelle – siehe unten aufgeführtes Beispiel.

Hier ist ein Beispiel für einen benutzerdefinierten Outputter:

```c#
        [SqlUserDefinedOutputter(AtomicFileProcessing = true)]
        public class HTMLOutputter : IOutputter
        {
            // Local variables initialization
            private string row_delimiter;
            private char col_delimiter;
            private bool isHeaderRow;
            private Encoding encoding;
            private bool IsTableHeader = true;
            private Stream g_writer;

            // Parameters definition            
            public HTMLOutputter(bool isHeader = false, Encoding encoding = null)
            {
                this.isHeaderRow = isHeader;
                this.encoding = ((encoding == null) ? Encoding.UTF8 : encoding);
            }

            // Close method is used to write footer to file - executed only once after all rows
            public override void Close()
            {
                //Reference to IO.Stream object - g_writer
                StreamWriter streamWriter = new StreamWriter(g_writer, this.encoding);
                streamWriter.Write("</table>");
                streamWriter.Flush();
                streamWriter.Close();
            }

            public override void Output(IRow row, IUnstructuredWriter output)
            {
                System.IO.StreamWriter streamWriter = new StreamWriter(output.BaseStream, this.encoding);

                // Metadata schema initialization to enumerate column names
                ISchema schema = row.Schema;

                // This is data independent header - HTML table definition
                if (IsTableHeader)
                {
                    streamWriter.Write("<table border=1>");
                    IsTableHeader = false;
                }

                // HTML table attributes
                string header_wrapper_on = "<th>";
                string header_wrapper_off = "</th>";
                string data_wrapper_on = "<td>";
                string data_wrapper_off = "</td>";

                // Header row output - runs only once
                if (isHeaderRow)
                {
                    streamWriter.Write("<tr>");
                    for (int i = 0; i < schema.Count(); i++)
                    {
                        var col = schema[i];
                        streamWriter.Write(header_wrapper_on + col.Name + header_wrapper_off);
                    }
                    streamWriter.Write("</tr>");
                }

                // Data row output
                streamWriter.Write("<tr>");                
                for (int i = 0; i < schema.Count(); i++)
                {
                    var col = schema[i];
                    string val = "";
                    try
                    {
                        // Data type enumeration - require to match the distinct list of types form OUTPUT statement
                        switch (col.Type.Name.ToString().ToLower())
                        {
                            case "string": val = row.Get<string>(col.Name).ToString(); break;
                            case "guid": val = row.Get<Guid>(col.Name).ToString(); break;
                            default: break;
                        }
                    }
                    // Hnadling NULL values - keeping them empty
                    catch (System.NullReferenceException)
                    {
                    }
                    streamWriter.Write(data_wrapper_on + val + data_wrapper_off);
                }
                streamWriter.Write("</tr>");

                if (isHeaderRow)
                {
                    isHeaderRow = false;
                }
                // Reference to the instance of the IO.Stream object for footer generation
                g_writer = output.BaseStream;
                streamWriter.Flush();
            }
        }

        // Define the factory classes
        public static class Factory
        {
            public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
            {
                return new HTMLOutputter(isHeader, encoding);
            }
        }
```

und das U-SQL-Basisskript

```sql
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.html";

@rs0 =
    EXTRACT
        guid Guid,
        dt String,
        user String,
        des String
    FROM @input_file
    USING new USQL_Programmability.FullDescriptionExtractor(Encoding.UTF8);

OUTPUT @rs0 TO @output_file USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

Hierbei handelt es sich um einen HTML-Outputter – er erstellt eine HTML-Datei mit Tabellendaten.

### <a name="calling-outputter-from-u-sql-base-script"></a>Aufrufen des Outputters aus dem U-SQL-Basisskript
Für den Aufruf eines benutzerdefinierten Outputters aus dem U-SQL-Basisskript muss die neue Instanz des Outputterobjekts erstellt werden.

```sql
OUTPUT @rs0 TO @output_file USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

Um zu vermeiden, eine Instanz des Objekts im Basisskript zu erstellen, können wir einen Funktionswrapper erstellen. Von oben:

```c#
        // Define the factory classes
        public static class Factory
        {
            public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
            {
                return new HTMLOutputter(isHeader, encoding);
            }
        }
```

In diesem Fall sieht der ursprüngliche Aufruf wie folgt aus:

```sql
OUTPUT @rs0 TO @output_file USING USQL_Programmability.Factory.HTMLOutputter(isHeader: true);
```

## <a name="user-defined-processor"></a>Benutzerdefinierter Prozessor
Der benutzerdefinierte Prozessor oder UDP (User-Defined Processor) ist ein U-SQL-UDO, das die Verarbeitung eingehender Zeilen durch mithilfe der Programmierbarkeitsfeatures ermöglicht. Mit dem UDP können Sie ggf. Spalten kombinieren, Werte ändern und neue Spalten hinzufügen. Im Wesentlichen unterstützt er die Verarbeitung eines Rowsets, um erforderliche Datenelemente zu erzeugen.

Um einen UDP zu definieren, müssen wir die `IProcessor`-Schnittstelle mit dem `SqlUserDefinedProcessor`-Attribut erstellen, das für den UDP optional ist.

Diese Schnittstelle muss die Definition für die Außerkraftsetzung des Rowsets der `IRow`-Schnittstelle enthalten.

```c#
    [SqlUserDefinedProcessor]
     public class MyProcessor: IProcessor
     {
        public override IRow Process(IRow input, IUpdatableRow output)
         {
            …
         }
     }
```

**SqlUserDefinedProcessor** gibt an, dass der Typ als benutzerdefinierter Prozessor registriert werden muss. Diese Klasse kann nicht vererbt werden.

Das SqlUserDefinedProcessor-Attribut ist für die UDP-Definition **optional**.

Die wichtigsten Programmierbarkeitsobjekte sind *input* und *output*. Das input-Objekt dient zum Aufzählen von Eingabespalten, und „output“, um Ausgabedaten als Ergebnis der Prozessoraktivität festzulegen.

Zur Eingabespalten-Aufzählung verwenden wir die `input.Get`-Methode.

```c#
    string column_name = input.Get<string>("column_name");
```

Der Parameter der `input.Get`-Methode ist eine Spalte, die als Teil der `PRODUCE`-Klausel der ` PROCESS`-Anweisung des U-SQL-Basisskripts übergeben wird. Wir müssen hier den richtigen Datentyp verwenden.

Für die Ausgabe – output.Set-Methode

Beachten Sie unbedingt, dass der benutzerdefinierte Producer nur mit dem Aufruf der output.Set-Methode definierte Spalten/Werte ausgibt.

```c#
    output.Set<string>("mycolumn", mycolumn);
```

Die tatsächliche Prozessorausgabe wird durch den Aufruf von `return output.AsReadOnly()` ausgelöst.

Hier ist ein Prozessorbeispiel.

```c#
     [SqlUserDefinedProcessor]
     public class FullDescriptionProcessor : IProcessor
     {
        public override IRow Process(IRow input, IUpdatableRow output)
         {
             string user = input.Get<string>("user");
             string des = input.Get<string>("des");
             string full_description = user.ToUpper() + "=>" + des;
             output.Set<string>("dt", input.Get<string>("dt"));
             output.Set<string>("full_description", full_description);
             output.Set<Guid>("new_guid", Guid.NewGuid());
             output.Set<Guid>("guid", input.Get<Guid>("guid"));
             return output.AsReadOnly();
         }
     }
```

In diesem Anwendungsfall erzeugt der Prozessor die neue Spalte „full_description“ durch die Kombination der vorhandenen Spalten – „user“ in Großbuchstaben und „des“. Er generiert außerdem eine GUID und gibt den ursprünglichen und neuen GUID-Wert zurück.

Wie Sie oben sehen, können Sie während des output.Set-Aufrufs C#-Methoden aufrufen.

U-SQL-Basisskript, das einen benutzerdefinierten Prozessor verwendet

```sql
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        guid Guid,
        dt String,
        user String,
        des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
     PROCESS @rs0
     PRODUCE dt String,
             full_description String,
             guid Guid,
             new_guid Guid
     USING new USQL_Programmability.FullDescriptionProcessor();

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

## <a name="user-defined-applier"></a>Benutzerdefinierter Applier
Mit dem benutzerdefinierten USQL-Applier können Sie für jede Zeile, die der äußere Tabellenausdruck einer Abfrage zurückgibt, eine benutzerdefinierte C#-Funktion aufrufen. Die rechte Eingabe wird für jede Zeile aus der linken Eingabe ausgewertet, und die erstellten Zeilen werden für die endgültige Ausgabe kombiniert. Die Liste der Spalten, die der APPLY-Operator erzeugt, ist die Kombination aus dem Satz von Spalten in der linken und rechten Eingabe.

Der benutzerdefinierte Applier wird als Teil des USQL SELECT-Ausdrucks aufgerufen.

Die typische Aufruf des benutzerdefinierten Appliers sieht so aus:

```sql
    SELECT …
    FROM …
    CROSS APPLY
    new MyScript.MyApplier(param1, param2) AS alias(output_param1 string, …);
```

Unter [U-SQL SELECT Selecting from CROSS APPLY and OUTER APPLY](https://msdn.microsoft.com/library/azure/mt621307.aspx) (U-SQL SELECT-Auswahl aus CROSS APPLY und OUTER APPLY) finden Sie weitere Informationen zur Verwendung von Appliers im SELECT-Ausdruck.

Die Basisklassendefinition eines benutzerdefinierten Appliers ist:

```c#
    public abstract class IApplier : IUserDefinedOperator
    {
        protected IApplier();

        public abstract IEnumerable<IRow> Apply(IRow input, IUpdatableRow output);
    }
```

Um einen benutzerdefinierten Applier zu definieren, müssen wir die `IApplier`-Schnittstelle mit dem [`SqlUserDefinedCombiner`]-Attribut erstellen, das für die Definition eines benutzerdefinierten Appliers optional ist.

```c#
    [SqlUserDefinedApplier]
    public class ParserApplier : IApplier
    {

        public ParserApplier()
        {
        …
        }

        public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
        {
        …
        }
    }
```

* „Apply“ wird für jede Zeile der äußeren Tabelle aufgerufen. Gibt das `IUpdatableRow`-Ausgaberowset zurück.
* Die Konstruktorklasse wird zum Übergeben von Parametern an den benutzerdefinierten Applier verwendet.

**SqlUserDefinedApplier** gibt an, dass der Typ als benutzerdefinierter Applier registriert werden muss. Diese Klasse kann nicht vererbt werden.

Das SqlUserDefinedApplier-Attribut ist für die Definition benutzerdefinierter Applier **optional**.


Die wichtigsten Programmierbarkeitsobjekte sind:

```c#
        public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
```

Eingaberowsets werden als `IRow`-Eingabe übergeben. Die Ausgabezeilen werden als `IUpdatableRow`-Ausgabeschnittstelle generiert.

Einzelne Spaltennamen können durch Aufrufen der `IRow`-Schema-Methode ermittelt werden.

```c#
    ISchema schema = row.Schema;
    var col = schema[i];
    string val = row.Get<string>(col.Name)
```

Um die tatsächlichen Datenwerte aus dem eingehenden `IRow` abzurufen, verwenden wir die Get()-Methode der `IRow`-Schnittstelle.

```c#
    mycolumn = row.Get<int>("mycolumn")
```

Oder anhand des Schema-Spaltennamens:

```c#
    row.Get<int>(row.Schema[0].Name)
```

Die Ausgabewerte müssen mit der `IUpdatableRow`-Ausgabe festgelegt werden.

```c#
    output.Set<int>("mycolumn", mycolumn)
```

Beachten Sie unbedingt, dass der benutzerdefinierte Applier nur mit dem Aufruf der output.Set-Methode definierte Spalten/Werte ausgibt.

Die tatsächliche Ausgabe wird durch den Aufruf von `yield return output.AsReadOnly()` ausgelöst.

Die Parameter des benutzerdefinierten Appliers können an den Konstruktor übergeben werden. Der Applier kann eine variable Anzahl von Spalten zurückgeben, die während des Applieraufrufs im U-SQL-Basisskript definiert werden muss.

```c#
  new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);
```

Hier ist das Beispiel eines benutzerdefinierten Appliers:

```c#
   [SqlUserDefinedApplier]
    public class ParserApplier : IApplier
    {
        private string parsingPart;

        public ParserApplier(string parsingPart)
        {
            if (parsingPart.ToUpper().Contains("ALL")
                || parsingPart.ToUpper().Contains("MAKE")
                || parsingPart.ToUpper().Contains("MODEL")
                || parsingPart.ToUpper().Contains("YEAR")
                || parsingPart.ToUpper().Contains("TYPE")
                || parsingPart.ToUpper().Contains("MILLAGE")
                )
            {
                this.parsingPart = parsingPart;
            }
            else
            {
                throw new ArgumentException("Incorrect parameter. Please use: 'ALL[MAKE|MODEL|TYPE|MILLAGE]'");
            }
        }

        public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
        {

            string[] properties = input.Get<string>("properties").Split(',');

            //  only process with correct number of properties
            if (properties.Count() == 5)
            {

                string make = properties[0];
                string model = properties[1];
                string year = properties[2];
                string type = properties[3];
                int millage = -1;

                // Only return millage if it is number, otherwise, -1
                if (!int.TryParse(properties[4], out millage))
                {
                    millage = -1;
                }

                if (parsingPart.ToUpper().Contains("MAKE") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("make", make);
                if (parsingPart.ToUpper().Contains("MODEL") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("model", model);
                if (parsingPart.ToUpper().Contains("YEAR") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("year", year);
                if (parsingPart.ToUpper().Contains("TYPE") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("type", type);
                if (parsingPart.ToUpper().Contains("MILLAGE") || parsingPart.ToUpper().Contains("ALL")) output.Set<int>("millage", millage);
            }
            yield return output.AsReadOnly();            
        }
    }
```

Und das U-SQL-Basisskript für diesen benutzerdefinierten Applier:

```sql
DECLARE @input_file string = @"c:\usql-programmability\car_fleet.tsv";
DECLARE @output_file string = @"c:\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        stocknumber int,
        vin String,
        properties String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT
        r.stocknumber,
        r.vin,
        properties.make,
        properties.model,
        properties.year,
        properties.type,
        properties.millage
    FROM @rs0 AS r
    CROSS APPLY
    new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

In diesem Anwendungsfall fungiert der benutzerdefinierte Applier als Parser mit Werttrennung durch Trennzeichen für die Eigenschaften der Autoflotte. Die Zeilen der Eingabedatei sehen wie folgt aus:

```
103    Z1AB2CD123XY45889    Ford,Explorer,2005,SUV,152345
303    Y0AB2CD34XY458890    Shevrolet,Cruise,2010,4Dr,32455
210    X5AB2CD45XY458893    Nissan,Altima,2011,4Dr,74000
```

Es handelt sich um eine typische TSV-Datei mit Tabstopptrennzeichen mit Properties-Spalte, die Autoeigenschaften wie Marke, Modell usw. enthält. Diese Eigenschaften müssen in die Tabellenspalten analysiert werden. Der bereitgestellte Applier ermöglicht auch das Generieren einer dynamischen Anzahl von Eigenschaften in einem Ergebnisrowset, basierend auf den übergebenen Parametern – entweder alle, oder nur ein bestimmter Satz von Eigenschaften.

    …USQL_Programmability.ParserApplier ("all")
    …USQL_Programmability.ParserApplier ("make")
    …USQL_Programmability.ParserApplier ("make&model")

Der benutzerdefinierte Applier kann entweder als eine neue Instanz des Applierobjekts aufgerufen werden:

```c#
    CROSS APPLY new MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

Oder mit Aufrufen einer Wrapper-Factory-Methode:

```c#
    CROSS APPLY MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

## <a name="user-defined-combiner"></a>Benutzerdefinierter Combiner
Mit einem benutzerdefinierten Combiner (User-Defined Combiner, UDC) können Sie Zeilen aus linken und rechten Rowsets auf der Grundlage einer benutzerdefinierten Logik kombinieren. Ein benutzerdefinierter Combiner wird mit dem COMBINE-Ausdruck verwendet.

Ein Combiner wird mit dem COMBINE-Ausdruck aufgerufen, der die erforderlichen Informationen über die Eingaberowsets, die Gruppierungsspalten, das erwartete Ergebnisschema und die zusätzlichen Informationen bereitstellt.

Um einen Combiner in einem U-SQL-Basisskript aufzurufen, verwenden Sie die folgende Syntax:

```sql
Combine_Expression :=
    'COMBINE' Combine_Input
    'WITH' Combine_Input
    Join_On_Clause
    Produce_Clause
    [Readonly_Clause]
    [Required_Clause]
    USING_Clause.
```

Weitere Informationen finden Sie unter [COMBINE Expression (U-SQL)](https://msdn.microsoft.com/library/azure/mt621339.aspx) (COMBINE-Ausdruck [U-SQL]).

Um einen benutzerdefinierten Combiner zu definieren, müssen wir die `ICombiner`-Schnittstelle mit dem [`SqlUserDefinedCombiner`]-Attribut erstellen, das für die Definition eines benutzerdefinierten Combiners optional ist.

`ICombiner`-Basisklassendefinition

```c#
    public abstract class ICombiner : IUserDefinedOperator
    {
        protected ICombiner();
        public virtual IEnumerable<IRow> Combine(List<IRowset> inputs,
               IUpdatableRow output);
        public abstract IEnumerable<IRow> Combine(IRowset left, IRowset right,
               IUpdatableRow output);
    }
```

Die benutzerdefinierte Implementierung der `ICombiner`-Schnittstelle muss die Definition für die `IEnumerable<IRow>`-Combine-Außerkraftsetzung enthalten.

```c#
    [SqlUserDefinedCombiner]
    public class MyCombiner : ICombiner
    {

        public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output)
        {
        …
        }
    }
```

Das **SqlUserDefinedCombiner**-Attribut gibt an, dass der Typ als benutzerdefinierter Combiner registriert werden muss. Diese Klasse kann nicht vererbt werden.

SqlUserDefinedCombiner ist ein optionales Attribut zur Definition eines benutzerdefinierten Combiners. Es wird zum Definieren der Combiner-Moduseigenschaft verwendet.

CombinerMode-Modus

Die CombinerMode-Aufzählung akzeptiert die folgenden Werte:

* „Full   (0)“    Jede Ausgabezeile ist potenziell von allen Eingabezeilen von links und rechts mit dem gleichen Schlüsselwert abhängig.

* „Left  (1)“    Jede Ausgabezeile ist von einer einzelnen Eingabezeile von links (und potenziell von allen Zeilen von rechts mit dem gleichen Schlüsselwert) abhängig.

* „Right (2)“    Jede Ausgabezeile ist von einer einzelnen Eingabezeile von rechts (und potenziell von allen Zeilen von links mit dem gleichen Schlüsselwert) abhängig.

* „Inner (3)“    Jede Ausgabezeile ist von einer einzelnen Eingabezeile von links und rechts mit dem gleichen Wert abhängig.

Beispiel:    [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]


Die wichtigsten Programmierbarkeitsobjekte sind:

```c#
    public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output
```

Eingaberowsets werden als *left*- und *right*-`IRowset`-Schnittstellentyp übergeben. Beide Rowsets müssen für die Verarbeitung aufgezählt werden. Es darf nur einmal für jede Schnittstelle aufgezählt werden, sodass die Aufzählung ggf. zwischengespeichert werden muss.

Zum Zwischenspeichern können wir den List\<T\>-Typ der Arbeitsspeicherstruktur als Ergebnis der LINQ-Abfragenausführung erstellen. Genauer: „List<`IRow`>“ Der anonyme Datentyp kann während der Aufzählung ebenfalls verwendet werden.

Unter [Introduction to LINQ Queries (C#)](https://msdn.microsoft.com/library/bb397906.aspx) (Einführung in LINQ-Abfragen [C#]) finden Sie weitere Informationen zu LINQ-Abfragen, und unter [IEnumerable\<T\> Interface](https://msdn.microsoft.com/library/9eekhta0(v=vs.110).aspx) (IEnumerable<T>-Schnittstelle) weitere Informationen zur IEnumerable\<T\>-Schnittstelle.

Um die tatsächlichen Datenwerte aus dem eingehenden `IRowset` abzurufen, verwenden wir die Get()-Methode der `IRow`-Schnittstelle.

```c#
    mycolumn = row.Get<int>("mycolumn")
```

Einzelne Spaltennamen können durch Aufrufen der `IRow`-Schema-Methode ermittelt werden.

```c#
    ISchema schema = row.Schema;
    var col = schema[i];
    string val = row.Get<string>(col.Name)
```

Oder anhand des Schema-Spaltennamens:

```
    c# row.Get<int>(row.Schema[0].Name)
```

Die allgemeine Aufzählung mit LINQ sieht wie folgt aus:

```c#
var myRowset =
            (from row in left.Rows
                          select new
                          {
                              Mycolumn = row.Get<int>("mycolumn"),
                          }).ToList();
```

Nach dem Aufzählen beider Rowsets werden alle Zeilen durchlaufen, und für jede Zeile im linken Rowset werden alle Zeilen gefunden, die die Bedingung des Combiners erfüllen.

Die Ausgabewerte müssen mit der `IUpdatableRow`-Ausgabe festgelegt werden.

```c#
    output.Set<int>("mycolumn", mycolumn)
```

Die tatsächliche Ausgabe wird durch den Aufruf von `yield return output.AsReadOnly()` ausgelöst.

Hier ist ein Combinerbeispiel:

```c#
    [SqlUserDefinedCombiner]
    public class CombineSales : ICombiner
    {

        public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output)
        {
            var internetSales =
            (from row in left.Rows
                          select new
                          {
                              ProductKey = row.Get<int>("ProductKey"),
                              OrderDateKey = row.Get<int>("OrderDateKey"),
                              SalesAmount = row.Get<decimal>("SalesAmount"),
                              TaxAmt = row.Get<decimal>("TaxAmt")
                          }).ToList();

            var resellerSales =
            (from row in right.Rows
             select new
             {
                 ProductKey = row.Get<int>("ProductKey"),
                 OrderDateKey = row.Get<int>("OrderDateKey"),
                 SalesAmount = row.Get<decimal>("SalesAmount"),
                 TaxAmt = row.Get<decimal>("TaxAmt")
             }).ToList();

            foreach (var row_i in internetSales)
            {
                foreach (var row_r in resellerSales)
                {

                    if (
                        row_i.OrderDateKey > 0
                        && row_i.OrderDateKey < row_r.OrderDateKey
                        && row_i.OrderDateKey == 20010701
                        && (row_r.SalesAmount + row_r.TaxAmt) > 20000)
                    {
                        output.Set<int>("OrderDateKey", row_i.OrderDateKey);
                        output.Set<int>("ProductKey", row_i.ProductKey);
                        output.Set<decimal>("Internet_Sales_Amount", row_i.SalesAmount + row_i.TaxAmt);
                        output.Set<decimal>("Reseller_Sales_Amount", row_r.SalesAmount + row_r.TaxAmt);
                    }

                }
            }
            yield return output.AsReadOnly();
        }
    }
```

In diesem Anwendungsfall erstellen wir einen Analysebericht für den Einzelhandel. Es sollen alle Produkte gefunden werden, deren Preis über $20.000 liegt, und die in einem bestimmten Zeitraum über die Website schneller verkauft werden als durch den regulären Einzelhandel.

So sieht das U-SQL-Basisskript aus. Sie können die Logik zwischen normalem JOIN und einem Combiner vergleichen:

```sql
DECLARE @LocalURI string = @"\usql-programmability\";

DECLARE @input_file_internet_sales string = @LocalURI+"FactInternetSales.txt";
DECLARE @input_file_reseller_sales string = @LocalURI+"FactResellerSales.txt";
DECLARE @output_file1 string = @LocalURI+"output_file1.tsv";
DECLARE @output_file2 string = @LocalURI+"output_file2.tsv";

@fact_internet_sales =
EXTRACT
    ProductKey int ,
    OrderDateKey int ,
    DueDateKey int ,
    ShipDateKey int ,
    CustomerKey int ,
    PromotionKey int ,
    CurrencyKey int ,
    SalesTerritoryKey int ,
    SalesOrderNumber String ,
    SalesOrderLineNumber  int ,
    RevisionNumber int ,
    OrderQuantity int ,
    UnitPrice decimal ,
    ExtendedAmount decimal,
    UnitPriceDiscountPct float ,
    DiscountAmount float ,
    ProductStandardCost decimal ,
    TotalProductCost decimal ,
    SalesAmount decimal ,
    TaxAmt decimal ,
    Freight decimal ,
    CarrierTrackingNumber String,
    CustomerPONumber String
FROM @input_file_internet_sales
USING Extractors.Text(delimiter:'|', encoding: Encoding.Unicode);

@fact_reseller_sales =
EXTRACT
    ProductKey int ,
    OrderDateKey int ,
    DueDateKey int ,
    ShipDateKey int ,
    ResellerKey int ,
    EmployeeKey int ,
    PromotionKey int ,
    CurrencyKey int ,
    SalesTerritoryKey int ,
    SalesOrderNumber String ,
    SalesOrderLineNumber  int ,
    RevisionNumber int ,
    OrderQuantity int ,
    UnitPrice decimal ,
    ExtendedAmount decimal,
    UnitPriceDiscountPct float ,
    DiscountAmount float ,
    ProductStandardCost decimal ,
    TotalProductCost decimal ,
    SalesAmount decimal ,
    TaxAmt decimal ,
    Freight decimal ,
    CarrierTrackingNumber String,
    CustomerPONumber String
FROM @input_file_reseller_sales
USING Extractors.Text(delimiter:'|', encoding: Encoding.Unicode);

@rs1 =
SELECT
    fis.OrderDateKey,
    fis.ProductKey,
    fis.SalesAmount+fis.TaxAmt AS Internet_Sales_Amount,
    frs.SalesAmount+frs.TaxAmt AS Reseller_Sales_Amount
FROM @fact_internet_sales AS fis
     INNER JOIN @fact_reseller_sales AS frs
     ON fis.ProductKey == frs.ProductKey
WHERE
    fis.OrderDateKey < frs.OrderDateKey
    AND fis.OrderDateKey == 20010701
    AND frs.SalesAmount+frs.TaxAmt > 20000;

@rs2 =
COMBINE @fact_internet_sales AS fis
WITH @fact_reseller_sales AS frs
ON fis.ProductKey == frs.ProductKey
PRODUCE OrderDateKey int,
        ProductKey int,
        Internet_Sales_Amount decimal,
        Reseller_Sales_Amount decimal
USING new USQL_Programmability.CombineSales();

OUTPUT @rs1 TO @output_file1 USING Outputters.Tsv();
OUTPUT @rs2 TO @output_file2 USING Outputters.Tsv();
```

Der benutzerdefinierte Combiner kann entweder als eine neue Instanz des Applierobjekts aufgerufen werden:

```c#
    USING new MyNameSpace.MyCombiner();
```


Oder mit Aufrufen einer Wrapper-Factory-Methode:

```c#
    USING MyNameSpace.MyCombiner();
```

## <a name="user-defined-reducer"></a>Benutzerdefinierter Reducer
U-SQL bietet die Möglichkeit, benutzerdefinierte Rowsetreducer in C# zu schreiben. Hierzu müssen Sie das Framework der benutzerdefinierten Operatorerweiterbarkeit nutzen, indem Sie eine IReducer-Schnittstelle implementieren.

Mit dem benutzerdefinierten Reducer (User-Defined Reducer, UDR) können Sie überflüssige Zeilen während der Datenextraktion (Import) entfernen. Er kann auch zum Bearbeiten und Auswerten von Zeilen/Spalten verwendet werden und basiert auf Programmierlogik, die definiert, welche Zeilen extrahiert werden müssen.

Zum Definieren einer UDR-Klasse müssen wir eine `IReducer`-Schnittstelle mit optionalem `SqlUserDefinedReducer`-Attribut erstellen.

Diese Klassenschnittstelle muss die Definition für die Außerkraftsetzung des Rowsets der `IEnumerable`-Schnittstelle enthalten.

```c#
        [SqlUserDefinedReducer]
        public class EmptyUserReducer : IReducer
        {

            public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
            {
            …
            }

        }
```

Das **SqlUserDefinedReducer**-Attribut gibt an, dass der Typ als benutzerdefinierter Reducer registriert werden muss. Diese Klasse kann nicht vererbt werden.

SqlUserDefinedReducer ist ein optionales Attribut zur Definition eines benutzerdefinierten Reducers. Es wird verwendet, um die IsRecursive-Eigenschaft zu definieren.

* bool     IsRecursive    
* **true**  = Gibt an, ob dieser Reducer idempotent ist.

Die wichtigsten Programmierbarkeitsobjekte sind *input* und *output*. Das input-Objekt dient zum Aufzählen von Eingabedaten, und „output“, um Ausgabedaten als Ergebnis der Reduzierungsaktivität festzulegen.

Zur Eingabezeilen-Aufzählung verwenden wir die `Row.Get`-Methode.

```c#
            foreach (IRow row in input.Rows)
            {
                        row.Get<string>("mycolumn");
            }
```

Einige wichtige Punkte sind hier zu beachten. Der Parameter, der der `Row.Get`-Methode übergeben wird, ist eine Spalte, die als Teil der `PRODUCE`-Klasse der `REDUCE`-Anweisung des U-SQL-Basisskripts übergeben wird. Wir müssen hier ebenfalls den richtigen Datentyp verwenden.

Für die Ausgabe – output.Set-Methode

Beachten Sie unbedingt, dass der benutzerdefinierte Reducer nur mit dem Aufruf der output.Set-Methode definierte Werte ausgibt.

```c#
    output.Set<string>("mycolumn", guid);
```

Die tatsächliche Reducerausgabe wird durch den Aufruf von `yield return output.AsReadOnly()` ausgelöst.

Hier ist ein Reducerbeispiel.

```c#
        [SqlUserDefinedReducer]
        public class EmptyUserReducer : IReducer
        {

            public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
            {
                string guid;
                DateTime dt;
                string user;
                string des;

                foreach (IRow row in input.Rows)
                {
                    guid = row.Get<string>("guid");
                    dt = row.Get<DateTime>("dt");
                    user = row.Get<string>("user");
                    des = row.Get<string>("des");

                    if (user.Length > 0)
                    {
                        output.Set<string>("guid", guid);
                        output.Set<DateTime>("dt", dt);
                        output.Set<string>("user", user);
                        output.Set<string>("des", des);

                        yield return output.AsReadOnly();
                    }
                }
            }

        }
```

In diesem Anwendungsfall überspringt der Reducer Zeilen mit leerem Benutzernamen. Für jede Zeile im Rowset liest er jede erforderliche Spalte, wertet dann die Länge des Benutzernamens aus und gibt die tatsächliche Zeile nur dann aus, wenn der Längenwert des Benutzernamens größer als 0 ist.

U-SQL-Basisskript, das einen benutzerdefinierten Reducer verwendet

```sql
DECLARE @input_file string = @"\usql-programmability\input_file_reducer.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        guid string,
        dt DateTime,
        user String,
        des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    REDUCE @rs0 PRESORT guid
    ON guid  
    PRODUCE guid string, dt DateTime, user String, des String
    USING new USQL_Programmability.EmptyUserReducer();

@rs2 =
    SELECT guid AS start_id,
           dt AS start_time,
           DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).ToString() AS start_fiscalperiod,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 TO @output_file USING Outputters.Text();
```



<!--HONumber=Feb17_HO2-->


