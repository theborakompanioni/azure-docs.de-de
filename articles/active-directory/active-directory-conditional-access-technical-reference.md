
<properties
	pageTitle="Technische Referenz zum bedingten Azure Active Directory-Zugriff | Microsoft Azure"
	description="Mit der bedingten Zugriffssteuerung überprüft Azure Active Directory die besonderen Bedingungen, die Sie beim Authentifizieren des Benutzers und vor dem Gewähren des Zugriffs auf die Anwendung auswählen. Nachdem diese Bedingungen erfüllt sind, wird der Benutzer authentifiziert und erhält Zugriff auf die Anwendung."
    services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.devlang="na"
	ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
	ms.date="07/26/2016"
	ms.author="markvi"/>

# Technische Referenz zum bedingten Azure Active Directory-Zugriff

## Dienste mit aktiviertem bedingtem Zugriff
Bedingte Zugriffsregeln werden über verschiedene Arten von Azure AD-Anwendungen hinweg unterstützt. Diese Liste enthält:

- Verbundanwendungen aus dem Azure AD-Anwendungskatalog
- Kennwort-SSO-Anwendungen aus dem Azure AD-Anwendungskatalog
- Beim Azure-Anwendungsproxy registrierte Anwendungen
- Entwickelte branchenspezifische und mehrinstanzenfähige Anwendungen, die bei Azure AD registriert sind
- Visual Studio Online
- Azure RemoteApp
- 	Dynamics CRM
- Microsoft Office 365 Yammer
- Microsoft Office 365 Exchange Online
- Microsoft Office 365 SharePoint Online (einschließlich OneDrive for Business)


## Aktivieren von Zugriffsregeln

Jede Regel kann auf Anwendungsbasis aktiviert oder deaktiviert werden. Wenn Regeln auf **ON** festgelegt sind, sind sie aktiviert und werden für Benutzer durchgesetzt, die auf die Anwendung zugreifen. Wenn sie auf **OFF** festgelegt sind, werden sie nicht verwendet und haben keine Auswirkungen auf den Anmeldevorgang der Benutzer.

## Anwenden von Regeln auf bestimmte Benutzer
Regeln können basierend auf Sicherheitsgruppen auf bestimmte Gruppen von Benutzern angewendet werden, indem Sie **Anwenden auf** einstellen. **Anwenden auf** kann auf **Alle Benutzer** oder auf **Gruppen** festgelegt werden. Bei der Einstellung **Alle Benutzer** gelten die Regeln für alle Benutzer mit Zugriff auf die Anwendung. Die Option **Gruppen** ermöglicht die Auswahl bestimmter Sicherheits- und Verteilergruppen. Die Regeln werden nur für diese Gruppen durchgesetzt.

Wenn Sie eine Regel bereitstellen, ist es üblich, sie zuerst auf eine begrenzte Anzahl von Benutzern anzuwenden, die Mitglieder einer Pilotgruppe sind. Die Regel kann nach der Fertigstellung auf **Alle Benutzer** angewendet werden. Dadurch wird die Regel zwingend für alle Benutzer in der Organisation durchgesetzt.

Mithilfe der Option **Ausgenommen** können ausgewählte Gruppen auch von der Richtlinie ausgenommen werden. Dies gilt für alle Mitglieder der jeweiligen Gruppen, selbst dann, wenn sie in einer eingeschlossenen Gruppe angezeigt werden.

## Arbeitsnetzwerke


Regeln für den bedingten Zugriff, die ein Arbeitsplatznetzwerk verwenden, greifen auf vertrauenswürdige IP-Adressbereiche zurück, die in Azure AD konfiguriert wurden, oder verwenden den AD FS-Anspruch „innerhalb des Unternehmensnetzwerks“. Zu diesen Regeln gehören:

- Erfordert mehrstufige Authentifizierung, wenn nicht bei der Arbeit
- Zugriff blockieren, wenn nicht gearbeitet wird

Optionen zum Festlegen von Arbeitsnetzwerken

1. Konfigurieren Sie vertrauenswürdige IP-Adressbereiche auf der [Konfigurationsseite für die mehrstufige Authentifizierung](../multi-factor-authentication/multi-factor-authentication-whats-next.md). Die Richtlinie für den bedingten Zugriff verwendet die konfigurierten Bereiche für jede Authentifizierungsanfrage und Tokenausstellung zum Auswerten von Regeln.
2. Konfigurieren Sie die Verwendung des Anspruchs „innerhalb des Unternehmensnetzwerks“. Diese Option kann mit Verbundverzeichnissen und AD FS verwendet werden. [Erfahren Sie mehr über Ansprüche „innerhalb des Unternehmensnetzwerks“](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).
3. Konfigurieren Sie öffentliche IP-Adressbereiche. Auf der Registerkarte „Konfigurieren“ für Ihr Verzeichnis können Sie öffentliche IP-Adressen festlegen. Der bedingte Zugriff verwendet diese „arbeitsplatzbasierten“ IP-Adressen, mit denen zusätzlich zu dem auf der MFA-Einstellungsseite angegebenen Limit von 50 IP-Adressen weitere Adressbereiche konfiguriert werden können.



## Regeln auf Grundlage der Anwendungsvertraulichkeit

Regeln werden für jede Anwendung konfiguriert, damit hochwertige Dienste abgesichert werden, ohne den Zugriff auf andere Dienste zu beeinträchtigen. Regeln für den bedingten Zugriff können auf der Registerkarte **Konfigurieren** der Anwendung konfiguriert werden.

Regeln, die derzeit angeboten werden:

- **Erfordern von Multi-Factor Authentication**
 - Alle Benutzer, auf die die Richtlinie angewendet wird, müssen sich mindestens einmal per mehrstufiger Authentifizierung authentifizieren.
 
- **Erfordert mehrstufige Authentifizierung, wenn nicht bei der Arbeit**
 - Wenn diese Richtlinie angewendet wird, müssen alle Benutzer mindestens einmal die mehrstufige Authentifizierung durchgeführt haben, wenn sie von einem arbeitsfreien Remotestandort aus auf den Dienst zugreifen. Wenn sie von einem Unternehmensstandort an einen Remotestandort wechseln, müssen sie beim Zugriff auf den Dienst die mehrstufige Authentifizierung ausführen.
 
- **Zugriff blockieren, wenn nicht gearbeitet wird**
 - Wenn Benutzer von einem Unternehmensstandort an einen Remotestandort wechseln, werden sie blockiert, wenn für sie die Richtlinie „Zugriff blockieren, wenn nicht gearbeitet wird“ gilt. Wenn sie sich an einem Unternehmensstandort befinden, wird Ihnen der Zugriff wieder gewährt.


## Verwandte Themen

- [Sichern des Zugriffs auf Office 365 und andere mit Azure Active Directory verbundene Apps](active-directory-conditional-access.md)
- [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)

<!---HONumber=AcomDC_0727_2016-->