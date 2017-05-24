---
title: Einrichten eines HPC-Hybridclusters mit Microsoft HPC Pack | Microsoft-Dokumentation
description: "Erfahren Sie mehr über die Verwendung von Microsoft HPC Pack und Azure für die Einrichtung eines kleinen HPC-Hybridclusters (High Performance Computing)."
services: cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: b11f3e1b-b9e1-4b0d-8455-6607b88814e9
ms.service: cloud-services
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: danlep
ms.translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 0fcfcc53641ebdf8a668b353db8eebb3bc64795d
ms.contentlocale: de-de
ms.lasthandoff: 03/31/2017


---
# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a>Einrichten eines HPC-Hybridclusters (High Performance Computing) mit Microsoft HPC Pack und bedarfsgesteuerten Azure-Computeknoten
Nutzen Sie Microsoft HPC Pack 2012 R2 und Azure für die Einrichtung eines kleinen HPC-Hybridclusters (High Performance Computing). Das Cluster wird aus einem lokalen Hauptknoten (einem Computer mit dem Windows Server-Betriebssystem und HPC Pack) und einigen Serverknoten bestehen, die Sie bei Bedarf als Workerrollen-Instanzen in einem Azure-Clouddienst bereitstellen. Dann können Sie auf dem Hybrid-Cluster Rechenaufträge (Compute Jobs) ausführen.

![Hybrid-HPC-Cluster][Overview] 

Dieses Tutorial veranschaulicht einen gelegentlich als Cloud-Bursting bezeichneten Ansatz. Es erklärt, wie mit skalierbaren, bei Bedarf verfügbaren Computeressourcen in Azure rechenintensive Anwendungen ausgeführt werden können.

In diesem Tutorial wird davon ausgegangen, dass Sie noch keine Erfahrung mit Computeclustern oder HPC Pack 2012 R2 haben. Es soll Ihnen lediglich helfen, ein Hybrid-Rechencluster für Demonstrationszwecke schnell bereitzustellen. Welche Überlegungen und Schritte zur Bereitstellung eines größeren Hybridclusters mit HPC Pack in einer Produktionsumgebung anzustellen bzw. auszuführen sind oder wie Sie HPC Pack 2016 verwenden, ist in der [ausführlichen Anleitung](http://go.microsoft.com/fwlink/p/?LinkID=200493) beschrieben. Informationen zu weiteren Szenarios mit HPC Pack, z.B. zur automatischen Bereitstellung von Clustern auf virtuellen Azure-Computern, finden Sie unter [Optionen zum Erstellen und Verwalten eines Windows HPC-Clusters (High Performance Computing) in Azure mit Microsoft HPC Pack](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="prerequisites"></a>Voraussetzungen
* **Azure-Abonnement** – Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie in wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/free/) einrichten.
* **Ein lokaler Computer, auf dem Windows Server 2012 R2 oder Windows Server 2012 ausgeführt wird** : Dieser Computer fungiert als Hauptknoten des HPC-Clusters. Falls Sie noch nicht Windows Server verwenden, können Sie eine [Evaluierungsversion](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)herunterladen und installieren.
  
  * Der Computer muss in eine Active Directory-Domäne eingebunden werden. Für ein Testszenario mit einer Neuinstallation von Windows Server können Sie die Serverrolle „Active Directory-Domänendienste“ hinzufügen und den Hauptknotencomputer als Domänencontroller in einer neuen Domänengesamtstruktur höher stufen. (Weitere Informationen finden Sie in der Windows Server-Dokumentation.)
  * Damit das HPC Pack unterstützt werden kann, muss das Betriebssystem in einer der folgenden Sprachen installiert werden: Englisch, Japanisch oder Chinesisch (vereinfacht).
  * Stellen Sie sicher, dass wichtige und kritische Updates installiert wurden.
