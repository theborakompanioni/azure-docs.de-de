<properties
	pageTitle="Streamen von Azure-Diagnosedaten im heißen Pfad mithilfe von Event Hubs | Microsoft Azure"
	description="In diesem Artikel wird veranschaulicht, wie Sie eine End-to-End-Konfiguration der Azure-Diagnose mit Event Hubs durchführen, und es werden Anleitungen für gängige Szenarien bereitgestellt."
	services="event-hubs"
	documentationCenter="na"
	authors="sethmanheim"
	manager="timlt"
	editor="" />
<tags
	ms.service="event-hubs"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="na"
	ms.date="07/12/2016"
	ms.author="sethm" />

# Streamen von Azure-Diagnosedaten im heißen Pfad mithilfe von Event Hubs

Die Azure-Diagnose bietet flexible Methoden zum Erfassen von Metriken und Protokollen virtueller Computer in Clouddiensten sowie zur Übertragung der Ergebnisse an Azure Storage. Seit März 2016 (SDK 2.9) haben Sie die Möglichkeit, vollständig benutzerdefinierte Datenquellen als Senke für Azure-Diagnosedaten zu verwenden und Daten im heißen Pfad mithilfe von Azure Event Hubs innerhalb von Sekunden zu übertragen.

Zu den unterstützten Datentypen gehören:

- Ereignisablaufverfolgung für Windows-Ereignisse (ETW)
- Leistungsindikatoren
- Windows-Ereignisprotokolle
- Anwendungsprotokolle
- Infrastrukturprotokolle der Azure-Diagnose

In diesem Artikel erfahren Sie, wie Sie eine End-to-End-Konfiguration der Azure-Diagnose mit Event Hubs durchführen. Zusätzlich finden Sie Anleitungen für die folgenden gängigen Szenarien:

- Anpassen der Protokolle und Metriken, für die Event Hubs als Senke verwendet werden soll
- Ändern der Konfigurationen in den einzelnen Umgebungen
- Anzeigen von Event Hubs-Streamdaten
- Behandlung von Verbindungsproblemen

## Voraussetzungen
Die Verwendung von Event Hubs als Senke für Azure-Diagnosedaten wird von Cloud Services, VMs, VM-Skalierungsgruppen und Service Fabric unterstützt. Die Unterstützung beginnt mit Azure SDK 2.9 und den entsprechenden Azure-Tools für Visual Studio.

