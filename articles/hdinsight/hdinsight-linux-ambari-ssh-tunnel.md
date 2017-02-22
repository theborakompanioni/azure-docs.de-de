---
title: "Verwenden eines SSH-Tunnels für den Zugriff auf Azure HDInsight-Dienste | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie einen SSH-Tunnel verwenden, um auf Webressourcen auf Ihren Linux-basierten HDInsight-Knoten zuzugreifen.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 879834a4-52d0-499c-a3ae-8d28863abf65
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/08/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 5ec4b964066687b506686709c3dc5ed5b402fbaf
ms.openlocfilehash: 8045f9d927e9c877573085eb43eaadcd60f96a67


---
# <a name="use-ssh-tunneling-to-access-ambari-web-ui-jobhistory-namenode-oozie-and-other-web-uis"></a>Verwenden von SSH-Tunneling zum Zugriff auf die Ambari-Webbenutzeroberfläche, JobHistory, NameNode, Oozie und andere Webbenutzeroberflächen

Linux-basierte HDInsight-Cluster bieten Zugriff auf die Ambari-Webbenutzeroberfläche über das Internet, dabei stehen aber einige Funktionen der Benutzeroberfläche nicht zur Verfügung. Dies gilt beispielsweise für die Webbenutzeroberflächen anderer Dienste, die nur durch Ambari dargestellt werden. Für eine vollständige Funktionalität der Ambari-Webbenutzeroberfläche müssen Sie einen SSH-Tunnel zum Clusterhauptknoten verwenden.

## <a name="why-use-an-ssh-tunnel"></a>Weshalb ist die Verwendung eines SSH-Tunnels empfehlenswert?

Einige Menüs in Ambari werden ohne einen SSH-Tunnel nicht vollständig aufgefüllt, da sie Websites und Dienste benötigen, die von anderen im Cluster ausgeführten Hadoop-Diensten verfügbar gemacht werden. Diese Websites sind oft nicht gesichert, sodass es nicht sicher wäre, sie direkt im Internet verfügbar zu machen. In einigen Fällen führt der Dienst die Website auf einem anderen Clusterknoten aus, z. B. auf einem Zookeeper-Knoten.

Im Folgenden finden Sie die Dienste, die von der Ambari-Webbenutzeroberfläche verwendet werden und auf die nur über einen SSH-Tunnel zugegriffen werden kann:

* JobHistory
* NameNode
* Threadstapel
* Oozie-Webbenutzeroberfläche
* Benutzeroberfläche für HBase-Master und -Protokolle

Wenn Sie für die Clusteranpassung Skriptaktionen verwenden, benötigen Sie für alle Dienste und Dienstprogramme, die Sie installieren und die eine Webbenutzeroberfläche verfügbar machen, einen SSH-Tunnel. Bei der Installation von Hue mit einer Skriptaktion müssen Sie z. B. einen SSH-Tunnel verwenden, um auf die Hue-Webbenutzeroberfläche zuzugreifen.

## <a name="what-is-an-ssh-tunnel"></a>Was ist ein SSH-Tunnel?

[Secure Shell (SSH)-Tunneling](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) leitet Datenverkehr an einen Port auf der lokalen Arbeitsstation über eine SSH-Verbindung zu Ihrem HDInsight-Clusterhauptknoten weiter, auf dem die Anforderung dann so aufgelöst wird, als ob sie vom Hauptknoten stammt. Die Antwort wird dann wieder durch den Tunnel an Ihre Arbeitsstation weitergeleitet.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen für die Verwendung eines SSH-Tunnels für Webdatenverkehr Folgendes:

