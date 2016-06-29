**Datenträger virtueller Computer: Einschränkungen pro Konto**

Ressource|Standardlimit
---|---
Datenträgerkapazität insgesamt pro Konto|35 TB
Kapazität für Momentaufnahmen insgesamt pro Konto|10 TB
Max. Bandbreite pro Konto (Ein- und Ausgang<sup>1</sup>)|<= 50 GBit/s

<sup>1</sup>*Eingang* bezieht sich auf alle Daten (Anforderungen), die an ein Speicherkonto gesendet werden. *Ausgang* bezieht sich auf alle Daten (Antworten), die von einem Speicherkonto empfangen werden.

**Datenträger virtueller Computer: Einschränkungen pro Datenträger**

Datenträgertyp des Premium-Speichers | P10 | P20 | P30
---|---|---|---
Datenträgergröße | 128 GB | 512 GB | 1\.024 GiB (1 TB)
Max. IOPS pro Datenträger | 500 | 2\.300 | 5\.000
Max. Durchsatz pro Datenträger | 100 MB pro Sekunde | 150 MB pro Sekunde | 200 MB pro Sekunde
Maximale Anzahl von Datenträgern pro Speicherkonto | 280 | 70 | 35

**Datenträger virtueller Computer: Einschränkungen pro virtuellem Computer**

Ressource|Standardlimit
---|---
Max. IOPS pro virtuellem Computer|80\.000 IOPS mit GS5-VM<sup>1</sup>
Max. Durchsatz pro virtuellem Computer|2\.000 MB/s mit GS5-VM<sup>1</sup>

<sup>1</sup>Informationen zu Grenzwerten bei virtuellen Computern anderer Größen finden Sie unter [VM-Größe](../articles/virtual-machines/virtual-machines-linux-sizes.md).

<!---HONumber=AcomDC_0615_2016-->