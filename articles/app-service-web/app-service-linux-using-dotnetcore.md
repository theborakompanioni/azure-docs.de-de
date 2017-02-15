---
title: Verwendung von .NET Core in Azure App Service-Web-Apps unter Linux | Microsoft Docs
description: Verwendung von .NET Core in Azure App Service-Web-Apps unter Linux
keywords: Azure App Service, Web-App, DotNet, Core, Linux, OSS
services: app-service
documentationCenter: 
authors: aelnably
manager: wpickett
editor: 
ms.assetid: c02959e6-7220-496a-a417-9b2147638e2e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2016
ms.author: aelnably
translationtype: Human Translation
ms.sourcegitcommit: 317d6980d304cc503cc43358c4b91459d4abd1ba
ms.openlocfilehash: a54b7413c2eb6ae67b7424a8966d0cd82cd1ce6a


---

# <a name="using-net-core-in-web-apps-on-linux"></a>Verwendung von .NET Core in Web-Apps unter Linux #

Mit dem aktuellen Update unseres Back-Ends wurde Unterstützung für .NET Core v.1.0 eingeführt. Durch Festlegen der Konfiguration Ihrer Linux-Web-App können Sie den Anwendungsstapel ändern.


## <a name="using-xplat-cli"></a>Verwendung der XPlat-CLI ##

Mithilfe der aktuellen plattformübergreifenden Azure-Befehlszeilenschnittstelle können Sie den Befehl **azure webapp config set** zum Ändern des Anwendungsstapels verwenden. Hier ist ein Beispiel dafür:

        azure webapp config set --name ContosoAppServicePlan --resource-group ContosoLinuxAzureResourceGroup --netframeworkversion v1.0 --appcommandline aspnetcore.dll

Beachten Sie, dass **aspnetcore.dll** die DLL-Datei Ihrer Anwendung ist. Sie können in Ihrer App einen beliebigen Namen verwenden.

Dadurch wird das .Net Core-Image geladen und Ihre Web-App gestartet. Mit dem Befehl **azure webapp config show** können Sie überprüfen, ob die Einstellungen richtig erfolgt sind. Hier ist ein Beispiel dafür:

        azure webapp config show --name ContosoAppServicePlan --resource-group ContosoLinuxAzureResourceGroup

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in App Service unter Linux](./app-service-linux-intro.md) 
* [Was ist App Service unter Linux?](app-service-linux-intro.md)
* [Erstellen von Web-Apps in App Service unter Linux](./app-service-linux-how-to-create-a-web-app.md)
* [Plattformübergreifende Azure-Web-App-CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)



<!--HONumber=Nov16_HO3-->


