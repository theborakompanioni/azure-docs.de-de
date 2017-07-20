---
title: Behandeln von Problemen mit Azure Backup Server | Microsoft-Dokumentation
description: Behandeln von Problemen bei der Installation, bei der Registrierung von Azure Backup Server und bei der Sicherung und Wiederherstellung von Anwendungsworkloads
services: backup
documentationcenter: 
author: pvrk
manager: shreeshd
editor: 
ms.assetid: 2d73c349-0fc8-4ca8-afd8-8c9029cb8524
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: pullabhk;markgal;
ms.translationtype: Human Translation
ms.sourcegitcommit: 9edcaee4d051c3dc05bfe23eecc9c22818cf967c
ms.openlocfilehash: a8df63821af039b7a5ad34f065423701485ee7d8
ms.contentlocale: de-de
ms.lasthandoff: 06/08/2017


---

# <a name="troubleshoot-azure-backup-server"></a>Behandeln von Problemen mit Azure Backup Server

Die folgende Tabelle enthält Problembehandlungsinformationen für Fehler, die bei der Verwendung von Azure Backup Server auftreten.


## <a name="installation-issues"></a>Probleme bei der Installation

| Vorgang | Fehlerdetails | Problemumgehung |
|-----------|---------------|------------|
|Installation | Setup konnten Registrierungsmetadaten nicht aktualisieren. Dieser Updatefehler kann zu einer übermäßigen Speicherbelegung führen. Um dies zu vermeiden, aktualisieren Sie den Registrierungseintrag zum Trimmen von ReFS. | Passen Sie den Registrierungsschlüssel „SYSTEM\CurrentControlSet\Control\FileSystem\RefsEnableInlineTrim“ an. Legen Sie den Wert „Dword“ auf 1 fest. |
|Installation | Setup konnten Registrierungsmetadaten nicht aktualisieren. Dieser Updatefehler kann zu einer übermäßigen Speicherbelegung führen. Um dies zu vermeiden, aktualisieren Sie den Registrierungseintrag „Volume SnapOptimization“. | Erstellen Sie den Registrierungsschlüssel „SOFTWARE\Microsoft Data Protection Manager\Configuration\VolSnapOptimization\WriteIds“ mit einem leeren Zeichenfolgenwert. |

