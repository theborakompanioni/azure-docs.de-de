---
title: "Verwalten von Paketerfassungen mit Azure Network Watcher – Azure-Portal | Microsoft-Dokumentation"
description: "Auf dieser Seite wird erläutert, wie das Network Watcher-Feature zur Paketerfassung mithilfe des Azure-Portals verwaltet wird."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 59edd945-34ad-4008-809e-ea904781d918
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: d2a65104743d9497debdc85c134fd1a06114c514
ms.openlocfilehash: f7739d38822d9cb57bc47f1c33f48045e5e97406
ms.lasthandoff: 02/23/2017

---

# <a name="manage-packet-captures-with-azure-network-watcher-using-the-portal"></a>Verwalten von Paketerfassungen mit Azure Network Watcher über das Portal

> [!div class="op_single_selector"]
> - [Azure-Portal](network-watcher-packet-capture-manage-portal.md)
> - [PowerShell](network-watcher-packet-capture-manage-powershell.md)
> - [BEFEHLSZEILENSCHNITTSTELLE (CLI)](network-watcher-packet-capture-manage-cli.md)
> - [REST-API](network-watcher-packet-capture-manage-rest.md)

Mithilfe der Paketerfassung von Network Watcher können Sie Sitzungen erfassen, um den eingehenden und ausgehenden Datenverkehr eines virtuellen Computers nachzuverfolgen. Für die Erfassungssitzung werden Filter bereitgestellt, um sicherzustellen, dass nur der gewünschte Datenverkehr erfasst wird. Mithilfe der Paketerfassung können Sie Netzwerkanomalien sowohl reaktiv als auch proaktiv diagnostizieren. Weitere Verwendungszwecke sind das Erfassen von Netzwerkstatistiken, das Gewinnen von Informationen zu Netzwerkangriffen, das Debuggen der Kommunikation zwischen Client und Server und vieles mehr. Durch die Möglichkeit zur Remoteauslösung von Paketerfassungen erleichtert diese Funktion die manuelle Ausführung einer Paketerfassung auf dem gewünschten Computer. So sparen Sie wertvolle Zeit.

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

Dieser Artikel führt Sie durch die verschiedenen Verwaltungsaufgaben, die derzeit für die Paketerfassung verfügbar sind.

