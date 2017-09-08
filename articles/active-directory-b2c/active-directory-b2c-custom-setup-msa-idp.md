---
title: "Azure Active Directory B2C: Hinzufügen des Microsoft-Kontos (Microsoft Account, MSA) als Identitätsanbieter mithilfe benutzerdefinierter Richtlinien"
description: "Beispiel mit Verwendung von Microsoft als Identitätsanbieter mithilfe des OpenID Connect-Protokolls (OIDC)"
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
ms.openlocfilehash: 928e33392924a7461a4a03ac16ff0994680e68f9
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---

# <a name="azure-active-directory-b2c-add-microsoft-account-msa-as-an-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Hinzufügen des Microsoft-Kontos (Microsoft Account, MSA) als Identitätsanbieter mithilfe benutzerdefinierter Richtlinien

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel erfahren Sie, wie Sie die Anmeldung für Benutzer von einem Microsoft-Konto (MSA) mittels [benutzerdefinierter Richtlinien](active-directory-b2c-overview-custom.md) aktivieren.

## <a name="prerequisites"></a>Voraussetzungen
Führen Sie die Schritte im Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) aus.

Diese Schritte umfassen:

1.  Erstellen einer Microsoft-Kontoanwendung.
2.  Hinzufügen des Microsoft-Kontoanwendungsschlüssels zu Azure AD B2C
3.  Hinzufügen des Anspruchsanbieters zu einer Richtlinie.
4.  Registrieren des Microsoft-Kontoanspruchsanbieters bei einer User Journey.
5.  Hochladen der Richtlinie in einen Azure AD B2C-Mandanten und deren Test.

## <a name="create-a-microsoft-account-application"></a>Erstellen einer Microsoft-Kontoanwendung
Um den Dienst „Microsoft-Konto“ als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Microsoft-Kontoanwendung erstellen und die entsprechenden Parameter bereitstellen. Sie benötigen ein Microsoft-Konto. Wenn Sie über kein Konto verfügen, besuchen Sie [https://www.live.com/](https://www.live.com/).

1.  Navigieren Sie zum [Microsoft-Portal für die Anwendungsregistrierung](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), und melden Sie sich mit den Anmeldeinformationen für Ihr Microsoft-Konto an.
2.  Klicken Sie auf **App hinzufügen**.

    ![Microsoft-Konto – App hinzufügen](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-new-app.png)

3.  Geben Sie einen **Namen** für Ihre Anwendung und eine **Kontakt-E-Mail**-Adresse ein, deaktivieren Sie **Wir unterstützen Sie bei den ersten Schritten.**, und klicken Sie auf **Erstellen**.

    ![Microsoft-Konto – Ihre Anwendung registrieren](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-name.png)

4.  Kopieren Sie den Wert unter **Anwendungs-ID**. Sie benötigen ihn, um das Microsoft-Konto als Identitätsanbieter in Ihrem Mandanten zu konfigurieren.

    ![Microsoft-Konto – Wert von „Anwendungs-ID“ kopieren](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-id.png)

5.  Klicken Sie auf **Plattform hinzufügen**.

    ![Microsoft-Konto – Plattform hinzufügen](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-platform.png)

6.  Wählen Sie aus der Plattformliste **Web**.

    ![Microsoft-Konto – „Web“ aus der Liste der Plattformen wählen](media/active-directory-b2c-custom-setup-ms-account-idp/msa-web.png)

7.  Geben Sie im Feld `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`Umleitungs-URIs **den Wert** ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. „contosob2c.onmicrosoft.com“).

    ![Microsoft-Konto – Umleitungs-URLs festlegen](media/active-directory-b2c-custom-setup-ms-account-idp/msa-redirect-url.png)

