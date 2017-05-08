---
title: Wiederherstellen eines StorSimple-Volumes aus einer Sicherung | Microsoft Docs
description: "Erläutert, wie Sie die Seite &quot;Sicherungskatalog&quot; des StorSimple Manager-Diensts zum Wiederherstellen eines StorSimple-Volumes aus einem Sicherungssatz verwenden."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6f289c39-96c7-4d57-b68a-4bc2e99aef9d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/22/2017
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: 99b76e3bc2939c65654cbf606fda6f8a45e0c44b
ms.lasthandoff: 04/20/2017


---
# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>Wiederherstellen eines StorSimple-Volumes aus einem Sicherungssatz (Update 2)
[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Übersicht
Auf der Seite **Sicherungskatalog** werden alle Sicherungssätze angezeigt, die mithilfe manueller oder automatisierter Sicherungen erstellt wurden. Auf dieser Seite können Sie Sicherungen auflisten und verwalten, eine Wiederherstellung aus einem Sicherungssatz durchführen oder ein Volume klonen.

 ![Seite "Sicherungskatalog"](./media/storsimple-restore-from-backup-set-u2/restore.png)

In diesem Tutorial erfahren Sie, wie Sie Ihr Gerät mithilfe der Seite **Sicherungskatalog** aus einem Sicherungssatz wiederherstellen.

Sie können ein Volume aus einer lokalen oder aus einer Cloudsicherung wiederherstellen. In beiden Fällen wird das Volume durch die Wiederherstellung sofort online geschaltet, während die Daten im Hintergrund heruntergeladen werden. 

## <a name="before-you-restore"></a>Vor der Wiederherstellung
Bevor Sie einen Wiederherstellungsvorgang initiieren, sollten Sie Folgendes beachten:

* **Offlineschalten des Volumes:** Schalten Sie das Volume sowohl auf dem Host als auch auf dem Gerät offline, bevor Sie die Wiederherstellung initiieren. Sie können das Volume auf dem Host online schalten, wenn das Volume auf dem Gerät online ist. (Sie müssen nicht warten, bis die Wiederherstellung abgeschlossen ist.) Anleitungen hierzu finden Sie unter [Offlineschalten von Volumes](storsimple-manage-volumes-u2.md#take-a-volume-offline).
* **Volumetyp nach der Wiederherstellung:** Gelöschte Volumes werden basierend auf dem Typ in der Momentaufnahme wiederhergestellt. Lokale Volumes werden als lokale Volumes wiederhergestellt und mehrstufige Volumes als mehrstufige Volumes.
  
    Bei vorhandenen Volumes überschreibt der aktuelle Verwendungstyp des Volumes den Typ, der in der Momentaufnahme gespeichert ist. Wenn Sie beispielsweise ein Volume aus einer Momentaufnahme wiederherstellen, die erstellt wurde, als das Volume mehrstufig war, und der Volumetyp jetzt (aufgrund eines Konvertierungsvorgangs) lokal ist, wird das Volume als lokales Volume wiederhergestellt. In gleicher Weise gilt: Wenn ein vorhandenes lokales Volume erweitert wird und anschließend aus einer älteren Momentaufnahme, bei der das Volume kleiner war, wiederhergestellt wird, behält das wiederhergestellte Volume die aktuelle erweiterte Größe bei.
  
    Sie können ein Volume nicht von einem mehrstufigen Volume in ein lokales Volume konvertieren oder _umgekehrt_, während das Volume wiederhergestellt wird. Warten Sie, bis die Wiederherstellung abgeschlossen ist. Anschließend können Sie das Volume in einen anderen Typ konvertieren. Informationen zum Konvertieren eines Volumes finden Sie unter [Ändern des Volumetyps](storsimple-manage-volumes-u2.md#change-the-volume-type). 
* **Die Größe des Volumes spiegelt sich im wiederhergestellten Volume wider:** Dies ist ein wichtiger Aspekt, wenn Sie ein gelöschtes lokales Volume wiederherstellen (weil lokale Volumes vollständig bereitgestellt werden). Stellen Sie sicher, dass Sie über genügend Speicherplatz verfügen, bevor Sie versuchen, ein gelöschtes lokales Volume wiederherzustellen. 
* **Ein Volume kann während der Wiederherstellung nicht erweitert werden.** Warten Sie, bis die Wiederherstellung abgeschlossen ist, bevor Sie versuchen, das Volume zu erweitern. Informationen zum Erweitern eines Datenträgers finden Sie unter [Ändern eines Volumes](storsimple-manage-volumes-u2.md#modify-a-volume).
* **Sie können eine Sicherung ausführen, während Sie ein lokales Volume wiederherstellen.** Verfahren finden Sie unter [Verwalten von Sicherungsrichtlinien mithilfe des StorSimple Manager-Diensts](storsimple-manage-backup-policies.md).
* **Sie können einen Wiederherstellungsvorgang abbrechen:** Wenn Sie den Wiederherstellungsauftrag abbrechen, wird das Volume in den Zustand zurückversetzt, in dem es sich vor dem Start der Wiederherstellung befand. Verfahren hierzu finden Sie unter [Abbrechen eines Auftrags](storsimple-manage-jobs-u2.md#cancel-a-job).

## <a name="how-does-restore-work"></a>Funktionsweise der Wiederherstellung
Für Geräte, auf denen Update 4 oder höher ausgeführt wird, wird eine Heatmap-basierte Wiederherstellung implementiert. Wenn die Hostanforderungen für den Zugriff auf Daten das Gerät erreichen, werden diese Anforderungen erfasst, und eine Heatmap wird erstellt. Eine hohe Anforderungsrate führt zu Datenblöcken mit höheren Heat-Werten, eine niedrigere Anforderungsrate dagegen zu Datenblöcken mit niedrigeren Heat-Werten. Sie müssen mindestens zweimal auf die Daten zugreifen, damit sie als _hot_ markiert werden. Eine Datei, die geändert wird, wird auch als _hot_ markiert. Nach dem Initiieren der Wiederherstellung erfolgt die proaktive Aktualisierung der Daten basierend auf der Heatmap. Für Versionen vor Update 4 wurden die Daten während der Wiederherstellung nur basierend auf dem Zugriff heruntergeladen. 

Die Heatmap-basierte Erfassung ist nur für mehrstufige Volumes aktiviert. Lokale Volumes werden nicht unterstützt. Auch beim Klonen eines Volumes auf einem anderen Gerät wird die Heatmap-basierte Wiederherstellung nicht unterstützt. Wenn bei einer direkten Wiederherstellung auf dem Gerät eine lokale Momentaufnahme für das wiederherzustellende Volume vorhanden ist, wird keine Aktivierung durchgeführt (da die Daten bereits lokal verfügbar sind). Standardmäßig werden bei der Wiederherstellung die Aktivierungsaufträge initiiert, mit denen Daten basierend auf der Heatmap proaktiv aktiviert werden. In Update 4 können Windows PowerShell-Cmdlets zum Abfragen der ausgeführten Aktivierungsaufträge, zum Abbrechen eines Aktivierungsauftrags und zum Abrufen des Status des Aktivierungsvorgangs verwendet werden.

* `Get-HcsRehydrationJob`: Mit diesem Cmdlet wird der Status des Aktivierungsauftrags abgerufen. Für ein Volume wird jeweils ein Aktivierungsauftrag ausgelöst.
* `Set-HcsRehydrationJob`: Mit diesem Cmdlet können Sie den Aktivierungsauftrag unterbrechen, beenden und wiederaufnehmen, wenn die Aktivierung ausgeführt wird.    

Weitere Informationen zu den Cmdlets für die Aktivierung finden Sie in der [Cmdlet-Referenz für Windows PowerShell für StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

Bei der automatischen Aktivierung wird normalerweise eine höhere vorübergehende Leseleistung erwartet. Die tatsächlichen Optimierungen hängen von verschiedenen Faktoren wie dem Zugriffsmuster, der Datenänderungsrate und dem Datentyp ab. Sie können einen Aktivierungsauftrag mit dem PowerShell-Cmdlet abbrechen. Wenn Sie Aktivierungsaufträge für alle zukünftigen Wiederherstellungen dauerhaft deaktivieren möchten, wenden Sie sich an den Microsoft-Support.

## <a name="how-to-use-the-backup-catalog"></a>So verwenden Sie den Sicherungskatalog
Die Seite **Sicherungskatalog** bietet eine Abfrage, mit der Sie die Auswahl der Sicherungssätze einschränken können. Sie können die abgerufenen Sicherungssätze anhand der folgenden Parameter filtern:

* **Gerät** – das Gerät, auf dem der Sicherungssatz erstellt wurde.
* **Sicherungsrichtlinie** oder **Volume**: die Sicherungsrichtlinie oder das Volume, der oder dem dieser Sicherungssatz zugeordnet ist.
* **Von** und **Bis**: der Datums- und Uhrzeitbereich, in dem die Sicherung erstellt wurde.

Die gefilterten Sicherungssätze werden dann basierend auf den folgenden Attributen in Tabellenform angezeigt:

* **Name** – der Name der Sicherungsrichtlinie oder des Volumes, der oder dem dieser Sicherungssatz zugeordnet ist.
* **Größe** – die tatsächliche Größe des Sicherungssatzes.
* **Erstellt am** – das Datum und die Uhrzeit der Erstellung der Sicherungen. 
* **Typ** – Sicherungssätze können lokale Momentaufnahmen oder Cloudmomentaufnahmen sein. Eine lokale Momentaufnahme ist eine Sicherung aller Volumedaten, die lokal auf dem Gerät gespeichert sind. Die Sicherung von Volumedaten in der Cloud wird dagegen als Cloudmomentaufnahme bezeichnet. Lokale Momentaufnahmen bieten schnelleren Zugriff, während Cloudmomentaufnahmen für Datenstabilität ausgewählt werden.
* **Initiiert von:** Die Sicherungen können automatisch nach einem Zeitplan oder manuell von Ihnen initiiert werden. (Sie können eine Sicherungsrichtlinie verwenden, um Sicherungen zu planen. Es ist aber auch möglich, mithilfe der Option **Sicherung erstellen** eine interaktive Sicherung durchzuführen.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>So stellen Sie Ihr StorSimple-Volume aus einer Sicherung wieder her
Sie können Ihr StorSimple-Volume auf der Seite **Sicherungskatalog** aus einer bestimmten Sicherung wiederherstellen. Beachten Sie dabei, dass das Volume durch die Wiederherstellung auf den Zustand zum Zeitpunkt der Sicherung zurückgesetzt wird. Alle Daten, die nach dem Sicherungsvorgang hinzugefügt wurden, gehen verloren.

> [!WARNING]
> Beim Wiederherstellen aus einer Sicherung werden die vorhandenen Volumes durch die Sicherung ersetzt. Dadurch können Daten verloren gehen, die nach dem Erstellen der Sicherung geschrieben wurden.
> 
> 

### <a name="to-restore-your-volume"></a>So stellen Sie Ihr Volume wieder her
1. Klicken Sie auf der Seite des StorSimple Manager-Diensts auf die Registerkarte **Sicherungskatalog** .
   
    ![Sicherungskatalog](./media/storsimple-restore-from-backup-set-u2/restore.png)
2. Wählen Sie wie folgt einen Sicherungssatz aus:
   
   1. Wählen Sie das entsprechende Gerät aus.
   2. Wählen Sie in der Dropdownliste das Volume oder die Sicherungsrichtlinie für die gewünschte Sicherung aus.
   3. Geben Sie den Zeitraum an.
   4. Klicken Sie auf das Häkchensymbol  ![Häkchensymbol](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) , um diese Abfrage durchzuführen.
      
      Die dem ausgewählten Volume oder der Sicherungsrichtlinie zugeordneten Sicherungen sollten in der Liste der Sicherungssätze angezeigt werden.
3. Erweitern Sie einen Sicherungssatz, um die zugehörigen Volumes anzuzeigen. Diese Volumes müssen auf dem Host und dem Gerät offline geschaltet werden, bevor sie wiederhergestellt werden können. Greifen Sie auf der Seite **Volumecontainer** auf die Volumes zu, und führen Sie dann die Schritte in [Offlineschalten eines Volumes](storsimple-manage-volumes-u2.md#take-a-volume-offline) durch, um diese offline zu schalten.
   
   > [!IMPORTANT]
   > Vergewissern Sie sich, dass die Volumes auf dem Host offline sind, bevor Sie diese auf dem Gerät offline schalten. Wenn Sie die Volumes auf dem Host nicht offline schalten, kann es zur Beschädigung von Daten kommen.
   > 
   > 
4. Navigieren Sie zurück zur Registerkarte **Sicherungskatalog** , und wählen Sie einen Sicherungssatz aus.
5. Klicken Sie unten auf der Seite auf **Wiederherstellen** .
6. Sie werden aufgefordert, diesen Schritt zu bestätigen. Überprüfen Sie die Wiederherstellungsinformationen, und aktivieren Sie dann das Kontrollkästchen zur Bestätigung.
   
    ![Bestätigungsseite](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)
7. Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) Ein Wiederherstellungsauftrag wird gestartet. Über die Seite **Aufträge** können Sie den Auftrag anzeigen. 
8. Nachdem die Wiederherstellung abgeschlossen ist, können Sie überprüfen, ob die Inhalte der Volumes durch die aus der Sicherung ersetzt wurden.

![Video verfügbar](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **Video verfügbar**

Um ein Video zu schauen, in dem gezeigt wird, wie Sie mithilfe des Klons und Wiederherstellungsfunktionen in StorSimple gelöschte Dateien wiederherstellen können, klicken Sie [hier](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-the-restore-fails"></a>Bei einem Wiederherstellungsfehler
Sie erhalten eine Warnung, wenn der Wiederherstellungsvorgang aus einem bestimmten Grund nicht durchgeführt werden kann. Aktualisieren Sie in diesem Fall die Sicherungsliste, um zu überprüfen, ob die Sicherung noch gültig ist. Wenn die Sicherung gültig ist und die Wiederherstellung aus der Cloud erfolgt, wird der Fehler möglicherweise durch Verbindungsprobleme verursacht. 

Um den Wiederherstellungsvorgang erfolgreich abzuschließen, schalten Sie das Volume auf dem Host offline, und wiederholen Sie den Wiederherstellungsvorgang. Alle Änderungen an den Volumedaten, die während des Wiederherstellungsvorgangs vorgenommen wurden, gehen verloren.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie Sie [StorSimple-Volumes verwalten](storsimple-manage-volumes-u2.md).
* Erfahren Sie, wie Sie [Ihr StorSimple-Gerät mithilfe des StorSimple Manager-Diensts verwalten](storsimple-manager-service-administration.md).


