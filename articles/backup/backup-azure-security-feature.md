---
title: "Sicherheitsfunktionen für den Schutz von Hybridsicherungen mit Azure Backup | Microsoft Docs"
description: "In diesem Artikel wird erläutert, wie Sie mithilfe der Azure Backup-Sicherheitsfunktionen mehr Sicherheit für Ihre Sicherungen gewinnen können."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 47bc8423-0a08-4191-826d-3f52de0b4cb8
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2016
ms.author: pajosh
translationtype: Human Translation
ms.sourcegitcommit: 4a846e86fa8cefbee04e1e30078a0d2aabdd834d
ms.openlocfilehash: 1b7df71f103e8284bdcc7bc3a27fe44815b12305


---
# <a name="security-features-for-protecting-hybrid-backups-using-azure-backup"></a>Sicherheitsfunktionen für den Schutz von Hybridsicherungen mit Azure Backup
Immer mehr Kunden sind mit Sicherheitsproblemen wie Schadsoftware, Ransomware oder Angriffen konfrontiert. Diese Sicherheitsprobleme führen zu Datenverlusten, und die Kosten pro Sicherheitsverletzung steigen ständig. Zum Schutz gegen solche Angriffe bietet Azure Backup jetzt neue Sicherheitsfunktionen für den Schutz von Hybridsicherungen. In diesem Artikel wird erläutert, wie diese Funktionen mit Microsoft Azure Recovery Services-Agent und Microsoft Azure Backup Server aktiviert und genutzt werden können. Diese Funktionen konzentrieren sich auf drei Sicherheitsbereiche:

1. **Prävention** – Es wurde eine zusätzliche Authentifizierungsebene hinzugefügt, die bei allen kritischen Vorgängen wie Passphrasenänderungen zum Tragen kommt. Durch diese Überprüfung wird gewährleistet, dass solche Vorgänge nur von Benutzern durchgeführt werden können, die über gültige Azure-Anmeldeinformationen verfügen.
2. **Warnungen** – Bei kritischen Vorgängen wie dem Löschen von Sicherungsdaten wird eine E-Mail-Benachrichtigung an den Administrator der Abonnements gesendet. Durch diese E-Mail ist sichergestellt, dass der Benutzer rechtzeitig über solche Aktionen benachrichtigt wird.
3. **Wiederherstellung** – Gelöschte Sicherungsdaten werden für zusätzliche 14 Tage ab dem Löschdatum aufbewahrt. So wird gewährleistet, dass die Daten innerhalb des vorgegebenen Zeitraums wiederhergestellt werden können und es also auch bei Angriffen nicht zu Datenverlusten kommt. Zum Schutz vor beschädigten Daten werden darüber hinaus auch mehr Mindestwiederherstellungspunkte gepflegt.

> [!NOTE]
> Die Sicherheitsfunktionen sollten nur aktiviert werden, wenn Sie die folgenden Elemente verwenden: <br/>
> * **MAB Agent** – ab Version 2.0.9052. Nach der Aktivierung dieser Funktionen sollten Sie auf diese Agent-Version upgraden, um kritische Vorgänge wie „Change Passphrase“ (Passphrase ändern) oder „Stop backup with Delete data“ (Sicherungsabbruch mit Datenlöschung) auszuführen. <br/>
> * **Azure Backup Server** – ab MAB Agent-Version 2.0.9052 mit Azure Backup Server Update 1 <br/>
> * **DPM** – Aktivieren Sie für DPM diese Funktionen nicht. Sie werden in Kürze in zukünftigen Updaterollups zur Verfügung stehen, die Aktivierung hat also auf die vorhandene Funktionalität keine Auswirkung. <br/>
> * **VM-Backup-Funktion von IaaS** – Aktivieren Sie diese Funktionen nicht für IaaS VM Backup. Sie stehen für IaaS VM Backup noch nicht zur Verfügung, die Aktivierung hat also auf IaaS VM Backup keine Auswirkung.
> * Nach der Aktivierung stehen Ihnen die Sicherheitsfunktionen für alle Microsoft Azure Recovery Services Agent-Computer (MARS) und für alle Server von Azure Backup Server zur Verfügung, die zu dem Tresor registriert sind. <br/>
> * Die Aktivierung dieser Einstellung ist eine einmalige Aktion, und die Funktionen können nach der Aktivierung nicht wieder deaktiviert werden. <br/>
> * Diese Funktion ist nur für den Recovery Services-Tresor verfügbar.
>
>

