---
title: "Speicherorte von Aktivitätsberichten im Azure-Portal | Microsoft-Dokumentation"
description: "Erfahren Sie, wo Azure Active Directory-Aktivitätsberichte im Azure-Portal gespeichert werden."
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: d93521f8-dc21-4feb-aaff-4bb300f04812
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/01/2017
ms.author: dhanyahk;markvi
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: f2f5ed98df88df69ee300c9da94ad013a631b3b9
ms.lasthandoff: 03/14/2017


---
# <a name="find-activity-reports-in-the-azure-portal"></a>Speicherorte von Aktivitätsberichten im Azure-Portal

Wenn Sie vom klassischen Azure-Portal zum Azure-Portal wechseln, ändert sich die Anzeige der Aktivitätsprotokolle in Azure Active Directory (Azure AD). In einem kürzlich veröffentlichten [Blogbeitrag](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/) erläutern wir, wie Sie Aktivitätsprotokolle im Kontext der Ressource, mit der Sie im Azure-Portal arbeiten, anzeigen können. In diesem Artikel beschrieben wir, wie Sie die im klassischen Azure-Portal vorhandenen Berichte in das neue Azure-Portal übertragen.

## <a name="whats-new"></a>Neuigkeiten

Die Berichte im klassischen Azure-Portal sind in Kategorien unterteilt:

1.    Sicherheitsberichte
2.    Aktivitätsberichte
3.    Berichte zu integrierten Apps

### <a name="activity-and-integrated-app-reports"></a>Aktivitätsberichte und Berichte zu integrierten Apps

Mit der kontextbasierten Berichterstellung im Azure-Portal werden die vorhandenen Berichte in einer zentralen Ansicht zusammengeführt. Eine einzelne zugrunde liegende API stellt die Daten in der Ansicht bereit.

Sie finden diese Ansicht auf dem Blatt **Azure Active Directory** im Abschnitt **AKTIVITÄT** unter **Überwachungsprotokolle**.

![Überwachungsprotokolle](./media/active-directory-reporting-migration/482.png "Überwachungsprotokolle")

Die folgenden Berichte sind in dieser Ansicht zusammengefasst:

-    Überwachungsbericht
-     Kennwortzurücksetzungsaktivität
-     Aktivität "Registrierung für Zurücksetzen des Kennworts"
-     Self-Service-Gruppenaktivität
-     Namensänderungen für Office&365;-Gruppen
-     Kontobereitstellungsaktivität
-     Status des Kennwortrollovers
-     Kontobereitstellungsfehler


Der Bericht zur Anwendungsnutzung wurde verbessert und ist nun in der Ansicht **Anmeldungen** enthalten. Sie finden diese Ansicht, indem Sie auf dem Blatt **Azure Active Directory** im Abschnitt **AKTIVITÄT** die Option **Anmeldungen** wählen.

![Ansicht „Anmeldungen“](./media/active-directory-reporting-migration/483.png "Ansicht „Anmeldungen“")

Die Ansicht **Anmeldungen** enthält alle Benutzeranmeldungen. Sie können diese Informationen zum Abrufen von Informationen zur Anwendungsnutzung verwenden. Informationen zur Anwendungsnutzung finden Sie zudem in der Übersicht über die **Unternehmensanwendungen** im Abschnitt **VERWALTEN**.

![Unternehmensanwendungen](./media/active-directory-reporting-migration/484.png "Unternehmensanwendungen")

## <a name="access-a-specific-report"></a>Zugreifen auf einen bestimmten Bericht

Obwohl das Azure-Portal eine einzige Ansicht bietet, können Sie auch bestimmte Berichte prüfen.

### <a name="audit-logs"></a>Überwachungsprotokolle

Als Reaktion auf Kundenfeedback Kunden können Sie im Azure-Portal nun über erweiterte Filter auf die gewünschten Daten zugreifen. Ein möglicher Filter ist die *Aktivitätskategorie*. Damit werden die verschiedenen Arten von Aktivitätsprotokollen in Azure AD aufgelistet. Durch Auswahl der gewünschten Kategorie können Sie die Ergebnisse Ihrer Suche eingrenzen.

Wenn Sie beispielsweise nur an Aktivitäten im Zusammenhang mit der Self-Service-Kennwortzurücksetzung interessiert sind, können Sie die Kategorie **Self-Service-Kennwortverwaltung** wählen. Die angezeigten Kategorien basieren auf dem Kontext der Ressource, mit der Sie arbeiten.  

![Kategorieoptionen der Seite mit Filter nach Überwachungsprotokollen](./media/active-directory-reporting-migration/06.png "Kategorieoptionen der Seite mit Filter nach Überwachungsprotokollen")

Zu Aktivitätskategorien zählen:

- Kernverzeichnis
- Self-Service-Kennwortverwaltung
- Self-Service-Gruppenverwaltung
- Kontobereitstellung

### <a name="application-usage"></a>Anwendungsnutzung

Wählen Sie unter **Aktivität** die Option **Anmeldungen**, um die Anwendungsnutzung für alle oder eine einzelne App anzuzeigen. Wenn Sie die Ergebnisse eingrenzen möchten, können Sie anhand des Benutzer- oder Anwendungsnamens filtern.

![Seite mit Filter nach Anmeldeereignissen](./media/active-directory-reporting-migration/07.png "Seite mit Filter nach Anmeldeereignissen")

### <a name="security-reports"></a>Sicherheitsberichte

#### <a name="azure-ad-anomalous-activity-reports"></a>Azure AD-Berichte zu anomalen Aktivitäten

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


#### <a name="detected-risk-events"></a>Erkannte Risikoereignisse

Sie können im Azure-Portal auf dem Blatt **Azure Active Directory** unter **SICHERHEIT** auf die Berichte zu erkannten Risikoereignissen zugreifen. Die erkannten Risikoereignisse werden in den folgenden Berichten nachverfolgt:   

- Gefährdete Benutzer
- Riskante Anmeldungen

![Sicherheitsberichte](./media/active-directory-reporting-migration/04.png "Sicherheitsberichte")

Weitere Informationen zu Sicherheitsberichten finden Sie unter:

- [Sicherheitsbericht „Gefährdete Benutzer“ im Azure Active Directory-Portal – Vorschau](active-directory-reporting-security-user-at-risk.md)
- [Bericht „Riskante Anmeldungen“ im Azure Active Directory-Portal – Vorschau](active-directory-reporting-security-risky-sign-ins.md)


## <a name="activity-reports-in-the-azure-classic-portal-vs-the-azure-portal"></a>Aktivitätsberichte im klassischen Azure-Portal im Vergleich zum Azure-Portal

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
| Namensänderungen für Office&365;-Gruppen         | Wählen Sie als **Aktivitätskategorie** die Option **Self-Service-Kennwortverwaltung**. Wählen Sie **Aktivitätsressourcentyp** die Option **Gruppe**. Wählen Sie als **Aktivitätsquelle** die Option **Office&365;-Gruppen**.|

Wählen Sie zum Anzeigen des Berichts zur **Anwendungsnutzung** auf dem Blatt **Azure Active Directory** unter **VERWALTEN** die **Unternehmensanwendungen**, und wählen Sie dann **Anmeldevorgänge**.


![Bericht zu Anmeldungen bei Unternehmensanwendungen](./media/active-directory-reporting-migration/199.png "Bericht zu Anmeldungen bei Unternehmensanwendungen")

