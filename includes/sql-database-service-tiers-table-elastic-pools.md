<!--
Used in:
sql-database-elastic-pool.md   
sql-database-resource-limits.md
sql-database-service-tiers.md  
-->
 
### <a name="basic-elastic-pool-limits"></a>Grenzwerte für elastische Pools – Basic

| Poolgröße (eDTUs)  | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Max. Datenspeicher pro Pool* | 5 GB | 10 GB | 20 GB | 29 GB | 39 GB | 78 GB | 117 GB | 156 GB |
| Max. In-Memory-OLTP-Speicher pro Pool | N/V | N/V | N/V | N/V | N/V | N/V | N/V | N/V |
| Max. Anzahl Datenbanken pro Pool | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Max. gleichzeitige Worker (Anforderungen) pro Pool | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Max. gleichzeitige Anmeldungen pro Pool | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| Min. Anz. von eDTUs pro Datenbank | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} |
| Max. Anz. von eDTUs pro Datenbank | {5} | {5} | {5} | {5} | {5} | {5} | {5} | {5} | {5} |
| Max. Datenspeicher pro Datenbank | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB | 2 GB |
||||||||

### <a name="standard-elastic-pool-limits"></a>Grenzwerte für elastische Pools – Standard

| Poolgröße (eDTUs)  | **50** | **100** | **200** | **300** | **400** | **800** | 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Max. Datenspeicher pro Pool* | 50 GB| 100 GB| 200 GB | 300 GB| 400 GB | 800 GB | 
| Max. In-Memory-OLTP-Speicher pro Pool | N/V | N/V | N/V | N/V | N/V | N/V | 
| Max. Anzahl Datenbanken pro Pool | 100 | 200 | 500 | 500 | 500 | 500 | 
| Max. gleichzeitige Worker (Anforderungen) pro Pool | 100 | 200 | 400 | 600 |  800 | 1600 |
| Max. gleichzeitige Anmeldungen pro Pool | 100 | 200 | 400 | 600 |  800 | 1600 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| Min. Anz. von eDTUs pro Datenbank | {0,10,20,<br>50} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} |
| Max. Anz. von eDTUs pro Datenbank | {10,20,<br>50} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | 
| Max. Datenspeicher pro Datenbank | 250 GB | 250 GB | 250 GB | 250 GB | 250 GB | 250 GB |
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Grenzwerte für elastische Pools – Standard (Fortsetzung) 

| Poolgröße (eDTUs)  |  **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| Max. Datenspeicher pro Pool* | 1,2 TB | 1,6 TB | 2 TB | 2,4 TB | 2,9 TB | 
| Max. In-Memory-OLTP-Speicher pro Pool | N/V | N/V | N/V | N/V | N/V | 
| Max. Anzahl Datenbanken pro Pool | 500 | 500 | 500 | 500 | 500 | 500 |
| Max. gleichzeitige Worker (Anforderungen) pro Pool |  2400 | 3200 | 4000 | 5.000 | 6000 |
| Max. gleichzeitige Anmeldungen pro Pool |  2400 | 3200 | 4000 | 5.000 | 6000 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Min. Anz. von eDTUs pro Datenbank | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} |
| Max. Anz. von eDTUs pro Datenbank | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | 
| Max. Datenspeicher pro Datenbank | 250 GB | 250 GB | 250 GB | 250 GB | 250 GB | 250 GB |
||||||||

### <a name="premium-elastic-pool-limits"></a>Grenzwerte für elastische Pools – Premium

| Poolgröße (eDTUs)  | **125** | **250** | **500** | **1000** | **1500** **| 
|:---|---:|---:|---:| ---: | ---: | 
| Max. Datenspeicher pro Pool* | 250 GB | 500 GB | 750 GB | 750 GB | 1,5 TB | 
| Max. In-Memory-OLTP-Speicher pro Pool | 1 GB| 2 GB| 4 GB| 10 GB| 12 GB| 
| Max. Anzahl Datenbanken pro Pool | 50 | 100 | 100 | 100 | 100 |  
| Max. gleichzeitige Worker pro Pool (Anforderungen) | 200 | 400 | 800 | 1600 |  2400 | 
| Max. gleichzeitige Anmeldungen pro Pool | 200 | 400 | 800 | 1600 |  2400 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Min. Anz. von eDTUs pro Datenbank | {0,25,50,75,<br>125} | {0,25,50,75,<br>125,250} | {0,25,50,75,<br>125,250,500} | {0,25,50,75,<br>125,250,500,<br>1000} | {0,25,50,75,<br>125,250,500,<br>1000,1500} | 
| Max. Anz. von eDTUs pro Datenbank | {25,50,75,<br>125} | {25,50,75,<br>125,250} | {25,50,75,<br>125,250,500} | {25,50,75,<br>125,250,500,<br>1000} | {25,50,75,<br>125,250,500,<br>1000,1500} |
| Max. Datenspeicher pro Datenbank | 500 GB | 500 GB | 500 GB | 500 GB | 500 GB | 500 GB |
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Grenzwerte für elastische Pools – Premium (Fortsetzung) 

