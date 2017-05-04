---
title: "Azure CLI-Skriptbeispiel – Erstellen eines Azure Redis Cache vom Typ „Premium“ mit Clustering | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen eines Azure Redis Cache mit Premium-Tarif mit Clustering"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 07bcceae-2521-4fe3-b88f-ed833104ddd2
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 87d0fe4c3eaa8f7b75343a36a069ecdac8241d74
ms.lasthandoff: 04/15/2017

---

# <a name="create-a-premium-azure-redis-cache-with-clustering"></a>Erstellen eines Azure Redis Cache vom Typ „Premium“ mit Clustering

In diesem Szenario erfahren Sie, wie Sie einen 6 GB-Azure Redis Cache mit Premium-Tarif erstellen. Dabei ist Clustering aktiviert, und es sind zwei Shards vorhanden.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe und einen Redis-Cache mit Premium-Tarif und aktiviertem Clustering zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az redis create](https://docs.microsoft.com/cli/azure/redis#create) | Erstellt eine Redis-Cache-Instanz. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche Azure Redis Cache CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Redis Cache](../cli-samples.md).
