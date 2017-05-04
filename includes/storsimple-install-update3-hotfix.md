<!--author=alkohli last changed: 09/01/16-->

#### <a name="to-download-hotfixes"></a>So laden Sie Hotfixes herunter
Führen Sie die folgenden Schritte aus, um das Softwareupdate aus dem Microsoft Update-Katalog herunterzuladen.

1. Starten Sie Internet Explorer, und navigieren Sie zu [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Falls Sie den Microsoft Update-Katalog zum ersten Mal auf diesem Computer verwenden, klicken Sie auf **Installieren** , wenn Sie zum Installieren des Add-Ons für den Microsoft Update-Katalog aufgefordert werden.
    ![Katalog installieren](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)
3. Geben Sie im Suchfeld des Microsoft Update-Katalogs die KB-Nummer des Hotfixes ein, den Sie herunterladen möchten (beispielsweise **3186843**), und klicken Sie anschließend auf **Suchen**.
   
    Die Hotfixauflistung wird angezeigt, z.B. das **kumulative Softwarepaket-Update 3.0 für die StorSimple 8000-Serie**.
   
    ![Katalog durchsuchen](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)
4. Klicken Sie auf **Hinzufügen**. Das Update wird dem Warenkorb hinzugefügt.
5. Suchen Sie aus der obigen Tabelle (**3186859**) nach weiteren Hotfixes, und fügen Sie sie jeweils dem Warenkorb hinzu.
6. Klicken Sie auf **Warenkorb anzeigen**.
7. Klicken Sie auf **Download**. Geben Sie einen lokalen Speicherort an, oder klicken Sie auf **Durchsuchen** , und navigieren Sie zum gewünschten lokalen Speicherort für den Download. Die Updates werden heruntergeladen und am angegebenen Speicherort in einem nach dem Update benannten Unterordner gespeichert. Der Ordner kann auch in eine Netzwerkfreigabe kopiert werden, auf die vom Gerät aus zugegriffen werden kann.

> [!NOTE]
> Der Zugriff auf die Hotfixes muss über beide Controller möglich sein, um die Erkennung von Fehlermeldungen des Peercontrollers zu ermöglichen.
> 
> 

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a>So installieren und überprüfen Sie Hotfixes für den normalen Modus
Führen Sie zum Installieren und Überprüfen der Hotfixes für den normalen Modus die folgenden Schritte aus. Falls Sie sie bereits über das Azure-Portal installiert haben, fahren Sie mit [Installieren und Überprüfen von Wartungsmodus-Hotfixes](#to-install-and-verify-maintenance-mode-hotfixes)fort.

1. Greifen Sie zum Installieren der Hotfixes über die serielle Konsole Ihres StorSimple-Geräts auf die Windows PowerShell-Benutzeroberfläche zu. Ausführliche Anweisungen dazu finden Sie unter [Verwenden von PuTTy zum Herstellen einer Verbindung mit der seriellen Konsole](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console). Drücken Sie bei der Eingabeaufforderung die **Eingabetaste**.
2. Wählen Sie **Option 1** , um sich mit Vollzugriff beim Gerät anzumelden. Es wird empfohlen, dass Sie den Hotfix zunächst auf dem passiven Controller installieren.
3. Geben Sie an der Eingabeaufforderung Folgendes ein, um den Hotfix zu installieren:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Verwenden Sie die IP-Adresse anstelle von DNS im Freigabepfad des obigen Befehls. Der Anmeldeinformationsparameter wird nur verwendet, wenn Sie auf eine authentifizierte Freigabe zugreifen.
   
    Es wird empfohlen, den Parameter für die Anmeldeinformationen zu verwenden, um auf Freigaben zuzugreifen. Sogar Freigaben, die für "jeden" offen sind, sind dies in der Regel nicht für nicht authentifizierte Benutzer.
   
    Geben Sie Ihr Kennwort ein, wenn Sie dazu aufgefordert werden.
   
    Nachfolgend sehen Sie eine Beispielausgabe.
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \hcsmdssoftwareupdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts the hotfix installation and could reboot one or
        both of the controllers. If the device is serving I/Os, these will not
        be disrupted. Are you sure you want to continue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
4. Geben Sie **Y** ein, um die Hotfixinstallation zu bestätigen.
5. Überwachen Sie das Update mithilfe des `Get-HcsUpdateStatus` -Cmdlets. Das Update wird zunächst auf dem passiven Controller abgeschlossen. Nachdem der passive Controller aktualisiert wurde, findet ein Failover statt, und das Update wird dann auf dem anderen Controller angewendet. Die Aktualisierung ist abgeschlossen, wenn beide Controller aktualisiert wurden.
   
    Die folgende Beispielausgabe zeigt den Status des Updates. Der Parameter `RunInprogress` ist `True`, wenn das Update ausgeführt wird.

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 8/29/2016 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     Die folgende Beispielausgabe zeigt an, dass das Update abgeschlossen ist. Der Parameter `RunInProgress` ist `False`, wenn das Update abgeschlossen ist.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 8/30/2016 9:15:55 AM
    LastUpdateTimestamp : 8/30/2016 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE] 
    > In manchen Fällen gibt das Cmdlet `False` zurück, wenn das Update noch ausgeführt wird. Um sicherzustellen, dass das Update abgeschlossen ist, warten Sie einige Minuten, führen Sie diesen Befehl erneut aus, und überprüfen Sie, ob `RunInProgress` gleich `False` ist. Wenn dies der Fall ist, wurde das Hotfix abgeschlossen.

1. Überprüfen Sie nach dem Abschließen des Softwareupdates die Versionen der Systemsoftware. Geben Sie Folgendes ein:
   
    `Get-HcsSystem`
   
    Daraufhin sollten die folgenden Versionen angezeigt werden.
   
   * `HcsSoftwareVersion: 6.3.9600.17759`
   * `CisAgentVersion:  1.0.9343.0`
   * `MdsAgentVersion: 30.0.4698.16`
     
     Wenn die Versionsnummern nach dem Anwenden des Updates nicht geändert werden, bedeutet dies, dass der Hotfix nicht angewendet werden konnte. Sollte dies der Fall sein, wenden Sie sich an den [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md), um weitere Unterstützung zu erhalten.
     
     > [!IMPORTANT]
     > Sie müssen den aktiven Controller mit dem Cmdlet `Restart-HcsController` neu starten, bevor Sie die verbleibenden Updates anwenden können. 
     > 
     > 
2. Wiederholen Sie die Schritte 3 bis 5, um den Hotfix für LSI-Treiber und -Firmware **KB3186859**zu installieren. Nachdem der Hotfix installiert wurde, verwenden Sie das Cmdlet `Get-HcsSystem` . Die LSI-Version muss wie folgt lauten:
   
   * `Lsisas2Version: 2.0.78.00`
3. Wiederholen Sie die Schritte 3 bis 5, um das Update von Storport und Spaceport **KB3121261**zu installieren.
4. Wenn Sie ausgehend von Update 2 oder einer früheren Version aktualisieren, müssen Sie auch Folgendes herunterladen:
   
   * iSCSI-Fix mithilfe von KB3146621
   * WMI-Fix mithilfe von KB3103616

#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a>So installieren und überprüfen Sie Wartungsmodus-Hotfixes
Verwenden Sie KB3121899, um die Datenträger-Firmwareupdates zu installieren. Es handelt sich dabei um beeinträchtigende Updates, die ca. 30 Minuten dauern. Sie können diese während eines geplanten Wartungsfensters durch Herstellen einer Verbindung mit der seriellen Konsole des Geräts installieren.

Beachten Sie, dass Sie diese Updates nicht installieren müssen, wenn Ihre Datenträger-Firmware bereits auf dem neuesten Stand ist. Führen Sie das Cmdlet `Get-HcsUpdateAvailability` über die serielle Gerätekonsole aus, um zu überprüfen, ob Updates verfügbar sind, und ob die Updates beeinträchtigend (Wartungsmodus) oder nicht beeinträchtigend (normaler Modus) sind.

Folgen Sie den Anweisungen unten, um die Datenträger-Firmwareupdates zu installieren.

1. Versetzen Sie das Gerät in den Wartungsmodus. Beachten Sie, dass Sie kein Windows PowerShell-Remoting verwenden sollten, wenn Sie für ein Gerät im Wartungsmodus eine Verbindung herstellen. Führen Sie dieses Cmdlet stattdessen auf dem Gerätecontroller aus, wenn eine Verbindung über die serielle Konsole des Geräts besteht. Geben Sie Folgendes ein:
   
    `Enter-HcsMaintenanceMode`
   
    Nachfolgend sehen Sie eine Beispielausgabe.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update2-8100-SHG0997879L76673
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    Daraufhin werden beide Controller im Wartungsmodus gestartet.
2. Geben Sie Folgendes ein, um das Datenträger-Firmwareupdate zu installieren:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Nachfolgend sehen Sie eine Beispielausgabe.
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. By installing new updates you agree to, and accept any additional terms associated with, the new functionality listed in the release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. Überwachen Sie den Installationsstatus mit dem Befehl `Get-HcsUpdateStatus` . Das Update ist abgeschlossen, wenn `RunInProgress` in `False` geändert wird.
4. Nachdem die Installation abgeschlossen ist, wird der Controller, auf dem der Hotfix für den Wartungsmodus installiert wurde, neu gestartet. Verwenden Sie für die Anmeldung Option 1 mit Vollzugriff, und überprüfen Sie die Firmwareversion des Datenträgers. Geben Sie Folgendes ein:
   
   `Get-HcsFirmwareVersion`
   
   Die erwarteten Datenträger-Firmwareversionen lauten:
   
   `XMGG, XGEG, KZ50, F6C2, VR08`
   
   Nachfolgend sehen Sie eine Beispielausgabe.
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8100
       Name: Update2-8100-SHG0997879L76YD
       Software Version: 6.3.9600.17705
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected to Controller1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
         ActiveBIOS:0.45.0006
         BackupBIOS:0.45.0008
         MainCPLD:17.0.0005
         ActiveBMCRoot:2.0.000E
         BackupBMCRoot:2.0.000E
         BMCBoot:2.0.0001
         LsiFirmware:19.00.00.00
         LsiBios:07.37.00.00
         Battery1Firmware:06.29
         Battery2Firmware:06.29
         DomFirmware:X231600
         CanisterFirmware:3.5.0.32
         CanisterBootloader:5.03
         CanisterConfigCRC:0xD1B030A4
         CanisterVPDStructure:0x06
         CanisterGEMCPLD:0x17
         CanisterVPDCRC:0xEE3504B4
         MidplaneVPDStructure:0x0C
         MidplaneVPDCRC:0xA6BD4F64
         MidplaneCPLD:0x10
         PCM1Firmware:1.00|1.05
         PCM1VPDStructure:0x05
         PCM1VPDCRC:0x41BEF99C
         PCM2Firmware:1.00|1.05
         PCM2VPDStructure:0x05
         PCM2VPDCRC:0x41BEF99C
   
         DisksFirmware
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
   
    Führen Sie den Befehl `Get-HcsFirmwareVersion` auf dem zweiten Controller aus, um zu überprüfen, ob die Softwareversion aktualisiert wurde. Anschließend können Sie den Wartungsmodus beenden. Geben Sie hierzu den folgenden Befehl für jeden Gerätecontroller ein:
   
   `Exit-HcsMaintenanceMode`
5. Beim Beenden des Wartungsmodus werden die Controller neu gestartet. Nachdem die Firmwareupdates für Datenträger erfolgreich installiert wurden und das Gerät den Wartungsmodus verlassen hat, kehren Sie zum klassischen Azure-Portal zurück. Beachten Sie, dass es bis zu 24 Stunden dauern kann, bis die Installation der Wartungsmodus-Updates im Portal angezeigt wird.

