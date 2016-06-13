<properties
	pageTitle="Active Directory-Authentifizierungsprotokolle | Microsoft Azure"
	description="Dieser Artikel enthält eine Übersicht über die verschiedenen Authentifizierungs- und Autorisierungsprotokolle, die von Azure Active Directory unterstützt werden."
	services="active-directory"
	documentationCenter=".net"
	authors="priyamohanram"
	manager="mbaldwin"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/21/2016"
	ms.author="priyamo"/>

# Active Directory-Authentifizierungsprotokolle

Azure Active Directory (Azure AD) unterstützt mehrere der am häufigsten verwendeten Protokolle zur Authentifizierung und Autorisierung.

In diese Gruppe von Artikeln sehen wir uns die unterstützten Protokolle und deren Implementierung in Azure AD an. Es werden Beispielanforderungen und -antworten verwendet, und da die Integration direkt in die Protokolle erfolgt, sind diese Artikel größtenteils sprachunabhängig.

- [OAuth 2.0 in Azure AD](active-directory-protocols-oauth-code.md): Enthält Informationen zum OAuth2.0-Ablauf für die Autorisierungserteilung und zur Implementierung in Azure AD.
- [Open ID Connect 1.0](active-directory-protocols-openid-connect-code.md): Es wird beschrieben, wie Sie das OpenID Connect-Authentifizierungsprotokoll in Azure AD verwenden.
- [SAML-Protokollreferenz](active-directory-saml-protocol-reference.md): Es wird beschrieben, wie Sie das SAML-Protokoll zur Unterstützung des [einmaligen Anmeldens](active-directory-single-sign-on-protocol-reference.md) und [einmaligen Abmeldens](active-directory-single-sign-out-protocol-reference.md) in Azure AD verwenden.


## Referenz und Problembehandlung

Diese Artikelreihe enthält zusätzliche Informationen, die für die Problembehandlung für Ihre Azure AD-Anwendungen hilfreich sein können und ein besseres Verständnis von Azure AD ermöglichen.

- [Verbundmetadaten](active-directory-federation-metadata.md): Es wird beschrieben, wie Sie von Azure AD generierte Metadatendokumente suchen und interpretieren.
- [Unterstützte Token und Anspruchstypen](active-directory-token-and-claims.md): Erfahren Sie mehr über die unterschiedlichen Ansprüche in den Token, die von Azure AD ausgegeben werden.
- [Rollover von Signaturschlüsseln in Azure AD](active-directory-signing-key-rollover.md): Enthält Informationen zur Rolloverkadenz von Signaturschlüsseln von Azure AD und zur Aktualisierung des Schlüssels für die häufigsten Anwendungsszenarien.
- [Problembehandlung für Authentifizierungsprotokolle](active-directory-error-handling.md): Erfahren Sie, wie Sie die häufigsten Fehler bei der Verwendung von OAuth 2.0 und Azure AD interpretieren und beheben.
- [Bewährte Methoden für OAuth 2.0 in Azure AD](active-directory-oauth-best-practices.md): Enthält Informationen zu den bewährten Methoden für die Verwendung von OAuth 2.0 in Azure AD und zur Vermeidung von häufigen Problemen.

<!---HONumber=AcomDC_0601_2016-->