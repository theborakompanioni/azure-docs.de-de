---
title: "Optimieren der Umgebung mit der System Center Operations Manager-Bewertungslösung in Log Analytics | Microsoft-Dokumentation"
description: "Sie können die System Center Operations Manager-Bewertungslösung verwenden, um die Risiken und die Integrität Ihrer Serverumgebungen in regelmäßigen Abständen zu bewerten."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: jwhit
editor: tysonn
ms.assetid: 49aad8b1-3e05-4588-956c-6fdd7715cda1
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/18/2016
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 04eebbe19354ab1e927ee2bb41f25dc6676a7c93
ms.openlocfilehash: c28e73f0a6833793561d8702eb6c6480b9fbc0a4


---

# <a name="optimize-your-environment-with-the-system-center-operations-manager-assessment-preview-solution-in-log-analytics"></a>Optimieren der Umgebung mit der System Center Operations Manager-Bewertungslösung (Vorschau) in Log Analytics

Sie können die System Center Operations Manager-Bewertungslösung verwenden, um die Risiken und die Integrität Ihrer System Center Operations Manager-Serverumgebungen in regelmäßigen Abständen zu bewerten. Dieser Artikel unterstützt Sie beim Installieren, Konfigurieren und Verwenden der Lösung, sodass Sie bei potenziellen Problemen korrigierende Maßnahmen ergreifen können.

Die Lösung bietet eine priorisierte Liste von Empfehlungen, die spezifisch für Ihre bereitgestellte Serverinfrastruktur gelten. Die Empfehlungen sind in vier Schwerpunktbereiche eingeteilt, mit deren Hilfe Sie die Risiken schnell einschätzen und korrigierende Maßnahmen ergreifen können.

Die Empfehlungen basieren auf den Kenntnissen und Erfahrungen, die Microsoft-Experten bei Tausenden von Kundenbesuchen gesammelt haben. Jede Empfehlung enthält Informationen, warum ein Problem ggf. für Sie relevant ist und wie Sie die vorgeschlagenen Änderungen umsetzen können.

Sie können Schwerpunktbereiche wählen, die für Ihre Organisation am wichtigsten sind, und Ihren Fortschritt in Richtung einer risiko- und fehlerfreien Umgebung nachverfolgen.

Nachdem Sie die Lösung hinzugefügt haben und eine Bewertung durchgeführt wurde, werden zusammenfassende Informationen für Schwerpunktbereiche im Dashboard **System Center Operations Manager-Bewertung** für Ihre Infrastruktur angezeigt. In den folgenden Abschnitten wird beschrieben, wie Sie die Informationen im Dashboard **System Center Operations Manager-Bewertung** anzeigen und dann die empfohlenen Maßnahmen für Ihre SCOM-Infrastruktur durchführen können.

![Kachel für die System Center Operations Manager-Lösung](./media/log-analytics-scom-assessment/scom-tile.png)

![Dashboard „System Center Operations Manager-Bewertung“](./media/log-analytics-scom-assessment/scom-dashboard01.png)

## <a name="installing-and-configuring-the-solution"></a>Installieren und Konfigurieren der Lösung

Die Lösung kann mit Microsoft System Operations Manager 2012 R2 und 2012 SP1 verwendet werden.

Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

