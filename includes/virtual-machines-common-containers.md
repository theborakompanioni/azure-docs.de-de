Azure-Cloudlösungen basieren auf virtuellen Computern (Emulation von physischer Computerhardware) und ermöglichen agiles Verpacken von Softwarebereitstellungen sowie eine deutlich bessere Ressourcenkonsolidierung als physische Hardware. Dank [Docker](https://www.docker.com)-Containern und des Docker-Ökosystems können Sie verteilte Software auf äußerst vielfältige Weise entwickeln, ausliefern und verwalten. Anwendungscode in einem Container wird vom virtuellen Hostcomputer sowie von anderen Containern auf dem gleichen virtuellen Computer isoliert. Diese Isolierung ermöglicht eine agilere Entwicklung und Bereitstellung.

Azure bietet folgende Docker-Vorteile:

* [Viele](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) [verschiedene](../articles/virtual-machines/linux/dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Möglichkeiten zum Erstellen von Docker-Hosts für Container, die zu Ihren individuellen Anforderungen passen
* Der [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) erstellt Cluster mit Containerhosts, indem Orchestrators wie **Marathon** und **Swarm** verwendet werden.
* [Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md) und [Ressourcengruppenvorlagen](../articles/resource-group-authoring-templates.md) zur Vereinfachung von Bereitstellung und Updates von komplexen verteilten Anwendungen
* Integration einer großen Anzahl von proprietären und Open-Source-Tools für die Konfigurationsverwaltung

Da Sie virtuelle Computer und Linux-Container in Azure programmgesteuert erstellen können, können Sie zudem auch Tools zur *Orchestrierung* für virtuelle Computer und Container verwenden, um VM-Gruppen (Virtual Machines, virtuelle Computer) zu erstellen und Anwendungen in Linux-Containern und jetzt auch in [Windows-Containern](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)bereitzustellen.

In diesem Artikel werden diese Konzepte nicht nur erörtert, sondern Sie finden auch zahlreiche Links zu weiteren Informationen, Lernprogrammen und Produkten in Bezug auf die Container- und Clusternutzung in Azure. Wenn für Sie lediglich die Links von Interesse sind, finden Sie diese in den [Tools für die Arbeit mit Containern](#tools-for-working-with-azure-vms-and-containers).

## <a name="the-difference-between-virtual-machines-and-containers"></a>Der Unterschied zwischen virtuellen Computern und Containern
Virtuelle Computer werden in einer Virtualisierungsumgebung mit isolierter Hardware ausgeführt, die von einem [Hypervisor](http://en.wikipedia.org/wiki/Hypervisor) bereitgestellt wird. In Azure wird das alles vom Dienst [Virtual Machines](https://azure.microsoft.com/services/virtual-machines/) übernommen: Sie erstellen einen virtuellen Computer, indem Sie das Betriebssystem auswählen und den Computer konfigurieren – oder ein benutzerdefiniertes VM-Image hochladen. Virtuelle Computer sind eine bewährte, vielfach erprobte Technologie, und es stehen viele Tools für die Verwaltung des enthaltenen Betriebssystems und der enthaltenen Apps zur Verfügung.  Apps auf einem virtuellen Computer sind für das Hostbetriebssystem nicht sichtbar. Für eine Anwendung auf einem virtuellen Computer oder den Benutzer eines virtuellen Computers wirkt der virtuelle Computer wie ein autonomer physischer Computer.

[Linux-Container](http://en.wikipedia.org/wiki/LXC) und Container, die mithilfe von Docker-Tools erstellt und gehostet werden, verwenden keinen Hypervisor zur Isolation. Bei Containern verwendet der Containerhost die Isolationsfeatures für Prozesse und Dateisysteme des Linux-Kernels, um für den Container (und dessen Apps) bestimmte Kernel-Features und das eigene isolierte Dateisystem verfügbar zu machen. Aus der Perspektive einer App, die in einem Container ausgeführt wird, wirkt der Container wie eine eigenständige Betriebssysteminstanz. Für eine enthaltene App sind Prozesse und andere Ressourcen außerhalb ihres Containers nicht sichtbar.

In einem Docker-Container werden wesentlich weniger Ressourcen verwendet als auf einem virtuellen Computer. Docker-Container verwenden ein Anwendungsisolations- und -ausführungsmodell ohne gemeinsame Nutzung des Kernels des Docker-Hosts. Der Datenträgerbedarf des Containers ist erheblich geringer, da er nicht das gesamte Betriebssystem enthält. Die Startzeit ist deutlich kürzer, und der erforderliche Speicherplatz ist deutlich geringer als bei einem virtuellen Computer.
Windows-Container bieten für Apps unter Windows die gleichen Vorteile wie Linux-Container. Windows-Container unterstützen das Docker-Imageformat und die Docker-API, können jedoch auch mithilfe von PowerShell verwaltet werden. Für Windows-Container sind zwei Containerlaufzeiten verfügbar, Windows Server-Container und Hyper-V-Container. Hyper-V-Container bieten eine zusätzliche Isolationsebene, da jeder Container auf einem hoch optimierten virtuellen Computer gehostet wird. Weitere Informationen über Windows-Container finden Sie unter [About Windows Containers](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)(in englischer Sprache). Informieren Sie sich als Einstieg in Windows-Container unter Azure über die [Bereitstellung eines Azure Container Service-Clusters](/articles/container-service/container-service-deployment.md).

## <a name="what-are-containers-good-for"></a>Welche Vorteile bieten Container?

Container können Folgendes verbessern:

* Die Geschwindigkeit, mit der Anwendungscode entwickelt und umfassend freigegeben werden kann
* Die Geschwindigkeit und Zuverlässigkeit, mit der eine App getestet werden kann
* Die Geschwindigkeit und Zuverlässigkeit, mit der eine App bereitgestellt werden kann

Container werden auf einem Containerhost (also unter einem Betriebssystem) ausgeführt. In Azure bedeutet das: auf einem virtuellen Azure-Computer. Auch wenn das Konzept von Containern Sie schon überzeugt hat, benötigen Sie weiterhin eine VM-Infrastruktur, die die Container hostet. Der Vorteil ist jedoch, dass es nicht wichtig ist, auf welchem virtuellen Computer die Container ausgeführt werden (ob ein Container allerdings in einer Linux- oder in einer Windows-Ausführungsumgebung ausgeführt wird, ist zum Beispiel wichtig).


## <a name="what-are-containers-good-for"></a>Welche Vorteile bieten Container?
Sie eignen sich für vieles, fördern aber genau wie [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) und [Azure Service Fabric](../articles/service-fabric/service-fabric-overview.md) insbesondere die Erstellung von Microservice-orientierten verteilten Einzeldienstanwendungen, bei denen der Anwendungsentwurf mehr auf kleinen, kombinierbaren Einzelteilen und weniger auf größeren, stärker verknüpften Komponenten beruht.

Dies gilt insbesondere in öffentlichen Cloudumgebungen wie Azure, bei denen Sie virtuelle Computer nach Bedarf mieten. Nicht nur erhalten Sie Tools für Isolation, schnelle Bereitstellung und Orchestrierung, sondern Sie können auch effizientere Entscheidungen zur Anwendungsinfrastruktur treffen.

Beispielsweise besteht Ihre derzeitige Bereitstellung vielleicht aus 9 großen virtuellen Azure-Computern für eine verteilte Anwendung mit hoher Verfügbarkeit. Wenn die Komponenten dieser Anwendung in Containern bereitgestellt werden können, kann es möglich sein, dass Sie nur 4 virtuelle Computer verwenden und die Anwendungskomponenten innerhalb von 20 Containern für Redundanz und Lastenausgleich bereitstellen.

Dies ist natürlich nur ein Beispiel, aber wenn dies in Ihrem Szenario denkbar ist, können Sie Nutzungsspitzen mit mehr Containern anstelle von mehr virtuellen Azure-Computern anpassen und die verbleibende CPU-Gesamtauslastung wesentlich effizienter verwenden.

Außerdem gibt es viele Szenarios, die nicht für einen Microservices-Ansatz geeignet sind. Sie können selbst am besten einschätzen, ob Microservices und Container Ihnen helfen.

### <a name="container-benefits-for-developers"></a>Vorteile von Containern für Entwickler
Allgemein gesehen, ist es offenkundig, dass die Containertechnologie ein Schritt nach vorne ist, aber es gibt auch spezifischere Vorteile. Zur Veranschaulichung dient hier das Beispiel der Docker-Container. In diesem Thema wird Docker nicht ausführlich behandelt (lesen Sie hierzu [What is Docker?](https://www.docker.com/whatisdocker/) [Was ist Docker?] oder den entsprechenden Artikel bei [Wikipedia](http://wikipedia.org/wiki/Docker_%28software%29)). Docker und das zugehörige Ökosystem sind für Entwickler und IT-Experten aber mit erheblichen Vorteilen verbunden.

Entwickler sind schnell von Docker-Containern überzeugt, da diese insbesondere die Verwendung von Linux- und Windows-Containern vereinfachen:

* Sie können einfache, inkrementelle Befehle verwenden, um ein festes Image zu erstellen, das leicht bereitgestellt werden kann, und die Erstellung dieser Images mithilfe einer Docker-Datei automatisieren.
* Sie können diese Images problemlos über einfache Push- und Pull-Befehle im [git](https://git-scm.com/)-Format für [öffentliche](https://registry.hub.docker.com/) oder [private](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Docker-Registrierungen freigeben.
* Sie können sich auf isolierte Anwendungskomponenten anstelle von Computern konzentrieren.
* Sie können eine große Anzahl von Tools verwenden, die für Docker-Container und andere Basisimages geeignet sind.

### <a name="container-benefits-for-operations-and-it-professionals"></a>Vorteile von Containern für Betriebs- und IT-Experten
IT- und Betriebsexperten profitieren ebenfalls von der Kombination aus Containern und virtuellen Computern.

* Enthaltene Dienste sind von der Ausführungsumgebung des VM-Hosts isoliert.
* Enthaltener Code ist überprüfbar identisch.
* Enthaltene Dienste können schnell gestartet und beendet sowie zwischen Entwicklungs-, Test- und Produktionsumgebungen verschoben werden.

Funktionen wie diese und andere sind außerordentlich interessant für etablierte Unternehmen, deren IT-Abteilungen die Aufgabe zukommt, die Ressourcen – darunter auch die reine Verarbeitungsleistung – an den Bedarf anzupassen, um im Geschäft zu bleiben und gleichzeitig die Kundenzufriedenheit zu erhöhen und mehr Kunden zu erreichen. Die Anforderungen von kleinen Unternehmen, ISVs und Startups sind identisch, werden aber möglicherweise anders formuliert.

## <a name="what-are-virtual-machines-good-for"></a>Welche Vorteile bieten virtuelle Computer?
Virtuelle Computer sind die tragende Säule des Cloud Computing und das ist unumstößlich. Virtuelle Computer starten langsamer, haben einen größeren Datenträgerbedarf und sind nicht direkt einer Microservices-Architektur zugeordnet, haben aber dennoch sehr große Vorteile:

1. Ihre Standardsicherheitsmaßnahmen für Hostcomputer sind wesentlich robuster.
2. Sie unterstützen alle wichtigen Betriebssystem- und Anwendungskonfigurationen.
3. Sie verfügen über einen bewährten Bestand an Befehls- und Steuerungstools.
4. Sie stellen die Ausführungsumgebung für Hostcontainer bereit.

Der letzte Punkt ist wichtig, da eine enthaltene Anwendung weiterhin ein bestimmtes Betriebssystem und einen bestimmten CPU-Typ erfordert, abhängig von den Aufrufen, die die Anwendung vornimmt. Es ist wichtig zu beachten, dass Container auf virtuellen Computern installiert werden, da sie die Anwendungen enthalten, die Sie bereitstellen möchten. Container sind kein Ersatz für virtuelle Computer oder Betriebssystemen.

## <a name="high-level-feature-comparison-of-vms-and-containers"></a>Allgemeiner Funktionsvergleich zwischen virtuellen Computern und Containern
Die folgende Tabelle beschreibt ganz allgemein die Featureunterschiede, die – ohne großen Mehraufwand – zwischen virtuellen Computern und Linux-Containern bestehen. Beachten Sie, dass manche Features nicht so geeignet sind wie andere, abhängig von Ihren eigenen Anwendungsanforderungen, und dass zusätzliche Arbeit wie bei jeder Software mehr Funktionsunterstützung mit sich bringt, insbesondere im Bereich der Sicherheit.

| Funktion | VMs | Container |
|:--- | --- | --- |
| "Standardmäßige" Sicherheitsunterstützung |In höherem Maß |In etwas geringerem Maß |
| Speicher auf Datenträger erforderlich |Vollständiges Betriebssystem plus Apps |Nur App-Anforderungen |
| Startdauer |Wesentlich länger: Starten des Betriebssystems plus Laden der App |Wesentlich kürzer: Nur Apps müssen gestartet werden, da der Kernel bereits ausgeführt wird |
| Portabilität |Portabel bei richtiger Vorbereitung |Portable innerhalb des Imageformats; in der Regel kleiner |
| Imageautomatisierung |Variiert stark in Abhängigkeit von Betriebssystem und Apps |[Docker-Registrierung](https://registry.hub.docker.com/); andere |

## <a name="creating-and-managing-groups-of-vms-and-containers"></a>Erstellen und Verwalten von Gruppen von virtuellen Computern und Containern
Da das alles automatisiert werden kann, betrachten Architekten, Entwickler oder IT-Betriebsexperten dies an diesem Punkt möglicherweise als "Rechenzentrum-as-a-Service".

Dies kann durchaus zutreffen. Es gibt eine große Anzahl von Systemen, von denen Sie vielleicht viele schon nutzen, die Gruppen von Azure-VMs verwalten und benutzerdefinierten Code mithilfe von Skripts einfügen können. Hierfür wird häufig die [CustomScriptingExtension für Windows](https://msdn.microsoft.com/library/azure/dn781373.aspx) oder die [CustomScriptingExtension für Linux](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/) verwendet. Sie können Ihre Azure-Bereitstellungen mithilfe von PowerShell oder Azure-Befehlszeilenschnittstellen-Skripts automatisieren (und haben es vielleicht schon getan).

Diese Fähigkeiten werden dann oft zu Tools wie [Puppet](https://puppetlabs.com/) und [Chef](https://www.chef.io/) migriert, um die skalierte Erstellung und Konfiguration für virtuelle Computer zu automatisieren. (Links zur Verwendung dieser Tools mit Azure finden Sie [hier](#tools-for-working-with-containers).)

### <a name="azure-resource-group-templates"></a>Azure-Ressourcengruppenvorlagen
Vor Kurzem wurde die REST-API der [Azure-Ressourcenverwaltung](../articles/resource-manager-deployment-model.md) veröffentlicht, und die PowerShell- und Azure-Befehlszeilenschnittstellen-Tools wurden aktualisiert, um die Verwendung zu erleichtern. Sie können komplette Anwendungstopologien bereitstellen, ändern oder erneut bereitstellen, indem Sie die [Azure Resource Manager-Vorlagen](../articles/resource-group-authoring-templates.md) mit der Azure-Ressourcenverwaltungs-API verwenden. Nutzen Sie hierbei Folgendes:

* Das [Azure-Portal mit Vorlagen](https://github.com/Azure/azure-quickstart-templates) (Tipp: Verwenden Sie die Schaltfläche „DeployToAzure“.)
* Die [Azure-Befehlszeilenschnittstelle](../articles/virtual-machines/linux/cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Die [Azure PowerShell-Module](../articles/virtual-machines/linux/cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="deployment-and-management-of-entire-groups-of-azure-vms-and-containers"></a>Bereitstellung und Verwaltung vollständiger Gruppen von Azure-VMs und Containern
Es gibt mehrere beliebte Systeme, die ganze Gruppen von virtuellen Computern bereitstellen und auf diesen Docker (oder andere Linux-Containerhostsysteme) als automatisierbare Gruppe installieren können. Direkte Links finden Sie weiter unten im Abschnitt [Container und Tools](#containers-and-vm-technologies). Es gibt eine Reihe von Systemen, die dies in größerem oder weniger großem Umfang können. Diese Liste ist nicht vollständig. Wie nützlich sie sind, hängt von Ihren Kenntnissen und Szenarios ab.

Docker verfügt über einen eigenen Satz von Tools zur Erstellung von virtuellen Computern ([docker-machine](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) und über ein Clusterverwaltungstool für Lastenausgleich und Docker-Container ([swarm](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Darüber hinaus bietet die [Azure VM-Erweiterung für Docker](https://github.com/Azure/azure-docker-extension/blob/master/README.md) standardmäßig Unterstützung für [`docker-compose`](https://docs.docker.com/compose/), womit konfigurierte Anwendungscontainer über mehrere Container hinweg bereitgestellt werden können.

Außerdem können Sie auch [Data Center Operating System (DCOS) von Mesosphere](http://docs.mesosphere.com/install/azurecluster/)testen. DCOS basiert auf dem "Open-Source-Kernel für verteilte Systeme" [mesos](http://mesos.apache.org/) , der es Ihnen ermöglicht, Ihr Rechenzentrum als einen adressierbaren Dienst zu behandeln. DCOS verfügt über integrierte Pakete für verschiedene wichtige Systeme wie [Spark](http://spark.apache.org/) und [Kafka](http://kafka.apache.org/) (und andere) sowie über integrierte Dienste wie [Marathon](https://mesosphere.github.io/marathon/) (ein Containersteuerungssystem) und [Chronos](https://mesos.github.io/chronos/) (ein verteilter Scheduler). Mesos entstand auf Basis von Erfahrungen mit Twitter, AirBnb und anderen im Web operierenden Unternehmen. Sie können auch **swarm** als Orchestrierungsmodul verwenden.

Darüber hinaus ist [kubernetes](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/) ein Open-Source-System für die VM- und Containergruppenverwaltung, das auf Grundlage der gewonnenen Erkenntnisse bei Google entstand. Möglich ist auch die [Verwendung von kubernetes mit weave, um Netzwerkunterstützung bereitzustellen](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave).

[Deis](http://deis.io/overview/) ist eine Open-Source-"Plattform-as-a-Service" (PaaS), die das Bereitstellen und Verwalten von Anwendungen auf Ihren eigenen Servern erleichtert. Deis baut auf Docker und CoreOS auf, um eine einfache PaaS mit einem durch Heroku inspirierten Workflow bereitzustellen. Sie können in Azure auf einfache Weise [eine Azure-VM-Gruppe mit drei Knoten erstellen und Deis installieren](../articles/virtual-machines/linux/deis-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) und dann [eine Hello World-Go-Anwendung installieren](../articles/virtual-machines/linux/deis-cluster.md#deploy-and-scale-a-hello-world-application).

[CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html), eine Linux-Distribution mit optimiertem Ressourcenbedarf, Docker-Unterstützung und einem eigenen Containersystem namens [rkt](https://github.com/coreos/rkt) verfügt ebenfalls über ein Verwaltungstool für Containergruppen namens [fleet](https://coreos.com/using-coreos/clustering/).

Ubuntu, eine weitere sehr populäre Linux-Distribution, unterstützt Docker sehr gut, aber auch [Linux-Cluster (LXC-Format)](https://help.ubuntu.com/lts/serverguide/lxc.html).

## <a name="tools-for-working-with-azure-vms-and-containers"></a>Tools für das Arbeiten mit Azure-VMs und Containern
Für die Arbeit mit Containern und Azure-VMs sind Tools erforderlich. Dieser Abschnitt enthält eine Liste einiger der nützlichsten oder wichtigsten Konzepte und Tools für Container und Gruppen sowie der umfassenderen Konfigurations- und Orchestrierungstools, die mit ihnen verwendet werden.

> [!NOTE]
> Dieser Bereich ist schnellen Veränderungen unterworfen und obwohl wir stets intensiv daran arbeiten, dieses Thema und die zugehörigen Links auf dem aktuellen Stand zu halten, ist dies vielleicht nicht immer möglich. Recherchieren Sie zu Themen, die Sie interessieren, um auf dem Laufenden zu bleiben.
>
>

### <a name="containers-and-vm-technologies"></a>Container- und VM-Technologien
Einige Technologien für Linux-Container:

* [Docker](https://www.docker.com)
* [LXC](https://linuxcontainers.org/)
* [CoreOS und rkt](https://github.com/coreos/rkt)
* [Open Container-Projekt](http://opencontainers.org/)
* [RancherOS](http://rancher.com/rancher-os/)

Links zu Windows-Containern:

* [Windows Containers (in englischer Sprache)](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)

Links zu Visual Studio-Docker:

* [Visual Studio 2015 RC-Tools für Docker - Vorschau](https://visualstudiogallery.msdn.microsoft.com/6f638067-027d-4817-bcc7-aa94163338f0)

Docker-Tools:

* [Docker-Daemon](https://docs.docker.com/installation/#installation)
* Docker-Clients
  * [Windows-Docker-Client auf Chocolatey](https://chocolatey.org/packages/docker)
  * [Docker-Installationsanweisungen](https://docs.docker.com/installation/#installation)

Docker in Microsoft Azure:

* [Docker-VM-Erweiterung für Linux auf Azure](../articles/virtual-machines/linux/dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Benutzerhandbuch für die Azure-Docker-VM-Erweiterung](https://github.com/Azure/azure-docker-extension/blob/master/README.md)
* [Verwenden der Docker-VM-Erweiterung aus der Azure-Befehlszeilenschnittstelle (Azure-CLI)](../articles/virtual-machines/linux/classic/cli-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Verwenden der Docker-VM-Erweiterung über das Azure-Portal](../articles/virtual-machines/linux/classic/portal-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Verwenden von "docker-machine" auf Azure](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Verwenden von Docker mit Swarm auf Azure](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erste Schritte mit Docker und Compose auf Azure](../articles/virtual-machines/linux/docker-compose-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Using an Azure resource group template to create a Docker host on Azure quickly ("Verwenden einer Azure-Ressourcengruppenvorlage zur schnellen Erstellung eines Docker-Hosts in Azure", in englischer Sprache)](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)
* [Integrierte Unterstützung für `compose`](https://github.com/Azure/azure-docker-extension#11-public-configuration-keys) für enthaltene Anwendungen
* [Implementieren einer privaten Docker-Registrierung für Azure](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Linux-Distributionen und Azure-Beispiele:

* [CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html)

Konfiguration, Clusterverwaltung und Containerorchestrierung:

* [Fleet in CoreOS](https://coreos.com/using-coreos/clustering/)
* Deis

  * [Erstellen einer Gruppe von virtuellen Azure-Computern mit drei Knoten, Installieren von Deis und Starten einer Hello World-Go-Anwendung](../articles/virtual-machines/linux/deis-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* kubernetes

  * [Vollständige Anleitung für die automatisierte Kubernetes- Clusterbereitstellung mit CoreOS und Weave](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
  * [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)
* [Mesos](http://mesos.apache.org/)

  * [Data Center Operating System (DCOS) von Mesosphere](http://beta-docs.mesosphere.com/install/azurecluster/)
* [Jenkins](https://jenkins-ci.org/) und [Hudson](http://hudson-ci.org/)

  * [Blog: Jenkins Slave-Plug-In für Azure](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
  * [GitHub-Repository: Jenkins Speicher-Plug-In für Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)
  * [Drittanbieter: Hudson Slave-Plug-In für Azure](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
  * [Drittanbieter: Hudson Speicher-Plug-In für Azure](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
* [Azure Automation](https://azure.microsoft.com/services/automation/)

  * [Video: How to Use Azure Automation with Linux VMs (Video: Verwenden von Azure Automation mit virtuellen Linux-Computern, in englischer Sprache)](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)
* Powershell DSC für Linux

  * [Blog: Powershell DSC für Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
  * [GitHub: Docker Client DSC](https://github.com/anweiss/DockerClientDSC)

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich über [Docker](https://www.docker.com) und [Windows-Container](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview).

<!--Anchors-->
[microservices]: http://martinfowler.com/articles/microservices.html
[microservice]: http://martinfowler.com/articles/microservices.html
<!--Image references-->
