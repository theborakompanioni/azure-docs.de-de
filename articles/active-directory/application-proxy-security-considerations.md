---
title: Sicherheitsaspekte beim Remotezugriff auf Apps mit dem Azure AD-Anwendungsproxy | Microsoft-Dokumentation
description: Die Sicherheitsaspekte bei Verwendung des Azure AD-Anwendungsproxys werden beschrieben.
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
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 4019b379939e43e4e974552f1f0be245c5b1d33e
ms.lasthandoff: 04/17/2017


---

# <a name="security-considerations-for-accessing-apps-remotely-by-using-azure-ad-application-proxy"></a>Sicherheitsaspekte beim Remotezugriff auf Apps mit dem Azure AD-Anwendungsproxy

>[!NOTE]
>Das Anwendungsproxy-Feature ist nur verfügbar, wenn Sie Azure Active Directory auf die Premium oder Basic Edition aktualisiert haben. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).

In diesem Artikel wird beschrieben, wie mit dem Azure Active Directory-Anwendungsproxy (Azure AD) ein sicherer Dienst für die Veröffentlichung und den Remotezugriff auf Anwendungen bereitgestellt wird.

Der Azure AD-Anwendungsproxy bietet die folgenden Sicherheitsvorteile:

**Authentifizierter Zugriff:** Nur authentifizierte Verbindungen können auf Ihr Netzwerk zugreifen.

* Der Azure AD-Anwendungsproxy basiert auf dem Azure AD-Sicherheitstokendienst (Security Token Service, STS) für alle Authentifizierungen. Bei Anwendungen, die mit Vorauthentifizierung veröffentlicht werden, kann kein Datenverkehr ohne gültiges STS-Token den Anwendungsproxydienst für Ihre Umgebung passieren.
* Bei der Vorauthentifizierung liegt es in der Natur der Sache, dass eine erhebliche Anzahl von anonymen Angriffen blockiert wird, da nur für authentifizierte Identitäten der Zugriff auf die Back-End-Anwendung zugelassen wird.

**Bedingter Zugriff:** Wenden Sie umfassendere Richtlinienkontrollen an, bevor Verbindungen mit Ihrem Netzwerk hergestellt werden.

* Beim bedingten Zugriff können Sie Einschränkungen dazu, welcher Datenverkehr auf Ihre Back-End-Anwendungen zugreifen kann, genauer definieren. Sie können Einschränkungen basierend auf dem Standort, der Authentifizierungssicherheit und dem Benutzerrisikoprofil definieren.
* Mit dieser Funktion werden weitere Barrieren gegen Angreifer errichtet. Weitere Informationen zum bedingten Zugriff finden Sie unter [Erste Schritte mit dem bedingten Zugriff in Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access-azuread-connected-apps).

**Beendigung des Datenverkehrs:** Der gesamte Datenverkehrsvorgang wird in der Cloud beendet.

* Da es sich beim Azure AD-Anwendungsproxy um einen Reverseproxy handelt, wird der gesamte Datenverkehr zu Back-End-Anwendungen beim Dienst beendet. Die Sitzung kann nur mit dem Back-End-Server wiederhergestellt werden. Das bedeutet, dass Ihre Back-End-Server keinem direkten HTTP-Datenverkehr ausgesetzt werden. Beispielsweise können Sie hierdurch zielgerichtete Angriffe leichter eindämmen.

**Gesamter Zugriff erfolgt in ausgehender Richtung:** Sie müssen keine eingehenden Verbindungen mit dem Unternehmensnetzwerk öffnen.

* Azure AD-Connectors verwalten ausgehende Verbindungen mit dem Azure AD-Anwendungsproxydienst, d.h. Firewallports müssen nicht für eingehende Verbindungen geöffnet werden.
* Herkömmliche Ansätze erfordern ein Umkreisnetzwerk (auch bekannt als *DMZ*, *demilitarisierte Zone* und *überwachtes Subnetz*) und öffnen den Zugriff auf nicht authentifizierte Verbindungen am Rand des Netzwerks. Aus diesem Grund mussten viele zusätzliche Investitionen in Web Application Firewall-Produkte (WAF) getätigt werden, um Datenverkehr zu analysieren und den Schutz der Umgebung auszubauen. Mit dem Anwendungsproxy können Sie dieses Szenario vermeiden. Sie können auch erwägen, das Umkreisnetzwerk nicht zu verwenden, da alle Verbindungen ausgehend sind und über einen sicheren Kanal erfolgen.

