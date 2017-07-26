---
title: Externe Office 365-Freigaben und Azure Active Directory B2B-Zusammenarbeit | Microsoft-Dokumentation
description: "Referenz für die Zuordnung von Ansprüchen für die Azure Active Directory B2B-Zusammenarbeit"
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
ms.openlocfilehash: cad0ce8f745f3d6ca14436fd714b08c60de0e459
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017


---

# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a>Externe Office 365-Freigaben und Azure Active Directory B2B-Zusammenarbeit

Externe Office 365-Freigaben (OneDrive, SharePoint Online, einheitliche Gruppen usw.) und die Verwendung der Azure AD B2B-Zusammenarbeit sind technisch gesehen das Gleiche. Bei allen externen Freigabetools (außer OneDrive/SharePoint Online), einschließlich Gästen in Office 365-Gruppen, werden bereits die Einladungs-APIs der Azure AD B2B-Zusammenarbeit für die Freigabe verwendet.

OneDrive/SharePoint Online hat einen separaten Einladung-Manager. Unterstützung für die externe Freigabe in OneDrive/SharePoint Online begann, bevor die Unterstützung durch Azure AD entwickelt wurde. Im Lauf der Zeit wurde die externe OneDrive/SharePoint Online-Freigabe um eine Vielzahl von Features erweitert, und viele Millionen Benutzer verwenden das integrierte Freigabemuster des Produkts. Es gibt jedoch einige feine Unterschiede zwischen der Funktionsweise der externen OneDrive-/SharePoint Online-Freigaben und der Azure AD B2B-Zusammenarbeit:

- OneDrive/SharePoint Online fügt Benutzer dem Verzeichnis hinzu, nachdem sie ihre Einladungen eingelöst haben. Deshalb werden die Benutzer vor dem Einlösen nicht im Azure AD-Portal angezeigt. Wenn der Benutzer in der Zwischenzeit von einer anderen Website eingeladen wird, wird eine neue Einladung generiert. Wenn Sie jedoch Azure AD B2B-Zusammenarbeit verwenden, werden Benutzer sofort bei Einladung hinzugefügt, sodass sie überall angezeigt werden.

- Die Einlösungserfahrung in OneDrive/SharePoint Online unterscheidet sich von der Erfahrung in Azure AD B2B-Zusammenarbeit. Nachdem ein Benutzer eine Einladung eingelöst hat, sind die Erfahrungen gleich.

- In Azure AD B2B-Zusammenarbeit eingeladene Benutzer können in Freigabedialogfeldern von OneDrive/SharePoint Online ausgewählt werden. Benutzer, die über OneDrive/SharePoint Online eingeladen wurden, werden nach dem Einlösen der Einladung auch in Azure AD angezeigt.

- Um externe Freigaben in OneDrive/SharePoint Online mit der Azure AD B2B-Zusammenarbeit zu verwalten, legen Sie die Einstellung für externe OneDrive-/SharePoint Online-Freigaben auf **Freigabe nur für externe Benutzer zulassen, die sich bereits im Verzeichnis befinden** fest. Benutzer können zu extern freigegebenen Websites wechseln und aus externen Projektmitarbeitern auswählen, die der Administrator hinzugefügt hat. Ein Administrator kann die externen Projektmitarbeiter über die Einladungs-APIs der B2B-Zusammenarbeit hinzufügen.

![Die Einstellung der externen OneDrive/SharePoint Online-Freigabe](media/active-directory-b2b-o365-external-user/odsp-sharing-setting.png)

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
* [Aktuelle Einschränkungen der B2B-Zusammenarbeit](active-directory-b2b-current-limitations.md)

