---
title: "Azure Active Directory B2C: Hinzufügen von Google+ als OAuth2-Identitätsanbieter mithilfe benutzerdefinierter Richtlinien"
description: "Beispiel mit Google+ als Identitätsanbieter unter Verwendung des OAuth2-Protokolls"
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
ms.openlocfilehash: 890690f704f033895e42877e3c9355a5a3df07b7
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---

# <a name="azure-active-directory-b2c-add-google-as-an-oauth2-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Hinzufügen von Google+ als OAuth2-Identitätsanbieter mithilfe benutzerdefinierter Richtlinien

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dieser Anleitung erfahren Sie, wie Sie die Anmeldung für Benutzer von einem Google+-Konto mittels [benutzerdefinierter Richtlinien](active-directory-b2c-overview-custom.md) aktivieren.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte im Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) aus.

Diese Schritte umfassen:

1.  Erstellen einer Google-Kontoanwendung.
2.  Hinzufügen des Google+-Kontoanwendungsschlüssels zu Azure AD B2C
3.  Hinzufügen des Anspruchsanbieters zu einer Richtlinie.
4.  Registrieren des Google+-Kontoanspruchsanbieters bei einer User Journey.
5.  Hochladen der Richtlinie in einen Azure AD B2C-Mandanten und deren Test.

