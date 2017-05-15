---
title: Speichern von Sicherungen von Azure SQL-Datenbank bis zu 10 Jahre lang| Microsoft Docs
description: "Erfahren Sie, wie Azure SQL-Datenbank das Speichern von Sicherungen bis zu 10 Jahre lang unterstützt."
keywords: 
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 66fdb8b8-5903-4d3a-802e-af08d204566e
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/22/2016
ms.author: sashan
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: e47c10c2c979622fba254757d3f7b9f19d3240f8
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="storing-azure-sql-database-backups-for-up-to-10-years"></a>Speichern von Sicherungen von Azure SQL-Datenbank bis zu 10 Jahre lang
Viele Anwendungen dienen gesetzlichen, ordnungsgemäßen oder anderen geschäftlichen Zwecken, die voraussetzen, dass die vollständige automatische Datenbanksicherung länger als der Zeitraum zwischen 7-35 Tagen beibehalten wird, der von den [automatischen Sicherungen](sql-database-automated-backups.md) der SQL-Datenbank zur Verfügung gestellt wurde. Das Feature der **langfristigen Beibehaltung der Sicherung** lässt Sie Ihre Sicherungen von Azure SQL-Datenbank in einem Azure Recovery Services-Tresor bis zu 10 Jahre lang speichern. Sie können bis zu 1000 Datenbanken pro Tresor speichern. Sie können jede Sicherung im Tresor auswählen, um sie als eine neue Datenbank wiederherzustellen.

> [!IMPORTANT]
> Langfristige Beibehaltung der Sicherung ist zurzeit in den folgenden Regionen als Vorschau verfügbar: Australien, Osten; Australien, Südosten; Brasilien, Süden; USA, Mitte; Asien, Osten; USA, Osten; USA, Osten 2; Indien, Mitte; Indien, Süden; Japan, Osten; Japan, Westen; USA, Norden-Mitte; Europa, Norden; USA, Süden-Mitte; Asien, Südosten; Europa, Westen und USA, Westen.
>

> [!NOTE]
> Sie können bis zu 200 Datenbanken pro Tresor innerhalb eines Zeitraums von 24 Stunden aktivieren. Darum empfehlen wir, dass Sie einen separaten Tresor für jeden Server verwenden, um die Auswirkung dieser Einschränkung zu minimieren. 
> 



 
## <a name="how-does-sql-database-long-term-backup-retention-work"></a>Wie funktioniert die langfristige Sicherungsaufbewahrung von SQL-Datenbank?

Mithilfe der langfristigen Aufbewahrung von Sicherungen können Sie einen Azure SQL-Datenbank-Server einem Azure Recovery Services-Tresor zuordnen. 

* Der Tresor muss im gleichen Azure-Abonnement erstellt werden, das den SQL-Server erstellt hat und sich in der gleichen geografischen Region und Ressourcengruppe befindet. 
* Dann konfigurieren Sie eine Beibehaltungsrichtlinie für jede beliebige Datenbank. Die Richtlinie sorgt dafür, dass die wöchentlichen vollständigen Datenbanksicherungen in den Recovery Services-Tresor kopiert werden und für die angegebene Beibehaltungsdauer (bis zu 10 Jahre) aufbewahrt werden. 
* Sie können dann aus jeder dieser Sicherungen eine neue Datenbank in jedem beliebigen Server innerhalb des Abonnements wiederherstellen. Die Kopie wird vom Azure-Speicher aus vorhandenen Sicherungen erstellt und schränkt die Leistung auf die vorhandene Datenbank nicht ein.

> [!TIP]
> Einen Leitfaden finden Sie unter [Konfiguration einer langfristig aufbewahrten Sicherung einer Azure SQL-Datenbank und die Wiederherstellung daraus](sql-database-long-term-backup-retention-configure.md).

## <a name="how-do-i-enable-long-term-backup-retention"></a>Wie aktiviere ich die langfristige Sicherungsaufbewahrung?

So konfigurieren Sie die langfristige Beibehaltung der Sicherung für eine Datenbank:

1. Erstellen Sie einen Azure Recovery Services-Tresor in der gleichen Region, im gleichen Abonnement und in der gleichen Ressourcengruppe wie Ihren SQL-Datenbankserver. 
2. Registrieren Sie den Server im Tresor.
3. Erstellen Sie eine Azure Recovery Services-Schutzrichtlinie.
4. Wenden Sie die Schutzrichtlinie auf die Datenbank an, die die langfristige Beibehaltung der Sicherung benötigt.

### <a name="azure-portal"></a>Azure-Portal

