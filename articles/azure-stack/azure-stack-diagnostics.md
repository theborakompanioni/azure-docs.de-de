---
title: Diagnose in Azure Stack | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie Protokolldateien für die Diagnose in Azure Stack sammeln."
services: azure-stack
documentationcenter: 
author: adshar
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: adshar
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: 70004cfd83360ac4c66fd4c90632d341709d2e6f
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="azure-stack-diagnostics-tools"></a>Azure Stack-Diagnosetools
 
Bei Azure Stack handelt es sich um eine große Sammlung von Komponenten, die zusammenarbeiten und miteinander interagieren. Alle diese Komponenten generieren ihre eigenen eindeutigen Protokolle. Dies bedeutet, dass das Diagnostizieren von Problemen schnell zu einer anspruchsvollen Aufgabe werden kann (besonders für Fehler, die von mehreren interagierenden Azure Stack-Komponenten stammen). 

Mit unseren Diagnosetools wird sichergestellt, dass der Mechanismus für die Protokollsammlung einfach und effizient ist. Im folgenden Diagramm ist dargestellt, wie Tools zum Sammeln von Protokollen in Azure Stack funktionieren:

![Tools für die Protokollsammlung](media/azure-stack-diagnostics/image01.png)
 
 
## <a name="trace-collector"></a>Collector der Ablaufverfolgung
 
Der Collector der Ablaufverfolgung ist standardmäßig aktiviert. Er wird fortlaufend im Hintergrund ausgeführt und sammelt von den Komponentendiensten in Azure Stack Protokolle vom Typ „Ereignisablaufverfolgung für Windows“ (Event Tracing for Windows, ETW) und speichert sie auf einer gemeinsamen lokalen Freigabe. 

Hier sind einige wichtige Informationen zum Collector der Ablaufverfolgung aufgeführt:
 
* Der Collector der Ablaufverfolgung wird fortlaufend mit Standardgrößenbeschränkungen ausgeführt. Die zulässige maximale Standardgröße jeder Datei (200 MB) ist **keine** endgültige Größe. Die Größe wird regelmäßig überprüft (derzeit alle 10 Minuten), und wenn die aktuelle Datei größer als 200 MB ist, wird sie gespeichert, und es wird eine neue Datei generiert. Außerdem gilt ein (konfigurierbarer) Grenzwert von 8 GB für die Gesamtdateigröße, die pro Ereignissitzung generiert werden kann. Nachdem dieser Grenzwert erreicht wurde, werden die ältesten Dateien gelöscht, wenn neue Dateien erstellt werden.
* Für die Protokolle gilt ein Altersgrenzwert von fünf Tagen. Dieser Grenzwert kann ebenfalls konfiguriert werden. 
* Jede Komponente definiert die Eigenschaften für die Ablaufverfolgungskonfiguration mit einer JSON-Datei. Die JSON-Dateien werden unter `C:\TraceCollector\Configuration` gespeichert. Bei Bedarf können diese Dateien bearbeitet werden, um die Grenzwerte für Alter und Größe für die gesammelten Protokolle zu ändern. Für Änderungen an diesen Dateien ist ein Neustart des Diensts *Microsoft Azure Stack-Collector für die Ablaufverfolgung* erforderlich, damit die Änderungen wirksam werden.
* Das folgende Beispiel ist eine JSON-Datei für die Konfiguration der Ablaufverfolgung für FabricRingServices Operations über die XRP-VM: 

```
{
    "LogFile": 
    {
        "SessionName": "FabricRingServicesOperationsLogSession",
        "FileName": "\\\\SU1FileServer\\SU1_ManagementLibrary_1\\Diagnostics\\FabricRingServices\\Operations\\AzureStack.Common.Infrastructure.Operations.etl",
        "RollTimeStamp": "00:00:00",
        "MaxDaysOfFiles": "5",
        "MaxSizeInMB": "200",
        "TotalSizeInMB": "5120"
    },
    "EventSources":
    [
        {"Name": "Microsoft-AzureStack-Common-Infrastructure-ResourceManager" },
        {"Name": "Microsoft-OperationManager-EventSource" },
        {"Name": "Microsoft-Operation-EventSource" }
    ]
}
```

