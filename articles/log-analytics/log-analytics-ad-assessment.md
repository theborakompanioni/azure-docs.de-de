---
title: Optimieren Ihrer Active Directory-Umgebung mit Azure Log Analytics | Microsoft-Dokumentation
description: "Sie können die Active Directory Assessment-Lösung verwenden, um die Risiken und die Integrität Ihrer Serverumgebungen in regelmäßigen Abständen zu bewerten."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 81eb41b8-eb62-4eb2-9f7b-fde5c89c9b47
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: a0c8af30fbed064001c3fd393bf0440aa1cb2835
ms.openlocfilehash: d5caa7a6c1b1a36a4ad08e1b28c7f9157a2c6349
ms.lasthandoff: 02/28/2017


---
# <a name="optimize-your-active-directory-environment-with-the-active-directory-assessment-solution-in-log-analytics"></a>Optimieren Ihrer Active Directory-Umgebung mit der Active Directory Assessment-Lösung in Log Analytics

Sie können die Active Directory Assessment-Lösung verwenden, um die Risiken und die Integrität Ihrer Serverumgebungen in regelmäßigen Abständen zu bewerten. Dieser Artikel unterstützt Sie beim Installieren und Verwenden der Lösung, sodass Sie bei potenziellen Problemen korrigierende Maßnahmen ergreifen können.

Die Lösung bietet eine priorisierte Liste von Empfehlungen, die spezifisch für Ihre bereitgestellte Serverinfrastruktur gelten. Die Empfehlungen sind in vier Schwerpunktbereiche eingeteilt, mit deren Hilfe Sie die Risiken schnell einschätzen und entsprechende Maßnahmen ergreifen können.

Die Empfehlungen basieren auf den Kenntnissen und Erfahrungen, die Microsoft-Experten bei Tausenden von Kundenbesuchen gesammelt haben. Jede Empfehlung enthält Informationen, warum ein Problem ggf. für Sie relevant ist und wie Sie die vorgeschlagenen Änderungen umsetzen können.

Sie können Schwerpunktbereiche wählen, die für Ihre Organisation am wichtigsten sind, und Ihren Fortschritt in Richtung einer risiko- und fehlerfreien Umgebung nachverfolgen.

Nachdem Sie die Lösung hinzugefügt haben und eine Bewertung durchgeführt wurde, werden zusammenfassende Informationen für Schwerpunktbereiche im Dashboard **AD Assessment** für die Infrastruktur in Ihrer Umgebung angezeigt. In den folgenden Abschnitten wird beschrieben, wie Sie die Informationen im Dashboard **AD Assessment** anzeigen und dann die empfohlenen Maßnahmen für Ihre Active Directory-Serverinfrastruktur durchführen können.

![Abbildung der Kachel "SQL Assessment"](./media/log-analytics-ad-assessment/ad-tile.png)

![Abbildung des Dashboards "SQL-Assessment"](./media/log-analytics-ad-assessment/ad-assessment.png)

## <a name="installing-and-configuring-the-solution"></a>Installieren und Konfigurieren der Lösung
Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösungen.

* Agents müssen auf Domänencontrollern installiert werden, die Mitglieder der auszuwertenden Domäne sind.
* Für die Active Directory Assessment-Lösung muss eine unterstützte Version von .NET Framework 4 (4.5.2 oder höher) auf jedem Computer installiert sein, der über einen OMS-Agent verfügt.
* Fügen Sie mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md)beschriebenen Prozesses die Active Directory Assessment-Lösung Ihrem OMS-Arbeitsbereich hinzu.  Es ist keine weitere Konfiguration erforderlich.

  > [!NOTE]
  > Nachdem Sie die Lösung hinzugefügt haben, wird die Datei AdvisorAssessment.exe den Servern mit Agents hinzugefügt. Konfigurationsdaten werden gelesen und dann zur Verarbeitung an den OMS-Dienst in der Cloud gesendet. Auf die empfangenen Daten wird Logik angewendet, und der Clouddienst zeichnet die Daten auf.
  >
  >

