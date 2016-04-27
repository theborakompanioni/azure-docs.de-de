<properties
	pageTitle="Preise und Leistung elastischer SQL-Datenbankpools"
	description="Hier finden Sie Preisinformationen zu Pools für elastische Datenbanken."
	services="sql-database"
	documentationCenter=""
	authors="sidneyh"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.date="04/11/2016"
	ms.author="sidneyh"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>


# Abrechnungs- und Preisinformationen für Pools für elastische Datenbanken

Pools für elastische Datenbanken werden anhand der folgenden Merkmale abgerechnet:

- Ein elastischer Pool wird bei seiner Erstellung abgerechnet, auch wenn keine Datenbanken im Pool vorhanden sind.
- Ein elastischer Pool wird stündlich abgerechnet. Dies ist die gleiche Häufigkeit wie für Leistungsstufen von Einzeldatenbanken.
- Wenn ein elastischer Pool auf die Größe einer neuen Anzahl von eDTUs geändert wird, wird der Pool erst dann gemäß den neuen eDTUs berechnet, wenn die Größenänderung abgeschlossen ist. Dies erfolgt nach dem gleichen Muster wie beim Ändern der Leistungsstufe von eigenständigen Datenbanken.


- Der Preis für einen elastischen Pool basiert auf der Anzahl von eDTUs des Pools. Der Preis eines elastischen Pools hängt nicht von der Anzahl und Auslastung der darin enthaltenen elastischen Datenbanken ab.
- Der Preis wird wie folgt berechnet: (Anzahl von Pool-eDTUs) x (Einzelpreis pro eDTU).

Der eDTU-Einzelpreis für einen elastischen Anwendungspool ist höher als der DTU-Stückpreis für eine eigenständige Datenbank derselben Dienstebene. Weitere Informationen finden Sie unter [SQL-Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/).


Grundlegende Informationen zu eDTUs und Dienstebenen finden Sie unter [SQL-Datenbankoptionen und -leistung](sql-database-service-tiers.md).

## Nächste Schritte

- [Erstellen eines elastischen Datenbankpools](sql-database-elastic-pool-create-portal.md)
- [Einen Pool für elastische Datenbanken überwachen, verwalten und skalieren](sql-database-elastic-pool-manage-portal.md)
- [SQL-Datenbankoptionen und -leistung: Grundlegendes zum Angebot in den einzelnen Tarifen](sql-database-service-tiers.md)
- [PowerShell script for identifying databases suitable for an elastic database pool (PowerShell-Skript zum Ermitteln von Datenbanken mit Eignung für einen Pool für elastische Datenbanken)](sql-database-elastic-pool-database-assessment-powershell.md)

<!---HONumber=AcomDC_0413_2016-->