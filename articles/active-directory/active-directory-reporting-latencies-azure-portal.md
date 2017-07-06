---
title: Latenzen bei Azure Active Directory-Berichten | Microsoft-Dokumentation
description: "Erfahren Sie etwas über den erforderliche Zeitraum, bis Ereignisse in Ihrem Azure-Portal angezeigt werden."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: markvi;dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 1c6b79c5f67cee5d62c9879bdeec926091253af6
ms.contentlocale: de-de
ms.lasthandoff: 04/12/2017


---
# <a name="azure-active-directory-reporting-latencies---preview"></a>Latenzen bei Azure Active Directory-Berichten – Vorschau

Mit der Berichterstellungsfunktion in Azure Active Directory [(Vorschau)](active-directory-preview-explainer.md) erhalten Sie alle Informationen, die Sie zum Ermitteln des Zustands Ihrer Umgebung benötigen. Die Dauer bis zur Anzeige von Berichtsdaten im Azure-Portal wird auch als Latenz bezeichnet. 

Dieses Thema enthält Informationen zur Latenz für alle Berichtskategorien im Azure-Portal. 


## <a name="activity-reports"></a>Aktivitätsberichte

Es gibt zwei Bereiche von Aktivitätsberichten:

- **Anmeldeaktivitäten** : Informationen zur Nutzung von verwalteten Anwendungen und Aktivitäten der Benutzeranmeldung
- **Überwachungsprotokolle** : Systemaktivitätsinformationen zu Benutzern und zur Gruppenverwaltung, zu verwalteten Anwendungen und Verzeichnisaktivitäten

Die folgende Tabelle enthält Latenzzeitinformationen für Aktivitätsberichte.

| Bericht | Minimum | Durchschnitt | Maximum |
| :-- | --- | --- | --- |
| Überwachungsprotokolle             | 30 Minuten  | 45 Minuten | 1 Stunde     |
| Anmeldungen               | 15 Minuten  | 15 Minuten | 2 Stunden*   |

>[!NOTE]
> Bei einigen Aktivitätsdaten zu Anmeldungen von älteren Office-Anwendungen kann es bis zu 8 Stunden dauern, bis die Berichtsdaten angezeigt werden. 


## <a name="security-reports"></a>Sicherheitsberichte

Es gibt zwei Bereiche von Sicherheitsberichten:

- **Riskante Anmeldungen:** Eine riskante Anmeldung ist ein Indikator für einen Anmeldeversuch von einem Benutzer, der nicht der rechtmäßige Besitzer eines Benutzerkontos ist. 
- **Benutzer mit Risikomarkierung:** Ein Benutzer mit Risikomarkierung ist ein Indikator für ein möglicherweise kompromittiertes Benutzerkonto. 

Die folgende Tabelle enthält Latenzzeitinformationen für Sicherheitsberichte.

| Bericht | Minimum | Durchschnitt | Maximum |
| :-- | --- | --- | --- |
| Gefährdete Benutzer          | 5 Minuten   | 15 Minuten  | 2 Stunden  |
| Riskante Anmeldungen         | 5 Minuten   | 15 Minuten  | 2 Stunden  |

## <a name="risk-events"></a>Risikoereignisse

Azure Active Directory verwendet adaptive Machine Learning-Algorithmen und -Heuristiken, um verdächtige Aktivitäten im Zusammenhang mit Ihren Benutzerkonten zu erkennen. Jede erkannte verdächtige Aktion wird in einem Datensatz gespeichert, der als Risikoereignis bezeichnet wird.

Die folgende Tabelle enthält Latenzzeitinformationen für Risikoereignisse.

| Bericht | Minimum | Durchschnitt | Maximum |
| :-- | --- | --- | --- |
| Anmeldungen von anonymen IP-Adressen |5 Minuten |15 Minuten |2 Stunden |
| Anmeldungen von unbekannten Standorten |5 Minuten |15 Minuten |2 Stunden |
| Benutzer mit kompromittierten Anmeldeinformationen |2 Stunden |4 Stunden |8 Stunden |
| Unmöglicher Ortswechsel zu atypischen Orten |2 Stunden |4 Stunden |8 Stunden  |
| Anmeldungen von infizierten Geräten |2 Stunden |4 Stunden |8 Stunden  |
| Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten |2 Stunden |4 Stunden |8 Stunden  |



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Aktivitätsberichten im Azure-Portal finden Sie unter:

- [Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal](active-directory-reporting-activity-sign-ins.md)
- [Berichte zu Überwachungsaktivitäten im Azure Active Directory-Portal](active-directory-reporting-activity-audit-logs.md)

Weitere Informationen zu Sicherheitsberichten im Azure-Portal finden Sie unter:

- [Sicherheitsbericht „Gefährdete Benutzer“ im Azure Active Directory-Portal](active-directory-reporting-security-user-at-risk.md)
- [Bericht „Riskante Anmeldungen“ im Azure Active Directory-Portal](active-directory-reporting-security-risky-sign-ins.md)

Weitere Informationen zu Risikoereignissen finden Sie unter [Azure Active Directory-Risikoereignisse](active-directory-reporting-risk-events.md).
