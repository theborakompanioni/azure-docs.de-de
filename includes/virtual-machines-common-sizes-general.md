
<!-- A-series, Av2-series, D-series, Dv2-series, DS-series*, DSv2-series* -->

Die virtuellen Computer der A- und der Av2-Serie können für viele verschiedene Hardwaretypen und Prozessoren bereitgestellt werden. Die Größe ist basierend auf der Hardware gedrosselt, um eine konsistente Prozessorleistung für die ausgeführte Instanz zu ermöglichen – unabhängig von der Hardware, die für die Bereitstellung gewählt wird. Fragen Sie die virtuelle Hardware über die virtuelle Maschine ab, um die physische Hardware zu ermitteln, auf der diese Größe bereitgestellt wird.

VMs der D-Serie dienen zum Ausführen von Anwendungen, die eine höhere Rechenleistung und eine höhere temporäre Datenträgerleistung erfordern. VMs der D-Serie bieten schnellere Prozessoren, ein höheres Verhältnis von Speicher zu Kern und ein SSD (Solid State Drive) für den temporären Datenträger. Einzelheiten finden Sie in der Ankündigung im Azure-Blog unter [New D-Series Virtual Machine Sizes](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/)(Neue VM-Größen der D-Serie, in englischer Sprache).

Die Dv2-Serie, eine Nachfolgerin der ursprünglichen D-Serie, hat eine leistungsfähigere CPU. Die CPU der Dv2-Serie ist ca. 35 % schneller als die CPU der D-Serie. Sie basiert auf der neuesten Generation des 2,4-GHz-Intel Xeon® E5-2673 v3-Prozessors (Haswell) und kann mit der Intel Turbo Boost Technology 2.0 bis auf 3,1 GHz erhöht werden. Die Dv2-Serie hat die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die D-Serie.

