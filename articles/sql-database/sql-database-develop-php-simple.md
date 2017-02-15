---
title: Herstellen von Verbindungen mit SQL-Datenbank mithilfe von PHP unter Windows | Microsoft Docs
description: "Enthält ein PHP-Beispielprogramm, das eine Verbindung von einem Windows-Client mit Azure SQL-Datenbank herstellt, sowie Links zu den auf dem Client erforderlichen Softwarekomponenten."
services: sql-database
documentationcenter: 
author: meet-bhagdev
manager: jhubbard
editor: 
ms.assetid: 4e71db4a-a22f-4f1c-83e5-4a34a036ecf3
ms.service: sql-database
ms.custom: development
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: article
ms.date: 10/03/2016
ms.author: meetb
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 162c89b07a0bc690600f0cc59e3f4c70e02eac91


---
# <a name="connect-to-sql-database-by-using-php-on-windows"></a>Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von PHP unter Windows
[!INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]

In diesem Thema wird veranschaulicht, wie Sie von einer in PHP geschriebenen Clientanwendung, die unter Windows ausgeführt wird, eine Verbindung mit Azure SQL-Datenbank herstellen können.

## <a name="step-1--configure-development-environment"></a>Schritt 1: Konfigurieren der Entwicklungsumgebung
[Configure development environment for PHP development](https://msdn.microsoft.com/library/mt720663.aspx)

## <a name="step-2-create-a-sql-database"></a>Schritt 2: Erstellen einer SQL-Datenbank
Auf der [Seite für erste Schritte](sql-database-get-started.md) erhalten Sie Informationen zum Erstellen einer Beispieldatenbank.  Sie sollten unbedingt die Anleitung zum Erstellen einer **AdventureWorks-Datenbankvorlage**befolgen. Die unten gezeigten Beispiele funktionieren nur mit dem **AdventureWorks-Schema**.

## <a name="step-3-get-connection-details"></a>Schritt 3: Abrufen der Verbindungsdetails
[!INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## <a name="step-4-run-sample-code"></a>Schritt 4: Ausführen von Beispielcode
* [Proof of concept connecting to SQL using PHP](https://msdn.microsoft.com/library/mt720665.aspx)
* [Connect resiliently to SQL with PHP](https://msdn.microsoft.com/library/mt720667.aspx)

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie die [Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md)
* Weitere Informationen zum [Microsoft PHP-Treiber für SQL Server](https://msdn.microsoft.com/library/dn865013.aspx)
* Weitere Informationen über die Installation und Verwendung von PHP finden Sie unter [Accessing SQL Server Databases with PHP](http://social.technet.microsoft.com/wiki/contents/articles/1258.accessing-sql-server-databases-from-php.aspx)(Zugreifen auf SQL Server-Datenbanken mit PHP, in englischer Sprache).

## <a name="additional-resources"></a>Weitere Ressourcen
* [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Entdecken Sie alle [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)




<!--HONumber=Nov16_HO3-->


