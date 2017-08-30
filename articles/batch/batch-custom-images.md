---
title: "Bereitstellen von Azure Batch-Pools über benutzerdefinierte Images | Microsoft-Dokumentation"
description: "Sie können einen Batch-Pool über ein benutzerdefiniertes Image erstellen, um Serverknoten bereitzustellen, die die Software und die Daten enthalten, die Sie für Ihre Anwendung benötigen. Benutzerdefinierte Images sind eine effiziente Möglichkeit zum Konfigurieren von Computeknoten, um Ihre Batch-Workloads auszuführen."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 08/07/2017
ms.author: tamram
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 3d655766b4f2a5efb0c8c29ffa81a89f84b3e17c
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="use-a-custom-image-to-create-a-pool-of-virtual-machines"></a>Verwenden eines benutzerdefinierten Images zum Erstellen eines VM-Pools

Wenn Sie einen VM-Pool in Azure Batch erstellen, geben Sie das Image eines virtuellen Computers (VM) an, das das Betriebssystem für jeden Computeknoten im Pool bereitstellt. Sie können einen VM-Pool entweder unter Verwendung eines Azure Marketplace-Images oder durch Bereitstellen eines benutzerdefinierten, von Ihnen vorbereiteten VHD-Images erstellen. Wenn Sie ein benutzerdefiniertes Image bereitstellen, können Sie steuern, wie das Betriebssystem zu dem Zeitpunkt konfiguriert ist, zu dem die einzelnen Computeknoten bereitgestellt werden. Ihr benutzerdefiniertes Image kann auch Anwendungen und Verweisdaten enthalten, die auf dem Computeknoten verfügbar sind, sobald er bereitgestellt wird.

Durch die Verwendung eines benutzerdefinierten Images sparen Sie Zeit beim Vorbereiten der Computekonten des Pools für die Ausführung Ihrer Batch-Workload. Sie können immer ein Azure Marketplace-Image verwenden und Software auf jedem Computeknoten installieren, nachdem er bereitgestellt wurde. Dieser Ansatz ist aber möglicherweise weniger effizient als die Verwendung eines benutzerdefinierten Images. 

Zu den Gründen, die für die Verwendung eines benutzerdefinierten Images sprechen, das für Ihr Szenario konfiguriert ist, zählen folgende Notwendigkeiten:

- **Konfigurieren des Betriebssystems (BS)**: Jede spezielle Konfiguration des Betriebssystems kann auf dem benutzerdefinierten Image ausgeführt werden. 
- **Installieren umfangreicher Anwendungen.** Das Installieren von Anwendungen auf einem benutzerdefinierten Image ist effizienter als deren Installation auf jedem Computeknoten nach dessen Bereitstellung.
- **Kopieren großer Datenmengen.** Wenn die Daten auf das benutzerdefinierte Image kopiert werden, müssen sie nur einmal kopiert werden, anstatt auf die einzelnen Computeknoten – dies spart Zeit und Bandbreite.
- **Starten Sie den virtuellen Computer während des Setupvorgangs neu.** Das Neustarten des virtuellen Computers kann sehr zeitaufwändig sein, insbesondere, wenn Sie über zahlreiche Computeknoten verfügen.

## <a name="prerequisites"></a>Voraussetzungen

