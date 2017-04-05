---
title: "Problembehandlung bei Azure Virtual Network-Gateways und -Verbindungen – Azure CLI | Microsoft-Dokumentation"
description: "Auf dieser Seite wird erläutert, wie die Azure CLI in Azure Network Watcher zur Problembehandlung verwendet wird."
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
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: 0dcc02e4f3b1e512a2c9e4b96894324414182af9
ms.lasthandoff: 03/28/2017


---

# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli"></a>Problembehandlung bei Virtual Network-Gateways und -Verbindungen mit der Azure CLI in Azure Network Watcher

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [BEFEHLSZEILENSCHNITTSTELLE (CLI)](network-watcher-troubleshoot-manage-cli.md)
> - [REST-API](network-watcher-troubleshoot-manage-rest.md)

Network Watcher bietet viele Funktionen zum Erfassen von Informationen über Ihre Netzwerkressourcen in Azure. Zu diesen Funktionen zählt die Ressourcenproblembehandlung. Die Ressourcenproblembehandlung kann über PowerShell, die CLI oder die REST-API aufgerufen werden. Bei Aufruf untersucht Network Watcher die Integrität von Virtual Network-Gateways oder -Verbindungen und gibt entsprechende Ergebnisse zurück.

In diesem Artikel wird die plattformübergreifende Azure CLI 1.0 verwendet, die für Windows, Mac und Linux zur Verfügung steht. Network Watcher verwendet derzeit Azure CLI 1.0 als Unterstützung für die Befehlszeilenschnittstelle.

## <a name="before-you-begin"></a>Voraussetzungen

Dieses Szenario setzt voraus, dass Sie die Schritte unter [Create an Azure Network Watcher instance](network-watcher-create.md) (Erstellen einer Azure Network Watcher-Instanz) bereits ausgeführt haben, um eine Network Watcher-Instanz zu erstellen.

## <a name="overview"></a>Übersicht

Die Ressourcenproblembehandlung bietet die Möglichkeit zum Behandeln von Problemen, die bei Virtual Network-Gateways und -Verbindungen auftreten können. Wenn eine Anforderung an die Ressourcenproblembehandlung gesendet wird, werden Protokolle abgefragt und untersucht. Nach Abschluss der Untersuchung werden die Ergebnisse zurückgegeben. Anforderungen der Ressourcenproblembehandlung sind Anforderungen mit langer Ausführungszeit. Daher kann es mehrere Minuten dauern, bis ein Ergebnis zurückgegeben wird. Die Protokolle der Problembehandlung werden in einem Container in einem angegebenen Speicherkonto gespeichert.

## <a name="retrieve-a-virtual-network-gateway-connection"></a>Abrufen einer Virtual Connection-Gatewayverbindung

In diesem Beispiel wird die Ressourcenproblembehandlung für eine Verbindung ausgeführt. Sie können auch ein Virtual Network-Gateway übergeben. Das folgende Cmdlet listet die VPN-Verbindungen in einer Ressourcengruppe auf.

```azurecli
azure network vpn-connection list -g resourceGroupName
```

Sie können den Befehl auch ausführen, um die Verbindungen in einem Abonnement anzuzeigen.

```azurecli
azure network vpn-connection list -s subscription
```

Sobald Sie den Namen des Speicherkontos kennen, können Sie mit diesem Befehl die zugehörige Ressourcen-ID abrufen:

```azurecli
azure network vpn-connection show -g resourceGroupName -n connectionName
```

## <a name="create-a-storage-account"></a>Erstellen eines Speicherkontos

Die Ressourcenproblembehandlung gibt Daten zur Integrität der Ressource zurück. Zudem speichert sie Protokolle zur Überprüfung in einem Speicherkonto. In diesem Schritt erstellen wir ein Speicherkonto. Wenn bereits ein Speicherkonto vorhanden ist, können Sie es verwenden.

```azurecli
azure storage account create -n storageAccountName -l location -g resourceGroupName
```

## <a name="run-network-watcher-resource-troubleshooting"></a>Ausführen der Network Watcher-Ressourcenproblembehandlung

Die Problembehandlung für Ressourcen wird mithilfe des Cmdlets `network watcher troubleshoot` durchgeführt. Wir geben im Cmdlet die Ressourcengruppe, den Namen der Network Watcher-Instanz, die ID der Verbindung, die ID des Speicherkontos und den Pfad zu dem Blob an, in dem die Ergebnisse der Problembehandlung gespeichert werden sollen.

```azurecli
azure network watcher -g resourceGroupName -n networkWatcherName -t connectionId -i storageId -p storagePath
```

Sobald Sie das Cmdlet ausführen, überprüft Network Watcher die Ressource auf ihre Integrität. Die Ergebnisse werden an die Shell zurückgegeben, und Protokolle der Ergebnisse werden im angegebenen Speicherkonto gespeichert.

## <a name="understanding-the-results"></a>Grundlegendes zu den Ergebnissen

Der Aktionstext enthält allgemeine Richtlinien zur Behebung des Problems. Wenn eine Aktion für das Problem ausgeführt werden kann, wird ein Link mit weiteren Anleitungen bereitgestellt. Falls es keine weitere Anleitungen gibt, enthält die Antwort die URL zum Öffnen einer Supportanfrage.  Weitere Informationen zu den Eigenschaften der Antwort und zu deren Inhalt finden Sie unter [Übersicht über die Network Watcher-Problembehandlung](network-watcher-troubleshoot-overview.md).

Anweisungen zum Herunterladen von Dateien von Azure-Speicherkonten finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](../storage/storage-dotnet-how-to-use-blobs.md). Ein weiteres Tool, das verwendet werden kann, ist der Speicher-Explorer. Weitere Informationen zum Speicher-Explorer finden Sie unter [Speicher-Explorer](http://storageexplorer.com/).

## <a name="next-steps"></a>Nächste Schritte

Wenn Einstellungen geändert wurden und die VPN-Konnektivität beenden, finden Sie unter [Verwalten von Netzwerksicherheitsgruppen](../virtual-network/virtual-network-manage-nsg-arm-portal.md) Informationen zum Ermitteln der fraglichen Netzwerksicherheitsgruppe und der Sicherheitsregeln.

