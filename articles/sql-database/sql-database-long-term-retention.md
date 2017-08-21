---
title: Speichern von Sicherungen von Azure SQL-Datenbank bis zu 10 Jahre lang| Microsoft-Dokumentation
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
ms.translationtype: HT
ms.sourcegitcommit: 818f7756189ed4ceefdac9114a0b89ef9ee8fb7a
ms.openlocfilehash: 25e651203f804fbf32d632b5f83145a3f3f72a7f
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a>Speichern von Sicherungen von Azure SQL-Datenbank bis zu 10 Jahre lang
Viele Anwendungen dienen gesetzlichen, ordnungsgemäßen oder anderen geschäftlichen Zwecken, die voraussetzen, dass Datenbanksicherungen länger als der Zeitraum von 7–35 Tagen, der für [automatischen Sicherungen](sql-database-automated-backups.md) von Azure SQL-Datenbank zur Verfügung gestellt wird, aufbewahrt werden. Mit dem Feature zur langfristigen Aufbewahrung von Sicherungen können Sie Ihre Sicherungen von SQL-Datenbank bis zu 10 Jahre lang in einem Azure Recovery Services-Tresor speichern. Sie können bis zu 1.000 Datenbanken pro Tresor speichern. Sie können dann jede Sicherung im Tresor auswählen, um sie als eine neue Datenbank wiederherzustellen.

> [!IMPORTANT]
> Langfristige Aufbewahrung der Sicherung ist zurzeit in den folgenden Regionen als Vorschauversion verfügbar: „Australien, Osten“, „Australien, Südosten“, „Brasilien, Süden“, „USA, Mitte“, „Asien, Osten“, „USA, Osten“, „USA, Osten 2“, „Indien, Mitte“, „Indien, Süden“, „Japan, Osten“, „Japan, Westen“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Europa, Westen“ und „USA, Westen“.
>

> [!NOTE]
> Sie können bis zu 200 Datenbanken pro Tresor innerhalb eines Zeitraums von 24 Stunden aktivieren. Wir empfehlen, dass Sie einen separaten Tresor für jeden Server verwenden, um die Auswirkung dieser Einschränkung zu minimieren. 
> 

## <a name="how-sql-database-long-term-backup-retention-works"></a>Wie funktioniert die langfristige Aufbewahrung von Sicherungen von SQL-Datenbank?

Mithilfe der langfristigen Aufbewahrung von Sicherungen können Sie einen SQL-Datenbank-Server einem Azure Recovery Services-Tresor zuordnen. 

* Sie müssen den Tresor im gleichen Azure-Abonnement erstellen, in dem der SQL-Server erstellt wurde, und er muss sich in der gleichen geografischen Region und Ressourcengruppe befinden. 
* Dann konfigurieren Sie eine Beibehaltungsrichtlinie für jede beliebige Datenbank. Die Richtlinie sorgt dafür, dass die wöchentlichen vollständigen Datenbanksicherungen in den Recovery Services-Tresor kopiert werden und für die angegebene Beibehaltungsdauer (bis zu 10 Jahre) aufbewahrt werden. 
* Sie können dann aus jeder dieser Datenbanksicherungen eine neue Datenbank auf jedem beliebigen Server innerhalb des Abonnements wiederherstellen. Azure Storage erstellt aus vorhandenen Sicherungen eine Kopie, die keine Auswirkungen auf die Leistung der vorhandenen Datenbank hat.

> [!TIP]
> Einen Leitfaden finden Sie unter [Konfigurieren und Wiederherstellen einer langfristig aufbewahrten Sicherung von Azure SQL-Datenbank](sql-database-long-term-backup-retention-configure.md).

## <a name="enable-long-term-backup-retention"></a>Langfristiges Aufbewahren von Sicherungen

So konfigurieren Sie die langfristige Beibehaltung der Sicherung für eine Datenbank:

1. Erstellen Sie einen Azure Recovery Services-Tresor in der gleichen Region, im gleichen Abonnement und in der gleichen Ressourcengruppe wie Ihren SQL-Datenbank-Server. 
2. Registrieren Sie den Server im Tresor.
3. Erstellen Sie eine Azure Recovery Services-Schutzrichtlinie.
4. Wenden Sie die Schutzrichtlinie auf die Datenbanken an, die eine langfristige Aufbewahrung von Sicherungen erfordern.

Um die langfristige Aufbewahrung von automatisierten Datenbanksicherungen in einem Azure Recovery Services-Tresor zu konfigurieren, zu verwalten und wiederherzustellen, wenden Sie eine der folgenden Vorgehensweisen an:

