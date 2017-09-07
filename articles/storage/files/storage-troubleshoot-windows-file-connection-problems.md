---
title: Behandlung von Problemen in Azure File Storage in Windows | Microsoft-Dokumentation
description: Behandlung von Problemen in Azure File Storage in Windows
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
ms.date: 06/28/2017
ms.author: genli
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: daaf7d0589f5e2d82b43dad879bffd23370a2c81
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="troubleshoot-azure-file-storage-problems-in-windows"></a>Behandlung von Problemen in Azure File Storage in Windows

Dieser Artikel beschreibt allgemeine Probleme im Zusammenhang mit Microsoft Azure File Storage, wenn Sie eine Verbindung von Windows-Clients herstellen. Darüber hinaus werden die möglichen Ursachen und Lösungen für diese Probleme bereitgestellt. Zusätzlich zu den Schritten zur Problembehandlung in diesem Artikel können Sie auch [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) verwenden, um sicherzustellen, dass die Windows-Clientumgebung über richtige erforderliche Komponenten verfügt. AzFileDiagnostics automatisiert die Erkennung eines Großteils der Symptome, die in diesem Artikel erwähnt werden und hilft, Ihre Umgebung einzurichten, um eine optimale Leistung zu erzielen. Sie erhalten diese Informationen auch unter [Problembehandlung für Azure-Dateifreigaben](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares). Hier finden Sie Schritte zum Beheben von Problemen mit dem Verbinden, Zuordnen und Einbinden von Azure-Dateifreigaben.


<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a>„Fehler 53“, „Fehler 67“ oder „Fehler 87“ beim Versuch, eine Azure-Dateifreigabe einzubinden oder die Einbindung aufzuheben

Wenn Sie versuchen, eine Dateifreigabe aus einem lokalen oder einem anderen Datencenter einzubinden, erhalten Sie möglicherweise die folgenden Fehler:

- „Systemfehler 53 ist aufgetreten. Der Netzwerkpfad wurde nicht gefunden.“
- „Systemfehler 67 ist aufgetreten. „Der Netzwerkname wurde nicht gefunden.“
- „Systemfehler 87 ist aufgetreten. „Der Parameter ist falsch.“

### <a name="cause-1-unencrypted-communication-channel"></a>Ursache 1: Unverschlüsselter Kommunikationskanal

Aus Sicherheitsgründen werden Verbindungen mit Azure-Dateifreigaben blockiert, wenn der Kommunikationskanal nicht verschlüsselt ist und der Verbindungsversuch nicht von dem gleichen Rechenzentrum aus erfolgt, in dem sich die Azure-Dateifreigaben befinden. Die Verschlüsselung des Kommunikationskanals ist nur verfügbar, wenn das Clientbetriebssystem des Benutzers die SMB-Verschlüsselung unterstützt.

Windows 8, Windows Server 2012 und höhere Versionen jedes Systems handeln Anforderung aus, die SMB 3.0 umfassen, wodurch die Verschlüsselung unterstützt wird.

### <a name="solution-for-cause-1"></a>Lösung für Ursache 1

Stellen Sie eine Verbindung von einem Client her, der einen der folgenden Punkte erfüllt:

- Erfüllt die Anforderungen von Windows 8 und Windows Server 2012 oder höher
- Stellt eine Verbindung von einem virtuellen Computer im selben Datencenter wie das Azure-Speicherkonto her, das für die Azure-Dateifreigabe verwendet wird

### <a name="cause-2-port-445-is-blocked"></a>Ursache 2: Port 445 ist gesperrt

„Systemfehler 53“ oder „Systemfehler 67“ können auftreten, wenn die von Port 445 ausgehende Kommunikation zu einem Azure File Storage-Datencenter blockiert ist. Um eine Zusammenfassung der ISPs anzuzeigen, die den Zugang von Port 445 aus zulassen oder verweigern, gehen Sie auf [TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).