* Einen SSH-Client. Für Linux- und Unix-Distributionen sowie für Macintosh OS X und Bash unter Windows 10 steht der Befehl `ssh` über das Betriebssystem zur Verfügung. Für Windows-Versionen, die den Befehl `ssh` nicht enthalten, empfehlen wir [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
  
  > [!NOTE]
  > Wenn Sie einen anderen SSH-Client als `ssh` oder PuTTY verwenden möchten, finden Sie Informationen zum Herstellen eines SSH-Tunnels in der Dokumentation zu Ihrem Client.

* Einen Webbrowser, der für die Verwendung eines SOCKS-Proxys konfiguriert werden kann.

## <a name="a-nameusesshacreate-a-tunnel-using-the-ssh-command"></a><a name="usessh"></a>Erstellen von Tunneln mit dem Befehl "ssh"

Verwenden Sie den folgenden Befehl zum Erstellen eines SSH-Tunnels mithilfe des Befehls `ssh` . Ersetzen Sie **USERNAME** durch einen SSH-Benutzer für Ihren HDInsight-Cluster und **CLUSTERNAME** durch den Namen des HDInsight-Clusters.

```
ssh -C2qTnNf -D 9876 USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
```

Dadurch wird eine Verbindung erstellt, über die der Datenverkehr über SSH an den lokalen Port 9876 des Clusters weitergeleitet wird. Die Optionen sind:

* **D 9876**: Der lokale Port, der den Datenverkehr durch den Tunnel weiterleitet.
* **C** : Alle Daten werden komprimiert, da der Webdatenverkehr hauptsächlich aus Text besteht.
* **2** : SSH zwingen, nur Protokollversion 2 zu verwenden.
* **q** : Stiller Modus.
* **T** : Pseudo-TTY-Zuordnung deaktivieren, da lediglich ein Port weitergeleitet wird.
* **n** : Verhindert den Lesevorgang für STDIN, da lediglich ein Port weitergeleitet wird.
* **N** : Keine Remotebefehle ausführen, da lediglich ein Port weitergeleitet wird.
* **f** : Im Hintergrund ausführen.

Wenn Sie den Cluster mit einem SSH-Schlüssel konfiguriert haben, müssen Sie unter Umständen den Parameter `-i` verwenden und den Pfad zum privaten SSH-Schlüssel angeben.

Nach Abschluss des Befehls wird der an den lokalen Port 9876 des lokalen Computers gesendete Datenverkehr über SSL (Secure Sockets Layer) an den Hauptknoten des Clusters weitergeleitet, der dann seinen Ursprung darstellt.

## <a name="a-nameuseputtyacreate-a-tunnel-using-putty"></a><a name="useputty"></a>Erstellen von Tunneln mit PuTTY

Führen Sie die folgenden Schritte aus, um einen SSH-Tunnel mithilfe von PuTTY zu erstellen.

1. Öffnen Sie PuTTY, und geben Sie die Verbindungsinformationen ein. Wenn Sie nicht mit PuTTY vertraut sind, finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md) Informationen für die Verwendung mit HDInsight.

2. Erweitern Sie auf der linken Seite des Dialogfelds im Abschnitt **Category** erst **Connection**, dann **SSH**, und wählen Sie anschließend **Tunnels** aus.

3. Geben Sie die folgenden Informationen in das Formular **Options controlling SSH port forwarding** ein:
   
   * **Source port** : Der Port auf dem Client, den Sie weiterleiten möchten. Beispiel: **9876**.

   * **Destination** : Die SSH-Adresse des Linux-basierten HDInsight-Clusters. Beispiel: **mycluster-ssh.azurehdinsight.net**.

   * **Dynamic** : Ermöglicht das dynamische SOCKS-Proxyrouting.
     
     ![Abbildung von Tunneloptionen](./media/hdinsight-linux-ambari-ssh-tunnel/puttytunnel.png)

4. Klicken Sie auf **Add**, um die Einstellungen hinzuzufügen. Klicken Sie dann auf **Open**, um eine SSH-Verbindung zu öffnen.

5. Melden Sie sich bei entsprechender Aufforderung am Server an. Dadurch wird eine SSH-Sitzung eingerichtet und der Tunnel aktiviert.

## <a name="use-the-tunnel-from-your-browser"></a>Verwenden des Tunnels im Browser

> [!NOTE]
> Für die Schritte in diesem Abschnitt wird der Firefox-Browser verwendet, der für Linux, Unix, Macintosh OS X und Windows-Systeme frei verfügbar ist. Andere moderne Browser, die die Verwendung eines SOCKS-Proxys unterstützen, funktionieren ebenfalls.

1. Konfigurieren Sie den Browser so, dass er **localhost** sowie den Port verwendet, den Sie bei der Erstellung des Tunnels als **SOCKS v5**-Proxy verwendet haben. Die Firefox-Einstellungen sollten wie folgt aussehen. Wenn Sie einen anderen Port als 9876 verwenden, ändern Sie den Port entsprechend:
   
    ![Abbildung von Firefox-Einstellungen](./media/hdinsight-linux-ambari-ssh-tunnel/firefoxproxy.png)
   
   > [!NOTE]
   > Durch die Auswahl von **Remote-DNS** werden DNS-Anforderungen (Domain Name System) mithilfe des HDInsight-Clusters aufgelöst. Ist diese Option deaktiviert, wird DNS lokal aufgelöst.

