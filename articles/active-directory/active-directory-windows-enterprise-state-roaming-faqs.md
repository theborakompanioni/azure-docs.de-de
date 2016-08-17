<properties
	pageTitle="Roaming von Einstellungen und Daten – Häufig gestellte Fragen | Microsoft Azure"
	description="Enthält Antworten auf einige Fragen, die für IT-Administratoren in Bezug auf die Synchronisierung von Einstellungen und App-Daten ggf. interessant sind."
	services="active-directory"
    keywords="Enterprise State Roaming-Einstellungen, Windows-Cloud, Häufig gestellte Fragen zu Enterprise State Roaming"
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
	ms.date="07/21/2016"
	ms.author="femila"/>

# Roaming von Einstellungen und Daten – Häufig gestellte Fragen

Dieses Thema enthält Antworten auf einige Fragen, die für IT-Administratoren in Bezug auf die Synchronisierung von Einstellungen und App-Daten unter Umständen interessant sind.

## Für welche Daten wird das Roaming durchgeführt?
**Windows-Einstellungen:**: Die PC-Einstellungen, die in das Windows-Betriebssystem integriert sind. Im Allgemeinen sind dies Einstellungen, mit denen der PC eines Benutzers personalisiert wird. Sie lassen sich in folgende grobe Kategorien unterteilen:

- **Design:** Desktopdesign, Taskleisteneinstellungen usw.
- **Internet Explorer-Einstellungen:** Zuletzt geöffnete Registerkarten, Favoriten usw.
- **Einstellungen des Edge-Browsers**: Favoriten, Leseliste
- **Kennwörter:** Internet-Kennwörter, WLAN-Profile usw.
- **Spracheinstellungen:** Tastaturlayouts, Systemsprache, Datum und Uhrzeit usw.
- **Erleichterte Bedienung:** Design mit hohem Kontrast, Sprachausgabe, Bildschirmlupe usw.
- **Andere Windows-Einstellungen:** Einstellungen für Eingabeaufforderung, Anwendungsliste usw.