## <a name="create-a-google-account-application"></a>Erstellen einer Google+-Kontoanwendung
Um Google+ als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden zu können, müssen Sie eine Google+-Anwendung erstellen und die entsprechenden Parameter bereitstellen. Sie können eine Google+-Anwendung hier registrieren: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1.  Klicken Sie auf die [Google Developers Console](https://console.developers.google.com/), und melden Sie sich mit Ihren Anmeldeinformationen für Google+ an.
2.  Klicken Sie auf **Projekt erstellen**, geben Sie unter **Projektname** einen Projektnamen ein, und klicken Sie auf **Erstellen**.

3.  Klicken Sie auf das **Menü „Projekte“**.

    ![Google+-Konto – Projekt auswählen](media/active-directory-b2c-custom-setup-goog-idp/goog-add-new-app1.png)

4.  Klicken Sie auf die Schaltfläche **+**.

    ![Google+-Konto – neues Projekt erstellen](media/active-directory-b2c-custom-setup-goog-idp//goog-add-new-app2.png)

5.  Geben Sie einen **Projektnamen** ein, und klicken Sie auf **Erstellen**.

    ![Google+-Konto – neues Projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-app-name.png)

6.  Warten Sie, bis das Projekt bereit ist, und klicken Sie auf das **Menü „Projekte“**.

    ![Google+-Konto – warten, bis ein neues Projekt zur Verwendung bereit ist](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app1.png)

7.  Klicken Sie auf Ihren Projektnamen.

    ![Google+-Konto – neues Projekt auswählen](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app2.png)

8.  Klicken Sie links im Navigationsbereich auf **API Manager** und dann auf **Zugangsdaten**.
9.  Klicken Sie oben auf die Registerkarte **OAuth-Zustimmungsbildschirm**.

    ![Google+-Konto – OAuth-Zustimmungsbildschirm festlegen](media/active-directory-b2c-custom-setup-goog-idp/goog-add-cred.png)

10.  Wählen Sie unter **E-Mail-Adresse** eine gültige Adresse aus, oder geben Sie sie ein, geben Sie einen Wert für **Produktname** ein, und klicken Sie auf **Speichern**.

    ![Google+ – Anwendungsanmeldeinformationen](media/active-directory-b2c-custom-setup-goog-idp/goog-consent-screen.png)

11.  Klicken Sie auf **Anmeldeinformationen hinzufügen**, und wählen Sie dann **OAuth-Client-ID** aus.

    ![Google+ – neue Anwendungsanmeldeinformationen erstellen](media/active-directory-b2c-custom-setup-goog-idp/goog-add-oauth2-client-id.png)

12.  Wählen Sie unter **Anwendungstyp** die Option **Webanwendung** aus.

    ![Google+ – Anwendungstyp auswählen](media/active-directory-b2c-custom-setup-goog-idp/goog-web-app.png)

13.  Geben Sie einen **Namen** für die Anwendung an, und geben Sie `https://login.microsoftonline.com` im Feld **Autorisierte JavaScript-Quellen** und `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` im Feld **Autorisierte Weiterleitungs-URIs** ein. Ersetzen Sie **{tenant}** durch den Namen Ihres Mandanten (z.B. „contosob2c.onmicrosoft.com“). Beim Wert für **{tenant}** wird die Groß-/Kleinschreibung beachtet. Klicken Sie auf **Erstellen**.

    ![Google+ – autorisierte JavaScript-Ursprünge und Umleitungs-URIs bereitstellen](media/active-directory-b2c-custom-setup-goog-idp/goog-create-client-id.png)

14.  Kopieren Sie die Werte für **Client-ID** und **Geheimer Clientschlüssel**. Sie benötigen beide, um Google+ als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. Das **Clientgeheimnis** ist eine wichtige Anmeldeinformation.

    ![Google+ – Werte für „Client-ID“ und „Geheimer Clientschlüssel“ kopieren](media/active-directory-b2c-custom-setup-goog-idp/goog-client-secret.png)

## <a name="add-the-google-account-application-key-to-azure-ad-b2c"></a>Hinzufügen des Google+-Kontoanwendungsschlüssels zu Azure AD B2C
Verbund mit Google+-Konten erfordert einen geheimen Clientschlüssel für das Google+-Konto, um Azure AD B2C im Auftrag der Anwendung zu vertrauen. Sie müssen Ihr Google+-Anwendungsgeheimnis im Azure AD B2C-Mandanten speichern:  

1.  Wechseln Sie zu Ihrem Azure AD B2C-Mandanten, und wählen Sie **B2C-Einstellungen** > **Framework für die Identitätsfunktion**.
2.  Wählen Sie **Policy Keys** (Richtlinienschlüssel), um die in Ihrem Mandanten verfügbaren Schlüssel anzuzeigen.
3.  Klicken Sie auf **+Hinzufügen**.
4.  Verwenden Sie für **Optionen** die Option **Manuell**.
5.  Verwenden Sie für **Name** den Wert `GoogleSecret`.  
    Das Präfix `B2C_1A_` wird möglicherweise automatisch hinzugefügt.
6.  Geben Sie in das Feld **Geheimnis** Ihr Microsoft-Anwendungsgeheimnis aus „https://apps.dev.microsoft.com“ ein.
7.  Verwenden Sie für **Schlüsselverwendung**, die Option **Signatur**.
8.  Klicken Sie auf **Erstellen**
9.  Vergewissern Sie sich, dass Sie den Schlüssel `B2C_1A_GoogleSecret` erstellt haben.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Hinzufügen eines Anspruchsanbieters in Ihrer Erweiterungsrichtlinie

Wenn Sie möchten, dass Benutzer sich mithilfe des Google+-Kontos anmelden, müssen Sie das Google+-Konto als Anspruchsanbieter definieren. Das heißt, Sie müssen einen Endpunkt festlegen, mit dem Azure AD B2C kommuniziert. Der Endpunkt bietet eine Reihe von Ansprüchen, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde.

Definieren Sie das Google+-Konto als Anspruchsanbieter, indem Sie den `<ClaimsProvider>`-Knoten in Ihrer Erweiterungsrichtliniendatei hinzufügen:

1.  Öffnen Sie die Erweiterungsrichtliniendatei (TrustFrameworkExtensions.xml) von Ihrem Arbeitsverzeichnis aus. Wenn Sie einen XML-Editor benötigen, können Sie [Visual Studio Code](https://code.visualstudio.com/download), einen einfachen plattformübergreifenden Editor, ausprobieren.
2.  Suchen Sie nach dem `<ClaimsProviders>`-Abschnitt.
3.  Fügen Sie den folgenden XML-Codeausschnitt unter dem `ClaimsProviders`-Element hinzu, und ersetzen Sie den `client_id`-Wert vor dem Speichern der Datei durch Ihre Google+-Kontoanwendungsclient-ID.  

```xml
<ClaimsProvider>
    <Domain>google.com</Domain>
    <DisplayName>Google</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Google-OAUTH">
        <DisplayName>Google</DisplayName>
        <Protocol Name="OAuth2" />
        <Metadata>
        <Item Key="ProviderName">google</Item>
        <Item Key="authorization_endpoint">https://accounts.google.com/o/oauth2/auth</Item>
        <Item Key="AccessTokenEndpoint">https://accounts.google.com/o/oauth2/token</Item>
        <Item Key="ClaimsEndpoint">https://www.googleapis.com/oauth2/v1/userinfo</Item>
        <Item Key="scope">email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Google+ application ID</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_GoogleSecret" />
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="google.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        <ErrorHandlers>
        <ErrorHandler>
            <ErrorResponseFormat>json</ErrorResponseFormat>
            <ResponseMatch>$[?(@@.error == 'invalid_grant')]</ResponseMatch>
            <Action>Reauthenticate</Action>
            <!--In case of authorization code used error, we don't want the user to select his account again.-->
            <!--AdditionalRequestParameters Key="prompt">select_account</AdditionalRequestParameters-->
        </ErrorHandler>
        </ErrorHandlers>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-the-google-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Registrieren des Google+-Kontoanspruchsanbieters zum Registrieren oder Anmelden bei einer User Journey

Der Identitätsanbieter wurde eingerichtet.  Er ist jedoch nicht in den Bildschirmen zur Registrierung/Anmeldung verfügbar. Fügen Sie den Google+-Kontoidentitätsanbieter Ihrer Benutzer-`SignUpOrSignIn`-User Journey hinzu. Um ihn verfügbar zu machen, erstellen wir ein Duplikat einer vorhandenen Vorlagen-User Journey.  Dann fügen wir den Google+-Kontoidentitätsanbieter hinzu:

>[!NOTE]
>
>Wenn Sie das `<UserJourneys>`-Element aus der Basisdatei Ihrer Richtlinie in die Erweiterungsdatei (TrustFrameworkExtensions.xml) kopiert haben, können Sie zu diesem Abschnitt springen.

1.  Öffnen Sie die Basisdatei Ihrer Richtlinie (z.B. „TrustFrameworkBase.xml“).
2.  Suchen Sie das `<UserJourneys>`-Element, und kopieren Sie den gesamten Inhalt des `<UserJourneys>`-Knotens.
3.  Öffnen Sie die Erweiterungsdatei (z.B. „TrustFrameworkExtensions.xml“), und suchen Sie nach dem Element `<UserJourneys>`. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
4.  Fügen Sie den gesamten Inhalt des `<UserJournesy>`-Knotens ein, den Sie als untergeordnetes Element des `<UserJourneys>`-Elements kopiert haben.

### <a name="display-the-button"></a>Anzeigen der Schaltfläche
Das `<ClaimsProviderSelections>`-Element definiert die Liste der Anspruchsanbieter-Auswahloptionen und deren Reihenfolge.  Das `<ClaimsProviderSelection>`-Element entspricht einer Schaltfläche für einen Identitätsanbieter auf einer Registrierungs-/Anmeldeseite. Wenn Sie ein `<ClaimsProviderSelection>`-Element für das Google+-Konto hinzufügen, wird eine neue Schaltfläche angezeigt, wenn ein Benutzer zu der Seite gelangt. So fügen Sie dieses Element hinzu:

1.  Suchen Sie den `<UserJourney>`-Knoten, der `Id="SignUpOrSignIn"` in der User Journey enthält, die Sie kopiert haben.
2.  Suchen Sie den `<OrchestrationStep>`-Knoten, der `Order="1"` enthält.
3.  Fügen Sie folgenden XML-Codeausschnitt unter dem `<ClaimsProviderSelections>`-Knoten hinzu:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion
Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. In diesem Fall soll bei der Aktion Azure AD B2C mit dem Google+-Konto kommunizieren, um ein Token zu empfangen.

1.  Suchen Sie `<OrchestrationStep>`, das `Order="2"` im `<UserJourney>`-Knoten enthält.
2.  Fügen Sie folgenden XML-Codeausschnitt unter dem `<ClaimsExchanges>`-Knoten hinzu:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

>[!NOTE]
>
> * Stellen Sie sicher, dass `Id` den gleichen Wert wie `TargetClaimsExchangeId` im vorherigen Abschnitt hat.
> * Stellen Sie sicher, dass für `TechnicalProfileReferenceId`-ID das technische Profil festgelegt ist, das Sie zuvor erstellt haben (Google-OAUTH).

## <a name="upload-the-policy-to-your-tenant"></a>Hochladen der Richtlinie in Ihren Mandanten
1.  Wechseln Sie im [Azure-Portal](https://portal.azure.com) in den [Kontext Ihres Azure AD B2C-Mandanten](active-directory-b2c-navigate-to-b2c-context.md), und öffnen Sie das Blatt **Azure AD B2C**.
2.  Wählen Sie **Framework für die Identitätsfunktion** aus.
3.  Öffnen Sie das Blatt **Alle Richtlinien**.
4.  Wählen Sie **Richtlinie hochladen** aus.
5.  Aktivieren Sie das Kontrollkästchen **Richtlinie überschreiben, sofern vorhanden**.
6.  **Laden** Sie „TrustFrameworkExtensions.xml“ hoch, und stellen Sie sicher, dass kein Fehler bei der Validierung auftritt.

## <a name="test-the-custom-policy-by-using-run-now"></a>Testen der benutzerdefinierten Richtlinie mit „Jetzt ausführen“
1.  Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.

    >[!NOTE]
    >
    >    **Jetzt ausführen** macht erforderlich, dass mindestens eine Anwendung im Mandanten vorab registriert wird. 
    >    Informationen zum Registrieren von Anwendungen finden Sie in den Artikeln [Erste Schritte](active-directory-b2c-get-started.md) bzw. [Anwendungsregistrierung](active-directory-b2c-app-registration.md) zu Azure AD B2C.


2.  Öffnen Sie **B2C_1A_signup_signin** – die benutzerdefinierte Richtlinie der vertrauenden Seite (Relying Party, RP), die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus.
3.  Sie sollten sich mit dem Google+-Konto anmelden können.

## <a name="optional-register-the-google-account-claims-provider-to-profile-edit-user-journey"></a>[Optional] Registrieren des Google+-Kontoanspruchsanbieters bei der Profile-Edit-User Journey
Vielleicht möchten Sie den Google+-Kontoidentitätsanbieter auch Ihrer Benutzer-`ProfileEdit`-User Journey hinzufügen. Um ihn verfügbar zu machen, wiederholen wir die letzten beiden Schritte:

### <a name="display-the-button"></a>Anzeigen der Schaltfläche
1.  Öffnen Sie die Erweiterungsdatei Ihrer Richtlinie (z.B. TrustFrameworkExtensions.xml).
2.  Suchen Sie den `<UserJourney>`-Knoten, der `Id="ProfileEdit"` in der User Journey enthält, die Sie kopiert haben.
3.  Suchen Sie den `<OrchestrationStep>`-Knoten, der `Order="1"` enthält.
4.  Fügen Sie folgenden XML-Codeausschnitt unter dem `<ClaimsProviderSelections>`-Knoten hinzu:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion
1.  Suchen Sie `<OrchestrationStep>`, das `Order="2"` im `<UserJourney>`-Knoten enthält.
2.  Fügen Sie folgenden XML-Codeausschnitt unter dem `<ClaimsExchanges>`-Knoten hinzu:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Testen der benutzerdefinierten Profile-Edit-Richtlinie mit „Jetzt ausführen“

1.  Öffnen Sie die **Azure AD B2C-Einstellungen**, und navigieren Sie zu **Framework für die Identitätsfunktion**.
2.  Öffnen Sie **B2C_1A_ProfileEdit** – die benutzerdefinierte Richtlinie der vertrauenden Seite, die Sie hochgeladen haben. Wählen Sie **Jetzt ausführen** aus.
3.  Sie sollten sich mit dem Google+-Konto anmelden können.

## <a name="download-the-complete-policy-files"></a>Herunterladen der vollständigen Richtliniendateien
Optional: Sie sollten nach Abschluss der exemplarischen Vorgehensweise „Erste Schritte mit benutzerdefinierten Richtlinien“ Ihr Szenario mithilfe Ihrer eigenen Dateien mit benutzerdefinierten Richtlinien erstellen, anstatt diese Beispieldateien zu verwenden.  [Beispielrichtliniendateien zur Referenz](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-goog-app)