- [**Starten einer Paketerfassung**](#start-a-packet-capture)
- [**Beenden einer Paketerfassung**](#stop-a-packet-capture)
- [**Löschen einer Paketerfassung**](#delete-a-packet-capture)
- [**Herunterladen einer Paketerfassung**](#download-a-packet-capture)

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie über die folgenden Ressourcen verfügen:

- Eine Instanz von Network Watcher in der Region, in der Sie eine Paketerfassung erstellen möchten
- Einen virtuellen Computer mit aktivierter Paketerfassungserweiterung

> [!IMPORTANT]
> Für die Paketerfassung ist die VM-Erweiterung `AzureNetworkWatcherExtension` erforderlich. Informationen zur Installation der Erweiterung finden Sie für einen virtuellen Windows-Computer unter [VM-Erweiterung für den Network Watcher-Agent für Windows](../virtual-machines/virtual-machines-windows-extensions-nwa.md) und für einen virtuellen Linux-Computer unter [VM-Erweiterung für den Network Watcher-Agent für Linux](../virtual-machines/virtual-machines-linux-extensions-nwa.md).

### <a name="packet-capture-agent-extension-through-the-portal"></a>Erweiterung für Paketerfassungs-Agent über das Portal

Navigieren Sie zum Installieren des VM-Agents für die Paketerfassung über das Portal zu Ihrem virtuellen Computer, klicken Sie auf **Erweiterungen** > **Hinzufügen**, und suchen Sie nach **Network Watcher Agent für Windows**.

![Agent-Ansicht][agent]

## <a name="packet-capture-overview"></a>Übersicht zur Paketerfassung

Navigieren Sie zum [Azure-Portal](https://portal.azure.com), und klicken Sie auf **Netzwerk** > **Network Watcher** > **Paketerfassung**.

Auf der Übersichtsseite wird eine Liste mit allen vorhandenen Paketerfassungen angezeigt. Der Status spielt dabei keine Rolle.

> [!NOTE]
> Für die Paketerfassung ist eine Verbindung mit dem Speicherkonto über Port 443 erforderlich.

![Übersichtsseite zur Paketerfassung][1]

## <a name="start-a-packet-capture"></a>Starten einer Paketerfassung

Klicken Sie auf **Hinzufügen**, um eine Paketerfassung zu erstellen.

Folgende Eigenschaften können für eine Paketerfassung definiert werden:

**Haupteinstellungen**

- **Abonnement**: Dieser Wert ist das verwendete Abonnement. Für jedes Abonnement wird eine Instanz von Network Watcher benötigt.
- **Ressourcengruppe**: Die Ressourcengruppe des virtuellen Computers, für die der Vorgang durchgeführt werden soll.
- **Virtueller Zielcomputer**: Der virtuelle Computer, auf dem die Paketerfassung durchgeführt wird.
- **Paketerfassungsname**: Dieser Wert gibt den Namen der Paketerfassung an.

**Konfiguration erfassen**

- **Storage**: Gibt an, ob die Paketerfassung in einem Speicherkonto gespeichert wird.
- **Datei**: Gibt an, ob eine Paketerfassung lokal auf dem virtuellen Computer gespeichert wird.
- **Speicherkonten**: Das ausgewählte Speicherkonto, unter dem die Paketerfassung gespeichert wird. Der Standardspeicherort ist „https://{Name des Speicherkontos}.blob.core.windows.net/network-watcher-logs/subscriptions/{Abonnement-ID}/resourcegroups/{Name der Ressourcengruppe}/providers/microsoft.compute/virtualmachines/{Name des virtuellen Computers}/{JJ}/{MM}/{TT}/packetcapture_{HH}_{MM}_{SS}_{XXX}.cap“. (Nur aktiviert, wenn **Storage** ausgewählt ist.)
- **Lokaler Dateipfad**: Der lokale Dateipfad auf einem virtuellen Computer zum Speichern der Paketerfassung. (Nur aktiviert, wenn **Datei** ausgewählt ist.) Ein gültiger Pfad muss angegeben werden.
- **Maximale Anzahl von Bytes pro Paket**: Die Anzahl von Bytes aus jedem Paket, die erfasst werden. Wenn keine Angabe erfolgt, werden alle Bytes erfasst.
- **Maximale Anzahl von Bytes pro Sitzung**: Die Gesamtzahl der erfassten Bytes, nachdem die Paketerfassung beendet wurde.
- **Zeitlimit (Sekunden)**: Legt ein Zeitlimit für die Beendigung der Paketerfassung fest. Der Standardwert ist 1.800 Sekunden.

> [!NOTE]
> Für Storage Premium-Konten wird das Speichern von Paketerfassungen derzeit nicht unterstützt.

**Filterung (optional)**

- **Protokoll**: Das zu filternde Protokoll für die Paketerfassung. Die verfügbaren Werte sind „TCP“, „UDP“ und „Alle“.
- **Lokale IP-Adresse**: Dieser Wert filtert bei der Paketerfassung nach den Paketen, deren lokale IP-Adresse mit diesem Filterwert übereinstimmt.
- **Lokaler Port**: Dieser Wert filtert bei der Paketerfassung nach den Paketen, deren lokaler Port mit diesem Filterwert übereinstimmt.
- **Remote-IP-Adresse**: Dieser Wert filtert bei der Paketerfassung nach den Paketen, deren Remote-IP-Adresse mit diesem Filterwert übereinstimmt.
- **Remoteport**: Dieser Wert filtert bei der Paketerfassung nach den Paketen, deren Remoteport mit diesem Filterwert übereinstimmt.

> [!NOTE]
> Bei den Werten für Port und IP-Adresse kann es sich um einen Einzelwert, einen Bereich von Werten oder einen Satz handeln (für den Port 80 bis 1.024). Sie können beliebig viele Filter definieren.

Klicken Sie nach dem Einfügen der Werte auf **OK**, um die Paketerfassung zu erstellen.

![Erstellen einer Paketerfassung][2]

Nachdem das für die Paketerfassung festgelegte Zeitlimit abgelaufen ist, wird die Paketerfassung beendet und kann überprüft werden. Sie können Paketerfassungssitzungen auch manuell beenden.

## <a name="delete-a-packet-capture"></a>Löschen einer Paketerfassung

Klicken Sie in der Paketerfassungsansicht auf das **Kontextmenü** (...), oder klicken Sie mit der rechten Maustaste, und klicken Sie dann auf **Löschen**, um die Paketerfassung zu beenden.

![Löschen einer Paketerfassung][3]

> [!NOTE]
> Durch das Löschen einer Paketerfassung wird die Datei im Speicherkonto nicht gelöscht.

Klicken Sie auf **Ja**, wenn die Aufforderung zum Bestätigen des Löschvorgangs angezeigt wird.

![Bestätigung][4]

## <a name="stop-a-packet-capture"></a>Beenden einer Paketerfassung

Klicken Sie in der Paketerfassungsansicht auf das **Kontextmenü** (...), oder klicken Sie mit der rechten Maustaste, und klicken Sie dann auf **Beenden**, um die Paketerfassung zu beenden.

## <a name="download-a-packet-capture"></a>Herunterladen einer Paketerfassung

Nachdem die Paketerfassungssitzung abgeschlossen wurde, wird die Erfassungsdatei in den Blobspeicher oder in eine lokale Datei auf dem virtuellen Computer hochgeladen. Der Speicherort der Paketerfassung wird beim Erstellen der Sitzung definiert. Ein nützliches Tool für den Zugriff auf diese in einem Speicherkonto gespeicherten Erfassungsdateien ist der Microsoft Azure-Speicher-Explorer, der hier heruntergeladen werden kann: http://storageexplorer.com/

Wenn ein Speicherkonto angegeben wird, werden Paketerfassungsdateien in einem Speicherkonto am folgenden Speicherort gespeichert:
```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Automatisierung von Paketerfassungen mit VM-Warnungen finden Sie unter [Erstellen einer durch Warnungen ausgelösten Paketerfassung](network-watcher-alert-triggered-packet-capture.md).

Lesen Sie den Artikel zur [IP-Datenflussüberprüfung](network-watcher-check-ip-flow-verify-portal.md), um herauszufinden, ob bestimmter eingehender oder ausgehender Datenverkehr für Ihren virtuellen Computer zulässig ist.

<!-- Image references -->
[1]: ./media/network-watcher-packet-capture-manage-portal/figure1.png
[2]: ./media/network-watcher-packet-capture-manage-portal/figure2.png
[3]: ./media/network-watcher-packet-capture-manage-portal/figure3.png
[4]: ./media/network-watcher-packet-capture-manage-portal/figure4.png
[agent]: ./media/network-watcher-packet-capture-manage-portal/agent.png