- **Ein Batch-Konto, das mit dem Poolzuordnungsmodus „Benutzerabonnement“ erstellt wurde.** Wenn Sie VM-Pools auf der Grundlage eines benutzerdefinierten Images bereitstellen möchten, erstellen Sie Ihr Batch-Konto mit dem [Poolzuordnungsmodus](batch-api-basics.md#pool-allocation-mode) „Benutzerabonnement“. In diesem Modus werden Batch-Pools dem Abonnement zugeordnet, in dem sich das Konto befindet. Im Abschnitt [Konto](batch-api-basics.md#account) unter [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](batch-api-basics.md) finden Sie Informationen zum Festlegen des Poolzuordnungsmodus beim Erstellen eines Batch-Kontos.

- Ein **Azure Storage-Konto.** Um einen VM-Pool mithilfe eines benutzerdefinierten Images zu erstellen, benötigen Sie ein standardmäßiges, allgemeines Azure Storage-Konto im gleichen Abonnement und in der gleichen Region. Wenn Sie ein benutzerdefiniertes Image aus einer Azure-VM erstellen und das Image in das Speicherkonto kopieren, auf dem sich die VM-Betriebssystemdatenträger befinden, dann müssen Sie kein separates Speicherkonto erstellen. 
    
## <a name="prepare-a-custom-image"></a>Vorbereiten eines benutzerdefinierten Images

Um ein benutzerdefiniertes Image für die Verwendung mit Batch vorzubereiten, müssen Sie eine vorhandene Installation von Linux oder Windows generalisieren. Beim Generalisieren einer Betriebssysteminstallation werden computerspezifische Informationen entfernt. Das Ergebnis ist ein Image, das auf anderen Computern oder VMs installiert werden kann.  

> [!IMPORTANT]
> Batch unterstützt die Bereitstellung eines Pools mithilfe von Azure verwalteter Images derzeit nicht. Das benutzerdefinierte Image, das Sie zum Bereitstellen eines Pools verwenden, muss in Azure Storage gespeichert werden. 
>
> Beachten Sie bei der Vorbereitung Ihres benutzerdefinierten Images folgende Punkte:
> - Vergewissern Sie sich, dass das Betriebssystem-Basisimage, das Sie zum Bereitstellen Ihrer Batch-Pools verwenden, über keine vorinstallierten Azure-Erweiterungen (beispielsweise die benutzerdefinierte Skripterweiterung) verfügt. Wenn das Image eine vorinstallierte Erweiterung enthält, treten beim Bereitstellen des virtuellen Computers unter Umständen Probleme auf.
> - Vergewissern Sie sich, dass das von Ihnen bereitgestellte Betriebssystem-Basisimage das standardmäßige temporäre Laufwerk verwendet, da dies aktuell vom Batch-Knoten-Agent erwartet wird.
>
>

Sie können jedes vorhandene vorbereitete Windows- oder Linux-Image als benutzerdefiniertes Image verwenden. Wenn Sie z.B. ein lokales Image verwenden möchten, dann laden Sie das Image mit [AzCopy](../storage/storage-use-azcopy.md) oder einem anderen Uploadtool in ein Azure Storage-Konto hoch, das sich in dem gleichen Abonnement und der gleichen Region wie das Batch-Konto befindet.

Sie können auch ein benutzerdefiniertes Image auf Basis einer neuen oder vorhandenen Azure-VM vorbereiten. Wenn Sie eine neue VM erstellen, können Sie ein Azure Marketplace-Image als Basisimage für Ihr benutzerdefiniertes Image verwenden und dann anpassen. Erstellen Sie zum Anpassen des Basisimages eine Azure-VM, und fügen Sie ihr Ihre Anwendungen oder Daten hinzu. Generalisieren Sie dann die VM, die als Ihr benutzerdefiniertes Image dienen soll, und speichern Sie sie in Azure Storage. 

Um ein benutzerdefiniertes Image auf Basis einer Azure-VM vorzubereiten, gehen Sie folgendermaßen vor:

1. Erstellen Sie einen **nicht verwalteten** virtuellen Azure-Computer aus einem Azure Marketplace-Image. Azure Marketplace enthält sowohl Images für [Windows](../virtual-machines/windows/quick-create-portal.md) als auch für [Linux](../virtual-machines/linux/quick-create-portal.md).
    
    Wählen Sie in Schritt 3 des Erstellungsprozesses des virtuellen Computers unbedingt **Nein** für die Option **Speicher: Verwaltete Datenträger verwenden**. Notieren Sie auch den Speicherkontonamen für die VM-Betriebssystemdatenträger, da Azure in diesem Speicherkonto auch Ihr benutzerdefiniertes Image speichert:

    ![Erstellen eines nicht verwalteten virtuellen Computers und Notieren des Speicherkontonamens](media/batch-custom-images/vm-create-storage.png)
 
2. Schließen Sie das Erstellen Ihres virtuellen Computers ab, und warten Sie, bis er von Azure zugeordnet wird. Dieses Bild zeigt einen virtuellen Computer im Azure-Portal im Ausführungsstatus:

    ![Erstellen eines virtuellen Computers aus einem Marketplace-Image](media/batch-custom-images/vm-status-running.png)

3. Sobald der virtuelle Computer ausgeführt wird, stellen Sie über RDP (für Windows) oder SSH (für Linux) eine Verbindung mit ihm her. Installieren Sie erforderliche Software, oder kopieren Sie die gewünschten Daten, und generalisieren Sie dann den virtuellen Computer. Führen Sie die in [Generalize the VM](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-copy-generalized.md#generalize-the-vm) (Generalisieren der VM) beschriebenen Schritte aus. 
   
4. Führen Sie die Schritte zum [Anmelden an Azure PowerShell](../virtual-machines/windows/sa-copy-generalized.md#log-in-to-azure-powershell) aus. Wie Sie Azure PowerShell installieren, erfahren Sie unter [Overview of Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.2.0) (Übersicht zu Azure PowerShell). 

5. Führen Sie als Nächstes die Schritte zum [Aufheben der VM-Zuordnung und Festlegen des Status auf „Generalisiert“](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-copy-generalized#deallocate-the-vm-and-set-the-state-to-generalized) aus. 

    Beachten Sie im Azure-Portal, dass die Zuordnung des virtuellen Computers aufgehoben wird:

    ![Sicherstellen, dass die Zuordnung des virtuellen Computers aufgehoben wird](media/batch-custom-images/vm-status-deallocated.png)

6.  Erstellen Sie das VM-Image, und speichern Sie es mithilfe des PowerShell-Cmdlets [Save-AzureRmVMImage](https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvmimage) in Azure Storage. Befolgen Sie die in [Erstellen des Image](../virtual-machines/windows/sa-copy-generalized.md#create-the-image) beschriebenen Anweisungen.
    
    Das VM-Image wird auf dem Azure Storage-Konto gespeichert, wenn der virtuelle Computer wie in Schritt 1 dieses Verfahrens dargestellt erstellt wurde. Das Save-AzureRmVMImage-Cmdlet speichert das Image auf diesem Speicherkonto im Container **system**. Der `-DestinationContainername`-Parameter benennt ein virtuelles Verzeichnis innerhalb des Containers **system**. Der `-VHDNamePrefix`-Parameter gibt ein Präfix für den Blobnamen an. Dieses Präfix wird dem Blobnamen mit einem Bindestrich vorangestellt. 

    Nehmen Sie beispielsweise an, dass Sie Save-AzureRmVMImage mit den folgenden Parametern aufrufen:  

        Save-AzureRmVMImage -ResourceGroupName sample-resource-group -Name vm-custom-image -DestinationContainerName batchimages -VHDNamePrefix custom -Path C:\Temp\Images\vm-custom-image.json

    Das resultierende Image wird am folgenden Speicherort unter folgendem Blobnamen gespeichert:

    ![Speicherort der gespeicherten VHD im Container „system“](media/batch-custom-images/vhd-in-vm-storage-account.png)

    > [!NOTE]
    > Eine nicht verwaltete Azure-VM erstellt mehrere Speicherkonten für unterschiedliche Zwecke. Wenn Sie bei der Erstellung des virtuellen Computers nicht den Namen des Speichercontainers für den Betriebssystem-Datenträger notiert haben, suchen Sie das zugeordnete Speicherkonto, das den Container **system** enthält. Navigieren Sie im Container **system**, um das benutzerdefinierte Image mit den Werten zu finden, die Sie mit dem Befehl **Save-AzureRmVMImage** angegeben haben.

## <a name="create-a-pool-from-a-custom-image-in-the-portal"></a>Erstellen eines Pools über ein benutzerdefiniertes Image im Portal

Wenn Sie Ihr benutzerdefiniertes Image gespeichert haben und den Speicherort kennen, können Sie von diesem Image einen Batch-Pool erstellen. Führen Sie die folgenden Schritte aus, um einen Pool im Azure-Portal zu erstellen:

1. Navigieren Sie im Azure-Portal zu Ihrem Batch-Konto. Das Konto muss sich in dem gleichen Abonnement und der gleichen Region wie das Speicherkonto befinden, das das benutzerdefinierte Image enthält. 
2. Wählen Sie links im Fenster **Einstellungen** die Menüoption **Pools** aus.
3. Wählen Sie im Fenster **Pools** den Befehl **Hinzufügen** aus.
4. Wählen Sie im Fenster **Pool hinzufügen** die Option **Benutzerdefiniertes Image (Linux/Windows)** in der Dropdownliste **Imagetyp** aus. Das Portal zeigt die Auswahl **Benutzerdefiniertes Image** an. Navigieren Sie zu dem Speicherkonto, wo sich Ihr benutzerdefiniertes Image befindet, und wählen Sie die gewünschte VHD aus dem Container. 
5. Wählen Sie **Verleger/Angebot/SKU** für Ihre benutzerdefinierte VHD passend aus.
6. Geben Sie die übrigen erforderlichen Einstellungen an, einschließlich **Knotengröße**, **Ziel für dedizierte Knoten** und **Knoten mit niedriger Priorität**, sowie alle gewünschten optionalen Einstellungen.

    Für ein benutzerdefiniertes Image im Microsoft Windows Server Datacenter 2016 wird das Fenster **Pool hinzufügen** z.B. so angezeigt:

    ![Hinzufügen eines Pools aus dem benutzerdefinierten Windows-Image](media/batch-custom-images/add-pool-custom-image.png)
  
Um festzustellen, ob ein vorhandener Pool auf einem benutzerdefinierten Image basiert, überprüfen Sie die **Betriebssystem**-Eigenschaft im Abschnitt mit der Ressourcenzusammenfassung im Fenster **Pool**. Wenn der Pool aus einem benutzerdefinierten Image erstellt wurde, wird er auf **Benutzerdefiniertes VM-Image** festgelegt.

Alle benutzerdefinierten VHDs, die einem Pool zugeordnet sind, werden im Fenster **Eigenschaften** des Pools angezeigt.
 
## <a name="next-steps"></a>Nächste Schritte

- Eine detaillierte Übersicht über Batch finden Sie unter [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](batch-api-basics.md).
- Informationen zum Erstellen eines Batch-Kontos finden Sie unter [Erstellen eines Batch-Kontos mit dem Azure-Portal](batch-account-create-portal.md).
