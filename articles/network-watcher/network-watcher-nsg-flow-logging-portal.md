---
title: "Verwalten von Flowprotokollen für Netzwerksicherheitsgruppen mit Azure Network Watcher | Microsoft-Dokumentation"
description: "Auf dieser Seite wird erläutert, wie Flowprotokolle für Netzwerksicherheitsgruppen in Azure Network Watcher verwaltet werden."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 01606cbf-d70b-40ad-bc1d-f03bb642e0af
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: d716f11818bc0ad6dd9e5f93951b011dd6774c7b
ms.lasthandoff: 03/22/2017


---

# <a name="manage-network-security-group-flow-logs-in-the-azure-portal"></a>Verwalten von Flowprotokollen für Netzwerksicherheitsgruppen über das Azure-Portal

> [!div class="op_single_selector"]
> - [Azure-Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [BEFEHLSZEILENSCHNITTSTELLE (CLI)](network-watcher-nsg-flow-logging-cli.md)
> - [REST-API](network-watcher-nsg-flow-logging-rest.md)

Flowprotokolle für Netzwerksicherheitsgruppen sind ein Network Watcher-Feature, mit dem Sie Informationen zu ein- und ausgehendem IP-Datenverkehr über eine Netzwerksicherheitsgruppe anzeigen können. Diese Flowprotokolle sind im JSON-Format geschrieben und zeigen ausgehende und eingehende Datenflüsse pro Regel, die NIC, auf die sich der Datenfluss bezieht, 5-Tupel-Informationen über den Datenfluss (Quell-/Ziel-IP, Quell-/Zielport, Protokoll) und Informationen zu zugelassenem oder verweigertem Datenverkehr.

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

## <a name="before-you-begin"></a>Voraussetzungen

Dieses Szenario setzt voraus, dass Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](network-watcher-create.md) bereits durchgeführt haben, um eine Network Watcher-Instanz zu erstellen. Ferner wird davon ausgegangen, dass eine Ressourcengruppe mit einem gültigen virtuellen Computer vorhanden ist und verwendet werden kann.

## <a name="enable-flow-logs"></a>Aktivieren von Flowprotokollen

Diese Schritte führen Sie durch das Aktivieren von Flowprotokollen in einer Netzwerksicherheitsgruppe.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu einer Network Watcher-Instanz, und wählen Sie **Flowprotokolle**.

![Übersicht zu Flowprotokollen][1]

### <a name="step-2"></a>Schritt 2

Wählen Sie eine Netzwerksicherheitsgruppe aus der Liste, indem Sie darauf klicken.

![Übersicht zu Flowprotokollen][2]

### <a name="step-3"></a>Schritt 3 

Legen Sie auf dem Blatt **Flowprotokolleinstellungen** den Status auf **Ein** fest, und konfigurieren Sie ein Speicherkonto.  Wenn Sie fertig sind, klicken Sie auf **OK** und **Speichern**.

![Übersicht zu Flowprotokollen][3]

## <a name="download-flow-logs"></a>Herunterladen von Flowprotokollen

Flowprotokolle werden in einem Speicherkonto gespeichert. Um Ihre Flowprotokolle anzuzeigen, müssen Sie sie herunterladen.

### <a name="step-1"></a>Schritt 1

Um Flowprotokolle herunterzuladen, klicken Sie auf **Sie können Flowprotokolle aus konfigurierten Speicherkonten herunterladen**.  So gelangen Sie zu einer Speicherkontoansicht, wo Sie zum Herunterladen zu Ihrem Protokoll navigieren können.

![Flowprotokolleinstellungen][4]

### <a name="step-2"></a>Schritt 2

Navigieren Sie zum richtigen Speicherkonto und dann zu **Container** > **insights-log-networksecuritygroupflowevent**.

![Flowprotokolleinstellungen][5]

### <a name="step-3"></a>Schritt 3

Navigieren Sie zum Speicherort des Flowprotokolls, wählen Sie das Flowprotokoll aus, und klicken Sie auf **Herunterladen**.

![Flowprotokolleinstellungen][6]

Informationen zur Struktur des Protokolls finden Sie unter [Introduction to flow logging for Network Security Groups](network-watcher-nsg-flow-logging-overview.md) (Einführung in die Flowprotokollierung für Netzwerksicherheitsgruppen).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Ihre NSG-Flowprotokolle mit Power BI visualisieren](network-watcher-visualize-nsg-flow-logs-power-bi.md).

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-portal/figure1.png
[2]: ./media/network-watcher-nsg-flow-logging-portal/figure2.png
[3]: ./media/network-watcher-nsg-flow-logging-portal/figure3.png
[4]: ./media/network-watcher-nsg-flow-logging-portal/figure4.png
[5]: ./media/network-watcher-nsg-flow-logging-portal/figure5.png
[6]: ./media/network-watcher-nsg-flow-logging-portal/figure6.png