## <a name="active-directory-assessment-data-collection-details"></a>Details zur Active Directory Assessment-Datensammlung
Active Directory Assessment erfasst WMI-Daten, Registrierungsdaten und Leistungsdaten mithilfe von Agents, die Sie aktiviert haben.

Die folgende Tabelle zeigt die Datensammlungsmethoden für Agents und gibt an, ob Operations Manager (SCOM) erforderlich ist und wie häufig Daten von einem Agent gesammelt werden.

| Plattform | Direkt-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |![Ja](./media/log-analytics-ad-assessment/oms-bullet-green.png) |![Ja](./media/log-analytics-ad-assessment/oms-bullet-green.png) |![Nein](./media/log-analytics-ad-assessment/oms-bullet-red.png) |![Nein](./media/log-analytics-ad-assessment/oms-bullet-red.png) |![Ja](./media/log-analytics-ad-assessment/oms-bullet-green.png) |7 Tage |

## <a name="understanding-how-recommendations-are-prioritized"></a>Grundlegendes zum Priorisieren von Empfehlungen
Jede vorgenommene Empfehlung erhält einen Gewichtungswert, der die relative Wichtigkeit der Empfehlung angibt. Es werden nur die zehn wichtigsten Empfehlungen angezeigt.

### <a name="how-weights-are-calculated"></a>Berechnen von Gewichtungen
Gewichtungen sind aggregierte Werte, die auf drei wesentlichen Faktoren basieren:

* Der *Wahrscheinlichkeit* , dass ein erkanntes Problem Schwierigkeiten verursacht. Eine höhere Wahrscheinlichkeit entspricht einer höheren Gesamtwertung für die Empfehlung.
* Der *Auswirkung* des Problems auf Ihre Organisation, wenn es tatsächlich Schwierigkeiten verursacht. Eine stärkere Auswirkung entspricht einer höheren Gesamtwertung für die Empfehlung.
* Dem erforderlichen *Aufwand* zur Umsetzung der Empfehlung. Ein höherer Aufwand entspricht einer niedrigeren Gesamtwertung für die Empfehlung.

Die Gewichtung für jede Empfehlung wird als Prozentsatz der Gesamtwertung ausgedrückt, die für jeden Schwerpunktbereich verfügbar ist. Wenn beispielsweise eine Empfehlung im Schwerpunktbereich "Sicherheit und Einhaltung" eine Wertung von 5 % hat, erhöht eine Umsetzung dieser Empfehlung die Gesamtwertung von "Sicherheit und Einhaltung" um 5 %.

### <a name="focus-areas"></a>Schwerpunktbereiche
**Sicherheit und Compliance** : Dieser Schwerpunktbereich zeigt Empfehlungen hinsichtlich potenzieller Sicherheitsrisiken und Sicherheitsverletzungen sowie Unternehmensrichtlinien und Anforderungen an die Einhaltung gesetzlicher Bestimmungen und technischer Vorgaben.

**Verfügbarkeit und Geschäftskontinuität** : Dieser Schwerpunktbereich enthält Empfehlungen hinsichtlich Dienstverfügbarkeit, Infrastrukturstabilität und Schutz des Geschäftsbetriebs.

**Leistung und Skalierbarkeit** : Dieser Schwerpunktbereich zeigt Empfehlungen zum Wachstum Ihrer IT-Infrastruktur sowie zur Sicherstellung, dass Ihre IT-Umgebung die aktuellen Leistungsanforderungen erfüllt und auf veränderte Infrastrukturanforderungen reagieren kann.

