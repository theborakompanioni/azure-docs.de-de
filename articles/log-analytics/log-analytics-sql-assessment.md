<properties
	pageTitle="Optimieren der Umgebung mit der SQL Assessment-Lösung in Log Analytics | Microsoft Azure"
	description="Sie können die SQL Assessment-Lösung verwenden, um die Risiken und die Integrität Ihrer Serverumgebungen in regelmäßigen Abständen zu bewerten.."
	services="log-analytics"
	documentationCenter=""
	authors="bandersmsft"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="log-analytics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/28/2016"
	ms.author="banders"/>

# Optimieren der Umgebung mit der SQL Assessment-Lösung in Log Analytics


Sie können die SQL Assessment-Lösung verwenden, um die Risiken und die Integrität Ihrer Serverumgebungen in regelmäßigen Abständen zu bewerten.. Dieser Artikel unterstützt Sie beim Installieren der Lösung, sodass Sie bei potenziellen Problemen korrigierende Maßnahmen ergreifen können.

Die Lösung bietet eine priorisierte Liste von Empfehlungen, die spezifisch für Ihre bereitgestellte Serverinfrastruktur gelten. Die Empfehlungen sind in sechs Schwerpunktbereiche eingeteilt, mit deren Hilfe Sie die Risiken schnell einschätzen und korrigierende Maßnahmen ergreifen können.

Die Empfehlungen basieren auf den Kenntnissen und Erfahrungen, die Microsoft-Experten bei Tausenden von Kundenbesuchen gesammelt haben. Jede Empfehlung enthält Informationen, warum ein Problem ggf. für Sie relevant ist und wie Sie die vorgeschlagenen Änderungen umsetzen können.

Sie können Schwerpunktbereiche wählen, die für Ihre Organisation am wichtigsten sind, und Ihren Fortschritt in Richtung einer risiko- und fehlerfreien Umgebung nachverfolgen.

Nachdem Sie die Lösung hinzugefügt haben und eine Bewertung durchgeführt wurde, werden zusammenfassende Informationen für Schwerpunktbereiche im Dashboard **SQL Assessment** für die Infrastruktur in Ihrer Umgebung angezeigt. In den folgenden Abschnitten wird beschrieben, wie Sie die Informationen im Dashboard **SQL Assessment** anzeigen und dann die empfohlenen Maßnahmen für Ihre SQL-Serverinfrastruktur durchführen können.

