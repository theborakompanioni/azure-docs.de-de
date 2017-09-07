---
title: "Ermitteln, Identifizieren und Klassifizieren von persönlichen Daten in Microsoft Azure | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie nach Daten suchen und Daten klassifizieren, ermitteln sowie identifizieren.
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: 953df2f0dbbccc153b0a4206c9c0b5f5ea9a51a7
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="discover-identify-and-classify-personal-data-in-microsoft-azure"></a>Ermitteln, Identifizieren und Klassifizieren von persönlichen Daten in Microsoft Azure

Dieser Artikel bietet einen Leitfaden für das Ermitteln, Identifizieren und Klassifizieren von persönlichen Daten in mehreren Azure-Tools und -Diensten, einschließlich der Verwendung von Azure Data Catalog, Azure Active Directory, SQL-Datenbank, Power Query für Hadoop-Cluster in Azure HDInsight, Azure Information Protection, Azure Search und SQL-Abfragen für Azure Cosmos DB.

## <a name="scenario-problem-statement-and-goal"></a>Szenario, Problembeschreibung und Ziel

Ein in den USA ansässiges Sportunternehmen sammelt eine Vielzahl persönlicher und anderer Daten. Zu diesen Daten zählen Kunden- und Mitarbeiterdaten. Das Unternehmen verwaltet die Daten in mehreren Datenbanken und speichert sie an verschiedenen Speicherorten in seiner Azure-Umgebung. Neben dem Verkauf von Sportgeräten richtet das Unternehmen auch Elite-Sportveranstaltungen auf der ganzen Welt (einschließlich der EU) aus und verwaltet die Registrierung für diese Veranstaltungen. In manchen Fällen enthalten die gesammelten Kundendaten medizinische Informationen.

Da das Unternehmen jedes Jahr viele internationale Fahrradtouren veranstaltet und rund um den Globus Personal beschäftigt, sind einige der Datasets sehr groß. Das Unternehmen verfügt auch über eigene Anwendungen, die von Entwicklern erstellt wurden und sowohl von Kunden als auch von Mitarbeitern verwendet werden.

Das Unternehmen möchte die folgenden Probleme lösen:

- Persönliche Kunden- und Mitarbeiterdaten müssen klassifiziert/von den anderen vom Unternehmen gesammelten Daten unterschieden werden, um den ordnungsgemäßen Zugriff und die Sicherheit zu gewährleisten.
- Der Datenadministrator muss den Speicherort von persönlichen Kundendaten problemlos in verschiedenen Bereichen der Azure-Umgebung ermitteln können.
- Persönliche Kunden- und Mitarbeiterdaten, die in Dokumenten und der E-Mail-Kommunikation freigegeben werden, müssen mit Bezeichnungen versehen werden, um einen angemessenen Schutz dieser Daten sicherzustellen.
- Die App-Entwickler des Unternehmens benötigen eine Möglichkeit zur mühelosen Suche nach persönlichen Kunden- und Mitarbeiterdaten in ihren Web-Apps und mobilen Apps.
- Entwickler müssen zudem persönlichen Daten aus der Dokumentdatenbank abfragen.

### <a name="company-goals"></a>Unternehmensziele

- Alle persönlichen Kunden- und Mitarbeiterdaten müssen in Azure Data Catalog markiert/kommentiert werden, damit sie leicht gefunden werden können. Im Idealfall werden persönliche Kunden- und Mitarbeiterdaten getrennt voneinander markiert/kommentiert.
- In Azure Active Directory gespeicherte persönliche Daten aus Benutzerprofilen von Kunden und Mitarbeitern und Arbeitsinformationen müssen leicht zu finden sein.
- Für persönliche Daten, die in mehreren SQL-Datenbanken gespeichert sind, muss eine einfache Abfrage möglich sein. 
- Einige der großen Datasets des Unternehmens werden über Azure HDInsight verwaltet und in Hadoop gespeichert. Sie müssen in Excel importiert werden, damit persönliche Daten aus ihnen abgefragt werden können.
- In Dokumenten und der E-Mail-Kommunikation freigegebene persönliche Daten müssen klassifiziert, mit Bezeichnungen versehen und mit Azure Information Protection gesichert werden.
- Die App-Entwickler des Unternehmens müssen in der Lage sein, persönliche Kunden- und Mitarbeiterdaten in den von ihnen erstellten Apps zu ermitteln. Zu diesem Zweck können sie Azure Search verwenden.
- Entwickler müssen persönliche Daten in ihrer Dokumentdatenbank finden können.