- Ein Operations Manager-Verwaltungsserver aus einer Verwaltungsgruppe muss für die Verbindung mit OMS konfiguriert werden. Informationen zum Verbinden des Operations Manager-Verwaltungsservers mit OMS finden Sie unter [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](log-analytics-om-agents.md#connecting-operations-manager-to-oms).
    - Wenn Sie mit der OMS-verwalteten Computergruppe mehrere Verwaltungsserver in einer Verwaltungsgruppe überwachen, stellen Sie sicher, dass die Bewertung für die Ausführung auf einem Verwaltungsserver konfiguriert ist. Weitere Informationen finden Sie unter [Konfigurieren der Bewertungsregel](#configure-the-assessment-rule).
- Bevor Sie eine Assessment-Lösung in OMS verwenden können, müssen Sie die Lösung installieren. Weitere Informationen zum Installieren von Lösungen finden Sie unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md).
- Wenn Sie den Operations Manager-Agent mit der System Center Operations Manager-Bewertung einsetzen möchten, müssen Sie ein ausführendes Operations Manager-Konto verwenden. Weitere Informationen finden Sie unter [Ausführende Operations Manager-Konten für OMS](#operations-manager-run-as-accounts-for-oms).
    >[!NOTE]
    Nachdem Sie die Lösung hinzugefügt haben, wird die Datei „AdvisorAssessment.exe“ dem SCOM-Server hinzugefügt. Konfigurationsdaten werden gelesen und dann zur Verarbeitung an den OMS-Dienst in der Cloud gesendet. Auf die empfangenen Daten wird Logik angewendet, und der Clouddienst zeichnet die Daten auf.

# <a name="system-center-operations-manager-assessment-data-collection-details"></a>Details zur Datensammlung mit der System Center Operations Manager-Bewertung

Die System Center Operations Manager-Bewertung sammelt unter Verwendung des von Ihnen aktivierten Servers über Windows PowerShell, SQL-Abfragen und Dateiinformationssammler WMI-, Registrierungs-, EventLog- und Operations Manager-Daten.

Die folgende Tabelle zeigt die Datensammlungsmethoden für die System Center Operations Manager-Bewertung und gibt an, wie häufig Daten von einem Agent gesammelt werden.

| Plattform | Direkt-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |  ![Nein](./media/log-analytics-scom-assessment/oms-bullet-red.png) | ![Ja](./media/log-analytics-scom-assessment/oms-bullet-green.png)  | ![Nein](./media/log-analytics-scom-assessment/oms-bullet-red.png)  |  ![Ja](./media/log-analytics-scom-assessment/oms-bullet-green.png) | ![Nein](./media/log-analytics-scom-assessment/oms-bullet-red.png)  | sieben Tage |

## <a name="operations-manager-run-as-accounts-for-oms"></a>Ausführende Operations Manager-Konten für OMS

OMS basiert auf Management Packs für Workloads, um Dienste bereitzustellen, die einen Mehrwert schaffen. Jede Arbeitsauslastung erfordert spezifische Berechtigungen zum Ausführen von Management Packs in einem anderen Sicherheitskontext, z. B. ein Domänenkonto. Konfigurieren Sie ein ausführendes Operations Manager-Konto, um Anmeldeinformationen anzugeben.

Verwenden Sie die folgenden Informationen, um das ausführende Operations Manager-Konto für die System Center Operations Manager-Bewertung festzulegen.

### <a name="set-the-run-as-account"></a>Festlegen des ausführenden Kontos

1. Rufen Sie in der Operations Manager-Konsole die Registerkarte **Administration** auf.
2. Klicken Sie unter **Run As Configuration** (Ausführende Konfiguration) auf **Konten**.
3. Erstellen Sie das ausführende Konto, indem Sie über den Assistenten ein Windows-Konto erstellen. Das zu verwendende Konto muss die folgenden Voraussetzungen erfüllen:

    >[!NOTE]
    Das ausführende Konto muss die folgenden Anforderungen erfüllen: - Es muss Domänenkontomitglied der lokalen Administratorgruppe auf allen Servern in der Umgebung sein (alle Operations Manager-Rollen – Management Server, OpsMgr-Datenbank, Data Warehouse, Berichterstellung, Webkonsole, Gateway). - Ihm muss die Operation Manager-Administratorrolle für die Verwaltungsgruppe zugewiesen sein, die bewertet wird. - Ihm muss die SysAdmin-Rolle auf allen Computern mit SQL Server oder in allen SQL Server-Instanzen zugewiesen sein, die von Operations Manager verwendet werden.

4. Wählen Sie unter **Verteilungssicherheit** die Option **More secure** (Höhere Sicherheit).
5. Geben Sie den Verwaltungsserver an, auf dem das Konto verteilt wird.
3. Wechseln Sie zurück zur ausführenden Konfiguration, und klicken Sie auf **Profile**.
4. Suchen Sie nach dem *Profil der SCOM-Bewertung*.
5. Der Profilname sollte wie folgt lauten: *Microsoft System Center Advisor SCOM Assessment Run As Profile* (Ausführendes Profil für Microsoft System Center Advisor-SCOM-Bewertung).
6. Klicken Sie mit der rechten Maustaste, aktualisieren Sie die Eigenschaften, und fügen Sie das in Schritt 3 erstellte ausführende Konto hinzu.

### <a name="sql-script-granting-permissions-to-the-run-as-account"></a>SQL-Skript, das dem ausführenden Konto Berechtigungen erteilt

Führen Sie das folgende SQL-Skript aus, um dem ausführenden Konto in der von Operations Manager verwendeten SQL-Instanz die erforderlichen Berechtigungen zu gewähren.

```
-- Replace <UserName> with the actual user name being used as Run As Account.
USE master

-- Create login for the user, comment this line if login is already created.
CREATE LOGIN [UserName] FROM WINDOWS


--GRANT permissions to user.
GRANT VIEW SERVER STATE TO [UserName]
GRANT VIEW ANY DEFINITION TO [UserName]
GRANT VIEW ANY DATABASE TO [UserName]

-- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
-- NOTE: This command must be run anytime new databases are added to SQL Server instances.
EXEC sp_msforeachdb N'USE [?]; CREATE USER [UserName] FOR LOGIN [UserName];'

Use msdb
GRANT SELECT To [UserName]
Go

--Give SELECT permission on all Operations Manager related Databases

--Replace the Operations Manager database name with the one in your environment
Use [OperationsManager];
GRANT SELECT To [UserName]
GO

--Replace the Operations Manager DatawareHouse database name with the one in your environment
Use [OperationsManagerDW];
GRANT SELECT To [UserName]
GO

--Replace the Operations Manager Audit Collection database name with the one in your environment
Use [OperationsManagerAC];
GRANT SELECT To [UserName]
GO

--Give db_owner on [OperationsManager] DB
--Replace the Operations Manager database name with the one in your environment
USE [OperationsManager]
GO
ALTER ROLE [db_owner] ADD MEMBER [UserName]

```


### <a name="configure-the-assessment-rule"></a>Konfigurieren der Bewertungsregel

Das Management Pack der System Center Operations Manager-Bewertungslösung enthält eine Regel namens *Microsoft System Center Advisor SCOM Assessment Run Assessment Rule* (Microsoft System Center Advisor-SCOM-Bewertungsregel für die Ausführung der Bewertung). Diese Regel ist für die Ausführung der Bewertung zuständig. Führen Sie zum Aktivieren der Regel und zum Konfigurieren der Häufigkeit die unten aufgeführten Schritte aus.

Die Microsoft System Center Advisor-SCOM-Bewertungsregel für die Ausführung der Bewertung ist standardmäßig deaktiviert. Zum Ausführen der Bewertung müssen Sie die Regel auf einem Verwaltungsserver aktivieren. Führen Sie die folgenden Schritte aus:

#### <a name="enable-the-rule-for-a-specific-management-server"></a>Aktivieren der Regel für einen bestimmten Verwaltungsserver

1. Suchen Sie in der Operations Manager-Konsole im Arbeitsbereich **Erstellung** im Bereich **Regeln** die Regel *Microsoft System Center Advisor SCOM Assessment Run Assessment Rule* (Microsoft System Center Advisor-SCOM-Bewertungsregel für die Ausführung der Bewertung).
2. Wählen Sie in den Suchergebnissen die Regel aus, die den Text *Typ: Verwaltungsserver* enthält.
3. Klicken Sie mit der rechten Maustaste auf die Regel, und klicken Sie dann auf **Außerkraftsetzungen** > **Für ein bestimmtes Objekt der Klasse: Verwaltungsserver**.
4.  Wählen Sie in der Liste der verfügbaren Verwaltungsserver den Verwaltungsserver aus, auf dem die Regel ausgeführt werden soll.
5.  Stellen Sie sicher, dass Sie für den Parameterwert **Aktiviert** den Außerkraftsetzungswert in **True** ändern.  
    ![Außerkraftsetzungsparameter](./media/log-analytics-scom-assessment/rule.png)

Konfigurieren Sie mit dem folgenden Verfahren im gleichen Fenster die Häufigkeit für die Ausführung.

#### <a name="configure-the-run-frequency"></a>Konfigurieren der Ausführungshäufigkeit

Als Standardintervall für die Bewertungsausführung werden 10.080 Minuten (d.h. sieben Tage) festgelegt. Sie können den Wert außer Kraft setzen und einen Mindestwert von 1.440 Minuten (d.h. einem Tag) festlegen. Der Wert stellt den zwischen aufeinanderfolgenden Bewertungsausführungen erforderlichen Mindestabstand dar. Führen Sie zum Überschreiben des Intervalls die folgenden Schritte aus:

1. Suchen Sie in der Operations Manager-Konsole im Arbeitsbereich **Erstellung** im Bereich **Regeln** die Regel *Microsoft System Center Advisor SCOM Assessment Run Assessment Rule* (Microsoft System Center Advisor-SCOM-Bewertungsregel für die Ausführung der Bewertung).
2. Wählen Sie in den Suchergebnissen die Regel aus, die den Text *Typ: Verwaltungsserver* enthält.
3. Klicken Sie mit der rechten Maustaste auf die Regel, und klicken Sie dann auf **Override the Rule** (Regel außer Kraft setzen) > **Für alle Objekte der Klasse: Verwaltungsserver**.
4. Ändern Sie den Parameterwert **Intervall** in den gewünschten Intervallwert. Im folgenden Beispiel wird der Wert auf 1.440 Minuten (ein Tag) festgelegt.  
    ![Intervallparameter](./media/log-analytics-scom-assessment/interval.png)  
    Wird ein niedrigerer Wert als 1.440 Minuten festgelegt, wird die Regel einmal täglich ausgeführt. In diesem Beispiel wird der Intervallwert ignoriert und ein Ausführungsintervall von einem Tag verwendet.


## <a name="understanding-how-recommendations-are-prioritized"></a>Grundlegendes zum Priorisieren von Empfehlungen

Jede vorgenommene Empfehlung erhält einen Gewichtungswert, der die relative Wichtigkeit der Empfehlung angibt. Es werden nur die zehn wichtigsten Empfehlungen angezeigt.

### <a name="how-weights-are-calculated"></a>Berechnen von Gewichtungen

Gewichtungen sind aggregierte Werte, die auf drei wesentlichen Faktoren basieren:

- Der *Wahrscheinlichkeit* , dass ein erkanntes Problem Schwierigkeiten verursacht. Eine höhere Wahrscheinlichkeit entspricht einer höheren Gesamtwertung für die Empfehlung.
- Der *Auswirkung* des Problems auf Ihre Organisation, wenn es tatsächlich Schwierigkeiten verursacht. Eine stärkere Auswirkung entspricht einer höheren Gesamtwertung für die Empfehlung.
- Dem erforderlichen *Aufwand* zur Umsetzung der Empfehlung. Ein höherer Aufwand entspricht einer niedrigeren Gesamtwertung für die Empfehlung.

Die Gewichtung für jede Empfehlung wird als Prozentsatz der Gesamtwertung ausgedrückt, die für jeden Schwerpunktbereich verfügbar ist. Beispiel: Wenn eine Empfehlung im Schwerpunktbereich „Verfügbarkeit und Geschäftskontinuität“ eine Wertung von fünf Prozent hat, erhöht eine Umsetzung dieser Empfehlung die Gesamtwertung von „Verfügbarkeit und Geschäftskontinuität“ um fünf Prozent.

### <a name="focus-areas"></a>Schwerpunktbereiche

**Verfügbarkeit und Geschäftskontinuität** : Dieser Schwerpunktbereich enthält Empfehlungen hinsichtlich Dienstverfügbarkeit, Infrastrukturstabilität und Schutz des Geschäftsbetriebs.

**Leistung und Skalierbarkeit** : Dieser Schwerpunktbereich zeigt Empfehlungen zum Wachstum Ihrer IT-Infrastruktur sowie zur Sicherstellung, dass Ihre IT-Umgebung die aktuellen Leistungsanforderungen erfüllt und auf veränderte Infrastrukturanforderungen reagieren kann.

**Aktualisierung, Migration und Bereitstellung**: Dieser Schwerpunktbereich zeigt Empfehlungen, mit deren Hilfe Sie SQL Server aktualisieren, migrieren und in Ihrer vorhandenen Infrastruktur bereitstellen können.

**Betrieb und Überwachung**: Dieser Schwerpunktbereich zeigt Empfehlungen zum Optimieren Ihrer IT-Abläufe, Umsetzen vorbeugender Wartungsmaßnahmen und Maximieren der Leistung.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Müssen in jedem Schwerpunktbereich 100 % erzielt werden?

Nicht unbedingt. Die Empfehlungen basieren auf den Kenntnissen und Erfahrungen, die Microsoft-Experten bei Tausenden von Kundenbesuchen gesammelt haben. Jedoch sind keine zwei Serverinfrastrukturen identisch, und spezifische Empfehlungen können mal mehr oder mal weniger relevant für Sie sein. Zum Beispiel sind einige Sicherheitsempfehlungen möglicherweise weniger relevant, wenn Ihre virtuellen Computer nicht mit dem Internet verbunden sind. Verschiedene Verfügbarkeitsempfehlungen können weniger relevant für Dienste sein, die Ad-hoc-Datensammlung und -Berichterstattung mit niedriger Priorität bereitstellen. Probleme, die für ein gewachsenes Unternehmen Relevanz haben, sind für ein Start-up ggf. weniger wichtig. Es empfiehlt sich zu ermitteln, welche Schwerpunktbereiche zu Ihren Prioritäten zählen, und dann zu beobachten, wie sich Ihre Wertungen mit der Zeit verändern.

Jede Empfehlung enthält Informationen dazu, warum sie wichtig ist. Ermitteln Sie anhand dieser Anleitung, ob die Umsetzung der Empfehlung bei Berücksichtigung der Art Ihrer IT-Dienste und der geschäftlichen Anforderungen Ihrer Organisation für Sie geeignet ist.

## <a name="use-assessment-focus-area-recommendations"></a>Befolgen von Empfehlungen anhand der Bewertung des Schwerpunktbereichs

Bevor Sie eine Assessment-Lösung in OMS verwenden können, müssen Sie die Lösung installieren. Weitere Informationen zum Installieren von Lösungen finden Sie unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md). Nach der Installation können Sie die Zusammenfassung der Empfehlungen anzeigen, indem Sie in OMS auf der Seite „Übersicht“ die Kachel „System Center Operations Manager-Bewertung“ verwenden.

Sehen Sie sich die zusammengefassten Compliancebewertungen für Ihre Infrastruktur sowie Details in den Empfehlungen an.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>So werden Empfehlungen für einen Schwerpunktbereich angezeigt und korrigierende Maßnahmen ergriffen

1. Klicken Sie auf der Seite **Übersicht** auf die Kachel **System Center Operations Manager-Bewertung**.
2. Überprüfen Sie auf der Seite **System Center Operations Manager-Bewertung** die Zusammenfassungsinformationen auf einem der Blätter mit Schwerpunktbereichen, und klicken Sie dann auf einen Schwerpunktbereich, um Empfehlungen für diesen Bereich anzuzeigen.
3. Auf jeder der Schwerpunktbereichsseiten können Sie mit Prioritäten versehene Empfehlungen für Ihre Umgebung anzeigen. Klicken Sie unter **Betroffene Objekte** auf eine Empfehlung, um Details zu den Gründen für diese Empfehlung anzuzeigen.  
    ![Schwerpunktbereich](./media/log-analytics-scom-assessment/focus-area.png)
4. Sie können die unter **Vorgeschlagene Aktionen**vorgeschlagenen Korrekturmaßnahmen durchführen. Nachdem das Element behandelt wurde, geben spätere Bewertungen an, dass empfohlene Aktionen ausgeführt wurden, und Ihre Bewertung der Einhaltung erhöht sich. Korrigierte Elemente werden als **Passed Objects**angezeigt.

## <a name="ignore-recommendations"></a>Ignorieren von Empfehlungen

Wenn Sie Empfehlungen ignorieren möchten, können Sie eine Textdatei erstellen, mit der OMS verhindert, dass diese Empfehlungen in Ihren Bewertungsergebnissen angezeigt werden.

### <a name="to-identify-recommendations-that-you-want-to-ignore"></a>So ermitteln Sie Empfehlungen, die Sie ignorieren möchten

1. Melden Sie sich bei Ihrem Arbeitsbereich an, und öffnen Sie die Protokollsuche. Verwenden Sie folgende Abfrage, um Empfehlungen aufzulisten, die für Computer in Ihrer Umgebung nicht funktionieren.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Dieser Screenshot zeigt eine Protokollsuchabfrage:   
    ![Protokollsuche](./media/log-analytics-scom-assessment/scom-log-search.png)

2. Wählen Sie die Empfehlungen aus, die Sie ignorieren möchten. Sie verwenden die Werte für „RecommendationId“ in der nächsten Prozedur.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Erstellen und Verwenden einer IgnoreRecommendations.txt-Textdatei

1. Erstellen Sie eine Datei namens IgnoreRecommendations.txt.
2. Fügen oder geben Sie auf separaten Zeilen die RecommendationId für jede Empfehlung ein, die OMS ignorieren soll, und speichern und schließen Sie die Datei.
3. Legen Sie die Datei auf jedem Computer, auf dem OMS die Empfehlungen ignorieren soll, in folgendem Ordner ab.
4. Auf dem Operations Manager-Verwaltungsserver – *Systemlaufwerk*:\Programme\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Überprüfen, ob Empfehlungen ignoriert werden

1. Nach Ausführung der nächsten geplanten Bewertung (standardmäßig alle 7 Tage) werden die angegebenen Empfehlungen als „Ignoriert“ gekennzeichnet und nicht auf dem Bewertungsdashboard angezeigt.
2. Sie können folgende Protokollsuchabfragen verwenden, um alle ignorierten Empfehlungen aufzulisten.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

3. Wenn Sie sich später dazu entscheiden, die ignorierten Empfehlungen anzuzeigen, entfernen Sie alle IgnoreRecommendations.txt-Dateien, oder entfernen Sie einzelne RecommendationIDs aus den Dateien.

## <a name="system-center-operations-manager-assessment-solution-faq"></a>System Center Operations Manager-Bewertung – häufig gestellte Fragen

*Gibt es eine Möglichkeit, die Häufigkeit der Bewertung zu konfigurieren?* Ja. Lesen Sie die Informationen unter [Konfigurieren der Ausführungshäufigkeit](#configure-the-run-frequency).

*Wird ein anderer Server, der erst nach dem Hinzufügen der System Center Operations Manager-Bewertungslösung erkannt wird, auch bewertet?* Ja. Er wird ab dem Zeitpunkt der Erkennung bewertet (standardmäßig alle sieben Tage).

*Wie lautet der Name des Prozesses, der die Daten sammelt?* AdvisorAssessment.exe

*Wo wird der Prozess „AdvisorAssessment.exe“ ausgeführt?* „AdvisorAssessment.exe“ wird unter „HealthService“ des Verwaltungsservers ausgeführt, auf dem die Bewertungsregel aktiviert ist. Mithilfe dieses Prozesses wird die Ermittlung der gesamten Umgebung über Remotedatensammlung erreicht.

*Wie lange dauert die Datensammlung?* Die Datensammlung auf dem Server dauert etwa eine Stunde. In Umgebungen mit zahlreichen Operations Manager-Instanzen oder -Datenbanken kann sie länger dauern.

*Was geschieht, wenn ich das Intervall der Bewertung auf einen niedrigeren Wert als 1.440 Minuten festlege?* Die Bewertung ist so vorkonfiguriert, dass sie maximal ein Mal pro Tag ausgeführt wird. Wenn Sie für das Intervall einen niedrigeren Wert als 1.440 Minuten festlegen, verwendet die Bewertung 1.440 Minuten als Intervallwert.

*Wie erkenne ich, dass Fehler bei Voraussetzungen aufgetreten sind?* Wenn die Bewertung ausgeführt wurde und keine Ergebnisse angezeigt werden, sind bei einigen der Voraussetzungen für die Bewertung wahrscheinlich Fehler aufgetreten. Sie können die Abfragen `Type=Operation Solution=SCOMAssessment` und `Type=SCOMAssessmentRecommendation FocusArea=Prerequisites` in der Protokollsuche ausführen, um Fehler bei Voraussetzungen anzuzeigen.

*Bei Fehlern bei Voraussetzungen wird die Meldung `Failed to connect to the SQL Instance (….).` angezeigt. Was ist das Problem?* „AdvisorAssessment.exe“, der Prozess zur Datensammlung, wird unter „HealthService“ des Verwaltungsservers ausgeführt. Der Prozess versucht im Rahmen der Bewertung eine Verbindung mit der SQL Server-Instanz herzustellen, in der sich die Operations Manager-Datenbank befindet. Dieser Fehler kann auftreten, wenn Firewallregeln die Verbindung mit der SQL Server-Instanz blockieren.

*Welche Art von Daten wird gesammelt?* Die folgenden Datentypen werden über Windows PowerShell, SQL-Abfragen und Dateiinformationssammler gesammelt: - WMI-Daten - Registrierungsdaten - EventLog-Daten - Operations Manager-Daten

*Warum muss ich ein ausführendes Konto konfigurieren?* Für einen Operations Manager-Server werden verschiedene SQL-Abfragen ausgeführt. Damit sie ausgeführt werden können, müssen Sie ein ausführendes Konto mit den erforderlichen Berechtigungen verwenden. Zum Abfragen von WMI sind darüber hinaus lokale Administratorrechte erforderlich.

*Warum werden nur die ersten 10 Empfehlungen angezeigt?* Anstatt Ihnen eine umfangreiche und erdrückende Aufgabenliste zu präsentieren, empfehlen wir, sich zuerst auf die Empfehlungen mit hoher Priorität zu konzentrieren. Nach deren Umsetzung werden weitere Empfehlungen verfügbar. Wenn Sie jedoch lieber die ganze Liste mit allen Einzelheiten anzeigen möchten, können Sie mithilfe der Protokollsuche alle Empfehlungen anzeigen.

*Gibt es eine Möglichkeit, eine Empfehlung zu ignorieren?* Ja. Lesen Sie die Informationen unter [Ignorieren von Empfehlungen](#Ignore-recommendations).


## <a name="next-steps"></a>Nächste Schritte

- [Durchsuchen von Protokollen](log-analytics-log-searches.md), um detaillierte System Center Operations Manager-Bewertungsdaten und -empfehlungen anzuzeigen.



<!--HONumber=Nov16_HO3-->


