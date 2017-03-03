---
title: "Ausführen eines MariaDB (MySQL)-Clusters in Azure | Microsoft-Dokumentation"
description: Erstellen eines MariaDB- + Galera MySQL-Clusters auf virtuellen Azure-Computern
services: virtual-machines-linux
documentationcenter: 
author: sabbour
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: d0d21937-7aac-4222-8255-2fdc4f2ea65b
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/15/2015
ms.author: asabbour
translationtype: Human Translation
ms.sourcegitcommit: 3136b8345d0c851c29a9498089da73c8564549d1
ms.openlocfilehash: 72aed50d3dd86c5875d3b741a6fff16cb8f17a2d
ms.lasthandoff: 01/31/2017


---
# <a name="mariadb-mysql-cluster-azure-tutorial"></a>MariaDB (MySQL)-Cluster: Azure-Tutorial
> [!IMPORTANT]
> Azure bietet zwei Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Azure Resource Manager](../azure-resource-manager/resource-manager-deployment-model.md) und klassisch. Dieser Artikel gilt für das klassische Bereitstellungsmodell. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Azure Resource Manager-Modells.

> [!NOTE]
> Der MariaDB Enterprise-Cluster ist jetzt in Azure Marketplace verfügbar. Mit dem neuen Angebot wird automatisch ein MariaDB Galera-Cluster im Azure Resource Manager bereitgestellt. Verwenden Sie nach Möglichkeit das neue Angebot auf [Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/).
>
>

