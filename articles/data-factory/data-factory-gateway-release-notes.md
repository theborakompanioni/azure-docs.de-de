<properties 
	pageTitle="Versionshinweise für Datenverwaltungsgateway | Azure Data Factory" 
	description="Versionshinweise für Datenverwaltungsgateway" 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/17/2016" 
	ms.author="spelluru"/>

# Versionshinweise für Datenverwaltungsgateway

Eine Herausforderung für die moderne Datenintegration ist das nahtlose Verschieben von Daten zwischen lokalen Speicherorten und der Cloud. Dank Data Factory verläuft diese Integration mit dem Datenverwaltungsgateway nahtlos. Es handelt sich um einen Agent, den Sie lokal installieren können, um die Hybriddatenverschiebung zu ermöglichen.

Weitere Informationen finden Sie unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit Azure Data Factory](data-factory-move-data-between-onprem-and-cloud.md).

## AKTUELLE VERSION (2.0.6013.1)

- Sie können die Sprache/Kultur auswählen, die von einem Gateway während der manuellen Installation verwendet werden soll.
- Wenn das Gateway nicht wie erwartet funktioniert, können Sie Gatewayprotokolle der letzten 7 Tage an Microsoft senden, um die Behebung des Problems zu erleichtern. Wenn das Gateway nicht mit dem Clouddienst verbunden ist, können Sie Gatewayprotokolle speichern und archivieren.
- Verbesserte Benutzeroberfläche für Gatewaykonfigurations-Manager:
	- Der Gatewaystatus ist auf der Registerkarte „Start“ besser sichtbar.
	- Neu organisierte und vereinfachte Steuerelemente.
