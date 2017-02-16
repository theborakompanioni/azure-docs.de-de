---
title: "Konfigurationsschema für die Azure-Diagnose 1.3, 1.4, 1.5 | Microsoft-Dokumentation"
description: "Schemaversion 1.3, 1.4 und 1.5 für Azure-Diagnose, geliefert als Bestandteil des Microsoft Azure SDK."
services: multiple
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: robb
translationtype: Human Translation
ms.sourcegitcommit: 8c9c9dea1248205aa6303e11e1166d5d38786c1b
ms.openlocfilehash: 550f328ddd03f67419a339aa995920b1feef8d4e


---
# <a name="azure-diagnostics-13-14-15-configuration-schema"></a>Konfigurationsschema für die Azure-Diagnose 1.3, 1.4, 1.5
> [!NOTE]
> Azure-Diagnose ist die Komponente, die zum Erfassen von Leistungsindikatoren und anderen Statistiken aus Azure Virtual Machines, VM-Skalierungsgruppen, Service Fabric und Cloud Services verwendet wird.  Diese Seite ist nur relevant, wenn Sie einen dieser Dienste verwenden.
>

Azure-Diagnose wird zusammen mit anderen Microsoft-Diagnoseprodukten wie Azure Monitor, Application Insights und Log Analytics verwendet.

Die Konfigurationsdatei der Azure-Diagnose wird verwendet, um beim Start des Diagnosemonitors Diagnosekonfigurationseinstellungen festzulegen.  

