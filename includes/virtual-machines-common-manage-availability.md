## <a name="understand-planned-vs-unplanned-maintenance"></a>Verstehen geplanter und nicht geplanter Wartung
Es gibt zwei Arten von Microsoft Azure Platform-Ereignissen, die sich auf die Verfügbarkeit von virtuellen Computern auswirken können: geplante und ungeplante Wartung.

* **Geplante Wartungsereignisse** sind regelmäßige Updates, die von Microsoft für die zugrunde liegende Azure-Plattform ausgeführt werden können, um die gesamte Verfügbarkeit, Leistung und Sicherheit der Plattforminfrastruktur, unter der die virtuellen Computer ausgeführt werden, zu verbessern. Die meisten dieser Updates werden ohne Auswirkungen auf die virtuellen Computer oder Clouddienste ausgeführt. Es gibt jedoch Instanzen, bei denen diese Updates einen Neustart des virtuellen Computers erfordern, um die erforderlichen Updates auf die Plattforminfrastruktur anzuwenden.
* **Ungeplante Wartungsereignisse** treten auf, wenn die Hardware oder physische Infrastruktur, die dem virtuellen Computer zugrunde liegt, einen Ausfall verursacht. Diese können Ausfälle des lokalen Netzwerks, Datenträgers oder andere Fehler auf Rackebene umfassen. Wenn ein solcher Fehler festgestellt wird, migriert die Azure-Plattform Ihren virtuellen Computer automatisch vom fehlerhaften physischen Computer, der den virtuellen Computer hostet, zu einem fehlerfreien physischen Computer. Diese Ereignisse treten selten auf, verursachen jedoch u. U. den Neustart des virtuellen Computers.

Um die Downtime aufgrund eines oder mehrerer dieser Ereignisse zu verringern, sollten Sie die folgenden bewährten Methoden befolgen, um die hohe Verfügbarkeit virtueller Computer zu gewährleisten:

* [Konfigurieren mehrerer virtueller Computer in einer Verfügbarkeitsgruppe für höhere Redundanz]
* [Konfigurieren einzelner Anwendungsebenen in separaten Verfügbarkeitsgruppen]
* [Kombinieren des Lastenausgleichs mit Verfügbarkeitsgruppen]
* [Verwenden von Managed Disks für VMs in einer Verfügbarkeitsgruppe]

