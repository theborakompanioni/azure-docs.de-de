---
title: Entwickeln von Skriptaktionen mit Linux-basiertem HDInsight | Microsoft Docs
description: "Es wird beschrieben, wie Sie mit Skriptaktionen Linux-basierte HDInsight-Cluster anpassen können. Skriptaktionen stellen eine Möglichkeit zum Anpassen von Azure HDInsight-Clustern dar, indem Clusterkonfigurationseinstellungen angegeben oder zusätzliche Dienste, Tools oder andere Software auf dem Cluster installiert werden. "
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf4c89cd-f7da-4a10-857f-838004965d3e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 8c07f0da21eab0c90ad9608dfaeb29dd4a01a6b7
ms.openlocfilehash: 6eb692f7c3374f9073944b8c4c0f34af2ed35b3c
ms.lasthandoff: 01/19/2017


---
# <a name="script-action-development-with-hdinsight"></a>Entwickeln von Skriptaktionen mit HDInsight

Skriptaktionen stellen eine Möglichkeit zum Anpassen von Azure HDInsight-Clustern dar, indem Clusterkonfigurationseinstellungen angegeben oder zusätzliche Dienste, Tools oder andere Software auf dem Cluster installiert werden. Sie können Skriptaktionen während der Clustererstellung oder auf einem ausgeführten Cluster verwenden.

> [!IMPORTANT]
> Die Schritte in diesem Dokument erfordern einen HDInsight-Cluster mit Linux. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

## <a name="what-are-script-actions"></a>Was sind Skriptaktionen?

Skriptaktionen sind Bash-Skripts, die Azure auf dem Clusterknoten ausführt, um Konfigurationsänderungen vorzunehmen oder Software zu installieren. Eine Skriptaktion wird als Stamm ausgeführt und bietet Vollzugriffsrechte auf die Clusterknoten.

Skriptaktionen können mithilfe der folgenden Methoden angewendet werden:

| Verwenden Sie dies, um ein Skript anzuwenden... | Während der Clustererstellung... | In einem ausgeführten Cluster... |
| --- |:---:|:---:|
| Azure-Portal |✓ |✓ |
| Azure PowerShell |✓ |✓ |
| Azure-Befehlszeilenschnittstelle |&nbsp; |✓ |
| HDInsight .NET-SDK |✓ |✓ |
| Azure Resource Manager-Vorlage |✓ |&nbsp; |

Weitere Informationen zur Verwendung dieser Methoden zum Anwenden von Skriptaktionen finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Bewährte Methoden für die Entwicklung von Skripts

Wenn Sie ein benutzerdefiniertes Skript für einen HDInsight-Cluster entwickeln, sollten Sie mehrere bewährte Methoden beachten:

