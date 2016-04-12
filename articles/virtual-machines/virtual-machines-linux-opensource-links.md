<properties
	pageTitle="Linux und Open-Source-Computing auf Azure | Microsoft Azure"
	description="Dieses Thema enthält eine Liste von Artikeln zu Linux- und Open Source-Computing auf Azure, u. a. die grundlegende Verwendung von Linux, einige grundlegende Konzepte zum Ausführen oder Hochladen von Linux-Images auf Azure und anderen Inhalten zu bestimmten Technologien und Optimierungen."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="squillace"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager,azure-service-management"/>

<tags
	ms.service="virtual-machines-linux"
	ms.devlang="NA"
	ms.topic="article"
	ms.tgt_pltfrm="vm-linux"
	ms.workload="infrastructure-services"
	ms.date="03/26/2016"
	ms.author="rasquill"/>



# Linux und Open-Source-Computing auf Azure

Dieses Dokument versucht, an einem Ort alle Themen zum klassischen Bereitstellungsmodell aufzulisten, die von Microsoft und seinen Partnern zur Ausführung von Linux-basierten virtuellen Maschinen und anderen Open Source-Computing-Umgebungen und Anwendungen auf Microsoft Azure erstellt wurden.

Da sowohl Azure als auch das Open-Source-Computing einer ständigen Weiterentwicklung unterliegen ist dieses Dokument vermutlich nicht mehr auf dem aktuellen Stand. *Trotz* dieser Tatsache geben wir unser Bestes, kontinuierlich neue Themen hinzuzufügen und veraltete Einträge zu entfernen. Wenn wir ein Thema vergessen haben, senden Sie uns einen Kommentar, oder übermitteln Sie eine Pull-Anforderung an das [Github-Repository](https://github.com/Azure/azure-content/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## Allgemeine Hinweise
Die Abschnitte werden am rechten Rand dieser Seite aufgeschlüsselt. (Links können in mehr als einem Abschnitt auftreten, da Themen sich mit mehr als einem Konzept, mehr als einer Distribution oder mehr als einer Technologie beschäftigen können.) Darüber hinaus gibt es mehrere Themen, in denen verschiedene Linux-Optionen, Image-Repositorys, Fallstudien und Gewusst-wie-Themen zum Hochladen Ihrer eigenen benutzerdefinierten Images beschrieben werden:

- [Azure Marketplace](https://azure.microsoft.com/marketplace/virtual-machines/)
- [MSOpenTech VM Depot](https://vmdepot.msopentech.com/List/Index)
- [Veranstaltungen und Demonstrationen: Microsoft Openness CEE](http://www.opennessatcee.com/)
- [Gewusst wie: Hochladen Ihres eigenen Distro-Images](virtual-machines-linux-classic-create-upload-vhd.md) (plus Anweisungen zum Verwenden einer [unterstützten Azure-Verteilung](virtual-machines-linux-endorsed-distros.md))
- [Hinweise: Allgemeine Linux-Anforderungen für die Ausführung in Azure](virtual-machines-linux-create-upload-generic.md)
- [Hinweise: Allgemeine Einführung zu Linux auf Azure](virtual-machines-linux-intro-on-azure.md)

## Distributionen

Es gibt Unmengen von Linux-Verteilungen, die in der Regel durch die Paket-Verwaltungssysteme unterteilt sind. Einige sind dpgk-basiert, wie Debian und Ubuntu, andere sind rpm-basiert, wie CentOS, SUSE und RedHat. Einige Unternehmen bieten Distributions-Images als formale Partner von Microsoft. Diese werden unterstützt. Andere werden von der Community bereitgestellt. Für die Distributionen in diesem Abschnitt gibt es formale Artikel, selbst wenn sie nur in Beispielen für andere Technologien verwendet wurden.

### [Ubuntu](https://azure.microsoft.com/marketplace/partners/Canonical/)

Ubuntu ist eine äußerst populäre und von Azure unterstützte Linux-Verteilung, die auf der dkpg- und apt-get-Paketverwaltung basiert.

3. [Gewusst wie: MySQL-Cluster](virtual-machines-linux-classic-mysql-cluster.md)
4. [Gewusst wie: Node.js und Cassandra](virtual-machines-linux-classic-cassandra-nodejs.md)
6. [Ausführen von ASP.NET 5 auf Linux mithilfe von Docker-Containern](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)

### CentOS

Die CentOS-Linux-Distribution ist eine stabile, vorhersehbare, verwaltbare und reproduzierbare Plattform, die aus den Quellen von Red Hat Enterprise Linux (RHEL) abgeleitet wurde.

4. [Blog: Bereitstellen eines CentOS-VM-Images aus OpenLogic](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
6. [Gewusst wie: Installieren von Apache Qpid Proton-C für AMQP und Service Bus](../service-bus/service-bus-amqp-apache.md/)

### SUSE Linux Enterprise Server und openSUSE

11. [Gewusst wie: Installieren und Ausführen von MySQL](virtual-machines-linux-classic-mysql-on-opensuse.md)
13. [[SUSE-Forum] How to: Move to a New Patch Server (Wechseln zu einem neuen Patchserver, in englischer Sprache)](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
14. [Images: SUSE Linux Enterprise Server für SAP Cloud Appliance Library](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)

### CoreOS

CoreOS ist eine kleine, optimierte Distribution für die reine Computingskalierung mit einem hohen Maß an Kontrolle hinsichtlich der Anpassung.

10. [Image-Katalog](https://azure.microsoft.com/marketplace/partners/coreos/)  
11. [Gewusst wie: Verwenden von CoreOS in Azure](virtual-machines-linux-classic-coreos-howto.md)
12. [Gewusst wie: Erste Schritte mit Fleet und Docker unter CoreOS in Azure](virtual-machines-linux-classic-coreos-fleet-get-started.md)


### FreeBSD

12. [MSOpenTech VM Depot](https://vmdepot.msopentech.com/List/Index?sort=Date&search=FreeBSD)
13. [Blog: Ausführen von FreeBSD in Azure](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
14. [Blog: Einfache Bereitstellung von FreeBSD](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
15. [Blog: Bereitstellen eines benutzerdefinierten FreeBSD-Images](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
18. [Marketplace: Kaspersky AV für Linux-Dateiserver](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

## Grundlagen

1. [Grundlagen: Azure-Befehlszeilenschnittstelle (Azure-CLI)](../xplat-cli-install.md)
5. [Grundlagen: Auswählen von Linux-Benutzernamen](virtual-machines-linux-usernames.md)
6. [Grundlagen: Anmelden bei einem virtuellen Linux-Computer über das klassische Azure-Portal](virtual-machines-linux-classic-log-on.md)
7. [Grundlagen: SSH](virtual-machines-linux-ssh-from-linux.md)
8. [Grundlagen: Zurücksetzen von Kennwörtern oder SSH-Eigenschaften für Linux](virtual-machines-linux-classic-reset-access.md)
9. [Grundlagen: Verwenden von Root](virtual-machines-linux-use-root-privileges.md)
10. [Grundlagen: Anfügen eines Datenträgers an einen virtuellen Linux-Computer](virtual-machines-linux-classic-attach-disk.md)
11. [Grundlagen: Trennen eines Datenträgers von einem virtuellen Linux-Computer](virtual-machines-linux-classic-detach-disk.md)
12. [Blog zu den Grundlagen: Optimieren von Speicher, Festplatten und Leistung mit Linux und Azure](http://blogs.msdn.com/b/igorpag/archive/2014/10/23/azure-storage-secrets-and-linux-i-o-optimizations.aspx)
13. [Grundlagen: RAID](virtual-machines-linux-configure-raid.md)
14. [Grundlagen: Erfassen eines virtuellen Linux-Computers zum Erstellen einer Vorlage](virtual-machines-linux-classic-capture-image.md)
15. [Grundlagen: Der Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md)
16. [Grundlagen: Azure-VM-Erweiterungen und -Features](virtual-machines-windows-extensions-features.md)
17. [Grundlagen: Einfügen benutzerdefinierter Daten in einen virtuellen Computer zur Verwendung mit "Cloud-init"](virtual-machines-windows-classic-inject-custom-data.md)
18. [Blog zu den Grundlagen: Erstellen von Linux auf Azure in zwölf Schritten für Hochverfügbarkeit](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
19. [Blog zu den Grundlagen: Automatisieren der Bereitstellung von Linux auf Azure mit der Azure-CLI, node.js, jhawk](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
23. [Azure-Dienstverwaltungs-REST-API](https://msdn.microsoft.com/library/azure/ee460799.aspx) – Referenz
24. [GlusterFS auf Azure](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

## Community-Images und Repositorys
4. [GitHub](https://github.com/Azure/) &ndash; für die Azure-Befehlszeilenschnittstelle und viele weitere Tools und Projekte.
5. [Docker-Hub-Registrierung](https://registry.hub.docker.com/) &ndash; die Registrierung für Docker-Container-Images.

## Data

Dieser Abschnitt enthält Informationen zu mehreren verschiedenen Speicheransätzen und Technologien, einschließlich NoSQL, Relational und Big Data.

### NoSQL

1. [Blog: 8 NoSql-Datenbanken für Azure (Open-Source)](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
2. Couchdb
    - [Slideshare (MSOpenTech): Erfahrungen mit CouchDb auf Azure](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
    - [Blog:Ausführen von CouchDB-as-a-Service mit node.js, CORS und Grunt](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)
4. Cassandra
    - [Gewusst wie: Ausführen von Cassandra mit Linux auf Azure und Zugreifen aus Node.js](virtual-machines-linux-classic-cassandra-nodejs.md)
5. Redis
    - [Blog: Redis unter Windows im Azure Redis Cache-Dienst](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
    - [Blog: Ankündigung des ASP.NET-Sitzungsstatusanbieters für Redis-Vorschauversion](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)
6. RavenHQ
    - [Blog: RavenHQ jetzt in Azure Marketplace verfügbar](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### Big Data
2. Hadoop/Cloudera  
	- [Blog: Installieren von Hadoop auf Azure Linux-VMs](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
3. [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) – ein vollständig verwalteter Hadoop-Dienst auf Azure.

### Relationale Datenbank
2. MySQL
    - [Gewusst wie: Installieren und Ausführen von MySQL](virtual-machines-linux-classic-mysql-on-opensuse.md)
    - [Gewusst wie: Optimieren der Leistung von MySQL in Azure](virtual-machines-linux-classic-optimize-mysql.md)
    - [Gewusst wie: MySQL-Cluster](virtual-machines-linux-classic-mysql-cluster.md)
    - [Hoch verfügbare MySQL-Architektur in Microsoft Azure](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
7. MariaDB
    - [Gewusst wie: Erstellen eines Clusters mit mehreren Mastern von MariaDbs](virtual-machines-linux-classic-mariadb-mysql-cluster.md)
8. [Installieren von Postgres mit Corosync Pg\_bouncer mit ILB](https://github.com/chgeuer/postgres-azure)


## Authentifizierung und Verschlüsselung

Authentifizierung und Verschlüsselung sind wichtige Themen in der Software-Entwicklung, und es gibt sehr viele Themen im Internet, die beschreiben, wie Sie angemessene Sicherheitsverfahren für beides erlernen und verwenden. Wir beschreiben einen Teil der grundlegenden Verwendung, um schnell Linux- und Open-Source-Workloads verwenden sowie auf Tools verweisen zu können, mit denen Remote-Sicherheitsfeatures zurückgesetzt oder entfernt werden können. Hierbei handelt es sich um grundlegende Verfahren, es werden in Kürze komplexere Szenarien folgen.

4. [Grundlagen: Zertifikatverwendung und -verwaltung](http://msdn.microsoft.com/library/azure/gg981929.aspx)
7. [Grundlagen: SSH](virtual-machines-linux-ssh-from-linux.md)
8. [Grundlagen: Zurücksetzen von Kennwörtern oder SSH-Eigenschaften für Linux](virtual-machines-linux-classic-reset-access.md)
9. [Grundlagen: Verwenden von Root](virtual-machines-linux-use-root-privileges.md)

## Linux HPC (High Performance Computing)

Führen Sie HPC-Workloads auf Linux-VM-Clustern aus, die mit Open-Source-Tools oder mit dem Microsoft HPC Pack erstellt wurden.

1.	[Schnellstartvorlage: Einrichten eines SLURM-Clusters](https://azure.microsoft.com/documentation/templates/slurm/) und [Blogbeitrag](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)
3.	[Schnellstartvorlage: Erstellen eines HPC-Clusters mit Linux-Computeknoten](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)
4.	[Tutorial: Erste Schritte mit Linux-Computeknoten in einem HPC Pack-Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md)
5.	[Tutorial: Ausführen von NAMD mit dem Microsoft HPC Pack auf Linux-Computeknoten in Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
6.	[Tutorial: Einrichten eines Linux RDMA-Clusters zum Ausführen von MPI-Anwendungen](virtual-machines-linux-classic-rdma-cluster.md)


## DevOps, Verwaltung und Optimierung

Dieser Abschnitt beginnt mit einem Blogeintrag mit einer Reihe von Videos unter [Video: Virtuelle Azure-Computer: Verwenden von Chef, Puppet und Docker für die Verwaltung virtueller Linux-Computer.](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/). Der Bereich "DevOps, Verwaltung und Optimierung" ist jedoch ziemlich umfangreich und verändert sich sehr schnell. Sie sollten die Liste unten also als Ausgangspunkt betrachten.

1. Docker
	- [Verwenden der Docker-VM-Erweiterung aus der Azure-Befehlszeilenschnittstelle (Azure-CLI)](virtual-machines-linux-classic-cli-use-docker.md)
	- [Verwenden der Docker-VM-Erweiterung über das Azure-Portal](virtual-machines-linux-classic-portal-use-docker.md)
    - [Schneller Einstieg in Docker in Azure Marketplace](virtual-machines-linux-classic-docker-quickstart.md)
	- [Verwenden von "docker-machine" auf Azure](virtual-machines-linux-classic-docker-machine.md)

2. [Fleet mit CoreOS](virtual-machines-linux-classic-coreos-howto.md)
4. Kubernetes
	- [Vollständige Anleitung für die automatisierte Kubernetes- Clusterbereitstellung mit CoreOS und Weave](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
	- [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)
5. Jenkins und Hudson
	- [Blog: Jenkins Slave-Plug-In für Azure](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
	- [GitHub-Repository: Jenkins Speicher-Plug-In für Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)
	- [Drittanbieter: Hudson Slave-Plug-In für Azure](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
	- [Drittanbieter: Hudson Speicher-Plug-In für Azure](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
10. Chef
	- [Video: Was ist Chef, und wie funktioniert es?](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)

12. Azure-Automatisierung
	- [Video: Verwenden von Azure-Automatisierung mit virtuellen Linux-Computern](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)
13. Powershell DSC für Linux
    - [Blog: Powershell DSC für Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
    - [GitHub: Docker Client DSC](https://github.com/anweiss/DockerClientDSC)
13. [Ubuntu Juju](https://juju.ubuntu.com/docs/config-azure.html)
14. [Packer-Plug-In für Azure](https://github.com/msopentech/packer-azure)

## Support, Problembehandlung und weitergehende Problembehandlung

1. Microsoft-Support-Dokumentation
	- [Unterstützung: Unterstützung für Linux-Images in Microsoft Azure](http://support2.microsoft.com/kb/2941892)

<!---HONumber=AcomDC_0330_2016-->