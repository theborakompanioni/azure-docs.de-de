---
title: Grundlegendes zu ausgehenden Verbindungen in Azure | Microsoft Docs
description: "In diesem Artikel wird erläutert, wie Azure virtuellen Computern die Kommunikation mit öffentlichen Internetdiensten ermöglicht."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
ms.assetid: 5f666f2a-3a63-405a-abcd-b2e34d40e001
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: bec4f89556a2daa41e19b0ecb2ab9bbbed849107
ms.openlocfilehash: 0bf40c5b44ea87c88d4464baf958e8afb7a59c38

---

# <a name="understanding-outbound-connections-in-azure"></a>Grundlegendes zu ausgehenden Verbindungen in Azure

Ein virtueller Computer (VM) in Azure kann mit Endpunkten außerhalb von Azure im öffentlichen IP-Adressraum kommunizieren. Wenn eine VM einen ausgehenden Datenfluss in einen öffentlichen IP-Adressraum einleitet, ordnet Azure die private IP-Adresse der VM einer öffentlichen IP-Adresse zu und lässt zu, das zurückfließender Datenverkehr die VM erreicht.

Azure bietet drei Methoden, um ausgehende Verbindungen zu ermöglichen. Jede Methode weist eigene Merkmale und Einschränkungen auf. Überprüfen Sie jede Methode sorgfältig, um diejenige auszuwählen, die Ihren Anforderungen entspricht.

| Szenario | Methode | Hinweis |
| --- | --- | --- |
| Eigenständige VM (kein Lastenausgleich, keine öffentliche IP-Adresse auf Instanzebene) |Standardmäßiges SNAT |Azure ordnet eine öffentliche IP-Adresse für SNAT zu. |
| VM mit Lastenausgleich (keine öffentliche IP-Adresse auf Instanzebene auf VM) |SNAT mit Lastenausgleich |Azure verwendet eine öffentliche IP-Adresse des Lastenausgleichs für SNAT. |
| VM mit öffentlicher IP-Adresse auf Instanzebene (mit oder ohne Lastenausgleich) |SNAT nicht verwendet |Azure verwendet die öffentliche IP-Adresse, die der VM zugewiesen ist. |

