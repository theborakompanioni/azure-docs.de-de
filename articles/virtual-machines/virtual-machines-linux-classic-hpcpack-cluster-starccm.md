<properties
 pageTitle="Ausführen von StarCCM+ mit HPC Pack auf virtuellen Linux-Computern | Microsoft Azure"
 description="Hier erfahren Sie, wie Sie einen Microsoft HPC Pack-Cluster unter Azure bereitstellen und einen StarCCM+-Auftrag auf mehreren Linux-Computeknoten in einem RDMA-Netzwerk ausführen."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="xpillons"
 manager="kateh"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="04/13/2016"
 ms.author="xpillons"/>

# Ausführen von StarCCM+ mit Microsoft HPC Pack auf einem Linux-RDMA-Cluster in Azure
In diesem Artikel erfahren Sie, wie Sie einen Microsoft HPC Pack-Cluster unter Azure bereitstellen und einen Auftrag vom Typ [CD-Adapco StarCCM+](http://www.cd-adapco.com/products/star-ccm%C2%AE) auf mehreren Linux-Computeknoten ausführen, die per Infiniband miteinander verbunden sind.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Microsoft HPC Pack bietet eine Vielzahl von umfangreichen HPC- und parallelen Anwendungen, einschließlich MPI-Anwendungen, auf Clustern mit virtuellen Microsoft Azure-Computern. HPC Pack unterstützt auch die Ausführung von Linux-HPC-Anwendungen auf virtuellen Computern für Linux-Computeknoten, die in einem HPC Pack-Cluster bereitgestellt werden. Eine Einführung in die Verwendung von Linux-Computeknoten mit HPC Pack finden Sie unter [Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## Einrichten des HPC Pack-Clusters
Laden Sie [hier](https://www.microsoft.com/de-DE/download/details.aspx?id=44949) die HPC Pack-IaaS-Bereitstellungsskripts herunter, und extrahieren Sie sie lokal.

Sollte Azure PowerShell auf Ihrem lokalen Computer nicht konfiguriert sein, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

Zum Zeitpunkt der Erstellung dieses Artikels stehen Linux-Images aus dem Azure-Katalog mit Infiniband-Treiber für Azure für SLES 12, CentOS 6.5 und CentOS 7.1 zur Verfügung. Dieser Artikel basiert auf der Verwendung von SLES 12. Die Namen aller im Katalog verfügbaren Linux-Images mit HPC-Unterstützung können mithilfe des folgenden PowerShell-Befehls abgerufen werden:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

Daraufhin wird eine Liste mit Orten zurückgegeben, an denen diese Images zur Verfügung stehen, sowie der Image-Name, der später in der Bereitstellungsvorlage verwendet wird. Vor der Bereitstellung des Clusters müssen Sie eine Vorlagendatei für die HPC Pack-Bereitstellung erstellen. Da es sich bei unserem Ziel um einen kleinen Cluster handelt, wird als Hauptknoten der Domänencontroller verwendet und eine lokale SQL-Datenbank gehostet. Die folgende Vorlage stellt einen solchen Hauptknoten bereit, erstellt eine XML-Datei namens **MyCluster.xml** und ersetzt die Werte **SubscriptionId**, **StorageAccount**, **Location**, **VMName** und **ServiceName** durch Ihre eigenen Werte.

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

Führen Sie zum Erstellen des Hauptknotens zunächst den folgenden PowerShell-Befehl an einer Eingabeaufforderung mit erhöhten Rechten aus:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

Der Hauptknoten steht nach etwa 20 bis 30 Minuten bereit. Sie können eine Verbindung mit dem Knoten herstellen, indem Sie im Azure-Portal auf das Verbindungssymbol des virtuellen Computers klicken.

Unter Umständen müssen Sie die DNS-Weiterleitung korrigieren. Starten Sie hierzu den DNS-Manager.

1.  Klicken Sie im DNS-Manager mit der rechten Maustaste auf den Servernamen, klicken Sie auf „Eigenschaften“, und klicken Sie anschließend auf die Registerkarte „Weiterleitungen“.

2.  Klicken Sie auf die Schaltfläche „Bearbeiten“, entfernen Sie alle Weiterleitungen, und klicken Sie anschließend auf „OK“.

3.  Vergewissern Sie sich, dass das Kontrollkästchen „Stammhinweise verwenden, wenn keine Weiterleitungen verfügbar sind“ aktiviert ist, und klicken Sie auf „OK“.

## Einrichten von Linux-Computeknoten
Die Linux Computeknoten werden mithilfe der gleichen Bereitstellungsvorlage bereitgestellt wie der Hauptknoten weiter oben. Kopieren Sie die Datei **MyCluster.xml** von Ihrem lokalen Computer auf den Hauptknoten, und aktualisieren Sie das Tag **NodeCount** mit der Anzahl von Knoten, die Sie bereitstellen möchten (<= 20). Stellen Sie sicher, dass in Ihrem Azure-Kontingent genügend Kerne verfügbar sind, da jede A9-Instanz 16 Kerne Ihres Abonnements belegt. Falls Sie innerhalb des gleichen Budgets mehr virtuelle Computer verwenden möchten, können Sie anstelle von A9-Instanzen A8-Instanzen (acht Kerne) verwenden.

Kopieren Sie auf dem Hauptknoten die HPC Pack-IaaS-Bereitstellungsskripts.

Führen Sie die folgenden Azure PowerShell-Befehle an einer Eingabeaufforderung mit erhöhten Rechten aus:

1.  Führen Sie **Add-AzureAccount** aus, um eine Verbindung mit Ihrem Azure-Abonnement herzustellen.

2.  Sollten Sie über mehrere Abonnements verfügen, führen Sie **Get-AzureSubscription** aus, um sie aufzuführen.

3.  Führen Sie den Befehl **Select-AzureSubscription -SubscriptionName xxxx -Default** aus, um ein Standardabonnement festzulegen.

4.  Führen Sie **.\\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml** aus, um die Bereitstellung von Linux-Computeknoten zu starten. ![Hauptknotenbereitstellung][hndeploy]

Öffnen Sie das HPC Pack-Cluster-Manager-Tool. Nach einigen Minuten werden Linux-Computeknoten regulär in der Liste mit Cluster-Computeknoten angezeigt. Im klassischen Bereitstellungsmodus werden IaaS-VMs nacheinander erstellt. Wenn also die Anzahl von Knoten wichtig ist, kann es eine ganze Weile dauern, bis alle bereitgestellt wurden. ![Linux-Knoten im HPC Pack-Cluster-Manager][clustermanager]

Nachdem nun alle Knoten im Cluster betriebsbereit sind, müssen noch einige weitere Infrastruktureinstellungen konfiguriert werden.

## Einrichten der Azure-Dateifreigabe für Windows- und Linux-Knoten
Um uns die Arbeit zu erleichtern, verwenden wir Azure-Dateien. Diese bieten CIFS-Funktionen sowie Azure-Blobs als permanenten Speicher für unsere Skripts, Anwendungspakete und Datendateien. Diese Lösung ist zwar nicht besonders skalierbar, dafür aber sehr einfach und erfordert keine dedizierten virtuellen Computer.

Erstellen Sie eine Azure-Dateifreigabe. Eine entsprechende Anleitung finden Sie im Artikel [Erste Schritte mit Azure File Storage unter Windows](..\storage\storage-dotnet-how-to-use-files.md).

Lassen Sie den Namen Ihres Speicherkontos (**Saname**), den Namen der Dateifreigabe (**sharename**) und den Speicherkontoschlüssel (**sakey**) unverändert.

### Einbinden der Azure-Dateifreigabe auf dem Hauptknoten
Öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten, und führen Sie den folgenden Befehl aus, um die Anmeldeinformationen im Tresor des lokalen Computers zu speichern:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Führen Sie dann den folgenden Befehl aus, um die Azure-Dateifreigabe einzubinden:

```
    net use Z: \<saname>.file.core.windows.net<sharename> /persistent:yes
```

### Einbinden der Azure-Dateifreigabe auf Linux-Computeknoten
Das HPC Pack enthält unter anderem das praktische Tool „clusrun“. Mithilfe dieses Befehlszeilentools können Sie einen Befehl gleichzeitig für eine Gruppe von Computeknoten ausführen. In unserem Fall verwenden wir das Tool, um die Azure-Dateifreigabe einzubinden und so zu konfigurieren, dass sie auch nach einem Neustart erhalten bleibt. Führen Sie auf dem Hauptknoten in einer Eingabeaufforderung mit erhöhten Rechten folgende Befehle aus:

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

## Aktualisieren der Linux-Treiber
Die Infiniband-Treiber der Linux-Computeknoten müssen unter Umständen aktualisiert werden. Ob dies erforderlich ist und wie Sie dazu vorgehen müssen, erfahren Sie unter [Aktualisieren Sie die Linux RDMA-Treiber für SLES 12](virtual-machines-linux-classic-rdma-cluster.md/#update-the-linux-rdma-drivers-for-sles-12).

## Installieren von StarCCM+
Die Azure-VM-Instanzen A8 und A9 bieten Infiniband-Unterstützung und RDMA-Funktionen. Die Kernel-Treiber, die diese Funktionen ermöglichen, stehen zum Zeitpunkt der Artikelerstellung für Windows 2012 R2- und SUSE 12-Images im Azure-Katalog zur Verfügung. CentOS 7.x wird in Kürze verfügbar. Microsoft MPI und Intel MPI (Version 5.x) sind die beiden MPI-Bibliotheken, die diese Treiber in Azure unterstützen.

CD-Adapco StarCCM+ (ab Version 11.x) ist mit Intel MPI (Version 5.x) gebündelt, sodass Infiniband-Unterstützung für Azure enthalten ist.

Das Linux64 StarCCM+-Paket ist im [CD-Adapco-Portal](https://steve.cd-adapco.com) erhältlich. In unserem Fall haben Version 11.02.010 mit gemischter Genauigkeit verwendet.

Erstellen Sie auf dem Hauptknoten in der Azure-Dateifreigabe **/hpcdata** ein Shellskript namens **setupstarccm.sh** mit folgendem Inhalt. Dieses Skript wird auf jedem Computeknoten gestartet, um StarCCM+ lokal einzurichten.

#### Beispielskript für „setupstarcm.sh“
```
    #!/bin/bash
    # setupstarcm.sh setup StarCCM+ locally

    # create the CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # copy the starccm package from the file share to the local dir
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # extract the package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # start a silent installation of starccm without flexlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Öffnen Sie nun eine Eingabeaufforderung mit erhöhten Rechten, und führen Sie den folgenden Befehl aus, um StarCCM+ auf allen Ihren Linux Computeknoten einzurichten:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Während der Befehlsausführung können Sie mithilfe der Heatmap des Cluster-Managers die CPU-Auslastung überwachen. Nach einigen Minuten sollten alle Knoten ordnungsgemäß eingerichtet sein.

## Ausführen von StarCCM+-Aufträgen
HPC Pack wird wegen seiner Auftragsplanerfunktionen zur Ausführung von StarCCM+-Aufträgen verwendet. Zu diesem Zweck benötigen wir einige Skripts, mit denen wir den Auftrag starten und StarCCM+ ausführen. Die Eingabedaten werden der Einfachheit halber zunächst auf der Azure-Dateifreigabe gespeichert. Das folgende Powershell-Skript wird verwendet, um einen StarCCM+-Auftrag der Warteschlange hinzuzufügen. Es akzeptiert drei Argumente:

*  Den Modellnamen
*  Die Anzahl der zu verwendenden Knoten
*  Die Anzahl der pro Knoten zu verwendenden Kerne

Da StarCCM+ die gesamte Speicherbandbreite beanspruchen kann, empfiehlt es sich in der Regel, weniger Kerne pro Computeknoten zu verwenden und neue Knoten hinzuzufügen. Das genaue Kernverhältnis pro Knoten hängt von der Prozessorfamilie und von der Verbindungsgeschwindigkeit ab.

Die Knoten werden exklusiv für den Auftrag zugeordnet und können nicht für andere Aufträge verwendet werden. Der Auftrag wird nicht direkt als MPI-Auftrag gestartet. Das MPI-Startprogramm wird über das Shellskript **runstarccm.sh** gestartet.

Das Eingabemodell und **runstarccm.sh** müssen in der zuvor eingebundenen Freigabe **/hpcdata** gespeichert werden.

Die Protokolldateien werden mit der Auftrags-ID benannt und zusammen mit den StarCCM+-Ausgabedateien in der Freigabe **/hpcdata** gespeichert.


#### Beispielskript für „SubmitStarccmJob.ps1“
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # create a new job, this will give us the JobId used to identify the name of the uploaded package in azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # submit job 	
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
Ersetzen Sie **runner.java** durch Ihr bevorzugtes StarCCM+-Javamodell-Startprogramm und Ihren bevorzugten Protokollierungscode.

#### Beispielskript für „runstarccm.sh“
```
    #!/bin/bash
    echo "start"
    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set mpirun runtime environment
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

    # Run the starccm with hostfile arg
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

In unserem Test haben wir ein Power-On-Demand-Lizenztoken verwendet, für das Sie die Umgebungsvariable **$CDLMD\_LICENSE\_FILE** auf ****1999@flex.cd-adapco.com** und den Schlüssel in der Befehlszeilenoption **-podkey** festgelegt haben.

Nach der Initialisierung extrahiert das Skript (auf der Grundlage der von HPC Pack festgelegten Umgebungsvariablen vom Typ **$CCP\_NODES\_CORES**) eine Liste mit Knoten zum Erstellen einer Hostdatei für das MPI-Startprogramm. Diese Hostdatei enthält eine Liste mit den Computeknotennamen für den Auftrag (jeweils ein Name pro Zeile).

Für das Format von **$CCP\_NODES\_CORES** wird das folgende Muster verwendet:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Hierbei gilt:

* `<Number of nodes>`: Anzahl der Knoten, die diesem Auftrag zugeordnet sind.  
* `<Name of node_n_...>`: Namen der einzelnen Knoten, die dem Auftrag zugeordnet sind.
* `<Cores of node_n_...>`: Anzahl der Kerne auf dem Knoten, die dem Auftrag zugeordnet sind.

Die Berechnung der Kernanzahl (**$NBCORES**) basiert auch auf der Anzahl von Knoten (**$NBNODES**) und der Anzahl von Kernen pro Knoten (angegeben als Parameter **$NBCORESPERNODE**).

Für Intel MPI unter Azure werden folgende MPI-Optionen verwendet:

*   -mpi intel => Angabe von Intel MPI
*   -fabric UDAPL => Verwendung von Azure Infiniband-Verben
*   -cpubind bandwidth,v => Optimierung der Bandbreite für MPI mit StarCCM+
*   -mppflags "-ppn $NBCORESPERNODE -genv I\_MPI\_DAPL\_PROVIDER=ofa-v2-ib0 -genv I\_MPI\_DAPL\_UD=0 -genv I\_MPI\_DYNAMIC\_CONNECTION=0" => Intel MPI-Einstellungen für die Verwendung von Azure Infiniband sowie zum Festlegen der erforderlichen Kernanzahl pro Knoten
*   -batch => Start von StarCCM+ im Batchmodus ohne Benutzeroberfläche


Vergewissern Sie sich vor dem Starten eines Auftrags im Cluster-Manager, dass die Knoten betriebsbereit und online sind. Führen Sie anschließend in einem PowerShell-Befehlsfenster Folgendes aus:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## Beenden von Knoten
Nach Abschluss der Tests können Sie Knoten mithilfe der folgenden HPC Pack-PowerShell-Befehle beenden und starten:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## Nächste Schritte
Experimentieren Sie mit anderen Linux-Workloads. Beispiele:

* [Ausführen von NAMD mit dem Microsoft HPC Pack auf Linux-Computeknoten in Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Ausführen von OpenFOAM mit Microsoft HPC Pack auf einem Linux-RDMA-Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)


<!--Image references-->
[hndeploy]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/hndeploy.png
[clustermanager]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/ClusterManager.png

<!---HONumber=AcomDC_0427_2016-->