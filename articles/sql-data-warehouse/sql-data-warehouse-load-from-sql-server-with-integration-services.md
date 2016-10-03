<properties
   pageTitle="Laden von Daten aus SQL Server in Azure SQL Data Warehouse (SSIS) | Microsoft Azure"
   description="Hier erfahren Sie, wie Sie ein SQL Server Integration Services (SSIS)-Paket erstellen, um Daten aus verschiedensten Datenquellen in SQL Data Warehouse zu verschieben."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;sonyama;barbkess"/>

# Laden von Daten aus SQL Server in Azure SQL Data Warehouse (SSIS)

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [bcp](sql-data-warehouse-load-from-sql-server-with-bcp.md)


Erstellen eines SQL Server Integration Services (SSIS)-Pakets zum Laden von Daten von SQL Server in Azure SQL Data Warehouse. Im Rahmen des SSIS-Datenflusses können die Daten optional auch umstrukturiert, transformiert und bereinigt werden.

In diesem Lernprogramm lernen Sie Folgendes:

- Erstellen eines neuen Integration Services-Projekts in Visual Studio
- Herstellen einer Verbindung mit Datenquellen wie SQL Server (als Quelle) und SQL Data Warehouse (als Ziel)
- Entwerfen eines SSIS-Pakets, um Daten aus der Quelle in das Ziel zu laden
- Ausführen des SSIS-Pakets, um die Daten zu laden

Dieses Tutorial verwendet SQL Server als Datenquelle. SQL Server kann lokal oder auf einem virtuellen Azure-Computer ausgeführt werden.

## Grundlegende Konzepte

Das Paket ist die Arbeitseinheit in SSIS. Zusammengehörige Pakete werden in Projekten gruppiert. Projekte und Paketentwürfe werden in Visual Studio mit SQL Server Data Tools erstellt. Der Entwurfsprozess ist ein visueller Prozess, bei dem Sie Komponenten per Drag&Drop aus der Toolbox auf die Entwurfsoberfläche ziehen, miteinander verknüpfen und ihre Eigenschaften festlegen. Das fertig gestellte Paket kann zur Nutzung umfassender Verwaltungs-, Überwachungs- und Sicherheitsfeatures optional für SQL Server bereitgestellt werden.

## Optionen beim Laden von Daten mit SSIS

SQL Server Integration Services (SSIS) ist ein flexibles Toolset und bietet verschiedene Optionen, um eine Verbindung mit SQL Data Warehouse herzustellen und Daten in SQL Data Warehouse zu laden:

1. Verwenden Sie ein ADO.NET-Ziel für die Verbindung mit SQL Data Warehouse. In diesem Tutorial wird ein ADO.NET-Ziel verwendet, da hierbei die wenigsten Konfigurationsoptionen zur Verfügung stehen.
2. Verwenden Sie ein OLE DB-Ziel für die Verbindung mit SQL Data Warehouse. Diese Option bietet unter Umständen eine etwas bessere Leistung als das ADO.NET-Ziel.
3. Verwenden Sie den Azure BLOB-Uploadvorgang, um die Daten im Azure-Blobspeicher bereitzustellen. Verwenden Sie dann den SSIS-Task „SQL ausführen“, um ein Polybase-Skript zu starten, das die Daten in SQL Data Warehouse lädt. Von den drei hier aufgeführten Optionen bietet diese Option die beste Leistung. Den Azure Blob-Uploadvorgang erhalten Sie durch Herunterladen von [Microsoft SQL Server 2016 Integration Services Feature Pack für Azure][]. Weitere Informationen zu PolyBase finden Sie im [PolyBase-Handbuch][].

## Vorbereitung

Für dieses Lernprogramm ist Folgendes erforderlich:

