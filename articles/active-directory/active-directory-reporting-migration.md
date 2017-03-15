---
title: "Speicherorte von Aktivitätsberichten im Azure-Portal | Microsoft-Dokumentation"
description: "Erfahren Sie, wo Aktivitätsberichte im Azure-Portal gespeichert werden"
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
ms.date: 02/17/2017
ms.author: dhanyahk;markvi
translationtype: Human Translation
ms.sourcegitcommit: afd48a77faeeab6d5a8730934345cb7000a59831
ms.openlocfilehash: e72e49140aaad55a6e66a9f331ce7fde20b8577c
ms.lasthandoff: 02/23/2017


---
# <a name="how-to-find-activity-reports-in-the-azure-portal"></a>Speicherorte von Aktivitätsberichten im Azure-Portal

Mit der Migration von klassischen Azure-Portal zum Azure-Portal werfen wir einen neuen Blick auf die Azure Active Directory-Aktivitätsprotokolle. Wir haben vor einigen Monaten einen [Blogbeitrag](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/) veröffentlicht, aus dem hervorgeht, wie wir Aktivitätsprotokolle im Kontext der Ressource bieten, die Sie nutzen. In diesem Artikel wird beschrieben, wie wir die im klassischen Azure-Portal vorhandenen Berichte in das neue Azure-Portal übertragen haben.

## <a name="what-is-new"></a>Was ist neu?

Die Berichte im klassischen Azure-Portal sind in verschiedene Kategorien unterteilt:

1.    Sicherheitsberichte
2.    Aktivitätsberichte
3.    Berichte zu integrierten Apps

### <a name="activity-and-integrated-app-reports"></a>Aktivitätsberichte und Berichte zu integrierten Apps

Im neuen Azure-Portal sind wir zur kontextbasierten Berichterstellung gewechselt und haben die vorhandenen Berichte in einer zentralen Ansicht zusammengeführt, wobei eine einzelne zugrunde liegende API die Daten in der Ansicht bereitstellt. 

Sie finden diese Ansicht auf dem Blatt **Azure Active Directory** im Abschnitt **Aktivität** unter **Überwachungsprotokolle**.


![Überwachungsprotokolle](./media/active-directory-reporting-migration/482.png "Überwachungsprotokolle")








Es folgen die Berichte, die in dieser Ansicht zusammengeführt wurden.

-    Überwachungsbericht

-     Kennwortzurücksetzungsaktivität

-     Aktivität "Registrierung für Zurücksetzen des Kennworts"

-     Self-Service-Gruppenaktivität

-     Namensänderungen für Office&365;-Gruppen

-     Kontobereitstellungsaktivität

-     Status des Kennwortrollovers
-     Kontobereitstellungsfehler


Der Bericht zur Anwendungsnutzung wurde verbessert und enthält nun die Ansicht **Anmeldungen**. Sie finden diese Ansicht auf dem Blatt **Azure Active Directory** im Abschnitt **Aktivität**.


![Überwachungsprotokolle](./media/active-directory-reporting-migration/483.png "Überwachungsprotokolle")

Diese Ansicht enthält die Benutzeranmeldungen, die wiederum zum Abrufen der Informationen zur Anwendungsverwendung abgeleitet werden können. In der Übersicht über die **Unternehmensanwendungen** finden Sie ebenfalls Informationen zur Anwendungsnutzung.

![Überwachungsprotokolle](./media/active-directory-reporting-migration/484.png "Überwachungsprotokolle")



## <a name="how-can-i-access-a-specific-report-in-this-single-view"></a>Wie kann ich in dieser zentralen Ansicht auf einen bestimmten Bericht zugreifen?

### <a name="audit-logs"></a>Überwachungsprotokolle

Eine der häufigsten Anforderungen vieler Kunden ist die Möglichkeit, in Azure AD mittels mehrerer Filteroptionen auf Aktivitätsprotokolle zuzugreifen. An deren Stelle bieten wir nun einen erweiterten Filtermechanismus zum Filtern der gewünschten Daten. Einer der Filter, den wir bereitgestellt haben, heißt **Aktivitätskategorie**. Er listet die verschiedenen Typen von Aktivitätsprotokollen auf, die Azure AD bietet. Durch Auswahl der gewünschten Kategorie können Sie die Ergebnisse auf Ihre Suche eingrenzen. 

Wenn Sie beispielsweise nur an Aktivitäten im Zusammenhang mit der **Self-Service-Kennwortzurücksetzung** interessiert sind, können Sie die Kategorie**Self-Service-Kennwortverwaltung** wählen. Die angezeigten Kategorien sind im Kontext der Ressource, mit der Sie arbeiten.  


![Überwachungsprotokolle](./media/active-directory-reporting-migration/06.png "Überwachungsprotokolle")

 
Derzeit gibt es die folgenden Kategorien:

- Kernverzeichnis

