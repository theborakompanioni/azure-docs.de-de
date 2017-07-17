---
title: "Speicherorte von Aktivitätsberichten im Azure-Portal | Microsoft-Dokumentation"
description: "Erfahren Sie, wo Azure Active Directory-Aktivitätsberichte im Azure-Portal gespeichert werden."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: d93521f8-dc21-4feb-aaff-4bb300f04812
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 7f6d82d211e9b3d25b0efe62ab8dd32f827b08ef
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017

---
# Speicherorte von Aktivitätsberichten im Azure-Portal
<a id="find-activity-reports-in-the-azure-portal" class="xliff"></a>

Wenn Sie vom klassischen Azure-Portal zum Azure-Portal wechseln, ändert sich die Anzeige der Aktivitätsprotokolle in Azure Active Directory (Azure AD). In einem kürzlich veröffentlichten [Blogbeitrag](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/) erläutern wir, wie Sie Aktivitätsprotokolle im Kontext der Ressource, mit der Sie im Azure-Portal arbeiten, anzeigen können. In diesem Artikel beschrieben wir, wie Sie die im klassischen Azure-Portal vorhandenen Berichte in das neue Azure-Portal übertragen.

## Neuigkeiten
<a id="whats-new" class="xliff"></a>

Die Berichte im klassischen Azure-Portal sind in Kategorien unterteilt:

1.  Sicherheitsberichte
2.  Aktivitätsberichte
3.  Berichte zu integrierten Apps

### Aktivitätsberichte und Berichte zu integrierten Apps
<a id="activity-and-integrated-app-reports" class="xliff"></a>

Mit der kontextbasierten Berichterstellung im Azure-Portal werden die vorhandenen Berichte in einer zentralen Ansicht zusammengeführt. Eine einzelne zugrunde liegende API stellt die Daten in der Ansicht bereit.

Sie finden diese Ansicht auf dem Blatt **Azure Active Directory** im Abschnitt **AKTIVITÄT** unter **Überwachungsprotokolle**.

![Überwachungsprotokolle](./media/active-directory-reporting-migration/482.png "Überwachungsprotokolle")

Die folgenden Berichte sind in dieser Ansicht zusammengefasst:

-   Überwachungsbericht
-   Kennwortzurücksetzungsaktivität
-   Aktivität "Registrierung für Zurücksetzen des Kennworts"
-   Self-Service-Gruppenaktivität
-   Namensänderungen für Office 365-Gruppen
-   Kontobereitstellungsaktivität
-   Status des Kennwortrollovers
-   Kontobereitstellungsfehler


Der Bericht zur Anwendungsnutzung wurde verbessert und ist nun in der Ansicht **Anmeldungen** enthalten. Sie finden diese Ansicht, indem Sie auf dem Blatt **Azure Active Directory** im Abschnitt **AKTIVITÄT** die Option **Anmeldungen** wählen.

![Ansicht „Anmeldungen“](./media/active-directory-reporting-migration/483.png "Ansicht „Anmeldungen“")

Die Ansicht **Anmeldungen** enthält alle Benutzeranmeldungen. Sie können diese Informationen zum Abrufen von Informationen zur Anwendungsnutzung verwenden. Informationen zur Anwendungsnutzung finden Sie zudem in der Übersicht über die **Unternehmensanwendungen** im Abschnitt **VERWALTEN**.

![Unternehmensanwendungen](./media/active-directory-reporting-migration/484.png "Unternehmensanwendungen")

## Zugreifen auf einen bestimmten Bericht
<a id="access-a-specific-report" class="xliff"></a>

Obwohl das Azure-Portal eine einzige Ansicht bietet, können Sie auch bestimmte Berichte prüfen.

### Überwachungsprotokolle
<a id="audit-logs" class="xliff"></a>

Als Reaktion auf Kundenfeedback Kunden können Sie im Azure-Portal nun über erweiterte Filter auf die gewünschten Daten zugreifen. Ein möglicher Filter ist die *Aktivitätskategorie*. Damit werden die verschiedenen Arten von Aktivitätsprotokollen in Azure AD aufgelistet. Durch Auswahl der gewünschten Kategorie können Sie die Ergebnisse Ihrer Suche eingrenzen.

Wenn Sie beispielsweise nur an Aktivitäten im Zusammenhang mit der Self-Service-Kennwortzurücksetzung interessiert sind, können Sie die Kategorie **Self-Service-Kennwortverwaltung** wählen. Die angezeigten Kategorien basieren auf dem Kontext der Ressource, mit der Sie arbeiten.  

![Kategorieoptionen der Seite mit Filter nach Überwachungsprotokollen](./media/active-directory-reporting-migration/06.png "Kategorieoptionen der Seite mit Filter nach Überwachungsprotokollen")

Zu Aktivitätskategorien zählen:

- Kernverzeichnis
- Self-Service-Kennwortverwaltung
- Self-Service-Gruppenverwaltung
- Kontobereitstellung

### Anwendungsnutzung
<a id="application-usage" class="xliff"></a>

Wählen Sie unter **Aktivität** die Option **Anmeldungen**, um die Anwendungsnutzung für alle oder eine einzelne App anzuzeigen. Wenn Sie die Ergebnisse eingrenzen möchten, können Sie anhand des Benutzer- oder Anwendungsnamens filtern.

![Seite mit Filter nach Anmeldeereignissen](./media/active-directory-reporting-migration/07.png "Seite mit Filter nach Anmeldeereignissen")

