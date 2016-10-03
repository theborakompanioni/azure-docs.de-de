 <properties
   pageTitle="Azure und Linux | Microsoft Azure"
   description="Beschreibt die Azure Compute-, Speicher- und Netzwerkdienste in Bezug auf virtuelle Linux-Computer."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines-linux"
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/14/2016"
   ms.author="v-livech"/>

# Azure und Linux

Microsoft Azure umfasst eine wachsende Sammlung von integrierten Public Cloud-Diensten, z.B. Analyse, Virtual Machines, Datenbanken, mobile Geräte, Netzwerke, Speicher und Web, und ist somit ideal für das Hosten Ihrer Lösungen geeignet. Microsoft Azure stellt eine skalierbare Computingplattform bereit, bei der Sie nur für die tatsächliche gewünschte Nutzung bezahlen – ohne dass Sie in lokale Hardware investieren müssen. Azure ist darauf ausgelegt, dass Sie Ihre Lösungen wie gewünscht vertikal und horizontal auf den Stand hochskalieren, der für die Erfüllung der Anforderungen Ihrer Kunden erforderlich ist.

Wenn Sie mit den Funktionen der verschiedenen Amazon Web Services (AWS) vertraut sind, sehen Sie sich den [Vergleich der Dienste von Azure und AWS](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/) an.


## Regionen
Microsoft Azure-Ressourcen sind auf mehrere geografische Regionen weltweit verteilt. Eine „Region“ umfasst mehrere Rechenzentren in einem bestimmten geografischen Bereich. Ab 1. Januar 2016 gilt folgender Stand: 8 in Amerika, 2 in Europa, 6 in der Asien-Pazifik-Region, 2 in China (Festland) und 3 in Indien. Falls Sie eine vollständige Liste mit allen Azure-Regionen benötigen, können wir Ihnen eine Liste mit den vorhandenen und neu angekündigten Regionen zur Verfügung stellen.

