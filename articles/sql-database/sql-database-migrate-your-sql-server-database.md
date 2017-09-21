---
title: Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine SQL Server-Datenbank zu Azure SQL-Datenbank migrieren.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,load & move data
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 09/01/2017
ms.author: carlrab
ms.translationtype: HT
ms.sourcegitcommit: 12c20264b14a477643a4bbc1469a8d1c0941c6e6
ms.openlocfilehash: 8206cba416df4d43594678428632ff161a9564d6
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---

# <a name="migrate-your-sql-server-database-to-azure-sql-database"></a>Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank

Das Verschieben Ihrer SQL Server-Datenbank zur Azure SQL-Datenbank ist so einfach, wie eine leere SQL­Datenbank in Azure zu erstellen und sie dann mithilfe des [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) in Azure zu importieren. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer leeren Azure SQL-Datenbank im Azure-Portal (mit einem neuen oder vorhandenen Azure SQL-Datenbankserver)
> * Erstellen einer Firewallregel auf Serverebene im Azure-Portal (falls nicht bereits erstellt)
> * Verwenden des [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) zum Importieren Ihrer SQL Server-Datenbank in die leere Azure SQL-Datenbank 
> * Verwenden von [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS), um die Datenbankeigenschaften zu ändern.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zum Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

- Die neueste Version von [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) muss installiert sein.  
- Die neueste Version des [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) muss installiert sein.
- Sie haben die zu migrierende Datenbank angegeben, und haben Zugriff auf diese. In diesem Tutorial wird die [SQL Server 2008-OLTP-Datenbank „AdventureWorks2008R2“](https://msftdbprodsamples.codeplex.com/releases/view/59211) für eine Instanz von SQL Server 2008R2 oder höher verwendet, Sie können aber jede Datenbank Ihrer Wahl verwenden. Um Kompatibilitätsprobleme zu beheben, verwenden Sie [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).

## <a name="log-in-to-the-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="create-a-blank-sql-database"></a>Erstellen einer leeren SQL-­Datenbank

Eine Azure SQL-Datenbank wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](sql-database-service-tiers.md) erstellt. Die Datenbank wird in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) und auf einem [logischen Azure SQL-Datenbankserver](sql-database-features.md) erstellt. 

Führen Sie die folgenden Schritte aus, um eine leere SQL-­Datenbank zu erstellen. 

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.

2. Wählen Sie auf der Seite **Neu** die Option **Datenbanken** und dann auf der Seite **Neu** unter **SQL-Datenbank** die Option **Erstellen** aus.

   ![Leere Datenbank erstellen](./media/sql-database-design-first-database/create-empty-database.png)

3. Geben Sie die folgenden Informationen in das SQL-Datenbank-Formular ein, wie in der obigen Abbildung dargestellt:   

   | Einstellung       | Empfohlener Wert | Beschreibung | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Datenbankname** | mySampleDatabase | Gültige Datenbanknamen finden Sie unter [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) (Datenbankbezeichner). | 
   | **Abonnement** | Ihr Abonnement  | Ausführliche Informationen zu Ihren Abonnements finden Sie unter [Abonnements](https://account.windowsazure.com/Subscriptions). |
   | **Ressourcengruppe** | myResourceGroup | Gültige Ressourcengruppennamen finden Sie unter [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). |
   | **Quelle auswählen** | Leere Datenbank | Gibt an, dass eine leere Datenbank erstellt werden soll. |

4. Klicken Sie auf **Server**, um einen neuen Server für Ihre neue Datenbank zu erstellen und zu konfigurieren. Füllen Sie das Formular **Neuer Server** mit den folgenden Informationen aus: 

   | Einstellung       | Empfohlener Wert | Beschreibung | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servername** | Ein global eindeutiger Name | Gültige Servernamen finden Sie unter [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Benennungsregeln und Einschränkungen). | 
   | **Serveradministratoranmeldung** | Ein gültiger Name | Gültige Anmeldenamen finden Sie unter [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) (Datenbankbezeichner).|
   | **Kennwort** | Ein gültiges Kennwort | Ihr Kennwort muss mindestens acht Zeichen umfassen und Zeichen aus drei der folgenden Kategorien enthalten: Großbuchstaben, Kleinbuchstaben, Zahlen und nicht alphanumerische Zeichen. |
   | **Location** | Gültiger Standort | Informationen zu Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/). |

   ![Erstellung des Datenbankservers](./media/sql-database-design-first-database/create-database-server.png)

