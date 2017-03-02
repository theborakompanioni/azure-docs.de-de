---
title: Verwendung von .NET Core in Azure App Service-Web-Apps unter Linux | Microsoft Docs
description: Verwendung von .NET Core in Azure App Service-Web-Apps unter Linux
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
ms.date: 02/16/2017
ms.author: aelnably;wesmc
translationtype: Human Translation
ms.sourcegitcommit: bb4c7ea7adfe1326ae8259782b5de2762c8c2bf5
ms.openlocfilehash: 769de52a85d1d5078b2ba583e94cabd22b0fde65
ms.lasthandoff: 02/17/2017


---

# <a name="using-net-core-in-web-apps-on-linux"></a>Verwendung von .NET Core in Web-Apps unter Linux #

Mit dem aktuellen Update unseres Back-Ends wurde Unterstützung für .NET Core v.1.0 eingeführt. Durch Festlegen der Konfiguration Ihrer Linux-Web-App können Sie den Anwendungsstapel ändern.


## <a name="using-xplat-cli"></a>Verwendung der XPlat-CLI ##

Mithilfe der aktuellen plattformübergreifenden Azure-Befehlszeilenschnittstelle können Sie den Befehl **azure webapp config set** zum Ändern des Anwendungsstapels verwenden. Beispiel:

        azure webapp config set --name ContosoAppServicePlan --resource-group ContosoLinuxAzureResourceGroup --netframeworkversion v1.0 --appcommandline aspnetcore.dll

Die Datei **aspnetcore.dll** ist die DLL-Datei Ihrer App. Sie können in Ihrer App einen beliebigen Namen verwenden.

Dadurch wird das .NET Core-Image geladen und Ihre Web-App gestartet. Mit dem Befehl **azure webapp config show** können Sie überprüfen, ob die Einstellungen richtig erfolgt sind. Beispiel:

        azure webapp config show --name ContosoAppServicePlan --resource-group ContosoLinuxAzureResourceGroup

## <a name="next-steps"></a>Nächste Schritte
* [Was ist App Service unter Linux?](app-service-linux-intro.md)
* [Erstellen von Web-Apps in App Service unter Linux](./app-service-linux-how-to-create-a-web-app.md)
* [Plattformübergreifende Azure-Web-App-CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
* [Häufig gestellte Fragen zu Azure App Service-Web-Apps unter Linux](app-service-linux-faq.md)
