---
title: Erstellen einer Anwendung, die beliebige Azure Active Directory-Benutzer anmelden kann | Microsoft Docs
description: Ausführliche Anweisungen zum Erstellen einer Anwendung, die einen Benutzer über jeden Azure Active Directory-Mandanten anmelden kann. Eine solche Anwendung wird auch als mehrinstanzenfähige Anwendung bezeichnet.
services: active-directory
documentationcenter: ''
author: skwan
manager: mbaldwin
editor: ''

ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/25/2016
ms.author: skwan;bryanla

---
# Anmelden von Azure Active Directory-Benutzern (AD) mit dem mehrinstanzenfähigen Anwendungsmuster
Wenn Sie vielen Organisationen eine Software-as-a-Service-Anwendung anbieten, können Sie Ihre Anwendung so konfigurieren, dass Anmeldungen von beliebigen Azure AD-Mandanten akzeptiert werden. In Azure AD wird eine solche Anwendung als mehrinstanzenfähig bezeichnet. Benutzer eines Azure AD-Mandanten können sich bei Ihrer Anwendung anmelden, nachdem sie zugestimmt haben, ihr Konto mit Ihrer Anwendung zu verwenden.

Wenn Sie eine vorhandene Anwendung haben, die über ein eigenes Kontosystem verfügt oder andere Arten von Anmeldungen über andere Cloudanbieter unterstützt, müssen Sie für das Hinzufügen einer Azure AD-Anmeldung von einem beliebigen Mandanten nur folgende Schritte ausführen: Registrieren Ihrer App, Hinzufügen von Anmeldecode über OAuth2, OpenID Connect oder SAML und Einbinden einer Schaltfläche „Bei Microsoft anmelden“ in die Anwendung. Klicken Sie auf die unten angegebene Schaltfläche, um weitere Informationen zum Branding Ihrer Anwendung zu erhalten.

[![Anmeldeschaltfläche][AAD-Sign-In]][AAD-App-Branding]

In diesem Artikel wird davon ausgegangen, dass Sie mit der Erstellung einer Anwendung für einen Mandanten für Azure AD bereits vertraut sind. Besuchen Sie andernfalls die [Startseite des Entwicklerhandbuchs][AAD-Dev-Guide], und nutzen Sie unsere Schnellstartanleitungen.

Sie müssen vier einfache Schritte zum Konvertieren der Anwendung in eine mehrinstanzenfähige Azure AD-App ausführen:

1. Aktualisieren der Anwendungsregistrierung, sodass sie mehrinstanzenfähig ist
2. Aktualisieren des Codes zum Senden von Anforderungen an den Endpunkt „/common“
3. Aktualisieren des Codes zum Verarbeiten mehrerer Ausstellerwerte
4. Interpretieren der Benutzer- und Administratorzustimmung und Vornehmen der entsprechenden Codeänderungen

Betrachten wir jeden Schritt im Detail. Sie können auch direkt zu [dieser Liste von Beispielen für Mehrinstanzenfähigkeit][AAD-Samples-MT] wechseln.

## Aktualisieren der Registrierung, sodass sie mehrinstanzenfähig ist
Standardmäßig gelten Web-App-/API-Registrierungen in Azure AD für einen einzelnen Mandanten. Sie können die Registrierung mehrinstanzenfähig machen, indem Sie die Option „Die Anwendung ist mehrinstanzenfähig“ auf der Konfigurationsseite der Anwendungsregistrierung im [klassischen Azure-Portal][AZURE-classic-portal] auf „Ja“ festlegen.

Hinweis: Damit eine Anwendung mehrinstanzenfähig sein kann, muss der App-ID-URI der Anwendung in Azure AD global eindeutig sein. Der App-ID-URI ist eine der Methoden, mit der eine Anwendung in Protokollmeldungen identifiziert wird. Bei einer App für einen Mandanten muss der App-ID-URI nur in diesem Mandanten eindeutig sein. Bei einer mehrinstanzenfähigen Anwendung muss er global eindeutig sein, damit Azure AD die Anwendung in allen Mandanten finden kann. Globale Eindeutigkeit wird durch die Anforderung erzwungen, dass der App-ID-URI einen Hostnamen aufweisen muss, der mit einer überprüften Domäne des Azure AD-Mandanten übereinstimmt. Wenn der Name Ihres Mandanten beispielsweise „contoso.onmicrosoft.com“ lautet, wäre `https://contoso.onmicrosoft.com/myapp` ein gültiger App-ID-URI. Enthält Ihr Mandant die überprüfte Domäne `contoso.com`, wäre `https://contoso.com/myapp` auch ein gültiger App-ID-URI. Das Festlegen einer Anwendung als mehrinstanzenfähig schlägt fehl, wenn der App-ID-URI nicht diesem Muster folgt.

