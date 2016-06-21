<properties
	pageTitle="Azure-SAML-Protokoll für einmaliges Anmelden | Microsoft Azure"
	description="In diesem Artikel wird das SAML-Protokoll für einmaliges Anmelden in Azure Active Directory beschrieben."
	services="active-directory"
	documentationCenter=".net"
	authors="priyamohanram"
	manager="mbaldwin"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/31/2016"
	ms.author="priyamo"/>

# SAML-Protokoll für einmaliges Anmelden

[AZURE.INCLUDE [active-directory-protocols](../../includes/active-directory-protocols.md)]

In diesem Artikel lernen Sie die SAML 2.0-Authentifizierungsanforderungen und -antworten kennen, die Azure Active Directory (Azure AD) für das einmalige Anmelden unterstützt.

Das folgende Protokolldiagramm beschreibt den Ablauf für das einmalige Anmelden. Der Clouddienst (der Dienstanbieter) verwendet eine HTTP-Umleitungsbindung zum Übergeben einer Authentifizierungsanforderung (`AuthnRequest`) an Azure AD (den Identitätsanbieter). Azure AD verwendet daraufhin eine HTTP Post-Bindung, um ein `Response`-Element an den Clouddienst zu senden.

![Workflow für einmaliges Anmelden](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## AuthnRequest

Um eine Benutzerauthentifizierung anzufordern, senden die Clouddienste ein `AuthnRequest`-Element an Azure AD. Im Folgenden finden Sie eine Beispielanforderung (`AuthnRequest`) für SAML 2.0:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```


| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| ID | erforderlich | Azure AD verwendet dieses Attribut zum Auffüllen des `InResponseTo`-Attributs der zurückgegebenen Antwort. Die ID darf nicht mit einer Zahl beginnen, weshalb dem GUID-String häufig eine Zeichenfolge wie etwa „id“ vorangestellt wird. `id6c1c178c166d486687be4aaf5e482730` ist beispielsweise eine gültige ID. |
| Version | erforderlich | Diese sollte **2.0** sein.|
| IssueInstant | erforderlich | Dies ist eine DateTime-Zeichenfolge mit einem UTC-Wert im [Roundtrip-Format („o“)](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD erwartet einen DateTime-Wert dieses Typs, dieser Wert wird jedoch weder bewertet noch verwendet. |
| AssertionConsumerServiceUrl | optional | Muss (falls angegeben) dem `RedirectUri` des Clouddiensts in Azure AD entsprechen. |
| ForceAuthn | optional | Sollte „false“ sein (falls angegeben). Jeder andere Wert verursacht einen Fehler.|
| IsPassive | optional | Sollte „false“ sein (falls angegeben). Jeder andere Wert verursacht einen Fehler. |  

Alle anderen `AuthnRequest`-Attribute, wie Consent, Destination, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex und ProviderName werden **ignoriert**.

Azure AD ignoriert auch das `Conditions`-Element in `AuthnRequest`.

### Aussteller

Das `Issuer`-Element in einem `AuthnRequest`-Element muss genau mit einem der **ServicePrincipalNames** im Clouddienst von Azure AD übereinstimmen. Normalerweise ist es auf den **App-ID-URI** festgelegt, der während der Anwendungsregistrierung angegeben wird.

Ein SAML-Beispielausschnitt, der das `Issuer`-Element enthält, sieht wie folgt aus:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### NameIDPolicy

Dieses Element erfordert in der Antwort ein bestimmtes Namens-ID-Format und ist bei an Azure AD gesendeten `AuthnRequest`-Elementen optional.

Ein Beispiel für ein `NameIdPolicy`-Element sieht wie folgt aus:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Wenn `NameIDPolicy` angegeben ist, können Sie sein optionales `Format`-Attribut einbeziehen. Das `Format`-Attribut kann nur einen der folgenden Werte aufweisen. Jeder andere Wert führt zu einem Fehler.

-  `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure Active Directory stellt den NameID-Anspruch als paarweisen Bezeichner aus.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory stellt den NameID-Anspruch als im E-Mail-Adressformat aus.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Dieser Wert ermöglicht es Azure Active Directory, das Anspruchsformat auszuwählen. Azure Active Directory stellt die NameID als paarweisen Bezeichner aus.

Beziehen Sie das `SPNameQualifer`-Attribut nicht ein. Azure AD ignoriert das `AllowCreate`-Attribut.

### RequestAuthnContext

Das `RequestedAuthnContext`-Element legt die gewünschten Authentifizierungsmethoden fest. In `AuthnRequest`-Elementen, die an Azure AD gesendet werden, ist es optional. Azure AD unterstützt nur einen `AuthnContextClassRef`-Wert: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### Bereichsdefinition

Das `Scoping` -Element, das eine Liste von Identitätsanbietern enthält, ist bei `AuthnRequest`-Elementen, die an Azure AD gesendet werden, optional.

Beziehen Sie das `ProxyCount`-Attribut oder die Elemente `IDPListOption` oder `RequesterID`, sofern angegeben, nicht ein, da diese nicht unterstützt werden.

### Signatur

Beziehen Sie kein `Signature`-Element in `AuthnRequest`-Elementen ein, da Azure AD signierte Authentifizierungsanfragen nicht unterstützt.

### Betreff

Azure AD ignoriert das `Subject`-Element von `AuthnRequest`-Elementen.

## Antwort

Wenn eine angeforderte Anmeldung erfolgreich abgeschlossen wird, sendet Azure AD eine Antwort an den Clouddienst. Ein Beispiel für eine Antwort auf einen erfolgreichen Anmeldeversuch sieht folgendermaßen aus:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### Antwort

Das `Response`-Element enthält das Ergebnis der Autorisierungsanforderung. Azure AD legt die Werte `ID`, `Version` und `IssueInstant` im `Response`-Element fest. Außerdem legt es die folgenden Attribute fest:

- `Destination`: Wird nach erfolgreichem Abschluss der Anmeldung auf den `RedirectUri` des Dienstanbieters (Clouddienst) festgelegt.
- `InResponseTo`: Wird auf das `ID`-Attribut des `AuthnRequest`-Elements festgelegt, das die Antwort initiiert hat.

### Aussteller

Azure AD legt das `Issuer`-Element auf `https://login.microsoftonline.com/<TenantIDGUID>/` fest, wobei <TenantIDGUID> die Mandanten-ID des Azure AD-Mandanten ist.

Eine Beispiel für eine Antwort mit einem Issuer-Element sieht beispielsweise wie folgt aus:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### Signatur

Azure AD signiert das `Response`-Element nach erfolgreicher Anmeldung. Das `Signature`-Element enthält eine digitale Signatur, die von der Anwendung zum Authentifizieren der Quelle und zum Überprüfen der Integrität der Antwort verwendet werden kann.

Azure AD verwendet den im `IDPSSODescriptor`-Element des Metadatendokuments festgelegten Signaturschlüssel. Weitere Informationen finden Sie unter [Federation Metadata](active-directory-federation-metadata.md) (Verbundmetadaten).

Azure AD signiert auch das `Assertion`-Element, die beiden Signaturelemente sind jedoch unabhängig.

Eine Beispiel für ein `Signature`-Element in der Antwort sieht wie folgt aus:

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
```

### Status

Das `Status`-Element gibt an, ob die Anmeldung erfolgreich war. Es enthält das `StatusCode`-Element, das einen Code oder mehrere geschachtelte Codes enthält, die den Status der Anforderung darstellen. Es enthält zudem das `StatusMessage`-Element, das spezifische, während der Anmeldung generierte Fehlermeldungen enthält.

<!-- TODO: Add a authentication protocol error reference -->

Im Folgenden finden Sie eine SAML-Antwort auf einen fehlgeschlagenen Anmeldeversuch.

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: The SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### Assertion

Zusätzlich zu `ID`, `IssueInstant` und `Version` legt Azure AD die folgenden Elemente im `Assertion`-Element der Antwort fest.

#### Aussteller

Ist auf `https://sts.windows.net/<TenantIDGUID>/` festgelegt, wobei <TenantIDGUID> die Mandanten-ID des Azure AD-Mandanten ist.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### Signatur

Azure AD signiert die Assertion als Antwort auf eine erfolgreiche Anmeldung. Das `Signature`-Element enthält eine digitale Signatur, die vom Clouddienst zum Authentifizieren der Quelle und zum Überprüfen der Integrität der Assertion verwendet werden kann.

Azure AD verwendet den im `IDPSSODescriptor`-Element des Metadatendokuments festgelegten Signaturschlüssel, um diese digitale Signatur zu generieren.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### Betreff

Legt den Prinzipal fest, der Betreff der Anweisungen in der Assertion ist. Er enthält ein `NameID`-Element, das den authentifizierten Benutzer darstellt. Der `NameID`-Wert ist ein gezielter Bezeichner, der nur an den Dienstanbieter gerichtet ist, welcher die Zielgruppe für das Token darstellt. Er ist persistent – er kann widerrufen werden, eine erneute Zuweisung ist jedoch nicht mehr möglich. Er ist zudem nicht transparent, d.h. Informationen über den Benutzer werden nicht preisgegeben, und er kann nicht als Bezeichner für Attributabfragen verwendet werden.

Das `Method`-Attribut des `SubjectConfirmation`-Elements ist immer auf `urn:oasis:names:tc:SAML:2.0:cm:bearer` festgelegt.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### Bedingungen

Dieses Element legt die Bedingungen fest, die die akzeptable Verwendung von SAML-Assertions definieren.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

Die Attribute `NotBefore` und `NotOnOrAfter` legen das Intervall fest, in dem die Assertion gültig ist.

- Der Wert des `NotBefore`-Attributs entspricht dem Wert des `IssueInstant`-Attributs des `Assertion`-Elements oder ist etwas (weniger als einer Sekunde) höher. Azure AD berücksichtigt keine Zeitunterschiede zwischen sich selbst und dem Clouddienst (Dienstanbieter) und fügt dieser Zeit keinen Puffer hinzu.
- Der Wert des `NotOnOrAfter`-Attributs ist 70 Minuten nach dem Wert des `NotBefore`-Attributs.

#### Zielgruppe

Dieses Element enthält einen URI, der die beabsichtigte Zielgruppe identifiziert. Azure AD legt den Wert dieses Elements auf den Wert des `Issuer`-Elements der `AuthnRequest` fest, die die Anmeldung initiiert hat. Verwenden Sie zum Auswerten des Werts `Audience` den Wert von `App ID URI`, der während der Anwendungsregistrierung bereitgestellt wurde.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Wie der `Issuer`-Wert muss auch der `Audience`-Wert mit einem der Dienstprinzipalnamen des Clouddiensts in Azure AD genau übereinstimmen. Wenn der Wert des `Issuer`-Elements kein URI-Wert ist, entspricht der `Audience`-Wert in der Antwort dem `Issuer`-Wert mit dem Präfix `spn:`.

#### AttributeStatement

Enthält Ansprüche bezüglich des Antragstellers oder des Benutzers. Der folgende Auszug enthält ein `AttributeStatement`-Beispielelement. Die Auslassungszeichen weisen darauf hin, dass das Element mehrere Attribute und Attributwerte enthalten kann.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```		

- **Namensanspruch** : Der Wert des `Name`-Attributs (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) ist der Benutzerprinzipalname des authentifizierten Benutzers, wie z.B. `testuser@managedtenant.com`.
- **ObjectIdentifier-Anspruch**: Der Wert des `ObjectIdentifier`-Attributs (`http://schemas.microsoft.com/identity/claims/objectidentifier`) ist die `ObjectId` des Verzeichnisobjekts, das den authentifizierten Benutzer in Azure AD darstellt. `ObjectId` ist ein unveränderlicher, global eindeutiger und sicher wiederverwendbarer Bezeichner des authentifizierten Benutzers.

#### AuthnStatement

Dieses Element bestätigt, dass das Assertion-Subjekt mit einer bestimmten Methode zu einem bestimmten Zeitpunkt authentifiziert wurde.

- Das `AuthnInstant`-Attribut gibt die Uhrzeit an, zu der der Benutzer mit Azure AD authentifiziert wurde.
- Das `AuthnContext`-Element gibt den Authentifizierungskontext an, der zum Authentifizieren des Benutzers verwendet wurde.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```

<!---HONumber=AcomDC_0608_2016-->