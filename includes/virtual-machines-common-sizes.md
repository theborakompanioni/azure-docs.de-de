
Informationen zu den allgemeinen Einschränkungen von virtuellen Azure-Geräten finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../articles/azure-subscription-service-limits.md).

Die Standardgrößen bestehen aus mehreren Serien: A, D, DS, F, Fs, G und GS. Beachten Sie für einige dieser Größen die folgenden Punkte:

*   VMs der D-Serie dienen zum Ausführen von Anwendungen, die eine höhere Rechenleistung und eine höhere temporäre Datenträgerleistung erfordern. VMs der D-Serie bieten schnellere Prozessoren, ein höheres Verhältnis von Speicher zu Kern und ein SSD (Solid State Drive) für den temporären Datenträger. Einzelheiten finden Sie in der Ankündigung im Azure-Blog unter [New D-Series Virtual Machine Sizes](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/) (Neue VM-Größen der D-Serie, in englischer Sprache).

*   Die Dv2-Serie, eine Nachfolgerin der ursprünglichen D-Serie, hat eine leistungsfähigere CPU. Die CPU der Dv2-Serie ist ca. 35 % schneller als die CPU der D-Serie. Sie basiert auf der neuesten Generation des 2,4-GHz-Intel Xeon® E5-2673 v3-Prozessors (Haswell) und kann mit der Intel Turbo Boost Technology 2.0 bis auf 3,1 GHz erhöht werden. Die Dv2-Serie hat die gleichen Arbeitsspeicher- und Datenträgerkonfigurationen wie die D-Serie.

* Die F-Serie basiert auf dem Intel Xeon® E5-2673 v3-Prozessor (Haswell) mit 2,4 GHz, der mit der Intel Turbo Boost Technology 2.0 Taktfrequenzen von 3,1 GHz erreichen kann. Dies ist die gleiche CPU-Leistung wie bei virtuellen Computern (VMs) der Dv2-Serie. Die F-Serie hat einen niedrigeren Listenpreis pro Stunde und bietet auf Basis der Azure-Berechnungseinheit ACU (Azure Compute Unit) pro Kern das beste Preis-Leistungs-Verhältnis im Azure-Portfolio.

	Bei der F-Serie wird außerdem ein neuer Standard bei der Benennung von VM-Größen in Azure eingeführt. Bei dieser Serie und bei künftig freigegebenen VM-Größen entspricht der hinter dem Buchstaben für den Familiennamen angegebene Zahlenwert der Anzahl von CPU-Kernen. Weitere Eigenschaften und Fähigkeiten, wie z.B. die Optimierung für Storage Premium, werden nach dem Zahlenwert für die Anzahl von CPU-Kernen durch Buchstaben angegeben. Dieses Namensformat wird für zukünftig freigegebene VM-Größen verwendet. Die Namen von bestehenden und freigegebenen VM-Größen werden jedoch nicht rückwirkend geändert.


*   Virtuelle Computer der G-Serie bieten den meisten Arbeitsspeicher und werden auf Hosts mit Prozessoren der Intel Xeon E5 V3-Familie ausgeführt.


*   Virtuelle Computer der DS-, DSv2-, Fs- und der GS-Serie können Storage Premium nutzen, wobei Hochleistungsspeicher mit geringer Latenz für E/A-intensive Workloads geboten wird. Diese virtuellen Computer nutzen Solid State Drives (SSDs) zum Hosten der Datenträger eines virtuellen Computers und stellen zudem einen lokalen SSD-Datenträgercache bereit. Premium-Speicher ist in bestimmten Regionen verfügbar. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/storage/storage-premium-storage.md).


*   Die VMs der A-Reihe können auf vielen verschiedenen Hardwaretypen und Prozessoren bereitgestellt werden. Die Größe ist basierend auf der Hardware gedrosselt, um eine konsistente Prozessorleistung für die ausgeführte Instanz zu ermöglichen – unabhängig von der Hardware, die für die Bereitstellung gewählt wird. Fragen Sie die virtuelle Hardware über die virtuelle Maschine ab, um die physische Hardware zu ermitteln, auf der diese Größe bereitgestellt wird.