- [Azure-Regionen](https://azure.microsoft.com/regions/)

## Availability
Damit Ihre Bereitstellung die Qualifikation für unsere VM-Vereinbarung zum Servicelevel von 99,95% erreicht, müssen Sie mindestens zwei VMs bereitstellen, die Ihre Workload innerhalb einer Verfügbarkeitsgruppe ausführen. So wird sichergestellt, dass Ihre VMs auf mehrere Fehlerdomänen in unseren Rechenzentren verteilt und auf Hosts mit unterschiedlichen Wartungsfenstern bereitgestellt werden. Die vollständige [Azure-SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) erläutert die garantierte Verfügbarkeit von Azure insgesamt.

## Virtuelle Azure-Computer und -Instanzen
Microsoft Azure unterstützt die Ausführung einer Reihe von beliebten Linux-Distributionen, die von verschiedenen Partnerunternehmen bereitgestellt und gepflegt werden. Im Azure Marketplace finden Sie Distributionen wie Red Hat Enterprise, CentOS, Debian, Ubuntu, CoreOS, RancherOS, FreeBSD und viele weitere. Wir arbeiten aktiv mit verschiedenen Linux-Communitys zusammen, um der Liste der [von Azure unterstützten Linux-Distributionen](virtual-machines-linux-endorsed-distros.md) weitere Varianten hinzuzufügen.

Falls Ihre bevorzugte Linux-Distribution derzeit nicht im Katalog enthalten ist, können Sie den Ansatz „Bring your own Linux VM“ verfolgen, indem Sie eine [Linux-VHD erstellen und in Azure hochladen](virtual-machines-linux-create-upload-generic.md).

Mit Azure Virtual Machines können Sie sehr flexibel eine Vielzahl unterschiedlicher Computinglösungen bereitstellen. Sie können praktisch jede Workload und jede Sprache bereitstellen, und das auf fast jedem Betriebssystem – Windows, Linux oder einem benutzerdefiniert erstellten, das von einem unserer immer zahlreicher werdenden Partner stammt. Werden Ihre Anforderungen hiermit immer noch nicht erfüllt? Keine Sorge. Sie können auch eigene Images von Ihrem lokalen Standort verwenden.

## VM-Größen
Wenn Sie einen virtuellen Computer in Azure bereitstellen, wählen Sie eine unserer verfügbaren VM-Größen aus, die für Ihre Workload geeignet ist. Die Größe beeinflusst auch die Verarbeitungsleistung, den Arbeitsspeicher und die Speicherkapazität des virtuellen Computers. Die Kosten werden Ihnen basierend auf der Ausführungsdauer der VM und der Nutzung der zugeordneten Ressourcen berechnet. Hier finden Sie eine vollständige Liste der [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).

Hier sind einige grundlegende Richtlinien zur Auswahl einer VM-Größe aus einer unserer Serien (A, D, DS, G und GS) aufgeführt.

* VMs der A-Serie sind unsere kostengünstigen Einsteiger-VMs für kleine Workloads und Entwicklungs- und Testszenarien. Sie sind in allen Regionen verfügbar und ermöglichen die Verbindung mit und Nutzung aller Standardressourcen für virtuelle Computer.
* Bei den Größen der A-Serie (A8 - A11) handelt es sich um spezielle rechenintensive Konfigurationen, die für Computing-Clusteranwendungen mit hoher Leistung geeignet sind.
* VMs der D-Serie dienen zum Ausführen von Anwendungen, die eine höhere Rechenleistung und eine höhere temporäre Datenträgerleistung erfordern. VMs der D-Serie bieten schnellere Prozessoren, ein höheres Verhältnis von Speicher zu Kern und ein SSD (Solid State Drive) für den temporären Datenträger.
* Die Dv2-Serie ist die neuste Version unserer D-Serie und verfügt über eine leistungsfähigere CPU. Die CPU der Dv2-Serie ist ca. 35 % schneller als die CPU der D-Serie. Sie basiert auf der neuesten Generation des Intel Xeon® E5-2673 v3-Prozessors mit 2,4 GHz (Haskell) und kann mit der Intel Turbo Boost Technology 2.0 bis auf 3,2 GHz erhöht werden. Die Dv2-Serie hat die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die D-Serie.
* Virtuelle Computer der G-Serie bieten den meisten Arbeitsspeicher und werden auf Hosts mit Prozessoren der Intel Xeon E5 V3-Familie ausgeführt.

Hinweis: Für VMs der DS-Serie und GS-Serie besteht Zugriff auf Storage Premium, unsere Speicherlösung mit SSD-Unterstützung, hoher Leistung und geringer Latenz für Workloads mit hoher E/A-Intensität. Premium-Speicher ist in bestimmten Regionen verfügbar. Einzelheiten dazu finden Sie hier:

- [Storage Premium: Hochleistungsspeicher für Workloads auf virtuellen Azure-Computern](../storage/storage-premium-storage.md)

## Automation
Um eine sinnvolle DevOps-Kultur zu erzielen, muss die gesamte Infrastruktur als Code definiert sein. Wenn die Infrastruktur vollständig im Codeformat vorliegt, kann sie problemlos neu erstellt werden (Phoenix-Server). Azure arbeitet mit allen wichtigen Automatisierungstools zusammen, wie z.B. Ansible, Chef, SaltStack und Puppet. Azure stellt auch eigene Tools für die Automatisierung bereit:

- [Azure-Vorlagen](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

- [Azure VMAccess](virtual-machines-linux-using-vmaccess-extension.md)

Azure führt Unterstützung für [cloud-init](http://cloud-init.io/) für die meisten Linux-Distributionen ein, die dies unterstützen. Zurzeit werden die Ubuntu-VMs von Canonical mit standardmäßig aktiviertem cloud-init bereitgestellt. RHEL, CentOS und Fedora von Red Hat unterstützen cloud-init, auf den von Red Hat verwalteten Azure-Images ist cloud-init jedoch nicht installiert. Um cloud-init unter einem Betriebssystem der Red Hat-Familie zu verwenden, müssen Sie ein benutzerdefiniertes Image mit installiertem cloud-init erstellen.

- [Verwenden von cloud-init auf virtuellen Linux-Computern in Azure](virtual-machines-linux-using-cloud-init.md)

## Kontingente
Jedes Azure-Abonnement verfügt über standardmäßige Kontingentgrenzen, durch die die Bereitstellung einer großen Anzahl von virtuellen Computern für Ihr Projekt beeinträchtigt werden kann. Der derzeitige Grenzwert pro Abonnement liegt bei 20 VMs pro Region. Sie können die Kontingentgrenzen erhöhen lassen, indem Sie ein Supportticket erstellen und eine Erhöhung anfordern. Weitere Informationen zu Kontingentgrenzen finden Sie hier:

- [Einschränkungen für Azure-Abonnementdienste](../azure-subscription-service-limits.md)


## Partner

Microsoft arbeitet eng mit Partnern zusammen, um sicherzustellen, dass die verfügbaren Images für die Azure-Laufzeit aktualisiert und optimiert sind. Weitere Informationen zu unseren Partnern finden Sie auf den jeweiligen Marketplace-Seiten unten.

- [Linux auf von Azure unterstützten Verteilungen](virtual-machines-linux-endorsed-distros.md)

Red Hat: [Azure Marketplace – Red Hat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)

Canonical: [Azure Marketplace – Ubuntu Server 16.04 LTS](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)

Debian: [Azure Marketplace – Debian 8 „Jessie“](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)

FreeBSD: [Azure Marketplace – FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)

CoreOS: [Azure Marketplace – CoreOS (Stable)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)

RancherOS: [Azure Marketplace – RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)

Bitnami: [Bitnami-Bibliothek für Azure](https://azure.bitnami.com/)

Mesosphere: [Azure Marketplace – Mesosphere DC/OS on Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)

Docker: [Azure Marketplace – Azure Container Service mit Docker Swarm](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)

Jenkins: [Azure Marketplace – CloudBees Jenkins Platform](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)


## Erste Schritte mit Azure
Um mit der Verwendung von Azure zu beginnen, benötigen Sie ein Azure-Konto, die installierte Azure-Befehlszeilenschnittstelle und ein Paar aus öffentlichen und privaten SSH-Schlüsseln.

## Registrieren für ein Konto
Der erste Schritt zur Verwendung der Azure-Cloud besteht darin, sich für ein Azure-Konto zu registrieren. Besuchen Sie die Seite [Erstellen eines kostenlosen Azure-Kontos](https://azure.microsoft.com/pricing/free-trial/), um zu beginnen.

## Installieren der Befehlszeilenschnittstelle
Mit Ihrem neuen Azure-Konto können Sie sofort mit dem Azure-Portal loslegen, einem webbasierten Verwaltungsbereich. Um die Azure-Cloud über die Befehlszeile zu verwalten, installieren Sie die `azure-cli`. Installieren Sie die [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) auf Ihrem Mac oder Ihrer Linux-Arbeitsstation.

## Erstellen eines SSH-Schlüsselpaars
Jetzt verfügen Sie über ein Azure-Konto, das Azure-Webportal und die Azure-Befehlszeilenschnittstelle. Der nächste Schritt besteht darin, ein SSH-Schlüsselpaar zu erstellen, um über SSH eine Verbindung mit Linux herzustellen, ohne ein Kennwort zu verwenden. [Erstellen Sie SSH-Schlüssel unter Linux und Mac](virtual-machines-linux-mac-create-ssh-keys.md), um Anmeldungen ohne Kennwort und eine höhere Sicherheit zu ermöglichen.

## Erste Schritte mit Linux unter Microsoft Azure
Nachdem Sie Ihr Azure-Konto eingerichtet, die Azure-Befehlszeilenschnittstelle installiert und SSH-Schlüssel erstellt haben, sind Sie jetzt bereit, eine Infrastruktur in der Azure-Cloud aufzubauen. Die erste Aufgabe besteht darin, einige virtuelle Computer zu erstellen.

## Erstellen eines virtuellen Computers über die Befehlszeilenschnittstelle
Die Erstellung eines virtuellen Linux-Computers über die Befehlszeilenschnittstelle ist eine schnelle Möglichkeit, einen virtuellen Computer bereitzustellen, ohne das Terminal zu verlassen, an dem Sie gerade arbeiten. Alle Elemente, die Sie im Webportal festlegen können, stehen auch über ein Flag bzw. eine Option in der Befehlszeile zur Verfügung.

- [Erstellen eines virtuellen Linux-Computers über die Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli.md)

## Erstellen eines virtuellen Computers im Portal
Die Erstellung eines virtuellen Linux-Computers im Azure-Webportal ist eine einfache Möglichkeit, per Klick auf verschiedene Optionen zu einer Bereitstellung zu gelangen. Anstatt manuell Befehlszeilenflags oder -optionen einzugeben, können Sie die verschiedenen Optionen und Einstellungen in einem benutzerfreundlichen Weblayout anzeigen. Alle über die Befehlszeilenschnittstelle verfügbaren Optionen stehen auch im Portal zur Verfügung.

- [Erstellen eines virtuellen Linux-Computers mithilfe des Portals](virtual-machines-linux-quick-create-portal.md)

## Anmelden ohne Kennwort über SSH
Der virtuelle Computer wird jetzt in Azure ausgeführt, und Sie können sich anmelden. Die Verwendung von Kennwörtern zur Anmeldung über SSH ist unsicher und zeitaufwendig. SSH-Schlüssel sind die sicherste und gleichzeitig schnellste Möglichkeit zur Anmeldung. Beim Erstellen eines virtuellen Linux-Computers im Portal oder über die Befehlszeilenschnittstelle stehen Ihnen zwei Authentifizierungsoptionen zur Verfügung. Wenn Sie ein Kennwort für SSH auswählen, konfiguriert Azure den virtuellen Computer so, dass Anmeldungen mit Kennwort zulässig sind. Wenn Sie sich für einen öffentlichen SSH-Schlüssel entschieden haben, konfiguriert Azure den virtuellen Computer so, dass Anmeldungen nur mit SSH-Schlüsseln möglich sind. Anmeldungen per Kennwort werden deaktiviert. Um Ihren virtuellen Linux-Computer zu sichern, indem Sie nur Anmeldungen per SSH-Schlüssel zulassen, verwenden Sie beim Erstellen des virtuellen Computers im Portal oder über die Befehlszeilenschnittstelle die Option für öffentliche SSH-Schlüssel.

- [Deaktivieren von SSH-Kennwörtern auf Ihrer Linux-VM durch Konfigurieren von SSHD](virtual-machines-linux-mac-disable-ssh-password-usage.md)

## Verwandte Azure-Komponenten

## Speicher

- [Einführung in Microsoft Azure Storage](../storage/storage-introduction.md)

- [Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer über die Azure-Befehlszeilenschnittstelle](virtual-machines-linux-add-disk.md)

- [Anfügen eines Datenträgers an eine Linux-VM im Azure-Portal ](virtual-machines-linux-attach-disk-portal.md)

## Netzwerk

- [Virtuelle Netzwerke im Überblick](../virtual-network/virtual-networks-overview.md)

- [IP-Adressen in Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md)

- [Öffnen von Ports für eine Linux-VM in Azure](virtual-machines-linux-nsg-quickstart.md)

- [Erstellen eines vollständig qualifizierten Domänennamens im Azure-Portal](virtual-machines-linux-portal-create-fqdn.md)


## Container

- [Virtuelle Computer und Container in Azure](virtual-machines-linux-containers.md)

- [Einführung in Azure Container Service](../container-service/container-service-intro.md)

- [Bereitstellen eines Azure Container Service-Clusters](../container-service/container-service-deployment.md)

## Nächste Schritte

Sie haben nun einen Überblick über die Verwendung von Linux in Azure. Jetzt können Sie richtig loslegen und weitere virtuelle Computer erstellen!

- [Erstellen eines virtuellen Linux-Computers in Azure mithilfe des Portals](virtual-machines-linux-quick-create-portal.md)

- [Erstellen einer Linux-VM in Azure mithilfe der CLI](virtual-machines-linux-quick-create-cli.md)

<!---HONumber=AcomDC_0921_2016-->