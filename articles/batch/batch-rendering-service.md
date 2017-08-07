---
title: Rendern in der Cloud mithilfe des Azure Batch Rendering-Diensts | Microsoft-Dokumentation
description: "Rendern Sie Aufträge auf virtuellen Azure-Computern direkt über Maya – mit einem nutzungsbasierten Zahlungsmodell."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: hero-article
ms.date: 07/31/2017
ms.author: tamram
ms.translationtype: HT
ms.sourcegitcommit: 7bf5d568e59ead343ff2c976b310de79a998673b
ms.openlocfilehash: 4d22f92cafdbceee5213361d6d2b2f38904d12c6
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---

# <a name="get-started-with-the-batch-rendering-service"></a>Erste Schritte mit dem Batch Rendering-Dienst

Der Azure Batch Rendering-Dienst bietet cloudbasierte Renderfunktionen und ein nutzungsbasiertes Zahlungsmodell. Er kümmert sich um die Auftragsplanung und um Auftragswarteschlangen, um Fehler und Wiederholungen sowie um die automatische Skalierung für Ihren Renderauftrag. Der Batch Rendering Dienst unterstützt Autodesk Maya, 3ds Max und Arnold. Unterstützung für weitere Anwendungen folgt in Kürze. Mit dem Batch-Plug-In für Maya 2017 können Sie einen Renderauftrag in Azure ganz einfach direkt über Ihren Desktop starten. 

## <a name="supported-applications"></a>Unterstützte Anwendungen

Der Batch Rendering-Dienst unterstützt derzeit folgende Anwendungen:

- Autodesk Maya
- Autodesk 3ds Max
- Autodesk Arnold

## <a name="prerequisites"></a>Voraussetzungen

Für die Verwendung des Batch Rendering-Diensts benötigen Sie Folgendes:

