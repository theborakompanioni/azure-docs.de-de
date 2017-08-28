---
title: Leitfaden zur Lizenzierung von Azure Active Directory B2B-Kollaboration | Microsoft-Dokumentation
description: "Für die Azure Active Directory B2B-Kollaboration sind keine kostenpflichtigen Azure AD-Lizenzen erforderlich, aber es sind auch kostenpflichtige Features für B2B-Gastbenutzer erhältlich."
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
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: sasubram
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: a9cfd6052b58fe7a800f1b58113aec47a74095e3
ms.openlocfilehash: dfef32c05af157ae8d3a5434016f87f488a35051
ms.contentlocale: de-de
ms.lasthandoff: 08/12/2017

---

# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Leitfaden zur Lizenzierung von Azure Active Directory B2B-Kollaboration

Mithilfe der Azure AD B2B-Zusammenarbeitsfunktionen können Sie Gastbenutzer für Ihren Azure AD-Mandanten einladen, um ihnen Zugriff auf Azure AD-Dienste und andere Ressourcen in Ihrer Organisation zu gewähren. Falls Sie den Zugriff auf kostenpflichtige Azure AD-Features ermöglichen möchten, müssen B2B-Zusammenarbeitsgastbenutzer mit entsprechenden Azure AD-Lizenzen lizenziert werden. 

Dies bedeutet Folgendes:
* Für Gastbenutzer stehen ohne zusätzliche Lizenzen Azure AD Free-Funktionen zur Verfügung.
* Wenn Sie B2B-Benutzern Zugriff auf kostenpflichtige Azure AD-Funktionen gewähren möchten, müssen Sie zur Unterstützung dieser B2B-Gastbenutzer über ausreichende Lizenzen verfügen.
* Ein einladender Mandant mit einer kostenpflichtigen Azure AD-Lizenz besitzt B2B-Kollaborationsbenutzerrechte für weitere fünf B2B-Gastbenutzer, die für den Mandanten eingeladen wurden.
* Der Kunde, der im Besitz des einladenden Mandanten ist, muss ermitteln, wie viele B2B-Kollaborationsbenutzer kostenpflichtige Azure AD-Funktionen benötigen. Je nachdem, welche kostenpflichtigen Azure AD-Features Sie für Ihre Gastbenutzer verwenden möchten, müssen Sie über genügend kostenpflichtige Azure AD-Lizenzen zur Unterstützung der B2B-Kollaborationsbenutzer im selben Verhältnis von 5:1 verfügen.

Ein B2B-Zusammenarbeitsgastbenutzer wird von einem Partnerunternehmen als Benutzer hinzugefügt, nicht als Mitarbeiter Ihrer Organisation oder als Mitarbeiter eines anderen Unternehmens in Ihrem Großkonzern. Ein B2B-Gastbenutzer kann sich mit externen Anmeldeinformationen oder den Anmeldeinformationen Ihrer Organisation anmelden, wie in diesem Artikel beschrieben wird. 

Mit anderen Worten: Die B2B-Lizenzierung wird nicht abhängig von der Authentifizierungsmethode des Benutzers festgelegt, sondern anhand der Beziehung zwischen dem Benutzer und Ihrer Organisation. Wenn diese Benutzer keine Partner sind, werden sie hinsichtlich der Lizenzbestimmungen unterschiedlich behandelt. Sie werden für Lizenzierungszwecke nicht als B2B-Zusammenarbeitsbenutzer behandelt, selbst, wenn „Gast“ für „UserType“ festgelegt ist. Sie sollten ordnungsgemäß mit einer Lizenz pro Benutzer lizenziert werden. Zu diesen Benutzer gehören Folgenden:
* Ihre Mitarbeiter
* Mitarbeiter, die sich mit externen Identitäten anmelden
* Ein Mitarbeiter eines anderen Unternehmens in Ihrem Großkonzern


