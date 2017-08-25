# <a name="azure-managed-disks-overview"></a>Azure Managed Disks – Übersicht

Azure Managed Disks vereinfacht die Datenträgerverwaltung für Azure IaaS-VMs, indem die [Speicherkonten](../articles/storage/common/storage-introduction.md) verwaltet werden, die den VM-Datenträgern zugeordnet sind. Sie müssen nur die Art ([Premium](../articles/storage/common/storage-premium-storage.md) oder [Standard](../articles/storage/common/storage-standard-storage.md)) und die benötigte Größe des Datenträgers angeben. Azure erstellt und verwaltet ihn dann für Sie.

## <a name="benefits-of-managed-disks"></a>Vorteile von verwalteten Datenträgern

Werfen wir einen Blick auf einige der Vorteile, von denen Sie durch die Verwendung verwalteter Datenträger profitieren, indem wir mit dem Channel 9-Video [Bessere Resilienz der Azure-VM durch Managed Disks](https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency) beginnen.
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency/player]

### <a name="simple-and-scalable-vm-deployment"></a>Einfache und skalierbare VM-Bereitstellung

Managed Disks verwaltet die Speicherung für Sie im Hintergrund. Bisher mussten Sie Speicherkonten für die Datenträger (VHD-Dateien) Ihrer Azure-VMs erstellen. Beim zentralen Hochskalieren mussten Sie sicherstellen, dass zusätzliche Speicherkonten erstellt wurden, um den IOPS-Speichergrenzwert für Ihre Datenträger nicht zu überschreiten. Wenn Managed Disks die Verwaltung des Speichers übernimmt, gelten für Sie die Speicherkonto-Grenzwerte (z.B. 20.000 IOPS/Konto) nicht mehr. Außerdem ist es nicht mehr erforderlich, Ihre benutzerdefinierten Images (VHD-Dateien) in mehrere Speicherkonten zu kopieren. Sie können sie an einem zentralen Ort verwalten – ein Speicherkonto pro Azure-Region – und nutzen, um Hunderte von VMs unter einem Abonnement zu erstellen.

Mit Managed Disks können Sie für ein Abonnement bis zu 10.000 VM-**Datenträger** erstellen – und für ein Abonnement somit Tausende von **VMs**. Außerdem wird mit diesem Feature die Skalierbarkeit von [VM-Skalierungsgruppen (VMSS)](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) weiter erhöht, sodass Sie per Marketplace-Image bis zu 1.000 VMs in einer VMSS erstellen können.

### <a name="better-reliability-for-availability-sets"></a>Bessere Zuverlässigkeit für Verfügbarkeitsgruppen

Managed Disks ermöglicht eine bessere Zuverlässigkeit für Verfügbarkeitsgruppen, indem sichergestellt wird, dass die Datenträger von [VMs in einer Verfügbarkeitsgruppe](../articles/virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) ausreichend voneinander isoliert sind, um Ausfälle einzelner Komponenten zu vermeiden. Hierzu werden die Datenträger automatisch in verschiedenen Speicherskalierungseinheiten („Stamps“) angeordnet. Wenn ein Stamp aufgrund eines Hardware- oder Softwarefehlers ausfällt, treten nur für die VM-Instanzen mit Datenträgern auf diesen Stamps Fehler auf. Angenommen, Sie führen eine Anwendung auf fünf VMs aus, und die VMs sind in einer Verfügbarkeitsgruppe enthalten. Die Datenträger für diese VMs werden nicht alle auf demselben Stamp gespeichert, damit die anderen Instanzen der Anwendung weiter ausgeführt werden, wenn ein Stamp ausfällt.

### <a name="highly-durable-and-available"></a>Hoch verfügbar und stabil

Azure-Datenträger sind auf eine Verfügbarkeit von 99,999% ausgelegt. Schlafen Sie ruhiger in dem Bewusstsein, dass Sie drei Replikate Ihrer Daten besitzen und so eine hohe Dauerhaftigkeit gewährleisten. Wenn bei einem oder sogar bei zwei Ihrer Replikate Probleme auftreten, stellen Sie mit den übrigen Replikaten die Persistenz Ihrer Daten und eine hohe Fehlertoleranz sicher. Durch diese Architektur konnte Azure für IaaS-Datenträger durchgängig eine Dauerhaftigkeit auf Unternehmensniveau bereitstellen, mit einer branchenweit führenden auf das Jahr umgerechneten Fehlerrate von 0 %. 

### <a name="granular-access-control"></a>Genau abgestimmte Zugriffssteuerung

