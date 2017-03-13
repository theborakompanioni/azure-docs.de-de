---
title: Importieren von Daten in Machine Learning Studio aus Onlinedatenquellen | Microsoft-Dokumentation
description: Informationen zum Importieren von Trainingsdaten aus verschiedenen Datenquellen in Azure Machine Learning Studio.
keywords: Importieren von Daten,Datenformat,Datentypen,Datenquellen,Trainingsdaten
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 701b93fe-765b-4d15-a1cf-9b607f17add6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: bradsev;garye
translationtype: Human Translation
ms.sourcegitcommit: a6bc79b2cb5b73109cddd6cf57caeba754b52e2e
ms.openlocfilehash: afecdde0cbc0bcbe0932b23dc1a8e067d02ded12
ms.lasthandoff: 12/20/2016


---
# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-the-import-data-module"></a>Importieren von Trainingsdaten aus verschiedenen Onlinedatenquellen mit dem „Import Data“-Modul in Azure Machine Learning Studio
In diesem Artikel werden die Unterstützung des Importierens von Onlinedaten aus verschiedenen Datenquellen und die erforderlichen Informationen zum Verschieben von Daten aus diesen Quellen in ein Azure Machine Learning-Experiment beschrieben.

> [!NOTE]
> Dieser Artikel bietet allgemeine Informationen zum Modul [Import Data][import-data]. Ausführlichere Informationen zu den Datentypen, auf die Sie zugreifen können, zu Formaten, Parametern und Antworten auf häufig gestellte Fragen finden Sie im Modulreferenzthema zum Modul [Import Data][import-data].
> 
> 

<!-- -->

