---
title: "Azure CLI-Skriptbeispiel – manuelles Skalieren einer Web-App mithilfe von Azure CLI 2.0 | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – manuelles Skalieren einer Web-App mithilfe von Azure CLI 2.0"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 251d9074-8fff-4121-ad16-9eca9556ac96
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 5f7eb21b48e36fa0087b354f8661f941e9780d94
ms.lasthandoff: 03/21/2017

---

# <a name="scale-a-web-app-manually"></a>Manuelles Skalieren einer Web-App

In diesem Szenario erfahren Sie, wie Sie eine Ressourcengruppe, einen App Service-Plan und eine Web-App erstellen. Danach skalieren Sie den App Service-Plan von einer auf mehrere Instanzen.

Installieren Sie bei Bedarf die Azure-Befehlszeilenschnittstelle anhand der Anleitung im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli), und führen Sie dann `az login` aus, um eine Verbindung mit Azure herzustellen.

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../../virtual-machines/virtual-machines-windows-cli-options.md).

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/app-service/scale-manual/scale-manual.sh "Manuelles Skalieren")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine Web-App und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. Dies ist wie eine Serverfarm für die Azure-Web-App. |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#create) | Erstellt eine Azure-Web-App im App Service-Plan. |
| [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update) | Aktualisiert Eigenschaften des App Service-Plans. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../app-service-cli-samples.md).
