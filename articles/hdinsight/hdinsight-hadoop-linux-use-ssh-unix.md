---
title: Verwenden von SSH mit HDInsight (Hadoop) unter Windows, Linux, Unix oder OS X | Microsoft-Dokumentation
description: " Sie können auf HDInsight über Secure Shell (SSH) zugreifen. Dieses Dokument enthält Informationen zur Verwendung von SSH mit HDInsight auf Windows-, Linux-, Unix- oder OS X-Clients.."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: a6a16405-a4a7-4151-9bbf-ab26972216c5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 279990a67ae260b09d056fd84a12160150eb4539
ms.openlocfilehash: 37409ad3f50cdd4a7a384c96a57a35ef8c83fb8f


---
# <a name="use-ssh-with-hdinsight-hadoop-from-windows-linux-unix-or-os-x"></a>Verwenden von SSH mit HDInsight (Hadoop) unter Windows, Linux, Unix oder OS X

> [!div class="op_single_selector"]
> * [PuTTY (Windows)](hdinsight-hadoop-linux-use-ssh-windows.md)
> * [SSH (Windows, Linux, Unix, OS X)](hdinsight-hadoop-linux-use-ssh-unix.md)

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) ermöglicht die Anmeldung bei einem Linux-basierten HDInsight-Cluster und die Ausführung von Befehlen über eine Befehlszeilenschnittstelle. Dieses Dokument enthält grundlegende Informationen zu SSH sowie spezifische Informationen zur Verwendung von SSH mit HDInsight.

## <a name="what-is-ssh"></a>Was ist SSH?

SSH ist ein kryptografisches Netzwerkprotokoll für die sichere Kommunikation mit einem Remoteserver über ein unsicheres Netzwerk. SSH ermöglicht eine sichere befehlszeilenbasierte Anmeldung bei einem Remoteserver. In diesem Fall: die Hauptknoten oder der Edgeknoten eines HDInsight-Clusters.

