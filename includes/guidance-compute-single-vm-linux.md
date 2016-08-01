In diesem Artikel werden verschiedene bewährte Methoden zur Ausführung einer Linux-VM in Azure beschrieben. Dabei geht es vor allem um Skalierbarkeit, Verfügbarkeit, Verwaltbarkeit und Sicherheit. Azure unterstützt die Ausführung verschiedener beliebter Linux-Distributionen, so z.B. CentOS, Debian, Red Hat Enterprise, Ubuntu und FreeBSD. Weitere Informationen finden Sie unter [Azure und Linux][azure-linux].

> [AZURE.NOTE] Azure bietet zwei verschiedene Bereitstellungsmodelle: [Resource Manager][resource-manager-overview] und klassisch. In diesem Artikel wird Resource Manager verwendet, der von Microsoft für neue Bereitstellungen empfohlen wird.

Wir empfehlen nicht den Einsatz einer einzelnen VM für Produktionsworkloads, da es für einzelne VMs in Azure keine SLA zur Betriebszeit gibt. Um die SLA zu erhalten, müssen Sie mehrere VMs in einer Verfügbarkeitsgruppe bereitstellen. Weitere Informationen finden Sie unter [Ausführen mehrerer VMs in Azure][multi-vm].

## Architekturdiagramm

Die Bereitstellung einer einzelnen VM in Azure umfasst mehr „bewegliche Teile“ als die VM selbst. Es sind Compute-, Netzwerk- und Speicherelemente vorhanden.

![[0]][0]

- **Ressourcengruppe:** Eine [Ressourcengruppe][resource-manager-overview] ist ein Container, der verwandte Ressourcen enthält. Erstellen Sie eine Ressourcengruppe für die Ressourcen dieser VM.

- **VM**. Sie können eine VM über eine Liste mit veröffentlichten Images oder über eine VHD-Datei bereitstellen, die Sie in Azure Blob Storage hochladen.

- **Betriebssystem-Datenträger:** Der Betriebssystemdatenträger ist eine virtuelle Festplatte (Virtual Hard Disk, VHD), die im [Azure-Speicher][azure-storage] gespeichert ist. Dies bedeutet, dass er auch dann noch vorhanden ist, wenn der Hostcomputer ausfällt. Der Betriebssystemdatenträger ist `/dev/sda1`.

- **Temporärer Datenträger.** Die VM wird mit einem temporären Datenträger erstellt. Dieser Datenträger wird auf dem Hostcomputer auf einem physischen Laufwerk gespeichert. Er wird _nicht_ im Azure-Speicher gespeichert und kann bei Neustarts und anderen Ereignissen während des VM-Lebenszyklus verloren gehen. Verwenden Sie diesen Datenträger nur für temporäre Daten, z. B. Auslagerungsdateien. Der temporäre Datenträger ist `/dev/sdb1` und wird in `/mnt/resource` oder `/mnt` bereitgestellt.

- **Datenträger:** Bei einem [Datenträger für Daten][data-disk] handelt es sich um eine persistente VHD, die für Anwendungsdaten verwendet wird. Datenträger werden im Azure-Speicher gespeichert, z. B. auf dem Betriebssystem-Datenträger.

- **Virtuelles Netzwerk (VNet) und Subnetz\_** Jede VM in Azure wird in einem virtuellen Netzwerk (VNet) bereitgestellt, das weiter in Subnetze unterteilt wird.

- **Öffentliche IP-Adresse.** Eine öffentliche IP-Adresse wird für die Kommunikation mit der VM benötigt, z.B. über SSH.

- **Netzwerkschnittstelle bzw. Netzwerkschnittstellenkarte (Network Interface Card, NIC)**. Die NIC ermöglicht der VM die Kommunikation mit dem virtuellen Netzwerk.

- **Netzwerksicherheitsgruppe (NSG)**. Die [Netzwerksicherheitsgruppe][nsg] wird verwendet, um den Netzwerkdatenverkehr in das Subnetz zuzulassen oder zu verweigern. Sie können eine NSG einer einzelnen NIC oder einem Subnetz zuordnen. Wenn Sie sie einem Subnetz zuordnen, gelten die NSG-Regeln für alle VMs in diesem Subnetz.
 
- **Diagnose:** Diagnoseprotokolle sind für die Verwaltung und Problembehandlung des virtuellen Computers von entscheidender Bedeutung.

