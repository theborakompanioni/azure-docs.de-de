---
title: Wie speichert Azure RemoteApp Benutzerdaten und -einstellungen? | Microsoft Docs
description: "Erfahren Sie, wie Azure RemoteApp Benutzerdaten mit dem Benutzerprofil-Datenträger speichert."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: e125af62-5c1a-4186-a238-52f537f7bb34
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: dd281ead7d2c06af98ea0baea31d897b5be0d9d7
ms.openlocfilehash: 04d8031a4c5f06d56afc1d08d4a4fa5fb50f8a25
ms.lasthandoff: 01/12/2017


---
# <a name="how-does-azure-remoteapp-save-user-data-and-settings"></a>Wie speichert Azure RemoteApp Benutzerdaten und -einstellungen?
> [!IMPORTANT]
> Azure RemoteApp wird eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Azure RemoteApp speichert die Benutzeridentität und -anpassungen über alle Geräte und Sitzungen hinweg. Diese Benutzerdaten werden auf einem benutzer- und sammlungsspezifischen Datenträger gespeichert, der sogenannte Benutzerprofil-Datenträger. Der Datenträger gilt für den Benutzer und stellt sicher, dass er eine einheitliche Umgebung erfährt, unabhängig davon, wo er sich anmeldet.

Benutzerprofil-Datenträger sind für den Benutzer vollständig transparent – Benutzer speichern Dokumente in ihrem Dokumentenordner (scheinbar auf einem lokalen Laufwerk), und ändern ihre App-Einstellungen wie gewohnt. Gleichzeitig bleiben alle persönlichen Einstellungen beim Verbinden mit Azure RemoteApp von jedem Gerät aus bestehen. Der Benutzer sieht lediglich seine Daten am selben Ort.

Jeder Benutzerprofil-Datenträger verfügt über 50 GB permanenten Speicher und enthält sowohl Benutzerdaten als auch Anwendungseinstellungen. 

Lesen Sie weiter, um mehr über die Besonderheiten von Benutzerprofildaten zu erfahren.

> [!NOTE]
> Müssen Sie den Benutzerprofil-Datenträger (User Profile Disks,UPD) deaktivieren? Dies ist nun möglich. Die Details dazu finden Sie im Blogbeitrag von Pavithra [Disable User Profile Disks (UPDs)](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/) in Azure RemoteApp.
> 
> 

## <a name="how-can-an-admin-get-to-the-data"></a>Wie kann ein Administrator auf die Daten zugreifen?
Wenn Sie Zugriff auf die Daten für einen Ihrer Benutzer benötigen (für die Wiederherstellung im Notfall oder wenn der Benutzer das Unternehmen verlässt), wenden Sie sich an den Azure-Support, und stellen Sie die Abonnementinformationen für die Sammlung und die Identität des Benutzers bereit. Das Team von Azure RemoteApp stellt Ihnen eine URL für die virtuelle Festplatte zur Verfügung. Laden Sie diese VHD herunter, und rufen Sie alle Dokumente oder Dateien ab, die Sie benötigen. Die virtuelle Festplatte hat eine Größe von 50 GB, weshalb das Herunterladen ein wenig Zeit in Anspruch nimmt.

## <a name="is-the-data-backed-up"></a>Werden die Daten gesichert?
Ja, wir speichern einen Backup der Benutzerdaten je nach geografischem Standort. Die Daten sind schreibgeschützt und auf die gleiche Weise zugänglich wie die regulären Daten (kontaktieren Sie dafür das Azure RemoteApp-Team), wenn das primäre Rechenzentrum ausgefallen ist. Die Daten werden in Echtzeit in den Sicherungsspeicherort kopiert, und wir behalten keine Kopien der verschiedenen Versionen bei. Bei einer Beschädigung von Daten sind wir also nicht in der Lage, diese auf eine zuvor funktionierende Version wiederherzustellen. Wenn allerdings das primäre Rechenzentrum ausfällt, können Sie die Benutzerdaten von einem anderen Speicherort abrufen.

