---
title: "Azure CLI-Skriptbeispiel – Löschen eines Azure Redis Cache | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Löschen eines Azure Redis Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 7beded7a-d2c9-43a6-b3b4-b8079c11de4a
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: sdanie
ms.translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: f959823b3a7c5b0262f693ecad1e6efc4eec4f35
ms.contentlocale: de-de
ms.lasthandoff: 04/15/2017

---

# <a name="delete-an-azure-redis-cache"></a>Löschen eines Azure Redis Cache

In diesem Szenario erfahren Sie, wie Sie einen Azure Redis Cache löschen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/redis-cache/delete-cache/delete-cache.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um eine Instanz eines Azure Redis Cache zu löschen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az redis delete](https://docs.microsoft.com/cli/azure/redis#delete) | Löscht eine Redis-Cache-Instanz. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche Azure Redis Cache CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Redis Cache](../cli-samples.md).
