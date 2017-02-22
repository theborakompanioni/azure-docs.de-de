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
ms.date: 02/03/2017
ms.author: meetb
translationtype: Human Translation
ms.sourcegitcommit: 1f1c6c89c492d18e0678fa4650b6c5744dc9f7d1
ms.openlocfilehash: e1c3e7e0f6ca097e3ee41995defe5c1df666d39e


---
# <a name="connect-to-sql-database-by-using-php-on-windows"></a>Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von PHP unter Windows
[!INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]

In diesem Thema wird veranschaulicht, wie Sie von einer in PHP geschriebenen Clientanwendung, die unter Windows ausgeführt wird, eine Verbindung mit Azure SQL-Datenbank herstellen können.

## <a name="step-1--configure-development-environment"></a>Schritt 1: Konfigurieren der Entwicklungsumgebung
[Configure development environment for PHP development](https://docs.microsoft.com/sql/connect/php/step-1-configure-development-environment-for-php-development/)

## <a name="step-2-create-a-sql-database"></a>Schritt 2: Erstellen einer SQL-Datenbank
Auf der [Seite für erste Schritte](sql-database-get-started.md) erhalten Sie Informationen zum Erstellen einer Beispieldatenbank.  Sie sollten unbedingt die Anleitung zum Erstellen einer **AdventureWorks-Datenbankvorlage**befolgen. Die unten gezeigten Beispiele funktionieren nur mit dem **AdventureWorks-Schema**.

## <a name="step-3-get-connection-details"></a>Schritt 3: Abrufen der Verbindungsdetails
[!INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## <a name="step-4-run-sample-code"></a>Schritt 4: Ausführen von Beispielcode
* [Proof of concept connecting to SQL using PHP](https://docs.microsoft.com/sql/connect/php/step-3-proof-of-concept-connecting-to-sql-using-php/)
* [Connect resiliently to SQL with PHP](https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php/)

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie die [Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md)
* Weitere Informationen zum [Microsoft PHP-Treiber für SQL Server](https://docs.microsoft.com/sql/connect/php/microsoft-php-driver-for-sql-server/)
* Weitere Informationen über die Installation und Verwendung von PHP finden Sie unter [Accessing SQL Server Databases with PHP](http://social.technet.microsoft.com/wiki/contents/articles/1258.accessing-sql-server-databases-from-php.aspx)(Zugreifen auf SQL Server-Datenbanken mit PHP, in englischer Sprache).

## <a name="additional-resources"></a>Weitere Ressourcen
* [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Entdecken Sie alle [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)




<!--HONumber=Feb17_HO1-->


