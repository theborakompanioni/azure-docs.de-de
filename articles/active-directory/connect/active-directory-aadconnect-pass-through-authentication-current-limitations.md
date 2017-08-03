---
title: "Azure AD Connect: Passthrough-Authentifizierung – Aktuelle Einschränkungen | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Informationen zu den aktuellen Einschränkungen der Azure AD-Passthrough-Authentifizierung (Azure Active Directory)."
services: active-directory
keywords: "Azure AD Connect-Passthrough-Authentifizierung, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, Single Sign-On, einmaliges Anmelden"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: billmath
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 40d07ce6e480924c7c1af601cc25e4a74280141f
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---

# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory-Passthrough-Authentifizierung: aktuelle Einschränkungen

>[!IMPORTANT]
>Die Azure AD-Passthrough-Authentifizierung befindet sich derzeit in der Vorschauphase. Es handelt sich um ein kostenloses Feature, sodass Sie für dessen Verwendung keine kostenpflichtigen Editionen von Azure AD benötigen. Die Passthrough-Authentifizierung steht nur in der weltweiten Instanz von Azure AD zur Verfügung, nicht für [Microsoft Cloud Deutschland](http://www.microsoft.de/cloud-deutschland) oder [Microsoft Azure Government-Cloud](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Folgende Szenarien werden in der Preview-Version vollständig unterstützt:

- Benutzeranmeldungen bei allen webbrowserbasierten Anwendungen
- Benutzeranmeldungen bei Office 365-Clientanwendungen, die die [moderne Authentifizierung](https://aka.ms/modernauthga) unterstützen
- Azure AD Join für Windows 10-Geräte

## <a name="unsupported-scenarios"></a>Nicht unterstützte Szenarien

Folgende Szenarien werden in der Vorschauversion _nicht_ unterstützt:

- Benutzeranmeldungen bei älteren Office-Clientanwendungen und bei Exchange ActiveSync (also native E-Mail-Anwendungen auf Mobilgeräten). Organisationen wird empfohlen, möglichst zur modernen Authentifizierung zu wechseln. Die moderne Authentifizierung ermöglicht nicht nur die Unterstützung der Passthrough-Authentifizierung, sondern trägt mit Features für [bedingten Zugriff](../active-directory-conditional-access.md) wie etwa Multi-Factor Authentication (MFA) auch zum Schutz Ihrer Identitäten bei.
- Benutzeranmeldungen bei Skype for Business-Clientanwendungen
- Benutzeranmeldungen bei PowerShell v1.0. Es empfiehlt sich, stattdessen PowerShell 2.0 zu verwenden.

>[!IMPORTANT]
>Zur Umgehung nicht unterstützter Szenarien können Sie im Azure AD Connect-Assistenten auf der Seite [Optionale Features](active-directory-aadconnect-get-started-custom.md#optional-features) die Kennworthashsynchronisierung aktivieren. Die Kennworthashsynchronisierung ist _nur eine Ausweichlösung für vorherigen Szenarien_ (also _keine_ allgemeine Ausweichlösung für die Passthrough-Authentifizierung). Wenn Sie diese Szenarien nicht benötigen, deaktivieren Sie die Kennworthashsynchronisierung.

## <a name="next-steps"></a>Nächste Schritte
- [**Schnellstartanleitung:**](active-directory-aadconnect-pass-through-authentication-quick-start.md) Richten Sie die Azure AD-Passthrough-Authentifizierung ein.
- [**Ausführliche technische Betrachtung:**](active-directory-aadconnect-pass-through-authentication-how-it-works.md) Informieren Sie sich über die Funktionsweise dieses Features.
- [**Häufig gestellte Fragen**](active-directory-aadconnect-pass-through-authentication-faq.md) – Antworten auf häufig gestellte Fragen
- [**Problembehandlung**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): Beheben häufig auftretender Probleme mit dieser Funktion
- [**Nahtlose SSO mit Azure AD**](active-directory-aadconnect-sso.md): Informationen zu dieser Ergänzungsfunktion
- [**UserVoice:**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) Verfassen neuer Feature-Anforderungen