2. Überprüfen Sie, ob Datenverkehr durch den Tunnel weitergeleitet wird, indem Sie eine Website wie [http://www.whatismyip.com/](http://www.whatismyip.com/) mit aktivierten und deaktivierten Proxyeinstellungen in Firefox aufrufen. Wenn diese Einstellungen aktiviert sind, stammt die zurückgegebene IP-Adresse von einem Computer im Microsoft Azure-Rechenzentrum.

## <a name="verify-with-ambari-web-ui"></a>Überprüfen der Ambari-Webbenutzeroberfläche

Nachdem der Cluster eingerichtet wurde, gehen Sie folgendermaßen vor, um zu überprüfen, ob Sie von der Ambari-Webbenutzeroberfläche auf Dienst-Webbenutzeroberflächen zugreifen können:

1. Öffnen Sie in Ihrem Browser „http://headnodehost:8080“. Die `headnodehost`-Adresse wird durch den Tunnel an den Cluster gesendet und in den Hauptknoten aufgelöst, auf dem Ambari ausgeführt wird. Geben Sie bei der entsprechenden Aufforderung den Benutzernamen ("admin") und das Kennwort des Administrators für den Cluster ein. Sie werden eventuell von der Ambari-Webbenutzeroberfläche ein zweites Mal zur Eingabe aufgefordert. Geben Sie in diesem Fall die Informationen erneut ein.
   
   > [!NOTE]
   > Bei Verwendung der Adresse „http://headnodehost:8080“ zum Herstellen der Verbindung mit dem Cluster wird die Verbindung direkt über den Tunnel mit dem Hauptknoten hergestellt, auf dem Ambari ausgeführt wird. Hierfür wird HTTP genutzt, und die Kommunikation ist mit dem SSH-Tunnel geschützt. Bei einer Verbindung über das Internet ohne Tunnel wird die Kommunikation per HTTPS geschützt. Verwenden Sie zum Herstellen einer Verbindung über das Internet per HTTPS die Adresse „https://CLUSTERNAME.azurehdinsight.net“, wobei **CLUSTERNAME** der Name des Clusters ist.

2. Wählen Sie auf der Ambari-Webbenutzeroberfläche aus der Liste links auf der Seite „HDFS“ aus.
   
    ![Abbildung mit ausgewähltem HDFS-Dienst](./media/hdinsight-linux-ambari-ssh-tunnel/hdfsservice.png)
3. Wenn die Informationen zum HDFS-Dienst angezeigt werden, wählen Sie **QuickLinks**aus. Eine Liste der Clusterhauptknoten wird angezeigt. Wählen Sie einen der Hauptknoten und dann **NameNode UI**aus.
   
    ![Bild mit erweitertem Menü "QuickLinks"](./media/hdinsight-linux-ambari-ssh-tunnel/namenodedropdown.png)
   
   > [!NOTE]
   > Wenn Sie über eine langsame Internetverbindung verfügen oder der Hauptknoten stark ausgelastet ist, wird möglicherweise anstelle eines Menüs eine Benachrichtigung zum Warten angezeigt, wenn Sie **QuickLinks**auswählen. Warten Sie in diesem Fall eine oder zwei Minuten auf den Empfang der Daten vom Server, und wiederholen Sie dann das Auflisten.
   > 
   > Wenn Sie einen Monitor mit niedriger Auflösung verwenden oder das Browserfenster nicht maximiert ist, werden einige Einträge im Menü **QuickLinks** möglicherweise auf der rechten Seite des Bildschirms abgeschnitten. Erweitern Sie in diesem Fall das Menü mit der Maus, und verwenden Sie die Nach-Rechts-Taste, um nach rechts zu scrollen und das restliche Menü anzuzeigen.
   > 
   > 
4. Eine Seite ähnlich der folgenden wird angezeigt:
   
    ![Abbildung der NameNode-Benutzeroberfläche](./media/hdinsight-linux-ambari-ssh-tunnel/namenode.png)
   
   > [!NOTE]
   > Beachten Sie die URL für diese Seite. Sie sollte **http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/cluster** oder ähnlich lauten. Dabei wird der interne vollständig qualifizierte Domänenname (FQDN) des Knotens verwendet, auf den nicht ohne Verwendung eines SSH-Tunnels zugegriffen werden kann.
   > 
   > 

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie erfahren haben, wie Sie einen SSH-Tunnel erstellen und verwenden, finden Sie in den folgenden Ressourcen Informationen zum Überwachen und Verwalten des Clusters mithilfe von Ambari:

* [Verwalten von HDInsight-Clustern mit Ambari](hdinsight-hadoop-manage-ambari.md)

Weitere Informationen zur Verwendung von SSH mit HDInsight finden Sie in den folgenden Artikeln:

* [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)




<!--HONumber=Feb17_HO2-->


