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
ms.sourcegitcommit: f9003c65d1818952c6a019f81080d595791f63bf
ms.openlocfilehash: 1248aeeaa159789b008eb003c2cd7babe0432919
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---

# <a name="use-a-custom-image-to-create-a-pool"></a>Verwenden eines benutzerdefinierten Images zum Erstellen eines Pools

Wenn Sie einen Pool in Azure Batch erstellen, geben Sie das Image eines virtuellen Computers (VM) an, das die Konfiguration des Betriebssystems für jeden Computeknoten im Pool bereitstellt. Sie können einen Pool entweder unter Verwendung eines Azure Marketplace-Images oder durch Bereitstellen eines benutzerdefinierten Images, das Sie vorbereitet haben, erstellen. Wenn Sie ein benutzerdefiniertes Image bereitstellen, können Sie steuern, wie das Betriebssystem zu dem Zeitpunkt konfiguriert ist, zu dem die einzelnen Computeknoten bereitgestellt werden. Ihr benutzerdefiniertes Image kann auch Anwendungen und Verweisdaten enthalten, die auf dem Computeknoten verfügbar sind, sobald er bereitgestellt wird.

Durch die Verwendung eines benutzerdefinierten Images sparen Sie Zeit beim Vorbereiten der Computekonten des Pools für die Ausführung Ihrer Batch-Workload. Sie können immer ein Azure Marketplace-Image verwenden und Software auf jedem Computeknoten installieren, nachdem er bereitgestellt wurde. Dieser Ansatz ist aber möglicherweise weniger effizient als die Verwendung eines benutzerdefinierten Images. Wenn Sie große Anwendungen installieren, große Datenmengen kopieren oder die VM während des Setupvorgangs neu starten müssen, sollten Sie die Verwendung eines benutzerdefinierten Images in Betracht ziehen, das für Ihre Anforderungen konfiguriert ist.  

## <a name="prerequisites"></a>Voraussetzungen

