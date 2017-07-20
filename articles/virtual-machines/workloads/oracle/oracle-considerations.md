---
title: "Oracle-Lösungen in Microsoft Azure | Microsoft-Dokumentation"
description: "Hier finden Sie Informationen zu unterstützten Konfigurationen und Einschränkungen von Oracle-Lösungen in Microsoft Azure."
services: virtual-machines-linux
documentationcenter: 
manager: timlt
author: rickstercdn
tags: azure-resource-management
ms.assetid: 5d71886b-463a-43ae-b61f-35c6fc9bae25
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/15/2017
ms.author: rclaus
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: 8f95c79637db39208267e477735aefb9bec14512
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---
# <a name="oracle-solutions-and-their-deployment-on-microsoft-azure"></a>Oracle-Lösungen und deren Bereitstellung in Microsoft Azure
Dieser Artikel enthält Informationen, die Sie benötigen, um verschiedene Oracle-Lösungen erfolgreich in Microsoft Azure bereitzustellen. Diese Lösungen basieren auf VM-Images, die von Oracle im Azure Marketplace veröffentlicht werden. Um eine Liste der derzeit verfügbaren Images zu erhalten, führen Sie den folgenden Befehl aus:
```azurecli-interactive
az vm image list --publisher oracle -o table --all
```
Bis zum 16.6.2017 lautet die Liste der Images wie folgt:
```bash
Offer                   Publisher    Sku                     Urn                                                          Version
----------------------  -----------  ----------------------  -----------------------------------------------------------  -------------
Oracle-Database-Ee      Oracle       12.1.0.2                Oracle:Oracle-Database-Ee:12.1.0.2:12.1.20170202             12.1.20170202
Oracle-Database-Se      Oracle       12.1.0.2                Oracle:Oracle-Database-Se:12.1.0.2:12.1.20170202             12.1.20170202
Oracle-Linux            Oracle       6.4                     Oracle:Oracle-Linux:6.4:6.4.20141206                         6.4.20141206
Oracle-Linux            Oracle       6.7                     Oracle:Oracle-Linux:6.7:6.7.20161007                         6.7.20161007
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.20161020                         6.8.20161020
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.20170406                         6.9.20170406
Oracle-Linux            Oracle       7.0                     Oracle:Oracle-Linux:7.0:7.0.20141217                         7.0.20141217
Oracle-Linux            Oracle       7.2                     Oracle:Oracle-Linux:7.2:7.2.20161020                         7.2.20161020
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.20170320                         7.3.20170320
Oracle-WebLogic-Server  Oracle       Oracle-WebLogic-Server  Oracle:Oracle-WebLogic-Server:Oracle-WebLogic-Server:12.1.2  12.1.2
```

Diese Images werden als „Bring Your Own License“ betrachtet. Daher werden nur die Kosten für Computing, Speicherung und Netzwerke berechnet, die durch die Ausführung eines virtuellen Computers anfallen.  Es wird vorausgesetzt, dass Sie über eine ordnungsgemäße Lizenz für die Verwendung von Oracle-Software und über einen aktuellen Supportvertrag mit Oracle verfügen. Oracle bietet eine garantierte Lizenzmobilität von einer lokalen Verwendung zu Azure. Informationen zur Lizenzmobilität finden Sie im veröffentlichten Artikel [Oracle und Microsoft](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html). 

Einzelpersonen können als Basis für ihre Lösungen auch benutzerdefinierte Images verwenden, die sie in Azure von Grund auf neu erstellen, oder sie können ein benutzerdefiniertes Image aus ihren lokalen Umgebungen hochladen.

## <a name="support-for-jd-edwards"></a>Unterstützung für JD Edwards
Laut dem Oracle-Support-Artikel [Dok.-ID 2178595.1](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=573435677515785&id=2178595.1&_afrWindowMode=0&_adf.ctrl-state=o852dw7d_4) werden JD Edwards EnterpriseOne, Versionen 9.2 und höher, in **jedem öffentlichen Cloudangebot** unterstützt, das ihre spezifischen `Minimum Technical Requirements` (MTR, technische Mindestanforderungen) erfüllt.  Sie müssen benutzerdefinierte Images erstellen, die ihre MTR-Spezifikationen für das Betriebssystem und die Anwendungskompatibilität erfüllen. 

