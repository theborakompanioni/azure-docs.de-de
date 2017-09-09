---
title: Replizieren einer Dynamics AX-Bereitstellung mit mehreren Schichten mit Azure Site Recovery | Microsoft-Dokumentation
description: "In diesem Artikel wird beschrieben, wie Sie Dynamics AX mit Azure Site Recovery (ASR) replizieren und schützen können."
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/24/2017
ms.author: asgang
ms.translationtype: HT
ms.sourcegitcommit: 7456da29aa07372156f2b9c08ab83626dab7cc45
ms.openlocfilehash: 03127c8f4841b67436c4819628319705af0b2cd5
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="replicate-a-multi-tier-dynamics-ax-application-using-azure-site-recovery"></a>Replizieren einer Dynamics AX-Anwendung mit mehreren Schichten mit Azure Site Recovery

## <a name="overview"></a>Übersicht


Microsoft Dynamics AX ist eine der bei Unternehmen am beliebtesten ERP-Lösungen zur standortübergreifenden Standardisierung von Prozessen, zum Verwalten von Ressourcen und zum Vereinfachen der Konformität. Da die Anwendung für eine Organisation unternehmenskritisch ist, ist es von höchster Wichtigkeit, dass die Anwendung in einem Notfall schnellstmöglich wieder in Betrieb genommen werden kann.

Microsoft Dynamics AX bietet derzeit keine vorgefertigten Funktionen zur Notfallwiederherstellung. Microsoft Dynamics AX besteht aus vielen Serverkomponenten wie z.B. Application Object Server, Active Directory (AD), SQL Database Server, SharePoint Server, Reporting Server usw. Das manuelle Verwalten der Notfallwiederherstellung jeder Komponente ist nicht nur teuer, sondern auch fehleranfällig.

In diesem Artikel wird ausführlich erklärt, wie Sie eine Lösung zur Notfallwiederherstellung für Ihre Dynamics AX-Anwendung mithilfe von [Azure Site Recovery](site-recovery-overview.md) (ASR) erstellen können. Außerdem werden geplante/nicht geplante/Testfailover mit einem durch einen Klick aktivierbaren Wiederherstellungsplan, unterstützten Konfigurationen und Voraussetzungen behandelt.
Die Lösung zur Notfallwiederherstellung mit Azure Site Recovery wurde vollständig geprüft und zertifiziert und wird von Microsoft Dynamics AX empfohlen.



## <a name="prerequisites"></a>Voraussetzungen

Das Implementieren der Notfallwiederherstellung für Dynamics AX mit Azure Site Recovery erfordert folgende Voraussetzungen.

• Eine lokale Bereitstellung von Dynamics AX wurde eingerichtet.

• Ein Azure Site Recovery Services-Tresor wurde in einem Microsoft Azure-Abonnement erstellt.

• Falls Azure Ihr Wiederherstellungsstandort ist, führen Sie zusätzlich das Bereitschaftsbewertungstool für virtuelle Azure-Computer auf VMs aus, um sicherzustellen, dass diese mit Azure-VMs und Azure Site Recovery Services kompatibel sind.


## <a name="site-recovery-support"></a>Site Recovery-Unterstützung

Für die Erstellung dieses Artikels wurden virtuelle VMware-Computer mit Dynamics AX 2012R3 unter Windows Server 2012 R2 Enterprise verwendet. Da die Site Recovery-Replikation anwendungsunabhängig ist, gelten die hier aufgezeigten Empfehlungen voraussichtlich auch für die folgenden Szenarios:

### <a name="source-and-target"></a>Quelle und Ziel

**Szenario** | **Am sekundären Standort** | **In Azure**
--- | --- | ---
**Hyper-V** | Ja | Ja
**VMware** | Ja | Ja
**Physischer Server** | Ja | Ja

## <a name="enable-dr-of-dynamics-ax-application-using-azure-site-recovery"></a>Aktivieren der Notfallwiederherstellung der Dynamics AX-Anwendung mit Azure Site Recovery
### <a name="protect-your-dynamics-ax-application"></a>Schützen Sie Ihre Dynamics AX-Anwendung
Jede Komponente von Dynamics AX muss geschützt werden, um die vollständige Replikation und Wiederherstellung der Anwendung zu aktivieren. Dieser Abschnitt behandelt:

**1. Schutz von Active Directory**

**2. Schutz der SQL-Schicht**

**3. Schutz der Anwendungs- und Webschicht**

**4. Netzwerkkonfiguration**

**5. Wiederherstellungsplan**

### <a name="1-setup-ad-and-dns-replication"></a>1. Einrichten der Replikation mit AD und DNS

Damit die Dynamics AX-Anwendung funktioniert, muss AD am Standort der Notfallwiederherstellung vorhanden sein. Basierend auf der Komplexität der lokalen Umgebung des Kunden gibt es zwei empfohlene Optionen.