## <a name="licensing-examples"></a>Beispiele für die Lizenzierung
- Ein Kunde möchte für seinen Azure AD-Mandanten 100 B2B-Kollaborationsbenutzer einladen. Der Kunde weist die Zugriffsverwaltung und Bereitstellung für alle Benutzer zu, aber 50 Benutzer benötigen zusätzlich MFA und bedingten Zugriff. Dieser Kunde muss zehn Azure AD-Lizenzen vom Typ „Basic“ und zehn Azure AD-Lizenzen vom Typ „Premium P1“ erwerben, um alle B2B-Benutzer richtig abzudecken. Wenn der Kunde plant, Identity Protection-Features mit B2B-Benutzern zu verwenden, müssen Azure AD-Lizenzen vom Typ „Premium P2“ vorhanden sein, um die eingeladenen Benutzer in demselben Verhältnis von 5:1 abzudecken.
- Ein Kunde verfügt über zehn Mitarbeiter, die derzeit alle über eine Azure AD-Lizenz vom Typ „Premium P1“ abgedeckt sind. Der Kunde möchte nun 60 B2B-Benutzer einladen, für die Multi-Factor Authentication (MFA) benötigt wird. Laut dem in der Lizenzierungsregel festgelegten Verhältnis von 5:1 muss der Kunde mindestens zwölf Azure AD-Lizenzen vom Typ „Premium P1“ besitzen, um alle 60 Benutzer für die B2B-Kollaboration abzudecken. Da bereits zehn Premium P1-Lizenzen für die zehn Mitarbeiter vorhanden sind, ist der Kunde im Besitz von Rechten, die ihm das Einladen von 50 B2B-Benutzern mit Premium P1-Features wie MFA ermöglichen. In diesem Beispiel müssen also zwei zusätzliche Premium P1-Lizenzen erworben werden, um die restlichen zehn Benutzer für die B2B-Kollaboration abzudecken.

> [!NOTE]
> Es ist noch nicht möglich, den B2B-Benutzern Lizenzen direkt zuzuweisen, um diese Benutzerrechte für die B2B-Kollaboration zu aktivieren.

Der Kunde, der im Besitz des einladenden Mandanten ist, muss ermitteln, wie viele B2B-Kollaborationsbenutzer kostenpflichtige Azure AD-Funktionen benötigen. Je nachdem, welche kostenpflichtigen Azure AD-Features Sie für Ihre Gastbenutzer verwenden möchten, müssen Sie über genügend kostenpflichtige Azure AD-Lizenzen zum Abdecken der B2B-Kollaborationsbenutzer im Verhältnis 5:1 verfügen. 

## <a name="additional-licensing-details"></a>Weitere Details zur Lizenzierung
- Es ist nicht erforderlich, B2B-Benutzerkonten tatsächlich Lizenzen zuzuweisen. Basierend auf dem Verhältnis von 5:1 wird die Lizenzierung automatisch berechnet und gemeldet.
- Wenn im Mandanten keine kostenpflichtige Azure AD-Lizenz vorhanden ist, erhält jeder eingeladene Benutzer die Rechte, die für die Azure AD Free-Edition von Azure AD bereitgestellt werden.
- Wenn ein B2B-Kollaborationsbenutzer bereits über eine kostenpflichtige Azure AD-Lizenz seiner Organisation verfügt, verbraucht er keine Lizenz des einladenden Mandanten für die B2B-Kollaboration.

## <a name="advanced-discussion-what-are-the-licensing-considerations-when-we-add-users-from-a-conglomerate-organization-as-members-using-your-apis"></a>Weiter gehende Fragen: Was muss bei der Lizenzierung beachtet werden, wenn wir mit Ihren APIs Benutzer aus einer Mischorganisation als Mitglieder hinzufügen?
Ein B2B-Gastbenutzer ist ein Benutzer, der von einer Partnerorganisation für die Zusammenarbeit mit der Gastgeberorganisation eingeladen wird. In der Regel gilt jeder hiervon abweichende Fall auch dann nicht als B2B-Fall, wenn B2B-Features verwendet werden. Betrachten wir zwei spezielle Fälle:

1. Ein Gastgeber lädt einen Mitarbeiter unter Verwendung einer Consumeradresse ein.
  * Dieses Szenario ist nicht mit unseren Lizenzierungsrichtlinien konform und daher nicht zu empfehlen.

2. Eine Gastgeberorganisation fügt einen Benutzer aus einer anderen Mischorganisation hinzu.
  1. In diesem Fall wird der Benutzer mit B2B-APIs eingeladen, aber es handelt sich nicht um einen herkömmlichen B2B-Fall. Idealerweise sollten diese Organisationen die Benutzer der anderen Organisation als Mitglieder einladen (unsere API lässt dies zu). In diesem Fall müssen diesen Mitgliedern Lizenzen zugewiesen werden, damit sie auf die Ressourcen in der einladenden Organisation Zugriff haben.

  2. Manche Organisationen möchten möglicherweise als Richtlinie festlegen, dass die Benutzer der anderen Organisation als Gast eingeladen werden. Für dieses Szenario gibt es zwei Fälle:
      * Die Mischorganisation verwendet bereits Azure AD, und die eingeladenen Benutzer sind in der anderen Organisation lizenziert: In diesem Fall erwarten wir nicht, dass eingeladene Benutzer das weiter oben in diesem Dokument beschriebene Verhältnis von 1:5 einhalten müssen. 

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

