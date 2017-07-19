---
title: Was ist eine Netzwerk-Zugriffssteuerungsliste von Azure (Access Control List, ACL)?
description: "Erfahren Sie mehr über Zugriffssteuerungslisten in Azure"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 83d66c84-8f6b-4388-8767-cd2de3e72d76
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.translationtype: Human Translation
ms.sourcegitcommit: b1d56fcfb472e5eae9d2f01a820f72f8eab9ef08
ms.openlocfilehash: 9a0c85367968c9b38104012d75b1f3975be82cc1
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="what-is-an-endpoint-access-control-list"></a>Was ist eine Endpunkt-Zugriffssteuerungsliste?

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene [Bereitstellungsmodelle](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) für das Erstellen und Verwenden von Ressourcen: Resource Manager- und klassische Bereitstellung. Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Resource Manager-Bereitstellungsmodells. 

Eine Endpunkt-Zugriffssteuerungsliste (ACL) trägt zur Verbesserung der Sicherheit Ihrer Azure-Bereitstellung bei. Mit einer ACL können Sie selektiv Datenverkehr für einen Endpunkt eines virtuellen Computers zulassen oder verweigern. Diese Paketfilterungsfunktion bietet eine zusätzliche Schutzebene. Netzwerk-ACLs können nur für Endpunkte angegeben werden. Sie können keine ACL für ein virtuelles Netzwerk oder für ein bestimmtes Subnetz innerhalb eines virtuellen Netzwerks angeben. Es wird empfohlen, anstelle von ACLs möglichst Netzwerksicherheitsgruppen (NSGs) zu verwenden. Weitere Informationen zu NSGs finden Sie im [Überblick zu Netzwerksicherheitsgruppen](virtual-networks-nsg.md).

