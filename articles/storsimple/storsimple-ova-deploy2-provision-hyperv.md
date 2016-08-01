<properties
   pageTitle="Bereitstellen des StorSimple Virtual Array – Bereitstellen in Hyper-V"
   description="In diesem zweiten Tutorial der Reihe zur StorSimple Virtual Array-Bereitstellung geht es um das Bereitstellen eines virtuellen Geräts in Hyper-V."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/14/2016"
   ms.author="alkohli"/>

# Bereitstellen des StorSimple Virtual Array – Bereitstellen eines Virtual Array in Hyper-V

![](./media/storsimple-ova-deploy2-provision-hyperv/hyperv4.png)

## Übersicht

Dieses Tutorial zur Bereitstellung bezieht sich auf Microsoft Azure StorSimple Virtual Arrays (auch als „lokale virtuelle StorSimple-Geräte“ oder „virtuelle StorSimple-Geräte“ bezeichnet) mit der Version vom März 2016 (allgemeine Verfügbarkeit). In diesem Tutorial wird erläutert, wie Sie ein StorSimple Virtual Array auf einem Hostsystem mit Hyper-V unter Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2 bereitstellen. Dieser Artikel bezieht sich auf die Bereitstellung von StorSimple Virtual Arrays im klassischem Azure-Portal sowie in der Microsoft Azure Government-Cloud.

Sie benötigen Administratorrechte, um ein virtuelles Gerät bereitzustellen und zu konfigurieren. Die Bereitstellung und die anfängliche Einrichtung dauern ca. 10 Minuten.


## Voraussetzungen für die Bereitstellung

Hier finden Sie die Voraussetzungen, die erfüllt sein müssen, damit Sie ein virtuelles Gerät auf einem Hostsystem mit Hyper-V unter Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2 bereitstellen können.

### Für den StorSimple Manager-Dienst

Stellen Sie Folgendes sicher, bevor Sie beginnen:

-   Sie haben alle Schritte unter [Vorbereiten des Portals für StorSimple Virtual Array](storsimple-ova-deploy1-portal-prep.md) ausgeführt.

-   Sie haben das Image des virtuellen Geräts für Hyper-V aus dem Azure-Portal heruntergeladen. Weitere Informationen finden Sie unter [Schritt 3: Herunterladen des Image mit dem virtuellen Gerät](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

	> [AZURE.IMPORTANT] Die Software auf dem StorSimple Virtual Array kann nur in Verbindung mit dem StorSimple Manager-Dienst verwendet werden.

### Für das virtuelle StorSimple-Gerät

Stellen Sie Folgendes sicher, bevor Sie ein virtuelles Gerät bereitstellen:

-   Sie haben Zugriff auf ein Hostsystem mit Hyper-V unter Windows Server 2008 R2 oder höher, das zum Bereitstellen eines Geräts verwendet werden kann.

-   Das Hostsystem verfügt für die Bereitstellung des virtuellen Geräts über die folgenden Ressourcen:

	-   Mindestens 4 Kerne

	-   Mindestens 8 GB RAM

	-   Eine Netzwerkschnittstelle

	-   Einen virtuellen Datenträger mit 500 GB für Systemdaten

### Für das Netzwerk im Datencenter

Bevor Sie beginnen, überprüfen Sie die Netzwerkanforderungen zum Bereitstellen eines virtuellen StorSimple-Geräts, und konfigurieren Sie das Netzwerk des Rechenzentrums entsprechen. Weitere Informationen finden Sie unter [StorSimple Virtual Array – Netzwerkanforderungen](storsimple-ova-system-requirements.md#networking-requirements).

## Schritt-für-Schritt-Bereitstellung

Zum Bereitstellen und Herstellen der Verbindung mit einem virtuellen Gerät müssen Sie die folgenden Schritte ausführen:

1.  Stellen Sie sicher, dass das Hostsystem über genügend Ressourcen zum Erfüllen der Mindestanforderungen des virtuellen Geräts verfügt.

2.  Stellen Sie ein virtuelles Gerät in Ihrem Hypervisor bereit.

3.  Starten Sie das virtuelle Gerät, und rufen Sie die IP-Adresse ab.

Jeder dieser Schritte wird in den folgenden Abschnitten erläutert.

## Schritt 1: Sicherstellen, dass das Hostsystem die Mindestanforderungen für virtuelle Geräte erfüllt

Zum Erstellen eines virtuellen Geräts benötigen Sie Folgendes:

-   Die unter Windows Server 2012 R2, Windows Server 2012 oder Windows Server 2008 R2 mit SP1 installierte Hyper-V-Rolle.

-   Microsoft Hyper-V Manager auf einem Microsoft Windows-Client mit einer Verbindung mit dem Host

Sie müssen sicherstellen, dass die zugrunde liegende Hardware (Hostsystem), auf der Sie das virtuelle Gerät erstellen, die folgenden Ressourcen für Ihr virtuelles Gerät zur Verfügung stellen kann:

- Mindestens 4 Kerne
- Mindestens 8 GB RAM
- Eine Netzwerkschnittstelle
- Einen virtuellen Datenträger mit 500 GB für Systemdaten

## Schritt 2: Bereitstellen eines virtuellen Geräts in Hypervisor

Führen Sie die folgenden Schritte aus, um ein Gerät im Hypervisor bereitzustellen.

#### So stellen Sie ein virtuelles Gerät bereit

1.  Kopieren Sie das Image des virtuellen Geräts auf den lokalen Datenträger Ihres Windows Server-Hosts. Dies ist das Image (VHD oder VHDX), das Sie über das Azure-Portal heruntergeladen haben. Notieren Sie sich den Speicherort, an den Sie das Image kopiert haben, da Sie ihn später noch benötigen.

2.  Öffnen Sie den **Server-Manager**. Klicken Sie in der oberen rechten Ecke auf **Extras**, und wählen Sie **Hyper-V-Manager**.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image1.png)

	Wenn Sie Windows Server 2008 R2 ausführen, öffnen Sie den Hyper-V-Manager. Klicken Sie im Server-Manager auf **Rollen > Hyper-V > Hyper-V-Manager**.

1.  Klicken Sie im **Hyper-V-Manager** unter „Bereich“ mit der rechten Maustaste auf Ihren Systemknoten, um das Kontextmenü zu öffnen. Klicken Sie dann auf **Neu** > **Virtueller Computer**.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image2.png)

