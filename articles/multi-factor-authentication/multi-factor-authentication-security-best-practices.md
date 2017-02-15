---
title: "Bewährte Sicherheitsmethoden für MFA | Microsoft Docs"
description: "In diesem Dokument sind bewährte Methoden bei Verwendung von Azure MFA mit Azure-Konten beschrieben"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 3be7d968-96bb-4320-8701-869fd04a2595
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 2471a8daa91113c1865507239361d093f8695e30


---
# <a name="security-best-practices-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Bewährte Sicherheitsmethoden beim Einsatz von Azure Multi-Factor Authentication mit Azure AD-Konten
Die Überprüfung in zwei Schritten ist für die meisten Organisationen die bevorzugte Option, wenn de Authentifizierungsprozesses verbessert werden soll. Azure MFA (Multi-Factor Authentication) ermöglicht es Unternehmen, ihre Sicherheits- und Complianceanforderungen zu erfüllen und gleichzeitig einen benutzerfreundlichen Anmeldevorgang bereitzustellen. In diesem Artikel werden verschiedene bewährte Methoden erläutert, die Sie beim Planen der Einführung von Azure MFA berücksichtigen sollten.

## <a name="best-practices-for-azure-multi-factor-authentication-in-the-cloud"></a>Best Practices für Azure Multi-Factor Authentication in der Cloud
Um die Überprüfung in zwei Schritten für all Ihre Benutzer zu ermöglichen und von den erweiterten Features zu profitieren, die Azure MFA bietet, müssen Sie Azure MFA für all Ihre Benutzer aktivieren.  Dies erreichen Sie mit einer der folgenden Implementierungen:

* Azure AD Premium oder Enterprise Mobility Suite
* Multi-Factor Authentication-Anbieter