ACLs können mithilfe von PowerShell sowie über das Azure-Portal konfiguriert werden. Informationen zum Konfigurieren von Netzwerk-ACLs mittels PowerShell finden Sie unter [Verwalten von Endpunkt-Zugriffssteuerungslisten mit PowerShell im klassischen Bereitstellungsmodell](virtual-networks-acl-powershell.md). Informationen zum Konfigurieren von Netzwerk-ACLs mithilfe des Verwaltungsportals finden Sie unter [Einrichten von Endpunkten auf einem klassischen virtuellen Computer](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Netzwerk-ACLs bieten folgende Möglichkeiten:

* Sie können eingehenden Datenverkehr für einen Eingabeendpunkt eines virtuellen Computers selektiv auf der Grundlage des IPv4-Adressbereichs für das Remote-Subnetz zulassen oder verweigern.
* Sie können IP-Adressen in eine Sperrliste aufnehmen.
* Sie können pro Endpunkt eines virtuellen Computers mehrere Regeln erstellen.
* Sie können Regel aufsteigend sortieren, um sicherzustellen, dass für einen bestimmten Endpunkt eines virtuellen Computers die richtigen Regeln angewendet werden.
* Sie können eine ACL für eine bestimmte IPv4-Adresse des Remote-Subnetzes angeben.

Informationen zu ACL-Einschränkungen finden Sie im Artikel zu [Azure-Einschränkungen](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits).

## <a name="how-acls-work"></a>Funktionsweise von ACLs
Eine ACL ist ein Objekt mit einer Regelliste. Wenn Sie eine ACL erstellen und auf einen Endpunkt eines virtuellen Computers anwenden, erfolgt die Paketfilterung auf dem Hostknoten des virtuellen Computers. Das bedeutet, dass der Hostknoten (nicht Ihr virtueller Computer) den Datenverkehr von Remote-IP-Adressen filtert und nach passenden ACL-Regeln sucht. Dadurch wird verhindert, dass Ihr virtueller Computer wertvolle CPU-Zyklen für die Paketfilterung verwendet.

Bei der Erstellung eines virtuellen Computers wird eine Standard-ACL eingerichtet, die sämtlichen eingehenden Datenverkehr blockiert. Wird jedoch ein Endpunkt für Port 3389 erstellt, wird die Standard-ACL so modifiziert, dass sämtlicher eingehender Datenverkehr für diesen Endpunkt zugelassen wird. Daraufhin ist der Endpunkt für eingehenden Datenverkehr aus einem beliebigen Remote-Subnetz erreichbar, und es muss keine Firewall bereitgestellt werden. Eingehender Datenverkehr für andere Ports wird blockiert, es sei denn, es wurden Endpunkte für diese Ports erstellt. Ausgehender Datenverkehr wird standardmäßig zugelassen.

**Beispieltabelle für eine Standard-ACL**

| **Regelnr.** | **Remote-Subnetz** | **Endpunkt** | **Zulassen/Verweigern** |
| --- | --- | --- | --- |
| 100 |0.0.0.0/0 |3389 |Zulassen |

## <a name="permit-and-deny"></a>Zulassen und verweigern
Sie können Netzwerkdatenverkehr für einen Eingabeendpunkt eines virtuellen Computers durch Erstellung entsprechender Regeln selektiv zulassen oder verweigern. Beachten Sie, dass beim Erstellen eines Endpunkts standardmäßig sämtlicher an den Endpunkt gerichteter Datenverkehr erlaubt wird. Daher müssen Sie mit der Erstellung von Zulassungs-/Verweigerungsregeln vertraut sein und sie in die richtige Reihenfolge bringen, um präzise steuern zu können, welcher Netzwerkdatenverkehr den Endpunkt eines virtuellen Computers erreichen darf.

Zu berücksichtigende Punkte:

1. **Keine ACL** : Bei der Endpunkterstellung wird standardmäßig sämtlicher Datenverkehr für den Endpunkt zugelassen.
2. **Zulassen** : Durch Hinzufügen zuzulassende Bereiche werden standardmäßig alle anderen Bereiche verweigert. Nur Pakete aus dem zugelassenen IP-Adressbereich können mit dem Endpunkt des virtuellen Computers kommunizieren.
3. **Ablehnen** : Durch Hinzufügen abzulehnender Bereiche werden standardmäßig alle anderen Datenverkehrsbereiche zugelassen.
4. **Kombination aus Zulassen und Verweigern** : Sie können eine Kombination aus „Zulassen“ und „Verweigern“ verwenden, wenn Sie einen bestimmten IP-Adressbereich angeben möchten, der zugelassen oder verweigert werden soll.

## <a name="rules-and-rule-precedence"></a>Regeln und Reihenfolge
Netzwerk-ACLs können für bestimmte Endpunkte virtueller Computer eingerichtet werden. So können Sie beispielsweise eine Netzwerk-ACL für einen RDP-Endpunkt angeben, der auf einem virtuellen Computer erstellt wurde, und so den Zugriff für bestimmte IP-Adressen sperren. Die folgende Tabelle zeigt, wie Sie öffentlichen virtuellen IP-Adressen (VIPs) eines bestimmten Bereichs Zugriff gewähren, um den RDP-Zugriff zu ermöglichen. Alle anderen Remote-IPs werden verweigert. Bei der Regelreihenfolge *hat jeweils der niedrigste Wert Vorrang* .

### <a name="multiple-rules"></a>Mehrere Regeln
Im folgenden Beispiel gilt: Wenn Sie den Zugriff auf den RDP-Endpunkt nur für zwei öffentliche IPv4-Adressbereiche (65.0.0.0/8 und 159.0.0.0/8) zulassen möchten, können Sie dazu zwei Regeln vom Typ *Zulassen* angeben. Da RDP für einen virtuellen Computer standardmäßig erstellt wird, empfiehlt es sich in diesem Fall unter Umständen, den Zugriff auf den RDP-Port auf der Grundlage eines Remote-Subnetzes zu sperren. Das folgende Beispiel zeigt, wie Sie öffentlichen virtuellen IP-Adressen (VIPs) eines bestimmten Bereichs Zugriff gewähren, um den RDP-Zugriff zu ermöglichen. Alle anderen Remote-IPs werden verweigert. Das funktioniert, weil Netzwerk-ACLs für einen bestimmten Endpunkt eines virtuellen Computers eingerichtet werden können und der Zugriff standardmäßig verweigert wird.

**Beispiel: mehrere Regeln**

| **Regelnr.** | **Remote-Subnetz** | **Endpunkt** | **Zulassen/Verweigern** |
| --- | --- | --- | --- |
| 100 |65.0.0.0/8 |3389 |Zulassen |
| 200 |159.0.0.0/8 |3389 |Zulassen |

### <a name="rule-order"></a>Reihenfolge der Regeln
Da für einen Endpunkt mehrere Regeln angegeben werden können, müssen die Regeln in einer bestimmten Reihenfolge angeordnet werden, um anzugeben, welche Regel Vorrang hat. Die Reihenfolge der Regeln gibt die Rangfolge an. Bei Netzwerk-ACLs hat jeweils der *niedrigste Wert Vorrang*. Im folgenden Beispiel wird dem Endpunkt an Port 80 selektiv Zugriff auf bestimmte IP-Adressbereiche gewährt. Zu diesem Zweck wird eine Verweigerungsregel (Nr.\# 100) für den Adressraum 175.1.0.1/24 konfiguriert. Außerdem wird noch eine zweite Regel (Nr. 200) angegeben, die den Zugriff auf alle anderen Adressen unter 175.0.0.0/8 zulässt.

**Beispiel: Regelreihenfolge**

| **Regelnr.** | **Remote-Subnetz** | **Endpunkt** | **Zulassen/Verweigern** |
| --- | --- | --- | --- |
| 100 |175.1.0.1/24 |80 |Verweigern |
| 200 |175.0.0.0/8 |80 |Zulassen |

## <a name="network-acls-and-load-balanced-sets"></a>Netzwerk-ACLs und Sätze mit Lastenausgleich
Netzwerk-ACLs können auf einem Endpunkt der Lastenausgleichsgruppe angegeben werden. Bei Angabe einer ACL für eine Lastenausgleichsgruppe wird die Netzwerk-ACL auf alle virtuellen Computer in dieser Lastenausgleichsgruppe angewendet. Ein Beispiel: Bei Erstellung einer Lastenausgleichsgruppe mit „Port 80“ und drei virtuellen Computern wird die Netzwerk-ACL, die auf dem Endpunkt „Port 80“ eines einzelnen virtuellen Computers erstellt wurde, automatisch auch auf die anderen virtuellen Computer angewendet.

![Netzwerk-ACLs und Sätze mit Lastenausgleich](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Nächste Schritte
[Verwalten von Endpunkt-Zugriffssteuerungslisten mit PowerShell](virtual-networks-acl-powershell.md)


