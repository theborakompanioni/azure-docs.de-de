---
title: Automatisches Skalieren von HPC Pack-Clusterknoten | Microsoft Docs
description: "Automatisches Vergrößern und Verkleinern der Anzahl der HPC Pack-Cluster-Compute-Knoten in Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: 
editor: tysonn
ms.assetid: 38762cd1-f917-464c-ae5d-b02b1eb21e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/08/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 0dc0d15c64d8951c3c457df73588c37418a3c8a4
ms.lasthandoff: 03/25/2017


---
# <a name="automatically-grow-and-shrink-the-hpc-pack-cluster-resources-in-azure-according-to-the-cluster-workload"></a>Automatisches Vergrößern oder Verkleinern der HPC Pack-Clusterressourcen in Azure gemäß der Clusterworkload
Wenn Sie Azure-Burstknoten im HPC Pack-Cluster bereitstellen oder einen HPC Pack-Cluster auf virtuellen Azure-Computern erstellen, möchten Sie möglicherweise die Anzahl der Clusterressourcen, wie z.B. Knoten oder Kerne, entsprechend der Workload im Cluster automatisch vergrößern oder verkleinern. Indem Sie die Clusterressourcen auf diese Weise skalieren, können Sie Ihre Azure-Ressourcen effizienter nutzen und die Kosten kontrollieren.

In diesem Artikel werden zwei Möglichkeiten erläutert, die HPC Pack bereitstellt, um Computeressourcen automatisch zu skalieren:

* Die HPC Pack-Cluster-Eigenschaft **AutoGrowShrink**

* Das HPC-PowerShell-Skript **AzureAutoGrowShrink.ps1**

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Zurzeit können Sie nur die HPC Pack-Computeknoten automatisch vergrößern oder verkleinern, auf denen ein Windows Server-Betriebssystem ausgeführt wird.


## <a name="set-the-autogrowshrink-cluster-property"></a>Festlegen der AutoGrowShrink-Clustereigenschaft
### <a name="prerequisites"></a>Voraussetzungen

* **HPC Pack 2012 R2 Update 2 oder späteres Cluster** – Der Clusterhauptknoten kann entweder lokal oder auf einem virtuellen Azure-Computer bereitgestellt werden. Informationen für die ersten Schritte mit einem lokalen Hauptknoten und Azure-Burstknoten finden Sie unter [Einrichten eines Hybrid-Rechenclusters mit Microsoft HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) . Informationen zur schnellen Bereitstellung eines HPC Pack-Clusters auf virtuellen Azure-Computern finden Sie im [HPC Pack-IaaS-Bereitstellungsskript](hpcpack-cluster-powershell-script.md).