## <a name="azure-active-directory-data-discovery"></a>Azure Active Directory: Datenermittlung

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft. Sie können im [Azure-Portal](https://portal.azure.com/) nach Benutzerprofilen von Kunden und Mitarbeitern sowie Arbeitsinformationen von Benutzern in Ihrer [Azure Active Directory](https://azure.microsoft.com/services/active-directory/)-Umgebung (AAD) suchen, die persönlichen Daten enthalten.

Dies ist besonders hilfreich, wenn Sie nach persönlichen Daten für einen bestimmten Benutzer suchen oder diese ändern möchten. Sie können auch Benutzerprofil- und Arbeitsinformationen hinzufügen oder ändern. Dazu müssen Sie sich mit einem Konto anmelden, das als globaler Administrator für das Verzeichnis konfiguriert ist.

### <a name="how-do-i-locate-or-view-user-profile-and-work-information"></a>Gewusst wie: Suchen und Anzeigen von Benutzerprofil- und Arbeitsinformationen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.

2. Wählen Sie **Weitere Dienste** aus, geben Sie **Benutzer und Gruppen** in das Textfeld ein, und drücken Sie die **EINGABETASTE** .

   ![Gewusst wie: Suchen nach Benutzerprofil- und Arbeitsinformationen](media/how-to-discover-classify-personal-data-azure/user-profile.png)

3. Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Benutzer** aus.

  ![Öffnen von „Benutzer und Gruppen“](media/how-to-discover-classify-personal-data-azure/users-groups.png)

4. Wählen Sie auf dem Blatt **Benutzer und Gruppen - Benutzer** einen Benutzer in der Liste aus, und klicken Sie dann auf dem Blatt für den ausgewählten Benutzer auf **Profil**, um Benutzerprofilinformationen anzuzeigen, die möglicherweise persönliche Daten enthalten.

  ![Benutzer auswählen](media/how-to-discover-classify-personal-data-azure/select-user.png)

5. Sie können bei Bedarf Benutzerprofilinformationen hinzufügen oder ändern und anschließend auf der Befehlsleiste auf **Speichern** klicken.
6. Klicken Sie auf dem Blatt für den ausgewählten Benutzer auf **Arbeitsinformationen**, um Arbeitsinformationen für den Benutzer anzuzeigen, die möglicherweise persönliche Daten enthalten.

 ![Anzeigen von Arbeitsinformationen](media/how-to-discover-classify-personal-data-azure/work-info.png)

7. Sie können bei Bedarf Arbeitsinformationen für den Benutzer hinzufügen oder ändern und anschließend auf der Befehlsleiste auf **Speichern** klicken.

## <a name="azure-sql-database-data-discovery"></a>Azure SQL-Datenbank: Datenermittlung

[Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/?v=16.50) ist eine Clouddatenbank, die Entwickler bei der Erstellung und Verwaltung von Anwendungen unterstützt. In [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/?v=16.50) können Sie mit standardmäßigen SQL-Abfragen nach persönlichen Daten suchen. Die elastische Azure SQL-Abfrage (Vorschau) ermöglicht Benutzern die Durchführung von datenbankübergreifenden Abfragen.

In einem ausführlichen Tutorial zu [SQL-Datenbank](../sql-database/sql-database-technical-overview.md) werden viele Aspekte der Verwendung einer SQL-Datenbank erläutert, unter anderem die Erstellung und Ausführung von Datenabfragen. Im Folgenden finden Sie eine Zusammenfassung der Informationen im Tutorial mit Links zu bestimmten Abschnitten.

### <a name="how-do-i-build-a-sql-database"></a>Gewusst wie: Erstellen einer SQL-Datenbank

Zum Erstellen einer SQL-Datenbank stehen drei Methoden zur Auswahl:

- Eine Azure SQL-Datenbank kann im [Azure-Portal](https://portal.azure.com/) erstellt werden. Im Tutorial verwenden Sie eine Liste bestimmter Compute- und Speicherressourcen in einer Ressourcengruppe und auf einem logischen Server. Sie verwenden Beispieldaten von einem fiktiven Unternehmen namens AdventureWorks. Außerdem erstellen Sie eine Firewallregel auf Serverebene. Ausführliche Anweisungen und Informationen dazu finden Sie im Tutorial [Erstellen einer Azure SQL-Datenbank im Azure-Portal](../sql-database/sql-database-get-started-portal.md).

  ![Erstellen einer Azure SQL-Datenbank](media/how-to-discover-classify-personal-data-azure/create-database.png)
- Eine SQL-Datenbank kann auch in der [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/)-CLI erstellt werden, einem browserbasierten Befehlszeilentool. Das Tool ist im Azure-Portal verfügbar und kann dort direkt ausgeführt werden. In diesem Tutorial führen Sie folgende Aufgaben aus: Sie starten das Tool, definieren Skriptvariablen, erstellen eine Ressourcengruppe sowie einen logischen Server und konfigurieren eine Serverfirewallregel. Anschließend erstellen Sie eine Datenbank mit Beispieldaten. Ausführliche Anweisungen und Informationen zu dieser Methode der Datenbankerstellung finden Sie im Tutorial [Erstellen einer einzelnen Azure SQL-Datenbank mithilfe der Azure CLI](../sql-database/sql-database-get-started-cli.md).

  ![CLIT-Tutorial](media/how-to-discover-classify-personal-data-azure/cli-tutorial.png)

>[!NOTE]
Die Azure-Befehlszeilenschnittstelle (CLI) wird häufig von Linux-Administratoren und -Entwicklern verwendet. Einige Benutzer finden sie einfacher und intuitiver als PowerShell (die dritte Option).

- Die dritte und letzte Option zur Erstellung einer SQL-Datenbank ist die Verwendung von PowerShell, einem Befehlszeilen-/Skripttool, das zum Erstellen und Verwalten von Azure- und anderen Ressourcen verwendet wird. In diesem Tutorial führen Sie folgende Aufgaben aus: Sie starten das Tool, definieren Skriptvariablen, erstellen eine Ressourcengruppe sowie einen logischen Server und konfigurieren eine Serverfirewallregel. Anschließend erstellen Sie eine Datenbank mit Beispieldaten.

Für dieses Tutorial ist das Azure PowerShell-Modul Version 4.0 oder höher erforderlich. Führen Sie „Get-Module -ListAvailable AzureRM“ aus, um Ihre Version zu ermitteln. Falls Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie im Artikel zum Installieren des Azure PowerShell-Moduls weitere Informationen.

```PowerShell
New-AzureRmSQLDatabase -ResourceGroupName $resourcegroupname `
-ServerName $servername `
-DatabaseName $databasename `
-RequestedServiceObjectiveName "s0"
```

Ausführliche Anweisungen und Informationen zu dieser Methode der Datenbankerstellung finden Sie im Tutorial [Erstellen einer einzelnen Azure SQL-Datenbank mithilfe von PowerShell](../sql-database/sql-database-get-started-powershell.md).

>[!Note]
Windows-Administratoren verwenden meist PowerShell, manche von ihnen bevorzugen jedoch die Azure-Befehlszeilenschnittstelle.

### <a name="how-do-i-search-for-personal-data-in-sql-database-in-the-azure-portal"></a>Gewusst wie: Suchen nach persönlichen Daten in SQL-Datenbank im Azure-Portal**

Sie können das integrierte Abfrage-Editor-Tool im Azure-Portal verwenden, um nach persönlichen Daten zu suchen. Sie melden sich mit Ihrem SQL Server-Administratorbenutzernamen und -kennwort beim Tool an und geben dann eine Abfrage ein.

  ![Suchen in einer SQL-Datenbank im Portal](media/how-to-discover-classify-personal-data-azure/search-sql-portal.png)

Schritt 5 des Tutorials zeigt eine Beispielabfrage im Abfrage-Editor-Bereich, im Mittelpunkt stehen hierbei jedoch nicht persönliche oder sensible Informationen (zudem werden Daten aus zwei Tabellen kombiniert und Aliase für die Quellspalte im zurückgegebenen Dataset erstellt). Der folgende Screenshot zeigt die Abfrage aus Schritt 5 und den zurückgegebenen Ergebnisbereich:

  ![Abfrage-Editor](media/how-to-discover-classify-personal-data-azure/query-editor.png)

Im Fall einer Datenbank mit dem Namen „MyTable“ würde eine Beispielabfrage für persönliche Informationen, die den Namen, die Sozialversicherungsnummer und die ID-Nummer enthält, wie folgt aussehen:

„SELECT Name, SSN, ID number FROM MyTable“

Sie würden die Abfrage ausführen und die Ergebnisse dann im Bereich **Ergebnisse** sehen.

Weitere Informationen zum Abfragen einer SQL-­Datenbank im Azure-Portal finden Sie im Abschnitt [Abfragen der SQL-Datenbank](../sql-database/sql-database-get-started-portal.md) des Tutorials.

### <a name="how-do-i-search-for-data-across-multiple-databases"></a>Gewusst wie: Suchen nach Daten in mehreren Datenbanken

Eine elastische SQL-Abfrage (Vorschau) ermöglicht es Ihnen, mehrere datenbankübergreifende Abfragen auszuführen, für die ein einziges Ergebnis zurückgegeben wird. Die [Übersicht über das Tutorial](../sql-database/sql-database-elastic-query-overview.md) enthält eine detaillierte Beschreibung der Szenarien und erläutert den Unterschied zwischen der vertikalen und horizontalen Datenbankpartitionierung. Die horizontale Partitionierung wird als „Sharding“ bezeichnet.

  ![Vertikale Partitionierung](media/how-to-discover-classify-personal-data-azure/vertical-partition.png)

  ![Horizontale Partitionierung](media/how-to-discover-classify-personal-data-azure/horizontal.png)

Lesen Sie zum Einstieg in die Thematik die [Übersicht über elastische Abfragen in Azure SQL-Datenbank (Vorschau)](../sql-database/sql-database-elastic-query-overview.md).

#### <a name="power-query-for-importing-azure-hdinsight-hadoop-clusters-data-discovery-for-large-data-sets"></a>Power Query (zum Importieren von Azure HDInsight Hadoop-Clustern): Datenermittlung für große Datasets

Hadoop ist ein Open Source-Speicher- und -Verarbeitungsdienst von Apache für große Datasets, die in Hadoop-Clustern analysiert und gespeichert werden. [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) ermöglicht Benutzern die Verwendung von Hadoop-Clustern in Azure. Power Query ist ein Excel-Add-In, das Benutzern u. a. die Ermittlung von Daten aus verschiedenen Quellen ermöglicht.

Persönliche Daten, die Hadoop-Clustern in Azure HDInsight zugeordnet sind, können mit Power Query in Excel importiert werden. Nachdem die Daten in Excel importiert wurden, können Sie sie mithilfe einer Abfrage identifizieren.

#### <a name="how-do-i-use-excel-power-query-to-import-hadoop-clusters-in-azure-hdinsight-into-excel"></a>Gewusst wie: Verwenden von Microsoft Power Query für Excel zum Importieren von Hadoop-Clustern in Azure HDInsight in Excel

Ein HDInsight-Tutorial begleitet Sie durch den gesamten Prozess. Das Tutorial erläutert die Voraussetzungen und enthält einen Link zu einem Tutorial zu den [ersten Schritten mit Azure HDInsight](../hdinsight/hdinsight-hadoop-linux-tutorial-get-started.md). In den Anweisungen werden Excel 2016 sowie 2013 und 2010 behandelt (die Schritte für die älteren Excel-Versionen unterscheiden sich geringfügig). Falls Sie nicht über das Microsoft Power Query für Excel-Add-In verfügen, erfahren Sie im Tutorial, wie Sie es herunterladen können. Sie beginnen das Tutorial in Excel und benötigen ein Azure Blob Storage-Konto, das Ihrem Cluster zugeordnet ist.

  ![Abfrage in Excel](media/how-to-discover-classify-personal-data-azure/excel.png)

Ausführliche Anweisungen und Informationen hierzu finden Sie im Tutorial [Verbinden von Excel mit Hadoop mithilfe von Power Query](../hdinsight/hdinsight-connect-excel-power-query.md).

Quelle: [Verbinden von Excel mit Hadoop mithilfe von Power Query](../hdinsight/hdinsight-connect-excel-power-query.md)

## <a name="azure-information-protection-personal-data-classification-for-documents-and-email"></a>Azure Information Protection: Klassifizierung von persönlichen Daten für Dokumente und E-Mails

[Azure Information Protection](https://www.microsoft.com/cloud-platform/azure-information-protection) kann Azure-Kunden dabei helfen, Bezeichnungen zum Klassifizieren und Schützen intern oder extern freigegebener Dokumente und E-Mail-Kommunikationen anzuwenden. Einige dieser Elemente können persönliche Kunden- oder Mitarbeiterdaten enthalten. Regeln und Bedingungen können automatisch oder manuell von Administratoren oder Benutzern definiert werden. Wenn ein Benutzer ein Dokument speichert, das Kreditkarteninformationen enthält, würde er beispielsweise eine vom Administrator konfigurierte Bezeichnungsempfehlung sehen.

### <a name="how-do-i-try-it"></a>Gewusst wie: Ausprobieren

Wenn Sie ausprobieren möchten, ob sich Azure Information Protection für Ihre Organisation eignet, besuchen Sie das [Schnellstart-Tutorial für Azure Information Protection](https://docs.microsoft.com/information-protection/get-started/infoprotect-quick-start-tutorial). Das Tutorial enthält detaillierte Anweisungen für fünf grundlegende Schritte – von der Installation über die Konfiguration von Richtlinien bis hin zur praktischen Verwendung der Klassifizierung, Bezeichnung und Freigabe – und nimmt in der Regel weniger als eine halbe Stunde in Anspruch.

### <a name="how-do-i-deploy-it"></a>Gewusst wie: Bereitstellen

Wenn Sie Azure Information Protection für Ihre Organisation bereitstellen möchten, finden Sie in der [Roadmap für die Bereitstellung von Azure Information Protection](https://docs.microsoft.com/information-protection/plan-design/deployment-roadmap) für Klassifizierung, Bezeichnung und Schutz weitere Informationen.

### <a name="is-there-anything-else-i-should-know"></a>Gibt es noch etwas, das ich wissen sollte?

Im Blogbeitrag [Azure Information Protection: Ready, set, protect!](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/21/azure-information-protection-ready-set-protect/) (Azure Information Protection: Vorbereiten, Konfigurieren, Schützen) finden Sie
ergänzende Informationen zur Einrichtung. Weitere Informationen zu Azure Information Protection finden Sie auch unter den unten stehen Links.

## <a name="azure-search-data-discovery-for-developer-apps"></a>Azure Search: Datenermittlung für Entwickler-Apps

[Azure Search](https://azure.microsoft.com/services/search/) ist eine Cloudsuchlösung für Entwickler und bietet umfassende Funktionen zur Datensuche für Ihre Anwendungen. Mit Azure Search können Sie in benutzerdefinierten Indizes nach Daten suchen, die aus Azure Cosmo DB, Azure SQL-Datenbank, Azure Blob Storage, Azure Table Storage oder benutzerdefinierten JSON-Kundendaten stammen. Sie können mithilfe der Azure Search-REST-API auch Lucene-Abfragen erstellen, um nach persönlichen Datentypen oder den persönlichen Daten bestimmter Personen zu suchen. Zu den verfügbaren Funktionen zählen Volltextsuche, einfache Abfragesyntax und Lucene-Abfragesyntax. 

## <a name="how-do-i-use-sql-to-query-data"></a>Gewusst wie: Verwenden von SQL zum Abfragen von Daten

Im Tutorial [Azure Cosmos DB: Wie werden Abfragen mit SQL durchgeführt?](../cosmos-db/tutorial-query-documentdb.md) werden die Grundlagen erläutert. Das Tutorial enthält ein Beispieldokument sowie zwei SQL-Beispielabfragen und -ergebnisse.

Ausführlichere Anweisungen zum Erstellen von SQL-Abfragen finden Sie unter [SQL-Abfragen für Azure Cosmos DB-DocumentDB-API](../cosmos-db/documentdb-sql-query.md).

Falls Sie noch nicht mit Azure Cosmos DB vertraut sind und mehr über die Erstellung einer Datenbank sowie das Hinzufügen einer Sammlung und von Daten erfahren möchten, besuchen Sie das Schnellstart-Tutorial [Azure Cosmos DB: Erstellen einer Web-App mit einer DocumentDB-API mit .NET und dem Azure-Portal](../cosmos-db/create-documentdb-dotnet.md). Wenn Sie eine andere Programmiersprache als .NET verwenden möchten (z. B. Java oder Python), wählen Sie auf der Website einfach Ihre bevorzugte Sprache aus.

## <a name="next-steps"></a>Nächste Schritte

[Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/?v=16.50)

[Was ist SQL-Datenbank?](../sql-database/sql-database-technical-overview.md)

[SQL-Datenbank-Abfrage-Editor, verfügbar im Azure-Portal] ((https://azure.microsoft.com/blog/t-sql-query-editor-in-browser-azure-portal/)

[Was ist Azure Information Protection?](https://docs.microsoft.com/information-protection/understand-explore/what-is-information-protection)

[Was ist Azure Rights Management?](https://docs.microsoft.com/information-protection/understand-explore/what-is-azure-rms)

[Azure Information Protection: Ready, set, protect!](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/21/azure-information-protection-ready-set-protect/) (Azure Information Protection: Vorbereiten, Konfigurieren, Schützen)

