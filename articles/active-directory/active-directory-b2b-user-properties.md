---
title: Eigenschaften eines Azure Active Directory B2B-Zusammenarbeitsbenutzers | Microsoft-Dokumentation
description: Die Eigenschaften von Benutzern der Azure Active Directory B2B-Zusammenarbeit sind konfigurierbar.
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
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 3d7801d8a53ac048333e43ee64724c11c25bf6a8
ms.lasthandoff: 04/12/2017


---

# <a name="properties-of-an-azure-active-directory-b2b-collaboration-user"></a>Eigenschaften eines Azure Active Directory B2B-Zusammenarbeitsbenutzers

Ein Azure Active Directory (Azure AD) Business-to-Business (B2B)-Zusammenarbeitsbenutzer ist ein Benutzer mit dem Benutzertyp „Gast“. Dieser Gastbenutzer ist ein der Regel ein Benutzer aus einer Partnerorganisation und verfügt standardmäßig über eingeschränkte Berechtigungen im einladenden Verzeichnis.

Je nach Anforderungen der einladenden Organisation kann ein Azure AD B2B-Zusammenarbeitsbenutzer einen der folgenden Kontozustände aufweisen:

- Zustand 1: Befindet sich in einer externen Instanz von Azure AD und wird als Gastbenutzer in der Hostorganisation dargestellt. In diesem Fall meldet sich der B2B-Benutzer mit einem Azure AD-Konto an, das zu seinem eigenen Mandanten gehört. Wenn die externe Organisation des Benutzers zum Zeitpunkt der Einladung Azure AD nicht verwendet, wird der Gastbenutzer in Azure AD erstellt, nachdem der Benutzer seine Einladung eingelöst hat und seine E-Mail-Adresse überprüft wurde. Ein solcher Mandant wird auch als Just-in-Time (JIT)-Mandant oder viraler Mandant bezeichnet.

- Zustand 2: Befindet sich in einem Microsoft-Konto und wird als Gastbenutzer in der Hostorganisation dargestellt. In diesem Fall meldet sich der Gastbenutzer mit einem Microsoft-Konto an. Die soziale Identität eines eingeladenen Benutzers, bei der es sich nicht um ein Microsoft-Konto handelt (also z.B. google.com), wird während der Einlösung des Angebots als Microsoft-Konto erstellt.

- Zustand 3: Befindet sich im lokalen Active Directory der Hostorganisation und wird mit dem Azure AD der Hostorganisation synchronisiert. In dieser Version müssen Sie die Eigenschaft „UserType“ solcher Benutzer in der Cloud mithilfe von PowerShell manuell ändern.

- Zustand 4: Befindet sich im Azure AD der Hostorganisation mit dem Benutzertyp „Gast“, und die Anmeldeinformationen werden von der Hostorganisation verwaltet.

  ![Anzeigen der Initialen des einladenden Benutzers](media/active-directory-b2b-user-properties/redemption-diagram.png)


Sehen wir uns an, wie ein Azure AD B2B-Zusammenarbeitsbenutzer mit Zustand 1 in Azure AD aussieht.

### <a name="before-invitation-redemption"></a>Vor dem Einlösen der Einladung

![Vor dem Einlösen des Angebots](media/active-directory-b2b-user-properties/before-redemption.png)

### <a name="after-invitation-redemption"></a>Nach dem Einlösen der Einladung

![Nach dem Einlösen des Angebots](media/active-directory-b2b-user-properties/after-redemption.png)

## <a name="key-properties-of-the-azure-ad-b2b-collaboration-user"></a>Wichtige Eigenschaften von Azure AD B2B-Zusammenarbeitsbenutzern
### <a name="usertype"></a>UserType
Diese Eigenschaft gibt die Beziehung des Benutzers zum Hostmandanten an. Dieses Attribut kann zwei Werte aufweisen:
- Mitglied: Dieser Wert weist auf einen Mitarbeiter der Hostorganisation und einen Benutzer auf der Gehaltsliste der Organisation hin. Für diesen Benutzer sollte beispielsweise Zugriff auf rein interne Websites möglich sein. Ein solcher Benutzer wird nicht als externer Projektmitarbeiter betrachtet.

- Gast: Dieser Wert weist auf einen Benutzer hin, der nicht als interner Mitarbeiter des Unternehmens betrachtet wird. Hierbei kann es sich um einen externen Projektmitarbeiter, einen Partner, einen Kunden oder einen ähnlichen Benutzer handeln, der beispielsweise keine internen Mitteilungen des Geschäftsführers erhält oder betriebliche Sozialleistungen bezieht.

  > [!NOTE]
  > Die Eigenschaft „UserType“ steht in keinem Zusammenhang damit, wie sich der Benutzer anmeldet, welche Verzeichnisrolle er hat usw. Die Eigenschaft gibt einfach nur die Beziehung des Benutzers zur Hostorganisation an und ermöglicht der Organisation, Richtlinien basierend auf dieser Eigenschaft zu erzwingen.

### <a name="source"></a>Quelle
Diese Eigenschaft gibt an, wie sich der Benutzer anmeldet.

- Eingeladener Benutzer: Dieser Benutzer wurde eingeladen, hat die Einladung aber noch nicht eingelöst.

- Externes Active Directory: Dieser Benutzer befindet sich in einer externen Organisation und authentifiziert sich mit einem Azure AD-Konto, das zur anderen Organisation gehört. Diese Art der Anmeldung entspricht Zustand 1.

