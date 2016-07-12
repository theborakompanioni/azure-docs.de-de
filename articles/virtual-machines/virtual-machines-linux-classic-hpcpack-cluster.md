<properties
 pageTitle="Virtuelle Linux-Computeknoten-Computer in einem HPC Pack-Cluster | Microsoft Azure"
 description="Erfahren Sie, wie Sie einen HPC Pack-Cluster in Azure für Linux-HPC-Workloads (High Performance Computing) erstellen und verwenden."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="06/23/2016"
 ms.author="danlep"/>

# Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure

Richten Sie einen Microsoft HPC Pack-Cluster in Azure ein, der einen Hauptknoten unter Windows Server sowie mehrere Computeknoten mit einer Linux-Distribution enthält. Informieren Sie sich über die Optionen zum Verschieben von Daten zwischen den Linux-Knoten und dem Windows-Hauptknoten des Clusters. Finden Sie heraus, wie Sie Linux-HPC-Aufträge an den Cluster senden.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)].


Das folgende Diagramm gibt einen allgemeinen Überblick über den HPC Pack-Cluster, den Sie erstellen und mit dem Sie arbeiten werden.

![HPC Pack-Cluster mit Linux-Knoten][scenario]


## Bereitstellen eines HPC Pack-Clusters mit Linux Compute-Knoten

Nachfolgend sind zwei empfohlene Vorgehensweisen zum Erstellen eines HPC Pack-Clusters in Azure mit Linux-Computeknoten beschrieben:

* **Azure Resource Manager-Vorlage**: Verwenden Sie eine Vorlage aus dem Azure Marketplace oder eine Schnellstartvorlage aus der Community, um die Erstellung des Clusters im Resource Manager-Bereitstellungsmodell zu automatisieren. Beispielsweise erstellt die Vorlage [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) (HPC Pack-Cluster für Linux-Workloads) im Azure Marketplace eine vollständige HPC Pack-Clusterinfrastruktur für Linux HPC-Workloads.

* **PowerShell-Skript**: Verwenden Sie das [Microsoft HPC Pack-IaaS-Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) (**New-HpcIaaSCluster.ps1**), um eine vollständige Clusterbereitstellung im klassischen Bereitstellungsmodell zu automatisieren. Dieses Azure PowerShell-Skript verwendet ein HPC Pack-VM-Image im Azure Marketplace für die schnelle Bereitstellung und bietet eine umfassende Reihe von Konfigurationsparametern, um Linux-Computeknoten bereitzustellen.

Einen Überblick über die Optionen für HPC Pack-Clusterbereitstellungen in Azure finden Sie unter [Optionen zum Erstellen und Verwalten eines HPC-Clusters (High Performance Computing) in Azure mit Microsoft HPC Pack](virtual-machines-linux-hpcpack-cluster-options.md).

### Voraussetzungen

* **Azure-Abonnement:** Sie können ein Abonnement entweder im Azure Global- oder Azure China-Dienst nutzen. Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.

* **Kernkontingent**: Unter Umständen muss das Kontingent für die Kerne erhöht werden. Dies gilt insbesondere, wenn Sie mehrere Clusterknoten mit Multicore-VM-Größen bereitstellen. Um ein Kontingent zu erhöhen, können Sie kostenlos eine Anfrage an den Onlinekundensupport richten.

* **Linux-Distributionen**: HPC Pack unterstützt derzeit die folgenden Linux-Distributionen für Computeknoten. Sie können Marketplace-Versionen dieser Distributionen verwenden (sofern verfügbar) oder die Distributionen selbst bereitstellen.

    * CentOS-basiert: 6.5, 6.6, 6.7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1 HPC
    * Red Hat Enterprise Linux: 6.7, 6.8, 7.2
    * SUSE Linux Enterprise Server: SLES 12, SLES 12 (Premium), SLES 12 für HPC, SLES 12 für HPC (Premium)
    * Ubuntu Server: 14.04 LTS, 16.04 LTS



    >[AZURE.TIP]Um das Azure RDMA-Netzwerk mit Computeknoten-VMs der Größen A8 und A9 zu nutzen, geben Sie eines der SUSE Linux Enterprise Server 12 HPC-Images oder CentOS-basierten HPC-Images aus dem Marketplace an. Weitere Informationen hierzu finden Sie unter [Informationen zu den rechenintensiven A8-, A9-, A10- und A11-Instanzen](virtual-machines-linux-a8-a9-a10-a11-specs.md).

