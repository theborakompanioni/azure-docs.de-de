<!--
Used in:
sql-database-performance-guidance.md 
sql-database-single-database-resources.md 
-->

### <a name="basic-service-tier"></a>Basic-Dienstebene
| **Leistungsstufe** | **Basic** |
| :--- | --: |
| Max. DTU-Anzahl | 5 |
| Inbegriffener Speicher (GB) | 2 |
| Max. Speicherlösungen (GB) | 2 |
| Max. In-Memory-OLTP-Speicher (GB) |N/V |
| Max. gleichzeitige Worker (Anforderungen) | 30 |
| Maximale Anzahl gleichzeitiger Anmeldungen | 30 |
| Max. gleichzeitige Sitzungen | 300 |
|||

### <a name="standard-service-tier"></a>Standard-Dienstebene
| **Leistungsstufe** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|---:|
| Max. DTU-Anzahl | 10 | 20 | 50 | 100 |
| Inbegriffener Speicher (GB) | 250 | 250 | 250 | 250 |
| Max. Speicherlösungen (GB)* | 250 | 250 | 250 | 250, 500, 750, 1024 |
| Max. In-Memory-OLTP-Speicher (GB) | N/V | N/V | N/V | N/V |
| Max. gleichzeitige Worker (Anforderungen)| 60 | 90 | 120 | 200 |
| Maximale Anzahl gleichzeitiger Anmeldungen | 60 | 90 | 120 | 200 |
| Max. gleichzeitige Sitzungen |600 | 900 | 1200 | 2400 |
||||||

### <a name="standard-service-tier-continued"></a>Standard-Dienstebene (Fortsetzung)
| **Leistungsstufe** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|---:|
| Max. DTU-Anzahl | 200 | 400 | 800 | 1600 | 3000 |
| Inbegriffener Speicher (GB) | 250 | 250 | 250 | 250 | 250 |
| Max. Speicherlösungen (GB)* | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| Max. In-Memory-OLTP-Speicher (GB) | N/V | N/V | N/V | N/V |N/V |
| Max. gleichzeitige Worker (Anforderungen)| 400 | 800 | 1600 | 3200 |6000 |
| Maximale Anzahl gleichzeitiger Anmeldungen | 400 | 800 | 1600 | 3200 |6000 |
| Max. gleichzeitige Sitzungen |4800 | 9600 | 19200 | 30000 |30000 |
|||||||

### <a name="premium-service-tier"></a>Premium-Dienstebene 
| **Leistungsstufe** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** | 
| :--- |---:|---:|---:|---:|---:|---:|
| Max. DTU-Anzahl | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Inbegriffener Speicher (GB) | 500 | 500 | 500 | 500 | 4096 | 4096 |
| Max. Speicherlösungen (GB)* | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096 | 4096 |
| Max. In-Memory-OLTP-Speicher (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| Max. gleichzeitige Worker (Anforderungen)| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maximale Anzahl gleichzeitiger Anmeldungen | 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Max. gleichzeitige Sitzungen | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
|||||||

### <a name="premium-rs-service-tier"></a>Premium RS-Dienstebene 
| **Leistungsstufe** | **PRS1** | **PRS2** | **PRS4** | **PRS6** |
| :--- |---:|---:|---:|---:|---:|---:|
| Max. DTU-Anzahl | 125 | 250 | 500 | 1000 |
| Inbegriffener Speicher (GB) | 500 | 500 | 500 | 500 |
| Max. Speicherlösungen (GB)* | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 |
| Max. In-Memory-OLTP-Speicher (GB) | 1 | 2 | 4 | 8 |
| Max. gleichzeitige Worker (Anforderungen)| 200 | 400 | 800 | 1600 |
| Maximale Anzahl gleichzeitiger Anmeldungen | 200 | 400 | 800 | 1600 |
| Max. gleichzeitige Sitzungen | 30000 | 30000 | 30000 | 30000 |
|||||||

> [!IMPORTANT]
> \* Speichergrößen, die den integrierten Speicher überschreiten, befinden sich in der Vorschauphase und werden gegen Aufpreis bereitgestellt. Weitere Informationen finden Sie unter [SQL-Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/). 
>
>\* Im Premium-Tarif ist eine Speicherkapazität von mehr als 1 TB folgenden Regionen verfügbar: „USA, Osten 2“, „USA, Westen“, „USA Gov Virginia“, „Europa, Westen“, „Deutschland, Mitte“, „Asien, Südosten“, „Japan, Osten“, „Australien, Osten“, „Kanada, Mitte“ und „Kanada, Osten“. Siehe [P11-P15: Aktuelle Einschränkungen](../articles/sql-database/sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 
