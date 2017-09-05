<!--
Used in:
sql-database-elastic-pool.md 
-->

 
### <a name="basic-elastic-pool-limits"></a>Grenzwerte für elastische Pools – Basic

| eDTUs pro Pool | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Inbegriffener Speicher pro Pool (GB) | 5 | 10 | 20 | 29 | 11,9 | 78 | 117 | 156 |
| Max. Speicherlösungen pro Pool (GB) | 5 | 10 | 20 | 29 | 11,9 | 78 | 117 | 156 |
| Max. In-Memory-OLTP-Speicher pro Pool (GB) | N/V | N/V | N/V | N/V | N/V | N/V | N/V | N/V |
| Max. Anzahl Datenbanken pro Pool | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Max. gleichzeitige Worker (Anforderungen) pro Pool | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Max. gleichzeitige Anmeldungen pro Pool | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| Min. Anzahl von eDTU-Lösungen pro Datenbank | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| Max. Anzahl von eDTU-Lösungen pro Datenbank | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Max. Speicherkapazität pro Datenbank (GB) | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 
||||||||

### <a name="standard-elastic-pool-limits"></a>Grenzwerte für elastische Pools – Standard

| eDTUs pro Pool | **50** | **100** | **200** | **300** | **400** | **800**| 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Inbegriffener Speicher pro Pool (GB) | 50 | 100 | 200 | 300 | 400 | 800 | 
| Max. Speicherlösungen pro Pool (GB)* | 50, 250, 500 | 100, 250, 500, 750 | 200, 250, 500, 750, 1024 | 300, 500, 750, 1024, 1280 | 400, 500, 750, 1024, 1280, 1536 | 800, 1024, 1280, 1536, 1792, 2048 | 
| Max. In-Memory-OLTP-Speicher pro Pool (GB) | N/V | N/V | N/V | N/V | N/V | N/V | 
| Max. Anzahl Datenbanken pro Pool | 100 | 200 | 500 | 500 | 500 | 500 | 
| Max. gleichzeitige Worker (Anforderungen) pro Pool | 100 | 200 | 400 | 600 | 800 | 1600 |
| Max. gleichzeitige Anmeldungen pro Pool | 100 | 200 | 400 | 600 | 800 | 1600 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| Min. Anzahl von eDTU-Lösungen pro Datenbank** | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| Max. Anzahl von eDTU-Lösungen pro Datenbank** | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 | 
| Max. Speicherkapazität pro Datenbank (GB)* | 500 | 750 | 1024 | 1024 | 1024 | 1024 |
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Grenzwerte für elastische Pools – Standard (Fortsetzung) 

| eDTUs pro Pool | **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| Inbegriffener Speicher pro Pool (GB) | 1200 | 1600 | 2000 | 2500 | 3000 | 
| Max. Speicherlösungen pro Pool (GB)* | 1200, 1280, 1536, 1792, 2048, 2304, 2560 | 1600, 1792, 2048, 2304, 2560, 2816, 3072 | 2000, 2048, 2304, 2560, 2816, 3072, 3328, 3584 | 2500, 2560, 2816, 3072, 3328, 3584, 3840, 4096 | 3000, 3072, 3328, 3584, 3840, 4096 |
| Max. In-Memory-OLTP-Speicher pro Pool (GB) | N/V | N/V | N/V | N/V | N/V | 
| Max. Anzahl Datenbanken pro Pool | 500 | 500 | 500 | 500 | 500 | 
| Max. gleichzeitige Worker (Anforderungen) pro Pool | 2400 | 3200 | 4000 | 5.000 | 6000 |
| Max. gleichzeitige Anmeldungen pro Pool | 2400 | 3200 | 4000 | 5.000 | 6000 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Min. Anzahl von eDTU-Lösungen pro Datenbank** | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Max. Anzahl von eDTU-Lösungen pro Datenbank** | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 | 
| Max. Speicherlösungen pro Datenbank (GB)* | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-elastic-pool-limits"></a>Grenzwerte für elastische Pools – Premium