* [Auswählen der Hadoop-Version](#bPS1)
* [Auswählen der Betriebssystemversion](#bps10)
* [Einrichten stabiler Verknüpfungen mit Skriptressourcen](#bPS2)
* [Verwenden vorkompilierter Ressourcen](#bPS4)
* [Sicherstellen, dass das Clusteranpassungsskript idempotent ist](#bPS3)
* [Sicherstellen einer hohen Verfügbarkeit der Clusterarchitektur](#bPS5)
* [Konfigurieren benutzerdefinierter Komponenten zur Verwendung von Azure-Blobspeicher](#bPS6)
* [Schreiben von Informationen in STDOUT und STDERR](#bPS7)
* [Speichern von Dateien im ASCII-Format mit LF-Zeilenenden](#bps8)
* [Verwenden von Wiederholungsversuchlogik zum Wiederherstellen bei vorübergehenden Fehlern](#bps9)

> [!IMPORTANT]
> Skriptaktionen müssen innerhalb von 60 Minuten abgeschlossen sein, andernfalls tritt ein Timeout ein. Während der Knotenbereitstellung wird das Skript gleichzeitig mit anderen Einrichtungs- und Konfigurationsprozessen ausgeführt. Der Wettbewerb um Ressourcen wie CPU-Zeit oder Netzwerkbandbreite kann dazu führen, dass es länger als in Ihrer Entwicklungsumgebung dauert, bis das Skript abgeschlossen ist.

### <a name="bPS1"></a>Auswählen der Hadoop-Version

In den verschiedenen HDInsight-Versionen sind unterschiedliche Versionen von Hadoop-Diensten und Hadoop-Komponenten installiert. Wenn bei Ihrem Skript eine bestimmte Version eines Diensts oder einer Komponente vorausgesetzt wird, sollten Sie das Skript nur mit der HDInsight-Version verwenden, die die erforderlichen Komponenten enthält. Informationen zu den in HDInsight enthaltenen Komponentenversionen finden Sie im Dokument [Neuheiten in den von HDInsight bereitgestellten Hadoop-Clusterversionen](hdinsight-component-versioning.md) .

### <a name="bps10"></a> Auswählen der Betriebssystemversion

Linux-basiertes HDInsight basiert auf der Ubuntu Linux-Distribution. Da unterschiedliche Versionen von HDInsight auf verschiedenen Versionen von Ubuntu basieren, können sich Auswirkungen auf das Skriptverhalten ergeben. Beispielsweise basiert HDInsight 3.4 und früher auf Ubuntu-Versionen, für die Upstart genutzt wird. Version 3.5 basiert auf Ubuntu 16.04 mit Verwendung von Systemd. Für Systemd und Upstart sind unterschiedliche Befehle erforderlich, und Ihr Skript sollte so geschrieben sein, dass es für beide Fälle funktioniert.

Ein weiterer wichtiger Unterschied zwischen HDInsight 3.4 und 3.5 besteht darin, dass `JAVA_HOME` jetzt auf Java 8 verweist.

Sie können `lsb_release` verwenden, um die Betriebssystemversion zu prüfen. Die folgenden Codeausschnitte aus dem Hue-Installationsskript zeigen, wie ermittelt werden kann, ob das Skript unter Ubuntu 14 oder 16 ausgeführt wird:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
...
if [[ $OS_VERSION == 16* ]]; then
    echo "Using systemd configuration"
    systemctl daemon-reload
    systemctl stop webwasb.service    
    systemctl start webwasb.service
else
    echo "Using upstart configuration"
    initctl reload-configuration
    stop webwasb
    start webwasb
fi
...
if [[ $OS_VERSION == 14* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
elif [[ $OS_VERSION == 16* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
fi
```

Sie finden das vollständige Skript, in dem diese Codeausschnitte enthalten sind, unter „https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh“.

Informationen zur Version von Ubuntu, die von HDInsight verwendet wird, finden Sie im Dokument [HDInsight-Komponentenversion](hdinsight-component-versioning.md).

Informationen zu den Unterschieden zwischen Systemd und Upstart finden Sie unter [Systemd for Upstart users](https://wiki.ubuntu.com/SystemdForUpstartUsers) (Systemd für Upstart-Benutzer).

### <a name="bPS2"></a>Einrichten stabiler Verknüpfungen mit Skriptressourcen

Sie müssen dafür sorgen, dass alle Skripts und Ressourcen, die im Skript verwendet werden, für die gesamte Nutzungsdauer des Clusters verfügbar bleiben und dass sich die Versionen dieser Dateien während dieses Zeitraums nicht ändern. Diese Ressourcen werden benötigt, wenn während der Skalierungsvorgänge neue Knoten zum Cluster hinzugefügt werden.

Die bewährte Methode ist das Herunterladen und Archivieren aller Daten in einem Azure-Speicherkonto Ihres Abonnements.

> [!IMPORTANT]
> Bei dem verwendeten Speicherkonto muss es sich um das Standardspeicherkonto des Clusters oder um einen öffentlichen, schreibgeschützten Container eines anderen Speicherkontos handeln.

Die von Microsoft bereitgestellten Beispiele sind beispielsweise im Speicherkonto [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) gespeichert, einem öffentlichen, schreibgeschützten Container, der vom HDInsight-Team verwaltet wird.

### <a name="bPS4"></a>Verwenden vorkompilierter Ressourcen

Zur Verringerung des Zeitraums, der für die Ausführung des Skripts benötigt wird, sollten Sie Vorgänge vermeiden, mit denen Ressourcen aus dem Quellcode kompiliert werden. Führen Sie stattdessen eine Vorkompilierung der Ressourcen durch, und speichern Sie die binäre Version im Azure-BLOB-Speicher, damit sie per Skript schnell in den Cluster heruntergeladen werden kann.

### <a name="bPS3"></a>Sicherstellen, dass das Clusteranpassungsskript idempotent ist

Skripts müssen dahingehend idempotent sein, dass sichergestellt ist, dass der Cluster bei jeder Ausführung in denselben Zustand zurückgesetzt wird, wenn das Skript mehrmals ausgeführt wird.

Beispiel: Wenn ein benutzerdefiniertes Skript bei der ersten Ausführung eine Anwendung unter "/usr/local/bin" installiert, muss das Skript bei allen nachfolgenden Ausführungen prüfen, ob die Anwendung am Speicherort "/usr/local/bin" bereits vorhanden ist, ehe mit anderen Schritten im Skript fortgefahren wird.

### <a name="bPS5"></a>Sicherstellen einer hohen Verfügbarkeit der Clusterarchitektur

Linux-basierte HDInsight-Cluster umfassen zwei Hauptknoten, die im Cluster aktiv sind. Skriptaktionen werden für beide Knoten ausgeführt. Wenn die zu installierenden Komponenten nur einen Hauptknoten erwarten, müssen Sie ein Skript erstellen, mit dem die Komponenten nur auf einem der beiden Hauptknoten im Cluster installiert werden.

> [!IMPORTANT]
> Standarddienste, die als Teil von HDInsight installiert werden, sind so konzipiert, dass bei Bedarf ein Failover zwischen den beiden Hauptknoten durchgeführt wird. Diese Funktionalität erstreckt sich jedoch nicht auf benutzerdefinierte Komponenten, die über Skriptaktionen installiert werden. Wenn die über eine Skriptaktion installierten Komponenten eine hohe Verfügbarkeit aufweisen sollen, müssen Sie einen eigenen Failovermechanismus für die beiden verfügbaren Hauptknoten implementieren.

### <a name="bPS6"></a>Konfigurieren benutzerdefinierter Komponenten zur Verwendung von Azure-Blobspeicher

Die Komponenten, die Sie auf dem Cluster installieren, sind möglicherweise standardmäßig so konfiguriert, dass sie den HDFS-Speicher (Hadoop Distributed File System) verwenden. HDInsight verwendet Azure Blob Storage (WASB) als Standardspeicher. Dieser bietet ein mit HDFS kompatibles Dateisystem, das Daten auch dann beibehält, wenn der Cluster gelöscht wird. Sie sollten die Komponenten, die Sie installieren, so konfigurieren, dass sie WASB anstelle von HDFS verwenden.

Mit dem folgenden Code wird beispielsweise die Datei "giraph-examples.jar" aus dem lokalen Dateisystem in WASB kopiert:

```bash
hdfs dfs -put /usr/hdp/current/giraph/giraph-examples.jar /example/jars/
```

### <a name="bPS7"></a>Schreiben von Informationen in STDOUT und STDERR

Die während der Skriptausführung in STDOUT und STDERR geschriebenen Informationen werden protokolliert und können über die Ambari-Webbenutzeroberfläche angezeigt werden.

> [!NOTE]
> Ambari ist nur dann verfügbar, wenn der Cluster erfolgreich erstellt wurde. Wenn Sie während der Clustererstellung eine Skriptaktion verwenden, und ein Fehler bei der Erstellung auftritt, finden Sie im Abschnitt zur Problembehandlung unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) andere Möglichkeiten, um auf protokollierte Informationen zuzugreifen.

Die meisten Dienstprogramme und Installationspakete schreiben bereits Informationen in STDOUT und STDERR. Möglicherweise möchten Sie jedoch weitere Protokollierungsinformationen hinzufügen. Verwenden Sie `echo`, um Text an STDOUT zu senden. Beispiel:

```bash
echo "Getting ready to install Foo"
```

Standardmäßig wird durch `echo` der String an STDOUT gesendet. Soll dieser an STDERR geleitet werden, setzen Sie `>&2` vor `echo`. Beispiel:

```bash
>&2 echo "An error occurred installing Foo"
```

Damit werden die an STDOUT gesendeten Informationen (1, Standardwert und daher hier nicht aufgeführt) an STDERR (2) umgeleitet. Weitere Informationen zur E/A-Umleitung finden Sie unter [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).

Weitere Informationen zum Anzeigen der durch Skriptaktionen protokollierten Daten finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

### <a name="bps8"></a> Speichern von Dateien im ASCII-Format mit LF-Zeilenenden

Bash-Skripts sollten im ASCII-Format und mit LF als Zeilenende gespeichert werden. Wenn Dateien im Format UTF-8, das eine Bytereihenfolge-Marke am Anfang der Datei enthalten kann, oder mit CR-LF-Zeilenenden gespeichert werden (gängig bei Windows-Editoren), schlägt das Skript mit Fehlermeldungen fehl, die der folgenden ähneln:

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <a name="bps9"></a>Verwenden von Wiederholungsversuchlogik zum Wiederherstellen bei vorübergehenden Fehlern

Beim Herunterladen von Dateien, die Pakete mit apt-get installieren, oder anderen Aktionen, die Daten über das Internet übertragen, können vorübergehende Netzwerkfehler die erfolgreiche Ausführung verhindern. Für die Remoteressource, mit der Sie kommunizieren, könnte z.B. gerade ein Failover zu einem Sicherungsknoten durchgeführt werden.

Damit Ihr Skript gegenüber vorübergehenden Fehlern stabil ist, können Sie die Wiederholungsversuchlogik implementieren. Die folgende Beispielfunktion führt jeden Befehl aus, der ihr übergeben wird, und falls bei der Ausführung ein Fehler auftritt, versucht sie bis zu dreimal erneut, den Befehl auszuführen. Zwischen den einzelnen Neuversuchen liegt eine Wartezeit von zwei Sekunden.

```bash
#retry
MAXATTEMPTS=3

retry() {
    local -r CMD="$@"
    local -i ATTMEPTNUM=1
    local -i RETRYINTERVAL=2

    until $CMD
    do
        if (( ATTMEPTNUM == MAXATTEMPTS ))
        then
                echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                return 1
        else
                echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                sleep $(( RETRYINTERVAL ))
                ATTMEPTNUM=$ATTMEPTNUM+1
        fi
    done
}
```

Es folgen Beispiele für die Verwendung dieser Funktion.

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <a name="helpermethods"></a>Hilfsmethoden für benutzerdefinierte Skripts

Hilfsmethoden für Skriptaktionen sind Hilfsprogramme, die Sie zum Schreiben von benutzerdefinierten Skripts verwenden können. Diese werden in der Datei [https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh)definiert und können wie folgt in Ihre Skripts eingefügt werden:

```bash
# Import the helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

Damit können Sie folgende Hilfsprogramme in Ihrem Skript nutzen:

| Hilfsprogramm | Beschreibung |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |Lädt eine Datei aus dem Quell-URI in den angegebenen Dateipfad herunter. Standardmäßig wird eine vorhandene Datei nicht überschrieben. |
| `untar_file TARFILE DESTDIR` |Extrahiert eine TAR-Datei (mit `-xf`) in das Zielverzeichnis. |
| `test_is_headnode` |Bei Ausführung auf dem Hauptknoten eines Clusters wird "1" zurückgegeben, andernfalls "0". |
| `test_is_datanode` |Wenn der aktuelle Knoten ein Datenknoten (Workerknoten) ist, wird "1" zurückgegeben, andernfalls "0". |
| `test_is_first_datanode` |Wenn der aktuelle Knoten der erste Datenknoten (Workerknoten) ist (mit dem Namen "workernode0"), wird "1" zurückgegeben, andernfalls "0". |
| `get_headnodes` |Gibt den vollqualifizierten Domänennamen der Hauptknoten im Cluster zurück. Namen sind durch Kommas getrennt. Bei einem Fehler wird eine leere Zeichenfolge zurückgegeben. |
| `get_primary_headnode` |Ruft den vollqualifizierten Domänennamen des primären Hauptknotens ab. Bei einem Fehler wird eine leere Zeichenfolge zurückgegeben. |
| `get_secondary_headnode` |Ruft den vollqualifizierten Domänennamen des sekundären Hauptknotens ab. Bei einem Fehler wird eine leere Zeichenfolge zurückgegeben. |
| `get_primary_headnode_number` |Ruft das numerische Suffix des primären Hauptknotens ab. Bei einem Fehler wird eine leere Zeichenfolge zurückgegeben. |
| `get_secondary_headnode_number` |Ruft das numerische Suffix des sekundären Hauptknotens ab. Bei einem Fehler wird eine leere Zeichenfolge zurückgegeben. |

## <a name="commonusage"></a>Gängige Verwendungsmuster

Dieser Abschnitt enthält Anweisungen für das Implementieren einiger gängiger Verwendungsmuster, die Ihnen beim Schreiben Ihrer eigenen benutzerdefinierten Skripts begegnen können.

### <a name="passing-parameters-to-a-script"></a>Übergeben von Parametern an ein Skript

In einigen Fällen müssen Sie Parameter für das Skript angeben. Beispielsweise müssen Sie das Administratorkennwort für den Cluster angeben, um Informationen über die Ambari-REST-API abzurufen.

Parameter, die an das Skript übergeben werden, werden als *Positionsparameter* bezeichnet und für den ersten Parameter `$1` zugewiesen, dann für den zweiten Parameter `$2` usw. `$0` enthält den Namen des Skripts.

Werte, die als Parameter an das Skript übergeben werden, sollten in einfache Anführungszeichen (') eingeschlossen werden, sodass der übergebene Wert als Literal behandelt wird und darin enthaltene Zeichen wie z. B. "!" nicht besonders behandelt werden.

### <a name="setting-environment-variables"></a>Festlegen von Umgebungsvariablen

Eine Umgebungsvariable wird wie folgt festgelegt:

    VARIABLENAME=value

Dabei ist VARIABLENNAME der Name der Variable. Verwenden Sie `$VARIABLENAME`, um anschließend auf die Variable zuzugreifen. Um einen Wert, der von einem Positionsparameter bereitgestellt wird, als Umgebungsvariable mit dem Namen "PASSWORD" zuzuweisen, verwenden Sie beispielsweise folgenden Code:

    PASSWORD=$1

Für den anschließenden Zugriff auf die Informationen kann dann `$PASSWORD` verwendet werden.

Umgebungsvariablen, die im Skript festgelegt werden, gelten nur innerhalb des Gültigkeitsbereichs des Skripts. In einigen Fällen müssen Sie möglicherweise systemweite Umgebungsvariablen hinzufügen, die nach Abschluss des Skripts beibehalten werden. Dies erfolgt in der Regel, damit Benutzer, die über SSH eine Verbindung mit dem Cluster herstellen, die mit Ihrem Skript installierten Komponenten verwenden können. Fügen Sie dazu `/etc/environment` die Umgebungsvariable hinzu. Mit dem folgenden Beispiel wird z.B. **HADOOP\_CONF\_DIR** hinzugefügt:

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Zugriff auf Speicherorte benutzerdefinierter Skripts

Skripts zum Anpassen eines Clusters müssen an einem der folgenden Speicherorte gespeichert werden:

* __Standardspeicherkonto__ für den Cluster

* __Zusätzliches Speicherkonto__, das dem Cluster zugeordnet ist

* __Öffentlich lesbarer URI__ wie OneDrive, Dropbox usw.

* __Azure Data Lake Store__-Konto, das dem HDInsight-Cluster zugeordnet ist Weitere Informationen zum Verwenden von Azure Data Lake Store mit HDInsight finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md). 

    > [!NOTE]
    > Der Dienstprinzipal, der von HDInsight zum Zugreifen auf Data Lake Store genutzt wird, muss über Lesezugriff auf das Skript verfügen.

Wenn Ihr Skript auf externe Ressourcen zugreift, müssen auch diese öffentlich zugänglich sein (oder mindestens einen öffentlichen Lesezugriff aufweisen). Beispielsweise können Sie eine Datei mithilfe von `download_file` in den Cluster herunterladen.

Durch Speichern der Datei in einem Azure-Speicherkonto, auf das der Cluster zugreifen kann (z. B. das Standardspeicherkonto), wird ein schneller Zugriff ermöglicht, da sich dieser Speicher im Azure-Netzwerk befindet.

> [!NOTE]
> Das URI-Format zum Verweisen auf das Skript unterscheidet sich je nach dem verwendeten Dienst. Verwenden Sie für Speicherkonten, die dem HDInsight-Cluster zugeordnet sind, `wasb://` oder `wasbs://`. Verwenden Sie für öffentlich lesbare URIs `http://` oder `https://`. Verwenden Sie für Data Lake Store `adl://`.

### <a name="checking-the-operating-system-version"></a>Überprüfen der Betriebssystemversion

Unterschiedliche Versionen von HDInsight basieren auf bestimmten Versionen von Ubuntu. Unter Umständen bestehen Unterschiede zwischen den Betriebssystemversionen, die Sie für Ihr Skript überprüfen sollten. Beispielweise müssen Sie ggf. eine Binärdatei installieren, die an die Version von Ubuntu gebunden ist.

Verwenden Sie `lsb_release`, um die Betriebssystemversion zu überprüfen. Hier ist beispielsweise dargestellt, wie Sie je nach Betriebssystemversion auf eine andere TAR-Datei verweisen:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
```

## <a name="deployScript"></a>Prüfliste für die Bereitstellung einer Skriptaktion

Es folgen unsere Schritte bei der Vorbereitung der Bereitstellung dieser Skripts:

* Legen Sie Daten mit den benutzerdefinierten Skripts an einem Speicherort ab, auf den die Clusterknoten während der Bereitstellung zugreifen können. Dies kann ein beliebiges Standardkonto oder zusätzliches Speicherkonto, das während der Bereitstellung angegeben wurde, oder ein anderer öffentlich zugänglicher Speichercontainer sein.
* Fügen Sie Skripts Überprüfungen hinzu, um sicherzustellen, dass sie idempotent ausgeführt werden, damit das Skript mehrmals auf demselben Knoten ausgeführt werden kann.
* Legen Sie die heruntergeladenen von den Skripts verwendeten Dateien in einem temporären Dateiverzeichnis ab (z. B. "/tmp"), und löschen Sie sie nach der Ausführung der Skripts.
* Wenn sich Einstellungen auf Betriebssystemebene oder Hadoop-Dienstkonfigurationsdateien geändert haben, können Sie bei Bedarf die HDInsight-Dienste neu starten. Diese können dann Einstellungen auf Betriebssystemebene übernehmen, z. B. die in den Skripts festgelegten Umgebungsvariablen.

## <a name="runScriptAction"></a>Ausführen einer Skriptaktion

Sie können Skriptaktionen zum Anpassen von HDInsight-Clustern über das Azure-Portal, Azure PowerShell, Azure Resource Manager-Vorlagen oder über das HDInsight .NET SDK ausführen. Anweisungen finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Beispiele benutzerdefinierter Skripts

Microsoft bietet Beispielskripts für die Installation von Komponenten in einem HDInsight-Cluster. Die Beispielskripts und Nutzungsanweisungen stehen unter den folgenden Links zur Verfügung:

* [Installieren und Verwenden von Hue in HDInsight-Clustern](hdinsight-hadoop-hue-linux.md)
* [Installieren und Verwenden von R in HDInsight Hadoop-Clustern](hdinsight-hadoop-r-scripts-linux.md)
* [Installieren und Verwenden von Solr in HDInsight-Clustern](hdinsight-hadoop-solr-install-linux.md)
* [Installieren und Verwenden von Giraph in HDInsight-Clustern](hdinsight-hadoop-giraph-install-linux.md)  

> [!NOTE]
> Die oben aufgeführten Dokumente gelten für Linux-basierte HDInsight-Cluster. Informationen zu Skripts für Windows-basierte HDInsight-Cluster finden Sie unter [Entwickeln von Skriptaktionen mit HDInsight (Windows)](hdinsight-hadoop-script-actions.md) , oder über die Links am Seitenanfang der einzelnen Artikel.

## <a name="troubleshooting"></a>Problembehandlung

Bei der Verwendung der von Ihnen entwickelten Skripts können die folgenden Fehler auftreten:

**Fehler**: `$'\r': command not found`. Manchmal gefolgt von `syntax error: unexpected end of file`.

*Ursache*: Dieser Fehler tritt auf, wenn die Zeilen in einem Skript mit CR-LF enden. In UNIX-Systemen wird als Zeilenende nur LF erwartet.

Dieses Problem tritt am häufigsten auf, wenn das Skript in einer Windows-Umgebung erstellt wird, da CR-LF in vielen Text-Editoren unter Windows als gängiges Zeilenende verwendet wird.

*Lösung*: Wenn Ihr Text-Editor über die entsprechende Option verfügt, wählen Sie UNIX als Format oder LF für das Zeilenende aus. In einem UNIX-System können Sie außerdem die folgenden Befehle verwenden, um CR-LF in LF zu ändern:

> [!NOTE]
> Mit den folgenden annähernd gleichwertigen Befehlen sollten sich die CR-LF-Zeilenenden in LF ändern lassen. Wählen Sie einen Befehl entsprechend den in Ihrem System verfügbaren Dienstprogrammen aus.

| Befehl | Hinweise |
| --- | --- |
| `unix2dos -b INFILE` |Die ursprüngliche Datei wird mit einer BAK-Erweiterung gesichert. |
| `tr -d '\r' < INFILE > OUTFILE` |OUTFILE enthält eine Version, die ausschließlich LF-Zeilenenden umfasst. |
| `perl -pi -e 's/\r\n/\n/g' INFILE` |Damit wird die Datei direkt geändert, ohne dass eine neue Datei erstellt wird. |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |OUTFILE enthält eine Version, die ausschließlich LF-Zeilenenden umfasst. |

**Fehler**: `line 1: #!/usr/bin/env: No such file or directory`.

*Ursache*: Dieser Fehler tritt auf, wenn das Skript im Format UTF-8 mit einer Bytereihenfolge-Marke (BOM) gespeichert wurde.

*Lösung*: Speichern Sie die Datei entweder im ASCII-Format oder im UTF-8-Format ohne Bytereihenfolge-Marke. In einem Linux- oder UNIX-System können Sie auch mit dem folgenden Befehl eine neue Datei ohne Bytereihenfolge-Marke erstellen:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Ersetzen Sie den oben aufgeführten Befehl **INFILE** durch die Datei mit Bytereihenfolge-Marke. Für **OUTFILE** sollte ein neuer Dateiname eingegeben werden. Die Datei enthält dann das Skript ohne Bytereihenfolge-Marke.

## <a name="seeAlso"></a>Nächste Schritte

* Lernen Sie das [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md)
* Verwenden Sie die [HDInsight .NET SDK-Referenz](https://msdn.microsoft.com/library/mt271028.aspx) , um mehr über das Erstellen von .NET-Anwendungen zu erfahren, die HDInsight verwalten.
* Verwenden Sie die [HDInsight-REST-API](https://msdn.microsoft.com/library/azure/mt622197.aspx) , um zu erfahren, wie Sie REST verwenden, um Verwaltungsaktionen auf HDInsight-Clustern auszuführen.


