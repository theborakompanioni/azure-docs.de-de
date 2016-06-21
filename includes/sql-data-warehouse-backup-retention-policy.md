
<!--
includes/sql-data-warehouse-backup-retention-policies.md

Latest Freshness check:  2016-05-05 , barbkess.

As of circa 2016-04-22, the following topics might include this include:
articles/sql-data-warehouse/sql-data-warehouse-overview-expectations.md
articles/sql-data-warehouse/sql-data-warehouse-overview-backup-and-restore.md
-->
SQL Data Warehouse erstellt mit Azure Storage-Momentaufnahmen mindestens alle 8 Stunden Momentaufnahmen sämtlicher Livedaten. Diese Momentaufnahmen werden 7 Tage lang aufbewahrt. Dadurch können Sie Daten an einem von mindestens 21 Punkten innerhalb der letzten 7 Tage bis zu dem Zeitpunkt wiederherstellen, zu dem die letzte Momentaufnahme erstellt wurde.

SQL Data Warehouse erstellt eine Datenbankmomentaufnahme, bevor eine Datenbank gelöscht wird, und bewahrt diese sieben Tage lang auf. In diesem Fall werden keine Momentaufnahmen der Livedatenbank mehr beibehalten. Auf diese Weise können Sie eine gelöschte Datenbank bis zu dem Punkt wiederherstellen, zu dem sie gelöscht wurde.

<!---HONumber=AcomDC_0608_2016-->