**Aktualisierung, Migration und Bereitstellung** : Dieser Schwerpunktbereich zeigt Empfehlungen, mit deren Hilfe Sie Active Directory aktualisieren, migrieren und in Ihrer vorhandenen Infrastruktur bereitstellen können.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Müssen in jedem Schwerpunktbereich 100 % erzielt werden?
Nicht unbedingt. Die Empfehlungen basieren auf den Kenntnissen und Erfahrungen, die Microsoft-Experten bei Tausenden von Kundenbesuchen gesammelt haben. Jedoch sind keine zwei Serverinfrastrukturen identisch, und spezifische Empfehlungen können mal mehr oder mal weniger relevant für Sie sein. Zum Beispiel sind einige Sicherheitsempfehlungen möglicherweise weniger relevant, wenn Ihre virtuellen Computer nicht mit dem Internet verbunden sind. Verschiedene Verfügbarkeitsempfehlungen können weniger relevant für Dienste sein, die Ad-hoc-Datensammlung und -Berichterstattung mit niedriger Priorität bereitstellen. Probleme, die für ein gewachsenes Unternehmen Relevanz haben, sind für ein Start-up ggf. weniger wichtig. Es empfiehlt sich zu ermitteln, welche Schwerpunktbereiche zu Ihren Prioritäten zählen, und dann zu beobachten, wie sich Ihre Wertungen mit der Zeit verändern.

Jede Empfehlung enthält Informationen dazu, warum sie wichtig ist. Sie sollten anhand dieser Anleitung feststellen, ob die Umsetzung der Empfehlung bei Berücksichtigung der Art Ihrer IT-Dienste und der geschäftlichen Anforderungen Ihrer Organisation für Sie geeignet ist.

## <a name="use-assessment-focus-area-recommendations"></a>Befolgen von Empfehlungen anhand der Bewertung des Schwerpunktbereichs
Bevor Sie eine Assessment-Lösung in OMS verwenden können, müssen Sie die Lösung installieren. Weitere Informationen zum Installieren von Lösungen finden Sie unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md). Nach der Installation können Sie die Zusammenfassung der Empfehlungen anzeigen, indem Sie in OMS auf der Übersichtsseite die Kachel „Assessment“ auswählen.

Sehen Sie sich die zusammengefassten Compliancebewertungen für Ihre Infrastruktur sowie Details in den Empfehlungen an.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>So werden Empfehlungen für einen Schwerpunktbereich angezeigt und korrigierende Maßnahmen ergriffen
1. Klicken Sie auf der Seite **Übersicht** auf die Kachel **Bewertung** für Ihre Serverinfrastruktur.
2. Überprüfen Sie auf der Seite **Assessment** die Zusammenfassungsinformationen in einem der Schwerpunktbereiche, und klicken Sie dann auf einen Schwerpunktbereich, um Empfehlungen für diesen Bereich anzuzeigen.
3. Auf jeder der Schwerpunktbereichsseiten können Sie mit Prioritäten versehene Empfehlungen für Ihre Umgebung anzeigen. Klicken Sie unter **Betroffene Objekte** auf eine Empfehlung, um Details zu den Gründen für diese Empfehlung anzuzeigen.  
    ![Abbildung der Bewertungsempfehlungen](./media/log-analytics-ad-assessment/ad-focus.png)
4. Sie können die unter **Vorgeschlagene Aktionen**vorgeschlagenen Korrekturmaßnahmen durchführen. Nachdem das Element behandelt wurde, geben spätere Bewertungen an, dass empfohlene Aktionen ausgeführt wurden, und Ihre Bewertung der Einhaltung erhöht sich. Korrigierte Elemente werden als **Passed Objects**angezeigt.

## <a name="ignore-recommendations"></a>Ignorieren von Empfehlungen
Wenn Sie Empfehlungen ignorieren möchten, können Sie eine Textdatei erstellen, die OMS verwendet, um zu verhindern, dass diese Empfehlungen in Ihren Bewertungsergebnissen angezeigt werden.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Ermitteln von Empfehlungen, die Sie ignorieren möchten
1. Melden Sie sich bei Ihrem Arbeitsbereich an, und öffnen Sie die Protokollsuche. Verwenden Sie folgende Abfrage, um Empfehlungen aufzulisten, die für Computer in Ihrer Umgebung nicht funktionieren.

   ```
   Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```

   Dieser Screenshot zeigt eine Protokollsuchabfrage: ![Empfehlungen mit Fehlern](./media/log-analytics-ad-assessment/ad-failed-recommendations.png).
