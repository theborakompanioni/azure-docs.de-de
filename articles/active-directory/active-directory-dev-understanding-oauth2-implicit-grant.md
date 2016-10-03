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
   ms.date="08/17/2016"
   ms.author="vittorib;bryanla"/>

# Grundlegendes zum Ablauf der impliziten OAuth2-Gewährung in Azure Active Directory (AD)

Die implizite OAuth2-Gewährung ist dafür bekannt, dass sie über die längste Liste der Sicherheitsbedenken in der OAuth2-Spezifikation verfügt. Dennoch wird diese von ADAL JS implementierte Methode für die Erstellung von SPA-Anwendungen empfohlen. Woran liegt das? Alles hat Vor- und Nachteile, und die implizite Gewährung ist die beste Methode im Fall von Anwendungen, die eine Web-API über JavaScript in einem Browser nutzen.

## Was ist die implizite OAuth2-Gewährung?

Die [OAuth2-Autorisierungscodegewährung](https://tools.ietf.org/html/rfc6749#section-1.3.1) ist die Autorisierungsgewährung, die zwei separate Endpunkte nutzt. Der Autorisierungsendpunkt wird für die Benutzerinteraktionsphase verwendet, was zu einem Autorisierungscode führt. Der Tokenendpunkt wird anschließend vom Client verwendet, um den Code gegen ein Zugriffstoken (und häufig auch gegen ein Aktualisierungstoken) einzutauschen. Webanwendungen sind erforderlich, um dem Tokenendpunkt ihre eigenen Anwendungsanmeldeinformationen anzuzeigen, damit der Autorisierungsserver den Client authentifizieren kann.

Die [implizite OAuth2-Gewährung](https://tools.ietf.org/html/rfc6749#section-1.3.2) ist eine Variante, bei der ein Client ein Zugriffstoken (und im Fall von [OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) ein ID-Token) direkt vom Autorisierungsendpunkt abrufen kann, ohne den Tokenendpunkt zu kontaktieren oder die Clientanwendung zu authentifizieren. Diese Variante wurde speziell für JavaScript-basierte Anwendungen entwickelt, die in einem Webbrowser ausgeführt werden: In der ursprünglichen OAuth2-Spezifikation werden Token in einem URI-Fragment zurückgegeben. Dadurch sind die Token-Bits für den JavaScript-Code im Client verfügbar, werden jedoch nicht in Umleitungen an den Server einbezogen. Beim Zurückgeben von Token über den Browser erfolgt eine direkte Umleitung vom Autorisierungsendpunkt. Dies hat auch den Vorteil, dass keine ursprungsübergreifenden Aufrufe mehr benötigt werden. Diese sind erforderlich, wenn die JavaScript-Anwendung zum Kontaktieren des Tokenendpunkts benötigt wird.

Ein wichtiges Merkmal der impliziten OAuth2-Gewährung besteht darin, dass solche Abläufe keine Aktualisierungstoken an den Client zurückgeben. Wie wir im nächsten Abschnitt sehen werden, ist dies nicht unbedingt erforderlich und würde zudem ein Sicherheitsrisiko darstellen.

## Geeignete Szenarien für die implizite OAuth2-Gewährung

Gemäß Deklaration der OAuth2-Spezifikation wurde die implizite Gewährung entwickelt, um die Verwendung von Benutzer-Agent-Anwendungen (JavaScript-Anwendungen, die in einem Browser ausgeführt werden) zu ermöglichen. Solcher Anwendungen zeichnen sich insbesondere dadurch aus, dass für den Zugriff auf Serverressourcen (üblicherweise eine Web-API) und für die entsprechende Aktualisierung der Benutzeroberfläche der Anwendung JavaScript-Code verwendet wird. Denken Sie dabei z.B. an Anwendungen wie Gmail oder Outlook Web Access: Wenn Sie eine Nachricht aus Ihrem Posteingang auswählen, ändert sich nur der Bereich, in dem die Nachricht dargestellt wird, während der Rest der Seite unverändert bleibt. Im Gegensatz dazu führt bei traditionellen, auf Umleitungen basierenden Web-Apps jede Benutzerinteraktion zu einem ganzseitigen Postback sowie zum ganzseitigen Rendern der neuen Serverantwort.

Extrembeispiele für Anwendungen mit der JavaScript-basierten Methode sind die so genannten Single-Page-Anwendungen (SPAs): Diese Anwendungen sind nur für die Versorgung einer HTML-Startseite und des dazugehörigen JavaScript-Codes konzipiert. Alle nachfolgenden Interaktionen werden mithilfe von Web-API-Aufrufen über JavaScript ausgeführt. Hybride Methoden, bei denen Anwendungen größtenteils Postback-orientiert sind, aber auch einzelne JS-Aufrufe ausführen, sind allerdings nichts Ungewöhnliches, und die Informationen zur Verwendung impliziter Abläufe sind auch für diese Methoden relevant.

Auf Umleitungen basierende Anwendungen schützen ihre Anforderungen in der Regel mithilfe von Cookies. Bei JavaScript-Anwendungen funktioniert diese Methode jedoch nicht so gut. Cookies können nur für die Domäne verwendet werden, für die sie erstellt wurden. JavaScript-Aufrufe können dagegen an andere Domänen weitergeleitet werden. Das kommt in der Praxis auch recht häufig vor – etwa bei Anwendungen, die die Microsoft Graph-API, Office-API oder Azure-API aufrufen: Diese befinden sich alle außerhalb der Domäne, von der aus die Anwendung bereitgestellt wird. Bei JavaScript-Anwendungen geht der Trend zunehmend dahin, dass kein Back-End vorhanden ist und sie zur Implementierung der Geschäftsfunktion zu 100% auf Web-APIs basieren.

Aktuell wird zum Schutz von Web-API-Aufrufen die Verwendung der OAuth2-Bearertoken-Methode empfohlen. Bei dieser Methode wird jeder Aufruf von einem OAuth2-Zugriffstoken begleitet. Die Web-API untersucht das eingehende Zugriffstoken und gewährt dem angeforderten Vorgang Zugriff, wenn das Token in den erforderlichen Bereichen gefunden wurde. Der implizite Ablauf ist eine praktische Methode, mit der JavaScript-Anwendungen Zugriffstoken für eine Web-API beziehen können. Gegenüber Cookies bietet er diverse Vorteile:

- Token können zuverlässig abgerufen werden, ohne dass ursprungsübergreifende Aufrufe erforderlich sind. Die obligatorische Registrierung des Umleitungs-URI, an den Token zurückgegeben werden, stellt sicher, dass Token nicht verschoben werden.
- JavaScript-Anwendungen können beliebig viele Zugriffstoken für beliebig viele Web-APIs beziehen. Hierbei gelten keine Domänenbeschränkungen.
- HTML5-Features wie Sitzungen oder lokale Speicher ermöglichen es, das Zwischenspeichern von Token und die Lebensdauerverwalltung vollständig zu steuern, wobei die Verwaltung von Cookies für die App nicht transparent ist.
- Zugriffstoken sind für websiteübergreifende Anforderungsfälschungen (CSRF) nicht anfällig.

Bei der impliziten Gewährung werden insbesondere aus Sicherheitsgründen keine Aktualisierungstoken ausgegeben. Ein Aktualisierungstoken hat einen breiteren Umfang als Zugriffstoken und gewährt mehr Rechte, weshalb der Schaden bei Verlust auch größer sein kann. Beim impliziten Ablauf werden Token in der URL übermittelt, weshalb die Gefahr eines Abfangens höher ist als bei der Gewährung über einen Autorisierungscode.

JavaScript-Anwendungen verfügen jedoch über andere Möglichkeiten, um Zugriffstoken zu erneuern, bei denen der Benutzer nicht wiederholt zur Eingabe der Anmeldeinformationen aufgefordert wird. Zum Ausführen neuer Tokenanforderungen in Bezug auf den Autorisierungsendpunkt von Azure AD kann die Anwendung ein ausgeblendetes iFrame verwenden: Wenn der Browser über eine aktive Sitzung mit der Azure AD-Domäne (d.h. über ein Sitzungscookie) verfügt, kann die Authentifizierungsanforderung ausgeführt werden, ohne dass eine Benutzerinteraktion erforderlich ist.

Bei diesem Modell kann die JavaScript-Anwendung Zugriffstoken unabhängig erneuern und sogar neue Token für eine neue API beziehen (sofern der Benutzer dem vorher zugestimmt hat). Dadurch entfällt das zusätzliche Beziehen, Verwalten und Schützen eines hochwertigen Artefakts (etwa ein Aktualisierungstoken). Das Azure AD-Sitzungscookie, das die Erneuerung im Hintergrund ermöglicht, wird außerhalb der Anwendung verwaltet. Ein weiterer Vorteil dieser Methode: Ein Benutzer kann sich über eine beliebige, bei Azure AD angemeldete Anwendung, die auf einer beliebigen Browserregisterkarte ausgeführt wird, von Azure AD abmelden. Daraufhin wird das Azure AD-Sitzungscookie gelöscht, sodass die JavaScript-Anwendung für den abgemeldeten Benutzer keine Token mehr erneuern kann.

## Eignet sich die implizite Gewährung für meine App?

Die implizite Gewährung beinhaltet mehr Risiken als andere Methoden. Die Bereiche, die Sie beachten müssen, sind gut dokumentiert (siehe z.B. [Misuse of Access Token to Impersonate Resource Owner in Implicit Flow][OAuth2-Spec-Implicit-Misuse] \(Missbräuchliche Verwendung von Zugriffstoken zum Annehmen der Identität von Ressourcenbesitzern in impliziten Abläufen) und [OAuth 2.0 Threat Model and Security Considerations][OAuth2-Threat-Model-And-Security-Implications] \(OAuth 2.0-Bedrohungsmodell und Sicherheitsüberlegungen). Das höhere Risiko liegt größtenteils darin begründet, dass Anwendungen aktiviert werden, die aktiven Code ausführen, der von einer Remoteressource direkt für einen Browser bereitgestellt wird. Wenn Sie eine SPA-Architektur planen, über keine Back-End-Komponenten verfügen oder eine Web-API über JavaScript aufrufen möchten, empfiehlt sich die Verwendung des impliziten Ablaufs für den Tokenabruf.

Falls es sich bei Ihrer Anwendung dagegen um einen nativen Client handelt, ist der implizite Ablauf weniger empfehlenswert. Da bei nativen Clients keine Azure AD-Sitzungscookies vorhanden sind, hat Ihre Anwendung keine Möglichkeit, eine Sitzung mit langer Laufzeit aufrechtzuerhalten. Das bedeutet, dass Ihre Anwendung beim Abrufen von Zugriffstoken für neue Ressourcen immer wieder Eingabeaufforderung an den Benutzer richtet.

Auch wenn Sie eine Webanwendung mit einem Back-End entwickeln, die eine API über den Back-End-Code nutzen soll, ist der implizite Ablauf nicht die beste Wahl. Bei anderen Methoden haben Sie deutlich mehr Steuerungsmöglichkeiten. Bei der OAuth2-Gewährung mit Clientanmeldeinformationen haben Sie beispielsweise die Möglichkeit, Token zu beziehen, die die Berechtigungen für die Anwendung widerspiegeln (im Gegensatz zu Benutzerdelegierungen). Das bedeutet unter anderem, dass der Client den programmgesteuerten Zugriff auf Ressourcen verwalten kann, auch wenn ein Benutzer nicht aktiv in eine Sitzung eingebunden ist. Darüber hinaus gewährleisten Gewährungen dieser Art eine höhere Sicherheit. So durchlaufen Zugriffstoken etwa niemals den Browser des Benutzers und können dadurch gar nicht erst im Browserverlauf gespeichert werden. Die Clientanwendung kann außerdem beim Anfordern eines Tokens eine strenge Authentifizierung durchführen.

## Nächste Schritte

- Eine vollständige Liste mit Ressourcen für Entwickler (einschließlich Referenzinformationen zu den Protokollen und zur Unterstützung der Abläufe für die OAuth2-Autorisierungsberechtigungen durch Azure AD) finden Sie im [Entwicklerhandbuch zu Azure Active Directory][AAD-Developers-Guide].
- Weitere Informationen zur Anwendungsintegration finden Sie unter [Integration in Azure Active Directory][ACOM-How-To-Integrate].

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

<!---HONumber=AcomDC_0817_2016-->