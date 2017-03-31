---
title: "U-SQL-Programmierbarkeitshandbuch für Azure Data Lake | Microsoft-Dokumentation"
description: "Hier finden Sie Informationen zu den Diensten in Azure Data Lake, mit denen Sie eine cloudbasierte Big Data-Plattform erstellen können."
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
ms.sourcegitcommit: 9553c9ed02fa198d210fcb64f4657f84ef3df801
ms.openlocfilehash: ca9937d4c0d87827f8b78ecc495fdad791d139e8
ms.lasthandoff: 03/23/2017


---

# <a name="u-sql-programmability-guide"></a>U-SQL-Programmierbarkeitshandbuch
## <a name="azure-data-lake"></a>Azure Data Lake
Azure Data Lake bietet Funktionen, mit denen Entwickler und (Daten-)Analysten komfortabel Daten beliebiger Größe, Art und Geschwindigkeit speichern können. Darüber hinaus stehen Entwicklern zahlreiche plattform- und sprachübergreifende Verarbeitungs- und Analysemöglichkeiten zur Verfügung. Data Lake vereinfacht die Erfassung und Speicherung Ihrer Daten und ermöglicht eine schnellere Verwendung von Batches, Streaming und interaktiven Analysen.

Bei Azure Data Lake handelt es sich um eine Sammlung von Diensten, die gemeinsam eine cloudbasierte Big Data-Plattform bereitstellen. Zu diesen Diensten gehören:

- Azure HDInsight
- Azure Data Lake Store
- Azure Data Lake Analytics

U-SQL ist eine für Big Data-Workloads konzipierte Abfragesprache. Eines der einzigartigen Features von U-SQL ist die Kombination der SQL-ähnlichen deklarativen Sprache mit dem Erweiterungs- und Programmierbarkeitspotenzial von C#. Darüber hinaus besteht die Möglichkeit, auf Schemametadaten zuzugreifen, sie zu bearbeiten und benutzerdefinierte Komponenten wie Datenprozessoren und Reducer zu erstellen.

In diesem Handbuch konzentrieren wir uns auf die durch C# ermöglichte Erweiterbarkeit und Programmierbarkeit der U-SQL-Sprache.

## <a name="requirements"></a>Anforderungen
Für die Azure Data Lake-Entwicklung müssen Sie [Azure Data Lake-Tools für Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504) herunterladen und installieren.