1.  Klicken Sie auf der Seite **Vorbereitung** des Assistenten für neue virtuelle Computer auf **Weiter**.

1.  Geben Sie auf der Seite **Namen und Speicherort angeben** einen **Namen** für Ihr virtuelles Gerät an. Klicken Sie auf **Weiter**.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image4.png)

1.  Wählen Sie auf der Seite **Generation angeben** den Imagetyp für das Gerät, und klicken Sie dann auf **Weiter**. Diese Seite wird nicht angezeigt, wenn Sie Windows Server 2008 R2 verwenden.

    * Wählen Sie **Generation 2**, wenn Sie ein VHDX-Image für Windows Server 2012 oder höher heruntergeladen haben.
    * Wählen Sie **Generation 1**, wenn Sie ein VHD-Image für Windows Server 2008 R2 oder höher heruntergeladen haben.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image5.png)

1.  Geben Sie auf der Seite **Speicher zuweisen** einen **Startspeicher** von mindestens **8192 MB** an, aktivieren Sie nicht den dynamischen Arbeitsspeicher, und klicken Sie auf **Weiter**.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image6.png)

1.  Geben Sie auf der Seite **Netzwerk konfigurieren** den virtuellen Switch an, der mit dem Internet verbunden ist, und klicken Sie auf **Weiter**.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image7.png)

1.  Wählen Sie auf der Seite **Virtuelle Festplatte verbinden** die Option **Vorhandene virtuelle Festplatte verwenden**, geben Sie den Speicherort des Images des virtuellen Geräts an (VHDX oder VHD), und klicken Sie auf **Weiter**.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image8m.png)

1.  Überprüfen Sie die **Zusammenfassung**, und klicken Sie dann auf **Fertig stellen**, um den virtuellen Computer zu erstellen. Vorher müssen Sie aber noch einige CPU-Kerne und ein zweites Laufwerk hinzufügen.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image9.png)

1.  Sie benötigen vier Kerne, um die Mindestanforderungen zu erfüllen. Gehen Sie zum Hinzufügen von virtuellen Prozessoren wie folgt vor: Wählen Sie im Fenster **Hyper-V-Manager** Ihr Hostsystem aus, und suchen Sie im rechten Bereich in der Liste **Virtuelle Computer** nach dem virtuellen Computer, den Sie gerade erstellt haben. Klicken Sie mit der rechten Maustaste auf den ausgewählten Namen der Maschine, und wählen Sie **Einstellungen**.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image10.png)

1.  Klicken Sie auf der Seite **Einstellungen** im linken Bereich auf **Prozessor**. Legen Sie im rechten Bereich die **Anzahl virtueller Prozessoren** auf 4 (oder mehr) fest. Klicken Sie auf **Übernehmen**.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image11.png)

