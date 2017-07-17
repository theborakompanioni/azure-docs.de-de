---
title: Migrieren zu Azure Storage Premium mit Azure Site Recovery | Microsoft-Dokumentation
description: "Migrieren Sie die vorhandenen virtuellen Computer mithilfe von Site Recovery zu Azure Storage Premium. Storage Premium bietet Datenträgerunterstützung für hohe Leistung mit geringer Latenz für E/A-intensive Workloads, die auf virtuellen Azure-Computern ausgeführt werden."
services: storage
cloud: Azure
documentationcenter: na
author: luywang
manager: kavithag
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2017
ms.author: luywang
ms.translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 522fd46e8c0ccc64eb97ee6622e9886bb51f1e24
ms.contentlocale: de-de
ms.lasthandoff: 04/15/2017

---
<a id="migrating-to-premium-storage-using-azure-site-recovery" class="xliff"></a>

# Migrieren zu Storage Premium mit Azure Site Recovery

[Azure Storage Premium](storage-premium-storage.md) bietet Datenträgerunterstützung mit hoher Leistung und geringer Wartezeit für virtuelle Computer (VMs), auf denen E/A-intensive Workloads ausgeführt werden. Dieser Leitfaden soll Benutzern beim Migrieren ihrer VM-Datenträger von einem Standard-Speicherkonto zu einem Storage Premium-Konto mithilfe von [Azure Site Recovery](../site-recovery/site-recovery-overview.md) als Hilfe dienen.

Site Recovery ist ein Azure-Dienst, der einen Beitrag zu Ihrer Strategie für die Geschäftskontinuität und Notfallwiederherstellung leistet, indem die Replikation von lokalen physischen Servern und VMs in die Cloud (Azure) oder in ein sekundäres Datencenter orchestriert wird. Wenn es an Ihrem primären Standort zu Ausfällen kommt, wird ein Failover zum sekundären Standort durchgeführt, um die Verfügbarkeit von Anwendungen und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, führen Sie das Failback zum primären Standort durch. Site Recovery verfügt über Testfailover, mit denen Übungen zur Notfallwiederherstellung unterstützt werden, ohne dass sich dies auf Produktionsumgebungen auswirkt. Sie können Failover mit minimalem Datenverlust (je nach Replikationshäufigkeit) für unerwartete Notfälle ausführen. Bei der Migration zu Storage Premium können Sie ein [Failover in Site Recovery](../site-recovery/site-recovery-failover.md) in Azure Site Recovery verwenden, um Zieldatenträger zu einem Storage Premium-Konto zu migrieren.