**Sicherheitsanalyse und auf Machine Language basierende Intelligence:** Sicherheit und Schutz auf dem neuesten Stand

* Azure AD Identity Protection mit Intelligence auf Machine Learning-Basis mit Datenfeeds von unserer Digital Crimes Unit und aus dem Microsoft Security Response Center. Zusammen identifizieren wir proaktiv kompromittierte Konten und ermöglichen den Echtzeitschutz vor Anmeldungen mit hohem Risikofaktor. Wir berücksichtigen viele verschiedene Faktoren, z.B. den Zugriff von infizierten Geräten und über Anonymisierungsnetzwerke sowie von ungewöhnlichen und zweifelhaften Standorten.
* Viele dieser Berichte und Ereignisse sind bereits über eine API für die Integration in Ihre Sicherheitsinformations- und Ereignisverwaltungssysteme (Security Information and Event Management, SIEM) verfügbar.
* Weitere Informationen finden Sie unter [Azure AD Identity Protection](https://azure.microsoft.com/documentation/articles/active-directory-identityprotection).

**Remotezugriff als Dienst:** Sie müssen sich nicht mit dem Warten und Patchen von lokalen Servern beschäftigen.

* Der Azure AD-Anwendungsproxy ist ein Internetskalierungsdienst, der sich im Besitz von Microsoft befindet. So sind Sie sicher, dass Sie immer die neuesten Sicherheitspatches und -upgrades erhalten.
* Software ohne die richtigen Patches ist immer noch eine häufige Ursache für eine große Zahl von Angriffen. Mit unserem Dienstmodell müssen Sie sich nicht mehr mit der Verwaltung der Edgeserver belasten.

Die Remotezugriffsdienste von Azure AD werden gemäß den Richtlinien und Standards betrieben, die im [Azure Trust Center](https://azure.microsoft.com/support/trust-center) beschrieben sind.

Im folgenden Diagramm ist dargestellt, wie mit Azure AD der sichere Remotezugriff auf Ihre lokalen Anwendungen ermöglicht wird.

 ![Diagramm des sicheren Remotezugriffs über den Azure AD-Anwendungsproxy](./media/application-proxy-security-considerations/secure-remote-access.png)

>[!NOTE]
>Zur Erhöhung der Sicherheit von Anwendungen, die vom Azure AD-Anwendungsproxy veröffentlicht werden, blockieren wir die Indizierung und Archivierung Ihrer Anwendungen durch Webcrawlerroboter. Jedes Mal, wenn ein Webcrawlerroboter versucht, die robots-Einstellungen für eine veröffentlichte App abzurufen, antwortet der Anwendungsproxy mit einer Datei „robots.txt“, die folgenden Text enthält:
>
>_Benutzer-Agent: *_  
>_Nicht zulassen: /_

## <a name="components-of-the-azure-ad-application-proxy-solution"></a>Komponenten der Azure AD-Anwendungsproxylösung

Der Azure AD-Anwendungsproxy besteht aus zwei Teilen:

* Dem cloudbasierten Dienst: In diesem Dienst werden die externen Client-/Benutzerverbindungen hergestellt.
* Dem Azure AD-Anwendungsproxyconnector: Als lokale Komponente lauscht der Connector auf Anfragen vom Azure AD-Anwendungsproxydienst und wickelt die Verbindungen mit den internen Anwendungen ab. Der Dienst kümmert sich auch um Elemente wie z.B. die eingeschränkte Kerberos-Delegierung für SSO.

Ein Flow zwischen dem Connector und dem Anwendungsproxydienst wird in folgenden Fällen eingerichtet:

* Bei der Ersteinrichtung des Connectors.
* Der Connector ruft Konfigurationsinformationen aus dem Anwendungsproxydienst ab, z.B. die Connectorgruppe, deren Mitglied jeder Connector ist.
* Ein Benutzer greift auf eine veröffentlichte Anwendung zu.

>[!NOTE]
>Die gesamte Kommunikation erfolgt per SSL und verläuft immer vom Connector zum Anwendungsproxydienst. Der Dienst gilt nur für ausgehenden Datenverkehr.

Der Connector verwendet ein Clientzertifikat, um den Anwendungsproxydienst für nahezu alle Aufrufe zu authentifizieren. Die einzige Ausnahme zu dieser Vorgehensweise ist hierbei der anfängliche Setupschritt, bei dem das Clientzertifikat eingerichtet wird.

### <a name="installing-the-connector"></a>Installieren des Connectors

Bei der Ersteinrichtung des Connectors treten die folgenden Flow-Ereignisse ein:

1. Die Connectorregistrierung beim Dienst erfolgt im Rahmen der Connectorinstallation. Derzeit werden Benutzer aufgefordert, ihre Azure AD-Administratoranmeldeinformationen einzugeben. Das abgerufene Token wird dann dem Azure AD-Anwendungsproxydienst präsentiert.
2. Der Anwendungsproxy wertet das Token aus, um sicherzustellen, dass der Benutzer Mitglied der Administratorrolle des Unternehmens auf dem Mandanten ist, für den das Token ausgestellt wurde. Wenn der Benutzer kein Mitglied der Administratorrolle ist, wird der Prozess beendet.
3. Der Connector generiert eine Clientzertifikatanforderung und übergibt sie mit dem Token an den Anwendungsproxydienst, wo wiederum das Token überprüft und die Clientzertifikatanforderung signiert wird.
4. Der Connector verwendet dieses Clientzertifikat für die zukünftige Kommunikation mit dem Anwendungsproxydienst.
5. Der Connector führt einen ersten Abruf der Systemkonfigurationsdaten vom Dienst durch, indem das dazugehörige Clientzertifikat verwendet wird, und ist jetzt zum Verarbeiten von Anforderungen bereit.

### <a name="updating-the-configuration-settings"></a>Aktualisieren der Konfigurationseinstellungen

Wenn der Anwendungsproxydienst die Konfigurationseinstellungen aktualisiert, treten die folgenden Flow-Ereignisse ein:

1. Der Connector stellt die Verbindung mit dem Konfigurationsendpunkt im Anwendungsproxydienst mit dem dazugehörigen Clientzertifikat her.
2. Nachdem das Clientzertifikat überprüft wurde, gibt der Anwendungsproxydienst Konfigurationsdaten an den Connector zurück. Dies kann beispielsweise die Connectorgruppe sein, in der der Connector enthalten sein soll.
3. Der Connector generiert eine neue Zertifikatanforderung, wenn das aktuelle Zertifikat mehr als 30 Tage alt ist, sodass das Clientzertifikat praktisch alle 30 Tage aktualisiert wird.

### <a name="accessing-published-applications"></a>Zugreifen auf veröffentlichte Anwendungen

Die folgenden Ereignisse treten ein, wenn Benutzer auf eine veröffentlichte Anwendung zugreifen:

1. Der Anwendungsproxydienst überprüft die Konfigurationseinstellungen für die App. Falls die App für die Verwendung der Vorauthentifizierung mit Azure AD konfiguriert ist, werden die Benutzer zur Authentifizierung an Azure AD STS umgeleitet. Wenn Sie die App mithilfe von Pass-Through veröffentlichen, wird dieser Schritt übersprungen.

 a. Während der Authentifizierung mit Azure AD überprüft der Anwendungsproxy etwaige Richtlinienanforderungen in Bezug auf den bedingten Zugriff für die jeweilige Anwendung. Mit diesem Schritt wird sichergestellt, dass der Benutzer der Anwendung zugewiesen wurde. Wenn Multi-Factor Authentication (MFA) erforderlich ist, fordert die Authentifizierungssequenz den Benutzer zu einer zweistufigen Authentifizierung auf.

 b. Nachdem alle Überprüfungen bestanden wurden, stellt Azure AD STS ein signiertes Token für die Anwendung aus und leitet den Benutzer zurück an den Anwendungsproxydienst.

 c. Der Anwendungsproxy überprüft das Token, um sicherzustellen, dass es für die Anwendung ausgestellt wurde, für die der Benutzer den Zugriff angefordert hat. Dieser Schritt wird zusammen mit anderen Überprüfungen durchgeführt, z.B. mit der Sicherstellung, dass das Token von Azure AD signiert wurde und noch nicht abgelaufen ist.

 d. Mit dem Anwendungsproxy wird ein verschlüsseltes Cookie für die Authentifizierung (z.B. ein nicht dauerhaftes Cookie) festgelegt, um anzugeben, dass die Authentifizierung für die Anwendung erfolgt ist. Das Cookie enthält einen Ablaufzeitstempel, der auf dem Token von Azure AD und anderen Daten basiert, z.B. dem Benutzernamen, auf dem die Authentifizierung basiert. Das Cookie wird mit einem privaten Schlüssel verschlüsselt, der nur dem Anwendungsproxydienst bekannt ist.

 e. Der Anwendungsproxy leitet den Benutzer zurück zur ursprünglich angeforderten URL.

 >[!NOTE]
 >Wenn für einen Teil der Schritte für die Vorauthentifizierung Fehler auftreten, wird die Anforderung des Benutzers verworfen, und dem Benutzer wird eine Meldung mit einem Hinweis zur Problemursache angezeigt.
 >

2. Nach dem Empfang der Anforderung vom Client überprüft der Anwendungsproxy, ob die Bedingung für die Vorauthentifizierung erfüllt wurde und ob das Cookie noch gültig ist (falls erforderlich). Der Anwendungsproxy reiht anschließend eine Anforderung in die entsprechende Warteschlange ein, damit sie von einem lokalen Connector verarbeitet werden kann. 

 >[!NOTE]
 >Alle Anforderungen vom Connector verlaufen in ausgehender Richtung an den Anwendungsproxydienst. Für Connectors wird eine ausgehende Verbindung zum Anwendungsproxy offengehalten. Beim Empfang einer Anforderung reiht der Anwendungsproxy diese in eine Warteschlange für eine der offenen Verbindungen ein, damit sie vom Connector verarbeitet werden kann.

 * Die Anforderung enthält Elemente aus der Anwendung, z.B. Anforderungsheader und Daten aus dem verschlüsselten Cookie, den Benutzer, der die Anforderung gesendet hat, und die Anforderungs-ID. Das verschlüsselte Authentifizierungscookie wird jedoch nicht an den Connector gesendet.

3. Der Connector empfängt die Anforderung aus der Warteschlange basierend auf einer ausgehenden Verbindung mit langer Lebensdauer. Auf Grundlage der Anforderung führt der Anwendungsproxy eine der folgenden Aktionen durch:

 * Der Connector bestätigt, ob er die Anwendung identifizieren kann. Wenn er die Anwendung nicht ermitteln kann, stellt der Connector eine Verbindung mit dem Anwendungsproxydienst her, um Details zur Anwendung zu erfassen, und führt eine lokale Zwischenspeicherung durch.

 * Wenn die Anforderung ein einfacher Vorgang ist, z.B. ohne Daten im Text wie bei einer *GET*-RESTful-Anforderung, stellt der Connector eine Verbindung mit der internen Zielressource her und wartet dann auf eine Antwort.

 * Wenn in der Anforderung im Textbereich Daten zugeordnet sind, z.B. bei einem *POST*-RESTful-Vorgang, stellt der Connector mit dem Clientzertifikat eine ausgehende Verbindung mit der Anwendungsproxyinstanz her. Die Verbindung wird zum Anfordern der Daten und zum Öffnen einer Verbindung mit der internen Ressource hergestellt. Nach Erhalt der Anforderung vom Connector beginnt der Anwendungsproxydienst damit, Inhalte vom Benutzer zu akzeptieren und Daten an den Connector weiterzuleiten. Der Connector leitet die Daten wiederum an die interne Ressource weiter.

4. Nachdem die Anforderung/Übertragung des gesamten Inhalts an das Back-End abgeschlossen ist, wartet der Connector auf eine Antwort.

5. Nach dem Erhalt einer Antwort stellt der Connector eine ausgehende Verbindung mit dem Anwendungsproxydienst her, um die Headerdetails zurückzugeben und mit dem Streamen der Rückgabedaten zu beginnen.

6. Der Anwendungsproxy „streamt“ die Daten an den Benutzer. Zu diesem Zeitpunkt können einige Verarbeitungsschritte, sofern erforderlich und von der Anwendung definiert, ausgeführt werden.

Falls Sie Hilfe zur Kommunikation von einer Azure-Webanwendung über einen Clientbrowser mit einem lokalen SOAP-Endpunkt (Simple Object Access Protocol) mit Windows-Authentifizierung benötigen, helfen Ihnen die Informationen im [Azure Field Notes Blog](http://www.azurefieldnotes.com/2016/12/02/claims-to-windows-identity-translation-solutions-and-its-flaws-when-using-azure-ad-application-proxy) weiter.

## <a name="next-steps"></a>Nächste Schritte

[Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-understand-connectors.md)

