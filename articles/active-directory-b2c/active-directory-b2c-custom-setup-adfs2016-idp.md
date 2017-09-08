---
title: "Azure Active Directory B2C: Hinzufügen von AD FS als SAML-Identitätsanbieter mithilfe benutzerdefinierter Richtlinien"
description: "Ein „Gewusst wie“-Artikel zum Einrichten von AD FS 2016 mit SAML-Protokoll und benutzerdefinierten Richtlinien"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: joroja
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: yoelh
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: 6cd0d19e5fd90cb9fb6d3fc4c17119476d7b4f62
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---

# <a name="azure-active-directory-b2c-add-adfs-as-a-saml-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Hinzufügen von AD FS als SAML-Identitätsanbieter mithilfe benutzerdefinierter Richtlinien

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel erfahren Sie, wie Sie die Anmeldung für Benutzer von einem AD FS-Konto mittels [benutzerdefinierter Richtlinien](active-directory-b2c-overview-custom.md) aktivieren.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte im Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) aus.

Diese Schritte umfassen:

1.  Erstellen einer AD FS-Vertrauensstellung der vertrauenden Seite.
2.  Hinzufügen des Zertifikats der AD FS-Vertrauensstellung der vertrauenden Seite zu Azure AD B2C.
3.  Hinzufügen des Anspruchsanbieters zu einer Richtlinie.
4.  Registrieren des AD FS-Kontoanspruchsanbieters bei einer User Journey.
5.  Hochladen der Richtlinie in einen Azure AD B2C-Mandanten und deren Test.

## <a name="to-create-a-claims-aware-relying-party-trust"></a>So erstellen Sie eine Ansprüche unterstützenden Vertrauensstellung der vertrauenden Seite

Um AD FS als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine AD FS-Vertrauensstellung der vertrauenden Seite erstellen und mit den entsprechenden Parametern bereitstellen.

Um eine neue Vertrauensstellung der vertrauenden Seite mithilfe des AD FS-Verwaltungs-Snap-Ins hinzuzufügen und die Einstellungen manuell zu konfigurieren, führen Sie das folgende Verfahren auf einem Verbundserver aus.

