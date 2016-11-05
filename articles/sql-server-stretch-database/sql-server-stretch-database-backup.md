---
title: Sichern von Stretch-fähigen Datenbanken | Microsoft Docs
description: Erfahren Sie mehr über das Sichern von Stretch-fähigen Datenbanken.
services: sql-server-stretch-database
documentationcenter: ''
author: douglaslMS
manager: ''
editor: ''

ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: douglasl

---
# Sichern von Stretch-fähigen Datenbanken
Mit Sicherungskopien von Datenbanken können Sie Ihre Daten nach vielen Arten von Ausfällen, Fehlern und Notfällen wiederherstellen.

* Sie müssen die Stretch-fähigen SQL Server-Datenbanken sichern.  
* Microsoft Azure sichert automatisch die Remotedaten, die von der Stretch-Datenbank von SQL Server zu Azure migriert werden.

> [!NOTE]
> Die Datensicherung ist nur ein Teil einer vollständigen Lösung für hohe Verfügbarkeit und Geschäftskontinuität. Weitere Informationen zu hoher Verfügbarkeit finden Sie unter [Lösungen für hohe Verfügbarkeit](https://msdn.microsoft.com/library/ms190202.aspx).
> 
> 

## Sichern von SQL Server-Daten
Zum Sichern und Wiederherstellen von Stretch-fähigen SQL Server-Datenbanken können Sie weiterhin die bereits verwendeten SQL Server-Sicherungsmethoden nutzen. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen von SQL Server-Datenbanken](https://msdn.microsoft.com/library/ms187048.aspx).

Sicherungen einer Stretch-fähigen SQL Server-Datenbank enthalten nur die lokalen Daten sowie Daten, die zum Zeitpunkt der Sicherung für die Migration relevant sind. (Bei relevanten Daten handelt es sich um Daten, die noch nicht migriert wurden, für die jedoch basierend auf den Migrationseinstellungen in den Tabellen eine Migration nach Azure geplant ist.) Dies wird als **flache** Sicherung bezeichnet, in der nicht die Daten enthalten sind, die bereits zu Azure migriert wurden.

## Sichern der Azure-Remotedaten
Microsoft Azure sichert automatisch die Remotedaten, die von der Stretch-Datenbank von SQL Server zu Azure migriert werden.

### Mit automatischer Datensicherung reduziert Azure das Datenverlustrisiko
Der Azure-Dienst SQL Server Stretch-Datenbank schützt Ihre Remotedatenbanken mindestens alle 8 Stunden durch automatische Speichermomentaufnahmen. Jede Momentaufnahme wird 7 Tage lang beibehalten, um Ihnen eine Reihe von möglichen Wiederherstellungspunkten zu bieten.

### Mit Georedundanz reduziert Azure das Datenverlustrisiko
Azure-Datenbanksicherungen werden in georedundantem Azure Storage (RA-GRS) gespeichert und sind daher standardmäßig georedundant. Georedundanter Speicher repliziert Ihre Daten in eine sekundäre Region, die Hunderte von Kilometern von der primären Region entfernt ist. In primären und sekundären Regionen werden Ihre Daten jeweils dreimal in separaten Fehler- und Upgradedomänen repliziert. Dadurch wird sichergestellt, dass Ihre Daten selbst bei vollständigen regionalen Ausfällen oder bei Notfällen, in denen eine Azure-Region nicht zur Verfügung steht, erhalten bleiben.

### <a name="stretchRPO"></a>Die Stretch-Datenbank reduziert das Verlustrisiko für Ihre Azure-Daten, indem migrierte Zeilen vorübergehend beibehalten werden
Nachdem die Stretch-Datenbank relevante Zeilen aus SQL Server nach Azure migriert hat, behält sie diese Zeilen mindestens 8 Stunden lang in der Stagingtabelle bei. Wenn Sie eine Sicherungskopie Ihrer Azure-Datenbank wiederherstellen, verwendet die Stretch-Datenbank die in der Stagingtabelle gespeicherten Zeilen, um die SQL Server- und die Azure-Datenbanken abzustimmen.

Nach der Wiederherstellung einer Sicherung Ihrer Azure-Daten müssen Sie die gespeicherte Prozedur [sys.sp\_rda\_reauthorize\_db](https://msdn.microsoft.com/library/mt131016.aspx) ausführen, um die Verbindung zwischen der Stretch-fähigen SQL Server-Datenbank und der Azure-Remotedatenbank wiederherzustellen. Wenn Sie **sys.sp\_rda\_reauthorize\_db** ausführen, stimmt die Stretch-Datenbank automatisch die SQL Server- und die Azure-Datenbanken ab.

Um die Anzahl von Stunden zu erhöhen, für die migrierte Daten von der Stretch-Datenbank vorübergehend in der Stagingtabelle beibehalten werden, führen Sie die gespeicherte Prozedur [sys.sp\_rda\_set\_rpo\_duration](https://msdn.microsoft.com/library/mt707766.aspx) aus, und geben Sie eine Stundenanzahl von über 8 an. Um zu entscheiden, wie viele Daten beibehalten werden sollen, müssen Sie folgende Faktoren berücksichtigen:

* Die Häufigkeit der automatischen Azure-Sicherungen (mindestens alle 8 Stunden).
* Die erforderliche Zeit, um ein Problem nach seinem Auftreten zu erkennen und zu entscheiden, ob eine Sicherungskopie wiederhergestellt werden soll.
* Die Dauer des Azure-Wiederherstellungsvorgangs.

> [!NOTE]
> Wird die Datenmenge erhöht, die die Stretch-Datenbank vorübergehend in der Stagingtabelle beibehält, erhöht sich dadurch auch der erforderliche Speicherplatz auf dem SQL Server-Computer.
> 
> 

Um die Anzahl von Stunden zu überprüfen, für die die Daten von der Stretch-Datenbank vorübergehend in der Stagingtabelle beibehalten werden, führen Sie die gespeicherte Prozedur [sys.sp\_rda\_get\_rpo\_duration](https://msdn.microsoft.com/library/mt707767.aspx) aus.  

## Weitere Informationen
[Verwalten von Stretch Database und Behandeln von Problemen ](sql-server-stretch-database-manage.md)

[Sys.sp\_rda\_reauthorize\_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Sichern und Wiederherstellen von SQL Server-Datenbanken](https://msdn.microsoft.com/library/ms187048.aspx)

<!---HONumber=AcomDC_0615_2016-->