---
title: Geplante Ereignisse mit dem Azure-Metadatendienst | Microsoft-Dokumentation
description: "Reagieren Sie auf für Ihren virtuellen Computer weitreichende Ereignisse, ehe sie auftreten."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 28d8e1f2-8e61-4fbe-bfe8-80a68443baba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/10/2016
ms.author: zivr
ms.translationtype: Human Translation
ms.sourcegitcommit: e155891ff8dc736e2f7de1b95f07ff7b2d5d4e1b
ms.openlocfilehash: 7f0613285bc548e1329be3c33c30939f5998f379
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---
# <a name="azure-metadata-service---scheduled-events-preview"></a>Azure-Metadatendienst: Geplante Ereignisse (Vorschau)

> [!NOTE] 
> Die Vorschauen werden Ihnen zur Verfügung gestellt, wenn Sie die folgenden Nutzungsbedingungen akzeptieren. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen] (https://azure.microsoft.com/en-us/support/legal/preview-supplemental-terms/).
>

„Geplante Ereignisse“ ist einer der untergeordneten Dienste des Azure-Metadatendiensts, der Informationen zu anstehenden Ereignissen (z.B. Neustart) liefert, damit sich Ihre Anwendung darauf vorbereiten und Unterbrechungen begrenzen kann. Der Dienst steht für sämtliche Arten virtueller Azure-Computer zur Verfügung, einschließlich PaaS und IaaS. „Geplante Ereignisse“ räumt Ihrem virtuellen Computer Zeit ein, vorbeugende Maßnahmen zu ergreifen und die Auswirkungen eines Ereignisses zu minimieren. 


## <a name="introduction---why-scheduled-events"></a>Einführung: Gründe für geplante Ereignisse

Mit geplanten Ereignissen können Sie Maßnahmen ergreifen, um Auswirkungen auf Ihren Dienst zu begrenzen. Workloads mit mehreren Instanzen, die Replikationstechniken zur Statusverwaltung verwenden, sind ggf. anfällig für Ausfälle, die auf mehreren Instanzen auftreten. Solche Ausfälle können zu aufwendigen Aufgaben (z.B. Neuerstellung von Indizes) oder sogar zu einem Replikatsverlust führen. In vielen Fällen wird die allgemeine Dienstverfügbarkeit durch eine ordnungsgemäße Herunterfahrsequenz verbessert. Beispiel: Abschließen (oder Abbrechen) laufender Transaktionen, Neuzuweisen anderer Aufgaben zu anderen VMs im Cluster (manuelles Failover), Entfernen des virtuellen Computers aus dem Pool mit Lastenausgleich. Es gibt Fälle, bei denen eine Benachrichtigung eines Administrators über ein anstehendes Ereignis oder sogar die bloße Protokollierung eines Ereignisses die Wartbarkeit von Anwendungen verbessern kann, die in der Cloud gehostet werden.
Der Azure-Metadatendienst zeigt geplante Ereignisse in den folgenden Anwendungsfällen an:
-    Von der Plattform ausgelöste Wartungsaktionen (Beispiel: Rollout des Hostbetriebssystems)
-    Vom Benutzer ausgelöste Aufrufe (z.B. Neustart oder erneute Bereitstellung eines virtuellen Computers durch den Benutzer)


## <a name="scheduled-events---the-basics"></a>Geplante Ereignisse: Grundlagen  

Der Azure-Metadatendienst macht Informationen zu ausgeführten virtuellen Computern (VMs) mithilfe eines REST-Endpunkts innerhalb einer VM verfügbar. Die Informationen sind über eine nicht routbare IP-Adresse verfügbar, die außerhalb der VM nicht verfügbar gemacht wird.

### <a name="scope"></a>Umfang
Geplante Ereignisse werden allen virtuellen Computern in einem Clouddienst oder allen virtuellen Computern in einer Verfügbarkeitsgruppe angezeigt. Daher sollten Sie das Feld **Ressourcen** in einem Ereignis überprüfen, um zu bestimmen, welche VMs betroffen sein werden. 

### <a name="discover-the-endpoint"></a>Ermitteln des Endpunkts
Wenn ein virtueller Computer innerhalb eines virtuellen Netzwerks (VNet) erstellt wurde, ist der Metadatendienst über die nicht routingfähige IP-Adresse 169.254.169.254 verfügbar. In den Standardszenarien für Clouddienste und klassische virtuelle Computer ist eine zusätzliche Logik erforderlich, um den zu verwendenden Endpunkt zu ermitteln. In diesem Beispiel erfahren Sie, wie Sie den [Hostendpunkt ermitteln] (https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).

### <a name="versioning"></a>Versionsverwaltung 
Für den Instanzmetadatendienst wird die Versionsverwaltung genutzt. Versionen sind obligatorisch, und die aktuelle Version ist 2017-03-01.

> [!NOTE] 
> In früheren Vorschauversionen von geplanten Ereignissen wird {latest} als „api-version“ unterstützt. Dieses Format wird nicht mehr unterstützt und wird zukünftig veraltet sein.
>


### <a name="using-headers"></a>Verwenden von Headern
Zum Abfragen des Metadatendiensts müssen Sie den folgenden Header angeben: *Metadata: true*. 

### <a name="enable-scheduled-events"></a>Aktivieren geplanter Ereignisse
Wenn Sie geplante Ereignisse erstmals aufrufen, aktiviert Azure dieses Feature für Ihren virtuellen Computer implizit. Daher ist beim ersten Aufruf eine um bis zu zwei Minuten verzögerte Antwort zu erwarten.

### <a name="testing-your-logic-with-user-initiated-operations"></a>Testen der Logik mit vom Benutzer initiierten Vorgängen
Um die Logik zu testen, können Sie das Azure-Portal, eine Azure-API, die Azure-Befehlszeilenschnittstelle oder PowerShell verwenden, um Vorgänge zu initiieren, die zu geplanten Ereignissen führen. Der Neustart eines virtuellen Computers führt zu einem geplanten Ereignis, dessen Ereignistyp „Reboot“ entspricht. Die erneute Bereitstellung eines virtuellen Computers führt zu einem geplanten Ereignis, dessen Ereignistyp „Redeploy“ entspricht.
In beiden Fällen dauert der vom Benutzer initiierte Vorgang länger, da geplante Ereignisse einer Anwendung mehr Zeit für ein ordnungsgemäßes Herunterfahren einräumen. 

## <a name="using-the-api"></a>Verwenden der API

### <a name="query-for-events"></a>Abfragen von Ereignissen
Sie können geplante Ereignisse abfragen, indem Sie einfach folgenden Aufruf ausführen

    curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01


Eine Antwort enthält ein Array geplanter Ereignisse. Ein leeres Array bedeutet, dass derzeit keine Ereignisse geplant sind.
Sofern geplante Ereignisse vorliegen, enthält die Antwort ein Array mit Ereignissen: 

    {
     "DocumentIncarnation":{IncarnationID},
     "Events":[
          {
                "EventId":{eventID},
                "EventType":"Reboot" | "Redeploy" | "Freeze",
                "ResourceType":"VirtualMachine",
                "Resources":[{resourceName}],
                "EventStatus":"Scheduled" | "Started",
                "NotBefore":{timeInUTC},              
         }
     ]
    }

EventType: Erfasst die erwartete Auswirkung auf den virtuellen Computer. Dabei gilt:
- Freeze: Das Anhalten des virtuellen Computers für einige Sekunden ist geplant. Es gibt keine Auswirkungen auf Arbeitsspeicher, geöffnete Dateien oder Netzwerkverbindungen.
- Reboot: Der Neustart des virtuellen Computers ist geplant (der Arbeitsspeicher wird geleert).
- Redeploy: Das Verschieben des virtuellen Computers auf einen anderen Knoten ist geplant (der kurzlebige Datenträger geht verloren). 

Wenn ein Ereignis geplant ist (Status = Scheduled), gibt Azure die Uhrzeit bekannt, nach der das Ereignis starten kann (wird im Feld „NotBefore“ angegeben).

### <a name="starting-an-event-expedite"></a>(Beschleunigtes) Starten eines Ereignisses

Sobald Sie von einem anstehenden Ereignis erfahren und Ihre Logik für ein ordnungsgemäßes Herunterfahren vervollständigt haben, können Sie Azure (sofern möglich) beschleunigt arbeiten lassen, indem Sie einen Aufruf des Typs **POST** ausführen. 
 

## <a name="powershell-sample"></a>PowerShell-Beispiel 

Im folgenden Beispiel liest der Metadatenserver für geplante Ereignisse und stimmt diesen Ereignissen zu.

```PowerShell
# How to get scheduled events 
function GetScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How to approve a scheduled event
function ApproveScheduledEvent($eventId, $uri)
{    
    # Create the Scheduled Events Approval Json
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests} 
    $approvalString = ConvertTo-Json $scheduledEventsApproval

    Write-Host "Approving with the following: `n" $approvalString

    # Post approval string to scheduled events endpoint
    Invoke-RestMethod -Uri $uri -Headers @{"Metadata"="true"} -Method POST -Body $approvalString
}