### Sicherheitsberichte
<a id="security-reports" class="xliff"></a>

#### Azure AD-Berichte zu anomalen Aktivitäten
<a id="azure-ad-anomalous-activity-reports" class="xliff"></a>

Die Sicherheitsberichte zur anormalen Aktivität von Azure AD im klassischen Azure-Portal wurden zusammengestellt, um Ihnen einen zentralen Blick auf alle sicherheitsrelevanten Risikoereignisse zu gewähren, die Azure AD ermitteln und melden kann.

Die folgende Tabelle listet die Sicherheitsberichte zur anormalen Aktivität von Azure AD und die entsprechenden Risikoereignistypen im Azure-Portal auf.

| Bericht zur anormalen Aktivität von Azure AD |  Typ des Identity Protection-Risikoereignisses|
| :--- | :--- |
| Benutzer mit kompromittierten Anmeldeinformationen | Kompromittierte Anmeldeinformationen |
| Irreguläre Anmeldeaktivitäten | Unmöglicher Ortswechsel zu atypischen Orten |
| Anmeldungen von möglicherweise infizierten Geräten | Anmeldungen von infizierten Geräten|
| Anmeldungen von unbekannten Quellen | Anmeldungen von anonymen IP-Adressen |
| Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten | Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten |
| - | Anmeldungen von unbekannten Standorten |

Folgende Sicherheitsberichte zur anormalen Aktivität von Azure AD gehören nicht zu Risikoereignissen im Azure-Portal:

* Anmeldungen nach mehreren Fehlern
* Anmeldungen aus mehreren geografischen Regionen

Diese Berichte sind im klassischen Azure-Portal weiterhin verfügbar, werden allerdings zu einem zukünftigen Zeitpunkt als veraltet markiert.

Weitere Informationen finden Sie unter [Azure Active Directory-Risikoereignisse](active-directory-identity-protection-risk-events.md).  


#### Erkannte Risikoereignisse
<a id="detected-risk-events" class="xliff"></a>

Sie können im Azure-Portal auf dem Blatt **Azure Active Directory** unter **SICHERHEIT** auf die Berichte zu erkannten Risikoereignissen zugreifen. Die erkannten Risikoereignisse werden in den folgenden Berichten nachverfolgt:   

- Gefährdete Benutzer
- Riskante Anmeldungen

![Sicherheitsberichte](./media/active-directory-reporting-migration/04.png "Sicherheitsberichte")

Weitere Informationen zu Sicherheitsberichten finden Sie unter:

- [Sicherheitsbericht „Gefährdete Benutzer“ im Azure Active Directory-Portal](active-directory-reporting-security-user-at-risk.md)
- [Bericht „Riskante Anmeldungen“ im Azure Active Directory-Portal](active-directory-reporting-security-risky-sign-ins.md)


## Aktivitätsberichte im klassischen Azure-Portal im Vergleich zum Azure-Portal
<a id="activity-reports-in-the-azure-classic-portal-vs-the-azure-portal" class="xliff"></a>

Die Tabelle in diesem Abschnitt enthält vorhandene Berichte im klassischen Azure-Portal. Außerdem wird erläutert, wie Sie diese Informationen im Azure-Portal abrufen können.

Rufen Sie zum Anzeigen aller Überwachungsdaten auf dem Blatt **Azure Active Directory** unter **AKTIVITÄT** die **Überwachungsprotokolle** auf.

![Überwachungsprotokolle](./media/active-directory-reporting-migration/61.png "Überwachungsprotokolle")

| Klassisches Azure-Portal                 | Im Azure-Portal zu finden                                                         |
| ---                                  | ---                                                                        |
| Überwachungsprotokolle                           | Wählen Sie als **Aktivitätskategorie** die Option **Kernverzeichnis** aus.                       |
| Kennwortzurücksetzungsaktivität              | Wählen Sie als **Aktivitätskategorie** die Option **Self-Service-Kennwortverwaltung**. |
| Aktivität "Registrierung für Zurücksetzen des Kennworts" | Wählen Sie als **Aktivitätskategorie** die Option **Self-Service-Kennwortverwaltung**.     |
| Self-Service-Gruppenaktivität         | Wählen Sie als **Aktivitätskategorie** die Option **Self-Service-Gruppenverwaltung** aus.        |
| Kontobereitstellungsaktivität        | Wählen Sie als **Aktivitätskategorie** die Option **Benutzerkontenbereitstellung** aus.         |
| Status des Kennwortrollovers             | Wählen Sie als **Aktivitätskategorie** die Option **Automatischer Rollover von App-Kennwörtern** aus.      |
| Kontobereitstellungsfehler          | Wählen Sie als **Aktivitätskategorie** die Option **Benutzerkontenbereitstellung** aus.        |
| Namensänderungen für Office 365-Gruppen         | Wählen Sie als **Aktivitätskategorie** die Option **Self-Service-Kennwortverwaltung**. Wählen Sie **Aktivitätsressourcentyp** die Option **Gruppe**. Wählen Sie als **Aktivitätsquelle** die Option **Office 365-Gruppen**.|

Wählen Sie zum Anzeigen des Berichts zur **Anwendungsnutzung** auf dem Blatt **Azure Active Directory** unter **VERWALTEN** die **Unternehmensanwendungen**, und wählen Sie dann **Anmeldevorgänge**.


![Bericht zu Anmeldungen bei Unternehmensanwendungen](./media/active-directory-reporting-migration/199.png "Bericht zu Anmeldungen bei Unternehmensanwendungen")

