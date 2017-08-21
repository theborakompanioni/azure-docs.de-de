---
title: "Azure Active Directory B2C: Hinzufügen eines Salesforce-SAML-Anbieters mithilfe von benutzerdefinierten Richtlinien | Microsoft-Dokumentation"
description: In diesem Artikel erfahren Sie, wie Sie in Azure Active Directory B2C benutzerdefinierte Richtlinien erstellen und verwalten.
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: d7f4143f-cd7c-4939-91a8-231a4104dc2c
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 06/11/2017
ms.author: parakhj
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 269cbd80fb6e861fa8588025eec70b6c6e2890d7
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---
# <a name="azure-active-directory-b2c-sign-in-by-using-salesforce-accounts-via-saml"></a>Azure Active Directory B2C: Anmelden mit Salesforce-Konten per SAML

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In diesem Artikel erfahren Sie, wie Sie mit [benutzerdefinierter Richtlinien](active-directory-b2c-overview-custom.md) die Anmeldung für Benutzer von einer bestimmten Salesforce-Organisation aktivieren.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="azure-ad-b2c-setup"></a>Einrichtung von Azure AD B2C

Stellen Sie sicher, dass Sie alle Themen zu den [ersten Schritten mit benutzerdefinierten Richtlinien](active-directory-b2c-get-started-custom.md) in Azure Active Directory B2C (Azure AD B2C) abgeschlossen haben.

Diese umfassen:

* Erstellen eines Azure AD B2C-Mandanten
* Erstellen einer Azure AD B2C-Anwendung
* Registrieren von zwei Richtlinienmodulanwendungen
* Einrichten von Schlüsseln
* Einrichten des Starter Pack

### <a name="salesforce-setup"></a>Einrichtung von Salesforce

In diesem Artikel wird davon ausgegangen, dass Sie bereits Folgendes getan haben:

* Registrierung für ein Salesforce-Konto. Sie können sich für ein [kostenloses Developer Edition-Konto](https://developer.salesforce.com/signup) registrieren.
* [Einrichtung einer eigenen Domäne](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) für Ihre Salesforce-Organisation.

## <a name="set-up-salesforce-so-users-can-federate"></a>Einrichten von Salesforce, damit Benutzer einen Verbund bilden können

Damit Azure AD B2C mit Salesforce kommunizieren kann, müssen Sie die Salesforce-Metadaten-URL abrufen.

### <a name="set-up-salesforce-as-an-identity-provider"></a>Einrichten von Salesforce als Identitätsanbieter

> [!NOTE]
> In diesem Artikel wird davon ausgegangen, dass Sie die [Salesforce Lightning Experience](https://developer.salesforce.com/page/Lightning_Experience_FAQ) verwenden.

1. [Anmelden Sie sich bei Salesforce an](https://login.salesforce.com/). 
2. Erweitern Sie im Menü auf der linken Seite die Option **Einstellungen** und dann **Identität**, und klicken Sie dann auf **Identitätsanbieter**.
3. Klicken Sie auf **Enable Identity Provider** (Identitätsanbieter aktivieren).
4. Wählen Sie unter **Select the certificate** (Zertifikat auswählen) das Zertifikat aus, das Salesforce für die Kommunikation mit Azure AD B2C verwendet soll. (Sie können zwei Standardzertifikate verwenden.) Klicken Sie auf **Speichern**. 

### <a name="create-a-connected-app-in-salesforce"></a>Erstellen einer verbundenen App in Salesforce

1. Navigieren Sie auf der Seite **Identitätsanbieter** zu **Service Providers** (Dienstanbieter).
2. Klicken Sie auf **Service Providers are now created via Connected Apps (Dienstanbieter werden jetzt über verbundene Apps erstellt) Click here.** (Dienstanbieter werden jetzt mit verbundenen Apps erstellt. Hier klicken.).
3. Geben Sie unter **Grundlegende Informationen** die erforderlichen Werte für die verbundene App ein.
4. Aktivieren Sie unter **Web-App-Einstellungen** das Kontrollkästchen **SAML aktivieren**.
5. Geben Sie im Feld **Entitäts-ID** die folgende URL ein. Stellen Sie sicher, dass Sie den Wert für `tenantName` ersetzen.
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```
6. Geben Sie im Feld **ACS-URL** die folgende URL ein. Stellen Sie sicher, dass Sie den Wert für `tenantName` ersetzen.
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. Behalten Sie die Standardwerte für alle anderen Einstellungen bei.
8. Scrollen Sie zum Ende der Liste, und klicken Sie dann auf **Speichern**.

### <a name="get-the-metadata-url"></a>Abrufen der Metadaten-URL

1. Klicken Sie auf der Übersichtsseite der verbundenen App auf **Verwalten**.
2. Kopieren Sie den Wert für **Metadata Discovery Endpoint** (Metadaten-Ermittlungsendpunkt), und speichern Sie ihn. Sie werden ihn später in diesem Artikel brauchen.

### <a name="set-up-salesforce-users-to-federate"></a>Einrichten von Salesforce-Benutzern für einen Verbund

1. Navigieren Sie auf der Seite **Verwalten** der verbundenen App zu **Profile**.
2. Klicken Sie auf **Manage Profiles** (Profile verwalten).
3. Wählen Sie die Profile (oder Benutzergruppen) aus, für die ein Verbund mit Azure AD B2C hergestellt werden soll. Wählen Sie als Systemadministrator das Kontrollkästchen **Systemadministrator** aus, damit Sie den Verbund mit Ihrem Salesforce-Konto herstellen können.

## <a name="generate-a-signing-certificate-for-azure-ad-b2c"></a>Generieren Sie ein Signaturzertifikat für Azure AD B2C

An Salesforce gesendete Anforderungen müssen von Azure AD B2C signiert worden sein. Um ein Signaturzertifikat zu generieren, öffnen Sie Azure PowerShell, und führen Sie die folgenden Befehle.

> [!NOTE]
> Stellen Sie sicher, dass Sie den Namen des Mandanten und das Kennwort in den oberen beiden Zeilen aktualisieren.

```PowerShell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="add-the-saml-signing-certificate-to-azure-ad-b2c"></a>Hinzufügen des SAML-Signaturzertifikats zu Azure AD B2C

Laden Sie das Signaturzertifikat in Ihren Azure AD B2C-Mandanten hoch: 

1. Wechseln zu Ihrem Azure AD B2C-Mandanten. Klicken Sie auf **Einstellungen** > **Framework für die Identitätsfunktion** > **Richtlinienschlüssel**.
2. Klicken Sie auf **+ Hinzufügen**, und klicken Sie dann aus:
    1. Klicken Sie auf **Optionen** > **Hochladen**.
    2. Geben Sie einen **Namen** ein (z.B. SAMLSigningCert). Dem Namen Ihres Schlüssels wird automatisch das Präfix *B2C_1A_* hinzugefügt.
    3. Um Ihr Zertifikat auszuwählen, wählen **upload file control** (Dateisteuerelement hochladen) aus. 
    4. Geben Sie das Kennwort des Zertifikats ein, das Sie im PowerShell-Skript festlegen.
3. Klicken Sie auf **Erstellen**.
4. Überprüfen Sie, ob Sie einen Schlüssel (z.B. B2C_1A_SAMLSigningCert) erstellt haben. Notieren Sie den vollständigen Namen (einschließlich *B2C_1A_*). Sie werden auf diesen Schlüssel später in der Richtlinie Bezug nehmen.

## <a name="create-the-salesforce-saml-claims-provider-in-your-base-policy"></a>Erstellen des Salesforce-SAML-Anspruchsanbieters in Ihrer Basisrichtlinie

Sie müssen Salesforce als Anspruchsanbieter definieren, damit sich Benutzer mithilfe von Salesforce anmelden können. Anders ausgedrückt: Sie müssen den Endpunkt festlegen, mit dem Azure AD B2C kommunizieren soll. Der Endpunkt *bietet* eine Reihe von *Ansprüchen*, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde. Hierfür können Sie für Salesforce `<ClaimsProvider>` in der Erweiterungsdatei Ihrer Richtlinie hinzufügen:

1. Öffnen Sie die Erweiterungsdatei aus Ihrem Arbeitsverzeichnis (TrustFrameworkExtensions.xml).
2. Suchen Sie nach dem Abschnitt `<ClaimsProviders>`. Wenn dieser nicht vorhanden sein sollte, erstellen Sie ihn im Stammknoten.
3. Fügen Sie einen neuen `<ClaimsProvider>` hinzu:

    ```XML
    <ClaimsProvider>
      <Domain>salesforce</Domain>
      <DisplayName>Salesforce</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="salesforce">
          <DisplayName>Salesforce</DisplayName>
          <Description>Login with your Salesforce account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="RequestsSigned">false</Item>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="WantsSignedAssertions">false</Item>
            <Item Key="PartnerEntity">https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp.xml</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userId"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="username"/>
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

Im Knoten `<ClaimsProvider>`:

1. Ändern Sie den Wert für `<Domain>` in einen eindeutigen Wert, mit dem `<ClaimsProvider>` von anderen Identitätsanbietern unterschieden wird.
2. Aktualisieren Sie den Wert für `<DisplayName>` in einen Anzeigenamen für den Anspruchsanbieter. Derzeit wird dieser Wert nicht verwendet.

### <a name="update-the-technical-profile"></a>Aktualisieren des technischen Profils

Um ein SAML-Token von Salesforce zu erhalten, müssen Sie die Protokolle definieren, die Azure AD B2C zur Kommunikation mit Azure Active Directory (Azure AD) verwenden soll. Führen Sie dies im Element `<TechnicalProfile>` von `<ClaimsProvider>` aus:

1. Aktualisieren Sie die ID des Knotens `<TechnicalProfile>`. Diese ID wird als Verweis auf dieses technische Profil aus anderen Teilen der Richtlinie verwendet.
2. Aktualisieren Sie den Wert für `<DisplayName>`. Dieser Wert wird auf die Anmeldeschaltfläche auf der Anmeldeseite angezeigt.
3. Aktualisieren Sie den Wert für `<Description>`.
4. Salesforce verwendet das SAML 2.0-Protokoll. Stellen Sie sicher, dass der Wert für `<Protocol>` **SAML2** lautet.

Aktualisieren Sie den Abschnitt `<Metadata>` im vorangehenden XML entsprechend der Einstellungen für Ihre spezifischen Salesforce-Konto. Aktualisieren Sie die Metadatenwerte wie folgt im XML-Dokument :

1. Aktualisieren Sie den Wert `<Item Key="PartnerEntity">` mit der Salesforce-Metadaten-URL, die Sie zuvor kopiert haben. Sie hat folgendes Format: 

    `https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp/connectedapp.xml`

2. Aktualisieren Sie im Abschnitt `<CryptographicKeys>` den Wert für beide Instanzen von `StorageReferenceId` auf den Namen des Schlüssels des Signaturzertifikats (z.B. B2C_1A_SAMLSigningCert).

### <a name="upload-the-extension-file-for-verification"></a>Hochladen der Erweiterungsdatei zur Überprüfung

Ihre Richtlinie ist jetzt konfiguriert, sodass Azure AD B2C mit Salesforce kommunizieren kann. Versuchen Sie, die Erweiterungsdatei Ihrer Richtlinie hochzuladen, um sicherzustellen, dass bisher keine Probleme bestehen. So laden Sie die Erweiterungsdatei Ihrer Richtlinie hoch:

1. Navigieren Sie in Ihrem Azure AD B2C-Mandanten zum Blatt **Alle Richtlinien**.
2. Aktivieren Sie das Kontrollkästchen **Richtlinie überschreiben, sofern vorhanden**.
3. Laden Sie die Erweiterungsdatei hoch (TrustFrameworkExtensions.xml). Stellen Sie sicher, dass die Validierung nicht fehlschlägt.

## <a name="register-the-salesforce-saml-claims-provider-to-a-user-journey"></a>Registrieren des Salesforce-SAML-Anspruchsanbieters auf eine User Journey

Fügen Sie nun einer Ihrer User Journeys den Salesforce-SAML-Identitätsanbieter hinzu. Nun ist der Identitätsanbieter zwar eingerichtet, ist jedoch auf keiner der Registrierungs- oder Anmeldeseiten für Benutzer vorhanden. Um einer Anmeldeseite den Identitätsanbieter hinzuzufügen, erstellen Sie zunächst ein Duplikat einer vorhandenen Vorlagen-User Journey. Ändern Sie dann die Vorlage, sodass sie auch den Azure AD-Identitätsanbieter hat.

1. Öffnen Sie die Basisdatei Ihrer Richtlinie (z.B. „TrustFrameworkBase.xml“).
2. Suchen Sie das Element `<UserJourneys>`, und kopieren Sie dann die gesamte Wert `<UserJourney>`, einschließlich Id=”SignUpOrSignIn”.
3. Öffnen Sie die Erweiterungsdatei (z.B. TrustFrameworkExtensions.xml). Suchen Sie das Element `<UserJourneys>`. Wenn das Element nicht vorhanden ist, fügen Sie es hinzu.
4. Fügen Sie die gesamte kopierte `<UserJourney>` als untergeordnetes Element des Elements `<UserJourneys>` ein.
5. Benennen Sie die ID der neuen `<UserJourney>` um (z.B. SignUpOrSignUsingContoso).

### <a name="display-the-identity-provider-button"></a>Anzeigen der Schaltfläche für einen Identitätsanbieter

Das Element `<ClaimsProviderSelection>` entspricht einer Schaltfläche für einen Identitätsanbieter auf einer Registrierungs- oder Anmeldeseite. Durch Hinzufügen eines `<ClaimsProviderSelection>`-Elements für Salesforce wird eine neue Schaltfläche angezeigt, wenn ein Benutzer diese Seite besucht. So zeigen Sie die Schaltfläche für einen Identitätsanbieter an:

1. Suchen Sie in der `<UserJourney>`, die Sie erstellt haben, `<OrchestrationStep>` mit `Order="1"`.
2. Fügen Sie folgenden XML-Code hinzu:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

3. Legen Sie `TargetClaimsExchangeId` auf einen logischen Wert fest. Es wird empfohlen, der allgemein üblichen Konvention zu folgen: *\[NameDesAnspruchsanbieters\]Exchange*.

### <a name="link-the-identity-provider-button-to-an-action"></a>Verknüpfen der Schaltfläche für einen Identitätsanbieter mit einer Aktion

Nun, da Sie eine Schaltfläche für einen Identitätsanbieter haben, verknüpfen Sie sie mit einer Aktion. In diesem Fall besteht die Aktion darin, dass Azure AD B2C mit Salesforce kommuniziert, um ein SAML-Token zu empfangen. Verknüpfen Sie hierfür das technische Profil für Ihren Salesforce-SAML-Anspruchsanbieter:

1. Suchen Sie im Knoten `<UserJourney>` mit `Order="2"` nach `<OrchestrationStep>`.
2. Fügen Sie folgenden XML-Code hinzu:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

3. Aktualisieren Sie `Id` auf denselben Wert, den Sie zuvor für `TargetClaimsExchangeId` verwendet haben.
4. Aktualisieren Sie `TechnicalProfileReferenceId` in die `Id` des technischen Profils, das Sie zuvor erstellt haben (z.B. „ContosoProfile“).

### <a name="upload-the-updated-extension-file"></a>Hochladen der aktualisierten Erweiterungsdatei

Die Änderung der Erweiterungsdatei ist abgeschlossen. Speichern Sie diese Datei, und laden Sie sie hoch. Stellen Sie sicher, dass alle Überprüfungen erfolgreich waren.

### <a name="update-the-relying-party-file"></a>Aktualisieren der Datei der vertrauenden Seite

Aktualisieren Sie als Nächstes die Datei der vertrauenden Seite (Relying Party, RP), die die User Journey initiiert, die Sie erstellt haben:

1. Erstellen Sie in Ihrem Arbeitsverzeichnis eine Kopie von „SignUpOrSignIn.xml“. Benennen Sie es dann um (z.B. in „SignUpOrSignInWithAAD.xml“).
2. Öffnen Sie die neue Datei, und ändern Sie das `PolicyId`-Attribut für `<TrustFrameworkPolicy>` in einen eindeutigen Wert. Dies ist der Name der Richtlinie (z.B. SignUpOrSignInWithAAD).
3. Ändern Sie das `ReferenceId`-Attribut in `<DefaultUserJourney>`, sodass es der `Id` der neuen User Journey, die Sie erstellt haben, entspricht (z.B. „SignUpOrSignUsingContoso“).
4. Speichern Sie die Änderungen, und laden Sie die Datei dann hoch.

## <a name="test-and-troubleshoot"></a>Testen und Problembehandlung

Um die benutzerdefinierte Richtlinie zu testen, die Sie soeben hochgeladen haben, wechseln Sie im Azure-Portal zum Richtlinienblatt, und klicken Sie dann auf **Jetzt ausführen**. Bei Fehlern finden Sie Informationen unter [Problembehandlung benutzerdefinierter Richtlinien](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Nächste Schritte

Senden Sie uns Ihr Feedback an [AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).

