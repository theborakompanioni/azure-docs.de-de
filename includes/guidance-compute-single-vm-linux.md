In diesem Artikel werden verschiedene bewährte Methoden zur Ausführung einer Linux-VM in Azure beschrieben. Dabei geht es vor allem um Skalierbarkeit, Verfügbarkeit, Verwaltbarkeit und Sicherheit. Azure unterstützt die Ausführung verschiedener beliebter Linux-Distributionen, so z.B. CentOS, Debian, Red Hat Enterprise, Ubuntu und FreeBSD. Weitere Informationen finden Sie unter [Azure und Linux][azure-linux].

> [AZURE.NOTE] Azure bietet zwei verschiedene Bereitstellungsmodelle: [Resource Manager][resource-manager-overview] und klassisch. In diesem Artikel wird Resource Manager verwendet, der von Microsoft für neue Bereitstellungen empfohlen wird.

Wir empfehlen nicht den Einsatz einer einzelnen VM für Produktionsworkloads, da es für einzelne VMs in Azure keine SLA zur Betriebszeit gibt. Um die SLA zu erhalten, müssen Sie mehrere VMs in einer Verfügbarkeitsgruppe bereitstellen. Weitere Informationen finden Sie unter [Ausführen mehrerer VMs in Azure][multi-vm].

## Architekturdiagramm

Die Bereitstellung einer einzelnen VM in Azure umfasst mehr „bewegliche Teile“ als die VM selbst. Es sind Compute-, Netzwerk- und Speicherelemente vorhanden.

![[0]][0]

- **Ressourcengruppe:** Eine [_Ressourcengruppe_][resource-manager-overview] ist ein Container, der verwandte Ressourcen enthält. Erstellen Sie eine Ressourcengruppe für die Ressourcen dieser VM.

- **VM**. Sie können eine VM über eine Liste mit veröffentlichten Images oder über eine VHD-Datei bereitstellen, die Sie in Azure Blob Storage hochladen.

- **Betriebssystem-Datenträger:** Der Betriebssystemdatenträger ist eine virtuelle Festplatte (Virtual Hard Disk, VHD), die im [Azure-Speicher][azure-storage] gespeichert ist. Dies bedeutet, dass er auch dann noch vorhanden ist, wenn der Hostcomputer ausfällt. Der Betriebssystemdatenträger ist `/dev/sda1`.

- **Temporärer Datenträger.** Die VM wird mit einem temporären Datenträger erstellt. Dieser Datenträger wird auf dem Hostcomputer auf einem physischen Laufwerk gespeichert. Er wird _nicht_ im Azure-Speicher gespeichert und kann bei Neustarts und anderen Ereignissen während des VM-Lebenszyklus verloren gehen. Verwenden Sie diesen Datenträger nur für temporäre Daten, z. B. Auslagerungsdateien. Der temporäre Datenträger ist `/dev/sdb1` und wird in `/mnt/resource` oder `/mnt` bereitgestellt.

- **Datenträger:** Bei einem [Datenträger für Daten][data-disk] handelt es sich um eine persistente VHD, die für Anwendungsdaten verwendet wird. Datenträger werden im Azure-Speicher gespeichert, z. B. auf dem Betriebssystem-Datenträger.

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

- Ein Speicherkonto kann bis zu 20 virtuelle Computer unterstützen.

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

- Sie können einen virtuellen Computer zentral hoch- oder herunterskalieren, indem Sie [die VM-Größe ändern][vm-resize].

- Um horizontal zu skalieren, platzieren Sie zwei oder mehr VMs in einer Verfügbarkeitsgruppe hinter einem Load Balancer. Weitere Informationen finden Sie unter [Ausführen mehrerer virtueller Computer in Azure][multi-vm].

## Überlegungen zur Verfügbarkeit

- Wie bereits erwähnt, gibt es keine SLA für eine einzelne VM. Um die SLA zu erhalten, müssen Sie mehrere VMs in einer Verfügbarkeitsgruppe bereitstellen.

- Ihr virtueller Computer kann von einer [geplanten Wartung][planned-maintenance] oder [ungeplanten Wartung][manage-vm-availability] betroffen sein. Sie können [VM-Neustartprotokolle][reboot-logs] verwenden, um zu ermitteln, ob ein VM-Neustart durch einen geplanten Wartungsvorgang verursacht wurde.

