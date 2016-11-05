## <a name="specifying-formats"></a>Angeben von Formaten
### <a name="specifying-textformat"></a>Angeben von "TextFormat"
Wenn das Format auf **TextFormat** festgelegt ist, können Sie die folgenden **optionalen** Eigenschaften im Abschnitt **Format** angeben:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| columnDelimiter |Das Zeichen, das in einer Datei zum Trennen von Spalten verwendet wird. |Es ist nur ein Zeichen zulässig. Der Standardwert ist das Komma (,). |Nein |
| rowDelimiter |Das Zeichen, das in einer Datei zum Trennen von Zeilen verwendet wird. |Es ist nur ein Zeichen zulässig. Der Standardwert ist einer der Folgenden: ["\r\n", "\r", "\n"] beim Lesen und "\r\n" beim Schreiben. |Nein |
| escapeChar |Das Sonderzeichen, das als Escapezeichen für das Spaltentrennzeichen im Inhalt der Eingabedatei dient. <br/><br/>Sie können für eine Tabelle nicht gleichzeitig escapeChar und quoteChar verwenden. |Es ist nur ein Zeichen zulässig. Kein Standardwert. <br/><br/>Beispiel: Wenn Sie das Komma (,) als Spaltentrennzeichen gewählt haben, das Kommazeichen jedoch im Text (Beispiel: „Hello, world“) verwenden möchten, können Sie das Dollarzeichen ($) als Escapezeichen definieren und die Zeichenfolge „Hello$, world“ in der Quelle verwenden. |Nein |
| quoteChar |Das Zeichen, das verwendet wird, um einen Zeichenfolgenwert zu zitieren. Die Spalten- und Zeilentrennzeichen innerhalb der Anführungszeichen werden als Teil des Zeichenfolgenwerts behandelt. Diese Eigenschaft gilt für Eingabe- und Ausgabedatasets.<br/><br/>Sie können für eine Tabelle nicht gleichzeitig escapeChar und quoteChar verwenden. |Es ist nur ein Zeichen zulässig. Kein Standardwert. <br/><br/>Beispiel: Wenn Sie das Komma (,) als Spaltentrennzeichen gewählt haben, das Kommazeichen jedoch im Text (Beispiel: <Hello, world>) verwenden möchten, können Sie das doppelte gerade Anführungszeichen (") als Escapezeichen definieren und die Zeichenfolge "Hello, world" in der Quelle verwenden. |Nein |
| nullValue |Ein oder mehrere Zeichen, das/die verwendet wird/werden, um einen Null-Wert darzustellen. |Ein oder mehrere Zeichen. Die Standardwerte lauten „\N“ und „NULL“ beim Lesen und „\N“ beim Schreiben. |Nein |
| encodingName |Geben Sie den Codierungsnamen an. |Ein gültiger Codierungsname. Siehe [Encoding.EncodingName-Eigenschaft](https://msdn.microsoft.com/library/system.text.encoding.aspx). Beispiel: windows-1250 oder shift_jis. Der Standardwert lautet „UTF-8“. |Nein |
| firstRowAsHeader |Gibt an, ob die erste Zeile als Kopfzeile betrachtet werden soll. Bei einem Eingabedataset liest Data Factory die erste Zeile als Kopfzeile. Bei einem Ausgabedataset schreibt Data Factory die erste Zeile als Kopfzeile. <br/><br/>Beispielszenarien finden Sie unter [Szenarien für die Verwendung von **firstRowAsHeader** und **skipLineCount**](#scenarios-for-using-firstrowasheader-and-skiplinecount). |True<br/>False (Standardwert) |Nein |
| skipLineCount |Gibt an, wie viele Zeilen beim Lesen von Daten aus Eingabedateien übersprungen werden sollen. Wenn sowohl skipLineCount und firstRowAsHeader angegeben sind, werden erst die Zeilen übersprungen und dann die Kopfzeileninformationen aus der Eingabedatei gelesen. <br/><br/>Beispielszenarien finden Sie unter [Szenarien für die Verwendung von firstRowAsHeader und skipLineCount](#scenarios-for-using-firstrowasheader-and-skiplinecount) . |Integer |Nein |
| treatEmptyAsNull |Gibt an, ob eine NULL- oder eine leere Zeichenfolge beim Lesen von Daten aus einer Eingabedatei als NULL-Wert behandelt werden sollen. |True (Standardwert)<br/>False |Nein |

#### <a name="textformat-example"></a>TextFormat-Beispiel
Im folgenden Beispiel werden einige der Formateigenschaften für "TextFormat" gezeigt.

    "typeProperties":
    {
        "folderPath": "mycontainer/myfolder",
        "fileName": "myblobname"
        "format":
        {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";",
            "quoteChar": "\"",
            "NullValue": "NaN"
            "firstRowAsHeader": true,
            "skipLineCount": 0,
            "treatEmptyAsNull": true
        }
    },

Um „escapeChar“ anstelle von „quoteChar“ zu verwenden, ersetzen Sie die Zeile mit „quoteChar“ durch folgende escapeChar-Angabe:

    "escapeChar": "$",



### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>Szenarien für die Verwendung von firstRowAsHeader und skipLineCount
* Sie kopieren Daten aus einer Quelle, bei der es sich nicht um eine Datei handelt, in eine Textdatei und möchten eine Kopfzeile mit den Schemametadaten (z.B. ein SQL-Schema) hinzufügen. Legen Sie für dieses Szenario **firstRowAsHeader** im Ausgabedataset als „true“ fest. 
* Sie kopieren aus einer Textdatei mit einer Kopfzeile in eine Senke, bei der es sich nicht um eine Datei handelt, und möchten diese Zeile löschen. Legen Sie **firstRowAsHeader** im Eingabedatenset als „true“ fest.
* Sie kopieren aus einer Textdatei und möchten einige Zeilen am Anfang überspringen, die keine Daten oder Kopfzeileninformationen enthalten. Geben Sie **skipLineCount** mit der Anzahl von Zeilen an, die übersprungen werden sollen. Wenn der Rest der Datei eine Kopfzeile enthält, können Sie auch **firstRowAsHeader**angeben. Wenn sowohl **skipLineCount** und **firstRowAsHeader** angegeben sind, werden erst die Zeilen übersprungen und dann die Kopfzeileninformationen aus der Eingabedatei gelesen.

### <a name="specifying-avroformat"></a>Angeben von "AvroFormat"
Wenn das Format auf "AvroFormat" festgelegt ist, müssen Sie im Abschnitt "Format" innerhalb des Abschnitts "typeProperties" keine Eigenschaften angeben. Beispiel:

    "format":
    {
        "type": "AvroFormat",
    }

Um das Avro-Format in einer Hive-Tabelle zu verwenden, sehen Sie sich zuvor das [Apache Hive-Tutorial](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe)an.

### <a name="specifying-jsonformat"></a>Angeben von „JsonFormat“
Wenn das Format auf **JsonFormat** festgelegt ist, können Sie die folgenden **optionalen** Eigenschaften im Abschnitt **Format** angeben.

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| filePattern |Geben Sie das Muster der in jeder JSON-Datei gespeicherten Daten an. Zulässige Werte sind **setOfObjects** und **arrayOfObjects**. Der **Standardwert** ist **setOfObjects**. Weitere Informationen zu diesen Mustern finden Sie in den folgenden Abschnitten. |Nein |
| encodingName |Geben Sie den Codierungsnamen an. Eine Liste der gültigen Codierungsnamen finden Sie unter: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) -Eigenschaft. Beispiel: Windows-1250 oder Shift-JIS. Der **Standardwert** lautet **UTF-8**. |Nein |
| nestingSeparator |Zeichen, das zur Trennung der Schachtelungsebenen verwendet wird. Der Standardwert ist „.“ (Punkt). |Nein |

#### <a name="setofobjects-file-pattern"></a>Dateimuster setOfObjects
Jede Datei enthält ein einzelnes Objekt oder mehrere durch Zeilen getrennte/verkettete Objekte. Wenn diese Option in einem Ausgabedataset ausgewählt wird, erzeugt die Kopieraktivität eine einzelne JSON-Datei mit jedem Objekt pro Zeile (durch Zeilen getrennt).

**einzelnes Objekt** 

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }

**durch Zeilen getrennte JSON** 

    {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
    {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
    {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"789037573","callingnum2":"789044691","switch1":"UK","switch2":"Australia"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789044691","switch1":"US","switch2":"Australia"}

**verkettete JSON**

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }
    {
        "time": "2015-04-29T07:13:21.0220000Z",
        "callingimsi": "466922202613463",
        "callingnum1": "123436380",
        "callingnum2": "789037573",
        "switch1": "US",
        "switch2": "UK"
    }
    {
        "time": "2015-04-29T07:13:21.4370000Z",
        "callingimsi": "466923101048691",
        "callingnum1": "678901578",
        "callingnum2": "345626404",
        "switch1": "Germany",
        "switch2": "UK"
    }


#### <a name="arrayofobjects-file-pattern."></a>Dateimuster arrayOfObjects
Jede Datei enthält ein Array von Objekten. 

    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "789037573",
            "callingnum2": "789044691",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789044691",
            "switch1": "US",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:24.2120000Z",
            "callingimsi": "466922201102759",
            "callingnum1": "345698602",
            "callingnum2": "789097900",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:25.6320000Z",
            "callingimsi": "466923300236137",
            "callingnum1": "567850552",
            "callingnum2": "789086133",
            "switch1": "China",
            "switch2": "Germany"
        }
    ]

### <a name="jsonformat-example"></a>JsonFormat-Beispiel
Wenn Sie eine JSON-Datei mit dem folgenden Inhalt haben:  

    {
        "Id": 1,
        "Name": {
            "First": "John",
            "Last": "Doe"
        },
        "Tags": ["Data Factory”, "Azure"]
    }

und Sie möchten sie im folgenden Format in eine Azure SQL-Tabelle kopieren: 

| ID | Name.First | Name.Middle | Name.Last | Tags |
| --- | --- | --- | --- | --- |
| 1 |John |null |Doe |[„Data Factory”, „Azure“] |

Das Eingabedataset vom Typ JsonFormat ist wie folgt definiert: (Teildefinition ausschließlich mit den relevanten Teilen)

    "properties": {
        "structure": [
            {"name": "Id", "type": "Int"},
            {"name": "Name.First", "type": "String"},
            {"name": "Name.Middle", "type": "String"},
            {"name": "Name.Last", "type": "String"},
            {"name": "Tags", "type": "string"}
        ],
        "typeProperties":
        {
            "folderPath": "mycontainer/myfolder",
            "format":
            {
                "type": "JsonFormat",
                "filePattern": "setOfObjects",
                "encodingName": "UTF-8",
                "nestingSeparator": "."
            }
        }
    }

Wenn die Struktur nicht definiert ist, vereinfacht die Kopieraktivität die Struktur standardmäßig und kopiert alles. 

#### <a name="supported-json-structure"></a>Unterstützte JSON-Struktur
Beachten Sie folgende Punkte: 

* Jedes Objekt mit einer Sammlung von Name-Wert-Paaren wird einer Zeile mit Daten in einem Tabellenformat zugeordnet. Objekte können geschachtelt werden, und Sie können definieren, wie die Struktur in einem Dataset mit dem Schachtelungstrennzeichen (.) standardmäßig vereinfacht werden soll. Ein Beispiel finden Sie im vorherigen Abschnitt [JsonFormat-Beispiel](#jsonformat-example).  
* Wenn die Struktur im Data Factory-Dataset definiert ist, erkennt die Kopieraktivität das Schema des ersten Objekts und vereinfacht das gesamte Objekt. 
* Wenn die JSON-Eingabe ein Array hat, konvertiert die Kopieraktivität den gesamten Array-Wert in eine Zeichenfolge. Sie können dies überspringen, indem Sie [Spaltenzuordnung oder -filterung](#column-mapping-with-translator-rules)verwenden.
* Wenn auf derselben Ebene doppelte Namen vorkommen, wählt die Kopieraktivität den letzten aus.
* Bei Eigenschaftennamen wird zwischen Groß- und Kleinschreibung unterschieden. Zwei Eigenschaften mit demselben Namen und unterschiedlicher Groß- und Kleinschreibung werden als zwei getrennte Eigenschaften behandelt. 

### <a name="specifying-orcformat"></a>Angeben von „OrcFormat“
Wenn das Format auf „OrcFormat“ festgelegt ist, müssen Sie im Abschnitt „Format“ innerhalb des Abschnitts „typeProperties“ keine Eigenschaften angeben. Beispiel:

    "format":
    {
        "type": "OrcFormat"
    }

> [!IMPORTANT]
> Wenn Sie ORC-Dateien nicht **unverändert** zwischen lokalen und Clouddatenspeichern kopieren, müssen Sie die JRE 8 (Java Runtime Environment) auf Ihrem Gatewaycomputer installieren. Für ein 64-Bit-Gateway ist die 64-Bit-JRE erforderlich, für ein 32-Bit-Gateway die 32-Bit-JRE. Beide Versionen finden Sie [hier](http://go.microsoft.com/fwlink/?LinkId=808605). Wählen Sie die geeignete Version aus.
> 
> 

Beachten Sie folgende Punkte:

* Komplexe Datentypen werden nicht unterstützt (STRUCT, MAP, LIST, UNION).
* Für die ORC-Datei stehen drei [mit der Komprimierung zusammenhängende Optionen](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/)zur Verfügung: NONE, ZLIB, SNAPPY. Data Factory unterstützt das Lesen von Daten aus ORC-Dateien in jedem der oben genannten komprimierten Formate. Zum Lesen der Daten wird der Komprimierungscodec in den Metadaten verwendet. Beim Schreiben in eine ORC-Datei wählt Data Factory hingegen ZLIB (Standardeinstellung für ORC). Derzeit gibt es keine Option zum Überschreiben dieses Verhaltens. 

### <a name="specifying-parquetformat"></a>Angeben von „ParquetFormat“
Wenn das Format auf „ParquetFormat“ festgelegt ist, müssen Sie im Abschnitt „Format“ innerhalb des Abschnitts „typeProperties“ keine Eigenschaften angeben. Beispiel:

    "format":
    {
        "type": "ParquetFormat"
    }

> [!IMPORTANT]
> Wenn Sie Parquet-Dateien nicht **unverändert** zwischen lokalen und Clouddatenspeichern kopieren, müssen Sie die JRE 8 (Java Runtime Environment) auf Ihrem Gatewaycomputer installieren. Für ein 64-Bit-Gateway ist die 64-Bit-JRE erforderlich, für ein 32-Bit-Gateway die 32-Bit-JRE. Beide Versionen finden Sie [hier](http://go.microsoft.com/fwlink/?LinkId=808605). Wählen Sie die geeignete Version aus.
> 
> 

Beachten Sie folgende Punkte:

* Komplexe Datentypen werden nicht unterstützt (MAP, LIST).
* Für die Parquet-Datei stehen die folgenden mit der Komprimierung zusammenhängenden Optionen zur Verfügung: NONE, SNAPPY, GZIP und LZO. Data Factory unterstützt das Lesen von Daten aus ORC-Dateien in jedem der oben genannten komprimierten Formate. Zum Lesen der Daten wird der Komprimierungscodec in den Metadaten verwendet. Beim Schreiben in eine Parquet-Datei wählt Data Factory hingegen SNAPPY (Standardeinstellung für das Parquet-Format). Derzeit gibt es keine Option zum Überschreiben dieses Verhaltens. 

<!--HONumber=Oct16_HO2-->


