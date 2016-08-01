<properties
 pageTitle="Compute-Benchmarkergebnisse für virtuelle Windows-Computer | Microsoft Azure"
 description="Vergleichen Sie SPECint-Compute-Benchmarkergebnisse für Azure-VMs unter Windows"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="07/18/2016"
 ms.author="danlep"/>

# Compute-Benchmarkergebnisse für virtuelle Windows-Computer

Die folgenden SPECInt-Benchmarkergebnisse zeigen die Computeleistung für äußerst leistungsfähig Azure-VMs unter Windows Server. Compute-Benchmarkergebnisse stehen auch für [virtuelle Linux-Computer](virtual-machines-linux-compute-benchmark-scores.md) zur Verfügung.



## A-Serie – rechenintensiv


Größe | vCPUs | NUMA-Knoten | CPU | Ausführungen | Durchschnittliche Basisrate | Standardabweichung
------- | ------ | ---- | -------| ---- | ---- | -----
Standard\_A8 | 8 | 1 | Intel Xeon CPU E5-2670 0 mit 2,6 GHz | 10 | 236,1 | 1\.1
Standard\_A9 | 16 | 2 | Intel Xeon CPU E5-2670 0 mit 2,6 GHz | 10 | 450,3 | 7,0
Standard\_A10 | 8 | 1 | Intel Xeon CPU E5-2670 0 mit 2,6 GHz | 5 | 235,6 | 0\.9
Standard\_A11 | 16 | 2 | Intel Xeon CPU E5-2670 0 mit 2,6 GHz |7 | 454,7 | 4\.8

## Dv2-Serie


Größe | vCPUs | NUMA-Knoten | CPU | Ausführungen | Durchschnittliche Basisrate | Standardabweichung
------- | ------ | ---- | -------| ---- | ---- | -----
Standard\_D1\_v2 | 1 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 83 | 36,6 | 2\.6
Standard\_D2\_v2 | 2 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 27 | 70,0 | 3,7
Standard\_D3\_v2 | 4 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 19 | 130,5 | 4\.4
Standard\_D4\_v2 | 8 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 19 | 238,1 | 5,2
Standard\_D5\_v2 | 16 | 2 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 14 | 460,9 | 15,4
Standard\_D11\_v2 | 2 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 19 | 70,1 | 3,7
Standard\_D12\_v2 | 4 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 2 | 132,0 | 1\.4
Standard\_D13\_v2 | 8 | 1 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 17 | 235,8 | 3\.8
Standard\_D14\_v2 | 16 | 2 | Intel Xeon E5-2673 v3 mit 2,4 GHz | 15 | 460,8 | 6,5


## G-Serie, GS-Serie


Größe | vCPUs | NUMA-Knoten | CPU | Ausführungen | Durchschnittliche Basisrate | Standardabweichung
------- | ------ | ---- | -------| ---- | ---- | -----
Standard\_G1, Standard\_GS1 | 2 | 1 | Intel Xeon E5-2698B v3 mit 2 GHz | 31 | 71,8 | 6,5
Standard\_G2, Standard\_GS2 | 4 | 1 | Intel Xeon E5-2698B v3 mit 2 GHz | 5 | 133,4 | 13,0
Standard\_G3, Standard\_GS3 | 8 | 1 | Intel Xeon E5-2698B v3 mit 2 GHz | 6 | 242,3 | 6,0
Standard\_G4, Standard\_GS4 | 16 | 1 | Intel Xeon E5-2698B v3 mit 2 GHz | 15 | 398,9 | 6,0
Standard\_G5, Standard\_GS5 | 32 | 2 | Intel Xeon E5-2698B v3 mit 2 GHz | 22 | 762,8 | 3,7

## Informationen zu SPECint

Die Windows-Zahlen wurden durch die Ausführung von [SPECint 2006](https://www.spec.org/cpu2006/results/rint2006.html) unter Windows Server berechnet. SPECint wurde mit der Option für die Basisrate (SPECint\_rate2006) mit einer Kopie pro Kern ausgeführt. SPECint besteht aus 12 getrennten Tests, die jeweils dreimal ausgeführt wurden. Der Mittelwert jedes Tests wurde gewichtet, um eine zusammengesetzte Bewertung zu erhalten. Diese wurden dann auf mehreren virtuellen Computern ausgeführt, um die gezeigten Durchschnittsergebnisse zu liefern.


## Nächste Schritte

* Speicherkapazitäten, Details zu Datenträgern und weitere Überlegungen zur Auswahl von Größen für virtuelle Computer finden Sie unter [Größen für virtuelle Computer](virtual-machines-windows-sizes.md).

<!---HONumber=AcomDC_0720_2016-->