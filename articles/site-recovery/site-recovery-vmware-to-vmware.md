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
	ms.date="06/14/2016"
	ms.author="nisoneji"/>


# Replizieren von lokalen virtuellen VMware-Maschinen oder physischen Servern an einen sekundären Standort


## Übersicht

InMage Scout in Azure Site Recovery ermöglicht eine Echtzeitreplikation zwischen lokalen VMware-Standorten. InMage Scout ist Teil der Abonnements des Azure Site Recovery-Diensts.


## Voraussetzungen

- **Azure-Konto**: Sie benötigen ein [Microsoft Azure](https://azure.microsoft.com/)-Konto. Für den Einstieg steht ein [kostenloses Testkonto](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung. [Erfahren Sie mehr](https://azure.microsoft.com/pricing/details/site-recovery/) über die Preise für Site Recovery.


## Schritt 1: Erstellen eines Tresors

1. Melden Sie sich im [Verwaltungsportal](https://portal.azure.com) an.
2. Klicken Sie auf **Data Services** > **Recovery Services** > **Site Recovery-Tresor**.
3. Klicken Sie auf **Neu erstellen** > **Schnellerfassung**.
4. Geben Sie unter **Name** einen aussagekräftigen Anzeigenamen für den Tresor ein.
5. Wählen Sie unter **Region** die geografische Region für den Tresor aus. Eine Liste mit den unterstützten Regionen finden Sie in den [Preisdetails zu Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) unter „Geografische Verfügbarkeit“.

Überprüfen Sie auf der Statusleiste, ob der Tresor erfolgreich erstellt wurde. Der Tresor wird auf der Hauptseite von Recovery Services als **aktiv** aufgelistet.

## Schritt 2: Konfigurieren des Tresors und Herunterladen von InMage Scout-Komponenten

1. Klicken Sie auf **Tresor erstellen**.
2. Klicken Sie auf der Seite **Recovery Services** auf den Tresor, um die Seite „Schnellstart“ zu öffnen.
3. Wählen Sie in der Dropdownliste die Option **Zwischen zwei lokalen VMware-Standorten** aus.
4. Laden Sie InMage Scout herunter. Die Setupdateien für alle erforderlichen Komponenten befinden sich in der heruntergeladenen ZIP-Datei.


## Schritt 3: Installieren von Komponentenupdates

0Erfahren Sie mehr über die neuesten[Updates](#updates). Installieren Sie die Updatedateien in der folgenden Reihenfolge:

1. RX-Server, falls vorhanden
2. Konfigurationsserver
3. Prozessserver
3. Masterzielserver.
4. vContinuum-Server.
5. Quellserver (nur für Windows-Server)

Installieren Sie wie folgt:

1. Laden Sie die ZIP-Datei für das [Update](https://aka.ms/asr-scout-update3) herunter. Diese ZIP-Datei enthält die folgenden Dateien:

	-  RX\_8.0.3.0\_GA\_Update\_3\_6684045\_17Mar16.tar.gz
	-  CX\_Windows\_8.0.3.0\_GA\_Update\_3\_5048668\_16Mar16.exe
	-  UA\_Windows\_8.0.3.0\_GA\_Update\_3\_7101745\_04Apr16.exe
	-  UA\_RHEL6-64\_8.0.3.0\_GA\_Update\_3\_7101745\_04Apr16.zip
	-  vCon\_Windows\_8.0.3.0\_GA\_Update\_3\_6873369\_16Mar16.exe

2. Extrahieren Sie die ZIP-Dateien.
3. **RX-Server**: Kopieren Sie **RX\_8.0.3.0\_GA\_Update\_3\_6684045\_17Mar16.tar.gz** auf den RX-Server, und extrahieren Sie die Datei. Führen Sie im extrahierten Ordner **/Install** aus.
4. **Konfigurationsserver/Prozessserver**: Kopieren Sie die Datei **CX\_Windows\_8.0.3.0\_GA\_Update\_3\_5048668\_16Mar16.exe** auf den Konfigurationsserver und Prozessserver. Führen Sie sie durch Doppelklicken aus.
5. **Windows-Masterzielserver**: Um die Kopie des vereinheitlichten Agents zu aktualisieren, kopieren Sie die Datei **UA\_Windows\_8.0.3.0\_GA\_Update\_3\_7101745\_04Apr16.exe** auf den Masterzielserver. Führen Sie sie durch Doppelklicken aus. Beachten Sie, dass der vereinheitlichte Agent auch für den Quellserver anwendbar ist. Er sollte wie unten erwähnt auch auf dem Quellserver installiert werden.
6. **Linux-Masterzielserver**: Um den vereinheitlichten Agent zu aktualisieren, kopieren Sie **UA\_RHEL6-64\_8.0.3.0\_GA\_Update\_3\_7101745\_04Apr16.zip** auf den Masterzielserver und extrahieren die Datei. Führen Sie im extrahierten Ordner **/Install** aus.
7. **vContinuum-Server**: Kopieren Sie **vCon\_Windows\_8.0.3.0\_GA\_Update\_3\_6873369\_16Mar16.exe** auf den vContinuum-Server. Stellen Sie sicher, dass Sie den vContinuum Assistenten geschlossen haben. Doppelklicken Sie auf die Datei, um Sie auszuführen.
8. **Windows-Quellserver**: Um den vereinheitlichten Agent zu aktualisieren, kopieren Sie die Datei **UA\_Windows\_8.0.3.0\_GA\_Update\_3\_7101745\_04Apr16.exe** auf den Quellserver. Führen Sie sie durch Doppelklicken aus. 

## Schritt 4: Einrichten der Replikation
1. Richten Sie die Replikation zwischen den Quell- und Zielstandorten von VMware ein.
2. Anleitungen finden Sie in der InMage Scout-Dokumentation, die zusammen mit dem Produkt heruntergeladen wird. Alternativ können Sie wie folgt auf die Dokumentation zugreifen:

	- [Versionshinweise](https://aka.ms/asr-scout-release-notes)
	- [Kompatibilitätsmatrix](https://aka.ms/asr-scout-cm)
	- [Benutzerhandbuch](https://aka.ms/asr-scout-user-guide)
	- [RX-Benutzerhandbuch](https://aka.ms/asr-scout-rx-user-guide)
	- [Schnellinstallationsanleitung](https://aka.ms/asr-scout-quick-install-guide)


## Aktualisierungen

### ASR Scout 8.0.1 Update 3
Update 3 enthält die folgenden Fehlerbehebungen und Verbesserungen:

1. Der Konfigurationsserver und RX konnten im ASR-Tresor nicht registriert werden, wenn sie sich hinter dem Proxy befanden.
2. Die Stundenanzahl für RPO-Nichterfüllung wird im Integritätsbericht nicht aktualisiert.
3. Konfigurationsserver wird nicht mit RX synchronisiert, wenn ESX-Hardwaredetails oder Netzwerkdetails ein UTF-8-Zeichen enthalten.
4. Windows 2008 Server R2 DC-Computer werden nach der Wiederherstellung nicht neu gestartet.
5. Die Offlinesynchronisierung funktioniert nicht wie erwartet. 
6. Nach dem VM-Failover hängt die Replikationspaarlöschung in der CX-UI über längere Zeit, und der Benutzer kann den Failback-Fortsetzungsvorgang nicht durchführen.
7. Optimierte Momentaufnahmen-Gesamtvorgänge, die vom Konsistenzauftrag zur Reduzierung von Anwendungstrennungen durchgeführt werden, z.B. SQL-Clients.
8. Verbesserte VACP-Leistung durch eine Reduzierung der Speichernutzung, die zum Erstellen von Momentaufnahmen unter Windows erforderlich ist.
9. Der Pushinstallationsdienst stürzt ab, wenn das Kennwort länger als 16 Zeichen ist
10. Beim Ändern der Anmeldeinformationen führt vContinuum keine Überprüfung und Aufforderung zur Eingabe neuer vCenter-Anmeldeinformationen durch.
11. Auf dem Linux-Masterziel-Cache-Manager (cachemgr) werden keine Dateien vom Prozessserver heruntergeladen, was zu einer Drosselung des Replikationspaars führt.
12. Wenn die Reihenfolge der physischen MSCS-Clusterdatenträger nicht auf allen Knoten gleich ist, wird die Replikation für einige Clustervolumes nicht festgelegt. <br/>HINWEIS: Bei dieser Behebung muss der Schutz des Clusters erneuert werden.  
13. SMTP-Funktionalität verhält sich nicht erwartungsgemäß, nachdem RX von Scout 7.1 auf Scout 8.0.1 aktualisiert wurde.
14. Dem Protokoll wurden weitere Statistiken hinzugefügt, damit der Rollbackvorgang nachverfolgt, wie viel Zeit bis zum Abschluss benötigt wird.
15. Unterstützung für Linux-Betriebssysteme auf dem Quellserver hinzugefügt 
	- RHEL 6 Update 7
	- CentOS 6 Update 7 
16. CX- und RX-Benutzeroberfläche kann jetzt die Benachrichtigung für das Paar anzeigen, das in den Bitmap-Modus wechselt.
17. Die folgenden Sicherheitsfixes werden in RX hinzugefügt.

**#**|**Problembeschreibung**|**Implementierungsverfahren**
---|---|---
1\. |Autorisierungsumgehung per Parametermanipulation|Zugriff ist auf unzulässige Benutzer beschränkt
2\. |Websiteübergreifende Anforderungsfälschung|Seitentokenkonzept mit zufälliger Generierung für jede Seite implementiert. <br/>Hierbei ist Folgendes zu beobachten: <br/>1) Nur eine Anmeldeinstanz für einen Benutzer.<br/>2) Seitenaktualisierung funktioniert nicht, und es erfolgt eine Umleitung zum Dashboard. <br/>
3\. |Upload schädlicher Dateien|Dateien wurden auf bestimmte Erweiterungen beschränkt. Zulässig sind: 7z, aiff, asf, avi, bmp, csv, doc, docx, fla, flv, gif, gz, gzip, jpeg, jpg, log, mid, mov, mp3, mp4, mpc, mpeg, mpg, ods, odt, pdf, png, ppt, pptx, pxd, qt, ram, rar, rm, rmi, rmvb, rtf, sdc, sitd, swf, sxc, sxw, tar, tgz, tif, tiff, txt, vsd, wav, wma, wmv, xls, xlsx, xml, zip
4\. | Persistentes websiteübergreifendes Skripting | Eingabevalidierungen hinzugefügt


>[AZURE.NOTE]
>
>-	Alle ASR-Updates sind kumulativ. Update 3 verfügt über alle Fixes von update1 und update2. Update 3 kann direkt auf 8.0.1 GA angewendet werden.
>-	Die Updates vom Typ CS und RX können nicht rückgängig gemacht werden, sobald sie auf das System angewendet wurden.

### ASR Scout 8.0.1 Update 03Dec15 (Update2)

Fixes in Update 2 umfassen Folgendes:

- **Konfigurationsserver**: Behebt ein Problem, das ein ordnungsgemäßes Funktionieren der 31 Tage kostenlosen Messfunktion verhinderte, wenn der Konfigurationsserver in Site Recovery registriert wurde.
- **Vereinheitlichter Agent**: Behebt ein Problem in Update 1 für das Masterziel, das bewirkte, dass das Update nicht auf dem Masterzielserver installiert wurde, wenn ein Upgrade von Version 8.0 auf 8.0.1 erfolgte.


### ASR Scout 8.0.1 Update 1

Update 1 umfasst Fehlerbehebungen und neue Features:

- 31 Tage lang kostenloser Schutz pro Serverinstanz. Dies ermöglicht es Ihnen, die Funktionalität zu testen oder eine Machbarkeitsstudie einzurichten.
	- Alle Vorgänge auf dem Server, einschließlich Failover und Failback sind für die ersten 31 Tage kostenlos, ab dem Zeitpunkt, zu dem ein Server erstmalig mit ASR Scout geschützt wird.
	- Ab dem 32. Tag wird für jeden geschützten Server die Standardinstanzgebühr für Azure Site Recovery-Schutz für eine Website in Rechnung gestellt, die im Besitz eines Kunden ist.
	- Die Anzahl der geschützten Server, die zurzeit in Rechnung gestellt werden, kann jederzeit auf der Seite "Dashboard" des Azure Site Recovery-Tresors angezeigt werden.
- Unterstützung für vCLI 5.5 Update 2 hinzugefügt.
- Unterstützung für Linux-Betriebssysteme auf dem Quellserver hinzugefügt:
	- RHEL 6 Update 6
	- RHEL 5 Update 11
	- CentOS 6 Update 6
	- CentOS 5 Update 11
- Fehlerbehebungen für folgende Probleme:
	- Fehler der Tresorregistrierung für den Konfigurationsserver oder RX-Server.
	- Clustervolumes werden nicht wie erwartet angezeigt, wenn gruppierte virtuelle Ccomputer während der Wiederaufnahme erneut geschützt werden.
	- Failbackfehler, wenn der Masterzielserver auf einem anderen ESXi-Server als den lokalen virtuellen Produktionscomputern gehostet wird.
	- Die Konfigurationsdateiberechtigungen ändern sich, wenn ein Upgrade auf 8.0.1 durchgeführt wird, was sich auf den Schutz und Vorgänge auswirkt.
	- Der Neusynchronisierungs-Schwellenwert wird nicht wie erwartet erzwungen, was zu inkonsistentem Replikationsverhalten führt.
	- RPO-Einstellungen werden nicht ordnungsgemäß in der Konfigurationsserverschnittstelle angezeigt. Der unkomprimierte Datenwert zeigt fälschlicherweise einen komprimierten Wert an.
	-  Der Vorgang "Entfernen" löscht nicht wie erwartet im vContinuum-Assistenten, und die Replikation wird nicht in der Konfigurationsserverschnittstelle gelöscht.
	-  Im vContinuum-Assistenten wird der Datenträger automatisch deaktiviert, wenn Sie in der Datenträgeransicht während des Schutzes von virtuellen MSCS-Computern auf **Details** klicken.
	- Während des P2V-Szenarios erforderliche HP-Dienste wie CIMnotify und CqMgHost werden im wiederhergestellten virtuellen Computer nicht in "Manuell" verschoben, was zu einem verlängerten Startvorgang führt.
	- Fehler eines geschützten virtuellen Linux-Computers, wenn auf dem Masterzielserver mehr als 26 Datenträger vorhanden sind.

## Nächste Schritte

Etwaige Fragen können Sie im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) stellen.

<!---HONumber=AcomDC_0615_2016-->