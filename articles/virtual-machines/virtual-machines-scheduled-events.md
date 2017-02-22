---
title: Geplante Ereignisse mit dem Azure-Metadatadienst | Microsoft-Dokumentation
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
translationtype: Human Translation
ms.sourcegitcommit: c7f552825f3230a924da6e5e7285e8fa7fa42842
ms.openlocfilehash: 541709ca17b96f8334e67dbdbbd9a10eefffa06b


---
# <a name="azure-metadata-service---scheduled-events"></a>Azure-Metadatadienst: Geplante Ereignisse

Der Azure-Metadatadienst ermöglicht Ihnen das Ermitteln von Informationen zu Ihrem in Azure gehosteten virtuellen Computer. „Geplante Ereignisse“, eine der verfügbar gemachten Kategorien, liefert Informationen zu anstehenden Ereignissen (z.B. Neustart), damit sich Ihre Anwendung darauf vorbereiten und Unterbrechungen begrenzen kann. Der Dienst steht für sämtliche Typen virtueller Azure-Computer zur Verfügung, einschließlich PaaS und IaaS. Der Dienst räumt Ihrem virtuellen Computer Zeit ein, vorbeugende Maßnahmen zu ergreifen und die Auswirkungen eines Ereignisses zu minimieren. Ihr Dienst kann beispielsweise, um Unterbrechungen zu vermeiden, Sitzungen ausgleichen, eine neue übergeordnete Instanz wählen oder Daten kopieren, nachdem erkannt wurde, dass bei einer Instanz ein Neustart geplant ist.



## <a name="introduction---why-scheduled-events"></a>Einführung: Gründe für geplante Ereignisse

Mithilfe geplanter Ereignisse können Sie anstehende Ereignisse ermitteln, die sich ggf. auf die Verfügbarkeit Ihres virtuellen Computers auswirken, und proaktive Maßnahmen ergreifen, um die Auswirkungen auf Ihren Dienst zu begrenzen.
Workloads mit mehreren Instanzen, die Replikationstechniken zur Statusverwaltung verwenden, sind ggf. anfällig für Ausfälle, die auf mehreren Instanzen auftreten. Solche Ausfälle können zu aufwendigen Aufgaben (z.B. Neuerstellung von Indizes) oder sogar zu einem Replikatsverlust führen.
In vielen Fällen wird die allgemeine Dienstverfügbarkeit durch eine ordnungsgemäße Herunterfahrsequenz verbessert. Beispiel: Abschließen (oder Abbrechen) laufender Transaktionen, Neuzuweisen anderer Aufgaben zu anderen VMs im Cluster (manuelles Failover), Entfernen des virtuellen Computers aus dem Pool mit Lastenausgleich.
Es gibt Fälle, bei denen eine Benachrichtigung eines Administrators über ein anstehendes Ereignis oder sogar die bloße Protokollierung eines Ereignisses die Wartbarkeit von Anwendungen verbessern kann, die in der Cloud gehostet werden.

Der Azure-Metadatadienst zeigt geplante Ereignisse in den folgenden Anwendungsfällen an:
-   Von der Plattform ausgelöste Wartung mit weitreichenden Auswirkungen (Beispiel: Einspielen des Hostbetriebssystems)
-   Von der Plattform ausgelöste Wartung mit nicht weitreichenden Auswirkungen (Beispiel: direkte VM-Migration)
-   Interaktive Aufrufe (z.B. Neustart oder Neubereitstellung einer VM durch den Benutzer)



## <a name="scheduled-events---the-basics"></a>Geplante Ereignisse: Grundlagen  

Der Azure-Metadatadienst macht Informationen zu ausgeführten virtuellen Computern (VMs) mithilfe eines REST-Endpunkts innerhalb einer VM verfügbar. Die Informationen sind über eine nicht routbare IP-Adresse verfügbar, die außerhalb der VM nicht verfügbar gemacht wird.

### <a name="scope"></a>Umfang 
Geplante Ereignisse werden allen virtuellen Computern in einem Clouddienst oder allen virtuellen Computern in einer Verfügbarkeitsgruppe angezeigt. Daher sollten Sie das Feld **Ressourcen** in einem Ereignis überprüfen, um zu bestimmen, welche VMs betroffen sein werden.

### <a name="discover-the-endpoint"></a>Ermitteln des Endpunkts
In Fällen, in denen ein virtueller Computer innerhalb eines virtuellen Netzwerks (VNet) erstellt wird, ist der Metadatendienst über die folgende nicht routbaren IP-Adresse verfügbar: 169.254.169.254

In Fällen, in denen ein virtueller Computer für Clouddienste (PaaS) verwendet wird, kann der Endpunkt des Metadatendiensts mithilfe der Registrierung ermittelt werden.

    {HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure\DeploymentManagement}

### <a name="versioning"></a>Versionsverwaltung 
Der Metadatendienst nutzt eine API mit Versionsangabe im folgenden Format: http://{IP-Adresse}/metadata/{Version}/scheduledevents Es wird empfohlen, dass der Dienst die neueste Version nutzt, die Sie unter dieser Adresse finden: http://{IP-Adresse}/metadata/latest/scheduledevents

