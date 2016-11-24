<!--author=SharS last changed: 02/22/16-->

### <a name="to-configure-and-register-the-device"></a>So konfigurieren und registrieren Sie das Gerät
1. Greifen Sie an der seriellen Konsole Ihres StorSimple-Geräts auf die Windows PowerShell-Benutzeroberfläche zu. Anweisungen dazu finden Sie unter [Verwenden von PuTTY für das Herstellen einer Verbindung mit der seriellen Gerätekonsole](#use-putty-to-connect-to-the-device-serial-console) . **Achten Sie darauf, dass Sie die Vorgehensweise genau befolgen. Andernfalls sind Sie nicht in der Lage, auf die Konsole zuzugreifen.**
2. Drücken Sie in der Sitzung, die geöffnet wird, einmal die EINGABETASTE, um eine Eingabeaufforderung zu öffnen. 
3. Sie werden aufgefordert, die Sprache auszuwählen, die für Ihr Gerät festgelegt werden soll. Geben Sie die Sprache an, und drücken Sie dann die EINGABETASTE. 
   
    ![StorSimple – Konfigurieren und Registrieren des Geräts 1](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice1-gov-include.png)
4. Wählen Sie im Menü der seriellen Konsole, das angezeigt wird, Option 1 aus, d. h. die Anmeldung mit Vollzugriff. 
   
    ![StorSimple – Registrieren des Geräts 2](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice2-gov-include.png)
5. Führen Sie die folgenden Schritte aus, um die mindestens erforderlichen Netzwerkeinstellungen für Ihr Gerät zu konfigurieren.
   
   > [!IMPORTANT]
   > Diese Konfigurationsschritte müssen für den aktiven Controller des Geräts ausgeführt werden. Das Menü der seriellen Konsole gibt den Zustand des Controllers in der Bannermeldung an. Wenn Sie nicht mit dem aktiven Controller verbunden sind, trennen Sie die Verbindung, und stellen Sie dann eine Verbindung mit dem aktiven Controller her.
   > 
   > 
   
   1. Geben Sie an der Eingabeaufforderung Ihr Kennwort ein. Das Standardkennwort für das Gerät lautet **Password1**.
   2. Geben Sie den folgenden Befehl ein:
      
        `Invoke-HcsSetupWizard`
   3. Es wird ein Installations-Assistent angezeigt, der Sie beim Konfigurieren der Netzwerkeinstellungen für das Gerät unterstützt. Geben Sie die folgenden Informationen an: 
      
      * IP-Adresse für die Netzwerkschnittstelle DATA 0
      * Subnetzmaske
      * Gateway
      * IP-Adresse für den primären DNS-Server
      * IP-Adresse für den sekundären DNS-Server
      
      > [!NOTE]
      > Sie müssen ggf. einige Minuten warten, bis die Subnetzmaske und die DNS-Einstellungen übernommen werden. 
      > 
      > 
   4. (Optional) Konfigurieren Sie Ihren Webproxyserver.
      
      > [!IMPORTANT]
      > Die Webproxykonfiguration ist optional. Achten Sie jedoch bei Verwendung eines Webproxys darauf, dass dieser nur hier konfiguriert werden kann. Weitere Informationen finden Sie unter [Konfigurieren des Webproxys für Ihr Gerät](../articles/storsimple/storsimple-configure-web-proxy.md). 
      > 
      > 
6. Drücken Sie STRG+C, um den Setup-Assistenten zu beenden.
7. Installieren Sie die Updates wie folgt:
   
   1. Verwenden Sie das folgende Cmdlet, um IP-Adressen für beide Controller festzulegen:
      
      `Set-HcsNetInterface -InterfaceAlias Data0 -Controller0IPv4Address <Controller0 IP> -Controller1IPv4Address <Controller1 IP>`
   2. Führen Sie an der Eingabeaufforderung Folgendes aus: `Get-HcsUpdateAvailability`. Sie sollten eine Benachrichtigung über Updates erhalten.
   3. Führen Sie `Start-HcsUpdate` aus. Sie können diesen Befehl auf einem beliebigen Knoten ausführen. Die Updates werden auf dem ersten Domänencontroller angewendet, und für den Controller wird ein Failover ausgeführt. Anschließend werden die Updates auf dem anderen Domänencontroller angewendet.
      
      Sie können den Fortschritt des Updateauftrags verfolgen, indem Sie `Get-HcsUpdateStatus` ausführen.    
      
      Die folgende Beispielausgabe zeigt den Status des Updates.
      
      ````
      Controller0>Get-HcsUpdateStatus
      RunInprogress       : True
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   : 
      ````
      
      Die folgende Beispielausgabe zeigt an, dass das Update abgeschlossen ist.
      
      ````
      Controller1>Get-HcsUpdateStatus
      
      RunInprogress       : False
      LastHotfixTimestamp : 4/13/2015 10:56:13 PM
      LastUpdateTimestamp : 4/13/2015 10:35:25 PM
      Controller0Events   :
      Controller1Events   :
      
      It may take up to 11 hours to apply all the updates, including the Windows Updates.
8. After all the updates are successfully installed, run the following cmdlet to confirm that the software updates were applied correctly:
   
     `Get-HcsSystem`
   
    You should see the following versions:
   
   * HcsSoftwareVersion: 6.3.9600.17491
   * CisAgentVersion: 1.0.9037.0
   * MdsAgentVersion: 26.0.4696.1433
9. Run the following cmdlet to confirm that the firmware update was applied correctly:
   
    `Start-HcsFirmwareCheck`.
   
     The firmware status should be **UpToDate**.
10. Run the following cmdlet to point the device to the Microsoft Azure Government portal (because it points to the public Azure classic portal by default). This will restart both controllers. We recommend that you use two PuTTY sessions to simultaneously connect to both controllers so that you can see when each controller is restarted.
    
     `Set-CloudPlatform -AzureGovt_US`
    
    You will see a confirmation message. Accept the default (**Y**).
11. Run the following cmdlet to resume setup:
    
     `Invoke-HcsSetupWizard`
    
     ![Resume setup wizard](./media/storsimple-configure-and-register-device-gov/HCS_ResumeSetup-gov-include.png)
    
    When you resume setup, the wizard will be the Update 1 version (which corresponds to version 17469). 
12. Accept the network settings. You will see a validation message after you accept each setting.
13. For security reasons, the device administrator password expires after the first session, and you will need to change it now. When prompted, provide a device administrator password. A valid device administrator password must be between 8 and 15 characters. The password must contain three of the following: lowercase, uppercase, numeric, and special characters.
    
    <br/>![StorSimple register device 5](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice5_gov-include.png)
14. The final step in the setup wizard registers your device with the StorSimple Manager service. For this, you will need the service registration key that you obtained in [Step 2: Get the service registration key](#step-2-get-the-service-registration-key). After you supply the registration key, you may need to wait for 2-3 minutes before the device is registered.
    
    > [!NOTE]
    > You can press Ctrl + C at any time to exit the setup wizard. If you have entered all the network settings (IP address for Data 0, Subnet mask, and Gateway), your entries will be retained.
    > 
    > 
    
    ![StorSimple registration progress](./media/storsimple-configure-and-register-device-gov/HCS_RegistrationProgress-gov-include.png)
15. After the device is registered, a Service Data Encryption key will appear. Copy this key and save it in a safe location. **This key will be required with the service registration key to register additional devices with the StorSimple Manager service.** Refer to [StorSimple security](../articles/storsimple/storsimple-security.md) for more information about this key.
    
    ![StorSimple register device 7](./media/storsimple-configure-and-register-device-gov/HCS_RegisterYourDevice7_gov-include.png)    
    
    > [!IMPORTANT]
    > To copy the text from the serial console window, simply select the text. You should then be able to paste it in the clipboard or any text editor. 
    > 
    > DO NOT use Ctrl + C to copy the service data encryption key. Using Ctrl + C will cause you to exit the setup wizard. As a result, the device administrator password will not be changed and the device will revert to the default password.
    > 
    > 
16. Exit the serial console.
17. Return to the Azure Government Portal, and complete the following steps:
    
    1. Double-click your StorSimple Manager service to access the **Quick Start** page.
    2. Click **View connected devices**.
    3. On the **Devices** page, verify that the device has successfully connected to the service by looking up the status. The device status should be **Online**.
       
        ![StorSimple Devices page](./media/storsimple-configure-and-register-device-gov/HCS_DeviceOnline-gov-include.png) 
       
        If the device status is **Offline**, wait for a couple of minutes for the device to come online. 
       
        If the device is still offline after a few minutes, then you need to make sure that your firewall network was configured as described in [networking requirements for your StorSimple device](../articles/storsimple/storsimple-system-requirements.md). 
       
        Verify that port 9354 is open for outbound communication as this is used by the service bus for StorSimple Manager service-to-device communication.



<!--HONumber=Nov16_HO3-->


