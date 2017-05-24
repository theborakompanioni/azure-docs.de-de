---
title: "Azure Active Directory B2C: Hinzufügen von Salesforce-SAML-Anbietern mithilfe von benutzerdefinierten Richtlinien | Microsoft-Dokumentation"
description: Thema zu benutzerdefinierten Azure Active Directory B2C-Richtlinien
services: active-directory-b2c
documentationcenter: 
author: gsacavdm
manager: krassk
editor: gsacavdm
ms.assetid: d7f4143f-cd7c-4939-91a8-231a4104dc2c
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/30/2017
ms.author: gsacavdm
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 1d97c75f3130ea6fdacbc6335b6e70677b4d226e
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="azure-active-directory-b2c-log-in-using-salesforce-accounts-via-saml"></a>Azure Active Directory B2C: Anmelden mit Salesforce-Konten per SAML

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel erfahren Sie, wie Sie die Anmeldung für Benutzer von einer bestimmten Salesforce-Organisation mittels [benutzerdefinierter Richtlinien](active-directory-b2c-overview-custom.md) aktivieren.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="azure-ad-b2c-setup"></a>Azure AD B2C-Setup
Stellen Sie sicher, dass Sie alle Schritte unter [Erste Schritte mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) durchgeführt haben.

Dies umfasst:

1. Erstellen eines Azure AD B2C-Mandanten
1. Erstellen einer Azure AD B2C-Anwendung
1. Registrieren von zwei Richtlinienmodulanwendungen
1. Einrichten von Schlüsseln
1. Einrichten des Starter Pack

