---
title: Verwenden von .NET Core in Azure App Service-Web-Apps unter Linux | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie .NET Core in Azure App Service-Web-Apps unter Linux verwenden.
keywords: Azure App Service, Web-App, DotNet, Core, Linux, OSS
services: app-service
documentationCenter: 
authors: aelnably
manager: erikre
editor: 
ms.assetid: c02959e6-7220-496a-a417-9b2147638e2e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: aelnably;wesmc
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: 2d979b954912b8f344aa681980514821058ce5da
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017


---

# <a name="using-net-core-in-azure-web-app-on-linux"></a>Verwenden von .NET Core in Azure-Web-Apps unter Linux #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


Mit dem aktuellen Update unseres Back-Ends wurde Unterstützung für .NET Core v.1.0 eingeführt. Durch Festlegen der Konfiguration Ihrer Linux-Web-App können Sie den Anwendungsstapel ändern.


## <a name="using-the-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle ##

Mithilfe der [aktuellen Azure-Befehlszeilenschnittstelle (CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli) können Sie den Befehl **azure webapp config set** zum Ändern des Anwendungsstapels verwenden. Beispiel:

        azure webapp config set --name ContosoAppServicePlan --resource-group ContosoLinuxAzureResourceGroup --netframeworkversion v1.0 --appcommandline aspnetcore.dll

Die Datei **aspnetcore.dll** ist die DLL-Datei Ihrer App. Sie können in Ihrer App einen beliebigen Namen verwenden.

Dadurch wird das .NET Core-Image geladen und Ihre Web-App gestartet. Mit dem Befehl **azure webapp config show** können Sie überprüfen, ob die Einstellungen richtig erfolgt sind. Beispiel:

        azure webapp config show --name ContosoAppServicePlan --resource-group ContosoLinuxAzureResourceGroup

## <a name="next-steps"></a>Nächste Schritte
* [Was ist Azure-Web-Apps unter Linux?](app-service-linux-intro.md)
* [Erstellen von Web-Apps in Azure-Web-Apps unter Linux](./app-service-linux-how-to-create-web-app.md)
* [Plattformübergreifende Azure-Web-App-CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
* [Häufig gestellte Fragen zu Azure App Service-Web-Apps unter Linux](app-service-linux-faq.md)