## <a name="get-started-with-u-sql"></a>Erste Schritte mit U-SQL  
Die U-SQL-Sprache wird in diesem Dokument nicht erläutert. Es werden jedoch grundlegende U-SQL-Konstrukte zur schrittweisen Einführung von U-SQL-Programmierbarkeitsfeatures beschrieben. Weitere Informationen finden Sie in der [U-SQL Language Reference](http://aka.ms/usql_reference) (Referenz zur U-SQL-Sprache).

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

Im diesem Beispiel haben wir eine **Eingabedatei** („input_file.tsv“), die durch die **lokale Variable** @input_file definiert wird.

Das in diesem Beispiel gezeigte U-SQL-Skript führt folgende Aktionen aus:

* Zu Anfang lädt die **EXTRACT**-Anweisung Daten durch Konvertieren der Eingabedatei in das **Arbeitsspeicherrowset** in den Arbeitsspeicher.
* **SELECT** aggregiert Daten aus dem Datenrowset und bereitet sie zum Exportieren vor.
* **OUTPUT** exportiert das Datenrowset in die (externe) **Ausgabedatei**.

Betrachten wir zunächst einige Optionen zur direkten Verwendung eines C#-Ausdrucks in einem U-SQL-Skript.

## <a name="c-types-and-expressions-in-u-sql-script"></a>C#-Typen und -Ausdrücke in einem U-SQL-Skript
Bei einem U-SQL-C#-Ausdruck handelt es sich (ähnlich wie bei einem Ausdruck im allgemeinem C#) um eine Sequenz mit mindestens einem Operator, die zu einem einzelnen Wert, Objekt, einer einzelnen Methode oder einem einzelnen Namespace ausgewertet werden kann. Ausdrücke können aus einem Literalwert, einem Methodenaufruf, einem Operator oder einem einfachen Name bestehen. Einfache Namen können der Name einer Variablen, eines Typmembers, Methodenparameters, Namespaces oder Typs sein.

Wenn wir über U-SQL-C#-Ausdrücke sprechen, beziehen wir uns spezifisch auf die C#-Ausdrücke im U-SQL-Basisskript. Der zugrunde liegende C#-CodeBehind-Abschnitt, der im weiteren Verlauf dieses Dokuments behandelt wird, kann auch C#-Ausdrücke als reguläre C#-codebasierte Elemente enthalten.

Ausdrücke können Operatoren verwenden, die andere Ausdrücke als Parameter verwenden. Sie können auch Methodenaufrufe mit Parametern verwenden, bei denen es sich um andere Methodenaufrufe handelt. Beispiele für einen Ausdruck:  

```c#
    Convert.ToDateTime(Convert.ToDateTime(dt).ToString("yyyy-MM-dd"))
```

Die U-SQL-Sprache ermöglicht die Verwendung standardmäßiger C#-Ausdrücke aus integrierten Namespaces.  

```c#
    Microsoft.Analytics.Interfaces;  
    Microsoft.Analytics.Types.Sql;  
    System;  
    System.Collections.Generic;  
    System.Linq;  
    System.Text;  
```

Allgemeine C#-Ausdrücke können in U-SQL in SELECT und EXTRACT verwendet werden.

Die C#-Ausdrücke können auch in DECLARE- oder IF-Anweisungen verwendet werden. Ein Beispiel für diese Art von Ausdruck:   

```c#
    DateTime.Today.Day   
    Convert.ToDateTime
```

U-SQL-basiertes Skriptbeispiel:  

```sql
    DECLARE @default_dt DateTime = Convert.ToDateTime("06/01/2016");
```

C#-Ausdrücke können erweiterte Funktionen bieten, wenn Sie Spalten als Teil eines Rowsets bearbeiten. Wenn Sie also etwa eine DateTime-Spalte in das Datum mit null Stunden konvertieren möchten, können Sie den folgenden SELECT-Teil eines U-SQL-basierten Skripts verwenden:

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

Wie Sie sehen, verwenden wir die `System.Convert.ToDateTime`-Methode, um die Konvertierung zu durchlaufen.

Im Anschluss finden Sie ein etwas komplizierteres Szenario, das die Verwendung einiger grundlegender C#-Operatoren veranschaulicht:

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

Hier sehen Sie ein Beispiel für einen Ausdruck mit einem bedingten C#-Operator.

Die vorherigen Beispiele veranschaulichen die Verwendung von C#-Ausdrücken im U-SQL-Basisskript. U-SQL bietet jedoch noch umfangreichere Programmierbarkeitsfeatures. Diese werden im weiteren Verlauf dieses Dokuments behandelt.  

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

### <a name="use-c-expressions-for-data-type-conversions"></a>Verwenden von C#-Ausdrücken für Datentypkonvertierungen
Im folgenden Beispiel sehen Sie, wie Sie mithilfe von C#-Ausdrücken eine DateTime-Datenkonvertierung durchführen. In diesem speziellen Szenario werden DateTime-Zeichenfolgendaten in das Standard-DateTime-Format mit der Zeitnotation „00:00:00“ für Mitternacht konvertiert.

```sql
DECLARE @dt String = "2016-07-06 10:23:15";
@rs1 =
    SELECT Convert.ToDateTime(Convert.ToDateTime(@dt).ToString("yyyy-MM-dd")) AS dt,
           dt AS olddt
    FROM @rs0;
OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

### <a name="use-c-expressions-for-todays-date"></a>Verwenden von C#-Ausdrücken für das aktuelle Datum
Das aktuelle Datum können wir mithilfe des folgenden C#-Ausdrucks abrufen:

```c#
DateTime.Now.ToString("M/d/yyyy")
```

Das folgende Beispiel zeigt, wie Sie diesen Ausdruck in einem Skript verwenden:

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

## <a name="use-inline-c-function-expressions"></a>Verwenden von C#-Inlinefunktionsausdrücken
Mit U-SQL können Sie Inlinefunktionsausdrücke als Teil von C#-Ausdrücken definieren. Dadurch entstehen zusätzliche Verwendungsmöglichkeiten für C#-Funktionen mit Ausgabeverweisparametern.

Das folgende Beispiel zeigt eine allgemeine Definition für einen Inlinefunktionsausdruck:

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

In diesem Beispiel wird eine Inlinefunktion mit dem Zeichenfolgen-Eingabeparameter „input_p“ definiert. Innerhalb dieser Funktion überprüfen wir, ob die Eingabezeichenfolge ein gültiger DateTime-Wert ist. Falls ja, wird sie zurückgegeben; andernfalls wird NULL zurückgegeben.

Die Inlinefunktion ist in diesem Szenario notwendig, da die Funktion „DateTime.TryParse“ den Ausgabeparameter `out dt_result` enthält. Wir definieren ihn als `DateTime dt_result;`.


## <a name="verify-data-type-values"></a>Überprüfen von Datentypwerten
Einige C#-Funktionen können in U-SQL-Basisskripts nicht direkt als C#-Ausdrücke verwendet werden. Das gilt insbesondere für Funktionen, die einen Ausgabeverweisparameter erfordern. Diese Funktionen können jedoch, wie bereits erläutert, als Teil eines Inlinefunktionsausdrucks definiert und verwendet werden.

### <a name="use-inline-function-expressions"></a>Verwenden von Inlinefunktionsausdrücken
Um die Gültigkeit des DateTime-Werts zu überprüfen, können wir `DateTime.TryParse` verwenden.

Das folgende vollständige Beispiel zeigt, wie Sie mithilfe eines Inlinefunktionsausdrucks den Datentypwert mit `DateTime.TryParse` überprüfen.

In diesem Szenario überprüfen wir den DateTime-Datentypwert:

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

### <a name="use-code-behind"></a>Verwenden des CodeBehinds
Um die gleiche Funktion im CodeBehind-Abschnitt des U-SQL-Programms zu verwenden, definieren wir die C#-Funktion „ToDateTime“.

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

Hier ist die CodeBehind-Funktion:

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

## <a name="use-code-behind"></a>Verwenden des CodeBehinds
CodeBehind ist ein C#-Programmierbarkeitsabschnitt des U-SQL-Projekts. Grundsätzlich ist CodeBehind eine kompilierte Assembly (DLL), auf die im U-SQL-Skript verwiesen wird. Mit Visual Studio-Tools können Sie einen C#-Programmierbarkeitsabschnitt als Teil des U-SQL-Projekts verwalten und debuggen.

Ein typisches in Visual Studio erstelltes U-SQL-Projekt besteht aus zwei Teilen: einem Basisskript und einer Datei mit der Erweiterung **.usql**.

![Basisskriptdatei](./media/data-lake-analytics-u-sql-programmability-guide/base-script-file.png)


Typisches Projektmappenprojekt:   
![Typisches Projektmappenprojekt](./media/data-lake-analytics-u-sql-programmability-guide/typical-solution-project.png)


Im zweiten Teil des Projekts rufen wir die CodeBehind-Datei „Script.usql.cs“ auf.  
![CodeBehind](./media/data-lake-analytics-u-sql-programmability-guide/code-behind.png)

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

Die Vorschau der CodeBehind-Vorlage wird automatisch generiert. Diese Datei enthält eine Namespace-Standarddefinition für Programmierbarkeitsobjekte. Während der Ausführung des Projekts wird sie kompiliert, und in einem U-SQL-Basisskript wird auf sie verwiesen.

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

Mögliche Optionen für Programmierbarkeitsobjekte sind unter anderem UDF (User-Defined Function; benutzerdefinierte Funktion), UDT (User-Defined Type; benutzerdefinierter Typ) PROCESS und REDUCER.

## <a name="register-u-sql-assemblies"></a>Registrieren von U-SQL-Assemblys
Das U-SQL-Erweiterbarkeitsmodell hängt stark davon ab, benutzerdefinierten Code hinzufügen zu können. Mit U-SQL können Sie derzeit komfortabel Ihren eigenen, Microsoft .NET-basierten Code hinzuzufügen. Das gilt insbesondere für C#. Aber auch benutzerdefinierter Code in anderen .NET-Sprachen (wie VB.NET oder F#) kann hinzugefügt werden.

Sie können sogar eine eigene Laufzeit für andere Sprachen bereitstellen, müssen aber weiterhin selbst über eine .NET-Ebene für Interoperabilität sorgen. Falls Sie sich die Unterstützung einer bestimmten Sprache wünschen, reichen Sie eine Featureanforderung ein, oder hinterlassen Sie einen Kommentar: http://aka.ms/adlfeedback.

### <a name="learn-the-difference-between-code-behind-and-assembly-registration-through-azure-data-lake-tools-in-visual-studio"></a>Unterschied zwischen CodeBehind und Assemblyregistrierung über Azure Data Lake-Tools in Visual Studio
Die einfachste Möglichkeit zur Verwendung von benutzerdefiniertem Code ist die Verwendung der Azure Data Lake-Tools für die CodeBehind-Funktionen von Visual Studio.

Wie bereits erwähnt, tragen Sie den benutzerdefinierten Code für das Skript (z.B. „Script.usql“) in die zugehörige CodeBehind-Datei (z.B. „Script.usql.cs“) ein.

![CodeBehind-Beispiel](./media/data-lake-analytics-u-sql-programmability-guide/code-behind-example.jpg)
**Abbildung 1:** CodeBehind-Beispiel in Azure Data Lake-Tools in Visual Studio. (Klicken Sie auf das Bild, um es zu vergrößern. Beispielcode steht [hier](https://github.com/Azure/usql/tree/master/Examples/TweetAnalysis) zur Verfügung.)


Der Vorteil von CodeBehind ist, dass die Tools die folgenden Schritte für Sie übernehmen, wenn Sie Ihr Skript übermitteln:  

1. Sie erstellen die Assembly für die CodeBehind-Datei.  

2. Sie fügen dem Skript einen Prolog hinzu, der die Assemblydatei mithilfe der Anweisung [CREATE ASSEMBLY](https://msdn.microsoft.com/library/azure/mt763293.aspx) registriert. Sie verwenden außerdem [REFERENCE ASSEMBLY] (https://msdn.microsoft.com/library/azure/mt763294.aspx), um die Assembly in den Kontext des Skripts zu laden.

3. Sie fügen dem Skript einen Epilog hinzu, der die vorübergehend registrierte Assembly mithilfe von [DROP ASSEMBLY](https://msdn.microsoft.com/library/azure/mt763295.aspx) wieder entfernt.

Sie können den generierten Prolog und Epilog anzeigen, wenn Sie das Skript öffnen:

![Generierter Prolog](./media/data-lake-analytics-u-sql-programmability-guide/generated-prologue.png)

**Abbildung 2**: Automatisch für CodeBehind generierter Prolog und Epilog
<br />

CodeBehind hat unter anderem folgende Nachteile:

* Der Code wird für jede Skriptübermittlung hochgeladen.
* Die Funktionen können nicht mit anderen geteilt werden.

Daher können Sie Ihrer Projektmappe ein separates C#-Klassenbibliotheksprojekt (für U-SQL) hinzufügen (Abbildung 3), den Code entwickeln oder vorhandenen CodeBehind-Code kopieren (ohne Änderungen am C#-Code vornehmen zu müssen, wie in Abbildung 4 zu sehen). Verwenden Sie dann für das Projekt die Menüoption **Assembly registrieren**, um die Assembly zu registrieren (wie in Schritt 1 der Abbildung 5 zu sehen).

![Erstellen des Projekts](./media/data-lake-analytics-u-sql-programmability-guide/creating-project.png)
**Abbildung 3:** Erstellen eines U-SQL-C#-Codeprojekts  
<br />

![Klassenbibliothek](./media/data-lake-analytics-u-sql-programmability-guide/class-library.png)
**Abbildung 4:** U-SQL-C#-Klassenbibliothek neben der CodeBehind-Datei
<br />

![Registrieren des Codes](./media/data-lake-analytics-u-sql-programmability-guide/register-code.png)
**Abbildung 5:** Registrieren des U-SQL-C#-Codeprojekts
<br />

Das Registrierungsdialogfeld (siehe Schritt 2 der Abbildung 5) enthält die Registrierungsoptionen für die Assembly. Hier können Sie beispielsweise das zu verwendende Data Lake Analytics-Konto und die zu verwendende Datenbank angeben. Darüber hinaus enthält es Informationen zur Benennung der Assembly. (Der lokale Assemblypfad wird vom Tool automatisch ausgefüllt.) Des Weiteren stehen eine Option zum erneuten Registrieren einer bereits registrierten Assembly und zwei Optionen zum Hinzufügen zusätzlicher Abhängigkeiten zur Verfügung:

**Verwaltete Abhängigkeiten:** Zeigt die benötigten verwalteten Assemblys an. Jede ausgewählte Assembly wird einzeln registriert, und Skripts können darauf verweisen. Dies wird für andere .NET-Assemblys benötigt.

**Zusätzliche Dateien:** Ermöglicht das Hinzufügen zusätzlicher Ressourcendateien, die von der Assembly benötigt werden. Diese werden zusammen mit der Assembly registriert und automatisch geladen, wenn auf die Assembly verwiesen wird. Verwenden Sie diese Option unter anderem für Konfigurationsdateien, native Assemblys sowie für Laufzeiten anderer Sprachen und für deren Ressourcen.

In den folgenden Beispielen werden beide Optionen verwendet. Der [aktuelle Blogeintrag zur Bildverarbeitung](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/18/introducing-image-processing-in-u-sql/) ist ein weiteres Beispiel für die Verwendung einer vordefinierten Assembly, die diese Optionen zur Registrierung verwenden kann.

Nun können Sie in einem beliebigen U-SQL-Skript, das über Berechtigungen für die Datenbank der registrierten Assemblys verfügt, auf die registrierten Assemblys verweisen. (Weitere Informationen finden Sie im Code des U-SQL-Skripts in Abbildung 4.) Für jede separat registrierte Assembly muss ein Verweis hinzugefügt werden. Die zusätzlichen Ressourcendateien werden automatisch bereitgestellt. Dieses Skript sollte nicht mehr über eine CodeBehind-Datei für den Code in referenzierten Assemblys verfügen, die CodeBehind-Datei kann jedoch weiterhin anderen Code bereitstellen.

### <a name="register-assemblies-via-azure-data-lake-tools-in-visual-studio-and-in-u-sql-scripts"></a>Registrieren von Assemblys über Azure Data Lake-Tools in Visual Studio und in U-SQL-Skripts
Die Azure Data Lake-Tools in Visual Studio vereinfachen zwar die Registrierung einer Assembly, die Registrierung kann allerdings auch per Skript erfolgen. (Dabei werden die gleichen Schritte ausgeführt, die sonst von den Tools übernommen werden.). Diese Vorgehensweise kann beispielsweise gewählt werden, wenn Sie die Entwicklung auf einer anderen Plattform durchführen oder bereits Assemblys kompiliert haben, die Sie hochladen und registrieren möchten. Führen Sie die folgenden Schritte aus:

1. Laden Sie Ihre Assembly-DLL sowie alle erforderlichen systemfremden DLLs und Ressourcendateien in einen Speicherort Ihrer Wahl hoch. Sie können sie auch in Ihr Azure Data Lake-Speicherkonto oder in ein mit Ihrem Azure Data Lake-Konto verknüpftes Microsoft Azure Blob Storage-Konto hochladen. Dazu können Sie ein beliebiges verfügbares Uploadtool verwenden – beispielsweise Windows PowerShell-Befehle, den Data Lake Explorer-Upload der Azure Data Lake-Tools von Visual Studio, den Uploadbefehl Ihres bevorzugten SDKs oder Tools, auf die Sie über das Azure-Portal zugreifen.

1. Nachdem Sie die DLLs hochgeladen haben, registrieren Sie sie mithilfe der Anweisungen vom Typ [CREATE ASSEMBLY](https://msdn.microsoft.com/library/azure/mt763293.aspx).

Wir verwenden diesen Ansatz im folgenden spatial-Beispiel.

### <a name="register-assemblies-that-use-other-net-assemblies-based-on-the-json-and-xml-sample-library"></a>Registrieren von Assemblys, die andere .NET-Assemblys verwenden (auf der Grundlage der JSON- und XML-Beispielbibliothek)
Auf unserer [U-SQL-GitHub-Website](https://github.com/Azure/usql/) steht eine Reihe von freigegebenen Beispielassemblys zur Verfügung, die Sie verwenden können. Eine der Assemblys ([Microsoft.Analytics.Samples.Formats](https://github.com/Azure/usql/tree/master/Examples/DataFormats)) bietet Extraktoren, Funktionen und Outputter zur Verarbeitung von JSON- und XML-Dokumenten. Für die JSON- bzw. XML-Verarbeitung ist die Assembly „Microsoft.Analytics.Samples.Formats“ auf zwei vorhandene domänenspezifische Assemblys angewiesen. Sie verwendet die Bibliothek [Newtonsoft Json.Net](http://www.newtonsoft.com/) für die Verarbeitung der JSON-Dokumente und die Assembly [System.Xml](https://msdn.microsoft.com/data/bb291078.aspx) für die XML-Verarbeitung. Wir verwenden sie, um zu zeigen, wie sie registriert wird und wie die Assemblys in unseren Skripts verwendet werden.

Erst laden wir das [Visual Studio-Projekt](https://github.com/Azure/usql/tree/master/Examples/DataFormats) in unsere lokale Entwicklungsumgebung herunter (beispielsweise in Form einer lokalen Kopie, die wir mithilfe des GitHub-Tools für Windows erstellen). Anschließend öffnen wir die Projektmappe in Visual Studio, und klicken wie zuvor erläutert mit der rechten Maustaste auf das Projekt, um die Assembly zu registrieren.

Diese Assembly besitzt zwar zwei Abhängigkeiten, wir müssen jedoch nur die Abhängigkeit „Newtonsoft“ einbeziehen, da „System.xml“ bereits in Azure Data Lake verfügbar ist. (Es muss jedoch explizit darauf verwiesen werden.) Abbildung 6 zeigt, wie wir die Assembly benennen und die Newtonsoft-DLL hinzufügen. (Sie können auch einen anderen Namen ohne Punkte verwenden.) Die beiden Assemblys werden nun einzeln in der angegebenen Datenbank (beispielsweise „JSONBlog“) registriert.

![Registrieren der Assembly](./media/data-lake-analytics-u-sql-programmability-guide/register-assembly.png)

**Abbildung 6**: Registrieren der Microsoft.Analytics.Samples.Formats-Assembly aus Visual Studio
<br />

Wenn Sie selbst oder andere Benutzer, für die Sie die registrierten Assemblys durch Gewähren von Lesezugriff auf die Datenbank freigegeben haben, nun die JSON-Funktionen in ihren Skripts verwenden möchten, müssen Sie Ihrem Skript die beiden folgenden Verweise hinzufügen:

```
REFERENCE ASSEMBLY JSONBlog.[NewtonSoft.Json];
REFERENCE ASSEMBLY JSONBlog.[Microsoft.Analytics.Samples.Formats];
```

Falls Sie die XML-Funktionen verwenden möchten, müssen Sie einen Systemassemblyverweis und einen Verweis auf die registrierte Assembly hinzufügen:

```
REFERENCE SYSTEM ASSEMBLY [System.Xml];
REFERENCE ASSEMBLY JSONBlog.[Microsoft.Analytics.Samples.Formats];
```

Weitere Informationen zur Verwendung der JSON-Funktionen finden Sie in [diesem Blogbeitrag](https://blogs.msdn.microsoft.com/mrys/?p=755).

### <a name="register-assemblies-that-use-native-c-assemblies-using-the-sql-server-2016-spatial-type-assembly-from-the-feature-pack"></a>Registrieren von Assemblys, die native C++-Assemblys verwenden (unter Verwendung der SQL Server 2016-Assembly vom Typ „spatial“ aus dem Feature Pack)
Jetzt untersuchen wir ein etwas anderes Szenario. Angenommen, die zu verwendende Assembly ist von Code abhängig, der nicht auf .NET basiert. Genauer gesagt: Die Assembly ist von einer nativen C++-Assembly abhängig. Ein Beispiel für eine solche Assembly ist die SQL Server-Typassembly [Microsoft.SqlServer.Types.dll](https://www.microsoft.com/download/details.aspx?id=52676). Sie stellt .NET-basierte Implementierungen der SQL Server-Typen „hierarchyID“, „geometry“ und „geography“ bereit, die von clientseitigen SQL Server-Anwendungen für die Behandlung der SQL Server-Typen verwendet werden. (Vor SQL Server 2016 war dies ursprünglich die Assembly zur Implementierung der räumlichen SQL Server-Typen.)

Hier erfahren Sie, wie Sie diese Assembly in U-SQL registrieren.

Zuerst laden wir die Assembly aus dem [SQL Server 2016 Feature Pack](https://www.microsoft.com/download/details.aspx?id=52676) herunter und installieren sie. Wählen Sie die 64-Bit-Version des Installationsprogramms („ENU\x64\SQLSysClrTypes.msi“) aus, um die 64-Bit-Version der Bibliotheken zu erhalten.

Das Installationsprogramm installiert die verwaltete Assembly „Microsoft.SqlServer.Types.dll“ in „C:\Program Files (x86)\Microsoft SQL Server\130\SDK\Assemblies“ und die native Assembly „SqlServerSpatial130.dll“ in „\Windows\System32“\. Nun laden wir die Assemblys in unseren Azure Data Lake Store hoch (z.B. in einen Ordner mit dem Namen „/upload/asm/spatial“).

Da das Installationsprogramm die native Bibliothek im Systemordner „c:\Windows\System32“ installiert hat, müssen wir entweder „SqlServerSpatial130.dll“ vor dem Hochladen aus diesem Ordner herauskopieren oder darauf achten, dass das von uns verwendete Tool keine [Dateisystemumleitung](https://msdn.microsoft.com/library/windows/desktop/aa384187(v=vs.85).aspx) des Systemordners ausführt.

Wenn Sie sie also beispielsweise mit dem aktuellen Azure Data Lake-Explorer von Visual Studio hochladen möchten, müssen Sie die Datei zuerst in ein anderes Verzeichnis kopieren. Andernfalls wird die 32-Bit-Version hochgeladen (Stand zum Zeitpunkt der Artikelerstellung). Der Grund: Visual Studio ist eine 32-Bit-Anwendung mit Dateisystemumleitung im Auswahlfenster für die Azure Data Lake -Uploaddatei. Beim Ausführen eines U-SQL-Skripts mit Aufrufen für die native Assembly tritt dann zur Laufzeit der folgende (interne) Fehler auf:

**Interne Ausnahme von Benutzerausdruck: Es wurde versucht, ein Programm mit einem falschen Format zu laden. (Ausnahme von HRESULT:0x8007000B)**

Nach dem Hochladen der beiden Assemblydateien registrieren wir sie mit dem folgenden Skript in der Datenbank „SQLSpatial“:

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

In diesem Fall registrieren wir nur eine U-SQL-Assembly und beziehen die native Assembly als Zeichenfolgenabhängigkeit in die U-SQL-Assembly ein. Zur Verwendung der Assemblys vom Typ „spatial“ müssen wir lediglich auf die U-SQL-Assembly verweisen. Die zusätzliche Datei wird automatisch für die Assembly verfügbar gemacht. Hier sehen Sie ein einfaches Beispielskript mit der Assembly vom Typ „spatial“:

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

Die SQL-Typen-Bibliothek hat eine Abhängigkeit von der System.XML-Assembly, daher müssen wir darauf verweisen. Darüber hinaus verwenden einige der Methoden „System.Data.SqlTypes“ anstelle der integrierten C#-Typen. Da „System.Data“ bereits standardmäßig enthalten ist, können wir auf den erforderlichen SQL-Typ verweisen. Den vorherigen Code finden Sie auf unserer [GitHub-Website](https://github.com/Azure/usql/tree/master/Examples/SQLSpatialExample).


### <a name="use-assembly-versioning"></a>Verwenden der Versionsverwaltung für Assemblys
U-SQL verwendet derzeit .NET Framework 4.5. Achten Sie daher darauf, dass Ihre eigenen Assemblys mit dieser Laufzeitversion kompatibel sind.

Wie bereits erwähnt, wird Code von U-SQL in einem 64-Bit-Format (x64) ausgeführt. Stellen Sie daher sicher, dass Ihr Code für x64 kompiliert wird. Andernfalls tritt der zuvor gezeigte Formatfehler auf.

Jede hochgeladene Assembly-DLL und Ressourcendatei (beispielsweise eine andere Laufzeit, eine native Assembly oder eine Konfigurationsdatei) darf maximal 400 MB groß sein. Die Gesamtgröße der Ressourcen, die entweder mithilfe von „DEPLOY RESOURCE“ oder mittels Verweis auf Assemblys und deren zusätzliche Dateien bereitgestellt werden, darf maximal 3 GB betragen.

Beachten Sie außerdem, dass jede U-SQL-Datenbank nur eine einzelne Version einer bestimmten angegebenen Assembly enthalten kann. Wenn Sie also beispielsweise sowohl Version 7 als auch Version 8 der Bibliothek „Newtonsoft Json.NET“ benötigen, müssen diese in zwei verschiedenen Datenbanken registriert werden. Darüber hinaus kann jedes Skript nur auf eine einzelne Version einer bestimmten Assembly-DLL verweisen. Hierbei folgt U-SQL der C#-Semantik für die Assembly- und Versionsverwaltung.


## <a name="use-user-defined-functions-udf"></a>Verwenden benutzerdefinierter Funktionen (User Defined Functions, UDFs)
Benutzerdefinierte Funktionen (User Defined Functions, UDFs) von U-SQL sind Programmierroutinen, die Parameter akzeptieren, eine Aktion (beispielsweise eine komplexe Berechnung) ausführen und das Ergebnis dieser Aktion als Wert zurückgeben. Der Rückgabewert der benutzerdefinierten Funktion kann nur ein einzelner Skalarwert sein. Eine U-SQL-UDF kann im U-SQL-Basisskript wie jede andere skalare Funktion in C# aufgerufen werden.

Es empfiehlt sich, benutzerdefinierte U-SQL-Funktionen als **öffentlich** und **statisch** zu initialisieren.

```c#
        public static string MyFunction(string param1)
        {
            return "my result";
        }
```

Sehen wir uns zunächst das einfache Beispiel zur UDF-Erstellung an.

In diesem Szenario müssen wir den Geschäftszeitraum bestimmen – einschließlich des Geschäftsquartals und -monats der ersten Anmeldung für einen bestimmten Benutzer. Der erste Geschäftsmonat des Jahres ist in unserem Szenario der Juni.

Zur Berechnung des Geschäftszeitraums verwenden wir die folgende C#-Funktion:

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

Sie berechnet einfach Geschäftsmonat und -quartal und gibt einen Zeichenfolgenwert zurück. Für Juni (den ersten Monat des ersten Geschäftsquartals) verwenden wir „Q1:P1“. Für Juli verwenden wir „Q1:P2“. Und so weiter.

Dies ist eine reguläre C#-Funktion, die wir in unserem U-SQL-Projekt verwenden.

Der CodeBehind-Abschnitt sieht in diesem Szenario wie folgt aus:

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

Jetzt rufen wir diese Funktion aus dem U-SQL-Basisskript heraus auf. Zu diesem Zweck müssen wir für die Funktion einen vollqualifizierten Namen angeben und dabei auch den Namespace einbeziehen. In diesem Fall ist das „NameSpace.Class.Function(parameter)“.

```sql
    USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)