* Im Azure-Portal: Klicken Sie auf **Langfristige Sicherungsaufbewahrung**, wählen Sie eine Datenbank aus, und klicken Sie dann auf **Konfigurieren**. 

   ![Auswählen einer Datenbank für die langfristige Aufbewahrung von Sicherungen](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

* Mit PowerShell: Weitere Informationen finden Sie unter [Konfigurieren und Wiederherstellen einer langfristig aufbewahrten Sicherung von Azure SQL-Datenbank](sql-database-long-term-backup-retention-configure.md).

## <a name="restore-a-database-thats-stored-with-the-long-term-backup-retention-feature"></a>Wiederherstellen einer Datenbank, die mit dem Feature zur langfristigen Aufbewahrung von Sicherungen gespeichert wurde

So stellen Sie Datenbanken aus einer langfristigen Aufbewahrung von Sicherungen wieder her

1. Listen Sie den Tresor auf, in dem die Sicherung gespeichert ist.
2. Listen Sie den Container auf, der Ihrem logischen Server zugeordnet ist.
3. Listen Sie die Datenquelle innerhalb des Tresors auf, die Ihrer Datenbank zugeordnet ist.
4. Listen Sie die für die Wiederherstellung verfügbaren Wiederherstellungspunkte auf.
5. Stellen Sie die Datenbank aus dem Wiederherstellungspunkt auf dem Zielserver innerhalb Ihres Abonnements wieder her.

Um die langfristige Aufbewahrung von automatisierten Datenbanksicherungen in einem Azure Recovery Services-Tresor zu konfigurieren, zu verwalten und wiederherzustellen, wenden Sie eine der folgenden Vorgehensweisen an:

* Mit dem Azure-Portal: Weitere Informationen finden Sie unter [Verwalten der langfristigen Aufbewahrung von Sicherungen mit dem Azure-Portal](sql-database-long-term-backup-retention-configure.md). 

* Mit PowerShell: Weitere Informationen finden Sie unter [Verwalten der langfristigen Aufbewahrung von Sicherungen mit PowerShell](sql-database-long-term-backup-retention-configure.md).

## <a name="get-pricing-for-long-term-backup-retention"></a>Preisinformationen zur langfristigen Aufbewahrung von Sicherungen

Die langfristige Aufbewahrung von Sicherungen einer SQL-Datenbank-Instanz wird gemäß den [Tarifen des Azure Backup-Diensts](https://azure.microsoft.com/pricing/details/backup/) in Rechnung gestellt.

Nachdem der SQL-Datenbank-Server im Tresor registriert wurde, wird Ihnen der gesamte Speicher, der von den wöchentlichen Sicherungen im Tresor benötigt wird, berechnet.

## <a name="view-available-backups-that-are-stored-in-long-term-backup-retention"></a>Anzeigen der verfügbaren Sicherungen in der langfristigen Sicherungsaufbewahrung

Um die langfristige Aufbewahrung von automatisierten Datenbanksicherungen in einem Azure Recovery Services-Tresor im Azure-Portal zu konfigurieren, zu verwalten und wiederherzustellen, wenden Sie eine der folgenden Vorgehensweisen an:

* Mit dem Azure-Portal: Weitere Informationen finden Sie unter [Verwalten der langfristigen Aufbewahrung von Sicherungen mit dem Azure-Portal](sql-database-long-term-backup-retention-configure.md). 

* Mit PowerShell: Weitere Informationen finden Sie unter [Verwalten der langfristigen Aufbewahrung von Sicherungen mit PowerShell](sql-database-long-term-backup-retention-configure.md).

## <a name="disable-long-term-retention"></a>Langfristige Aufbewahrung

Recovery Service kümmert sich anhand der bereitgestellten Aufbewahrungsrichtlinie automatisch um die Bereinigung der Sicherungen. 

Damit die Sicherungen einer bestimmten Datenbank nicht länger zum Tresor gesendet werden, entfernen Sie die Beibehaltungsrichtlinie für diese Datenbank.
  
```
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName 'RG1' -ServerName 'Server1' -DatabaseName 'DB1' -State 'Disabled' -ResourceId $policy.Id
```

> [!NOTE]
> Die Sicherungen, die sich bereits im Tresor befinden, sind davon nicht betroffen. Sie werden automatisch vom Recovery Service gelöscht, wenn ihre Beibehaltungsdauer abläuft.

## <a name="long-term-backup-retention-faq"></a>Häufig gestellte Fragen zur langfristigen Aufbewahrung von Sicherungen

**Kann ich bestimmte Sicherungen im Tresor manuell löschen?**

Derzeit ist dies nicht möglich. Der Tresor bereinigt Sicherungen nach Ablauf des Aufbewahrungszeitraums automatisch.

**Kann ich meinen Server registrieren, um in mehr als einem Tresor Sicherungen zu speichern?**

Nein, Sie können momentan nur Sicherungen in einem Tresor gleichzeitig speichern.

**Kann ich über einen Tresor und Server in verschiedenen Abonnements verfügen?**

Nein, derzeit müssen der Tresor und der Server im gleichen Abonnement und der gleichen Ressourcengruppe sein.

**Kann ich einen von mir erstellten Tresor in einer anderen Region als der meines Servers verwenden?**

Nein, der Tresor und der Server müssen in der gleichen Region sein, um die Zeit für den Kopiervorgang und die Gebühren für den Datenverkehr zu minimieren.

**Wie viele Datenbanken kann ich in einem Tresor speichern?**

Derzeit unterstützen wir bis zu 1.000 Datenbanken pro Tresor. 

**Wie viele Tresore kann ich pro Abonnement erstellen?**

Sie können bis zu 25 Tresore pro Abonnement erstellen.

**Wie viele Datenbanken kann ich pro Tresor und pro Tag konfigurieren?**

Sie können 200 Datenbanken pro Tresor und pro Tag einrichten.

**Funktioniert die langfristige Aufbewahrung von Sicherungen auch mit Pools für elastische Datenbanken?**

Ja. Jede Datenbank im Pool kann mit der Beibehaltungsrichtlinie konfiguriert werden.

**Kann ich den Zeitpunkt auswählen, an dem die Sicherungen erstellt werden?**

Nein, SQL-Datenbank kontrolliert den Zeitplan der Sicherungen, um die Auswirkungen auf die Leistung Ihrer Datenbanken zu minimieren.

**Ich habe die Transparent Data Encryption (TDE) für die Datenbank aktiviert. Kann ich dieses Feature mit dem Tresor verwenden?** 

Ja, Transparent Data Encryption wird unterstützt. Selbst wenn die ursprüngliche Datenbank nicht mehr vorhanden ist, können Sie die Datenbank aus dem Tresor wiederherstellen.

**Was geschieht mit den Sicherungen im Tresor, wenn mein Abonnement unterbrochen wird?** 

Falls Ihr Abonnement unterbrochen wird, werden wir die vorhandenen Datenbanken und Sicherungen beibehalten. Neue Sicherungen werden jedoch nicht in den Tresor kopiert. Nachdem Sie das Abonnement reaktiviert haben, setzt der Dienst das Kopieren der Sicherungen in den Tresor fort. Ihr Tresor wird wieder für Wiederherstellungsvorgänge verfügbar, indem die Sicherungen genutzt werden, die vor dem Unterbrechen des Abonnements kopiert wurden. 

**Kann ich Zugriff auf die SQL-Datenbank-Sicherungsdateien erhalten, damit ich sie herunterladen oder auf dem SQL-Server wiederherstellen kann?**

Derzeit ist dies nicht möglich.

**Ist es möglich, mehrere Zeitpläne (täglich, wöchentlich, monatlich, jährlich) in einer SQL-Beibehaltungsrichtlinie zu verwenden?**

Nein, mehrere Zeitpläne sind derzeit nur für Sicherungen virtueller Computer verfügbar.

**Was geschieht, wenn wir die langfristige Aufbewahrung von Sicherungen für eine Datenbank einrichten, die eine sekundäre Datenbank einer aktiven Georeplikation ist?**

Da wir derzeit keine Sicherungen von Replikaten erstellen, steht keine Option für die langfristige Aufbewahrung von Sicherungen sekundärer Datenbanken zur Verfügung. Es ist jedoch aus folgenden Gründen für einen Benutzer wichtig, eine langfristige Aufbewahrung von Sicherungen für eine sekundäre Datenbank in einer aktiven Georeplikation zu erstellen:
* Wenn ein Failover erfolgt und die Datenbank die primäre Datenbank wird, führen wir eine vollständige Sicherung durch, die dann in den Tresor hochgeladen wird.
* Für den Kunden fallen keine zusätzlichen Kosten für die Einrichtung der langfristigen Aufbewahrung von Sicherungen einer sekundären Datenbank an.

## <a name="next-steps"></a>Nächste Schritte
Datenbanksicherungen sind ein wesentlicher Bestandteil jeder Strategie für Geschäftskontinuität und Notfallwiederherstellung, da Ihre Daten vor versehentlichen Beschädigungen und Löschungen geschützt werden. Weitere Informationen zu den anderen Geschäftskontinuitätslösungen von SQL-Datenbank finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).

