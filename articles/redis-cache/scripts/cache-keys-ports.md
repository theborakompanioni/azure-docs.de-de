---
title: "Azure CLI-Skriptbeispiel: Abrufen des Hostnamens, der Ports und Schlüssel für Azure Redis Cache | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel: Abrufen des Hostnamens, der Ports und Schlüssel für eine Azure Redis Cache-Instanz"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: cd9adc784bceb0fff5e7c2bbee2be0950c51c8f6
ms.lasthandoff: 04/15/2017

---

# <a name="get-the-hostname-ports-and-keys-for-azure-redis-cache"></a>Abrufen des Hostnamens, der Ports und Schlüssel für Azure Redis Cache

In diesem Szenario erfahren Sie, wie den Hostnamen, die Ports und Schlüssel für Azure Redis Cache abrufen, um eine Verbindung mit einer Azure Redis Cache-Instanz herzustellen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um den Hostnamen, die Schlüssel und Ports einer Azure-Redis-Cache-Instanz abzurufen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az redis show](https://docs.microsoft.com/cli/azure/redis#show) | Abrufen von Details einer Azure Redis Cache-Instanz. |
| [az redis list-keys](https://docs.microsoft.com/cli/azure/redis#list-keys) | Abrufen von Zugriffsschlüsseln für eine Azure Redis Cache-Instanz. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche Azure Redis Cache CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Redis Cache](../cli-samples.md).
