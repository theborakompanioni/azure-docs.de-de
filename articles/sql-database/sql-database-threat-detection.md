---
title: "Bedrohungserkennung – Azure SQL-Datenbank | Microsoft Docs"
description: "Die Bedrohungserkennung erkennt anormale Datenbankaktivitäten, die auf potenzielle Sicherheitsrisiken für die Datenbank hindeuten."
services: sql-database
documentationcenter: 
author: rmatchoro
manager: jhubbard
editor: v-romcal
ms.assetid: b50d232a-4225-46ed-91e7-75288f55ee84
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/19/2017
ms.author: ronmat; ronitr
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 5c2742a1d8ed6df7496a14226a38e02ca993abf3
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---
# <a name="sql-database-threat-detection"></a>Bedrohungserkennung von SQL-Datenbank

Die SQL-Bedrohungserkennung erkennt anomale Aktivitäten, die auf ungewöhnliche und möglicherweise schädliche Versuche hinweisen, auf Datenbanken zuzugreifen oder diese zu missbrauchen.

## <a name="overview"></a>Übersicht

Die SQL-Bedrohungserkennung bietet eine neue Sicherheitsebene, die es den Kunden ermöglicht, auf erkannte potenzielle Bedrohungen zu reagieren. Zu diesem Zweck werden Sicherheitshinweise zu anomalen Aktivitäten bereitgestellt.  Benutzer erhalten Warnungen zu verdächtigen Datenbankaktivitäten, potenziellen Sicherheitsrisiken sowie Angriffen mit Einschleusung von SQL-Befehlen und anomalen Datenbank-Zugriffsmustern. Warnungen der SQL-Bedrohungserkennung bieten Detailinformationen zu verdächtigen Aktivitäten sowie Empfehlungen, wie die Bedrohung untersucht und abgewendet werden kann. Die Benutzer können die verdächtigen Ereignisse mithilfe der [Überwachung von SQL-Datenbank](sql-database-auditing.md) untersuchen, um zu ermitteln, ob sie auf einen Zugriffsversuch zurückzuführen sind oder die Verletzung der Datensicherheit oder den Missbrauch von Daten zum Ziel haben. Die Bedrohungserkennung vereinfacht den Umgang mit potenziellen Bedrohungen für die Datenbank, ohne das Fachwissen eines Sicherheitsexperten besitzen oder komplexe Sicherheitsüberwachungssysteme verwalten zu müssen.

Beispielsweise ist die Einschleusung von SQL-Befehlen ein verbreitetes Sicherheitsproblem für Webanwendungen im Internet und wird für Angriffe auf datengesteuerte Anwendungen verwendet. Die Angreifer nutzen Sicherheitslücken der Anwendung, um böswillige SQL-Anweisungen in Eingabefelder der Anwendung einzuschleusen, sodass Daten in der Datenbank manipuliert oder verändert werden können.

Durch die SQL-Bedrohungserkennung werden Warnungen in [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/) integriert. Jeder geschützte SQL-Datenbankserver wird zu demselben Preis wie der Standardtarif von Azure Security Center abgerechnet (15 $/Knoten/Monat). Hierbei zählt jeder geschützte SQL-Datenbankserver als ein Knoten. Sie sind herzlich eingeladen, dieses Feature 60 Tage lang kostenlos zu testen. 

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Einrichten der Bedrohungserkennung für Ihre Datenbank im Azure-Portal
1. Rufen Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com) auf.
2. Navigieren Sie zum Konfigurationsblatt der SQL-Datenbank, die Sie überwachen möchten. Wählen Sie auf dem Blatt „Einstellungen“ die Option **Überwachung und Bedrohungserkennung**. 
    ![Navigationsbereich][1]
3. Schalten Sie auf dem Konfigurationsblatt **Überwachung und Bedrohungserkennung** die Option „Überwachung“ auf **EIN**. Daraufhin werden die Einstellungen für die Bedrohungserkennung angezeigt.
  
    ![Navigationsbereich][2]
4. **Aktivieren Sie** die Bedrohungserkennung.
5. Konfigurieren Sie die Liste der E-Mail-Empfänger, die bei Erkennung anormaler Datenbankaktivitäten einen Sicherheitshinweis erhalten sollen.
6. Klicken Sie auf dem Blatt **Überwachung und Bedrohungserkennung** auf **Speichern**, um die neuen oder aktualisierten Einstellungen für Überwachung und Bedrohungserkennung zu speichern.
       
    ![Navigationsbereich][3]

