<properties
	pageTitle="Installieren von RStudio mit R Server in HDInsight (Vorschau) | Microsoft Azure"
	description="So installieren von RStudio mit R Server in HDInsight (Vorschau)."
	services="hdinsight"
	documentationCenter=""
	authors="jeffstokes72"
	manager="jhubbard"
	editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/16/2016"
   ms.author="jeffstok"/>


# Installieren von RStudio mit R Server in HDInsight (Vorschau)

Es stehen derzeit verschiedene integrierte Entwicklungsumgebungen (IDE) für R zur Verfügung, darunter die kürzlich angekündigten [R-Tools für Visual Studio](https://www.visualstudio.com/de-DE/features/rtvs-vs.aspx) (RTVS) von Microsoft, eine Familie aus Desktop- und Servertools aus [RStudio](https://www.rstudio.com/products/rstudio-server/), oder das auf Eclipse basierende [StatET](http://www.walware.de/goto/statet) von Walware. Auf Linux ist insbesondere [RStudio Server](https://www.rstudio.com/products/rstudio-server/) sehr populär, das eine browserbasierte Entwicklungsumgebung zur Verwendung durch Remoteclients bereitstellt. Das Installieren von RStudio Server auf dem Edgeknoten eines HDInsight Premium-Clusters bietet eine umfassende IDE-Umgebung für die Entwicklung und Ausführung von R-Skripts mit R Server im Cluster und kann die Produktivität im Vergleich zur Standardverwendung der R-Konsole deutlich steigern.

In diesem Artikel erfahren Sie, wie Sie mithilfe eines benutzerdefinierten Skripts die (kostenlose) Communityversion von RStudio Server auf dem Edgeknoten eines Clusters installieren. Wenn Sie die kommerziell lizenzierte Pro-Version von RStudio Server vorziehen, müssen Sie den Installationsanweisungen von [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/) folgen.

> [AZURE.NOTE] Die Schritte in diesem Dokument erfordern einen Cluster mit R Server in HDInsight und funktionieren nicht ordnungsgemäß, wenn Sie einen HDInsight-Cluster verwenden, in dem R mithilfe der [Skriptaktion zum Installieren von R](hdinsight-hadoop-r-scripts-linux.md) installiert wurde.

## Voraussetzungen

* Einen Azure HDInsight-Cluster, auf dem R Server installiert ist. Anweisungen finden Sie unter [Erste Schritte mit R Server in HDInsight (Vorschau)](hdinsight-hadoop-r-server-get-started.md).
* Einen SSH-Client. Für Linux- und Unix-Distributionen oder Macintosh OS X steht der Befehl `ssh` über das Betriebssystem zur Verfügung. Für Windows empfehlen wir [Cygwin](http://www.redhat.com/services/custom/cygwin/) mit der Option [OpenSSH](https://www.youtube.com/watch?v=CwYSvvGaiWU) oder [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).


## Installieren von RStudio auf dem Cluster mit einem benutzerdefinierten Skript

1. Identifizieren Sie den Edgeknoten des Clusters. Für einen HDInsight-Cluster mit R Server gilt die folgende Namenskonvention für Hauptknoten und Edgeknoten.

	* Hauptknoten: `CLUSTERNAME-ssh.azurehdinsight.net`
	* Edgeknoten: `R-Server.CLUSTERNAME-ssh.azurehdinsight.net`

2. Greifen Sie unter Verwendung des obigen Namensmusters per SSH auf den Edgeknoten des Clusters zu.
 
	* Wenn Sie die Verbindung von einem Linux-Client aus herstellen, finden Sie weitere Informationen unter [Verbinden mit einem Linux-basierten HDInsight-Cluster](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).
	* Wenn Sie die Verbindung von einem Windows-Client aus herstellen, finden Sie weitere Informationen unter [Verbinden mit einem Linux-basierten HDInsight-Cluster unter Verwendung von PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster).

3. Legen Sie sich nach der Verbindungsherstellung als Root-Benutzer im Cluster fest. Verwenden Sie in der SSH-Sitzung den folgenden Befehl:

		sudo su -

4. Laden Sie das benutzerdefinierte Skript zum Installieren von RStudio herunter. Verwenden Sie den folgenden Befehl:

		wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Ändern Sie die Berechtigungen in der benutzerdefinierten Skriptdatei, und führen Sie das Skript aus. Verwenden Sie die folgenden Befehle:

		chmod 755 InstallRStudio.sh
		./InstallRStudio.sh

6. Wenn Sie beim Erstellen eines HDInsight-Clusters mit R Server ein SSH-Kennwort verwendet haben, können Sie diesen Schritt überspringen und mit dem nächsten Schritt fortfahren. Wenn Sie stattdessen einen SSH-Schlüssel zum Erstellen des Clusters verwendet haben, müssen Sie ein Kennwort für Ihren SSH-Benutzer festlegen. Sie benötigen dieses Kennwort bei der Verbindungsherstellung mit RStudio. Führen Sie die folgenden Befehle aus: Wenn Sie aufgefordert werden, das **aktuelle Kerberos-Kennwort** einzugeben, drücken Sie lediglich die **EINGABETASTE**. Hinweis: `USERNAME` muss durch einen SSH-Benutzer für Ihren HDInsight-Cluster ersetzt werden.

		passwd USERNAME
		Current Kerberos password:
		New password:
		Retype new password:
		Current Kerberos password:
		
	Wenn Ihr Kennwort erfolgreich festgelegt wurde, sollten Sie eine Meldung wie diese sehen:

		passwd: password updated successfully


	Beenden Sie die SSH-Sitzung.

7. Erstellen Sie einen SSH-Tunnel zum Cluster, indem Sie `localhost:8787` im HDInsight-Cluster dem Clientcomputer zuordnen. Sie müssen einen SSH-Tunnel erstellen, bevor Sie eine neue Browsersitzung öffnen.

	* Öffnen Sie auf einem Linux-Client oder auf einem Windows-Client mit [Cygwin](http://www.redhat.com/services/custom/cygwin/) eine Terminalsitzung, und verwenden Sie den folgenden Befehl:

			ssh -L localhost:8787:localhost:8787 USERNAME@R-Server.CLUSTERNAME-ssh.azurehdinsight.net
			
		Ersetzen Sie **USERNAME** durch einen SSH-Benutzer für Ihren HDInsight-Cluster und **CLUSTERNAME** durch den Namen des HDInsight-Clusters. Durch Hinzufügen von `-i id_rsa_key` können Sie anstelle eines Kennworts auch einen SSH-Schlüssel verwenden.

	* Bei Verwendung eines Windows-Clients mit PuTTY:

		1.  Öffnen Sie PuTTY, und geben Sie die Verbindungsinformationen ein. Wenn Sie nicht mit PuTTY vertraut sind, finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md) Informationen für die Verwendung mit HDInsight.
		2.  Erweitern Sie auf der linken Seite des Dialogfelds im Abschnitt **Category** erst **Connection**, dann **SSH**, und wählen Sie anschließend **Tunnels** aus.
		3.  Geben Sie die folgenden Informationen in das Formular **Options controlling SSH port forwarding** ein:

			* **Source port**: Der Port auf dem Client, den Sie weiterleiten möchten. Beispiel: **8787**.
			* **Destination**: Das Ziel, das dem lokalen Clientcomputer zugeordnet werden muss. Beispiel **localhost:8787**.

			![Erstellen eines SSH-Tunnels](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Erstellen eines SSH-Tunnels")

		4. Klicken Sie auf **Add**, um die Einstellungen hinzuzufügen. Klicken Sie dann auf **Open**, um eine SSH-Verbindung zu öffnen.
		5. Melden Sie sich bei entsprechender Aufforderung am Server an. Dadurch wird eine SSH-Sitzung eingerichtet und der Tunnel aktiviert.

8. Öffnen Sie einen Webbrowser, und geben Sie basierend auf dem Port, den Sie für den Tunnel eingegeben haben, die folgende URL ein:

		http://localhost:8787/ 

9. Sie werden aufgefordert, den SSH-Benutzernamen und das Kennwort zur Verbindungsherstellung mit dem Cluster einzugeben. Wenn Sie beim Erstellen des Clusters einen SSH-Schlüssel verwendet haben, müssen Sie das Kennwort eingeben, das Sie oben in Schritt 5 erstellt haben.

	![Verbindungsherstellung mit RStudio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Erstellen eines SSH-Tunnels")

10. Um zu testen, ob die RStudio-Installation erfolgreich war, können Sie ein Testskript ausführen, mit dem R-basierte MapReduce- und Spark-Aufträge auf dem Cluster ausgeführt werden. Kehren Sie zur SSH-Konsole zurück, und geben Sie die folgenden Befehle ein, um das Testskript zur Ausführung in RStudio herunterzuladen:

	* Wenn Sie einen Hadoop-Cluster mit R erstellt haben, verwenden Sie diesen Befehl:
		
			wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r

	* Wenn Sie einen Spark-Cluster mit R erstellt haben, verwenden Sie diesen Befehl:

			wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

11. Sie sollten das heruntergeladene Testskript in RStudio sehen. Doppelklicken Sie auf die Datei, um sie zu öffnen, wählen Sie die Inhalte der Datei aus, und klicken Sie auf **Run**. Die Ausgabe sollte im Bereich **Console** angezeigt werden.
 
	![Testen der Installation](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Testen der Installation")

Alternativ können Sie `source(testhdi.r)` oder `source(testhdi_spark.r)` eingeben, um das Skript auszuführen.

## Siehe auch

- [Rechenkontextoptionen für R Server auf HDInsight-Clustern](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure Storage options for R Server on HDInsight Premium (Azure Storage-Optionen für R Server in HDInsight Premium)](hdinsight-hadoop-r-server-storage.md)


 

<!---HONumber=AcomDC_0921_2016-->