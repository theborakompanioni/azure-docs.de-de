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
ms.custom: quick start
ms.workload: data-management
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: hero-article
ms.date: 03/13/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: be5839e04fae457b889db11dffe56f31afe723a5
ms.lasthandoff: 03/28/2017


---
# <a name="create-an-azure-sql-database-in-the-azure-portal"></a>Erstellen einer Azure SQL-Datenbank im Azure-Portal

In diesem Schnellstart-Tutorial wird schrittweise beschrieben, wie Sie in Azure eine SQL-Datenbank erstellen.  Azure SQL-Datenbank ist ein „Database as a Service“-Angebot, bei dem Sie hoch verfügbare SQL Server-Datenbanken in der Cloud ausführen und skalieren können.  In diesem Schnellstart wird gezeigt, wie Sie die ersten Schritte ausführen, indem Sie mit dem Azure-Portal eine neue SQL-Datenbank erstellen.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="create-a-sql-database"></a>Erstellen einer SQL-Datenbank

Eine Azure SQL-Datenbank wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](sql-database-service-tiers.md) erstellt. Die Datenbank wird in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) und auf einem [logischen Azure SQL-Datenbankserver](sql-database-features.md) erstellt. 

Führen Sie diese Schritte aus, um eine SQL-Datenbank mit den Adventure Works LT-Beispieldaten zu erstellen. 

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann auf der Seite **Datenbanken** die Option **SQL-Datenbank**.

    ![Datenbankerstellung 1](./media/sql-database-get-started/create-database-1.png)

3. Geben Sie die folgenden Informationen in das SQL-Datenbank-Formular ein, wie in der obigen Abbildung dargestellt: 
   - Datenbankname: Verwenden Sie **mySampleDatabase**.
   - Ressourcengruppe: Verwenden Sie **MyResourceGroup**.
   - Quelle: Wählen Sie **Beispiel (AdventureWorksLT)**.

4. Klicken Sie auf **Server**, um einen neuen Server für Ihre neue Datenbank zu erstellen und zu konfigurieren. Füllen Sie das Formular **Neuer Server** aus, und geben Sie einen global eindeutigen Servernamen, einen Namen für die Serveradministrator-Anmeldung und dann das Kennwort Ihrer Wahl an. 

    ![Erstellung des Datenbankservers](./media/sql-database-get-started/create-database-server.png)
5. Klicken Sie auf **Auswählen**.

6. Klicken Sie auf **Tarif**, um die Dienstebene und die Leistungsstufe für die neue Datenbank anzugeben. Wählen Sie für diesen Schnellstart **20 DTUs** und **250** GB Speicher.

    ![Datenbankerstellung s1](./media/sql-database-get-started/create-database-s1.png)

7. Klicken Sie auf **Übernehmen**.  

8. Klicken Sie auf **Erstellen**, um die Datenbank bereitzustellen. Die Bereitstellung dauert einige Minuten. 

9. Klicken Sie in der Symbolleiste auf **Benachrichtigungen**, um den Bereitstellungsprozess zu überwachen.

    ![Benachrichtigung](./media/sql-database-get-started/notification.png)


## <a name="create-a-server-level-firewall-rule"></a>Erstellen einer Firewallregel auf Serverebene

Der SQL-Datenbankdienst erstellt eine Firewall auf der Serverebene, um zu verhindern, dass externe Anwendungen und Tools eine Verbindung mit dem Server oder Datenbanken auf dem Server herstellen – sofern keine Firewallregel erstellt wird, um die Firewall für bestimmte IP-Adressen zu öffnen. Führen Sie die hier angegebenen Schritte zum Erstellen einer [SQL-Datenbank-Firewallregel auf Serverebene](sql-database-firewall-configure.md) für die IP-Adresse Ihres Clients und Zulassen der externen Konnektivität durch die SQL-Datenbankfirewall nur für Ihre IP-Adresse aus. 

