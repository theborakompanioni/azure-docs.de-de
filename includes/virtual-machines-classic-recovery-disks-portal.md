Wenn für Ihren virtuellen Computer in Azure ein Start- oder Datenträgerfehler auftritt, müssen Sie unter Umständen Problembehandlungsschritte für die virtuelle Festplatte ausführen. Ein allgemeines Beispiel wäre etwa ein fehlerhaftes Anwendungsupdate, das den Start des virtuellen Computers verhindert. In diesem Artikel erfahren Sie, wie Sie über das Azure-Portal eine Verbindung zwischen Ihrer virtuellen Festplatte und einem anderen virtuellen Computer herstellen, alle Fehler beheben und anschließend den ursprünglichen virtuellen Computer neu erstellen.

## <a name="recovery-process-overview"></a>Übersicht über den Wiederherstellungsprozess
Der Problembehebungsprozess sieht wie folgt aus:

1. Löschen Sie den virtuellen Computer, auf dem Probleme auftreten, aber behalten Sie die virtuellen Festplatten bei.
2. Fügen Sie die virtuelle Festplatte zur Problembehandlung an einen anderen virtuellen Computer an, und binden Sie sie ein.
3. Stellen Sie eine Verbindung mit der Problembehebungs-VM her. Bearbeiten Sie Dateien, oder führen Sie Problembehandlungstools auf der ursprünglichen virtuellen Festplatte aus.
4. Heben Sie die Bereitstellung auf, und trennen Sie die virtuelle Festplatte von der Problembehebungs-VM.
5. Erstellen Sie einen virtuellen Computer unter Verwendung der ursprünglichen virtuellen Festplatte.

## <a name="delete-the-original-vm"></a>Löschen des ursprünglichen virtuellen Computers
Virtuelle Festplatten und VMs sind zwei unterschiedliche Ressourcen in Azure. Auf einer virtuellen Festplatte werden das Betriebssystem, Anwendungen und Konfigurationen gespeichert. Bei dem virtuellen Computer handelt es sich lediglich um Metadaten, die die Größe oder den Speicherort definieren und auf Ressourcen wie virtuelle Festplatten oder virtuelle Netzwerkschnittstellenkarten (NICs) verweisen. Jeder virtuellen Festplatte wird eine Lease zugewiesen, wenn sie an einen virtuellen Computer angefügt wird. Obwohl Datenträger auch bei laufendem Betrieb der VM hinzugefügt und getrennt werden können, kann der Betriebssystemdatenträger nicht getrennt werden, es sei denn, die VM-Ressource wird gelöscht. Die Lease ordnet den Betriebssystemdatenträger weiterhin einem virtuellen Computer zu, auch wenn der dieser beendet und die Zuordnung aufgehoben wurde.

Der erste Schritt beim Wiederherstellen Ihres virtuellen Computers besteht im Löschen der VM-Ressource. Das Löschen der VM belässt die virtuellen Festplatten in Ihrem Speicherkonto. Nach dem Löschen des virtuellen Computers können Sie die virtuelle Festplatte an einen anderen virtuellen Computer anfügen und die Fehler beheben. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an. 
2. Klicken Sie im Menü auf der linken Seite auf **Virtuelle Computer (klassisch)**.
3. Wählen Sie den virtuellen Computer aus, auf dem ein Problem auftritt, klicken Sie auf **Datenträger**, und ermitteln Sie anschließend den Namen der virtuellen Festplatte. 
4. Wählen Sie den virtuellen Betriebssystemdatenträger aus, und überprüfen Sie den **Speicherort**, um das Speicherkonto zu ermitteln, das die virtuelle Festplatte enthält. Im folgenden Beispiel ist die Zeichenfolge unmittelbar vor „.blob.core.windows.net“ der Name des Speicherkontos.

    ```
    https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd
    ```

    ![Abbildung zum Speicherort des virtuellen Computers](./media/virtual-machines-classic-recovery-disks-portal/vm-location.png)

