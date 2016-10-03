<properties
   pageTitle="Schnellstartanleitung für die manuelle Installation von SAP HANA auf Azure-VMs | Microsoft Azure"
   description="Schnellstartanleitung für die manuelle Installation von SAP HANA auf Azure-VMs"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# Schnellstartanleitung für die manuelle Installation von SAP HANA (Einzelinstanz) auf Azure-VMs

## Einführung

In dieser Schnellstartanleitung wird beschrieben, wie Sie eine Einzelinstanz eines SAP HANA-Prototyp-/Demosystems auf Azure-VMs per manueller Installation von SAP NetWeaver 7.5 und SAP HANA SP12 einrichten. In der Anleitung wird vorausgesetzt, dass der Leser mit den Grundlagen von Azure IaaS vertraut ist, z.B. mit dem Bereitstellen von virtuellen Computern oder virtuellen Netzwerken entweder über das Azure-Portal oder PowerShell/CLI, einschließlich der optionalen Verwendung von JSON-Vorlagen. Darüber hinaus wird davon ausgegangen, dass der Leser mit SAP HANA, SAP NetWeaver und der Vorgehensweise bei der lokalen Installation vertraut ist.

Es wird vorausgesetzt, dass der Leser die allgemeine SAP-Azure-Dokumentation kennt, auf die im Abschnitt mit den allgemeinen Informationen am Ende des Artikels hingewiesen wird.

Aufgrund der Beschränkung auf Nicht-Produktionssysteme wird in dieser Anleitung nicht auf Themen wie hohe Verfügbarkeit, Sicherung, Notfallwiederherstellung, hohe Leistung oder Sicherheitsaspekte eingegangen.

Die Beispieleinrichtung wurde mit zwei virtuellen Computern durchgeführt, um eine verteilte SAP NetWeaver-Installation über das Azure Resource Manager-Modell zu erhalten, da SAP-Linux-Azure nur unter Azure Resource Manager und nicht für das klassische Modell unterstützt wird. Links zu weiteren Informationen zu Azure Resource Manager finden Sie im Abschnitt mit den allgemeinen Informationen am Ende dieses Artikels.

Diese beiden Test-VMs wurden für die Beispielinstallation verwendet:

* hana-appsrv (Typ DS3) zum Hosten von NW 7.5 ASCS-Instanz und PAS
* hana-dbsrv (Typ GS4) zum Hosten von HANA SP12
* Beide virtuellen Computer gehörten zu einem virtuellen Azure-Netzwerk (azure-hana-test-vnet)
* In beiden Fällen war das Betriebssystem SLES 12 SP1


Beachten Sie aber, dass ab Juli 2016 SAP HANA nur für OLAP-Produktionssysteme (BW) unter Azure-VMs vom Typ GS5 vollständig unterstützt wird. Zu Testzwecken, bei denen kein offizieller SAP-Support erwartet wird, sind auch kleinere Lösungen ausreichend, z.B. GS4. Für SAP HANA unter Azure sollte immer Azure Storage Premium für HANA-Daten und -Protokolldateien verwendet werden. Informationen hierzu finden Sie weiter unten im Abschnitt „Datenträgereinrichtung“. Weitere Informationen dazu, welche SAP-Produkte unter Azure unterstützt werden, finden Sie im Abschnitt mit den allgemeinen Informationen am Ende dieses Artikels.


In der Anleitung werden zwei unterschiedliche Möglichkeiten zum manuellen Installieren von SAP HANA auf Azure-VMs beschrieben:

* Installieren von SAP HANA über SAP Software Provisioning Manager (SWPM) im Rahmen einer verteilten NetWeaver-Installation im Schritt „Datenbankinstanz“
* Installieren von SAP HANA mit dem HANA Life Cycle Manager-Tool „hdblcm“ und anschließendes Installieren von NetWeaver

Es ist auch möglich, SWPM zu verwenden und alle Komponenten (SAP HANA, SAP-Anwendungsserver, ASCS-Instanz, SAP GUI) auf einer einzelnen VM zu installieren. Diese Option ist in diesem Artikel nicht beschrieben, aber die zu berücksichtigenden Elemente sind identisch.

Lesen Sie sich vor dem Starten einer Installation den Abschnitt weiter unten nach den Prüflisten durch, in dem die Einrichtung der Azure-Test-VMs beschrieben wird. So können Sie mehrere einfache Fehler vermeiden, die passieren können, wenn Sie nur eine Azure-VM-Standardkonfiguration verwenden.


## Prüfliste für SAP HANA-Installation per SAP SWPM

Dies ist eine einfache Prüfliste mit den wichtigsten Elementen der manuellen Installation einer SAP HANA-Einzelinstanz zu Demo- oder Prototypzwecken mit SAP SWPM und einer verteilten SAP NW 7.5-Installation. Die einzelnen Elemente werden im Verlauf des Artikels beschrieben und in Form von Screenshots ausführlicher veranschaulicht:

