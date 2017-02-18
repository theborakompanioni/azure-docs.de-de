---
title: "Bereitstellen von StorSimple Virtual Array 1 – Vorbereiten des Portals"
description: Erstes Tutorial zum Bereitstellen von virtuellen StorSimple-Arrays mit Vorbereitung des Portals
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 232e765b-9d2c-4907-8b11-aa7e1a17d3df
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/24/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: c0e2324a2b2e6294df6e502f2e7a0ae36ff94158
ms.openlocfilehash: 842ea26941a1141cde7db4e6ebff07d52aed9450


---
# <a name="deploy-storsimple-virtual-array---prepare-the-portal"></a>Bereitstellen von StorSimple Virtual Array – Vorbereiten des Portals
![](./media/storsimple-ova-deploy1-portal-prep/getstarted4.png)

## <a name="overview"></a>Übersicht
Dieser Artikel bezieht sich auf Microsoft Azure StorSimple Virtual Array (auch als „lokales virtuelles StorSimple-Gerät“ oder „virtuelles StorSimple-Gerät“ bezeichnet) mit der Version vom März 2016 (allgemeine Verfügbarkeit). Dies ist der erste Artikel einer Reihe von Tutorials, in denen die vollständige Bereitstellung Ihres virtuellen Arrays als Dateiserver oder iSCSI-Server erläutert wird. In diesem Artikel wird die Vorbereitung beschrieben, die für die Erstellung und Konfiguration Ihres StorSimple Manager-Diensts vor dem Bereitstellen eines virtuellen Arrays erforderlich ist. Außerdem enthält dieser Artikel Links zu einer Prüfliste für die Bereitstellungskonfiguration und Konfigurationsvoraussetzungen.

Sie benötigen Administratorrechte, um die Einrichtung und Konfiguration abzuschließen. Es wird empfohlen, die Prüfliste für die Bereitstellungskonfiguration durchzugehen, bevor Sie beginnen. Die Vorbereitung des Portals dauert weniger als 10 Minuten.

Die in diesem Artikel veröffentlichten Informationen gelten für die Bereitstellung von StorSimple Virtual Arrays, sowohl im klassischen Azure-Portal als auch in der Microsoft Azure Government-Cloud.

### <a name="get-started"></a>Erste Schritte
Der Bereitstellungsworkflow umfasst das Vorbereiten des Portals, das Bereitstellen eines virtuellen Arrays in Ihrer virtualisierten Umgebung und das Durchführen des Setups. Zum Beginnen mit der StorSimple Virtual Array-Bereitstellung als Dateiserver oder iSCSI-Server benötigen Sie die folgenden Ressourcen (Artikel und Videos) in der angegebenen Tabelle.

#### <a name="deployment-articles"></a>Artikel zur Bereitstellung
Sehen Sie sich die folgenden Artikel in der angegebenen Reihenfolge an, um Ihr StorSimple Virtual Array bereitzustellen.

| **#** | **Schritt** | **Aktion** | **Verwendete Dokumente** |
| --- | --- | --- | --- |
| 1. |**Einrichten des klassischen Azure-Portals** |Erstellen und konfigurieren Sie Ihren StorSimple Manager-Dienst, bevor Sie ein virtuelles StorSimple-Gerät bereitstellen. |[Vorbereiten des Portals](storsimple-ova-deploy1-portal-prep.md) |
| 2. |**Bereitstellen des Virtual Array** |Stellen Sie für Hyper-V ein virtuelles StorSimple-Gerät auf einem Hostsystem mit Hyper-V unter Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2 bereit, und stellen Sie die Verbindung her. <br></br> <br></br> Stellen Sie für VMware ein lokales virtuelles StorSimple-Gerät auf einem Hostsystem mit VMware ESXi 5.5 und höher bereit, und stellen Sie die Verbindung her.<br></br> |[Bereitstellen eines Virtual Arrays in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) <br></br> <br></br> [Bereitstellen eines virtuellen Arrays in VMware](storsimple-ova-deploy2-provision-vmware.md) |
| 3. |**Einrichten des Virtual Array** |Führen Sie für Ihren Dateiserver das anfängliche Setup durch, registrieren Sie Ihren StorSimple-Dateiserver, und führen Sie die Geräteinstallation durch. Anschließend können Sie die SMB-Freigaben bereitstellen. <br></br> <br></br> Führen Sie für Ihren iSCSI-Server das anfängliche Setup durch, registrieren Sie Ihren StorSimple-iSCSI-Server, und führen Sie die Geräteinstallation durch. Anschließend können Sie die iSCSI-Volumes bereitstellen. |[Einrichten des virtuellen Arrays als Dateiserver](storsimple-ova-deploy3-fs-setup.md)<br></br> <br></br>[Einrichten des virtuellen Arrays als iSCSI-Server](storsimple-ova-deploy3-iscsi-setup.md) |

