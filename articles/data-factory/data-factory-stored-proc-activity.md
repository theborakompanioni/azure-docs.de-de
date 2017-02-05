---
title: "SQL Server-Aktivität &quot;Gespeicherte Prozedur&quot;"
description: "Informationen, wie Sie die SQL Server-Aktivität &quot;Gespeicherte Prozedur&quot; in einer Data Factory-Pipeline zum Aufrufen einer gespeicherten Prozedur in einer Azure SQL-Datenbank oder einem Azure SQL Data Warehouse verwenden können."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 1b2514e1e6f39bb3ce9d8a46f4af01835284cdcc
ms.openlocfilehash: 3510b0446cf3c1a7ffb3ff02a4d84d24ead1cae7


---
# <a name="sql-server-stored-procedure-activity"></a>SQL Server-Aktivität "Gespeicherte Prozedur"
> [!div class="op_single_selector"]
> [Hive](data-factory-hive-activity.md)  
> [Pig](data-factory-pig-activity.md)  
> [MapReduce](data-factory-map-reduce.md)  
> [Hadoop-Datenströme](data-factory-hadoop-streaming-activity.md)
> [Machine Learning](data-factory-azure-ml-batch-execution-activity.md)
> [Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
> [Data Lake Analytics U-SQL](data-factory-usql-activity.md)
> [Benutzerdefinierte .NET-Aktivität](data-factory-use-custom-activities.md)
>
>

Sie können die SQL Server-Aktivität „Gespeicherte Prozedur“ in einer Data Factory-[Pipeline](data-factory-create-pipelines.md) verwenden, um eine gespeicherte Prozedur in einem der folgenden Datenspeicher aufzurufen:

* Azure SQL-Datenbank
* Azure SQL Data Warehouse  
* SQL Server-Datenbank in Ihrem Unternehmen oder auf einem virtuellen Azure-Computer Sie müssen das Datenverwaltungsgateway auf dem Computer installieren, der die Datenbank hostet, oder auf einem separaten Computer, um zu vermeiden, dass der Computer mit der Datenbank um Ressourcen konkurriert. Das Datenverwaltungsgateway ist eine Software, die lokale Datenquellen oder Datenquellen auf virtuellen Azure-Computern auf sichere, verwaltete Weise mit Cloud-Diensten verbindet. Weitere Informationen zum Datenverwaltungsgateway finden Sie im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) .

Dieser Artikel baut auf dem Artikel zu [Datentransformationsaktivitäten](data-factory-data-transformation-activities.md) auf, der eine allgemeine Übersicht über die Datentransformation und die unterstützten Transformationsaktivitäten bietet.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise
### <a name="sample-table-and-stored-procedure"></a>Beispiel für eine Tabelle und eine gespeicherte Prozedur
1. Erstellen Sie die folgende **Tabelle** in der Azure SQL-Datenbank mithilfe von SQL Server Management Studio oder anderen Tools, mit denen Sie vertraut sind. Die Spalte "datetimestamp" enthält das Datum und die Uhrzeit, zu der die entsprechende ID generiert wird.

        CREATE TABLE dbo.sampletable
        (
            Id uniqueidentifier,
            datetimestamp nvarchar(127)
        )
        GO

        CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
        GO

    "Id" ist der eindeutige Bezeichner, und die Spalte "datetimestamp" enthält das Datum und die Uhrzeit, zu der die entsprechende ID generiert wird.
    ![Beispieldaten](./media/data-factory-stored-proc-activity/sample-data.png)

   > [!NOTE]
   > In diesem Beispiel wird eine Azure SQL-Datenbank verwendet, es funktioniert für ein Azure SQL Data Warehouse und eine SQL Server-Datenbank aber auf gleiche Weise.
   >
   >
2. Erstellen Sie die folgende **gespeicherte Prozedur**, die Daten in **sampletable** einfügt.

        CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
        AS

        BEGIN
            INSERT INTO [sampletable]
            VALUES (newid(), @DateTime)
        END

   > [!IMPORTANT]
   > **Name** und **Groß-/Kleinschreibung** des Parameters („DateTime“ in diesem Beispiel) müssen mit dem Parameter übereinstimmen, der im JSON-Code der Pipeline/Aktivität angegeben ist. Stellen Sie bei der Definition der gespeicherten Prozedur sicher, dass **@** als Präfix für den Parameter verwendet wird.
   >
   >

