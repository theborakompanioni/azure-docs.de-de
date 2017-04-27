---
title: Kopieren oder Verschieben von Daten nach Storage mit AzCopy | Microsoft Docs
description: Verwenden Sie das Hilfsprogramm AzCopy zum Verschieben oder Kopieren von Daten zu oder von Blob-, Tabellen- und Dateiinhalten. Kopieren Sie Daten aus lokalen Dateien nach Azure Storage oder innerhalb von bzw. zwischen Speicherkonten. Migrieren Sie Ihre Daten ganz einfach nach Azure Storage.
services: storage
documentationcenter: 
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: aa155738-7c69-4a83-94f8-b97af4461274
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: seguler
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: f703da63c4243c73cf68d3df9953f73d2462ac1c
ms.lasthandoff: 04/06/2017


---
# <a name="transfer-data-with-the-azcopy-command-line-utility"></a>Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy
## <a name="overview"></a>Übersicht
AzCopy ist ein Windows Befehlszeilenprogramm, das zum Kopieren von Daten zu und von Microsoft Azure Blob, File und Table Storage entwickelt wurde, wobei durch einfache Befehle optimale Leistung erzielt wird. Sie können Daten von einem Objekt zu einem anderen innerhalb Ihres Speicherkontos oder von einem Speicherkonto zu einem anderen kopieren.

