---
title: "Einschränkungen der Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation"
description: "Aktuelle Einschränkungen der Azure Active Directory B2B-Zusammenarbeit (Vorschau)"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 02/16/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: 0e71a840d4f503779131ee4a21fe6063d33185f1
ms.openlocfilehash: cbefca2d45a332cd57cfea49dfeaa300426d5502
ms.lasthandoff: 02/24/2017


---

# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Einschränkungen der Azure AD B2B-Zusammenarbeit
Azure Active Directory B2B-Zusammenarbeit (Azure AD) unterliegt derzeit den in diesem Artikel beschriebenen Einschränkungen.

## <a name="invitation-apis-are-in-preview"></a>Einladungs-APIs sind in der Vorschau
Die API-Oberfläche ist unser voraussichtlicher Schritt nach vorn. Doch wie alle Vorabversionen unterliegt die API dem Vorschaunamespacevertrag. Mit unserem Release für allgemeine Verfügbarkeit (General Availability, GA) wird die API eine Versionsnummer erhalten.

## <a name="possible-double-multi-factor-authentication"></a>Mögliche doppelte mehrstufige Authentifizierung
Diese Redundanz kann auftreten, wenn Ihr Partner bereits über eine Richtlinie für die Azure Multi-Factor Authentication verfügt. Mehrstufige Authentifizierung für B2B-Zusammenarbeit wird in der einladenden Organisation durchgeführt und verwaltet. Eine solche Authentifizierung ist wünschenswert, da damit alle Identitäten abgedeckt werden und Sie die Authentifizierungsstärke für diejenigen Benutzer steuern können, die Sie zur B2B-Zusammenarbeit einladen.

Wenn ein Partner jedoch bereits die mehrstufige Authentifizierung eingerichtet hat und sie erzwingt, müssen die Benutzer des Partners die Authentifizierung möglicherweise einmal in ihrer eigenen Organisation ausführen und dann erneut in Ihrer.

In einem zukünftigen Release möchten wir eine Richtlinie einführen, mit der Sie das Problem der doppelten Authentifizierung vermeiden können, indem Sie auswählen, der mehrstufigen Authentifizierung des Partners zu vertrauen.

## <a name="instant-on"></a>Instant-On
Im Workflow der B2B-Zusammenarbeit werden Benutzer zum Verzeichnis hinzugefügt und während der Einlösung der Einladung, der App-Zuweisung usw. dynamisch aktualisiert. Die Aktualisierungs- und Schreibvorgänge erfolgen im Allgemeinen in einer Verzeichnisinstanz und müssen in allen Instanzen repliziert werden. Wir haben festgestellt, dass wegen der Zeitspanne, die für die Replikation erforderlich sein kann, manchmal Autorisierungsprobleme auftreten können. Wir arbeiten intensiv daran, diese Probleme vor unserem GA-Release zu minimieren bzw. eliminieren. In der Zwischenzeit werden Sie sie kaum bemerken, aber falls doch, sollten sie sich durch Aktualisieren oder erneuten Versuch lösen lassen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Kollaboration:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Eigenschaften von B2B-Zusammenarbeitsbenutzern](active-directory-b2b-user-properties.md)
* [Hinzufügen eines B2B-Zusammenarbeitsbenutzers zu einer Rolle](active-directory-b2b-add-guest-to-role.md)
* [Delegieren von Einladungen zur B2B-Zusammenarbeit](active-directory-b2b-delegate-invitations.md)
* [Dynamische Gruppen und B2B-Zusammenarbeit](active-directory-b2b-dynamic-groups.md)
* [B2B-Zusammenarbeit: Code- und PowerShell-Beispiele](active-directory-b2b-code-samples.md)
* [Konfigurieren von SaaS-Apps für die B2B-Zusammenarbeit](active-directory-b2b-configure-saas-apps.md)
* [Benutzertoken für die B2B-Zusammenarbeit](active-directory-b2b-user-token.md)
* [Zuordnen von Benutzeransprüchen für die B2B-Zusammenarbeit](active-directory-b2b-claims-mapping.md)
* [Externe Office 365-Freigaben](active-directory-b2b-o365-external-user.md)