## <a name="how-do-users-see-the-upd-on-the-server-side"></a>Wie wird ein Benutzerprofil-Datenträger dem Benutzer auf der Serverseite angezeigt?
Jeder Benutzer hat sein eigenes Verzeichnis auf dem Server, das dem Benutzerprofil-Datenträger zugeordnet ist: c:\Users\username.

## <a name="whats-the-best-way-to-use-outlook-and-upd"></a>Was ist die beste Möglichkeit, Outlook und den Benutzerprofil-Datenträger zu verwenden?
Azure RemoteApp speichert den Outlook-Zustand (Postfächer, PSTs) zwischen den Sitzungen. Um dies zu ermöglichen, müssen PSTs in den Benutzerprofildaten gespeichert werden (c:\users\<Benutzername>). Dies ist der Standardspeicherort für die Daten, und so lange dieser Speicherort nicht geändert wird, werden die Daten zwischen den Sitzungen beibehalten.

Außerdem wird empfohlen, dass Sie den "Cache"-Modus in Outlook und den "Server/online"-Modus für die Suche verwenden.

[In diesem Artikel](remoteapp-outlook.md) finden Sie weitere Informationen zur Verwendung von Outlook und Azure RemoteApp.

## <a name="what-about-redirection"></a>Was ist mit Umleitungen?
Sie können konfigurieren, dass Azure RemoteApp Benutzern durch Einrichten einer [Umleitung](remoteapp-redirection.md)Zugriff auf lokale Geräte gewährt. Lokale Geräte können dann auf die Daten auf dem Benutzerprofil-Datenträger zugreifen.

## <a name="can-i-use-my-upd-as-a-network-share"></a>Kann ich meinen Benutzerprofil-Datenträger als Netzwerkfreigabe verwenden?
Nein. Benutzerprofil-Datenträger können nicht als Netzwerkfreigabe verwendet werden. Ein Benutzerprofil-Datenträger steht dem Bentuzer nur zur Verfügung, wenn er aktiv mit Azure RemoteApp verbunden ist.

## <a name="if-i-delete-a-user-from-a-collection-is-their-upd-deleted"></a>Wird beim Löschen eines Benutzers aus einer Sammlung dessen Benutzerprofil-Datenträger gelöscht?
Nein. Wenn Sie einen Benutzer löschen, wird der Benutzerprofil-Datenträger nicht automatisch gelöscht. Stattdessen speichern wir die Daten, bis Sie die Sammlung löschen. 90 Tage nach dem Löschen der Sammlung löschen wir alle Benutzerprofil-Datenträger. 

Wenn Sie einen Benutzerprofil-Datenträger aus einer Sammlung löschen müssen, kontaktieren Sie das Azure RemoteApp-Team. Wir können den Datenträger von unserer Seite aus löschen.

## <a name="can-i-access-my-users-upds-either-current-or-deleted-users"></a>Kann ich auf die Benutzerprofil-Datenträger meiner Benutzer zugreifen (aktuelle oder gelöschte Benutzer)?
Ja, wenn Sie sich an das [Azure RemoteApp](mailto:remoteappforum@microsoft.com)-Team wenden, können wir eine URL für den Datenzugriff für Sie generieren. Sie haben etwa 10 Stunden Zeit, um Daten oder Dateien vom Benutzerprofil-Datenträger herunterzuladen, bis der Zugriff abläuft.

## <a name="are-upds-available-offline"></a>Sind Benutzerprofil-Datenträger offline verfügbar?
Momentan bieten wir über das zehnstündige Zugriffsfenster hinaus keinen Offline-Zugriff auf Benutzerprofil-Datenträger. Dies bedeutet, dass wir derzeit keine Möglichkeit haben, Ihnen längeren Zugriff für komplexere Aufgaben wie dem Ausführen von Antivirus-Software auf Benutzerprofil-Datenträgern oder dem Zugriff auf die Daten für eine Überwachung zu gewähren.

## <a name="do-registry-key-settings-persist"></a>Werden Einstellungen für Registrierungsschlüssel beibehalten?
Ja. Alles, was in „HKEY_Current_User“ geschrieben ist, ist Teil des Benutzerprofil-Datenträgers.