# Add logic relevant to your service here
function HandleScheduledEvents($scheduledEvents)
{

}

######### Sample Scheduled Events Interaction #########

# Set up the scheduled events uri for VNET enabled VM
$localHostIP = "169.254.169.254"
$scheduledEventURI = 'http://{0}/metadata/scheduledevents?api-version=2017-03-01' -f $localHostIP 


# Get the document
$scheduledEvents = GetScheduledEvents $scheduledEventURI


# Handle events however is best for your service
HandleScheduledEvents $scheduledEvents


# Approve events when ready (optional)
foreach($event in $scheduledEvents.Events)
{
    Write-Host "Current Event: `n" $event
    $entry = Read-Host "`nApprove event? Y/N"
    if($entry -eq "Y" -or $entry -eq "y")
    {
    ApproveScheduledEvent $event.EventId $scheduledEventURI 
    }
}
``` 


## <a name="c-sample"></a>C\#-Beispiel 
Das folgende Codebeispiel gilt für einen Client, der APIs für die Kommunikation mit dem Metadatendienst verfügbar macht.
```csharp
   public class ScheduledEventsClient
    {
        private readonly string scheduledEventsEndpoint;
        private readonly string defaultIpAddress = "169.254.169.254"; 

        public ScheduledEventsClient()
        {
            scheduledEventsEndpoint = string.Format("http://{0}/metadata/scheduledevents?api-version=2017-03-01", defaultIpAddress);
        }
        /// Retrieve Scheduled Events 
        public string GetDocument()
        {
            Uri cloudControlUri = new Uri(scheduledEventsEndpoint);
            using (var webClient = new WebClient())
            {
                webClient.Headers.Add("Metadata", "true");
                return webClient.DownloadString(cloudControlUri);
            }   
        }

        /// Issues a post request to the scheduled events endpoint with the given json string
        public void PostResponse(string jsonPost)
        {
            using (var webClient = new WebClient())
            {
                webClient.Headers.Add("Content-Type", "application/json");
                webClient.UploadString(scheduledEventsEndpoint, jsonPost);
            }
        }
    }

