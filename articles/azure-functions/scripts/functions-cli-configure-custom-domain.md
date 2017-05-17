---
title: "Azure CLI-Skriptbeispiel – Zuordnen einer benutzerdefinierten Domäne zu einer Funktionen-App | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Zuordnen einer benutzerdefinierten Domäne zu einer Funktionen-App in Azure."
services: functions
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: d127e347-7581-47d7-b289-e0f51f2fbfbc
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 04/09/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 60005cb19dd78e9ef7eec02f889b37fd58db0dfd
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="map-a-custom-domain-to-a-function-app"></a>Zuordnen einer benutzerdefinierten Domäne zu einer Funktionen-App

Dieses Beispielskript erstellt eine Funktionen-App mit den zugehörigen Ressourcen und ordnet ihr dann `www.<yourdomain>` zu. Um eine benutzerdefinierte Domäne zuzuordnen, muss Ihre Funktionen-App in einem App Service-Plan und nicht in einem Verbrauchsplan erstellt werden. Azure Functions unterstützt nur die Zuordnung einer benutzerdefinierten Domäne mithilfe eines A-Eintrags.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Zuordnen einer benutzerdefinierten Domäne zu einer Funktionen-App")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#create) | Erstellt ein Speicherkonto, das für die Funktionen-App erforderlich ist. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan, der zum Zuordnen einer benutzerdefinierten Domäne erforderlich ist. |
| [az functionapp create]() | Erstellt eine Funktionen-App. |
| [az appservice web config hostname add](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | Ordnet eine benutzerdefinierte Domäne zu einer Funktionen-App zu. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Weitere Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions]().