* **Für einen Cluster mit einem Hauptknoten in Azure (Resource Manager-Bereitstellungsmodell)**: Ab HPC Pack 2016 wird die Zertifikatauthentifizierung in einer Azure Active Directory-Anwendung zum automatischen Vergrößern und Verkleinern von virtuellen Clustercomputern verwendet, die mithilfe von Azure Resource Manager bereitgestellt wurden. Konfigurieren Sie wie folgt ein Zertifikat:

  1. Nach der Clusterbereitstellung stellen Sie über Remotedesktop eine Verbindung mit einem Hauptknoten her.

  2. Laden Sie das Zertifikat (PFX-Format mit privatem Schlüssel) auf jeden Hauptknoten hoch, und installieren Sie es in „Cert:\LocalMachine\My“ und „Cert:\LocalMachine\Root“.

  3. Starten Sie Azure PowerShell als Administrator, und führen Sie die folgenden Befehle auf einem Hauptknoten aus:

    ```powershell
        cd $env:CCP_HOME\bin

        Login-AzureRmAccount
    ```
        
    Wenn sich Ihr Konto in mehreren Azure Active Directory-Mandanten oder Azure-Abonnements befindet, können Sie den folgenden Befehl ausführen, um den richtigen Mandanten und das richtige Abonnement auszuwählen:
  
    ```powershell
        Login-AzureRMAccount -TenantId <TenantId> -SubscriptionId <subscriptionId>
    ```     
       
    Führen Sie den folgenden Befehl aus, um den aktuell ausgewählten Mandanten und das aktuell ausgewählte Abonnement anzuzeigen:
    
    ```powershell
        Get-AzureRMContext
    ```

  4. Führen Sie das folgende Skript aus:

    ```powershell
        .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -CertificateThumbprint "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" -TenantId xxxxxxxx-xxxxx-xxxxx-xxxxx-xxxxxxxxxxxx
    ```

    Hierbei gilt:

    **DisplayName**: Der Anzeigename der Azure Active Directory-Anwendung. Wenn die Anwendung nicht vorhanden ist, wird sie in Azure Active Directory erstellt.

    **HomePage**: Die Startseite der Anwendung. Sie können, wie im obigen Beispiel, eine Pseudo-URL konfigurieren.

    **IdentifierUri**: Bezeichner der Anwendung. Sie können, wie im obigen Beispiel, eine Pseudo-URL konfigurieren.

    **CertificateThumbprint**: Fingerabdruck des Zertifikats, das Sie in Schritt 1 auf dem Hauptknoten installiert haben.

    **TenantId**: Mandanten-ID Ihrer Azure Active Directory-Instanz. Sie können die Mandanten-ID über die Seite **Eigenschaften** im Azure Active Directory-Portal abrufen.

    Um weitere Details zu **ConfigARMAutoGrowShrinkCert.ps1** zu erhalten, führen Sie `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed` aus.


