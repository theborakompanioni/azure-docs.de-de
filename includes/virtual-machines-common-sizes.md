


In Azure stehen mehrere Standardgrößen zur Auswahl zur Verfügung. Beachten Sie für einige dieser Größen die folgenden Punkte:

* VMs der D-Serie dienen zum Ausführen von Anwendungen, die eine höhere Rechenleistung und eine höhere temporäre Datenträgerleistung erfordern. VMs der D-Serie bieten schnellere Prozessoren, ein höheres Verhältnis von Speicher zu Kern und ein SSD (Solid State Drive) für den temporären Datenträger. Einzelheiten finden Sie in der Ankündigung im Azure-Blog unter [New D-Series Virtual Machine Sizes](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/)(Neue VM-Größen der D-Serie, in englischer Sprache).
* Die Dv2-Serie, eine Nachfolgerin der ursprünglichen D-Serie, hat eine leistungsfähigere CPU. Die CPU der Dv2-Serie ist ca. 35 % schneller als die CPU der D-Serie. Sie basiert auf der neuesten Generation des 2,4-GHz-Intel Xeon® E5-2673 v3-Prozessors (Haswell) und kann mit der Intel Turbo Boost Technology 2.0 bis auf 3,1 GHz erhöht werden. Die Dv2-Serie hat die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die D-Serie.
* Die F-Serie basiert auf dem Intel Xeon® E5-2673 v3-Prozessor (Haswell) mit 2,4 GHz, der mit der Intel Turbo Boost Technology 2.0 Taktfrequenzen von 3,1 GHz erreichen kann. Dies ist die gleiche CPU-Leistung wie bei virtuellen Computern (VMs) der Dv2-Serie.  Die F-Serie hat einen niedrigeren Listenpreis pro Stunde und bietet auf Basis der Azure-Berechnungseinheit ACU (Azure Compute Unit) pro Kern das beste Preis-Leistungs-Verhältnis im Azure-Portfolio. 
  
    Bei der F-Serie wird außerdem ein neuer Standard bei der Benennung von VM-Größen in Azure eingeführt. Bei dieser Serie und bei künftig freigegebenen VM-Größen entspricht der hinter dem Buchstaben für den Familiennamen angegebene Zahlenwert der Anzahl von CPU-Kernen. Weitere Eigenschaften und Fähigkeiten, wie z.B. die Optimierung für Storage Premium, werden nach dem Zahlenwert für die Anzahl von CPU-Kernen durch Buchstaben angegeben. Dieses Namensformat wird für zukünftig freigegebene VM-Größen verwendet. Die Namen von bestehenden und freigegebenen VM-Größen werden jedoch nicht rückwirkend geändert.
* Virtuelle Computer der G-Serie bieten den meisten Arbeitsspeicher und werden auf Hosts mit Prozessoren der Intel Xeon E5 V3-Familie ausgeführt.
* Virtuelle Computer der DS-, DSv2-, Fs- und der GS-Serie können Storage Premium nutzen, wobei Hochleistungsspeicher mit geringer Latenz für E/A-intensive Workloads geboten wird. Diese virtuellen Computer nutzen Solid State Drives (SSDs) zum Hosten der Datenträger eines virtuellen Computers und stellen zudem einen lokalen SSD-Datenträgercache bereit. Premium-Speicher ist in bestimmten Regionen verfügbar. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/storage/storage-premium-storage.md).
*   Die virtuellen Computer der A- und der Av2-Serie können für viele verschiedene Hardwaretypen und Prozessoren bereitgestellt werden. Die Größe ist basierend auf der Hardware gedrosselt, um eine konsistente Prozessorleistung für die ausgeführte Instanz zu ermöglichen – unabhängig von der Hardware, die für die Bereitstellung gewählt wird. Fragen Sie die virtuelle Hardware über die virtuelle Maschine ab, um die physische Hardware zu ermitteln, auf der diese Größe bereitgestellt wird.
* Die Größe „A0“ ist auf der physischen Hardware „überzeichnet“. Nur für diese spezielle Größe kann es dazu kommen, dass sich andere Kundenbereitstellungen negativ auf die Leistung Ihrer aktiven Workload auswirken. Die relative Leistung ist unten als erwartete Baseline beschrieben und unterliegt einer ungefähren Variabilität von 15 Prozent.

