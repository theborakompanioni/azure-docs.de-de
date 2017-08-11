---
title: "Azure AD Connect: Passthrough-Authentifizierung – Häufig gestellte Fragen | Microsoft-Dokumentation"
description: "In diesem Artikel erhalten Sie Antworten auf häufig gestellte Fragen zur Passthrough-Authentifizierung von Azure Active Directory."
services: active-directory
keywords: "Passthrough-Authentifizierung mit Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, einmaliges Anmelden"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: billmath
ms.translationtype: HT
ms.sourcegitcommit: 7bf5d568e59ead343ff2c976b310de79a998673b
ms.openlocfilehash: b8a08eb8fd036ad07ee6ce4cf624e8b5bc4c3ddc
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---

# <a name="azure-active-directory-pass-through-authentication-frequently-asked-questions"></a>Azure Active Directory Passthrough-Authentifizierung: Häufig gestellte Fragen

In diesem Artikel werden häufig gestellte Fragen zur Passthrough-Authentifizierung in Azure Active Directory (Azure AD) behandelt. Schauen Sie öfter vorbei, da wir regelmäßig neue Fragen hinzufügen.

## <a name="which-of-the-azure-ad-sign-in-methods---pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs---should-i-choose"></a>Welche der Azure AD-Anmeldemethoden (Passthrough-Authentifizierung, Kennworthashsynchronisierung oder Active Directory-Verbunddienste (AD FS)) sollte ich auswählen?

Das hängt von Ihrer lokalen Umgebung und den Anforderungen Ihres Unternehmens ab. In diesem Artikel finden Sie einen [Vergleich der verschiedenen Azure AD-Anmeldemethoden](active-directory-aadconnect-user-signin.md).

## <a name="is-pass-through-authentication-a-free-feature"></a>Ist die Passthrough-Authentifizierung ein kostenloses Feature?

Ja, es handelt sich um eine kostenlose Funktion, sodass Sie keine kostenpflichtigen Azure AD-Versionen benötigen. Die Funktion bleibt auch kostenlos, wenn sie allgemein verfügbar wird.

## <a name="is-pass-through-authentication-available-in-microsoft-cloud-germanyhttpwwwmicrosoftdecloud-deutschland-and-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Ist die Passthrough-Authentifizierung in [Microsoft Cloud Deutschland](http://www.microsoft.de/cloud-deutschland) und [Microsoft Azure Government Cloud](https://azure.microsoft.com/features/gov/) verfügbar?

Nein, die Passthrough-Authentifizierung ist nur in der weltweiten Instanz von Azure AD verfügbar.

## <a name="does-conditional-accessactive-directory-conditional-accessmd-work-with-pass-through-authentication"></a>Funktioniert der [bedingte Zugriff](../active-directory-conditional-access.md) mit der Passthrough-Authentifizierung?

Ja, alle Funktionen des bedingten Zugriffs, einschließlich der Azure Multi-Factor Authentication, funktionieren mit der Passthrough-Authentifizierung.

## <a name="does-pass-through-authentication-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Unterstützt die Passthrough-Authentifizierung eine alternative Anmelde-ID (Alternate ID) anstelle von „UserPrincipalName“ als Benutzernamen?

Ja. Die Passthrough-Authentifizierung unterstützt `Alternate ID` als Benutzername, wenn dies in Azure AD Connect wie [hier](active-directory-aadconnect-get-started-custom.md) beschrieben entsprechend konfiguriert ist. Nicht alle Office 365-Anwendungen unterstützen `Alternate ID`. Angaben zur Supporterklärung finden Sie in der Dokumentation der jeweiligen Anwendung.

## <a name="does-password-hash-synchronization-act-as-a-fallback-to-pass-through-authentication"></a>Kann die Kennworthashsynchronisierung als Fallback auf die Passthrough-Authentifizierung verwendet werden?

