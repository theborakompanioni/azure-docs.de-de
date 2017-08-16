---
title: "Durchführen eines Upgrades des Recovery-Tresors auf einen Recovery Services-Tresor"
description: "Erfahren Sie, wie Sie ein Upgrade des Azure Site Recovery-Tresors auf einen Recovery Services-Tresor durchführen."
ddocumentationcenter: 
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
ms.sourcegitcommit: 14915593f7bfce70d7bf692a15d11f02d107706b
ms.openlocfilehash: 523cab85b195d85007bd85c45dbe3645f7a00ab1
ms.contentlocale: de-de
ms.lasthandoff: 08/10/2017

---
# <a name="upgrade-site-recovery-vaults-to-azure-resource-manager-based-recovery-services-vaults"></a>Durchführen eines Upgrades von Site Recovery-Tresoren auf Azure Resource Manager-basierte Recovery Services-Tresore

In diesem Artikel wird beschrieben, wie Sie ein Upgrade von Site Recovery-Tresoren auf Azure Resource Manager-basierte Recovery Services-Tresore ohne Beeinträchtigung der laufenden Replikation durchführen. Weitere Informationen über die Funktionen und Vorteile von Azure Resource Manager finden Sie [hier](../azure-resource-manager/resource-group-overview.md).

## <a name="introduction"></a>Einführung
Ein Recovery Services-Tresor ist eine Azure Resource Manager-Ressource, mit der Sie Ihre Sicherungen und Szenarien zur Notfallwiederherstellung nativ in der Cloud verwalten können. Dieser einheitliche Tresor kann im neuen Azure-Portal als Ersatz für klassische Sicherungs- und Site Recovery-Tresore verwendet werden.

Recovery Services-Tresore bieten Ihnen eine Fülle an Funktionen wie beispielsweise Folgende:

-   Unterstützung für Azure Resource Manager: Sie können Ihre virtuellen und physischen Computer schützen und ein Failover im Azure Resource Manager-Stapel durchführen.

-   Ausschließen von Datenträgern – Wenn Sie temporäre Dateien oder Daten mit großen Änderungen besitzen, für die Sie keine Bandbreite verbrauchen möchten, können Sie Volumes von der Replikation ausschließen. Diese Funktion wurde gegenwärtig für Szenarien wie „VMware nach Azure“ und „Hyper-V nach Azure“ aktiviert und wird in Kürze auch auf andere Szenarien erweitert.

- Unterstützung für Storage Premium und lokal redundanten Speicher (Locally Redundant Storage, LRS) – Ab sofort können Sie Server in Storage Premium-Konten schützen, mit denen Kunden Anwendungen mit höheren IOPS-Werten schützen können. Diese Funktion wurde gegenwärtig im Szenario „VMware nach Azure“ aktiviert.

-   Optimierte Benutzeroberfläche „Erste Schritte“ – Die verbesserte Benutzeroberfläche „Erste Schritte“ wurde angepasst, um eine reibungslose Einrichtung der Notfallwiederherstellung sicherzustellen.

- Verwalten von Backup- und Site Recovery-Vorgängen über denselben Tresor – Nun können Sie über denselben Tresor Server für die Notfallwiederherstellung schützen oder Sicherungen durchführen. Hierdurch wird der Verwaltungsaufwand deutlich reduziert.

Weitere Details in Bezug auf die verbesserte Benutzererfahrung und die verbesserten Funktionen finden Sie in [diesem Blog](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).

## <a name="salient-features"></a>Highlights

- **Keine Auswirkung auf die laufende Replikation**: Laufende Replikationen werden während und nach dem Upgrade ohne Unterbrechung fortgesetzt.

- **Keine zusätzlichen Kosten** – Sie profitieren von einer ganzen Reihe neuer Funktionen, ohne zusätzliche Kosten in Kauf nehmen zu müssen.

- **Kein Datenverlust** – Da es sich um ein Upgrade und nicht um eine Migration handelt, werden Replikationsinformationen (Wiederherstellungspunkte, Replikationseinstellungen etc.) während und nach dem Upgrade beibehalten.