```

Hier sehen Sie das tatsächliche U-SQL-Basisskript:

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

Im Anschluss finden Sie die Ausgabedatei der Skriptausführung:

```
0d8b9630-d5ca-11e5-8329-251efa3a2941,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User1",""

20843640-d771-11e5-b87b-8b7265c75a44,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User2",""

301f23d2-d690-11e5-9a98-4b4f60a1836f,2016-02-11T09:01:33.9720000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User3",""
```

Dieses Beispiel zeigt eine einfache Verwendung einer Inline-UDF in U-SQL.

### <a name="keep-state-between-udf-invocations"></a>Beibehalten des Zustands zwischen UDF-Aufrufen
U-SQL-C#-Programmierbarkeitsobjekte können komplexer sein und Interaktivität über die globalen CodeBehind-Variablen nutzen. Sehen wir uns einmal das folgende Geschäftsszenario an:

In großen Organisationen können Benutzer zwischen Varianten interner Anwendungen wechseln. Beispiele wären etwa Microsoft Dynamics CRM und PowerBI. Kunden möchten unter Umständen mithilfe einer Telemetrieanalyse ermitteln, wie Benutzer zwischen verschiedenen Anwendungen wechseln, welche Nutzungstrends zu beobachten sind und Ähnliches. Das Unternehmen möchte so die Anwendungsnutzung optimieren. Außerdem möchte das Unternehmen unter Umständen verschiedene Anwendungen oder bestimmte Anmelderoutinen miteinander kombinieren.

Hierzu müssen wir Sitzungs-IDs und die Verzögerung für die letzte Sitzung bestimmen.

Wir müssen eine vorherige Anmeldung finden und sie allen Sitzungen zuweisen, die für die gleiche Anwendung generiert werden. Die erste Herausforderung besteht darin, dass es uns mit dem U-SQL-Basisskript nicht möglich ist, Berechnungen auf bereits berechnete Spalten mit LAG-Funktion anzuwenden. Die zweite Herausforderung ist, dass wir die bestimmte Sitzung für alle Sitzungen im gleichen Zeitraum halten müssen.

Dieses Problem lösen wir mit einer globalen Variablen in einem CodeBehind-Abschnitt: `static public string globalSession;`.

Diese globale Variable wird bei der Skriptausführung auf das gesamte Rowset angewendet.

Hier ist der CodeBehind-Abschnitt unseres U-SQL-Programms:

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

Dieses Beispiel zeigt die globale Variable `static public string globalSession;`, die innerhalb der Funktion `getStampUserSession` verwendet und bei jeder Änderung des Sitzungsparameters neu initialisiert wird.

Das U-SQL-Basisskript sieht wie folgt aus:

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

Die Funktion `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` wird hier während der Berechnung des zweiten Arbeitsspeicherrowsets aufgerufen. Sie übergibt die Spalte `UserSessionTimestamp` und gibt den Wert zurück, bis `UserSessionTimestamp` geändert wurde.

Die Ausgabedatei sieht wie folgt aus:

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

Dieses Beispiel zeigt ein etwas komplizierteres Szenario, in dem wir eine globale Variable im CodeBehind-Abschnitt verwenden und auf das gesamte Arbeitsspeicherrowset anwenden.

## <a name="use-user-defined-types-udt"></a>Verwenden benutzerdefinierter Typen (User-Defined Types, UDTs)
Benutzerdefinierte Typen (User-Defined Types, UDTs) sind ein weiteres Programmierbarkeitsfeature von U-SQL. Ein U-SQL-UDT verhält sich wie ein regulärer benutzerdefinierter C#-Typ. C# ist eine stark typisierte Sprache, die die Verwendung integrierter und benutzerdefinierter Typen ermöglicht.

Beliebige UDTs können von U-SQL nicht implizit serialisiert oder deserialisiert werden, wenn die UDT zwischen Scheitelpunkten in Rowsets übergeben wird. Der Benutzer muss daher die IFormatter-Schnittstelle verwenden, um einen expliziten Formatierer bereitzustellen. Dadurch erhält U-SQL die Serialisierungs- und Deserialisierungsmethoden für die UDT.

> [!NOTE]
> Die in U-SQL integrierten Extraktoren und Outputter können derzeit keine UDT-Daten in oder aus Dateien serialisieren oder deserialisieren – unabhängig davon, ob IFormatter festgelegt wurde. Wenn Sie also mithilfe der OUTPUT-Anweisung UDT-Daten in eine Datei schreiben oder mit einem Extraktor lesen, müssen Sie sie als Zeichenfolge oder Bytearray übergeben. Anschließend müssen Sie explizit den Serialisierungs- und Deserialisierungscode (sprich: die ToString()-Methode des UDTs) aufrufen. Benutzerdefinierte Extraktoren und Outputter können hingegen UDTs lesen und schreiben.

Angenommen, wir versuchen, den UDT in EXTRACTOR oder OUTPUTTER (außerhalb des vorherigen SELECT-Ausdrucks) zu verwenden, wie hier zu sehen:

```sql
@rs1 =
    SELECT MyNameSpace.Myfunction_Returning_UDT(filed1) AS myfield
    FROM @rs0;

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