- VHDs werden mithilfe von [Azure Storage][azure-storage] gesichert, und der Speicher wird repliziert, um die Dauerhaftigkeit und Verfügbarkeit sicherzustellen.

- Als Schutz vor versehentlichen Datenverlusten während des normalen Betriebs (z.B. aufgrund eines Benutzerfehlers) sollten Sie auch Point-in-Time-Sicherungen implementieren, indem Sie [Blobmomentaufnahmen][blob-snapshot] oder ein anderes Tool verwenden.

## Überlegungen zur Verwaltbarkeit

- **Ressourcengruppen.** Legen Sie eng miteinander verknüpfte Ressourcen mit demselben Lebenszyklus in derselben [Ressourcengruppe][resource-manager-overview] ab. Ressourcengruppen ermöglichen Ihnen das Bereitstellen und Überwachen von Ressourcen und das Zusammenfassen von Abrechnungskosten nach Ressourcengruppe. Sie können auch Ressourcen als Gruppe löschen, was für Testbereitstellungen sehr nützlich ist. Versehen Sie Ressourcen mit aussagekräftigen Namen. Dies vereinfacht das Auffinden einer bestimmten Ressource und Verstehen ihrer Rolle. Siehe [Empfohlene Benennungskonventionen für Azure-Ressourcen][naming conventions].

- **ssh**. Bevor Sie eine Linux-VM erstellen, wird ein 2048-Bit-RSA-Paar aus privatem und öffentlichem Schlüssel generiert. Verwenden Sie die öffentliche Schlüsseldatei bei der Erstellung der VM. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux und Mac in Azure][ssh-linux].

- **VM-Diagnose.** Aktivieren Sie die Überwachung und Diagnose, einschließlich grundlegender Integritätsmetriken, Infrastrukturprotokolle zur Diagnose sowie [Startdiagnose][boot-diagnostics]. Startdiagnosen dienen dazu, einen Fehler beim Startvorgang zu untersuchen, wenn Ihre VM einen nicht startfähigen Zustand hat. Weitere Informationen finden Sie unter [Aktivieren von Überwachung und Diagnose][enable-monitoring].

    Der folgende CLI-Befehl aktiviert die Diagnose:

    ```text
    azure vm enable-diag <resource-group> <vm-name>
    ```

- **Beenden einer VM.** Unter Azure wird zwischen den Zuständen „Stopped“ (Beendet) und „De-allocated“ (Zuordnung aufgehoben) unterschieden. Wenn der VM-Status „Stopped“ lautet, fallen für Sie Kosten an. Wenn der VM-Status „De-allocated“ lautet, fallen für Sie keine Kosten an.

    Verwenden Sie den folgenden CLI-Befehl, um die Zuordnung für eine VM aufzuheben:

    ```text
    azure vm deallocate <resource-group> <vm-name>
    ```

    Sie können die Zuordnung des virtuellen Computers auch mit der Schaltfläche **Beenden** im Azure-Portal aufheben. Wenn das Herunterfahren über das Betriebssystem erfolgt, während Sie angemeldet sind, wird der virtuelle Computer zwar beendet, aber die Zuordnung wird _nicht_ aufgehoben. Es fallen also weiter Kosten an.

- **Löschen einer VM.** Wenn Sie eine VM löschen, werden die VHDs nicht gelöscht. Dies bedeutet, dass Sie die VM problemlos löschen können, ohne dass Daten verloren gehen. Allerdings wird Ihnen der Speicherplatz weiter in Rechnung gestellt. Um die VHD zu löschen, löschen Sie die Datei aus dem [Blobspeicher][blob-storage].

  Um ein versehentliches Löschen zu verhindern, verwenden Sie eine [Ressourcensperre][resource-lock], um die gesamte Ressourcengruppe oder einzelne Ressourcen wie z.B. den virtuellen Computer zu sperren.

## Sicherheitshinweise