Die Größen des Basic-Tarifs sind hauptsächlich für Entwicklungsworkloads und andere Anwendungen bestimmt, für die kein Lastenausgleich, keine automatische Skalierung und keine speicherintensiven virtuellen Computer erforderlich sind. Informationen zu VM-Größen, die für Produktionsanwendungen besser geeignet sind, finden Sie unter (Größen für virtuelle Computer)[virtual-machines-size-specs.md], und Informationen zu VM-Preisen finden Sie unter [Virtuelle Linux-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="dsv2-series"></a>DSv2-Serie*

ACU: 210 - 250

| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Maximaler Datenträgerdurchsatz mit Cache: IOPS / MB/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1_v2 |1 |3,5 |7 |2 |4.000/32 (43) |3.200/48 |2 moderat |
| Standard_DS2_v2 |2 |7 |14 |4 |8.000/64 (86) |6.400/96 |2 hoch |
| Standard_DS3_v2 |4 |14 |28 |8 |16.000/128 (172) |12.800/192 |4 hoch |
| Standard_DS4_v2 |8 |28 |56 |16 |32.000/256 (344) |25.600/384 |8 hoch |
| Standard_DS5_v2 |16 |56 |112 |32 |64.000/512 (688) |51.200/768 |8 äußerst hoch |



## <a name="dv2-series"></a>Dv2-Serie

ACU: 210 - 250

| Größe              | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Durchsatz lokale Datenträger: IOPS/Lesen (MBit/s)/Schreiben (MBit/s) | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1_v2    | 1         | 3,5         | 50             | 3000/46/23                                           | 2/2 x 500                         | 2/moderat                 |
| Standard_D2_v2    | 2         | 7           | 100            | 6000/93/46                                           | 4/4 x 500                         | 2/hoch                     |
| Standard_D3_v2    | 4         | 14          | 200            | 12000/187/93                                         | 8/8 x 500                         | 4/hoch                     |
| Standard_D4_v2    | 8         | 28          | 400            | 24000/375/187                                        | 16/16 x 500                       | 8/hoch                     |
| Standard_D5_v2    | 16        | 56          | 800            | 48000/750/375                                        | 32/32 x 500                       | 8/äußerst hoch           |

<br>

## <a name="ds-series"></a>DS-Serie*
| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Maximaler Datenträgerdurchsatz mit Cache und lokal: IOPS/MBit/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3,5 |7 |2 |4.000/32 (43) |3.200/32 |2^/moderat |
| Standard_DS2 |2 |7 |14 |4 |8.000/64 (86) |6.400/64 |2/hoch |
| Standard_DS3 |4 |14 |28 |8 |16.000/128 (172) |12.800/128 |4/hoch |
| Standard_DS4 |8 |28 |56 |16 |32.000/256 (344) |25.600/256 |8/hoch |

<br>
## <a name="d-series"></a>D-Serie 

ACU: 160

| Größe         | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Durchsatz lokale Datenträger: IOPS/Lesen (MBit/s)/Schreiben (MBit/s) | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1  | 1         | 3,5         | 50             | 3000/46/23                                           | 2/2 x 500                         | 2/moderat                 |
| Standard_D2  | 2         | 7           | 100            | 6000/93/46                                           | 4/4 x 500                         | 2/hoch                     |
| Standard_D3  | 4         | 14          | 200            | 12000/187/93                                         | 8/8 x 500                         | 4/hoch                     |
| Standard_D4  | 8         | 28          | 400            | 24000/375/187                                        | 16/16 x 500                       | 8/hoch                     |

<br>


## <a name="av2-series"></a>Av2-Serie

ACU: 100

| Größe            | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Durchsatz lokale Datenträger: IOPS/Lesen (MBit/s)/Schreiben (MBit/s) | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
|-----------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_A1_v2  | 1         | 2           | 10             | 1000/20/10                                           | 2/2 x 500                         | 2/moderat                 |
| Standard_A2_v2  | 2         | 4           | 20             | 2000/40/20                                           | 4/4 x 500                         | 2/moderat                 |
| Standard_A4_v2  | 4         | 8           | 40             | 4000/80/40                                           | 8/8 x 500                         | 4/hoch                     |
| Standard_A8_v2  | 8         | 16          | 80             | 8000/160/80                                          | 16/16 x 500                       | 8/hoch                     |
| Standard_A2m_v2 | 2         | 16          | 20             | 2000/40/20                                           | 4/4 x 500                         | 2/moderat                 |
| Standard_A4m_v2 | 4         | 32          | 40             | 4000/80/40                                           | 8/8 x 500                         | 4/hoch                     |
| Standard_A8m_v2 | 8         | 64          | 80             | 8000/160/80                                          | 16/16 x 500                       | 8/hoch                     |

<br>

## <a name="a-series"></a>A-Serie

ACU: 50 - 100

| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales HDD: GiB | Max. Anzahl Datenträger | Max. Datenträger-Datendurchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0* |1 |0,768 |20 |1 |1 x 500 |2/niedrig |
| Standard_A1 |1 |1,75 |70 |2 |2 x 500 |2/moderat |
| Standard_A2 |2 |3,5 |135 |4 |4 x 500 |2/moderat |
| Standard_A3 |4 |7 |285 |8 |8 x 500 |2/hoch |
| Standard_A4 |8 |14 |605 |16 |16 x 500 |4/hoch |
| Standard_A5 |2 |14 |135 |4 |4 x 500 |2/moderat |
| Standard_A6 |4 |28 |285 |8 |8 x 500 |2/hoch |
| Standard_A7 |8 |56 |605 |16 |16 x 500 |4/hoch |
<br>

*Die Größe „A0“ ist auf der physischen Hardware „überzeichnet“. Nur für diese spezielle Größe kann es dazu kommen, dass sich andere Kundenbereitstellungen negativ auf die Leistung Ihrer aktiven Workload auswirken. Die relative Leistung ist unten als erwartete Baseline beschrieben und unterliegt einer ungefähren Variabilität von 15 Prozent.

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Standard_A0 – A4 mit Befehlszeilenschnittstelle (CLI) und PowerShell
Im klassischen Bereitstellungsmodell unterscheiden sich einige VM-Größennamen in der Befehlszeilenschnittstelle und in PowerShell:

* Standard_A0 ist „Sehr klein“ 
* Standard_A1 ist „Klein“
* Standard_A2 ist „Mittel“
* Standard_A3 ist „Groß“
* Standard_A4 ist „Extragroß“

## <a name="basic-a"></a>Basic A

|Größe – Größe\Name |CPU-Kerne|Arbeitsspeicher|Netzwerkkarten (max.)|Max. Größe der temporären Datenträger |Max. Datenträger (jeweils 1.023 GB)|Max. IOPS (300 pro Datenträger)|
|---|---|---|---|---|---|---|
|A0\Basic_A0|1|768 MB|2| 20 GB|1|1 x 300|
|A1\Basic_A1|1|1,75 GB|2| 40 GB |2|2 x 300|
|A2\Basic_A2|2|3,5 GB|2| 60 GB|4|4 x 300|
|A3\Basic_A3|4|7 GB|2| 120 GB |8|4 x 300|
|A4\Basic_A4|8|14 GB|2| 240 GB |16|16 x 300|
