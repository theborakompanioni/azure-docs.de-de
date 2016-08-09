<properties
 pageTitle="Compute-Benchmarkergebnisse für virtuelle Linux-Computer | Microsoft Azure"
 description="Vergleichen Sie Compute-Benchmarkergebnisse von CoreMark für Azure-VMs unter Linux"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="07/18/2016"
 ms.author="danlep"/>

# Compute-Benchmarkergebnisse für virtuelle Linux-Computer

Die folgenden CoreMark-Benchmarkergebnisse zeigen die Computeleistung für äußerst leistungsfähig Azure-VMs unter Ubuntu. Compute-Benchmarkergebnisse stehen auch für [virtuelle Windows-Computer](virtual-machines-windows-compute-benchmark-scores.md) zur Verfügung.




## A-Serie – rechenintensiv


Größe | vCPUs | NUMA-Knoten | CPU | Ausführungen | Iterationen/s | Standardabweichung
------- | ------ | ---- | -------| ---- | ---- | -----
Standard\_A8 | 8 | 1 | Intel Xeon CPU E5-2670 0 mit 2,6 GHz| 179 | 110\.294 | 554
Standard\_A9 | 16 | 2 | Intel Xeon CPU E5-2670 0 mit 2,6 GHz| 189 | 210\.816| 2\.126
Standard\_A10 | 8 | 1 | Intel Xeon CPU E5-2670 0 mit 2,6 GHz| 188 | 110\.025 | 1\.045
Standard\_A11 | 16 | 2 | Intel Xeon CPU E5-2670 0 mit 2,6 GHz| 188 | 210\.727| 2\.073

## Dv2-Serie


Größe | vCPUs | NUMA-Knoten | CPU | Ausführungen | Iterationen/s | Standardabweichung
------- | ------ | ---- | -------| ---- | ---- | -----
Standard\_D1\_v2 | 1 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 140 | 14\.852 | 780
Standard\_D2\_v2 | 2 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 133 | 29\.467 | 1\.863
Standard\_D3\_v2 | 4 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 139 | 56\.205 | 1\.167
Standard\_D4\_v2 | 8 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 126 | 108\.543 | 3\.446
Standard\_D5\_v2 | 16 | 2 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 126 | 205\.332 | 9\.998
Standard\_D11\_v2 | 2 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 125 | 28\.598 | 1\.510
Standard\_D12\_v2 | 4 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 131 | 55\.673 | 1\.418
Standard\_D13\_v2 | 8 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 140 | 107\.986 | 3\.089
Standard\_D14\_v2 | 16 | 2 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 140 | 208\.186 | 8\.839
Standard\_D15\_v2 | 20 | 2 | Intel Xeon E5-2673 v3 mit 2,4 GHz |28 | 268\.560 | 4\.667

## F-Serie

Größe | vCPUs | NUMA-Knoten | CPU | Ausführungen | Iterationen/s | Standardabweichung
------- | ------ | ---- | -------| ---- | ---- | -----
Standard\_F1 | 1 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 154 | 15\.602 | 787
Standard\_F2 | 2 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 126 | 29\.519 | 1\.233
Standard\_F4 | 4 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 147 | 58\.709 | 1\.227
Standard\_F8 | 8 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 224 | 112\.772 | 3\.006
Standard\_F16 | 16 | 2 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 42 | 218\.571 | 5\.113



## G-Serie


Größe | vCPUs | NUMA-Knoten | CPU | Ausführungen | Iterationen/s | Standardabweichung
------- | ------ | ---- | -------| ---- | ---- | -----
Standard\_G1 | 2 | 1 | Intel Xeon E5-2698B v3 mit 2 GHz | 83 | 31\.310 | 2\.891
Standard\_G2 | 4 | 1 | Intel Xeon E5-2698B v3 mit 2 GHz | 84 | 60\.112 | 3\.537
Standard\_G3 | 8 | 1 | Intel Xeon E5-2698B v3 mit 2 GHz | 84 | 107\.522 | 4\.537
Standard\_G4 | 16 | 1 | Intel Xeon E5-2698B v3 mit 2 GHz | 83 | 195\.116 | 5\.024
Standard\_G5 | 32 | 2 | Intel Xeon E5-2698B v3 mit 2 GHz | 84 | 360\.329 | 14\.212

## GS-Serie


Größe | vCPUs | NUMA-Knoten | CPU | Ausführungen | Iterationen/s | Standardabweichung
------- | ------ | ---- | -------| ---- | ---- | -----
Standard\_GS1 | 2 | 1 | Intel Xeon E5-2698B v3 mit 2 GHz | 84 | 28\.613 | 1\.884
Standard\_GS2 | 4 | 1 | Intel Xeon E5-2698B v3 mit 2 GHz | 83 | 54\.348 | 3\.474
Standard\_GS3 | 8 | 1 | Intel Xeon E5-2698B v3 mit 2 GHz | 83 | 104\.564 | 1\.834
Standard\_GS4 | 16 | 1 | Intel Xeon E5-2698B v3 mit 2 GHz | 84 | 194\.111 | 4\.735
Standard\_GS5 | 32 | 2 | Intel Xeon E5-2698B v3 mit 2 GHz | 84 | 357\.396 | 16\.228


## Informationen zu CoreMark

Die Linux-Zahlen wurden durch die Ausführung von [CoreMark](http://www.eembc.org/coremark/faq.php) unter Ubuntu berechnet. CoreMark wurde so konfiguriert, dass die Anzahl der Threads auf die Anzahl der virtuellen CPUs und die Parallelität auf PThreads festgelegt waren. Die Zielanzahl von Iterationen wurde basierend auf der erwarteten Leistung angepasst, um eine Laufzeit von mindestens 20 Sekunden (in der Regel viel länger) zu ermöglichen. Das Endergebnis stellt die Anzahl der abgeschlossenen Iterationen geteilt durch die Anzahl der Sekunden für die Testausführung dar. Jeder Test wurde mindestens sieben Mal auf jedem virtuellen Computer ausgeführt. Die Tests wurden im Oktober 2015 auf mehreren virtuellen Computern in jeder öffentlichen Azure-Region ausgeführt, in der der virtuelle Computer zum Zeitpunkt der Ausführung unterstützt wurde.
## Nächste Schritte



* Speicherkapazitäten, Details zu Datenträgern und weitere Überlegungen zur Auswahl von Größen für virtuelle Computer finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).

* Laden Sie zum Ausführen des CoreMark-Skripts auf virtuellen Linux-Computern das [CoreMark-Skriptpaket](http://download.microsoft.com/download/3/0/5/305A3707-4D3A-4599-9670-AAEB423B4663/AzureCoreMarkScriptPack.zip) herunter.

<!---HONumber=AcomDC_0727_2016-->