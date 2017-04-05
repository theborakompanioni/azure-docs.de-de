
## <a name="about-vhds"></a>Informationen zu VHDs

Die in Azure verwendeten VHDs sind VHD-Dateien, die als Seiten-Blobs in einem Standard- oder Premium-Speicherkonto in Azure gespeichert sind. Informationen zu Seitenblobs finden Sie unter [Grundlegendes zu Blockblobs und Seitenblobs](/rest/api/storageservices/fileservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs/). Ausführliche Informationen zu Storage Premium finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Workloads auf virtuellen Azure-Computern](../articles/storage/storage-premium-storage.md).

Azure unterstützt das VHD-Format mit eingebauten („festen“) Datenträgern. Das feste Format legt den logischen Datenträger in der Datei linear aus, daher wird Datenträger-Offset X bei Blob-Offset X gespeichert. Eine kleinere Fußzeile am Ende des Blobs beschreibt die Eigenschaften der VHD. Oftmals verschwendet das feste Format Speicherplatz, da die meisten Datenträger über große ungenutzte Bereiche davon verfügen. Azure speichert VHD-Dateien jedoch in einem platzsparenden Format. Daher profitieren Sie gleichzeitig von den Vorteilen fester und dynamischer Datenträger. Nähere Informationen finden Sie unter [Erste Schritte mit virtuellen Festplatten](https://technet.microsoft.com/library/dd979539.aspx).

Alle VHD-Dateien in Azure, die Sie als Quelle zum Erstellen von Datenträgern oder Images verwenden möchten, sind schreibgeschützt. Beim Erstellen eines Datenträgers oder Images werden Kopien der VHD-Dateien von Azure erstellt. Diese Kopien sind je nach Verwendung der VHD entweder schreibgeschützt oder ermöglichen Lese- und Schreibvorgänge.

Beim Erstellen eines virtuellen Computers mithilfe eines Images, erstellt Azure einen Datenträger für den virtuellen Computer, bei dem es sich um eine Kopie der VHD-Quelldatei handelt. Um ein versehentliches Löschen zu vermeiden, setzt Azure eine Lease für alle VHD-Quelldateien, die zum Erstellen eines Images, Betriebssystem-Datenträgers oder Datenträgers für Daten verwendet wird.

Um eine VHD-Quelldatei löschen zu können, müssen Sie die Lease entfernen, indem Sie den Datenträger oder das Image löschen. Um eine VHD-Datei zu löschen, die von einem virtuellen Computer als Betriebssystem-Datenträger verwendet wird, können Sie den virtuellen Computer, den Betriebssystem-Datenträger und die VHD-Quelldatei auf einmal entfernen, indem Sie den virtuellen Computer und alle verbundenen Datenträger löschen. Zum Entfernen einer VHD-Datei, die als Quelle für einen Datenträger verwendet wird, sind jedoch mehrere Schritte in einer bestimmten Reihenfolge erforderlich. Trennen Sie den Datenträger zunächst vom virtuellen Computer, und löschen Sie den Datenträger und dann die VHD-Datei.

> [!WARNING]
> Wenn Sie eine VHD-Quelldatei aus dem Speicher löschen oder das Speicherkonto löschen, kann Microsoft diese Daten nicht wiederherstellen.
> 

## <a name="types-of-disks"></a>Datenträgertypen 

Bei der Datenträgererstellung stehen zwei Leistungsstufen für den Speicher zur Auswahl: Standardspeicher und Storage Premium. Auf beiden Leistungsstufen können Sie außerdem zwischen nicht verwalteten und verwalteten Datenträgern wählen.  

### <a name="standard-storage"></a>Standardspeicher 

Standardspeicher basiert auf Festplatten und stellt eine kostengünstige, performante Speicherlösung dar. Standardspeicher kann lokal in einem einzelnen Datencenter repliziert oder georedundant mit primärem und sekundärem Rechenzentrum verwendet werden. Weitere Informationen zur Speicherreplikation finden Sie unter [Azure Storage-Replikation](../articles/storage/storage-redundancy.md). 

Weitere Informationen zur Verwendung von Standardspeicher mit VM-Datenträgern finden Sie unter [Cost-effective Standard Storage and unmanaged and managed Azure VM disks](../articles/storage/storage-standard-storage.md) (Kostengünstiger Standardspeicher und nicht verwaltete und verwaltete Azure-VM-Datenträger).

### <a name="premium-storage"></a>Storage Premium 

Storage Premium basiert auf SSDs und unterstützt Datenträger mit hoher Leistung und geringer Latenz für virtuelle Computer mit E/A-intensiven Workloads. Storage Premium steht für virtuelle Azure-Computer vom Typ DS, DSv2, GS und FS zur Verfügung. Weitere Informationen finden Sie unter [Storage Premium](../articles/storage/storage-premium-storage.md).

### <a name="unmanaged-disks"></a>Nicht verwaltete Datenträger

Bei nicht verwalteten Datenträgern handelt es sich um den herkömmlichen Datenträgertyp für virtuelle Computer. Bei dieser Option erstellen Sie Ihr eigenes Speicherkonto und geben es beim Erstellen des Datenträgers an. Sie müssen darauf achten, dass Sie nicht zu viele Datenträger unter dem gleichen Speicherkonto erstellen, da Sie ansonsten unter Umständen die [Skalierbarkeitsziele](../articles/storage/storage-scalability-targets.md) des Speicherkontos (beispielsweise 20.000 IOPS) überschreiten, was eine Drosselung der virtuellen Computer zur Folge hat. Bei nicht verwalteten Datenträgern müssen Sie herausfinden, wie Sie einzelne oder mehrere Speicherkonten optimal nutzen, um die bestmögliche Leistung für Ihre virtuellen Computer zu erzielen.

### <a name="managed-disks"></a>Verwaltete Datenträger 

Managed Disks nimmt Ihnen die Speicherkontoerstellung und -verwaltung ab und sorgt dafür, dass Sie sich keine Gedanken mehr über die Skalierbarkeitsgrenzwerte des Speicherkontos machen müssen. Sie geben einfach die Datenträgergröße und die Leistungsstufe (Standard/Premium) an und überlassen Azure die Erstellung und Verwaltung der Datenträger. So müssen Sie sich selbst beim Hinzufügen von Datenträgern oder beim Skalieren des virtuellen Computers keine Gedanken mehr über den verwendeten Speicher machen. 

Darüber hinaus können Sie Ihre benutzerdefinierten Images pro Azure-Region in einem einzelnen Speicherkonto verwalten und mit ihnen Hunderte von virtuellen Computern im gleichen Abonnement erstellen. Weitere Informationen zu Managed Disks finden Sie in der [Übersicht über Managed Disks](../articles/storage/storage-managed-disks-overview.md).

Wir empfehlen, Azure Managed Disks für neue virtuelle Computer zu verwenden und bislang nicht verwaltete Datenträger in verwaltete Datenträger umzuwandeln, um von den zahlreichen Features zu profitieren, die in Managed Disks zur Verfügung stehen.

### <a name="disk-comparison"></a>Datenträgervergleich

Die folgende Tabelle enthält eine Gegenüberstellung von Storage Premium und Standardspeicher (sowohl für nicht verwaltete als auch für verwaltete Datenträger), um Ihnen die Entscheidung zu erleichtern:

|    | Azure-Premium-Datenträger | Azure-Standarddatenträger |
|--- | ------------------ | ------------------- |
| Datenträgertyp | Solid-State-Laufwerke (SSD) | Festplattenlaufwerke (HDD)  |
| Übersicht  | SSD-basierte Datenträgerunterstützung mit hoher Leistung und geringer Wartezeit für virtuelle Computer mit E/A-intensiven Workloads oder zum Hosten unternehmenskritischer Produktionsumgebungen | HDD-basierte, kostengünstige Datenträgerunterstützung für Entwicklungs-/Testszenarien mit virtuellen Computern |
| Szenario  | Produktionsworkloads und leistungsabhängige Workloads | Entwicklung/Test, nicht kritischer, <br>seltener Zugriff |
| Datenträgergröße | P10: 128 GB<br>P20: 512 GB<br>P30: 1024 GB | Nicht verwaltete Datenträger: 1 GB bis 1 TB <br><br>Verwaltete Datenträger:<br> S4: 32 GB <br>S6: 64 GB <br>S10: 128 GB <br>S20: 512 GB <br>S30: 1024 GB |
| Max. Durchsatz pro Datenträger | 200 MB/s | 60 MB/s |
| Max. IOPS pro Datenträger | 5000 IOPS | 500 IOPS |
