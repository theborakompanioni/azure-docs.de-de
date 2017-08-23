---
title: "Azure Active Directory B2C: Hinzufügen eines Azure AD-Anbieters mithilfe von benutzerdefinierten Richtlinien | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über benutzerdefinierte Azure Active Directory B2C-Richtlinien."
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
ms.translationtype: HT
ms.sourcegitcommit: 495a695466c47c6030716d97c52b3bbf5ce9bf99
ms.openlocfilehash: 6c073d70debfdc3560405955d65fa9ccaa7d8b1f
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="azure-active-directory-b2c-sign-in-by-using-azure-ad-accounts"></a>Azure Active Directory B2C: Anmelden mithilfe von Azure AD-Konten

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel erfahren Sie, wie Sie die Anmeldung für Benutzer einer bestimmten Azure Active Directory-Organisation (Azure AD) mittels [benutzerdefinierter Richtlinien](active-directory-b2c-overview-custom.md) aktivieren.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte im Artikel [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) aus.

Diese Schritte umfassen:

1. Erstellen eines Azure Active Directory B2C-Mandanten (Azure AD B2C)
2. Erstellen einer Azure AD B2C-Anwendung
3. Registrieren von zwei Richtlinienmodulanwendungen
4. Einrichten von Schlüsseln
5. Einrichten des Starter Pack

## <a name="create-an-azure-ad-app"></a>Erstellen einer Azure AD-App

Um die Anmeldung für Benutzer von einer bestimmten Azure AD-Organisation zu aktivieren, müssen Sie eine Anwendung im Azure AD-Mandanten der Organisation registrieren.

>[!NOTE]
> Wir verwenden „Contoso.com“ für den Azure AD-Mandanten der Organisation und „fabrikamb2c.onmicrosoft.com“ als den Azure AD B2C-Mandanten in den folgenden Anweisungen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
1. Wählen Sie in der oberen Leiste Ihr Konto aus. Wählen Sie in der **Verzeichnisliste** den Azure AD-Mandanten der Organisation aus, bei dem Sie Ihre Anwendung registrieren möchten (contoso.com).
1. Wählen Sie im linken Navigationsbereich **Weitere Dienste** aus, und suchen Sie nach „App-Registrierungen“.
1. Wählen Sie **Registrierung einer neuen Anwendung** aus.
1. Geben Sie einen Namen für Ihre Anwendung ein (z.B. `Azure AD B2C App`).
1. Wählen Sie **Web-App/API** als Anwendungstyp aus.
1. Geben Sie für **Anmelde-URL** die folgende URL ein, in der `yourtenant` durch den Namen Ihres Azure AD B2C-Mandanten (`fabrikamb2c.onmicrosoft.com`) ersetzt wird:

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Speichern Sie die Anwendungs-ID.
1. Wählen Sie die neu erstellte Anwendung aus.
1. Wählen Sie auf dem Blatt **Einstellungen** die Option **Schlüssel** aus.
1. Erstellen Sie einen neuen Schlüssel, und speichern Sie ihn. Sie werden ihn in den Schritten im nächsten Abschnitt verwenden.

## <a name="add-the-azure-ad-key-to-azure-ad-b2c"></a>Hinzufügen des Azure AD-Schlüssels zu Azure AD B2C

Sie müssen den Anwendungsschlüssel „contoso.com“ in Ihrem Azure AD B2C-Mandanten speichern. Gehen Sie dazu folgendermaßen vor:
1. Wechseln Sie zu Ihrem Azure AD B2C-Mandanten, und wählen Sie **B2C Settings** (B2C Settings) > **Framework für die Identitätsfunktion** > **Richtlinienschlüssel**.
1. Wählen Sie **+ Hinzufügen** aus.
1. Wählen Sie diese Optionen aus, oder geben Sie sie ein:
   * Wählen Sie **Manuell** aus.
   * Wählen Sie für **Name** einen Namen aus, der mit dem Namen Ihres Azure AD-Mandanten übereinstimmt (z.B. `ContosoAppSecret`).  Dem Namen Ihres Schlüssels wird automatisch das Präfix `B2C_1A_` hinzugefügt.
   * Fügen Sie Ihren Anwendungsschlüssel in das Textfeld **Secret** (Geheimer Schlüssel) ein.
   * Wählen Sie **Signatur** aus.
1. Klicken Sie auf **Erstellen**.
1. Vergewissern Sie sich, dass Sie den Schlüssel `B2C_1A_ContosoAppSecret` erstellt haben.


## <a name="add-a-claims-provider-in-your-base-policy"></a>Hinzufügen eines Anspruchsanbieters in Ihrer Basisrichtlinie

Wenn Sie möchten, dass Benutzer sich mithilfe von Azure AD anmelden, müssen Sie Azure AD als Anspruchsanbieter definieren. Das heißt, Sie müssen einen Endpunkt festlegen, mit dem Azure AD B2C kommunizieren soll. Der Endpunkt bietet eine Reihe von Ansprüchen, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde. 