## <a name="can-i-disable-upds-for-a-collection"></a>Kann ich Benutzerprofil-Datenträger für eine Sammlung deaktivieren?
Ja, Sie können das Azure RemoteApp-Team darum bitten, Benutzerprofil-Datenträger für ein Abonnement zu löschen. Sie können dies jedoch nicht selbst tun. Das bedeutet, dass Benutzerprofil-Datenträger für alle Sammlungen im Abonnement deaktiviert werden.

In den folgenden Situationen möchten Sie Benutzerprofil-Datenträger möglicherweise deaktivieren: 

* Sie benötigen Vollzugriff auf und Kontrolle über Benutzerdaten (z. B. zu Überwachungs- und Prüfzwecken bei Finanzinstituten).
* Sie verfügen über lokale Lösungen zur Benutzerprofilverwaltung, die Sie in Ihrer in die Domäne eingebundenen Azure RemoteApp-Bereitstellung weiter nutzen möchten. Dazu muss der Profil-Agent in das Gold-Image geladen werden. 
* Sie benötigen keinen lokalen Datenspeicher oder haben alle Daten in die Cloud oder eine Dateifreigabe verlagert und möchten das lokale Speichern von Daten mit Azure RemoteApp steuern.

Weitere Informationen finden Sie unter [Deaktivieren von Benutzerprofil-Datenträgern (UPDs) in Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/).

## <a name="can-i-restrict-users-from-saving-data-to-the-system-drive"></a>Kann ich verhindern, dass Benutzer Daten auf dem Systemlaufwerk speichern?
Ja, aber Sie müssen dies im Vorlagenimage einrichten, bevor Sie die Sammlung erstellen. Führen Sie die folgenden Schritte aus, um den Zugriff auf das Systemlaufwerk zu blockieren:

1. Führen Sie **gpedit.msc** auf dem Vorlagenimage aus.
2. Navigieren Sie zu **Benutzerkonfiguration > Administrative Vorlagen > Windows-Komponenten > Explorer**.
3. Wählen Sie die folgenden Optionen:
   * **Ausblenden angegebener Datenträger vom Arbeitsplatz**
   * **Verhindern des Zugriffs auf Laufwerke vom Arbeitsplatz**

## <a name="can-i-seed-upds-i-want-to-put-some-data-in-the-upd-thats-available-the-first-time-the-user-signs-in"></a>Kann bei Benutzerprofil-Datenträgern Seeding ausgeführt werden? Ich möchte dem Benutzerprofil-Datenträger einige Daten hinzufügen, die dem Benutzer zur Verfügung stehen, wenn er sich zum ersten Mal anmeldet.
Ja, wenn Sie das Vorlagenimage erstellen, können Sie Informationen zum Standardprofil hinzufügen. Diese Informationen werden dann zum Benutzerprofil-Datenträger hinzugefügt.

## <a name="can-i-change-the-size-of-the-upd-depending-on-how-much-data-i-want-to-store"></a>Kann ich die Größe des Benutzerprofil-Datenträgers je nach gespeicherter Datenmenge ändern?
Nein, alle Benutzerprofil-Datenträger verfügen über 50 GB Speicher. Wenn Sie unterschiedliche Mengen an Daten speichern möchten, versuchen Sie Folgendes:

1. Deaktivieren Sie den Benutzerprofil-Datenträger für die Sammlung.
2. Richten Sie eine Dateifreigabe ein, damit Benutzer Zugriff erhalten.
3. Laden Sie die Dateifreigabe mithilfe eines Skripts zum Starten. Informationen zu Skripts zum Starten in Azure RemoteApp finden Sie weiter unten.
4. Weisen Sie die Benutzer an, alle Daten in der Dateifreigabe zu speichern.

