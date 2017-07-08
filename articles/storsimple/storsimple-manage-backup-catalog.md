---
title: Verwalten Ihres StorSimple-Sicherungskatalogs | Microsoft Docs
description: "Erläutert, wie Sie die Seite \"Sicherungskatalog\" des StorSimple Manager-Diensts nutzen können, um Sicherungssätze für ein Volume aufzulisten, auszuwählen und zu löschen."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ad81bee9-fe43-40b3-a384-b15fb274ecd9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/28/2016
ms.author: v-sharos
ms.translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 5ee9855e1428c7a2d871d9c215d302c5c3b7101a
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="use-the-storsimple-manager-service-to-manage-your-backup-catalog"></a>Verwalten des Sicherungskatalogs mithilfe des StorSimple Manager-Diensts
## <a name="overview"></a>Übersicht
Auf der Seite **Sicherungskatalog** des StorSimple Manager-Diensts werden alle Sicherungssätze angezeigt, die mithilfe manueller oder geplanter Sicherungen erstellt wurden. Sie können auf dieser Seite alle Sicherungen für eine Sicherungsrichtlinie oder ein Volume auflisten, Sicherungen auswählen oder löschen oder eine Sicherung zum Wiederherstellen oder Klonen eines Volumes verwenden.

In diesem Tutorial wird erläutert, wie ein Sicherungssatz aufgeführt, ausgewählt und gelöscht wird. Um zu erfahren, wie Sie Ihr Gerät aus einer Sicherung wiederherstellen, gehen Sie zu [Wiederherstellen Ihres Geräts aus einem Sicherungssatz](storsimple-restore-from-backup-set.md). Um zu erfahren, wie Sie ein Volume klonen, gehen Sie zu [Klonen von StorSimple-Volumes](storsimple-clone-volume.md).

