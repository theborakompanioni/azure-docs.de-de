---
title: "Erstellen eines VM-Images für Azure Marketplace | Microsoft-Dokumentation"
description: "Ausführliche Anleitung zum Erstellen eines VM-Images, um es anderen Benutzern über den Azure Marketplace zum Kauf anzubieten"
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 5c937b8e-e28d-4007-9fef-624046bca2ae
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 09/30/2016
ms.author: hascipio; v-divte
translationtype: Human Translation
ms.sourcegitcommit: c40545833da86426d3e71955b8eb8627db3c1e4b
ms.openlocfilehash: 6e3088d372b6036c37828549fda2c9c39092e478


---
# <a name="guide-to-create-a-virtual-machine-image-for-the-azure-marketplace"></a>Anleitung zum Erstellen eines VM-Images für Azure Marketplace
In diesem Artikel ( **Schritt 2**) werden Sie durch die Vorbereitung der virtuellen Festplatten (VHDs) geführt, die Sie im Azure Marketplace bereitstellen möchten. Ihre VHDs bilden die Grundlage Ihrer SKU. Der Prozess variiert in Abhängigkeit davon, ob Sie eine Linux- oder Windows-basierte SKU bereitstellen. Dieser Artikel deckt beide Szenarien ab. Dieser Vorgang kann parallel zum [Erstellen und Registrieren eines Kontos][link-acct-creation] ausgeführt werden.

## <a name="1-define-offers-and-skus"></a>1. Definieren von Angeboten und SKUs
In diesem Abschnitt lernen Sie, wie Sie die Angebote und die zugehörigen SKUs definieren.

Ein Angebot ist ein „übergeordnetes Element“ für alle darin enthaltenen SKUs. Sie können mehrere Angebote definieren. Wie Sie Ihre Angebote aufbauen, bleibt Ihnen überlassen. Wenn ein Angebot in die Stagingphase überführt wird, werden alle SKUs mit einbezogen. Die SKU-IDs sollten sorgfältig geprüft werden, da sie in der URL sichtbar sind.

* Azure.com: http://azure.microsoft.com/marketplace/partners/{Partner-Namespace}/{Angebots-ID}-{SKU-ID}
* Azure-Vorschauportal: https://portal.azure.com/#gallery/{Herausgeber-Namespace}.{Angebots-ID}{SKU-ID}  

SKU ist der Handelsname für ein VM-Image. Ein VM-Image enthält einen Betriebssystem-Datenträger und null oder mehr Datenträger. Es handelt sich im Wesentlichen um das komplette Speicherprofil für einen virtuellen Computer. Pro Datenträger wird eine VHD benötigt. Auch für leere Datenträger wird für die Erstellung eine VHD benötigt.

Fügen Sie unabhängig vom verwendeten Betriebssystem immer nur die von der SKU benötigte Mindestanzahl an Datenträgern hinzu. Kunden können Datenträger, die Teil eines Images sind, zum Zeitpunkt der Bereitstellung nicht entfernen. Sie haben aber immer die Möglichkeit, während oder nach der Bereitstellung Datenträger hinzuzufügen, sofern diese benötigt werden.

> [!IMPORTANT]
> **Ändern Sie nicht die Anzahl der Datenträger in einer neuen Imageversion.** Wenn Sie Datenträger im Image neu konfigurieren müssen, definieren Sie eine neue SKU. Das Veröffentlichen einer neuen Imageversion mit einer anderen Anzahl von Datenträgern birgt das Risiko des Misslingens der neuen Bereitstellung basierend auf der neuen Imageversion bei automatischer Skalierung, automatischen Bereitstellungen von Lösungen mithilfe von ARM-Vorlagen und anderen Szenarien.
> 
> 

### <a name="11-add-an-offer"></a>1.1 Hinzufügen eines Angebots
1. Melden Sie sich mit Ihrem Verkäuferkonto beim [Veröffentlichungsportal][link-pubportal] an.
2. Wählen Sie die Registerkarte **Virtuelle Computer** des Veröffentlichungsportals. Geben Sie im angezeigten Eingabefeld den Angebotsnamen ein. Der Angebotsname ist in der Regel der Name des Produkts/Diensts, das bzw. den Sie im Azure Marketplace verkaufen möchten.
3. Klicken Sie auf **Erstellen**.

### <a name="12-define-a-sku"></a>1.2 Definieren einer SKU
Nach dem Hinzufügen eines Angebots müssen Sie Ihre SKUs definieren und angeben. Sie können mehrere Angebote definieren, und jedes Angebot kann mehrere SKUs enthalten. Wenn ein Angebot in die Stagingphase überführt wird, werden alle SKUs mit einbezogen.

1. **Fügen Sie eine SKU hinzu.** Für die SKU ist ein Bezeichner erforderlich, der in der URL verwendet wird. Der Bezeichner muss in Ihrem Veröffentlichungsprofil eindeutig sein. Allerdings besteht keine Gefahr eines ID-Konflikts mit anderen Herausgebern.
   
   > [!NOTE]
   > Angebots- und SKU-Bezeichner werden in der Angebots-URL im Marketplace angezeigt.
   > 
   > 
