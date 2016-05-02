<properties
 pageTitle="Automatische Skalierung von Computeressourcen im HPC-Cluster | Microsoft Azure"
 description="Automatisches Vergrößern und Verkleinern der Anzahl der HPC Pack-Cluster-Compute-Knoten in Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="04/14/2016"
 ms.author="danlep"/>

# Automatisches Vergrößern oder Verkleinern der HPC Pack-Clusterressourcen in Azure gemäß der Clusterworkload




Wenn Sie Azure-Burstknoten im HPC Pack-Cluster bereitstellen oder einen HPC Pack-Cluster in virtuellen Azure-Computern erstellen, möchten Sie möglicherweise die Anzahl der Azure-Computeressourcen, wie Kerne, entsprechend der aktuellen Workload im Cluster automatisch vergrößern oder verkleinern. Auf diese Weise können Sie die Azure-Ressourcen effizienter nutzen und die Kosten kontrollieren. Richten Sie zu diesem Zweck die HPC Pack-Cluster-Eigenschaft **AutoGrowShrink** ein. Führen Sie hierzu das HPC PowerShell-Skript **AzureAutoGrowShrink.ps1** aus, das mit HPC Pack installiert wird.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ressourcen-Manager-Modell. Darüber hinaus können Sie derzeit nur die HPC Pack-Computeknoten automatisch vergrößern oder verkleinern, auf denen ein Windows Server-Betriebssystem ausgeführt wird.

## Festlegen der AutoGrowShrink-Clustereigenschaft

### Voraussetzungen

