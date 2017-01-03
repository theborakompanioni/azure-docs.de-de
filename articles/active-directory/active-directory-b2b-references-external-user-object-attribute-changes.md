---
title: "Objektattributänderungen für externe Benutzer bei der Vorschau der Azure Active Directory B2B-Zusammenarbeit | Microsoft Docs"
description: "Azure Active Directory B2B ermöglicht Geschäftspartnern den gezielten Zugriff auf Ihre Unternehmensanwendungen und unterstützt so Ihre unternehmensübergreifenden Beziehungen."
services: active-directory
documentationcenter: 
author: viv-liu
manager: cliffdi
editor: 
tags: 
ms.assetid: 4ede9c7b-3830-42f2-b73d-cbecbcc7db64
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/09/2016
ms.author: viviali
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: d57fd7d6dcd186d1e79528c716d8bf81ad868ae5


---
# <a name="azure-ad-b2b-collaboration-preview-external-user-object-attribute-changes"></a>Vorschau der Azure AD B2B-Zusammenarbeit: Objektattributänderungen für externe Benutzer
Jeder Benutzer in  einem Azure AD-Verzeichnis wird durch ein Benutzerobjekt repräsentiert. Das Benutzerobjekt in Azure AD unterliegt Attributänderungen während der verschiedenen Phasen von der Einladung zur B2B-Zusammenarbeit bis zur Einlösung dieser Einladung. Das Benutzerobjekt, das im Verzeichnis den Partnerbenutzer repräsentiert, besitzt Attribute, die sich zur Einlösungszeit ändern können, wenn der Partnerbenutzer auf den Link in der Einladungs-E-Mail klickt. Dies bedeutet Folgendes:

* Die Attribute **SignInName** und **AltSecId** werden aufgefüllt
* Das Attribut **DisplayName** ändert sich vom Benutzerprinzipalnamen (user_fabrikam.com#EXT#@contoso.com) zum Anmeldenamen (user@fabrikam.com)

Wenn Sie diese Attribute in Azure AD verfolgen, können Sie bei der Problembehandlung feststellen, ob ein Partner seine Einladung zur B2B-Zusammenarbeit eingelöst hat oder nicht.

## <a name="related-articles"></a>Verwandte Artikel
Weitere Artikel zur Azure AD B2B-Zusammenarbeit:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [So funktioniert's](active-directory-b2b-how-it-works.md)
* [Ausführliche exemplarische Vorgehensweise](active-directory-b2b-detailed-walkthrough.md)
* [Referenz zum CSV-Dateiformat](active-directory-b2b-references-csv-file-format.md)
* [Tokenformat für externe Benutzer](active-directory-b2b-references-external-user-token-format.md)
* [Aktuelle Einschränkungen der Vorschau](active-directory-b2b-current-preview-limitations.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)




<!--HONumber=Dec16_HO4-->


