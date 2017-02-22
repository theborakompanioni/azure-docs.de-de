---
title: Herstellen von Verbindungen mit SQL-Datenbank mithilfe von Node.js | Microsoft Docs
description: Zeigt ein Node.js-Codebeispiel zum Herstellen einer Verbindung mit Azure SQL-Datenbank.
services: sql-database
documentationcenter: 
author: LuisBosquez
manager: jhubbard
editor: 
ms.assetid: 53f70e37-5eb4-400d-972e-dd7ea0caacd4
ms.service: sql-database
ms.custom: development
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 02/03/2017
ms.author: lbosq
translationtype: Human Translation
ms.sourcegitcommit: 1f1c6c89c492d18e0678fa4650b6c5744dc9f7d1
ms.openlocfilehash: 6c3f6bc00d147bc498a859560ffe4719ecf888bf


---
# <a name="connect-to-sql-database-by-using-nodejs"></a>Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Node.js
[!INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]

In diesem Thema wird gezeigt, wie Sie mithilfe von Node.js eine Verbindung mit einer Azure SQL-Datenbank herstellen und Abfragen ausführen. Dieses Beispiel kann auf Windows-, Ubuntu Linux- oder Mac-Plattformen ausgeführt werden.

## <a name="step-1-configure-development-environment"></a>Schritt 1: Konfigurieren der Entwicklungsumgebung
[Prerequisites for using the Tedious Node.js Driver for SQL Server (Voraussetzungen für die Verwendung des Tedious Node.js-Treibers für SQL Server)](https://docs.microsoft.com/sql/connect/node-js/step-1-configure-development-environment-for-node-js-development/)

## <a name="step-2-create-a-sql-database"></a>Schritt 2: Erstellen einer SQL-Datenbank
Auf der [Seite für erste Schritte](sql-database-get-started.md) erhalten Sie Informationen zum Erstellen einer Beispieldatenbank.  Sie sollten unbedingt die Anleitung zum Erstellen einer **AdventureWorks-Datenbankvorlage**befolgen. Die unten gezeigten Beispiele funktionieren nur mit dem **AdventureWorks-Schema**.

## <a name="step-3-get-connection-details"></a>Schritt 3: Abrufen der Verbindungsdetails
[!INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## <a name="step-4-run-sample-code"></a>Schritt 4: Ausführen von Beispielcode
[Proof of Concept connecting to SQL using Node.js](https://docs.microsoft.com/sql/connect/node-js/step-3-proof-of-concept-connecting-to-sql-using-node-js/)

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie die [Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md)
* Weitere Informationen zum [Microsoft Node.js-Treiber für SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Entdecken Sie alle [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)




<!--HONumber=Feb17_HO1-->


