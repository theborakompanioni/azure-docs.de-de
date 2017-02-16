---
title: Verschieben von Daten aus einem lokalen HDFS | Microsoft Docs
description: Erfahren Sie, wie Sie Daten aus einem lokalem HDFS mithilfe von Azure Data Factory verschieben.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3215b82d-291a-46db-8478-eac1a3219614
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: d49d7e6b4a9485c2371eb02ac8068adfde9bad6b
ms.openlocfilehash: c7f27fe2560c1800f05c205a73fe738cc609d642


---
# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Verschieben von Daten aus einem lokalem HDFS mithilfe von Azure Data Factory
Dieser Artikel beschreibt, wie Sie die Kopieraktivität in einer Azure Data Factory verwenden können, um Daten aus einem lokalen HDFS (Hadoop Distributed File System) in einen anderen Datenspeicher zu verschieben. Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit der Kopieraktivität und unterstützten Datenspeicherkombinationen enthält.

Data Factory unterstützt derzeit nur das Verschieben von Daten aus einem lokalen HDFS in andere Datenspeicher, aber nicht das Verschieben aus anderen Datenspeichern in ein lokales HDFS.

## <a name="enabling-connectivity"></a>Herstellen der Verbindung
Der Data Factory-Dienst unterstützt das Herstellen einer Verbindung mit einem lokalen HDFS über das Datenverwaltungsgateway. Im Artikel [Verschieben von Daten zwischen lokalen Standorten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) erfahren mehr zum Datenverwaltungsgateway und erhalten eine schrittweise Anleitung zum Einrichten des Gateways. Verwenden Sie das Gateway, um eine Verbindung mit dem HDFS herzustellen, auch wenn es auf einem virtuellen Azure-IaaS-Computer gehostet wird.

