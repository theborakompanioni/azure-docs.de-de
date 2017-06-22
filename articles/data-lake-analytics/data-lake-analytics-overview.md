---
title: "Übersicht über Microsoft Azure Data Lake Analytics | Microsoft Docs"
description: "Data Lake Analytics ist ein Azure Big Data-Dienst zur Nutzung von Daten für die Betriebsführung auf Basis der aus Ihren Clouddaten ermittelten Erkenntnisse – unabhängig von Größe und Speicherort der Daten."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 1e1d443a-48a2-47fb-bc00-bf88274222de
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/06/2017
ms.author: edmaca
ms.translationtype: Human Translation
ms.sourcegitcommit: 67ee6932f417194d6d9ee1e18bb716f02cf7605d
ms.openlocfilehash: d44570c7faa3e2468022ef539239c88f0418b2fa
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="overview-of-microsoft-azure-data-lake-analytics"></a>Übersicht über Microsoft Azure Data Lake Analytics
## <a name="what-is-azure-data-lake-analytics"></a>Was ist Azure Data Lake Analytics?
Azure Data Lake Analytics ist ein bedarfsgesteuerter Dienst für Analyseaufträge zum Vereinfachen von Big Data-Analysen. Sie können sich auf das Schreiben, Ausführen und Verwalten von Aufträgen konzentrieren und müssen sich nicht mit dem Betrieb der verteilten Infrastruktur auseinandersetzen. Anstatt sich der Bereitstellung, Konfiguration und Optimierung von Hardware zu widmen, schreiben Sie Abfragen, mit denen Sie Ihre Daten transformieren und nützliche Einblicke erhalten. Der Analysedienst ist in der Lage, umgehend Aufträge jeglicher Größenordnung zu verarbeiten. Wählen Sie dazu die jeweils erforderliche Ressourcenkapazität aus. Da Sie nur für die Leistung bezahlen, die während der Ausführung Ihres Auftrags tatsächlich in Anspruch genommen wurde, ist dies eine überaus kosteneffektive Lösung. Durch die Unterstützung von Azure Active Directory kann Data Lake zur Verwaltung von Zugriffsberechtigungen und Rollen in Ihr lokales Identitätssystem integriert werden. Darüber hinaus umfasst dieser Dienst U-SQL, eine Sprache, bei der die Vorteile von SQL mit den Ausdrücken von Benutzercode kombiniert werden. Die skalierbare verteilte Laufzeit von U-SQL ermöglicht eine effiziente Analyse der Daten im Speicher sowie in SQL Server-Instanzen in Azure, Azure SQL-Datenbank und Azure SQL Data Warehouse.

## <a name="key-capabilities"></a>Wichtige Funktionen
* **Dynamische Skalierung**
  
    Data Lake Analytics wurde für eine hohe Leistung in der Cloud entwickelt und optimiert.  Profitieren Sie von einer dynamischen Ressourcenbereitstellung und der Möglichkeit, Daten im Terabyte- oder sogar Exabytebereich zu analysieren. Sobald ein Auftrag abgeschlossen ist, werden die Ressourcen automatisch herunterskaliert, und Sie bezahlen nur für die tatsächlich in Anspruch genommene Verarbeitungsleistung. Außerdem sind keine Codeänderungen notwendig, wenn Sie die Menge an gespeicherten Daten oder die genutzte Computekapazität erhöhen oder verringern. So können Sie sich ausschließlich auf Ihre Geschäftslogik konzentrieren und müssen sich keinerlei Gedanken darüber machen, wie die großen Datenmengen verarbeitet oder gespeichert werden.
* **Entwickeln, debuggen und optimieren Sie Ihren Code schneller mithilfe vertrauter Tools**
  
    Data Lake Analytics ist nahtlos in Visual Studio integriert – dadurch können Sie für die Ausführung, das Debugging und die Optimierung Ihres Codes vertraute Tools nutzen. Mithilfe von Visualisierungen Ihrer U-SQL-Aufträge können Sie ermitteln, wie Ihr Code bei der Skalierung ausgeführt wird. Anhand dieser Einblicke lassen sich Leistungsengpässe leicht ermitteln und Kosten optimieren.
* **U-SQL: einfach und vertraut, problemlos erweiterbar**
  
    Data Lake Analytics enthält U-SQL, eine Abfragesprache, bei der die bekannte, einfache deklarative SQL-Sprache mit den Ausdrücken von C# kombiniert wird. U-SQL basiert auf derselben verteilten Laufzeit, die auch den Big Data-Systemen bei Microsoft zugrunde liegt. Millionen von SQL- und .NET-Entwickler können ihre Daten nun anhand bereits vorhandener Fähigkeiten verarbeiten und analysieren.
* **Nahtlose Integration in Ihre vorhandenen IT-Systeme**
  
    Data Lake Analytics kann vorhandene IT-Systeme für Identitäten, Verwaltung, Sicherheit und Data Warehousing verwenden. Dieser Ansatz trägt zur Vereinfachung von Datengovernance und zur problemlosen Erweiterung aktueller Datenanwendungen dar. Data Lake Analytics ist für die Benutzerverwaltung und Berechtigungsvergabe in Active Directory integriert und enthält Überwachungs- und Auditierungsfunktionen.
* **Erschwinglich und kosteneffektiv**
  
    Data Lake Analytics ist eine kosteneffektive Lösung für die Ausführung von Big Data-Workloads. Die Bezahlung erfolgt bei Datenverarbeitung pro Auftrag. Sie benötigen weder Hardware noch Lizenzen oder dienstspezifische Supportvereinbarungen. Wenn Aufträge gestartet oder beendet werden, wird das System automatisch zentral hoch- oder herunterskaliert. So bezahlen Sie stets nur für die tatsächlich in Anspruch genommene Ressourcenmenge.
* **Funktioniert mit allen Ihren Daten in Azure**
  
    Data Lake Analytics ist für die Arbeit mit Azure Data Lake optimiert und liefert erstklassige Leistungs-, Durchsatz- und Parallelisierungsergebnisse bei Big Data-Workloads.  Data Lake Analytics kann auch mit Azure Blob Storage und Azure SQL-Datenbank kombiniert werden.

## <a name="see-also"></a>Weitere Informationen
* Erste Schritte
  
  * Erste Schritte mit Data Lake Analytics mithilfe [des Azure-Portals](data-lake-analytics-get-started-portal.md) | [von Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [des Azure .NET SDK](data-lake-analytics-get-started-net-sdk.md)
  * [Erste Schritte mit Azure Data Lake Analytics-U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md)

* Verwaltung
  
  * [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md)
  * [Verwalten von Azure Data Lake Analytics mithilfe von Azure PowerShell](data-lake-analytics-manage-use-powershell.md)
  * [Überwachung und Problembehandlung von Azure Data Lake Analytics-Aufträgen mithilfe des Azure-Portals](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

* Teilen Sie uns Ihre Meinung mit
  
  * [Feature anfordern](http://aka.ms/adlafeedback)
  * [Hilfe in MSDN-Foren](http://aka.ms/adlaforums)


