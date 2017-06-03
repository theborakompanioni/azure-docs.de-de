---
title: "Erstellen einer Funktionen-App und Bereitstellen von Funktionscode über GitHub | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen einer Funktionen-App und Bereitstellen von Funktionscode über GitHub"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: f87cf7d300b4c2b89ad692aadcda958e9747c7f9
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017

---
# <a name="create-a-function-app-and-deploy-function-code-from-github"></a>Erstellen einer Funktionen-App und Bereitstellen von Funktionscode über GitHub

Dieses Beispielskript erstellt mit dem [Verbrauchsplan](../functions-scale.md#consumption-plan) eine Funktionen-App mit den zugehörigen Ressourcen und stellt Ihren Funktionscode aus einem öffentlichen GitHub-Repository (ohne Continuous Deployment) bereit. Informationen zu Continuous Delivery von Funktionscode über GitHub finden Sie unter [Erstellen einer Funktionen-App und kontinuierliches Bereitstellen über GitHub](functions-cli-create-function-app-github-continuous.md).

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

Dieses Beispiel erstellt eine Azure-Funktionen-App und stellt Funktionscode über GitHub bereit.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github/deploy-function-app-with-function-github.sh?highlight=3 "Erstellen einer Funktionen-App mit Bereitstellung über GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft. Das Skript verwendet die folgenden Befehle:

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/appservice/web#delete) | Erstellt eine Azure-Funktionen-App. |
| [az appservice web source-control config](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | Ordnet eine Funktionen-App einem Git- oder Mercurial-Repository zu. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).