- Automatisieren Sie Betriebssystemupdates mithilfe der VM-Erweiterung [OSPatching]. Installieren Sie diese Erweiterung, wenn Sie die VM bereitstellen. Sie können angeben, wie oft Patches installiert werden sollen und ob im Anschluss daran ein Neustart erfolgen soll.

- Arbeiten Sie mit der [rollenbasierten Zugriffssteuerung][rbac] (Role-Based Access Control, RBAC) zum Steuern des Zugriffs auf die Azure-Ressourcen, die Sie bereitstellen. Mithilfe der RBAC können Sie Mitglieder Ihres DevOps-Teams Autorisierungsrollen zuweisen. Die Rolle „Leser“ kann beispielsweise Azure-Ressourcen anzeigen, diese aber nicht erstellen, verwalten oder löschen. Einige Rollen sind für bestimmte Azure-Ressourcentypen spezifisch. Die VM-Rolle „Mitwirkender“ kann z.B. eine VM neu starten oder freigeben, das Administratorkennwort zurücksetzen, eine neue VM erstellen usw. Andere [integrierte RBAC-Rollen][rbac-roles], die für diese Referenzarchitektur nützlich sein können, sind u.a. [DevTest Lab-Benutzer][rbac-devtest] und [Netzwerkmitwirkender][rbac-network]. Ein Benutzer kann mehreren Rollen zugewiesen werden. Außerdem können Sie für noch präzisere Berechtigungen benutzerdefinierte Rollen erstellen.

    > [AZURE.NOTE] Die RBAC schränkt nicht die Aktionen eines Benutzers ein, der bei einer VM angemeldet ist. Diese Berechtigungen werden vom Kontotyp im Gastbetriebssystem bestimmt.

- Verwenden Sie [Überwachungsprotokolle][audit-logs], um Bereitstellungsaktionen und andere VM-Ereignisse anzuzeigen.

- Ziehen Sie [Azure Disk Encryption][disk-encryption] in Betracht, wenn Sie die Datenträger für Betriebssystem und Daten verschlüsseln müssen.

## Lösungskomponenten

Das Beispielskript [Deploy-ReferenceArchitecture.ps1][solution-script] steht zur Verfügung, mit dessen Hilfe Sie die Architektur implementieren können, die den Empfehlungen in diesem Artikel folgt. Dieses Skript nutzt [Azure Resource Manager][arm-templates]-Vorlagen. Die Vorlagen bestehen aus verschiedenen Basisbausteinen, von denen jeder eine bestimmte Aktion ausführt, z.B. Erstellen eines virtuellen Netzwerks oder Konfigurieren einer Netzwerksicherheitsgruppe. Zweck des Skripts ist es, die Vorlagenbereitstellung zu orchestrieren.

Die Vorlagen enthalten Parameter, die sich in getrennten JSON-Dateien befinden. Sie können die Parameter in diesen Dateien so ändern, dass die Bereitstellung entsprechend Ihren Anforderungen konfiguriert wird. Sie müssen die Vorlagen nicht selbst ändern. Beachten Sie, dass Sie die Schemas der Objekte in den Parameterdateien nicht ändern dürfen.

Wenn Sie die Vorlagen bearbeiten, erstellen Sie Objekte, die den unter [Empfohlene Benennungskonventionen für Azure-Ressourcen][naming conventions] beschriebenen Benennungskonventionen folgen.

Das Skript verweist zum Erstellen des virtuellen Computers und der zugehörigen Infrastruktur auf die folgenden Parameterdateien:

- **[virtualNetwork.parameters.json][vnet-parameters]** Diese Datei definiert die VNet-Einstellungen, z.B. Name, Adressraum, Subnetze und die Adressen erforderlicher DNS-Server. Beachten Sie, dass Subnetzadressen vom Adressraum des VNet klassifiziert werden müssen.

	```json
  "parameters": {
    "virtualNetworkSettings": {
      "value": {
        "name": "app1-vnet",
        "resourceGroup": "app1-dev-rg",
        "addressPrefixes": [
          "172.17.0.0/16"
        ],
        "subnets": [
          {
            "name": "app1-subnet",
            "addressPrefix": "172.17.0.0/24"
          }
        ],
        "dnsServers": [ ]
      }
    }
  }
	```