## <a name="enabling-security-features"></a>Aktivieren von Sicherheitsfunktionen
Benutzer, die einen Recovery Services-Tresor erstellen, können dann alle Sicherheitsfunktionen nutzen. Für vorhandene Recovery Services-Tresore müssen Sie die folgenden Schritte ausführen, um diese Funktionen zu aktivieren:

1. Melden Sie sich mit Ihren Azure-Anmeldeinformationen beim Azure-Portal an.
2. Geben Sie im Hub-Menü „Recovery Services“ ein, um zur Liste der Wiederherstellungsdienste zu navigieren.

    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-security-feature/browse-to-rs-vaults.png) <br/>

    Die Liste mit den Recovery Services-Tresoren wird angezeigt. Wählen Sie aus dieser Liste einen Tresor aus.

    Das ausgewählte Tresor-Dashboard wird geöffnet.
3. Klicken Sie in der Liste mit den Elementen, die im Tresor angezeigt werden, in den **Einstellungen** auf **Eigenschaften**.

    ![Eigenschaften des Öffnens von Tresoren](./media/backup-azure-security-feature/vault-list-properties.png)
4. Klicken Sie unter **Sicherheitseinstellungen** auf **Aktualisieren**.

    ![Sicherheitseinstellungen öffnen](./media/backup-azure-security-feature/security-settings-update.png)

    Über den Updatelink wird ein Blatt mit den Sicherheitseinstellungen geöffnet, auf dem Sie diese Funktionen aktivieren können. Weiterhin wird die Funktion kurz beschrieben.
5. Wählen Sie einen Wert aus der Dropdownliste **Have you configured Azure Multi-Factor Authentication?** (Haben Sie die Azure Multi-Factor Authentification konfiguriert?), um zu bestätigen, dass Sie [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) aktiviert haben. Wenn Azure Multi-Factor Authentification aktiviert ist, werden Sie gebeten, sich bei der Anmeldung beim Azure-Portal von einem anderen Gerät (z.B. Mobiltelefon) aus zu authentifizieren.

   Als Teil der Sicherheitsfunktionen müssen Sie die im Azure-Portal verfügbare Sicherheits-PIN eingeben, wenn in Azure Backup kritische Vorgänge ausgeführt werden. Durch das Aktivieren von Multi-Factor Authentication wird eine Sicherheitsebene hinzugefügt und gewährleistet, dass nur Benutzer mit gültigen Azure-Anmeldedaten, die sich außerdem von einem zweiten Gerät aus authentifiziert haben, auf das Azure-Portal zugreifen und solche kritischen Vorgänge ausführen können.
6. Verwenden Sie wie in der Abbildung dargestellt die Umschaltfläche **Aktivieren**, um die Sicherheitseinstellungen zu aktivieren, und klicken Sie auf die oben sichtbare Schaltfläche **Speichern**, um die Sicherheitseinstellungen zu speichern. **Aktivieren** kann erst ausgewählt werden, nachdem ein Wert aus der Dropdownliste „Have you configured Azure Multi-Factor Authentication?“ (Haben Sie Azure Multi-Factor Authentication konfiguriert?)  ausgewählt wurde.

    ![Aktivieren der Sicherheitseinstellungen](./media/backup-azure-security-feature/enable-security-settings.png)

## <a name="recovering-deleted-backup-data"></a>Wiederherstellen von gelöschten Sicherungsdaten
Als Sicherheitsmaßnahme bewahrt Azure Backup gelöschte Sicherungsdaten zusätzlich 14 Tage lang auf und löscht sie nicht sofort, wenn der Datenvorgang „Stop backup with delete backup“ (Sicherungsabbruch mit Sicherungslöschung) ausgeführt wird. Um diese Daten innerhalb der 14-tägigen Frist wiederherzustellen, führen Sie die folgenden Schritte aus:

Für Benutzer von **Microsoft Recovery Services Agent (MARS)**:

1. Wenn der Computer, auf dem die Sicherungen durchgeführt wurden, noch verfügbar ist, verwenden Sie in MARS die [Wiederherstellung von Daten auf demselben Computer](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-the-same-machine), um die Wiederherstellung mithilfe aller alten Wiederherstellungspunkte durchzuführen.
2. Wenn der obengenannte Computer nicht mehr verfügbar ist, verwenden Sie die [Wiederherstellung auf einem anderen Computer](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine), um einen anderen MARS-Computer für die Wiederherstellung dieser Daten zu verwenden.