**Option 1:**

Falls der Kunde über eine geringe Anzahl von Anwendungen und über einen einzelnen Domänencontroller für die gesamte lokale Umgebung verfügt und ein Failover für den gesamten Standorts gleichzeitig ausführt, wird die Verwendung der Azure Site Recovery-Replikation zum Replizieren des Computers mit dem Domänencontroller an einen sekundären Standort empfohlen (gilt sowohl für die Standort-zu-Standort- als auch für die Standort-zu-Azure-Replikation).

**Option 2**

Wenn der Kunde über eine hohe Anzahl von Anwendungen verfügt und eine Active Directory-Gesamtstruktur und ein Failover für einige Anwendungen gleichzeitig ausführt, wird die Einrichtung eines zusätzlichen Domänencontrollers am DR-Standort empfohlen (ein sekundärer Standort oder in Azure).

Halten Sie sich dafür an das [Begleithandbuch zum Verfügbarmachen eines Domänencontrollers am Standort einer Notfallwiederherstellung](site-recovery-active-directory.md). Für den Rest dieses Dokuments wird davon ausgegangen, dass ein Domänencontroller am Standort der Notfallwiederherstellung verfügbar ist.

### <a name="2-setup-sql-server-replication"></a>2. Einrichten der SQL Server-Replikation
Wenden Sie sich für eine ausführlichere technische Anleitung zur empfohlenen Option zum Schutz der [SQL-Schicht](site-recovery-sql.md) an das Begleithandbuch.

### <a name="3-enable-protection-for-dynamics-ax-client-and-aos-vms"></a>3. Aktivieren des Schutz für den Dynamics AX-Client und AOS-VMs
Nehmen Sie relevante Konfigurationen für ASR durch, je nachdem wo die VMs bereitgestellt wurden – auf [Hyper-V](site-recovery-hyper-v-site-to-azure.md) oder auf [VMware](site-recovery-vmware-to-azure.md).

> [!TIP]
> Die empfohlene Frequenz der Absturzkonsistenz beträgt 15 Minuten.
>

Die unten stehende Abbildung zeigt den Schutzstatus der Komponenten-VMs von Dynamics AX im Schutzszenario „VMware-Standort zu Azure“.
![Geschützte Elemente](./media/site-recovery-dynamics-ax/protecteditems.png)

### <a name="4-configure-networking"></a>4. Konfigurieren Sie die Netzwerkeinstellungen
Konfigurieren der Netzwerkeinstellungen für Compute und Netzwerk

Konfigurieren Sie die Netzwerkeinstellungen für den AX-Client und die AOS-VMs in Azure Site Recovery so, dass die VM-Netzwerke nach dem Failover mit dem richtigen Notfallwiederherstellungsnetzwerk verbunden werden. Stellen Sie sicher, dass das Netzwerk der Notfallwiederherstellung auf der SQL-Schicht geroutet werden kann.

Sie können den virtuellen Computer in den replizierten Elemente auswählen, um die Netzwerkeinstellungen wie in der folgenden Abbildung gezeigt zu konfigurieren.

* Wählen Sie für AOS-Server die richtige Verfügbarkeitsgruppe aus.

* Geben Sie bei Verwendung einer statischen IP-Adresse die IP an, die vom virtuellen Computer im **Ziel-IP**-Feld ![Netzwerkeinstellungen](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png) genutzt werden soll.



### <a name="5-creating-a-recovery-plan"></a>5. Erstellen eines Wiederherstellungsplans

Sie können einen Wiederherstellungsplan in Azure Site Recovery erstellen, um den Failoverprozess zu automatisieren. Fügen Sie dem Wiederherstellungsplan eine Anwendungs- und eine Webschicht hinzu. Sortieren Sie diese nach verschiedenen Gruppen, damit das Front-End vor der Anwendungsschicht beendet wird.

1)  Wählen Sie den Azure Site Recovery-Tresor in Ihrem Abonnement aus, und klicken Sie auf die Kachel „Wiederherstellungspläne“.

2)  Klicken Sie auf „+ Wiederherstellungsplan“, und geben Sie einen Namen ein.

3)  Wählen Sie „Quelle“ und „Ziel“ aus. Das Ziel kann sowohl Azure als auch ein sekundärer Standort sein. Sollten Sie Azure wählen, müssen Sie ein Bereitstellungsmodell angeben

![Wiederherstellungsplan erstellen](./media/site-recovery-dynamics-ax/recoveryplancreation1.png)

4)  Wählen Sie die AOS- und Client-VMs für den Wiederherstellungsplan aus und klicken Sie auf ✓.
![Wiederherstellungsplan erstellen](./media/site-recovery-dynamics-ax/selectvms.png)


![Wiederherstellungsplan](./media/site-recovery-dynamics-ax/recoveryplan.png)

