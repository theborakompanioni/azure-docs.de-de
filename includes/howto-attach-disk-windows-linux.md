


## <a name="attach-an-empty-disk"></a>Anfügen eines leeren Datenträgers
Eine einfachere Methode zum Hinzufügen eines Datenträgers besteht im Anfügen eines leeren Datenträgers, weil Azure die VHD-Datei automatisch erstellt und sie im Speicherkonto ablegt.

1. Klicken Sie auf **Virtuelle Computer (klassisch)**, und wählen Sie dann den betreffenden virtuellen Computer aus.

2. Klicken Sie im Menü „Einstellungen“ auf **Datenträger**.

   ![Neuen leeren Datenträger anfügen](./media/howto-attach-disk-windows-linux/menudisksattachnew.png)

3. Klicken Sie auf der Befehlsleiste auf **Neuen anfügen**.  
    Das Dialogfeld **Neuen Datenträger anfügen** wird angezeigt.

    ![Anfügen eines neuen Datenträgers](./media/howto-attach-disk-windows-linux/newdiskdetail.png)

    Geben Sie die folgenden Informationen ein:
    - Übernehmen Sie in **Dateiname**den Standardnamen, oder geben Sie einen anderen Namen für die VHD-Datei ein. Der Datenträger verwendet einen automatisch generierten Namen, selbst wenn Sie einen anderen Namen für die VHD-Datei eingeben.
    - Wählen Sie den **Typ** des Datenträger aus. Alle virtuellen Computer unterstützen Standarddatenträgern. Viele virtuelle Computer unterstützen auch Premium-Datenträger.
    - Wählen Sie die **Größe (GB)** des Datenträgers.
    - Wählen Sie für **Hostzwischenspeicherung** die Option „Keine“ oder „Schreibgeschützt“ aus.
    - Klicken Sie auf „OK“, um den Vorgang abzuschließen.

4. Nachdem der Datenträger erstellt und angefügt ist, wird er im Datenträgerbereich des virtuellen Computers aufgeführt.

   ![Neuer und leerer Datenträger erfolgreich angefügt](./media/howto-attach-disk-windows-linux/newdiskemptysuccessful.png)

> [!NOTE]
> Nachdem Sie einen Datenträger hinzugefügt haben, müssen Sie sich beim virtuellen Computer anmelden und den Datenträger initialisieren, sodass dieser verwendet werden kann.

## <a name="how-to-attach-an-existing-disk"></a>Gewusst wie: Anfügen eines vorhandenen Datenträgers
Zum Anfügen eines vorhandenen Datenträgers wird eine VHD-Datei im Speicherkonto benötigt. Verwenden Sie das Cmdlet [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) , um die VHD-Datei in das Speicherkonto hochzuladen. Nachdem Sie eine VHD-Datei erstellt und hochgeladen haben, können Sie sie an einen virtuellen Computer anfügen.

1. Klicken Sie auf **Virtuelle Computer (klassisch)**, und wählen Sie dann den betreffenden virtuellen Computer aus.

2. Klicken Sie im Menü „Einstellungen“ auf **Datenträger**.

3. Klicken Sie auf der Befehlsleiste auf **Vorhandenen anfügen**.

    ![Datenträger anfügen](./media/howto-attach-disk-windows-linux/menudisksattachexisting.png)

4. Klicken Sie auf **Speicherort**. Die verfügbaren Speicherkonten werden angezeigt. Wählen Sie als Nächstes ein geeignetes Speicherkonto aus der Liste aus.

    ![Angeben eines Speicherkontos für den Datenträger](./media/howto-attach-disk-windows-linux/existdiskstorageaccounts.png)

5. Ein **Speicherkonto** enthält mindestens einen Container mit Festplatten (VHDs). Wählen Sie den geeigneten Container aus der Liste aus.

    ![Bereitstellen des Containers von „virtual-machines-windows“](./media/howto-attach-disk-windows-linux/existdiskcontainers.png)

6. Im Bereich **VHDs** sind die im Container enthaltenen Festplattenlaufwerke aufgeführt. Klicken Sie auf einen der Datenträger und dann auf „Auswählen“.

    ![Bereitstellen des Datenträgerimages von „virtual-machines-windows“](./media/howto-attach-disk-windows-linux/existdiskvhds.png)

7. Der Bereich **Anfügen eines vorhandenen Datenträgers** wird erneut angezeigt und enthält den Speicherort mit dem Speicherkonto, Container und der ausgewählte Festplatte (VHD), die dem virtuellen Computer hinzugefügt werden sollen.

  Legen Sie für **Hostzwischenspeicherung** die Option „Keine“ oder „Schreibgeschützt“ fest, und klicken Sie dann auf „OK“.

    ![Datenträger erfolgreich angefügt](./media/howto-attach-disk-windows-linux/exisitingdisksuccessful.png)