### <a name="create-a-data-factory"></a>Erstellen einer Data Factory
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie im Menü auf der linken Seite auf **NEU**, und klicken Sie auf **Intelligence + Analyse** und dann auf **Data Factory**.

    ![Neue Data Factory](media/data-factory-stored-proc-activity/new-data-factory.png)    
3. Geben Sie auf dem Blatt **Neue Data Factory** unter „Name“ die Zeichenfolge **SProcDF** ein. Azure Data Factory-Namen sind **global eindeutig**. Sie müssen dem Namen der Data Factory Ihren Namen voranstellen, damit die Factory erfolgreich erstellt werden kann.

   ![Neue Data Factory](media/data-factory-stored-proc-activity/new-data-factory-blade.png)         
4. Wählen Sie Ihr Azure-**Abonnement** aus.
5. Führen Sie unter **Ressourcengruppe** einen der folgenden Schritte aus:
   1. Klicken Sie auf **Neu erstellen**, und geben Sie einen Namen für die Ressourcengruppe ein.
   2. Wählen Sie **Vorhandene verwenden**, um eine vorhandene Ressourcengruppe auszuwählen.  
6. Wählen Sie den **Standort** für die Data Factory aus.
7. Wählen Sie **An Dashboard anheften**, damit Sie die Data Factory auf dem Dashboard sehen, wenn Sie sich das nächste Mal anmelden.
8. Klicken Sie auf dem Blatt **Neue Data Factory** auf **Erstellen**.
9. Im **Dashboard** des Azure-Portals sehen Sie, dass die Data Factory erstellt wird. Nachdem die Data Factory erfolgreich erstellt wurde, sehen Sie die Data Factory-Seite mit dem Inhalt der Data Factory.
   ![Data Factory-Startseite](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Erstellen eines mit Azure SQL verknüpften Diensts
Nach dem Erstellen der Data Factory erstellen Sie einen mit Azure SQL verknüpften Dienst, der Ihre Azure SQL-Datenbank mit der Data Factory verknüpft. Diese Datenbank enthält die Beispieltabelle „sampletable“ und die gespeicherte Prozedur „sp_sample“.

1. Klicken Sie auf dem Blatt **Data Factory** für **SProcDF** auf **Verfassen und bereitstellen**, um den Data Factory-Editor zu starten.
2. Klicken Sie in der Befehlsleiste auf **Neuer Datenspeicher**, und wählen Sie **Azure SQL-Datenbank**. Das JSON-Skript zum Erstellen eines mit Azure SQL verknüpften Diensts wird im Editor angezeigt.

   ![Neuer Datenspeicher](media/data-factory-stored-proc-activity/new-data-store.png)
3. Nehmen Sie die folgenden Änderungen am JSON-Skript vor:

   1. Ersetzen Sie **&lt;servername&gt;** mit dem Namen des Azure SQL-Datenbank-Servers.
   2. Ersetzen Sie **&lt;databasename&gt;** mit der Datenbank, in der Sie die Tabelle und die gespeicherte Prozedur erstellt haben.
   3. Ersetzen Sie **&lt;username@servername&gt;** mit dem Benutzerkonto, das über Zugriff auf die Datenbank verfügt.
   4. Ersetzen Sie **&lt;password&gt;** mit dem Kennwort für das Benutzerkonto.

      ![Neuer Datenspeicher](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. Klicken Sie auf der Befehlsleiste auf **Bereitstellen** , um den verknüpften Dienst bereitzustellen. Vergewissern Sie sich, dass AzureSqlLinkedService in der Strukturansicht links angezeigt wird.

    ![Strukturansicht mit verknüpften Diensten](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Erstellen eines Ausgabedatasets
1. Klicken Sie in der Symbolleiste auf **... Weitere**, klicken Sie auf **Neues Dataset**, und klicken Sie auf **Azure SQL**. **Neues Dataset**, und wählen Sie **Azure SQL**.

    ![Strukturansicht mit verknüpften Diensten](media/data-factory-stored-proc-activity/new-dataset.png)
2. Kopieren Sie das folgende JSON-Skript, und fügen Sie es in den JSON-Editor ein.

        {                
            "name": "sprocsampleout",
            "properties": {
                "type": "AzureSqlTable",
                "linkedServiceName": "AzureSqlLinkedService",
                "typeProperties": {
                    "tableName": "sampletable"
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }
3. Klicken Sie in der Befehlsleiste auf **Bereitstellen** , um das Dataset bereitzustellen. Vergewissern Sie sich, dass das Dataset in der Strukturansicht angezeigt wird.

    ![Strukturansicht mit verknüpften Diensten](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Erstellen einer Pipeline mit der Aktivität "SqlServerStoredProcedure"
Erstellen wir jetzt eine Pipeline mit der Aktivität "SqlServerStoredProcedure".

1. Klicken Sie in der Symbolleiste auf **... Weitere** und dann auf **Neue Pipeline**.
2. Kopieren Sie folgenden JSON-Codeausschnitt, und fügen Sie ihn ein. Der **storedProcedureName** wird auf **sp_sample** festgelegt. Name und Schreibweise des Parameters **DateTime** muss mit dem Namen und der Schreibweise des Parameters in der Definition der gespeicherten Prozedur übereinstimmen.  

        {
            "name": "SprocActivitySamplePipeline",
            "properties": {
                "activities": [
                    {
                        "type": "SqlServerStoredProcedure",
                        "typeProperties": {
                            "storedProcedureName": "sp_sample",
                            "storedProcedureParameters": {
                                "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                            }
                        },
                        "outputs": [
                            {
                                "name": "sprocsampleout"
                            }
                        ],
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "SprocActivitySample"
                    }
                ],
                 "start": "2016-08-02T00:00:00Z",
                 "end": "2016-08-02T05:00:00Z",
                "isPaused": false
            }
        }

    Wenn Sie für einen Parameter Null übergeben müssen, verwenden Sie die folgende Syntax: "param1": null (nur Kleinbuchstaben).
3. Klicken Sie in der Symbolleiste auf **Bereitstellen** , um die Pipeline bereitzustellen.  

### <a name="monitor-the-pipeline"></a>Überwachen der Pipeline
1. Klicken Sie auf **X**, um die Data Factory-Editor-Blätter zu schließen und zum Blatt „Data Factory“ zurückzukehren, und klicken Sie auf **Diagramm**.

    ![Kachel „Diagramm“](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
2. In der **Diagrammansicht**sehen Sie eine Übersicht über die in diesem Tutorial verwendeten Pipelines und Datasets.

    ![Kachel „Diagramm“](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
3. Doppelklicken Sie in der Diagrammansicht auf das Dataset **sprocsampleout**. Die Slices werden im Zustand „Bereit“ angezeigt. Es sollten fünf Slices vorhanden sein, da ein Slice für jede Stunde zwischen der Startzeit und Endzeit aus der JSON erstellt wird.

    ![Kachel „Diagramm“](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. Wenn sich ein Slice im Zustand **Bereit** befindet, führen Sie eine Abfrage **select * from sampledata** für die Azure SQL-Datenbank aus, um sicherzustellen, dass die Daten von der gespeicherten Prozedur in die Tabelle eingefügt wurden.

   ![Ausgabedaten](./media/data-factory-stored-proc-activity/output.png)

   Unter [Überwachen der Pipeline](data-factory-monitor-manage-pipelines.md) finden Sie ausführliche Informationen zur Überwachung von Azure Data Factory-Pipelines.  

> [!NOTE]
> In diesem Beispiel weist SprocActivitySample keine Eingaben auf. Wenn Sie diese Aktivität mit einer Upstreamaktivität verketten möchten (d.h. vor der Verarbeitung), können die Ausgaben der Upstreamaktivität in dieser Aktivität als Eingaben verwendet werden. In diesem Fall wird die Aktivität erst ausgeführt, nachdem die Upstreamaktivität abgeschlossen wurde und ihre Ausgaben verfügbar sind (im Status „Bereit“). Die Eingaben können nicht direkt als Parameter für die Aktivität „Gespeicherte Prozedur“ verwendet werden.
>
>

## <a name="json-format"></a>JSON-Format
    {
        "name": "SQLSPROCActivity",
        "description": "description",
        "type": "SqlServerStoredProcedure",
        "inputs":  [ { "name": "inputtable"  } ],
        "outputs":  [ { "name": "outputtable" } ],
        "typeProperties":
        {
            "storedProcedureName": "<name of the stored procedure>",
            "storedProcedureParameters":  
            {
                "param1": "param1Value"
                …
            }
        }
    }

## <a name="json-properties"></a>JSON-Eigenschaften
| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| Name |Der Name der Aktivität |Ja |
| Beschreibung |Ein Text, der beschreibt, wofür die Aktivität verwendet wird. |Nein |
| Typ |SqlServerStoredProcedure |Ja |
| inputs |Optional. Wenn Sie ein Eingabedataset angeben, muss es (im Status „Bereit“) verfügbar sein, damit die Aktivität „Gespeicherte Prozedur“ ausgeführt wird. Das Eingabedataset kann nicht als Parameter in der gespeicherten Prozedur genutzt werden. Es wird nur verwendet, um vor dem Start der Aktivität „Gespeicherte Prozedur“ die Abhängigkeit zu überprüfen. |Nein |
| outputs |Sie müssen ein Ausgabedataset für eine Aktivität „Gespeicherte Prozedur“ angeben. Das Ausgabedataset gibt den **Zeitplan** für die Aktivität „Gespeicherte Prozedur“ an (stündlich, wöchentlich, monatlich usw.). <br/><br/>Das Ausgabedataset muss einen **verknüpften Dienst** verwenden, der auf eine Azure SQL-Datenbank, ein Azure SQL Data Warehouse oder eine SQL Server-Datenbank verweist, in der bzw. dem die gespeicherte Prozedur ausgeführt werden soll. <br/><br/>Das Ausgabedataset kann verwendet werden, um das Ergebnis der gespeicherten Prozedur für die nachfolgende Verarbeitung durch eine andere Aktivität in der Pipeline zu übergeben ([Verketten von Aktivitäten](data-factory-scheduling-and-execution.md#run-activities-in-a-sequence)). Data Factory schreibt die Ausgabe einer gespeicherten Prozedur jedoch nicht automatisch in dieses Dataset. Die gespeicherte Prozedur schreibt die Ausgabe in eine SQL-Tabelle, auf die das Ausgabedataset verweist. <br/><br/>In einigen Fällen kann das Ausgabedataset ein **Dummy-Dataset** sein, das nur dazu dient, den Zeitplan für die Ausführung der Aktivität „Gespeicherte Prozedur“ anzugeben. |Ja |
| storedProcedureName |Geben Sie den Namen der gespeicherten Prozedur in der Azure SQL-Datenbank oder dem Azure SQL Data Warehouse an, die bzw. das vom verknüpften Dienst dargestellt wird, den die Ausgabetabelle verwendet. |Ja |
| storedProcedureParameters |Geben Sie Werte für Parameter der gespeicherten Prozedur an. Wenn Sie für einen Parameter Null übergeben müssen, verwenden Sie die folgende Syntax: "param1": null (nur Kleinbuchstaben). Das folgende Beispiel veranschaulicht die Verwendung dieser Eigenschaft. |Nein |

## <a name="passing-a-static-value"></a>Übergeben eines statischen Werts
Lassen Sie uns nun der Tabelle eine weitere Spalte mit dem Namen "Scenario" hinzufügen, die den statischen Wert "Document sample" enthält.

![Beispieldaten 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)

    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime, @Scenario)
    END

Hierzu übergeben Sie den Parameter „Scenario“ und den Wert aus der Aktivität „Gespeicherte Prozedur“. Der Abschnitt typeProperties im obigen Beispiel sieht wie der folgende Codeausschnitt aus:

    "typeProperties":
    {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters":
        {
            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
            "Scenario": "Document sample"
        }
    }



<!--HONumber=Nov16_HO3-->


