<properties
   pageTitle="Grundlegendes zum Ablauf der impliziten OAuth2-Gewährung in Azure Active Directory | Microsoft Azure"
   description="Erhalten Sie weitere Informationen zur Implementierung des Ablaufs der impliziten OAuth2-Gewährung durch Azure Active Directory und dazu, ob dies die richtige Methode für Ihre Anwendung ist."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="vibronet"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/01/2016"
   ms.author="vittorib;bryanla"/>

# Grundlegendes zum Ablauf der impliziten OAuth2-Gewährung in Azure Active Directory (AD)

Die implizite OAuth2-Gewährung ist dafür bekannt, dass sie über die längste Liste der Sicherheitsbedenken in der OAuth2-Spezifikation verfügt. Dennoch wird diese von ADAL JS implementierte Methode für die Erstellung von SPA-Anwendungen empfohlen. Woran liegt das? Alles hat Vor- und Nachteile, und die implizite Gewährung ist die beste Methode im Fall von Anwendungen, die eine Web-API über JavaScript in einem Browser nutzen.

## Was ist die implizite OAuth2-Gewährung?

Die [OAuth2-Autorisierungscodegewährung](https://tools.ietf.org/html/rfc6749#section-1.3.1) ist die Autorisierungsgewährung, die zwei separate Endpunkte nutzt. Der Autorisierungsendpunkt wird für die Benutzerinteraktionsphase verwendet und gibt einen Autorisierungscode zurück. Der Tokenendpunkt wird vom Client genutzt, um den Code für ein Zugriffstoken und häufig auch für ein Aktualisierungstoken auszutauschen. Webanwendungen sind erforderlich, um dem Tokenendpunkt ihre eigenen Anwendungsanmeldeinformationen anzuzeigen, damit der Autorisierungsserver den Client authentifizieren kann.

Die [implizite OAuth2-Gewährung](https://tools.ietf.org/html/rfc6749#section-1.3.2) wird als Variante definiert, in der ein Client ein Zugriffstoken (und im Fall von [OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) ein id\_token) direkt vom Autorisierungsendpunkt abrufen kann, ohne dass der Tokenendpunkt kontaktiert oder die Clientanwendung authentifiziert werden muss. Diese Variante wurde speziell für JavaScript-basierte Apps entwickelt, die in einem Webbrowser ausgeführt werden: In der Original-OAuth2-Spezifikation werden Token in einem URI-Fragment zurückgegeben. Dadurch sind die Token-Bits für den JavaScript-Code im Client verfügbar, werden jedoch nicht in Umleitungen an den Server einbezogen. Das Zurückgeben von Token über Browserumleitungen direkt von den Autorisierungsendpunkten hat auch den Vorteil, dass ursprungsübergreifende Aufrufe nicht mehr erforderlich sind. Dies wäre der Fall, wenn die JavaScript-App zum Kontaktieren des Tokenendpunkts notwendig ist.

Ein wichtiges Merkmal der impliziten OAuth2-Gewährung besteht darin, dass solche Abläufe keine Aktualisierungstoken an den Client zurückgeben. Wie wir im nächsten Abschnitt sehen werden, ist dies nicht unbedingt erforderlich und würde zudem ein Sicherheitsrisiko darstellen.

## Geeignete Szenarien für die implizite OAuth2-Gewährung

Wie von der OAuth2-Spezifikation deklariert, wurde die implizite Gewährung entworfen, um die Benutzer-Agent-Anwendungen, d.h. JavaScript-Apps, die in einem Browser ausgeführt werden, zu ermöglichen. Das entscheidende Merkmal solcher Apps besteht darin, dass JavaScript-Code für den Zugriff auf Serverressourcen (i.d.R. eine Web-API) und für die entsprechende Aktualisierung der Benutzeroberfläche der Anwendung verwendet wird. Denken Sie dabei z.B. an Anwendungen wie Gmail oder Outlook Web Access: Wenn Sie eine Nachricht aus Ihrem Posteingang auswählen, ändert sich nur der Bereich, in dem die Nachricht dargestellt wird, während der Rest der Seite unverändert bleibt. Im Gegensatz dazu führt bei traditionellen, auf Umleitungen basierenden Web-Apps jede Benutzerinteraktion zu einem ganzseitigen Postback sowie im ganzseitigen Rendern der neuen Serverantwort.

Extreme Beispiele solcher Anwendungen, die die JavaScript-basierte Methode einsetzen, sind die so genannten Single-Page-Anwendungen (SPAs): Diese Apps dienen nur als anfängliche HTML-Seite und zugeordnetes JavaScript, wobei alle nachfolgenden Interaktionen von über JavaScript ausgeführten Web-API-Aufrufen ausgeführt werden. Hybride Methoden, bei denen Apps größtenteils Postback-orientiert sind, für einige Umgebungen jedoch einzelne JS-Aufrufe ausführen, kommen jedoch auch häufig vor, und die Informationen über die Verwendung impliziter Abläufe sind auch für sie relevant.

Auf Umleitungen basierende Anwendungen sichern ihre Anforderungen in der Regel mithilfe von Cookies. Bei JavaScript-Anwendungen funktioniert diese Methode jedoch nicht so gut, da Cookies nur für die Domäne funktionieren, für die sie erstellt wurden, während JavaScript-Aufrufe an andere Domänen weitergeleitet werden. Und dies ist häufig der Fall, wie z.B. bei Anwendungen, die Microsoft Graph-API, Office-API oder Azure-API aufrufen: Diese Anwendungen befinden sich außerhalb der Domäne, von der die App bereitgestellt wird. Bei JavaScript-Anwendungen geht der Trend zunehmend dahin, dass kein Back-End vorhanden ist und sie zur Implementierung der Geschäftsfunktion zu 100% auf Web-APIs basieren.

Die aktuell bevorzugte Methode zum Schützen von Web-API-Aufrufen besteht in der OAuth2-Bearertoken-Methode. Dabei erfolgt jeder Aufruf gemeinsam mit einem OAuth2-Zugriffstoken, die Web-API untersucht das eingehende Zugriffstoken und gewährt dem angeforderten Vorgang Zugriff, wenn er in den erforderlichen Bereichen gefunden wurde. Der implizite Ablauf ist eine praktische Möglichkeit, mit der JavaScript-Apps Zugriffstoken für eine Web-API abrufen können. Gegenüber Cookies bietet er diverse Vorteile:

- Token können zuverlässig abgerufen werden, ohne dass ursprungsübergreifende Aufrufe erforderlich sind. Die obligatorische Registrierung des Umleitungs-URI, an den Token zurückgegeben werden, stellt sicher, dass Token nicht verschoben werden.
- JavaScript-Apps können beliebig viele Zugriffstoken für beliebig viele Web-APIs abrufen – ohne Domänenbeschränkungen.
- HTML5-Features wie Sitzungen oder lokale Speicher ermöglichen es, das Zwischenspeichern von Token und die Lebensdauerverwalltung vollständig zu steuern, wobei die Verwaltung von Cookies für die App nicht transparent ist.
- Zugriffstoken sind für websiteübergreifende Anforderungsfälschungen (CSRF) nicht anfällig.

Bei der impliziten Gewährung werden insbesondere aus Sicherheitsgründen keine Aktualisierungstoken ausgegeben. Ein Aktualisierungstoken hat einen breiteren Umfang als Zugriffstoken und gewährt mehr Rechte, weshalb der Schaden bei Verlust auch größer sein kann. Beim impliziten Ablauf werden Token in der URL übermittelt, weshalb die Gefahr eines Abfangens höher ist als bei der Gewährung über einen Autorisierungscode.

JavaScript-Anwendungen verfügen jedoch über andere Möglichkeiten, um Zugriffstoken zu erneuern, bei denen der Benutzer nicht wiederholt zur Eingabe der Anmeldeinformationen aufgefordert wird. Zum Ausführen neuer Tokenanforderungen in Bezug auf den Autorisierungsendpunkt von Azure AD kann die Anwendung ein ausgeblendetes iFrame verwenden: Wenn der Browser über eine aktive Sitzung mit der Azure AD-Domäne (d.h. über ein Sitzungscookie) verfügt, kann die Authentifizierungsanforderung ausgeführt werden, ohne dass eine Benutzerinteraktion erforderlich ist.

Bei diesem Modell hat die JavaScript-App die Möglichkeit, Zugriffstoken unabhängig zu erneuern und für eine neue API neue Token abzurufen (sofern der Benutzer dem zuvor zugestimmt hat), ohne dass Artefakte mit hohem Wert wie Aktualisierungstoken zusätzlich abgerufen, verwaltet und geschützt werden müssen. Das Azure AD-Sitzungscookie, das die Erneuerung im Hintergrund ermöglicht, wird außerhalb der Anwendung verwaltet. Ein weiterer Vorteil dieser Methode besteht in Folgendem: Wenn sich ein Benutzer mit einer Anwendung, die mithilfe von Azure AD in einer Browserregisterkarte ausgeführt wird, von Azure AD abmeldet, wird das Azure AD-Sitzungscookie gelöscht, und die JavaScript-App hat automatisch nicht mehr die Möglichkeit, Token für den abgemeldeten Benutzer zu erneuern.

## Eignet sich die implizite Gewährung für meine App?

Die implizite Gewährung beinhaltet mehr Risiken als andere Methoden. Die Bereiche, die Sie beachten müssen, sind gut dokumentiert (siehe z.B. [Misuse of Access Token to Impersonate Resource Owner in Implicit Flow][OAuth2-Spec-Implicit-Misuse] (Missbräuchliche Verwendung von Zugriffstoken zum Annehmen der Identität von Ressourcenbesitzern in impliziten Abläufen) und [OAuth 2.0 Threat Model and Security Considerations][OAuth2-Threat-Model-And-Security-Implications] (OAuth 2.0-Bedrohungsmodell und Sicherheitsüberlegungen). Das höhere Risiko liegt größtenteils darin begründet, dass Anwendungen aktiviert werden, die aktiven Code ausführen, der von einer Remoteressource direkt für einen Browser bereitgestellt wird. Bei einer SPA-Architektur, wenn Sie über keine Back-End-Komponenten verfügen oder wenn Sie eine Web-API über JavaScript aufrufen möchten, wird der implizite Ablauf für den Tokenabruf empfohlen.

Wenn Ihre Anwendung ein nativer Client ist, ist der implizite Ablauf nicht sonderlich gut geeignet: Da bei nativen Clients keine Azure AD-Sitzungscookies vorhanden sind, gibt es bei Ihrer App keine Möglichkeit, um Sitzungen mit langer Lebensdauer zu verwalten und Zugriffstoken für neue Ressourcen ohne wiederholte Benutzeraufforderungen abzurufen.

Wenn Sie eine Webanwendung mit einem Back-End entwickeln, die API über den Back-End-Code nutzen soll, ist der implizite Ablauf ebenfalls nicht erste Wahl. Bei anderen Methoden erhalten Sie mehr Rechte: Bei der OAuth2-Clientanmeldegewährung haben Sie beispielsweise die Möglichkeit, Token abzurufen, die den Berechtigungen für die App entsprechen (im Gegensatz zur Benutzerdelegierung), oder programmatischen Zugriff auf Ressourcen zu verwalten, auch wenn ein Benutzer in eine Sitzung nicht aktiv eingebunden ist usw. Außerdem wird bei solchen Gewährungen eine größere Sicherheit gewährleistet, da Zugriffstoken den Browser des Benutzers nie durchlaufen und somit nicht im Browserverlauf gespeichert werden können, die Clientanwendung beim Anfordern eines Tokens eine strenge Authentifizierung ausführen kann usw.

## Nächste Schritte

- Eine vollständige Liste der Ressourcen für Entwickler, einschließlich Referenzinformationen zu allen Protokollen und zur Unterstützung der Abläufe für die OAuth2-Autorisierungsberechtigungen durch Azure AD finden Sie im [Entwicklerhandbuch zu Azure Active Directory][AAD-Developers-Guide].
- Weitere Informationen zur Anwendungsintegration finden Sie unter [Integration in Azure Active Directory][ACOM-How-To-Integrate].

<!--Image references-->
[Scenario-Topology]: ./media/active-directory-devhowto-auth-using-any-aad/multi-tenant-aad-components.png

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

<!---HONumber=AcomDC_0601_2016-->