## Empfehlungen

### Empfehlungen für virtuelle Computer

- Wir empfehlen die DS- und GS-Serie, es sei denn, Sie haben einen speziellen Workload, z.B. High Performance Computing. Ausführliche Informationen finden Sie unter [Größen virtueller Computer][virtual-machine-sizes]. Starten Sie beim Verlagern eines vorhandenen Workloads in Azure mit der VM-Größe, die Ihren lokalen Servern am ehesten entspricht. Messen Sie dann die Leistung Ihres tatsächlichen Workloads hinsichtlich CPU, Arbeitsspeicher und Datenträger-IOPS, und passen Sie die Größe bei Bedarf an. Wenn Sie mehrere NICs benötigen, beachten Sie zudem den NIC-Grenzwert für jede Größe.

- Wenn Sie die VM und anderen Ressourcen bereitstellen, müssen Sie einen Standort angeben. Es ist im Allgemeinen ratsam, einen Standort zu wählen, der sich in der Nähe Ihrer internen Benutzer oder Ihrer Kunden befindet. Es sind jedoch nicht alle VM-Größen an allen Standorten verfügbar. Weitere Informationen finden Sie unter [Dienste nach Region][services-by-region]. Führen Sie den folgenden Azure-CLI-Befehl aus, um die an einem bestimmten Standort verfügbaren VM-Größen aufzulisten:

    ```
    azure vm sizes --location <location>
    ```

- Informationen zur Auswahl eines veröffentlichten VM-Image finden Sie unter [Navigieren zwischen und Auswählen von Images für virtuelle Azure-Computer][select-vm-image].

### Empfehlungen für Datenträger und Speicher

- Für eine optimale E/A-Leistung empfehlen wir [Storage Premium][premium-storage] zum Speichern von Daten auf SSDs (Solid State Drives). Die Kosten basieren auf der Größe des bereitgestellten Datenträgers. IOPS und Durchsatz (also die Datenübertragungsrate) richten sich ebenfalls nach der Datenträgergröße. Berücksichtigen Sie beim Bereitstellen eines Datenträgers also alle drei Faktoren (Kapazität, IOPS und Durchsatz).

- Fügen Sie einen oder mehrere Datenträger hinzu. Wenn Sie eine neue virtuelle Festplatte (VHD) erstellen, ist sie nicht formatiert. Melden Sie sich an der VM an, um den Datenträger zu formatieren. Die Datenträger für Daten werden als `/dev/sdc`, `/dev/sdd` usw. angezeigt. Sie können `lsblk` ausführen, um die Blockgeräte einschließlich Datenträgern aufzulisten. Um einen Datenträger für Daten zu verwenden, erstellen Sie eine neue Partition und ein Dateisystem, und binden Sie den Datenträger ein. Beispiel:

    ```bat
    # Create a partition.
    sudo fdisk /dev/sdc     # Enter 'n' to partition, 'w' to write the change.     
    
    # Create a file system.
    sudo mkfs -t ext3 /dev/sdc1
    
    # Mount the drive.
    sudo mkdir /data1
    sudo mount /dev/sdc1 /data1
    ```

- Wenn Sie über eine große Zahl von Datenträgern verfügen, sollten Sie sich über die E/A-Grenzwerte des Speicherkontos bewusst sein. Weitere Informationen finden Sie unter [Grenzwerte für Datenträger virtueller Computer][vm-disk-limits].

- Wenn Sie einen Datenträger für Daten hinzufügen, wird dem Datenträger eine logische Einheitennummer (Logical Unit Number, LUN) zugewiesen. Optional können Sie die LUN-ID angeben, z.B. wenn Sie einen Datenträger austauschen und dieselbe LUN-ID beibehalten möchten oder über eine verfügen, die eine bestimmte LUN-ID sucht. Beachten Sie jedoch, dass die LUN-IDs für jeden Datenträger eindeutig sein muss.

- Nach Wunsch können Sie den E/A-Scheduler ändern, um die Leistung der (von Storage Premium verwendeten) SSD-Laufwerke zu optimieren. Eine übliche Empfehlung ist die Verwendung des NOOP-Schedulers für SSDs. Sie sollten jedoch ein Tool wie [iostat] zur Überwachung der E/A-Datenträgerleistung für Ihre spezifische Workload einsetzen.

