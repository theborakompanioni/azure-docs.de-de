In diesem Artikel werden verschiedene bewährte Methoden zur Ausführung einer Windows-VM in Azure beschrieben. Dabei geht es vor allem um Skalierbarkeit, Verfügbarkeit, Verwaltbarkeit und Sicherheit.

> [AZURE.NOTE] Azure bietet zwei verschiedene Bereitstellungsmodelle: [Resource Manager][resource-manager-overview] und klassisch. In diesem Artikel wird Resource Manager verwendet, der von Microsoft für neue Bereitstellungen empfohlen wird.

Wir empfehlen nicht den Einsatz einer einzelnen VM für Produktionsworkloads, da es für einzelne VMs in Azure keine SLA zur Betriebszeit gibt. Um die SLA zu erhalten, müssen Sie mehrere VMs in einer Verfügbarkeitsgruppe bereitstellen. Weitere Informationen finden Sie unter [Ausführen mehrerer Windows-VMs in Azure][multi-vm].

## Architekturdiagramm

Die Bereitstellung einer einzelnen VM in Azure umfasst mehr „bewegliche Teile“ als die VM selbst. Es sind Compute-, Netzwerk- und Speicherelemente vorhanden.

![IaaS: einzelne VM](./media/guidance-blueprints/compute-single-vm.png)

- **Ressourcengruppe:** Eine [_Ressourcengruppe_][resource-manager-overview] ist ein Container, der verwandte Ressourcen enthält. Erstellen Sie eine Ressourcengruppe für die Ressourcen dieser VM.

- **VM**. Sie können eine VM über eine Liste mit veröffentlichten Images oder über eine VHD-Datei bereitstellen, die Sie in Azure Blob Storage hochladen.

- **Betriebssystem-Datenträger:** Der Betriebssystem-Datenträger ist eine virtuelle Festplatte (VHD), die in [Azure-Speicher][azure-storage] gespeichert ist. Dies bedeutet, dass er auch dann noch vorhanden ist, wenn der Hostcomputer ausfällt.

- **Temporärer Datenträger:** Die VM wird mit einem temporären Datenträger erstellt (Laufwerk `D:` unter Windows). Dieser Datenträger wird auf dem Hostcomputer auf einem physischen Laufwerk gespeichert. Er wird _nicht_ in Azure-Speicher gespeichert und kann bei Neustarts und anderen Ereignissen während des VM-Lebenszyklus verloren gehen. Verwenden Sie diesen Datenträger nur für temporäre Daten, z. B. Auslagerungsdateien.

- **Datenträger:** Bei einem [Datenträger für Daten][data-disk] handelt es sich um eine permanente VHD, die für Anwendungsdaten verwendet wird. Datenträger werden im Azure-Speicher gespeichert, z. B. auf dem Betriebssystem-Datenträger.

- **Virtuelles Netzwerk (VNet) und Subnetz\_** Jede VM in Azure wird in einem virtuellen Netzwerk (VNet) bereitgestellt, das weiter in Subnetze unterteilt wird.

- **Öffentliche IP-Adresse:** Eine öffentliche IP-Adresse wird für die Kommunikation mit der VM benötigt, z. B. per Remotedesktop (RDP).

- **Netzwerkschnittstelle (NIC)**. Die NIC ermöglicht der VM die Kommunikation mit dem virtuellen Netzwerk.

- **Netzwerksicherheitsgruppe (NSG)**. Die [NSG][nsg] wird verwendet, um den Netzwerk-Datenverkehr in das Subnetz zuzulassen oder zu verweigern. Sie können eine NSG einer einzelnen NIC oder einem Subnetz zuordnen. Wenn Sie sie einem Subnetz zuordnen, gelten die NSG-Regeln für alle VMs in diesem Subnetz.
 
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

- Für eine optimale E/A-Leistung empfehlen wir [Storage Premium][premium-storage] zum Speichern von Daten auf SSD-Laufwerken. Die Kosten basieren auf der Größe des bereitgestellten Datenträgers. IOPS und Durchsatz (also die Datenübertragungsrate) richten sich ebenfalls nach der Datenträgergröße. Berücksichtigen Sie beim Bereitstellen eines Datenträgers also alle drei Faktoren (Kapazität, IOPS und Durchsatz).

