---
title: "Verwalten von Flowprotokollen für Netzwerksicherheitsgruppen mit Azure Network Watcher | Microsoft-Dokumentation"
description: "Auf dieser Seite wird erläutert, wie Flowprotokolle für Netzwerksicherheitsgruppen in Azure Network Watcher verwaltet werden."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 01606cbf-d70b-40ad-bc1d-f03bb642e0af
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: 41cb5ffab9bd3a3bed75ffdb6a7383ca1690f810
ms.contentlocale: de-de
ms.lasthandoff: 06/01/2017

---

# <a name="manage-network-security-group-flow-logs-in-the-azure-portal"></a>Verwalten von Flowprotokollen für Netzwerksicherheitsgruppen über das Azure-Portal

> [!div class="op_single_selector"]
> - [Azure-Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI 1.0](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [CLI 2.0](network-watcher-nsg-flow-logging-cli.md)
> - [REST-API](network-watcher-nsg-flow-logging-rest.md)

Flowprotokolle für Netzwerksicherheitsgruppen sind ein Network Watcher-Feature, mit dem Sie Informationen zu ein- und ausgehendem IP-Datenverkehr über eine Netzwerksicherheitsgruppe anzeigen können. Diese Flowprotokolle sind im JSON-Format geschrieben und stellen die folgenden wichtigen Informationen bereit: 

- Aus- und eingehende Datenflüsse auf Regelbasis
- Die Netzwerkkarte, auf die sich der Datenfluss bezieht
- 5-Tupel-Informationen über den Datenfluss (Quell-/Ziel-IP, Quell-/Zielport, Protokoll)
- Informationen zu zugelassenem oder verweigertem Datenverkehr

## <a name="before-you-begin"></a>Voraussetzungen

Dieses Szenario setzt voraus, dass Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](network-watcher-create.md) bereits durchgeführt haben. Ferner wird davon ausgegangen, dass eine Ressourcengruppe mit einem gültigen virtuellen Computer vorhanden ist.

## <a name="register-insights-provider"></a>Registrieren von Insights-Anbietern

Der Anbieter **Microsoft.Insights** muss registriert sein, damit die Datenflussprotokollierung ordnungsgemäß funktioniert. Führen Sie zum Registrieren des Anbieters die folgenden Schritte aus: 

1. Wechseln Sie zu **Abonnements**, und wählen Sie das Abonnement aus, für das Sie die Datenflussprotokollierung aktivieren möchten. 
2. Wählen Sie auf dem Blatt **Abonnement** die Option **Ressourcenanbieter** aus. 
3. Vergewissern Sie sich anhand der Liste der Anbieter, dass der Anbieter **microsoft.insights** registriert ist. Ist dies nicht der Fall, wählen Sie **Registrieren**.

![Anzeigen von Anbietern][providers]

## <a name="enable-flow-logs"></a>Aktivieren von Flowprotokollen

Diese Schritte führen Sie durch den Prozess des Aktivierens von Flowprotokollen in einer Netzwerksicherheitsgruppe.

### <a name="step-1"></a>Schritt 1

Wechseln Sie zu einer Network Watcher-Instanz, und wählen Sie **NSG-Flowprotokolle**.

![Übersicht zu Flowprotokollen][1]

### <a name="step-2"></a>Schritt 2

Wählen Sie in der Liste eine Netzwerksicherheitsgruppe aus.

![Übersicht zu Flowprotokollen][2]

### <a name="step-3"></a>Schritt 3 

Legen Sie auf dem Blatt **Flowprotokolleinstellungen** den Status auf **Ein** fest, und konfigurieren Sie ein Speicherkonto.  Wenn Sie fertig sind, wählen Sie **OK**. Wählen Sie dann **Speichern**.

![Übersicht zu Flowprotokollen][3]

## <a name="download-flow-logs"></a>Herunterladen von Flowprotokollen

Flowprotokolle werden in einem Speicherkonto gespeichert. Laden Sie Ihre Flowprotokolle herunter, um sie anzuzeigen.

### <a name="step-1"></a>Schritt 1

Um Flowprotokolle herunterzuladen, wählen Sie **Sie können Flowprotokolle aus konfigurierten Speicherkonten herunterladen**. So gelangen Sie zu einer Speicherkontoansicht. Dort können Sie wählen, welche Protokolle Sie herunterladen möchten.

![Flowprotokolleinstellungen][4]

### <a name="step-2"></a>Schritt 2

Wechseln Sie zum richtigen Speicherkonto. Wählen Sie **Container** > **insights-log-networksecuritygroupflowevent**.

![Flowprotokolleinstellungen][5]

### <a name="step-3"></a>Schritt 3

Wechseln Sie zum Speicherort des Flowprotokolls, wählen Sie das Flowprotokoll aus, und klicken Sie auf **Herunterladen**.

![Flowprotokolleinstellungen][6]

Informationen zur Struktur des Protokolls finden Sie unter [Einführung in die Datenflussprotokollierung für Netzwerksicherheitsgruppen](network-watcher-nsg-flow-logging-overview.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Ihre NSG-Flowprotokolle mit Power BI visualisieren](network-watcher-visualize-nsg-flow-logs-power-bi.md).

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-portal/figure1.png
[2]: ./media/network-watcher-nsg-flow-logging-portal/figure2.png
[3]: ./media/network-watcher-nsg-flow-logging-portal/figure3.png
[4]: ./media/network-watcher-nsg-flow-logging-portal/figure4.png
[5]: ./media/network-watcher-nsg-flow-logging-portal/figure5.png
[6]: ./media/network-watcher-nsg-flow-logging-portal/figure6.png
[providers]: ./media/network-watcher-nsg-flow-logging-portal/providers.png

