---
title: Klonen von Volumes der StorSimple 8000-Serie | Microsoft-Dokumentation
description: Beschreibt die verschiedenen Klontypen und ihre Verwendung und wie Sie mit einem Sicherungssatz ein einzelnes Volume klonen.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 070ac53e-7388-4c48-b8a5-8ed7f9108b2c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/27/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: eb56cae77722268f42e5126c45ad2878af7db94a
ms.openlocfilehash: 0db0af490f03525f4c23c4e9a23e62a1ef9f1c7f


---
# <a name="use-the-storsimple-manager-service-to-clone-a-volume-update-2"></a>Klonen eines Volumes mithilfe des StorSimple Manager-Diensts (Update 2)
[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Übersicht
Auf der Seite **Sicherungskatalog** des StorSimple Manager-Diensts werden alle Sicherungssätze angezeigt, die mithilfe manueller oder automatisierter Sicherungen erstellt wurden. Sie können auf dieser Seite alle Sicherungen für eine Sicherungsrichtlinie oder ein Volume auflisten, Sicherungen auswählen oder löschen oder eine Sicherung zum Wiederherstellen oder Klonen eines Volumes verwenden.

![Seite "Sicherungskatalog"](./media/storsimple-clone-volume-u2/backupCatalog.png)  

In diesem Tutorial wird beschrieben, wie Sie einen Sicherungssatz zum Klonen eines einzelnen Volumes verwenden können. Außerdem wird der Unterschied zwischen *vorübergehenden* und *dauerhaften* Klonen beschrieben.

> [!NOTE]
> Ein lokal fixiertes Volume wird als mehrstufiges Volume geklont. Wenn das geklonte Volume lokal fixiert werden muss, können Sie den Klon in ein lokal fixiertes Volume konvertieren, nachdem der Klonvorgang erfolgreich abgeschlossen wurde. Informationen zum Konvertieren eines mehrstufigen Volumes in ein lokal fixiertes Volume finden Sie unter [Ändern des Volumetyps](storsimple-manage-volumes-u2.md#change-the-volume-type).
> 
> Wenn Sie versuchen, ein geklontes Volume von mehrstufig in lokal fixiert zu ändern, schlägt die Konvertierung sofort nach dem Klonen (wenn es sich noch um einen vorübergehenden Klon handelt) mit der folgenden Fehlermeldung fehl:
> 
> `Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.` 
> 
> Dieser Fehler wird nur ausgegeben, wenn das Klonen auf einem anderen Gerät erfolgt. Sie können das Volume in lokal fixiert konvertieren, wenn Sie zunächst den vorübergehenden Klon in einen permanenten Klon konvertieren. Zeichnen Sie zum Konvertieren des vorübergehenden Klons in einen permanenten Klon eine Cloudmomentaufnahme auf.
> 
> 

## <a name="create-a-clone-of-a-volume"></a>Erstellen von Klonen von einem Volume
Sie können einen Klon anhand einer lokalen oder einer Cloudmomentaufnahme auf dem gleichen Gerät, auf einem anderen Gerät oder sogar auf einem virtuellen Computer erstellen.

#### <a name="to-clone-a-volume"></a>So klonen Sie ein Volume
1. Klicken Sie auf der Seite des StorSimple-Manager-Diensts auf die Registerkarte **Sicherungskatalog** , und wählen Sie einen Sicherungssatz.
2. Erweitern Sie einen Sicherungssatz, um die zugehörigen Volumes anzuzeigen. Klicken Sie auf ein Volume im Sicherungssatz, um dieses auszuwählen.
   
     ![Volume klonen](./media/storsimple-clone-volume-u2/CloneVol.png) 
3. Klicken Sie auf **Klonen** , um mit dem Klonen des ausgewählten Volumes zu beginnen.
4. Führen Sie im Assistenten zum Klonen von Volumes unter **Name und Pfad angeben**folgende Schritte aus:
   
   1. Ermitteln Sie ein Zielgerät. Dies ist der Speicherort, an dem der Klon erstellt wird. Sie können das gleiche Gerät auswählen oder ein anderes Gerät angeben. Bei Auswahl eines Volumes, das anderen Cloud-Service Providern als Azure zugeordnet ist, werden in der Dropdownliste für das Zielgerät nur physische Geräte angezeigt. Ein Volume kann nicht auf ein virtuelles Gerät geklont werden, wenn es anderen Cloud-Service Providern zugeordnet ist.
      
      > [!NOTE]
      > Stellen Sie sicher, dass für den Klon weniger Kapazität benötigt wird, als auf dem Zielgerät zur Verfügung steht.
      > 
      > 
   2. Geben Sie einen eindeutigen Volumenamen für Ihren Klon an. Der Name muss zwischen 3 und 127 Zeichen lang sein. 
      
      > [!NOTE]
      > Das Feld **Volume klonen als** lautet selbst dann **Mehrstufig**, wenn Sie ein lokales Volume klonen. Sie können diese Einstellung nicht ändern. Sie können jedoch das geklonte Volume nach dem erfolgreichen Erstellen des Klons in ein lokales Volume konvertieren. Informationen zum Konvertieren eines mehrstufigen Volumes in ein lokal fixiertes Volume finden Sie unter [Ändern des Volumetyps](storsimple-manage-volumes-u2.md#change-the-volume-type).
      > 
      > 
      
        ![Klon-Assistent 1](./media/storsimple-clone-volume-u2/clone1.png) 
   3. Klicken Sie auf das Pfeilsymbol  ![Pfeilsymbol](./media/storsimple-clone-volume-u2/HCS_ArrowIcon.png) , um zur nächsten Seite zu wechseln.
5. Führen Sie unter **Hosts angeben, die dieses Volume verwenden können**folgende Schritte aus:
   
   1. Geben Sie einen Zugriffssteuerungsdatensatz (ACR) für den Klon an. Sie können einen neuen ACR hinzufügen oder diesen aus der Liste mit vorhandenen ACRs auswählen.
      
        ![Klon-Assistent 2](./media/storsimple-clone-volume-u2/clone2.png) 
   2. Klicken Sie zum Speichern der Änderungen am Zeitplan auf das Häkchensymbol  ![Häkchensymbol](./media/storsimple-clone-volume-u2/HCS_CheckIcon.png), um den Vorgang abzuschließen.
6. Es wird ein Klonauftrag initiiert. Sie werden benachrichtigt, wenn der Klon erstellt wurde. Klicken Sie auf **Auftrag anzeigen**, um den Klonauftrag auf der Seite **Aufträge** zu überwachen. Wenn der Klonauftrag abgeschlossen ist, wird die folgende Meldung angezeigt:
   
    ![Klonmeldung](./media/storsimple-clone-volume-u2/CloneMsg.png) 
7. Nach Abschluss des Klonauftrags:
   
   1. Navigieren Sie zur Seite **Geräte**, und klicken Sie auf die Registerkarte **Volumecontainer**. 
   2. Wählen Sie den Volumecontainer aus, der dem geklonten Ausgangsvolume zugeordnet ist. In der Liste der Volumes sollte der gerade erstellte Klon angezeigt werden.

> [!NOTE]
> Die Überwachung und die Standardsicherung werden auf geklonten Volumes automatisch deaktiviert.
> 
> 

Ein auf diese Weise erstellter Klon ist ein vorübergehender Klon. Weitere Informationen zu Klontypen finden Sie unter [Vergleich von vorübergehenden und dauerhaften Klonen](#transient-vs.-permanent-clones).

Dieser Klon ist jetzt ein reguläres Volume, und jeder mit einem Volume mögliche Vorgang kann auch für den Klon durchgeführt werden. Sie müssen dieses Volume für Sicherungen konfigurieren.

## <a name="transient-vs-permanent-clones"></a>Vergleich von vorübergehenden und dauerhaften Klonen
Vorübergehende Klone werden nur erstellt, wenn das Klonen auf einem anderen Gerät erfolgt. Sie können ein bestimmtes Volume aus einem Sicherungssatz auf einem anderen, vom StorSimple Manager verwalteten Gerät klonen. Der vorübergehende Klon enthält Verweise auf die Daten im ursprünglichen Volume, und er verwendet diese Daten zum lokalen Lesen und Schreiben auf dem Zielgerät. 

Nachdem Sie eine Cloudmomentaufnahme eines vorübergehenden Klons erstellt haben, wird der daraus resultierende Klon ein *dauerhafter* Klon. Während dieses Vorgangs wird eine Kopie der Daten in der Cloud erstellt, und die Zeit zum Kopieren dieser Daten wird durch die Datenmenge und die Azure-Latenzen bestimmt (hierbei handelt es sich um eine Azure-zu-Azure-Kopie). Dieser Vorgang kann einige Tage bis Wochen dauern. Der vorübergehende Klon wird so zum dauerhaften Klon und verfügt nicht über Verweise auf die ursprünglichen Volumedaten, aus denen er geklont wurde. 

## <a name="scenarios-for-transient-and-permanent-clones"></a>Szenarios für vorübergehende und dauerhafte Klone
In den folgenden Abschnitten werden beispielhafte Situationen beschreiben, in denen vorübergehende und dauerhafte Klone verwendet werden können.

### <a name="item-level-recovery-with-a-transient-clone"></a>Wiederherstellung auf Elementebene mit einem vorübergehenden Klon
Sie müssen eine ein Jahr alte Datei mit einer Microsoft PowerPoint-Präsentation wiederherstellen. Der IT-Administrator identifiziert die entsprechende Sicherung aus diesem Zeitraum und filtert das Volume dann. Anschließend klont der Administrator das Volume, ermittelt die Datei, nach der Sie suchen, und stellt sie Ihnen dann zur Verfügung. In diesem Szenario wird ein vorübergehender Klon verwendet. 

![Video verfügbar](./media/storsimple-clone-volume-u2/Video_icon.png) **Video verfügbar**

Um ein Video zu schauen, in dem gezeigt wird, wie Sie mithilfe des Klons und Wiederherstellungsfunktionen in StorSimple gelöschte Dateien wiederherstellen können, klicken Sie [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Testen in der Produktionsumgebung mit einem dauerhaften Klon
Sie müssen einen Testfehler in der Produktionsumgebung überprüfen. Sie erstellen einen Klon des Volumes in der Produktionsumgebung und dann eine Cloudmomentaufnahme dieses Klons, um ein unabhängiges geklontes Volume zu erstellen. In diesem Szenario wird ein dauerhafter Klon verwendet.  

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie Sie [StorSimple-Volumes aus einem Sicherungssatz wiederherstellen](storsimple-restore-from-backup-set-u2.md).
* Informationen zum [Verwalten Ihres StorSimple-Geräts mithilfe des StorSimple Manager-Diensts](storsimple-manager-service-administration.md).




<!--HONumber=Jan17_HO4-->