In diesem Fall erhalten wir folgende Fehlermeldung:

```
    Error    1    E_CSC_USER_INVALIDTYPEINOUTPUTTER: Outputters.Text was used to output column myfield of type
    MyNameSpace.Myfunction_Returning_UDT.

    Description:

    Outputters.Text only supports built-in types.

    Resolution:

    Implement a custom outputter that knows how to serialize this type, or call a serialization method on the type in
    the preceding SELECT.    C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\
    USQL-Programmability\Types.usql    52    1    USQL-Programmability
```

Für die Zusammenarbeit von UDT und Outputter müssen wir den UDT entweder mit der ToString()-Methode in eine Zeichenfolge serialisieren oder einen benutzerdefinierten Outputter erstellen.

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

* Fügen Sie die folgenden Namespaces hinzu:

```c#
    using Microsoft.Analytics.Interfaces
    using System.IO;
```

* Fügen Sie `Microsoft.Analytics.Interfaces` hinzu. Dieses Element wird für die UDT-Schnittstellen benötigt. Außerdem wird `System.IO` unter Umständen zum Definieren der IFormatter-Schnittstelle benötigt.

* Definieren Sie einen benutzerdefinierten Typ mit dem SqlUserDefinedType-Attribut.

**SqlUserDefinedType** wird verwendet, um in U-SQL die Definition eines Typs in einer Assembly als benutzerdefinierten Typ (UDT) zu markieren. Die Eigenschaften des Attributs spiegeln die physischen Merkmale des UDT wider. Diese Klasse kann nicht vererbt werden.

