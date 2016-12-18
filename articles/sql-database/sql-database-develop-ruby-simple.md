---
title: Herstellen von Verbindungen mit SQL-Datenbank mithilfe von Ruby | Microsoft Docs
description: "Geben Sie ein Ruby-Codebeispiel an, das zum Herstellen einer Verbindung mit Azure SQL-Datenbank ausgeführt werden kann."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 94fec528-58ba-4352-ba0d-25ae4b273e90
ms.service: sql-database
ms.custom: development
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 10/03/2016
ms.author: andrela
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: c66726e199e897b08825798031caf20de181d5a1


---
# <a name="connect-to-sql-database-by-using-ruby"></a>Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Ruby
[!INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]

In diesem Thema wird gezeigt, wie Sie mithilfe von Ruby eine Verbindung mit einer Azure SQL-Datenbank herstellen und Abfragen ausführen. Dieses Beispiel kann auf Windows-, Ubuntu Linux- oder Mac-Plattformen ausgeführt werden.

## <a name="step-1-configure-development-environment"></a>Schritt 1: Konfigurieren der Entwicklungsumgebung
[Voraussetzungen für die Verwendung des TinyTDS Ruby-Treibers für SQL Server](https://msdn.microsoft.com/library/mt711041.aspx)

## <a name="step-2-create-a-sql-database"></a>Schritt 2: Erstellen einer SQL-Datenbank
Auf der [Seite für erste Schritte](sql-database-get-started.md) erhalten Sie Informationen zum Erstellen einer Beispieldatenbank.  Sie sollten unbedingt die Anleitung zum Erstellen einer **AdventureWorks-Datenbankvorlage**befolgen. Die unten gezeigten Beispiele funktionieren nur mit dem **AdventureWorks-Schema**.

## <a name="step-3-get-connection-details"></a>Schritt 3: Abrufen der Verbindungsdetails
[!INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## <a name="step-4-run-sample-code"></a>Schritt 4: Ausführen von Beispielcode
[Proof of Concept connecting to SQL using Ruby](http://msdn.microsoft.com/library/mt715797.aspx)

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie die [Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md)
* Weitere Informationen zum [Microsoft Ruby-Treiber für SQL Server](https://msdn.microsoft.com/library/mt691981.aspx)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Entdecken Sie alle [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)




<!--HONumber=Nov16_HO3-->