## <a name="configure-multiple-virtual-machines-in-an-availability-set-for-redundancy"></a>Konfigurieren mehrerer virtueller Computer in einer Verfügbarkeitsgruppe für höhere Redundanz
Um Redundanz für Ihre Anwendung zu gewährleisten, empfehlen wir die Gruppierung von zwei oder mehr virtuellen Computern in einer Verfügbarkeitsgruppe. Durch diese Konfiguration wird sichergestellt, dass während eines geplanten oder ungeplanten Wartungsereignisses mindestens ein virtueller Computer verfügbar ist und die von der Azure-SLA zugesicherte Verfügbarkeit von 99,95 Prozent eingehalten wird. Weitere Informationen finden Sie unter [SLA für Virtual Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Vermeiden Sie es, virtuelle Computer, die eine Einzelinstanz darstellen, alleine einer Verfügbarkeitsgruppe zuzuordnen. Virtuelle Computer in dieser Konfiguration erfüllen nicht die Anforderungen für eine zugesicherte SLA und sind während geplanter Azure-Wartungsereignisse nicht verfügbar – es sei denn, ein einzelner virtueller Computer verwendet [Azure Storage Premium](../articles/storage/storage-premium-storage.md). Für einzelne virtuelle Computer mit Storage Premium gilt die Azure-SLA.

Jeder virtuelle Computer in der Verfügbarkeitsgruppe wird einer **Updatedomäne** (UD) und einer **Fehlerdomäne** (FD) der zugrunde liegenden Azure-Plattform zugewiesen. Für eine bestimmte Verfügbarkeitsgruppe werden fünf nicht von Benutzern konfigurierbare Updatedomänen standardmäßig zugewiesen (Resource Manager-Bereitstellungen können dann vergrößert werden, um bis zu 20 Updatedomänen bereitzustellen), um die Gruppen mit virtuellen Computern und die zugehörigen physischen Hardwareelemente zu kennzeichnen, die gleichzeitig neu gestartet werden können. Wenn innerhalb einer Verfügbarkeitsgruppe mehr als fünf virtuelle Computer konfiguriert sind, wird der sechste virtuelle Computer in derselben Updatedomäne wie der erste virtuelle Computer gespeichert, der siebte in derselben Updatedomäne wie der zweite virtuelle Computer usw. Während einer geplanten Wartung werden die Updatedomänen unter Umständen nicht der Reihe nach neu gestartet, sondern es wird jeweils nur eine Updatedomäne neu gestartet.

Mit Fehlerdomänen wird die Gruppe der virtuellen Computer definiert, die eine Stromquelle und einen Netzwerkswitch gemeinsam nutzen. Standardmäßig sind die innerhalb Ihrer Verfügbarkeitsgruppe konfigurierten virtuellen Computer auf bis zu drei Fehlerdomänen bei Resource Manager-Bereitstellungen verteilt (zwei Fehlerdomänen bei klassischen Bereitstellungen). Auch wenn Verfügbarkeitsgruppen Ihre Anwendung nicht gänzlich vor Fehlern des Betriebssystems oder der Anwendung selbst schützen können, verringern sie doch die Auswirkungen von potenziellen Hardwarefehlern, Netzwerkausfällen oder Stromunterbrechungen.

<!--Image reference-->
   ![Schematische Darstellung der Konfiguration mit Updatedomäne und Fehlerdomäne](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

### <a name="managed-disk-fault-domains-and-availability-sets"></a>Fehlerdomänen und Verfügbarkeitsgruppen für verwaltete Datenträger
Virtuelle Computer mit [Azure Managed Disks](../articles/storage/storage-faq-for-disks.md) werden bei Verwendung einer verwalteten Verfügbarkeitsgruppe auf Fehlerdomänen für verwaltete Datenträger ausgerichtet. Diese Ausrichtung stellt sicher, dass sich alle verwalteten, an einen virtuellen Computer angefügten Datenträger innerhalb der gleichen Fehlerdomäne für verwaltete Datenträger befinden. Nur virtuelle Computer mit verwalteten Datenträgern können in einer verwalteten Verfügbarkeitsgruppe erstellt werden. Die Anzahl der Fehlerdomänen für verwaltete Datenträger variiert je nach Region – pro Region sind entweder zwei oder drei Fehlerdomänen für verwaltete Datenträger vorhanden.


## <a name="configure-each-application-tier-into-separate-availability-sets"></a>Konfigurieren einzelner Anwendungsebenen in separaten Verfügbarkeitsgruppen
Wenn Ihre virtuellen Computer fast alle identisch sind und die Anwendung auf die gleiche Weise unterstützen, wird empfohlen, für jede einzelne Anwendungsebene eine Verfügbarkeitsgruppe zu konfigurieren.  Wenn Sie eine Verfügbarkeitsgruppe mit zwei verschiedenen Ebenen anlegen, können alle virtuellen Computer derselben Anwendungsebene zur gleichen Zeit neu gestartet werden. Indem Sie mindestens zwei virtuelle Computer in einer Verfügbarkeitsgruppe für jede Ebene konfigurieren, gewährleisten Sie, dass mindestens ein virtueller Computer pro Ebene verfügbar ist.

Beispielsweise können Sie alle VMs aus dem Front-End der Anwendung, auf denen IIS, Apache und Nginx ausgeführt wird, einer einzelnen Verfügbarkeitsgruppe zuordnen. Stellen Sie sicher, dass derselben Verfügbarkeitsgruppe nur virtuelle Front-End-Computer zugeordnet werden. Stellen Sie analog dazu sicher, dass virtuelle Computer der Datenebene ihre eigene Verfügbarkeitsgruppe haben. Dazu gehören beispielsweise virtuelle SQL Server-Computer oder virtuelle MySQL-Computer.

<!--Image reference-->
   ![Anwendungsebenen](./media/virtual-machines-common-manage-availability/application-tiers.png)

## <a name="combine-a-load-balancer-with-availability-sets"></a>Kombinieren des Lastenausgleichs mit Verfügbarkeitsgruppen
Kombinieren Sie den Azure-Lastenausgleich ( [Azure Load Balancer](../articles/load-balancer/load-balancer-overview.md) ) mit einer Verfügbarkeitsgruppe, um höchste Anwendungsresilienz zu erzielen. Der Azure-Lastenausgleich verteilt den Datenverkehr auf mehrere virtuelle Computer. In die virtuellen Computer der Standardebene ist der Azure-Lastenausgleich bereits integriert. Der Azure Load Balancer ist aber nicht auf allen Ebenen des virtuellen Computers verfügbar. Weitere Informationen zum Lastenausgleich zwischen virtuellen Computern finden Sie unter [Lastenausgleich zwischen virtuellen Computern](../articles/virtual-machines/virtual-machines-linux-load-balance.md).

Wenn der Lastenausgleich nicht für die gleichmäßige Verteilung des Datenverkehrs auf mehrere virtuelle Computer konfiguriert ist, wirkt sich ein geplantes Wartungsereignis schließlich auf den einzigen virtuellen Computer, der den Datenverkehr aufrechterhält, aus und führt zu einem Ausfall der Anwendungsebene. Werden dagegen mehrere virtuelle Computer derselben Ebene demselben Lastenausgleich und derselben Verfügbarkeitsgruppe zugeordnet, wird der Datenverkehr kontinuierlich von mindestens einer Instanz aufrechterhalten.

## <a name="use-managed-disks-for-vms-in-availability-set"></a>Verwenden von Managed Disks für VMs in einer Verfügbarkeitsgruppe
Falls Sie derzeit VMs mit nicht verwalteten Datenträgern verwenden, empfehlen wir Ihnen dringend, [VMs in der Verfügbarkeitsgruppe für die Verwendung von Managed Disks zu konvertieren](../articles/virtual-machines/windows/convert-unmanaged-to-managed-disks.md#convert-vms-in-an-availability-set-to-managed-disks-in-a-managed-availability-set).

[Managed Disks](../articles/storage/storage-managed-disks-overview.md) ermöglicht eine bessere Zuverlässigkeit für Verfügbarkeitsgruppen, indem sichergestellt wird, dass die Datenträger von VMs in einer Verfügbarkeitsgruppe ausreichend voneinander isoliert sind, um einzelne Fehlerquellen zu vermeiden. Hierzu werden die Datenträger automatisch in verschiedenen Speicherskalierungseinheiten („Stamps“) angeordnet. Wenn ein Stamp aufgrund eines Hardware- oder Softwarefehlers ausfällt, treten nur für die VM-Instanzen mit Datenträgern auf diesen Stamps Fehler auf. 

Gehen Sie wie folgt vor, wenn Sie planen, VMs mit [nicht verwalteten Datenträgern](../articles/storage/storage-about-disks-and-vhds-windows.md#types-of-disks) zu verwenden: Halten Sie sich an die unten angegebenen bewährten Methoden für Storage-Konten, bei denen virtuelle Festplatten (VHDs) von VMs als [Seitenblobs](https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs) gespeichert werden. 

1. **Alle Datenträger (Betriebssystem und Daten) müssen einem virtuellen Computer im selben Speicherkonto zugeordnet sein**
2. **Überprüfen Sie die [Grenzwerte](../articles/storage/storage-scalability-targets.md) für die Anzahl von nicht verwalteten Datenträgern eines Storage-Kontos**, bevor Sie einem Speicherkonto weitere VHDs hinzufügen.
3. **Verwenden Sie mehrere Speicherkonten für jeden virtuellen Computer in einer Verfügbarkeitsgruppe.** Geben Sie Storage-Konten mit mehreren VMs in derselben Verfügbarkeitsgruppe nicht für die gemeinsame Nutzung frei. Für VMs, die über verschiedene Verfügbarkeitsgruppen verteilt sind, können Speicherkonten freigegeben werden, solange die oben genannten bewährten Methoden befolgt werden.

<!-- Link references -->
[Konfigurieren mehrerer virtueller Computer in einer Verfügbarkeitsgruppe für höhere Redundanz]: #configure-multiple-virtual-machines-in-an-availability-set-for-redundancy
[Konfigurieren einzelner Anwendungsebenen in separaten Verfügbarkeitsgruppen]: #configure-each-application-tier-into-separate-availability-sets
[Kombinieren des Lastenausgleichs mit Verfügbarkeitsgruppen]: #combine-a-load-balancer-with-availability-sets
[Avoid single instance virtual machines in availability sets]: #avoid-single-instance-virtual-machines-in-availability-sets
[Verwenden von Managed Disks für VMs in einer Verfügbarkeitsgruppe]: #use-managed-disks-for-vms-in-availability-set

