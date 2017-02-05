---
title: 'Azure AD Connect Health: Versionsverlauf'
description: In diesem Dokument werden die Versionen von Azure AD Connect Health beschrieben und was darin enthalten ist.
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 8dd4e998-747b-4c52-b8d3-3900fe77d88f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: vakarand
translationtype: Human Translation
ms.sourcegitcommit: aa20b20c86763791eb579883b5273ea79cc714b5
ms.openlocfilehash: 12c14222be76ad6d31d968f0704159068c85b21c


---
# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect Health: Versionsveröffentlichungsverlauf
Das Azure Active Directory-Team aktualisiert Azure AD Connect Health regelmäßig mit neuen Features und Funktionen. In diesem Artikel werden die veröffentlichten Versionen und Features beschrieben.

## <a name="october-2016"></a>Oktober 2016
**Agent-Aktualisierung:**

* Azure AD Connect Health-Agent für AD FS \(Version 2.6.408.0\)
  1. Verbesserungen beim Erkennen von Client-IP-Adressen bei Authentifizierungsanforderungen
  2. Fehlerbehebungen im Zusammenhang mit Warnungen
* Azure AD Connect Health-Agent für AD DS (Version 2.6.408.0)
  1. Fehlerbehebungen im Zusammenhang mit Warnungen.
* Azure AD Connect Health-Agent für Sync (Version 2.6.353.0), veröffentlicht mit Azure AD Connect Version 1.1.281.0
  1. Bereitstellen der für die Berichte zu Synchronisierungsfehlern benötigten Daten
  2. Fehlerbehebungen im Zusammenhang mit Warnungen

**Neue Vorschaufeatures:**

* Berichte zu Synchronisierungsfehlern für Azure AD Connect

**Neue Features:**

* Azure AD Connect Health für AD FS – IP-Adressfeld ist im Bericht über die 50 häufigsten Benutzer mit falschem Benutzernamen/Kennwort verfügbar.

## <a name="july-2016"></a>Juli 2016
**Neue Vorschaufeatures:**

* [Azure AD Connect Health für AD DS](active-directory-aadconnect-health-adds.md).

## <a name="january-2016"></a>Januar 2016
**Agent-Aktualisierung:**

* Azure AD Connect Health-Agent für AD FS (Version 2.6.91.1512)

**Neue Features:**

* [Tool zum Testen der Verbindung für Azure AD Connect Health-Agents](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)

## <a name="november-2015"></a>November 2015
**Neue Features:**

* Unterstützung für die [rollenbasierte Zugriffssteuerung](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)

**Neue Vorschaufeatures:**

* [Azure AD Connect Health für die Synchronisierung](active-directory-aadconnect-health-sync.md)

**Behobene Probleme:**

* Fehlerbehebungen für bei Agent-Registrierungen aufgetretene Fehler

## <a name="september-2015"></a>September 2015
**Neue Features:**

* Bericht über falschen Benutzernamen/falsches Kennwort für AD FS
* Konfigurationsunterstützung für nicht authentifizierten HTTP-Proxy
* Konfigurationsunterstützung für Agents auf Serverkern
* Verbesserte Warnungen für AD FS
* Verbesserte Verbindungen und Datenuploads beim Azure AD Connect Health-Agent für AD FS

**Behobene Probleme:**

* Fehlerbehebungen bei den Nutzungsinformationen für AD FS-Fehlertypen

## <a name="june-2015"></a>Juni 2015
**Erste Version von Azure AD Connect Health für AD FS und AD FS-Proxy**

**Neue Features:**

* Warnungen für die Überwachung von AD FS und AD FS-Proxy-Server mit E-Mail-Benachrichtigungen
* Einfacher Zugriff auf AD FS-Topologien und Muster in AD FS-Leistungsindikatoren
* Trend bei erfolgreichen Tokenanforderungen auf AD FS-Servern gruppiert nach Anwendungen, Authentifizierungsmethoden, Netzwerkspeicherort-Anforderungen usw.
* Trends bei fehlgeschlagenen Anforderungen an AD FS-Server gruppiert nach Anwendungen, Fehlertypen usw.
* Einfachere Agent-Bereitstellung mit den Anmeldeinformationen für globale Azure AD-Administratoren  

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Überwachen Ihrer lokalen Identitätsinfrastruktur und Synchronisierung von Diensten in der Cloud](active-directory-aadconnect-health.md)




<!--HONumber=Nov16_HO3-->


