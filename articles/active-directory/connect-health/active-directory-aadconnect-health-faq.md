---
title: "Häufig gestellte Fragen zu Azure Active Directory Connect Health – Azure | Microsoft-Dokumentation"
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
ms.date: 02/12/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 7c320a043322fefea1f58301492d4c5a0567320c
ms.openlocfilehash: aff2ced283de8282860d0aa356b12478f647d09c

---
# <a name="azure-ad-connect-health-frequently-asked-questions-faq"></a>Häufig gestellte Fragen zu Azure AD Connect Health
Dieses FAQ-Dokument beantwortet Fragen zu Azure AD Connect Health. Dazu gehören Fragen zur Verwendung des Diensts, einschließlich Abrechnungsmodell, Funktionen, Einschränkungen und Unterstützung.

## <a name="general-questions"></a>Allgemeine Fragen
**F: Ich verwalte mehrere Azure AD-Verzeichnisse. Wie wechsle ich im Azure-Portal zwischen verschiedenen Verzeichnissen?**

Sie können zwischen verschiedenen Azure AD-Mandanten wechseln, indem Sie in der rechten oberen Ecke den derzeit angemeldeten Benutzernamen sowie das entsprechende Konto auswählen. Wenn das Konto hier nicht aufgeführt ist, wählen Sie „Abmelden“, und verwenden Sie dann die entsprechenden Anmeldeinformationen des Verzeichnisses zum Anmelden.

**F: Welche Version der Identitätsrollen wird von Azure AD Connect Health unterstützt?**

In der folgenden Tabelle werden die Rollen und unterstützten Betriebssystemversionen aufgelistet.

|Rolle| Betriebssystem/Version|
|--|--|
|AD FS| <ul> <li> Windows Server 2008R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|
|Azure AD Connect | Version 1.0.9125 oder höher|
|AD DS| <ul> <li> Windows Server 2008R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|

Beachten Sie, dass die vom Dienst bereitgestellten Features sich möglicherweise basierend auf Rolle und Betriebssystem unterscheiden. Anders gesagt: Möglicherweise sind nicht alle Features für alle Betriebssystemversionen verfügbar. Näheres finden Sie in den Featurebeschreibungen für Details.

**F: Wie viele Lizenzen benötige ich, um meine Infrastruktur zu überwachen?**

* Der erste Connect Health-Agent benötigt mindestens eine Azure AD Premium-Lizenz.
* Jeder weitere registrierte Agent benötigt 25 weitere AADP-Lizenzen. 
* Die Anzahl der Agents entspricht der Gesamtanzahl der für alle überwachten Rollen registrierten Agents (AD FS, Azure AD Connect und/oder AD DS).

