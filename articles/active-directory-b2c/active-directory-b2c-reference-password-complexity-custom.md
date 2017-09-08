---
title: "Kennwortkomplexität in benutzerdefinierten Richtlinien – Azure AD B2C | Microsoft-Dokumentation"
description: "Gewusst wie: Konfigurieren von Komplexitätsanforderungen für Kennwörter in der benutzerdefinierten Richtlinie"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 53ef86c4-1586-45dc-9952-dbbd62f68afc
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeeda
ms.translationtype: HT
ms.sourcegitcommit: 48dfc0fa4c9ad28c4c64c96ae2fc8a16cd63865c
ms.openlocfilehash: 5be521a08ab70be891ddd5fcb374adc52d5c0705
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="configure-password-complexity-in-custom-policies"></a>Konfigurieren der Kennwortkomplexität in benutzerdefinierten Richtlinien

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dieser Artikel ist eine erweiterte Beschreibung, wie Kennwortkomplexität funktioniert und mithilfe benutzerdefinierter Azure AD B2C-Richtlinien aktiviert wird.

## <a name="azure-ad-b2c-configure-complexity-requirements-for-passwords"></a>Azure AD B2C: Konfigurieren von Komplexitätsanforderungen für Kennwörter

Azure Active Directory B2C (Azure AD B2C) unterstützt das Ändern der Komplexitätsanforderungen für Kennwörter, die von einem Endbenutzer beim Erstellen eines Kontos bereitgestellt werden.  Standardmäßig verwendet Azure AD B2C **starke** Kennwörter.  Azure AD B2C unterstützt außerdem Konfigurationsoptionen zur Steuerung der Komplexität von Kennwörtern, die Kunden verwenden können.  In diesem Artikel erfahren Sie, wie Sie Kennwortkomplexität in benutzerdefinierten Richtlinien konfigurieren.  Sie können auch [Azure AD B2C: Configure complexity requirements for passwords](active-directory-b2c-reference-password-complexity.md) (Azure AD B2C: Konfigurieren von Komplexitätsanforderungen für Kennwörter) lesen.

## <a name="prerequisites"></a>Voraussetzungen

Ein Azure AD B2C-Mandant ist zur Durchführung der Registrierung/Anmeldung für ein lokales Konto konfiguriert, wie unter [Erste Schritte](active-directory-b2c-get-started-custom.md) beschrieben.

## <a name="how-to-configure-password-complexity-in-custom-policy"></a>Gewusst wie: Konfigurieren der Kennwortkomplexität in benutzerdefinierten Richtlinien

Um die Kennwortkomplexität in einer benutzerdefinierten Richtlinie zu konfigurieren, muss die allgemeine Struktur Ihrer benutzerdefinierten Richtlinie ein `ClaimsSchema`-, `Predicates`- und `InputValidations`-Element in enthalten `BuildingBlocks`.

```XML
  <BuildingBlocks>
    <ClaimsSchema>...</ClaimsSchema>
    <Predicates>...</Predicates>
    <InputValidations>...</InputValidations>
  </BuildingBlocks>
```

Der Zweck dieser Elemente lautet folgendermaßen:

- Jedes `Predicate`-Element definiert eine einfache Zeichenfolgenüberprüfung, die „true“ oder „false“ zurückgibt.
- Das `InputValidations`-Element enthält ein oder mehrere `InputValidation`-Elemente.  Jede `InputValidation` wird mit einer Reihe von `Predicate`-Elementen erstellt. Mit diesem Element können Sie boolesche Aggregationen durchführen (ähnlich wie `and` und `or`).
- Das `ClaimsSchema` definiert, welcher Anspruch überprüft wird.  Es definiert dann, welche `InputValidation`-Regel zum Überprüfen des Anspruchs verwendet wird.

### <a name="defining-a-predicate-element"></a>Definieren eines Prädikatelements

Prädikate haben zwei Methodentypen: IsLengthRange oder MatchesRegex. Wir betrachten jeweils ein Beispiel.  Zunächst haben wir ein Beispiel für die Methode MatchesRegex, die die Übereinstimmung mit einem regulären Ausdruck überprüft.  In diesem Beispiel wird eine Übereinstimmung mit einer Zeichenfolge überprüft, die Zahlen enthält.

```XML
      <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be a pin.">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
```

Als Nächstes betrachten wir ein Beispiel für IsLengthRange.  Diese Methode verwendet eine minimale und maximale Zeichenfolgenlänge.

```XML
      <Predicate Id="Length" Method="IsLengthRange" HelpText="The password must be between 8 and 16 characters.">
        <Parameters>
          <Parameter Id="Minimum">8</Parameter>
          <Parameter Id="Maximum">16</Parameter>
        </Parameters>
      </Predicate>
```

Stellen Sie mit dem `HelpText`-Attribut eine Fehlermeldung für Endbenutzer bereit, falls die Überprüfung negativ ausfällt.  Diese Zeichenfolge kann mithilfe der [Sprachanpassungsfunktion](active-directory-b2c-reference-language-customization.md) lokalisiert werden.

### <a name="defining-an-inputvalidation-element"></a>Definieren eines InputValidation-Elements

Eine `InputValidation` ist eine Aggregation von `PredicateReferences`. Alle `PredicateReferences` müssen „true“ sein, damit die `InputValidation` erfolgreich ist.  Verwenden Sie jedoch innerhalb des `PredicateReferences`-Elements ein Attribut namens `MatchAtLeast`, um festzulegen wie viele `PredicateReference`-Überprüfungen „true“ zurückgeben müssen.  Definieren Sie optional ein `HelpText`-Attribut, um die Fehlermeldung zu überschreiben, die in den `Predicate`-Elementen definiert ist, auf die es verweist.

