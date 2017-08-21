---
title: "Lösungsauswertungstool für Cortana Intelligence | Microsoft-Dokumentation"
description: "Im Folgenden werden alle Schritte beschrieben, die Sie als Microsoft-Partner beim Veröffentlichen Ihrer Cortana Intelligence-Lösung in AppSource durchlaufen müssen."
services: machine-learning
documentationcenter: 
author: AnupamMicrosoft
manager: jhubbard
editor: cgronlun
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: anupams;v-bruham;garye
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: e79ca131bddc9e65dd24da109b9e8a08606f7bc6
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017

--- 
# <a name="cortana-intelligence-solution-evaluation-tool"></a>Lösungsauswertungstool für Cortana Intelligence
## <a name="overview"></a>Übersicht
Sie können das Lösungsauswertungstool Cortana Intelligence dafür verwenden, Ihre erweiterten Analyselösungen in Bezug auf die Konformität zu den von Microsoft empfohlenen bewährten Methoden zu bewerten. Wir freuen uns darauf, mit den Microsoft-Partner (ISVs/SIs) zusammenzuarbeiten, um qualitativ hochwertige Lösungen für Kunden, Händler und Implementierungen bieten zu können. In diesem Handbuch wird schrittweise erläutert, wie das Lösungsauswertungstool in Ihrer Lösung verwendet wird. Darüber hinaus werden die spezifischen zu prüfenden bewährten Methoden beschrieben.

