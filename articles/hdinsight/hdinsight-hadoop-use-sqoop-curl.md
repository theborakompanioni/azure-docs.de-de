<properties
   pageTitle="Verwenden von Hadoop Sqoop mit Curl in HDInsight | Microsoft Azure"
   description="Erfahren Sie, wie Sie Sqoop-Aufträge mithilfe von Curl remote an HDInsight übermitteln."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"
	tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/25/2016"
   ms.author="jgao"/>

#Ausführen von Sqoop-Aufträgen mit Hadoop in HDInsight mit Curl

[AZURE.INCLUDE [Sqoop-Auswahl](../../includes/hdinsight-selector-use-sqoop.md)]

In diesem Dokument erfahren Sie, wie mithilfe von Curl Sqoop-Aufträge auf einem Hadoop-Cluster in Azure HDInsight ausgeführt werden.

Curl wird verwendet, um zu veranschaulichen, wie Sie über unformatierte HTTP-Anforderungen zum Ausführen, Überwachen und Abrufen der Ergebnisse der Sqoop-Aufträge mit HDInsight interagieren können. Dies funktioniert mithilfe der WebHCat REST-API (ehemals Templeton), die von Ihrem HDInsight-Cluster bereitgestellt wird.

> [AZURE.NOTE] Wenn Sie bereits mit der Verwendung von Linux-basierten Hadoop-Servern vertraut sind, jedoch noch nicht mit HDInsight, finden Sie weitere Informationen unter [Was Sie über Hadoop auf Linux-basiertem HDInsight wissen müssen](hdinsight-hadoop-linux-information.md).

##Voraussetzungen

Um die in diesem Artikel aufgeführten Schritte auszuführen, benötigen Sie Folgendes:

* Einen Hadoop-Cluster in HDInsight (Linux- oder Windows-basiert)

