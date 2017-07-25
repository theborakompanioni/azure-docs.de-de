---
title: "Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal | Microsoft-Dokumentation"
description: "Enthält eine Einführung in die Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: b9e61950654ba427b09dd608d354589a0804aaa5
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal"></a>Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal

Mit der Azure Active Directory-Berichterstellungsfunktion (Azure AD) im [Azure-Portal](https://portal.azure.com) können Sie alle Informationen abrufen, die Sie zum Ermitteln des Zustands Ihrer Umgebung benötigen.

Die Architektur für die Berichterstellung in Azure Active Directory umfasst die folgenden Komponenten:

- **Aktivität** 
    - **Anmeldeaktivitäten** : Informationen zur Nutzung von verwalteten Anwendungen und Aktivitäten der Benutzeranmeldung
    - **Überwachungsprotokolle:** Systemaktivitätsinformationen zu Benutzern und zur Gruppenverwaltung sowie zu verwalteten Anwendungen und Verzeichnisaktivitäten.
- **Sicherheit** 
    - **Riskante Anmeldungen:** Eine riskante Anmeldung ist ein Indikator für einen Anmeldeversuch von einem Benutzer, der nicht der rechtmäßige Besitzer eines Benutzerkontos ist. Weitere Informationen finden Sie unter „Riskante Anmeldungen“.
    - **Benutzer mit Risikomarkierung:** Ein Benutzer mit Risikomarkierung ist ein Indikator für ein möglicherweise kompromittiertes Benutzerkonto. Weitere Informationen finden Sie unter „Benutzer mit Risikomarkierung“.

In diesem Thema erhalten Sie einen Überblick über die Anmeldeaktivitäten.

## <a name="pre-requisite"></a>Voraussetzung

### <a name="who-can-access-the-data"></a>Wer kann auf die Daten zugreifen?
* Benutzer mit den Rollen „Sicherheitsadministrator“ oder der Berechtigung „Sicherheit lesen“
* Globale Administratoren
* Jeder Benutzer (Nicht-Administratoren) kann auf seine eigenen Anmeldungen zugreifen. 

### <a name="what-azure-ad-license-do-you-need-to-access-sign-in-activity"></a>Welche Azure AD-Lizenz benötigen Sie für den Zugriff auf die Anmeldeaktivität?
* Ihrem Mandanten muss eine Azure AD Premium-Lizenz zugewiesen sein, damit der Gesamtbericht für Anmeldeaktivitäten angezeigt werden kann.


## <a name="signs-in-activities"></a>Anmeldeaktivitäten

Mit den Informationen, die vom Bericht zur Benutzeranmeldung geliefert werden, können Sie beispielsweise Antworten auf folgende Fragen ermitteln:

* Wie sieht das Anmeldemuster eines Benutzers aus?
* Wie viele Benutzer sind für Benutzer im Laufe einer Woche angemeldet?
* Wie lautet der Status dieser Anmeldungen?

Ihr erster Einstiegspunkt für alle Anmeldeaktivitäten ist **Anmeldungen** im Abschnitt „Aktivität“ von **Azure Active**


![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/61.png "Anmeldeaktivität")


Ein Überwachungsprotokoll enthält eine Standardlistenansicht mit folgenden Informationen:

- Zugehöriger Benutzer
- Anwendung, an der sich der Benutzer angemeldet hat
- Anmeldestatus
- Zeitpunkt der Anmeldung

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/41.png "Anmeldeaktivität")

Sie können die Listenansicht anpassen, indem Sie in der Symbolleiste auf **Spalten** klicken.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/19.png "Anmeldeaktivität")

Sie können dann weitere Felder anzeigen oder Felder entfernen, die bereits angezeigt werden.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/42.png "Anmeldeaktivität")

Wenn Sie in der Listenansicht auf einen Eintrag klicken, werden alle Details dazu angezeigt.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/43.png "Anmeldeaktivität")


## <a name="filtering-sign-in-activities"></a>Filtern von Anmeldeaktivitäten

Sie können die Anmeldedaten mit den folgenden Feldern filtern, um die gemeldeten Daten gemäß Ihren Bedürfnissen einzugrenzen:

- Zeitintervall
- Benutzer
- Anwendung
- Client-
- Anmeldestatus

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/44.png "Anmeldeaktivität")


Mit dem Filter **Zeitintervall** können Sie einen Zeitrahmen für die zurückgegebenen Daten festlegen.  
Mögliche Werte:

- 1 Monat
- 7 Tage
- 24 Stunden
- Benutzerdefiniert

Beim Auswählen eines benutzerdefinierten Zeitraums können Sie eine Startzeit und eine Endzeit konfigurieren.