```XML
      <InputValidation Id="PasswordValidation">
        <PredicateReferences Id="LengthGroup" MatchAtLeast="1">
          <PredicateReference Id="Length" />
        </PredicateReferences>
        <PredicateReferences Id="3of4" MatchAtLeast="3" HelpText="You must have at least 3 of the following character classes:">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </InputValidation>
```

### <a name="defining-a-claimsschema-element"></a>Definieren eines ClaimsSchema-Elements

Die Anspruchstypen `newPassword` und `reenterPassword` gelten als speziell, darum ändern Sie ihre Namen nicht.  Die Benutzeroberfläche überprüft basierend auf diesen `ClaimType`-Elementen, ob der Benutzer das Kennwort während der Kontoerstellung richtig neu eingegeben hat.  Um die gleichen `ClaimType`-Elemente zu finden, suchen Sie in Ihrem Starter Pack in der „TrustFrameworkBase.xml“.  Neu in diesem Beispiel ist, dass wir diese Elemente zum Definieren einer `InputValidationReference` überschreiben. Das `ID`-Attribut dieses neuen Elements verweist auf das `InputValidation`-Element, das wir definiert haben.

```XML
    <ClaimsSchema>
      <ClaimType Id="newPassword">
        <Restriction>
          <Pattern RegularExpression="^.*$" HelpText="" />
        </Restriction>
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
      <ClaimType Id="reenterPassword">
        <Restriction>
          <Pattern RegularExpression="^.*$" HelpText="" />
        </Restriction>
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
    </ClaimsSchema>
```

### <a name="putting-it-all-together"></a>Zusammenfügen des Gesamtbilds

Dieses Beispiel zeigt, wie die einzelnen Teile miteinander eine funktionierende Richtlinie bilden.  Zum Verwenden des Beispiels:

1. Führen Sie die Anweisungen unter [Erste Schritte](active-directory-b2c-get-started-custom.md) zum Herunterladen, Konfigurieren und Hochladen von „TrustFrameworkBase.xml“ und „TrustFrameworkExtensions.xml“ aus.
1. Erstellen Sie eine SignUporSignIn.xml-Datei mithilfe des Beispielinhalts in diesem Abschnitt.
1. Aktualisieren Sie „SignUporSignIn.xml“, indem Sie `yourtenant` durch den Namen Ihres Azure AD B2C-Mandanten ersetzen.
1. Laden Sie zuletzt die Richtliniendatei „SignUporSignIn.xml“ hoch.

Dieses Beispiel enthält eine Überprüfung für PIN-Kennwörter und eine für starke Kennwörter:

- Suchen Sie nach `PINpassword`. Dieses `InputValidation`-Element überprüft eine PIN mit beliebiger Länge.  Es wird im Moment nicht verwendet, da im `InputValidationReference`-Element in `ClaimType` nicht darauf verwiesen wird. 
- Suchen Sie nach `PasswordValidation`. Dieses `InputValidation`-Element überprüft, ob ein Kennwort 8 bis 16 Zeichen lang ist und 3 der 4 Zeichentypen Ziffern, Groß- und Kleinbuchstaben oder Symbole enthält.  In `ClaimType` wird darauf verwiesen.  Aus diesem Grund wird diese Regel in dieser Richtlinie erzwungen.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<TrustFrameworkPolicy
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="yourtenant.onmicrosoft.com"
  PolicyId="B2C_1A_signup_signin"
  PublicPolicyUri="http://yourtenant.onmicrosoft.com/B2C_1A_signup_signin">
 <BasePolicy>
    <TenantId>yourtenant.onmicrosoft.com</TenantId>
    <PolicyId>B2C_1A_TrustFrameworkExtensions</PolicyId>
  </BasePolicy>
  <BuildingBlocks>
    <ClaimsSchema>
      <ClaimType Id="newPassword">
        <Restriction>
          <Pattern RegularExpression="^.*$" HelpText="" />
        </Restriction>
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
      <ClaimType Id="reenterPassword">
        <Restriction>
          <Pattern RegularExpression="^.*$" HelpText="" />
        </Restriction>
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
    </ClaimsSchema>
    <Predicates>
      <Predicate Id="Lowercase" Method="MatchesRegex" HelpText="a lowercase">
        <Parameters>
          <Parameter Id="RegularExpression">^[a-z]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Uppercase" Method="MatchesRegex" HelpText="an uppercase">
        <Parameters>
          <Parameter Id="RegularExpression">^[A-Z]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Number" Method="MatchesRegex" HelpText="a number">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Symbol" Method="MatchesRegex" HelpText="a symbol">
        <Parameters>
          <Parameter Id="RegularExpression">^[!@#$%^*()]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Length" Method="IsLengthRange" HelpText="The password must be between 8 and 16 characters.">
        <Parameters>
          <Parameter Id="Minimum">8</Parameter>
          <Parameter Id="Maximum">16</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be a pin.">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
    </Predicates>
    <InputValidations>
      <InputValidation Id="PasswordValidation">
        <PredicateReferences Id="LengthGroup" MatchAtLeast="1">
          <PredicateReference Id="Length" />
        </PredicateReferences>
        <PredicateReferences Id="3of4" MatchAtLeast="3" HelpText="You must have at least 3 of the following character classes:">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </InputValidation>
      <InputValidation Id="PINpassword">
        <PredicateReferences Id="PINGroup">
          <PredicateReference Id="PIN" />
        </PredicateReferences>
      </InputValidation>
    </InputValidations>
  </BuildingBlocks>
  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration, DisableStrongPassword" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      </OutputClaims>
      <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
  </RelyingParty>
</TrustFrameworkPolicy>
```