Die Größe des virtuellen Computers wirkt sich auf den Preis aus. Die Größe beeinflusst auch die Verarbeitung, den Arbeitsspeicher und die Speicherkapazität des virtuellen Computers. Speicherkosten werden separat basierend auf bereits verwendete Seiten im Speicherkonto berechnet. Weitere Informationen finden Sie unter [Virtuelle Computer – Preisdetails](https://azure.microsoft.com/pricing/details/virtual-machines/) und [Preisdetails zu Azure Storage](https://azure.microsoft.com/pricing/details/storage/). 

Die folgenden Überlegungen können Ihnen bei der Entscheidung über die Größe behilflich sein:

* Die Größen A8 bis A11 und die Größen der H-Reihe werden auch als *rechenintensive Instanzen* bezeichnet. Die Hardware, auf der diese Größen ausgeführt werden, wurde für rechenintensive Anwendungen mit hoher Netzwerkauslastung konzipiert und optimiert. Hierzu zählen beispielsweise HPC-Clusteranwendungen (High Performance Computing), Modellierung und Simulationen. Die Reihen A8 bis A11 nutzen Intel Xeon E5-2670 @mit 2,6 GHZ und die H-Reihe Intel Xeon E5-2667 v3@ mit 3,2 GHz. Ausführliche Informationen und Überlegungen zum Verwenden dieser Größen finden Sie unter [Informationen zu den rechenintensiven A8-, A9-, A10- und A11-Instanzen](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
* Die Serien Dv2, D, G sowie deren DS/GS-Entsprechungen eignen sich ideal für Clientanwendungen, die schnellere CPUs oder bessere lokale Datenträgerleistung erfordern oder einen höheren Speicherbedarf haben.  Sie bieten eine leistungsfähige Kombination für viele Anwendungen für den Unternehmenseinsatz.
* Virtuelle Computer der F-Serie sind eine hervorragende Wahl für Arbeitslasten, die schnellere CPUs erfordern, aber nicht so viel Arbeitsspeicher oder lokalen SSD pro CPU-Kern benötigen.  Bei Arbeitslasten wie Analysen, Gamingservern, Webservern und Batchverarbeitung kommen die Vorteile und der Nutzen der F-Serie besonders gut zum Tragen.
* Einige der physischen Hosts in Azure-Rechenzentren unterstützen möglicherweise keine der größeren VM-Größen, z.B. A5 bis A11. Daher wird möglicherweise die Fehlermeldung **Fehler beim Konfigurieren des virtuellen Computers<machine name>** oder **Fehler beim Erstellen des virtuellen Computers<machine name>** beim Ändern der Größe eines vorhandenen virtuellen Computers, beim Erstellen eines neuen virtuellen Computers in einem virtuellen Netzwerk, das vor dem 16. April 2013 erstellt wurde, oder beim Hinzufügen eines neuen virtuellen Computers zu einem vorhandenen Clouddienst angezeigt. Problemumgehungen für die einzelnen Bereitstellungsszenarios finden Sie im Supportforum unter [Error: “Failed to configure virtual machine”](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) (Fehler beim Konfigurieren des virtuellen Computers).  
* Möglicherweise ist bei Ihrem Abonnement auch die Anzahl von Kernen beschränkt, die in bestimmten Größenkategorien bereitgestellt werden können. Wenn Sie Kontingent erhöhen möchten, wenden Sie sich an den Azure-Support.

## <a name="performance-considerations"></a>Überlegungen zur Leistung
Wir haben das Konzept der Azure-Berechnungseinheit ACU (Azure Compute Unit) erstellt, um eine Möglichkeit zum Vergleichen der Rechenleistung (CPU) zwischen den Azure-SKUs zu ermöglichen. Auf diese Weise können Sie leicht feststellen, welche SKU Ihren Leistungsanforderungen am ehesten entspricht.  Zurzeit ist der ACU-Wert auf einem kleinen virtuellen Computer (Standard_A1) auf den Standardwert 100 festgelegt, und an den übrigen SKUs kann ungefähr abgelesen werden, wie viel schneller die jeweilige SKU einen Standard-Benchmarktest ausführen kann. 

> [!IMPORTANT]
> Die ACU ist nur ein Richtwert.  Die Ergebnisse für Ihre Workload können abweichen. 
> 
> 

<br>

| SKU-Familie | ACU/Kern |
| --- | --- |
| [A0](#a-series) |50 |
| [A1–A4](#a-series) |100 |
| [A5–A7](#a-series) |100 |
| [A1_v2–A8_v2](#av2-series) |100 |
| [A2m_v2–A8m_v2](#av2-series) |100 |
| [A8-A11](#a-series) |225* |
| [D1–D14](#d-series) |160 |
| [D1_v2–D15_v2](#dv2-series) |210 - 250* |
| [DS1–DS14](#ds-series) |160 |
| [DS1_v2–DS15_v2](#dsv2-series) |210-250* |
| [F1-F16](#f-series) |210-250* |
| [F1s-F16s](#fs-series) |210-250* |
| [G1–G5](#g-series) |180 - 240* |
| [GS1–GS5](#gs-series) |180 - 240* |
| [H](#h-series) |290 - 300* |

Mit * gekennzeichnete ACUs verwenden die Intel® Turbo-Technologie, um die CPU-Frequenz zu erhöhen eine Leistungssteigerung zu erzielen.  Das Maß der Leistungssteigerung variiert basierend auf der Größe und Workload des virtuellen Computers sowie auf anderen Workloads, die auf dem gleichen Host ausgeführt werden.

## <a name="size-tables"></a>Größentabellen
In den folgenden Tabellen sind die Größe und die von den einzelnen Größen bereitgestellte Kapazität aufgeführt.

* Speicherkapazität wird in GiB-Einheiten oder 1.024^3 Bytes angezeigt. Beachten Sie beim Vergleich von in GB (1.000^3 Bytes) gemessenen Datenträgern mit in GiB (1.024^3) gemessenen Datenträgern, dass die in GiB angegebenen Kapazitätszahlen kleiner erscheinen können. Beispiel: 1.023 GiB = 1.098,4 GB.
* Der Datenträgerdurchsatz wird in E/A-Vorgängen pro Sekunde (Input/Output Operations Per Second, IOPS) und MB/s gemessen, wobei MB/s = 10^6 Bytes/Sekunde beträgt.
* Datenträger können mit oder ohne Cache betrieben werden.  Beim Datenträgerbetrieb mit Cache ist der Hostcachemodus auf **ReadOnly** oder **ReadWrite** festgelegt.  Beim Datenträgerbetrieb ohne Cache ist der Hostcachemodus auf **None**festgelegt.
* Bei der maximalen Netzwerkbandbreite handelt es sich um die maximale aggregierte Bandbreite, die pro VM-Typ zugewiesen wurde. Die maximale Bandbreite dient als Orientierungshilfe bei der Wahl des richtigen VM-Typs, um sicherzustellen, dass ausreichend Netzwerkkapazität verfügbar ist. Bei einem Wechsel zu „niedrig“, „mittel“, „hoch“ oder „sehr hoch“ ändert sich der Durchsatz entsprechend. Die tatsächliche Netzwerkleistung hängt von zahlreichen Faktoren ab. Hierzu zählen beispielsweise die Netzwerk- und Anwendungslast und die Netzwerkeinstellungen der Anwendung.

## <a name="a-series"></a>A-Serie
| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales HDD: GiB | Max. Anzahl Datenträger | Max. Datenträger-Datendurchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0 |1 |0,768 |20 |1 |1 x&500; |1/niedrig |
| Standard_A1 |1 |1,75 |70 |2 |2 x&500; |1/moderat |
| Standard_A2 |2 |3,5 |135 |4 |4 x&500; |1/moderat |
| Standard_A3 |4 |7 |285 |8 |8 x&500; |2/hoch |
| Standard_A4 |8 |14 |605 |16 |16 x&500; |4/hoch |
| Standard_A5 |2 |14 |135 |4 |4 x&500; |1/moderat |
| Standard_A6 |4 |28 |285 |8 |8 x&500; |2/hoch |
| Standard_A7 |8 |56 |605 |16 |16 x&500; |4/hoch |

<br>

## <a name="a-series---compute-intensive-instances"></a>A-Serie: Rechenintensive Instanzen
Informationen und Überlegungen zum Verwenden dieser Größen finden Sie unter [Informationen zu den rechenintensiven A8-, A9-, A10- und A11-Instanzen](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales HDD: GiB | Max. Anzahl Datenträger | Max. Datenträger-Datendurchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8* |8 |56 |382 |16 |16 x&500; |2/hoch |
| Standard_A9* |16 |112 |382 |16 |16 x&500; |4/sehr hoch |
| Standard_A10 |8 |56 |382 |16 |16 x&500; |2/hoch |
| Standard_A11 |16 |112 |382 |16 |16 x&500; |4/sehr hoch |

* RDMA-fähig

<br>

## <a name="av2-series"></a>Av2-Serie

| Größe        | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Max. Datenträger-Datendurchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A1_v2 | 1         | 2            | 10                   | 2              | 2 x&500;              | 1/moderat              |
| Standard_A2_v2 | 2         | 4            | 20                   | 4              | 4 x&500;              | 2/moderat              |
| Standard_A4_v2 | 4         | 8            | 40                   | 8              | 8 x&500;              | 4/hoch                  |
| Standard_A8_v2 | 8         | 16           | 80                   | 16             | 16 x&500;             | 8/hoch                  |
| Standard_A2m_v2 | 2        | 16           | 20                   | 4              | 4 x&500;              | 2/moderat              |
| Standard_A4m_v2 | 4        | 32           | 40                   | 8              | 8 x&500;              | 4/hoch                  |
| Standard_A8m_v2 | 8        | 64           | 80                   | 16             | 16 x&500;             | 8/hoch                  |


## <a name="d-series"></a>D-Serie
| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Max. Datenträger-Datendurchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D1 |1 |3,5 |50 |2 |2 x&500; |1/moderat |
| Standard_D2 |2 |7 |100 |4 |4 x&500; |2/hoch |
| Standard_D3 |4 |14 |200 |8 |8 x&500; |4/hoch |
| Standard_D4 |8 |28 |400 |16 |16 x&500; |8/hoch |
| Standard_D11 |2 |14 |100 |4 |4 x&500; |2/hoch |
| Standard_D12 |4 |28 |200 |8 |8 x&500; |4/hoch |
| Standard_D13 |8 |56 |400 |16 |16 x&500; |8/hoch |
| Standard_D14 |16 |112 |800 |32 |32 x&500; |8/sehr hoch |

<br>

## <a name="dv2-series"></a>Dv2-Serie
| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Max. Datenträger-Datendurchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D1_v2 |1 |3,5 |50 |2 |2 x&500; |1/moderat |
| Standard_D2_v2 |2 |7 |100 |4 |4 x&500; |2/hoch |
| Standard_D3_v2 |4 |14 |200 |8 |8 x&500; |4/hoch |
| Standard_D4_v2 |8 |28 |400 |16 |16 x&500; |8/hoch |
| Standard_D5_v2 |16 |56 |800 |32 |32 x&500; |8/äußerst hoch |
| Standard_D11_v2 |2 |14 |100 |4 |4 x&500; |2/hoch |
| Standard_D12_v2 |4 |28 |200 |8 |8 x&500; |4/hoch |
| Standard_D13_v2 |8 |56 |400 |16 |16 x&500; |8/hoch |
| Standard_D14_v2 |16 |112 |800 |32 |32 x&500; |8/äußerst hoch |
| Standard_D15_v2** |20 |140 |1.000 |40 |40 x&500; |8/äußerst hoch* |

*In einigen Regionen stehen für die Größe „Standard_D15_v2“ beschleunigte Netzwerke zur Verfügung. Weitere Informationen zur Verwendung und Verfügbarkeit finden Sie unter [Accelerated Networking befindet sich in der Vorschauphase](https://azure.microsoft.com/updates/accelerated-networking-in-preview/) sowie unter [Accelerated Networking für virtuelle Computer](../articles/virtual-network/virtual-network-accelerated-networking-powershell.md).

**Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.

<br>

## <a name="ds-series"></a>DS-Serie*
| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Maximaler Datenträgerdurchsatz mit Cache: IOPS / MB/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3,5 |7 |2 |4.000/32 (43) |3.200/32 |1/moderat |
| Standard_DS2 |2 |7 |14 |4 |8.000/64 (86) |6.400/64 |2/hoch |
| Standard_DS3 |4 |14 |28 |8 |16.000/128 (172) |12.800/128 |4/hoch |
| Standard_DS4 |8 |28 |56 |16 |32.000/256 (344) |25.600/256 |8/hoch |
| Standard_DS11 |2 |14 |28 |4 |8.000/64 (72) |6.400/64 |2/hoch |
| Standard_DS12 |4 |28 |56 |8 |16.000/128 (144) |12.800/128 |4/hoch |
| Standard_DS13 |8 |56 |112 |16 |32.000/256 (288) |25.600/256 |8/hoch |
| Standard_DS14 |16 |112 |224 |32 |64.000/512 (576) |51.200/512 |8/sehr hoch |

MB/s = 10^6 Bytes pro Sekunde und GB = 1.024^3 Bytes.

* Der mit einer VM der DS-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden.  Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/storage/storage-premium-storage.md).

<br>

## <a name="dsv2-series"></a>DSv2-Serie*
| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Maximaler Datenträgerdurchsatz mit Cache: IOPS / MB/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1_v2 |1 |3,5 |7 |2 |4.000/32 (43) |3.200/48 |1 moderat |
| Standard_DS2_v2 |2 |7 |14 |4 |8.000/64 (86) |6.400/96 |2 hoch |
| Standard_DS3_v2 |4 |14 |28 |8 |16.000/128 (172) |12.800/192 |4 hoch |
| Standard_DS4_v2 |8 |28 |56 |16 |32.000/256 (344) |25.600/384 |8 hoch |
| Standard_DS5_v2 |16 |56 |112 |32 |64.000/512 (688) |51.200/768 |8 äußerst hoch |
| Standard_DS11_v2 |2 |14 |28 |4 |8.000/64 (72) |6.400/96 |2 hoch |
| Standard_DS12_v2 |4 |28 |56 |8 |16.000/128 (144) |12.800/192 |4 hoch |
| Standard_DS13_v2 |8 |56 |112 |16 |32.000/256 (288) |25.600/384 |8 hoch |
| Standard_DS14_v2 |16 |112 |224 |32 |64.000/512 (576) |51.200/768 |8 äußerst hoch |
| Standard_DS15_v2*** |20 |140 |280 |40 |80.000/640 (720) |64.000/960 |8 äußerst hoch** |

MB/s = 10^6 Bytes pro Sekunde und GB = 1.024^3 Bytes.

* Der mit einer VM der DSv2-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden.  Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/storage/storage-premium-storage.md).

**In einigen Regionen stehen für die Größe „Standard_DS15_v2“ beschleunigte Netzwerke zur Verfügung. Weitere Informationen zur Verwendung und Verfügbarkeit finden Sie unter [Accelerated Networking befindet sich in der Vorschauphase](https://azure.microsoft.com/updates/accelerated-networking-in-preview/) sowie unter [Accelerated Networking für virtuelle Computer](../articles/virtual-network/virtual-network-accelerated-networking-powershell.md).

***Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.
<br>

## <a name="f-series"></a>F-Serie
| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Max. Datenträger-Datendurchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_F1 |1 |2 |16 |2 |2 x&500; |1/moderat |
| Standard_F2 |2 |4 |32 |4 |4 x&500; |2/hoch |
| Standard_F4 |4 |8 |64 |8 |8 x&500; |4/hoch |
| Standard_F8 |8 |16 |128 |16 |16 x&500; |8/hoch |
| Standard_F16 |16 |32 |256 |32 |32 x&500; |8/äußerst hoch |

<br>

## <a name="fs-series"></a>Fs-Serie*
| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Maximaler Datenträgerdurchsatz mit Cache: IOPS / MB/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |2 |4.000/32 (12) |3.200/48 |1/moderat |
| Standard_F2s |2 |4 |8 |4 |8.000/64 (24) |6.400/96 |2/hoch |
| Standard_F4s |4 |8 |16 |8 |16.000/128 (48) |12.800/192 |4/hoch |
| Standard_F8s |8 |16 |32 |16 |32.000/256 (96) |25.600/384 |8/hoch |
| Standard_F16s |16 |32 |64 |32 |64.000/512 (192) |51.200/768 |8/äußerst hoch |

MB/s = 10^6 Bytes pro Sekunde und GB = 1.024^3 Bytes.

* Der mit einer VM der Fs-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden.  Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/storage/storage-premium-storage.md).

<br>

## <a name="g-series"></a>G-Serie
| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Max. Datenträger-Datendurchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_G1 |2 |28 |384 |4 |4 x 500 |1/hoch |
| Standard_G2 |4 |56 |768 |8 |8 x 500 |2/hoch |
| Standard_G3 |8 |112 |1.536 |16 |16 x 500 |4/sehr hoch |
| Standard_G4 |16 |224 |3.072 |32 |32 x 500 |8/äußerst hoch |
| Standard_G5* |32 |448 |6.144 |64 |64 x 500 |8/äußerst hoch |

*Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.
<br>

## <a name="gs-series"></a>GS-Serie*
| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Maximaler Datenträgerdurchsatz mit Cache: IOPS / MB/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_GS1 |2 |28 |56 |4 |10.000/100 (264) |5.000 / 125 |1/hoch |
| Standard_GS2 |4 |56 |112 |8 |20.000/200 (528) |10.000/250 |2/hoch |
| Standard_GS3 |8 |112 |224 |16 |40.000/400 (1.056) |20.000/500 |4/sehr hoch |
| Standard_GS4 |16 |224 |448 |32 |80.000/800 (2.112) |40.000/1.000 |8/äußerst hoch |
| Standard_GS5** |32 |448 |896 |64 |160.000/1.600 (4.224) |80.000/2.000 |8/äußerst hoch |

MB/s = 10^6 Bytes pro Sekunde und GB = 1.024^3 Bytes.

* Der mit einer VM der GS-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden. 

**Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.
<br>

## <a name="h-series"></a>H-Reihe
Virtuelle Azure-Computer der H-Reihe sind High Performing Computing-VMs der nächsten Generation für High-End-Berechnungsanforderungen, z.B. Molekülmodellierung und Strömungsdynamikberechnung. Diese virtuellen Computer mit 8 und 16 Kernen basieren auf der Intel Haswell E5-2667 V3-Prozessortechnologie mit DDR4-Speicher und lokalem SSD-Speicher. 

Neben beträchtlicher CPU-Leistung bietet die H-Serie verschiedene Optionen für RDMA-Netzwerke mit niedriger Latenz unter Verwendung von FDR InfiniBand sowie verschiedene Speicherkonfigurationen für Berechnungsanforderungen mit hohem Speicherbedarf.

Informationen und Überlegungen zum Verwenden dieser Größen finden Sie unter [Informationen zu den rechenintensiven A8-, A9-, A10- und A11-Instanzen](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Max. Datenträger-Datendurchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |16 |16 x 500 |2/hoch |
| Standard_H16 |16 |112 |2000 |32 |32 x 500 |4/sehr hoch |
| Standard_H8m |8 |112 |1000 |16 |16 x 500 |2/hoch |
| Standard_H16m |16 |224 |2000 |32 |32 x 500 |4/sehr hoch |
| Standard_H16r* |16 |112 |2000 |32 |32 x 500 |4/sehr hoch |
| Standard_H16mr* |16 |224 |2000 |32 |32 x 500 |4/sehr hoch |

* RDMA-fähig

<br>

## <a name="n-series"></a>N-Serie
Die NC- und NV-Größen werden auch als GPU-fähige Instanzen bezeichnet. Dabei handelt es sich um spezielle virtuelle Computer, die für verschiedene Szenarien und Anwendungsfälle optimierte GPU-Karten von NVIDIA enthalten. Die NV-Größen sind optimiert und konzipiert für Remotevisualisierung, Streaming, Spiele, Codierung und VDI-Szenarien mit Frameworks wie OpenGL und DirectX. Die NC-Größen sind eher für rechen- und netzwerkintensive Anwendungen und Algorithmen optimiert, beispielsweise CUDA- und OpenCL-basierte Anwendungen und Simulationen. 

### <a name="nv-instances"></a>NV-Instanzen
Die NV-Instanzen werden durch die Tesla M60 GPU-Karte von NVIDIA und NVIDIA GRID unterstützt und ermöglichen für Desktops beschleunigte Anwendungen und virtuelle Desktops, sodass die Kunden ihre Daten oder Simulationen visualisieren können. Benutzer können ihre grafikintensiven Workflows auf den NV-Instanzen visualisieren, um überlegene Grafikfunktionen zu erhalten, und darüber hinaus Workloads mit einfacher Genauigkeit ausführen, z.B. Codierung und Rendering. Tesla M60 bietet 4096 CUDA-Kerne als duale GPU mit bis zu 36 Datenströmen mit 1080p H.264. 


| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | GPU |
| --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 |
| Standard_NV12 |12 |112 |680 | 2 |
| Standard_NV24 |24 |224 |1.440 | 4 |

1 GPU = halbe M60-Karte

**Unterstützte Betriebssysteme**

* Windows Server 2016, Windows Server 2012 R2. Informationen finden Sie unter [N-series driver setup for Windows](../articles/virtual-machines/virtual-machines-windows-n-series-driver-setup.md) (Einrichtung von Treibern für die N-Serie unter Windows).

### <a name="nc-instances"></a>NC-Instanzen
Die NC-Instanzen werden durch die Grafikkarte Tesla K80 von NVIDIA unterstützt. Benutzer können Daten jetzt wesentlich schneller analysieren, indem sie CUDA für Anwendungen zur Energieuntersuchung, Absturzsimulationen, Rendering mit Raytracing, Deep Learning und mehr verwenden. Tesla K80 bietet 4992 CUDA-Kerne als duale GPU mit einer Leistung von bis zu 2,91 Teraflops mit doppelter Genauigkeit und bis zu 8,93 Teraflops mit einfacher Genauigkeit.

| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | GPU |
| --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 |
| Standard_NC24 |24 |224 | 1.440 | 4 |
| Standard_NC24r* |24 |224 | 1.440 | 4 |

1 GPU = halbe K80-Karte

* RDMA-fähig

**Unterstützte Betriebssysteme**

* Windows Server 2016, Windows Server 2012 R2. Informationen finden Sie unter [N-series driver setup for Windows](../articles/virtual-machines/virtual-machines-windows-n-series-driver-setup.md) (Einrichtung von Treibern für die N-Serie unter Windows).
* Ubuntu 16.04 LTS. Informationen finden Sie unter [N-series driver setup for Linux](../articles/virtual-machines/virtual-machines-linux-n-series-driver-setup.md) (Einrichtung von Treibern für die N-Serie unter Linux).

<br>

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Hinweise: Standard_A0 – A4 mit Befehlszeilenschnittstelle (CLI) und PowerShell
Im klassischen Bereitstellungsmodell unterscheiden sich einige VM-Größennamen in der Befehlszeilenschnittstelle und in PowerShell:

* Standard_A0 ist „Sehr klein“ 
* Standard_A1 ist „Klein“
* Standard_A2 ist „Mittel“
* Standard_A3 ist „Groß“
* Standard_A4 ist „Extragroß“

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Einschränkungen für Azure-Abonnements und -Dienste, Kontingente und Einschränkungen](../articles/azure-subscription-service-limits.md).
* Erfahren Sie mehr über [rechenintensive A8-, A9-, A10- und A11-Instanzen](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) für Workloads wie etwa High-Performance Computing (HPC).



<!--HONumber=Feb17_HO2-->


