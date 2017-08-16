---
title: "Azure Active Directory B2C: Token, Sitzung und einmaliges Anmelden – Konfiguration | Microsoft Docs"
description: "Token, Sitzung und einmaliges Anmelden – Konfiguration in Azure Active Directory B2C "
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: e78e6344-0089-49bf-8c7b-5f634326f58c
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: swkrish
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: 4442174a857681adff33001e660809ec7d47ad7d
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory B2C: Token, Sitzung und einmaliges Anmelden – Konfiguration
Diese Funktion ermöglicht Ihnen eine präzisere Steuerung der folgenden Einstellungen auf [Basis einer Richtlinie](active-directory-b2c-reference-policies.md):

1. Lebensdauer von Sicherheitstoken, die von Azure Active Directory (Azure AD) B2C ausgegeben werden.
2. Lebensdauer von Webanwendungssitzungen, die von Azure AD B2C verwaltet werden.
3. Formate wichtiger Ansprüche in den von Azure AD B2C ausgegebenen Sicherheitstoken
4. App- und richtlinienübergreifendes SSO-Verhalten in Ihrem B2C-Mandanten.

Sie können diese Funktion in Ihrem B2C-Mandanten wie folgt verwenden:

1. Führen Sie die folgenden Schritte aus, um im Azure-Portal [zum Blatt „B2C-Funktionen“ zu navigieren](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klicken Sie auf **Anmelderichtlinien**. *Hinweis: Sie können diese Funktion auf jeden Richtlinientyp anwenden, nicht nur auf **Anmelderichtlinien***.
3. Öffnen Sie eine Richtlinie, indem Sie darauf klicken. Klicken Sie z.B. auf **B2C_1_SiIn**.
4. Klicken Sie oben auf dem Blatt auf **Bearbeiten**.
5. Klicken Sie auf **Token, Sitzung und einmaliges Anmelden**.
6. Nehmen Sie die gewünschten Änderungen vor. Informieren Sie sich in den folgenden Abschnitten zu verfügbaren Eigenschaften.
7. Klicken Sie auf **OK**.
8. Klicken Sie oben auf dem Blatt auf **Speichern**.

## <a name="token-lifetimes-configuration"></a>Konfiguration der Tokengültigkeitsdauer
Azure AD B2C unterstützt das [OAuth 2.0-Autorisierungsprotokoll](active-directory-b2c-reference-protocols.md) zum Aktivieren des sicheren Zugriffs auf geschützte Ressourcen. Um diese Unterstützung zu implementieren, gibt Azure AD B2C verschiedene [Sicherheitstoken](active-directory-b2c-reference-tokens.md) aus. Dies sind die Eigenschaften, die Sie zum Verwalten der Gültigkeitsdauer von Sicherheitstoken, die von Azure AD B2C ausgegeben werden, verwenden können:

* **Lebensdauer von Zugriffs- und ID-Token (Minuten)**: Die Gültigkeitsdauer des OAuth 2.0-Bearertokens, das verwendet wird, um auf eine geschützte Ressource zuzugreifen. Azure AD B2C gibt zurzeit nur ID-Token heraus. Dieser Wert würde auch für Zugriffstoken gelten, wenn wir Unterstützung für sie hinzufügen.
  * Standardwert: 60 Minuten.
  * Minimum (inklusive): 5 Minuten.
  * Maximum (inklusive): 1.440 Minuten.
* **Lebensdauer des Aktualisierungstokens (Tage)**: Der maximale Zeitraum, vor dessen Verstreichen ein Aktualisierungstoken verwendet werden kann, um einen neuen Zugriff oder ein neues ID-Token zu erwerben (und optional ein neues Aktualisierungstoken, wenn Ihrer Anwendung der `offline_access`-Bereich gewährt wurde).
  * Standardwert: 14 Tage.
  * Minimum (inklusive): 1 Tag.
  * Minimum (inklusive): 90 Tage.
* **Lebensdauer für gleitendes Fenster des Aktualisierungstokens (Tage)**: Nach Ablauf dieses Zeitraums muss der Benutzer sich erneut authentifizieren, unabhängig von der Gültigkeitsdauer des zuletzt von der Anwendung abgerufenen Aktualisierungstokens. Es kann nur bereitgestellt werden, wenn die Option auf **Begrenzt** festgelegt ist. Sein Wert muss mindestens dem Wert **Lebensdauer des Aktualisierungstokens (Tage)** entsprechen. Wenn die Option auf **Unbegrenzt** festgelegt ist, können Sie keinen bestimmten Wert angeben.
  * Standardwert: 90 Tage.
  * Minimum (inklusive): 1 Tag.
  * Maximum (inklusive): 365 Tage.

Dies sind einige Anwendungsfälle, die Sie mit diesen Eigenschaften aktivieren können:

* Ermöglichen Sie einem Benutzer, unbegrenzt in einer mobilen Anwendung angemeldet zu bleiben, solange er ständig in der Anwendung aktiv ist. Hierzu können Sie die Option **Lebensdauer für gleitendes Fenster des Aktualisierungstokens (Tage)** in Ihrer Richtlinie auf **Unbegrenzt** festlegen.
* Erfüllen Sie die Sicherheits- und Complianceanforderungen Ihrer Branche durch Festlegung der entsprechenden Zugriffstoken-Gültigkeitsdauer.

    > [!NOTE]
    > Diese Einstellungen sind für Richtlinien zur Kennwortzurücksetzung nicht verfügbar.
    > 
    > 

## <a name="token-compatibility-settings"></a>Tokenkompatibilitätseinstellungen
Das Format wichtiger Ansprüche in den von Azure AD B2C ausgegebenen Sicherheitstoken wurde geändert. Diese Änderung wurde zur Verbesserung der Standardprotokollunterstützung und zur besseren Interoperabilität mit Identitätsbibliotheken von Drittanbietern vorgenommen. Um die fehlerfreie Nutzung vorhandener Apps zu gewährleisten, wurden die folgenden Eigenschaften erstellt, um Kunden die Anwendung nach Bedarf zu ermöglichen:

* **Issuer (iss) claim** (Ausstelleranspruch): Der Azure AD B2C-Mandant, der das Token ausgestellt hat
  * `https://login.microsoftonline.com/{B2C tenant GUID}/v2.0/`: Dies ist der Standardwert.
  * `https://login.microsoftonline.com/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/`: Dieser Wert enthält IDs für den B2C-Mandanten und die in der Tokenanforderung verwendete Richtlinie. Verwenden Sie diesen Wert, wenn für Ihre App oder Bibliothek Azure AD B2C die [OpenID Connect Discovery 1.0-Spezifikationen](http://openid.net/specs/openid-connect-discovery-1_0.html) erfüllen muss.
* **Subject (sub) claim** (Antragstelleranspruch): Die Entität, d.h. der Benutzer, für den das Token Informationen bestätigt
  * **ObjectID**: Dies ist der Standardwert. Er wird als Objekt-ID des Benutzers im Verzeichnis in den `sub`-Anspruch des Tokens eingefügt.
  * **Not supported** (Nicht unterstützt): Nur für Abwärtskompatibilität. Es wird empfohlen, baldmöglichst auf **ObjectID** umzustellen.
* **Claim representing policy ID** (Anspruch, der für die Richtlinien-ID steht): Der Anspruchstyp, in den die in der Tokenanforderung verwendete Richtlinien-ID eingefügt wird
  * **tfp**: Dies ist der Standardwert.
  * **acr**: Nur für Abwärtskompatibilität. Es wird empfohlen, baldmöglichst auf `tfp` umzustellen.

## <a name="session-behavior"></a>Sitzungsverhalten
Azure AD B2C unterstützt das [OpenID Connect-Authentifizierungsprotokoll](active-directory-b2c-reference-oidc.md) zum Aktivieren der sicheren Anmeldung bei Webanwendungen. Dies sind die Eigenschaften, die Sie verwenden können, um Webanwendungssitzungen zu verwalten:

* **Lebensdauer der Web-App-Sitzung (Minuten)**: Die Gültigkeitsdauer von Azure AD B2C-Sitzungscookies, die nach erfolgreicher Authentifizierung des Benutzers im Browser gespeichert werden.
  * Standardwert: 1.440 Minuten.
  * Minimum (inklusive): 15 Minuten.
  * Maximum (inklusive): 1.440 Minuten.
* **Timeout für Web-App-Sitzung**: Wenn diese Option auf **Absolut** festgelegt ist, muss der Benutzer sich nach dem in **Lebensdauer der Web-App-Sitzung (Minuten)** angegebenen Zeitraum erneut authentifizieren. Wenn diese Option auf **Rollen** festgelegt ist (Standardeinstellung), bleibt der Benutzer angemeldet, solange er ständig in Ihrer Webanwendung aktiv ist.

Dies sind einige Anwendungsfälle, die Sie mit diesen Eigenschaften aktivieren können:

* Erfüllen Sie die Sicherheits- und Complianceanforderungen Ihrer Branche durch Festlegung der entsprechenden Lebensdauer für Webanwendungssitzungen.
* Erzwingen Sie die erneute Authentifizierung nach einem festgelegten Zeitraum während der Interaktion eines Benutzers mit einer Komponente Ihrer Webanwendung, die eine höhere Sicherheit erfordert. 

    > [!NOTE]
    > Diese Einstellungen sind für Richtlinien zur Kennwortzurücksetzung nicht verfügbar.
    > 
    > 

## <a name="single-sign-on-sso-configuration"></a>Einmaliges Anmelden (Single Sign-on, SSO) – Konfiguration
Wenn Sie in Ihrem B2C-Mandanten über mehrere Anwendungen und Richtlinien verfügen, können Sie die Interaktionen der Benutzer mithilfe der Eigenschaft **Konfiguration des einmaligen Anmeldens** übergreifend verwalten. Sie können die Eigenschaft auf eine der folgenden Optionen festlegen:

* **Mandant**: Dies ist die Standardeinstellung. Mit dieser Einstellung können mehrere Anwendungen und Richtlinien in Ihrem B2C-Mandanten die gleiche Benutzersitzung gemeinsam nutzen. Sobald sich ein Benutzer bei einer Anwendung – beispielsweise Contoso Shopping – angemeldet hat, erfolgt die Anmeldung bei einer anderen Anwendung – etwa Contoso Pharmacy – nahtlos beim Zugriff auf die Anwendung.
* **Anwendung**: Damit können Sie eine Benutzersitzung ausschließlich für eine Anwendung beibehalten, unabhängig von anderen Anwendungen. Verwenden Sie diese Einstellung beispielsweise, wenn Sie möchten, dass sich der Benutzer (mit denselben Anmeldeinformationen) bei Contoso Pharmacy anmeldet – auch wenn der Benutzer bereits bei Contoso Shopping angemeldet ist, einer anderen Anwendung im selben B2C-Mandanten. 
* **Richtlinie**: Damit können Sie eine Benutzersitzung ausschließlich für eine Richtlinie beibehalten, unabhängig von den Anwendungen, die sie verwenden. Wenn der Benutzer sich z.B. bereits angemeldet und einen Multi-Factor Authentication-Schritt (MFA) abgeschlossen hat, kann er in mehreren Anwendungen Zugriff auf Komponenten mit höherer Sicherheit erhalten, solange die an die Richtlinie gebundene Sitzung nicht abläuft.
* **Deaktiviert**: Bei jeder Ausführung der Richtlinie muss der Benutzer die gesamte User Journey ausführen. Dies erlaubt beispielsweise mehreren Benutzern, sich bei der Anwendung anzumelden (in einem Szenario mit freigegebenem Desktop), auch während ein einzelner Benutzer während der gesamten Zeit angemeldet bleibt.

    > [!NOTE]
    > Diese Einstellungen sind für Richtlinien zur Kennwortzurücksetzung nicht verfügbar.
    > 
    > 


