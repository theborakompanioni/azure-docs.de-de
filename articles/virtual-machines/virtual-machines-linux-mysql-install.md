---
title: Einrichten von MySQL auf einem virtuellen Linux-Computer in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den MySQL-Stack auf einem virtuellen Linux-Computer (Betriebssystem der Ubuntu- oder RedHat-Familie) in Azure installieren.
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 153bae7c-897b-46b3-bd86-192a6efb94fa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/01/2016
ms.author: mingzhan
translationtype: Human Translation
ms.sourcegitcommit: 233116deaaaf2ac62981453b05c4a5254e836806
ms.openlocfilehash: 84722fffc5419f866250d4093250839066c54c46


---
# <a name="how-to-install-mysql-on-azure"></a>Installieren von MySQL in Azure
In diesem Artikel erfahren Sie, wie Sie MySQL auf einem virtuellen Azure-Computer, auf dem Linux ausgeführt wird, installieren und konfigurieren.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-mysql-on-your-virtual-machine"></a>Installieren von MySQL auf dem virtuellen Computer
> [!NOTE]
> Sie benötigen einen virtuellen Microsoft Azure-Computer mit Linux, um dieses Lernprogramm auszuführen. Bevor Sie fortfahren, informieren Sie sich im [Tutorial zu Azure Linux-VMs](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), wie ein virtueller Linux-Computer mit `mysqlnode` als VM-Namen und `azureuser` als Benutzer erstellt und eingerichtet wird.
> 
> 

Verwenden Sie in diesem Fall Port 3306 als MySQL-Port.  

Stellen Sie über PuTTY eine Verbindung mit dem virtuellen Linux-Computer her. Wenn Sie zum ersten Mal einen virtuellen Linux-Computer in Azure verwenden, finden Sie [hier](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Informationen zum Herstellen einer Verbindung mit einem virtuellen Linux-Computer mit PuTTY.

Wir verwenden zur Veranschaulichung in diesem Artikel das Repositorypaket, um MySQL&5;.6 zu installieren. MySQL&5;.6 zeichnet sich im Vergleich zu MySQL&5;.5 durch mehr Leistung aus.  Weitere Informationen finden Sie [hier](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).

### <a name="how-to-install-mysql56-on-ubuntu"></a>Installieren von MySQL&5;.6 unter Ubuntu
Wir verwenden in diesem Artikel eine Linux-VM mit Ubuntu von Azure.

* Schritt 1: Installieren von MySQL Server 5.6 – Wechseln zum Benutzer `root`:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    Installieren von MySQL Server 5.6:
  
            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6
  
    Während der Installation werden Sie in einem Dialogfenster aufgefordert, das MySQL-Kennwort für "root" festzulegen, was Sie hier tun müssen.
  
    ![image](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p1.png)

    Geben Sie das Kennwort zur Bestätigung erneut ein.

    ![image](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p2.png)

* Schritt 2: Anmelden beim MySQL-Server
  
    Der MySQL-Dienst wird automatisch gestartet, sobald die MySQL-Serverinstallation abgeschlossen ist. Sie können sich mit dem Benutzer `root` beim MySQL-Server anmelden.
    Geben Sie den folgenden Befehl ein, um das Kennwort einzugeben und sich anzumelden.
  
             #[root@mysqlnode ~]# mysql -uroot -p
* Schritt 3: Verwalten des ausgeführten MySQL-Diensts
  
    (a) Abrufen des Status des MySQL-Diensts
  
             #[root@mysqlnode ~]# service mysql status
  
    (b) Starten des MySQL-Diensts
  
             #[root@mysqlnode ~]# service mysql start
  
    (c) Beenden des MySQL-Diensts
  
             #[root@mysqlnode ~]# service mysql stop
  
    (d) Neustarten des MySQL-Diensts
  
             #[root@mysqlnode ~]# service mysql restart

### <a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>Installieren von MySQL in einer Red Hat-Betriebssystemfamilie, z. B. CentOS oder Oracle Linux
Wir verwenden hier eine Linux-VM mit CentOS oder Oracle Linux.

* Schritt 1: Hinzufügen von MySQL Yum-Repositorys – Wechseln zum Benutzer `root`:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    Laden Sie das Paket der gewünschten MySQL-Version herunter, und installieren Sie es:
  
            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm
* Schritt 2: Bearbeiten Sie die nachstehende Datei, um das MySQL-Repository für das Herunterladen des MySQL 5.6.-Pakets vorzubereiten.
  
            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo
  
    Ändern Sie die einzelnen Werte dieser Datei wie folgt:
  
        \# *Enable to use MySQL 5.6*
  
        [mysql56-community]
        name=MySQL 5.6 Community Server
  
        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
        enabled=1
  
        gpgcheck=1
  
        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
* Schritt 3: Installieren von MySQL aus dem des MySQL-Repository – Installieren von MySQL:
  
           #[root@mysqlnode ~]#yum install mysql-community-server
  
    Das MySQL-RPM-Paket und alle zugehörigen Pakete werden installiert.
* Schritt 4: Verwalten des ausgeführten MySQL-Diensts
  
    (a) Überprüfen des Dienststatus des MySQL-Servers:
  
           #[root@mysqlnode ~]#service mysqld status
  
    (b) Überprüfen, ob der Standardport des MySQL-Servers aktiv ist:
  
           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306

    (c) Starten des MySQL-Servers:

           #[root@mysqlnode ~]#service mysqld start

    (d) Beenden des MySQL-Servers:

           #[root@mysqlnode ~]#service mysqld stop

    (e) Festlegen, dass MySQL beim Start des Systems gestartet wird:

           #[root@mysqlnode ~]#chkconfig mysqld on


### <a name="how-to-install-mysql-on-suse-linux"></a>Installieren von MySQL unter Suse Linux
Wir verwenden hier eine Linux-VM mit OpenSUSE.

* Schritt 1: Herunterladen und Installieren von MySQL Server
  
    Wechseln Sie über folgenden Befehl zum Benutzer `root` :  
  
           #sudo su -
  
    Herunterladen und Installieren des MySQL-Pakets:
  
           #[root@mysqlnode ~]# zypper update
  
           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql
* Schritt 2: Verwalten des ausgeführten MySQL-Diensts
  
    (a) Überprüfen des Status des MySQL-Servers:
  
           #[root@mysqlnode ~]# rcmysql status
  
    (b) Überprüfen, ob der Standardport des MySQL-Servers aktiv ist:
  
           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306

    (c) Starten des MySQL-Servers:

           #[root@mysqlnode ~]# rcmysql start

    (d) Beenden des MySQL-Servers:

           #[root@mysqlnode ~]# rcmysql stop

    (e) Festlegen, dass MySQL beim Start des Systems gestartet wird:

           #[root@mysqlnode ~]# insserv mysql

### <a name="next-step"></a>Nächster Schritt
Weitere Informationen zur Verwendung von MySQL finden Sie [hier](https://www.mysql.com/).




<!--HONumber=Jan17_HO5-->