- **[networkSecurityGroup.parameters.json][nsg-parameters]** Diese Datei enthält die Definitionen von NSGs und NSG-Regeln. Der Parameter `name` im Block `virtualNetworkSettings` gibt das VNet an, dem die NSG zugeordnet ist. Der Parameter `subnets` im Block `networkSecurityGroupSettings` identifiziert alle Subnetze, für die die NSG-Regeln im VNet gelten. Dies müssen Elemente sein, die in der Datei **virtualNetwork.parameters.json** definiert sind.

	Die im Beispiel gezeigte Sicherheitsregel ermöglicht Benutzern eine Verbindung mit dem virtuellen Computer über eine SSH-Verbindung. Sie können zusätzliche Ports öffnen (oder den Zugriff über bestimmte Ports verweigern), indem Sie dem `securityRules`-Array weitere Elemente hinzufügen.

	```json
  "parameters": {
    "virtualNetworkSettings": {
      "value": {
        "name": "app1-vnet",
        "resourceGroup": "app1-dev-rg"
      },
      "metadata": {
        "description": "Infrastructure Settings"
      }
    },
    "networkSecurityGroupSettings": {
      "value": [
        {
          "name": "app1-nsg",
          "subnets": [
            "app1-subnet"
          ],
          "securityRules": [
            {
              "name": "default-allow-ssh",
              "direction": "Inbound",
              "priority": 1000,
              "sourceAddressPrefix": "*",
              "destinationAddressPrefix": "*",
              "sourcePortRange": "*",
              "destinationPortRange": "22",
              "access": "Allow",
              "protocol": "Tcp"
            }
          ]
        }
      ]
    }
  }
	```

- **[virtualMachineParameters.json][vm-parameters]** Diese Datei definiert die Einstellungen für den virtuellen Computer selbst, einschließlich Name und Größe, die Sicherheitsanmeldeinformationen für den Benutzer „admin“, die zu erstellenden Datenträger und die Speicherkonten für diese Datenträger.

	Stellen Sie sicher, dass Sie den Parameter `osType` auf `linux` festlegen. Sie müssen auch im Abschnitt `imageReference` ein Image angeben. Anhand der nachstehenden Werte wird ein virtueller Computer mit dem neuesten Build von RedHat Linux 7.2 erstellt. Über den folgenden Azure CLI-Befehl können Sie eine Liste aller verfügbaren RedHat-Images in einer Region abrufen (im Beispiel ist die Region „USA, Westen“):

	```powershell
	azure vm image list westus redhat rhel
	```

	Der Parameter `subnetName` im Abschnitt `nics` gibt das Subnetz für den virtuellen Computer an. Der Parameter `name` in `virtualNetworkSettings` gibt das zu verwendende VNet an. Dies muss der Name eines Subnetzes und VNet sein, das in der Datei **virtualNetwork.parameters.json** definiert ist.

	Sie können mehrere virtuelle Computer erstellen, die entweder ein Speicherkonto gemeinsam nutzen oder jeweils ein eigenes Speicherkonto haben, indem Sie die Einstellungen im Abschnitt `buildingBlockSettings` ändern. Wenn Sie mehrere virtuelle Computer erstellen, müssen Sie im Abschnitt `availabilitySet` auch den Namen einer zu verwendenden oder zu erstellenden Verfügbarkeitsgruppe angeben.

	```json
  "parameters": {
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "app1",
        "computerNamePrefix": "cn",
        "size": "Standard_DS1",
        "osType": "linux",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "true",
            "subnetName": "app1-subnet",
            "privateIPAllocationMethod": "dynamic",
            "publicIPAllocationMethod": "dynamic",
            "isPrimary": "true"
          }
        ],
        "imageReference": {
          "publisher": "RedHat",
          "offer": "RHEL",
          "sku": "7.2",
          "version": "latest"
        },
        "dataDisks": {
          "count": 2,
          "properties": {
            "diskSizeGB": 128,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": ""
        }
      },
      "metadata": {
        "description": "Settings for Virtual Machines"
      }
    },
    "virtualNetworkSettings": {
      "value": {
        "name": "app1-vnet",
        "resourceGroup": "app1-dev-rg"
      },
      "metadata": {
        "description": "Infrastructure Settings"
      }
    },
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 1,
        "vmCount": 1,
        "vmStartIndex": 0
      },
      "metadata": {
        "description": "Settings specific to the building block"
      }
    }
  }
	```

