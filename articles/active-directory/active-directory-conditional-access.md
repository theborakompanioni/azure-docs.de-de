<properties
	pageTitle="Sichern des Zugriffs auf Office 365 und andere mit Azure Active Directory verbundene Apps | Microsoft Azure"  
    description="Mit der bedingten Zugriffssteuerung überprüft Azure Active Directory die besonderen Bedingungen, die Sie beim Authentifizieren des Benutzers und vor dem Gewähren des Zugriffs auf die Anwendung auswählen. Nachdem diese Bedingungen erfüllt sind, wird der Benutzer authentifiziert und erhält Zugriff auf die Anwendung."  
    services="active-directory" 
	keywords="bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff" 
	documentationCenter=""
	authors="femila"
	manager="swadhwa"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.devlang="na"
	ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
	ms.date="06/23/2016"
	ms.author="femila"/>


# Sichern des Zugriffs auf Office 365 und andere mit Azure Active Directory verbundene Apps  
  
In jedem Unternehmen ist das Sichern des Zugriffs auf Unternehmensressourcen von größter Bedeutung. Mit der Einführung von Clouddiensten und mobilen Geräten hat sich der Zugriff der Benutzer auf Unternehmensressourcen maßgeblich geändert. Dadurch werden Änderungen an der Strategie zum Sichern von Unternehmensressourcen erforderlich.
  
## Gründe für den bedingten Zugriff  
 Die Steuerungsfunktionen für den bedingten Zugriff in Azure Active Directory bieten einfache Methoden, mit denen Unternehmen ihre Ressourcen sowohl in der Cloud als auch lokal sichern können. Wenn Sie beispielsweise den Zugriff auf Ihre Ressourcen über ein gestohlenes Kennwort verhindern oder für den Zugriff auf Unternehmensinhalte ein zuverlässiges, verwaltetes Gerät durchsetzen möchten, ist Azure Active Directory genau das Richtige.

## Wie wird die bedingte Zugriffskontrolle durchgesetzt?  
 Mit der bedingten Zugriffssteuerung überprüft Azure Active Directory beim Authentifizieren des Benutzers die von Ihnen ausgewählten besonderen Bedingungen, bevor der Zugriff auf eine Anwendung gewährt wird. Sobald diese Zugriffsanforderungen erfüllt sind, wird der Benutzer authentifiziert und erhält Zugriff auf die Anwendung.
   
![](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## Benutzerbasierter Zugriffs auf Ressourcen
  
- **Benutzerattribute**: Auf Ebene der Benutzerattribute können Sie Richtlinien anwenden, um sicherzustellen, dass nur autorisierte Benutzer auf Unternehmensressourcen zugreifen können.
- **Gruppenmitgliedschaft eines Benutzers**: Sie können die Zugriffsebene für einen Benutzer auch über dessen Mitgliedschaft in einer oder mehreren Gruppen steuern.
- **Multi-Factor Authentication (MFA)**: Sie können auch Richtlinien erzwingen, bei denen ein Benutzer seine Identität über ein mehrstufiges Authentifizierungssystem authentifizieren muss. Beispielsweise können Sie einen Benutzer zum Bestätigen einer PIN auf einem persönlichen Mobiltelefon zwingen, um eine zusätzliche Sicherheitsebene zu gewährleisten. Die MFA schützt Ihre Ressourcen vor dem Zugriff durch einen nicht autorisierten Benutzer, der Zugriff auf den Benutzernamen und das Kennwort eines gültigen Benutzers erlangt hat.
- **Anmeldungs- und Benutzerrisiken**: Richtlinien zu Risiken beim bedingten Zugriff stehen mit Azure AD Identity Protection zur Verfügung und bieten erweiterten Schutz basierend auf Risikoereignissen und ungewöhnlichen Anmeldeaktivitäten.
 

## Gerätebasierter bedingter Zugriff 

**Registrierte Geräte**: Auf der Geräteebene können Sie Richtlinien festlegen, die erzwingen, dass nur über MDM (Mobile Device Management, Verwaltung mobiler Geräte) registrierte Geräte zugreifen dürfen. Um zu überprüfen, ob das Gerät registriert und kompatibel ist, wird Microsoft Intune verwendet. Durch den bedingten Zugriff auf Geräteebene wird sichergestellt, dass nur solche Geräte Zugriff erhalten, die mit Ihren Richtlinien kompatibel sind, z. B. mit dem Erzwingen der Dateiverschlüsselung auf einem Gerät. Darüber hinaus können Sie mit MDM-Lösungen sicherstellen, dass Unternehmensdaten auf einem verloren gegangenen/gestohlenen Gerät remote gelöscht werden können.
  

Die Zugriffsebene, die Sie mit diesen Richtlinien festlegen können, können auf Ressourcen in der Cloud oder lokal angewendet werden. Die Ressourcen in der Cloud könnten z. B. Apps wie Office 365 und SaaS-Apps von Drittanbietern sein. Ferner kann es sich um branchenspezifische Apps handeln, die Sie in der Cloud gehostet haben.
  
## Bedingter Zugriff – eine Inhaltszuordnung  
In der folgenden Inhaltszuordnung werden Dokumente aufgeführt, in denen Sie weitere Informationen über das Aktivieren des bedingten Zugriffs in Ihrer aktuellen Bereitstellung finden.


| Szenario | Artikel |
|------------------------------------------------------|----------|
| Schützen von Ressourcen basierend auf Authentifizierungsstärke oder Benutzer |[Bedingter Zugriff unter Azure – Vorschau für SaaS-Apps](active-directory-conditional-access-azuread-connected-apps.md)<br><br>[Unterstützung des bedingten Zugriffs für Anwendungen](active-directory-conditional-access-supported-apps.md)|
| Schützen von Unternehmensdaten auf verloren gegangenen/gestohlenen Geräten |[Schützen Ihrer Daten mit vollständigem oder selektivem Löschen über Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)|
|Schützen von Ressource basierend auf dem Anmelderisiko |[Azure AD Identity Protection](active-directory-identityprotection.md) |
| Additional Info |[Häufig gestellte Fragen zum bedingten Zugriff](active-directory-conditional-faqs.md)<br><br>[Technische Referenz](active-directory-conditional-access-technical-reference.md) |

<!---HONumber=AcomDC_0629_2016-->