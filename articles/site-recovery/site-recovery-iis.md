---
title: Replizieren einer IIS-basierten Webanwendung mit mehreren Ebenen mit Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie virtuelle Computer der IIS-Webfarm mit Azure Site Recovery replizieren.
services: site-recovery
documentationcenter: 
author: nsoneji
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: nisoneji
ms.translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: b23624fc7e82af1cb593a1aedd138ae0d6637ae7
ms.contentlocale: de-de
ms.lasthandoff: 03/28/2017

---
# <a name="replicate-a-multi-tier-iis-based-web-application-using-azure-site-recovery"></a>Replizieren einer IIS-basierten Webanwendung mit mehreren Ebenen mit Azure Site Recovery

## <a name="overview"></a>Übersicht


Die Anwendungssoftware ist das Antriebsmodul der geschäftlichen Produktivität in einer Organisation. Verschiedene Webanwendungen können in einer Organisation unterschiedliche Zwecke erfüllen. Einige davon, z.B. Gehaltsabrechnung, Finanzanwendungen und Kundenwebsites, können für eine Organisation von entscheidender Bedeutung sein. Es ist für die Organisation wichtig, dass sie jederzeit in Betrieb sind, um Produktivitätsverluste zu verhindern und – was noch wichtiger ist – Schaden in Bezug auf das Image der Organisation abzuwenden.

Wichtige Webanwendungen werden normalerweise als Anwendungen mit mehreren Ebenen eingerichtet, sodass der Web-, Datenbank- und Anwendungsbereich auf unterschiedlichen Ebenen angeordnet werden kann. Die Anwendungen sind nicht nur auf unterschiedliche Ebenen verteilt, sondern auf jeder Ebene können dafür auch mehrere Server eingesetzt werden, um für den Datenverkehr einen Lastenausgleich zu erzielen. Außerdem können die Zuordnungen zwischen verschiedenen Ebenen und auf dem Webserver auf statischen IP-Adressen basieren. Bei einem Failover müssen einige dieser Zuordnungen aktualisiert werden. Dies gilt vor allem, wenn Sie auf dem Webserver mehrere Websites konfiguriert haben. Bei Webanwendungen, für die SSL verwendet wird, müssen Zertifikatbindungen aktualisiert werden.

Herkömmliche Wiederherstellungsmethoden ohne Replikation umfassen das Sichern von verschiedenen Konfigurationsdateien, Registrierungseinstellungen, Bindungen, benutzerdefinierten Komponenten (COM oder .NET), Inhalten sowie Zertifikaten und das Wiederherstellen der Dateien in mehreren manuellen Schritten. Diese Verfahren sind umständlich, fehleranfällig und nicht skalierbar. Beispielsweise kann es leicht passieren, dass Sie das Sichern der Zertifikate vergessen und dann keine andere Wahl haben, als nach dem Failover neue Zertifikate für den Server zu erwerben.

Bei einer guten Lösung für die Notfallwiederherstellung sollte die Modellierung von Wiederherstellungsplänen für die obigen komplexen Anwendungsarchitekturen möglich sein. Darüber hinaus sollten Sie angepasste Schritte hinzufügen können, um Anwendungszuordnungen zwischen unterschiedlichen Ebenen durchzuführen und so eine sichere Single-Click-Lösung zu haben, falls es in einem Notfall zu einem niedrigeren RTO-Wert kommt.


In diesem Artikel wird beschrieben, wie Sie eine IIS-basierte Webanwendung mit einer [Azure Site Recovery](site-recovery-overview.md)-Instanz schützen. Wir stellen die bewährten Methoden zum Replizieren einer IIS-basierten Webanwendung mit drei Ebenen in Azure, Durchführen einer Übung für die Notfallwiederherstellung und Durchführen eines Failovers für die Anwendung nach Azure vor.


## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zunächst sicher, dass Sie mit den folgenden Verfahren vertraut sind:

