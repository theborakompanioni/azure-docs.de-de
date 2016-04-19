
<properties
	pageTitle="Technische Referenz: Bedingter Zugriff auf Azure AD-Apps | Microsoft Azure"
	description="Mit der bedingten Zugriffssteuerung überprüft Azure Active Directory die besonderen Bedingungen, die Sie beim Authentifizieren des Benutzers und vor dem Gewähren des Zugriffs auf die Anwendung auswählen. Nachdem diese Bedingungen erfüllt sind, wird der Benutzer authentifiziert und erhält Zugriff auf die Anwendung."
    services="active-directory"
	documentationCenter=""
	authors="femila"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.devlang="na"
	ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
	ms.date="03/30/2016"
	ms.author="femila"/>

# Technische Referenz: Bedingter Zugriff auf Azure AD-Apps

## Dienste mit aktiviertem bedingtem Zugriff
Bedingte Zugriffsregeln werden über verschiedene Arten von Azure AD-Anwendungen hinweg unterstützt. Diese Liste enthält:

- Verbundanwendungen aus dem Azure AD-Anwendungskatalog
- Kennwort-SSO-Anwendungen aus dem Azure AD-Anwendungskatalog
- Beim Azure-Anwendungsproxy registrierte Anwendungen
- Entwickelte branchenspezifische und mehrinstanzenfähige Anwendungen, die bei Azure AD registriert sind
- Visual Studio Online
- Azure RemoteApp

## Aktivieren von Zugriffsregeln

Jede Regel kann auf Anwendungsbasis aktiviert oder deaktiviert werden. Wenn Regeln auf **ON** festgelegt sind, sind sie aktiviert und werden für Benutzer durchgesetzt, die auf die Anwendung zugreifen. Wenn sie auf **OFF** festgelegt sind, werden sie nicht verwendet und haben keine Auswirkungen auf den Anmeldevorgang der Benutzer.

## Anwenden von Regeln auf bestimmte Benutzer
Regeln können basierend auf Sicherheitsgruppen auf bestimmte Gruppen von Benutzern angewendet werden, indem Sie **Anwenden auf** einstellen. **Anwenden auf** kann auf **Alle Benutzer** oder auf **Gruppen** festgelegt werden. Bei der Einstellung **Alle Benutzer** gelten die Regeln für alle Benutzer mit Zugriff auf die Anwendung. Die Option **Gruppen** ermöglicht die Auswahl bestimmter Sicherheits- und Verteilergruppen, für die die Regeln durchgesetzt werden.

  Wenn Sie eine Regel bereitstellen, ist es üblich, sie zuerst auf eine begrenzte Anzahl von Benutzern anzuwenden, die Mitglieder einer Pilotgruppe sind. Die Regel kann nach der Fertigstellung auf **Alle Benutzer** angewendet werden. Dadurch wird die Regel zwingend für alle Benutzer in der Organisation durchgesetzt.

Mithilfe der Option **Ausgenommen** können ausgewählte Gruppen auch von der Richtlinie ausgenommen werden. Dies gilt für alle Mitglieder der jeweiligen Gruppen, selbst dann, wenn sie in einer eingeschlossenen Gruppe angezeigt werden.

## Arbeitsnetzwerke


Regeln für den bedingten Zugriff, die ein Arbeitsplatznetzwerk verwenden, greifen auf vertrauenswürdige IP-Adressbereiche zurück, die in Azure AD konfiguriert wurden. Zu diesen Regeln gehören:

- Erfordert mehrstufige Authentifizierung, wenn nicht bei der Arbeit
- Zugriff blockieren, wenn nicht gearbeitet wird

Vertrauenswürdige IP-Adressbereiche können auf der [Konfigurationsseite für die mehrstufige Authentifizierung](../multi-factor-authentication/multi-factor-authentication-whats-next.md) konfiguriert werden. Die Richtlinie für den bedingten Zugriff verwendet die konfigurierten Bereiche für jede Authentifizierungsanfrage und Tokenausstellung zum Auswerten von Regeln. Der innerhalb des Unternehmensnetzwerks geltende Anspruch wird nicht verwendet, da er für längere Sitzungen, z. B. Aktualisierungstoken in mobilen Anwendungen, nicht verfügbar ist.

## Regeln pro Anwendung
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

<!---HONumber=AcomDC_0406_2016-->