- Erstellen Sie ein separates Speicherkonto zum Speichern der Diagnoseprotokolle, um eine optimale Leistung zu erzielen. Ein standardmäßiger lokal redundanter Speicher (LRS) reicht für Diagnoseprotokolle aus.

### Netzwerkempfehlungen

- Die öffentliche IP-Adresse kann dynamisch oder statisch sein. Die Standardeinstellung ist „Dynamisch“.

    - Reservieren Sie eine [statische IP-Adresse][static-ip], falls Sie eine feste IP-Adresse benötigen, die sich nicht ändert – z.B. wenn Sie einen A-Eintrag in DNS erstellen oder die IP-Adresse auf eine Positivliste setzen müssen.

    - Sie können auch einen vollständig qualifizierten Domänennamen (FQDN) für die IP-Adresse erstellen. Sie können anschließend einen [CNAME-Eintrag][cname-record] in DNS registrieren, der auf den FQDN verweist. Weitere Informationen finden Sie unter [Erstellen eines vollqualifizierten Domänennamens im Azure-Portal][fqdn].

- Alle Netzwerksicherheitsgruppen enthalten eine Reihe von [Standardregeln][nsg-default-rules], einschließlich einer Regel, die den gesamten eingehenden Internetverkehr blockiert. Die Standardregeln können nicht gelöscht, aber von anderen Regeln überschrieben werden. Um Internetdatenverkehr zu ermöglichen, erstellen Sie Regeln, die eingehenden Datenverkehr an bestimmten Ports zulassen, z.B. Port 80 für HTTP.

- Fügen Sie zum Aktivieren von SSH der NSG eine Regel hinzu, die den eingehenden Datenverkehr am TCP-Port 22 zulässt.

## Überlegungen zur Skalierbarkeit

- Sie können eine VM zentral hoch- oder herunterskalieren, indem Sie [die VM-Größe ändern][vm-resize].

- Um horizontal zu skalieren, platzieren Sie zwei oder mehr VMs in einer Verfügbarkeitsgruppe hinter einem Load Balancer. Weitere Informationen finden Sie unter [Ausführen mehrerer VMs in Azure][multi-vm].

## Überlegungen zur Verfügbarkeit

- Wie bereits erwähnt, gibt es keine SLA für eine einzelne VM. Um die SLA zu erhalten, müssen Sie mehrere VMs in einer Verfügbarkeitsgruppe bereitstellen.

- Ihre VM kann von einer [geplanten Wartung][planned-maintenance] oder [ungeplanten Wartung][manage-vm-availability] betroffen sein. Sie können [VM-Neustartprotokolle][reboot-logs] verwenden, um zu ermitteln, ob ein VM-Neustart durch einen geplanten Wartungsvorgang verursacht wurde.

- VHDs werden mithilfe von [Azure Storage][azure-storage] gesichert, der repliziert wird, um die Dauerhaftigkeit und Verfügbarkeit sicherzustellen.

- Als Schutz vor versehentlichen Datenverlusten während des normalen Betriebs (z.B. aufgrund eines Benutzerfehlers) sollten Sie auch Point-in-Time-Sicherungen implementieren, indem Sie [Blobmomentaufnahmen][blob-snapshot] oder ein anderes Tool verwenden.

## Überlegungen zur Verwaltbarkeit

- **Ressourcengruppen.** Legen Sie eng miteinander verknüpfte Ressourcen mit demselben Lebenszyklus in derselben [Ressourcengruppe][resource-manager-overview] ab. Ressourcengruppen ermöglichen Ihnen das Bereitstellen und Überwachen von Ressourcen und das Zusammenfassen von Abrechnungskosten nach Ressourcengruppe. Sie können auch Ressourcen als Gruppe löschen, was für Testbereitstellungen sehr nützlich ist. Versehen Sie Ressourcen mit aussagekräftigen Namen. Dies vereinfacht das Auffinden einer bestimmten Ressource und Verstehen ihrer Rolle. Siehe [Empfohlene Benennungskonventionen für Azure-Ressourcen][naming conventions].

- **SSH**. Bevor Sie eine Linux-VM erstellen, wird ein 2048-Bit-RSA-Paar aus privatem und öffentlichem Schlüssel generiert. Verwenden Sie die öffentliche Schlüsseldatei bei der Erstellung der VM. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux und Mac in Azure][ssh-linux].

