---
title: Leitfaden zur Lizenzierung von Azure Active Directory B2B-Kollaboration | Microsoft-Dokumentation
description: "Für die Azure Active Directory B2B-Kollaboration sind je nach den Features, die Sie für Ihre Benutzer der B2B-Kollaboration verwenden möchten, kostenpflichtige Azure AD-Lizenzen erforderlich."
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
ms.date: 02/09/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: a4e59dfa8a098f63c3173176c4d2675d6a59af00
ms.openlocfilehash: 59a7fe2d1eea948f1452f21d2185e6bdf93dc41f


---

# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Leitfaden zur Lizenzierung von Azure Active Directory B2B-Kollaboration

Bei der Azure Active Directory (Azure AD) B2B-Kollaboration wird ein ausgewählter Satz von vorhandenen Azure AD-Features auf Gastbenutzer erweitert, die eine Einladung für den Azure AD-Mandanten erhalten haben. Daher werden Gastbenutzer bei der Azure AD B2B-Kollaboration mithilfe von Azure AD-Lizenzen lizenziert und wie hier beschrieben den vorhandenen Lizenztarifen „Free“, „Basic“ und „Premium P1/P2“ zugeordnet: https://azure.microsoft.com/de-de/pricing/details/active-directory/.

Es fallen Kosten für das Einladen von B2B-Benutzern und das Zuweisen zu einer Anwendung in Azure AD an. Außerdem sind bis zu zehn Apps pro Gastbenutzer und drei einfache Berichte für B2B-Benutzer kostenlos, da sie im Azure AD-Tarif „Free“ enthalten sind.
Alle kostenpflichtigen Azure AD-Features, die per B2B-Kollaborationsfunktion auf B2B-Benutzer erweitert werden, müssen anhand von kostenpflichtigen Azure AD-Lizenzen (Basic, Premium P1 oder Premium P2, je nach den verwendeten Features) lizenziert werden. Der Mandant, von dem die Einladung ausgeht, erhält für jede kostenpflichtige Azure AD-Lizenz B2B-Rechte für fünf Benutzer. Jede kostenpflichtige Azure AD-Lizenz, mit der Rechte für einen Mitarbeiterbenutzer eines Mandanten bereitgestellt werden, enthält dann also auch Rechte für fünf B2B-Benutzer, die vom Mandanten eingeladen werden.

## <a name="licensing-examples"></a>Beispiele für die Lizenzierung
- Ein Kunde möchte 100 B2B-Benutzer für seinen Azure AD-Mandanten einladen und verwendet die gruppenbasierte Zugriffsverwaltung und Bereitstellung für alle Benutzer, aber für 50 Benutzer werden außerdem MFA und der bedingte Zugriff benötigt. Der Kunde muss in diesem Fall zehn Azure AD-Lizenzen vom Typ „Basic“ und zehn Azure AD-Lizenzen vom Typ „Premium P1“ erwerben, um alle B2B-Benutzer abzudecken. Ebenso muss ein einladender Mandant über genügend Azure AD-Lizenzen vom Typ „Premium P2“ verfügen, um alle B2B-Benutzer im Verhältnis 5:1 abzudecken, wenn dieser Mandant für B2B-Benutzer die Verwendung von Identity Protection-Features plant.
- Ein Kunde verfügt über zehn Mitarbeiter, die derzeit alle über eine Azure AD-Lizenz vom Typ „Premium P1“ abgedeckt sind. Der Kunde möchte nun 60 B2B-Benutzer einladen, für die Multi-Factor Authentication (MFA) benötigt wird. Laut dem in der Lizenzierungsregel festgelegten Verhältnis von 5:1 muss der Kunde mindestens zwölf Azure AD-Lizenzen vom Typ „Premium P1“ besitzen, um alle 60 Benutzer für die B2B-Kollaboration abzudecken. Da bereits zehn Premium P1-Lizenzen für die zehn Mitarbeiter vorhanden sind, ist der Kunde im Besitz von Rechten, die ihm das Einladen von 50 B2B-Benutzern mit Premium P1-Features wie MFA ermöglichen. In diesem Beispiel müssen also zwei zusätzliche Premium P1-Lizenzen erworben werden, um die restlichen zehn Benutzer für die B2B-Kollaboration abzudecken.

> [!NOTE]
Es ist nicht erforderlich, den B2B-Benutzern Lizenzen zuzuweisen, um diese Benutzerrechte für die B2B-Kollaboration zu aktivieren.

Der Kunde, der im Besitz des einladenden Mandanten ist, muss ermitteln, für wie viele Benutzer der B2B-Kollaboration kostenpflichtige Azure AD-Funktionen benötigt werden. Je nachdem, ob es sich um Features der Tarife „Basic“, „Premium P1“ oder „Premium P2“ handelt, müssen Kunden über eine ausreichende Anzahl von geeigneten kostenpflichtigen Azure AD-Lizenzen verfügen, um die Benutzer der B2B-Kollaboration im Verhältnis 5:1 abzudecken. Falls ein Unternehmen zusätzliche Benutzerrechte für die B2B-Kollaboration benötigt, muss es die erforderlichen kostenpflichtigen Azure AD-Lizenzen erwerben.

## <a name="additional-licensing-details"></a>Weitere Details zur Lizenzierung
- Bei der B2B-Kollaboration können unterschiedliche Funktionen für unterschiedliche Benutzer der B2B-Kollaboration basierend auf dem vorhandenen Modell der Azure AD-Editionen bereitgestellt werden.
- Jede kostenpflichtige Azure AD-Lizenz enthält Rechte für fünf Benutzer der B2B-Kollaboration (5:1-Modell).
- Es ist nicht erforderlich, B2B-Benutzerkonten tatsächlich Lizenzen zuzuweisen. Die Berechnung und Berichterstellung wird automatisch durchgeführt.
- Jeder eingeladene Benutzer erhält die Rechte, die für die Free-Edition von Azure AD bereitgestellt werden, wenn im Mandanten keine kostenpflichtige Azure AD-Lizenz vorhanden ist.
- Wenn ein Benutzer der B2B-Kollaboration als Mitarbeiter seiner Organisation über eine kostenpflichtige Azure AD-Lizenz verfügt, verbraucht er keine Lizenz des einladenden Mandanten für die B2B-Kollaboration.

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Kollaboration:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Wie fügen Azure Active Directory-Administratoren B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-admin-add-users.md)
* [Wie fügen Information-Worker B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-iw-add-users.md)
* [Die Elemente der Einladungs-E-Mail von B2B-Zusammenarbeit](active-directory-b2b-invitation-email.md)
* [B2B-Zusammenarbeit: Einlösen von Einladungen](active-directory-b2b-redemption-experience.md)
* [Problembehandlung für die Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-troubleshooting.md)
* [Häufig gestellte Fragen zur Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-faq.md)
* [Azure Active Directory B2B-Zusammenarbeit: API und Anpassung](active-directory-b2b-api.md)
* [Multi-Factor Authentication für Benutzer der B2B-Zusammenarbeit](active-directory-b2b-mfa-instructions.md)
* [Hinzufügen von Benutzern der B2B-Zusammenarbeit ohne Einladung](active-directory-b2b-add-user-without-invite.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)



<!--HONumber=Feb17_HO2-->


