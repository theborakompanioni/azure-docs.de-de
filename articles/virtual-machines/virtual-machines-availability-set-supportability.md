---
title: "Unterstützungsmöglichkeiten für das Hinzufügen virtueller Azure-Computer zu einer vorhandenen Verfügbarkeitsgruppe | Microsoft-Dokumentation"
description: "Unterstützungsmöglichkeiten für das Hinzufügen virtueller Azure-Computer zu einer vorhandenen Verfügbarkeitsgruppe."
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/15/2017
ms.author: delhan
ms.translationtype: HT
ms.sourcegitcommit: 8021f8641ff3f009104082093143ec8eb087279e
ms.openlocfilehash: f07108f5d7a792dcc9aae8a4fe889878db168bc6
ms.contentlocale: de-de
ms.lasthandoff: 07/21/2017

---
# <a name="supportability-of-adding-azure-vms-to-an-existing-availability-set"></a>Unterstützungsmöglichkeiten für das Hinzufügen virtueller Azure-Computer zu einer vorhandenen Verfügbarkeitsgruppe

Beim Hinzufügen von neuen virtuellen Computern zu einer vorhandenen Verfügbarkeitsgruppe stoßen Sie gelegentlich an Grenzen. Im folgenden Diagramm ist aufgeführt, welche VM-Serien in der gleichen Verfügbarkeitsgruppe verwendet werden können.

Hier sehen Sie die Matrix für die Unterstützungsmöglichkeiten beim Kombinieren verschiedener VM-Typen:

Serie und Verfügbarkeitsgruppe|Zweiter virtueller Computer|A|Av2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|Erster virtueller Computer|||||||
|A||OK|OK|OK|OK|OK|
|Av2||OK|OK|OK|OK|OK|
|D||OK|OK|OK|OK|OK|
|Dv2||OK|OK|OK|OK|OK|
|Dv3||OK|OK|OK|OK|OK|

Alle anderen Serien können nicht in der gleichen Verfügbarkeitsgruppe enthalten sein, da sie spezielle Hardware erfordern.