* **HPC Pack 2012 R2** - [hier](http://go.microsoft.com/fwlink/p/?linkid=328024) das Installationspaket für die neueste Version kostenlos herunter, und kopieren Sie die Dateien auf den Hauptknotencomputer oder an einen Speicherort im Netzwerk. Wählen Sie Installationsdateien in derselben Sprache aus wie für die Installation von Windows Server.

    >[!NOTE]
    > Wenn Sie HPC Pack 2016 anstelle von HPC Pack 2012 R2 verwenden möchten, sind zusätzliche Konfigurationsschritte erforderlich. Informationen finden Sie in der [ausführlichen Anleitung](http://go.microsoft.com/fwlink/p/?LinkID=200493).
    > 
* **Domänenkonto** : Dieses Konto muss für die HPC Pack-Installation mit lokalen Administratorberechtigungen auf dem Hauptknoten konfiguriert werden.
* **TCP-Konnektivität an Port 443** vom Hauptknoten zu Azure.

## <a name="install-hpc-pack-on-the-head-node"></a>Installieren des HPC Pack auf dem Hauptknoten
Zuerst muss das Microsoft HPC Pack auf Ihrem lokalen Computer mit Windows Server installiert werden, der als Hauptknoten im Cluster fungiert.

1. Melden Sie sich bei dem Hauptknoten mit einem Domänenkonto an, das über lokale Administratorrechte verfügt.
2. Starten Sie den Installations-Assistenten von HPC Pack, indem Sie die in den HPC Pack-Installationsdateien enthaltene Datei Setup.exe ausführen.
3. Klicken Sie im **HPC Pack 2012 R2 Setup**-Bildschirm auf **New installation or add new features to an existing installation** (Neue Installation, oder neue Features zu einer vorhandenen Installation hinzufügen).
   
    ![HPC Pack 2012 Setup][install_hpc1]
4. Klicken Sie auf der **Seite mit dem Vertrag für Benutzer von Microsoft-Software** auf **Weiter**.
5. Klicken Sie auf der Seite **Select Installation Type** (Installationstyp auswählen) auf **Create a new HPC cluster by creating a head node** (Neuen HPC-Cluster durch Erstellung eines Hauptknotens erstellen), und klicken Sie dann auf **Weiter**.
   
    ![Installationstyp auswählen][install_hpc2]
6. Der Installations-Assistent führt vor der Installation verschiedene Tests durch. Klicken Sie auf **Next** on the **Installation Rules** , wenn alle Tests erfolgreich durchgeführt worden sind. Lesen Sie andernfalls die bereitgestellten Informationen, und nehmen Sie eventuell notwendige Änderungen in Ihrer Umgebung vor. Führen Sie dann die Tests nochmals aus, oder starten Sie gegebenenfalls den Installations-Assistenten erneut.
   
    ![Installation Rules][install_hpc3]
7. Vergewissern Sie sich, dass auf der Seite **HPC DB Configuration** (HPC-DB-Konfiguration) die Option **Hauptknoten** für alle HPC-Datenbanken ausgewählt ist, und klicken Sie dann auf **Weiter**.
   
    ![DB-Konfiguration][install_hpc4]
8. Akzeptieren Sie auf den verbleibenden Seiten des Assistenten die Standardeinstellungen. Klicken Sie auf der Seite **Install Required Components** (Erforderliche Komponenten installieren) auf **Installieren**.
   
    ![Installieren][install_hpc6]
9. Deaktivieren Sie nach Abschluss der Installation **Start HPC Cluster Manager** (HPC-Cluster-Manager starten), und klicken dann auf **Fertig stellen**. (Sie starten HPC Cluster Manager in einem späteren Schritt.)
   
    ![Finish][install_hpc7]

## <a name="prepare-the-azure-subscription"></a>Erstellen des Azure-Abonnements
Führen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) die nachfolgend beschriebenen Schritte für Ihr Azure-Abonnement aus. Diese sind erforderlich, damit Sie später Azure-Knoten von dem lokalen Hauptknoten aus bereitstellen können. Die nächsten Abschnitte enthalten ausführliche Verfahren.

* Laden Sie ein Verwaltungszertifikat hoch (wird für sichere Verbindungen zwischen dem Hauptknoten und den Azure-Diensten benötigt)
* Erstellen Sie einen Azure-Clouddienst, in dem Azure-Knoten (Workerrollen-Instanzen) laufen werden
* Erstellen eines Azure-Speicherkontos
  
  > [!NOTE]
  > Notieren Sie auch Ihre Azure-Abonnement-ID, die Sie später noch brauchen werden. Diese finden Sie im klassischen Portal, indem Sie auf **Einstellungen** > **Abonnements** klicken.
  > 
  > 

### <a name="upload-the-default-management-certificate"></a>Hochladen des Standard-Verwaltungszertifikats
HPC Pack installiert im Hauptknoten ein selbstsigniertes Zertifikat mit dem Namen Default Microsoft HPC Azure Management, das Sie als ein Azure-Verwaltungszertifikat hochladen können. Dieses Zertifikat wird für Testzwecke und Machbarkeitsstudien bereitgestellt.

1. Melden Sie sich vom Hauptknotencomputer aus beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Klicken Sie auf **Einstellungen** > **Verwaltungszertifikate**.
3. Klicken Sie in der Befehlsleiste auf **Upload**.
   
    ![Zertifikateinstellungen][upload_cert1]
4. Suchen Sie im Hauptknoten nach der Datei C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer. Klicken Sie dann auf die Schaltfläche mit dem **Häkchen** .
   
    ![Hochladen des Zertifikats][install_hpc10]

In der Liste der Verwaltungszertifikate sehen Sie **Default HPC Azure Management** .

### <a name="create-an-azure-cloud-service"></a>Erstellen eines Azure-Clouddiensts
> [!NOTE]
> Erstellen Sie den Clouddienst und das Speicherkonto (in einem späteren Schritt) in derselben geografischen Region, um eine maximale Leistung zu erzielen.
> 
> 

1. Klicken Sie im klassischen Portal auf der Befehlsleiste auf **Neu**.
2. Klicken Sie auf **Compute** > **Clouddienst** > **Schnellerfassung**.
3. Geben Sie eine URL für den Clouddienst ein, und klicken Sie dann auf **Clouddienst erstellen**.
   
    ![Erstellen des Dienstes][createservice1]

### <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos
1. Klicken Sie im klassischen Portal auf der Befehlsleiste auf **Neu**.
2. Klicken Sie auf **Data Services** > **Storage** > **Schnellerfassung**.
3. Geben Sie eine URL für das Konto ein, und klicken Sie dann auf **Speicherkonto erstellen**.
   
    ![Erstellen des Speicherkontos][createstorage1]

## <a name="configure-the-head-node"></a>Konfigurieren des Hauptknotens
Bevor Sie mit HPC Cluster Manager Azure-Knoten bereitstellen und Aufträge übermitteln können, müssen noch einige Schritte zur Konfiguration des Clusters ausgeführt werden.

1. Starten Sie HPC Cluster Manager auf dem Hauptknoten. Wenn das Dialogfeld **Select Head Node** (Hauptknoten auswählen) angezeigt wird, klicken Sie auf **Local Computer** (Lokaler Computer). Die **Deployment To-do List** wird angezeigt.
2. Klicken Sie unter **Required deployment tasks** (Erforderliche Bereitstellungsaufgaben) auf **Configure your network** (Netzwerk konfigurieren).
   
    ![Netzwerkkonfiguration][config_hpc2]
3. Wählen Sie im Network Configuration Wizard **All nodes only on an enterprise network** (Topology 5) aus.
   
    ![Topologie 5][config_hpc3]
   
   > [!NOTE]
   > Dies ist die einfachste Konfiguration für Demonstrationszwecke, da der Hauptknoten für die Verbindung zu Active Directory und zum Internet nur einen einzigen Netzwerkadapter braucht. Clusterkonfigurationen, die mehrere Netzwerke benötigen, werden in diesem Tutorial nicht behandelt.
   > 
   > 
4. Klicken Sie auf **Next** , und akzeptieren Sie auf den verbleibenden Seiten des Assistenten die Standardwerte. Klicken Sie auf der Registerkarte **Review** (Überprüfen) auf **Konfigurieren**, um die Netzwerkkonfiguration abzuschließen.
5. Klicken Sie in der **Deployment To-do List**auf **Provide installation credentials**herunterladen und installieren.
6. Geben Sie in das Dialogfeld **Installation Credentials** die Anmeldedaten des Domänenkontos ein, das Sie für die Installation von HPC Pack verwendet haben. Klicken Sie dann auf **OK**.
   
    ![Installation Credentials][config_hpc6]
   
   > [!NOTE]
   > HPC Pack-Dienste verwenden Installationsanmeldeinformationen nur für die Bereitstellung mit der Domäne verknüpfter Compute-Knoten. Die Azure-Knoten, die Sie in diesem Tutorial hinzufügen, sind nicht mit der Domäne verknüpft.
   > 
   > 
7. Klicken Sie in der **Deployment To-do List**auf **Configure the naming of new nodes**herunterladen und installieren.
8. Akzeptieren Sie im Dialogfeld **Specify Node Naming Series** (Knotenbenennungsreihe angeben) die Standard-Knotenbenennungsreihe, und klicken Sie auf **OK**.
   
    ![Knotenbenennung][config_hpc8]
   
   > [!NOTE]
   > Die Benennungsreihe erzeugt nur Namen für Serverknoten, die in die Domäne eingebunden sind. Azure-Workerknoten werden automatisch benannt.
   > 
   > 
9. Klicken Sie in der **Deployment To-do List**auf **Create a node template**herunterladen und installieren. Sie brauchen die Knotenvorlage, um Azure-Knoten zum Cluster hinzuzufügen.
10. Führen Sie im Create Node Template Wizard, folgende Schritte aus:
    
    a. Klicken Sie auf der Seite **Choose Node Template Type** (Knotenvorlagentyp auswählen) auf **Azure Node Template** (Azure-Knotenvorlage) und dann auf **Weiter**.
    
    ![Knotenvorlage][config_hpc10]
    
    b. Klicken Sie auf **Next** , um den Standardvorlagennamen zu akzeptieren.
    
    c. Geben Sie auf der Seite **** zur Angabe von Abonnementinformationen Ihre Azure-Abonnement-ID (zu finden in den Azure-Kontoinformationen) ein. Klicken Sie dann neben **Verwaltungszertifikat** auf **Durchsuchen**, und wählen Sie **Default HPC Azure Management** (HPC Azure-Standardverwaltung). Klicken Sie auf **Weiter**.
    
    ![Knotenvorlage][config_hpc12]
    
    d. Wählen Sie auf der Seite **Provide Service Information** (Dienstinformationen angeben) den Clouddienst und das Speicherkonto aus, den bzw. das Sie in einem vorherigen Schritt erstellt haben. Klicken Sie auf **Weiter**.
    
    ![Knotenvorlage][config_hpc13]
    
    e. Klicken Sie auf **Next** , und akzeptieren Sie auf den verbleibenden Seiten des Assistenten die Standardwerte. Klicken Sie dann auf der Registerkarte **Review** (Überprüfen) auf **Erstellen**, um die Knotenvorlage zu erstellen.
    
    > [!NOTE]
    > Standardmäßig enthält die Azure-Knotenvorlage unter anderem Einstellungen, mit denen Sie Knoten unter Verwendung von HPC Cluster Manager manuell starten (bereitstellen) und anhalten können. Sie können wahlweise auch einen Zeitplan für das automatische Starten und Stoppen der Azure-Knoten konfigurieren.
    > 
    > 

## <a name="add-azure-nodes-to-the-cluster"></a>Hinzufügen von Azure-Knoten zum Cluster
Sie werden nun mit der Knotenvorlage Azure-Knoten zu dem Cluster hinzufügen. Wenn Knoten zu dem Cluster hinzugefügt werden, werden deren Konfigurationsinformationen gespeichert, so dass Sie sie jederzeit als Rolleninstanzen im Clouddienst starten (bereitstellen) können. Azure-Knoten werden Ihrem Abonnement erst belastet, wenn die Rolleninstanzen im Clouddienst ausgeführt werden.

In diesem Tutorial werden Sie zwei kleine Knoten hinzufügen.

1. Klicken Sie in HPC-Cluster-Manager unter **Node Management** (Knotenverwaltung; in früheren Versionen von HPC Pack auch als **Ressourcenverwaltung** bezeichnet) im Bereich **Actions** (Aktionen) auf **Add Node** (Knoten hinzufügen).
   
    ![Add Node][add_node1]
2. Klicken Sie im Assistenten zum Hinzufügen von Knoten auf der Seite **Select Deployment Method** (Bereitstellungsmethode auswählen) auf **Add Windows Azure nodes** (Windows Azure-Knoten hinzufügen) und dann auf **Weiter**.
   
    ![Hinzufügen eines Azure-Knotens][add_node1_1]
3. Wählen Sie auf der Seite **Specify New Nodes** (Neue Knoten angeben) die Azure-Knotenvorlage aus, die Sie zuvor angelegt haben (Standardname **Default AzureNode Template** (AzureNode-Standardvorlage)). Geben Sie dann **2** Knoten der Größe **Small** an, und klicken Sie anschließend auf **Weiter**.
   
    ![Spezifikation von Knoten][add_node2]
   
    Details zu den verfügbaren Größen finden Sie unter [Größen für Clouddienste](cloud-services-sizes-specs.md).
4. Klicken Sie auf der Seite **Completing the Add Node Wizard** (Abschließen des Assistenten zum Hinzufügen von Knoten) auf **Fertig stellen**.
   
     Daraufhin werden zwei Azure-Knoten mit der Bezeichnung **AzureCN-0001** und **AzureCN-0002** in HPC-Cluster-Manager angezeigt. In beiden Fällen lautet der Status **Not-Deployed** (Nicht bereitgestellt).
   
    ![Hinzugefügte Knoten][add_node3]

## <a name="start-the-azure-nodes"></a>Starten der Azure-Knoten
Wenn Sie die Clusterressourcen in Azure nutzen wollen, veranlassen Sie den Start (die Bereitstellung) der Azure-Knoten mit HPC Cluster Manager und schalten sie online.

1. Klicken Sie in HPC-Cluster-Manager unter **Node Management** (Knotenverwaltung; in früheren Versionen von HPC Pack auch als **Ressourcenverwaltung** bezeichnet) auf einen oder beide Knoten, und klicken Sie dann im Bereich **Actions** (Aktionen) auf **Start** (Starten).
   
   ![Starten von Knoten][add_node4]
2. Klicken Sie im Dialogfeld **Stop Windows Azure Nodes** (Windows Azure-Knoten beenden) auf **Start** (Starten).
   
    ![Starten von Knoten][add_node5]
   
    Die Knoten wechseln in den Status **Provisioning** . Zeigen Sie das Bereitstellungsprotokoll an, um den Fortschritt der Bereitstellung zu verfolgen.
   
    ![Bereitstellung von Knoten][add_node6]
3. Nach einigen Minuten ist die Bereitstellung der Azure-Knoten beendet, und die Knoten befinden sich im Status **Offline** . In diesem Status werden die Rolleninstanzen zwar akzeptieren, aber Clusteraufträge noch nicht angenommen.
4. Sie können wie folgt prüfen, ob die Rolleninstanzen ausgeführt werden: Klicken Sie im [klassischen Portal](https://manage.windowsazure.com) auf **Cloud Services** > *Name_Ihres_Clouddiensts* > **Instanzen**.
   
    ![Ausführen von Instanzen][view_instances1]
   
    Sie sehen, dass in dem Dienst zwei Workerrollen-Instanzen ausgeführt werden. HPC Pack stellt außerdem automatisch zwei **HpcProxy** -Instanzen (mittlere Größe) bereit, die für die Kommunikation zwischen dem Hauptknoten und Azure zuständig sind.
5. Um nun die Azure-Knoten online zu schalten und Clusteraufträge ausführen zu können, wählen Sie die Knoten aus, klicken mit der rechten Maustaste und klicken dann auf **Bring online**.
   
    ![Offline geschaltete Knoten][add_node7]
   
    HPC Cluster Manager zeigt an, dass die Knoten **Online** sind.

## <a name="run-a-command-across-the-cluster"></a>Clusterweites Ausführen eines Befehls
Um die Installation zu überprüfen, führen Sie mithilfe des HPC Pack-Befehls **clusrun** einen Befehl oder eine Anwendung auf einem oder mehreren Clusterknoten aus. Rufen Sie hier in einem einfachen Beispiel mit **clusrun** die IP-Konfiguration der Azure-Knoten ab.

1. Öffnen Sie auf dem Hauptnoten eine Eingabeaufforderung.
2. Geben Sie folgenden Befehl ein:
   
    `clusrun /nodes:azurecn* ipconfig`
3. Eine Ausgabe ähnlich der folgenden wird angezeigt.
   
    ![clusrun][clusrun1]

## <a name="run-a-test-job"></a>Ausführen eines Testauftrags
Übermitteln Sie nun einen Testauftrag, der auf dem Hybridcluster ausgeführt wird. Dieses Beispiel ist ein sehr einfacher Parameter-Sweep (eine Art intrinsische parallele Berechnung). In diesem Beispiel werden mithilfe des Befehls **set /a** Unteraufgaben ausgeführt, die eine ganze Zahl mit sich selbst addieren. Alle Knoten des Clusters wirken an der Ausführung der Unteraufgaben für Ganzzahlen von 1 bis 100 mit.

1. Klicken Sie in HPC-Cluster-Manager unter **Auftragsverwaltung** im Bereich **Actions** (Aktionen) auf **New Parametric Sweep Job** (Neuer Parameter-Sweep-Auftrag).
   
    ![Neuer Auftrag][test_job1]
2. Geben Sie im Dialogfeld **New Parametric Sweep Job** (Neuer Parameter-Sweep-Auftrag) neben **Befehlszeile** den Befehl `set /a *+*` ein (überschreiben Sie dabei den in der Befehlsleiste angezeigten Standardbefehl). Belassen Sie bei den übrigen Einstellungen die Standardwerte, und klicken Sie dann auf **Submit** , um den Auftrag zu übermitteln.
   
    ![Parameter-Sweep][param_sweep1]
3. Wenn der Auftrag ausgeführt worden ist, doppelklicken Sie auf dem Auftrag **My Sweep Task** .
4. Klicken Sie auf **View Tasks**und dann auf eine Unteraufgabe, so dass die berechnete Ausgabe dieser Unteraufgabe angezeigt wird.
   
    ![Ergebnisse einer Aufgabe][view_job361]
5. Wenn Sie wissen möchten, welcher Knoten die Berechnung für diese Unteraufgabe ausgeführt hat, klicken Sie auf **Allocated Nodes**. (Möglicherweise zeigt Ihr Cluster einen anderen Knotennamen an.)
   
    ![Ergebnisse einer Aufgabe][view_job362]

## <a name="stop-the-azure-nodes"></a>Anhalten der Azure-Knoten
Nachdem Sie den Cluster getestet haben, können Sie die Azure-Knoten stoppen, um unnötige Belastungen Ihres Kontos zu vermeiden. Dabei wird der Clouddienst angehalten und die Azure-Rolleninstanzen werden entfernt.

1. Wählen Sie in HPC-Cluster-Manager unter **Node Management** (Knotenverwaltung; in früheren Versionen von HPC Pack auch als **Ressourcenverwaltung** bezeichnet) beide Azure-Knoten aus. Klicken Sie dann im Bereich **Actions** (Aktionen) auf **Stop** (Beenden).
   
    ![Knoten anhalten][stop_node1]
2. Klicken Sie im Dialogfeld **Stop Windows Azure Nodes** (Windows Azure-Knoten beenden) auf **Stop** (Beenden).
   
    ![Knoten anhalten][stop_node2]
3. Die Knoten wechseln in den Status **Stopping** . Nach einigen Minuten zeigt HPC Cluster Manager an, dass die Knoten **Not-Deployed**sind.
   
    ![Nicht bereitgestellte Knoten][stop_node4]
4. Sie können wie folgt sicherstellen, dass die Rolleninstanzen nicht mehr in Azure ausgeführt werden: Klicken Sie im [klassischen Portal](https://manage.windowsazure.com) auf **Cloud Services** > *Name_Ihres_Clouddiensts* > **Instanzen**. In der Produktionsumgebung werden keine Instanzen mehr bereitgestellt.
   
    ![Keine Instanzen][view_instances2]
   
    Damit schließt dieses Tutorial.

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie die Dokumentation zu [HPC Pack](https://technet.microsoft.com/library/cc514029).
* Informationen zum Einrichten einer umfangreicheren HPC Pack-Hybridclusterbereitstellung finden Sie unter [Burst to Azure with Microsoft HPC Pack](http://go.microsoft.com/fwlink/p/?LinkID=200493)(Burst zu Azure mit Microsoft HPC Pack).
* Andere Methoden zum Erstellen eines HPC Pack-Clusters in Azure (einschließlich Azure Resource Manager-Vorlagen) sind unter [Optionen zum Erstellen und Verwalten eines Windows HPC-Clusters (High Performance Computing) in Azure mit Microsoft HPC Pack](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) beschrieben.
* Unter [Big Compute in Azure: Technische Ressourcen für Batch und HPC (High Performance Computing)](../batch/big-compute-resources.md) finden Sie weitere Informationen zu den verschiedenen Big Compute- und HPC-Cloudlösungen in Azure.

[Overview]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/hybrid_cluster_overview.png
[install_hpc1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc1.png
[install_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc2.png
[install_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc3.png
[install_hpc4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc4.png
[install_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc6.png
[install_hpc7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc7.png
[install_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc10.png
[upload_cert1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/upload_cert1.png
[createstorage1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/createstorage1.png
[createservice1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/createservice1.png
[config_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc2.png
[config_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc3.png
[config_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc6.png
[config_hpc8]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc8.png
[config_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc10.png
[config_hpc12]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc12.png
[config_hpc13]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc13.png
[add_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1.png
[add_node1_1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1_1.png
[add_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node2.png
[add_node3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node3.png
[add_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node4.png
[add_node5]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node5.png
[add_node6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node6.png
[add_node7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node7.png
[view_instances1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances1.png
[clusrun1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/clusrun1.png
[test_job1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/test_job1.png
[param_sweep1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/param_sweep1.png
[view_job361]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job361.png
[view_job362]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job362.png
[stop_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node1.png
[stop_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node2.png
[stop_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node4.png
[view_instances2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances2.png