## <a name="registration-and-agent-related-issues"></a>Probleme mit der Registrierung oder dem Agent
| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Registrieren bei einem Tresor | Es wurden ungültige Tresoranmeldeinformationen angegeben. Die Datei ist entweder beschädigt oder enthält nicht die aktuellen Anmeldeinformationen für den Wiederherstellungsdienst. | Gehen Sie wie folgt vor, um diesen Fehler zu beheben:  <ol><li> Laden Sie die neueste Datei mit Anmeldeinformationen aus dem Tresor herunter, und versuchen Sie es anschließend erneut. <br>(ODER)</li> <li> Falls sich das Problem mit der obigen Aktion nicht beheben lässt, laden Sie die Anmeldeinformationen in ein anderes lokales Verzeichnis herunter, oder erstellen Sie einen neuen Tresor. <br>(ODER)</li> <li> Aktualisieren Sie die Datums- und Uhrzeiteinstellungen gemäß den Angaben in [diesem Blog](https://azure.microsoft.com/blog/troubleshooting-common-configuration-issues-with-azure-backup/). <br>(ODER)</li> <li> Überprüfen Sie, ob sich unter „C:\Windows\Temp“ mehr als 65.000 Dateien befinden. Verschieben Sie veraltete Dateien an einen anderen Speicherort, oder löschen Sie die Elemente im Ordner „Temp“. <br>(ODER)</li> <li> Überprüfen Sie den Status der Zertifikate. <br> a. Öffnen Sie „Computerzertifikate verwalten“ (in der Systemsteuerung). <br> b. Erweitern Sie den Knoten „Persönlich“ und den untergeordneten Knoten „Zertifikate“. <br> c.  Entfernen Sie das Zertifikat „Windows Azure Tools“. <br> d. Wiederholen Sie die Registrierung im Azure Backup-Client. <br> (ODER) </li> <li> Überprüfen Sie, ob eine Gruppenrichtlinie eingerichtet ist. </li></ol> |
| Übertragen von Agents auf geschützte Server mithilfe von Push | Fehler beim Agent-Vorgang. Bei der Kommunikation mit dem DPM-Agent-Koordinator-Dienst auf \<Servername> ist ein Fehler aufgetreten. | **Vorgehensweise für den Fall, dass die im Produkt empfohlene Aktion nicht funktioniert:** <ol><li> Falls Sie einen Computer aus einer nicht vertrauenswürdigen Domäne anfügen, führen Sie [diese Schritte](https://technet.microsoft.com/library/hh757801(v=sc.12).aspx) aus. <br> (ODER) </li><li> Falls Sie einen Computer aus einer vertrauenswürdigen Domäne anfügen, verwenden Sie die Problembehandlungsschritte aus [diesem Blog](https://blogs.technet.microsoft.com/dpm/2012/02/06/data-protection-manager-agent-network-troubleshooting/). <br>(ODER)</li><li> Deaktivieren Sie die Antivirensoftware. Wird das Problem dadurch behoben, ändern Sie die Antivireneinstellungen, wie in [diesem Artikel] (https://technet.microsoft.com/library/hh757911.aspx) beschrieben.</li></ol> |
| Übertragen von Agents auf geschützte Server mithilfe von Push | Die für den Server angegebenen Anmeldeinformationen sind ungültig. | **Vorgehensweise für den Fall, dass die im Produkt empfohlene Aktion nicht funktioniert:** <br> Installieren Sie den Schutz-Agent manuell auf dem Produktionsserver, wie in [diesem Artikel](https://technet.microsoft.com/library/hh758186(v=sc.12).aspx#BKMK_Manual) beschrieben.|


## <a name="configuring-protection-group"></a>Konfigurieren von Schutzgruppen
| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Konfigurieren von Schutzgruppen | Die Anwendungskomponente konnte von DPM auf dem geschützten Computer nicht aufgelistet werden. | Klicken Sie auf dem Konfigurationsbildschirm für Schutzgruppen auf der entsprechenden Datenquellen-/Komponentenebene auf „Aktualisieren“. |
| Konfigurieren von Schutzgruppen | Der Schutz kann nicht konfiguriert werden. | Falls es sich bei dem geschützten Server um einen SQL-Server handelt, überprüfen Sie, ob das Systemkonto (NTAuthority\System) auf dem geschützten Computer über Berechtigungen der SysAdmin-Rolle verfügt, wie in [diesem Artikel](https://technet.microsoft.com/library/hh757977(v=sc.12).aspx) angegeben.
| Konfigurieren von Schutzgruppen | Im Speicherpool für diese Schutzgruppe ist nicht genügend freier Speicherplatz vorhanden. | Die dem Speicherpool hinzugefügten Datenträger [dürfen keine Partition enthalten](https://technet.microsoft.com/library/hh758075(v=sc.12).aspx). Löschen Sie alle auf den Datenträgern vorhandenen Volumes, und fügen Sie die Datenträger dann dem Speicherpool hinzu.|
| Richtlinienänderung |Die Sicherungsrichtlinie konnte nicht geändert werden. Fehler: Der aktuelle Vorgang ist aufgrund eines internen Dienstfehlers [0x29834] fehlgeschlagen. Wiederholen Sie den Vorgang nach einiger Zeit. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support. |**Ursache:**<br/>Dieser Fehler tritt auf, wenn Sicherheitseinstellungen aktiviert sind, Sie versuchen, die Beibehaltungsdauer unter die oben angegebenen Mindestwerte zu verkürzen, und eine nicht unterstützte Version verwenden (unter MAB-Version 2.0.9052 und Azure Backup Server-Update 1). <br/>**Empfohlene Maßnahme:**<br/> In diesem Fall müssen Sie die Beibehaltungsdauer höher als den angegebenen Mindestzeitraum festlegen (sieben Tage für „Täglich“, vier Wochen für „Wöchentlich“, drei Wochen für „Monatlich“ oder ein Jahr für „Jährlich“), um mit richtlinienbezogenen Aktualisierungen fortzufahren. Die optional bevorzugte Vorgehensweise ist das Aktualisieren des Backup-Agents und von Azure Backup Server, um in den Genuss aller Sicherheitsupdates zu kommen. |

## <a name="backup"></a>Sicherung
| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Sicherung | Replikat inkonsistent | Ausführlichere Informationen zu den Ursachen für ein inkonsistentes Replikat sowie entsprechende Vorschläge finden Sie [hier](https://technet.microsoft.com/library/cc161593.aspx). <br> <ol><li> Überprüfen Sie bei einer Systemstatus- oder BMR-Sicherung, ob Windows Server-Sicherung auf dem geschützten Server installiert ist. </li><li> Prüfen Sie den DPM-Speicherpool auf dem DPM-/MABS-Server auf Speicherplatzprobleme, und ordnen Sie ggf. Speicherplatz zu. </li><li> Prüfen Sie den Zustand des Volumeschattenkopie-Diensts auf dem geschützten Server. Ist er deaktiviert, legen Sie ihn auf „Manuell starten“ fest, und starten Sie den Dienst auf dem Server. Kehren Sie anschließend zur DPM-/MABS-Konsole zurück, und starten Sie die Synchronisierung mit Konsistenzprüfung.</li></ol>|
| Sicherung | Beim Ausführen des Auftrags ist ein unerwarteter Fehler aufgetreten. Das Gerät ist nicht bereit. | **Vorgehensweise für den Fall, dass die im Produkt empfohlene Aktion nicht funktioniert:** <br> <ol><li>Legen Sie den Schattenkopie-Speicherbereich für die Elemente in der Schutzgruppe auf „Unbegrenzt“ fest, und führen Sie die Konsistenzprüfung aus. <br></li> (ODER) <li>Löschen Sie die vorhandene Schutzgruppe, und erstellen Sie mehrere neue mit jeweils einem der Elemente.</li></ol> |
| Sicherung | Wenn nur der Systemstatus gesichert werden soll, überprüfen Sie, ob ausreichend freier Speicherplatz auf dem geschützten Computer vorhanden ist, um die Systemstatussicherung zu speichern. | <ol><li>Vergewissern Sie sich, dass WSB auf dem geschützten Computer installiert ist.</li><li>Vergewissern Sie sich, dass auf dem geschützten Computer genügend Speicherplatz für den Systemstatus vorhanden ist: Öffnen Sie hierzu am besten WSB auf dem geschützten Computer, klicken Sie sich durch die Auswahloptionen, und wählen Sie „BMR“ aus. Daraufhin wird auf der Benutzeroberfläche der erforderliche Speicherplatz angezeigt. Öffnen Sie „WSB“ > „Lokale Sicherung“ > „Sicherungszeitplan“ > „Sicherungskonfiguration auswählen“ > „Vollständiger Server (<Größe>)“. Verwenden Sie diese Größe zur Überprüfung.</li></ol>
| Sicherung | Fehler bei der Erstellung eines Onlinewiederherstellungspunkts. | Falls die Fehlermeldung „Der Microsoft Azure Backup-Agent konnte keine Momentaufnahme des ausgewählten Volumes erstellen.“ angezeigt wird, erhöhen Sie den Speicherplatz im Replikat- und Wiederherstellungspunktvolume.
| Sicherung | Fehler bei der Erstellung eines Onlinewiederherstellungspunkts. | Falls die Fehlermeldung „Die Verschlüsselungspassphrase für diesen Server ist nicht festgelegt. Konfigurieren Sie eine Verschlüsselungspassphrase.“ angezeigt wird, konfigurieren Sie eine Verschlüsselungspassphrase. Sollte dies nicht erfolgreich sein, <br> <ol><li>überprüfen Sie, ob der Ablageordner vorhanden ist. Der in der Registrierung „HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config“ angegebene Speicherort namens „ScratchLocation“ muss vorhanden sein.</li><li> Falls der Ablageordner vorhanden ist, führen Sie eine erneute Registrierung mit der alten Passphrase durch. **Speichern Sie konfigurierte Verschlüsselungspassphrasen immer an einem sicheren Ort.**</li><ol>
| Sicherung | Sicherungsfehler für BMR | Verschieben Sie bei einer großen BMR einige Anwendungsdateien auf das Betriebssystemlaufwerk, und versuchen Sie es dann erneut. |
| Sicherung | Fehler beim Zugreifen auf Dateien/freigegebene Ordner | Ändern Sie die Antivireneinstellungen anhand [dieser Vorschläge](https://technet.microsoft.com/library/hh757911.aspx).|

## <a name="change-passphrase"></a>Ändern der Passphrase
| Vorgang | Fehlerdetails | Problemumgehung |
| --- | --- | --- |
| Ändern der Passphrase |Die eingegebene Sicherheits-PIN ist nicht korrekt. Geben Sie die korrekte Sicherheits-PIN an, um diesen Vorgang abzuschließen. |**Ursache:**<br/> Dieser Fehler tritt bei der Eingabe einer ungültigen oder abgelaufenen Sicherheits-PIN beim Ausführen eines kritischen Vorgangs (z.B. Ändern der Passphrase) auf. <br/>**Empfohlene Maßnahme:**<br/> Um den Vorgang abzuschließen, müssen Sie die gültige Sicherheits-PIN eingeben. Um die PIN abzurufen, melden Sie sich beim Azure-Portal an und navigieren zu „Recovery Services-Tresor > Einstellungen > Eigenschaften > Sicherheits-PIN generieren“. Verwenden Sie diese PIN, um die Passphrase zu ändern. |
| Ändern der Passphrase |Fehler beim Vorgang. ID: 120002 |**Ursache:**<br/>Dieser Fehler tritt auf, wenn Sicherheitseinstellungen aktiviert sind, Sie versuchen, die Passphrase zu ändern, und eine nicht unterstützte Version verwenden.<br/>**Empfohlene Maßnahme:**<br/> Um die Passphrase zu ändern, müssen Sie zuerst den Backup-Agent auf Mindestversion 2.0.9052 und Azure Backup Server auf mindestens Update 1 aktualisieren und dann die gültige Sicherheits-PIN eingeben. Um die PIN abzurufen, melden Sie sich beim Azure-Portal an und navigieren zu „Recovery Services-Tresor > Einstellungen > Eigenschaften > Sicherheits-PIN generieren“. Verwenden Sie diese PIN, um die Passphrase zu ändern. |