Wir empfehlen Ihnen die Migration zu Storage Premium mithilfe von Site Recovery, weil es bei dieser Option zu minimalen Ausfallzeiten kommt und das manuelle Kopieren von Datenträgern und Erstellen von neuen VMs entfällt. Mit Site Recovery werden Ihre Datenträger bei einem Failover systematisch kopiert und neue VMs erstellt. Site Recovery unterstützt verschiedene Failoverarten mit minimalen oder ganz ohne Ausfallzeiten. Die Informationen in der Tabelle mit den [Failoverarten](../site-recovery/site-recovery-failover.md) in Site Recovery sind hilfreich für das Planen der Ausfallzeiten und das Schätzen von Datenverlusten. Wenn Sie [die Vorbereitung zum Herstellen der Verbindung mit Azure-VMs nach dem Failover durchführen](../site-recovery/site-recovery-vmware-to-azure.md#prepare-vms-for-replication), sollten Sie dazu in der Lage sein, nach dem Failover per RDP eine Verbindung mit der Azure-VM herzustellen.

![][1]

<a id="azure-site-recovery-components" class="xliff"></a>

## Azure Site Recovery-Komponenten

Hierbei handelt es sich um die Site Recovery-Komponenten, die für dieses Migrationsszenario relevant sind.

* Ein **Konfigurationsserver** ist eine Azure-VM, mit der die Kommunikation koordiniert wird und die Datenreplikations- und Wiederherstellungsvorgänge verwaltet werden. Auf dieser VM führen Sie eine einzelne Setupdatei aus, um den Konfigurationsserver und eine zusätzliche Komponente, die als Prozessserver bezeichnet wird, als Replikationsgateway zu installieren. Informieren Sie sich über die [Voraussetzungen für den Konfigurationsserver](../site-recovery/site-recovery-vmware-to-azure.md#prerequisites). Der Konfigurationsserver muss nur einmal konfiguriert werden und kann für alle Migrationen zu derselben Region verwendet werden.

* Ein **Prozessserver** ist ein Replikationsgateway, das Replikationsdaten von Quell-VMs empfängt, die Daten per Zwischenspeicherung, Komprimierung und Verschlüsselung optimiert und sie dann an ein Speicherkonto sendet. Außerdem wickelt er die Pushinstallation des Mobilitätsdiensts auf Quell-VMs ab und führt die automatische Ermittlung von Quell-VMs durch. Auf dem Konfigurationsserver ist der Standardprozessserver installiert. Sie können zusätzliche eigenständige Prozessserver bereitstellen, um Ihre Bereitstellung zu skalieren. Informieren Sie sich über die [bewährten Methoden für die Bereitstellung von Prozessservern](https://azure.microsoft.com/blog/best-practices-for-process-server-deployment-when-protecting-vmware-and-physical-workloads-with-azure-site-recovery/) und die [Bereitstellung zusätzlicher Prozessserver](../site-recovery/site-recovery-plan-capacity-vmware.md#deploy-additional-process-servers). Der Prozessserver muss nur einmal konfiguriert werden und kann für alle Migrationen zu derselben Region verwendet werden.

* Ein **Mobilitätsdienst** ist eine Komponente, die auf jeder zu replizierenden Standard-VM bereitgestellt wird. Er erfasst die Datenschreibvorgänge auf der Standard-VM und sendet sie an den Prozessserver. Informieren Sie sich über die [Voraussetzungen für replizierte Computer](../site-recovery/site-recovery-vmware-to-azure.md#prerequisites).

Diese Grafik zeigt, wie diese Komponenten zusammenwirken.

![][15]

> [!NOTE]
> Site Recovery unterstützt die Migration von Speicherplätze-Datenträgern nicht.

Informationen zu weiteren Komponenten für andere Szenarien finden Sie unter [Szenarioarchitektur](../site-recovery/site-recovery-vmware-to-azure.md).

<a id="azure-essentials" class="xliff"></a>

## Wichtige Azure-Komponenten

Hierbei handelt es sich um die Azure-Anforderungen für dieses Migrationsszenario.

* Ein Azure-Abonnement
* Ein Azure Storage Premium-Konto zum Speichern von replizierten Daten
* Ein virtuelles Azure-Netzwerk (VNet), mit dem für VMs eine Verbindung hergestellt wird, wenn sie bei einem Failover erstellt werden. Das Azure VNet muss sich in derselben Region wie das VNet befinden, in dem Site Recovery ausgeführt wird.
* Ein Azure-Speicherkonto vom Typ „Standard“, in dem die Replikationsprotokolle gespeichert werden. Dies kann dasselbe Speicherkonto wie für die zu migrierenden VM-Datenträger sein.

<a id="prerequisites" class="xliff"></a>

## Voraussetzungen

* Kenntnis der relevanten Migrationsszenariokomponenten aus dem vorherigen Abschnitt
* Planung der Ausfallzeit basierend auf den Informationen zu [Failovern in Site Recovery](../site-recovery/site-recovery-failover.md)

<a id="setup-and-migration-steps" class="xliff"></a>

## Setup- und Migrationsschritte

Sie können Site Recovery verwenden, um Azure IaaS-VMs zwischen Regionen oder innerhalb derselben Region zu migrieren. Die folgende Anleitung wurde basierend auf dem Artikel [Replizieren von VMware-VMs oder physischen Servern in Azure](../site-recovery/site-recovery-vmware-to-azure.md) für dieses Migrationsszenario angepasst. Verwenden Sie zusätzlich zu der Anleitung in diesem Artikel die Links zu den ausführlichen Schritten.

1. **Erstellen Sie einen Recovery Services-Tresor**. Erstellen und verwalten Sie den Site Recovery-Tresor über das [Azure-Portal](https://portal.azure.com). Klicken Sie auf **Neu** > **Verwaltung** > **Backup** und **Site Recovery (OMS)**. Sie können auch auf **Durchsuchen** > **Recovery Services-Tresor** > **Hinzufügen** klicken. Die VMs werden in der Region repliziert, die Sie in diesem Schritt angeben. Wählen Sie für eine Migration in derselben Region die Region aus, in der sich Ihre Quell-VMs und Quellspeicherkonten befinden. Beachten Sie, dass die Migration zu Storage Premium-Konten nur im [Azure-Portal](https://portal.azure.com) unterstützt wird, und nicht im [klassischen Portal](https://manage.windowsazure.com).

2. Die folgenden Schritte helfen Ihnen bei der **Auswahl Ihrer Schutzziele**.

    2a. Öffnen Sie auf der VM, auf der Sie den Konfigurationsserver installieren möchten, das [Azure-Portal](https://portal.azure.com). Navigieren Sie zu **Recovery Services-Tresore** > **Einstellungen**. Wählen Sie unter **Einstellungen** die Option **Site Recovery**. Wählen Sie unter **Site Recovery** die Option **Schritt 1: Bereiten Sie die Infrastruktur vor**. Wählen Sie unter **Infrastruktur vorbereiten** die Option **Schutzziel**.

    ![][2]

    2b. Wählen Sie unter **Schutzziel** in der Dropdownliste die Option **To Azure** (In Azure). Wählen Sie in der zweiten Dropdownliste die Option **Nicht virtualisiert/Andere**, und klicken Sie dann auf **OK**.

    ![][3]

3. Die folgenden Schritte helfen Ihnen beim **Einrichten der Quellumgebung (Konfigurationsserver)**.

    3a. Laden Sie das **einheitliche Setup von Azure Site Recovery** und den **Tresorregistrierungsschlüssel** herunter, indem Sie zum Blatt **Infrastruktur vorbereiten** > **Quelle vorbereiten** > **Server hinzufügen** navigieren. Sie benötigen den Tresorregistrierungsschlüssel zum Ausführen des einheitlichen Setups. Der Schlüssel ist nach der Erstellung fünf Tage lang gültig.

    ![][4]

    3b. Fügen Sie den Konfigurationsserver auf dem Blatt **Server hinzufügen** hinzu.

    ![][5]

    3c. Führen Sie auf der VM, die Sie als Konfigurationsserver verwenden, das einheitliche Setup aus, um den Konfigurationsserver und den Prozessserver zu installieren. Sie können die Screenshots [hier](../site-recovery/site-recovery-vmware-to-azure.md#set-up-the-source-environment) nacheinander anzeigen, um die Installation durchzuführen. Bei den für dieses Migrationsszenario angegebenen Schritten helfen Ihnen die folgenden Screenshots weiter.

    Wählen Sie unter **Vorbereitung** die Option **Install the configuration server and process server** (Konfigurationsserver und Prozessserver installieren) aus.

    ![][6]

    3d. Navigieren Sie unter **Registrierung** zu dem Registrierungsschlüssel, den Sie aus dem Tresor heruntergeladen haben, und wählen Sie ihn aus.

    ![][7]

    3e. Wählen Sie unter **Umgebungsdetails** aus, ob Sie VMware-VMs replizieren werden. Wählen Sie für dieses Migrationsszenario die Option **Nein**.

    ![][8]

    3f. Nachdem die Installation abgeschlossen ist, wird das Fenster **Microsoft Azure Site Recovery Configuration Server** (Microsoft Azure Site Recovery-Konfigurationsserver) angezeigt. Verwenden Sie die Registerkarte **Konten verwalten**, um das Konto zu erstellen, das von Site Recovery für die automatische Ermittlung verwendet werden kann. (Beim Szenario zum Schützen von physischen Computern ist das Einrichten des Kontos nicht relevant. Sie benötigen aber mindestens ein Konto, um einen der folgenden Schritte zu ermöglichen. In diesem Fall können Sie das Konto und Kennwort beliebig benennen.) Verwenden Sie die Registerkarte **Vault Registration** (Tresorregistrierung), um die Datei mit den Tresoranmeldeinformationen hochzuladen.

    ![][9]

4. **Richten Sie die Zielumgebung ein**. Klicken Sie auf **Infrastruktur vorbereiten** > **Ziel**, und geben Sie das Bereitstellungsmodell an, das Sie für VMs nach dem Failover verwenden möchten. Sie können je nach Szenario die Option **Klassisch** oder **Resource Manager** wählen.

    ![][10]

    Site Recovery prüft, ob Sie über ein oder mehrere kompatible Azure-Speicherkonten und -Netzwerke verfügen. Beachten Sie Folgendes: Wenn Sie ein Storage Premium-Konto für replizierte Daten verwenden, müssen Sie ein weiteres Standardspeicherkonto zum Speichern von Replikationsprotokollen einrichten.

5. **Richten Sie Replikationseinstellungen ein**. Führen Sie die Schritte unter [Einrichten von Replikationseinstellungen](../site-recovery/site-recovery-vmware-to-azure.md#set-up-replication-settings) aus, um sicherzustellen, dass Ihr Konfigurationsserver erfolgreich der von Ihnen erstellten Replikationsrichtlinie zugeordnet wurde.

6. **Kapazitätsplanung**. Verwenden Sie den [Capacity Planner](../site-recovery/site-recovery-capacity-planner.md), um Ihre Netzwerkbandbreite, Speicheranforderungen und anderen Anforderungen genau einschätzen und Ihren Replikationsbedarf erfüllen zu können. Wählen Sie anschließend unter **Haben Sie die Kapazitätsplanung abgeschlossen?** die Option **Ja**.

    ![][11]

7. Die folgenden Schritte helfen Ihnen beim **Installieren des Mobilitätsdiensts und beim Aktivieren der Replikation**.

    7a. Sie können die [Pushinstallation](../site-recovery/site-recovery-vmware-to-azure.md#prepare-for-automatic-discovery-and-push-installation) für Ihre Quell-VMs wählen oder [den Mobilitätsdienst manuell auf Ihren Quell-VMs installieren](../site-recovery/site-recovery-vmware-to-azure-install-mob-svc.md). Die Anforderungen für die Pushinstallation und den Pfad für die manuelle Installation finden Sie unter dem angegebenen Link. Bei Verwendung einer manuellen Installation müssen Sie unter Umständen eine interne IP-Adresse verwenden, um den Konfigurationsserver zu ermitteln.

    ![][12]

    Die für das Failover verwendete VM verfügt über zwei temporäre Datenträger: einen von der primären VM, und der andere wird während der Bereitstellung der VM in der Wiederherstellungsregion erstellt. Installieren Sie den Mobilitätsdienst vor dem Aktivieren der Replikation, um den temporären Datenträger vor der Replikation auszuschließen. Weitere Informationen dazu, wie Sie den temporären Datenträger ausschließen, finden Sie unter [Ausschließen von Datenträgern von der Replikation](../site-recovery/site-recovery-vmware-to-azure.md#exclude-disks-from-replication).

    7b. Aktivieren Sie die Replikation jetzt wie folgt:
      * Klicken Sie auf **Replicate application (Anwendung replizieren)** > **Quelle**. Klicken Sie nach der erstmaligen Aktivierung der Replikation im Tresor auf „+Replizieren“, um die Replikation für weitere Computer zu aktivieren.
      * Richten Sie die Quelle in Schritt 1 als Prozessserver ein.
      * Geben Sie in Schritt 2 das Bereitstellungsmodell für die Zeit nach dem Failover, ein Storage Premium-Konto als Migrationsziel, ein Standard-Speicherkonto zum Speichern von Protokollen und ein virtuelles Netzwerk für das Failover an.
      * Fügen Sie in Schritt 3 geschützte VMs nach IP-Adresse hinzu (unter Umständen benötigen Sie eine interne IP-Adresse, um sie zu ermitteln).
      * Konfigurieren Sie in Schritt 4 die Eigenschaften, indem Sie die Konten auswählen, die Sie zuvor auf dem Prozessserver eingerichtet haben.
      * Wählen Sie in Schritt 5 die zuvor erstellte Replikationsrichtlinie aus, und richten Sie die Replikationseinstellungen ein.
      Klicken Sie auf **OK**, und aktivieren Sie die Replikation.

    > [!NOTE]
    > Wenn die Zuordnung einer Azure-VM aufgehoben und die VM neu gestartet wird, ist nicht sichergestellt, dass sie die gleiche IP-Adresse erhält. Falls sich die IP-Adresse des Konfigurationsservers/Prozessservers oder der geschützten Azure-VMs ändert, funktioniert die Replikation in diesem Szenario unter Umständen nicht richtig.

    ![][13]

    Für das Entwerfen Ihrer Azure Storage-Umgebung empfehlen wir Ihnen, für jede VM einer Verfügbarkeitsgruppe separate Speicherkonten zu verwenden. Es empfiehlt sich, die bewährte Methode in der Speicherebene zum [Verwenden mehrerer Speicherkonten für jede Verfügbarkeitsgruppe](../virtual-machines/windows/manage-availability.md) auszuführen. Das Verteilen von VM-Datenträgern auf mehrere Speicherkonten trägt zur Verbesserung der Speicherverfügbarkeit bei, und der E/A-Aufwand wird auf die gesamte Azure-Speicherinfrastruktur verteilt. Falls sich Ihre VMs in einer Verfügbarkeitsgruppe befinden und Datenträger aller VMs nicht unter einem Speicherkonto repliziert werden, empfehlen wir dringend, mehrere VMs mehrfach zu migrieren. Hiermit soll erreicht werden, dass für die VMs einer Verfügbarkeitsgruppe nicht nur ein gemeinsames Speicherkonto genutzt wird. Verwenden Sie das Blatt **Replikation aktivieren**, um nacheinander jeweils ein Zielspeicherkonto für jede VM einzurichten. Sie können je nach Ihren Anforderungen ein Bereitstellungsmodell für die Zeit nach dem Failover wählen. Wenn Sie als Bereitstellungsmodell für die Zeit nach dem Failover „Resource Manager“ (RM) wählen, können Sie für eine RM-VM ein Failover zu einer anderen RM-VM oder für eine klassische VM zu einer RM-VM durchführen.

8. **Führen Sie ein Testfailover durch**. Um zu prüfen, ob die Replikation abgeschlossen ist, klicken Sie auf Ihre Site Recovery-Instanz und dann auf **Einstellungen** > **Replizierte Elemente**. Der Status und der Prozentsatz Ihres Replikationsprozesses werden angezeigt. Führen Sie nach Abschluss des ersten Replikationsvorgangs ein Testfailover durch, um Ihre Replikationsstrategie zu überprüfen. Die ausführlichen Schritte des Testfailovers finden Sie unter [Durchführen eines Testfailovers in Site Recovery](../site-recovery/site-recovery-vmware-to-azure.md#run-a-test-failover). Der Status des Testfailovers wird unter **Einstellungen** > **Aufträge** > **NAME_IHRES_FAILOVERPLANS** angezeigt. Das Blatt enthält eine Übersicht über die Schritte und Ergebnisse (Erfolg/Fehler). Wenn für das Testfailover in einem beliebigen Schritt ein Fehler auftritt, können Sie auf den Schritt klicken, um die Fehlermeldung zu überprüfen. Stellen Sie sicher, dass für Ihre VMs und die Replikationsstrategie die Anforderungen erfüllt sind, bevor Sie ein Failover ausführen. Lesen Sie für weitere Informationen und Anweisungen zu Testfailovern das Dokument [Testfailover in Azure in Site Recovery](../site-recovery/site-recovery-test-failover-to-azure.md).

9. **Ausführen eines Failovers**. Führen Sie nach Abschluss des Testfailovers ein Failover durch, um Ihre Datenträger zu Storage Premium zu migrieren und die VM-Instanzen zu replizieren. Befolgen Sie die ausführlichen Schritte in [Ausführen eines Failovers](../site-recovery/site-recovery-failover.md#run-a-failover). Achten Sie darauf, die Option **Virtuelle Computer herunterfahren und die aktuellen Daten synchronisieren** auszuwählen, damit Site Recovery die geschützten VMs herunterfährt und die Daten synchronisiert und das Failover mit der neuesten Version der Daten erfolgt. Wenn Sie diese Option nicht auswählen oder der Vorgang nicht erfolgreich ausgeführt werden kann, wird das Failover für den neuesten verfügbaren Wiederherstellungspunkt der VM durchgeführt. Site Recovery erstellt eine VM-Instanz, die den Typ „Storage Premium-fähige VM“ oder einen ähnlichen Typ aufweist. Sie können sich über die Leistung und den Preis verschiedener VM-Instanzen unter [Virtuelle Windows-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) und [Virtuelle Linux-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) informieren.

<a id="post-migration-steps" class="xliff"></a>

## Schritte nach der Migration

1. **Konfigurieren Sie replizierte VMs in der Verfügbarkeitsgruppe (falls zutreffend)**. Für Site Recovery wird das Migrieren von VMs zusammen mit der Verfügbarkeitsgruppe nicht unterstützt. Wählen Sie je nach Bereitstellungstyp Ihrer replizierten VM eine der folgenden Vorgehensweisen:
  * Für eine mit dem klassischen Bereitstellungsmodell erstellte VM: Fügen Sie die VM im Azure-Portal der Verfügbarkeitsgruppe hinzu. Ausführliche Schritte finden Sie unter [Fügen Sie einer Verfügbarkeitsgruppe einen vorhandenen virtuellen Computer hinzu](../virtual-machines/windows/classic/configure-availability.md#a-idaddmachine-aoption-2-add-an-existing-virtual-machine-to-an-availability-set).
  * Für das Resource Manager-Bereitstellungsmodell: Speichern Sie Ihre Konfiguration der VM, und führen Sie anschließend das Löschen und Neuerstellen der VMs in der Verfügbarkeitsgruppe durch. Verwenden Sie hierfür das Skript unter [Set Azure Resource Manager VM Availability Set](https://gallery.technet.microsoft.com/Set-Azure-Resource-Manager-f7509ec4) (Festlegen der Azure Resource Manager-VM-Verfügbarkeitsgruppe). Überprüfen Sie die Einschränkungen dieses Skripts, und planen Sie Ausfallzeit ein, bevor Sie das Skript ausführen.

2. **Löschen Sie alte VMs und Datenträger**. Stellen Sie vor dem Löschen sicher, dass die Premium-Datenträger mit den Quelldatenträgern konsistent sind und die neuen VMs die gleiche Funktion wie die Quell-VMs erfüllen. Löschen Sie beim Resource Manager-Bereitstellungsmodell (RM) die VM, und löschen Sie die Datenträger aus Ihren Quellspeicherkonten im Azure-Portal. Beim klassischen Bereitstellungsmodell können Sie die VM und die Datenträger im klassischen Portal oder im Azure-Portal löschen. Falls der Datenträger nicht gelöscht wird, obwohl Sie die VM gelöscht haben, helfen Ihnen die Informationen unter [Beheben von Fehlern beim Löschen von Azure-Speicherkonten, -Containern oder -VHDs in einer Resource Manager-Bereitstellung](storage-resource-manager-cannot-delete-storage-account-container-vhd.md) oder [Problembehandlung beim Löschen von Azure Storage-Konten, -Containern oder -VHDs in einer klassischen Bereitstellung](storage-cannot-delete-storage-account-container-vhd.md) weiter.

3. **Bereinigen Sie die Azure Site Recovery-Infrastruktur**. Falls Site Recovery nicht mehr benötigt wird, können Sie die dazugehörige Infrastruktur bereinigen, indem Sie die replizierten Elemente, den Konfigurationsserver und die Wiederherstellungsrichtlinie löschen und anschließend den Azure Site Recovery-Tresor löschen.

<a id="troubleshooting" class="xliff"></a>

## Problembehandlung

* [Überwachung und Problembehandlung für den Schutz von virtuellen Computern und physischen Servern](../site-recovery/site-recovery-monitoring-and-troubleshooting.md)
* [Microsoft Azure Site Recovery-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)

<a id="next-steps" class="xliff"></a>

## Nächste Schritte

Informationen zu bestimmten Szenarios zur Migration virtueller Computer finden Sie in den folgenden Ressourcen:

* [Migrate Azure Virtual Machines between Storage Accounts (in englischer Sprache)](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Erstellen und Hochladen einer Windows Server-VHD nach Azure](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Erstellen und Hochladen einer virtuellen Festplatte, die das Linux-Betriebssystem enthält](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Migrating Virtual Machines from Amazon AWS to Microsoft Azure (in englischer Sprache)](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Lesen Sie außerdem die folgenden Ressourcen, um mehr über Azure Storage und Azure Virtual Machines zu erfahren:

* [Azure Storage (in englischer Sprache)](https://azure.microsoft.com/documentation/services/storage/)
* [Dokumentation zu virtuellen Computern](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Premium-Speicher: Hochleistungsspeicher für Workloads in Azure Virtual Machine](storage-premium-storage.md)

[1]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-1.png
[2]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-2.png
[3]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-3.png
[4]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-4.png
[5]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-5.png
[6]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-6.PNG
[7]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-7.PNG
[8]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-8.PNG
[9]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-9.PNG
[10]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-10.png
[11]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-11.PNG
[12]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-12.PNG
[13]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-13.png
[14]:../site-recovery/media/site-recovery-vmware-to-azure/v2a-architecture-henry.png
[15]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-14.png