Native Clientregistrierungen sind standardmäßig mehrinstanzenfähig. Sie müssen keine Maßnahmen ergreifen, um eine native Clientanwendungsregistrierung mehrinstanzenfähig zu machen.

## Aktualisieren des Codes zum Senden von Anforderungen an „/common“
Bei einer Anwendung für einen Mandanten werden Anmeldeanforderungen an den Anmeldeendpunkt des Mandanten gesendet. Für „contoso.onmicrosoft.com“ wäre der Endpunkt z.B.:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Mit Anforderungen, die an den Endpunkt eines Mandanten gesendet werden, können Benutzer (oder Gäste) in diesem Mandanten für Anwendungen dieses Mandanten angemeldet werden. Bei einer mehrinstanzenfähigen Anwendung weiß die Anwendung zunächst nicht, von welchem Mandanten der Benutzer stammt. Deshalb können Sie keine Anforderungen an den Endpunkt eines Mandanten senden. Stattdessen werden Anforderungen an einen Endpunkt gesendet, der sie per Multiplex an alle Azure AD-Mandanten verteilt:

    https://login.microsoftonline.com/common

Wenn Azure AD eine Anforderung am Endpunkt „/common“ erhält , wird der Benutzer angemeldet, und dadurch wird der Mandant ermittelt, von dem der Benutzer stammt. Der Endpunkt „/common“ funktioniert mit allen von Azure AD unterstützten Authentifizierungsprotokollen: OpenID Connect, OAuth 2.0, SAML 2.0 und WS-Verbund.

Die Anmeldeantwort an die Anwendung enthält dann ein Token, das den Benutzer darstellt. Anhand des Ausstellerwerts im Token erfährt eine Anwendung, von welchem Mandanten der Benutzer stammt. Wenn eine Antwort vom Endpunkt „/common“ zurückgegeben wird, entspricht der Ausstellerwert im Token dem Mandanten des Benutzers. Sie müssen beachten, dass der Endpunkt „/common“ kein Mandant und kein Aussteller ist, sondern lediglich ein Multiplexer. Bei Verwendung von „/common“ muss die Logik zum Überprüfen von Token in Ihrer Anwendung aktualisiert werden, um dies zu berücksichtigen.

Wie bereits erwähnt, sollte von mehrinstanzenfähigen Anwendungen auch eine einheitliche Anmeldeoberfläche für Benutzer bereitgestellt werden, die die Brandingrichtlinien für Azure AD-Anwendungen erfüllt. Klicken Sie auf die unten angegebene Schaltfläche, um weitere Informationen zum Branding Ihrer Anwendung zu erhalten.

[![Anmeldeschaltfläche][AAD-Sign-In]][AAD-App-Branding]

Wir sehen uns nun die Nutzung des Endpunkts „/common“ und Ihre Codeimplementierung genauer an.

## Aktualisieren des Codes zum Verarbeiten mehrerer Ausstellerwerte
Webanwendungen und Web-APIs empfangen und überprüfen Token von Azure AD.

> [!NOTE]
> Wenn native Clientanwendungen Token von Azure AD anfordern und empfangen, werden diese zur Überprüfung an APIs weitergeleitet. Native Anwendungen überprüfen Token nicht und müssen sie als nicht transparent behandeln.
> 
> 

Sehen wir uns an, wie eine Anwendung Token überprüft, die sie von Azure AD erhält. Eine Anwendung für einen Mandanten verwendet normalerweise einen Endpunktwert wie:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Daraus wird eine Metadaten-URL (in diesem Fall OpenID Connect) wie die folgende erstellt:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

Damit können zwei wichtige Informationen heruntergeladen werden, die zum Überprüfen von Token verwendet werden: die Signaturschlüssel und der Ausstellerwert des Mandanten. Jeder Azure AD-Mandant weist einen eindeutigen Ausstellerwert in folgender Form auf:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

