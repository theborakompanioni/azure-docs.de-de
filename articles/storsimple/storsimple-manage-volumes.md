---
title: Verwalten von StorSimple-Volumes | Microsoft Docs
description: "Erläutert, wie Sie StorSimple-Volumes hinzufügen, ändern, überwachen und löschen sowie ggf. offline schalten können."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ccabd859-590c-4568-a81d-6ff38f125be3
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/11/2016
ms.author: v-sharos
ms.translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 31ed9dad8ba56a3746873b7b35e678e97743fbfe
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="use-the-storsimple-manager-service-to-manage-volumes"></a>Verwalten von Volumes mithilfe des StorSimple Manager-Diensts
[!INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Übersicht
In diesem Tutorial wird erläutert, wie Sie den StorSimple-Manager-Dienst zum Erstellen und Verwalten von Volumes auf dem StorSimple-Gerät und dem virtuellen StorSimple-Gerät verwenden können.

Der StorSimple-Manager-Dienst ist eine Erweiterung des klassischen Azure-Portals, mit der Sie Ihre StorSimple-Lösung über eine einzelne Weboberfläche verwalten können. Zusätzlich zum Verwalten von Datenträgern können Sie mit dem StorSimple-Manager-Dienst StorSimple-Dienste erstellen und verwalten, Geräte anzeigen und verwalten, Warnungen anzeigen sowie Sicherungsrichtlinien und den Sicherungskatalog verwalten.

> [!NOTE]
> Mit Azure StorSimple können nur nach Bedarf bereitgestellte Volumes erstellt werden. In einem Azure StorSimple-System können keine vollständig oder teilweise bereitgestellten Volumes erstellt werden.
> 
> Die Bereitstellung nach Bedarf ist eine Virtualisierungstechnologie, bei der der verfügbare Speicher scheinbar die physischen Ressourcen überschreitet. Anstatt vorab ausreichend Speicherplatz zu reservieren, ordnet Azure StorSimple bei der Bereitstellung nach Bedarf nur den für die aktuellen Anforderungen erforderlichen Speicherplatz zu. Die Flexibilität der Cloudspeicherung ermöglicht diesen Ansatz, da Azure StorSimple diesen Cloudspeicher erhöhen oder verringern kann, um die veränderten Anforderungen zu erfüllen.
> 
> 

## <a name="the-volumes-page"></a>Die Seite "Volumes"
Auf der Seite **Volumes** können Sie die Speichervolumes verwalten, die auf dem Microsoft Azure StorSimple-Gerät für die Initiatoren (Server) bereitgestellt werden. Auf der Seite wird eine Liste der Volumes auf dem StorSimple-Gerät angezeigt.

 ![Seite „Volumes“](./media/storsimple-manage-volumes/HCS_VolumesPage.png)

Ein Volume umfasst eine Reihe von Attributen:

* **Name** – ein beschreibender Name, der eindeutig sein muss und den Datenträger identifiziert. Dieser Name wird auch in Überwachungsberichten zum Filtern nach einem bestimmten Volume verwendet.
* **Status** – online oder offline. Offline-Volumes sind für die Initiatoren (Server), die über Zugriff für die Verwendung des Volumes verfügen, nicht sichtbar.
* **Kapazität** – gibt an, wie groß das Volume aus Sicht des Initiators (Servers) ist. Die Kapazität entspricht der Gesamtmenge der Daten, die vom Initiator (Server) gespeichert werden können. Die Volumes verfügen über eine schlanke Speicherzuweisung, und die Daten werden dedupliziert. Dies bedeutet, dass Ihr Gerät die physische Speicherkapazität weder intern noch in der Cloud vorab anhand der konfigurierten Volumekapazität zuweist. Die Volumekapazität wird nach Bedarf zugewiesen und genutzt.
* **Typ** – Der Volumetyp kann mehrstufig oder Archivierung (ein Untertyp von mehrstufig) sein.
* **Zugriff** – gibt die Initiatoren (Server) an, die Zugriff auf dieses Volume haben. Initiatoren, die nicht Mitglieder des dem Volume zugeordneten Zugriffssteuerungsdatensatzes (ACR) sind, wird das Volume nicht angezeigt.
* **Überwachung** – gibt an, ob ein Volume überwacht wird. Beim Erstellen von Volumes wird die Überwachung standardmäßig aktiviert. Für einen Volumeklon wird die Überwachung jedoch deaktiviert. Befolgen Sie zum Überwachen eines Volumes die Anweisungen unter "Überwachen von Volumes".

Die häufigsten Aufgaben im Zusammenhang mit einem Volume sind:

* Hinzufügen eines Volumes
* Ändern von Volumes
* Löschen von Volumes
* Offlineschalten von Volumes
* Überwachen von Volumes

## <a name="add-a-volume"></a>Hinzufügen eines Volumes
Sie haben beim Bereitstellen der StorSimple-Lösung [ein Volume erstellt](storsimple-deployment-walkthrough-u1.md#step-6-create-a-volume) . Das Hinzufügen eines Volumes ist ein ähnliches Verfahren.

### <a name="to-add-a-volume"></a>So fügen Sie ein Volume hinzu
1. Wählen Sie auf der Seite **Geräte** das Gerät aus, doppelklicken Sie darauf, und klicken Sie dann auf die Registerkarte **Volumecontainer**.
2. Wählen Sie einen Volumecontainer aus, und klicken Sie auf den Pfeil in der entsprechenden Zeile, um auf die dem Container zugeordneten Volumes zuzugreifen.
3. Klicken Sie unten auf der Seite auf **Add** . Der Assistent "Volume hinzufügen" wird gestartet.
   
     ![Assistent zum Hinzufügen von Volumes – Grundeinstellungen](./media/storsimple-manage-volumes/AddVolume1.png)
4. Gehen Sie im Assistenten "Volume hinzufügen" unter **Grundlegende Einstellungen**folgendermaßen vor:
   
   1. Geben Sie den **Namen** für das Volume ein.
   2. Geben Sie die **Bereitgestellte Kapazität** für das Volume in GB oder TB an. Die Kapazität muss für ein physisches Gerät zwischen 1 GB und 64 TB liegen. Die maximale Kapazität, die für ein Volume auf einem virtuellen StorSimple-Gerät bereitgestellt werden kann, beträgt 30 TB.
   3. Wählen Sie den **Verwendungstyp** für das Volume aus. Bei Verwendung des mehrstufigen Volumes für archivierte Daten wird durch Aktivieren des Kontrollkästchens **Verwenden Sie dieses Volume für Archivdaten, auf die Sie seltener zugreifen** die Deduplizierungsblockgröße für das Volume in 512 KB geändert. Wenn dieses Feld nicht aktiviert ist, verwendet das entsprechende mehrstufige Volume eine Blockgröße von 64 KB. Bei Verwendung größerer Deduplizierungsblöcke kann das Gerät die Übertragung umfangreicher Archivdaten in die Cloud beschleunigen. (Mehrstufige Volumes wurden früher primäre Volumes genannt.)
   4. Klicken Sie auf das Pfeilsymbol ![Pfeilsymbol](./media/storsimple-manage-volumes/HCS_ArrowIcon.png), um zur Seite **Weitere Einstellungen** zu wechseln.
      
        ![Assistent zum Hinzufügen von Volumes – Zusätzliche Einstellungen](./media/storsimple-manage-volumes/AddVolume2.png)
5. Fügen Sie unter **Weitere Einstellungen**einen neuen Zugriffssteuerungsdatensatz (Access Control Record, ACR) hinzu:
   
   1. Wählen Sie aus der Dropdownliste einen Zugriffssteuerungsdatensatz aus. Sie können auch einen neuen Zugriffssteuerungsdatensatz hinzufügen. Mithilfe von Zugriffssteuerungsdatensätzen wird festgelegt, welche Hosts auf die Volumes zugreifen dürfen, indem der Host-IQN mit dem im Datensatz aufgeführten abgeglichen wird.
   2. Es wird empfohlen, eine Standardsicherung zu aktivieren, indem Sie das Kontrollkästchen **Standardsicherung für dieses Volume aktivieren** aktivieren.
   3. Klicken Sie auf das Häkchensymbol  ![Häkchensymbol](./media/storsimple-manage-volumes/HCS_CheckIcon.png) , um das Volume mit den angegebenen Einstellungen zu erstellen.

Das neue Volume kann nun verwendet werden.

## <a name="modify-a-volume"></a>Ändern von Volumes
Zum Ändern eines Volumes müssen Sie dieses erweitern oder die Hosts ändern, die auf das Volume zugreifen.

> [!IMPORTANT]
> * Wenn Sie die Größe des Volumes auf dem Gerät ändern, muss auch die Größe des Volumes auf dem Host geändert werden.
> * Die hier beschriebenen hostseitigen Schritte gelten für Windows Server 2012 (2012R2). Vorgehensweisen für Linux oder andere Hostbetriebssysteme können davon abweichen. Beachten Sie beim Ändern des Volumes auf einem Host mit einem anderen Betriebssystem die Anweisungen zu Ihrem Hostbetriebssystem.
> 
> 

### <a name="to-modify-a-volume"></a>So ändern Sie ein Volume
1. Wählen Sie auf der Seite **Geräte** das Gerät aus, doppelklicken Sie darauf, und klicken Sie dann auf die Registerkarte **Volumecontainer**. Auf dieser Seite werden die dem Gerät zugeordneten Volumecontainer in Tabellenform aufgeführt.
2. Wählen Sie einen Volumecontainer aus, und klicken Sie auf diesen, um die Liste aller Volumes innerhalb des Containers anzuzeigen.
3. Wählen Sie auf der Seite **Volumes** ein Volume aus, und klicken Sie auf **Ändern**.
4. Gehen Sie im Assistenten "Ändern von Volume" unter **Grundlegende Einstellungen**folgendermaßen vor:
   
   * Bearbeiten Sie den **Namen** und den **Typ**, wenn Sie ein mehrstufiges Volume in ein Archivierungsvolume ändern möchten, indem Sie das Kontrollkästchen **Verwenden Sie dieses Volume für Archivdaten, auf die Sie seltener zugreifen** aktivieren und so die Deduplizierungsblockgröße für das Volume in 512 KB ändern.
   * Erhöhen Sie die **Bereitgestellte Kapazität**. Die **Bereitgestellte Kapazität** kann nur erhöht werden. Ein Volume kann nach seiner Erstellung nicht mehr verkleinert werden.
     
     > [!NOTE]
     > Sie können den Volumecontainer nicht ändern, nachdem dieser einem Volume zugeordnet wurde.
     > 
     > 
5. Unter **Weitere Einstellungen**können Sie folgende Aufgaben durchführen:
   
   * Ändern Sie die Zugriffssteuerungsdatensätze, sofern das Volume offline ist. Wenn das Volume online ist, müssen Sie dieses zunächst offline schalten. Lesen Sie vor dem Bearbeiten eines Zugriffssteuerungsdatensatzes die Schritte unter [Offlineschalten von Volumes](#take-a-volume-offline) .
   * Ändern Sie die Liste der Zugriffssteuerungsdatensätze, nachdem das Volume offline ist.
     
     > [!NOTE]
     > Die Option **Standardsicherung für dieses Volume aktivieren** kann für das Volume nicht geändert werden.
     > 
     > 
6. Speichern Sie die Änderungen, indem Sie auf das Häkchensymbol  ![Häkchensymbol](./media/storsimple-manage-volumes/HCS_CheckIcon.png). Das klassische Azure-Portal zeigt eine Meldung zur Aktualisierung des Volumes an. Eine Erfolgsmeldung wird angezeigt, wenn das Volume aktualisiert wurde.
7. Wenn Sie ein Volume erweitern, führen Sie folgende Schritte auf dem Windows-Hostcomputer aus:
   
   1. Wechseln Sie zu **Computerverwaltung** ->**Datenträgerverwaltung**.
   2. Klicken Sie mit der rechten Maustaste auf **Datenträgerverwaltung**, und wählen Sie **Datenträger neu einlesen** aus.
   3. Wählen Sie in der Liste der Datenträger die aktualisierten Volumes aus und dann **Volume erweitern**. Der Assistent zum Erweitern von Volumes wird gestartet. Klicken Sie auf **Weiter**.

   4. Schließen Sie den Assistenten unter Verwendung der angegebenen Standardwerte ab. Nach Abschluss des Assistenten sollte für das Volume die höhere Speichergröße angezeigt werden.

![Video verfügbar](./media/storsimple-manage-volumes/Video_icon.png) **Video verfügbar**

Sie können sich [hier](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/)ein Video anschauen, in dem das Erweitern eines Volumes demonstriert wird.

## <a name="take-a-volume-offline"></a>Offlineschalten von Volumes
Möglicherweise müssen Sie ein Volume offline schalten, wenn Sie dieses ändern oder löschen möchten. Wenn ein Volume offline ist, ist es nicht für den Lese-/ Schreibzugriff verfügbar. Sie müssen das Volume sowohl auf dem Host als auch auf dem Gerät offline schalten. Führen Sie die folgenden Schritte durch, um ein Volume offline zu schalten.

### <a name="to-take-a-volume-offline"></a>So schalten Sie ein Volume offline
1. Stellen Sie vor dem Offlineschalten sicher, dass das betreffende Volume nicht verwendet wird.
2. Schalten Sie das Volume zunächst auf dem Host offline. Dadurch werden potenzielle Schäden an den Daten auf dem Volume vermieden. Genaue Anweisungen finden Sie in den Anweisungen für das Hostbetriebssystem.
3. Wenn der Host offline ist, schalten Sie das Volume anhand der folgenden Schritte auf dem Gerät offline:
   
   1. Wählen Sie auf der Seite **Geräte** das Gerät aus, doppelklicken Sie darauf, und klicken Sie dann auf die Registerkarte **Volumecontainer**. Auf dieser Seite werden die dem Gerät zugeordneten **Volumecontainer** in Tabellenform aufgeführt.
   2. Wählen Sie einen Volumecontainer aus, und klicken Sie auf diesen, um die Liste aller Volumes innerhalb des Containers anzuzeigen.
   3. Wählen Sie ein Volume aus, und klicken Sie auf **Offline schalten**.
   4. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **Ja**. Das Volume sollte jetzt offline sein.
      
      Wenn ein Volume offline geschaltet wurde, wird die Option **Online schalten** angezeigt.

> [!NOTE]
> Mit dem Befehl **Offline schalten** wird eine Anforderung an das Gerät gesendet, das Volume offline zu schalten. Wenn das Volume weiterhin von Hosts verwendet wird, führt dies zur Unterbrechung von Verbindungen, das Offlineschalten des Volumes schlägt jedoch nicht fehl.
> 
> 

## <a name="delete-a-volume"></a>Löschen von Volumes
> [!IMPORTANT]
> Sie können ein Volume nur löschen, wenn dieses offline ist.
> 
> 

Führen Sie die folgenden Schritte durch, um ein Volume zu löschen.

### <a name="to-delete-a-volume"></a>So löschen Sie ein Volume
1. Wählen Sie auf der Seite **Geräte** das Gerät aus, doppelklicken Sie darauf, und klicken Sie dann auf die Registerkarte **Volumecontainer**.
2. Wählen Sie den Volumecontainer mit dem zu löschenden Volume aus. Klicken Sie auf den Volumecontainer, um die Seite **Volumes** anzuzeigen.
3. Alle diesem Container zugeordneten Volumes werden in Tabellenform angezeigt. Überprüfen Sie den Status des zu löschenden Volumes. Wenn das zu löschende Volume nicht offline ist,schalten Sie es zunächst mithilfe der Schritte unter [Offlineschalten von Volumes](#take-a-volume-offline)offline.
4. Nachdem das Volume offline ist, klicken Sie unten auf der Seite auf **Löschen** .
5. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **Ja**. Das Volume wird nun gelöscht. Auf der Seite **Volumes** wird die aktualisierte Liste der Volumes innerhalb des Containers angezeigt.

## <a name="monitor-a-volume"></a>Überwachen von Volumes
Mit der Volumeüberwachung können Sie E/A-bezogene Statistiken für ein Volume erfassen. Die Überwachung wird standardmäßig für die ersten 32 von Ihnen erstellten Volumes aktiviert. Die Überwachung der zusätzlichen Volumes ist in der Standardeinstellung deaktiviert. Die Überwachung geklonter Volumes ist ebenfalls standardmäßig deaktiviert.

Führen Sie die folgenden Schritte durch, um die Überwachung eines Volumes zu aktivieren oder zu deaktivieren.

### <a name="to-enable-or-disable-volume-monitoring"></a>So aktivieren oder deaktivieren Sie die Volumeüberwachung
1. Wählen Sie auf der Seite **Geräte** das Gerät aus, doppelklicken Sie darauf, und klicken Sie dann auf die Registerkarte **Volumecontainer**.
2. Wählen Sie den Volumecontainer mit dem Volume aus, und klicken Sie dann auf den Volumecontainer, um die Seite **Volumes** anzuzeigen.
3. Alle diesem Container zugeordneten Volumes werden in Tabellenform angezeigt. Klicken Sie zum Auswählen auf das Volume oder den Volumeklon.
4. Klicken Sie unten auf der Seite auf **Ändern**.
5. Klicken Sie im Assistenten „Ändern von Volume“ unter **Grundeinstellungen** auf **Aktivieren** oder **Deaktivieren** in der Dropdownliste **Überwachen**.
   
    ![Ändern der Grundeinstellungen eines Volumes](./media/storsimple-manage-volumes/HCS_MonitorVolumeM.png)

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie Sie ein [StorSimple-Volume klonen](storsimple-clone-volume.md).
* Erfahren Sie, wie Sie [Ihr StorSimple-Gerät mithilfe des StorSimple Manager-Diensts verwalten](storsimple-manager-service-administration.md).


