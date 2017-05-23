---
title: "Azure AD B2C – Integrieren des Austauschs von REST-API-Ansprüchen als Validierung in benutzerdefinierten Richtlinien | Microsoft-Dokumentation"
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
ms.date: 04/24/2017
ms.author: joroja
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 46abe48c3c9a7aab3fe013811d088a63957fe500
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017

---

# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journeys-as-validation-on-user-input"></a>Exemplarische Vorgehensweise: Integrieren von REST-API-Anspruchsaustausch-Vorgängen in Ihre Azure AD B2C User Journeys als Validierung der Benutzereingabe

Das **Identity Experience Framework** (IEF), das Azure AD B2C zugrunde liegt, ermöglicht dem Identitätsentwickler das Integrieren einer Interaktion mit einer RESTful-API in eine User Journey.  

Am Ende dieser exemplarischen Vorgehensweise sind Sie in der Lage, Azure AD B2C User Journeys zu erstellen, die mit RESTful-Diensten interagieren.

Das IEF sendet Daten in Ansprüchen und erhält Daten in Ansprüchen zurück.  Die Interaktion in die API kann als Austausch von REST-API-Ansprüchen (in Form eines Validierungsprofils), der innerhalb eines Orchestrierungsschritts erfolgt, entworfen werden.

- Hiermit wird normalerweise die Eingabe eines Benutzers validiert.
- Wenn der Wert des Benutzers abgelehnt wird, kann er erneut versuchen, einen gültigen Wert einzugeben, und für den Benutzer kann eine Fehlermeldung zurückgegeben werden.

Die Interaktion kann auch als Orchestrierungsschritt entworfen werden. Weitere Informationen finden Sie unter [Exemplarische Vorgehensweise: Integrieren von REST-API-Anspruchsaustausch-Vorgängen in Ihre Azure AD B2C User Journeys als Orchestrierungsschritt](active-directory-b2c-rest-api-step-custom.md).

Für das Beispiel für das Validierungsprofil verwenden wir die User Journey der Profilbearbeitung in der Starter Pack-Datei „ProfileEdit.xml“.

Wir können sicherstellen, dass der Name, der vom Benutzer bei der Profilbearbeitung angegeben wird, nicht Teil einer Ausschlussliste ist.

## <a name="prerequisites"></a>Voraussetzungen

- Konfiguration eines Azure AD B2C-Mandanten zur Durchführung der Registrierung/Anmeldung für ein lokales Konto wie unter [Erste Schritte](active-directory-b2c-get-started-custom.md) beschrieben.
- Ein REST-API-Endpunkt, mit dem interagiert werden kann. Die Demowebsite [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) wurde mit einem REST-API-Dienst eingerichtet, der für diese exemplarische Vorgehensweise verwendet wird.

## <a name="step-1---prepare-the-rest-api-function"></a>Schritt 1: Vorbereiten der REST-API-Funktion

