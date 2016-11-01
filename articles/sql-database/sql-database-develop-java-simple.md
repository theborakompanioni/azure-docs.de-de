<properties
	pageTitle="Herstellen von Verbindungen mit der SQL-Datenbank mithilfe von Java mit JDBC unter Windows | Microsoft Azure"
	description="Zeigt ein Java-Codebeispiel zum Herstellen einer Verbindung mit Azure SQL-Datenbank. Das Beispiel verwendet JDBC und wird auf einem Clientcomputer unter Windows ausgeführt."
	services="sql-database"
	documentationCenter=""
	authors="LuisBosquez"
	manager="jhubbard"
	editor="genemi"/>


<tags
	ms.service="sql-database"
	ms.workload="drivers"
	ms.tgt_pltfrm="na"
	ms.devlang="java"
	ms.topic="article"
	ms.date="06/16/2016"
	ms.author="lbosq"/>


# Herstellen von Verbindungen mit der SQL-Datenbank mithilfe von Java mit JDBC unter Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]


Dieses Thema enthält ein Java-Codebeispiel zum Herstellen einer Verbindung mit Azure SQL-Datenbank. Das Java-Beispiel beruht auf dem Java Development Kit (JDK), Version 1.8. Das Beispiel stellt mithilfe des JDBC-Treibers eine Verbindung mit Azure SQL-Datenbank her.

## Schritt 1: Konfigurieren der Entwicklungsumgebung

[Configure development environment for Java development](https://msdn.microsoft.com/library/mt720658.aspx) (Konfigurieren der Entwicklungsumgebung für die Java-Entwicklung)

## Schritt 2: Erstellen einer SQL-Datenbank

Auf der [Seite für erste Schritte](sql-database-get-started.md) erhalten Sie Informationen zum Erstellen einer Datenbank.

## Schritt 3: Abrufen der Verbindungszeichenfolge

[AZURE.INCLUDE [sql-database-include-connection-string-jdbc-20-portalshots](../../includes/sql-database-include-connection-string-jdbc-20-portalshots.md)]

> [AZURE.NOTE] Wenn Sie den JTDS JDBC-Treiber verwenden, müssen Sie „ssl=require“ in der URL der Verbindungszeichenfolge hinzufügen, und Sie müssen die folgende Option für die JVM festlegen: „-Djsse.enableCBCProtection=false“. Diese JVM-Option deaktiviert einen Fix für ein Sicherheitsrisiko. Daher sollten Sie diese Option nur festlegen, wenn Sie das Risiko genau kennen.

## Schritt 4: Ausführen von Beispielcode

* [Proof of concept connecting to SQL using Java](https://msdn.microsoft.com/library/mt720656.aspx) (Machbarkeitsstudie für das Herstellen einer Verbindung mit SQL mithilfe von Java)

## Nächste Schritte

* Besuchen Sie das [Java Developer Center](/develop/java/).
* Lesen Sie die [Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md)
* Weitere Informationen zum [Microsoft JDBC-Treiber für SQL Server](https://msdn.microsoft.com/library/mt484311.aspx)

## Zusätzliche Ressourcen 

* [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Entdecken Sie alle [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/).

<!---HONumber=AcomDC_0622_2016-->