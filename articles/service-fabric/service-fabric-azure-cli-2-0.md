---
title: Erste Schritte mit Service Fabric und der Azure CLI 2.0
description: Es wird beschrieben, wie Sie das Service Fabric-Befehlsmodul in der Azure CLI, Version 2.0, verwenden und Clusterverbindungen herstellen und Anwendungen verwalten.
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: edwardsa
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: c5cc6e54acf27456185eeb48858c4d981aa46b4b
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017

---
<a id="service-fabric-and-azure-cli-20" class="xliff"></a>

# Service Fabric und Azure CLI 2.0

Die neue Azure CLI 2.0 enthält jetzt Befehle zum Verwalten von Service Fabric-Clustern. In dieser Dokumentation werden die ersten Schritte für die Azure CLI beschrieben.

<a id="install-azure-cli-20" class="xliff"></a>

## Installieren von Azure CLI 2.0

Die Azure CLI enthält jetzt Befehle zum Interagieren mit und Verwalten von Service Fabric-Clustern. Sie können den [Standardinstallationsprozess](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) befolgen, um die neueste Azure CLI zu erhalten.

Weitere Informationen finden Sie in der [Azure CLI 2.0-Dokumentation](https://docs.microsoft.com/en-us/cli/azure/overview).

<a id="cli-syntax" class="xliff"></a>

## CLI-Syntax

Alle Azure Service Fabric-Befehle weisen in der Azure CLI das Präfix `az sf` auf. Weitere allgemeine Informationen zu den verfügbaren Befehlen erhalten Sie, indem Sie `az sf -h` ausführen. Sie können auch `az sf <command> -h` ausführen, um ausführliche Hilfe zu einem einzelnen Befehl zu erhalten.

Namensmuster für Azure Service Fabric-Befehle in der CLI

```azurecli
az sf <object> <action>
```

Hier ist `<object>` das Ziel für `<action>`.

<a id="selecting-a-cluster" class="xliff"></a>

## Auswählen eines Clusters

Bevor Sie Vorgänge durchführen können, müssen Sie einen Cluster auswählen, mit dem die Verbindung hergestellt wird. Der folgende Codeausschnitt enthält beispielsweise Code zum Herstellen einer Verbindung mit einem nicht gesicherten Cluster.

> [!WARNING]
> Vermeiden der Verwendung von nicht gesicherten Service Fabric-Clustern für Produktionsumgebungen

```azurecli
az sf cluster select --endpoint http://testcluster.com:19080
```

Der Clusterendpunkt muss das Präfix `http` oder `https` aufweisen und sollte den Port für das HTTP-Gateway enthalten. Dieser Port und diese Adresse entsprechen der Service Fabric Explorer-URL.

Für Cluster, die mit einem Zertifikat gesichert werden, werden entweder unverschlüsselte `pem`- oder `crt`- und `key`-Dateien unterstützt.

```azurecli
az sf cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Weitere Informationen finden Sie im [ausführlichen Dokument zur Herstellung der Verbindung mit sicheren Clustern](service-fabric-connect-to-secure-cluster.md).

> [!NOTE]
> Mit dem Select-Befehl werden vor der Rückgabe keine Anforderungen durchgeführt. Gehen Sie wie folgt vor, um zu überprüfen, ob ein Cluster richtig angegeben wurde: Führen Sie einen Befehl wie `az sf cluster health` aus, und stellen Sie sicher, dass für den Befehl keine Fehler zurückgegeben werden.

<a id="performing-basic-operations" class="xliff"></a>

## Durchführen von grundlegenden Vorgängen

Clusterverbindungsinformationen werden über verschiedene Azure CLI-Sitzungen hinweg beibehalten. Nach der Auswahl eines Service Fabric-Clusters können Sie einen beliebigen Service Fabric-Befehl ausführen.

Führen Sie beispielsweise den folgenden Befehl aus, um den Integritätsstatus des Service Fabric-Clusters abzurufen:

```azurecli
az sf cluster health
```

Der Befehl ergibt die folgende Ausgabe, wenn in der Azure CLI-Konfiguration die JSON-Ausgabe angegeben ist:

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

<a id="tips-and-faq" class="xliff"></a>

## Tipps und häufig gestellte Fragen

Hier sind einige Informationen angegeben, die hilfreich sein können, wenn beim Verwenden der Service Fabric-Befehle in der Azure CLI Probleme auftreten.

<a id="converting-a-certificate-from-pfx-to-pem" class="xliff"></a>

### Konvertieren eines Zertifikats aus PFX in PEM

Die Azure CLI unterstützt clientseitige Zertifikate als PEM-Dateien (Erweiterung `.pem`). Bei Verwendung von PFX-Dateien aus Windows müssen diese Zertifikate in das PEM-Format konvertiert werden. Mit dem folgenden Befehl konvertieren Sie eine PFX-Datei in eine PEM-Datei:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Ausführliche Informationen finden Sie in der [OpenSSL-Dokumentation](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html).

<a id="connection-issues" class="xliff"></a>

### Verbindungsprobleme

Beim Durchführen von Vorgängen tritt ggf. der folgende Fehler auf:

> `Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

Stellen Sie in diesem Fall sicher, dass der angegebene Clusterendpunkt erreichbar ist und lauscht. Überprüfen Sie auch, ob die Service Fabric Explorer-Benutzeroberfläche unter diesem Host und Port erreichbar ist. Verwenden Sie `az sf cluster select`, um den Endpunkt zu aktualisieren.

<a id="getting-detailed-logs" class="xliff"></a>

### Abrufen von ausführlichen Protokollen

Beim Debuggen oder Melden eines Problems ist es nützlich, ausführliche Protokolle einzubeziehen. Die Azure CLI enthält ein globales `--debug`-Flag, das die Ausführlichkeit der Protokolle erhöht.

<a id="command-help-and-syntax" class="xliff"></a>

### Befehle – Hilfe und Syntax

Für die Service Fabric-Befehle gilt die gleiche Konvention wie für die Azure CLI. Geben Sie das `-h`-Flag an, um Hilfe zu einem bestimmten Befehl oder einer Gruppe von Befehlen zu erhalten. Beispiel:

```azurecli
az sf application -h
```

oder

```azurecli
az sf application create -h
```

