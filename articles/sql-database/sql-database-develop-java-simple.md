---
title: Herstellen von Verbindungen mit SQL-Datenbank mithilfe von Java mit JDBC unter Windows | Microsoft Docs
description: "Zeigt ein Java-Codebeispiel zum Herstellen einer Verbindung mit Azure SQL-Datenbank. Das Beispiel verwendet JDBC und wird auf einem Clientcomputer unter Windows ausgeführt."
services: sql-database
documentationcenter: 
author: LuisBosquez
manager: jhubbard
editor: genemi
ms.assetid: 08fc49b1-cd48-4dcc-a293-ff22a4d2d62c
ms.service: sql-database
ms.custom: development
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 02/03/2017
ms.author: lbosq
translationtype: Human Translation
ms.sourcegitcommit: 1f1c6c89c492d18e0678fa4650b6c5744dc9f7d1
ms.openlocfilehash: aafc89ca958e4bdc2ba52bf1a7d379e3a4cddf1c


---
# <a name="connect-to-sql-database-by-using-java-with-jdbc-on-windows"></a>Herstellen von Verbindungen mit der SQL-Datenbank mithilfe von Java mit JDBC unter Windows
[!INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]

Dieses Thema enthält ein Java-Codebeispiel zum Herstellen einer Verbindung mit Azure SQL-Datenbank. Das Java-Beispiel beruht auf dem Java Development Kit (JDK), Version 1.8. Das Beispiel stellt mithilfe des JDBC-Treibers eine Verbindung mit Azure SQL-Datenbank her.

## <a name="step-1--configure-development-environment"></a>Schritt 1: Konfigurieren der Entwicklungsumgebung
[Configure development environment for Java development](https://docs.microsoft.com/sql/connect/jdbc/step-1-configure-development-environment-for-java-development/)

## <a name="step-2-create-a-sql-database"></a>Schritt 2: Erstellen einer SQL-Datenbank
Auf der [Seite für erste Schritte](sql-database-get-started.md) erhalten Sie Informationen zum Erstellen einer Datenbank.  

## <a name="step-3-get-connection-string"></a>Schritt 3: Abrufen der Verbindungszeichenfolge
[!INCLUDE [sql-database-include-connection-string-jdbc-20-portalshots](../../includes/sql-database-include-connection-string-jdbc-20-portalshots.md)]

> [!NOTE]
> Wenn Sie den JTDS JDBC-Treiber verwenden, müssen Sie „ssl=require“ in der URL der Verbindungszeichenfolge hinzufügen, und Sie müssen die folgende Option für die JVM festlegen: „-Djsse.enableCBCProtection=false“. Diese JVM-Option deaktiviert einen Fix für ein Sicherheitsrisiko. Daher sollten Sie diese Option nur festlegen, wenn Sie das Risiko genau kennen.
> 
> 

## <a name="step-4-run-sample-code"></a>Schritt 4: Ausführen von Beispielcode
* [Proof of concept connecting to SQL using Java](https://docs.microsoft.com/sql/connect/jdbc/step-3-proof-of-concept-connecting-to-sql-using-java/)

## <a name="next-steps"></a>Nächste Schritte
* Besuchen Sie das [Java Developer Center](/develop/java/).
* Lesen Sie die [Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md)
* Weitere Informationen zum [Microsoft JDBC-Treiber für SQL Server](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server/)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Entdecken Sie alle [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)




<!--HONumber=Feb17_HO1-->