| Poolgröße (eDTUs)  |  **2000** **| **2500**  **| **3000**  **| **3500**  **| **4000**  **|
|:---|---:|---:|---:| ---: | ---: | 
| Max. Datenspeicher pro Pool* | 2 TB | 2,5 TB | 3 TB | 3,5 TB | 4 TB |
| Max. In-Memory-OLTP-Speicher pro Pool | 16 GB | 20 GB | 24 GB | 28 GB | 32 GB |
| Max. Anzahl Datenbanken pro Pool | 100 | 100 | 100 | 100 | 100 | 
| Max. gleichzeitige Worker (Anforderungen) pro Pool |  3200 | 4000 | 4800 | 5600 | 6400 |
| Max. gleichzeitige Anmeldungen pro Pool |  3200 | 4000 | 4800 | 5600 | 6400 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Min. Anz. von eDTUs pro Datenbank | {0,25,50,75,<br>125,250,500,<br>1000,1750} | {0,25,50,75,<br>125,250,500,<br>1000,1750} | {0,25,50,75,<br>125,250,500,<br>1000,1750} | {0,25,50,75,<br>125,250,500,<br>1000,1750} |  {0,25,50,75,<br>125,250,500,<br>1000,1750,4000} | 
| Max. Anz. von eDTUs pro Datenbank | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750,4000} | 
| Max. Datenspeicher pro Datenbank | 500 GB | 500 GB | 500 GB | 500 GB | 500 GB | 500 GB |
||||||||

### <a name="premium-rs-elastic-pool-limits"></a>Grenzwerte für elastische RS-Pools – Premium

| Poolgröße (eDTUs)  | **125** | **250** | **500** | **1000** |
|:---|---:|---:|---:| ---: | ---: | 
| Max. Datenspeicher pro Pool* | 250 GB| 500 GB | 750 GB | 750 GB |
| Max. In-Memory-OLTP-Speicher pro Pool | 1 GB | 2 GB | 4 GB | 10 GB |
| Max. Anzahl Datenbanken pro Pool | 50 | 100 | 100 | 100 |
| Max. gleichzeitige Worker (Anforderungen) pro Pool | 200 | 400 | 800 | 1600 |
| Max. gleichzeitige Anmeldungen pro Pool | 200 | 400 | 800 | 1600 |
| Max. gleichzeitige Sitzungen pro Pool | 30000 | 30000 | 30000 | 30000 |
| Min. Anz. von eDTUs pro Datenbank | {0,25,50,75,<br>125} | {0,25,50,75,<br>125,250} | {0,25,50,75,<br>125,250,500} | {0,25,50,75,<br>125,250,500,<br>1000} |
| Max. Anz. von eDTUs pro Datenbank | {25,50,75,<br>125} | {25,50,75,<br>125,250} | {25,50,75,<br>125,250,500} | {25,50,75,<br>125,250,500,<br>1000} | 
| Max. Datenspeicher pro Datenbank | 500 GB | 500 GB | 500 GB | 500 GB | 500 GB | 500 GB |
||||||||

> [!IMPORTANT]
>\* In einem Pool zusammengefasste Datenbanken nutzen den Poolspeicher gemeinsam, daher ist der Datenspeicher in einem Pool für elastische Datenbanken auf den jeweils kleineren Wert des verbleibenden Poolspeichers oder des maximalen Speicherplatzes pro Datenbank beschränkt. 
>
>\*\* Die standardmäßige maximale Datenspeicherkapazität pro Pool für Premium-Pools mit 1.500 eDTUs oder mehr beträgt 750 GB. Für die Nutzung einer höheren maximalen Datenspeichergröße pro Pool muss die entsprechende Größe explizit mit dem Azure-Portal oder [PowerShell](../articles/sql-database/sql-database-elastic-pool-manage-powershell.md#change-the-storage-limit-for-an-elastic-pool) ausgewählt werden. Premium-Pools mit einer Speicherkapazität von mehr als 750 GB sind derzeit in folgenden Regionen in der öffentlichen Vorschau verfügbar: USA, Osten 2; USA, Westen; USA Gov Virginia; Europa, Westen; Deutschland, Mitte; Asien, Südosten; Japan, Osten; Australien, Osten; Kanada, Mitte und Kanada, Osten. Die maximale Speicherkapazität pro Pool für alle anderen Regionen ist derzeit auf 750 GB beschränkt.
>