SqlUserDefinedType ist ein erforderliches Attribut für die UDT-Definition.

Der Konstruktor der Klasse:  

* SqlUserDefinedTypeAttribute (Typformatierung)

* Typformatierung: Erforderlicher Parameter zum Definieren einer UDT-Formatierung. Genauer gesagt muss hier die Art der `IFormatter`-Schnittstelle übergeben werden.

```c#
    [SqlUserDefinedType(typeof(MyTypeFormatter))]
      public class MyType
              {
             …
           }
```

* Für einen typischen UDT muss außerdem die IFormatter-Schnittstelle definiert werden, wie im folgenden Beispiel zu sehen:

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

Die `IFormatter`-Schnittstelle serialisiert und deserialisiert ein Objektdiagramm mit dem Stammtyp \<typeparamref name="T">.

\<Typeparam Name = "T"> – der Stammtyp für Serialisierung und Deserialisierung des Objektgraphs.

* **Deserialize:** Deserialisiert die Daten im angegebenen Datenstrom und stellt das Objektdiagramm wieder her.

* **Serialize:** Serialisiert ein Objekt oder Objektdiagramm mit dem angegebenen Stamm in den angegebenen Datenstrom.

`MyType`-Instanz: Die Instanz des Typs.  
`IColumnWriter`-Writer/`IColumnReader`-Reader: Der zugrunde liegende Spaltendatenstrom.  
`ISerializationContext`-Kontext: Enumeration, die eine Gruppe von Flags definiert, die den Quell- oder Zielkontext für den Datenstrom während der Serialisierung angibt.

   * **Intermediate:** Gibt an, dass der Quell- oder Zielkontext kein permanenter Speicher ist.

   * **Persistence:** Gibt an, dass der Quell- oder Zielkontext ein permanenter Speicher ist.

Als regulärer C#-Typ kann eine U-SQL-UDT-Definition Überschreibungen für Operatoren wie +/==/!= enthalten. Sie kann auch statische Methoden enthalten. Wenn wir diesen UDT beispielsweise als Parameter für eine U-SQL-MIN-Aggregatfunktion verwenden, müssen wir die Überschreibung des <-Operators definieren.

Weiter oben in dieser Anleitung haben wir uns mit einem Beispiel zur Identifizierung des Geschäftszeitraums ab einem bestimmten Datum im Format Qn:Pn (Q1:P10) beschäftigt. Das folgende Beispiel zeigt das Definieren eines benutzerdefinierten Typs für Geschäftszeitraumwerte.

Im Anschluss sehen Sie ein Beispiel für einen CodeBehind-Abschnitt mit UDT und IFormatter-Schnittstelle:

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

Der definierte Typ umfasst zwei Zahlen: Quartal und Monat. Die Operatoren ==/!=/>/< und die statische Methode ToString() sind hier definiert.

Wie bereits erwähnt, kann der UDT in SELECT-Ausdrücken verwendet werden. In OUTPUTTER-/EXTRACTOR-Ausdrücken ist dies hingegen nur mit benutzerdefinierter Serialisierung möglich. Er muss entweder mithilfe von „ToString()“ als Zeichenfolge serialisiert oder mit einem benutzerdefinierten OUTPUTTER-/EXTRACTOR-Ausdruck verwendet werden.

Jetzt betrachten wird die Verwendung des UDT. In einem CodeBehind-Abschnitt haben wir unsere GetFiscalPeriod-Funktion wie folgt geändert:

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

Wie Sie sehen, wird der Wert unseres FiscalPeriod-Typs zurückgegeben.

Hier finden Sie ein Beispiel zur weiteren Verwendung im U-SQL-Basisskript. Dieses Beispiel zeigt verschiedene Möglichkeiten des UDT-Aufrufs im U-SQL-Skript.

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

Im Anschluss folgt ein Beispiel für einen vollständigen CodeBehind-Abschnitt:

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

## <a name="use-user-defined-aggregates-udagg"></a>Verwenden benutzerdefinierter Aggregate (User-Defined Aggregates, UDAGGs)
Benutzerdefinierte Aggregate sind alle aggregationsbezogenen Funktionen, die nicht standardmäßig in U-SQL verfügbar sind. Ein Beispiel wäre etwa ein Aggregat zur Durchführung benutzerdefinierter mathematischer Berechnungen, Zeichenfolgenverkettungen oder Bearbeitungen mit Zeichenfolgen.

Die Basisklassendefinition eines benutzerdefinierten Aggregats sieht wie folgt aus:

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


Mithilfe der Basisklasse können Sie drei abstrakte Parameter übergeben: zwei als Eingabe und einen als Ergebnis. Die Datentypen sind variabel und sollten während der Klassenvererbung definiert werden.

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

* **Init** wird während der Berechnung für jede Gruppe einmal aufgerufen. Bietet eine Initialisierungsroutine für jede Aggregationsgruppe.  
* **Accumulate** wird für jeden Wert einmal ausgeführt. Stellt die Hauptfunktion für den Aggregationsalgorithmus bereit. Kann zum Aggregieren von Werten mit verschiedenen, während der Klassenvererbung definierten Datentypen verwendet werden. Kann zwei Parameter mit variablen Datentypen akzeptieren.
* **Terminate** wird am Ende der Verarbeitung einmal pro Aggregationsgruppe ausgeführt, um das Ergebnis für die einzelnen Gruppen auszugeben.

Verwenden Sie die Klassendefinition wie folgt, um korrekte Ein- und Ausgabedatentypen zu deklarieren:

```c#
    public abstract class IAggregate<T1, T2, TResult> : IAggregate
```

* T1: Erster Parameter für „accumulate“
* T2: Zweiter Parameter für „accumulate“
* TResult: Rückgabetyp von „Terminate“

Beispiel:

```c#
    public class GuidAggregate : IAggregate<string, int, int>
```

oder

```c#
    public class GuidAggregate : IAggregate<string, string, string>
```

### <a name="use-udagg-in-u-sql"></a>Verwenden von UDAGGs in U-SQL
Wenn Sie ein UDAGG verwenden möchten, definieren Sie es zuerst im CodeBehind, oder verweisen Sie über die vorhandene Programmierbarkeits-DLL darauf (wie weiter oben bereits erläutert).

Verwenden Sie dann die folgende Syntax:

```c#
    AGG<UDAGG_functionname>(param1,param2)
```

Hier sehen Sie ein UDAGG-Beispiel:

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

U-SQL-Basisskript:

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

In diesem Szenario verketten wir Klassen-GUIDs für bestimmte Benutzer.

## <a name="use-user-defined-objects-udo"></a>Verwenden benutzerdefinierter Objekte (User-Defined Objects, UDOs)
Mit U-SQL können Sie angepasste Programmierbarkeitsobjekte – so genannte benutzerdefinierte Objekte (User-Defined Objects ,UDOs) – definieren.

Im Anschluss finden Sie eine Liste mit UDOs in U-SQL:

* Benutzerdefinierte Extraktoren
    * Zeilenweise Extraktion
    * Wird verwendet, um die Datenextraktion aus benutzerdefinierten strukturierten Dateien zu implementieren.

* Benutzerdefinierte Outputter
    * Zeilenweise Ausgabe
    * Wird zur Ausgabe benutzerdefinierter Datentypen oder Dateiformate verwendet.

* Benutzerdefinierte Prozessoren
    * Eine Zeile nehmen und eine Zeile erzeugen
    * Wird verwendet, um die Anzahl der Spalten zu verringern oder eine neue Spalte mit Werten zu erzeugen, die vom vorhandenen Spaltensatz abgeleitet werden.

* Benutzerdefinierte Applier
    * Eine Zeile nehmen und 0 bis n Zeilen erzeugen
    * Wird mit OUTER/CROSS APPLY verwendet.

* Benutzerdefinierte Combiner
    * Kombiniert Rowsets – benutzerdefinierte JOINs.