- **VM-Diagnose.** Ermöglichen Sie die Überwachung und Diagnose, einschließlich grundlegender Integritätsmetriken, Infrastrukturprotokolle zur Diagnose und [Startdiagnose][boot-diagnostics]. Startdiagnosen dienen dazu, einen Fehler beim Startvorgang zu untersuchen, wenn Ihre VM einen nicht startfähigen Zustand hat. Weitere Informationen finden Sie unter [Aktivieren von Überwachung und Diagnose][enable-monitoring].

    Der folgende CLI-Befehl aktiviert die Diagnose:

    ```text
    azure vm enable-diag <resource-group> <vm-name>
    ```

- **Beenden einer VM.** Unter Azure wird zwischen den Zuständen „Stopped“ (Beendet) und „De-allocated“ (Zuordnung aufgehoben) unterschieden. Wenn der VM-Status „Stopped“ lautet, fallen für Sie Kosten an. Wenn der VM-Status „De-allocated“ lautet, fallen für Sie keine Kosten an.

    Verwenden Sie den folgenden CLI-Befehl, um die Zuordnung für eine VM aufzuheben:

    ```text
    azure vm deallocate <resource-group> <vm-name>
    ```

    Auch mit der Schaltfläche **Beenden** im Azure-Portal können Sie die Zuordnung der VM aufheben. Wenn das Herunterfahren über das Betriebssystem erfolgt, während Sie angemeldet sind, wird die VM zwar beendet, aber die Zuordnung wird _nicht_ aufgehoben. Es fallen also weiter Kosten an.

- **Löschen einer VM.** Wenn Sie eine VM löschen, werden die VHDs nicht gelöscht. Dies bedeutet, dass Sie die VM problemlos löschen können, ohne dass Daten verloren gehen. Allerdings wird Ihnen der Speicherplatz weiter in Rechnung gestellt. Um die VHD zu löschen, löschen Sie die Datei aus dem [Blobspeicher][blob-storage].

  Um ein versehentliches Löschen zu verhindern, verwenden Sie eine [Ressourcensperre][resource-lock], um die gesamte Ressourcengruppe oder einzelne Ressourcen wie z.B. die VM zu sperren.

## Sicherheitshinweise

- Automatisieren Sie Betriebssystemupdates mithilfe der VM-Erweiterung [OSPatching]. Installieren Sie diese Erweiterung, wenn Sie die VM bereitstellen. Sie können angeben, wie oft Patches installiert werden sollen und ob im Anschluss daran ein Neustart erfolgen soll.

- Arbeiten Sie mit der [rollenbasierten Zugriffssteuerung][rbac] (Role-based Access Control, RBAC) zum Steuern des Zugriffs auf Azure-Ressourcen, die Sie bereitstellen. Mithilfe der RBAC können Sie Mitglieder Ihres DevOps-Teams Autorisierungsrollen zuweisen. Die Rolle „Leser“ kann beispielsweise Azure-Ressourcen anzeigen, diese aber nicht erstellen, verwalten oder löschen. Einige Rollen sind für bestimmte Azure-Ressourcentypen spezifisch. Die VM-Rolle „Mitwirkender“ kann z.B. eine VM neu starten oder freigeben, das Administratorkennwort zurücksetzen, eine neue VM erstellen usw. Andere [integrierte RBAC-Rollen][rbac-roles], die für diese Referenzarchitektur nützlich sein können, sind u.a. [DevTest Lab-Benutzer][rbac-devtest] und [Netzwerkmitwirkender][rbac-network]. Ein Benutzer kann mehreren Rollen zugewiesen werden. Außerdem können Sie für noch präzisere Berechtigungen benutzerdefinierte Rollen erstellen.

    > [AZURE.NOTE] Die RBAC schränkt nicht die Aktionen eines Benutzers ein, der bei einer VM angemeldet ist. Diese Berechtigungen werden vom Kontotyp im Gastbetriebssystem bestimmt.

- Verwenden Sie [Überwachungsprotokolle][audit-logs], um Bereitstellungsaktionen und andere VM-Ereignisse anzuzeigen.

- Ziehen Sie [Azure Disk Encryption][disk-encryption] in Betracht, wenn Sie die Datenträger für Betriebssystem und Daten verschlüsseln müssen.

## Lösungskomponenten

<!-- TO BE UPDATED WHEN THE NEW TEMPLATES ARE AVAILABLE -->

