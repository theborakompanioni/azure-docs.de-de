---
title: "Erstellen einer Funktionen-App und Bereitstellen von Funktionscode über GitHub | Microsoft-Dokumentation"
description: "Erstellen einer Funktionen-App und Bereitstellen von Funktionscode über GitHub"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: functions
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: dc1b15d4e5c5ab0525b64e8844fe644cde3cb257
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="create-an-app-service"></a>Erstellen eines App Service

Dieses Beispielskript erstellt mit dem [Verbrauchsplan](../functions-scale.md#consumption-plan) eine Funktionen-App mit den zugehörigen Ressourcen und stellt Ihren Funktionscode kontinuierlich aus einem GitHub-Repository bereit. In diesem Beispiel benötigen Sie Folgendes:

* Ein GitHub-Repository mit Funktionscode, für das Sie über Administratorberechtigungen verfügen.
* Ein [persönliches Zugriffstoken (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) für Ihr GitHub-Konto.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure-Dienst")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft. Das Skript verwendet die folgenden Befehle:

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [az appservice web source-control config](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | Ordnet eine Funktionen-App einem Git- oder Mercurial-Repository zu. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).
