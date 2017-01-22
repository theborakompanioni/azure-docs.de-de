---
title: "Speicherkonfiguration für SQL Server-VMs | Microsoft Docs"
description: "In diesem Thema wird beschrieben, wie Azure den Speicher für SQL Server-VMs während der Bereitstellung konfiguriert (Resource Manager-Bereitstellungsmodell). Außerdem wird erläutert, wie Sie den Speicher für Ihre vorhandenen SQL Server-VMs konfigurieren können."
services: virtual-machines-windows
documentationcenter: na
author: ninarn
manager: jhubbard
tags: azure-resource-manager
ms.assetid: 169fc765-3269-48fa-83f1-9fe3e4e40947
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 11/11/2016
ms.author: ninarn
translationtype: Human Translation
ms.sourcegitcommit: 0c23ee550d8ac88994e8c7c54a33d348ffc24372
ms.openlocfilehash: 3fe2a44d7e05538bdc3341110a517e7e9bdf3e7d


---
# <a name="storage-configuration-for-sql-server-vms"></a>Speicherkonfiguration für SQL Server-VMs
Wenn Sie in Azure einen virtuellen SQL Server-Computer konfigurieren, unterstützt Sie das Portal beim Automatisieren Ihrer Speicherkonfiguration. Hierzu gehören auch das Anfügen von Speicher an die VM, das Verfügbarmachen dieses Speichers für SQL Server und die anschließende Konfiguration, um eine Optimierung in Bezug auf Ihre besonderen Leistungsanforderungen zu erzielen.

In diesem Thema wird erläutert, wie der Speicher unter Azure für Ihre SQL Server-VMs konfiguriert wird – sowohl während der Bereitstellung als auch für vorhandene VMs. Diese Konfiguration basiert auf den [bewährten Methoden für die Leistung](virtual-machines-windows-sql-performance.md) für Azure-VMs, auf denen SQL Server ausgeführt wird.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Voraussetzungen
Zur Verwendung der Einstellungen für die automatische Speicherkonfiguration muss Ihr virtueller Computer über die folgenden Merkmale verfügen:

* Bereitstellung mit einem [SQL Server-Katalogimage](virtual-machines-windows-sql-server-iaas-overview.md#option-1-create-a-sql-vm-with-per-minute-licensing)
* Verwendung des [Resource Manager-Bereitstellungsmodells](../../../azure-resource-manager/resource-manager-deployment-model.md)
* Verwendung von [Storage Premium](../../../storage/storage-premium-storage.md)

## <a name="new-vms"></a>Neue virtuelle Computer
In den folgenden Abschnitten wird beschrieben, wie Speicher für neue virtuelle SQL Server-Computer konfiguriert wird.

### <a name="azure-portal"></a>Azure-Portal
Beim Bereitstellen einer Azure-VM mit einem SQL Server-Katalogimage können Sie auswählen, dass der Speicher für die neue VM automatisch konfiguriert wird. Sie geben die Speichergröße, die Leistungsgrenzwerte und den Workloadtyp an. Im folgenden Screenshot ist das Blatt für die Speicherkonfiguration dargestellt, das während der SQL-VM-Bereitstellung verwendet wird.

![SQL Server-VM-Speicherkonfiguration während der Bereitstellung](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

Je nach Ihrer Auswahl führt Azure nach dem Erstellen der VM die folgenden Aufgaben der Speicherkonfiguration durch:

* Erstellt Storage Premium-Datenträger und fügt sie an den virtuellen Computer an.
* Konfiguriert die Datenträger so, dass sie für SQL Server zugänglich sind.
* Konfiguriert die Datenträger in einem Speicherpool basierend auf der angegebenen Größe und den Leistungsanforderungen (IOPS und Durchsatz).
* Ordnet dem Speicherpool ein neues Laufwerk auf dem virtuellen Computer zu.
* Optimiert dieses neue Laufwerk basierend auf Ihrem angegebenen Workloadtyp („Data Warehousing“, „Transaktionsverarbeitung“ oder „Allgemein“).

Weitere Details dazu, wie unter Azure Speichereinstellungen konfiguriert werden, finden Sie im [Abschnitt zur Speicherkonfiguration](#storage-configuration). Eine vollständige exemplarische Vorgehensweise zur Erstellung einer SQL Server-VM im Azure-Portal finden Sie unter [Tutorial zur Bereitstellung](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Resource Manager-Vorlagen
Wenn Sie die folgenden Resource Manager-Vorlagen verwenden, werden standardmäßig zwei Premium-Datenträger ohne Speicherpoolkonfiguration angefügt. Sie können diese Vorlagen aber anpassen, um die Anzahl von Premium-Datenträgern zu ändern, die an den virtuellen Computer angefügt sind.

* [Create VM with Automated Backup](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
* [Create VM with Automated Patching](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
* [Create VM with AKV Integration](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault) (Erstellen eines virtuellen Computers mit AKV-Integration)

## <a name="existing-vms"></a>Vorhandene virtuelle Computer
Für vorhandene SQL Server-VMs können Sie im Azure-Portal einige Speichereinstellungen ändern. Wählen Sie Ihre VM aus, greifen Sie auf den Bereich „Einstellungen“ zu, und wählen Sie dann „SQL Server-Konfiguration“. Auf dem Blatt „SQL Server-Konfiguration“ wird die aktuelle Speichernutzung Ihrer VM angezeigt. Alle Laufwerke, die auf Ihrer VM vorhanden sind, werden in diesem Diagramm angezeigt. Für jedes Laufwerk wird der Speicherplatz in vier Abschnitten angezeigt:

* SQL-Daten
* SQL-Protokoll
* Andere (anderer Speicher als SQL-Speicher)
* Verfügbar

![Konfigurieren von Speicher für vorhandene SQL Server-VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

Klicken Sie oberhalb des Diagramms auf den Link „Bearbeiten“, um den Speicher für das Hinzufügen eines neuen Laufwerks oder Erweitern eines vorhandenen Laufwerks zu konfigurieren.

Die angezeigten Konfigurationsoptionen variieren in Abhängigkeit davon, ob Sie dieses Feature schon einmal verwendet haben. Wenn Sie es zum ersten Mal verwenden, können Sie die Speicheranforderungen für ein neues Laufwerk angeben. Falls Sie dieses Feature schon einmal zum Erstellen eines Laufwerks genutzt haben, können Sie den Speicher des Laufwerks bei Bedarf erweitern.

### <a name="use-for-the-first-time"></a>Erstmalige Verwendung
Wenn Sie dieses Feature zum ersten Mal verwenden, können Sie die Speichergröße und die Leistungsgrenzwerte für ein neues Laufwerk angeben. Diese Oberfläche ist mit der Anzeige zur Bereitstellungszeit vergleichbar. Der Hauptunterschied besteht darin, dass Sie nicht berechtigt sind, den Workloadtyp anzugeben. Mit dieser Einschränkung wird verhindert, dass SQL Server-Konfigurationen, die auf dem virtuellen Computer vorhanden sind, beschädigt werden.

![Konfigurieren von SQL Server-Speicherschiebereglern](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-usage-sliders.png)

In Azure wird ein neues Laufwerk basierend auf Ihren Spezifikationen erstellt. In diesem Szenario werden in Azure die folgenden Aufgaben der Speicherkonfiguration durchgeführt:

* Erstellt Storage Premium-Datenträger und fügt sie an den virtuellen Computer an.
* Konfiguriert die Datenträger so, dass sie für SQL Server zugänglich sind.
* Konfiguriert die Datenträger in einem Speicherpool basierend auf der angegebenen Größe und den Leistungsanforderungen (IOPS und Durchsatz).
* Ordnet dem Speicherpool ein neues Laufwerk auf dem virtuellen Computer zu.

Weitere Details dazu, wie unter Azure Speichereinstellungen konfiguriert werden, finden Sie im [Abschnitt zur Speicherkonfiguration](#storage-configuration).

### <a name="add-a-new-drive"></a>Hinzufügen eines neuen Laufwerks
Wenn Sie auf Ihrer SQL Server-VM bereits Speicher konfiguriert haben, ergeben sich beim Erweitern des Speichers bis zu zwei neue Optionen. Die erste Option ist das Hinzufügen eines neuen Laufwerks, womit die Leistungsebene Ihrer VM gesteigert werden kann.

![Hinzufügen eines neuen Laufwerks zu einer SQL-VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-add-new-drive.png)

Nach dem Hinzufügen des Laufwerks müssen Sie aber eine zusätzliche manuelle Konfiguration durchführen, um die Leistungssteigerung zu erzielen.

### <a name="extend-the-drive"></a>Erweitern des Laufwerks
Die andere Option für die Erweiterung des Speichers besteht darin, das vorhandene Laufwerk zu erweitern. Mit dieser Option wird der verfügbare Speicherplatz für Ihr Laufwerk erhöht, aber die Leistung wird nicht gesteigert. Bei Speicherpools können Sie die Anzahl von Spalten nach der Erstellung des Speicherpools nicht mehr ändern. Anhand der Anzahl von Spalten wird die Anzahl von parallelen Schreibvorgängen bestimmt, die auf die Datenträger verteilt werden können. Aus diesem Grund kann die Leistung durch das Hinzufügen von Datenträgern nicht erhöht werden. So kann nur mehr Speicher für die zu schreibenden Daten bereitgestellt werden. Diese Beschränkung bedeutet auch, dass die Anzahl von Spalten beim Erweitern des Laufwerks die minimale Anzahl von Datenträgern bestimmt, die Sie hinzufügen können. Wenn Sie einen Speicherpool mit vier Datenträgern erstellen, beträgt die Anzahl von Spalten also 4. Bei jeder Erweiterung des Speichers müssen Sie mindestens vier Datenträger hinzufügen.

![Erweitern eines Laufwerks für eine SQL-VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Speicherkonfiguration
Dieser Abschnitt dient als Referenz für die Änderungen der Speicherkonfiguration, die von Azure automatisch vorgenommen werden, während die SQL-VM-Bereitstellung oder -konfiguration im Azure-Portal durchgeführt wird.

* Falls Sie weniger als zwei TB Speicher für Ihre VM ausgewählt haben, erstellt Azure keinen Speicherpool.
* Falls Sie mindestens zwei TB Speicher für Ihre VM ausgewählt haben, konfiguriert Azure einen Speicherpool. Im nächsten Abschnitt dieses Themas sind die Details der Speicherpoolkonfiguration angegeben.
* Bei der automatischen Speicherkonfiguration werden immer [Storage Premium](../../../storage/storage-premium-storage.md) -P30-Datenträger verwendet. Daher besteht eine 1:1-Zuordnung zwischen der ausgewählten TB-Menge und der Anzahl von Datenträgern, die an die VM angefügt sind.

Preisinformationen finden Sie auf der Seite [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage) auf der Registerkarte **Disk Storage** .

### <a name="creation-of-the-storage-pool"></a>Erstellung des Speicherpools
In Azure werden die folgenden Einstellungen verwendet, um den Speicherpool auf SQL Server-VMs zu erstellen:

| Einstellung | Wert |
| --- | --- |
| Stripegröße |256 KB (Data Warehousing), 64 KB (Transaktional) |
| Datenträgergrößen |1 TB jeder |
| Cache |Lesen |
| Zuordnungsgröße |Größe der NTFS-Zuordnungseinheit: 64 KB |
| Sofortige Dateiinitialisierung |Aktiviert |
| Sperren von Seiten im Speicher |Aktiviert |
| Wiederherstellen |Einfache Wiederherstellung (keine Resilienz) |
| Anzahl von Spalten |Anzahl von Datenträgern<sup>1</sup> |
| Speicherort von TempDB |Auf Datenträgern gespeichert<sup>2</sup> |

<sup>1</sup> Nach der Erstellung des Speicherpools können Sie die Anzahl von Spalten im Speicherpool nicht mehr ändern.

<sup>2</sup> Diese Einstellung gilt nur für das erste Laufwerk, das Sie mit dem Feature für die Speicherkonfiguration erstellen.

## <a name="workload-optimization-settings"></a>Einstellungen für die Workloadoptimierung
In der folgenden Tabelle sind die drei verfügbaren Optionen für den Workloadtyp und die entsprechenden Optimierungen beschrieben:

| Workloadtyp | Beschreibung | Optimierungen |
| --- | --- | --- |
| **Allgemein** |Standardeinstellung, die die meisten Workloads unterstützt |Keine |
| **Transaktionale Verarbeitung** |Optimiert den Speicher für herkömmliche OLTP-Datenbankworkloads |Ablaufverfolgungsflag 1117<br/>Ablaufverfolgungsflag 1118 |
| **Data Warehousing** |Optimiert den Speicher für Analyse- und Berichterstellungsworkloads |Ablaufverfolgungsflag 610<br/>Ablaufverfolgungsflag 1117 |

> [!NOTE]
> Beim Bereitstellen eines virtuellen SQL-Computers können Sie den Workloadtyp nur angeben, indem Sie ihn im Schritt für die Speicherkonfiguration auswählen.
>
>

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Ausführen von SQL Server auf virtuellen Azure-Computern finden Sie unter [SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).



<!--HONumber=Jan17_HO2-->


