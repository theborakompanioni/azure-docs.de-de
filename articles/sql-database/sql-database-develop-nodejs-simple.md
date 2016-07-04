<properties
	pageTitle="Herstellen von Verbindungen mit SQL-Datenbank mithilfe von Node.js | Microsoft Azure"
	description="Zeigt ein Node.js-Codebeispiel zum Herstellen einer Verbindung mit Azure SQL-Datenbank."
	services="sql-database"
	documentationCenter=""
	authors="meet-bhagdev"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.workload="drivers"
	ms.tgt_pltfrm="na"
	ms.devlang="nodejs"
	ms.topic="article"
	ms.date="06/16/2016"
	ms.author="meetb"/>

# Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Node.js

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]

In diesem Thema wird gezeigt, wie Sie mithilfe von Node.js eine Verbindung mit einer Azure SQL-Datenbank herstellen und Abfragen ausführen. Dieses Beispiel kann auf Windows-, Ubuntu Linux- oder Mac-Plattformen ausgeführt werden.

## Schritt 1: Konfigurieren der Entwicklungsumgebung

[Prerequisites for using the Tedious Node.js Driver for SQL Server (Voraussetzungen für die Verwendung des Tedious Node.js-Treibers für SQL Server)](https://msdn.microsoft.com/library/mt652094.aspx)

## Schritt 2: Erstellen einer SQL-Datenbank

Auf der [Seite für erste Schritte](sql-database-get-started.md) erhalten Sie Informationen zum Erstellen einer Beispieldatenbank. Sie sollten unbedingt die Anleitung zum Erstellen einer **AdventureWorks-Datenbankvorlage** befolgen. Die unten gezeigten Beispiele funktionieren nur mit dem **AdventureWorks-Schema**.

## Schritt 3: Abrufen der Verbindungsdetails

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## Schritt 4: Ausführen von Beispielcode

[Proof of Concept connecting to SQL using Node.js](https://msdn.microsoft.com/library/mt715784.aspx) (Machbarkeitsstudie für das Herstellen einer Verbindung mit SQL mithilfe von Node.js)

## Nächste Schritte

* Lesen Sie die [Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md)
* Weitere Informationen zum [Microsoft Node.js-Treiber für SQL Server](https://msdn.microsoft.com/library/mt652093.aspx)

## Zusätzliche Ressourcen 

* [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Entdecken Sie alle [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/).

<!---HONumber=AcomDC_0622_2016-->