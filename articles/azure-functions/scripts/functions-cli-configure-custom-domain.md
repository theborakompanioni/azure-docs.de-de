---
title: "Azure CLI-Skriptbeispiel – Zuordnen einer benutzerdefinierten Domäne zu einer Funktionen-App | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Zuordnen einer benutzerdefinierten Domäne zu einer Funktionen-App in Azure."
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: d127e347-7581-47d7-b289-e0f51f2fbfbc
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/01/2017
ms.author: glenga
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 190ca4b228434a7d1b30348011c39a979c22edbd
ms.openlocfilehash: 87b79d6222f40e3dc1306ecace51bae50b06e484
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---
# <a name="map-a-custom-domain-to-a-function-app"></a>Zuordnen einer benutzerdefinierten Domäne zu einer Funktionen-App

Dieses Beispielskript erstellt eine Funktionen-App mit den zugehörigen Ressourcen und ordnet ihr dann `www.<yourdomain>` zu. Um eine benutzerdefinierte Domäne zuzuordnen, muss Ihre Funktionen-App in einem App Service-Plan und nicht in einem Verbrauchsplan erstellt werden. Azure Functions unterstützt nur die Zuordnung einer benutzerdefinierten Domäne mithilfe eines A-Eintrags.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, benötigen Sie mindestens die Version 2.0 der Azure CLI. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 


## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Zuordnen einer benutzerdefinierten Domäne zu einer Funktionen-App")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript benutzt die folgenden Befehle: Jeder Befehl in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Erstellt ein Speicherkonto, das für die Funktionen-App erforderlich ist. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Erstellt einen App Service-Plan, der zum Zuordnen einer benutzerdefinierten Domäne erforderlich ist. |
| [az functionapp create]() | Erstellt eine Funktionen-App. |
| [az appservice web config hostname add](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#az_appservice_web_config_hostname_add) | Ordnet eine benutzerdefinierte Domäne zu einer Funktionen-App zu. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Weitere Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions]().