## <a name="what-happens-during-the-upgrade"></a>Was geschieht während des Upgrades?

Vorgänge wie das Registrieren eines neuen Servers, das Aktivieren der Replikation für eine VM sind während des Upgrades nicht zulässig. Vorgänge, bei denen lediglich Daten in den Tresor geschrieben bzw. aus dem Tresor gelesen werden, werden ohne Unterbrechung fortgesetzt. Dies ist beispielsweise bei der laufenden Replikation von geschützten Elementen im Tresor der Fall.

## <a name="changes-to-your-automation-and-tooling-after-vault-upgrade"></a>Änderungen an der Automatisierung und an den Tools nach dem Tresorupgrade
Im Rahmen des Upgrades Ihres Tresortyps vom klassischen auf das Resource Manager-Bereitstellungsmodell müssen Sie Ihre bestehende Automatisierung oder die bestehenden Tools aktualisieren, um sicherzustellen, dass sie nach dem Upgrade weiterhin funktionieren.

## <a name="preparing-your-environment-for-vault-upgrade"></a>Vorbereiten der Umgebung für das Tresorupgrade

1.  Installieren Sie über [diesen Link](https://www.microsoft.com/download/details.aspx?id=50395) PowerShell Version 5 oder höher, oder führen Sie ein Upgrade auf diese Version durch.

2.  Installieren Sie über [diesen Link](https://github.com/Azure/azure-powershell/releases) die neueste Azure PowerShell-MSI-Version.

3.  Laden Sie über [diesen Link](https://aka.ms/vaultupgradescript) das Skript für das Tresorupgrade herunter.

## <a name="prerequisites-for-upgrade"></a>Voraussetzungen für das Upgrade
Um ein Upgrade für Ihre Tresore von Site Recovery-Tresoren auf Azure Resource Manager-basierte Recovery Service-Tresore durchzuführen, müssen folgende Voraussetzungen erfüllt sein:

- Mindestens erforderliche Agent-Version: Für die Durchführung des Upgrades muss mindestens Version 5.1.1700.0 des Azure Site Recovery-Anbieters auf Ihrem Server installiert sein.

- Unterstützte Konfiguration: Ihr Tresor darf nicht mit einem SAN oder mit SQL Server AlwaysOn-Verfügbarkeitsgruppen konfiguriert sein. Alle anderen Konfigurationen werden unterstützt.

>[!NOTE]
> Die Speicherzuordnung darf nach dem Upgrade nur über PowerShell verwaltet werden.

- Unterstütztes Bereitstellungsszenario – Ihr Tresor darf nicht im Legacybereitstellungsmodell „VMware nach Azure“ konfiguriert sein.  Bevor Sie fortfahren, müssen Sie zuerst eine Migration auf das verbesserte Bereitstellungsmodell durchführen.

- Keine aktiven, benutzerinitiierten Aufträge, die Vorgänge auf der Verwaltungsebene umfassen: Da der Zugriff auf die Verwaltungsebene während des Upgrades beschränkt ist, müssen Sie alle Aktionen auf der Verwaltungsebene abschließen und danach das Upgrade auslösen. Laufende Replikationen sind davon nicht betroffen.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

- Wirkt sich dieses Upgrade auf meine laufende Replikation aus?

  Nr. Ihre laufende Replikation wird während und nach dem Upgrade ohne Unterbrechung fortgesetzt.

- Was geschieht mit den Netzwerkeinstellungen, wie etwa den Einstellungen für das Standort-zu-Standort-VPN und den IP-Einstellungen?

  Das Upgrade hat keine Auswirkungen auf die Netzwerkeinstellungen. Alle Verbindungen zwischen Azure und lokalen Netzwerken werden aufrechterhalten.
- Was geschieht mit meinen Tresoren, wenn ich in Kürze kein Upgrade durchführe?

  Die Unterstützung für Site Recovery-Tresore im alten Azure-Portal wird ab September als veraltet markiert. Daher empfehlen wir Kunden ausdrücklich, die Upgradefunktion zur Migration auf das neue Portal zu verwenden.

- Was bedeutet dieser Migrationsplan für meine vorhandenen Tools?  

  Die Aktualisierung Ihrer Tools auf das Resource Manager-Bereitstellungsmodell, auf dem Recovery Services-Tresore basieren, ist eine der wichtigsten Änderungen, die Sie in Ihren Upgradeplänen berücksichtigen müssen.

- Wie lange dauert die Downtime der Verwaltungsebene?

  Das Upgrade dauert ca. 15-30 Minuten. Es kann maximal bis zu eine Stunde in Anspruch nehmen.

- Kann ich nach dem Upgrade ein Rollback ausführen?

  Nr. Ein Rollback wird nicht unterstützt, nachdem erfolgreich ein Upgrade für die Ressourcen durchgeführt wurde.

- Kann ich mein Abonnement oder meine Ressourcen überprüfen, um zu ermitteln, ob sie für das Upgrade geeignet sind?

  Ja. Bei der Option für das plattformgestützte Upgrade besteht der erste Schritt darin, zu überprüfen, ob die Ressourcen für das Upgrade geeignet sind. Falls bei der Überprüfung der Voraussetzungen ein Fehler auftritt, werden entsprechende Fehler/Warnungen ausgegeben.

- Wie melde ich ein Problem in Bezug auf das Upgrade?

  Falls während des Upgrades Fehler auftreten, notieren Sie sich das im Fehler aufgeführte Attribut „OperationId“. Der Microsoft-Support arbeitet proaktiv an der Behebung des Problems. Bei Fragen oder Vorschlägen können Sie sich unter Angabe Ihrer Abonnement-ID, des Tresornamens und der Vorgangs-ID auch an das Supportteam wenden. Wir werden das Problem so schnell wie möglich bearbeiten. Wiederholen Sie den Vorgang nicht, es sei denn, Sie werden von Microsoft explizit dazu aufgefordert.

## <a name="how-to-run-the-script"></a>Wie wird das Skript ausgeführt?

Führen Sie in einer PowerShell-Eingabeaufforderung den folgenden Befehl aus:

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionID <subscriptionID>  -VaultName <vaultname> -Location <location> -ResourceType HyperVRecoveryManagerVault -TargetResourceGroupName <rgname>

- SubscriptionId: Die mit dem Tresor verknüpfte Abonnement-ID, für den das Upgrade durchgeführt wird.
- VaultName: Der Name des Tresors, für den das Upgrade durchgeführt wird.
- Speicherort: Der Speicherort des Tresors, für den das Upgrade durchgeführt wird.
- ResourceType: HyperVRecoveryManagerVault für Site Recovery-Tresore
- TargetResourceGroupName: Ressourcengruppe, in die der aktualisierte Tresor platziert werden sollen. Bei „TargetResourceGroupName“ kann es sich um eine vorhandene Ressourcengruppe in Azure Resource Manager oder eine neue Ressourcengruppe handeln. Falls die angegebene Ressourcengruppe „TargetResourceGroupName“ nicht vorhanden ist, wird sie im Rahmen des Upgrades am selben Speicherort wie den des Tresors erstellt. Um mehr über Ressourcengruppen zu erfahren, klicken Sie [hier](../azure-resource-manager/resource-group-overview.md#resource-groups).

>[!NOTE]
>Für Namen von Ressourcengruppen gelten Beschränkungen. Halten Sie diese ein, da anderenfalls Fehler beim Tresorupgrade auftreten können.

Beispiel:

    .\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionId 1234-54123-354354-56416-8645 -VaultName gen2dr -Location "north europe" -ResourceType hypervrecoverymanagervault -TargetResourceGroupName abc


Alternativ können Sie das u.g. Skript ausführen. Daraufhin werden Sie aufgefordert, Eingaben für alle erforderlichen Parameter vorzunehmen.

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1
    cmdlet RecoveryServicesVaultUpgrade-1.0.0.ps1 at command pipeline position 1

    Supply values for the following parameters:
    SubscriptionId:
    VaultName:
    Location:
    ResourceType:
    TargetResourceGroupName:

1.  Das PowerShell-Skript fordert Sie zur Eingabe Ihrer Anmeldeinformationen auf. Sie müssen zweimal Ihre Anmeldeinformationen eingeben: einmal für das ASM-Konto und ein zweites Mal für das Azure Resource Manager-Konto.

2.  Nachdem Sie Ihre Anmeldeinformationen eingegeben haben, führt das Skript eine Voraussetzungsprüfung durch, um festzustellen, ob Ihre Infrastruktureinrichtung die weiter oben im Dokument genannten Voraussetzungen erfüllt.

3.  Nachdem die Voraussetzungen überprüft wurden, werden Sie durch ein Bestätigungsfenster aufgefordert, das Tresorupgrade fortzusetzen. Nach der Bestätigung wird der Prozess zur Durchführung des Tresorupgrades gestartet. Das gesamte Upgrade kann 15 bis 30 Minuten dauern.

4.  Nach dem erfolgreichen Abschluss des Upgrades können Sie im neuen Azure-Portal auf den aktualisierten Tresor zugreifen.

## <a name="management-experience-post-upgrade"></a>Verwaltungsoberfläche nach dem Upgrade

### <a name="how-to-replicate-using-azure-site-recovery-in-the-recovery-services-vault"></a>Vorgehensweise zum Replizieren von Azure Site Recovery im Recovery Services-Tresor

- Nun können Sie Ihre Azure-VMs regionsübergreifend schützen. Weitere Informationen hierzu finden Sie in der Dokumentation unter [diesem Link](site-recovery-azure-to-azure.md).

- Um mehr über die Replikation von VMware-VMs nach Azure zu erfahren, lesen Sie die Dokumentation unter [diesem Link](vmware-walkthrough-overview.md).

- Um mehr über die Replikation von Hyper-VMs (ohne VMM) nach Azure zu erfahren, lesen Sie die Dokumentation unter [diesem Link](hyper-v-site-walkthrough-overview.md).

- Um mehr über die Replikation von Hyper-VMs (mit VMM) nach Azure zu erfahren, lesen Sie die Dokumentation unter [diesem Link](vmm-to-azure-walkthrough-overview.md).

- Um mehr über die Replikation von Hyper-VMs (mit VMM) an einem sekundären Standort zu erfahren, lesen Sie die Dokumentation unter [diesem Link](site-recovery-vmm-to-vmm.md).

- Um mehr über die Replikation von VMware-VMs an einen sekundären Standort zu erfahren, lesen Sie die Dokumentation unter [diesem Link](site-recovery-vmware-to-vmware.md).

### <a name="how-to-view-your-replicated-items"></a>Vorgehensweise zum Anzeigen replizierter Elemente

Nachfolgend sehen Sie die Anzeige, auf der die Dashboardseite des Recovery Services-Tresors mit wichtigen Entitäten für den Tresor angezeigt wird. Klicken Sie auf **Site Recovery** -> **Replizierte Elemente**, um die Liste der geschützten Entitäten im Tresor anzuzeigen.


![Replizierte Elemente](./media/upgrade-site-recovery-vaults/replicateditems.png)

Auf der folgenden Anzeige werden der Workflow zum Anzeigen Ihrer replizierten Elemente und die Vorgehensweise zum Initiieren eines Failovers angezeigt.

![Replizierte Elemente](./media/upgrade-site-recovery-vaults/failover.png)

### <a name="how-to-view-your-replication-settings"></a>Vorgehensweise zum Anzeigen von Replikationseinstellungen

Im Site Recovery-Tresor ist jede Schutzgruppe mit Replikationseinstellungen konfiguriert, wie etwa der Kopierhäufigkeit, der Aufbewahrung für Wiederherstellungspunkte, der Häufigkeit der konsistenten Anwendungsmomentaufnahmen. In Recovery Services-Tresor sind diese Einstellungen als Replikationsrichtlinie konfiguriert. Der Name der Richtlinie ist der Name der Schutzgruppe oder „primarycloud_Policy“.

Weitere Informationen zur Replikationsrichtlinie finden Sie [hier](site-recovery-setup-replication-settings-vmware.md).

