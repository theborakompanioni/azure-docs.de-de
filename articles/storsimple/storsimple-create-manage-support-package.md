---
title: Erstellen eines StorSimple-Unterstützungspakets | Microsoft Docs
description: Hier erfahren Sie, wie Sie ein Unterstützungspaket für Ihr StorSimple-Gerät erstellen, entschlüsseln und bearbeiten.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''

ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli

---
# Erstellen und Verwalten eines StorSimple-Unterstützungspakets
## Übersicht
Ein StorSimple-Unterstützungspaket ist ein einfach zu verwendender Mechanismus, der alle relevanten Protokolle zur Unterstützung des Microsoft Supports bei der Behandlung von Problemen mit StorSimple-Geräten erfasst. Die erfassten Protokolle werden verschlüsselt und komprimiert.

Dieses Tutorial enthält schrittweise Anleitungen für die Erstellung und Verwaltung des Unterstützungspakets.

## Erstellen und Hochladen eines Unterstützungspakets im klassischen Azure-Portal
Sie können im klassischen Azure-Portal über die Seite **Wartung** des Diensts ein Unterstützungspaket erstellen und an die Microsoft Support-Website hochladen.

> [!NOTE]
> Der Upload erfordert einen Supporthauptschlüssel. Der für Sie zuständige Supportmitarbeiter sollte Ihnen diesen Schlüssel per E-Mail bereitstellen.
> 
> 

Ein verschlüsseltes und komprimiertes Unterstützungspaket (CAB-Datei) wird erstellt und an die Support-Website hochgeladen. Der Supportmitarbeiter kann dann dieses Paket für die Problembehandlung von der Support-Website abrufen.

Führen Sie im klassischen Portal die folgenden Schritte aus, um ein Unterstützungspaket zu erstellen.

#### Erstellen eines Unterstützungspakets im klassischen Azure-Portal
1. Wählen Sie **Geräte** > **Wartung**.
2. Wählen Sie im Abschnitt **Unterstützungspaket** die Option **Unterstützungspaket erstellen und hochladen**.
3. Führen Sie im Dialogfeld **Unterstützungspaket erstellen und hochladen** die folgenden Schritte aus:
   
    ![Erstellen eines Unterstützungspakets](./media/storsimple-create-manage-support-package/IC740923.png)
   
   * Geben Sie das Textfeld **Supporthauptschlüssel** den Hauptschlüssel ein. Ihr Microsoft-Supportmitarbeiter sollte Ihnen diesen Hauptschlüssel per E-Mail senden.
   * Wählen Sie das Kontrollkästchen für die Zustimmung, das Unterstützungspaket automatisch auf die Microsoft Support-Website hochzuladen.
   * Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-create-manage-support-package/IC740895.png).

## Manuelles Erstellen eines Unterstützungspakets
In einigen Fällen müssen Sie das Unterstützungspaket über Windows PowerShell für StorSimple manuell erstellen. Beispiel:

* Wenn Sie vertrauliche Informationen vor dem Freigeben für den Microsoft Support aus Ihren Protokolldateien entfernen müssen.
* Wenn das Hochladen des Pakets aufgrund von Konnektivitätsproblemen schwierig ist.

Sie können das manuell generierte Unterstützungspaket per E-Mail für den Microsoft Support freigeben. Führen Sie die folgenden Schritte aus, um in Windows PowerShell ein Unterstützungspaket für StorSimple zu erstellen.

#### So erstellen Sie ein Unterstützungspaket in Windows PowerShell für StorSimple
1. Geben Sie den folgenden Befehl ein, um auf dem Remotecomputer, über den eine Verbindung mit dem StorSimple-Gerät hergestellt wird, eine Windows PowerShell-Sitzung als Administrator zu starten:
   
    `Start PowerShell`
2. Stellen Sie in der Windows PowerShell-Sitzung eine Verbindung mit der SSAdmin-Konsole des Geräts her:
   
   * Geben Sie an der Eingabeaufforderung Folgendes ein:
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   * Geben Sie im angezeigten Dialogfeld Ihr Geräteadministratorkennwort ein. Standardkennwort:
     
      `Password1`
     
      ![PowerShell-Anmeldeinformationen-Dialogfeld](./media/storsimple-create-manage-support-package/IC740962.png)
   * Klicken Sie auf **OK**.
   * Geben Sie an der Eingabeaufforderung Folgendes ein:
     
      `Enter-PSSession $MS`
3. Geben Sie in der geöffneten Sitzung den entsprechenden Befehl ein.
   
   * Geben Sie für kennwortgeschützte Netzwerkfreigaben Folgendes ein:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       Sie werden aufgefordert, ein Kennwort, einen Pfad zum freigegebenen Netzwerkordner und eine Verschlüsselungspassphrase (da das Unterstützungspaket verschlüsselt ist) einzugeben. Ein Unterstützungspaket wird dann im angegebenen Ordner erstellt.
   * Bei Freigaben ohne Kennwortschutz wird der `-Credential`-Parameter nicht benötigt. Geben Sie Folgendes ein:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       Das Unterstützungspaket wird für beide Controller im freigegebenen Netzwerkordner erstellt. Hierbei handelt es sich um eine verschlüsselte, komprimierte Datei, die zur Problembehandlung an den Support von Microsoft gesendet werden kann. Weitere Informationen finden Sie unter [Kontaktaufnahme mit dem Microsoft-Support](storsimple-contact-microsoft-support.md).

### Die Parameter des Export-HcsSupportPackage-Cmdlets
Sie können die folgenden Parameter mit dem Export-HcsSupportPackage-Cmdlet verwenden.

