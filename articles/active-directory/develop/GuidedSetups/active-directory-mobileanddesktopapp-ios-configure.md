---
title: "Azure AD v2 – iOS – Erste Schritte – Konfigurieren | Microsoft-Dokumentation"
description: "Informationen, wie iOS (Swift)-Anwendungen eine API aufrufen können, die Zugriffstoken vom Azure Active Directory v2-Endpunkt anfordert"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.translationtype: HT
ms.sourcegitcommit: b309108b4edaf5d1b198393aa44f55fc6aca231e
ms.openlocfilehash: 8a4f412102c0e75654030f5d4f8aab575fdcebba
ms.contentlocale: de-de

---
## <a name="create-an-application-express"></a>Erstellen einer Anwendung (Express)
Nun müssen Sie Ihre Anwendung im *Microsoft-Anwendungsregistrierungsportal* registrieren:
1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure).
2.  Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein.
3.  Stellen Sie sicher, dass die Option für geführtes Setup aktiviert ist.
4.  Befolgen Sie die Anweisungen zum Abrufen der Anwendungs-ID, und fügen Sie sie in Ihren Code ein.

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Hinzufügen der Registrierungsinformationen Ihrer Anwendung zur Projektmappe (für Fortgeschrittene)

1.  Navigieren Sie zum [Microsoft App-Registrierungsportal](https://apps.dev.microsoft.com/portal/register-app)
2.  Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein.
3.  Stellen Sie sicher, dass die Option für geführtes Setup deaktiviert ist.
4.  Klicken Sie auf `Add Platform`, wählen Sie `Native Application` aus, und klicken Sie auf `Save`
5.  Kehren Sie zu Xcode zurück. Ersetzen Sie in `ViewController.swift` die mit „`let kClientID`“ beginnende Zeile mit der Anwendungs-ID, die Sie gerade registriert haben:

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
Halten Sie STRG gedrückt, und klicken Sie auf <code>Info.plist</code> im Kontextmenü. Klicken Sie dann auf: <code>Open As</code>> <code>Source Code</code>
</li>
<li>
Fügen Sie unter dem <code>dict</code>-Stammknoten Folgendes hinzu:
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Your_Application_Id_Here]</string>
            <string>auth</string>
        </array>
    </dict>
</array>
```
<ol start="8">
<li>
Ersetzen Sie <i><code>[Your_Application_Id_Here]</code></i> durch die Anwendungs-ID, die Sie gerade registriert haben
</li>
</ol>