* Benutzerdefinierte Reducer
    * N Zeilen nehmen und eine Zeile erzeugen
    * Wird verwendet, um die Zeilenanzahl zu verringern.

UDOs werden im U-SQL-Skript in der Regel explizit als Teil der folgenden U-SQL-Anweisungen aufgerufen:

* EXTRACT
* OUTPUT
* PROCESS
* COMBINE
* REDUCE

## <a name="use-user-defined-extractors"></a>Verwenden benutzerdefinierter Extraktoren
In U-SQL können Sie externe Daten mithilfe einer EXTRACT-Anweisung importieren. Eine EXTRACT-Anweisung kann integrierte UDO-Extraktoren verwenden:  

* *Extractors.Text()*: Extraktion aus durch Trennzeichen getrennten Textdateien mit verschiedenen Codierungen.

* *Extractors.Csv()*: Extraktion aus durch Trennzeichen getrennten Dateien (CSV) mit verschiedenen Codierungen.

* *Extractors.Tsv()*: Extraktion aus durch Tabstopp getrennten Dateien (TSV) mit verschiedenen Codierungen.

Es empfiehlt sich unter Umständen, einen benutzerdefinierten Extraktor zu entwickeln. Dies kann hilfreich sein, wenn wir während des Datenimports eine der folgenden Aufgaben ausführen möchten:

* Ändern von Eingabedaten durch Aufteilen von Spalten und Ändern einzelner Werte. Die PROCESSOR-Funktion eignet sich besser zum Kombinieren von Spalten.
* Analysieren unstrukturierter Daten wie Webseiten und E-Mails oder teilweise unstrukturierter Daten wie XML/JSON.
* Analysieren von Daten mit nicht unterstützter Codierung.

Um einen benutzerdefinierten Extraktor (User-Defined Extractor, UDE) zu definieren, müssen wir eine `IExtractor`-Schnittstelle erstellen. Alle Eingabeparameter des Extraktors (wie etwa Spalten-/Zeilentrennzeichen und die Codierung) müssen im Konstruktor der Klasse definiert werden. Die `IExtractor`-Schnittstelle muss auch eine Definition für die `IEnumerable<IRow>`-Überschreibung enthalten:

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

Das Attribut **SqlUserDefinedExtractor** gibt an, dass der Typ als benutzerdefinierter Extraktor registriert werden muss. Diese Klasse kann nicht vererbt werden.

SqlUserDefinedExtractor ist ein optionales Attribut für die UDE-Definition. Es wird verwendet, um die AtomicFileProcessing-Eigenschaft für das UDE-Objekt zu definieren.

* bool     AtomicFileProcessing   

* **true:** Gibt an, dass dieser Extraktor atomische Eingabedateien (JSON, XML, ...) benötigt.
* **false:** Gibt an, dass dieser Extraktor aufgeteilte/verteilte Dateien (CSV, SEQ, ...) behandeln kann.

Die wichtigsten UDE-Programmierbarkeitsobjekte sind **input** und **output**. Das Eingabeobjekt dient zum Aufzählen von Eingabedaten als `IUnstructuredReader`. Das Ausgabeobjekt dient zum Festlegen von Ausgabedaten als Ergebnis der Extraktoraktivität.

Der Zugriff auf die Eingabedaten erfolgt über `System.IO.Stream` und `System.IO.StreamReader`.

Für die Eingabespaltenenumeration teilen wir zunächst den Eingabedatenstrom mit Zeilentrennzeichen auf.

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

Zum Festlegen der Ausgabedaten verwenden wir die `output.Set`-Methode.

Wichtig: Der benutzerdefinierte Extraktor gibt nur Spalten und Werte aus, die mit der Ausgabe definiert wurden. Wir legen den Methodenaufruf fest.

```c#
    output.Set<string>(count, part);
```

Die tatsächliche Extraktorausgabe wird durch Aufrufen von `yield return output.AsReadOnly();` ausgelöst.

Hier sehen Sie das Extraktorbeispiel:

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

In diesem Szenario generiert der Extraktor die GUID für die Spalte „guid“ neu und konvertiert die Werte der Spalte „user“ in Großbuchstaben. Benutzerdefinierte Extraktoren können durch Analysieren der Eingabedaten und deren Bearbeitung kompliziertere Ergebnisse hervorbringen.

Im Anschluss sehen Sie ein U-SQL-Basisskript mit einem benutzerdefinierten Extraktor:

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

## <a name="use-user-defined-outputters"></a>Verwenden benutzerdefinierter Outputter
Der benutzerdefinierte Outputter ist ein weiteres U-SQL-UDO, das die Erweiterung der integrierten U-SQL-Funktionen ermöglicht. Ähnlich wie beim Extraktor gibt es verschiedene integrierte Outputter.

* *Outputters.Text()*: schreibt Daten in durch Trennzeichen getrennte Textdateien mit verschiedenen Codierungen.
* *Outputters.Csv()*: schreibt Daten in durch Trennzeichen getrennte Textdateien (CSV) mit verschiedenen Codierungen.
* *Outputters.Tsv()*: schreibt Daten in durch Tabstopp getrennte Textdateien (TSV) mit verschiedenen Codierungen.

Benutzerdefinierte Outputter ermöglichen Ihnen, Daten in einem benutzerdefinierten Format zu schreiben. Dies kann bei folgenden Aufgaben hilfreich sein:

* Schreiben von Daten in teilweise strukturierte oder unstrukturierte Dateien
* Schreiben von Daten mit nicht unterstützter Codierung
* Ändern von Ausgabedaten oder Hinzufügen von benutzerdefinierten Attributen

Um einen benutzerdefinierten Outputter zu definieren, müssen wir die `IOutputter`-Schnittstelle erstellen.

Im Anschluss sehen Sie die `IOutputter`-Basisklassenimplementierung:

```c#
    public abstract class IOutputter : IUserDefinedOperator
    {
        protected IOutputter();

        public virtual void Close();
        public abstract void Output(IRow input, IUnstructuredWriter output);
    }
```

Alle Eingabeparameter des Outputters (wie etwa Spalten-/Zeilentrennzeichen und die Codierung) müssen im Konstruktor der Klasse definiert werden. Die `IOutputter`-Schnittstelle muss auch eine Definition für die `void Output`-Überschreibung enthalten. Das `[SqlUserDefinedOutputter(AtomicFileProcessing = true)`-Attribut kann optional für die Verarbeitung atomischer Dateien festgelegt werden. Ausführlichere Informationen finden Sie im weiteren Verlauf.

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

* `Output` wird für jede Eingabezeile aufgerufen. Es gibt das Rowset `IUnstructuredWriter output` zurück.
* Die Konstruktorklasse dient zum Übergeben von Parametern an den benutzerdefinierten Outputter.
* `Close` dient zum optionalen Überschreiben, um den ressourcenintensiven Zustand aufzuheben oder zu ermitteln, wann die letzte Zeile geschrieben wurde.

Das **SqlUserDefinedOutputter**-Attribut gibt an, dass der Typ als benutzerdefinierter Outputter registriert werden muss. Diese Klasse kann nicht vererbt werden.

„SqlUserDefinedOutputter“ ist ein optionales Attribut für die Definition eines benutzerdefinierten Outputters. Das Element wird verwendet, um die AtomicFileProcessing-Eigenschaft zu definieren.

* bool     AtomicFileProcessing   

* **true:** Gibt an, dass dieser Outputter atomische Ausgabedateien (JSON, XML, ...) benötigt.
* **false:** Gibt an, dass dieser Outputter aufgeteilte/verteilte Dateien (CSV, SEQ, ...) behandeln kann.

Die wichtigsten Programmierbarkeitsobjekte sind **row** und **output**. Das Objekt **row** dient zum Aufzählen von Ausgabedaten als `IRow`-Schnittstelle. **Output** wird verwendet, um die Ausgabedaten auf die Zieldatei festzulegen.

Der Zugriff auf die Ausgabedaten erfolgt über die `IRow`-Schnittstelle. Ausgabedaten werden jeweils zeilenweise übergeben.

Die einzelnen Werte werden durch den Aufruf der Get-Methode der IRow-Schnittstelle aufgezählt:

```c#
    row.Get<string>("column_name")
```

Einzelne Spaltennamen können durch Aufrufen von `row.Schema` ermittelt werden:

```c#
    ISchema schema = row.Schema;
    var col = schema[i];
    string val = row.Get<string>(col.Name)
```

Dieser Ansatz ermöglicht die Erstellung eines flexiblen Outputters für alle Metadatenschemas.

Die Ausgabedaten werden mithilfe von `System.IO.StreamWriter` in eine Datei geschrieben. Der Streamparameter wird im Rahmen von `IUnstructuredWriter output` auf `output.BaseStrea` festgelegt.

Wichtig: Der Datenpuffer muss nach jeder Zeileniteration in die Datei geleert werden. Darüber hinaus muss das `StreamWriter`-Objekt mit aktiviertem Disposable-Attribut (Standardeinstellung) und mit dem Schlüsselwort **using** verwendet werden:

```c#
    using (StreamWriter streamWriter = new StreamWriter(output.BaseStream, this._encoding))
            {
        …
            }
```

Rufen Sie andernfalls nach jeder Iteration explizit die Flush()-Methode auf. Dies wird im folgenden Beispiel veranschaulicht.

