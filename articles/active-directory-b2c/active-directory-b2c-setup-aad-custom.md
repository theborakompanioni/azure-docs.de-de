---
title: "Azure Active Directory B2C: Hinzufügen von Azure AD-Anbietern mithilfe von benutzerdefinierten Richtlinien | Microsoft-Dokumentation"
description: Ein Thema zu benutzerdefinierten Azure Active Directory B2C-Richtlinien
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 31f0dfe5-1ad0-4a25-a53b-8acc71bcea72
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: parakhj
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 652a2eb0db0e41c4706e06cd0cb2e97ce1eb6ab2
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---
# <a name="azure-active-directory-b2c-log-in-using-azure-ad-accounts"></a>Azure Active Directory B2C: Anmelden mithilfe von Azure AD-Konten

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel erfahren Sie, wie Sie die Anmeldung für Benutzer von einer bestimmten Azure AD-Organisation mittels [benutzerdefinierter Richtlinien](active-directory-b2c-overview-custom.md) aktivieren.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass Sie alle Schritte unter [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) durchgeführt haben.

Dies umfasst:

1. Erstellen eines Azure AD B2C-Mandanten
1. Erstellen einer Azure AD B2C-Anwendung
1. Registrieren von zwei Richtlinienmodulanwendungen
1. Einrichten von Schlüsseln
1. Einrichten des Starter Pack

## <a name="create-an-azure-ad-app"></a>Erstellen einer Azure AD-App

Um die Anmeldung für Benutzer von einer bestimmten Azure AD-Organisation zu aktivieren, müssen Sie eine Anwendung im Azure AD-Mandanten der Organisation registrieren.

>[!NOTE]
> Auf den Azure AD-Mandanten der Organisation wird als `contoso.com` verwiesen und auf den Azure AD B2C-Mandanten als `fabrikamb2c.onmicrosoft.com`.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
1. Klicken Sie auf der oberen Leiste auf Ihr Konto, und wählen Sie in der **Verzeichnisliste** den Azure AD-Mandanten der Organisation aus, bei dem Sie Ihre Anwendung (d.h. contoso.com) registrieren möchten.
1. Klicken Sie im linken Navigationsbereich auf **Weitere Dienste**, und suchen Sie nach **App-Registrierungen**.
1. Wählen Sie **Registrierung einer neuen Anwendung** aus.
1. Geben Sie einen **Namen** für Ihre Anwendung ein (z.B. „Azure AD B2C-App“).
1. Wählen Sie **Web-App/API** als Anwendungstyp aus.
1. Geben Sie für „Anmelde-URL“ die nachfolgende URL ein, wobei `yourtenant` durch den Namen Ihres Azure AD B2C-Mandanten (also „fabrikamb2c.onmicrosoft.com“) ersetzt werden sollte.

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Speichern Sie die **Anwendungs-ID**.
1. Klicken Sie auf die neu erstellte App.
1. Klicken Sie auf dem Blatt „Einstellungen“ auf **Schlüssel**.
1. Erstellen Sie einen neuen Schlüssel, und speichern Sie ihn für den nächsten Abschnitt.

## <a name="add-the-azure-ad-key-to-azure-ad-b2c"></a>Hinzufügen des Azure AD-Schlüssels zu Azure AD B2C

Sie müssen den Anwendungsschlüssel `contoso.com` in Ihrem Azure AD B2C-Mandanten speichern. Gehen Sie dazu folgendermaßen vor:

1. Navigieren Sie zu Ihrem Azure AD B2C-Mandanten, und öffnen Sie „B2C-Einstellungen“ > „Identity Experience Framework“ > „Policy Keys“ (Richtlinienschlüssel).
1. Klicken Sie auf „+ Hinzufügen“.
1. Optionen:
 * Wählen Sie „Option“ > `Manual`.
 * Name: > `ContosoAppSecret`: Wählen Sie einen Namen aus, der mit dem Namen Ihres Azure AD-Mandanten übereinstimmt.  Dem Namen Ihres Schlüssels wird automatisch das Präfix „B2C_1A_“ hinzugefügt.
 * Fügen Sie Ihren Anwendungsschlüssel in das Textfeld `Secret` ein.
 * Wählen Sie „Signatur“.
