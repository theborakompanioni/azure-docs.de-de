---
title: Alle Themen über den SQL-Datenbankdienst | Microsoft Docs
description: Tabelle mit allen Themen für den Azure-Dienst namens „SQL-Datenbank“ auf https://azure.microsoft.com/de-de/documentation/articles/, Titel und Beschreibung.
services: sql-database
documentationcenter: ''
author: MightyPen
manager: jhubbard
editor: MightyPen

ms.service: sql-database
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2016
ms.author: genemi

---
# <a name="all-topics-for-azure-sql-database-service"></a>Alle Themen für den Azure SQL-Datenbankdienst
Dieses Thema führt alle Themen mit direktem Bezug zum **SQL-Datenbankdienst** in Azure auf. Sie können auf dieser Webseite mit **STRG+F**nach Schlüsselwörtern suchen, um aktuell interessante Themen zu finden.

## <a name="new"></a>Neu
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 1 |[Daxko/CSI verwendet Azure, um den Entwicklungszyklus zu beschleunigen und sowohl Kundendienste als auch Leistung zu verbessern](sql-database-implementation-daxko.md) |Erfahren Sie, wie Daxko/CSI SQL-Datenbank verwendet, um den Entwicklungszyklus zu beschleunigen und sowohl Kundendienste als auch Leistung zu verbessern. |
| 2 |[Azure bietet GEP globale Reichweite und mehr Effizienz](sql-database-implementation-gep.md) |Erfahren Sie, wie GEP mithilfe von SQL-Datenbank mehr globale Kunden erreicht und größere Effizienz erzielt. |
| 3 |[Dank Azure erweiterte SnelStart seine Businessdienste schnell um 1.000 neue Azure SQL-Datenbanken pro Monat](sql-database-implementation-snelstart.md) |Erfahren Sie, wie SnelStart seine Businessdienste dank SQL-Datenbank schnell um 1.000 neue Azure SQL-Datenbanken pro Monat erweiterte. |
| 4 |[Umbraco verwendet Azure SQL-Datenbank, um Dienste für Tausende von Mandanten in der Cloud schnell bereitzustellen und zu skalieren](sql-database-implementation-umbraco.md) |Erfahren Sie, wie Umbraco SQL-Datenbank verwendet, um Dienste für Tausende von Mandanten in der Cloud schnell bereitzustellen und zu skalieren. |
| 5 |[Verwalten von Azure SQL-Datenbanken mit PowerShell](sql-database-manage-powershell.md) |Verwalten von Azure SQL-Datenbank mit PowerShell. |
| 6 |[SSMS-Unterstützung für Azure AD MFA mit SQL-Datenbank und SQL Data Warehouse](sql-database-ssms-mfa-authentication.md) |Verwenden Sie die Multi-Factor Authentication mit SSMS für SQL-Datenbank und SQL Data Warehouse. |
| 7 |[Datenbanktransaktionseinheiten (DTUs) und elastische Datenbanktransaktionseinheiten (eDTUs)](sql-database-what-is-a-dtu.md) |Es wird beschrieben, was eine Azure SQL-Datenbanktransaktionseinheit ist. |

## <a name="updated-articles,-sql-database"></a>Aktualisierte Artikel zu SQL-Datenbank
In diesem Abschnitt werden Artikel aufgelistet, die kürzlich mit einem sehr umfangreichen oder wesentlichen Update aktualisiert wurden. Für jeden aktualisierten Artikel wird ein grober Ausschnitt des hinzugefügten Markdowntexts angezeigt. Die Artikel wurden zwischen dem **22.08.2016** und dem **05.10.2016** aktualisiert.

