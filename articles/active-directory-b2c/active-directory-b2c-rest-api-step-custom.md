---
title: 'Exemplarische Vorgehensweise: REST-API-Anspruchsaustausch als Schritt in Ihren benutzerdefinierten B2C-Richtlinien| Microsoft-Dokumentation'
description: Thema zu benutzerdefinierten Azure Active Directory B2C-Richtlinien und zur API-Integration
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
ms.date: 04/24/2017
ms.author: joroja
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 27d066cc6f985565a765989837e4a2744387a628
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017

---

# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journeys-as-an-orchestration-step"></a>Exemplarische Vorgehensweise: Integrieren von REST-API-Anspruchsaustausch-Vorgängen in Ihre Azure AD B2C User Journeys als Orchestrierungsschritt

Das **Identity Experience Framework** (IEF), das Azure AD B2C zugrunde liegt, ermöglicht dem Identitätsentwickler das Integrieren einer Interaktion mit einer RESTful-API in eine User Journey.  

Am Ende dieser exemplarischen Vorgehensweise sind Sie in der Lage, Azure AD B2C User Journeys zu erstellen, die mit RESTful-Diensten interagieren.

Das IEF sendet Daten in Ansprüchen und erhält Daten in Ansprüchen zurück.  Der REST-API-Anspruchsaustausch kann als Orchestrierungsschritt entworfen werden.

- Hierbei kann eine externe Aktion ausgelöst werden. Beispielsweise kann ein Ereignis in einer externen Datenbank protokolliert werden.
- Außerdem kann ein Wert abgerufen werden, der dann in der Benutzerdatenbank gespeichert wird.
- Die empfangenen Ansprüche können später zum Ändern des Ausführungsablaufs verwendet werden.

Die Interaktion kann auch als Überprüfungsprofil entworfen werden. Weitere Informationen hierzu finden Sie unter [Exemplarische Vorgehensweise: Integrieren von REST-API-Anspruchsaustausch-Vorgängen in Ihre Azure AD B2C User Journeys als Validierung der Benutzereingabe](active-directory-b2c-rest-api-validation-custom.md).

Das Szenario ist wie folgt: Wenn ein Benutzer eine Profilbearbeitung durchführt, soll in einem externen System nach dem Benutzer gesucht werden, der Ort, an dem der Benutzer gemeldet ist, soll abgerufen werden, und anschließend soll das Attribut als Anspruch an die Anwendung zurückgegeben werden.

## <a name="prerequisites"></a>Voraussetzungen

- Konfiguration eines Azure AD B2C-Mandanten zur Durchführung der Registrierung/Anmeldung für ein lokales Konto wie unter [Erste Schritte](active-directory-b2c-get-started-custom.md) beschrieben.
- Ein REST-API-Endpunkt für die Interaktion (in dieser exemplarischen Vorgehensweise wird ein sehr einfaches WebHook-Element einer Azure-Funktionen-App als Beispiel verwendet)
- **Empfohlen**: Führen Sie den [Schritt „Exemplarische Vorgehensweise: Integrieren von REST-API-Anspruchsaustausch-Vorgängen in Ihre Azure AD B2C User Journeys als Validierung der Benutzereingabe“](active-directory-b2c-rest-api-validation-custom.md) aus.

## <a name="step-1---prepare-the-rest-api-function"></a>Schritt 1: Vorbereiten der REST-API-Funktion