## <a name="how-do-i-run-a-startup-script-in-azure-remoteapp"></a>Wie führe ich ein Startskript in Azure RemoteApp aus?
Wenn Sie ein Startskript ausführen möchten, beginnen Sie mit dem Erstellen einer geplanten Aufgabe im Vorlagenimage, das Sie für die Sammlung verwenden möchten. (Tun Sie dies, *bevor* Sie "Sysprep" ausführen.) 

![Erstellen Sie eine Systemaufgabe.](./media/remoteapp-upd/upd1.png)

![Erstellen Sie eine Systemaufgabe, die ausgeführt wird, wenn sich ein Benutzer anmeldet.](./media/remoteapp-upd/upd2.png)

Ändern Sie auf der Registerkarte **Allgemein** unbedingt das **Benutzerkonto** in den Sicherheitsoptionen auf „BUILTIN\Users“.

![Ändern Sie das Benutzerkonto zu einer Gruppe](./media/remoteapp-upd/upd4.png)

Die geplante Aufgabe wird mit den Anmeldeinformationen des Benutzers Ihr Startskript starten. Planen Sie die Ausführung der Aufgabe jedes Mal, wenn sich ein Benutzer anmeldet.

![Legen Sie den Trigger für die Aufgabe auf "Bei Anmeldung" fest.](./media/remoteapp-upd/upd3.png)

Sie können auch [auf Gruppenrichtlinien basierte Startskripts](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx)verwenden. 

## <a name="what-about-placing-a-startup-script-in-the-start-menu-would-that-work"></a>Kann das Startskript auch im Startmenü platziert werden? Funktioniert das?
Mit anderen Worten, kann ich eine BAT-Datei erstellen, die ein Config-Fenster-Skript ausführt, es im Ordner "c:\ProgramData\Microsoft\Windows\Start" speichern, und das Skript dann immer dann ausführen lassen, wenn ein Benutzer eine RemoteApp-Sitzung startet?

Nein, dies wird von Azure RemoteApp nicht unterstützt, da der RDSH genutzt wird, der Startskripts im Startmenü ebensowenig unterstützt.

## <a name="can-i-use-mstscexe-the-remote-desktop-program-to-configure-logon-scripts"></a>Kann ich "mstsc.exe" (das Remotedesktop-Programm) zum Konfigurieren von Anmeldeskripts verwenden?
Nein, dies wird von Azure RemoteApp nicht unterstützt.

## <a name="can-i-store-data-on-the-vm-locally"></a>Kann ich die Daten auf dem virtuellen Computer auch lokal speichern?
NEIN, Daten, die zwar auf der VM, aber nicht auf dem Benutzerprofil-Datenträger gespeichert werden, gehen verloren. Die Wahrscheinlichkeit ist hoch, dass der Benutzer bei der nächsten Sitzung in Azure RemoteApp nicht den gleichen virtuellen Computer wie zuvor erhält. Wir ordnen Benutzern keinen bestimmten virtuellen Computer zu. Der Benutzer meldet sich beim nächsten Mal also nicht auf demselben virtuellen Computer an, und die Daten gehen verloren. Wenn die Sammlung aktualisiert wird, werden die vorhandenen virtuellen Computer zudem durch neue ersetzt – wodurch alle auf der VM gespeicherten Daten verloren gehen. Für die Datenspeicherung empfehlen sich daher der Benutzerprofil-Datenträger, freigegebener Speicher wie Azure Files, Dateiserver in einem VNET oder die Cloud mithilfe eines Cloudspeichersystems wie DropBox.

## <a name="how-do-i-mount-an-azure-file-share-on-a-vm-using-powershell"></a>Wie stelle ich eine Azure-Dateifreigabe mithilfe von PowerShell auf einem virtuellen Computer bereit?
Mithilfe des Net-PSDrive-Cmdlets können Sie das Laufwerk wie folgt bereitstellen:

    New-PSDrive -Name <drive-name> -PSProvider FileSystem -Root \\<storage-account-name>.file.core.windows.net\<share-name> -Credential :<storage-account-name>


Sie können Ihre Anmeldeinformationen außerdem wie folgt speichern:

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>


So können Sie das Parameter „-Credential“ im New-PSDrive-Cmdlet überspringen.