| Parameter | Erforderlich/Optional | Beschreibung |
| --- | --- | --- |
| `-Path` |Erforderlich |Dient zum Angeben des Orts des freigegebenen Netzwerkordners für das Unterstützungspaket. |
| `-EncryptionPassphrase` |Erforderlich |Dient zum Angeben einer Passphrase für die Verschlüsselung des Unterstützungspakets. |
| `-Credential` |Optional |Dient zum Angeben von Anmeldeinformationen für den Zugriff auf den freigegebenen Netzwerkordner. |
| `-Force` |Optional |Dient zum Überspringen des Bestätigungsschritts für die Verschlüsselungspassphrase. |
| `-PackageTag` |Optional |Dient zum Angeben eines Verzeichnisses unter *Path*, in dem das Unterstützungspaket platziert werden soll. Standardmäßig wird Folgendes verwendet: [Gerätename]-[aktuelles Datum und aktuelle Uhrzeit:JJJJ-MM-TT-HH-mm-ss]. |
| `-Scope` |Optional |Geben Sie **Cluster** (Standardeinstellung) an, um ein Unterstützungspaket für beide Controller zu erstellen. Wenn Sie nur ein Paket für den aktuellen Controller erstellen möchten, geben Sie **Controller** an. |

## Bearbeiten eines Unterstützungspakets
Nachdem Sie ein Unterstützungspaket generiert haben, müssen Sie das Paket vielleicht bearbeiten, um vertrauliche Informationen zu entfernen. Dies kann Volumenamen, Geräte-IP-Adressen und Sicherungsnamen aus den Protokolldateien umfassen.

> [!IMPORTANT]
> Unterstützungspakete können nur bearbeitet werden, wenn sie mit Windows PowerShell für StorSimple generiert wurden. Pakete, die im klassischen Azure-Portal mit dem StorSimple Manager-Dienst erstellt wurden, können nicht bearbeitet werden.
> 
> 

Wenn Sie ein Unterstützungspaket vor dem Hochladen an die Microsoft Support-Website bearbeiten möchten, müssen Sie es zunächst entschlüsseln, die Dateien bearbeiten und anschließend wieder verschlüsseln. Führen Sie die folgenden Schritte aus:

#### So bearbeiten Sie ein Unterstützungspaket in Windows PowerShell für StorSimple
1. Generieren Sie ein Unterstützungspaket, wie bereits unter [So erstellen Sie ein Unterstützungspaket in Windows PowerShell für StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple) beschrieben.
2. [Laden Sie das Skript](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokal auf den Client herunter.
3. Importieren Sie das Windows PowerShell-Modul. Geben Sie den Pfad des lokalen Ordners an, in den Sie das Skript heruntergeladen haben. Geben Sie zum Importieren des Moduls Folgendes ein:
   
    `Import-module <Path to the folder that contains the Windows PowerShell script>`
4. Bei allen Dateien es sich um komprimierte, verschlüsselte *AES*-Dateien. Geben Sie zum Dekomprimieren und Entschlüsseln der Dateien Folgendes ein:
   
    `Open-HcsSupportPackage <Path to the folder that contains support package files>`
   
    Beachten Sie, dass nun für alle Dateien die tatsächlichen Dateierweiterungen angezeigt werden.
   
    ![Bearbeiten des Unterstützungspakets](./media/storsimple-create-manage-support-package/IC750706.png)
5. Geben Sie die bei der Paketerstellung verwendete Verschlüsselungspassphrase ein, wenn Sie dazu aufgefordert werden.
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for the following parameters:EncryptionPassphrase: ****
6. Navigieren Sie zum Ordner mit den Protokolldateien. Da die Protokolldateien nun dekomprimiert und entschlüsselt sind, werden sie mit ihren ursprünglichen Dateierweiterungen angezeigt. Entfernen Sie alle kundenspezifischen Informationen wie Volumenamen und Geräte-IP-Adressen aus den Dateien, und speichern Sie sie.
7. Schließen Sie die Dateien, damit sie mit Gzip komprimiert und mit AES-256 verschlüsselt werden. Dies macht die Übertragung des Unterstützungspakets über ein Netzwerk sicherer und schneller. Geben Sie zum Komprimieren und Verschlüsseln der Dateien Folgendes ein:
   
    `Close-HcsSupportPackage <Path to the folder that contains support package files>`
   
    ![Bearbeiten des Unterstützungspakets](./media/storsimple-create-manage-support-package/IC750707.png)
8. Geben Sie eine Verschlüsselungspassphrase für das geänderte Unterstützungspaket ein, wenn Sie dazu aufgefordert werden.
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****
9. Notieren Sie sich die Passphrase, um Sie auf Anfrage dem Support von Microsoft mitteilen zu können.

### Beispiel: Bearbeiten von Dateien in einem Unterstützungspaket auf einer kennwortgeschützten Freigabe
Das folgende Beispiel veranschaulicht das Entschlüsseln, Bearbeiten und erneute Verschlüsseln eines Unterstützungspakets:

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## Nächste Schritte
* Informieren Sie sich darüber, wie Sie [mithilfe von Unterstützungspaketen und Geräteprotokollen Probleme bei der Gerätebereitstellung behandeln](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).
* Erfahren Sie mehr über das [Verwalten Ihres StorSimple-Geräts mithilfe des StorSimple Manager-Diensts](storsimple-manager-service-administration.md).

<!---HONumber=AcomDC_0817_2016-->