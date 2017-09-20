---
title: "Bewährte Sicherheitsmethoden für MFA | Microsoft Docs"
description: "In diesem Dokument sind bewährte Methoden bei Verwendung von Azure MFA mit Azure-Konten beschrieben"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 3be7d968-96bb-4320-8701-869fd04a2595
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 6166e20a4652379dc9eeffe522feb651f5e57ea2
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="security-best-practices-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Bewährte Sicherheitsmethoden beim Einsatz von Azure Multi-Factor Authentication mit Azure AD-Konten

Die Überprüfung in zwei Schritten ist für die meisten Organisationen die bevorzugte Option, wenn der Authentifizierungsprozess verbessert werden soll. Azure Multi-Factor Authentication (MFA) unterstützt Unternehmen dabei, ihre Sicherheits- und Complianceanforderungen zu erfüllen und gleichzeitig einen einfachen Anmeldevorgang für ihre Benutzer bereitzustellen. In diesem Artikel werden verschiedene Tipps erläutert, die Sie beim Planen der Einführung von Azure MFA berücksichtigen sollten.

## <a name="deploy-azure-mfa-in-the-cloud"></a>Bereitstellen von Azure MFA in der Cloud

Es gibt zwei Möglichkeiten, Azure MFA für all Ihre Benutzer zu aktivieren.

* Sie erwerben Lizenzen für jeden Benutzer (Azure MFA, Azure AD Premium oder Enterprise Mobility + Security).
* Sie erstellen einen Multi-Factor Authentication-Anbieter und zahlen pro Benutzer oder pro Authentifizierung.

