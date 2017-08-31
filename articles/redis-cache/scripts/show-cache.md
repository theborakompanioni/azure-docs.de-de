---
title: "Azure CLI-Skriptbeispiel – Abrufen von Details zu einem Azure Redis Cache | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Abrufen von Details zu einem Azure Redis Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 155924e6-00d5-4a8c-ba99-5189f300464a
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: sdanie
ms.translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 9f4eb32227bd8a68837eabd58b9d058bc4995d17
ms.contentlocale: de-de
ms.lasthandoff: 04/15/2017

---

# <a name="get-details-of-an-azure-redis-cache"></a>Abrufen von Details zu einem Azure Redis Cache

In diesem Szenario erfahren Sie, wie die Details einer Azure Redis Cache-Instanz abgerufen werden, einschließlich ihres Bereitstellungsstatus.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/redis-cache/show-cache/show-cache.sh "Azure Redis Cache")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um die Details einer Azure Redis Cache-Instanz abzurufen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az redis show](https://docs.microsoft.com/cli/azure/redis#show) | Details zu einer Azure Redis Cache-Instanz abrufen. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche Azure Redis Cache CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Redis Cache](../cli-samples.md).
