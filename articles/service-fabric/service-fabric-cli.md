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
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 5ce9adf6c82e3a5521883c5de1e0689d5bf0d94e
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="azure-service-fabric-command-line"></a>Azure Service Fabric-Befehlszeile

Bei der Azure Service Fabric-Befehlszeilenschnittstelle (sfctl) handelt es sich um ein Befehlszeilenprogramm für die Interaktion mit und die Verwaltung von Azure Service Fabric-Entitäten. sfctl kann mit Windows- oder Linux-Clustern verwendet werden. sfctl wird auf jeder Plattform ausgeführt, die Python unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie vor der Installation sicher, dass in Ihrer Umgebung Python und pip installiert sind. Weitere Informationen finden Sie in der [Schnellstartdokumentation zu pip](https://pip.pypa.io/en/latest/quickstart/) und in der offiziellen [Dokumentation zur Python-Installation](https://wiki.python.org/moin/BeginnersGuide/Download).

Python 2.7 und 3.6 werden unterstützt, es wird jedoch die Verwendung von Python 3.6 empfohlen.

## <a name="install"></a>Installieren

Die Azure Service Fabric CLI (sfctl) ist als Python-Paket gepackt. Führen Sie zur Installation der aktuellen Version Folgendes aus:

```bash
pip install sfctl
```

Führen Sie nach der Installation `sfctl -h` aus, um Informationen zu verfügbaren Befehlen zu erhalten.

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

