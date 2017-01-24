---
title: Beheben von Problemen mit Azure File Storage | Microsoft Docs
description: Beheben von Problemen mit Azure File Storage
services: storage
documentationcenter: 
author: genlin
manager: felixwu
editor: na
tags: storage
ms.assetid: fbc5f600-131e-4b99-828a-42d0de85fff7
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2016
ms.author: genli
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 71da2f8aaa994c8cfc48f968a5275f7f79604251


---
# <a name="troubleshooting-azure-file-storage-problems"></a>Beheben von Problemen mit Azure File Storage
Dieser Artikel beschreibt allgemeine Probleme im Zusammenhang mit Microsoft Azure File Storage, wenn Sie eine Verbindung von Windows- und Linux-Clients herstellen. Darüber hinaus werden die möglichen Ursachen und Lösungen für diese Probleme bereitgestellt.

**Allgemeine Probleme (treten sowohl auf Windows- als auch auf Linux-Clients auf)**

* [Kontingentfehler beim Öffnen einer Datei](#quotaerror)
* [Geringe Leistung beim Zugriff auf Azure File Storage über Windows oder Linux](#slowboth)

**Windows-Clientprobleme**

* [Geringe Leistung beim Zugriff auf Azure File Storage über Windows 8.1 oder Windows Server 2012 R2](#windowsslow)
* [Fehler 53 beim Versuch, eine Azure-Dateifreigabe einzubinden](#error53)
* [„net use“ war erfolgreich, aber die Azure Dateifreigabe wird nicht als im Windows-Explorer eingebunden angezeigt](#netuse)
* [Mein Speicherkonto enthält „/“, und beim Ausführen des Befehls „net use“ tritt ein Fehler auf](#slashfails)
* [Meine Anwendung bzw. mein Dienst kann nicht auf das eingebundene Azure Files-Laufwerk zugreifen.](#accessfiledrive)
* [Zusätzliche Empfehlungen zur Leistungsoptimierung](#additional)

**Linux-Clientprobleme**

* [Fehler „You are copying a file to a destination that does not support encryption“ (Sie kopieren eine Datei in ein Ziel, das die Verschlüsselung nicht unterstützt) beim Hochladen/Kopieren von Dateien in Azure Files](#encryption)
* [Fehler „Host is down“ (Host nicht verfügbar) bei vorhandenen Dateifreigaben, oder die Shell hängt beim Ausführen von Listenbefehlen auf dem Einbindungspunkt](#errorhold)
* [Einbindungsfehler 115 beim Versuch, Azure Files auf der Linux-VM einzubinden](#error15)
* [Bei der Linux-VM treten zufällige Verzögerungen in Befehlen wie „Is“ auf](#delayproblem)

<a id="quotaerror"></a>

## <a name="quota-error-when-trying-to-open-a-file"></a>Kontingentfehler beim Öffnen einer Datei
Unter Windows erhalten Sie Fehlermeldungen, die wie folgt aussehen:

**1816 ERROR_NOT_ENOUGH_QUOTA <--> 0xc0000044**

**STATUS_QUOTA_EXCEEDED**

**Das Kontingent reicht für die Verarbeitung dieses Befehls nicht aus**

**Ungültiger Handlewert GetLastError: 53**

Unter Linux erhalten Sie Fehlermeldungen, die wie folgt aussehen:

**<filename> [Zugriff verweigert]**

**Datenträgerkontingent überschritten**

### <a name="cause"></a>Ursache
Das Problem tritt auf, da Sie die obere Grenze der gleichzeitig geöffneten Handles erreicht haben, die für eine Datei zulässig sind.

### <a name="solution"></a>Lösung
Reduzieren Sie die Anzahl der gleichzeitig geöffneten Handles, indem Sie einige Handles schließen und es anschließend erneut versuchen. Weitere Informationen finden Sie unter [Checkliste zu Leistung und Skalierbarkeit von Microsoft Azure Storage](storage-performance-checklist.md).

<a id="slowboth"></a>

## <a name="slow-performance-when-accessing-file-storage-from-windows-or-linux"></a>Geringe Leistung beim Zugriff auf Azure File Storage über Windows oder Linux
* Wenn Sie keine bestimmte Anforderung für die Mindest-E/A-Größe haben, empfehlen wir Ihnen für eine optimale Leistung die Verwendung von 1 MB als E/A-Größe.
* Wenn Sie die endgültige Größe einer Datei kennen, die Sie mit Schreibvorgängen erweitern, und Ihre Software keine Kompatibilitätsprobleme aufweist, wenn das noch nicht geschriebene Fragment in der Datei Nullen enthält, legen Sie die Dateigröße im Voraus fest, anstatt dass jeder Schreibvorgang einen Erweiterungsschreibvorgang darstellt.

<a id="windowsslow"></a>

## <a name="slow-performance-when-accessing-the-file-storage-from-windows-81-or-windows-server-2012-r2"></a>Geringe Leistung beim Zugriff auf Azure File Storage über Windows 8.1 oder Windows Server 2012 R2
Stellen Sie für Clients, auf denen Windows 8.1 oder Windows Server 2012 R2 ausgeführt wird, sicher, dass der Hotfix [KB3114025](https://support.microsoft.com/kb/3114025) installiert ist. Dieser Hotfix verbessert die Leistung beim Erstellen und Schließen eines Handle.

Sie können das folgende Skript ausführen, um zu überprüfen, ob der Hotfix installiert wurde:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Wenn der Hotfix installiert wurde, wird die folgende Ausgabe angezeigt:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies**

**{96c345ef-3cac-477b-8fcd-bea1a564241c}    REG_DWORD    0x1**

> [!NOTE]
> Bei Windows Server 2012 R2-Images in Azure Marketplace ist der Hotfix KB3114025 ab Dezember 2015 standardmäßig installiert.
>
>

<a id="additional"></a>

## <a name="additional-recommendations-to-optimize-performance"></a>Zusätzliche Empfehlungen zur Leistungsoptimierung
Erstellen oder öffnen Sie niemals eine Datei für zwischengespeichertes E/A, die Schreibzugriff, jedoch keinen Lesezugriff anfordert. D.h., geben Sie beim Aufruf von **CreateFile()** niemals nur **GENERIC_WRITE** an, sondern immer **GENERIC_READ | GENERIC_WRITE**. Ein lesegeschütztes Handle kann kleine Schreibvorgänge nicht lokal zwischenspeichern, selbst wenn es das einzige geöffnete Handle der Datei ist. Dies führt zu schwerwiegenden Leistungseinbußen für kleine Schreibvorgänge. Beachten Sie, dass der „a“-Modus auf CRT **fopen()** ein lesegeschütztes Handle öffnet.

<a id="error53"></a>

## <a name="error-53-or-error-67-when-you-try-to-mount-or-unmount-an-azure-file-share"></a>„Fehler 53“ oder „Fehler 67“ beim Versuch, eine Azure-Dateifreigabe einzubinden oder die Einbindung aufzuheben
Dieses Problem kann folgende Ursachen haben:

### <a name="cause-1"></a>Ursache 1
„Systemfehler 53 ist aufgetreten. Zugriff verweigert.“ Aus Sicherheitsgründen werden Verbindungen mit Azure File-Freigaben blockiert, wenn der Kommunikationskanal nicht verschlüsselt ist und der Verbindungsversuch nicht von dem gleichen Rechenzentrum aus erfolgt, in dem sich die Azure File-Freigaben befinden. Die Verschlüsselung des Kommunikationskanals ist nicht verfügbar, wenn das Clientbetriebssystem des Benutzers die SMB-Verschlüsselung nicht unterstützt. Dies wird durch die Fehlermeldung „Systemfehler 53 ist aufgetreten. Zugriff verweigert“ angezeigt, wenn ein Benutzer versucht, eine Dateifreigabe von einer lokalen Umgebung oder einem anderen Rechenzentrum aus einzubinden. Windows 8, Windows Server 2012 und höhere Versionen jeder Negotiate-Anforderung, die SMB 3.0 umfassen, wodurch die Verschlüsselung unterstützt wird.

### <a name="solution-for-cause-1"></a>Lösung für Ursache 1
Stellen Sie eine Verbindung von einem Client aus her, der die Anforderungen von Windows 8, Windows Server 2012 und höheren Versionen erfüllt, oder stellen Sie eine Verbindung von einem virtuellen Computer aus her, der sich im gleichen Rechenzentrum befindet wie das Azure Storage-Konto, das für die Azure-Dateifreigabe verwendet wird.

### <a name="cause-2"></a>Ursache 2
„Systemfehler 53“ oder „Systemfehler 67“ kann beim Einbinden einer Azure-Dateifreigabe auftreten, wenn die von Port 445 ausgehende Kommunikation zum Azure Files-Rechenzentrum blockiert ist. Klicken Sie [hier](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx), um eine Zusammenfassung der ISPs anzuzeigen, die den Zugang von Port 445 aus zulassen oder verweigern.

Comcast und einige IT-Organisationen blockieren diesen Port. Um zu verstehen, ob dies der Grund für die Meldung „Systemfehler 53“ ist, können Sie Portqry verwenden, um den Endpunkt TCP:445 abzufragen. Wenn der Endpunkt TCP:445 als gefiltert angezeigt wird, wird der TCP-Port blockiert. Dies ist eine Beispielabfrage:

`g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Wenn der TCP 445 durch eine Regel entlang des Netzwerkpfads blockiert wird, wird Ihnen die folgende Ausgabe angezeigt:

**TCP port 445 (microsoft-ds service): FILTERED**

Weitere Informationen zur Verwendung von Portqry finden Sie unter [Beschreibung des Befehlszeilenprogramms Portqry.exe](https://support.microsoft.com/kb/310099).

### <a name="solution-for-cause-2"></a>Lösung für Ursache 2
Arbeiten Sie mit Ihrer IT-Organisation zusammen, um den Port 445-Ausgang zu [Azure-IP-Bereichen](https://www.microsoft.com/download/details.aspx?id=41653) zu öffnen.

### <a name="cause-3"></a>Ursache 3
„Systemfehler 53“ kann auch auftreten, wenn die NTLMv1-Kommunikation auf dem Client aktiviert ist. Wenn NTLMv1 aktiviert ist, ist der Client weniger sicher. Aus diesem Grund wird die Kommunikation für Azure-Dateien blockiert. Um zu überprüfen, ob dies die Ursache des Fehlers ist, überprüfen Sie, ob der folgende Registrierungsunterschlüssel auf den Wert 3 festgelegt ist:

HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel.

Weitere Informationen finden Sie im Thema [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) im TechNet.

### <a name="solution-for-cause-3"></a>Lösung für Ursache 3
Um dieses Problem zu beheben, setzen Sie den LmCompatibilityLevel-Wert im Registrierungsschlüssel „HKLM\SYSTEM\CurrentControlSet\Control\Lsa“ auf den Standardwert „3“ zurück.

Azure Files unterstützt nur die NTLMv2-Authentifizierung. Stellen Sie sicher, dass die Gruppenrichtlinie auf die Clients angewendet wird. Dadurch wird verhindert, dass dieser Fehler auftritt. Dies gilt zudem als bewährte Sicherheitsmethode. Weitere Informationen finden Sie unter [Netzwerksicherheit: LAN Manager-Authentifizierungsebene](https://technet.microsoft.com/library/jj852207\(v=ws.11\).aspx).

Die empfohlene Richtlinieneinstellung ist **Nur NTLMv2-Antworten senden**. Dies entspricht einem Registrierungswert von 3. Clients verwenden nur die NTLMv2-Authentifizierung, und sie verwenden die NTLMv2-Sitzungssicherheit, wenn der Server diese unterstützt. Domänencontroller akzeptieren die NTLMv2-, LM- und NTLM-Authentifizierung.

<a id="netuse"></a>

## <a name="net-use-was-successful-but-dont-see-the-azure-file-share-mounted-in-windows-explorer"></a>„net use“ war erfolgreich, jedoch wird nicht angezeigt, dass die Azure-Dateifreigabe in den Windows-Explorer eingebunden wurde
### <a name="cause"></a>Ursache
Standardmäßig wird der Windows-Explorer nicht als Administrator ausgeführt. Wenn Sie **net use** von einer Administratoreingabeaufforderung aus ausführen, ordnen Sie das Netzlaufwerk „Als Administrator“ zu. Da zugeordnete Laufwerke benutzerorientiert sind, zeigt das angemeldete Benutzerkonto die Laufwerke nicht an, wenn sie unter einem anderen Benutzerkonto eingebunden werden.

### <a name="solution"></a>Lösung
Binden Sie die Freigabe von einer Befehlszeile für Benutzer ohne Administratorrechte aus ein. Alternativ können Sie [diesem TechNet-Thema folgen](https://technet.microsoft.com/library/ee844140.aspx), um den Registrierungswert **EnableLinkedConnections** zu konfigurieren.

<a id="slashfails"></a>

## <a name="my-storage-account-contains--and-the-net-use-command-fails"></a>Mein Speicherkonto enthält „/“, und beim Ausführen des Befehls „net use“ tritt ein Fehler auf
### <a name="cause"></a>Ursache
Wenn der Befehl **net use** unter einer Eingabeaufforderung ausgeführt wird (cmd.exe), wird er durch das Hinzufügen von „/“ als Befehlszeilenoption analysiert. Dadurch tritt ein Fehler bei der Zuordnung des Laufwerks auf.

### <a name="solution"></a>Lösung
Sie können eine der folgenden Methoden verwenden, um dieses Problem zu umgehen:

•    Verwenden Sie den folgenden PowerShell-Befehl:

`New-SmbMapping -LocalPath y: -RemotePath \\server\share  -UserName acountName -Password "password can contain / and \ etc"`

Von einer Batchdatei aus kann dies folgendermaßen aussehen:

`Echo new-smbMapping ... | powershell -command –`

•    Setzen Sie den Schlüssel in doppelte Anführungszeichen, um dieses Problem zu umgehen, sofern das erste Zeichen nicht „/“ ist. Wenn dies der Fall ist, verwenden Sie den interaktiven Modus, und geben Sie Ihr Kennwort separat ein, oder generieren Sie Ihre Schlüssel neu, um einen Schlüssel zu erhalten, der nicht mit einem Schrägstrich (/) beginnt.

<a id="accessfiledrive"></a>

## <a name="my-applicationservice-cannot-access-mounted-azure-files-drive"></a>Meine Anwendung bzw. mein Dienst kann nicht auf das eingebundene Azure Files-Laufwerk zugreifen
### <a name="cause"></a>Ursache
Laufwerke werden pro Benutzer eingebunden. Wenn Ihre Anwendung oder Ihr Dienst unter einem anderen Benutzerkonto ausgeführt wird, wird Benutzern das Laufwerk nicht angezeigt.

### <a name="solution"></a>Lösung
Binden Sie das Laufwerk von dem gleichen Benutzerkonto aus ein, unter dem sich die Anwendung befindet. Dies kann durch die Verwendung von Tools wie z.B. psexec erfolgen.

Alternativ können Sie einen neuen Benutzer erstellen, der über die gleichen Berechtigungen wie das Netzwerkdienst- oder das Systemkonto verfügt, und führen Sie dann **cmdkey** und **net use** unter diesem Konto aus. Der Benutzername muss der Name des Speicherkontos sein, und das Kennwort muss der Speicherkontoschlüssel sein. Eine weitere Möglichkeit für **net use** ist, den Speicherkontonamen und -schlüssel an die Parameter des **net use**-Befehls zu übergeben.

Nachdem Sie diese Anleitung befolgt haben, erhalten Sie möglicherweise die folgende Fehlermeldung: „Systemfehler 1312 ist aufgetreten. Eine angegebene Anmeldesitzung ist nicht vorhanden. Sie wurde gegebenenfalls bereits beendet.“, wenn Sie **net use** für das System-/Netzwerkdienstkonto ausführen. Wenn dies auftritt, vergewissern Sie sich, dass der an **net use** übergebene Benutzername Domäneninformationen enthält (z.B. „[Speicherkontoname].file.core.windows.net“).

<a id="encryption"></a>

## <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>Fehler „You are copying a file to a destination that does not support encryption“ (Sie kopieren eine Datei in ein Ziel, das die Verschlüsselung nicht unterstützt)
### <a name="cause"></a>Ursache
Mit BitLocker verschlüsselte Dateien können in Azure Files kopiert werden. File Storage unterstützt NTFS EFS jedoch nicht. Aus diesem Grund werden Sie in diesem Fall wahrscheinlich EFS verwenden. Wenn Sie über Dateien verfügen, die über EFS verschlüsselt sind, kann beim Kopiervorgang in File Storage ein Fehler auftreten, sofern der Kopierbefehl keine kopierte Datei entschlüsselt.

### <a name="workaround"></a>Problemumgehung
Um eine Datei in File Storage zu kopieren, müssen Sie sie zunächst entschlüsseln. Hierfür können Sie eine der folgenden Methoden verwenden:

•    Verwenden Sie **copy /d**.

•    Legen Sie den folgenden Registrierungsschlüssel fest:

* Path=HKLM\Software\Policies\Microsoft\Windows\System
* Value type=DWORD
* Name = CopyFileAllowDecryptedRemoteDestination
* Value = 1

Beachten Sie jedoch, dass sich das Festlegen des Registrierungsschlüssels auf alle Kopiervorgänge von Netzwerkfreigaben auswirkt.

<a id="errorhold"></a>

## <a name="host-is-down-error-on-existing-file-shares-or-the-shell-hangs-when-you-run-list-commands-on-the-mount-point"></a>Fehler „Host is down“ (Host nicht verfügbar) bei vorhandenen Dateifreigaben, oder die Shell hängt beim Ausführen von Listenbefehlen auf dem Einbindungspunkt
### <a name="cause"></a>Ursache
Dieser Fehler tritt auf dem Linux-Client auf, wenn sich der Client für einen längeren Zeitraum im Leerlauf befand. Wenn dieser Fehler auftritt, trennt der Client die Verbindung, und der Timeout für die Clientverbindung wird erreicht.

### <a name="solution"></a>Lösung
Dieses Problem wurde im Linux-Kernel nun als Teil von [change set](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93) behoben, wobei die Zurückportierung in die Linux-Distribution aussteht.

Um dieses Problem zu umgehen, halten Sie die Verbindung aufrecht, und vermeiden Sie, in einen Leerlaufzustand zu geraten, und behalten Sie eine Datei in der Azure-Dateifreigabe, in die Sie regelmäßig schreiben. Dabei muss es sich um einen Schreibvorgang handeln, wie z.B. die Umschreibung des Erstellungs-/Änderungsdatums in der Datei. Andernfalls erhalten Sie möglicherweise zwischengespeicherte Ergebnisse, und Ihr Vorgang kann die Verbindung möglicherweise nicht auslösen.

<a id="error15"></a>

## <a name="mount-error-115-when-you-try-to-mount-azure-files-on-the-linux-vm"></a>„Einbindungsfehler 115“ beim Versuch, Azure Files auf der Linux-VM einzubinden
### <a name="cause"></a>Ursache
Linux-Distributionen unterstützen die Verschlüsselungsfunktion in SMB 3.0 noch nicht. In manchen Distributionen erhalten Benutzer eine „115“-Fehlermeldung, wenn sie aufgrund eines fehlenden Features versuchen, Azure Files mithilfe von SMB 3.0 einzubinden.

### <a name="solution"></a>Lösung
Wenn der verwendete Linux-SMB-Client die Verschlüsselung nicht unterstützt, binden Sie Azure Files mithilfe von SMB 2.1 von einer Linux-VM aus ein, die sich im gleichen Rechenzentrum wie das File Storage-Konto befindet.

<a id="delayproblem"></a>

## <a name="linux-vm-experiencing-random-delays-in-commands-like-ls"></a>Bei der Linux-VM treten zufällige Verzögerungen in Befehlen wie „Is“ auf
### <a name="cause"></a>Ursache
Dies kann auftreten, wenn der „mount“-Befehl die Option **serverino** nicht enthält. Ohne **serverino** führt der „Is“-Befehl auf jeder Datei **stat** aus.

### <a name="solution"></a>Lösung
Überprüfen Sie die Option **serverino** in Ihrem Eintrag „/etc/fstab“:

//azureuser.file.core.windows.net/wms/comer on /home/sampledir type cifs (rw,nodev,relatime,vers=2.1,sec=ntlmssp,cache=strict,username=xxx,domain=X, file_mode=0755,dir_mode=0755,serverino,rsize=65536,wsize=65536,actimeo=1)

Wenn die Option **serverino** nicht vorhanden ist, heben Sie die Einbindung von Azure Files auf, und binden Sie Azure Files erneut ein, wobei die Option **serverino** ausgewählt ist.

## <a name="learn-more"></a>Weitere Informationen
* [Erste Schritte mit Azure File Storage unter Windows](storage-dotnet-how-to-use-files.md)
* [Verwenden von Azure File Storage unter Linux](storage-how-to-use-files-linux.md)



<!--HONumber=Dec16_HO2-->