Für Benutzer von **Azure Backup Server **:

1. Wenn der Server, auf dem die Sicherungen durchgeführt wurden, noch verfügbar ist, schützen Sie die gelöschten Datenquellen erneut, und verwenden Sie die Funktion „Daten wiederherstellen“, um die Wiederherstellung mithilfe aller alten Wiederherstellungspunkte durchzuführen.
2. Wenn der oben genannte Computer nicht mehr verfügbar ist, befolgen Sie die Schritte unter [Daten von einem anderen Azure Backup-Server wiederherstellen](backup-azure-alternate-dpm-server.md#recover-data-from-another-azure-backup-server), um diese Daten von einem anderen Server von Azure Backup Server wiederherzustellen.

## <a name="preventing-attacks"></a>Angriffsprävention
Als Teil dieser Funktion wurden Prüfungen hinzugefügt, mit denen sichergestellt wird, dass bestimmte Vorgänge nur von gültigen Benutzern ausgeführt werden können.

### <a name="authentication-to-perform-critical-operations"></a>Authentifizierung für das Ausführen von kritischen Vorgängen
Als Teil der zusätzlich hinzugefügten Authentifizierungsebene für kritische Vorgänge werden Sie beim Ausführen der Vorgänge „Stop Protection with Delete data“ (Schutz mit Datenlöschung beenden) und „Change Passphrase“ dazu aufgefordert, die Sicherheits-PIN einzugeben.

Um die Sicherheits-PIN zu erhalten, führen Sie die folgenden Schritte aus:

1. Melden Sie sich am Azure-Portal an.
2. Navigieren Sie zu Recovery Service Vault (Wiederherstellungsdiensttresor) > Einstellungen > Eigenschaften.
3. Klicken Sie unter Sicherheits-PIN auf **Generieren**. Wenn Sie einen Link generieren, wird ein Blatt geöffnet. Dieses enthält die Sicherheits-PIN, die in der Azure Recovery Services Agent-Benutzeroberfläche eingegeben werden muss.
    Diese PIN gilt nur für fünf Minuten, und wird nach Ablauf dieses Zeitraums automatisch generiert.

### <a name="maintaining-minimum-retention-range"></a>Beibehalten der Mindestvermerkdauer
Um sicherzustellen, dass immer eine geeignete Anzahl von Wiederherstellungspunkten verfügbar ist, wurden die folgenden Prüfungen hinzugefügt:

1. Bei einer eintägigen Aufbewahrung sollten mindestens **sieben** Vermerktage vorgesehen werden.
2. Bei einer einwöchigen Aufbewahrung sollten mindestens **vier** Vermerkwochen vorgesehen werden.
3. Bei einer einmonatigen Aufbewahrung sollten mindestens **drei** Vermerkmonate vorgesehen werden.
4. Bei einer einjährigen Aufbewahrung sollte mindestens **ein** Vermerkjahr vorgesehen werden.

## <a name="notifications-for-critical-operations"></a>Benachrichtigungen über kritische Vorgänge
Immer, wenn kritische Vorgänge ausgeführt werden, erhält der Administrator der Abonnements eine E-Mail-Benachrichtigung mit den Details des Vorgangs. Wenn Sie zusätzliche E-Mail-IDs einrichten möchten, die ebenfalls Benachrichtigungen erhalten sollen, können Sie diese im Azure-Portal konfigurieren.

Die in diesem Artikel genannten Sicherheitsfunktionen bieten Abwehrmechanismen gegen gezielte Angriffe und hindern Angreifer daran, an die Sicherungen heranzukommen. Und was vielleicht noch wichtiger ist: Die Funktionen bieten die Möglichkeit zur Datenwiederherstellung, falls es doch zu einem Angriff kommen sollte.

## <a name="next-steps"></a>Nächste Schritte
* [Erste Schritte mit Azure Recovery Services-Tresor](backup-azure-vms-first-look-arm.md), um diese Funktionen zu aktivieren
* [Laden Sie den neuesten Azure Recovery Services-Agent herunter](http://aka.ms/azurebackup_agent), um Ihre Windows-Computer und Ihre Sicherungsdaten gegen Angriffe zu schützen.
* [Laden Sie die neueste Version von Azure Backup Server herunter](https://aka.ms/latest_azurebackupserver), um Ihre Workloads und Sicherungsdaten gegen Angriffe zu schützen.



<!--HONumber=Feb17_HO1-->