### <a name="licenses"></a>Lizenzen
![EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

Wenn Sie Azure AD Premium- oder Enterprise Mobility + Security-Lizenzen besitzen, verfügen Sie bereits über Azure MFA. Ihre Organisation benötigt keine weiteren Komponenten, um die Überprüfung in zwei Schritten für alle Benutzer zu implementieren. Sie müssen nur einem Benutzer eine Lizenz zuweisen und können dann MFA aktivieren.

Berücksichtigen Sie bei der Einrichtung der Multi-Factor Authentication folgende Hinweise:

* Erstellen Sie keinen Multi-Factor Authentication-Anbieter mit Bezahlung pro Authentifizierung. Wenn Sie es tun, bezahlen Sie möglicherweise für die Überprüfung von Anforderungen von Benutzern, die bereits über Lizenzen verfügen.
* Wenn Sie nicht über genügend Lizenzen für all Ihre Benutzer verfügen, können Sie einen Multi-Factor Authentication-Anbieter mit Bezahlung pro Benutzer erstellen, um Lizenzen für den Rest Ihrer Organisation bereitzustellen. 
* Azure AD Connect ist nur erforderlich, wenn Sie Ihre lokale Active Directory-Umgebung mit einem Azure AD-Verzeichnis synchronisieren. Wenn Sie ein Azure AD-Verzeichnis verwenden, das nicht mit einer lokalen Active Directory-Instanz synchronisiert wird, ist Azure AD Connect nicht erforderlich.

### <a name="multi-factor-auth-provider"></a>Multi-Factor Authentication-Anbieter
![Multi-Factor Authentication-Anbieter](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Wenn Sie über keine Lizenzen verfügen, die Azure MFA enthalten, können Sie einen MFA-Authentifizierungsanbieter erstellen. 

Beim Erstellen des Authentifizierungsanbieters müssen Sie ein Verzeichnis auswählen und Folgendes berücksichtigen:

* Zum Erstellen eines Anbieters für mehrstufige Authentifizierung ist ein Azure AD-Verzeichnis nicht unbedingt erforderlich, bietet Ihnen aber mehr Funktionalität. Folgende Features stehen zur Verfügung, wenn Sie den Authentifizierungsanbieter zu einem Azure AD-Verzeichnis zuordnen:  
  * Erweitern der Überprüfung in zwei Schritten auf alle Benutzer  
  * Bieten Sie Ihren globalen Benutzern zusätzliche Features wie z.B. das Verwaltungsportal, benutzerdefinierte Begrüßungen und Berichte.
* Wenn Sie Ihre lokale Active Directory-Umgebung mit einem Azure AD-Verzeichnis synchronisieren, ist entweder DirSync oder AAD Sync erforderlich. Wenn Sie ein Azure AD-Verzeichnis verwenden, das nicht mit einer lokalen Instanz von Active Directory synchronisiert wird, benötigen Sie weder DirSync noch AAD Sync.
* Wählen Sie das Nutzungsmodell aus, das am besten zu Ihrem Unternehmen passt. Das Nutzungsmodell kann nach der Auswahl nicht mehr geändert werden. Diese beiden Modelle stehen zur Auswahl:
  * Pro Authentifizierung: Jede Überprüfung wird abgerechnet. Verwenden Sie dieses Modell, wenn Sie die Überprüfung in zwei Schritten für jede Person verwenden möchten, die auf eine bestimmte App zugreift, nicht nur für bestimmte Benutzer.
  * Pro aktiviertem Benutzer: Jeder Benutzer, den Sie für Azure MFA aktivieren, wird abgerechnet. Verwenden Sie dieses Modell, wenn einige Ihrer Benutzer Azure AD Premium- oder Enterprise Mobility Suite-Lizenzen verwenden, andere dagegen nicht.

### <a name="supportability"></a>Unterstützungsmöglichkeiten
Die meisten Benutzer sind es gewöhnt, für die Authentifizierung lediglich Kennwörter zu verwenden. Daher ist es wichtig, dass Ihr Unternehmen das Bewusstsein für diesen Prozess bei sämtlichen Benutzern fördert. Dadurch lassen sich Anrufe beim Helpdesk aufgrund kleinerer Probleme im Zusammenhang mit MFA reduzieren. In einigen Szenarien muss MFA jedoch vorübergehend deaktiviert werden. Befolgen Sie die unten stehenden Richtlinien für den Umgang mit diesen Szenarien:

* Schulen Sie Ihre technischen Supportmitarbeiter für Szenarios, in denen Benutzer sich nicht anmelden können, weil mobile Apps oder Mobiltelefone keine Nachrichten oder Anrufe empfangen. Der technische Support kann eine [Einmalumgehung aktivieren](multi-factor-authentication-whats-next.md#one-time-bypass), mit der die Überprüfung in zwei Schritten umgangen wird, damit der Benutzer sich einmalig anmelden kann. Die Umgehung ist vorübergehend und läuft nach einer angegebenen Anzahl von Sekunden ab.
* [Vertrauenswürdige IPs](multi-factor-authentication-whats-next.md#trusted-ips) in Azure MFA sind eine Möglichkeit, die Überprüfung in zwei Schritten auf ein Minimum zu reduzieren. Mit diesem Feature können Administratoren eines verwalteten Mandanten oder Verbundmandanten die Überprüfung in zwei Schritten für Benutzer umgehen, die sich über das lokale Intranet des Unternehmens anmelden. Die Features sind für Azure AD-Mandanten verfügbar, die über Azure AD Premium-, Enterprise Mobility Suite- oder Azure Multi-Factor Authentication-Lizenzen verfügen.

## <a name="best-practices-for-an-on-premises-deployment"></a>Bewährte Methoden für lokale Bereitstellungen
Wenn Ihr Unternehmen die eigene Infrastruktur für die Aktivierung der MFA nutzen möchte, müssen Sie einen lokalen Azure Multi-Factor Authentication-Server bereitstellen. In der folgenden Abbildung sind die MFA-Serverkomponenten dargestellt:

![Standardkomponenten für MFA-Server: Konsole, Synchronisierungsmodul, Verwaltungsportal, Clouddienst](./media/multi-factor-authentication-security-best-practices/server.png) \*Nicht standardmäßig installiert \**Installiert, aber nicht standardmäßig aktiviert

Der Microsoft Azure Multi-Factor Authentication-Server kann Cloudressourcen und lokale Ressourcen durch Verwendung des Verbunds sichern. Sie müssen über AD FS verfügen und für die Verbunddienste einen Partnerverbund mit Ihrem Azure AD-Mandanten konfiguriert haben.
Berücksichtigen Sie bei der Einrichtung des Multi-Factor Authentication-Servers folgende Aspekte:

* Wenn Sie Azure AD-Ressourcen unter Verwendung der Active Directory-Verbunddienste (Active Directory Federation Services, AD FS) schützen, erfolgt der erste Schritt der Authentifizierung lokal mithilfe von AD FS. Der zweite Schritt wird lokal unter Berücksichtigung des Anspruchs ausgeführt.
* Sie müssen den Azure Multi-Factor Authentication-Server nicht auf dem AD FS-Verbundserver installieren. Allerdings muss der Multi-Factor Authentication-Adapter für AD FS auf einem Windows Server 2012 R2-Computer installiert sein, auf dem AD FS ausgeführt wird. Sie können den Server auf einem anderen Computer installieren, sofern es sich um eine unterstützte Version handelt, und den AD FS-Adapter separat auf dem AD FS-Verbundserver installieren. 
* Der Installations-Assistent für den Multi-Factor Authentication AD FS-Adapter erstellt die Sicherheitsgruppe „PhoneFactor Admins“ in Ihrem Active Directory und fügt dieser Gruppe dann Ihr AD FS-Dienstkonto hinzu. Vergewissern Sie sich, dass die Gruppe „PhoneFactor Admins“ auf Ihrem Domänencontroller wirklich erstellt wurde und dass das AD FS-Dienstkonto dieser Gruppe angehört. Bei Bedarf fügen Sie das AD FS-Dienstkonto der Gruppe "PhoneFactor Admins" auf dem Domänencontroller manuell hinzu.

### <a name="user-portal"></a>Benutzerportal
Das Benutzerportal erlaubt Self-Service-Funktionen und bietet umfangreiche Verwaltungsfunktionen für Benutzer. Es wird auf einer Website für Internet Information Server (IIS) ausgeführt. Befolgen Sie die unten stehenden Richtlinien, um diese Komponente zu konfigurieren:

* Verwenden Sie IIS 6 oder höher.
* Installieren und registrieren Sie ASP.NET v2.0.507207.
* Stellen Sie sicher, dass dieser Server in einem Umkreisnetzwerk bereitgestellt werden kann.

### <a name="app-passwords"></a>App-Kennwörter
Wenn für Ihr Unternehmen ein SSO-Verbund mit Azure AD konfiguriert ist und Sie Azure MFA verwenden möchten, sollten Sie folgende Details berücksichtigen:

* App-Kennwörter werden von Azure AD überprüft, sodass der Verbund umgangen wird. Der Verbund wird nur beim Einrichten von App-Kennwörtern verwendet.
* Die Kennwörter der SSO-Verbundbenutzer werden in der Organisations-ID gespeichert. Wenn der Benutzer das Unternehmen verlässt, muss diese Information zur Organisations-ID übertragen werden, und zwar mithilfe von DirSync. Nach dem Deaktivieren oder Löschen von Konten kann die Synchronisierung bis zu drei Stunden dauern, sodass sich das Deaktivieren bzw. Löschen von App-Kennwörtern in Azure AD verzögert.
* Lokale Einstellungen für die Clientzugriffssteuerung werden vom App-Kennwort nicht berücksichtigt.
* Für App-Kennwörter ist keine lokale Funktion zur Protokollierung oder Überwachung der Authentifizierung verfügbar.
* In bestimmten erweiterten Architekturentwürfen ist bei der Verwendung der Multi-Factor Authentication mit Clients möglicherweise eine Kombination aus Organisationsbenutzername/-kennwort sowie App-Kennwörtern erforderlich, je nachdem, wo die Authentifizierung stattfindet. Bei Clients, die sich bei einer lokalen Infrastruktur authentifizieren, verwenden Sie einen Organisationsbenutzernamen und ein Organisationskennwort. Für Clients, die bei Azure AD authentifizieren, verwenden Sie das App-Kennwort.
* Standardmäßig können keine Benutzer App-Kennwörter erstellen. Wenn Sie es Benutzern erlauben müssen, App-Kennwörter zu erstellen, wählen Sie die Option **Benutzern das Erstellen von App-Kennwörtern zum Anmelden bei Anwendungen gestatten, die nicht auf Browsern basieren** aus.

## <a name="additional-considerations"></a>Weitere Überlegungen
In der Liste sind zusätzliche Überlegungen und Anleitungen für jede Komponente aufgeführt, die lokal bereitgestellt werden:

- Einrichten der Multi-Factor Authentication mit [Active Directory-Verbunddiensten](multi-factor-authentication-get-started-adfs.md)
- Einrichten und Konfigurieren des Azure MFA-Servers mit [RADIUS-Authentifizierung](multi-factor-authentication-get-started-server-radius.md)
- Einrichten und Konfigurieren des Azure MFA-Servers mit [IIS-Authentifizierung](multi-factor-authentication-get-started-server-iis.md)
- Einrichten und Konfigurieren des Azure MFA-Servers mit [Windows-Authentifizierung](multi-factor-authentication-get-started-server-windows.md)
- Einrichten und Konfigurieren des Azure MFA-Servers mit [LDAP-Authentifizierung](multi-factor-authentication-get-started-server-ldap.md)
- Einrichten und Konfigurieren des Azure MFA-Servers mit [Remotedesktop-Gateway und Azure Multi-Factor Authentication-Server mithilfe von RADIUS](multi-factor-authentication-get-started-server-rdg.md)
- Einrichten und Konfigurieren der Synchronisierung zwischen dem Azure MFA-Server und [Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md)
- [Bereitstellen des mobilen App-Webdiensts für den Azure Multi-Factor Authentication-Server](multi-factor-authentication-get-started-server-webservice.md)
- [Erweiterte VPN-Konfiguration mit der Azure Multi-Factor Authentication](multi-factor-authentication-advanced-vpn-configurations.md) für Cisco ASA-, Citrix Netscaler und Juniper/Pulse Secure VPN-Appliances unter Verwendung von LDAP oder RADIUS

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel sind einige bewährte Methoden für Azure MFA beschrieben. Zusätzlich sind weitere Ressourcen verfügbar, die Sie bei der Planung Ihrer MFA-Bereitstellung nutzen können.  In der nachfolgenden Liste sind einige wichtige Artikel aufgeführt, die Sie bei diesen Aufgaben unterstützen können:

* [Berichte in Azure Multi-Factor Authentication](multi-factor-authentication-manage-reports.md)
* [Einrichten meines Kontos für die Überprüfung in zwei Schritten](multi-factor-authentication-end-user-first-time.md)
* [Azure Multi-Factor Authentication – Häufig gestellte Fragen](multi-factor-authentication-faq.md)