Weitere Voraussetzungen für die Bereitstellung des Clusters über das HPC Pack-IaaS-Bereitstellungsskript:

* **Clientcomputer**: Sie benötigen einen Windows-basierten Clientcomputer für die Ausführung des Skripts zur Clusterbereitstellung.

* **Azure PowerShell**: [Installieren und konfigurieren Sie Azure PowerShell](../powershell-install-configure.md) (ab Version 0.8.10) auf Ihrem Clientcomputer.

* **HPC Pack-IaaS-Bereitstellungsskript**: Laden Sie die neueste Version des Skripts aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949) herunter, und entpacken Sie sie. Führen Sie `.\New-HPCIaaSCluster.ps1 –Version` aus, um die Version des Skripts zu überprüfen. Dieser Artikel basiert auf der Skriptversion 4.4.1 oder höher.

### Bereitstellungsoption 1: Verwenden einer Resource Manager-Vorlage

1. Wechseln Sie im Azure Marketplace zur Vorlage [HPC Pack cluster for Linux workloads](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/), und klicken Sie auf **Bereitstellen**.

2. Überprüfen Sie die Informationen im Azure-Portal, und klicken Sie auf **Erstellen**.

    ![Erstellung über das Portal][portal]

3. Geben Sie auf dem Blatt **Grundeinstellungen** einen Namen für den Cluster ein, der auch für die Hauptknoten-VM verwendet wird. Sie können eine vorhandene Ressourcengruppe wählen oder eine neue Gruppe für die Bereitstellung erstellen.

4. Für eine erste Bereitstellung können Sie auf dem Blatt mit den Einstellungen des Hauptknotens im Allgemeinen die Standardeinstellungen übernehmen.

    >[AZURE.NOTE]Über die optionale Einstellung **URL für Skript nach der Konfiguration** können Sie ein öffentlich verfügbares Windows PowerShell-Skript angeben, das auf der Hauptknoten-VM ausgeführt werden soll, sobald diese in Betrieb genommen wurde.
    
5. Auf dem Blatt mit den Einstellungen für Computeknoten wählen Sie ein Benennungsmuster für die Knoten, die Anzahl und die Größe der Knoten sowie die Linux-Distribution, die bereitgestellt werden soll.

6. Auf dem Blatt **Infrastruktureinstellungen** geben Sie Folgendes ein: den Namen für das virtuelle Netzwerk und die Active Directory-Domäne für den Cluster, die Anmeldeinformationen für den Domänen- und VM-Administrator sowie ein Benennungsmuster für die Speicherkonten, die für den Cluster benötigt werden.

    >[AZURE.NOTE]HPC Pack verwendet die Active Directory-Domäne zur Authentifizierung von Clusterbenutzern.

7. Klicken Sie nach der Ausführung der Tests zur Überprüfung und der Prüfung der Vereinbarung auf **Kaufen**.


### Bereitstellungsoption 2: Verwenden des IaaS-Bereitstellungsskripts

