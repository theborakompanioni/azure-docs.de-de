---
title: "Herstellen einer Verbindung mit Azure SQL-Datenbank über Visual Studio | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie in Visual Studio eine Verbindung mit einer Azure SQL-Datenbank herstellen.
services: sql-database
keywords: Herstellen einer Verbindung mit der Datenbank
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 676bd799-a571-4bb8-848b-fb1720007866
ms.service: sql-database
ms.custom: development
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: 6789662847197ff3abb44df2d65a8ab761dfc1e7
ms.lasthandoff: 03/04/2017


---
# <a name="connect-to-a-sql-database-with-visual-studio"></a>Herstellen der Verbindung mit einer SQL-Datenbank mit Visual Studio

Erfahren Sie, wie Sie in Visual Studio eine Verbindung mit einer Azure SQL-Datenbank herstellen. 

## <a name="prerequisites"></a>Voraussetzungen
Zum Herstellen einer Verbindung mit einer SQL-Datenbank mit Visual Studio benötigen Sie Folgendes: 

* Eine SQL-Datenbank, mit der eine Verbindung hergestellt werden kann. In diesem Artikel wird die **AdventureWorks** -Beispieldatenbank verwendet. Die AdventureWorks-Beispieldatenbank finden Sie unter [Erstellen Sie die Demodatenbank](sql-database-get-started.md).
* Visual Studio 2013, Update 4 (oder höher). Microsoft bietet Visual Studio Community jetzt *kostenlos*an.
  
  * [Download von Visual Studio Community](http://www.visualstudio.com/products/visual-studio-community-vs)
  * [Weitere Optionen für kostenlose Visual Studio-Versionen](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)

## <a name="open-visual-studio-from-the-azure-portal"></a>Öffnen von Visual Studio über das Azure-Portal
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie auf **Weitere Dienste** > **SQL-Datenbanken**.
3. Öffnen Sie das Blatt der **AdventureWorks** -Datenbank, indem Sie nach der *AdventureWorks* -Datenbank suchen und darauf klicken.
4. Klicken Sie oben auf dem Datenbankblatt auf die Schaltfläche **Tools** :
   
    ![Neue Abfrage Verbinden mit einem SQL-Datenbankserver: SQL Server Management Studio](./media/sql-database-connect-query/tools.png)
5. Klicken Sie auf **In Visual Studio öffnen** (klicken Sie auf den Downloadlink, wenn Sie Visual Studio benötigen):
   
    ![Neue Abfrage Verbinden mit einem SQL-Datenbankserver: SQL Server Management Studio](./media/sql-database-connect-query/open-in-vs.png)
6. Visual Studio wird geöffnet, und im Fenster **Mit Server verbinden** sind bereits die Angaben für die Verbindung mit dem Server und der Datenbank gemäß Ihrer Auswahl im Portal vorhanden.  (Klicken Sie auf **Optionen**, um sicherzustellen, dass die Verbindung auf die richtige Datenbank festgelegt ist.) Geben Sie Ihr Serveradministratorkennwort ein, und klicken Sie auf **Verbinden**.

    ![Neue Abfrage Verbinden mit einem SQL-Datenbankserver: SQL Server Management Studio](./media/sql-database-connect-query/connect.png)


1. Wenn Sie für die IP-Adresse Ihres Computers keine Firewallregel eingerichtet haben, erhalten Sie hier eine Meldung der Art *Keine Verbindung möglich* . Informationen zur Erstellung einer Firewall finden Sie unter [Konfigurieren einer Firewallregel auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md).
2. Nachdem die Verbindung hergestellt wurde, wird das Fenster **SQL Server-Objekt-Explorer** mit einer Verbindung mit Ihrer Datenbank geöffnet.
   
    ![Neue Abfrage Verbinden mit einem SQL-Datenbankserver: SQL Server Management Studio](./media/sql-database-connect-query/sql-server-object-explorer.png)

## <a name="run-a-sample-query"></a>Ausführen einer Beispielabfrage
Nachdem wir die Verbindung mit der Datenbank hergestellt haben, wird anhand der folgenden Schritte veranschaulicht, wie Sie eine einfache Abfrage ausführen:

1. Klicken Sie mit der rechten Maustaste auf die Datenbank, und wählen Sie dann **Neue Abfrage**aus.
   
    ![Neue Abfrage Verbinden mit einem SQL-Datenbankserver: SQL Server Management Studio](./media/sql-database-connect-query/new-query.png)
2. Kopieren Sie den folgenden Code, und fügen Sie ihn im Abfragefenster ein.
   
        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;
3. Klicken Sie auf die Schaltfläche **Ausführen** , um die Abfrage auszuführen:
   
    ![Erfolgreich. Verbinden mit einem SQL-Datenbankserver: Visual Studio](./media/sql-database-connect-query/run-query.png)

## <a name="next-steps"></a>Nächste Schritte
* Beim Öffnen von SQL-Datenbanken in Visual Studio werden SQL Server Data Tools verwendet. Weitere Informationen finden Sie unter [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686.aspx).
* Informationen zum Herstellen einer Verbindung mit einer SQL-Datenbank finden Sie unter [Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von .NET (C#)](sql-database-develop-dotnet-simple.md).


