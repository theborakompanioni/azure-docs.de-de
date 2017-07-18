
## <a name="size-table-definitions"></a>Definitionen der Größentabelle

- Speicherkapazität wird in GiB-Einheiten oder 1.024^3 Bytes angezeigt. Beachten Sie beim Vergleich von in GB (1.000^3 Bytes) gemessenen Datenträgern mit in GiB (1.024^3) gemessenen Datenträgern, dass die in GiB angegebenen Kapazitätszahlen kleiner erscheinen können. Beispiel: 1.023 GiB = 1.098,4 GB.
- Der Datenträgerdurchsatz wird in E/A-Vorgängen pro Sekunde (Input/Output Operations Per Second, IOPS) und MB/s gemessen, wobei MB/s = 10^6 Bytes/Sekunde beträgt.
- Datenträger können mit oder ohne Cache betrieben werden.  Beim Datenträgerbetrieb mit Cache ist der Hostcachemodus auf **ReadOnly** oder **ReadWrite** festgelegt.  Beim Datenträgerbetrieb ohne Cache ist der Hostcachemodus auf **None**festgelegt.
- **Erwartete Netzwerkbandbreite** ist die maximale aggregierte Bandbreite, die pro VM-Typ NIC-übergreifend für alle Ziele zugeordnet ist. Obergrenzen werden zwar nicht garantiert, können aber bei der Wahl des passenden VM-Typs für die geplante Anwendung als Richtwert herangezogen werden. Die tatsächliche Netzwerkleistung hängt von unterschiedlichen Faktoren ab. Hierzu zählen beispielsweise Netzwerküberlastung, Anwendungslasten und die Netzwerkeinstellungen. Informationen zum Optimieren des Netzwerkdurchsatzes finden Sie unter [Optimieren des Netzwerkdurchsatzes für virtuelle Azure-Computer](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md). Unter Umständen muss eine bestimmte Version gewählt oder der virtuelle Computer optimiert werden, um die erwartete Netzwerkbandbreite unter Linux oder Windows zu erzielen. Weitere Informationen finden Sie unter [Testen der Bandbreite/des Durchsatzes (NTTTCP)](../articles/virtual-network/virtual-network-bandwidth-testing.md).

- &#8224; Bei 16 Kernen wird in einer kommenden Version konsequent die Leistungsobergrenze erreicht.


