


## Suchen des virtuellen Computers

1. Melden Sie sich beim Azure-Portal an.

2. Klicken Sie im Hub-Menü auf **Virtuelle Computer**.

3.	Wählen Sie den gewünschten virtuellen Computer aus der Liste aus.

4. Klicken Sie rechts unter **Zusammenfassung** auf **Alle Einstellungen**, und klicken Sie anschließend auf **Datenträger**.

	![Öffnen der Datenträgereinstellungen](./media/virtual-machines-common-attach-disk-portal/find-disk-settings.png)

Folgen Sie den Anweisungen zum Anfügen eines neuen oder eines vorhandenen Datenträgers.

## Option 1: Anfügen eines neuen Datenträgers

1.	Klicken Sie auf dem Blatt **Datenträger** auf **Neuen anfügen**.

2.	Überprüfen und aktualisieren Sie nach Bedarf die Standardeinstellungen, und klicken Sie anschließend auf **OK**.

 	![Überprüfen der Datenträgereinstellungen](./media/virtual-machines-common-attach-disk-portal/attach-new.png)

3.	Nachdem der Datenträger von Azure erstellt und an den virtuellen Computer angefügt wurde, wird der neue Datenträger in den Datenträgereinstellungen des virtuellen Computers unter **Datenträger** angezeigt.

## Option 2: Anfügen eines vorhandenen Datenträgers

1.	Klicken Sie auf dem Blatt **Datenträger** auf **Vorhandenen anfügen**.

2.	Klicken Sie unter **Vorhandenen Datenträger anfügen** auf **VHD-Datei**.

	![Anfügen eines vorhandenen Datenträgers](./media/virtual-machines-common-attach-disk-portal/attach-existing.png)

3.	Wählen Sie unter **Speicherkonten** das Konto und den Container aus, der die VHD-Datei enthält.

	![Suchen nach VHD-Dateien](./media/virtual-machines-common-attach-disk-portal/find-storage-container.png)

4.	Wählen Sie die VHD-Datei aus.

5.	Die ausgewählte Datei wird unter **Vorhandenen Datenträger anfügen** und dort unter **VHD-Datei** angezeigt. Klicken Sie auf **OK**.

6.	Nachdem der Datenträger von Azure an den virtuellen Computer angefügt wurde, wird er in den Datenträgereinstellungen des virtuellen Computers unter **Datenträger** angezeigt.

<!---HONumber=AcomDC_0330_2016-->