1. **SQL Server Integration Services (SSIS)**. SSIS ist eine Komponente von SQL Server und setzt eine Evaluierungsversion oder eine lizenzierte Version von SQL Server voraus. Eine Evaluierungsversion von SQL Server 2016 Preview finden Sie unter [SQL Server-Evaluierungsversionen][].
2. **Visual Studio** Die kostenlose Visual Studio 2015 Community Edition finden Sie unter [Visual Studio Community][].
3. **SQL Server Data Tools (SSDT) für Visual Studio**. SQL Server Data Tools für Visual Studio 2015 ist unter [Download der neuesten SQL Server-Datatools][] erhältlich.
4. **Beispieldaten**. In diesem Tutorial werden als Quelldaten in SQL Server gespeicherte Beispieldaten aus der AdventureWorks-Beispieldatenbank verwendet, die in SQL Data Warehouse geladen werden sollen. Die AdventureWorks-Beispieldatenbank finden Sie unter [AdventureWorks 2014 Sample Databases][] \(Adventure Works 2014 Beispieldatenbank).
5. **Eine SQL Data Warehouse-Datenbank und entsprechende Berechtigungen**. In diesem Tutorial wird eine Verbindung mit einer SQL Data Warehouse-Instanz hergestellt, und es werden Daten in diese Instanz geladen. Sie müssen über Berechtigungen zum Erstellen einer Tabelle und zum Laden von Daten verfügen.
6. **Eine Firewallregel**. Für SQL Data Warehouse muss eine Firewallregel mit der IP-Adresse Ihres lokalen Computers erstellt werden, um Daten an SQL Data Warehouse hochladen zu können.

## Schritt 1: Erstellen eines neuen Integration Services-Projekts

1. Starten Sie Visual Studio 2015.
2. Wählen Sie im Menü **Datei** die Option „Neu“.| Project**.
3. Navigieren Sie zu „Installiert“.| Templates | Business Intelligence | Integration Services** project types.
4. Wählen Sie **Integration Services-Projekt** aus. Geben Sie Werte für **Name** und **Speicherort** ein, und wählen Sie anschließend **OK**.

Visual Studio wird geöffnet und erstellt ein neues SQL Server Integration Services (SSIS)-Projekt. Anschließend öffnet Visual Studio den Designer für das einzelne neue SSIS-Paket (Package.dtsx) des Projekts. Daraufhin stehen folgende Bildschirmbereiche zur Verfügung:

- Links befindet sich die **Toolbox** mit den SSIS-Komponenten.
- In der Mitte befindet sich die Entwurfsoberfläche mit mehreren Registerkarten. In der Regel verwenden Sie mindestens die Registerkarten **Ablaufsteuerung** und **Datenfluss**.
- Auf der rechten Seite befinden sich der **Projektmappen-Explorer** und der Bereich **Eigenschaften**.

    ![][01]

## Schritt 2: Erstellen des grundlegenden Datenflusses

1. Ziehen Sie einen Datenflusstask aus der Toolbox in die Mitte der Entwurfsoberfläche (auf der Registerkarte **Ablaufsteuerung**).

    ![][02]

2. Doppelklicken Sie auf den Datenflusstask, um zur Registerkarte „Datenfluss“ zu wechseln.
3. Ziehen Sie in der Toolbox aus der Liste mit anderen Quellen eine ADO.NET-Quelle auf die Entwurfsoberfläche. Ändern Sie den Namen im Bereich **Eigenschaften** in **SQL Server source**, während der Quelladapter noch ausgewählt ist.
4. Ziehen Sie in der Toolbox aus der Liste mit anderen Quellen ein ADO.NET-Ziel auf die Entwurfsoberfläche, und platzieren Sie es unter der ADO.NET-Quelle. Ändern Sie den Namen im Bereich **Eigenschaften** in **SQL DW destination**, während der Zieladapter noch ausgewählt ist.

    ![][09]

## Schritt 3: Konfigurieren des Quelladapters

1. Doppelklicken Sie auf den Quelladapter, um den **ADO.NET-Quellen-Editor** zu öffnen.

    ![][03]

2. Klicken Sie auf der Registerkarte **Verbindungs-Manager** des **ADO.NET-Quellen-Editors** neben der Liste **ADO.NET-Verbindungs-Manager** auf die Schaltfläche **Neu**, um das Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren** zu öffnen. Erstellen Sie dann Verbindungseinstellungen für die SQL Server-Datenbank, die in diesem Tutorial als Datenquelle fungiert.

    ![][04]

3. Klicken Sie im Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren** auf die Schaltfläche **Neu**, um das Dialogfeld **Verbindungs-Manager** zu öffnen und eine neue Datenverbindung zu erstellen.

    ![][05]

