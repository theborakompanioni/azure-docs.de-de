---
title: Beheben von Problemen mit Azure File Storage unter Linux | Microsoft-Dokumentation
description: Beheben von Problemen mit Azure File Storage unter Linux
services: storage
documentationcenter: 
author: genlin
manager: willchen
editor: na
tags: storage
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: genli
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 5c22c2d8c00882c45ecc2991916e389b2a00586d
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017


---
# <a name="troubleshoot-azure-file-storage-problems-in-linux"></a>Beheben von Problemen mit Azure File Storage unter Linux

Dieser Artikel beschreibt allgemeine Probleme im Zusammenhang mit Microsoft Azure File Storage, wenn Sie eine Verbindung von Linux-Clients herstellen. Darüber hinaus werden die möglichen Ursachen und Lösungen für diese Probleme bereitgestellt.

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-to-open-a-file"></a>„[Zugriff verweigert] Datenträgerkontingent überschritten“, wenn Sie versuchen, eine Datei zu öffnen

Unter Linux erhalten Sie eine Fehlermeldung, die wie folgt aussieht:

**<filename>[Zugriff verweigert] Datenträgerkontingent überschritten**

### <a name="cause"></a>Ursache

Sie haben die obere Grenze der gleichzeitig geöffneten Handles erreicht, die für eine Datei zulässig sind.

### <a name="solution"></a>Lösung

Reduzieren Sie die Anzahl der gleichzeitig geöffneten Handles, indem Sie einige Handles schließen und es anschließend erneut versuchen. Weitere Informationen finden Sie unter [Checkliste zu Leistung und Skalierbarkeit von Microsoft Azure Storage](storage-performance-checklist.md).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-file-storage-in-linux"></a>Unter Linux langsame Datei in und aus Azure File Storage kopieren