5. Klicken Sie mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Löschen** aus. Vergewissern Sie sich beim Löschen des virtuellen Computers, dass die Datenträger nicht ausgewählt sind.
6. Erstellen Sie einen neuen virtuellen Wiederherstellungscomputer. Dieser virtuelle Computer muss sich in der gleichen Region und Ressourcengruppe (Clouddienst) befinden wie der virtuelle Computer, auf dem die Probleme aufgetreten sind.
7. Wählen Sie den virtuellen Wiederherstellungscomputer und anschließend **Datenträger** > **Vorhandenen anfügen** aus.
8. Klicken Sie auf **VHD-Datei**, um Ihre vorhandene virtuelle Festplatte auszuwählen:

    ![Navigieren zu einer vorhandenen VHD](./media/virtual-machines-classic-recovery-disks-portal/select-vhd-location.png)

9. Wählen Sie das Speicherkonto, den VHD-Container und die virtuelle Festplatte aus, und klicken Sie auf die Schaltfläche **Auswählen**, um Ihre Auswahl zu bestätigen.

    ![Auswählen Ihrer vorhandenen VHD](./media/virtual-machines-classic-recovery-disks-portal/select-vhd.png)

10. Klicken Sie nach dem Auswählen Ihrer virtuellen Festplatte auf **OK**, um die vorhandene virtuelle Festplatte anzufügen.
11. Nach wenigen Sekunden wird im Bereich **Datenträger** für Ihren virtuellen Computer angezeigt, dass die vorhandene virtuelle Festplatte als Datenträger verbunden ist:

    ![Vorhandene virtuelle Festplatte, die als Datenträger angefügt ist](./media/virtual-machines-classic-recovery-disks-portal/attached-disk.png)

## <a name="fix-issues-on-the-original-virtual-hard-disk"></a>Beheben von Problemen auf der ursprünglichen virtuellen Festplatte
Wenn die vorhandene virtuelle Festplatte eingebunden ist, können Sie die erforderlichen Wartungs- und Problembehandlungsschritte ausführen. Fahren Sie mit den folgenden Schritte fort, nachdem Sie die Probleme behoben haben.

## <a name="unmount-and-detach-the-original-virtual-hard-disk"></a>Aufheben der Bereitstellung und Trennen der ursprünglichen virtuellen Festplatte
Heben Sie nach der Behebung aller Fehler die Bereitstellung auf, und trennen Sie die vorhandene virtuelle Festplatte von Ihrem virtuellen Problembehandlungscomputer. Die virtuelle Festplatte kann erst wieder für einen anderen virtuellen Computer verwendet werden, nachdem die Lease freigegeben wurde, die die virtuelle Festplatte an den virtuellen Problembehandlungscomputer anfügt.  

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an. 
2. Wählen Sie im Menü auf der linken Seite die Option **Virtuelle Computer (klassisch)** aus.
3. Suchen Sie nach dem virtuellen Wiederherstellungscomputer. Wählen Sie „Datenträger“ aus, klicken Sie mit der rechten Maustaste auf den Datenträger, und wählen Sie anschließend **Trennen** aus.

## <a name="create-a-vm-from-the-original-hard-disk"></a>Erstellen eines virtuellen Computers auf der Grundlage der ursprünglichen Festplatte

Verwenden Sie das [klassische Azure-Portal](https://manage.windowsazure.com), um einen virtuellen Computers auf der Grundlage der ursprünglichen Festplatte zu erstellen.

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) an.
2. Wählen Sie am unteren Rand des Portals **Neu** > **Compute** > **Virtueller Computer** > **Aus Katalog** aus.
3. Wählen Sie im Abschnitt **Image auswählen** die Option für Ihre eigenen Datenträger**** und anschließend die ursprüngliche virtuelle Festplatte aus. Überprüfen Sie den Standort. Hierbei handelt es sich um die Region, in der der virtuelle Computer bereitgestellt werden muss. Wählen Sie „Weiter“ aus.
4. Geben Sie im Abschnitt **Konfiguration des virtuellen Computers** den Namen des virtuellen Computers ein, und wählen Sie eine Größe für den virtuellen Computer aus.