---
title: "Überprüfen der Konnektivität mit Azure Network Watcher – Azure-Portal | Microsoft-Dokumentation"
description: "Auf dieser Seite erfahren Sie, wie Sie die Konnektivitätsprüfung von Network Watcher mithilfe des Azure-Portals verwenden."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: gwallace
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 84774d0f40e06a819ca6de6cf0be68e17ba474e4
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---

# <a name="check-connectivity-with-azure-network-watcher-using-the-azure-portal"></a>Überprüfen der Konnektivität mit Azure Network Watcher mithilfe des Azure-Portals

> [!div class="op_single_selector"]
> - [Portal](network-watcher-connectivity-portal.md)
> - [PowerShell](network-watcher-connectivity-powershell.md)
> - [CLI 2.0](network-watcher-connectivity-cli.md)
> - [Azure-REST-API](network-watcher-connectivity-rest.md)

Erfahren Sie, wie Sie mit der Konnektivität überprüfen können, ob von einem virtuellen Computer zu einem bestimmten Endpunkt eine direkte TCP-Verbindung hergestellt werden kann.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie über die folgenden Ressourcen verfügen:

* Eine Instanz von Network Watcher in der Region, in der Sie die Konnektivität überprüfen möchten

* Virtuelle Computer, mit denen Sie die Konnektivität überprüfen.

> [!IMPORTANT]
> Für die Konnektivitätsprüfung ist die VM-Erweiterung `AzureNetworkWatcherExtension` erforderlich. Informationen zur Installation der Erweiterung finden Sie für einen virtuellen Windows-Computer unter [VM-Erweiterung für den Network Watcher-Agent für Windows](../virtual-machines/windows/extensions-nwa.md) und für einen virtuellen Linux-Computer unter [VM-Erweiterung für den Network Watcher-Agent für Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="check-connectivity-to-a-virtual-machine"></a>Überprüfen der Konnektivität zu einem virtuellen Computer

In diesem Beispiel wird die Konnektivität zu einem virtuellen Zielcomputer über Port 80 überprüft.

Navigieren Sie zu Network Watcher, und klicken Sie auf **Konnektivitätsprüfung (Vorschau)**. Wählen Sie den virtuellen Computer aus, über den die Konnektivität überprüft werden soll. Wählen Sie im Abschnitt **Ziel** die Option **Virtuellen Computer auswählen** und dann den jeweiligen zu testenden virtuellen Computer und Port.

Nachdem Sie auf **Überprüfen** geklickt haben, wird die Konnektivität zwischen den virtuellen Computern am angegebenen Port überprüft. In dem Beispiel ist die Ziel-VM nicht erreichbar, sodass eine Auflistung von Hops angezeigt wird.

![Ergebnisse der Konnektivitätsprüfung für einen virtuellen Computer][1]

## <a name="check-remote-endpoint-connectivity"></a>Überprüfen der Remoteendpunktkonnektivität

Um die Konnektivität mit einem Remoteendpunkt und dessen Latenz zu überprüfen, aktivieren Sie im Abschnitt **Ziel** das Optionsfeld **Manuell angeben**, geben Sie die URL sowie den Port ein, und klicken Sie auf **Überprüfen**.  Diese Angaben werden für Remoteendpunkte wie Websites und Speicherendpunkte verwendet.

![Ergebnisse der Konnektivitätsprüfung für eine Website][2]

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Automatisierung von Paketerfassungen mit VM-Warnungen finden Sie unter [Erstellen einer durch Warnungen ausgelösten Paketerfassung](network-watcher-alert-triggered-packet-capture.md).

Lesen Sie den Artikel zur [IP-Datenflussüberprüfung](network-watcher-check-ip-flow-verify-portal.md), um herauszufinden, ob bestimmter eingehender oder ausgehender Datenverkehr für Ihren virtuellen Computer zulässig ist.

[1]: ./media/network-watcher-connectivity-portal/figure1.png
[2]: ./media/network-watcher-connectivity-portal/figure2.png

