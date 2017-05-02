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
ms.date: 04/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: 90761317944b935b76df65d25a2035bfabe71c09
ms.lasthandoff: 04/22/2017


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
   - Quelle: **Beispiel (AdventureWorksLT)**

   > [!IMPORTANT]
   > Sie müssen die Beispieldatenbank in diesem Formular auswählen, da sie im restlichen Teil dieses Schnellstarts verwendet wird.
   > 

4. Klicken Sie auf **Server**, und füllen Sie anschließend das Formular **Neuer Server** aus. Geben Sie einen global eindeutigen Servernamen, einen Namen für die Serveradministrator-Anmeldung und dann das Kennwort Ihrer Wahl an. 

   > [!IMPORTANT]
   > Der hier angegebene Benutzername bzw. das Kennwort für den Serveradministrator sind erforderlich, um später in diesem Schnellstart die Anmeldung am Server und an den dazugehörigen Datenbanken durchzuführen. Behalten Sie diese Angaben im Kopf, oder notieren Sie sie zur späteren Verwendung. 
   >  

    ![Erstellung des Datenbankservers](./media/sql-database-get-started-portal/create-database-server.png)
5. Klicken Sie nach dem Ausfüllen des Formulars auf **Auswählen**.

6. Klicken Sie auf **Tarif**, um die Dienstebene und die Leistungsstufe für die neue Datenbank anzugeben. Wählen Sie mit dem Schieberegler **20 DTUs** und **250** GB Speicher aus. Weitere Informationen zu DTUs finden Sie unter [Was ist eine DTU?](sql-database-what-is-a-dtu.md).

    ![Datenbankerstellung s1](./media/sql-database-get-started-portal/create-database-s1.png)

7. Klicken Sie nach dem Auswählen der DTU-Menge auf **Übernehmen**.  

8. Nachdem Sie das SQL-Datenbank-Formular ausgefüllt haben, können Sie auf **Erstellen** klicken, um die Datenbank bereitzustellen. Die Bereitstellung dauert einige Minuten. 

9. Klicken Sie in der Symbolleiste auf **Benachrichtigungen**, um den Bereitstellungsprozess zu überwachen.

    ![Benachrichtigung](./media/sql-database-get-started-portal/notification.png)


## <a name="create-a-server-level-firewall-rule"></a>Erstellen einer Firewallregel auf Serverebene

Der SQL-Datenbankdienst erstellt eine Firewall auf Serverebene, um zu verhindern, dass externe Anwendungen und Tools eine Verbindung mit dem Server oder Datenbanken auf dem Server herstellen – sofern keine Firewallregel erstellt wird, um die Firewall für bestimmte IP-Adressen zu öffnen. Führen Sie die hier angegebenen Schritte zum Erstellen einer [SQL-Datenbank-Firewallregel auf Serverebene](sql-database-firewall-configure.md) für die IP-Adresse Ihres Clients und Zulassen der externen Konnektivität durch die SQL-Datenbankfirewall nur für Ihre IP-Adresse aus. 

> [!NOTE]
> SQL-Datenbank kommuniziert über Port 1433. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihre IT-Abteilung Port 1433 öffnet.
>

1. Klicken Sie nach Abschluss der Bereitstellung im Menü auf der linken Seite auf **SQL-Datenbanken**, und klicken Sie auf der Seite „SQL-Datenbanken“ dann auf **mySampleDatabase**. Die Übersichtsseite für Ihre Datenbank wird geöffnet, die den vollqualifizierten Servernamen (z.B. **mynewserver20170411.database.windows.net**) und Optionen für die weitere Konfiguration enthält.

   > [!IMPORTANT]
   > Sie benötigen diesen vollqualifizierten Servernamen, um in den nachfolgenden Schnellstarts eine Verbindung mit Ihrem Server und den Datenbanken herzustellen.
   > 

      ![Servername](./media/sql-database-get-started-portal/server-name.png) 

2. Klicken Sie auf der Symbolleiste wie in der obigen Abbildung dargestellt auf **Serverfirewall festlegen**. Die Seite **Firewalleinstellungen** für den SQL-Datenbankserver wird geöffnet. 

      ![Serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png) 


3. Klicken Sie in der Symbolleiste auf **Client-IP-Adresse hinzufügen**, um Ihre aktuelle IP-Adresse einer neuen Firewallregel hinzuzufügen. Eine Firewallregel kann Port 1433 für eine einzelne IP-Adresse oder einen Bereich von IP-Adressen öffnen.

4. Klicken Sie auf **Speichern**. Für Ihre aktuelle IP-Adresse wird eine Firewallregel auf Serverebene erstellt, und auf dem logischen Server wird Port 1433 geöffnet.

      ![Festlegen der Serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

4. Klicken Sie auf **OK**, und schließen Sie anschließend die Seite **Firewalleinstellungen**.

Sie können für diese IP-Adresse jetzt eine Verbindung mit dem SQL-Datenbankserver und den dazugehörigen Datenbanken herstellen. Verwenden Sie hierfür SQL Server Management Studio oder ein anderes Tool Ihrer Wahl sowie das zuvor erstellte Serveradministratorkonto.

> [!IMPORTANT]
> Standardmäßig ist der Zugriff über die SQL-Datenbank-Firewall für alle Azure-Dienste aktiviert. Klicken Sie auf dieser Seite auf **AUS**, um dies für alle Azure-Dienste zu deaktivieren.

## <a name="query-the-sql-database"></a>Abfragen der SQL-Datenbank

Nachdem Sie nun eine Beispieldatenbank in Azure erstellt haben, können Sie das integrierte Abfragetool im Azure-Portal verwenden, um sich zu vergewissern, dass Sie eine Verbindung mit der Datenbank herstellen und die Daten abfragen können. 

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

Andere Schnellstarts dieser Sammlung bauen auf diesem Schnellstart auf. 

> [!TIP]
> Wenn Sie planen, mit den nachfolgenden Schnellstarts fortzufahren, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte ausführen, um alle erstellten Ressourcen dieses Schnellstarts im Azure-Portal zu löschen.
>

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen** und dann auf **myResourceGroup**. 
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld **myResourceGroup** ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

Sie besitzen nun eine Datenbank, können eine Verbindung herstellen und Abfragen mit Ihren bevorzugten Tools ausführen. Weitere Informationen erhalten Sie, indem Sie eines der folgenden Tools auswählen:

- [SQL Server Management Studio](sql-database-connect-query-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [.NET](sql-database-connect-query-dotnet.md)
- [PHP](sql-database-connect-query-php.md)
- [Node.js](sql-database-connect-query-nodejs.md)
- [Java](sql-database-connect-query-java.md)
- [Python](sql-database-connect-query-python.md)
- [Ruby](sql-database-connect-query-ruby.md)

