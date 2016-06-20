<properties
	pageTitle="Windows 10-Roamingeinstellungen – Referenz | Microsoft Azure"
	description="Vollständige Liste mit allen Einstellungen, für die unter Windows 10 das Roaming oder eine Sicherung durchgeführt wird."
	services="active-directory"
    keywords="Enterprise State Roaming, Windows-Cloud"
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

# Windows 10-Roamingeinstellungen – Referenz

Hier ist eine vollständige Liste mit allen Einstellungen angegeben, für die unter Windows 10 das Roaming oder eine Sicherung durchgeführt wird.

##Geräte und Endpunkte
Die folgende Tabelle enthält eine Zusammenfassung der Geräte und Kontotypen, die unter Windows 10 vom Framework für die Synchronisierung, Sicherung und Wiederherstellung unterstützt werden.

| Kontotyp und Vorgang | Desktop | Mobile |
|----------------------------|---------|--------|
|Azure Active Directory: Synchronisierung| Ja | Nein |
|Azure Active Directory: Sicherung/Wiederherstellung| Nein | Nein |
|Microsoft-Konto: Synchronisierung|Ja | Ja |
|Microsoft-Konto: Sicherung/Wiederherstellung| Nein | Ja |


##Was ist die Sicherung?
Windows-Einstellungen werden normalerweise standardmäßig synchronisiert. Einige Einstellungen werden aber nur gesichert, z. B. die Liste mit den installierten Anwendungen auf einem Gerät. Sicherungen können nur für mobile Geräte angewendet werden und sind zurzeit für Enterprise State Roaming-Benutzer nicht verfügbar. Bei der Sicherung wird ein Microsoft-Konto verwendet, die Einstellungen und Anwendungsdaten werden in OneDrive gespeichert. Wenn ein Benutzer die Synchronisierung auf dem Gerät mit der Einstellungen-App deaktiviert, werden die Anwendungsdaten, die normalerweise synchronisiert werden, nur noch gesichert. Auf Sicherungsdaten kann nur über den Wiederherstellungsvorgang während der ersten Ausführung eines neuen Geräts zugegriffen werden. Sicherungen können über die Geräteeinstellungen deaktiviert und über das OneDrive-Konto des Benutzers verwaltet und gelöscht werden.

## Übersicht über die Windows-Einstellungen
Die folgenden Einstellungsgruppen sind für Endbenutzer verfügbar, um die Einstellungssynchronisierung auf Windows 10-Geräten zu aktivieren oder zu deaktivieren.