8.  Klicken Sie im Abschnitt **Anwendungsgeheimnisse** auf **Neues Kennwort generieren**. Kopieren Sie das neue Kennwort, das auf dem Bildschirm angezeigt wird. Sie benötigen ihn, um das Microsoft-Konto als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. Dieses Kennwort ist eine wichtige Anmeldeinformation.

    ![Microsoft-Konto – Neues Kennwort generieren](media/active-directory-b2c-custom-setup-ms-account-idp/msa-generate-new-password.png)

    ![Microsoft-Konto – neues Kennwort kopieren](media/active-directory-b2c-custom-setup-ms-account-idp/msa-new-password.png)

9.  Aktivieren Sie im Abschnitt **Erweiterte Optionen** das Kontrollkästchen **Live SDK-Unterstützung**. Klicken Sie auf **Speichern**.

    ![Microsoft-Konto – Live SDK-Unterstützung](media/active-directory-b2c-custom-setup-ms-account-idp/msa-live-sdk-support.png)

## <a name="add-the-microsoft-account-application-key-to-azure-ad-b2c"></a>Hinzufügen des Microsoft-Kontoanwendungsschlüssels zu Azure AD B2C
Verbund mit Microsoft-Konten erfordert einen geheimen Clientschlüssel für das Microsoft-Konto, um Azure AD B2C im Auftrag der Anwendung zu vertrauen. Sie müssen Ihr Microsoft-Kontoanwendungsgeheimnis im Azure AD B2C-Mandanten speichern:   

1.  Wechseln Sie zu Ihrem Azure AD B2C-Mandanten, und wählen Sie **B2C-Einstellungen** > **Framework für die Identitätsfunktion**.
2.  Wählen Sie **Policy Keys** (Richtlinienschlüssel), um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen.
3.  Klicken Sie auf **+Hinzufügen**.
4.  Verwenden Sie für **Optionen** die Option **Manuell**.
5.  Verwenden Sie für **Name** den Wert `MSASecret`.  
    Das Präfix `B2C_1A_` wird möglicherweise automatisch hinzugefügt.
6.  Geben Sie in das Feld **Geheimnis** Ihr Microsoft-Anwendungsgeheimnis aus „https://apps.dev.microsoft.com“ ein.
7.  Verwenden Sie für **Schlüsselverwendung**, die Option **Signatur**.
8.  Klicken Sie auf **Erstellen**
9.  Vergewissern Sie sich, dass Sie den Schlüssel `B2C_1A_MSASecret` erstellt haben.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Hinzufügen eines Anspruchsanbieters in Ihrer Erweiterungsrichtlinie
Wenn Sie möchten, dass Benutzer sich mithilfe des Microsoft-Kontos anmelden, müssen Sie das Microsoft-Konto als Anspruchsanbieter definieren. Das heißt, Sie müssen einen Endpunkt festlegen, mit dem Azure AD B2C kommuniziert. Der Endpunkt bietet eine Reihe von Ansprüchen, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde.

Definieren Sie das Microsoft-Konto als Anspruchsanbieter, indem Sie den `<ClaimsProvider>`-Knoten in Ihrer Erweiterungsrichtliniendatei hinzufügen:

