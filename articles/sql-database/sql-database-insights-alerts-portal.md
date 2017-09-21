---
title: Verwenden des Azure-Portals zum Erstellen von SQL-Datenbankwarnungen | Microsoft Docs
description: "Verwenden Sie das Azure-Portal, um SQL-Datenbankwarnungen zu erstellen, die Benachrichtigungen oder eine Automatisierung auslösen, wenn die angegebenen Bedingungen erfüllt sind."
author: aamalvea
manager: jhubbard
editor: 
services: sql-database
documentationcenter: 
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: sql-database
ms.custom: monitor and tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: aamalvea
ms.translationtype: HT
ms.sourcegitcommit: 12c20264b14a477643a4bbc1469a8d1c0941c6e6
ms.openlocfilehash: 2b3f9f226039759a68e28b9392d30256f308c8d3
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---
# <a name="use-azure-portal-to-create-alerts-for-azure-sql-database-and-data-warehouse"></a>Verwenden des Azure-Portals zum Erstellen von Warnungen für Azure SQL-Datenbanken und Data Warehouse

## <a name="overview"></a>Übersicht
In diesem Artikel erfahren Sie, wie Sie mit dem Azure-Portal Azure SQL-Datenbank- und Data Warehouse-Warnungen einrichten können. Dieser Artikel nennt auch bewährte Methoden für das Festlegen von Warnungszeiträumen.    

Sie können auf der Grundlage von Überwachungsmetriken für Ihre Azure-Services oder von Ereignissen, die bei diesen auftreten, eine Warnung empfangen.

* **Metrikwerte** : Die Warnung wird ausgelöst, wenn der Wert einer angegebenen Metrik einen von Ihnen festgelegten Schwellenwert in beliebiger Richtung überschreitet. Das Auslösen erfolgt sowohl, wenn die Bedingung erstmals erfüllt wird, als auch danach, wenn diese Bedingung nicht mehr erfüllt wird.    
* **Aktivitätsprotokollereignisse** : Eine Warnung kann für *jedes* Ereignis oder nur dann ausgelöst werden, wenn eine bestimmte Anzahl von Ereignissen erfolgt ist.

Sie können konfigurieren, dass bei einer Warnung Folgendes erfolgt, wenn sie ausgelöst wird:

* Senden von E-Mail-Benachrichtigungen an den Dienstadministrator und Co-Administratoren
* Senden von E-Mal an weitere von Ihnen angegebene Adressen
* Aufrufen eines Webhooks

Sie haben folgende Möglichkeiten zum Konfigurieren von Warnregeln und Abrufen zugehöriger Informationen:

