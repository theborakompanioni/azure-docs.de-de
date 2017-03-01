---
title: Sicherheitsaspekte beim Remotezugriff auf Apps mit dem Azure AD-Anwendungsproxy | Microsoft-Dokumentation
description: Es werden die Sicherheitsaspekte bei Verwendung des Azure AD-Anwendungsproxys beschrieben.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 67650676117fef834ea819da2fc45ca66ed15493
ms.openlocfilehash: 1b713e015f48b044c6c9d8ff6a0d8669d4532a9f


---

# <a name="security-considerations-when-accessing-apps-remotely-using-azure-ad-application-proxy"></a>Sicherheitsaspekte beim Remotezugriff auf Apps mit dem Azure AD-Anwendungsproxy

>[!NOTE]
>Das Feature "Anwendungsproxy" ist nur verfügbar, wenn Sie Azure Active Directory auf die Premium oder Basic Edition aktualisiert haben. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).
>  

In diesem Artikel wird beschrieben, wie mit dem Azure AD-Anwendungsproxy ein sicherer Dienst für die Veröffentlichung und den Remotezugriff auf Anwendungen bereitgestellt wird. 

Der Azure AD-Anwendungsproxy bietet die folgenden Sicherheitsvorteile:

**Authentifizierter Zugriff:** Nur authentifizierte Verbindungen können auf Ihr Netzwerk zugreifen.

* Für den Azure AD-Anwendungsproxy wird für die gesamte Authentifizierung Azure AD STS verwendet. Für Anwendungen, die mit Vorauthentifizierung veröffentlicht werden, ist es nicht zulässig, dass Datenverkehr ohne gültiges STS-Token durch den Anwendungsproxydienst in Ihre Umgebung fließt.
* Bei der Vorauthentifizierung liegt es in der Natur der Sache, dass eine erhebliche Anzahl von anonymen Angriffen blockiert wird, da nur für authentifizierte Identitäten der Zugriff auf die Back-End-Anwendung zugelassen wird.

**Bedingter Zugriff:** Wenden Sie umfassendere Richtlinienkontrollen an, bevor Verbindungen mit Ihrem Netzwerk hergestellt werden.

* Beim bedingten Zugriff können Sie Einschränkungen dazu, welcher Datenverkehr auf Ihre Back-End-Anwendungen zugreifen kann, genauer definieren. Sie können Einschränkungen basierend auf dem Standort, der Authentifizierungssicherheit und dem Benutzerrisikoprofil definieren.
* Mit dieser Funktion werden weitere Barrieren gegen Angreifer errichtet. Weitere Informationen zum bedingten Zugriff finden Sie unter [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access-azuread-connected-apps).

**Beendigung des Datenverkehrs:** Der gesamte Datenverkehrsvorgang wird in der Cloud beendet.

* Der Azure AD-Anwendungsproxy ist ein Reverseproxy, sodass der gesamte Datenverkehr zu Back-End-Anwendungen beim Dienst beendet wird. Die Sitzung kann nur mit dem Back-End-Server wiederhergestellt werden. Dies bedeutet, dass Ihre Back-End-Server nicht für direkten HTTP-Datenverkehr zugänglich sind. Beispielsweise können Sie hierdurch zielgerichtete Angriffe leichter eindämmen.

**Gesamter Zugriff erfolgt in ausgehender Richtung:** Es müssen keine eingehenden Verbindungen zum Unternehmensnetzwerk geöffnet werden.

* Azure AD-Connectors verfügen über ausgehende Verbindungen mit dem Azure AD-Anwendungsproxydienst. Dies bedeutet, dass keine Firewallports für eingehende Verbindungen geöffnet werden müssen. 
* Für herkömmliche Ansätze waren eine DMZ und das Zulassen des Zugriffs auf nicht authentifizierte Verbindungen im Netzwerkedge erforderlich. Aus diesem Grund mussten viele zusätzliche Investitionen in WAF-Produkte getätigt werden, um Datenverkehr zu analysieren und den Schutz der Umgebung auszubauen. Mit dem Anwendungsproxy können Sie dieses Szenario vermeiden. Sie können auch auf die DMZ verzichten, da alle Verbindungen in ausgehender Richtung und über einen sicheren Kanal verlaufen.

**Sicherheitsanalyse und auf Machine Learning basierende Intelligence:** Sicherheit und Schutz auf dem neuesten Stand