Nein. Die Kennworthashsynchronisierung ist kein generisches Fallback auf die Passthrough-Authentifizierung. Es dient nur als Fallback für [Szenarios, die zurzeit noch nicht von der Passthrough-Authentifizierung unterstützt werden](active-directory-aadconnect-pass-through-authentication-current-limitations.md#unsupported-scenarios). Um Benutzeranmeldefehler zu vermeiden, sollten Sie die Passthrough-Authentifizierung für eine [hohe Verfügbarkeit](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-4-ensure-high-availability) konfigurieren.

## <a name="can-i-install-an-azure-ad-application-proxyactive-directory-application-proxy-get-startedmd-connector-on-the-same-server-as-a-pass-through-authentication-agent"></a>Kann ich einen [Azure AD-Anwendungsproxy](../active-directory-application-proxy-get-started.md)-Connector auf demselben Server wie dem eines Passthrough-Authentifizierungs-Agents installieren?

Diese Konfiguration wird auch von den umbenannten Versionen des Passthrough-Authentifizierungs-Agenten (Versionen 1.5.193.0 oder höher) unterstützt.

## <a name="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need"></a>Welche Versionen von Azure AD Connect und dem Passthrough-Authentifizierungs-Agent werden benötigt?

Sie benötigen Version 1.1.486.0 oder höher für Azure AD Connect und 1.5.58.0 oder höher für den Passthrough-Authentifizierungs-Agent, damit dieses Feature funktioniert. Die gesamte Software sollte auf Servern mit Windows Server 2012 R2 oder höher installiert werden.

## <a name="what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-using-pass-through-authentication"></a>Was geschieht, wenn das Kennwort des Benutzers abgelaufen ist und er versucht, sich mit der Passthrough-Authentifizierung anzumelden?

Wenn Sie [das Kennwortrückschreiben](../active-directory-passwords-update-your-own-password.md) für einen bestimmten Benutzer konfiguriert haben und der Benutzer sich mit der Passthrough-Authentifizierung anmeldet, kann er sein Kennwort ändern oder zurücksetzen. Die Kennwörter werden erwartungsgemäß in das lokale Active Directory zurückgeschrieben.

Wenn das Kennwortrückschreiben jedoch nicht konfiguriert ist oder der Benutzer keine gültige Azure AD keine Lizenz hat, kann der Benutzer sein Kennwort in der Cloud nicht aktualisieren. Das Kennwort lässt sich nicht aktualisiert, auch wenn das Kennwort bereits abgelaufen ist. Der Benutzer sieht dann folgende Meldung: „Eine Kennwortänderung auf dieser Website wird von Ihrer Organisation nicht gestattet. Ändern Sie Ihr Kennwort anhand der von Ihrer Organisation empfohlenen Methode, oder bitten Sie Ihren Administrator um Hilfe.“. Der Benutzer oder der Administrator muss das Kennwort in Ihrem lokalen Active Directory zurücksetzen.

## <a name="how-does-pass-through-authentication-protect-you-against-brute-force-password-attacks"></a>Wie schützt die Passthrough-Authentifizierung vor Brute-Force-Kennwortangriffen?

Weitere Informationen finden Sie [in diesem Artikel](active-directory-aadconnect-pass-through-authentication-smart-lockout.md).

## <a name="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443"></a>Was kommunizieren Passthrough-Authentifizierungs-Agents über die Ports 80 und 443?

- Die Authentifizierung-Agents stellen über Port 443 HTTPS-Anforderungen für alle Feature-Vorgänge, z.B. das Aktivieren der Funktion, das Verarbeiten alle Benutzeranforderungen.
- Die Authentifizierungs-Agents stellen HTTP-Anforderungen über Port 80, um SSL-Zertifikatsperrlisten herunterzuladen.

     >[!NOTE]
     >Während der neuesten Updates haben wir die Anzahl der Ports reduziert, die die Authentifizierungs-Agents für die Kommunikation mit Azure AD erfordern. Wenn Sie eine ältere Versionen von Azure AD Connect und/oder eigenständige Connectors haben, sollten Sie diese zusätzlichen Ports weiterhin geöffnet lassen: 5671, 8080, 9090, 9091, 9350, 9352, 10100–10120.

## <a name="can-the-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server"></a>Können die Passthrough-Authentifizierungs-Agents über einen ausgehenden Webproxyserver kommunizieren?

Ja. Wenn in Ihrer lokalen Umgebung WPAD (Web Proxy Auto-Discovery) aktiviert ist, versuchen die Authentifizierungs-Agents automatisch, einen Webproxyserver im Netzwerk zu suchen und zu verwenden.

## <a name="can-i-install-two-or-more-pass-through-authentication-agents-on-the-same-server"></a>Kann ich zwei oder mehr Passthrough-Authentifizierungs-Agents auf dem gleichen Server installieren?

Nein, Sie können nur einen Passthrough-Authentifizierungs-Agent auf einem einzelnen Server installieren. Wenn Sie die Passthrough-Authentifizierung für hohe Verfügbarkeit konfigurieren möchten, befolgen Sie stattdessen die Anweisungen in diesem [Artikel](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-4-ensure-high-availability).

## <a name="i-already-use-active-directory-federation-services-ad-fs-for-azure-ad-sign-in-how-do-i-switch-it-to-pass-through-authentication"></a>Ich verwende bereits die Active Directory-Verbunddienste (AD FS) für die Anmeldung in Azure AD. Wie kann ich zur Passthrough-Authentifizierung wechseln?

Wenn Sie AD FS mit dem Azure AD Connect-Assistenten als Ihre Anmeldemethode konfiguriert haben, ändern Sie die Anmeldemethode für Benutzer in die Passthrough-Authentifizierung. Diese Änderung ermöglicht die Passthrough-Authentifizierung für den Mandanten und konvertiert _alle_ Ihre Verbunddomänen in verwaltete Domänen. Alle nachfolgenden Anmeldeanforderungen in Ihrem Mandanten werden mit der Passthrough-Authentifizierung verarbeitet. Derzeit besteht in Azure AD Connect keine Möglichkeit, eine AD FS und die Passthrough-Authentifizierung domänenübergreifend zusammen zu verwenden.

Wenn AD FS als Anmeldemethode _außerhalb_ des Azure AD Connect-Assistenten konfiguriert wurde, ändern Sie die Anmeldemethode für Benutzer in die Passthrough-Authentifizierung (über die Optionen „Nicht konfigurieren“). Diese Änderung lässt die Passthrough-Authentifizierung auf dem Mandanten zu. All Ihre Verbunddomänen verwenden weiterhin AD FS für die Anmeldung. Mit PowerShell lassen sich einige oder alle dieser Verbunddomänen manuell in verwaltete Domänen konvertieren. Daraufhin werden alle Anmeldeanforderungen für verwalteten Domänen (_ausschließlich_) von der Passthrough-Authentifizierung verarbeitet.

>[!IMPORTANT]
>Die Passthrough-Authentifizierung verarbeitet jedoch nicht die Anmeldung für ausschließlich cloudbasierte Azure AD-Benutzer.

## <a name="can-i-use-pass-through-authentication-in-a-multi-forest-ad-environment"></a>Kann ich die Passthrough-Authentifizierung in einer Active Directory-Umgebung mit mehreren Gesamtstrukturen verwenden?

Ja. Umgebungen mit mehreren Gesamtstrukturen werden unterstützt, wenn Gesamtstruktur-Vertrauensstellungen zwischen Ihren AD-Gesamtstrukturen bestehen und das Namensuffixrouting ordnungsgemäß konfiguriert ist.

## <a name="do-pass-through-authentication-agents-provide-load-balancing-capability"></a>Stellen die Passthrough-Authentifizierungs-Agents Lastenausgleichsfunktionen bereit?

Nein. Die Installation von mehreren Passthrough-Authentifizierungs-Agents stellt eine [hohe Verfügbarkeit](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-4-ensure-high-availability) sicher, jedoch nicht den Lastenausgleich. Letztlich verarbeiten möglicherweise ein oder zwei Authentifizierungs-Agents den Großteil der Anmeldeanforderungen.

Die Anforderungen zur Kennwortüberprüfung, die die Authentifizierungs-Agents verarbeiten müssen, sind einfach. Die maximale und durchschnittliche Last wird für die meisten Kunden daher problemlos von insgesamt zwei oder drei Authentifizierungs-Agents bewältigt.

Es wird empfohlen, die Authentifizierungs-Agents in der Nähe Ihres Domänencontrollers zu installieren, um die Anmeldungslatenz zu verbessern.

## <a name="can-i-install-the-first-pass-through-authentication-agent-on-a-server-other-than-the-one-that-runs-azure-ad-connect"></a>Kann ich den ersten Passthrough-Authentifizierungs-Agent auf einem anderen Server als demjenigen mit Azure AD Connect installieren?

Nein, dieses Szenario wird _nicht_ unterstützt.

## <a name="how-many-pass-through-authentication-agents-should-i-install"></a>Wie viele Passthrough-Authentifizierungs-Agents sollte ich installieren?

Wir empfehlen Folgendes:

- Installieren Sie insgesamt zwei oder drei Authentifizierungs-Agents. Dies ist für die meisten Kunden ausreichend.
- Installieren Sie die Authentifizierung-Agents auf den Domänencontrollern (oder so nah wie möglich), um die Anmeldelatenz zu verbessern.
- Stellen Sie sicher, dass Server (auf denen Authentifizierungs-Agents installiert sind) derselben AD-Gesamtstruktur wie die Benutzer hinzugefügt werden, deren Kennwörter überprüft werden müssen.

Beachten Sie, dass in einem System maximal 12 Authentifizierungs-Agents pro Mandant installiert werden können.

## <a name="how-can-i-disable-pass-through-authentication"></a>Wie kann ich die Passthrough-Authentifizierung deaktivieren?

Führen Sie den Azure AD Connect-Assistenten erneut aus, und ändern Sie die Anmeldemethode für Benutzer von der Passthrough-Authentifizierung in eine andere Methode. Diese Änderung deaktiviert die Passthrough-Authentifizierung für den Mandanten und deinstalliert den Authentifizierungs-Agent vom Server. Auf anderen Servern müssen Sie den Authentifizierungs-Agent manuell deinstallieren.

## <a name="what-happens-when-i-uninstall-a-pass-through-authentication-agent"></a>Was geschieht, wenn ich einen Passthrough-Authentifizierungs-Agent deinstalliere?

Durch die Deinstallation eines Passthrough-Authentifizierungs-Agent von einem Server werden keine Anmeldeanforderungen mehr angenommen. Stellen Sie sicher, dass ein anderer Authentifizierungs-Agent ausgeführt wird, bevor Sie diesen Vorgang ausführen, um eine Unterbrechung bei der Benutzeranmeldung für Ihren Mandanten zu vermeiden.

## <a name="next-steps"></a>Nächste Schritte
- [**Aktuelle Einschränkungen**](active-directory-aadconnect-pass-through-authentication-current-limitations.md): Diese Funktion befindet sich derzeit in der Vorschauphase. Erfahren Sie, welche Szenarios unterstützt werden.
- [**Schnellstart**](active-directory-aadconnect-pass-through-authentication-quick-start.md): Einrichten und Ausführen der Passthrough-Authentifizierung mit Azure AD
- [**Ausführliche technische Informationen**](active-directory-aadconnect-pass-through-authentication-how-it-works.md): Informationen zur Funktionsweise dieser Funktion
- [**Problembehandlung**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): Beheben häufig auftretender Probleme mit dieser Funktion
- [**Nahtlose SSO mit Azure AD**](active-directory-aadconnect-sso.md): Informationen zu dieser Ergänzungsfunktion
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Verfassen neuer Feature-Anforderungen

