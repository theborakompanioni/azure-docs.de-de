---
title: Verschieben von Daten aus Cassandra mit Data Factory | Microsoft Docs
description: Enthält Informationen zum Verschieben von Daten aus einer lokalen Cassandra-Datenbank mit Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: jhubbard
editor: monicar

ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: jingwang

---
# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Verschieben von Daten aus einer lokalen Cassandra-Datenbank mit Azure Data Factory
In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in einer Azure Data Factory verwenden, um Daten aus einer lokalen Cassandra-Datenbank in einen Datenspeicher zu kopieren, der im Abschnitt [Unterstützte Quellen und Senken](data-factory-data-movement-activities.md#supported-data-stores) in der Spalte „Senke“ aufgeführt ist. Dieser Artikel baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht über die Datenverschiebung mit der Kopieraktivität sowie über unterstützte Datenspeicherkombinationen enthält.

Data Factory unterstützt derzeit nur das Verschieben von Daten aus einer Cassandra-Datenbank in [unterstützte Senkendatenspeicher](data-factory-data-movement-activities.md#supported-data-stores), aber nicht das Verschieben von Daten aus anderen Datenspeichern in eine Cassandra-Datenbank.

## <a name="prerequisites"></a>Voraussetzungen
Damit der Azure Data Factory-Dienst eine Verbindung mit Ihrer lokalen Cassandra-Datenbank herstellen kann, muss Folgendes installiert werden: 

* Datenverwaltungsgateway 2.0 oder höher auf dem Computer, der die Datenbank hostet, oder auf einem separaten Computer, um zu vermeiden, dass der Computer mit der Datenbank um Ressourcen konkurriert. Datenverwaltungsgateway ist eine Software, die lokale Datenquellen mit Cloud-Diensten auf sichere, verwaltete Weise verbindet. Weitere Informationen zum Datenverwaltungsgateway finden Sie im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) .
  
    Beim Installieren des Gateways wird automatisch ein Microsoft Cassandra-ODBC-Treiber installiert, der zum Herstellen einer Verbindung mit der Cassandra-Datenbank verwendet wird. 

> [!NOTE]
> Unter [Problembehandlung bei Gateways](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) finden Sie Tipps zur Behandlung von Verbindungs- bzw. Gatewayproblemen. 
> 
> 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Art, eine Pipeline zu erstellen, mit der Daten aus einer Cassandra-Datenbank in einen der unterstützten Senkendatenspeicher kopiert werden, ist die Verwendung des Assistenten zum Kopieren von Daten. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten. 

Das folgende Beispiel stellt JSON-Beispieldefinitionen bereit, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder mit [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Darin wird veranschaulicht, wie Sie Daten aus einer Cassandra-Datenbank in Azure Blob Storage kopieren. Daten können jedoch auch mithilfe der Kopieraktivität in Azure Data Factory in eine beliebige der [hier](data-factory-data-movement-activities.md#supported-data-stores) aufgeführten Senken kopiert werden.   

## <a name="sample:-copy-data-from-cassandra-to-blob"></a>Beispiel: Kopieren von Daten aus Cassandra in ein Blob
Im Beispiel werden stündlich Daten aus einer Cassandra-Datenbank in ein Azure-Blob kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen. Daten können mithilfe der Kopieraktivität in Azure Data Factory direkt in die im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md#supported-data-stores) aufgeführten Senken kopiert werden. 

* Einen verknüpften Dienst des Typs [OnPremisesCassandra](#onpremisescassandra-linked-service-properties)
* Einen verknüpften Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)
* Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [CassandraTable](#cassandratable-properties)
* Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties)
* Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [CassandraSource](#cassandrasource-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) verwendet

**Verknüpfter Cassandra-Dienst**

In diesem Beispiel wird der verknüpfte **Cassandra** -Dienst verwendet. Der Abschnitt [Verknüpfter Cassandra-Dienst](#onpremisescassandra-linked-service-properties) enthält eine Liste mit den Eigenschaften, die von diesem verknüpften Dienst unterstützt werden.  

    {
        "name": "CassandraLinkedService",
        "properties":
        {
            "type": "OnPremisesCassandra",
            "typeProperties":
            {
                "authenticationType": "Basic",
                "host": "mycassandraserver",
                "port": 9042,
                "username": "user",
                "password": "password",
                "gatewayName": "mygateway"
            }
        }
    }


**Mit Azure Storage verknüpfter Dienst**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
        "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }

**Cassandra-Eingabedataset**

    {
        "name": "CassandraInput",
        "properties": {
            "linkedServiceName": "CassandraLinkedService",
            "type": "CassandraTable",
            "typeProperties": {
                "tableName": "mytable",
                "keySpace": "mykeyspace" 
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

Durch Festlegen von **external** auf **true** wird dem Data Factory-Dienst mitgeteilt, dass das Dataset für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

**Azure-Blob-Ausgabedataset**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). 

    {
        "name": "AzureBlobOutput",
        "properties":
        {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties":
            {
                "folderPath": "adfgetstarted/fromcassandra"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Pipeline mit Kopieraktivität**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **CassandraSource** und der Typ **sink** auf **BlobSink** festgelegt. 

Unter [RelationalSource-Typeigenschaften](#cassandrasource-type-properties) finden Sie die Liste der Eigenschaften, die von RelationalSource unterstützt werden. 

    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2016-06-01T18:00:00",
            "end":"2016-06-01T19:00:00",
            "description":"pipeline with copy activity",
            "activities":[  
            {
                "name": "CassandraToAzureBlob",
                "description": "Copy from Cassandra to an Azure blob",
                "type": "Copy",
                "inputs": [
                {
                    "name": "CassandraInput"
                }
                ],
                "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "CassandraSource",
                        "query": "select id, firstname, lastname from mykeyspace.mytable"

                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
            ]   
        }
    }
## <a name="onpremisescassandra-linked-service-properties"></a>Eigenschaften des verknüpften OnPremisesCassandra-Diensts
Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die speziell für den verknüpften Cassandra-Dienst gelten.

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| type |Die type-Eigenschaft muss auf **OnPremisesCassandra** |Ja |
| host |Mindestens eine IP-Adresse oder ein Hostname von Cassandra-Servern.<br/><br/>Geben Sie eine durch Trennzeichen getrennte Liste mit IP-Adressen oder Hostnamen an, um gleichzeitig mit allen Servern Verbindungen herzustellen. |Ja |
| port |Der TCP-Port, den der Cassandra-Server verwendet, um auf Clientverbindungen zu lauschen. |Nein. Standardwert: 9042 |
| authenticationType |Basic (Standard) oder Anonymous (Anonym) |Ja |
| username |Geben Sie einen Benutzernamen für das Benutzerkonto an. |Ja, wenn authenticationType auf „Basic“ (Standard) festgelegt ist. |
| password |Geben Sie ein Kennwort für das Benutzerkonto an. |Ja, wenn authenticationType auf „Basic“ (Standard) festgelegt ist. |
| gatewayName |Der Name des Gateways, das zum Herstellen der Verbindung mit der lokalen Cassandra-Datenbank verwendet wird. |Ja |
| encryptedCredential |Anmeldeinformationen, die vom Gateway verschlüsselt werden. |Nein |

## <a name="cassandratable-properties"></a>CassandraTable-Eigenschaften
Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ des JSON-Codes eines Datasets sind bei allen Dataset-Typen (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Der Abschnitt **typeProperties** unterscheidet sich bei jedem Typ von Dataset und bietet Informationen zum Speicherort der Daten im Datenspeicher. Der Abschnitt „typeProperties“ für ein Dataset des Typs **CassandraTable** hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| keyspace |Name des Keyspace oder Schemas in der Cassandra-Datenbank. |Ja (wenn **query** für **CassandraSource** nicht definiert ist). |
| tableName |Name der Tabelle in der Cassandra-Datenbank. |Ja (wenn **query** für **CassandraSource** nicht definiert ist). |

## <a name="cassandrasource-type-properties"></a>Eigenschaften des Typs CassandraSource
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinie sind für alle Arten von Aktivitäten verfügbar. 

Eigenschaften im Abschnitt „typeProperties“ der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken.

Bei einer Quelle des Typs **SqlDWSource**sind im Abschnitt „typeProperties“ folgende Eigenschaften verfügbar:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| query |Verwendet die benutzerdefinierte Abfrage zum Lesen von Daten. |SQL-92-Abfrage oder CQL-Abfrage. Weitere Informationen finden Sie in der [Referenz zu CQL](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Geben Sie beim Verwenden der SQL-Abfrage **keyspace name.table name** für die Tabelle an, die Sie abfragen möchten. |Nein (wenn tableName und keyspace im Dataset definiert sind) |
| consistencyLevel |Mit der Konsistenzebene (consistencyLevel) wird angegeben, wie viele Replikate auf eine Leseanforderung reagieren müssen, bevor Daten an die Clientanwendung zurückgegeben werden. Cassandra überprüft die angegebene Anzahl von Replikaten auf Daten, um die Leseanforderung zu erfüllen. |ONE, TWO, THREE, QUORUM, ALL, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE. Ausführliche Informationen finden Sie unter [Configuring data consistency](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) (Konfigurieren der Datenkonsistenz). |Nein. Der Standardwert ist ONE. |

### <a name="type-mapping-for-cassandra"></a>Typzuordnung für Cassandra
| Cassandra-Typ | .NET-basierter Typ |
| --- | --- |
| ASCII |String |
| BIGINT |Int64 |
| BLOB |Byte[] |
| BOOLEAN |BOOLEAN |
| DECIMAL |DECIMAL |
| DOUBLE |DOUBLE |
| FLOAT |Single |
| INET |String |
| INT |Int32 |
| TEXT |String |
| TIMESTAMP |DateTime |
| TIMEUUID |GUID |
| UUID |GUID |
| VARCHAR |String |
| VARINT |DECIMAL |

> [!NOTE]
> Informationen zu Sammlungstypen (map, set, list usw.) finden Sie im Abschnitt [Verwenden von Cassandra-Sammlungstypen mit einer virtuellen Tabelle](#work-with-collections-using-virtual-table) . 
> 
> Benutzerdefinierte Typen werden nicht unterstützt.
> 
> Die Länge von „Binary Column“ und „String Column“ darf 4.000 nicht übersteigen. 
> 
> 

## <a name="work-with-collections-using-virtual-table"></a>Verwenden von Cassandra-Sammlungstypen mit einer virtuellen Tabelle
Azure Data Factory verwendet einen integrierten ODBC-Treiber, um eine Verbindung mit der Cassandra-Datenbank herzustellen und Daten daraus zu kopieren. Für Sammlungstypen, z.B. „map“, „set“ und „list“, normalisiert der Treiber die Daten erneut in die entsprechenden virtuellen Tabellen. Wenn eine Tabelle Sammlungsspalten enthält, generiert der Treiber die folgenden virtuellen Tabellen:

* Eine **Basistabelle**, die die gleichen Daten wie die tatsächliche Tabelle enthält, mit Ausnahme der Sammlungsspalten. Für die Basistabelle wird der gleiche Name wie für die echte Tabelle verwendet, die sie repräsentiert.
* Eine **virtuelle Tabelle** für jede Sammlungsspalte (Erweiterung der geschachtelten Daten). Die virtuellen Tabellen, die Sammlungen repräsentieren, werden mit dem Namen der tatsächlichen Tabelle benannt und erhalten zusätzlich das Trennzeichen „*vt*“ und den Namen der Spalte.

Virtuelle Tabellen beziehen sich auf die Daten in der echten Tabelle, sodass der Treiber auf die denormalisierten Daten zugreifen kann. Details finden Sie unten im Abschnitt mit dem Beispiel. Sie können auf den Inhalt von Cassandra-Sammlungen zugreifen, indem Sie die virtuellen Tabellen abfragen und verknüpfen.

Sie können den [Kopier-Assistenten](data-factory-data-movement-activities.md#data-factory-copy-wizard) verwenden, um die Liste mit den Tabellen in der Cassandra-Datenbank anzuzeigen, einschließlich der virtuellen Tabellen. Sie können im Kopier-Assistenten auch eine Abfrage erstellen und eine Überprüfung durchführen, um das Ergebnis anzuzeigen.

### <a name="example"></a>Beispiel
Die folgende Beispieltabelle „ExampleTable“ ist beispielsweise eine Cassandra-Datenbanktabelle, die eine Spalte mit dem Namen „pk_int“ für ganzzahlige Primärschlüssel, eine Textspalte mit dem Namen „value“ und die Spalten „list“, „map“ und „set“ („StringSet“) enthält.

| pk_int | Wert | Auflisten | Map | StringSet |
| --- | --- | --- | --- | --- |
| 1 |"sample value 1" |["1", "2", "3"] |{"S1": "a", "S2": "b"} |{"A", "B", "C"} |
| 3 |"sample value 3" |["100", "101", "102", "105"] |{"S1": "t"} |{"A", "E"} |

Der Treiber erzeugt mehrere virtuelle Tabellen, um diese einzelne Tabelle zu repräsentieren. Die Fremdschlüsselspalten in den virtuellen Tabellen verweisen auf die Primärschlüsselspalten in der echten Tabelle und geben an, welcher Zeile der echten Tabelle die Zeile der virtuellen Tabelle entspricht. 

Die erste virtuelle Tabelle ist die unten dargestellte Basistabelle mit dem Namen „ExampleTable“. Die Basistabelle enthält die gleichen Daten wie die ursprüngliche Datenbanktabelle, mit Ausnahme der Sammlungen, die in dieser Tabelle fehlen und in anderen virtuellen Tabellen enthalten sind.

| pk_int | Wert |
| --- | --- |
| 1 |"sample value 1" |
| 3 |"sample value 3" |

Die folgenden Tabellen enthalten die virtuellen Tabellen, in denen die Daten aus den Spalten „List“, „Map“ und „StringSet“ erneut normalisiert werden. Die Spalten mit Namen, die auf „_index“ oder „_key“ enden, geben die Position der Daten in der Originalliste (list) bzw. -zuordnung (map) an. Die Spalten mit Namen, die auf „_value“ enden, enthalten die erweiterten Daten aus der Sammlung.

#### <a name="table-“exampletable_vt_list”:"></a>Tabelle „ExampleTable_vt_List“:
| pk_int | List_index | List_value |
| --- | --- | --- |
| 1 |0 |1 |
| 1 |1 |2 |
| 1 |2 |3 |
| 3 |0 |100 |
| 3 |1 |101 |
| 3 |2 |102 |
| 3 |3 |103 |

#### <a name="table-“exampletable_vt_map”:"></a>Tabelle „ExampleTable_vt_Map“:
| pk_int | Map_key | Map_value |
| --- | --- | --- |
| 1 |S1 |Eine |
| 1 |S2 |b |
| 3 |S1 |t |

#### <a name="table-“exampletable_vt_stringset”:"></a>Tabelle „ExampleTable_vt_StringSet“:
| pk_int | StringSet_value |
| --- | --- |
| 1 |Eine |
| 1 |b |
| 1 |C |
| 3 |Eine |
| 3 |E |

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Leistung und Optimierung
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

<!--HONumber=Oct16_HO2-->