### <a name="set-headers-and-footers-for-user-defined-outputter"></a>Festlegen von Kopf- und Fußzeilen für benutzerdefinierte Outputter
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

Verwenden Sie für die Fußzeile den Verweis auf die Instanz des `System.IO.Stream`-Objekts (`output.BaseStream`). Schreiben Sie die Fußzeile in die Close()-Methode der `IOutputter`-Schnittstelle.  (Weitere Informationen finden Sie im folgenden Beispiel.)

Das folgende Beispiel zeigt einen benutzerdefinierten Outputter:

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

            // The Close method is used to write the footer to the file. It's executed only once, after all rows
            public override void Close().
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

                // This is a data-independent header--HTML table definition
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

                // Header row output--runs only once
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
                        // Data type enumeration--required to match the distinct list of types from OUTPUT statement
                        switch (col.Type.Name.ToString().ToLower())
                        {
                            case "string": val = row.Get<string>(col.Name).ToString(); break;
                            case "guid": val = row.Get<Guid>(col.Name).ToString(); break;
                            default: break;
                        }
                    }
                    // Handling NULL values--keeping them empty
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

U-SQL-Basisskript:

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

Hierbei handelt es sich um einen HTML-Outputter, der eine HTML-Datei mit Tabellendaten erstellt.

### <a name="call-outputter-from-u-sql-base-script"></a>Aufrufen des Outputters über das U-SQL-Basisskript
Für den Aufruf eines benutzerdefinierten Outputters aus dem U-SQL-Basisskript muss die neue Instanz des Outputterobjekts erstellt werden.

```sql
OUTPUT @rs0 TO @output_file USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

Um die Erstellung einer Instanz des Objekts im Basisskript zu vermeiden, können wir einen Funktionswrapper erstellen, wie in unserem vorherigen Beispiel zu sehen:

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

## <a name="use-user-defined-processors"></a>Verwenden benutzerdefinierter Prozessoren
Der benutzerdefinierte Prozessor (User-Defined Processor, UDP) ist ein U-SQL-UDO, das die Verarbeitung eingehender Zeilen durch Anwenden von Programmierbarkeitsfeatures ermöglicht. Mit dem UDP können Sie bei Bedarf Spalten kombinieren, Werte ändern und neue Spalten hinzufügen. Im Wesentlichen unterstützt er die Verarbeitung eines Rowsets, um erforderliche Datenelemente zu erzeugen.

Um einen UDP zu definieren, müssen wir eine `IProcessor`-Schnittstelle mit dem `SqlUserDefinedProcessor`-Attribut (für UDP optional) erstellen.

Diese Schnittstelle muss die Definition für die Überschreibung des `IRow`-Schnittstellenrowsets enthalten, wie im folgenden Beispiel zu sehen:

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

Die wichtigsten Programmierbarkeitsobjekte sind **input** und **output**. Das Eingabeobjekt dient zum Aufzählen von Eingabespalten. Das Ausgabeobjekt dient zum Festlegen der Ausgabedaten als Ergebnis der Prozessoraktivität.

Zur Eingabespaltenenumeration verwenden wir die `input.Get`-Methode.

```c#
    string column_name = input.Get<string>("column_name");
```

Der Parameter für die `input.Get`-Methode ist eine Spalte, die als Teil der `PRODUCE`-Klausel der `PROCESS`-Anweisung des U-SQL-Basisskripts übergeben wird. Wir müssen hier den richtigen Datentyp verwenden.

Verwenden Sie für die Ausgabe die `output.Set`-Methode.

Wichtig: Der benutzerdefinierte Producer gibt nur Spalten und Werte aus, die mit dem `output.Set`-Methodenaufruf definiert wurden.

```c#
    output.Set<string>("mycolumn", mycolumn);
```

Die tatsächliche Prozessorausgabe wird durch Aufrufen von `return output.AsReadOnly();` ausgelöst.

Im Anschluss finden Sie ein Prozessorbeispiel:

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

In diesem Szenario erzeugt der Prozessor eine neue Spalte namens „full_description“, indem er die vorhandenen Spalten kombiniert (in diesem Fall: „user“ in Großbuchstaben und „des“). Darüber hinaus generiert er eine neue GUID und gibt den ursprünglichen und neuen GUID-Wert zurück.

Wie im vorherigen Beispiel zu sehen, können Sie C#-Methoden im Rahmen eines `output.Set`-Methodenaufrufs aufrufen.

Bei dem folgenden Beispiel handelt es sich um ein U-SQL-Basisskript mit einem benutzerdefinierten Prozessor:

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

## <a name="use-user-defined-appliers"></a>Verwenden benutzerdefinierter Applier
Mit einem benutzerdefinierten U-SQL-Applier können Sie für jede Zeile, die vom äußeren Tabellenausdruck einer Abfrage zurückgegeben wird, eine benutzerdefinierte C#-Funktion aufrufen. Die rechte Eingabe wird für jede Zeile aus der linken Eingabe ausgewertet, und die erstellten Zeilen werden für die endgültige Ausgabe kombiniert. Bei der Liste mit den Spalten, die durch den APPLY-Operator erzeugt werden, handelt es sich um die Kombination aus dem Satz von Spalten in der linken und rechten Eingabe.

Der benutzerdefinierte Applier wird im Rahmen des U-SQL SELECT-Ausdrucks aufgerufen.

Ein typischer Aufruf des benutzerdefinierten Appliers sieht wie folgt aus:

```sql
    SELECT …
    FROM …
    CROSS APPLYis used to pass parameters
    new MyScript.MyApplier(param1, param2) AS alias(output_param1 string, …);
```

Weitere Informationen zur Verwendung von Appliers in einem SELECT-Ausdruck finden Sie unter [U-SQL SELECT Selecting from CROSS APPLY and OUTER APPLY](https://msdn.microsoft.com/library/azure/mt621307.aspx) (U-SQL SELECT: Auswählen aus CROSS APPLY und OUTER APPLY).

Die Basisklassendefinition eines benutzerdefinierten Appliers sieht wie folgt aus:

```c#
    public abstract class IApplier : IUserDefinedOperator
    {
        protected IApplier();

        public abstract IEnumerable<IRow> Apply(IRow input, IUpdatableRow output);
    }
```

Um einen benutzerdefinierten Applier zu definieren, müssen wir die `IApplier`-Schnittstelle mit dem [`SqlUserDefinedApplier`]-Attribut (für die Definition eines benutzerdefinierten Appliers optional) erstellen.

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

* „Apply“ wird für jede Zeile der äußeren Tabelle aufgerufen. Es gibt das `IUpdatableRow`-Ausgaberowset zurück.
* Die Konstruktorklasse dient zum Übergeben von Parametern an den benutzerdefinierten Applier.

**SqlUserDefinedApplier** gibt an, dass der Typ als benutzerdefinierter Applier registriert werden muss. Diese Klasse kann nicht vererbt werden.

**SqlUserDefinedApplier** ist für die Definition benutzerdefinierter Applier **optional**.


Die wichtigsten Programmierbarkeitsobjekte sind:

```c#
        public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
```

Eingaberowsets werden als `IRow`-Eingabe übergeben. Die Ausgabezeilen werden als `IUpdatableRow`-Ausgabeschnittstelle generiert.

Einzelne Spaltennamen können durch Aufrufen der `IRow`-Schemamethode ermittelt werden.

```c#
    ISchema schema = row.Schema;
    var col = schema[i];
    string val = row.Get<string>(col.Name)
```

Um die tatsächlichen Datenwerte aus dem eingehenden `IRow`-Element abzurufen, verwenden wir die Get()-Methode der `IRow`-Schnittstelle.

```c#
    mycolumn = row.Get<int>("mycolumn")
```

Alternativ können wir auch den Schemaspaltennamen verwenden:

```c#
    row.Get<int>(row.Schema[0].Name)
```

Die Ausgabewerte müssen mit der `IUpdatableRow`-Ausgabe festgelegt werden:

```c#
    output.Set<int>("mycolumn", mycolumn)
```

Wichtig: Benutzerdefinierte Applier geben nur Spalten und Werte aus, die mit dem `output.Set`-Methodenaufruf definiert wurden.

Die tatsächliche Ausgabe wird durch Aufrufen von `yield return output.AsReadOnly();` ausgelöst.

Die Parameter des benutzerdefinierten Appliers können an den Konstruktor übergeben werden. Der Applier kann eine variable Anzahl von Spalten zurückgeben, die während des Applieraufrufs im U-SQL-Basisskript definiert werden müssen.

```c#
  new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);
