---
title: Verwalten horizontal hochskalierter Clouddatenbanken | Microsoft Docs
description: "Veranschaulicht den Dienst für elastische Datenbankaufträge"
metakeywords: azure sql database elastic databases
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 6fa47cf2-1162-4534-a206-6e2d95b78580
ms.service: sql-database
ms.custom: multiple databases
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: 0d95f9f0e0c5b69aed6ba74a2488e46540589c00


---
# <a name="managing-scaled-out-cloud-databases"></a>Verwalten horizontal hochskalierter Clouddatenbanken
Für die Verwaltung horizontal hochskalierter Sharddatenbanken ermöglicht Ihnen das Feature **Auftrag für die elastische Datenbank** (Vorschau) das zuverlässige Ausführen eines Transact-SQL-Skripts (T-SQL) für eine Gruppe von Datenbanken. Dies schließt Folgendes ein:

* Eine benutzerdefinierte Sammlung von Datenbanken (siehe Erläuterung weiter unten)
* Alle Datenbanken in einem [Pool für elastische Datenbanken](sql-database-elastic-pool.md)
* Eine (mit der [Clientbibliothek für elastische Datenbanken](sql-database-elastic-database-client-library.md)erstellte) Shard-Gruppe 

## <a name="documentation"></a>Dokumentation
* [Installieren von Komponenten der Aufträge für die elastische Datenbank](sql-database-elastic-jobs-service-installation.md) 
* [Erste Schritte mit Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-getting-started.md)
* [Erstellen und Verwalten von Aufträgen über PowerShell](sql-database-elastic-jobs-powershell.md)
* [Erstellen und Verwalten von horizontal hochskalierten Azure SQL-Datenbanken](sql-database-elastic-jobs-getting-started.md)

**Auftrag für die elastische Datenbank** ist derzeit ein vom Kunden gehosteter Azure-Clouddienst, der die geplante und die Ad-hoc-Ausführung von Verwaltungsaufgaben, so genannten **Aufträgen**, ermöglicht. Mithilfe von Aufträgen lassen sich große Gruppen von Azure SQL-Datenbanken durch Ausführen von Transact-SQL-Skripts für administrative Vorgänge problemlos und zuverlässig verwalten. 

![Dienst für elastische Datenbankaufträge][1]

## <a name="why-use-jobs"></a>Argumente für die Verwendung von Aufträgen
**Verwalten**

Führen Sie Schemaänderungen, die Verwaltung von Anmeldeinformationen, Verweisdatenaktualisierungen, Leistungsdatensammlung oder Telemetrieerfassung für Mandanten (Kunden) problemlos durch.

**Berichte**

Aggregieren Sie Daten aus einer Sammlung von Azure SQL-Datenbanken in einer einzigen Zieltabelle.

**Reduzierter Aufwand**

Normalerweise müssen Sie unabhängige Verbindungen mit jeder einzelnen Datenbank herstellen, um Transact-SQL-Anweisungen oder andere Verwaltungsaufgaben auszuführen. Ein Auftrag nimmt Ihnen die Anmeldung bei den einzelnen Datenbanken in der Zielgruppe ab. Zudem können Sie Transact-SQL-Skripts zur Ausführung für eine Gruppe von Azure SQL-Datenbanken definieren, verwalten und speichern.

**Nachverfolgbarkeit**

Aufträge führen das Skript aus und protokollieren den Ausführungsstatus für die einzelnen Datenbanken. Außerdem werden Vorgänge automatisch wiederholt, wenn Fehler auftreten.

**Flexibilität**

Definieren Sie benutzerdefinierte Gruppen von Azure SQL-Datenbanken, und definieren Sie einen Zeitplan für die Ausführung eines Auftrags.

> [!NOTE]
> Im Azure-Portal steht nur eine eingeschränkte Anzahl von Funktionen für elastische SQL Azure-Pools zur Verfügung. Verwenden Sie die PowerShell-APIs, um auf den gesamten aktuell verfügbaren Funktionsumfang zuzugreifen.
> 
> 

