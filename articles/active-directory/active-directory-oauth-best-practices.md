<properties
	pageTitle="Bewährte Methoden für OAuth 2.0 in Azure AD | Microsoft Azure"
	description="In diesem Artikel werden die bewährten Methoden zur Entwicklung von Anwendungen beschrieben, für die OAuth 2.0 in Azure Active Directory verwendet wird."
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


# Bewährte Methoden für OAuth 2.0 in Azure Active Directory

[AZURE.INCLUDE [active-directory-protocols](../../includes/active-directory-protocols.md)]

## Verwenden des state-Parameters

Der `state`-Parameter ist ein zufällig generierter, nicht wiederverwendbarer Wert (normalerweise eine GUID), der vom Client in der Anforderung gesendet wird. Es ist ein optionaler Parameter, aber es wird dringend empfohlen, ihn in Anforderungen für den Autorisierungscode zu verwenden. Die Antwort enthält den gleichen `state`-Wert, und die Anwendung muss überprüfen, ob die state-Werte identisch sind, bevor die Antwort verwendet wird.

Der `state`-Parameter trägt zur Erkennung von Angriffen vom Typ „websiteübergreifende Anforderungsfälschung“ auf den Client bei. Weitere Informationen zu Angriffen dieser Art finden Sie unter [Cross-Site Request Forgery](https://tools.ietf.org/html/rfc6749#section-10.12) (Websiteübergreifende Anforderungsfälschung) im Artikel zum OAuth 2.0-Autorisierungsframework.

## Zwischenspeichern von Zugriffstoken

Um die Anzahl von Netzwerkaufrufen der Clientanwendung und die damit verbundene Latenz zu verringern, sollte die Clientanwendung Zugriffstoken für die Tokenlebensdauer zwischenspeichern, die in der OAuth 2.0-Antwort angegeben ist. Verwenden Sie zum Bestimmen der Tokenlebensdauer entweder den Parameterwert `expires_in` oder `expires_on`.

Wenn eine Web-API-Ressource den Fehlercode `invalid_token` zurückgibt, kann dies darauf hinweisen, dass von der Ressource ein abgelaufenes Token ermittelt wurde. Falls sich die Zeiten der Client- und Ressourcenuhr unterscheiden (als „zeitlicher Versatz“ bezeichnet), wird das Token von der Ressource ggf. als abgelaufen angesehen, bevor das Token aus dem Clientcache entfernt wird. Löschen Sie das Token in diesem Fall auch dann aus dem Cache, wenn der berechnete Lebensdauerzeitraum noch nicht abgelaufen ist.

## Verarbeiten von Aktualisierungstoken

Für Aktualisierungstoken werden keine Lebensdauern angegeben. Normalerweise verfügen Aktualisierungstoken über relativ lange Lebensdauern. In einigen Fällen laufen Aktualisierungstoken aber ab, werden widerrufen oder verfügen nicht über ausreichende Berechtigungen für die gewünschte Aktion. Von der Clientanwendung müssen Fehler, die vom Tokenausstellungsendpunkt zurückgegeben werden, erwartet und richtig behandelt werden.

Wenn Sie eine Antwort mit einem Aktualisierungstokenfehler erhalten, sollten Sie das aktuelle Aktualisierungstoken verwerfen und einen neuen Autorisierungscode oder ein neues Zugriffstoken anfordern. Wenn Sie ein Aktualisierungstoken im Ablauf der Autorisierungscodeerteilung verwenden und eine Antwort mit dem Fehlercode `interaction_required` oder `invalid_grant` erhalten, sollten Sie besonders darauf achten, dass Sie das Aktualisierungstoken verwerfen und einen neuen Autorisierungscode anfordern.

## Überprüfen des resource\_id-Parameters

Die Clientanwendung muss in der Antwort den Wert des `resource_id`-Parameters überprüfen. Andernfalls ist es für einen schädlichen Dienst unter Umständen möglich, einen Angriff mit einer Erhöhung von Rechten durchzuführen.

 Die empfohlene Strategie zur Verhinderung eines Angriffs besteht darin sicherzustellen, dass die „resource\_id“ mit dem Basiselement der Web-API-URL übereinstimmt, auf die zugegriffen wird. Wenn auf https://service.contoso.com/data zugegriffen wird, kann `resource_id` beispielsweise https://service.contoso.com/ lauten. Die Clientanwendung muss eine `resource_id` ablehnen, die nicht mit der Basis-URL beginnt, sofern es kein zuverlässiges alternatives Verfahren zum Überprüfen der ID gibt.

## Tokenausstellung und Wiederholungsrichtlinien

Azure AD unterstützt mehrere Tokenausstellungsendpunkte, die von Clients abgefragt werden können. Verwenden Sie die folgenden Richtlinien zum Implementieren von Wiederholungslogik für diese Endpunkte, um einen unerwarteten Netzwerk- oder Serverausfall verarbeiten zu können.

Wenn bei Ausfällen auf Wiederholungen geantwortet wird, wird normalerweise ein Fehlercode der HTTP 500-Serie für eine Anforderung an einen Azure AD-Endpunkt zurückgegeben. In einigen Fällen ist der Client eine Anwendung oder ein Dienst, von der bzw. dem automatische Anforderungen an Azure AD gesendet werden. In anderen Fällen, z.B. dem webbasierten Verbund mit dem WS-Verbundprotokoll, ist der Client ein Webbrowser, und der Endbenutzer muss den Wiederholungsversuch manuell durchführen.

### Implementieren von Wiederholungslogik basierend auf Fehlerantworten der HTTP 500-Serie

Die Verwendung von Wiederholungslogik wird dringend empfohlen, wenn Active Directory Access Control Service (ACS) Fehler der HTTP 500-Serie zurückgibt. Dies umfasst:

- HTTP-Fehler 500 – Interner Serverfehler
- HTTP-Fehler 502 – Fehlerhaftes Gateway
- HTTP-Fehler 503 – Dienst nicht verfügbar
- HTTP-Fehler 504 – Gatewaytimeout

In der Wiederholungslogik können einzelne HTTP-Codes explizit aufgelistet werden, aber es ist ausreichend, die Wiederholungslogik aufzurufen, wenn ein Fehler der HTTP 500-Serie zurückgegeben wird.

Wiederholungslogik ist normalerweise nicht zu empfehlen, wenn Fehlercodes der HTTP 400-Serie zurückgegeben werden. Eine Fehlerantwort der HTTP 400-Serie von ACS bedeutet, dass die Anforderung ungültig ist und überarbeitet werden muss.

Wiederholungslogik sollte von HTTP-Fehlercodes ausgelöst werden, z.B. HTTP 504 (Zeitüberschreitung des externen Servers) oder HTTP 500, und nicht von ACS-Fehlercodes, z.B. ACS90005. ACS-Fehlercodes werden nur zu Informationszwecken bereitgestellt und können Änderungen unterliegen.

### Für Wiederholungsversuche sollte ein Backoff-Timer verwendet werden, um eine optimale Flusssteuerung zu erzielen.

Wenn ein Client einen Fehler der HTTP 500-Serie empfängt, sollte der Client warten, bevor er versucht, die Anforderung zu wiederholen. Die besten Ergebnisse erzielen Sie, wenn Sie diesen Zeitraum für jeden nachfolgenden Wiederholungsversuch verlängern. Bei diesem Ansatz werden vorübergehende Fehler schnell behoben, und die Anforderungsrate für vorübergehende Netzwerk- oder Serverprobleme mit längerer Behebungsdauer wird optimiert.

Verwenden Sie beispielsweise einen exponentiellen Backoff-Timer, bei dem die Verzögerung vor der Wiederholung mit jeder Instanz exponentiell erhöht wird, z.B. 1. Wiederholungsversuch: 1 Sekunde, 2. Wiederholungsversuch: 2 Sekunden, 3. Wiederholungsversuch: 4 Sekunden usw.

Passen Sie die Anzahl von Wiederholungsversuchen und den Zeitraum zwischen jedem Wiederholungsversuch je nach Ihren Benutzeranforderungen an. Wir empfehlen Ihnen aber bis zu fünf Wiederholungsversuche über einen Zeitraum von fünf Minuten. Die Behebung von Ausfällen, die durch ein Timeout verursacht werden, dauert länger.

## Nächste Schritte

[Azure Active Directory-Authentifizierungsbibliotheken (ADAL)](active-directory-authentication-libraries.md)

<!---HONumber=AcomDC_0601_2016-->