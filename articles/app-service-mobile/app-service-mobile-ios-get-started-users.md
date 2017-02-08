---
title: "Hinzufügen von Authentifizierung auf iOS mit Azure Mobile Apps"
description: "Erfahren Sie, wie Sie Azure Mobile Apps zum Authentifizieren Ihrer iOS-App über eine Vielzahl von Identitätsanbietern nutzen können, darunter AAD, Google, Facebook, Twitter und Microsoft."
services: app-service\mobile
documentationcenter: ios
author: ysxu
manager: yochayk
editor: 
ms.assetid: ef3d3cbe-e7ca-45f9-987f-80c44209dc06
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 4df1f27e5cefe9813fdc79b96d42e5c5d033901c


---
# <a name="add-authentication-to-your-ios-app"></a>Hinzufügen der Authentifizierung zu Ihrer iOS-App
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In diesem Lernprogramm fügen Sie dem [iOS-Schnellstartlernprogramm] mithilfe eines unterstützten Identitätsanbieters eine Authentifizierung hinzu. Dieses Lernprogramm baut auf dem [iOS-Schnellstartlernprogramm] auf, das Sie zuerst abschließen müssen.

## <a name="a-nameregisteraregister-your-app-for-authentication-and-configure-the-app-service"></a><a name="register"></a>Registrieren Ihrer App für die Authentifizierung und Konfigurieren von App Service
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="a-namepermissionsarestrict-permissions-to-authenticated-users"></a><a name="permissions"></a>Einschränken von Berechtigungen für authentifizierte Benutzer
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Klicken Sie in Xcode auf **Ausführen**, um die App zu starten. Es wird eine Ausnahme ausgelöst, da die App als nicht authentifizierter Benutzer auf das Back-End zugreift, aber die *TodoItem*-Tabelle nun eine Authentifizierung verlangt.

## <a name="a-nameadd-authenticationaadd-authentication-to-app"></a><a name="add-authentication"></a>Hinzufügen von Authentifizierung zur App
[!INCLUDE [app-service-mobile-ios-authenticate-app](../../includes/app-service-mobile-ios-authenticate-app.md)]

<!-- URLs. -->

[iOS-Schnellstartlernprogramm]: app-service-mobile-ios-get-started.md

[Azure-Portal]: https://portal.azure.com



<!--HONumber=Nov16_HO3-->


