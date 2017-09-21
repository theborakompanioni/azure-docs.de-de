---
title: Erste Schritte mit der Azure Service Fabric CLI
description: Hier erfahren Sie, wie Sie die Azure Service Fabric CLI verwenden. Es wird beschrieben, wie Sie eine Verbindung mit einem Cluster herstellen und Anwendungen verwalten.
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: edwardsa
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 851f04c8b5eee762ec43060f02c8b83f00c1782e
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="azure-service-fabric-cli"></a>Azure Service Fabric CLI

Bei der Azure Service Fabric-Befehlszeilenschnittstelle (CLI) handelt es sich um ein Befehlszeilen-Hilfsprogramm zum Interagieren mit und Verwalten von Service Fabric-Entitäten. Für die Service Fabric CLI ist die Verwendung mit Windows- oder mit Linux-Clustern möglich. Die Service Fabric CLI wird auf allen Plattformen ausgeführt, die Python unterstützen.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie vor der Installation sicher, dass in Ihrer Umgebung Python und pip installiert sind. Weitere Informationen finden Sie in der [Schnellstartdokumentation zu pip](https://pip.pypa.io/en/latest/quickstart/) und der offiziellen [Python-Installationsdokumentation](https://wiki.python.org/moin/BeginnersGuide/Download).

Die Versionen Python 2.7 und 3.6 werden zwar unterstützt, aber wir empfehlen Ihnen die Verwendung von Python 3.6. Im folgenden Abschnitt wird beschrieben, wie Sie die erforderlichen Komponenten und die Befehlszeilenschnittstelle installieren.

## <a name="install-pip-python-and-the-service-fabric-cli"></a>Installieren von pip, Python und der Service Fabric CLI

 Es gibt viele Möglichkeiten, pip und Python auf Ihrer Plattform zu installieren. Hier sind einige Schritte aufgeführt, mit denen Sie die gängigen Betriebssysteme mit Python 3.6 und pip schnell einrichten können.

### <a name="windows"></a>Windows

Verwenden Sie für Windows 10, Windows Server 2016 und Windows Server 2012 R2 die offizielle Standardanleitung für die Installation. Mit dem Python-Installationsprogramm wird standardmäßig auch pip installiert.

1. Navigieren Sie zur offiziellen [Python-Downloadseite](https://www.python.org/downloads/), und laden Sie die neueste Version von Python 3.6 herunter.

2. Starten Sie das Installationsprogramm.

3. Wählen Sie unten in der Eingabeaufforderung die Option **Add Python 3.6 to PATH** (Python 3.6 zu PATH hinzufügen).

4. Wählen Sie **Jetzt installieren**, und schließen Sie die Installation ab.

Sie können nun ein neues Befehlsfenster öffnen und die Version von Python und PIP abrufen.

```bat
python --version
pip --version
```

Installieren Sie anschließend die Service Fabric-Befehlszeilenschnittstelle mit dem folgenden Befehl:

```
pip install sfctl
sfctl -h
```

### <a name="ubuntu"></a>Ubuntu

Für Ubuntu 16.04 Desktop können Sie Python 3.6 installieren, indem Sie ein PPA (Personal Package Archive) eines Drittanbieters verwenden.

Führen Sie am Terminal die folgenden Befehle aus:

```bash
sudo add-apt-repository ppa:jonathonf/python-3.6
sudo apt-get update
sudo apt-get install python3.6
sudo apt-get install python3-pip
```

Führen Sie dann zum Installieren der Service Fabric CLI nur für Ihre Installation von Python 3.6 den folgenden Befehl aus:

```bash
python3.6 -m pip install sfctl
sfctl -h
```

Diese Schritte haben keine Auswirkungen auf die Systeminstallationen von Python 3.5 und 2.7. Versuchen Sie nicht, diese Installationen zu ändern, sofern Sie nicht mit Ubuntu vertraut sind.

### <a name="macos"></a>macOS

Für MacOS empfehlen wir Ihnen die Nutzung des [HomeBrew-Paket-Managers](https://brew.sh). Wenn HomeBrew nicht bereits installiert ist, können Sie die Installation mit dem folgenden Befehl durchführen:

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Installieren Sie dann über das Terminal Python 3.6, pip und die Service Fabric CLI, indem Sie die folgenden Befehle ausführen:

```bash
brew install python3
pip3 install sfctl
sfctl -h
```

Diese Schritte haben keine Auswirkungen auf die Systeminstallation von Python 2.7.

## <a name="cli-syntax"></a>CLI-Syntax

Die Befehle verfügen immer über das Präfix `sfctl`. Allgemeine Informationen zu allen Befehlen, die Sie nutzen können, erhalten Sie mit `sfctl -h`. Hilfe zu einem einzelnen Befehl erhalten Sie mit `sfctl <command> -h`.

Befehle folgen einer wiederholbaren Struktur. Dabei steht das Ziel des Befehls vor dem Verb oder der Aktion.

```azurecli
sfctl <object> <action>
```

In diesem Beispiel ist `<object>` das Ziel für `<action>`.

## <a name="select-a-cluster"></a>Auswählen eines Clusters

Bevor Sie Vorgänge durchführen, müssen Sie einen Cluster auswählen, mit dem die Verbindung hergestellt wird. Führen Sie beispielsweise den folgenden Befehl aus, um den Cluster `testcluster.com` auszuwählen und eine Verbindung damit herzustellen:

> [!WARNING]
> Vermeiden Sie die Verwendung von ungeschützten Service Fabric-Clustern in einer Produktionsumgebung.

```azurecli
sfctl cluster select --endpoint http://testcluster.com:19080
```

Der Clusterendpunkt muss das Präfix `http` oder `https` aufweisen. Er muss den Port für das HTTP-Gateway enthalten. Der Port und die Adresse entsprechen der Service Fabric Explorer-URL.

Für Cluster, die mit einem Zertifikat gesichert sind, können Sie ein PEM-codiertes Zertifikat angeben. Das Zertifikat kann als eine einzelne Datei oder als Zertifikat und Schlüsselpaar angegeben werden.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md).

## <a name="basic-operations"></a>Einfache Vorgänge

Informationen zur Clusterverbindung werden über mehrere Service Fabric CLI-Sitzungen hinweg beibehalten. Nach dem Auswählen eines Service Fabric-Clusters können Sie im Cluster einen beliebigen Service Fabric-Befehl ausführen.

Verwenden Sie beispielsweise den folgenden Befehl, um den Integritätsstatus des Service Fabric-Clusters abzurufen:

```azurecli
sfctl cluster health
```

Mit diesem Befehl wird die folgende Ausgabe zurückgegeben:

```json
{
  "aggregatedHealthState": "Ok",
  "applicationHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "name": "fabric:/System"
    }
  ],
  "healthEvents": [],
  "nodeHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "id": {
        "id": "66aa824a642124089ee474b398d06a57"
      },
      "name": "_Test_0"
    }
  ],
  "unhealthyEvaluations": []
}
```

## <a name="tips-and-troubleshooting"></a>Tipps und Problembehandlung

Hier sind einige Vorschläge und Tipps zum Beheben von allgemeinen Problemen angegeben.

### <a name="convert-a-certificate-from-pfx-to-pem-format"></a>Konvertieren eines Zertifikats aus dem PFX- in das PEM-Format

Die Service Fabric-Befehlszeilenschnittstelle unterstützt clientseitige Zertifikate als PEM-Dateien (Erweiterung „.pem“). Bei Verwendung von PFX-Dateien in Windows müssen Sie diese Zertifikate in das PEM-Format konvertieren. Mit dem folgenden Befehl konvertieren Sie eine PFX-Datei in eine PEM-Datei:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Weitere Informationen finden Sie in der [OpenSSL-Dokumentation](https://www.openssl.org/docs/).

### <a name="connection-problems"></a>Verbindungsprobleme

Für einige Vorgänge wird unter Umständen die folgende Meldung generiert:

`Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

Stellen Sie sicher, dass der angegebene Clusterendpunkt verfügbar ist und lauscht. Überprüfen Sie auch, ob die Service Fabric Explorer-Benutzeroberfläche unter diesem Host und Port verfügbar ist. Verwenden Sie `sfctl cluster select`, um den Endpunkt zu aktualisieren.

### <a name="detailed-logs"></a>Detaillierte Protokolle

Detaillierte Protokolle sind häufig hilfreich, wenn Sie das Debuggen durchführen oder ein Problem melden. Mit einem globalen `--debug`-Flag wird die Ausführlichkeit von Protokolldateien erhöht.

### <a name="command-help-and-syntax"></a>Befehle – Hilfe und Syntax

Verwenden Sie das `-h`-Flag, um Hilfe zu einem bestimmten Befehl oder einer Gruppe von Befehlen zu erhalten.

```azurecli
sfctl application -h
```

Hier ist ein weiteres Beispiel angegeben:

```azurecli
sfctl application create -h
```

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen einer Anwendung mit der Azure Service Fabric-Befehlszeilenschnittstelle](service-fabric-application-lifecycle-sfctl.md)
* [Erste Schritte mit Service Fabric unter Linux](service-fabric-get-started-linux.md)

