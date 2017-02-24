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
ms.date: 02/06/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: d3f68340592d9032999ecb5cc16ac1fedcce4c33
ms.openlocfilehash: 716d19be09085db91c17d7ed76422e05d164d137


---

# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a>Externe Office 365-Freigaben und Azure Active Directory B2B-Zusammenarbeit

Externe Office&365;-Freigaben (OneDrive, SharePoint Online, einheitliche Gruppen usw.) und die Verwendung der Azure AD B2B-Zusammenarbeit sind technisch gesehen das Gleiche. Bei allen externen Freigabetools (außer OneDrive/SharePoint Online), einschließlich **Gästen in einheitlichen Gruppen**, wird bereits die Einladungs-API der Azure AD B2B-Zusammenarbeit für die Freigabe verwendet.

OneDrive/SharePoint Online (ODSP) verfügt über einen separaten Einladungs-Manager, da die ODSP-Unterstützung für externe Freigaben bereits existierte, bevor diese im Rahmen des Azure AD-Fabrics inhärent unterstützt wurden. Im Lauf der Zeit wurde die externe ODSP-Freigabe um eine Vielzahl von Funktionen erweitert, und viele Millionen Benutzer verwenden das integrierte Freigabemuster des Produkts. Wir arbeiten daran, ODSP in die Azure AD B2B-Einladungs-APIs (um die es in dieser Dokumentation geht) zu integrieren, um alle Vorteile zu kombinieren und um alle Innovationen von Azure AD zu ergänzen. Bis es soweit ist, gibt es einige feine Unterschiede zwischen der Funktionsweise der externen ODSP-Freigaben und der Azure AD B2B-Zusammenarbeit:

1. Nachdem ein Benutzer seine Einladung eingelöst hat, fügt ODSP diesen Benutzer zum Verzeichnis hinzu. Der Benutzer wird also erst im Azure AD-Portal angezeigt, wenn die Einlösung erfolgt ist. Wenn der Benutzer in der Zwischenzeit von einer anderen Website eingeladen wird, wird eine neue Einladung generiert. Bei der B2B-Zusammenarbeit dagegen wird ein Benutzer sofort bei der Einladung hinzugefügt und daher an allen relevanten Stellen angezeigt.

2. Die Einlösung funktioniert in ODSP etwas anders als bei der B2B-Zusammenarbeit.

3. Nachdem ein Benutzer seine Einladung eingelöst hat, ist das Prozedere gleich.

4. Benutzer, die im Rahmen der B2B-Zusammenarbeit eingeladen wurden, können aus Dialogfeldern von ODSP-Freigaben ausgewählt werden. Benutzer, die über ODSP eingeladen wurden, werden nach dem Einlösen der Einladung auch in Azure AD angezeigt.

5. Um externe Freigaben in ODSP gemeinsam mit der B2B-Zusammenarbeit zu verwenden, legen Sie die Einstellung für externe ODSP-Freigaben auf **Freigabe nur für externe Benutzer zulassen, die sich bereits im Verzeichnis befinden** fest. Benutzer können zu extern freigegebenen Websites wechseln und aus externen Projektmitarbeitern auswählen, die ein Administrator hinzugefügt hat. Ein Administrator kann die externen Projektmitarbeiter über die Einladungs-APIs der B2B-Zusammenarbeit hinzufügen.

![Einstellung für die externe ODSP-Freigabe](media/active-directory-b2b-o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Zusammenarbeit:

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



<!--HONumber=Feb17_HO1-->


