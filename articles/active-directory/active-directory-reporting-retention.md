---
title: "Aufbewahrungsrichtlinien für Azure Active Directory-Berichte | Microsoft Docs"
description: "Aufbewahrungsrichtlinien für Berichtdaten in Azure Active Directory"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/30/2016
ms.author: dhanyahk;markvi
translationtype: Human Translation
ms.sourcegitcommit: 47594d1d435ee92d57150a01a99765bab0e87846
ms.openlocfilehash: 2ac62c2c04975167b243b49bf2c536900e77df73


---
# <a name="azure-active-directory-report-retention-policies"></a>Aufbewahrungsrichtlinien für Azure Active Directory-Berichte
*Diese Dokumentation ist Teil des Handbuchs für [Azure Active Directory Reporting](active-directory-reporting-guide.md).*


Dieses Thema enthält Antworten auf die am häufigsten gestellten Fragen im Zusammenhang mit der Datenaufbewahrung für die verschiedenen Aktivitätsberichte in Azure Active Directory. 

Wie wird die Erfassung von Aktivitätsdaten gestartet?

| Azure AD-Edition | Start der Erfassung |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Bei der Registrierung für ein Abonnement |
| Azure AD Free | Beim ersten Öffnen des Blatts [Azure Active Directory](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) oder bei der ersten Verwendung der [Berichterstellungs-APIs](https://aka.ms/aadreports)  |


Wann sind die Aktivitätsdaten im Azure-Portal verfügbar?

- **Sofort** – Wenn Sie bereits mit Berichten im klassischen Azure-Portal gearbeitet haben.
- **Innerhalb von 2 Stunden** – Wenn Sie die Berichterstellung im klassischen Azure-Portal nicht aktiviert haben.

Wie wird die Erfassung von Sicherheitssignalen gestartet?  
Bei Sicherheitssignalen wird der Erfassungsprozess gestartet, wenn Sie sich für die Verwendung von Identity Protection Center entscheiden. 

Wie lange werden die gesammelten Daten gespeichert?

**Aktivitätsberichte**    

| Bericht | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--    | :--           | :--                | :--                |
| Verzeichnisprüfbericht | 7 Tage | 30 Tage | 30 Tage |
| Benutzeranmeldeaktivität |    7 Tage | 30 Tage | 30 Tage |

**Sicherheitssignale**

| Bericht | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--    | :--           | :--                | :--                |
| Riskante Anmeldungen | 7 Tage | 30 Tage | 90 Tage |





<!--HONumber=Feb17_HO1-->