[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a>Einführung
Mithilfe des Moduls [Import Data][import-data] können Sie auf Daten aus einer von mehreren Onlinedatenquellen zugreifen, während Ihr Experiment in [Azure Machine Learning Studio](https://studio.azureml.net/Home) ausgeführt wird:

* Web-URL über HTTP
* Hadoop über HiveQL
* Azure Blob Storage
* Azure-Tabelle
* Azure SQL-Datenbank oder SQL Server in Azure VM
* Lokale SQL Server-Datenbank
* Datenfeedanbieter (derzeit OData)

Für den Zugriff auf Onlinedatenquellen in Ihrem Studio-Experiment fügen Sie Ihrem Experiment das Modul [Import Data][import-data] hinzu, wählen **Data source** (Datenquelle) aus und geben dann die für den Zugriff auf die Daten erforderlichen Parameter an. In der folgenden Tabelle werden die unterstützten Onlinedatenquellen beschrieben. Diese Tabelle enthält auch die unterstützten Dateiformate und die Parameter für den Zugriff auf die Daten.

Hinweis: Da auf diese Trainingsdaten zugegriffen wird, während das Experiment läuft, stehen sie nur in diesem Experiment zur Verfügung. Im Vergleich dazu stehen Daten, die in Datasetmodulen gespeichert sind, allen Experimente in Ihrem Arbeitsbereich zur Verfügung.

> [!IMPORTANT]
> Derzeit können die Module [Import Data][import-data] und [Export Data][export-data] Daten nur in Azure-Speicher lesen und schreiben, der mit dem klassischen Bereitstellungsmodell erstellt wurde. Das heißt, dass der neue Azure Blob Storage-Kontotyp, der eine „heiße“ oder „kalte“ Speicherzugriffsebene bietet, noch nicht unterstützt wird. 
> 
> Im Allgemeinen sollten Azure-Speicherkonten, die Sie ggf. erstellt haben, bevor diese Dienstoption verfügbar war, nicht betroffen sein. 
> Wenn Sie ein neues Konto erstellen müssen, wählen Sie **Klassisch** als Bereitstellungsmodell, oder verwenden Sie Resource Manager, und wählen Sie für **Kontoart** die Option **Allgemein** anstelle von **Blobspeicher**. 
> 
> Weitere Informationen finden Sie unter [Azure Blob Storage: „Heiße“ und „kalte“ Speicherebenen](../storage/storage-blob-storage-tiers.md).
> 
> 

## <a name="supported-online-data-sources"></a>Unterstützte Onlinedatenquellen
Das **Import Data** -Modul von Azure Machine Learning unterstützt die folgenden Datenquellen:

| Data source | Beschreibung | Parameter |
| --- | --- | --- |
| Web URL via HTTP |Liest Daten im CSV-Format (mit Trennzeichen getrennte Werte), im TSV-Format (mit Tabulatoren getrennte Werte), im ARFF-Format (Attribute-Relation File Format) und SVM-light-Format (Support Vector Machines) aus allen Web-URLs, die HTTP verwenden. |<b>URL</b>: Gibt den vollständigen Namen der Datei (einschließlich Website-URL und Dateiname) mit beliebiger Erweiterung an. <br/><br/><b>Data format</b>: Gibt eines der unterstützten Formate an: CSV, TSV, ARFF oder SVM-light. Wenn die Daten eine Kopfzeile haben, wird sie für das Zuweisen von Spaltennamen verwendet. |
| Hadoop/HDFS |Liest Daten aus verteiltem Speicher in Hadoop. Sie geben die gewünschten Daten an, indem Sie HiveQL, eine SQL-ähnliche Abfragesprache, verwenden. HiveQL kann auch für die Aggregation von Daten und die Datenfilterung verwendet werden, ehe Sie die Daten Machine Learning Studio hinzufügen. |<b>Hive database query</b>: Gibt die Hive-Abfrage zum Generieren der Daten an.<br/><br/><b>HCatalog server URI</b>: Gibt den Namen Ihres Clusters im Format *&lt;Ihr Clustername&gt;.azurehdinsight.net* an.<br/><br/><b>Hadoop user account name</b>: Gibt den Hadoop-Benutzerkontonamen an, der zum Bereitstellen des Clusters verwendet wird.<br/><br/><b>Hadoop user account password</b>: Gibt die Anmeldeinformationen an, die beim Bereitstellen des Clusters verwendet werden. Weitere Informationen finden Sie unter [Erstellen von Hadoop-Clustern in HDInsight](../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Location of output data</b>: Gibt an, ob die Daten in einem HDFS (Hadoop Distributed File System) oder in Azure gespeichert werden. <br/><ul>Wenn Sie die Ausgabedaten in HDFS speichern, geben Sie den HDFS-Server-URI an. (Achten Sie darauf, den Namen des HDInsight-Clusters ohne das Präfix HTTPS:// zu verwenden). <br/><br/>Wenn Sie Ausgabedaten in Azure speichern, müssen Sie den Namen des Azure-Speicherkontos, den Speicherzugriffsschlüssel und Speichercontainernamen angeben.</ul> |
| SQL database |Liest Daten, die in einer Azure SQL-Datenbank oder einer SQL Server-Datenbank gespeichert sind, die in einer Azure-VM ausgeführt wird. |<b>Database server name</b>: Gibt den Namen des Servers an, auf dem die Datenbank ausgeführt wird.<br/><ul>Geben Sie bei Azure SQL-Datenbank den Servernamen ein, der generiert wird. Dieser hat meist das Format *&lt;Generierter_Bezeichner&gt;.database.windows.net*. <br/><br/>Geben Sie Folgendes ein, wenn eine SQL Server-Instanz auf einem virtuellen Azure-Computer gehostet wird: *tcp:&lt;DNS-Name des virtuellen Computers&gt;, 1433*.</ul><br/><b>Database name</b>: Gibt den Namen der Datenbank auf dem Server an. <br/><br/><b>Server user account name</b>: Gibt einen Benutzernamen für ein Konto an, das über Zugriffsberechtigungen für die Datenbank verfügt. <br/><br/><b>Server user account password</b>: Gibt das Kennwort für das Benutzerkonto an.<br/><br/><b>Accept any server certificate</b>: Verwenden Sie diese (weniger sichere) Option, um das Überprüfen des Websitezertifikats vor dem Lesen Ihrer Daten zu überspringen.<br/><br/><b>Database query</b>: Geben Sie eine SQL-­Anweisung als Beschreibung der Daten ein, die Sie lesen möchten. |
| On-premises SQL database |Liest Daten, die in einer lokalen SQL-Datenbank gespeichert ist. |<b>Data gateway</b>: Gibt den Namen des Datenverwaltungsgateways an, das auf einem Computer installiert ist, auf dem es auf Ihre SQL Server-Datenbank zugreifen kann. Informationen zum Einrichten des Gateways finden Sie unter [Durchführen der erweiterten Analyse mit Azure Machine Learning mit Daten aus einer lokalen SQL Server-Datenbank](machine-learning-use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Database server name</b>: Gibt den Namen des Servers an, auf dem die Datenbank ausgeführt wird.<br/><br/><b>Database name</b>: Gibt den Namen der Datenbank auf dem Server an. <br/><br/><b>Server user account name</b>: Gibt einen Benutzernamen für ein Konto an, das über Zugriffsberechtigungen für die Datenbank verfügt. <br/><br/><b>User name and password</b>: Klicken Sie auf <b>Enter values</b>, und geben Sie Ihre Datenbank-Anmeldeinformationen ein. Je nach Ihrer lokalen SQL Server-Konfiguration können Sie die Integrierte Windows-Authentifizierung oder SQL Server-Authentifizierung verwenden.<br/><br/><b>Database query</b>: Geben Sie eine SQL-­Anweisung als Beschreibung der Daten ein, die Sie lesen möchten. |
| Azure Table |Liest Daten aus dem Tabellenspeicherdienst in Azure Storage.<br/><br/>Wenn große Mengen von Daten selten gelesen werden, verwenden Sie den Azure-Tabellenspeicherdienst. Dieser Dienst bietet eine flexible, nicht relationale (NoSQL), überaus skalierbare, kostengünstige und hochverfügbare Speicherlösung. |Die Optionen im **Import Data**-Modul ändern sich je nachdem, ob Sie auf öffentliche Informationen oder ein privates Speicherkonto zugreifen, das Anmeldeinformationen erfordert. Dies wird anhand der Option <b>Authentication Type</b> bestimmt, die den Wert „PublicOrSAS“ oder „Account“ haben kann, der jeweils über einen eigenen Satz von Parametern verfügen kann. <br/><br/><b>Public or Shared Access Signature (SAS) URI</b>: Die Parameter lauten wie folgt:<br/><br/><ul><b>Table URI</b>: Gibt die öffentliche URL oder SAS-URL für die Tabelle an.<br/><br/><b>Gibt die Zeilen an, die für Eigenschaftsnamen durchsucht werden</b>: Die Werte sind <i>TopN</i> zum Ermitteln der angegebenen Anzahl von Zeilen und <i>ScanAll</i> zum Abrufen aller Zeilen der Tabelle. <br/><br/>Wenn die Daten homogen und vorhersagbar sind, wird empfohlen, *TopN* auszuwählen und eine Zahl für N einzugeben. Für große Tabellen kann dies zu kürzeren Lesedauern führen.<br/><br/>Wenn die Daten mit Sätzen von Eigenschaften strukturiert sind, die abhängig von der Tiefe und Position der Tabelle variieren, wählen Sie die Option *ScanAll*, um alle Zeilen zu überprüfen. Dadurch wird die Integrität der resultierenden Eigenschafts- und Metadatenkonvertierung sichergestellt.<br/><br/></ul><b>Private Storage Account</b>: Die Parameter lauten wie folgt: <br/><br/><ul><b>Account name</b>: Gibt den Namen des Kontos an, das die zu lesende Tabelle enthält.<br/><br/><b>Account key</b>: Gibt den Speicherschlüssel an, der dem Konto zugeordnet ist.<br/><br/><b>Table name</b>: Gibt den Namen der Tabelle an, die die zu lesenden Daten enthält.<br/><br/><b>Rows to scan for property names</b>: Die Werte sind <i>TopN</i> zum Ermitteln der angegebenen Anzahl von Zeilen und <i>ScanAll</i> zum Abrufen aller Zeilen der Tabelle.<br/><br/>Wenn die Daten homogen und vorhersagbar sind, wird empfohlen, *TopN* auszuwählen und eine Zahl für N einzugeben. Für große Tabellen kann dies zu kürzeren Lesedauern führen.<br/><br/>Wenn die Daten mit Sätzen von Eigenschaften strukturiert sind, die abhängig von der Tiefe und Position der Tabelle variieren, wählen Sie die Option *ScanAll*, um alle Zeilen zu überprüfen. Dadurch wird die Integrität der resultierenden Eigenschafts- und Metadatenkonvertierung sichergestellt.<br/><br/> |
| Azure Blob Storage |Liest im Blob-Dienst in Azure Storage gespeicherte Daten, so z. B. Bilder, unstrukturierten Text oder Binärdaten.<br/><br/>Mithilfe des Blob-Diensts können Sie Daten öffentlich verfügbar machen oder Anwendungsdaten privat speichern. Über HTTP oder HTTPS-Verbindungen können Sie von überall auf Ihre Daten zugreifen. |Die Optionen im **Import Data** -Modul ändern sich je nachdem, ob Sie auf öffentliche Informationen oder ein privates Speicherkonto zugreifen, das Anmeldeinformationen erfordert. Dies wird anhand der Option <b>Authentication Type</b> bestimmt, die den Wert "PublicOrSAS" oder "Account" haben kann.<br/><br/><b>Public or Shared Access Signature (SAS) URI</b>: Die Parameter lauten wie folgt:<br/><br/><ul><b>URI</b>: Gibt die öffentliche URL oder SAS-URL für das Speicherblob an.<br/><br/><b>File Format</b>: Gibt das Format der Daten im Blob-Dienst an. Die unterstützten Formate sind CSV, TSV und ARFF.<br/><br/></ul><b>Private Storage Account</b>: Die Parameter lauten wie folgt: <br/><br/><ul><b>Account name</b>: Gibt den Namen des Kontos an, das das zu lesende Blob enthält.<br/><br/><b>Account key</b>: Gibt den Speicherschlüssel an, der dem Konto zugeordnet ist.<br/><br/><b>Path to container, directory, or blob </b>: Gibt den Namen des Blobs an, das die zu lesenden Daten enthält.<br/><br/><b>Blob file format</b>: Gibt das Format der Daten im Blob-Dienst an. Die unterstützten Datenformate sind CSV, TSV, ARFF, CSV mit einer angegebenen Codierung und Excel. <br/><br/><ul>Wenn das Format CSV oder TSV ist, müssen Sie angeben, ob die Datei eine Kopfzeile enthält.<br/><br/>Sie können die Excel-Option zum Lesen von Daten aus Excel-Arbeitsmappen verwenden. Bei der Option <i>Excel data format</i> geben Sie an, ob die Daten in einem Excel-Arbeitsblattbereich oder einer Excel-Tabelle enthalten sind. Bei der Option <i>Excel sheet or embedded table</i> geben Sie den Namen des Blatts oder der Tabelle an, das bzw. die Sie lesen möchten.</ul><br/> |
| Data Feed Provider |Liest Daten aus einem unterstützten Feedanbieter. Derzeit wird nur das OData-Format (Open Data Protocol) unterstützt. |<b>Data content type</b>: Gibt das OData-Format an.<br/><br/><b>Source URL</b>: Gibt die vollständige URL für den Datenfeed an. <br/>Mit der folgenden URL wird beispielsweise aus der Northwind-Beispieldatenbank gelesen: „http://services.odata.org/northwind/northwind.svc/“. |

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Azure ML-Webdiensten, die Module zum Importieren und Exportieren von Daten verwenden](machine-learning-web-services-that-use-import-export-modules.md)


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/

