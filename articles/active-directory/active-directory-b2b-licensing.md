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
ms.date: 04/12/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: 7f469fb309f92b86dbf289d3a0462ba9042af48a
ms.openlocfilehash: 4e620f3d76caa25ac0e5afb134f37ffe263935f0
ms.lasthandoff: 04/13/2017


---

# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Leitfaden zur Lizenzierung von Azure Active Directory B2B-Kollaboration

Bei der Azure Active Directory (Azure AD) B2B-Kollaboration wird ein ausgewählter Satz von vorhandenen Azure AD-Features auf Gastbenutzer erweitert, die eine Einladung für den Azure AD-Mandanten erhalten haben. Daher werden Gastbenutzer bei der Azure AD B2B-Zusammenarbeit mithilfe von Azure AD-Lizenzen lizenziert und wie hier beschrieben den vorhandenen Lizenztarifen „Free“, „Basic“ und „Premium P1/P2“ zugeordnet: https://azure.microsoft.com/pricing/details/active-directory/.

Es fallen Kosten für das Einladen von B2B-Benutzern und das Zuweisen zu einer Anwendung in Azure AD an. Außerdem sind bis zu zehn Apps pro Gastbenutzer und drei einfache Berichte für B2B-Benutzer kostenlos, da sie im Azure AD-Tarif „Free“ enthalten sind.
Alle kostenpflichtigen Azure AD-Features, die per B2B-Kollaborationsfunktion auf B2B-Benutzer erweitert werden, müssen anhand von kostenpflichtigen Azure AD-Lizenzen (Basic, Premium P1 oder Premium P2, je nach den verwendeten Features) lizenziert werden. Der Mandant, von dem die Einladung ausgeht, erhält für jede kostenpflichtige Azure AD-Lizenz B2B-Rechte für fünf Benutzer. Das bedeutet: Jede kostenpflichtige Azure AD-Lizenz, die die Rechte für kostenpflichtige Azure AD-Features einem Mitarbeiterbenutzer eines Mandanten bereitstellt, stellt jetzt auch die Rechte für die gleichen kostenpflichtigen Azure AD-Features 5 zusätzlichen B2B-Benutzern bereit, die zu dem Mandanten eingeladen sind.

## <a name="licensing-examples"></a>Beispiele für die Lizenzierung
- Ein Kunde möchte 100 B2B-Benutzer für seinen Azure AD-Mandanten einladen und verwendet die gruppenbasierte Zugriffsverwaltung und Bereitstellung für alle Benutzer, aber für 50 Benutzer werden außerdem MFA und der bedingte Zugriff benötigt. Der Kunde muss in diesem Fall zehn Azure AD-Lizenzen vom Typ „Basic“ und zehn Azure AD-Lizenzen vom Typ „Premium P1“ erwerben, um alle B2B-Benutzer abzudecken. Ebenso muss ein einladender Mandant über genügend Azure AD-Lizenzen vom Typ „Premium P2“ verfügen, um alle B2B-Benutzer im Verhältnis 5:1 abzudecken, wenn dieser Mandant für B2B-Benutzer die Verwendung von Identity Protection-Features plant.
- Ein Kunde verfügt über zehn Mitarbeiter, die derzeit alle über eine Azure AD-Lizenz vom Typ „Premium P1“ abgedeckt sind. Der Kunde möchte nun 60 B2B-Benutzer einladen, für die Multi-Factor Authentication (MFA) benötigt wird. Laut dem in der Lizenzierungsregel festgelegten Verhältnis von 5:1 muss der Kunde mindestens zwölf Azure AD-Lizenzen vom Typ „Premium P1“ besitzen, um alle 60 Benutzer für die B2B-Kollaboration abzudecken. Da bereits zehn Premium P1-Lizenzen für die zehn Mitarbeiter vorhanden sind, ist der Kunde im Besitz von Rechten, die ihm das Einladen von 50 B2B-Benutzern mit Premium P1-Features wie MFA ermöglichen. In diesem Beispiel müssen also zwei zusätzliche Premium P1-Lizenzen erworben werden, um die restlichen zehn Benutzer für die B2B-Kollaboration abzudecken.