> [!NOTE]
> Die Einrichtung von REST-API-Funktionen wird in diesem Artikel nicht beschrieben. Für [Azure-Funktionen-Apps](https://docs.microsoft.com/azure/azure-functions/functions-reference) gibt es ein hervorragendes Toolkit für die Erstellung von RESTful-Diensten in der Cloud.

Wir haben eine Azure-Funktion erstellt, die einen Anspruch erhält. Dieser wird als „playerTag“ erwartet, und es wird überprüft, ob der Anspruch vorhanden ist. Sie können auf [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples) auf den gesamten Azure-Funktionscode zugreifen.

```csharp
if (requestContentAsJObject.playerTag == null)
{
  return request.CreateResponse(HttpStatusCode.BadRequest);
}

var playerTag = ((string) requestContentAsJObject.playerTag).ToLower();

if (playerTag == "mcvinny" || playerTag == "msgates123" || playerTag == "revcottonmarcus")
{
  return request.CreateResponse<ResponseContent>(
    HttpStatusCode.Conflict,
    new ResponseContent
    {
      version = "1.0.0",
      status = (int) HttpStatusCode.Conflict,
      userMessage = $"The player tag '{requestContentAsJObject.playerTag}' is already used."
    },
    new JsonMediaTypeFormatter(),
    "application/json");
}

return request.CreateResponse(HttpStatusCode.OK);
```

Der von der Azure-Funktion zurückgegebene `userMessage`-Anspruch wird vom Identity Experience Framework erwartet und wird für den Benutzer als Zeichenfolge dargestellt, wenn die Validierung fehlschlägt. Ein Beispiel hierfür ist das Zurückgeben des Konfliktstatus 409 im obigen Beispiel.

## <a name="step-2---configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworkextensionsxml-file"></a>Schritt 2: Konfigurieren des RESTful-API-Anspruchsaustauschs als technisches Profil in der Datei „TrustFrameworkExtensions.xml“

Ein technisches Profil umfasst die vollständige Konfiguration des Austauschs, der für den RESTful-Dienst gewünscht ist. Öffnen Sie die Datei `TrustFrameworkExtensions.xml`, und fügen Sie im `<ClaimsProviders>`-Element den XML-Codeausschnitt hinzu.

> [!NOTE]
> Betrachten Sie den „Restful Provider, Version 1.0.0.0“, der unten als Protokoll angegeben ist, als die Funktion, die mit dem externen Dienst interagiert.  Eine vollständige Definition des Schemas finden Sie unter <!-- TODO: Link to RESTful Provider schema definition>-->.

```xml
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-CheckPlayerTagWebHook">
            <DisplayName>Check Player Tag Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/CheckPlayerTagWebHook?code=L/05YRSpojU0nECzM4Tp3LjBiA2ZGh3kTwwp1OVV7m0SelnvlRVLCg==</Item>
                <Item Key="AuthenticationType">None</Item>
                <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="playerTag" />
            </InputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
            <ValidationTechnicalProfiles>
                <ValidationTechnicalProfile ReferenceId="AzureFunctions-CheckPlayerTagWebHook" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

Das `InputClaims`-Element definiert die Ansprüche, die von der IEE zum REST-Dienst gesendet werden. Im obigen Beispiel wird der Inhalt des `givenName`-Anspruchs als `playerTag` an den REST-Dienst gesendet. In diesem Beispiel wird von der IEE keine Rückgabe von Ansprüchen erwartet. Stattdessen wird auf eine Antwort vom REST-Dienst gewartet, und basierend auf den empfangenen Statuscodes werden die entsprechenden Schritte ausgeführt.

## <a name="step-3---include-the-restful-service-claims-exchange-in-self-asserted-technical-profile-where-you-wish-to-validate-the-user-input"></a>Schritt 3: Einfügen des Austauschs von Ansprüchen des RESTful-Diensts in ein selbstbestätigtes technisches Profil zum Validieren der Benutzereingabe

Der Validierungsschritt wird am häufigsten bei der Interaktion mit einem Benutzer eingesetzt.  Alle Interaktionen, bei denen vom Benutzer eine Eingabe erwartet wird, sind **selbstbestätigte technische Profile**. In diesem Beispiel fügen wir diese Validierung dem technischen Profil (TP) **Self-Asserted-ProfileUpdate** hinzu.  Dies ist das TP, das von der RP-Richtliniendatei `Profile Edit` verwendet wird.

Gehen Sie wie folgt vor, um dem selbstbestätigten Profil den Anspruchsaustausch hinzuzufügen:

1. Öffnen Sie die TrustFrameworkBase-Datei, und suchen Sie nach `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.
2. Prüfen Sie die Konfiguration dieses TP, und ermitteln Sie, wie der Austausch mit dem Benutzer basierend auf Ansprüchen definiert ist: Ansprüche, die vom Benutzer gefordert werden (Eingabeansprüche), und Ansprüche, die vom selbstbestätigten Anbieter zurückerwartet werden (Ausgabeansprüche).
3. Suchen Sie nach `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`. Beachten Sie, dass dieses Profil als Orchestrierungsschritt 6 von `<UserJourney Id="ProfileEdit">` aufgerufen wird.

## <a name="step-4---upload-and-test-the-profile-edit-rp-policy-file"></a>Schritt 4: Hochladen und Testen der RP-Richtliniendatei für die Profilbearbeitung

1. Laden Sie die neue Version der Datei `TrustframeworkExtensions` hoch.
2. Verwenden Sie die Option **Jetzt ausführen**, um die RP-Richtliniendatei für die Profilbearbeitung zu testen.
3. Testen Sie die Validierung, indem Sie einen der vorhandenen Namen (z.B. „mcvinny“) im Feld **Given Name** (Angegebener Name) eingeben. Wenn alles richtig eingerichtet ist, sollten Sie eine Meldung mit dem Hinweis erhalten, dass das `player tag` bereits verwendet wird.

## <a name="next-steps"></a>Nächste Schritte

[How to modify profile edit and user registration to gather additional information from your users](active-directory-b2c-create-custom-attributes-profile-edit-custom.md) (Ändern der Profilbearbeitung und der Benutzerregistrierung zum Sammeln von Informationen von Ihren Benutzern)

[Exemplarische Vorgehensweise: Integrieren von REST-API-Anspruchsaustausch-Vorgängen in Ihre Azure AD B2C User Journeys als Orchestrierungsschritt](active-directory-b2c-rest-api-step-custom.md)