Sie können Azure AD als Anspruchsanbieter definieren, indem Sie Azure AD in der Erweiterungsdatei Ihrer Richtlinie im `<ClaimsProvider>`-Knoten hinzufügen:

1. Öffnen Sie die Erweiterungsdatei (TrustFrameworkExtensions.xml) aus Ihrem Arbeitsverzeichnis.
1. Suchen Sie nach dem Abschnitt `<ClaimsProviders>`. Wenn dieser nicht vorhanden sein sollte, fügen Sie ihn im Stammknoten hinzu.
1. Fügen Sie einen neuen `<ClaimsProvider>`-Knoten wie folgt ein:

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

1. Aktualisieren Sie im `<ClaimsProvider>`-Knoten den Wert für `<Domain>` in einen eindeutigen Wert, der eine Unterscheidung von anderen Identitätsanbietern ermöglicht.
1. Ändern Sie im Knoten `<ClaimsProvider>` den Wert für `<DisplayName>` in einen Anzeigenamen für den Anspruchsanbieter. Dieser Wert wird derzeit nicht verwendet.

### <a name="update-the-technical-profile"></a>Aktualisieren des technischen Profils

Um ein Token vom Azure AD-Endpunkt zu erhalten, müssen Sie die Protokolle definieren, die Azure AD B2C zur Kommunikation mit Azure AD verwenden soll. Dies erfolgt im Element `<TechnicalProfile>` von `<ClaimsProvider>`.
1. Aktualisieren Sie die ID des `<TechnicalProfile>`-Knotens. Diese ID wird als Verweis auf dieses technische Profil aus anderen Teilen der Richtlinie verwendet.
1. Aktualisieren Sie den Wert für `<DisplayName>`. Dieser Wert wird auf dem Anmeldebildschirm auf der Anmeldeschaltfläche angezeigt.
1. Aktualisieren Sie den Wert für `<Description>`.
1. Azure AD verwendet das OpenID Connect-Protokoll. Stellen Sie daher sicher, dass der Wert für `<Protocol>` `"OpenIdConnect"` lautet.

Sie müssen den Abschnitt `<Metadata>` im zuvor erwähnten XML-Dokument entsprechend den Konfigurationseinstellungen für Ihren speziellen Azure AD-Mandanten aktualisieren. Aktualisieren Sie wie folgt die Metadatenwerte in der XML-Datei:

1. Legen Sie `<Item Key="METADATA">` auf `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration` fest, wobei `yourAzureADtenant` der Name Ihres Azure AD-Mandanten (contoso.com) ist.
1. Öffnen Sie Ihren Browser, und rufen Sie die URL `METADATA` auf, die Sie gerade aktualisiert haben.
1. Suchen Sie im Browser nach dem Objekt „Aussteller“, und kopieren Sie dessen Wert. Es sollte in etwa wie folgt aussehen: `https://sts.windows.net/{tenantId}/`.
1. Fügen Sie den Wert für `<Item Key="ProviderName">` in die XML-Datei ein.
1. Legen Sie `<Item Key="client_id">` auf die Anwendungs-ID aus der App-Registrierung fest.
1. Legen Sie `<Item Key="IdTokenAudience">` auf die Anwendungs-ID aus der App-Registrierung fest.
1. Vergewissern Sie sich, dass `<Item Key="response_types">` auf `id_token` gesetzt ist.
1. Vergewissern Sie sich, dass `<Item Key="UsePolicyInRedirectUri">` auf `false` gesetzt ist.

Sie müssen außerdem das Azure AD-Geheimnis, das Sie in Ihrem Azure AD B2C-Mandanten registriert haben, mit den `<ClaimsProvider>`-Informationen von Azure AD verknüpfen:

* Aktualisieren Sie im Abschnitt `<CryptographicKeys>` des vorherigen XML-Dokuments den Wert für `StorageReferenceId` in die Referenz-ID des Geheimnisses (z.B. `ContosoAppSecret`), das Sie definiert haben.

### <a name="upload-the-extension-file-for-verification"></a>Hochladen der Erweiterungsdatei zur Überprüfung

Nun haben Sie Ihre Richtlinie so konfiguriert, dass Azure AD B2C mit Ihrem Azure AD-Verzeichnis kommunizieren kann. Versuchen Sie, die Erweiterungsdatei Ihrer Richtlinie hochzuladen, um sich zu vergewissern, dass soweit keine Probleme vorliegen. Gehen Sie dazu wie folgt vor:

1. Öffnen Sie in Ihrem Azure AD B2C-Mandanten das Blatt **Alle Richtlinien**.
1. Aktivieren Sie das Kontrollkästchen **Richtlinie überschreiben, sofern vorhanden**.
1. Laden Sie die Erweiterungsdatei (TrustFrameworkExtensions.xml) hoch, und stellen Sie sicher, dass die Validierung nicht fehlschlägt.

