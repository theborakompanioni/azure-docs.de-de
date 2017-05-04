---
title: "Ausführen von STAR-CCM+ mit HPC Pack auf virtuellen Linux-Computern | Microsoft Docs"
description: "Stellen Sie einen Microsoft HPC Pack-Cluster unter Azure bereit, und führen Sie einen StarCCM+-Auftrag auf mehreren Linux-Computeknoten in einem RDMA-Netzwerk aus."
services: virtual-machines-linux
documentationcenter: 
author: xpillons
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 75523406-d268-4623-ac3e-811c7b74de4b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 09/13/2016
ms.author: xpillons
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 9a2796c66fa49c543e52b654e583ade7625a7f31
ms.lasthandoff: 04/27/2017


---
# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Ausführen von STAR-CCM+ mit Microsoft HPC Pack auf einem Linux-RDMA-Cluster in Azure
In diesem Artikel erfahren Sie, wie Sie einen Microsoft HPC Pack-Cluster unter Azure bereitstellen und einen Auftrag vom Typ [CD-adapco STAR-CCM+](http://www.cd-adapco.com/products/star-ccm%C2%AE) auf mehreren Linux-Computeknoten ausführen, die per InfiniBand miteinander verbunden sind.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Microsoft HPC Pack bietet eine Vielzahl von umfangreichen HPC- und parallelen Anwendungen, einschließlich MPI-Anwendungen, auf Clustern mit virtuellen Microsoft Azure-Computern. HPC Pack unterstützt auch die Ausführung von Linux-HPC-Anwendungen auf virtuellen Computern für Linux-Computeknoten, die in einem HPC Pack-Cluster bereitgestellt werden. Eine Einführung in die Verwendung von Linux-Serverknoten mit HPC Pack finden Sie unter [Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](hpcpack-cluster.md).

## <a name="set-up-an-hpc-pack-cluster"></a>Einrichten eines HPC Pack-Clusters
Laden Sie die HPC Pack-IaaS-Bereitstellungsskripts vom [Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=44949) herunter, und extrahieren Sie sie lokal.

Azure PowerShell ist hierfür erforderlich. Sollte PowerShell auf Ihrem lokalen Computer nicht konfiguriert sein, lesen Sie den Artikel [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).

Zum Zeitpunkt der Erstellung dieses Artikels stehen Linux-Images aus dem Azure Marketplace (mit InfiniBand-Treibern für Azure) für SLES 12, CentOS 6.5 und CentOS 7.1 zur Verfügung. Dieser Artikel basiert auf der Verwendung von SLES 12. Die Namen aller im Marketplace verfügbaren Linux-Images mit HPC-Unterstützung können mithilfe des folgenden PowerShell-Befehls abgerufen werden:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

Daraufhin wird eine Liste mit Orten zurückgegeben, an denen diese Images zur Verfügung stehen, sowie der Imagename (**ImageName**), der später in der Bereitstellungsvorlage verwendet wird.

Vor der Bereitstellung des Clusters müssen Sie eine Vorlagendatei für die HPC Pack-Bereitstellung erstellen. Da es sich bei unserem Ziel um einen kleinen Cluster handelt, wird als Hauptknoten der Domänencontroller verwendet und eine lokale SQL-Datenbank gehostet.

Die folgende Vorlage stellt einen solchen Hauptknoten bereit, erstellt eine XML-Datei namens **MyCluster.xml** und ersetzt die Werte **SubscriptionId**, **StorageAccount**, **Location**, **VMName** und **ServiceName** durch Ihre eigenen Werte.

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

Führen Sie zum Erstellen des Hauptknotens zunächst den folgenden PowerShell-Befehl in einer Eingabeaufforderung mit erhöhten Rechten aus:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

Nach 20 bis 30 Minuten sollte der Hauptknoten fertiggestellt sein. Sie können eine Verbindung mit dem Knoten herstellen, indem Sie im Azure-Portal auf das **Verbindungssymbol** des virtuellen Computers klicken.

Unter Umständen müssen Sie die DNS-Weiterleitung korrigieren. Starten Sie hierzu den DNS-Manager.

1. Klicken Sie im DNS-Manager mit der rechten Maustaste auf den Servernamen, und klicken Sie auf **Eigenschaften** und anschließend auf die Registerkarte **Weiterleitungen**.
2. Klicken Sie auf die Schaltfläche **Bearbeiten**, entfernen Sie alle Weiterleitungen, und klicken Sie danach auf **OK**.
3. Aktivieren Sie das Kontrollkästchen **Stammhinweise verwenden, wenn keine Weiterleitungen verfügbar sind**, und klicken Sie auf **OK**.

## <a name="set-up-linux-compute-nodes"></a>Einrichten von Linux-Computeknoten
Linux-Computeknoten werden mit der gleichen Bereitstellungsvorlage bereitgestellt, die auch zum Erstellen des Hauptknotens verwendet wurde.

Kopieren Sie die Datei **MyCluster.xml** von Ihrem lokalen Computer auf den Hauptknoten, und aktualisieren Sie das Tag **NodeCount** mit der Anzahl von Knoten, die Sie bereitstellen möchten (<= 20). Stellen Sie sicher, dass in Ihrem Azure-Kontingent genügend Kerne verfügbar sind, da jede A9-Instanz 16 Kerne in Ihrem Abonnement belegt. Falls Sie innerhalb des gleichen Budgets mehr virtuelle Computer verwenden möchten, können Sie anstelle von A9-Instanzen A8-Instanzen (acht Kerne) verwenden.

Kopieren Sie auf dem Hauptknoten die HPC Pack-IaaS-Bereitstellungsskripts.

Führen Sie die folgenden Azure PowerShell-Befehle in einer Eingabeaufforderung mit erhöhten Rechten aus:

1. Führen Sie **Add-AzureAccount** aus, um eine Verbindung mit Ihrem Azure-Abonnement herzustellen.
2. Sollten Sie über mehrere Abonnements verfügen, führen Sie **Get-AzureSubscription** aus, um diese aufzulisten.
3. Führen Sie den Befehl **Select-AzureSubscription -SubscriptionName xxxx -Default** aus, um ein Standardabonnement festzulegen.
4. Führen Sie **.\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml** aus, um die Bereitstellung von Linux-Serverknoten zu starten.
   
   ![Hauptknotenbereitstellung][hndeploy]

Öffnen Sie das HPC Pack-Cluster-Manager-Tool. Nach einigen Minuten werden Linux-Computeknoten regulär in der Liste mit Cluster-Computeknoten angezeigt. Beim klassischen Bereitstellungsmodell werden virtuelle IaaS-Computer nacheinander erstellt. Wenn die Anzahl der Knoten wichtig ist, kann die Bereitstellung einige Zeit in Anspruch nehmen.

![Linux-Knoten im HPC Pack-Cluster-Manager][clustermanager]

Nachdem nun alle Knoten im Cluster betriebsbereit sind, müssen noch einige weitere Infrastruktureinstellungen vorgenommen werden.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>Einrichten der Azure-Dateifreigabe für Windows- und Linux-Knoten
Sie können den Azure-Dateidienst zum Speichern von Skripts, Anwendungspaketen und Datendateien verwenden. Der Azure-Dateidienst bietet zusätzlich zum Azure-BLOB-Speicher auch CIFS-Funktionen als permanenten Speicher. Diese Lösung ist zwar nicht besonders skalierbar, dafür aber sehr einfach und erfordert keine dedizierten virtuellen Computer.

Erstellen Sie eine Azure-Dateifreigabe. Eine entsprechende Anleitung finden Sie im Artikel [Erste Schritte mit Azure File Storage unter Windows](../../../storage/storage-dotnet-how-to-use-files.md).

Lassen Sie den Namen Ihres Speicherkontos (**saname**), den Namen der Dateifreigabe (**sharename**) und den Speicherkontoschlüssel (**sakey**) unverändert.

### <a name="mount-the-azure-file-share-on-the-head-node"></a>Einbinden der Azure-Dateifreigabe auf dem Hauptknoten
Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten, und führen Sie den folgenden Befehl aus, um die Anmeldeinformationen im Tresor des lokalen Computers zu speichern:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Führen Sie dann den folgenden Befehl aus, um die Azure-Dateifreigabe einzubinden:

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-the-azure-file-share-on-linux-compute-nodes"></a>Einbinden der Azure-Dateifreigabe auf Linux-Computeknoten
HPC Pack enthält unter anderem das praktische Tool „clusrun“. Mithilfe dieses Befehlszeilentools können Sie einen Befehl gleichzeitig für eine Gruppe von Computeknoten ausführen. Wir verwenden das Tool in diesem Fall, um die Azure-Dateifreigabe einzubinden und so zu konfigurieren, dass sie auch nach einem Neustart erhalten bleibt.
Führen Sie auf dem Hauptknoten in einer Eingabeaufforderung mit erhöhten Rechten folgende Befehle aus:

Erstellen des Bereitstellungsverzeichnisses:

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

Einbinden der Azure-Dateifreigabe:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

Beibehalten der eingebundenen Freigabe:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Installieren von STAR-CCM+
Die Azure-VM-Instanzen A8 und A9 bieten InfiniBand-Unterstützung und RDMA-Funktionen. Die Kernel-Treiber, die diese Funktionen ermöglichen, stehen im Azure Marketplace für Windows Server 2012 R2-, SUSE 12-, CentOS 6.5- und CentOS 7.1-Images zur Verfügung. Microsoft MPI und Intel MPI (Version 5.x) sind die beiden MPI-Bibliotheken, die diese Treiber in Azure unterstützen.

CD-adapco STAR-CCM+ der Version 11.x und höher ist mit Intel MPI der Version 5.x gebündelt, sodass InfiniBand-Unterstützung für Azure enthalten ist.

Laden Sie das Linux64 STAR-CCM+-Paket vom [CD-adapco-Portal](https://steve.cd-adapco.com)herunter. Wir haben in diesem Fall Version 11.02.010 mit gemischter Genauigkeit verwendet.

Erstellen Sie auf dem Hauptknoten in der Azure-Dateifreigabe **/hpcdata** ein Shellskript namens **setupstarccm.sh** mit folgendem Inhalt. Dieses Skript wird auf jedem Computeknoten zur lokalen Einrichtung von STAR-CCM+ verwendet.

#### <a name="sample-setupstarcmsh-script"></a>Beispielskript für „setupstarcm.sh“
```
    #!/bin/bash
    # setupstarcm.sh to set up STAR-CCM+ locally

    # Create the CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy the STAR-CCM package from the file share to the local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract the package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without the FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Öffnen Sie nun eine Eingabeaufforderung mit erhöhten Rechten, und führen Sie den folgenden Befehl aus, um STAR-CCM+ auf allen Ihren Linux-Computeknoten einzurichten:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Während der Befehlsausführung können Sie mithilfe der Heatmap des Cluster-Managers die CPU-Auslastung überwachen. Nach einigen Minuten sollten alle Knoten ordnungsgemäß eingerichtet sein.

## <a name="run-star-ccm-jobs"></a>Ausführen von STAR-CCM+-Aufträgen
HPC Pack wird wegen seiner Auftragsplanerfunktionen zum Ausführen von STAR-CCM+-Aufträgen verwendet. Zu diesem Zweck benötigen wir einige Skripts, mit denen wir den Auftrag starten und STAR-CCM+ ausführen. Die Eingabedaten werden der Einfachheit halber zunächst auf der Azure-Dateifreigabe gespeichert.

Das folgende PowerShell-Skript wird verwendet, um der Warteschlange einen STAR-CCM+-Auftrag hinzuzufügen. Es verwendet drei Argumente:

* Den Modellnamen
* Die Anzahl der zu verwendenden Knoten
* Die Anzahl der pro Knoten zu verwendenden Kerne

Da STAR-CCM+ die gesamte Speicherbandbreite beanspruchen kann, empfiehlt es sich in der Regel, weniger Kerne pro Computeknoten zu verwenden und neue Knoten hinzuzufügen. Die genaue Anzahl von Kernen pro Knoten hängt von der Prozessorfamilie sowie von der Verbindungsgeschwindigkeit ab.

Die Knoten werden exklusiv für den Auftrag zugeordnet und können nicht für andere Aufträge verwendet werden. Der Auftrag wird nicht direkt als MPI-Auftrag gestartet. Das MPI-Startprogramm wird über das Shellskript **runstarccm.sh** gestartet.

Das Eingabemodell und das Skript **runstarccm.sh** werden in der zuvor eingebundenen Freigabe **/hpcdata** gespeichert.

Die Protokolldateien werden mit der Auftrags-ID benannt und zusammen mit den STAR-CCM+-Ausgabedateien in der Freigabe **/hpcdata**gespeichert.

#### <a name="sample-submitstarccmjobps1-script"></a>Beispielskript für „SubmitStarccmJob.ps1“
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us the job ID that's used to identify the name of the uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit the job     
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
Ersetzen Sie **runner.java** durch Ihr bevorzugtes STAR-CCM+-Javamodell-Startprogramm und Ihren bevorzugten Protokollierungscode.

#### <a name="sample-runstarccmsh-script"></a>Beispielskript für „runstarccm.sh“
```
    #!/bin/bash
    echo "start"
    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set the mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into the hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with the hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

In unserem Test haben wir ein Power-On-Demand-Lizenztoken verwendet. Für dieses Token müssen Sie die Umgebungsvariable **$CDLMD_LICENSE_FILE** auf **1999@flex.cd-adapco.com** und außerdem den Schlüssel in der Befehlszeilenoption **-podkey** festlegen.

Nach der Initialisierung extrahiert das Skript aus den von HPC Pack festgelegten Umgebungsvariablen **$CCP_NODES_CORES** eine Liste mit Knoten zum Erstellen einer Hostdatei für das MPI-Startprogramm. Diese Hostdatei enthält eine Liste mit den Computeknotennamen für den Auftrag (jeweils ein Name pro Zeile).

Für das Format von **$CCP_NODES_CORES** wird das folgende Muster verwendet:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Hierbei gilt:

* `<Number of nodes>` entspricht der Anzahl von Knoten, die diesem Auftrag zugeordnet sind.
* `<Name of node_n_...>` ist der Name des jeweiligen Knotens, der dem Auftrag zugeordnet ist.
* `<Cores of node_n_...>` entspricht der Anzahl von Kernen auf dem Knoten, die dem Auftrag zugeordnet sind.

Die Berechnung der Kernanzahl (**$NBCORES**) basiert auch auf der Anzahl von Knoten (**$NBNODES**) und der Anzahl von Kernen pro Knoten (angegeben als Parameter **$NBCORESPERNODE**).

Für Intel MPI unter Azure werden folgende MPI-Optionen verwendet:

* `-mpi intel` zum Festlegen von Intel MPI
* `-fabric UDAPL` zum Verwenden von Azure InfiniBand-Verben
* `-cpubind bandwidth,v` zum Optimieren der Bandbreite für MPI mit STAR-CCM+
* `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"` zum Sicherstellen, dass Intel MPI mit Azure InfiniBand funktioniert, und zum Festlegen der erforderlichen Anzahl von Kernen pro Knoten
* `-batch` zum Starten von STAR-CCM+ im Batchmodus ohne Benutzeroberfläche

Vergewissern Sie sich vor dem Starten eines Auftrags im Cluster-Manager, dass die Knoten betriebsbereit und online sind. Führen Sie anschließend in einem PowerShell-Befehlsfenster Folgendes aus:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Anhalten von Knoten
Nach Abschluss der Tests können Sie Knoten mithilfe der folgenden HPC Pack-PowerShell-Befehle beenden und starten:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Nächste Schritte
Versuchen Sie, andere Linux-Workloads auszuführen. Siehe hierzu z.B.:

* [Ausführen von NAMD mit dem Microsoft HPC Pack auf Linux-Computeknoten in Azure](hpcpack-cluster-namd.md)
* [Ausführen von OpenFOAM mit Microsoft HPC Pack auf einem Linux-RDMA-Cluster in Azure](hpcpack-cluster-openfoam.md)

<!--Image references-->
[hndeploy]:media/hpcpack-cluster-starccm/hndeploy.png
[clustermanager]:media/hpcpack-cluster-starccm/ClusterManager.png

