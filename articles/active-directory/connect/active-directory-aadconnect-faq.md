---
title: "Häufig gestellte Fragen zu Azure Active Directory Connect | Microsoft-Dokumentation"
description: "Auf dieser Seite finden Sie häufig gestellte Fragen zu Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: c22a8f4a895efc86abc328c6cf82685d7db8c19c
ms.openlocfilehash: 33de5839e1e8fa70f75636488a0769f7aebf8b95
ms.lasthandoff: 02/24/2017


---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a>Häufig gestellte Fragen zu Azure Active Directory Connect

## <a name="general-installation"></a>Allgemeine Installation
**F: Ist die Installation möglich, wenn der globale Azure AD-Administrator 2FA aktiviert hat?**  
Dies wird in den Builds ab Februar 2016 unterstützt.

**F: Gibt es eine Möglichkeit, Azure AD Connect unbeaufsichtigt zu installieren?**  
Azure AD Connect kann nur mit dem Installationsassistenten installiert werden. Eine unbeaufsichtigte und automatische Installation wird nicht unterstützt.

**F: In meiner Gesamtstruktur ist eine Domäne nicht erreichbar. Wie installiere ich Azure AD Connect?**  
Dies wird in den Builds ab Februar 2016 unterstützt.

**F: Funktioniert der AD DS Health-Agent im Serverkern?**  
Ja. Nach der Installation des Agents können Sie den Registrierungsprozess abschließen, indem Sie das folgende PowerShell-Cmdlet verwenden: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>Netzwerk
**F: Die maximale Länge für offene Verbindungen in meinem Netzwerk wird durch eine Firewall, ein Netzwerkgerät o. ä. eingeschränkt. Wie hoch sollte der clientseitige Schwellenwert für die Zeitüberschreitung bei der Verwendung von Azure AD Connect sein?**  
Für Netzwerksoftware, physische Geräte und alle anderen Komponenten, durch die die maximale Länge von offenen Verbindungen eingeschränkt wird, sollte ein Schwellenwert von mindestens 5 Minuten (300 Sekunden) für die Konnektivität zwischen dem Server, auf dem der Azure AD Connect-Client installiert ist, und Azure Active Directory verwendet werden. Dies gilt auch für alle zuvor veröffentlichten Microsoft Identity-Synchronisierungswerkzeuge.

**F: Werden SLDs (einteilige Domänen) unterstützt?**  
Nein. Azure AD Connect unterstützt keine lokalen Gesamtstrukturen/Domänen mit SLDs.

**F: Werden NetBios mit punktierten Namen unterstützt?**  
Nein. Azure AD Connect unterstützt keine lokalen Gesamtstrukturen/Domänen, deren NetBios-Name einen Punkt enthält.

## <a name="federation"></a>Verbund
**F: Was muss ich tun, wenn ich eine E-Mail erhalte, in der ich aufgefordert werde, mein Office 365-Zertifikat zu erneuern?**  
Befolgen Sie die Anweisungen, die Sie im Thema [Erneuern von Zertifikaten](active-directory-aadconnect-o365-certs.md) finden, um Ihr Zertifikat zu erneuern.

