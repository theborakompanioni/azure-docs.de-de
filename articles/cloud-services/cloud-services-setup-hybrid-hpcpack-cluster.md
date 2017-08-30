---
title: Einrichten eines HPC Pack-Hybridclusters in Azure | Microsoft-Dokumentation
description: "Erfahren Sie mehr über die Verwendung von Microsoft HPC Pack und Azure für die Einrichtung eines kleinen HPC-Hybridclusters (High Performance Computing)."
services: cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: hpc-pack
ms.assetid: 
ms.service: cloud-services
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: danlep
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: f6dc9657e64160be1e68a7356863b53131e9b3c3
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a>Einrichten eines HPC-Hybridclusters (High Performance Computing) mit Microsoft HPC Pack und bedarfsgesteuerten Azure-Computeknoten
Nutzen Sie Microsoft HPC Pack 2012 R2 und Azure für die Einrichtung eines kleinen HPC-Hybridclusters (High Performance Computing). Der in diesem Artikel gezeigte Cluster besteht aus einem lokalen HPC Pack-Hauptknoten und verschiedenen Computeknoten, die Sie bei Bedarf in einem Azure-Clouddienst bereitstellen. Dann können Sie im Hybrid-Cluster Rechenaufträge (Compute Jobs) ausführen.

![HPC-Hybridcluster][Overview] 

Dieses Tutorial veranschaulicht einen Ansatz zum Verwenden skalierbarer, bei Bedarf verfügbarer Computeressourcen in Azure zum Ausführen rechenintensiver Anwendungen.

In diesem Tutorial wird davon ausgegangen, dass Sie noch keine Erfahrung mit Serverclustern oder HPC Pack haben. Es soll Ihnen lediglich helfen, ein Hybrid-Rechencluster für Demonstrationszwecke schnell bereitzustellen. Welche Überlegungen und Schritte zur Bereitstellung eines größeren Hybridclusters mit HPC Pack in einer Produktionsumgebung anzustellen bzw. auszuführen sind oder wie Sie HPC Pack 2016 verwenden, ist in der [ausführlichen Anleitung](http://go.microsoft.com/fwlink/p/?LinkID=200493) beschrieben. Informationen zu weiteren Szenarios mit HPC Pack, z.B. zur automatischen Bereitstellung von Clustern auf virtuellen Azure-Computern, finden Sie unter [Optionen zum Erstellen und Verwalten eines Windows HPC-Clusters (High Performance Computing) in Azure mit Microsoft HPC Pack](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="prerequisites"></a>Voraussetzungen
* **Azure-Abonnement**: Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie in wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/free/) einrichten.
* **Ein lokaler Computer, auf dem Windows Server 2012 R2 oder Windows Server 2012 ausgeführt wird**: Dieser Computer fungiert als Hauptknoten des HPC-Clusters. Falls Sie noch nicht Windows Server verwenden, können Sie eine [Evaluierungsversion](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)herunterladen und installieren.
  
  * Der Computer muss in eine Active Directory-Domäne eingebunden werden. Zu Testzwecken können Sie den Hauptknotencomputer als Domänencontroller konfigurieren. Informationen zum Hinzufügen der Serverrolle „Active Directory Domain Services“ und Höherstufen des Hauptknotencomputers zum Domänencontroller finden Sie in der Windows Server-Dokumentation.
  * Damit das HPC Pack unterstützt werden kann, muss das Betriebssystem in einer der folgenden Sprachen installiert werden: Englisch, Japanisch oder Chinesisch (vereinfacht).
  * Stellen Sie sicher, dass wichtige und kritische Updates installiert wurden.