*   Die Größe „A0“ ist auf der physischen Hardware „überzeichnet“. Nur für diese spezielle Größe kann es dazu kommen, dass sich andere Kundenbereitstellungen negativ auf die Leistung Ihrer aktiven Workload auswirken. Die relative Leistung ist unten als erwartete Baseline beschrieben und unterliegt einer ungefähren Variabilität von 15 Prozent.


Die Größe des virtuellen Computers wirkt sich auf den Preis aus. Die Größe beeinflusst auch die Verarbeitung, den Arbeitsspeicher und die Speicherkapazität des virtuellen Computers. Speicherkosten werden separat basierend auf bereits verwendete Seiten im Speicherkonto berechnet. Weitere Informationen finden Sie unter [Virtuelle Computer – Preisdetails](https://azure.microsoft.com/pricing/details/virtual-machines/) und [Preisdetails zu Azure Storage](https://azure.microsoft.com/pricing/details/storage/).


Die folgenden Überlegungen können Ihnen bei der Entscheidung über die Größe behilflich sein:


* Die Größen A8 bis A11 werden auch als *rechenintensive Instanzen* bezeichnet. Die Hardware, auf der diese Größen ausgeführt werden, wurde für rechenintensive Anwendungen mit hoher Netzwerkauslastung konzipiert und optimiert. Hierzu zählen beispielsweise HPC-Clusteranwendungen (High Performance Computing), Modellierung und Simulationen. Ausführliche Informationen und Überlegungen zum Verwenden dieser Größen finden Sie unter [Informationen zu den rechenintensiven A8-, A9-, A10- und A11-Instanzen](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).


*	Die Serien Dv2, D, G sowie deren DS/GS-Entsprechungen eignen sich ideal für Clientanwendungen, die schnellere CPUs oder bessere lokale Datenträgerleistung erfordern oder einen höheren Speicherbedarf haben. Sie bieten eine leistungsfähige Kombination für viele Anwendungen für den Unternehmenseinsatz.

* Virtuelle Computer der F-Serie sind eine hervorragende Wahl für Arbeitslasten, die schnellere CPUs erfordern, aber nicht so viel Arbeitsspeicher oder lokalen SSD pro CPU-Kern benötigen. Bei Arbeitslasten wie Analysen, Gamingservern, Webservern und Batchverarbeitung kommen die Vorteile und der Nutzen der F-Serie besonders gut zum Tragen.

*   Einige der physischen Hosts in Azure-Rechenzentren unterstützen möglicherweise keine der größeren VM-Größen, z.B. A5 bis A11. Daher wird möglicherweise die Fehlermeldung **Fehler beim Konfigurieren des virtuellen Computers <Computername>** oder **Fehler beim Erstellen des virtuellen Computers <Computername>** beim Ändern der Größe eines vorhandenen virtuellen Computers, beim Erstellen eines neuen virtuellen Computers in einem virtuellen Netzwerk, das vor dem 16. April 2013 erstellt wurde, oder beim Hinzufügen eines neuen virtuellen Computers zu einem vorhandenen Clouddienst angezeigt. Problemumgehungen für die einzelnen Bereitstellungsszenarios finden Sie unter [Error: “Failed to configure virtual machine”](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) (in englischer Sprache) im Supportforum.


## Überlegungen zur Leistung

Wir haben das Konzept der Azure-Berechnungseinheit ACU (Azure Compute Unit) erstellt, um eine Möglichkeit zum Vergleichen der Rechenleistung (CPU) zwischen den Azure-SKUs zu ermöglichen. Auf diese Weise können Sie leicht feststellen, welche SKU Ihren Leistungsanforderungen am ehesten entspricht. Zurzeit ist der ACU-Wert auf einem kleinen virtuellen Computer (Standard\_A1) auf den Standardwert 100 festgelegt, und an den übrigen SKUs kann ungefähr abgelesen werden, wie viel schneller die jeweilige SKU einen Standard-Benchmarktest ausführen kann.

>[AZURE.IMPORTANT] Die ACU ist nur ein Richtwert. Die Ergebnisse für Ihre Workload können abweichen.

<br>

|SKU-Familie |ACU/Kern |
|---|---|
|[Standard\_A0](#a-series) |50 |
|[Standard\_A1-4](#a-series) |100 |
|[Standard\_A5-7](#a-series) |100 |
|[A8-A11](#a-series) |225*|
|[D1-14](#d-series) |160 |
|[D1-15v2](#dv2-series) |210 - 250*|
|[DS1-14](#ds-series) |160 |
|[DS1-15v2](#dsv2-series) |210-250* |
|[F1-F16](#f-series) | 210-250*|
|[F1s-F16s](#fs-series) | 210-250*|
|[G1-5](#g-series) |180 - 240*|
|[GS1-5](#gs-series) |180 - 240*|


Mit * gekennzeichnete ACUs verwenden die Intel® Turbo-Technologie, um die CPU-Frequenz zu erhöhen eine Leistungssteigerung zu erzielen. Das Maß der Leistungssteigerung variiert basierend auf der Größe und Workload des virtuellen Computers sowie auf anderen Workloads, die auf dem gleichen Host ausgeführt werden.

## Größentabellen

In den folgenden Tabellen sind die Größe und die von den einzelnen Größen bereitgestellte Kapazität aufgeführt.

* Die Speicherkapazität wird unter Verwendung von 1024^3 Bytes als Maßeinheit für GB dargestellt. Dies wird manchmal als Gibibyte oder Binärdefinition (bzw. Definition zur Basis 2) bezeichnet. Denken Sie beim Vergleichen von Größen, die andere Basissysteme verwenden, jedoch daran, dass eine als Binärwert angegebene Größe möglicherweise kleiner als ein Dezimalwert zur Basis 10 erscheint (z.B. 1 GB), das Binärsystem jedoch mehr Kapazität als das Dezimalsystem bietet, da 1024^3 größer als 1000^3 ist.

* Bei der maximalen Netzwerkbandbreite handelt es sich um die maximale aggregierte Bandbreite, die pro VM-Typ zugewiesen wurde. Die maximale Bandbreite dient als Orientierungshilfe bei der Wahl des richtigen VM-Typs, um sicherzustellen, dass ausreichend Netzwerkkapazität verfügbar ist. Bei einem Wechsel zu „niedrig“, „mittel“, „hoch“ oder „sehr hoch“ ändert sich der Durchsatz entsprechend. Die tatsächliche Netzwerkleistung hängt von zahlreichen Faktoren ab. Hierzu zählen beispielsweise die Netzwerk- und Anwendungslast und die Netzwerkeinstellungen der Anwendung.


## A-Serie

|Größe |CPU-Kerne|Arbeitsspeicher|Netzwerkkarten (max.)|Maximale Datenträgergröße|Max. Datenträger (jeweils 1023 GB)|Max. IOPS (500 pro Datenträger)| Maximale Netzwerkbandbreite |
|---|---|---|---|---|---|---|---|
|Standard\_A0 |1|768 MB|1| Temporär = 20 GB |1|1 x 500| niedrig |
|Standard\_A1 |1|1,75 GB|1|Temporär = 70 GB |2|2 x 500| mittel |
|Standard\_A2 |2|3,5 GB|1|Temporär = 135 GB |4|4 x 500| mittel |
|Standard\_A3 |4|7 GB|2|Temporär = 285 GB |8|8 x 500| hoch |
|Standard\_A4 |8|14 GB|4|Temporär = 605 GB |16|16 x 500| hoch |
|Standard\_A5 |2|14 GB|1|Temporär = 135 GB |4|4 x 500| mittel |
|Standard\_A6 |4|28 GB|2|Temporär = 285 GB |8|8 x 500| hoch |
|Standard\_A7 |8|56 GB|4|Temporär = 605 GB |16|16 x 500| hoch |



## A-Serie: Rechenintensive Instanzen

Informationen und Überlegungen zum Verwenden dieser Größen finden Sie unter [Informationen zu den rechenintensiven A8-, A9-, A10- und A11-Instanzen](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).

|Größe |CPU-Kerne|Arbeitsspeicher|Netzwerkkarten (max.)|Maximale Datenträgergröße|Max. Datenträger (jeweils 1023 GB)|Max. IOPS (500 pro Datenträger)| Maximale Netzwerkbandbreite |
|---|---|---|---|---|---|---|---|
|Standard\_A8|8|56 GB|2| Temporär = 382 GB |16|16 x 500| hoch |
|Standard\_A9|16|112 GB|4| Temporär = 382 GB |16|16 x 500| sehr hoch |
|Standard\_A10|8|56 GB|2| Temporär = 382 GB |16|16 x 500| hoch |
|Standard\_A11|16|112 GB|4| Temporär = 382 GB |16|16 x 500| sehr hoch |

## D-Serie

|Größe |CPU-Kerne|Arbeitsspeicher|Netzwerkkarten (max.)|Maximale Datenträgergröße|Max. Datenträger (jeweils 1023 GB)|Max. IOPS (500 pro Datenträger)| Maximale Netzwerkbandbreite |
|---|---|---|---|---|---|---|---|
|Standard\_D1 |1|3,5 GB|1|Temporär (SSD) =50 GB |2|2 x 500| mittel |
|Standard\_D2 |2|7 GB|2|Temporär (SSD) = 100 GB |4|4 x 500| hoch |
|Standard\_D3 |4|14 GB|4|Temporär (SSD) = 200 GB |8|8 x 500| hoch |
|Standard\_D4 |8|28 GB|8|Temporär (SSD) = 400 GB |16|16 x 500| hoch |
|Standard\_D11 |2|14 GB|2|Temporär (SSD) = 100 GB |4|4 x 500| hoch |
|Standard\_D12 |4|28 GB|4|Temporär (SSD) = 200 GB |8|8 x 500| hoch |
|Standard\_D13 |8|56 GB|8|Temporär (SSD) = 400 GB |16|16 x 500| hoch |
|Standard\_D14 |16|112 GB|8|Temporär (SSD) = 800 GB |32|32 x 500| sehr hoch |


## Dv2-Serie

|Größe |CPU-Kerne|Arbeitsspeicher|Netzwerkkarten (max.)|Maximale Datenträgergröße|Max. Datenträger (jeweils 1023 GB)|Max. IOPS (500 pro Datenträger)| Maximale Netzwerkbandbreite |
|---|---|---|---|---|---|---|---|
|Standard\_D1\_v2 |1|3,5 GB|1|Temporär (SSD) =50 GB |2|2 x 500| mittel |
|Standard\_D2\_v2 |2|7 GB|2|Temporär (SSD) = 100 GB |4|4 x 500| hoch |
|Standard\_D3\_v2 |4|14 GB|4|Temporär (SSD) = 200 GB |8|8 x 500| hoch |
|Standard\_D4\_v2 |8|28 GB|8|Temporär (SSD) = 400 GB |16|16 x 500| hoch |
|Standard\_D5\_v2 |16|56 GB|8|Temporär (SSD) = 800 GB |32|32 x 500| äußerst hoch |
|Standard\_D11\_v2 |2|14 GB|2|Temporär (SSD) = 100 GB |4|4 x 500| hoch |
|Standard\_D12\_v2 |4|28 GB|4|Temporär (SSD) = 200 GB |8|8 x 500| hoch |
|Standard\_D13\_v2 |8|56 GB|8|Temporär (SSD) = 400 GB |16|16 x 500| hoch |
|Standard\_D14\_v2 |16|112 GB|8|Temporär (SSD) = 800 GB |32|32 x 500| äußerst hoch |
|Standard\_D15\_v2 |20|140 GB|10|Temporär (SSD) = 1 TB |40|40 x 500| äußerst hoch |


## DS-Serie*

|Größe |CPU-Kerne|Arbeitsspeicher|Netzwerkkarten (max.)|Maximale Datenträgergröße|Max. Datenträger (jeweils 1023 GB)|Cachegröße (GB)|Max. Datenträger-IOPS &amp; Bandbreite| Maximale Netzwerkbandbreite |
|---|---|---|---|---|---|---|---|---|
|Standard\_DS1 |1|3,5|1|Lokales SSD-Laufwerk = 7 GB |2|43| 3\.200, 32 MB pro Sekunde | mittel |
|Standard\_DS2 |2|7|2|Lokales SSD-Laufwerk = 14 GB |4|86| 6\.400, 64 MB pro Sekunde | hoch |
|Standard\_DS3 |4|14|4|Lokales SSD-Laufwerk = 28 GB |8|172| 12\.800, 128 MB pro Sekunde | hoch |
|Standard\_DS4 |8|28|8|Lokales SSD-Laufwerk = 56 GB |16|344| 25\.600, 256 MB pro Sekunde | hoch |
|Standard\_DS11 |2|14|2|Lokales SSD-Laufwerk = 28 GB |4|72| 6\.400, 64 MB pro Sekunde | hoch |
|Standard\_DS12 |4|28|4|Lokales SSD-Laufwerk = 56 GB |8|144| 12\.800, 128 MB pro Sekunde | hoch |
|Standard\_DS13 |8|56|8|Lokales SSD-Laufwerk = 112 GB |16|288| 25\.600, 256 MB pro Sekunde | hoch |
|Standard\_DS14 |16|112|8|Lokales SSD-Laufwerk = 224 GB |32|576| 51\.200, 512 MB pro Sekunde | sehr hoch |

*Die bei einer DS-Serie möglichen Höchstwerte für E/A-Vorgänge pro Sekunde (IOPS) und Durchsatz (Bandbreite) werden durch die Größe des Datenträgers beeinflusst. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/storage/storage-premium-storage.md).


## DSv2-Serie*

|Größe |CPU-Kerne|Arbeitsspeicher|Netzwerkkarten (max.)|Maximale Datenträgergröße|Max. Datenträger (jeweils 1023 GB)|Cachegröße (GB)|Max. Datenträger-IOPS &amp; Bandbreite| Maximale Netzwerkbandbreite |
|---|---|---|---|---|---|---|---|---|
|Standard\_DS1\_v2 |1|3,5|1|Lokales SSD-Laufwerk = 7 GB |2|43| 3\.200 48 MB pro Sekunde | mittel |
|Standard\_DS2\_v2 |2|7|2|Lokales SSD-Laufwerk = 14 GB |4|86| 6\.400 96 MB pro Sekunde | hoch |
|Standard\_DS3\_v2 |4|14|4|Lokales SSD-Laufwerk = 28 GB |8|172| 12\.800 192 MB pro Sekunde | hoch |
|Standard\_DS4\_v2 |8|28|8|Lokales SSD-Laufwerk = 56 GB |16|344| 25\.600 384 MB pro Sekunde | hoch |
|Standard\_DS5\_v2 |16|56|8|Lokales SSD-Laufwerk = 112 GB |32|688| 51\.200 768 MB pro Sekunde | äußerst hoch |
|Standard\_DS11\_v2 |2|14|2|Lokales SSD-Laufwerk = 28 GB |4|72| 6\.400 96 MB pro Sekunde | hoch |
|Standard\_DS12\_v2 |4|28|4|Lokales SSD-Laufwerk = 56 GB |8|144| 12\.800 192 MB pro Sekunde | hoch |
|Standard\_DS13\_v2 |8|56|8|Lokales SSD-Laufwerk = 112 GB |16|288| 25\.600 384 MB pro Sekunde | hoch |
|Standard\_DS14\_v2 |16|112|8|Lokales SSD-Laufwerk = 224 GB |32|576| 51\.200 768 MB pro Sekunde | äußerst hoch |
|Standard\_DS15\_v2 |20|140 GB|10|Lokales SSD-Laufwerk = 280 GB |40| 720|64\.000 960 MB pro Sekunde | äußerst hoch |


*Die bei einer DS-Serie möglichen Höchstwerte für E/A-Vorgänge pro Sekunde (IOPS) und Durchsatz (Bandbreite) werden durch die Größe des Datenträgers beeinflusst. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/storage/storage-premium-storage.md).


## F-Serie


| Größe | CPU-Kerne | Arbeitsspeicher | Netzwerkkarten (max.) | Datenträgergröße | Max. Datenträger (jeweils 1023 GB) | Max. IOPS (500 pro Datenträger) | Maximale Netzwerkbandbreite |
|--------------|-----------|--------|------------|-------------------------|--------------------------|--------------------------|-------------|
| Standard\_F1 | 1 | 2 GB | 1 | Temporär (SSD) = 16 GB | 2 | 2 x 500 | mittel |
| Standard\_F2 | 2 | 4 GB | 2 | Temporär (SSD) = 32 GB | 4 | 4 x 500 | hoch |
| Standard\_F4 | 4 | 8 GB | 4 | Temporär (SSD) = 64 GB | 8 | 8 x 500 | hoch |
| Standard\_F8 | 8 | 16 GB | 8 | Temporär (SSD) = 128 GB | 16 | 16 x 500 | hoch |
| Standard\_F16 | 16 | 32 GB | 8 | Temporär (SSD) = 256 GB | 32 | 32 x 500 | äußerst hoch |



## Fs-Serie*

| Größe | CPU-Kerne | Arbeitsspeicher | Netzwerkkarten (max.) | Datenträgergröße | Max. Datenträger (jeweils 1023 GB) | Cachegröße (GB) | Max. Datenträger-IOPS & Bandbreite | Maximale Netzwerkbandbreite |
|---------------|-----------|--------|------------|------------------------|-----------|-----------|----------------------------|------------|
| Standard\_F1s | 1 | 2 | 1 | Lokales SSD-Laufwerk = 4 GB | 2 | 12 | 3\.200 48 MB pro Sekunde | mittel |
| Standard\_F2s | 2 | 4 | 2 | Lokales SSD-Laufwerk = 8 GB | 4 | 24 | 6\.400 96 MB pro Sekunde | hoch |
| Standard\_F4s | 4 | 8 | 4 | Lokales SSD-Laufwerk = 16 GB | 8 | 48 | 12\.800 192 MB pro Sekunde | hoch |
| Standard\_F8s | 8 | 16 | 8 | Lokales SSD-Laufwerk = 32 GB | 16 | 96 | 25\.600 384 MB pro Sekunde | hoch |
| Standard\_F16s | 16 | 32 | 8 | Lokales SSD-Laufwerk = 64 GB | 32 | 192 | 51\.200 768 MB pro Sekunde | äußerst hoch |



*Die bei einer Fs-Serie möglichen Höchstwerte für E/A-Vorgänge pro Sekunde (IOPS) und Durchsatz (Bandbreite) werden durch die Größe des Datenträgers beeinflusst. Nähere Informationen finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/storage/storage-premium-storage.md).





## G-Serie

|Größe |CPU-Kerne|Arbeitsspeicher|Netzwerkkarten (max.)|Maximale Datenträgergröße|Max. Datenträger (jeweils 1023 GB)|Max. IOPS (500 pro Datenträger)| Maximale Netzwerkbandbreite |
|---|---|---|---|---|---|---|---|
|Standard\_G1 |2|28 GB|1|Lokales SSD-Laufwerk = 384 GB |4|4 x 500| hoch |
|Standard\_G2 |4|56 GB|2|Lokales SSD-Laufwerk = 768 GB |8|8 x 500| hoch |
|Standard\_G3 |8|112 GB|4|Lokales SSD-Laufwerk = 1.536 GB |16|16 x 500| sehr hoch | 
|Standard\_G4 |16|224 GB|8|Lokales SSD-Laufwerk = 3.072 GB |32|32 x 500| äußerst hoch |
|Standard\_G5 |32|448 GB|8|Lokales SSD-Laufwerk = 6.144 GB |64| 64 x 500 | äußerst hoch |




## GS-Serie

|Größe |CPU-Kerne|Arbeitsspeicher|Netzwerkkarten (max.)|Maximale Datenträgergröße|Max. Datenträger (jeweils 1023 GB)|Cachegröße (GB)|Max. Datenträger-IOPS &amp; Bandbreite| Maximale Netzwerkbandbreite |
|---|---|---|---|---|---|---|---|---|
|Standard\_GS1|2|28|1|Lokales SSD-Laufwerk = 56 GB |4|264| 5\.000, 125 MB pro Sekunde | hoch |
|Standard\_GS2|4|56|2|Lokales SSD-Laufwerk = 112 GB |8|528| 10\.000, 250 MB pro Sekunde | hoch | 
|Standard\_GS3|8|112|4|Lokales SSD-Laufwerk = 224 GB |16|1056| 20\.000, 500 MB pro Sekunde | sehr hoch |
|Standard\_GS4|16|224|8|Lokales SSD-Laufwerk = 448 GB |32|2112| 40\.000, 1.000 MB pro Sekunde | äußerst hoch |
|Standard\_GS5|32|448|8|Lokales SSD-Laufwerk = 896 GB |64|4224| 80\.000, 2.000 MB pro Sekunde | äußerst hoch |

## N-Serie

Die NC- und NV-Größen werden auch als GPU-fähige Instanzen bezeichnet. Dabei handelt es sich um spezielle virtuelle Computer, die für verschiedene Szenarien und Anwendungsfälle optimierte GPU-Karten von NVIDIA enthalten. Die NV-Größen sind optimiert und konzipiert für Remotevisualisierung, Streaming, Spiele, Codierung und VDI-Szenarien mit Frameworks wie OpenGL und DirectX. Die NC-Größen sind eher für rechen- und netzwerkintensive Anwendungen und Algorithmen optimiert, beispielsweise CUDA- und OpenCL-basierte Anwendungen und Simulationen.


### NV-Instanzen
Die NV-Instanzen werden durch Tesla M60 GPUs von NVIDIA und NVIDIA GRID unterstützt und ermöglichen für Desktops beschleunigte Anwendungen und virtuelle Desktops, sodass die Kunden ihre Daten oder Simulationen visualisieren können. Benutzer können ihre grafikintensiven Workflows auf den NV-Instanzen visualisieren, um überlegene Grafikfunktionen zu erhalten, und darüber hinaus Workloads mit einfacher Genauigkeit ausführen, z.B. Codierung und Rendering. Tesla M60 bietet 4096 CUDA-Kerne als duale GPU mit bis zu 36 Datenströmen mit 1080p H.264.


| Größe | Kerne | GPU | Arbeitsspeicher | Datenträger |
|------|-------|----------------|--------|-------------|
| NV6 | 6 | 1 x NVIDIA M60 | 56 GB | 380 GB SSD |
| NV12 | 12 | 2 x NVIDIA M60 | 112 GB | 680 GB SSD |
| NV24 | 24 | 4 x NVIDIA M60 | 224 GB | 1440 GB SSD | 



### NC-Instanzen

Die NC-Instanzen werden durch Tesla K80 von NVIDIA unterstützt. Benutzer können Daten jetzt wesentlich schneller analysieren, indem sie CUDA für Anwendungen zur Energieuntersuchung, Absturzsimulationen, Rendering mit Raytracing, Deep Learning und mehr verwenden. Tesla K80 bietet 4992 CUDA-Kerne als duale GPU mit einer Leistung von bis zu 2,91 Teraflops mit doppelter Genauigkeit und bis zu 8,93 Teraflops mit einfacher Genauigkeit.


| Größe | Kerne | GPU | Arbeitsspeicher | Datenträger |
|------|-------|----------------|--------|-------------|
| NC6 | 6 | 1 x NVIDIA K80 | 56 GB | 380 GB SSD |
| NC12 | 12 | 2 x NVIDIA K80 | 112 GB | 680 GB SSD |
| NC24 | 24 | 4 x NVIDIA K80 | 224 GB | 1440 GB SSD |

## Hinweise: Standard\_A0 – A4 mit Befehlszeilenschnittstelle (CLI) und PowerShell 


Im klassischen Bereitstellungsmodell unterscheiden sich einige VM-Größennamen in der Befehlszeilenschnittstelle und in PowerShell:

* Standard\_A0 ist „Sehr klein“
* Standard\_A1 ist „Klein“
* Standard\_A2 ist „Mittel“
* Standard\_A3 ist „Groß“
* Standard\_A4 ist „Extragroß“


## Nächste Schritte

- Erfahren Sie mehr über [Einschränkungen für Azure-Abonnements und -Dienste, Kontingente und Einschränkungen](../articles/azure-subscription-service-limits.md).
- Erfahren Sie mehr über [rechenintensive A8-, A9-, A10- und A11-Instanzen](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) für Workloads wie etwa High-Performance Computing (HPC).

<!---HONumber=AcomDC_0803_2016-->