## <a name="getting-started"></a>Erste Schritte
Bitte laden Sie das Lösungsauswertungstool für Cortana Intelligence [herunter](https://aka.ms/aa-evaluation-tool-download), und installieren Sie es.

Voraussetzungen:
- Windows 10: [offizielle Website für Windows 10](https://www.microsoft.com/en-us/windows)
- Azure PowerShell: [Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).

## <a name="identifying-your-app"></a>Identifizieren Ihrer App
Nach Abschluss der Installation öffnen Sie das Tool, und beginnen Sie die erste Auswertung.

![Öffnen des Auswertungstools](./media/cortana-intelligence-appsource-evaluation-tool/1-open-evaluation-tool.png)

Geben Sie die Identifizierungsinformationen zu Ihrer Lösung ein.

![Stellen Sie eine Verbindung zum Azure-Abonnement her](./media/cortana-intelligence-appsource-evaluation-tool/2-connect-azure-subscription.png)

Verbinden Sie sich mit Ihrem Azure-Abonnement, und geben Sie die Ressourcengruppe an, zu der Ihre App gehört.

![Wählen Sie die Ressourcen aus](./media/cortana-intelligence-appsource-evaluation-tool/3-select-resources.png)

Nachdem die Ressourcengruppe geladen ist, wählen Sie die in Ihrer Lösung enthaltenen Ressourcen, und geben Sie an, wie auf die Datenressourcen zugegriffen werden kann:
- Erfassung
- Nutzung
- Intern

Wir verwenden diese Informationen, um besser zu verstehen, wie Ihre Lösung unterschiedliche Komponenten nutzt, und um sicherzustellen, dass die Benutzerkomponenten mit den bewährten Methoden übereinstimmen.

### <a name="ingestion"></a>Erfassung
In diesem Fall ist unter „Erfassung“ jegliche Datenquelle zu verstehen, die zum Abrufen von Daten von außerhalb der Lösung oder von beliebigen Diensten außerhalb der Lösung zum Übertragen von Daten mithilfe von Push in die Lösung verwendet wird.

### <a name="consumption"></a>Nutzung
In diesem Fall ist unter „Verbrauch“ jegliches Dataset zu verstehen, das zum direkten oder indirekten Übertragen von Daten mithilfe von Push an die Endbenutzer verwendet wird. Beispiel:
- Die Datasets, die in direkten Abfragen von PowerBI verwendet werden.
- In WebApp abgefragte Datasets.

>[!NOTE]
Wenn eine bestimmte Ressource sowohl für die Erfassung als auch für die Nutzung verwendet wird, wählen Sie bitte **Nutzung**.

### <a name="internal"></a>Intern
Verwenden Sie diese Option für alle Datenressourcen, die nur in der internen Anwendungsverarbeitung verwendet werden.

Als Nächstes werden sie dazu aufgefordert, gültige Anmeldeinformationen für alle im vorherigen Schritt angegebenen Datenbanken zu erfassen:

![Set-Test-Voraussetzungen](./media/cortana-intelligence-appsource-evaluation-tool/4-set-test-prerequisites.png)

## <a name="solution-test-cases"></a>Testfälle für Lösungen
Das Lösungstool führt eine Sammlung automatisierter Tests für Ihre Lösung durch.

![Set-Test-Ausführung](./media/cortana-intelligence-appsource-evaluation-tool/5-set-test-execution.png)

Nach Abschluss der Tests werden Sie dazu aufgefordert, eine Erklärung oder Begründung dafür zu geben, weshalb Ihre Lösung die Anforderung nicht erfüllt.

![Geben Sie eine geschäftliche Begründung](./media/cortana-intelligence-appsource-evaluation-tool/6-provide-business-justification.png)

Wenn Ihre Lösung z.B. an Azure SQL DW veröffentlicht, verlangen Auswertungstests von Ihnen, ebenfalls an Azure Analysis Services zu veröffentlichen. 

Ihre Lösung könnte beispielsweise virtuelle IaaS-Computer verwenden und SQL Server Analysis Services statt Azure Analysis Services verwenden. Dies wäre ein akzeptabler Grund für das Versagen des Tests.
## <a name="packaging-your-evaluation-results"></a>Verpacken der Auswertungsergebnisse
Nach Abschluss der Testfälle wird Ihr Auswertungspaket in eine ZIP-Datei exportiert, und Sie werden dazu aufgefordert, Feedback zum Auswertungstool zu geben. 

Sie müssen diese ZIP-Datei mit den Testergebnissen für Microsoft freigeben, damit Ihre Lösung ausgewertet werden kann, bevor Sie die Genehmigung für die Hinzufügung zu AppSource erhalten.

![Einstufen des Auswertungstools](./media/cortana-intelligence-appsource-evaluation-tool/7-grade-evaluation-tool.png)

In dem oben genannten Abschnitt dieses Artikels werden verschiedene Funktionen des Tools beschrieben. Sehen wir uns nun die Arten der bewährten Methoden an, die dieses Tool auswertet.

## <a name="security-evaluation-considerations"></a>Sicherheitsüberlegungen für die Auswertung
### <a name="databases-should-use-azure-active-directory-authentication"></a>Die Datenbanken sollten die Azure Active Directory-Authentifizierung verwenden
Bei allen Ressourcen von Azure SQL und Azure SQL DW in der Lösung sollte die Azure Active Directory-Authentifizierung (AAD) aktiviert werden. AAD bietet die Verwaltung aller Identitäten und Rollen an einer zentralen Stelle.

| Weitere Informationen über | Siehe dieser Artikel |
| --- | --- |
| AAD mit der SQL-Datenbank und mit SQL Data Warehouse | [Verwenden der Azure Active Directory-Authentifizierung für die Authentifizierung mit SQL-Datenbank oder SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication) |
| Konfigurieren und Verwalten von AAD | [Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit SQL-Datenbank oder SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication-configure) |
| Azure WebApps-Authentifizierung | [Authentifizierung und Autorisierung in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/app-service-authentication-overview) |
| Konfigurieren von WebApps mit AAD | [So konfigurieren Sie Ihre App Service-Anwendung zum Verwenden der Azure Active Directory-Anmeldung](https://docs.microsoft.com/en-us/azure/app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication)|

### <a name="datasets-accessible-to-end-users-should-support-role-based-access-control"></a>Datasets, auf die die Endbenutzer zugreifen können, müssen die rollenbasierte Zugriffssteuerung unterstützen
Beim Ausführung des Auswertungstools werden Sie dazu aufgefordert, alle Reporting- und Veröffentlichungsressourcen anzugeben. Es wird angenommen, dass diese Ressourcen für den Endbenutzerzugriff gedacht sind und nicht für den Entwicklerzugriff. Diese Ressourcen müssen eine rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) bieten, damit sichergestellt werden kann, dass nur die Endbenutzer Zugriff auf die autorisierten Daten erhalten.

Dies bedeutet, dass jede der folgenden Azure-Ressourcen mit RBAC konfiguriert werden kann und als akzeptable angenommen wird:
- Zu Absicherung von HDInsight siehe [Einführung in die Hadoop-Sicherheit mit in die Domäne eingebundenen HDInsight-Clustern](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-domain-joined-introduction)
- Weitere Informationen zu Azure SQL finden Sie unter [AAD-Authentifizierung mit Azure SQL]( https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication)
- Weitere Informationen zu Azure Analysis Services finden Sie unter [Verwalten von Datenbankrollen und Benutzern für Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-database-users)
- Azure SQL Data Warehouse (Hinweis: Da SQL DW keine RBAC unterstützt, wird es für den direkten Endbenutzerzugriff nicht empfohlen).

Wenn Sie einen anderen Ressourcentyp verwenden und dieser die RBAC unterstützt, geben Sie dies bitte in der Begründung für den Testfall an.

### <a name="azure-data-lake-store-should-use-at-rest-encryption"></a>Azure Data Lake Store muss Verschlüsselung für ruhende Daten bieten
Azure Data Lake Store (ADLS) unterstützt die Verschlüsselung ruhender Daten standardmäßig und verwendet dabei ADLS-verwaltete Verschlüsselungsschlüssel. Sie können die Verschlüsselung auch mit Azure Key Vault konfigurieren.

Weitere Informationen zum Festlegen von ADLS-Verschlüsselungseinstellungen finden Sie in [Erstellen eines Azure Data Lake Store-Kontos](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal#create-an-azure-data-lake-store-account).

### <a name="azure-sql-and-azure-sql-data-warehouse-should-use-encryption"></a>Azure SQL und Azure SQL Data Warehouse müssen Verschlüsselung verwenden
Azure SQL und Azure SQL DW unterstützen beide Transparent Data Encryption (TDE). Diese bietet Ver- und Entschlüsselung von Daten und Protokolldateien in Echtzeit.

| Weitere Informationen über | Siehe dieser Artikel |
| --- | --- |
| Transparent Data Encryption (TDE) | [Transparente Datenverschlüsselung (TDE)](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-tde) |
| Azure SQL Data Warehouse mit TDE | [SQL Data Warehouse-Verschlüsselung mit TDE TSQL](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-encryption-tde-tsql) |
| Konfigurieren von Azure SQL mit TDE | [Transparent Data Encryption mit Azure SQL-Datenbank](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database) |
| Konfigurieren von Azure SQL mit „Always Encrypted“ | [SQL Database Always Encrypted – Azure Key Vault](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-always-encrypted-azure-key-vault)|

Zusätzlich zu TDE unterstützt Azure SQL auch die neue Datenverschlüsselungstechnologie „Always Encrypted“. Mit dieser werden die Daten nicht nur im Ruhezustand und bei Datenübertragungen zwischen Client und Server verschlüsselt, sondern auch während ihrer Verwendung beim Ausführen von Befehlen auf dem Server.

### <a name="any-virtual-machines-must-be-deployed-from-the-azure-marketplace"></a>Alle virtuellen Computer müssen vom Azure Marketplace bereitgestellt werden
Um in AppSource ein konsistentes Sicherheitsniveau bieten zu können, können nur zertifizierte und im Azure Marketplace veröffentlichte virtuelle Computer als Teil der Cortana Intelligence-Lösung bereitgestellt werden.

Die aktuelle Liste der Azure Marketplace-Images kann im [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute) durchsucht werden.

Weitere Informationen über das Veröffentlichen von virtuellen Computern im Azure Marketplace finden Sie in der [Anleitung zum Erstellen eines VM-Images für Azure Marketplace](https://docs.microsoft.com/en-us/azure/marketplace-publishing/marketplace-publishing-vm-image-creation).

## <a name="scalability-evaluation-considerations"></a>Überlegungen zur Skalierbarkeitsauswertung
### <a name="cortana-intelligence-solutions-should-include-a-scalable-big-data-platform"></a>Cortana Intelligence-Lösungen müssen eine skalierbare Big Data-Plattform umfassen.
Cortana Intelligence-Lösungen müssen für sehr große Datengrößen skalierbar sein. In Azure bedeutet dies, dass eine der beiden Datenplattformen des Petabytebereichs enthalten sein muss:
- Azure Data Lake Store
- Azure SQL Data Warehouse

Wenn Ihre Lösung keine Unterstützung für diese Datengrößen benötigt oder Sie eine alternative Datenplattform verwenden, erläutern Sie dies in der Testfallbegründung.
### <a name="cortana-intelligence-solutions-should-include-dedicated-ingestion-data-environments"></a>Cortana Intelligence-Lösungen müssen spezifische Datenumgebungen für die Erfassung beinhalten.
Cortana Intelligence-Lösungen sollten es im Allgemeinen vermeiden, Daten direkt in relationale Datenquellen einzufügen. Stattdessen sollten die Daten in einer unstrukturierten Umgebung gespeichert werden. Hierfür sollten in Azure Data Factory idempotente Einfügungen/Aktualisierungen in beliebigen relationalen Speichern verwendet werden.

Weitere Informationen zum Kopieren von Daten mit Azure Data Factory finden Sie in [Tutorial: Erstellen einer Pipeline mit Kopieraktivität mithilfe von Visual Studio](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-copy-activity-tutorial-using-visual-studio).

### <a name="azure-sql-data-warehouse-should-use-polybase-for-data-ingestion"></a>In Azure SQL Data Warehouse muss für die Datenerfassung PolyBase verwendet werden
Azure SQL DW unterstützt PolyBase für das hochgradig skalierbare parallele Einfügen von Daten. Mit PolyBase können Sie Azure SQL DW dazu verwenden, Abfragen an externe Datasets abzusetzen, die entweder in Azure Blob Storage oder in Azure Data Lake Store gespeichert sind. Im Vergleich zu alternativen Massenaktualisierungsverfahren ist dies performanter.

Die ersten Schritte mit PolyBase und Azure SQL DW finden Sie in [Laden von Daten mit PolyBase in SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-get-started-load-with-polybase).

Weitere Informationen zu bewährten Methoden mit PolyBase und Azure SQL Data Warehouse finden Sie unter [Laden von Daten mit PolyBase in SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide).

## <a name="availability-evaluation-considerations"></a>Überlegungen zur Verfügbarkeitsauswertung

### <a name="datasets-accessible-to-end-users-should-support-a-large-volume-of-concurrent-users"></a>Datasets, auf die die Endbenutzer zugreifen können, müssen eine große Anzahl von parallel aktiven Benutzern unterstützen können
Beim Ausführung des Auswertungstools werden Sie dazu aufgefordert, alle Reporting- und Veröffentlichungsressourcen anzugeben. Es wird angenommen, dass diese Ressourcen für den Endbenutzerzugriff gedacht sind und nicht für den Entwicklerzugriff. Diese Ressourcen müssen eine mittelgroße Anzahl von parallel aktiven Benutzern unterstützen können.

Insbesondere darf Azure SQL Data Warehouse NICHT die einzige für die Endbenutzer verfügbare Datenquelle sein. Wenn Azure SQL DW als Ressource für Hauptbenutzer angeboten wird, sollte für die typischen Benutzer Azure Analysis Services zur Verfügung gestellt werden.

Weitere Informationen zu Azure SQL DW-Parallelitätseinschränkungen finden Sie unter [Parallelitäts- und Workloadverwaltung in SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-develop-concurrency).

Weitere Informationen zu Azure Analysis Services, finden Sie in der [Übersicht über Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview).

### <a name="azure-sql-resources-should-have-a-read-only-replica-for-failover"></a>Azure SQL-Ressourcen müssen über ein schreibgeschütztes Failoverreplikat verfügen
Azure SQL-Datenbanken unterstützen die Georeplikation in einer sekundären Instanz. Diese Instanz kann dann als Failoverinstanz verwendet werden, um Anwendungen mit hoher Verfügbarkeit bereitstellen zu können.

Weitere Informationen zur Georeplikation für Azure SQL-Datenbanken finden Sie in der [Übersicht: Failovergruppen und aktive Georeplikation](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-overview).

Anweisungen zum Konfigurieren der Georeplikation für SQL Azure finden Sie in [Konfigurieren der aktiven Georeplikation für Azure SQL-Datenbank mit Transact-SQL](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-transact-sql).

### <a name="azure-sql-data-warehouse-should-have-geo-redundant-backups-enabled"></a>In Azure SQL Data Warehouse müssen georedundante Sicherungen aktiviert sein
Azure SQL Data Warehouse unterstützt tägliche Sicherungen im georedundanten Speicher. Durch diese Georeplikation wird sichergestellt, dass das Data Warehouse auch dann wiederhergestellt werden kann, wenn kein Zugriff auf die in der primären Region gespeicherten Momentaufnahmen besteht. Diese Funktion ist standardmäßig aktiviert und darf bei Cortana Intelligence-Lösungen nicht deaktiviert werden.

Weitere Informationen zu Azure SQL DW-Sicherungen und Wiederherstellung finden Sie in [SQL Data Warehouse-Sicherungen](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-backups).

### <a name="virtual-machines-should-be-configured-with-availability-sets"></a>Bei virtuellen Computern müssen Verfügbarkeitsgruppen konfiguriert sein
Virtuelle Azure-Computer müssen in Verfügbarkeitsgruppen konfiguriert werden, um die Auswirkung geplanter und ungeplanter Wartungsereignisse zu minimieren.

Weitere Informationen zur Verfügbarkeit von virtuellen Azure-Computern finden Sie unter [Verwalten der Verfügbarkeit virtueller Windows-Computer in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability).

## <a name="other-evaluation-considerations"></a>Weitere Auswertungsaspekte
### <a name="cortana-intelligence-apps-should-use-a-centralized-tool-for-data-orchestration"></a>Cortana Intelligence-Apps müssen ein zentrales Tool für die Datenorchestrierung verwenden
Durch das Verwenden eines einzigen Tools für das Verwalten und Planen von Datenbewegungen und -umwandlungen wird die Konsistenz der unternehmenskritischen Daten gewährleistet. In Bezug auf die Wiederholungslogik, die Abhängigkeitsverwaltung, Warnungen/Protokollierung bietet dies eine klare Logik. Für die Datenorchestrierung in Azure empfehlen wir [Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-introduction).

Wenn Sie nicht Azure Data Factory sondern ein anderes Tool für die Datenorchestrierung verwenden, beschreiben Sie bitte, welches Tool oder welche Tools Sie verwenden.
### <a name="azure-machine-learning-models-should-be-retrained-using-azure-data-factory"></a>Azure Machine Learning-Modellen müssen mit Azure Data Factory erneut trainiert werden
Azure Machine Learning (AzureML) bietet unkomplizierte Tools für das Erstellen und Bereitstellen von Pipelines für Vorhersagemodellierungen und Machine Learning. Allerdings ist es wichtig, dass die Produktionsbereitstellungen dieser AzureML-Modelle nicht auf einem einzigen festen Dataset basieren, sondern sich an die neuen Dynamiken der Echtzeitphänomene anpassen.

Weitere Informationen zum Erstellen von Webdiensten für erneutes Training in Azure ML finden Sie im Artikel [Programmgesteuertes erneutes Trainieren von Machine Learning-Modellen](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-retrain-models-programmatically).

Weitere Informationen zum Automatisieren des Modelltrainingsprozesses mit Azure Data Factory finden Sie im Artikel [Aktualisieren von Azure Machine Learning-Modellen mithilfe der Ressourcenaktualisierungsaktivität](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-azure-ml-update-resource-activity).

## <a name="existing-documentation"></a>Vorhandene Dokumentation
[Microsoft Azure Certified zum Erweitern Ihres Business in der Cloud](https://azure.microsoft.com/en-us/marketplace/programs/certified/)

[Microsoft Azure Certified für Cortana Intelligence](https://azure.microsoft.com/en-us/marketplace/programs/certified/cortana/)


