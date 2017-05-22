---
title: Verwenden von Ruby in Azure App Service-Web-Apps unter Linux | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ruby in Azure App Service-Web-Apps unter Linux verwenden.
keywords: "Azure App Service, Web-App, häufig gestellte Fragen, Linux, OSS, Ruby"
services: app-service
documentationCenter: 
authors: aelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: aelnably;wesmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 5aeef6f31dacb1b27c605d39a35a81bd0211e06a
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="using-ruby-in-web-app-on-linux"></a>Verwenden von Ruby in Web-Apps unter Linux #

Mit dem aktuellen Update unseres Back-Ends wurde die Unterstützung für Ruby Version 2.3 eingeführt. Durch Festlegen der Konfiguration Ihrer Linux-Web-App können Sie den Anwendungsstapel ändern.

## <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals ##

Im Menü „Neu“ im [Azure-Portal](https://portal.azure.com) können Sie auswählen, dass eine Web-App unter Linux über die Option „Web und mobil“ erstellt wird, wie in der folgenden Abbildung dargestellt:

![Beginnen mit der Erstellung einer Web-App im Azure-Portal][1]

Als Nächstes wird wie hier dargestellt das Blatt **Erstellen** geöffnet:

![Blatt „Erstellen“][2]

1. Geben Sie der Web-App einen Namen.
2. Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe. (Die verfügbaren Regionen sind im [Abschnitt „Einschränkungen“](app-service-linux-intro.md) angegeben.)
3. Wählen Sie einen vorhandenen Azure App Service-Plan aus, oder erstellen Sie einen neuen. (Hinweise zu App Service-Plänen finden Sie im [Abschnitt „Einschränkungen“](app-service-linux-intro.md).)
4. Wählen Sie Ruby zwischen den integrierten Laufzeitstapeln aus.

Nachdem die Ruby-Web-App erstellt wurde, können Sie sie über Git oder FTP bereitstellen.

## <a name="next-steps"></a>Nächste Schritte
* [Was ist Web-Apps unter Linux?](app-service-linux-intro.md)
* [Erstellen von Web-Apps in Web-Apps unter Linux](app-service-linux-how-to-create-web-app.md)
* [Lokale Git-Bereitstellung in Azure App Service](app-service-deploy-local-git.md)
* [Häufig gestellte Fragen zu Azure App Service-Web-Apps unter Linux](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-ruby/New-Linux.png
[2]: ./media/app-service-linux-using-ruby/Ruby-UX.png
