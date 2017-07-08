---
title: "Verwenden von Elasticsearch als Service Fabric-Ablaufverfolgungsspeicher für Anwendungen | Microsoft Docs"
description: "Beschreibt, wie Service Fabric-Anwendungen Elasticsearch und Kibana für die Speicherung, Indizierung und Suche auf der Grundlage von Anwendungsablaufverfolgungen (Protokollen) verwenden können."
services: service-fabric
documentationcenter: .net
author: karolz-ms
manager: adegeo
editor: 
ms.assetid: e59b0c39-e468-4d9e-b453-d5f2a8ad20d8
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: karolz@microsoft.com
redirect_url: /azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow
ms.translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 86c1b4dd0cc8833404bd7b66804fa67bbf0d119a
ms.contentlocale: de-de
ms.lasthandoff: 12/08/2016


---
# <a name="use-elasticsearch-as-a-service-fabric-application-trace-store"></a>Verwenden von Elasticsearch als Service Fabric-Anwendungsüberwachungsspeicher
## <a name="introduction"></a>Einführung
Dieser Artikel beschreibt, wie [Azure Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric/)-Anwendungen **Elasticsearch** und **Kibana** als Anwendungsüberwachungsspeicher sowie zur Indizierung und Suche verwenden können. [Elasticsearch](https://www.elastic.co/guide/index.html) ist ein verteiltes und skalierbares Open Source-Modul für Such- und Analysevorgänge in Echtzeit, das sich hervorragend für diese Aufgabe eignet. Es kann auf virtuellen, unter Microsoft Azure ausgeführten Windows- oder Linux-Computern installiert werden. Elasticsearch zeichnet sich durch eine effiziente Verarbeitung *strukturierter* Ablaufverfolgungen aus, die von Technologien wie **Ereignisablaufverfolgung für Windows**(Event Tracing for Windows, ETW) generiert werden.

ETW wird während der Service Fabric-Laufzeit verwendet, um Diagnoseinformationen (Ablaufverfolgungen) zu erfassen. Es ist auch die empfohlene Methode für Service Fabric-Anwendungen zum Erfassen ihrer Diagnoseinformationen. Die Verwendung des gleichen Mechanismus ermöglicht die Korrelation zwischen Ablaufverfolgungen der Laufzeit und Ablaufverfolgungen der Anwendung, was die Problembehandlung erleichtert. Die Service Fabric-Projektvorlagen in Visual Studio enthalten eine (auf der Klasse **EventSource** basierende) Protokollierungs-API, die standardmäßig ETW-Ablaufverfolgungen ausgibt. Eine allgemeine Übersicht der Service Fabric-Anwendungsablaufverfolgung mit ETW finden Sie unter [Überwachen und Diagnostizieren von Diensten in einer Entwicklungsumgebung auf einem lokalen Computer](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

Damit die Ablaufverfolgungsdaten in Elasticsearch angezeigt werden können, müssen sie in Echtzeit (also während der Anwendungsausführung) an den Service Fabric-Clusterknoten erfasst und an den Elasticsearch-Endpunkt gesendet werden. Für die Erfassung von Ablaufverfolgungen stehen zwei Hauptoptionen zur Verfügung:

* **In-Process-Ablaufverfolgungserfassung**  
  : Die Diagnosedaten werden von der Anwendung (genauer gesagt: vom Dienstprozess) an den Ablaufverfolgungsspeicher (Elasticsearch) gesendet.
* **Out-of-process trace capturing**  
  Die Ablaufverfolgungen vom Dienstprozess bzw. den Prozessen werden von einem separaten Agenten erfasst und an den Ablaufverfolgungsspeicher gesendet.

Im weiteren Verlauf wird beschrieben, wie Elasticsearch in Azure eingerichtet wird, welche Vor- und Nachteile die beiden Erfassungsoptionen haben, und wie Sie einen Service Fabric-Dienst für das Senden von Daten an Elasticsearch konfigurieren.

## <a name="set-up-elasticsearch-on-azure"></a>Einrichten von Elasticsearch für Azure
Am einfachsten lässt sich der Elasticsearch-Dienst für Azure mit [**Azure Resource Manager-Vorlagen**](../azure-resource-manager/resource-group-overview.md) einrichten. Eine umfassende [Azure-Ressourcen-Manager-Schnellstartvorlage für Elasticsearch](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) steht im Repository für Azure-Schnellstartvorlagen zur Verfügung. Diese Vorlage verwendet separate Speicherkonten für Skalierungseinheiten (Gruppen von Knoten). Sie kann auch separate Client- und Serverknoten mit unterschiedlichen Konfigurationen und unterschiedlicher Anzahl angehängter Datenträger bereitstellen.

Nachstehend werden wir eine andere Vorlage namens **ES-MultiNode** aus dem [Azure Diagnosetools-Repository](https://github.com/Azure/azure-diagnostics-tools)verwenden. Diese Vorlage ist leichter zu verwenden und erstellt einen Elasticsearch-Cluster, der mittels HTTP-Standardauthentifizierung geschützt ist. Bevor Sie fortfahren, laden Sie das Repository von GitHub auf Ihren Computer herunter (entweder durch Klonen des Repositorys oder durch Herunterladen einer ZIP-Datei). Die Vorlage „ES-MultiNode“ befindet sich im gleichnamigen Ordner.

### <a name="prepare-a-machine-to-run-elasticsearch-installation-scripts"></a>Vorbereiten eines Computers für die Ausführung von Elasticsearch-Installationsskripts
Am einfachsten lässt sich die Vorlage „ES-MultiNode“ über das bereitgestellte Azure PowerShell-Skript namens `CreateElasticSearchCluster`verwenden. Sie müssen PowerShell-Module und ein Tool namens **OpenSSL**installieren, um dieses Skript verwenden zu können. Letzteres wird zum Erstellen eines SSH-Schlüssels benötigt, der die Remoteverwaltung Ihres Elasticsearch-Clusters ermöglicht.

`CreateElasticSearchCluster` -Skript für die einfache Verwendung mit der Vorlage ES-MultiNode aus einem Windows-Computer entwickelt wurde. Die Vorlage kann zwar auch auf einem Windows-fremden Computer verwendet werden, ein solches Szenario ist jedoch nicht Gegenstand dieses Artikels.

1. Installieren Sie die [**Azure PowerShell-Module**](http://aka.ms/webpi-azps), sofern diese noch nicht installiert sind. Klicken Sie bei Aufforderung auf **Ausführen** und anschließend auf **Installieren**. Azure PowerShell 1.3 oder höher ist erforderlich.
2. Das Tool **openssl** ist in der Distribution von [**Git für Windows**](http://www.git-scm.com/downloads) enthalten. Installieren Sie jetzt [Git for Windows](http://www.git-scm.com/downloads) , sofern dies noch nicht installiert ist. (Die Standardinstallationsoptionen sind OK.)
3. Falls Git bereits installiert wurde, aber nicht im Systempfad enthalten ist, führen Sie in einem Fenster von Microsoft Azure PowerShell die folgenden Befehle aus:
   
    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```
   
    Ersetzen Sie `<Git installation folder>` durch den Speicherort von Git auf Ihrem Computer (standardmäßig **C:\Programme\Git**). Beachten Sie das Semikolon am Anfang des ersten Pfads.
4. Vergewissern Sie sich, dass Sie bei Azure angemeldet sind (über das Cmdlet [`Add-AzureRmAccount`](https://msdn.microsoft.com/library/mt619267.aspx) ), und dass Sie das Abonnement ausgewählt haben, das zum Erstellen Ihres Elasticsearch-Clusters verwendet werden soll. Mithilfe der Cmdlets `Get-AzureRmContext` und `Get-AzureRmSubscription` können Sie überprüfen, ob das richtige Abonnement ausgewählt wurde.
5. Ändern Sie das aktuelle Verzeichnis in den Ordner „ES-MultiNode“, sofern noch nicht geschehen.

### <a name="run-the-createelasticsearchcluster-script"></a>Ausführen des Skripts „CreateElasticSearchCluster“
Öffnen Sie vor dem Ausführen des Skripts die Datei `azuredeploy-parameters.json`, und überprüfen Sie die Werte für die Skriptparameter bzw. geben Sie entsprechende Werte an. Folgende Parameter stehen zur Verfügung:

| Parametername | Beschreibung |
| --- | --- |
| dnsNameForLoadBalancerIP |Dieser Name wird zum Erstellen des öffentlich sichtbaren DNS-Namens für den Elasticsearch-Cluster verwendet. Dabei wird die Azure-Regionsdomäne dem angegebenen Namen angehängt. Wenn der Parameterwert also beispielsweise myBigCluster lautet und als Azure-Region „USA, Westen“ ausgewählt ist, erhält der Cluster den DNS-Namen myBigCluster.westus.cloudapp.azure.com. <br /><br />Dieser Name fungiert auch als Namensstamm für viele Artefakte des Elasticsearch-Clusters (etwa für Datenknotennamen). |
| adminUsername |Der Name des Administratorkontos für die Verwaltung des Elasticsearch-Clusters (entsprechende SSH-Schlüssel werden automatisch generiert). |
| dataNodeCount |Die Anzahl von Knoten im Elasticsearch-Cluster. Bei der aktuellen Version des Skripts wird nicht zwischen Daten- und Abfrageknoten unterschieden. Alle Knoten übernehmen beide Rollen. Standardmäßig werden 3 Knoten verwendet. |
| dataDiskSize |Die Datenträgergröße (in GB), die für die einzelnen Datenknoten reserviert wird. Jeder Knoten erhält 4 Datenträger, die ausschließlich für den Elasticsearch-Dienst vorgesehen sind. |
| region |Der Name der Azure-Region, in der sich der Elasticsearch-Cluster befinden soll. |
| esUserName |Der Benutzername des Benutzers, der für den Zugriff auf den ES-Cluster (mit HTTP-Standardauthentifizierung) konfiguriert wird. Das Kennwort ist nicht Teil der Parameterdatei und muss angegeben werden, wenn das Skript `CreateElasticSearchCluster` aufgerufen wird. |
| vmSizeDataNodes |Die Größe des virtuellen Azure-Computers für Elasticsearch-Clusterknoten. Der Standardwert ist „Standard_D2“. |

Nun können Sie das Skript ausführen. Geben Sie den folgenden Befehl ein:

```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name> -Region <azure-region> -EsPassword <es-password>
```

Hierbei gilt: 

| Skriptparametername | Beschreibung |
| --- | --- |
| `<es-group-name>` |Der Name der Azure-Ressourcengruppe, die alle Elasticsearch-Clusterressourcen enthält. |
| `<azure-region>` |Der Name der Azure-Region, in der der Elasticsearch-Cluster erstellt werden soll. |
| `<es-password>` |Das Kennwort für den Elasticsearch-Benutzer. |

> [!NOTE]
> Falls das Cmdlet „Test-AzureResourceGroup“ eine Ausnahme vom Typ „NullReferenceException“ zurückgibt, haben Sie vergessen, sich bei Azure anzumelden (`Add-AzureRmAccount`).
> 
> 

Tritt bei der Skriptausführung ein Fehler auf, der auf einen falschen Vorlagenparameterwert zurückzuführen ist, korrigieren Sie die Parameterdatei, und führen Sie das Skript mit einem anderen Ressourcengruppennamen erneut aus. Sie können auch den gleichen Ressourcengruppennamen erneut verwenden und angeben, dass das Skript die alte Version bereinigen soll. Hierzu müssen Sie dem Skriptaufruf den Parameter `-RemoveExistingResourceGroup` hinzufügen.

### <a name="result-of-running-the-createelasticsearchcluster-script"></a>Ergebnis der Ausführung des Skripts „CreateElasticSearchCluster“
Nach der Ausführung des Skripts `CreateElasticSearchCluster` werden die folgenden Hauptartefakte erstellt. Bei diesem Beispiel wird davon ausgegangen, dass als Parameterwert für `dnsNameForLoadBalancerIP` myBigCluster verwendet und der Cluster in der Region „USA, Westen“ erstellt wurde.

| Artefakt | Name, Ort und Anmerkungen |
| --- | --- |
| SSH-Schlüssel für die Remoteverwaltung |Datei „myBigCluster.key“ (in dem Verzeichnis, in dem „CreateElasticSearchCluster“ ausgeführt wurde). <br /><br />Mit dieser Schlüsseldatei kann eine Verbindung mit dem Verwaltungsknoten und (über den Verwaltungsknoten) mit Datenknoten im Cluster hergestellt werden. |
| Verwaltungsknoten |myBigCluster-admin.westus.cloudapp.azure.com  <br /><br />Hierbei handelt es sich um einen dedizierten virtuellen Computer für die Remoteverwaltung des Elasticsearch-Clusters – des einzigen, der externe SSH-Verbindungen zulässt. Er wird im gleichen virtuellen Netzwerk ausgeführt wie die Elasticsearch-Clusterknoten, führt aber keine Elasticsearch-Dienste aus. |
| Datenknoten |myBigCluster1 … myBigCluster*N* <br /><br />Datenknoten, die Elasticsearch- und Kibana-Dienste ausführen. Sie können über SSH eine Verbindung mit den einzelnen Knoten herstellen (allerdings nur über den Verwaltungsknoten). |
| Elasticsearch-Cluster |http://myBigCluster.westus.cloudapp.azure.com/es/ <br /><br />Der primäre Endpunkt für den Elasticsearch-Cluster (beachten Sie das Suffix „/es“). Dieser ist durch die Standard-HTTP-Authentifizierung geschützt (die Anmeldeinformationen wurden mithilfe der Parameter „esUserName“/„esPassword“ der Vorlage „ES-MultiNode“ angegeben). Zur grundlegenden Clusterverwaltung ist für den Cluster auch das Head-Plug-In installiert (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head). |
| Kibana-Dienst |http://myBigCluster.westus.cloudapp.azure.com <br /><br />Der Kibana-Dienst wird zum Anzeigen von Daten aus dem erstellten Elasticsearch-Cluster eingerichtet. Er wird durch die gleichen Authentifizierungsanmeldeinformationen wie der Cluster selbst geschützt. |

## <a name="in-process-versus-out-of-process-trace-capturing"></a>Gegenüberstellung von In-Process- und Out-of-Process-Ablaufverfolgungserfassung
In der Einführung haben wir zwei grundlegende Arten für die Erfassung von Diagnosedaten angesprochen: In-Process und Out-of-Process. Beide Arten haben Vor- und Nachteile.

Vorteile der **In-Process-Ablaufverfolgungserfassung** :

1. *Einfache Konfiguration und Bereitstellung*
   
   * Die Konfiguration der Erfassung von Diagnosedaten ist Teil der Anwendungskonfiguration. Es ist einfach, sie stets mit dem Rest der Anwendung „synchron“ zu halten.
   * Eine anwendungs- oder dienstspezifische Konfiguration ist problemlos möglich.
   * Für die Out-of-Process-Ablaufverfolgungserfassung ist in der Regel eine separate Bereitstellung und Konfiguration des Diagnose-Agents erforderlich. Dies bedeutet eine zusätzliche Verwaltungsaufgabe und eine potenzielle Fehlerquelle. Die jeweilige Agent-Technologie lässt häufig nur eine Instanz des Agents pro virtuellem Computer (Knoten) zu. Dies bedeutet, dass die Konfiguration der Erfassung der Diagnosedaten von allen Anwendungen und Diensten, die auf diesem Knoten ausgeführt werden, gemeinsam verwendet wird.
2. *Flexibilität*
   
   * Die Anwendung kann die Daten an ein beliebiges Ziel senden – vorausgesetzt, es ist eine Clientbibliothek vorhanden, die das gewünschte Datenspeichersystem unterstützt. Bei Bedarf können neue Senken hinzugefügt werden.
   * Komplexe Erfassungs-, Filter- und Datenaggregierungsregeln können implementiert werden.
   * Die Out-of-Process-Ablaufverfolgungserfassung wird häufig durch die vom Agent unterstützten Datensenken beschränkt. Einige Agents sind erweiterbar.
3. *Zugriff auf interne Anwendungsdaten und Kontext*
   
   * Das Diagnosesubsystem innerhalb des Anwendungs-/Dienstprozesses kann die Ablaufverfolgungen problemlos mit Kontextinformationen ergänzen.
   * Bei der Out-of-Process-Methode müssen die Daten über einen prozessübergreifenden Kommunikationsmechanismus wie etwa die Ereignisablaufverfolgung für Windows an einen Agent gesendet werden. Dieser Mechanismus hat unter Umständen weitere Einschränkungen zur Folge.

Vorteile der **Out-of-Process-Ablaufverfolgungserfassung** :

1. *Die Möglichkeit der Überwachung der Anwendung und Sammlung von Absturzabbildern*
   
   * Die In-Process-Ablaufverfolgungserfassung ist unter Umständen nicht erfolgreich, wenn die Anwendung abstürzt oder nicht startet. Ein unabhängiger Agent kann hier deutlich besser entscheidende Problembehandlungsinformationen erfassen.<br /><br />
2. *Ausgereiftheit, Stabilität und bewährte Leistung*
   
   * Ein von einem Plattformanbieter entwickelter Agent (etwa der Microsoft Azure-Diagnose-Agent) wurde ausgiebig getestet und in der Praxis optimiert.
   * Bei der In-Process-Ablaufverfolgungserfassung muss darauf geachtet werden, dass das Senden von Diagnosedaten aus einem Anwendungsprozess nicht die Hauptaufgaben der Anwendung beeinträchtigt bzw. zu Timing- oder Leistungsproblemen führt. Ein unabhängig ausgeführter Agent ist hier weniger anfällig und speziell darauf ausgelegt, die Auswirkungen auf das System möglichst gering zu halten.

Natürlich ist es möglich, beide Ansätze zu kombinieren und von Ihnen zu profitieren. In der Tat ist dies möglicherweise die beste Lösung für viele Anwendungen.

Nachstehend verwenden wir die Bibliothek **Microsoft.Diagnostic.Listeners** und die In-Process-Ablaufverfolgungserfassung, um Daten aus einer Service Fabric-Anwendung an den Elasticsearch-Cluster zu senden.

## <a name="use-the-listeners-library-to-send-diagnostic-data-to-elasticsearch"></a>Senden von Diagnosedaten an Elasticsearch mithilfe der Listeners-Bibliothek
Die Bibliothek „Microsoft.Diagnostic.Listeners“ ist Teil der PartyCluster-Service Fabric-Beispielanwendung. Gehen Sie zur Verwendung wie folgt vor:

1. Laden Sie das [PartyCluster-Beispiel](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) von GitHub herunter.
2. Kopieren Sie jeweils den gesamten Ordner der Projekte „Microsoft.Diagnostics.Listeners“ und „Microsoft.Diagnostics.Listeners.Fabric“ aus dem Verzeichnis des PartyCluster-Beispiels in den Projektmappenordner der Anwendung, die die Daten an Elasticsearch senden soll.
3. Öffnen Sie die Ziellösung, klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektmappenknoten, und wählen Sie **Vorhandenes Projekt hinzufügen**aus. Fügen Sie der Projektmappe das Projekt „Microsoft.Diagnostics.Listeners“ hinzu. Wiederholen Sie den Vorgang für das Projekt „Microsoft.Diagnostics.Listeners.Fabric“.
4. Fügen Sie einen Projektverweis von Ihren Dienstprojekten zu den beiden hinzugefügten Projekten hinzu. (Jeder Dienst, der Daten an Elasticsearch senden soll, muss auf „Microsoft.Diagnostics.EventListeners“ und „Microsoft.Diagnostics.EventListeners.Fabric“ verweisen.)
   
    ![Projektverweise auf die Bibliotheken „Microsoft.Diagnostics.EventListeners“ und „Microsoft.Diagnostics.EventListeners.Fabric“][1]

### <a name="service-fabric-general-availability-release-and-microsoftdiagnosticstracing-nuget-package"></a>Das GA-Release von Service Fabric und Microsoft.Diagnostics.Tracing-NuGet-Paket
Anwendungen, die mit dem GA-Release von Service Fabric (2.0.135, veröffentlicht am 31. März 2016) erstellt wurden, richten sich an **.NET Framework 4.5.2**. Dies ist die höchste Version von .NET Framework, die von Azure zum Zeitpunkt der Veröffentlichung des GA-Releases unterstützt wurde. Leider fehlen bei dieser Framework-Version bestimmte EventListener-APIs, die für die Bibliothek „Microsoft.Diagnostics.Listeners“ benötigt werden. Da „EventSource“ (die Komponente, die die Grundlage für Protokollierungs-APIs in Fabric-Anwendungen bildet) und „EventListener“ eng zusammenhängen, muss jedes Projekt, das die Bibliothek „Microsoft.Diagnostics.Listeners“ verwendet, eine alternative Implementierung von „EventSource“ verwenden. Diese Implementierung wird durch das von Microsoft erstellte Paket **Microsoft.Diagnostics.Tracing NuGet** bereitgestellt. Dieses Paket ist vollständig mit „EventSource“ aus dem Framework abwärtskompatibel, sodass außer einer Änderung der referenzierten Namespaces keine weiteren Codeänderungen erforderlich sein sollten.

Führen Sie für jedes Dienstprojekt, das Daten an Elasticsearch senden soll, die folgenden Schritte aus, um mit der Verwendung der Microsoft.Diagnostics.Tracing-Implementierung der Klasse „EventSource“ zu beginnen:

1. Klicken Sie mit der rechten Maustaste auf das Dienstprojekt, und wählen Sie **NuGet-Pakete verwalten**aus.
2. Wechseln Sie zur Paketquelle „nuget.org“ (sofern nicht bereits ausgewählt), und suchen Sie nach**Microsoft.Diagnostics.Tracing**.
3. Installieren Sie das Paket `Microsoft.Diagnostics.Tracing.EventSource` (einschließlich seiner Abhängigkeiten).
4. Öffnen Sie in Ihrem Dienstprojekt die Datei **ServiceEventSource.cs** oder **ActorEventSource.cs**, und ersetzen Sie die Direktive `using System.Diagnostics.Tracing` am Dateianfang durch die Direktive `using Microsoft.Diagnostics.Tracing`.

Sobald **.NET Framework 4.6** von Microsoft Azure unterstützt wird, sind diese Schritte nicht mehr nötig.

### <a name="elasticsearch-listener-instantiation-and-configuration"></a>Instanziierung und Konfiguration des Elasticsearch-Listeners
Im letzten Schritt zum Senden von Diagnosedaten an Elasticsearch wird eine Instanz von `ElasticSearchListener` erstellt und mit Elasticsearch-Verbindungsdaten konfiguriert. Der Listener erfasst automatisch alle Ereignisse, die über die im Dienstprojekt definierten EventSource-Klassen ausgelöst werden. Da er während der Lebensdauer des Diensts aktiv sein muss, wird er am besten im Initialisierungscode des Diensts erstellt. Hier sehen Sie ein Beispiel für den Initialisierungscode eines zustandslosen Diensts nach den erforderlichen Änderungen (Ergänzungen sind in den Kommentaren mit `****`gekennzeichnet):

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Runtime;

// **** Add the following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>        
        private static void Main()
        {
            try
            {
                // **** Instantiate ElasticSearchListener
                var configProvider = new FabricConfigurationProvider("ElasticSearchEventListener");
                ElasticSearchListener esListener = null;
                if (configProvider.HasConfiguration)
                {
                    esListener = new ElasticSearchListener(configProvider, new FabricHealthReporter("ElasticSearchEventListener"));
                }

                // The ServiceManifest.XML file defines one or more service type names.
                // Registering a service maps a service type name to a .NET type.
                // When Service Fabric creates an instance of this service type,
                // an instance of the class is created in this host process.

                ServiceRuntime.RegisterServiceAsync("Stateless1Type", 
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                // Prevents this host process from terminating so services keep running.
                Thread.Sleep(Timeout.Infinite);

                // **** Ensure that the ElasticSearchListner instance is not garbage-collected prematurely
                GC.KeepAlive(esListener);
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e);
                throw;
            }
        }
    }
}
```

Die Elasticsearch-Verbindungsdaten müssen in einem separaten Abschnitt der Dienstkonfigurationsdatei (**PackageRoot\Config\Settings.xml**) platziert werden. Der Abschnittsname muss dem an den Konstruktor `FabricConfigurationProvider` übergebenen Wert entsprechen. Beispiel:

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
Die Werte der Parameter `serviceUri`, `userName` und `password` entsprechen der Endpunktadresse des Elasticsearch-Clusters bzw. dem Elasticsearch-Benutzernamen und dem Kennwort. `indexNamePrefix` ist das Präfix für Elasticsearch-Indizes. Die Bibliothek „Microsoft.Diagnostics.Listeners“ erstellt täglich einen neuen Index für die Daten.

### <a name="verification"></a>Überprüfung
Das ist alles! Wenn der Dienst nun ausgeführt wird, sendet er Ablaufverfolgungen an den in der Konfiguration angegebenen Elasticsearch-Dienst. Sie können dies überprüfen, indem Sie die Kibana-Benutzeroberfläche öffnen, die der Elasticsearch-Zielinstanz zugeordnet ist. In unserem Beispiel lautet die Seitenadresse http://myBigCluster.westus.cloudapp.azure.com/. Überprüfen Sie, ob tatsächlich Indizes mit dem für die `ElasticSearchListener` -Instanz ausgewählten Namenspräfix erstellt und mit Daten aufgefüllt wurden.

![Kibana mit PartyCluster-Anwendungsereignissen][2]

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zur Diagnose und Überwachung eines Service Fabric-Diensts](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png