-   Wenn Sie keine bestimmte Anforderung für die Mindest-E/A-Größe haben, empfehlen wir Ihnen für eine optimale Leistung die Verwendung von 1 MB als E/A-Größe.
-   Wenn Sie die endgültige Größe einer Datei kennen, die Sie durch die Verwendung von Schreibvorgängen erweitern, und Ihre Software keine Kompatibilitätsprobleme aufweist, wenn das ungeschriebene Fragment in der Datei Nullen enthält, legen Sie die Dateigröße im Voraus fest, anstatt aus jedem Schreibvorgang einen Erweiterungsschreibvorgang zu machen.
-   Verwenden Sie die richtige Kopiermethode:
    -   Verwenden Sie [AzCopy](storage-use-azcopy.md#file-copy) für Übertragungen zwischen zwei Dateifreigaben.
    -   Verwenden Sie [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) zwischen Dateifreigaben auf einem lokalen Computer.

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a>„Bereitstellungsfehler (112): Host ist ausgefallen“, aufgrund eines Timeouts bei der Herstellung einer erneuten Verbindung

Ein „112“-Bereitstellungsfehler tritt auf dem Linux-Client auf, wenn sich der Client für eine längere Zeit im Leerlauf befand. Nach längerer Zeit im Leerlauf trennt der Client die Verbindung, und die Verbindung wird abgebrochen.  

### <a name="cause"></a>Ursache

Die Verbindung kann aus den folgenden Gründen im Leerlauf sein:

-   Ein Grund dafür sind Netzwerkkommunikationsfehler, die das erneute Wiederherstellen einer TCP-Verbindung mit dem Server verhindern, wenn die Standardoption zur zeitweiligen Einbindung verwendet wird
-   Aktuelle Verbindungswiederherstellungs-Updates, die in älteren Kernel nicht vorhanden sind

### <a name="solution"></a>Lösung

Dieses Verbindungswiederherstellungs-Problem im Linux-Kernel wurde jetzt im Rahmen der folgenden Änderungen behoben:

- [Fix reconnect to not defer smb3 session reconnect long after socket reconnect (Stellen Sie die Verbindung wieder her, um die Verbindungswiederherstellung der smb3-Sitzung lange nach der Socket-Verbindungswiederherstellung rückzustellen)](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
-   [Call echo service immediately after socket reconnect (Sofortiges Aufrufen des Echo-Diensts nach der Socket-Verbindungswiederherstellung)](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
-   [CIFS: Fix a possible memory corruption during reconnect (CIFS: Beheben einer Arbeitsspeicherbeschädigung während der Verbindungswiederherstellung)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
-   [CIFS: Fix a possible double locking of mutex during reconnect - for kernel v4.9 and later (CIFS: Beheben einer möglichen doppelten Sperre des Mutex während der Verbindungswiederherstellung – für Kernel v4.9 und spätere Versionen)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

Allerdings können diese Änderungen noch nicht zu allen Linux-Distributionen portiert werden. Diese Behebung und andere Verbindungswiederherstellungs-Behebungen erfolgen in folgenden gängigen Linux-Kernel: 4.4.40, 4.8.16 und 4.9.1. Sie können diese Behebung abrufen, indem Sie ein Upgrade auf eine dieser empfohlenen Kernelversionen vornehmen.

### <a name="workaround"></a>Problemumgehung

Sie können dieses Problem umgehen, indem Sie eine ständige Bereitstellung festlegen. Dies erzwingt, dass der Client wartet bis eine Verbindung hergestellt oder explizit unterbrochen wurde und verwendet werden kann, um Fehler aufgrund von Netzwerktimeouts zu verhindern. Diese Problemumgehung kann jedoch endlose Wartevorgänge verursachen. Seien Sie darauf vorbereitet, Verbindungen nach Bedarf zu beenden.

Wenn Sie kein Upgrade auf die neuesten Kernelversionen vornehmen können, können Sie dieses Problem umgehen, indem Sie eine Datei in der Azure-Dateifreigabe speichern, in die Sie alle 30 Sekunden oder früher schreiben. Dabei muss es sich um einen Schreibvorgang handeln, wie z.B. die Umschreibung des Erstellungs- oder Änderungsdatums in der Datei. Andernfalls erhalten Sie möglicherweise zwischengespeicherte Ergebnisse, und Ihr Vorgang kann die Verbindungswiederherstellung möglicherweise nicht auslösen.

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-file-storage-by-using-smb-30"></a>„Bereitstellungsfehler (115): Vorgang wird ausgeführt“, beim Bereitstellen von Azure File Storage mit SMB 3.0

### <a name="cause"></a>Ursache

Linux-Distributionen unterstützen die Verschlüsselungsfunktionen in SMB 3.0 noch nicht. In manchen Distributionen können Benutzer eine „115“-Fehlermeldung erhalten, wenn sie aufgrund eines fehlenden Features versuchen, Azure File Storage mithilfe von SMB 3.0 einzubinden.

### <a name="solution"></a>Lösung

Wenn der Linux-SMB-Client die Verschlüsselung nicht unterstützt, binden Sie Azure File Storage mithilfe von SMB 2.1 von einer Azure Linux-VM aus ein, die sich im gleichen Rechenzentrum wie das File Storage-Konto befindet.

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a>Langsame Leistung in einer Azure-Dateifreigabe, die in einer Linux-VM bereit gestellt ist

### <a name="cause"></a>Ursache

Eine mögliche Ursache der langsamen Leistung ist der deaktivierte Cache.

### <a name="solution"></a>Lösung

Um zu überprüfen, ob der Cache deaktiviert ist, suchen Sie nach dem **cache=**-Eintrag. 

**Cache=none** gibt an, dass das Caching deaktiviert ist.  Stellen Sie die Freigabe erneut bereit, entweder durch die Verwendung des Standardbereitstellungsbefehls oder explizit durch das Hinzufügen der Option **cache=strict** zum Bereitstellungsbefehl, um sicherzustellen, dass das Standardcaching oder der Cachingmodus „strict“ aktiviert ist.

In einigen Szenarios kann die **serverino**-Bereitstellungsoption dazu führen, dass der **ls**-Befehl unverzüglich gegen jeden Verzeichniseintrag läuft. Dieses Verhalten führt zu einer Leistungsminderung, wenn Sie ein großes Verzeichnis auflisten. Sie können die Bereitstellungsoption in Ihrem **/etc/fstab**-Eintrag finden:

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=3.0,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

Sie können auch überprüfen, ob die richtigen Optionen verwendet werden, indem Sie den **sudo mount | grep cifs**-Befehl ausführen und dessen Output überprüfen, wie z.B. der folgende Beispieloutput:

`//mabiccacifs.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=3.0,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

Wenn die Optionen **cache=strict** oder **serverino** nicht vorhanden sind, heben Sie die Bereitstellung von Azure File Storage auf, und stellen Sie sie wieder her, indem Sie den „mount“-Befehl aus der [Dokumentation](storage-how-to-use-files-linux.md#mount-the-file-share) ausführen. Überprüfen Sie dann erneut, ob der **/etc/fstab**-Eintrag die richtigen Optionen hat.

<a id="error11"></a>
## <a name="mount-error11-resource-temporarily-unavailable-when-youre-mounting-to-an-ubuntu-48-kernel"></a>„Bereitstellungsfehler (11): Ressource vorübergehend nicht verfügbar“, beim Bereitstellen von Ubuntu-Kerneln ab Version 4.8

### <a name="cause"></a>Ursache

Im Ubuntu 16.10-Kernel (Version 4.8) wird der Client dokumentiert, um die Verschlüsselung zu unterstützen, tut dies aber eigentlich nicht.

### <a name="solution"></a>Lösung

Geben Sie bis zur Behebung des Problems in Ubuntu 16.10 die `vers=2.1`-Bereitstellungsoption an, oder verwenden Sie Ubuntu 16.04.

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-to-linux"></a>Zeitstempel gingen beim Kopieren von Dateien von Windows auf Linux verloren

Auf Linux/Unix-Plattformen schlägt der **cp -p**-Befehl fehl, wenn Datei 1 und Datei 2 verschiedenen Benutzern gehören.

### <a name="cause"></a>Ursache

Das Force-Flag **f** in COPYFILE führt zu der Ausführung von **cp -p -f** unter Unix. Dieser Befehl schlägt auch fehl, um den Zeitstempel der Datei beizubehalten, die Ihnen nicht gehört.

### <a name="workaround"></a>Problemumgehung

Verwenden Sie das Speicherbenutzerkonto, um die Dateien zu kopieren:

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

