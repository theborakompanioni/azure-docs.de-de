---
title: "Konfigurieren einer neuen mehrinstanzenfähigen Anwendung | Microsoft-Dokumentation"
description: "Konfigurieren des einmaliges Anmeldens für eine mit Azure AD entwickelte und registrierte benutzerdefinierte Anwendung"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 2a6831cbfb747dd134a9cc17ed0f1925d8cc62d4
ms.contentlocale: de-de
ms.lasthandoff: 04/17/2017

---

# <a name="how-to-configure-a-new-multi-tenant-application"></a>Konfigurieren einer neuen mehrinstanzenfähigen Anwendung

Die Aktivierung der einmaligen Verbundanmeldung in Ihrer App wird beim Verbund mit Azure AD für OpenID Connect, SAML 2.0 oder WS-Fed automatisch aktiviert. Wenn sich Ihre Endbenutzer anmelden müssen, anstatt bereits über eine vorhandene Sitzung mit Azure AD zu verfügen, ist es wahrscheinlich, dass Ihre App möglicherweise falsch konfiguriert ist.

* Wenn Sie ADAL/MSAL verwenden, stellen Sie sicher, dass für **PromptBehavior** die Option **Auto** statt **Immer** festgelegt ist.

* Wenn Sie eine mobile App erstellen, sind möglicherweise zusätzliche Konfigurationen erforderlich, um Broker-SSO oder Nicht-Broker-SSO zu aktivieren.

Informationen für Android-Geräte finden Sie unter [Aktivieren von App-übergreifendem SSO in Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android).<br>

Informationen für iOS-Geräte finden Sie unter [Aktivieren von App-übergreifendem SSO in iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios).

## <a name="next-steps"></a>Nächste Schritte

[Azure AD-SSO](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)<br>

[Aktivieren von App-übergreifendem SSO in Android](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-android)<br>

[Aktivieren von App-übergreifendem SSO in iOS](https://docs.microsoft.com/azure/active-directory/develop/active-directory-sso-ios)<br>

[Integrieren von Apps in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)<br>

[Genehmigen und Zuweisen von Berechtigungen für konvergierte Azure AD V2.0-Apps](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)

