---
title: "Azure CLI-Skriptbeispiel – weltweites Skalieren einer Web-App mit einer Hochverfügbarkeitsarchitektur | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – weltweites Skalieren einer Web-App mit einer Hochverfügbarkeitsarchitektur"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: e4033a50-0e05-4505-8ce8-c876204b2acc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 02/21/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: c23bab16ee8bf771f4af44abc94e958f512d5390
ms.openlocfilehash: db94d24e6f4efc0e49e0adbcc7a8de851be87586
ms.lasthandoff: 02/27/2017

---

# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a>Weltweites Skalieren einer Web-App mit einer Hochverfügbarkeitsarchitektur

In diesem Szenario erstellen Sie eine Ressourcengruppe, zwei App-Servicepläne, zwei Web-Apps, ein Traffic Manager-Profil und zwei Traffic Manager-Endpunkte. Nach dem Abschluss der Übung verfügen Sie über eine hoch verfügbare Architektur, die – basierend auf der niedrigsten Netzwerklatenz – globale Verfügbarkeit für Ihre Web-App ermöglicht.

Stellen Sie vor dem Ausführen dieses Skripts sicher, dass über den Befehl `az login` eine Verbindung mit Azure hergestellt wurde.

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../../virtual-machines/virtual-machines-windows-cli-options.md).

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/app-service/scale-geographic/scale-geographic.sh "Geografischer Umfang")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die App Service-App und alle zugehörigen Ressourcen entfernt werden.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine Web-App, ein Traffic Manager-Profil und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. Dies ist wie eine Serverfarm für die Azure-Web-App. |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#create) | Erstellt eine Azure-Web-App im App Service-Plan. |
| [az network traffic-manager profile create](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) | Erstellt ein Azure Traffic Manager-Profil. |
| [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) | Fügt einem Azure Traffic Manager-Profil einen Endpunkt hinzu. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../app-service-cli-samples.md).