## <a name="applications"></a>Anwendungen
* Führen Sie Verwaltungsaufgaben durch, wie z.B. die Bereitstellung eines neuen Schemas.
* Aktualisieren Sie Referenzdaten, z.B. allgemeine Produktinformationen, die für alle Datenbanken gelten. Sie können auch automatische Updates für jeden Arbeitstag nach Geschäftsschluss planen.
* Neuerstellung von Indizes zum Verbessern der Abfrageleistung. Für die Neuerstellung kann die Ausführung für eine gesamte Sammlung von Datenbanken auf wiederkehrender Basis konfiguriert werden, etwa in Zeiten mit geringer Auslastung.
* Sammlung von Abfrageergebnissen aus einer Menge von Datenbanken in einer zentralen Tabelle auf fortlaufender Grundlage. Leistungsabfragen können fortlaufend ausgeführt werden und für die Auslösung der Ausführung weiterer Aufgaben konfiguriert werden.
* Ausführung von Abfragen zur Datenverarbeitung mit längerer Laufzeit für eine große Anzahl von Datenbanken, z. B. bei der Sammlung von Kundentelemetrie. Die Ergebnisse werden zur weiteren Analyse in einer einzelnen Zieltabelle gesammelt.

## <a name="elastic-database-jobs-end-to-end"></a>Umfassender Überblick über Aufträge für die elastische Datenbank
1. Installieren Sie die Komponenten der **Aufträge für die elastische Datenbank** . Weitere Informationen finden Sie unter [Installieren von elastischen Datenbankaufträgen](sql-database-elastic-jobs-service-installation.md). Wenn die Installation fehlschlägt, lesen Sie die Informationen zum [Deinstallieren](sql-database-elastic-jobs-uninstall.md).
2. Verwenden Sie die PowerShell-APIs, um auf weitere Funktionen zuzugreifen, beispielsweise zum Erstellen von benutzerdefinierten Datenbanksammlungen, zum Hinzufügen von Zeitplänen und/oder dem Erfassen von Ergebnismengen. Verwenden Sie das Portal für die einfache Installation sowie die Erstellung und Überwachung von Aufträgen, die auf die Ausführung für einen **Pool für elastische Datenbanken** beschränkt sind. 
3. Erstellen Sie verschlüsselte Anmeldeinformationen für die Auftragsausführung und [fügen Sie den Benutzer (oder die Rolle) jeder Datenbank in der Gruppe hinzu](sql-database-security-overview.md).
4. Erstellen Sie ein idempotentes T-SQL-Skript, das für jede Datenbank in der Gruppe ausgeführt werden kann. 
5. Führen Sie die im folgenden Artikel erläuterten Schritte aus, um Aufträge mithilfe des Azure-Portals zu erstellen: [Erstellen und Verwalten von Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-create-and-manage.md). 
6. Alternativ können Sie auch PowerShell-Skripts verwenden: [Erstellen und Verwalten von Aufträgen für die elastische SQL-Datenbank mithilfe von PowerShell (Vorschau)](sql-database-elastic-jobs-powershell.md).

