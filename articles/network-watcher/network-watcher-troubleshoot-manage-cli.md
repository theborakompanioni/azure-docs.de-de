---
title: "Problembehandlung bei Azure Virtual Network-Gateways und -Verbindungen – Azure CLI 2.0 | Microsoft-Dokumentation"
description: "Auf dieser Seite wird erläutert, wie die Azure CLI 2.0 in Azure Network Watcher zur Problembehandlung verwendet wird."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2838bc61-b182-4da8-8533-27db8fdbd177
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: 09270cf3181476f3ed2c1720b497e707edff880e
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---

# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-20"></a>Problembehandlung bei Virtual Network-Gateways und -Verbindungen mit der Azure CLI 2.0 in Azure Network Watcher

> [!div class="op_single_selector"]
> - [Portal](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST-API](network-watcher-troubleshoot-manage-rest.md)

Network Watcher bietet viele Funktionen zum Erfassen von Informationen über Ihre Netzwerkressourcen in Azure. Zu diesen Funktionen zählt die Ressourcenproblembehandlung. Die Ressourcenproblembehandlung kann über das Portal, PowerShell, die CLI oder die REST-API aufgerufen werden. Bei Aufruf untersucht Network Watcher die Integrität von Virtual Network-Gateways oder -Verbindungen und gibt entsprechende Ergebnisse zurück.

In diesem Artikel wird unsere Befehlszeilenschnittstelle der nächsten Generation für das Ressourcenverwaltungs-Bereitstellungsmodell verwendet, Azure CLI 2.0, die für Windows, Mac und Linux verfügbar ist.

Um die Schritte in diesem Artikel ausführen zu können, müssen Sie [die Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows (Azure CLI) installieren](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Voraussetzungen

Dieses Szenario setzt voraus, dass Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](network-watcher-create.md) bereits durchgeführt haben, um eine Network Watcher-Instanz zu erstellen.

Eine Liste mit unterstützten Gatewaytypen finden Sie unter [Unterstützte Gatewaytypen](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Übersicht

Die Ressourcenproblembehandlung bietet die Möglichkeit zum Behandeln von Problemen, die bei Virtual Network-Gateways und -Verbindungen auftreten können. Wenn eine Anforderung an die Ressourcenproblembehandlung gesendet wird, werden Protokolle abgefragt und untersucht. Nach Abschluss der Untersuchung werden die Ergebnisse zurückgegeben. Anforderungen der Ressourcenproblembehandlung sind Anforderungen mit langer Ausführungszeit. Daher kann es mehrere Minuten dauern, bis ein Ergebnis zurückgegeben wird. Die Protokolle der Problembehandlung werden in einem Container in einem angegebenen Speicherkonto gespeichert.

## <a name="retrieve-a-virtual-network-gateway-connection"></a>Abrufen einer Virtual Connection-Gatewayverbindung

In diesem Beispiel wird die Ressourcenproblembehandlung für eine Verbindung ausgeführt. Sie können auch ein Virtual Network-Gateway übergeben. Das folgende Cmdlet listet die VPN-Verbindungen in einer Ressourcengruppe auf.

```azurecli
az network vpn-connection list --resource-group resourceGroupName
```

Sobald Sie den Namen der Verbindung kennen, können Sie diesen Befehl ausführen, um die zugehörige Ressourcen-ID abzurufen:

```azurecli
az network vpn-connection show --resource-group resourceGroupName --ids vpnConnectionIds
```

## <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto.

Die Ressourcenproblembehandlung gibt Daten zur Integrität der Ressource zurück. Zudem speichert sie Protokolle zur Überprüfung in einem Speicherkonto. In diesem Schritt erstellen wir ein Speicherkonto. Wenn bereits ein Speicherkonto vorhanden ist, können Sie es verwenden.

1. Erstellen des Speicherkontos

    ```azurecli
    az storage account create --name storageAccountName --location westcentralus --resource-group resourceGroupName --sku Standard_LRS
    ```

1. Abrufen der Speicherkontoschlüssel

    ```azurecli
    az storage account keys list --resource-group resourcegroupName --account-name storageAccountName
    ```

1. Erstellen des Containers

    ```azurecli
    az storage container create --account-name storageAccountName --account-key {storageAccountKey} --name logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a>Ausführen der Network Watcher-Ressourcenproblembehandlung

Die Problembehandlung für Ressourcen wird mithilfe des Cmdlets `az network watcher troubleshooting` durchgeführt. Wir geben im Cmdlet die Ressourcengruppe, den Namen der Network Watcher-Instanz, die ID der Verbindung, die ID des Speicherkontos und den Pfad zu dem Blob an, in dem die Ergebnisse der Problembehandlung gespeichert werden sollen.

```azurecli
az network watcher troubleshooting start --resource-group resourceGroupName --resource resourceName --resource-type {vnetGateway/vpnConnection} --storage-account storageAccountName  --storage-path https://{storageAccountName}.blob.core.windows.net/{containerName}
```

Sobald Sie das Cmdlet ausführen, überprüft Network Watcher die Ressource auf ihre Integrität. Die Ergebnisse werden an die Shell zurückgegeben, und Protokolle der Ergebnisse werden im angegebenen Speicherkonto gespeichert.

## <a name="understanding-the-results"></a>Grundlegendes zu den Ergebnissen

Der Aktionstext enthält allgemeine Richtlinien zur Behebung des Problems. Wenn eine Aktion für das Problem ausgeführt werden kann, wird ein Link mit weiteren Anleitungen bereitgestellt. Falls es keine weitere Anleitungen gibt, enthält die Antwort die URL zum Öffnen einer Supportanfrage.  Weitere Informationen zu den Eigenschaften der Antwort und zu deren Inhalt finden Sie unter [Übersicht über die Network Watcher-Problembehandlung](network-watcher-troubleshoot-overview.md).

Anweisungen zum Herunterladen von Dateien von Azure-Speicherkonten finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](../storage/storage-dotnet-how-to-use-blobs.md). Ein weiteres Tool, das verwendet werden kann, ist der Speicher-Explorer. Weitere Informationen zum Speicher-Explorer finden Sie unter [Speicher-Explorer](http://storageexplorer.com/).

## <a name="next-steps"></a>Nächste Schritte

Wenn Einstellungen geändert wurden und die VPN-Konnektivität beenden, finden Sie unter [Verwalten von Netzwerksicherheitsgruppen](../virtual-network/virtual-network-manage-nsg-arm-portal.md) Informationen zum Ermitteln der fraglichen Netzwerksicherheitsgruppe und der Sicherheitsregeln.

