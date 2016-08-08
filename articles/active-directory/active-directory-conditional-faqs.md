<properties
	pageTitle="Häufig gestellte Fragen zum bedingten Zugriff mit Azure Active Directory | Microsoft Azure"
	description="Häufig gestellte Fragen zum bedingten Zugriff "
	services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="swadhwa"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/26/2016"
	ms.author="markvi"/>

# Häufig gestellte Fragen zum bedingten Zugriff mit Azure Active Directory

## Welche Anwendungen arbeiten mit Richtlinien für den bedingten Zugriff?

**A**: Informationen dazu finden Sie im Thema [Bedingter Zugriff – unterstützte Anwendungen](active-directory-conditional-access-supported-apps.md).

## Werden Richtlinien für den bedingten Zugriff für B2B-Zusammenarbeits- und Gastbenutzer erzwungen?

**A**: Richtlinien werden für B2B-Zusammenarbeitsbenutzer erzwungen. In einigen Fällen erfüllt ein Benutzer möglicherweise die Richtlinienanforderungen nicht, beispielsweise dann, wenn eine Organisation keine Multi-Factor Authentication unterstützt.

Die Richtlinie wird zurzeit für SharePoint-Gastbenutzer nicht erzwungen. Die Gastbeziehung wird innerhalb von SharePoint verwaltet. Gastbenutzerkonten unterliegen daher keinen Zugriffsrichtlinien auf dem Authentifizierungsserver. Der Gastzugriff kann auf SharePoint verwaltet werden.

## Gelten SharePoint Online-Richtlinien auch für OneDrive for Business?

**A:** Ja.
 
## Warum kann ich keine Richtlinien für Clientanwendungen wie Word oder Outlook festlegen?

**A**: Eine Richtlinie für den bedingten Zugriff legt Anforderungen für den Zugriff auf einen Dienst fest und wird erzwungen, wenn die Authentifizierung für diesen Dienst erfolgt. Die Richtlinie wird also nicht direkt in einer Clientanwendung festgelegt, sondern wird beim Aufrufen eines Diensts angewendet. Eine für SharePoint festgelegte Richtlinie wird beispielsweise auf Clients angewendet, die SharePoint aufrufen. Eine für Exchange festgelegte Richtlinie wird auf Outlook angewendet.


## Gilt eine Richtlinie für bedingten Zugriff für Dienstkonten?

**A**: Richtlinien für bedingten Zugriff gelten für alle Benutzerkonten. Dies schließt Benutzerkonten ein, die als Dienstkonten verwendet werden. In vielen Fällen kann ein unbeaufsichtigt ausgeführtes Dienstkonto keine Richtlinie erfüllen, z. B. wenn eine MFA erforderlich ist. In diesen Fällen können Dienstkonten über die Einstellungen zur Verwaltung der Richtlinie für bedingten Zugriff von einer Richtlinie ausgeschlossen werden. Hier erfahren Sie mehr über das Anwenden einer Richtlinie für Benutzer.

<!---HONumber=AcomDC_0727_2016-->