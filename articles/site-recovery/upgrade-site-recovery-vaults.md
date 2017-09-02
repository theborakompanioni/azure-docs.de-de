---
title: "Durchführen eines Upgrades des Site Recovery-Tresors auf einen Azure Recovery Services-Tresor"
description: "Erfahren Sie, wie Sie ein Upgrade des Azure Site Recovery-Tresors auf einen Recovery Services-Tresor durchführen."
documentationcenter: 
author: rajani-janaki-ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/31/2017
ms.author: rajani-janaki-ram
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: fdb33ea0d08353b491f2934fcf885fcb6910b9a2
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="upgrade-a-site-recovery-vault-to-an-azure-resource-manager-based-recovery-services-vault"></a>Durchführen eines Upgrades des Site Recovery-Tresors auf einen Azure Resource Manager-basierten Recovery Services-Tresor

In diesem Artikel wird beschrieben, wie Sie ein Upgrade von Azure Site Recovery-Tresoren auf Azure Resource Manager-basierte Recovery Services-Tresore ohne Beeinträchtigung der laufenden Replikation durchführen. Weitere Informationen zu den Features und Vorteilen von Azure Resource Manager finden Sie unter [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="introduction"></a>Einführung
Ein Recovery Services-Tresor ist eine Azure Resource Manager-Ressource, mit der Sie Sicherungen und die Notfallwiederherstellung nativ in der Cloud verwalten können. Dieser einheitliche Tresor kann im neuen Azure-Portal als Ersatz für klassische Sicherungs- und Site Recovery-Tresore verwendet werden.

Recovery Services-Tresore bieten Ihnen eine Fülle an Funktionen wie beispielsweise folgende:

* Unterstützung für Azure Resource Manager: Sie können Ihre virtuellen und physischen Computer schützen und ein Failover im Azure Resource Manager-Stapel durchführen.

* Ausschließen von Datenträgern: Wenn Sie temporäre Dateien oder Daten mit großen Änderungen besitzen, für die Sie nicht die gesamte Bandbreite verbrauchen möchten, können Sie Volumes von der Replikation ausschließen. Diese Funktion wurde gegenwärtig für Szenarien wie *VMware zu Azure* und *Hyper-V zu Azure* aktiviert und wird auch auf andere Szenarien erweitert.

* Unterstützung für Storage Premium und lokal redundanten Speicher: Ab sofort können Sie Server in Storage Premium-Konten schützen, mit denen Kunden Anwendungen mit höheren IOPS-Werten (Input/Output Operations Per Second, Eingabe-/Ausgabevorgänge pro Sekunde) schützen können. Diese Funktion ist gegenwärtig im Szenario *VMware zu Azure* aktiviert.

* Optimierte Benutzeroberfläche „Erste Schritte“: Die verbesserte Benutzeroberfläche „Erste Schritte“ wurde dazu konzipiert, die Einrichtung der Notfallwiederherstellung zu vereinfachen.

* Backup- und Site Recovery-Verwaltung über denselben Tresor: Nun können Sie über denselben Tresor Server für die Notfallwiederherstellung schützen oder Sicherungen durchführen. Hierdurch kann der Verwaltungsaufwand deutlich reduziert werden.

Weitere Informationen zur verbesserten Benutzererfahrung und zu den verbesserten Funktionen finden Sie im [Blog zu Speicherung, Sicherung und Wiederherstellung](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).

## <a name="salient-features"></a>Highlights

* Keine Auswirkung auf die laufende Replikation: Laufende Replikationen werden während und nach dem Upgrade ohne Unterbrechung fortgesetzt.

* Keine zusätzlichen Kosten: Sie erhalten einen ganzen Satz aktualisierter Funktionen ohne zusätzliche Kosten.

* Kein Datenverlust: Da es sich bei diesem Prozess um ein Upgrade und nicht um eine Migration handelt, werden vorhandene Wiederherstellungspunkte und Einstellungen für die Replikation während und nach dem Upgrade beibehalten.


## <a name="what-happens-during-the-vault-upgrade"></a>Was geschieht während des Tresorupgrades?

Während des Upgrades können keine Vorgänge (etwa Registrieren eines neuen Servers oder Aktivieren der Replikation für einen virtuellen Computer) ausgeführt werden. Vorgänge, bei denen Daten aus dem Tresor gelesen bzw. in den Tresor geschrieben werden, werden ohne Unterbrechung fortgesetzt. Dies ist beispielsweise bei der laufenden Replikation von geschützten Elementen im Tresor der Fall.

### <a name="changes-to-automation-and-tooling-after-the-upgrade"></a>Änderungen an der Automatisierung und an den Tools nach dem Upgrade
Beim Upgrade Ihres Tresortyps vom klassischen auf das Resource Manager-Bereitstellungsmodell aktualisieren Sie Ihre bestehende Automatisierung oder die bestehenden Tools, um sicherzustellen, dass sie nach dem Upgrade weiterhin funktionieren.

### <a name="prepare-your-environment-for-the-upgrade"></a>Vorbereiten der Umgebung auf das Upgrade

* [Installieren von PowerShell oder Upgrade von PowerShell auf Version 5 oder höher](https://www.microsoft.com/download/details.aspx?id=50395)
* [Installieren der neuesten Version von Azure PowerShell MSI](https://github.com/Azure/azure-powershell/releases)
* [Herunterladen des Upgradeskripts für den Recovery Services-Tresor](https://aka.ms/vaultupgradescript)

### <a name="prerequisites"></a>Voraussetzungen
Um ein Upgrade für Ihre Site Recovery-Tresore auf Azure Resource Manager-basierte Recovery Service-Tresore durchzuführen, müssen folgende Voraussetzungen erfüllt sein:

* Mindestens erforderliche Agent-Version: Es muss mindestens Version 5.1.1700.0 des Azure Site Recovery-Anbieters auf Ihrem Server installiert sein.

* Unterstützte Konfiguration: Sie können den Tresor nicht mit einem Storage Area Network (SAN) oder SQL Server AlwaysOn-Verfügbarkeitsgruppen konfigurieren. Alle anderen Konfigurationen werden unterstützt.

    >[!NOTE]
    >Nach dem Upgrade können Sie die Speicherzuordnung nur über PowerShell verwalten.

* Unterstütztes Bereitstellungsszenario: Ihr Tresor darf nicht im Legacybereitstellungsmodell *VMware zu Azure* konfiguriert sein. Bevor Sie fortfahren, migrieren Sie zunächst zum verbesserten Bereitstellungsmodell.

* Keine aktiven, benutzerinitiierten Aufträge, die Vorgänge auf der Verwaltungsebene umfassen: Da der Zugriff auf die Verwaltungsebene während des Upgrades beschränkt ist, müssen Sie alle Aktionen auf der Verwaltungsebene abschließen, bevor Sie das Upgrade auslösen. Laufende Replikationen sind von diesem Prozess nicht betroffen.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Wirkt sich dieses Upgrade auf meine laufende Replikation aus?**

Nein. Ihre laufende Replikation wird während und nach dem Upgrade ohne Unterbrechung fortgesetzt.

**Was geschieht mit den Netzwerkeinstellungen, wie etwa den Einstellungen für das Site-to-Site-VPN und den IP-Einstellungen?**

Das Upgrade wirkt sich nicht auf die Netzwerkeinstellungen aus. Alle Verbindungen zwischen Azure und lokalen Standorten bleiben erhalten.

**Was geschieht mit meinen Tresoren, wenn ich in Kürze kein Upgrade durchführe?**

Die Unterstützung für Site Recovery-Tresore im alten Azure-Portal gilt ab September 2017 als veraltet. Daher empfehlen wir ausdrücklich, die Upgradefunktion zur Migration zum neuen Portal zu verwenden.

**Was bedeutet dieser Migrationsplan für meine vorhandenen Tools?**  

Die Aktualisierung Ihrer Tools auf das Resource Manager-Bereitstellungsmodell ist eine der wichtigsten Änderungen, die Sie in Ihren Upgradeplänen berücksichtigen sollten. Recovery Services-Tresore basieren auf dem Resource Manager-Bereitstellungsmodell. 

**Wie lange dauert die Downtime der Verwaltungsebene?**

Das Upgrade dauert in der Regel ca. 15 bis 30 Minuten, kann jedoch bis maximal eine Stunde dauern.

**Kann ich nach dem Upgrade ein Rollback ausführen?**

Nein. Ein Rollback wird nicht unterstützt, nachdem erfolgreich ein Upgrade für die Ressourcen durchgeführt wurde.

**Kann ich mein Abonnement oder meine Ressourcen überprüfen, um zu ermitteln, ob sie aktualisiert werden können?**

Ja. Bei der Option für das plattformgestützte Upgrade besteht der erste Schritt darin, zu überprüfen, ob die Ressourcen für das Upgrade geeignet sind. Falls bei der Überprüfung ein Fehler auftritt, werden entsprechende Fehlermeldungen oder Warnungen ausgegeben.

**Wie melde ich ein Problem in Bezug auf das Upgrade?**

Wenn während des Upgrades ein Fehler auftritt, notieren Sie die Vorgangs-ID aus der Fehlermeldung. Der Microsoft-Support arbeitet proaktiv an der Behebung des Problems. Sie können sich auch unter Angabe Ihrer Abonnement-ID, des Tresornamens und der Vorgangs-ID an das Supportteam wenden. Der Support versucht, das Problem so schnell wie möglich zu beheben. Wiederholen Sie den Vorgang nicht, es sei denn, Sie werden von Microsoft explizit dazu aufgefordert.

## <a name="run-the-script"></a>Ausführen des Skripts

Führen Sie in PowerShell den folgenden Befehl aus:

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionID <subscriptionID>  -VaultName <vaultname> -Location <location> -ResourceType HyperVRecoveryManagerVault -TargetResourceGroupName <rgname>

* SubscriptionId: Die mit dem Tresor verknüpfte Abonnement-ID, für den das Upgrade durchgeführt wird

* VaultName: Der Name des Tresors, für den das Upgrade durchgeführt wird

* Location: Der Standort des Tresors, für den das Upgrade durchgeführt wird

* ResourceType: HyperVRecoveryManagerVault für Site Recovery-Tresore

* TargetResourceGroupName: Die Ressourcengruppe, in der der aktualisierte Tresor platziert werden sollen. Bei „TargetResourceGroupName“ kann es sich um eine vorhandene Ressourcengruppe in Azure Resource Manager oder eine neue Ressourcengruppe handeln. Falls die angegebene Ressourcengruppe „TargetResourceGroupName“ nicht vorhanden ist, wird sie im Rahmen des Upgrades am selben Speicherort wie der Tresor erstellt. Weitere Informationen finden Sie unter [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups) im Abschnitt „Ressourcengruppen“.

    >[!NOTE]
    >Beim Benennen von Ressourcengruppen gelten bestimmte Einschränkungen. Um einen Fehler beim Tresorupgrade zu verhindern, halten Sie sich unbedingt an die Benennungskonvention.
    >
    >Beispiel:
    >
    >.\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionId 1234-54123-354354-56416-8645 -VaultName gen2dr -Location "north europe" -ResourceType hypervrecoverymanagervault -TargetResourceGroupName abc

Alternativ können Sie das folgende Skript ausführen. Geben Sie die Werte für die erforderlichen Parameter ein.

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1
    cmdlet RecoveryServicesVaultUpgrade-1.0.0.ps1 at command pipeline position 1

    Supply values for the following parameters:
    SubscriptionId:
    VaultName:
    Location:
    ResourceType:
    TargetResourceGroupName:

1. Das PowerShell-Skript fordert Sie zur Eingabe Ihrer Anmeldeinformationen auf. Geben Sie sie zweimal ein: einmal für das Konto des klassischen Bereitstellungsmodells und ein zweites Mal für das Azure Resource Manager-Konto.

2. Nachdem Sie Ihre Anmeldeinformationen eingegeben haben, überprüft das Skript, ob Ihre Infrastruktureinrichtung die zuvor genannten Voraussetzungen erfüllt.

3. Nachdem die Voraussetzungen überprüft und bestätigt wurden, werden Sie dazu aufgefordert, das Tresorupgrade fortzusetzen. Der Prozess zur Durchführung des Tresorupgrades wird gestartet. Das gesamte Upgrade kann 15 bis 30 Minuten dauern.

4. Nach dem erfolgreichen Abschluss des Upgrades können Sie im neuen Azure-Portal auf den aktualisierten Tresor zugreifen.

## <a name="post-upgrade-vault-management"></a>Tresorverwaltung nach dem Upgrade

### <a name="replicate-by-using-azure-site-recovery-in-the-recovery-services-vault"></a>Replizieren mithilfe von Azure Site Recovery im Recovery Services-Tresor

* Nun können Sie Ihre Azure-VMs regionsübergreifend schützen. Weitere Informationen finden Sie unter [Replizieren von Azure-VMs zwischen Regionen mit Azure Site Recovery](site-recovery-azure-to-azure.md).

* Weitere Informationen zum Replizieren virtueller VMware-Computer in Azure finden Sie unter [Replizieren von VMware-VMs in Azure mit Site Recovery](vmware-walkthrough-overview.md).

* Weitere Informationen zum Replizieren virtueller Hyper-V-Computer (ohne VMM) in Azure finden Sie unter [Replizieren von Hyper-V-VMs (ohne VMM) in Azure](hyper-v-site-walkthrough-overview.md).

* Weitere Informationen zum Replizieren virtueller Hyper-V-Computer (mit VMM) in Azure finden Sie unter [Replizieren von virtuellen Hyper-V-Computern in VMM-Clouds in Azure per Site Recovery im Azure-Portal](vmm-to-azure-walkthrough-overview.md).

* Weitere Informationen zum Replizieren virtueller Hyper-V-Computer (mit VMM) an einem sekundären Standort finden Sie unter [Replizieren von virtuellen Hyper-V-Computern in VMM-Clouds an einen sekundären VMM-Standort mithilfe des Azure-Portals](site-recovery-vmm-to-vmm.md).

* Weitere Informationen zum Replizieren virtueller VMware-Computer an einem sekundären Standort finden Sie unter [Replizieren lokaler virtueller VMware-Computer oder physischer Server an einen sekundären Standort im klassischen Azure-Portal](site-recovery-vmware-to-vmware.md).

### <a name="view-your-replicated-items"></a>Anzeigen replizierter Elemente

In der folgenden Abbildung sehen Sie die Dashboardseite des Recovery Services-Tresors mit wichtigen Entitäten für den Tresor. Klicken Sie auf **Site Recovery** > **Replizierte Elemente**, um eine Liste der geschützten Entitäten im Tresor anzuzeigen.


![Replizierte Elemente](./media/upgrade-site-recovery-vaults/replicateditems.png)

Die folgende Abbildung zeigt den Workflow zum Anzeigen der replizierten Elemente sowie den **Failover**-Befehl zum Initiieren eines Failovers.

![Replizierte Elemente](./media/upgrade-site-recovery-vaults/failover.png)

### <a name="view-your-replication-settings"></a>Anzeigen der Replikationseinstellungen

Im Site Recovery-Tresor ist jede Schutzgruppe mit Kopierhäufigkeit, Aufbewahrung für Wiederherstellungspunkte, Häufigkeit der Momentaufnahmen für Anwendungskonsistenz und anderen Replikationseinstellungen konfiguriert. Im Recovery Services-Tresor sind diese Einstellungen als Replikationsrichtlinie konfiguriert. Der Name der Richtlinie ist der Name der Schutzgruppe oder *primarycloud_Policy*.

Weitere Informationen zur Replikationsrichtlinie finden Sie unter [Verwalten der Replikationsrichtlinie für VMware in Azure](site-recovery-setup-replication-settings-vmware.md).

