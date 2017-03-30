---
title: "Benennungsrichtlinien für die Azure-Infrastruktur – Windows | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die wichtigsten Entwurfs- und Implementierungsrichtlinien für Benennungen in Azure-Infrastrukturdiensten."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 660765fa-4d42-49cb-a9c6-8c596d26d221
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: e335ecafdd6347bf8b839f50bf36e3d5e2cd42fa
ms.lasthandoff: 03/18/2017


---
# <a name="azure-infrastructure-naming-guidelines-for-windows-vms"></a>Benennungsrichtlinien für die Azure-Infrastruktur für Windows-VMs

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

In diesem Artikel wird erläutert, wie Sie Benennungskonventionen für all Ihre verschiedenen Azure-Ressourcen entwickeln, sodass Sie einen logischen und leicht erkennbaren Satz an Ressourcen für Ihre gesamte Umgebung erstellen können.

## <a name="implementation-guidelines-for-naming-conventions"></a>Implementierungsrichtlinien für Benennungskonventionen
Entscheidungen:

* Wie lauten Ihre Benennungskonventionen für Azure-Ressourcen?

Aufgaben:

* Legen Sie die Affixe fest, die Sie für die Ressourcen nutzen, um Konsistenz zu gewährleisten.
* Legen Sie die Namen der Speicherkonten fest. Beachten Sie dabei, dass diese global eindeutig sein müssen.
* Dokumentieren Sie die zu verwendende Benennungskonvention, und geben Sie diese an alle beteiligten Parteien weiter, um Konsistenz zwischen den Bereitstellungen zu gewährleisten.

## <a name="naming-conventions"></a>Benennungskonventionen
Es sollte eine geeignete Benennungskonvention vorhanden sein, bevor Sie mit der Entwicklung in Azure beginnen. Durch eine Benennungskonvention wird sichergestellt, dass alle Ressourcen vorhersagbare Namen haben, die den Verwaltungsaufwand für diese Ressourcen senken.

Sie können auch einen bestimmten Satz von Benennungskonventionen für Ihre gesamte Organisation oder für ein bestimmtes Azure-Abonnement oder -Konto anwenden. Es ist zwar einfach, für Einzelpersonen innerhalb von Organisationen implizite Regeln für die Arbeit mit Azure-Ressourcen einzurichten, bei der Arbeit eines Teams an einem Projekt in Azure ist dieses Modell jedoch nicht gut geeignet.

Vereinbaren Sie im Voraus den Satz von Benennungskonventionen. Es gibt einige Überlegungen hinsichtlich Benennungskonventionen, die diesen Regelsätzen widersprechen.

## <a name="affixes"></a>Affixe
Beim Festlegen einer Benennungskonvention müssen Sie sich auch für die Position des Affixes entscheiden:

* Am Anfang des Namens (Präfix)
* Am Ende des Namens (Suffix)

Folgende Namen sind z.B. für eine Ressourcengruppe mit dem Affix `rg` möglich:

* Rg-WebApp (Präfix)
* WebApp-Rg (Suffix)

Affixe können auf verschiedene Aspekte verweisen, die die entsprechenden Ressourcen beschreiben. Die folgende Tabelle zeigt einige üblicherweise verwendete Beispiele.

| Aspekt | Beispiele | Hinweise |
|:--- |:--- |:--- |
| Environment |dev, stg, prod |Abhängig von Zweck und Name der jeweiligen Umgebung. |
| Ort |usw (USA Westen), use (USA Osten 2) |Abhängig von der Region des Datencenters oder der Region der Organisation. |
| Azure-Komponente, -Dienst oder -Produkt |„Rg“ für Ressourcengruppe, „VNet“ für virtuelles Netzwerk |Je nach Produkt, das die Ressource unterstützt. |
| Role |sql, ora, sp, iis |Abhängig von der Rolle des virtuellen Computers. |
| Instanz |01, 02, 03 usw. |Für Ressourcen, die mehr als eine Instanz besitzen. Beispielsweise Webserver mit Lastenausgleich in einem Clouddienst. |

Beim Einrichten der Benennungskonventionen sollten Sie sicherstellen, dass diese eindeutig angeben, welche Affixe für die einzelnen Ressourcentypen verwendet werden sollen und an welcher Position (Präfix und Suffix).

## <a name="dates"></a>Datumsangaben
In vielen Fällen ist es wichtig, das Datum der Erstellung anhand des Namens einer Ressource bestimmen zu können. Wir empfehlen das Datumsformat JJJJMMTT. Durch dieses Format wird sichergestellt, dass nicht nur das vollständige Datum aufgezeichnet wird, sondern auch zwei Ressourcen gleichzeitig alphabetisch und chronologisch sortiert werden, deren Namen sich nur durch das Datum unterscheiden.

## <a name="naming-resources"></a>Benennen von Ressourcen
Definieren Sie die einzelnen Ressourcentypen in der Benennungskonvention, die Regeln für die Zuweisung von Namen für die erstellten Ressourcen enthalten sollte. Diese Regeln sollten auf alle Ressourcentypen angewendet werden, z. B.:

* Abonnements
* Konten
* Speicherkonten
* Virtuelle Netzwerke
* Subnetze
* Verfügbarkeitsgruppen
* Ressourcengruppen
* Virtuelle Computer
* Endpunkte
* Netzwerksicherheitsgruppen
* Rollen

Sie sollten aussagekräftige Namen verwenden, sodass der Name genügend Informationen bietet, um festzustellen, auf welche Ressource er verweist.

## <a name="computer-names"></a>Computernamen
Wenn Sie einen virtuellen Computer (VM) erstellen, ist für Microsoft Azure ein VM-Name aus bis zu 15 Zeichen erforderlich, der für den Ressourcennamen verwendet wird. Azure verwendet den gleichen Namen für das Betriebssystem, das auf dem virtuellen Computer installiert ist. Allerdings können diese Namen nicht immer identisch sein.

Falls ein virtueller Computer aus einer VHD-Imagedatei erstellt wird, die bereits ein Betriebssystem enthält, kann sich der Name des virtuellen Computers in Azure vom Computernamen des Betriebssystems des virtuellen Computers unterscheiden. Da dies zusätzliche Probleme bei der Verwaltung virtueller Computer verursachen kann, wird davon abgeraten. Geben Sie der Ressource des virtuellen Azure-Computers den gleichen Computernamen, den Sie dem Betriebssystem dieses virtuellen Computers zuweisen.

Es wird empfohlen, für den virtuellen Azure-Computer und das zugrunde liegende Betriebssystem den gleichen Computernamen zu verwenden.

## <a name="storage-account-names"></a>Speicherkontonamen
Dieser Abschnitt gilt nicht für [Azure Managed Disks](../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), da Sie kein separates Speicherkonto erstellen. Bei nicht verwalteten Datenträgern gelten für die Benennung von Speicherkonten spezielle Regeln. Sie können nur Kleinbuchstaben und Zahlen verwenden. Weitere Informationen finden Sie unter [Erstellen eines Speicherkontos](../storage/storage-create-storage-account.md#create-a-storage-account). Darüber hinaus sollte der Namen des Speicherkontos in Kombination mit „core.windows.net“ ein global gültiger und eindeutiger DNS-Name sein. Wenn beispielsweise das Speicherkonto den Namen "meinspeicherkonto" besitzt, sollten die folgenden resultierenden DNS-Namen eindeutig sein:

* meinspeicherkonto.blob.core.windows.net
* meinspeicherkonto.table.core.windows.net
* meinspeicherkonto.queue.core.windows.net

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]


