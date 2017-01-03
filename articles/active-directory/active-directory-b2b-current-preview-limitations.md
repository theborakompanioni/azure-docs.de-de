---
title: "Aktuelle Einschränkungen der Vorschau für die Azure Active Directory B2B-Zusammenarbeit | Microsoft Docs"
description: "Azure Active Directory B2B ermöglicht Geschäftspartnern den gezielten Zugriff auf Ihre Unternehmensanwendungen und unterstützt so Ihre unternehmensübergreifenden Beziehungen."
services: active-directory
documentationcenter: 
author: viv-liu
manager: cliffdi
editor: 
tags: 
ms.assetid: 1cc8440c-b74b-4789-9401-428105e8a00c
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/09/2016
ms.author: viviali
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 852bc6e8880525bf72e86f3a0e64e63086c0ad04


---
# <a name="azure-ad-b2b-collaboration-preview-current-preview-limitations"></a>Vorschau der Azure AD B2B-Zusammenarbeit: Aktuelle Einschränkungen der Vorschau
* Die Multi-Factor Authentication (MFA) wird für externe Benutzer nicht unterstützt. Beispiel: Wenn Contoso MFA nutzt, Partner Org jedoch nicht, kann den Partner Org-Benutzern die MFA über B2B-Zusammenarbeit nicht ermöglicht werden.
* Einladungen sind nur über CSV möglich. Einzelne Einladungen und API-Zugriff werden nicht unterstützt.
* Nur globale Azure AD-Administratoren können CSV-Dateien hochladen.
* Einladungen an E-Mail-Adressen von Privatbenutzern (wie „hotmail.com“, „Gmail.com“ oder „comcast.net“) werden derzeit nicht unterstützt.
* Der Zugriff externer Benutzer auf lokale Anwendungen wurde nicht getestet.
* Externe Benutzer werden nicht automatisch bereinigt, wenn der jeweilige Benutzer aus dem Verzeichnis gelöscht wird.
* Einladungen an Verteilerlisten werden nicht unterstützt.
* Über CSV können maximal 2.000 Datensätze hochgeladen werden.

## <a name="related-articles"></a>Verwandte Artikel
Weitere Artikel zur Azure AD B2B-Zusammenarbeit:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [So funktioniert's](active-directory-b2b-how-it-works.md)
* [Ausführliche exemplarische Vorgehensweise](active-directory-b2b-detailed-walkthrough.md)
* [Referenz zum CSV-Dateiformat](active-directory-b2b-references-csv-file-format.md)
* [Tokenformat für externe Benutzer](active-directory-b2b-references-external-user-token-format.md)
* [Objektattributänderungen für externe Benutzer](active-directory-b2b-references-external-user-object-attribute-changes.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)




<!--HONumber=Jan17_HO1-->


