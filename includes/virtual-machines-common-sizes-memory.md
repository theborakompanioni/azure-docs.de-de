

* Die Serien Dv2, D, G sowie deren DS/GS-Entsprechungen eignen sich ideal für Clientanwendungen, die schnellere CPUs oder bessere lokale Datenträgerleistung erfordern oder einen höheren Speicherbedarf haben.  Sie bieten eine leistungsfähige Kombination für viele Anwendungen für den Unternehmenseinsatz.

VMs der D-Serie dienen zum Ausführen von Anwendungen, die eine höhere Rechenleistung und eine höhere temporäre Datenträgerleistung erfordern. VMs der D-Serie bieten schnellere Prozessoren, ein höheres Verhältnis von Speicher zu Kern und ein SSD (Solid State Drive) für den temporären Datenträger. Einzelheiten finden Sie in der Ankündigung im Azure-Blog unter [New D-Series Virtual Machine Sizes](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/)(Neue VM-Größen der D-Serie, in englischer Sprache).

Die Dv2-Serie, eine Nachfolgerin der ursprünglichen D-Serie, hat eine leistungsfähigere CPU. Die CPU der Dv2-Serie ist ca. 35 % schneller als die CPU der D-Serie. Sie basiert auf der neuesten Generation des 2,4-GHz-Intel Xeon® E5-2673 v3-Prozessors (Haswell) und kann mit der Intel Turbo Boost Technology 2.0 bis auf 3,1 GHz erhöht werden. Die Dv2-Serie hat die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die D-Serie.


## <a name="gs-series"></a>GS-Serie*

ACU: 180 - 240

| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Maximaler Datenträgerdurchsatz mit Cache und lokal: IOPS/MBit/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_GS1 |2 |28 |56 |4 |10.000/100 (264) |5.000 / 125 |1/hoch |
| Standard_GS2 |4 |56 |112 |8 |20.000/200 (528) |10.000/250 |2/hoch |
| Standard_GS3 |8 |112 |224 |16 |40.000/400 (1.056) |20.000/500 |4/sehr hoch |
| Standard_GS4 |16 |224 |448 |32 |80.000/800 (2.112) |40.000/1.000 |8/äußerst hoch |
| Standard_GS5** |32 |448 |896 |64 |160.000/1.600 (4.224) |80.000/2.000 |8/äußerst hoch |

MB/s = 10^6 Bytes pro Sekunde und GB = 1.024^3 Bytes.

* Der mit einer VM der GS-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/storage/storage-premium-storage.md). 

**Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.
<br>

## <a name="g-series"></a>G-Serie

ACU: 180 - 240

| Größe         | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Durchsatz lokale Datenträger: IOPS/Lesen (MBit/s)/Schreiben (MBit/s) | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_G1  | 2         | 28          | 384            | 6000/93/46                                           | 4/4 x 500                       | 1/hoch                     |
| Standard_G2  | 4         | 56          | 768            | 12000/187/93                                         | 8/8 x 500                       | 2/hoch                     |
| Standard_G3  | 8         | 112         | 1.536          | 24000/375/187                                        | 16/16 x 500                     | 4/sehr hoch                |
| Standard_G4  | 16        | 224         | 3.072          | 48000/750/375                                        | 32/32 x 500                     | 8/äußerst hoch           |
| Standard_G5* | 32        | 448         | 6.144          | 96000/1500/750                                       | 64/64 x 500                     | 8/äußerst hoch           |

*Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.
<br>


## <a name="dsv2-series"></a>DSv2-Serie*

ACU: 210 - 250

| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Maximaler Datenträgerdurchsatz mit Cache: IOPS / MB/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- | --- |
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
Bit/s = 10^6 Byte pro Sekunde und GB = 1024^3 Byte.

* Der mit einer VM der DS-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden.  Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/storage/storage-premium-storage.md).

## <a name="dv2-series"></a>Dv2-Serie

ACU: 21 - 250

| Größe              | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Durchsatz lokale Datenträger: IOPS/Lesen (MBit/s)/Schreiben (MBit/s) | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000/93/46                                           | 4/4 x 500                         | 2/hoch                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12000/187/93                                         | 8/8 x 500                         | 4/hoch                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000/375/187                                        | 16/16 x 500                       | 8/hoch                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000/750/375                                        | 32/32 x 500                       | 8/äußerst hoch           |
| Standard_D15_v2** | 20        | 140         | 1.000          | 60000/937/468                                        | 40/40 x 500                       | 8/äußerst hoch*          |

*In einigen Regionen stehen für die Größe „Standard_D15_v2“ beschleunigte Netzwerke zur Verfügung. Weitere Informationen zur Verwendung und Verfügbarkeit finden Sie unter [Accelerated Networking befindet sich in der Vorschauphase](https://azure.microsoft.com/updates/accelerated-networking-in-preview/) sowie unter [Accelerated Networking für virtuelle Computer](../articles/virtual-network/virtual-network-accelerated-networking-powershell.md).

**Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.

<br>

## <a name="ds-series"></a>DS-Serie*

ACU: 160

| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Maximaler Datenträgerdurchsatz mit Cache: IOPS / MB/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11 |2 |14 |28 |4 |8.000/64 (72) |6.400/64 |2/hoch |
| Standard_DS12 |4 |28 |56 |8 |16.000/128 (144) |12.800/128 |4/hoch |
| Standard_DS13 |8 |56 |112 |16 |32.000/256 (288) |25.600/256 |8/hoch |
| Standard_DS14 |16 |112 |224 |32 |64.000/512 (576) |51.200/512 |8/sehr hoch |


MB/s = 10^6 Bytes pro Sekunde und GB = 1.024^3 Bytes.

* Der mit einer VM der DS-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden.  Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/storage/storage-premium-storage.md).

## <a name="d-series"></a>D-Serie

ACU: 160

| Größe         | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Durchsatz lokale Datenträger: IOPS/Lesen (MBit/s)/Schreiben (MBit/s) | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11 | 2         | 14          | 100            | 6000/93/46                                           | 4/4 x 500                         | 2/hoch                     |
| Standard_D12 | 4         | 28          | 200            | 12000/187/93                                         | 8/8 x 500                         | 4/hoch                     |
| Standard_D13 | 8         | 56          | 400            | 24000/375/187                                        | 16/16 x 500                       | 8/hoch                     |
| Standard_D14 | 16        | 112         | 800            | 48000/750/375                                        | 32/32 x 500                       | 8/sehr hoch                |
<br>


<br>

