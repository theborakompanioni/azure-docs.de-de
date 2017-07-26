---
title: Verwenden von Leistungsindikatoren in Azure-Diagnose | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Leistungsindikatoren in Azure Cloud Services oder Virtual Machine verwenden, um nach Engpässen zu suchen und die Leistung zu optimieren."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: fc4c61e9-d49d-4ed9-a32c-b91cdf851909
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/29/2016
ms.author: robb
ms.translationtype: Human Translation
ms.sourcegitcommit: be747170a0d8a7a6defd790a3f8a122c4d397671
ms.openlocfilehash: b4357150d0be21672d0b8e212c4ea1a7bec99412
ms.contentlocale: de-de
ms.lasthandoff: 05/23/2017


---
# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Erstellen und Verwenden von Leistungsindikatoren in einer Azure-Anwendung
In diesem Artikel werden die Vorteile von Leistungsindikatoren in Ihrer Azure-Anwendung und die Vorgehensweise zur Einbindung beschrieben. Sie können damit Daten sammeln, Engpässe ermitteln und die System- und Anwendungsleistung optimieren.

Für Windows Server, IIS und ASP.NET verfügbare Leistungsindikatoren können auch erfasst und verwendet werden, um die Integrität von Webrollen, Workerrollen und virtuellen Computern von Azure zu bestimmen. Sie können auch benutzerdefinierte Leistungsindikatoren erstellen und verwenden.  

Sie können Leistungsindikatordaten wie folgt untersuchen:

1. Direkt auf dem Anwendungshost mit dem Systemmonitor-Tool per Remotedesktop.
2. Mit System Center Operations Manager und dem Azure Management Pack.
3. Mit anderen Überwachungstools, mit denen auf die Diagnosedaten zugegriffen wird, die an den Azure-Speicher übertragen werden. Weitere Informationen finden Sie unter [Speichern und Anzeigen von Diagnosedaten im Azure-Speicher](https://msdn.microsoft.com/library/azure/hh411534.aspx) .  

Weitere Informationen zum Überwachen der Leistung Ihrer Anwendung im [Azure-Portal](http://portal.azure.com/) finden Sie unter [Überwachen von Clouddiensten](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).

Weitere ausführliche Anweisungen zum Erstellen einer Protokollierungs- und Nachverfolgungsstrategie und zum Verwenden von Diagnose- und anderen Methoden zum Beheben von Problemen und Optimieren von Azure-Anwendungen finden Sie unter [Troubleshooting Best Practices for Developing Azure Applications](https://msdn.microsoft.com/library/azure/hh771389.aspx)(Bewährte Methoden zur Problembehandlung bei der Entwicklung von Azure-Anwendungen, in englischer Sprache).

## <a name="enable-performance-counter-monitoring"></a>Aktivieren der Überwachung von Leistungsindikatoren
Leistungsindikatoren sind standardmäßig nicht aktiviert. Ihre Anwendung oder eine Startaufgabe muss die Standardkonfiguration des Diagnose-Agents ändern, um die speziellen Leistungsindikatoren einzubinden, die Sie für jede Rolleninstanz überwachen möchten.

### <a name="performance-counters-available-for-microsoft-azure"></a>Verfügbare Leistungsindikatoren für Microsoft Azure
Unter Azure wird eine Teilmenge der Leistungsindikatoren bereitgestellt, die für Windows Server, IIS und den ASP.NET-Stapel verfügbar sind. Die folgende Tabelle enthält einige Leistungsindikatoren, die für Azure-Anwendungen besonders interessant sind.

| Leistungsindikatorkategorie: Objekt (Instanz) | Name des Leistungsindikators | Referenz |
| --- | --- | --- |
| .NET CLR-Ausnahmen(*Global*) |Anzahl der ausgelösten Ausnahmen/Sek. |Ausnahmeleistungsindikatoren |
| .NET CLR-Speicher(*Global*) |GC-Zeitdauer in Prozent |Arbeitsspeicher-Leistungsindikatoren |
| ASP.NET |Anwendungsneustarts |Leistungsindikatoren für ASP.NET |
| ASP.NET |Ausführungszeit der Anforderung |Leistungsindikatoren für ASP.NET |
| ASP.NET |Getrennte Anforderungen |Leistungsindikatoren für ASP.NET |
| ASP.NET |Workerprozess-Neustarts |Leistungsindikatoren für ASP.NET |
| ASP.NET-Anwendungen(**Gesamt**) |Anforderungen gesamt |Leistungsindikatoren für ASP.NET |
| ASP.NET-Anwendungen(**Gesamt**) |Anforderungen/s |Leistungsindikatoren für ASP.NET |
| ASP.NET v4.0.30319 |Ausführungszeit der Anforderung |Leistungsindikatoren für ASP.NET |
| ASP.NET v4.0.30319 |Wartezeit der Anforderung |Leistungsindikatoren für ASP.NET |
| ASP.NET v4.0.30319 |Aktuelle Anforderungen |Leistungsindikatoren für ASP.NET |
| ASP.NET v4.0.30319 |Anforderungen in Warteschlange |Leistungsindikatoren für ASP.NET |
| ASP.NET v4.0.30319 |Zurückgewiesene Anforderungen |Leistungsindikatoren für ASP.NET |
| Arbeitsspeicher |Verfügbare MB |Arbeitsspeicher-Leistungsindikatoren |
| Arbeitsspeicher |Zugesicherte Bytes |Arbeitsspeicher-Leistungsindikatoren |
| Processor(_Total) (Prozessor(_Gesamt) |% Prozessorzeit |Leistungsindikatoren für ASP.NET |
| TCPv4 |Verbindungsfehler |TCP-Objekt |
| TCPv4 |Hergestellte Verbindungen |TCP-Objekt |
| TCPv4 |Zurückgesetzte Verbindungen |TCP-Objekt |
| TCPv4 |Segmente gesendet/s |TCP-Objekt |
| Netzwerkschnittstelle(*) |Empfangene Byte/Sek. |Netzwerkschnittstellenobjekt |
| Netzwerkschnittstelle(*) |Gesendete Byte/Sek. |Netzwerkschnittstellenobjekt |
| Netzwerkschnittstelle(Netzwerkkarte für Microsoft Virtual Machine-Bus_2) |Empfangene Byte/Sek. |Netzwerkschnittstellenobjekt |
| Netzwerkschnittstelle(Netzwerkkarte für Microsoft Virtual Machine-Bus_2) |Gesendete Byte/Sek. |Netzwerkschnittstellenobjekt |
| Netzwerkschnittstelle(Netzwerkkarte für Microsoft Virtual Machine-Bus_2) |Byte gesamt/Sek. |Netzwerkschnittstellenobjekt |

## <a name="create-and-add-custom-performance-counters-to-your-application"></a>Erstellen und Hinzufügen benutzerdefinierter Leistungsindikatoren zur Anwendung
Azure verfügt über Unterstützung zum Erstellen und Ändern benutzerdefinierter Leistungsindikatoren für Webrollen und Workerrollen. Die Leistungsindikatoren können zum Nachverfolgen und Überwachen von anwendungsspezifischem Verhalten verwendet werden. Sie können benutzerdefinierte Leistungsindikatorkategorien und -bezeichner erstellen und aus einer Startaufgabe, Webrolle oder Workerrolle mit erhöhten Rechten löschen.

> [!NOTE]
> Code, mit dem Änderungen an benutzerdefinierten Leistungsindikatoren vorgenommen werden, muss für die Ausführung über erhöhte Rechte verfügen. Wenn der Code in einer Webrolle oder Workerrolle enthalten ist, muss die Rolle das Tag <Runtime executionContext="elevated" /> in der Datei „ServiceDefinition.csdef“ enthalten, damit die Rolle richtig initialisiert wird.
>
>

Sie können benutzerdefinierte Leistungsindikatordaten mit dem Diagnose-Agent an den Azure-Speicher senden.

Die standardmäßigen Leistungsindikatordaten werden von Azure-Prozessen generiert. Benutzerdefinierte Leistungsindikatordaten müssen von der Webrollen- oder Workerrollenanwendung erstellt werden. Informationen zu den Datentypen, die in benutzerdefinierten Leistungsindikatoren gespeichert werden können, finden Sie unter [Arten von Leistungsindikatoren](https://msdn.microsoft.com/library/z573042h.aspx) . Ein Beispiel, bei dem benutzerdefinierte Leistungsindikatordaten in einer Webrolle erstellt und festgelegt werden, finden Sie unter [PerformanceCounters-Beispiel](http://code.msdn.microsoft.com/azure/) .

## <a name="store-and-view-performance-counter-data"></a>Speichern und Anzeigen von Leistungsindikatordaten
Azure speichert Leistungsindikatordaten mit anderen Diagnoseinformationen zwischen. Diese Daten stehen für die Remoteüberwachung zur Verfügung, während die Rolleninstanz ausgeführt wird, indem der Remotedesktopzugriff zum Anzeigen von Tools wie dem Systemmonitor verwendet wird. Um die Daten außerhalb der Rolleninstanz beizubehalten, muss der Diagnose-Agent die Daten in den Azure-Speicher übertragen. Die Größenbeschränkung der zwischengespeicherten Leistungsindikatordaten kann im Diagnose-Agent konfiguriert werden, oder sie kann als Teil eines gemeinsamen Grenzwerts für alle Diagnosedaten konfiguriert werden. Weitere Informationen zum Festlegen der Puffergröße finden Sie unter [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) und [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). Eine Übersicht über das Einrichten des Diagnose-Agents zum Übertragen von Daten in ein Speicherkonto finden Sie unter [Speichern und Anzeigen von Diagnosedaten im Azure-Speicher](https://msdn.microsoft.com/library/azure/hh411534.aspx) .

Jede konfigurierte Leistungsindikatorinstanz wird mit einer angegebenen Samplingrate aufgezeichnet. Die erfassten Daten werden an das Speicherkonto übertragen, indem entweder eine geplante Übertragungsanforderung oder eine bedarfsgesteuerte Übertragungsanforderung verwendet wird. Automatische Übertragungen können bis zu einmal pro Minute eingeplant werden. Die vom Diagnose-Agent übertragenen Leistungsindikatordaten werden in einer Tabelle (WADPerformanceCountersTable) im Speicherkonto gespeichert. Auf diese Tabelle kann mit standardmäßigen Azure-Speicher-API-Methoden zugegriffen werden, und es können Abfragen durchgeführt werden. Ein Beispiel zum Abfragen und Anzeigen von Leistungsindikatordaten aus der WADPerformanceCountersTable-Tabelle finden Sie unter [Microsoft Azure PerformanceCounters Sample](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) (Microsoft Azure-PerformanceCounters-Beispiel).

> [!NOTE]
> Je nach Übertragungshäufigkeit und Warteschlangenlatenz des Diagnose-Agents können die letzten Leistungsindikatordaten im Speicherkonto um mehrere Minuten veraltet sein.
>
>

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Aktivieren von Leistungsindikatoren mit der Diagnosekonfigurationsdatei
Verwenden Sie das folgende Verfahren, um die Leistungsindikatoren in der Azure-Anwendung zu aktivieren.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Abschnitt wird vorausgesetzt, dass Sie den Diagnosemonitor in Ihre Anwendung importiert und die Diagnosekonfigurationsdatei Ihrer Visual Studio-Projektmappe („diagnostics.wadcfg“ in SDK 2.4 und früher oder „diagnostics.wadcfgx“ in SDK 2.5 und höher) hinzugefügt haben. Weitere Informationen finden Sie in Schritt 1 und 2 unter [Aktivieren der Diagnose in Azure Cloud Services und Virtual Machines](cloud-services-dotnet-diagnostics.md).

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Schritt 1: Sammeln und Speichern der Daten von Leistungsindikatoren
Nachdem Sie der Visual Studio-Projektmappe die Diagnosedatei hinzugefügt haben, können Sie die Sammlung und Speicherung von Leistungsindikatordaten in einer Azure-Anwendung konfigurieren. Dazu werden der Diagnosedatei Leistungsindikatoren hinzugefügt. Die Diagnosedaten einschließlich der Leistungsindikatoren werden zuerst in der Instanz gesammelt. Die Daten werden dann in der Tabelle WADPerformanceCountersTable im Azure-Tabellendienst gespeichert, sodass Sie in Ihrer Anwendung auch das Speicherkonto angeben müssen. Wenn Sie Ihre Anwendung im Serveremulator lokal testen, können Sie die Diagnosedaten auch lokal im Speicheremulator speichern. Bevor Sie die Diagnosedaten speichern, müssen Sie das [Azure-Portal](http://portal.azure.com/) aufrufen und ein klassisches Speicherkonto erstellen. Eine bewährte Methode besteht darin, das Speicherkonto im selben geografischen Standort wie die Azure-Anwendung zu erstellen. Wenn sich die Azure-Anwendung und das Speicherkonto am selben geografischen Standort befinden, vermeiden Sie Kosten für externe Bandbreite und verringern die Latenz.

### <a name="add-performance-counters-to-the-diagnostics-file"></a>Hinzufügen von Leistungsindikatoren zur Diagnosedatei
Es gibt zahlreiche Leistungsindikatoren, die Sie verwenden können. Im folgenden Beispiel werden mehrere Leistungsindikatoren demonstriert, die für die Web- und Workerrollenüberwachung empfohlen werden.

Öffnen Sie die Diagnosedatei („diagnostics.wadcfg“ in SDK 2.4 und früher oder „diagnostics.wadcfgx“ in SDK 2.5 und höher), und fügen Sie dem DiagnosticMonitorConfiguration-Element Folgendes hinzu:

```xml
<PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
    <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use the Process(w3wp) category counters in a web role -->

    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use the Process(WaWorkerHost) category counters in a worker role.
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
</PerformanceCounters>
```

Das Attribut bufferQuotaInMB gibt die maximale Größe des Dateisystemspeichers an, der für den Datensammlungstyp (Azure-Protokolle, IIS-Protokolle usw.) zur Verfügung steht. Der Standardwert ist 0. Wenn das Kontingent erreicht ist, werden die ältesten Daten gelöscht, sobald neue Daten hinzugefügt werden. Die Summe aller bufferQuotaInMB-Eigenschaften müssen größer sein als der Wert des Attributs OverallQuotaInMB. Ausführlichere Informationen dazu, wie Sie feststellen, wie viel Speicher für die Sammlung von Diagnosedaten erforderlich ist, finden Sie im Abschnitt "Setup WAD" im Artikel [Troubleshooting Best Practices for Developing Azure Applications](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx)(Bewährte Methoden zur Problembehandlung bei der Entwicklung von Azure-Anwendungen, in englischer Sprache).

Das Attribut scheduledTransferPeriod gibt das auf die nächste Minute gerundete Intervall zwischen geplanten Datenübertragungen an. In den folgenden Beispielen ist es auf PT30M (30 Minuten) festgelegt. Wenn das Übertragungsintervall auf einen kleinen Wert wie z. B. 1 Minute festgelegt wird, hat dies in der Produktion negative Auswirkungen auf die Anwendungsleistung, kann jedoch beim Testen nützlich sein, um festzustellen, ob die Diagnose schnell arbeitet. Das geplante Übertragungsintervall muss so klein sein, dass Diagnosedaten in der Instanz nicht überschrieben werden, andererseits aber auch so groß, dass die Leistung der Anwendung nicht beeinträchtigt wird.

Das Attribut counterSpecifier gibt den zu erfassenden Leistungsindikator an. Das Attribut sampleRate gibt die Rate an, mit der der Leistungsindikator gemessen werden soll, in diesem Fall 30 Sekunden.

Nachdem Sie die zu erfassenden Leistungsindikatoren hinzugefügt haben, speichern Sie die Änderungen in der Diagnosedatei. Anschließend müssen Sie das Speicherkonto angeben, in dem die Diagnosedaten gespeichert werden.

### <a name="specify-the-storage-account"></a>Angeben des Speicherkontos
Um die Diagnoseinformationen in Ihrem Azure-Speicherkonto abzulegen, müssen Sie in der Dienstkonfigurationsdatei (ServiceConfiguration.cscfg) eine Verbindungszeichenfolge angeben.

Für Azure SDK 2.5 kann das Speicherkonto in der Datei „diagnostics.wadcfgx“ angegeben werden.

> [!NOTE]
> Diese Anleitung gilt nur für Azure SDK 2.4 und früher. Für Azure SDK 2.5 kann das Speicherkonto in der Datei „diagnostics.wadcfgx“ angegeben werden.
>
>

So legen Sie die Verbindungszeichenfolgen fest:

1. Öffnen Sie die Datei „ServiceConfiguration.Cloud.cscfg“ mit Ihrem bevorzugten Text-Editor, und legen Sie die Verbindungszeichenfolge für Ihren Speicher fest. Sie finden die Werte für *AccountName* und *AccountKey* im Azure-Portal im Speicherkontodashboard unter „Zugriffsschlüssel“.

    ```xml
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Speichern Sie die Datei ServiceConfiguration.Cloud.cscfg.
3. Öffnen Sie die Datei „ServiceConfiguration.Local.cscfg“, und prüfen Sie, ob „UseDevelopmentStorage“ auf „true“ festgelegt ist.

    ```xml
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
   Nachdem die Verbindungszeichenfolgen festgelegt sind, speichert Ihre Anwendung Diagnosedaten in Ihrem Speicherkonto, wenn die Anwendung bereitgestellt wird.
4. Speichern und Erstellen Sie das Projekt, und stellen Sie danach Ihre Anwendung bereit.

## <a name="step-2-optional-create-custom-performance-counters"></a>Schritt 2: (Optional) Erstellen benutzerdefinierter Leistungsindikatoren
Neben den vordefinierten Leistungsindikatoren können Sie Ihre eigenen benutzerdefinierten Leistungsindikator hinzufügen, um Web- oder Workerrollen zu überwachen. Mit benutzerdefinierten Leistungsindikatoren können Sie anwendungsspezifisches Verhalten verfolgen und überwachen. Diese Indikatoren können in einer Startaufgabe, einer Webrolle oder einer Workerrolle mit erhöhten Rechten erstellt oder gelöscht werden.

Der Azure-Diagnose-Agent aktualisiert die Leistungsindikatorkonfiguration über die WADCFG-Datei eine Minute nach dem Starten.  Falls Sie benutzerdefinierte Leistungsindikatoren in der OnStart-Methode erstellen und die Ausführung der Startaufgaben länger als eine Minute dauert, wurden Ihre benutzerdefinierten Leistungsindikatoren noch nicht erstellt, wenn der Azure-Diagnose-Agent versucht, sie zu laden.  In diesem Szenario sehen Sie, dass die Azure-Diagnose alle Diagnosedaten richtig erfasst, mit Ausnahme Ihrer benutzerdefinierten Leistungsindikatoren.  Um dieses Problem zu beheben, erstellen Sie die Leistungsindikatoren in einer Startaufgabe, oder verschieben Sie einen Teil des Startaufgabenaufwands in die OnStart-Methode, nachdem die Leistungsindikatoren erstellt wurden.

Führen Sie die folgenden Schritte aus, um einen einfachen benutzerdefinierten Leistungsindikator mit dem Namen "\MyCustomCounterCategory\MyButton1Counter" zu erstellen:

1. Öffnen Sie die Dienstdefinitionsdatei (CSDEF) für Ihre Anwendung.
2. Fügen Sie das Element Runtime dem Element WebRole oder WorkerRole hinzu, um eine Ausführung mit erhöhten Rechten zuzulassen:

    ```xml
    <runtime executioncontext="elevated"/>
    ```
3. Speichern Sie die Datei.
4. Öffnen Sie die Diagnosedatei („diagnostics.wadcfg“ in SDK 2.4 und früher oder „diagnostics.wadcfgx“ in SDK 2.5 und höher), und fügen Sie dem DiagnosticMonitorConfiguration-Element Folgendes hinzu:

    ```xml
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
      <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Speichern Sie die Datei.
6. Erstellen Sie die benutzerdefinierte Leistungsindikatorkategorie in der OnStart-Methode Ihrer Rolle, bevor base.OnStart aufgerufen wird. Mit dem folgenden C#-Beispiel wird eine benutzerdefinierte Kategorie erstellt, falls noch keine vorhanden ist:

    ```csharp
    public override bool OnStart()
    {
      if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
      {
         CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

         // add a counter tracking user button1 clicks
         CounterCreationData operationTotal1 = new CounterCreationData();
         operationTotal1.CounterName = "MyButton1Counter";
         operationTotal1.CounterHelp = "My Custom Counter for Button1";
         operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
         counterCollection.Add(operationTotal1);

         PerformanceCounterCategory.Create(
           "MyCustomCounterCategory",
           "My Custom Counter Category",
           PerformanceCounterCategoryType.SingleInstance, counterCollection);

         Trace.WriteLine("Custom counter category created.");
      }
      else {
        Trace.WriteLine("Custom counter category already exists.");
      }

    return base.OnStart();
    }
    ```
7. Aktualisieren Sie die Indikatoren in Ihrer Anwendung. Mit dem folgenden Beispiel wird ein benutzerdefinierter Leistungsindikator bei Button1_Click-Ereignissen aktualisiert:

    ```csharp
    protected void Button1_Click(object sender, EventArgs e)
    {
      PerformanceCounter button1Counter = new PerformanceCounter(
        "MyCustomCounterCategory",
        "MyButton1Counter",
        string.Empty,
        false);
      button1Counter.Increment();
      this.Button1.Text = "Button 1 count: " +
        button1Counter.RawValue.ToString();
    }
    ```
8. Speichern Sie die Datei.  

Benutzerdefinierte Leistungsindikatordaten werden vom Azure-Diagnosemonitor jetzt erfasst.

## <a name="step-3-query-performance-counter-data"></a>Schritt 3: Abfragen von Leistungsindikatordaten
Wenn Ihre Anwendung bereitgestellt ist und ausgeführt wird, beginnt der Diagnosemonitor mit dem Erfassen von Leistungsindikatoren und dem Speichern dieser Daten in Azure Storage. Mit Tools wie Server-Explorer in Visual Studio, [Azure-Speicher-Explorer](http://azurestorageexplorer.codeplex.com/) oder [Azure-Diagnose-Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) von Cerebrata zeigen Sie die Daten der Leistungsindikatoren in der Tabelle „WADPerformanceCountersTable“ an. Sie können den Tabellenspeicherdienst mit [C#](../storage/storage-dotnet-how-to-use-tables.md), [Java](../storage/storage-java-how-to-use-table-storage.md), [Node.js](../storage/storage-nodejs-how-to-use-table-storage.md), [Python](../storage/storage-python-how-to-use-table-storage.md), [Ruby](../storage/storage-ruby-how-to-use-table-storage.md) oder [PHP](../storage/storage-php-how-to-use-table-storage.md) auch programmgesteuert abfragen.

Das folgende C#-Beispiel zeigt eine einfache Abfrage der Tabelle WADPerformanceCountersTable und speichert die Diagnosedaten in einer CSV-Datei. Wenn Sie die Leistungsindikatoren in einer CSV-Datei speichern, können Sie die Daten mit den grafischen Darstellungsfunktionen von Microsoft Excel oder eines anderen Tools darstellen. Fügen Sie einen Verweis auf die Datei Microsoft.WindowsAzure.Storage.dll hinzu, die im Azure SDK für .NET Oktober 2012 und höher enthalten ist. Die Assembly wird im Verzeichnis „%Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\Versionsnummer\ref\“ installiert.

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Table;
...

// Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
// you store your connection string using the Microsoft Azure service configuration
// system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
// to retrieve your storage connection string.  If you're not using Cloud Services, it's
// recommended that you store the connection string in your web.config or app.config file.
// Use the ConfigurationManager type to retrieve your storage connection string.

string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
//string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

// Get a reference to the storage account using the connection string.  You can also use the development
// storage account (Storage Emulator) for local debugging.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
//CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

// Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
  .Where(
    TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
      TableOperators.And,
      TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
      TableOperators.And,
      TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
    )
  )
);

// Execute the table query.
IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

// Process the query results and build a CSV file.
StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

foreach (PerformanceCountersEntity entity in result)
{
  sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
    + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
}

StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
sw.Write(sb.ToString());
sw.Close();
```

Entitäten werden mithilfe einer aus **TableEntity**abgeleiteten benutzerdefinierten Klasse C#-Objekten zugeordnet. Der folgende Code definiert in der Tabelle **WADPerformanceCountersTable** eine Entitätsklasse, die einen Leistungsindikator darstellt.

```csharp
public class PerformanceCountersEntity : TableEntity
{
  public long EventTickCount { get; set; }
  public string DeploymentId { get; set; }
  public string Role { get; set; }
  public string RoleInstance { get; set; }
  public string CounterName { get; set; }
  public double CounterValue { get; set; }
}
```

## <a name="next-steps"></a>Nächste Schritte
[Weitere Artikel zu Azure-Diagnose](../azure-diagnostics.md)