**Anwendungsdaten:** Universelle Windows-Apps können Einstellungsdaten in einen Ordner „Roaming“ schreiben, und alle Daten, die in diesen Ordner geschrieben werden, werden automatisch synchronisiert. App-Entwickler können eine App entwerfen, um diese Funktion zu nutzen. Weitere Informationen zur Entwicklung einer universellen Windows-App, für die das Roaming verwendet wird, finden Sie im Artikel zur [APP-Daten-API (Speichern und Abrufen von Einstellungen und anderen App-Daten)](https://msdn.microsoft.com/library/windows/apps/mt299098.aspx) und im [Entwicklerblog zum Roaming von Windows 8-App-Daten](http://blogs.msdn.com/b/windowsappdev/archive/2012/07/17/roaming-your-app-data.aspx).

## Welches Konto wird für die Einstellungssynchronisierung verwendet?
In Windows 8 und Windows 8.1 wurden für die Einstellungssynchronisierung immer Microsoft-Benutzerkonten verwendet. Unternehmensbenutzer hatten die Möglichkeit, ein Microsoft-Konto mit ihrem Active Directory-Domänenkonto zu verbinden, um Zugriff auf die Einstellungssynchronisierung zu erhalten. Unter Windows 10 wurde diese Funktion des „verbundenen Microsoft-Kontos“ durch das Modell „Framework mit primärem/sekundärem Konto“ ersetzt.

Das primäre Konto ist als das Konto definiert, das zum Anmelden an Windows verwendet wird. Dies kann ein Microsoft-Konto sein, ein Azure Active Directory-Konto (Azure AD), ein lokales Active Directory-Konto oder ein lokales Konto. Zusätzlich zum primären Konto können Windows 10-Benutzer ihrem Gerät ein oder mehrere sekundäre Cloudkonten hinzufügen. Bei diesen sekundären Konten handelt es sich im Allgemeinen um ein Microsoft-Konto, Azure Active Directory-Konto oder ein anderes Konto, z. B. Gmail oder Facebook. Diese sekundären Konten ermöglichen den Zugriff auf weitere Dienste, z. B. einmaliges Anmelden und den Windows Store, aber sie sind nicht für die Einstellungssynchronisierung geeignet.

Unter Windows 10 kann nur das primäre Konto für das Gerät für die Einstellungssynchronisierung verwendet werden (siehe Ausnahme unter [Gewusst wie: Aktualisieren von Microsoft-Konto-Einstellungssynchronisierung unter Windows 8 auf Azure AD-Einstellungssynchronisierung unter Windows 10](active-directory-windows-enterprise-state-roaming-faqs.md#How-do-I-upgrade-from-Microsoft-account-settings-sync-in-Windows-8-to-Azure-AD-settings-sync-in Windows-10?)).

Daten werden in Bezug auf die verschiedenen Benutzerkonten niemals gemischt. Es gibt zwei Regeln für die Synchronisierung von Einstellungen:
- Für Windows-Einstellungen wird das Roaming immer mit dem primären Konto durchgeführt.
- App-Daten werden mit dem Konto verknüpft, das zum Beschaffen der App verwendet wird. Es werden nur Apps synchronisiert, die mit dem primären Konto verknüpft („getaggt“) sind. Die App-Besitzverknüpfung wird bestimmt, wenn eine App über den Windows Store oder per mobiler Geräteverwaltung (Mobile Device Management, MDM) quergeladen wird.

Falls der Besitzer einer App nicht identifiziert werden kann, wird das Roaming basierend auf dem primären Konto durchgeführt. Wenn ein Gerät von Windows 8 oder Windows 8.1 auf Windows 10 aktualisiert wird, werden alle Apps so gekennzeichnet, dass sie über das Microsoft-Konto beschafft wurden. Normalerweise werden die meisten Apps über den Windows Store beschafft, und vor Windows 10 gab es keine Windows Store-Unterstützung für Azure AD-Konten. Wenn eine App per Offlinelizenz installiert wird, wird die App mit dem primären Konto auf dem Gerät verknüpft.

>[AZURE.NOTE]  
Microsoft-Konten von Windows 10-Geräten, die sich im Besitz von Unternehmen befinden und mit Azure AD verbunden sind, können nicht mehr mit einem Domänenkonto verbunden werden. Die Möglichkeit zum Verbinden eines Microsoft-Kontos mit einem Domänenkonto und Synchronisieren aller Daten des Benutzers mit dem Microsoft-Konto (also das Microsoft-Konto-Roaming mit der Funktion „Verbindung von Microsoft-Konto und Active Directory”) wird für Windows 10-Geräte entfernt, für die der Beitritt zu einer verbundenen Active Directory/Azure AD-Umgebung durchgeführt wird.

## Gewusst wie: Aktualisieren von Microsoft-Konto-Einstellungssynchronisierung unter Windows 8 auf Azure AD-Einstellungssynchronisierung unter Windows 10
Für einen Benutzer, der der Active Directory-Domäne beigetreten ist und Windows 8 oder Windows 8.1 mit einem verbundenen Microsoft-Konto ausführt, werden Einstellungen über das Microsoft-Konto synchronisiert. Nach der Aktualisierung auf Windows 10 werden für Benutzer, die der Domäne beigetreten sind, die Benutzereinstellungen weiterhin über das Microsoft-Konto synchronisiert, solange für die Active Directory-Domäne keine Verbindung mit Azure AD hergestellt wird. Falls für die lokale Active Directory-Domäne doch eine Verbindung mit Azure AD hergestellt wird, beginnt das Gerät des Benutzers mit dem Versuch, die Einstellungen über das verbundene Azure AD-Konto zu synchronisieren. Wenn Enterprise State Roaming vom Azure AD-Administrator nicht aktiviert wird, wird das Synchronisieren von Einstellungen für Benutzer mit einem verbundenen Azure AD-Konto beendet. Nachdem die Synchronisierung von Einstellungen über Azure AD aktiviert wurde, wird für Windows 10-Benutzer sofort mit dem Synchronisieren von Windows-Einstellungen per Azure AD begonnen, wenn sich die Benutzer mit einer Azure AD-Identität anmelden.

Benutzer, die persönliche Daten auf ihren Unternehmensgeräten gespeichert haben, sollten sich darüber im Klaren sein, dass für die Daten des Windows-Betriebssystems und der -Anwendungen die Synchronisierung mit Azure AD durchgeführt wird. Dies hat folgende Auswirkungen:

- Für Benutzer mit persönlichen Microsoft-Konten weichen die Einstellungen nach und nach von den Einstellungen der Azure AD-Unternehmenskonten ab, da für die Synchronisierung der Microsoft-Konto- und Azure AD-Einstellungen jetzt separate Konten verwendet werden.
- Persönliche Daten wie WLAN-Kennwörter, Webanmeldeinformationen und Internet Explorer-Favoriten und -Registerkarten, die bislang über das verbundene Microsoft-Konto synchronisiert wurden, werden nun per Azure AD synchronisiert.


## Wie funktioniert die Enterprise State Roaming-Interoperabilität zwischen Microsoft- und Azure AD-Konten?
In den Windows 10-Versionen ab November 2015 wird Enterprise State Roaming nur für jeweils ein Konto unterstützt. Wenn sich Benutzer bei Windows mit einem Geschäfts- oder Schulkonto anmelden, werden alle Daten über Azure AD synchronisiert. Wenn sich Benutzer an Windows mit einem persönlichen Microsoft-Konto anmelden, werden alle Daten über das Microsoft-Konto synchronisiert. Für universelle App-Daten wird das Roaming nur mit dem primären Anmeldekonto auf dem Gerät durchgeführt – und auch nur dann, wenn sich die Lizenz der App im Besitz des primären Kontos befindet. Universelle App-Daten für Apps, die sich im Besitz von sekundären Konten befinden, werden nicht synchronisiert.

## Können Einstellungen für Azure AD-Konten mehrerer Mandanten synchronisiert werden?
Wenn sich mehrere Azure AD-Konten von unterschiedlichen Azure AD-Mandanten auf demselben Gerät befinden, müssen Sie die Registrierung des Geräts aktualisieren, um mit dem Dienst „Azure Rights Management Service (RMS)“ für jeden Azure AD-Mandanten zu kommunizieren.

1. Zuerst benötigen Sie die GUID für jeden Azure AD-Mandanten. Öffnen Sie das klassische Azure-Portal, und wählen Sie einen Azure AD-Mandanten aus. Die GUID für den Mandanten ist in der URL in der Adressleiste des Browsers enthalten: `https://manage.windowsazure.com/YourAccount.onmicrosoft.com#Workspaces/ActiveDirectoryExtension/Directory/Tenant GUID/directoryQuickStart`
2. Fügen Sie nach Ermittlung der GUID den folgenden Registrierungsschlüssel hinzu: **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\SettingSync\\WinMSIPC<Mandanten-ID-GUID>**. Erstellen Sie unter dem Schlüssel <**Mandanten-ID-GUID**> einen neuen mehrteiligen Zeichenfolgenwert (REG-MULTI-SZ) mit dem Namen **AllowedRMSServerUrls**, und geben Sie als Daten die Lizenzierungsverteilungspunkt-URLs der anderen Azure-Mandanten an, auf die das Gerät zugreift.
3. Die Lizenzierungsverteilungspunkt-URLs können durch Ausführen des **Get-AadrmConfiguration**-Cmdlets ermittelt werden. Falls sich die Werte für **LicensingIntranetDistributionPointUrl** und **LicenseingExtranetDistributionPointUrl** unterscheiden, müssen Sie beide Werte angeben. Wenn die Werte gleich sind, müssen Sie den Wert nur einmal angeben.


## Welche Optionen stehen für das Roaming von Einstellungen für bereits vorhandene Windows-Desktopanwendungen zur Verfügung?
Roaming kann nur für universelle Windows-Apps verwendet werden. Das Roaming für eine bereits vorhandene Windows-Desktopanwendung kann auf zwei Arten aktiviert werden:

- Mithilfe der [Desktop-Brücke](http://aka.ms/desktopbridge) lassen sich bereits vorhandene Windows-Desktop-Apps problemlos zur universellen Windows-Plattform migrieren. Anschließend sind nur minimale Codeänderungen erforderlich, um das Roaming von Azure AD-App-Daten zu nutzen. Die Desktop-Brücke versieht Ihre Apps mit einer App-Identität. Diese wird benötigt, um das App-Datenroaming für bereits vorhandene Desktop-Apps zu ermöglichen.
- Mithilfe von [User Experience Virtualization (UE-V)](https://technet.microsoft.com/library/dn458947.aspx) können Sie eine benutzerdefinierte Einstellungsvorlage für bereits vorhandene Windows-Desktop-Apps erstellen und das Roaming lediglich für Win32-Apps ermöglichen. Bei dieser Option sind keinerlei Änderungen am App-Code erforderlich. UE-V ist auf lokales Active Directory-Roaming für Kunden beschränkt, die das Microsoft Desktop Optimization Package erworben haben.

Der Administrator kann UE-V so konfigurieren, dass das Roaming nur für Daten von Windows-Desktop-Apps stattfindet. Hierzu kann er mithilfe von [UE-V-Gruppenrichtlinien](https://technet.microsoft.com/itpro/mdop/uev-v2/configuring-ue-v-2x-with-group-policy-objects-both-uevv2) das Roaming von Windows-Betriebssystemeinstellungen und universellen App-Daten deaktivieren.

- Deaktivieren Sie die Gruppenrichtlinie „Roam Windows settings“ (Roaming von Windows-Einstellungen).
- Aktivieren Sie die Gruppenrichtlinie „Do not synchronize Windows Apps“ (Windows-Apps nicht synchronisieren).
- Deaktivieren Sie im Anwendungsabschnitt das Roaming für IE.

Es ist möglich, dass Microsoft in Zukunft nach Wegen suchen wird, wie UE-V fest in Windows integriert und erweitert werden kann, um das Roaming von Einstellungen über die Azure AD-Cloud zu ermöglichen.


## Können synchronisierte Einstellungen und Daten lokal gespeichert werden?
Beim Enterprise State Roaming werden alle synchronisierten Daten in der Azure-Cloud gespeichert, während UE-V eine lokale Roaminglösung für Unternehmen bietet, die eine lokale Roaminglösung benötigen.

## Wem gehören die Daten, für die das Roaming durchgeführt wird?
Daten, für die Enterprise State Roaming verwendet wird, gehören den Unternehmen. Die Daten werden in einem Azure-Datencenter gespeichert. Alle Benutzerdaten sind sowohl bei der Übertragung als auch im Ruhezustand in der Cloud per Azure Rights Management (Azure RMS) verschlüsselt. Dies ist eine Verbesserung im Vergleich zur Synchronisierung von Einstellungen, die auf Microsoft-Konten basieren, da dabei nur bestimmte vertrauliche Daten, z. B. Anmeldeinformationen von Benutzern, verschlüsselt werden, bevor sie das Gerät verlassen.

Microsoft liegt der Schutz von Kundendaten am Herzen. Die Einstellungsdaten eines Unternehmensbenutzers werden immer automatisch per Azure RMS verschlüsselt, bevor sie ein Windows 10-Gerät verlassen, damit andere Benutzer diese Daten nicht lesen können. Wenn Ihr Unternehmen über ein kostenpflichtiges Abonnement für Azure RMS verfügt, können Sie weitere Azure RMS-Features nutzen, z. B. Nachverfolgen und Widerrufen von Dokumenten, automatisches Schützen von E-Mails mit vertraulichen Informationen und Verwalten Ihrer eigenen Schlüssel (Lösung „Bring Your Own Key“ (BYOK)). Unter [Was ist Azure Rights Management?](https://technet.microsoft.com/jj585026.aspx) finden Sie weitere Informationen zu diesen Features und zur Funktionsweise von Azure RMS.

## Kann ich die Synchronisierung für eine bestimmte Anwendung oder Einstellung verwalten?
Unter Windows 10 gibt es keine MDM- oder Gruppenrichtlinien-Einstellung, mit der das Roaming für eine einzelne Anwendung deaktiviert werden kann. Mandantenadministratoren können die Synchronisierung von App-Daten für alle Apps auf einem verwalteten Gerät deaktivieren, aber es ist keine präzisere Steuerung auf App-Ebene oder in den Apps vorhanden.

## Was können einzelne Benutzer tun, um das Roaming zu aktivieren oder zu deaktivieren?
Navigieren Sie in der App **Einstellungen** zu **Konten** > **Einstellungen synchronisieren**. Auf dieser Seite sehen Sie, welches Konto für das Roaming von Einstellungen verwendet wird, und Sie können einzelne Gruppen mit Einstellungen für das Roaming aktivieren oder deaktivieren.

##Was wird von Microsoft in Bezug auf die Aktivierung des Roamings in Windows 10 derzeit empfohlen?
Microsoft bietet verschiedene Lösungen für das Roaming von Einstellungen. Hierzu zählen etwa Roamingbenutzerprofile, UE-V und Enterprise State Roaming. In zukünftigen Versionen von Windows setzt Microsoft verstärkt auf Enterprise State Roaming. Wenn Ihr Unternehmen noch nicht für das Verschieben von Daten in die Cloud bereit ist oder noch Bedenken bestehen, empfiehlt Microsoft die Verwendung von UE-V als primäre Roamingtechnologie. Falls Ihr Unternehmen Roamingunterstützung für bereits vorhandene Windows-Desktopanwendungen benötigt, aber an einer schnellen Nutzung der Cloud interessiert ist, empfiehlt Microsoft die parallele Verwendung von Enterprise State Roaming und UE-V. UE-V und Enterprise State Roaming sind zwar sehr ähnliche Technologien, schließen sich aber nicht gegenseitig aus, sondern ergänzen sich vielmehr, um sicherzustellen, dass Ihr Unternehmen die Roamingdienste bereitstellen kann, die Ihre Benutzer benötigen.

Bei der parallelen Nutzung der Synchronisierung von Unternehmenseinstellungen von Enterprise State Roaming und UE-V gelten folgende Regeln:

- Enterprise State Roaming ist der primäre Roaming-Agent auf dem Gerät. UE-V wird zum Schließen der „Win32-Lücke“ verwendet.
- Das UE-V-Roaming für Windows-Einstellungen und moderne UWP-App-Daten muss mithilfe der UE-V-Gruppenrichtlinien deaktiviert werden, da diese Aufgaben bereits durch das Enterprise State Roaming abgedeckt werden.

##Wie unterstützt Enterprise State Roaming die virtuelle Desktopinfrastruktur (Virtual Desktop Infrastructure, VDI)?
Enterprise State Roaming wird nur für Windows 10-Client-SKUs und nicht für Server-SKUs unterstützt. Wenn ein virtueller Clientcomputer auf einem Hypervisorcomputer gehostet wird und ein Endbenutzer remote auf den virtuellen Computer zugreift, wird das Roaming für die Benutzerdaten durchgeführt. Wenn mehrere Benutzer das gleiche Betriebssystem verwenden und remote auf einen Server zugreifen, um uneingeschränkte Desktopfeatures zu nutzen, ist nicht sicher, ob das Roaming stattfindet. Das letztere sitzungsbasierte Szenario wird offiziell nicht unterstützt.


## Was passiert, wenn mein Unternehmen Azure RMS erwirbt, nachdem es eine Zeit lang das Roaming genutzt hat?
Falls Ihr Unternehmen das Roaming unter Windows 10 bereits über das eingeschränkte, kostenlose Azure RMS-Abonnement nutzt, hat der Erwerb eines kostenpflichtigen Azure RMS-Abonnements keinerlei Auswirkungen auf die Funktionen des Roamingfeatures, und Ihr IT-Administrator muss keine Änderungen an der Konfiguration vornehmen.

## Bekannte Probleme

- Das Anmelden per Smartcard oder virtueller Smartcard an Windows führt dazu, dass die Synchronisierung von Einstellungen nicht mehr funktioniert. Wenn Sie versuchen, sich mit einer Smartcard oder einer virtuellen Smartcard an Ihrem Gerät anzumelden, funktioniert die Synchronisierung nicht mehr. Dieses Problem wird im Rahmen von zukünftigen Updates für Windows 10 voraussichtlich behoben.
- Internet Explorer-Favoriten werden bei älteren Versionen des Windows 10-Builds nicht synchronisiert. Sie benötigen das kumulative Juli-Update für Windows 10 (Build 10586.494 oder höher), damit die Synchronisierung von Internet Explorer-Favoriten funktioniert.
- In bestimmten Fällen werden Daten von Enterprise State Roaming unter Umständen nicht synchronisiert, wenn die Multi-Factor Authentication (MFA) konfiguriert ist.
    - Wenn für den Benutzer die [Azure MFA](multi-factor-authentication.md) für das Azure AD-Portal konfiguriert ist, ist die Synchronisierung von Einstellungen unter Umständen nicht erfolgreich, während er sich mit einem Kennwort bei einem Windows-10-Gerät anmeldet. Diese Art der MFA-Konfiguration dient dem Schutz eines Azure-Administratorkontos. Administratoren können die Synchronisierung ggf. trotzdem durchführen, indem sie sich bei ihrem Windows 10-Gerät mithilfe der PIN vom Typ [Microsoft Passport for Work](active-directory-azureadjoin-passport.md) anmelden oder alternativ die Multi-Factor Authentication durchführen, während sie auf andere Azure-Dienste wie Office 365 zugreifen.
    - Die Synchronisierung ist unter Umständen nicht erfolgreich, wenn der Administrator die AD FS MFA-Richtlinie für bedingten Zugriff konfiguriert und das Zugriffstoken auf dem Gerät abläuft. Melden Sie sich ab und mit der PIN vom Typ [Microsoft Passport for Work](active-directory-azureadjoin-passport.md) an, oder führen Sie die Multi-Factor Authentication durch, während sie auf andere Azure-Dienste wie Office 365 zugreifen.
   
- Wenn ein Computer mit automatischer Registrierung bei Azure AD-Geräten in die Domäne eingebunden ist, treten unter Umständen Synchronisierungsfehler auf, wenn sich der Computer längere Zeit an einem externen Standort befindet, und die Domänenauthentifizierung kann nicht ausgeführt werden. Stellen Sie zum Beheben dieses Problems eine Verbindung zwischen dem Computer und einem Unternehmensnetzwerk her, sodass die Synchronisierung fortgesetzt werden kann.


## Verwandte Themen
- [Übersicht über Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
- [Aktivieren von Enterprise State Roaming in Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md)
- [Gruppenrichtlinien- und MDM-Einstellungen für Einstellungssynchronisierung](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [Windows 10-Roamingeinstellungen – Referenz](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)

<!---HONumber=AcomDC_0803_2016-->