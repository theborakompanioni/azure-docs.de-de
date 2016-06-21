<properties
	pageTitle="Übersicht über Enterprise State Roaming | Microsoft Azure"
	description="Enthält Informationen zu Enterprise State Roaming-Einstellungen auf Windows-Geräten. Beim Enterprise State Roaming profitieren Benutzer von einer einheitlichen Benutzeroberfläche auf allen Windows-Geräten und von einer Reduzierung der Zeit, die zum Konfigurieren eines neuen Geräts benötigt wird."
	services="active-directory"
    keywords="was ist Enterprise State Roaming, Synchronisierung in Unternehmen, Windows-Cloud"
	documentationCenter=""
	authors="femila"
	manager="swadhwa"
	editor="curtand"/>

<tags
	ms.service="active-directory"  
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/07/2016"
	ms.author="femila"/>

# Übersicht über Enterprise State Roaming

Unter Windows 10 haben [Azure Active Directory (Azure AD)](active-directory-whatis.md)-Benutzer die Möglichkeit, die Daten ihrer Benutzer- und Anwendungseinstellungen sicher in der Cloud zu synchronisieren. Beim Enterprise State Roaming profitieren Benutzer von einer einheitlichen Benutzeroberfläche auf allen Windows-Geräten und von einer Reduzierung der Zeit, die zum Konfigurieren eines neuen Geräts benötigt wird. Enterprise State Roaming funktioniert ähnlich wie die standardmäßige [Synchronisierung von Verbrauchereinstellungen](http://windows.microsoft.com/de-DE/windows-8/sync-settings-pcs) die unter Windows 8 eingeführt wurde. Enterprise State Roaming bietet außerdem:

- **Trennung von Unternehmens- und Verbraucherdaten** – Unternehmen haben die Kontrolle über ihre Daten, und es erfolgt kein Vermischen von Unternehmensdaten in einem Verbraucher-Cloudkonto oder Verbraucherdaten in einem Unternehmens-Cloudkonto.
- **Erweiterte Sicherheit** – bevor Daten das Windows 10-Gerät des Benutzers verlassen, werden sie mithilfe von Azure Rights Management (Azure RMS) automatisch verschlüsselt und bleiben in der Cloud verschlüsselt im Ruhezustand. Alle Inhalte bleiben verschlüsselt im Ruhezustand in der Cloud, mit Ausnahme der Namespaces, z. B. Namen von Einstellungen und Windows-Apps.  
- **Bessere Verwaltung und Überwachung** – dank Azure AD-Portalintegration mehr Kontrolle und Transparenz dazu, welche Personen auf welchen Geräten in Ihrem Unternehmen Einstellungen synchronisieren. 

Enterprise State Roaming ist in mehreren Azure-Regionen verfügbar. Die aktuelle Liste mit verfügbaren Regionen finden Sie auf der Seite [Azure-Dienste nach Region](https://azure.microsoft.com/regions/#services) unter „Azure Active Directory“.



| Artikel | Beschreibung |
|--------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Aktivieren von Enterprise State Roaming in Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md) | Enterprise State Roaming ist für Organisationen mit einem Premium Azure Active Directory-Abonnement (Azure AD) verfügbar. Weitere Informationen zum Erhalt eines Azure AD-Abonnements finden Sie auf der [Azure AD-Produktseite](https://azure.microsoft.com/services/active-directory). |
| [Roaming von Einstellungen und Daten – Häufig gestellte Fragen](active-directory-windows-enterprise-state-roaming-faqs.md) | Dieses Thema enthält Antworten auf einige Fragen, die für IT-Administratoren in Bezug auf die Synchronisierung von Einstellungen und App-Daten unter Umständen interessant sind. |
| [Gruppenrichtlinien- und MDM-Einstellungen für Einstellungssynchronisierung](active-directory-windows-enterprise-state-roaming-group-policy-settings.md) | Windows 10 bietet Gruppenrichtlinien- und MDM-Einstellungen (Mobile Device Management, Verwaltung mobiler Geräte), um die Synchronisierung von Einstellungen zu beschränken. |
| [Windows 10-Roamingeinstellungen – Referenz](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md) | Hier ist eine vollständige Liste mit allen Einstellungen angegeben, für die unter Windows 10 das Roaming und/oder eine Sicherung durchgeführt wird. |

<!---HONumber=AcomDC_0608_2016-->