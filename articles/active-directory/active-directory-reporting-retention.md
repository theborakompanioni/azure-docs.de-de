---
title: "Aufbewahrungsrichtlinien für Azure Active Directory-Berichte | Microsoft Docs"
description: "Aufbewahrungsrichtlinien für Berichtdaten in Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: aa7a69c933abfda3bf4d1ac1a298c4ba684efd7e
ms.contentlocale: de-de
ms.lasthandoff: 04/06/2017

---
# Aufbewahrungsrichtlinien für Azure Active Directory-Berichte
<a id="azure-active-directory-report-retention-policies" class="xliff"></a>


Dieses Thema enthält Antworten auf die am häufigsten gestellten Fragen im Zusammenhang mit der Datenaufbewahrung für die verschiedenen Aktivitätsberichte in Azure Active Directory. 

**F: Wie wird die Erfassung von Aktivitätsdaten gestartet?**

**A:**

| Azure AD-Edition | Start der Erfassung |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Bei der Registrierung für ein Abonnement |
| Azure AD Free | Beim ersten Öffnen des Blatts [Azure Active Directory](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) oder bei der ersten Verwendung der [Berichterstellungs-APIs](https://aka.ms/aadreports)  |

---
**F: Wann sind die Aktivitätsdaten im Azure-Portal verfügbar?**

**A:**

- **Sofort** – Wenn Sie bereits mit Berichten im klassischen Azure-Portal gearbeitet haben.
- **Innerhalb von 2 Stunden** – Wenn Sie die Berichterstellung im klassischen Azure-Portal nicht aktiviert haben.

---
**F: Wie wird die Erfassung von Sicherheitssignalen gestartet?**  

**A:** Bei Sicherheitssignalen wird der Erfassungsprozess gestartet, wenn Sie sich für die Verwendung von Identity Protection Center entscheiden. 


---
**F: Wie lange werden die gesammelten Daten gespeichert?**

**A:**

**Aktivitätsberichte**    

| Bericht                 | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Verzeichnisprüfbericht        | 7 Tage        | 30 Tage             | 30 Tage             |
| Benutzeranmeldeaktivität       | 7 Tage        | 30 Tage             | 30 Tage             |

**Sicherheitssignale**

| Bericht         | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Gefährdete Benutzer  | 7 Tage        | 30 Tage             | 90 Tage             |
| Riskante Anmeldungen | 7 Tage        | 30 Tage             | 90 Tage             |

---
