---
title: "Verzögerungen bei Azure Active Directory-Berichten | Microsoft Docs"
description: Der erforderliche Zeitraum, bis Ereignisse in Ihrem Azure Active Directory-Bericht angezeigt werden.
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 346b14f8-d16d-4b07-8211-e6c5eec07062
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/07/2016
ms.author: dhanyahk
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e15c82330ac27f58f3faec3a224123cb45c9b28b


---
# <a name="azure-active-directory-report-latencies"></a>Wartezeiten von Azure Active Directory-Berichten
*Diese Dokumentation ist Teil des Handbuchs für [Azure Active Directory Reporting](active-directory-reporting-guide.md).*

| Bericht | Minimum | Durchschnitt | Maximum |
| --- | --- | --- | --- |
| **Sicherheitsberichte** | | | |
| Irreguläre Anmeldeaktivitäten |2 Stunden |4 Stunden |8 Stunden |
| Anmeldungen von unbekannten Quellen |2 Stunden |4 Stunden |8 Stunden |
| Anmeldungen nach mehreren Fehlern |2 Stunden |4 Stunden |8 Stunden |
| Anmeldungen aus mehreren geografischen Regionen |2 Stunden |4 Stunden |8 Stunden |
| Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten |2 Stunden |4 Stunden |8 Stunden |
| Anmeldungen von möglicherweise infizierten Geräten |2 Stunden |4 Stunden |8 Stunden |
| Benutzer mit anomalen Anmeldeaktivitäten |2 Stunden |4 Stunden |8 Stunden |
| Benutzer mit kompromittierten Anmeldeinformationen |2 Stunden |4 Stunden |8 Stunden |
| **Anwendungsberichte** | | | |
| Kontobereitstellungsaktivität |2 Stunden |4 Stunden |8 Stunden |
| Kontobereitstellungsfehler |2 Stunden |4 Stunden |8 Stunden |
| Anwendungsnutzung |2 Stunden |4 Stunden |8 Stunden |
| Status des Kennwortrollovers |2 Stunden |4 Stunden |8 Stunden |
| **Überwachungs- und Aktivitätsberichte** | | | |
| Überwachungsbericht |1 Minute |15 Minuten |30 Minuten |
| Aktivität "Zurücksetzen des Kennworts" (Azure AD) |2 Stunden |4 Stunden |8 Stunden |
| Aktivität "Zurücksetzen des Kennworts" (Identity Manager) |2 Stunden |4 Stunden |8 Stunden |
| Aktivität "Registrierung für Zurücksetzen des Kennworts" (Azure AD) |2 Stunden |4 Stunden |8 Stunden |
| Aktivität "Registrierung für Zurücksetzen des Kennworts" (Identity Manager) |2 Stunden |4 Stunden |8 Stunden |
| Self-Service-Gruppenaktivität (Azure AD) |2 Stunden |4 Stunden |8 Stunden |
| Self-Service-Gruppenaktivität (Identity Manager) |2 Stunden |4 Stunden |8 Stunden |
| **RMS-Berichte** | | | |
| Aktivste RMS-Benutzer |2 Stunden |4 Stunden |8 Stunden |
| RMS-Nutzung |2 Stunden |4 Stunden |8 Stunden |
| Nutzung von RMS-Geräten |2 Stunden |4 Stunden |8 Stunden |
| Nutzung RMS-fähiger Anwendungen |2 Stunden |4 Stunden |8 Stunden |
| **Private Berichtsvorschau** | | | |
| Alle Benutzeranmeldeaktivitäten |2 Stunden |4 Stunden |8 Stunden |




<!--HONumber=Nov16_HO3-->