#### <a name="deployment-videos"></a>Videos zur Bereitstellung
| **Schritt** | **Video** |
| --- | --- |
| Enthält eine Schritt-für-Schritt-Anleitung für die ersten Schritte mit dem StorSimple Virtual Array. |[Get Started with the StorSimple Virtual Array](https://azure.microsoft.com/documentation/videos/get-started-with-the-storsimple-virtual-array/) |
| Enthält eine Schritt-für-Schritt-Anleitung zum Bereitstellen eines StorSimple Virtual Array in Hyper-V. |[Create a StorSimple Virtual Array](https://azure.microsoft.com/documentation/videos/create-a-storsimple-virtual-array/) |
| Enthält eine Schritt-für-Schritt-Anleitung zum Konfigurieren und Registrieren eines StorSimple Virtual Array. |[Configure a StorSimple Virtual Array](https://azure.microsoft.com/documentation/videos/configure-a-storsimple-virtual-array/) |
| Enthält eine Schritt-für-Schritt-Anleitung zum Erstellen von Freigaben, Sichern von Freigaben und Wiederherstellen von Daten in einem StorSimple Virtual Array, das als Dateiserver konfiguriert wird. |[Use the StorSimple Virtual Array](https://azure.microsoft.com/documentation/videos/use-the-storsimple-virtual-array/) |
| Enthält eine Schritt-für-Schritt-Anleitung für das Failover und die Notfallwiederherstellung eines StorSimple Virtual Array. |[StorSimple Virtual Array Disaster Recovery](https://azure.microsoft.com/documentation/videos/storsimple-virtual-array-disaster-recovery/) |

Sie können jetzt mit der Einrichtung des klassischen Azure-Portals beginnen.

## <a name="configuration-checklist"></a>Prüfliste für die Konfiguration
Die Prüfliste für die Konfiguration enthält die Informationen, die Sie vor der Konfiguration der Software auf dem StorSimple-Gerät erfassen müssen. Wenn Sie diese Informationen im Voraus sammeln, beschleunigt dies die Bereitstellung des StorSimple-Geräts in Ihrer Umgebung. Welche der folgenden Prüflisten Sie benötigen, hängt davon ab, ob Sie das virtuelle StorSimple-Gerät als Dateiserver oder iSCSI-Server bereitstellen.

* Laden Sie die PDF-Datei [StorSimple Virtual Array File Server Configuration Checklist](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf)(Konfigurationsprüfliste für StorSimple Virtual Array-Dateiserver) herunter.
* Laden Sie die PDF-Datei [StorSimple Virtual Array iSCSI Server Configuration Checklist](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf)(Konfigurationsprüfliste für StorSimple Virtual Array-iSCSI-Server) herunter.

## <a name="prerequisites"></a>Voraussetzungen
Hier sind die Konfigurationsvoraussetzungen für Ihren StorSimple Manager-Dienst, Ihr virtuelles StorSimple-Gerät und das Netzwerk des Rechenzentrums angegeben.

### <a name="for-the-storsimple-manager-service"></a>Für den StorSimple Manager-Dienst
Stellen Sie Folgendes sicher, bevor Sie beginnen:

* Sie besitzen ein Microsoft-Konto mit Anmeldeinformationen für den Zugriff.
* Sie besitzen ein Microsoft Azure-Speicherkonto mit Anmeldeinformationen für den Zugriff.
* Ihr Microsoft Azure-Abonnement ist für den StorSimple Manager-Dienst aktiviert.

### <a name="for-the-storsimple-virtual-device"></a>Für das virtuelle StorSimple-Gerät
Stellen Sie Folgendes sicher, bevor Sie ein virtuelles Gerät bereitstellen:

* Sie haben Zugriff auf ein Hostsystem mit Hyper-V unter Windows Server 2008 R2 oder höher oder VMware (ESXi 5.5 oder höher), das zum Bereitstellen eines Geräts verwendet werden kann.
* Das Hostsystem verfügt für die Bereitstellung des virtuellen Geräts über die folgenden Ressourcen:
  
  * Mindestens 4 Kerne
  * Mindestens 8 GB RAM
  * Eine Netzwerkschnittstelle
  * Einen virtuellen Datenträger mit 500 GB für Systemdaten

### <a name="for-the-datacenter-network"></a>Für das Netzwerk des Rechenzentrums
Stellen Sie Folgendes sicher, bevor Sie beginnen:

* Das Netzwerk in Ihrem Rechenzentrum ist gemäß den Netzwerkanforderungen für das StorSimple-Gerät konfiguriert. Weitere Informationen finden Sie unter [Systemanforderungen für das StorSimple Virtual Array](storsimple-ova-system-requirements.md).
* Für das virtuelle StorSimple-Gerät steht jederzeit eine Internetbandbreite von 5 MBit/s (oder mehr) zur Verfügung. Diese Bandbreite sollte nicht gemeinsam mit anderen Anwendungen genutzt werden.

## <a name="step-by-step-preparation"></a>Schritt-für-Schritt-Vorbereitung
Verwenden Sie die folgende Schritt-für-Schritt-Anleitung, um Ihr Portal für den StorSimple Manager-Dienst vorzubereiten.

## <a name="step-1-create-a-new-service"></a>Schritt 1: Erstellen eines neuen Diensts
Mit einer einzelnen Instanz des StorSimple Manager-Diensts können mehrere StorSimple 1200-Geräte verwaltet werden. Führen Sie die folgenden Schritte aus, um einen neuen StorSimple Manager-Dienst zu erstellen. Falls Sie einen vorhandenen StorSimple Manager-Dienst zum Verwalten Ihrer 1200-Geräte verwenden, können Sie diesen Schritt überspringen und mit [Schritt 2: Abrufen des Dienstregistrierungsschlüssels](#step-2-get-the-service-registration-key)fortfahren.

[!INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

> [!IMPORTANT]
> Wenn Sie nicht die automatische Erstellung eines Speicherkontos mit Ihrem Dienst aktiviert haben, müssen Sie mindestens ein Speicherkonto erstellen, nachdem Sie einen Dienst erstellt haben.
> 
> * Wenn Sie nicht automatisch ein Speicherkonto erstellt haben, finden Sie unter [Konfigurieren eines neuen Speicherkontos für den Dienst](#optional-step-configure-a-new-storage-account-for-the-service) ausführliche Anweisungen.
> * Wenn Sie die automatische Erstellung eines Speicherkontos aktiviert haben, fahren Sie mit [Schritt 2: Abrufen des Dienstregistrierungsschlüssels](#step-2-get-the-service-registration-key)fort.
> 
> 

## <a name="step-2-get-the-service-registration-key"></a>Schritt 2: Abrufen des Dienstregistrierungsschlüssels
Nachdem der StorSimple Manager-Dienst eingerichtet wurde und ausgeführt wird, müssen Sie den Dienstregistrierungsschlüssel abrufen. Dieser Schlüssel wird zum Registrieren Ihres StorSimple-Geräts sowie zum Herstellen einer Verbindung mit dem Dienst verwendet.

Führen Sie die folgenden Schritte im [klassischen Azure-Portal](https://manage.windowsazure.com/)aus.

[!INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

> [!NOTE]
> Der Dienstregistrierungsschlüssel wird zur Registrierung aller StorSimple Manager-Geräte verwendet, die für den StorSimple-Manager-Dienst registriert werden müssen.
> 
> 

## <a name="step-3-download-the-virtual-device-image"></a>Schritt 3: Herunterladen des Image mit dem virtuellen Gerät
Nach dem Beschaffen des Dienstregistrierungsschlüssels müssen Sie nun das passende Image mit dem virtuellen Gerät herunterladen, um auf Ihrem Hostsystem ein virtuelles Gerät bereitzustellen. Die Images virtueller Geräte sind betriebssystemspezifisch und können im klassischen Azure-Portal von der Seite „Schnellstart“ heruntergeladen werden.

> [!IMPORTANT]
> Die Software auf dem StorSimple Virtual Array kann nur in Verbindung mit dem StorSimple Manager-Dienst verwendet werden.
> 
> 

Führen Sie die folgenden Schritte im [klassischen Azure-Portal](https://manage.windowsazure.com/)aus.

#### <a name="to-get-the-virtual-device-image"></a>So rufen Sie das Image mit dem virtuellen Gerät ab
1. Klicken Sie auf der Seite des Diensts **StorSimple-Manager** auf den Dienst, den Sie erstellt haben. Damit gelangen Sie auf die Seite **Schnellstart** . (Sie können jederzeit über das Schnellstartsymbol ![](./media/storsimple-ova-deploy1-portal-prep/image8.png) auf die Seite **Schnellstart** zugreifen.)
2. Klicken Sie auf den Link für das Image, das Sie aus dem Microsoft Download Center herunterladen möchten. Die Imagedateien sind ca. 4,8 GB groß.
   
   * VHDX für Hyper-V unter Windows Server 2012 und höher
   * VHD für Hyper-V unter Windows Server 2008 R2 und höher
   * VMDK für VMWare ESXi 5.5 und höher
3. Laden Sie die Datei auf einen lokalen Datenträger herunter, und entzippen Sie sie. Notieren Sie den Speicherort der entzippten Datei.

![Video-Symbol](./media/storsimple-ova-deploy1-portal-prep/video_icon.png) **Video verfügbar**

Sehen Sie sich dieses Video mit einer Schritt-für-Schritt-Anleitung für die ersten Schritte mit dem StorSimple Virtual Array an.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Get-Started-with-the-StorSimple-Virtual-Array/player]
> 
> 

## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a>Optionaler Schritt: Konfigurieren eines neuen Speicherkontos für den Dienst
Dies ist ein optionaler Schritt, der nur dann ausgeführt werden muss, wenn Sie nicht die automatische Erstellung eines Speicherkontos mit Ihrem Dienst aktiviert haben.

Wenn Sie ein Azure-Speicherkonto in einer anderen Region erstellen müssen, finden Sie unter [Erstellen eines Speicherkontos](../storage/storage-create-storage-account.md#create-a-storage-account) entsprechende Schrittanleitungen.

Führen Sie die folgenden Schritte im [klassischen Azure-Portal](https://manage.windowsazure.com/) auf der Seite für den StorSimple Manager-Dienst aus, um ein vorhandenes Microsoft Azure-Speicherkonto hinzuzufügen.

#### <a name="to-add-a-storage-account"></a>So fügen Sie ein Speicherkonto hinzu
1. Wählen Sie auf der StorSimple-Manager-Dienst-Startseite Ihren Dienst aus, und doppelklicken Sie dann darauf. Damit gelangen Sie auf die Seite **Schnellstart** . Wählen Sie die Seite **Konfigurieren** aus.
2. Klicken Sie auf **Speicherkonto hinzufügen/bearbeiten**. Gehen Sie im Dialogfeld **Speicherkonten hinzufügen/bearbeiten** folgendermaßen vor:
   
   1. Klicken Sie auf **Neues hinzufügen**.
   2. Geben Sie einen Namen für Ihr Speicherkonto an.
   3. Geben Sie den primären **Zugriffsschlüssel** für Ihr Microsoft Azure-Speicherkonto an.
   4. Wählen Sie **SSL-Modus aktivieren** aus, um einen sicheren Kanal für die Netzwerkkommunikation zwischen Ihrem Gerät und der Cloud zu erstellen. Deaktivieren Sie das Kontrollkästchen **SSL-Modus aktivieren** nur, wenn der Betrieb in einer privaten Cloud stattfindet.
   5. Klicken Sie auf das Häkchensymbol ![](./media/storsimple-ova-deploy1-portal-prep/image7.png). Sie werden benachrichtigt, nachdem das Speicherkonto erfolgreich erstellt wurde.
      
      ![](./media/storsimple-ova-deploy1-portal-prep/image11.png)
3. Das neu erstellte Speicherkonto wird auf der Seite **Konfigurieren** unter **Speicherkonten** angezeigt. Klicken Sie auf **Speichern** , um das neu erstellte Speicherkonto zu speichern. Klicken Sie auf **OK** , wenn Sie zur Bestätigung aufgefordert werden.

## <a name="next-step"></a>Nächster Schritt
Im nächsten Schritt wird eine virtuelle Maschine für Ihr virtuelles StorSimple-Gerät bereitgestellt. Sehen Sie sich je nach Hostbetriebssystem die folgenden ausführlichen Anweisungen an:

* [Bereitstellen eines Virtual Array in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md)
* [Bereitstellen eines Virtual Array in VMware](storsimple-ova-deploy2-provision-vmware.md)




<!--HONumber=Jan17_HO5-->