- Azure-Diagnoseerweiterung 1.6 (standardmäßiges Ziel im [Azure SDK für .NET 2.9 oder höher](https://azure.microsoft.com/downloads/))
- [Visual Studio 2013 oder höher](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- Vorhandene Konfigurationen der Azure-Diagnose in einer Anwendung mithilfe einer *.wadcfgx*-Datei und einer der folgenden Methoden:
	- Visual Studio: [Konfigurieren der Diagnose für Azure Cloud Services und Virtual Machines](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
	- Windows PowerShell: [Aktivieren der Diagnose mithilfe von PowerShell in Azure Cloud Services](../cloud-services/cloud-services-diagnostics-powershell.md)
- Es muss ein Event Hubs-Namespace bereitgestellt werden, wie im Artikel [Erste Schritte mit Event Hubs](./event-hubs-csharp-ephcs-getstarted.md) beschrieben

## Herstellen einer Verbindung zwischen der Azure-Diagnose und der Event Hubs-Senke
Die Azure-Diagnose verwendet standardmäßig stets ein Azure Storage-Konto als Senke für Protokolle und Metriken. Eine Anwendung kann zusätzlich Event Hubs als Senke verwenden, indem der *.wadcfgx* -Datei im **WadCfg**-Element des **PublicConfig**-Abschnitts ein neuer **Sinks**-Abschnitt hinzugefügt wird. In Visual Studio wird die *.wadcfgx* -Datei unter folgendem Pfad gespeichert:**Cloud Services-Projekt** > **Rollen** > **(RoleName)** > **diagnostics.wadcfgx**-Datei.

	  <SinksConfig>
	    <Sink name="HotPath">
	      <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
	    </Sink>
	  </SinksConfig>

In diesem Beispiel wird die Event Hub-URL auf den vollqualifizierten Namespace des Event Hubs festgelegt: ServiceBus-Namespace + „/“ + Event Hub-Name.

Die Event Hub-URL wird im [Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885) auf dem Event Hubs-Dashboard angezeigt.

Der Name der **Senke** kann auf eine beliebige gültige Zeichenfolge festgelegt werden, solange der Wert in der gesamten Konfigurationsdatei konsistent verwendet wird.

>[AZURE.NOTE]  Es können zusätzliche Senken in diesem Abschnitt konfiguriert werden, beispielsweise *applicationInsights*. Für die Azure-Diagnose können mehrere Senken definiert werden, wenn jede Senke ebenfalls im Abschnitt **PrivateConfig** deklariert ist.

Die Event Hubs-Senke muss ebenfalls im Abschnitt **PrivateConfig** der *.wadcfgx*-Konfigurationsdatei deklariert und definiert werden.

	  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
	    <StorageAccount name="" key="" endpoint="" />
	    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="9B3SwghJOGEUvXigc6zHPInl2iYxrgsKHZoy4nm9CUI=" />
	  </PrivateConfig>

Der **SharedAccessKeyName** muss einem SAS-Schlüssel (Shared Access Signature) und einer Richtlinie entsprechen, die im **ServiceBus/EventHub**-Namespace definiert wurde. Navigieren Sie zum Event Hubs-Dashboard im [Azure-Portal](https://manage.windowsazure.com), wählen Sie die Registerkarte **Konfigurieren** aus, und richten Sie eine benannte Richtlinie (z.B. „SendRule“) mit Berechtigungen zum *Senden* ein. Das **StorageAccount** wird ebenfalls in **PrivateConfig** konfiguriert. Hier müssen keine Werte geändert werden – vor allem dann nicht, wenn sie funktionieren. In diesem Beispiel lassen wir die Werte leer. Dies bedeutet, dass ein Downstream-Asset später die Werte festlegt. Beispielsweise legt die Umgebungskonfigurationsdatei *ServiceConfiguration.Cloud.cscfg* die geeigneten Namen und Schlüssel für die Umgebung fest.

>[AZURE.WARNING] Beachten Sie, dass der Event Hub-SAS-Schlüssel als Klartext in der *.wadcfgx*-Datei gespeichert wird. Häufig wird dieser Schlüssel in der Quellcodeverwaltung eingecheckt oder ist als Asset in Ihrem Buildserver verfügbar, deshalb sollten die Daten in geeigneter Weise geschützt werden. Es wird empfohlen, hier einen SAS-Schlüssel mit der Berechtigung *Nur senden* zu verwenden, damit böswillige Benutzer Schreibvorgänge im Event Hub, aber keinerlei Lauschvorgänge durchführen oder den Event Hub verwalten können.

## Konfigurieren von Event Hubs als Senke für Protokolle und Metriken der Azure-Diagnose
Wie bereits erläutert, werden alle standardmäßigen und benutzerdefinierten Diagnosedaten (d.h. Metriken und Protokolle) in den konfigurierten Intervallen automatisch an Azure Storage übertragen. Mit Event Hubs und allen zusätzlichen Senken können Sie den Event Hub für einen beliebigen Stamm- oder Blattknoten in der Hierarchie als Senke verwenden. Hierzu gehören ETW-Ereignisse, Leistungsindikatoren, Windows-Ereignisprotokolle und Anwendungsprotokolle.

Es ist wichtig abzuwägen, wie viele Datenpunkte tatsächlich nach Event Hubs übertragen werden sollten. In der Regel übertragen Entwickler Daten mit niedriger Latenz im heißen Pfad, die schnell genutzt und interpretiert werden müssen. Beispiele dafür sind Systeme, die Warnungen oder Regeln für die automatische Skalierung überwachen. Entwickler können auch einen alternativen Analyse- oder Suchspeicher konfigurieren – beispielsweise Azure Stream Analytics, ElasticSearch, ein benutzerdefiniertes Überwachungssystem oder ein bevorzugtes Überwachungssystem eines Drittanbieters.

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

Im vorhergehenden Beispiel wird die Senke nur auf drei Leistungsindikatoren angewendet: „Anforderungen in Warteschlange“, „Zurückgewiesene Anforderungen“ und „Prozessorzeit (%)“.

Das folgende Beispiel zeigt, wie ein Entwickler die Menge der gesendeten Daten auf die kritischen Metriken für die Dienstintegrität einschränken kann.

        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />

In diesem Beispiel wird die Senke auf Protokolle angewendet und auf die Ablaufverfolgung auf Fehlerebene gefiltert.

## Bereitstellen und Aktualisieren der Konfiguration einer Clouddienstanwendung und Diagnose

Visual Studio bietet die einfachste Methode, um die Anwendung und die Event Hubs-Senkenkonfiguration bereitzustellen. Öffnen Sie zum Anzeigen und Bearbeiten der Datei die *.wadcfgx*-Datei in Visual Studio, und bearbeiten und speichern Sie sie. Der Pfad lautet **Cloud Services-Projekt** > **Rollen** > **(RoleName)** > **diagnostics.wadcfgx**.

Zu diesem Zeitpunkt ist *.wadcfgx* im Verpackungsprozess aller Aktionen für die Bereitstellung und Bereitstellungsaktualisierung in Visual Studio, Visual Studio Team System und allen Befehlen oder Skripts enthalten, die auf MSBuild basieren und „/t:publish target“ verwenden. Darüber hinaus wird die Datei bei allen Bereitstellungen und Aktualisierungen mit der entsprechenden Azure-Diagnoseagenterweiterung auf Ihren VMs in Azure bereitgestellt.

Nach der Bereitstellung der Konfiguration von Anwendung und Azure-Diagnose wird die Aktivität sofort auf dem Dashboard des Event Hubs angezeigt. Anschließend können Sie die Daten im heißen Pfad im Listenerclient oder im Analysetool Ihrer Wahl anzeigen.

In der folgenden Abbildung zeigt das Event Hub-Dashboard, dass Diagnosedaten nach 23:00 Uhr ohne Fehler an den Event Hub gesendet wurden. Zu dieser Zeit wurde die Anwendung mit einer aktualisierten *.wadcfgx*-Datei bereitgestellt, und die Senke wurde ordnungsgemäß konfiguriert.

![][0]

>[AZURE.NOTE] Wenn Sie an der Konfigurationsdatei für die Azure-Diagnose (.wadcfgx) Änderungen vornehmen, wird empfohlen, die Aktualisierungen per Push an die gesamte Anwendung sowie die Konfiguration zu übertragen. Verwenden Sie hierzu entweder eine Visual Studio-Veröffentlichung oder ein Windows PowerShell-Skript.

## Anzeigen von Daten im heißen Pfad

Wie bereits erläutert, gibt es viele Anwendungsfälle für das Überwachen und Verarbeiten von Event Hubs-Daten.

Ein einfacher Ansatz ist das Erstellen einer kleinen Testkonsolenanwendung, mit der der Event Hub überwacht wird und der Ausgabedatenstrom gedruckt wird. Sie können den folgenden Code (dieser wird in [Erste Schritte mit Event Hubs](./event-hubs-csharp-ephcs-getstarted.md) ausführlicher erläutert) in einer Konsolenanwendung platzieren.

Beachten Sie, dass die Konsolenanwendung das [EventProcessor Nuget-Paket](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) umfassen muss.

Denken Sie daran, die Werte in spitzen Klammern in der **Main**-Funktion durch Werte für Ihre Ressourcen zu ersetzen.

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

- Der Event Hub zeigt keine eingehende oder ausgehende Ereignisaktivität.

	Überprüfen Sie, ob der Event Hub erfolgreich bereitgestellt wurde. Alle Verbindungsinformationen im Abschnitt **PrivateConfig** der *.wadcfgx*-Datei müssen mit den Werten Ihrer Ressourcen übereinstimmen, wie sie im Portal angezeigt werden. Stellen Sie sicher, dass Sie eine SAS-Richtlinie im Portal definiert haben (im Beispiel „SendRule“) und die Berechtigung *Senden* gewährt wurde.

- Nach einer Aktualisierung zeigt der Event Hub keine eingehende oder ausgehende Ereignisaktivität mehr.

	Vergewissern Sie sich zunächst, dass die Event Hub- und Konfigurationsinformationen richtig sind (wie zuvor beschrieben). Manchmal wird **PrivateConfig** bei einer Bereitstellungsaktualisierung zurückgesetzt. Zur Beseitigung des Problems wird empfohlen, alle Änderungen an der *.wadcfgx*-Datei im Projekt durchzuführen und dann ein vollständiges Anwendungsupdate per Push zu übertragen. Wenn dies nicht möglich ist, stellen Sie sicher, dass mit dem Diagnoseupdate eine vollständige **PrivateConfig** einschließlich des SAS-Schlüssels per Push übertragen wird.

- Ich habe die empfohlenen Schritte ausgeführt, aber der Event Hub funktioniert weiterhin nicht.

	Überprüfen Sie die Azure Storage-Tabelle, die Protokolle und Fehler zur Azure-Diagnose selbst enthält: **WADDiagnosticInfrastructureLogsTable**. Sie können hierzu beispielsweise mit dem Tool [Azure Storage-Explorer](http://www.storageexplorer.com) eine Verbindung mit diesem Speicherkonto herstellen, diese Tabelle anzeigen und eine Abfrage nach einem TimeStamp innerhalb der letzten 24 Stunden hinzufügen. Sie können das Tool verwenden, um eine CSV-Datei zu exportieren und in einer Anwendung wie Microsoft Excel zu öffnen. Excel erleichtert das Suchen nach Callingcard-Zeichenfolgen wie EventHubs, um festzustellen, welcher Fehler gemeldet wird.

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

<!---HONumber=AcomDC_0713_2016-->