```
Geplante Ereignisse können mit den folgenden Datenstrukturen analysiert werden. 

```csharp
    public class ScheduledEventsDocument
    {
        public List<CloudControlEvent> Events { get; set; }
    }

    public class CloudControlEvent
    {
        public string EventId { get; set; }
        public string EventStatus { get; set; }
        public string EventType { get; set; }
        public string ResourceType { get; set; }
        public List<string> Resources { get; set; }
        public DateTime NoteBefore { get; set; }
    }

    public class ScheduledEventsApproval
    {
        public List<StartRequest> StartRequests = new List<StartRequest>();
    }

    public class StartRequest
    {
        [JsonProperty("EventId")]
        private string eventId;

        public StartRequest(string eventId)
        {
            this.eventId = eventId;
        }
    }

```

Ein Beispielprogramm, das den Client zum Abrufen, Verarbeiten und Bestätigen von Ereignissen verwendet:   

```csharp
public class Program
    {
    static ScheduledEventsClient client;
    static void Main(string[] args)
    {
        while (true)
        {
            client = new ScheduledEventsClient();
            string json = client.GetDocument();
            ScheduledEventsDocument scheduledEventsDocument = JsonConvert.DeserializeObject<ScheduledEventsDocument>(json);

            HandleEvents(scheduledEventsDocument.Events);

            // Wait for user response
            Console.WriteLine("Press Enter to approve executing events\n");
            Console.ReadLine();

            // Approve events
            ScheduledEventsApproval scheduledEventsApprovalDocument = new ScheduledEventsApproval();
            foreach (CloudControlEvent ccevent in scheduledEventsDocument.Events)
            {
                scheduledEventsApprovalDocument.StartRequests.Add(new StartRequest(ccevent.EventId));
            }
            if (scheduledEventsApprovalDocument.StartRequests.Count > 0)
            {
                // Serialize using Newtonsoft.Json
                string approveEventsJsonDocument =
                    JsonConvert.SerializeObject(scheduledEventsApprovalDocument);

                Console.WriteLine($"Approving events with json: {approveEventsJsonDocument}\n");
                client.PostResponse(approveEventsJsonDocument);
            }

            Console.WriteLine("Complete. Press enter to repeat\n\n");
            Console.ReadLine();
            Console.Clear();
        }
    }

    private static void HandleEvents(List<CloudControlEvent> events)
    {
        // Add logic for handling events here
    }
}

```

## <a name="python-sample"></a>Python-Beispiel 

```python


#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url="http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01"
headers="{Metadata:true}"
this_host=socket.gethostname()

def get_scheduled_events():
   req=urllib2.Request(metadata_url)
   req.add_header('Metadata','true')
   resp=urllib2.urlopen(req)
   data=json.loads(resp.read())
   return data

def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid=evt['EventId']
        status=evt['EventStatus']
        resources=evt['Resources'][0]
        eventype=evt['EventType']
        restype=evt['ResourceType']
        notbefore=evt['NotBefore'].replace(" ","_")
        if this_host in evt['Resources'][0]:
            print "+ Scheduled Event. This host is scheduled for " + eventype + " not before " + notbefore
            print "++ Add you logic here"

def main():
   data=get_scheduled_events()
   handle_scheduled_events(data)
   

if __name__ == '__main__':
  main()
  sys.exit(0)


```
## <a name="next-steps"></a>Nächste Schritte 
[Geplante Wartung für virtuelle Computer in Azure](linux/planned-maintenance.md)
[Instanzmetadatendienst](virtual-machines-instancemetadataservice-overview.md)