Um zu verstehen, ob dies der Grund für die Meldung „Systemfehler 53“ ist, können Sie Portqry verwenden, um den Endpunkt TCP:445 abzufragen. Wenn der Endpunkt TCP:445 als gefiltert angezeigt wird, wird der TCP-Port blockiert. Dies ist eine Beispielabfrage:

  `g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Wenn der TCP-Port 445 durch eine Regel entlang des Netzwerkpfads blockiert wird, wird Ihnen die folgende Ausgabe angezeigt:

  `TCP port 445 (microsoft-ds service): FILTERED`

Weitere Informationen zur Verwendung von Portqry finden Sie unter [Beschreibung des Befehlszeilenprogramms Portqry.exe](https://support.microsoft.com/help/310099).

### <a name="solution-for-cause-2"></a>Lösung für Ursache 2

Arbeiten Sie mit Ihrer IT-Abteilung zusammen, um den Port 445-Ausgang zu [Azure-IP-Bereichen](https://www.microsoft.com/download/details.aspx?id=41653) zu öffnen.

### <a name="cause-3-ntlmv1-is-enabled"></a>Ursache 3: NTLMv1 ist aktiviert

„Systemfehler 53“ oder „Systemfehler 87“ können auftreten, wenn die NTLMv1-Kommunikation auf dem Client aktiviert ist. Azure File Storage unterstützt nur die NTLMv2-Authentifizierung. Wenn NTLMv1 aktiviert ist, ist der Client weniger sicher. Aus diesem Grund wird die Kommunikation für Azure File Storage blockiert. 

Um zu bestimmen, ob dies die Ursache des Fehlers ist, überprüfen Sie, ob der folgende Registrierungsunterschlüssel auf den Wert 3 festgelegt ist:

**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**

Weitere Informationen finden Sie im Thema [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) im TechNet.

### <a name="solution-for-cause-3"></a>Lösung für Ursache 3

Setzen Sie den Wert **LmCompatibilityLevel** auf den Standardwert 3 im folgenden Registrierungsunterschlüssel zurück:

  **HKLM\SYSTEM\CurrentControlSet\Control\Lsa**

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-to-process-this-command-when-you-copy-to-an-azure-file-share"></a>Fehler 1816 „Nicht genügend Quoten verfügbar, um diesen Befehl zu verarbeiten“, wenn Sie in eine Azure-Dateifreigabe kopieren

### <a name="cause"></a>Ursache

Der Fehler 1816 tritt auf, wenn Sie die Obergrenze der parallelen offenen Handles erreichen, die für eine Datei auf dem Computer zulässig sind, auf dem die Dateifreigabe bereitgestellt wird.

### <a name="solution"></a>Lösung

Reduzieren Sie die Anzahl der gleichzeitig geöffneten Handles, indem Sie einige Handles schließen und es anschließend erneut versuchen. Weitere Informationen finden Sie unter [Checkliste zur Leistung und Skalierbarkeit von Microsoft Azure Storage](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-file-storage-in-windows"></a>Langsames Kopieren von Dateien in und aus Azure File Storage unter Windows

Die Leistung ist möglicherweise langsam, wenn Sie versuchen, Dateien in den Azure-Dateidienst zu übertragen.

- Wenn Sie keine bestimmte Anforderung für die Mindest-E/A-Größe haben, empfehlen wir Ihnen für eine optimale Leistung die Verwendung von 1 MB als E/A-Größe.
-   Wenn Sie die endgültige Größe einer Datei kennen, die Sie mit Schreibvorgängen erweitern, und Ihre Software keine Kompatibilitätsprobleme aufweist, wenn das noch nicht geschriebene Fragment in der Datei Nullen enthält, legen Sie die Dateigröße im Voraus fest, anstatt dass jeder Schreibvorgang einen Erweiterungsschreibvorgang darstellt.
-   Verwenden Sie die richtige Kopiermethode:
    -   Verwenden Sie [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) für Übertragungen zwischen zwei Dateifreigaben.
    -   Verwenden Sie [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) zwischen Dateifreigaben auf einem lokalen Computer.

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a>Überlegungen zu Windows 8.1 oder Windows Server 2012 R2

Stellen Sie für Clients, die Windows 8.1 oder Windows Server 2012 R2 ausführen, sicher, dass der Hotfix [KB3114025](https://support.microsoft.com/help/3114025) installiert ist. Dieser Hotfix verbessert die Leistung beim Erstellen und Schließen eines Handle.

Sie können das folgende Skript ausführen, um zu überprüfen, ob der Hotfix installiert wurde:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Wenn der Hotfix installiert wurde, wird die folgende Ausgabe angezeigt:

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> Bei Windows Server 2012 R2-Images in Azure Marketplace ist der Hotfix KB3114025 ab Dezember 2015 standardmäßig installiert.

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer"></a>Kein Ordner mit einem Laufwerkbuchstaben im **Arbeitsplatz**

Wenn Sie eine Azure-Dateifreigabe als Administrator mithilfe des Befehls „net use“ zuordnen, scheint es so, als würde die Freigabe fehlen.

### <a name="cause"></a>Ursache

Standardmäßig wird der Windows-Datei-Explorer nicht als Administrator ausgeführt. Wenn Sie „net use“ von einer administrativen Eingabeaufforderung aus ausführen, ordnen Sie das Netzlaufwerk als Administrator zu. Da zugeordnete Laufwerke benutzerorientiert sind, zeigt das angemeldete Benutzerkonto die Laufwerke nicht an, wenn sie unter einem anderen Benutzerkonto eingebunden werden.

### <a name="solution"></a>Lösung
Binden Sie die Freigabe von einer Befehlszeile für Benutzer ohne Administratorrechte aus ein. Alternativ können Sie [diesem TechNet-Thema folgen](https://technet.microsoft.com/library/ee844140.aspx), um den Registrierungswert **EnableLinkedConnections** zu konfigurieren.

<a id="netuse"></a>
## <a name="net-use-command-fails-if-the-storage-account-contains-a-forward-slash"></a>Der Befehl „net use“ schlägt fehl, wenn das Speicherkonto einen Schrägstrich enthält

### <a name="cause"></a>Ursache

Der Befehl „net use“ interpretiert einen Schrägstrich (/) als Befehlszeilenoption. Wenn der Name Ihres Benutzerkontos mit einem Schrägstrich beginnt, schlägt die Zuordnung des Laufwerks fehl.

### <a name="solution"></a>Lösung

Sie können eine der folgenden Methoden verwenden, um dieses Problem zu umgehen:

- Führen Sie den folgenden PowerShell-Befehl aus:

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc" `

  Von einer Batchdatei aus können Sie wie unten angegeben den Befehl ausführen:

  `Echo new-smbMapping ... | powershell -command –`