* [Curl](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##Übermitteln von Sqoop-Aufträgen mithilfe von Curl

> [AZURE.NOTE] Wenn Sie Curl oder eine andere REST-Kommunikation mit WebHCat verwenden, müssen Sie die Anforderungen authentifizieren, indem Sie den Benutzernamen und das Kennwort des Administrators des HDInsight-Clusters bereitstellen. Sie müssen auch den Clusternamen als Teil des URIs (Uniform Resource Identifier) verwenden, um die Anforderungen an den Server zu senden.
>
> Ersetzen Sie für die Befehle in diesem Abschnitt die Option **BENUTZERNAME** zur Authentifizierung im Cluster durch den Benutzer und die Option **KENNWORT** durch das Kennwort für das Benutzerkonto. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters.
>
> Die REST-API wird durch [Standardauthentifizierung](http://en.wikipedia.org/wiki/Basic_access_authentication) gesichert. Sie sollten Anforderungen immer über HTTPS (Secure HTTP) stellen, um sicherzustellen, dass Ihre Anmeldeinformationen sicher an den Server gesendet werden.

1. Verwenden Sie den folgenden Befehl in einer Befehlszeile, um zu überprüfen, ob Sie die Verbindung zum HDInsight-Cluster herstellen können:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Sie sollten eine Antwort empfangen, die in etwa der im Folgenden aufgeführten entspricht:

        {"status":"ok","version":"v1"}

    Folgende Parameter werden in diesem Befehl verwendet:

    * **-u** – Der Benutzername und das Kennwort für die Authentifizierung der Anforderung
    * **-G** – Gibt an, dass dies eine GET-Anforderung ist

    Der Anfang der URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, ist für alle Anforderungen identisch. Der Pfad, **/status**, gibt an, dass die Anforderung den Status von WebHCat (auch bekannt als Templeton) für den Server zurückgibt.

2. Geben Sie den folgenden Befehl an, um einen Sqoop-Auftrag zu übermitteln:


        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    Folgende Parameter werden in diesem Befehl verwendet:

    * **-d** - Da `-G` nicht verwendet wird, ist die Anforderung standardmäßig die POST-Methode. `-d` gibt die Datenwerte an, die mit der Anforderung gesendet werden.

        * **user.name** – Der Benutzer, der den Befehl ausführt

        * **command** – Der auszuführende Sqoop-Befehl.

        * **statusdir** – Das Verzeichnis, in das die Statusangaben für diesen Auftrag geschrieben werden.

    Dieser Befehl sollte eine Auftrags-ID zurückgeben, mit der der Status des Auftrags überprüft werden kann.

        {"id":"job_1415651640909_0026"}

3. Verwenden Sie den folgenden Befehl, um den Status des Auftrags zu prüfen. Ersetzen Sie **JOBID** durch den Wert, der im vorherigen Schritt zurückgegeben wurde. Wenn der Rückgabewert z. B. `{"id":"job_1415651640909_0026"}` lautet, ist die **JOBID** `job_1415651640909_0026` .

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

	Wenn der Auftrag abgeschlossen ist, wird der Status **ERFOLGREICH** angezeigt.

    > [AZURE.NOTE] Diese Curl-Anforderung gibt ein JSON-Dokument (JavaScript Object Notation) mit Informationen zum Auftrag zurück. Mithilfe von "jq" wird nur der Statuswert abgerufen.

4. Sobald der Status des Auftrags zu **ERFOLGREICH** wechselt, können Sie die Ergebnisse des Auftrags aus dem Azure Blob-Speicher abrufen. Der mit der Abfrage übergebene Parameter `statusdir` enthält den Speicherort der Ausgabedatei. In diesem Fall **wasbs:///example/curl**. Diese Adresse speichert die Ausgabe des Auftrags im Verzeichnis **example/curl** des Standardspeichercontainers, der von Ihrem HDInsight-Cluster verwendet wird.

    Sie können diese Dateien mithilfe der [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) auflisten und herunterladen. Wenn Sie z. B. Dateien im Verzeichnis **Beispiel/Curl** auflisten möchten, verwenden Sie den folgenden Befehl:

		azure storage blob list <container-name> example/curl

	Verwenden Sie den folgenden Befehl, um eine Datei herunterzuladen:

		azure storage blob download <container-name> <blob-name> <destination-file>

	> [AZURE.NOTE] Sie müssen entweder den Namen des Speicherkontos, das den Blob enthält, mithilfe der Parameter `-a` und `-k` angeben oder die Umgebungsvariablen **AZURE\\_STORAGE\\_ACCOUNT** und **AZURE\\_STORAGE\\_ACCESS\\_KEY** festlegen. Weitere Informationen finden Sie unter<a href="hdinsight-upload-data.md" target="\_blank".

##Einschränkungen

* Massenexport: Bei Linux-basiertem HDInsight unterstützt der zum Exportieren von Daten nach Microsoft SQL Server oder Azure SQL-Datenbank verwendete Sqoop-Connector derzeit keine Masseneinfügungen.

* Batchverarbeitung: Wenn Sie in Linux-basiertem HDInsight zum Ausführen von Einfügevorgängen den Schalter `-batch` verwenden, führt Sqoop mehrere Einfügevorgänge aus, anstatt diese zu einem Batch zusammenzufassen.

##Zusammenfassung

Wie in diesem Dokument veranschaulicht, können Sie unformatierte HTTP-Anforderungen dazu verwenden, um Sqoop-Aufträge auf dem HDInsight-Cluster auszuführen, zu überwachen und deren Ergebnisse anzuzeigen.

Weitere Informationen zur REST-Schnittstelle, die in diesem Artikel verwendet wird, finden Sie unter <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API guide</a> (Leitfaden zur Sqoop-REST-API).

##Nächste Schritte

Allgemeine Informationen zu Hive mit HDInsight:

* [Verwenden von Sqoop mit Hadoop in HDInsight](hdinsight-use-sqoop.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)

* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

<!---HONumber=AcomDC_0914_2016-->