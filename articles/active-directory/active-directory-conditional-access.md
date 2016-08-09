<properties
	pageTitle="Bedingter Zugriff mit Azure Active Directory | Microsoft Azure"  
    description="Mit der bedingten Zugriffssteuerung überprüft Azure Active Directory die besonderen Bedingungen, die Sie beim Authentifizieren des Benutzers und vor dem Gewähren des Zugriffs auf die Anwendung auswählen. Nachdem diese Bedingungen erfüllt sind, wird der Benutzer authentifiziert und erhält Zugriff auf die Anwendung."  
    services="active-directory" 
	keywords="bedingter Zugriff auf Apps, bedingter Zugriff mit Azure AD, sicherer Zugriff auf Unternehmensressourcen, Richtlinien für bedingten Zugriff" 
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


# Bedingter Zugriff mit Azure Active Directory   
  
In jedem Unternehmen ist das Sichern des Zugriffs auf Unternehmensressourcen von größter Bedeutung. Mit der Einführung von Clouddiensten und mobilen Geräten hat sich der Zugriff der Benutzer auf Unternehmensressourcen maßgeblich geändert. Aus diesem Grund ist ein neuer Sicherheitsansatz erforderlich.
  
## Gründe für den bedingten Zugriff  

Die Steuerungsfunktionen für den bedingten Zugriff in Azure Active Directory bieten einfache Methoden, mit denen Unternehmen Ressourcen in der Cloud und lokal sichern können. Richtlinien für bedingten Zugriff können verwendet werden, um für Schutz vor der Gefahr durch gestohlene oder per Phishing entwendete Anmeldeinformationen zu sorgen, indem die Multi-Factor Authentication erzwungen wird. Außerdem werden die Unternehmensdaten geschützt, indem ein von Intune verwaltetes Gerät verwendet werden muss, das den Zugriff auf vertrauliche Dienste gewährt.



## Lizenzanforderungen

Der bedingte Zugriff ist eine Funktion von [Azure AD Premium](http://www.microsoft.com/identity). Alle Benutzer, die beim Zugreifen auf eine Anwendung eine Richtlinie für bedingten Zugriff verwenden, müssen über eine Azure AD Premium-Lizenz verfügen. Weitere Informationen zur Nutzung finden Sie im [Unlicensed User report](https://aka.ms/utc5ix) (Bericht zu nicht lizenzierten Benutzern).





## Wie wird die bedingte Zugriffskontrolle durchgesetzt?  

Mit der bedingten Zugriffssteuerung überprüft Azure Active Directory beim Authentifizieren des Benutzers die von Ihnen ausgewählten besonderen Bedingungen, bevor der Zugriff auf eine Anwendung gewährt wird. Sobald diese Zugriffsanforderungen erfüllt sind, wird der Benutzer authentifiziert und erhält Zugriff auf die Anwendung.
   
![](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## Bedingungen
  
- **Gruppenmitgliedschaft**: Sie können die Zugriffsebene für einen Benutzer basierend auf seiner Mitgliedschaft in einer Gruppe steuern.

- **Standort**: Sie können den Standort des Benutzers verwenden, um MFA auszulösen und Steuerelemente zu blockieren, wenn sich ein Benutzer nicht in einem vertrauenswürdigen Netzwerk befindet.

- **Geräteplattform**: Sie können den Typ der Geräteplattform, z.B. iOS, Android, Windows Mobile und Windows, als Bedingung für die Anwendung der Richtlinie verwenden.

- **Gerät aktiviert**: Der Status „Gerät aktiviert/Gerät deaktiviert“ wird während der Auswertung der Geräterichtlinie überprüft. Wenn ein verlorenes oder gestohlenes Gerät im Verzeichnis deaktiviert wird, kann es nicht mehr verwendet werden, um Richtlinienanforderungen zu erfüllen.

- **Anmeldungs- und Benutzerrisiken**: Richtlinien zu Risiken beim bedingten Zugriff stehen mit Azure AD Identity Protection zur Verfügung und bieten erweiterten Schutz basierend auf Risikoereignissen und ungewöhnlichen Anmeldeaktivitäten.


## Steuerelemente
   
- **Multi Factor Authentication (MFA)**: Sie können per MFA eine sichere Authentifizierung erzwingen. MFA kann über Azure MFA oder einen lokalen MFA-Anbieter per AD FS bereitgestellt werden. MFA unterstützt den Schutz Ihrer Ressourcen vor dem Zugriff durch einen nicht autorisierten Benutzer, der Zugriff auf den Benutzernamen und das Kennwort eines gültigen Benutzers erlangt hat.

- **Blockieren**: Der Zugriff kann bedingungsorientiert angewendet werden, z.B. nach dem Standort des Benutzers. Beispielsweise kann der Zugriff blockiert werden, wenn sich ein Benutzer nicht in einem vertrauenswürdigen Netzwerk befindet.

- **Registrierte/Kompatible Geräte**: Auf der Geräteebene können Sie Richtlinien festlegen, die erzwingen, dass nur über MDM (Mobile Device Management, Verwaltung mobiler Geräte) registrierte und kompatible Geräte zugreifen dürfen. Um zu überprüfen, ob das Gerät registriert und kompatibel ist, wird Microsoft Intune verwendet. Durch den bedingten Zugriff auf Geräteebene wird dann sichergestellt, dass nur Geräten Zugriff gewährt wird, die mit der MDM-Richtlinie kompatibel sind.
 

## Anwendungen

- Die Zugriffsebene, die Sie mit diesen Richtlinien festlegen können, kann auf Anwendungen und Dienste in der Cloud oder lokal angewendet werden. Die Richtlinie wird direkt auf die Website oder den Dienst angewendet. Die Richtlinie wird dann für den Browserzugriff und für Anwendungen durchgesetzt, die auf den Dienst zugreifen. Die Liste mit den Diensten, die angewendet werden können, ist hier angegeben.


  
## Bedingter Zugriff – eine Inhaltszuordnung  
In der folgenden Inhaltszuordnung werden Dokumente aufgeführt, in denen Sie weitere Informationen über das Aktivieren des bedingten Zugriffs in Ihrer aktuellen Bereitstellung finden.


### MFA- und Standortrichtlinien

- [Erste Schritte mit bedingtem Zugriff auf verbundene Azure AD-Apps basierend auf Gruppen-, Standort- und MFA-Richtlinien](active-directory-conditional-access-azuread-connected-apps.md)
- [Unterstützte Anwendungsarten](active-directory-conditional-access-supported-apps.md)


### Geräterichtlinien

[Schützen von Daten durch vollständiges oder selektives Zurücksetzen mit Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)


### Schützen von Ressource basierend auf dem Anmelderisiko

[Azure AD Identity Protection](active-directory-identityprotection.md)

### Additional Info

- [Häufig gestellte Fragen zum bedingten Zugriff](active-directory-conditional-faqs.md)
- [Technische Referenz](active-directory-conditional-access-technical-reference.md)

<!---HONumber=AcomDC_0727_2016-->