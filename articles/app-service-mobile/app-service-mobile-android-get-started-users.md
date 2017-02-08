---
title: "Hinzufügen von Authentifizierung unter Android mit Mobile Apps | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Mobile Apps in Azure App Service verwenden, um die Benutzer Ihrer Android-App über verschiedene Identitätsanbieter, einschließlich Google, Facebook, Twitter und Microsoft, zu authentifizieren."
services: app-service\mobile
documentationcenter: android
author: ysxu
manager: erikre
editor: 
ms.assetid: 1fc8e7c1-6c3c-40f4-9967-9cf5e21fc4e1
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/01/2016
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 3caf6edb1e3a33a451ec8773b1e14fe0c95734a1


---
# <a name="add-authentication-to-your-android-app"></a>Hinzufügen der Authentifizierung zu Ihrer Android-App
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Zusammenfassung
In diesem Tutorial fügen Sie dem Aufgabenlisten-Schnellstartprojekt unter Android mithilfe eines unterstützten Identitätsanbieters eine Authentifizierung hinzu. Dieses Tutorial baut auf dem Tutorial [Erste Schritte mit Mobile Apps] auf, das Sie zuerst abschließen müssen.

## <a name="a-nameregisteraregister-your-app-for-authentication-and-configure-the-app-service"></a><a name="register"></a>Registrieren Ihrer App für die Authentifizierung und Konfigurieren von App Service
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="a-namepermissionsarestrict-permissions-to-authenticated-users"></a><a name="permissions"></a>Einschränken von Berechtigungen für authentifizierte Benutzer
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* Öffnen Sie in Android Studio das Projekt, das Sie im Tutorial [Erste Schritte mit Mobile Apps]erstellt haben. Klicken Sie im Menü **Ausführen** auf **App ausführen**. Vergewissern Sie sich, dass nach dem Start der App ein Ausnahmefehler mit dem Statuscode 401 (nicht autorisiert) ausgelöst wird.
  
     Diese Ausnahme wird ausgelöst, weil die App als nicht authentifizierter Benutzer versucht, auf das Back-End zuzugreifen, aber die *TodoItem*-Tabelle nun eine Authentifizierung verlangt.

Als Nächstes werden Sie die App aktualisieren, um Benutzer zu authentifizieren, bevor diese Ressourcen vom Mobile App-Back-End anfordern.

## <a name="add-authentication-to-the-app"></a>Hinzufügen von Authentifizierung zur App
[!INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="a-namecache-tokensacache-authentication-tokens-on-the-client"></a><a name="cache-tokens"></a>Zwischenspeichern von Authentifizierungstoken auf dem Client
[!INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie dieses einfache Tutorial zur Authentifizierung abgeschlossen haben, können Sie mit den folgenden Tutorials fortfahren:

* [Hinzufügen von Pushbenachrichtigungen zu Ihrer Android-App](app-service-mobile-android-get-started-push.md) Hier erfahren Sie, wie Sie Ihr Mobile App-Back-End für die Verwendung von Azure Notification Hubs zum Senden von Pushbenachrichtigungen konfigurieren.
* [Aktivieren der Offlinesynchronisierung für Ihre Android-App](app-service-mobile-android-get-started-offline-data.md) Erfahren Sie, wie Sie mithilfe eines Mobile App-Back-Ends Ihrer App Offlineunterstützung hinzufügen. Die Offlinesynchronisierung ermöglicht Endbenutzern die Interaktion mit einer mobilen App (also das &mdash;Anzeigen, Hinzufügen oder Ändern von Daten&mdash;), auch wenn keine Netzwerkverbindung besteht.

<!-- Anchors. -->
[Registrieren Ihrer App für Authentifizierung und Konfigurieren von Mobile Services]: #register
[Einschränken von Tabellenberechtigungen für authentifizierte Benutzer]: #permissions
[Hinzufügen von Authentifizierung zur App]: #add-authentication
[Speichern von Authentifizierungstoken auf dem Client]: #cache-tokens
[Aktualisieren abgelaufener Token]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Erste Schritte mit Mobile Apps]: app-service-mobile-android-get-started.md



<!--HONumber=Nov16_HO3-->


