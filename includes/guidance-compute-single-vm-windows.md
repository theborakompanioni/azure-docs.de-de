In diesem Artikel werden verschiedene bewährte Methoden zur Ausführung einer Windows-VM in Azure beschrieben. Dabei geht es vor allem um Skalierbarkeit, Verfügbarkeit, Verwaltbarkeit und Sicherheit. 

> [!NOTE]
> Azure bietet zwei verschiedene Bereitstellungsmodelle: [Azure Resource Manager][resource-manger-overview] und klassisch. In diesem Artikel wird Resource Manager verwendet, der von Microsoft für neue Bereitstellungen empfohlen wird.
> 
> 

Wir raten vom Einsatz einer einzelnen VM für Produktionsworkloads ab, da es für einzelne VMs in Azure keine SLA (Vereinbarung zum Servicelevel) zur Betriebszeit gibt. Um die SLA zu erhalten, müssen Sie mehrere VMs in einer [Verfügbarkeitsgruppe][availability-set] bereitstellen. Weitere Informationen finden Sie unter [Ausführen mehrerer Windows-VMs in Azure][multi-vm]. 

## <a name="architecture-diagram"></a>Architekturdiagramm
Die Bereitstellung einer einzelnen VM in Azure umfasst mehr „bewegliche Teile“ als die VM selbst. Es sind Compute-, Netzwerk- und Speicherelemente vorhanden.  

![[0]][0]

* **Ressourcengruppe:** Eine [*Ressourcengruppe*][resource-manager-overview] ist ein Container, der verwandte Ressourcen für eine Anwendung enthält. Erstellen Sie eine Ressourcengruppe für die Ressourcen dieser VM.
* **VM**. Sie können eine VM über eine Liste mit veröffentlichten Images oder über eine virtuelle Festplattendatei (VHD) bereitstellen, die Sie in Azure Blob Storage hochladen.
* **Betriebssystem-Datenträger:** Der Betriebssystemdatenträger ist eine virtuelle Festplatte (Virtual Hard Disk, VHD), die im [Azure-Speicher][azure-storage] gespeichert ist. Dies bedeutet, dass er auch dann noch vorhanden ist, wenn der Hostcomputer ausfällt.
* **Temporärer Datenträger** Die VM wird mit einem temporären Datenträger erstellt (Laufwerk `D:` unter Windows). Dieser Datenträger wird auf dem Hostcomputer auf einem physischen Laufwerk gespeichert. Er wird *nicht* im Azure-Speicher gespeichert und kann bei Neustarts und anderen Ereignissen während des VM-Lebenszyklus verloren gehen. Verwenden Sie diesen Datenträger nur für temporäre Daten, z. B. Auslagerungsdateien.
* **Datenträger** Bei einem [Datenträger für Daten][data-disk] handelt es sich um eine persistente VHD, die für Anwendungsdaten verwendet wird. Datenträger werden im Azure-Speicher gespeichert, z. B. auf dem Betriebssystem-Datenträger.
* **Virtuelles Netzwerk (VNet) und Subnetz_**  Jede VM in Azure wird in einem VNet bereitgestellt, das weiter in Subnetze unterteilt wird.
* **Öffentliche IP-Adresse** Eine öffentliche IP-Adresse wird für die Kommunikation mit der VM&mdash; benötigt, z. B. per Remotedesktop (RDP).
* **Netzwerkschnittstelle (NIC)** Die NIC ermöglicht der VM die Kommunikation mit dem virtuellen Netzwerk.
* **Netzwerksicherheitsgruppen (NSG)** Die [Netzwerksicherheitsgruppe][nsg] wird verwendet, um den Netzwerkdatenverkehr in das Subnetz zuzulassen oder zu verweigern. Sie können eine NSG einer einzelnen NIC oder einem Subnetz zuordnen. Wenn Sie sie einem Subnetz zuordnen, gelten die NSG-Regeln für alle VMs in diesem Subnetz.
* **Diagnose:**  Diagnoseprotokolle sind für die Verwaltung und Problembehandlung des virtuellen Computers von entscheidender Bedeutung.