> [!NOTE]
> Es ist nicht erforderlich, den B2B-Benutzern Lizenzen zuzuweisen, um diese Benutzerrechte für die B2B-Kollaboration zu aktivieren.

Der Kunde, der im Besitz des einladenden Mandanten ist, muss ermitteln, für wie viele Benutzer der B2B-Kollaboration kostenpflichtige Azure AD-Funktionen benötigt werden. Je nachdem, ob es sich um Features der Tarife „Basic“, „Premium P1“ oder „Premium P2“ handelt, müssen Kunden über eine ausreichende Anzahl von geeigneten kostenpflichtigen Azure AD-Lizenzen verfügen, um die Benutzer der B2B-Kollaboration im Verhältnis 5:1 abzudecken. Falls ein Unternehmen zusätzliche Benutzerrechte für die B2B-Kollaboration benötigt, muss es die erforderlichen kostenpflichtigen Azure AD-Lizenzen erwerben.

## <a name="additional-licensing-details"></a>Weitere Details zur Lizenzierung
- Bei der B2B-Kollaboration können unterschiedliche Funktionen für unterschiedliche Benutzer der B2B-Kollaboration basierend auf dem vorhandenen Modell der Azure AD-Editionen bereitgestellt werden.
- Jede kostenpflichtige Azure AD-Lizenz enthält Rechte für fünf Benutzer der B2B-Kollaboration (5:1-Modell).
- Es ist nicht erforderlich, B2B-Benutzerkonten tatsächlich Lizenzen zuzuweisen. Die Berechnung und Berichterstellung wird automatisch durchgeführt.
- Jeder eingeladene Benutzer erhält die Rechte, die für die Free-Edition von Azure AD bereitgestellt werden, wenn im Mandanten keine kostenpflichtige Azure AD-Lizenz vorhanden ist.
- Wenn ein Benutzer der B2B-Kollaboration als Mitarbeiter seiner Organisation über eine kostenpflichtige Azure AD-Lizenz verfügt, verbraucht er keine Lizenz des einladenden Mandanten für die B2B-Kollaboration.

## <a name="advanced-discussion-what-are-the-licensing-considerations-when-we-add-users-from-a-conglomerate-organization-as-members-using-your-apis"></a>Weiter gehende Fragen: Was muss bei der Lizenzierung beachtet werden, wenn wir mit Ihren APIs Benutzer aus einer Mischorganisation als Mitglieder hinzufügen?
Ein B2B-Gastbenutzer ist ein Benutzer, der von einer Partnerorganisation für die Zusammenarbeit mit der Gastgeberorganisation eingeladen wird. In der Regel ist jeder hiervon abweichende Fall nicht für B2B qualifiziert, auch wenn B2B-Features verwendet werden. Betrachten wir zwei spezielle Fälle:

1. Ein Gastgeber lädt einen Mitarbeiter unter Verwendung einer Consumeradresse ein.
  1. Dies entspricht nicht unseren Lizenzierungsrichtlinien und wird derzeit nicht empfohlen.

2.    Eine Gastgeberorganisation fügt einen Benutzer aus einer anderen Mischorganisation hinzu.
  1. Dies ist der Fall, wenn der Benutzer mit B2B-APIs eingeladen wird, es ist jedoch kein herkömmliches B2B. Idealerweise sollten diese Organisationen die Benutzer der anderen Organisation als Mitglieder einladen (unsere API lässt dies zu). In diesem Fall müssen diesen Mitgliedern Lizenzen zugewiesen werden, damit sie auf die Ressourcen in der einladenden Organisation Zugriff haben.

  2. Manche Organisationen möchten möglicherweise als Richtlinie festlegen, dass die Benutzer der anderen Organisation als Gast eingeladen werden. Für dieses Szenario gibt es zwei Fälle:
      * Die Mischorganisation verwendet bereits Azure AD, und die eingeladenen Benutzer sind in der anderen Organisation lizenziert: In diesem Fall erwarten wir nicht, dass die eingeladenen Benutzer das weiter oben in diesem Dokument beschriebene Verhältnis von 1:5 einhalten müssen. 

      * Die Mischorganisation verwendet nicht Azure AD oder verfügt über keine entsprechenden Lizenzen: Halten Sie in diesem Fall das weiter oben beschriebene Verhältnis von 1:5 ein.


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

