---
title: Erste Schritte mit Azure Service Fabric und Azure CLI 2.0
description: "Enthält Informationen zur Verwendung des Moduls mit den Azure Service Fabric-Befehlen in Version 2.0 der Azure CLI. Es wird beschrieben, wie Sie eine Verbindung mit einem Cluster herstellen und Anwendungen verwalten."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: edwardsa
ms.translationtype: HT
ms.sourcegitcommit: 49bc337dac9d3372da188afc3fa7dff8e907c905
ms.openlocfilehash: ee3302b984ca2f5509755dc17b0a5fd06ace0afe
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---
# <a name="azure-service-fabric-and-azure-cli-20"></a>Azure Service Fabric und Azure CLI 2.0

Version 2.0 des Azure-Befehlszeilentools (Azure CLI) enthält Befehle, mit denen Sie Azure Service Fabric-Cluster verwalten können. Sie erfahren, wie Sie die ersten Schritte mit der Azure CLI und Service Fabric ausführen.

## <a name="install-azure-cli-20"></a>Installieren von Azure CLI 2.0

Sie können Azure CLI 2.0-Befehle verwenden, um mit Service Fabric-Clustern zu interagieren und diese zu verwalten. Befolgen Sie den [Prozess für die Azure CLI 2.0-Standardinstallation](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

Weitere Informationen finden Sie in der Übersicht über [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview).

## <a name="azure-cli-syntax"></a>Azure CLI-Syntax

In Azure CLI sind alle Service Fabric-Befehle mit dem Präfix `az sf` versehen. Allgemeine Informationen zu den Befehlen, die Sie nutzen können, erhalten Sie mit `az sf -h`. Hilfe zu einem einzelnen Befehl erhalten Sie mit `az sf <command> -h`.

Namensmuster für Service Fabric-Befehle in Azure CLI:

```azurecli
az sf <object> <action>
```

`<object>` ist das Ziel für `<action>`.

## <a name="select-a-cluster"></a>Auswählen eines Clusters

Bevor Sie Vorgänge durchführen, müssen Sie einen Cluster auswählen, mit dem die Verbindung hergestellt wird. Der folgende Code enthält ein Beispiel hierzu. Im Code wird eine Verbindung mit einem ungeschützten Cluster hergestellt.

> [!WARNING]
> Vermeiden Sie die Verwendung von ungeschützten Service Fabric-Clustern in einer Produktionsumgebung.

```azurecli
az sf cluster select --endpoint http://testcluster.com:19080
```

Der Clusterendpunkt muss das Präfix `http` oder `https` aufweisen. Er muss den Port für das HTTP-Gateway enthalten. Der Port und die Adresse entsprechen der Service Fabric Explorer-URL.

Für Cluster, die durch ein Zertifikat geschützt sind, können Sie entweder unverschlüsselte PEM-Dateien oder CRT- und KEY-Dateien verwenden. Beispiel:

```azurecli
az sf cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md).

> [!NOTE]
> Mit dem Befehl `select` werden vor der Rückgabe keine Schritte für Anforderungen ausgeführt. Verwenden Sie einen Befehl wie `az sf cluster health`, um sicherzustellen, dass Sie einen Cluster richtig angegeben haben. Vergewissern Sie sich, dass für den Befehl keine Fehler zurückgegeben werden.

## <a name="basic-operations"></a>Einfache Vorgänge

Informationen zur Clusterverbindung werden über mehrere Azure CLI-Sitzungen hinweg beibehalten. Nach dem Auswählen eines Service Fabric-Clusters können Sie im Cluster einen beliebigen Service Fabric-Befehl ausführen.

Verwenden Sie beispielsweise den folgenden Befehl, um den Integritätsstatus des Service Fabric-Clusters abzurufen:

```azurecli
az sf cluster health
```

Der Befehl ergibt die folgende Ausgabe (sofern in der Azure CLI-Konfiguration die JSON-Ausgabe angegeben ist):

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

Die folgenden Informationen können für Sie hilfreich sein, falls beim Verwenden von Service Fabric-Befehlen in Azure CLI Probleme auftreten.

### <a name="convert-a-certificate-from-pfx-to-pem-format"></a>Konvertieren eines Zertifikats aus dem PFX- in das PEM-Format

Azure CLI unterstützt clientseitige Zertifikate als PEM-Dateien (Erweiterung „.pem“). Bei Verwendung von PFX-Dateien in Windows müssen Sie diese Zertifikate in das PEM-Format konvertieren. Mit dem folgenden Befehl konvertieren Sie eine PFX-Datei in eine PEM-Datei:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Weitere Informationen finden Sie in der [OpenSSL-Dokumentation](https://www.openssl.org/docs/).

### <a name="connection-issues"></a>Verbindungsprobleme

Für einige Vorgänge wird unter Umständen die folgende Meldung generiert:

`Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

Stellen Sie sicher, dass der angegebene Clusterendpunkt verfügbar ist und lauscht. Überprüfen Sie auch, ob die Service Fabric Explorer-Benutzeroberfläche unter diesem Host und Port verfügbar ist. Verwenden Sie `az sf cluster select`, um den Endpunkt zu aktualisieren.

### <a name="detailed-logs"></a>Detaillierte Protokolle

Detaillierte Protokolle sind häufig hilfreich, wenn Sie das Debuggen durchführen oder ein Problem melden. Azure CLI verfügt über ein globales `--debug`-Flag, mit dem die Ausführlichkeit von Protokolldateien erhöht wird.

### <a name="command-help-and-syntax"></a>Befehle – Hilfe und Syntax

Für Service Fabric-Befehle gelten die gleichen Konventionen wie für die Azure CLI. Verwenden Sie das `-h`-Flag, um Hilfe zu einem bestimmten Befehl oder einer Gruppe von Befehlen zu erhalten:

```azurecli
az sf application -h
```

Hier ist ein weiteres Beispiel angegeben:

```azurecli
az sf application create -h
```

