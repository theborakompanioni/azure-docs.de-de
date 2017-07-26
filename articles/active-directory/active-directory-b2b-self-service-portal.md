---
title: "Self-Service-Anmeldungsportal für Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation"
description: "Die Azure Active Directory B2B-Zusammenarbeit ermöglicht Geschäftspartnern den gezielten Zugriff auf Ihre Unternehmensanwendungen und unterstützt so Ihre unternehmensübergreifenden Beziehungen."
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
ms.date: 05/24/2017
ms.author: sasubram
ms.translationtype: Human Translation
ms.sourcegitcommit: ef74361c7a15b0eb7dad1f6ee03f8df707a7c05e
ms.openlocfilehash: 307373c75bbb87cec683f7a3097f8f159c9d5e61
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017


---


# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Self-Service-Anmeldungsportal für Azure AD B2B-Zusammenarbeit

Kunden können mit den integrierten Features, die über das [Azure-Portal](https://portal.azure.com) für IT-Administratoren und den [Anwendungszugriffsbereich](https://myapps.microsoft.com) für Endbenutzer verfügbar gemacht werden, viele Aufgaben erledigen. Uns ist aber auch bewusst, dass Unternehmen den Einstellungsworkflow für B2B-Benutzer an die Bedürfnisse ihrer Organisation anpassen müssen. Sie können dies mit [unserer API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation) vornehmen.

Bei der Diskussion hierüber mit vielen unserer Kunden ist aufgefallen, dass eine Anforderung vor allen anderen genannt wird. Dabei geht es um den Fall, wenn die einladende Organisation die externen Mitarbeiter, die Zugriff auf ihre Ressourcen benötigen, möglicherweise nicht im Voraus kennt. Die Organisationen wünschen sich eine Möglichkeit, Benutzern von Partnerunternehmen die selbstständige Registrierung gemäß einigen Richtlinien der einladenden Organisation zu erlauben. Dieses Szenario ist über unsere APIs möglich. Deshalb haben wir genau dafür ein Projekt auf GitHub veröffentlicht: [GitHub-Beispielprojekt](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

Unser GitHub-Projekt zeigt, wie Organisationen unsere APIs verwenden können, um eine richtlinienbasierte Self-Service-Registrierungsfunktion für ihre vertrauenswürdigen Partner bereitzustellen, die Regeln für die zugelassenen Apps enthält. Partnerbenutzer erhalten so sicheren Zugriff auf die richtigen Ressourcen, wenn sie diesen benötigen, ohne dass eine Person in der einladenden Organisation sie manuell integrieren muss. Sie können das Projekt ganz einfach in einem Azure-Abonnement Ihrer Wahl bereitstellen.

## <a name="as-is-code"></a>Unveränderter Code

Denken Sie daran, dass dieser Code als ein Beispiel zur Veranschaulichung der Verwendung der Einladungs-API von Azure Active Directory B2B verfügbar gemacht wird. Er sollte von Ihrem Entwicklungsteam oder einem Partner angepasst und überprüft werden, bevor er in einem Produktionsszenario bereitgestellt wird.

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Kollaboration:
* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Wie fügen Azure Active Directory-Administratoren B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-admin-add-users.md)
* [Wie fügen Information-Worker B2B-Zusammenarbeitsbenutzer hinzu?](active-directory-b2b-iw-add-users.md)
* [Die Elemente der Einladungs-E-Mail von B2B-Zusammenarbeit](active-directory-b2b-invitation-email.md)
* [B2B-Zusammenarbeit: Einlösen von Einladungen](active-directory-b2b-redemption-experience.md)
* [Lizenzierung von Azure AD B2B-Zusammenarbeit](active-directory-b2b-licensing.md)
* [Problembehandlung für die Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-troubleshooting.md)
* [Häufig gestellte Fragen zur Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-faq.md)
* [Multi-Factor Authentication für Benutzer der B2B-Zusammenarbeit](active-directory-b2b-mfa-instructions.md)
* [Hinzufügen von Benutzern der B2B-Zusammenarbeit ohne Einladung](active-directory-b2b-add-user-without-invite.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
