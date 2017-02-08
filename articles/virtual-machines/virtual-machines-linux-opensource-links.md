---
title: Linux und Open Source-Computing in Azure | Microsoft Docs
description: "Dieses Thema enthält eine Liste von Artikeln zu Linux- und Open Source-Computing auf Azure, u.a. die grundlegende Verwendung von Linux, einige grundlegende Konzepte zum Ausführen oder Hochladen von Linux-Images auf Azure und anderen Inhalten zu bestimmten Technologien und Optimierungen."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: a7e608b5-26ea-41e0-b46b-1a483a257754
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/27/2016
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: f6537e4ebac76b9f3328223ee30647885ee15d3e
ms.openlocfilehash: 2224028e314ca32b51c36cc2b9b8134a5eda330a


---
# <a name="linux-and-open-source-computing-on-azure"></a>Linux und Open-Source-Computing auf Azure
Hier finden Sie die gesamte Dokumentation, die Sie zum Erstellen und Verwalten Linux-basierter virtueller Computer im klassischen Bereitstellungsmodell benötigen.

> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells.

## <a name="get-started"></a>Erste Schritte
* [Einführung in Linux in Azure](virtual-machines-linux-intro-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Häufig gestellte Fragen zu virtuellen Azure-Computern, die mit dem klassischen Bereitstellungsmodell erstellt wurden](virtual-machines-linux-classic-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Informationen zu Images für virtuelle Computer](virtual-machines-linux-classic-about-images.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Hochladen Ihres eigenen Distro-Images](virtual-machines-linux-classic-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) (plus Anweisungen zum Verwenden einer [unterstützten Azure-Distribution](virtual-machines-linux-endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json))
* [Anmelden bei einem virtuellen Linux-Computer über das klassische Azure-Portal](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="set-up"></a>Einrichtung
* [Installieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md)

## <a name="tutorials"></a>Lernprogramme
* [Installieren des LAMP-Stapels auf einem virtuellen Linux-Computer in Azure](virtual-machines-linux-create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ruby on Rails-Webanwendung auf einem virtuellen Azure-Computer](linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)
* [Gewusst wie: Installieren von Apache Qpid Proton-C für AMQP und Service Bus](../service-bus-messaging/service-bus-amqp-apache.md)

### <a name="databases"></a>Datenbanken
* [Optimieren der Leistung von MySQL in Azure](virtual-machines-linux-classic-optimize-mysql.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [MySQL-Cluster](virtual-machines-linux-classic-mysql-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Ausführen von Cassandra mit Linux auf Azure und Zugreifen aus Node.js](virtual-machines-linux-classic-cassandra-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Erstellen eines Clusters mit mehreren Mastern von MariaDbs](virtual-machines-linux-classic-mariadb-mysql-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="hpc"></a>HPC
* [Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Ausführen von NAMD mit dem Microsoft HPC Pack auf Linux-Computeknoten in Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](virtual-machines-linux-classic-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="docker"></a>Docker
* [Verwenden der Docker-VM-Erweiterung aus der Azure-Befehlszeilenschnittstelle (Azure-CLI)](virtual-machines-linux-classic-cli-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Verwenden der Docker-VM-Erweiterung über das Azure-Portal](virtual-machines-linux-classic-portal-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Verwenden von "docker-machine" auf Azure](virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="ubuntu"></a>Ubuntu
* [Gewusst wie: MySQL-Cluster](virtual-machines-linux-classic-mysql-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Gewusst wie: Node.js und Cassandra](virtual-machines-linux-classic-cassandra-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="opensuse"></a>OpenSUSE
* [Gewusst wie: Installieren und Ausführen von MySQL](virtual-machines-linux-classic-mysql-on-opensuse.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="coreos"></a>CoreOS
* [Gewusst wie: Verwenden von CoreOS in Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)

## <a name="planning"></a>Planung
* [Implementierungsrichtlinien für Azure-Infrastrukturdienste](virtual-machines-linux-infrastructure-subscription-accounts-guidelines.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Auswählen von Linux-Benutzernamen](virtual-machines-linux-usernames.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Konfigurieren einer Verfügbarkeitsgruppe für virtuelle Computer im klassischen Bereitstellungsmodell](virtual-machines-linux-classic-configure-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Gewusst wie: Planen der Wartung auf virtuellen Azure-Computern](virtual-machines-linux-planned-maintenance-schedule.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Verwalten der Verfügbarkeit virtueller Computer](virtual-machines-linux-manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Geplante Wartung für virtuelle Linux-Computer in Azure](virtual-machines-linux-planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="deployment"></a>Bereitstellung
* [Erstellen eines benutzerdefinierten virtuellen Linux-Computers](virtual-machines-linux-classic-createportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Grundlagen: Erfassen eines virtuellen Linux-Computers zum Erstellen einer Vorlage](virtual-machines-linux-classic-capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Informationen zu nicht unterstützten Verteilungen](virtual-machines-linux-create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="management"></a>Verwaltung
* [SSH](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Zurücksetzen von Kennwörtern oder SSH-Eigenschaften für Linux](virtual-machines-linux-classic-reset-access.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Verwenden von Root](virtual-machines-linux-use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="azure-resources"></a>Azure-Ressourcen
* [Azure Linux Agent](virtual-machines-linux-agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure-VM-Erweiterungen und Features](virtual-machines-windows-extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Einfügen benutzerdefinierter Daten in einen virtuellen Computer zur Verwendung mit Cloud-init](virtual-machines-windows-classic-inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="storage"></a>Speicher
* [Anfügen eines Datenträgers an einen virtuellen Linux-Computer](virtual-machines-linux-classic-attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Trennen eines Datenträgers von einem virtuellen Linux-Computer](virtual-machines-linux-classic-detach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [RAID](virtual-machines-linux-configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="networking"></a>Netzwerk
* [Gewusst wie: Einrichten von Endpunkten auf einem klassischen virtuellen Computer in Azure](virtual-machines-linux-classic-setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

## <a name="troubleshooting"></a>Problembehandlung
* [Behandeln von Problemen mit Secure Shell (SSH)-Verbindungen mit einem Linux-basierten virtuellen Azure-Computer](virtual-machines-linux-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Behandeln von Problemen beim Erstellen eines neuen virtuellen Linux-Computers in Azure (klassisches Bereitstellungsmodell)](virtual-machines-linux-classic-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)  
* [Behandeln von Problemen beim Neustart oder Ändern der Größe eines vorhandenen virtuellen Linux-Computers in Azure (klassisches Bereitstellungsmodell)](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) 

## <a name="reference"></a>Referenz
* [Befehle der Azure-Befehlszeilenschnittstelle im Modus „Azure-Dienstverwaltung“ (Azure Service Management, ASM)](../virtual-machines-command-line-tools.md)
* [Azure-Dienstverwaltungs-REST-API](https://msdn.microsoft.com/library/azure/ee460799.aspx)

## <a name="general-links"></a>Allgemeine Links
Im Gegensatz zu der oben genannten Dokumentation auf Azure.com sind im Folgenden Links für Microsoft-Blogs, TechNet-Seiten und externe Websites aufgeführt. Da sowohl Azure als auch Open Source-Computing einer ständigen Weiterentwicklung unterliegen, sind diese Links vermutlich nicht mehr auf dem aktuellen Stand. *Trotz* dieser Tatsache geben wir unser Bestes, kontinuierlich neue Themen hinzuzufügen und veraltete Einträge zu entfernen. Wenn wir ein Thema vergessen haben, senden Sie uns einen Kommentar, oder übermitteln Sie eine Pull-Anforderung an das [Github-Repository](https://github.com/Azure/azure-content/).

* [Ausführen von ASP.NET 5 auf Linux mithilfe von Docker-Containern](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
* [Bereitstellen eines CentOS-VM-Images aus OpenLogic](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
* [SUSE-Updateinfrastruktur](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
* [SUSE Linux Enterprise Server for SAP Cloud Appliance Library](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)
* [Erstellen von Linux auf Azure in 12 Schritten für Hochverfügbarkeit](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
* [Automatisieren der Bereitstellung von Linux auf Azure mit der Azure-Befehlszeilenschnittstelle, node.js, jhawk](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
* [GlusterFS auf Azure](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

### <a name="freebsd"></a>FreeBSD
* [Ausführen von FreeBSD in Azure](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
* [Einfache Bereitstellung von FreeBSD](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
* [Bereitstellen eines benutzerdefinierten FreeBSD-Images](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
* [Kaspersky AV für Linux-Dateiserver](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

### <a name="nosql"></a>NoSQL
* [8 NoSql-Datenbanken für Azure (Open-Source)](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
* [Slideshare (MSOpenTech): Erfahrungen mit CouchDb auf Azure](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
* [Ausführen von CouchDB-as-a-Service mit node.js, CORS und Grunt](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)
* [Redis unter Windows im Azure Redis Cache-Dienst](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
* [Ankündigung des ASP.NET- Sitzungsstatusanbieters für Redis-Vorschauversion](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)
* [Blog: RavenHQ jetzt in Azure Marketplace verfügbar](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### <a name="big-data"></a>Big Data
* [Installieren von Hadoop auf Azure Linux-VMs](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
* [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)

### <a name="relational-database"></a>Relationale Datenbank
* [Hoch verfügbare MySQL-Architektur in Microsoft Azure](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
* [Installing Postgres with corosync, pg_bouncer using ILB](https://github.com/chgeuer/postgres-azure) (Installieren von Postgres mit Corosync Pg_bouncer mit ILB)

### <a name="linux-high-performance-computing-hpc"></a>Linux HPC (High Performance Computing)
* [Quickstart template: Spin up a SLURM cluster](https://github.com/Azure/azure-quickstart-templates/tree/master/slurm) (Schnellstartvorlage: Einrichten eines SLURM-Clusters) und [Blogbeitrag](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx))
* [Schnellstartvorlage: Erstellen eines HPC-Clusters mit Linux-Computeknoten](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

### <a name="devops-management-and-optimization"></a>DevOps, Verwaltung und Optimierung
Dieser Bereich ist recht umfangreich und ändert sich sehr schnell. Sie sollten die Liste unten also als Ausgangspunkt betrachten.

* [Video: Virtuelle Azure-Computer: Verwenden von Chef, Puppet und Docker für die Verwaltung virtueller Linux-Computer](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)
* [Vollständige Anleitung für die automatisierte Kubernetes- Clusterbereitstellung mit CoreOS und Weave](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
* [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)
* [Jenkins Slave-Plug-In für Azure](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
* [GitHub-Repository: Jenkins Speicher-Plug-In für Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)
* [Drittanbieter: Hudson Slave-Plug-In für Azure](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
* [Drittanbieter: Hudson Speicher-Plug-In für Azure](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
* [Video: Was ist Chef, und wie funktioniert es?](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)
* [Video: How to Use Azure Automation with Linux VMs (Video: Verwenden von Azure Automation mit virtuellen Linux-Computern, in englischer Sprache)](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)
* [Blog: Powershell DSC für Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
* [GitHub: Docker Client DSC](https://github.com/anweiss/DockerClientDSC)
* [Packer-Plug-In für Azure](https://github.com/msopentech/packer-azure)




<!--HONumber=Dec16_HO1-->