- Self-Service-Kennwortverwaltung

- Self-Service-Gruppenverwaltung

- Kontobereitstellung

### <a name="application-usage"></a>Anwendungsnutzung

In der Ansicht **Aktivitäten > Anmeldungen** können Sie die Anwendungsnutzung für alle oder eine einzelne App anzeigen. Wie unten dargestellt, ist diese Ansicht für alle Anwendungen oder eine einzelne Anwendung vorhanden. Wenn Sie die Ergebnisse eingrenzen möchten, können Sie anhand des **Benutzer**- oder **Anwendungsnamens** filtern.
 

![Überwachungsprotokolle](./media/active-directory-reporting-migration/07.png "Überwachungsprotokolle")


### <a name="security-reports"></a>Sicherheitsberichte

Die Sicherheitsberichte wurden zusammengeführt, um eine vollständige Übersicht über alle sicherheitsrelevanten Risikoereignisse bereitzustellen, die Azure Active Directory erkennen und melden kann. Eine vollständige Übersicht finden Sie unter [Azure Active Directory-Risikoereignisse](active-directory-identity-protection-risk-events.md).  
In diesem Thema finden Sie eine Übersicht darüber, in welchem Zusammenhang die Azure Active Directory-Berichte zu anomalen Aktivitäten zu den Risikoereignissen in Azure AD im Abschnitt [Azure AD-Berichte zu anomalen Aktivitäten](active-directory-identity-protection-risk-events.md#azure-ad-anomalous-activity-reports) stehen.

Sie können im Azure-Portal auf dem Blatt **Azure Active Directory** im Abschnitt **Sicherheit** auf die Berichte zu erkannten Risikoereignissen zugreifen. Die erkannten Risikoereignisse werden in den folgenden Berichten nachverfolgt:   

- Gefährdete Benutzer
- Riskante Anmeldungen 


![Überwachungsprotokolle](./media/active-directory-reporting-migration/04.png "Überwachungsprotokolle")

Genauere Informationen zu diesen Berichten finden Sie unter:

- [Sicherheitsbericht „Gefährdete Benutzer“ im Azure Active Directory-Portal – Vorschau](active-directory-reporting-security-user-at-risk.md)
- [Bericht „Riskante Anmeldungen“ im Azure Active Directory-Portal – Vorschau](active-directory-reporting-security-risky-sign-ins.md)






## <a name="activity-reports-in-azure-classic-portal-versus-azure-portal"></a>Aktivitätsberichte im klassischen Azure-Portal im Vergleich zum Azure-Portal

Dieser Abschnitt enthält die vorhandenen Berichte im klassischen Azure-Portal und erläutert, wie Sie diese Informationen in das Azure-Portal übertragen können.


Ihr Einstiegspunkt für alle Überwachungsdaten ist die Option **Überwachungsprotokolle** im Abschnitt **Aktivität** auf dem Blatt **Azure Active Directory**.

![Überwachungsprotokolle](./media/active-directory-reporting-migration/61.png "Überwachungsprotokolle")


| Klassisches Azure-Portal                 | Schritte im Azure-Portal                                                         |
| ---                                  | ---                                                                        |
| Überwachungsprotokolle                           | Wählen Sie als **Aktivitätskategorie** die Option **Kernverzeichnis** aus.                       |
| Kennwortzurücksetzungsaktivität              | Wählen Sie als **Aktivitätskategorie** die Option **Self-Service-Kennwortverwaltung** aus. | 
| Aktivität "Registrierung für Zurücksetzen des Kennworts" | Wählen Sie als **Aktivitätskategorie** die Option **Self-Service-Kennwortverwaltung**.     |
| Self-Service-Gruppenaktivität         | Wählen Sie als **Aktivitätskategorie** die Option **Self-Service-Gruppenverwaltung** aus.        |
| Kontobereitstellungsaktivität        | Wählen Sie als **Aktivitätskategorie** die Option **Benutzerkontenbereitstellung** aus.         |
| Status des Kennwortrollovers             | Wählen Sie als **Aktivitätskategorie** die Option **Automatischer Rollover von App-Kennwörtern** aus.      |
| Kontobereitstellungsfehler          | Wählen Sie als **Aktivitätskategorie** die Option **Benutzerkontenbereitstellung** aus.        |
| Namensänderungen für Office&365;-Gruppen         | Wählen Sie als **Aktivitätskategorie** die Option **Self-Service-Kennwortverwaltung**, als **Aktivitätsressourcentyp** die Option **Gruppe** und als **Aktivitätsquelle** die Option **Office&365;-Gruppen** aus.|

 

Der Einstiegspunkt für den Bericht zur **Anwendungsnutzung** ist **Azure Active Directory > Unternehmensanwendungen > Anmeldungen**. 


![Überwachungsprotokolle](./media/active-directory-reporting-migration/199.png "Überwachungsprotokolle")