- Sie können Daten aus einem anderen Speicher als einem Azure-Blob über Polybase und ein Stagingblob mithilfe des [in der Vorschauphase befindlichen codelosen Kopiertools](data-factory-copy-data-wizard-tutorial.md) in Azure SQL Data Warehouse kopieren. Unter [Gestaffeltes Kopieren](data-factory-copy-activity-performance.md#staged-copy) finden Sie allgemeine Details zu diesem Feature.
- Sie können das Datenverwaltungsgateway nutzen, um Daten direkt aus einer lokalen SQL Server-Datenbank in Azure Machine Learning zu erfassen.
- Leistungsverbesserungen
	- Verbessern der Leistung beim Anzeigen des Schemas bzw. einer Vorschau im Vergleich zu SQL Server im in der Vorschauphase befindlichen codelosen Kopiertool.


## Frühere Versionen

## 1\.12.5953.1
- Fehlerbehebungen

## 1\.11.5918.1

- Die maximale Größe des Gatewayereignisprotokolls wurde von 1 MB auf 40 MB erhöht.
- Wenn während der automatischen Aktualisierung des Gateways ein Neustart erforderlich ist, wird ein Warnungsdialogfeld angezeigt. Sie können auswählen, ob das Gateway jetzt oder zu einem späteren Zeitpunkt neugestartet werden soll.
- Wenn bei der automatischen Aktualisierung ein Fehler auftritt, versucht das Gatewayinstallationsprogramm maximal 3-mal, die automatische Aktualisierung zu wiederholen.
- Leistungsverbesserungen
	- Verbessern Sie die Leistung beim Laden großer Tabellen von lokalen Servern in Kopierszenarieren ohne Code.
- Fehlerbehebungen

## 1\.10.5892.1

- Leistungsverbesserungen
- Fehlerbehebungen

## 1\.9.5865.2

- Automatische Aktualisierung ohne Benutzereingriff
- Neues Taskleistensymbol mit Gatewaystatusanzeige
- „Jetzt aktualisieren“ auf dem Client
- Festlegen der geplanten Updatezeit
- PowerShell-Skript für Aktivierung/Deaktivierung der automatischen Aktualisierung
- Unterstützung für JSON-Format
- Leistungsverbesserungen
- Fehlerbehebungen

## 1\.8.5822.1

- Verbesserung der Problembehandlung
- Leistungsverbesserungen
- Fehlerbehebungen

### 1\.7.5795.1

- Leistungsverbesserungen
- Fehlerbehebungen

### 1\.7.5764.1

- Leistungsverbesserungen
- Fehlerbehebungen

### 1\.6.5735.1

- Unterstützung für lokale HDFS-Quelle/Senke
- Leistungsverbesserungen
- Fehlerbehebungen

### 1\.6.5696.1

- Leistungsverbesserungen
- Fehlerbehebungen

### 1\.6.5676.1

- Unterstützung für Diagnosetools im Konfigurations-Manager
- Unterstützung für Tabellenspalten für tabellarische Datenquellen für Azure Data Factory
- SQL-DW-Unterstützung für Azure Data Factory
- Reclusive-Unterstützung in BlobSource und FileSource für Azure Data Factory
- Unterstützung von CopyBehavior – MergeFiles, PreserveHierarchy und FlattenHierarchy in BlobSink und FileSink mit binärer Kopie für Azure Data Factory
- Unterstützung für Kopieraktivität mit Fortschrittsbericht für Azure Data Factory
- Unterstützung für Überprüfung der Datenquellenverbindung für Azure Data Factory
- Fehlerbehebungen


### 1\.6.5672.1

- Unterstützung des Tabellennamens für ODBC-Datenquelle für Azure Data Factory
- Leistungsverbesserungen
- Fehlerbehebungen

### 1\.6.5658.1

- Unterstützung der Dateisenke für Azure Data Factory
- Unterstützung für Beibehaltung der Hierarchie in binärer Kopie für Azure Data Factory
- Unterstützung der Kopieraktivität-Idempotenz für Azure Data Factory
- Fehlerbehebungen

### 1\.6.5640.1

- Unterstützung von drei weiteren Datenquellen für Azure Data Factory (ODBC, OData, HDFS)
- Unterstützung für Anführungszeichen im CSV-Parser für Azure Data Factory
- Unterstützung der Komprimierung (BZip2)
- Fehlerbehebungen

### 1\.5.5612.1

- Unterstützung von fünf relationalen Datenbanken für Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata und Sybase)
- Unterstützung der Komprimierung (Gzip und Deflate)
- Leistungsverbesserungen
- Fehlerbehebungen


### 1\.4.5549.1

- Unterstützung von Oracle-Datenquellen für Azure Data Factory hinzugefügt
- Leistungsverbesserungen
- Fehlerbehebungen

### 1\.4.5492.1

- Einheitliche Binärdatei, die sowohl Microsoft Azure Data Factory als auch Office 365 Power BI-Dienste unterstützt
- Optimierung von Konfigurationsbenutzeroberfläche und Registrierungsprozess
- Azure Data Factory – Azure Ingress- und Egress-Unterstützung für SQL Server-Datenquelle

### 1\.2.5303.1

- 	Beheben von Problemen mit der Zeitüberschreitung, um zeitaufwändigere Datenquellenverbindungen zu unterstützen
 	
### 1\.1.5526.8

- Erfordert .NET Framework 4.5.1 als Voraussetzung während des Setups.

### 1\.0.5144.2

- Keine Änderungen, die Azure Data Factory-Szenarien betreffen.

## Fragen/Antworten

### Warum versucht der Datenquellen-Manager, eine Verbindung mit dem Gateway herzustellen?
Dies ist ein Sicherheitsdesign, bei dem Sie nur lokale Datenquellen für den Cloudzugriff in Ihrem Unternehmensnetzwerk konfigurieren können. Ihre Anmeldeinformationen gelangen nicht an Orte außerhalb Ihrer Unternehmensfirewall. Stellen Sie sicher, dass Ihr Computer den Computer erreichen kann, auf dem das Gateway installiert ist.

<!---HONumber=AcomDC_0706_2016-->