### <a name="using-headers"></a>Verwenden von Headern
Zum Abfragen des Metadatendiensts müssen Sie den folgenden Header angeben: *Metadata: true*. 

### <a name="enable-scheduled-events"></a>Aktivieren geplanter Ereignisse
Wenn Sie geplante Ereignisse erstmals aufrufen, aktiviert Azure dieses Feature für Ihren virtuellen Computer implizit. Daher ist beim ersten Aufruf eine um bis zu einer Minute verzögerte Antwort zu erwarten. 


## <a name="using-the-api"></a>Verwenden der API

### <a name="query-for-events"></a>Abfragen von Ereignissen
Sie können geplante Ereignisse abfragen, indem Sie einfach folgenden Aufruf ausführen

    curl -H Metadata:true http://169.254.169.254/metadata/latest/scheduledevents

Eine Antwort enthält ein Array geplanter Ereignisse. Ein leeres Array bedeutet, dass derzeit keine Ereignisse geplant sind.
Sofern geplante Ereignisse vorliegen, enthält die Antwort ein Array mit Ereignissen: 

    {
     "Events":[
          {
                "EventId":{eventID},
                "EventType":"Reboot" | "Redeploy" | "Pause",
                "ResourceType":"VirtualMachine",
                "Resources":[{resourceName}],
                "EventStatus":"Scheduled" | "Started",
                "NotBefore":{timeInUTC},              
         }
     ]
    }

EventType: Erfasst die erwartete Auswirkung auf den virtuellen Computer. Dabei gilt:
- Pause: Das Anhalten des virtuellen Computers für einige Sekunden ist geplant. Es gibt keine Auswirkungen auf Arbeitsspeicher, geöffnete Dateien oder Netzwerkverbindungen.
- Reboot: Der Neustart des virtuellen Computers ist geplant (der Arbeitsspeicher wird geleert).
- Redeploy: Das Verschieben des virtuellen Computers auf einen anderen Knoten ist geplant (der kurzlebige Datenträger geht verloren). 

Wenn ein Ereignis geplant ist (Status = Scheduled), gibt Azure die Uhrzeit bekannt, nach der das Ereignis starten kann (wird im Feld „NotBefore“ angegeben).

### <a name="starting-an-event-expedite"></a>(Beschleunigtes) Starten eines Ereignisses

Sobald Sie von einem anstehenden Ereignis erfahren und Ihre Logik für ein ordnungsgemäßes Herunterfahren vervollständigt haben, können Sie Azure (sofern möglich) beschleunigt arbeiten lassen, indem Sie einen Aufruf des Typs **POST** ausführen. 
 

## <a name="powershell-sample"></a>PowerShell-Beispiel 

Im folgende Beispiel wird der Metadatenserver auf geplante Ereignisse überprüft, die vor dem Bestätigen im Anwendungsereignisprotokoll aufgezeichnet werden.

```PowerShell
$localHostIP = "169.254.169.254"
$ScheduledEventURI = "http://"+$localHostIP+"/metadata/latest/scheduledevents"

# Call Azure Metadata Service - Scheduled Events 
$scheduledEventsResponse =  Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $ScheduledEventURI -Method get 

if ($json.Events.Count -eq 0 )
{
    Write-Output "++No scheduled events were found"
}

for ($eventIdx=0; $eventIdx -lt $scheduledEventsResponse.Events.Length ; $eventIdx++)
{
    if ($scheduledEventsResponse.Events[$eventIdx].Resources[0].ToLower().substring(1) -eq $env:COMPUTERNAME.ToLower())
    {    
        # YOUR LOGIC HERE 
         pause "This Virtual Machine is scheduled for to "+ $scheduledEventsResponse.Events[$eventIdx].EventType

        # Acknoledge the event to expedite
        $jsonResp = "{""StartRequests"" : [{ ""EventId"": """+$scheduledEventsResponse.events[$eventIdx].EventId +"""}]}"
        $respbody = convertto-JSon $jsonResp
       
        Invoke-RestMethod -Uri $ScheduledEventURI  -Headers @{"Metadata"="true"} -Method POST -Body $jsonResp 
    }
}


``` 


## <a name="c-sample"></a>C\#-Beispiel 
Der folgende Code gilt für einen Client, der APIs für die Kommunikation mit dem Metadatendienst verfügbar macht.
```csharp
   public class ScheduledEventsClient
    {
        private readonly string scheduledEventsEndpoint;
        private readonly string defaultIpAddress = "169.254.169.254"; 

        public ScheduledEventsClient()
        {
            scheduledEventsEndpoint = string.Format("http://{0}/metadata/latest/scheduledevents", defaultIpAddress);
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

metadata_url="http://169.254.169.254/metadata/latest/scheduledevents"
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
[Geplante Wartung für virtuelle Computer in Azure](./virtual-machines-linux-planned-maintenance.md)



<!--HONumber=Jan17_HO1-->