Sie können die [Rollenbasierte Zugriffssteuerung in Azure (RBAC)](../articles/active-directory/role-based-access-control-what-is.md) verwenden, um die spezifischen Berechtigungen für einen verwalteten Datenträger einem oder mehreren Benutzern zuzuweisen. Managed Disks bietet viele verschiedene Vorgänge, z.B. Lesen, Schreiben (Erstellen/Aktualisieren), Löschen und Abrufen eines [SAS-URI (Shared Access Signature)](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md) für den Datenträger. Sie haben die Möglichkeit, Personen nur Zugriff auf die Vorgänge zu gewähren, die sie jeweils benötigen, um ihre Aufgaben zu erledigen. Wenn Sie es für eine Person beispielsweise nicht zulassen möchten, dass sie einen verwalteten Datenträger auf ein Speicherkonto kopiert, können Sie festlegen, dass der Zugriff auf die Exportaktion für diesen verwalteten Datenträger nicht gewährt wird. Wenn Sie nicht möchten, dass eine Person einen SAS-URI zum Kopieren eines verwalteten Datenträgers verwendet, können Sie auch festlegen, dass diese Berechtigung für den verwalteten Datenträger nicht gewährt wird.

### <a name="azure-backup-service-support"></a>Azure Backup-Dienst – Unterstützung
Sie können den Azure Backup-Dienst in Kombination mit Managed Disks verwenden, um einen Sicherungsauftrag mit zeitbasierten Sicherungen, unkomplizierter Wiederherstellung von virtuellen Computern und Aufbewahrungsrichtlinien für Sicherungen zu erstellen. Managed Disks unterstützen nur den lokal redundanten Speicher (LRS) als Replikationsoption. Dies bedeutet, dass innerhalb einer Region drei Kopien der Daten beibehalten werden. Für eine regionale Notfallwiederherstellung müssen Sie Ihre VM-Datenträger mit dem [Azure Backup-Dienst](../articles/backup/backup-introduction-to-azure-backup.md) und einem GRS-Speicherkonto als Sicherungstresor in einer anderen Region sichern. Aktuell unterstützt Azure Backup Datenträgergrößen von bis zu 1 TB für die Sicherung. Weitere Informationen hierzu finden Sie im Artikel zur [Verwenden des Azure Backup-Diensts für virtuelle Computer mit Managed Disks](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="pricing-and-billing"></a>Preise und Abrechnung

Bei der Verwendung von Managed Disks gelten die folgenden Abrechnungsaspekte:
* Speichertyp

* Datenträgergröße

* Anzahl von Transaktionen

* Ausgehende Datenübertragungen

* Managed Disk-Momentaufnahmen (vollständige Datenträgerkopie)

Wir sehen uns die einzelnen Aspekte nun genauer an.

**Speichertyp:** Managed Disks verfügt über zwei Leistungsstufen: [Premium](../articles/storage/common/storage-premium-storage.md) (SSD-basiert) und [Standard](../articles/storage/common/storage-standard-storage.md) (HDD-basiert). Die Abrechnung für einen verwalteten Datenträger richtet sich danach, welchen Speichertyp Sie für den Datenträger ausgewählt haben.


**Datenträgergröße**: Die Abrechnung für verwaltete Datenträger richtet sich nach der bereitgestellten Datenträgergröße. Azure ordnet die bereitgestellte Größe (aufgerundet) der nächstgelegenen Managed Disks-Option zu. Dies ist in den Tabellen unten angegeben. Jeder verwaltete Datenträger wird einer der unterstützten bereitgestellten Größen zugeordnet und entsprechend abgerechnet. Wenn Sie beispielsweise einen verwalteten Standard-Datenträger erstellen und eine bereitgestellte Größe von 200 GB angeben, wird die Abrechnung gemäß den Preisen für den Datenträgertyp S20 durchgeführt.

Hier sind die Datenträgergrößen aufgeführt, die für einen verwalteten Premium-Datenträger verfügbar sind:

| **Datenträgertyp<br>Premium – verwaltet** | **P4** | **P6** |**P10** | **P20** | **P30** | **P40** | **P50** | 
|------------------|---------|---------|---------|---------|----------------|----------------|----------------|  
| Datenträgergröße        | 32 GB   | 64 GB   | 128 GB  | 512 GB  | 1024 GB (1 TB) | 2048 GB (2 TB) | 4095 GB (4 TB) | 


Hier sind die Datenträgergrößen aufgeführt, die für einen verwalteten Standard-Datenträger verfügbar sind:

| **Datenträgertyp<br>Standard – verwaltet** | **S4** | **S6** | **S10** | **S20** | **S30** | **S40** | **S50** |
|------------------|---------|---------|--------|--------|----------------|----------------|----------------| 
| Datenträgergröße        | 32 GB   | 64 GB   | 128 GB | 512 GB | 1024 GB (1 TB) | 2048 GB (2 TB) | 4095 GB (4 TB) | 


**Anzahl von Transaktionen**: Ihnen wird die Anzahl von Transaktionen berechnet, die Sie für einen verwalteten Standard-Datenträger durchführen. Für einen verwalteten Premium-Datenträger fallen keine Kosten für Transaktionen an.

**Ausgehende Datenübertragungen:**[Ausgehende Datenübertragungen](https://azure.microsoft.com/pricing/details/data-transfers/) (Daten, die von den Azure-Rechenzentren ausgehen) verursachen Kosten bei der Bandbreitenverwendung.

Ausführliche Informationen zu Preisen für Managed Disks finden Sie unter [Managed Disks Preise](https://azure.microsoft.com/pricing/details/managed-disks).


## <a name="managed-disk-snapshots"></a>Momentaufnahmen von verwalteten Datenträgern

Bei einer Momentaufnahme für einen verwalteten Datenträger handelt es sich um eine schreibgeschützte vollständige Kopie eines verwalteten Datenträgers, die standardmäßig als verwalteter Standarddatenträger gespeichert wird. Mit Momentaufnahmen können Sie Ihre verwalteten Datenträger jederzeit sichern. Diese Momentaufnahmen existieren unabhängig vom Quelldatenträger und können zum Erstellen von neuen verwalteten Datenträgern verwendet werden. Sie werden auf Basis der verwendeten Größe in Rechnung gestellt. Wenn Sie beispielsweise eine Momentaufnahme eines verwalteten Datenträgers mit einer bereitgestellten Kapazität von 64 GB und einer tatsächlichen Datengröße von 10 GB erstellen, wird die Momentaufnahme nur für die in Anspruch genommene Datengröße von 10 GB in Rechnung gestellt.  

Für Managed Disks werden derzeit keine [inkrementellen Momentaufnahmen](../articles/virtual-machines/windows/incremental-snapshots.md) unterstützt, aber dies ist bereits geplant.

Weitere Informationen dazu, wie Sie Momentaufnahmen mit Managed Disks erstellen, finden Sie unter diesen Ressourcen:

* [Erstellen einer Kopie einer als verwalteter Datenträger gespeicherten virtuellen Festplatte mithilfe von Momentaufnahmen unter Windows](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Erstellen einer Kopie einer als verwalteter Datenträger gespeicherten virtuellen Festplatte mithilfe von Momentaufnahmen unter Linux](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)


## <a name="images"></a>Bilder

Für Managed Disks wird auch die Erstellung eines verwalteten benutzerdefinierten Image unterstützt. Sie können ein Image aus Ihrer benutzerdefinierten VHD in einem Speicherkonto oder direkt von einer generalisierten VM (mit Systemvorbereitung) erstellen. Hierbei werden in einem Image alle verwalteten Datenträger zusammengefasst, die einer VM zugeordnet sind, einschließlich der Datenträger für das Betriebssystem und für die Daten. Dies ermöglicht Ihnen die Erstellung von Hunderten von VMs mit Ihrem benutzerdefinierten Image, ohne dass Sie Speicherkonten kopieren oder verwalten müssen.

Informationen zur Erstellung von Images finden Sie in den folgenden Artikeln:
* [How to capture a managed image of a generalized VM in Azure](../articles/virtual-machines/windows/capture-image-resource.md) (Erstellen eines verwalteten Image eines generalisierten virtuellen Computers in Azure)
* [Generalisieren und Erfassen eines virtuellen Linux-Computers mithilfe von Azure CLI 2.0](../articles/virtual-machines/linux/capture-image.md)

## <a name="images-versus-snapshots"></a>Vergleich von Images und Momentaufnahmen

Der Begriff „Image“ wird in Verbindung mit VMs bereits häufig verwendet, und jetzt kommen noch die „Momentaufnahmen“ hinzu. Es ist wichtig, dass Ihnen der Unterschied klar ist. Bei Managed Disks können Sie ein Image einer generalisierten VM erstellen, deren Zuordnung aufgehoben wurde. Dieses Image umfasst alle Datenträger, die an die VM angefügt sind. Sie können dieses Image verwenden, um eine neue VM mit allen Datenträgern zu erstellen.

Eine Momentaufnahme ist eine Kopie eines Datenträgers zum Zeitpunkt der Erstellung. Sie gilt nur für einen Datenträger. Wenn Sie eine VM mit nur einem Datenträger (Betriebssystem) verwenden, können Sie eine Momentaufnahme oder ein Image davon erstellen und dann entweder aus der Momentaufnahme oder dem Image eine VM erstellen.

Was passiert, wenn Sie eine VM mit fünf Stripesetdatenträgern verwenden? Sie können eine Momentaufnahme für jeden Datenträger erstellen, aber auf der VM liegen keine Informationen zum Zustand der Datenträger vor. Für die Momentaufnahmen sind nur Informationen zum jeweiligen Datenträger vorhanden. In diesem Fall müssen die Momentaufnahmen miteinander koordiniert werden, und dies wird derzeit nicht unterstützt.

## <a name="managed-disks-and-encryption"></a>Managed Disks und Verschlüsselung

Es gibt zwei Arten von Verschlüsselung, die im Zusammenhang mit verwalteten Datenträgern besprochen werden müssen. Die erste ist Storage Service Encryption (SSE), die vom Speicherdienst durchgeführt wird. Die zweite ist Azure Disk Encryption, die Sie für Datenträger für das Betriebssystem und die Daten Ihrer virtuellen Computer aktivieren können.

### <a name="storage-service-encryption-sse"></a>Storage Service Encryption (SSE)

[Azure Storage Service Encryption](../articles/storage/common/storage-service-encryption.md) bietet Verschlüsselung für ruhende Daten und schützt Ihre Daten, um die Sicherheits- und Konformitätsverpflichtungen Ihrer Organisation zu erfüllen. SSE ist standardmäßig für alle verwalteten Festplatten, Momentaufnahmen und Images in allen Regionen aktiviert, in denen verwaltete Datenträger vorhanden sind. Ab dem 10. Juni 2017 werden alle neuen verwalteten Datenträger, Momentaufnahmen bzw. Images und neuen Daten, die auf die vorhandenen verwalteten Datenträger geschrieben wurden, automatisch mit den von Microsoft verwalteten Schlüsseln verschlüsselt.  Weitere Einzelheiten finden Sie auf der Seite mit [FAQs zu Managed Disks](../articles/virtual-machines/windows/faq-for-disks.md#managed-disks-and-storage-service-encryption).


### <a name="azure-disk-encryption-ade"></a>Azure Disk Encryption (ADE)

Mit Azure Disk Encryption können Sie die Betriebssystemdatenträger und andere Datenträger verschlüsseln, die von einem virtuellen IaaS-Computer verwendet werden. Dies schließt verwaltete Datenträger ein. Unter Windows werden Laufwerke mit branchenüblicher BitLocker-Verschlüsselung verschlüsselt. Unter Linux werden Datenträger mit der DM-Crypt-Technologie verschlüsselt. Diese ist in Azure Key Vault integriert, damit Sie die Datenträger-Verschlüsselungsschlüssel steuern und verwalten können. Weitere Informationen finden Sie unter [Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer](../articles/security/azure-security-disk-encryption.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Managed Disks finden Sie in den folgenden Artikeln.

### <a name="get-started-with-managed-disks"></a>Erste Schritte mit verwalteten Datenträgern

* [Erstellen eines virtuellen Computers mithilfe von Resource Manager und PowerShell](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md)

* [Erstellen eines virtuellen Linux-Computers mit Azure CLI 2.0](../articles/virtual-machines/linux/quick-create-cli.md)

* [Attach a data disk to a Windows VM using PowerShell](../articles/virtual-machines/windows/attach-disk-ps.md) (Anfügen eines Datenträgers an einen virtuellen Windows-Computer mithilfe von PowerShell)

* [Hinzufügen eines verwalteten Datenträgers zu einem virtuellen Linux-Computer](../articles/virtual-machines/linux/add-disk.md)

* [Managed Disks – PowerShell-Beispielskripts](https://github.com/Azure-Samples/managed-disks-powershell-getting-started)

* [Verwenden von Managed Disks in Resource Manager-Vorlagen](../articles/virtual-machines/windows/using-managed-disks-template-deployments.md)

### <a name="compare-managed-disks-storage-options"></a>Vergleichen der Managed Disks-Speicheroptionen

* [Storage Premium und Datenträger](../articles/storage/common/storage-premium-storage.md)

* [Standard-Speicher und -Datenträger](../articles/storage/common/storage-standard-storage.md)

### <a name="operational-guidance"></a>Leitfaden

* [Migrieren von AWS und anderen Plattformen zu Managed Disks in Azure](../articles/virtual-machines/windows/on-prem-to-azure.md)

* [Migrate Azure VMs to Managed Disks in Azure](../articles/virtual-machines/windows/migrate-to-managed-disks.md) (Migrieren von Azure-VMs zu Managed Disks in Azure)
