In Azure ist jetzt Unterstützung von zwei Debuggingfunktionen verfügbar: Konsolenausgaben- und Screenshotunterstützung für das Resource Manager-Bereitstellungsmodell für virtuelle Azure-Computer. 

Wenn Sie Ihr eigenes Image in Azure verwenden oder sogar eines der Plattformimages starten, kann sich ein virtueller Computer aus zahlreichen Gründen in einem nicht startfähigen Zustand befinden. Diese Funktionen ermöglichen Ihnen ein einfaches Diagnostizieren und Wiederherstellten Ihrer virtuellen Computer nach Startfehlern.

Für virtuelle Linux-Computer können Sie die Ausgabe des Konsolenprotokolls problemlos über das Portal anzeigen:

![Azure-Portal](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)
 
Azure ermöglicht Ihnen jedoch sowohl für virtuelle Windows- als auch Linux-Computer auch das Anzeigen eines Screenshots des virtuellen Computers im Hypervisor:

![Error](./media/virtual-machines-common-boot-diagnostics/screenshot2.png)

Beide Funktionen werden für virtuelle Azure-Computer in allen Regionen unterstützt. Hinweis: Es kann bis zu 10 Minuten dauern, bis Screenshots und Ausgaben in Ihrem Speicherkonto angezeigt werden.

## <a name="common-boot-errors"></a>Häufige Startfehler

- [0xC000000E](https://support.microsoft.com/help/4010129)
- [0xC000000F](https://support.microsoft.com/help/4010130)
- [0xC0000011](https://support.microsoft.com/help/4010134)
- [0xC0000034](https://support.microsoft.com/help/4010140)
- [0xC0000098](https://support.microsoft.com/help/4010137)
- [0xC00000BA](https://support.microsoft.com/help/4010136)
- [0xC000014C](https://support.microsoft.com/help/4010141)
- [0xC0000221](https://support.microsoft.com/help/4010132)
- [0xC0000225](https://support.microsoft.com/help/4010138)
- [0xC0000359](https://support.microsoft.com/help/4010135)
- [0xC0000605](https://support.microsoft.com/help/4010131)
- [Betriebssystem nicht gefunden](https://support.microsoft.com/help/4010142)
- [Startfehler oder INACCESSIBLE_BOOT_DEVICE](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Aktivieren der Diagnose auf einem virtuellen Computer
1. Wählen Sie beim Erstellen eines neuen virtuellen Computers über das Vorschauportal in der Dropdownliste „Bereitstellungsmodell“ die Option **Azure Resource Manager** aus:
 
    ![Ressourcen-Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. Konfigurieren Sie die Überwachungsoption für die Auswahl des Speicherkontos, in dem diese Diagnosedateien abgelegt werden sollen.
 
    ![Erstellen eines virtuellen Computers](./media/virtual-machines-common-boot-diagnostics/screenshot4.jpg)

3. Wenn Sie aus einer Azure Resource Manager-Vorlage bereitstellen, navigieren Sie zur Ressource des virtuellen Computers und fügen den Diagnoseprofilabschnitt an. Denken Sie daran, den API-Versionsheader „2015-06-15“ zu verwenden.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. Das Diagnoseprofil ermöglicht Ihnen die Auswahl des Speicherkontos, in dem diese Protokolle abgelegt werden sollen.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

Informationen zum Bereitstellen eines virtuellen Beispielcomputers mit aktivierter Startdiagnose finden Sie in unserem Repository.

## <a name="update-an-existing-virtual-machine"></a>Aktualisieren eines vorhandenen virtuellen Computers ##

Zum Aktivieren der Startdiagnose über das Portal können Sie auch einen vorhandenen virtuellen Computer über das Portal aktualisieren. Wählen Sie die Option „Startdiagnose“, und speichern Sie. Starten Sie den virtuellen Computer neu, damit die Einstellungen übernommen werden.

![Aktualisieren eines vorhandenen virtuellen Computers](./media/virtual-machines-common-boot-diagnostics/screenshot5.png)