1.  Um die Mindestanforderungen zu erfüllen, müssen Sie auch einen virtuellen Datenträger mit 500 GB hinzufügen. Auf der Seite **Einstellungen**:

    1.  Wählen Sie im linken Bereich die Option **SCSI-Controller**.
    2.  Wählen Sie im rechten Bereich die Option **Festplatte**, und klicken Sie auf **Hinzufügen**.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image12.png)

1.  Wählen Sie auf der Seite **Festplatte** die Option **Virtuelle Festplatte**, und klicken Sie auf **Neu**. Der **Assistent für neue virtuelle Festplatten** wird gestartet.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image13.png)

1.  Klicken Sie auf der Seite **Vorbereitung** des Assistenten für neue virtuelle Festplatten auf **Weiter**.

1.  Übernehmen Sie auf der Seite **Datenträgerformat auswählen** die Standardoption **VHDX** für das Format. Klicken Sie auf **Weiter**. Dieser Bildschirm wird nicht angezeigt, wenn Sie Windows Server 2012 R2 oder Windows Server 2008 R2 ausführen.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image15.png)

1.  Legen Sie auf der Seite **Datenträgertyp auswählen** den Datenträgertyp für die virtuelle Festplatte auf **Dynamisch erweiterbar** fest (empfohlen). Sie können auch die Option **Feste Größe** wählen, aber dies ist unter Umständen mit einer langen Wartezeit verbunden. Die Verwendung der Option **Differenzierend** ist nicht zu empfehlen. Klicken Sie auf **Weiter**. Beachten Sie, dass die Option **Dynamisch erweiterbar** der Standardwert in Windows Server 2012 R2 und Windows Server 2012 ist. In Windows Server 2008 R2 lautet die Standardoption **Feste Größe**.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image16.png)

1.  Geben Sie auf der Seite **Namen und Speicherort angeben** einen **Namen** und einen **Speicherort** (z.B. per Durchsuchen) für den Datenträger an. Klicken Sie auf **Weiter**.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image17.png)

1.  Wählen Sie auf der Seite **Datenträger konfigurieren** die Option **Neue virtuelle Festplatte ohne Inhalt erstellen**, und geben Sie als Größe **500 GB** (oder mehr) an. Klicken Sie auf **Weiter**.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image18.png)

1.  Überprüfen Sie auf der Seite **Zusammenfassung** die Details Ihres virtuellen Datenträgers, und klicken Sie zum Erstellen des Datenträgers auf **Fertig stellen**, wenn alles korrekt ist. Der Assistent wird geschlossen, und Ihrem Computer wird eine virtuelle Festplatte hinzugefügt.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image19.png)

2.  Sie gelangen zurück zur Seite **Einstellungen**. Klicken Sie auf **OK**, um die Seite **Einstellungen** zu schließen und zum Hyper-V-Manager-Fenster zurückzukehren.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image20.png)

## Schritt 3: Starten des virtuellen Geräts und Abrufen der IP-Adresse

Führen Sie die folgenden Schritte aus, um Ihr virtuelles Gerät zu starten und eine Verbindung dafür herzustellen.

#### So starten Sie das virtuelle Gerät

1.  Starten Sie das virtuelle Gerät.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image21.png)

1.  Wenn das Gerät ausgeführt wird, wählen Sie das Gerät aus, klicken Sie mit der rechten Maustaste, und wählen Sie **Verbinden**.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image22.png)

1.  Unter Umständen müssen Sie 5 bis 10 Minuten warten, bis das Gerät bereit ist. In der Konsole wird eine Statusmeldung angezeigt, die den Fortschritt angibt. Wenn das Gerät bereit ist, wählen Sie **Aktion**. Drücken Sie `Ctrl + Alt + Delete`, um sich am virtuellen Gerät anzumelden. Der Standardbenutzer lautet *StorSimpleAdmin*, und das Standardkennwort lautet *Password1*.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image23.png)

1.  Aus Sicherheitsgründen gilt das Kennwort des Geräteadministrators nur für die erste Anmeldung. Sie werden aufgefordert, das Kennwort zu ändern.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image24.png)

	Geben Sie ein Kennwort ein, das mindestens acht Zeichen lang ist. Das Kennwort muss mindestens drei der folgenden vier Anforderungen erfüllen: Großbuchstaben, Kleinbuchstaben, Zahlen und Sonderzeichen. Geben Sie das Kennwort erneut ein, um es zu bestätigen. Sie werden benachrichtigt, dass das Kennwort geändert wurde.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image25.png)

1.  Nachdem das Kennwort erfolgreich geändert wurde, wird das virtuelle Gerät unter Umständen neu gestartet. Warten Sie, bis das Gerät gestartet wurde.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image26.png)

 	Die Windows PowerShell-Konsole des Geräts wird zusammen mit der Statusanzeige eingeblendet.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image27.png)

