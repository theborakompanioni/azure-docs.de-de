<properties 
   pageTitle="Streamen von Azure-Diagnosedaten im heißen Pfad mithilfe von Event Hubs"
   description="In diesem Artikel wird veranschaulicht, wie Sie eine End-to-End-Konfiguration der Azure-Diagnose mit Event Hubs durchführen, und es werden Richtlinien für häufige Szenarien bereitgestellt."
   services="event-hubs"
   documentationCenter="na"
   authors="tomarcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="event-hubs"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/08/2016"
   ms.author="tarcher" />

# Streamen von Azure-Diagnosedaten im heißen Pfad mithilfe von Event Hubs

## Übersicht
Die Azure-Diagnose bietet flexible Methoden zum Erfassen von Metriken und Protokollen von Compute-VMs und deren Übertragung in Azure Storage. Seit März 2016 (SDK 2.9) gibt es die Möglichkeit, vollständig benutzerdefinierte Datenquellen als Senke für Azure-Diagnosedaten zu verwenden und Daten im heißen Pfad mithilfe von Azure Event Hubs innerhalb von Sekunden zu übertragen.

Zu den unterstützten Datentypen gehören:

- ETW-Ereignisse
- Leistungsindikatoren
- Windows-Ereignisprotokolle 
- Anwendungsprotokolle
- Infrastrukturprotokolle der Azure-Diagnose
 
In diesem Artikel erfahren Sie, wie Sie eine End-to-End-Konfiguration der Azure-Diagnose mit Event Hubs durchführen. Darüber hinaus werden Informationen zu häufigen Szenarien bereitgestellt, z. B. das Festlegen der Protokolle und Metriken, für die Event Hubs als Senke verwendet werden soll. Sie erfahren außerdem, wie Sie die Konfiguration in jeder Umgebung ändern, und es wird ein Beispiel für die vielen Anzeigemöglichkeiten für Event Hubs-Streamdaten vorgestellt. Schließlich erfahren Sie, wie Sie eine Problembehandlung der Verbindung durchführen.

## Voraussetzungen
Die Verwendung von Event Hubs als Senke für Azure-Diagnosedaten wird für alle Computetypen mit WAD-Unterstützung unterstützt: Cloud-Dienst, VM, VMSS und Service Fabric. Die Unterstützung beginnt mit Azure SDK 2.9 und entsprechenden Azure-Tools für Visual Studio.
  