In diesem Artikel wird gezeigt, wie Sie einen Multimaster-[Galera](http://galeracluster.com/products/)-Cluster von [MariaDBs](https://mariadb.org/en/about/) (einen robusten, skalierbaren und zuverlässiger Ersatz für MySQL) für das Arbeiten in einer hoch verfügbaren Umgebung auf virtuellen Azure-Computern erstellen.

## <a name="architecture-overview"></a>Übersicht über die Architektur
In diesem Artikel wird beschrieben, wie Sie die folgenden Schritte ausführen:

- Erstellen eines Clusters mit drei Knoten
- Trennen der Datenträger vom Betriebssystem-Datenträger
- Erstellen der Datenträger mit der Einstellung „RAID&0; (Stripeset)“, um die IOPS zu erhöhen
- Verwenden von Azure Load Balancer, um die Last der drei Knoten auszugleichen
- Um sich wiederholende Aufgaben zu minimieren, erstellen Sie ein Image des virtuellen Computers mit MariaDB und Galera und verwenden dieses, um die anderen Cluster-VMs zu erstellen.

![Systemarchitektur](media/virtual-machines-linux-classic-mariadb-mysql-cluster/Setup.png)

> [!NOTE]
> In diesem Thema werden die [Azure CLI](../xplat-cli-install.md)-Tools verwendet. Denken Sie deshalb daran, sie herunterzuladen und gemäß den Anweisungen mit Ihrem Azure-Abonnement zu verbinden. Referenzinformationen zu den Befehlen der Azure-Befehlszeilenschnittstelle (Azure CLI) finden Sie unter [Befehlsreferenz zur Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). Darüber hinaus müssen Sie einen [SSH-Schlüssel für die Authentifizierung erstellen] und sich den Speicherort der PEM-Datei notieren.
>
>

## <a name="create-the-template"></a>Erstellen der Vorlage
### <a name="infrastructure"></a>Infrastruktur
1. Erstellen Sie eine Affinitätsgruppe zum Zusammenstellen der Ressourcen.

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"
2. Erstellen Sie ein virtuelles Netzwerk.

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet
3. Erstellen Sie ein Speicherkonto, um alle Datenträger zu hosten. Sie dürfen nicht mehr als 40 häufig verwendete Datenträger in dem gleichen Speicherkonto platzieren, um die Begrenzung des Speicherkontos von 20.000 IOPS nicht zu überschreiten. In diesem Fall sind wir weit von diesem Grenzwert entfernt, sodass Sie aus Gründen der Einfachheit alles im selben Konto speichern können.

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster
4. Suchen Sie den Namen des CentOS 7-Images des virtuellen Computers.

        azure vm image list | findstr CentOS
   Die Ausgabe sieht in etwa wie folgt aus: `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`.

   Verwenden Sie diesen Namen im folgenden Schritt.
5. Erstellen Sie die Vorlage des virtuellen Computers, und ersetzen Sie dabei „/path/to/key.pem“ durch den Pfad, in dem Sie den generierten PEM-SSH-Schlüssel gespeichert haben.

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser
6. Fügen Sie vier 500-GB-Datenträger an den virtuellen Computer an, die für die RAID-Konfiguration verwendet werden können.

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd
7. Nutzen Sie SSH, um sich bei der Vorlagen-VM anzumelden, die Sie unter „mariadbhatemplate.cloudapp.net:22“ erstellt haben. Stellen Sie unter Verwendung Ihres privaten Schlüssels eine Verbindung her.

### <a name="software"></a>Software
1. Abrufen von „root“

        sudo su

2. Installieren Sie die RAID-Unterstützung:

    a. Installieren von mdadm

              yum install mdadm

    b. Erstellen der RAID0/Stripe-Konfiguration mit einem EXT4-Dateisystem

              mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
              mdadm --detail --scan >> /etc/mdadm.conf
              mkfs -t ext4 /dev/md0
    c. Erstellen des Bereitstellungspunktverzeichnisses

              mkdir /mnt/data
    d. Abrufen der UUID des neu erstellten RAID-Geräts

              blkid | grep /dev/md0
    e. Bearbeiten von „/etc/fstab“

              vi /etc/fstab
    f. Fügen Sie das Gerät hinzu, um die automatische Bereitstellung beim Neustart zu aktivieren, und ersetzen Sie dabei den UUID durch den Wert, den Sie vorher mithilfe des Befehls **blkid** erhalten haben.

              UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2
    g. Bereitstellen der neuen Partition

              mount /mnt/data

3. Installieren Sie MariaDB.

    a. Erstellen Sie die Datei „MariaDB.repo“.

                vi /etc/yum.repos.d/MariaDB.repo

    b. Füllen Sie die REPO-Datei mit dem folgenden Inhalt:

              [mariadb]
              name = MariaDB
              baseurl = http://yum.mariadb.org/10.0/centos7-amd64
              gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
              gpgcheck=1
    c. Entfernen Sie vorhandene Postfix- und MariaDB-Bibliotheken, um Konflikte zu vermeiden.

           yum remove postfix mariadb-libs-*
    d. Installieren von MariaDB mit Galera

           yum install MariaDB-Galera-server MariaDB-client galera

4. Verschieben Sie das MySQL-Datenverzeichnis in das RAID-Blockgerät.

    a. Kopieren Sie das aktuelle MySQL-Verzeichnis an den neuen Speicherort, und entfernen Sie das alte Verzeichnis.

           cp -avr /var/lib/mysql /mnt/data  
           rm -rf /var/lib/mysql
    b. Legen Sie die Berechtigungen für das neue Verzeichnis entsprechend fest.

           chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/

    c. Erstellen Sie einen Symlink, der vom alten Verzeichnis auf den neuen Speicherort auf der RAID-Partition verweist.

           ln -s /mnt/data/mysql /var/lib/mysql

5. Da [SELinux die Clustervorgänge beeinträchtigt](http://galeracluster.com/documentation-webpages/configuration.html#selinux), müssen Sie es für die aktuelle Sitzung deaktivieren. Bearbeiten Sie `/etc/selinux/config`, um es für weitere Neustarts zu deaktivieren.

            setenforce 0

            then editing `/etc/selinux/config` to set `SELINUX=permissive`
6. Überprüfen Sie, ob MySQL ausgeführt wird.

   a. Starten Sie MySQL.

           service mysql start
   b. Schützen Sie die MySQL-Installation, legen Sie das Root-Kennwort fest, entfernen Sie anonyme Benutzer, wobei Sie die Remoteanmeldedaten für Root deaktivieren und die Testdatenbank entfernen.

           mysql_secure_installation
   c. Erstellen Sie einen Benutzer für die Datenbank für Clustervorgänge und optional für Ihre Anwendungen.

           mysql -u root -p
           GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
           exit

   d. Beenden Sie MySQL.

            service mysql stop
7. Erstellen Sie einen Konfigurationsplatzhalter.

   a. Bearbeiten Sie die MySQL-Konfiguration, um einen Platzhalter für die Clustereinstellungen zu erstellen. Ersetzen Sie **`<Variables>`** nicht, und heben Sie die Auskommentierung nicht auf. Dies geschieht nach dem Erstellen eines virtuellen Computers anhand dieser Vorlage.

            vi /etc/my.cnf.d/server.cnf
   b. Bearbeiten Sie den Abschnitt **[galera]**, und bereinigen Sie ihn.

   c. Bearbeiten Sie den Abschnitt **[mariadb]**.

           wsrep_provider=/usr/lib64/galera/libgalera_smm.so
           binlog_format=ROW
           wsrep_sst_method=rsync
           bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
           default_storage_engine=InnoDB
           innodb_autoinc_lock_mode=2

           wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
           #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
           #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
           #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
           #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server
8. Öffnen Sie die erforderlichen Ports in der Firewall mithilfe von FirewallD unter CentOS 7.

   * MySQL: `firewall-cmd --zone=public --add-port=3306/tcp --permanent`
   * GALERA: `firewall-cmd --zone=public --add-port=4567/tcp --permanent`
   * GALERA IST: `firewall-cmd --zone=public --add-port=4568/tcp --permanent`
   * RSYNC: `firewall-cmd --zone=public --add-port=4444/tcp --permanent`
   * Laden Sie die Firewall neu: `firewall-cmd --reload`

9. Optimieren Sie die Leistung des Systems. Weitere Informationen finden Sie unter [Strategie zur Optimierung der Leistung](virtual-machines-linux-classic-optimize-mysql.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

   a. Bearbeiten Sie die MySQL-Konfigurationsdatei erneut.

            vi /etc/my.cnf.d/server.cnf
   b. Bearbeiten Sie den Abschnitt **[mariadb]**, und fügen Sie folgenden Inhalt an:

   > [!NOTE]
   > Es empfiehlt sich, „innodb\_buffer\_pool_size“ auf 70 % des Arbeitsspeichers Ihres virtuellen Computers festzulegen. In diesem Fall wurde dieser Wert für den mittleren virtuellen Azure-Computer mit 3,5 GB RAM auf 2,45 GB festgelegt.
   >
   >

           innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70 percent of physical memory.
           innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
           max_connections = 5000 # A larger value will give the server more time to recycle idled connections
           innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
           innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
           innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
           query_cache_size = 0
10. Beenden Sie MySQL. Deaktivieren Sie den MySQL-Dienst so, dass er beim Start nicht ausgeführt wird, um beim Hinzufügen eines neuen Knotens den Cluster nicht zu stören. Heben Sie außerdem die Bereitstellung des Computers auf.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision
11. Erfassen Sie den virtuellen Computer über das Portal: (Derzeit wird unter dem [Problem 1268 für die Azure CLI-Tools](https://github.com/Azure/azure-xplat-cli/issues/1268) beschrieben, dass bei Images, die mit den Azure CLI-Tools erfasst werden, die angeschlossenen Datenträger nicht erfasst werden.)

    a. Fahren Sie den virtuellen Computer über das Portal herunter.

    b. Klicken Sie auf **Erfassen**, und geben Sie den Namen des Images als **mariadb-galera-image** an. Geben Sie eine Beschreibung ein, und versehen Sie „Ich habe waagent ausgeführt“ mit einem Häkchen.
      
      ![Erfassen des virtuellen Computers](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture2.PNG)

## <a name="create-the-cluster"></a>Cluster erstellen
Erstellen Sie drei virtuelle Computer anhand der Vorlage, die Sie gerade erstellt haben, und konfigurieren und starten Sie dann den Cluster.

1. Erstellen Sie den ersten virtuellen CentOS 7-Computer anhand des erstellten Images „mariadb-galera-image“, und geben Sie die folgenden Informationen an:

 - Name des virtuellen Netzwerks: mariadbvnet
 - Subnetz: mariadb
 - VM-Größe: Mittel
 - Name des Clouddiensts: mariadbha (oder beliebiger Name, auf den über „mariadbha.cloudapp.net“ zugegriffen werden soll)
 - Computername: mariadb1
 - Benutzername: azureuser
 - SSH-Zugriff: Aktiviert
 - Übergeben Sie die PEM-Datei des SSH-Zertifikat, und ersetzen Sie „/path/to/key.pem“ durch den Pfad, in dem Sie den generierten PEM-SSH-Schlüssel gespeichert haben.

   > [!NOTE]
   > Die folgenden Befehle sind aus Gründen der Übersichtlichkeit auf mehrere Zeilen aufgeteilt. Sie sollten jedoch jeden Befehl als eine Zeile eingeben.
   >
   >
        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser
2. Erstellen Sie zwei weitere virtuelle Computer, und verbinden Sie sie mit dem Clouddienst „mariadbha“. Ändern Sie den VM-Namen und den SSH-Port in einen eindeutigen Port, der nicht mit anderen virtuellen Computern im selben Clouddienst in Konflikt steht.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
  Für MariaDB3:

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser
3. Für den nächsten Schritt müssen Sie die interne IP-Adresse aller drei virtuellen Computer abrufen:

    ![Beziehen der IP-Adresse](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/IP.png)
4. Verwenden Sie SSH, um sich bei den drei virtuellen Computern anzumelden, und bearbeiten Sie jeweils die Konfigurationsdatei.

        sudo vi /etc/my.cnf.d/server.cnf

    Heben Sie die Auskommentierung von ** `wsrep_cluster_name` ** und ** `wsrep_cluster_address` ** durch Entfernen von **#** am Anfang der Zeile auf.
    Ersetzen Sie außerdem **`<ServerIP>`** in **`wsrep_node_address`** und **`<NodeName>`** in **`wsrep_node_name`** durch die IP-Adresse bzw. den Namen des virtuellen Computers, und kommentieren Sie auch diese Zeilen aus.
5. Starten Sie den Cluster auf MariaDB1, und führen Sie ihn beim Start aus.

        sudo service mysql bootstrap
        chkconfig mysql on
6. Starten Sie MySQL auf MariaDB2 und MariaDB3, und führen Sie die Anwendung beim Start aus.

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balance-the-cluster"></a>Lastenausgleich für den Cluster
Als Sie die gruppierten virtuellen Computer erstellt haben, haben Sie sie einer Verfügbarkeitsgruppen mit dem Namen „clusteravset“ hinzugefügt, um sicherzustellen, dass sie in unterschiedlichen Fehler- und Updatedomänen platziert werden und dass von Azure niemals eine Wartung auf allen Computern gleichzeitig ausgeführt wird. Diese Konfiguration erfüllt die Anforderungen, die von dieser Azure-SLA (Vereinbarung zum Servicelevel) unterstützt werden sollen.

Nun können Sie Azure Load Balancer verwenden, um Anforderungen gleichmäßig auf die drei Knoten zu verteilen.

Führen Sie die folgenden Befehle auf Ihrem Computer mithilfe der Azure CLI aus.

Struktur der Befehlsparameter: `azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

Die CLI legt das Load Balancer-Testintervall auf 15 Sekunden fest, was möglicherweise etwas zu lang. Ändern Sie diesen Wert für die virtuellen Computer im Portal unter **Endpunkte**.

![Endpunkt bearbeiten](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint.PNG)

Aktivieren Sie **Gruppe mit Lastenausgleich neu konfigurieren**.

![Gruppe mit Lastenausgleich neu konfigurieren](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint2.PNG)

Ändern Sie dann **Testintervall** in 5 Sekunden, und speichern Sie Ihre Änderungen.

![Testintervall ändern](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validate-the-cluster"></a>Überprüfen des Clusters
Die harte Arbeit ist nun erledigt. Der Cluster ist jetzt unter `mariadbha.cloudapp.net:3306` für den Lastenausgleich erreichbar und leitet Anforderungen zwischen den drei virtuellen Computern reibungslos und effektiv weiter.

Verwenden Sie Ihren bevorzugten MySQL-Client, um eine Verbindung herzustellen, oder stellen Sie einfach eine Verbindung von einem der virtuellen Computer her, um zu überprüfen, ob dieser Cluster funktioniert.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Erstellen Sie dann eine Datenbank, und füllen Sie sie mit Daten.

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

Die Datenbank, die Sie erstellt haben, gibt die folgende Tabelle zurück:

    +----+--------+
    | id | value  |
    +----+--------+
    |  1 | Value1 |
    |  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie einen hoch verfügbaren MariaDB + Galera-Cluster mit drei Knoten auf virtuellen Azure-Computern erstellt, auf denen CentOS 7 ausgeführt wird. Der Lastenausgleich für die virtuellen Computer erfolgt mit Azure Load Balancer.

Es empfiehlt sich unter Umständen, sich über [eine weitere Möglichkeit zur Erstellung eines MySQL-Clusters unter Linux](virtual-machines-linux-classic-mysql-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) sowie über Möglichkeiten zum [Optimieren und Testen der MySQL-Leistung auf virtuellen Azure-Computern mit Linux](virtual-machines-linux-classic-optimize-mysql.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) zu informieren.

<!--Anchors-->
[Architecture overview]: #architecture-overview
[Creating the template]: #creating-the-template
[Creating the cluster]: #creating-the-cluster
[Load balancing the cluster]: #load-balancing-the-cluster
[Validating the cluster]: #validating-the-cluster
[Next steps]: #next-steps

<!--Image references-->

<!--Link references-->
[Galera]: http://galeracluster.com/products/
[MariaDBs]: https://mariadb.org/en/about/
[SSH-Schlüssel für die Authentifizierung erstellen]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[issue #1268 in the Azure CLI]:https://github.com/Azure/azure-xplat-cli/issues/1268