* **Für einen Cluster mit einem Hauptknoten in Azure (klassisches Bereitstellungsmodell)**: Wenn Sie das HPC Pack-IaaS-Bereitstellungsskript verwenden, um den Cluster im klassischen Bereitstellungsmodell zu erstellen, aktivieren Sie die **AutoGrowShrink**-Clustereigenschaft durch Festlegen der AutoGrowShrink-Option in der Clusterkonfigurationsdatei. Details finden Sie in der Dokumentation, die beim [Skriptdownload](https://www.microsoft.com/download/details.aspx?id=44949)bereitgestellt wird.

    Aktivieren Sie alternativ die **AutoGrowShrink** -Clustereigenschaft nach der Bereitstellung des Clusters mit den HPC PowerShell-Befehlen, die im folgenden Abschnitt beschrieben werden. Führen Sie zur Vorbereitung zuerst die folgenden Schritte aus:

  1. Konfigurieren Sie ein Azure-Verwaltungszertifikat auf dem Hauptknoten und im Azure-Abonnement. In einer Testbereitstellung können Sie das standardmäßige selbstsignierte Microsoft Azure-HPC-Zertifikat verwenden, das HPC auf dem Hauptknoten installiert, und dieses Zertifikat in Ihr Azure-Abonnement hochladen. Optionen und Schritte finden Sie in den [Anleitungen der TechNet-Bibliothek](https://technet.microsoft.com/library/gg481759.aspx).

  2. Führen Sie **regedit** auf dem Hauptknoten aus, wechseln Sie zu „HKLM\SOFTWARE\Microsoft\HPC\IaasInfo“, und fügen Sie einen Zeichenfolgenwert hinzu. Legen Sie den Namen des Werts auf „Fingerabdruck“ und die Daten des Werts auf den Fingerabdruck des Zertifikats von Schritt 1 fest.

### <a name="hpc-powershell-commands-to-set-the-autogrowshrink-property"></a>HPC PowerShell-Befehle zum Festlegen der AutoGrowShrink-Eigenschaft
Im Folgenden finden Sie HPC PowerShell-Beispielbefehle zum Festlegen von **AutoGrowShrink** und zum Optimieren des Verhaltens mit zusätzlichen Parametern. Unter [AutoGrowShrink-Parameter](#AutoGrowShrink-parameters) weiter unten in diesem Artikel finden Sie eine vollständige Liste der Einstellungen.

Starten Sie zum Ausführen dieser Befehle HPC PowerShell auf dem Clusterhauptknoten als Administrator.

**So aktivieren Sie die AutoGrowShrink-Eigenschaft**

```powershell
Set-HpcClusterProperty –EnableGrowShrink 1
```

**So deaktivieren Sie die AutoGrowShrink-Eigenschaft**

```powershell
Set-HpcClusterProperty –EnableGrowShrink 0
```

**So ändern Sie das Vergrößerungsintervall in Minuten**

```powershell
Set-HpcClusterProperty –GrowInterval <interval>
```

**So ändern Sie das Verkleinerungsintervall in Minuten**

```powershell
Set-HpcClusterProperty –ShrinkInterval <interval>
```

**So zeigen Sie die aktuelle Konfiguration von AutoGrowShrink an**

```powershell
Get-HpcClusterProperty –AutoGrowShrink
```

**So schließen Sie Knotengruppen von AutoGrowShrink aus**

```powershell
Set-HpcClusterProperty –ExcludeNodeGroups <group1,group2,group3>
```

>[!NOTE] 
> Dieser Parameter wird ab HPC Pack 2016 unterstützt.
>

### <a name="autogrowshrink-parameters"></a>AutoGrowShrink-Parameter
Im Folgenden werden AutoGrowShrink-Parameter aufgeführt, die Sie mit dem **Set-HpcClusterProperty** -Befehl ändern können.

* **EnableGrowShrink**: Option zum Aktivieren oder Deaktivieren der **AutoGrowShrink**-Eigenschaft.
* **ParamSweepTasksPerCore** – Anzahl der Parameter-Sweep-Aufgaben zum Vergrößern eines Kerns. Standardmäßig wird ein Kern pro Aufgabe vergrößert.

  > [!NOTE]
  > HPC Pack QFE KB3134307 ändert **ParamSweepTasksPerCore** in **TasksPerResourceUnit**. Dies basiert auf dem Auftragsressourcentyp, wobei es sich um einen Knoten, einen Socket oder einen Kern handeln kann.
  >
  >
* **GrowThreshold**: Schwellenwert für Aufgaben in der Warteschlange zum Auslösen der automatischen Vergrößerung. Der Standardwert ist 1, d. h., wenn es mehr als eine Aufgabe in der Warteschlange gibt, werden die Knoten automatisch vergrößert.
* **GrowInterval** – Intervall in Minuten zum Auslösen der automatischen Vergrößerung. Das Standardintervall beträgt 5 Minuten.
* **ShrinkInterval** – Intervall in Minuten zum Auslösen der automatischen Verkleinerung. Das Standardintervall beträgt 5 Minuten.
* **ShrinkIdleTimes** – Anzahl der fortlaufenden Verkleinerungsüberprüfungen, um anzugeben, dass die Knoten inaktiv sind. Der Standardwert ist 3 Mal. Wenn **ShrinkInterval** 5 Minuten beträgt, überprüft HPC Pack beispielsweise alle 5 Minuten, ob der Knoten inaktiv ist. Wenn die Knoten nach 3 fortlaufenden Prüfungen (15 Minuten) inaktiv sind, verkleinert HPC Pack diesen Knoten.
* **ExtraNodesGrowRatio**: Zusätzlicher Prozentsatz an zu vergrößernden Knoten für Aufträge von Message Passing Interface (MPI). Der Standardwert ist 1, d. h. dass die MPI-Aufträge durch das HPC Pack um 1 % zusätzliche Knoten erweitert werden.
* **GrowByMin** – Schalter, um anzugeben, ob die Richtlinie für die automatische Vergrößerung auf den für den Auftrag erforderlichen Mindestressourcen basiert. Der Standardwert ist „false“, was bedeutet, dass das HPC Pack Knoten für Aufträge basierend auf den für die Aufträge erforderlichen Maximalressourcen vergrößert.
* **SoaJobGrowThreshold** – Schwellenwert von eingehenden SOA-Anforderungen zum Auslösen des automatischen Vergrößerungsprozesses. Der Standardwert ist 50000.

  > [!NOTE]
  > Dieser Parameter wird ab HPC Pack 2012 R2 Update 3 unterstützt.
  >
  >
* **SoaRequestsPerCore** – Anzahl der eingehenden SOA-Anforderungen zum Vergrößern eines Kerns. Der Standardwert ist 20000.

  > [!NOTE]
  > Dieser Parameter wird ab HPC Pack 2012 R2 Update 3 unterstützt.
  >
* **ExcludeNodeGroups**: Knoten in den angegebenen Knotengruppen werden nicht automatisch vergrößert und verkleinert.
  
  > [!NOTE]
  > Dieser Parameter wird ab HPC Pack 2016 unterstützt.
  >

### <a name="mpi-example"></a>MPI-Beispiel
Standardmäßig erstellt das HPC Pack 1 % zusätzliche Knoten für MPI-Aufträge (**ExtraNodesGrowRatio** ist auf 1 festgelegt). Der Grund dafür ist, dass MPI möglicherweise mehrere Knoten benötigt und dass der Auftrag nur ausgeführt werden kann, wenn alle Knoten bereit sind. Wenn Azure Knoten startet, benötigt ein Knoten gelegentlich mehr Zeit als andere zum Starten, wodurch andere Knoten inaktiv sind, während sie darauf warten, dass dieser Knoten bereit ist. Durch die Erweiterung mit zusätzlichen Knoten, reduziert HPC Pack die Wartezeit für diese Ressource und spart potenziell Kosten. Um den Prozentsatz an zusätzlichen Knoten für MPI-Aufträge (z.B. auf 10%) zu erhöhen, führen Sie einen Befehl ähnlich dem folgenden aus:

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>SOA-Beispiel
Standardmäßig ist **SoaJobGrowThreshold** auf 50.000 und **SoaRequestsPerCore** auf 200.000 festgelegt. Wenn Sie einen SOA-Auftrag mit 70.000 Anforderungen senden, gibt es eine Aufgabe in der Warteschlange, und die Anzahl der eingehenden Anforderungen beträgt 70.000. In diesem Fall erstellt das HPC Pack 1 Kern für die Aufgabe in der Warteschlange und (70.000 - 50.000)/20.000 = 1 Kern für eingehende Anforderungen. Insgesamt beträgt die Vergrößerung für diesen SOA-Auftrag also 2 Kerne.

## <a name="run-the-azureautogrowshrinkps1-script"></a>Ausführen des AzureAutoGrowShrink.ps1-Skripts
### <a name="prerequisites"></a>Voraussetzungen

* **Cluster mit HPC Pack 2012 R2 Update 1 oder höher**: Das Skript **AzureAutoGrowShrink.ps1** ist im Ordner „%CCP_HOME%bin“ installiert. Der Clusterhauptknoten kann entweder lokal oder auf einem virtuellen Azure-Computer bereitgestellt werden. Informationen für die ersten Schritte mit einem lokalen Hauptknoten und Azure-Burstknoten finden Sie unter [Einrichten eines Hybrid-Rechenclusters mit Microsoft HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) . Informationen zur schnellen Bereitstellung eines HPC Pack-Clusters auf virtuellen Azure-Computern finden Sie im [HPC Pack IaaS-Bereitstellungsskript](hpcpack-cluster-powershell-script.md). Alternativ können Sie eine [Azure-Schnellstartvorlage](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) nutzen.
* **Azure PowerShell 1.4.0**: Das Skript ist derzeit von dieser spezifischen Version von Azure PowerShell abhängig.
* **Für einen Cluster mit Azure-Burstknoten** – Führen Sie das Skript auf einem Clientcomputer, auf dem HPC Pack installiert ist, oder auf dem Hauptknoten aus. Wenn Sie das Skript auf einem Clientcomputer ausführen, stellen Sie sicher, dass Sie die Variable „$env:CCP_SCHEDULER“ so festlegen, dass sie auf den Hauptknoten verweist. Die Azure-Burstknoten müssen dem Cluster bereits hinzugefügt sein, können jedoch den Status „Nicht bereitgestellt“ aufweisen.
* **Für einen auf virtuellen Azure-Computern bereitgestellten Cluster (Resource Manager-Bereitstellungsmodell)**: Bei einem Cluster aus virtuellen Azure-Computern, der im Resource Manager-Bereitstellungsmodell bereitgestellt wurde, unterstützt das Skript zwei Methoden der Azure-Authentifizierung: Anmelden bei Ihrem Azure-Konto, um das Skript jedes Mal auszuführen (mit `Login-AzureRmAccount`), oder Konfigurieren eines Dienstprinzipals zur Authentifizierung mit einem Zertifikat. HPC Pack stellt das Skript **ConfigARMAutoGrowShrinkCert.ps** bereit, um einen Dienstprinzipal mit Zertifikat zu erstellen. Das Skript erstellt eine Azure Active Directory-Anwendung (Azure AD) und einen Dienstprinzipal und weist dem Dienstprinzipal die Rolle „Mitwirkender“ zu. Um das Skript auszuführen, starten Sie Azure PowerShell als Administrator, und führen Sie die folgenden Befehle aus:

    ```powershell
    cd $env:CCP_HOME\bin

    Login-AzureRmAccount

    .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -PfxFile "d:\yourcertificate.pfx"
    ```

    Um weitere Details zu **ConfigARMAutoGrowShrinkCert.ps1** zu erhalten, führen Sie `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed` aus.

* **Für einen auf virtuellen Azure-Computern bereitgestellten Cluster (klassisches Bereitstellungsmodell)**: Führen Sie das Skript auf dem virtuellen Computer des Hauptknotens aus, da es von den Skripts **Start-HpcIaaSNode.ps1** und **Stop-HpcIaaSNode.ps1** abhängig ist, die dort installiert sind. Für diese Skripts ist zudem ein Azure-Verwaltungszertifikat oder eine Azure-Veröffentlichungseinstellungsdatei erforderlich (siehe [Manage the number and availability of compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster-node-manage.md)(in englischer Sprache)). Stellen Sie sicher, dass alle erforderlichen virtuellen Maschinen für Computeknoten dem Cluster bereits hinzugefügt sind. Sie können sich jedoch im Status „Beendet“ befinden.



### <a name="syntax"></a>Syntax
```powershell
AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfActiveQueuedTasksPerNodeToGrow <Single> [-NumOfActiveQueuedTasksToGrowThreshold <Int32>]
    [-NumOfInitialNodesToGrow <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>]
    [-ShrinkCheckIdleTimes <Int32>] [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>]
    [<CommonParameters>]

AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfQueuedJobsPerNodeToGrow <Single> [-NumOfQueuedJobsToGrowThreshold <Int32>] [-NumOfInitialNodesToGrow
    <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>] [-ShrinkCheckIdleTimes <Int32>]
    [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]

AzureAutoGrowShrink.ps1 -UseLastConfigurations [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]
```
### <a name="parameters"></a>Parameter
* **NodeTemplates**: Namen der Knotenvorlagen zum Definieren des Bereichs, in dem die Knoten vergrößert und verkleinert werden. Wenn kein Bereich angegeben ist (der Standardwert ist @()), gilt Folgendes: Alle Knoten in der Knotengruppe **AzureNodes** liegen im Bereich, wenn für **NodeType** der Wert „AzureNodes“ festgelegt ist. Alle Knoten in der Knotengruppe **ComputeNodes** liegen im Bereich, wenn für **NodeType** der Wert „ComputeNodes“ festgelegt ist.
* **JobTemplates** – Namen der Auftragsvorlagen zum Definieren des Bereichs, in dem sich die Knoten vergrößern.
* **NodeType** – Der Typ des Knotens, der sich vergrößert und verkleinert. Folgende Werte werden unterstützt:

  * **AzureNodes** – Für Azure-PaaS (Burst)-Knoten in einem lokalen oder Azure-IaaS-Cluster.
  * **ComputeNodes** – Nur für virtuelle Computer für Computeknoten in einem Azure-IaaS-Cluster.

* **NumOfQueuedJobsPerNodeToGrow** – Die Anzahl der in der Warteschlange befindlichen Aufträge, die erforderlich sind, damit ein Knoten vergrößert wird.
* **NumOfQueuedJobsToGrowThreshold** – Der Schwellenwert der in der Warteschlange befindlichen Aufträge zum Starten des Vergrößerungsvorgangs.
* **NumOfActiveQueuedTasksPerNodeToGrow** – Die Anzahl der aktiven in der Warteschlange befindlichen Aufgaben, die erforderlich sind, damit ein Knoten vergrößert wird. Wenn **NumOfQueuedJobsPerNodeToGrow** mit einem Wert größer als 0 angegeben ist, wird dieser Parameter ignoriert.
* **NumOfActiveQueuedTasksToGrowThreshold** – Der Schwellenwert der aktiven in der Warteschlange befindlichen Aufgaben zum Starten des Vergrößerungsvorgangs.
* **NumOfInitialNodesToGrow**: Die anfängliche minimale Anzahl der zu vergrößernden Knoten, wenn alle Knoten im Bereich den Status **Nicht bereitgestellt** oder **Beendet (Zuordnung aufgehoben)** aufweisen.
* **GrowCheckIntervalMins** – Das Intervall in Minuten zwischen Überprüfungen auf Vergrößerung.
* **ShrinkCheckIntervalMins** – Das Intervall in Minuten zwischen Überprüfungen auf Verkleinerung.
* **ShrinkCheckIdleTimes**: Die Anzahl der fortlaufenden Verkleinerungsüberprüfungen (getrennt durch **ShrinkCheckIntervalMins**), durch die angegeben wird, dass die Knoten inaktiv sind.
* **UseLastConfigurations** – Die vorherigen in der Argumentdatei gespeicherten Konfigurationen.
* **ArgFile**– Der Name der Argumentdatei, in der die Konfigurationen zum Ausführen des Skripts gespeichert und aktualisiert werden.
* **LogFilePrefix** – Der Präfixname der Protokolldatei. Sie können einen Pfad angeben. Standardmäßig wird das Protokoll in das aktuelle Arbeitsverzeichnis geschrieben.

### <a name="example-1"></a>Beispiel 1
Im folgenden Beispiel werden die mit der Standardvorlage "AzureNode" bereitgestellten Azure-Burstknoten so konfiguriert, dass sie automatisch vergrößert und verkleinert werden. Wenn sich alle Knoten anfänglich im Status **Nicht bereitgestellt** befinden, werden mindestens 3 Knoten gestartet. Wenn die Anzahl der in der Warteschlange befindlichen Aufträge 8 überschreitet, startet das Skript Knoten, bis ihre Anzahl das Verhältnis der Aufträge in der Warteschlange zu **NumOfQueuedJobsPerNodeToGrow**überschreitet. Wenn sich ein Knoten in 3 aufeinanderfolgenden Leerlaufzeiten im Leerlauf befindet, wird er beendet.

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>Beispiel 2
Im folgenden Beispiel werden die mit der Standardvorlage "ComputeNode" bereitgestellten virtuellen Azure Computer für den Computeknoten so konfiguriert, dass sie automatisch vergrößert und verkleinert werden.
Die durch die Standardauftragsvorlage konfigurierten Aufträge definieren den Bereich der Workload auf dem Cluster. Wenn alle Knoten anfänglich beendet sind, werden mindestens 5 Knoten gestartet. Wenn die Anzahl der in der Warteschlange befindlichen aktiven Aufgaben 15 überschreitet, startet das Skript Knoten, bis ihre Anzahl das Verhältnis der aktiven Aufgaben in der Warteschlange zu **NumOfActiveQueuedTasksPerNodeToGrow**überschreitet. Wenn sich ein Knoten in 10 aufeinanderfolgenden Leerlaufzeiten im Leerlauf befindet, wird er beendet.

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```

