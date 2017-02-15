---
title: "Replizieren von lokalen virtuellen VMware-Computern oder physischen Servern an einen sekundären Standort | Microsoft Docs"
description: "Verwenden Sie diesen Artikel, wenn Sie VMware-VMs oder physische Windows-/Linux-Server mithilfe von Azure Site Recovery an einen sekundären Standort replizieren möchten."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: jwhit
editor: 
ms.assetid: b2cba944-d3b4-473c-8d97-9945c7eabf63
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2016
ms.author: nisoneji
translationtype: Human Translation
ms.sourcegitcommit: 5614c39d914d5ae6fde2de9c0d9941e7b93fc10f
ms.openlocfilehash: d325ce41e79ec85e08fbf4bb86e0cd7e0edf1c8f


---
# <a name="replicate-on-premises-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Replizieren von lokalen virtuellen VMware-Maschinen oder physischen Servern an einen sekundären Standort
## <a name="overview"></a>Übersicht
InMage Scout in Azure Site Recovery ermöglicht eine Echtzeitreplikation zwischen lokalen VMware-Standorten. InMage Scout ist Teil der Abonnements des Azure Site Recovery-Diensts.

## <a name="prerequisites"></a>Voraussetzungen
**Azure-Konto**: Sie benötigen ein [Microsoft Azure](https://azure.microsoft.com/) -Konto. Für den Einstieg steht ein [kostenloses Testkonto](https://azure.microsoft.com/pricing/free-trial/)zur Verfügung. [Preise für Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) .

## <a name="step-1-create-a-vault"></a>Schritt 1: Erstellen eines Tresors
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf „Neu“ > „Verwaltung“ > „Sicherung und Site Recovery (OMS)“. Alternativ können Sie auf „Durchsuchen“ > „Recovery Services-Tresor“ > „Hinzufügen“ klicken.
3. Geben Sie unter **Name** einen Anzeigenamen an, mit dem der Tresor identifiziert wird. Wenn Sie mehrere Abonnements haben, müssen Sie ein Abonnement auswählen.
4. Erstellen Sie unter **Ressourcengruppe** eine neue Ressourcengruppe, oder wählen Sie eine vorhandene aus. Geben Sie eine Azure-Region an, um die erforderlichen Felder auszufüllen.
5. Wählen Sie unter **Standort** die geografische Region für den Tresor aus. Informationen zu den unterstützten Regionen finden Sie unter [Site Recovery – Preise](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Wenn Sie über das Dashboard schnell auf den Tresor zugreifen möchten, klicken Sie auf „An Dashboard anheften“ und anschließend auf „Erstellen“.
7. Der neue Tresor wird unter „Dashboard“ > „Alle Ressourcen“ und auf dem Hauptblatt „Recovery Services-Tresore“ angezeigt.

## <a name="step-2-configure-the-vault-and-download-inmage-scout-components"></a>Schritt 2: Konfigurieren des Tresors und Herunterladen von InMage Scout-Komponenten
1. Wählen Sie auf dem Blatt „Recovery Services-Tresore“ Ihren Tresor aus, und klicken Sie auf „Einstellungen“.
2. Klicken Sie unter **Einstellungen** > **Erste Schritte** auf **Site Recovery** > Schritt 1: **Bereiten Sie die Infrastruktur vor** > **Schutzziel**.
3. Wählen Sie unter **Schutzziel** die Option „Auf Wiederherstellungsstandort“ und dann „Yes, with VMware vSphere Hypervisor“ (Ja, mit VMware vSphere-Hypervisor) aus. Klicken Sie dann auf „OK“.
4. Klicken Sie unter **Scout-Einrichtung** auf die Option zum Herunterladen, um die InMage Scout 8.0.1 GA-Software und den Registrierungsschlüssel herunterzuladen. Die Setupdateien für alle erforderlichen Komponenten befinden sich in der heruntergeladenen ZIP-Datei.

## <a name="step-3-install-component-updates"></a>Schritt 3: Installieren von Komponentenupdates
0Erfahren Sie mehr über die neuesten [Updates](#updates). Installieren Sie die Updatedateien in der folgenden Reihenfolge auf Servern:

1. RX-Server, falls vorhanden
2. Konfigurationsserver
3. Prozessserver
4. Masterzielserver
5. vContinuum-Server
6. Quellserver (sowohl Windows- als auch Linux-Server)

Installieren Sie die Updates wie folgt:

1. Laden Sie die ZIP-Datei für das [Update](https://aka.ms/asr-scout-update4) herunter. Diese ZIP-Datei enthält die folgenden Dateien:

   * RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz
   * CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
   * UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe
   * UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
   * vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe
   * UA update4-Bits für RHEL5, OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
2. Extrahieren Sie die ZIP-Dateien.<br>
3. **RX-Server**: Kopieren Sie **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** auf den RX-Server, und extrahieren Sie die Datei. Führen Sie im extrahierten Ordner **/Install**aus.<br>
4. **Konfigurationsserver/Prozessserver**: Kopieren Sie **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** auf den Konfigurationsserver und den Prozessserver. Doppelklicken Sie auf die Datei, um sie auszuführen.<br>
5. **Windows-Masterzielserver**: Um den vereinheitlichten Agent zu aktualisieren, kopieren Sie **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** auf den Masterzielserver. Doppelklicken Sie auf die Datei, um sie auszuführen. Beachten Sie, dass der vereinheitlichte Agent auch für den Quellserver anwendbar ist. Sie sollten ihn auch auf dem Quellserver installieren. Dies ist weiter unten in dieser Liste beschrieben.<br>
6. **vContinuum-Server**: Kopieren Sie **vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe** auf den vContinuum-Server.  Stellen Sie sicher, dass Sie den vContinuum-Assistenten geschlossen haben. Doppelklicken Sie auf die Datei, um sie auszuführen.<br>
7. **Linux-Masterzielserver**: Um den vereinheitlichten Agent zu aktualisieren, kopieren Sie **UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** auf den Masterzielserver und extrahieren die Datei. Führen Sie im extrahierten Ordner **/Install**aus.<br>
8. **Windows-Quellserver**: Um den vereinheitlichten Agent zu aktualisieren, kopieren Sie die Datei **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** auf den Quellserver. Doppelklicken Sie auf die Datei, um sie auszuführen.<br>
9. **Linux-Quellserver**: Um den vereinheitlichten Agent zu aktualisieren, kopieren Sie die entsprechende Version der UA-Datei auf den Linux-Server und extrahieren sie. Führen Sie im extrahierten Ordner **/Install**aus.  Beispiel: Kopieren Sie für den RHEL 6.7 64-Bit-Server die Datei **UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** auf den Server, und extrahieren Sie sie. Führen Sie im extrahierten Ordner **/Install**aus.

## <a name="step-4-set-up-replication"></a>Schritt 4: Einrichten der Replikation
1. Richten Sie die Replikation zwischen den Quell- und Zielstandorten von VMware ein.
2. Anleitungen finden Sie in der InMage Scout-Dokumentation, die zusammen mit dem Produkt heruntergeladen wird. Alternativ hierzu können Sie wie folgt auf die Dokumentation zugreifen:

   * [Versionshinweise](https://aka.ms/asr-scout-release-notes)
   * [Kompatibilitätsmatrix](https://aka.ms/asr-scout-cm)
   * [Benutzerhandbuch](https://aka.ms/asr-scout-user-guide)
   * [RX-Benutzerhandbuch](https://aka.ms/asr-scout-rx-user-guide)
   * [Schnellinstallationsanleitung](https://aka.ms/asr-scout-quick-install-guide)

## <a name="updates"></a>Aktualisierungen
### <a name="azure-site-recovery-scout-801-update-4"></a>Azure Site Recovery Scout 8.0.1 Update 4
Scout Update 4 ist ein kumulatives Update. Es verfügt über alle Fixes von Update 1 bis Update 3 und die folgenden neuen Bugfixes und Verbesserungen.

**Unterstützung für die neue Plattform**

* Unterstützung für vCenter/vSphere 6.0, 6.1 und 6.2 hinzugefügt
* Unterstützung für die folgenden Linux-Betriebssysteme wurde hinzugefügt:
  * Red Hat Enterprise Linux (RHEL) 7.0, 7.1 und 7.2
  * CentOS 7.0, 7.1 und 7.2
  * Red Hat Enterprise Linux (RHEL) 6.8
  * CentOS 6.8

> [!NOTE]
> RHEL/CentOS 7 64 Bit **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** ist im grundlegenden Scout GA-Paket **InMage_Scout_Standard_8.0.1 GA.zip** enthalten. Laden Sie das Scout GA-Paket vom Portal herunter (siehe [Schritt 1](#step-1-create-a-vault)).
>
>

**Programmfehlerbehebungen und Verbesserungen**

* Die Behandlung des Herunterfahrens wurde für die folgenden Linux-Betriebssysteme und Klone verbessert, um unerwünschte Probleme bei der erneuten Synchronisierung zu verhindern:
  * Red Hat Enterprise Linux (RHEL) 6.x
  * Oracle Linux (OL) 6.x
* Für Linux sind die vollständigen Berechtigungen für den Ordnerzugriff bei der Installation des vereinheitlichten Agents jetzt ausschließlich auf den lokalen Benutzer beschränkt.
* Windows-Zeitüberschreitungsproblem: Beim Ausgeben von häufigen Vorgängen mit verteilter Konsistenz Verwendung von Lesezeichen für stark ausgelastete verteilte Anwendungen wie SQL- und Share Point-Cluster.
* Protokollfehlerkorrektur im CX Base Installer durchgeführt.
* Downloadlink für VMware vCLI 6.0 für Windows-Masterziel-Base Installer hinzugefügt.
* Weitere Prüfungen und Protokolle für Änderungen der Netzwerkkonfiguration bei Failover- und Notfallwiederherstellungsübungen hinzugefügt.
* Es kann vorkommen, dass Aufbewahrungsinformationen nicht an CX gemeldet werden.  
* Für den physischen Cluster tritt für den Vorgang zur Änderung der Volumegröße mit dem vContinuum-Assistenten ein Fehler auf, wenn es zu einer Verkleinerung des Quellvolumens kommt.
* Für den Clusterschutz ist der Fehler „Failed to find the disk signature“ (Datenträgersignatur nicht gefunden) aufgetreten, wenn der Clusterdatenträger der PRDM-Datenträger ist.
* Absturz des cxps-Transportservers aufgrund einer Ausnahme vom Typ „Außerhalb des Bereichs“.
* Auf der Seite für die Pushinstallation im vContinuum-Assistenten kann die Größe der Servernamen- und IP-Spalten jetzt geändert werden.
* RX-API-Verbesserungen
  * Die fünf letzten verfügbaren allgemeinen Konsistenzpunkte werden bereitgestellt (nur garantierte Tags).
  * Details zu Kapazität und freiem Speicherplatz werden für alle geschützten Geräte angegeben.
  * Der Status des Scout-Treibers wird auf dem Quellserver angegeben.

> [!NOTE]
> * Das Basispaket **InMage_Scout_Standard_8.0.1_GA.zip** verfügt jetzt über den aktualisierten CX Base Installer **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** und den Windows-Masterziel-Base Installer **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**. Verwenden Sie für alle neuen Installationsvorgänge neue CX- und Windows-Masterziel-GA-Bits.
> * Update 4 kann direkt auf 8.0.1 GA angewendet werden.
> * Für die Updates für den Konfigurationsserver und RX kann kein Rollback durchgeführt werden, nachdem sie auf das System angewendet wurden.
>
>

### <a name="azure-site-recovery-scout-801-update-3"></a>Azure Site Recovery Scout 8.0.1 Update 3
Update 3 enthält die folgenden Fehlerbehebungen und Verbesserungen:

* Der Konfigurationsserver und RX können nicht im Site Recovery-Tresor registriert werden, wenn sie sich hinter dem Proxy befinden.
* Die Anzahl von Stunden, in denen die Recovery Point Objective (RPO) nicht erfüllt ist, wird im Integritätsbericht nicht aktualisiert.
* Der Konfigurationsserver wird nicht mit RX synchronisiert, wenn die ESX-Hardwaredetails oder Netzwerkdetails ein UTF-8-Zeichen enthalten.
* Windows Server 2008 R2-Domänencontroller werden nach der Wiederherstellung nicht gestartet.
* Die Offlinesynchronisierung funktioniert nicht wie erwartet.
* Nach dem VM-Failover bleibt die Replikationspaarlöschung in der CX-UI längere Zeit hängen, und Benutzer können den Failback- oder Fortsetzungsvorgang nicht durchführen.
* Momentaufnahmen-Gesamtvorgänge, die vom Konsistenzauftrag durchgeführt werden, wurden optimiert, um Trennungen von Anwendungsverbindungen zu reduzieren, z.B. für SQL-Clients.
* Die Leistung des Konsistenztools (VACP.exe) wurde verbessert, indem die Arbeitsspeichernutzung reduziert wurde, die zum Erstellen von Momentaufnahmen unter Windows erforderlich ist.
* Der Dienst für die Pushinstallation stürzt ab, wenn das Kennwort länger als 16 Zeichen ist.
* Beim Ändern der Anmeldeinformationen führt vContinuum keine Überprüfung und Aufforderung zur Eingabe neuer vCenter-Anmeldeinformationen durch.
* Unter Linux lädt der Masterziel-Cache-Manager (cachemgr) keine Dateien vom Prozessserver herunter, was zu einer Drosselung des Replikationspaars führt.
* Wenn die Datenträgerreihenfolge des physischen Failoverclusters (MSCS) nicht auf allen Knoten gleich ist, wird die Replikation für einige Clustervolumen nicht festgelegt.
  <br/>Beachten Sie, dass der Cluster erneut geschützt werden muss, damit Sie von diesem Fix profitieren können.  
* SMTP-Funktionalität verhält sich nicht erwartungsgemäß, nachdem RX von Scout 7.1 auf Scout 8.0.1 aktualisiert wurde.
* Dem Protokoll wurden weitere Statistiken hinzugefügt, damit der Rollbackvorgang nachverfolgt, wie viel Zeit bis zum Abschluss benötigt wird.
* Unterstützung für Linux-Betriebssysteme auf dem Quellserver wurde hinzugefügt:
  * Red Hat Enterprise Linux (RHEL) 6 Update 7
  * CentOS 6 Update 7
* Die CX- und RX-Benutzeroberfläche kann jetzt die Benachrichtigung für das Paar anzeigen, das in den Bitmapmodus wechselt.
* Die folgenden Sicherheitsfixes wurden in RX hinzugefügt:

| **Problembeschreibung** | **Implementierungsverfahren** |
| --- | --- |
| Autorisierungsumgehung per Parametermanipulation |Der Zugriff auf unzulässige Benutzer wurde beschränkt. |
| Websiteübergreifende Anforderungsfälschung |Das Seitentokenkonzept mit zufälliger Generierung für jede Seite wurde implementiert. <br/>Hierfür gilt Folgendes: <li> Für einen Benutzer ist nur eine Instanz für das einmalige Anmelden vorhanden.</li><li>Die Seitenaktualisierung funktioniert nicht. Es wird eine Umleitung an das Dashboard durchgeführt.</li> |
| Upload schädlicher Dateien |Dateien wurden auf bestimmte Erweiterungen beschränkt. Zulässige Erweiterungen: 7z, aiff, asf, avi, bmp, csv, doc, docx, fla, flv, gif, gz, gzip, jpeg, jpg, log, mid, mov, mp3, mp4, mpc, mpeg, mpg, ods, odt, pdf, png, ppt, pptx, pxd, qt, ram, rar, rm, rmi, rmvb, rtf, sdc, sitd, swf, sxc, sxw, tar, tgz, tif, tiff, txt, vsd, wav, wma, wmv, xls, xlsx, xml und zip. |
| Persistentes websiteübergreifendes Skripting |Eingabevalidierungen wurden hinzugefügt. |

> [!NOTE]
> * Alle Site Recovery-Updates sind kumulativ. Update 3 verfügt über alle Fixes von Update 1 und Update 2. Update 3 kann direkt auf 8.0.1 GA angewendet werden.
> * Für die Updates für den Konfigurationsserver und RX kann kein Rollback durchgeführt werden, nachdem sie auf das System angewendet wurden.
>
>

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure Site Recovery Scout 8.0.1 Update 2 (Update 03Dec15)
Fixes in Update 2 umfassen Folgendes:

* **Konfigurationsserver**: Behebung eines Problems, das die korrekte Funktion der kostenlosen 31-Tage-Messfunktion verhinderte, wenn der Konfigurationsserver in Site Recovery registriert wurde.
* **Vereinheitlichter Agent**: Behebung eines Problems in Update 1, das die Installation des Updates auf dem Masterzielserver verhinderte, wenn ein Upgrade von Version 8.0 auf 8.0.1 durchgeführt wurde.

### <a name="azure-site-recovery-scout-801-update-1"></a>Azure Site Recovery Scout 8.0.1 Update 1
Update 1 enthält die folgenden Fehlerbehebungen und neuen Features:

* 31 Tage lang kostenloser Schutz pro Serverinstanz. Dies ermöglicht es Ihnen, die Funktionalität zu testen oder eine Machbarkeitsstudie einzurichten.
  * Alle Vorgänge auf dem Server, einschließlich Failover und Failback, sind für die ersten 31 Tage kostenlos. Dies gilt ab dem Zeitpunkt, zu dem ein Server erstmalig mit Site Recovery Scout geschützt wird.
  * Ab dem 32. Tag wird für jeden geschützten Server die Standardinstanzgebühr für den Azure Site Recovery-Schutz für eine Website in Rechnung gestellt, die im Besitz eines Kunden ist.
  * Die Anzahl der geschützten Server, die zurzeit in Rechnung gestellt werden, kann jederzeit auf der Seite "Dashboard" des Azure Site Recovery-Tresors angezeigt werden.
* Unterstützung für die vSphere-Befehlszeilenschnittstelle (vCLI) 5.5 Update 2 wurde hinzugefügt.
* Unterstützung für Linux-Betriebssysteme auf dem Quellserver hinzugefügt:
  * RHEL 6 Update 6
  * RHEL 5 Update 11
  * CentOS 6 Update 6
  * CentOS 5 Update 11
* Fehlerbehebungen für folgende Probleme:
  * Fehler der Tresorregistrierung für den Konfigurationsserver oder RX-Server.
  * Clustervolumes werden nicht wie erwartet angezeigt, wenn gruppierte virtuelle Computer beim Fortsetzen erneut geschützt werden.
  * Failbackfehler, wenn der Masterzielserver auf einem anderen ESXi-Server als den lokalen virtuellen Produktionscomputern gehostet wird.
  * Die Konfigurationsdateiberechtigungen werden geändert, wenn Sie das Upgrade auf 8.0.1 durchführen. Dies wirkt sich auf den Schutz und die Vorgänge aus.
  * Der Schwellenwert für die Neusynchronisierung wird nicht wie erwartet erzwungen, was zu inkonsistentem Replikationsverhalten führt.
  * Die RPO-Einstellungen werden nicht richtig in der Konfigurationsserverschnittstelle angezeigt. Für den unkomprimierten Datenwert wird fälschlicherweise der komprimierte Wert angezeigt.
  * Der Vorgang „Entfernen“ führt das Löschen im vContinuum-Assistenten nicht wie erwartet durch, und die Replikation wird in der Konfigurationsserverschnittstelle nicht gelöscht.
  * Im vContinuum-Assistenten wird der Datenträger automatisch deaktiviert, wenn Sie in der Datenträgeransicht während des Schutzes von virtuellen MSCS-Computern auf **Details** klicken.
  * Beim Szenario „physisch zu virtuell“ (P2V) werden erforderliche HP-Dienste, z.B. CIMnotify und CqMgHost, bei der VM-Wiederherstellung nicht auf manuell umgestellt. Dies führt dazu, dass sich die Startdauer verlängert.
  * Für den Schutz eines virtuellen Linux-Computers tritt ein Fehler auf, wenn auf dem Masterzielserver mehr als 26 Datenträger vorhanden sind.

## <a name="next-steps"></a>Nächste Schritte
Etwaige Fragen können Sie im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)stellen.



<!--HONumber=Nov16_HO3-->