Wenn eine VM mit Endpunkten außerhalb von Azure im öffentlichen IP-Adressraum kommunizieren soll, können Sie zum Blockieren des Zugriffs Netzwerksicherheitsgruppen verwenden. Netzwerksicherheitsgruppen werden unter [Verhindern öffentlicher Verbindungen](#preventing-public-connectivity) detaillierter behandelt.

## <a name="standalone-vm-with-no-instance-level-public-ip-address"></a>Eigenständige VM ohne öffentliche IP-Adresse auf Instanzebene

In diesem Szenario gehört die VM nicht zu einem Azure Load Balancer-Pool, und ihr ist keine öffentliche IP-Adresse auf Instanzebene zugewiesen. Wenn der virtuelle Computer einen ausgehenden Datenfluss einleitet, übersetzt Azure die private IP-Quelladresse für den ausgehenden Datenfluss in eine öffentliche IP-Quelladresse. Die für diesen ausgehenden Datenfluss verwendete öffentliche IP-Adresse ist nicht konfigurierbar. Azure verwendet das Verfahren „Source Network Address Translation (SNAT, Übersetzung der Quellnetzwerkadresse)“ für diese Aufgabe. Kurzlebige Ports der öffentlichen IP-Adresse werden verwendet, um einzelne Datenflüsse zu unterscheiden, die von der VM stammen. Beim Erstellen von Datenflüssen weist SNAT kurzlebige Ports dynamisch zu. In diesem Kontext werden die kurzlebigen für SNAT verwendeten Ports als SNAT-Ports bezeichnet.

SNAT Ports sind begrenzte Ressourcen, die sich erschöpfen können. Es ist wichtig, ihre Nutzung zu verstehen. Pro Datenfluss zu einer einzelnen IP-Zieladresse wird ein SNAT-Port genutzt. Für mehrere Datenflüsse zur gleichen IP-Zieladresse belegt jeder Datenfluss einen einzelnen SNAT-Port. Dadurch wird sichergestellt, dass die Datenflüsse eindeutig sind, wenn sie von der gleichen öffentlichen IP-Adresse stammen und die gleiche IP-Zieladresse haben. Mehrere Datenflüsse mit jeweils einer anderen IP-Zieladresse nutzen pro Ziel einen einzelnen SNAT-Port. Die IP-Zieladresse sorgt für Eindeutigkeit der Datenflüsse.

Sie können [Log Analytics für Azure Load Balancer](load-balancer-monitor-log.md) und [Warnungsereignisprotokolle zum Überwachen auf Meldungen zur SNAT-Portauslastung](load-balancer-monitor-log.md#alert-event-log) verwenden. Sobald SNAT-Portressourcen erschöpft sind, sind ausgehende Datenflüsse erst wieder möglich, nachdem SNAT-Ports von vorhandenen Datenflüssen freigegeben wurden. Der Lastenausgleich verwendet ein vierminütiges Leerlauftimeout für die Freigabe von SNAT-Ports.

## <a name="load-balanced-vm-with-no-instance-level-public-ip-address"></a>VM mit Lastenausgleich ohne öffentliche IP-Adresse auf Instanzebene

In diesem Szenario gehört die VM zum Azure Load Balancer-Pool. Der VM ist keine öffentliche IP-Adresse zugewiesen. Wenn die dem Lastenausgleich unterliegende VM einen ausgehenden Datenfluss erstellt, übersetzt Azure die private IP-Quelladresse des ausgehenden Datenflusses in die öffentliche IP-Adresse des öffentlichen Lastenausgleichs-Frond-End. Azure verwendet das Verfahren „Source Network Address Translation (SNAT, Übersetzung der Quellnetzwerkadresse)“ für diese Aufgabe. Kurzlebiger Ports der öffentlichen IP-Adresse des Lastenausgleichs werden verwendet, um einzelne Datenflüsse zu unterscheiden, die von der VM stammen. Beim Erstellen ausgehender Datenflüsse weist SNAT kurzlebige Ports dynamisch zu. In diesem Kontext werden die kurzlebigen für SNAT verwendeten Ports als SNAT-Ports bezeichnet.

SNAT Ports sind begrenzte Ressourcen, die sich erschöpfen können. Es ist wichtig, ihre Nutzung zu verstehen. Pro Datenfluss zu einer einzelnen IP-Zieladresse wird ein SNAT-Port genutzt. Für mehrere Datenflüsse zur gleichen IP-Zieladresse belegt jeder Datenfluss einen einzelnen SNAT-Port. Dadurch wird sichergestellt, dass die Datenflüsse eindeutig sind, wenn sie von der gleichen öffentlichen IP-Adresse stammen und die gleiche IP-Zieladresse haben. Mehrere Datenflüsse mit jeweils einer anderen IP-Zieladresse nutzen pro Ziel einen einzelnen SNAT-Port. Die IP-Zieladresse sorgt für Eindeutigkeit der Datenflüsse.

Sie können [Log Analytics für Azure Load Balancer](load-balancer-monitor-log.md) und [Warnungsereignisprotokolle zum Überwachen auf Meldungen zur SNAT-Portauslastung](load-balancer-monitor-log.md#alert-event-log) verwenden. Sobald SNAT-Portressourcen erschöpft sind, sind ausgehende Datenflüsse erst wieder möglich, nachdem SNAT-Ports von vorhandenen Datenflüssen freigegeben wurden. Der Lastenausgleich verwendet ein vierminütiges Leerlauftimeout für die Freigabe von SNAT-Ports.

## <a name="vm-with-an-instance-level-public-ip-address-with-or-without-load-balancer"></a>VM mit öffentlicher IP-Adresse auf Instanzebene (mit oder ohne Lastenausgleich)

In diesem Szenario ist der VM eine öffentliche IP-Adresse auf Instanzebene (Instance Level Public IP, ILPIP) zugewiesen. Es ist unerheblich, ob der virtuelle Computer einen Lastenausgleich hat oder nicht. Wenn eine öffentliche IP-Adresse auf Instanzebene verwendet wird, kommt SNAT nicht zum Einsatz. Die VM nutzt die öffentliche IP-Adresse auf Instanzebene für alle ausgehenden Datenflüsse. Wenn Ihre Anwendung viele ausgehende Datenflüsse auslöst und SNAT überlastet sein sollte, sollten Sie das Zuweisen einer öffentlichen IP-Adresse auf Instanzebene erwägen, um SNAT-Engpässe zu vermeiden.

## <a name="discovering-the-public-ip-used-by-a-given-vm"></a>Ermitteln der von einer bestimmten VM verwendeten öffentlichen IP-Adresse

Es gibt viele Möglichkeiten, die öffentliche IP-Quelladresse einer ausgehenden Verbindung zu bestimmen. OpenDNS bietet einen Dienst, der die öffentliche IP-Adresse Ihrer VM anzeigen kann. Mithilfe des Befehls „nslookup“ können Sie eine DNS-Abfrage für den Namen „myip.opendns.com“ an den OpenDNS-Resolver senden. Der Dienst gibt die IP-Quelladresse zurück, die zum Senden der Abfrage verwendet wurde. Wenn Sie die folgende Abfrage auf Ihrer VM ausführen, ist die Antwort die öffentliche IP-Adresse, die für diese VM verwendet wird.

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventing-public-connectivity"></a>Verhindern öffentlicher Verbindungen

Mitunter ist es nicht wünschenswert, dass eine VM einen ausgehenden Datenfluss erstellen darf. Oder ggf. gibt es eine Anforderung zum Festlegen, welche Ziele ausgehende Datenflüsse erreichen dürfen. In diesem Fall verwenden Sie [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md) zum Bestimmen der Ziele, die die VM erreichen kann. Wenn Sie eine Netzwerksicherheitsgruppe einer VM mit Lastenausgleich zuordnen, müssen Sie die [Standardtags](../virtual-network/virtual-networks-nsg.md#default-tags) und [Standardregeln](../virtual-network/virtual-networks-nsg.md#default-rules) beachten.

Sie müssen sicherstellen, dass die VM Anforderungen von Integritätstests von Azure Load Balancer empfangen kann. Wenn eine Netzwerksicherheitsgruppe Anforderungen von Integritätstests vom Standardtag AZURE_LOADBALANCER blockiert, misslingt Ihr VM-Integritätstests, weshalb die VM mit „Außer Betrieb“ markiert wird. Der Lastenausgleich beendet das Senden neuer Datenflüsse an diese VM.



<!--HONumber=Nov16_HO3-->