## <a name="set-up-threat-detection-using-powershell"></a>Einrichten der Bedrohungserkennung über PowerShell

Ein Skriptbeispiel finden Sie unter [Konfigurieren von Überwachung von SQL-Datenbank und Bedrohungserkennung mit PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Untersuchen anormaler Datenbankaktivitäten bei Erkennung eines verdächtigen Ereignisses
1. Bei Erkennung anormaler Datenbankaktivitäten erhalten Sie eine E-Mail-Benachrichtigung. <br/>
   Die E-Mail enthält Informationen zum verdächtigen Sicherheitsereignis (dazu gehören Art der anomalen Aktivitäten, Datenbankname, Servername, Anwendungsname und Zeit des Ereignisses). Darüber hinaus enthält die E-Mail Angaben zu möglichen Ursachen und empfohlenen Maßnahmen zur Untersuchung und Abwehr der potenziellen Bedrohung für die Datenbank.<br/>
     
    ![Navigationsbereich][4]
2. Die E-Mail-Warnung enthält einen direkten Link zum SQL Überwachungsprotokoll. Durch Klicken auf diesen Link wird das Azure-Portal gestartet und werden die SQL-Überwachungsdatensätze geöffnet, die in zeitlicher Nähe zum verdächtigen Ereignis liegen. Klicken Sie auf einen Überwachungsdatensatz, um weitere Details zu den verdächtigen Datenbankaktivitäten anzuzeigen. Damit haben Sie es einfacher, die SQL-Anweisungen zu finden, die ausgeführt wurden (wer hat den Zugriff ausgelöst, was wurde wann ausgeführt), und zu bestimmen, ob das Ereignis zulässig oder böswillig war (z. B. Anwendungsanfälligkeit für die Einschleusung von SQL-Befehlen wurde ausgenutzt, jemand hat vertrauliche Daten verletzt usw.).<br/>
   ![Navigationsbereich][5]


## <a name="explore-threat-detection-alerts-for-your-database-in-the-azure-portal"></a>Auswerten von Bedrohungserkennungswarnungen für Ihre Datenbank im Azure-Portal

Die Warnungen der Bedrohungserkennung von SQL-Datenbank sind in [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/) eingebunden. Auf einer Livekachel für SQL-Sicherheit auf dem Datenbankblatt im Azure-Portal werden die Status von aktiven Bedrohungen nachverfolgt. 

   ![Navigationsbereich][6]
   
1. Durch Klicken auf die Kachel für SQL-Sicherheit wird das Azure Security Center-Blatt für Warnungen geöffnet, das einen Überblick über die aktiven SQL-Bedrohungen bietet, die in der Datenbank erkannt wurden. 

  ![Navigationsbereich][7]

2. Durch Klicken auf eine bestimmte Warnung werden weitere Details und Aktionen zum Untersuchen dieser Bedrohung und Abwehren zukünftiger Bedrohungen bereitgestellt.

  ![Navigationsbereich][8]


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur Bedrohungserkennung: siehe [Azure-Blog](https://azure.microsoft.com/en-us/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/) 
* Weitere Informationen zu [Überwachung von Azure SQL-Datenbank](sql-database-auditing.md)
* Weitere Informationen zu [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro)
* Ausführlichere Informationen zu den Preisen: siehe [Seite mit Preisen von SQL-Datenbank](https://azure.microsoft.com/en-us/pricing/details/sql-database/)  
* Ein PowerShell-Skriptbeispiel: siehe [Konfigurieren von Überwachung von SQL-Datenbank und Bedrohungserkennung mit PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md)



<!--Image references-->
[1]: ./media/sql-database-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-database-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-database-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-database-threat-detection/4_td_email.png
[5]: ./media/sql-database-threat-detection/5_td_audit_record_details.png
[6]: ./media/sql-database-threat-detection/6_td_security_tile_view_alerts.png
[7]: ./media/sql-database-threat-detection/7_td_SQL_security_alerts_list.png
[8]: ./media/sql-database-threat-detection/8_td_SQL_security_alert_details.png



