<!--
Used in:
sql-database-performance-guidance.md  
sql-database-resource-limits.md
sql-database-service-tiers.md  
-->

### <a name="basic-service-tier"></a>Basic-Dienstebene
| **Leistungsstufe** | **Basic** |
| --- | :---: |
| Max. DTU-Anzahl | 5 |
| Max. Datenbankgröße* |2 GB|
| Max. In-Memory-OLTP-Speicher |– |
| Max. gleichzeitige Worker |30 |
| Maximale Anzahl gleichzeitiger Anmeldungen |30 |
| Max. gleichzeitige Sitzungen |300 |
|||

### <a name="standard-service-tier"></a>Standard-Dienstebene
| **Leistungsstufe** | **S0** | **S1** | **S2** | **S3** |
| --- |---:| ---:|---:|---:|---:|
| Max. DTU-Anzahl | 10 | 20 | 50 | 100 |
| Max. Datenbankgröße* | 250 GB| 250 GB | 250 GB | 250 GB |
| Max. In-Memory-OLTP-Speicher | – | N/V | N/V | N/V |
| Max. gleichzeitige Worker | 60 | 90 | 120 | 200 |
| Maximale Anzahl gleichzeitiger Anmeldungen | 60 | 90 | 120 | 200 |
| Max. gleichzeitige Sitzungen |600 | 900 | 1200 | 2400 |
||||||

### <a name="premium-service-tier"></a>Premium-Dienstebene 
| **Leistungsstufe** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** | 
| --- |---:|---:|---:|---:|---:|---:|
| Max. DTU-Anzahl | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Max. Datenbankgröße* | 500 GB | 500 GB | 500 GB | 500 GB | 4 TB* | 4 TB* |
| Max. In-Memory-OLTP-Speicher | 1 GB | 2 GB | 4 GB | 8 GB | 14 GB | 32 GB |
| Max. gleichzeitige Worker | 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maximale Anzahl gleichzeitiger Anmeldungen | 200 | 400| 800| 1600| 2400| 6400 |
| Max. gleichzeitige Sitzungen | 30000| 30000| 30000| 30000| 30000| 30000 |
|||||||

### <a name="premium-rs-service-tier"></a>Premium RS-Dienstebene 
| **Leistungsstufe** | **PRS1** | **PRS2** | **PRS4** | **PRS6** |
| --- |---:|---:|---:|---:|---:|---:|
| Max. DTU-Anzahl | 125 | 250 | 500 | 1000 |
| Max. Datenbankgröße* | 500 GB | 500 GB | 500 GB | 500 GB |
| Max. In-Memory-OLTP-Speicher | 1 GB | 2 GB | 4 GB | 8 GB |
| Max. gleichzeitige Worker | 200 | 400 | 800 | 1600 |
| Maximale Anzahl gleichzeitiger Anmeldungen | 200 | 400| 800| 1600|
| Max. gleichzeitige Sitzungen | 30000| 30000| 30000| 30000|
|||||||

\* Die maximale Datenbankgröße bezieht sich auf die maximale Größe der Daten in der Datenbank. 