## <a name="recommendations"></a>Empfehlungen
### <a name="vm-recommendations"></a>Empfehlungen für virtuelle Computer
* Wir empfehlen die DS- und GS-Serie, es sei denn, Sie haben einen speziellen Workload, z.B. High Performance Computing. Ausführliche Informationen finden Sie unter [Größen virtueller Computer][virtual-machine-sizes]. Starten Sie beim Verlagern eines vorhandenen Workloads in Azure mit der VM-Größe, die Ihren lokalen Servern am ehesten entspricht. Messen Sie dann die Leistung Ihres tatsächlichen Workloads hinsichtlich CPU, Arbeitsspeicher und Datenträger-IOPS (E/A-Vorgänge pro Sekunde), und passen Sie die Größe bei Bedarf an. Wenn Sie mehrere NICs benötigen, beachten Sie zudem den NIC-Grenzwert für jede Größe.  
* Wenn Sie die VM und anderen Ressourcen bereitstellen, müssen Sie einen Standort angeben. Es ist im Allgemeinen ratsam, einen Standort zu wählen, der sich in der Nähe Ihrer internen Benutzer oder Ihrer Kunden befindet. Es sind jedoch nicht alle VM-Größen an allen Standorten verfügbar. Weitere Informationen finden Sie unter [Dienste nach Region][services-by-region]. Führen Sie den folgenden Befehl der Azure-Befehlszeilenschnittstelle (CLI) aus, um die an einem bestimmten Standort verfügbaren VM-Größen aufzulisten:
  
    ```
    azure vm sizes --location <location>
    ```