- Microsoft-Konto: Dieser Benutzer befindet sich in einem Microsoft-Konto und authentifiziert sich mit einem Microsoft-Konto. Diese Art der Anmeldung entspricht Zustand 2.

- Windows Server Active Directory: Dieser Benutzer wird aus einem lokalen Active Directory angemeldet, das zu dieser Organisation gehört. Diese Art der Anmeldung entspricht Zustand 3.

- Azure Active Directory: Dieser Benutzer authentifiziert sich mit einem Azure AD-Konto, das zu dieser Organisation gehört. Diese Art der Anmeldung entspricht Zustand 4.
  > [!NOTE]
  > „Source“ und „UserType“ sind unabhängige Eigenschaften. Ein Wert für „Source“ impliziert keinen bestimmten Wert für „UserType“.

## <a name="can-azure-ad-b2b-users-be-added-as-members-instead-of-guests"></a>Können Azure AD B2B-Benutzer als Mitglieder statt als Gäste hinzugefügt werden?
Üblicherweise sind die Begriffe „Azure AD B2B-Benutzer“ und „Gastbenutzer“ synonym. Daher wird ein Azure AD B2B-Zusammenarbeitsbenutzer standardmäßig als Benutzer mit dem Benutzertyp „Gast“ hinzugefügt. In einigen Fällen ist eine Partnerorganisation jedoch Teil einer größeren Organisation, zu der auch die Hostorganisation gehört. Hierbei betrachtet die Hostorganisation die Benutzer der Partnerorganisation eher als Mitglieder, nicht als Gäste. Verwenden Sie in diesen Fällen die Azure AD B2B-Einladungs-Manager-APIs, um einen Benutzer aus der Partnerorganisation als Mitglied zur Hostorganisation einzuladen oder hinzuzufügen.

## <a name="filter-for-guest-users-in-the-directory"></a>Filtern nach Gastbenutzern im Verzeichnis

![Filtern nach Gastbenutzern](media/active-directory-b2b-user-properties/filter-guest-users.png)

## <a name="convert-usertype"></a>Konvertieren des Benutzertyps
Derzeit können Benutzer mithilfe von PowerShell die Eigenschaft „UserType“ von „Mitglied“ in „Gast“ ändern und umgekehrt. Diese Eigenschaft ist jedoch dafür gedacht, die Beziehung eines Benutzers zur Organisation darzustellen. Daher sollte ihr Wert nur dann geändert werden, wenn sich die Beziehung des Benutzers zur Organisation ändert. Wenn sich die Beziehung des Benutzers ändert, sind dann auch andere Punkte zu berücksichtigen, wie etwa ob der Benutzerprinzipalname (UPN) geändert werden muss? Soll der Benutzer weiterhin Zugriff auf die gleichen Ressourcen haben? Muss ein Postfach zugewiesen werden? Es empfiehlt sich daher nicht, die Eigenschaft „UserType“ in PowerShell als alleinige Aktivität zu ändern. Wenn diese Eigenschaft durch die Verwendung von PowerShell unveränderlich wird, empfiehlt es sich nicht, Abhängigkeiten von diesem Wert einzurichten.

## <a name="remove-guest-user-limitations"></a>Aufheben von Einschränkungen für Gastbenutzer
Es gibt eine Vielzahl von Situationen, in denen Sie Ihren Gastbenutzern höhere Berechtigungen einräumen möchten. In einer solchen Situation können Sie Gastbenutzer zu beliebigen Rollen hinzufügen und sogar die standardmäßigen Einschränkungen für Gastbenutzer im Verzeichnis aufheben, um diesen die gleichen Berechtigungen zu gewähren wie Mitgliedern.

Sie können die standardmäßigen Einschränkungen für Gastbenutzer aufheben, sodass Gastbenutzer im Unternehmensverzeichnis die gleichen Verzeichnisberechtigungen erhalten wie normale Benutzer (Mitglieder).

![Aufheben von Einschränkungen für Gastbenutzer](media/active-directory-b2b-user-properties/remove-guest-limitations.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Kollaboration:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hinzufügen eines B2B-Zusammenarbeitsbenutzers zu einer Rolle](active-directory-b2b-add-guest-to-role.md)
* [Delegieren von Einladungen zur B2B-Zusammenarbeit](active-directory-b2b-delegate-invitations.md)
* [B2B collaboration user auditing and reporting](active-directory-b2b-auditing-and-reporting.md) (Überwachung eines B2B-Zusammenarbeitsbenutzers und Berichterstellung)
* [Dynamische Gruppen und B2B-Zusammenarbeit](active-directory-b2b-dynamic-groups.md)
* [B2B-Zusammenarbeit: Code- und PowerShell-Beispiele](active-directory-b2b-code-samples.md)
* [Konfigurieren von SaaS-Apps für die B2B-Zusammenarbeit](active-directory-b2b-configure-saas-apps.md)
* [Benutzertoken für die B2B-Zusammenarbeit](active-directory-b2b-user-token.md)
* [Zuordnen von Benutzeransprüchen für die B2B-Zusammenarbeit](active-directory-b2b-claims-mapping.md)
* [Externe Office 365-Freigabe](active-directory-b2b-o365-external-user.md)
* [Aktuelle Einschränkungen der B2B-Zusammenarbeit](active-directory-b2b-current-limitations.md)