| eDTUs pro Pool | **125** | **250** | **500** | **1000** | **1500**| 
|:---|---:|---:|---:| ---: | ---: | 
| Inbegriffener Speicher pro Pool (GB) | 250 | 500 | 750 | 1024 | 1536 | 
| Max. Speicherlösungen pro Pool (GB)* | 250, 500, 750, 1024 | 500, 750, 1024 | 750, 1024 | 1024 | 1536 |
| Max. In-Memory-OLTP-Speicher pro Pool (GB) | 1 | 2 | 4 | 10 | 12 | 
| Max. Anzahl Datenbanken pro Pool | 50 | 100 | 100 | 100 | 100 | 
| Max. gleichzeitige Worker pro Pool (Anforderungen) | 200 | 400 | 800 | 1600 | 2400 | 
| Max. gleichzeitige Anmeldungen pro Pool | 200 | 400 | 800 | 1600 | 2400 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Min. Anz. von eDTUs pro Datenbank | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000, 1500 | 
| Max. Anz. von eDTUs pro Datenbank | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000, 1500 |
| Max. Speicherkapazität pro Datenbank (GB)* | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Grenzwerte für elastische Pools – Premium (Fortsetzung) 

| eDTUs pro Pool | **2000** | **2500** | **3000** | **3500** | **4000**|
|:---|---:|---:|---:| ---: | ---: | 
| Inbegriffener Speicher pro Pool (GB) | 2048 | 2.560 | 3072 | 3548 | 4096 |
| Max. Speicherlösungen pro Pool (GB)* | 2048 | 2.560 | 3072 | 3548 | 4096|
| Max. In-Memory-OLTP-Speicher pro Pool (GB) | 16 | 20 | 24 | 28 | 32 |
| Max. Anzahl Datenbanken pro Pool | 100 | 100 | 100 | 100 | 100 | 
| Max. gleichzeitige Worker (Anforderungen) pro Pool | 3200 | 4000 | 4800 | 5600 | 6400 |
| Max. gleichzeitige Anmeldungen pro Pool | 3200 | 4000 | 4800 | 5600 | 6400 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Min. Anzahl von eDTU-Lösungen pro Datenbank | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Max. Anzahl von eDTU-Lösungen pro Datenbank | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Max. Speicherkapazität pro Datenbank (GB)* | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-rs-elastic-pool-limits"></a>Grenzwerte für elastische RS-Pools – Premium

| eDTUs pro Pool | **125** | **250** | **500** | **1000** |
|:---|---:|---:|---:| ---: | ---: | 
| Inbegriffener Speicher pro Pool (GB) | 250 | 500 | 750 | 750 |
| Max. Speicherlösungen pro Pool (GB)* | 250, 500, 750, 1024 | 500, 750, 1024 | 750, 1024 | 1024 | 
| Max. In-Memory-OLTP-Speicher pro Pool (GB) | 1 | 2 | 4 | 10 |
| Max. Anzahl Datenbanken pro Pool | 50 | 100 | 100 | 100 |
| Max. gleichzeitige Worker (Anforderungen) pro Pool | 200 | 400 | 800 | 1600 |
| Max. gleichzeitige Anmeldungen pro Pool | 200 | 400 | 800 | 1600 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 |
| Min. Anzahl von eDTU-Lösungen pro Datenbank | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 |
| Max. Anzahl von eDTU-Lösungen pro Datenbank | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 
| Max. Speicherkapazität pro Datenbank (GB)* | 1024 | 1024 | 1024 | 1024 | 
||||||||

> [!IMPORTANT]
> \* Speichergrößen, die den integrierten Speicher überschreiten, befinden sich in der Vorschauphase und werden gegen Aufpreis bereitgestellt. Weitere Details finden Sie unter [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/). Speichergrößen, die den integrierten Speicher überschreiten, befinden sich in der Vorschauphase und werden gegen Aufpreis bereitgestellt. Weitere Details finden Sie unter [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/).
>
> \* Im Premium-Tarif ist eine Speicherkapazität von mehr als 1 TB folgenden Regionen verfügbar: „USA, Osten 2“, „USA, Westen“, „USA Gov Virginia“, „Europa, Westen“, „Deutschland, Mitte“, „Asien, Südosten“, „Japan, Osten“, „Australien, Osten“, „Kanada, Mitte“ und „Kanada, Osten“. 
>
>\*\* Die minimale bzw. maximale Anzahl von eDTUs pro Datenbank ab 200 eDTUs in **Standard**-Pools befindet sich in der Vorschauphase.
>