![Sicherungskatalog](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

Die Seite **Sicherungskatalog** bietet eine Abfrage, mit der Sie die Auswahl der Sicherungssätze einschränken können. Sie können die abgerufenen Sicherungssätze anhand der folgenden Parameter filtern:

* **Gerät** : das Gerät, auf dem der Sicherungssatz erstellt wurde.
* **Sicherungsrichtlinie oder Volume** : die Sicherungsrichtlinie oder das Volume, der oder dem dieser Sicherungssatz zugeordnet ist.
* **Von und Bis** : der Datums- und Uhrzeitbereich, in dem die Sicherung erstellt wurde.

Die gefilterten Sicherungssätze werden dann basierend auf den folgenden Attributen in Tabellenform angezeigt:

* **Name** – der Name der Sicherungsrichtlinie oder des Volumes, der oder dem dieser Sicherungssatz zugeordnet ist.
* **Größe** : die tatsächliche Größe des Sicherungssatzes.
* **Erstellt am** : das Datum und die Uhrzeit der Erstellung der Sicherungen. 
* **Typ** : Sicherungssätze können lokale Momentaufnahmen oder Cloudmomentaufnahmen sein. Eine lokale Momentaufnahme ist eine Sicherung aller Volumedaten, die auf dem lokalen Gerät gespeichert ist, während die Sicherung von Volumedaten in der Cloud als Cloudmomentaufnahme bezeichnet wird. Lokale Momentaufnahmen bieten schnelleren Zugriff, während Cloudmomentaufnahmen für Datenstabilität ausgewählt werden.
* **Initiiert von** : Die Sicherungen können automatisch nach einem Zeitplan oder manuell durch einen Benutzer initiiert werden. Sie können eine Sicherungsrichtlinie verwenden, um Sicherungen zu planen. Es ist aber auch möglich, mithilfe der Option **Sicherung erstellen** eine manuelle Sicherung durchzuführen.

## <a name="list-backup-sets-for-a-volume"></a>Aufführen der Sicherungssätze für ein Volume
Führen Sie die folgenden Schritte aus, um alle Sicherungen für ein Volume aufzuführen.

#### <a name="to-list-backup-sets"></a>Aufführen der Sicherungssätze
1. Klicken Sie auf der Seite des StorSimple Manager-Diensts auf die Registerkarte **Sicherungskatalog** .
2. Filtern Sie die Auswahl wie folgt:
   
   1. Wählen Sie das entsprechende Gerät aus.
   2. Wählen Sie in der Dropdown-Liste ein Volume zum Anzeigen der entsprechenden Sicherungen aus.
   3. Geben Sie den Zeitraum an.
   4. Klicken Sie auf das Häkchensymbol  ![Häkchensymbol](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) , um diese Abfrage durchzuführen.
      
      Die dem ausgewählten Volume zugeordneten Sicherungen sollten in der Liste der Sicherungssätze angezeigt werden.

## <a name="select-a-backup-set"></a>Wählen Sie einen Sicherungssatz aus
Führen Sie die folgenden Schritte aus, um einen Sicherungssatz für ein Volume oder eine Sicherungsrichtlinie auszuwählen.

#### <a name="to-select-a-backup-set"></a>Auswählen eines Sicherungssatzes
1. Klicken Sie auf der Seite des StorSimple Manager-Diensts auf die Registerkarte **Sicherungskatalog** .
2. Filtern Sie die Auswahl wie folgt:
   
   1. Wählen Sie das entsprechende Gerät aus.
   2. Wählen Sie in der Dropdownliste das Volume oder die Sicherungsrichtlinie für die gewünschte Sicherung aus.
   3. Geben Sie den Zeitraum an.
   4. Klicken Sie auf das Häkchensymbol  ![Häkchensymbol](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) , um diese Abfrage durchzuführen.
      
      Die dem ausgewählten Volume oder der Sicherungsrichtlinie zugeordneten Sicherungen sollten in der Liste der Sicherungssätze angezeigt werden.
3. Wählen Sie einen Sicherungssatz aus und erweitern Sie diesen. Die Optionen **Wiederherstellen** und **Löschen** werden unten auf der Seite angezeigt. Sie können beide Aktionen in dem Sicherungssatz durchführen, den Sie ausgewählt haben.

## <a name="delete-a-backup-set"></a>Löschen eines Sicherungssatzes
Löschen Sie eine Sicherung, wenn Sie die zugeordneten Daten nicht mehr beibehalten möchten. Führen Sie die folgenden Schritte aus, um einen Sicherungssatz zu löschen.

#### <a name="to-delete-a-backup-set"></a>Löschen eines Sicherungssatzes
1. Klicken Sie auf der Seite des StorSimple Manager-Diensts auf die Registerkarte **Sicherungskatalog**.
2. Filtern Sie die Auswahl wie folgt:
   
   1. Wählen Sie das entsprechende Gerät aus.
   2. Wählen Sie in der Dropdownliste das Volume oder die Sicherungsrichtlinie für die gewünschte Sicherung aus.
   3. Geben Sie den Zeitraum an.
   4. Klicken Sie auf das Häkchensymbol  ![Häkchensymbol](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) , um diese Abfrage durchzuführen.
      
      Die dem ausgewählten Volume oder der Sicherungsrichtlinie zugeordneten Sicherungen sollten in der Liste der Sicherungssätze angezeigt werden.
3. Wählen Sie einen Sicherungssatz aus und erweitern Sie diesen. Die Optionen **Wiederherstellen** und **Löschen** werden unten auf der Seite angezeigt. Klicken Sie auf **Löschen**.
4. Sie werden benachrichtigt, wenn der Löschvorgang durchgeführt wird und wenn er erfolgreich abgeschlossen wurde. Aktualisieren Sie die Abfrage auf dieser Seite, nachdem der Löschvorgang abgeschlossen wurde. Der gelöschte Sicherungssatz wird nicht mehr in der Liste der Sicherungssätze angezeigt.

## <a name="next-steps"></a>Nächste Schritte
* Informationen zur [Verwendung des Sicherungskatalogs zum Wiederherstellen Ihres Geräts aus einem Sicherungssatz](storsimple-restore-from-backup-set.md).
* Informationen zum [Verwalten Ihres StorSimple-Geräts mithilfe des StorSimple Manager-Diensts](storsimple-manager-service-administration.md).