- Fügen Sie einen oder mehrere Datenträger hinzu. Wenn Sie eine neue virtuelle Festplatte (VHD) erstellen, ist sie nicht formatiert. Melden Sie sich an der VM an, um den Datenträger zu formatieren.

- Wenn Sie über eine große Zahl von Datenträgern verfügen, sollten Sie sich über die E/A-Grenzwerte des Speicherkontos bewusst sein. Weitere Informationen finden Sie unter [Einschränkungen für Datenträger virtueller Computer][vm-disk-limits].

- Erstellen Sie ein separates Speicherkonto zum Speichern der Diagnoseprotokolle, um eine optimale Leistung zu erzielen. Ein standardmäßiger lokal redundanter Speicher (LRS) reicht für Diagnoseprotokolle aus.

- Installieren Sie, sofern möglich, Anwendungen auf einem Datenträger für Daten und nicht auf dem Datenträger für das Betriebssystem. Allerdings müssen einige ältere Anwendungen Komponenten ggf. auf Laufwerk C: installieren. In diesem Fall können Sie [Größe den Betriebssystem-Datenträgers ändern][resize-os-disk], und zwar mithilfe von PowerShell.

### Netzwerkempfehlungen

- Die öffentliche IP-Adresse kann dynamisch oder statisch sein. Die Standardeinstellung ist „Dynamisch“.

    - Reservieren Sie eine [statische IP-Adresse][static-ip], falls Sie eine feste IP-Adresse benötigen, die sich nicht ändert. Es kann beispielsweise sein, dass Sie einen A-Eintrag in DNS erstellen oder die IP-Adresse auf eine Positivliste setzen müssen.

    - Sie können auch einen vollständig qualifizierten Domänennamen (FQDN) für die IP-Adresse erstellen. Sie können anschließen einen [CNAME-Eintrag][cname-record] im DNS registrieren, der auf den FQDN verweist. Weitere Informationen finden Sie unter [Erstellen eines vollständig qualifizierten Domänennamens im Azure-Portal][fqdn].

- Alle NSGs enthalten eine Reihe von [Standardregeln][nsg-default-rules], einschließlich einer Regel, die den gesamten eingehenden Internetverkehr blockiert. Die Standardregeln können nicht gelöscht, aber von anderen Regeln überschrieben werden. Um Internetdatenverkehr zu ermöglichen, erstellen Sie Regeln, die eingehenden Datenverkehr an bestimmten Ports zulassen, z.B. Port 80 für HTTP.

- Fügen Sie zum Aktivieren von RDP eine NSG-Regel hinzu, die eingehenden Datenverkehr am TCP-Port 3389 zulässt.

## Überlegungen zur Skalierbarkeit

- Sie können eine VM zentral hoch- oder herunterskalieren, indem Sie [die VM-Größe ändern][vm-resize].

- Um horizontal zu skalieren, platzieren Sie zwei oder mehr VMs in einer Verfügbarkeitsgruppe hinter einem Load Balancer. Weitere Informationen finden Sie unter [Ausführen mehrerer Windows-VMs in Azure][multi-vm].

## Überlegungen zur Verfügbarkeit

- Wie bereits erwähnt, gibt es keine SLA für eine einzelne VM. Um die SLA zu erhalten, müssen Sie mehrere VMs in einer Verfügbarkeitsgruppe bereitstellen.

- Ihre VM kann von einer [geplanten Wartung][planned-maintenance] oder [ungeplanten Wartung][manage-vm-availability] betroffen sein. Sie können [VM-Neustartprotokolle][reboot-logs] verwenden, um zu ermitteln, ob ein VM-Neustart durch einen geplanten Wartungsvorgang verursacht wurde.

- VHDs werden mithilfe von [Azure Storage][azure-storage] gesichert, der repliziert wird, um die Dauerhaftigkeit und Verfügbarkeit sicherzustellen.

- Als Schutz vor versehentlichen Datenverlusten während des normalen Betriebs (z.B. aufgrund eines Benutzerfehlers) sollten Sie auch Point-in-Time-Sicherungen implementieren, indem Sie [Blobmomentaufnahmen][blob-snapshot] oder ein anderes Tool verwenden.

## Überlegungen zur Verwaltbarkeit