![Abbildung der Kachel "SQL Assessment"](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![Abbildung des Dashboards "SQL-Assessment"](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## Installieren und Konfigurieren der Lösung
SQL Assessment funktioniert mit allen derzeit unterstützten Versionen von SQL-Server für die Editionen Standard, Developer und Enterprise.

Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

- Für die SQL Assessment-Lösung muss .NET Framework 4 auf jedem Computer installiert sein, der über einen OMS-Agent verfügt.
- Wenn Sie den Operations Manager-Agent mit SQL Assessment einsetzen möchten, müssen Sie ein ausführendes Operations Manager-Konto verwenden. Weitere Informationen finden Sie unter [Ausführende Operations Manager-Konten für OMS](#operations-manager-run-as-accounts-for-oms).

    >[AZURE.NOTE] Der MMA-Agent unterstützt ausführende Operations Manager-Konten nicht.

- Fügen Sie mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses die SQL Assessment-Lösung zu Ihrem OMS-Arbeitsbereich hinzu. Es ist keine weitere Konfiguration erforderlich.

>[AZURE.NOTE] Nachdem Sie die Lösung hinzugefügt haben, wird die Datei AdvisorAssessment.exe den Servern mit Agents hinzugefügt. Konfigurationsdaten werden gelesen und dann zur Verarbeitung an den OMS-Dienst in der Cloud gesendet. Auf die empfangenen Daten wird Logik angewendet, und der Clouddienst zeichnet die Daten auf.

## Details zur SQL Assessment-Datensammlung

Die folgende Tabelle zeigt die Datensammlungsmethoden für Agents und gibt an, ob Operations Manager (SCOM) erforderlich ist und wie häufig Daten von einem Agent gesammelt werden.

| Plattform | Direkt-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-sql-assessment/oms-bullet-green.png)|![Ja](./media/log-analytics-sql-assessment/oms-bullet-green.png)|![Nein](./media/log-analytics-sql-assessment/oms-bullet-red.png)|	![Nein](./media/log-analytics-sql-assessment/oms-bullet-red.png)|![Ja](./media/log-analytics-sql-assessment/oms-bullet-green.png)|	7 Tage|

## Ausführende Operations Manager-Konten für OMS

Log Analytics in OMS verwendet den Agent und die Verwaltungsgruppe von Operations Manager, um Daten zu sammeln und an den OMS-Dienst zu senden. OMS basiert auf Management Packs für Workloads, um Dienste bereitzustellen, die einen Mehrwert schaffen. Jede Arbeitsauslastung erfordert spezifische Berechtigungen zum Ausführen von Management Packs in einem anderen Sicherheitskontext, z. B. ein Domänenkonto. Sie müssen Anmeldeinformationen angeben, indem Sie ein ausführendes Operations Manager-Konto konfigurieren.

Verwenden Sie die folgenden Informationen, um das ausführende Operations Manager-Konto für SQL Assessment festzulegen.

### Festlegen des ausführenden Kontos für SQL Assessment

 Wenn Sie bereits das SQL Server Management Pack verwenden, sollten Sie das entsprechende ausführende Konto verwenden.

#### So konfigurieren Sie das ausführende SQL-Konto in der Betriebskonsole

1. Öffnen Sie in Operations Manager die Betriebskonsole, und klicken Sie dann auf **Verwaltung**.

2. Klicken Sie unter **Ausführen als-Konfiguration** auf **Profile**, und öffnen Sie das **ausführende Profil für OMS SQL Assessment**.

3. Klicken Sie auf der Seite **Ausführende Konten** auf **Hinzufügen**.

4. Wählen Sie ein ausführendes Windows-Konto aus, das die Anmeldeinformationen für SQL Server enthält, oder klicken Sie auf **Neu**, um eines zu erstellen.
	>[AZURE.NOTE] Der Typ des ausführenden Kontos muss "Windows" sein. Das ausführende Konto muss auch Teil der lokalen Administratorengruppe auf allen Windows-Servern sein, auf denen SQL Server-Instanzen gehostet werden.

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
#### To configure the SQL Run As account using Windows PowerShell

Open a PowerShell window and run the following script after you’ve updated it with your information:

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"
     
    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## Understanding how recommendations are prioritized

Every recommendation made is given a weighting value that identifies the relative importance of the recommendation. Only the ten most important recommendations are shown.

### How weights are calculated

Weightings are aggregate values based on three key factors:

- The *probability* that an issue identified will cause problems. A higher probability equates to a larger overall score for the recommendation.

- The *impact* of the issue on your organization if it does cause a problem. A higher impact equates to a larger overall score for the recommendation.

- The *effort* required to implement the recommendation. A higher effort equates to a smaller overall score for the recommendation.

The weighting for each recommendation is expressed as a percentage of the total score available for each focus area. For example, if a recommendation in the Security and Compliance focus area has a score of 5%, implementing that recommendation will increase your overall Security and Compliance score by 5%.

### Focus areas

**Security and Compliance** - This focus area shows recommendations for potential security threats and breaches, corporate policies, and technical, legal and regulatory compliance requirements.

**Availability and Business Continuity** - This focus area shows recommendations for service availability, resiliency of your infrastructure, and business protection.

**Performance and Scalability** - This focus area shows recommendations to help your organization's IT infrastructure grow, ensure that your IT environment meets current performance requirements, and is able to respond to changing infrastructure needs.

**Upgrade, Migration and Deployment** - This focus area shows recommendations to help you upgrade, migrate, and deploy SQL Server to your existing infrastructure.

**Operations and Monitoring** - This focus area shows recommendations to help streamline your IT operations, implement preventative maintenance, and maximize performance.

**Change and Configuration Management** - This focus area shows recommendations to help protect day-to-day operations, ensure that changes don't negatively affect your infrastructure, establish change control procedures, and to track and audit system configurations.

### Should you aim to score 100% in every focus area?

Not necessarily. The recommendations are based on the knowledge and experiences gained by Microsoft engineers across thousands of customer visits. However, no two server infrastructures are the same, and specific recommendations may be more or less relevant to you. For example, some security recommendations might be less relevant if your virtual machines are not exposed to the Internet. Some availability recommendations may be less relevant for services that provide low priority ad hoc data collection and reporting. Issues that are important to a mature business may be less important to a start-up. You may want to identify which focus areas are your priorities and then look at how your scores change over time.

Every recommendation includes guidance about why it is important. You should use this guidance to evaluate whether implementing the recommendation is appropriate for you, given the nature of your IT services and the business needs of your organization.

## Use assessment focus area recommendations

Before you can use an assessment solution in OMS, you must have the solution installed. To read more about installing solutions, see [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md). After it is installed, you can view the summary of recommendations by using the SQL Assessment tile on the Overview page in OMS.

View the summarized compliance assessments for your infrastructure and then drill-into recommendations.

### To view recommendations for a focus area and take corrective action

1. On the **Overview** page, click the **SQL Assessment** tile.
2. On the **SQL Assessment** page, review the summary information in one of the focus area blades and then click one to view recommendations for that focus area.
3. On any of the focus area pages, you can view the prioritized recommendations made for your environment. Click a recommendation under **Affected Objects** to view details about why the recommendation is made.  
    ![image of SQL Assessment recommendations](./media/log-analytics-sql-assessment/sql-assess-focus.png)
4. You can take corrective actions suggested in **Suggested Actions**. When the item has been addressed, later assessments will record that recommended actions were taken and your compliance score will increase. Corrected items appear as **Passed Objects**.

## Ignore recommendations

If you have recommendations that you want to ignore, you can create a text file that OMS will use to prevent recommendations from appearing in your assessment results.

### To identify recommendations that you will ignore

1.	Sign in to your workspace and open Log Search. Use the following query to list recommendations that have failed for computers in your environment.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Here's a screen shot showing the Log Search query:
    ![failed recommendations](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)

2.	Wählen Sie die Empfehlungen aus, die Sie ignorieren möchten. Sie werden die Werte für RecommendationId in der nächsten Prozedur verwenden.


### Erstellen und Verwenden einer IgnoreRecommendations.txt-Textdatei

1.	Erstellen Sie eine Datei namens IgnoreRecommendations.txt.
2.	Fügen oder geben Sie auf separaten Zeilen die RecommendationId für jede Empfehlung ein, die OMS ignorieren soll, und speichern und schließen Sie die Datei.
3.	Legen Sie die Datei auf jedem Computer, auf dem OMS die Empfehlungen ignorieren soll, in folgendem Ordner ab.
    - Auf Computern mit dem Microsoft Monitoring Agent (direkt verbunden oder über Operations Manager) – *Systemlaufwerk*:\\Programme\\Microsoft Monitoring Agent\\Agent
    - Auf dem Operations Manager-Verwaltungsserver – *Systemlaufwerk*:\\Programme\\Microsoft System Center 2012 R2\\Operations Manager\\Server

### Überprüfen, ob Empfehlungen ignoriert werden

1.	Nach Ausführung der nächsten geplanten Bewertung – standardmäßig alle 7 Tage – werden die angegebenen Empfehlungen als „Ignoriert“ gekennzeichnet und auf dem Bewertungsdashboard nicht angezeigt.
2.	Sie können folgende Protokollsuchabfragen verwenden, um alle ignorierten Empfehlungen aufzulisten.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```
3.	Wenn Sie sich später dazu entscheiden, die ignorierten Empfehlungen anzuzeigen, entfernen Sie alle IgnoreRecommendations.txt-Dateien, oder entfernen Sie einzelne RecommendationIDs aus den Dateien.

## SQL Assessment-Lösungen – häufig gestellte Fragen

*Wie oft erfolgt eine Bewertung?*
- Die Bewertung erfolgt alle sieben Tage.

*Gibt es eine Möglichkeit, die Häufigkeit der Bewertung zu konfigurieren?*
- Derzeit leider nicht.

*Wird ein anderer Server, der erst nach dem Hinzufügen der SQL Assessment-Lösung erkannt wird, auch bewertet?*
- Ja. Ab dem Zeitpunkt der Erkennung wird er alle sieben Tage bewertet.

*Wann wird ein Server, der außer Betrieb genommen wird, aus der Bewertung entfernt?*
- Ein Server, der drei Wochen keine Daten übertragen hat, wird entfernt.

*Wie lautet der Name des Prozesses, der die Daten sammelt?*
- AdvisorAssessment.exe

*Wie lange dauert das Sammeln der Daten?*
- Die eigentliche Datensammlung auf dem Server dauert etwa eine Stunde. Auf Servern mit vielen SQL-Instanzen oder -Datenbanken kann der Vorgang auch mehr Zeit in Anspruch nehmen.

*Welche Art von Daten wird gesammelt?*
- Die folgenden Datentypen werden gesammelt:
    - WMI
    - Registrierung
    - Leistungsindikatoren
    - Dynamische SQL-Verwaltungssichten

*Gibt es eine Möglichkeit, den Zeitpunkt der Datensammlung zu konfigurieren?*
- Derzeit leider nicht.

*Warum muss ich ein ausführendes Konto konfigurieren?*
- Für SQL Server werden nur wenige SQL-Abfragen ausgeführt. Damit diese ausgeführt werden können, muss ein ausführendes Konto mit der SQL-Berechtigung "Serverstatus anzeigen" verwendet werden. Zum Abfragen von WMI sind darüber hinaus lokale Administratorrechte erforderlich.

*Warum werden nur die ersten 10 Empfehlungen angezeigt?*
- Anstatt Ihnen eine umfangreiche und erdrückende Aufgabenliste zu präsentieren, empfehlen wir, sich zuerst auf die Empfehlungen mit hoher Priorität zu konzentrieren. Nach deren Umsetzung werden weitere Empfehlungen verfügbar. Wenn Sie jedoch lieber die ganze Liste mit allen Einzelheiten anzeigen möchten, können Sie mithilfe der OMS-Protokollsuche alle Empfehlungen anzeigen.

*Gibt es eine Möglichkeit, eine Empfehlung zu ignorieren?*
- Ja, siehe oben stehenden Abschnitt [Ignorieren von Empfehlungen](#ignore-recommendations).



## Nächste Schritte

- Verwenden Sie die [Protokollsuche](log-analytics-log-searches.md), um detaillierte SQL Assessment-Daten und -Empfehlungen anzuzeigen.

<!---HONumber=AcomDC_0504_2016-->