1.  Öffnen Sie die Erweiterungsrichtliniendatei (TrustFrameworkExtensions.xml) von Ihrem Arbeitsverzeichnis aus. Wenn Sie einen XML-Editor benötigen, können Sie [Visual Studio Code](https://code.visualstudio.com/download), einen einfachen plattformübergreifenden Editor, ausprobieren.
2.  Suchen Sie nach dem `<ClaimsProviders>`-Abschnitt.
3.  Fügen Sie folgenden XML-Codeausschnitt unter dem `ClaimsProviders`-Element hinzu:

    ```xml
<ClaimsProvider>
    <Domain>live.com</Domain>
    <DisplayName>Microsoft Account</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="MSA-OIDC">
        <DisplayName>Microsoft Account</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <Metadata>
        <Item Key="ProviderName">https://login.live.com</Item>
        <Item Key="METADATA">https://login.live.com/.well-known/openid-configuration</Item>
        <Item Key="response_types">code</Item>
        <Item Key="response_mode">form_post</Item>
        <Item Key="scope">openid profile email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Microsoft application client id</Item>
        </Metadata>
    <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_MSASecret" />
    </CryptographicKeys>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="sub" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="email" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

4.  Ersetzen Sie den `client_id`-Wert mit Ihrer Microsoft-Kontoanwendungsclient-ID.

5.  Speichern Sie die Datei.

## <a name="register-the-microsoft-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Registrieren des Microsoft-Kontoanspruchsanbieters zum Registrieren oder Anmelden bei einer User Journey

An dieser Stelle wurde der Identitätsanbieter eingerichtet, ist jedoch auf keinem der Registrierungs-/Anmeldebildschirme vorhanden. Nun müssen Sie den Microsoft-Kontoidentitätsanbieter Ihrer Benutzer-`SignUpOrSignIn`-User Journey hinzufügen. Um ihn verfügbar zu machen, erstellen wir ein Duplikat einer vorhandenen Vorlagen-User Journey.  Dann fügen wir den Microsoft-Kontoidentitätsanbieter hinzu:

> [!NOTE]
>
>Wenn Sie vorher das `<UserJourneys>`-Element aus der Basisdatei Ihrer Richtlinie in die Erweiterungsdatei `TrustFrameworkExtensions.xml` kopiert haben, können Sie zu diesem Abschnitt springen.

1.  Öffnen Sie die Basisdatei Ihrer Richtlinie (z.B. „TrustFrameworkBase.xml“).
2.  Suchen Sie das `<UserJourneys>`-Element, und kopieren Sie den gesamten Inhalt des `<UserJourneys>`-Knotens.
3.  Öffnen Sie die Erweiterungsdatei (z.B. „TrustFrameworkExtensions.xml“), und suchen Sie nach dem Element `<UserJourneys>`. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
4.  Fügen Sie den gesamten Inhalt des `<UserJournesy>`-Knotens ein, den Sie als untergeordnetes Element des `<UserJourneys>`-Elements kopiert haben.

### <a name="display-the-button"></a>Anzeigen der Schaltfläche
Das `<ClaimsProviderSelections>`-Element definiert die Liste der Anspruchsanbieter-Auswahloptionen und deren Reihenfolge.  Das `<ClaimsProviderSelection>`-Element entspricht einer Schaltfläche für einen Identitätsanbieter auf einer Registrierungs-/Anmeldeseite. Wenn Sie ein `<ClaimsProviderSelection>`-Element für das Microsoft-Konto hinzufügen, wird eine neue Schaltfläche angezeigt, wenn ein Benutzer zu der Seite gelangt. So fügen Sie dieses Element hinzu:

1.  Suchen Sie den `<UserJourney>`-Knoten, der `Id="SignUpOrSignIn"` in der User Journey enthält, die Sie kopiert haben.
2.  Suchen Sie den `<OrchestrationStep>`-Knoten, der `Order="1"` enthält.
3.  Fügen Sie folgenden XML-Codeausschnitt unter dem `<ClaimsProviderSelections>`-Knoten hinzu:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion
Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. In diesem Fall soll bei der Aktion Azure AD B2C mit dem Microsoft-Konto kommunizieren, um ein Token zu empfangen. Verknüpfen Sie die Schaltfläche mit einer Aktion, indem Sie das technische Profil für Ihren Microsoft-Kontoanspruchsanbieter verknüpfen:

1.  Suchen Sie `<OrchestrationStep>`, das `Order="2"` im `<UserJourney>`-Knoten enthält.
2.  Fügen Sie folgenden XML-Codeausschnitt unter dem `<ClaimsExchanges>`-Knoten hinzu:

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

> [!NOTE]
>
>   * Stellen Sie sicher, dass `Id` den gleichen Wert wie `TargetClaimsExchangeId` im vorherigen Abschnitt hat.
>   * Stellen Sie sicher, dass für `TechnicalProfileReferenceId`-ID das technische Profil festgelegt ist, das Sie zuvor erstellt haben (MSA-OIDC).

## <a name="upload-the-policy-to-your-tenant"></a>Hochladen der Richtlinie in Ihren Mandanten
1.  Wechseln Sie im [Azure-Portal](https://portal.azure.com) in den [Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und öffnen Sie das Blatt **Azure AD B2C**.
2.  Wählen Sie **Framework für die Identitätsfunktion** aus.
3.  Öffnen Sie das Blatt **Alle Richtlinien**.
4.  Wählen Sie **Richtlinie hochladen** aus.
5.  Aktivieren Sie das Kontrollkästchen **Richtlinie überschreiben, sofern vorhanden**.
6.  **Laden** Sie „TrustFrameworkExtensions.xml“ hoch, und stellen Sie sicher, dass kein Fehler bei der Validierung auftritt.

## <a name="test-the-custom-policy-by-using-run-now"></a>Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“

1.  Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.
> [!NOTE]
>
>**Jetzt ausführen** macht erforderlich, dass mindestens eine Anwendung im Mandanten vorab registriert wird. Informationen zum Registrieren von Anwendungen finden Sie in den Artikeln [Erste Schritte](active-directory-b2c-get-started.md) bzw. [Anwendungsregistrierung](active-directory-b2c-app-registration.md) zu Azure AD B2C.
2.  Öffnen Sie **B2C_1A_signup_signin** – die benutzerdefinierte Richtlinie der vertrauenden Seite (Relying Party, RP), die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus.
3.  Sie sollten sich mit dem Microsoft-Konto anmelden können.

## <a name="optional-register-the-microsoft-account-claims-provider-to-profile-edit-user-journey"></a>[Optional] Registrieren des Microsoft-Kontoanspruchsanbieters bei der Profile-Edit-User Journey
Vielleicht möchten Sie den Microsoft-Kontoidentitätsanbieter auch Ihrer Benutzer-`ProfileEdit`-User Journey hinzufügen. Um ihn verfügbar zu machen, wiederholen wir die letzten beiden Schritte:

### <a name="display-the-button"></a>Anzeigen der Schaltfläche
1.  Öffnen Sie die Erweiterungsdatei Ihrer Richtlinie (z.B. TrustFrameworkExtensions.xml).
2.  Suchen Sie den `<UserJourney>`-Knoten, der `Id="ProfileEdit"` in der User Journey enthält, die Sie kopiert haben.
3.  Suchen Sie den `<OrchestrationStep>`-Knoten, der `Order="1"` enthält.
4.  Fügen Sie folgenden XML-Codeausschnitt unter dem `<ClaimsProviderSelections>`-Knoten hinzu:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion
1.  Suchen Sie `<OrchestrationStep>`, das `Order="2"` im `<UserJourney>`-Knoten enthält.
2.  Fügen Sie folgenden XML-Codeausschnitt unter dem `<ClaimsExchanges>`-Knoten hinzu:

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Testen der benutzerdefinierten Profile-Edit-Richtlinie mit „Jetzt ausführen“
1.  Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.
2.  Öffnen Sie **B2C_1A_ProfileEdit** – die benutzerdefinierte Richtlinie der vertrauenden Seite, die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus.
3.  Sie sollten sich mit dem Microsoft-Konto anmelden können.

## <a name="download-the-complete-policy-files"></a>Herunterladen der vollständigen Richtliniendateien
Optional: Sie sollten nach Abschluss der exemplarischen Vorgehensweise „Erste Schritte mit benutzerdefinierten Richtlinien“ Ihr Szenario mithilfe Ihrer eigenen Dateien mit benutzerdefinierten Richtlinien erstellen, anstatt diese Beispieldateien zu verwenden.  [Beispielrichtliniendateien zur Referenz](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-msa-app)