- Design: Desktophintergrund, Benutzerkachel, Taskleistenposition usw. 
- Internet Explorer-Einstellungen: Browserverlauf, eingegebene URLs, Favoriten usw. 
- Kennwörter: [Schließfach für Anmeldeinformationen](https://technet.microsoft.com/library/jj554668.aspx), einschließlich WLAN-Profile 
- Spracheinstellungen: Wörterbuch, Einstellungen für Systemsprache 
- Erleichterte Bedienung: Sprachausgabe, Bildschirmtastatur, Bildschirmlupe 
- Weitere Windows-Einstellungen: siehe Details zu Windows-Einstellungen

![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-individual-sync-settings.png)

Die Synchronisierung der Einstellungsgruppe des Edge-Browsers (Favoriten, Leseliste) kann von Endbenutzern über das Einstellungsmenü des Edge-Browsers aktiviert oder deaktiviert werden.

![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-sync-content.png)
 
## Details zu Windows-Einstellungen
In der folgenden Tabelle bezieht sich „Sonstige“ in der Spalte „Gruppe“ auf Einstellungen, die unter „Einstellungen“ > „Konten“ > „Einstellungen synchronisieren“ > „Weitere Windows-Einstellungen“ deaktiviert werden können.

Die Einträge „Intern“ in der Spalte „Gruppe“ beziehen sich auf Einstellungen und Apps, für die die Synchronisierung nur in der App selbst oder für das gesamte Gerät per MDM- oder Gruppenrichtlinien-Einstellungen deaktiviert werden kann. Einstellungen, für die kein Roaming oder keine Synchronisierung durchgeführt wird, gehören keiner Gruppe an.


| Einstellungen | Desktop | Mobile | Gruppe |
|----------------------------------|---------|---------|-------|
| **Konten**: Kontobild | Sync. |X |Design |
| **Konten**: weitere Kontoeinstellungen |X |X | |
| **Erweitertes mobiles Breitband**: Netzwerkname der Internetverbindungsfreigabe (ermöglicht automatische Ermittlung von mobilen WLAN-Hotspots per Bluetooth)|X |X |Kennwörter |
|**App-Daten**: einzelne Apps können Daten synchronisieren|Sync.sicherung | Sync.sicherung|Intern |
|**App-Liste**: Liste der installierten Apps |X |Sicherung |Sonstige |
|**Bluetooth**: alle Bluetooth-Einstellungen |X |X | |
|**Eingabeaufforderung**: alle Einstellungen für die Eingabeaufforderung |Sync. |X | |
|**Cortana**: Ein/Aus |X |X | |
|**Cortana**: Cortana auf dem Sperrbildschirm ermöglichen |X |X | |
|**Cortana**: Benutzername |Sync. |Sync. |Intern|
|**Cortana**: SMS vorlesen |X |Sync. |Intern|
|**Cortana**: SafeSearch |X |Sync. |Intern|
|**Cortana**: Informationen zu Flügen usw. suchen|X |Sync. |Intern|
|**Anmeldeinformationen**: Schließfach für Anmeldeinformationen |Sync. |Sync. |Kennwort|
|**Datum, Uhrzeit und Region**: automatische Uhrzeit (Internetzeitsynchronisierung) |Sync. |Sync. |Sprache|
|**Datum, Uhrzeit und Region**: 24-Stunden-Format|sync |X |Sprache|
|**Datum, Uhrzeit und Region**: Datum und Uhrzeit|Sync. |X |Sprache|
|**Datum, Uhrzeit und Region**: Zeitzone | |X |Sprache|
|**Datum, Uhrzeit und Region**: Sommerzeit|Sync. |X |Sprache|
|**Datum, Uhrzeit und Region**: Land/Region |Sync. |X |Sprache|
|**Datum, Uhrzeit und Region**: erster Tag der Woche |Sync. |X |Sprache|
|**Datum, Uhrzeit und Region**: regionales Format (Gebietsschema) |Sync. |X |Sprache|
|**Datum, Uhrzeit und Region**: kurzes Datum |Sync. |X |Sprache|
|**Datum, Uhrzeit und Region**: langes Datum |Sync. |X |Sprache|
|**Datum, Uhrzeit und Region**: kurze Uhrzeit |Sync. |X |Sprache|
|**Datum, Uhrzeit und Region**: lange Uhrzeit |Sync. |X |Sprache|
|**Desktoppersonalisierung**: Desktopdesign (Hintergrund, Systemfarbe, standardmäßige Systemsounds, Bildschirmschoner) |Sync. |X |Design|
|**Desktoppersonalisierung**: Diashow-Hintergrundbild |Sync. |X |Design|
|**Desktoppersonalisierung**: Einstellungen der Taskleiste (Position, automatisches Ausblenden usw.) |Sync. |X |Design|
|**Desktoppersonalisierung**: Layout des Startbildschirms |X |Sichern ||
|**Geräte**: verbundene freigegebene Drucker |X | X |Sonstige |
|**Edge-Browser**: Leseliste |Sync. |Sync. |Intern|
|**Edge-Browser**: Favoriten |Sync. |Sync. |Intern|
|**Edge-Browser**: alle anderen Edge-Einstellungen|X |X ||
|**Hoher Kontrast**: Ein/Aus |Sync. |Sync. |Erleichterte Bedienung|
|**Hoher Kontrast**: Designeinstellungen|Sync. |X ||Erleichterte Bedienung|
|**Internet Explorer**: geöffnete Registerkarten (URL und Titel)|Sync. |Sync. |Internet Explorer|
|**Internet Explorer**: Leseliste|Sync. |Sync. |Internet Explorer|
|**Internet Explorer**: eingegebene URLs|Sync. |Sync. |Internet Explorer|
|**Internet Explorer**: Browserverlauf|Sync. |Sync. |Internet Explorer|
|**Internet Explorer**: Favoriten|Sync. |Sync. |Internet Explorer|
|**Internet Explorer**: ausgeschlossene URLs|Sync. |Sync. |Internet Explorer|
|**Internet Explorer**: Startseiten|Sync. |Sync. |Internet Explorer|
|**Internet Explorer**: Domänenvorschläge|Sync. |Sync. |Internet Explorer|
|**Tastatur**: Benutzer können Bildschirmtastatur ein-/ausschalten|Sync. |X |Erleichterte Bedienung|
|**Tastatur**: Einrastfunktion einschalten (standardmäßig deaktiviert)|Sync. |X |Erleichterte Bedienung|
|**Tastatur**: Anschlagverzögerung einschalten (standardmäßig deaktiviert)|Sync. |X |Erleichterte Bedienung|
|**Tastatur**: Umschalttasten einschalten (standardmäßig deaktiviert)|Sync. |X |Erleichterte Bedienung|
|**Internet Explorer**: Domänensprache: Chinesisch (CHS) QWERTY – Selbstlernfunktion aktivieren|Sync. |X |Sprache|
|**Sprache**: CHS QWERTY – dynamische Kandidateneinstufung aktivieren|sync |X |Sprache|
|**Sprache**: CHS QWERTY – Zeichensatz Chinesisch (vereinfacht)|Sync. |X |Sprache|
|**Sprache**: CHS QWERTY – Zeichensatz Chinesisch (traditionell)|Sync. |X |Sprache|
|**Sprache**: CHS QWERTY – Fuzzy Pinyin|Sync. |Sync. |Sprache|
|**Sprache**: CHS QWERTY – Fuzzy Pairs|Sync. |Sync. |Sprache|
|**Sprache**: CHS QWERTY – Pinyin (vollständig)|Sync. |X |Sprache|
|**Sprache**: CHS QWERTY – Doppel-Pinyin|Sync. |X |Sprache|
|**Sprache**: CHS QWERTY – Autokorrektur beim Lesen|Sync. |X |Sprache|
|**Sprache**: CHS QWERTY – C/E-Umschalttaste, UMSCHALT|Sync. |X |Sprache|
|**Sprache**: CHS QWERTY – C/E-Umschalttaste, STRG|Sync. |X |Sprache|
|**Sprache**: CHS WUBI – Eingabemodus für einzelne Zeichen |Sync. |X |Sprache|
|**Sprache**: CHS WUBI – verbleibende Codierung des Kandidaten anzeigen |sync |X |Sprache|
|**Sprache**: CHS WUBI – Signalton bei ungültigem 4-Code|Sync. |X |Sprache|
|**Sprache**: CHS Bopomofo – CJK-Erweiterung A einschließen|Sync. |X |Sprache|
|**Sprache**: Japanisches IME – Eingabevorhersage und benutzerdefinierte Wörter|Sync. |Sync. |Sprache|
|**Sprache**: Koreanisches (KOR) IME|X |X |Sprache|
|**Sprache**: Handschrifterkennung|X |X |Sprache|
|**Sprache**: Sprachprofil|Sync. |Sicherung |Sprache|
|**Sprache**: Rechtschreibprüfung – AutoKorrektur und Hervorhebung von Rechtschreibfehlern|Sync. |Sicherung |Sprache|
|**Sprache**: Liste der Tastaturen|Sync. |Sicherung |Sprache|
|**Sperrbildschirm**: alle Einstellungen des Sperrbildschirms|X |X ||
|**Bildschirmlupe**: Ein/Aus (Masterumschalter)|X |X |Erleichterte Bedienung|
|**Bildschirmlupe**: Inversionsfarbe ein/aus (standardmäßig deaktiviert)|Sync. |X |Erleichterte Bedienung|
|**Bildschirmlupe**: Nachverfolgung – dem Tastaturfokus folgen|Sync. |X |Erleichterte Bedienung|
|**Bildschirmlupe**: Nachverfolgung – dem Mauszeiger folgen|Sync. |X |Erleichterte Bedienung|
|**Bildschirmlupe**: beim Anmelden von Benutzern starten (standardmäßig deaktiviert)|Sync. |X |Erleichterte Bedienung|
|**Maus**: Größe des Mauszeigers ändern|Sync. |X |Sonstige|
|**Maus**: Farbe des Mauszeigers ändern|Sync. |X |Sonstige|
|**Maus**: alle anderen Einstellungen|X |X ||
|**Sprachausgabe**: Schnellstart|Sync. |X |Erleichterte Bedienung|
|**Sprachausgabe**: Benutzer können Tonhöhe der Sprachausgabe ändern|Sync. |X |Erleichterte Bedienung|
|**Sprachausgabe**: Benutzer können Lesehinweise der Sprachausgabe für häufige Elemente ein- oder ausschalten (standardmäßig aktiviert)|Sync. |X |Erleichterte Bedienung|
|**Sprachausgabe**: Benutzer können aktivieren oder deaktivieren, ob eingegebene Zeichen zu hören sein sollen (standardmäßig aktiviert)|Sync. |X |Erleichterte Bedienung|
|**Sprachausgabe**: Benutzer können aktivieren oder deaktivieren, ob eingegebene Wörter zu hören sein sollen (standardmäßig aktiviert)|Sync. |X |Erleichterte Bedienung|
|**Sprachausgabe**: Einfügecursor nach Sprachausgabe (standardmäßig aktiviert)|Sync. |X |Erleichterte Bedienung|
|**Sprachausgabe**: visuelle Hervorhebung des Sprachausgabecursors aktivieren (standardmäßig aktiviert)|Sync. |X |Erleichterte Bedienung|
|**Sprachausgabe**: Audiohinweise wiedergeben (standardmäßig aktiviert)|Sync. |X |Erleichterte Bedienung|
|**Sprachausgabe**: aktivieren Sie Tasten auf der Bildschirmtastatur beim Heben Ihres Fingers (standardmäßig deaktiviert)|Sync. |Sync. |Erleichterte Bedienung|
|**Erleichterte Bedienung**: legen Sie die Breite des blinkenden Cursors fest|Sync. |X |Erleichterte Bedienung|
|**Erleichterte Bedienung**: Hintergrundbilder entfernen (standardmäßig deaktiviert)|Sync. |X |Erleichterte Bedienung|
|**Netzschalter und Energiesparen**: alle Einstellungen|X |X ||
|**Personalisierung des Startbildschirms**: Systemfarbe|Sync. |Sync. |Design|
|**Eingabe**: Wörterbuch|Sync. |Sicherung |Sprache|
|**Eingabe**: falsch geschriebenes Wort automatisch korrigieren|Sync. |Sicherung |Sprache|
|**Eingabe**: Rechtschreibfehler hervorheben|Sync. |Sicherung |Sprache|
|**Eingabe**: Textvorschläge bei der Eingabe anzeigen|Sync. |Sicherung |Sprache|
|**Eingabe**: nach Auswahl eines Textvorschlags Leerzeichen einfügen|Sync. |Sicherung |Sprache|
|**Eingabe**: nach Doppeltippen auf die LEERTASTE Punkt einfügen|Sync. |Sicherung |Sprache|
|**Eingabe**: Großbuchstaben am Satzanfang|Sync. |Sicherung |Sprache|
|**Eingabe**: beim Doppeltippen auf die UMSCHALTTASTE Großbuchstaben verwenden|Sync. |Sicherung |Sprache|
|**Eingabe**: Tastentöne bei der Eingabe|Sync. |Sicherung |Sprache|
|**Eingabe**: Personalisierungsdaten für Bildschirmtastatur|Sync. |Sicherung |Sprache|
|**WLAN**: WLAN-Profile (nur WPA)|Sync. |Sync. |Kennwörter|


## Verwandte Themen
- [Übersicht über Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
- [Aktivieren von Enterprise State Roaming in Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md)
- [Roaming von Einstellungen und Daten – Häufig gestellte Fragen](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Gruppenrichtlinien- und MDM-Einstellungen für Einstellungssynchronisierung](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)






  

<!---HONumber=AcomDC_0608_2016-->