Mit dem folgenden Bash-Skript werden die [Azure-CLI][azure-cli]-Befehle ausgeführt, um eine einzelne VM-Instanz und die dazugehörigen Netzwerk- und Speicherressourcen bereitzustellen (siehe das vorherige Diagramm).

Das Skript verwendet die Benennungskonventionen, die unter [Empfohlene Benennungskonventionen für Azure-Ressourcen][naming conventions] beschrieben sind.

So führen Sie das Skript aus

1. Generieren Sie einen 2048-Bit-RSA-Authentifizierungsschlüssel.

        ssh-keygen -t rsa -b 2048

2. Übergeben Sie Ihre Azure-Abonnement-ID und den Namen der öffentlichen Schlüsseldatei als Parameter an das Skript.

        ./azurecli-single-vm-sample.sh <subscription ID> ~/.ssh/id_rsa.pub

3. Sobald das Skript abgeschlossen ist, melden Sie sich über SSH an der VM an. Verwenden Sie zur Authentifizierung den privaten Schlüssel.

        ssh testuser@<app>-vm1.<location>.cloudapp.azure.com -i ~/.ssh/id_rsa

    Dabei ist `<app>` der Wert der Skriptvariablen `APP_NAME` und `<location>` der Wert der Variablen `LOCATION`.

```bat
#!/bin/bash

############################################################################
#script for generating infrastructure for single VM running linux          #
# of user choice. It creates azure resource group, storage account for VM  #
# vnet, subnets for VM, and NSG rule                                       #
# tags for main variables used                                             #
# ScriptCommandParameters                                                  #
# ScriptVars                                                               #
############################################################################

############################################################################
# User defined functions for single VM script                              #
# errhandle : handles errors via trap if any exception happens             #
# in the cli execution or if the user interrupts with CTRL+C               #
# allowing for fast interruption                                           #
############################################################################

# error handling or interruption via ctrl-c.
# line number and error code of executed command is passed to errhandle function

trap 'errhandle $LINENO $?' SIGINT ERR

errhandle()
{
  echo "Error or Interruption at line ${1} exit code ${2} "
  exit ${2}
}

###############################################################################
############################## End of user defined functions ##################
###############################################################################

# 2 paramaters are expected
# public key file needs to be generated using ssh-keygen

if [ $# -ne 2  ]
then
	echo  "Usage:  ${0}  subscription-id public-ssh-key-file"
	exit
fi

if [ ! -f $2  ]
then
	echo "Public Key file ${2} does not exist. please generate it"
	echo "ssh-keygen -t rsa -b 2048"
	exit
fi

# Explicitly set the subscription to avoid confusion as to which subscription
# is active/default
# ScriptCommandParameters
SUBSCRIPTION=$1
PUBLICKEYFILE=$2

# ScriptVars
LOCATION=eastus2
APP_NAME=app1
ENVIRONMENT=dev
USERNAME=testuser
VM_NAME="${APP_NAME}-vm1"
RESOURCE_GROUP="${APP_NAME}-${ENVIRONMENT}-rg"
IP_NAME="${APP_NAME}-pip"
NIC_NAME="${VM_NAME}-1nic"
NSG_NAME="${APP_NAME}-nsg"
SUBNET_NAME="${APP_NAME}-subnet"
VNET_NAME="${APP_NAME}-vnet"
VHD_STORAGE="${VM_NAME//-}st1"
DIAGNOSTICS_STORAGE="${VM_NAME//-}diag"

# Use the following command to get the list of URNs for RHEL, UBUNTU, and OPENSUSE:
# RHEL
# azure vm image list $LOCATION  redhat RHEL 7.2
# UBUNTU
# azure vm image list $LOCATION canonical ubuntuserver 14.04.3-LTS
# SUSE
# azure vm image $LOCATION  suse opensuse 13.2

LINUX_BASE_IMAGE=redhat:rhel:7.2:7.2.20160302

# For a list of VM sizes see: 
#   https://azure.microsoft.com/documentation/articles/virtual-machines-size-specs/
# To see the VM sizes available in a region:
# 	azure vm sizes --location <location>
VM_SIZE=Standard_DS1

# Set up the postfix variables attached to most CLI commands

POSTFIX="--resource-group ${RESOURCE_GROUP} --subscription ${SUBSCRIPTION}"

azure config mode arm

#::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
#Create resources

#Create the enclosing resource group
azure group create --name $RESOURCE_GROUP --location $LOCATION --subscription $SUBSCRIPTION

# Create the VNet
azure network vnet create --address-prefixes 172.17.0.0/16 --name $VNET_NAME \
--location $LOCATION $POSTFIX

#Create the network security group
azure network nsg create --name $NSG_NAME --location $LOCATION $POSTFIX

#Create the subnet
azure network vnet subnet create --vnet-name $VNET_NAME --address-prefix  "172.17.0.0/24" \
--name $SUBNET_NAME --network-security-group-name $NSG_NAME $POSTFIX

#Create the public IP address (dynamic)
azure network public-ip create --name $IP_NAME --domain-name-label $VM_NAME \
--location $LOCATION $POSTFIX

#Create the NIC
azure network nic create --public-ip-name $IP_NAME --subnet-name $SUBNET_NAME \
--subnet-vnet-name $VNET_NAME --name $NIC_NAME --location $LOCATION $POSTFIX

#Create the storage account for the OS VHD
azure storage account create --type PLRS --location $LOCATION $POSTFIX $VHD_STORAGE

#Create the storage account for diagnostics logs
azure storage account create --type LRS --location $LOCATION $POSTFIX $DIAGNOSTICS_STORAGE

#Create the VM
azure vm create --name $VM_NAME --os-type Linux --image-urn  $LINUX_BASE_IMAGE \
--vm-size $VM_SIZE --vnet-subnet-name $SUBNET_NAME --vnet-name $VNET_NAME \
--nic-name $NIC_NAME --storage-account-name $VHD_STORAGE \
--os-disk-vhd "${VM_NAME}-osdisk.vhd" --admin-username $USERNAME \
--ssh-publickey-file $PUBLICKEYFILE --boot-diagnostics-storage-uri \
"https://${DIAGNOSTICS_STORAGE}.blob.core.windows.net/" --location $LOCATION $POSTFIX

#Attach a data disk
azure vm disk attach-new --vm-name $VM_NAME --size-in-gb 128 --vhd-name data1.vhd \
--storage-account-name $VHD_STORAGE $POSTFIX

#Allow SSH
azure network nsg rule create --nsg-name $NSG_NAME --direction Inbound --protocol Tcp \
--destination-port-range 22  --source-port-range "*"  --priority 100 --access Allow \
SSHAllow $POSTFIX

#Install patching extension
PATCH_CONFIG='{"rebootAfterPatch":"RebootIfNeed","startTime":"3:00","dayOfWeek":"Sunday","category":"ImportantAndRecommended"}'
azure vm extension set --name OSPatchingForLinux --publisher-name Microsoft.OSTCExtensions \
--public-config $PATCH_CONFIG --vm-name $VM_NAME --version 2.0 $POSTFIX
```

