---
title: "Azure AD Connect Health – FAQ"
description: "Dieses FAQ-Dokument beantwortet Fragen zu Azure AD Connect Health. Dazu gehören Fragen zur Verwendung des Diensts, einschließlich Abrechnungsmodell, Funktionen, Einschränkungen und Unterstützung."
services: active-directory
documentationcenter: 
author: billmath
manager: samueld
editor: curtand
ms.assetid: f1b851aa-54d7-4cb4-8f5c-60680e2ce866
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: vakarand
translationtype: Human Translation
ms.sourcegitcommit: 2f990f4c888da91422a6a6e9873307cbb483e782
ms.openlocfilehash: b6687bc0ccde0e257338a939f39e343f75a6f5df


---
# <a name="azure-ad-connect-health-frequently-asked-questions-faq"></a>Häufig gestellte Fragen zu Azure AD Connect Health
Dieses FAQ-Dokument beantwortet Fragen zu Azure AD Connect Health. Dazu gehören Fragen zur Verwendung des Diensts, einschließlich Abrechnungsmodell, Funktionen, Einschränkungen und Unterstützung.

## <a name="general-questions"></a>Allgemeine Fragen
**F: Ich verwalte mehrere Azure AD-Verzeichnisse. Wie kann ich zu dem Mandanten mit Azure Active Directory Premium wechseln?**

Sie können zwischen verschiedenen Azure AD-Mandanten wechseln, indem Sie in der rechten oberen Ecke den derzeit angemeldeten Benutzernamen sowie das entsprechende Konto auswählen. Wenn das Konto hier nicht aufgeführt ist, wählen Sie "Abmelden" aus, und melden Sie sich dann mit den globalen Administratoranmeldeinformationen des Verzeichnisses an, für das Azure Active Directory Premium aktiviert ist.

## <a name="installation-questions"></a>Fragen zur Installation
**F: Wie wirkt sich die Installation des Azure AD Connect Health-Agents auf die einzelnen Server aus?**

Die Installation des Microsoft Azure AD Connect Health-Agents auf AD FS-Servern, Webanwendungs-Proxyservern, Azure AD Connect-Servern (sycn) oder Domänencontrollern wirkt sich in Bezug auf CPU, Arbeitsspeichernutzung, Netzwerkbandbreite und Speicher nur minimal aus.

Die folgenden Zahlen stellen Näherungswerte dar.

* CPU-Auslastung: ~ 1 % Zunahme
* Arbeitsspeichernutzung: bis zu 10 % des insgesamt verfügbaren Systemarbeitsspeichers

> [!NOTE]
> Falls der Agent nicht mit Azure kommunizieren kann, speichert er die Daten bis zu einem definierten maximalen Grenzwert lokal. Dabei werden die zwischengespeicherten Daten zuerst überschrieben, deren Verwendung am längsten zurückliegt.
> 
> 

* Lokaler Pufferspeicher für Azure AD Connect Health Agents: ~20 MB
* Für AD FS-Server wird empfohlen, 1.024 MB (1 GB) Festplattenspeicher für den AD FS-Überwachungskanal für Azure AD Connect Health-Agents bereitzustellen, um sämtliche Überwachungsdaten zu verarbeiten, bevor sie überschrieben werden.

**F: Muss ich meine Server während der Installation der Azure AD Connect Health-Agents neu starten?**

Nein. Die Installation der Agents erfordert keinen Serverneustart. Während der Installation einiger vorbereitender Schritte muss der Server jedoch möglicherweise neu gestartet werden.

Beispielsweise erfordert die Installation des .NET 4.5-Frameworks unter Windows Server 2008 R2 einen Neustart des Servers.

**F: Nutzt der Azure AD Connect Health-Dienst einen Passthrough-Proxy über HTTP?**

Ja.  Für laufende Vorgänge können Sie den Health-Agent zum Weiterleiten ausgehender HTTP-Anforderungen mithilfe eines HTTP-Proxys konfigurieren.