1. [Replizieren von virtuellen VMware-Computern in Azure mithilfe von Site Recovery](site-recovery-vmware-to-azure.md)
1. [Entwerfen Ihres Netzwerks für die Notfallwiederherstellung](site-recovery-network-design.md)
1. [Testfailover in Azure in Site Recovery](./site-recovery-test-failover-to-azure.md)
1. [Failover in Site Recovery](site-recovery-failover.md)
1. Replizieren eines Domänencontrollers unter [Schützen von Active Directory und DNS mit Azure Site Recovery](site-recovery-active-directory.md)
1. Replizieren von SQL Server unter [Schützen von SQL Server mit der Notfallwiederherstellung von SQL Server und Azure Site Recovery](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Bereitstellungsmuster
Für eine IIS-basierte Webanwendung wird in der Regel eines der folgenden Bereitstellungsmuster verwendet:

**Bereitstellungsmuster 1** Eine IIS-basierte Webfarm mit Routing von Anwendungsanforderungen (Application Request Routing, ARR), IIS-Server und Microsoft SQL Server.

![Bereitstellungsmuster](./media/site-recovery-iis/deployment-pattern1.png)

**Bereitstellungsmuster 2** Eine IIS-basierte Webfarm mit Routing von Anwendungsanforderungen (Application Request Routing, ARR), IIS-Server, Anwendungsserver und Microsoft SQL Server.


![Bereitstellungsmuster](./media/site-recovery-iis/deployment-pattern2.png)

## <a name="site-recovery-support"></a>Site Recovery-Unterstützung

Für die Erstellung dieses Artikels wurden virtuelle VMware-Computer mit Version 7.5 des IIS-Servers unter Windows Server 2012 R2 Enterprise verwendet. Da die Site Recovery-Replikation anwendungsagnostisch ist, ist zu erwarten, dass die hier angegebenen Empfehlungen auch für nachfolgende Szenarien und verschiedene Versionen von IIS gelten.

### <a name="source-and-target"></a>Quelle und Ziel

**Szenario** | **Am sekundären Standort** | **In Azure**
--- | --- | ---
**Hyper-V** | Ja | Ja
**VMware** | Ja | Ja
**Physischer Server** | Nein | Ja

## <a name="replicate-virtual-machines"></a>Replizieren von virtuellen Computern

Befolgen Sie [diese Anleitung](site-recovery-vmware-to-azure.md), um mit dem Replizieren aller virtuellen Computer der IIS-Webfarm in Azure zu beginnen.

Geben Sie bei Verwendung einer statischen IP-Adresse dann die IP an, die vom virtuellen Computer in den Compute- und Netzwerkeinstellungen unter [**Ziel-IP**](./site-recovery-replicate-vmware-to-azure.md#view-and-manage-vm-properties) genutzt werden soll.

![Ziel-IP](./media/site-recovery-active-directory/dns-target-ip.png)


## <a name="creating-a-recovery-plan"></a>Erstellen eines Wiederherstellungsplans

Ein Wiederherstellungsplan ermöglicht die Sequenzierung des Failovers verschiedener Ebenen in einer Anwendung mit mehreren Ebenen, damit die Anwendungskonsistenz gewahrt wird. Führen Sie die unten angegebenen Schritte aus, während Sie einen Wiederherstellungsplan für eine Webanwendung mit mehreren Ebenen erstellen.  Lesen Sie die [weiteren Informationen](./site-recovery-create-recovery-plans.md) zur Erstellung eines Wiederherstellungsplans.

### <a name="adding-virtual-machines-to-failover-groups"></a>Hinzufügen von virtuellen Computern zu Failovergruppen
Eine typische IIS-Webanwendung mit mehreren Ebenen besteht aus einer Datenbankebene mit virtuellen SQL-Computern, der auf einem IIS-Server basierenden Webebene und einer Anwendungsebene. Fügen Sie diese virtuellen Computer je nach Ebene wie unten dargestellt unterschiedlichen Gruppen hinzu. [Erfahren Sie mehr](site-recovery-runbook-automation.md#customize-the-recovery-plan) über das Anpassen von Wiederherstellungsplänen.

1. Erstellen Sie einen Wiederherstellungsplan. Fügen Sie die virtuellen Computer der Datenbankebene unter Gruppe 1 hinzu, um sicherzustellen, dass sie zuletzt heruntergefahren und zuerst hochgefahren werden.

1. Fügen Sie die virtuellen Computer der Anwendungsebene unter Gruppe 2 hinzu, damit sie hochgefahren werden, nachdem die Datenbankebene hochgefahren wurde.

1. Fügen Sie die virtuellen Computer der Webebene unter Gruppe 3 hinzu, damit sie hochgefahren werden, nachdem die Anwendungsebene hochgefahren wurde.

1. Fügen Sie die virtuellen Computer des Lastenausgleichs unter Gruppe 4 hinzu, damit sie hochgefahren werden, nachdem die Webebene hochgefahren wurde.


### <a name="adding-scripts-to-the-recovery-plan"></a>Hinzufügen von Skripts zum Wiederherstellungsplan
Es kann erforderlich sein, nach dem Failover bzw. Testfailover einige Vorgänge auf den virtuellen Azure-Computern durchzuführen, damit die IIS-Webfarm richtig funktioniert. Sie können den Vorgang nach dem Failover, z.B. das Aktualisieren des DNS-Eintrags, Ändern der Websitebindung, Ändern der Verbindungszeichenfolge, automatisieren, indem Sie dem Wiederherstellungsplan wie unten dargestellt entsprechende Skripts hinzufügen. [Erfahren Sie mehr zum Hinzufügen von Skripts zum Wiederherstellungsplan](./site-recovery-create-recovery-plans.md#add-scripts).

#### <a name="dns-update"></a>DNS-Update
Wenn das DNS für das dynamische DNS-Update konfiguriert ist, führen virtuelle Computer nach dem Starten normalerweise ein Update des DNS mit der neuen IP-Adresse durch. Falls Sie einen expliziten Schritt zum Aktualisieren des DNS mit den neuen IPs der virtuellen Computer hinzufügen möchten, können Sie dieses [Skript zum Aktualisieren der IP-Adresse im DNS](https://aka.ms/asr-dns-update) als nachfolgende Aktion in Wiederherstellungsplangruppen hinzufügen.  

#### <a name="connection-string-in-an-applications-webconfig"></a>Verbindungszeichenfolge in der „web.config“ einer Anwendung
Die Verbindungszeichenfolge gibt die Datenbank an, mit der die Website kommuniziert.

Wenn die Verbindungszeichenfolge den Namen des virtuellen Datenbankcomputers hat, sind nach dem Failover keine weiteren Schritte erforderlich, und die Anwendung kann automatisch mit der DB kommunizieren. Wenn die IP-Adresse für den virtuellen Datenbankcomputer beibehalten wird, wird sie für die Aktualisierung der Verbindungszeichenfolge nicht benötigt. Falls die Verbindungszeichenfolge auf den virtuellen Datenbankcomputer mit einer IP-Adresse verweist, muss sie nach dem Failover aktualisiert werden. Beispiel: Die unten angegebene Verbindungszeichenfolge verweist auf die Datenbank mit der IP-Adresse 127.0.1.2.

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
        <connectionStrings>
        <add name="ConnStringDb1" connectionString="Data Source= 127.0.1.2\SqlExpress; Initial Catalog=TestDB1;Integrated Security=False;" />
        </connectionStrings>
        </configuration>

Sie können die Verbindungszeichenfolge auf der Webebene aktualisieren, indem Sie das [Skript für das IIS-Verbindungsupdate](https://aka.ms/asr-update-webtier-script-classic) nach Gruppe 3 im Wiederherstellungsplan hinzufügen.

#### <a name="site-bindings-for-the-application"></a>Websitebindungen für die Anwendung
Jede Website besteht aus Bindungsinformationen mit dem Typ der Bindung, der IP-Adresse, unter der der IIS-Server auf Anforderungen für die Website lauscht, der Portnummer und den Hostnamen für die Website. Bei einem Failover müssen diese Bindungen aktualisiert werden, wenn sich die zugeordneten IP-Adressen ändern.

> [!NOTE]
>
> Wenn Sie für die Sitebindung wie im Beispiel unten die Option „Keine zugewiesen“ gewählt haben, müssen Sie diese Bindung nach dem Failover nicht aktualisieren. Falls die einer Website zugeordnete IP-Adresse nach dem Failover nicht geändert wird, muss die Websitebindung außerdem nicht aktualisiert werden. (Die Beibehaltung der IP-Adresse hängt von der Netzwerkarchitektur und den Subnetzen ab, die dem primären Standort und den Wiederherstellungsstandorten zugewiesen sind. Dies kann für Ihre Organisation also machbar oder auch nicht machbar sein.)

![SSL-Bindung](./media/site-recovery-iis/sslbinding.png)

Wenn Sie die IP-Adresse einer Website zugeordnet haben, müssen Sie alle Websitebindungen mit der neuen IP-Adresse aktualisieren. Sie können das [Skript zum Aktualisieren der IIS-Webebene](https://aka.ms/asr-web-tier-update-runbook-classic) nach Gruppe 3 im Wiederherstellungsplan hinzufügen, um die Websitebindungen zu ändern.


#### <a name="update-load-balancer-ip-address"></a>Aktualisieren der IP-Adresse des Lastenausgleichs
Wenn Sie über einen virtuellen ARR-Computer (Application Request Routing, Routing von Anwendungsanforderungen) verfügen, fügen Sie das [IIS-ARR-Failoverskript](https://aka.ms/asr-iis-arrtier-failover-script-classic) nach Gruppe 4 hinzu, um die IP-Adresse zu aktualisieren.

#### <a name="the-ssl-cert-binding-for-an-https-connection"></a>SSL-Zertifikatbindung für eine HTTPS-Verbindung
Websites können über ein zugeordnetes SSL-Zertifikat verfügen, mit dem sichergestellt werden kann, dass zwischen dem Webserver und dem Browser des Benutzers eine sichere Kommunikation erfolgt. Falls für die Website eine HTTPS-Verbindung und eine zugeordnete HTTPS-Websitebindung mit der IP-Adresse des IIS-Servers mit einer SSL-Zertifikatbindung verwendet werden, muss für das Zertifikat mit der IP-Adresse des virtuellen IIS-Computers nach dem Failover eine neue Websitebindung hinzugefügt werden.

Das SSL-Zertifikat kann für Folgendes ausgestellt werden:

a) Vollqualifizierter Domänenname der Website<br>
b) Name des Servers<br>
c) Platzhalterzertifikat für den Domänennamen<br>
d) IP-Adresse: Wenn das SSL-Zertifikat für die IP-Adresse des IIS-Servers ausgestellt wird, muss ein weiteres SSL-Zertifikat für die IP-Adresse des IIS-Servers am Azure-Standort ausgestellt werden, und es muss eine zusätzliche SSL-Bindung für dieses Zertifikat erstellt werden. Daher ist es ratsam, kein für die IP-Adresse ausgestelltes SSL-Zertifikat zu verwenden. Dies ist eine weniger häufig genutzte Option, die in Kürze im Rahmen der neuen ZS-/Browser-Forumsänderungen als veraltet eingestuft werden.

#### <a name="update-the-dependency-between-the-web-and-the-application-tier"></a>Aktualisieren der Abhängigkeit zwischen Web- und Anwendungsebene
Wenn Sie über eine anwendungsspezifische Abhängigkeit verfügen, die auf der IP-Adresse der virtuellen Computer basiert, müssen Sie diese Abhängigkeit nach dem Failover aktualisieren.

## <a name="doing-a-test-failover"></a>Durchführen eines Testfailovers
Befolgen Sie [diese Anleitung](site-recovery-test-failover-to-azure.md), um ein Testfailover durchzuführen.

1.  Wechseln Sie zum Azure-Portal, und wählen Sie Ihren Recovery Services-Tresor aus.
1.  Klicken Sie auf den Wiederherstellungsplan, der für die IIS-Webfarm erstellt wurde.
1.  Klicken Sie auf „Testfailover“.
1.  Wählen Sie den Wiederherstellungspunkt und das virtuelle Azure-Netzwerk aus, um den Prozess für das Testfailover zu starten.
1.  Wenn die sekundäre Umgebung aktiv ist, können Sie Ihre Überprüfungen durchführen.
1.  Nachdem die Überprüfungen abgeschlossen wurden, können Sie die Option „Validations complete“ (Überprüfungen abgeschlossen) auswählen. Die Umgebung für das Testfailover wird dann bereinigt.

## <a name="doing-a-failover"></a>Durchführen eines Failovers
Befolgen Sie [diese Anleitung](site-recovery-failover.md), wenn Sie ein Failover durchführen.

1.  Wechseln Sie zum Azure-Portal, und wählen Sie Ihren Recovery Services-Tresor aus.
1.  Klicken Sie auf den Wiederherstellungsplan, der für die IIS-Webfarm erstellt wurde.
1.  Klicken Sie auf „Failover“.
1.  Wählen Sie einen Wiederherstellungspunkt aus, um den Failoverprozess zu starten.

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich weiter über das [Replizieren anderer Anwendungen](site-recovery-workload.md) mit Site Recovery.