**F: Für die vertrauende Seite für Office&365; habe ich „Vertrauende Seite automatisch aktualisieren“ festgelegt. Muss ich bestimmte Maßnahmen ergreifen, wenn mein Tokensignaturzertifikat automatisch verlängert wird?**  
Befolgen Sie die Anweisungen im Artikel [Erneuern von Zertifikaten](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Environment
**F: Kann der Server nach der Installation von Azure AD umbenannt werden?**  
Nein. Wenn Sie den Namen des Servers ändern, kann das Synchronisierungsmodul keine Verbindung zur SQL-Datenbank herstellen, und der Dienst kann nicht gestartet werden.

## <a name="identity-data"></a>Identitätsdaten
**F: Warum entspricht das UPN (UserPrincipalName)-Attribut in Azure AD dem lokalen UPN?**  
Informationen hierzu finden Sie in diesen Artikeln:

* [Benutzernamen in Office 365, Azure oder Intune entsprechen nicht dem lokalen UPN oder der alternativen Anmelde-ID.](https://support.microsoft.com/en-us/kb/2523192)
* [Änderungen werden nicht mehr vom Azure Active Directory-Synchronisierungstool synchronisiert, wenn der UPN eines Benutzerkontos für die Verbindung mit einer anderen Verbunddomäne geändert wird.](https://support.microsoft.com/en-us/kb/2669550)

Sie können Azure AD auch so konfigurieren, dass das Synchronisierungsmodul den userPrincipalName wie unter [Features des Azure AD Connect-Synchronisierungsdiensts](active-directory-aadconnectsyncservice-features.md)beschrieben aktualisiert.

**F: Ist es möglich, Soft-Match-Zuordnungen von lokalen AD-Gruppen-/Kontaktobjekten mit vorhandenen Azure AD-Gruppen-/Kontaktobjekten vorzunehmen?**  
Nein, dies wird derzeit nicht unterstützt.

**F: Ist es möglich, das ImmutableId-Attribut manuell für vorhandene Azure AD-Gruppen-/Kontaktobjekte festzulegen, um eine Hard-Match-Zuordnung zu lokalen AD-Gruppen-/Kontaktobjekten durchzuführen?**  
Nein, dies wird derzeit nicht unterstützt.

## <a name="security"></a>Sicherheit
**F: Werden Konten nach einer bestimmten Anzahl von Versuchen, bei denen Fehler aufgetreten sind, gesperrt, oder wird eine ausgereiftere Strategie angewandt?**</br>
Wir verwenden eine ausgereiftere Strategie zum Sperren von Konten.  Sie basiert auf der IP-Adresse der Anforderung und den eingegebenen Kennwörtern. Die Dauer der Sperre wird ebenfalls gemäß der Wahrscheinlichkeit erhöht, dass es sich um einen Angriff handelt.  

**F: Bestimmte (häufige) Kennwörter werden mit „Dieses Kennwort wurde zu oft verwendet“-Meldungen zurückgewiesen – bezieht sich dies auf Kennwörter, die im aktuellen Active Directory verwendet wurden?**</br>
Dies bezieht sich auf Kennwörter, die global häufig sind, z.B. alle Varianten von „Password“ und „123456“.

**F: Wird eine Anmeldeanforderung aus fragwürdigen Quellen (Botnets, Tor-Endpunkt) in einem B2C-Mandanten blockiert, oder setzt dies einen Basic- oder Premium Edition-Mandanten voraus?**</br>
Wir haben ein Gateway, das Anforderungen filtert, gewissen Schutz gegen Botnets bietet und für alle B2C-Mandanten gilt. 

## <a name="custom-configuration"></a>Benutzerdefinierte Konfiguration
**F: Wo sind die PowerShell-Cmdlets für Azure AD Connect dokumentiert?**  
Mit Ausnahme der auf dieser Website dokumentierten Cmdlets werden keine PowerShell-Cmdlets in Azure AD Connect zur Verwendung für Kunden unterstützt.

**F: Kann ich mithilfe von „Serverexport/Serverimport“ in *Synchronization Service Manager* die Konfiguration zwischen Servern verschieben?**  
Nein. Da bei dieser Option nicht alle Einstellungen abgerufen werden, sollte sie nicht verwendet werden. Verwenden Sie stattdessen den Assistenten zum Erstellen der Basiskonfiguration auf dem zweiten Server und verwenden Sie den Synchronisierungsregel-Editor, um PowerShell-Skripts zum Verschieben benutzerdefinierter Regeln zwischen Servern zu erstellen. Siehe [Swing-Migration](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**F: Können Kennwörter für die Azure-Anmeldeseite zwischengespeichert werden, und kann dies verhindert werden, da sie ein Kennworteingabeelement mit dem Attribut „autocomplete = "false"“ enthält?**</br>
Wir unterstützen derzeit keine Änderung der HTML-Attribute des Kennworteingabefelds, einschließlich des AutoVervollständigen-Tags. Wir arbeiten derzeit an einem Feature, das benutzerdefiniertes Javascript zulässt, sodass Sie dem Kennwortfeld beliebige Attribute hinzufügen können. Dies sollte Ende 2017 verfügbar sein.

**F: Auf der Azure-Anmeldeseite werden Benutzernamen für Benutzer angezeigt, die sich zuvor erfolgreich angemeldet haben.  Kann dieses Verhalten deaktiviert werden?**</br>
Wir unterstützen derzeit keine Änderung der HTML-Attribute der Anmeldeseite. Wir arbeiten derzeit an einem Feature, das benutzerdefiniertes Javascript zulässt, sodass Sie dem Kennwortfeld beliebige Attribute hinzufügen können. Dies sollte Ende 2017 verfügbar sein.

**F: Gibt es eine Möglichkeit, gleichzeitige Sitzungen zu verhindern?**</br>
Nein.



## <a name="troubleshooting"></a>Problembehandlung
**F: Wie erhalte ich Hilfe zu Azure AD Connect?**

[Durchsuchen der Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Suchen Sie in der Microsoft Knowledge Base (KB) nach technischen Lösungen für häufige Probleme im Zusammenhang mit der Unterstützung von Azure AD Connect.

[Microsoft Azure Active Directory-Foren](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Sie können in der Community nach technischen Fragen und Antworten suchen oder eine eigene Frage stellen, indem Sie [hier](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required)klicken.

[Azure AD Connect-Kundensupport](https://manage.windowsazure.com/?getsupport=true)

* Verwenden Sie diesen Link, um Support über das Azure-Portal zu erhalten.


