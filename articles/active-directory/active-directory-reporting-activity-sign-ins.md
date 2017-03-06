---
title: "Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal – Vorschau | Microsoft-Dokumentation"
description: "Enthält eine Einführung in die Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal (Vorschau)."
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
ms.date: 02/22/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 78617dc7e3a4b6eb4fc32d32b6850b8c0832d6d8
ms.openlocfilehash: 9819c5f6a3aea53664d86e3a23b25946c0f2b731
ms.lasthandoff: 02/22/2017


---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal---preview"></a>Berichte zu Anmeldeaktivitäten im Azure Active Directory-Portal – Vorschau

Mit der Berichterstellungsfunktion in Azure Active Directory [(Vorschau)](active-directory-preview-explainer.md) erhalten Sie alle Informationen, die Sie zum Ermitteln des Zustands Ihrer Umgebung benötigen.

Die Architektur für die Berichterstellung in Azure Active Directory umfasst die folgenden Komponenten:

- **Aktivität** 
    - **Anmeldeaktivitäten:** Informationen zur Nutzung von verwalteten Anwendungen und Aktivitäten der Benutzeranmeldung
    - **Überwachungsprotokolle:** Systemaktivitätsinformationen zu Benutzern und zur Gruppenverwaltung sowie zu verwalteten Anwendungen und Verzeichnisaktivitäten.
- **Sicherheit** 
    - **Riskante Anmeldungen:** Eine riskante Anmeldung ist ein Indikator für einen Anmeldeversuch von einem Benutzer, der nicht der rechtmäßige Besitzer eines Benutzerkontos ist. Weitere Informationen finden Sie unter „Riskante Anmeldungen“.
    - **Benutzer mit Risikomarkierung:** Ein Benutzer mit Risikomarkierung ist ein Indikator für ein möglicherweise kompromittiertes Benutzerkonto. Weitere Informationen finden Sie unter „Benutzer mit Risikomarkierung“.

In diesem Thema erhalten Sie einen Überblick über die Anmeldeaktivitäten.

## <a name="signs-in-activities"></a>Anmeldeaktivitäten

Mit den Informationen, die vom Bericht zur Benutzeranmeldung geliefert werden, können Sie beispielsweise Antworten auf folgende Fragen ermitteln:

* Wie sieht das Anmeldemuster eines Benutzers aus?
* Wie viele Benutzer sind für Benutzer im Laufe einer Woche angemeldet?
* Wie lautet der Status dieser Anmeldungen?

Ihr Einstiegspunkt für diese Daten ist der Graph zur Benutzeranmeldung im Abschnitt **Übersicht** unter **Benutzer und Gruppen**.

 ![Berichterstellung](./media/active-directory-reporting-activity-sign-ins/05.png "Reporting")

Der Graph für die Benutzeranmeldung zeigt wöchentliche Aggregationen von Anmeldungen für alle Benutzer in einem bestimmten Zeitraum an. Die Standardeinstellung für den Zeitraum beträgt 30 Tage.

![Berichterstellung](./media/active-directory-reporting-activity-sign-ins/02.png "Reporting")

Wenn Sie im Graph für die Anmeldung auf einen Tag klicken, wird eine ausführliche Liste mit den Anmeldeaktivitäten angezeigt.

![Berichterstellung](./media/active-directory-reporting-activity-sign-ins/03.png "Reporting")

In jeder Zeile der Liste mit den Anmeldeaktivitäten erhalten Sie die ausführlichen Informationen zur ausgewählten Anmeldung, z.B.:

* Wer hat sich angemeldet?
* Welcher UPN wurde verwendet?
* Welche Anwendung war das Ziel der Anmeldung?
* Wie lautet die IP-Adresse der Anmeldung?
* Wie lautete der Status der Anmeldung?

## <a name="usage-of-managed-applications"></a>Nutzung von verwalteten Anwendungen

Mit einer anwendungsorientierten Ansicht Ihrer Anmeldedaten können Sie beispielsweise folgende Fragen beantworten:

* Wer verwendet meine Anwendungen?
* Welche drei Anwendungen sind im Unternehmen am beliebtesten?
* Ich habe vor Kurzem eine Anwendung eingeführt. Wie gut funktioniert sie?

Ihr Einstiegspunkt für diese Daten ist die Liste mit den beliebtesten drei Anwendungen in Ihrem Unternehmen im Bericht zu den letzten 30 Tagen (Abschnitt **Übersicht** unter **Unternehmensanwendungen**).

 ![Berichterstellung](./media/active-directory-reporting-activity-sign-ins/06.png "Reporting")

Der Graph zur App-Nutzung gibt die wöchentlichen Aggregationen von Anmeldungen für Ihre beliebtesten drei Anwendungen in einem bestimmten Zeitraum an. Die Standardeinstellung für den Zeitraum beträgt 30 Tage.

![Berichterstellung](./media/active-directory-reporting-activity-sign-ins/78.png "Reporting")

Wenn Sie möchten, können Sie den Fokus auf eine bestimmte Anwendung festlegen.

![Berichterstellung](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Reporting")

Wenn Sie im Graph für die App-Nutzung auf einen Tag klicken, wird eine ausführliche Liste mit den Anmeldeaktivitäten angezeigt.

![Berichterstellung](./media/active-directory-reporting-activity-sign-ins/top_app_sign_ins.png "Reporting")

Mit der Option **Anmeldungen** können Sie eine vollständige Übersicht über alle Anmeldeereignisse für Ihre Anwendungen anzeigen.

![Berichterstellung](./media/active-directory-reporting-activity-sign-ins/85.png "Reporting")

Mit der Spaltenauswahl können Sie die Datenfelder auswählen, die Sie anzeigen möchten.

![Berichterstellung](./media/active-directory-reporting-activity-sign-ins/column_chooser.png "Reporting")

## <a name="filtering-sign-ins"></a>Filtern von Anmeldungen
Sie können Anmeldungen nach einem Zeitintervall filtern, um die Menge der angezeigten Daten mithilfe der folgenden Felder zu begrenzen:

* Datum und Uhrzeit 
* Benutzerprinzipalname (UPN) des Benutzers
* Anwendungsname
* Clientname
* Anmeldestatus

![Berichterstellung](./media/active-directory-reporting-activity-sign-ins/293.png "Reporting")

Eine andere Methode zum Filtern von Einträgen der Anmeldeaktivitäten ist das Suchen nach bestimmten Einträgen.
Mit der Suchmethode können Sie die Anmeldungen auf bestimmte **Benutzer**, **Gruppen** oder **Anwendungen** begrenzen.

![Berichterstellung](./media/active-directory-reporting-activity-sign-ins/84.png "Reporting")


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Anleitung für Azure Active Directory-Berichte](active-directory-reporting-guide.md).