## <a name="register-the-azure-ad-claims-provider-to-a-user-journey"></a>Registrieren des Azure AD-Anspruchsanbieters bei einer User Journey

Nun müssen Sie den Azure AD-Identitätsanbieter zu einer Ihrer User Journeys hinzufügen. An dieser Stelle wurde der Identitätsanbieter eingerichtet, ist jedoch auf keinem der Registrierungs-/Anmeldebildschirme vorhanden. Zu diesem Zweck erstellen wir ein Duplikat einer vorhandenen User Journey-Vorlage und ändern diese, damit sie ebenfalls einen Azure AD-Identitätsanbieter aufweist:

1. Öffnen Sie die Basisdatei Ihrer Richtlinie (z.B. „TrustFrameworkBase.xml“).
1. Suchen das Element `<UserJourneys>`, und kopieren Sie den gesamten `<UserJourney>`-Knoten, der `Id="SignUpOrSignIn"` enthält.
1. Öffnen Sie die Erweiterungsdatei (z.B. „TrustFrameworkExtensions.xml“), und suchen Sie nach dem Element `<UserJourneys>`. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
1. Fügen Sie den gesamten `<UserJourney>`-Knoten ein, die Sie als untergeordnetes Element des `<UserJourneys>`-Elements kopiert haben.
1. Benennen Sie die ID der neuen User Journey (z.B. als `SignUpOrSignUsingContoso`).

### <a name="display-the-button"></a>Anzeigen der Schaltfläche

Das Element `<ClaimsProviderSelection>` entspricht einer Schaltfläche für einen Identitätsanbieter auf einem Registrierungs-/Anmeldebildschirm. Wenn Sie ein `<ClaimsProviderSelection>`-Element für Azure AD hinzufügen, wird eine neue Schaltfläche angezeigt, wenn ein Benutzer zu der Seite gelangt. So fügen Sie dieses Element hinzu:

1. Suchen Sie den `<OrchestrationStep>`-Knoten, der `Order="1"` in der User Journey enthält, die Sie gerade erstellt haben.
1. Fügen Sie Folgendes hinzu:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. Legen Sie `TargetClaimsExchangeId` als entsprechenden Wert fest. Es wird empfohlen, der allgemein üblichen Konvention zu folgen: *\[NameDesAnspruchsanbieters\]Exchange*.

### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion

Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. In diesem Fall soll bei der Aktion Azure AD B2C mit Azure AD kommunizieren, um ein Token zu empfangen. Dies gelingt Ihnen, indem Sie das technische Profil für Ihren Azure AD-Anspruchsanbieter verknüpfen:

1. Suchen Sie `<OrchestrationStep>`, das `Order="2"` im `<UserJourney>`-Knoten enthält.
1. Fügen Sie Folgendes hinzu:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

1. Aktualisieren Sie `Id` auf den gleichen Wert wie `TargetClaimsExchangeId` im vorherigen Abschnitt.
1. Ändern Sie `TechnicalProfileReferenceId` in die ID des technischen Profils, das Sie zuvor erstellt haben (ContosoProfile).

### <a name="upload-the-updated-extension-file"></a>Hochladen der aktualisierten Erweiterungsdatei

Die Änderung der Erweiterungsdatei ist abgeschlossen. Speichern Sie es. Laden Sie die Datei hoch, und stellen Sie sicher, dass alle Validierungen erfolgreich waren.

### <a name="update-the-rp-file"></a>Aktualisieren der Datei der vertrauenden Seite

Nun müssen Sie die Datei der vertrauenden Seite (Relying Party, RP) aktualisieren, die die gerade erstellte User Journey initiiert:

1. Erstellen Sie eine Kopie von der Datei „SignUpOrSignIn.xml“ in Ihrem Arbeitsverzeichnis, und benennen Sie sie um (z.B. „SignUpOrSignInWithAAD.xml“).
1. Öffnen Sie die neue Datei, und ändern Sie das `PolicyId`-Attribut für `<TrustFrameworkPolicy>` in einen eindeutigen Wert (z.B. SignUpOrSignInWithAAD). <br> Dies ist der Name Ihrer Richtlinie (z.B. B2C\_1A\_SignUpOrSignInWithAAD).
1. Ändern Sie das `ReferenceId`-Attribut in `<DefaultUserJourney>`, sodass es der ID der neuen User Journey, die Sie erstellt haben, entspricht (SignUpOrSignUsingContoso).
1. Speichern Sie die Änderungen, und laden Sie die Datei hoch.

## <a name="troubleshooting"></a>Problembehandlung

Testen Sie die benutzerdefinierte Richtlinie, die Sie soeben hochgeladen haben, indem Sie das zugehörige Blatt öffnen und dann auf **Jetzt ausführen** klicken. Lesen Sie den Artikel zur [Problembehandlung](active-directory-b2c-troubleshoot-custom.md), um mehr zum Diagnostizieren von Problemen zu erfahren.

## <a name="next-steps"></a>Nächste Schritte

Senden Sie uns Ihr Feedback an [AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).

