<properties
	pageTitle="Replizieren von lokalen virtuellen VMware-Maschinen oder physischen Servern an einen sekundären Standort | Microsoft Azure"
	description="Verwenden Sie diesen Artikel, wenn Sie VMware-VMs oder physische Windows-/Linux-Server mithilfe von Azure Site Recovery an einen sekundären Standort replizieren möchten."
	services="site-recovery"
	documentationCenter=""
	authors="nsoneji"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="site-recovery"
	ms.workload="backup-recovery"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/16/2016"
	ms.author="nisoneji"/>


# Replizieren von lokalen virtuellen VMware-Maschinen oder physischen Servern an einen sekundären Standort


## Übersicht

InMage Scout in Azure Site Recovery ermöglicht eine Echtzeitreplikation zwischen lokalen VMware-Standorten. InMage Scout ist Teil der Abonnements des Azure Site Recovery-Diensts.


## Voraussetzungen

**Azure-Konto**: Sie benötigen ein [Microsoft Azure](https://azure.microsoft.com/)-Konto. Für den Einstieg steht ein [kostenloses Testkonto](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung. Erfahren Sie mehr über die [Preise für Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).


## Schritt 1: Erstellen eines Tresors

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Data Services** > **Recovery Services** > **Site Recovery-Tresor**.
3. Klicken Sie auf **Neu erstellen** > **Schnellerfassung**.
4. Geben Sie unter **Name** einen Anzeigenamen ein, über den der Tresor identifiziert wird.
5. Wählen Sie unter **Region** die geografische Region für den Tresor aus. Informationen zu den unterstützten Regionen finden Sie unter [Site Recovery – Preise](https://azure.microsoft.com/pricing/details/site-recovery/).

Überprüfen Sie auf der Statusleiste, ob der Tresor erfolgreich erstellt wurde. Der Tresor wird auf der Hauptseite von **Recovery Services** als **Aktiv** angegeben.

## Schritt 2: Konfigurieren des Tresors und Herunterladen von InMage Scout-Komponenten

1. Klicken Sie auf **Tresor erstellen**.
2. Klicken Sie auf der Seite **Recovery Services** auf den Tresor, um die Seite **Schnellstart** zu öffnen.
3. Wählen Sie in der Dropdownliste die Option **Zwischen zwei lokalen VMware-Standorten** aus.
4. Laden Sie InMage Scout herunter. Die Setupdateien für alle erforderlichen Komponenten befinden sich in der heruntergeladenen ZIP-Datei.


## Schritt 3: Installieren von Komponentenupdates

0Erfahren Sie mehr über die neuesten[Updates](#updates). Installieren Sie die Updatedateien in der folgenden Reihenfolge auf Servern:

1. RX-Server, falls vorhanden
2. Konfigurationsserver
3. Prozessserver
3. Masterzielserver
4. vContinuum-Server
5. Quellserver (nur für Windows-Server)

Installieren Sie die Updates wie folgt:

1. Laden Sie die ZIP-Datei für das [Update](https://aka.ms/asr-scout-update3) herunter. Diese ZIP-Datei enthält die folgenden Dateien:

	-  RX_8.0.3.0_GA\_Update_3_6684045\_17Mar16.tar.gz
	-  CX\_Windows_8.0.3.0_GA\_Update_3_5048668\_16Mar16.exe
	-  UA\_Windows_8.0.3.0_GA\_Update_3_7101745\_04Apr16.exe
	-  UA\_RHEL6-64_8.0.3.0_GA\_Update_3_7101745\_04Apr16.zip
	-  vCon\_Windows_8.0.3.0_GA\_Update_3_6873369\_16Mar16.exe

2. Extrahieren Sie die ZIP-Dateien.
3. **Für den RX-Server**: Kopieren Sie **RX_8.0.3.0_GA\_Update_3_6684045\_17Mar16.tar.gz** auf den RX-Server, und extrahieren Sie die Datei. Führen Sie im extrahierten Ordner **/Install** aus.
4. **Für den Konfigurationsserver/Prozessserver**: Kopieren Sie die Datei **CX\_Windows_8.0.3.0_GA\_Update_3_5048668\_16Mar16.exe** auf den Konfigurationsserver und Prozessserver. Doppelklicken Sie auf die Datei, um sie auszuführen.
5. **Für den Windows-Masterzielserver**: Um die Kopie des vereinheitlichten Agents zu aktualisieren, kopieren Sie die Datei **UA\_Windows_8.0.3.0_GA\_Update_3_7101745\_04Apr16.exe** auf den Masterzielserver. Doppelklicken Sie auf die Datei, um sie auszuführen. Beachten Sie, dass der vereinheitlichte Agent auch für den Quellserver anwendbar ist. Sie sollten ihn auch auf dem Quellserver installieren. Dies ist weiter unten in dieser Liste beschrieben.
6. **Für den Linux-Masterzielserver**: Um den vereinheitlichten Agent zu aktualisieren, kopieren Sie die Datei **UA\_RHEL6-64_8.0.3.0_GA\_Update_3_7101745\_04Apr16.zip** auf den Masterzielserver und extrahieren die Datei. Führen Sie im extrahierten Ordner **/Install** aus.
7. **Für den vContinuum-Server**: Kopieren Sie die Datei **vCon\_Windows_8.0.3.0_GA\_Update_3_6873369\_16Mar16.exe** auf den vContinuum-Server. Stellen Sie sicher, dass Sie den vContinuum-Assistenten geschlossen haben. Doppelklicken Sie auf die Datei, um sie auszuführen.
8. **Für den Windows-Quellserver**: Um den vereinheitlichten Agent zu aktualisieren, kopieren Sie die Datei **UA\_Windows_8.0.3.0_GA\_Update_3_7101745\_04Apr16.exe** auf den Quellserver. Doppelklicken Sie auf die Datei, um sie auszuführen.

## Schritt 4: Einrichten der Replikation
1. Richten Sie die Replikation zwischen den Quell- und Zielstandorten von VMware ein.
2. Anleitungen finden Sie in der InMage Scout-Dokumentation, die zusammen mit dem Produkt heruntergeladen wird. Alternativ hierzu können Sie wie folgt auf die Dokumentation zugreifen:

	- [Versionshinweise](https://aka.ms/asr-scout-release-notes)
	- [Kompatibilitätsmatrix](https://aka.ms/asr-scout-cm)
	- [Benutzerhandbuch](https://aka.ms/asr-scout-user-guide)
	- [RX-Benutzerhandbuch](https://aka.ms/asr-scout-rx-user-guide)
	- [Schnellinstallationsanleitung](https://aka.ms/asr-scout-quick-install-guide)


## Aktualisierungen

### Azure Site Recovery Scout 8.0.1 Update 3
Update 3 enthält die folgenden Fehlerbehebungen und Verbesserungen:

- Der Konfigurationsserver und RX können nicht im Site Recovery-Tresor registriert werden, wenn sie sich hinter dem Proxy befinden.
- Die Anzahl von Stunden, in denen die Recovery Point Objective (RPO) nicht erfüllt ist, wird im Integritätsbericht nicht aktualisiert.
- Der Konfigurationsserver wird nicht mit RX synchronisiert, wenn die ESX-Hardwaredetails oder Netzwerkdetails ein UTF-8-Zeichen enthalten.
- Windows Server 2008 R2-Domänencontroller werden nach der Wiederherstellung nicht gestartet.
- Die Offlinesynchronisierung funktioniert nicht wie erwartet.
- Nach dem VM-Failover bleibt die Replikationspaarlöschung in der CX-UI längere Zeit hängen, und Benutzer können den Failback- oder Fortsetzungsvorgang nicht durchführen.
- Momentaufnahmen-Gesamtvorgänge, die vom Konsistenzauftrag durchgeführt werden, wurden optimiert, um Trennungen von Anwendungsverbindungen zu reduzieren, z.B. für SQL-Clients.
- Die Leistung des Konsistenztools (VACP.exe) wurde verbessert, indem die Arbeitsspeichernutzung reduziert wurde, die zum Erstellen von Momentaufnahmen unter Windows erforderlich ist.
- Der Dienst für die Pushinstallation stürzt ab, wenn das Kennwort länger als 16 Zeichen ist.
- Beim Ändern der Anmeldeinformationen führt vContinuum keine Überprüfung und Aufforderung zur Eingabe neuer vCenter-Anmeldeinformationen durch.
- Unter Linux lädt der Masterziel-Cache-Manager (cachemgr) keine Dateien vom Prozessserver herunter, was zu einer Drosselung des Replikationspaars führt.
- Wenn die Datenträgerreihenfolge des physischen Failoverclusters (MSCS) nicht auf allen Knoten gleich ist, wird die Replikation für einige Clustervolumen nicht festgelegt. <br/>Beachten Sie, dass der Cluster erneut geschützt werden muss, damit Sie von diesem Fix profitieren können.
- SMTP-Funktionalität verhält sich nicht erwartungsgemäß, nachdem RX von Scout 7.1 auf Scout 8.0.1 aktualisiert wurde.
- Dem Protokoll wurden weitere Statistiken hinzugefügt, damit der Rollbackvorgang nachverfolgt, wie viel Zeit bis zum Abschluss benötigt wird.
- Unterstützung für Linux-Betriebssysteme auf dem Quellserver wurde hinzugefügt:
	- Red Hat Enterprise Linux (RHEL) 6 Update 7
	- CentOS 6 Update 7
- Die CX- und RX-Benutzeroberfläche kann jetzt die Benachrichtigung für das Paar anzeigen, das in den Bitmapmodus wechselt.
- Die folgenden Sicherheitsfixes wurden in RX hinzugefügt:

**Problembeschreibung**|**Implementierungsverfahren**
---|---
Autorisierungsumgehung per Parametermanipulation|Der Zugriff auf unzulässige Benutzer wurde beschränkt.
Websiteübergreifende Anforderungsfälschung|Das Seitentokenkonzept mit zufälliger Generierung für jede Seite wurde implementiert. <br/>Dies bedeutet Folgendes: <li> Für einen Benutzer ist nur eine Instanz für das einmalige Anmelden vorhanden.</li><li>Die Seitenaktualisierung funktioniert nicht, sondern es wird eine Umleitung auf das Dashboard durchgeführt.</li>
Upload schädlicher Dateien|Dateien wurden auf bestimmte Erweiterungen beschränkt. Zulässige Erweiterungen: 7z, aiff, asf, avi, bmp, csv, doc, docx, fla, flv, gif, gz, gzip, jpeg, jpg, log, mid, mov, mp3, mp4, mpc, mpeg, mpg, ods, odt, pdf, png, ppt, pptx, pxd, qt, ram, rar, rm, rmi, rmvb, rtf, sdc, sitd, swf, sxc, sxw, tar, tgz, tif, tiff, txt, vsd, wav, wma, wmv, xls, xlsx, xml und zip.
Persistentes websiteübergreifendes Skripting | Eingabevalidierungen wurden hinzugefügt.


>[AZURE.NOTE]
>
>-	Alle Site Recovery-Updates sind kumulativ. Update 3 verfügt über alle Fixes von Update 1 und Update 2. Update 3 kann direkt auf 8.0.1 GA angewendet werden.
>-	Für die Updates für den Konfigurationsserver und RX kann kein Rollback durchgeführt werden, nachdem sie auf das System angewendet wurden.

### Azure Site Recovery Scout 8.0.1 Update 2 (Update 03Dec15)

Fixes in Update 2 umfassen Folgendes:

- **Konfigurationsserver**: Behebung eines Problems, das die korrekte Funktion der kostenlosen 31-Tage-Messfunktion verhinderte, wenn der Konfigurationsserver in Site Recovery registriert wurde.
- **Vereinheitlichter Agent**: Behebung eines Problems in Update 1, das die Installation des Updates auf dem Masterzielserver verhinderte, wenn ein Upgrade von Version 8.0 auf 8.0.1 durchgeführt wurde.


### Azure Site Recovery Scout 8.0.1 Update 1

Update 1 enthält die folgenden Fehlerbehebungen und neuen Features:

- 31 Tage lang kostenloser Schutz pro Serverinstanz. Dies ermöglicht es Ihnen, die Funktionalität zu testen oder eine Machbarkeitsstudie einzurichten.
	- Alle Vorgänge auf dem Server, einschließlich Failover und Failback, sind für die ersten 31 Tage kostenlos. Dies gilt ab dem Zeitpunkt, zu dem ein Server erstmalig mit Site Recovery Scout geschützt wird.
	- Ab dem 32. Tag wird für jeden geschützten Server die Standardinstanzgebühr für den Azure Site Recovery-Schutz für eine Website in Rechnung gestellt, die im Besitz eines Kunden ist.
	- Die Anzahl der geschützten Server, die zurzeit in Rechnung gestellt werden, kann jederzeit auf der Seite "Dashboard" des Azure Site Recovery-Tresors angezeigt werden.
- Unterstützung für die vSphere-Befehlszeilenschnittstelle (vCLI) 5.5 Update 2 wurde hinzugefügt.
- Unterstützung für Linux-Betriebssysteme auf dem Quellserver hinzugefügt:
	- RHEL 6 Update 6
	- RHEL 5 Update 11
	- CentOS 6 Update 6
	- CentOS 5 Update 11
- Fehlerbehebungen für folgende Probleme:
	- Fehler der Tresorregistrierung für den Konfigurationsserver oder RX-Server.
	- Clustervolumes werden nicht wie erwartet angezeigt, wenn gruppierte virtuelle Computer beim Fortsetzen erneut geschützt werden.
	- Failbackfehler, wenn der Masterzielserver auf einem anderen ESXi-Server als den lokalen virtuellen Produktionscomputern gehostet wird.
	- Die Konfigurationsdateiberechtigungen werden geändert, wenn Sie das Upgrade auf 8.0.1 durchführen. Dies wirkt sich auf den Schutz und die Vorgänge aus.
	- Der Schwellenwert für die Neusynchronisierung wird nicht wie erwartet erzwungen, was zu inkonsistentem Replikationsverhalten führt.
	- Die RPO-Einstellungen werden nicht richtig in der Konfigurationsserverschnittstelle angezeigt. Für den unkomprimierten Datenwert wird fälschlicherweise der komprimierte Wert angezeigt.
	-  Der Vorgang „Entfernen“ führt das Löschen im vContinuum-Assistenten nicht wie erwartet durch, und die Replikation wird in der Konfigurationsserverschnittstelle nicht gelöscht.
	-  Im vContinuum-Assistenten wird der Datenträger automatisch deaktiviert, wenn Sie in der Datenträgeransicht während des Schutzes von virtuellen MSCS-Computern auf **Details** klicken.
	- Beim Szenario „physisch zu virtuell“ (P2V) werden erforderliche HP-Dienste, z.B. CIMnotify und CqMgHost, bei der VM-Wiederherstellung nicht auf manuell umgestellt. Dies führt dazu, dass sich die Startdauer verlängert.
	- Für den Schutz eines virtuellen Linux-Computers tritt ein Fehler auf, wenn auf dem Masterzielserver mehr als 26 Datenträger vorhanden sind.

## Nächste Schritte

Etwaige Fragen können Sie im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) stellen.

<!---HONumber=AcomDC_0921_2016-->