## <a name="idempotent-scripts"></a>Idempotente Skripts
Die Skripts müssen [idempotent](https://en.wikipedia.org/wiki/Idempotence)sein. Einfach ausgedrückt bedeutet „idempotent“, dass die erneute Ausführung eines bereits erfolgreich ausgeführten Skripts zum gleichen Ergebnis führt. Es kann vorkommen, dass ein Skript aufgrund von vorübergehenden Netzwerkproblemen nicht erfolgreich ausgeführt wird. In diesem Fall führt der Auftrag automatisch eine bestimmte Anzahl von Wiederholungsversuchen für das Skript aus. Ein idempotentes Skript führt auch bei zweimaliger erfolgreicher Ausführung zum gleichen Ergebnis. 

Vor dem Erstellen eines Objekts empfiehlt es sich, zu überprüfen, ob es bereits vorhanden ist.  

    IF NOT EXIST (some_object)
    -- Create the object 
    -- If it exists, drop the object before recreating it.

Analog dazu muss auch ein Skript erfolgreich ausgeführt werden können, indem logische Tests durchgeführt werden und angemessen auf die vorgefundenen Bedingungen reagiert wird.

## <a name="failures-and-logs"></a>Fehler und Protokolle
Wenn ein Skript auch nach mehreren Versuchen nicht erfolgreich ausgeführt werden kann, wird der Fehler protokolliert und der Auftrag fortgesetzt. Nach Beendigung eines Auftrags (also nachdem er für alle Datenbanken in der Gruppe ausgeführt wurde) können Sie sich die Liste mit den Fehlversuchen ansehen. Die Protokolle enthalten Details zum Debuggen der fehlerhaften Skripts. 

## <a name="group-types-and-creation"></a>Gruppentypen und Erstellung
Es gibt zwei Arten von Gruppen: 

1. Shard-Gruppen
2. Benutzerdefinierte Gruppen

Shardgruppen werden mithilfe der [Tools für elastische Datenbanken](sql-database-elastic-scale-introduction.md) erstellt. Wenn Sie eine Shard-Gruppe erstellen, werden der Gruppe automatisch Datenbanken hinzugefügt bzw. daraus entfernt. Beispielsweise ist ein neuer Shard automatisch in der Gruppe enthalten, wenn Sie ihn der Shardzuordnung hinzufügen. Ein Auftrag kann dann für die Gruppe ausgeführt werden.

Benutzerdefinierte Gruppen werden dagegen starr definiert. Sie müssen explizit Datenbanken zu benutzerdefinierten Gruppen hinzufügen oder daraus entfernen. Wenn eine Datenbank aus der Gruppe gelöscht wird, versucht der Auftrag, das Skript für die Datenbank auszuführen, was letztendlich zu einem Fehler führt. Mit dem Azure-Portal erstellte Gruppen sind derzeit benutzerdefinierte Gruppen. 

## <a name="components-and-pricing"></a>Komponenten und Preise
Die folgenden Komponenten arbeiten zusammen, um einen Azure-Clouddienst zu erstellen, der eine Ad-hoc-Ausführung von Verwaltungsaufgaben ermöglicht. Die Komponenten werden installiert und automatisch während der Installation in Ihrem Abonnement konfiguriert. Sie können die Dienste am automatisch generierten identischen Namen erkennen. Der Name ist eindeutig und besteht aus dem Präfix "edj", gefolgt von 21 zufällig generierten Zeichen.

* **Azure Cloud Service**: Elastische Datenbankaufträge (Vorschau) werden als vom Kunden gehosteter Azure-Clouddienst bereitgestellt, um die angeforderten Aufgaben auszuführen. Der Dienst wird im Portal bereitgestellt und im Microsoft Azure-Abonnement gehostet. Der standardmäßig bereitgestellte Dienst wird mit mindestens zwei Workerrollen für hohe Verfügbarkeit ausgeführt. Die Standardgröße der einzelnen Workerrollen ("ElasticDatabaseJobWorker") wird in einer A0-Instanz ausgeführt. Die Preise finden Sie unter [Cloud Services Preise](https://azure.microsoft.com/pricing/details/cloud-services/). 
* **Azure SQL-Datenbank**: Der Dienst verwendet zum Speichern aller Auftragsmetadaten eine Azure SQL-Datenbank, die als **Verwaltungsdatenbank** bezeichnet wird. Die Standarddienstebene ist S0. Informationen zu den Preisen finden Sie unter [SQL-Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/).
* **Azure Service Bus**: Ein Azure Service Bus dient der Koordination der Arbeit innerhalb von Azure Cloud Service. Siehe [Service Bus Preise](https://azure.microsoft.com/pricing/details/service-bus/).
* **Azure Storage:**Ein Azure Storage-Konto wird zum Speichern von Diagnoseausgabeprotokollen verwendet, falls ein aufgetretenes Problem weiteres Debuggen erfordert (siehe [Aktivieren der Diagnose in Azure Cloud Services und Virtual Machines](../cloud-services/cloud-services-dotnet-diagnostics.md)). Information zu den Preisen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-elastic-database-jobs-work"></a>Funktionsweise von Aufträgen für die elastische Datenbank
1. Einer Azure SQL-Datenbank wird eine **Steuerdatenbank** zugeordnet, die alle Meta- und Statusdaten enthält.
2. Auf die Steuerdatenbank wird durch den **Auftragsdienst** sowohl zum Starten als auch zum Nachverfolgen der auszuführenden Aufträge zugegriffen.
3. Zwei verschiedene Rollen kommunizieren mit der Steuerdatenbank: 
   * Controller: Bestimmt, welche Aufträge Aufgaben zum Durchführen des angeforderten Auftrags benötigen und führt Wiederholungsversuche bei Aufträgen mit Fehlern durch Erstellen neuer Auftragsaufgaben aus.
   * Ausführung von Auftragsaufgaben: Führt die Auftragsaufgaben aus.

### <a name="job-task-types"></a>Typen von Auftragsaufgaben
Es gibt mehrere Typen von Auftragsaufgaben, die die Ausführung von Aufträgen abwickeln:

* ShardMapRefresh: Fragt die Shardzuordnung ab, um alle als Shards verwendeten Datenbanken zu bestimmen
* ScriptSplit: Teilt das Skript entlang der GO-Anweisungen in Batches auf
* ExpandJob: Erstellt untergeordnete Aufträge für jede Datenbank aus einem Auftrag, der eine Gruppe von Datenbanken zum Ziel hat
* ScriptExecution: Führt ein Skript für eine bestimmte Datenbank mithilfe definierter Anmeldeinformationen aus
* Dacpac: Wendet ein DACPAC auf eine bestimmte Datenbank mithilfe bestimmter Anmeldeinformationen an

## <a name="end-to-end-job-execution-work-flow"></a>End-to-End-Arbeitsablauf bei der Auftragsausführung
1. Ein Auftrag wird entweder mithilfe des Portals oder der PowerShell-API in die **Verwaltungsdatenbank** eingefügt. Der Auftrag fordert die Ausführung eines Transact-SQL-Skripts für eine Gruppe von Datenbanken mithilfe bestimmter Anmeldeinformationen an.
2. Der Controller identifiziert den neuen Auftrag. Auftragsaufgaben werden erstellt und ausgeführt, um das Skript aufzuteilen und die Datenbanken der Gruppe zu aktualisieren. Abschließend wird ein neuer Auftrag erstellt und ausgeführt, um den Auftrag zu erweitern und neue untergeordnete Aufträge zu erstellen, von denen für jeden die Ausführung des Transact-SQL-Skripts für eine bestimmte Datenbank der Gruppe festgelegt ist.
3. Der Controller identifiziert die erstellten untergeordneten Aufträge. Für jeden Auftrag erstellt der Controller eine Auftragsaufgabe und löst sie aus, um das Skript für eine Datenbank auszuführen. 
4. Nach dem Abschluss aller Auftragsaufgaben aktualisiert der Controller die Aufträge mit dem Status „abgeschlossen“. 
   Während der Auftragsausführung kann die PowerShell-API verwendet werden, um den aktuellen Status der Auftragsausführung anzuzeigen. Alle von den PowerShell-APIs zurückgegebenen Uhrzeiten werden in UTC angegeben. Gegebenenfalls kann eine Abbruchanforderung eingeleitet werden, um einen Auftrag zu beenden. 

## <a name="next-steps"></a>Nächste Schritte
[Installieren Sie die Komponenten](sql-database-elastic-jobs-service-installation.md). Danach [erstellen Sie ein Protokoll für jede Datenbank in der Gruppe der Datenbanken und fügen es hinzu](sql-database-manage-logins.md). Weitere Informationen zum Erstellen und Verwalten von Aufträgen finden Sie unter [Erstellen und Verwalten von elastischen Datenbankaufträgen](sql-database-elastic-jobs-create-and-manage.md). Informationen hierzu finden Sie auch unter [Erste Schritte mit Aufträgen für die elastische Datenbank](sql-database-elastic-jobs-getting-started.md).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-overview/elastic-jobs.png
<!--anchors-->





<!--HONumber=Feb17_HO3-->


