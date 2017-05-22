---
title: 'Azure Active Directory B2C: Grundlegendes zu benutzerdefinierten Richtlinien des Starter Packs | Microsoft-Dokumentation'
description: Thema zu benutzerdefinierten Azure Active Directory B2C-Richtlinien
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: joroja
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 9847bcfcc139a769847678c1cca6a8b9c3a30e93
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---

# <a name="understanding-the-custom-policies-of-the-azure-ad-b2c-custom-policy-starter-pack"></a>Grundlegendes zu den benutzerdefinierten Azure AD B2C-Richtlinien des Starter Packs

In diesem Abschnitt sind alle Kernelemente der Richtlinie „B2C_1A_base“ aufgeführt, die im **Starter Pack** enthalten sind und zum Erstellen Ihrer eigenen Richtlinien per Vererbung durch die *Richtlinie „B2C_1A_base_extensions“* genutzt werden.

Der Schwerpunkt liegt mehr auf den bereits definierten Anspruchstypen, Anspruchstransformationen, Inhaltsdefinitionen, Anspruchsanbietern mit den dazugehörigen Profilen und den wichtigsten User Journeys.

> [!IMPORTANT]
> Microsoft übernimmt in Bezug auf die im Folgenden angegebenen Informationen keinerlei Garantie, weder ausdrücklich noch konkludent. Änderungen können jederzeit vorgenommen werden: vor, während und nach der Phase der allgemeinen Verfügbarkeit.

Sowohl mit Ihren eigenen Richtlinien als auch mit der Richtlinie „B2C_1A_base_extensions“ können diese Definitionen überschrieben werden, und diese übergeordnete Richtlinie kann erweitert werden, indem je nach Bedarf weitere bereitgestellt werden.

Die Kernelemente der *Richtlinie „B2C_1A_base“* sind Anspruchstypen, Anspruchstransformationen und Inhaltsdefinitionen. Auf diese Elemente kann in Ihren eigenen Richtlinien und in der *Richtlinie „B2C_1A_base_extensions“* verwiesen werden.

## <a name="claims-schemas"></a>Anspruchsschemas

Diese Anspruchsschemas sind in drei Abschnitte unterteilt:

1.    Im ersten Abschnitt sind die mindestens erforderlichen Ansprüche aufgeführt, die erforderlich sind, damit die User Journeys richtig funktionieren.
2.    Im zweiten Abschnitt sind die Ansprüche aufgeführt, die für Abfragezeichenfolgen-Parameter und andere spezielle Parameter erforderlich sind und an andere Anspruchsanbieter übergeben werden, z.B. an „login.microsoftonline.com“ zur Authentifizierung. **Ändern Sie diese Ansprüche bitte nicht**.
3.    Im dritten Abschnitt sind alle weiteren optionalen Ansprüche aufgeführt, die vom Benutzer erfasst, im Verzeichnis gespeichert und bei der Anmeldung in Token gesendet werden können. In diesem Abschnitt können neue Anspruchstypen, die vom Benutzer erfasst bzw. per Token gesendet werden sollen, hinzugefügt werden.

> [!IMPORTANT]
> Das Anspruchsschema enthält Einschränkungen zu bestimmten Ansprüchen, z.B. Kennwörter und Benutzernamen. Mit der TF-Richtlinie (Trust Framework, Vertrauensframework) wird Azure AD wie jeder andere Anspruchsanbieter behandelt, und alle Einschränkungen sind in der Premium-Richtlinie modelliert. Eine Richtlinie kann geändert werden, um weitere Einschränkungen hinzuzufügen, oder verwenden Sie einen anderen Anspruchsanbieter zum Speichern von Anmeldeinformationen, der über eigene Einschränkungen verfügt.

Die verfügbaren Anspruchstypen sind unten aufgeführt.

### <a name="claims-that-are-required-for-the-user-journeys"></a>Ansprüche, die für User Journeys erforderlich sind

Die folgenden Ansprüche sind erforderlich, damit User Journeys richtig funktionieren:

