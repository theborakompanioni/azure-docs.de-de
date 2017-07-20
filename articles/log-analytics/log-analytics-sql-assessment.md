---
title: Optimieren Ihrer SQL Server-Umgebung mit Azure Log Analytics | Microsoft-Dokumentation
description: "Mit Azure Log Analytics können Sie die SQL Assessment-Lösung verwenden, um die Risiken und die Integrität Ihrer SQL Server-Umgebungen in regelmäßigen Abständen zu bewerten."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e297eb57-1718-4cfe-a241-b9e84b2c42ac
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 74f34bdbf5707510c682814716aa0b95c19a5503
ms.openlocfilehash: 13453a831c3d42a9e291976a4d5cc80ced5c4755
ms.contentlocale: de-de
ms.lasthandoff: 06/09/2017


---
# <a name="optimize-your-sql-server-environment-with-the-sql-assessment-solution-in-log-analytics"></a>Optimieren der SQL Server-Umgebung mit der SQL Assessment-Lösung in Log Analytics

![Symbol für die SQL-Bewertung](./media/log-analytics-sql-assessment/sql-assessment-symbol.png)

Sie können die SQL Assessment-Lösung verwenden, um die Risiken und die Integrität Ihrer Serverumgebungen in regelmäßigen Abständen zu bewerten. Dieser Artikel unterstützt Sie beim Installieren der Lösung, sodass Sie bei potenziellen Problemen korrigierende Maßnahmen ergreifen können.

Die Lösung bietet eine priorisierte Liste von Empfehlungen, die spezifisch für Ihre bereitgestellte Serverinfrastruktur gelten. Die Empfehlungen sind in sechs Schwerpunktbereiche eingeteilt, mit deren Hilfe Sie die Risiken schnell einschätzen und korrigierende Maßnahmen ergreifen können.

Die Empfehlungen basieren auf den Kenntnissen und Erfahrungen, die Microsoft-Experten bei Tausenden von Kundenbesuchen gesammelt haben. Jede Empfehlung enthält Informationen, warum ein Problem ggf. für Sie relevant ist und wie Sie die vorgeschlagenen Änderungen umsetzen können.

Sie können Schwerpunktbereiche wählen, die für Ihre Organisation am wichtigsten sind, und Ihren Fortschritt in Richtung einer risiko- und fehlerfreien Umgebung nachverfolgen.

Nachdem Sie die Lösung hinzugefügt haben und eine Bewertung durchgeführt wurde, werden zusammenfassende Informationen für Schwerpunktbereiche im Dashboard **SQL Assessment** für die Infrastruktur in Ihrer Umgebung angezeigt. In den folgenden Abschnitten wird beschrieben, wie Sie die Informationen im Dashboard **SQL Assessment** anzeigen und dann die empfohlenen Maßnahmen für Ihre SQL-Serverinfrastruktur durchführen können.