Wenn Sie während der Agent-Registrierung einen Proxy konfigurieren müssen, müssen Sie möglicherweise zuerst die Internet Explorer-Proxyeinstellungen ändern.

1. Öffnen Sie "Internet Explorer -> Einstellungen -> Internetoptionen -> Verbindungen -> LAN-Einstellungen".
2. Wählen Sie „Proxyserver für LAN verwenden“ aus.
3. Wählen Sie „Erweitert“ aus, falls Sie über unterschiedliche Proxyports für HTTP und HTTPS/Secure verfügen.

**F: Unterstützt der Azure AD Connect Health-Dienst die Standardauthentifizierung bei der Verbindung mit HTTP-Proxys?**

Nein. Ein Mechanismus zum Angeben eines frei wählbaren Benutzernamens und Kennworts für die Standardauthentifizierung wird derzeit nicht unterstützt.

**F: Welche Version von AD DS wird von Azure AD Connect Health für AD DS unterstützt?**

Die Überwachung von AD DS wird unterstützt, wenn AD DS unter den folgenden Betriebssystemversionen installiert wird:

* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

## <a name="operations-questions"></a>Fragen zum Betrieb
**F: Muss ich die Überwachung auf meinen AD FS-Anwendungsproxyservern oder meinen Webanwendungsproxyservern aktivieren?**

Nein, die Überwachung muss auf den Webanwendungsproxy-Servern nicht aktiviert werden. Aktivieren Sie die Überwachung auf den AD FS-Servern.

**F: Wie werden Azure AD Connect Health-Warnungen aufgelöst?**

Azure AD Connect Health-Warnungen werden basierend auf einer Erfolgsbedingung aufgelöst. Azure AD Connect Health-Agents erkennen und melden die Erfolgsbedingungen in regelmäßigen Abständen an den Dienst. Bei einigen Warnungen gilt eine zeitbasierte Unterdrückung. Dies bedeutet: Wenn die gleiche Fehlerbedingung nicht innerhalb von 72 Stunden nach Generieren der Warnung erneut festgestellt wird, wird die Warnung automatisch aufgelöst.

**F: Welche Firewallports muss ich öffnen, damit der Azure AD Connect Health-Agent funktioniert?**

Sie müssen die TCP/UDP-Ports 443 und 5671 öffnen, damit der Azure AD Connect Health-Agent mit den Azure AD Connect Health-Dienstendpunkten kommunizieren kann.

**F: Warum sehe ich zwei Server mit dem gleichen Namen im Azure AD Connect Health-Portal?**

Wenn Sie einen Agent von einem Server entfernen, wird der Server nicht automatisch aus dem Azure AD Connect-Portal entfernt.  Wenn Sie manuell einen Agent von einem Server oder den Server selbst entfernt haben, müssen Sie den Servereintrag manuell aus dem Azure AD Connect Health-Portal löschen. 

Wenn Sie einen Server über ein Image neu erstellt oder einen neuen Server mit den gleichen Details (z.B. Computername) erstellt haben, ohne den bereits registrierten Server aus dem Azure AD Connect Health-Portal zu entfernen, und den Agent auf dem neuen Server installiert haben, werden möglicherweise zwei Einträge mit demselben Namen angezeigt.  
In diesem Fall sollten Sie den Eintrag für den älteren Server manuell löschen. Die Daten für diesen Server sollten veraltet sein.

## <a name="related-links"></a>Verwandte Links
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Installieren des Azure AD Connect Health-Agents](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-Vorgänge](active-directory-aadconnect-health-operations.md)
* [Verwenden von Azure AD Connect Health mit AD FS](active-directory-aadconnect-health-adfs.md)
* [Verwenden von Azure AD Connect Health für die Synchronisierung](active-directory-aadconnect-health-sync.md)
* [Verwenden von Azure AD Connect Health mit AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health: Versionsverlauf](active-directory-aadconnect-health-version-history.md)




<!--HONumber=Jan17_HO3-->


