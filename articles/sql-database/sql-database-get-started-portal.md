---
title: 'Azure-Portal: Erstellen einer SQL-Datenbank | Microsoft-Dokumentation'
description: "Erfahren Sie, wie Sie einen logischen SQL-Datenbankserver, eine Firewallregel auf Serverebene und Datenbanken mit dem Azure-Portal erstellen. Außerdem wird beschrieben, wie Sie eine Azure SQL-Datenbank mit dem Azure-Portal abfragen."
keywords: Tutorial zu SQL-Datenbank, Erstellen einer SQL-Datenbank
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: quick start create
ms.workload: data-management
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: hero-article
ms.date: 04/03/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 58af25d90b419b3ddb986118a8c9ba3b42aa95a6
ms.lasthandoff: 04/12/2017


---
# <a name="create-an-azure-sql-database-in-the-azure-portal"></a>Erstellen einer Azure SQL-Datenbank im Azure-Portal

In diesem Schnellstart-Tutorial wird schrittweise beschrieben, wie Sie in Azure eine SQL-Datenbank erstellen. Azure SQL-Datenbank ist ein „Database as a Service“-Angebot, bei dem Sie hoch verfügbare SQL Server-Datenbanken in der Cloud ausführen und skalieren können. In diesem Schnellstart wird gezeigt, wie Sie die ersten Schritte ausführen, indem Sie mit dem Azure-Portal eine SQL-Datenbank erstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="create-a-sql-database"></a>Erstellen einer SQL-Datenbank

Eine Azure SQL-Datenbank wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](sql-database-service-tiers.md) erstellt. Die Datenbank wird in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) und auf einem [logischen Azure SQL-Datenbankserver](sql-database-features.md) erstellt. 

Führen Sie diese Schritte aus, um eine SQL-Datenbank mit den Adventure Works LT-Beispieldaten zu erstellen. 

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann auf der Seite **Datenbanken** die Option **SQL-Datenbank**.

    ![Datenbankerstellung 1](./media/sql-database-get-started-portal/create-database-1.png)

3. Geben Sie die folgenden Informationen in das SQL-Datenbank-Formular ein, wie in der obigen Abbildung dargestellt:     
   - Datenbankname: **mySampleDatabase**
   - Ressourcengruppe: **myResourceGroup**
   - Quelle: **Sample (AdventureWorksLT)**

4. Klicken Sie auf **Server**, um den Server für Ihre neue Datenbank zu erstellen und zu konfigurieren. Füllen Sie das Formular **Neuer Server** aus, und geben Sie einen global eindeutigen Servernamen, einen Namen für die Serveradministrator-Anmeldung und dann das Kennwort Ihrer Wahl an. 

    ![Erstellung des Datenbankservers](./media/sql-database-get-started-portal/create-database-server.png)
5. Klicken Sie auf **Auswählen**.

6. Klicken Sie auf **Tarif**, um die Dienstebene und die Leistungsstufe für die neue Datenbank anzugeben. Wählen Sie für diesen Schnellstart **20 DTUs** und **250** GB Speicher.

    ![Datenbankerstellung s1](./media/sql-database-get-started-portal/create-database-s1.png)

7. Klicken Sie auf **Übernehmen**.  

8. Klicken Sie auf **Erstellen**, um die Datenbank bereitzustellen. Die Bereitstellung dauert einige Minuten. 

9. Klicken Sie in der Symbolleiste auf **Benachrichtigungen**, um den Bereitstellungsprozess zu überwachen.

    ![Benachrichtigung](./media/sql-database-get-started-portal/notification.png)


## <a name="create-a-server-level-firewall-rule"></a>Erstellen einer Firewallregel auf Serverebene

Der SQL-Datenbankdienst erstellt eine Firewall auf Serverebene, um zu verhindern, dass externe Anwendungen und Tools eine Verbindung mit dem Server oder Datenbanken auf dem Server herstellen – sofern keine Firewallregel erstellt wird, um die Firewall für bestimmte IP-Adressen zu öffnen. Führen Sie die hier angegebenen Schritte zum Erstellen einer [SQL-Datenbank-Firewallregel auf Serverebene](sql-database-firewall-configure.md) für die IP-Adresse Ihres Clients und Zulassen der externen Konnektivität durch die SQL-Datenbankfirewall nur für Ihre IP-Adresse aus. 