* Erstellen eines virtuellen Azure-Netzwerks, das die beiden Test-VMs enthält
* Bereitstellen von zwei Azure-VMs mit OS SLES 12 SP1 über das Azure Resource Manager-Modell
* Anfügen von zwei Standarddatenträgern für die Speicherung auf der App-Server-VM (z.B. 75 GB und 500 GB)
* Anfügen von vier Datenträgern an die HANA DB-Server-VM – zwei Standarddatenträger für die Speicherung, z.B. App-Server-VM und zwei Datenträger für Storage Premium (z.B. 2 x 512 GB)
* Anfügen von mehreren Datenträgern und Erstellen von Stripesetvolumes per „lvm“ oder „mdadm“ auf Betriebssystemebene in der VM, je nach den Anforderungen an Größe und/oder Durchsatz
* Erstellen von XFS-Dateisystemen auf den angefügten Datenträgern/logischen Volumes
* Bereitstellen der neuen XFS-Dateisysteme auf Betriebssystemebene. Verwenden Sie ein Dateisystem, um die gesamte SAP-Software vorzuhalten, und das andere beispielsweise für das sapmnt-Verzeichnis und ggf. Sicherungen. Stellen Sie auf dem SAP HANA DB-Server die XFS-Dateisysteme auf den Storage Premium-Datenträgern als „/hana“ und „/usr/sap“ bereit. Dies ist alles erforderlich, um zu vermeiden, dass die Kapazitätsgrenze des Stammdateisystems erreicht wird, da es auf Linux-Azure-VMs nicht sehr groß ist.
* Eingeben der lokalen IP-Adressen der Test-VMs in „/etc/hosts“
* Eingeben des Parameters „nofail“ in „/etc/fstab“
* Festlegen von Kernelparametern gemäß HANA-SLES-12-SAP-Hinweis (Details weiter unten im Abschnitt zu Kernelparametern)
* Hinzufügen des Auslagerungsbereichs
* Falls gewünscht: Installieren eines grafischen Desktops auf den Test-VMs. Andernfalls Verwendung einer sapinst-Remoteinstallation.
* Herunterladen der SAP-Software vom SAP Service Marketplace
* Installieren der SAP ASCS-Instanz auf der App-Server-VM
* Freigeben des sapmnt-Verzeichnisses über NFS für die Test-VMs (App-Server-VM ist der NFS-Server)
* Installieren der Datenbankinstanz, einschließlich HANA, über SWPM auf der DB-Server-VM
* Installieren des PAS auf der App-Server-VM
* Starten der SAP MC und Herstellen der Verbindung, z.B. über SAP GUI/HANA Studio



## Prüfliste für SAP HANA-Installation per „hdblcm“

Dies ist eine einfache Prüfliste mit den wichtigsten Elementen der manuellen Installation einer SAP HANA-Einzelinstanz zu Demo- oder Prototypzwecken mit SAP SWPM und einer verteilten SAP NW 7.5-Installation. Die einzelnen Elemente werden im Verlauf des Artikels beschrieben und in Form von Screenshots ausführlicher veranschaulicht:

* Erstellen eines virtuellen Azure-Netzwerks, das die beiden Test-VMs enthält
* Bereitstellen von zwei Azure-VMs mit OS SLES 12 SP1 über das Azure Resource Manager-Modell
* Anfügen von zwei Standarddatenträgern für die Speicherung auf der App-Server-VM (z.B. 75 GB und 500 GB)
* Anfügen von vier Datenträgern an die HANA DB-Server-VM – zwei Standardspeicher, z.B. App-Server-VM und zwei Datenträger für Storage Premium (z.B. 2 x 512 GB)
* Anfügen von mehreren Datenträgern und Erstellen von Stripesetvolumes per „lvm“ oder „mdadm“ auf Betriebssystemebene in der VM, je nach den Anforderungen an Größe und/oder Durchsatz
* Erstellen von XFS-Dateisystemen auf den angefügten Datenträgern/logischen Volumes
* Bereitstellen der neuen XFS-Dateisysteme auf Betriebssystemebene. Verwenden Sie ein Dateisystem, um die gesamte SAP-Software vorzuhalten, und das andere beispielsweise für das sapmnt-Verzeichnis und ggf. Sicherungen. Stellen Sie auf dem SAP HANA DB-Server die XFS-Dateisysteme auf den Storage Premium-Datenträgern als „/hana“ und „/usr/sap“ bereit. Dies ist alles erforderlich, um zu vermeiden, dass die Kapazitätsgrenze des Stammdateisystems erreicht wird, da es auf Linux-Azure-VMs nicht sehr groß ist.
* Eingeben der lokalen IP-Adressen der Test-VMs in „/etc/hosts“
* Eingeben des Parameters „nofail“ in „/etc/fstab“
* Festlegen von Kernelparametern gemäß HANA-SLES-12-SAP-Hinweis (Details weiter unten im Abschnitt zu Kernelparametern)
* Hinzufügen des Auslagerungsbereichs
* Falls gewünscht: Installieren eines grafischen Desktops auf den Test-VMs. Andernfalls Verwendung einer sapinst-Remoteinstallation.
* Herunterladen der SAP-Software vom SAP Service Marketplace
* Erstellen der Gruppe „sapsys“ mit der Gruppen-ID 1001 auf der HANA DB-Server-VM
* Installieren von SAP HANA auf der DB-Server-VM per „hdblcm“
* Installieren der SAP ASCS-Instanz auf der App-Server-VM
* Freigeben des sapmnt-Verzeichnisses über NFS für die Test-VMs (App-Server-VM ist der NFS-Server)
* Installieren der Datenbankinstanz, einschließlich HANA, über SWPM auf der DB-Server-VM
* Installieren des PAS auf der App-Server-VM
* Starten der SAP MC und Herstellen der Verbindung, z.B. über SAP GUI/HANA Studio




