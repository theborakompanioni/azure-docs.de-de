<!-- A-series - compute-intensive instances, H-series -->

Die Größen A8 bis A11 und die Größen der H-Reihe werden auch als *rechenintensive Instanzen* bezeichnet. Die Hardware, auf der diese Größen ausgeführt werden, wurde für rechenintensive Anwendungen mit hoher Netzwerkauslastung konzipiert und optimiert. Hierzu zählen beispielsweise HPC-Clusteranwendungen (High Performance Computing), Modellierung und Simulationen. Die Reihen A8 bis A11 nutzen Intel Xeon E5-2670 mit 2,6 GHZ und die H-Reihe Intel Xeon E5-2667 v3 mit 3,2 GHz. 

Virtuelle Azure-Computer der H-Reihe sind High Performing Computing-VMs der nächsten Generation für High-End-Berechnungsanforderungen, z.B. Molekülmodellierung und Strömungsdynamikberechnung. Diese virtuellen Computer mit 8 und 16 Kernen basieren auf der Intel Haswell E5-2667 V3-Prozessortechnologie mit DDR4-Speicher und lokalem SSD-Speicher. 

Neben beträchtlicher CPU-Leistung bietet die H-Serie verschiedene Optionen für RDMA-Netzwerke mit niedriger Latenz unter Verwendung von FDR InfiniBand sowie verschiedene Speicherkonfigurationen für Berechnungsanforderungen mit hohem Speicherbedarf.



## <a name="h-series"></a>H-Reihe

ACU: 290 - 300

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



## <a name="a-series---compute-intensive-instances"></a>A-Serie: Rechenintensive Instanzen

ACU: 225

| Größe | CPU-Kerne | Arbeitsspeicher: GiB | Lokales HDD: GiB | Max. Anzahl Datenträger | Max. Datenträger-Datendurchsatz: IOPS | Maximale Anzahl NICs/Netzwerkbandbreite |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8* |8 |56 |382 |16 |16 x 500 |2/hoch |
| Standard_A9* |16 |112 |382 |16 |16 x 500 |4/sehr hoch |
| Standard_A10 |8 |56 |382 |16 |16 x 500 |2/hoch |
| Standard_A11 |16 |112 |382 |16 |16 x 500 |4/sehr hoch |

* RDMA-fähig

<br>



