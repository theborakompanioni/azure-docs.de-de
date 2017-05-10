---
title: Konfigurieren von Azure-Funktionen-App-Einstellungen | Microsoft Docs
description: Hier erhalten Sie Informationen zum Konfigurieren von Azure Funktionen-App-Einstellungen.
services: 
documentationcenter: .net
author: rachelappel
manager: erikre
editor: 
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 04/23/2017
ms.author: rachelap, glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: e6f5798b260c4923452dcc96c18d4839f5c34bfc
ms.contentlocale: de-de
ms.lasthandoff: 04/25/2017


---
# <a name="how-to-manage-a-function-app-in-the-azure-portal"></a>Verwalten einer Funktionen-App im Azure-Portal 

In Azure Functions wird mit einer Funktionen-App der Ausführungskontext für die einzelnen Funktionen angegeben. Funktionen-App-Verhalten gelten für alle von einer bestimmten Funktionen-App gehosteten Funktionen. In diesem Thema wird beschrieben, wie Sie Ihre Funktionen-Apps im Azure-Portal konfigurieren und verwalten.

Wechseln Sie zunächst zum [Azure-Portal](http://portal.azure.com), und melden Sie sich bei Ihrem Azure-Konto an. Geben Sie auf der Suchleiste oben im Portal den Namen der Funktionen-App ein, und wählen Sie sie in der Liste aus. Nach dem Auswählen der Funktionen-App wird die folgende Seite angezeigt:

![„Übersicht“ für Funktionen-Apps im Azure-Portal](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

## <a name="manage-app-service-settings"></a>Registerkarte „Einstellungen“ für Funktionen-Apps

![Übersicht für Funktionen-Apps im Azure-Portal.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

Auf der Registerkarte **Einstellungen** können Sie die von Ihrer Funktionen-App verwendete Functions-Laufzeitversion aktualisieren. Hier verwalten Sie auch die Hostschlüssel, die zum Beschränken des HTTP-Zugriffs auf alle von der Funktionen-App gehosteten Funktionen verwendet werden.

Functions unterstützt verbrauchsbasierte sowie App Service-Hostingpläne. Weitere Informationen finden Sie unter [Auswählen des richtigen Serviceplans für Azure Functions](functions-scale.md). Zur besseren Vorhersagbarkeit im verbrauchsbasierten Plan können Sie in Functions die Plattformnutzung einschränken, indem Sie ein tägliches Nutzungskontingent in GB-Sekunden festlegen. Bei Erreichen des täglichen Nutzungskontingents wird die Funktionen-App beendet. Eine Funktionen-App, die aufgrund des Erreichens des Tageskontingents beendet wurde, kann in demselben Kontext wie beim Einrichten des täglichen Kontingents wieder aktiviert werden. Ausführliche Informationen zur Abrechnung finden Sie auf der [Preisseite für Azure Functions](http://azure.microsoft.com/pricing/details/functions/).   

## <a name="platform-features-tab"></a>Registerkarte „Plattformfeatures“

![Registerkarte „Plattformfeatures“ für Funktionen-Apps.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

Funktionen-Apps werden auf der Azure App Service-Plattform ausgeführt und verwaltet. So haben die Funktionen-Apps Zugriff auf die meisten Features der Azure-Kernplattform für das Webhosting. Auf der Registerkarte **Plattformfeatures** greifen Sie auf die vielen Features der App Service-Plattform zu, die Sie in Ihren Funktionen-Apps verwenden können. 

> [!NOTE]
> Nicht alle App Service-Features sind verfügbar, wenn eine Funktionen-App nach dem verbrauchsbasierten Hostingplan ausgeführt wird.

Im weiteren Verlauf dieses Themas werden schwerpunktmäßig folgende App Service-Features im Azure-Portal behandelt, die für Functions nützlich sind:

+ [App Service-Editor](#editor)
+ [Anwendungseinstellungen](#settings) 
+ [Console](#console)
+ [Erweiterte Tools (Kudu)](#kudu)
+ [Bereitstellungsoptionen](#deployment)
+ [CORS](#cors)
+ [Authentifizierung](#auth)
+ [API-Definition](#swagger)

Weitere Informationen zum Verwenden von App Service-Einstellungen finden Sie unter [Konfigurieren von Web-Apps in Azure App Service](../app-service-web/web-sites-configure.md).

### <a name="editor"></a>App Service-Editor

| | |
|-|-|
| ![App Service-Editor für Funktionen-Apps.](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | Der App Service-Editor ist ein leistungsstarker Editor im Portal, mit dem Sie JSON-Konfigurationsdateien und Codedateien gleichermaßen bearbeiten können. Bei Auswahl dieser Option wird eine separate Browserregisterkarte mit einem einfachen Editor gestartet. Sie können damit im Git-Repository arbeiten, Code ausführen und debuggen und Einstellungen von Funktionen-Apps ändern. Im Vergleich zum Standardblatt für Funktionen-Apps bietet dieser Editor eine erweiterte Entwicklungsumgebung für Ihre Funktionen.    |

![App Service-Editor](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <a name="settings"></a>Anwendungseinstellungen

| | |
|-|-|
| ![Anwendungseinstellungen für Funktionen-Apps.](./media/functions-how-to-use-azure-function-app-settings/function-app-application-settings.png) | Auf dem App Service-Blatt **Anwendungseinstellungen** können Sie Frameworkversionen, das Remotedebuggen, App-Einstellungen und Verbindungszeichenfolgen konfigurieren und verwalten. Wenn Sie Ihre Funktionen-App in andere Dienste von Azure oder Drittanbietern integrieren, können Sie hier die entsprechenden Einstellungen ändern. |

![Konfigurieren von Anwendungseinstellungen](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-settings.png)

### <a name="console"></a>Konsole

| | |
|-|-|
| ![Konsole für Funktionen-Apps im Azure-Portal](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | Die Konsole im Portal ist das ideale Entwicklungstool, wenn Sie Funktionen-Apps über die Befehlszeile konfigurieren möchten. Häufig verwendete Befehle sind z.B. Erstellen von Verzeichnissen und Dateien, Navigation sowie das Ausführen von Batchdateien und -skripts. |

![Konsole für Funktionen-Apps](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <a name="kudu"></a>Erweiterte Tools (Kudu)

| | |
|-|-|
| ![Kudu für Funktionen-Apps im Azure-Portal](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | Die erweiterten Tools für App Service (auch bekannt als Kudu) ermöglichen den Zugriff auf erweiterte Verwaltungsfunktionen der Funktionen-App. Über Kudu können Sie Systeminformationen, App-Einstellungen, Umgebungsvariablen, Websiteerweiterungen, HTTP-Header und Servervariablen verwalten. Sie können **Kudu** auch starten, indem Sie zum SCM-Endpunkt für die Funktionen-App navigieren, z.B. zu `https://<myfunctionapp>.scm.azurewebsites.net/`. |

![Konfigurieren von Kudu](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><a name="deployment">Bereitstellungsoptionen

| | |
|-|-|
| ![Bereitstellungsoptionen für Funktionen-Apps im Azure-Portal](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | Mit Functions können Sie den Funktionscode auf dem lokalen Computer entwickeln. Anschließend können Sie das lokale Funktionen-App-Projekt in Azure hochladen. Neben dem herkömmlichen FTP-Upload können Sie Ihre Funktionen-App in Functions mit gängigen Continuous Integration-Lösungen wie GitHub, VSTS, Dropbox, Bitbucket usw. bereitstellen. Weitere Informationen finden Sie unter [Continuous Deployment für Azure Functions](functions-continuous-deployment.md). Zum manuellen Hochladen über FTP oder das lokale Git müssen Sie auch [die Anmeldeinformationen für die Bereitstellung konfigurieren](functions-continuous-deployment.md#credentials). |


### <a name="cors"></a>CORS

| | |
|-|-|
| ![CORS für Funktionen-Apps im Azure-Portal](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | Um zu verhindern, dass schädlicher Code in Ihren Diensten ausgeführt wird, werden in App Service Aufrufe für Ihre Funktionen-Apps von externen Quellen blockiert. Functions unterstützt Cross-Origin Resource Sharing (CORS), sodass Sie eine Positivliste der zulässigen Ursprünge definieren können, von denen Remoteanforderungen für die Funktionen akzeptiert werden.  |

![Konfigurieren von CORS für die Funktionen-App](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <a name="auth"></a>Authentifizierung

| | |
|-|-|
| ![Authentifizierung von Funktionen-Apps im Azure-Portal](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | Wenn Funktionen einen HTTP-Trigger verwenden, können Sie festlegen, dass Aufrufe zunächst authentifiziert werden müssen. App Service unterstützt die Azure Active Directory-Authentifizierung und die Anmeldung bei Anbietern von sozialen Netzwerken wie Facebook, Microsoft und Twitter. Weitere Informationen zum Konfigurieren von bestimmten Authentifizierungsanbietern finden Sie unter [Authentifizierung und Autorisierung in Azure App Service](../app-service/app-service-authentication-overview.md). |

![Konfigurieren der Authentifizierung für eine Funktionen-App](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <a name="swagger"></a>API-Definition

| | |
|-|-|
| ![API-Swagger-Definition für Funktionen-Apps im Azure-Portal](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | Functions unterstützt Swagger, um Clients eine einfachere Nutzung Ihrer über HTTP ausgelösten Funktionen zu ermöglichen. Weitere Informationen zum Erstellen von API-Definitionen mit Swagger finden Sie unter [Erste Schritte mit API-Apps, ASP.NET und Swagger in Azure App Service](../app-service-api/app-service-api-dotnet-get-started.md). Sie können zudem Functions-Proxys verwenden, um eine API-Oberfläche für mehrere Funktionen zu definieren. Weitere Informationen finden Sie unter [Arbeiten mit Proxys in Azure Functions](functions-proxies.md). |

![Konfigurieren der API für die Funktionen-App](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a>Nächste Schritte

+ [Konfigurieren von Web-Apps in Azure App Service](../app-service-web/web-sites-configure.md)
+ [Kontinuierliche Bereitstellung für Azure Functions](functions-continuous-deployment.md)




