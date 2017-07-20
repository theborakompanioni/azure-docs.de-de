---
title: "Azure CLI-Skriptbeispiel – Überwachen einer Web-App mit Webserverprotokollen | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Überwachen einer Web-App mit Webserverprotokollen"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0887656f-611c-4627-8247-b5cded7cef60
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 4f68f90c3aea337d7b61b43e637bcfda3c98f3ea
ms.openlocfilehash: df4ca5b1270ada849e231ad9608a5b1d2edda8be
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017

---

# <a name="monitor-a-web-app-with-web-server-logs"></a>Überwachen einer Web-App mit Webserverprotokollen

In diesem Szenario erstellen Sie eine Ressourcengruppe, einen App Service-Plan und eine Web-App und konfigurieren die Web-App so, dass Webserverprotokolle aktiviert sind. Danach laden Sie die Protokolldateien zur Überprüfung herunter.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/monitor-with-logs/monitor-with-logs.sh "Überwachen von Protokollen")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine Web-App und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. Dies ist wie eine Serverfarm für die Azure-Web-App. |
| [az webapp create](https://docs.microsoft.com/cli/azure/webapp#create) | Erstellt eine Azure-Web-App. |
| [az webapp log config](https://docs.microsoft.com/cli/azure/webapp/log#config) | Konfiguriert, welche Protokolle von einer Azure-Web-App beibehalten werden. |
| [az webapp log download](https://docs.microsoft.com/cli/azure/webapp/log#download) | Lädt die Protokolle einer Azure-Web-App auf Ihren lokalen Computer herunter. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../app-service-cli-samples.md).

