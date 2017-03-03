---
title: Konfigurieren von MPIO auf dem Host, der mit dem StorSimple Virtual Array verbunden ist | Microsoft-Dokumentation
description: "Beschreibt, wie Sie Multipfad-E/A (MPIO) für Ihr StorSimple Virtual Array konfigurieren, das mit einem Host unter Windows Server 2012 R2 verbunden ist."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5b7a7f99-ee5b-4b7d-ab32-483a5a1fa504
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: c78eda33a64d630ba18aba6a174db372eb41dde9
ms.openlocfilehash: 24c2670394c2873b333f41c081a8134710d16d54
ms.lasthandoff: 01/26/2017

---
# <a name="configure-multipath-io-on-windows-server-host-for-the-storsimple-virtual-array"></a>Konfigurieren von Multipfad-E/A auf Windows Server-Host für das virtuelle StorSimple-Array
## <a name="overview"></a>Übersicht
Dieser Artikel beschreibt die Installation der Funktion für Multipfad-E/A (MPIO, Multipath I/O) auf dem Windows Server-Host, die Anwendung bestimmter Konfigurationseinstellungen für reine StorSimple-Volumes und die MPIO-Überprüfung für StorSimple-Volumes. Das Verfahren setzt voraus, dass Ihr virtuelles StorSimple 1200-Array mit zwei Netzwerkschnittstellen mit einem Windows Server-Host mit zwei Netzwerkschnittstellen verbunden ist. Die in diesem Artikel enthaltenen Informationen gelten nur für das virtuelle Array. Informationen zu Geräten der StorSimple 8000-Serie finden Sie unter [Konfigurieren von Multipfad-E/A für Ihr StorSimple-Gerät](storsimple-configure-mpio-windows-server.md). 