| &nbsp; | Artikel | Aktualisierter Text, Ausschnitt | Aktualisierungsdatum |
| ---:|:--- |:--- |:--- |
| 8 |[Verwalten von Azure SQL-Datenbanken mit PowerShell](sql-database-command-line-tools.md) |Erstellen Sie eine Ressourcengruppe für unsere SQL-Datenbank und die zugehörigen Azure-Ressourcen mit dem Cmdlet New-AzureRmResourceGroup (https://msdn.microsoft.com/library/azure/mt759837.aspx). * $resourceGroupName = "resourcegroup1" $resourceGroupLocation = "northcentralus" New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation * Weitere Informationen finden Sie unter „Verwenden von Azure PowerShell mit Azure Resource Manager“ (../powershell-azure-resource-manager.md). Ein Beispielskript finden Sie unter „Erstellen eines SQL-Datenbank PowerShell-Skripts“ (sql-database-get-started-powershell.md create-a-sql-database-powershell-script). **Erstellen eines SQL-Datenbankservers** Erstellen Sie einen SQL-Datenbankserver mit dem Cmdlet New-AzureRmSqlServer (https://msdn.microsoft.com/library/azure/mt603715.aspx). Ersetzen Sie *server1* durch den Namen Ihres Servers. Servernamen müssen für alle Azure SQL-Datenbank-Server eindeutig sein. Es wird eine Fehlermeldung angezeigt, wenn der Servername bereits vergeben ist. Dieser Befehl kann mehrere Minuten in Anspruch nehmen. Die Ressource |2016-09-14 |
| 9 |[Kopieren einer Azure SQL-Datenbank mithilfe von PowerShell](sql-database-copy-powershell.md) |SQL Datenbank-Quelle (die vorhandene Datenbank zum Kopieren)  --  $sourceDbName = "db1"  $sourceDbServerName = "server1"  $sourceDbResourceGroupName = "rg1"  SQL Datenbank-Kopie (die neu zu erstellende Datenbank)  --  $copyDbName = "db1_copy"  $copyDbServerName = "server2"  $copyDbResourceGroupName = "rg2"  Kopiere eine Datenbank auf den gleichen Server  --  New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyDatabaseName $copyDbName  Kopiere eine Datenbank auf einen anderen Server  --  New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -CopyDatabaseName $copyDbName  Kopiere eine Datenbank in einen Pool für elastische Datenbanken  --  $poolName = "pool1"  New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyReso |2016-09-08 |
| 10 |[Erstellen eines Pools für elastische Datenbanken mit C#](sql-database-elastic-pool-create-csharp.md) |Console.WriteLine("Server-Firewall...");  FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);  Console.WriteLine("Server-Firewall: " + fwr.FirewallRule.Id);  Console.WriteLine("Elastischer Pool...");  ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool(_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);  Console.WriteLine("Elastischer Pool: " + epr.ElasticPool.Id);  Console.WriteLine("Datenbank...");  DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);  Console.WriteLine("Datenbank: " + dbr.Database.Id);  Console.WriteLine("Drücken Sie eine beliebige Taste...");  Console.ReadKey();  }  static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupNa |2016-09-14 |
| 11 |[PowerShell script for identifying databases suitable for an elastic database pool (PowerShell-Skript zum Ermitteln von Datenbanken mit Eignung für einen Pool für elastische Datenbanken)](sql-database-elastic-pool-database-assessment-powershell.md) |** Für Kandidaten für einen Pool für elastische Datenbanken schließen wir Master und alle Datenbanken aus, die bereits in einem Pool enthalten sind. Sie können weitere Datenbanken in die Ausschluss-Liste nach Bedarf hinzufügen** $ListOfDBs = Get-AzureRmSqlDatabase -ServerName $servername.Split('.') 0  -ResourceGroupName $ResourceGroupName  /  Where-Object {$*.DatabaseName -notin ("master") -and $*.CurrentServiceLevelObjectiveName -notin ("ElasticPool") -and $*.CurrentServiceObjectiveName -notin ("ElasticPool")} $outputConnectionString = "Data Source=$outputServerName;Integrated Security=false;Initial Catalog=$outputdatabaseName;User Id=$outputDBUsername;Password=$outputDBpassword" $destinationTableName = "resource_stats_output" ** Erstellen Sie eine Tabelle in der Ausgabedatenbank zur Metrikensammlung** $sql = " IF  NOT EXISTS (SELECT * FROM sys.objects WHERE object_id = OBJECT_ID(N'$($destinationTableName)') AND type in (N'U')) BEGIN Create Table $($destinationTableName) (database_name varchar(128), slo varchar(20), end_time datetime, avg_cpu float, avg* |2016-09-29 |
| 12 |[SQL-Datenbank-Tutorial: Erstellen einer SQL-Datenbank in Minuten mit dem Azure-Portal](sql-database-get-started.md) |! Neue SQL-Datenbank 1 (./media/sql-database-get-started/sql-database-new-database-1.png) 3. Klicken Sie auf **SQL-Datenbank** , um das Blatt „SQL-Datenbank“ zu öffnen. Der Inhalt dieses Blatt variiert abhängig von Ihrer Anzahl von Abonnements und Ihren vorhandenen Objekten (z.B. vorhandene Server).  ! Neue SQL-Datenbank 2 (./media/sql-database-get-started/sql-database-new-database-2.png) 4. Geben Sie im Textfeld **Datenbankname** einen Namen für Ihre erste Datenbank ein, beispielsweise „Meine-Datenbank“. Wenn der angegebene Name gültig ist, wird ein grünes Häkchen angezeigt.  ! Neue SQL-Datenbank 3 (./media/sql-database-get-started/sql-database-new-database-3.png) 5. Wählen Sie ein Abonnement aus, falls Sie über mehrere Abonnements verfügen. 6. Klicken Sie unter **Ressourcengruppe** auf **Neu erstellen**, und geben Sie einen Namen für Ihre erste Ressourcengruppe ein, beispielsweise „Meine-Ressourcengruppe“. Wenn der angegebene Name gültig ist, wird ein grünes Häkchen angezeigt.  ! Neue SQL-Datenbank 4 (./media/sql-database-get-started/sql-database-new-database-4.png) 7. Unter **Quelle auswählen* |2016-09-08 |
| 13 |[Ausprobieren von SQL-Datenbank: Verwenden von C# zum Erstellen einer SQL-Datenbank mithilfe der SQL-Datenbankbibliothek für .NET](sql-database-get-started-csharp.md) |**Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen**. Mit dem folgenden PowerShell-Skript werden die Active Directory-Anwendung (AD) und der Dienstprinzipal erstellt, den wir zum Authentifizieren der C-App benötigen. Das Skript gibt Werte aus, die für das vorhergehende C-Beispiel erforderlich sind. Ausführliche Informationen finden Sie unter „Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell“ (../resource-group-authenticate-service-principal.md).  Melden Sie sich bei Azure an.  Add-AzureRmAccount  Wenn Sie über mehrere Abonnements verfügen, heben Sie die Auskommentierung auf, und wählen Sie das Abonnement aus, mit dem Sie arbeiten möchten.  $subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}" Set-AzureRmContext -SubscriptionId $subscriptionId  Stellen Sie diese Werte für Ihre neue AAD-App bereit.  $appName ist der Anzeigename für Ihre App, der in Ihrem Verzeichnis eindeutig sein muss.  $uri muss keine echte Uri sein.  $secret ist ein Kennwort, das Sie erstellen.  $appName = "{app-name}"  $uri = "http://{app-name}"  $secret = "{app-password}"  Erstellen einer AAD-App  $azureAdApplication = New-AzureRmADApp |2016-09-23 |
| 14 |[Verwalten von Azure SQL-Datenbanken über das Azure-Portal](sql-database-manage-portal.md) |Zum Anzeigen oder Aktualisieren Ihrer Datenbankeinstellungen klicken Sie auf dem Blatt „SQL-Datenbank“ auf die gewünschte Einstellung: ! Einstellungen der SQL-Datenbank (./media/sql-database-manage-portal/settings.png) ** Wie finde ich den vollqualifizierten Servernamen einer SQL-Datenbank?** Zum Anzeigen des Servernamens Ihrer Datenbank klicken Sie auf dem Blatt **SQL-Datenbank** auf **Übersicht** und notieren den Servernamen: ! Einstellungen der SQL-Datenbank (./media/sql-database-manage-portal/server-name.png) ** Wie verwalte ich Firewallregeln zum Steuern des Zugriffs auf meinen SQL-Server und die Datenbank?** Zum Anzeigen, Erstellen oder Aktualisieren von Firewallregeln klicken Sie auf dem Blatt **SQL-Datenbank** auf **Serverfirewall festlegen**. Details finden Sie unter „Konfigurieren einer Firewallregel auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals“ (sql-database-configure-firewall-settings.md). ! Firewall-Regeln (./media/sql-database-manage-portal/sql-database-firewall.png) ** Wie ändere ich die Dienstebene oder Leistungsstufe meiner SQL-Datenbank?** Zum Aktualisieren der Dienstebene oder Leistungsstufe einer SQL-Datenbank klicken Sie auf **Tarif (s |2016-09-20 |

## <a name="get-started"></a>Erste Schritte
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 15 |[Erstellt mehrinstanzenfähige Apps mit Azure SQL-Datenbank mit Isolation und Effizienz](sql-database-build-multi-tenant-apps.md) |Erfahren Sie, wie Sie mithilfe von Azure SQL-Datenbank mehrinstanzenfähige Apps entwickeln können. |
| 16 |[Herstellen einer Verbindung mit einer SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md) |Erfahren Sie, wie Sie über SQL Server Management Studio (SSMS) eine Verbindung mit einer SQL-Datenbank in Azure herstellen. Führen Sie dann eine Beispielabfrage mithilfe von Transact-SQL (T-SQL) aus. |
| 17 |[Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von .NET (C#)](sql-database-develop-dotnet-simple.md) |Verwenden Sie den Beispielcode in diesem Schnelleinstieg zum Erstellen einer modernen Anwendung in C#, die mit Azure SQL-Datenbank durch eine leistungsfähige relationale Datenbank in der Cloud unterstützt wird. |
| 18 |[Erstellen eines neuen Pools für elastische Datenbanken mit dem Azure-Portal](sql-database-elastic-pool-create-portal.md) |Enthält Informationen zum Hinzufügen eines skalierbaren Pools für elastische Datenbanken zu Ihrer SQL-Datenbankkonfiguration, um eine einfachere Verwaltung und Ressourcenfreigabe zwischen zahlreichen Datenbanken zu ermöglichen. |
| 19 |[Kennenlernen der Azure SQL-Datenbank-Tutorials](sql-database-explore-tutorials.md) |Erfahren Sie mehr über die Features und Funktionalität von SQL-Datenbank |
| 20 |[SQL-Datenbank-Tutorial: Erstellen einer SQL-Datenbank in Minuten mit dem Azure-Portal](sql-database-get-started.md) |Hier erhalten Sie Informationen zum Einrichten von logischen SQL-Datenbankservern, Serverfirewallregeln, SQL-Datenbanken und Beispieldaten. Darüber hinaus erfahren Sie, wie Sie eine Verbindung mit Clienttools herstellen, Benutzer konfigurieren und eine Datenbankfirewallregel einrichten. |
| 21 |[Azure SQL-Datenbank sichert und schützt](sql-database-helps-secures-and-protects.md) |Erfahren Sie, wie SQL-Datenbank hilft, sichert und schützt |
| 22 |[Azure SQL-Datenbank – Lernen &amp; Anpassungen](sql-database-learn-and-adapt.md) |Erfahren Sie, wie SQL-Datenbank lernt und sich anpasst. |
| 23 |[Wählen Sie eine SQL Server-Cloudoption: Azure SQL-Datenbank (PaaS) oder SQL Server auf Azure-VMs (IaaS)](sql-database-paas-vs-sql-server-iaas.md) |Erfahren Sie, welche SQL Server-Cloudoption sich am besten für Ihre Anwendung eignet: Azure SQL-Datenbank (PaaS) oder SQL Server auf Azure Virtual Machines. |
| 24 |[Spontane Skalierung von Azure SQL-Datenbank](sql-database-scale-on-the-fly.md) |Lernen Sie die spontane Skalierung von Azure SQL-Datenbank kennen |
| 25 |[Explore Azure SQL Database Solution Quick Starts (Entdecken Sie die lösungsbezogenen Schnellstarts für Azure SQL-Datenbank)](sql-database-solution-quick-starts.md) |Informationen zu Azure SQL-Datenbanklösungen |
| 26 |[Azure SQL-Datenbank funktioniert in Ihrer Umgebung](sql-database-works-in-your-environment.md) |Erfahren Sie, wie SQL-Datenbank hilft, sichert und schützt |

## <a name="build-an-app"></a>Eine App erstellen
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 27 |[Durchführen der Problembehandlung, Diagnose und Verhinderung von SQL-Verbindungsfehlern und vorübergehenden Fehlern für SQL-Datenbank](sql-database-connectivity-issues.md) |Erfahren Sie, wie Sie einen SQL-Verbindungsfehler oder vorübergehenden Fehler in Azure SQL-Datenbank behandeln, diagnostizieren und verhindern. |
| 28 |[Herstellen der Verbindung mit einer SQL-Datenbank mit Visual Studio](sql-database-connect-query.md) |Schreiben Sie ein Programm in C# zum Abfragen und Herstellen einer Verbindung mit einer SQL-Datenbank. Informationen zu IP-Adressen, Verbindungszeichenfolgen, sicherer Anmeldung und kostenlosem Visual Studio. |
| 29 |[Andere Ports als 1433 für ADO.NET 4.5 und SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md) |Bei Clientverbindungen von ADO.NET mit Azure SQL-Datenbank V12 wird der Proxy manchmal umgangen und direkt mit der Datenbank interagiert. Andere Ports als 1433 werden wichtig. |
| 30 |[SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Datenbankverbindungsfehler und andere Probleme](sql-database-develop-error-messages.md) |Erfahren Sie mehr über SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen, beispielsweise zu häufigen Datenbankverbindungsfehlern, Datenbankkopiefehlern und allgemeinen Fehlern. |
| 31 |[Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von PHP unter Windows](sql-database-develop-php-simple.md) |Enthält ein PHP-Beispielprogramm, das eine Verbindung von einem Windows-Client mit Azure SQL-Datenbank herstellt, sowie Links zu den auf dem Client erforderlichen Softwarekomponenten. |
| 32 |[Ausprobieren von SQL-Datenbank: Verwenden von C# zum Erstellen einer SQL-Datenbank mithilfe der SQL-Datenbankbibliothek für .NET](sql-database-get-started-csharp.md) |Probieren Sie SQL-Datenbank für das Entwickeln von SQL- und C#-Apps aus, und erstellen Sie eine Azure SQL-Datenbank mithilfe der SQL-Datenbankbibliothek für .NET. |
| 33 |[Getting started with JSON features in Azure SQL Database](sql-database-json-features.md) |Mit Azure SQL-Datenbank können Sie Daten in JavaScript Object Notation (JSON) analysieren, abfragen und formatieren. |
| 34 |[Beheben von Verbindungsproblemen mit der Azure SQL-Datenbank](sql-database-troubleshoot-common-connection-issues.md) |Schritte zum Ermitteln und Behandeln von häufigen Verbindungsproblemen für Azure SQL-Datenbank. |
| 35 |[Fehler „Datenbank auf dem Server ist zurzeit nicht verfügbar.“ beim Herstellen einer Verbindung mit SQL-Datenbank](sql-database-troubleshoot-connection.md) |Problembehandlung des Fehlers „Datenbank auf dem Server ist zurzeit nicht verfügbar.“, wenn eine Anwendung eine Verbindung mit SQL-Datenbank herstellt. |

## <a name="develop"></a>Entwickeln
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 36 |[Abrufen der Client-ID und des Schlüssels für die Verbindung mit der SQL-Datenbank aus dem Code](sql-database-client-id-keys.md) |Erstellen Sie einen Dienstprinzipalnamen für den Zugriff auf SQL-Datenbank aus dem Code. |
| 37 |[Herstellen von Verbindungen mit der SQL-Datenbank mithilfe von Java mit JDBC unter Windows](sql-database-develop-java-simple.md) |Zeigt ein Java-Codebeispiel zum Herstellen einer Verbindung mit Azure SQL-Datenbank. Das Beispiel verwendet JDBC und wird auf einem Clientcomputer unter Windows ausgeführt. |
| 38 |[Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Node.js](sql-database-develop-nodejs-simple.md) |Zeigt ein Node.js-Codebeispiel zum Herstellen einer Verbindung mit Azure SQL-Datenbank. |
| 11,9 |[Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md) |Hier erfahren Sie mehr über verfügbare Verbindungsbibliotheken und bewährte Methoden für Anwendungen, die eine Verbindung mit SQL-Datenbank herstellen. |
| 40 |[Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Python](sql-database-develop-python-simple.md) |Zeigt ein Python-Codebeispiel zum Herstellen einer Verbindung mit Azure SQL-Datenbank. |
| 41 |[Herstellen von Verbindungen mit SQL-Datenbanken mithilfe von Ruby](sql-database-develop-ruby-simple.md) |Geben Sie ein Ruby-Codebeispiel an, das zum Herstellen einer Verbindung mit Azure SQL-Datenbank ausgeführt werden kann. |
| 42 |[Erste Schritte mit temporalen Tabellen in der Azure SQL-Datenbank](sql-database-temporal-tables.md) |Lernen Sie die ersten Schritte mit temporalen Tabellen in der Azure SQL-Datenbank. |

## <a name="performance-and-scale"></a>Leistung und Skalierbarkeit
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 43 |[SQL-Datenbankratgeber](sql-database-advisor.md) |Der Azure SQL-Datenbank-Ratgeber stellt Empfehlungen für Ihre vorhandenen SQL-Datenbanken bereit, die die aktuelle Abfrageleistung verbessern können. |
| 44 |[Azure SQL-Datenbankratgeber im Azure-Portal](sql-database-advisor-portal.md) |Mit dem Azure SQL-Datenbankratgeber im Azure-Portal können Sie Empfehlungen für Ihre vorhandenen SQL-Datenbanken anzeigen und umsetzen und so die derzeitige Abfrageleistung verbessern. |
| 45 |[Übersicht über das Durchführen von Vergleichstests für Azure SQL-Datenbank](sql-database-benchmark-overview.md) |Dieses Thema beschreibt den Azure SQL Database Benchmark-Vergleichstest zum Messen der Leistung von Azure SQL-Datenbank. |
| 46 |[Wo sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool-guidance.md) |Ein elastischer Datenbankpool stellt eine Sammlung der verfügbaren Ressourcen dar, die von einer Gruppe von elastischen Datenbanken gemeinsam verwendet werden. Dieses Dokument enthält Anleitungen, die Ihnen helfen sollen, die Eignung eines elastischen Datenbankpools für eine Gruppe von Datenbanken einzuschätzen. |
| 47 |[Erste Schritte mit Tools für elastische Datenbanken](sql-database-elastic-scale-get-started.md) |Grundlegende Erläuterung der Funktion „Tools für elastische Datenbanken“ von Azure SQL, einschließlich einer einfachen Beispiel-App. |
| 48 |[SQL-Datenbank – Häufig gestellte Fragen](sql-database-faq.md) |Antworten auf häufig gestellte Fragen von Kunden zu Cloud-Datenbanken, zur Azure SQL-Datenbank, das Microsoft Managementsystem für relationale Datenbanken (RDBMS) und Database as a Service in der Cloud. |
| 49 |[Erste Schritte mit In-Memory (Vorschau) in SQL-Datenbank](sql-database-in-memory.md) |SQL In-Memory-Technologien verbessern die Leistung von Transaktions- und Analyseworkloads erheblich. Erfahren Sie, wie Sie von diesen Technologien profitieren. |
| 50 |[Verwenden von In-Memory-OLTP (Vorschau) zur Verbesserung der Anwendungsleistung in SQL-Datenbank](sql-database-in-memory-oltp-migration.md) |Verwenden von In-Memory OLTP zum Verbessern der Transaktionsleistung in einer vorhandenen SQL-Datenbank. |
| 51 |[Überwachen des In-Memory-OLTP-Speichers](sql-database-in-memory-oltp-monitoring.md) |Einschätzen und Überwachen der XTP-In-Memory-Speicherverwendung und -kapazität; Beheben des Kapazitätsfehlers 41823 |
| 52 |[Verwenden des Abfragespeichers in Azure SQL-Datenbank](sql-database-operate-query-store.md) |Erfahren Sie, wie Sie den Abfragespeicher in Azure SQL-Datenbank verwenden. |
| 53 |[Einblicke in die SQL-Datenbankleistung](sql-database-performance.md) |Die Azure SQL-Datenbank bietet Leistungstools, mit denen Sie Bereiche identifizieren können, in denen die aktuelle Abfrageleistung verbessert werden kann. |
| 54 |[Azure SQL-Datenbank und Leistung für Einzeldatenbanken](sql-database-performance-guidance.md) |Mit den Informationen in diesem Artikel können Sie ermitteln, welche Dienstebene für Ihre Anwendung am besten geeignet ist. Außerdem werden Möglichkeiten zum Optimieren Ihrer Anwendung empfohlen, um mit Azure SQL-Datenbank das beste Ergebnis zu erzielen. |
| 55 |[Query Performance Insight für Azure SQL-Datenbank](sql-database-query-performance.md) |Mit der Überwachung der Abfrageleistung werden für eine Azure SQL-Datenbank die Abfragen mit der höchsten CPU-Auslastung identifiziert. |
| 56 |[Ändern der Dienstebene und Leistungsstufe (Tarif) einer SQL-Datenbank](sql-database-scale-up.md) |In „Ändern der Dienstebene und Leistungsstufe einer Azure SQL-Datenbank“ wird beschrieben, wie Sie die Dienstebene und Leistungsstufe Ihrer SQL-Datenbank zentral hoch- oder herunterstufen. Ändern des Tarifs einer Azure SQL-Datenbank. |
| 57 |[Überwachen der Datenbankleistung mithilfe von Azure SQL-Datenbank](sql-database-single-database-monitor.md) |Lernen Sie die Optionen zum Überwachen Ihrer Datenbank mithilfe von Azure-Tools und dynamischen Verwaltungssichten kennen. |
| 58 |[Tipps zur Optimierung der SQL-Datenbankleistung](sql-database-troubleshoot-performance.md) |Tipps für die Leistungsoptimierung in Azure SQL-Datenbank durch Auswertung und Verbesserung. |
| 59 |[Gewusst wie: Verbessern der Leistung von SQL-Datenbankanwendungen mithilfe von Batchverarbeitung](sql-database-use-batching-to-improve-performance.md) |Dieses Thema belegt, dass die Verwendung der Batchverarbeitung bei Datenbankvorgängen erheblich zur Verbesserung der Geschwindigkeit und Skalierbarkeit Ihrer Azure SQL-Datenbankanwendungen beiträgt. Die Batchverarbeitungstechniken können zwar für jede beliebige SQL Server-Datenbank verwendet werden, der Artikel konzentriert sich jedoch auf Azure. |

## <a name="tools-for-scaling-out"></a>Tools für das horizontale Skalieren
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 60 |[Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md) |In diesem Artikel werden die Anforderungen und allgemeinen Datenarchitekturmuster von mehrinstanzenfähigen Datenbankanwendungen, die in einer Cloudumgebung ausgeführt werden, sowie die verschiedenen Vor- und Nachteile dieser Muster erläutert. Außerdem wird beschrieben, wie diese Anforderungen mit den elastischen Pools und elastischen Tools von Azure SQL-Datenbank ohne Kompromisse erfüllt werden können. |
| 61 |[Migrieren von vorhandenen Datenbanken zu horizontaler Hochskalierung](sql-database-elastic-convert-to-use-elastic-tools.md) |Erstellen eines Shardzuordnungs-Managers, um Sharddatenbanken für die Verwendung der Tools für elastische Datenbanken umzuwandeln |
| 62 |[Erstellen skalierbarer Clouddatenbanken](sql-database-elastic-database-client-library.md) |Erstellen skalierbarer .NET-Datenbank-Apps mit der Clientbibliothek für elastische Datenbanken |
| 63 |[Leistungsindikatoren für den Shardzuordnungs-Manager](sql-database-elastic-database-perf-counters.md) |ShardMapManager-Klasse und datenabhängiges Routing – Leistungsindikatoren |
| 64 |[Hinzufügen eines Shards mithilfe der Tools für elastische Datenbanken](sql-database-elastic-scale-add-a-shard.md) |Erfahren Sie, wie Sie einem Shard-Satz mithilfe der Elastic Scale-APIs neue Shards hinzufügen. |
| 65 |[Bereitstellen eines Split-Merge-Diensts](sql-database-elastic-scale-configure-deploy-split-and-merge.md) |Aufteilen und Zusammenführen mit Tools für elastische Datenbanken |
| 66 |[Datenabhängiges Routing](sql-database-elastic-scale-data-dependent-routing.md) |Erfahren Sie, wie Sie die ShardMapManager-Klasse in .NET-Apps für das datenabhängige Routing, einem Feature von elastischen Datenbanken für Azure SQL-Datenbank, verwenden können. |
| 67 |[Tools für elastische Datenbanken – Häufig gestellte Fragen](sql-database-elastic-scale-faq.md) |Häufig gestellte Fragen zur Elastic Scale-Funktion der Azure SQL-Datenbank. |
| 68 |[Tools für elastische Datenbanken – Glossar](sql-database-elastic-scale-glossary.md) |Erläuterung von Begriffen, die für die Tools für flexible Datenbanken verwendet werden |
| 69 |[Übersicht über Features für elastische Datenbanken](sql-database-elastic-scale-introduction.md) |SaaS-Entwickler (Software-as-a-Service) können mit diesen Tools problemlos elastische und skalierbare Datenbanken in der Cloud erstellen. |
| 70 |[Anmeldeinformationen für den Zugriff auf die Clientbibliothek für elastische Datenbanken](sql-database-elastic-scale-manage-credentials.md) |So richten Sie Anmeldeinformationen mit den passenden Berechtigungen (von Administrator- bis Leseberechtigungen) für Apps für elastische Datenbanken ein |
| 71 |[Abfragen von mehreren Shards](sql-database-elastic-scale-multishard-querying.md) |Führen Sie mithilfe der Clientbibliothek für elastische Datenbanken Abfragen mehrerer Shards durch. |
| 72 |[Skalierung mit dem Split-Merge-Tool für elastische Datenbanken](sql-database-elastic-scale-overview-split-and-merge.md) |Hier wird erläutert, wie Sie Shards manipulieren und Daten über einen selbst gehosteten Dienst mithilfe von APIs für elastische Datenbanken verschieben. |
| 73 |[Horizontales Skalieren von Datenbanken mit dem Shardzuordnungs-Manager](sql-database-elastic-scale-shard-map-management.md) |Erfahren Sie, wie Sie "ShardMapManager" und die Clienbtbibliothek für elastische Datenbanken verwenden. |
| 74 |[Split-Merge-Sicherheitskonfiguration](sql-database-elastic-scale-split-merge-security-configuration.md) |Einrichten von x409-Zertifikaten für die Verschlüsselung |
| 75 |[Upgrade einer App auf die neueste Clientbibliothek für elastische Datenbanken](sql-database-elastic-scale-upgrade-client-library.md) |Aktualisieren von Apps und Bibliotheken mithilfe von Nuget |
| 76 |[Clientbibliothek für elastische Datenbanken mit Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) |Verwenden der Clientbibliothek für elastische Datenbanken und von Entity Framework für die Programmierung von Datenbanken |
| 77 |[Verwenden der Clientbibliothek für elastische Datenbanken](sql-database-elastic-scale-working-with-dapper.md) |Verwenden der Clientbibliothek für elastische Datenbanken. |
| 78 |[Mehrinstanzenfähige Anwendungen mit elastischen Datenbanktools und zeilenbasierter Sicherheit](sql-database-elastic-tools-multi-tenant-row-level-security.md) |Erfahren Sie, wie Sie mithilfe von elastischen Datenbanktools in Verbindung mit zeilenbasierter Sicherheit eine Anwendung mit einer hochgradig skalierbaren Datenebene in einer Azure SQL-Datenbank erstellen, die mehrinstanzenfähige Shards unterstützt. |

## <a name="elastic-jobs"></a>Elastische Aufträge
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 79 |[Erstellen und Verwalten von horizontal hochskalierten Azure SQL-Datenbanken mithilfe von elastischen Aufträgen (Vorschau)](sql-database-elastic-jobs-create-and-manage.md) |Führen Sie die Schritte zur Erstellung und Verwaltung eines elastischen Datenbankauftrags aus. |
| 80 |[Erste Schritte mit Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-getting-started.md) |Verwenden von Aufträgen für die elastische Datenbank |
| 81 |[Verwalten horizontal hochskalierter Clouddatenbanken](sql-database-elastic-jobs-overview.md) |Veranschaulicht den Dienst für elastische Datenbankaufträge |
| 82 |[Erstellen und Verwalten von Aufträgen für die elastische SQL-Datenbank mithilfe von PowerShell (Vorschau)](sql-database-elastic-jobs-powershell.md) |PowerShell, verwendet zum Verwalten von Pools für Azure SQL-Datenbanken |
| 83 |[Installieren von Aufträgen für die elastische Datenbank – Übersicht](sql-database-elastic-jobs-service-installation.md) |Schrittweise Anleitung für die Installation der Funktion für elastische Aufträge |
| 84 |[Deinstallieren von Komponenten der Aufträge für die elastische Datenbank](sql-database-elastic-jobs-uninstall.md) |So deinstallieren Sie das Tool für elastische Datenbankaufträge |

## <a name="elastic-pools"></a>Elastische Pools
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 85 |[Was ist ein elastischer Azure-Pool?](sql-database-elastic-pool.md) |Verwalten Sie hunderte oder tausende Datenbanken mithilfe eines Pools. Ein Preis für einen Satz Leistungseinheiten kann über den Pool verteilt werden. Verschieben Sie die Datenbanken je nach Wunsch in den oder aus dem Pool. |
| 86 |[Erstellen eines Pools für elastische Datenbanken mit C#](sql-database-elastic-pool-create-csharp.md) |Verwenden Sie C#-Datenbankentwicklungstechniken, um einen skalierbaren Pool für elastische Datenbanken in Azure SQL-Datenbank zu erstellen, damit Sie Ressourcen für zahlreiche Datenbanken freigeben können. |
| 87 |[Erstellen eines neuen Pools für elastische Datenbanken mit PowerShell](sql-database-elastic-pool-create-powershell.md) |Erfahren Sie, wie Sie mithilfe von PowerShell Ressourcen für Azure SQL-Datenbanken horizontal hochskalieren können, indem Sie für die Verwaltung mehrerer Datenbanken einen skalierbaren Pool für elastische Datenbanken erstellen. |
| 88 |[PowerShell script for identifying databases suitable for an elastic database pool (PowerShell-Skript zum Ermitteln von Datenbanken mit Eignung für einen Pool für elastische Datenbanken)](sql-database-elastic-pool-database-assessment-powershell.md) |Ein elastischer Datenbankpool stellt eine Sammlung der verfügbaren Ressourcen dar, die von einer Gruppe von elastischen Datenbanken gemeinsam verwendet werden. Dieses Dokument enthält ein PowerShell-Skript, das Ihnen helfen soll, die Eignung eines Pools für elastische Datenbanken für eine Gruppe von Datenbanken einzuschätzen. |
| 89 |[Überwachen und Verwalten eines Pools für elastische Datenbanken mit C#](sql-database-elastic-pool-manage-csharp.md) |Methoden zur Entwicklung von C#-Datenbanken verwenden, um einen Pool für elastische Azure SQL-Datenbanken zu verwalten |
| 90 |[Überwachen und Verwalten eines Pools für elastische Datenbanken über das Azure-Portal](sql-database-elastic-pool-manage-portal.md) |Erfahren Sie, wie Sie mithilfe der im Azure-Portal und in der SQL-Datenbank integrierten Intelligenz einen skalierbaren Pool für elastische Datenbanken verwalten, überwachen und skalieren, um die Leistung der Datenbank und die Kostenverwaltung zu optimieren. |
| 91 |[Überwachen und Verwalten eines Pools für elastische Datenbanken mit PowerShell](sql-database-elastic-pool-manage-powershell.md) |Erfahren Sie, wie Sie PowerShell zum Verwalten eines Pools für elastische Datenbanken nutzen |
| 92 |[Überwachen und Verwalten eines Pools für elastische Datenbanken per Transact-SQL](sql-database-elastic-pool-manage-tsql.md) |Es wird beschrieben, wie Sie T-SQL verwenden, um eine Azure SQL-Datenbank in einem elastischen Pool zu erstellen. Außerdem können Sie T-SQL nutzen, um die Datenbank in Pools bzw. aus Pools zu verschieben. |
| 93 |[Abrechnungs- und Preisinformationen für Pools für elastische Datenbanken](sql-database-elastic-pool-price.md) |Hier finden Sie Preisinformationen zu Pools für elastische Datenbanken. |

## <a name="elastic-query"></a>Elastische Abfrage
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 94 |[Erstellen von Berichten für horizontal hochskalierte Clouddatenbanken](sql-database-elastic-query-getting-started.md) |Verwenden datenbankübergreifender Datenbankabfragen |
| 95 |[Erste Schritte mit datenbankübergreifenden Abfragen (vertikale Partitionierung, Vorschau)](sql-database-elastic-query-getting-started-vertical.md) |Sie erfahren, wie Sie Abfragen für elastische Datenbanken für vertikal partitionierte Datenbanken verwenden. |
| 96 |[Horizontal hochskalierte Clouddatenbanken übergreifende Berichte (Vorschau)](sql-database-elastic-query-horizontal-partitioning.md) |Informationen zum Einrichten elastischer Abfragen, die horizontal partitionierte Datenbanken übergreifen |
| 97 |[Übersicht über elastische Datenbankenabfragen in Azure SQL-Datenbank (Vorschau)](sql-database-elastic-query-overview.md) |Übersicht über das Abfragefeature für elastische Datenbanken |
| 98 |[Ausführen von Abfragen über Clouddatenbanken mit unterschiedlichen Schemas hinweg (Vorschau)](sql-database-elastic-query-vertical-partitioning.md) |Einrichten von datenbankübergreifenden Abfragen über vertikale Partitionen |

## <a name="elastic-transaction"></a>Elastische Transaktion
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 99 |[Übersicht über elastische Datenbanktransaktionen mit Azure SQL-Datenbank](sql-database-elastic-transactions-overview.md) |Übersicht über elastische Datenbanktransaktionen mit Azure SQL-Datenbank |

## <a name="backup-and-recovery"></a>Sicherung und Wiederherstellung
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 100 |[Sicherungen für SQL-Datenbank](sql-database-automated-backups.md) |Informieren Sie sich über integrierte Datenbanksicherungen der SQL-Datenbank, mit denen Sie einen früheren Zustand einer Azure SQL-Datenbank wiederherstellen oder eine Datenbank in eine neue Datenbank in einer geografischen Region kopieren können (bis zu 35 Tage). |
| 101 |[Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md) |Erfahren Sie, wie Azure SQL-Datenbank die Geschäftskontinuität in der Cloud sowie die Datenbankwiederherstellung unterstützt und dafür sorgt, dass geschäftskritische Cloudanwendungen in Betrieb gehalten werden. |
| 102 |[Wiederherstellen einer einzelnen Tabelle aus einer Azure SQL-Datenbanksicherung](sql-database-cloud-migrate-restore-single-table-azure-backup.md) |Erfahren Sie, wie Sie eine einzelne Tabelle aus einer Azure SQL-Datenbanksicherung wiederherstellen. |
| 103 |[Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung mithilfe der aktiven Georeplikation in SQL-Datenbank](sql-database-designing-cloud-solutions-for-disaster-recovery.md) |Erfahren Sie, wie Sie cloudbasierte Notfallwiederherstellungslösungen entwerfen, bei denen die Geschäftskontinuität unter Verwendung der Georeplikation für die App-Datensicherung mit Azure SQL-Datenbank erzielt wird. |
| 104 |[Wiederherstellen einer Azure SQL-Datenbank oder Failover auf eine sekundäre Datenbank](sql-database-disaster-recovery.md) |Erfahren Sie, wie Sie eine Datenbank nach Störungen in einem regionalen Rechenzentrum oder dessen Ausfall mithilfe der aktiven Georeplikation oder der Geowiederherstellung von Azure SQL-Datenbank wiederherstellen. |
| 105 |[Ausführen von Notfallwiederherstellungsverfahren](sql-database-disaster-recovery-drills.md) |Hier erhalten Sie Anleitungen und bewährte Methoden für die Verwendung von Azure SQL-Datenbank zum Ausführen von Notfallwiederherstellungsverfahren, mit denen Sie wichtige Geschäftsanwendungen vor Fehlern und Ausfällen schützen. |
| 106 |[Strategien für die Notfallwiederherstellung für Anwendungen mit elastischem SQL-Datenbankpool](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) |Erfahren Sie, wie Sie durch Wahl des richtigen Failovermusters Ihre Cloud-Lösung für die Notfallwiederherstellung entwerfen können. |
| 107 |[Verwenden der RecoveryManager-Klasse zur Behebung von Problemen mit der Shard-Zuordnung](sql-database-elastic-database-recovery-manager.md) |Verwenden der RecoveryManager-Klasse zum Beheben von Problemen mit Shardzuordnungen |
| 108 |[Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank](sql-database-import.md) |Erstellen Sie eine Azure SQL-Datenbank, indem Sie eine vorhandene BACPAC-Datei importieren. |
| 109 |[Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt über das Azure-Portal](sql-database-point-in-time-restore-portal.md) |Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt |
| 110 |[Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt mit PowerShell](sql-database-point-in-time-restore-powershell.md) |Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt |
| 111 |[Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md) |Point-in-Time-Wiederherstellung, Microsoft Azure SQL-Datenbank, Datenbank wiederherstellen, Datenbankwiederherstellung, klassisches Azure-Portal, klassisches Azure-Portal |
| 112 |[Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](sql-database-recovery-using-backups.md) |Hier finden Sie Informationen zur Point-in-Time-Wiederherstellung, mit der Sie ein Rollback für eine Azure SQL-Datenbank durchführen können (bis zu 35 Tage). |
| 113 |[Wiederherstellen einer gelöschten Azure SQL-Datenbank im Azure-Portal](sql-database-restore-deleted-database-portal.md) |Wiederherstellen einer gelöschten Azure SQL-Datenbank (Azure-Portal) |
| 114 |[Wiederherstellen einer gelöschten Azure SQL-Datenbank mit PowerShell](sql-database-restore-deleted-database-powershell.md) |Wiederherstellen einer gelöschten Azure SQL-Datenbank (PowerShell) |
| 115 |[Wiederherstellen einer Datenbank zu einem früheren Zeitpunkt, einer gelöschten Datenbank oder eines Rechenzentrums nach einem Ausfall](sql-database-troubleshoot-backup-and-restore.md) |Erfahren Sie, wie eine Clouddatenbank bei Fehlern oder Ausfällen mithilfe von Sicherungen und Replikaten in Azure SQL-Datenbank wiederhergestellt wird. |

## <a name="migrate"></a>Migrieren
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 116 |[Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei per SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) |Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank exportieren, BACPAC-Datei exportieren, sqlpackage |
| 117 |[Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei mit SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) |Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank exportieren, BACPAC-Datei exportieren, Assistent „Datenebenenanwendung exportieren“ |
| 118 |[Importieren aus einer BACPAC-Datei in SQL-Datenbank per SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md) |Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank importieren, BACPAC-Datei importieren, sqlpackage |
| 119 |[Importieren aus BACPAC zur SQL-Datenbank mithilfe von SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) |Microsoft Azure SQL-Datenbank, Datenbankbereitstellung, Datenbankmigration, Datenbank importieren, Datenbank exportieren, Migrations-Assistent |
| 120 |[Migrieren von SQL Server-Datenbank auf SQL-Datenbank mit dem Assistenten zum Bereitstellen einer Datenbank unter Microsoft Azure-Datenbank](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) |Microsoft Azure SQL-Datenbank, Datenbankmigration, Assistent für Microsoft Azure-Datenbank |
| 121 |[Migrieren von SQL Server-Datenbank zu Azure SQL-Datenbank per Transaktionsreplikation](sql-database-cloud-migrate-compatible-using-transactional-replication.md) |Microsoft Azure SQL-Datenbank, Datenbankmigration, Datenbank importieren, Transaktionsreplikation |
| 122 |[Ermitteln der SQL-Datenbankkompatibilität mithilfe von SqlPackage.exe](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md) |Microsoft Azure SQL-Datenbank, Datenbankmigration, SQL-Datenbankkompatibilität, SqlPackage |
| 123 |[Verwenden Sie SQL Server Management Studio zum Ermitteln der Kompatibilität von SQL-Datenbank vor der Migration zu Azure SQL-Datenbank.](sql-database-cloud-migrate-determine-compatibility-ssms.md) |Microsoft Azure SQL-Datenbank, Datenbankmigration, SQL-Datenbankkompatibilität, Assistent „Datenebenenanwendung exportieren“ |
| 124 |[Verwenden Sie den SQL Azure-Migrations-Assistenten, um vor der Migration zu Azure SQL-Datenbank Kompatibilitätsprobleme mit SQL Server-Datenbank zu beheben.](sql-database-cloud-migrate-fix-compatibility-issues.md) |Microsoft Azure SQL-Datenbank, Datenbankmigration, Kompatibilität, Migrations-Assistent für SQL Azure |
| 125 |[Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank mithilfe von SQL Server Data Tools für Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) |Microsoft Azure SQL-Datenbank, Datenbankmigration, Kompatibilität, Migrations-Assistent für SQL Azure, SSDT |
| 126 |[Beheben von Kompatibilitätsproblemen der SQL Server-Datenbank mit SQL Server Management Studio vor der Migration zur SQL-Datenbank](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) |Microsoft Azure SQL-Datenbank, Datenbankmigration, Kompatibilität, Migrations-Assistent für SQL Azure |
| 127 |[Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mit PowerShell](sql-database-export-powershell.md) |Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mit PowerShell |
| 128 |[Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank mithilfe von PowerShell](sql-database-import-powershell.md) |Importieren einer BACPAC-Datei zum Erstellen einer Azure SQL-Datenbank mithilfe von PowerShell |
| 129 |[Laden von Daten aus einer CSV-Datei in Azure SQL Data Warehouse (Flatfiles)](sql-database-load-from-csv-with-bcp.md) |Für kleinere Datenmengen wird BCP zum Importieren von Daten in Azure SQL-Datenbank verwendet. |
| 130 |[Verschieben von Datenbanken zwischen Servern und Abonnements sowie in und aus Azure](sql-database-troubleshoot-moving-data.md) |Kurzanweisungen zum Kopieren, Verschieben und Migrieren von Daten und Datenbanken in Azure SQL-Datenbank. |

## <a name="move-data-in-and-out"></a>Verschieben von Daten in und aus Azure
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 131 |[Migrieren einer SQL Server-Datenbank zu Azure SQL-Datenbank in der Cloud](sql-database-cloud-migrate.md) |Erfahren Sie, wie lokale SQL Server-Datenbanken zu Azure SQL-Datenbank in der Cloud migriert werden. Verwenden Sie Datenbankmigrationstools, um vor der Datenbankmigration die Kompatibilität zu testen. |
| 132 |[Kopieren einer Azure SQL-Datenbank](sql-database-copy.md) |Erstellen der Kopie einer Azure SQL-Datenbank |
| 133 |[Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe des Azure-Portals](sql-database-export.md) |Archivieren einer Azure SQL-Datenbank in eine BACPAC-Datei mithilfe des Azure-Portals |

## <a name="security"></a>Sicherheit
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 134 |[Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md) |Erfahren Sie, wie Sie unter Verwendung der Azure Active Directory-Authentifizierung eine Verbindung mit SQL-Datenbank herstellen. |
| 135 |[Always Encrypted: Schützen von vertraulichen Daten in SQL-Datenbank und Speichern der Verschlüsselungsschlüssel im Windows-Zertifikatspeicher](sql-database-always-encrypted.md) |Schützen Sie vertrauliche Daten in der SQL-Datenbank in wenigen Minuten. |
| 136 |[Always Encrypted – Schützen von vertraulichen Daten in SQL-Datenbank und Speichern der Verschlüsselungsschlüssel in Azure Key Vault](sql-database-always-encrypted-azure-key-vault.md) |Schützen Sie vertrauliche Daten in der SQL-Datenbank in wenigen Minuten. |
| 137 |[SQL-Datenbank – Unterstützung für kompatible Clients und IP-Endpunkt-Änderungen für die Überwachung](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md) |Erfahren Sie mehr über die Unterstützung für kompatible Clients und IP-Endpunkt-Änderungen für die Überwachung bei SQL-Datenbank. |
| 138 |[Konfigurieren von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe von PowerShell](sql-database-configure-firewall-settings-powershell.md) |Sie erfahren, wie Sie die Firewall für IP-Adressen mit Zugriff auf Azure SQL-Datenbanken konfigurieren. |
| 139 |[Konfigurieren von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe der REST-API](sql-database-configure-firewall-settings-rest.md) |Sie erfahren, wie Sie die Firewall für IP-Adressen mit Zugriff auf Azure SQL-Datenbanken konfigurieren. |
| 140 |[Konfigurieren von Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank mithilfe von T-SQL](sql-database-configure-firewall-settings-tsql.md) |Sie erfahren, wie Sie die Firewall für IP-Adressen mit Zugriff auf Azure SQL-Datenbanken konfigurieren. |
| 141 |[Erste Schritte mit der dynamischen Datenmaskierung für SQL-Datenbanken (Azure-Portal)](sql-database-dynamic-data-masking-get-started.md) |Einstieg in die dynamische Datenmaskierung für SQL-Datenbanken im Azure-Portal |
| 142 |[Erste Schritte mit der dynamischen Datenmaskierung für SQL-Datenbanken (klassisches Azure-Portal)](sql-database-dynamic-data-masking-get-started-portal.md) |Einstieg in die dynamische Datenmaskierung für SQL-Datenbanken im klassischen Azure-Portal |
| 143 |[Konfigurieren von Firewallregeln für Azure SQL-Datenbank \- Übersicht](sql-database-firewall-configure.md) |Erfahren Sie, wie eine SQL-Datenbankfirewall mit der Firewallregeln auf Server- und Datenbankebene zum Verwalten des Zugriffs konfigurieren. |
| 144 |[SQL Datenbank-Tutorial: Erstellen von SQL-Datenbankbenutzerkonten für den Zugriff auf und die Verwaltung von Datenbanken](sql-database-get-started-security.md) |Erfahren Sie, wie Sie Benutzerkonten für den Zugriff auf und die Verwaltung von Datenbanken erstellen. |
| 145 |[SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff](sql-database-manage-logins.md) |Erfahren Sie mehr über die Sicherheitsverwaltung für SQL-Datenbank und insbesondere dazu, wie der Datenbankzugriff und die Anmeldesicherheit über das Prinzipalkonto auf Serverebene verwaltet wird. |
| 146 |[Sicherheitsrichtlinien und Einschränkungen von Azure SQL-Datenbank](sql-database-security-guidelines.md) |Enthält Informationen zu den Richtlinien und Einschränkungen von Microsoft Azure SQL-Datenbank, die sich auf die Sicherheit beziehen. |
| 147 |[Erste Schritte mit der Bedrohungserkennung von SQL-Datenbank](sql-database-threat-detection-get-started.md) |Erste Schritte mit der Bedrohungserkennung der SQL-Datenbank im Azure-Portal |
| 148 |[Gewusst wie: Ausführen allgemeiner Verwaltungsaufgaben in Azure SQL-Datenbank](sql-database-troubleshoot-permissions.md) |Hier wird beschrieben, wie allgemeine Verwaltungsaufgaben in SQL-Datenbank ausgeführt werden. Sie erfahren beispielsweise, wie Sie das Administratorkennwort zurücksetzen und Zugriff gewähren bzw. entfernen. |

## <a name="manage-your-database"></a>Verwalten Ihrer Datenbank
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 149 |[Erste Schritte bei der Überwachung von SQL-Datenbank](sql-database-auditing-get-started.md) |Erste Schritte bei der Überwachung von SQL-Datenbank |
| 150 |[Konfigurieren einer Firewallregel auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md) |Hier erfahren Sie, wie Sie die Firewall für IP-Adressen mit Zugriff auf Azure SQL Server konfigurieren. |
| 151 |[Kopieren einer Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-copy-portal.md) |Erstellen der Kopie einer Azure SQL-Datenbank |
| 152 |[Verwalten von Azure SQL-Datenbanken mit Azure Automation](sql-database-manage-automation.md) |Erfahren Sie, wie der Azure Automation-Dienst zur angemessenen Verwaltung von Azure SQL-Datenbanken verwendet werden kann. |
| 153 |[Übersicht: Verwaltungstools für SQL-Datenbank](sql-database-manage-overview.md) |Vergleicht die Tools und Optionen zum Verwalten von Azure SQL-Datenbank |
| 154 |[Überwachen der Azure SQL-Datenbank mit dynamischen Verwaltungssichten](sql-database-monitoring-with-dmvs.md) |Erfahren Sie, wie Sie allgemeine Leistungsprobleme mithilfe der dynamischen Verwaltungssichten zum Überwachen von Microsoft Azure SQL-Datenbank ermitteln und diagnostizieren. |
| 155 |[Sichern der SQL-Datenbank](sql-database-security.md) |Erfahren Sie mehr über die Sicherheit von Azure SQL-Datenbank und SQL Server, einschließlich der Unterschiede zwischen einem SQL-Server in der Cloud und einem SQL-Server vor Ort hinsichtlich Authentifizierung, Autorisierung, Verbindungssicherheit, Verschlüsselung und Compliance. |

## <a name="extended-events"></a>Erweiterte Ereignisse
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 156 |[Code des Ziels "Ereignisdatei" für erweiterte Ereignisse in SQL-Datenbank](sql-database-xevent-code-event-file.md) |Stellt ein PowerShell- und ein Transact-SQL-Skript für ein zweiphasiges Codebeispiel zur Veranschaulichung des Ereignisdateiziels in einem erweiterten Ereignis in Azure SQL-Datenbank bereit. Azure Storage ist ein erforderlicher Bestandteil in diesem Szenario. |
| 157 |[Code des Ringpufferziels für erweiterte Ereignisse in SQL-Datenbank](sql-database-xevent-code-ring-buffer.md) |Zeigt ein Transact-SQL-Codebeispiel (für Azure SQL-Datenbank), das durch die Verwendung des Ringpufferziels eine einfache und schnelle Methode zum Erfassen und Ausgeben von Daten bietet. |
| 158 |[Erweiterte Ereignisse in Azure SQL-Datenbank](sql-database-xevent-db-diff-from-svr.md) |Beschreibt erweiterte Ereignisse (XEvents) in Azure SQL-Datenbank und wie sich Ereignissitzungen im Vergleich zu Microsoft SQL Server geringfügig unterscheiden. |

## <a name="geo-replication"></a>Georeplikation
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 159 |[Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit dem Azure-Portal](sql-database-geo-replication-failover-portal.md) |Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit dem Azure-Portal |
| 160 |[Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit PowerShell](sql-database-geo-replication-failover-powershell.md) |Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit PowerShell |
| 161 |[Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit Transact-SQL](sql-database-geo-replication-failover-transact-sql.md) |Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit Transact-SQL |
| 162 |[Übersicht: Aktive Georeplikation in Azure SQL-Datenbank](sql-database-geo-replication-overview.md) |Die aktive Georeplikation ermöglicht Ihnen das Einrichten von vier Replikaten Ihrer Datenbank in einem beliebigen Azure-Rechenzentrum. |
| 163 |[Konfigurieren der Georeplikation für Azure SQL-Datenbank mit dem Azure-Portal](sql-database-geo-replication-portal.md) |Konfigurieren der Georeplikation für eine Azure SQL-Datenbank mit dem Azure-Portal |
| 164 |[Konfigurieren der Georeplikation für die Azure SQL-Datenbank mit PowerShell](sql-database-geo-replication-powershell.md) |Konfigurieren der aktiven Georeplikation für die Azure SQL-Datenbank mit PowerShell |
| 165 |[Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md) |In diesem Thema werden Sicherheitsaspekte bei der Verwaltung der Sicherheit nach einer Wiederherstellung oder einem Failover einer Datenbank erläutert. |
| 166 |[Konfigurieren der Georeplikation für Azure SQL-Datenbank mit Transact-SQL](sql-database-geo-replication-transact-sql.md) |Konfigurieren der Georeplikation für Azure SQL-Datenbank mithilfe von Transact-SQL |
| 167 |[Geowiederherstellung einer Azure SQL-Datenbank aus einer georedundanten Sicherung über das Azure-Portal](sql-database-geo-restore-portal.md) |Geowiederherstellung einer Azure SQL-Datenbank aus einer georedundanten Sicherung (Azure-Portal) |
| 168 |[Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung mit PowerShell](sql-database-geo-restore-powershell.md) |Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung auf einem neuen Server |

## <a name="upgrade"></a>Upgrade
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 169 |[Verbesserte Abfrageleistung mit Kompatibilitätsgrad 130 in Azure SQL-Datenbank](sql-database-compatibility-level-query-performance-130.md) |Enthält eine Beschreibung der Schritte und Tools, mit denen ermittelt werden kann, welcher Kompatibilitätsgrad für Ihre Datenbank unter Azure SQL-Datenbank oder Microsoft SQL Server am besten geeignet ist. |
| 170 |[Verwalten von parallelen Upgrades von Cloudanwendungen mithilfe der aktiven Georeplikation von SQL-Datenbank](sql-database-manage-application-rolling-upgrade.md) |Erfahren Sie, wie Sie die Georeplikation von Azure SQL-Datenbank verwenden, um Onlineupgrades Ihrer Cloudanwendung zu unterstützen. |
| 171 |[Upgrade auf Azure SQL-Datenbank V12 mithilfe des Azure-Portals](sql-database-upgrade-server-portal.md) |Erläutert das Upgrade auf Azure SQL-Datenbank V12, einschließlich der Aktualisierung von Web- und Business-Datenbanken, sowie das Upgrade eines V11-Servers mit direkter Migration der Datenbanken in einen Pool für elastische Datenbanken mithilfe des Azure-Portals. |
| 172 |[Upgrade auf Azure SQL-Datenbank V12 mithilfe von PowerShell](sql-database-upgrade-server-powershell.md) |Erläutert das Upgrade auf Azure SQL-Datenbank V12, einschließlich der Aktualisierung von Web- und Business-Datenbanken, sowie das Upgrade eines V11-Servers mit direkter Migration der zugehörigen Datenbanken in einen Pool für elastische Datenbanken mithilfe von PowerShell. |
| 173 |[Plan and prepare to upgrade to SQL Database V12 (Planen und Vorbereiten des Upgrades auf die SQL-Datenbank V12; in englischer Sprache)](sql-database-v12-plan-prepare-upgrade.md) |Beschreibt die Vorbereitungen und Einschränkungen im Zusammenhang mit einem Upgrade auf Version V12 der Azure SQL-Datenbank. |
| 174 |[Neuerungen in SQL-Datenbank V12](sql-database-v12-whats-new.md) |Beschreibt, warum Geschäftssysteme, die eine Azure SQL-Datenbank in der Cloud verwenden von einem Upgrade auf Version V12 profitieren. |
| 175 |[Häufig gestellte Fragen zur Einstellung von Web Edition und Business Edition](sql-database-web-business-sunset-faq.md) |Erfahren Sie, wann die Web- und Business-Datenbanken in Azure SQL-Datenbank eingestellt werden und welche Features und Funktionen die neuen Dienstebenen bieten. |

## <a name="tools"></a>Tools
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 176 |[Verwalten von Azure SQL-Datenbanken mit PowerShell](sql-database-command-line-tools.md) |Verwalten von Azure SQL-Datenbank mit PowerShell. |
| 177 |[Tutorial zu SQL-Datenbank: Verbinden von Excel mit einer Azure SQL-Datenbank und Erstellen eines Berichts](sql-database-connect-excel.md) |Erfahren Sie, wie Sie Microsoft Excel mit Azure SQL-Datenbank in der Cloud verbinden. Importieren Sie Daten zwecks Berichterstellung und Untersuchung in Excel. |
| 178 |[Erstellen einer SQL-Datenbank und Ausführen gängiger Datenbankeinrichtungsaufgaben mithilfe von PowerShell-Cmdlets](sql-database-get-started-powershell.md) |Es wird beschrieben, wie Sie eine SQL-Datenbank mithilfe von PowerShell erstellen. Allgemeine Datenbankeinrichtungsaufgaben können mithilfe von PowerShell-Cmdlets verwaltet werden. |
| 179 |[Erste Schritte mit Azure SQL-Datensynchronisierung (Vorschauversion)](sql-database-get-started-sql-data-sync.md) |Dieses Lernprogramm unterstützt Sie bei den ersten Schritten mit Azure SQL-Datensynchronisierung (Vorschau). |
| 180 |[Verwalten von Azure SQL-Datenbank mit SQL Server Management Studio](sql-database-manage-azure-ssms.md) |Erfahren Sie, wie Sie SQL Server Management Studio für die Verwaltung von SQL-Datenbankservern und -Datenbanken verwenden. |
| 181 |[Verwalten von Azure SQL-Datenbanken über das Azure-Portal](sql-database-manage-portal.md) |Erfahren Sie, wie Sie das Azure-Portal verwenden, um eine relationale Datenbank mithilfe des Azure-Portals in der Cloud zu verwalten. |
| 182 |[Ändern der Dienstebene und Leistungsstufe (Tarif) einer SQL-Datenbank mit PowerShell](sql-database-scale-up-powershell.md) |In „Ändern der Dienstebene und Leistungsstufe einer Azure SQL-Datenbank“ wird beschrieben, wie Sie die Dienstebene und Leistungsstufe Ihrer SQL-Datenbank mit PowerShell zentral hoch- oder herunterstufen. Ändern des Tarifs einer Azure SQL-Datenbank mit PowerShell. |
| 183 |[Verwenden von SQL Server Management Studio in Azure RemoteApp zum Herstellen einer Verbindung mit SQL-Datenbank](sql-database-ssms-remoteapp.md) |Sie erfahren in diesem Tutorial, wie Sie SQL Server Management Studio in Azure RemoteApp für Sicherheit und Leistung beim Verbinden mit SQL-Datenbank verwenden. |

## <a name="reference"></a>Referenz
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 184 |[Verbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md) |Listet die mindestens erforderliche Versionsnummer für jeden Treiber auf, mit der Clientprogramme Verbindungen mit Azure SQL-Datenbank oder Microsoft SQL Server herstellen können. Es wird ein Link zu Versionsinformationen für die Treiber bereitgestellt, die nicht von Microsoft, sondern von der Community freigegeben werden. |
| 185 |[Ressourceneinschränkungen für Azure SQL-Datenbanken](sql-database-resource-limits.md) |Diese Seite beschreibt einige allgemeine Ressourceneinschränkungen für Azure SQL-Datenbanken. |
| 186 |[Azure SQL-Datenbank – Abweichungen bei Transact-SQL](sql-database-transact-sql-information.md) |Transact-SQL-Anweisungen, die in Azure SQL-Datenbank nicht vollständig unterstützt werden |

## <a name="miscellaneous"></a>Verschiedenes
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 187 |[Kopieren einer Azure SQL-Datenbank mithilfe von PowerShell](sql-database-copy-powershell.md) |Erstellen der Kopie einer Azure SQL-Datenbank mithilfe von PowerShell |
| 188 |[Kopieren einer Azure SQL-Datenbank mithilfe von Transact-SQL](sql-database-copy-transact-sql.md) |Erstellen der Kopie einer Azure SQL-Datenbank mithilfe von Transact-SQL |
| 189 |[Azure SQL-Datenbanken – Allgemeine Einschränkungen und Leitlinien](sql-database-general-limitations.md) |Auf dieser Seite werden einige allgemeine Einschränkungen von Azure SQL-Datenbank sowie Aspekte der Interoperabilität und Unterstützung beschrieben. |
| 190 |[Tarifempfehlungen für SQL-Datenbank](sql-database-service-tier-advisor.md) |Beim Ändern von Tarifen im Azure-Portal werden Tarifempfehlungen bereitgestellt, die den am besten geeigneten Tarif für die Ausführung des Workloads einer vorhandenen Azure SQL-Datenbank empfehlen. Tarife beschreiben die Dienstebene und die Leistungsebene einer SQL-Datenbank. |

&nbsp;

<hr/>

<a name="extras_append"></a>

## <a name="extras"></a>Extras
<a name="extra_related_articles"></a>

### <a name="related-articles-from-other-azure-services"></a>Verwandte Artikel von anderen Azure-Diensten
* [Sichern von Web-Apps in Azure App Service](../app-service-web/web-sites-backup.md)

<a name="extra_documentation_tools"></a>

### <a name="documentation-tools"></a>Dokumentationstools
* [Für Azure SQL-Datenbank angekündigte Updates](http://azure.microsoft.com/updates/?service=sql-database)
* [Durchsuchen aller Dokumentationstypen von Microsoft Azure, mit Filtern](http://azure.microsoft.com/search/documentation/)

<a name="extra_learning_path_graphics"></a>

### <a name="learning-path-graphics"></a>Lernpfadgrafiken
* [Lernpfaddiagramme für alle Microsoft Azure-Dienste](http://azure.microsoft.com/documentation/learning-paths/)
* [Lernpfad: „SQL-Datenbank-Schulung: Erlernen von Azure SQL Database“](http://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/)
* [Lernpfad: „SQL-Datenbank – Features und Tools elastischer Datenbanken verwenden“](http://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/)

<!--
AzIxAA.ExtraAppend.sql-database.txt
C:\_MainW\VS15\AzureIndexAllArticles\AzureIndexAllArticles\In-Out\In\

This bullet link is improperly disallowed by publishing automation due to presence of string 'docuXXmentation/arXXticles':
- [Search SQL Database documentation, with filters](http://azure.microsoft.com/docuXXmentation/arXXticles/?service=sql-database)
-->





<!--HONumber=Oct16_HO2-->


