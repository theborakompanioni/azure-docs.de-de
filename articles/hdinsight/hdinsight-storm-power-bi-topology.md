<properties
 pageTitle="Verwenden von Apache Storm mit Power BI | Microsoft Azure"
 description="Erstellen Sie einen Power BI-Bericht aus Daten einer C#-Topologie auf einem Apache Storm-Cluster in HDInsight."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
	tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="08/16/2016"
 ms.author="larryfr"/>

# Verwenden von Power BI zur Visualisierung von Daten aus einer Apache Storm-Topologie

Mit Power BI können Sie Daten als Berichte anzeigen. Mit den Visual Studio-Vorlagen für Storm in HDInsight können Sie problemlos Speicherdaten aus einer Topologie, die auf Apache Storm in einem HDInsight-Clusters ausgeführt wird, in SQL Azure verwenden und dann die Daten mithilfe von Power BI visualisieren.

In diesem Dokument erfahren Sie, wie Sie mit Power BI einen Bericht aus Daten erstellen, die von einer Apache Storm-Topologie generiert und in Azure SQL-Datenbank gespeichert werden.

## Voraussetzungen

- Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (in englischer Sprache).

* Einen Azure Active Directory-Benutzer mit [Power BI](https://powerbi.com)-Zugriff

* Visual Studio (eine der folgenden Versionen):

    * Visual Studio 2012 mit [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)

    * Visual Studio 2013 mit [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) oder [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)

    * [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)

* Informationen zum Installieren der HDInsight Tools für Visual Studio finden Sie unter [Erste Schritte mit den HDInsight Tools für Visual Studio](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md).

## So funktioniert's

Dieses Beispiel enthält eine C#-Storm-Topologie, die Protokolldaten für Internetinformationsdienste (Internet Information Services, IIS) zufällig generiert. Diese Daten werden dann in eine SQL-Datenbank geschrieben und von dort zum Generieren von Berichten in Power BI verwendet.

Im Folgenden finden Sie eine Liste der Dateien, die die wichtigsten Funktionen dieses Beispiels implementieren.

* **SqlAzureBolt.cs**: Schreibt Informationen, die in der Storm-Topologie erzeugt werden, in die SQL-Datenbank.

* **IISLogsTable.sql**: Die Transact-SQL-Anweisungen, die verwendet werden, um die Datenbank zu generieren, in der die Daten gespeichert werden.

> [AZURE.WARNING] Sie müssen die Tabelle in der SQL-Datenbank erstellen, bevor Sie die Topologie in Ihrem HDInsight-Cluster starten.

## Das Beispiel herunterladen

Herunterladen des [HDInsight C# Storm Power BI-Beispiels](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi). Zum Herunterladen verzweigen/klonen Sie es entweder mit [Git](http://git-scm.com/) oder verwenden Sie den **Download**-Link zum Herunterladen einer ZIP-Datei aus dem Archiv.

## Erstellen einer Datenbank

1. Verwenden Sie die Schritte im [Tutorial zur SQL-Datenbank](../sql-database/sql-database-get-started.md) zum Erstellen einer neuen SQL-Datenbank.

2. Stellen Sie mithilfe der Schritte im Dokument [Herstellen der Verbindung mit einer SQL-Datenbank mit Visual Studio](../sql-database/sql-database-connect-query.md) eine Verbindung mit der Datenbank her.

4. Klicken Sie mit der rechten Maustaste im Objekt-Explorer auf die Datenbank, und erstellen Sie eine __neue Abfrage__. Fügen Sie den Inhalt der Datei __IISLogsTable.sql__, die im heruntergeladenen Projekt enthalten ist, in das Abfragefenster ein, und verwenden Sie STRG+UMSCHALT+E, um die Abfrage auszuführen. Eine Meldung sollte angezeigt werden, die besagt, dass die Befehle erfolgreich abgeschlossen wurden.

    Sobald dies abgeschlossen ist, gibt es in der Datenbank eine neue Tabelle namens __IISLOGS__.

## Das Beispiel konfigurieren

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihre SQL-Datenbank aus. Wählen Sie im Abschnitt __Zusammenfassung__ auf dem Blatt der SQL-Datenbank __Datenbank-Verbindungszeichenfolgen anzeigen__ aus. Kopieren Sie aus der angezeigten Liste die Informationen für __ADO.NET (SQL-Authentifizierung)__.

1. Öffnen Sie das Beispiel in Visual Studio. Öffnen Sie im **Projektmappen-Explorer** die Datei **App.config**, und suchen Sie dann den folgenden Eintrag.

        <add key="SqlAzureConnectionString" value="##TOBEFILLED##" />
    
    Ersetzen Sie den Wert __##TOBEFILLED##__ durch die Datenbank-Verbindungszeichenfolge, die Sie im vorherigen Schritt kopiert haben. Ersetzen Sie __{your\_username}__ und __{your\_password}__ durch den Benutzernamen und das Kennwort für die Datenbank.

2. Speichern und schließen Sie die Dateien.

## Bereitstellen des Beispiels

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **StormToSQL**, und wählen Sie **An Storm in HDInsight übermitteln** aus. Wählen Sie den HDInsight-Cluster aus dem Dropdown-Dialogfeld **Storm-Cluster** aus.

    > [AZURE.NOTE] Es kann einige Sekunden dauern, bis die Dropdown-Liste **Storm-Cluster** mit Servernamen befüllt wird.
    >
    > Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen für Ihr Azure-Abonnement ein. Wenn Sie über mehrere Abonnements verfügen, melden Sie sich bei dem Abonnement an, das Ihren Storm-Cluster in HDInsight enthält.

2. Sobald die Topologie erfolgreich übermittelt wurde, sollten die Storm-Topologien für den Cluster angezeigt werden. Wählen Sie den Eintrag „SqlAzureWriterTopology“ in der Liste aus, um Informationen zur aktiven Topologie anzuzeigen.

    ![Die Topologien mit der ausgewählten Topologie](./media/hdinsight-storm-power-bi-topology/topologyview.png)

    Verwenden Sie diese Ansicht, um Informationen zur Topologie anzuzeigen. Sie können auch auf Einträge (z.B. „SqlAzureBolt“) doppelklicken, um Informationen zu einer Komponente in der Topologie anzuzeigen.

3. Nachdem die Topologie für ein paar Minuten ausgeführt wurde, kehren Sie zum SQL-Abfragefenster zurück, das Sie zum Erstellen der Datenbank verwendet haben. Ersetzen Sie die vorhandenen Anweisungen durch Folgendes:

        select * from iislogs;
    
    Führen Sie mit STRG+UMSCHALT+E die Abfrage aus, und Sie sollten Ergebnisse wie die folgenden erhalten.
    
        1	2016-05-27 17:57:14.797	255.255.255.255	/bar	GET	200
        2	2016-05-27 17:57:14.843	127.0.0.1	/spam/eggs	POST	500
        3	2016-05-27 17:57:14.850	123.123.123.123	/eggs	DELETE	200
        4	2016-05-27 17:57:14.853	127.0.0.1	/foo	POST	404
        5	2016-05-27 17:57:14.853	10.9.8.7	/bar	GET	200
        6	2016-05-27 17:57:14.857	192.168.1.1	/spam	DELETE	200

    Dies sind Daten, die von der Storm-Topologie geschrieben wurden.

## Erstellen eines Berichts

1. Stellen Sie eine Verbindung mit dem [Azure SQL-Datenbank-Connector](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect) für Power BI her.

2. Wählen Sie unter __Datenbanken__ die Option __Abrufen__ aus.

3. Wählen Sie __Azure SQL-Datenbank__ und dann __Verbinden__ aus.

4. Geben Sie die Informationen zum Herstellen der Verbindung mit der Azure SQL-Datenbank ein. Sie finden diese, indem Sie im [Azure-Portal](https://portal.azure.com) Ihre SQL-Datenbank auswählen.

    > [AZURE.NOTE] Sie können auch das Aktualisierungsintervall und benutzerdefinierte Filter festlegen, indem Sie im Verbindungsdialogfeld __Erweiterte Optionen aktivieren__ auswählen.

5. Nachdem Sie eine Verbindung hergestellt haben, wird ein neues Dataset mit dem gleichen Namen wie die Datenbank, mit der Sie verbunden sind, angezeigt. Wählen Sie das Dataset aus, um mit dem Entwerfen eines Berichts zu beginnen.

3. Erweitern Sie unter __Felder__ den Eintrag __IISLOGS__. Aktivieren Sie das Kontrollkästchen für __URISTEM__. Dadurch wird ein neuer Bericht erstellt, der URI-Stämme (/foo, /bar usw.) aufführt, die in der Datenbank protokolliert wurden.

    ![Erstellen eines Berichts](./media/hdinsight-storm-power-bi-topology/createreport.png)

5. Ziehen Sie als Nächstes __METHODE__ in den Bericht. Der Bericht wird aktualisiert und zeigt jetzt die Stämme und die entsprechende HTTP-Methode an, die für die HTTP-Anforderung verwendet wurde.

    ![Hinzufügen der Methodendaten](./media/hdinsight-storm-power-bi-topology/uristemandmethod.png)

4. Wählen Sie in der Spalte __Visualisierungen__ das Symbol __Felder__ aus, und wählen Sie dann im Abschnitt __Werte__ den Pfeil nach unten neben __METHODE__ aus. Wählen Sie in der angezeigten Liste __Anzahl__ aus. Dadurch wird im Bericht angezeigt, wie oft auf einen bestimmten URI zugegriffen wurde.

    ![Ändern in die Anzahl der Methoden](./media/hdinsight-storm-power-bi-topology/count.png)

6. Wählen Sie als Nächstes __Gestapeltes Säulendiagramm__ aus, um zu ändern, wie die Informationen angezeigt werden.

    ![Ändern in ein gestapeltes Diagramm](./media/hdinsight-storm-power-bi-topology/stackedcolumn.png)

7. Wenn der Bericht so aussieht, wie Sie ihn haben möchten, verwenden Sie den Menüeintrag __Speichern__, um einen Namen einzugeben und den Bericht zu speichern.

## Beenden der Topologie

Die Topologie wird weiterhin ausgeführt, bis Sie sie beenden oder das Storm-Cluster in HDInsight löschen. Führen Sie folgende Schritte aus, um die Topologie anzuhalten.

1. Kehren Sie in Visual Studio zum Topologie-Viewer zurück, und wählen Sie die Topologie aus.

2. Wählen Sie die Schaltfläche **Beenden** aus, um die Topologie anzuhalten.

    ![Schaltfläche „Beenden“ in der Topologie-Zusammenfassung](./media/hdinsight-storm-power-bi-topology/killtopology.png)

## Löschen des Clusters

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Daten aus einer Storm-Topologie an eine SQL-Datenbank gesendet und dann mit Power BI visualisiert werden. Informationen zum Arbeiten mit anderen Azure-Technologien mithilfe von Storm in HDInsight finden Sie hier:

* [Beispiele für Storm-Topologien für Storm in HDInsight](hdinsight-storm-example-topology.md)

<!---HONumber=AcomDC_0914_2016-->