## <a name="specifying-compression"></a>Angeben der Komprimierung
Die Verarbeitung großer Datenmengen kann zu E/A- und Netzwerkengpässen führen. Aus diesem Grund kann die Komprimierung von Daten in Speichern nicht nur die Datenübertragung im Netzwerk beschleunigen und Speicherplatz sparen, sondern auch erhebliche Leistungssteigerungen bei der Verarbeitung von Big Data bewirken. Zurzeit wird die Komprimierung für dateibasierte Datenspeicher wie etwa Azure-Blobs oder das lokale Dateisystem unterstützt.  

> [!NOTE]
> Für Daten im **AvroFormat**, **OrcFormat** oder **ParquetFormat** werden keine Komprimierungseinstellungen unterstützt. Beim Lesen von Dateien in diesen Formaten erkennt und verwendet Data Factory den Komprimierungscodec in den Metadaten. Beim Schreiben von Dateien in diesen Formaten wählt Data Factory den Standard-Komprimierungscodec für das jeweilige Format. Beispiel: ZLIB für OrcFormat und SNAPPY für ParquetFormat.
>
>

Um die Komprimierung für ein Dataset anzugeben, verwenden Sie im JSON-Dataset die Eigenschaft für die **Komprimierung** wie im folgenden Beispiel:   

```json
{  
    "name": "AzureBlobDataSet",  
    "properties": {  
        "availability": {  
            "frequency": "Day",  
              "interval": 1  
        },  
        "type": "AzureBlob",  
        "linkedServiceName": "StorageLinkedService",  
        "typeProperties": {  
            "fileName": "pagecounts.csv.gz",  
            "folderPath": "compression/file/",  
            "compression": {  
                "type": "GZip",  
                "level": "Optimal"  
            }  
        }  
    }  
}  
```
Angenommen, das obige Beispieldataset wird als Ausgabe eines Kopiervorgangs verwendet. Der Kopiervorgang komprimiert die Ausgabedaten mit dem GZIP-Codec bei optimalem Verhältnis und schreibt die komprimierten Daten anschließend in die Datei „pagecounts.csv.gz“ in Azure Blob Storage.   

Der Abschnitt für die **Komprimierung** enthält zwei Eigenschaften:  

* **Typ:** Der Komprimierungscodec, z.B. **GZIP**, **Deflate**, **BZIP2** oder **ZipDeflate**.  
* **Ebene:** Das Komprimierungsverhältnis, z.B. **Optimal** oder **Schnellstes**.

  * **Schnellstes:** Der Komprimierungsvorgang wird schnellstmöglich abgeschlossen, auch wenn die resultierende Datei nicht optimal komprimiert ist.
  * **Optimal**: Die Daten sollten optimal komprimiert sein, auch wenn der Vorgang eine längere Zeit in Anspruch nimmt.

    Weitere Informationen finden Sie im Thema [Komprimierungsstufe](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) .

Wenn Sie die `compression`-Eigenschaft in einem JSON-Eingabedataset angeben, kann die Pipeline komprimierte Daten aus der Quelle lesen. Bei Angabe der Eigenschaft in einem JSON-Ausgabedataset kann der Kopiervorgang komprimierte Daten am Ziel schreiben. Es folgen einige Beispielszenarios:

* Lesen Sie GZIP-komprimierte Daten aus einem Azure-Blob, dekomprimieren Sie sie, und schreiben Sie die resultierenden Daten in eine Azure SQL-Datenbank. Sie definieren das Azure-Blob-Eingabedataset mit der `compression` `type`-JSON-Eigenschaft als GZIP.
* Lesen Sie Daten aus einer Nur-Text-Datei aus einem lokalen Dateisystem, komprimieren Sie sie mithilfe des GZip-Formats, und schreiben Sie die komprimierten Daten in einen Azure-Blob. Sie definieren ein Azure-Blob-Ausgabedataset mit der `compression` `type`-JSON-Eigenschaft als GZip.
* Lesen Sie die ZIP-Datei vom FTP-Server, dekomprimieren Sie sie, um die Dateien zu extrahieren, und stellen Sie die Dateien im Azure Data Lake Store bereit. Sie definieren ein FTP-Eingabedataset mit der `compression` `type`-JSON-Eigenschaft als ZipDeflate.
* Lesen Sie GZIP-komprimierte Daten aus einem Azure-Blob, dekomprimieren Sie sie, komprimieren Sie sie mit BZIP2, und schreiben Sie die resultierenden Daten in einen Azure-Blob. In diesem Fall definieren Sie für das Azure-Blob-Eingabedataset die Einstellung GZIP als `compression` `type` und für das Ausgabedataset BZIP2 als `compression` `type`.   


<!--HONumber=Feb17_HO3-->


