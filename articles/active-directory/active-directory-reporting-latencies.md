---
title: "Verzögerungen bei Azure Active Directory-Berichten | Microsoft Docs"
description: Der erforderliche Zeitraum, bis Ereignisse in Ihrem Azure Active Directory-Bericht angezeigt werden.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 346b14f8-d16d-4b07-8211-e6c5eec07062
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.custom: oldportal
ms.reviewer: dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 9443a00232420d58dea52ed01f31a4ef964a1620
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017

---
# Wartezeiten von Azure Active Directory-Berichten
<a id="azure-active-directory-report-latencies" class="xliff"></a>
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
| Alle Benutzeranmeldeaktivitäten |2 Stunden |4 Stunden |8 Stunden |
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


