---
title: "Hohe Verfügbarkeit von SAP HANA auf virtuellen Azure-Computern (VMs) | Microsoft-Dokumentation"
description: "Richten Sie die Hochverfügbarkeit von SAP HANA auf virtuellen Azure-Computern (VMs) ein."
services: virtual-machines-linux
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: sedusch
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 258ccee349e07448ebebaebe64cd6fb6888d7ed4
ms.contentlocale: de-de
ms.lasthandoff: 05/20/2017


---
# <a name="high-availability-of-sap-hana-on-azure-virtual-machines-vms"></a>Hochverfügbarkeit von SAP HANA auf virtuellen Azure-Computern (VMs)

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d
[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

Lokal können Sie entweder die HANA-Systemreplikation oder freigegebenen Speicher verwenden, um Hochverfügbarkeit für SAP HANA einzurichten.
Wir unterstützen derzeit nur die Einrichtung der HANA-Systemreplikation in Azure. Die SAP HANA-Replikation umfasst einen Masterknoten und mindestens einen untergeordneten Knoten. Änderungen an den Daten auf dem Masterknoten werden synchron oder asynchron an die untergeordneten Knoten repliziert.

Dieser Artikel beschreibt das Bereitstellen der virtuellen Computer, ihre Konfiguration, das Installieren des Clusterframeworks sowie das Installieren und Konfigurieren der SAP HANA-Systemreplikation.
In den Beispielkonfigurationen, Installationsbefehlen usw. werden Instanznummer 03 und HANA-System-ID HDB verwendet.

Lesen Sie zuerst die folgenden SAP Notes und Dokumente:

* SAP Note [2205917] Empfohlene BS-Einstellungen für SUSE Linux Enterprise Server für SAP-Anwendungen
* SAP Note [1944799] SAP HANA-Richtlinien für SUSE Linux Enterprise Server für SAP-Anwendungen
* [SAP HANA SR Performance Optimized Scenario][suse-hana-ha-guide] (SAP HANA-SR-Leistung – optimiertes Szenario)  
  Das Handbuch enthält alle erforderlichen Informationen zum lokalen Einrichten der SAP HANA-Replikation. Verwenden Sie dieses Handbuch als Grundlage.
## <a name="deploying-linux"></a>Bereitstellen von Linux

Der Ressourcen-Agent für SAP HANA ist im SUSE Linux Enterprise Server für SAP-Anwendungen enthalten.
Azure Marketplace enthält ein Image für SUSE Linux Enterprise Server für SAP-Anwendungen 12 mit BYOS (Bring Your Own Subscription), die Sie zum Bereitstellen neuer virtueller Computer verwenden können.

### <a name="manual-deployment"></a>Manuelle Bereitstellung

1. Erstellen einer Ressourcengruppe
1. Erstellen eines virtuellen Netzwerks
1. Erstellen Sie zwei Speicherkonten.
1. Erstellen Sie eine Verfügbarkeitsgruppe.  
   Richten Sie die maximale Updatedomäne ein.
1. Erstellen Sie einen Load Balancer (intern)  
   Wählen Sie das VNET des obigen Schritts aus.
1. Erstellen Sie den virtuellen Computer 1.  
   https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1  
   SLES für SAP-Anwendungen 12 SP1 (BYOS)  
   Wählen Sie Speicherkonto 1 aus.  
   Wählen Sie die Verfügbarkeitsgruppe aus.  
1. Erstellen Sie den virtuellen Computer 2.  
   https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1  
   SLES für SAP-Anwendungen 12 SP1 (BYOS)  
   Wählen Sie Speicherkonto 2 aus.   
   Wählen Sie die Verfügbarkeitsgruppe aus.  
1. Fügen Sie die Datenträger hinzu.
1. Konfigurieren Sie den Load Balancer.
    1. Erstellen eines Front-End-IP-Pools
        1. Öffnen Sie den Load Balancer, wählen Sie den Front-End-IP-Pool aus und klicken Sie auf „Hinzufügen“.
        1. Geben Sie den Namen des neuen Front-End-IP-Pools ein (z.B. „hana-frontend“).
       1. OK klicken
        1. Notieren Sie nach Erstellen des neuen Front-End-IP-Pools dessen IP-Adresse.
    1. Erstellen eines Back-End-Pools
        1. Öffnen Sie den Load Balancer, wählen Sie Back-End-Pools und klicken Sie auf „Hinzufügen“.
        1. Geben Sie den Namen des neuen Back-End-Pools ein (z.B. „hana-backend“).
        1. Klicken Sie auf „Virtuellen Computer hinzufügen“.
        1. Wählen Sie die Verfügbarkeitsgruppe aus, die Sie zuvor erstellt haben.
        1. Wählen Sie die virtuellen Computer des SAP HANA-Clusters aus.
        1. OK klicken
    1. Erstellen eines Integritätstests
       1. Öffnen Sie den Load Balancer, wählen Sie Integritätstests aus, und klicken Sie auf „Hinzufügen“.
        1. Geben Sie den Namen des neuen Integritätstests ein (z.B. „hana-hp“).
        1. Wählen Sie TCP als Protokoll, Port 625**03**, halten Sie „Intervall 5“ und „Fehlerschwellenwert 2“ bei.
        1. OK klicken
    1. Erstellen von Lastenausgleichsregeln
        1. Öffnen Sie den Load Balancer, wählen Sie das Laden von Lastenausgleichsregeln, und klicken Sie auf „Hinzufügen“.
        1. Geben Sie den Namen der neuen Lastenausgleichsregel ein (z.B. „hana-lb-3**03**15“).
        1. Wählen Sie die Front-End-IP-Adresse, den Back-End-Pool und den Integritätstest, die Sie zuvor erstellt haben (z.B. „hana-frontend“).
        1. Behalten Sie TCP als Protokoll bei, geben Sie Port 3**03**15 ein.
        1. Erhöhen Sie die Leerlaufzeitüberschreitung auf 30 Minuten.
       1. **Achten Sie darauf, dass Sie „Floating IP“ aktivieren.**
        1. OK klicken
        1. Wiederholen Sie die oben genannten Schritte für Port 3**03**17.

### <a name="deploy-with-template"></a>Bereitstellen mit Vorlage
Sie können eine der Schnellstartvorlagen auf Github verwenden, um alle erforderliche Ressourcen bereitzustellen. Die Vorlage stellt die virtuellen Computer, den Load Balancer, die Verfügbarkeitsgruppe etc. bereit. Führen Sie diese Schritte aus, um die Vorlage bereitzustellen:

1. Öffnen Sie die [Datenbankvorlage][template-multisid-db] oder [konvergierte Vorlage][template-converged] im Azure-Portal. Die Datenbankvorlage erstellt nur die Regeln des Lastenausgleichs für eine Datenbank, während die konvergierte Vorlage auch die Regeln des Lastenausgleichs für eine ASCS/SCS- und ERS-Instanz (nur Linux) erstellt. Wenn Sie ein SAP NetWeaver-basiertes System installieren und auch die ASCS/SCS-Instanz auf dem gleichen Computer installieren möchten, verwenden Sie die [konvergierte Vorlage][template-converged].
1. Legen Sie die folgenden Parameter fest:
    1. SAP-System-ID  
       Geben Sie die SAP-System-ID des SAP-Systems ein, das Sie installieren möchten. Die ID wird als Präfix für die Ressourcen verwendet, die bereitgestellt werden.
    1. Stapeltyp (gilt nur, wenn Sie die konvergierte Vorlage verwenden).  
       Wählen Sie den SAP NetWeaver-Stapeltyp aus.
    1. Betriebssystemtyp  
       Wählen Sie eine der Linux-Distributionen aus. Wählen Sie für dieses Beispiel die Option „SLES 12 BYOS“.
    1. DB-Typ  
       Wählen Sie „HANA“.
    1. SAP-Systemgröße  
       Die Anzahl der vom neuen System bereitgestellten SAPS. Wenn Sie nicht sicher sind, wie viele SAPS das System benötigt, wenden Sie sich an den SAP-Technologiepartner oder Systemintegrator.
    1. Systemverfügbarkeit  
       Wählen Sie „HA“ (hohe Verfügbarkeit).
    1. Administratorbenutzername und Administratorkennwort  
       Es wird ein neuer Benutzer erstellt, der sich am Computer anmelden kann.
    1. Neues oder vorhandenes Subnetz  
       Bestimmt, ob ein neues virtuelles Netzwerk und Subnetz erstellt oder ein bestehendes Subnetz verwendet werden soll. Wenn Sie bereits über ein virtuelles Netzwerk verfügen, das mit dem lokalen Netzwerk verbunden ist, wählen Sie hier „vorhanden“ aus.
    1. Subnetz-ID  
    Die ID des Subnetzes, mit dem die virtuellen Computer eine Verbindung herstellen sollen. Wählen Sie das Subnetz des virtuellen VPN- oder Express Route-Netzwerks aus, um den virtuellen Computer mit dem lokalen Netzwerk zu verbinden. Die ID sieht in der Regel wie folgt aus: /subscriptions/`<subscription id`>/resourceGroups/`<resource group name`>/providers/Microsoft.Network/virtualNetworks/`<virtual network name`>/subnets/`<subnet name`>

## <a name="setting-up-linux-ha"></a>Einrichten von Linux mit hoher Verfügbarkeit

Die folgenden Elemente sind mit einem der folgenden Präfixe versehen: [A] – gilt für alle Knoten, [1] – gilt nur für Knoten 1, oder [2] – gilt nur für Knoten 2.

1. [A] SLES nur für SAP BYOS – registrieren Sie SLES, um die Repositorys verwenden zu können.
1. [A] SLES nur für SAP BYOS – fügen Sie das Öffentliche-Cloud-Modul hinzu.
1. [A] Aktualisieren Sie SLES.
    ```bash
    sudo zypper update

    ```

1. [1] Aktivieren Sie den ssh-Zugriff.
    ```bash
    sudo ssh-keygen -tdsa
    
    # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy the public key
    sudo cat /root/.ssh/id_dsa.pub
    ```

2. [2] Aktivieren Sie den ssh-Zugriff.
    ```bash
    sudo ssh-keygen -tdsa

    # insert the public key you copied in the last step into the authorized keys file on the second server
    sudo vi /root/.ssh/authorized_keys
    
    # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy the public key    
    sudo cat /root/.ssh/id_dsa.pub
    ```

1. [1] Aktivieren Sie den ssh-Zugriff.
    ```bash
    # insert the public key you copied in the last step into the authorized keys file on the first server
    sudo vi /root/.ssh/authorized_keys
    
    ```

1. [A] Installieren Sie die HA-Erweiterung.
    ```bash
    sudo zypper install sle-ha-release fence-agents
    
    ```

1. [A] Richten Sie das Datenträgerlayout ein.
    1. LVM  
    Im Allgemeinen sollten Sie LVM für Volumes verwenden, die Daten- und Protokolldateien speichern. Im folgenden Beispiel wird davon ausgegangen, dass die virtuellen Computer über vier Datenträger verfügen, die zum Erstellen von zwei Volumes verwendet werden sollten.
        * Erstellen Sie physische Volumes für alle Datenträger, die Sie verwenden möchten.
    <pre><code>
    sudo pvcreate /dev/sdc
    sudo pvcreate /dev/sdd
    sudo pvcreate /dev/sde
    sudo pvcreate /dev/sdf
    </code></pre>
        * Erstellen Sie eine Volumegruppe für Datendateien, eine Volumegruppe für die Protokolldateien und eine für das freigegebene Verzeichnis von SAP HANA.
    <pre><code>
    sudo vgcreate vg_hana_data /dev/sdc /dev/sdd
    sudo vgcreate vg_hana_log /dev/sde
    sudo vgcreate vg_hana_shared /dev/sdf
    </code></pre>
        * Erstellen Sie die logischen Volumes.
    <pre><code>
    sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
    sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
    sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
    sudo mkfs.xfs /dev/vg_hana_data/hana_data
    sudo mkfs.xfs /dev/vg_hana_log/hana_log
    sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
    </code></pre>
        * Erstellen Sie die Bereitstellungsverzeichnisse, und kopieren Sie die UUID aller logischen Volumes.
    <pre><code>
    sudo mkdir -p /hana/data
    sudo mkdir -p /hana/log
    sudo mkdir -p /hana/shared
    # write down the id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
    sudo blkid
    </code></pre>
        * Erstellen Sie fstab-Einträge für die drei logischen Volumes.
    <pre><code>
    sudo vi /etc/fstab
    </code></pre>
    Fügen Sie diese Zeile in „/etc/fstab“ ein.
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b> /hana/data xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b> /hana/log xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b> /hana/shared xfs  defaults,nofail  0  2
    </code></pre>
        * Stellen Sie die neuen Volumes bereit.
    <pre><code>
    sudo mount -a
    </code></pre>
    1. Einfache Datenträger  
       Für kleine oder Demosysteme können Sie Ihre HANA-Daten- und Protokolldateien auf einem Datenträger platzieren. Die folgenden Befehle erstellen auf „/dev/sdc“ eine Partition und formatieren sie mit XFS.
    ```bash
    sudo fdisk /dev/sdc
    sudo mkfs.xfs /dev/sdc1
    
    # <a name="write-down-the-id-of-devsdc1"></a>Notieren Sie sich die ID von „/dev/sdc1“.
    sudo /sbin/blkid  sudo vi /etc/fstab
    ```

    Insert this line to /etc/fstab
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
    </code></pre>

    Create the target directory and mount the disk.

    ```bash
    sudo mkdir /hana
    sudo mount -a
    ```

1. [A] Richten Sie die Hostnamensauflösung für alle Hosts ein.  
    Sie können entweder einen DNS-Server verwenden oder „/etc/hosts“ auf allen Knoten ändern. In diesem Beispiel wird die Verwendung der /etc/hosts-Datei veranschaulicht.
   Ersetzen Sie die IP-Adresse und den Hostnamen in den folgenden Befehlen.
    ```bash
    sudo vi /etc/hosts
    ```
    Fügen Sie „/etc/hosts“ die folgenden Zeilen hinzu. Ändern Sie die IP-Adresse und den Hostnamen Ihrer Umgebung entsprechend.    
    
    <pre><code>
    <b>&lt;IP address of host 1&gt; &lt;hostname of host 1&gt;</b>
    <b>&lt;IP address of host 2&gt; &lt;hostname of host 2&gt;</b>
    </code></pre>

1. [1] Installieren Sie den Cluster.
    ```bash
    sudo ha-cluster-init
    
    # Do you want to continue anyway? [y/N] -> y
    # Network address to bind to (e.g.: 192.168.1.0) [10.79.227.0] -> ENTER
    # Multicast address (e.g.: 239.x.x.x) [239.174.218.125] -> ENTER
    # Multicast port [5405] -> ENTER
    # Do you wish to use SBD? [y/N] -> N
    # Do you wish to configure an administration IP? [y/N] -> N
    ```
        
1. [2] Fügen Sie dem Cluster Knoten hinzu
    ```bash
    sudo ha-cluster-join
        
    # WARNING: NTP is not configured to start at system boot.
    # WARNING: No watchdog device found. If SBD is used, the cluster will be unable to start without a watchdog.
    # Do you want to continue anyway? [y/N] -> y
    # IP address or hostname of existing node (e.g.: 192.168.1.1) [] -> IP address of node 1 e.g. 10.0.0.5
    # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
    ```

1. [A] Ändern Sie das Kennwort „hacluster“ in das gleiche Kennwort.
    ```bash
    sudo passwd hacluster
    
    ```

1. [A] Konfigurieren Sie „corosync“ zur Verwendung einer anderen Transportart, und fügen Sie „nodelist“ hinzu. Andernfalls funktioniert der Cluster nicht.
    ```bash
    sudo vi /etc/corosync/corosync.conf    
    
    ```

    Fügen Sie der Datei den im Folgenden fett formatierten Inhalt hinzu.
    
    <pre><code> 
    [...]
      interface { 
          [...] 
      }
      <b>transport:      udpu</b>
    } 
    <b>nodelist {
      node {
        ring0_addr:     < ip address of note 1 >
      }
      node {
        ring0_addr:     < ip address of note 2 > 
      } 
    }</b>
    logging {
      [...]
    </code></pre>

    Starten Sie den corosync-Dienst dann neu.

    ```bash
    sudo service corosync restart
    
    ```

1. [A] Installieren Sie HANA-HA-Pakete.  
    ```bash
    sudo zypper install SAPHanaSR
    
    ```

## <a name="installing-sap-hana"></a>Installieren von SAP HANA

Installieren Sie die SAP HANA-Systemreplikation gemäß Kapitel 4 des [SAP HANA SR Performance Optimized Scenario guide][suse-hana-ha-guide] (Handbuch zum Szenario der SAP HANA-Systemreplikation mit optimierter Leistung).

1. [A] Führen Sie „hdblcm“ von der HANA-DVD aus.
    * Wählen Sie die Installation -> 1.
    * Wählen Sie weitere Komponenten für die Installation aus -> 1.
    * Geben Sie den Installationspfad ein [/hana/shared]: -> EINGABETASTE.
    * Geben Sie den Namen des lokalen Hosts ein [..]: -> EINGABETASTE.
    * Möchten Sie dem System weitere Hosts hinzufügen? (j/n) [n]: -> EINGABETASTE.
    * Geben Sie die SAP HANA-System-ID ein: <SID of HANA e.g. HDB>.
    * Geben Sie die Instanznummer ein [00]:   
  HANA Instanznummer. Verwenden Sie 03, wenn Sie die Azure-Vorlage verwendet haben oder dem obigen Beispiel gefolgt sind.
    * Wählen Sie den Datenbankmodus / geben Sie den Index ein [1]: -> EINGABETASTE.
    * Wählen Sie die Systemnutzung / geben Sie den Index [ein 4]:  
  Wählen Sie die Systemnutzung.
    * Geben Sie den Speicherort der Datenvolumes ein [/hana/data/HDB]: -> EINGABETASTE.
    * Geben Sie den Speicherort der Protokollvolumes ein [/hana/log/HDB]: -> EINGABETASTE.
    * Möchten Sie die maximale Speicherbelegung beschränken? [n]: -> EINGABETASTE.
    * Geben Sie den Zertifikathostnamen für Host „...“ ein [...]: -> EINGABETASTE.
    * Geben Sie das Kennwort für den SAP-Host-Agent-Benutzer ein (sapadm):
    * Bestätigen Sie das Kennwort für den SAP-Host-Agent-Benutzer (sapadm):
    * Geben Sie das Kennwort für den Systemadministrator ein (hdbadm):
    * Bestätigen Sie das Kennwort für den Systemadministrator (hdbadm):
    * Geben Sie das Basisverzeichnis für den Systemadministrator ein [/usr/sap/HDB/home]: -> EINGABETASTE.
    * Geben Sie die Anmelde-Shell für den Systemadministrator ein [/ bin/sh]: -> EINGABETASTE.
    * Geben Sie die Benutzer-ID für den Systemadministrator ein [1001]: -> EINGABETASTE.
    * Geben Sie die ID der Benutzergruppe ein (sapsys) [79]: -> EINGABETASTE.
    * Geben Sie das Datenbankbenutzer-Kennwort ein (SYSTEM):
    * Bestätigen Sie das Datenbankbenutzer-Kennwort (SYSTEM):
    * Soll das System nach dem Neustart des Computers neu starten? [n]: -> EINGABETASTE.
    * Möchten Sie fortfahren? (j/n):  
  Überprüfen Sie die Zusammenfassung, und geben Sie „j“ zum Fortfahren ein.
1. [A] Aktualisieren Sie den SAP-Hostagent.  
  Laden Sie das aktuelle SAP-Hostagentarchiv vom [SAP Softwarecenter][sap-swcenter] herunter, und führen Sie den folgenden Befehl zum Aktualisieren des Agent aus. Ersetzen Sie den Pfad zum Archiv, um auf die Datei zu verweisen, die Sie heruntergeladen haben.
    ```bash
    sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <path to SAP Host Agent SAR>
    ```

1. [1] Erstellen Sie eine HANA-Replikation (als Stamm).  
    Führen Sie den folgenden Befehl aus: Stellen Sie sicher, dass Sie die fett formatierten Zeichenfolgen (HANA-System-ID „HDB“ und Instanzenanzahl „03“) durch die Werte Ihrer SAP HANA-Installation ersetzen.
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
    hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync' 
    hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
    </code></pre>

1. [A] Erstellen Sie einen Keystoreeintrag (als Stamm).
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
    </code></pre>
1. [1] Sichern Sie die Datenbank (als Stamm).
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
    </code></pre>
1. [1] Wechseln Sie zu dem sapsid-Benutzer (z.B. „hdbadm“), und erstellen Sie den primären Standort.
    <pre><code>
    su - <b>hdb</b>adm
    hdbnsutil -sr_enable –-name=<b>SITE1</b>
    </code></pre>
1. [2] Wechseln Sie zu dem sapsid-Benutzer (z.B. „hdbadm“), und erstellen Sie den sekundären Standort.
    <pre><code>
    su - <b>hdb</b>adm
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>saphanavm1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="configure-cluster-framework"></a>Konfigurieren des Clusterframeworks

Ändern der Standardeinstellungen

<pre>
sudo vi crm-defaults.txt
# enter the following to crm-saphana.txt
<code>
property $id="cib-bootstrap-options" \
  no-quorum-policy="ignore" \
  stonith-enabled="true" \
  stonith-action="reboot" \
  stonith-timeout="150s"
rsc_defaults $id="rsc-options" \
  resource-stickiness="1000" \
  migration-threshold="5000"
op_defaults $id="op-options" \
  timeout="600"
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a>jetzt laden wir die Datei in den Cluster
sudo crm configure load update crm-defaults.txt
</pre>

### <a name="create-stonith-device"></a>Erstellen des STONITH-Geräts

Das STONITH-Gerät verwendet einen Dienstprinzipal zur Autorisierung bei Microsoft Azure. Führen Sie die folgenden Schritte aus, um einen Dienstprinzipal zu erstellen.

1. Gehen Sie zu <https://portal.azure.com>.
1. Öffnen Sie das Blatt „Azure Active Directory“.  
   Wechseln Sie zu „Eigenschaften“, und notieren Sie sich die „Verzeichnis-ID“. Dies ist die **Mandanten-ID**.
1. Klicken Sie auf „App-Registrierungen“.
1. Klicken Sie auf "Hinzufügen".
1. Geben Sie einen Namen ein, wählen Sie den Anwendungstyp „Web-App/API“, geben Sie eine Anmelde-URL ein (z.B. „http://localhost“), und klicken Sie auf „Erstellen“.
1. Die Anmelde-URL wird nicht verwendet und kann eine beliebige gültige URL sein.
1. Wählen Sie die neue App aus, und klicken Sie auf der Registerkarte „Einstellungen“ auf „Schlüssel“.
1. Geben Sie eine Beschreibung für einen neuen Schlüssel ein, wählen Sie „Läuft nie ab“, und klicken Sie auf „Speichern“.
1. Notieren Sie sich den Wert. Er dient als **Kennwort** für den Dienstprinzipal.
1. Notieren Sie sich die Anwendungs-ID. Sie wird als Benutzername (**Anmelde-ID** in den folgenden Schritten) des Dienstprinzipals verwendet.

Der Dienstprinzipal hat standardmäßig keine Zugriffsberechtigungen für Ihre Azure-Ressourcen. Sie müssen dem Dienstprinzipal Berechtigungen zum Starten und Beenden (Freigeben) aller virtuellen Computer des Clusters gewähren.

1. Gehen Sie zu „https://portal.azure.com“.
1. Öffnen Sie das Blatt „Alle Ressourcen“.
1. Wählen Sie den virtuellen Computer aus.
1. Klicken Sie auf „Zugriffssteuerung (IAM)“.
1. Klicken Sie auf "Hinzufügen".
1. Wählen Sie die Rolle „Besitzer“.
1. Geben Sie den Namen der Anwendung ein, die Sie zuvor erstellt haben.
1. OK klicken

Nachdem Sie die Berechtigungen für die virtuellen Computer bearbeitet haben, können Sie die STONITH-Geräte im Cluster konfigurieren.

<pre>
sudo vi crm-fencing.txt
# enter the following to crm-fencing.txt
# replace the bold string with your subscription id, resource group, tenant id, service principal id and password
<code>
primitive rsc_st_azure_1 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

primitive rsc_st_azure_2 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a>jetzt laden wir die Datei in den Cluster
sudo crm configure load update crm-fencing.txt
</pre>

### <a name="create-sap-hana-resources"></a>Erstellen der SAP HANA-Ressourcen

<pre>
sudo vi crm-saphanatop.txt
# enter the following to crm-saphana.txt
# replace the bold string with your instance number and HANA system id
<code>
primitive rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHanaTopology \
    operations $id="rsc_sap2_<b>HDB</b>_HDB<b>03</b>-operations" \
    op monitor interval="10" timeout="600" \
    op start interval="0" timeout="600" \
    op stop interval="0" timeout="300" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>"

clone cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a>jetzt laden wir die Datei in den Cluster
sudo crm configure load update crm-saphanatop.txt
</pre>

<pre>
sudo vi crm-saphana.txt
# enter the following to crm-saphana.txt
# replace the bold string with your instance number, HANA system id and the frontend IP address of the Azure load balancer. 
<code>
primitive rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHana \
    operations $id="rsc_sap_<b>HDB</b>_HDB<b>03</b>-operations" \
    op start interval="0" timeout="3600" \
    op stop interval="0" timeout="3600" \
    op promote interval="0" timeout="3600" \
    op monitor interval="60" role="Master" timeout="700" \
    op monitor interval="61" role="Slave" timeout="700" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
    DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"

ms msl_SAPHana_<b>HDB</b>_HDB<b>03</b> rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
    target-role="Started" interleave="true"

primitive rsc_ip_<b>HDB</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \ 
    meta target-role="Started" is-managed="true" \ 
    operations $id="rsc_ip_<b>HDB</b>_HDB<b>03</b>-operations" \ 
    op monitor interval="10s" timeout="20s" \ 
    params ip="<b>10.0.0.21</b>" 
primitive rsc_nc_<b>HDB</b>_HDB<b>03</b> anything \ 
    params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \ 
    op monitor timeout=20s interval=10 depth=0 
group g_ip_<b>HDB</b>_HDB<b>03</b> rsc_ip_<b>HDB</b>_HDB<b>03</b> rsc_nc_<b>HDB</b>_HDB<b>03</b>
 
colocation col_saphana_ip_<b>HDB</b>_HDB<b>03</b> 2000: g_ip_<b>HDB</b>_HDB<b>03</b>:Started \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>:Master  
order ord_SAPHana_<b>HDB</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a>jetzt laden wir die Datei in den Cluster
sudo crm configure load update crm-saphana.txt
</pre>

### <a name="test-cluster-setup"></a>Testen der Clustereinrichtung
Im folgenden Kapitel wird beschrieben, wie Sie Ihre Einrichtung testen können. Jeder Test setzt voraus, dass Sie der Stammbenutzer sind, und der SAP HANA-Master auf dem virtuellen Computer „saphanavm1“ ausgeführt wird.

#### <a name="fencing-test"></a>Umgrenzungstest

Sie können die Einrichtung des Umgrenzungs-Agent testen, indem Sie die Netzwerkschnittstelle auf Knoten „saphanavm1“ deaktivieren.

<pre><code>
sudo ifdown eth0
</code></pre>

Der virtuelle Computer sollte jetzt abhängig von Ihrer Clusterkonfiguration neu gestartet oder beendet werden.
Wenn Sie die Stonith-Aktion auf „Aus“ festlegen, wird der virtuelle Computer beendet, und die Ressourcen werden zu dem ausgeführten virtuellen Computer migriert.

Sobald Sie den virtuellen Computer erneut starten, wird die SAP HANA-Ressource nicht als sekundär gestartet, wenn Sie AUTOMATED_REGISTER=„false“ festlegen. In diesem Fall müssen Sie die HANA-Instanz als sekundär konfigurieren, indem Sie den folgenden Befehl ausführen:

<pre><code>
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-manual-failover"></a>Testen eines manuellen Failovers

Sie können ein manuelles Failover durch Beenden des Schrittmacherdiensts auf Knoten „saphanavm1“ testen.
<pre><code>
service pacemaker stop
</code></pre>

Nach dem Failover können Sie den Dienst erneut starten. Die SAP HANA-Ressource auf „saphanavm1“ wird nicht als sekundär gestartet, wenn Sie AUTOMATED_REGISTER=„false“ festlegen. In diesem Fall müssen Sie die HANA-Instanz als sekundär konfigurieren, indem Sie den folgenden Befehl ausführen:

<pre><code>
service pacemaker start
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 


# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-migration"></a>Testen einer Migration

Sie können den SAP HANA-Masterknoten migrieren, indem Sie den folgenden Befehl ausführen.
<pre><code>
crm resource migrate msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
crm resource migrate g_ip_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
</code></pre>

Dies sollte den SAP HANA-Masterknoten und die Gruppe, die die virtuelle IP-Adresse „saphanavm2“ enthält, migrieren.
Die SAP HANA-Ressource auf „saphanavm1“ wird nicht als sekundär gestartet, wenn Sie AUTOMATED_REGISTER=„false“ festlegen. In diesem Fall müssen Sie die HANA-Instanz als sekundär konfigurieren, indem Sie den folgenden Befehl ausführen:

<pre><code>
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 
</code></pre>

Die Migration erstellt Speicherorteinschränkungen, die erneut gelöscht werden müssen.

<pre><code>
crm configure edited

# delete location contraints that are named like the following contraint. You should have two contraints, one for the SAP HANA resource and one for the IP address group.
location cli-prefer-g_ip_<b>HDB</b>_HDB<b>03</b> g_ip_<b>HDB</b>_HDB<b>03</b> role=Started inf: <b>saphanavm2</b>
</code></pre>

Darüber hinaus müssen Sie auch den Status der sekundären Knotenressource bereinigen.

<pre><code>
# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

## <a name="next-steps"></a>Nächste Schritte
* [SAP NetWeaver auf virtuellen Azure-Computern – Planungs- und Implementierungshandbuch][planning-guide]
* [Bereitstellung von Azure Virtual Machines für SAP][deployment-guide]
* [SAP NetWeaver auf virtuellen Azure-Computern – DBMS-Bereitstellungshandbuch][dbms-guide]
* Informationen zur Erzielung von hoher Verfügbarkeit und zur Planung der Notfallwiederherstellung für SAP HANA in Azure (große Instanzen) finden Sie unter [Hohe Verfügbarkeit und Notfallwiederherstellung für SAP HANA in Azure (große Instanzen)](hana-overview-high-availability-disaster-recovery.md). 