- **Ein Batch-Konto, das mit dem Poolzuordnungsmodus „Benutzerabonnement“ erstellt wurde.** Wenn Sie VM-Pools auf der Grundlage eines benutzerdefinierten Images bereitstellen möchten, erstellen Sie Ihr Batch-Konto mit dem [Poolzuordnungsmodus](batch-api-basics.md#pool-allocation-mode) „Benutzerabonnement“. In diesem Modus werden Batch-Pools dem Abonnement zugeordnet, in dem sich das Konto befindet. Im Abschnitt [Konto](batch-api-basics.md#account) unter [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](batch-api-basics.md) finden Sie Informationen zum Festlegen des Poolzuordnungsmodus beim Erstellen eines Batch-Kontos.

- Ein **Azure Storage-Konto.** Wenn Sie einen Pool mit der Konfiguration „Virtueller Computer“ mithilfe eines benutzerdefinierten Images erstellen möchten, benötigen Sie Azure Storage-Standardkonten, um Ihre benutzerdefinierten VHD-Images zu speichern. Benutzerdefinierte Images werden als Blobs gespeichert. Wenn Sie beim Erstellen eines Pools auf Ihre benutzerdefinierten Images verweisen möchten, geben Sie in der [osDisk](https://docs.microsoft.com/rest/api/batchservice/add-a-pool-to-an-account#bk_osdisk)-Eigenschaft der [virtualMachineConfiguration](https://docs.microsoft.com/rest/api/batchservice/add-a-pool-to-an-account#bk_vmconf)-Eigenschaft die URIs der Blobs mit den benutzerdefinierten Image-VHDs an.

    Stellen Sie sicher, dass Ihre Speicherkonten die folgenden Kriterien erfüllen:   
    
    - Die Speicherkonten mit den Blobs mit den benutzerdefinierten Image-VHDs müssen demselben Abonnement wie das Batch-Konto (das Benutzerabonnement) angehören.
    - Die angegebenen Speicherkonten müssen sich in derselben Region wie das Batch-Konto befinden.
    - Zurzeit werden nur allgemeine Storage Standard-Konten unterstützt. Azure Storage Premium wird in der Zukunft unterstützt.
    - Sie können ein Speicherkonto mit mehreren Blobs mit benutzerdefinierten VHDs oder mehrere Speicherkonten mit jeweils einem einzelnen Blob angeben. Es wird empfohlen, mehrere Speicherkonten zu verwenden, um eine bessere Leistung zu erzielen.
    - Ein eindeutiges Blob für benutzerdefinierte Images unterstützt bis zu 40 Linux-VM-Instanzen oder 20 Windows-VM-Instanzen. Sie können Kopien des VHD-Blobs erstellen, um Pools mit mehr VMs zu erstellen. Ein Pool mit 200 virtuellen Windows-Computern benötigt beispielsweise 10 eindeutige VHD-Blobs, die in der **osDisk**-Eigenschaft angegeben werden.
    
## <a name="prepare-a-custom-image"></a>Vorbereiten eines benutzerdefinierten Images

Um ein benutzerdefiniertes Image für die Verwendung mit Batch vorzubereiten, müssen Sie eine vorhandene Installation von Linux oder Windows generalisieren. Beim Generalisieren einer Betriebssysteminstallation werden computerspezifische Informationen entfernt. Das Ergebnis ist ein Image, das auf anderen Computern oder VMs installiert werden kann.  

Sie können ein Azure Marketplace-Image als Basisimage für Ihr benutzerdefiniertes Image verwenden. Erstellen Sie zum Anpassen des Basisimages eine Azure-VM, und fügen Sie ihr Ihre Anwendungen oder Daten hinzu. Generalisieren Sie dann die VM, die als Ihr benutzerdefiniertes Image dienen soll.   

Informationen zum Vorbereiten benutzerdefinierter Linux-Images von Azure-VMs finden Sie unter [Generalisieren und Erfassen eines virtuellen Linux-Computers](../virtual-machines/linux/capture-image.md). 

Informationen zum Vorbereiten benutzerdefinierter Windows-Images von Azure-VMs finden Sie unter [Erstellen eines benutzerdefinierten Images eines virtuellen Azure-Computers mithilfe von PowerShell](../virtual-machines/windows/tutorial-custom-images.md) und [Übersicht über die Systemvorbereitung (Sysprep)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview). 

> [!IMPORTANT]
> Beachten Sie bei der Vorbereitung Ihres benutzerdefinierten Images Folgendes:
> - Vergewissern Sie sich, dass das Betriebssystem-Basisimage, das Sie zum Bereitstellen Ihrer Batch-Pools verwenden, über keine vorinstallierten Azure-Erweiterungen (beispielsweise die benutzerdefinierte Skripterweiterung) verfügt. Wenn das Image eine vorinstallierte Erweiterung enthält, treten beim Bereitstellen des virtuellen Computers unter Umständen Probleme auf.
> - Vergewissern Sie sich, dass das von Ihnen bereitgestellte Betriebssystem-Basisimage das standardmäßige temporäre Laufwerk verwendet, da dies aktuell vom Batch-Knoten-Agent erwartet wird.
>
>
    
## <a name="create-a-pool-from-a-custom-image-in-the-portal"></a>Erstellen eines Pools über ein benutzerdefiniertes Image im Portal

So erstellen Sie ein benutzerdefiniertes Image mithilfe des Azure-Portals

1. Navigieren Sie im Azure-Portal zu Ihrem Batch-Konto.
2. Wählen Sie auf dem Blatt **Einstellungen** die Menüoption **Pools** aus.
3. Wählen Sie auf dem Blatt **Pools** den Befehl **Hinzufügen** aus. Daraufhin wird das Blatt **Pool hinzufügen** angezeigt.
4. Wählen Sie in der Dropdownliste **Imagetyp** die Option **Benutzerdefiniertes Image (Linux/Windows)** aus. Das Portal zeigt die Auswahl **Benutzerdefiniertes Image** an. Wählen Sie eine oder mehrere VHDs aus demselben Container aus, und klicken Sie auf die Schaltfläche **Auswählen**. 
   Die Unterstützung für die Auswahl von VHDs aus verschiedenen Speicherkonten und unterschiedlichen Containern wird in einem künftigen Release verfügbar sein.
5. Wählen Sie die richtigen Werte für **Verleger/Angebot/SKU** für Ihre benutzerdefinierten VHDs und den gewünschten **Zwischenspeicherungsmodus** aus, und füllen Sie alle anderen Parameter für den Pool aus.
6. Um festzustellen, ob ein Pool auf einem benutzerdefinierten Image basiert, überprüfen Sie die **Betriebssystem**-Eigenschaft im Abschnitt mit der Ressourcenzusammenfassung auf dem Blatt **Pool**. Der Wert dieser Eigenschaft sollte **Benutzerdefiniertes VM-Image** lauten.
7. Alle benutzerdefinierten VHDs, die einem Pool zugeordnet sind, werden auf dem Blatt **Eigenschaften** des Pools angezeigt.
 
## <a name="next-steps"></a>Nächste Schritte

- Eine detaillierte Übersicht über Batch finden Sie unter [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](batch-api-basics.md).
- Informationen zum Erstellen eines Batch-Kontos finden Sie unter [Erstellen eines Batch-Kontos mit dem Azure-Portal](batch-account-create-portal.md).