* **HPC Pack 2012 R2 Update 2 oder späteres Cluster** – Der Clusterhauptknoten kann entweder lokal oder auf einem virtuellen Azure-Computer bereitgestellt werden. Informationen für die ersten Schritte mit einem lokalen Hauptknoten und Azure-Burstknoten finden Sie unter [Einrichten eines Hybrid-Rechenclusters mit Microsoft HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Informationen zur schnellen Bereitstellung eines HPC Pack-Clusters auf virtuellen Azure-Computern finden Sie im [HPC Pack IaaS-Bereitstellungsskripts](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).


* **Cluster mit einem Hauptknoten in Azure** – Wenn Sie das HPC Pack-IaaS-Bereitstellungsskript verwenden, um den Cluster zu erstellen, aktivieren Sie die **AutoGrowShrink**-Clustereigenschaft durch Festlegen der AutoGrowShrink-Option in der Clusterkonfigurationsdatei. Details finden Sie in der Dokumentation, die beim [Skriptdownload](https://www.microsoft.com/download/details.aspx?id=44949) bereitgestellt wird.

    Legen Sie alternativ die **AutoGrowShrink**-Clustereigenschaft nach der Bereitstellung des Clusters mit den HPC PowerShell-Befehlen fest, die im folgenden Abschnitt beschrieben werden. Führen Sie zunächst die folgenden Schritte aus, um dafür HPC PowerShell zu verwenden:
    1. Konfigurieren Sie ein Azure-Verwaltungszertifikat auf dem Hauptknoten und im Azure-Abonnement. Für eine Testbereitstellung können Sie das selbstsignierte Standard-Microsoft HPC Azure-Zertifikat verwenden, das mit dem HPC Pack auf dem Hauptknoten installiert wird. Laden Sie dieses Zertifikat dann einfach in das Azure-Abonnement hoch. Optionen und Schritte finden Sie in den [Anleitungen der TechNet-Bibliothek](https://technet.microsoft.com/library/gg481759.aspx).
    2. Führen Sie **regedit** für den Hauptknoten aus, gehen Sie zu „HKLM\\SOFTWARE\\Microsoft\\HPC\\IaasInfo“, und fügen Sie einen neuen Zeichenfolgenwert hinzu. Legen Sie den Namen des Werts auf „Fingerabdruck“ und die Daten des Werts auf den Fingerabdruck des Zertifikats von Schritt 1 fest.


### HPC PowerShell-Befehle zum Festlegen der AutoGrowShrink-Eigenschaft

Im Folgenden finden Sie HPC PowerShell-Beispielbefehle zum Festlegen von **AutoGrowShrink** und zum Optimieren des Verhaltens mit zusätzlichen Parametern. Unter [AutoGrowShrink-Parameter](#AutoGrowShrink-parameters) weiter unten in diesem Artikel finden Sie eine vollständige Liste der Einstellungen.

Starten Sie zum Ausführen dieser Befehle HPC PowerShell auf dem Clusterhauptknoten als Administrator.

**So aktivieren Sie die AutoGrowShrink-Eigenschaft**

    Set-HpcClusterProperty –EnableGrowShrink 1

**So deaktivieren Sie die AutoGrowShrink-Eigenschaft**

    Set-HpcClusterProperty –EnableGrowShrink 0

**So ändern Sie das Vergrößerungsintervall in Minuten**

    Set-HpcClusterProperty –GrowInterval <interval>

**So ändern Sie das Verkleinerungsintervall in Minuten**

    Set-HpcClusterProperty –ShrinkInterval <interval>

**So zeigen Sie die aktuelle Konfiguration von AutoGrowShrink an**

    Get-HpcClusterProperty –AutoGrowShrink

### AutoGrowShrink-Parameter

Im Folgenden werden AutoGrowShrink-Parameter aufgeführt, die Sie mit dem **Set-HpcClusterProperty**-Befehl ändern können.

* **EnableGrowShrink** – Schalter zum Aktivieren oder Deaktivieren der **AutoGrowShrink**-Eigenschaft.
* **ParamSweepTasksPerCore** – Anzahl der Parameter-Sweep-Aufgaben zum Vergrößern eines Kerns. Standardmäßig wird ein Kern pro Aufgabe vergrößert. 
 
    >[AZURE.NOTE] HPC Pack QFE KB3134307 ändert **ParamSweepTasksPerCore** in **TasksPerResourceUnit**. Dies basiert auf dem Auftragsressourcentyp, wobei es sich um einen Knoten, einen Socket oder einen Kern handeln kann.
    
* **GrowThreshold** – Schwellenwert für Aufgaben in der Warteschlange zum Auslösen der automatischen Vergrößerung. Der Standardwert ist 1, d. h., wenn es mehr als eine Aufgabe in der Warteschlange gibt, werden die Knoten automatisch vergrößert.
* **GrowInterval** – Intervall in Minuten zum Auslösen der automatischen Vergrößerung. Das Standardintervall beträgt 5 Minuten.
* **ShrinkInterval** – Intervall in Minuten zum Auslösen der automatischen Verkleinerung. Das Standardintervall beträgt 5 Minuten.
* **ShrinkIdleTimes** – Anzahl der fortlaufenden Verkleinerungsüberprüfungen, um anzugeben, dass die Knoten inaktiv sind. Der Standardwert ist 3 Mal. Wenn **ShrinkInterval** 5 Minuten beträgt, überprüft HPC Pack beispielsweise alle 5 Minuten, ob der Knoten inaktiv ist. Wenn die Knoten nach 3 fortlaufenden Prüfungen (15 Minuten) inaktiv sind, verkleinert HPC Pack diesen Knoten.
* **ExtraNodesGrowRatio** – Zusätzlicher Prozentsatz an zu vergrößernden Knoten für Aufträge des Message Passing Interface (MPI). Der Standardwert ist 1, d. h. dass die MPI-Aufträge durch das HPC Pack um 1 % zusätzliche Knoten erweitert werden. 
* **GrowByMin** – Schalter, um anzugeben, ob die Richtlinie für die automatische Vergrößerung auf den für den Auftrag erforderlichen Mindestressourcen basiert. Der Standardwert ist „false“, was bedeutet, dass das HPC Pack Knoten für Aufträge basierend auf den für die Aufträge erforderlichen Maximalressourcen vergrößert.
* **SoaJobGrowThreshold** – Schwellenwert von eingehenden SOA-Anforderungen zum Auslösen des automatischen Vergrößerungsprozesses. Der Standardwert ist 50000.  
    
    >[AZURE.NOTE] Dieser Parameter wird ab HPC Pack 2012 R2 Update 3 unterstützt.
    
* **SoaRequestsPerCore** – Anzahl der eingehenden SOA-Anforderungen zum Vergrößern eines Kerns. Der Standardwert ist 20000.

    >[AZURE.NOTE] Dieser Parameter wird ab HPC Pack 2012 R2 Update 3 unterstützt.

### MPI-Beispiel

Standardmäßig werden die MPI-Aufträge durch das HPC Pack um 1 % zusätzliche Knoten erweitert (**ExtraNodesGrowRatio** ist auf 1 festgelegt). Der Grund dafür ist, dass MPI möglicherweise mehrere Knoten benötigt und dass der Auftrag nur ausgeführt werden kann, wenn alle Knoten bereit sind. Wenn Azure Knoten startet, benötigt ein Knoten gelegentlich mehr Zeit als andere zum Starten, wodurch andere Knoten inaktiv sind, während sie darauf warten, dass dieser Knoten bereit ist. Durch die Erweiterung mit zusätzlichen Knoten, reduziert HPC Pack die Wartezeit für diese Ressource und spart potenziell Kosten. Um den Prozentsatz an zusätzlichen Knoten für MPI-Aufträge (z. B. auf 10 %) zu erhöhen, führen Sie einen Befehl ähnlich dem folgenden aus:

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### SOA-Beispiel

Standardmäßig ist **SoaJobGrowThreshold** auf 50000 und **SoaRequestsPerCore** auf 20000 festgelegt. Wenn Sie einen SOA-Auftrag mit 70000 Anforderungen senden, gibt es eine Aufgabe in der Warteschlange. Die Anzahl der eingehenden Anforderungen beträgt 70000. In diesem Fall vergrößert der HPC Pack 1 Kern für die Aufgabe in der Warteschlange und vergrößert (70000 - 50000)/20000 = 1 Kern für eingehende Anforderungen, insgesamt werden also 2 Kerne für diesen SOA-Auftrag vergrößert.

## Ausführen des AzureAutoGrowShrink.ps1-Skripts

### Voraussetzungen

* **Cluster mit HPC Pack 2012 R2 Update 1 oder höher** – Das Skript **AzureAutoGrowShrink.ps1** ist im Ordner "%CCP\_HOME%bin" installiert. Der Clusterhauptknoten kann entweder lokal oder auf einem virtuellen Azure-Computer bereitgestellt werden. Informationen für die ersten Schritte mit einem lokalen Hauptknoten und Azure-Burstknoten finden Sie unter [Einrichten eines Hybrid-Rechenclusters mit Microsoft HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Informationen zur schnellen Bereitstellung eines HPC Pack-Clusters auf virtuellen Azure-Computern finden Sie im [HPC Pack IaaS-Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). Alternativ können Sie eine [Azure-Schnellstartvorlage](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) nutzen.

* **Azure PowerShell 0.8.12**: Das Skript hängt derzeit von dieser bestimmten Version von Azure PowerShell ab. Wenn Sie eine höhere Version auf dem Hauptknoten ausführen, müssen Sie Azure PowerShell möglicherweise auf [Version 0.8.12](http://az412849.vo.msecnd.net/downloads03/azure-powershell.0.8.12.msi) herunterstufen, um das Skript ausführen zu können.

* **Für einen Cluster mit Azure-Burstknoten** – Führen Sie das Skript auf einem Clientcomputer, auf dem HPC Pack installiert ist, oder auf dem Hauptknoten aus. Wenn Sie das Skript auf einem Clientcomputer ausführen, stellen Sie sicher, dass Sie die Variable "$env:CCP\_SCHEDULER" so festlegen, dass sie auf den Hauptknoten verweist. Die Azure-Burstknoten müssen dem Cluster bereits hinzugefügt sein, sie können jedoch den Status "Nicht bereitgestellt" aufweisen.


* **Für einen auf virtuellen Azure-Computern bereitgestellten Cluster** – Führen Sie das Skript auf dem virtuellen Computer für den Hauptknoten aus, da er von den Skripts **Start HpcIaaSNode.ps1** und **Stop HpcIaaSNode.ps1** abhängt, die dort installiert sind. Für diese Skripts ist zudem ein Azure-Verwaltungszertifikat oder eine Azure-Veröffentlichungseinstellungsdatei erforderlich (siehe [Manage the number and availability of compute nodes in an HPC Pack cluster in Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md) (in englischer Sprache)). Stellen Sie sicher, dass alle erforderlichen virtuellen Maschinen für Computeknoten dem Cluster bereits hinzugefügt sind. Sie können sich jedoch im Status „Beendet“ befinden.

### Syntax

```
AzureAutoGrowShrink.ps1
[[-NodeTemplates] <String[]>] [[-JobTemplates] <String[]>] [[-NodeType] <String>]
[[-NumOfQueuedJobsPerNodeToGrow] <Int32>]
[[-NumOfQueuedJobsToGrowThreshold] <Int32>]
[[-NumOfActiveQueuedTasksPerNodeToGrow] <Int32>]
[[-NumOfActiveQueuedTasksToGrowThreshold] <Int32>]
[[-NumOfInitialNodesToGrow] <Int32>] [[-GrowCheckIntervalMins] <Int32>]
[[-ShrinkCheckIntervalMins] <Int32>] [[-ShrinkCheckIdleTimes] <Int32>]
[-UseLastConfigurations] [[-ArgFile] <String>] [[-LogFilePrefix] <String>]
[<CommonParameters>]

```
### Parameter

 * **NodeTemplates** – Namen der Knotenvorlagen zum Definieren des Bereichs, in dem sich die Knoten vergrößern und verkleinern. Wenn kein Bereich angegeben ist (der Standardwert ist "@()"), liegen alle Knoten in der Knotengruppe **AzureNodes** im Bereich, wenn für **NodeType** der Wert "AzureNodes" festgelegt ist. Und alle Knoten in der Knotengruppe **ComputeNodes** liegen im Bereich, wenn für **NodeType** der Wert "ComputeNodes" festgelegt ist.

 * **JobTemplates** – Namen der Auftragsvorlagen zum Definieren des Bereichs, in dem sich die Knoten vergrößern.

 * **NodeType** – Der Typ des Knotens, der sich vergrößert und verkleinert. Folgende Werte werden unterstützt:

     * **AzureNodes** – Für Azure-PaaS (Burst)-Knoten in einem lokalen oder Azure-IaaS-Cluster.

     * **ComputeNodes** – Nur für virtuelle Computer für Computeknoten in einem Azure-IaaS-Cluster.

* **NumOfQueuedJobsPerNodeToGrow** – Die Anzahl der in der Warteschlange befindlichen Aufträge, die erforderlich sind, damit ein Knoten vergrößert wird.

* **NumOfQueuedJobsToGrowThreshold** – Der Schwellenwert der in der Warteschlange befindlichen Aufträge zum Starten des Vergrößerungsvorgangs.

* **NumOfActiveQueuedTasksPerNodeToGrow** – Die Anzahl der aktiven in der Warteschlange befindlichen Aufgaben, die erforderlich sind, damit ein Knoten vergrößert wird. Wenn **NumOfQueuedJobsPerNodeToGrow** mit einem Wert größer als 0 angegeben ist, wird dieser Parameter ignoriert.

* **NumOfActiveQueuedTasksToGrowThreshold** – Der Schwellenwert der aktiven in der Warteschlange befindlichen Aufgaben zum Starten des Vergrößerungsvorgangs.

* **NumOfInitialNodesToGrow** – Die anfängliche minimale Anzahl der zu vergrößernden Knoten, wenn alle Knoten im Bereich den Status **Nicht bereitgestellt** oder **Beendet (Zuordnung aufgehoben)** aufweisen.

* **GrowCheckIntervalMins** – Das Intervall in Minuten zwischen Überprüfungen auf Vergrößerung.

* **ShrinkCheckIntervalMins** – Das Intervall in Minuten zwischen Überprüfungen auf Verkleinerung.

* **ShrinkCheckIdleTimes** – Die Anzahl der fortlaufenden Verkleinerungsüberprüfungen (getrennt durch **ShrinkCheckIntervalMins**), durch die angegeben wird, dass die Knoten inaktiv sind.

* **UseLastConfigurations** – Die vorherigen in der Argumentdatei gespeicherten Konfigurationen.

* **ArgFile** – Der Name der Argumentdatei, in der die Konfigurationen zum Ausführen des Skripts gespeichert und aktualisiert werden.

* **LogFilePrefix** – Der Präfixname der Protokolldatei. Sie können einen Pfad angeben. Standardmäßig wird das Protokoll in das aktuelle Arbeitsverzeichnis geschrieben.

### Beispiel 1

Im folgenden Beispiel werden die mit der Standardvorlage "AzureNode" bereitgestellten Azure-Burstknoten so konfiguriert, dass sie automatisch vergrößert und verkleinert werden. Wenn sich alle Knoten anfänglich im Status **Nicht bereitgestellt** befinden, werden mindestens 3 Knoten gestartet. Wenn die Anzahl der in der Warteschlange befindlichen Aufträge 8 überschreitet, startet das Skript Knoten, bis ihre Anzahl das Verhältnis der Aufträge in der Warteschlange zu **NumOfQueuedJobsPerNodeToGrow** überschreitet. Wenn sich ein Knoten in 3 aufeinanderfolgenden Leerlaufzeiten im Leerlauf befindet, wird er beendet.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### Beispiel 2

Im folgenden Beispiel werden die mit der Standardvorlage "ComputeNode" bereitgestellten virtuellen Azure Computer für den Computeknoten so konfiguriert, dass sie automatisch vergrößert und verkleinert werden. Die durch die Standardauftragsvorlage konfigurierten Aufträge definieren den Bereich der Workload auf dem Cluster. Wenn alle Knoten anfänglich beendet sind, werden mindestens 5 Knoten gestartet. Wenn die Anzahl der in der Warteschlange befindlichen aktiven Aufgaben 15 überschreitet, startet das Skript Knoten, bis ihre Anzahl das Verhältnis der aktiven Aufgaben in der Warteschlange zu **NumOfActiveQueuedTasksPerNodeToGrow** überschreitet. Wenn sich ein Knoten in 10 aufeinanderfolgenden Leerlaufzeiten im Leerlauf befindet, wird er beendet.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```

<!---HONumber=AcomDC_0420_2016-->