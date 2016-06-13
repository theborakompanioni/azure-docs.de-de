<properties
	pageTitle="Azure-SAML-Protokoll für einmaliges Abmelden | Microsoft Azure"
	description="In diesem Artikel wird das SAML-Protokoll für einmaliges Abmelden in Azure Active Directory beschrieben."
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
	ms.date="01/21/2016"
	ms.author="priyamo"/>


# SAML-Protokoll für einmaliges Abmelden

[AZURE.INCLUDE [active-directory-protocols](../../includes/active-directory-protocols.md)]

Azure Active Directory (Azure AD) unterstützt das SAML 2.0-Webbrowserprofil für einmaliges Abmelden. Damit das einmalige Abmelden richtig funktioniert, muss Azure AD seine Metadaten-URL während der Anwendungsregistrierung registrieren. Azure AD ruft die Abmelde-URL und den Signaturschlüssel des Clouddiensts aus den Metadaten ab. Azure AD verwendet den Signaturschlüssel zum Überprüfen der Signatur für das eingehende LogoutRequest-Element. Die LogoutURL wird dann verwendet, um Benutzer nach dem Abmelden umzuleiten.

Wenn der Clouddienst keinen Metadatenendpunkt unterstützt, muss sich der Entwickler nach dem Registrieren der Anwendung an den Microsoft Support wenden, um die Abmelde-URL und den Signaturschlüssel anzugeben.

In diesem Diagramm ist der Workflow des Azure AD-Prozesses für das einmalige Abmelden dargestellt.

![Workflow für einmaliges Abmelden](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## LogoutRequest

Der Clouddienst sendet eine `LogoutRequest`-Nachricht an Azure AD, um anzugeben, dass eine Sitzung beendet wurde. Der folgende Auszug enthält ein `LogoutRequest`-Beispielelement.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### LogoutRequest

Für das an Azure AD gesendete `LogoutRequest`-Element sind die folgenden Attribute erforderlich:

- `ID`: Dient zum Identifizieren der Abmeldeanforderung. Der Wert von `ID` sollte nicht mit einer Zahl beginnen. Die übliche Vorgehensweise besteht darin, **id** an die Zeichenfolgendarstellung einer GUID anzufügen.

- `Version`: Legen Sie den Wert dieses Elements auf **2.0** fest. Dieser Wert ist erforderlich.

- `IssueInstant`: Dies ist eine `DateTime`-Zeichenfolge mit einem UTC-Wert (Coordinated Universal Time) und [Roundtrip-Format („o“)](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD erwartet einen Wert dieses Typs, aber der Wert wird nicht erzwungen.

- Die Attribute `Consent`, `Destination`, `NotOnOrAfter` und `Reason` werden ignoriert, wenn sie in ein `LogoutRequest`-Element eingebunden sind.

### Aussteller

Das `Issuer`-Element in einem `LogoutRequest`-Element muss genau mit einem der **ServicePrincipalNames** im Clouddienst von Azure AD übereinstimmen. Normalerweise ist es auf den **App-ID-URI** festgelegt, der während der Anwendungsregistrierung angegeben wird.

### NameID

Der Wert des `NameID`-Elements muss genau mit der `NameID` des Benutzers übereinstimmen, der abgemeldet wird.
## LogoutResponse

Azure AD sendet als Antwort auf ein `LogoutRequest`-Element ein `LogoutResponse`-Element. Der folgende Auszug enthält ein Beispiel für `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### LogoutResponse

Azure AD legt die Werte `ID`, `Version` und `IssueInstant` im `LogoutResponse`-Element fest. Außerdem wird das `InResponseTo`-Element auf den Wert des `ID`-Attributs des `LogoutRequest`-Elements festgelegt, von dem die Antwort stammt.

### Aussteller

Azure AD legt diesen Wert auf `https://login.microsoftonline.com/<TenantIdGUID>/` fest, wobei <TenantIdGUID> die Mandanten-ID des Azure AD-Mandanten ist.

Verwenden Sie zum Auswerten des Werts für das `Issuer`-Element den **App-ID-URI**-Wert, der während der Anwendungsregistrierung bereitgestellt wurde.

### Status

Azure AD verwendet das `StatusCode`-Element im `Status`-Element, um anzugeben, ob die Abmeldung erfolgreich war. Wenn beim Abmeldeversuch ein Fehler auftritt, kann das `StatusCode`-Element auch benutzerdefinierte Fehlermeldungen enthalten.

<!---HONumber=AcomDC_0601_2016-->