1. Klicken Sie auf `Create`.
1. Bestätigen, dass der Schlüssel erstellt wurde: `B2C_1A_ContosoAppSecret`


## <a name="add-a-claims-provider-in-your-base-policy"></a>Hinzufügen eines Anspruchsanbieters in Ihrer Basisrichtlinie

Damit sich Benutzer mithilfe von Azure AD anmelden können, müssen Sie Azure AD als Anspruchsanbieter definieren. Das heißt, Sie müssen einen Endpunkt festlegen, mit dem Azure AD B2C kommunizieren soll. Der Endpunkt *bietet* eine Reihe von *Ansprüchen*, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde. Hierzu können Sie Azure AD als `<ClaimsProvider>` in der Erweiterungsdatei Ihrer Richtlinie hinzufügen.

1. Öffnen Sie die Erweiterungsdatei aus Ihrem Arbeitsverzeichnis (TrustFrameworkExtensions.xml).
1. Suchen Sie nach dem Abschnitt `<ClaimsProviders>`. Wenn dieser nicht vorhanden sein sollte, fügen Sie ihn im Stammknoten hinzu.
1. Fügen Sie einen neuen `<ClaimsProvider>` hinzu, wie nachfolgend beschrieben:

    ```XML
    <ClaimsProvider>
        <Domain>Contoso</Domain>
        <DisplayName>Login using Contoso</DisplayName>
        <TechnicalProfiles>
            <TechnicalProfile Id="ContosoProfile">
                <DisplayName>Contoso Employee</DisplayName>
                <Description>Login with your Contoso account</Description>
                <Protocol Name="OpenIdConnect"/>
                <OutputTokenFormat>JWT</OutputTokenFormat>
                <Metadata>
                    <Item Key="METADATA">https://login.windows.net/contoso.com/.well-known/openid-configuration</Item>
                    <Item Key="ProviderName">https://sts.windows.net/00000000-0000-0000-0000-000000000000/</Item>
                    <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="IdTokenAudience">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="response_types">id_token</Item>
                    <Item Key="UsePolicyInRedirectUri">false</Item>
                </Metadata>
                <CryptographicKeys>
                    <Key Id="client_secret" StorageReferenceId="B2C_1A_ContosoAppSecret"/>
                </CryptographicKeys>
                <OutputClaims>
                    <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="oid"/>
                    <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
                    <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
                    <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
                    <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
                    <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="contosoAuthentication" />
                    <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="AzureADContoso" />
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

1. Ändern Sie im Knoten `<ClaimsProvider>` den Wert für `<Domain>` in einen eindeutigen Wert, der eine Unterscheidung von anderen Identitätsanbietern ermöglicht.
1. Ändern Sie im Knoten `<ClaimsProvider>` den Wert für `<DisplayName>` in einen Anzeigenamen für den Anspruchsanbieter. Dieser Wert wird derzeit nicht verwendet.

### <a name="update-the-technical-profile"></a>Aktualisieren des technischen Profils

Um ein Token vom Azure AD-Endpunkt zu erhalten, müssen Sie die Protokolle definieren, die Azure AD B2C zur Kommunikation mit Azure AD verwenden soll. Dies erfolgt im Element `<TechnicalProfile>` von `<ClaimsProvider>`.

1. Aktualisieren Sie die ID des Knotens `<TechnicalProfile>`. Diese ID wird als Verweis auf dieses technische Profil aus anderen Teilen der Richtlinie verwendet.
1. Aktualisieren Sie den Wert für `<DisplayName>`. Dieser Wert wird auf der Anmeldeschaltfläche auf dem Anmeldebildschirm angezeigt.
1. Aktualisieren Sie den Wert für `<Description>`.
1. Azure AD verwendet das OpenID Connect-Protokoll. Stellen Sie daher sicher, dass der Wert für `<Protocol>` „OpenIDConnect“ lautet.

Sie müssen den Abschnitt `<Metadata>` im obigen XML-Dokument entsprechend den Konfigurationseinstellungen für Ihren speziellen Azure AD-Mandanten aktualisieren. Aktualisieren Sie wie folgt die Metadatenwerte im XML-Dokument :

1. Setzen Sie `<Item Key="METADATA">` auf `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, wobei `yourAzureADtenant` der Name Ihres Azure AD-Mandanten (z.B. „contoso.com“) ist.
1. Öffnen Sie Ihren Browser, und rufen Sie die URL `Metadata` auf, die Sie gerade aktualisiert haben.
1. Suchen Sie im Browser nach dem Objekt „Aussteller“, und kopieren Sie dessen Wert. Es sollte in etwa wie folgt aussehen: `https://sts.windows.net/{tenantId}/`.
1. Fügen Sie den Wert für `<Item Key="ProviderName">` in das XML-Dokument ein.
1. Setzen Sie über die App-Registrierung `<Item Key="client_id">` auf `Application ID`.
1. Setzen Sie über die App-Registrierung `<Item Key="IdTokenAudience">` auf `Application ID`.
1. Vergewissern Sie sich, dass `<Item Key="response_types">` auf `id_token` gesetzt ist.
1. Vergewissern Sie sich, dass `<Item Key="UsePolicyInRedirectUri">` auf `false` gesetzt ist.