4. Gehen Sie im Dialogfeld **Verbindungs-Manager** wie folgt vor:

    1. Wählen Sie unter **Anbieter** den Datenanbieter „SqlClient“ aus.
    2. Geben Sie unter **Servername** den SQL Server-Namen ein.
    3. Wählen Sie im Abschnitt für die **Serveranmeldung** Authentifizierungsinformationen aus, oder geben Sie sie ein.
    4. Wählen Sie im Abschnitt für die **Datenbankverbindung** die AdventureWorks-Beispieldatenbank aus.
    5. Klicken Sie auf **Test Connection**.
    
        ![][06]
    
    6. Klicken Sie im Dialogfeld mit den Ergebnissen des Verbindungstests auf **OK**, um zum Dialogfeld **Verbindungs-Manager** zurückzukehren.
    7. Klicken Sie im Dialogfeld **Verbindungs-Manager** auf **OK**, um zum Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren** zurückzukehren.
 
5. Klicken Sie im Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren** auf **OK**, um zum **ADO.NET-Quellen-Editor** zurückzukehren.
6. Wählen Sie im **ADO.NET-Quellen-Editor** in der Liste **Name der Tabelle oder Sicht** die Tabelle **Sales.SalesOrderDetail** aus.

    ![][07]

7. Klicken Sie auf **Vorschau**, um im Dialogfeld **Vorschau der Abfrageergebnisse anzeigen** die ersten 200 Datenzeilen der Quelltabelle anzuzeigen.

    ![][08]

8. Klicken Sie im Dialogfeld **Vorschau der Abfrageergebnisse anzeigen** auf **Schließen**, um zum **ADO.NET-Quellen-Editor** zurückzukehren.
9. Klicken Sie im **ADO.NET-Quellen-Editor** auf **OK**, um die Konfiguration der Datenquelle abzuschließen.

## Schritt 4: Verbinden des Quelladapters mit dem Zieladapter

1. Wählen Sie auf der Entwurfsoberfläche den Quelladapter aus.
2. Ziehen Sie den vom Quelladapter ausgehenden blauen Pfeil zum Zieladapter, sodass er dort andockt.

    ![][10]

    In einem SSIS-Paket wird zwischen Quelle und Ziel üblicherweise eine ganze Reihe verschiedener Komponenten aus der SSIS-Toolbox verwendet, um die Daten beim Durchlaufen des SSIS-Datenflusses neu zu strukturieren, zu transformieren und zu bereinigen. Hier verbinden wir die Quelle allerdings direkt mit dem Ziel, um das Beispiel möglichst einfach zu halten.

## Schritt 5: Konfigurieren des Zieladapters

1. Doppelklicken Sie auf den Zieladapter, um den **ADO.NET-Ziel-Editor** zu öffnen.

    ![][11]

2. Klicken Sie auf der Registerkarte **Verbindungs-Manager** des **ADO.NET-Ziel-Editors** neben der Liste **Verbindungs-Manager** auf die Schaltfläche **Neu**, um das Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren** zu öffnen. Erstellen Sie dann Verbindungseinstellungen für die Azure SQL Data Warehouse-Datenbank, die in diesem Tutorial als Datenziel fungiert.
3. Klicken Sie im Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren** auf die Schaltfläche **Neu**, um das Dialogfeld **Verbindungs-Manager** zu öffnen und eine neue Datenverbindung zu erstellen.
4. Gehen Sie im Dialogfeld **Verbindungs-Manager** wie folgt vor:
    1. Wählen Sie unter **Anbieter** den Datenanbieter „SqlClient“ aus.
    2. Geben Sie unter **Servername** den SQL Data Warehouse-Namen ein.
    3. Wählen Sie im Abschnitt für die **Serveranmeldung** die Option **SQL Server-Authentifizierung** aus, und geben Sie die Authentifizierungsinformationen ein.
    4. Wählen Sie im Abschnitt für die **Datenbankverbindung** eine vorhandene SQL Data Warehouse-Datenbank aus.
    5. Klicken Sie auf **Test Connection**.
    6. Klicken Sie im Dialogfeld mit den Ergebnissen des Verbindungstests auf **OK**, um zum Dialogfeld **Verbindungs-Manager** zurückzukehren.
    7. Klicken Sie im Dialogfeld **Verbindungs-Manager** auf **OK**, um zum Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren** zurückzukehren.
