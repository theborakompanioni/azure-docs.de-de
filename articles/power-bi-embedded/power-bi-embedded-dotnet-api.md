<properties
   pageTitle="Power BI Embedded .NET API"
   description=""
   services="power-bi-embedded"
   documentationCenter=""
   authors="dvana"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="03/29/2016"
   ms.author="derrickv"/>

# Power BI Embedded .NET API

**Microsoft Power BI Embedded**-Preview zielt in erster Linie auf das Verfügbarmachen der meisten vorhandenen Power BI-API-Funktionalitäten als Teil eines Azure-Diensts ab, mit dem Sie Anwendungen entwickeln können. Darüber hinaus können Sie die erforderlichen Ressourcen und den Power BI-Inhalt programmgesteuert bereitstellen, entwickeln und bereitstellen.

Sie können die **Power BI-API** verwenden, um Arbeitsbereiche von Power BI-Inhalt zu erstellen und zu verwalten. Mit der API können Sie

  - Eine Power BI-Desktop-Datei (PBIX) mithilfe einer schlüsselbasierten Authentifizierung in einen Arbeitsbereich importieren.
  - Verbindungszeichenfolgen eines Datasets ändern.
  - **Berichte** erhalten, um Sie in Ihre Anwendung zu integrieren.
  - Anmeldeinformationen für ein Dataset festlegen, um mit der richtigen Datenquelle kommunizieren zu können.
  - Weitere Informationen zu **Power BI-API** finden Sie unter [Get started with Microsoft Power BI Embedded preview](power-bi-embedded-get-started.md) (Erste Schritte mit Microsoft Power BI Embedded Preview).

Weitere Informationen finden Sie unter [Power BI-Referenz auf MSDN](https://msdn.microsoft.com/library/mt669800.aspx).

Im Folgenden sind einige der Klassen und Methoden aufgeführt, die im **Power BI Embedded-Beispiel** verwendet werden:

## Microsoft.PowerBI.Api.Beta-Namespace

### ReportsExtensions-Klasse
|Name|Beschreibung
|---|---
|[GetReports (IReports, String, String)](https://msdn.microsoft.com/library/microsoft.powerbi.api.beta.reportsextensions.getreports.aspx)|Ruft eine Liste der verfügbaren Berichte im angegebenen Arbeitsbereich ab
|[GetReportsAsync (IReports, String, String, CancellationToken)](https://msdn.microsoft.com/library/microsoft.powerbi.api.beta.reportsextensions.getreportsasync.aspx)|Ruft eine Liste der verfügbaren Berichte im angegebenen Arbeitsbereich ab

## Microsoft.PowerBI.Security-Namespace

### PowerBIToken-Methoden
|Name| Beschreibung
|---|---
|[CreateDevToken (String, Guid)](https://msdn.microsoft.com/library/mt670215.aspx)|Erstellt ein Entwicklertoken mit Standardablaufdauer, das verwendet wird, um auf Power BI-Plattformdienste zuzugreifen
|[CreateProvisionToken(String)](https://msdn.microsoft.com/library/mt670218.aspx)|Erstellt ein Bereitstellungstoken mit Standardablaufzeit, das verwendet wird, um Arbeitsbereiche innerhalb einer Arbeitsbereichsammlung zu verwalten.

## Weitere Informationen

- [Was ist Microsoft Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md)
- [Get started with Microsoft Power BI Embedded Preview](power-bi-embedded-get-started.md) (Erste Schritte mit Microsoft Power BI Embedded Preview)

<!---HONumber=AcomDC_0406_2016-->