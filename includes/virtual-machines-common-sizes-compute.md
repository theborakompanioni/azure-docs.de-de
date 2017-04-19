<!-- F-series, Fs-series* -->

Die F-Serie basiert auf dem Intel Xeon® E5-2673 v3-Prozessor (Haswell) mit 2,4 GHz, der mit der Intel Turbo Boost Technology 2.0 Taktfrequenzen von 3,1 GHz erreichen kann. Dies ist die gleiche CPU-Leistung wie bei virtuellen Computern (VMs) der Dv2-Serie.  Die F-Serie hat einen niedrigeren Listenpreis pro Stunde und bietet auf Basis der Azure-Berechnungseinheit ACU (Azure Compute Unit) pro Kern das beste Preis-Leistungs-Verhältnis im Azure-Portfolio. 

Virtuelle Computer der F-Serie sind eine hervorragende Wahl für Workloads, die schnellere CPUs erfordern, aber nicht so viel Arbeitsspeicher oder lokalen SSD pro CPU-Kern benötigen.  Bei Arbeitslasten wie Analysen, Gamingservern, Webservern und Batchverarbeitung kommen die Vorteile und der Nutzen der F-Serie besonders gut zum Tragen.

Die Fs-Serie verfügt zusätzlich zum Premium-Speicher über alle Vorteile der F-Serie.

## <a name="fs-series"></a>Fs-Serie*

ACU: 210 - 250

| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Maximaler Datenträgerdurchsatz mit Cache und lokal: IOPS/MBit/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |2 |4.000/32 (12) |3.200/48 |2/moderat |
| Standard_F2s |2 |4 |8 |4 |8.000/64 (24) |6.400/96 |2/hoch |
| Standard_F4s |4 |8 |16 |8 |16.000/128 (48) |12.800/192 |4/hoch |
| Standard_F8s |8 |16 |32 |16 |32.000/256 (96) |25.600/384 |8/hoch |
| Standard_F16s |16 |32 |64 |32 |64.000/512 (192) |51.200/768 |8/äußerst hoch |

MB/s = 10^6 Bytes pro Sekunde und GB = 1.024^3 Bytes.

* Der mit einer VM der Fs-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MB/s) kann durch Anzahl, Größe und Striping der angefügten Datenträger beschränkt werden.  Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/storage/storage-premium-storage.md).

<br>
## <a name="f-series"></a>F-Serie

ACU: 210 - 250

| Größe         | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Durchsatz lokale Datenträger: IOPS/Lesen (MBit/s)/Schreiben (MBit/s) | Max. Datenträger/Durchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_F1  | 1         | 2           | 16             | 3000/46/23                                           | 2/2 x 500                         | 2/moderat                 |
| Standard_F2  | 2         | 4           | 32             | 6000/93/46                                           | 4/4 x 500                         | 2/hoch                     |
| Standard_F4  | 4         | 8           | 64             | 12000/187/93                                         | 8/8 x 500                         | 4/hoch                     |
| Standard_F8  | 8         | 16          | 128            | 24000/375/187                                        | 16/16 x 500                       | 8/hoch                     |
| Standard_F16 | 16        | 32          | 256            | 48000/750/375                                        | 32/32 x 500                       | 8/äußerst hoch           |
<br>