- **Ressourcengruppen.** Legen Sie eng verknüpfte Ressourcen mit demselben Lebenszyklus in derselben [Ressourcengruppe][resource-manager-overview] ab. Ressourcengruppen ermöglichen Ihnen das Bereitstellen und Überwachen von Ressourcen und das Zusammenfassen von Abrechnungskosten nach Ressourcengruppe. Sie können auch Ressourcen als Gruppe löschen, was für Testbereitstellungen sehr nützlich ist. Versehen Sie Ressourcen mit aussagekräftigen Namen. Dies vereinfacht das Auffinden einer bestimmten Ressource und Verstehen ihrer Rolle. Siehe [Empfohlene Benennungskonventionen für Azure-Ressourcen][naming conventions].

- **VM-Diagnose.** Ermöglichen Sie die Überwachung und Diagnose, einschließlich grundlegender Integritätsmetriken, Diagnose-Infrastrukturprotokolle und [Startdiagnose][boot-diagnostics]. Startdiagnosen dienen dazu, einen Fehler beim Startvorgang zu untersuchen, wenn Ihre VM einen nicht startfähigen Zustand hat. Weitere Informationen finden Sie unter [Aktivieren von Überwachung und Diagnose][enable-monitoring]. Verwenden Sie die Erweiterung [Azure-Protokollsammlung][log-collector], um Azure-Plattformprotokolle zu erfassen und in Azure-Speicher hochzuladen.

    Der folgende CLI-Befehl aktiviert die Diagnose:

    ```text
    azure vm enable-diag <resource-group> <vm-name>
     ```

- **Beenden einer VM.** Unter Azure wird zwischen den Zuständen „Stopped“ (Beendet) und „De-allocated“ (Zuordnung aufgehoben) unterschieden. Wenn der VM-Status „Stopped“ lautet, fallen für Sie Kosten an. Wenn der VM-Status „De-allocated“ lautet, fallen für Sie keine Kosten an.

    Verwenden Sie den folgenden CLI-Befehl, um die Zuordnung für eine VM aufzuheben:

    ```text
    azure vm deallocate <resource-group> <vm-name>
    ```

    Mit der Schaltfläche **Beenden** im Azure-Portal können Sie die VM auch freigeben. Wenn das Herunterfahren über das Betriebssystem erfolgt, während Sie angemeldet sind, wird die VM zwar beendet, aber die Zuordnung wird _nicht_ aufgehoben. Es fallen also weiter Kosten an.

- **Löschen einer VM.** Wenn Sie eine VM löschen, werden die VHDs nicht gelöscht. Dies bedeutet, dass Sie die VM problemlos löschen können, ohne dass Daten verloren gehen. Allerdings wird Ihnen der Speicherplatz weiter in Rechnung gestellt. Um die VHD zu löschen, löschen Sie die Datei aus dem [Blobspeicher][blob-storage].

  Um ein versehentliches Löschen zu verhindern, verwenden Sie eine [Ressourcensperre][resource-lock], um die gesamte Ressourcengruppe oder einzelne Ressourcen wie die VM zu sperren.



## Sicherheitshinweise

- Verwenden Sie [Azure Security Center][security-center], um sich eine zentrale Übersicht über den Sicherheitszustand Ihrer Azure-Ressourcen zu verschaffen. Mit Security Center werden potenzielle Sicherheitsprobleme überwacht, z.B. Systemupdates, und Antischadsoftware, und Sie erhalten eine umfassende Darstellung des Sicherheitszustands Ihrer Bereitstellung.

    - Security Center wird für jedes Azure-Abonnement individuell konfiguriert. Aktivieren Sie die Sammlung von Sicherheitdaten, wie unter [Verwenden von Security Center] beschrieben.
    - Nachdem die Datensammlung aktiviert wurde, durchsucht Security Center VMs automatisch, die unter diesem Abonnement erstellt werden.

- **Patchverwaltung.** Falls aktiviert, prüft Security Center, ob kritische und Sicherheitsupdates fehlen. Verwenden Sie [Gruppenrichtlinieneinstellungen][group-policy] in der VM, um automatische Systemupdates zu aktivieren.

- **Antimalware.** Falls aktiviert, prüft Security Center, ob Antischadsoftware installiert ist. Sie können auch im Azure-Portal das Security Center nutzen, um Antischadsoftware zu installieren.

