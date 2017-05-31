---
title: Azure Active Directory-Authentifizierungsprotokolle | Microsoft Docs
description: "Eine Übersicht über die von Azure Active Directory (AD) unterstützten Authentifizierungsprotokolle"
documentationcenter: dev-center-name
author: bryanla
services: active-directory
manager: mbaldwin
editor: 
ms.assetid: 7a838ae2-c24c-4304-b6c0-e77fb888e6c0
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: priyamo
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 27c582436bd57adfb5e8edb0f63c0f206db66c89
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="azure-active-directory-authentication-protocols"></a>Azure Active Directory-Authentifizierungsprotokolle
Azure Active Directory (Azure AD) unterstützt mehrere der am häufigsten verwendeten Protokolle zur Authentifizierung und Autorisierung. Die Themen in diesem Abschnitt beschreiben die unterstützten Protokolle und ihre Implementierung in Azure AD. Die Themen umfassen eine Beschreibung der unterstützten Anspruchstypen, eine Einführung in die Verwendung von Verbundmetadaten, eine ausführliche OAuth 2.0.- und SAML 2.0-Protokoll-Referenzdokumentation und einen Abschnitt zur Problembehandlung.

## <a name="authentication-protocols-articles-and-reference"></a>Authentifizierungsprotokolle, Artikel und Referenz
* [Wichtige Informationen zum Signieren vom Schlüssel-Rollover in Azure AD](active-directory-signing-key-rollover.md) – Erfahren Sie mehr über das Signieren der Schlüssel-Rolloverkadenz von Azure AD, die Änderungen zum automatischen Update des Schlüssels und die Erläuterungen zum Aktualisieren der häufigsten Anwendungsszenarien.
* [Unterstützte Token- und Anspruchstypen](active-directory-token-and-claims.md) – Erfahren Sie mehr über die Ansprüche in den Token, die von Azure AD ausgegeben werden.
* [Verbundmetadaten](active-directory-federation-metadata.md) – Erfahren Sie mehr über das Suchen und Interpretieren der von Azure AD generierten Metadatendokumente.
* [OAuth 2.0 in Azure AD](active-directory-protocols-oauth-code.md) – Erfahren Sie mehr über die Implementierung von OAuth 2.0 in Azure AD.
* [OpenID Connect 1.0](active-directory-protocols-openid-connect-code.md) – Erfahren Sie mehr über die Verwendung des Authorisierungsprotokolls OAuth 2.0 für die Authentifizierung.
* [Dienst-zu-Dienst-Aufrufe mit Clientanmeldeinformationen](active-directory-protocols-oauth-service-to-service.md): Erfahren Sie mehr über den Ablauf der Gewährung von OAuth 2.0-Clientanmeldeinformationen für Dienst-zu-Dienst-Aufrufe.
* [Dienst-zu-Dienst-Aufrufe mit dem Im-Auftrag-von-Fluss](active-directory-protocols-oauth-on-behalf-of.md): Erfahren Sie mehr über das Verwenden das Im-Auftrag-von-Flusses von OAuth 2.0 für Dienst-zu-Dienst-Aufrufe.
* [SAML-Protokollreferenz](active-directory-saml-protocol-reference.md) – Erfahren Sie mehr über die Single Sign-On- und Single Sign-Out-SAML-Profile von Azure AD.

## <a name="see-also"></a>Weitere Informationen
[Entwicklerhandbuch zu Azure Active Directory](active-directory-developers-guide.md)

[Verwenden von Azure AD für Autorisierung](../../app-service-web/web-sites-authentication-authorization.md)

[Active Directory-Codebeispiele](active-directory-code-samples.md)