## Nächste Schritte

Damit die [SLA für virtuelle Computer][vm-sla] gilt, müssen Sie in einer Verfügbarkeitsgruppe mindestens zwei Instanzen bereitstellen. Weitere Informationen finden Sie unter [Ausführen mehrerer VMs in Azure][multi-vm].

<!-- links -->

[arm-templates]: ../articles/virtual-machines/virtual-machines-linux-cli-deploy-templates.md
[audit-logs]: https://azure.microsoft.com/de-DE/blog/analyze-azure-audit-logs-in-powerbi-more/
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-linux]: ../articles/virtual-machines/virtual-machines-linux-azure-overview.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/de-DE/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-linux-about-disks-vhds.md
[disk-encryption]: ../articles/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/azure-portal/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-linux-portal-create-fqdn.md
[iostat]: https://en.wikipedia.org/wiki/Iostat
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-linux-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[OSPatching]: https://github.com/Azure/azure-linux-extensions/tree/master/OSPatching
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-linux-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/de-DE/blog/viewing-vm-reboot-logs/
[Resize-VHD]: https://technet.microsoft.com/de-DE/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/de-DE/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/resource-group-overview.md
[select-vm-image]: ../articles/virtual-machines/virtual-machines-linux-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/de-DE/regions/#services
[ssh-linux]: ../articles/virtual-machines/virtual-machines-linux-ssh-from-linux.md
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-linux-sizes.md
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/de-DE/support/legal/sla/virtual-machines/v1_0/
[0]: ./media/guidance-blueprints/compute-single-vm.png "Allgemeine Architektur einer Azure-VM"

<!---HONumber=AcomDC_0720_2016-->