1.  Die Schritte 6 bis 8 gelten nur beim Starten in anderen Umgebungen als einer DHCP-Umgebung. Falls Sie sich in einer DHCP-Umgebung befinden, sollten Sie diese Schritte überspringen und mit Schritt 9 fortfahren. Wenn Sie Ihr Gerät in einer anderen Umgebung als einer DHCP-Umgebung gestartet haben, wird der folgende Bildschirm angezeigt.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image28m.png)

 	Sie müssen das Netzwerk jetzt konfigurieren.

1.  Verwenden Sie den Befehl `Get-HcsIpAddress`, um die Netzwerkschnittstellen aufzulisten, die auf Ihrem virtuellen Gerät aktiviert sind. Wenn für das Gerät eine einzelne Netzwerkschnittstelle aktiviert ist, wird dieser Schnittstelle der Standardname `Ethernet` zugewiesen.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image29m.png)

1.  Verwenden Sie das `Set-HcsIpAddress`-Cmdlet, um das Netzwerk zu konfigurieren. Unten ist ein Beispiel angegeben:

 	`Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

 	![](./media/storsimple-ova-deploy2-provision-hyperv/image30.png)

1.  Nachdem die anfängliche Einrichtung abgeschlossen und das Gerät gestartet wurde, wird der Bannertext für das Gerät angezeigt. Notieren Sie sich die IP-Adresse und die URL, die im Bannertext für die Verwaltung des Geräts angezeigt wird. Sie verwenden diese IP-Adresse zum Herstellen der Verbindung mit der Web-UI Ihres virtuellen Geräts und zum Durchführen des lokalen Setups und der Registrierung.

	![](./media/storsimple-ova-deploy2-provision-hyperv/image31m.png)



1. (Optional) Führen Sie diesen Schritt nur aus, wenn Sie Ihr Gerät in der Government-Cloud bereitstellen. Aktivieren Sie jetzt den FIPS-Modus (Federal Information Processing Standard) auf Ihrem Gerät. Der FIPS 140-Standard definiert kryptographische Algorithmen, die zum Schutz vertraulicher Daten in Computersystemen der US-Regierung zugelassen sind.
	1. Führen Sie das folgende Cmdlet aus, um den FIPS-Modus zu aktivieren:

		`Enter-HcsFIPSMode`

	2. Starten Sie das Gerät neu, nachdem Sie den FIPS-Modus aktiviert haben, damit die kryptografischen Überprüfungen greifen.

		> [AZURE.NOTE] Sie können auf Ihrem Gerät den FIPS-Modus entweder aktivieren oder deaktivieren. Ein ständiger Wechsel des Geräts vom FIPS-Modus in den Nicht-FIPS-Modus und umgekehrt wird nicht unterstützt.

Wenn Ihr Gerät die Mindestanforderungen für die Konfiguration nicht erfüllt, wird im Bannertext ein Fehler angezeigt (siehe unten). Sie müssen die Gerätekonfiguration ändern, damit sie über ausreichende Ressourcen zum Erfüllen der Mindestanforderungen verfügt. Sie können das Gerät dann neu starten und die Verbindung dafür herstellen. Die Mindestanforderungen für die Konfiguration finden Sie unter [Schritt 1: Sicherstellen, dass das Hostsystem die Mindestanforderungen für virtuelle Geräte erfüllt](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-hyperv/image32.png)

Falls bei der anfänglichen Konfiguration über die lokale Webbenutzeroberfläche andere Fehler auftreten, helfen Ihnen die folgenden Workflows unter [Verwaltung des StorSimple Virtual Array mithilfe der Web-UI](storsimple-ova-web-ui-admin.md) weiter.

-   Ausführen von Diagnosetests zum [Beheben von Setup-Fehlern für die Web-UI](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors)

-   [Generieren des Protokollpakets und Anzeigen von Protokolldateien](storsimple-ova-web-ui-admin.md#generate-a-log-package)

![Video-Symbol](./media/storsimple-ova-deploy2-provision-hyperv/video_icon.png) **Video verfügbar**

Sehen Sie sich das Video an, in dem die Bereitstellung eines StorSimple Virtual Array in Hyper-V beschrieben wird.

> [AZURE.VIDEO create-a-storsimple-virtual-array]

## Nächste Schritte

-   [Einrichten des StorSimple Virtual Array als Dateiserver](storsimple-ova-deploy3-fs-setup.md)

-   [Einrichten des StorSimple Virtual Array als iSCSI-Server](storsimple-ova-deploy3-iscsi-setup.md)

<!---HONumber=AcomDC_0720_2016-->