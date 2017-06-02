---
title: "Erstellen einer Funktionen-App und Bereitstellen von Funktionscode über Visual Studio Team Services | Microsoft-Dokumentation"
description: "Erstellen einer Funktionen-App und Bereitstellen von Funktionscode über Visual Studio Team Services"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/28/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: 8e5d8bdf61746d3bda5acc7bed97b164c311a3c3
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017

---
# <a name="create-an-app-service"></a>Erstellen eines App Service

In diesem Szenario erfahren Sie, wie eine Funktionen-App mit dem [Verbrauchsplan](../functions-scale.md#consumption-plan) mit den zugehörigen Ressourcen erstellt wird und kontinuierlich Ihren Funktionscode über ein Visual Studio Team Services-Repository (VSTS) bereitstellt. In diesem Beispiel benötigen Sie Folgendes:

* Ein VSTS-Repository mit Funktionscode, für das Sie über Administratorberechtigungen verfügen.
* Ein [persönliches Zugriffstoken (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) für Ihr GitHub-Konto.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

Dieses Beispiel erstellt eine Azure-Funktionen-App und stellt Funktionscode über Visual Studio Team Services bereit.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure-Dienst")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine Web-App, die DocumentDB-Instanz und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/appservice/web#delete) |
| [az appservice web source-control config](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | Ordnet eine Funktionen-App einem Git- oder Mercurial-Repository zu. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).

