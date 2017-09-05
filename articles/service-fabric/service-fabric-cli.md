---
title: Erste Schritte mit der Azure Service Fabric CLI (sfctl)
description: Hier erfahren Sie, wie Sie die Azure Service Fabric CLI verwenden. Es wird beschrieben, wie Sie eine Verbindung mit einem Cluster herstellen und Anwendungen verwalten.
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: edwardsa
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: 2faca2887f25b45d833dea7b2259277466290670
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="azure-service-fabric-command-line"></a>Azure Service Fabric-Befehlszeile

Bei der Azure Service Fabric-Befehlszeilenschnittstelle (sfctl) handelt es sich um ein Befehlszeilenprogramm für die Interaktion mit und die Verwaltung von Azure Service Fabric-Entitäten. sfctl kann mit Windows- oder Linux-Clustern verwendet werden. sfctl wird auf jeder Plattform ausgeführt, die Python unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie vor der Installation sicher, dass in Ihrer Umgebung Python und pip installiert sind. Weitere Informationen finden Sie in der [Schnellstartdokumentation zu pip](https://pip.pypa.io/en/latest/quickstart/) und in der offiziellen [Dokumentation zur Python-Installation](https://wiki.python.org/moin/BeginnersGuide/Download).

Python 2.7 und 3.6 werden unterstützt, es wird jedoch die Verwendung von Python 3.6 empfohlen. Im folgenden Abschnitt erfahren Sie mehr über die Installation der erforderlichen Komponenten und der Befehlszeilenschnittstelle.

## <a name="install-pip-python-and-sfctl"></a>Installieren von PIP, Python und SFCTL

Sie haben mehrere Möglichkeiten, PIP und Python auf Ihrer Plattform zu installieren. Die folgenden Schritte ermöglichen einen schnellen Einstieg in Python 3.6 und PIP für gängige Betriebssysteme:

### <a name="windows"></a>Windows

Bei Windows 10, Server 2016 und Server 2012 R2 können Sie die offiziellen Installationsanweisungen befolgen. Das Python-Installationsprogramm installiert standardmäßig auch PIP.

- Navigieren Sie zur offiziellen [Python-Downloadseite](https://www.python.org/downloads/), und laden Sie die neueste Version von Python 3.6 herunter.
- Starten Sie das Installationsprogramm.
- Wählen Sie die Option am unteren Rand der Aufforderung, um `Add Python 3.6 to PATH` auszuführen.
- Wählen Sie `Install Now`.
- Schließen Sie die Installation ab.

Jetzt sollten ein neues Befehlsfenster öffnen und die Version von Python und PIP abrufen können:

```bat
python --version
pip --version
```

Installieren Sie dann die Service Fabric-Befehlszeilenschnittstelle mit dem folgenden Befehl:

```
pip install sfctl
sfctl -h
```

### <a name="ubuntu"></a>Ubuntu

Bei Ubuntu 16.04 Desktop können Sie Python 3.6 über eine Drittanbieter-PPA installieren:

Führen Sie am Terminal die folgenden Befehle aus:

```bash
sudo add-apt-repository ppa:jonathonf/python-3.6
sudo apt-get update
sudo apt-get install python3.6
sudo apt-get install python3-pip
```

Führen Sie dann zum Installieren von SFCTL für Ihre Installation von Python 3.6 den folgenden Befehl aus:

```bash
python3.6 -m pip install sfctl
sfctl -h
```

Diese Schritte haben keine Auswirkungen auf die vom System installierten Versionen Python 3.5 und 2.7. Versuchen Sie nicht, diese Installationen zu ändern, insofern Sie nicht mit Ubuntu vertraut sind.

### <a name="macos"></a>macOS

Verwenden Sie bei MacOS den [HomeBrew-Paketmanager](https://brew.sh). Installieren Sie Homebrew, falls nicht bereits installiert, indem Sie den folgenden Befehl ausführen:

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Installieren Sie dann auf dem Terminal Python 3.6, PIP und SFCTL.

```bash
brew install python3
pip3 install sfctl
sfctl -h
```

Diese Schritte haben keine Auswirkungen auf die Systeminstallation von Python 2.7.

## <a name="cli-syntax"></a>CLI-Syntax

Befehle erhalten stets das Präfix `sfctl`. Allgemeine Informationen zu allen Befehlen, die Sie nutzen können, erhalten Sie mit `sfctl -h`. Hilfe zu einem einzelnen Befehl erhalten Sie mit `sfctl <command> -h`.

Befehle folgen einer wiederholbaren Struktur. Dabei steht das Ziel des Befehls vor dem Verb oder der Aktion:

```azurecli
sfctl <object> <action>
```

In diesem Beispiel ist `<object>` das Ziel für `<action>`.

## <a name="select-a-cluster"></a>Auswählen eines Clusters

Bevor Sie Vorgänge durchführen, müssen Sie einen Cluster auswählen, mit dem die Verbindung hergestellt wird. Führen Sie beispielsweise folgenden Befehl aus, um den Cluster `testcluster.com` auszuwählen und eine Verbindung mit ihm herzustellen.

> [!WARNING]
> Vermeiden Sie die Verwendung von ungeschützten Service Fabric-Clustern in einer Produktionsumgebung.

```azurecli
sfctl cluster select --endpoint http://testcluster.com:19080
```

Der Clusterendpunkt muss das Präfix `http` oder `https` aufweisen. Er muss den Port für das HTTP-Gateway enthalten. Der Port und die Adresse entsprechen der Service Fabric Explorer-URL.

Für Cluster, die mit einem Zertifikat gesichert sind, können Sie ein PEM-codiertes Zertifikat angeben. Das Zertifikat kann als eine einzelne Datei oder als Zertifikat- und Schlüsselpaar angegeben werden.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md).

## <a name="basic-operations"></a>Einfache Vorgänge

Informationen zur Clusterverbindung werden über mehrere sfctl-Sitzungen hinweg beibehalten. Nach dem Auswählen eines Service Fabric-Clusters können Sie im Cluster einen beliebigen Service Fabric-Befehl ausführen.

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

Hier finden Sie einige Vorschläge und Tipps zum Beheben allgemeiner Probleme.

### <a name="convert-a-certificate-from-pfx-to-pem-format"></a>Konvertieren eines Zertifikats aus dem PFX- in das PEM-Format

Die Service Fabric-Befehlszeilenschnittstelle unterstützt clientseitige Zertifikate als PEM-Dateien (Erweiterung „.pem“). Bei Verwendung von PFX-Dateien in Windows müssen Sie diese Zertifikate in das PEM-Format konvertieren. Mit dem folgenden Befehl konvertieren Sie eine PFX-Datei in eine PEM-Datei:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Weitere Informationen finden Sie in der [OpenSSL-Dokumentation](https://www.openssl.org/docs/).

### <a name="connection-issues"></a>Verbindungsprobleme

Für einige Vorgänge wird unter Umständen die folgende Meldung generiert:

`Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

Stellen Sie sicher, dass der angegebene Clusterendpunkt verfügbar ist und lauscht. Überprüfen Sie auch, ob die Service Fabric Explorer-Benutzeroberfläche unter diesem Host und Port verfügbar ist. Verwenden Sie `sfctl cluster select`, um den Endpunkt zu aktualisieren.

### <a name="detailed-logs"></a>Detaillierte Protokolle

Detaillierte Protokolle sind häufig hilfreich, wenn Sie das Debuggen durchführen oder ein Problem melden. Es gibt ein globales `--debug`-Flag, das die Ausführlichkeit von Protokolldateien erhöht.

### <a name="command-help-and-syntax"></a>Befehle – Hilfe und Syntax

Verwenden Sie das `-h`-Flag, um Hilfe zu einem bestimmten Befehl oder einer Gruppe von Befehlen zu erhalten:

```azurecli
sfctl application -h
```

Ein weiteres Beispiel:

```azurecli
sfctl application create -h
```

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen einer Anwendung mit der Azure Service Fabric-Befehlszeilenschnittstelle](service-fabric-application-lifecycle-sfctl.md)
* [Erste Schritte mit Service Fabric unter Linux](service-fabric-get-started-linux.md)

