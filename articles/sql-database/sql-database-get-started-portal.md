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
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 1c087f3ecbdd6956b50a8ef9b7e81340f83ff0cf
ms.lasthandoff: 03/21/2017


---
# <a name="create-and-query-a-single-azure-sql-database-in-the-azure-portal"></a>Erstellen und Abfragen einer einzelnen Azure SQL-Datenbank im Azure-Portal

Azure SQL-Datenbanken können über das Azure-Portal erstellt werden. Diese Methode bietet eine browserbasierte Benutzeroberfläche zum Erstellen und Konfigurieren von Azure SQL-Datenbanken und alle zugehörigen Azure-Ressourcen.

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="create-a-sql-database"></a>Erstellen einer SQL-Datenbank

Eine Azure SQL-Datenbank wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](sql-database-service-tiers.md) erstellt. Die Datenbank wird in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) und auf einem [logischen Azure SQL-Datenbankserver](sql-database-features.md) erstellt. 

Führen Sie diese Schritte aus, um eine SQL-Datenbank mit den Adventure Works LT-Beispieldaten zu erstellen. 

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann auf der Seite **Datenbanken** die Option **SQL-Datenbank**.

3. Geben Sie die erforderlichen Informationen in das Formular für die SQL-Datenbank ein: 
   - Datenbankname: Geben Sie einen Datenbanknamen an.
   - Abonnement: Wählen Sie Ihr Abonnement aus.
   - Ressourcengruppe: Wählen Sie eine neue oder vorhandene Ressourcengruppe aus.
   - Quelle: Wählen Sie **Beispiel (AdventureWorksLT)**.
   - Server: Erstellen Sie einen neuen Server (der Name für **Server** muss global eindeutig sein).
   - Elastischer Pool: Wählen Sie für diesen Schnellstart die Option **Nicht jetzt**.
   - Tarif: Wählen Sie **20 DTUs** und **250** GB Speicher.
   - Sortierung: Sie können diesen Wert beim Importieren der Beispieldatenbank nicht ändern. 
   - An Dashboard anheften: Aktivieren Sie dieses Kontrollkästchen.

      ![Datenbank erstellen](./media/sql-database-get-started/create-database-s1.png)

4. Klicken Sie auf **Erstellen**, wenn Sie alles eingegeben haben. Die Bereitstellung dauert einige Minuten.
5. Nachdem die Bereitstellung der SQL-Datenbank beendet wurde, können Sie im Dashboard die Option **SQL-Datenbanken** wählen oder im Menü auf der linken Seite auf **SQL-Datenbanken** klicken und dann auf der Seite **SQL-Datenbanken** auf die neue Datenbank klicken. Eine Übersichtsseite für Ihre Datenbank wird geöffnet, die den vollqualifizierten Servernamen (z.B. **mynewserver20170313.database.windows.net**) und Optionen für die weitere Konfiguration enthält.

      ![Neue SQL-Datenbank](./media/sql-database-get-started/new-database-s1-overview.png) 

## <a name="create-a-server-level-firewall-rule"></a>Erstellen einer Firewallregel auf Serverebene

Mit dem SQL-Datenbankdienst wird eine Firewall erstellt, die verhindert, dass externe Anwendungen und Tools eine Verbindung mit Ihrem Server und der Datenbank herstellen. Führen Sie die hier angegebenen Schritte zum Erstellen einer [SQL-Datenbank-Firewallregel auf Serverebene](sql-database-firewall-configure.md) für Ihre IP-Adresse aus, um die externe Verbindung durch die Firewall der SQL-Datenbank zuzulassen. 

1. Klicken Sie in der Symbolleiste für Ihre Datenbank auf **Set server firewall** (Serverfirewall festlegen). Die Seite **Firewalleinstellungen** für den SQL-Datenbankserver wird geöffnet. 

      ![Serverfirewallregel](./media/sql-database-get-started/server-firewall-rule.png) 

2. Klicken Sie in der Symbolleiste auf **Client-IP-Adresse hinzufügen** und dann auf **Speichern**. Eine Firewallregel auf Serverebene wird für Ihre aktuelle IP-Adresse erstellt.

3. Klicken Sie auf **OK** und dann auf das **X**, um die Seite mit den Firewalleinstellungen zu schließen.

Nun können Sie die Verbindung mit der Datenbank und dem zugehörigen Server herstellen, indem Sie SQL Server Management Studio oder ein anderes Tool Ihrer Wahl verwenden.

## <a name="query-the-sql-database"></a>Abfragen der SQL-Datenbank

Führen Sie die hier angegebenen Schritte aus, um die Datenbank mit dem Abfrage-Editor im Azure-Portal abzufragen. 

1. Klicken Sie auf der Seite „SQL-Datenbank“ für Ihre Datenbank auf der Symbolleiste auf **Tools**. Die Vorschauseite für den **Abfrage-Editor** wird geöffnet.

     ![Menü „Tools“](./media/sql-database-get-started/tools-menu.png) 

2. Klicken Sie auf **Abfrage-Editor (Vorschau)**, auf das Kontrollkästchen **Preview-Bedingungen** und dann auf **OK**. Der Abfrage-Editor wird geöffnet.

3. Klicken Sie auf **Anmelden**, und wählen Sie die Option **SQL Server-Authentifizierung**, wenn die Aufforderung angezeigt wird. Geben Sie anschließend den Benutzernamen und das Kennwort des Serveradministrators an.
4. Klicken Sie auf **OK**, um sich anzumelden.

5. Geben Sie nach Abschluss der Authentifizierung im Abfragefenster eine Abfrage Ihrer Wahl ein, z.B.:

   ```
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

6. Klicken Sie auf **Ausführen**, und sehen Sie sich die Abfrageergebnisse im Bereich **Ergebnisse** an.

    ![Abfrage-Editor – Ergebnisse](./media/sql-database-get-started/query-editor-results.png)

7. Klicken Sie auf das **X**, um die Seite mit dem Abfrage-Editor zu schließen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Die **Verbinden mit**-Schnellstarts in dieser Sammlung und die Tutorials der Tutorialsammlung bauen auf diesem Schnellstart auf. Wenn Sie planen, mit den nachfolgenden Schnellstarts oder Tutorials fortzufahren, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie die folgenden Schritte ausführen, um alle erstellten Ressourcen dieses Schnellstarts im Azure-Portal zu löschen.

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen** und dann auf **myResourceGroup**. 
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld **myResourceGroup** ein, und klicken Sie dann auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Herstellen einer Verbindung und Durchführen von Abfragen mit SQL Server Management Studio finden Sie unter [Verbinden und Abfragen mit SSMS](sql-database-connect-query-ssms.md).
- Informationen zum Herstellen einer Verbindung mit Visual Studio finden Sie unter [Verbinden und Abfragen mit Visual Studio](sql-database-connect-query.md).
- Eine technische Übersicht über SQL-Datenbank finden Sie unter [Was ist SQL Datenbank? Einführung in SQL-Datenbank](sql-database-technical-overview.md).