### <a name="azure-ad-premiumenterprise-mobility-suite"></a>Azure AD Premium/Enterprise Mobility Suite
![EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

Um Azure MFA unter Verwendung von Azure AD Premium oder Enterprise Mobility Suite in der Cloud zu nutzen, müssen Sie Ihren Benutzern zunächst Lizenzen zuweisen.  Da Azure Multi-Factor Authentication Teil dieser Suites sind, benötigt Ihre Organisation keine weiteren Komponenten, um die Überprüfung in zwei Schritten für alle Benutzer zu implementieren. Sie müssen nur einem Benutzer eine Lizenz zuweisen und können dann MFA aktivieren.

Berücksichtigen Sie bei der Einrichtung von Multi-Factor Authentication folgende Aspekte:

* Sie müssen keinen Multi-Factor Authentication-Anbieter erstellen.  Azure Multi-Factor Authentication ist sowohl in Azure AD Premium als auch in Enterprise Mobility Suite bereits enthalten.  
* Azure AD Connect ist nur erforderlich, wenn Sie Ihre lokale Active Directory-Umgebung mit einem Azure AD-Verzeichnis synchronisieren. Wenn Sie ein Azure AD-Verzeichnis verwenden, das nicht mit einer lokalen Active Directory-Instanz synchronisiert wird, ist Azure AD Connect nicht erforderlich.

### <a name="multi-factor-auth-provider"></a>Multi-Factor Authentication-Anbieter
![Multi-Factor Authentication-Anbieter](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Wenn Sie nicht über Azure AD Premium oder die Enterprise Mobility Suite verfügen, müssen Sie zunächst einen MFA-Authentifizierungsanbieter erstellen, bevor Sie Azure MFA in der Cloud einführen können. Obwohl MFA standardmäßig für globale Administratoren mit Azure Active Directory verfügbar ist, müssen Sie die Überprüfung in zwei Schritten auf alle Benutzer erweitern, wenn Sie MFA für Ihre Organisation bereitstellen. Zu diesem Zweck ist ein Multi-Factor Authentication-Anbieter erforderlich.
Bei der Auswahl des Authentifizierungsanbieters müssen Sie ein Verzeichnis auswählen und Folgendes berücksichtigen:

* Zum Erstellen eines Anbieters für mehrstufige Authentifizierung benötigen Sie kein Azure AD-Verzeichnis. Sie müssen den Multi-Factor Authentication-Anbieter jedoch einem Azure AD-Verzeichnis zuordnen, um die folgenden Funktionen nutzen zu können:  
  * Erweitern der Überprüfung in zwei Schritten auf alle Benutzer  
  * Bieten Sie Ihren globalen Benutzern zusätzliche Features wie z.B. das Verwaltungsportal, benutzerdefinierte Begrüßungen und Berichte.
* DirSync oder AAD Sync sind nur erforderlich, wenn Sie Ihre lokale Active Directory-Umgebung mit einem Azure AD-Verzeichnis synchronisieren. Wenn Sie ein Azure AD-Verzeichnis verwenden, das nicht mit einer lokalen Instanz von Active Directory synchronisiert wird, benötigen Sie weder DirSync noch AAD Sync.
* Stellen Sie sicher, dass Sie das richtige Nutzungsmodell für Ihr Unternehmen auswählen (pro Authentifizierung oder pro aktiviertem Benutzer). Das Nutzungsmodell kann nach der Auswahl nicht mehr geändert werden.
  * Pro Authentifizierung: Jede Überprüfung wird abgerechnet. Verwenden Sie dieses Modell, wenn Sie die Überprüfung in zwei Schritten für jede Person verwenden möchten, die auf eine bestimmte App zugreift, nicht nur für bestimmte Benutzer.
  * Pro aktiviertem Benutzer: Jeder Benutzer, den Sie für Azure MFA aktivieren, wird abgerechnet. Verwenden Sie dieses Modell, wenn einige Ihrer Benutzer Azure AD Premium- oder Enterprise Mobility Suite-Lizenzen verwenden, andere dagegen nicht.

### <a name="user-account"></a>Benutzerkonto
Wenn Sie Azure MFA für Ihre Benutzer aktivieren, können Benutzerkonten einen von drei Status aufweisen: deaktiviert, aktiviert oder erzwungen.
Befolgen Sie die unten stehenden Richtlinien, um sicherzustellen, dass Sie die am besten geeignete Option für Ihre Bereitstellung verwenden:

* Wenn der Status eines Benutzers auf **deaktiviert** festgelegt ist, führ dieser Benutzer die Überprüfung in zwei Schritten nicht aus. Dies ist die Standardeinstellung.
* Wenn der Status eines Benutzers auf **aktiviert** festgelegt ist, ist der Benutzer aktiviert, hat aber den Registrierungsprozess noch nicht abgeschlossen. Er wird beim nächsten Anmelden aufgefordert, den Vorgang abzuschließen. Diese Einstellung hat keine Auswirkungen auf nicht browserbasierte Apps. Alle Apps werden weiterhin ausgeführt, bis die Registrierung abgeschlossen ist.
* Wenn der Status eines Benutzers auf **erzwungen** festgelegt ist, hat er die Registrierung möglicherweise abgeschlossen, dies ist jedoch nicht unbedingt der Fall. Wenn der Benutzer die Registrierung abgeschlossen hat, verwendet er die Überprüfung in zwei Schritten. Anderenfalls wird der Benutzer beim nächsten Anmelden aufgefordert, die Registrierung abzuschließen. Diese Einstellung hat Auswirkungen auf nicht browserbasierte Apps. Diese Apps funktionieren erst, wenn App-Kennwörter erstellt und verwendet werden.

Verwenden Sie die Vorlage für Benutzerbenachrichtigungen im Artikel [Erste Schritte mit Azure Multi-Factor Authentication in der Cloud](multi-factor-authentication-get-started-cloud.md) , um eine E-Mail mit Informationen zur MFA-Implementierung an Ihre Benutzer zu senden.

### <a name="supportability"></a>Unterstützungsmöglichkeiten
Die meisten Benutzer sind es gewöhnt, für die Authentifizierung lediglich Kennwörter zu verwenden. Daher ist es wichtig, dass Ihr Unternehmen das Bewusstsein für diesen Prozess bei sämtlichen Benutzern fördert. Durch dieses Bewusstsein werden Anrufe beim Helpdesk aufgrund kleinerer Probleme im Zusammenhang mit MFA vermieden.
In einigen Szenarien muss MFA jedoch vorübergehend deaktiviert werden. Befolgen Sie die unten stehenden Richtlinien für den Umgang mit diesen Szenarien:

* Stellen Sie sicher, dass Ihre technischen Supportmitarbeiter für Szenarien geschult sind, in denen mobile Apps oder Mobiltelefone keine Nachrichten oder Anrufe empfangen und Benutzer sich daher nicht anmelden können. Der technische Support kann eine Einmalumgehung aktivieren, damit sich der Benutzer einmalig anmelden kann, indem die Überprüfung in zwei Schritten umgangen wird. Die Umgehung ist vorübergehend und läuft nach der angegebenen Anzahl von Sekunden ab.
* Bei Bedarf können Sie die Funktion für vertrauenswürdige IPs in Azure MFA nutzen. Mit diesem Feature können Administratoren eines Mandanten oder Verbundmandanten die Überprüfung in zwei Schritten für Benutzer umgehen, die sich über das lokale Intranet des Unternehmens anmelden. Die Features sind für Azure AD-Mandanten verfügbar, die über Azure AD Premium-, Enterprise Mobility Suite- oder Azure Multi-Factor Authentication-Lizenzen verfügen.

## <a name="best-practices-for-azure-multi-factor-authentication-on-premises"></a>Bewährte Methoden für lokale Azure Multi-Factor Authentication-Implementierungen
Wenn Ihr Unternehmen die eigene Infrastruktur für die Implementierung von MFA nutzen möchte, muss ein lokaler Azure Multi-Factor Authentication-Server bereitgestellt werden. In der folgenden Abbildung sind die MFA-Serverkomponenten dargestellt:

![Multi-Factor Authentication-Anbieter](./media/multi-factor-authentication-security-best-practices/server.png)
\*Standardmäßig nicht installiert \**Standardmäßig installiert, jedoch nicht aktiviert

Der Azure Multi-Factor Authentication-Server kann zum Schützen von Cloudressourcen und lokalen Ressourcen eingesetzt werden, auf die über Azure AD-Konten zugegriffen wird. Zu diesem Zweck ist jedoch ein Verbund erforderlich.  Das heißt, dass Sie über AD FS verfügen und für die Verbunddienste einen Partnerverbund mit Ihrem Azure AD-Mandanten konfiguriert haben müssen.
Berücksichtigen Sie bei der Einrichtung des Multi-Factor Authentication-Servers folgende Aspekte:

* Wenn Sie Azure AD-Ressourcen unter Verwendung der Active Directory-Verbunddienste (Active Directory Federation Services, AD FS) schützen, erfolgt die erste Stufe der Authentifizierung lokal mithilfe von AD FS. Die zweite Stufe wird lokal mithilfe des Anspruchs durchgeführt.
* Es ist nicht erforderlich, dass der Azure Multi-Factor Authentication-Server auf dem AD FS-Verbundserver installiert ist. Allerdings muss der Multi-Factor Authentication-Adapter für AD FS auf einem Windows Server 2012 R2-Computer installiert sein, auf dem AD FS ausgeführt wird. Sie können den Server auf einem anderen Computer installieren, sofern es sich um eine unterstützte Version handelt, und den AD FS-Adapter separat auf dem AD FS-Verbundserver installieren. Im folgenden Verfahren finden Sie Anweisungen, wie Sie den Adapter separat installieren.
* Der Installations-Assistent für den Multi-Factor Authentication AD FS-Adapter erstellt die Sicherheitsgruppe „PhoneFactor Admins“ in Ihrem Active Directory und fügt dieser Gruppe dann Ihr AD FS-Dienstkonto hinzu. Es wird empfohlen, auf Ihrem Domänencontroller sicherzustellen, dass die Gruppe „PhoneFactor Admins“ auch wirklich erstellt wird und dass das AD FS-Dienstkonto ein Mitglied dieser Gruppe ist. Bei Bedarf fügen Sie das AD FS-Dienstkonto der Gruppe "PhoneFactor Admins" auf dem Domänencontroller manuell hinzu.

### <a name="user-portal"></a>Benutzerportal
Dieses Portal wird über eine IIS-Website (Internetinformationsdienste) ausgeführt, die Self-Service-Funktionen sowie umfangreiche Verwaltungsfunktionen für Benutzer bietet. Befolgen Sie die unten stehenden Richtlinien, um diese Komponente zu konfigurieren:

* IIS 6 oder höher erforderlich
* ASP.NET v2.0.507207 muss installiert und registriert sein
* Dieser Server kann in einem Umkreisnetzwerk bereitgestellt werden.

### <a name="app-passwords"></a>App-Kennwörter
Wenn für Ihre Organisation ein SSO-Verbund mit Azure AD konfiguriert ist und Sie Azure MFA verwenden möchten, sollten Sie bei Verwendung von App-Kennwörtern Folgendes berücksichtigen (beachten Sie, dass dies nur bei Verwendung von SSO in einer Verbundkonfiguration gilt):

* Das App-Kennwort wird von Azure AD überprüft, sodass der Verbund umgangen wird. Der Verbund wird nur beim Einrichten des App-Kennworts verwendet.
* Die Kennwörter der SSO-Verbundbenutzer werden in der Organisations-ID gespeichert. Wenn der Benutzer das Unternehmen verlässt, muss diese Information zur Organisations-ID übertragen werden, und zwar mithilfe von DirSync in Echtzeit. Nach dem Deaktivieren oder Löschen von Konten kann die Synchronisierung bis zu drei Stunden dauern, sodass das Deaktivieren bzw. Löschen des App-Kennworts in Azure AD verzögert wird.
* Lokale Einstellungen für die Clientzugriffssteuerung werden vom App-Kennwort nicht berücksichtigt.
* Für das App-Kennwort ist keine lokale Authentifizierung-Protokollierung / -Überwachungsfunktion verfügbar.
* Weitere Schulungen für Endbenutzer sind für den Microsoft Lync 2013-Client erforderlich.
* In bestimmten erweiterten Architekturentwürfen ist bei der Verwendung der Multi-Factor Authentication mit Clients möglicherweise eine Kombination aus Organisationsbenutzername/-kennwort sowie App-Kennwörtern erforderlich, je nachdem, wo die Authentifizierung stattfindet. Bei Clients, die sich bei einer lokalen Infrastruktur authentifizieren, verwenden Sie einen Organisationsbenutzernamen und ein Organisationskennwort. Für Clients, die bei Azure AD authentifizieren, verwenden Sie das App-Kennwort.
* Standardmäßig können keine Benutzer App-Kennwörter erstellen. Sollte dies in Ihrem Unternehmen oder in bestimmten Szenarien erforderlich sein, müssen Sie sicherstellen, dass die Option **Benutzern das Erstellen von App-Kennwörtern zum Anmelden bei Anwendungen gestatten, die nicht auf Browsern basieren** ausgewählt ist.

## <a name="additional-considerations"></a>Weitere Überlegungen
In der nachfolgenden Liste sind zusätzliche Überlegungen und bewährte Methoden für die einzelnen Komponenten aufgeführt, die lokal bereitgestellt werden:

| Methode | Beschreibung |
|:--- |:--- |
| [Active Directory-Verbunddienste](multi-factor-authentication-get-started-adfs.md) |Informationen zum Einrichten von Multi-Factor Authentication mit AD FS. |
| [RADIUS-Authentifizierung](multi-factor-authentication-get-started-server-radius.md) |Informationen über Einrichtung und Konfiguration des Azure MFA-Servers mit RADIUS. |
| [IIS-Authentifizierung](multi-factor-authentication-get-started-server-iis.md) |Informationen über Einrichtung und Konfiguration des Azure MFA-Servers mit IIS. |
| [Windows-Authentifizierung](multi-factor-authentication-get-started-server-windows.md) |Informationen über Einrichtung und Konfiguration des Azure MFA-Servers mit Windows-Authentifizierung. |
| [LDAP-Authentifizierung](multi-factor-authentication-get-started-server-ldap.md) |Informationen über Einrichtung und Konfiguration des Azure MFA-Servers mit LDAP-Authentifizierung. |
| [Remotedesktop-Gateway und Azure Multi-Factor Authentication-Server mithilfe von RADIUS](multi-factor-authentication-get-started-server-rdg.md) |Informationen zum Einrichten und Konfigurieren des Azure MFA-Servers mit Remotedesktop-Gateway unter Verwendung von RADIUS. |
| [Synchronisieren mit Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md) |Informationen zum Einrichten und Konfigurieren der Synchronisierung zwischen Active Directory und dem Azure MFA-Server. |
| [Bereitstellen des mobilen App-Webdienstes für den Azure Multi-Factor Authentication-Server](multi-factor-authentication-get-started-server-webservice.md) |Informationen über das Einrichten und Konfigurieren des Azure MFA-Server-Webdienstes. |
| [Erweiterte VPN-Konfiguration mit Azure Multi-Factor Authentication](multi-factor-authentication-advanced-vpn-configurations.md) |Informationen zum Konfigurieren von Cisco ASA-, Citrix Netscaler- und Juniper/Pulse-Geräten über sichere VPN-Verbindungen unter Verwendung von LDAP oder RADIUS. |

## <a name="additional-resources"></a>Zusätzliche Ressourcen
In diesem Artikel sind einige bewährte Methoden für Azure MFA beschrieben. Zusätzlich sind weitere Ressourcen verfügbar, die Sie bei der Planung Ihrer MFA-Bereitstellung nutzen können.  In der nachfolgenden Liste sind einige wichtige Artikel aufgeführt, die Sie bei diesen Aufgaben unterstützen können:

* [Berichte in Azure Multi-Factor Authentication](multi-factor-authentication-manage-reports.md)
* [Einrichten von Azure Multi-Factor Authentication](multi-factor-authentication-end-user-first-time.md)
* [Azure Multi-Factor Authentication – Häufig gestellte Fragen](multi-factor-authentication-faq.md)




<!--HONumber=Nov16_HO3-->