* **HPC Pack 2012 R2** - [Laden Sie hier das Installationspaket für die neueste Version kostenlos herunter](http://go.microsoft.com/fwlink/p/?linkid=328024), und kopieren Sie die Dateien auf den Hauptknotencomputer. Wählen Sie Installationsdateien in derselben Sprache aus wie für die Installation von Windows Server.

    >[!NOTE]
    > Wenn Sie HPC Pack 2016 anstelle von HPC Pack 2012 R2 verwenden möchten, sind zusätzliche Konfigurationsschritte erforderlich. Informationen finden Sie in der [ausführlichen Anleitung](http://go.microsoft.com/fwlink/p/?LinkID=200493).
    > 
* **Domänenkonto**: Dieses Konto muss für die HPC Pack-Installation mit lokalen Administratorberechtigungen auf dem Hauptknoten konfiguriert werden.
* **TCP-Konnektivität an Port 443** vom Hauptknoten zu Azure.

## <a name="install-hpc-pack-on-the-head-node"></a>Installieren des HPC Pack auf dem Hauptknoten
Zuerst muss das Microsoft HPC Pack auf Ihrem lokalen Computer mit Windows Server installiert werden. Dieser Computer wird der Hauptknoten des Clusters.

1. Melden Sie sich bei dem Hauptknoten mit einem Domänenkonto an, das über lokale Administratorrechte verfügt.

2. Starten Sie den Installations-Assistenten von HPC Pack, indem Sie die in den HPC Pack-Installationsdateien enthaltene Datei Setup.exe ausführen.

3. Klicken Sie im **HPC Pack 2012 R2 Setup**-Bildschirm auf **New installation or add new features to an existing installation** (Neue Installation, oder neue Features zu einer vorhandenen Installation hinzufügen).

    ![HPC Pack 2012 Setup][install_hpc1]

4. Klicken Sie auf der **Seite mit dem Vertrag für Benutzer von Microsoft-Software** auf **Weiter**.

5. Klicken Sie auf der Seite **Select Installation Type** (Installationstyp auswählen) auf **Create a new HPC cluster by creating a head node** (Neuen HPC-Cluster durch Erstellung eines Hauptknotens erstellen), und klicken Sie dann auf **Weiter**.

6. Der Installations-Assistent führt vor der Installation verschiedene Tests durch. Klicken Sie auf **Next** on the **Installation Rules** , wenn alle Tests erfolgreich durchgeführt worden sind. Lesen Sie andernfalls die bereitgestellten Informationen, und nehmen Sie eventuell notwendige Änderungen in Ihrer Umgebung vor. Führen Sie dann die Tests nochmals aus, oder starten Sie gegebenenfalls den Installations-Assistenten erneut.
7. Vergewissern Sie sich, dass auf der Seite **HPC DB Configuration** (HPC-DB-Konfiguration) die Option **Hauptknoten** für alle HPC-Datenbanken ausgewählt ist, und klicken Sie dann auf **Weiter**. 

    ![DB-Konfiguration][install_hpc4]

8. Akzeptieren Sie auf den verbleibenden Seiten des Assistenten die Standardeinstellungen. Klicken Sie auf der Seite **Install Required Components** (Erforderliche Komponenten installieren) auf **Installieren**.
   
    ![Installieren][install_hpc6]

9. Deaktivieren Sie nach Abschluss der Installation **Start HPC Cluster Manager** (HPC-Cluster-Manager starten), und klicken dann auf **Fertig stellen**. (Sie starten den HPC-Cluster-Manager in einem späteren Schritt.)
   
    ![Beenden][install_hpc7]

## <a name="prepare-the-azure-subscription"></a>Vorbereiten des Azure-Abonnements
Führen Sie im [Azure-Portal](https://portal.azure.com) für Ihr Azure-Abonnement die nachfolgenden Schritte aus. Nach Ausführen dieser Schritte können Sie Azure-Knoten vom lokalen Hauptknoten aus bereitstellen. 
  
  > [!NOTE]
  > Notieren Sie auch Ihre Azure-Abonnement-ID, die Sie später benötigen. Sie finden die ID im Portal unter **Abonnements**.
  > 

### <a name="upload-the-default-management-certificate"></a>Hochladen des Standardverwaltungszertifikats
HPC Pack installiert im Hauptknoten ein selbstsigniertes Zertifikat mit dem Namen Default Microsoft HPC Azure Management, das Sie als ein Azure-Verwaltungszertifikat hochladen können. Dieses Zertifikat dient bei Test- und Proof-of-Concept-Bereitstellungen zum Schutz der Verbindung zwischen dem Hauptknoten und Azure.

1. Melden Sie sich auf dem Hauptknotencomputer beim [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie auf **Abonnements** > *Name_Ihres_Abonnements*.

3. Klicken Sie dann auf **Verwaltungszertifikate** > **Hochladen**. 4. Suchen Sie auf dem Hauptknoten die Datei „C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer“. Klicken Sie dann auf **Hochladen**.

   
In der Liste der Verwaltungszertifikate finden Sie **Default HPC Azure Management** vor.

### <a name="create-an-azure-cloud-service"></a>Erstellen eines Azure-Clouddiensts
> [!NOTE]
> Erstellen Sie den Clouddienst und das Speicherkonto (in einem späteren Schritt) in derselben geografischen Region, um eine maximale Leistung zu erzielen.
> 
> 

1. Klicken Sie im Portal auf **Clouddienste (klassisch)** > **+Hinzufügen**.

2.  Geben Sie einen DNS-Namen für den Dienst ein. Wählen Sie eine Ressourcengruppe und einen Speicherort, und klicken Sie dann auf **Erstellen**.


### <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos
1. Klicken Sie im Portal auf **Speicherkonten (klassisch)** > **+Hinzufügen**.

2. Geben Sie einen Namen für das Konto ein, und wählen Sie das Bereitstellungsmodell **Klassisch** aus.

3. Wählen Sie eine Ressourcengruppe und einen Speicherort, und belassen Sie andere Einstellungen auf Standardwerten. Klicken Sie dann auf **Erstellen**.

## <a name="configure-the-head-node"></a>Konfigurieren des Hauptknotens
Bevor Sie mit HPC Cluster Manager Azure-Knoten bereitstellen und Aufträge übermitteln können, müssen noch einige Schritte zur Konfiguration des Clusters ausgeführt werden.

1. Starten Sie HPC Cluster Manager auf dem Hauptknoten. Wenn das Dialogfeld **Select Head Node** (Hauptknoten auswählen) angezeigt wird, klicken Sie auf **Local Computer** (Lokaler Computer). Die **Deployment To-do List** wird angezeigt.

2. Klicken Sie unter **Required deployment tasks** (Erforderliche Bereitstellungsaufgaben) auf **Configure your network** (Netzwerk konfigurieren).
   
    ![Netzwerkkonfiguration][config_hpc2]

3. Wählen Sie im Network Configuration Wizard **All nodes only on an enterprise network** (Topology 5) aus. Diese Netzwerkkonfiguration ist für Demonstrationszwecke die einfachste.
   
    ![Topologie 5][config_hpc3]
   
4. Klicken Sie auf **Next** , und akzeptieren Sie auf den verbleibenden Seiten des Assistenten die Standardwerte. Klicken Sie auf der Registerkarte **Review** (Überprüfen) auf **Konfigurieren**, um die Netzwerkkonfiguration abzuschließen.

5. Klicken Sie in der **Deployment To-do List**auf **Provide installation credentials**herunterladen und installieren.

6. Geben Sie in das Dialogfeld **Installation Credentials** die Anmeldedaten des Domänenkontos ein, das Sie für die Installation von HPC Pack verwendet haben. Klicken Sie dann auf **OK**. 
   
    ![Installation Credentials][config_hpc6]
   
7. Klicken Sie in der **Deployment To-do List**auf **Configure the naming of new nodes**herunterladen und installieren.

8. Akzeptieren Sie im Dialogfeld **Specify Node Naming Series** (Knotenbenennungsreihe angeben) die Standard-Knotenbenennungsreihe, und klicken Sie auf **OK**. Führen Sie diesen Schritt auch dann aus, wenn die Azure-Knoten, die Sie in diesem Tutorial hinzufügen, automatisch benannt werden.
   
    ![Knotenbenennung][config_hpc8]
   
9. Klicken Sie in der **Deployment To-do List** auf **Create a node template**. Später in diesem Tutorial fügen Sie dem Cluster mithilfe der Knotenvorlage Azure-Knoten hinzu.

10. Führen Sie im Create Node Template Wizard folgende Schritte aus:
    
    a. Klicken Sie auf der Seite **Choose Node Template Type** (Knotenvorlagentyp auswählen) auf **Windows Azure Node Template** (Microsoft Azure-Knotenvorlage) und dann auf **Next** (Weiter).
    
    ![Knotenvorlage][config_hpc10]
    
    b. Klicken Sie auf **Next** , um den Standardvorlagennamen zu akzeptieren.
    
    c. Geben Sie auf der Seite zur Angabe von **Abonnementinformationen** Ihre Azure-Abonnement-ID (zu finden in den Azure-Kontoinformationen) ein. Suchen Sie dann neben **Verwaltungszertifikat** nach **Default HPC Azure Management** (HPC Azure-Standardverwaltung). Klicken Sie auf **Weiter**.
    
    ![Knotenvorlage][config_hpc12]
    
    d. Wählen Sie auf der Seite **Provide Service Information** (Dienstinformationen angeben) den Clouddienst und das Speicherkonto aus, den bzw. das Sie in einem vorherigen Schritt erstellt haben. Klicken Sie auf **Weiter**.
    
    ![Knotenvorlage][config_hpc13]
    
    e. Klicken Sie auf **Next** , und akzeptieren Sie auf den verbleibenden Seiten des Assistenten die Standardwerte. Klicken Sie dann auf der Registerkarte **Review** (Überprüfen) auf **Erstellen**, um die Knotenvorlage zu erstellen.
    
    > [!NOTE]
    > Standardmäßig enthält die Azure-Knotenvorlage unter anderem Einstellungen, mit denen Sie Knoten unter Verwendung von HPC Cluster Manager manuell starten (bereitstellen) und anhalten können. Sie können wahlweise auch einen Zeitplan für das automatische Starten und Stoppen der Azure-Knoten konfigurieren.
    > 
    > 

## <a name="add-azure-nodes-to-the-cluster"></a>Hinzufügen von Azure-Knoten zum Cluster
Nun fügen Sie mit der Knotenvorlage dem Cluster Azure-Knoten hinzu. Wenn dem Cluster Knoten hinzugefügt werden, werden deren Konfigurationsinformationen gespeichert, sodass Sie sie jederzeit im Clouddienst starten (bereitstellen) können. Azure-Knoten werden Ihrem Abonnement erst in Rechnung gestellt, sobald die Instanzen im Clouddienst ausgeführt werden.

Führen Sie diese Schritte aus, um zwei Knoten des Typs „Small“ hinzuzufügen.

1. Klicken Sie im HPC-Cluster-Manager auf **Node Management** (Knotenverwaltung; in aktuellen Versionen von HPC Pack auch als **Ressourcenverwaltung** bezeichnet) und dann auf **Add Node** (Knoten hinzufügen).
   
    ![Add Node][add_node1]

2. Klicken Sie im Assistenten zum Hinzufügen von Knoten auf der Seite **Select Deployment Method** (Bereitstellungsmethode auswählen) auf **Add Windows Azure nodes** (Windows Azure-Knoten hinzufügen) und dann auf **Weiter**.
   
    ![Hinzufügen eines Azure-Knotens][add_node1_1]

3. Wählen Sie auf der Seite **Specify New Nodes** (Neue Knoten angeben) die Azure-Knotenvorlage aus, die Sie zuvor angelegt haben (Standardname **Default AzureNode Template** (AzureNode-Standardvorlage)). Geben Sie dann **2** Knoten der Größe **Small** an, und klicken Sie anschließend auf **Weiter**.
   
    ![Spezifikation von Knoten][add_node2]
   
4. Klicken Sie auf der Seite **Completing the Add Node Wizard** (Abschließen des Assistenten zum Hinzufügen von Knoten) auf **Fertig stellen**.
    
     Daraufhin werden zwei Azure-Knoten mit der Bezeichnung **AzureCN-0001** und **AzureCN-0002** in HPC-Cluster-Manager angezeigt. In beiden Fällen lautet der Status **Not-Deployed** (Nicht bereitgestellt).
   
    ![Hinzugefügte Knoten][add_node3]

## <a name="start-the-azure-nodes"></a>Starten der Azure-Knoten
Wenn Sie die Clusterressourcen in Azure nutzen wollen, veranlassen Sie den Start (die Bereitstellung) der Azure-Knoten mit HPC Cluster Manager und schalten sie online.

1. Klicken Sie im HPC-Cluster-Manager auf **Node Management** (Knotenverwaltung; in aktuellen Versionen von HPC Pack auch als **Ressourcenverwaltung** bezeichnet), und wählen Sie die Azure-Knoten aus.

2. Klicken Sie auf **Starten** und dann auf **OK**.
   
   ![Starten von Knoten][add_node4]
   
    Die Knoten wechseln in den Status **Provisioning** . Zeigen Sie das Bereitstellungsprotokoll an, um den Fortschritt der Bereitstellung zu verfolgen.
   
    ![Bereitstellung von Knoten][add_node6]

3. Nach einigen Minuten ist die Bereitstellung der Azure-Knoten beendet, und die Knoten befinden sich im Status **Offline** . In diesem Status werden die Rolleninstanzen zwar ausgeführt, aber Clusteraufträge noch nicht angenommen.

4. Sie können wie folgt prüfen, ob die Rolleninstanzen ausgeführt werden: Klicken Sie im Azure-Portal auf **Cloud Services** > *Name_Ihres_Clouddiensts*.
   
   Es sollten zwei Instanzen (Knoten) des Typs **HpcWorkerRole** angezeigt werden, die im Dienst ausgeführt werden. HPC Pack stellt außerdem automatisch zwei **HpcProxy** -Instanzen (mittlere Größe) bereit, die für die Kommunikation zwischen dem Hauptknoten und Azure zuständig sind.

   ![Ausführen von Instanzen][view_instances1]

5. Um nun die Azure-Knoten online zu schalten und Clusteraufträge ausführen zu können, wählen Sie die Knoten aus, klicken mit der rechten Maustaste und klicken dann auf **Bring online**.
   
    ![Offline geschaltete Knoten][add_node7]
   
    HPC Cluster Manager zeigt an, dass die Knoten **Online** sind.

## <a name="run-a-command-across-the-cluster"></a>Clusterweites Ausführen eines Befehls
Um die Installation zu überprüfen, führen Sie mithilfe des HPC Pack-Befehls **clusrun** einen Befehl oder eine Anwendung auf einem oder mehreren Clusterknoten aus. Rufen Sie hier in einem einfachen Beispiel mit **clusrun** die IP-Konfiguration der Azure-Knoten ab.

1. Öffnen Sie auf dem Hauptknoten eine Eingabeaufforderung als Administrator.

2. Geben Sie folgenden Befehl ein:
   
    `clusrun /nodes:azurecn* ipconfig`

3. Geben Sie bei Aufforderung Ihr Administratorkennwort für den Cluster ein. Eine Befehlsausgabe ähnlich der folgenden sollte angezeigt werden.
   
    ![clusrun][clusrun1]

## <a name="run-a-test-job"></a>Ausführen eines Testauftrags
Übermitteln Sie nun einen Testauftrag, der auf dem Hybridcluster ausgeführt wird. Dieses Beispiel ist ein einfacher Parameter-Sweep-Auftrag (eine Art intrinsische parallele Berechnung). In diesem Beispiel werden mithilfe des Befehls **set /a** Unteraufgaben ausgeführt, die eine ganze Zahl mit sich selbst addieren. Alle Knoten des Clusters wirken an der Ausführung der Unteraufgaben für Ganzzahlen von 1 bis 100 mit.

1. Klicken Sie im HPC-Cluster-Manager auf **Job Management (Auftragsverwaltung)** > **New Parametric Sweep Job** (Neuer Parameter-Sweep-Auftrag).
   
    ![Neuer Auftrag][test_job1]

2. Geben Sie im Dialogfeld **New Parametric Sweep Job** (Neuer Parameter-Sweep-Auftrag) neben **Befehlszeile** den Befehl `set /a *+*` ein (überschreiben Sie dabei den in der Befehlsleiste angezeigten Standardbefehl). Belassen Sie bei den übrigen Einstellungen die Standardwerte, und klicken Sie dann auf **Submit** , um den Auftrag zu übermitteln.
   
    ![Parameter-Sweep][param_sweep1]

3. Wenn der Auftrag ausgeführt worden ist, doppelklicken Sie auf dem Auftrag **My Sweep Task** .

4. Klicken Sie auf **View Tasks**und dann auf eine Unteraufgabe, so dass die berechnete Ausgabe dieser Unteraufgabe angezeigt wird.
   
    ![Ergebnisse einer Aufgabe][view_job361]

5. Wenn Sie wissen möchten, welcher Knoten die Berechnung für diese Unteraufgabe ausgeführt hat, klicken Sie auf **Allocated Nodes**. (Möglicherweise zeigt Ihr Cluster einen anderen Knotennamen an.)
   
    ![Ergebnisse einer Aufgabe][view_job362]

## <a name="stop-the-azure-nodes"></a>Anhalten der Azure-Knoten
Nachdem Sie den Cluster getestet haben, können Sie die Azure-Knoten stoppen, um unnötige Belastungen Ihres Kontos zu vermeiden. Bei diesem Schritt wird der Clouddienst angehalten, und die Azure-Rolleninstanzen werden entfernt.

1. Wählen Sie im HPC-Cluster-Manager unter **Node Management** (Knotenverwaltung; in früheren Versionen von HPC Pack als **Ressourcenverwaltung** bezeichnet) beide Azure-Knoten aus. Klicken Sie dann auf **Stop** (Beenden).
   
    ![Knoten anhalten][stop_node1]

2. Klicken Sie im Dialogfeld **Stop Windows Azure Nodes** (Windows Azure-Knoten beenden) auf **Stop** (Beenden). 
   
3. Die Knoten wechseln in den Status **Stopping** . Nach einigen Minuten zeigt HPC Cluster Manager an, dass die Knoten **Not-Deployed**sind.
   
    ![Nicht bereitgestellte Knoten][stop_node4]

4. Sie können wie folgt sicherstellen, dass die Rolleninstanzen nicht mehr in Azure ausgeführt werden: Klicken Sie im Azure-Portal auf **Clouddienste (klassisch)** > *Name_Ihres_Clouddiensts*. In der Produktionsumgebung sind keine Instanzen mehr bereitgestellt. 
   
    Damit schließt dieses Tutorial.

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie die Dokumentation zu [HPC Pack](https://technet.microsoft.com/library/cc514029).
* Informationen zum Einrichten einer umfangreicheren HPC Pack-Hybridclusterbereitstellung finden Sie unter [Burst to Azure with Microsoft HPC Pack](http://go.microsoft.com/fwlink/p/?LinkID=200493)(Burst zu Azure mit Microsoft HPC Pack).
* Andere Methoden zum Erstellen eines HPC Pack-Clusters in Azure (einschließlich Azure Resource Manager-Vorlagen) sind unter [Optionen zum Erstellen und Verwalten eines Windows HPC-Clusters (High Performance Computing) in Azure mit Microsoft HPC Pack](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) beschrieben.


[Overview]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/hybrid_cluster_overview.png
[install_hpc1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc1.png
[install_hpc4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc4.png
[install_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc6.png
[install_hpc7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc7.png
[install_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc10.png
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
[add_node6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node6.png
[add_node7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node7.png
[view_instances1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances1.png
[clusrun1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/clusrun1.png
[test_job1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/test_job1.png
[param_sweep1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/param_sweep1.png
[view_job361]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job361.png
[view_job362]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job362.png
[stop_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node1.png
[stop_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node4.png
[view_instances2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances2.png

