---
title: Konfigurieren von Azure Functions-App-Einstellungen | Microsoft Docs
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
ms.date: 10/28/2016
ms.author: rachelap
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 836c5b48c6b9a635d4255e3954b7b9c0a2bae482


---
# <a name="how-to-configure-azure-function-app-settings"></a>Konfigurieren von Azure Funktionen-App-Einstellungen
## <a name="settings-overview"></a>Übersicht über die Einstellungen
Sie können Azure Funktionen-App-Einstellungen verwalten, indem Sie auf den Link **Funktionen-App-Einstellungen** unten links im Portal klicken. Azure-Funktionen-App-Einstellungen gelten für alle Funktionen in der App.

1. Wechseln Sie zum [Azure-Portal](http://portal.azure.com) , und melden Sie sich mit Ihrem Azure-Konto an.
2. Klicken Sie unten links im Portal auf **Funktionen-App-Einstellungen**. Dadurch werden mehrere Optionen zur Auswahl angezeigt. 

![Funktionen-App-Einstellungen für Azure](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

## <a name="memory-size"></a>Arbeitsspeichergröße
Sie können konfigurieren, wie viel Arbeitsspeicher für die Funktionen in der aktuellen Funktionen-App zugewiesen werden soll. 

Schieben Sie den Schieberegler auf die gewünschte Speichergröße, um den Speicher zu konfigurieren. Der Höchstwert beträgt 128 MB.

![Konfigurieren der Arbeitsspeichergröße für die Funktionen-App](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-memory-size.png)

## <a name="continuous-integration"></a>Continuous Integration
Sie können die Funktionen-App in GitHub, Visual Studio Team Services und anderen Diensten integrieren.

1. Klicken Sie auf den Link **Continuous Integration konfigurieren**. Daraufhin wird der Bereich **Bereitstellungen** mit Optionen angezeigt.
2. Klicken Sie auf **Setup** im Bereich **Bereitstellungen**, um den Bereich **Bereitstellungsquelle** mit einer Option anzuzeigen: Klicken Sie auf **Quelle auswählen**, um verfügbare Quellen anzuzeigen. 
3. Wählen Sie eine der verfügbaren Bereitstellungsquellen aus, indem Sie darauf klicken: Visual Studio Team Services, OneDrive, lokales Git-Repository, GitHub, Bitbucket, DropBox oder ein externes Repository. 
   
    ![Konfigurieren von CI für App-Funktionen](./media/functions-how-to-use-azure-function-app-settings/configure-function-ci.png)
4. Geben Sie Ihre Anmeldeinformationen und weitere Informationen ein, wenn Sie von den verschiedenen Bereitstellungsquellen dazu aufgefordert werden. Die Anmeldeinformationen und weiteren angeforderten Informationen können sich abhängig von der ausgewählten Quelle geringfügig unterscheiden. 

Sobald Sie CI eingerichtet haben, wird verbundener Code, den Sie per Push an die konfigurierte Quelle senden, automatisch in dieser Funktionen-App bereitgestellt.

## <a name="authenticationauthorization"></a>Authentifizierung/Autorisierung
Für Funktionen, die einen HTTP-Trigger verwenden, können Sie festlegen, dass Aufrufe authentifiziert werden müssen.

1. Klicken Sie zum Konfigurieren der Authentifizierung auf den Link **Authentifizierung konfigurieren**.
2. Stellen Sie die Schaltfläche **App Service-Authentifizierung** auf **Ein**.

![Konfigurieren von CI für App-Funktionen](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)

Die meisten Authentifizierungsanbieter fordern einen API-Schlüssel/Client-ID und einen geheimen Schlüssel an. Mit den Optionen für das Microsoft-Konto und Facebook können Sie aber auch Bereiche definieren (bestimmte Autorisierungsanmeldeinformationen). Active Directory verfügt über mehrere Expresseinstellungen oder erweiterte Konfigurationseinstellungen, die Sie festlegen können.

Weitere Informationen zum Konfigurieren von bestimmten Authentifizierungsanbietern finden Sie unter [Authentifizierung und Autorisierung in Azure App Service](../app-service/app-service-authentication-overview.md).

## <a name="cors"></a>CORS
Normalerweise sind aus Sicherheitsgründen Aufrufe an die Hosts (Domänen) aus externen Quellen, z.B. Ajax-Aufrufe aus einem Browser, nicht zulässig. Andernfalls könnte schädlicher Code an das Back-End gesendet und dort ausgeführt werden. Das sicherste Verfahren besteht daher darin, alle Codequellen mit Ausnahme einiger eigener vertrauenswürdiger Quellen auf die schwarze Liste zu setzen. Sie können in Azure-Funktionen konfigurieren, welche Datenquellen Aufrufe annehmen, indem Sie die Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS) konfigurieren. Mit CORS können Sie Domänen auflisten, die JavaScript-Quellen sind und Funktionen in Ihrer Azure-Funktionen-App aufrufen können. 

1. Um CORS zu konfigurieren, klicken Sie auf den Link **CORS konfigurieren**. 
2. Geben Sie die Domänen ein, die Sie auf die Whitelist setzen möchten.

![Konfigurieren von CORS für App-Funktionen](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

## <a name="api-definition"></a>API-Definition
Ermöglichen Sie den Clients eine einfachere Nutzung Ihrer über HTTP ausgelösten Funktionen.

1. Um eine API einzurichten, klicken Sie auf **API-Metadaten konfigurieren**. 
2. Geben Sie die URL ein, die auf eine Swagger-Json-Datei verweist.

![Konfigurieren der API für App-Funktionen](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)

Weitere Informationen zum Erstellen von API-Definitionen mit Swagger finden Sie unter [Erste Schritte mit API-Apps, ASP.NET und Swagger in Azure App Service](../app-service-api/app-service-api-dotnet-get-started.md).

## <a name="application-settings"></a>Anwendungseinstellungen
Sie können Umgebungsvariablen, Framework-Versionen, Remotedebugging, App-Einstellungen, Verbindungszeichenfolgen, Standarddokumente usw. verwalten. Diese Einstellungen sind spezifisch für Ihre Funktionen-App. 

Klicken Sie zum Konfigurieren von App-Einstellungen auf den Link **App-Einstellungen konfigurieren**. 

![App-Einstellungen konfigurieren](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-settings.png)

## <a name="in-portal-console"></a>Konsole im Portal
Sie können DOS-ähnliche Befehle mit der Konsole für Azure-Funktionen im Portal ausführen. Häufig verwendete Befehle sind z.B. Erstellen von Verzeichnissen und Dateien, Navigation sowie das Ausführen von Batchdateien und -skripts. 

> [!NOTE]
> Sie können Skripts hochladen, aber Sie müssen zuerst einen FTP-Client unter **Erweiterte Einstellungen** in Azure-Funktionen konfigurieren.
> 
> 

Um die Konsole im Portal zu öffnen, klicken Sie auf **Entwicklerkonsole öffnen**.

![Konfigurieren der Arbeitsspeichergröße für die Funktionen-App](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

> [!NOTE]
> Das Arbeiten in einer Konsole mit ASCII-Grafik macht Eindruck.
> 
> 

## <a name="kudu"></a>Kudu
Mit Kudu können Sie auf erweiterte Verwaltungsfunktionen einer Funktionen-App zugreifen.

Klicken Sie zum Öffnen von Kudu auf **Zu Kudu wechseln**. Mit dieser Aktion wird ein neues Browserfenster mit dem Kudu-Web-Administrator geöffnet.

> [!NOTE]
> Sie können **Kudu** auch starten, indem Sie „scm“ in der URL Ihrer Funktion einfügen, wie im Folgenden gezeigt: ```https://<YourFunctionName>.scm.azurewebsites.net/```
> 
> 

Auf der Kudu-Website können Sie Systeminformationen, App-Einstellungen, Umgebungsvariablen, HTTP-Header, Servervariablen und vieles mehr anzeigen und verwalten.

![Konfigurieren von Kudu](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)

## <a name="advanced-settings"></a>Erweiterte Einstellungen
Verwalten Sie Ihre Funktionen-App genau wie jede andere App Service-Instanz. Mit dieser Option erhalten Sie Zugriff auf die bereits beschriebenen sowie einige weitere Einstellungen.  

Um die erweiterten Einstellungen zu öffnen, klicken Sie auf den Link **Erweiterte Einstellungen**. 

![App Service-Einstellungen konfigurieren](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-settings.png)

Weitere Informationen zum Konfigurieren der einzelnen Einstellungen für App-Dienste finden Sie unter [Konfigurieren von Web-Apps in Azure App Service](../app-service-web/web-sites-configure.md).

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Nov16_HO3-->


