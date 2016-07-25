<properties 
   pageTitle="Autorisierung mit Resource Manager-API | Microsoft Azure"
   description="Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API und Active Directory für die Integration einer Anwendung in Azure."
   services="azure-resource-manager,active-directory"
   documentationCenter="na"
   authors="dushyantgill"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/12/2016"
   ms.author="dugill;tomfitz" />


# Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API

Wenn Sie Softwareentwickler sind und eine App in Azure integrieren oder Azure-Ressourcen von Kunden verwalten möchten, erhalten Sie in diesem Thema Informationen zur Authentifizierung mit den Azure Resource Manager-APIs.

Ihre App kann auf die Resource Manager-APIs auf verschiedene Arten zugreifen:

1. **Benutzer- und App-Zugriff**: Verwenden Sie diese Methode für Apps, die für einen angemeldeten Benutzer auf Ressourcen zugreifen. Dies funktioniert für Apps wie Web-Apps und Befehlszeilentools, die nur für die „interaktive Verwaltung“ von Azure-Ressourcen verwendet werden.
1. **Nur App-Zugriff**: Verwenden Sie diese Methode, wenn der App-Identität direkter Zugriff auf die Ressourcen gewährt werden muss. Dies funktioniert für Daemondienste und geplante Aufträge, die einen längeren „Offlinezugriff“ auf Azure erfordern.

Dieses Thema enthält eine schrittweise Beschreibung der Erstellung einer App, in der beide Autorisierungsmethoden genutzt werden.

Sie erstellen eine Webanwendung für folgende Aufgaben:

1. Einen Azure-Benutzer anmelden
2. Resource Manager für den Benutzer abfragen (Benutzer- und App-Zugriff), um eine Liste der Azure-Abonnements zu erhalten, die der Benutzer besitzt
3. Dem Benutzer ermöglichen, Abonnements mit der App zu „verbinden“, und dabei der Anwendung direkten Zugriff auf das Abonnement gewähren
4. Über die Anwendung auf Resource Manager zugreifen, um Offlinevorgänge auszuführen (nur App-Zugriff)

Im Folgenden ist der komplette Datenfluss in der Webanwendung dargestellt, die Sie schreiben.

![ARM-Autorisierung – App-Registrierung 1](./media/resource-manager-api-authentication/ARM-Auth-Swim-Lane.png)