* Azure AD Identity Protection mit Intelligence auf Machine Learning-Basis mit Datenfeeds von unserer Digital Crimes Unit und aus dem Microsoft Security Response Center. Zusammen identifizieren wir proaktiv kompromittierte Konten und ermöglichen den Echtzeitschutz vor Anmeldungen mit hohem Risikofaktor. Wir berücksichtigen viele verschiedene Faktoren, z.B. den Zugriff von infizierten Geräten und über Anonymisierungsnetzwerke sowie von ungewöhnlichen und zweifelhaften Standorten.
* Viele dieser Berichte und Ereignisse sind über eine API zur Integration in Ihre SIEM-Systeme bereits verfügbar.
* Weitere Informationen zu Azure AD Identity Protection finden Sie unter [Azure Active Directory Identity Protection](https://azure.microsoft.com/en-us/documentation/articles/active-directory-identityprotection).
!
**Remotezugriff als Dienst:** Sie müssen sich nicht mit dem Warten und Patchen von lokalen Servern beschäftigen.

* Der Azure AD-Anwendungsproxy ist ein Internetskalierungsdienst, der sich in unserem Besitz befindet. So können Sie sicher sein, dass Sie immer die neuesten Sicherheitspatches und -upgrades erhalten. Wir kümmern uns also um die Sicherheit Ihres Netzwerks.
* Software ohne die richtigen Patches ist immer noch eine häufige Ursache für eine große Zahl von Angriffen. Mit unserem Dienstmodell müssen Sie sich nicht mehr mit der Verwaltung der Edgeserver belasten. 

Die Remotezugriffsdienste von Azure AD werden gemäß den Richtlinien und Standards betrieben, die im [Azure Trust Center](https://azure.microsoft.com/en-us/support/trust-center) beschrieben sind. 

Im Diagramm unten ist dargestellt, wie mit Azure AD der sichere Remotezugriff auf Ihre lokalen Anwendungen ermöglicht wird.

 ![Azure AD-Anwendungsproxyconnectors](./media/application-proxy-security-considerations/secure-remote-access.png)

>[!NOTE] 
>Zur Erhöhung der Sicherheit von Anwendungen, die vom Azure AD-Anwendungsproxy veröffentlicht werden, blockieren wir die Indizierung und Archivierung Ihrer Anwendungen durch Webcrawlerroboter. Jedes Mal, wenn ein Webcrawlerroboter versucht, die robots-Einstellungen für eine veröffentlichte App abzurufen, antwortet der App-Proxy mit einer Datei „robots.txt“, die Folgendes enthält: <br>
>   _User-agent: *_<br>
>   _Disallow: /_
>

## <a name="components-of-the-azure-ad-app-proxy-solution"></a>Komponenten der Azure AD-Anwendungsproxylösung

Der Azure AD-Anwendungsproxy besteht aus zwei Teilen:

* Dem cloudbasierten Dienst (AAD AP). Hier werden die externen Client-/Benutzerverbindungen hergestellt.
* Einer lokalen Komponente, die als AAD-Anwendungsproxyconnector bezeichnet wird.  Der Connector lauscht auf Anforderungen vom AAD-AP-Dienst und verarbeitet Verbindungen mit den internen Anwendungen, z.B. in Bezug auf die eingeschränkte Kerberos-Delegierung (Kerberos Constrained Delegation, KCD) für SSO.

### <a name="traffic-flowsand-how-they-are-secured"></a>Datenverkehrsflüsse und ihr Schutz
Dieser Abschnitt enthält Details dazu, wie die Datenverkehrsflüsse („Flows“) geschützt werden. Ein Flow zwischen dem Connector und dem Anwendungsproxydienst wird in folgenden Fällen eingerichtet: 

* Der Connector wird zuerst eingerichtet.
* Der Connector ruft Konfigurationsinformationen aus dem Anwendungsproxydienst ab, z.B. die Connectorgruppe, deren Mitglied jeder Connector ist.
* Ein Benutzer greift auf eine veröffentlichte Anwendung zu.

>[!NOTE]
>Die gesamte Kommunikation erfolgt per SSL und verläuft immer vom Connector zum Anwendungsproxydienst. Dieser Dienst gilt nur für ausgehenden Datenverkehr.
>

Der Connector verwendet ein Clientzertifikat, um den Anwendungsproxydienst für alle Aufrufe zu authentifizieren. Die einzige Ausnahme ist hierbei der anfängliche Setupschritt, bei dem das Clientzertifikat eingerichtet wird.

#### <a name="install-the-connector"></a>Installieren des Connectors

Der folgende Flow findet statt, wenn der Connector zuerst eingerichtet wird.

1. Die Connectorregistrierung beim Dienst erfolgt im Rahmen der Connectorinstallation. Bei diesem Vorgang wird der Benutzer aufgefordert, seine Azure AD-Administratoranmeldeinformationen einzugeben. Das abgerufene Token wird dann dem Azure AD-Anwendungsproxydienst präsentiert.
2. Der Anwendungsproxy wertet das Token aus, um sicherzustellen, dass der Benutzer Mitglied der Administratorrolle des Unternehmens auf dem Mandanten ist, für den das Token ausgestellt wurde. Wenn dies nicht der Fall ist, wird der Prozess beendet.
3. Der Connector generiert eine Clientzertifikatanforderung und übergibt sie mit dem Token an den Anwendungsproxydienst, wo wiederum das Token überprüft und die Clientzertifikatanforderung signiert wird. 
4. Der Connector verwendet dieses Clientzertifikat für die zukünftige Kommunikation mit dem Anwendungsproxydienst.
4. Der Connector führt einen ersten Abruf der Systemkonfigurationsdaten vom Dienst durch, indem das dazugehörige Clientzertifikat verwendet wird, und ist zum Verarbeiten von Anforderungen bereit.

#### <a name="periodic-configuration-updates"></a>Regelmäßige Konfigurationsupdates

Der folgende Flow wird regelmäßig durchgeführt, und zwar bei jeder Aktualisierung der Konfigurationseinstellungen durch den Anwendungsproxydienst.

1. Der Connector stellt die Verbindung mit dem Konfigurationsendpunkt im Anwendungsproxydienst mit dem dazugehörigen Clientzertifikat her.
2. Nachdem das Clientzertifikat überprüft wurde, gibt der Anwendungsproxydienst Konfigurationsdaten an den Connector zurück. Dies kann beispielsweise die Connectorgruppe sein, in der der Connector enthalten sein soll.
3. Der Connector generiert eine neue Zertifikatanforderung, wenn das aktuelle Zertifikat mehr als 30 Tage alt ist, sodass das Clientzertifikat praktisch alle 30 Tage erneuert wird.

#### <a name="accesss-published-applications"></a>Zugreifen auf veröffentlichte Anwendungen

Der folgende Flow erfolgt, wenn Benutzer auf eine veröffentlichte Anwendung zugreifen.

1. Wenn ein Benutzer auf eine veröffentlichte Anwendung zugreift, überprüft der Anwendungsproxydienst die Konfigurationseinstellungen für die App.  Falls die App für die Verwendung der Vorauthentifizierung mit Azure AD konfiguriert ist, wird der Benutzer zur Authentifizierung an Azure AD STS umgeleitet.  Dies wird übersprungen, wenn Sie die App per Passthrough veröffentlichen.
 * Während der Authentifizierung mit Azure AD wird eine Überprüfung auf etwaige Richtlinienanforderungen in Bezug auf den bedingten Zugriff für die jeweilige Anwendung durchgeführt. So wird sichergestellt, dass der Benutzer der Anwendung zugewiesen wurde (wenn beispielsweise MFA erforderlich ist, wird der Benutzer von der Authentifizierungssequenz zur Durchführung einer zweistufigen Authentifizierung aufgefordert).
 * Nachdem alle Überprüfungen bestanden wurden, stellt Azure AD STS ein signiertes Token für die Anwendung aus und leitet den Benutzer zurück an den Anwendungsproxydienst.
 * Der Anwendungsproxy überprüft das Token, um sicherzustellen, dass es für die Anwendung ausgestellt wurde, für die der Benutzer den Zugriff angefordert hat. Dieser Schritt wird zusammen mit anderen Überprüfungen durchgeführt, z.B. der Sicherstellung, dass das Token von Azure AD signiert wurde, noch nicht abgelaufen ist usw.
 * Mit dem Anwendungsproxy wird ein verschlüsseltes Cookie für die Authentifizierung (z.B. ein nicht dauerhaftes Cookie) festgelegt, um anzugeben, dass die Authentifizierung für die Anwendung erfolgt ist.  Dieses Cookie enthält einen Ablaufzeitstempel, der auf dem Token von Azure AD und anderen Daten basiert, z.B. dem Benutzernamen, auf dem die Authentifizierung basiert.  Dieses Cookie wird mit einem privaten Schlüssel verschlüsselt, der nur dem Anwendungsproxydienst bekannt ist.
 * Der Anwendungsproxy leitet den Benutzer zurück zur ursprünglich angeforderten URL.
 >[!NOTE] 
 >Wenn für einen Teil der Schritte für die Vorauthentifizierung Fehler auftreten, wird die Anforderung des Benutzers verworfen, und dem Benutzer wird eine Meldung mit einem Hinweis zur Problemursache angezeigt.
 >

2. Beim Empfang der Anforderung vom Client überprüft der Anwendungsproxy, ob die Bedingung für die Vorauthentifizierung erfüllt wurde und ob das Cookie noch gültig ist (falls erforderlich). Anschließend wird eine Anforderung in die entsprechende Warteschlange eingereiht, damit sie von einem lokalen Connector verarbeitet werden kann. 

 >[!NOTE]
 >Alle Anforderungen vom Connector verlaufen in ausgehender Richtung an den Anwendungsproxydienst. Für Connectors wird eine ausgehende Verbindung zum Anwendungsproxy offengehalten. Beim Empfang einer Anforderung reiht der Anwendungsproxy diese in eine Warteschlange für eine der offenen Verbindungen ein, damit sie vom Connector verarbeitet werden kann.
 >

 * Die Anforderung enthält Elemente aus der Anwendung, z.B. Header der Anforderung und Daten aus dem verschlüsselten Cookie, den Benutzer, der die Anforderung gesendet hat, und die Anforderungs-ID.  Das verschlüsselte Authentifizierungscookie wird aber nicht an den Connector gesendet.
3. Der Connector empfängt die Anforderung aus der Warteschlange basierend auf einer ausgehenden Verbindung mit langer Lebensdauer. Der Anwendungsproxy führt basierend auf der Anforderung einen der folgenden Schritte aus:
 * Der Connector bestätigt, ob er die Anwendung identifizieren kann.  Falls nicht, stellt der Connector eine Verbindung mit dem Anwendungsproxydienst her, um Details zur Anwendung zu sammeln und lokal zu speichern.
 * Wenn die Anforderung ein einfacher Vorgang ist, z.B. ohne Daten im Text wie bei einer „GET“-RESTful-Anforderung, stellt der Connector eine Verbindung mit der internen Zielressource her und wartet auf eine Antwort.
 * Wenn in der Anforderung im Textbereich Daten zugeordnet sind, z.B. bei einem „POST“-RESTful-Vorgang, stellt der Connector mit dem Clientzertifikat eine ausgehende Verbindung mit der Anwendungsproxyinstanz her. Das Ziel hierbei ist das Anfordern der Daten und das Öffnen einer Verbindung mit der internen Ressource.  Bei Erhalt der Anforderung vom Connector beginnt der Anwendungsproxydienst damit, Inhalte vom Benutzer zu akzeptieren und diese an den Connector weiterzuleiten.  Der Connector leitet die Daten wiederum an die interne Ressource weiter.
4. Nachdem die Anforderung/Übertragung des gesamten Inhalts an das Back-End abgeschlossen ist, wartet der Connector auf eine Antwort.
5. Wenn eine Antwort eingeht, stellt der Connector eine ausgehende Verbindung mit dem Anwendungsproxydienst her, um die Headerdetails zurückzugeben und mit dem Streamen der Rückgabedaten zu beginnen.
6. Der Anwendungsproxy „streamt“ die Daten an den Benutzer.  Zu diesem Zeitpunkt können einige Verarbeitungsschritte, sofern erforderlich und von der Anwendung definiert, ausgeführt werden.

Falls Sie Hilfe zur Kommunikation von einer Azure-Webanwendung über einen Clientbrowser mit einem lokalen SOAP-Endpunkt (Simple Object Access Protocol) mit Windows-Authentifizierung benötigen, helfen Ihnen die Informationen in diesem [Azure Field Notes Blog](http://www.azurefieldnotes.com/2016/12/02/claims-to-windows-identity-translation-solutions-and-its-flaws-when-using-azure-ad-application-proxy) weiter. 

##<a name="next-steps"></a>Nächste Schritte
[Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-understand-connectors.md)




<!--HONumber=Feb17_HO2-->


