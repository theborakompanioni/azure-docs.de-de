---
title: "Ausführen von OpenFOAM mit HPC Pack auf virtuellen Linux-Computern | Microsoft Docs"
description: "Sie erfahren, wie Sie einen Microsoft HPC Pack-Cluster unter Azure bereitstellen und einen OpenFOAM-Auftrag auf mehreren Linux-Computeknoten in einem RDMA-Netzwerk ausführen."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: c0bb1637-bb19-48f1-adaa-491808d3441f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 07/22/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: ee34a7ebd48879448e126c1c9c46c751e477c406
ms.openlocfilehash: 63ea23ca3ec756723b97bcf5290ed0256ed171ea


---
# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Ausführen von OpenFOAM mit Microsoft HPC Pack auf einem Linux-RDMA-Cluster in Azure
In diesem Artikel wird eine Methode zum Ausführen von OpenFoam auf Azure-VMs beschrieben. Hier stellen Sie einen Microsoft HPC Pack-Cluster mit Linux-Serverknoten unter Azure bereit und führen einen [OpenFoam](http://openfoam.com/)-Auftrag mit Intel MPI aus. Sie können RDMA-fähige Azure-VMs für die Computeknoten verwenden, sodass die Computeknoten über das Azure RDMA-Netzwerk kommunizieren. Andere Optionen zum Ausführen von OpenFoam in Azure beinhalten im Marketplace verfügbare vollständig konfigurierte kommerzielle Images, z.B. [OpenFoam 2.3 on CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/) von UberCloud, und durch Ausführung von [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/). 

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (Open Field Operation and Manipulation) ist ein Open Source-Computational Fluid Dynamics (CFD)-Softwarepaket, das sowohl in kommerziellen als auch in akademischen Organisationen häufig in den Bereichen Maschinenbau und Wissenschaft eingesetzt wird. Es enthält Tools für das „Meshing“, insbesondere snappyHexMesh, eine parallelisierte Meshing-Anwendung für komplexe CAD-Geometrien, und für die Vor- und Nachbearbeitung. Fast alle Prozesse werden gleichzeitig ausgeführt, damit Benutzer in den Genuss aller Vorteile der verwendeten Computerhardware kommen.  

Microsoft HPC Pack verfügt über Funktionen zum Ausführen von umfangreichen HPC- und parallelen Anwendungen, z.B. MPI-Anwendungen, in Clustern mit virtuellen Microsoft Azure-Computern. HPC Pack unterstützt auch die Ausführung von Linux-HPC-Anwendungen auf virtuellen Computern für Linux-Computeknoten, die in einem HPC Pack-Cluster bereitgestellt werden. Eine Einführung in die Verwendung von Linux-Serverknoten mit HPC Pack finden Sie unter [Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) .

> [!NOTE]
> In diesem Artikel wird veranschaulicht, wie Sie eine Linux MPI-Workload mit HPC Pack ausführen. Es wird vorausgesetzt, dass Sie sich bereits etwas mit der Verwaltung von Linux-Systemen und der Ausführung von MPI-Workloads in Linux-Clustern auskennen. Wenn Sie andere Versionen von MPI und OpenFOAM verwenden, als die in diesem Artikel vorgestellten, müssen Sie möglicherweise einige Installations- und Konfigurationsschritte ändern. 
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
* **HPC Pack-Cluster mit RDMA-fähigen Linux-Computeknoten**: Stellen Sie einen HPC Pack-Cluster mit Linux-Serverknoten der Größe A8, A9, H16r oder H16rm entweder mit einer [Azure Resource Manager-Vorlage](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) oder einem [Azure PowerShell-Skript](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) bereit. Voraussetzungen und Schritte für beide Optionen finden Sie unter [Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) . Wenn Sie die Bereitstellungsoption per PowerShell-Skript wählen, hilft Ihnen die Beispielkonfigurationsdatei weiter, die im Rahmen der Beispieldateien am Ende dieses Artikels aufgeführt ist. Verwenden Sie diese Konfiguration zum Bereitstellen eines Azure-basierten HPC Pack-Clusters, der aus einem Hauptknoten der Größe A8 mit Windows Server 2012 R2 und zwei Computeknoten der Größe A8 mit SUSE Linux Enterprise Server 12 besteht. Ersetzen Sie die Werte durch die entsprechenden Werte für Ihr Abonnement und Ihre Dienstnamen. 
  
  **Weitere wichtige Informationen**
  
  * Informationen zu den Voraussetzungen für Linux RDMA-Netzwerke in Azure finden Sie unter [Informationen zu den rechenintensiven A8-, A9-, A10- und A11-Instanzen](virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  * Bei Verwendung der Bereitstellungsoption mit PowerShell-Skript stellen Sie alle Linux-Computeknoten unter einem Clouddienst bereit, um die RDMA-Netzwerkverbindung zu verwenden.
  * Stellen Sie nach der Bereitstellung der Linux-Knoten eine Verbindung per SSH her, um weitere Verwaltungsaufgaben durchzuführen. Suchen Sie nach den SSH-Verbindungsdetails für jede Linux-VM im Azure-Portal.  
* **Intel MPI**: Zum Ausführen von OpenFOAM auf SLES 12 HPC-Serverknoten in Azure müssen Sie die Intel MPI Library 5-Laufzeit von der [Intel.com](https://software.intel.com/en-us/intel-mpi-library/)-Website installieren. (Intel MPI 5 ist auf CentOS-basierten HPC-Images vorinstalliert.)  In einem späteren Schritt installieren Sie Intel MPI bei Bedarf auf Ihren Linux-Computeknoten. Klicken Sie nach der Registrierung bei Intel zur Vorbereitung dieses Schritts in der Bestätigungs-E-Mail auf den Link zur entsprechenden Webseite. Kopieren Sie anschließend den Downloadlink für die TGZ-Datei für die entsprechende Version von Intel MPI. Dieser Artikel basiert auf Intel MPI-Version 5.0.3.048.
* **OpenFOAM Source Pack** : Laden Sie die OpenFOAM Source Pack-Software für Linux auf der [Website der OpenFOAM Foundation](http://openfoam.org/download/2-3-1-source/)herunter. Dieser Artikel basiert auf Source Pack-Version 2.3.1, die als „OpenFOAM-2.3.1.tgz“ zum Download verfügbar ist. Befolgen Sie die Anleitung weiter unten in diesem Artikel, um OpenFOAM auf den Linux-Computeknoten zu entpacken und zu kompilieren.
* **EnSight** (optional): Um die Ergebnisse Ihrer OpenFOAM-Simulation anzuzeigen, müssen Sie die [EnSight](https://www.ceisoftware.com/download/) -Visualisierung und das Analyseprogramm herunterladen und installieren. Die Informationen zu Lizenzierung und Download finden Sie auf der EnSight-Website.

## <a name="set-up-mutual-trust-between-compute-nodes"></a>Einrichten der gegenseitigen Vertrauensstellung zwischen Computeknoten
Für das Ausführen eines knotenübergreifenden Auftrags auf mehreren Linux-Knoten müssen die Knoten sich gegenseitig vertrauen (per **rsh** oder **ssh**). Wenn Sie den HPC Pack-Cluster mit dem IaaS-Bereitstellungsskript für Microsoft HPC Pack erstellen, richtet das Skript automatisch dauerhafte gegenseitige Vertrauensstellungen für das angegebene Administratorkonto ein. Für Benutzer ohne Administratorrechte, die Sie in der Domäne des Clusters erstellen, müssen Sie temporäre gegenseitige Vertrauensstellungen zwischen den Knoten einrichten, wenn diesen ein Auftrag zugewiesen wird, und diese Beziehung nach Abschluss des Auftrags wieder entfernen. Wenn Sie für alle Benutzer Vertrauensstellungen einrichten möchten, geben Sie ein RSA-Schlüsselpaar für den Cluster an, der von HPC Pack für die Vertrauensstellung verwendet wird.

### <a name="generate-an-rsa-key-pair"></a>Generieren eines RSA-Schlüsselpaars
Das Generieren eines RSA-Schlüsselpaars, das einen öffentlichen Schlüssel und einen privaten Schlüssel enthält, ist mit dem Linux-Befehl **ssh-keygen** sehr einfach.

1. Melden Sie sich auf einem Linux-Computer an.
2. Führen Sie den folgenden Befehl aus:
   
   ```
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > Drücken Sie die **EINGABETASTE**, um die Standardeinstellungen zu verwenden, bis der Befehl abgeschlossen ist. Geben Sie hier keine Passphrase ein. Wenn Sie zur Eingabe eines Kennworts aufgefordert werden, drücken Sie einfach die **EINGABETASTE**.
   > 
   > 
   
   ![Generieren eines RSA-Schlüsselpaars][keygen]
3. Wechseln Sie in das Verzeichnis "~/.ssh". Der private Schlüssel wird in "id_rsa" gespeichert und der öffentliche Schlüssel in "id_rsa.pub".
   
   ![Private und öffentliche Schlüssel][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Hinzufügen des Schlüsselpaars zum HPC Pack-Cluster
1. Stellen Sie mit dem HPC Pack-Administratorkonto (das Administratorkonto, das Sie mit dem Bereitstellungsskript eingerichtet haben) eine Remotedesktopverbindung mit dem Hauptknoten her.
2. Verwenden Sie Windows Server-Standardverfahren zum Erstellen eines Domänenbenutzerkontos in der Active Directory-Domäne des Clusters. Verwenden Sie z. B. das Tool Active Directory-Benutzer und -Computer auf dem Hauptknoten. In den Beispielen in diesem Artikel wird davon ausgegangen, dass Sie einen Domänenbenutzer mit dem Namen "hpclab\hpcuser" erstellen.
3. Erstellen Sie die Datei "C:\cred.xml", und kopieren Sie die RSA-Schlüsseldaten in diese Datei. Am Ende dieses Artikels finden Sie ein Beispiel für die Datei „cred.xml“.
   
   ```
   <ExtendedData>
     <PrivateKey>Copy the contents of private key here</PrivateKey>
     <PublicKey>Copy the contents of public key here</PublicKey>
   </ExtendedData>
   ```
4. Öffnen Sie eine Eingabeaufforderung, und geben Sie den folgenden Befehl ein, um die Anmeldeinformationen für das Konto „hpclab\hpcuser“ festzulegen. Verwenden Sie den **extendeddata**-Parameter, um den Namen der für die Schlüsseldaten erstellten Datei „C:\cred.xml“ zu übergeben.
   
   ```
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   Dieser Befehl wird ohne Ausgabe abgeschlossen. Speichern Sie die Datei "cred.xml" nach dem Festlegen der Anmeldeinformationen für die zum Ausführen der Aufträge erforderlichen Benutzerkonten an einem sicheren Ort, oder löschen Sie sie.
5. Wenn Sie das RSA-Schlüsselpaar auf einem der Linux-Knoten generiert haben, sollten Sie die Schlüssel nach der Verwendung löschen. HPC Pack richtet keine gegenseitige Vertrauensstellung ein, wenn die Datei „id_rsa“ oder „id_rsa.pub“ vorhanden ist.

> [!IMPORTANT]
> Es wird nicht empfohlen, Linux-Aufträge als Clusteradministrator auf einem freigegebenen Cluster auszuführen, da ein von einem Administrator übermittelter Auftrag auf den Linux-Knoten unter dem root-Konto ausgeführt wird. Ein Auftrag, der von einem Benutzer ohne Administratorrechte übermittelt wurde, wird aber unter einem lokalen Linux-Benutzerkonto ausgeführt, das den gleichen Namen wie der Auftragsbenutzer hat. HPC Pack richtet dann das gegenseitige Vertrauen für diesen Linux-Benutzer auf allen Knoten ein, die dem Auftrag zugewiesen sind. Der Linux-Benutzer kann vor der Ausführung des Auftrags manuell auf den Linux-Knoten eingerichtet werden, oder HPC Pack erstellt den Benutzer automatisch, wenn der Auftrag übermittelt wird. Wenn der Benutzer von HPC Pack erstellt wurde, löscht HPC Pack ihn nach Abschluss des Auftrags wieder. Zur Reduzierung von Sicherheitsbedrohungen entfernt HPC Pack die Schlüssel, nachdem der Auftrag abgeschlossen wurde.
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a>Einrichten einer Dateifreigabe für Linux-Knoten
Richten Sie nun eine SMB-Standardfreigabe in einem Ordner auf dem Hauptknoten ein. Stellen Sie den freigegebenen Ordner auf den Linux-Knoten bereit, damit die Linux-Knoten auf die Anwendungsdateien mit einem gemeinsamen Pfad zugreifen können. Wenn Sie möchten, können Sie eine andere Dateifreigabeoption verwenden, z. B. eine Azure Files-Freigabe (für viele Szenarien empfohlen) oder eine NFS-Freigabe. Dateifreigabeinformationen und die zugehörigen ausführlichen Schritte finden Sie unter [Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

1. Erstellen Sie auf dem Hauptknoten einen Ordner, und geben Sie ihn für alle Benutzer frei, indem Sie Lese-/Schreibberechtigungen festlegen. Beispielsweise können Sie „C:\OpenFOAM“ auf dem Hauptknoten als \\\\SUSE12RDMA-HN\OpenFOAM freigeben. Hierbei ist *SUSE12RDMA-HN* der Hostname des Hauptknotens.
2. Öffnen Sie ein Windows PowerShell-Fenster, und führen Sie die folgenden Befehle aus:
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
   clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
   ```

Der erste Befehl erstellt einen Ordner namens „/openfoam“ auf allen Knoten in der Gruppe „LinuxNodes“. Der zweite Befehl stellt den freigegebenen Ordner „//SUSE12RDMA-HN/OpenFOAM“ auf den Linux-Knoten bereit, wobei die Bits „dir_mode“ und „file_mode“ auf 777 festgelegt sind. Die Parameter *username* und *password* im Befehl sollten mit den Anmeldeinformationen eines Benutzers auf dem Hauptknoten übereinstimmen.

> [!NOTE]
> Das Symbol „\`“ im zweiten Befehl ist ein Escapesymbol für PowerShell. „\`,“ bedeutet, dass das „,“ (Komma) ein Teil des Befehls ist.
> 
> 

## <a name="install-mpi-and-openfoam"></a>Installieren von MPI und OpenFOAM
Zum Ausführen von OpenFOAM als MPI-Auftrag im RDMA-Netzwerk müssen Sie OpenFOAM mit den Intel MPI-Bibliotheken kompilieren. 

Führen Sie zuerst mehrere **clusrun** -Befehle aus, um Intel MPI-Bibliotheken (falls nicht bereits installiert) und OpenFOAM auf Ihren Linux-Knoten zu installieren. Verwenden Sie die bereits konfigurierte Hauptknotenfreigabe, um die Installationsdateien auf die Linux-Knoten zu verteilen.

> [!IMPORTANT]
> Diese Schritte der Installation und Kompilierung sind Beispiele. Sie benötigen Grundkenntnisse der Linux-Systemverwaltung, um sicherstellen zu können, dass abhängige Compiler und Bibliotheken richtig installiert sind. Unter Umständen müssen Sie bestimmte Umgebungsvariablen oder andere Einstellungen für Ihre Versionen von Intel MPI und OpenFOAM ändern. Weitere Informationen finden Sie unter [Intel MPI Library für Linux – Installationsleitfaden](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) und [OpenFOAM Source Pack-Installation](http://openfoam.org/download/2-3-1-source/) für Ihre Umgebung.
> 
> 

### <a name="install-intel-mpi"></a>Installieren von Intel MPI
Speichern Sie das heruntergeladene Installationspaket für Intel MPI (in diesem Beispiel „l_mpi_p_5.0.3.048.tgz“) unter „C:\OpenFoam“ auf dem Hauptknoten, damit die Linux-Knoten auf diese Datei über „/openfoam“ zugreifen können. Führen Sie anschließend **clusrun** aus, um die Intel MPI Library auf allen Linux-Knoten zu installieren.

1. Mit den folgenden Befehlen wird das Installationspaket kopiert und in „/opt/intel“ auf jedem Knoten extrahiert.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
   ```
2. Verwenden Sie für die unbeaufsichtigte Installation von Intel MPI Library die Datei „silent.cfg“. Ein Beispiel finden Sie in den Beispieldateien am Ende dieses Artikels. Legen Sie diese Datei im freigegebenen Ordner „/openfoam“ ab. Weitere Informationen zur Datei „silent.cfg“ finden Sie unter [Intel MPI Library für Linux – Installationsleitfaden (Unbeaufsichtigte Installation)](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).
   
   > [!TIP]
   > Stellen Sie sicher, dass Sie die Datei „silent.cfg“ als Textdatei mit Linux-Zeilenenden (nur LF, nicht CR-LF) speichern. Mit diesem Schritt wird sichergestellt, dass sie auf den Linux-Knoten richtig ausgeführt wird.
   > 
   > 
3. Installieren Sie die Intel MPI Library im unbeaufsichtigten Modus.
   
   ```
   clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
   ```

### <a name="configure-mpi"></a>Konfigurieren von MPI
Zu Testzwecken sollten Sie auf den Linux-Knoten in „/etc/security/limits.conf“ die folgenden Zeilen hinzufügen:

    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Starten Sie die Linux-Knoten neu, nachdem Sie die Datei „limits.conf“ aktualisiert haben. Verwenden Sie z.B. den folgenden **clusrun**-Befehl:

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Stellen Sie nach dem Neustart sicher, dass der freigegebene Ordner als „/openfoam“ bereitgestellt wurde.

### <a name="compile-and-install-openfoam"></a>Kompilieren und Installieren von OpenFOAM
Speichern Sie das heruntergeladene Installationspaket für das OpenFOAM Source Pack (in diesem Beispiel „OpenFOAM-2.3.1.tgz“) unter „C:\OpenFoam“ auf dem Hauptknoten, damit die Linux-Knoten auf diese Datei über „/openfoam“ zugreifen können. Führen Sie anschließend **clusrun** -Befehle aus, um OpenFOAM auf allen Linux-Knoten zu kompilieren.

1. Erstellen Sie den Ordner „/opt/OpenFOAM“ auf jedem Linux-Knoten, kopieren Sie das Quellpaket in diesen Ordner, und extrahieren Sie ihn dort.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
   ```
2. Legen Sie zum Kompilieren von OpenFOAM mit der Intel MPI Library zuerst einige Umgebungsvariablen für Intel MPI und OpenFOAM fest. Verwenden Sie ein Bash-Skript mit dem Namen „settings.sh“, um die Variablen festzulegen. Ein Beispiel finden Sie in den Beispieldateien am Ende dieses Artikels. Fügen Sie diese Datei (mit Linux-Zeilenenden gespeichert) in den Ordner „/openfoam“ ein. Diese Datei enthält auch die Einstellungen für die MPI- und OpenFOAM-Laufzeiten, die Sie später zum Ausführen eines OpenFOAM-Auftrags verwenden.
3. Installieren Sie die abhängigen Pakete, die zum Kompilieren von OpenFOAM benötigt werden. Je nach Linux-Distribution müssen Sie hierfür eventuell zunächst ein Repository hinzufügen. Führen Sie **clusrun** -Befehle ähnlich dem folgenden aus:
   
    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
   
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
   
    Greifen Sie ggf. per SSH-Verbindung auf die einzelnen Linux-Knoten zu, um die Befehle auszuführen und zu prüfen, ob die Ausführung funktioniert.
4. Führen Sie den folgenden Befehl aus, um OpenFOAM zu kompilieren. Es dauert eine gewisse Zeit, bis der Kompilierungsprozess abgeschlossen ist. Hierbei wird ein große Menge von Protokollinformationen für die Standardausgabe generiert, sodass Sie die Option **/interleaved** verwenden sollten, um die Ausgabe überlappend anzuzeigen.
   
   ```
   clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
   ```
   
   > [!NOTE]
   > Das Symbol „\`“ im Befehl ist ein Escapesymbol für PowerShell. „\`&“ bedeutet, dass „&“ ein Teil des Befehls ist.
   > 
   > 

## <a name="prepare-to-run-an-openfoam-job"></a>Vorbereiten der Ausführung eines OpenFOAM-Auftrags
Als Nächstes bereiten Sie sich auf das Ausführen eines MPI-Auftrags mit dem Namen „sloshingTank3D“ auf zwei Linux-Knoten vor. Dies ist eines der OpenFoam-Beispiele. 

### <a name="set-up-the-runtime-environment"></a>Einrichten der Laufzeitumgebung
Führen Sie den folgenden Befehl in einem Windows PowerShell-Fenster auf dem Hauptknoten aus, um die Laufzeitumgebungen für MPI und OpenFOAM auf den Linux-Knoten einzurichten. (Dieser Befehl gilt nur für SUSE Linux.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Vorbereiten der Beispieldaten
Verwenden Sie die bereits konfigurierte Hauptknotenfreigabe, um Dateien für die Linux-Knoten freizugeben (Bereitstellung als „/openfoam“).

1. Stellen Sie eine SSH-Verbindung mit einem der Linux-Computeknoten her.
2. Führen Sie den folgenden Befehl aus, um die OpenFOAM-Laufzeitumgebung einzurichten, falls Sie dies noch nicht getan haben.
   
   ```
   $ source /openfoam/settings.sh
   ```
3. Kopieren Sie das sloshingTank3D-Beispiel in den freigegebenen Ordner, und navigieren Sie dorthin.
   
   ```
   $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/
   
   $ cd /openfoam/sloshingTank3D
   ```
4. Wenn Sie die Standardparameter dieses Beispiels verwenden, kann die Ausführung zehn Minuten oder auch deutlich länger dauern. Es kann also ratsam sein, einige Parameter zu ändern, um die Ausführung zu beschleunigen. Eine einfache Option ist das Ändern der Zeitschrittvariablen „deltaT“ und „writeInterval“ in der Datei „system/controlDict“. Darin werden alle Eingabedaten gespeichert, die sich auf die Zeitkontrolle und das Lesen und Schreiben von Lösungsdaten beziehen. Beispielsweise können Sie den Wert für „deltaT“ von 0,05 in 0,5 und den Wert für „writeInterval“ von 0,05 in 0,5 ändern.
   
   ![Schrittvariablen ändern][step_variables]
5. Geben Sie die gewünschten Werte für die Variablen in der Datei „system/decomposeParDict“ an. In diesem Beispiel werden zwei Linux-Knoten mit jeweils acht Kernen verwendet. Legen Sie also „numberOfSubdomains“ auf 1 1 16 und „n“ von „hierarchicalCoeffs“ auf (16) fest, damit OpenFOAM parallel mit 16 Prozessen ausgeführt wird. Weitere Informationen finden Sie unter [OpenFOAM User Guide: 3.4 Running applications in parallel](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4) (OpenFOAM-Benutzerhandbuch: 3.4 Parallele Ausführung von Anwendungen).
   
   ![Prozesse zerlegen][decompose]
6. Führen Sie die folgenden Befehle aus dem Verzeichnis „sloshingTank3D“ aus, um die Beispieldaten vorzubereiten.
   
   ```
   $ . $WM_PROJECT_DIR/bin/tools/RunFunctions
   
   $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict
   
   $ runApplication blockMesh
   
   $ cp 0/alpha.water.org 0/alpha.water
   
   $ runApplication setFields  
   ```
7. Auf dem Hauptknoten sollten Sie verfolgen können, dass die Beispieldatendateien in „C:\OpenFoam\sloshingTank3D“ kopiert werden. („C:\OpenFoam“ ist der freigegebene Ordner auf dem Hauptknoten.)
   
   ![Datendateien auf dem Hauptknoten][data_files]

### <a name="host-file-for-mpirun"></a>Hostdatei für mpirun
In diesem Schritt erstellen Sie eine Hostdatei (eine Liste mit Computeknoten), die vom Befehl **mpirun** verwendet wird.

1. Erstellen Sie auf einem der Linux-Knoten eine Datei mit dem Namen „hostfile“ unter „/openfoam“, damit die Datei auf allen Linux-Knoten unter „/openfoam/hostfile“ erreichbar ist.
2. Schreiben Sie die Namen Ihrer Linux-Knoten in diese Datei. In diesem Beispiel enthält die Datei die folgenden Namen:
   
   ```       
   SUSE12RDMA-LN1
   SUSE12RDMA-LN2
   ```
   
   > [!TIP]
   > Sie können diese Datei auch unter „C:\OpenFoam\hostfile“ auf dem Hauptknoten erstellen. Speichern Sie die Datei bei Auswahl dieser Option als Textdatei mit Linux-Zeilenenden (nur LF, nicht CR-LF). Dadurch wird sichergestellt, dass es auf den Linux-Knoten ordnungsgemäß ausgeführt wird.
   > 
   > 
   
   **Bash-Skript-Wrapper**
   
   Wenn Sie über viele Linux-Knoten verfügen und Ihr Auftrag nur auf einigen ausgeführt werden soll, ist die Verwendung einer festen Hostdatei nicht zu empfehlen. Sie wissen dann nämlich nicht, welche Knoten dem Auftrag zugeordnet werden. Schreiben Sie in diesem Fall einen Bash-Skript-Wrapper für **mpirun**, damit die Hostdatei automatisch erstellt wird. Ein Beispiel für einen Bash-Skript-Wrapper mit dem Namen „hpcimpirun.sh“ finden Sie am Ende dieses Artikels. Speichern Sie ihn als „/openfoam/hpcimpirun.sh“. Dieses Beispielskript bewirkt Folgendes:
   
   1. Richtet die Umgebungsvariablen für **mpirun**und einige weitere Befehlsparameter ein, um den MPI-Auftrag über das RDMA-Netzwerk auszuführen. In diesem Fall werden die folgenden Variablen festgelegt:
      
      * I_MPI_FABRICS=shm:dapl
      * I_MPI_DAPL_PROVIDER=ofa-v2-ib0
      * I_MPI_DYNAMIC_CONNECTION=0
   2. Erstellt eine Hostdatei gemäß der Umgebungsvariable $CCP_NODES_CORES, die vom HPC-Hauptknoten festgelegt wird, wenn der Auftrag aktiviert wird.
      
      Für das Format von $CCP_NODES_CORES wird das folgende Muster verwendet:
      
      ```
      <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
      ```
      
      Hierbei gilt:
      
      * `<Number of nodes>` : Anzahl der Knoten, die diesem Auftrag zugeordnet sind.  
      * `<Name of node_n_...>` : Namen der einzelnen Knoten, die dem Auftrag zugeordnet sind.
      * `<Cores of node_n_...>`: Anzahl der Kerne auf dem Knoten, der dem Auftrag zugeordnet ist.
      
      Wenn der Auftrag für die Ausführung beispielsweise zwei Kerne benötigt, lautet „$CCP_NODES_CORES“ etwa wie folgt:
      
      ```
      2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
      ```
   3. Ruft den Befehl **mpirun** auf und fügt zwei Parameter an die Befehlszeile an.
      
      * `--hostfile <hostfilepath>: <hostfilepath>` : der Pfad der Hostdatei, die mit dem Skript erstellt wird.
      * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}` : eine Umgebungsvariable, die vom HPC Pack-Hauptknoten festgelegt wird, auf dem die Anzahl aller dem Auftrag zugeordneten Kerne gespeichert ist. In diesem Fall wird die Anzahl von Prozessen für **mpirun**angegeben.

## <a name="submit-an-openfoam-job"></a>Übermitteln eines OpenFOAM-Auftrags
Sie können einen Auftrag jetzt in HPC Cluster Manager übermitteln. Sie müssen das Skript „hpcimpirun.sh“ in den Befehlszeilen für einige Aufgaben des Auftrags übergeben.

1. Stellen Sie eine Verbindung mit dem Hauptknoten des Clusters her, und starten Sie HPC-Cluster-Manager.
2. Vergewissern Sie sich unter **Ressourcenverwaltung**, dass die Linux-Serverknoten den Status **Online** aufweisen. Ist dies nicht der Fall, wählen Sie sie aus, und klicken Sie auf **Online schalten**.
3. Klicken Sie in **Job Management** auf **Neuer Auftrag**.
4. Geben Sie einen Namen für den Auftrag ein, z.B. *sloshingTank3D*.
   
   ![Auftragsdetails][job_details]
5. Wählen Sie in den **Auftragsressourcen**den Ressourcentyp „Knoten“ aus, und legen Sie den Minimalwert auf „2“ fest. Bei dieser Konfiguration wird der Auftrag auf zwei Linux-Knoten ausgeführt, die in diesem Beispiel jeweils acht Kerne haben.
   
   ![Auftragsressourcen][job_resources]
6. Klicken Sie im linken Navigationsbereich auf **Aufgaben bearbeiten**, und klicken Sie dann zum Hinzufügen einer Aufgabe zum Auftrag auf **Hinzufügen**. Fügen Sie dem Auftrag mit den folgenden Befehlszeilen und Einstellungen vier Aufgaben hinzu.
   
   > [!NOTE]
   > Mit der Ausführung von `source /openfoam/settings.sh` werden die OpenFOAM- und MPI-Laufzeitumgebungen eingerichtet. Bei jeder folgenden Aufgabe wird also vor dem OpenFOAM-Befehl die Laufzeitumgebung aufgerufen.
   > 
   > 
   
   * **Task 1**. Ausführen von **decomposePar**, um Datendateien für die parallele Ausführung von **interDyMFoam** zu generieren.
     
     * Zuweisen eines Knotens zur Aufgabe
     * **Befehlszeile** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
     * **Arbeitsverzeichnis** : /openfoam/sloshingTank3D
     
     Dies wird in der folgenden Abbildung veranschaulicht. Die restlichen Aufgaben werden auf ähnliche Weise konfiguriert.
     
     ![Aufgabe 1 Details][task_details1]
   * **Task 2**. Paralleles Ausführen von **interDyMFoam**, um das Beispiel zu berechnen.
     
     * Zuweisen von zwei Knoten zur Aufgabe
     * **Befehlszeile** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`
     * **Arbeitsverzeichnis** : /openfoam/sloshingTank3D
   * **Task 3**. Führen Sie **reconstructPar** aus, um die Sätze mit den Zeitverzeichnissen für jedes processor_N_-Verzeichnis zu einem zentralen Satz zusammenzuführen.
     
     * Zuweisen eines Knotens zur Aufgabe
     * **Befehlszeile** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`
     * **Arbeitsverzeichnis** : /openfoam/sloshingTank3D
   * **Task 4**. Paralleles Ausführen von **foamToEnsight**, um die OpenFOAM-Ergebnisdateien in das EnSight-Format zu kopieren und die EnSight-Dateien in ein Verzeichnis mit dem Namen „Ensight“ im case-Verzeichnis abzulegen.
     
     * Zuweisen von zwei Knoten zur Aufgabe
     * **Befehlszeile** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`
     * **Arbeitsverzeichnis** : /openfoam/sloshingTank3D
7. Fügen Sie diesen Aufgaben in aufsteigender Aufgabenreihenfolge Abhängigkeiten hinzu.
   
   ![Abhängigkeiten von Aufgaben][task_dependencies]
8. Klicken Sie auf **Senden** , um diesen Auftrag auszuführen.
   
   Standardmäßig sendet HPC Pack den Auftrag unter dem Konto des aktuell angemeldeten Benutzers. Nach dem Klicken auf **Senden**wird unter Umständen ein Dialogfeld mit der Aufforderung angezeigt, den Benutzernamen und das Kennwort einzugeben.
   
   ![Anmeldeinformationen][creds]
   
   Unter bestimmten Umständen speichert HPC Pack von Ihnen zuvor eingegebene Benutzerinformationen, sodass dieses Dialogfeld nicht angezeigt wird. Damit HPC Pack es wieder anzeigt, geben Sie den folgenden Befehl an einer Eingabeaufforderung ein und senden dann den Auftrag.
   
   ```
   hpccred delcreds
   ```
9. Die Ausführung des Auftrags kann zwischen zehn Minuten und mehreren Stunden dauern. Dies richtet sich nach den Parametern, die Sie für das Beispiel festgelegt haben. In der Heat Map sehen Sie, dass der Auftrag auf den Linux-Knoten ausgeführt wird. 
   
   ![Heat Map][heat_map]
   
   Auf jedem Knoten wurden acht Prozesse gestartet.
   
   ![Linux-Prozesse][linux_processes]
10. Wenn der Auftrag abgeschlossen ist, finden Sie die Ergebnisse des Auftrags in den Ordnern unter „C:\OpenFoam\sloshingTank3D“ und die Protokolldateien unter „C:\OpenFoam“.

## <a name="view-results-in-ensight"></a>Anzeigen der Ergebnisse in EnSight
Verwenden Sie bei Bedarf [EnSight](https://www.ceisoftware.com/) , um die Ergebnisse des OpenFOAM-Auftrags zu visualisieren und zu analysieren. Weitere Informationen zur Visualisierung und zu Animationen in EnSight finden Sie in dieser [Videoanleitung](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html)(in englischer Sprache).

1. Nachdem Sie EnSight auf dem Knoten installiert haben, können Sie die Anwendung starten.
2. Öffnen Sie „C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case“.
   
   Es wird ein Tank angezeigt.
   
   ![Tank in EnSight][tank]
3. Erstellen Sie eine **Isosurface** aus **internalMesh**, und wählen Sie dann die Variable **alpha_water** aus.
   
   ![Isofläche erstellen][isosurface]
4. Legen Sie die Farbe für das Element **Isosurface_part** fest, das Sie im vorherigen Schritt erstellt haben. Legen Sie sie beispielsweise auf Wasserblau fest.
   
   ![Farbe einer Isofläche ändern][isosurface_color]
5. Erstellen Sie ein **Iso-volume** aus **walls**, indem Sie im Bereich **Parts** die Option **walls** auswählen und auf der Symbolleiste auf die Schaltfläche **Isosurfaces** klicken.
6. Wählen Sie im Dialogfeld unter **Type** die Option **Isovolume** aus, und legen Sie den Minimalwert für **Isovolume range** auf 0,5 fest. Klicken Sie auf **Create with selected parts**, um das isovolume-Element zu erstellen.
7. Legen Sie die Farbe für das Element **Iso_volume_part** fest, das Sie im vorherigen Schritt erstellt haben. Legen Sie sie beispielsweise auf ein dunkles Wasserblau fest.
8. Legen Sie die Farbe für **walls**fest. Legen Sie sie beispielsweise auf transparentes Weiß fest.
9. Klicken Sie nun auf **Play** , um die Ergebnisse der Simulation anzuzeigen.
   
    ![Ergebnis für Tank][tank_result]

## <a name="sample-files"></a>Beispieldateien
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>XML-Beispielkonfigurationsdatei für die Clusterbereitstellung mit einem PowerShell-Skript
 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

### <a name="sample-credxml-file"></a>Beispieldatei "cred.xml"
```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```
### <a name="sample-silentcfg-file-to-install-mpi"></a>Beispiel für die Datei „silent.cfg“ zum Installieren von MPI
```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Beispielskript „settings.sh“
```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


### <a name="sample-hpcimpirunsh-script"></a>Beispielskript „hpcimpirun.sh“
```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keys.png
[step_variables]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/step_variables.png
[data_files]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/data_files.png
[decompose]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/decompose.png
[job_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_details.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_resources.png
[task_details1]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_dependencies.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/creds.png
[heat_map]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/heat_map.png
[tank]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank.png
[tank_result]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank_result.png
[isosurface]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/linux_processes.png



<!--HONumber=Nov16_HO3-->