Das HPC Pack-IaaS-Bereitstellungsskript verwendet als Eingabe eine XML-Konfigurationsdatei mit der Beschreibung der Infrastruktur des HPC-Clusters. Mit der folgenden Beispielkonfigurationsdatei wird ein kleiner Cluster bereitgestellt, der einen HPC Pack-Hauptknoten und zwei CentOS 7.0-Linux-Computeknoten der Größe A7 umfasst. Ändern Sie die Datei entsprechend den Anforderungen Ihrer Umgebung und der gewünschten Clusterkonfiguration, und speichern Sie sie unter einem Namen wie „HPCDemoConfig.xml“. Sie müssen beispielsweise den Namen Ihres Abonnements und einen eindeutigen Speicherkontonamen und Clouddienstnamen angeben, und Sie könnten ein anderes unterstütztes Linux-Image für den Computeknoten auswählen. Weitere Informationen zu den Elementen der Konfigurationsdatei finden Sie in der Datei „Manual.rtf“ im Skriptordner und unter [Create an HPC cluster with the HPC Pack IaaS deployment script (Erstellen eines HPC-Clusters mit dem HPC Pack-IaaS-Bereitstellungsskript)](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>centos7rdmavnetje</VNetName>
    <SubnetName>CentOS7RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS7RDMA-HN</VMName>
    <ServiceName>centos7rdma-je</ServiceName>
  <VMSize>ExtraLarge</VMSize>
  <EnableRESTAPI />
  <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS7RDMA-LN%1%</VMNamePattern>
    <ServiceName>centos7rdma-je</ServiceName>
    <VMSize>A7</VMSize>
    <NodeCount>2</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

**So führen Sie das HPC Pack-IaaS-Bereitstellungsskript aus**

1. Öffnen Sie die PowerShell-Konsole auf dem Clientcomputer als Administrator.

2. Navigieren Sie zum Skriptordner (in diesem Beispiel „E:\\IaaSClusterScript“).

    ```
    cd E:\IaaSClusterScript
    ```

3. Führen Sie den folgenden Befehl aus, um den HPC Pack-Cluster bereitzustellen. In diesem Beispiel wird davon ausgegangen, dass sich die Konfigurationsdatei unter „E:\\HPCDemoConfig.xml“ befindet.

    ```
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```

    Das Skript generiert automatisch eine Protokolldatei, da der **-LogFile**-Parameter nicht angegeben ist. Die Protokolle werden nicht in Echtzeit geschrieben, sondern am Schluss des Validierungs- und Bereitstellungsvorgangs erfasst. Wenn der PowerShell-Prozess angehalten wird, während das Skript ausgeführt wird, gehen einige Protokolle verloren.

    a. Da im obigen Befehl kein **AdminPassword** angegeben wird, werden Sie zur Eingabe des Kennworts für den Benutzer *MyAdminName* aufgefordert.

    b. Das Skript beginnt dann mit der Überprüfung der Konfigurationsdatei. Je nach Netzwerkverbindung dauert dies bis zu einigen Minuten.

    ![Überprüfen][validate]

    c. Nachdem die Überprüfungen erfolgreich waren, führt das Skript die Ressourcen auf, die für den HPC-Cluster erstellt werden. Geben Sie *Y* ein, um fortzufahren.

    ![Ressourcen][resources]

    d. Das Skript beginnt mit der Bereitstellung des HPC Pack-Clusters und schließt die Konfiguration ohne weitere manuelle Schritte ab. Dies kann einige Minuten dauern.

    ![Bereitstellen][deploy]

## Verbindung mit dem Hauptknoten herstellen

Nach erfolgreichem Abschluss der Bereitstellung [verbinden Sie sich über Remotedesktop mit dem Hauptknoten](virtual-machines-windows-connect-logon.md). Verwenden Sie dazu die Domänenanmeldeinformationen, die Sie bei der Bereitstellung des Clusters angegeben haben (z. B. *hpc\\clusteradmin*).

Starten Sie auf dem Hauptknoten den HPC-Cluster-Manager, um den Status des HPC Pack-Clusters zu überprüfen. Sie können Linux-Compute-Knoten auf die gleiche Weise wie Windows-Compute-Knoten verwalten und überwachen. Beispielsweise werden unter **Ressourcenverwaltung** die Linux-Knoten aufgeführt (diese Knoten werden mit der **LinuxNode**-Vorlage bereitgestellt).

![Knoten-Verwaltung][management]

In der Ansicht **Wärmebild** werden die Linux-Knoten ebenfalls angezeigt.

![Heat Map][heatmap]

## Gewusst wie: Verschieben von Daten in einem Cluster mit Linux-Knoten

Sie haben mehrere Optionen zum Verschieben von Daten zwischen Linux-Knoten und dem Windows-Head-Knoten des Clusters. Wir stellen drei allgemeine Methoden vor.

* **Azure File Storage**: Stellt eine verwaltete SMB-Dateifreigabe zum Speichern von Datendateien im Azure-Speicher zur Verfügung. Sowohl Windows-Knoten als auch Linux-Knoten können eine Azure-Dateifreigabe als ein Laufwerk oder Ordner gleichzeitig bereitstellen, auch wenn sie in verschiedenen virtuellen Netzwerken bereitgestellt werden.

* **Hauptknoten-SMB-Freigabe**: Bindet einen standardmäßigen freigegebenen Windows-Ordner des Hauptknotens auf Linux-Knoten ein.

* **Hauptknoten-NFS-Server**: Bietet eine Lösung zur Dateifreigabe für eine gemischte Windows- und Linux-Umgebung.

### Azure-Dateispeicher

Der [Azure-Dateidienst](https://azure.microsoft.com/services/storage/files/) stellt Dateifreigaben mit dem SMB 2.1-Standardprotokoll bereit. Azure-VMs und Clouddienste können Dateidaten in verschiedenen Anwendungskomponenten über eingebundene Freigaben teilen, und lokale Anwendungen können über die Dateispeicher-API auf freigegebene Dateien zugreifen.

Die Schritte zum Erstellen einer Azure-Dateifreigabe sowie zum Einbinden der Freigabe auf dem Hauptknoten sind im Detail unter [Erste Schritte mit Azure File Storage unter Windows](../storage/storage-dotnet-how-to-use-files.md) beschrieben. Informationen zum Einbinden der Azure-Dateifreigabe auf den Linux-Knoten finden Sie unter [Verwenden von Azure File Storage unter Linux](../storage/storage-how-to-use-files-linux.md). Informationen zum Einrichten bestehender Verbindungen finden Sie unter [Beibehalten der Verbindung mit Microsoft Azure-Dateien](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).

Im folgenden Beispiel erstellen Sie eine Azure-Dateifreigabe für ein Speicherkonto. Um die Freigabe auf dem Hauptknoten einzubinden, öffnen Sie eine Eingabeaufforderung, und geben Sie die folgenden Befehle ein:

```
> cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>
> net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

In diesem Beispiel ist „allvhdsje“ der Name des Speicherkontos, „storageaccountkey“ ist der Speicherkontoschlüssel, und „rdma“ ist der Name der Azure-Dateifreigabe. Die Azure-Dateifreigabe wird auf Z: auf dem Head-Knoten bereitgestellt.

Führen Sie zum Bereitstellen der Azure-Dateifreigabe auf Linux-Knoten einen **clusrun**-Befehl für den Hauptknoten aus. **[clusrun](https://technet.microsoft.com/library/cc947685.aspx)** ist ein nützliches Tool für HPC Pack, mit dem administrative Aufgaben auf mehreren Knoten ausgeführt werden können. (Siehe auch [„clusrun“ für Linux-Knoten](#Clusrun-for-Linux-nodes) in diesem Artikel.)

Öffnen Sie ein Windows PowerShell-Fenster und geben Sie die folgenden Befehle ein.

```
PS > clusrun /nodegroup:LinuxNodes mkdir -p /rdma

PS > clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

Der erste Befehl erstellt einen Ordner namens „/rdma“ auf allen Knoten in der „LinuxNodes“-Gruppe. Der zweite Befehl lädt die Azure-Dateifreigabe „allvhdsjw.file.core.windows.net/rdma“ auf den Ordner „/rdma“ mit Dir- und Datei-Modus-Bits auf 777 gesetzt. Im zweiten Befehl ist „Allvhdsje“ der Name Ihres Speicherkontos und „Storageaccountkey“ ist der Speicherkontoschlüssel.

>[AZURE.NOTE]Das Symbol „`“ im zweiten Befehl ist ein Escapesymbol für PowerShell. „`,“ bedeutet, dass „,“ (das Komma) Teil des Befehls ist.

### Head-Knoten-Freigabe

Alternativ können Sie einen freigegebenen Ordner des Hauptknotens auf Linux-Knoten einbinden. Dies ist die einfachste Methode zum Freigeben von Dateien, allerdings müssen der Head-Knoten und alle Linux-Knoten im gleichen virtuellen Netzwerk bereitgestellt werden. Gehen Sie wie folgt vor:

1. Erstellen Sie einen Ordner auf dem Head-Knoten, und geben Sie ihn für alle Benutzer mit Lese-/Schreibberechtigungen frei. Geben Sie beispielsweise „D:\\OpenFOAM“ auf dem Hauptknoten als „\\CentOS7RDMA-HN\\OpenFOAM“ frei. Hier ist „CentOS7RDMA-HN“ der Hostname des Hauptknotens.

    ![Berechtigungen für die Dateifreigabe][fileshareperms]

    ![Dateifreigabe][filesharing]

2. Öffnen Sie ein Windows PowerShell-Fenster, und führen Sie die folgenden Befehle aus, um den freigegebenen Ordner bereitzustellen.

```
PS > clusrun /nodegroup:LinuxNodes mkdir -p /openfoam

PS > clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
```

Der erste Befehl erstellt einen Ordner namens „/openfoam“ auf allen Knoten in der Gruppe „LinuxNodes“. Der zweite Befehl lädt den freigegebenen Ordner „//CentOS7RDMA-HN/OpenFOAM“ in den Ordner, mit Dir- und Datei-Modus-Bits auf 777 festgelegt. Der Benutzername und das Kennwort im Befehl sollten dem Benutzernamen und dem Kennwort eines Clusterbenutzers auf dem Hauptknoten entsprechen. Weitere Informationen finden Sie unter [Add or Remove Cluster Users (Hinzufügen oder Entfernen von Clusterbenutzern)](https://technet.microsoft.com/library/ff919330.aspx).

>[AZURE.NOTE]Das Symbol „`“ im zweiten Befehl ist ein Escapesymbol für PowerShell. „`,“ bedeutet, dass „,“ (das Komma) Teil des Befehls ist.


### NFS-Server

Der NFS-Dienst ermöglicht Ihnen die Freigabe und das Migrieren von Dateien zwischen Windows Server 2012-Computern über das SMB-Protokoll und zwischen Linux-basierten Computern über das NFS-Protokoll. Der NFS-Server und alle anderen Knoten müssen im gleichen virtuellen Netzwerk bereitgestellt werden. Im Vergleich zu einer SMB-Freigabe bietet diese Konfiguration eine bessere Kompatibilität mit Linux-Knoten (beispielsweise werden Dateilinks unterstützt).

1. Führen Sie zum Installieren und Einrichten eines NFS-Servers die Schritte unter [Server for Network File System First Share End-to-End](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx) (Umfassende Informationen zur ersten Freigabe des Servers für NFS; in englischer Sprache) aus.

    Erstellen Sie z. B. eine NFS-Freigabe mit dem Namen „Nfs“ mit den folgenden Eigenschaften.

    ![NFS-Autorisierung][nfsauth]

    ![Berechtigungen für die NFS-Freigabe][nfsshare]

    ![Berechtigungen für die NFS NTFS-Freigabe][nfsperm]

    ![NFS-Verwaltungseigenschaften][nfsmanage]

2. Öffnen Sie ein Windows PowerShell-Fenster, und führen Sie die folgenden Befehle aus, um die NFS-Freigabe einzubinden.

  ```
  PS > clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare
  PS > clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
  ```

  Der erste Befehl erstellt einen Ordner namens „/nfsshared“ auf allen Knoten in der „LinuxNodes“-Gruppe. Der zweite Befehl lädt die NFS-Freigabe „CentOS7RDMA-HN:/nf“ auf den Ordner. Hier ist „CentOS7RDMA-HN:/nf“ der Remotepfad der NFS-Freigabe.

## So senden Sie Aufträge ab
Es gibt verschiedene Methoden zum Übermitteln von Aufträgen an den HPC Pack-Cluster.

* HPC Cluster-Manager oder HPC-Auftrags-Manager-GUI

* HPC-Webportal

* REST-API

Das Senden eines Auftrags an den Cluster in Azure über HPC Pack-GUI-Tools und dem HPC-Webportal ist das gleiche wie für Windows-Compute-Knoten. Informationen finden Sie unter [HPC Pack Job Manager (HPC Pack-Auftrags-Manager)](https://technet.microsoft.com/library/ff919691.aspx) und [How to submit jobs from an on-premises client computer (Übermitteln von Aufträgen von einem lokalen Clientcomputer)](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).

Informationen zum Übermitteln von Aufträgen über die REST-API finden Sie unter [Erstellen und Übermitteln von Aufträgen mithilfe der REST-API in Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx). Informationen zum Übermitteln von Aufträgen von einem Linux-Client finden Sie zudem im Python-Beispiel im [HPC Pack-SDK](https://www.microsoft.com/download/details.aspx?id=47756).

## „clusrun“ für Linux-Knoten

Das HPC Pack-Tool **clusrun** kann zum Ausführen von Befehlen auf Linux-Knoten verwendet werden, und zwar entweder über eine Eingabeaufforderung oder über den HPC-Cluster-Manager. Im Folgenden finden Sie einige einfache Beispiele.

* Zeigen Sie aktuelle Benutzernamen auf allen Knoten im Cluster an.

    ```
    > clusrun whoami
    ```

* Installieren Sie das **gdb**-Debugtool mit **yum** auf allen Knoten in der linuxnodes-Gruppe, und starten Sie die Knoten nach 10 Minuten neu.

    ```
    > clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```

* Erstellen Sie ein Shellskript, das jede Zahl von 1 bis 10 für eine Sekunde auf den einzelnen Linux-Knoten im Cluster anzeigt. Führen Sie das Skript dann aus, und zeigen Sie direkt die Ausgabe für die Knoten an.

    ```
    > clusrun /interleaved /nodegroup:linuxnodes echo "for i in {1..10}; do echo \\"\$i\\"; sleep 1; done" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

>[AZURE.NOTE] Sie müssen möglicherweise bestimmte Escapezeichen in **clusrun**-Befehlen verwenden. Verwenden Sie das Zeichen „^“ wie im folgenden Beispiel gezeigt in einer Eingabeaufforderung als Escapezeichen für „>“.

## Nächste Schritte

* Versuchen Sie, den Cluster auf eine größere Anzahl von Knoten zu skalieren, oder versuchen Sie, eine Linux-Workload auf dem Cluster auszuführen. Ein Beispiel finden Sie unter [Ausführen von NAMD mit dem Microsoft HPC Pack auf Linux-Computeknoten in Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md).

* Versuchen Sie, einen Cluster mit Computeknoten der Größe [A8 oder A9](virtual-machines-windows-a8-a9-a10-a11-specs.md) zum Ausführen von MPI-Workloads zu verwenden. Ein Beispiel finden Sie unter [Ausführen von OpenFOAM mit Microsoft HPC Pack auf einem Linux-RDMA-Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md).

* Wenn Sie sich für das Arbeiten mit Linux-Knoten in einem lokalen HPC Pack-Cluster interessieren, finden Sie im [TechNet-Leitfaden](https://technet.microsoft.com/library/mt595803.aspx) weitere Informationen.

<!--Image references-->
[scenario]: ./media/virtual-machines-linux-classic-hpcpack-cluster/scenario.png
[portal]: ./media/virtual-machines-linux-classic-hpcpack-cluster/portal.png
[validate]: ./media/virtual-machines-linux-classic-hpcpack-cluster/validate.png
[resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster/resources.png
[deploy]: ./media/virtual-machines-linux-classic-hpcpack-cluster/deploy.png
[management]: ./media/virtual-machines-linux-classic-hpcpack-cluster/management.png
[heatmap]: ./media/virtual-machines-linux-classic-hpcpack-cluster/heatmap.png
[fileshareperms]: ./media/virtual-machines-linux-classic-hpcpack-cluster/fileshare1.png
[filesharing]: ./media/virtual-machines-linux-classic-hpcpack-cluster/fileshare2.png
[nfsauth]: ./media/virtual-machines-linux-classic-hpcpack-cluster/nfsauth.png
[nfsshare]: ./media/virtual-machines-linux-classic-hpcpack-cluster/nfsshare.png
[nfsperm]: ./media/virtual-machines-linux-classic-hpcpack-cluster/nfsperm.png
[nfsmanage]: ./media/virtual-machines-linux-classic-hpcpack-cluster/nfsmanage.png

<!---HONumber=AcomDC_0629_2016-->