5. Klicken Sie auf **Auswählen**.

6. Klicken Sie auf **Tarif**, um die Dienstebene, die Anzahl von DTUs und die Menge an Speicher anzugeben. Entdecken Sie die Optionen für die Anzahl von DTUs und Menge an Speicher, die für jede Dienstebene verfügbar sind. 

7. Wählen Sie in diesem Tutorial die Dienstebene **Standard** und dann mit dem Schieberegler **100 DTUs (S3)** und **400** GB Speicher aus.

   ![Datenbankerstellung s1](./media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

8. Akzeptieren Sie die Nutzungsbedingungen für die Vorschauversion, um die Option **Add-On-Speicher** zu verwenden. 

   > [!IMPORTANT]
   > \* Speichergrößen, die den integrierten Speicher überschreiten, befinden sich in der Vorschauphase und werden gegen Aufpreis bereitgestellt. Weitere Informationen finden Sie unter [SQL-Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/). 
   >
   >\* Im Premium-Tarif ist eine Speicherkapazität von mehr als 1 TB in folgenden Regionen verfügbar: „USA, Osten 2“, „USA, Westen“, „USA Gov Virginia“, „Europa, Westen“, „Deutschland, Mitte“, „Asien, Südosten“, „Japan, Osten“, „Australien, Osten“, „Kanada, Mitte“ und „Kanada, Osten“. Siehe [Aktuelle Einschränkungen für P11–P15](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
   > 

9. Klicken Sie auf **Übernehmen**, wenn Sie die Dienstebene, die Anzahl von DTUs und die Menge an Speicherplatz ausgewählt haben.  

10. Wählen Sie eine **Sortierung** für die leere Datenbank aus (verwenden Sie in diesem Tutorial den Standardwert). Weitere Informationen über Sortierungen finden Sie unter [Sortierungen](https://docs.microsoft.com/sql/t-sql/statements/collations).

11. Nachdem Sie das SQL-Datenbank-Formular ausgefüllt haben, können Sie auf **Erstellen** klicken, um die Datenbank bereitzustellen. Die Bereitstellung dauert einige Minuten. 

12. Klicken Sie in der Symbolleiste auf **Benachrichtigungen**, um den Bereitstellungsprozess zu überwachen.
    
     ![Benachrichtigung](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Erstellen einer Firewallregel auf Serverebene

Der SQL-Datenbankdienst erstellt eine Firewall auf Serverebene, um zu verhindern, dass externe Anwendungen und Tools eine Verbindung mit dem Server oder Datenbanken auf dem Server herstellen – sofern keine Firewallregel erstellt wird, um die Firewall für bestimmte IP-Adressen zu öffnen. Führen Sie die hier angegebenen Schritte zum Erstellen einer [SQL-Datenbank-Firewallregel auf Serverebene](sql-database-firewall-configure.md) für die IP-Adresse Ihres Clients und Zulassen der externen Konnektivität durch die SQL-Datenbankfirewall nur für Ihre IP-Adresse aus. 

> [!NOTE]
> SQL-Datenbank kommuniziert über Port 1433. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbankserver herstellen, wenn Ihre IT-Abteilung Port 1433 öffnet.
>

1. Klicken Sie nach Abschluss der Bereitstellung im Menü auf der linken Seite auf **SQL-Datenbanken**, und klicken Sie dann auf der Seite **SQL-Datenbanken** auf **mySampleDatabase**. Die Übersichtsseite für Ihre Datenbank wird geöffnet, die den vollqualifizierten Servernamen (z.B. **mynewserver20170824.database.windows.net**) und Optionen für die weitere Konfiguration enthält. 

2. Kopieren Sie diesen vollqualifizierten Servernamen, um in den nachfolgenden Schnellstarts eine Verbindung mit Ihrem Server und den Datenbanken herzustellen. 

   ![Servername](./media/sql-database-get-started-portal/server-name.png) 

3. Klicken Sie auf der Symbolleiste auf **Serverfirewall festlegen**. Die Seite **Firewalleinstellungen** für den SQL-Datenbankserver wird geöffnet. 

   ![Serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png) 

4. Klicken Sie in der Symbolleiste auf **Client-IP-Adresse hinzufügen**, um Ihre aktuelle IP-Adresse einer neuen Firewallregel hinzuzufügen. Eine Firewallregel kann Port 1433 für eine einzelne IP-Adresse oder einen Bereich von IP-Adressen öffnen.

5. Klicken Sie auf **Speichern**. Für Ihre aktuelle IP-Adresse wird eine Firewallregel auf Serverebene erstellt, und auf dem logischen Server wird Port 1433 geöffnet.

6. Klicken Sie auf **OK**, und schließen Sie anschließend die Seite **Firewalleinstellungen**.

Sie können für diese IP-Adresse jetzt eine Verbindung mit dem SQL-Datenbankserver und den dazugehörigen Datenbanken herstellen. Verwenden Sie hierfür SQL Server Management Studio, Data Migration Assistant oder ein anderes Tool Ihrer Wahl sowie das im vorherigen Verfahren erstellte Serveradministratorkonto.

> [!IMPORTANT]
> Standardmäßig ist der Zugriff über die SQL-Datenbank-Firewall für alle Azure-Dienste aktiviert. Klicken Sie auf dieser Seite auf **AUS**, um dies für alle Azure-Dienste zu deaktivieren.

## <a name="sql-server-connection-information"></a>SQL Server-Verbindungsinformationen

Rufen Sie den vollqualifizierten Servernamen für Ihren Azure SQL-Datenbankserver im Azure-Portal ab. Sie verwenden den vollqualifizierten Servernamen für das Herstellen der Verbindung mit Ihrem Azure SQL-Server mithilfe von Clienttools einschließlich Data Migration Assistance und SQL Server Management Studio.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie im Menü auf der linken Seite die Option **SQL-Datenbanken**, und klicken Sie auf der Seite **SQL-Datenbanken** auf Ihre Datenbank. 
3. Suchen Sie im Azure-Portal auf der Seite für Ihre Datenbank unter **Zusammenfassung** nach Ihrer Datenbank, und kopieren Sie den **Servernamen**.

   ![Verbindungsinformationen](./media/sql-database-get-started-portal/server-name.png)

## <a name="migrate-your-database"></a>Migrieren Ihrer Datenbank

Führen Sie die folgenden Schritte aus, um mit dem **[Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595)** die Bereitschaft Ihrer Datenbank für die Migration zu Azure SQL-Datenbank zu bewerten und die Migration abzuschließen.

1. Öffnen Sie den **Data Migration Assistant**. Sie können DMA auf jedem Computer ausführen, der mit der SQL Server-Instanz, die die Datenbank enthält, die Sie migrieren möchten, und dem Internet verbunden ist. Sie müssen DMA nicht auf dem Computer installieren, der die SQL Server-Instanz hostet, die Sie migrieren. Die Firewallregel, die Sie in einem vorherigen Verfahren erstellt haben, muss für den Computer gelten, auf dem Sie den Data Migration Assistant ausführen.

     ![Öffnen des Data Migration Assistant](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-open.png)

2. Klicken Sie im linken Menü auf **+ Neu**, um ein **Assessment**-Projekt zu erstellen. Geben Sie die angeforderten Werte ein, und klicken Sie dann auf **Erstellen**:

   | Einstellung      | Empfohlener Wert | Beschreibung | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Projekttyp | Migration | Wählen Sie entweder, Ihre Datenbank für die Migration zu bewerten, oder wählen Sie Bewertung und Migration als Teil des gleichen Workflows. |
   |Projektname|Migrationstutorial| Ein beschreibender Name |
   |Quellservertyp| SQL Server | Dies ist die einzige derzeit unterstützte Quelle. |
   |Zielservertyp| Azure SQL-Datenbank| Optionen: Azure SQL-Datenbank, SQL Server, SQL Server auf virtuellen Azure-Computern |
   |Migrationsumfang| Schema und Daten| Optionen: Schema und Daten, nur Schema, nur Daten |
   
   ![Neues Data Migration Assistant-Projekt](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-new-project.png)

3.  Tragen Sie auf der Seite **Quelle auswählen** die angeforderten Werte ein, und klicken Sie dann auf **Verbinden**:

    | Einstellung      | Empfohlener Wert | Beschreibung | 
    | ------------ | ------------------ | ------------------------------------------------- | 
    | Servername | Ihr Servername oder Ihre IP-Adresse | Ihr Servername oder Ihre IP-Adresse |
    | Authentifizierungsart | Ihr bevorzugter Authentifizierungstyp| Optionen: Windows-Authentifizierung, SQL Server-Authentifizierung, Integrierte Active Directory-Authentifizierung, Active Directory-Kennwortauthentifizierung |
    | Benutzername | Ihr Anmeldename | Ihre Anmeldung benötigt **SERVER STEUERN**-Berechtigungen. |
    | Kennwort| Ihr Kennwort | Ihr Kennwort |
    | Verbindungseigenschaften| Wählen Sie Ihrer Umgebung entsprechend **Verbindung verschlüsseln** und **Serverzertifikat vertrauen**. | Wählen Sie die entsprechenden Eigenschaften für die Verbindung mit Ihrem Server. |

    ![Neue Datenmigration – Quelle auswählen](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-source.png)

5. Wählen Sie eine einzelne Datenbank von Ihrem Quellserver zum Migrieren zu Azure SQL-Datenbank, und klicken Sie dann auf **Weiter**. Für dieses Tutorial ist nur eine einzelne Datenbank vorhanden.

6. Tragen Sie auf der Seite **Ziel auswählen** die angeforderten Werte ein, und klicken Sie dann auf **Verbinden**:

    | Einstellung      | Empfohlener Wert | Beschreibung | 
    | ------------ | ------------------ | ------------------------------------------------- | 
    | Servername | Ihr vollqualifizierter Azure-Datenbankservername | Ihr vollqualifizierter Azure-Datenbankservername aus dem vorherigen Verfahren |
    | Authentifizierungsart | SQL Server-Authentifizierung | Die SQL Server-Authentifizierung war beim Verfassen dieses Tutorials die einzige Option, aber Integrierte Active Directory-Authentifizierung und Active Directory-Kennwortauthentifizierung werden auch von Azure SQL-Datenbank unterstützt. |
    | Benutzername | Ihr Anmeldename | Ihre Anmeldung benötigt **DATENBANK STEUERN**-Berechtigungen für die Quelldatenbank. |
    | Kennwort| Ihr Kennwort | Ihr Kennwort |
    | Verbindungseigenschaften| Wählen Sie Ihrer Umgebung entsprechend **Verbindung verschlüsseln** und **Serverzertifikat vertrauen**. | Wählen Sie die entsprechenden Eigenschaften für die Verbindung mit Ihrem Server. |

    ![Neue Datenmigration – Ziel auswählen](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-target.png)

7. Wählen Sie die Datenbank auf dem Zielserver, die Sie in einem vorherigen Verfahren erstellt haben, und klicken Sie dann auf **Weiter**, um den Prozess der Bewertung des Quelldatenbankschemas zu starten. Für dieses Tutorial ist nur eine einzelne Datenbank vorhanden. Beachten Sie, dass der Kompatibilitätsgrad dieser Datenbank auf 140 festgelegt ist, dem standardmäßigen Kompatibilitätsgrad für alle neuen Datenbanken in Azure SQL-Datenbank.

   > [!IMPORTANT] 
   > Nach der Migration Ihrer Datenbank zu Azure SQL-Datenbank können Sie auswählen, die Datenbank aus Gründen der Abwärtskompatibilität mit einem angegebenen Kompatibilitätsgrad zu nutzen. Weitere Informationen zu den Auswirkungen und Optionen für das Ausführen einer Datenbank mit einem bestimmten Kompatibilitätsgrad finden Sie unter [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Lesen Sie auch [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql). Dort finden Sie Informationen zu weiteren Einstellungen auf Datenbankebene, die sich auf Kompatibilitätsgrade beziehen.
   >

8. Überprüfen Sie nach Abschluss der Datenbankschema-Bewertung auf der Seite **Objekte auswählen** die zur Migration ausgewählten Objekte sowie die Objekte, bei denen Probleme vorliegen. Überprüfen Sie z.b. das **dbo.uspSearchCandidateResumes**-Objekt auf **SERVERPROPERTY('LCID')**-Verhaltensänderungen und das **HumanResourcesJobCandidate**-Objekt auf Änderungen der Volltextsuche. 

   > [!IMPORTANT] 
   > Je nach Entwurf der Datenbank und Entwurf Ihrer Anwendung müssen Sie bei der Migration der Quelldatenbank nach der Migration (und in einigen Fällen vor der Migration) möglicherweise entweder die Datenbank oder die Anwendung oder beide ändern. Informationen zu Transact-SQL-Unterschieden, die Ihre Migration beeinflussen könnten, finden Sie unter [Auflösen von Transact-SQL-Unterschieden während der Migration zur SQL-Datenbank](sql-database-transact-sql-information.md).

     ![Neue Datenmigration – Bewertung und Objektauswahl](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results.png)

9. Klicken Sie auf **SQL-Skript generieren**, um für die Schemaobjekte in der Quelldatenbank ein Skript zu erstellen. 
10. Überprüfen Sie das generierte Skript, und klicken Sie dann ggf. auf **Nächstes Problem**, um die identifizierten Bewertungsprobleme und Empfehlungen zu untersuchen. Beispielsweise gilt für die Volltextsuche beim Aktualisieren die Empfehlung, Ihre Anwendungen mit den Volltextfeatures zu testen. Sie können das Skript speichern oder kopieren, wenn Sie möchten.

     ![Neue Datenmigration – generiertes Datenmigrationsskript](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-generated-script.png)

11. Klicken Sie auf **Schema bereitstellen**, und beobachten Sie den Schemamigrationsprozess.

     ![Neue Datenmigration – Schemamigration](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-schema-migration.png)

12. Wenn die Schemamigration abgeschlossen ist, überprüfen Sie die Ergebnisse auf Fehler, und klicken Sie dann – sofern keine vorhanden sind – auf **Daten migrieren**.
13. Überprüfen Sie auf der Seite **Tabellen auswählen** die zur Migration ausgewählten Tabellen, und klicken Sie dann auf **Datenmigration starten**.

     ![Neue Datenmigration – Datenmigration](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-data-migration.png)

14. Beobachten Sie den Migrationsprozess.

     ![Neue Datenmigration – Datenmigrationsprozess](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-data-migration-process.png)

## <a name="connect-to-the-database-with-ssms"></a>Herstellen einer Verbindung für die Datenbank mit SSMS

Verwenden Sie [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms), um eine Verbindung mit Ihrem Azure SQL-Datenbankserver herzustellen.

1. Öffnen Sie SQL Server Management Studio.

2. Geben Sie im Dialogfeld **Mit Server verbinden** die folgenden Informationen ein:

   | Einstellung       | Empfohlener Wert | Beschreibung | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Servertyp | Datenbankmodul | Dieser Wert ist erforderlich. |
   | Servername | Der vollqualifizierte Servername | Der Name sollte etwa wie folgt lauten: **mynewserver20170824.database.windows.net**. |
   | Authentifizierung | SQL Server-Authentifizierung | In diesem Tutorial haben wir als einzigen Authentifizierungstyp die SQL-Authentifizierung konfiguriert. |
   | Anmeldung | Das Serveradministratorkonto | Hierbei handelt es sich um das Konto, das Sie beim Erstellen des Servers angegeben haben. |
   | Kennwort | Das Kennwort für das Serveradministratorkonto | Hierbei handelt es sich um das Kennwort, das Sie beim Erstellen des Servers angegeben haben. |

   ![Verbindung mit dem Server herstellen](./media/sql-database-connect-query-ssms/connect.png)

3. Klicken Sie im Dialogfeld **Mit Server verbinden** auf **Optionen**. Geben Sie im Abschnitt **Verbindung mit Datenbank herstellen** den Text **mySampleDatabase** ein, um eine Verbindung mit dieser Datenbank herzustellen.

   ![Herstellen einer Verbindung mit der Datenbank auf dem Server](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. Klicken Sie auf **Verbinden**. Die Objekt-Explorer-Fenster wird in SSMS geöffnet. 

5. Erweitern Sie im Objekt-Explorer die Option **Datenbanken** und anschließend die Option **mySampleDatabase**, um die Objekte in der Beispieldatenbank anzuzeigen.

   ![Datenbankobjekte](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="change-database-properties"></a>Ändern von Datenbankeigenschaften

Sie können die Dienstebene, die Leistungsstufe und den Kompatibilitätsgrad über SQL Server Management Studio ändern. Es empfiehlt sich, dass Sie während des Importierens eine Datenbank mit einer höheren Leistungsstufe importieren, um die bestmögliche Leistung zu erreichen. Skalieren Sie diese nach dem abgeschlossenen Import jedoch herunter, um Geld zu sparen, bis Sie die importierte Datenbank aktiv verwenden können. Durch Ändern des Kompatibilitätsgrads erreichen Sie eine bessere Leistung und erhalten Zugriff auf die neuesten Funktionen des Azure SQL-Datenbankdiensts. Wenn Sie eine ältere Datenbank migrieren, bleibt ihr Datenbankkompatibilitätsgrad auf der niedrigsten Ebene, die mit der zu importierenden Datenbank kompatibel ist. Weitere Informationen finden Sie unter [Improved query performance with compatibility Level 130 in Azure SQL Database](sql-database-compatibility-level-query-performance-130.md) (Verbesserte Abfrageleistung mit Kompatibilitätsgrad 130 in Azure SQL-Datenbank).

1. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf **mySampleDatabase** und dann auf **Neue Abfrage**. Ein Abfragefenster mit einer Verbindung mit Ihrer Datenbank wird geöffnet.

2. Führen Sie folgenden Befehl aus, um die auf die Dienstebene auf **Standard** und die Leistungsstufe auf **S1** festzulegen.

    ```sql
    ALTER DATABASE mySampleDatabase 
    MODIFY 
        (
        EDITION = 'Standard'
        , MAXSIZE = 250 GB
        , SERVICE_OBJECTIVE = 'S1'
    );
    ```

## <a name="next-steps"></a>Nächste Schritte 
In diesem Tutorial haben Sie Folgendes gelernt:

> * Erstellen einer leeren Azure SQL-Datenbank im Azure-Portal 
> * Erstellen einer Firewall auf Serverebene im Azure-Portal 
> * Verwenden des [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) zum Importieren Ihrer SQL Server-Datenbank in die leere Azure SQL-Datenbank 
> * Verwenden von [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS), um die Datenbankeigenschaften zu ändern.

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihre Datenbank schützen.

> [!div class="nextstepaction"]
> [Schützen der Azure SQL-Datenbank](sql-database-security-tutorial.md)