Mit SSH können Sie Netzwerkdatenverkehr von Ihrem Client auch über einen Tunnel an den HDInsight-Cluster übermitteln. Bei Verwendung eines Tunnels stehen Ihnen im HDInsight-Cluster Dienste zur Verfügung, auf die nicht direkt über das Internet zugegriffen werden kann. Weitere Informationen zum Verwenden von SSH-Tunneln mit HDInsight finden Sie unter [Verwenden von SSH-Tunneling mit HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="ssh-clients"></a>SSH-Clients

Viele Betriebssysteme stellen SSH-Clientfunktionen über die Befehlszeilen-Hilfsprogramme `ssh` und `scp` bereit.

* __ssh__: Ein allgemeiner SSH-Client zum Einrichten einer Befehlszeilen-Remotesitzung und zum Erstellen von Tunneln.
* __scp__: Ein Hilfsprogramm, das Dateien unter Verwendung des SSH-Protokolls zwischen lokalen Systemen und Remotesystemen kopiert.

Vor Windows 10 Anniversary Edition wurde von Windows kein SSH-Client bereitgestellt. Diese Windows-Version enthält das Entwicklerfeature „Bash unter Windows 10“ und stellt somit `ssh`, `scp` und andere Linux-Befehle bereit. Weitere Informationen zur Verwendung von Bash unter Windows 10 finden Sie unter [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about) (Bash unter Ubuntu unter Windows).

Windows-Benutzern ohne Zugriff auf Bash unter Windows 10 können wir folgende SSH-Clients empfehlen:

* [Git für Windows](https://git-for-windows.github.io/): Stellt die Befehlszeilen-Hilfsprogramme `ssh` und `scp` bereit.
* [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/): Stellt einen grafischen SSH-Client bereit.
* [MobaXterm](http://mobaxterm.mobatek.net/): Stellt einen grafischen SSH-Client bereit.
* [Cygwin](https://cygwin.com/): Stellt die Befehlszeilen-Hilfsprogramme `ssh` und `scp` bereit.

> [!NOTE]
> Für die Schritte in diesem Dokument benötigen Sie Zugriff auf den Befehl `ssh`. Informieren Sie sich bei Verwendung eines Clients wie PuTTY oder MobaXterm in der Dokumentation des verwendeten Produkts über entsprechende Befehle und Parameter.

## <a name="ssh-authentication"></a>SSH-Authentifizierung

Eine SSH-Verbindung kann entweder mithilfe eines Kennworts oder unter Verwendung der Verschlüsselung mit öffentlichem Schlüssel ([Public-Key-Verschlüsselungsverfahren (https://de.wikipedia.org/wiki/Public-Key-Verschl%C3%BCsselungsverfahren)](https://en.wikipedia.org/wiki/Public-key_cryptography)) authentifiziert werden. Die Verwendung eines Schlüssels ist die sicherste Methode, da sie gegen viele der Angriffe immun ist, für die Kennwörter anfällig sind. Die Erstellung und Verwaltung von Schlüsseln ist jedoch komplizierter als die Verwendung eines Kennworts.

Wenn Sie die Kryptografie mit öffentlichem Schlüssel verwenden möchten, müssen Sie ein Paar aus einem _öffentlichen_ und einem _privaten_ Schlüssel erstellen.

* Der **öffentliche Schlüssel** wird auf die Knoten Ihres HDInsight-Clusters oder in einen anderen Dienst geladen, den Sie für die Verschlüsselung mit öffentlichem Schlüssel verwenden möchten.

* Der **private Schlüssel** wird dem HDInsight-Cluster bei der Anmeldung über einen SSH-Client präsentiert, um Ihre Identität zu bestätigen. Bewahren Sie diesen privaten Schlüssel sicher auf. Geben Sie ihn nicht weiter.

    Durch Erstellen einer Passphrase für den privaten Schlüssel können Sie die Sicherheit noch weiter erhöhen. Diese Passphrase muss angegeben werden, damit der Schlüssel verwendet werden kann.

### <a name="create-a-public-and-private-key"></a>Erstellen eines öffentlichen und eines privaten Schlüssels

Am einfachsten erstellen Sie ein Paar aus öffentlichem und privatem Schlüssel für HDInsight mit dem Hilfsprogramm `ssh-keygen`. Verwenden Sie an einer Befehlszeile den folgenden Befehl, um ein neues Schlüsselpaar für die Verwendung mit HDInsight zu erstellen:

> [!NOTE]
> Informieren Sie sich bei Verwendung eines SSH-Clients mit grafischer Benutzeroberfläche (beispielsweise MobaXterm oder PuTTY) in der Dokumentation Ihres Clients über die Vorgehensweise zum Generieren von Schlüsseln.

    ssh-keygen -t rsa -b 2048

Folgende Informationen werden abgefragt:

* Speicherort der Datei: Dies ist standardmäßig `~/.ssh/id_rsa`.

* Optionale Passphrase: Wenn Sie eine Passphrase eingeben, müssen Sie diese bei der Authentifizierung gegenüber Ihrem HDInsight-Cluster erneut eingeben.

> [!IMPORTANT]
> Die Passphrase ist ein Kennwort für den privaten Schlüssel. Sie muss jedes Mal angegeben werden, wenn der private Schlüssel zur Authentifizierung verwendet wird. Andernfalls kann der Schlüssel nicht verwendet werden. Sollte jemand in den Besitz Ihres privaten Schlüssel gelangen, kann diese Person den Schlüssel nur mit der Passphrase verwenden.
>
> Falls Sie die Passphrase vergessen, gibt es keine Möglichkeit, sie zurückzusetzen oder wiederherzustellen.

Nach Ausführung des Befehls verfügen Sie über zwei neue Dateien:

* __id\_rsa__: Diese Datei enthält den privaten Schlüssel.

    > [!WARNING]
    > Beschränken Sie den Zugriff auf diese Datei, um unberechtigte Zugriffe auf Dienste zu verhindern, die mit dem öffentlichen Schlüssel geschützt werden.

* __id\_rsa.pub__: Diese Datei enthält den öffentlichen Schlüssel. Sie wird beim Erstellen eines HDInsight-Clusters verwendet.

    > [!NOTE]
    > Der Zugriff auf den _öffentlichen_ Schlüssel muss nicht beschränkt werden. Er dient lediglich zur Verifizierung des privaten Schlüssels. Dienste wie der SSH-Server verwenden den öffentlichen Schlüssel zur Überprüfung Ihrer Identität, wenn Sie sich mit dem privaten Schlüssel authentifizieren.

## <a name="configure-ssh-on-hdinsight"></a>Konfigurieren von SSH für HDInsight

Wenn Sie einen Linux-basierten HDInsight-Cluster erstellen möchten, müssen Sie einen _SSH-Benutzernamen_ und entweder ein _Kennwort_ oder einen _öffentlichen Schlüssel_ angeben. Diese Informationen werden im Rahmen der Clustererstellung verwendet, um eine Anmeldung auf den HDInsight-Clusterknoten zu erstellen. Das Kennwort oder der öffentliche Schlüssel dient zum Schutz des Benutzerkontos.

Weitere Informationen zum Konfigurieren von SSH im Rahmen der Clustererstellung finden Sie in den folgenden Dokumenten:

* [Erstellen von Linux-basierten Clustern in HDInsight mithilfe des Azure-Portals](hdinsight-hadoop-create-linux-clusters-portal.md)
* [Erstellen von Linux-basierten Clustern in HDInsight mithilfe der Azure-Befehlszeilenschnittstelle](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
* [Erstellen von Linux-basierten Clustern in HDInsight mit Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Erstellen Linux-basierter Hadoop-Cluster in HDInsight mithilfe von Azure Resource Manager-Vorlagen](hdinsight-hadoop-create-linux-clusters-arm-templates.md)
* [Erstellen von Linux-basierten Clustern in HDInsight mit dem .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* [Erstellen von Linux-basierten Clustern in HDInsight mithilfe von cURL und der Azure-REST-API](hdinsight-hadoop-create-linux-clusters-curl-rest.md)

### <a name="additional-ssh-users"></a>Zusätzliche SSH-Benutzer

Es ist zwar möglich, einem Cluster nach der Erstellung weitere SSH-Benutzer hinzuzufügen, dies wird jedoch nicht empfohlen.

* Die neuen SSH-Benutzer müssen jedem Knoten im Cluster manuell hinzugefügt werden.

* Neue SSH-Benutzer haben den gleichen Zugriff auf HDInsight wie Standardbenutzer. Der Zugriff auf Daten oder Aufträge in HDInsight kann nicht auf der Grundlage des SSH-Benutzerkontos eingeschränkt werden.

Wenn Sie den Zugriff für einzelne Benutzer einschränken möchten, müssen Sie einen in die Domäne eingebundenen HDInsight-Cluster verwenden. Bei in die Domäne eingebundenen HDInsight-Clustern wird der Zugriff auf Clusterressourcen mithilfe von Active Directory gesteuert.

Wenn Sie einen in die Domäne eingebundenen HDInsight-Cluster verwenden, können Sie die Authentifizierung nach dem Herstellen der SSH-Verbindung über Active Directory durchführen. Mehrere Benutzer können eine SSH-Verbindung herstellen und sich anschließend bei ihrem Active Directory-Konto authentifizieren. Weitere Informationen finden Sie im Abschnitt [In die Domäne eingebundenes HDInsight](#domainjoined).

##<a name="a-idconnecta-connect-to-hdinsight"></a><a id="connect"></a> Herstellen einer Verbindung mit HDInsight

Die Knoten in einem HDInsight-Cluster werden zwar alle auf dem SSH-Server ausgeführt, Sie können jedoch nur eine öffentliche Internetverbindung mit den Haupt- oder Edgeknoten herstellen.

* Wenn Sie eine Verbindung mit den _Hauptknoten_ herstellen möchten, verwenden Sie `CLUSTERNAME-ssh.azurehdinsight.net`, und ersetzen Sie dabei __CLUSTERNAME__ durch den Namen des HDInsight-Clusters. Über Port 22 (Standardeinstellung für SSH) wird eine Verbindung mit dem primären Hauptknoten hergestellt. Über Port 23 wird eine Verbindung mit dem sekundären Hauptknoten hergestellt.

* Wenn Sie eine Verbindung mit einem _Edgeknoten_ herstellen möchten, verwenden Sie `EDGENAME.CLUSTERNAME-ssh.azurehdinsight.net`, und ersetzen Sie dabei __EDGENAME__ durch den Namen des Edgeknotens und __CLUSTERNAME__ durch den Namen des HDInsight-Clusters. Verwenden Sie beim Herstellen einer Verbindung mit dem Edgeknoten den Port 22.

In den folgenden Beispielen wird veranschaulicht, wie Sie unter Verwendung des SSH-Benutzernamens __sshuser__ eine Verbindung mit den Hauptknoten und dem Edgeknoten eines Clusters namens __myhdi__ herstellen. Der Name des Edgeknotens lautet __myedge__.

| Maßnahme | Option |
| ----- | ----- |
| Herstellen einer Verbindung mit dem primären Hauptknoten | `ssh sshuser@myhdi-ssh.azurehdinsight.net` |
| Herstellen einer Verbindung mit dem sekundären Hauptknoten | `ssh -p 23 sshuser@myhdi-ssh.azurehdinsight.net` |
| Herstellen einer Verbindung mit dem Edgeknoten | `ssh sshuser@edge.myhdi-ssh.azurehdinsight.net` |

Wenn Sie zum Schutz des SSH-Kontos ein Kennwort verwenden, werden Sie zur Eingabe dieses Kennworts aufgefordert.

Wenn Sie zum Schutz des SSH-Kontos einen öffentlichen Schlüssel verwenden, müssen Sie unter Umständen mithilfe des Schalters `-i` den Pfad zum entsprechenden privaten Schlüssel angeben. Das folgende Beispiel veranschaulicht die Verwendung des Schalters `-i`:

    ssh -i /path/to/public.key sshuser@myhdi-ssh.azurehdinsight.net

### <a name="connect-to-other-nodes"></a>Herstellen einer Verbindung mit anderen Knoten

Auf die Workerknoten und die Zookeeper-Knoten kann von außerhalb des Clusters nicht direkt zugegriffen werden. Der Zugriff ist jedoch über die Haupt- oder Edgeknoten des Clusters möglich. Im Anschluss finden Sie die allgemeinen Schritte hierfür:

1. Herstellen einer SSH-Verbindung mit einem Haupt- oder Edgeknoten:

        ssh sshuser@myhdi-ssh.azurehdinsight.net

2. Verwenden Sie für die SSH-Verbindung mit dem Haupt- oder Edgeknoten den Befehl `ssh`, um eine Verbindung mit einem Workerknoten im Cluster herzustellen:

        ssh sshuser@wn0-myhdi

    Informationen zum Abrufen einer Liste mit den Workerknoten im Cluster finden Sie im Dokument [Verwalten von HDInsight-Clustern mithilfe der Ambari-REST-API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) im Beispiel zum Abrufen des vollqualifizierten Domänennamens von Clusterknoten.

Ist das SSH-Konto durch ein Kennwort geschützt, werden Sie zur Eingabe dieses Kennworts aufgefordert, und die Verbindung wird hergestellt.

Wenn Sie Ihr Benutzerkonto mithilfe eines SSH-Schlüssels authentifizieren, muss Ihre lokale Umgebung für die SSH-Agent-Weiterleitung konfiguriert sein.

> [!IMPORTANT]
> In den folgenden Schritten wird davon ausgegangen, dass Sie ein Linux-/UNIX-basiertes System und Bash unter Windows 10 verwenden. Falls diese Schritte für Ihr System nicht geeignet sind, ziehen Sie die Dokumentation für Ihren SSH-Client zurate.

1. Öffnen Sie `~/.ssh/config`in einem Text-Editor. Sollte die Datei nicht vorhanden sein, können Sie sie durch Eingabe von `touch ~/.ssh/config` an einer Befehlszeile erstellen.

2. Fügen Sie der Datei folgenden Code hinzu. Ersetzen Sie *CLUSTERNAME* durch den Namen Ihres HDInsight-Clusters.

        Host CLUSTERNAME-ssh.azurehdinsight.net
          ForwardAgent yes

    Dadurch wird die SSH-Agent-Weiterleitung für Ihren HDInsight-Cluster konfiguriert.

3. Testen Sie die SSH-Agent-Weiterleitung über die Eingabe des folgenden Befehls in das Terminal:

        echo "$SSH_AUTH_SOCK"

    Die Ausgabe dieses Befehls sieht in etwa wie folgt aus:

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    Sollte nichts zurückgegeben werden, wird `ssh-agent` nicht ausgeführt. Sehen Sie sich unter [Using ssh-agent with ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) (Verwenden von „ssh-agent“ mit SSH) die Informationen zu den Agent-Startskripts an, oder informieren Sie sich in der Dokumentation Ihres SSH-Clients über spezifische Installations- und Konfigurationsschritte für `ssh-agent`.

4. Nachdem Sie sichergestellt haben, dass **ssh-Agent** ausgeführt wird, verwenden Sie folgenden Befehl, um Ihren privaten SSH-Schlüssel dem Agent hinzuzufügen:

        ssh-add ~/.ssh/id_rsa

    Wenn Ihr privater Schlüssel in einer anderen Datei gespeichert ist, ersetzen Sie `~/.ssh/id_rsa` durch den Pfad zur Datei.

<a id="domainjoined"></a>
### <a name="domain-joined-hdinsight"></a>In die Domäne eingebundenes HDInsight

Bei [in die Domäne eingebundenem HDInsight](hdinsight-domain-joined-introduction.md) wird Kerberos mit Hadoop in HDInsight integriert. Da der SSH-Benutzer kein Active Directory-Domänenbenutzer ist, können Hadoop-Befehle für dieses Benutzerkonto erst nach erfolgter Active Directory-Authentifizierung ausgeführt werden. Gehen Sie wie folgt vor, um Ihre SSH-Sitzung mit Active Directory zu authentifizieren:

1. Stellen Sie eine Verbindung mit einem in die Domäne eingebundenen HDInsight-Cluster her. Verwenden Sie dabei die SSH, wie im Abschnitt [Herstellen einer Verbindung mit HDInsight](#connect) gezeigt. Mit dem folgenden Befehl wird beispielsweise unter Verwendung eines SSH-Kontos namens __sshuser__ eine Verbindung mit einem HDInsight-Cluster namens __myhdi__ hergestellt:

        ssh sshuser@myhdi-ssh.azurehdinsight.net

2. Verwenden Sie Folgendes, um eine Authentifizierung mit einem Domänenbenutzer und einem Kennwort durchzuführen:

        kinit

     Geben Sie einen Domänenbenutzernamen und das Kennwort für den Domänenbenutzer ein, wenn Sie dazu aufgefordert werden.

    Weitere Informationen zum Konfigurieren von Domänenbenutzern für in die Domäne eingebundene HDInsight-Cluster finden Sie unter [Configure Domain-joined HDInsight clusters (Preview)](hdinsight-domain-joined-configure.md) (Konfigurieren von in die Domäne eingebundenen HDInsight-Clustern (Vorschau)).

Nach erfolgter Authentifizierung mithilfe des Befehls `kinit` können Sie Hadoop-Befehle wie `hdfs dfs -ls /` oder `hive` verwenden.

## <a name="a-idtunnelassh-tunneling"></a><a id="tunnel"></a>SSH-Tunnel

SSH kann auch zum Tunneln lokaler Anforderungen wie etwa Webanforderungen zum HDInsight-Cluster verwendet werden. Die Anforderung wird dann an die angeforderte Ressource weitergeleitet, als ob sie vom Stammknoten des HDInsight-Clusters stammen würde.

> [!IMPORTANT]
> Ein SSH-Tunnel ist für manche Hadoop-Dienste eine Voraussetzung für den Zugriff auf die Webbenutzeroberfläche. Auf die Benutzeroberfläche des Auftragsverlaufs und des Ressourcen-Managers kann beispielsweise nur über einen SSH-Tunnel zugegriffen werden.

Weitere Informationen zum Erstellen und Verwenden eines SSH-Tunnels finden Sie unter [Verwenden von SSH-Tunneling zum Zugriff auf die Ambari-Webbenutzeroberfläche, JobHistory, NameNode, Oozie und andere Webbenutzeroberflächen](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie jetzt wissen, wie die Authentifizierung mithilfe eines SSH-Schlüssels erfolgt, erfahren Sie, wie Sie MapReduce mit Hadoop für HDInsight verwenden.

* [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce-Aufträgen mit HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/



<!--HONumber=Feb17_HO2-->


