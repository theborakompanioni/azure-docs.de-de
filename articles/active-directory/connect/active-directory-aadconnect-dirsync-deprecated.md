---
title: Upgrade von DirSync und Azure AD Sync | Microsoft Docs
description: "Beschreibt, wie Sie ein Upgrade von DirSync auf Azure AD Connect auf Azure AD Connect durchführen."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: bd68fb88-110b-4d76-978a-233e15590803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 4e4a4f4e299dc2747eb48bbd2e064cd80783211c
ms.openlocfilehash: cd11ef761a39b47e2426766cba64b3975595f4d4
ms.lasthandoff: 04/04/2017

---

# <a name="upgrade-windows-azure-active-directory-sync-and-azure-active-directory-sync"></a>Aktualisieren der Windows Azure Active Directory-Synchronisierung und Azure Active Directory-Synchronisierung
Azure AD Connect bietet die beste Möglichkeit, um Ihr lokales Verzeichnis mit Azure AD und Office 365 zu verbinden. Da Windows Azure Active Directory Sync (DirSync) und Azure AD Sync veraltet sind und der Support für diese Tools am 13. April 2017 endet, ist dies ein guter Zeitpunkt, um ein Upgrade auf Azure AD Connect durchzuführen.

Die beiden veralteten Tools zur Synchronisierung von Identitäten wurden für Kunden mit einer einzelnen Gesamtstruktur (DirSync) sowie für Kunden mit mehreren Gesamtstrukturen oder anderen erweiterten Anforderungen (Azure AD Sync) angeboten. Diese älteren Tools wurden von einer einzelnen Lösung abgelöst, die für alle Szenarien verfügbar ist: Azure AD Connect. Diese neue Lösung bietet neue Funktionalität, erweiterte Features und Unterstützung für neue Szenarien. Um Ihre lokalen Identitätsdaten weiterhin mit Azure AD und Office 365 synchronisieren zu können, wird dringend empfohlen, ein Upgrade auf Azure AD Connect durchzuführen.

Das letzte Release von DirSync wurde im Juli 2014 veröffentlicht, das letzte Release von Azure AD Sync im Mai 2015.

## <a name="what-is-azure-ad-connect"></a>Was ist Azure AD Connect?
Azure AD Connect ist der Nachfolger von DirSync und Azure AD Sync. Diese Lösung vereint alle Szenarien, die von den beiden Vorgängerprodukten unterstützt wurden. Weitere Informationen finden Sie unter [Integrieren Ihrer lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md).

## <a name="deprecation-schedule"></a>Zeitplan für die Einstellung
| Date | Kommentar |
| --- | --- |
| 13. April 2016 |Windows Azure Active Directory-Synchronisierungstool („DirSync“) und Microsoft Azure Active Directory-Synchronisierungstool („Azure AD Sync“) als veraltet bekanntgegeben. |
| 13. April 2017 |Ende des Supports. Um Supportanfragen zu übermitteln, müssen Kunden zunächst ein Upgrade auf Azure AD Connect durchführen. |
|31. Dezember 2017|Azure AD akzeptiert keine Kommunikation mehr vom Windows Azure Active Directory-Synchronisierungstool („DirSync“) und vom Microsoft Azure Active Directory-Synchronisierungstool („Azure AD Sync“).

## <a name="how-to-transition-to-azure-ad-connect"></a>So führen Sie ein Upgrade auf Azure AD Connect durch
Wenn Sie DirSync ausführen, haben Sie zwei Möglichkeiten: ein direktes Upgrade und eine parallele Bereitstellung. Ein direktes Upgrade wird für die meisten Kunden empfohlen sowie für Kunden mit einem aktuellen Betriebssystem und weniger als 50.000 Objekten. In anderen Fällen wird eine parallele Bereitstellung empfohlen, bei der Ihre DirSync-Konfiguration auf einen neuen Server mit Azure AD Connect verschoben wird.

Wenn Sie Azure AD Sync verwenden, wird ein direktes Upgrade empfohlen. Sie können auch parallel eine neuen Azure AD Connect-Server installieren und eine Swing-Migration von Ihrem Azure AD Sync-Server zu Azure AD Connect durchführen.

| Lösung | Szenario |
| --- | --- |
| [Upgrade von DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) |<li>Bei einem vorhandenen DirSync-Server, der bereits ausgeführt wird</li> |
| [Upgrade von Azure AD Sync](active-directory-aadconnect-upgrade-previous-version.md) |<li>Bei einer Migration von Azure AD Sync</li> |

Informationen zu einem direkten Upgrade von DirSync auf Azure AD Connect finden Sie in diesem Channel 9-Video:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-in-place-upgrade-from-legacy-tools/player]
>
>

## <a name="faq"></a>Häufig gestellte Fragen
**F: Ich habe eine E-Mail-Benachrichtigung vom Azure-Team bzw. eine Nachricht vom Office 365-Nachrichtencenter erhalten, ich verwende aber Connect.**  
Die Benachrichtigung wurde auch an Kunden gesendet, die Azure AD Connect mit Buildnummer 1.0.\*.0 (also eine Version vor Version 1.1) verwenden. Microsoft empfiehlt Kunden, immer die neueste Azure AD Connect-Versionen zu verwenden. Mit der in 1.1 eingeführten [automatischen Upgradefunktion](active-directory-aadconnect-feature-automatic-upgrade.md) lässt sich einfach sicherstellen, dass immer eine aktuelle Version von Azure AD Connect installiert ist.

**F: Funktioniert DirSync/Azure AD Sync ab dem 13. April 2017 nicht mehr?**  
DirSync/Azure AD Sync funktionieren am 13. April 2017 weiterhin.  Azure AD akzeptiert jedoch am 31. Dezember 2017 keine Kommunikation mehr von DirSync/Azure AD Sync.

**F: Von welchen DirSync-Versionen kann ich ein Upgrade durchführen?**  
Ein Upgrade kann von allen aktuell verwendeten DirSync-Versionen durchgeführt werden.

**F: Und was ist mit dem Azure AD-Connector für FIM/MIM?**  
Der Azure AD-Connector für FIM/MIM wurde **nicht** als veraltet bekanntgegeben. Dabei handelt es sich lediglich um ein **Feature Freeze**, d.h., es wird keine neue Funktionalität hinzugefügt, und es werden keine Programmfehlerbehebungen bereitgestellt. Kunden, die den Connector verwenden, sollten eine Migration zu Azure AD Connect planen. Es wird dringend empfohlen, keine neuen Bereitstellungen mit diesem Connector durchzuführen. Dieser Connector wird in der Zukunft als veraltet bekanntgegeben.

## <a name="additional-resources"></a>Weitere Ressourcen
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)