Mit dem Filter **Benutzer** können Sie den Namen oder den Dienstprinzipalnamen (UPN) des gewünschten Benutzers angeben.

Mit dem Filter **Anwendung** können Sie den Namen der gewünschten Anwendung angeben.

Mit dem Filter **Client** können Sie Informationen zum gewünschten Gerät angeben.

Für den Filter **Anmeldestatus** können Sie eine der folgenden Filteroptionen auswählen:

- Alle
- Erfolgreich
- Fehler


## <a name="sign-in-activities-shortcuts"></a>Optionen für Anmeldeaktivitäten

Zusätzlich zu Azure Active Directory enthält das Azure-Portal zwei weitere Einstiegspunkte für Daten zu Anmeldeaktivitäten:

- Benutzer und Gruppen
- Unternehmensanwendungen


### <a name="users-and-groups-sign-ins-activities"></a>Anmeldeaktivitäten für Benutzer und Gruppen

Mit den Informationen, die vom Bericht zur Benutzeranmeldung geliefert werden, können Sie beispielsweise Antworten auf folgende Fragen ermitteln:

- Wie sieht das Anmeldemuster eines Benutzers aus?
- Wie viele Benutzer sind für Benutzer im Laufe einer Woche angemeldet?
- Wie lautet der Status dieser Anmeldungen?



Ihr Einstiegspunkt für diese Daten ist der Graph zur Benutzeranmeldung im Abschnitt **Übersicht** unter **Benutzer und Gruppen**.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/45.png "Anmeldeaktivität")

Der Graph für die Benutzeranmeldung zeigt wöchentliche Aggregationen von Anmeldungen für alle Benutzer in einem bestimmten Zeitraum an. Die Standardeinstellung für den Zeitraum beträgt 30 Tage.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/46.png "Anmeldeaktivität")

Wenn Sie im Graph für die Anmeldung auf einen Tag klicken, wird eine ausführliche Liste mit den Anmeldeaktivitäten für den entsprechenden Tag angezeigt.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/41.png "Anmeldeaktivität")

In jeder Zeile der Liste mit den Anmeldeaktivitäten erhalten Sie die ausführlichen Informationen zur ausgewählten Anmeldung, z.B.:

* Wer hat sich angemeldet?
* Welcher UPN wurde verwendet?
* Welche Anwendung war das Ziel der Anmeldung?
* Wie lautet die IP-Adresse der Anmeldung?
* Wie lautete der Status der Anmeldung?

Mit der Option **Anmeldungen** erhalten Sie eine vollständige Übersicht über alle Benutzeranmeldungen.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/51.png "Anmeldeaktivität")



## <a name="usage-of-managed-applications"></a>Nutzung von verwalteten Anwendungen

Mit einer anwendungsorientierten Ansicht Ihrer Anmeldedaten können Sie beispielsweise folgende Fragen beantworten:

* Wer verwendet meine Anwendungen?
* Welche drei Anwendungen sind im Unternehmen am beliebtesten?
* Ich habe vor Kurzem eine Anwendung eingeführt. Wie gut funktioniert sie?

Ihr Einstiegspunkt für diese Daten ist die Liste mit den beliebtesten drei Anwendungen in Ihrem Unternehmen im Bericht zu den letzten 30 Tagen (Abschnitt **Übersicht** unter **Unternehmensanwendungen**).

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/64.png "Anmeldeaktivität")

Der Graph zur App-Nutzung gibt die wöchentlichen Aggregationen von Anmeldungen für Ihre beliebtesten drei Anwendungen in einem bestimmten Zeitraum an. Die Standardeinstellung für den Zeitraum beträgt 30 Tage.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/47.png "Anmeldeaktivität")

Wenn Sie möchten, können Sie den Fokus auf eine bestimmte Anwendung festlegen.


![Berichterstellung](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Reporting")

Wenn Sie im Graph für die App-Nutzung auf einen Tag klicken, wird eine ausführliche Liste mit den Anmeldeaktivitäten angezeigt.


![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/48.png "Anmeldeaktivität")


Mit der Option **Anmeldungen** können Sie eine vollständige Übersicht über alle Anmeldeereignisse für Ihre Anwendungen anzeigen.

![Anmeldeaktivität](./media/active-directory-reporting-activity-sign-ins/49.png "Anmeldeaktivität")



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Fehlercodes von Anmeldeaktivitäten finden Sie unter [Fehlercodes des Berichts mit den Anmeldeaktivitäten im Azure Active Directory-Portal](active-directory-reporting-activity-sign-ins-errors.md).