2. **Fügen Sie eine zusammenfassende Beschreibung für die SKU hinzu.** Zusammenfassende Beschreibungen sind für Kunden sichtbar. Achten Sie also darauf, dass sie gut lesbar sind. Vor der Phase „Für Stagingumgebung freigeben“ besteht keine Notwendigkeit, diese Informationen zu sperren. Bis dahin können sie nach Belieben bearbeitet werden.
3. Erwerben Sie die genehmigten Versionen von Windows Server über die vorgeschlagenen Links, wenn Sie mit Windows-basierten SKUs arbeiten.

## <a name="2-create-an-azure-compatible-vhd-linux-based"></a>2. Erstellen einer Azure-kompatiblen VHD (Linux-basiert)
Im Mittelpunkt dieses Abschnitts stehen bewährte Methoden zum Erstellen eines Linux-basierten VM-Images für den Azure Marketplace. Eine schrittweise exemplarische Vorgehensweise finden Sie in der folgenden Dokumentation: [Erstellen und Hochladen einer virtuellen Festplatte, die das Linux-Betriebssystem enthält](../virtual-machines/virtual-machines-linux-classic-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="3-create-an-azure-compatible-vhd-windows-based"></a>3. Erstellen einer Azure-kompatiblen VHD (Windows-basiert)
Im Mittelpunkt dieses Abschnitts stehen die Schritte zum Erstellen einer SKU auf Basis von Windows Server für den Azure Marketplace.

### <a name="31-ensure-that-you-are-using-the-correct-base-vhds"></a>3.1. Verwenden der richtigen Basis-VHDs
Die Betriebssystem-VHD für Ihr VM-Image muss auf einem von Azure genehmigten Basisimage basieren, das Windows Server oder SQL Server enthält.

Erstellen Sie zunächst einen virtuellen Computer aus einem der folgenden Images, die sich im [Microsoft Azure-Portal][link-azure-portal] befinden:

* Windows Server ([2012 R2 Datacenter][link-datactr-2012-r2], [2012 Datacenter][link-datactr-2012], [2008 R2 SP1][link-datactr-2008-r2])
* SQL Server 2014 ([Enterprise][link-sql-2014-ent], [Standard][link-sql-2014-std], [Web][link-sql-2014-web])
* SQL Server 2012 SP2 ([Enterprise][link-sql-2012-ent], [Standard][link-sql-2012-std], [Web][link-sql-2012-web])

Diese Verknüpfungen finden Sie auch im Veröffentlichungsportal auf der SKU-Seite.

> [!TIP]
> Wenn Sie das aktuelle Azure-Portal oder PowerShell verwenden, sind am 8. September 2014 und später veröffentlichte Windows Server-Images genehmigt.
> 
> 

### <a name="32-create-your-windows-based-vm"></a>3.2 Erstellen des virtuellen Windows-basierten Computers
Im Microsoft Azure-Portal können Sie den virtuellen Computer auf der Grundlage eines genehmigten Basisimages in nur wenigen einfachen Schritten erstellen. Im Folgenden finden Sie eine Übersicht über den Prozess:

1. Wählen Sie auf der Seite des Basisimages die Option **Virtuellen Computer erstellen** aus, um zum neuen [Microsoft Azure-Portal][link-azure-portal] zu gelangen.
   
    ![Abbildung][img-acom-1]
2. Melden Sie sich mit dem Microsoft-Konto und -Kennwort für das gewünschte Azure-Abonnement beim Portal an.
3. Befolgen Sie die Anweisungen zum Erstellen eines virtuellen Computers auf der Grundlage des von Ihnen ausgewählten Basisimages. Sie müssen einen Hostnamen (Name des Computers), einen Benutzernamen (als Administrator registriert) und ein Kennwort für den virtuellen Computer angeben.
   
    ![Abbildung][img-portal-vm-create]
4. Wählen Sie die Größe des bereitzustellenden virtuellen Computers aus:
   
    a.    Wenn Sie planen, die VHD lokal zu entwickeln, spielt die Größe keine Rolle. Die Verwendung eines kleineren virtuellen Computers wird empfohlen.
   
    b.    Wenn Sie vorhaben, das Image in Azure zu entwickeln, sollten Sie eine der empfohlenen VM-Größen für das ausgewählte Image verwenden.
   
    c.    Preisinformationen finden Sie im Portal in der Auswahl **Empfohlener Tarif** . Hier sind die drei empfohlenen Größen angegeben, die vom Herausgeber bereitgestellt werden. (In diesem Fall ist der Herausgeber Microsoft.)
   
    ![Abbildung][img-portal-vm-size]
5. Legen Sie Eigenschaften fest:
   
    a.    Zur schnellen Bereitstellung können Sie die Standardwerte für die Eigenschaften unter **Optionale Konfiguration** und **Ressourcengruppe** beibehalten.
   
    b.    Unter **Speicherkonto**können Sie optional das Speicherkonto auswählen, unter dem die Betriebssystem-VHD gespeichert wird.
   
    c.    Unter **Ressourcengruppe**können Sie auf Wunsch optional die logische Gruppe auswählen, in der der virtuelle Computer abgelegt werden soll.
6. Wählen Sie den **Speicherort** für die Bereitstellung aus:
   
    a.    Wenn Sie planen, die VHD lokal zu entwickeln, spielt der Ort keine Rolle, da Sie das Image später in Azure hochladen.
   
    b.    Wenn Sie planen, das Image in Azure zu entwickeln, sollten Sie gleich zu Anfang eine der US-amerikanischen Microsoft Azure-Regionen auswählen. Hierdurch beschleunigen Sie den VHD-Kopiervorgang, den Microsoft für Sie ausführt, wenn Sie Ihr Image zur Zertifizierung einsenden.
   
    ![Abbildung][img-portal-vm-location]
7. Klicken Sie auf **Erstellen**. Die Bereitstellung des virtuellen Computers wird gestartet. Die Bereitstellung ist innerhalb weniger Minuten abgeschlossen, und Sie können mit dem Erstellen des Images für Ihre SKU beginnen.

### <a name="33-develop-your-vhd-in-the-cloud"></a>3.3 Entwickeln der VHD in der Cloud
Es wird dringend empfohlen, die VHD in der Cloud mithilfe des Remotedesktopprotokolls (RDP) zu entwickeln. Die Verbindung zum RDP wird mit dem Benutzernamen und dem Kennwort hergestellt, die Sie während der Bereitstellung angegeben haben.

> [!IMPORTANT]
> Wenn Sie die VHD lokal entwickeln (nicht zu empfehlen), helfen Ihnen die Informationen unter [Erstellen eines lokalen Images eines virtuellen Computers für Azure Marketplace](marketplace-publishing-vm-image-creation-on-premise.md)weiter. Beim Entwickeln in der Cloud ist das Herunterladen der VHD nicht notwendig.
> 
> 

**Herstellen der Verbindung mit dem [Microsoft Azure-Portal][link-azure-portal] über RDP**

1. Wählen Sie **Durchsuchen** > **VMs**.
2. Das Blatt „Virtuelle Computer“ wird geöffnet. Stellen Sie sicher, dass der virtuelle Computer, mit dem Sie eine Verbindung herstellen möchten, ausgeführt wird, und wählen Sie ihn dann in der Liste der bereitgestellten virtuellen Computer aus.
3. Es wird ein Blatt geöffnet, auf dem der ausgewählte virtuelle Computer beschrieben wird. Klicken Sie am oberen Rand auf **Verbinden**.
4. Sie werden aufgefordert, den Benutzernamen und das Kennwort einzugeben, den bzw. das Sie während der Bereitstellung angegeben haben.

**Herstellen der Verbindung über RDP mit PowerShell**

Zum Herunterladen einer Remotedesktopdatei auf einen lokalen Computer können Sie das [Cmdlet Get-AzureRemoteDesktopFile][link-technet-2] verwenden. Hierzu müssen Sie den Namen des Diensts und den Namen des virtuellen Computers kennen. Wenn Sie den virtuellen Computer im [Microsoft Azure-Portal][link-azure-portal] erstellt haben, finden Sie diese Informationen unter „VM-Eigenschaften“:

1. Wählen Sie im Microsoft Azure-Portal **Durchsuchen** > **VMs**.
2. Das Blatt „Virtuelle Computer“ wird geöffnet. Wählen Sie den virtuellen Computer aus, den Sie bereitgestellt haben.
3. Es wird ein Blatt geöffnet, auf dem der ausgewählte virtuelle Computer beschrieben wird.
4. Klicken Sie auf **Eigenschaften**.
5. Der erste Teil des Domänennamens ist der Dienstname. Der Hostname ist der Name des virtuellen Computers.
   
    ![Abbildung][img-portal-vm-rdp]
6. Das Cmdlet zum Herunterladen der RDP-Datei für den erstellten virtuellen Computer auf den lokalen Desktop des Administrators lautet wie folgt:
   
        Get-AzureRemoteDesktopFile -ServiceName “baseimagevm-6820cq00” -Name “BaseImageVM” –LocalPath “C:\Users\Administrator\Desktop\BaseImageVM.rdp”

Weitere Informationen zu RDP finden Sie im MSDN-Artikel [Herstellen einer Verbindung mit einem virtuellen Azure-Computer über RDP oder SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx).

**Konfigurieren eines virtuellen Computers und Erstellen der SKU**

Verwenden Sie nach dem Herunterladen der Betriebssystem-VHD Hyper-V, und konfigurieren Sie einen virtuellen Computer, um mit dem Erstellen einer SKU zu beginnen. Eine ausführliche Anleitung finden Sie unter folgendem TechNet-Link: [Installieren von Hyper-V und Erstellen eines virtuellen Computers](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="34-choose-the-correct-vhd-size"></a>3.4 Auswählen der richtigen VHD-Größe
Die Windows-Betriebssystem-VHD in Ihrem VM-Image sollte als VHD mit 128 GB und mit festem Format erstellt werden.  

Beträgt die physische Größe weniger als 128 GB, sollte die VHD von geringer Dichte sein. Die bereitgestellten Windows- und SQL Server-Images erfüllen diese Anforderungen. Ändern Sie weder das Format noch die Größe der VHD.  

Datenträger können bis zu 1 TB groß sein. Bedenken Sie bei der Entscheidung über die Datenträgergröße, dass Kunden die Größe von VHDs in einem Image zum Zeitpunkt der Bereitstellung nicht verändern können. Datenträger-VHDs sollten als VHDs mit festem Format erstellt werden. Sie sollten außerdem eine geringe Dichte aufweisen. Datenträger können leer sein oder Daten enthalten.

### <a name="35-install-the-latest-windows-patches"></a>3.5 Installieren der neuesten Windows-Patches
Die Basisimages enthalten die neuesten, bis zum Veröffentlichungsdatum erschienenen Patches. Vergewissern Sie sich vor der Veröffentlichung der von Ihnen erstellten Betriebssystem-VHD, dass Windows Update ausgeführt wurde und dass alle kritischen und wichtigen Sicherheitsupdates installiert wurden.

### <a name="36-perform-additional-configuration-and-schedule-tasks-as-necessary"></a>3.6 Konfigurieren zusätzlicher Einstellungen und Planen von Aufgaben nach Bedarf
Falls zusätzliche Einstellungen konfiguriert werden müssen, können Sie eine geplante Aufgabe einrichten, die beim Start ausgeführt wird. So ist es möglich, nach der Bereitstellung des virtuellen Computers letzte Änderungen daran vorzunehmen:

* Es gilt als bewährte Methode, die Aufgabe so zu konfigurieren, dass sie sich nach der erfolgreichen Ausführung selbst löscht.
* Die Konfiguration sollte sich nur auf die Laufwerke C oder D stützen, denn diese beiden Laufwerke sind die einzigen, die mit Sicherheit immer vorhanden sind. Laufwerk C ist der Betriebssystem-Datenträger, und Laufwerk D ist der temporäre lokale Datenträger.

### <a name="37-generalize-the-image"></a>3.7 Generalisieren des Images
Alle Images im Azure Marketplace müssen allgemein wiederverwendbar sein. Anders ausgedrückt: Die Betriebssystem-VHD muss generalisiert werden:

* Unter Windows sollte für das Image eine Systemvorbereitung mit „Sysprep“ durchgeführt werden. Einstellungen, die den Befehl **sysprep** nicht unterstützen, dürfen nicht konfiguriert werden.
* Sie können den folgenden Befehl über das Verzeichnis „%windir%\System32\Sysprep“ ausführen.
  
        sysprep.exe /generalize /oobe /shutdown
  
  Anleitungen zur Systemvorbereitung des Betriebssystems mit „sysprep“ finden Sie in Schritt 1 des folgenden MSDN-Artikels: [Erstellen und Hochladen einer Windows Server-VHD nach Azure](../virtual-machines/virtual-machines-windows-classic-createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="4-deploy-a-vm-from-your-vhds"></a>4. Bereitstellen eines virtuellen Computers auf Basis der VHDs
Nachdem Sie Ihre VHDs (generalisierte Betriebssystem-VHD und null oder mehr Datenträger-VHDs) in ein Azure-Speicherkonto hochgeladen haben, können Sie sie als Benutzer-VM-Image registrieren. Anschließend können Sie das Image testen. Beachten Sie Folgendes: Da Ihre Betriebssystem-VHD generalisiert wurde, können Sie den virtuellen Computer nicht direkt bereitstellen, indem Sie die VHD-URL angeben.

Weitere Informationen zu VM-Images finden Sie in den folgenden Blogbeiträgen:

* [VM-Image](https://azure.microsoft.com/blog/vm-image-blog-post/)
* [VM Image PowerShell How To (VM Image PowerShell – Vorgehensweisen; in englischer Sprache)](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)
* [About VM images in Azure (Informationen zu VM-Images in Azure; in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn790290.aspx)

### <a name="set-up-the-necessary-tools-powershell-and-azure-cli"></a>Einrichten der erforderlichen Tools, von PowerShell und der Azure-CLI
* [Einrichten von PowerShell](/powershell/azureps-cmdlets-docs)
* [Einrichten der Azure-CLI](../xplat-cli-install.md)

### <a name="41-create-a-user-vm-image"></a>4.1 Erstellen eines Benutzer-VM-Images
#### <a name="capture-vm"></a>Erfassen eines virtuellen Computers
Unter folgenden Links finden Sie Anweisungen zum Erfassen des virtuellen Computers mit einer API, mit PowerShell oder mit der Azure-CLI.

* [API](https://msdn.microsoft.com/library/mt163560.aspx)
* [PowerShell](../virtual-machines/virtual-machines-windows-capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure-CLI](../virtual-machines/virtual-machines-linux-capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="generalize-image"></a>Generalisieren eines Images
Unter folgenden Links finden Sie Anweisungen zum Erfassen des virtuellen Computers mit einer API, mit PowerShell oder mit der Azure-CLI.

* [API](https://msdn.microsoft.com/library/mt269439.aspx)
* [PowerShell](../virtual-machines/virtual-machines-windows-capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure-CLI](../virtual-machines/virtual-machines-linux-capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="42-deploy-a-vm-from-a-user-vm-image"></a>4.2 Bereitstellen eines virtuellen Computers aus einem Benutzer-VM-Image
Zum Bereitstellen eines virtuellen Computers aus einem Benutzer-VM-Image können Sie das aktuelle [Azure-Portal](https://manage.windowsazure.com) oder PowerShell verwenden.

**Bereitstellen eines virtuellen Computers aus dem aktuellen Azure-Portal**

1. Navigieren Sie zu **Neu** > **Compute** > **Virtueller Computer** > **From gallery** (Aus Katalog)
   
    ![Abbildung][img-manage-vm-new]
2. Wechseln Sie zu **Meine Images**, und wählen Sie dann das VM-Image aus, aus dem ein virtueller Computer bereitgestellt werden soll:
   
   1. Achten Sie genau darauf, welches Image Sie auswählen, da unter **Meine Images** sowohl Betriebssystemimages als auch VM-Images aufgeführt werden.
   2. Die Datenträgeranzahl lässt Rückschlüsse auf die Art des bereitgestellten Images zu, da die Mehrheit der VM-Images mehr als einen Datenträger enthält. Es ist jedoch möglich, dass ein VM-Image nur über einen Betriebssystem-Datenträger verfügt. Für die **Anzahl der Datenträger** wird in diesem Fall „1“ angegeben.
      
      ![Abbildung][img-manage-vm-select]
3. Folgen Sie den Schritten im Assistenten zum Erstellen virtueller Computer, und geben Sie den Namen und die Größe des virtuellen Computers, den Ort, den Benutzernamen und das Kennwort an.

**Bereitstellen eines virtuellen Computers mit PowerShell**

Zum Bereitstellen eines großen virtuellen Computers aus dem soeben erstellten generalisierten VM-Image können Sie die folgenden Cmdlets verwenden.

    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot

> [!IMPORTANT]
> Lesen Sie die Informationen unter [Behandlung häufig auftretender Probleme bei der Erstellung virtueller Festplatten], wenn Sie weitere Unterstützung benötigen.
> 
> 

## <a name="5-obtain-certification-for-your-vm-image"></a>5. Erwerb der Zertifizierung für das VM-Image
Die nächste Aufgabe in der Vorbereitung des VM-Images für den Azure Marketplace ist die Zertifizierung.

Dieser Prozess umfasst mehrere Schritte: Ausführen eines speziellen Zertifizierungstools, Hochladen der Ergebnisse der Überprüfung in den Azure-Container, in dem sich die VHDs befinden, Hinzufügen eines Angebots, Definieren der SKU und Einsenden des VM-Images zur Zertifizierung.

### <a name="51-download-and-run-the-certification-test-tool-for-azure-certified"></a>5.1 Herunterladen und Ausführen des „Certification Test Tool for Azure Certified“
Das Zertifizierungstool wird auf einem laufenden virtuellen Computer ausgeführt, der aus Ihrem Benutzer-VM-Image bereitgestellt wurde, um sicherzustellen, dass das VM-Image mit Microsoft Azure kompatibel ist. Das Tool überprüft, ob die Leitlinien und Anforderungen in Bezug auf die Vorbereitung Ihrer VHD eingehalten und erfüllt wurden. Die Ausgabe des Tools ist ein Kompatibilitätsbericht, der beim Anfordern der Zertifizierung in das Veröffentlichungsportal hochgeladen werden muss.

Das Zertifizierungstool funktioniert mit virtuellen Windows- und Linux-Computern. Die Verbindung mit virtuellen Windows-basierten Computern wird über PowerShell hergestellt, und mit virtuellen Linux-Computern über SSH.Net:

1. Laden Sie das Zertifizierungstool zunächst von der [Microsoft-Downloadwebsite][link-msft-download] herunter.
2. Öffnen Sie das Zertifizierungstool, und klicken Sie dann auf die Schaltfläche **Neuen Test starten** .
3. Geben Sie auf dem Bildschirm **Testinformationen** einen Namen für den Testlauf ein.
4. Geben Sie an, ob Ihr virtueller Computer unter Linux oder Windows ausgeführt wird. Wählen Sie je nach Auswahl die nachfolgenden Optionen aus.

### <a name="connect-the-certification-tool-to-a-linux-vm-image"></a>**Verbinden des Zertifizierungstools mit einem Linux-VM-Image**
1. Wählen Sie den SSH-Authentifizierungsmodus aus: Kennwort oder Schlüsseldatei.
2. Geben Sie bei kennwortbasierter Authentifizierung den DNS-Namen (Domain Name System), den Benutzernamen und das Kennwort ein.
3. Geben Sie bei Authentifizierung per Schlüsseldatei den DNS-Namen, Benutzernamen und Speicherort des privaten Schlüssels an.
   
   ![Kennwortauthentifizierung eines Linux-VM-Images][img-cert-vm-pswd-lnx]
   
   ![Authentifizierung per Schlüsseldatei eines Linux-VM-Images][img-cert-vm-key-lnx]

### <a name="connect-the-certification-tool-to-a-windows-based-vm-image"></a>**Verbinden des Zertifizierungstools mit einem Windows-basierten VM-Image**
1. Geben Sie den vollqualifizierten VM-DNS-Namen ein (z. B. MyVMName.Cloudapp.net).
2. Geben Sie den Benutzernamen und das Kennwort ein.
   
   ![Kennwortauthentifizierung eines Windows-VM-Images][img-cert-vm-pswd-win]

Wählen Sie nach dem Auswählen der richtigen Optionen für Ihr Linux- oder Windows-basiertes VM-Image die Option **Verbindung testen**, um sicherzustellen, dass von SSH.Net bzw. PowerShell eine gültige Verbindung für Testzwecke aufgebaut wurde. Wählen Sie nach dem Herstellen einer Verbindung die Option **Weiter**, um den Test zu starten.

Nach Abschluss des Tests erhalten Sie die Ergebnisse (Erfolgreich/Fehler/Warnung) für jedes einzelne Testelement.

![Testfälle für Linux-VM-Images][img-cert-vm-test-lnx]

![Testfälle für Windows-VM-Images][img-cert-vm-test-win]

Wenn einer dieser Tests fehlschlägt, wird das Image nicht zertifiziert. Überprüfen Sie in diesem Fall die Anforderungen, und nehmen Sie ggf. Änderungen vor.

Nach dem automatisierten Test werden Sie zur Eingabe zusätzlicher Informationen zum VM-Image über einen Bildschirm mit einem Fragebogen aufgefordert.  Beantworten Sie die Fragen, und wählen Sie dann **Weiter**.

![Fragebogen im Zertifizierungstool][img-cert-vm-questionnaire]

![Fragebogen im Zertifizierungstool][img-cert-vm-questionnaire-2]

Nachdem Sie den Fragebogen ausgefüllt haben, können Sie zusätzliche Informationen angeben, z. B. SSH-Zugriffsinformationen für das Linux-VM-Image und Erklärungen für fehlgeschlagene Bewertungen. Sie können die Testergebnisse und Protokolldateien für die ausgeführten Testfälle zusätzlich zu Ihren Antworten aus dem Fragebogen herunterladen. Speichern Sie die Ergebnisse in demselben Container wie die VHDs.

![Zertifizierungstestergebnisse speichern][img-cert-vm-results]

### <a name="52-get-the-shared-access-signature-uri-for-your-vm-images"></a>5.2 Abrufen des Shared Access Signature-URI für Ihre VM-Images
Im Laufe des Veröffentlichungsprozesses geben Sie die Uniform Resource Identifiers (URIs) an, die zu den einzelnen VHDs führen, die Sie für Ihre SKU erstellt haben. Microsoft benötigt während des Zertifizierungsprozesses Zugriff auf diese VHDs. Daher müssen Sie für jede VHD einen Shared Access Signature-URI erstellen. Dies ist der URI, der auf der Registerkarte **Images** im Veröffentlichungsportal eingegeben werden muss.

Der erstellte Shared Access Signature-URI sollte die folgenden Anforderungen erfüllen:

* Beim Generieren von Shared Access Signature-URIs für Ihre VHDs sind die Berechtigungen „Auflisten“ und „Lesen“ ausreichend. Gewähren Sie keinen Schreib- oder Löschzugriff.
* Die Zugriffsdauer sollte mindestens sieben Werktage ab der Erstellung des Shared Access Signature-URI betragen.
* Geben Sie eine Uhrzeit 15 Minuten vor der aktuellen Uhrzeit an, um unmittelbare Fehler aufgrund von Zeitabweichungen zu vermeiden.

Eine Anleitung zum Erstellen eines Shared Access Signature-URI finden Sie unter [Shared Access Signatures, Teil 1: Grundlagen zum SAS-Modell][link-azure-1] und [Shared Access Signatures, Teil 2: Erstellen und Verwenden einer SAS mit dem Azure-Blob-Dienst][link-azure-2].

Anstatt einen gemeinsam verwendeten Zugriffsschlüssel per Code zu erstellen, können Sie auch Speichertools wie den [Azure-Speicher-Explorer][link-azure-codeplex] verwenden.

**Generieren eines gemeinsam verwendeten Zugriffsschlüssels mit Azure-Speicher-Explorer**

1. Laden Sie den [Azure-Speicher-Explorer][link-azure-codeplex] 6 oder höher von CodePlex herunter.
2. Öffnen Sie die Anwendung, nachdem sie installiert wurde.
3. Klicken Sie auf **Konto hinzufügen**.
   
    ![Abbildung][img-azstg-add]
4. Geben Sie den Speicherkontonamen, den Speicherkontoschlüssel und die Domäne der Speicherendpunkte an. Wählen Sie nicht **HTTPS verwenden**aus.
   
    ![Abbildung][img-azstg-setup-1]
5. Azure-Speicher-Explorer ist jetzt mit Ihrem speziellen Speicherkonto verbunden. Anschließend werden alle Container im Speicherkonto angezeigt. Wählen Sie den Container, in den Sie die VHD-Datei mit dem Betriebssystem-Datenträger (auch normale Datenträger, wenn dies für Ihr Szenario zutrifft) kopiert haben.
   
    ![Abbildung][img-azstg-setup-2]
6. Nach der Auswahl des Blob-Containers startet der Azure-Speicher-Explorer und zeigt die Dateien im Container an. Wählen Sie die Image-Datei (.vhd), die gesendet werden soll.
   
    ![Abbildung][img-azstg-setup-3]
7. Nachdem Sie die VHD-Datei im Container ausgewählt haben, klicken Sie auf die Registerkarte **Sicherheit** .
   
    ![Abbildung][img-azstg-setup-4]
8. Lassen Sie die Standardeinstellung im Dialogfeld **Blob-Containersicherheit** auf der Registerkarte **Zugriffsebene** unverändert, und klicken Sie dann auf die Registerkarte **Shared Access Signatures**.
   
    ![Abbildung][img-azstg-setup-5]
9. Führen Sie die folgenden Schritte aus, um einen Shared Access Signature-URI für das VHD-Image zu generieren:
   
    ![Abbildung][img-azstg-setup-6]
   
    a.    **Zugriff erlaubt ab**: Wählen Sie den Tag vor dem aktuellen Datum aus, um in Bezug auf die UTC-Zeit sicherzugehen. Wählen Sie beispielsweise 05.10.2014 aus, wenn das aktuelle Datum der 6. Oktober 2014 ist.
   
    b.    **Zugriff erlaubt bis**: Wählen Sie ein Datum aus, das mindestens sieben bis acht Tage nach dem Datum liegt, das Sie unter **Zugriff erlaubt ab** angegeben haben.
   
    c.    **Zulässige Aktionen**: Wählen Sie die Berechtigungen **Auflisten** und **Lesen** aus.
   
    d.    Wenn Sie die VHD-Datei richtig ausgewählt haben, wird Ihre Datei unter **Blob-Name für Zugriff** mit der Erweiterung „.vhd“ angezeigt.
   
    e.    Klicken Sie auf **Signatur generieren**.
   
    f.    Überprüfen Sie im **generierten Shared Access Signature-URI dieses Containers**Folgendes (wie oben beschrieben):
   
   * Stellen Sie sicher, dass die URL nicht mit „https“ beginnt.
   * Stellen Sie sicher, dass Ihr Imagedateiname und „.vhd“ im URI enthalten sind.
   * Stellen Sie sicher, dass am Ende der Signatur „=rl“ angezeigt wird. Dies ist der Beweis, dass der Zugriff für „Lesen“ und „Auflisten“ erfolgreich bereitgestellt wurde.
     
     g.    Um sicherzustellen, dass der generierte Shared Access Signature-URI funktioniert, klicken Sie auf **In Browser testen**. Der Downloadvorgang sollte gestartet werden.
10. Kopieren Sie den Shared Access Signature-URI. Dies ist der URI, der in das Veröffentlichungsportal eingefügt wird.
11. Wiederholen Sie diese Schritte für jede VHD in der SKU.

### <a name="53-provide-information-about-the-vm-image-and-request-certification-in-the-publishing-portal"></a>5.3 Angeben von Informationen zum VM-Image und Anfordern der Zertifizierung im Veröffentlichungsportal
Nach dem Erstellen des Angebots und der SKU müssen Sie die zu dieser SKU gehörigen Imagedetails eingeben:

1. Wechseln Sie zum [Veröffentlichungsportal][link-pubportal], und melden Sie sich mit Ihrem Verkäuferkonto an.
2. Wählen Sie die Registerkarte **VM-Images** .
3. Die am oberen Seitenrand aufgeführte ID ist nicht die SKU-ID, sondern die Angebots-ID.
4. Tragen Sie im Abschnitt **SKUs** die Eigenschaften ein.
5. Klicken Sie unter **Betriebssystemfamilie**auf den zur Betriebssystem-VHD gehörigen Betriebssystemtyp.
6. Geben Sie im Feld **Betriebssystem** eine Beschreibung des Betriebssystems ein. Wählen Sie ein Format nach dem Schema Betriebssystemfamilie, Typ, Version und Updates. Beispiel: „Windows Server Datacenter 2014 R2“.
7. Wählen Sie bis zu sechs empfohlene VM-Größen aus. Dies sind Empfehlungen, die für Kunden auf dem Blatt „Tarif“ im Azure-Portal sichtbar sind, wenn sie Ihr Image erwerben und bereitstellen möchten. **Es handelt sich hierbei nur um Empfehlungen. Der Kunde kann eine beliebige VM-Größe auswählen, die sich für die in Ihrem Image festgelegten Datenträger eignet.**
8. Geben Sie die Version ein. Das Versionsfeld kapselt eine semantische Version, um das Produkt und seine Updates zu identifizieren:
   * Versionen sollten im Format „X.Y.Z“ angegeben werden. „X“, „Y“ und „Z“ sind dabei ganze Zahlen.
   * Images in verschiedenen SKUs können verschiedene Haupt- und Nebenversionen enthalten.
   * Bei Versionen innerhalb einer SKU sollte es sich nur um inkrementelle Änderungen handeln, die die Patchversion erhöhen („Z“ aus „X.Y.Z“).
9. Geben Sie im Feld **URL der Betriebssystem-VHD** den Shared Access Signature-URI ein, der für die Betriebssystem-VHD erstellt wurde.
10. Wenn dieser SKU Datenträger zugeordnet sind, wählen Sie die logische Gerätenummer (Logical Unit Number, LUN) aus, mit der dieser Datenträger bei der Bereitstellung eingebunden werden soll.
11. Geben Sie im Feld **URL der LUN X-VHD** den Shared Access Signature-URI ein, der für die erste Daten-VHD erstellt wurde.
    
    ![Abbildung](media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-3.png)

## <a name="next-step"></a>Nächster Schritt
Wenn Sie die SKU-Details abgeschlossen haben, können Sie mit der [Anleitung für Marketinginhalte in Azure Marketplace][link-pushstaging] fortfahren. In diesem Schritt des Veröffentlichungsprozesses geben Sie Marketinginhalte, Preise und andere Informationen an, die vor dem Fortfahren mit **Schritt 3: Testen Ihres VM-Angebots im Stagingmodus** benötigt werden. Bei diesem Schritt testen Sie verschiedene Anwendungsfallszenarien, bevor Sie das Angebot zum allgemeinen Anzeigen und Kaufen im Azure Marketplace bereitstellen.  

## <a name="see-also"></a>Siehe auch
* [Erste Schritte: Veröffentlichen eines Angebots im Azure Marketplace](marketplace-publishing-getting-started.md)

[img-acom-1]:media/marketplace-publishing-vm-image-creation/vm-image-acom-datacenter.png
[img-portal-vm-size]:media/marketplace-publishing-vm-image-creation/vm-image-portal-size.png
[img-portal-vm-create]:media/marketplace-publishing-vm-image-creation/vm-image-portal-create-vm.png
[img-portal-vm-location]:media/marketplace-publishing-vm-image-creation/vm-image-portal-location.png
[img-portal-vm-rdp]:media/marketplace-publishing-vm-image-creation/vm-image-portal-rdp.png
[img-azstg-add]:media/marketplace-publishing-vm-image-creation/vm-image-storage-add.png
[img-azstg-setup-1]:media/marketplace-publishing-vm-image-creation/vm-image-storage-setup.png
[img-azstg-setup-2]:media/marketplace-publishing-vm-image-creation/vm-image-storage-setup-2.png
[img-azstg-setup-3]:media/marketplace-publishing-vm-image-creation/vm-image-storage-setup-3.png
[img-azstg-setup-4]:media/marketplace-publishing-vm-image-creation/vm-image-storage-setup-4.png
[img-azstg-setup-5]:media/marketplace-publishing-vm-image-creation/vm-image-storage-setup-5.png
[img-azstg-setup-6]:media/marketplace-publishing-vm-image-creation/vm-image-storage-setup-6.png
[img-manage-vm-new]:media/marketplace-publishing-vm-image-creation/vm-image-manage-new.png
[img-manage-vm-select]:media/marketplace-publishing-vm-image-creation/vm-image-manage-select.png
[img-cert-vm-key-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-keyfile-linux.png
[img-cert-vm-pswd-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-linux.png
[img-cert-vm-pswd-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-win.png
[img-cert-vm-test-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-linux.png
[img-cert-vm-test-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-win.png
[img-cert-vm-results]:media/marketplace-publishing-vm-image-creation/vm-image-certification-results.png
[img-cert-vm-questionnaire]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire.png
[img-cert-vm-questionnaire-2]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire-2.png
[img-pubportal-vm-skus]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus.png
[img-pubportal-vm-skus-2]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-2.png

[link-pushstaging]:marketplace-publishing-push-to-staging.md
[link-github-waagent]:https://github.com/Azure/WALinuxAgent
[link-azure-codeplex]:https://azurestorageexplorer.codeplex.com/
[link-azure-2]: ../storage/storage-dotnet-shared-access-signature-part-2.md
[link-azure-1]: ../storage/storage-dotnet-shared-access-signature-part-1.md
[link-msft-download]:http://www.microsoft.com/download/details.aspx?id=44299
[link-technet-3]:https://technet.microsoft.com/library/hh846766.aspx
[link-technet-2]:https://msdn.microsoft.com/library/dn495261.aspx
[link-azure-portal]:https://portal.azure.com
[link-pubportal]:https://publish.windowsazure.com
[link-sql-2014-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014enterprisewindowsserver2012r2/
[link-sql-2014-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014standardwindowsserver2012r2/
[link-sql-2014-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014webwindowsserver2012r2/
[link-sql-2012-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2enterprisewindowsserver2012/
[link-sql-2012-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2standardwindowsserver2012/
[link-sql-2012-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2webwindowsserver2012/
[link-datactr-2012-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012r2datacenter/
[link-datactr-2012]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012datacenter/
[link-datactr-2008-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2008r2sp1/
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-technet-1]:https://technet.microsoft.com/library/hh848454.aspx
[link-azure-vm-2]:./virtual-machines-linux-agent-user-guide/
[link-openssl]:https://www.openssl.org/
[link-intsvc]:http://www.microsoft.com/download/details.aspx?id=41554
[link-python]:https://www.python.org/



<!--HONumber=Dec16_HO1-->


