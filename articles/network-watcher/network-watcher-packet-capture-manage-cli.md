---
title: "Verwalten von Paketerfassungen mit Azure Network Watcher – Azure CLI | Microsoft-Dokumentation"
description: "Auf dieser Seite wird erläutert, wie das Network Watcher-Feature zur Paketerfassung mithilfe der Azure CLI verwaltet wird."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: cb0c1d10-f7f2-4c34-b08c-f73452430be8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: d2a65104743d9497debdc85c134fd1a06114c514
ms.openlocfilehash: 279664a51eab79c42a14ed9c9bb5f65cc43aaab2
ms.lasthandoff: 02/23/2017

---

# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-cli"></a>Verwalten von Paketerfassungen mit Azure Network Watcher mithilfe der Azure CLI

> [!div class="op_single_selector"]
> - [Azure-Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [BEFEHLSZEILENSCHNITTSTELLE (CLI)](network-watcher-packet-capture-manage-cli.md)
> - [Azure-REST-API](network-watcher-packet-capture-manage-rest.md)

Mithilfe der Paketerfassung von Network Watcher können Sie Sitzungen erfassen, um den eingehenden und ausgehenden Datenverkehr eines virtuellen Computers nachzuverfolgen. Für die Erfassungssitzung werden Filter bereitgestellt, um sicherzustellen, dass nur der gewünschte Datenverkehr erfasst wird. Mithilfe der Paketerfassung können Sie Netzwerkanomalien sowohl reaktiv als auch proaktiv diagnostizieren. Weitere Verwendungszwecke sind das Erfassen von Netzwerkstatistiken, das Gewinnen von Informationen zu Netzwerkangriffen, das Debuggen der Kommunikation zwischen Client und Server und vieles mehr. Durch die Möglichkeit zur Remoteauslösung von Paketerfassungen erleichtert diese Funktion die manuelle Ausführung einer Paketerfassung auf dem gewünschten Computer. So sparen Sie wertvolle Zeit.

Dieser Artikel führt Sie durch die verschiedenen Verwaltungsaufgaben, die derzeit für die Paketerfassung verfügbar sind.

- [**Starten einer Paketerfassung**](#start-a-packet-capture)
- [**Beenden einer Paketerfassung**](#stop-a-packet-capture)
- [**Löschen einer Paketerfassung**](#delete-a-packet-capture)
- [**Herunterladen einer Paketerfassung**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie über die folgenden Ressourcen verfügen:

- Eine Instanz von Network Watcher in der Region, in der Sie eine Paketerfassung erstellen möchten
- Ein virtueller Computer mit aktivierter Paketerfassungserweiterung

> [!IMPORTANT]
> Für die Paketerfassung muss ein Agent auf dem virtuellen Computer ausgeführt werden. Der Agent wird als Erweiterung installiert. Anweisungen zu VM-Erweiterungen finden Sie unter [Erweiterungen und Features für virtuelle Computer](../virtual-machines/virtual-machines-windows-extensions-features.md).

## <a name="install-vm-extension"></a>Installieren der VM-Erweiterung

### <a name="step-1"></a>Schritt 1

Führen Sie das Cmdlet `azure vm extension set` aus, um den Paketerfassungs-Agent auf dem virtuellen Gastcomputer zu installieren.

Für virtuelle Windows-Computer:

```azurecli
azure vm extension set -g resourceGroupName -m virtualMachineName -p Microsoft.Azure.NetworkWatcher -r anyExtensionReferenceName -n NetworkWatcherAgentWindows -o 1.4
```

### <a name="step-2"></a>Schritt 2

Um sicherzustellen, dass der Agent installiert ist, führen Sie das Cmdlet `vm extension get` aus, und geben Sie die Ressourcengruppe und den Namen des virtuellen Computers an. Überprüfen Sie die Ergebnisliste, um sicherzustellen, dass der Agent installiert ist.

```azurecli
azure vm extension get -g resourceGroupName -m virtualMachineName
```

Das folgende Beispiel zeigt die Antwort nach der Ausführung von `azure vm extension get`.

```
info:    Executing command vm extension get
+ Looking up the VM "virtualMachineName"
data:    Publisher                       Name                     Version  State
data:    ------------------------------  -----------------------  -------  ---------
data:    Microsoft.Azure.NetworkWatcher  NetworkWatcherAgentTest  1.4      Succeeded
info:    vm extension get command OK
```

## <a name="start-a-packet-capture"></a>Starten einer Paketerfassung

Nachdem Sie die vorherigen Schritte abgeschlossen haben, wird der Paketerfassungs-Agent auf dem virtuellen Computer installiert.

### <a name="step-1"></a>Schritt 1

Der nächste Schritt besteht im Abrufen der Network Watcher-Instanz. Diese Variable wird in Schritt 4 an das Cmdlet `network watcher show` übergeben.

```azurecli
azure network watcher show -g resourceGroup -n networkWatcherName
```

### <a name="step-2"></a>Schritt 2

Erstellen Sie ein Speicherkonto. Dieses Speicherkonto wird verwendet, um die Paketerfassungsdatei zu speichern.

```azurecli
azure storage account list
```

### <a name="step-3"></a>Schritt 3

Mithilfe von Filtern können die von der Paketerfassung gespeicherten Daten eingeschränkt werden. Im folgenden Beispiel wird eine Paketerfassung mit mehreren Filtern eingerichtet.  Die ersten drei Filter erfassen nur ausgehenden TCP-Datenverkehr von der lokalen IP-Adresse 10.0.0.3 an die Zielports 20, 80 und 443.  Mit dem letzten Filter wird nur UDP-Datenverkehr erfasst.

```azurecli
azure network watcher packet-capture create -g resourceGroupName -w networkWatcherName -n packetCaptureName -t targetResourceId -o storageAccountResourceId -f "[{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"20\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"80\"},{\"protocol\":\"TCP\", \"remoteIPAddress\":\"1.1.1.1-255.255.255\",\"localIPAddress\":\"10.0.0.3\", \"remotePort\":\"443\"},{\"protocol\":\"UDP\"}]"
```

Für eine Paketerfassung können mehrere Filter definiert werden. Wenn Sie eine komplexe Filterstruktur verwenden, empfiehlt es sich, Filter als JSON-Datei zu verwenden, um Syntaxfehler zu vermeiden. Verwenden Sie beispielsweise das Flag „-r“ (anstelle von „-f“), und übergeben Sie den Speicherort einer JSON-Datei mit den folgenden Filtern:

```json
[
    {
        "protocol":"TCP",
        "remoteIPAddress":"1.1.1.1-255.255.255",
        "localIPAddress":"10.0.0.3",
        "remotePort":"20"
    },
    {
        "protocol":"TCP",
        "remoteIPAddress":"1.1.1.1-255.255.255",
        "localIPAddress":"10.0.0.3",
        "remotePort":"80"
    },
    {
        "protocol":"TCP",
        "remoteIPAddress":"1.1.1.1-255.255.255",
        "localIPAddress":"10.0.0.3",
        "remotePort":"443"
    },
    {
        "protocol":"UDP"
    }
]
```


Das folgende Beispiel zeigt die erwartete Ausgabe nach Ausführen des Cmdlets `network watcher packet-capture create`.

```
data:    Name                            : packetCaptureName
data:    Etag                            : W/"d59bb2d2-dc95-43da-b740-e0ef8fcacecb"
data:    Target                          : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/testVM
data:    Bytes To Capture Per Packet     : 0
data:    Total Bytes Per Session         : 1073741824
data:    Time Limit In Seconds           : 18000
data:    Storage Location:
data:      Storage Id                    : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/testStorage
data:      Storage Path                  : https://testStorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/testRG/providers/microsoft.compute/virtualmachines/testVM/2017/02/17/packetcapture_01_21_18_145.cap
data:    Filters                         : []
data:    Provisioning State              : Succeeded
info:    network watcher packet-capture create command OK
```

## <a name="get-a-packet-capture"></a>Abrufen einer Paketerfassung

Durch Ausführen des Cmdlets `network watcher packet-capture show` wird der Status einer zurzeit durchgeführten oder abgeschlossenen Paketerfassung abgerufen.

```azurecli
azure network watcher packet-capture show -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

Das folgende Beispiel zeigt die Ausgabe des Cmdlets `network watcher packet-capture show`. Im folgenden Beispiel ist die Erfassung abgeschlossen. Der PacketCaptureStatus-Wert lautet „Stopped“ mit dem StopReason „TimeExceeded“. Dieser Wert zeigt, dass die Paketerfassung erfolgreich war und über den dafür festgelegten Zeitraum ausgeführt wurde.

```
data:    Name                            : packetCaptureName
data:    Etag                            : W/"d59bb2d2-dc95-43da-b740-e0ef8fcacecb"
data:    Target                          : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/testVM
data:    Bytes To Capture Per Packet     : 0
data:    Total Bytes Per Session         : 1073741824
data:    Time Limit In Seconds           : 18000
data:    Storage Location:
data:      Storage Id                    : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/testStorage
data:      Storage Path                  : https://testStorage.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/testRG/providers/microsoft.compute/virtualmachines/testVM/2017/02/17/packetcapture_01_21_18_145.cap
data:    Filters                         : []
data:    Provisioning State              : Succeeded
info:    network watcher packet-capture show command OK
```

## <a name="stop-a-packet-capture"></a>Beenden einer Paketerfassung

Durch Ausführen des Cmdlets `network watcher packet-capture stop` wird eine Sitzung, die ggf. gerade ausgeführt wird, beendet.

```azurecli
azure network watcher packet-capture stop -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> Das Cmdlet gibt keine Antwort zurück, wenn es für eine aktuell ausgeführte Erfassungssitzung oder eine vorhandene Sitzung ausgeführt wird, die bereits beendet wurde.

## <a name="delete-a-packet-capture"></a>Löschen einer Paketerfassung

```azurecli
azure network watcher packet-capture delete -g resourceGroupName -w networkWatcherName -n packetCaptureName
```

> [!NOTE]
> Durch das Löschen einer Paketerfassung wird die Datei im Speicherkonto nicht gelöscht.

## <a name="download-a-packet-capture"></a>Herunterladen einer Paketerfassung

Nachdem die Paketerfassungssitzung abgeschlossen wurde, kann die Erfassungsdatei in den Blobspeicher oder in eine lokale Datei auf dem virtuellen Computer hochgeladen werden. Der Speicherort der Paketerfassung wird beim Erstellen der Sitzung definiert. Ein nützliches Tool für den Zugriff auf diese in einem Speicherkonto gespeicherten Erfassungsdateien ist der Microsoft Azure-Speicher-Explorer, der hier heruntergeladen werden kann: http://storageexplorer.com/

Wenn ein Speicherkonto angegeben wird, werden Paketerfassungsdateien in einem Speicherkonto am folgenden Speicherort gespeichert:

```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Automatisierung von Paketerfassungen mit VM-Warnungen finden Sie unter [Erstellen einer durch Warnungen ausgelösten Paketerfassung](network-watcher-alert-triggered-packet-capture.md).

Um herauszufinden, ob bestimmter eingehender oder ausgehender Datenverkehr für Ihren virtuellen Computer zulässig ist, lesen Sie den Artikel zur [IP-Datenflussüberprüfung](network-watcher-check-ip-flow-verify-portal.md).

<!-- Image references -->