Dabei ist der GUID-Wert die nicht änderbare Version der Mandanten-ID des Mandanten. Wenn Sie oben auf den Metadatenlink für `contoso.onmicrosoft.com` klicken, sehen Sie diesen Ausstellerwert im Dokument.

Wenn eine Anwendung für einen Mandanten ein Token überprüft, wird die Signatur des Tokens anhand der Signaturschlüssel aus dem Metadatendokument überprüft, und es wird sichergestellt, dass der Ausstellerwert im Token dem Wert entspricht, der im Metadatendokument gefunden wurde.

Da der Endpunkt „/common“ keinem Mandanten entspricht und kein Aussteller ist, hat der Ausstellerwert in den Metadaten für „/common“ eine Vorlagen-URL statt eines tatsächlichen Werts:

    https://sts.windows.net/{tenantid}/

Aus diesem Grund kann eine mehrinstanzenfähige Anwendung keine Token überprüfen, indem einfach der Ausstellerwert in den Metadaten mit dem `issuer`-Wert im Token abgeglichen wird. Eine mehrinstanzenfähige Anwendung benötigt Logik, um basierend auf der Mandanten-ID im Ausstellerwert zu entscheiden, welche Ausstellerwerte zulässig sind und welche nicht.

Wenn eine mehrinstanzenfähige Anwendung z.B. nur Anmeldungen von bestimmten Mandanten zulässt, die sich für den Dienst registriert haben, muss der Ausstellerwert oder der `tid`-Anspruchswert im Token überprüft werden. So wird sichergestellt, dass dieser Mandant in der Liste der Abonnenten enthalten ist. Wenn eine mehrinstanzenfähige Anwendung nur von Personen genutzt wird und keine Zugriffsentscheidungen basierend auf Mandanten trifft, kann der Ausstellerwert vollständig ignoriert werden.

In den Beispielen zur Mehrinstanzenfähigkeit, die Sie im Abschnitt [Verwandte Inhalte](#related-content) am Ende dieses Artikels finden, ist die Ausstellerüberprüfung deaktiviert, um die Anmeldung beliebiger Azure AD-Mandanten zu ermöglichen.

Sehen wir uns jetzt an, wie sich Benutzer bei mehrinstanzenfähigen Anwendungen anmelden können.

## Grundlegendes zur Benutzer- und Administratorzustimmung
Damit sich ein Benutzer bei einer Anwendung in Azure AD anmelden kann, muss die Anwendung im Mandanten des Benutzers dargestellt werden. Dies ermöglicht es der Organisation beispielsweise, eindeutige Richtlinien anzuwenden, wenn sich Benutzer ihres Mandanten bei der Anwendung anmelden. Bei einer Anwendung für einen Mandanten ist diese Registrierung einfach. Es handelt sich um die Registrierung, die vorgenommen wird, wenn Sie die Anwendung im [klassischen Azure-Portal][AZURE-classic-portal] registrieren.

Bei einer mehrinstanzenfähigen Anwendung erfolgt die erste Registrierung für die Anwendung in dem Azure AD-Mandanten, der vom Entwickler verwendet wurde. Wenn sich ein Benutzer von einem anderen Mandanten zum ersten Mal bei der Anwendung anmeldet, fordert Azure AD ihn auf, den von der Anwendung angeforderten Berechtigungen zuzustimmen. Wenn er zustimmt, wird eine Darstellung der Anwendung, die als*Dienstprinzipal* bezeichnet wird, im Mandanten des Benutzers erstellt, und die Anmeldung kann fortgesetzt werden. Im Verzeichnis, das die Zustimmung des Benutzers zur Anwendung erfasst, wird auch eine Delegierung erstellt. Ausführliche Informationen zu Anwendungsobjekten und Dienstprinzipalobjekten der Anwendung und deren Beziehungen zueinander finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte][AAD-App-SP-Objects].

![Zustimmung zur App mit einer Ebene][Consent-Single-Tier]

Dieser Zustimmungsprozess wird durch die von der Anwendung angeforderten Berechtigungen beeinflusst. Azure AD unterstützt zwei Arten von Berechtigungen – nur für eine App geltende und delegierte Berechtigungen:

* Eine delegierte Berechtigung gewährt einer Anwendung die Möglichkeit, für einen Teil der Funktionen, die der Benutzer ausführen kann, als angemeldeter Benutzer zu agieren. Sie können einer Anwendung z.B. die delegierte Berechtigung zum Lesen des Kalenders des angemeldeten Benutzers erteilen.
* Eine nur für die App geltende Berechtigung wird der Identität der Anwendung direkt gewährt. Beispielsweise können Sie einer Anwendung die nur für die App geltende Berechtigung zum Lesen der Liste der Benutzer in einem Mandanten erteilen, und sie kann die Liste unabhängig davon lesen, wer sich bei der Anwendung angemeldet hat.

Einigen Berechtigungen kann ein regulärer Benutzer zustimmen, während andere die Zustimmung eines Mandantenadministrators erfordern.

### Administratorzustimmung
Nur für die App geltende Berechtigungen erfordern immer die Zustimmung eines Mandantenadministrators. Wenn die Anwendung eine nur für die App geltende Berechtigung anfordert und ein normaler Benutzer versucht, sich bei der Anwendung anzumelden, wird in der Anwendung eine Fehlermeldung angezeigt, die besagt, dass der Benutzer nicht zustimmen kann.

Bestimmte delegierte Berechtigungen erfordern ebenfalls die Zustimmung eines Mandantenadministrators. Beispielsweise erfordert die Funktion zum Zurückschreiben in Azure AD als der angemeldete Benutzer die Zustimmung eines Mandantenadministrators. Wenn ein normaler Benutzer versucht, sich bei einer Anwendung anzumelden, die eine delegierte Berechtigung anfordert, für die die Zustimmung des Administrators erforderlich ist, wird in der Anwendung ein Fehler angezeigt, wie es auch bei nur für die App geltenden Berechtigungen der Fall ist. Ob eine Berechtigung die Zustimmung des Administrators erfordert, wird durch den Entwickler bestimmt, der die Ressource veröffentlicht hat. Sie können dies in der Dokumentation für die Ressource nachlesen. Links zu Themen, in denen die verfügbaren Berechtigungen für die Azure AD Graph-API und die Microsoft Graph-API beschrieben sind, finden Sie im Abschnitt [Verwandte Inhalte](#related-content) dieses Artikels.

Wenn Ihre Anwendung Berechtigungen nutzt, die die Zustimmung des Administrators erfordern, müssen Sie in Ihrer Anwendung z.B. eine Schaltfläche oder einen Link implementieren, damit der Administrator die Aktion initiieren kann. Die Anforderung, die die Anwendung für diese Aktion sendet, ist eine normale OAuth2/OpenID Connect-Autorisierungsanforderung. Sie enthält aber zusätzlich den Abfragezeichenfolgen-Parameter `prompt=admin_consent`. Nachdem der Administrator zugestimmt hat und der Dienstprinzipal im Mandanten des Kunden erstellt wurde, ist für nachfolgende Anmeldeanforderungen der Parameter `prompt=admin_consent` nicht mehr erforderlich. Da der Administrator entschieden hat, dass die angeforderten Berechtigungen zulässig sind, werden von diesem Zeitpunkt an keine anderen Benutzer im Mandanten zur Zustimmung aufgefordert.

Der Parameter `prompt=admin_consent` kann auch von Anwendungen verwendet werden, die Berechtigungen anfordern, für die die Zustimmung des Administrators nicht erforderlich ist, aber die eine Oberfläche für die einmalige Registrierung des Mandantenadministrators bei der Anwendung bieten möchten. Andere Benutzer werden dann nicht mehr zur Zustimmung aufgefordert.

Wenn für eine Anwendung die Zustimmung des Administrators erforderlich ist und sich der Administrator bei der Anwendung anmeldet, aber der Parameter `prompt=admin_consent` nicht gesendet wird, kann der Administrator der Anwendung erfolgreich zustimmen. Die Zustimmung gilt aber nur für das jeweilige Benutzerkonto. Reguläre Benutzer können sich weiterhin nicht anmelden und der Anwendung zustimmen. Dies ist hilfreich, wenn Sie dem Mandantenadministrator die Möglichkeit geben möchten, Ihre Anwendung zu untersuchen, bevor Sie anderen Benutzern Zugriff gewähren.

Ein Mandantenadministrator kann die Funktion deaktivieren, dass reguläre Benutzer Anwendungen zustimmen. Wenn diese Funktion deaktiviert ist, ist die Zustimmung des Administrators immer erforderlich, damit die Anwendung im Mandanten eingerichtet werden kann. Falls Sie Ihre Anwendung mit deaktivierter Zustimmung durch reguläre Benutzer testen möchten, finden Sie die Konfigurationsoption im Abschnitt mit der Azure AD-Mandantenkonfiguration im [klassischen Azure-Portal][AZURE-classic-portal].

> [!NOTE]
> Für einige Anwendungen wird eine Oberfläche verwendet, auf der reguläre Benutzer zunächst zustimmen können. Später kann die Anwendung den Administrator einbeziehen und Berechtigungen anfordern, für die die Zustimmung des Administrators erforderlich ist. Heute gibt es keine Möglichkeit, dies mit einer einzigen Anwendungsregistrierung in Azure AD zu erreichen. Im zukünftigen Azure AD v2-Endpunkt wird es möglich sein, Berechtigungen zur Laufzeit statt zum Zeitpunkt der Registrierung anzufordern, wodurch dieses Szenario ermöglicht wird. Weitere Informationen finden Sie unter [Anmelden von Benutzern mit Microsoft-Konto und aus Azure AD bei einer einzelnen Anwendung][AAD-V2-Dev-Guide].
> 
> 

### Zustimmung und Anwendungen mit mehreren Ebenen
Ihre Anwendung weist möglicherweise mehrere Ebenen auf, die in Azure AD jeweils durch eine eigene Registrierung dargestellt werden. Beispielsweise eine native Anwendung, die eine Web-API aufruft, oder eine Webanwendung, die eine Web-API aufruft. In beiden Fällen fordert der Client (native App oder Web-App) Berechtigungen an, um die Ressource (Web-API) aufzurufen. Damit dem Client erfolgreich die Zustimmung im Mandanten eines Kunden erteilt werden kann, müssen alle Ressourcen, für die Berechtigungen angefordert werden, bereits im Mandanten des Kunden vorhanden sein. Wenn diese Bedingung nicht erfüllt ist, gibt Azure AD einen Fehler zurück, der besagt, dass die Ressource zuerst hinzugefügt werden muss.

Dies kann ein Problem sein, wenn die logische Anwendung aus zwei oder mehr Anwendungsregistrierungen besteht, z.B. separate Clients und Ressourcen. Wie sorgen Sie zuerst dafür, dass die Ressource im Mandanten des Kunden vorhanden ist? In Azure AD wird dieser Fall abgedeckt, indem Client und Ressource in einem einzigen Schritt zugestimmt werden kann. Dabei sieht der Benutzer auf der Zustimmungsseite die Gesamtmenge der Berechtigungen, die vom Client und von der Ressource angefordert werden. Um dieses Verhalten zu aktivieren, muss die Anwendungsregistrierung der Ressource die App-ID des Clients als `knownClientApplications` im Anwendungsmanifest enthalten. Beispiel:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Diese Eigenschaft kann über das [Anwendungsmanifest][AAD-App-Manifest] der Ressource aktualisiert werden. Sie wird in einem Beispiel eines nativen Clients mit mehreren Ebenen, der eine Web-API aufruft, im Abschnitt [Verwandte Inhalte](#related-content) am Ende dieses Artikels veranschaulicht. Das folgende Diagramm enthält eine Übersicht über die Zustimmung für eine App mit mehreren Ebenen:

![Zustimmung zur bekannten Client-App mit mehreren Ebenen][Consent-Multi-Tier-Known-Client]

Ein ähnlicher Fall tritt ein, wenn die verschiedenen Ebenen einer Anwendung in verschiedenen Mandanten registriert sind. Betrachten Sie z.B. die Erstellung einer nativen Clientanwendung, die die Office 365 Exchange Online-API aufruft. Um die native Anwendung zu entwickeln und sie später im Mandanten eines Kunden auszuführen, muss der Exchange Online-Dienstprinzipal vorhanden sein. In diesem Fall muss der Kunde Exchange Online erwerben, damit der Dienstprinzipal in seinem Mandanten erstellt wird. Im Fall einer API, die von einer anderen Organisation als Microsoft erstellt wurde, muss der Entwickler der API für Kunden eine Möglichkeit bieten, der Anwendung im Mandanten des Kunden die Zustimmung zu erteilen, z.B. über eine Webseite, die mit den in diesem Artikel beschriebenen Mechanismen die Zustimmung steuert. Nachdem der Dienstprinzipal im Mandanten erstellt wurde, kann die native Anwendung Token für die API abrufen.

Das folgende Diagramm enthält eine Übersicht über die Zustimmung für eine App mit mehreren Ebenen, die unter verschiedenen Mandanten registriert wurde:

![Zustimmung zur Mehrparteien-App mit mehreren Ebenen][Consent-Multi-Tier-Multi-Party]

### Widerrufen der Zustimmung
Benutzer und Administratoren können die Zustimmung zu Ihrer Anwendung jederzeit widerrufen:

* Benutzer widerrufen den Zugriff auf einzelne Anwendungen, indem sie sie aus der Liste [Zugriffspanel – Anwendungen][AAD-Access-Panel] entfernen.
* Administratoren widerrufen den Zugriff auf Anwendungen, indem sie sie über den Abschnitt für die Azure AD-Verwaltung im [klassischen Azure-Portal][AZURE-classic-portal] aus Azure AD entfernen.

Wenn ein Administrator einer Anwendung für alle Benutzer in einem Mandanten seine Zustimmung gibt, können Benutzer den Zugriff nicht einzeln widerrufen. Nur der Administrator kann den Zugriff widerrufen, und dies nur für die gesamte Anwendung.

### Zustimmung und Protokollunterstützung
Zustimmung wird in Azure AD über die Protokolle OAuth, OpenID Connect, WS-Verbund und SAML unterstützt. Die Protokolle SAML und WS-Verbund unterstützen den Parameter `prompt=admin_consent` nicht. Die Administratorzustimmung ist also nur über OAuth und OpenID Connect möglich.

## Mehrinstanzenfähige Anwendungen und Zwischenspeichern von Zugriffstoken
Mehrinstanzenfähige Anwendungen können auch Zugriffstoken abrufen, um APIs aufzurufen, die von Azure AD geschützt sind. Ein häufiger Fehler bei der Verwendung von Active Directory Authentication Library (ADAL) mit einer mehrinstanzenfähigen Anwendung ist, zuerst ein Token für einen Benutzer mithilfe von „/common“ anzufordern, eine Antwort zu erhalten und dann ein weiteres Token für den gleichen Benutzer ebenfalls mit „/common“ anzufordern. Da die Antwort von Azure AD von einem Mandanten stammt (und nicht von „/common“), speichert ADAL das Token als Token von dem Mandanten zwischen. Beim nachfolgenden Aufruf von „/common“ zum Abrufen eines Zugriffstokens für den Benutzer wird der Cacheeintrag übersehen, und der Benutzer wird aufgefordert, sich erneut anzumelden. Um zu vermeiden, dass Cacheeinträge übersehen werden, stellen Sie sicher, dass nachfolgende Aufrufe für einen bereits angemeldeten Benutzer dem Endpunkt des Mandanten gelten.

## Verwandte Inhalte
* [Beispiele für mehrinstanzenfähige Anwendungen][AAD-Samples-MT]
* [Brandingrichtlinien für Anwendungen][AAD-App-Branding]
* [Entwicklerhandbuch zu Azure AD][AAD-Dev-Guide]
* [Anwendungsobjekte und Dienstprinzipalobjekte][AAD-App-SP-Objects]
* [Integrieren von Anwendungen in Azure Active Directory][AAD-Integrating-Apps]
* [Übersicht über das Consent Framework][AAD-Consent-Overview]
* [Microsoft Graph-API-Berechtigungsbereiche][MSFT-Graph-AAD]
* [Azure AD Graph-API-Berechtigungsbereiche][AAD-Graph-Perm-Scopes]

Verwenden Sie den unten angezeigten Disqus-Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.

<!--Reference style links IN USE -->
[AAD-Access-Panel]: https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/de-DE/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[MSFT-Graph-AAD]: https://graph.microsoft.io/de-DE/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ./active-directory-appmodel-v2-overview.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/de-DE/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken

<!---HONumber=AcomDC_0727_2016-->