Sie müssen außerdem das Azure AD-Geheimnis, das Sie in Ihrem Azure AD B2C-Mandanten registriert haben, mit `<ClaimsProvider>` von Azure AD verknüpfen.

* Ändern Sie im Abschnitt `<CryptographicKeys>` des obigen XML-Dokuments den Wert für `StorageReferenceId` in die Referenz-ID des Geheimnisses (z.B. „ContosoAppSecret“), das Sie definiert haben.

### <a name="upload-the-extension-file-for-verification"></a>Hochladen der Erweiterungsdatei zur Überprüfung

Nun haben Sie Ihre Richtlinie so konfiguriert, dass Azure AD B2C mit Ihrem Azure AD-Verzeichnis kommunizieren kann. Versuchen Sie, die Erweiterungsdatei Ihrer Richtlinie hochzuladen, um sich zu vergewissern, dass soweit keine Probleme vorliegen. Gehen Sie dazu wie folgt vor:

1. Navigieren Sie in Ihrem Azure AD B2C-Mandanten zum Blatt **Alle Richtlinien**.
1. Aktivieren Sie das Kontrollkästchen **Richtlinie überschreiben, sofern vorhanden**.
1. Laden Sie die Erweiterungsdatei (TrustFrameworkExtensions.xml) hoch, und stellen Sie sicher, dass die Validierung nicht fehlschlägt.

## <a name="register-the-azure-ad-claims-provider-to-a-user-journey"></a>Registrieren des Azure AD-Anspruchsanbieters bei einer User Journey

Nun müssen Sie den Azure AD-Identitätsanbieter zu einer Ihrer User Journeys hinzufügen. An dieser Stelle wurde der Identitätsanbieter eingerichtet, ist jedoch auf keinem der Registrierungs-/Anmeldebildschirme vorhanden. Zu diesem Zweck erstellen wir ein Duplikat einer vorhandenen User Journey-Vorlage und ändern diese, damit sie ebenfalls einen Azure AD-Identitätsanbieter aufweist.