- Azure-Diagnoseerweiterung 1.6 (standardmäßiges Ziel im [Azure SDK für .NET 2.9 oder höher](https://azure.microsoft.com/downloads/))
- [Visual Studio 2013 oder höher](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- Bevor die Azure-Diagnose erfolgreich konfiguriert werden kann, muss zunächst mithilfe einer *.wadcfgx*-Datei unter Verwendung einer der folgenden Methoden eine Konfiguration in der Anwendung durchgeführt werden:
	- Visual Studio: [Konfigurieren der Diagnose für Azure Cloud Services und Virtual Machines](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
	- Windows PowerShell: [Aktivieren der Diagnose mithilfe von PowerShell in Azure Cloud Services](../cloud-services/cloud-services-diagnostics-powershell.md)
- Es muss ein Event Hubs-Namespace bereitgestellt werden, wie im Artikel [Erste Schritte mit Event Hubs](./event-hubs-csharp-ephcs-getstarted.md) beschrieben

## Herstellen einer Verbindung zwischen der Azure-Diagnose und der Event Hubs-Senke
Die Azure-Diagnose verwendet standardmäßig stets ein Azure-Speicherkonto als Senke für Protokolle und Metriken. Eine Anwendung kann zusätzlich Event Hubs als Senke verwenden, indem der *.wadcfgx* -Datei im **WadCfg**-Elements des **PublicConfig**-Abschnitts ein neuer **Sinks**-Abschnitt hinzugefügt wird. In Visual Studio wird die *.wadcfgx* Datei unter *Cloud-Dienst-Projekt > Rollen > (RoleName) > diagnostics.wadcfgx* -Datei gespeichert.
  
	  <SinksConfig>
	    <Sink name="HotPath">
	      <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
	    </Sink>
	  </SinksConfig>

In diesem Beispiel wird die Event Hub-URL auf den vollqualifizierten Namespace des Event Hubs festgelegt (ServiceBus-Namespace + „/“ + Event Hub-Name).

Die Event Hub-URL wird im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885) im Event Hubs-Dashboard angezeigt.

Der Name der **Senke** kann auf eine beliebige gültige Zeichenfolge festgelegt werden, solange der Wert in der gesamten Konfigurationsdatei konsistent verwendet wird.

**Hinweis:** Es können zusätzliche Senken in diesem Abschnitt konfiguriert werden, beispielsweise „applicationInsights“. Für die Azure-Diagnose können mehrere Senken definiert werden. Jede Senke muss jedoch auch im Abschnitt **PrivateConfig** deklariert werden.

Die Event Hubs-Senke muss ebenfalls im Abschnitt **PrivateConfig** der *.wadcfgx*-Konfigurationsdatei deklariert und definiert werden.

	  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
	    <StorageAccount name="" key="" endpoint="" />
	    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="9B3SwghJOGEUvXigc6zHPInl2iYxrgsKHZoy4nm9CUI=" />
	  </PrivateConfig>

Der **SharedAccessKeyName** muss einem SAS-Schlüssel und einer Richtlinie entsprechen, die im **ServiceBus/EventHub**-Namespace definiert wurden. Wechseln Sie hierzu zum Event Hub-Dashboard im [klassischen Azure-Portal](https://manage.windowsazure.com), klicken Sie auf die Registerkarte **Konfigurieren**, und richten Sie eine benannte Richtlinie ein (z. B. „SendRule“), die die Berechtigung *Senden* umfasst. Das **StorageAccount** wird ebenfalls in **PrivateConfig** konfiguriert. Hier müssen keine Werte geändert werden – vor allem dann nicht, wenn sie funktionieren. In diesem Beispiel lassen wir die Werte leer, wodurch angegeben wird, dass die Werte durch ein Downstreamasset festgelegt werden, d. h. die für die Umgebung geeigneten Namen und Schlüssel werden durch die Datei *ServiceConfiguration.Cloud.cscfg* zur Umgebungskonfiguration festgelegt.

>[AZURE.WARNING] Beachten Sie, dass der Event Hub-SAS-Schlüssel als Klartext in der *.wadcfgx*-Datei gespeichert wird. Häufig werden diese Informationen in der Quellcodeverwaltung oder als Asset in Ihrem Buildserver eingecheckt, deshalb sollten die Daten in geeigneter Weise geschützt werden. Es wird empfohlen, hier einen SAS-Schlüssel mit der Berechtigung *Nur senden* zu verwenden, damit böswillige Benutzer allenfalls Schreibvorgänge im Event Hub, aber keinerlei Lauschvorgänge durchführen oder den Event Hub verwalten können.

## Konfigurieren von Event Hubs als Senke für Protokolle und Metriken der Azure-Diagnose
Wie bereits erläutert, werden alle standardmäßigen und benutzerdefinierten Diagnosedaten (d. h. Metriken und Protokolle) in den konfigurierten Intervallen automatisch an Azure Storage übertragen. Mit Event Hubs (und allen zusätzlichen Senken) haben Sie die Möglichkeit, den Event Hub für einen beliebigen Stamm- oder Blattknoten in der Hierarchie als Senke zu verwenden. Hierzu gehören ETW-Ereignisse, Leistungsindikatoren, Windows-Ereignisprotokolle und Anwendungsprotokolle.

Es ist wichtig abzuwägen, wie viele Datenpunkte tatsächlich nach Event Hubs übertragen werden sollten. Typischerweise verwenden Entwickler diese Möglichkeit für Daten mit niedriger Latenz im heißen Pfad, die schnell verarbeitet und ausgewertet werden müssen (z. B. durch das Überwachen von Warnsystemen oder AutoScale-Regeln). Es ist auch möglich, eine alternative Analyse oder einen Suchspeicher zu konfigurieren – beispielsweise Stream Analytics, ElasticSearch, ein benutzerdefiniertes Überwachungssystem oder Ihr bevorzugtes Überwachungssystem eines Drittanbieters.

Nachfolgend sehen Sie einige Beispielkonfigurationen.

        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>

Im folgenden Beispiel wird die Senke auf den übergeordneten **PerformanceCounters**-Knoten in der Hierarchie angewendet. Dies bedeutet, dass alle untergeordneten **PerformanceCounters** an Event Hubs gesendet werden.

        <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
        </PerformanceCounters>

Im obigen Beispiel wird die Senke nur auf drei Leistungsindikatoren angewendet: „Anforderungen in Warteschlange“, „Zurückgewiesene Anforderungen“ und „Prozessorzeit (%)“.

Das folgende Beispiel zeigt, wie ein Entwickler die Kontrolle übernehmen und die Menge der Daten auf die kritischen Metriken für die Dienstintegrität einschränken kann.

        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />

In diesem Beispiel wird die Senke auf Protokolle angewendet und auf die Ablaufverfolgung auf Fehlerebene gefiltert.
 
## Bereitstellen und Aktualisieren der Konfiguration einer Clouddienstanwendung und Diagnose

Die einfachste Methode zum Bereitstellen der Anwendung gemeinsam mit der Event Hubs-Senkenkonfiguration ist die Verwendung von Visual Studio. Um die Konfiguration anzuzeigen und die gewünschten Bearbeitungen vorzunehmen, öffnen Sie die *.wadcfgx*-Datei in Visual Studio, die unter *Cloud-Dienst-Projekt > Rollen > (RoleName) > diagnostics.wadcfgx* gespeichert ist.

Zu diesem Zeitpunkt enthalten alle Bereitstellungsaktionen und Aktionen zur Bereitstellungsaktualisierung in Visual Studio, Visual Studio Team oder MSBUILD-basierte Befehle oder Skripts (unter Verwendung von „/t:publish target“) die *.wadcfgx*-Datei im Paketierungsprozess und stellen sicher, dass sie mithilfe der geeigneten WAD-Agent-Erweiterung in Ihren VMs für Azure bereitgestellt wird.
  
Nach der erfolgreichen Bereitstellung der Konfiguration von Anwendung und Azure-Diagnose wird die Aktivität sofort im Dashboard des Event Hubs angezeigt. Anschließend können Sie die Daten im heißen Pfad im Listenerclient oder im Analysetool Ihrer Wahl anzeigen.
 
In der folgenden Abbildung zeigt das Event Hub-Dashboard das fehlerfreie Senden von Diagnosedaten an den Event Hub ab etwa 23:00 Uhr, als die Anwendung mit einer aktualisierten *.wadcfgx* und der ordnungsgemäß konfigurierten Senke bereitgestellt wurde.

![][0]
  
>[AZURE.NOTE] Wenn Sie an der Konfigurationsdatei für die Azure-Diagnose (.wadcfgx) Änderungen vornehmen, wird empfohlen, die Aktualisierungen an der gesamten Anwendung sowie die Konfiguration per Push zu übertragen. Verwenden Sie hierzu entweder eine Visual Studio-Veröffentlichung oder ein Windows PowerShell-Skript.

## Anzeigen von Daten im heißen Pfad

Wie bereits erläutert, gibt es viele Anwendungsfälle für das Überwachen und Verarbeiten von Event Hubs-Daten.
  
Ein einfacher Ansatz ist das Erstellen einer kleinen Testkonsolenanwendung, mit der der Event Hub überwacht wird und der Ausgabedatenstrom gedruckt wird. Der folgende Code (dieser wird im Artikel [Erste Schritte mit Event Hubs](./event-hubs-csharp-ephcs-getstarted.md) ausführlicher erläutert) kann in einer Konsolenanwendung platziert werden.

Beachten Sie, dass die Konsolenanwendung das [EventProcessor Nuget-Paket](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) umfassen muss.

Denken Sie daran, die Werte in spitzen Klammern in der nachstehenden **Main**-Funktion durch Werte für Ihre Ressourcen zu ersetzen.

	//Console application code for EventHub test client
	using System;
	using System.Collections.Generic;
	using System.Diagnostics;
	using System.Linq;
	using System.Text;
	using System.Threading.Tasks;
	using Microsoft.ServiceBus.Messaging;
	
	namespace EventHubListener
	{
	    class SimpleEventProcessor : IEventProcessor
	    {
	        Stopwatch checkpointStopWatch;
	
	        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
	        {
	            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
	            if (reason == CloseReason.Shutdown)
	            {
	                await context.CheckpointAsync();
	            }
	        }
	
	        Task IEventProcessor.OpenAsync(PartitionContext context)
	        {
	            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
	            this.checkpointStopWatch = new Stopwatch();
	            this.checkpointStopWatch.Start();
	            return Task.FromResult<object>(null);
	        }
	
	        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
	        {
	            foreach (EventData eventData in messages)
	            {
	                string data = Encoding.UTF8.GetString(eventData.GetBytes());
	
	                Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
	                    context.Lease.PartitionId, data));
	
	                foreach (var x in eventData.Properties)
	                {
	                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
	                }
	            }
	
	            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
	            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
	            {
	                await context.CheckpointAsync();
	                this.checkpointStopWatch.Restart();
	            }
	        }
	    }
	
	    class Program
	    {
	        static void Main(string[] args)
	        {
	            string eventHubConnectionString = "Endpoint= <Your Connection String>”
	            string eventHubName = "<EventHub Name>";
	            string storageAccountName = "<Storage Account Name>";
	            string storageAccountKey = "<Storage Account Key>”;
	            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);
	
	            string eventProcessorHostName = Guid.NewGuid().ToString();
	            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
	            Console.WriteLine("Registering EventProcessor...");
	            var options = new EventProcessorOptions();
	            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
	            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();
	
	            Console.WriteLine("Receiving. Press enter key to stop worker.");
	            Console.ReadLine();
	            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
	        }
	    }
	}

## Problembehandlung für die Event Hubs-Senke

- Der Event Hub zeigt keine eingehende oder ausgehende Ereignisaktivität

	Überprüfen Sie, ob der Event Hub erfolgreich bereitgestellt wurde. Alle Verbindungsinformationen im Abschnitt **PrivateConfig** der *.wadcfgx*-Datei müssen mit den Werten Ihrer Ressourcen übereinstimmen, wie sie im Portal angezeigt werden. Stellen Sie sicher, dass Sie eine SAS-Richtlinie im Portal definiert haben (im Beispiel „SendRule“) und die Berechtigung „Senden“ gewährt wurde.

- Nachdem eine Aktualisierung durchgeführt wurde, zeigt der Event Hub keine eingehende oder ausgehende Aktivität mehr

	Stellen Sie zunächst sicher, dass für den Event Hub und die Konfiguration die oben genannten Anforderungen erfüllt wurden. Einige Probleme werden dadurch verursacht, dass die **PrivateConfig** bei einer Bereitstellungsaktualisierung zurückgesetzt wird. Zur Beseitigung des Problems wird empfohlen, alle Änderungen an der *.wadcfgx*-Datei im Projekt durchzuführen und dann ein vollständiges Anwendungsupdate per Push zu übertragen. Wenn dies nicht möglich ist, stellen Sie sicher, dass mit dem Diagnoseupdate eine vollständige **PrivateConfig** einschließlich des SAS-Schlüssels per Push übertragen wird.

- Ich habe den obigen Schritt ausgeführt, aber der Event Hub funktioniert weiterhin nicht

	Überprüfen Sie die Azure Storage-Tabelle, die Protokolle und Fehler zur Azure-Diagnose selbst enthält: **WADDiagnosticInfrastructureLogsTable**. Sie können hierzu beispielsweise mit dem Tool [Azure Storage-Explorer](http://www.storageexplorer.com) eine Verbindung mit diesem Speicherkonto herstellen, diese Tabelle anzeigen und eine Abfrage nach einem TimeStamp innerhalb der letzten 24 Stunden hinzufügen. Sie können mit diesem Tool eine CSV-Datei exportieren und diese z. B. in einer Anwendung wie Microsoft Excel öffnen, weil es in Excel einfach ist, nach relevanten Zeichenfolgen wie „EventHubs“ zu suchen, um zu sehen, welcher Fehler gemeldet wird.

## Nächste Schritte
• [Weitere Informationen zu Event Hubs](https://azure.microsoft.com/services/event-hubs/)

## Anhang: Beispiel einer vollständigen Konfigurationsdatei für die Azure-Diagnose (.wadcfgx)
	<?xml version="1.0" encoding="utf-8"?>
	<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
	  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
	    <WadCfg>
	      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
	        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
	        <Directories scheduledTransferPeriod="PT1M">
	          <IISLogs containerName="wad-iis-logfiles" />
	          <FailedRequestLogs containerName="wad-failedrequestlogs" />
	        </Directories>
	        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
	          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
	          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
	          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
	          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
	          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
	          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
	          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
	          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
	        </PerformanceCounters>
	        <WindowsEventLog scheduledTransferPeriod="PT1M">
	          <DataSource name="Application!*" />
	        </WindowsEventLog>
	        <CrashDumps>
	          <CrashDumpConfiguration processName="WaIISHost.exe" />
	          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
	          <CrashDumpConfiguration processName="w3wp.exe" />
	        </CrashDumps>
	        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
	      </DiagnosticMonitorConfiguration>
	      <SinksConfig>
	        <Sink name="HotPath">
	          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
	        </Sink>
	        <Sink name="applicationInsights">
	          <ApplicationInsights />
	          <Channels>
	            <Channel logLevel="Error" name="errors" />
	          </Channels>
	        </Sink>
	      </SinksConfig>
	    </WadCfg>
	    <StorageAccount />
	  </PublicConfig>
	  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
	    <StorageAccount name="" key="" endpoint="" />
	    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
	  </PrivateConfig>
	  <IsEnabled>true</IsEnabled>
	</DiagnosticsConfiguration>

Die ergänzende Datei *ServiceConfiguration.Cloud.cscfg* für dieses Beispiel sieht folgendermaßen aus.

	<?xml version="1.0" encoding="utf-8"?>
	<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
	  <Role name="MyFixIt.WorkerRole">
	    <Instances count="1" />
	    <ConfigurationSettings>
	      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
	    </ConfigurationSettings>
	  </Role>
	</ServiceConfiguration>

<!-- Images. -->
[0]: ./media/event-hubs-streaming-azure-diags-data/dashboard.png

<!---HONumber=AcomDC_0511_2016-->