* **MaxDaysOfFiles**

    Mit diesem Parameter wird das Alter der beizubehaltenden Dateien gesteuert. Ältere Protokolldateien werden gelöscht.
* **MaxSizeInMB**

    Mit diesem Parameter wird der Größenschwellenwert für eine einzelne Datei gesteuert. Wenn die jeweilige Größe erreicht ist, wird eine neue ETL-Datei erstellt.
* **TotalSizeInMB**

    Mit diesem Parameter wird die Gesamtgröße der ETL-Dateien gesteuert, die für eine Ereignissitzung generiert werden. Wenn die Gesamtdateigröße größer als dieser Parameterwert ist, werden ältere Dateien gelöscht.
  
## <a name="log-collection-tool"></a>Tool für die Protokollsammlung
 
Der PowerShell-Befehl `Get-AzureStackLog` kann verwendet werden, um Protokolle für alle Komponenten einer Azure Stack-Umgebung zu sammeln. Sie werden an einem vom Benutzer angegebenen Ort in ZIP-Dateien gespeichert. Wenn unser Team für den technischen Support Ihre Protokolle für die Behandlung eines Problems benötigt, werden Sie unter Umständen gebeten, dieses Tool auszuführen.

> [!CAUTION]
> Es kann sein, dass diese Protokolldateien personenbezogene Informationen enthalten. Berücksichtigen Sie dies, bevor Sie Protokolldateien öffentlich bereitstellen.
 
Wir sammeln derzeit die folgenden Protokolltypen:
*   **Azure Stack-Bereitstellungsprotokolle**
*   **Windows-Ereignisprotokolle**
*   **Panther-Protokolle**

     Für die Behandlung von Problemen bei der VM-Erstellung
*   **Clusterprotokolle**
*   **Speicherdiagnoseprotokolle**
*   **ETW-Protokolle**

    Diese werden vom Collector für die Ablaufverfolgung gesammelt und auf einer Freigabe gespeichert, auf der sie von `Get-AzureStackLog` abgerufen werden.
 
Verwenden Sie die `<Logs>`-Tags in der Datei für die Kundenkonfiguration unter `C:\EceStore\<Guid>\<GuidWithMaxFileSize>`, um alle Protokolle zu identifizieren, die für die Komponenten gesammelt werden.
 
### <a name="to-run-get-azurestacklog"></a>So führen Sie Get-AzureStackLog aus
1.  Melden Sie sich auf dem Host als „AzureStack\AzureStackAdmin“ an.
2.  Öffnen Sie ein PowerShell-Fenster als Administrator.
3.  Führen Sie `Get-AzureStackLog`aus.  

    **Beispiele**

    - Sammeln Sie alle Protokolle für alle Rollen:

        `Get-AzureStackLog -OutputPath C:\AzureStackLogs`

    - Sammeln Sie Protokolle für die Rollen VirtualMachines und BareMetal:

        `Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal`

    - Sammeln Sie Protokolle für die Rollen VirtualMachines und BareMetal, und verwenden Sie die Datumsfilterung für Protokolldateien für die letzten acht Stunden:

        `Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date)`

Wenn die Parameter `FromDate` und `ToDate` nicht angegeben sind, werden standardmäßig Protokolle für die letzten vier Stunden gesammelt.

Derzeit können Sie den Parameter `FilterByRole` verwenden, um die Protokollsammlung nach den folgenden Rollen zu filtern:

|   |   |   |
| - | - | - |
| `ACSMigrationService`     | `ACSMonitoringService`   | `ACSSettingsService` |
| `ACS`                     | `ACSFabric`              | `ACSFrontEnd`        |
| `ACSTableMaster`          | `ACSTableServer`         | `ACSWac`             |
| `ADFS`                    | `ASAppGateway`           | `BareMetal`          |
| `BRP`                     | `CA`                     | `CPI`                |
| `CRP`                     | `DeploymentMachine`      | `DHCP`               |
|`Domain`                   | `ECE`                    | `ECESeedRing`        |        
| `FabricRing`              | `FabricRingServices`     | `FRP`                |
|` Gateway`                 | `HealthMonitoring`       | `HRP`                |               
| `IBC`                     | `InfraServiceController` | `KeyVaultAdminResourceProvider`|
| `KeyVaultControlPlane`    | `KeyVaultDataPlane`      | `NC`                 |            
| `NonPrivilegedAppGateway` | `NRP`                    | `SeedRing`           |
| `SeedRingServices`        | `SLB`                    | `SQL`                |     
| `SRP`                     | `Storage`                | `StorageController`  |
| `URP`                     | `UsageBridge`            | `VirtualMachines`    |  
| `WAS`                     | `WASPUBLIC`              | `WDS`                |


Im Folgenden einige Hinweise, die Sie beachten sollten:

* Je nachdem, welche Rollenprotokolle gesammelt werden, kann dieser Befehl für die Protokollsammlung etwas mehr Zeit beanspruchen. Zu den entscheidenden Faktoren gehören der Zeitraum, der für die Protokollsammlung angegeben wurde, und die Anzahl von Knoten in der Azure Stack-Umgebung.
* Überprüfen Sie nach Abschluss der Protokollsammlung den neuen Ordner, der mit dem Parameter `-OutputPath` des Befehls erstellt wird.
* Eine Datei mit dem Namen `Get-AzureStackLog_Output.log` wird in dem Ordner erstellt, der die ZIP-Dateien enthält. Sie enthält die Befehlsausgabe, die zum Beheben von Fehlern bei der Protokollsuche verwendet werden kann.
* Für jede Rolle sind die Protokolle in einer separaten ZIP-Datei enthalten. 
* Zum Untersuchen eines bestimmten Fehlers werden unter Umständen die Protokolle von mehr als einer Komponente benötigt.
    -   System- und Ereignisprotokolle für alle Infrastruktur-VMs werden unter der Rolle *VirtualMachines* gesammelt.
    -   System- und Ereignisprotokolle für alle Hosts werden unter der Rolle *BareMetal* gesammelt.
    -   Failovercluster- und Hyper-V-Ereignisprotokolle werden unter der Rolle *Storage* gesammelt.
    -   ACS-Protokolle werden unter den Rollen *Storage* und *ACS* gesammelt.
* Ausführliche Informationen finden Sie in der Datei für die Kundenkonfiguration. Untersuchen Sie die `<Logs>`-Tags für die unterschiedlichen Rollen.

> [!NOTE]
> Wir erzwingen Größen- und Altersgrenzwerte für die gesammelten Protokolle, da es sehr wichtig ist, für eine effiziente Nutzung Ihres Speicherplatzes zu sorgen. So wird sichergestellt, dass keine übermäßig hohe Zahl von Protokollen gespeichert wird. Andererseits gilt Folgendes: Beim Diagnostizieren eines Problems benötigen Sie häufig Protokolle, die unter Umständen nicht mehr vorhanden sind, weil diese Grenzwerte erzwungen werden. Daher wird **dringend empfohlen**, Ihre Protokolle alle acht bis zwölf Stunden in einen externen Speicher zu verlagern (Speicherkonto auf öffentlicher Azure-Instanz, zusätzliches lokales Speichergerät usw.) und je nach Ihren Anforderungen dort ein bis drei Monate lang aufzubewahren.


## <a name="next-steps"></a>Nächste Schritte
[Microsoft Azure Stack troubleshooting (Problembehandlung für Microsoft Azure Stack)](azure-stack-troubleshooting.md)