Grundsätzlich ist es zwar möglich, das Gateway auf dem gleichen lokalen Computer bzw. auf dem virtuellen Azure-Computer zu installieren, auf dem sich auch das HDFS befindet, es empfiehlt sich jedoch, das Gateway auf einem separaten Computer bzw. auf einem separaten virtuellen Azure-IaaS-Computer zu installieren. Diese Vorgehensweise dient zur Vermeidung von Ressourcenkonflikten und verbessert die Leistung. Wenn Sie das Gateway auf einem separaten Computer installieren, muss dieser Computer auf den Computer mit dem HDFS zugreifen können.

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit zum Erstellen einer Pipeline, die Daten aus einem lokalen HDFS kopiert, ist die Verwendung des Assistenten zum Kopieren von Daten. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Die folgenden Beispiele zeigen JSON-Beispieldefinitionen, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder mit [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Darin wird veranschaulicht, wie Sie Daten aus einem lokalen HDFS in Azure Blob Storage kopieren. Daten können jedoch auch mithilfe der Kopieraktivität in Azure Data Factory in eine beliebige der [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden.

## <a name="sample-copy-data-from-on-premises-hdfs-to-azure-blob"></a>Beispiel: Kopieren von Daten aus einem lokalen HDFS in ein Azure-Blob
In diesem Beispiel wird gezeigt, wie Sie Daten aus einem lokalen HDFS in Azure Blob Storage kopieren. Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** in die [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden.  

Das Beispiel enthält die folgenden Data Factory-Entitäten:

1. Einen verknüpften Dienst des Typs [OnPremisesHdfs](#hdfs-linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [FileShare](#hdfs-dataset-type-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
5. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [FileSystemSource](#hdfs-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) verwendet

Das Beispiel kopiert stündlich Daten aus einem lokalen HDFS in ein Azure-Blob. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

Als Erstes richten Sie das Datenverwaltungsgateway ein. Anweisungen dazu finden Sie im Artikel [Verschieben von Daten zwischen lokalen Standorten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) .

**Mit HDFS verknüpfter Dienst**: In diesem Beispiel wird die Windows-Authentifizierung verwendet. Informationen zu den verwendbaren Authentifizierungstypen finden Sie im Abschnitt [Mit HDFS verknüpfter Dienst](#hdfs-linked-service-properties).

```JSON
{
    "name": "HDFSLinkedService",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

**Mit Azure Storage verknüpfter Dienst**

```JSON
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

**HDFS-Eingabedataset**: Dieses Dataset verweist auf den HDFS-Ordner „DataTransfer/UnitTest/“. Die Pipeline kopiert alle Dateien in diesem Ordner an das Ziel.

Durch Festlegen von „external“ auf „true“ wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

```JSON
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

**Azure-Blob-Ausgabedataset**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). Der Ordnerpfad des Blobs wird basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

```JSON
{
    "name": "OutputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Pipeline mit Kopieraktivität**

Die Pipeline enthält eine Kopieraktivität, die für das Verwenden der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **FileSystemSource** und der Typ **sink** auf **BlobSink** festgelegt. Die für die **query** -Eigenschaft angegebene SQL-Abfrage wählt die aus der letzten Stunde zu kopierenden Daten aus.

```JSON
{
    "name": "pipeline",
    "properties":
    {
        "activities":
        [
            {
                "name": "HdfsToBlobCopy",
                "inputs": [ {"name": "InputDataset"} ],
                "outputs": [ {"name": "OutputDataset"} ],
                "type": "Copy",
                "typeProperties":
                {
                    "source":
                    {
                        "type": "FileSystemSource"
                    },
                    "sink":
                    {
                        "type": "BlobSink"
                    }
                },
                "policy":
                {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="hdfs-linked-service-properties"></a>Eigenschaften des mit HDFS verknüpften Diensts
Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die für den mit HDFS verknüpften Dienst spezifisch sind.

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| Typ |Die type-Eigenschaft muss auf **Hdfs** |Ja |
| Url |Die URL für das HDFS. |Ja |
| authenticationType |Anonym oder Windows. <br><br> Um Ihre lokale Umgebung zur Verwendung der **Kerberos-Authentifizierung** für den HDFS-Connector einzurichten, lesen Sie [diesen Abschnitt](#use-kerberos-authentication-for-hdfs-connector). |Ja |
| userName |Der Benutzername für die Windows-Authentifizierung. |Ja (für die Windows-Authentifizierung) |
| password |Das Kennwort für die Windows-Authentifizierung. |Ja (für die Windows-Authentifizierung) |
| gatewayName |Der Name des Gateways, das der Data Factory-Dienst zum Verbinden mit dem HDFS verwenden soll. |Ja |
| encryptedCredential |[New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) für den Zugriff. |Nein |

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für ein lokales HDFS finden Sie unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md).

### <a name="using-anonymous-authentication"></a>Verwenden der anonymen Authentifizierung

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-windows-authentication"></a>Verwenden der Windows-Authentifizierung

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>Verwenden der Kerberos-Authentifizierung für den HDFS-Connector
Zur Einrichtung der lokalen Umgebung für die Verwendung der Kerberos-Authentifizierung im HDFS-Connector gibt es zwei Optionen. Wählen Sie die Option, die in Ihrem Fall besser passt.
* Option 1: [Einrichten des Gatewaycomputers für den Beitritt zum Kerberos-Bereich](#kerberos-join-realm)
* Option 2: [Aktivieren der gegenseitigen Vertrauensstellung zwischen der Windows-Domäne und dem Kerberos-Bereich](#kerberos-mutual-trust)

### <a name="a-namekerberos-join-realmaoption-1-make-gateway-machine-join-kerberos-realm"></a><a name="kerberos-join-realm"></a>Option 1: Einrichten des Gatewaycomputers für den Beitritt zum Kerberos-Bereich

#### <a name="requirement"></a>Anforderung:

* Der Gatewaycomputer muss dem Kerberos-Bereich beitreten und darf in keine Windows-Domäne eingebunden sein.

#### <a name="how-to-configure"></a>Konfiguration:

**Auf dem Gatewaycomputer:**

1.  Führen Sie das Dienstprogramm **Ksetup** aus, um den Kerberos-KDC-Server und -Bereich zu konfigurieren.

    Der Computer muss als Mitglied einer Arbeitsgruppe konfiguriert werden, da sich Kerberos-Bereiche von Windows-Domänen unterscheiden. Sie erreichen dies, indem Sie den Kerberos-Bereich einrichten und einen KDC-Server hinzufügen, wie im Folgenden erläutert. Ersetzen Sie *REALM.COM* durch Ihren eigenen Bereich.

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    Nach Ausführung dieser beiden Befehle **starten Sie den Computer neu**.

2.  Überprüfen Sie die Konfiguration mit dem Befehl **Ksetup**. Die Ausgabe sollte wie folgt aussehen:

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

**In Azure Data Factory:**

* Konfigurieren Sie den HDFS-Connector mithilfe der **Windows-Authentifizierung** zusammen mit dem Namen und Kennwort Ihres Kerberos-Prinzipals, um eine Verbindung mit der HDFS-Datenquelle herzustellen. Informationen zu den Konfigurationsdetails finden Sie im Abschnitt [Eigenschaften des mit HDFS verknüpften Diensts](#hdfs-linked-service-properties).

### <a name="a-namekerberos-mutual-trustaoption-2-enable-mutual-trust-between-windows-domain-and-kerberos-realm"></a><a name="kerberos-mutual-trust"></a>Option 2: Aktivieren der gegenseitigen Vertrauensstellung zwischen der Windows-Domäne und dem Kerberos-Bereich

#### <a name="requirement"></a>Anforderung:
*   Der Gatewaycomputer muss einer Windows-Domäne beitreten.
*   Sie benötigen die Berechtigung zum Aktualisieren der Einstellungen des Domänencontrollers.

#### <a name="how-to-configure"></a>Konfiguration:

> [!NOTE]
> Ersetzen Sie im unten stehenden Tutorial die Zeichenfolgen REALM.COM und AD.COM durch Ihren eigenen Bereich und Domänencontroller.

**Auf dem KDC-Server:**

1.  Bearbeiten Sie die KDC-Konfiguration in der Datei **krb5.conf** mithilfe der unten stehenden Konfigurationsvorlage so, dass KDC der Windows-Domäne vertraut. Standardmäßig befindet sich die Konfiguration unter **/etc/krb5.conf**.

            [logging]
             default = FILE:/var/log/krb5libs.log
             kdc = FILE:/var/log/krb5kdc.log
             admin_server = FILE:/var/log/kadmind.log

            [libdefaults]
             default_realm = REALM.COM
             dns_lookup_realm = false
             dns_lookup_kdc = false
             ticket_lifetime = 24h
             renew_lifetime = 7d
             forwardable = true

            [realms]
             REALM.COM = {
              kdc = node.REALM.COM
              admin_server = node.REALM.COM
             }
            AD.COM = {
             kdc = windc.ad.com
             admin_server = windc.ad.com
            }

            [domain_realm]
             .REALM.COM = REALM.COM
             REALM.COM = REALM.COM
             .ad.com = AD.COM
             ad.com = AD.COM

            [capaths]
             AD.COM = {
              REALM.COM = .
             }

        **Restart** the KDC service after configuration.

2.  Bereiten Sie mit dem folgenden Befehl einen Prinzipal namens **krbtgt/REALM.COM@AD.COM** auf dem KDC-Server vor:

            Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3.  Fügen Sie in der HDFS-Dienstkonfigurationsdatei **hadoop.security.auth_to_local** diese Zeichenfolge hinzu: `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.

**Auf dem Domänencontroller:**

1.  Führen Sie die folgenden **Ksetup**-Befehle aus, um einen Bereichseintrag hinzuzufügen:

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  Richten Sie eine Vertrauensstellung zwischen der Windows-Domäne und dem Kerberos-Bereich ein. [password] ist das Kennwort für den Prinzipal **krbtgt/REALM.COM@AD.COM**.

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  Wählen Sie den in Kerberos verwendeten Verschlüsselungsalgorithmus aus.

    1. Wechseln Sie zu „Server-Manager > Gruppenrichtlinienverwaltung > Domäne > Gruppenrichtlinienobjekte > Standard- oder aktive Domänenrichtlinie“, und wählen Sie „Bearbeiten“.

    2. Wechseln Sie im Popupfenster **Gruppenrichtlinienverwaltungs-Editor** zu „Computerkonfiguration > Richtlinien > Windows-Einstellungen > Sicherheitseinstellungen > Lokale Richtlinien > Sicherheitsoptionen“, und konfigurieren Sie die Einstellung **Netzwerksicherheit: Für Kerberos zulässige Verschlüsselungstypen konfigurieren**.

    3. Wählen Sie den Verschlüsselungsalgorithmus, der beim Herstellen der Verbindung mit KDC verwendet werden soll. Im Allgemeinen können Sie einfach alle Optionen auswählen.

        ![Konfigurieren von Verschlüsselungstypen für Kerberos](media/data-factory-hdfs-connector/config-encryption-types-for-kerberos.png)

    4. Verwenden Sie den Befehl **Ksetup**, um den Verschlüsselungsalgorithmus anzugeben, der in dem bestimmten Bereich verwendet werden soll.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  Erstellen Sie die Zuordnung zwischen dem Domänenkonto und dem Kerberos-Prinzipal, um den Kerberos-Prinzipal in der Windows-Domäne verwenden zu können.

    1. Starten Sie „Verwaltung > **Active Directory-Benutzer und -Computer**“.

    2. Konfigurieren Sie erweiterte Funktionen, indem Sie auf **Ansicht** > **Erweiterte Funktionen** klicken.

    3. Suchen Sie das Konto, für das Sie Zuordnungen erstellen möchten, und klicken Sie mit der rechten Maustaste, um **Namenszuordnungen** anzuzeigen. Klicken Sie dann auf die Registerkarte **Kerberos-Namen**.

    4. Fügen Sie einen Prinzipal aus dem Bereich hinzu.

        ![Sicherheitsidentität zuordnen](media/data-factory-hdfs-connector/map-security-identity.png)

**Auf dem Gatewaycomputer:**

* Führen Sie die folgenden **Ksetup**-Befehle aus, um einen Bereichseintrag hinzuzufügen.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**In Azure Data Factory:**

* Konfigurieren Sie den HDFS-Connector mithilfe der **Windows-Authentifizierung** zusammen mit Ihrem Domänenkonto oder Ihrem Kerberos-Prinzipal, um eine Verbindung mit der HDFS-Datenquelle herzustellen. Informationen zu den Konfigurationsdetails finden Sie im Abschnitt [Eigenschaften des mit HDFS verknüpften Diensts](#hdfs-linked-service-properties).


## <a name="hdfs-dataset-type-properties"></a>Eigenschaften des Dataset-Typs „HDFS“
Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ des JSON-Codes eines Datasets sind bei allen Dataset-Typen (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt **typeProperties** unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt „typeProperties“ für ein Dataset vom Typ **FileShare** (mit HDFS-Dataset) hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| folderPath |Pfad zum Ordner. Beispiel: `myfolder`<br/><br/>Verwenden Sie für Sonderzeichen in der Zeichenfolge das Escapezeichen „\“. Geben Sie beispielsweise für „Ordner\Unterordner“ die Zeichenfolge „Ordner\\\\Unterordner“ und für „d:\Beispielordner“ die Zeichenfolge „d:\\\\Beispielordner“ an.<br/><br/>Sie können diese Eigenschaft mit **partitionBy** kombinieren, um Ordnerpfade auf der Grundlage von Datum und Uhrzeit für Start und Ende des Slices zu erhalten. |Ja |
| fileName |Geben Sie den Namen der Datei in **folderPath** an, wenn die Tabelle auf eine bestimmte Datei im Ordner verweisen soll. Wenn Sie keine Werte für diese Eigenschaft angeben, verweist die Tabelle auf alle Dateien im Ordner.<br/><br/>Wenn „fileName“ für ein Ausgabedataset nicht angegeben ist, hat der Name der generierten Datei folgendes Format: <br/><br/>Data.<Guid>.txt (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Nein |
| partitionedBy |Mit „partitionedBy“ kann für Zeitreihendaten ein dynamischer Wert für „folderPath“ und „filename“ angegeben werden. Beispiel: Parametrisierung von „folderPath“ für Daten nach Stunde. |Nein |
| format | Die folgenden Formattypen werden unterstützt: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Sie müssen die **type** -Eigenschaft unter „format“ auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Textformat](#specifying-textformat), [JSON-Format](#specifying-jsonformat), [Avro-Format](#specifying-avroformat), [Orc-Format](#specifying-orcformat) und [Parquet-Format](#specifying-parquetformat). <br><br> Wenn Sie **Dateien unverändert zwischen dateibasierten Speichern kopieren** möchten (binäre Kopie), können Sie den Formatabschnitt bei den Definitionen von Eingabe- und Ausgabedatasets überspringen. |Nein |
| Komprimierung | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Folgende Typen werden unterstützt: **GZip**, **Deflate**, **BZip2** und **ZipDeflate**. Folgende Komprimierungsstufen werden unterstützt: **Optimal** und **Schnellste**. Weitere Informationen finden Sie im Abschnitt [Angeben der Komprimierung](#specifying-compression). |Nein |

> [!NOTE]
> "filename" und "fileFilter" können nicht gleichzeitig verwendet werden.
>
>

### <a name="using-partionedby-property"></a>Verwenden der partionedBy-Eigenschaft
Wie im vorherigen Abschnitt erwähnt, kann „partitionedBy“ verwendet werden, um für Zeitreihendaten einen dynamischen Wert für „folderPath“ und „filename“ anzugeben. Sie können dazu die Data Factory-Makros und Systemvariablen "SliceStart" und "SliceEnd" verwenden, die den logischen Zeitraum für einen bestimmten Datenslice angeben.

In den Artikeln [Erstellen von Datasets](data-factory-create-datasets.md), [Planung und Ausführung](data-factory-scheduling-and-execution.md) und [Erstellen von Pipelines](data-factory-create-pipelines.md) finden Sie weitere Details zu Zeitreihen-Datasets, Planung und Slices.

#### <a name="sample-1"></a>Beispiel 1:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
Im obigen Beispiel wird „{Slice}“ durch den Wert der Data Factory-Systemvariablen „SliceStart“ ersetzt, die im Format „JJJJMMTTHH“ angegeben ist. "SliceStart" bezieht sich auf die Startzeit des Slices. "folderPath" unterscheidet sich für jeden Slice. Beispiel: "wikidatagateway/wikisampledataout/2014100103" oder "wikidatagateway/wikisampledataout/2014100104".

#### <a name="sample-2"></a>Beispiel 2:

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
Im Beispiel oben werden Jahr, Monat, Tag und Uhrzeit von SliceStart in separate Variablen extrahiert, die von den Eigenschaften „folderPath“ und „fileName“ verwendet werden.

[!INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]

[!INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="hdfs-copy-activity-type-properties"></a>Eigenschaften von HDFS-Kopieraktivitätstyp
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinien sind für alle Arten von Aktivitäten verfügbar.

Eigenschaften im Abschnitt „typeProperties“ der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

Wenn die Quelle der Kopieraktivität den Typ **FileSystemSource** hat, sind im Abschnitt „typeProperties“ die folgenden Eigenschaften verfügbar:

**FileSystemSource** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| recursive |Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. |True/False (Standardwert) |Nein |

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.



<!--HONumber=Jan17_HO2-->


