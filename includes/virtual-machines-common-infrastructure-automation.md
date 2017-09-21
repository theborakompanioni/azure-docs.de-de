# <a name="use-infrastructure-automation-tools-with-virtual-machines-in-azure"></a>Verwenden von Infrastrukturautomatisierungstools mit virtuellen Computern in Azure
Für die umfangreiche konsistente Erstellung und Verwaltung virtueller Azure-Computer (Virtual Machines, VMs) empfiehlt sich in der Regel eine Form der Automatisierung. Es gibt zahlreiche Tools und Lösungen, mit denen Sie die gesamte Azure-Infrastrukturbereitstellung und den gesamten Verwaltungslebenszyklus automatisieren können. In diesem Artikel werden einige der Infrastrukturautomatisierungstools vorgestellt, die Sie in Azure verwenden können. Diese Tools lassen sich in der Regel einem der folgenden Konzepte zuordnen:

- Automatisieren der Konfiguration virtueller Computer
    - Zur Verfügung stehen Tools wie [Ansible](#ansible), [Chef](#chef) und [Puppet](#puppet).
    - Spezifische Tools für die Anpassung virtueller Computer sind [cloud-init](#cloud-init) für virtuelle Linux-Computer sowie [PowerShell DSC (Desired State Configuration)](#powershell-dsc) und die [benutzerdefinierte Skripterweiterung von Azure](#azure-custom-script-extension) für alle virtuellen Azure-Computer.
 
- Automatisieren der Infrastrukturverwaltung
    - Zur Verfügung stehen Tools wie [Packer](#packer) zum Automatisieren benutzerdefinierter VM-Image-Builds und [Terraform](#terraform) zum Automatisieren des Infrastrukturbuildprozesses.
    - [Azure Automation](#azure-automation) kann Aktionen für Ihre Azure-Infrastruktur und Ihre lokale Infrastruktur ausführen.

- Automatisieren der Anwendungsbereitstellung und -übermittlung
    - Beispiele hierfür sind [Visual Studio Team Services](#visual-studio-team-services) und [Jenkins](#jenkins).


## <a name="ansible"></a>Ansible
[Ansible](https://www.ansible.com/) ist ein Automatisierungsmodul für die Konfigurationsverwaltung, VM-Erstellung oder Anwendungsbereitstellung. Für die Authentifizierung und Verwaltung der Zielcomputer kommt bei Ansible ein Agent-loses Modell (üblicherweise mit SSH-Schlüsseln) zum Einsatz. Konfigurationsaufgaben werden in Runbooks definiert. Dabei steht eine Reihe von Ansible-Modulen zur Ausführung bestimmter Aufgaben zur Verfügung. Weitere Informationen finden Sie unter [How Ansible works](https://www.ansible.com/how-ansible-works) (Funktionsweise von Ansible).

In diesem Artikel werden folgende Themen erläutert:

- [Installieren und Konfigurieren von Ansible unter Linux für die Verwendung mit Azure](../articles/virtual-machines/linux/ansible-install-configure.md)
- [Erstellen eines einfachen virtuellen Computers](../articles/virtual-machines/linux/ansible-create-vm.md)
- [Erstellen einer vollständigen VM-Umgebung einschließlich unterstützender Ressourcen](../articles/virtual-machines/linux/ansible-create-complete-vm.md)


## <a name="chef"></a>Chef
[Chef](https://www.chef.io/) ist eine Automatisierungsplattform, mit der Sie die Konfiguration, Bereitstellung und Verwaltung Ihrer Infrastruktur definieren können. Zusätzlich verfügbare Komponenten sind Chef Habitat für die Automatisierung des Anwendungslebenszyklus (anstelle der Infrastruktur) und Chef InSpec zur Automatisierung der Einhaltung von Sicherheits- und Richtlinienanforderungen. Auf den Zielcomputern werden Chef-Clients installiert, und es ist mindestens ein zentraler Chef-Server zum Speichern und Verwalten der Konfigurationen vorhanden. Weitere Informationen finden Sie in der [Übersicht über Chef](https://docs.chef.io/chef_overview.html).

In diesem Artikel werden folgende Themen erläutert:

- [Bereitstellen von Chef Automate über den Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/chef-software.chef-automate?tab=Overview)
- [Installieren von Chef unter Windows und Erstellen virtueller Azure-Computer](../articles/virtual-machines/windows/chef-automation.md)


## <a name="puppet"></a>Puppet
[Puppet](https://www.puppet.com) ist eine für Unternehmen geeignete Automatisierungsplattform zur Anwendungsübermittlung und -bereitstellung. Auf den Zielcomputern werden Agents installiert, um Puppet Master das Ausführen von Manifesten zu ermöglichen, die die gewünschte Konfiguration der Azure-Infrastruktur und der virtuellen Computer definieren. Zur Optimierung des DevOps-Workflows kann Puppet mit anderen Lösungen wie Jenkins und GitHub kombiniert werden. Weitere Informationen finden Sie unter [How Puppet works](https://puppet.com/product/how-puppet-works) (Funktionsweise von Puppet).

In diesem Artikel werden folgende Themen erläutert:

- [Bereitstellen von Puppet über den Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/puppet.puppet-enterprise-2016-1?tab=Overview)


## <a name="cloud-init"></a>cloud-init
[Cloud-init](https://cloudinit.readthedocs.io) ist ein weit verbreiteter Ansatz zum Anpassen einer Linux-VM beim ersten Start. Sie können mit cloud-init Pakete installieren und Dateien schreiben oder Benutzer und Sicherheit konfigurieren. Da cloud-init während des ersten Startvorgangs ausgeführt wird, müssen Sie keine zusätzlichen Schritte oder erforderlichen Agents auf Ihre Konfiguration anwenden.

Cloud-init funktioniert auch Distributionen übergreifend. Verwenden Sie z.B. nicht **apt-get install** oder **yum install**, um ein Paket zu installieren. Stattdessen definieren Sie eine Liste mit zu installierenden Paketen. „cloud-init“ verwendet automatisch das native Paketverwaltungstool für die ausgewählte Distribution.

Wir arbeiten mit unseren Partnern zusammen, damit cloud-init einbezogen wird und in den Images arbeitet, die sie in Azure bereitstellen. In der folgenden Tabelle ist die aktuelle cloud-init-Verfügbarkeit für Azure-Plattformimages aufgeführt:

| Alias | Herausgeber | Angebot | SKU | Version |
|:--- |:--- |:--- |:--- |:--- |:--- |
| UbuntuLTS |Canonical |UbuntuServer |14.04.5-LTS |neueste |
| UbuntuLTS |Canonical |UbuntuServer |16.04-LTS |neueste |
| CoreOS |CoreOS |CoreOS |Stable |neueste |

In diesem Artikel werden folgende Themen erläutert:

- [Anpassen eines virtuellen Linux-Computers mit cloud-Init](../articles/virtual-machines/linux/tutorial-automate-vm-deployment.md)


## <a name="powershell-dsc"></a>PowerShell DSC
[PowerShell DSC (Desired State Configuration)](https://msdn.microsoft.com/en-us/powershell/dsc/overview) ist eine Verwaltungsplattform zum Definieren der Konfiguration von Zielcomputern. DSC kann über den [OMI-Server (Open Management Infrastructure)](https://collaboration.opengroup.org/omi/) auch unter Linux verwendet werden.

DSC-Konfigurationen definieren, was auf einem Computer installiert und wie der Host konfiguriert werden soll. Ein LCM-Modul (Local Configuration Manager) wird auf jedem Zielknoten ausgeführt, der angeforderte Aktionen auf der Grundlage gepushter Konfigurationen verarbeitet. Ein Pull-Server ist ein Webdienst, der auf einem zentralen Host ausgeführt wird, um die DSC-Konfigurationen und die dazugehörigen Ressourcen zu speichern. Der Pull-Server kommuniziert mit dem LCM-Modul auf den einzelnen Zielhosts, um die erforderlichen Konfigurationen bereitzustellen und Complianceberichte zu erstellen.

In diesem Artikel werden folgende Themen erläutert:

- [Erstellen einer einfachen DSC-Konfiguration](https://msdn.microsoft.com/powershell/dsc/quickstart)
- [Konfigurieren eines DSC-Pull-Servers](https://msdn.microsoft.com/powershell/dsc/pullserver)
- [Verwenden von DSC für Linux](https://msdn.microsoft.com/powershell/dsc/lnxgettingstarted)


## <a name="azure-custom-script-extension"></a>Benutzerdefinierte Skripterweiterung von Azure
Die benutzerdefinierte Skripterweiterung von Azure (verfügbar für [Linux](../articles/virtual-machines/linux/extensions-customscript.md) und [Windows](../articles/virtual-machines/windows/extensions-customscript.md)) lädt Skripts auf virtuelle Azure-Computer herunter und führt sie dort aus. Die Erweiterung kann beim Erstellen eines virtuellen Computers oder zu einem beliebigen Zeitpunkt für einen verwendeten virtuellen Computer verwendet werden. 

Skripts können aus Azure-Speicher oder von einem anderen öffentlichen Speicherort (etwa einem GitHub-Repository) heruntergeladen werden. Mit der benutzerdefinierten Skripterweiterung können Sie Skripts in einer beliebigen Sprache schreiben, die auf dem virtuellen Quellcomputer ausgeführt werden kann. Mit diesen Skripts können Sie Anwendungen installieren oder den virtuellen Computer nach Ihren Vorstellungen konfigurieren. Zum Schutz von Anmeldeinformationen können vertrauliche Informationen wie etwa Kennwörter in einer geschützten Konfiguration gespeichert werden. Diese Anmeldeinformationen werden erst auf dem virtuellen Computer entschlüsselt.

In diesem Artikel werden folgende Themen erläutert:

- [Erstellen eines virtuellen Linux-Computers mit der Azure-Befehlszeilenschnittstelle und Verwenden der benutzerdefinierten Skripterweiterung](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fcli%2fazure%2ftoc.json)
- [Erstellen eines virtuellen Windows-Computers mit Azure PowerShell und Verwenden der benutzerdefinierten Skripterweiterung](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json)


## <a name="packer"></a>Packer
[Packer](https://www.packer.io) automatisiert den Buildprozess beim Erstellen eines benutzerdefinierten VM-Images in Azure. Mit Packer können Sie das Betriebssystem definieren und Skripts nach der Konfiguration ausführen, um den virtuellen Computer für Ihre speziellen Anforderungen anzupassen. Der konfigurierte virtuelle Computer wird dann als verwaltetes Datenträgerimage erfasst. Packer automatisiert das Erstellen des virtuellen Quellcomputers sowie der Netzwerk- und Speicherressourcen, das Ausführen der Konfigurationsskripts und das anschließende Erstellen des VM-Images.

In diesem Artikel werden folgende Themen erläutert:

- [Erstellen eines Linux-VM-Images in Azure mithilfe von Packer](../articles/virtual-machines/linux/build-image-with-packer.md)
- [Erstellen eines Windows-VM-Images in Azure mithilfe von Packer](../articles/virtual-machines/windows/build-image-with-packer.md)


## <a name="terraform"></a>Terraform
[Terraform](https://www.terraform.io) ist ein Automatisierungstool, mit dem Sie eine gesamte Azure-Infrastruktur mit einer einzelnen Vorlagenformatsprache – der HashiCorp Configuration Language (HCL) – definieren und erstellen können. Mit Terraform definieren Sie Vorlagen, die die Erstellung von Netzwerk-, Speicher- und VM-Ressourcen für eine bestimmte Anwendungslösung automatisieren. Ihre vorhandenen Terraform-Vorlagen können Sie für andere Plattformen mit Azure verwenden, um die Konsistenz zu gewährleisten und die Infrastrukturbereitstellung ohne Umwandlung in eine Azure Resource Manager-Vorlage zu vereinfachen.

In diesem Artikel werden folgende Themen erläutert:

- [Installieren und Konfigurieren von Terraform mit Azure](../articles/virtual-machines/linux/terraform-install-configure.md)
- [Erstellen einer Azure-Infrastruktur mit Terraform](../articles/virtual-machines/linux/terraform-create-complete-vm.md)


## <a name="azure-automation"></a>Azure Automation
[Azure Automation](https://azure.microsoft.com/services/automation/) verwendet Runbooks, um eine Reihe von Aufgaben auf Ihren virtuellen Zielcomputern zu verarbeiten. Azure Automation dient zur Verwaltung vorhandener virtueller Computer und nicht zur Erstellung einer Infrastruktur. Azure Automation kann für virtuelle Linux- und Windows-Computer sowie für lokale virtuelle oder physische Computer mit einem Hybrid Runbook Worker ausgeführt werden. Runbooks können in einem Quellcodeverwaltungs-Repository (beispielsweise GitHub) gespeichert werden. Diese Runbooks können dann manuell oder nach einem definierten Zeitplan ausgeführt werden.

Azure Automation bietet auch einen DSC-Dienst (Desired State Configuration), mit dem Sie Definitionen für die Konfiguration einer bestimmten Gruppe von virtuellen Computern erstellen können. DSC stellt dann sicher, dass die erforderliche Konfiguration angewendet wird und der virtuelle Computer konsistent bleibt. Azure Automation DSC kann auf Computern unter Windows und Linux ausgeführt werden.

In diesem Artikel werden folgende Themen erläutert:

- [Erstellen eines PowerShell-Runbooks](../articles/automation/automation-first-runbook-textual-powershell.md)
- [Verwalten von lokalen Ressourcen mithilfe eines Hybrid Runbook Workers](../articles/automation/automation-hybrid-runbook-worker.md)
- [Verwenden von Azure Automation DSC](../articles/automation/automation-dsc-getting-started.md)


## <a name="visual-studio-team-services"></a>Visual Studio Team Services
[Team Services](https://www.visualstudio.com/team-services/) ist eine Suite von Tools, mit denen Sie Code teilen und nachverfolgen, automatisierte Builds verwenden und eine vollständige CI/CD-Pipeline (Continuous Integration/Continuous Development) erstellen können. Team Services lässt sich zur Vereinfachung in Visual Studio und andere Editoren integrieren. Mit Team Services können Sie auch virtuelle Azure-Computer erstellen und konfigurieren und anschließend Code für sie bereitstellen.

In diesem Artikel werden folgende Themen erläutert:

- [Erstellen einer Continuous Integration-Pipeline mit Team Services](../articles/virtual-machines/windows/tutorial-vsts-iis-cicd.md)


## <a name="jenkins"></a>Jenkins
[Jenkins](https://www.jenkins.io) ist ein Continuous Integration-Server zum Bereitstellen und Testen von Anwendungen sowie zum Erstellen automatisierter Pipelines für die Codeübermittlung. Die Kernplattform von Jenkins lässt sich mit hunderten von Plug-Ins erweitern und über Webhooks auch in zahlreiche andere Produkte und Lösungen integrieren. Sie können Jenkins manuell auf einem virtuellen Azure-Computer installieren oder innerhalb eines Docker-Containers ausführen oder ein vorgefertigtes Azure Marketplace-Image verwenden.

In diesem Artikel werden folgende Themen erläutert:

- [Erstellen einer Entwicklungsinfrastruktur auf einem virtuellen Linux-Computer in Azure mit Jenkins, GitHub und Docker](../articles/virtual-machines/linux/tutorial-jenkins-github-docker-cicd.md)


## <a name="next-steps"></a>Nächste Schritte
Infrastrukturautomatisierungstools können in Azure auf unterschiedlichste Weise verwendet werden. Sie können sich flexibel für die Lösung entscheiden, die am besten zu Ihren Anforderungen und zu Ihrer Umgebung passt. Falls Sie noch keine Erfahrung haben und einige der in Azure integrierten Tools ausprobieren möchten, informieren Sie sich über die Automatisierung der Anpassung eines virtuellen Computers unter [Linux](../articles/virtual-machines/linux/tutorial-automate-vm-deployment.md) oder [Windows](../articles/virtual-machines/windows/tutorial-automate-vm-deployment.md).