Um über das Azure-Portal die langfristige Sicherungsaufbewahrung von automatisierten Sicherungen in einem Azure Recovery Services-Tresor zu konfigurieren oder zu verwalten oder eine Sicherung daraus wiederherzustellen, klicken Sie auf **Langfristige Sicherungsaufbewahrung**, wählen Sie eine Datenbank aus, und klicken Sie dann auf **Konfigurieren**. 

   ![Datenbank für langfristige Beibehaltung der Sicherung auswählen](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

Informationen, wie Sie mit PowerShell die langfristige Sicherungsaufbewahrung von automatisierten Sicherungen in einem Azure Recovery Services-Tresor konfigurieren oder verwalten oder eine Sicherung daraus wiederherstellen, finden Sie unter [Configure and restore from Azure SQL Database long-term backup retention](sql-database-long-term-backup-retention-configure.md).

## <a name="how-do-i-restore-a-database-stored-with-the-long-term-backup-retention-feature"></a>Wie stelle ich eine Datenbank wieder her, die mit dem Feature für die langfristige Sicherungsaufbewahrung gespeichert wurde?

So stellen Sie Datenbanken aus einer langfristigen Aufbewahrung von Sicherungen wieder her

1. Listen Sie auf, wo die Sicherung gespeichert ist.
2. Listen Sie den Container auf, der Ihrem logischen Server zugeordnet ist.
3. Listen Sie die Datenquelle innerhalb des Tresors auf, die Ihrer Datenbank zugeordnet ist.
4. Listen Sie die verfügbaren Wiederherstellungspunkte auf.
5. Stellen Sie die Datenbank aus dem Wiederherstellungspunkt auf den Zielserver innerhalb Ihres Abonnements wieder her.

Informationen zum Konfigurieren, Verwalten und Wiederherstellen der langfristigen Aufbewahrung automatisierter Sicherungen in einem Azure Recovery Services-Tresor im Azure-Portal finden Sie unter [Verwalten der langfristigen Aufbewahrung von Datenbanksicherungen über das Azure-Portal](sql-database-long-term-backup-retention-configure.md). Informationen zum Konfigurieren, Verwalten und Wiederherstellen der langfristigen Aufbewahrung automatisierter Sicherungen in einem Azure Recovery Services-Tresor mit PowerShell finden Sie unter [Verwalten der langfristigen Aufbewahrung von Datenbanksicherungen mit PowerShell](sql-database-long-term-backup-retention-configure.md).

## <a name="how-much-does-long-term-backup-retention-cost"></a>Wie viel kostet die langfristige Sicherungsaufbewahrung?

Die langfristige Sicherungsaufbewahrung einer Azure SQL-Datenbank-Instanz wird gemäß den [Tarifen des Azure Backup-Diensts](https://azure.microsoft.com/pricing/details/backup/) in Rechnung gestellt.

Nachdem der Azure SQL-Datenbankserver im Tresor registriert ist, wird Ihnen der gesamten Speicher, der von den wöchentlichen Sicherungen im Tresor benötigt wird, berechnet.

## <a name="view-available-backups-stored-in-long-term-backup-retention"></a>Anzeigen der verfügbaren Sicherungen in der langfristigen Sicherungsaufbewahrung

Informationen zum Konfigurieren, Verwalten und Wiederherstellen der langfristigen Aufbewahrung automatisierter Sicherungen in einem Azure Recovery Services-Tresor im Azure-Portal finden Sie unter [Verwalten der langfristigen Aufbewahrung von Datenbanksicherungen über das Azure-Portal](sql-database-long-term-backup-retention-configure.md). Informationen zum Konfigurieren, Verwalten und Wiederherstellen der langfristigen Aufbewahrung automatisierter Sicherungen in einem Azure Recovery Services-Tresor mit PowerShell finden Sie unter [Verwalten der langfristigen Aufbewahrung von Datenbanksicherungen mit PowerShell](sql-database-long-term-backup-retention-configure.md).

## <a name="disabling-long-term-retention"></a>Deaktivieren der langfristigen Beibehaltung

Der Recovery Service kümmert sich anhand der bereitgestellten Beibehaltungsrichtlinie automatisch um die Bereinigung der Sicherungen. 

* Damit die Sicherungen einer bestimmten Datenbank nicht länger zum Tresor gesendet werden, entfernen Sie die Beibehaltungsrichtlinie für diese Datenbank.
  
    ```
    Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName 'RG1' -ServerName 'Server1' -DatabaseName 'DB1' -State 'Disabled' -ResourceId $policy.Id
    ```

> [!NOTE]
> Die Sicherungen, die sich bereits im Tresor befinden, werden davon nicht betroffen. Sie werden automatisch vom Recovery Service gelöscht, wenn ihre Beibehaltungsdauer abläuft.

## <a name="long-term-backup-retention-faq"></a>Häufig gestellte Fragen zur langfristigen Aufbewahrung von Sicherungen:

1. Frage: Kann ich bestimmte Sicherungen im Tresor manuell löschen?

    Antwort: Derzeit nicht. Der Tresor bereinigt Sicherungen nach Ablauf des Aufbewahrungszeitraums automatisch.
2. Frage: Kann ich meinen Server registrieren, um in mehr als einem Tresor Sicherungen zu speichern?

    Antwort: Nein, Sie können momentan nur Sicherungen in einem Tresor gleichzeitig speichern.
3. Frage: Kann ich über einen Tresor und Server in verschiedenen Abonnements verfügen?

    Antwort: Nein, derzeit müssen sowohl der Tresor als auch der Server im gleichen Abonnement und der gleichen Ressourcengruppe sein.
4. Frage: Kann ich einen von mir erstellten Tresor in einer anderen Region als die meines Servers verwenden?

    Antwort: Nein, der Tresor und Server müssen in der gleichen Region sein, um die Zeit für den Kopiervorgang und Gebühren für den Datenverkehr zu minimieren.
5. Frage: Wie viele Datenbanken kann ich in einem Tresor speichern?

    A: Derzeit unterstützen wir nur bis zu 1.000 Datenbanken pro Tresor. 
6. F: Wie viele Tresore kann ich pro Abonnement erstellen?

    A: Sie können bis zu 25 Tresore pro Abonnement erstellen.
7. F: Wie viele Datenbanken kann ich pro Tresor und pro Tag konfigurieren?

    A: Sie können nur 200 Datenbanken pro Tresor und pro Tag einrichten.
8. F: Funktioniert die langfristige Sicherungsaufbewahrung mit Pools für elastische Datenbanken?

    A: Ja. Jede Datenbank im Pool kann mit der Beibehaltungsrichtlinie konfiguriert werden.
9. Frage: Kann ich den Zeitpunkt auswählen, an dem die Sicherungen erstellt werden?

    Antwort: SQL-Datenbanken kontrollieren den Zeitplan der Sicherungen, um die Auswirkungen auf die Leistung an Ihren Datenbanken zu minimieren.
10. Frage: Ich habe TDE für meine Datenbank aktiviert. Kann ich TDE mit dem Tresor verwenden? 

    A: Ja, TDE wird unterstützt. Selbst wenn die ursprüngliche Datenbank nicht mehr vorhanden ist, können Sie die Datenbank aus dem Tresor wiederherstellen.
11. F: Was geschieht mit den Sicherungen im Tresor, wenn mein Abonnement unterbrochen wird? 

    A: Falls Ihr Abonnement unterbrochen wird, werden wir die vorhandenen Datenbanken und Sicherungen beibehalten. Neue Sicherungen werden jedoch nicht in den Tresor kopiert. Nachdem Sie das Abonnement reaktiviert haben, setzt der Dienst das Kopieren der Sicherungen in den Tresor fort. Ihr Tresor wird wieder für Wiederherstellungsvorgänge verfügbar, indem die Sicherungen genutzt werden, die vor dem Unterbrechen des Abonnements kopiert wurden. 
12. Frage: Kann ich Zugriff auf die SQL-Datenbank-Sicherungsdateien erhalten, damit ich sie herunterladen/auf dem SQL Server wiederherstellen kann?

    Antwort: Derzeit ist dies nicht möglich.
13. Frage: Ist es möglich, mehrere Zeitpläne (täglich, wöchentlich, monatlich, jährlich) in einer SQL-Beibehaltungsrichtlinie zu haben?

    Antwort: Nein, dies ist derzeit nur für Sicherungen von virtuellen Computern verfügbar.
14. F: Was geschieht, wenn wir langfristige Sicherungsaufbewahrung für eine Datenbank einrichten, die eine sekundäre Datenbank einer aktiven Georeplikation ist?

    A: Derzeit erstellen wir keine Sicherungen von Replikaten. Es steht deshalb keine Option für die langfristige Sicherungsaufbewahrung sekundärer Datenbanken zur Verfügung. Es ist jedoch aus folgenden Gründen für einen Kunden wichtig, eine langfristige Sicherungsaufbewahrung für eine sekundäre Datenbank in einer aktiven Georeplikation zu erstellen:
    - Wenn ein Failover erfolgt und die Datenbank die primäre Datenbank wird, führen wir eine vollständige Sicherung durch, und diese wird dann in den Tresor hochgeladen.
    - Für den Kunden fallen keine zusätzlichen Kosten für die Einrichtung der langfristigen Sicherungsaufbewahrung einer sekundären Datenbank an.



## <a name="next-steps"></a>Nächste Schritte
Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Weitere Informationen zu den anderen Geschäftskontinuitätslösungen von Azure SQL-Datenbank finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md).