Der gesamte Code für dieses Thema wird als Web-App ausgeführt, die Sie unter [http://vipswapper.azurewebsites.net/cloudsense](http://vipswapper.azurewebsites.net/cloudsense) ausprobieren können.

Als Benutzer wählen Sie den Typ des Kontos für die Anmeldung aus:

![Anmeldung auswählen](./media/resource-manager-api-authentication/ARM-Auth-Sample-App-Ux-1.png)

Geben Sie Ihre Anmeldeinformationen ein.

![Anmeldeinformationen angeben](./media/resource-manager-api-authentication/ARM-Auth-Sample-App-Ux-2.png)

Gewähren Sie der App Zugriff auf Ihre Azure-Abonnements:
 
 ![Gewähren von Zugriff](./media/resource-manager-api-authentication/ARM-Auth-Sample-App-Ux-3.png)
 
Verbinden Sie Ihre Abonnements zur Überwachung mit der App:

![Abonnement verbinden](./media/resource-manager-api-authentication/ARM-Auth-Sample-App-Ux-4.png)

Trennen oder reparieren Sie die Verbindung mit der App:

![Abonnement trennen](./media/resource-manager-api-authentication/ARM-Auth-Sample-App-Ux-5.png)

## Registrieren Ihrer Anwendung für Azure Active Directory

Zunächst registrieren Sie die Web-App bei Azure Active Directory (AD). Durch die App-Registrierung wird eine zentrale Identität für Ihre App in Azure AD erstellt. Sie enthält grundlegende Informationen über Ihre Anwendung wie die OAuth-Client-ID, Antwort-URLs und Anmeldeinformationen, die Ihre Anwendung zur Authentifizierung von Azure Resource Manager-APIs und zum Zugriff darauf verwendet. Die App-Registrierung zeichnet auch die verschiedenen delegierten Berechtigungen auf, die Ihre Anwendung benötigt, um für den Benutzer auf Microsoft-APIs zuzugreifen.

Das Thema [Erstellen einer Active Directory-Anwendung und eines Dienstprinzipals mithilfe des Portals](resource-group-create-service-principal-portal.md) zeigt alle Schritte, die zur Einrichtung der Anwendung erforderlich sind. Ziehen Sie dieses Thema heran, während Sie eine Anwendung mit den folgenden Eigenschaften erstellen:

- Webanwendung mit dem Namen **CloudSense**
- Anmelde-URL und App-ID-URI im Format **http://{domain_name_of_your_directory}/{name_of_the_app}**.
- Authentifizierungsschlüssel für die Anmeldung der Anwendung
- Delegierte Berechtigung **Auf Azure-Dienstverwaltung zugreifen** für die **Azure-Dienstverwaltungs-API** Übernehmen Sie den Standardwert **Einmaliges Anmelden und Lesen des Benutzerprofils aktivieren** für **Azure Active Directory**.
- Mehrinstanzenfähige Anwendung

### Optionale Konfiguration – Zertifikatanmeldeinformationen

Azure AD unterstützt außerdem Zertifikatanmeldeinformationen für Anwendungen: Sie erstellen ein selbstsigniertes Zertifikat, behalten den privaten Schlüssel und fügen den öffentlichen Schlüssel der Azure AD-Anwendungsregistrierung hinzu. Zur Authentifizierung sendet Ihre Anwendung eine kleine Nutzlast an Azure AD, die mit Ihrem privaten Schlüssel signiert ist, und Azure AD überprüft die Signatur mithilfe des öffentlichen Schlüssels, den Sie registriert haben.

Informationen zum Konfigurieren des Zertifikats finden Sie unter [Build service and daemon apps in Office 365](https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365) (Erstellen von Dienst- und Daemon-Apps in Office 365). Im Abschnitt „Configuring a X.509 public cert for your application“ (Konfigurieren eines öffentlichen X.509-Zertifikats für die Anwendung) finden Sie eine schrittweise Anleitung zum Einrichten des Zertifikats. Unter [Authentifizieren eines Dienstprinzipals mit Azure Resource Manager](resource-group-authenticate-service-principal.md) finden Sie Beispiele für das Konfigurieren eines Zertifikats über Azure PowerShell oder die Azure-Befehlszeilenschnittstelle.

## Authentifizieren des Benutzers und Abrufen des Zugriffstokens

Sie haben jetzt alles, was Sie benötigen, um mit dem Programmieren der Anwendung zu beginnen. Dieses Thema enthält die REST-API-Beispiele für jeden Schritt des kompletten Datenflusses und Links zum entsprechenden C#-Code für jeden Schritt. Die vollständige ASP.NET MVC-Beispielanwendung finden Sie unter [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense).

Sie beginnen an dem Punkt, an dem der Benutzer entscheidet, sein Azure-Abonnement mit Ihrer Anwendung zu verbinden.

Sie müssen den Benutzer um zwei Dinge bitten:

1. **Verzeichnisdomänenname**: Der Domänenname des Azure Active Directory-Verzeichnisses, das mit dem Azure-Abonnement des Benutzers verknüpft ist. Die OAuth 2.0-Autorisierungsanforderung muss an dieses Azure AD-Verzeichnis gesendet werden. Der Benutzer kann den Azure AD-Domänennamen ermitteln, indem er zum Azure-Portal navigiert und das Konto oben rechts auswählt. Sie könnten dem Benutzer visuelle Hinweise wie den folgenden bereitstellen:

     ![](./media/resource-manager-api-authentication/show-directory.png)
   
1. **Microsoft-Konto oder Geschäftskonto**: Bestimmen Sie, ob der Benutzer sein eigenes Azure-Abonnement mit einem Microsoft-Konto (wird auch als Live-ID bezeichnet) oder einem Geschäftskonto (auch bekannt als Organisationskonto) verwaltet. Bei einem Microsoft-Konto leitet die Anwendung den Benutzer zur Azure Active Directory-Anmeldeseite weiter. Dies erfolgt, indem ein Abfragezeichenfolgen-Parameter (&domain\_hint=live.com) Azure AD anweist, den Benutzer direkt zur Anmeldeseite für das Microsoft-Konto weiterzuleiten. Autorisierungscode und -token, die Sie für beide Kontotypen erhalten, werden auf die gleiche Weise verarbeitet.

Die Anwendung leitet dann den Benutzer mit einer OAuth 2.0-Autorisierungsanforderung zu Azure AD weiter, um die Anmeldeinformationen des Benutzers zu authentifizieren und einen Autorisierungscode zu erhalten. Die Anwendung verwendet den Autorisierungscode, um ein Zugriffstoken für Resource Manager zu erhalten.

### Autorisierungsanforderung (OAuth 2.0)

Geben Sie eine Open ID Connect/OAuth2.0-Autorisierungsanforderung an den Azure AD-Autorisierungsendpunkt aus:

    https://login.microsoftonline.com/{directory_domain_name}/OAuth2/Authorize

Die Abfragezeichenfolgen-Parameter, die für diese Anforderung verfügbar sind, werden im Thema [Authorization Code Grant Flow](https://msdn.microsoft.com/library/azure/dn645542.aspx) (Datenfluss für Autorisierungscodeberechtigungen) beschrieben.

Das folgende Beispiel zeigt, wie Sie eine OAuth2.0-Autorisierung anfordern:

    https://login.windows.net/dushyantgill.com/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD authentifiziert den Benutzer und fordert den Benutzer ggf. auf, eine Berechtigung für die App zu erteilen. Dann wird der Autorisierungscode an die Antwort-URL der Anwendung zurückgegeben. Abhängig vom angeforderten Wert für „response\_mode“ sendet Azure AD die Daten in einer Abfragezeichenfolge oder als POST-Daten zurück.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### Autorisierungsanforderung (Open ID Connect)

Wenn Sie nicht nur für den Benutzer auf Azure Resource Manager zugreifen möchten, sondern dem Benutzer auch ermöglichen möchten, sich bei Ihrer Anwendung mit einem Azure AD-Konto anzumelden, geben Sie eine Open ID Connect-Autorisierungsanforderung aus. Mit Open ID Connect empfängt die Anwendung auch ein „id\_token“ von Azure AD, das Ihre App zum Anmelden des Benutzers verwenden kann.

Abfragezeichenfolgen-Parameter der OAuth2.0-Autorisierungsanforderung:

| Abfragezeichenfolgen-Parameter | Wert
|----|----
| client\_id | Client-ID der Anwendung
| response\_mode | **form\_post** oder **query**
| response\_type | **code+id\_token**
| redirect\_uri | URL-codierte Antwort-URL der Anwendung. Beispiel: http://www.vipswapper.com/cloudsense/Account/SignIn |
| resource | URL-codierter Bezeichner von Azure-Dienstverwaltungs-APIs: https://management.core.windows.net/ |
| Bereich | openid+profile
| nonce | Datenelement, um die Autorisierungsanforderung mit dem zurückgegebenen „id\_token“ zu verknüpfen, um sicherzustellen, dass die Autorisierungsantwort angefordert wurde und nicht wiedergegeben wird.
| domain\_hint | live.com <br />**Hinweis:** Verwenden Sie den domain\_hint-Parameter nur, wenn der Benutzer das Azure-Abonnement mit einem Microsoft-Konto verwaltet.
| state | Geben Sie optional Statusdaten an, die Azure AD mit der Antwort zurückgegeben soll.

Beispiel für eine Open ID Connect-Anforderung:

     https://login.windows.net/dushyantgill.com/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD authentifiziert den Benutzer und fordert den Benutzer ggf. auf, eine Berechtigung für die App zu erteilen. Dann wird der Autorisierungscode an die Antwort-URL der Anwendung zurückgegeben. Abhängig vom angeforderten Wert für „response\_mode“ sendet Azure AD die Daten in einer Abfragezeichenfolge oder als POST-Daten zurück.

Beispiel für eine Open ID Connect-Antwort:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### Überprüfen von „id\_token“

Bevor die Anwendung den Benutzer anmeldet, muss „id\_token“ überprüft werden. Die Tokenüberprüfung ist ein komplexes Thema, und es empfiehlt sich, für die Entwicklungsplattform eine Standardbibliothek für JSON-Webtoken-Handler zu verwenden. (Informationen finden Sie im [Quellcode des .NET Azure AD-JWT-Handlers](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/blob/master/src/System.IdentityModel.Tokens.Jwt/JwtSecurityTokenHandler.cs).) Sie sind für die Sicherheit der Anwendung verantwortlich. Stellen Sie also sicher, dass die Bibliothek, die Sie für die Verarbeitung von „id\_token“ verwenden, ordnungsgemäß die folgenden Aspekte des Tokens überprüft:

- **Timing des Tokens**: Vergewissern Sie sich anhand der nbf- und exp-Ansprüche, dass das Token nicht zu neu oder zu alt ist. Es ist üblich, Pufferzeit (5 Minuten) einzuplanen, um Zeitabweichungen zu berücksichtigen.
- **Aussteller**: Vergewissern Sie sich anhand des iss-Anspruchs, dass Azure Active Directory der Aussteller des Tokens ist: https://sts.windows.net/{tenant_id_of_the_directory}
- **Zielgruppe**: Vergewissern Sie sich anhand des aud-Anspruchs, dass das Token für die Anwendung vorgesehen ist. Der Wert muss die Client-ID der Anwendung sein.
- **Nonce**: Überprüfen Sie den Nonce-Anspruch, um die Nonce-Daten mit den Daten abzugleichen, die Sie in der Autorisierungsanforderung gesendet haben. So können Sie sich vergewissern, dass die Antwort von der Anwendung angefordert wurde und es sich nicht um ein Token-Replay handelt.
- **Signatur**: Ihre App muss überprüfen, ob das Token von Azure Active Directory signiert wurde. Azure AD-Signaturschlüssel werden häufig zurückgesetzt. Ihre App muss also täglich aktualisierte Schlüssel abrufen oder die aktualisierten Schlüssel laden, wenn die Überprüfung der Signatur fehlschlägt. Weitere Informationen finden Sie unter [Wichtige Informationen zum Signaturschlüsselrollover in Azure AD](active-directory/active-directory-signing-key-rollover.md).

Nachdem **id\_token** überprüft wurde, verwenden Sie den Wert des oid-Anspruchs als unveränderlichen und nicht wiederverwendbaren Bezeichner des Benutzers. Verwenden Sie entweder den **unique\_name**-Anspruch oder den upn/email-Anspruch als für Menschen lesbaren Anzeigenamen des Benutzers. Sie können auch die optionalen Ansprüche „given\_name“ und „family\_name“ für Anzeigezwecke verwenden.

### Tokenanforderung (Datenfluss für OAuth2.0-Codeberechtigungen)

Nachdem die Anwendung den Autorisierungscode von Azure AD erhalten hat, ist es an der Zeit, das Zugriffstoken für Azure Resource Manager abzurufen. Senden Sie eine Anforderung für ein OAuth2.0-Codeberechtigungstoken an den Azure AD-Tokenendpunkt:

    https://login.microsoftonline.com/{directory_domain_name}/OAuth2/Token

Die Abfragezeichenfolgen-Parameter, die für diese Anforderung verfügbar sind, werden im Thema [Authorization Code Grant Flow](https://msdn.microsoft.com/library/azure/dn645542.aspx) (Datenfluss für Autorisierungscodeberechtigungen) beschrieben.

Das folgende Beispiel zeigt eine Anforderung für ein Codeberechtigungstoken mit Kennwort:

    POST https://login.windows.net/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Wenn Sie Zertifikatanmeldeinformationen verwenden, erstellen Sie ein JSON-Webtoken (JWT), und signieren Sie (RSA-SHA256) mit dem privaten Schlüssel der Zertifikatanmeldeinformationen Ihrer Anwendung. Die Anspruchstypen für das Token werden unter [Authorization Code Grant Flow](https://msdn.microsoft.com/library/azure/dn645542.aspx) (Datenfluss für Autorisierungscodeberechtigungen) dargestellt. Informationen zum Signieren von JWT-Token für die Clientassertion finden Sie im [Code der Active Directory Authentication Library (.NET)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/master/src/ADAL.NET/CryptographyHelper.cs).

Informationen zur Clientauthentifizierung finden Sie in der [Open ID Connect-Spezifikation](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication). Ein Beispiel für ein JWT-Token für die Clientassertion finden Sie [hier](https://www.authnauthz.com/OAuth/ParseJWTToken?token=eyJhbGciOiJSUzI1NiIsIng1dCI6IlFwcXdKZnJNZ003ekJ4M1hkM2NSSFdkYVFsTSJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ud2luZG93cy5uZXRcL2FhbHRlc3RzLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQyODk2Mjk5MSwiaXNzIjoiOTA4M2NjYjgtOGE0Ni00M2U3LTg0MzktMWQ2OTZkZjk4NGFlIiwianRpIjoiMmYyMjczMzQtZGQ3YS00NzZkLWFlOTYtYzg4NDQ4YTkxZGM0IiwibmJmIjoxNDI4OTYyMzkxLCJzdWIiOiI5MDgzY2NiOC04YTQ2LTQzZTctODQzOS0xZDY5NmRmOTg0YWUifQ.UXQE9H-FlwxYQmRVG0-p7pAX9TFgiRXcYr7GhbcC7ndIPHKpZ5tfHWPEgBl3ZVRvF2l8uA7HEV86T7t2w7OHhHwLBoW7XTgj-17hnV1CY21MwjrebPjaPIVITiilekKiBASfW2pmss3MjeOYcnBV2MuUnIgt4A_iUbF_-opRivgI4TFT4n17_3VPlChcU8zJqAMpt3TcAxC3EXXfh10Mw0qFfdZKqQOQxKHjnL8y7Of9xeB9BBD_b22JNRv0m7s0cYRx2Cz0cUUHw-ipHhWaW7YwhVRMfK6BMkaDUgaie4zFkcgHb7rm1z0rM1CvzIqP-Mwu3oEqYpY9cYo8nEjMyA).

Das folgende Beispiel zeigt eine Anforderung für ein Codeberechtigungstoken mit Zertifikatanmeldeinformationen:

	POST https://login.windows.net/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1
	
	Content-Type: application/x-www-form-urlencoded
	Content-Length: 1012
	
	grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Eine Beispielantwort für ein Codeberechtigungstoken:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### Verarbeiten der Antwort für ein Codeberechtigungstoken

Eine erfolgreiche Antwort enthält das Zugriffstoken (Benutzer und App) für Azure Resource Manager. Ihre Anwendung verwendet dieses Zugriffstoken, um für den Benutzer auf Resource Manager zuzugreifen. Die Lebensdauer des von Azure AD ausgestellten Zugriffstokens beträgt eine Stunde. Es ist unwahrscheinlich, dass Ihre Webanwendung das Zugriffstoken (Benutzer und App) erneuern muss. Ist dies jedoch der Fall, können Sie das Aktualisierungstoken verwenden, das die Anwendung in der Tokenantwort empfängt. Senden Sie eine Anforderung für ein OAuth2.0-Token an den Azure AD-Tokenendpunkt:

    https://login.microsoftonline.com/{directory_domain_name}/OAuth2/Token

Die Parameter für die Aktualisierungsanforderung sind unter [Authorization Code Grant-Datenfluss](https://msdn.microsoft.com/library/azure/dn645542.aspx) (Datenfluss für Autorisierungscodeberechtigungen) beschrieben.

Das folgende Beispiel zeigt die Verwendung des Aktualisierungstokens:

    POST https://login.windows.net/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Beachten Sie, dass Sie zwar mit Aktualisierungstoken neue Zugriffstoken für Azure Resource Manager abrufen können, sie sind aber für Offlinezugriff durch Ihre Anwendung nicht geeignet. Die Lebensdauer der Aktualisierungstoken ist begrenzt, und Aktualisierungstoken sind an den Benutzer gebunden. Wenn der Benutzer die Organisation verlässt, kann die Anwendung, die das Aktualisierungstoken nutzt, nicht mehr zugreifen. Dieser Ansatz ist nicht für Anwendungen geeignet, die von Teams zum Verwalten ihrer Azure-Ressourcen verwendet werden.

## Auflisten verfügbarer Abonnements

Ihre Anwendung verfügt jetzt über ein Token, um für den Benutzer auf Azure Resource Manager zuzugreifen.

Der nächste Schritt ist, dem Benutzer zu ermöglichen, sein Azure-Abonnement mit Ihrer App zu verbinden, damit Ihre App diese Abonnements verwalten kann, selbst wenn der Benutzer nicht erreichbar ist (langfristiger Offlinezugriff). Zeigen Sie dem Benutzer die Liste der Azure-Abonnements an, für die der Benutzer den Zugriff verwalten kann, und ermöglichen Sie es dem Benutzer, der Identität Ihrer Anwendung direkt eine RBAC-Rolle zuzuweisen.

![ARM-Autorisierung – Beispiel-App-UX 4](./media/resource-manager-api-authentication/ARM-Auth-Sample-App-Ux-4-full.png)

### Auflisten der Abonnements, in denen der Benutzer über Zugriff verfügt

Zuerst rufen wir die [Resource Manager-API zum Auflisten von Abonnements](https://msdn.microsoft.com/library/azure/dn790531.aspx) auf, um alle Abonnements aufzulisten, in denen der Benutzer über eine beliebige Art von Zugriff verfügt. Anschließend ermitteln wir die Abonnements, für die der Benutzer den Zugriff verwalten kann.

Die [GetUserSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L79)-Methode der ASP.NET MVC-Beispiel-App implementiert diesen Aufruf.

Eine Beispielanforderung zum Auflisten von Abonnements:

    GET https://management.azure.com/subscriptions?api-version=2014-04-01-preview HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Eine Beispielantwort zum Auflisten von Abonnements:

    HTTP/1.1 200 OK

    {"value":[{"id":"/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2","subscriptionId":"34370e90-ac4a-4bf9-821f-85eeedeae1a2","displayName":"Sandbox","state":"Enabled","subscriptionPolicies":{"locationPlacementId":"Public_2014-09-01","quotaId":"PayAsYouGo_2014-09-01"}},{"id":"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e","subscriptionId":"c276fc76-9cd4-44c9-99a7-4fd71546436e","displayName":"Production","state":"Enabled","subscriptionPolicies":{"locationPlacementId":"Public_2014-09-01","quotaId":"PayAsYouGo_2014-09-01"}}]}

### Abrufen von Benutzerberechtigungen für Abonnements

Die Aktion zum Verbinden/Trennen sollte nur für Abonnements angezeigt werden, für die der Benutzer den Zugriff verwalten kann. Sie rufen für jedes Abonnement die [Resource Manager-API zum Auflisten von Berechtigungen](https://msdn.microsoft.com/library/azure/dn906889.aspx) auf, um zu bestimmen, ob der Benutzer über Zugriffsverwaltungsrechte für das Abonnement verfügt.

Die [UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L132)-Methode der ASP.NET MVC-Beispiel-App implementiert diesen Aufruf.

Im folgenden Beispiel wird veranschaulicht, wie die Berechtigungen eines Benutzers für ein Abonnement angefordert werden. „83cfe939-2402-4581-b761-4f59b0a041e4“ ist die ID des Abonnements.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2014-07-01-preview HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Beispiel für die Antwort beim Abrufen der Berechtigungen eines Benutzers für ein Abonnement:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

Die API für Berechtigungen gibt mehrere Berechtigungen zurück. Jede Berechtigung besteht aus zulässigen Aktionen (actions) und nicht zulässigen Aktionen (notactions). Wenn eine Aktion in der Liste der zulässigen Aktionen für eine Berechtigung vorhanden und in der Liste „notactions“ für diese Berechtigung nicht vorhanden ist, darf der Benutzer diese Aktion durchführen. **microsoft.authorization/roleassignments/write** ist die Aktion, mit der Zugriffsverwaltungsrechte gewährt werden. Ihre Anwendung muss das Ergebnis für die Berechtigungen analysieren und nach einem übereinstimmenden regulären Ausdruck für diese Aktionszeichenfolge in den zulässigen und nicht zulässigen Aktionen der einzelnen Berechtigungen suchen.

### Optional: Auflisten von Verzeichnissen, in denen das Benutzerkonto vorhanden ist

Ein Benutzerkonto kann in mehreren Azure Active Directory-Verzeichnissen vorhanden sein. Es ist möglich, dass der Benutzer anfangs nicht den richtigen Verzeichnisnamen angegeben hat. In diesem Fall wird das gewünschte Abonnement nicht in der Liste angezeigt.

Die [Resource Manager-API zum Auflisten von Mandanten](https://msdn.microsoft.com/library/azure/dn790536.aspx) listet die Bezeichner aller Verzeichnisse auf, in denen das Benutzerkonto vorhanden ist. Sie könnten die API aufrufen, um festzustellen, ob das Benutzerkonto in mehr als einem Verzeichnis vorhanden ist. Optional können Sie dem Benutzer eine Meldung wie die folgende anzeigen: „Sie haben das gesuchte Abonnement nicht gefunden? Es könnte in einem anderen Azure Active Directory-Verzeichnis vorhanden sein, in dem Sie Mitglied sind. Klicken Sie hier, um das Verzeichnis zu wechseln.“

Die [GetUserOrganizations](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20)-Methode der ASP.NET MVC-Beispiel-App implementiert diesen Aufruf.

Eine Beispielanforderung zum Auflisten von Verzeichnissen:

    GET https://management.azure.com/tenants?api-version=2014-04-01-preview HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1Qi****8DJf1UO4a-ZZ_TJmWFlwO1mM7Cw6JWtfY2lGc5A

Eine Beispielantwort zum Auflisten von Verzeichnissen:

    HTTP/1.1 200 OK

    {"value":[{"id":"/tenants/7fe877e6-a150-4992-bbfe-f517e304dfa0","tenantId":"7fe877e6-a150-4992-bbfe-f517e304dfa0"},{"id":"/tenants/62e173e9-301e-423e-bcd4-29121ec1aa24","tenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24"}]}

## Verbinden des Abonnements mit der Anwendung

Sie verfügen nun über eine Liste der Azure-Abonnements, die der Benutzer mit der Anwendung verbinden kann. Der nächste Schritt ist, dem Benutzer einen Befehl bereitzustellen, um die Verbindung zu erstellen. Wenn der Benutzer **Verbinden** auswählt, führt Ihre App folgende Schritte durch:

1. Sie weist der Anwendungsidentität die entsprechende RBAC-Rolle für das Abonnement zu.
2. Sie überprüft die Zugriffszuweisung, indem die Anwendungsberechtigung für das Abonnement abgefragt wird oder indem mit dem nur für die App geltenden Token auf Resource Manager zugegriffen wird.
1. Sie zeichnet die Verbindung in der Datenstruktur für „verbundene Abonnements“ Ihrer Anwendung auf, sodass die ID des Abonnements beibehalten wird.

Sehen wir uns den ersten Schritt näher an: Um der Anwendungsidentität die entsprechenden RBAC-Rolle zuzuweisen, müssen Sie Folgendes bestimmen:

- Die Objekt-ID Ihrer Anwendungsidentität im Azure Active Directory-Verzeichnis des Benutzers
- Den Bezeichner der RBAC-Rolle, die die Anwendung für das Abonnement benötigt

Sehen wir uns den ersten Teil genauer an: Wenn Ihre Anwendung zum ersten Mal einen Benutzer aus Azure AD authentifiziert, wird ein Dienstprinzipalobjekt für die Anwendung in Azure AD erstellt. Azure ermöglicht es, dass RBAC-Rollen Dienstprinzipalen zugewiesen werden, um direkten Zugriff auf entsprechende Anwendungen auf Azure-Ressourcen zu gewähren. Dies ist genau das, was wir tun möchten. Also beginnen wir, indem wir die Azure AD Graph-API abfragen, um den Bezeichner des Dienstprinzipals der Anwendung im Azure AD-Verzeichnis des angemeldeten Benutzers zu bestimmen.

Sie haben nur ein Zugriffstoken für Azure Resource Manager. Sie benötigen ein neues Zugriffstoken zum Aufrufen der Azure AD Graph-API. Jede Anwendung in Azure AD verfügt über Berechtigungen, um das eigene Dienstprinzipalobjekt abzufragen. Daher müssen wir kein Zugriffstoken für den Benutzer und die App erstellen. Ein nur für die App geltendes Zugriffstoken ist ausreichend.

<a id="app-azure-ad-graph">
### Abrufen eines nur für die App geltenden Zugriffstokens für die Azure AD Graph-API

Geben Sie zum Authentifizieren Ihrer App und zum Abrufen eines Tokens für die Azure AD Graph-API im OAuth2.0-Datenfluss eine Tokenanforderung zur Gewährung von Clientanmeldeinformationen an den Azure AD-Tokenendpunkt aus (**https://login.microsoftonline.com/{directory\_domain\_name}/OAuth2/Token**).

Die Zeilen 73 bis 77 der [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#L73)-Methode der ASP.NET MVC-Beispielanwendung rufen unter Verwendung der Active Directory Authentication Library für .NET ein nur für die App geltendes Zugriffstoken für die Graph-API ab.

Die Daten der Tokenanforderung zur Gewährung von Clientanmeldeinformationen:

| Element | Wert
|----|----
| grant\_type | **client\_credentials**
| client\_id | Client-ID der Anwendung
| resource | URL-codierter Bezeichner der Ressource, für die das Zugriffstoken angefordert wird. In diesem Fall der Bezeichner der Azure AD Graph-API: **https://graph.windows.net/**
| client\_secret oder client\_assertion\_type und client\_assertion | Wenn Ihre Anwendung ein Kennwort verwendet, verwenden Sie „client\_secret“. Wenn Ihre Anwendung ein Zertifikat verwendet, verwenden Sie „client\_assertion“.

Beispielanforderung für ein Token zur Gewährung von Clientanmeldeinformationen:

    POST https://login.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Beispielantwort für ein Token zur Gewährung von Clientanmeldeinformationen:

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### Abrufen der Objekt-ID des Dienstprinzipals der Anwendung im Azure AD-Verzeichnis des Benutzers

Verwenden Sie jetzt das nur für die App geltende Zugriffstoken, um die API für [Azure AD Graph-Dienstprinzipale](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#ServicePrincipalEntity) abzufragen und die Objekt-ID des Dienstprinzipals der Anwendung im Verzeichnis zu ermitteln.

Die [GetObjectIdOfServicePrincipalInOrganiation](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#L66)-Methode der ASP.NET MVC-Beispielanwendung implementiert diesen Aufruf.

Im folgenden Beispiel wird gezeigt, wie der Dienstprinzipal einer Anwendung angefordert wird. „a0448380-c346-4f9f-b897-c18733de9394“ ist die Client-ID der Anwendung.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

Das folgende Beispiel zeigt eine Antwort auf die Anforderung des Dienstprinzipals einer Anwendung.

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow the application to access CloudSense on behalf of the signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow the application to access CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### Abrufen des Bezeichners der Azure-RBAC-Rolle

Sie müssen dem Dienstprinzipal der Anwendung im ausgewählten Abonnement die entsprechende RBAC-Rolle zuweisen. Dafür müssen Sie den Bezeichner der Azure-RBAC-Rolle bestimmen.

Die richtige RBAC-Rolle für Ihre Anwendung:

- Wenn Ihre Anwendung das Abonnement nur überwacht, ohne Änderungen vorzunehmen, sind für das Abonnement nur Leseberechtigungen erforderlich. Weisen Sie die Rolle **Leser** zu.
- Wenn Ihre Anwendung das Azure-Abonnement verwaltet und Entitäten erstellt/ändert/löscht, sind die Berechtigungen eines Mitwirkenden erforderlich.
  - Um einen bestimmten Typ von Ressourcen zu verwalten, weisen Sie die ressourcenspezifischen Rollen für Mitwirkende zu (Mitwirkender von virtuellen Computern, Mitwirkender von virtuellem Netzwerk, Speicherkontomitwirkender usw.)
  - Weisen die Rolle **Mitwirkender** zu, um einen beliebigen Ressourcentyp zu verwalten.

Die Rollenzuweisung für die Anwendung wird den Benutzern angezeigt, wählen Sie also nur die mindestens erforderlichen Berechtigungen aus.

Rufen Sie die [Resource Manager-API für die Rollendefinition](https://msdn.microsoft.com/library/azure/dn906879.aspx) zum Auflisten aller Azure-RBAC-Rollen auf. Durchsuchen Sie dann das Ergebnis, um die gewünschte Rollendefinition anhand des Namens zu ermitteln.

Die [GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L354)-Methode der ASP.NET MVC-Beispiel-App implementiert diesen Aufruf.

Das folgende Anforderungsbeispiel zeigt, wie Azure-RBAC-Rollenbezeichner abgerufen werden. „09cbd307-aa71-4aca-b346-5f253e6e3ebb“ ist die ID des Abonnements.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2014-07-01-preview HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

Die Antwort hat folgendes Format:

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

Beachten Sie, dass Sie diese API nicht regelmäßig aufrufen müssen. Wenn Sie die bekannte GUID der Rollendefinition ermittelt haben, können Sie die ID von Rollendefinitionen wie folgt erstellen:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Dies sind die bekannten GUIDs der häufig verwendeten integrierten Rollen:

| Rolle | Guid |
| ----- | ------ |
| Reader | acdd72a7-3385-48ef-bd42-f606fba81ae7
| Mitwirkender | b24988ac-6180-42a0-ab88-20f7382dd24c
| Mitwirkender von virtuellen Computern | d73bb868-a0df-4d4d-bd69-98a00b01fccb
| Mitwirkender von virtuellem Netzwerk | b34d265f-36f7-4a0d-a4d4-e158ca92e90f
| Mitwirkender von Speicherkonto | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25
| Mitwirkender von Website | de139f84-1756-47ae-9be6-808fbbe84772
| Mitwirkender von Webplan | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b
| Mitwirkender von SQL Server | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437
| Mitwirkender von SQL DB | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec

### Zuweisen der RBAC-Rolle zur Anwendung

Sie haben alles, was Sie benötigen, um dem Dienstprinzipal Ihrer Anwendung unter dem ausgewählten Abonnement mithilfe der [Resource Manager-API zum Erstellen einer Rollenzuweisung](https://msdn.microsoft.com/library/azure/dn906887.aspx) die passende RBAC-Rolle zuzuweisen.

Die [GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L269)-Methode der ASP.NET MVC-Beispiel-App implementiert diesen Aufruf.

Beispielanforderung zum Zuweisen einer RBAC-Rolle zur Anwendung:

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2014-10-01-preview HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

In der Anforderung werden die folgenden Werte verwendet:

| Guid | Beschreibung |
| ------ | --------- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb | Die ID des Abonnements
| c3097b31-7309-4c59-b4e3-770f8406bad2 | Die Objekt-ID des Dienstprinzipals der Anwendung
| acdd72a7-3385-48ef-bd42-f606fba81ae7 | Die bekannte GUID der RBAC-Rolle „Leser“
| 4f87261d-2816-465d-8311-70a27558df4c | Eine neue GUID, erstellt für die neue Rollenzuweisung

Die Antwort hat folgendes Format:

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### Abrufen eines nur für die App geltenden Zugriffstokens für Azure Resource Manager

Im nächsten Schritt überprüfen Sie, ob die App über den gewünschten Zugriff auf das Abonnement verfügt. Dazu sollten Sie eine Testaufgabe für das Abonnement mit einem nur für die App geltenden Token für Azure Resource Manager durchführen. Mit der Testaufgabe sollte überprüft werden, ob die Anwendung tatsächlich den gewünschten Zugriff auf das Abonnement hat, um die Offlineüberwachung/-verwaltung auszuführen.

Befolgen Sie die Anweisungen im Abschnitt [Abrufen eines nur für die App geltenden Zugriffstokens für die Azure AD Graph-API](#app-azure-ad-graph), und verwenden Sie dabei einen anderen Wert für den Ressourcenparameter, um ein nur für die App geltendes Zugriffstoken für Azure Resource Manager zu erhalten:

    https://management.core.windows.net/

Die Zeilen 210 bis 214 der [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L203)-Methode der ASP.NET MVC-Beispielanwendung rufen unter Verwendung der Active Directory Authentication Library für .NET ein nur für die App geltendes Zugriffstoken für Azure Resource Manager ab.

#### Abrufen der Berechtigungen für die Anwendung für das Abonnement

Um zu überprüfen, ob die Anwendung den gewünschten Zugriff auf ein Azure-Abonnement hat, können Sie auch die [Resource Manager-API für Berechtigungen](https://msdn.microsoft.com/library/azure/dn906889.aspx) aufrufen – ähnlich wie bei der Vorgehensweise zum Bestimmen, ob der Benutzer über Zugriffsverwaltungsrechte für das Abonnement verfügt. Rufen Sie jedoch dieses Mal die Berechtigungs-API mit dem nur für die App geltenden Token auf, das Sie im vorherigen Schritt erhalten haben.

Die [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L203)-Methode der ASP.NET MVC-Beispiel-App implementiert diesen Aufruf.

## Verwalten verbundener Abonnements

Wenn dem Dienstprinzipal Ihrer Anwendung die entsprechende RBAC-Rolle für das Abonnement zugewiesen wurde, kann Ihre Anwendung es mit den nur für die App geltenden Zugriffstoken für Azure Resource Manager überwachen und verwalten.

Wenn ein Abonnementbesitzer die Rollenzuweisung Ihrer Anwendung mit dem Azure-Verwaltungsportal oder über Befehlszeilentools entfernt, kann Ihre Anwendung nicht mehr auf das Abonnement zugreifen. In diesem Fall sollten Sie den Benutzer informieren, dass die Verbindung mit dem Abonnement von außerhalb der Anwendung unterbrochen wurde, und ihm die Möglichkeit geben, die Verbindung zu „reparieren“. Durch das „Reparieren“ wird einfach die Rollenzuweisung neu erstellt, die offline gelöscht wurde.


## Trennen von Abonnements

So wie Sie es dem Benutzer ermöglicht haben, seine Abonnements mit der Anwendung zu verbinden, müssen Sie dem Benutzer das Trennen von Abonnements ermöglichen. Aus Sicht der Zugriffsverwaltung bedeutet Trennen, dass die Rollenzuweisung entfernt wird, die der Dienstprinzipal der Anwendung für das Abonnement hat. Optional kann jeder Status in der Anwendung für das Abonnement ebenfalls entfernt werden. Nur Benutzer mit Berechtigungen zur Zugriffsverwaltung für das Abonnement können das Abonnement trennen.

Die [RevokeRoleFromServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L303)-Methode der ASP.NET MVC-Beispiel-App implementiert diesen Aufruf.

Jetzt können Benutzer Ihre Anwendung verwenden, um problemlos eine Verbindung mit ihren Azure-Abonnements herzustellen und sie zu verwalten.

<!---HONumber=AcomDC_0713_2016-->