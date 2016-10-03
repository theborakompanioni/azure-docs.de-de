<properties
	pageTitle="Verstehen von Hadoop mithilfe einer Hadoop-Sandbox | Microsoft Azure"
	description="Um sich mit dem Hadoop-Ökosystem vertraut zu machen, können Sie eine Hadoop-Sandbox von Hortonworks auf einem virtuellen Azure-Computer einrichten. "
	keywords="hadoop emulator,hadoop sandbox"
	editor="cgronlun"
	manager="jhubbard"
	services="hdinsight"
	authors="nitinme"
	documentationCenter=""
	tags="azure-portal"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/24/2016"
	ms.author="nitinme"/>

# Erste Schritte im Hadoop-Ökosystem mit einer Hadoop-Sandbox auf einem virtuellen Computer

Hier erfahren Sie, wie Sie die Hadoop-Sandbox von Hortonworks auf einem virtuellen Computer installieren, um sich mit dem Hadoop-Ökosystem vertraut zu machen. Die Sandbox bietet eine lokale Entwicklungsumgebung, in der Sie Hadoop, Hadoop Distributed File System (HDFS) und die Übermittlung von Aufträgen kennenlernen.

## Voraussetzungen

* [Oracle VirtualBox](https://www.virtualbox.org/)

Nachdem Sie sich mit Hadoop vertraut gemacht haben, können Sie Hadoop in Azure verwenden und einen HDInsight-Cluster erstellen. Weitere Informationen für Ihren Einstieg finden Sie unter [Erste Schritte mit Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

## Herunterladen und Installieren des virtuellen Computers

1. Wählen Sie unter [http://hortonworks.com/downloads/#sandbox](http://hortonworks.com/downloads/#sandbox) für „HDP 2.4 on Hortonworks Sandbox“ die Option __DOWNLOAD FOR VIRTUALBOX__ (FÜR VIRTUALBOX HERUNTERLADEN) aus. Vor Beginn des Downloads werden Sie aufgefordert, sich bei Hortonworks zu registrieren.

    ![Abbildung des Links zum Herunterladen von Hortonworks Sandbox für VirtualBox](./media/hdinsight-hadoop-emulator-get-started/download-sandbox.png)

2. Wählen Sie auf der gleichen Webseite für „HDP 2.4 on Hortonworks Sandbox“ die Option __VirtualBox Install Guide__ (VirtualBox-Installationshandbuch) aus. Dadurch wird eine PDF-Datei mit Installationsanweisungen für den virtuellen Computer heruntergeladen.

    ![Anzeigen des Installationshandbuchs](./media/hdinsight-hadoop-emulator-get-started/view-install-guide.png)

## Starten des virtuellen Computers

1. Starten Sie VirtualBox. Wählen Sie „Hortonworks Sandbox“ > __Start__ und anschließend __Normal Start__ (Normaler Start) aus.

    ![Normaler Start](./media/hdinsight-hadoop-emulator-get-started/normal-start.png)

2. Nach Abschluss des Startvorgangs für den virtuellen Computer werden Anmeldeanweisungen angezeigt. Öffnen Sie einen Webbrowser, und navigieren Sie zur angezeigten URL (in der Regel http://127.0.0.1:8888).

## Festlegen von Kennwörtern

1. Wählen Sie im Schritt __Get Started__ (Erste Schritte) der Seite „Hortonworks Sandbox“ die Option __View Advanced Options__ (Erweiterte Optionen anzeigen) aus. Verwenden Sie die Informationen auf dieser Seite, um sich mittels SSH bei der Sandbox anzumelden. Verwenden Sie den angegebenen Namen und das angegebene Kennwort.

    > [AZURE.NOTE] Sollte kein SSH-Client installiert sein, können Sie die webbasierte SSH verwenden, die vom virtuellen Computer unter „\_\_http://localhost:4200/__“ bereitgestellt wird.

    Beim erstmaligen Herstellen einer Verbindung über SSH werden Sie aufgefordert, das Kennwort für das Stammkonto zu ändern. Geben Sie ein neues Kennwort ein. Dieses Kennwort wird dann bei zukünftigen SSH-Anmeldungen verwendet.

2. Geben Sie nach der Anmeldung den folgenden Befehl ein:

        ambari-admin-password-reset
    
    Geben Sie ein Kennwort für das Ambari-Administratorkonto ein, wenn Sie dazu aufgefordert werden. Dieses wird verwendet, wenn Sie auf die Ambari-Webbenutzeroberfläche zugreifen.

## Verwenden des Hive-Befehls

1. Verwenden Sie bei einer SSH-Verbindung mit der Sandbox den folgenden Befehl, um die Hive-Shell zu starten:

        hive

2. Verwenden Sie nach dem Start der Shell Folgendes, um die Tabellen anzuzeigen, die zusammen mit der Sandbox bereitgestellt werden:

        show tables;

3. Verwenden Sie Folgendes, um zehn Zeilen aus der Tabelle `sample_07` abzurufen:

        select * from sample_07 limit 10;

## Nächste Schritte

* [Verwenden von Visual Studio mit der Sandbox von Hortonworks](hdinsight-hadoop-emulator-visual-studio.md)
* [Learning the ropes of the Hortonworks Sandbox](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/) (Einführung in Hortonworks Sandbox)
* [Hadoop tutorial - Getting started with HDP](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/) (Hadoop-Tutorial – Erste Schritte mit HDP)

<!---HONumber=AcomDC_0921_2016-->