| Anspruchstyp | Beschreibung |
|-------------|-------------|
| *UserId* | Benutzername |
| *signInName* | Anmeldename |
| *tenantId* | Mandantenbezeichner (ID) des Benutzerobjekts in Azure AD B2C Premium |
| *objectId* | Objektbezeichner (ID) des Benutzerobjekts in Azure AD B2C Premium |
| *password* | Kennwort |
| *newPassword* | |
| *reenterPassword* | |
| *passwordPolicies* | Kennwortrichtlinien, die von Azure AD B2C Premium zum Bestimmen von Kennwortsicherheit, -ablauf usw. verwendet werden |
| *sub* | |
| *alternativeSecurityId* | |
| *identityProvider* | |
| *displayName* | |
| *strongAuthenticationPhoneNumber* | Telefonnummer des Benutzers |
| *Verified.strongAuthenticationPhoneNumber* | |
| *email* | E-Mail-Adresse, die für die Kontaktaufnahme mit dem Benutzer verwendet werden kann |
| *signInNamesInfo.emailAddress* | E-Mail-Adresse, die vom Benutzer zum Anmelden verwendet werden kann |
| *otherMails* | E-Mail-Adressen, die für die Kontaktaufnahme mit dem Benutzer verwendet werden können |
| *userPrincipalName* | In Azure AD B2C Premium gespeicherter Benutzername |
| *upnUserName* | Benutzername zum Erstellen des Benutzerprinzipalnamens |
| *mailNickName* | In Azure AD B2C Premium gespeicherter E-Mail-Kontoname des Benutzers |
| *newUser* | |
| *executed-SelfAsserted-Input* | Anspruch, mit dem angegeben wird, ob Attribute vom Benutzer erfasst wurden |
| *executed-PhoneFactor-Input* | Anspruch, mit dem angegeben wird, ob eine neue Telefonnummer des Benutzers erfasst wurde |
| *authenticationSource* | Gibt an, ob der Benutzer per sozialem Netzwerk als Identitätsanbieter, „login.microsoftonline.com“ oder lokalem Konto authentifiziert wurde |

### <a name="claims-required-for-query-string-parameters-and-other-special-parameters"></a>Für Abfragezeichenfolgen-Parameter und andere spezielle Parameter erforderliche Ansprüche

Die folgenden Ansprüche sind erforderlich, um spezielle Parameter (z.B. einige Abfragezeichenfolgen-Parameter) an andere Anspruchsanbieter zu übergeben:

| Anspruchstyp | Beschreibung |
|-------------|-------------|
| *nux* | Spezieller Parameter, der zur Authentifizierung des lokalen Kontos an „login.microsoftonline.com“ übergeben wird |
| *nca* | Spezieller Parameter, der zur Authentifizierung des lokalen Kontos an „login.microsoftonline.com“ übergeben wird |
| *prompt* | Spezieller Parameter, der zur Authentifizierung des lokalen Kontos an „login.microsoftonline.com“ übergeben wird |
| *mkt* | Spezieller Parameter, der zur Authentifizierung des lokalen Kontos an „login.microsoftonline.com“ übergeben wird |
| *lc* | Spezieller Parameter, der zur Authentifizierung des lokalen Kontos an „login.microsoftonline.com“ übergeben wird |
| *grant_type* | Spezieller Parameter, der zur Authentifizierung des lokalen Kontos an „login.microsoftonline.com“ übergeben wird |
| *scope* | Spezieller Parameter, der zur Authentifizierung des lokalen Kontos an „login.microsoftonline.com“ übergeben wird |
| *client_id* | Spezieller Parameter, der zur Authentifizierung des lokalen Kontos an „login.microsoftonline.com“ übergeben wird |
| *objectIdFromSession* | Parameter, der vom Standard-Sitzungsverwaltungsanbieter bereitgestellt wird, um anzugeben, dass die Objekt-ID aus einer SSO-Sitzung abgerufen wurde |
| *isActiveMFASession* | Von der MFA-Sitzungsverwaltung bereitgestellter Parameter, um anzugeben, dass der Benutzer über eine aktive MFA-Sitzung verfügt |

### <a name="additional-optional-claims-that-can-be-collected"></a>Zusätzliche Ansprüche (optional), die erfasst werden können

Bei den folgenden Ansprüchen handelt es sich um weitere Ansprüche, die von Benutzern erfasst, im Verzeichnis gespeichert und im Token gesendet werden können. Wie bereits erwähnt, können dieser Liste weitere Ansprüche hinzugefügt werden.

| Anspruchstyp | Beschreibung |
|-------------|-------------|
| *givenName* | Vorname des Benutzers (auch als Rufname bezeichnet) |
| *surname* | Nachname des Benutzers (auch als Familienname bezeichnet) |
| *Extension_picture* | Bild des Benutzers aus sozialem Netzwerk |

## <a name="claim-transformations"></a>Anspruchstransformationen

Unten sind die verfügbaren Anspruchstransformationen aufgeführt.

| Anspruchstransformation | Beschreibung |
|----------------------|-------------|
| *CreateOtherMailsFromEmail* | |
| *CreateRandomUPNUserName* | |
| *CreateUserPrincipalName* | |
| *CreateSubjectClaimFromObjectID* | |
| *CreateSubjectClaimFromAlternativeSecurityId* | |
| *CreateAlternativeSecurityId* | |

## <a name="content-definitions"></a>Inhaltsdefinitionen

In diesem Abschnitt werden die Inhaltsdefinitionen beschrieben, die in der Richtlinie *B2C_1A_base* bereits deklariert sind. Auf diese Inhaltsdefinitionen kann in Ihren eigenen Richtlinien und in der Richtlinie *B2C_1A_base_extensions* verwiesen werden, und sie können je nach Bedarf außer Kraft gesetzt bzw. erweitert werden.

| Anspruchsanbieter | Beschreibung |
|-----------------|-------------|
| *Facebook* | |
| *Anmeldung mit lokalem Konto* | |
| *PhoneFactor* | |
| *Azure Active Directory* | |
| *Selbstbestätigt* | |
| *Lokales Konto* | |
| *Sitzungsverwaltung* | |
| *Vertrauensframework-Richtlinienmodul* | |
| *TechnicalProfiles* | |
| *Tokenaussteller* | |

