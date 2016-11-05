---
title: Tokenformat für externe Benutzer bei der Vorschau der Azure Active Directory B2B-Zusammenarbeit | Microsoft Docs
description: Azure Active Directory B2B ermöglicht Geschäftspartnern den gezielten Zugriff auf Ihre Unternehmensanwendungen und unterstützt so Ihre unternehmensübergreifenden Beziehungen.
services: active-directory
documentationcenter: ''
author: viv-liu
manager: cliffdi
editor: ''
tags: ''

ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/09/2016
ms.author: viviali

---
# Vorschau der Azure AD B2B-Zusammenarbeit: Tokenformat für externe Benutzer
Die Ansprüche für ein standardmäßiges Azure AD-Token werden im Artikel [Unterstützte Token- und Anspruchstypen](active-directory-token-and-claims.md) auf azure.microsoft.com beschrieben.

Die Ansprüche, die für einen für die B2B-Zusammenarbeit authentifizierten externen Benutzer anders sind, lauten wie folgt:<br/>

* **OID:** Objekt-ID aus dem Ressourcenmandanten<br/>
* **TID:** Mandanten-ID aus dem Ressourcenmandanten<br/>
* **Aussteller:** Dies ist der Ressourcenmandant.<br/>
* **IDP:** Hierbei handelt es sich um den Home-Mandanten des Benutzers.<br/>
* **AltSecId:** Dies ist die alternative Sicherheits-ID, die für Sie nicht transparent ist.<br/>

## Verwandte Artikel
Weitere Artikel zur Azure AD B2B-Zusammenarbeit:

* [Was ist die Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [So funktioniert's](active-directory-b2b-how-it-works.md)
* [Ausführliche exemplarische Vorgehensweise](active-directory-b2b-detailed-walkthrough.md)
* [Referenz zum CSV-Dateiformat](active-directory-b2b-references-csv-file-format.md)
* [Objektattributänderungen für externe Benutzer](active-directory-b2b-references-external-user-object-attribute-changes.md)
* [Aktuelle Einschränkungen der Vorschau](active-directory-b2b-current-preview-limitations.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)

<!---HONumber=AcomDC_0511_2016-->