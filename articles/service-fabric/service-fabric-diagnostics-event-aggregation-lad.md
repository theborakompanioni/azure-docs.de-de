---
title: Azure Service Fabric-Ereignisaggregation mit Linux-Azure-Diagnose | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Ereignisse unter Verwendung von LAD zur Überwachung und Diagnose von Azure Service Fabric-Clustern aggregieren und sammeln."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2017
ms.author: dekapur
ms.translationtype: HT
ms.sourcegitcommit: 0425da20f3f0abcfa3ed5c04cec32184210546bb
ms.openlocfilehash: bcc3a229369a065cfcfbd32eadbf3f6ae6fe0036
ms.contentlocale: de-de
ms.lasthandoff: 07/20/2017

---

# <a name="event-aggregation-and-collection-using-linux-azure-diagnostics"></a>Ereignisaggregation und -sammlung mit Linux-Azure-Diagnose
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Bei Verwendung eines Azure Service Fabric-Clusters empfiehlt es sich, die Protokolle aller Knoten an einem zentralen Ort zu sammeln. Das Sammeln der Protokolle an einem zentralen Ort hilft Ihnen bei Analyse und Behandlung von Problemen, die ggf. in Ihrem Cluster oder in den Anwendungen und Diensten des Clusters auftreten.

Eine Möglichkeit zum Hochladen und Sammeln von Protokollen ist die Verwendung der Erweiterung Linux-Azure-Diagnose (LAD), mit der Protokolle in Azure Storage hochgeladen werden und an Azure Application Insights oder Event Hubs gesendet werden können. Sie können zudem einen externen Prozess verwenden, um die Ereignisse aus dem Speicher zu lesen und in einem Analyseplattformprodukt wie [OMS Log Analytics](../log-analytics/log-analytics-service-fabric.md) oder in einer anderen Protokollanalyselösung zu verwenden.

## <a name="log-and-event-sources"></a>Protokoll- und Ereignisquellen

### <a name="service-fabric-platform-events"></a>Service Fabric-Plattformereignisse
Service Fabric gibt über [LTTng](http://lttng.org) einige vordefinierte Protokolle aus, einschließlich Betriebs- oder Laufzeitereignissen. Diese Protokolle werden an dem in der Resource Manager-Vorlage des Clusters angegebenen Speicherort gespeichert. Um Informationen zum Speicherkonto abzurufen oder festzulegen, suchen Sie nach dem Tag **AzureTableWinFabETWQueryable** und dort nach **StoreConnectionString**.

### <a name="application-events"></a>Anwendungsereignisse
 Ereignisse, die über den Code Ihrer Anwendungen und Dienste so ausgegeben werden, wie Sie dies beim Instrumentieren der Software angegeben haben. Sie können eine beliebige Protokollierungslösung verwenden, die textbasierte Protokolldateien schreibt (beispielsweise LTTng). Weitere Informationen finden Sie in der LTTng-Dokumentation zur Ablaufverfolgung für Ihre Anwendung.

[Überwachen und Diagnostizieren von Diensten in einer Entwicklungsumgebung auf einem lokalen Computer](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

## <a name="deploy-the-diagnostics-extension"></a>Bereitstellen der Diagnoseerweiterung
Zum Sammeln von Protokollen muss zunächst die Diagnoseerweiterung auf allen VMs des Service Fabric-Clusters bereitgestellt werden. Die Diagnoseerweiterung sammelt Protokolle auf allen VMs und lädt sie in das angegebene Speicherkonto hoch. Die auszuführenden Schritte hängen davon ab, ob Sie das Azure-Portal oder Azure Resource Manager verwenden.

Wenn Sie die Diagnoseerweiterung im Rahmen der Clustererstellung für die im Cluster enthaltenen virtuellen Computer bereitstellen möchten, legen Sie **Diagnose** auf **Ein** fest. Nach der Erstellung des Clusters können Sie diese Einstellung nicht im Portal ändern.

Konfigurieren Sie dann die Linux-Azure-Diagnose (LAD) so, dass sie die Dateien erfasst und in Ihrem Speicherkonto ablegt. Dieser Prozess wird im Artikel [Verwenden der Linux-Diagnoseerweiterung zur Überwachung der Leistungs- und Diagnosedaten einer Linux-VM](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) als drittes Szenario („Hochladen eigener Protokolldateien“) beschrieben. Befolgen Sie diesen Prozess, um Zugriff auf die Ablaufverfolgungen zu erhalten. Sie können die Ablaufverfolgungsdaten in ein Visualisierungstool Ihrer Wahl hochladen.

Die Diagnoseerweiterung kann auch mithilfe von Azure Resource Manager bereitgestellt werden. Die Dokumentation für Windows-Cluster finden Sie unter [Sammeln von Protokollen mit Azure-Diagnose](service-fabric-diagnostics-how-to-setup-wad.md). Für Linux wird ein ähnlicher Prozess verwendet.

Sie können auch die Operations Management Suite verwenden, wie unter [Operations Management Suite Log Analytics with Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/) (Operations Management Suite-Protokollanalyse mit Linux) beschrieben.

Nach Abschluss der Konfiguration überwacht der LAD-Agent die angegebenen Protokolldateien. Sobald bei einer Datei eine neue Zeile hinzukommt, erstellt der Agent einen Syslog-Eintrag, der an den von Ihnen angegebenen Speicher gesendet wird.

## <a name="next-steps"></a>Nächste Schritte

Ausführlichere Informationen zu relevanten Ereignissen für die Problembehandlung finden Sie in der [LTTng-Dokumentation](http://lttng.org/docs) sowie unter [Verwenden der Linux-Diagnoseerweiterung zur Überwachung der Leistungs- und Diagnosedaten einer Linux-VM](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).
