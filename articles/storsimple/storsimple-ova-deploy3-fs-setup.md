---
title: "Bereitstellen von StorSimple Virtual Array 3 – Einrichten des virtuellen Geräts als Dateiserver"
description: "In diesem dritten Tutorial zur StorSimple Virtual Array-Bereitstellung lernen Sie, wie Sie ein virtuelles Gerät als Dateiserver einrichten."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: adc1c942-dd4e-4589-bc10-18ae3f7cbcdc
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/26/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 8f99e3dc59c0637053cad6c2b3b6baedac803f42


---
# <a name="deploy-storsimple-virtual-array---set-up-as-file-server"></a>Bereitstellen von StorSimple Virtual Array – Einrichten als Dateiserver
![](./media/storsimple-ova-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Einführung
Dieser Artikel bezieht sich auf Microsoft Azure StorSimple Virtual Array (auch als „lokales virtuelles StorSimple-Gerät“ oder „virtuelles StorSimple-Gerät“ bezeichnet) mit der Version vom März 2016 (allgemeine Verfügbarkeit). In diesem Artikel wird beschrieben, wie Sie die anfängliche Einrichtung durchführen, Ihren StorSimple-Dateiserver registrieren, die Geräteinstallation durchführen und SMB-Freigaben erstellen und die Verbindung dafür herstellen. Dies ist der letzte Artikel einer Reihe von Tutorials, in denen die vollständige Bereitstellung Ihres virtuellen Arrays als Dateiserver oder iSCSI-Server erläutert wird.

Der Einrichtungs- und Konfigurationsvorgang kann ca. zehn Minuten dauern.

## <a name="setup-prerequisites"></a>Voraussetzungen für das Setup
Überprüfen Sie Folgendes, bevor Sie Ihr virtuelles StorSimple-Gerät konfigurieren und einrichten:

* Sie haben ein virtuelles Gerät bereitgestellt und die Verbindung dafür hergestellt, wie unter [Bereitstellen eines StorSimple Virtual Array](storsimple-ova-deploy2-provision-hyperv.md) in Hyper-V bzw. [Bereitstellen eines StorSimple Virtual Array in VMware](storsimple-ova-deploy2-provision-vmware.md) beschrieben.
* Sie verfügen über den Dienstregistrierungsschlüssel aus dem StorSimple Manager-Dienst, den Sie zum Verwalten von virtuellen StorSimple-Geräten erstellt haben. Weitere Informationen finden Sie unter [Schritt 2: Abrufen des Dienstregistrierungsschlüssels](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) für StorSimple Virtual Array.
* Falls dies das zweite oder ein nachfolgendes virtuelles Gerät ist, das Sie für einen vorhandenen StorSimple Manager-Dienst registrieren, sollten Sie den Dienstdaten-Verschlüsselungsschlüssel bereithalten. Dieser Schlüssel wurde generiert, als das erste Gerät erfolgreich für den Dienst registriert wurde. Wenn Sie diesen Schlüssel verloren haben, helfen Ihnen die Informationen unter [Abrufen des Dienstdaten-Verschlüsselungsschlüssels](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) zum StorSimple Virtual Array weiter.

## <a name="step-by-step-setup"></a>Schritt-für-Schritt-Einrichtung
Verwenden Sie die folgende Schritt-für-Schritt-Anleitung, um Ihr virtuelles StorSimple-Gerät einzurichten und zu konfigurieren.

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Schritt 1: Durchführen der Einrichtung für die lokale Webbenutzeroberfläche und Registrieren Ihres Geräts
#### <a name="to-complete-the-setup-and-register-the-device"></a>So führen Sie das Setup durch und registrieren das Gerät
1. Öffnen Sie ein Browserfenster, und stellen Sie eine Verbindung mit der lokalen Webbenutzeroberfläche her. Geben Sie Folgendes ein:    
   
   `https://<ip-address of network interface>`
   
   Verwenden Sie die Verbindungs-URL aus dem vorherigen Schritt. Es wird eine Fehlermeldung mit dem Hinweis angezeigt, dass ein Problem mit dem Sicherheitszertifikat der Website aufgetreten ist. Klicken Sie auf **Mit dieser Webseite fortfahren**.
   
   ![](./media/storsimple-ova-deploy3-fs-setup/image2.png)
2. Melden Sie sich bei der Webbenutzeroberfläche des virtuellen Geräts als **StorSimpleAdmin** an. Geben Sie das Kennwort des Geräteadministrators ein, das Sie in „Schritt 3: Starten des virtuellen Geräts“ unter [Bereitstellen von StorSimple Virtual Array in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) bzw. [Bereitstellen von StorSimple Virtual Array in VMware](storsimple-ova-deploy2-provision-vmware.md) geändert haben.
   
   ![](./media/storsimple-ova-deploy3-fs-setup/image3.png)
3. Sie gelangen auf die **Startseite** . Auf dieser Seite werden die verschiedenen Einstellungen beschrieben, die zum Konfigurieren und Registrieren des virtuellen Geräts beim StorSimple Manager-Dienst erforderlich sind. Beachten Sie, dass die **Netzwerkeinstellungen**, **Webproxyeinstellungen** und **Uhrzeiteinstellungen** optional sind. Die einzigen erforderlichen Einstellungen sind **Geräteeinstellungen** und **Cloudeinstellungen**.
   
   ![](./media/storsimple-ova-deploy3-fs-setup/image4.png)
4. Auf der Seite **Netzwerkeinstellungen** unter **Netzwerkschnittstellen** wird DATA 0 automatisch für Sie konfiguriert. Jede Netzwerkschnittstelle ist standardmäßig so eingestellt, dass die IP-Adresse automatisch abgerufen wird (DHCP). Daher werden IP-Adresse, Subnetz und Gateway automatisch zugewiesen (sowohl für IPv4 als auch für IPv6).
   
   ![](./media/storsimple-ova-deploy3-fs-setup/image5.png)
   
   Wenn Sie während der Bereitstellung des Geräts mehr als eine Netzwerkschnittstelle hinzugefügt haben, können Sie diese hier konfigurieren. Beachten Sie, dass Sie die Netzwerkschnittstelle als nur IPv4 oder als IPv4 und IPv6 konfigurieren können. Konfigurationen für nur IPv6 werden nicht unterstützt.
5. DNS-Server sind erforderlich, da sie verwendet werden, wenn Ihr Gerät versucht, mit Ihren Cloudspeicher-Dienstanbietern zu kommunizieren. Sie werden auch genutzt, um Ihr Gerät nach dem Namen aufzulösen, wenn es als Dateiserver konfiguriert ist. Auf der Seite **Netzwerkeinstellungen** unter **DNS-Server**:
   
   1. Es werden automatisch ein primärer und ein sekundärer DNS-Server konfiguriert. Wenn Sie sich für die Konfiguration statischer IP-Adressen entscheiden, können Sie DNS-Server angeben. Zur Sicherstellung der hohen Verfügbarkeit wird empfohlen, einen primären und einen sekundären DNS-Server zu konfigurieren.
   2. Klicken Sie auf **Übernehmen**. Die Netzwerkeinstellungen werden angewendet und überprüft.
6. Auf der Seite **Geräteeinstellungen** :
   
   1. Weisen Sie Ihrem Gerät einen eindeutigen **Namen** zu. Dieser Name kann 1 bis 15 Zeichen lang sein und Buchstaben, Zahlen und Bindestriche enthalten.
   2. Klicken Sie auf das **Dateiserver**-Symbol ![](./media/storsimple-ova-deploy3-fs-setup/image6.png) für den **Typ** von Gerät, den Sie erstellen. Mit einem Dateiserver können Sie freigegebene Ordner erstellen.
   3. Da es sich bei Ihrem Gerät um einen Dateiserver handelt, müssen Sie dafür den Beitritt zu einer Domäne durchführen. Geben Sie einen **Domänennamen**ein.
   4. Klicken Sie auf **Übernehmen**.
7. Ein Dialogfeld wird angezeigt. Geben Sie die Anmeldeinformationen für die Domäne im angegebenen Format ein. Klicken Sie auf das Häkchensymbol. Die Anmeldeinformationen für die Domäne werden überprüft. Es wird eine Fehlermeldung angezeigt, wenn die Anmeldeinformationen falsch sind.
   
   ![](./media/storsimple-ova-deploy3-fs-setup/image7.png)
8. Klicken Sie auf **Übernehmen**. Die Geräteeinstellungen werden angewendet und überprüft.
   
   ![](./media/storsimple-ova-deploy3-fs-setup/image8.png)
   
   > [!NOTE]
   > Stellen Sie sicher, dass sich Ihr virtuelles Array in einer eigenen Organisationseinheit für Active Directory befindet und keine Gruppenrichtlinienobjekte darauf angewendet werden oder geerbt sind. Gruppenrichtlinien können Anwendung wie beispielsweise Virenschutzsoftware auf dem StorSimple Virtual Array installieren. Die Installation zusätzlicher Software wird nicht unterstützt und kann zur Beschädigung von Daten führen. 
   > 
   > 
9. Optional: Konfigurieren Sie Ihren Webproxyserver. Die Webproxykonfiguration ist optional. Achten Sie jedoch bei Verwendung eines Webproxys darauf, dass dieser nur hier konfiguriert werden kann.
   
   ![](./media/storsimple-ova-deploy3-fs-setup/image9.png)
   
   Auf der Seite **Webproxy** :
   
   1. Geben Sie die **Webproxy-URL** in diesem Format an: *http://&lt;Host-IP-Adresse oder FDQN&gt;:Portnummer*. Beachten Sie, dass HTTPS-URLs nicht unterstützt werden.
   2. Geben Sie unter **Authentifizierung** die Option **Einfach** oder **Keine** an.
   3. Wenn Sie die Authentifizierung verwenden, müssen Sie auch einen **Benutzernamen** und ein **Kennwort** angeben.
   4. Klicken Sie auf **Übernehmen**. Die konfigurierten Webproxyeinstellungen werden überprüft und angewendet.
10. Optional: Konfigurieren Sie die Zeiteinstellungen für Ihr Gerät, z. B. die Zeitzone und die primären und sekundären NTP-Server. NTP-Server sind für die Zeitsynchronisierung erforderlich, damit Ihr Gerät bei den Clouddienstanbietern authentifiziert werden kann.
    
    ![](./media/storsimple-ova-deploy3-fs-setup/image10.png)
    
    Auf der Seite **Uhrzeiteinstellungen** :
    
    1. Legen Sie über die Dropdownliste die **Zeitzone** basierend auf dem geografischen Standort fest, an dem das Gerät bereitgestellt wird. Die Standardzeitzone für Ihr Gerät ist „PST“. Ihr Gerät verwendet diese Zeitzone für alle geplanten Vorgänge.
    2. Geben Sie einen **primären NTP-Server** für das Gerät an, oder übernehmen Sie den Standardwert „time.windows.com“. Stellen Sie sicher, dass Ihr Netzwerk NTP-Datenverkehr vom Rechenzentrum ins Internet zulässt.
    3. Geben Sie optional einen **sekundären NTP-Server** für Ihr Gerät an.
    4. Klicken Sie auf **Übernehmen**. Die konfigurierten Uhrzeiteinstellungen werden überprüft und angewendet.
11. Konfigurieren Sie die Cloudeinstellungen für Ihr Gerät. In diesem Schritt führen Sie die lokale Gerätekonfiguration durch und registrieren das Gerät dann beim StorSimple Manager-Dienst.
    
    1. Geben Sie den **Dienstregistrierungsschlüssel** ein, den Sie in [Schritt 2: Abrufen des Dienstregistrierungsschlüssels](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) für StorSimple Virtual Array erhalten haben.
    2. Überspringen Sie diesen Schritt, wenn dies das erste Gerät ist, das Sie bei diesem Dienst registrieren, und fahren Sie mit dem nächsten Schritt fort. Falls dies nicht das erste Gerät ist, das Sie bei diesem Dienst registrieren, müssen Sie den **Dienstdaten-Verschlüsselungsschlüssel**angeben. Dieser Schlüssel ist zusammen mit dem Dienstregistrierungsschlüssel zum Registrieren weiterer Geräte beim StorSimple Manager-Dienst erforderlich. Sehen Sie sich die weiteren Informationen zum Abrufen des [Dienstdaten-Verschlüsselungsschlüssels](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) für Ihre lokale Webbenutzeroberfläche an.
    3. Klicken Sie auf **Registrieren**. Das Gerät wird neu gestartet. Sie müssen ggf. zwei bis drei Minuten warten, bis das Gerät erfolgreich registriert wurde. Nachdem das Gerät neu gestartet wurde, wird die Anmeldeseite angezeigt.
       
       ![](./media/storsimple-ova-deploy3-fs-setup/image13.png)
12. Wechseln Sie zurück zum klassischen Azure-Portal. Vergewissern Sie sich auf der Seite **Geräte** , dass das Gerät erfolgreich mit dem Dienst verbunden wurde, indem Sie seinen Status überprüfen. Der Gerätestatus sollte **Aktiv**lauten.

![](./media/storsimple-ova-deploy3-fs-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Schritt 2: Durchführen der erforderlichen Geräteinstallation
Für die Konfiguration Ihres StorSimple-Geräts benötigen Sie Folgendes:

* Wählen Sie ein Speicherkonto, das Sie Ihrem Gerät zuordnen möchten.
* Wählen Sie die Verschlüsselungseinstellungen für die Daten aus, die an die Cloud gesendet werden sollen.

Führen Sie die folgenden Schritte im [klassischen Azure-Portal](https://manage.windowsazure.com/) aus, um die erforderliche Geräteinstallation durchzuführen.

#### <a name="to-complete-the-minimum-device-setup"></a>So führen Sie die minimale Geräteinstallation durch
1. Wählen Sie auf der Seite **Geräte** das gerade erstellte Gerät aus. Für das Gerät wird **Aktiv**angezeigt. Klicken Sie auf den Pfeil neben dem Gerätenamen und dann auf **Schnellstart**.
2. Klicken Sie auf **Geräteinstallation abschließen** , um den Assistenten zum Konfigurieren von Geräten zu starten.
3. Gehen Sie im Assistenten zum Konfigurieren von Geräten auf der Seite **Grundlegende Einstellungen** wie folgt vor:
   
   1. Geben Sie ein Speicherkonto für das Gerät an. Sie können in der Dropdownliste ein vorhandenes Speicherkonto des Abonnements auswählen oder **Weitere hinzufügen** angeben, um ein Konto eines anderen Abonnements auszuwählen.
   2. Definieren Sie die Verschlüsselungseinstellungen für alle ruhenden Daten (AES-Verschlüsselung), die in die Cloud gesendet werden. Aktivieren Sie das Kombinationsfeld **Verschlüsselungsschlüssel für Cloudspeicher aktivieren**, um die Daten zu verschlüsseln. Geben Sie eine Cloudspeicherverschlüsselung ein, die 32 Zeichen lang ist. Geben Sie den Schlüssel erneut ein, um ihn zu bestätigen. Ein 256-Bit-AES-Schlüssel wird zusammen mit dem benutzerdefinierten Schlüssel für die Verschlüsselung verwendet.
   3. Klicken Sie auf das Häkchensymbol ![](./media/storsimple-ova-deploy3-fs-setup/image15.png).
      
      ![](./media/storsimple-ova-deploy3-fs-setup/image16.png)

Die Einstellungen werden jetzt aktualisiert. Nachdem die Einstellungen erfolgreich aktualisiert wurden, ist die Schaltfläche „Geräteinstallation abschließen“ abgeblendet. Sie gelangen zurück auf die Seite **Schnellstart** des Geräts.

 ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)

> [!NOTE]
> Alle anderen Geräteeinstellungen können über die Seite **Konfigurieren** geändert werden.
> 
> 

## <a name="step-3-add-a-share"></a>Schritt 3: Hinzufügen einer Freigabe
Führen Sie die folgenden Schritte im [klassischen Azure-Portal](https://manage.windowsazure.com/) aus, um eine Freigabe zu erstellen.

#### <a name="to-create-a-share"></a>So erstellen Sie eine Freigabe
1. Klicken Sie auf dem Gerät auf der Seite **Schnellstart** auf **Freigabe hinzufügen**. Der Assistent „Freigabe hinzufügen“ wird gestartet.
   
   ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)
2. Gehen Sie auf der Seite **Grundeinstellungen** folgendermaßen vor:
   
   1. Geben Sie einen eindeutigen Namen für die Freigabe an. Der Name muss eine Zeichenfolge mit einer Länge von 3 bis 127 Zeichen sein.
   2. Optional: Geben Sie eine Beschreibung für die Freigabe an. Die Beschreibung ist für die Identifizierung der Freigabenbesitzer hilfreich.
   3. Wählen Sie einen Verwendungstyp für die Freigabe aus. Der Verwendungstyp kann **Mehrstufig** oder **Lokal** lauten. „Mehrstufig“ ist die Standardeinstellung. Für Workloads, die lokale Garantien, niedrige Latenzzeiten und höhere Leistung benötigen, wählen Sie **Lokal** für die Freigabe aus. Wählen Sie für alle anderen Daten **Mehrstufig** für die Freigabe aus.
   
   Eine lokale Freigabe wird mit vollständiger Speicherzuweisung (Thick Provisioning) bereitgestellt und stellt sicher, dass die primären Daten auf der Freigabe lokal auf dem Gerät verbleiben und nicht in die Cloud übergehen. Eine mehrstufige Freigabe dagegen wird mit schlanker Speicherzuweisung bereitgestellt. Wenn Sie eine mehrstufige Freigabe erstellen, werden 10 % des Speicherplatzes auf der lokalen Ebene und 90 % des Speicherplatzes in der Cloud bereitgestellt. Wenn Sie beispielsweise ein Volume mit 1 TB bereitstellen, befinden sich 100 GB lokal, und 900 GB werden in der Cloud zum Anordnen der Daten genutzt. Dies bedeutet wiederum Folgendes: Wenn der gesamte lokale Speicherplatz auf dem Gerät aufgebraucht ist, können Sie keine mehrstufige Freigabe bereitstellen.
3. Geben Sie die bereitgestellte Kapazität für die Freigabe an. Beachten Sie, dass die angegebene Kapazität kleiner als die verfügbare Kapazität sein sollte. Bei Verwendung einer mehrstufigen Freigabe sollte die Freigabegröße zwischen 500 GB und 20 TB liegen. Geben Sie für eine lokale Freigabe eine Freigabegröße zwischen 50 GB und 2 TB an. Verwenden Sie die verfügbare Kapazität als Richtschnur für die Bereitstellung einer Freigabe. Wenn die verfügbare lokale Kapazität 0 GB beträgt, können Sie keine lokalen oder mehrstufigen Freigaben bereitstellen.
   
   ![](./media/storsimple-ova-deploy3-fs-setup/image18.png)
4. Klicken Sie auf den Pfeil ![](./media/storsimple-ova-deploy3-fs-setup/image19.png) , um zur nächsten Seite zu wechseln.
5. Weisen Sie die Berechtigungen auf der Seite **Zusätzliche Einstellungen** dem Benutzer oder der Gruppe zu, der bzw. die auf die Freigabe zugreift. Geben Sie den Namen des Benutzers oder der Benutzergruppe im Format *<mailto:john@contoso.com>* ein. Es wird empfohlen, eine Benutzergruppe (anstelle eines einzelnen Benutzers) zu verwenden, um für Administratorberechtigungen den Zugriff auf diese Freigaben zuzulassen. Nachdem Sie hier die Berechtigungen zugewiesen haben, können Sie Windows-Explorer verwenden, um diese Berechtigungen zu ändern.
   
   ![](./media/storsimple-ova-deploy3-fs-setup/image20.png)
6. Klicken Sie auf das Häkchensymbol ![](./media/storsimple-ova-deploy3-fs-setup/image21.png). Es wird eine Freigabe mit den angegebenen Einstellungen erstellt. Standardmäßig werden für die Freigabe die Überwachung und die Sicherung aktiviert.

## <a name="step-4-connect-to-the-share"></a>Schritt 4: Herstellen der Verbindung mit der Freigabe
Als Nächstes müssen Sie eine Verbindung mit der bzw. den Freigaben herstellen, die Sie im vorherigen Schritt erstellt haben. Führen Sie die folgenden Schritte auf Ihrem Windows Server-Host aus.

#### <a name="to-connect-to-the-share"></a>So stellen Sie eine Verbindung mit der Freigabe her
1. Drücken Sie ![](./media/storsimple-ova-deploy3-fs-setup/image22.png) + R. Geben Sie im Fenster „Ausführen“ als Pfad *\\<file server name>* an, und ersetzen Sie *file server name* durch den Gerätenamen, den Sie Ihrem Dateiserver zugewiesen haben. Klicken Sie auf **OK**.
   
   ![](./media/storsimple-ova-deploy3-fs-setup/image23.png)
2. Der Explorer wird geöffnet. Die von Ihnen erstellten Freigaben sollten jetzt als Ordner angezeigt werden. Wählen Sie eine Freigabe (einen Ordner) aus, und doppelklicken Sie darauf, um den Inhalt anzuzeigen.
   
   ![](./media/storsimple-ova-deploy3-fs-setup/image24.png)
3. Sie können den Freigaben jetzt Dateien hinzufügen und eine Sicherung durchführen.

![Video-Symbol](./media/storsimple-ova-deploy3-fs-setup/video_icon.png) **Video verfügbar**

Im Video wird gezeigt, wie Sie ein StorSimple Virtual Array als Dateiserver konfigurieren und registrieren können.

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Configure-a-StorSimple-Virtual-Array/player]
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie die lokale Webbenutzeroberfläche verwenden, um [Ihr StorSimple Virtual Array zu verwalten](storsimple-ova-web-ui-admin.md).




<!--HONumber=Nov16_HO3-->