Die MPIO-Funktion in Windows Server unterstützt das Erstellen hoch verfügbarer, fehlertoleranter Speicherkonfigurationen. MPIO verwendet redundante physische Pfadkomponenten (Adapter, Kabel und Switches), um logische Pfade zwischen dem Server und dem Speichergerät zu erstellen. Wenn bei einer Komponente ein Fehler auftritt, durch den ein logischer Pfad fehlschlägt, verwendet die Multipfad-Logik einen anderen Pfad für E/A, sodass Anwendungen weiterhin auf ihre Daten zugreifen können. Darüber hinaus kann MPIO abhängig von Ihrer Konfiguration auch die Leistung durch ein Umverteilen der Lasten auf alle Pfade verbessern. Weitere Informationen finden Sie unter [Multipfad-E/A (Übersicht)](https://technet.microsoft.com/library/cc725907.aspx "Multipfad-E/A (Übersicht) and features").

Konfigurieren Sie MPIO für die hohe Verfügbarkeit Ihrer StorSimple-Lösung auf den Windows Server-Hosts, die mit Ihrem StorSimple Virtual Array (1200er-Modell) verbunden sind. Die Hostserver können dann einen Verknüpfungs-, Netzwerk oder Schnittstellenfehler tolerieren. 

Führen Sie die folgenden Schritte aus, um MPIO zu konfigurieren: 

* Konfigurationsvoraussetzungen
* Schritt 1: Installieren von MPIO auf dem Windows Server-Host
* Schritt 2: Konfigurieren von MPIO für StorSimple-Volumes
* Schritt 3: Bereitstellen von StorSimple-Volumes auf dem Host

Jeder der oben genannten Schritte wird in den folgenden Abschnitten erläutert.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Abschnitt werden die Konfigurationsvoraussetzungen für den Windows Server-Host und Ihr virtuelles Array im Detail beschrieben.

### <a name="on-windows-server-host"></a>Auf dem Windows Server-Host
* Stellen Sie sicher, dass Ihr Windows Server-Host über zwei aktivierte Netzwerkschnittstellen verfügt.

### <a name="on-storsimple-virtual-array"></a>Auf dem StorSimple Virtual Array
* Das virtuelle Array sollte als iSCSI-Server konfiguriert werden. Weitere Informationen finden Sie unter [Bereitstellen von StorSimple Virtual Array – Einrichten des virtuellen Geräts als iSCSI-Server](storsimple-virtual-array-deploy3-iscsi-setup.md). Mindestens eine Netzwerkschnittstelle sollte auf dem Array aktiviert sein.   
* Die Netzwerkschnittstellen auf Ihrem virtuellen Array sollten vom Windows Server-Host aus erreichbar sein.
* Mindestens ein Volume sollte auf Ihrem virtuellen StorSimple-Array erstellt werden. Weitere Informationen zum Hinzufügen eines Volume auf Ihrem StorSimple Virtual Array finden Sie unter [Hinzufügen eines Volumes](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume). In diesem Verfahren haben wir 3 Volumes (1 lokales und 2 mehrstufige Volumes, wie unten gezeigt) auf dem virtuellen Array erstellt.
  
    ![mpio0](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>Hardwarekonfiguration für StorSimple Virtual Array
Die nachstehende Abbildung zeigt die Hardwarekonfiguration für hohe Verfügbarkeit und Lastenausgleich mit Multipfad für Ihren Windows Server-Host und das in diesem Verfahren verwendete StorSimple Virtual Array.

![MPIO-Hardwarekonfiguration](./media/storsimple-virtual-array-configure-mpio-windows-server/1200hardwareconfig.png)

Die Abbildung oben zeigt Folgendes:

* Das auf Hyper-V bereitgestellte StorSimple Virtual Array ist ein aktives Einzelknotengerät, das als iSCSI-Server konfiguriert ist.
* Auf Ihrem Array sind zwei virtuelle Netzwerkschnittstellen aktiviert. Überprüfen Sie in der lokalen Webbenutzeroberfläche Ihres virtuellen Arrays, ob zwei Netzwerkschnittstellen aktiviert sind, indem Sie wie unten dargestellt zu den **Netzwerkeinstellungen** wechseln:
  
    ![Auf 1200 aktivierte Netzwerkschnittstellen](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio9.png)
  
    Notieren Sie sich die IPv4-Adressen der aktivierten Netzwerkschnittstellen (Ethernet, Ethernet 2 in der Standardeinstellung), und legen Sie die Notiz für die spätere Verwendung auf dem Host zurück.
* Auf Ihrem Windows Server-Host sind zwei Netzwerkschnittstellen aktiviert. Wenn die verbundenen Schnittstellen für Host und Array sich im gleichen Subnetz befinden, dann sind 4 Pfade verfügbar. Dies ist in diesem Verfahren der Fall. Wenn sich jedoch jede Netzwerkschnittstelle des Arrays und die Hostschnittstelle in einem unterschiedlichen IP-Subnetz befinden (und nicht routingfähig sind), stehen nur 2 Pfade zur Verfügung.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Schritt 1: Installieren von MPIO auf dem Windows Server-Host
MPIO ist ein optionales Feature und wird unter Windows Server nicht standardmäßig installiert. Diese Funktion sollte als Feature über den Server-Manager installiert werden. Gehen Sie folgendermaßen vor, um dieses Feature auf Ihrem Windows Server-Host zu installieren.

[!INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Schritt 2: Konfigurieren von MPIO für StorSimple-Volumes
MPIO muss konfiguriert werden, um StorSimple-Volumes zu erkennen. Führen Sie zum Konfigurieren von MPIO für die Erkennung von StorSimple-Geräten die folgenden Schritte aus.

[!INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Schritt 3: Bereitstellen von StorSimple-Volumes auf dem Host
Nachdem MPIO unter Windows Server konfiguriert wurde, können auf dem StorSimple-Array erstellte Volumes bereitgestellt werden und anschließend MPIO für Redundanz nutzen. Führen Sie die folgenden Schritte aus, um ein Volume bereitzustellen.

#### <a name="to-mount-volumes-on-the-host"></a>So stellen Sie Volumes auf dem Host bereit
1. Öffnen Sie das Fenster **Eigenschaften des iSCSI-Initiators** auf dem Windows Server-Host. Klicken Sie auf **Server-Manager > Dashboard > Tools > iSCSI-Initiator**.
2. Klicken Sie im Dialogfeld **Eigenschaften des iSCSI-Initiators** auf **Erkennung**, und klicken Sie anschließend auf **Zielportal ermitteln**.
3. Gehen Sie im Dialogfeld **Zielportal ermitteln** folgendermaßen vor:
   
    1. Geben Sie die IP-Adresse der ersten aktivierten Netzwerkschnittstelle auf Ihrem StorSimple Virtual Array ein. Standardmäßig wäre dies die **Ethernet**-Adresse. 
    2. Klicken Sie auf **OK** , um zum Dialogfeld **Eigenschaften des iSCSI-Initiators** zurückzukehren.
     
    > [!IMPORTANT]
    > Wenn Sie ein privates Netzwerk für iSCSI-Verbindungen verwenden, geben Sie die IP-Adresse des DATA-Ports ein, der mit dem privaten Netzwerk verbunden ist.
     
4. Wiederholen Sie die Schritte 2 bis 3 für eine zweite Netzwerkschnittstelle (z. B. Ethernet 2) auf Ihrem Array. 
5. Wählen Sie die Registerkarte **Ziele** im Dialogfeld **Eigenschaften des iSCSI-Initiators** aus. Für das virtuelle Array sollten Sie jede Volumeoberfläche als Ziel unter **Erkannte Ziele**sehen. In diesem Fall würden drei Ziele (entspricht drei Volumes) erkannt werden.
   
    ![mpio1](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio1.png)
6. Klicken Sie auf **Verbinden**, um eine iSCSI-Sitzung mit Ihrem StorSimple Virtual Array einzurichten. Ein Dialogfeld **Mit Ziel verbinden** wird angezeigt. Aktivieren Sie das Kontrollkästchen **Multipfad aktivieren** . Klicken Sie auf **Erweitert**.
   
    ![mpio2](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio2.png)
7. Gehen Sie im Dialogfeld **Erweiterte Eigenschaften** folgendermaßen vor:                                        
   
    1. Wählen Sie in der Dropdownliste **Lokaler Adapter** die Option **Microsoft iSCSI-Initiator** aus.
    2. Wählen Sie die IP-Adresse des Hosts in der Dropdownliste **Initiator-IP** aus.
    3. Wählen Sie die IP-Adresse der Arrayschnittstelle in der Dropdownliste **Zielportal-IP** aus.
    4. Klicken Sie auf **OK** , um zum Dialogfeld **Eigenschaften des iSCSI-Initiators** zurückzukehren.
     
        ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

8. Klicken Sie auf **Eigenschaften**. 
   
    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)

9. Klicken Sie im Dialogfeld **Eigenschaften** auf **Sitzung hinzufügen**.
   
   ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)
10. Aktivieren Sie im Dialogfeld **Mit Ziel verbinden** das Kontrollkästchen **Multipfad aktivieren**. Klicken Sie auf **Erweitert**.
11. Gehen Sie im Dialogfeld **Erweiterte Einstellungen** folgendermaßen vor:                                        
    
    1. Wählen Sie "Microsoft iSCSI-Initiator" in der Dropdownliste **Lokaler Adapter** aus.

    2. Wählen Sie die IP-Adresse, die dem Host entspricht, in der Dropdownliste **Initiator-IP** aus. In diesem Fall werden zwei Netzwerkschnittstellen auf dem Array mit einer einzelnen Netzwerkschnittstelle auf dem Host verbunden. Aus diesem Grund handelt es sich bei dieser Schnittstelle um die gleiche Schnittstelle, die für die erste Sitzung bereitgestellt wurde.

    3. Wählen Sie die IP-Adresse für die zweite Datenschnittstelle, die auf dem Array aktiviert ist, in der Dropdownliste **Zielportal-IP** aus.

    4. Klicken Sie auf **OK** , um zum Dialogfeld "Eigenschaften des iSCSI-Initiators" zurückzukehren. Sie haben dem Ziel eine zweite Sitzung hinzugefügt.
      
       ![mpio11](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio11.png)
    
    5. Nachdem Sie die gewünschten Sitzungen (Pfade) hinzugefügt haben, wählen Sie das Ziel im Dialogfeld **Eigenschaften des iSCSI-Initiators** aus, und klicken dann auf **Eigenschaften**. Beachten Sie auf der Registerkarte "Sitzungen" des Dialogfelds **Eigenschaften** die vier Sitzungsbezeichner, die den möglichen Pfadpermutationen entsprechen. Wenn Sie eine Sitzung abbrechen möchten, aktivieren Sie das Kontrollkästchen neben einem Sitzungsbezeichner, und klicken Sie dann auf **Trennen**.

    6. Wenn Sie Geräte in den Sitzungen anzeigen möchten, wählen Sie die Registerkarte **Geräte** aus. Klicken Sie zum Konfigurieren der MPIO-Richtlinie für ein ausgewähltes Gerät auf **MPIO**.

    7. Das Dialogfenster **Details** wird angezeigt. Auf der Registerkarte **MPIO** können Sie die entsprechenden Einstellungen für die **Lastenausgleichsrichtlinie** auswählen. Sie können auch den Pfadtyp **Aktiv** oder **Standby** anzeigen.
12. Wiederholen Sie die Schritte 8 bis 11, um dem Ziel weitere Sitzungen (Pfade) hinzuzufügen. Mit zwei Schnittstellen auf dem Host und zwei Schnittstellen auf dem virtuellen Array können Sie insgesamt vier Sitzungen für jedes Ziel hinzufügen. 
    
    ![mpio14](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio14.png)
13. Sie müssen diese Schritte für jedes Volume (Oberflächen als Ziel) wiederholen.
    
    ![mpio15](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio15.png)
14. Öffnen Sie die **Computerverwaltung**, indem Sie zu **Server-Manager > Dashboard > Computerverwaltung** navigieren. Klicken Sie im linken Bereich auf **Speicher > Datenträgerverwaltung**. Die auf dem StorSimple Virtual Array erstellten und für diesen Host sichtbaren Volumes werden unter **Datenträgerverwaltung** als neue/r Datenträger angezeigt.
15. Initialisieren Sie den Datenträger, und erstellen Sie dann ein neues Volume. Wählen Sie während des Formatierungsvorgangs eine Zuordnungseinheitengröße (AUS, Allocation Unit Size) von 64 KB. Wiederholen Sie den Vorgang für alle verfügbaren Volumes.
    
    ![Datenträgerverwaltung](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio20.png)
16. Klicken Sie unter **Datenträgerverwaltung** mit der rechten Maustaste auf den **Datenträger**, und wählen Sie dann **Eigenschaften** aus.
17. Klicken Sie im Dialogfeld **Eigenschaften von Multipfad-Datenträgergerät** auf die Registerkarte **MPIO**.
    
    ![Datenträgereigenschaften](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio21.png)
18. Klicken Sie im Abschnitt **DSM-Name** auf **Details**, und vergewissern Sie sich, dass die Parameter auf die Standardparameter festgelegt sind. Die Standardparameter lauten:
    
    * Pfadüberprüfungszeitraum = 30
    * Anzahl der Wiederholungsversuche = 3
    * Zeitraum für das Entfernen von PDO = 20
    * Wiederholungsintervall = 1
    * Pfadüberprüfung aktiviert = nicht aktiviert.
    
    > [!NOTE]
    > **Ändern Sie die Standardparameter nicht.**
   
## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Use the StorSimple Device Manager service to administer your StorSimple Virtual Array (Verwalten des StorSimple Virtual Array mithilfe des StorSimple Manager-Diensts)](storsimple-virtual-array-manager-service-administration.md).


