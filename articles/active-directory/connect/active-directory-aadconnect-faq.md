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
ms.date: 07/13/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: 27cc51d3f9220756fc1188f978dc158f17037bc3
ms.contentlocale: de-de
ms.lasthandoff: 03/30/2017

---
# Häufig gestellte Fragen zu Azure Active Directory Connect
<a id="frequently-asked-questions-for-azure-active-directory-connect" class="xliff"></a>

## Allgemeine Installation
<a id="general-installation" class="xliff"></a>
**F: Ist die Installation möglich, wenn der globale Azure AD-Administrator 2FA aktiviert hat?**  
Dies wird in den Builds ab Februar 2016 unterstützt.

**F: Gibt es eine Möglichkeit, Azure AD Connect unbeaufsichtigt zu installieren?**  
Azure AD Connect kann nur mit dem Installationsassistenten installiert werden. Eine unbeaufsichtigte und automatische Installation wird nicht unterstützt.

**F: In meiner Gesamtstruktur ist eine Domäne nicht erreichbar. Wie installiere ich Azure AD Connect?**  
Dies wird in den Builds ab Februar 2016 unterstützt.

**F: Funktioniert der AD DS Health-Agent im Serverkern?**  
Ja. Nach der Installation des Agents können Sie den Registrierungsprozess abschließen, indem Sie das folgende PowerShell-Cmdlet verwenden: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## Netzwerk
<a id="network" class="xliff"></a>
**F: Die maximale Länge für offene Verbindungen in meinem Netzwerk wird durch eine Firewall, ein Netzwerkgerät o. ä. eingeschränkt. Wie hoch sollte der clientseitige Schwellenwert für die Zeitüberschreitung bei der Verwendung von Azure AD Connect sein?**  
Für Netzwerksoftware, physische Geräte und alle anderen Komponenten, durch die die maximale Länge von offenen Verbindungen eingeschränkt wird, sollte ein Schwellenwert von mindestens 5 Minuten (300 Sekunden) für die Konnektivität zwischen dem Server, auf dem der Azure AD Connect-Client installiert ist, und Azure Active Directory verwendet werden. Dies gilt auch für alle zuvor veröffentlichten Microsoft Identity-Synchronisierungswerkzeuge.

**F: Werden SLDs (einteilige Domänen) unterstützt?**  
Nein. Azure AD Connect unterstützt keine lokalen Gesamtstrukturen/Domänen mit SLDs.

**F: Werden NetBios mit punktierten Namen unterstützt?**  
Nein. Azure AD Connect unterstützt keine lokalen Gesamtstrukturen/Domänen, deren NetBios-Name einen Punkt enthält.

## Verbund
<a id="federation" class="xliff"></a>
**F: Was muss ich tun, wenn ich eine E-Mail erhalte, in der ich aufgefordert werde, mein Office 365-Zertifikat zu erneuern?**  
Befolgen Sie die Anweisungen, die Sie im Thema [Erneuern von Zertifikaten](active-directory-aadconnect-o365-certs.md) finden, um Ihr Zertifikat zu erneuern.

**F: Für die vertrauende Seite für Office 365 habe ich „Vertrauende Seite automatisch aktualisieren“ festgelegt. Muss ich bestimmte Maßnahmen ergreifen, wenn mein Tokensignaturzertifikat automatisch verlängert wird?**  
Befolgen Sie die Anweisungen im Artikel [Erneuern von Zertifikaten](active-directory-aadconnect-o365-certs.md).

## Environment
<a id="environment" class="xliff"></a>
**F: Kann der Server nach der Installation von Azure AD umbenannt werden?**  
Nein. Wenn Sie den Namen des Servers ändern, kann das Synchronisierungsmodul keine Verbindung zur SQL-Datenbank herstellen, und der Dienst kann nicht gestartet werden.

## Identitätsdaten
<a id="identity-data" class="xliff"></a>
**F: Warum entspricht das UPN (UserPrincipalName)-Attribut in Azure AD dem lokalen UPN?**  
Informationen hierzu finden Sie in diesen Artikeln:

* [Benutzernamen in Office 365, Azure oder Intune entsprechen nicht dem lokalen UPN oder der alternativen Anmelde-ID.](https://support.microsoft.com/en-us/kb/2523192)
* [Änderungen werden nicht mehr vom Azure Active Directory-Synchronisierungstool synchronisiert, wenn der UPN eines Benutzerkontos für die Verbindung mit einer anderen Verbunddomäne geändert wird.](https://support.microsoft.com/en-us/kb/2669550)

Sie können Azure AD auch so konfigurieren, dass das Synchronisierungsmodul den userPrincipalName wie unter [Features des Azure AD Connect-Synchronisierungsdiensts](active-directory-aadconnectsyncservice-features.md)beschrieben aktualisiert.

**F: Ist es möglich, Soft-Match-Zuordnungen von lokalen AD-Gruppen-/Kontaktobjekten mit vorhandenen Azure AD-Gruppen-/Kontaktobjekten vorzunehmen?**  
Nein, dies wird derzeit nicht unterstützt.

**F: Ist es möglich, das ImmutableId-Attribut manuell für vorhandene Azure AD-Gruppen-/Kontaktobjekte festzulegen, um eine Hard-Match-Zuordnung zu lokalen AD-Gruppen-/Kontaktobjekten durchzuführen?**  
Nein, dies wird derzeit nicht unterstützt.



## Benutzerdefinierte Konfiguration
<a id="custom-configuration" class="xliff"></a>
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



## Problembehandlung
<a id="troubleshooting" class="xliff"></a>
**F: Wie erhalte ich Hilfe zu Azure AD Connect?**

[Durchsuchen der Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Suchen Sie in der Microsoft Knowledge Base (KB) nach technischen Lösungen für häufige Probleme im Zusammenhang mit der Unterstützung von Azure AD Connect.

[Microsoft Azure Active Directory-Foren](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Sie können in der Community nach technischen Fragen und Antworten suchen oder eine eigene Frage stellen, indem Sie [hier](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required)klicken.

[Azure AD Connect-Kundensupport](https://manage.windowsazure.com/?getsupport=true)

* Verwenden Sie diesen Link, um Support über das Azure-Portal zu erhalten.


