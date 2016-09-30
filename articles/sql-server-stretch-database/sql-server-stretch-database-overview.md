<properties
	pageTitle="Stretch-Datenbank-Übersicht | Microsoft Azure"
	description="Erfahren Sie, wie Stretch-Datenbank Ihre inaktiven Daten transparent und sicher zu Microsoft Azure Cloud migriert."
	services="sql-server-stretch-database"
	documentationCenter=""
	authors="douglaslMS"
	manager=""
	editor=""/>

<tags
	ms.service="sql-server-stretch-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="06/27/2016"
	ms.author="douglasl"/>

# Übersicht über Stretch-Datenbank

Stretch-Datenbank migriert Ihre inaktiven Daten (Cold Data) transparent und sicher zu Microsoft Azure Cloud.

Wenn Sie sofort mit Stretch-Datenbank beginnen möchten, lesen Sie [Erste Schritte mit der Datenbank aktivieren für Stretch-Assistenten](sql-server-stretch-database-wizard.md).

## Was sind die Vorteile von Stretch-Datenbank?
Stretch-Datenbank bietet die folgenden Vorteile:

### Bietet kostengünstige Verfügbarkeit für inaktive Daten
Verlagern Sie aktive und inaktive Transaktionsdaten mit SQL Server Stretch-Datenbank dynamisch von SQL Server zu Microsoft Azure. Im Gegensatz zu anderen Arten der Speicherung von inaktiven Daten, sind Ihre Daten immer online und jederzeit abfragbar. Sie können eine längere Dauer der Datenaufbewahrung anbieten, ohne für große Tabellen wie eine Kundenauftragshistorie hohe Ausgaben tätigen zu müssen. Profitieren Sie vom kostengünstigen Azure statt teure lokale Speicher hochzuskalieren. Wählen Sie einen Tarif, und konfigurieren Sie die Einstellungen im Azure-Portal, um die Kontrolle über die Kosten zu behalten. Skalieren Sie je nach Bedarf hoch oder herunter. Einzelheiten finden Sie auf der Seite [SQL Server Stretch-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### Keine Änderungen von Abfragen oder Anwendungen erforderlich
Greifen Sie nahtlos auf Ihre SQL Server-Daten zu – unabhängig davon, ob diese lokal gespeichert oder in die Cloud verlagert wurden. Sie legen die Richtlinien dafür fest, wo Daten gespeichert werden, und wie SQL Server das Verschieben von Daten im Hintergrund handhabt. Die ganze Tabelle ist immer online und jederzeit abfragbar. Die Stretch-Datenbank benötigt außerdem keine Änderungen der vorhandenen Abfragen oder Anwendungen – der Speicherort der Daten ist für die Anwendung vollständig transparent.

### Optimierung der Wartung lokaler Daten
Verringern Sie den lokalen Wartungs- und Speicheraufwand für Ihre Daten. Die Sicherungen für Ihre lokalen Daten werden schneller durchgeführt und enden innerhalb des Wartungsfensters. Sicherungen für den Teil Ihrer Daten, der sich in der Cloud befindet, werden automatisch ausgeführt. Ihr Bedarf an lokalem Speicher verringert sich in hohem Umfang. Azure-Speicher kann bis zu 80 % günstiger sein als der Ausbau von lokalen SSD.

### Datensicherheit auch während der Migration
Sie können Ihre wichtigsten Anwendungen ganz beruhigt auf sicherem Wege in die Cloud verlagern. Always Encrypted in SQL Server verschlüsselt Ihre sich in Bewegung befindlichen Daten. Sicherheit auf Zeilenebene (RLS) und andere erweiterte Sicherheitsfunktionen von SQL Server funktionieren auch mit Stretch-Datenbank zum Schutz Ihrer Daten.

## Wie funktioniert Stretch-Datenbank?
Nachdem Sie Stretch-Datenbank für eine SQL Server-Instanz, eine Datenbank und mindestens eine Tabelle aktiviert haben, beginnt Stretch-Datenbank im Hintergrund damit, inaktive Daten zu Azure zu migrieren.

-   Wenn Sie inaktive Daten in einer separaten Tabelle speichern, können Sie die ganze Tabelle migrieren.

-   Wenn die Tabelle sowohl aktive als auch inaktive Daten enthält, können Sie eine Filterfunktion zum Auswählen der zu migrierenden Zeilen angeben.

**Sie müssen vorhandene Abfragen und Clientanwendungen nicht ändern.** Sie haben weiterhin unkomplizierten Zugriff, sowohl auf lokale als auch auf Remotedaten, sogar während der Migration. Bei Remoteabfragen ist mit einer kurzen Latenzzeit zu rechnen, die allerdings nur bei der Abfrage von inaktiven Daten auftritt.

**Stretch-Datenbank stellt sicher, dass keine Daten verloren gehen**, falls während der Migration ein Failover auftritt. Stretch-Datenbank enthält außerdem die Wiederholungslogik, um mit Verbindungsproblemen umzugehen, die während der Migration auftreten können. Eine dynamische Verwaltungsansicht zeigt den Status der Migration.

**Sie können die Datenmigration anhalten**, um Probleme auf dem lokalen Server zu behandeln oder um die verfügbare Netzwerkbandbreite zu maximieren.

![Stretch-Datenbank-Übersicht][StretchOverviewImage1]

## Eignet sich Stretch-Datenbank für Sie?
Wenn die folgenden Aussagen auf Sie zutreffen, kann Ihnen Stretch-Datenbank helfen, Ihre Ansprüche umzusetzen und Ihre Probleme zu lösen.

|Wenn Sie ein Entscheidungsträger sind|Wenn Sie ein Datenbankadministrator sind|
|------------------------------|-------------------|
|Ich muss Transaktionsdaten lange aufbewahren.|Die Größe meiner Tabellen artet aus.|
|Manchmal ist es erforderlich, die inaktiven Daten abzufragen.|Meine Benutzer sagen, dass sie Zugriff auf inaktive Daten haben möchten, aber sie nutzen diesen nur selten.|
|Ich habe Apps, inklusive älteren Apps, die ich nicht aktualisieren möchte.|Ich muss immer wieder neuen Speicherplatz kaufen und hinzufügen.|
|Ich will eine Möglichkeit finden, um beim Speichern Geld zu sparen.|Innerhalb meiner SLA kann ich solch große Tabellen weder sichern noch wiederherstellen.|

## Welche Art von Datenbanken und Tabellen eignen sich für Stretch-Datenbank?
Stretch-Datenbank ist für Transaktionsdatenbanken mit großen Mengen an inaktiven Daten gedacht, wobei diese Datenmengen in der Regel in wenigen Tabellen gespeichert werden. Diese Tabellen können mehr als eine Milliarde Zeilen enthalten.

Falls Sie die Funktion „temporale Tabelle“ von SQL Server 2016 verwenden, verwenden Sie Stretch-Datenbank, um die ganze oder einen Teil der zugehörigen Verlaufstabelle zur kostengünstigen Speicherung in Azure zu migrieren. Weitere Informationen finden Sie unter [Verwalten der Beibehaltung von Verlaufsdaten in temporalen Tabellen mit Systemversionsverwaltung](https://msdn.microsoft.com/library/mt637341.aspx).

Verwenden Sie den Stretch-Datenbank-Ratgeber, eine Funktion von SQL Server 2016-Upgraderatgeber, zum Identifizieren von Datenbanken und Tabellen für Stretch-Datenbank. Weitere Informationen finden Sie unter [Identifizieren von Datenbanken und Tabellen für Stretch-Datenbank](sql-server-stretch-database-identify-databases.md). Weitere Informationen zu möglichen Blockierungsproblemen finden Sie unter [Einschränkungen für Stretch-Datenbank](sql-server-stretch-database-limitations.md).

## Testen Sie Stretch-Datenbank
**Testen Sie Stretch-Datenbank mit der AdventureWorks-Beispieldatenbank.** Um die AdventureWorks-Beispieldatenbank zu erhalten, laden Sie [hier](https://www.microsoft.com/download/details.aspx?id=49502) mindestens die Datenbankdatei sowie die Beispiel- und Skriptdatei herunter. Nachdem Sie die Beispieldatenbank auf einer Instanz von SQL Server 2016 wiederhergestellt haben, entpacken Sie die Datei „Stretch DB Samples“ aus dem Ordner „Stretch DB “. Führen Sie die Skripts in dieser Datei aus, um den von Ihren Daten verwendeten Speicherplatz vor und nach der Aktivierung von Stretch-Datenbank zu überprüfen, den Fortschritt der Datenmigration zu verfolgen, und um zu bestätigen, dass Sie weiterhin vorhandene Daten abfragen und neue Daten einfügen können, sowohl während als auch nach der Datenmigration.

## Nächster Schritt
**Identifizieren Sie Datenbanken und Tabellen, die sich für Stretch-Datenbank eignen.** Laden Sie SQL Server 2016-Upgraderatgeber herunter, und führen Sie den Stretch-Datenbank-Ratgeber aus, um Datenbanken und Tabellen, die sich für Stretch-Datenbank eignen, zu identifizieren. Der Stretch-Datenbank-Ratgeber erkennt auch Blockierungsprobleme. Weitere Informationen finden Sie unter [Identifizieren von Datenbanken und Tabellen für Stretch-Datenbank](sql-server-stretch-database-identify-databases.md).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png

<!----HONumber=AcomDC_0629_2016--->