> [!NOTE]
> Die Einrichtung von REST-API-Funktionen wird in diesem Artikel nicht beschrieben. Für [Azure-Funktionen-Apps](https://docs.microsoft.com/azure/azure-functions/functions-reference) gibt es ein hervorragendes Toolkit für die Erstellung von RESTful-Diensten in der Cloud.

Wir haben eine Azure-Funktion eingerichtet, die den Anspruch `email` empfängt und dann einfach den Anspruch `city` mit dem zugewiesenen Wert `Redmond` zurückgibt. Die Azure-Beispielfunktion finden Sie hier: [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

Der von der Azure-Funktion zurückgegebene `userMessage`-Anspruch ist in diesem Kontext optional und wird vom IEF ignoriert.  Er kann ggf. als Meldung verwendet werden, die an die Anwendung übergeben und dem Benutzer später angezeigt wird.

```csharp
if (requestContentAsJObject.email == null)
{
    return request.CreateResponse(HttpStatusCode.BadRequest);
}

var email = ((string) requestContentAsJObject.email).ToLower();

return request.CreateResponse<ResponseContent>(
    HttpStatusCode.OK,
    new ResponseContent
    {
        version = "1.0.0",
        status = (int) HttpStatusCode.OK,
        userMessage = "User Found",
        city = "Redmond"
    },
    new JsonMediaTypeFormatter(),
    "application/json");
```

Mit **Azure-Funktionen-Apps** ist es einfach, die Funktions-URL abzurufen. Darin ist auch der Bezeichner der spezifischen Funktion enthalten.  In diesem Fall lautet die URL: „https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==“. Sie können sie für Testzwecke verwenden.

## <a name="step-2---configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworextensionsxml-file"></a>Schritt 2: Konfigurieren des RESTful-API-Anspruchsaustauschs als technisches Profil in der Datei „TrustFrameworkExtensions.xml“

Ein technisches Profil umfasst die vollständige Konfiguration des Austauschs, der für den RESTful-Dienst gewünscht ist. Öffnen Sie die Datei `TrustFrameworkExtensions.xml`, und fügen Sie im `<ClaimsProvider>`-Element den XML-Codeausschnitt hinzu.

> [!NOTE]
> Betrachten Sie den „Restful Provider, Version 1.0.0.0“, der unten als Protokoll angegeben ist, als die Funktion, die mit dem externen Dienst interagiert.  Eine vollständige Definition des Schemas finden Sie unter <!-- TODO: Link to RESTful Provider schema definition>-->.

```XML
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-LookUpLoyaltyWebHook">
            <DisplayName>Check LookUpLoyalty Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==</Item>
                <Item Key="AuthenticationType">None</Item>
                <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="email" />
            </InputClaims>
            <OutputClaims>
                <OutputClaim ClaimTypeReferenceId="city" PartnerClaimType="city" />
            </OutputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

Das `<InputClaims>`-Element definiert die Ansprüche, die vom IEF zum REST-Dienst gesendet werden. Im obigen Beispiel wird der Inhalt des `givenName`-Anspruchs als `email`-Anspruch an den REST-Dienst gesendet.  

Das `<OutputClaims>`-Element definiert die Ansprüche, die das IEF vom REST-Dienst erwartet. Unabhängig von der empfangenen Anzahl von Ansprüchen werden vom IEF nur die hier identifizierten Ansprüche verwendet. In diesem Beispiel wird ein Anspruch, der als `city` empfangen wird, dem IEF-Anspruch `city` zugeordnet.

## <a name="step-3---add-a-new-claim-city-to-the-schema-of-your-trustframeworkextensionsxml-file"></a>Schritt 3: Hinzufügen eines neuen `city`-Anspruchs zum Schema der Datei „TrustFrameworkExtensions.xml“

Der `city`-Anspruch ist in unserem Schema nicht anderweitig definiert. Wir fügen daher im `<BuildingBlocks>`-Element, das sich am Anfang der Datei `TrustFrameworkExtensions.xml` befindet, eine Definition hinzu.

```XML
<BuildingBlocks>
    <!--The claimtype city must be added to the TrustFrameworkPolicy-->
    <!-- You can add new claims in the BASE file Section III, or in the extensions file-->
    <ClaimsSchema>
        <ClaimType Id="city">
            <DisplayName>City</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Your city</UserHelpText>
            <UserInputType>TextBox</UserInputType>
        </ClaimType>
    </ClaimsSchema>
</BuildingBlocks>
```

## <a name="step-4---include-the-rest-service-claims-exchange-as-an-orchestration-step-in-your-profile-edit-user-journey-in-your-trustframeworkextensionsxml"></a>Schritt 4: Einfügen des REST-Dienst-Anspruchsaustauschs als Orchestrierungsschritt in die User Journey der Profilbearbeitung in der Datei „TrustFrameworkExtensions.xml“

Wir haben uns entschieden, den Schritt für die User Journey der Profilbearbeitung hinzuzufügen, nachdem sich der Benutzer authentifiziert (Orchestrierungsschritte 1 bis 4 weiter unten) und die aktualisierten Profilinformationen angegeben hat (Schritt 5).

> [!NOTE]
> Es gibt viele Anwendungsfälle, in denen der REST-API-Aufruf als Orchestrierungsschritt verwendet werden kann.  Als Orchestrierungsschritt kann er für die Aktualisierung eines externen Systems verwendet werden, nachdem ein Benutzer eine Aufgabe wie die erstmalige Registrierung oder die Profilaktualisierung durchgeführt hat, um die Informationen synchron zu halten.  In diesem Fall wird er zum Erweitern der Informationen verwendet, die für die Anwendung nach der Profilbearbeitung bereitgestellt werden.

Kopieren Sie den XML-Code für die User Journey der Profilbearbeitung aus der Datei `TrustFrameworkBase.xml` in das `<UserJourneys>`-Element der Datei `TrustFrameworkExtensions.xml`, und nehmen Sie anschließend die Änderung unter Schritt 6 vor.

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
    <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
    </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Falls die Reihenfolge nicht mit Ihrer Version übereinstimmt, müssen Sie sicherstellen, dass Sie als Schritt vor dem ClaimsExchange-Typ `SendClaims` einfügen.

Der endgültige UserJourney-XML-Code sollte wie folgt aussehen:

```XML
<UserJourney Id="ProfileEdit">
    <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsProviderSelection" ContentDefinitionReferenceId="api.idpselections">
            <ClaimsProviderSelections>
                <ClaimsProviderSelection TargetClaimsExchangeId="FacebookExchange" />
                <ClaimsProviderSelection TargetClaimsExchangeId="LocalAccountSigninEmailExchange" />
            </ClaimsProviderSelections>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
                <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="ClaimsExchange">
            <Preconditions>
                <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
                    <Value>authenticationSource</Value>
                    <Value>localAccountAuthentication</Value>
                    <Action>SkipThisOrchestrationStep</Action>
                </Precondition>
            </Preconditions>
            <ClaimsExchanges>
                <ClaimsExchange Id="AADUserRead" TechnicalProfileReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="4" Type="ClaimsExchange">
            <Preconditions>
                <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
                    <Value>authenticationSource</Value>
                    <Value>socialIdpAuthentication</Value>
                    <Action>SkipThisOrchestrationStep</Action>
                </Precondition>
            </Preconditions>
            <ClaimsExchanges>
                <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="5" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="B2CUserProfileUpdateExchange" TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <!-- Add a step 6 to the user journey before the jwt token is created-->
        <OrchestrationStep Order="6" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
    </OrchestrationSteps>
    <ClientDefinition ReferenceId="DefaultWeb" />
</UserJourney>
```

## <a name="step-5---add-the-claim-city-to-your-relying-party-policy-file-so-the-claim-is-sent-to-your-application"></a>Schritt 5: Hinzufügen des „city“-Anspruchs zur Datei mit den Richtlinien der vertrauenden Seite, damit der Anspruch an Ihre Anwendung gesendet wird

Bearbeiten Sie hierzu die Datei `ProfileEdit.xml` der vertrauenden Seite, und ändern Sie das `<TechnicalProfile Id="PolicyProfile">`-Element, um Folgendes hinzuzufügen: `<OutputClaim ClaimTypeReferenceId="city" />`.

Nach dem Hinzufügen des neuen Anspruchs sieht das TechnicalProfile-Element wie folgt aus:

```XML
<DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="city" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
</TechnicalProfile>
```

## <a name="step-6---upload-your-changes-and-test"></a>Schritt 6: Hochladen Ihrer Änderungen und Durchführen eines Tests

Sie überschreiben vorhandene Versionen der Richtlinie.

1.    (OPTIONAL) Speichern Sie vor dem Fortfahren die vorhandene Version (Download) Ihrer Datei mit den Erweiterungen.  Wir empfehlen Ihnen, nicht mehrere Versionen der Datei mit den Erweiterungen hochzuladen, um die anfängliche Komplexität gering zu halten.
2.    (OPTIONAL) Sie können die neue Version der PolicyId in der Richtlinienbearbeitungsdatei umbenennen, indem Sie PolicyId="B2C_1A_TrustFrameworkProfileEdit" ändern.
3.    Laden Sie die Datei mit den Erweiterungen hoch.
4.    Laden Sie die Richtlinienbearbeitungsdatei für die vertrauende Seite hoch.
5.    Verwenden Sie **Jetzt ausführen**, um die Richtlinie zu testen.  Überprüfen Sie das Token, das vom IEF an die Anwendung zurückgegeben wird.

Wenn alles richtig eingerichtet wurde, enthält das Token den neuen `city`-Anspruch mit dem Wert `Redmond`.

```JSON
{
  "exp": 1493053292,
  "nbf": 1493049692,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493049692,
  "auth_time": 1493049692,
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Nächste Schritte

[Use a REST API as a validation step](active-directory-b2c-rest-api-validation-custom.md) (Verwenden einer REST-API als Überprüfungsschritt)

[How to modify profile edit to gather additional information from your users](active-directory-b2c-create-custom-attributes-profile-edit-custom.md) (Ändern der Profilbearbeitung zum Sammeln von Informationen von Ihren Benutzern)