## <a name="technical-profiles"></a>Technische Profile

In diesem Abschnitt sind die technischen Profile dargestellt, die in der Richtlinie *B2C_1A_base* bereits pro Anspruchsanbieter deklariert sind. Auf diese technischen Profile kann in Ihren eigenen Richtlinien und in der Richtlinie *B2C_1A_base_extensions* weiter verwiesen werden, und sie können je nach Bedarf außer Kraft gesetzt bzw. erweitert werden.

### <a name="technical-profiles-for-facebook"></a>Technische Profile für Facebook

| Technisches Profil | Beschreibung |
|-------------------|-------------|
| *Facebook-OAUTH* | |

### <a name="technical-profiles-for-local-account-signin"></a>Technische Profile für die Anmeldung per lokalem Konto

| Technisches Profil | Beschreibung |
|-------------------|-------------|
| *Login-NonInteractive* | |

### <a name="technical-profiles-for-phone-factor"></a>Technische Profile für PhoneFactor

| Technisches Profil | Beschreibung |
|-------------------|-------------|
| *PhoneFactor-Input* | |
| *PhoneFactor-InputOrVerify* | |
| *PhoneFactor-Verify* | |

### <a name="technical-profiles-for-azure-active-directory"></a>Technische Profile für Azure Active Directory

| Technisches Profil | Beschreibung |
|-------------------|-------------|
| *AAD-Common* | Technisches Profil, das von anderen technischen AAD-xxx-Profilen eingebunden wird |
| *AAD-UserWriteUsingAlternativeSecurityId* | Technisches Profil für Anmeldungen per sozialem Netzwerk |
| *AAD-UserReadUsingAlternativeSecurityId* | Technisches Profil für Anmeldungen per sozialem Netzwerk |
| *AAD-UserReadUsingAlternativeSecurityId-NoError* | Technisches Profil für Anmeldungen per sozialem Netzwerk |
| *AAD-UserWritePasswordUsingLogonEmail* | Technisches Profil für lokale Konten |
| *AAD-UserReadUsingEmailAddress* | Technisches Profil für lokale Konten |
| *AAD-UserWriteProfileUsingObjectId* | Technisches Profil für die Aktualisierung des Benutzerdatensatzes mit objectId |
| *AAD-UserWritePhoneNumberUsingObjectId* | Technisches Profil für die Aktualisierung des Benutzerdatensatzes mit objectId |
| *AAD-UserWritePasswordUsingObjectId* | Technisches Profil für die Aktualisierung des Benutzerdatensatzes mit objectId |
| *AAD-UserReadUsingObjectId* | Technisches Profil zum Lesen von Daten nach Benutzerauthentifizierungen |

### <a name="technical-profiles-for-self-asserted"></a>Technische Profile für „Selbstbestätigt“

| Technisches Profil | Beschreibung |
|-------------------|-------------|
| *SelfAsserted-Social* | |
| *SelfAsserted-ProfileUpdate* | |

### <a name="technical-profiles-for-local-account"></a>Technische Profile für lokales Konto

| Technisches Profil | Beschreibung |
|-------------------|-------------|
| *LocalAccountSignUpWithLogonEmail* | |

### <a name="technical-profiles-for-session-management"></a>Technische Profile für Sitzungsverwaltung

| Technisches Profil | Beschreibung |
|-------------------|-------------|
| *SM-Noop* | |
| *SM-AAD* | |
| *SM-SocialSignup* | Profilname wird verwendet, um für die AAD-Sitzung Eindeutigkeit für die Registrierung und Anmeldung herzustellen |
| *SM-SocialLogin* | |
| *SM-MFA* | |

### <a name="technical-profiles-for-trustframework-policy-engine-technicalprofiles"></a>Technische Profile für TechnicalProfiles des Vertrauensframework-Richtlinienmoduls

Derzeit werden für den Anspruchsanbieter vom Typ **TechnicalProfiles des Vertrauensframework-Richtlinienmoduls** keine technischen Profile definiert.

### <a name="technical-profiles-for-token-issuer"></a>Technische Profile für Tokenaussteller

| Technisches Profil | Beschreibung |
|-------------------|-------------|
| *JwtIssuer* | |

## <a name="user-journeys"></a>User Journeys

In diesem Abschnitt sind die User Journeys aufgeführt, die in der Richtlinie *B2C_1A_base* bereits deklariert sind. Auf diese User Journeys kann in Ihren eigenen Richtlinien und in der Richtlinie *B2C_1A_base_extensions* weiter verwiesen werden, und sie können je nach Bedarf außer Kraft gesetzt bzw. erweitert werden.

| User Journey | Beschreibung |
|--------------|-------------|
| *SignUp* | |
| *SignIn* | |
| *SignUpOrSignIn* | |
| *EditProfile* | |
| *PasswordReset* | |