```

Hier sehen Sie das Beispiel für den benutzerdefinierten Applier:

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

Im Anschluss finden Sie das U-SQL-Basisskript für diesen benutzerdefinierten Applier:

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

In diesem Szenario fungiert der benutzerdefinierte Applier als Parser mit Werttrennung durch Trennzeichen für die Eigenschaften der Autoflotte. Die Zeilen der Eingabedatei sehen wie folgt aus:

```
103    Z1AB2CD123XY45889    Ford,Explorer,2005,SUV,152345
303    Y0AB2CD34XY458890    Shevrolet,Cruise,2010,4Dr,32455
210    X5AB2CD45XY458893    Nissan,Altima,2011,4Dr,74000
```

Es handelt sich um eine typische TSV-Datei mit Tabstopptrennzeichen und einer Eigenschaftenspalte, die Fahrzeugeigenschaften wie Marke und Modell enthält. Diese Eigenschaften müssen für die Tabellenspalten analysiert werden. Der bereitgestellte Applier ermöglicht auch die Generierung einer dynamischen Anzahl von Eigenschaften im Ergebnisrowset auf der Grundlage des übergebenen Parameters. Sie können entweder alle Eigenschaften generieren oder nur einen bestimmten Satz von Eigenschaften.

    …USQL_Programmability.ParserApplier ("all")
    …USQL_Programmability.ParserApplier ("make")
    …USQL_Programmability.ParserApplier ("make&model")

Der benutzerdefinierte Applier kann als neue Instanz des Applierobjekts aufgerufen werden:

```c#
    CROSS APPLY new MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

Eine weitere Möglichkeit ist das Aufrufen einer Wrapper-Factorymethode:

```c#
    CROSS APPLY MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

## <a name="use-user-defined-combiners"></a>Verwenden benutzerdefinierter Combiner
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

Um einen benutzerdefinierten Combiner zu definieren, müssen wir die `ICombiner`-Schnittstelle mit dem [`SqlUserDefinedCombiner`]-Attribut (für die Definition eines benutzerdefinierten Combiners optional) erstellen.

`ICombiner`-Basisklassendefinition:

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

Die benutzerdefinierte Implementierung einer `ICombiner`-Schnittstelle muss die Definition für eine Combine-Überschreibung für `IEnumerable<IRow>` enthalten.

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

Das Attribut **SqlUserDefinedCombiner** gibt an, dass der Typ als benutzerdefinierter Combiner registriert werden muss. Diese Klasse kann nicht vererbt werden.

**SqlUserDefinedCombiner** dient zum Definieren der Combiner-Moduseigenschaft. Das Attribut ist für die Definition eines benutzerdefinierten Combiners optional.

CombinerMode-Modus

Die CombinerMode-Aufzählung akzeptiert die folgenden Werte:

* „Full“ (0): Jede Ausgabezeile ist potenziell von allen Eingabezeilen von links und rechts mit dem gleichen Schlüsselwert abhängig.

* „Left“ (1): Jede Ausgabezeile ist von einer einzelnen Eingabezeile von links (und potenziell von allen Zeilen von rechts mit dem gleichen Schlüsselwert) abhängig.

* „Right“ (2): Jede Ausgabezeile ist von einer einzelnen Eingabezeile von rechts (und potenziell von allen Zeilen von links mit dem gleichen Schlüsselwert) abhängig.

* „Inner“ (3): Jede Ausgabezeile ist von einer einzelnen Eingabezeile von links und rechts mit dem gleichen Wert abhängig.

Beispiel:    [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]


Die wichtigsten Programmierbarkeitsobjekte sind:

```c#
    public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output
```

Eingaberowsets werden als **left**- und **right**-`IRowset`-Schnittstellentyp übergeben. Beide Rowsets müssen für die Verarbeitung aufgezählt werden. Die einzelnen Schnittstellen dürfen jeweils nur einmal aufgezählt werden, sodass die Aufzählung ggf. zwischengespeichert werden muss.

Für die Zwischenspeicherung können wir einen List\<T\>-Typ der Arbeitsspeicherstruktur als Ergebnis einer LINQ-Abfragenausführung (genauer gesagt: List<`IRow`>) erstellen. Der anonyme Datentyp kann während der Aufzählung ebenfalls verwendet werden.

Unter [Introduction to LINQ Queries (C#)](https://msdn.microsoft.com/library/bb397906.aspx) (Einführung in LINQ-Abfragen (C#)) finden Sie weitere Informationen zu LINQ-Abfragen. Unter [IEnumerable\<T\> Interface](https://msdn.microsoft.com/library/9eekhta0(v=vs.110).aspx) (IEnumerable<T>-Schnittstelle) finden Sie weitere Informationen zur IEnumerable\<T\>-Schnittstelle.

Um die tatsächlichen Datenwerte aus dem eingehenden `IRowset`-Element abzurufen, verwenden wir die Get()-Methode der `IRow`-Schnittstelle.

```c#
    mycolumn = row.Get<int>("mycolumn")
```

Einzelne Spaltennamen können durch Aufrufen der `IRow`-Schemamethode ermittelt werden.

```c#
    ISchema schema = row.Schema;
    var col = schema[i];
    string val = row.Get<string>(col.Name)
```

Alternativ können wir auch den Schemaspaltennamen verwenden:

```
    c# row.Get<int>(row.Schema[0].Name)
```

Die allgemeine Enumeration mit LINQ sieht wie folgt aus:

```c#
var myRowset =
            (from row in left.Rows
                          select new
                          {
                              Mycolumn = row.Get<int>("mycolumn"),
                          }).ToList();
```

Nach dem Aufzählen beider Rowsets durchlaufen wir alle Zeilen mithilfe einer Schleife. Für jede Zeile im linken Rowset suchen wir nach allen Zeilen, die die Bedingung unseres Combiners erfüllen.

Die Ausgabewerte müssen mit der `IUpdatableRow`-Ausgabe festgelegt werden.

```c#
    output.Set<int>("mycolumn", mycolumn)
```

Die tatsächliche Ausgabe wird durch Aufrufen von `yield return output.AsReadOnly();` ausgelöst.

Im Anschluss finden Sie ein Beispiel für den Combiner:

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

In diesem Szenario erstellen wir einen Analysebericht für den Einzelhandel. Es sollen alle Produkte gefunden werden, deren Preis über 20.000 USD liegt und die innerhalb eines bestimmten Zeitraums schneller über die Website verkauft werden als über den regulären Einzelhandel.

So sieht das U-SQL-Basisskript aus. Sie können die Logik zwischen einem regulären JOIN-Element und einem Combiner vergleichen:

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

Ein benutzerdefinierter Combiner kann als neue Instanz des Applierobjekts aufgerufen werden:

```c#
    USING new MyNameSpace.MyCombiner();
```


Eine weitere Möglichkeit ist das Aufrufen einer Wrapper-Factorymethode:

```c#
    USING MyNameSpace.MyCombiner();
```

## <a name="use-user-defined-reducers"></a>Verwenden benutzerdefinierter Reducer
Mit U-SQL können Sie benutzerdefinierte Rowsetreducer in C# schreiben. Hierzu müssen Sie das Erweiterbarkeitsframework des benutzerdefinierten Operators verwenden und eine IReducer-Schnittstelle implementieren.

Mit einem benutzerdefinierten Reducer (User-Defined Reducer, UDR) können Sie überflüssige Zeilen während der Datenextraktion (Import) entfernen. Darüber hinaus kann er zur Bearbeitung und Auswertung von Zeilen und Spalten verwendet werden. Auf der Grundlage der Programmierbarkeitslogik kann er zudem die zu extrahierenden Zeilen definieren.

Zum Definieren einer UDR-Klasse müssen wir eine `IReducer`-Schnittstelle mit optionalem `SqlUserDefinedReducer`-Attribut erstellen.

Diese Klassenschnittstelle muss eine Definition für die Überschreibung des `IEnumerable`-Schnittstellenrowsets enthalten.

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

Das Attribut **SqlUserDefinedReducer** gibt an, dass der Typ als benutzerdefinierter Reducer registriert werden muss. Diese Klasse kann nicht vererbt werden.
**SqlUserDefinedReducer** ist ein optionales Attribut für die Definition eines benutzerdefinierten Reducers. Es wird verwendet, um die IsRecursive-Eigenschaft zu definieren.

* bool     IsRecursive    
* **true**  = Gibt an, ob dieser Reducer idempotent ist.

Die wichtigsten Programmierbarkeitsobjekte sind **input** und **output**. Das Eingabeobjekt dient zum Aufzählen von Eingabezeilen. Die Ausgabe dient zum Festlegen von Ausgabezeilen als Ergebnis der Reduzierungsaktivität.

Zum Aufzählen der Eingabezeilen verwenden wir die `Row.Get`-Methode.

```c#
            foreach (IRow row in input.Rows)
            {
                        row.Get<string>("mycolumn");
            }
```

Der Parameter für die `Row.Get`-Methode ist eine Spalte, die als Teil der `PRODUCE`-Klasse der `REDUCE`-Anweisung des U-SQL-Basisskripts übergeben wird. Wir müssen hier ebenfalls den richtigen Datentyp verwenden.

Verwenden Sie für die Ausgabe die `output.Set`-Methode.

Wichtig: Benutzerdefinierte Reducer geben nur Werte aus, die mit dem `output.Set`-Methodenaufruf definiert wurden.

```c#
    output.Set<string>("mycolumn", guid);
```

Die tatsächliche Reducerausgabe wird durch Aufrufen von `yield return output.AsReadOnly();` ausgelöst.

Im Anschluss finden Sie ein Beispiel für den Reducer:

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

In diesem Szenario überspringt der Reducer Zeilen mit leerem Benutzernamen. Er liest für jede Zeile im Rowset die einzelnen erforderlichen Spalten und wertet dann die Länge des Benutzernamens aus. Die eigentliche Zeile wird nur ausgegeben, wenn der Längenwert des Benutzernamens größer 0 ist.

Im Anschluss sehen Sie ein U-SQL-Basisskript mit einem benutzerdefinierten Reducer:

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