- Setzen Sie den Schlüssel in doppelte Anführungszeichen, um dieses Problem zu umgehen, es sei denn, der Schrägstich ist das erste Zeichen. Wenn dies der Fall ist, verwenden Sie den interaktiven Modus, und geben Sie Ihr Kennwort separat ein, oder generieren Sie Ihre Schlüssel neu, um einen Schlüssel zu erhalten, der nicht mit einem Schrägstrich beginnt.

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-file-storage-drive"></a>Die Anwendung oder der Dienst können auf keinen bereitgestellten Azure File Storage-Laufwerk zugreifen.

### <a name="cause"></a>Ursache

Laufwerke werden pro Benutzer eingebunden. Wenn Ihre Anwendung oder der Dienst unter einem anderen Benutzerkonto als dem ausgeführt wird, welches das Laufwerk bereitgestellt hat, wird für die Anwendung das Laufwerk nicht angezeigt.

### <a name="solution"></a>Lösung

Verwenden Sie eine der folgenden Lösungen:

-   Binden Sie das Laufwerk vom selben Benutzerkonto ein, das auch die Anwendung enthält. Sie können ein Tool wie z.B. PsExec verwenden.
- Übergeben Sie den Speicherkontonamen und -schlüssel an die Parameter des „net use“-Befehls.

Nachdem Sie die erforderlichen Schritte der Anleitung durchgeführt haben, erhalten Sie womöglich die folgende Fehlermeldung, wenn Sie „net use“ für das System- bzw. Netzwerkdienstkonto ausführen: „Systemfehler 1312 ist aufgetreten. Eine angegebene Anmeldesitzung ist nicht vorhanden. Sie wird möglicherweise bereits verwendet.“ Wenn dies auftritt, vergewissern Sie sich, dass der an „net use“ übergebene Benutzername Domäneninformationen enthält (z.B. „[Speicherkontoname].file.core.windows.net“).

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>Fehler „You are copying a file to a destination that does not support encryption“ (Sie kopieren eine Datei in ein Ziel, das die Verschlüsselung nicht unterstützt)

Wenn eine Datei über das Netzwerk kopiert wird, wird die Datei auf dem Quellcomputer entschlüsselt, in Klartext übermittelt und am Ziel erneut verschlüsselt. Jedoch erscheint möglicherweise der folgende Fehler, wenn Sie versuchen, eine verschlüsselte Datei zu kopieren: „Die Datei wird an ein Ziel kopiert, das keine Verschlüsselung unterstützt.“

### <a name="cause"></a>Ursache
Dieses Problem kann auftreten, wenn Sie ein verschlüsselndes Dateisystem (EFS, Encrypting File System) verwenden. Mit BitLocker verschlüsselte Dateien können in Azure File Storage kopiert werden. Azure File Storage unterstützt NTFS EFS jedoch nicht.

### <a name="workaround"></a>Problemumgehung
Um eine Datei über das Netzwerk zu kopieren, müssen Sie sie zunächst entschlüsseln. Verwenden Sie eine der folgenden Methoden an:

- Verwenden Sie den **copy /d**-Befehl. Sie können die verschlüsselten Dateien dadurch als entschlüsselte Dateien am Ziel speichern.
- Legen Sie den folgenden Registrierungsschlüssel fest:
  - Path = HKLM\Software\Policies\Microsoft\Windows\System
  - Werttyp = DWORD
  - Name = CopyFileAllowDecryptedRemoteDestination
  - Value = 1

Beachten Sie jedoch, dass sich das Festlegen des Registrierungsschlüssels auf alle Kopiervorgänge von Netzwerkfreigaben auswirkt.

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.
[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.