1. Klicken Sie nach Abschluss der Bereitstellung im Menü auf der linken Seite auf **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf die neue Datenbank **mySampleDatabase**. Die Übersichtsseite für Ihre Datenbank wird geöffnet, die den vollqualifizierten Servernamen (z.B. **mynewserver20170327.database.windows.net**) und Optionen für die weitere Konfiguration enthält.

      ![Serverfirewallregel](./media/sql-database-get-started/server-firewall-rule.png) 

2. Klicken Sie auf der Symbolleiste wie in der obigen Abbildung dargestellt auf **Serverfirewall festlegen**. Die Seite **Firewalleinstellungen** für den SQL-Datenbankserver wird geöffnet. 

3. Klicken Sie in der Symbolleiste auf **Client-IP-Adresse hinzufügen** und dann auf **Speichern**. Eine Firewallregel auf Serverebene wird für Ihre aktuelle IP-Adresse erstellt.

      ![Festlegen der Serverfirewallregel](./media/sql-database-get-started/server-firewall-rule-set.png) 

4. Klicken Sie auf **OK** und dann auf das **X**, um die Seite mit den **Firewalleinstellungen** zu schließen.

Nun können Sie die Verbindung mit der Datenbank und dem zugehörigen Server herstellen, indem Sie SQL Server Management Studio oder ein anderes Tool Ihrer Wahl verwenden.

## <a name="query-the-sql-database"></a>Abfragen der SQL-Datenbank

Nach dem Erstellen der SQL-Datenbank haben wir sie mit den Daten der **AdventureWorksLT**-Beispieldatenbank gefüllt (über eine der Optionen, die wir während der Erstellung weiter oben in diesem Schnellstart gewählt haben). Nun verwenden wir das integrierte Abfragetool im Azure-Portal, um die Daten abzufragen. 

1. Klicken Sie auf der Seite „SQL-Datenbank“ für Ihre Datenbank auf der Symbolleiste auf **Tools**. Die Seite **Tools** wird geöffnet.

     ![Menü „Tools“](./media/sql-database-get-started/tools-menu.png) 

2. Klicken Sie auf **Abfrage-Editor (Vorschau)**, auf das Kontrollkästchen **Preview-Bedingungen** und dann auf **OK**. Die Seite mit dem Abfrage-Editor wird geöffnet.

3. Klicken Sie auf **Anmelden**, und wählen Sie die Option **SQL Server-Authentifizierung**, wenn die Aufforderung angezeigt wird. Geben Sie anschließend den Benutzernamen und das Kennwort des Serveradministrators an.

    ![Anmeldung](./media/sql-database-get-started/login.png) 

4. Klicken Sie auf **OK**, um sich anzumelden.

5. Geben Sie nach der Authentifizierung im Bereich für den Abfrage-Editor die folgende Abfrage ein:

   ```
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

6. Klicken Sie auf **Ausführen**, und sehen Sie sich die Abfrageergebnisse im Bereich **Ergebnisse** an.

    ![Abfrage-Editor – Ergebnisse](./media/sql-database-get-started/query-editor-results.png)

7. Klicken Sie auf das **X**, um die Seite mit dem **Abfrage-Editor** zu schließen, und klicken Sie erneut auf das **X**, um die Seite **Tools** zu schließen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstarts dieser Sammlung bauen auf diesem Schnellstart auf. Wenn Sie planen, mit den nachfolgenden Schnellstarts oder Tutorials fortzufahren, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte ausführen, um alle erstellten Ressourcen dieses Schnellstarts im Azure-Portal zu löschen.

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen** und dann auf **myResourceGroup**. 
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld **myResourceGroup** ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Herstellen einer Verbindung und Durchführen von Abfragen mit SQL Server Management Studio finden Sie unter [Verbinden und Abfragen mit SSMS](sql-database-connect-query-ssms.md).
- Informationen zum Herstellen einer Verbindung mit Visual Studio finden Sie unter [Verbinden und Abfragen mit Visual Studio](sql-database-connect-query.md).
- Eine technische Übersicht über SQL-Datenbank finden Sie unter [Was ist SQL Datenbank? Einführung in SQL-Datenbank](sql-database-technical-overview.md).

