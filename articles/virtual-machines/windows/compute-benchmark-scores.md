---
title: "Compute-Benchmarkergebnisse für virtuelle Windows-Computer | Microsoft Docs"
description: "Vergleichen Sie SPECint-Compute-Benchmarkergebnisse für Azure-VMs unter Windows"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 69ae72ec-e8be-4e46-a8f0-e744aebb5cc2
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: a3e4ce890d02290ba954119de8443d28378c0f98
ms.lasthandoff: 04/03/2017


---
# <a name="compute-benchmark-scores-for-windows-vms"></a>Compute-Benchmarkergebnisse für virtuelle Windows-Computer
Die folgenden SPECInt-Benchmarkergebnisse zeigen die Computeleistung für äußerst leistungsfähig Azure-VMs unter Windows Server. Compute-Benchmarkergebnisse stehen auch für [virtuelle Linux-Computer](../linux/compute-benchmark-scores.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)zur Verfügung.

## <a name="a-series---compute-intensive"></a>A-Serie – rechenintensiv
| Größe | vCPUs | NUMA-Knoten | CPU | Ausführungen | Durchschnittliche Basisrate | Standardabweichung |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8 |8 |1 |Intel Xeon CPU E5-2670 0 mit 2,6 GHz |10 |236,1 |1.1 |
| Standard_A9 |16 |2 |Intel Xeon CPU E5-2670 0 mit 2,6 GHz |10 |450,3 |7.0 |
| Standard_A10 |8 |1 |Intel Xeon CPU E5-2670 0 mit 2,6 GHz |5 |235,6 |0.9 |
| Standard_A11 |16 |2 |Intel Xeon CPU E5-2670 0 mit 2,6 GHz |7 |454,7 |4.8 |

## <a name="dv2-series"></a>Dv2-Serie
| Größe | vCPUs | NUMA-Knoten | CPU | Ausführungen | Durchschnittliche Basisrate | Standardabweichung |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D1_v2 |1 |1 |Intel Xeon E5-2673 v3 mit 2,4 GHz |83 |36,6 |2.6 |
| Standard_D2_v2 |2 |1 |Intel Xeon E5-2673 v3 mit 2,4 GHz |27 |70,0 |3,7 |
| Standard_D3_v2 |4 |1 |Intel Xeon E5-2673 v3 mit 2,4 GHz |19 |130,5 |4.4 |
| Standard_D4_v2 |8 |1 |Intel Xeon E5-2673 v3 mit 2,4 GHz |19 |238,1 |5,2 |
| Standard_D5_v2 |16 |2 |Intel Xeon E5-2673 v3 mit 2,4 GHz |14 |460,9 |15,4 |
| Standard_D11_v2 |2 |1 |Intel Xeon E5-2673 v3 mit 2,4 GHz |19 |70,1 |3,7 |
| Standard_D12_v2 |4 |1 |Intel Xeon E5-2673 v3 mit 2,4 GHz |2 |132,0 |1.4 |
| Standard_D13_v2 |8 |1 |Intel Xeon E5-2673 v3 mit 2,4 GHz |17 |235,8 |3.8 |
| Standard_D14_v2 |16 |2 |Intel Xeon E5-2673 v3 mit 2,4 GHz |15 |460,8 |6,5 |

## <a name="g-series-gs-series"></a>G-Serie, GS-Serie
| Größe | vCPUs | NUMA-Knoten | CPU | Ausführungen | Durchschnittliche Basisrate | Standardabweichung |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_G1, Standard_GS1 |2 |1 |Intel Xeon E5-2698B v3 mit 2 GHz |31 |71,8 |6,5 |
| Standard_G2, Standard_GS2 |4 |1 |Intel Xeon E5-2698B v3 mit 2 GHz |5 |133,4 |13,0 |
| Standard_G3, Standard_GS3 |8 |1 |Intel Xeon E5-2698B v3 mit 2 GHz |6 |242,3 |6,0 |
| Standard_G4, Standard_GS4 |16 |1 |Intel Xeon E5-2698B v3 mit 2 GHz |15 |398,9 |6,0 |
| Standard_G5, Standard_GS5 |32 |2 |Intel Xeon E5-2698B v3 mit 2 GHz |22 |762,8 |3,7 |

## <a name="h-series"></a>H-Reihe
| Größe | vCPUs | NUMA-Knoten | CPU | Ausführungen | Durchschnittliche Basisrate  | Standardabweichung |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |1 |Intel Xeon E5-2667 v3 mit 3,2 GHz |5 |297,4 |0.9 |
| Standard_H16 |16 |2 |Intel Xeon E5-2667 v3 mit 3,2 GHz |5 |575,8 |6,8 |
| Standard_H8m |8 |1 |Intel Xeon E5-2667 v3 mit 3,2 GHz |5 |297,0 |1.2 |
| Standard_H16m |16 |2 |Intel Xeon E5-2667 v3 mit 3,2 GHz |5 |572,2 |3.9 |
| Standard_H16r |16 |2 |Intel Xeon E5-2667 v3 mit 3,2 GHz |5 |573,2 |2,9 |
| Standard_H16mr |16 |2 |Intel Xeon E5-2667 v3 mit 3,2 GHz |7 |569,6 |2.8 |

## <a name="about-specint"></a>Informationen zu SPECint
Die Windows-Zahlen wurden durch die Ausführung von [SPECint 2006](https://www.spec.org/cpu2006/results/rint2006.html) unter Windows Server berechnet. SPECint wurde mit der Option für die Basisrate (SPECint_rate2006) mit einer Kopie pro Kern ausgeführt. SPECint besteht aus 12 getrennten Tests, die jeweils dreimal ausgeführt wurden. Der Mittelwert jedes Tests wurde gewichtet, um eine zusammengesetzte Bewertung zu erhalten. Diese Tests wurden dann auf mehreren virtuellen Computern ausgeführt, um die gezeigten Durchschnittsergebnisse zu liefern.

## <a name="next-steps"></a>Nächste Schritte
* Speicherkapazitäten, Details zu Datenträgern und weitere Überlegungen zur Auswahl von Größen für virtuelle Computer finden Sie unter [Größen für virtuelle Computer](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