Lizenzierungsinformationen finden Sie auch auf der [Seite „Azure Active Directory Preise“](https://aka.ms/aadpricing).

Beispiel:

| Registrierte Agents | Benötigte Lizenzen | Beispielüberwachungskonfiguration |
| ------ | --------------- | --- |
| 1 | 1 | 1 AAD Connect-Server |
| 2 | 26| 1 AAD Connect-Server und 1 Domänencontroller |
| 3 | 51 | 1 AD FS-Server, 1 AD FS-Proxy und 1 Domänencontroller |
| 4 | 76 | 1 AD FS-Server, 1 AD FS-Proxy und 2 Domänencontroller |
| 5 | 101 | 1 AAD Connect-Server, 1 AD FS-Server, 1 AD FS-Proxy und 2 Domänencontroller |
## <a name="installation-questions"></a>Fragen zur Installation
**F: Wie wirkt sich die Installation des Azure AD Connect Health-Agents auf die einzelnen Server aus?**

Die Installation des Microsoft Azure AD Connect Health-Agents auf AD FS-Servern, Webanwendungs-Proxyservern, Azure AD Connect-(Synchronisierungs-)Servern oder Domänencontrollern wirkt sich nur minimal auf CPU, Arbeitsspeichernutzung, Netzwerkbandbreite und Speicher aus.

Die folgenden Zahlen stellen Näherungswerte dar.

    * CPU-Auslastung: ~1-5% Zunahme
    * Arbeitsspeichernutzung: bis zu 10 % des insgesamt verfügbaren Systemarbeitsspeichers

> [!NOTE]
> Wenn der Integritäts-Agent die Konnektivität mit dem Connect Health-Dienst verliert, werden die Daten lokal bis zu einer definierten Höchstgrenze gespeichert. Dabei werden die zwischengespeicherten Daten zuerst überschrieben, deren Verwendung am längsten zurückliegt.
>
>

* Lokaler Pufferspeicher für Azure AD Connect Health Agents: ~20 MB
* Für AD FS-Server wird empfohlen, 1.024 MB (1 GB) Festplattenspeicher für den AD FS-Überwachungskanal für Azure AD Connect Health-Agents bereitzustellen, um sämtliche Überwachungsdaten zu verarbeiten, bevor sie überschrieben werden.

**F: Muss ich meine Server während der Installation der Azure AD Connect Health-Agents neu starten?**

Nein. Die Installation der Agents erfordert keinen Serverneustart.

Während der Installation einiger vorbereitender Schritte muss der Server möglicherweise neu gestartet werden. Beispielsweise erfordert die Installation des .NET 4.5-Frameworks unter Windows Server 2008 R2 einen Neustart des Servers.

**F: Nutzt der Azure AD Connect Health-Dienst einen Passthrough-Proxy über HTTP?**

Ja.  Für laufende Vorgänge können Sie den Health-Agent zum Weiterleiten ausgehender HTTP-Anforderungen mithilfe eines HTTP-Proxys konfigurieren. Erfahren Sie mehr über das [Konfigurieren des HTTP-Proxys für Integritäts-Agents](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy).

Wenn Sie während der Agent-Registrierung einen Proxy konfigurieren müssen, müssen Sie möglicherweise zuerst die Internet Explorer-Proxyeinstellungen ändern.

1. Öffnen Sie "Internet Explorer -> Einstellungen -> Internetoptionen -> Verbindungen -> LAN-Einstellungen".
2. Wählen Sie „Proxyserver für LAN verwenden“ aus.
3. Wählen Sie „Erweitert“ aus, falls Sie über unterschiedliche Proxyports für HTTP und HTTPS/Secure verfügen.

**F: Unterstützt der Azure AD Connect Health-Dienst die Standardauthentifizierung bei der Verbindung mit HTTP-Proxys?**

Nein. Ein Mechanismus zum Angeben eines frei wählbaren Benutzernamens und Kennworts für die Standardauthentifizierung wird derzeit nicht unterstützt.

**F: Welche Firewallports muss ich öffnen, damit der Azure AD Connect Health-Agent funktioniert?**

Im [Abschnitt „Anforderungen“](active-directory-aadconnect-health-agent-install.md#requirements) finden Sie die Liste der Firewallports und andere Anforderungen an die Konnektivität.


**F: Warum sehe ich zwei Server mit dem gleichen Namen im Azure AD Connect Health-Portal?**

Wenn Sie einen Agent von einem Server entfernen, wird der Server nicht automatisch aus dem Azure AD Connect-Portal entfernt.  Wenn Sie manuell einen Agent von einem Server oder den Server selbst entfernt haben, müssen Sie den Servereintrag manuell aus dem Azure AD Connect Health-Portal löschen.

Wenn Sie einen Server über ein Image neu erstellt oder einen neuen Server mit den gleichen Details (z.B. Computername) erstellt haben, ohne den bereits registrierten Server aus dem Azure AD Connect Health-Portal zu entfernen, und den Agent auf dem neuen Server installiert haben, werden möglicherweise zwei Einträge mit demselben Namen angezeigt.  
In diesem Fall sollten Sie den Eintrag für den älteren Server manuell löschen. Die Daten für diesen Server sollten veraltet sein.

## <a name="health-agent-registration-and-data-freshness"></a>Integritäts-Agent-Registrierung und Datenaktualität

**F: Was sind die häufigsten Gründe für Integritäts-Agent-Registrierungsfehler, und wie werden sie behoben?**

Bei der Registrierung des Integritäts-Agent kann aus folgenden Gründen ein Fehler auftreten:
    * Der Agent kann nicht mit den erforderlichen Endpunkten kommunizieren, weil die Firewall den Datenverkehr blockiert. Dies ist besonders häufig bei Webanwendungsproxy-Servern der Fall. Stellen Sie sicher, dass Sie die ausgehende Kommunikation mit den erforderlichen Endpunkten und Ports zugelassen haben. Ausführliche Informationen finden Sie im Abschnitt [Anforderungen](active-directory-aadconnect-health-agent-install.md#requirements).
    * Die ausgehende Kommunikation wird durch die Netzwerkebene einer SSL-Überprüfung unterzogen. Dies bewirkt, dass das vom Agent verwendete Zertifikat durch den Überprüfungsserver/die Überprüfungsentität ersetzt wird und nicht die erforderlichen Schritte zum Abschluss der Agent-Registrierung ausführt.
    * Der Benutzer verfügt nicht über die Zugriffsrechte, um die Registrierung des Agents auszuführen. Globale Administratoren haben standardmäßig Zugriff. Sie können den Zugriff mit der [Rollenbasierten Zugriffssteuerung](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) an andere Benutzer delegieren.

**F: Ich erhalte die Warnmeldung: „Die Daten des Integritätsdiensts sind nicht aktuell“. Wie lässt sich das Problem beheben?**

Diese Warnung wird vom Integritätsdienst generiert, wenn er nicht in den letzten 2 Stunden alle Datenpunkte vom Server erhalten hat. Es kann mehrere Gründe für die Auslösung dieser Warnung geben.
    * Der Agent kann nicht mit den erforderlichen Endpunkten kommunizieren, weil die Firewall den Datenverkehr blockiert. Dies ist besonders häufig bei Webanwendungsproxy-Servern der Fall. Stellen Sie sicher, dass Sie die ausgehende Kommunikation mit den erforderlichen Endpunkten und Ports zugelassen haben. Ausführliche Informationen finden Sie im Abschnitt [Anforderungen](active-directory-aadconnect-health-agent-install.md#requirements).
    * Die ausgehende Kommunikation wird durch die Netzwerkebene einer SSL-Überprüfung unterzogen. Dies bewirkt, dass das vom Agent verwendete Zertifikat durch den Überprüfungsserver/die Überprüfungsentität ersetzt wird und keine Daten in den Connect Health-Dienst hochlädt.
    * Sie können den im Agent integrierten Konnektivitätsbefehl verwenden. [Weitere Informationen](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)
    * Die Agents unterstützen auch ausgehende Konnektivität über einen nicht authentifizierten HTTP-Proxy. [Weitere Informationen](active-directory-aadconnect-health-agent-install.md##configure-azure-ad-connect-health-agents-to-use-http-proxy)


## <a name="operations-questions"></a>Fragen zum Betrieb
**F: Muss ich die Überwachung auf den Webanwendungsproxy-Servern aktivieren?**

Nein, die Überwachung muss auf den Webanwendungsproxy-Servern nicht aktiviert werden.

**F: Wie werden Azure AD Connect Health-Warnungen aufgelöst?**

Azure AD Connect Health-Warnungen werden basierend auf einer Erfolgsbedingung aufgelöst. Azure AD Connect Health-Agents erkennen und melden die Erfolgsbedingungen in regelmäßigen Abständen an den Dienst. Bei einigen Warnungen gilt eine zeitbasierte Unterdrückung. Dies bedeutet: Wenn die gleiche Fehlerbedingung nicht innerhalb von 72 Stunden nach Generieren der Warnung erneut festgestellt wird, wird die Warnung automatisch aufgelöst.


## <a name="migration-questions"></a>Fragen zur Migration

Dieser Abschnitt gilt nur für Kunden, die wegen einer anstehenden Migration ihrer Azure AD Connect Health-Dienstdaten benachrichtigt wurden.

**F: Muss ich nach der Migration meine Agents neu registrieren oder meine Benachrichtigungseinstellungen neu konfigurieren?**

Nein. Agent-Registrierungsinformationen und die Benachrichtigungseinstellungen werden im Rahmen der Migration ebenfalls verschoben.

**F: Wie lange dauert es nach der Migration, bis Daten im Portal angezeigt werden?**

Daten werden nach der Migration innerhalb einer Stunde im Portal angezeigt.

**F: Was geschieht mit meinen vorhanden aktiven Warnungen?**

Entsprechende Warnungen werden nach der Migration innerhalb einer Stunde reaktiviert.


## <a name="related-links"></a>Verwandte Links
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Installieren des Azure AD Connect Health-Agents](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-Vorgänge](active-directory-aadconnect-health-operations.md)
* [Verwenden von Azure AD Connect Health mit AD FS](active-directory-aadconnect-health-adfs.md)
* [Verwenden von Azure AD Connect Health für die Synchronisierung](active-directory-aadconnect-health-sync.md)
* [Verwenden von Azure AD Connect Health mit AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health: Versionsverlauf](active-directory-aadconnect-health-version-history.md)



<!--HONumber=Feb17_HO2-->