## <a name="oracle-database-virtual-machine-images"></a>Images von virtuellen Oracle Database-Computern
Oracle unterstützt die Ausführung von Oracle DB 12.1 Standard und Enterprise Edition in Azure auf virtuellen Computerimages, die auf Oracle Linux basieren.  Um die optimale Leistung für Produktionsarbeitslasten von Oracle DB in Azure zu erzielen, achten Sie darauf, die Größe des VM-Images angemessen festzulegen. Verwenden Sie außerdem verwaltete Datenträger, die durch Storage Premium gestützt werden. Anweisungen, wie Sie eine Oracle DB mit dem von Oracle veröffentlichten VM-Image schnell in Azure einrichten, erhalten Sie im [Oracle DB-Schnellstart](oracle-database-quick-create.md).

### <a name="attached-disk-configuration-options"></a>Optionen für die Konfiguration angefügter Datenträger

Angefügte Datenträger nutzen den Azure-BLOB-Speicherdienst. Auf jedem Standarddatenträger können theoretisch bis zu 500 E/A-Vorgänge pro Sekunde (IOPS) ausgeführt werden. Unser Premium-Datenträgerangebot wird in erster Linie für Datenbanknutzlasten mit hoher Leistung verwendet und kann bis zu 5.000 IOps pro Datenträger erreichen. Sie können zwar einen einzelnen Datenträger verwenden, wenn dies Ihren Anforderungen an die Leistung genügt. Sie können die effektive IOPS-Leistung jedoch verbessern, wenn Sie mehrere angefügte Datenträger verwenden, Datenbankdaten auf diesen verteilen und dann Oracle Automatic Storage Management (ASM) verwenden. Spezifische Informationen zu Oracle ASM finden Sie unter [Oracle Automatic Storage Management Overview](http://www.oracle.com/technetwork/database/index-100339.html) (in englischer Sprache). Ein Beispiel für das Installieren und Konfigurieren von Oracle ASM auf einem virtuellen Azure-Linux-Computer erhalten Sie im Tutorial [Installing and Configuring Oracle Automated Storage Management](configure-oracle-asm.md) (Installieren und Konfigurieren von Oracle Automated Storage Management).

### <a name="oracle-realtime-application-cluster-rac"></a>Oracle Real Application Cluster (RAC)
Oracle RAC ist so konzipiert, dass der Ausfall eines einzelnen Knotens in einer lokalen Clusterkonfiguration mit mehreren Knoten möglichst geringe Auswirkungen hat.  RAC beruht auf zwei lokalen Technologien, die in öffentlichen Cloudumgebungen mit Hyperskalierung nicht nativ funktionieren: Netzwerk-Multicast und freigegebene Datenträger. Drittanbieterlösungen von Unternehmen wie [FlashGrid](https://www.flashgrid.io/oracle-rac-in-azure/) emulieren diese Technologien, falls Sie Oracle RAC in Azure bereitstellen müssen. 

### <a name="high-availability-and-disaster-recovery-considerations"></a>Überlegungen zu Hochverfügbarkeit und Notfallwiederherstellung
Wenn Sie Oracle-Datenbanken in Azure verwenden, müssen Sie eine Lösung für hohe Verfügbarkeit und Notfallwiederherstellung implementieren, um Ausfallzeiten zu vermeiden. 

Hochverfügbarkeit und Notfallwiederherstellung für Oracle Database Enterprise Edition (ohne RAC) in Azure lassen sich mithilfe von [Data Guard, Active Data Guard](http://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html) oder [Oracle GoldenGate](http://www.oracle.com/technetwork/middleware/goldengate) mit zwei Datenbanken auf zwei eigenen virtuellen Computern erreichen. Beide virtuellen Computer sollten sich in demselben [virtuellen Netzwerk](https://azure.microsoft.com/documentation/services/virtual-network/) befinden, um sicherzustellen, dass sie über die private statische IP-Adresse Zugriff aufeinander haben.  Darüber hinaus wird empfohlen, die virtuellen Computer in derselben Verfügbarkeitsgruppe zu platzieren, damit sie von Azure in eigenen Fehlerdomänen und Upgradedomänen angeordnet werden können.  Falls Sie Georedundanz verwenden möchten, können Sie festlegen, dass diese zwei Datenbanken zwischen verschiedenen Regionen repliziert werden, und die zwei Instanzen mit einem VPN Gateway verbinden.

Unser Tutorial [Implementieren von Oracle DataGuard in Azure](configuring-oracle-dataguard.md) führt Sie durch die grundlegenden Schritte der Einrichtung, um dies in Azure zu testen.  

Mit Oracle Data Guard kann Hochverfügbarkeit mit einer primären Datenbank auf einem virtuellen Computer, einer sekundären Datenbank (Standbydatenbank) auf einem weiteren virtuellen Computer und unidirektionaler Replikation zwischen diesen Komponenten erzielt werden. Das Ergebnis ist Lesezugriff auf die Kopie der Datenbank. Mit Oracle GoldenGate können Sie bidirektionale Replikation zwischen den beiden Datenbanken konfigurieren. Informationen zum Einrichten einer Hochverfügbarkeitslösung für die Datenbanken mithilfe dieser Tools finden Sie in der Dokumentation zu [Active Data Guard](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) und [GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) auf der Oracle-Website. Wenn Sie Schreibzugriff auf die Kopie der Datenbank benötigen, können Sie [Oracle Active Data Guard](http://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html)verwenden.

Unser Tutorial [Implementieren von Oracle GoldenGate in Azure](configure-oracle-golden-gate.md) führt Sie durch die grundlegenden Schritte der Einrichtung, um dies in Azure zu testen.

Trotz einer Lösung für hohe Verfügbarkeit und Notfallwiederherstellung in Azure, sollten Sie sicherstellen, dass Sie über eine Sicherungsstrategie zum Wiederherstellen der Datenbank verfügen.  Unser Tutorial [Sichern und Wiederherstellen einer Oracle-Datenbank](oracle-backup-recovery.md) führt Sie durch die grundlegenden Schritte der Einrichtung einer konsistenten Sicherung.

## <a name="oracle-weblogic-server-virtual-machine-images"></a>Images von virtuellen Oracle WebLogic Server-Computern
* **Clustering wird nur in der Enterprise Edition unterstützt.** Sie sind nur für die Verwendung von WebLogic-Clustern lizenziert, wenn Sie die Enterprise Edition von WebLogic Server verwenden. Verwenden Sie Clustering nicht mit der WebLogic Server Standard Edition.
* **UDP-Multicast wird nicht unterstützt.** Azure unterstützt UDP-Unicasting, jedoch weder Multicasting noch Broadcasting. WebLogic Server kann die UDP-Unicast-Funktionen von Azure nutzen. Um optimale Ergebnisse bei der Nutzung von UDP-Unicast zu erzielen, empfiehlt es sich, eine statische Größe des WebLogic-Clusters zu verwenden oder nicht mehr als 10 verwaltete Server in den Cluster aufzunehmen.
* **WebLogic Server erwartet, dass für den T3-Zugriff (z.B. bei Verwendung von Enterprise JavaBeans) öffentliche und private Ports identisch sind.** Beispielszenario: Eine Anwendung auf Dienstebene (EJB) wird in einem WebLogic-Servercluster, der aus mindestens zwei VMs besteht, in einem vNet mit dem Namen **SLWLS** ausgeführt. Die Clientebene befindet sich in einem anderen Subnetz in demselben vNet, und es wird ein einfaches Java-Programm ausgeführt, das versucht, EJB auf der Dienstebene aufzurufen. Da die Dienstebene einen Lastenausgleich erfordert, muss für die virtuellen Computer im WebLogic Server-Cluster ein öffentlicher Endpunkt mit Lastenausgleich erstellt werden. Wenn der private Port, den Sie angeben, nicht mit dem öffentlichen Port identisch ist (z.B. 7006:7008), tritt ein Fehler wie der folgende auf:

       [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

       Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

   Der Grund dafür ist, dass WebLogic Server für jeden T3-Remotezugriff erwartet, dass der Port für das Load Balancer-Modul und der Port des verwalteten WebLogic-Servers identisch sind. Im obigen Fall greift der Client auf Port 7006 (den Port für das Load Balancer-Modul) zu, und der verwaltete Server lauscht an Port 7008 (dem privaten Port). Diese Einschränkung gilt nur für den T3-Zugriff und nicht für HTTP.

   Sie können dieses Problem mit einer der folgenden Problemumgehungen vermeiden:

  * Verwenden Sie für Endpunkte mit Lastenausgleich, die für T3-Zugriff vorgesehen sind, die gleiche Nummer für den öffentlichen und privaten Port.
  * Schließen Sie beim Starten von WebLogic Server den folgenden JVM-Parameter ein:

         -Dweblogic.rjvm.enableprotocolswitch=true

Weitere Informationen finden Sie im KB-Artikel **860340.1** unter <http://support.oracle.com>.

* **Einschränkungen für dynamisches Clustering und Lastenausgleich**. Angenommen, Sie möchten in WebLogic Server einen dynamischen Cluster verwenden und ihn in Azure über einen einzelnen Endpunkt mit Lastenausgleich verfügbar machen. Dies ist möglich, solange Sie für jeden verwalteten Server eine feste Portnummer (keine dynamisch aus einem Bereich zugewiesene Nummer) verwenden und nicht mehr verwaltete Server starten, als Computer vorhanden sind, die vom Administrator nachverfolgt werden (d. h. nicht mehr als einen verwalteten Server pro virtuellem Computer). Wenn die Konfiguration bewirkt, dass mehr WebLogic-Server gestartet werden, als virtuelle Computer vorhanden sind (d.h., wenn mehrere WebLogic Server-Instanzen denselben virtuellen Computer verwenden), kann nicht mehr als eine dieser WebLogic Server-Instanzen an eine angegebene Portnummer gebunden werden, und die anderen Instanzen auf diesem virtuellen Computer schlagen fehl.

   Wenn Sie hingegen festlegen, dass der Admin-Server automatisch den verwalteten Servern eindeutige Portnummern zuweist, ist kein Lastenausgleich möglich. Der Grund dafür ist, dass diese Konfiguration das Zuordnen eines einzelnen öffentlichen Ports zu mehreren privaten Ports erfordert. Dies wird jedoch von Azure nicht unterstützt.
* **Mehrere Instanzen von WebLogic Server auf einem virtuellen Computer**. Je nach den Anforderungen der Bereitstellung sollten Sie die Möglichkeit in Betracht ziehen, mehrere Instanzen von WebLogic Server auf demselben virtuellen Computer auszuführen, sofern die Größe des virtuellen Computers ausreicht. Beispielsweise können Sie auf einem virtuellen Computer mittlerer Größe, der zwei Kerne enthält, zwei Instanzen von WebLogic Server ausführen. Es wird jedoch davon abgeraten, nur einen virtuellen Computer zu verwenden, auf dem mehrere Instanzen von WebLogic Server ausgeführt werden, da hierdurch einzelne Fehlerquellen in die Architektur eingeführt werden. Ein besserer Ansatz ist die Verwendung von mindestens zwei virtuellen Computern. Auf jedem virtuellen Computer könnten dann mehrere Instanzen von WebLogic Server ausgeführt werden. Jede dieser Instanzen von WebLogic Server kann dennoch Teil desselben Clusters sein. Beachten Sie jedoch, dass Azure derzeit nicht für den Lastenausgleich von Endpunkten verwendet werden kann, die durch WebLogic Server-Bereitstellungen auf demselben virtuellen Computer verfügbar gemacht werden. Der Grund dafür ist, dass der Azure-Load Balancer die Verteilung der Server mit Lastenausgleich auf eindeutige virtuelle Computer erfordert.

## <a name="oracle-jdk-virtual-machine-images"></a>Images von virtuellen Oracle JDK-Computern
* **Neueste Updates von JDK 6 und 7**. Es wird zwar empfohlen, die neueste öffentliche unterstützte Version von Java (derzeit Java 8) zu verwenden, aber für Azure werden auch JDK 6- und JDK 7-Images verfügbar gemacht. Diese sind für ältere Anwendungen vorgesehen, die noch nicht auf JDK 8 aktualisiert werden können. Updates früherer JDK-Images sind möglicherweise nicht mehr für die allgemeine Öffentlichkeit verfügbar. Aufgrund der Partnerschaft von Microsoft mit Oracle bieten die von Azure bereitgestellten JDK 6- und JDK 7-Images jedoch ein neueres nicht öffentliches Update, das Oracle normalerweise nur einer bestimmten Gruppe unterstützter Oracle-Kunden anbietet. Im Lauf der Zeit werden neuere Version der JDK-Images mit aktualisierten Versionen von JDK 6 und 7 zur Verfügung gestellt.

   In diesen JDK 6- und JDK 7-Images verfügbare JDK und die von ihnen abgeleiteten virtuellen Computer und Images können nur in Azure verwendet werden.
* **64-Bit-JDK.** Die von Azure bereitgestellten Images virtueller Oracle WebLogic Server-Computer und virtueller Oracle JDK-Computer enthalten die 64-Bit-Versionen von Windows Server und dem JDK.

## <a name="next-steps"></a>Nächste Schritte
Sie haben jetzt einen Überblick über aktuelle Oracle-Lösungen in Microsoft Azure. Der nächste Schritt besteht darin, die erste Oracle-Datenbank in Azure bereitzustellen.
- Führen Sie für die ersten Schritte das Tutorial [Erstellen einer Oracle-Datenbank in Azure](oracle-database-quick-create.md) durch.
