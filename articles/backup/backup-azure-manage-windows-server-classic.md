---
title: Verwalten von Azure Backup-Tresoren und -Servern mit dem klassischen Bereitstellungsmodell | Microsoft Docs
description: In diesem Tutorial erfahren Sie, wie Sie Azure Backup-Tresore und -Server verwalten.
services: backup
documentationcenter: ''
author: markgalioto
manager: jwhit
editor: tysonn

ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: jimpark;markgal

---
# <a name="manage-azure-backup-vaults-and-servers-using-the-classic-deployment-model"></a>Verwalten von Azure Backup-Tresoren und -Servern mit dem klassischen Bereitstellungsmodell
> [!div class="op_single_selector"]
> * [Ressourcen-Manager](backup-azure-manage-windows-server.md)
> * [Klassisch](backup-azure-manage-windows-server-classic.md)
> 
> 

In diesem Artikel finden Sie eine Übersicht der Aufgaben im Rahmen der Sicherungsverwaltung, die über das klassische Azure-Portal und den Microsoft Azure Backup-Agent zur Verfügung stehen.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Ressourcen-Manager-Bereitstellungsmodell

## <a name="management-portal-tasks"></a>Aufgaben im Verwaltungsportal
1. Melden Sie sich im [Verwaltungsportal](https://manage.windowsazure.com)an.
2. Klicken Sie auf **Wiederherstellungsdienste**und dann auf den Namen des Sicherungstresors, um die Seite "Schnellstart" anzuzeigen.
   
    ![Wiederherstellungsdienste](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

Über die Optionen oben auf der Schnellstartseite können Sie die verfügbaren Verwaltungsaufgaben anzeigen.

![Registerkarten verwalten](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a>Dashboard
Klicken Sie auf **Dashboard** , um die Nutzungsübersicht für den Server anzuzeigen. In der **Nutzungsübersicht** finden Sie Folgendes:

* Die Anzahl der in der Cloud registrierten Windows-Server
* Die Anzahl der in der Cloud geschützten virtuelle Azure-Computer
* Die insgesamt in Azure verwendete Speichermenge
* Den Status der letzten Aufträge

Unten im Dashboard können Sie folgende Aufgaben ausführen:

* **Zertifikat verwalten** : Wenn ein Zertifikat zum Registrieren des Servers verwendet wurde, können Sie hierüber das Zertifikat aktualisieren. Wenn Sie Tresoranmeldeinformationen verwenden, verwenden Sie **Zertifikat verwalten**nicht.
* **Löschen** : Löscht den aktuellen Sicherungstresor. Wenn ein Sicherungstresor nicht mehr verwendet wird, können Sie ihn löschen, um Speicherplatz freizugeben. **Löschen** wird erst aktiviert, wenn alle registrierten Server aus dem Tresor gelöscht wurden.

![Aufgaben im Sicherungsdashboard](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a>Registrierte Elemente
Klicken Sie auf **Registrierte Elemente** , um die Namen der Server anzuzeigen, die für diesen Tresor registriert sind.

![Registrierte Elemente](./media/backup-azure-manage-windows-server-classic/registered-items.png)

Der Filter **Typ** ist standardmäßig auf virtuelle Azure-Computer festgelegt. Wählen Sie im Dropdownmenü die Einstellung **Windows-Server** aus, um die Namen der Server anzuzeigen, die für diesen Tresor registriert sind.

Jetzt können Sie folgende Aufgaben ausführen:

* **Erneute Registrierung zulassen**: Wenn diese Option für einen Server ausgewählt ist, können Sie den **Registrierungs-Assistenten** im lokalen Microsoft Azure Backup-Agent verwenden, um den Server erneut für den Sicherungstresor zu registrieren. Eine erneute Registrierung ist möglicherweise nötig, wenn im Zertifikat ein Fehler auftritt oder wenn ein Server erneut erstellt werden muss.
* **Löschen** – Dies wird verwendet, um einen Server aus dem Sicherungstresor zu löschen. Alle gespeicherten Daten dieses Servers werden sofort gelöscht.
  
    ![Aufgaben für registrierte Elemente](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a>Geschützte Elemente
Klicken Sie auf **Geschützte Elemente** , um die Elemente anzuzeigen, die von den Servern gesichert wurden.

![Geschützte Elemente](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a>Konfigurieren
Auf der Registerkarte **Konfigurieren** können Sie die geeignete Speicherredundanzoption auswählen. Der beste Zeitpunkt zum Auswählen der Speicherredundanzoption ist direkt nach dem Erstellen eines Tresors und vor dem Registrieren von Computern für den Tresor.

> [!WARNING]
> Sobald ein Element beim Tresor registriert wurde, wird die Speicherredundanzoption gesperrt und kann nicht mehr geändert werden.
> 
> 

![Konfigurieren](./media/backup-azure-manage-windows-server-classic/configure.png)

Weitere Informationen finden Sie im Artikel zur [Speicherredundanz](../storage/storage-redundancy.md).

## <a name="microsoft-azure-backup-agent-tasks"></a>Aufgaben im Microsoft Azure Backup-Agent
### <a name="console"></a>Konsole
Öffnen Sie den **Microsoft Azure Backup-Agent**. (Sie finden den Agent, indem Sie auf Ihrem Computer nach *Microsoft Azure Backup* suchen.)

![Backup-Agent](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

Über die im rechten Bereich der Backup-Agent-Konsole angezeigten **Aktionen** können Sie die folgenden Verwaltungsaufgaben ausführen:

* Server registrieren
* Sicherung planen
* Jetzt sichern
* Eigenschaften ändern

![Aktionen der Agent-Konsole](./media/backup-azure-manage-windows-server-classic/console-actions.png)

> [!NOTE]
> Informationen zum **Wiederherstellen von Daten**finden Sie unter [Wiederherstellen von Dateien auf einem Windows-Server- oder Windows-Clientcomputer mit dem Resource Manager-Bereitstellungsmodell](backup-azure-restore-windows-server.md).
> 
> 

### <a name="modify-an-existing-backup"></a>Ändern einer vorhandenen Sicherung
1. Klicken Sie im Microsoft Azure Backup-Agent auf **Sicherung planen**.
   
    ![Planen einer Windows Server-Sicherung](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
2. Lassen Sie im **Assistenten zum Planen von Sicherungen** die Option **Sicherungselemente oder -zeiten ändern** aktiviert, und klicken Sie auf **Weiter**.
   
    ![Geplante Sicherung ändern](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
3. Wenn Sie Elemente hinzufügen oder ändern möchten, klicken Sie auf dem Bildschirm **Elemente für die Sicherung auswählen** auf **Elemente hinzufügen**.
   
    Sie können auf dieser Seite des Assistenten auch **Ausschlusseinstellungen** festlegen. Wenn Sie Dateien oder Dateitypen ausschließen möchten, lesen Sie das Verfahren zum Hinzufügen von [Ausschlusseinstellungen](#exclusion-settings).
4. Wählen Sie die Dateien und Ordner aus, die Sie sichern möchten, und klicken Sie auf **OK**.
   
    ![Elemente hinzufügen](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)
5. Geben Sie den **Sicherungszeitplan** an, und klicken Sie auf **Weiter**.
   
    Sie können tägliche (maximal drei pro Tag) oder wöchentliche Sicherungen planen.
   
    ![Sicherungszeitplan angeben](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)
   
   > [!NOTE]
   > Die Festlegung des Sicherungszeitplans wird in diesem [Artikel](backup-azure-backup-cloud-as-tape.md)ausführlich beschrieben.
   > 
   > 
6. Wählen Sie die **Aufbewahrungsrichtlinie** für die Sicherungskopie aus, und klicken Sie auf **Weiter**.
   
    ![Aufbewahrungsrichtlinie auswählen](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)
7. Lesen Sie sich die Informationen auf dem Bildschirm **Bestätigung** durch, und klicken Sie auf **Fertig stellen**.
8. Nachdem der Assistent die Erstellung des **Sicherungszeitplans** abgeschlossen hat, klicken Sie auf **Schließen**.
   
    Nach dem Ändern der Einstellungen können Sie die richtige Auslösung der Sicherungen überprüfen, indem Sie zur Registerkarte **Aufträge** wechseln und sich vergewissern, dass die Änderungen in den Sicherungsaufträgen übernommen wurden.

### <a name="enable-network-throttling"></a>Aktivieren der Netzwerkdrosselung
Der Azure Backup-Agent verfügt über eine Registerkarte für die Drosselung, mit der Sie steuern können, wie die Netzwerkbandbreite während der Datenübertragung verwendet wird. Diese Steuerungsmöglichkeit kann hilfreich sein, wenn Sie Daten während der Geschäftszeiten sichern möchten, der Sicherungsprozess aber keine Auswirkung auf den weiteren Internetdatenverkehr haben soll. Die Drosselung der Datenübertragung gilt für Sicherungs- und Wiederherstellungsaktivitäten.  

So aktivieren Sie die Drosselung

1. Klicken Sie im **Backup-Agent** auf **Eigenschaften ändern**.
2. Aktivieren Sie das Kontrollkästchen **Internet-Bandbreiteneinschränkung für Sicherungsvorgänge aktivieren** .
   
    ![Netzwerkdrosselung](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)
3. Nachdem Sie die Drosselung aktiviert haben, geben Sie die zulässige Bandbreite für die Sicherungsdatenübertragung für **Arbeitsstunden** und **Arbeitsfreie Stunden** an.
   
    Die Bandbreitenwerte beginnen bei 512 Kilobytes pro Sekunde (KBit/s) und können mit bis zu 1023 Megabytes pro Sekunde (MBit/s) angegeben werden. Sie können außerdem den Start und das Ende für die **Arbeitsstunden**angeben und festlegen, welche Tage der Woche als Arbeitstage betrachtet werden. Die Zeiten außerhalb der angegebenen Arbeitsstunden werden als arbeitsfreie Stunden betrachtet.
4. Klicken Sie auf **OK**.

## <a name="exclusion-settings"></a>Ausschlusseinstellungen
1. Öffnen Sie den **Microsoft Azure Backup-Agent**. (Sie finden den Agent, indem Sie auf Ihrem Computer nach *Microsoft Azure Backup* suchen.)
   
    ![Backup-Agent öffnen](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)
2. Klicken Sie im Microsoft Azure Backup-Agent auf **Sicherung planen**.
   
    ![Planen einer Windows Server-Sicherung](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
3. Lassen Sie im Assistenten zum Planen von Sicherungen die Option **Sicherungselemente oder -zeiten ändern** aktiviert, und klicken Sie auf **Weiter**.
   
    ![Zeitplan ändern](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
4. Klicken Sie auf **Ausschlusseinstellungen**.
   
    ![Auszuschließende Elemente auswählen](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)
5. Klicken Sie auf **Ausschluss hinzufügen**.
   
    ![Ausschlüsse hinzufügen](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)
6. Wählen Sie den Speicherort aus, und klicken Sie dann auf **OK**.
   
    ![Speicherort für den Ausschluss auswählen](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)
7. Fügen Sie im Feld **Dateityp** die Dateierweiterung hinzu.
   
    ![Nach Dateityp ausschließen](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)
   
    Hinzufügen einer MP3-Erweiterung
   
    ![Beispiel für einen Dateityp](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)
   
    Um eine weitere Erweiterung hinzuzufügen, klicken Sie auf **Ausschluss hinzufügen** und geben eine weitere Dateityperweiterung ein (hier wird die JPEG-Erweiterung hinzugefügt).
   
    ![Weiteres Beispiel für einen Dateityp](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)
8. Wenn Sie alle Erweiterungen hinzugefügt haben, klicken Sie auf **OK**.
9. Setzen Sie den Assistenten zum Planen von Sicherungen fort, indem Sie auf **Weiter** klicken, bis die Seite **Bestätigung** angezeigt wird. Klicken Sie anschließend auf **Fertig stellen**.
   
    ![Ausschlussbestätigung](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a>Nächste Schritte
* [Wiederherstellen von Windows-Servern oder Windows-Clients aus Azure](backup-azure-restore-windows-server.md)
* Weitere Informationen zu Azure Backup finden Sie unter [Azure Backup – Übersicht](backup-introduction-to-azure-backup.md)
* Besuchen Sie das [Azure Backup-Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933)

<!--HONumber=Oct16_HO2-->


