---
title: "Verwenden eines SSH-Tunnels für den Zugriff auf Azure HDInsight | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie einen SSH-Tunnel verwenden, um auf Webressourcen auf Ihren Linux-basierten HDInsight-Knoten zuzugreifen.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 879834a4-52d0-499c-a3ae-8d28863abf65
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/30/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: 9f13e6300f77e2d9e84b0f7ce7f3cf289c327157
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="use-ssh-tunneling-to-access-ambari-web-ui-jobhistory-namenode-oozie-and-other-web-uis"></a>Verwenden von SSH-Tunneling zum Zugriff auf die Ambari-Webbenutzeroberfläche, JobHistory, NameNode, Oozie und andere Webbenutzeroberflächen

Linux-basierte HDInsight-Cluster bieten Zugriff auf die Ambari-Webbenutzeroberfläche über das Internet, dabei stehen aber einige Funktionen der Benutzeroberfläche nicht zur Verfügung. Dies gilt beispielsweise für die Webbenutzeroberflächen anderer Dienste, die nur durch Ambari dargestellt werden. Für eine vollständige Funktionalität der Ambari-Webbenutzeroberfläche müssen Sie einen SSH-Tunnel zum Clusterhauptknoten verwenden.

## <a name="why-use-an-ssh-tunnel"></a>Weshalb ist die Verwendung eines SSH-Tunnels empfehlenswert?

Einige der Menüs in Ambari funktionieren nur über einen SSH-Tunnel. Diese Menüs basieren auf Websites und Diensten, die auf anderen Knotentypen ausgeführt werden, z.B. Workerknoten. Diese Websites sind oft nicht gesichert, sodass es nicht sicher wäre, sie direkt im Internet verfügbar zu machen.

Die folgenden Webbenutzeroberflächen erfordern einen SSH-Tunnel:

* JobHistory
* NameNode
* Threadstapel
* Oozie-Webbenutzeroberfläche
* Benutzeroberfläche für HBase-Master und -Protokolle

Wenn Sie für die Clusteranpassung Skriptaktionen verwenden, benötigen Sie für alle Dienste und Dienstprogramme, die Sie installieren und die eine Webbenutzeroberfläche verfügbar machen, einen SSH-Tunnel. Bei der Installation von Hue mit einer Skriptaktion müssen Sie z. B. einen SSH-Tunnel verwenden, um auf die Hue-Webbenutzeroberfläche zuzugreifen.

## <a name="what-is-an-ssh-tunnel"></a>Was ist ein SSH-Tunnel?

[SSH-Tunneling (Secure Shell)](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) leitet den an einen Port gesendeten Datenverkehr an Ihre lokale Arbeitsstation weiter. Der Datenverkehr wird über eine SSH-Verbindung an Ihren HDInsight-Clusterhauptknoten weitergeleitet. Die Anforderung wird so aufgelöst, als ob sie vom Hauptknoten stammt. Die Antwort wird dann wieder durch den Tunnel an Ihre Arbeitsstation weitergeleitet.

## <a name="prerequisites"></a>Voraussetzungen

* Einen SSH-Client. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

* Einen Webbrowser, der für die Verwendung eines SOCKS5-Proxys konfiguriert werden kann.

    > [!WARNING]
    > Die in Windows integrierte SOCKS-Proxy-Unterstützung unterstützt nicht SOCKS5 und funktioniert nicht mit den Schritten in diesem Dokument. Die folgenden Browser basieren auf Windows-Proxyeinstellungen und funktionieren derzeit nicht mit den Schritten in diesem Dokument:
    >
    > * Microsoft Edge
    > * Microsoft Internet Explorer
    >
    > Google Chrome basiert auch auf den Windows-Proxyeinstellungen. Allerdings können Sie Erweiterungen installieren, die SOCKS5 unterstützen. Wir empfehlen [FoxyProxy Standard](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).

## <a name="usessh"></a>Erstellen von Tunneln mit dem Befehl "ssh"

