---
title: "Azure CLI-Skriptbeispiel – Erstellen einer Web-App und Bereitstellen von Code über ein lokales Git-Repository | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen einer Web-App und Bereitstellen von Code über ein lokales Git-Repository"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 048f98aa-f708-44cb-9b9e-953f67dc6da8
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 09851d3f2b3cf33015e508aec6cdae613a747038
ms.openlocfilehash: b10da867c4ab43053019313956b09635917781e4
ms.lasthandoff: 02/27/2017

---

# <a name="create-a-web-app-and-deploy-code-from-a-local-git-repository"></a>Erstellen einer Web-App und Bereitstellen von Code über ein lokales Git-Repository

Dieses Beispielskript erstellt eine Web-App in App Service mit den zugehörigen Ressourcen und stellt dann Ihren Web-App-Code über ein lokales Git-Repository bereit.

Stellen Sie vor dem Ausführen dieses Skripts sicher, dass über den Befehl `az login` eine Verbindung mit Azure hergestellt wurde und dass Ihr Anwendungscode in ein lokales Git-Repository übernommen wurde.

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../../virtual-machines/virtual-machines-windows-cli-options.md).

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/app-service/deploy-local-git/deploy-local-git.sh?highlight=3-5 "Erstellen einer Web-App und Bereitstellen von Code über ein lokales Git-Repository")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die App Service-App und alle zugehörigen Ressourcen entfernt werden.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#delete) | Erstellt eine Azure-Web-App. |
| [az appservice web deployment user set](https://docs.microsoft.com/cli/azure/appservice/web/deployment/user#set) | Legt die Anmeldeinformationen der Bereitstellung für App Service auf Kontoebene fest. |
| [az appservice web source-control config-local-git](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config-local-git) | Erstellt eine Konfiguration der Quellcodeverwaltung für ein lokales Git-Repository. |
| [az appservice web browse](https://docs.microsoft.com/cli/azure/appservice/web#browse) | Öffnet eine Azure-Web-App in einem Browser. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../app-service-cli-samples.md).
