
Die Ls-Serie ist für Workloads optimiert, für die ein lokaler Speicher mit geringer Wartezeit erforderlich ist, z.B. NoSQL-Datenbanken (z.B. Cassandra, MongoDB, Cloudera und Redis). Die Ls-Reihe bietet bis zu 32 CPU-Kerne unter Verwendung der [Prozessorfamilie Intel® Xeon® E5 v3](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). Dies ist die gleiche CPU-Leistung wie bei der G/GS-Serie mit 8 GiB Arbeitsspeicher pro CPU-Kern.  

## <a name="ls-series"></a>Ls-Serie

ACU: 180 - 240
 
| Größe          | CPU-Kerne | Arbeitsspeicher: GiB | Lokales SSD: GiB | Max. Anzahl Datenträger | Maximaler Datenträgerdurchsatz mit Cache und lokal: IOPS/MBit/s (Cachegröße in GiB) | Maximaler Datenträgerdurchsatz ohne Cache: IOPS / MB/s | Maximale Anzahl NICs/Netzwerkbandbreite | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s  | 4    | 32   | 678   | 8              | N/V/N/V (0)          | 5.000 / 125                               | 2/hoch       | 
| Standard_L8s  | 8    | 64   | 1.388 | 16             | N/V/N/V (0)          | 10.000/250                              | 4/sehr hoch  | 
| Standard_L16s | 16   | 128  | 2.807 | 32             | N/V/N/V (0)          | 20.000/500                              | 8/äußerst hoch | 
| Standard_L32s** | 32 | 256  | 5.630 | 64             | N/V/N/V (0)          | 40.000/1.000                            | 8/äußerst hoch | 
 
MB/s = 10^6 Bytes pro Sekunde und GB = 1.024^3 Bytes. 

*Der mit einer VM der Ls-Serie maximal mögliche Datenträgerdurchsatz (IOPS oder MBit/s) kann durch Anzahl, Größe und Striping angefügter Datenträger beschränkt sein. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/storage/storage-premium-storage.md). 

**Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.