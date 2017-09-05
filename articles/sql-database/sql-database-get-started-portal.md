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
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: quickstart
ms.date: 08/25/2017
ms.author: carlrab
ms.translationtype: HT
ms.sourcegitcommit: 48dfc0fa4c9ad28c4c64c96ae2fc8a16cd63865c
ms.openlocfilehash: 6bf1b94b38fffe31dd67a5146c3c9c5e9367084e
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

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

2. Wählen Sie auf der Seite **Neu** die Option **Datenbanken**, und wählen Sie dann die Option **Erstellen** unter **SQL-Datenbank** auf der Seite **Neu**.

   ![Datenbankerstellung 1](./media/sql-database-get-started-portal/create-database-1.png)

3. Geben Sie die folgenden Informationen in das SQL-Datenbank-Formular ein, wie in der obigen Abbildung dargestellt:   

   | Einstellung       | Empfohlener Wert | Beschreibung | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Datenbankname** | mySampleDatabase | Gültige Datenbanknamen finden Sie unter [Database Identifiers](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers) (Datenbankbezeichner). | 
   | **Abonnement** | Ihr Abonnement  | Ausführliche Informationen zu Ihren Abonnements finden Sie unter [Abonnements](https://account.windowsazure.com/Subscriptions). |
   | **Ressourcengruppe**  | myResourceGroup | Gültige Ressourcengruppennamen finden Sie unter [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). |
   | **Quelle auswählen** | Sample (AdventureWorksLT) | Lädt das AdventureWorksLT-Schema und die Daten in Ihre neue Datenbank. |

   > [!IMPORTANT]
   > Sie müssen die Beispieldatenbank in diesem Formular auswählen, da sie im restlichen Teil dieses Schnellstarts verwendet wird.
   > 

4. Klicken Sie unter **Server** auf **Erforderliche Einstellungen konfigurieren**, und geben Sie im Formular für den (logischen) SQL-Server die folgenden Informationen an, wie in der folgenden Abbildung zu sehen:   

   | Einstellung       | Empfohlener Wert | Beschreibung | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servername** | Ein global eindeutiger Name | Gültige Servernamen finden Sie unter [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). | 
   | **Serveradministratoranmeldung** | Ein gültiger Name | Gültige Anmeldenamen finden Sie unter [Database Identifiers](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers) (Datenbankbezeichner). |
   | **Kennwort** | Ein gültiges Kennwort | Ihr Kennwort muss mindestens acht Zeichen umfassen und Zeichen aus drei der folgenden Kategorien enthalten: Großbuchstaben, Kleinbuchstaben, Zahlen und nicht alphanumerische Zeichen. |
   | **Abonnement** | Ihr Abonnement | Ausführliche Informationen zu Ihren Abonnements finden Sie unter [Abonnements](https://account.windowsazure.com/Subscriptions). |
   | **Ressourcengruppe** | myResourceGroup | Gültige Ressourcengruppennamen finden Sie unter [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). |
   | **Location** | Gültiger Standort | Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/). |

   > [!IMPORTANT]
   > Der hier angegebene Benutzername bzw. das Kennwort für den Serveradministrator sind erforderlich, um später in diesem Schnellstart die Anmeldung am Server und an den dazugehörigen Datenbanken durchzuführen. Behalten Sie diese Angaben im Kopf, oder notieren Sie sie zur späteren Verwendung. 
   >  

   ![Erstellung des Datenbankservers](./media/sql-database-get-started-portal/create-database-server.png)

5. Klicken Sie nach dem Ausfüllen des Formulars auf **Auswählen**.

6. Klicken Sie auf **Tarif**, um die Dienstebene, die Anzahl von DTUs und die Menge an Speicher anzugeben. Entdecken Sie die Optionen für die Menge an DTUs und Speicher, die für jede Dienstebene verfügbar ist. 

   > [!IMPORTANT]
   > \* Speichergrößen, die den integrierten Speicher überschreiten, befinden sich in der Vorschauphase und werden gegen Aufpreis bereitgestellt. Weitere Informationen finden Sie unter [SQL-Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/). 
   >
   >\* Im Premium-Tarif ist eine Speicherkapazität von mehr als 1 TB folgenden Regionen verfügbar: „USA, Osten 2“, „USA, Westen“, „USA Gov Virginia“, „Europa, Westen“, „Deutschland, Mitte“, „Asien, Südosten“, „Japan, Osten“, „Australien, Osten“, „Kanada, Mitte“ und „Kanada, Osten“. Siehe [P11-P15: Aktuelle Einschränkungen](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
   > 

7. Wählen Sie in diesem Schnellstarttutorial die Dienstebene **Standard**, und wählen Sie dann mit dem Schieberegler **100 DTUs (S3)** und **400** GB Speicher aus.

   ![Datenbankerstellung s1](./media/sql-database-get-started-portal/create-database-s1.png)

8. Akzeptieren Sie die Nutzungsbedingungen für die Vorschau, um die Option **Add-On-Speicher** zu verwenden. 

   > [!IMPORTANT]
   > \* Speichergrößen, die den integrierten Speicher überschreiten, befinden sich in der Vorschauphase und werden gegen Aufpreis bereitgestellt. Weitere Informationen finden Sie unter [SQL-Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/). 
   >
   >\* Im Premium-Tarif ist eine Speicherkapazität von mehr als 1 TB folgenden Regionen verfügbar: „USA, Osten 2“, „USA, Westen“, „USA Gov Virginia“, „Europa, Westen“, „Deutschland, Mitte“, „Asien, Südosten“, „Japan, Osten“, „Australien, Osten“, „Kanada, Mitte“ und „Kanada, Osten“. Siehe [P11-P15: Aktuelle Einschränkungen](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
   > 

9. Klicken Sie auf **Übernehmen**, wenn Sie die Server-Ebene, die Anzahl von DTUs und die Menge an Speicherplatz ausgewählt haben.  

10. Nachdem Sie das SQL-Datenbank-Formular ausgefüllt haben, können Sie auf **Erstellen** klicken, um die Datenbank bereitzustellen. Die Bereitstellung dauert einige Minuten. 

11. Klicken Sie in der Symbolleiste auf **Benachrichtigungen**, um den Bereitstellungsprozess zu überwachen.
    
     ![Benachrichtigung](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Erstellen einer Firewallregel auf Serverebene

Der SQL-Datenbankdienst erstellt eine Firewall auf Serverebene, um zu verhindern, dass externe Anwendungen und Tools eine Verbindung mit dem Server oder Datenbanken auf dem Server herstellen – sofern keine Firewallregel erstellt wird, um die Firewall für bestimmte IP-Adressen zu öffnen. Führen Sie die hier angegebenen Schritte zum Erstellen einer [SQL-Datenbank-Firewallregel auf Serverebene](sql-database-firewall-configure.md) für die IP-Adresse Ihres Clients und Zulassen der externen Konnektivität durch die SQL-Datenbankfirewall nur für Ihre IP-Adresse aus. 

> [!NOTE]
> SQL-Datenbank kommuniziert über Port 1433. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihre IT-Abteilung Port 1433 öffnet.
>

1. Klicken Sie nach Abschluss der Bereitstellung im Menü auf der linken Seite auf **SQL-Datenbanken**, und klicken Sie dann auf der Seite **SQL-Datenbanken** auf **mySampleDatabase**. Die Übersichtsseite für Ihre Datenbank wird geöffnet, die den vollqualifizierten Servernamen (z.B. **mynewserver20170824.database.windows.net**) und Optionen für die weitere Konfiguration enthält. 

2. Kopieren Sie diesen vollqualifizierten Servernamen, um in den nachfolgenden Schnellstarts eine Verbindung mit Ihrem Server und den Datenbanken herzustellen. 

   ![Servername](./media/sql-database-get-started-portal/server-name.png) 

3. Klicken Sie auf der Symbolleiste wie in der obigen Abbildung dargestellt auf **Serverfirewall festlegen**. Die Seite **Firewalleinstellungen** für den SQL-Datenbankserver wird geöffnet. 

   ![Serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png) 

4. Klicken Sie in der Symbolleiste auf **Client-IP-Adresse hinzufügen**, um Ihre aktuelle IP-Adresse einer neuen Firewallregel hinzuzufügen. Eine Firewallregel kann Port 1433 für eine einzelne IP-Adresse oder einen Bereich von IP-Adressen öffnen.

5. Klicken Sie auf **Speichern**. Für Ihre aktuelle IP-Adresse wird eine Firewallregel auf Serverebene erstellt, und auf dem logischen Server wird Port 1433 geöffnet.

6. Klicken Sie auf **OK**, und schließen Sie anschließend die Seite **Firewalleinstellungen**.

Sie können für diese IP-Adresse jetzt eine Verbindung mit dem SQL-Datenbankserver und den dazugehörigen Datenbanken herstellen. Verwenden Sie hierfür SQL Server Management Studio oder ein anderes Tool Ihrer Wahl sowie das zuvor erstellte Serveradministratorkonto.

> [!IMPORTANT]
> Standardmäßig ist der Zugriff über die SQL-Datenbank-Firewall für alle Azure-Dienste aktiviert. Klicken Sie auf dieser Seite auf **AUS**, um dies für alle Azure-Dienste zu deaktivieren.
>

## <a name="query-the-sql-database"></a>Abfragen der SQL-Datenbank

Nachdem Sie nun eine Beispieldatenbank in Azure erstellt haben, können Sie das integrierte Abfragetool im Azure-Portal verwenden, um sich zu vergewissern, dass Sie eine Verbindung mit der Datenbank herstellen und die Daten abfragen können. 

1. Klicken Sie auf der Seite „SQL-Datenbank“ für Ihre Datenbank in der Symbolleiste auf **Tools**, und klicken Sie dann auf **Abfrage-Editor (Vorschau)**.

   ![Menü „Tools“](./media/sql-database-get-started-portal/tools-menu.png) 

2. Aktivieren Sie auf das Kontrollkästchen mit den **Nutzungsbedingungen für die Vorschau**, und klicken Sie dann auf **OK**. Die Seite mit dem Abfrage-Editor wird geöffnet.

3. Klicken Sie auf **Anmelden**, überprüfen Sie die Anmeldeinformationen, und klicken Sie dann auf **OK**, um sich über die SQL Server-Authentifizierung mit den zuvor erstellten Serveradministrator-Anmeldedaten anzumelden.

   ![Anmeldung](./media/sql-database-get-started-portal/login.png) 

4. Klicken Sie auf **OK**, um sich anzumelden.

5. Geben Sie nach der Authentifizierung als **Serveradministrator** im Bereich für den Abfrage-Editor die folgende Abfrage ein:

   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

6. Klicken Sie auf **Ausführen**, und sehen Sie sich die Abfrageergebnisse im Bereich **Ergebnisse** an.

   ![Abfrage-Editor – Ergebnisse](./media/sql-database-get-started-portal/query-editor-results.png)

7. Schließen Sie den **Abfrage-Editor**, klicken Sie auf **OK**, um nicht gespeicherte Änderungen zu verwerfen, und schließen Sie dann die Seite **Tools**.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Speichern Sie diese Ressourcen, um unter [Nächste Schritte](#next-steps) zu erfahren, wie Sie eine Verbindung herstellen und Ihre Datenbank mit verschiedenen Methoden abfragen. Wenn Sie die in diesem Schnellstart erstellten Ressourcen löschen möchten, wenden Sie die folgenden Schritte an. 


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