* Informationen zur Auswahl eines veröffentlichten VM-Image finden Sie unter [Navigieren zu und Auswählen von Images virtueller Windows-Computer in Azure mithilfe von PowerShell oder der Befehlszeilenschnittstelle][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Empfehlungen für Datenträger und Speicher
* Für eine optimale E/A-Leistung empfehlen wir [Storage Premium][premium-storage] zum Speichern von Daten auf SSDs (Solid State Drives). Die Kosten basieren auf der Größe des bereitgestellten Datenträgers. IOPS und Durchsatz richten sich ebenfalls nach der Datenträgergröße. Berücksichtigen Sie beim Bereitstellen eines Datenträgers also alle drei Faktoren (Kapazität, IOPS und Durchsatz). 
* Ein Speicherkonto kann bis zu 20 virtuelle Computer unterstützen.
* Fügen Sie einen oder mehrere Datenträger hinzu. Wenn Sie eine neue virtuelle Festplatte (VHD) erstellen, ist sie nicht formatiert. Melden Sie sich an der VM an, um den Datenträger zu formatieren.
* Wenn Sie über eine große Zahl von Datenträgern verfügen, sollten Sie sich über die E/A-Grenzwerte des Speicherkontos bewusst sein. Weitere Informationen finden Sie unter [Grenzwerte für Datenträger virtueller Computer][vm-disk-limits].
* Erstellen Sie ein separates Speicherkonto zum Speichern der Diagnoseprotokolle, um eine optimale Leistung zu erzielen. Ein standardmäßiger lokal redundanter Speicher (LRS) reicht für Diagnoseprotokolle aus.
* Installieren Sie, sofern möglich, Anwendungen auf einem Datenträger für Daten und nicht auf dem Datenträger für das Betriebssystem. Allerdings müssen einige ältere Anwendungen Komponenten ggf. auf Laufwerk C: installieren. In diesem Fall können Sie mithilfe von PowerShell die [Größe des Betriebssystem-Datenträgers ändern][resize-os-disk].

### <a name="network-recommendations"></a>Netzwerkempfehlungen
* Die öffentliche IP-Adresse kann dynamisch oder statisch sein. Die Standardeinstellung ist „Dynamisch“.
  
  * Reservieren Sie eine [statische IP-Adresse][static-ip], falls Sie eine feste IP-Adresse benötigen, die sich nicht ändert – z.B. wenn Sie einen A-Eintrag in DNS erstellen oder die IP-Adresse auf eine Positivliste setzen müssen.
  * Sie können auch einen vollständig qualifizierten Domänennamen (FQDN) für die IP-Adresse erstellen. Sie können anschließend einen [CNAME-Eintrag][cname-record] in DNS registrieren, der auf den FQDN verweist. Weitere Informationen finden Sie unter [Erstellen eines vollqualifizierten Domänennamens im Azure-Portal][fqdn].
* Alle Netzwerksicherheitsgruppen enthalten eine Reihe von [Standardregeln][nsg-default-rules], einschließlich einer Regel, die den gesamten eingehenden Internetverkehr blockiert. Die Standardregeln können nicht gelöscht, aber von anderen Regeln überschrieben werden. Um Internetdatenverkehr zu ermöglichen, erstellen Sie Regeln, die eingehenden Datenverkehr an bestimmten Ports zulassen, z.B. Port 80 für HTTP.  
* Fügen Sie zum Aktivieren von RDP eine NSG-Regel hinzu, die eingehenden Datenverkehr am TCP-Port 3389 zulässt.

## <a name="scalability-considerations"></a>Überlegungen zur Skalierbarkeit
* Sie können eine VM zentral hoch- oder herunterskalieren, indem Sie [die VM-Größe ändern][vm-resize]. 
* Um horizontal zu skalieren, platzieren Sie zwei oder mehr VMs in einer Verfügbarkeitsgruppe hinter einem Load Balancer. Weitere Informationen finden Sie unter [Ausführen mehrerer Windows-VMs in Azure][multi-vm].

## <a name="availability-considerations"></a>Überlegungen zur Verfügbarkeit
* Wie bereits erwähnt, gibt es keine SLA für eine einzelne VM. Um die SLA zu erhalten, müssen Sie mehrere VMs in einer Verfügbarkeitsgruppe bereitstellen.
* Ihr virtueller Computer kann von einer [geplanten Wartung][planned-maintenance] oder [ungeplanten Wartung][manage-vm-availability] betroffen sein. Sie können [VM-Neustartprotokolle][reboot-logs] verwenden, um zu ermitteln, ob ein VM-Neustart durch einen geplanten Wartungsvorgang verursacht wurde.
* VHDs werden mithilfe von [Azure Storage][azure-storage] gesichert, und der Speicher wird repliziert, um die Dauerhaftigkeit und Verfügbarkeit sicherzustellen.
* Als Schutz vor versehentlichen Datenverlusten während des normalen Betriebs (z.B. aufgrund eines Benutzerfehlers) sollten Sie auch Point-in-Time-Sicherungen implementieren, indem Sie [Blobmomentaufnahmen][blob-snapshot] oder ein anderes Tool verwenden.

## <a name="manageability-considerations"></a>Überlegungen zur Verwaltbarkeit
* **Ressourcengruppen.** Legen Sie eng miteinander verknüpfte Ressourcen mit demselben Lebenszyklus in derselben [Ressourcengruppe][resource-manager-overview] ab. Ressourcengruppen ermöglichen Ihnen das Bereitstellen und Überwachen von Ressourcen und das Zusammenfassen von Abrechnungskosten nach Ressourcengruppe. Sie können auch Ressourcen als Gruppe löschen, was für Testbereitstellungen sehr nützlich ist. Versehen Sie Ressourcen mit aussagekräftigen Namen. Dies vereinfacht das Auffinden einer bestimmten Ressource und Verstehen ihrer Rolle. Siehe [Empfohlene Benennungskonventionen für Azure-Ressourcen][naming conventions].
* **VM-Diagnose.** Aktivieren Sie die Überwachung und Diagnose, einschließlich grundlegender Integritätsmetriken, Infrastrukturprotokolle zur Diagnose sowie [Startdiagnose][boot-diagnostics]. Startdiagnosen dienen dazu, einen Fehler beim Startvorgang zu untersuchen, wenn sich Ihre VM in einem nicht startfähigen Zustand befindet. Weitere Informationen finden Sie unter [Aktivieren von Überwachung und Diagnose][enable-monitoring]. Verwenden Sie die Erweiterung [Azure-Protokollsammlung][log-collector], um Azure-Plattformprotokolle zu erfassen und in den Azure-Speicher hochzuladen.   
  
    Der folgende CLI-Befehl aktiviert die Diagnose:
  
    ```text
    azure vm enable-diag <resource-group> <vm-name>
     ```
* **Beenden einer VM.**  Unter Azure wird zwischen den Zuständen „Stopped“ (Beendet) und „Deallocated“ (Zuordnung aufgehoben) unterschieden. Wenn der VM-Status „Stopped“ lautet, fallen für Sie Kosten an. Wenn der VM-Status „Deallocated“ lautet, fallen für Sie keine Kosten an.
  
    Verwenden Sie den folgenden CLI-Befehl, um die Zuordnung für eine VM aufzuheben:
  
    ```text
    azure vm deallocate <resource-group> <vm-name>
    ```
  
    Sie können die Zuordnung des virtuellen Computers auch mit der Schaltfläche **Beenden** im Azure-Portal aufheben. Wenn das Herunterfahren über das Betriebssystem erfolgt, während Sie angemeldet sind, wird der virtuelle Computer zwar beendet, aber die Zuordnung wird *nicht* aufgehoben. Es fallen also weiter Kosten an.
* **Löschen einer VM.**  Wenn Sie eine VM löschen, werden die VHDs nicht gelöscht. Dies bedeutet, dass Sie die VM problemlos löschen können, ohne dass Daten verloren gehen. Allerdings wird Ihnen der Speicherplatz weiter in Rechnung gestellt. Um die VHD zu löschen, löschen Sie die Datei aus dem [Blobspeicher][blob-storage].
  
  Zur Verhinderung des versehentlichen Löschens verwenden Sie eine [Ressourcensperre][resource-lock], um die gesamte Ressourcengruppe oder einzelne Ressourcen, z. B. den virtuellen Computer, zu sperren. 

## <a name="security-considerations"></a>Sicherheitshinweise
* Verwenden Sie [Azure Security Center][security-center], um sich eine zentrale Übersicht über den Sicherheitszustand Ihrer Azure-Ressourcen zu verschaffen. Mit Security Center werden potenzielle Sicherheitsprobleme überwacht, z.B. Systemupdates, und Antischadsoftware, und Sie erhalten eine umfassende Darstellung des Sicherheitszustands Ihrer Bereitstellung. 
  
  * Security Center wird für jedes Azure-Abonnement individuell konfiguriert. Aktivieren Sie die Sammlung von Sicherheitsdaten, wie unter [Verwenden von Security Center]beschrieben.
  * Nachdem die Datensammlung aktiviert wurde, durchsucht Security Center VMs automatisch, die unter diesem Abonnement erstellt werden.
* **Patchverwaltung.**  Falls aktiviert, prüft Security Center, ob kritische und Sicherheitsupdates fehlen. Verwenden Sie [Gruppenrichtlinieneinstellungen][group-policy] auf dem virtuellen Computer, um automatische Systemupdates zu aktivieren.
* **Antimalware.**  Falls aktiviert, prüft Security Center, ob Antischadsoftware installiert ist. Sie können auch das Security Center nutzen, um Antischadsoftware über das Azure-Portal zu installieren.
* Arbeiten Sie mit der [rollenbasierten Zugriffssteuerung][rbac] (Role-Based Access Control, RBAC) zum Steuern des Zugriffs auf die Azure-Ressourcen, die Sie bereitstellen. Mithilfe der RBAC können Sie Mitglieder Ihres DevOps-Teams Autorisierungsrollen zuweisen. Die Rolle „Leser“ kann beispielsweise Azure-Ressourcen anzeigen, diese aber nicht erstellen, verwalten oder löschen. Einige Rollen sind für bestimmte Azure-Ressourcentypen spezifisch. Die VM-Rolle „Mitwirkender“ kann z.B. eine VM neu starten oder Ihre Zuordnung aufheben, das Administratorkennwort zurücksetzen, eine neue VM erstellen usw. Andere [integrierte RBAC-Rollen][rbac], die für diese Referenzarchitektur nützlich sein können, sind u. a. [DevTest Labs-Benutzer][rbac-devtest] und [Netzwerkmitwirkender][rbac-network]. Ein Benutzer kann mehreren Rollen zugewiesen werden. Außerdem können Sie für noch präzisere Berechtigungen benutzerdefinierte Rollen erstellen.
  
  > [!NOTE]
  > Die RBAC schränkt nicht die Aktionen eines Benutzers ein, der bei einer VM angemeldet ist. Diese Berechtigungen werden vom Kontotyp im Gastbetriebssystem bestimmt.   
  > 
  > 
* Führen Sie zum Zurücksetzen des lokalen Administratorkennworts den Azure-CLI-Befehl `vm reset-access` aus.
  
    ```text
    azure vm reset-access -u <user> -p <new-password> <resource-group> <vm-name>
    ```
* Verwenden Sie [Überwachungsprotokolle][audit-logs], um Bereitstellungsaktionen und andere VM-Ereignisse anzuzeigen.
* Ziehen Sie [Azure Disk Encryption][disk-encryption] in Betracht, wenn Sie die Datenträger für Betriebssystem und Daten verschlüsseln müssen. 

## <a name="solution-deployment"></a>Bereitstellung von Lösungen
Die Beispielbereitstellung in dieser Anleitung verwendet drei verschiedene [Vorlagenbausteine][blocks] zum Erstellen von:

* eines virtuellen Netzwerks
* einer Netzwerksicherheitsgruppe (NSG)
* eines virtuellen Computers (VM)

Diese Referenzarchitektur verwendet eine einzelne Ressourcengruppe, die Sie durch Klicken auf die folgende Schaltfläche und Akzeptieren der Standardwerte für alle Parameter bereitstellen können.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

### <a name="customize-the-deployment"></a>Anpassen der Bereitstellung
Wenn Sie die Bereitstellung an Ihre Anforderungen anpassen müssen, befolgen Sie die Anweisungen auf der Seite mit der [guidance-single-vm][Infodatei]. 

## <a name="next-steps"></a>Nächste Schritte
Damit die [SLA für virtuelle Computer][vm-sla] gilt, müssen Sie in einer Verfügbarkeitsgruppe mindestens zwei Instanzen bereitstellen. Weitere Informationen finden Sie unter [Ausführen mehrerer VMs in Azure][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]: ../articles/virtual-machines/virtual-machines-windows-create-availability-set.md
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-windows-about-disks-vhds.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/azure-portal/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-windows-portal-create-fqdn.md
[group-policy]: https://technet.microsoft.com/en-us/library/dn595129.aspx
[log-collector]: https://azure.microsoft.com/en-us/blog/simplifying-virtual-machine-troubleshooting-using-azure-log-collector/
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-windows-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[Benennungskonventionen]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-windows-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[resize-os-disk]: ../articles/virtual-machines/virtual-machines-windows-expand-os-disk.md
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Ändern der Größe des virtuellen Computers]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/resource-group-overview.md
[security-center]: https://azure.microsoft.com/en-us/services/security-center/
[select-vm-image]: ../articles/virtual-machines/virtual-machines-windows-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[Verwenden von Security Center]: ../articles/security-center/security-center-get-started.md#use-security-center
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-windows-sizes.md
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_0/
[ARM-Templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm/Deploy-ReferenceArchitecture.ps1
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm/parameters/windows/virtualNetwork.parameters.json
[nsg-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm/parameters/windows/networkSecurityGroups.parameters.json
[vm-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm/parameters/windows/virtualMachine.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[0]: ./media/guidance-blueprints/compute-single-vm.png "Architektur einer einzelnen Windows-VM in Azure"
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[blocks]: https://github.com/mspnp/template-building-blocks


<!--HONumber=Oct16_HO2-->