Sie können den Wiederherstellungsplan für die Dynamics AX-Anwendung anpassen, indem Sie Schritte wie unten aufgezeigt hinzufügen. Die oben stehende Abbildung zeigt den vollständigen Wiederherstellungsplan, nachdem Sie alle Schritte hinzugefügt haben.

*Schritte*:

*1. Schritte für das SQL Server-Failover*

Ausführlichere Informationen zu SQL Server-spezifischen Wiederherstellungsschritten finden Sie im Begleithandbuch [SQL Server DR Solution](site-recovery-sql.md) (Notfallwiederherstellungslösung für SQL Server).

*2. Failovergruppe 1: Durchführen eines Failovers für AOS-VMs*

Stellen Sie sicher, dass der gewählte Wiederherstellungspunkt sich so nah wie möglich am PIT der Datenbank befindet.

*3. Skript: Hinzufügen eines Lastenausgleichs (nur E-A)* Fügen Sie über Azure Automation hinter der AOS-VM-Gruppe ein Skript hinzu, um einen Lastenausgleich hinzuzufügen. Sie können ein Skript in diese Aufgabe einfügen. Weitere Informationen finden Sie im Artikel [how to add load balancer for multi-tier application DR (So können Sie einen Lastenausgleich für die Notfallwiederherstellung für Anwendungen mit mehreren Schichten hinzufügen)](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/)

*4. Failovergruppe 2: Durchführen eines Failovers für AX-Client-VMs*
Durchführen eines Failovers für VMs der Webschicht im Rahmen des Wiederherstellungsplans.


### <a name="doing-a-test-failover"></a>Durchführen eines Testfailovers

Halten Sie sich während des Testfailovers an die Begleithandbücher „AD DR solution“ (Notfallwiederherstellungslösung für AD) und „SQL Server DR solution“ (Notfallwiederherstellungslösung für SQL Server) für weitere Überlegungen zu AD bzw. SQL Server.

1.  Wählen Sie im Azure-Portal Ihren Site Recovery-Tresor aus.
2.  Klicken Sie auf den Wiederherstellungsplan, der für Dynamics AX erstellt wurde.
3.  Klicken Sie auf „Testfailover“.
4.  Wählen Sie das virtuelle Netzwerk aus, um den Testfailoverprozess zu starten.
5.  Wenn die sekundäre Umgebung aktiv ist, können Sie Ihre Überprüfungen durchführen.
6.  Nachdem die Überprüfungen abgeschlossen wurden, können Sie die Option „Validations complete“ (Überprüfungen abgeschlossen) auswählen. Die Umgebung für das Testfailover wird dann bereinigt.

Befolgen Sie [diese Anleitung](site-recovery-test-failover-to-azure.md), um ein Testfailover durchzuführen.

### <a name="doing-a-failover"></a>Durchführen eines Failovers

1.  Wählen Sie im Azure-Portal Ihren Site Recovery-Tresor aus.
2.  Klicken Sie auf den Wiederherstellungsplan, der für Dynamics AX erstellt wurde.
3.  Klicken Sie auf „Failover“, und wählen Sie „Failover“ aus.
4.  Wählen Sie das Zielnetzwerk und klicken Sie auf ✓, um mit dem Failoverprozess zu beginnen.

Befolgen Sie [diese Anleitung](site-recovery-failover.md), wenn Sie ein Failover durchführen.

### <a name="perform-a-failback"></a>Durchführen eines Failbacks

Ausführlichere Informationen zu SQL Server-spezifischen Wiederherstellungsschritten finden Sie im Begleithandbuch „SQL Server DR Solution“ (Notfallwiederherstellungslösung für SQL Server).

1.  Wählen Sie im Azure-Portal Ihren Site Recovery-Tresor aus.
2.  Klicken Sie auf den Wiederherstellungsplan, der für Dynamics AX erstellt wurde.
3.  Klicken Sie auf „Failover“, und wählen Sie „Failover“ aus.
4.  Klicken Sie auf „Richtung ändern“.
5.  Wählen Sie die entsprechenden Optionen aus: Datensynchronisierung und Erstellen einer VM.
6.  Klicken Sie auf ✓, um den Failbackprozess zu starten.


Befolgen Sie [diese Anleitung](site-recovery-failback-azure-to-vmware.md), wenn Sie ein Failback durchführen.

##<a name="summary"></a>Zusammenfassung
Mit Azure Site Recovery können Sie für Active Directory einen vollständig automatisierten Notfallwiederherstellungsplan für Ihre Dynamics AX-Anwendung erstellen. Bei einer Störung können Sie das Failover von überall aus in Sekundenschnelle einleiten und die Anwendung binnen weniger Minuten wieder in Betrieb nehmen.

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie [Welche Workloads können mit Azure Site Recovery geschützt werden?](site-recovery-workload.md) , um weitere Informationen über den Schutz von Unternehmensworkloads mit Azure Site Recovery zu erhalten.