- Arbeiten Sie mit der [rollenbasierten Zugriffssteuerung][rbac] (Role-based Access Control, RBAC) zum Steuern des Zugriffs auf Azure-Ressourcen, die Sie bereitstellen. Mithilfe der RBAC können Sie Mitglieder Ihres DevOps-Teams Autorisierungsrollen zuweisen. Die Rolle „Leser“ kann beispielsweise Azure-Ressourcen anzeigen, diese aber nicht erstellen, verwalten oder löschen. Einige Rollen sind für bestimmte Azure-Ressourcentypen spezifisch. Die VM-Rolle „Mitwirkender“ kann z.B. eine VM neu starten oder Ihre Zuordnung aufheben, das Administratorkennwort zurücksetzen, eine neue VM erstellen usw. Andere [integrierte RBAC-Rollen][rbac-roles], die für diese Referenzarchitektur nützlich sein können, sind u.a. [DevTest Lab-Benutzer][rbac-devtest] und [Netzwerkmitwirkender][rbac-network]. Ein Benutzer kann mehreren Rollen zugewiesen werden. Außerdem können Sie für noch präzisere Berechtigungen benutzerdefinierte Rollen erstellen.

    > [AZURE.NOTE] Die RBAC schränkt nicht die Aktionen eines Benutzers ein, der bei einer VM angemeldet ist. Diese Berechtigungen werden vom Kontotyp im Gastbetriebssystem bestimmt.

- Führen Sie zum Zurücksetzen des lokalen Administratorkennworts den Azure-CLI-Befehl `vm reset-access` aus.

    ```text
    azure vm reset-access -u <user> -p <new-password> <resource-group> <vm-name>
    ```

- Nutzen Sie [Überwachungsprotokolle][audit-logs], um Bereitstellungsaktionen und andere VM-Ereignisse zu verfolgen.

- Ziehen Sie [Azure Disk Encryption][disk-encryption] in Betracht, wenn Sie die Datenträger für Betriebssystem und Daten verschlüsseln müssen.

## Beispiel eines Bereitstellungsskripts

Mit dem folgenden Windows-Batchskript werden die [Azure-CLI][azure-cli]-Befehle ausgeführt, um eine einzelne VM-Instanz und die dazugehörigen Netzwerk- und Speicherressourcen bereitzustellen (siehe das vorherige Diagramm).

Das Skript verwendet die Benennungskonventionen, die unter [Empfohlene Benennungskonventionen für Azure-Ressourcen][naming conventions] beschrieben sind.