Verwenden Sie den folgenden Befehl zum Erstellen eines SSH-Tunnels mithilfe des Befehls `ssh` . Ersetzen Sie **USERNAME** durch einen SSH-Benutzer für Ihren HDInsight-Cluster und **CLUSTERNAME** durch den Namen des HDInsight-Clusters:

```bash
ssh -C2qTnNf -D 9876 USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
```

Durch diesen Befehl wird eine Verbindung erstellt, über die der Datenverkehr über SSH an den lokalen Port 9876 des Clusters weitergeleitet wird. Die Optionen sind:

* **D 9876**: Der lokale Port, der den Datenverkehr durch den Tunnel weiterleitet.
* **C** : Alle Daten werden komprimiert, da der Webdatenverkehr hauptsächlich aus Text besteht.
* **2** : SSH zwingen, nur Protokollversion 2 zu verwenden.
* **q** : Stiller Modus.
* **T** : Pseudo-TTY-Zuordnung deaktivieren, da lediglich ein Port weitergeleitet wird.
* **n:** verhindert den Lesevorgang für STDIN, da lediglich ein Port weitergeleitet wird
* **N** : Keine Remotebefehle ausführen, da lediglich ein Port weitergeleitet wird.
* **f** : Im Hintergrund ausführen.

Nach Abschluss des Befehls wird der an den Port 9876 des lokalen Computers gesendete Datenverkehr an den Hauptknoten des Clusters weitergeleitet.

## <a name="useputty"></a>Erstellen von Tunneln mit PuTTY

[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty) ist ein SSH-Client für Windows mit grafischer Benutzeroberfläche. Führen Sie die folgenden Schritte aus, um mithilfe von PuTTY einen SSH-Tunnel zu erstellen:

1. Öffnen Sie PuTTY, und geben Sie die Verbindungsinformationen ein. Wenn Sie nicht mit PuTTY vertraut sind, lesen Sie die [PuTTY-Dokumentation (http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html)](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).

2. Erweitern Sie auf der linken Seite des Dialogfelds im Abschnitt **Category** erst **Connection**, dann **SSH**, und wählen Sie anschließend **Tunnels** aus.

3. Geben Sie die folgenden Informationen in das Formular **Options controlling SSH port forwarding** ein:
   
   * **Source port** : Der Port auf dem Client, den Sie weiterleiten möchten. Beispiel: **9876**.

   * **Destination** : Die SSH-Adresse des Linux-basierten HDInsight-Clusters. Beispiel: **mycluster-ssh.azurehdinsight.net**.

   * **Dynamic** : Ermöglicht das dynamische SOCKS-Proxyrouting.
     
     ![Abbildung von Tunneloptionen](./media/hdinsight-linux-ambari-ssh-tunnel/puttytunnel.png)

4. Klicken Sie auf **Add**, um die Einstellungen hinzuzufügen. Klicken Sie dann auf **Open**, um eine SSH-Verbindung zu öffnen.

5. Melden Sie sich bei entsprechender Aufforderung am Server an.

## <a name="use-the-tunnel-from-your-browser"></a>Verwenden des Tunnels im Browser

> [!IMPORTANT]
> Die Schritte in diesem Abschnitt verwenden den Mozilla FireFox-Browser, da dieser auf allen Plattformen die gleichen Proxyeinstellungen bietet. Andere moderne Browser wie z.B. Google Chrome erfordern möglicherweise eine Erweiterung wie z.B. FoxyProxy, um mit dem Tunnel zu arbeiten.

1. Konfigurieren Sie den Browser so, dass er **localhost** sowie den Port verwendet, den Sie bei der Erstellung des Tunnels als **SOCKS v5**-Proxy verwendet haben. Die Firefox-Einstellungen sollten wie folgt aussehen. Wenn Sie einen anderen Port als 9876 verwenden, ändern Sie den Port entsprechend:
   
    ![Abbildung von Firefox-Einstellungen](./media/hdinsight-linux-ambari-ssh-tunnel/firefoxproxy.png)
   
   > [!NOTE]
   > Durch die Auswahl von **Remote-DNS** werden DNS-Anforderungen (Domain Name System) mithilfe des HDInsight-Clusters aufgelöst. Diese Einstellung löst DNS mit dem Hauptknoten des Clusters auf.