## Vorbereiten der Azure-VMs für die manuelle Installation von SAP HANA

Dieses Kapitel zur Vorbereitung der Azure-VMs für die manuelle Installation von SAP HANA besteht aus fünf Abschnitten, in denen Folgendes behandelt wird:

* Datenträgereinrichtung
* Kernelparameter
* Dateisysteme
* /etc/hosts
* /etc/fstab


### Datenträgereinrichtung

Das Stammdateisystem einer Linux-VM unter Azure hat eine begrenzte Größe. Daher ist es erforderlich, an eine VM zur Ausführung von SAP zusätzlichen Datenträgerspeicherplatz anzufügen. Bei einer SAP-App-Server-VM, die in einer reinen Prototyp-/Demoumgebung verwendet wird, können Azure-Standarddatenträger für die Speicherung verwendet werden. Für die SAP HANA DB-Daten und -Protokolldateien sollten stattdessen Azure Storage Premium-Datenträger eingesetzt werden. Dies gilt auch für Umgebungen, die nicht für die Produktion bestimmt sind.

Einige Details dazu, wie Sie Datenträger an eine Linux-VM anfügen, finden Sie [hier](virtual-machines-linux-add-disk.md).

In Bezug auf die Azure-Datenträgerzwischenspeicherung muss „None“ (Keine) für Datenträger verwendet werden, die zum Speichern der HANA-Transaktionsprotokolle genutzt werden. Für HANA-Datendateien kann das Zwischenspeichern von Lesevorgängen verwendet werden. Da es sich bei HANA um eine speicherinterne Datenbank handelt, hängt es vom übergeordneten Nutzungsmuster ab, wie stark der Lesecache auf Azure-Datenträgerebene die Leistung verbessert (z.B. Starten von HANA und Lesen von Daten vom Datenträger in den Arbeitsspeicher).

Details zu Azure Storage Premium finden Sie [hier](../storage/storage-premium-storage.md).

