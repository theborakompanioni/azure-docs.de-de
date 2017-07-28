---
title: "Installieren von RStudio mit R Server in HDInsight – Azure | Microsoft-Dokumentation"
description: So installieren Sie RStudio mit R Server in HDInsight.
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 918abb0d-8248-4bc5-98dc-089c0e007d49
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 416420d855505508735ebd8526e93efdb230ad53
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---
# <a name="installing-rstudio-with-r-server-on-hdinsight"></a>Installieren von RStudio mit R Server in HDInsight

In diesem Artikel wird beschrieben, wie Sie mithilfe eines benutzerdefinierten Skripts die (kostenlose) Communityversion von [RStudio Server](https://www.rstudio.com/products/rstudio-server/) auf dem Edgeknoten eines Clusters installieren. RStudio Server stellt eine browserbasierte IDE zur Verwendung durch Remoteclients bereit und wird unter Linux häufig eingesetzt. Es stehen derzeit verschiedene integrierte Entwicklungsumgebungen (Integrated Developer Environments, IDE) für R zur Verfügung, darunter folgende:

- [R Tools für Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS) von Microsoft 
- [RStudio Server](https://www.rstudio.com/products/rstudio-server/) 
- Das auf Eclipse basierende [StatET](http://www.walware.de/goto/statet) von Walware

Die Installation von RStudio Server auf dem Edgeknoten eines HDInsight-Clusters hat den Vorteil, dass dadurch eine umfassende IDE-Umgebung für die Entwicklung und Ausführung von R-Skripts mit R Server im Cluster zur Verfügung steht. Durch diese Konfiguration kann sich die Produktivität im Vergleich zur Standardverwendung der R-Konsole deutlich steigern.

> [!NOTE]
> Das in diesem Artikel beschriebene Verfahren ist nur relevant, wenn Sie bei der Bereitstellung des Clusters nicht die Installation der Communityedition von RStudio Server ausgewählt haben. Wenn Sie RStudio Server während der Bereitstellung hinzugefügt haben, greifen Sie durch Klicken auf die Kachel **R Server-Dashboards** im Azure-Portal-Eintrag für den Cluster darauf zu, und klicken Sie dann auf die Kachel **R Studio Server**. 

Wenn Sie die kommerziell lizenzierte Pro-Version von RStudio Server vorziehen, müssen Sie den Installationsanweisungen von [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/)folgen.

> [!NOTE]
> Wenn Sie einen HDInsight-Cluster verwenden, für den R mithilfe der [Skriptaktion zum Installieren von R](hdinsight-hadoop-r-scripts-linux.md) installiert wurde, funktionieren die Schritte in diesem Dokument nicht ordnungsgemäß, da sie einen Cluster mit R Server in HDInsight erfordern.
>
> 

## <a name="prerequisites"></a>Voraussetzungen

* Einen Azure HDInsight-Cluster, auf dem R Server installiert ist. Anweisungen finden Sie unter [Erste Schritte mit R Server in HDInsight](hdinsight-hadoop-r-server-get-started.md).
* Einen SSH-Client. Für Linux- und Unix-Distributionen oder Macintosh OS X steht der Befehl `ssh` über das Betriebssystem zur Verfügung. Für Windows empfehlen wir [Cygwin](http://www.redhat.com/services/custom/cygwin/) mit der Option [OpenSSH](https://www.youtube.com/watch?v=CwYSvvGaiWU) oder [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  

## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a>Installieren von RStudio auf dem Cluster mit einem benutzerdefinierten Skript

Gehen Sie wie folgt vor:

1. Identifizieren Sie den Edgeknoten des Clusters. Für einen HDInsight-Cluster mit R Server gilt die folgende Namenskonvention für Hauptknoten und Edgeknoten.
   * Hauptknoten `CLUSTERNAME-ssh.azurehdinsight.net`
   * Edgeknoten `CLUSTERNAME-ed-ssh.azurehdinsight.net` 

2. Greifen Sie unter Verwendung des in Schritt 1 beschriebenen Namensmusters per SSH auf den Edgeknoten des Clusters zu. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Legen Sie sich nach der Verbindungsherstellung als Root-Benutzer im Cluster fest. Verwenden Sie in der SSH-Sitzung den folgenden Befehl:

        sudo su -

4. Laden Sie das benutzerdefinierte Skript zum Installieren von RStudio herunter. Verwenden Sie den folgenden Befehl:

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Ändern Sie die Berechtigungen in der benutzerdefinierten Skriptdatei, und führen Sie das Skript aus. Verwenden Sie die folgenden Befehle:

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Wenn Sie beim Erstellen eines HDInsight-Clusters mit R Server ein SSH-Kennwort verwendet haben, können Sie diesen Schritt überspringen und mit dem nächsten Schritt fortfahren. Wenn Sie stattdessen einen SSH-Schlüssel zum Erstellen des Clusters verwendet haben, müssen Sie ein Kennwort für Ihren SSH-Benutzer festlegen. Sie benötigen dieses Kennwort bei der Verbindungsherstellung mit RStudio. Führen Sie die folgenden Befehle aus:

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:


7. Wenn Sie aufgefordert werden, das **aktuelle Kerberos-Kennwort** einzugeben, drücken Sie die **EINGABETASTE**.  Hinweis: `USERNAME` muss durch einen SSH-Benutzer für Ihren HDInsight-Cluster ersetzt werden. Wenn Ihr Kennwort erfolgreich festgelegt wurde, sollte die folgende Meldung angezeigt werden:

        passwd: password updated successfully

    Beenden Sie die SSH-Sitzung.

8. Erstellen Sie einen SSH-Tunnel zum Cluster, indem Sie `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` im HDInsight-Cluster dem Clientcomputer zuordnen. Sie müssen einen SSH-Tunnel erstellen, bevor Sie eine neue Browsersitzung öffnen.

   * Öffnen Sie auf einem Linux-Client oder auf einem Windows-Client mit [Cygwin](http://www.redhat.com/services/custom/cygwin/) eine Terminalsitzung, und verwenden Sie den folgenden Befehl:

             ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

       Ersetzen Sie **USERNAME** durch einen SSH-Benutzer für Ihren HDInsight-Cluster und **CLUSTERNAME** durch den Namen des HDInsight-Clusters.
       Sie können anstelle eines Kennworts auch einen SSH-Schlüssel verwenden, indem Sie `-i id_rsa_key` hinzufügen.        
   * Bei Verwendung eines Windows-Clients mit PuTTY:

     1. Öffnen Sie PuTTY, und geben Sie die Verbindungsinformationen ein.
     2. Erweitern Sie auf der linken Seite des Dialogfelds im Abschnitt **Category** erst **Connection**, dann **SSH**, und wählen Sie anschließend **Tunnels** aus.
     3. Geben Sie die folgenden Informationen in das Formular **Options controlling SSH port forwarding** ein:

        * **Source port** : Der Port auf dem Client, den Sie weiterleiten möchten. Beispiel: **8787**.
        * **Destination** : Das Ziel, das dem lokalen Clientcomputer zugeordnet werden muss. Beispiel **localhost:8787**.

            ![Erstellen eines SSH-Tunnels](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Erstellen eines SSH-Tunnels")

     4. Klicken Sie auf **Add**, um die Einstellungen hinzuzufügen. Klicken Sie dann auf **Open**, um eine SSH-Verbindung zu öffnen.
     5. Wenn Sie dazu aufgefordert werden, melden Sie sich beim Server an, um eine SSH-Sitzung einzurichten und den Tunnel zu aktivieren.

9. Öffnen Sie einen Webbrowser, und geben Sie basierend auf dem Port, den Sie für den Tunnel eingegeben haben, die folgende URL ein:

        http://localhost:8787/ 

10. Sie werden aufgefordert, den SSH-Benutzernamen und das Kennwort zur Verbindungsherstellung mit dem Cluster einzugeben. Wenn Sie beim Erstellen des Clusters einen SSH-Schlüssel verwendet haben, müssen Sie das Kennwort eingeben, das Sie in Schritt 5 erstellt haben.

    ![Herstellen einer Verbindung mit R Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Erstellen eines SSH-Tunnels")

11. Um zu testen, ob die RStudio-Installation erfolgreich war, können Sie ein Testskript ausführen, mit dem R-basierte MapReduce- und Spark-Aufträge auf dem Cluster ausgeführt werden. Kehren Sie zur SSH-Konsole zurück, und geben Sie die folgenden Befehle ein, um das Testskript zur Ausführung in RStudio herunterzuladen:

    *    Wenn Sie einen Hadoop-Cluster mit R erstellt haben, verwenden Sie diesen Befehl:

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r
    *    Wenn Sie einen Spark-Cluster mit R erstellt haben, verwenden Sie diesen Befehl:

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

12. Sie sollten das heruntergeladene Testskript in RStudio sehen. Doppelklicken Sie auf die Datei, um sie zu öffnen, wählen Sie die Inhalte der Datei aus, und klicken Sie auf **Ausführen**. Die Ausgabe sollte im Bereich **Konsole** angezeigt werden.

   ![Testen der Installation](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Testen der Installation")

Alternativ können Sie `source(testhdi.r)` oder `source(testhdi_spark.r)` eingeben, um das Skript auszuführen.

## <a name="see-also"></a>Weitere Informationen

* [Rechenkontextoptionen für R Server auf HDInsight-Clustern](hdinsight-hadoop-r-server-compute-contexts.md)
* [Azure Storage-Optionen für R Server in HDInsight](hdinsight-hadoop-r-server-storage.md)


