---
title: Verwalten einer Web-App in Azure App Service
description: "Links zu Ressourcen für die Verwaltung einer Web-App in Azure App Service."
services: app-service\web
documentationcenter: 
author: erikre
manager: erikre
editor: 
ms.assetid: d5e2887a-84f9-4747-a573-867635cb8b39
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: rachelap
ms.translationtype: Human Translation
ms.sourcegitcommit: 67ee6932f417194d6d9ee1e18bb716f02cf7605d
ms.openlocfilehash: 08a84ace63465e7c058add69a25687fe46c7ea07
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="manage-a-web-app-in-azure-app-service"></a>Verwalten einer Web-App in Azure App Service
Dieses Thema enthält Links zu Ressourcen für die Verwaltung einer Web-App in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Die Verwaltung umfasst alle Aufgaben, die für eine reibungslose Ausführung Ihrer Web-App sorgen. 

Im Laufe der Lebensdauer einer Web-App führen Sie verschiedene Verwaltungsaufgaben aus, von der ersten Bereitstellung zum Normalbetrieb, einschließlich Wartung und Aktualisierungen.

Viele Aufgaben der Web-App-Verwaltung können im Azure-Portal ausgeführt werden.

## <a name="before-you-deploy-your-web-app-to-production"></a>Bevor Sie Ihre Web-App für die Produktion bereitstellen
### <a name="choose-a-tier"></a>Wählen Sie eine Ebene aus:
Azure App Service wird in fünf Stufen angeboten: Kostenlos, Freigegeben, Basic, Standard und Premium. Weitere Informationen zu Funktionen und der Preisgestaltung für jede Ebene finden Sie unter [Preisdetails](https://azure.microsoft.com/pricing/details/app-service/). 

* [App Service-Plänen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) können Sie mehrere Web-Apps unter der gleichen Ebene zusammenfassen.
* Sie können jederzeit [zwischen Ebenen wechseln](web-sites-scale.md) , nachdem Sie Ihre Web-App erstellt haben.

### <a name="configuration"></a>Konfiguration
Verwenden Sie das [Azure-Portal](https://portal.azure.com/) , um verschiedene Konfigurationsoptionen festzulegen. Weitere Informationen finden Sie unter [Konfigurieren von Web-Apps in Azure App Service](web-sites-configure.md). Hier sehen Sie eine kurze Checkliste:

* Wählen Sie **Laufzeitversionen** für .NET, PHP, Java oder Python aus, falls erforderlich.
* Aktivieren Sie **WebSockets** , wenn Ihre Web-App das WebSocket-Protokoll verwendet. (Dies beinhaltet Apps, die [ASP.NET SignalR](http://www.asp.net/signalr) oder [socket.io](web-sites-nodejs-chat-app-socketio.md) verwenden.)
* Lassen Sie kontinuierliche WebJobs laufen? Falls dies der Fall ist, aktivieren Sie **Immer bereit**.
* Stellen Sie das **Standarddokument**ein, beispielsweise index.html.

Zusätzlich zu diesen grundlegenden Konfigurationseinstellungen können Sie Folgendes festlegen:

* **Secure Socket Layer (SSL)** -Verschlüsselung. Um SSL mit einem benutzerdefinierten Domänennamen zu verwenden, müssen Sie ein SSL-Zertifikat anfordern und Ihre Web-App entsprechend konfigurieren. Weitere Informationen finden Sie unter [Aktivieren von HTTPS für eine Web-App in Azure App Service](app-service-web-tutorial-custom-ssl.md).
* **Benutzerdefinierte Domänennamen.** Zu Ihrer Web-App gibt es automatisch eine Unterdomäne unter azurewebsites.net. Sie können einen benutzerdefinierten Domänennamen zuweisen, beispielsweise contoso.com. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens in Azure App Service](app-service-web-tutorial-custom-domain.md).

Sprachspezifische Konfiguration:

* **PHP**: [Konfigurieren von PHP in Azure App Service-Web-Apps](web-sites-php-configure.md).
* **Python**: [Konfigurieren von Python in Azure App Service-Web-Apps](web-sites-python-configure.md)

## <a name="while-your-web-app-is-running"></a>Während Ihre Web-App ausgeführt wird
Bei der Ausführung Ihrer Web-App müssen Sie sicherstellen, dass sie verfügbar und dem Benutzerverkehr entsprechend skaliert ist. Möglicherweise müssen Sie auch Fehler beheben.

### <a name="monitoring"></a>Überwachung
* Über das Azure-Portal können Sie [Leistungsmetriken hinzufügen](web-sites-monitor.md), wie z.B. CPU-Auslastung und Anzahl der Clientanforderungen.
* [Skalieren Sie Ihre Web-App](web-sites-scale.md) entsprechend dem Verkehr. Abhängig von Ihrer Ebene können Sie die Anzahl von VMs und/oder die Größe der VM-Instanzen skalieren. Auf den Stufen Standard oder Premium können Sie auch automatische Skalierung einrichten, sodass Ihre Website automatisch skaliert wird, entweder nach einem festen Zeitplan oder entsprechend der Last.  

### <a name="backups"></a>Backups
* Legen Sie [automatische Sicherungen](web-sites-backup.md) Ihrer Web-App fest. Erfahren Sie in [diesem Video](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/)mehr über Backups.
* Erfahren Sie mehr über die Optionen für die [Datenbankwiederherstellung](../sql-database/sql-database-business-continuity.md) in der Azure SQL-Datenbank.

### <a name="troubleshooting"></a>Problembehandlung
* Wenn etwas nicht funktioniert, können Sie [Probleme in Visual Studio behandeln](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), unter Verwendung von Diagnoseprotokollen und durch Live-Debuggen in der Cloud. 
* Außerhalb von Visual Studio gibt es verschiedene Möglichkeiten, Diagnoseprotokolle zu sammeln. Weitere Informationen finden Sie unter [Aktivieren der Diagnoseprotokollierung für Web-Apps in Azure App Service](web-sites-enable-diagnostic-log.md).
* Für Node.js-Anwendungen siehe [Debuggen einer Node.js-Web-App in Azure App Service](web-sites-nodejs-debug.md).

### <a name="restoring-data"></a>Datenwiederherstellung
* [Wiederherstellen](web-sites-restore.md) einer Web-App, die zuvor gesichert wurde.

## <a name="when-you-update-your-web-app"></a>Wenn Sie Ihre Web-App aktualisieren
Wenn Sie automatische Sicherungen nicht aktiviert haben, können Sie eine [manuelle Sicherung](web-sites-backup.md)erstellen.

Ziehen Sie eine [gestaffelte Bereitstellung](web-sites-staged-publishing.md)in Betracht. Mit dieser Option können Sie Updates in Form einer gestaffelten Bereitstellung veröffentlichen, die parallel zu Ihrer Produktionsbereitstellung ausgeführt wird. 

Wenn Sie Visual Studio Team Services verwenden, können Sie die fortlaufende Bereitstellung aus der Quellcodeverwaltung festlegen:

* [Verwendung von Team Foundation Version Control (TFVC)](../cloud-services/cloud-services-continuous-delivery-use-vso.md) 
* [Verwendung von Git](../cloud-services/cloud-services-continuous-delivery-use-vso-git.md)

<!-- Anchors. -->

[Before you deploy your site to production]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website



