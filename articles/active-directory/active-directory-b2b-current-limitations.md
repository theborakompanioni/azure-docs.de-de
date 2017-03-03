---
title: "Aktuelle Einschränkungen der Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation"
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
ms.sourcegitcommit: 0c07c842ba8c6214d6746b0361af7b416069a6f5
ms.openlocfilehash: 4dae74740d8fd00d36492e5a9fad8848b5d03952
ms.lasthandoff: 02/17/2017


---

# <a name="azure-active-directory-b2b-collaboration-current-limitations"></a>Aktuelle Einschränkungen der Azure Active Directory B2B-Zusammenarbeit

1. Die Einladungs-APIs für Azure Active Directory B2B-Zusammenarbeit befinden sich in der Betatestphase. Diese API-Benutzeroberfläche ist eine Weiterentwicklung, unterliegt jedoch, wie alle Betaversionen, der Vereinbarung für Betanamespaces. Mit dem allgemein verfügbaren Release werden die API-Versionen durchnummeriert.
2. Mögliche doppelte Multi-Factor Authentication (MFA). Wenn Ihr Partner bereits eine MFA-Richtlinie eingerichtet hat, erfolgt die Durchführung und Verwaltung der MFA für die B2B-Zusammenarbeit bei der einladenden Organisation. Dies ist wünschenswert, da damit alle Identitäten abgedeckt werden und Sie die Authentifizierungsstärke für diejenigen Benutzer steuern können, die Sie zur B2B-Zusammenarbeit einladen. Wenn einer Ihrer Partner allerdings eine MFA-Richtlinie eingerichtet und diese durchsetzt, müssen die Benutzer des Partners die MFA einmal in der eigenen Organisation und einmal in Ihrer Organisation durchführen. In einer zukünftigen Version werden wir eine Richtlinie einführen, mit der Sie auswählen können, ob Sie der MFA bestimmter Partner vertrauen, um das Problem einer doppelten MFA zu vermeiden.
3. Instant-On. Im Workflow der B2B-Zusammenarbeit werden Benutzer zum Verzeichnis hinzugefügt und während der Einlösung der Einladung, der App-Zuweisung usw. dynamisch aktualisiert. Die Aktualisierungs- und Schreibvorgänge erfolgen im Allgemeinen in einer Verzeichnisinstanz und müssen in allen Instanzen repliziert werden. Wir haben festgestellt, dass die Zeitspanne, die für die Replikation erforderlich ist, zu Problemen führen kann, die sich als Autorisierungsprobleme manifestieren. Wir arbeiten daran, diese Art von Problemen vor dem Release in die allgemeine Verfügbarkeit zu beseitigen oder zumindest zu minimieren. Sie werden auf dieses Problem vermutlich nicht stoßen. Sollte das Problem doch auftreten, lässt es sich durch eine Aktualisierung oder Wiederholung lösen.

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