* [Azure-Portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../monitoring-and-diagnostics/insights-alerts-powershell.md)
* [Befehlszeilenschnittstelle (CLI)](../monitoring-and-diagnostics/insights-alerts-command-line-interface.md)
* [Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Erstellen einer Warnungsregel anhand einer Metrik mit dem Azure-Portal
1. Suchen Sie im [Portal](https://portal.azure.com/)die Ressource, die Sie überwachen möchten, und wählen Sie sie aus.
2. Dieser Schritt ist für SQL-Datenbank und elastische Pools im Vergleich zu SQL DW unterschiedlich: 

   - **Nur SQL-Datenbank und elastische Pools**: Wählen Sie im Abschnitt ÜBERWACHUNG **Warnungen** oder **Warnungsregeln** aus. Text und Symbol können je nach Ressource geringfügig variieren.  
   
     ![Überwachung](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButton.png)
  
   - **NUR SQL DW**: Wählen Sie unter dem Abschnitt ALLGEMEINE AUFGABEN **Überwachung** aus. Klicken Sie auf den Graphen **DWU-Nutzung**.

     ![ALLGEMEINE AUFGABEN](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButtonDW.png)

3. Wählen Sie den Befehl **Warnung hinzufügen** aus, und füllen Sie die Felder aus.
   
    ![Warnung hinzufügen](../monitoring-and-diagnostics/media/insights-alerts-portal/AddDBAlertPage.png)
4. **Benennen** Sie Ihre Warnungsregel, und wählen Sie eine **Beschreibung** aus, die auch in Benachrichtigungs-E-Mails angezeigt wird.
5. Wählen Sie die **Metrik** aus, die Sie überwachen möchten, und dann je einen Wert für **Bedingung** und **Schwellenwert** für die Metrik aus. Wählen Sie auch den **Zeitraum** der Metrikregel aus, der erfüllt sein muss, ehe die Warnung ausgelöst wird. Wenn Sie z.B. den Zeitraum „PT5M“ wählen, und die Warnung nach einer CPU-Auslastung von über 80 % sucht, wird die Warnung ausgelöst, wenn die **durchschnittliche** CPU-Auslastung 5 Minuten über 80 % lag. Nachdem der erste Trigger ausgelöst wurde, erfolgt ein erneutes Auslösen, wenn die durchschnittliche CPU-Auslastung 5 Minuten unter 80 % bleibt. Die CPU-Messung erfolgt minütlich. In der folgenden Tabelle finden Sie unterstützte Zeitfenster und den Aggregationstyp für die einzelnen Warnungen – nicht alle Warnungen verwenden den durchschnittlichen Wert.   
6. Aktivieren Sie **E-Mail-Besitzer...** , wenn Sie möchten, dass Administratoren und Co-Administratoren per E-Mail benachrichtigt werden, wenn die Warnung ausgelöst wird.
7. Wenn Sie möchten, dass bei Auslösen der Warnung eine Benachrichtigung an weitere E-Mail-Adressen gesendet wird, fügen Sie diese dem Feld **Zusätzliche Administrator-E-Mail-Adresse** hinzu. Trennen Sie mehrere E-Mail-Adressen durch Semikolons: *email@contoso.com;email2@contoso.com*
8. Fügen Sie in einen gültigen URI in das Feld **Webhook** ein, wenn dieser bei Auslösen der Warnung aufgerufen werden soll.
9. Wählen Sie **OK** aus, wenn das Erstellen der Warnung abgeschlossen ist.   

Innerhalb weniger Minuten wird die Warnung aktiv und wie oben beschrieben ausgelöst.

## <a name="managing-your-alerts"></a>Verwalten von Warnungen
Nachdem Sie eine Warnung erstellt haben, können Sie sie auswählen und:

* ein Diagramm einblenden, das den Schwellenwert der Metrik und die tatsächlichen Werte vom Vortag zeigt.
* bearbeiten oder löschen.
* sie **deaktivieren** oder **aktivieren**, wenn Sie den Empfang von Benachrichtigungen zu dieser Warnung vorübergehend beenden oder fortsetzen möchten.


## <a name="sql-database-alert-values"></a>Werte für SQL-Datenbankwarnungen

| Ressourcentyp | Metrikname | Anzeigename | Aggregationstyp | Mindestzeitfenster für Warnungen|
| --- | --- | --- | --- | --- |
| SQL database | cpu_percent | CPU-Prozentsatz | Durchschnitt | 5 Minuten |
| SQL database | physical_data_read_percent | E/A-Prozentsatz für Daten | Durchschnitt | 5 Minuten |
| SQL database | log_write_percent | E/A-Prozentsatz für Protokoll | Durchschnitt | 5 Minuten |
| SQL database | dtu_consumption_percent | DTU-Prozentsatz | Durchschnitt | 5 Minuten |
| SQL database | storage | Datenbankgröße gesamt | Maximum | 30 Minuten |
| SQL database | connection_successful | Erfolgreiche Verbindungen | Gesamt | 10 Minuten |
| SQL database | connection_failed | Verbindungsfehler | Gesamt | 10 Minuten |
| SQL database | blocked_by_firewall | Von der Firewall blockiert | Gesamt | 10 Minuten |
| SQL database | deadlock | Deadlocks | Gesamt | 10 Minuten |
| SQL database | storage_percent | Datenbankgröße als Prozentsatz | Maximum | 30 Minuten |
| SQL database | xtp_storage_percent | In-Memory-OLTP-Speicher in Prozent (Vorschau) | Durchschnitt | 5 Minuten |
| SQL database | workers_percent | Worker in Prozent | Durchschnitt | 5 Minuten |
| SQL database | sessions_percent | Sitzungen in Prozent | Durchschnitt | 5 Minuten |
| SQL database | dtu_limit | DTU-Grenzwert | Durchschnitt | 5 Minuten |
| SQL database | dtu_used | DTU-Verbrauch | Durchschnitt | 5 Minuten |
||||||
| Elastischer Pool | cpu_percent | CPU-Prozentsatz | Durchschnitt | 10 Minuten |
| Elastischer Pool | physical_data_read_percent | E/A-Prozentsatz für Daten | Durchschnitt | 10 Minuten |
| Elastischer Pool | log_write_percent | E/A-Prozentsatz für Protokoll | Durchschnitt | 10 Minuten |
| Elastischer Pool | dtu_consumption_percent | DTU-Prozentsatz | Durchschnitt | 10 Minuten |
| Elastischer Pool | storage_percent | Speicher in Prozent | Durchschnitt | 10 Minuten |
| Elastischer Pool | workers_percent | Worker in Prozent | Durchschnitt | 10 Minuten |
| Elastischer Pool | eDTU_limit | eDTU-Grenzwert | Durchschnitt | 10 Minuten |
| Elastischer Pool | storage_limit | Speicherbegrenzung | Durchschnitt | 10 Minuten |
| Elastischer Pool | eDTU_used | eDTU-Verbrauch | Durchschnitt | 10 Minuten |
| Elastischer Pool | storage_used | Verwendeter Speicher | Durchschnitt | 10 Minuten |
||||||               
| SQL Data Warehouse | cpu_percent | CPU-Prozentsatz | Durchschnitt | 10 Minuten |
| SQL Data Warehouse | physical_data_read_percent | E/A-Prozentsatz für Daten | Durchschnitt | 10 Minuten |
| SQL Data Warehouse | storage | Datenbankgröße gesamt | Maximum | 10 Minuten |
| SQL Data Warehouse | connection_successful | Erfolgreiche Verbindungen | Gesamt | 10 Minuten |
| SQL Data Warehouse | connection_failed | Verbindungsfehler | Gesamt | 10 Minuten |
| SQL Data Warehouse | blocked_by_firewall | Von der Firewall blockiert | Gesamt | 10 Minuten |
| SQL Data Warehouse | service_level_objective | SLO der Datenbank | Gesamt | 10 Minuten |
| SQL Data Warehouse | dwu_limit | DWU-Grenzwert | Maximum | 10 Minuten |
| SQL Data Warehouse | dwu_consumption_percent | DWU in Prozent | Durchschnitt | 10 Minuten |
| SQL Data Warehouse | dwu_used | DWU-Verbrauch | Durchschnitt | 10 Minuten |
||||||


## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über die Azure-Überwachung](../monitoring-and-diagnostics/monitoring-overview.md) , einschließlich der Typen von Informationen, die Sie sammeln und überwachen können.
* Erfahren Sie mehr über das [Konfigurieren von Webhooks in Warnungen](../monitoring-and-diagnostics/insights-webhooks-alerts.md).
* Verschaffen Sie sich einen [Überblick über Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) , um detaillierte Hochfrequenzmetriken für Ihren Dienst zu erfassen.
* Verschaffen Sie sich einen Überblick über das [Sammeln von Dienstmetriken](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) , um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist.