1. Öffnen Sie die Basisdatei Ihrer Richtlinie (z.B. „TrustFrameworkBase.xml“).
1. Suchen Sie nach dem Element `<UserJourneys>`, und kopieren Sie die gesamte `<UserJourney>` mit der ID „SignUpOrSignIn“.
1. Öffnen Sie die Erweiterungsdatei (z.B. „TrustFrameworkExtensions.xml“), und suchen Sie nach dem Element `<UserJourneys>`. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
1. Fügen Sie die gesamte `<UserJourney>` ein, die Sie als untergeordnetes Element des `<UserJourneys>`-Elements kopiert haben.
1. Benennen Sie die ID der neuen `<UserJourney>` um (d.h. „SignUpOrSignUsingContoso“).

### <a name="display-the-button"></a>Anzeigen der Schaltfläche

Das Element `<ClaimsProviderSelection>` entspricht einer Schaltfläche für einen Identitätsanbieter auf einem Registrierungs-/Anmeldebildschirm. Durch Hinzufügen eines `<ClaimsProviderSelection>`-Elements für Azure AD wird eine neue Schaltfläche angezeigt, wenn ein Benutzer zu der Seite gelangt. Gehen Sie dazu folgendermaßen vor:

1. Suchen Sie nach `<OrchestrationStep>` mit `Order="1"` in der `<UserJourney>`, die Sie gerade erstellt haben.
1. Fügen Sie Folgendes hinzu:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. Legen Sie `TargetClaimsExchangeId` als entsprechenden Wert fest. Es wird empfohlen, derselben Konvention wie anderen zu folgen –  *\[ClaimProviderName\]Exchange*.

### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion

Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. In diesem Fall soll bei der Aktion Azure AD B2C mit Azure AD kommunizieren, um ein Token zu empfangen. Dies gelingt Ihnen, indem Sie das technische Profil für Ihren Azure AD-Anspruchsanbieter verknüpfen.

1. Suchen Sie nach `<OrchestrationStep>` mit `Order="2"` im Knoten `<UserJourney>`.
1. Fügen Sie Folgendes hinzu:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

1. Ändern Sie `Id` in denselben Wert wie den von `TargetClaimsExchangeId` oben.
1. Ändern Sie `TechnicalProfileReferenceId` in `Id` des technischen Profils, das Sie zuvor erstellt haben (z.B. „ContosoProfile“).

### <a name="upload-the-updated-extension-file"></a>Hochladen der aktualisierten Erweiterungsdatei

Die Änderung der Erweiterungsdatei ist abgeschlossen. Speichern Sie diese Datei und laden Sie sie hoch. Stellen Sie sicher, dass alle Validierungen erfolgreich waren.

### <a name="update-the-rp-file"></a>Aktualisieren der Datei der vertrauenden Seite

Nun müssen Sie die Datei der vertrauenden Seite aktualisieren, die die gerade erstellte User Journey initiiert.

1. Erstellen Sie eine Kopie von der Datei „SignUpOrSignIn.xml“ in Ihrem Arbeitsverzeichnis, und benennen Sie sie um (z.B. „SignUpOrSignInWithAAD.xml“).
1. Öffnen Sie die neue Datei, und ändern Sie das `PolicyId`-Attribut für `<TrustFrameworkPolicy>` in einen eindeutigen Wert (z.B. SignUpOrSignInWithAAD). Dies ist der Name Ihrer Richtlinie (d.h. B2C\_1A\_SignUpOrSignInWithAAD).
1. Ändern Sie das `ReferenceId`-Attribut in `<DefaultUserJourney>`, sodass es der ID der neuen User Journey, die Sie erstellt haben, entspricht (z.B. „SignUpOrSignUsingContoso“).
1. Speichern Sie die Änderungen, und laden Sie die Datei hoch.

## <a name="troubleshooting"></a>Problembehandlung

Testen Sie die benutzerdefinierte Richtlinie, die Sie soeben hochgeladen haben, indem Sie das zugehörige Blatt öffnen und dann auf „Jetzt ausführen“ klicken. Informationen zum Umgang mit etwaigen auftretenden Fehlern finden Sie unter [Problembehandlung](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Nächste Schritte

Senden Sie uns Ihr Feedback an AADB2CPreview@microsoft.com.