5. Klicken Sie im Dialogfeld **ADO.NET-Verbindungs-Manager konfigurieren** auf **OK**, um zum **ADO.NET-Ziel-Editor** zurückzukehren.
6. Klicken Sie im **ADO.NET-Ziel-Editor** neben der Liste zum **Verwenden einer Tabelle oder Sicht** auf **Neu**, um das Dialogfeld **Tabelle erstellen** zu öffnen und damit eine neue Zieltabelle mit einer der Quelltabelle entsprechenden Spaltenliste zu erstellen.

    ![][12a]

7. Gehen Sie im Dialogfeld **Tabelle erstellen** wie folgt vor:

    1. Ändern Sie den Namen der Zieltabelle in **SalesOrderDetail**.
    2. Entfernen Sie die Spalte **rowguid**. Der Datentyp **uniqueidentifier** wird in SQL Data Warehouse nicht unterstützt.
    3. Ändern Sie den Datentyp der Spalte **LineTotal** in **money**. Der Datentyp **decimal** wird in SQL Data Warehouse nicht unterstützt. Informationen zu unterstützten Datentypen finden Sie unter [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][].
    
        ![][12b]
    
    4. Klicken Sie auf **OK**, um die Tabelle zu erstellen und wieder zum **ADO.NET-Ziel-Editor** zurückzukehren.

8. Wählen Sie im **ADO.NET-Ziel-Editor** die Registerkarte **Zuordnungen** aus, um die Zuordnung der Quell- und Zielspalten anzuzeigen.

    ![][13]

9. Klicken Sie auf **OK**, um die Konfiguration der Datenquelle abzuschließen.

## Schritt 6: Ausführen des Pakets, um die Daten zu laden

Führen Sie das Paket aus, indem Sie entweder auf der Symbolleiste auf die Schaltfläche **Starten** klicken oder im Menü **Debuggen** eine der **Ausführungsoptionen** auswählen.

Wenn die Paketausführung beginnt, wird durch gelbe, sich drehende Zahnräder angegeben, dass eine Aktivität stattfindet, und auch die Anzahl bereits verarbeiteter Zeilen wird angezeigt.

![][14]

Nach Abschluss der Paketausführung geben grüne Häkchen an, dass der Vorgang erfolgreich war, und es wird die Gesamtanzahl von Zeilen angezeigt, die aus der Quelle in das Ziel geladen wurden.

![][15]

Glückwunsch! Sie haben mit SQL Server Integration Services erfolgreich Daten in Azure SQL Data Warehouse geladen.

## Nächste Schritte

- Informieren Sie sich ausführlicher über den SSIS-Datenfluss. Beginnen Sie hier: [Datenfluss][].
- Informieren Sie sich über direkt in der Entwurfsumgebung verfügbare Debug- und Problembehandlungsmöglichkeiten für Ihre Pakete. Beginnen Sie hier: [Tools zur Problembehandlung für die Paketentwicklung][].
- Informieren Sie sich darüber, wie Sie Ihre SSIS-Projekte und -Pakete für Integration Services-Server oder andere Speicherorte bereitstellen. Beginnen Sie hier: [Bereitstellung von Projekten und Paketen][].

<!-- Image references -->
[01]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[PolyBase-Handbuch]: https://msdn.microsoft.com/library/mt143171.aspx
[Download der neuesten SQL Server-Datatools]: https://msdn.microsoft.com/library/mt204009.aspx
[CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)]: https://msdn.microsoft.com/library/mt203953.aspx
[Datenfluss]: https://msdn.microsoft.com/library/ms140080.aspx
[Tools zur Problembehandlung für die Paketentwicklung]: https://msdn.microsoft.com/library/ms137625.aspx
[Bereitstellung von Projekten und Paketen]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Microsoft SQL Server 2016 Integration Services Feature Pack für Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[SQL Server-Evaluierungsversionen]: https://www.microsoft.com/de-DE/evalcenter/evaluate-sql-server-2016
[Visual Studio Community]: https://www.visualstudio.com/de-DE/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 Sample Databases]: https://msftdbprodsamples.codeplex.com/releases/view/125550

<!---HONumber=AcomDC_0810_2016-->