### <a name="salesforce-setup"></a>Salesforce-Setup
In diesem Tutorial wird vorausgesetzt, dass Sie bereits über Folgendes verfügen bzw. folgende Schritte ausgeführt haben:
1. Registrierung für ein Salesforce-Konto. Sie können sich für eine [kostenlose Developer Edition](https://developer.salesforce.com/signup) registrieren. 
1. [Einrichtung einer eigenen Domäne](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) für Ihre Salesforce-Organisation

## <a name="get-the-salesforce-saml-metadata"></a>Abrufen der Salesforce-SAML-Metadaten
>[!NOTE]
> In diesem Tutorial wird vorausgesetzt, dass Sie die [Salesforce Lighting Experience](https://developer.salesforce.com/page/Lightning_Experience_FAQ) verwenden.

1. [Anmeldung an Salesforce](https://login.salesforce.com/) 
1. Erweitern Sie im Menü auf der linken Seite die Option **Einstellungen** und dann **Identität**, und klicken Sie auf **Identitätsanbieter**.
1. Klicken Sie auf **Enable Identity Provider** (Identitätsanbieter aktivieren).
1. **Wählen Sie das Zertifikat aus**, das von Salesforce für die Kommunikation mit Azure AD B2C verwendet werden soll, und klicken Sie auf **Speichern**. Sie können das Standardzertifikat verwenden.
1. Klicken Sie auf die Schaltfläche **Download Metadata** (Metadaten herunterladen), die jetzt verfügbar ist, und speichern Sie die Datei mit den Metadaten, da Sie sie in einem späteren Schritt benötigen.

## <a name="add-a-saml-signing-certificate-to-azure-ad-b2c"></a>Hinzufügen eines SAML-Signaturzertifikats zu Azure AD B2C
Sie müssen ein SAML-Zertifikat in Ihren Azure AD B2C-Mandanten hochladen, das zum Signieren der SAML-Anforderungen verwendet werden soll. Gehen Sie dazu folgendermaßen vor:

1. Navigieren Sie zu Ihrem Azure AD B2C-Mandanten, und öffnen Sie **B2C-Einstellungen > Identity Experience Framework > Policy Keys** (Richtlinienschlüssel).
1. Klicken Sie auf **+ Hinzufügen**.
1. Optionen:
 * Wählen Sie **Optionen > Hochladen**.
 * **Name**: > `ContosoIdpSamlCert`.  Dem Namen Ihres Schlüssels wird automatisch das Präfix „B2C_1A_“ hinzugefügt. Notieren Sie den vollständigen Namen (mit „B2C_1A_“), da Sie später in der Richtlinie darauf verweisen.
 * Wählen Sie mit dem Steuerelement zum Hochladen von Dateien**** Ihr Zertifikat aus, und geben Sie ggf. das Kennwort für das Zertifikat an.
1. Klicken Sie auf **Erstellen**
1. Bestätigen, dass der Schlüssel erstellt wurde: `B2C_1A_ContosoIdpSamlCert`

## <a name="create-the-salesforce-saml-claims-provider-in-your-base-policy"></a>Erstellen des Salesforce-SAML-Anspruchsanbieters in Ihrer Basisrichtlinie

Damit sich Benutzer mithilfe von Salesforce anmelden können, müssen Sie Salesforce als Anspruchsanbieter definieren. Anders ausgedrückt: Sie müssen den Endpunkt festlegen, mit dem Azure AD B2C kommunizieren soll. Der Endpunkt *bietet* eine Reihe von *Ansprüchen*, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde. Hierfür können Sie für Salesforce `<ClaimsProvider>` in der Erweiterungsdatei Ihrer Richtlinie hinzufügen.

1. Öffnen Sie die Erweiterungsdatei aus Ihrem Arbeitsverzeichnis (TrustFrameworkExtensions.xml).
1. Suchen Sie nach dem Abschnitt `<ClaimsProviders>`. Wenn dieser nicht vorhanden sein sollte, fügen Sie ihn im Stammknoten hinzu.
1. Fügen Sie einen neuen `<ClaimsProvider>` hinzu, wie nachfolgend beschrieben:

    ```XML
    <ClaimsProvider>
      <Domain>contoso</Domain>
      <DisplayName>Contoso</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Contoso">
          <DisplayName>Contoso</DisplayName>
          <Description>Login with your Contoso account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="RequestsSigned">false</Item>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="PartnerEntity">
    <![CDATA[ <md:EntityDescriptor xmlns:md="urn:oasis:names:tc:SAML:2.0:metadata" entityID="https://contoso.com" validUntil="2026-10-05T23:57:13.854Z" xmlns:ds="http://www.w3.org/2000/09/xmldsig#"><md:IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol"><md:KeyDescriptor use="signing"><ds:KeyInfo><ds:X509Data><ds:X509Certificate>MIIErDCCA….qY9SjVXdu7zy8tZ+LqnwFSYIJ4VkE9UR1vvvnzO</ds:X509Certificate></ds:X509Data></ds:KeyInfo></md:KeyDescriptor><md:NameIDFormat>urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified</md:NameIDFormat><md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location="https://contoso.com/idp/endpoint/HttpPost"/><md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://contoso.com/idp/endpoint/HttpRedirect"/></md:IDPSSODescriptor></md:EntityDescriptor>]]>
            </Item>
          </Metadata>       
          <CryptographicKeys>
            <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_ContosoIdpSamlCert"/>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ContosoIdpSamlCert "/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userId"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="externalIdp"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="SAMLIdp" />
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

Um ein SAML-Token von Salesforce zu erhalten, müssen Sie die Protokolle definieren, die Azure AD B2C zur Kommunikation mit Azure AD verwenden soll. Dies erfolgt im Element `<TechnicalProfile>` von `<ClaimsProvider>`.

1. Aktualisieren Sie die ID des Knotens `<TechnicalProfile>`. Diese ID wird als Verweis auf dieses technische Profil aus anderen Teilen der Richtlinie verwendet.
1. Aktualisieren Sie den Wert für `<DisplayName>`. Dieser Wert wird auf der Anmeldeschaltfläche auf dem Anmeldebildschirm angezeigt.
1. Aktualisieren Sie den Wert für `<Description>`.
1. Azure AD verwendet das OpenID Connect-Protokoll. Stellen Sie daher sicher, dass der Wert für `<Protocol>` „SAML2“ lautet.

Sie müssen den Abschnitt `<Metadata>` im obigen XML-Dokument entsprechend den Konfigurationseinstellungen für Ihren speziellen Azure AD-Mandanten aktualisieren. Aktualisieren Sie wie folgt die Metadatenwerte im XML-Dokument:

1. Aktualisieren Sie den Wert von `<Item Key="PartnerEntity">` mit dem Inhalt der Datei „Metadata.xml“, die Sie von Salesforce heruntergeladen haben. **Kapseln Sie dies mit <![CDATA[ …metadata… ]]>**.

1. Aktualisieren Sie im Abschnitt `<CryptographicKeys>` des obigen XML-Codes den Wert für `StorageReferenceId` jeweils auf die von Ihnen definierte Zertifikat-ID (z.B. ContosoSalesforceCert).

### <a name="upload-the-extension-file-for-verification"></a>Hochladen der Erweiterungsdatei zur Überprüfung

Nun haben Sie Ihre Richtlinie so konfiguriert, dass Azure AD B2C mit Ihrem Azure AD-Verzeichnis kommunizieren kann. Versuchen Sie, die Erweiterungsdatei Ihrer Richtlinie hochzuladen, um sich zu vergewissern, dass soweit keine Probleme vorliegen. Gehen Sie dazu wie folgt vor:

1. Navigieren Sie in Ihrem Azure AD B2C-Mandanten zum Blatt **Alle Richtlinien**.
1. Aktivieren Sie das Kontrollkästchen **Richtlinie überschreiben, sofern vorhanden**.
1. Laden Sie die Erweiterungsdatei (TrustFrameworkExtensions.xml) hoch, und stellen Sie sicher, dass die Validierung nicht fehlschlägt.

## <a name="register-the-salesforce-saml-claims-provider-to-a-user-journey"></a>Registrieren des Salesforce-SAML-Anspruchsanbieters für eine User Journey

Nun müssen Sie den Salesforce-SAML-Identitätsanbieter einer Ihrer User Journeys hinzufügen. Der Identitätsanbieter wurde nun eingerichtet, aber er ist auf keinem der Registrierungs-/Anmeldebildschirme vorhanden. Zu diesem Zweck erstellen wir ein Duplikat einer vorhandenen User Journey-Vorlage und ändern diese, damit sie ebenfalls einen Azure AD-Identitätsanbieter aufweist.

1. Öffnen Sie die Basisdatei Ihrer Richtlinie (z.B. „TrustFrameworkBase.xml“).
1. Suchen Sie nach dem Element `<UserJourneys>`, und kopieren Sie die gesamte `<UserJourney>` mit der ID „SignUpOrSignIn“.
1. Öffnen Sie die Erweiterungsdatei (z.B. „TrustFrameworkExtensions.xml“), und suchen Sie nach dem Element `<UserJourneys>`. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
1. Fügen Sie die gesamte `<UserJourney>` ein, die Sie als untergeordnetes Element des `<UserJourneys>`-Elements kopiert haben.
1. Benennen Sie die ID der neuen `<UserJourney>` um (d.h. „SignUpOrSignUsingContoso“).

### <a name="display-the-button"></a>Anzeigen der Schaltfläche

Das Element `<ClaimsProviderSelection>` entspricht einer Schaltfläche für einen Identitätsanbieter auf einem Registrierungs-/Anmeldebildschirm. Durch Hinzufügen eines `<ClaimsProviderSelection>`-Elements für Salesforce wird eine neue Schaltfläche angezeigt, wenn ein Benutzer auf die Seite gelangt. Gehen Sie dazu folgendermaßen vor:

1. Suchen Sie nach `<OrchestrationStep>` mit `Order="1"` in der `<UserJourney>`, die Sie gerade erstellt haben.
1. Fügen Sie Folgendes hinzu:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. Legen Sie `TargetClaimsExchangeId` als entsprechenden Wert fest. Es wird empfohlen, derselben Konvention wie anderen zu folgen –  *\[ClaimProviderName\]Exchange*.

### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion

Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. In diesem Fall soll Azure AD B2C bei der Aktion mit Salesforce kommunizieren, um ein SAML-Token zu empfangen. Verknüpfen Sie hierfür das technische Profil für Ihren Salesforce-SAML-Anspruchsanbieter.

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
1. Öffnen Sie die neue Datei, und ändern Sie das `PolicyId`-Attribut für `<TrustFrameworkPolicy>` in einen eindeutigen Wert. Dies ist der Name Ihrer Richtlinie (z.B. „SignUpOrSignInWithAAD“).
1. Ändern Sie das `ReferenceId`-Attribut in `<DefaultUserJourney>`, sodass es der ID der neuen User Journey, die Sie erstellt haben, entspricht (z.B. „SignUpOrSignUsingContoso“).
1. Speichern Sie die Änderungen, und laden Sie die Datei hoch.

## <a name="create-a-connected-app-in-salesforce"></a>Erstellen einer verbundenen App in Salesforce
Sie müssen Azure AD B2C in Salesforce als verbundene App („Connected App“) registrieren.

1. [Anmeldung an Salesforce](https://login.salesforce.com/) 
1. Erweitern Sie im Menü auf der linken Seite die Option **Einstellungen** und dann **Identität**, und klicken Sie auf **Identitätsanbieter**.
1. Klicken Sie unten im Abschnitt **Service Providers** (Dienstanbieter) auf **Service Providers are now created via Connected Apps. Click here.** (Dienstanbieter werden jetzt mit verbundenen Apps erstellt. Hier klicken.).
1. Geben Sie die erforderlichen **grundlegenden Informationen** für Ihre verbundene App an.
1. Gehen Sie im Abschnitt **Web-App-Einstellungen** jetzt wie folgt vor:
    1. Aktivieren Sie die Option **SAML aktivieren**.
    1. Geben Sie die folgende URL im Feld **Entitäts-ID** ein, und ersetzen Sie `tenantName`. 
    
        ```
        https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase
        ```

    1. Geben Sie die folgende URL im Feld **ACS-URL** ein, und ersetzen Sie `tenantName`. 
        ```
        https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
        ```

    1. Behalten Sie für alle anderen Einstellungen die Standardwerte bei.
1. Führen Sie einen Bildlauf ganz nach unten durch, und klicken Sie auf die Schaltfläche **Speichern**.


## <a name="troubleshooting"></a>Problembehandlung

Testen Sie die benutzerdefinierte Richtlinie, die Sie soeben hochgeladen haben, indem Sie das zugehörige Blatt öffnen und dann auf „Jetzt ausführen“ klicken. Informationen zum Umgang mit etwaigen auftretenden Fehlern finden Sie unter [Problembehandlung](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Nächste Schritte
 
Senden Sie uns Ihr Feedback an [AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).


