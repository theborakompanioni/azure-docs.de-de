---
title: "Erweiterungs-App – Azure AD B2C | Microsoft-Dokumentation"
description: Wiederherstellen der b2c-extensions-app
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f0392e32-0771-473c-a799-81438ca2bcff
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: parja
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: 17500b572a0e92c1c233c6967840a5b6d96e21cb
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

---
# <a name="azure-ad-b2c-extensions-app"></a>Azure AD B2C: Erweiterungs-App

Wenn ein Azure AD B2C-Verzeichnis erstellt wird, wird automatisch eine App namens `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` im neuen Verzeichnis erstellt. Diese als **b2c-extensions-app** bezeichnete App wird unter *App-Registrierungen* angezeigt. Sie wird vom Azure AD B2C-Dienst zum Speichern von Informationen zu Benutzern und benutzerdefinierten Attributen verwendet. Wenn die App gelöscht wird, funktioniert Azure AD B2C nicht ordnungsgemäß, und Ihre Produktionsumgebung ist beeinträchtigt.

> [!IMPORTANT]
> Löschen Sie die b2c-extensions-app nur, wenn Sie vorhaben, sofort Ihren Mandanten zu löschen. Wenn die App länger als 30 Tage gelöscht bleibt, gehen die Benutzerinformationen dauerhaft verloren.

## <a name="verifying-that-the-extensions-app-is-present"></a>Überprüfen des Vorhandenseins der Erweiterungs-App

So überprüfen Sie das Vorhandensein der b2c-extensions-app

1. Klicken Sie in Ihrem Azure AD B2C-Mandanten im Navigationsmenü auf der linken Seite auf **Weitere Dienste**.
1. Suchen Sie nach **App-Registrierungen**, und öffnen Sie sie.
1. Suchen Sie nach einer App, die mit **b2c-extensions-app** beginnt.

## <a name="recover-the-extensions-app"></a>Wiederherstellen der Erweiterungs-App

Wenn Sie die b2c-extensions-app versehentlich gelöscht haben, haben Sie 30 Tage Zeit, sie wiederherzustellen. Sie können die App mithilfe der Graph-API wiederherstellen:

1. Rufen Sie [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/) auf.
1. Melden Sie sich bei der Website als globaler Administrator für das Azure AD B2C-Verzeichnis an, für das die gelöschte App wiederhergestellt werden soll.
1. Verwenden Sie eine HTTP-GET-Methode für die URL `https://graph.windows.net/{tenantName}.onmicrosoft.com/deletedApplications` mit dem Wert „1.6“ für „api-version“. Ersetzen Sie `{tenantName}` durch Ihren Mandantennamen. Bei diesem Vorgang werden alle Anwendungen aufgelistet, die in den letzten 30 Tagen gelöscht wurden.
1. Suchen Sie nach der Anwendung, indem Sie in der Liste nach einem Namen beginnend mit „b2c-extension-app“ suchen, und kopieren Sie den zugehörigen Eigenschaftswert `objectid`.
1. Verwenden Sie eine HTTP-POST-Methode für die URL `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`. Ersetzen Sie den Teil `{OBJECTID}` der URL durch die `objectid` aus dem vorherigen Schritt. 

Sie sollten jetzt [die wiederhergestellte App im Azure-Portal sehen](#verifying-that-the-extensions-app-is-present) können.

> [!NOTE]
> Eine Anwendung kann nur wiederhergestellt werden, wenn sie in den letzten 30 Tagen gelöscht wurde. Wenn mehr als 30 Tage vergangen sind, gehen die Daten dauerhaft verloren. Um weitere Unterstützung zu erhalten, erstellen Sie ein Supportticket.
