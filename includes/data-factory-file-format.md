### Angeben von "TextFormat"

Wenn das Format auf **TextFormat** festgelegt ist, können Sie die folgenden **optionalen** Eigenschaften im Abschnitt **Format** angeben.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| columnDelimiter | Das Zeichen, das als Spaltentrennzeichen in einer Datei verwendet wird. Derzeit ist nur ein Zeichen zulässig. Dieses Tag ist optional. Der Standardwert ist das Komma (,). | Nein |
| rowDelimiter | Das Zeichen, das als Zeilentrennzeichen in einer Datei verwendet wird. Derzeit ist nur ein Zeichen zulässig. Dieses Tag ist optional. Der Standardwert ist einer der Folgenden: ["\\r\\n", "\\r", "\\n"]. | Nein |
| escapeChar | Das Sonderzeichen, das als Escapezeichen für das Spaltentrennzeichen im Inhalt dient. Dieses Tag ist optional. Kein Standardwert. Sie dürfen maximal ein Zeichen für diese Eigenschaft angeben.<br/><br/>Beispiel: Wenn Sie das Komma (,) als Spaltentrennzeichen gewählt haben, es jedoch im Text (Beispiel: „Hello, world“) verwenden möchten, können Sie „$“ als Escapezeichen definieren und die Zeichenfolge „Hello$, world“ in der Quelle verwenden.<br/><br/>Beachten Sie, dass Sie escapeChar und quoteChar nicht gleichzeitig für eine Tabelle angeben können. | Nein | 
| quoteChar | Das Sonderzeichen, das als Anführungszeichen für einen Zeichenfolgenwert dient. Die Spalten- und Zeilentrennzeichen innerhalb der Anführungszeichen werden als Teil des Zeichenfolgenwerts behandelt. Dieses Tag ist optional. Kein Standardwert. Sie dürfen nicht mehr als ein Zeichen für diese Eigenschaft angeben.<br/><br/>Beispiel: Wenn Sie das Komma (,) als Spaltentrennzeichen gewählt haben, das Kommazeichen jedoch im Text (Beispiel: <Hello  world>) verwenden möchten, können Sie '"' als Anführungszeichen definieren und die Zeichenfolge <"Hello, world"> in der Quelle verwenden. Diese Eigenschaft gilt für Eingabe- und Ausgabetabellen.<br/><br/>Beachten Sie, dass Sie escapeChar und quoteChar nicht gleichzeitig für eine Tabelle angeben können. | Nein |
| nullValue | Die Zeichen, die zur Darstellung von NULL-Werten im Blobdateiinhalt dienen. Dieses Tag ist optional. Der Standardwert ist „\\N“.<br/><br/>Beispielsweise wird gemäß dem oben genannten Beispiel „NaN“ im Blob beim Kopieren (z.B. in SQL Server) als NULL-Wert übersetzt. | Nein |
| encodingName | Geben Sie den Codierungsnamen an. Eine Liste der gültigen Codierungsnamen finden Sie unter [Encoding.EncodingName-Eigenschaft](https://msdn.microsoft.com/library/system.text.encoding.aspx). Beispiel: Windows-1250 oder Shift-JIS. Der Standardwert lautet "UTF-8". | Nein | 

#### TextFormat-Beispiel
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
	        "quoteChar": """,
	        "NullValue": "NaN"
	    }
	},

Um "escapeChar" anstelle von "quoteChar" zu verwenden, ersetzen Sie die Zeile mit "quoteChar" durch Folgendes:

	"escapeChar": "$",

### Angeben von "AvroFormat"
Wenn das Format auf "AvroFormat" festgelegt ist, müssen Sie im Abschnitt "Format" innerhalb des Abschnitts "typeProperties" keine Eigenschaften angeben. Beispiel:

	"format":
	{
	    "type": "AvroFormat",
	}

Um das Avro-Format in einer Hive-Tabelle zu verwenden, sehen Sie sich zuvor das [Apache Hive-Tutorial](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe) an.

### Angeben von „JsonFormat“

Wenn das Format auf **JsonFormat** festgelegt ist, können Sie die folgenden **optionalen** Eigenschaften im Abschnitt **Format** angeben.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| filePattern | Geben Sie das Muster der in jeder JSON-Datei gespeicherten Daten an. Zulässige Werte sind: **setOfObjects** und **arrayOfObjects**. Der **Standardwert** ist **setOfObjects**. Weitere Informationen zu diesen Mustern finden Sie in den nachstehenden Abschnitten.| Nein |
| encodingName | Geben Sie den Codierungsnamen an. Eine Liste der gültigen Codierungsnamen finden Sie unter [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx)-Eigenschaft. Beispiel: Windows-1250 oder Shift-JIS. Der **Standardwert** ist **UTF-8**. | Nein | 
| nestingSeparator | Zeichen, das zur Trennung der Schachtelungsebenen verwendet wird. Der **Standardwert** ist **. (Punkt)**. | Nein | 


#### Dateimuster setOfObjects

Jede Datei enthält ein einzelnes Objekt oder mehrere durch Zeilen getrennte/verkettete Objekte. Wenn diese Option in einem Ausgabedataset ausgewählt wird, erzeugt die Kopie eine einzelne JSON-Datei mit jedem Objekt pro Zeile (durch Zeilen getrennt).

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


#### Dateimuster arrayOfObjects 

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

### JsonFormat-Beispiel

Wenn Sie eine JSON-Datei mit dem folgenden Inhalt haben:

	{
		"Id": 1,
		"Name": {
			"First": "John",
			"Last": "Doe"
		},
		"Tags": ["Data Factory”, "Azure"]
	}

und Sie wollen sie im folgenden Format in eine Azure SQL-Tabelle kopieren:

ID | Name.First | Name.Middle | Name.Last | Tags
--- | ---------- | ----------- | --------- | ----
1 | John | null | Doe | [„Data Factory”, „Azure“]

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

#### Unterstützte JSON-Struktur
Beachten Sie Folgendes:

- Jedes Objekt mit einer Sammlung von Name/Wert-Paaren wird einer Zeile mit Daten in einem Tabellenformat zugeordnet. Objekte können geschachtelt werden, und Sie können definieren, wie die Struktur in einem Dataset mit dem Schachtelungstrennzeichen (.) standardmäßig vereinfacht werden soll. Ein Beispiel finden Sie im Abschnitt [JsonFormat-Beispiel](#jsonformat-example) weiter oben.  
- Wenn die Struktur im Data Factory-Dataset definiert ist, erkennt die Kopieraktivität das Schema des ersten Objekts und vereinfacht das gesamte Objekt. 
- Wenn die JSON-Eingabe ein Array hat, konvertiert die Kopieraktivität den gesamten Array-Wert in eine Zeichenfolge. Sie können dies überspringen, indem Sie [Spaltenzuordnung oder -filterung](#column-mapping-with-translator-rules) verwenden.
- Wenn auf derselben Ebene doppelte Namen vorkommen, wählt die Kopieraktivität den letzten aus.
- Bei Eigenschaftennamen wird zwischen Groß- und Kleinschreibung unterschieden. Zwei Eigenschaften mit demselben Namen, aber unterschiedlicher Schreibweise, werden als zwei getrennte Eigenschaften behandelt. 

### Angeben von „OrcFormat“
Wenn das Format auf „OrcFormat“ festgelegt ist, müssen Sie im Abschnitt „Format“ innerhalb des Abschnitts „typeProperties“ keine Eigenschaften angeben. Beispiel:

	"format":
	{
	    "type": "OrcFormat",
	}

Beachten Sie Folgendes:
 
-	Wenn Sie Daten im OCR-Format zwischen lokalen und cloudbasierten Datenspeichern (und nicht ORC-Dateien unverändert von der Quelle in die Senke) kopieren, müssen Sie auf dem Gatewaycomputer JRE (Java Runtime Environment) installieren. 
-	Komplexe Datentypen werden nicht unterstützt (STRUCT, MAP, LIST, UNION).
-	Für die ORC-Datei stehen drei [mit der Komprimierung zusammenhängende Optionen](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/) zur Verfügung: NONE, ZLIB, SNAPPY. Data Factory unterstützt das Lesen von Daten aus ORC-Dateien in eines der oben genannten komprimierten Formate. Zum Lesen der Daten wird der Komprimierungscodec in den Metadaten verwendet. Beim Schreiben in eine ORC-Datei wählt Data Factory hingegen ZLIB (Standardeinstellung für ORC). Derzeit steht keine Option zur Außerkraftsetzung dieses Verhaltens zur Verfügung. 

<!---HONumber=AcomDC_0525_2016-->