[Hier](https://github.com/Azure/azure-quickstart-templates) finden Sie JSON-Beispielvorlagen zum Erstellen von VMs. Unter „101-vm-simple-linux“ wird veranschaulicht, wie eine einfache Vorlage aussieht, einschließlich des Speicherabschnitts, mit dem ein Datenträger mit 100 GB für Daten hinzugefügt wird.

[Dieser Artikel](virtual-machines-linux-sap-on-suse-quickstart.md) enthält Informationen dazu, wie Sie ein SUSE-Image über PowerShell oder CLI ermitteln und wie wichtig das Anfügen eines Datenträgers per UUID ist.


Je nach der Größe des Systems und den Durchsatzanforderungen kann es erforderlich sein, anstelle eines Datenträgers mehrere Datenträger anzufügen und später auf Betriebssystemebene ein Stripeset für diese Datenträger zu erstellen. Dies sind die beiden Gründe für die Erstellung eines Stripesets für mehrere Azure-Datenträger:

* Erhöhung des Durchsatzes
* Einzelnes Dateisystem > 1 TB erforderlich, da die aktuelle Größenbeschränkung für Azure-Datenträger 1 TB beträgt (Stand: Juli 2016)


Weitere Informationen zu den zwei wichtigsten Tools zum Konfigurieren des Stripings finden Sie hier:

[Artikel zur Verwendung von „mdadm“ zum Konfigurieren von Linux-Software-RAID auf einer Azure-VM](virtual-machines-linux-configure-raid.md)

[Artikel zum Konfigurieren des Logical Volume Manager auf einer Linux-Azure-VM](virtual-machines-linux-configure-lvm.md)



![](./media/virtual-machines-linux-sap-hana-get-started/image003.jpg)

In der Testumgebung wurden zwei Azure-Standarddatenträger für die Speicherung an die SAP-App-Server-VM angefügt. Einer wurde zum Speichern der gesamten SAP-Software für die Installation (z.B. NetWeaver 7.5, SAP GUI, SAP HANA, ...) verwendet, und der andere für etwaige Erfordernisse (z.B. Sicherung, Testdaten) sowie für die Freigabe des sapmnt-Verzeichnisses (z.B. SAP-Profile) für alle VMs, die derselben SAP-Umgebung angehören.

![](./media/virtual-machines-linux-sap-hana-get-started/image004.jpg)

Im Gegensatz zur App-Server-VM wurden an die SAP HANA-Server-VM vier Datenträger angefügt. Wie zuvor wurden zwei Datenträger verwendet, um die SAP-Software vorzuhalten (SAP-Softwaredatenträger kann auch per NFS freigegeben werden) und über genügend Speicherplatz zu verfügen, z.B. für Sicherungen. Die beiden zusätzlichen Datenträger waren Azure Storage Premium-Datenträger für SAP HANA-Daten und -Protokolldateien und das Verzeichnis „/usr/sap“.


### Kernelparameter


Für SAP HANA sind spezifische Linux-Kerneleinstellungen erforderlich, die nicht Teil des Standardimage des Azure-Katalogs sind und manuell festgelegt werden müssen. Es gibt einen spezifischen SAP-Hinweis, in dem die Einstellungen beschrieben werden.


SAP-Hinweis zu SAP HANA DB: Empfohlene Betriebssystemeinstellungen für SLES 12/SLE für SAP Applications 12: [SAP-Hinweis 2205917](https://launchpad.support.sap.com/#/notes/2205917).

Ein weiteres Thema zum Seitencache für die Ausführung von SAP HANA unter SLES finden Sie [hier](https://www.suse.com/documentation/sles_for_sap/singlehtml/sles_for_sap_guide/sles_for_sap_guide.html#sec.s4s.configure.page-cache) in Kapitel 6.1 „Kernel: Page-Cache Limit“ (Kernel: Seitencachelimit).

Es gibt auch einen SAP-Hinweis zum Seitencachelimit: [SAP-Hinweis 1557506](https://service.sap.com/sap/support/notes/1557506).

SLES 12 verfügt über ein neues Tool. Es ersetzt das alte Hilfsprogramm „sapconf“. Das Tool heißt „tuned-adm“, und es ist ein spezielles SAP HANA-Profil verfügbar. Weitere Details zu diesem Tool finden Sie unter den beiden folgenden Links.

Die SLES-Dokumentation zum tuned-adm-Profil „sap-hana“ finden Sie [hier](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html).

Die SLES-Dokumentation zum tuned-adm-Profil „sap-hana“ – Kapitel 6.2: Tuning Systems for SAP Workloads with tuned-adm (Optimierungssysteme für SAP-Workloads mit „tuned-adm“) – finden Sie [hier](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf).


![](./media/virtual-machines-linux-sap-hana-get-started/image005.jpg)

Hier wird veranschaulicht, wie mit „tuned-adm“ das Element „transparent\_hugepage“ und die Werte für „numa\_balancing“ gemäß den erforderlichen SAP HANA-Einstellungen geändert wurden.


Um die SAP HANA-Kerneleinstellungen permanent zu machen, muss „grub2“ unter SLES 12 verwendet werden. Weitere Informationen zu „grub2“ finden Sie [hier](https://www.suse.com/documentation/sled-12/book_sle_admin/data/sec_grub2_file_structure.html).


![](./media/virtual-machines-linux-sap-hana-get-started/image006.jpg)

Dieser Screenshot zeigt, wie die Kerneleinstellungen in der config-Datei geändert und dann mit „grub2-mkconfig“ „kompiliert“ wurden.


![](./media/virtual-machines-linux-sap-hana-get-started/image007.jpg)

Eine andere Möglichkeit ist die Änderung der Einstellungen über Yast und der Kernelparametereinstellungen des Startladeprogramms.


### Dateisysteme 

![](./media/virtual-machines-linux-sap-hana-get-started/image008.jpg)

Hier sind die zwei Dateisysteme zu sehen, die auf der SAP-App-Server-VM zusätzlich zu den beiden angefügten Azure-Standarddatenträgern für die Speicherung erstellt wurden. Beide Dateisysteme haben den Typ XFS und werden unter „/sapdata“ und „/sapsoftware“ bereitgestellt.

Es ist aber nicht zwingend erforderlich, so vorzugehen. Es gibt verschiedene Möglichkeiten, wie der Speicherplatz des Datenträgers strukturiert werden kann. Am wichtigsten ist es zu verhindern, dass für das Stammdateisystem kein Speicherplatz mehr vorhanden ist.


![](./media/virtual-machines-linux-sap-hana-get-started/image009.jpg)

In Bezug auf die SAP HANA DB-VM ist es wichtig zu wissen, dass bei einer Datenbankinstallation mit „sapinst“ (swpm) und der einfachen Installationsoption „Typisch“ die Daten standardmäßig unter „/hana“ und „/usr/sap“ installiert werden. Die Standardeinstellung für die Sicherung von SAP HANA-Protokollen befindet sich beispielsweise unter „/usr/sap“. Wie zuvor auch, muss unbedingt verhindert werden, dass die Kapazität des Stammdateisystems erschöpft ist. Daher sollten Sie sicherstellen, dass unter „/hana“ und „/usr/sap“ genügend freier Speicherplatz vorhanden ist, bevor Sie SAP HANA über swpm installieren.

In [diesem Artikel](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm) von SAP wird das Standardlayout des Dateisystems von SAP HANA beschrieben.


![](./media/virtual-machines-linux-sap-hana-get-started/image010.jpg)

Beim Installieren von SAP NetWeaver auf einem Standardimage aus dem SLES 12-Azure-Katalog wird eine Meldung angezeigt, dass kein Auslagerungsbereich vorhanden ist. Damit diese Meldung nicht mehr eingeblendet wird, können Sie beispielsweise eine Auslagerungsdatei wie in diesem Dokument beschrieben über dd, mkswap und swapon manuell hinzufügen. Suchen Sie in [diesem Artikel](https://www.suse.com/documentation/sled-12/book_sle_deployment/data/sec_yast2_i_y2_part_expert.html) einfach nach „Adding a Swap File Manually“ (Manuelles Hinzufügen einer Auslagerungsdatei).

Eine weitere Möglichkeit ist das Konfigurieren des Auslagerungsbereichs über den Linux-VM-Agent. Weitere Informationen finden Sie [hier](virtual-machines-linux-agent-user-guide.md).


### /etc/hosts

![](./media/virtual-machines-linux-sap-hana-get-started/image011.jpg)

Ein weiterer wichtiger Aspekt vor Beginn der Installation von SAP ist das Einbeziehen der Hostnamen und IP-Adressen von SAP-VMs in die Datei „/etc/hosts“. Alle SAP-VMs sollten in einem virtuellen Azure-Netzwerk bereitgestellt werden, und dann sollten die internen IP-Adressen verwendet werden.

### /etc/fstab

![](./media/virtual-machines-linux-sap-hana-get-started/image000c.jpg)

Es hat sich herausgestellt, dass es ratsam ist, während der Testphase „fstab“ dem Parameter „nofail“ hinzuzufügen. Falls mit den Datenträgern etwas schiefgeht, wird die VM trotzdem gestartet und bleibt nicht im Startprozess hängen. Gehen Sie aber mit Bedacht vor: In diesem Fall ist der zusätzliche Speicherplatz auf dem Datenträger unter Umständen nicht verfügbar, und die Prozesse können das Stammdateisystem verstopfen. Falls „/hana“ nicht verfügbar ist, wird SAP HANA nicht gestartet.


## Installieren des grafischen Gnome-Desktops unter SLES 12

Dieses Kapitel besteht aus zwei Abschnitten, in denen die folgenden Themen behandelt werden:

* Installation des Gnome-Desktops und von „xrdp“ unter SLES 12
* Ausführen der Java-basierten SAP MC mit Firefox unter SLES 12

Sie können auch Alternativen wie Xterminal oder VNC nutzen. In diesem Artikel wird aber nur „xrdp“ beschrieben (Stand: September 2016).

### Installation des Gnome-Desktops und von „xrdp“ unter SLES 12

Besonders für Benutzer mit Microsoft Windows-Hintergrund, die einen grafischen Desktop direkt auf den SAP Linux-VMs nutzen möchten, um Firefox, Sapinst, SAP GUI, SAP MC oder HANA Studio auszuführen und ggf. von einem Microsoft Windows-Computer per RDP eine Verbindung mit der VM herzustellen, ist dies auf einfache Weise möglich. Für einen Produktionsdatenbankserver ist diese Vorgehensweise unter Umständen nicht geeignet, aber für eine reine Prototyp-/Demoumgebung ist dies kein Problem. Hier sind die Schritte zum Installieren des Gnome-Desktops auf einer Azure SLES 12-VM angegeben:

Installieren Sie den gnome-Desktop mit dem folgenden Befehl (z.B. in einem PuTTY-Fenster):

   zypper in -t pattern gnome-basic

Installieren Sie anschließend „xrdp“, um eine Verbindung mit der VM per RDP zu ermöglichen:

   zypper in xrdp

Bearbeiten Sie „/etc/sysconfig/windowmanager“, und legen Sie den Standard-Manager für Fenster auf „Gnome“ fest:

   DEFAULT\_WM="gnome"

Stellen Sie durch Ausführen von „chkconfig“ sicher, dass „xrdp“ nach einem Neustart automatisch gestartet wird:

  chkconfig -level 3 xrdp on

Falls ein Problem mit der RDP-Verbindung auftritt, können Sie es mit einem Neustart probieren (ggf. über ein PuTTY-Fenster):

  /etc/xrdp/xrdp.sh restart

Falls der xrdp-Neustart nicht wie oben beschrieben funktioniert, sollten Sie das Vorhandensein einer PID-Datei überprüfen und diese entfernen:

  Überprüfen Sie „/var/run“, und suchen Sie nach „xrdp.pid“. Entfernen Sie die Datei, und führen Sie den Neustart dann erneut aus.



### SAP MC


Beim Starten der grafischen Java-basierten SAP MC mit dem Firefox-Browser, der auf einer Azure SLES 12-VM ausgeführt wird, tritt nach dem Installieren des Gnome-Desktops gemäß der Beschreibung im vorherigen Abschnitt ein Fehler aufgrund eines fehlenden Java-Browser-Plug-Ins auf.

Die URL zum Starten der SAP MC lautet „<Server>:5<Instanznummer>13“.

Weitere Informationen finden Sie [hier](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm).


![](./media/virtual-machines-linux-sap-hana-get-started/image013.jpg)

Im obigen Screenshot ist zu sehen, wie die Fehlermeldung aussehen kann, wenn das Java-Browser-Plug-In fehlt.

![](./media/virtual-machines-linux-sap-hana-get-started/image014.jpg)

Eine Möglichkeit zur Lösung des Problems ist die einfache Installation des fehlenden Plug-Ins über Yast.

![](./media/virtual-machines-linux-sap-hana-get-started/image015.jpg)

Beim Wiederholen der SAP MC-URL wird beim ersten Mal ein kleines Dialogfeld mit der Aufforderung zum Aktivieren des Plug-Ins angezeigt.


Eine weitere Fehlermeldung, die ggf. angezeigt wird, bezieht sich auf eine fehlende Datei: „javafx.properties“. Dies hängt wahrscheinlich mit der Installation von Java 1.8 zusammen, da diese Version für SAP GUI 7.4 erforderlich ist.

Die IBM Java-Version, die über Yast verfügbar ist, enthält diese Datei nicht. Die Lösung ist ein Java-Download von Oracle. Einen Artikel, in dem es um dieses spezifische Problem geht, finden Sie [hier](https://scn.sap.com/thread/3908306).



## Manuelle SAP HANA-Installation per SWPM im Rahmen einer NetWeaver 7.5-Installation


Die folgende Liste mit Screenshots veranschaulicht die wichtigsten Schritte der Installation von SAP NetWeaver 7.5 und SAP HANA SP12 per SWPM (sapinst). Als Teil einer NW 7.5-Installation verfügt SWPM auch über die Funktionen zum Installieren der HANA-Datenbank als Einzelinstanz.


![](./media/virtual-machines-linux-sap-hana-get-started/image012.jpg)

Für die Beispieltestumgebung wurde nur ein ABAP-App-Server installiert. Die Option „Verteiltes System“ wurde zum Installieren der ASCS-Instanz und der Instanz des primären App-Servers auf einer Azure-VM und von SAP HANA als Datenbanksystem auf einer anderen Azure-VM verwendet.


![](./media/virtual-machines-linux-sap-hana-get-started/image016.jpg)

Nachdem die ASCS-Instanz auf der App-Server-VM installiert und in der SAP MC auf „grün“ festgelegt wurde, muss das Verzeichnis „sapmnt“, in dem z.B. das SAP-Profilverzeichnis enthalten ist, für die SAP HANA DB-Server-VM freigegeben werden. Für den DB-Installationsschritt muss Zugriff auf diese Informationen bestehen. Die beste Möglichkeit ist die Verwendung des NFS, das für die Nutzung von Yast konfiguriert werden kann.


![](./media/virtual-machines-linux-sap-hana-get-started/image017b.jpg)

Auf der App-Server-VM sollte das Verzeichnis „sapmnt“ über NFS freigegeben werden, indem die Optionen „rw“ und „no\_root\_squash“ verwendet werden. Die Standardeinstellungen sind „ro“ und „root\_squash“. Dies kann zu Problemen beim Installieren der Datenbankinstanz führen.


![](./media/virtual-machines-linux-sap-hana-get-started/image018b.jpg)

Auf der SAP HANA DB-Server-VM muss die sapmnt-Freigabe von der App-Server-VM über „NFS client“ konfiguriert werden (z.B. mithilfe von Yast).


![](./media/virtual-machines-linux-sap-hana-get-started/image019.jpg)

Als Nächstes muss die Anmeldung an der SAP HANA DB-Server-VM durchgeführt werden und SWPM gestartet werden, um den nächsten Schritt einer verteilten NW 7.5-Installation auszuführen: „Datenbankinstanz“.


![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

In Bezug auf die SAP HANA-Installation muss nach dem Auswählen einer „typischen“ Installation eigentlich nicht mehr viel eingegeben werden. Zusätzlich zum Pfad zu den Installationsmedien müssen eine DB-SID, der Hostname, die Instanznummer und das DB Sys Admin-Kennwort eingegeben werden.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Der nächste Schritt ist das Eingeben des Kennworts für das DBACOCKPIT-Schema.



![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Danach folgt die Abfrage des SAPABAP1-Schemakennworts.


![](./media/virtual-machines-linux-sap-hana-get-started/image023.jpg)

Am Ende sollten vor jeder Phase des DB-Installationsprozesses ausschließlich grüne Häkchen zu sehen sein, und das kleine eingeblendete Meldungsfeld sollte den Text „Execution of ... Database Instance has completed“ (Ausführung von ... Datenbankinstanz wurde abgeschlossen) enthalten.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image024.jpg)

Nach der erfolgreichen Installation sollte die DB-Instanz in der SAP MC auch als „grün“ angezeigt werden, und die vollständige Liste mit den SAP HANA-Prozessen sollte zu sehen sein (z.B. hdbindexserver, hdbcompileserver).


![](./media/virtual-machines-linux-sap-hana-get-started/image025.jpg)

Dieser Screenshot zeigt Teile der Dateistruktur unter „/hana/shared“, die von SWPM während der HANA-Installation erstellt werden. Es gibt keine Möglichkeit, einen anderen Pfad anzugeben. Aus diesem Grund ist es so wichtig, vor der SAP HANA-Installation über SWPM zusätzlichen Datenträgerplatz unter „/hana“ bereitzustellen, um zu verhindern, dass im Stammdateisystem kein freier Speicherplatz mehr verfügbar ist.


![](./media/virtual-machines-linux-sap-hana-get-started/image026.jpg)

Hier ist derselbe Sachverhalt für das Verzeichnis „/usr/sap“ dargestellt.


![](./media/virtual-machines-linux-sap-hana-get-started/image027b.jpg)

Der letzte Schritt der verteilten ABAP-Installation ist die „Instanz des primären Anwendungsservers“.


![](./media/virtual-machines-linux-sap-hana-get-started/image028b.jpg)

Nachdem der PAS und die SAP GUI installiert wurden, können Sie über die Transaktion „dbacockpit“ überprüfen, ob für die SAP HANA-Installation alles korrekt ist.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Als letzten Schritt können Sie SAP HANA Studio auf der SAP-App-Server-VM installieren und eine Verbindung mit der SAP HANA-Instanz herstellen, die auf der DB-Server-VM ausgeführt wird.




## Manuelle Installation von SAP HANA mit dem HANA Life Cycle Manager-Tool „hdblcm“


Neben der Installation von SAP HANA als Teil einer verteilten Installation über SWPM ist es auch möglich, HANA zuerst eigenständig mit „hdblcm“ und danach beispielsweise SAP NetWeaver 7.5 zu installieren. In der unten angegebenen Liste mit den Screenshots wird veranschaulicht, wie dies funktioniert.

Hier sind drei Quellen mit Informationen zum HANA-Tool „hdblcm“ aufgeführt:

[Choosing the Correct SAP HANA HDBLCM for Your Task (Auswählen des richtigen SAP HANA-HDBLCM-Tools für Ihre Aufgabe)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)

[SAP HANA Lifecycle Management Tools (SAP HANA-Tools für die Lebenszyklusverwaltung)](http://saphanatutorial.com/sap-hana-lifecycle-management-tools/)

[SAP HANA Server Installation and Update Guide (SAP HANA-Serverinstallation und Aktualisierungshandbuch)](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)



![](./media/virtual-machines-linux-sap-hana-get-started/image030.jpg)

Um spätere Probleme mit einer Gruppen-ID-Standardeinstellung für den Benutzer „<HANA-SID>adm“ (mit dem Tool „hdblcm“ erstellt) zu vermeiden, sollte eine neue Gruppe mit dem Namen „sapsys“ und der Gruppen-ID 1001 definiert werden, bevor SAP HANA mit „hdblcm“ installiert wird.




![](./media/virtual-machines-linux-sap-hana-get-started/image031.jpg)

Beim ersten Starten von „hdblcm“ wird ein einfaches Startmenü angezeigt, in dem Sie den ersten Eintrag „Install new System“ (Neues System installieren) auswählen.



![](./media/virtual-machines-linux-sap-hana-get-started/image032.jpg)

In diesem Screenshot sind alle wichtigen Optionen zu sehen, die zuvor eingegeben wurden. Wichtig: Verzeichnisse, die für HANA-Protokoll- und -Datenvolumes benannt wurden, sowie der Installationspfad (hier „/hana/shared“) und „/usr/sap“ sollten NICHT Teil des Stammdateisystems sein, sondern zu Azure-Datenträgern für Daten gehören, die an die VM angefügt wurden (wie im Abschnitt zur Azure-VM-Einrichtung beschrieben). So wird verhindert, dass die Kapazität des Stammdateisystems erschöpft ist. Es ist auch zu sehen, dass der HANA-Administratorbenutzer über die Benutzer-ID 1005 verfügt und Teil der Gruppe „sapsys“ (ID 1001) ist, die vor der Installation definiert wurde.



![](./media/virtual-machines-linux-sap-hana-get-started/image033.jpg)

Sie können die HANA -Benutzerdetails für „<HANA-SID>adm“ (hier „azdadm“) unter „/etc/passwd“ überprüfen.



![](./media/virtual-machines-linux-sap-hana-get-started/image034.jpg)

Nach der Installation von SAP HANA mit „hdblcm“ wird der entsprechende Eintrag in SAP HANA Studio angezeigt. Das Schema SAPABAP1, in dem beispielsweise alle SAP NetWeaver-Tabellen enthalten sind, ist noch nicht verfügbar.



![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Nach der Installation von SAP HANA kann zusätzlich SAP NetWeaver installiert werden. In diesem Beispiel wurde eine „verteilte Installation“ über SWPM wie im entsprechenden Abschnitt beschrieben verwendet. Beim Installieren der Datenbankinstanz über SWPM müssen Sie lediglich die gleichen Daten wie zuvor für „hdblcm“ eingeben (z.B. Hostname, HANA-SID, Instanznummer). SWPM verwendet dann die vorhandene HANA-Installation und fügt zusätzliche Schemas hinzu.



![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Dies ist die Abbildung des SWPM-Installationsschritts, in dem die Daten für das DBACOCKPIT-Schema eingegeben werden.


![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Dann wird von SWPM erwartet, dass Daten zum SAPABAP1-Schema eingegeben werden.


![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Nach Abschluss der Installation der SWPM-Datenbankinstanz wird das SAPABAP1-Schema in HANA Studio angezeigt.



![](./media/virtual-machines-linux-sap-hana-get-started/image039b.jpg)

Nach der Installation des SAP-App-Servers und der SAP GUI kann die HANA DB-Instanz dann mit der Transaktion „dbacockpit“ überprüft werden.




## Allgemeine Informationen zu SAP-Azure-Zertifizierungen, zur Ausführung von SAP HANA unter Azure und zum Download der SAP-Software

* Allgemeine SAP-Azure-Dokumentation zur Ausführung von SAP unter Azure mit dem Windows-Betriebssystem im klassischen Modus: [Verwenden von SAP auf virtuellen Windows-Computern in Azure](virtual-machines-windows-classic-sap-get-started.md).

* Informationen zu vorhandenen SAP-Vorlagen zur Nutzung durch Kunden: [Azure Quickstart Templates for SAP](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/) (Azure-Schnellstartvorlagen für SAP).

* Allgemeine SAP-Azure-Dokumentation zur Ausführung von SAP unter Azure mit dem Linux-Betriebssystem im Azure Resource Manager-Modell: [Verwenden von SAP auf virtuellen Linux-Computern](virtual-machines-linux-sap-get-started.md).

* Verzeichnis mit zertifizierter SAP HANA-Hardware, in dem aufgeführt ist, welche Arten von Azure-VMs für die Produktion unterstützt werden: [Certified SAP HANA® Hardware Directory](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).

* Informationen zur Größe von virtuellen Computern, speziell für Linux-Workloads: [Größen für virtuelle Computer in Azure](virtual-machines-linux-sizes.md).

* SAP-Hinweis, in dem alle unterstützten SAP-Produkte unter Azure und die unterstützten Azure-VM-Typen für SAP aufgeführt sind: [SAP-Hinweis 1928533](https://launchpad.support.sap.com/#/notes/1928533/E).

* SAP-Hinweis zur „erweiterten Überwachung“ von SAP mit Linux-VMs unter Azure: [SAP-Hinweis 2191498](https://launchpad.support.sap.com/#/notes/2191498/E).

* SAP HANA-Angebot zu „großen Instanzen“ von Azure. Es ist wichtig zu verstehen, dass es hierbei nicht um die Ausführung von SAP HANA auf Azure-VMs geht, sondern um eine Hybridumgebung, bei der die SAP-App-Server auf Azure-VMs und SAP HANA auf Bare-Metal-Servern ausgeführt werden: [SAP-Hinweis 2316233](https://launchpad.support.sap.com/#/notes/2316233/E).

* SAP-Hinweis mit Informationen zu SAPOSCOL unter Linux: [SAP-Hinweis 1102124](https://launchpad.support.sap.com/#/notes/1102124/E).

* Wichtige Überwachungsmetriken für SAP in Microsoft Azure: [SAP-Hinweis 2178632](https://launchpad.support.sap.com/#/notes/2178632/E).

* Weitere Informationen zu Azure Resource Manager: [Übersicht über den Azure Resource Manager](../resource-group-overview.md).

* Informationen zum Bereitstellen von virtuellen Linux-Computern über Vorlagen: [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI](virtual-machines-linux-cli-deploy-templates.md).

* Vergleich der Bereitstellungsmodelle (Azure Resource Manager und klassisch): [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung: Grundlegendes zu Bereitstellungsmodellen und zum Status von Ressourcen](../resource-manager-deployment-model.md).

* Download von NetWeaver 7.5 für Linux/HANA über den SAP Service Marketplace: ![](./media/virtual-machines-linux-sap-hana-get-started/image001.jpg)

* Download der HANA SP12 Platform Edition über den SAP Service Marketplace: ![](./media/virtual-machines-linux-sap-hana-get-started/image002.jpg)

<!---HONumber=AcomDC_0921_2016-->