![Abbildung der Kachel "SQL Assessment"](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![Abbildung des Dashboards "SQL-Assessment"](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-the-solution"></a>Installieren und Konfigurieren der Lösung
SQL Assessment funktioniert mit allen derzeit unterstützten Versionen von SQL-Server für die Editionen Standard, Developer und Enterprise.

Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

* Agents müssen auf Servern installiert werden, auf denen SQL Server installiert ist.
* Für die SQL Assessment-Lösung muss eine unterstützte Version von .NET Framework 4 auf jedem Computer installiert sein, der über einen OMS-Agent verfügt.
* Zum Installieren der Lösung muss der Benutzer bei Verwendung des Azure-Portals ein Administrator oder Mitwirkender des Azure-Abonnements sein. Außerdem muss der Benutzer Mitglied der Rolle „Mitwirkender“ oder „Administrator“ des OMS-Arbeitsbereichs im OMS-Portal sein.
* Wenn Sie den Operations Manager-Agent mit SQL Assessment einsetzen möchten, müssen Sie ein ausführendes Operations Manager-Konto verwenden. Weitere Informationen finden Sie unter [Ausführende Operations Manager-Konten für OMS](#operations-manager-run-as-accounts-for-oms) .

  > [!NOTE]
  > Der MMA-Agent unterstützt ausführende Operations Manager-Konten nicht.
  >
  >
* Fügen Sie mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md)beschriebenen Prozesses die SQL Assessment-Lösung Ihrem OMS-Arbeitsbereich hinzu. Es ist keine weitere Konfiguration erforderlich.

> [!NOTE]
> Nachdem Sie die Lösung hinzugefügt haben, wird die Datei AdvisorAssessment.exe den Servern mit Agents hinzugefügt. Konfigurationsdaten werden gelesen und dann zur Verarbeitung an den OMS-Dienst in der Cloud gesendet. Auf die empfangenen Daten wird Logik angewendet, und der Clouddienst zeichnet die Daten auf.

## <a name="sql-assessment-data-collection-details"></a>Details zur SQL Assessment-Datensammlung
SQL Assessment erfasst WMI-Daten, Registrierungsdaten, Leistungsdaten und Ergebnisse der dynamischen Verwaltungssicht von SQL Server mithilfe von Agents, die Sie aktiviert haben.

Die folgende Tabelle zeigt die Datensammlungsmethoden für Agents und gibt an, ob Operations Manager (SCOM) erforderlich ist und wie häufig Daten von einem Agent gesammelt werden.

| Plattform | Direkt-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |![Ja](./media/log-analytics-sql-assessment/oms-bullet-green.png) |![Ja](./media/log-analytics-sql-assessment/oms-bullet-green.png) |![Nein](./media/log-analytics-sql-assessment/oms-bullet-red.png) |![Nein](./media/log-analytics-sql-assessment/oms-bullet-red.png) |![Ja](./media/log-analytics-sql-assessment/oms-bullet-green.png) |7 Tage |

## <a name="operations-manager-run-as-accounts-for-oms"></a>Ausführende Operations Manager-Konten für OMS
Log Analytics in OMS verwendet den Agent und die Verwaltungsgruppe von Operations Manager, um Daten zu sammeln und an den OMS-Dienst zu senden. OMS basiert auf Management Packs für Workloads, um Dienste bereitzustellen, die einen Mehrwert schaffen. Jede Arbeitsauslastung erfordert spezifische Berechtigungen zum Ausführen von Management Packs in einem anderen Sicherheitskontext, z. B. ein Domänenkonto. Sie müssen Anmeldeinformationen angeben, indem Sie ein ausführendes Operations Manager-Konto konfigurieren.

Verwenden Sie die folgenden Informationen, um das ausführende Operations Manager-Konto für SQL Assessment festzulegen.

### <a name="set-the-run-as-account-for-sql-assessment"></a>Festlegen des ausführenden Kontos für SQL Assessment
 Wenn Sie bereits das SQL Server Management Pack verwenden, sollten Sie das entsprechende ausführende Konto verwenden.

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a>So konfigurieren Sie das ausführende SQL-Konto in der Betriebskonsole
> [!NOTE]
> Wenn Sie den direkten OMS-Agenten statt des SCOM-Agenten verwenden, wird das Management Pack immer im Sicherheitskontext des lokalen Systemkontos ausgeführt. Überspringen Sie die Schritte 1 bis 5 unten, und führen Sie entweder das T-SQL- oder das Powershell-Beispiel aus, wobei Sie „NT AUTHORITY\SYSTEM“ als Benutzernamen angeben.
>
>

1. Öffnen Sie in Operations Manager die Betriebskonsole, und klicken Sie dann auf **Verwaltung**.
2. Klicken Sie unter **Ausführen als-Konfiguration** auf **Profile**, und öffnen Sie das **ausführende Profil für OMS SQL Assessment**.
3. Klicken Sie auf der Seite **Ausführende Konten** auf **Hinzufügen**.
4. Wählen Sie ein ausführendes Windows-Konto aus, das die Anmeldeinformationen für SQL Server enthält, oder klicken Sie auf **Neu** , um eines zu erstellen.

   > [!NOTE]
   > Der Typ des ausführenden Kontos muss "Windows" sein. Das ausführende Konto muss auch Teil der lokalen Administratorengruppe auf allen Windows-Servern sein, auf denen SQL Server-Instanzen gehostet werden.
   >
   >
5. Klicken Sie auf **Speichern**.
6. Ändern und führen Sie dann das folgende T-SQL-Beispiel auf jeder SQL Server-Instanz aus, um dem ausführenden Konto die erforderlichen Mindestberechtigungen zum Ausführen von SQL Assessment zu erteilen. Dies ist jedoch nicht erforderlich, wenn ein ausführendes Konto bereits Teil der Serverrolle "sysadmin" auf SQL Server-Instanzen ist.

```
---
    -- Replace <UserName> with the actual user name being used as Run As Account.
    USE master

    -- Create login for the user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions to user.
    GRANT VIEW SERVER STATE TO [<UserName>]
    GRANT VIEW ANY DEFINITION TO [<UserName>]
    GRANT VIEW ANY DATABASE TO [<UserName>]

    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a>So konfigurieren Sie das ausführende SQL-Konto mithilfe von Windows PowerShell
Öffnen Sie ein PowerShell-Fenster, und führen Sie das folgende Skript aus, nachdem Sie es mit Ihren Informationen aktualisiert haben:

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

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

**Aktualisierung, Migration und Bereitstellung**: Dieser Schwerpunktbereich zeigt Empfehlungen, mit deren Hilfe Sie SQL Server aktualisieren, migrieren und in Ihrer vorhandenen Infrastruktur bereitstellen können.

**Betrieb und Überwachung**: Dieser Schwerpunktbereich zeigt Empfehlungen zum Optimieren Ihrer IT-Abläufe, Umsetzen vorbeugender Wartungsmaßnahmen und Maximieren der Leistung.

**Änderungs- und Konfigurationsverwaltung**: Dieser Schwerpunktbereich zeigt Empfehlungen dazu, wie Sie den täglichen Betrieb schützen, sicherstellen, dass Änderungen sich nicht negativ auf die Infrastruktur auswirken, Verfahren zur Änderungssteuerung einrichten und Systemkonfigurationen nachverfolgen und überwachen.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Müssen in jedem Schwerpunktbereich 100 % erzielt werden?
Nicht unbedingt. Die Empfehlungen basieren auf den Kenntnissen und Erfahrungen, die Microsoft-Experten bei Tausenden von Kundenbesuchen gesammelt haben. Jedoch sind keine zwei Serverinfrastrukturen identisch, und spezifische Empfehlungen können mal mehr oder mal weniger relevant für Sie sein. Zum Beispiel sind einige Sicherheitsempfehlungen möglicherweise weniger relevant, wenn Ihre virtuellen Computer nicht mit dem Internet verbunden sind. Verschiedene Verfügbarkeitsempfehlungen können weniger relevant für Dienste sein, die Ad-hoc-Datensammlung und -Berichterstattung mit niedriger Priorität bereitstellen. Probleme, die für ein gewachsenes Unternehmen Relevanz haben, sind für ein Start-up ggf. weniger wichtig. Es empfiehlt sich zu ermitteln, welche Schwerpunktbereiche zu Ihren Prioritäten zählen, und dann zu beobachten, wie sich Ihre Wertungen mit der Zeit verändern.

Jede Empfehlung enthält Informationen dazu, warum sie wichtig ist. Sie sollten anhand dieser Anleitung feststellen, ob die Umsetzung der Empfehlung bei Berücksichtigung der Art Ihrer IT-Dienste und der geschäftlichen Anforderungen Ihrer Organisation für Sie geeignet ist.

## <a name="use-assessment-focus-area-recommendations"></a>Befolgen von Empfehlungen anhand der Bewertung des Schwerpunktbereichs
Bevor Sie eine Assessment-Lösung in OMS verwenden können, müssen Sie die Lösung installieren. Weitere Informationen zum Installieren von Lösungen finden Sie unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md). Nach der Installation können Sie die Zusammenfassung der Empfehlungen anzeigen, indem Sie in OMS auf der Übersichtsseite die Kachel „SQL Assessment“ auswählen.

Sehen Sie sich die zusammengefassten Compliancebewertungen für Ihre Infrastruktur sowie Details in den Empfehlungen an.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>So werden Empfehlungen für einen Schwerpunktbereich angezeigt und korrigierende Maßnahmen ergriffen
1. Klicken Sie auf der Seite **Übersicht** auf die Kachel **SQL Assessment**.
2. Überprüfen Sie auf der Seite **SQL Assessment** die Zusammenfassungsinformationen auf einem der Blätter mit Schwerpunktbereichen, und klicken Sie dann auf einen Schwerpunktbereich, um Empfehlungen für diesen Bereich anzuzeigen.
3. Auf jeder der Schwerpunktbereichsseiten können Sie mit Prioritäten versehene Empfehlungen für Ihre Umgebung anzeigen. Klicken Sie unter **Betroffene Objekte** auf eine Empfehlung, um Details zu den Gründen für diese Empfehlung anzuzeigen.  
    ![Abbildung der Empfehlungen zu „SQL Assessment“](./media/log-analytics-sql-assessment/sql-assess-focus.png)
4. Sie können die unter **Vorgeschlagene Aktionen**vorgeschlagenen Korrekturmaßnahmen durchführen. Nachdem das Element behandelt wurde, geben spätere Bewertungen an, dass empfohlene Aktionen ausgeführt wurden, und Ihre Bewertung der Einhaltung erhöht sich. Korrigierte Elemente werden als **Passed Objects**angezeigt.

## <a name="ignore-recommendations"></a>Ignorieren von Empfehlungen
Wenn Sie Empfehlungen ignorieren möchten, können Sie eine Textdatei erstellen, die OMS verwendet, um zu verhindern, dass diese Empfehlungen in Ihren Bewertungsergebnissen angezeigt werden.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Ermitteln von Empfehlungen, die Sie ignorieren möchten
1. Melden Sie sich bei Ihrem Arbeitsbereich an, und öffnen Sie die Protokollsuche. Verwenden Sie folgende Abfrage, um Empfehlungen aufzulisten, die für Computer in Ihrer Umgebung nicht funktionieren.

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```

   Dieser Screenshot zeigt eine Protokollsuchabfrage: ![Empfehlungen mit Fehlern](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png).
2. Wählen Sie die Empfehlungen aus, die Sie ignorieren möchten. Sie werden die Werte für RecommendationId in der nächsten Prozedur verwenden.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Erstellen und Verwenden einer IgnoreRecommendations.txt-Textdatei
1. Erstellen Sie eine Datei namens IgnoreRecommendations.txt.
2. Fügen oder geben Sie auf separaten Zeilen die RecommendationId für jede Empfehlung ein, die OMS ignorieren soll, und speichern und schließen Sie die Datei.
3. Legen Sie die Datei auf jedem Computer, auf dem OMS die Empfehlungen ignorieren soll, in folgendem Ordner ab.
   * Auf Computern mit Microsoft Monitoring Agent (direkt oder über Operations Manager verbunden) – *Systemlaufwerk*:\Programme\Microsoft Monitoring Agent\Agent
   * Auf dem Operations Manager-Verwaltungsserver – *Systemlaufwerk*:\Programme\Microsoft System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Überprüfen, ob Empfehlungen ignoriert werden
1. Nach Ausführung der nächsten geplanten Bewertung – standardmäßig alle 7 Tage – werden die angegebenen Empfehlungen als „Ignoriert“ gekennzeichnet und auf dem Bewertungsdashboard nicht angezeigt.
2. Sie können folgende Protokollsuchabfragen verwenden, um alle ignorierten Empfehlungen aufzulisten.

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
3. Wenn Sie sich später dazu entscheiden, die ignorierten Empfehlungen anzuzeigen, entfernen Sie alle IgnoreRecommendations.txt-Dateien, oder entfernen Sie einzelne RecommendationIDs aus den Dateien.

## <a name="sql-assessment-solution-faq"></a>SQL Assessment-Lösungen – häufig gestellte Fragen
*Wie oft erfolgt eine Bewertung?*

* Die Bewertung erfolgt alle sieben Tage.

*Gibt es eine Möglichkeit, die Häufigkeit der Bewertung zu konfigurieren?*

* Derzeit leider nicht.

*Wird ein anderer Server, der erst nach dem Hinzufügen der SQL Assessment-Lösung erkannt wird, auch bewertet?*

* Ja. Ab dem Zeitpunkt der Erkennung wird er alle sieben Tage bewertet.

*Wann wird ein Server, der außer Betrieb genommen wird, aus der Bewertung entfernt?*

* Ein Server, der drei Wochen keine Daten übertragen hat, wird entfernt.

*Wie lautet der Name des Prozesses, der die Daten sammelt?*

* AdvisorAssessment.exe

*Wie lange dauert das Sammeln der Daten?*

* Die eigentliche Datensammlung auf dem Server dauert etwa eine Stunde. Auf Servern mit vielen SQL-Instanzen oder -Datenbanken kann der Vorgang auch mehr Zeit in Anspruch nehmen.

*Welche Art von Daten wird gesammelt?*

* Die folgenden Datentypen werden gesammelt:
  * WMI
  * Registrierung
  * Leistungsindikatoren
  * Dynamische SQL-Verwaltungssichten

*Gibt es eine Möglichkeit, den Zeitpunkt der Datensammlung zu konfigurieren?*

* Derzeit leider nicht.

*Warum muss ich ein ausführendes Konto konfigurieren?*

* Für SQL Server werden nur wenige SQL-Abfragen ausgeführt. Damit diese ausgeführt werden können, muss ein ausführendes Konto mit der SQL-Berechtigung "Serverstatus anzeigen" verwendet werden.  Zum Abfragen von WMI sind darüber hinaus lokale Administratorrechte erforderlich.

*Warum werden nur die ersten 10 Empfehlungen angezeigt?*

* Anstatt Ihnen eine umfangreiche und erdrückende Aufgabenliste zu präsentieren, empfehlen wir, sich zuerst auf die Empfehlungen mit hoher Priorität zu konzentrieren. Nach deren Umsetzung werden weitere Empfehlungen verfügbar. Wenn Sie jedoch lieber die ganze Liste mit allen Einzelheiten anzeigen möchten, können Sie mithilfe der OMS-Protokollsuche alle Empfehlungen anzeigen.

*Gibt es eine Möglichkeit, eine Empfehlung zu ignorieren?*

* Ja, siehe oben stehenden Abschnitt [Ignorieren von Empfehlungen](#ignore-recommendations) .

## <a name="next-steps"></a>Nächste Schritte
* [Protokollsuche](log-analytics-log-searches.md) , um detaillierte SQL Assessment-Daten und -Empfehlungen anzuzeigen.