## Bereitstellung

Für die Lösung wird Folgendes vorausgesetzt:

- Sie haben ein Azure-Abonnement, in dem Sie Ressourcengruppen erstellen können.

- Sie haben den neuesten Build von Azure PowerShell heruntergeladen und installiert. Anweisungen dazu finden Sie [hier][azure-powershell-download].

So führen Sie das Skript aus, das die Lösung bereitstellt:

1. Wechseln Sie auf Ihrem lokalen Computer zu einem geeigneten Ordner, und erstellen Sie die beiden Unterordner:

	- Skripts

	- Vorlagen

2. Erstellen Sie im Ordner „Templates“ einen weiteren Unterordner namens „Linux“.

3. Laden Sie die Datei [Deploy-ReferenceArchitecture.ps1][solution-script] in den Ordner „Scripts“ herunter.

4. Laden Sie folgende Dateien in den Ordner „Templates/Linux“ herunter:

	- [virtualNetwork.parameters.json][vnet-parameters]

	- [networkSecurityGroup.parameters.json][nsg-parameters]

	- [virtualMachineParameters.json][vm-parameters]

5. Bearbeiten Sie im Ordner „Scripts“ die Datei „Deploy-ReferenceArchitecture.ps1“, und ändern Sie die folgende Zeile so, dass die Ressourcengruppe angegeben wird, die zum Aufnehmen des vom Skript erstellten virtuellen Computers und der zugehörigen Ressourcen erstellt bzw. verwendet werden soll:

	```powershell
	$resourceGroupName = "app1-dev-rg"
	```
6. Bearbeiten Sie die JSON-Dateien im Ordner „Templates/Linux“ so, dass die Parameter für das virtuelle Netzwerk, die NSG und VM entsprechend der Beschreibung im vorherigen Abschnitt „Lösungskomponenten“ festgelegt werden.

	>[AZURE.NOTE] Stellen Sie sicher, dass Sie den Parameter `resourceGroup` im Abschnitt `virtualNetworkSettings` der Datei „virtualMachineParameters.json“ entsprechend der Angabe in der Skriptdatei „Deploy-ReferenceArchitecture.ps1“ festlegen.

7. Öffnen Sie ein Azure PowerShell-Fenster, wechseln Sie zum Ordner „Scripts“, und führen Sie den folgenden Befehl aus:

	```powershell
	.\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Linux
	```

	Ersetzen Sie `<subscription id>` durch Ihre Azure-Abonnement-ID.

	Geben Sie für `<location>` eine Azure-Region an, z.B. `eastus` oder `westus`.

8. Überprüfen Sie nach Abschluss des Skripts im Azure-Portal, ob das Netzwerk, die NSG und VM erfolgreich erstellt wurden.

## Nächste Schritte

Damit die [SLA für virtuelle Computer][vm-sla] gilt, müssen Sie in einer Verfügbarkeitsgruppe mindestens zwei Instanzen bereitstellen. Weitere Informationen finden Sie unter [Ausführen mehrerer virtueller Computer in Azure][multi-vm].

<!-- links -->

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
[arm-templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
[solution-script]: https://raw.githubusercontent.com/mspnp/arm-building-blocks/master/guidance-compute-single-vm/Scripts/Deploy-ReferenceArchitecture.ps1
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/arm-building-blocks/master/guidance-compute-single-vm/Templates/linux/virtualNetwork.parameters.json
[nsg-parameters]: https://raw.githubusercontent.com/mspnp/arm-building-blocks/master/guidance-compute-single-vm/Templates/linux/networkSecurityGroup.parameters.json
[vm-parameters]: https://raw.githubusercontent.com/mspnp/arm-building-blocks/master/guidance-compute-single-vm/Templates/linux/virtualMachine.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[0]: ./media/guidance-blueprints/compute-single-vm.png "Architektur einer einzelnen Linux-VM in Azure"

<!---HONumber=AcomDC_0803_2016-->