2. Überprüfen Sie, ob der Tunnel funktioniert, indem Sie eine Website wie [http://www.whatismyip.com/](http://www.whatismyip.com/) aufrufen. Wenn der Proxy richtig konfiguriert wurde, stammt die zurückgegebene IP-Adresse von einem Computer im Microsoft Azure-Rechenzentrum.

## <a name="verify-with-ambari-web-ui"></a>Überprüfen der Ambari-Webbenutzeroberfläche

Nachdem der Cluster eingerichtet wurde, gehen Sie folgendermaßen vor, um zu überprüfen, ob Sie von der Ambari-Webbenutzeroberfläche auf Dienst-Webbenutzeroberflächen zugreifen können:

1. Öffnen Sie in Ihrem Browser http://headnodehost:8080 . Die `headnodehost`-Adresse wird durch den Tunnel an den Cluster gesendet und in den Hauptknoten aufgelöst, auf dem Ambari ausgeführt wird. Geben Sie bei der entsprechenden Aufforderung den Benutzernamen ("admin") und das Kennwort des Administrators für den Cluster ein. Sie werden eventuell von der Ambari-Webbenutzeroberfläche ein zweites Mal zur Eingabe aufgefordert. Geben Sie in diesem Fall die Informationen erneut ein.

   > [!NOTE]
   > Wenn Sie die Adresse http://headnodehost:8080 für die Verbindung mit dem Cluster verwenden, wird diese über den Tunnel hergestellt. Die Kommunikation wird durch den SSH-Tunnel und nicht per HTTPS geschützt. Verwenden Sie zum Herstellen einer Verbindung über das Internet per HTTPS die Adresse „https://CLUSTERNAME.azurehdinsight.net“, wobei **CLUSTERNAME** der Name des Clusters ist.

2. Wählen Sie auf der Ambari-Webbenutzeroberfläche aus der Liste links auf der Seite „HDFS“ aus.

    ![Abbildung mit ausgewähltem HDFS-Dienst](./media/hdinsight-linux-ambari-ssh-tunnel/hdfsservice.png)

3. Wenn die Informationen zum HDFS-Dienst angezeigt werden, wählen Sie **QuickLinks**aus. Eine Liste der Clusterhauptknoten wird angezeigt. Wählen Sie einen der Hauptknoten und dann **NameNode UI**aus.

    ![Bild mit erweitertem Menü "QuickLinks"](./media/hdinsight-linux-ambari-ssh-tunnel/namenodedropdown.png)

   > [!NOTE]
   > Bei Auswahl von __Quicklinks__ wird möglicherweise eine Benachrichtigung zum Warten angezeigt. Dies kann auftreten, wenn Sie über eine langsame Internetverbindung verfügen. Warten Sie eine oder zwei Minuten auf den Empfang der Daten vom Server, und wiederholen Sie dann das Auflisten.
   >
   > Einige Einträge im Menü **Quicklinks** können an der rechten Seite des Bildschirms abgeschnitten sein. Erweitern Sie in diesem Fall das Menü mit der Maus, und verwenden Sie die Nach-Rechts-Taste, um nach rechts zu scrollen und das restliche Menü anzuzeigen.

4. Eine Seite ähnlich der folgenden Abbildung wird angezeigt:

    ![Abbildung der NameNode-Benutzeroberfläche](./media/hdinsight-linux-ambari-ssh-tunnel/namenode.png)

   > [!NOTE]
   > Beachten Sie die URL für diese Seite. Sie sollte **http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/cluster** oder ähnlich lauten. Bei diesem URI wird der interne vollqualifizierte Domänenname (FQDN) des Knotens verwendet, auf den nur über einen SSH-Tunnel zugegriffen werden kann.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie einen SSH-Tunnel erstellen und verwenden, finden Sie im folgenden Dokument weitere Möglichkeiten für die Verwendung von Ambari:

* [Verwalten von HDInsight-Clustern mit Ambari](hdinsight-hadoop-manage-ambari.md)

Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter [Verwenden von SSH mit HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).


