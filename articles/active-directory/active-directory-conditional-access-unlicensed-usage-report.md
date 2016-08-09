<properties
	pageTitle="Bericht zu nicht lizenzierter Nutzung | Microsoft Azure"
	description="Der Bericht zu nicht lizenzierter Nutzung hilft Ihnen dabei, nicht lizenzierte Benutzer zu finden, die kostenpflichtige Azure AD-Funktionen nutzen."
	services="active-directory"
	documentationCenter=""
	authors="markusvi"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/25/2016"
	ms.author="markvi"/>

# Bericht zu nicht lizenzierter Nutzung

Der Bericht zu nicht lizenzierter Nutzung hilft Ihnen dabei, nicht lizenzierte Benutzer zu finden, die kostenpflichtige Azure AD-Funktionen nutzen. Somit können Sie Ihre gekauften Lizenzen besser nutzen und leichter ermitteln, wann Sie weitere Lizenzen benötigen.

Der Bericht zeigt, welche kostenpflichtigen Funktionen in den letzten 30 Tagen aktiv genutzt wurden.

## Berichtsstruktur
 
| Spaltenname |	Beschreibung |
| :--                  | :--         |
| Nicht lizenzierter Benutzer |	Name des Benutzers |
| Funktion | Name des Features. Beispiel: Bedingter Zugriff |
| Genutzte Anwendung | Der Name der Anwendung, auf die mit der Funktion zugegriffen wird. Beispiel: Office 365 SharePoint Online |

 
> [AZURE.NOTE] Wenn ein Benutzerkonto gelöscht wurde, wird eine ID wie z. B. 1003000090D8B285 in der Spalte „Nicht lizenzierte Benutzer“ angezeigt.


## Feature für den bedingten Zugriff

Nicht lizenzierte Benutzer werden beim Zugriff auf einen Dienst, auf den eine Richtlinie für den bedingten Zugriff angewendet wird, gekennzeichnet, wenn sie nicht über eine Azure AD Premium-Lizenz verfügen.

Dies gilt für MFA-/Standortrichtlinien sowie Geräterichtlinien, die Intune verwenden.
 

## Weitere Informationen

- [Verwenden von bedingtem Zugriff mit Office 365 und anderen verbundenen Azure Active Directory-Apps](active-directory-conditional-access.md)
- [Erste Schritte mit bedingtem Zugriff auf Azure AD](active-directory-conditional-access-azuread-connected-apps.md)

<!---HONumber=AcomDC_0727_2016-->