> [!NOTE]
> Diese Anleitung setzt voraus, dass Sie sich bereits mit [Azure Storage](https://azure.microsoft.com/services/storage/)auskennen. Falls dies nicht der Fall sein sollte, lesen Sie bitte zunächst die Dokumentation [Einführung in Azure Storage](storage-introduction.md) . Allem voran müssen Sie ein [Konto für Azure Storage](storage-create-storage-account.md#create-a-storage-account) erstellen, um mit AzCopy arbeiten zu können.
> 
> 

## <a name="download-and-install-azcopy"></a>Herunterladen und Installieren von AzCopy
### <a name="windows"></a>Windows
Laden Sie die [neueste Version von AzCopy](http://aka.ms/downloadazcopy)herunter.

### <a name="maclinux"></a>Mac/Linux
AzCopy ist für Mac-/Linux-Betriebssysteme nicht verfügbar. Jedoch ist die Azure-Befehlszeilenschnittstelle eine geeignete Alternative zum Kopieren von Daten zu und von Azure Storage. Lesen Sie [Verwenden der Azure-Befehlszeilenschnittstelle mit Azure-Speicher](storage-azure-cli.md) , um mehr zu erfahren.

## <a name="writing-your-first-azcopy-command"></a>Schreiben Ihres ersten AzCopy-Befehls
Die grundlegende Syntax für AzCopy-Befehle ist:

```azcopy
AzCopy /Source:<source> /Dest:<destination> [Options]
```

Öffnen Sie ein Befehlsfenster, und navigieren Sie zum Installationsverzeichnis von AzCopy auf Ihrem Computer – dort befindet sich die ausführbare Datei `AzCopy.exe` . Bei Bedarf können Sie den Speicherort für die AzCopy-Installation zum Systempfad hinzufügen. In der Standardeinstellung ist AzCopy in `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` oder `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy` installiert.

Die folgenden Beispiele zeigen eine Vielzahl von Szenarien zum Kopieren von Daten von und zu Microsoft Azure Blobs, Dateien und Tabellen. Für eine detailliertere Erklärung der in jedem Beispiel verwendeten Parameter gehen Sie zum Abschnitt [Einführung in die Parameter](#azcopy-parameters) .

## <a name="blob-download"></a>Blob: Herunterladen
### <a name="download-single-blob"></a>Herunterladen eines einzelnen Blobs

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"
```

Beachten Sie, dass, sofern der Ordner `C:\myfolder` noch nicht besteht, AzCopy diesen erstellt und `abc.txt ` in den neuen Ordner herunterlädt.

### <a name="download-single-blob-from-secondary-region"></a>Ein einzelnes Blob aus der sekundären Region herunterladen

```azcopy
AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

Beachten Sie, dass der Lesezugriff im georedundanten Speicher aktiviert sein muss.

### <a name="download-all-blobs"></a>Herunterladen aller Blobs

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S
```

Nehmen Sie an, die folgenden BLOBs befinden sich im angegebenen Container:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Nach dem Herunterladen enthält das Verzeichnis `C:\myfolder` die folgenden Dateien:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Wenn Sie die Option `/S`nicht angeben, werden keine Blobs heruntergeladen.

### <a name="download-blobs-with-specified-prefix"></a>Herunterladen von Blobs mit angegebenem Präfix

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S
```

Nehmen Sie an, die folgenden BLOBs befinden sich im angegebenen Container. Alle Blobs, die mit dem Präfix `a` beginnen, werden heruntergeladen:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Nach dem Herunterladen enthält der Ordner `C:\myfolder` die folgenden Dateien:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Das Präfix gilt für das virtuelle Verzeichnis, das den ersten Teil des Blobnamens bildet. Im obigen Beispiel stimmt das virtuelle Verzeichnis nicht mit dem angegebenen Präfix überein. Daher wird es nicht heruntergeladen. Darüber hinaus lädt AzCopy keine Blobs herunter, wenn die Option `\S` nicht angegeben ist.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>Für exportierte Dateien und Quell-Blobs dieselbe Uhrzeit der letzten Änderung festlegen

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT
```

Sie haben auch die Möglichkeit, Blobs auf Basis der Uhrzeit ihrer letzten Änderung vom Herunterladen auszuschließen. Fügen Sie die Option `/XN` hinzu, falls Sie beispielsweise Blobs, deren Uhrzeit der letzten Änderung die gleiche oder eine spätere ist als die der Zieldatei, ausschließen wollen:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN
```

Fügen Sie die Option `/XO` hinzu, falls Sie Blobs, deren Uhrzeit der letzten Änderung die gleiche oder eine frühere ist als die der Zieldatei, ausschließen wollen:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO
```

## <a name="blob-upload"></a>Blob: Hochladen
### <a name="upload-single-file"></a>Hochladen einer einzelnen Datei

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"
```

Wenn der angegebene Zielcontainer nicht vorhanden ist, wird er von AzCopy erstellt und die Datei anschließend in den Container hochgeladen.

### <a name="upload-single-file-to-virtual-directory"></a>Hochladen einer einzelnen Datei in ein virtuelles Verzeichnis

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt
```

Wenn das angegebene virtuelle Verzeichnis nicht vorhanden ist, lädt AzCopy die Datei so hoch, dass das virtuelle Verzeichnis Teil des Dateinamens wird (**z.B. `vd/abc.txt` im obigen Beispiel).

### <a name="upload-all-files"></a>Hochladen aller Dateien

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S
```

Wenn Sie die Option `/S` angeben, werden die Inhalte des angegebenen Verzeichnisses rekursiv in den Blobspeicher hochgeladen. Das bedeutet, dass alle Unterordner und die darin enthaltenen Dateien ebenfalls hochgeladen werden. Nehmen Sie z. B. an, die folgenden Dateien befinden sich im Ordner `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Nach dem Hochladen enthält der Container die folgenden Dateien:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Wenn Sie die Option `/S`nicht angeben, lädt AzCopy nicht rekursiv hoch. Nach dem Hochladen enthält der Container die folgenden Dateien:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a>Hochladen von Dateien, die einem angegebenen Muster entsprechen

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S
```

Nehmen Sie z. B. an, die folgenden Dateien befinden sich im Ordner `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Nach dem Hochladen enthält der Container die folgenden Dateien:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Wenn Sie die Option `/S`nicht angeben, lädt AzCopy nur die Blobs hoch, die sich nicht in einem virtuellen Verzeichnis befinden:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Angeben des MIME-Inhaltstyps für Ziel-Blobs
AzCopy legt den Inhaltstyp eines Zielblobs standardmäßig als `application/octet-stream`fest. Ab Version 3.1.0 können Sie den Inhaltstyp mit der Option `/SetContentType:[content-type]`explizit angeben. Mit dieser Syntax wird der Inhaltstyp für alle Blobs in einem Hochladevorgang festgelegt.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4
```

Wenn Sie für `/SetContentType` keinen Wert angeben, legt AzCopy den Inhaltstyp jedes Blobs und jeder Datei gemäß der jeweiligen Dateierweiterung fest.

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType
```

## <a name="blob-copy"></a>Blob: Kopieren
### <a name="copy-single-blob-within-storage-account"></a>Kopieren eines einzelnen Blobs innerhalb eines Azure Storage-Kontos

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt
```

Wenn Sie einen Blob innerhalb eines Azure Storage-Kontos kopieren, wird ein [serverseitiger Kopier](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) -Vorgang ausgeführt.

### <a name="copy-single-blob-across-storage-accounts"></a>Kopieren von Blobs zwischen Azure Storage-Konten

```azcopy
AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

Wenn Sie einen Blob zwischen Azure Storage-Konten kopieren, wird ein [serverseitiger Kopier](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) -Vorgang ausgeführt.

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Kopieren eines einzelnen Blobs aus der sekundären Region in die primäre Region

```azcopy
AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
```

Beachten Sie, dass der Lesezugriff im georedundanten Speicher aktiviert sein muss.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Kopieren eines einzelnen Blobs und seiner Momentaufnahmen zwischen Storage-Konten

```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot
```

Nach dem Kopiervorgang enthält der Zielcontainer den BLOB und seine Momentaufnahmen. Unter der Annahme, der BLOB im obigen Beispiel besitzt zwei Momentaufnahmen, enthält der Container folgenden BLOB und folgende Momentaufnahmen:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Synchrones Kopieren von Blobs zwischen Storage-Konten
Standardmäßig kopiert AzCopy Daten zwischen zwei Speicherendpunkten auf asynchrone Weise. Der Kopiervorgang wird also im Hintergrund ausgeführt und verwendet nur nicht reservierte Bandbreite, für die bezüglich der Kopiergeschwindigkeit für Blobs kein SLA besteht. AzCopy überprüft den Kopierstatus regelmäßig, bis der Vorgang beendet ist oder abbricht.

Die Option `/SyncCopy` gewährleistet, dass der jeweilige Kopiervorgang mit gleichmäßiger Geschwindigkeit erfolgt. Beim synchronen Kopieren lädt AzCopy die zu kopierenden Blobs aus der angegebenen Quelle in den lokalen Arbeitsspeicher herunter und dann in das Speicherziel des jeweiligen Blobs hoch.

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy
```

Anders als beim asynchronen Kopieren kann es bei der Verwendung von `/SyncCopy` zu Zusatzkosten für den ausgehenden Datenverkehr kommen. Um dies zu vermeiden, sollten Sie diese Option in einer Azure VM verwenden, die sich in derselben Region wie das Storage-Quellkonto befindet.

## <a name="file-download"></a>Datei: Herunterladen
### <a name="download-single-file"></a>Herunterladen einer einzelnen Datei

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt
```

Wenn es sich bei der angegebenen Quelle um eine Azure-Dateifreigabe handelt, müssen Sie entweder den genauen Dateinamen angeben (**z.B. `abc.txt`), um eine einzelne Datei herunterzuladen, oder die Option `/S`, um alle Dateien in der Freigabe rekursiv herunterzuladen. Wenn Sie sowohl ein Dateimuster als auch die Option `/S` angeben, führt dies zu einem Fehler.

### <a name="download-all-files"></a>Herunterladen aller Dateien

```azcopy
AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S
```

Beachten Sie, dass leere Ordner nicht heruntergeladen werden.

## <a name="file-upload"></a>Datei: Hochladen
### <a name="upload-single-file"></a>Hochladen einer einzelnen Datei

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt
```

### <a name="upload-all-files"></a>Hochladen aller Dateien

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S
```

Beachten Sie, dass leere Ordner nicht hochgeladen werden.

### <a name="upload-files-matching-specified-pattern"></a>Hochladen von Dateien, die einem angegebenen Muster entsprechen

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S
```

## <a name="file-copy"></a>Datei: Kopieren
### <a name="copy-across-file-shares"></a>Kopieren zwischen Dateifreigaben

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S
```
Wenn Sie eine Datei zwischen Dateifreigaben kopieren, wird ein [serverseitiger Kopiervorgang](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) ausgeführt.

### <a name="copy-from-file-share-to-blob"></a>Kopieren aus einer Dateifreigabe zum Blob

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S
```
Wenn Sie eine Datei von einer Dateifreigabe in ein Blob kopieren, wird ein [serverseitiger Kopiervorgang](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) ausgeführt.


### <a name="copy-from-blob-to-file-share"></a>Kopieren vom Blob zur Dateifreigabe

```azcopy
AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S
```
Wenn Sie eine Datei aus einem Blob in eine Dateifreigabe kopieren, wird ein [serverseitiger Kopiervorgang](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) ausgeführt.

### <a name="synchronously-copy-files"></a>Synchrones Kopieren von Dateien
Sie können die Option `/SyncCopy` angeben, um Daten synchron aus einem File Storage in einen anderen File Storage, aus einem File Storage in einen Blob Storage oder aus einem Blob Storage in einen File Storage zu kopieren. AzCopy lädt dazu die Quelldaten in den lokalen Arbeitsspeicher herunter, um diese dann wieder in das Ziel hochzuladen. Es gelten die Standardausgangskosten.

```azcopy
AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
```

Der Standardblobtyp beim Kopieren aus File Storage in einen Blob Storage ist Blockblob. Benutzer können die Option `/BlobType:page` angeben, um den Zielblobtyp zu ändern.

Hinweis: Anders als beim asynchronen Kopieren kann es bei der Verwendung von `/SyncCopy` zu Zusatzkosten für den ausgehenden Datenverkehr kommen. Um dies zu vermeiden, sollten Sie diese Option in dem virtuellen Azure-Computer verwenden, der sich in derselben Region wie das Quellspeicherkonto befindet.

## <a name="table-export"></a>Tabelle: Exportieren
### <a name="export-table"></a>Exportieren einer Tabelle

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key
```

AzCopy schreibt eine Manifestdatei in den angegebenen Zielordner. Die Manifestdatei wird beim Importvorgang verwendet, um die erforderlichen Datendateien zu finden, und um die Datenüberprüfung durchzuführen. Für die Manifestdatei wird standardmäßig die folgende Namenskonvention verwendet:

    <account name>_<table name>_<timestamp>.manifest

Ein Benutzer kann auch die Option `/Manifest:<manifest file name>` angeben, um den Namen der Manifestdatei festzulegen.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest
```

### <a name="split-export-into-multiple-files"></a>Aufteilen eines Exports auf mehrere Dateien

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100
```

AzCopy verwendet einen *Volumeindex* in den Dateinamen für die Aufteilungsdaten zum Unterscheiden mehrerer Dateien. Der Volumeindex besteht aus zwei Teilen, einem *Partitionsschlüsselbereichs-Index* und einem *Aufteilungsdateiindex*. Beide Indizes sind nullbasiert.

Der Partitionsschlüsselbereichs-Index ist 0, wenn der Benutzer die Option `/PKRS`nicht angibt.

Angenommen, AzCopy generiert zwei Datendateien, nachdem der Benutzer die Option `/SplitSize`angegeben hat. Die daraus resultierenden Datendateinamen könnten wie folgt aussehen:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Beachten Sie, dass der zulässige Mindestwert für die Option `/SplitSize` 32 MB lautet. Wenn es sich beim angegebenen Ziel um einen Blobspeicher handelt, teilt AzCopy die Datendatei auf, sobald die Größe die Blobgrößenbegrenzung (200 GB) erreicht, und zwar unabhängig davon, ob die Option `/SplitSize` durch den Benutzer angegeben wurde.

### <a name="export-table-to-json-or-csv-data-file-format"></a>Exportieren einer Tabelle im JSON- oder CSV-Datendateiformat
AzCopy exportiert Tabellen standardmäßig als JSON-Datendateien. Sie können mit der Option `/PayloadFormat:JSON|CSV` angeben, ob die Tabellen als JSON oder CSV exportiert werden sollen.

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV
```

Wenn das CSV-Nutzlastformat angegeben wird, generiert AzCopy auch eine Schemadatei mit der Dateierweiterung `.schema.csv` für jede Datendatei.

### <a name="export-table-entities-concurrently"></a>Zeitgleiches Exportieren von Tabellenentitäten

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"
```

AzCopy startet gleichzeitige Vorgänge zum Exportieren von Entitäten, wenn der Benutzer die Option `/PKRS`angibt. Jeder Vorgang exportiert einen Partitionsschlüsselbereich.

Beachten Sie, dass die Anzahl gleichzeitiger Vorgänge auch durch die Option `/NC`gesteuert wird. AzCopy verwendet beim Kopieren von Tabellenentitäten die Anzahl der Core-Prozessoren als den Standardwert von `/NC`, selbst wenn `/NC` nicht angegeben wurde. Wenn der Benutzer die Option `/PKRS`angibt, verwendet AzCopy den kleineren der beiden Werte (Partitionsschlüsselbereich im Vergleich zu implizit oder explizit angegebenen gleichzeitigen Vorgängen), um die Anzahl der zu startenden gleichzeitigen Vorgänge zu bestimmen. Geben Sie Geben Sie `AzCopy /?:NC` an der Befehlszeile ein, um weitere Informationen zu erhalten.

### <a name="export-table-to-blob"></a>Exportieren einer Tabelle in ein Blob

```azcopy
AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2
```

AzCopy generiert eine JSON-Datendatei in den Blob-Container mit der folgenden Namenskonvention:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

Die generierte JSON-Datendatei folgt dem Nutzlastformat für minimale Metadaten. Details in Bezug auf dieses Nutzlastformat finden Sie unter [Nutzlastformat für Tabellendienstvorgänge](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Beachten Sie, dass AzCopy beim Export von Tabellen in Blobs die Tabellenentitäten in die lokalen temporären Datendateien herunterlädt und diese Entitäten dann in das Blob hochlädt. Diese temporären Datendateien werden in den Journaldateiordner mit dem Standardpfad „<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>” gespeichert. Sie können die Option „/Z:[Journaldateiordner]“ angeben, um den Journaldateiordner-Speicherort und damit den temporären Datendateien-Speicherort zu ändern. Die Größe der temporären Datendateien wird durch die Tabellenentitäten und die Größe bestimmt, die Sie mit der Option „/SplitSize“ angegeben haben. Auch wenn die temporäre Datei auf dem lokalen Datenträger sofort gelöscht wird, sobald sie in das Blob hochgeladen wurde, sollten Sie sicherstellen, dass Sie über genügend lokalen Speicherplatz für diese temporären Datendateien verfügen, bevor sie gelöscht werden.

## <a name="table-import"></a>Tabelle: Importieren
### <a name="import-table"></a>Importieren einer Tabelle

```azcopy
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

Die Option `/EntityOperation` gibt an, wie Entitäten in die Tabelle eingefügt werden. Mögliche Werte:

* `InsertOrSkip`: Überspringt eine vorhandene Entität oder fügt eine neue Entität ein, wenn sie in der Tabelle nicht vorhanden ist.
* `InsertOrMerge`: Führt eine vorhandene Entität zusammen oder fügt eine neue Entität ein, wenn sie in der Tabelle nicht vorhanden ist.
* `InsertOrReplace`: Ersetzt eine vorhandene Entität oder fügt eine neue Entität ein, wenn sie in der Tabelle nicht vorhanden ist.

Beachten Sie, dass Sie die Option `/PKRS` im Importszenario nicht angeben können. Im Gegensatz zum Exportszenario, in dem Sie die Option `/PKRS` zum Starten gleichzeitiger Vorgänge angeben müssen, startet AzCopy beim Importieren einer Tabelle standardmäßig gleichzeitige Vorgänge. Die standardmäßige Anzahl gleichzeitig gestarteter Vorgänge entspricht der Anzahl an Core-Prozessoren. Sie können jedoch eine unterschiedliche Anzahl gleichzeitiger Vorgänge mit der Option `/NC` angeben. Geben Sie Geben Sie `AzCopy /?:NC` an der Befehlszeile ein, um weitere Informationen zu erhalten.

Beachten Sie, dass AzCopy nur den Import für JSON, nicht aber für CSV unterstützt. AzCopy unterstützt keine Tabellenimporte aus von Benutzern erstellten JSON- und Manifestdateien. Beide Dateien müssen aus einem AzCopy-Tabellenexport stammen. Ändern Sie weder die exportierte JSON-Datei noch die exportierte Manifestdatei, um Fehler zu vermeiden.

### <a name="import-entities-to-table-using-blobs"></a>Importieren von Entitäten in eine Tabelle mithilfe von Blobs
Angenommen, ein Blobcontainer enthält folgende Dateien: eine JSON-Datei, die eine Azure-Tabelle darstellt, und deren Manifestdatei.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

Sie können den folgenden Befehl ausführen, um Entitäten mithilfe der Manifestdatei im Blobcontainer in eine Tabelle zu importieren:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"
```

## <a name="other-azcopy-features"></a>Andere AzCopy-Funktionen
### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Nur Daten kopieren, die im Ziel nicht vorhanden sind.
Die Parameter `/XO` und `/XN` erlauben Ihnen, jeweils ältere oder neuere Quellressourcen vom Kopieren auszuschließen. Falls Sie nur Quellressourcen kopieren wollen, die im Ziel nicht existieren, können Sie beide Parameter im AzCopy-Befehl angeben.

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Beachten Sie, dass dies nicht unterstützt wird, wenn es sich bei Quelle oder Ziel um eine Tabelle handelt.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Verwenden einer Antwortdatei zum Angeben von Befehlszeilenparametern

```azcopy
AzCopy /@:"C:\responsefiles\copyoperation.txt"
```

Sie können beliebige AzCopy-Befehlszeilenparameter in eine Antwortdatei einfügen. AzCopy verarbeitet die Parameter in der Datei ebenso wie bei der Angabe über die Befehlszeile, wobei mit den Inhalten der Datei ein direkter Austausch erfolgt.

Nehmen wir an, eine Antwortdatei namens `copyoperation.txt`enthält die folgenden Zeilen. Jeder AzCopy-Parameter kann in einer einzelnen Zeile angegeben werden

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

oder in verschiedenen Zeilen:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

Wenn Sie den Parameter auf zwei Zeilen verteilen, tritt bei der Ausführung von AzCopy ein Fehler auf, wie hier für den Parameter `/sourcekey` dargestellt:

    http://myaccount.blob.core.windows.net/mycontainer
     C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Verwenden von mehreren Antwortdateien zum Angeben der Befehlszeilenparameter
Nehmen wir an, eine Antwortdatei namens `source.txt` gibt einen Quellcontainer an:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

Und eine Antwortdatei namens `dest.txt` gibt einen Zielordner im Dateisystem an:

    /Dest:C:\myfolder

Und eine Antwortdatei namens `options.txt` gibt die Optionen für AzCopy an:

    /S /Y

Verwenden Sie den folgenden Befehl, um AzCopy mit diesen Antwortdateien aufzurufen, die sich jeweils im Verzeichnis `C:\responsefiles`befinden:

```azcopy
AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   
```

AzCopy verarbeitet diesen Befehl auf dieselbe Weise wie bei der Angabe der einzelnen Parameter in der Befehlszeile:

```azcopy
AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y
```

### <a name="specify-a-shared-access-signature-sas"></a>Angeben einer Shared Access Signature (SAS)

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt
```

Sie können auch eine SAS in der Container-URI angeben:

```azcopy
AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S
```

### <a name="journal-file-folder"></a>Journaldateiordner
Sobald ein Befehl für AzCopy ausgeführt wird, prüft es, ob eine Journaldatei im Standardordner oder in einem Ordner vorhanden ist, den Sie über diese Option angegeben haben. Wenn die Journaldatei in keinem dieser Ordner vorhanden ist, behandelt AzCopy diesen Vorgang als neuen Vorgang und generiert somit eine neue Journaldatei.

Wenn die Journaldatei vorhanden ist, prüft AzCopy, ob die von Ihnen eingegebene Befehlszeile mit der Befehlszeile in der Journaldatei übereinstimmt. Wenn die beiden Befehlszeilen übereinstimmen, setzt AzCopy den unvollständigen Vorgang fort. Wenn sie nicht übereinstimmen, werden Sie aufgefordert, entweder die Journaldatei zu überschreiben, um einen neuen Vorgang zu starten, oder den aktuellen Vorgang abzubrechen.

Falls Sie den Standardspeicherort für die Journaldatei verwenden wollen:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z
```

Wenn Sie die Option `/Z` auslassen oder die Option `/Z` ohne den Ordnerpfad angeben (wie oben gezeigt), dann erstellt AzCopy die Journaldatei am Standardspeicherort `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Wenn die Journaldatei bereits vorhanden ist, setzt AzCopy den Vorgang auf Basis der Journaldatei fort.

Falls Sie einen benutzerdefinierten Speicherort für die Journaldatei angeben wollen:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\
```

In diesem Beispiel wird die Journaldatei erstellt, sofern diese nicht bereits vorhanden ist. Wenn sie vorhanden ist, setzt AzCopy den Vorgang auf Basis der Journaldatei fort.

Falls Sie einen AzCopy-Vorgang fortsetzen wollen:

```azcopy
AzCopy /Z:C:\journalfolder\
```

In diesem Beispiel wird der letzte Vorgang fortgesetzt, der möglicherweise nicht abgeschlossen werden konnte.

### <a name="generate-a-log-file"></a>Generieren einer Protokolldatei

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V
```

Wenn Sie die Option `/V` ohne einen Dateipfad zum ausführlichen Protokoll angeben, dann erstellt AzCopy die Protokolldatei am Standardspeicherort `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Ansonsten können Sie eine Protokolldatei an einem benutzerdefinierten Speicherort erstellen:

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log
```

Wenn Sie nach der Option `/V` einen relativen Pfad angeben, z. B. `/V:test/azcopy1.log`, dann wird das ausführliche Protokoll im aktuellen Arbeitsverzeichnis in einem Unterordner namens `test` erstellt.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Angeben der Anzahl der zu startenden gleichzeitigen Vorgängen
Die Option `/NC` gibt die Anzahl der gleichzeitigen Kopiervorgänge an. AzCopy startet standardmäßig eine bestimmte Anzahl gleichzeitiger Vorgänge zum Erhöhen des Datenübertragungsdurchsatzes. Bei Tabellenvorgängen entspricht die Anzahl gleichzeitiger Vorgänge der Anzahl der verfügbaren Prozessoren. Bei Blob- und Dateivorgängen entspricht die Anzahl gleichzeitiger Vorgänge der achtfachen Anzahl der verfügbaren Prozessoren. Wenn Sie AzCopy in einem Netzwerk mit geringer Bandbreite ausführen, können Sie eine niedrigere Anzahl für „/NC“ angeben, um Fehler durch gleichzeitig verwendete Ressourcen zu vermeiden.

### <a name="run-azcopy-against-azure-storage-emulator"></a>AzCopy über Azure-Speicheremulator ausführen
Sie können AzCopy über den [Azure-Speicheremulator](storage-use-emulator.md) ausführen. Für Blobs:

```azcopy
AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S
```

und Tabellen:

```azcopy
AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table
```

## <a name="azcopy-parameters"></a>Einführung in die Parameter
Parameter für AzCopy werden unten beschrieben. Sie können auch einen der folgenden Befehle von der Befehlszeile für die Unterstützung in Bezug auf die Verwendung von AzCopy eingeben:

* Für eine detaillierte Befehlszeilenhilfe für AzCopy: `AzCopy /?`
* Für eine detaillierte Hilfe zu einzelnen AzCopy-Parametern: `AzCopy /?:SourceKey`
* Für Befehlszeilenbeispiele: `AzCopy /?:Samples`

### <a name="sourcesource"></a>/Source: "Quelle"
Gibt die Quelldaten an, aus denen kopiert werden soll. Die Quelle kann ein Dateisystemverzeichnis, ein Blobcontainer, ein virtuelles Blobverzeichnis, eine Speicherdateifreigabe, ein Speicherdateiverzeichnis oder eine Azure-Tabelle sein.

**Gilt für:** Blobs, Dateien, Tabellen

### <a name="destdestination"></a>/Dest:"Ziel"
Geben das Ziel für das Kopieren an. Das Ziel kann ein Dateisystemverzeichnis, ein Blobcontainer, ein virtuelles Blobverzeichnis, eine Speicherdateifreigabe, ein Speicherdateiverzeichnis oder eine Azure-Tabelle sein.

**Gilt für:** Blobs, Dateien, Tabellen

### <a name="patternfile-pattern"></a>/Pattern:"Dateimuster"
Gibt ein Dateimuster an, das angibt, welche Dateien kopiert werden sollen. Das Verhalten des „/Pattern“-Parameters wird über den Speicherort der Quelldaten und über die Angabe der Option für den rekursiven Modus bestimmt. Der rekursive Modus wird über die Option „/S“ angegeben.

Wenn es sich bei der angegebenen Quelle um ein Verzeichnis im Dateisystem handelt, dann sind Standardplatzhalter wirksam und das bereitgestellte Dateimuster wird anhand von Dateien in diesem Verzeichnis verglichen. Wenn die Option „/S“ angegeben ist, vergleicht AzCopy das angegebene Muster auch mit allen Dateien in sämtlichen Unterordnern dieses Verzeichnisses.

Wenn die angegebene Quelle ein BLOB-Container oder ein virtuelles Verzeichnis ist, werden keine Platzhalter angewendet. Wenn die Option „/S“ angegeben ist, interpretiert AzCopy die angegebenen Dateimuster als Blobpräfix. Wenn die Option „/S“ nicht angegeben ist, vergleicht AzCopy die angegebenen Dateimuster mit den genauen Blobnamen.

Wenn es sich bei der angegebenen Quelle um eine Azure-Dateifreigabe handelt, müssen Sie entweder den genauen Dateinamen (z. B. „abc.txt“) angeben, um eine einzelne Datei zu kopieren, oder die Option „/S“ angeben, um alle Dateien in der Freigabe rekursiv zu kopieren. Wenn Sie sowohl ein Dateimuster als auch die Option „/S“ angeben, führt dies zu einem Fehler.

AzCopy beachtet für das Abgleichen die Groß-/Kleinschreibung, wenn die Quelle (/Source) ein BLOB-Container oder ein virtuelles BLOB-Verzeichnis ist. In allen anderen Fällen wird die Groß-/Kleinschreibung nicht beachtet.

Wenn kein Dateimuster angegeben ist, ist das Standarddateimuster für einen Dateisystem-Speicherort *auskennen.* oder ein leeres Präfix für einen Azure Storage-Speicherort. Das Angeben mehrerer Dateimuster wird nicht unterstützt.

**Gilt für:** Blobs, Dateien

### <a name="destkeystorage-key"></a>/DestKey:"Speicherschlüssel"
Gibt den Speicherkontoschlüssel für die Zielressource an.

**Gilt für:** Blobs, Dateien, Tabellen

### <a name="destsassas-token"></a>/DestSAS:"SAS-Token"
Gibt eine SAS (Shared Access Signature) mit LESE- und SCHREIB-Berechtigung für das Ziel an (falls zutreffend). Schließen Sie die SAS in doppelte Anführungszeichen ein, da sie möglicherweise spezielle Befehlszeilenzeichen enthält.

Wenn die Zielressource ein BLOB-Container, eine Dateifreigabe oder eine Tabelle ist, können Sie entweder diese Option gefolgt vom SAS-Token oder die SAS als Teil des Ziel-BLOB-Containers, der Dateifreigabe oder des URI der Tabelle ohne diese Option angeben.

Wenn es sich bei der Quelle und beim Ziel um Blobs handelt, muss sich das Zielblob im selben Speicherkonto wie das Quellblob befinden.

**Gilt für:** Blobs, Dateien, Tabellen

### <a name="sourcekeystorage-key"></a>/SourceKey:"Speicherschlüssel"
Gibt den Speicherkontoschlüssel für die Quellressource an.

**Gilt für:** Blobs, Dateien, Tabellen

### <a name="sourcesassas-token"></a>/SourceSAS:"SAS-Token"
Gibt eine Shared Access Signature mit LESE- und AUFLISTUNGS-Berechtigung für die Quelle an (falls zutreffend). Schließen Sie die SAS in doppelte Anführungszeichen ein, da sie möglicherweise spezielle Befehlszeilenzeichen enthält.

Wenn die Quellressource ein Blobcontainer ist und weder ein Schlüssel noch eine SAS angegeben wird, wird der Blobcontainer über den anonymen Zugriff gelesen.

Wenn die Quelle eine Dateifreigabe oder Tabelle ist, muss ein Schlüssel oder eine SAS angegeben werden.

**Gilt für:** Blobs, Dateien, Tabellen

### <a name="s"></a>/S
Gibt den rekursiven Modus für Kopiervorgänge an. Im rekursiven Modus kopiert AzCopy alle BLOBs oder Dateien, die mit dem angegebenen Dateimuster übereinstimmen, einschließlich der BLOBs oder Dateien in den Unterordnern.

**Gilt für:** Blobs, Dateien

### <a name="blobtypeblock--page--append"></a>/BlobType:"block" | "page" | "append"
Gibt an, ob es sich beim Zielblob um ein Block-, Seiten- oder Anfügeblob handelt. Diese Option gilt nur, wenn Sie einen Blob hochladen. Andernfalls tritt ein Fehler auf. Wenn das Ziel ein Blob ist und diese Option nicht angegeben wurde, erstellt AzCopy standardmäßig ein Blockblob.

**Gilt für:** Blobs

### <a name="checkmd5"></a>/CheckMD5
Berechnet einen MD5-Hash für heruntergeladene Daten und überprüft, ob der in der Content-MD5-Eigenschaft des BLOBs oder der Datei gespeicherte MD5-Hash mit dem berechneten Hash übereinstimmt. Die MD5-Prüfung ist standardmäßig deaktiviert, daher müssen Sie diese Option angeben, um die MD5-Prüfung beim Herunterladen von Daten auszuführen.

Beachten Sie, dass Azure Storage nicht dafür garantiert, dass der für den BLOB oder die Datei gespeicherte MD5-Hash auf dem neuesten Stand ist. Der Client ist für die MD5-Aktualisierung zuständig, wenn der BLOB oder die Datei geändert wird.

AzCopy legt die Content-MD5-Eigenschaft für einen Azure-BLOB oder eine entsprechende Datei immer fest, nachdem diese(r) in den Dienst hochgeladen wurde.  

**Gilt für:** Blobs, Dateien

### <a name="snapshot"></a>/Snapshot
Gibt an, ob Momentaufnahmen übertragen werden. Diese Option ist nur gültig, wenn es sich bei der Quelle um einen BLOB handelt.

Die übertragenen Blobmomentaufnahmen werden im folgenden Format umbenannt: „Blob-Name (Zeitpunkt-der-Momentaufnahme).Erweiterung“.

Momentaufnahmen werden standardmäßig nicht kopiert.

**Gilt für:** Blobs

### <a name="vverbose-log-file"></a>/V:[Ausführliche Protokolldatei]
Gibt ausführliche Statusmeldungen in eine Protokolldatei aus.

Die ausführliche Protokolldatei erhält in `%LocalAppData%\Microsoft\Azure\AzCopy`standardmäßig die Bezeichnung „AzCopyVerbose.log“. Wenn Sie für diese Option einen vorhandenen Dateispeicherort angeben, wird das ausführliche Protokoll an diese Datei angefügt.  

**Gilt für:** Blobs, Dateien, Tabellen

### <a name="zjournal-file-folder"></a>/Z:[Journaldatei-Ordner]
Gibt einen Journaldateiordner zum Fortsetzen eines Vorgangs an.

AzCopy unterstützt die Fortsetzung immer, wenn ein Vorgang unterbrochen wurde.

Wenn diese Option nicht angegeben oder ohne einen Ordnerpfad angegeben wird, dann erstellt AzCopy die Journaldatei am Standardspeicherort „%LocalAppData%\Microsoft\Azure\AzCopy“.

Sobald ein Befehl für AzCopy ausgeführt wird, prüft es, ob eine Journaldatei im Standardordner oder in einem Ordner vorhanden ist, den Sie über diese Option angegeben haben. Wenn die Journaldatei in keinem dieser Ordner vorhanden ist, behandelt AzCopy diesen Vorgang als neuen Vorgang und generiert somit eine neue Journaldatei.

Wenn die Journaldatei vorhanden ist, prüft AzCopy, ob die von Ihnen eingegebene Befehlszeile mit der Befehlszeile in der Journaldatei übereinstimmt. Wenn die beiden Befehlszeilen übereinstimmen, setzt AzCopy den unvollständigen Vorgang fort. Wenn sie nicht übereinstimmen, werden Sie aufgefordert, entweder die Journaldatei zu überschreiben, um einen neuen Vorgang zu starten, oder den aktuellen Vorgang abzubrechen.

Nachdem der Vorgang erfolgreich abgeschlossen wurde, wird die Journaldatei gelöscht.

Beachten Sie, dass die Fortsetzung eines Vorgangs aus einer Journaldatei nicht unterstützt wird, die mit einer früheren Version von AzCopy erstellt wurde.

**Gilt für:** Blobs, Dateien, Tabellen

### <a name="parameter-file"></a>/@:"Parameterdatei"
Gibt eine Datei an, die Parameter enthält. AzCopy verarbeitet die Parameter in der Datei auf die gleiche Weise wie bei der Angabe über die Befehlszeile.

In einer Antwortdatei können Sie entweder mehrere Parameter in einer einzelnen Zeile oder jeden Parameter in einer eigenen Zeile angeben. Beachten Sie, dass ein einzelner Parameter nicht mehrere Zeilen umfassen darf.

Antwortdateien können Kommentarzeilen einbeziehen, die mit dem Symbol „#“ beginnen.

Sie können mehrere Antwortdateien angeben. Beachten Sie jedoch, dass AzCopy geschachtelte Antwortdateien nicht unterstützt.

**Gilt für:** Blobs, Dateien, Tabellen

### <a name="y"></a>/Y
Unterdrückt alle Bestätigungsaufforderungen von AzCopy.

**Gilt für:** Blobs, Dateien, Tabellen

### <a name="l"></a>/L
Gibt nur einen Auflistungsvorgang an. Dabei werden keine Daten kopiert.

AzCopy interpretiert die Verwendung dieser Option so, als ob ein Ausführen der Befehlszeile ohne die Option „/L“ simuliert und gezählt wird, wie viele Objekte kopiert werden. Sie können gleichzeitig die Option „/V“ angeben, um zu überprüfen, welche Objekte in das ausführliche Protokoll kopiert werden.

Das Verhalten dieser Option wird auch durch den Speicherort der Quelldaten sowie dadurch bestimmt, ob die Option "/S" für den rekursiven Modus und die Dateimusteroption "/Pattern" vorhanden sind.

Wird diese Option verwendet, benötigt AzCopy AUFLISTUNGS- und LESE-Berechtigung für den Quellspeicherort.

**Gilt für:** Blobs, Dateien

### <a name="mt"></a>/MT
Legt für die Uhrzeit der letzten Änderung der heruntergeladenen Datei denselben Zeitpunkt wie für den Quell-BLOB oder die Quelldatei fest.

**Gilt für:** Blobs, Dateien

### <a name="xn"></a>/XN
Schließt eine neuere Quellressource aus. Die Ressource wird nicht kopiert, wenn der Zeitpunkt der letzten Änderung der Quelle mit dem Ziel identisch oder neuer ist.

**Gilt für:** Blobs, Dateien

### <a name="xo"></a>/XO
Schließt eine ältere Quellressource aus. Die Ressource wird nicht kopiert, wenn der Zeitpunkt der letzten Änderung der Quelle mit dem Ziel identisch oder älter ist.

**Gilt für:** Blobs, Dateien

### <a name="a"></a>/A
Lädt ausschließlich Dateien hoch, für die das Archivattribut festgelegt ist.

**Gilt für:** Blobs, Dateien

### <a name="iarashcnetoi"></a>/IA:[RASHCNETOI]
Lädt ausschließlich Dateien hoch, für die eines der angegebenen Attribute festgelegt ist.

Zu den verfügbaren Attributen zählen:

* R = Schreibgeschützte Dateien
* A = Dateien, die bereit sind für die Archivierung
* S = Systemdateien
* H = Versteckte Dateien
* C = Komprimierte Dateien
* N = Normale Dateien
* E = Verschlüsselte Dateien
* T = Temporäre Dateien
* O = Offlinedateien
* I = Nicht indizierte Dateien

**Gilt für:** Blobs, Dateien

### <a name="xarashcnetoi"></a>/XA:[RASHCNETOI]
Schließt Dateien aus, für die eines der angegebenen Attribute festgelegt ist.

Zu den verfügbaren Attributen zählen:

* R = Schreibgeschützte Dateien
* A = Dateien, die bereit sind für die Archivierung
* S = Systemdateien
* H = Versteckte Dateien
* C = Komprimierte Dateien
* N = Normale Dateien
* E = Verschlüsselte Dateien
* T = Temporäre Dateien
* O = Offlinedateien
* I = Nicht indizierte Dateien

**Gilt für:** Blobs, Dateien

### <a name="delimiterdelimiter"></a>/Delimiter:"Trennzeichen"
Zeigt das Trennzeichen an, mit dem virtuelle Verzeichnisse in einem BLOB-Namen getrennt werden.

AzCopy verwendet „/“ standardmäßig als Trennzeichen. AzCopy unterstützt jedoch die Verwendung beliebiger allgemeiner Zeichen (z. B. @, # oder %) als Trennzeichen. Wenn Sie eines dieser Sonderzeichen auf der Befehlszeile einbeziehen müssen, schließen Sie den Dateinamen in doppelte Anführungszeichen ein.

Diese Option kann nur zum Herunterladen von BLOBs angewendet werden.

**Gilt für:** Blobs

### <a name="ncnumber-of-concurrent-operations"></a>/NC:"Anzahl-gleichzeitiger-Vorgänge"
Gibt die Anzahl gleichzeitiger Vorgänge an.

AzCopy startet standardmäßig eine bestimmte Anzahl gleichzeitiger Vorgänge zum Erhöhen des Datenübertragungsdurchsatzes. Beachten Sie, dass eine große Anzahl gleichzeitiger Vorgänge in einer Umgebung mit geringer Bandbreite die Netzwerkverbindung überlasten kann, wodurch möglicherweise verhindert wird, dass die Vorgänge vollständig abgeschlossen werden. Senken Sie die Anzahl gleichzeitiger Vorgänge auf Basis der tatsächlich verfügbaren Netzwerkbandbreite.

Maximal können 512 gleichzeitige Vorgänge ausgeführt werden.

**Gilt für:** Blobs, Dateien, Tabellen

### <a name="sourcetypeblob--table"></a>/SourceType:"Blob" | "Table"
Gibt an, dass die Ressource `source` ein Blob ist, das in der lokalen Entwicklungsumgebung verfügbar ist und im Speicheremulator ausgeführt wird.

**Gilt für:** Blobs, Tabellen

### <a name="desttypeblob--table"></a>/DestType:"Blob" | "Table"
Gibt an, dass die Ressource `destination` ein Blob ist, das in der lokalen Entwicklungsumgebung verfügbar ist und im Speicheremulator ausgeführt wird.

**Gilt für:** Blobs, Tabellen

### <a name="pkrskey1key2key3"></a>/PKRS:"Schlüssel1#Schlüssel2#Schlüssel3#..."
Teilt den Partitionsschlüsselbereich auf, um parallel das Exportieren von Tabellendaten zu ermöglichen, wodurch die Geschwindigkeit des Exportvorgangs erhöht wird.

Wenn diese Option nicht angegeben ist, verwendet AzCopy einen einzelnen Thread zum Exportieren von Tabellenentitäten. Wenn der Benutzer beispielsweise /PKRS:"aa#bb" angibt, startet AzCopy drei gleichzeitige Vorgänge.

Jeder Vorgang exportiert einen der drei Partitionsschlüsselbereiche, wie dies im Folgenden gezeigt wird.

  [erster Partitionsschlüssel, aa)

  [aa, bb)

  [bb, letzter Partitionsschlüssel]

**Gilt für:** Tabellen

### <a name="splitsizefile-size"></a>/SplitSize:"Dateigröße"
Gibt die Teilungsgröße der exportierten Datei in MB an. Der zulässige Mindestwert ist 32.

Wenn diese Option nicht angegeben ist, exportiert AzCopy Tabellendaten in eine einzelne Datei.

Wenn die Tabellendaten in ein Blob exportiert wird, und die exportierte Dateigröße die 200-GB-Begrenzung für die Blobgröße erreicht, teilt AzCopy die exportierte Datei auf, selbst wenn diese Option nicht angegeben ist.

**Gilt für:** Tabellen

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/EntityOperation:"InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"
Gibt das Tabellendaten-Importverhalten an.

* InsertOrSkip – Überspringt eine vorhandene Entität oder fügt eine neue Entität ein, wenn sie in der Tabelle nicht vorhanden ist.
* InsertOrMerge – Führt eine vorhandene Entität zusammen oder fügt eine neue Entität ein, wenn sie in der Tabelle nicht vorhanden ist.
* InsertOrReplace – Ersetzt eine vorhandene Entität oder fügt eine neue Entität ein, wenn sie in der Tabelle nicht vorhanden ist.

**Gilt für:** Tabellen

### <a name="manifestmanifest-file"></a>/Manifest: "Manifestdatei"
Gibt die Manifestdatei für den Tabellenexport- und -importvorgang an.

Diese Option ist während eines Exportvorgangs optional. AzCopy generiert eine Manifestdatei mit vordefiniertem Namen, wenn diese Option nicht angegeben ist.

Diese Option ist während eines Importvorgangs erforderlich, damit die Datendateien gefunden werden.

**Gilt für:** Tabellen

### <a name="synccopy"></a>/SyncCopy
Gibt an, ob Blobs oder Dateien synchron zwischen zwei Azure-Speicherendpunkten kopiert werden sollen.

Standardmäßig führt AzCopy serverseitige asynchrone Kopiervorgänge aus. Geben Sie diese Option an, um Kopiervorgänge synchron ausführen zu lassen. Blobs und Dateien werden so in den lokalen Arbeitsspeicher herunter- und dann in den Azure-Speicher hochgeladen.

Diese Option eignet sich zum Kopieren von Dateien innerhalb eines Blob-Speichers oder eines Dateispeichers sowie zwischen Blob- und Dateispeicher.

**Gilt für:** Blobs, Dateien

### <a name="setcontenttypecontent-type"></a>/SetContentType: "Inhaltstyp"
Gibt den MIME-Inhaltstyp für Ziel-Blobs oder -dateien an.

AzCopy legt den Inhaltstyp eines Blobs oder einer Datei standardmäßig als „application/octet-stream“ fest. Sie können den Inhaltstyp für alle Blobs oder Dateien ändern, indem Sie für diese Option einen Wert angeben.

Wenn Sie für diese Option keinen Wert angeben, legt AzCopy den Inhaltstyp jedes Blobs und jeder Datei gemäß der jeweiligen Dateierweiterung fest.

**Gilt für:** Blobs, Dateien

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: "JSON" | "CSV"
Gibt das Format der exportierten Tabellendatendatei an.

Ist diese Option nicht angegeben, exportiert AzCopy eine Tabellendatendatei standardmäßig im JSON-Format.

**Gilt für:** Tabellen

## <a name="known-issues-and-best-practices"></a>Bekannte Probleme und Best Practices
### <a name="limit-concurrent-writes-while-copying-data"></a>Einschränken gleichzeitiger Schreibvorgänge beim Kopieren von Daten
Wenn Sie BLOBs oder Dateien mit AzCopy kopieren, denken Sie daran, dass eine andere Anwendung die Daten möglicherweise modifiziert, während Sie diese kopieren. Stellen Sie nach Möglichkeit sicher, dass die von Ihnen kopierten Daten während des Kopiervorgangs nicht verändert werden. Wenn Sie z. B. eine virtuelle Festplatte (Virtual Hard Disk, VHD) mit einem virtuellen Azure-Computer kopieren, stellen Sie sicher, dass derzeit keine anderen Anwendungen auf diese virtuelle Festplatte schreiben. Eine gute Möglichkeit hierfür bietet das Leasen der Ressource, die kopiert werden soll. Alternativ können Sie zunächst eine Momentaufnahme der virtuellen Festplatte (VHD) erstellen und anschließend die Momentaufnahme kopieren.

Wenn Sie andere Anwendungen nicht daran hindern können, während des Kopiervorgangs in BLOBs oder Dateien zu schreiben, beachten Sie, dass die kopierten Ressourcen bei der Beendigung des Auftrags möglicherweise nicht mehr vollständig mit den Quellressourcen übereinstimmen.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Führen Sie eine AzCopy-Instanz auf einem Computer aus.
AzCopy ist darauf ausgelegt, die Auslastung Ihrer Computerressourcen zu maximieren, um die Datenübertragung zu beschleunigen. Am besten führen Sie nur eine AzCopy-Instanz auf einem Computer aus und geben die Option `/NC` an, wenn mehrere gleichzeitige Vorgänge erforderlich sind. Geben Sie Geben Sie `AzCopy /?:NC` an der Befehlszeile ein, um weitere Informationen zu erhalten.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>Aktivieren Sie den FIPS-konformen MD5-Algorithmus für AzCopy, wenn Sie "FIPS-konformen Algorithmus für Verschlüsselung, Hashing und Signatur verwenden".
AzCopy verwendet standardmäßig die MD5-Implementierung von .NET, um den MD5 beim Kopieren von Objekten zu berechnen. Es gibt aber einige Sicherheitsanforderungen, die zu berücksichtigen sind, damit AzCopy eine FIPS-konforme MD5-Einstellung aktivieren kann.

Sie können eine Anwendungskonfigurationsdatei namens `AzCopy.exe.config` erstellen, die die Eigenschaft `AzureStorageUseV1MD5` enthält, und diese Datei im selben Speicherort wie "AzCopy.exe" ablegen.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

Ist die Eigenschaft „AzureStorageUseV1MD5„ gleich „true“ (Standardwert), verwendet AzCopy die MD5-Implementierung von .NET.
Ist die Eigenschaft gleich "false", verwendet AzCopy den FIPS-konformen MD5-Algorithmus.

Auf einem Windows-Computer ist der FIPS-konforme Algorithmus standardmäßig deaktiviert. Sie können "secpol.msc" im Fenster "Ausführen" eingeben und diesen Parameter über "Sicherheitseinstellungen -> Lokale Richtlinien -> Sicherheitsoptionen -> Systemkryptografie: FIPS-konformen Algorithmus für Verschlüsselung, Hashing und Signatur verwenden" überprüfen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure Storage und zu AzCopy finden Sie in den folgenden Ressourcen:

### <a name="azure-storage-documentation"></a>Azure Storage-Dokumentation:
* [Einführung in Azure Storage](storage-introduction.md)
* [Verwenden des Blob-Speichers mit .NET](storage-dotnet-how-to-use-blobs.md)
* [Verwenden von File Storage mit .NET](storage-dotnet-how-to-use-files.md)
* [Verwenden des Tabellenspeichers mit .NET](storage-dotnet-how-to-use-tables.md)
* [Erstellen, Verwalten oder Löschen von Speicherkonten](storage-create-storage-account.md)

### <a name="azure-storage-blog-posts"></a>Azure Storage-Blogbeiträge:
* [Einführung in die Vorschau der Microsoft Azure Storage Data Movement-Bibliothek](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
* [AzCopy: Einführung in das synchrone Kopieren und benutzerdefinierte Inhaltstypen](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
* [AzCopy: Ab sofort allgemein verfügbar: AzCopy 3.0 sowie die Vorschau auf AzCopy 4.0 mit Tabellen- und Dateiunterstützung](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
* [AzCopy: Optimiert für große Kopierszenarien](http://go.microsoft.com/fwlink/?LinkId=507682)
* [AzCopy: Unterstützung des Lesezugriffs auf georedundanten Speicher](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
* [AzCopy: Übertragen von Daten mit neu startbarem Modus und SAS-Token](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
* [AzCopy: Verwenden des kontoübergreifenden Kopierblobs](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
* [AzCopy: Hochladen/Herunterladen von Dateien für Microsoft Azure-Blobs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)