1. Klicken Sie nach Abschluss der Bereitstellung im Menü auf der linken Seite auf **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. Die Übersichtsseite für Ihre Datenbank wird geöffnet, die den vollqualifizierten Servernamen (z.B. **mynewserver20170327.database.windows.net**) und Optionen für die weitere Konfiguration enthält.

      ![Serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png) 

2. Klicken Sie auf der Symbolleiste wie in der obigen Abbildung dargestellt auf **Serverfirewall festlegen**. Die Seite **Firewalleinstellungen** für den SQL-Datenbankserver wird geöffnet. 

3. Klicken Sie in der Symbolleiste auf **Client-IP-Adresse hinzufügen** und dann auf **Speichern**. Eine Firewallregel auf Serverebene wird für Ihre aktuelle IP-Adresse erstellt.

      ![Festlegen der Serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

4. Klicken Sie auf **OK**, und schließen Sie anschließend die Seite **Firewalleinstellungen**.

Sie können jetzt eine Verbindung mit dem SQL-Datenbankserver und den dazugehörigen Datenbanken herstellen. Verwenden Sie hierfür SQL Server Management Studio oder ein anderes Tool Ihrer Wahl sowie das zuvor erstellte Serveradministratorkonto.

> [!NOTE]
> SQL-Datenbank kommuniziert über Port 1433. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihre IT-Abteilung Port 1433 öffnet.
>

## <a name="query-the-sql-database"></a>Abfragen der SQL-Datenbank

Nach dem Erstellen der SQL-Datenbank haben wir sie mit den Daten der **AdventureWorksLT**-Beispieldatenbank gefüllt (über eine der Optionen, die wir während der Erstellung weiter oben in diesem Schnellstart gewählt haben). Nun verwenden wir das integrierte Abfragetool im Azure-Portal, um die Daten abzufragen. 

1. Klicken Sie auf der Seite „SQL-Datenbank“ für Ihre Datenbank auf der Symbolleiste auf **Tools**. Die Seite **Tools** wird geöffnet.

     ![Menü „Tools“](./media/sql-database-get-started-portal/tools-menu.png) 

2. Klicken Sie auf **Abfrage-Editor (Vorschau)**, auf das Kontrollkästchen **Preview-Bedingungen** und dann auf **OK**. Die Seite mit dem Abfrage-Editor wird geöffnet.

3. Klicken Sie auf **Anmelden**, und wählen Sie die Option **SQL Server-Authentifizierung**, wenn die Aufforderung angezeigt wird. Geben Sie anschließend den Benutzernamen und das Kennwort des Serveradministrators an.

    ![Anmeldung](./media/sql-database-get-started-portal/login.png) 

4. Klicken Sie auf **OK**, um sich anzumelden.

5. Geben Sie nach der Authentifizierung im Bereich für den Abfrage-Editor die folgende Abfrage ein:

   ```
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

6. Klicken Sie auf **Ausführen**, und sehen Sie sich die Abfrageergebnisse im Bereich **Ergebnisse** an.

    ![Abfrage-Editor – Ergebnisse](./media/sql-database-get-started-portal/query-editor-results.png)

7. Schließen Sie die Seite **Abfrage-Editor** und die Seite **Tools**.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstarts dieser Sammlung bauen auf diesem Schnellstart auf. Wenn Sie planen, mit den nachfolgenden Schnellstarts fortzufahren, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte ausführen, um alle erstellten Ressourcen dieses Schnellstarts im Azure-Portal zu löschen.

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen** und dann auf **myResourceGroup**. 
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld **myResourceGroup** ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Herstellen einer Verbindung und Durchführen von Abfragen mit SQL Server Management Studio finden Sie unter [Verbinden und Abfragen mit SSMS](sql-database-connect-query-ssms.md).
- Informationen zum Herstellen einer Verbindung mit Visual Studio finden Sie unter [Verbinden und Abfragen mit Visual Studio Code](sql-database-connect-query-vscode.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit .NET finden Sie unter [Verbinden und Abfragen mit .NET](sql-database-connect-query-dotnet.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit PHP finden Sie unter [Verbinden und Abfragen mit PHP](sql-database-connect-query-php.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Node.js finden Sie unter [Verbinden und Abfragen mit Node.js](sql-database-connect-query-nodejs.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Java finden Sie unter [Verbinden und Abfragen mit Java](sql-database-connect-query-java.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Python finden Sie unter [Verbinden und Abfragen mit Python](sql-database-connect-query-python.md).
- Informationen zum Herstellen einer Verbindung und Senden von Abfragen mit Ruby finden Sie unter [Verbinden und Abfragen mit Ruby](sql-database-connect-query-ruby.md).