- Ein [Azure-Konto](https://azure.microsoft.com/free/). 
- Ein **Azure Batch-Konto.** Eine Anleitung zum Erstellen eines Batch-Kontos im Azure-Portal finden Sie unter [Erstellen eines Batch-Kontos mit dem Azure-Portal](batch-account-create-portal.md).
- Ein **Azure Storage-Konto.** Die für Ihren Renderauftrag verwendeten Ressourcen werden in Azure Storage gespeichert. Beim Einrichten Ihres Batch-Kontos können Sie automatisch ein Speicherkonto erstellen. Sie können aber auch ein bereits vorhandenes Speicherkonto verwenden. Weitere Informationen zu Speicherkonten finden Sie unter [Informationen zu Azure-Speicherkonten](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

Für die Verwendung des Batch-Plug-Ins für Maya benötigen Sie Folgendes:

- **Maya 2017**
- **Arnold for Maya**

Über das [Azure-Portal](https://portal.azure.com) können Sie auch Pools mit virtuellen Computern erstellen, die mit Maya, 3ds Max und Arnold vorkonfiguriert sind. Sie können über das Portal Aufträge überwachen und nicht erfolgreiche Aufgaben diagnostizieren, indem Sie Anwendungsprotokolle herunterladen und mit einzelnen virtuellen Computern eine Remoteverbindung per RDP oder SSH herstellen.

## <a name="basic-batch-concepts"></a>Grundlegende Batch-Konzepte

Vor der Verwendung des Batch Rendering-Diensts sollten Sie sich mit einigen Batch-Konzepten wie Computeknoten, Pools und Aufträgen vertraut machen. Allgemeine Informationen zu Azure Batch finden Sie unter [Ausführen intrinsisch paralleler Workloads mit Batch](batch-technical-overview.md).

### <a name="pools"></a>Pools

Batch ist ein Plattformdienst zur Durchführung rechenintensiver Aufgaben (beispielsweise Rendering) mit einem **Pool** von **Computeknoten**. Bei den einzelnen Computeknoten in einem Pool handelt es sich jeweils um einen virtuellen Azure-Computer (Virtual Machine, VM) unter Linux oder Windows. 

Weitere Informationen zu Batch-Pools und Computeknoten finden Sie unter [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](batch-api-basics.md) in den Abschnitten [Pool](batch-api-basics.md#pool) und [Computeknoten](batch-api-basics.md#compute-node).

### <a name="jobs"></a>Aufträge

Ein **Auftrag** in Batch ist eine Sammlung von Aufgaben, die auf den Computeknoten in einem Pool ausgeführt werden. Wenn Sie einen Renderauftrag übermitteln, unterteilt Batch den Auftrag in Aufgaben und verteilt diese zur Ausführung auf die Computeknoten des Pools.

Weitere Informationen zu Batch-Aufträgen finden Sie unter [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](batch-api-basics.md) im Abschnitt [Auftrag](batch-api-basics.md#job).

## <a name="use-the-batch-plug-in-for-maya-to-submit-a-render-job"></a>Übermitteln eines Renderauftrags mithilfe des Batch-Plug-Ins für Maya

Mit dem Batch-Plug-In für Maya können Sie direkt in Maya einen Auftrag an den Batch Rendering-Dienst übermitteln. In den folgenden Abschnitten erfahren Sie, wie Sie den Auftrag über das Plug-In konfigurieren und anschließend übermitteln. 

### <a name="load-the-batch-plug-in-in-maya"></a>Laden des Batch-Plug-Ins in Maya

Das Batch-Plug-In steht auf [GitHub](https://github.com/Azure/azure-batch-maya/releases) zur Verfügung. Entzippen Sie das Archiv in einem Verzeichnis Ihrer Wahl. Das Plug-In kann direkt aus dem Verzeichnis *azure_batch_maya* geladen werden.

So laden Sie das Plug-In in Maya:

1. Führen Sie Maya aus.
2. Öffnen Sie **Window** > **Settings/Preferences** > **Plug-in Manager** (Fenster > Einstellungen/Voreinstellungen > Plug-In-Manager).
3. Klicken Sie auf **Durchsuchen**.
4. Navigieren Sie zu *azure_batch_maya/plug-in/AzureBatch.py*, und wählen Sie es aus.

### <a name="authenticate-access-to-your-batch-and-storage-accounts"></a>Authentifizieren des Zugriffs auf Ihr Batch- und Ihr Storage-Konto

Um das Plug-In verwenden zu können, müssen Sie sich mit Ihrem Azure Batch- und Ihrem Azure Storage-Kontoschlüssel authentifizieren. So rufen Sie Ihre Kontoschlüssel ab:

1. Zeigen Sie das Plug-In in Maya an, und wählen Sie die Registerkarte **Config** (Konfiguration) aus.
2. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
3. Wählen Sie im linken Menü die Option **Batch-Konten** aus. (Unter Umständen müssen Sie auf **Weitere Dienste** klicken und nach _Batch_ filtern.)
4. Suchen Sie in der Liste nach dem gewünschten Batch-Konto.
5. Wählen Sie das Menüelement **Schlüssel** aus, um Ihren Kontonamen, Ihre Konto-URL und Ihre Zugriffsschlüssel anzuzeigen:
    - Fügen Sie im Batch-Plug-In die Batch-Konto-URL in das Feld **Service** ein.
    - Fügen Sie den Kontonamen in das Feld **Batch Account** (Batch-Konto) ein.
    - Fügen Sie den primären Kontoschlüssel in das Feld **Batch Key** (Batch-Schlüssel) ein.
7. Wählen Sie im linken Menü die Option „Speicherkonten“ aus. (Unter Umständen müssen Sie auf **Weitere Dienste** klicken und nach _Speicher_ filtern.)
8. Suchen Sie in der Liste nach dem gewünschten Speicherkonto.
9. Wählen Sie das Menüelement **Zugriffsschlüssel** aus, um den Namen und die Schlüssel des Speicherkontos anzuzeigen.
    - Fügen Sie im Batch-Plug-In den Namen des Storage-Kontos in das Feld **Storage Account** (Speicherkonto) ein.
    - Fügen Sie den primären Kontoschlüssel in das Feld **Storage Key** (Speicherschlüssel) ein.
10. Vergewissern Sie sich durch Klicken auf **Authenticate** (Authentifizieren), dass das Plug-In auf beide Konten zugreifen kann.

Nach erfolgreicher Authentifizierung wird im Statusfeld des Plug-Ins der Status **Authenticated** (Authentifiziert) angezeigt: 

![Authentifizieren Ihres Batch- und Storage-Kontos](./media/batch-rendering-service/authentication.png)

### <a name="configure-a-pool-for-a-render-job"></a>Konfigurieren eines Pools für einen Renderauftrag

Nach der Authentifizierung Ihres Batch- und Storage-Kontos können Sie einen Pool für Ihren Renderauftrag einrichten. Ihre Auswahl wird vom Plug-In sitzungsübergreifend gespeichert. Dadurch müssen Sie Ihre bevorzugte Konfiguration nur einmal einrichten und dann nur noch bearbeiten, wenn sich etwas ändert.

In den folgenden Abschnitten werden die verfügbaren Optionen der Registerkarte **Submit** (Übermitteln) beschrieben:

#### <a name="specify-a-new-or-existing-pool"></a>Angeben eines neuen oder vorhandenen Pools

Wählen Sie zum Angeben eines Pools für die Ausführung des Renderauftrags die Registerkarte **Submit** (Übermitteln) aus. Diese Registerkarte bietet Optionen zum Erstellen eines Pools bzw. zum Auswählen eines bereits vorhandenen Pools:

- Bei Verwendung der Standardoption **Auto provision a pool for this job** wird automatisch ein Pool für den Auftrag bereitgestellt. Batch erstellt den Pool in diesem Fall exklusiv für den aktuellen Auftrag und löscht ihn nach Abschluss des Renderauftrags wieder. Diese Option empfiehlt sich, wenn Sie einen einzelnen Renderauftrag ausführen möchten.
- Mit der Option **Reuse an existing persistent pool** können Sie einen vorhandenen persistenten Pool wiederverwenden. Wenn Sie bereits über einen Pool verfügen und er sich im Leerlauf befindet, können Sie ihn in der Dropdownliste auswählen, um ihn für die Ausführung des Renderauftrags anzugeben. Die Wiederverwendung eines bereits vorhandenen persistenten Pools spart Zeit, die ansonsten für die Bereitstellung des Pools aufgewendet werden müsste.  
- Mit der Option **Create a new persistent pool** können Sie einen neuen persistenten Pool erstellen. Bei Verwendung dieser Option wird ein neuer Pool für die Auftragsausführung erstellt. Der Pool wird nach Abschluss des Auftrags nicht wieder gelöscht und kann für weitere Aufträge wiederverwendet werden. Wählen Sie diese Option aus, wenn Sie häufig Renderaufträge ausführen. Bei weiteren Aufträgen können Sie **Reuse an existing persistent pool** (Vorhandenen persistenten Pool wiederverwenden) auswählen, um den persistenten Pool zu verwenden, den Sie für den ersten Auftrag erstellt haben.

![Angeben von Pool, Betriebssystemimage, Größe des virtuellen Computers und Lizenz](./media/batch-rendering-service/submit.png)

Weitere Informationen zu den Kosten virtueller Azure-Computer finden Sie in den [häufig gestellten Fragen zu Preisen unter Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#faq) sowie in den [häufig gestellten Fragen zu Preisen unter Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/#faq).

#### <a name="specify-the-os-image-to-provision"></a>Angeben des bereitzustellenden Betriebssystemimages

Auf der Registerkarte **Env** (Umgebung) können Sie angeben, welche Art von Betriebssystemimage bei der Bereitstellung von Computeknoten im Pool verwendet werden soll. Batch unterstützt derzeit folgende Imageoptionen für Renderaufträge:

|Betriebssystem  |Image  |
|---------|---------|
|Linux     |Batch CentOS Preview |
|Windows     |Batch Windows Preview |

#### <a name="choose-a-vm-size"></a>Auswählen einer VM-Größe

Auf der Registerkarte **Env** (Umgebung) können Sie die Größe des virtuellen Computers angeben. Weitere Informationen zu verfügbaren VM-Größen finden Sie unter [Größen für virtuelle Linux-Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) sowie unter [Größen für virtuelle Windows-Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes). 

![Angeben von Betriebssystemimage und Größe für virtuelle Computer auf der Registerkarte „Env“ (Umgebung)](./media/batch-rendering-service/environment.png)

#### <a name="specify-licensing-options"></a>Angeben von Lizenzoptionen

Auf der Registerkarte **Env** (Umgebung) können Sie die zu verwendenden Lizenzen angeben. Beispiele für Optionen:

- **Maya** (standardmäßig aktiviert)
- **Arnold** (aktiviert, wenn Arnold als aktives Renderingmodul in Maya erkannt wird)

 Wenn Sie beim Rendern Ihre eigene Lizenz verwenden möchten, können Sie Ihren Lizenzendpunkt konfigurieren, indem Sie der Tabelle die entsprechenden Umgebungsvariablen hinzufügen. In diesem Fall müssen allerdings die Standardlizenzoptionen deaktiviert werden.

> [!IMPORTANT]
> Die Abrechnung für die Lizenzverwendung erfolgt auf der Grundlage der ausgeführten virtuellen Computer im Pool, auch wenn diese gerade nicht zum Rendern verwendet werden. Legen Sie daher die Größe des Pools auf der Registerkarte **Pools** bis zur Ausführung eines weiteren Renderauftrags auf null Knoten fest, um unnötige Kosten zu vermeiden. 
>
>

#### <a name="manage-persistent-pools"></a>Verwalten persistenter Pools

Vorhandene persistente Pools können auf der Registerkarte **Pools** verwaltet werden. Wenn Sie in der Liste einen Pool auswählen, wird der aktuelle Status des Pools angezeigt.

Darüber hinaus können Sie auf der Registerkarte **Pools** den Pool löschen und die Anzahl virtueller Computer im Pool anpassen. In Leerlaufphasen können Sie die Poolgröße auf null Knoten festlegen, um Kosten zu sparen.

![Anzeigen, Ändern der Größe und Löschen von Pools](./media/batch-rendering-service/pools.png)

### <a name="configure-a-render-job-for-submission"></a>Konfigurieren eines Renderauftrags für die Übermittlung

Nach Angabe der Parameter für den Pool, in dem der Renderauftrag ausgeführt wird, können Sie den eigentlichen Auftrag konfigurieren. 

#### <a name="specify-scene-parameters"></a>Angeben von Szenenparametern

Das Batch-Plug-In erkennt, welches Renderingmodul derzeit in Maya verwendet wird, und zeigt auf der Registerkarte **Submit** (Übermitteln) die entsprechenden Rendereinstellungen an. Diese Einstellungen umfassen Startframe, Endframe, Ausgabepräfix und Frameschritt. Die Rendereinstellungen der Szenendatei können durch Angabe anderer Einstellungen im Plug-In überschrieben werden. Geänderte Plug-In-Einstellungen werden nicht in den Rendereinstellungen der Szenendatei gespeichert. Dadurch können Sie auftragsspezifische Änderungen vornehmen, ohne die Szenendatei erneut hochladen zu müssen.

Sollte das in Maya ausgewählte Renderingmodul nicht unterstützt werden, erscheint eine entsprechende Warnung.

Wenn Sie eine neue Szene laden, während das Plug-In geöffnet ist, klicken Sie auf die Schaltfläche **Refresh** (Aktualisieren), um sicherzustellen, dass die Einstellungen aktualisiert werden.

#### <a name="resolve-asset-paths"></a>Auflösen von Ressourcenpfaden

Beim Laden des Plug-Ins wird die Szenendatei nach externen Dateiverweisen durchsucht. Diese Verweise werden auf der Registerkarte **Assets** (Ressourcen) angezeigt. Falls ein Pfad nicht aufgelöst werden kann, werden unter anderem folgende Standardspeicherorte nach der Datei durchsucht:

- Der Speicherort der Szenendatei 
- Das Verzeichnis _sourceimages_ des aktuellen Projekts
- Das aktuelle Arbeitsverzeichnis 

Falls die Ressource trotzdem nicht gefunden werden kann, wird sie mit einem Warnsymbol gekennzeichnet:

![Fehlende Ressourcen werden mit einem Warnsymbol gekennzeichnet.](./media/batch-rendering-service/missing_assets.png)

Falls Ihnen der Speicherort eines nicht aufgelösten Dateiverweises bekannt ist, können Sie auf das Warnsymbol klicken und einen Suchpfad angeben. Daraufhin versucht das Plug-In, alle fehlenden Ressourcen auf der Grundlage dieses Suchpfads aufzulösen. Sie können eine beliebige Anzahl von zusätzlichen Suchpfaden hinzufügen.

Aufgelöste Verweise werden mit einem grünen Ampelsymbol gekennzeichnet:

![Aufgelöste Ressourcen mit grünem Ampelsymbol](./media/batch-rendering-service/found_assets.png)

Falls für Ihre Szene weitere Dateien erforderlich sind, die das Plug-In nicht erkannt hat, können Sie zusätzliche Dateien oder Verzeichnisse hinzufügen. Verwenden Sie hierzu die Schaltflächen **Add Files** (Dateien hinzufügen) und **Add Directory** (Verzeichnis hinzufügen). Wenn Sie eine neue Szene laden, während das Plug-In geöffnet ist, klicken Sie auf **Refresh** (Aktualisieren), um die Szenenverweise zu aktualisieren.

#### <a name="upload-assets-to-an-asset-project"></a>Hochladen von Ressourcen in ein Ressourcenprojekt

Wenn Sie einen Renderauftrag übermitteln, werden die referenzierten Dateien, die auf der Registerkarte **Assets** (Ressourcen) angezeigt werden, automatisch als Ressourcenprojekt in Azure Storage hochgeladen. Die Ressourcendateien können auch unabhängig von einem Renderauftrag hochgeladen werden. Verwenden Sie hierzu auf der Registerkarte **Assets** (Ressourcen) die Schaltfläche **Upload** (Hochladen). Der Name des Ressourcenprojekts wird im Feld **Project** (Projekt) angegeben und entspricht standardmäßig dem Namen des aktuellen Maya-Projekts. Beim Hochladen von Ressourcendateien bleibt die lokale Datenstruktur erhalten. 

Auf die hochgeladenen Ressourcen kann in beliebig vielen Renderaufträgen verwiesen werden. Alle hochgeladenen Ressourcen stehen für jeden Auftrag zur Verfügung, der auf das Ressourcenprojekt verweist. Dabei spielt es keine Rolle, ob sie Teil der Szene sind. Wenn Sie das Ressourcenprojekt ändern möchten, auf das Ihr nächster Auftrag verweist, müssen Sie auf der Registerkarte **Assets** (Ressourcen) den Namen im Feld **Project** (Projekt) ändern. Wenn bestimmte referenzierte Dateien nicht hochgeladen werden sollen, heben Sie deren Auswahl mithilfe der grünen Schaltfläche neben dem jeweiligen Eintrag auf.

#### <a name="submit-and-monitor-the-render-job"></a>Übermitteln und Überwachen des Renderauftrags

Klicken Sie nach dem Konfigurieren des Renderauftrags im Plug-In auf der Registerkarte **Submit** (Übermitteln) auf die Schaltfläche **Submit Job** (Auftrag übermitteln), um den Auftrag an Batch zu übermitteln:

![Übermitteln des Renderauftrags](./media/batch-rendering-service/submit_job.png)

Aktive Aufträge können auf der Plug-In-Registerkarte **Jobs** (Aufträge) überwacht werden. Wählen Sie in der Liste einen Auftrag aus, um den aktuellen Status des Auftrags anzuzeigen. Auf dieser Registerkarte können Sie auch Aufträge abbrechen und löschen sowie die Ausgaben und Renderprotokolle herunterladen. 

Geben Sie zum Herunterladen von Ausgaben im Feld **Outputs** (Ausgaben) das gewünschte Zielverzeichnis an. Klicken Sie auf das Zahnradsymbol, um einen Hintergrundprozess zu starten, der den Auftrag überwacht und Ausgaben herunterlädt: 

![Anzeigen des Auftragsstatus und Herunterladen von Ausgaben](./media/batch-rendering-service/jobs.png)

Sie können Maya schließen, ohne den Downloadvorgang zu unterbrechen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Batch finden Sie unter [Ausführen intrinsisch paralleler Workloads mit Batch](batch-technical-overview.md).
