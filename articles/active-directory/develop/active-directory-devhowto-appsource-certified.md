---
title: "Zertifizieren von AppSource für Azure Active Directory | Microsoft Docs"
description: "Detailinformationen zur Zertifizierung Ihrer AppSource-Anwendung für Azure Active Directory."
services: active-directory
documentationcenter: 
author: skwan
manager: mbaldwin
editor: 
ms.assetid: 21206407-49f8-4c0b-84d1-c25e17cd4183
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/28/2017
ms.author: skwan;bryanla
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: e1299c1f7f8a31f7034fc0736fcd9d66153a9758
ms.openlocfilehash: 3290a375963bc3e625cbdb05b5f9686e8cfb34f6
ms.contentlocale: de-de
ms.lasthandoff: 03/01/2017


---
# <a name="how-to-get-appsource-certified-for-azure-active-directory-ad"></a>Zertifizieren von AppSource für Azure Active Directory (AD)
Um die AppSource-Zertifizierung für Azure AD zu erhalten, muss Ihre Anwendung das mehrinstanzenfähige Anmeldemuster mit Azure AD mithilfe der Protokolle OpenID Connect oder OAuth 2.0 umsetzen.  

Wenn Sie nicht mit der Azure AD-Anmeldung oder der mehrinstanzenfähigen Anwendungsentwicklung vertraut sind:

1. Lesen Sie zunächst die [Webbrowser-zu-Web-App-Szenarien in „Authentifizierungsszenarien für Azure AD“][AAD-Auth-Scenarios-Browser-To-WebApp].  
2. Sehen Sie sich als Nächstes die Azure AD-[Schnellstarthandbücher für Webanwendungen][AAD-QuickStart-Web-Apps] an, die die Umsetzung der Anmeldung veranschaulichen und Begleitcodebeispiele enthalten.

   > [!TIP]
   > Testen Sie die Vorschau unseres neuen [Entwicklerportals](https://identity.microsoft.com/Docs/Web) , mit dem Sie Azure Active Directory innerhalb weniger Minuten betriebsbereit machen.  Im Entwicklerportal werden Sie durch den Vorgang zum Registrieren einer App und die Integration von Azure AD in Ihren Code geleitet.  Nach dem Durchführen dieser Schritte verfügen Sie über eine einfache Anwendung zur Authentifizierung von Benutzern in Ihrem Mandanten sowie über ein Back-End, das Token akzeptieren und eine Überprüfung durchführen kann.
   >
   >
3. Weitere Informationen zum Implementieren des mehrinstanzenfähigen Anmeldemusters mit Azure AD finden Sie unter [Anmelden eines Azure Active Directory-Benutzers (AD) mit dem Muster einer mehrinstanzenfähigen Anwendung][AAD-Howto-Multitenant-Overview].

## <a name="related-content"></a>Verwandte Inhalte
Weitere Informationen zum Erstellen von Anwendungen, die die Azure AD-Anmeldung unterstützen, sowie Hilfe- und Supportangebote finden Sie in dem [Entwicklerhandbuch zu Azure AD][AAD-Dev-Guide].

Verwenden Sie den Disqus-Kommentarabschnitt nach diesem Artikel, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.

<!--Reference style links -->
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]: ./active-directory-authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Howto-Multitenant-Overview]: ./active-directory-devhowto-multi-tenant-overview.md
[AAD-QuickStart-Web-Apps]: ./active-directory-developers-guide.md#get-started


<!--Image references-->