Mitgliedschaft in **Administratoren** oder Entsprechendem auf dem lokalen Computer ist die Mindestvoraussetzung, um dieses Verfahren abzuschließen. Beachten Sie die näheren Informationen zur Verwendung der geeigneten Konten und Gruppenmitgliedschaften unter [Local and Domain Default Groups](http://go.microsoft.com/fwlink/?LinkId=83477) (Lokale und Domänenstandardgruppen).

1.  Klicken Sie im Server-Manager auf **Tools**, und wählen Sie dann **ADFS-Verwaltung**.

2.  Klicken Sie auf **Vertrauensstellung der vertrauenden Seite hinzufügen**.
    ![Vertrauensstellung der vertrauenden Seite hinzufügen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)

3.  Wählen Sie auf der **Willkommen**-Seite **Ansprüche unterstützend**, und klicken Sie auf **Start**.
    ![Auf der „Willkommen“-Seite „Ansprüche unterstützend“ wählen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)
4.  Klicken Sie auf der Seite **Datenquelle auswählen** auf den Eintrag **Daten über die vertrauende Seite manuell eingeben** und dann auf **Weiter**.
    ![Daten über die vertrauende Seite eingeben](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)

5.  Geben Sie auf der Seite **Anzeigename angeben** einen Namen in **Anzeigename** ein, geben Sie unter **Hinweise** eine Beschreibung für diese Vertrauensstellung der vertrauenden Seite ein, und klicken Sie dann auf **Weiter**.
    ![Anzeigename und Hinweise eingeben](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)
6.  Optional. Falls Sie ein optionales Tokenverschlüsselungszertifikat haben, klicken Sie auf die Seite **Zertifikat konfigurieren**, auf **Durchsuchen**, um Ihre Zertifikatdatei zu suchen, und dann auf **Weiter**.
    ![Zertifikat konfigurieren](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)
7.  Aktivieren Sie auf der Seite **URL konfigurieren** das Kontrollkästchen **Unterstützung für das SAML 2.0 WebSSO-Protokoll aktivieren**. Geben Sie unter **URL des SAML 2.0-Diensts für einmaliges Anmelden der vertrauenden Seite:** die Security Assertion Markup Language-Dienstendpunkt-URL (SAML) für diese Vertrauensstellung der vertrauenden Seite ein, und klicken Sie dann auf **Weiter**.  Fügen Sie für **URL des SAML 2.0-Diensts für einmaliges Anmelden der vertrauenden Seite:** die URL `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/{policy}` ein. Ersetzen Sie „{Tenant}“ durch den Namen Ihres Mandanten (z.B. „contosob2c.onmicrosoft.com“) und „{policy}“ durch den Namen Ihrer Erweiterungsrichtlinie (z.B. B2C_1A_TrustFrameworkExtensions).
    > [!IMPORTANT]
    >Vom Richtliniennamen – in diesem Fall `B2C_1A_TrustFrameworkExtensions` – erbt die signup_or_signin-Richtlinie.
    >Die URL kann zum Beispiel lauten:   „https://login.microsoftonline.com/te/**contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**“.

    ![URL des SAML 2.0-Diensts für einmaliges Anmelden](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-6.png)
8. Geben Sie auf der Seite **Konfigurieren von Bezeichnern** die gleiche URL wie im vorherigen Schritt an, klicken Sie auf **Hinzufügen**, um sie der Liste hinzuzufügen, und dann auf **Weiter**.
    ![Bezeichner der Vertrauensstellung der vertrauenden Seite](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)
9.  Wählen Sie auf der Seite **Zugriffssteuerungsrichtlinie auswählen** eine Richtlinie aus, und klicken Sie auf **Weiter**.
    ![Zugriffssteuerungsrichtlinie auswählen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)
10.  Überprüfen Sie die Einstellungen auf der Seite **Bereit zum Hinzufügen der Vertrauensstellung**, und klicken Sie dann auf **Weiter**, um die Daten Ihrer Vertrauensstellung der vertrauenden Seite zu speichern.
    ![Speichern der Daten Ihrer Vertrauensstellung der vertrauenden Seite](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)
11.  Klicken Sie auf der Seite **Fertig stellen** auf **Schließen**. Diese Aktion zeigt automatisch das Dialogfeld **Anspruchsregeln bearbeiten** an.
    ![Anspruchsregeln bearbeiten](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)
12. Klicken Sie auf **Regel hinzufügen**.  
      ![Neue Regel hinzufügen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)
13.  Wählen Sie in **Anspruchsregelvorlage** die Option **Senden von LDAP-Attributen als Ansprüche** aus.
    ![„Senden von LDAP-Attributen als Ansprüche“ wählen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)
14.  Geben Sie den **Anspruchsregelnamen** an. Wählen Sie als **Attributspeicher** **Active Directory**. Fügen Sie die folgenden Ansprüche hinzu, und klicken Sie auf **Fertig stellen** und **OK**.
    ![Regeleigenschaften festlegen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)
15.  Wählen Sie im Server-Manager **Vertrauensstellung der vertrauenden Seiten** und dann die Vertrauensstellung der vertrauenden Seite, die Sie erstellt haben, und klicken Sie auf **Eigenschaften**.
    ![Eigenschaften der vertrauenden Seite bearbeiten](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)
16.  Klicken Sie im Eigenschaftenfenster der Vertrauensstellung der vertrauenden Seite (B2C-Demo) auf die Registerkarte **Signatur** und auf **Hinzufügen**.  
    ![Signatur festlegen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)
17.  Fügen Sie Ihr Signaturzertifikat (CERT-Datei, ohne privaten Schlüssel) hinzu.  
    ![Ihr Signaturzertifikat hinzufügen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-3.png)
18.  Klicken Sie im Eigenschaftenfenster der Vertrauensstellung der vertrauenden Seite (B2C-Demo) auf die Registerkarte **Erweitert**, und ändern Sie den **Sicheren Hashalgorithmus** in **SHA-1**. Klicken Sie auf **OK**.  
    ![Sicheren Hashalgorithmus auf „SHA-1“ festlegen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)

## <a name="add-the-adfs-account-application-key-to-azure-ad-b2c"></a>Hinzufügen des AD FS-Kontoanwendungsschlüssels zu Azure AD B2C
Verbund mit AD FS-Konten erfordert einen geheimen Clientschlüssel für das AD FS-Konto, um Azure AD B2C im Auftrag der Anwendung zu vertrauen. Sie müssen Ihr AD FS-Zertifikat in Ihrem Azure AD B2C-Mandanten speichern. 

1.  Wechseln Sie zu Ihrem Azure AD B2C-Mandanten, und wählen Sie **B2C-Einstellungen** > **Framework für die Identitätsfunktion**.
2.  Wählen Sie **Policy Keys** (Richtlinienschlüssel), um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen.
3.  Klicken Sie auf **+Hinzufügen**.
4.  Verwenden Sie für **Optionen** die Option **Hochladen**.
5.  Verwenden Sie für **Name** den Wert `ADFSSamlCert`.  
    Das Präfix `B2C_1A_` wird möglicherweise automatisch hinzugefügt.
6.  Wählen Sie in „Dateiupload“** Ihre Zertifikat-PFX-Datei mit dem privaten Schlüssel. Hinweis: Dieses Zertifikat (mit dem privaten Schlüssel) sollte das gleiche sein, das für die AD FS-Vertrauensstellung der vertrauenden Seite ausgestellt wurde und verwendet wird.
![Richtlinienschlüssel hochladen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)
7.  Klicken Sie auf **Erstellen**
8.  Vergewissern Sie sich, dass Sie den Schlüssel `B2C_1A_ADFSSamlCert` erstellt haben.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Hinzufügen eines Anspruchsanbieters in Ihrer Erweiterungsrichtlinie
Wenn Sie möchten, dass Benutzer sich mithilfe des AD FS-Kontos anmelden, müssen Sie das AD FS-Konto als Anspruchsanbieter definieren. Das heißt, Sie müssen einen Endpunkt festlegen, mit dem Azure AD B2C kommuniziert. Der Endpunkt bietet eine Reihe von Ansprüchen, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde.

Definieren Sie AD FS als Anspruchsanbieter, indem Sie den `<ClaimsProvider>`-Knoten in Ihrer Erweiterungsrichtliniendatei hinzufügen:

1. Öffnen Sie die Erweiterungsrichtliniendatei (TrustFrameworkExtensions.xml) von Ihrem Arbeitsverzeichnis aus. Wenn Sie einen XML-Editor benötigen, können Sie [Visual Studio Code](https://code.visualstudio.com/download), einen einfachen plattformübergreifenden Editor, ausprobieren.
2. Suchen Sie nach dem `<ClaimsProviders>`-Abschnitt.
3. Fügen Sie den folgenden XML-Codeausschnitt unter dem `ClaimsProviders`-Element hinzu, ersetzen Sie `identityProvider` mit Ihrem DNS (beliebiger Wert, der Ihre Domäne angibt), und speichern Sie die Datei. 

```xml
<ClaimsProvider>
    <Domain>contoso.com</Domain>
    <DisplayName>Contoso ADFS</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Contoso-SAML2">
        <DisplayName>Contoso ADFS</DisplayName>
        <Description>Login with your Contoso account</Description>
        <Protocol Name="SAML2"/>
        <Metadata>
        <Item Key="RequestsSigned">false</Item>
        <Item Key="WantsEncryptedAssertions">false</Item>
        <Item Key="PartnerEntity">https://{your_ADFS_domain}/federationmetadata/2007-06/federationmetadata.xml</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userPrincipalName" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop"/>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-the-adfs-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Registrieren des AD FS-Kontoanspruchsanbieters zum Registrieren oder Anmelden bei einer User Journey
An diesem Punkt wurde der Identitätsanbieter eingerichtet.  Er ist jedoch nicht in den Bildschirmen zur Registrierung/Anmeldung verfügbar. Nun müssen Sie den AD FS-Kontoidentitätsanbieter Ihrer Benutzer-`SignUpOrSignIn`-User Journey hinzufügen. Um ihn verfügbar zu machen, erstellen wir ein Duplikat einer vorhandenen Vorlagen-User Journey.  Anschließend ändern wir es, sodass es den AD FS-Identitätsanbieter einbezieht:
    >[!NOTE]
    >If you previously copied the `<UserJourneys>` element from base file of your policy to the extension file (TrustFrameworkExtensions.xml) you can skip this section.
1.  Öffnen Sie die Basisdatei Ihrer Richtlinie (z.B. „TrustFrameworkBase.xml“).
2.  Suchen Sie das `<UserJourneys>`-Element, und kopieren Sie den gesamten Inhalt des `<UserJourneys>`-Knotens.
3.  Öffnen Sie die Erweiterungsdatei (z.B. „TrustFrameworkExtensions.xml“), und suchen Sie nach dem Element `<UserJourneys>`. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
4.  Fügen Sie den gesamten Inhalt des `<UserJournesy>`-Knotens ein, den Sie als untergeordnetes Element des `<UserJourneys>`-Elements kopiert haben.

### <a name="display-the-button"></a>Anzeigen der Schaltfläche
Das `<ClaimsProviderSelections>`-Element definiert die Liste der Anspruchsanbieter-Auswahloptionen und deren Reihenfolge.  Das `<ClaimsProviderSelection>`-Element entspricht einer Schaltfläche für einen Identitätsanbieter auf einer Registrierungs-/Anmeldeseite. Wenn Sie ein `<ClaimsProviderSelection>`-Element für das AD FS-Konto hinzufügen, wird eine neue Schaltfläche angezeigt, wenn ein Benutzer zu der Seite gelangt. So fügen Sie dieses Element hinzu:

1.  Suchen Sie den `<UserJourney>`-Knoten, der `Id="SignUpOrSignIn"` in der User Journey enthält, die Sie kopiert haben.
2.  Suchen Sie den `<OrchestrationStep>`-Knoten, der `Order="1"` enthält.
3.  Fügen Sie folgenden XML-Codeausschnitt unter dem `<ClaimsProviderSelections>`-Knoten hinzu:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```
### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion

Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. In diesem Fall soll bei der Aktion Azure AD B2C mit dem AD FS-Konto kommunizieren, um ein Token zu empfangen. Verknüpfen Sie die Schaltfläche mit einer Aktion, indem Sie das technische Profil für Ihren AD FS-Kontoanspruchsanbieter verknüpfen:

1.  Suchen Sie `<OrchestrationStep>`, das `Order="2"` im `<UserJourney>`-Knoten enthält.
2.  Fügen Sie folgenden XML-Codeausschnitt unter dem `<ClaimsExchanges>`-Knoten hinzu:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

> [!NOTE]
> * Stellen Sie sicher, dass `Id` den gleichen Wert wie `TargetClaimsExchangeId` im vorherigen Abschnitt hat.
> * Stellen Sie sicher, dass für `TechnicalProfileReferenceId` das technische Profil festgelegt ist, das Sie zuvor erstellt haben (Contoso-SAML2).

## <a name="upload-the-policy-to-your-tenant"></a>Hochladen der Richtlinie in Ihren Mandanten
1.  Wechseln Sie im [Azure-Portal](https://portal.azure.com) in den [Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und öffnen Sie das Blatt **Azure AD B2C**.
2.  Wählen Sie **Framework für die Identitätsfunktion** aus.
3.  Öffnen Sie das Blatt **Alle Richtlinien**.
4.  Wählen Sie **Richtlinie hochladen** aus.
5.  Aktivieren Sie das Kontrollkästchen **Richtlinie überschreiben, sofern vorhanden**.
6.  **Laden** Sie „TrustFrameworkExtensions.xml“ hoch, und stellen Sie sicher, dass kein Fehler bei der Validierung auftritt.

## <a name="test-the-custom-policy-by-using-run-now"></a>Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“
1.  Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.
2.  Öffnen Sie **B2C_1A_signup_signin** – die benutzerdefinierte Richtlinie der vertrauenden Seite (Relying Party, RP), die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus.
3.  Sie sollten sich mit dem AD FS-Konto anmelden können.

## <a name="optional-register-the-adfs-account-claims-provider-to-profile-edit-user-journey"></a>[Optional] Registrieren des AD FS-Kontoanspruchsanbieters bei der Profile-Edit-User Journey
Vielleicht möchten Sie den AD FS-Kontoidentitätsanbieter auch Ihrer Benutzer-`ProfileEdit`-User Journey hinzufügen. Um ihn verfügbar zu machen, wiederholen wir die letzten beiden Schritte:

### <a name="display-the-button"></a>Anzeigen der Schaltfläche
1.  Öffnen Sie die Erweiterungsdatei Ihrer Richtlinie (z.B. TrustFrameworkExtensions.xml).
2.  Suchen Sie den `<UserJourney>`-Knoten, der `Id="ProfileEdit"` in der User Journey enthält, die Sie kopiert haben.
3.  Suchen Sie den `<OrchestrationStep>`-Knoten, der `Order="1"` enthält.
4.  Fügen Sie folgenden XML-Codeausschnitt unter dem `<ClaimsProviderSelections>`-Knoten hinzu:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```

### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion
1.  Suchen Sie `<OrchestrationStep>`, das `Order="2"` im `<UserJourney>`-Knoten enthält.
2.  Fügen Sie folgenden XML-Codeausschnitt unter dem `<ClaimsExchanges>`-Knoten hinzu:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Testen der benutzerdefinierten Profile-Edit-Richtlinie mit „Jetzt ausführen“
1.  Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.
2.  Öffnen Sie **B2C_1A_ProfileEdit** – die benutzerdefinierte Richtlinie der vertrauenden Seite, die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus.
3.  Sie sollten sich mit dem AD FS-Konto anmelden können.

## <a name="download-the-complete-policy-files"></a>Herunterladen der vollständigen Richtliniendateien
Optional: Sie sollten nach Abschluss der exemplarischen Vorgehensweise „Erste Schritte mit benutzerdefinierten Richtlinien“ Ihr Szenario mithilfe Ihrer eigenen Dateien mit benutzerdefinierten Richtlinien erstellen. [Richtlinienbeispieldateien dienen nur zur Referenz](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app)

