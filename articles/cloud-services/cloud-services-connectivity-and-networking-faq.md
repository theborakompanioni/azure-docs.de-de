---
title: "Probleme mit Konnektivität und Netzwerken in Microsoft Azure Cloud Services – Häufig gestellte Fragen | Microsoft-Dokumentation"
description: "Dieser Artikel behandelt häufig gestellte Fragen zu Konnektivität und Netzwerkproblemen in Microsoft Azure Cloud Services."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 3fc0d34fdb617ebb1af9c9f33e018d5fe6ec9a7d
ms.contentlocale: de-de
ms.lasthandoff: 06/15/2017


---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Probleme mit Konnektivität und Netzwerken in Microsoft Azure Cloud Services – Häufig gestellte Fragen (FAQs)

Dieser Artikel enthält häufig gestellte Fragen zu Konnektivität und Netzwerkproblemen in [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Informationen zur Größe finden sie auch unter [Größen für Clouddienste](cloud-services-sizes-specs.md) .

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Ich kann keine IP in einem Clouddienst mit mehreren VIPs reservieren.
Stellen Sie zunächst sicher, dass die Instanz des virtuellen Computers, für den Sie die IP reservieren möchten, eingeschaltet ist. Vergewissern Sie sich anschließend, dass Sie reservierte IPs für Staging- und Produktionsbereitstellungen verwenden. **nicht** , während ein Upgrade der Bereitstellung durchgeführt wird.

## <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a>Wie verwende ich Remotedesktop bei einer NSG?
Fügen Sie der NSG Regeln hinzu, die Datenverkehr an den Ports **3389** und **20000** zulassen.  Remotedesktop verwendet Port **3389**.  Für Cloud Services-Instanzen erfolgt ein Lastenausgleich, daher können Sie nicht direkt steuern, mit welcher Instanz eine Verbindung hergestellt wird.  Die Agents *RemoteForwarder* und *RemoteAccess* verwalten den RDP-Datenverkehr und ermöglichen es dem Client, ein RDP-Cookie zu senden und eine bestimmte Instanz anzugeben, mit der eine Verbindung hergestellt werden soll.  Die Agents *RemoteForwarder* und *RemoteAccess* fordern, dass Port **20000** geöffnet wird, der bei Vorhandensein einer NSG möglicherweise blockiert ist.

## <a name="can-i-ping-a-cloud-service"></a>Kann ich meinen Clouddienst mit „Ping“ abfragen?

Nein, nicht mithilfe des normalen „ping“/ICMP-Protokolls. Das ICMP-Protokoll ist durch Azure Load Balancer nicht zulässig.

Wir empfehlen, dass Sie die Konnektivität mithilfe eines Ping-Signals an einen Port testen. Während „ping.exe“ ICMP verwendet, ermöglichen es andere Tools, z.B. PSPing, Nmap und telnet, die Konnektivität zu einem bestimmten TCP-Port zu testen.

Weitere Informationen finden Sie unter [Use port pings instead of ICMP to test Azure VM connectivity (Verwenden eines Ping-Signals an einen Port statt ICMP zum Testen von Azure-VM-Konnektivität)](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/).

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-indicate-some-sort-of-malicious-attack-to-the-cloud-service"></a>Wie verhindere ich, Tausende von Treffern von unbekannten IP-Adressen zu erhalten, die einen böswilligen Angriff auf den Clouddienst andeuten?
Azure implementiert mehrstufige Netzwerksicherheit zum Schutz von Platform-Diensten gegen verteilte Denial-of-Services-Angriffe (DDoS). Das Azure DDoS-Verteidigungssystem ist Teil des ständigen Überwachungsprozesses von Azure, der ständig per Penetrationstests verbessert wird. Dieses DDoS-Verteidigungssystem ist darauf ausgelegt, nicht nur Angriffen von außerhalb, sondern auch von anderen Azure-Mandanten standzuhalten. Weitere Informationen finden Sie unter [Microsoft Azure-Netzwerksicherheit](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf).

Sie können auch Starttasks erstellen, um einige bestimmte IP-Adressen selektiv zu blockieren. Weitere Informationen finden Sie unter [Blockieren bestimmter IP-Adressen](cloud-services-startup-tasks-common.md#block-a-specific-ip-address).

## <a name="when-i-try-to-rdp-to-my-cloud-service-instance-i-get-the-message-the-user-account-has-expired"></a>Wenn ich versuche, eine RDP-Verbindung mit meiner Cloud Service-Instanz herzustellen, erhalte ich die Meldung „Dieses Benutzerkonto ist abgelaufen.“
Sie erhalten die Fehlermeldung „Dieses Benutzerkonto ist abgelaufen“ möglicherweise, wenn Sie das Ablaufdatum umgehen, das in Ihren RDP-Einstellungen festgelegt ist. Sie können das Ablaufdatum mithilfe der folgenden Schritte im Portal ändern:
1. Melden Sie sich in der Azure-Verwaltungskonsole an (https://manage.windowsazure.com), navigieren Sie zu Ihrem Clouddienst, und wählen Sie die Registerkarte **Konfigurieren** aus.
2. Wählen Sie **Remote** aus.
3. Ändern Sie das Datum bei „Gültig bis“, und speichern Sie die Konfiguration anschließend.

Jetzt sollten Sie eine RDP-Verbindung zu Ihrem Computer herstellen können.

## <a name="why-is-loadbalancer-not-balancing-traffic-equally"></a>Warum gleicht Load Balancer Datenverkehr nicht gleichmäßig aus?
Weitere Informationen zur Funktionsweise von internen Lastenausgleichen finden Sie unter [Azure Load Balancer-Neue Verteilung](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/).

Standardmäßig wird ein 5-Tupel-Hash-Verteilungsalgorithmus (Quell-IP, Quellport, IP-Zieladresse, Zielport, Protokolltyp) zum Zuordnen des Datenverkehrs an verfügbare Server verwendet. Dabei wird Bindung nur in einer Transportsitzung angeboten. Pakete in derselben TCP- oder UDP-Sitzung werden an die gleiche Instanz der Rechenzentrums-IP (DIP) hinter dem Lastenausgleichs-Endpunkt geleitet. Wenn der Client die Verbindung schließt und erneut öffnet oder eine neue Sitzung über die gleiche Quell-IP startet, wird der Quellport geändert, sodass der Datenverkehr an einen anderen DIP-Endpunkt geleitet wird.

