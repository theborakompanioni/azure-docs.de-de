
> [AZURE.IMPORTANT] *Elastische Datenbanken nutzen den Poolspeicher gemeinsam, daher ist der Datenbankspeicher auf den jeweils kleineren Wert des verbleibenden Poolspeichers oder des maximalen Speicherplatzes pro Datenbank beschränkt.

### Grenzwerte für elastische Pools – Basic

| | |
|---|:---:|
| Maximale Anzahl von eDTUs pro Pool | &nbsp;100 &nbsp;&nbsp;&nbsp; 200 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 400 &nbsp;&nbsp;&nbsp;&nbsp; 800 &nbsp;&nbsp;&nbsp;&nbsp; 1200 |
| Max. Speicherkapazität pro Pool (GB)*| &nbsp;&nbsp;&nbsp;&nbsp;10 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;20 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;39 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;73 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;117 |
| Max. Anzahl von Datenbanken pro Pool | &nbsp;&nbsp;&nbsp;200 &nbsp;&nbsp;&nbsp;&nbsp;400 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;400 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;400 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;400 |
| Max. In-Memory-OLTP-Speicher pro Pool (GB)| – |
| Max. gleichzeitige Worker pro Pool | &nbsp;&nbsp;&nbsp;200 &nbsp;&nbsp; 400 &nbsp;&nbsp;&nbsp;&nbsp; 800 &nbsp;&nbsp;&nbsp; 1600 &nbsp;&nbsp;&nbsp;&nbsp;2400 |
| Max. gleichzeitige Anmeldungen pro Pool | &nbsp;&nbsp;&nbsp;200 &nbsp;&nbsp; 400 &nbsp;&nbsp;&nbsp;&nbsp; 800 &nbsp;&nbsp;&nbsp; 1600 &nbsp;&nbsp;&nbsp;&nbsp;2400 |
| Max. gleichzeitige Sitzungen pro Pool | 4800 &nbsp;9600 &nbsp; 19.200 &nbsp; 28.800 &nbsp; 28.800 |
| Max. Anz. von eDTUs pro Datenbank | 5 |
| Min. Anz. von eDTUs pro Datenbank | 0,5 |
| Max. Speicherkapazität pro Datenbank (GB)* | 2 |
| Point-in-Time-Wiederherstellung | Beliebiger Punkt der letzten sieben Tage |
| Notfallwiederherstellung | Aktive Georeplikation |
|||

### Grenzwerte für elastische Pools – Standard

| | |
|---|:---:|
| Maximale Anzahl von eDTUs pro Pool | &nbsp;100 &nbsp;&nbsp;&nbsp; 200 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 400 &nbsp;&nbsp;&nbsp;&nbsp; 800 &nbsp;&nbsp;&nbsp;&nbsp; 1200 |
| Max. Speicherkapazität pro Pool (GB)*| &nbsp;100 &nbsp;&nbsp;&nbsp; 200 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 400 &nbsp;&nbsp;&nbsp;&nbsp; 800 &nbsp;&nbsp;&nbsp;&nbsp; 1200 |
| Max. Anzahl von Datenbanken pro Pool | &nbsp;200 &nbsp;&nbsp;&nbsp;&nbsp;400 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;400 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;400 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;400 |
| Max. In-Memory-OLTP-Speicher pro Pool (GB)| – |
| Max. gleichzeitige Worker pro Pool | &nbsp;&nbsp;200 &nbsp;&nbsp;&nbsp; 750 &nbsp;&nbsp;&nbsp; 1300 &nbsp;&nbsp; 1850 &nbsp;&nbsp;&nbsp; 2400 |
| Max. gleichzeitige Anmeldungen pro Pool | &nbsp;&nbsp;200 &nbsp;&nbsp;&nbsp; 750 &nbsp;&nbsp;&nbsp; 1300 &nbsp;&nbsp; 1850 &nbsp;&nbsp;&nbsp; 2400 |
| Max. gleichzeitige Sitzungen pro Pool | 4800 &nbsp; 9600 &nbsp;19.200 &nbsp;28.800 &nbsp;&nbsp; 28.800 |
| Max. Anz. von eDTUs pro Datenbank | 10, 20, 50, 100 |
| Min. Anz. von eDTUs pro Datenbank | 0, 10, 20, 50, 100 |
| Max. Speicherkapazität pro Datenbank (GB)* | 250 |
| Point-in-Time-Wiederherstellung | Beliebiger Punkt der letzten 35 Tage |
| Notfallwiederherstellung | Aktive Georeplikation |


### Grenzwerte für elastische Pools – Premium

| | |
|---|:---:|
| Maximale Anzahl von eDTUs pro Pool | 125 &nbsp;&nbsp;&nbsp; 250 &nbsp;&nbsp;&nbsp; 500 &nbsp;&nbsp;&nbsp; 1000 &nbsp;&nbsp;&nbsp; &nbsp;1500 |
| Max. Speicherkapazität pro Pool (GB)*| 250 &nbsp;&nbsp;&nbsp; 500 &nbsp;&nbsp;&nbsp; 750 &nbsp;&nbsp;&nbsp;&nbsp; 750 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 750 |
| Max. Anzahl von Datenbanken pro Pool | 50 |
| Max. In-Memory-OLTP-Speicher pro Pool (GB)| – |
| Max. gleichzeitige Worker pro Pool | &nbsp;&nbsp;200 &nbsp;&nbsp;&nbsp; 750 &nbsp;&nbsp;&nbsp; 1300 &nbsp;&nbsp; 1850 &nbsp;&nbsp;&nbsp; 2400 |
| Max. gleichzeitige Anmeldungen pro Pool | &nbsp;&nbsp;200 &nbsp;&nbsp;&nbsp; 750 &nbsp;&nbsp;&nbsp; 1300 &nbsp;&nbsp; 1850 &nbsp;&nbsp;&nbsp; 2400 |
| Max. gleichzeitige Sitzungen pro Pool | 4800 &nbsp; 9600 &nbsp;19.200 &nbsp;28.800 &nbsp;&nbsp; 28.800 |
| Max. Anz. von eDTUs pro Datenbank | 125, 250, 500, 1000 |
| Min. Anz. von eDTUs pro Datenbank | 0, 125, 250, 500, 1000 |
| Max. Speicherkapazität pro Datenbank (GB)* | 500 |
| Point-in-Time-Wiederherstellung | Beliebiger Punkt der letzten 35 Tage |
| Notfallwiederherstellung | Aktive Georeplikation |

<!---HONumber=AcomDC_0817_2016-->