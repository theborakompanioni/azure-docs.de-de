<properties
	pageTitle="Häufig gestellte Fragen zum bedingten Zugriff | Microsoft Azure"
	description="Häufig gestellte Fragen zum bedingten Zugriff "
	services="active-directory"
	documentationCenter=""
	authors="femila"
	manager="swadhwa"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/23/2016"
	ms.author="femila"/>

# Häufig gestellte Fragen zum bedingten Zugriff

## Welche Anwendungen arbeiten mit Richtlinien für den bedingten Zugriff?
Informationen dazu finden Sie im Thema [Bedingter Zugriff – unterstützte Anwendungen](active-directory-conditional-access-supported-apps.md).

## Werden Richtlinien für den bedingten Zugriff für B2B-Zusammenarbeits- und Gastbenutzer erzwungen?
Richtlinien werden für B2B-Zusammenarbeitsbenutzer erzwungen. In einigen Fällen erfüllt ein Benutzer möglicherweise die Richtlinienanforderungen nicht, beispielsweise dann, wenn die Organisation des Benutzers keine mehrstufige Authentifizierung unterstützt. Richtlinien werden zurzeit für SharePoint-Gastbenutzer nicht erzwungen. Die Gastbeziehung wird innerhalb von SharePoint verwaltet, Gastbenutzerkonten unterliegen daher keinen Zugriffsrichtlinien auf dem Authentifizierungsserver. Der Gastzugriff kann auf SharePoint verwaltet werden.

## Gelten SharePoint Online-Richtlinien auch für OneDrive for Business?
Ja.
 
## Warum kann ich keine Richtlinien für Clientanwendungen wie Word oder Outlook festlegen?
Eine Richtlinie für den bedingten Zugriff legt Anforderungen für den Zugriff auf einen Dienst fest und wird erzwungen, wenn die Authentifizierung für diesen Dienst erfolgt. Die Richtlinie wird also nicht direkt in einer Clientanwendung festgelegt, sondern wird beim Aufrufen eines Diensts angewendet. Eine für SharePoint festgelegte Richtlinie wird auf Clients angewendet, die SharePoint aufrufen. Eine für Exchange festgelegte Richtlinie wird auf Outlook angewendet.

<!---HONumber=AcomDC_0629_2016-->