```bat
ECHO OFF
SETLOCAL

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
:: Set up variables for deploying resources to Azure.
:: Change these variables for your own deployment.

:: The APP_NAME variable must not exceed 4 characters in size.
:: If it does the 15 character size limitation of the VM name may be exceeded.

SET APP_NAME=app1
SET LOCATION=eastus2
SET ENVIRONMENT=dev
SET USERNAME=testuser


:: For Windows, use the following command to get the list of URNs:
:: azure vm image list %LOCATION% MicrosoftWindowsServer WindowsServer 2012-R2-Datacenter
SET WINDOWS_BASE_IMAGE=MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20160126

:: For a list of VM sizes see:
::   https://azure.microsoft.com/documentation/articles/virtual-machines-size-specs/
:: To see the VM sizes available in a region:
:: 	azure vm sizes --location <location>
SET VM_SIZE=Standard_DS1

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

IF "%2"=="" (
    ECHO Usage: %0 subscription-id admin-password
    EXIT /B
    )

:: Explicitly set the subscription to avoid confusion as to which subscription
:: is active/default
SET SUBSCRIPTION=%1
SET PASSWORD=%2

:: Set up the names of things using recommended conventions
SET RESOURCE_GROUP=%APP_NAME%-%ENVIRONMENT%-rg
SET VM_NAME=%APP_NAME%-vm0

SET IP_NAME=%APP_NAME%-pip
SET NIC_NAME=%VM_NAME%-0nic
SET NSG_NAME=%APP_NAME%-nsg
SET SUBNET_NAME=%APP_NAME%-subnet
SET VNET_NAME=%APP_NAME%-vnet
SET VHD_STORAGE=%VM_NAME:-=%st0
SET DIAGNOSTICS_STORAGE=%VM_NAME:-=%diag

:: Set up the postfix variables attached to most CLI commands
SET POSTFIX=--resource-group %RESOURCE_GROUP% --subscription %SUBSCRIPTION%

CALL azure config mode arm

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
:: Create resources

:: Create the enclosing resource group
CALL azure group create --name %RESOURCE_GROUP% --location %LOCATION% ^
  --subscription %SUBSCRIPTION%

:: Create the VNet
CALL azure network vnet create --address-prefixes 172.17.0.0/16 ^
  --name %VNET_NAME% --location %LOCATION% %POSTFIX%

:: Create the network security group
CALL azure network nsg create --name %NSG_NAME% --location %LOCATION% %POSTFIX%

:: Create the subnet
CALL azure network vnet subnet create --vnet-name %VNET_NAME% --address-prefix ^
  172.17.0.0/24 --name %SUBNET_NAME% --network-security-group-name %NSG_NAME% ^
  %POSTFIX%

:: Create the public IP address (dynamic)
CALL azure network public-ip create --name %IP_NAME% --location %LOCATION% %POSTFIX%

:: Create the NIC
CALL azure network nic create --public-ip-name %IP_NAME% --subnet-name ^
  %SUBNET_NAME% --subnet-vnet-name %VNET_NAME%  --name %NIC_NAME% --location ^
  %LOCATION% %POSTFIX%

:: Create the storage account for the OS VHD
CALL azure storage account create --type PLRS --location %LOCATION% %POSTFIX% ^
  %VHD_STORAGE%

:: Create the storage account for diagnostics logs
CALL azure storage account create --type LRS --location %LOCATION% %POSTFIX% ^
  %DIAGNOSTICS_STORAGE%

:: Create the VM
CALL azure vm create --name %VM_NAME% --os-type Windows --image-urn ^
  %WINDOWS_BASE_IMAGE% --vm-size %VM_SIZE%   --vnet-subnet-name %SUBNET_NAME% ^
  --vnet-name %VNET_NAME% --nic-name %NIC_NAME% --storage-account-name ^
  %VHD_STORAGE% --os-disk-vhd "%VM_NAME%-osdisk.vhd" --admin-username ^
  "%USERNAME%" --admin-password "%PASSWORD%" --boot-diagnostics-storage-uri ^
  "https://%DIAGNOSTICS_STORAGE%.blob.core.windows.net/" --location %LOCATION% ^
  %POSTFIX%

:: Attach a data disk
CALL azure vm disk attach-new --vm-name %VM_NAME% --size-in-gb 128 --vhd-name ^
  data1.vhd --storage-account-name %VHD_STORAGE% %POSTFIX%

:: Allow RDP
CALL azure network nsg rule create --nsg-name %NSG_NAME% --direction Inbound ^
  --protocol Tcp --destination-port-range 3389 --source-port-range * ^
  --priority 100 --access Allow RDPAllow %POSTFIX%
```

## Nächste Schritte

Damit die [SLA für virtuelle Computer][vm-sla] gilt, müssen Sie in einer Verfügbarkeitsgruppe mindestens zwei Instanzen bereitstellen. Weitere Informationen finden Sie unter [Ausführen mehrerer Windows-VMs in Azure][multi-vm].

<!-- links -->

[arm-templates]: ../articles/virtual-machines/virtual-machines-windows-cli-deploy-templates.md
[audit-logs]: https://azure.microsoft.com/de-DE/blog/analyze-azure-audit-logs-in-powerbi-more/
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/de-DE/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-windows-about-disks-vhds.md
[disk-encryption]: ../articles/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/azure-portal/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-windows-portal-create-fqdn.md
[group-policy]: https://technet.microsoft.com/de-DE/library/dn595129.aspx
[log-collector]: https://azure.microsoft.com/de-DE/blog/simplifying-virtual-machine-troubleshooting-using-azure-log-collector/
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-windows-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-windows-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/de-DE/blog/viewing-vm-reboot-logs/
[resize-os-disk]: ../articles/virtual-machines/virtual-machines-windows-expand-os-disk.md
[Resize-VHD]: https://technet.microsoft.com/de-DE/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/de-DE/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/resource-group-overview.md
[security-center]: https://azure.microsoft.com/de-DE/services/security-center/
[select-vm-image]: ../articles/virtual-machines/virtual-machines-windows-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/de-DE/regions/#services
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[Verwenden von Security Center]: ../articles/security-center/security-center-get-started.md#use-security-center
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-windows-sizes.md
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/de-DE/support/legal/sla/virtual-machines/v1_0/

<!---HONumber=AcomDC_0713_2016-->