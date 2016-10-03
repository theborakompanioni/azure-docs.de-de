<properties
	pageTitle="Herstellen von Verbindungen mit SQL-Datenbank mithilfe von Ruby | Microsoft Azure"
	description="Geben Sie ein Ruby-Codebeispiel an, das zum Herstellen einer Verbindung mit Azure SQL-Datenbank ausgeführt werden kann."
	services="sql-database"
	documentationCenter=""
	authors="ajlam"
	manager="jhubbard"
	editor=""/>


<tags
	ms.service="sql-database"
	ms.workload="drivers"
	ms.tgt_pltfrm="na"
	ms.devlang="ruby"
	ms.topic="article"
	ms.date="09/16/2016"
	ms.author="andrela"/>


# Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Ruby 

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]

In diesem Thema wird gezeigt, wie Sie mithilfe von Ruby eine Verbindung mit einer Azure SQL-Datenbank herstellen und Abfragen ausführen. Dieses Beispiel kann auf Windows-, Ubuntu Linux- oder Mac-Plattformen ausgeführt werden.

## Schritt 1: Konfigurieren der Entwicklungsumgebung

[Voraussetzungen für die Verwendung des TinyTDS Ruby-Treibers für SQL Server](https://msdn.microsoft.com/library/mt711041.aspx)

## Schritt 2: Erstellen einer SQL-Datenbank

Auf der [Seite für erste Schritte](sql-database-get-started.md) erhalten Sie Informationen zum Erstellen einer Beispieldatenbank. Sie sollten unbedingt die Anleitung zum Erstellen einer **AdventureWorks-Datenbankvorlage** befolgen. Die unten gezeigten Beispiele funktionieren nur mit dem **AdventureWorks-Schema**.

## Schritt 3: Abrufen der Verbindungsdetails

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## Schritt 4: Ausführen von Beispielcode

[Proof of Concept connecting to SQL using Ruby](http://msdn.microsoft.com/library/mt715797.aspx) (Machbarkeitsstudie für das Herstellen einer Verbindung mit SQL mithilfe von Ruby)

## Nächste Schritte

* Lesen Sie die [Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md)
* Weitere Informationen zum [Microsoft Ruby-Treiber für SQL Server](https://msdn.microsoft.com/library/mt691981.aspx)

## Zusätzliche Ressourcen 

* [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Entdecken Sie alle [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/).

<!---HONumber=AcomDC_0921_2016-->