2. Wählen Sie die Empfehlungen aus, die Sie ignorieren möchten. Sie werden die Werte für RecommendationId in der nächsten Prozedur verwenden.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Erstellen und Verwenden einer IgnoreRecommendations.txt-Textdatei
1. Erstellen Sie eine Datei namens IgnoreRecommendations.txt.
2. Fügen oder geben Sie auf separaten Zeilen die RecommendationId für jede Empfehlung ein, die Log Analytics ignorieren soll, und speichern und schließen Sie die Datei.
3. Legen Sie die Datei auf jedem Computer, auf dem OMS die Empfehlungen ignorieren soll, in folgendem Ordner ab.
   * Auf Computern mit Microsoft Monitoring Agent (direkt oder über Operations Manager verbunden) – *Systemlaufwerk*:\Programme\Microsoft Monitoring Agent\Agent
   * Auf dem Operations Manager-Verwaltungsserver – *Systemlaufwerk*:\Programme\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Überprüfen, ob Empfehlungen ignoriert werden
Nach Ausführung der nächsten geplanten Bewertung – standardmäßig alle 7 Tage – werden die angegebenen Empfehlungen als *Ignoriert* gekennzeichnet und auf dem Bewertungsdashboard nicht angezeigt.

1. Sie können folgende Protokollsuchabfragen verwenden, um alle ignorierten Empfehlungen aufzulisten.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
2. Wenn Sie sich später dazu entscheiden, die ignorierten Empfehlungen anzuzeigen, entfernen Sie alle IgnoreRecommendations.txt-Dateien, oder entfernen Sie einzelne RecommendationIDs aus den Dateien.

## <a name="ad-assessment-solutions-faq"></a>AD Assessment-Lösungen – häufig gestellte Fragen
*Wie oft erfolgt eine Bewertung?*

* Die Bewertung erfolgt alle sieben Tage.

*Gibt es eine Möglichkeit, die Häufigkeit der Bewertung zu konfigurieren?*

* Derzeit leider nicht.

*Wird ein anderer Server, der erst nach dem Hinzufügen einer Assessment-Lösung erkannt wird, auch bewertet?*

* Ja. Ab dem Zeitpunkt der Erkennung wird er alle sieben Tage bewertet.

*Wann wird ein Server, der außer Betrieb genommen wird, aus der Bewertung entfernt?*

* Ein Server, der drei Wochen keine Daten übertragen hat, wird entfernt.

*Wie lautet der Name des Prozesses, der die Daten sammelt?*

* AdvisorAssessment.exe

*Wie lange dauert das Sammeln der Daten?*

* Die eigentliche Datensammlung auf dem Server dauert etwa eine Stunde. Auf Servern mit vielen Active Directory-Servern kann der Vorgang auch mehr Zeit in Anspruch nehmen.

*Welche Art von Daten wird gesammelt?*

* Die folgenden Datentypen werden gesammelt:
  * WMI
  * Registrierung
  * Leistungsindikatoren

*Gibt es eine Möglichkeit, den Zeitpunkt der Datensammlung zu konfigurieren?*

* Derzeit leider nicht.

*Warum werden nur die ersten 10 Empfehlungen angezeigt?*

* Anstatt Ihnen eine umfangreiche und erdrückende Aufgabenliste zu präsentieren, empfehlen wir, sich zuerst auf die Empfehlungen mit hoher Priorität zu konzentrieren. Nach deren Umsetzung werden weitere Empfehlungen verfügbar. Wenn Sie jedoch lieber die ganze Liste mit allen Einzelheiten anzeigen möchten, können Sie mithilfe der Protokollsuche alle Empfehlungen anzeigen.

*Gibt es eine Möglichkeit, eine Empfehlung zu ignorieren?*

* Ja, siehe oben stehenden Abschnitt [Ignorieren von Empfehlungen](#ignore-recommendations) .

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie die [Protokollsuche in Log Analytics](log-analytics-log-searches.md) , um detaillierte Daten und Empfehlungen von AD Assessment anzuzeigen.