Diese Seite gilt für die Versionen 1.3, 1.4 und 1.5 (Azure SDK 2.4 und höher). Neuere Konfigurationsabschnitte enthalten einen Kommentar mit der Version, in der sie hinzugefügt wurden.  

 Laden Sie die Schemadefinition für die öffentliche Konfigurationsdatei mit dem folgenden PowerShell-Befehl herunter:  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

 Weitere Informationen zur Verwendung der Azure-Diagnose erhalten Sie unter [Aktivieren der Diagnose in Azure Cloud Services](http://azure.microsoft.com/documentation/articles/cloud-services-dotnet-diagnostics/).  

## <a name="example-of-the-diagnostics-configuration-file"></a>Beispiel der Diagnosekonfigurationsdatei  
 Das folgende Beispiel zeigt eine typische Diagnosekonfigurationsdatei:  

```xml  
<?xml version="1.0" encoding="utf-8"?>  
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">   
  <PublicConfig>  
    <WadCfg>  
      <DiagnosticMonitorConfiguration overallQuotaInMB="10000">  

        <PerformanceCounters scheduledTransferPeriod="PT1M">  
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />  
        </PerformanceCounters>  

        <Directories scheduledTransferPeriod="PT5M">  
          <IISLogs containerName="iislogs" />  
          <FailedRequestLogs containerName="iisfailed" />  

          <DataSources>  
            <DirectoryConfiguration containerName="mynewprocess">  
              <Absolute path="C:\MyNewProcess" expandEnvironment="false" />  
            </DirectoryConfiguration>  
            <DirectoryConfiguration containerName="badapp">  
              <Absolute path="%SYSTEMDRIVE%\BadApp" expandEnvironment="true" />  
            </DirectoryConfiguration>  
            <DirectoryConfiguration containerName="goodapp">  
              <LocalResource name="Skippy" relativePath="..\PeanutButter"/>  
            </DirectoryConfiguration>  
          </DataSources>  

        </Directories>  

        <EtwProviders>  
          <EtwEventSourceProviderConfiguration   
                       provider="MyProviderClass"   
                       scheduledTransferPeriod="PT5M">  
            <Event id="0"/>  
            <Event id="1" eventDestination="errorTable"/>  
            <DefaultEvents />  
          </EtwEventSourceProviderConfiguration>  
          <EtwManifestProviderConfiguration provider="5974b00b-84c2-44bc-9e58-3a2451b4e3ad" scheduledTransferLogLevelFilter="Information" scheduledTransferPeriod="PT2M">  
            <Event id="0"/>  
            <DefaultEvents eventDestination="defaultTable"/>  
          </EtwManifestProviderConfiguration>  
        </EtwProviders>  

        <WindowsEventLog scheduledTransferPeriod="PT5M">  
          <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>  
          <DataSource name="System!*[System[Provider[@Name='NTFS'] and (EventID=55)]]" />  
          <DataSource name="System!*[System[Provider[@Name='disk'] and (EventID=7 or EventID=52 or EventID=55)]]" />  
        </WindowsEventLog>  

        <Logs  bufferQuotaInMB="1024"   
             scheduledTransferPeriod="PT1M"   
             scheduledTransferLogLevelFilter="Verbose"   
             sinks="ApplicationInsights.AppLogs"/>  <!-- sinks attribute added in 1.5 -->  

        <CrashDumps containerName="wad-crashdumps" directoryQuotaPercentage="30" dumpType="Mini">  
          <CrashDumpConfiguration processName="mynewprocess.exe" />  
          <CrashDumpConfiguration processName="badapp.exe"/>  
        </CrashDumps>  

      </DiagnosticMonitorConfiguration>  

      <SinksConfig>   <!-- Added in 1.5 -->  
        <Sink name="ApplicationInsights">   
          <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>   
          <Channels>   
            <Channel logLevel="Error" name="Errors"  />   
            <Channel logLevel="Verbose" name="AppLogs"  />   
          </Channels>   
        </Sink>   
      </SinksConfig>   

    </WadCfg>  
  </PublicConfig>  

  <PrivateConfig>  <!-- Added in 1.3 -->  
    <StorageAccount name="" key="" endpoint="" />  
  </PrivateConfig>  
  <IsEnabled>true</IsEnabled>  
</DiagnosticsConfiguration>  

```  

## <a name="reading-this-page"></a>Lesen dieser Seite  
 Die folgenden Tags weisen in etwa dieselbe Reihenfolge wie im vorherigen Beispiel auf.  Wenn Sie wider Erwarten keine vollständige Beschreibung sehen, durchsuchen Sie die Seite nach dem Element oder Attribut.  

## <a name="common-attribute-types"></a>Allgemeine Attributtypen  
 Das **scheduledTransferPeriod**-Attribut wird in mehreren Elementen angezeigt. Es handelt sich um das Intervall zwischen geplanten Datenübertragungen an den Speicher (minutengenau gerundet). Der Wert ist ein [Dauer-Datentyp im XML-Format](http://www.w3schools.com/schema/schema_dtypes_date.asp).

## <a name="diagnostics-configuration-namespace"></a>Namespace der Diagnosekonfiguration  
 Der XML-Namespace für die Diagnosekonfigurationsdatei lautet wie folgt:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="diagnosticsconfiguration-element"></a>DiagnosticsConfiguration-Element  
 Hinzugefügt in Version 1.3.  

 Das Element der obersten Ebene der Diagnosekonfigurationsdatei  

|Untergeordnete Elemente|Beschreibung|  
|--------------------|-----------------|  
|**PublicConfig**|Erforderlich. Siehe Beschreibung an anderer Stelle auf dieser Seite.|  
|**PrivateConfig**|Optional. Siehe Beschreibung an anderer Stelle auf dieser Seite.|  
|**IsEnabled**|Boolesch. Siehe Beschreibung an anderer Stelle auf dieser Seite.|  

## <a name="publicconfig-element"></a>PublicConfig-Element  
 Beschreibt die öffentliche Diagnosekonfiguration  

|Untergeordnete Elemente|Beschreibung|  
|--------------------|-----------------|  
|**WadCfg**|Erforderlich. Siehe Beschreibung an anderer Stelle auf dieser Seite.|  
|**StorageAccount**|Der Name des Azure Storage-Kontos zum Speichern der Daten. Dieser kann auch als Parameter angegeben werden, wenn das Cmdlet „Set-AzureServiceDiagnosticsExtension“ ausgeführt wird.|  
|**LocalResourceDirectory**|Das Verzeichnis auf dem virtuellen Computer, auf dem der Überwachungs-Agent Ereignisdaten speichert. Falls nicht anders festgelegt, wird das Standardverzeichnis verwendet:<br /><br /> Für eine Worker-/Webrolle: `C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> Für einen virtuellen Computer: `C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Erforderliche Attribute:<br /><br /> - **path**: Das Verzeichnis auf dem System, das von der Azure-Diagnose verwendet wird<br /><br /> - **expandEnvironment**: Steuert, ob Umgebungsvariablen im Pfadnamen erweitert werden.|  

## <a name="wadcfg-element"></a>WadCFG-Element  
 Identifiziert und konfiguriert die Telemetriedaten, die gesammelt werden sollen.  

|Untergeordnete Elemente|Beschreibung|  
|--------------------|-----------------|  
|**DiagnosticMonitorConfiguration**|Erforderliches Element<br /><br /> Optionale Attribute:<br /><br /> - **overallQuotaInMB**: Der maximale lokale Speicherplatz, der von den verschiedenen Typen von Diagnosedaten genutzt werden kann, die von der Azure-Diagnose erfasst werden. Die Standardeinstellung lautet 5.120 MB.<br /><br /> - **useProxyServer**: Konfigurieren Sie die Azure-Diagnose, um die Proxyservereinstellungen den IE-Einstellungen entsprechend zu verwenden.|  
|**CrashDumps**|Siehe Beschreibung an anderer Stelle auf dieser Seite.|  
|**DiagnosticInfrastructureLogs**|Aktivieren Sie die Sammlung der Protokolle, die von der Azure-Diagnose generiert wurde. Die Protokolle der Diagnoseinfrastruktur sind hilfreich für die Problembehandlung des Diagnosesystems selbst. Optionale Attribute:<br /><br /> - **scheduledTransferLogLevelFilter**: Konfiguriert den minimalen Schweregrad der erfassten Protokolle.<br /><br /> - **scheduledTransferPeriod**: Das Intervall zwischen geplanten Datenübertragungen an den Speicher (minutengenau gerundet). Der Wert ist ein [Dauer-Datentyp im XML-Format](http://www.w3schools.com/schema/schema_dtypes_date.asp). |  
|**Verzeichnisse**|Siehe Beschreibung an anderer Stelle auf dieser Seite.|  
|**EtwProviders**|Siehe Beschreibung an anderer Stelle auf dieser Seite.|  
|**Metriken**|Siehe Beschreibung an anderer Stelle auf dieser Seite.|  
|**PerformanceCounters**|Siehe Beschreibung an anderer Stelle auf dieser Seite.|  
|**WindowsEventLog**|Siehe Beschreibung an anderer Stelle auf dieser Seite.|  

## <a name="crashdumps-element"></a>CrashDumps-Element  
 Aktivieren Sie die Sammlung der Absturzabbilder.  

|Attribute|Beschreibung|  
|----------------|-----------------|  
|**containerName**|Optional. Der Name des Blobcontainers in Ihrem Azure Storage-Konto, der zum Speichern der Absturzabbilder verwendet wird|  
|**crashDumpType**|Optional.  Konfiguriert die Azure-Diagnose für die Erfassung von kleinen oder vollständigen Absturzabbildern.|  
|**directoryQuotaPercentage**|Optional.  Konfiguriert den Prozentsatz der **overallQuotaInMB**, der für Absturzabbilder auf dem virtuellen Computer reserviert werden soll.|  

|Untergeordnete Elemente|Beschreibung|  
|--------------------|-----------------|  
|**CrashDumpConfiguration**|Erforderlich. Definiert die Konfigurationswerte für jeden Prozess.<br /><br /> Das folgende Attribut ist ebenso erforderlich:<br /><br /> **processName**: Der Name des Prozesses, für den die Azure-Diagnose ein Absturzabbild erfassen soll|  

## <a name="directories-element"></a>Directories-Element  
 Ermöglicht das Sammeln der Inhalte eines Verzeichnisses, der IIS-Zugriffsfehlerprotokolle und/oder der IIS-Protokolle.  

 Optionales **scheduledTransferPeriod**-Attribut. Siehe Erklärung weiter oben.  

|Untergeordnete Elemente|Beschreibung|  
|--------------------|-----------------|  
|**DataSources**|Eine Liste der zu überwachenden Verzeichnisse|  
|**FailedRequestLogs**|Das Einbeziehen dieses Elements in die Konfiguration ermöglicht die Erfassung der Protokolle zu fehlgeschlagenen Anfragen an die IIS-Website oder -Anwendung. Zur Aktivierung müssen Sie auch die Verfolgungsoptionen in **Web.config** unter **system.WebServer** festlegen.|  
|**IISLogs**|Das Einbeziehen dieses Elements in die Konfiguration ermöglicht die Erfassung von IIS-Protokollen:<br /><br /> **containerName**: Der Name des Blobcontainers in Ihrem Azure Storage-Konto, der zum Speichern der IIS-Protokolle verwendet wird|  

## <a name="datasources-element"></a>DataSources-Element  
 Eine Liste der zu überwachenden Verzeichnisse  

|Untergeordnete Elemente|Beschreibung|  
|--------------------|-----------------|  
|**DirectoryConfiguration**|Erforderlich. Erforderliches Attribut:<br /><br /> **containerName**: Der Name des Blobcontainers in Ihrem Azure Storage-Konto, der zum Speichern der Protokolldateien verwendet wird|  

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration-Element  
 Kann das Element **Absolute** oder **LocalResource** enthalten, aber nicht beide.  

|Untergeordnete Elemente|Beschreibung|  
|--------------------|-----------------|  
|**Absolute**|Der absolute Pfad zum Verzeichnis, das überwacht werden soll Die folgenden Attribute sind erforderlich:<br /><br /> - **Path**: Der absolute Pfad zum Verzeichnis, das überwacht werden soll<br /><br /> - **expandEnvironment**: Konfiguriert, ob die Umgebungsvariablen im Pfad erweitert werden.|  
|**LocalResource**|Der Pfad in Relation zu einer lokalen Ressource, die überwacht werden soll. Erforderliche Attribute:<br /><br /> - **Name**: Die lokale Ressource, die das zu überwachende Verzeichnis enthält<br /><br /> - **relativePath**: Der Pfad in Relation zum Namen, der das zu überwachende Verzeichnis enthält|  

## <a name="etwproviders-element"></a>EtwProviders-Element  
 Konfiguriert die Erfassung der ETW-Ereignisse von EventSource und/oder der ETW-Manifest-basierten Anbieter  

|Untergeordnete Elemente|Beschreibung|  
|--------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|Konfiguriert die Erfassung von Ereignissen, die über die [EventSource-Klasse](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx) generiert wurden. Erforderliches Attribut:<br /><br /> **provider**: Der Klassenname des EventSource-Ereignisses<br /><br /> Optionale Attribute:<br /><br /> - **scheduledTransferLogLevelFilter**: Der minimale Schweregrad, der in Ihr Speicherkonto übertragen wird<br /><br /> - **scheduledTransferPeriod**: Das Intervall zwischen geplanten Datenübertragungen an den Speicher (minutengenau gerundet). Der Wert ist ein [Dauer-Datentyp im XML-Format](http://www.w3schools.com/schema/schema_dtypes_date.asp). |  
|**EtwManifestProviderConfiguration**|Erforderliches Attribut:<br /><br /> **provider**: Die GUID des Ereignisanbieters<br /><br /> Optionale Attribute:<br /><br /> - **scheduledTransferLogLevelFilter**: Der minimale Schweregrad, der in Ihr Speicherkonto übertragen wird<br /><br /> - **scheduledTransferPeriod**: Das Intervall zwischen geplanten Datenübertragungen an den Speicher (minutengenau gerundet). Der Wert ist ein [Dauer-Datentyp im XML-Format](http://www.w3schools.com/schema/schema_dtypes_date.asp). |  

## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration-Element  
 Konfiguriert die Erfassung von Ereignissen, die über die [EventSource-Klasse](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx) generiert wurden.  

|Untergeordnete Elemente|Beschreibung|  
|--------------------|-----------------|  
|**DefaultEvents**|Optionales Attribut:<br/><br/> **eventDestination**: Der Name der Tabelle zum Speichern der Ereignisse|  
|**Event**|Erforderliches Attribut:<br /><br /> **id**: Die ID des Ereignisses<br /><br /> Optionales Attribut:<br /><br /> **eventDestination**: Der Name der Tabelle zum Speichern der Ereignisse|  

## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration-Element  

|Untergeordnete Elemente|Beschreibung|  
|--------------------|-----------------|  
|**DefaultEvents**|Optionales Attribut:<br /><br /> **eventDestination**: Der Name der Tabelle zum Speichern der Ereignisse|  
|**Event**|Erforderliches Attribut:<br /><br /> **id**: Die ID des Ereignisses<br /><br /> Optionales Attribut:<br /><br /> **eventDestination**: Der Name der Tabelle zum Speichern der Ereignisse|  

## <a name="metrics-element"></a>Metrics-Element  
 Ermöglicht Ihnen das Generieren einer Leistungsindikatortabelle, die für schnelle Abfragen optimiert ist. Jeder Leistungsindikator, der im **PerformanceCounters**-Element definiert ist, wird in der Metriktabelle zusätzlich zur Leistungsindikatortabelle gespeichert.  

 Das **resourceId**-Attribut muss angegeben werden.  Dies ist die Ressourcen-ID des virtuellen Computers, auf dem Sie die Azure-Diagnose bereitstellen. Rufen Sie **resourceID** im [Azure-Portal](https://portal.azure.com) ab. Wählen Sie **Durchsuchen** -> **Ressourcengruppen** -> **<Name\>** aus. Klicken Sie auf die Kachel **Eigenschaften**, und kopieren Sie den Wert aus dem Feld **ID**.  

|Untergeordnete Elemente|Beschreibung|  
|--------------------|-----------------|  
|**MetricAggregation**|Erforderliches Attribut:<br /><br /> **scheduledTransferPeriod**: Das Intervall zwischen geplanten Datenübertragungen an den Speicher (minutengenau gerundet). Der Wert ist ein [Dauer-Datentyp im XML-Format](http://www.w3schools.com/schema/schema_dtypes_date.asp). |  

## <a name="performancecounters-element"></a>PerformanceCounters-Element  
 Ermöglicht das Sammeln von Leistungsindikatoren.  

 Optionales Attribut:  

 Optionales **scheduledTransferPeriod**-Attribut. Siehe Erklärung weiter oben.

|Untergeordnetes Element|Beschreibung|  
|-------------------|-----------------|  
|**PerformanceCounterConfiguration**|Die folgenden Attribute sind erforderlich:<br /><br /> - **counterSpecifier**: Der Name des Leistungsindikators Beispiel: `\Processor(_Total)\% Processor Time`. Führen Sie zum Abrufen einer Liste der Leistungsindikatoren auf Ihrem Host den Befehl `typeperf` aus.<br /><br /> - **sampleRate**: Gibt an, wie oft Stichproben für den Indikator erstellt werden.<br /><br /> Optionales Attribut:<br /><br /> **unit**: Die Maßeinheit des Indikators|  

## <a name="windowseventlog-element"></a>WindowsEventLog-Element  
 Ermöglicht das Sammeln von Windows-Ereignisprotokollen.  

 Optionales **scheduledTransferPeriod**-Attribut. Siehe Erklärung weiter oben.  

|Untergeordnetes Element|Beschreibung|  
|-------------------|-----------------|  
|**DataSource**|Die Windows-Ereignisprotokolle, die erfasst werden sollen. Erforderliches Attribut:<br /><br /> **name**: Die XPath-Abfrage, die die zu erfassenden Windows-Ereignisse beschreibt. Beispiel:<br /><br /> `Application!*[Application[(Level <=3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[Security[(Level <= 3)]`<br /><br /> Zum Erfassen aller Ereignisse geben Sie „*“ ein.|  

## <a name="logs-element"></a>Logs-Element  
 Vorhanden in Version 1.0 und 1.1. Nicht vorhanden in 1.2. Wieder vorhanden in 1.3.  

 Definiert die Pufferkonfiguration für grundlegende Azure-Protokolle.  

|Attribut|Typ|Beschreibung|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|**unsignedInt**|Optional. Gibt die Höchstmenge des Dateisystemspeichers an, der für die angegebenen Daten verfügbar ist.<br /><br /> Der Standardwert ist 0.|  
|**scheduledTransferLogLevelFilterr**|**string**|Optional. Gibt den minimalen Schweregrad für Protokolleinträge an, die übertragen werden. Der Standardwert ist **Undefined**, der alle Protokolle überträgt. Weitere mögliche Werte (meiste Informationen bis wenigste Informationen) sind **Verbose**, **Information**, **Warning**, **Error** und **Critical**.|  
|**scheduledTransferPeriod**|**duration**|Optional. Gibt das Intervall zwischen geplanten Datenübertragungen an (minutengenau gerundet).<br /><br /> Die Standardeinstellung lautet „PT0S“.|  
|**sinks** Hinzugefügt in 1.5|**string**|Optional. Verweist auf einen Senkenspeicherort, um auch Diagnosedaten zu senden. Beispiel: Application Insights.|  

## <a name="sinksconfig-element"></a>SinksConfig-Element  
 Eine Liste mit Standorten, an die die Diagnosedaten und die diesen Standorten zugeordnete Konfiguration gesendet werden soll.  

|Elementname|Beschreibung|  
|------------------|-----------------|  
|**Senke**|Siehe Beschreibung an anderer Stelle auf dieser Seite.|  

## <a name="sink-element"></a>Sink-Element  
 Hinzugefügt in Version 1.5.  

 Definiert die Standorte, an die die Diagnosedaten gesendet werden sollen. Beispiel: der Application Insights-Dienst.  

|Attribut|Typ|Beschreibung|  
|---------------|----------|-----------------|  
|**name**|string|Eine Zeichenfolge für den Senkennamen.|  

|Element|Typ|Beschreibung|  
|-------------|----------|-----------------|  
|**Application Insights**|string|Wird nur beim Senden von Daten an Application Insights verwendet. Enthält den Instrumentationsschlüssel für ein aktives Application Insights-Konto, für das Sie Zugriff besitzen.|  
|**Kanäle**|string|Einer für jeden zusätzlichen Filter, den Sie streamen|  

## <a name="channels-element"></a>Channels-Element  
 Hinzugefügt in Version 1.5.  

 Definiert die Filter für Datenströme von Protokolldaten, die durch eine Senke übergeben werden.  

|Element|Typ|Beschreibung|  
|-------------|----------|-----------------|  
|**Channel**|string|Siehe Beschreibung an anderer Stelle auf dieser Seite.|  

## <a name="channel-element"></a>Channel-Element  
 Hinzugefügt in Version 1.5.  

 Definiert die Standorte, an die die Diagnosedaten gesendet werden sollen. Beispiel: der Application Insights-Dienst.  

|Attribute|Typ|Beschreibung|  
|----------------|----------|-----------------|  
|**logLevel**|**string**|Gibt den minimalen Schweregrad für Protokolleinträge an, die übertragen werden. Der Standardwert ist **Undefined**, der alle Protokolle überträgt. Weitere mögliche Werte (meiste Informationen bis wenigste Informationen) sind **Verbose**, **Information**, **Warning**, **Error** und **Critical**.|  
|**name**|**string**|Ein eindeutiger Name des Kanals, auf den verwiesen wird|  

## <a name="privateconfig-element"></a>PrivateConfig-Element  
 Hinzugefügt in Version 1.3.  

 Optional  

 Speichert die privaten Details des Speicherkontos (Name, Schlüssel und Endpunkt). Diese Informationen werden an den virtuellen Computer gesendet, können aber nicht daraus abgerufen werden.  

|Untergeordnete Elemente|Beschreibung|  
|--------------------|-----------------|  
|**StorageAccount**|Das zu verwendende Speicherkonto. Die folgenden Attribute sind erforderlich:<br /><br /> - **name**: Der Name des Speicherkontos<br /><br /> - **key**: Der Schlüssel des Speicherkontos<br /><br /> - **endpoint**: Der Endpunkt für den Zugriff auf das Speicherkonto|  

## <a name="isenabled-element"></a>IsEnabled-Element  
 Boolesch. Verwenden Sie `true`, um die Diagnose zu aktivieren, oder `false`, um die Diagnose zu deaktivieren.



<!--HONumber=Jan17_HO5-->


