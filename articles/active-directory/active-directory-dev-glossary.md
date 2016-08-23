<properties
   pageTitle="Azure Active Directory-Glossar für Entwickler | Microsoft Azure"
   description="Eine Liste mit häufig verwendeten Konzepten und Features für Azure Active Directory-Entwickler."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/31/2016"
   ms.author="bryanla"/>

# Azure Active Directory-Glossar für Entwickler
Dieser Artikel enthält hilfreiche Definitionen für einige der zentralen Entwicklerkonzepte von Azure Active Directory (AD) im Zusammenhang mit der Anwendungsentwicklung für Azure AD.

## Zugriffstoken 
Eine Art von [Sicherheitstoken](#security-token). Wird von einem [Autorisierungsserver](#authorization-server) ausgestellt und von einer [Clientanwendung](#client-application) für den Zugriff auf einen [geschützten Ressourcenserver](#resource-server) verwendet. Das Token liegt in der Regel in Form eines [JSON Web Token (JWT)][JWT] vor und stellt die Autorisierung dar, die dem Client durch den [Ressourcenbesitzer](#resource-owner) für eine angeforderte Zugriffsstufe gewährt wurde. Das Token enthält alle geltenden [Ansprüche](#claim) für den Antragsteller und kann von der Clientanwendung beim Zugriff auf eine bestimmte Ressource als eine Art von Anmeldeinformationen verwendet werden, sodass der Ressourcenbesitzer gegenüber dem Client keine Anmeldeinformationen angeben muss.

Zugriffstoken werden abhängig von den vorgelegten Anmeldeinformationen gelegentlich auch als „Benutzer- und App-Token“ oder als „ App-exklusives Token“ bezeichnet. Beispielverwendungsszenarien für die Clientanwendung:

- [Autorisierungsgewährung mit Autorisierungscode](#authorization-grant): In diesem Fall wird der Endbenutzer zunächst als Ressourcenbesitzer authentifiziert, und die Autorisierung für den Ressourcenzugriff wird an den Client delegiert. Der Client wird später beim Beziehen des Zugriffstokens authentifiziert. Das Token wird gelegentlich spezifischer als Benutzer- und App-Token bezeichnet, da es sowohl den Benutzer, der die Clientanwendung autorisiert hat, als auch die Anwendung darstellt.
- [Autorisierungsgewährung mit Clientanmeldeinformationen](#authorization-grant): In diesem Fall stellt der Client die einzige Authentifizierung (ohne Authentifizierung/Autorisierung des Ressourcenbesitzers) bereit, weshalb das Token gelegentlich als App-exklusives Token bezeichnet wird.

Ausführlichere Informationen finden Sie in der [Azure AD-Tokenreferenz][AAD-Tokens-Claims].

## Anwendungsmanifest  
Ein Feature des [klassischen Azure-Portals][AZURE-classic-portal], das eine JSON-Darstellung der Identitätskonfiguration der Anwendung generiert. Diese wird als Mechanismus für die Aktualisierung der zugehörigen Entitäten [Anwendung][AAD-Graph-App-Entity] und [ServicePrincipal][AAD-Graph-Sp-Entity] verwendet. Ausführlichere Informationen finden Sie unter [Grundlegendes zum Azure Active Directory-Anwendungsmanifest][AAD-App-Manifest].

## Anwendungsobjekt  
Wenn Sie eine Anwendung im [klassischen Azure-Portal][AZURE-classic-portal] registrieren/aktualisieren, erstellt/aktualisiert das Portal sowohl ein Anwendungsobjekt als auch ein entsprechendes [Dienstprinzipalobjekt](#service-principal-object) für den Mandanten. Das Anwendungsobjekt *definiert* die Identitätskonfiguration der Anwendung global (also für alle Mandanten, auf die es Zugriff hat) und fungiert als Vorlage, von der die entsprechenden Dienstprinzipalobjekte für die lokale Verwendung zur Laufzeit (in einem bestimmten Mandanten) *abgeleitet* werden.

Weitere Informationen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte][AAD-App-SP-Objects].

## Anwendungsregistrierung  
Um einer Anwendung die Integration in Azure AD sowie die Delegierung von Identitäts- und Zugriffsverwaltungsfunktionen an Azure AD zu ermöglichen, muss sie bei einem Azure AD-[Mandanten](#tenant) registriert werden. Wenn Sie Ihre Anwendung bei Azure AD registrieren, stellen Sie eine Identitätskonfiguration für Ihre Anwendung bereit. Dies ermöglicht neben der Integration in Azure AD unter anderem auch die Verwendung folgender Features:

- Stabile Verwaltung von einmaligem Anmelden mit Azure AD-Identitätsverwaltung und [OpenID Connect][OpenIDConnect]-Protokollimplementierung
- Vermittelter Zugriff auf [geschützte Ressourcen](#resource-server) durch [Clientanwendungen](#client-application) über die [Autorisierungsserver](#authorization-server)-Implementierung (OAuth 2.0) von Azure AD
- [Consent Framework](#consent) zum Verwalten des Clientzugriffs auf geschützte Ressourcen auf der Grundlage der Ressourcenbesitzerautorisierung

Ausführlichere Informationen finden Sie unter [Integrieren von Anwendungen in Azure Active Directory][AAD-Integrating-Apps].

## Authentifizierung
Die Anforderung ordnungsgemäßer Anmeldeinformationen von einer Partei. Bildet die Grundlage für die Erstellung eines Sicherheitsprinzipals für die Identitäts- und Zugriffssteuerung. Im Rahmen einer OAuth2-[Autorisierungsgewährung](#authorization-grant) übernimmt die authentifizierende Partei beispielsweise abhängig von der verwendeten Gewährung entweder die Rolle des [Ressourcenbesitzers](#resource-owner) oder die Rolle der [Clientanwendung](#client-application).

## Autorisierung
Die Gewährung einer Handlungsberechtigung für einen authentifizierten Sicherheitsprinzipal. Im Programmiermodell von Azure AD sind im Wesentlichen zwei Anwendungsfälle vorgesehen:

- Im Rahmen einer [OAuth2-Autorisierungsgewährung](#authorization-grant): Wenn der [Ressourcenbesitzer](#resource-owner) die [Clientanwendung](#client-application) autorisiert und dem Client dadurch Zugriff auf Ressourcen der Ressourcenbesitzers gewährt.
- Im Rahmen des Ressourcenzugriffs durch den Client: Gemäß Implementierung durch den [Ressourcenserver](#resource-server) unter Verwendung der Werte des [Anspruchs](#claim) aus dem [Zugriffstoken](#access-token), die als Grundlage für Zugriffssteuerungsentscheidungen herangezogen werden.

## Autorisierungscode
Ein kurzlebiges Token, das einer [Clientanwendung](#client-application) vom [Autorisierungsendpunkt](#authorization-endpoint) im Rahmen des Autorisierungscodeablaufs bereitgestellt wird (eine der vier OAuth2-[Autorisierungsgewährungen](#authorization-grant)). Der Code wird als Antwort auf die Authentifizierung eines [Ressourcenbesitzers](#resource-owner) an die Clientanwendung zurückgegeben und gibt an, dass der Ressourcenbesitzer die Autorisierung an die Clientanwendung delegiert hat, um den Ressourcenzugriff in ihrem Namen zu ermöglichen. Im Rahmen des Ablaufs wird der Code gegen ein [Zugriffstoken](#access-token) eingetauscht.

## Autorisierungsendpunkt
Einer der vom [Autorisierungsserver](#authorization-server) implementierten Endpunkte. Wird für die Interaktion mit dem [Ressourcenbesitzer](#resource-owner) verwendet, um bei einem OAuth2-Autorisierungsgewährungsablauf eine [Autorisierungsgewährung](#authorization-grant) bereitzustellen. Die tatsächliche Gewährung kann je nach verwendetem Autorisierungsgewährungsablauf variieren (einschließlich [Autorisierungscode](#authorization-code) oder [Sicherheitstoken](#security-token)).

Ausführlichere Informationen finden Sie in der OAuth2-Spezifikation in den Abschnitten zu [Autorisierungsgewährungstypen][OAuth2-AuthZ-Grant-Types] und zum [Autorisierungsendpunkt][OAuth2-AuthZ-Endpoint] sowie in der [OpenIDConnect-Spezifikation][OpenIDConnect-AuthZ-Endpoint].

## Autorisierungsgewährung
Anmeldeinformationen, die die [Autorisierung](#authorization) des [Ressourcenbesitzers](#resource-owner) für den Zugriff auf seine geschützten Ressourcen durch eine [Clientanwendung](#client-application) darstellen. Eine Clientanwendung kann abhängig von Clienttyp/Anforderungen einen der [vier durch das OAuth2-Autorisierungsframework definierten Gewährungstypen][OAuth2-AuthZ-Grant-Types] verwenden, um eine Gewährung zu beziehen. Zur Auswahl stehen Autorisierungscodegewährung, Clientanmeldeinformations-Gewährung, implizite Gewährung und Ressourcenbesitzerkennwort-Anmeldeinformationsgewährung. Bei den an den Client zurückgegebenen Anmeldeinformationen handelt es sich je nach Art des verwendeten Autorisierungsgewährungsablaufs entweder um ein [Zugriffstoken](#access-token) oder um einen [Autorisierungscode](#authorization-code) (der später gegen ein Zugriffstoken eingetauscht wird).

## Autorisierungsserver
Gemäß Definition des [OAuth2-Autorisierungsframeworks][OAuth2-Role-Def] der Server für die Ausstellung von Zugriffstoken für den [Client](#client-application) nach erfolgreicher Authentifizierung des [Ressourcenbesitzers](#resource-owner) und Einholung seiner Autorisierung. Eine [Clientanwendung](#client-application) interagiert mit dem Autorisierungsserver zur Laufzeit über dessen [Autorisierungsendpunkt](#authorization-endpoint) und [Token-Endpunkt](#token-endpoint) (in Einklang mit den durch OAuth2 definierten [Autorisierungsgewährungen](#authorization-grant)).

Im Falle der Azure AD-Anwendungsintegration implementiert Azure AD die Autorisierungsserverrolle für Azure AD-Anwendungen und Microsoft-Dienst-APIs (etwa [Microsoft Graph-APIs][Microsoft-Graph]).

## Anspruch
Ein [Sicherheitstoken](#security-token) enthält Ansprüche, die Assertionen zu einer Entität (beispielsweise zu einer [Clientanwendung](#client-application) oder zu einem [Ressourcenbesitzer](#resource-owner)) für eine andere Entität (beispielsweise für den [Ressourcenserver](#resource-server)) bereitstellen. Ansprüche sind Name-Wert-Paare zur Weitergabe von Informationen über den Tokenantragsteller (beispielsweise der vom [Autorisierungsserver](#authorization-server) authentifizierte Sicherheitsprinzipal). Welche Ansprüche in einem Token enthalten sind, hängt von verschiedenen Variablen ab. Hierzu zählen unter anderem die Art des Tokens, die Art der Anmeldeinformationen für die Authentifizierung des Antragstellers und die Anwendungskonfiguration.

Ausführlichere Informationen finden Sie in der [Azure AD-Tokenreferenz][AAD-Tokens-Claims].

## Clientanwendung  
Gemäß Definition des [OAuth2-Autorisierungsframeworks][OAuth2-Role-Def] eine Anwendung, die im Auftrag des [Ressourcenbesitzers](#resource-owner) geschützte Ressourcen anfordert. Der Begriff „Client“ impliziert keine bestimmte Hardwareimplementierung (also beispielsweise, ob die Anwendung auf einem Server, Desktopcomputer oder anderen Gerät ausgeführt wird).

Eine Clientanwendung fordert eine [Autorisierung](#authorization) von einem Ressourcenbesitzer an, um an einer [OAuth2-Autorisierungsgewährung](#authorization-grant) teilzunehmen, und greift ggf. im Auftrag des Ressourcenbesitzers auf APIs/Daten zu. Das OAuth2-Autorisierungsframework [definiert zwei Arten von Clients][OAuth2-Client-Types]\: „vertraulich“ und „öffentlich“ (abhängig davon, ob der Client die Vertraulichkeit seiner Anmeldeinformationen gewährleisten kann). Anwendungen können einen auf einem Webserver ausgeführten [Webclient (vertraulich)](#web-client), eine auf einem Gerät installierte [native Clientanwendung (öffentlich)](#native-client) und einen im Browser eines Geräts ausgeführten [Benutzer-Agent-basierten Client (öffentlich)](#user-agent-based-client) implementieren.

## Zustimmung
Der Prozess, bei dem ein [Ressourcenbesitzer](#resource-owner) eine [Clientanwendung](#client-application) autorisiert und ihr spezifische [Berechtigungen](#permissions) für den Zugriff auf geschützte Ressourcen (im Auftrag der Ressourcenbesitzers) erteilt. Abhängig von den vom Client angeforderten Berechtigungen wird die Zustimmung eines Administrators oder Benutzers angefordert, um den Zugriff auf die entsprechenden Organisationsdaten/individuellen Daten zuzulassen. In einem Szenario mit [mehreren Mandanten](#multi-tenant-application) wird der [Dienstprinzipal](#service-principal-object) der Anwendung auch im Mandanten des zustimmenden Benutzers erfasst.

## ID-Token
Ein vom [Autorisierungsendpunkt](#authorization-endpoint) eines [Autorisierungsservers](#authorization-server) bereitgestelltes [OpenID Connect][OpenIDConnect-ID-Token]-[Sicherheitstoken](#security-token) mit [Ansprüchen](#claim) in Verbindung mit der Authentifizierung eines Endbenutzers vom Typ [Ressourcenbesitzer](#resource-owner). ID-Token werden genau wie Zugriffstoken als digital signiertes [JSON Web Token (JWT)][JWT] dargestellt. Im Gegensatz zu einem Zugriffstoken werden die Ansprüche eines ID-Tokens allerdings nicht im Zusammenhang mit dem Zugriff auf Ressourcen und speziell mit der Zugriffssteuerung verwendet.

Ausführlichere Informationen finden Sie in der [Azure AD-Tokenreferenz][AAD-Tokens-Claims].

## Mehrinstanzenfähige Anwendung
Eine Klasse von in Azure AD registrierten [Clientanwendungen](#client-application), die dazu dienen, die Anmeldung und [Zustimmung](#consent) über Benutzerkonten zu ermöglichen, die in einem beliebigen Azure AD-[Mandanten](#tenant) bereitgestellt wurden. Dies schließt auch andere Mandanten als den Mandanten ein, bei dem der Client ursprünglich registriert wurde. Im Gegensatz dazu ermöglicht eine als einzelner Mandant registrierte Anwendung nur Anmeldungen über Benutzerkonten, die unter dem gleichen Mandanten bereitgestellt wurden, bei dem auch die Anwendung registriert ist. [Native Clientanwendungen](#native-client) sind standardmäßig mehrinstanzenfähig. Bei [Webclientanwendungen](#web-client) kann es sich um Anwendungen mit einem einzelnen Mandanten oder um mehrinstanzenfähige Anwendungen handeln.

Ausführlichere Informationen finden Sie unter [Anmelden von Azure Active Directory-Benutzern (AD) mit dem mehrinstanzenfähigen Anwendungsmuster][AAD-Multi-Tenant-Overview].

## Nativer Client
Eine Art von [Clientanwendung](#client-application), die nativ auf einem Gerät installiert ist. Da der gesamte Code auf einem Gerät ausgeführt wird und keine private/vertrauliche Speicherung von Anmeldeinformationen möglich ist, wird dieser Client als „öffentlich“ betrachtet. Ausführlichere Informationen finden Sie in den [OAuth2-Clienttypen und -profilen][OAuth2-Client-Types].

## Berechtigungen
Eine [Clientanwendung](#client-application) erhält Zugriff auf einen [Ressourcenserver](#resource-server), indem sie Berechtigungsanforderungen deklariert. Zwei Arten sind verfügbar:

- Delegierte Berechtigungen: Fordern [bereichsbasierten](#scopes) Zugriff unter der delegierten Autorisierung des angemeldeten [Ressourcenbesitzers](#resource-owner) an und treten zur Laufzeit als [SCP-Ansprüche](#claim) im [Zugriffstoken](#access-token) des Clients in Erscheinung.
- Anwendungsberechtigungen: Fordern [rollenbasierten](#roles) Zugriff unter den Anmeldeinformationen/der Identität der Anwendung an und treten zur Laufzeit als [Rollenansprüche](#claim) im Zugriffstoken des Clients in Erscheinung.

Darüber hinaus werden sie im Rahmen des [Zustimmungsprozesses](#consent) verwendet, um dem Administrator oder Ressourcenbesitzer die Möglichkeit zu geben, den Clientzugriff auf Ressourcen in seinem Mandanten zu gewähren oder zu verweigern.

Berechtigungsanforderungen werden im [klassischen Azure-Portal][AZURE-classic-portal] auf der Registerkarte „Anwendungen“/„Konfigurieren“ unter „Berechtigungen für andere Anwendungen“ konfiguriert. Wählen Sie hier die gewünschten delegierten Berechtigungen und die gewünschten Anwendungsberechtigungen aus. (Für Letzteres wird die globale Administratorrolle benötigt.) Da ein [öffentlicher Client](#client-application) keine Anmeldeinformationen verwalten kann, kann er nur delegierte Berechtigungen anfordern. Ein [vertraulicher Client](#client-application) kann dagegen sowohl delegierte Berechtigungen als auch Anwendungsberechtigungen anfordern. Das [Anwendungsobjekt](#application-object) des Clients speichert die deklarierten Berechtigungen in der [requiredResourceAccess-Eigenschaft][AAD-Graph-App-Entity].

## Ressourcenbesitzer
Gemäß Definition des [OAuth2-Autorisierungsframeworks][OAuth2-Role-Def] eine Entität, die Zugriff auf eine geschützte Ressource gewähren kann. Handelt es sich bei dem Ressourcenbesitzer um eine Person, wird er als Endbenutzer bezeichnet. Wenn also beispielsweise eine [Clientanwendung](#client-application) über die [Microsoft Graph-API][Microsoft-Graph] auf das Postfach eines Benutzers zugreifen möchte, benötigt sie die Berechtigung des Ressourcenbesitzers für das Postfach.

## Ressourcenserver
Gemäß Definition des [OAuth2-Autorisierungsframeworks][OAuth2-Role-Def] ein Server, der geschützte Ressourcen hostet und von [Clientanwendungen](#client-application), die ein [Zugriffstoken](#access-token) vorlegen, Anforderungen für geschützte Ressourcen akzeptieren und darauf reagieren kann. Wird auch als geschützter Ressourcenserver oder als Ressourcenanwendung bezeichnet.

Ein Ressourcenserver macht APIs verfügbar und steuert den Zugriff auf seine geschützten Ressourcen über [Bereiche](#scopes) und [Rollen](#roles) (unter Verwendung des OAuth 2.0-Autorisierungsframeworks). Beispiele wären etwa die Azure AD Graph-API (bietet Zugriff auf Azure AD-Mandantendaten) und die Office 365-APIs (bieten Zugriff auf Daten wie E-Mails, Kalender und Dokumente). Beide stehen auch über die [Microsoft Graph-API][Microsoft-Graph] zur Verfügung.

Genau wie bei einer Clientanwendung wird auch die Identitätskonfiguration einer Ressourcenanwendung mittels [Registrierung](#application-registration) bei einem Azure AD-Mandanten eingerichtet und sowohl ein Anwendungs- als auch ein Dienstprinzipalobjekt bereitgestellt. Einige von Microsoft bereitgestellte APIs (etwa die Azure AD Graph-API) verfügen über vorab registrierte Dienstprinzipale, die bei der Bereitstellung in allen Mandanten verfügbar gemacht wurden.

## Rollen
Mithilfe von Rollen kann ein [Ressourcenserver](#resource-server) ähnlich wie mit [Bereichen](#scopes) den Zugriff auf seine geschützten Ressourcen steuern. Zwei Arten stehen zur Verfügung: Eine Benutzerrolle implementiert eine rollenbasierte Zugriffssteuerung für Benutzer/Gruppen, die Zugriff auf die Ressource benötigen. Eine Anwendungsrolle implementiert das Gleiche für [Clientanwendungen](#client-application), die Zugriff benötigen.

Bei Rollen handelt es sich um ressourcendefinierte Zeichenfolgen (wie etwa „Ausgabengenehmiger“ oder „Schreibgeschützt“). Sie werden im [klassischen Azure-Portal][AZURE-classic-portal] über das [Anwendungsmanifest](#application-manifest) der Ressource verwaltet und in der [appRoles-Eigenschaft][AAD-Graph-Sp-Entity] der Ressource gespeichert. Eine Clientanwendung fordert eine [Berechtigung](#permissions) an, um auf eine Anwendungsrolle zuzugreifen. Dies wird der Ressource zur Laufzeit im [Rollenanspruch](#claim) des Client-[Zugriffstokens](#access-token) präsentiert. Azure AD-Administratoren können Benutzer über das [klassische Azure-Portal][AZURE-classic-portal] Benutzerrollen zuweisen.

Ausführliche Informationen zu den Anwendungsrollen, die von der Azure AD Graph-API verfügbar gemacht werden, finden Sie unter [Graph API Permission Scopes][AAD-Graph-Perm-Scopes] \(Berechtigungsbereiche der Graph-API). Ein detailliertes Implementierungsbeispiel finden Sie unter [Roles based access control in cloud applications using Azure AD][Duyshant-Role-Blog] \(Rollenbasierte Zugriffssteuerung in Cloudanwendungen mit Azure AD).

## Bereiche
Mithilfe von Bereichen kann ein [Ressourcenserver](#resource-server) ähnlich wie mit [Rollen](#roles) den Zugriff auf seine geschützten Ressourcen steuern. Bereiche dienen zum Implementieren einer [bereichsbasierten][OAuth2-Access-Token-Scopes] Zugriffssteuerung für eine [Clientanwendung](#client-application), der durch den Ressourcenbesitzer delegierter Zugriff auf die Ressource gewährt wurde.

Bei Bereichen handelt es sich um ressourcendefinierte Zeichenfolgen (wie etwa „Mail.Read“ oder „Directory.ReadWrite.All“). Sie werden im [klassischen Azure-Portal][AZURE-classic-portal] über das [Anwendungsmanifest](#application-manifest) der Ressource verwaltet und in der [oauth2Permissions-Eigenschaft][AAD-Graph-Sp-Entity] der Ressource gespeichert. Eine Clientanwendung fordert eine [Berechtigung](#permissions) für den Zugriff an. Diese wird der Ressource zur Laufzeit im [SCP-Anspruch](#claim) des Client-[Zugriffstokens](#access-token) präsentiert.

Als Benennungskonvention hat sich das Format „Ressource.Vorgang.Einschränkung“ bewährt. Ausführliche Informationen zu den Bereichen, die von der Azure AD Graph-API verfügbar gemacht werden, finden Sie unter [Graph API Permission Scopes][AAD-Graph-Perm-Scopes] \(Berechtigungsbereiche der Graph-API). Informationen zu von Office 365-Diensten verfügbar gemachten Bereichen finden Sie unter [Office 365 API permissions reference][O365-Perm-Ref] \(API-Berechtigungsreferenz für Office 365).

## Sicherheitstoken
Ein signiertes Dokument mit Ansprüchen (etwa ein OAuth2-Token oder eine SAML 2.0-Assertion). Im Falle einer OAuth 2.0-[Autorisierungsgewährung](#authorization-grant) handelt es sich bei einem [Zugriffstoken](#access-token) (OAuth2) und einem [ID-Token](OpenID Connect) um eine Art von Sicherheitstoken, und beide werden jeweils als [JSON Web Token (JWT)][JWT] implementiert.

## Dienstprinzipalobjekt
Wenn Sie eine Anwendung im [klassischen Azure-Portal][AZURE-classic-portal] registrieren/aktualisieren, erstellt/aktualisiert das Portal sowohl ein [Anwendungsobjekt](#application-object) als auch ein entsprechendes Dienstprinzipalobjekt für den Mandanten. Das Anwendungsobjekt *definiert* die Identitätskonfiguration der Anwendung global (also für alle Mandanten, für die der zugeordneten Anwendung Zugriff gewährt wurde) und fungiert als Vorlage, von der die entsprechenden Dienstprinzipalobjekte für die lokale Verwendung zur Laufzeit (in einem bestimmten Mandanten) *abgeleitet* werden.

Weitere Informationen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte][AAD-App-SP-Objects].

## Anmeldung
Die Initiierung einer Endbenutzerauthentifizierung durch eine [Clientanwendung](#client-application) sowie die Erfassung des entsprechenden Zustands nach der Authentifizierung mit dem Ziel, ein [Sicherheitstoken](#security-token) zu beziehen und die Anwendungssitzung auf den Zustand abzustimmen. Der Zustand kann Artefakte (beispielsweise Benutzerprofilinformationen) und aus Tokenansprüchen abgeleitete Informationen enthalten.

Einmaliges Anmelden (Single Sign-On, SSO) wird in der Regel über die Anmeldefunktion einer Anwendung implementiert. Dieser kann bei Bedarf eine Registrierungsfunktion vorangestellt werden, die den Einstiegspunkt darstellt, über den ein Endbenutzer beim ersten Anmelden Zugriff auf eine Anwendung erlangt. Die Registrierungsfunktion dient zum Sammeln und Beibehalten eines zusätzlichen, benutzerspezifischen Zustands und erfordert unter Umständen die [Zustimmung des Benutzers](#consent).

## Abmeldung
Die Aufhebung der Authentifizierung eines Benutzers, wobei der Benutzerzustand, der der Sitzung der [Clientanwendung](#client-application) bei der [Anmeldung](#sign-in) zugeordnet wurde, getrennt wird.

## Mandant
Eine Instanz eines Azure AD-Verzeichnisses wird als Azure AD-Mandant bezeichnet. Dieser bietet eine Vielzahl von Features. Hierzu zählen unter anderem ein Registrierungsdienst für integrierte Anwendungen, die Authentifizierung von Benutzerkonten und registrierten Anwendungen sowie die REST-Endpunkte, die zur Unterstützung verschiedener Protokolle (einschließlich OAuth2 und SAML) erforderlich sind. Zu den Endpunkten zählen der [Autorisierungsendpunkt](#authorization-endpoint), der [Token-Endpunkt](#token-endpoint) und der allgemeine, von [mehrinstanzenfähigen Anwendungen](#multi-tenant-application) verwendete Endpunkt.

Ausführliche Informationen zu den verschiedenen Mandantenzugriffsmöglichkeiten finden Sie unter [Einrichten eines Azure Active Directory-Mandanten][AAD-How-To-Tenant].

## Token-Endpunkt
Einer der vom [Autorisierungsserver](#authorization-server) implementierten Endpunkte zur Unterstützung von OAuth2-[Autorisierungsgewährungen](#authorization-grant). Kann je nach verwendeter Gewährung zur Bereitstellung eines [Zugriffstokens](#access-token) (und ggf. eines Aktualisierungstokens) für einen [Client](#client-application) sowie zur Bereitstellung eines [ID-Tokens](#ID-token) verwendet werden, wenn die Gewährung in Kombination mit dem [OpenID Connect][OpenIDConnect]-Protokoll verwendet wird.

## Benutzer-Agent-basierter Client
Eine Art von [Clientanwendung](#client-application), die Code von einem Webserver herunterlädt und innerhalb eines Benutzer-Agents (Webbrowser oder Ähnliches) ausgeführt wird. Ein Beispiel wäre etwa eine Einzelseiten-Anwendung (Single Page Application, SPA). Da der gesamte Code auf einem Gerät ausgeführt wird und keine private/vertrauliche Speicherung von Anmeldeinformationen möglich ist, wird dieser Client als „öffentlich“ betrachtet. Ausführlichere Informationen finden Sie in den [OAuth2-Clienttypen und -profilen][OAuth2-Client-Types].

## Benutzerprinzipal
Ein Benutzerprinzipalobjekt ist (ähnlich wie ein Dienstprinzipalobjekt, das eine Anwendungsinstanz darstellt) eine weitere Art von Sicherheitsprinzipal und stellt einen Benutzer dar. Das Schema für ein Benutzerprinzipalobjekt wird durch die [Benutzerentität][AAD-Graph-User-Entity] von Azure AD Graph definiert. Die Benutzerentität umfasst benutzerbezogene Eigenschaften wie Vorname, Nachname, Benutzerprinzipalname und Verzeichnisrollenmitgliedschaft. Dadurch wird die erforderliche Benutzeridentitätskonfiguration für Azure AD bereitgestellt, um zur Laufzeit einen Benutzerprinzipal einzurichten. Der Benutzerprinzipal stellt bei der Erfassung der Delegierung der [Zustimmung](#consent) sowie bei Zugriffssteuerungsentscheidungen und Ähnlichem den authentifizierten Benutzer dar.

## Webclient
Eine Art von [Clientanwendung](#client-application), die sämtlichen Code auf einem Webserver ausführt und als vertraulicher Client gilt, da Anmeldeinformationen privat/vertraulich auf dem Server gespeichert werden können. Ausführlichere Informationen finden Sie in den [OAuth2-Clienttypen und -profilen][OAuth2-Client-Types].

## Nächste Schritte
Das [Entwicklerhandbuch zu Azure Active Directory][AAD-Dev-Guide] ist das Portal für alle relevanten Themen rund um die Azure AD-Entwicklung und enthält unter anderem eine Übersicht über die [Integration in Azure Active Directory][AAD-How-To-Integrate] sowie grundlegende Informationen zu [Authentifizierungsszenarien für Azure AD][AAD-Auth-Scenarios].

Verwenden Sie den unten angezeigten Disqus-Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.

<!--Image references-->

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
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/de-DE/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken

<!---HONumber=AcomDC_0803_2016-->