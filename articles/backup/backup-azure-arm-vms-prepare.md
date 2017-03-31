---
title: 'Azure Backup: Vorbereiten der Sicherung virtueller Computer | Microsoft-Dokumentation'
description: "Vergewissern Sie sich, dass Ihre Umgebung für die Sicherung virtueller Computer in Azure vorbereitet ist."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: Sicherung; Sichern;
ms.assetid: e87e8db2-b4d9-40e1-a481-1aa560c03395
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/7/2017
ms.author: markgal;trinadhk;
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: c059d39840ae268da647cffd2bfcb937f006356a
ms.lasthandoff: 03/25/2017


---
# <a name="prepare-your-environment-to-back-up-resource-manager-deployed-virtual-machines"></a>Vorbereiten der Umgebung für die Sicherung von mit Resource Manager bereitgestellten virtuellen Computern
> [!div class="op_single_selector"]
> * [Resource Manager-Modell](backup-azure-arm-vms-prepare.md)
> * [Klassisches Modell](backup-azure-vms-prepare.md)
>
>

Dieser Artikel enthält die Schritte zum Vorbereiten Ihrer Umgebung zum Sichern eines mit Resource Manager bereitgestellten virtuellen Computers (VM). In den im Verfahren dargestellten Schritten wird das Azure-Portal genutzt.  

Der Azure Backup-Dienst bietet zwei Typen von Tresoren (Sicherungstresore und Recovery Services-Tresore) zum Schutz Ihrer virtuellen Computer. Ein Sicherungstresor schützt VMs, die mit dem klassischen Bereitstellungsmodell bereitgestellt wurden. Ein Recovery Services-Tresor schützt **sowohl klassisch bereitgestellte als auch mit Resource Manager bereitgestellte virtuelle Computer**. Um einen mit Resource Manager bereitgestellten virtuellen Computer zu schützen, müssen Sie einen Recovery Services-Tresor verwenden.

> [!NOTE]
> Azure verfügt über zwei Bereitstellungsmodelle zum Erstellen und Verwenden von Ressourcen: [Resource Manager-Modell und klassisches Modell](../azure-resource-manager/resource-manager-deployment-model.md). Unter [Vorbereiten der Umgebung für die Sicherung virtueller Azure-Computer](backup-azure-vms-prepare.md) finden Sie ausführliche Informationen zum Arbeiten mit virtuellen Computern, die mit dem klassischen Bereitstellungsmodell bereitgestellt wurden.
>
>

Bevor Sie einen mit Resource Manager bereitgestellten virtuellen Computer (VM) schützen oder sichern können, schaffen Sie folgende Voraussetzungen:

* Erstellen Sie einen Recovery Services-Tresor *am gleichen Ort, an dem sich Ihr virtueller Computer befindet*(oder geben Sie einen dort vorhandenen Recovery Services-Tresor an).
* Wählen Sie ein Szenario aus, definieren Sie die Sicherungsrichtlinie, und definieren Sie die zu schützenden Elemente.
* Überprüfen Sie die Installation des VM-Agents auf dem virtuellen Computer.
* Überprüfen Sie die Netzwerkverbindung.
* Wenn Sie für virtuelle Linux-Computer die Sicherungsumgebung für anwendungskonsistente Sicherungen anpassen möchten, führen Sie die [Schritte zum Konfigurieren von Skripts vor und nach der Momentaufnahme](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent) aus.

Wenn Sie wissen, dass diese Bedingungen in Ihrer Umgebung bereits erfüllt sind, fahren Sie mit dem Artikel [Sichern von virtuellen Azure-Computern](backup-azure-vms.md)fort. Wenn Sie beliebige dieser Voraussetzungen einrichten oder überprüfen möchten, führt dieser Artikel Sie durch die Schritte zum Vorbereiten der jeweiligen Voraussetzung.

##<a name="supported-operating-system-for-backup"></a>Unterstütztes Betriebssystem für die Sicherung
 * **Linux**: Azure Backup unterstützt eine [Liste von Verteilungen, die von Azure unterstützt werden](../virtual-machines/virtual-machines-linux-endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ausnahme: CoreOS Linux. _Andere Bring-Your-Own-Linux-Distributionen sollten ebenfalls funktionieren, sofern der VM-Agent auf dem virtuellen Computer verfügbar ist und Python unterstützt wird. Wir empfehlen diese Distributionen jedoch nicht für die Sicherung._
 * **Windows Server**: Versionen, die älter als Windows Server 2008 R2 sind, werden nicht unterstützt.

## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Einschränkungen beim Sichern und Wiederherstellen eines virtuellen Computers
Bitte machen Sie sich vor der Vorbereitung der Umgebung mit den Einschränkungen vertraut.

* Die Sicherung von virtuellen Computern mit mehr als 16 Datenträgern wird nicht unterstützt.
* Die Sicherung von virtuellen Computern mit einer reservierten IP-Adresse und ohne definierten Endpunkt wird nicht unterstützt.
* Die Sicherung von virtuellen Computern, die nur mit BEK verschlüsselt sind, wird nicht unterstützt. Die Sicherung von virtuellen Linux-Computern, die mit LUKS-Verschlüsselung verschlüsselt sind, wird nicht unterstützt.
* Das Sichern von virtuellen Linux-Computern mit Docker-Erweiterung wird nicht unterstützt.
* Im Netzwerk bereitgestellte und an den virtuellen Computer angefügte Laufwerke werden nicht in die Sicherungsdaten einbezogen.
* Das Ersetzen eines vorhandenen virtuellen Computers während der Wiederherstellung wird nicht unterstützt. Wenn Sie versuchen, die VM wiederherzustellen, obwohl die VM vorhanden ist, wird die Wiederherstellung nicht ausgeführt.
* Die regionsübergreifende Sicherung und Wiederherstellung wird nicht unterstützt.
* Sie können virtuelle Computer in allen öffentlichen Regionen von Azure sichern (siehe [Checkliste](https://azure.microsoft.com/regions/#services) der unterstützten Regionen). Wenn die gewünschte Region derzeit nicht unterstützt wird, wird sie bei der Erstellung des Tresors in der Dropdownliste nicht angezeigt.
* Das Wiederherstellen eines virtuellen Domänencontrollercomputers, der Teil einer Konfiguration mit mehreren Domänencontrollern ist, wird nur über PowerShell unterstützt. Weitere Informationen hierzu finden Sie unter [Wiederherstellen von Multi-DC-Domänencontrollern](backup-azure-restore-vms.md#restoring-domain-controller-vms).
* Das Wiederherstellen virtueller Computer mit den folgenden besonderen Netzwerkkonfigurationen wird nur über PowerShell unterstützt. Virtuelle Computer, die mit dem Wiederherstellungsworkflow der Benutzeroberfläche erstellt werden, weisen diese Netzwerkkonfigurationen nach dem Abschluss des Wiederherstellungsvorgangs nicht auf. Weitere Informationen finden Sie unter [Wiederherstellen von VMs mit speziellen Netzwerkkonfigurationen](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations).
  * Virtuelle Computer unter Load Balancer-Konfiguration (intern und extern)
  * Virtuelle Computer mit mehreren reservierten IP-Adressen
  * Virtuelle Computer mit mehreren Netzwerkadaptern

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Erstellen eines Recovery Services-Tresor für eine VM
Bei einem Recovery Services-Tresor handelt es sich um eine Entität, in der alle Sicherungen und Wiederherstellungspunkte gespeichert werden, die im Laufe der Zeit erstellt wurden. Der Recovery Services-Tresor enthält auch die Sicherungsrichtlinien, die den geschützten VMs zugeordnet sind.

So erstellen Sie einen Recovery Services-Tresor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Hub-Menü auf **Durchsuchen**, und geben Sie in der Liste mit den Ressourcen **Recovery Services** ein. Wenn Sie mit der Eingabe beginnen, wird die Liste anhand Ihrer Eingaben gefiltert. Klicken Sie auf **Recovery Services-Tresor**.

    ![Klicken Sie auf die Schaltfläche „Durchsuchen“, und geben Sie „Recovery Services“ ein. Wenn die Option „Recovery Services-Tresor“ angezeigt wird, klicken Sie darauf, um das Blatt „Recovery Services-Tresor“ zu öffnen.](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    Die Liste mit den Recovery Services-Tresoren wird angezeigt.
3. Klicken Sie im Menü **Recovery Services-Tresore** auf **Hinzufügen**.

    ![Erstellen eines Recovery Services-Tresors – Schritt 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    Das Blatt „Recovery Services-Tresor“ wird geöffnet, und Sie werden aufgefordert, **Name**, **Abonnement**, **Ressourcengruppe** und **Standort** anzugeben.

    ![Erstellen eines Recovery Services-Tresors – Schritt 5](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. Geben Sie unter **Name**einen Anzeigenamen für den Tresor ein. Der Name muss für das Azure-Abonnement eindeutig sein. Geben Sie einen Namen ein, der zwischen 2 und 50 Zeichen enthält. Er muss mit einem Buchstaben beginnen und darf nur Buchstaben, Zahlen und Bindestriche enthalten.
5. Klicken Sie auf **Abonnement** , um die Liste mit den verfügbaren Abonnements anzuzeigen. Falls Sie nicht sicher sind, welches Abonnement geeignet ist, können Sie das Standardabonnement bzw. das vorgeschlagene Abonnement verwenden. Es sind nur dann mehrere Auswahlmöglichkeiten verfügbar, wenn Ihr Organisationskonto mehreren Azure-Abonnements zugeordnet ist.
6. Klicken Sie auf **Ressourcengruppe**, um die Liste mit den verfügbaren Ressourcengruppen anzuzeigen, oder klicken Sie auf **Neu**, um eine neue Ressourcengruppe zu erstellen. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
7. Klicken Sie auf **Standort** , um die geografische Region für den Tresor auszuwählen. Der Tresor **muss** sich in derselben Region wie die zu schützenden virtuellen Computer befinden.

   > [!IMPORTANT]
   > Wenn Sie sich nicht sicher sind, an welchem Standort sich Ihr virtueller Computer befindet, schließen Sie das Dialogfeld zur Tresorerstellung, und wechseln Sie zur Liste der virtuellen Computer im Portal. Wenn Sie über virtuelle Computer in mehreren Regionen verfügen, müssen Sie in jeder Region einen Recovery Services-Tresor erstellen. Erstellen Sie den Tresor am ersten Standort, bevor Sie mit dem nächsten Standort fortfahren. Es müssen keine Speicherkonten zum Speichern der Sicherungsdaten angegeben werden. Dies erfolgt automatisch über den Recovery Services-Tresor und den Azure Backup-Dienst.
   >
   >

8. Klicken Sie auf **Erstellen**. Es kann einige Zeit dauern, bis der Recovery Services-Tresor erstellt wurde. Verfolgen Sie die Benachrichtigungen oben rechts im Portal. Nach Abschluss des Erstellungsvorgangs wird der Tresor in der Liste mit den Recovery Services-Tresoren angezeigt. Sollte der Tresor nicht angezeigt werden, klicken Sie auf **Aktualisieren**.

    ![Liste der Sicherungstresore](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

    Nachdem Sie Ihren Tresor erstellt haben, widmen wir uns nun dem Festlegen der Speicherreplikation.

## <a name="set-storage-replication"></a>Festlegen der Speicherreplikation
Bei der Speicherreplikation haben Sie die Wahl zwischen georedundantem Speicher und lokal redundantem Speicher. Standardmäßig verfügt Ihr Tresor über einen georedundanten Speicher. Behalten Sie den georedundanten Speicher bei, wenn es sich hierbei um Ihre primäre Sicherung handelt. Wählen Sie lokal redundanten Speicher, wenn Sie eine günstigere und weniger langfristige Option wünschen.

So bearbeiten Sie die Einstellung für die Speicherreplikation:

1. Wählen Sie auf dem Blatt **Recovery Services-Tresore** Ihren Tresor aus.
    Wenn Sie auf Ihren Tresor klicken, werden das Blatt mit den Einstellungen (*auf dem oben der Name des Tresors angegeben ist*) sowie das Blatt mit den Tresordetails geöffnet.

    ![Auswählen des Tresors aus der Liste der Sicherungstresore](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. Verwenden Sie auf dem Blatt **Einstellungen** den vertikalen Schieberegler, um nach unten zum Abschnitt **Verwalten** zu scrollen. Klicken Sie auf **Sicherungsinfrastruktur**, um das entsprechende Blatt zu öffnen. Klicken Sie im Abschnitt **Allgemein** auf **Sicherungskonfiguration**, um das entsprechende Blatt zu öffnen. Wählen Sie auf dem Blatt **Speicherkonfiguration** die Speicherreplikationsoption für Ihren Tresor aus. Standardmäßig verfügt Ihr Tresor über einen georedundanten Speicher. Wenn Sie den Speicherreplikationstyp ändern, klicken Sie auf **Speichern**.

    ![Liste der Sicherungstresore](./media/backup-azure-arm-vms-prepare/full-blade.png)

     Wenn Sie Azure als primären Speicherendpunkt für die Sicherung verwenden, verwenden Sie weiterhin den georedundanten Speicher. Wenn Sie Azure nicht als primären Speicherendpunkt für die Sicherung verwenden, wählen Sie „Lokal redundanter Speicher“ aus. Weitere Informationen zu den Optionen für [georedundanten](../storage/storage-redundancy.md#geo-redundant-storage) und [lokal redundanten](../storage/storage-redundancy.md#locally-redundant-storage) Speicher finden Sie in der [Übersicht über die Azure Storage-Replikation](../storage/storage-redundancy.md).
    Wenn Sie die Speicheroption für Ihren Tresor ausgewählt haben, können Sie den virtuellen Computer dem Tresor zuordnen. Ermitteln und registrieren Sie die virtuellen Azure-Computer, um mit der Zuordnung zu beginnen.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Auswählen eines Sicherungsziels, Festlegen der Richtlinie und Definieren von zu schützenden Elementen
Führen Sie zunächst den Ermittlungsprozess durch, bevor Sie einen virtuellen Computer registrieren, um sicherzustellen, dass alle neuen virtuellen Computer, die dem Abonnement hinzugefügt wurden, identifiziert werden. Bei diesem Vorgang wird Azure nach der Liste virtueller Computer im Abonnement abgefragt. Außerdem werden zusätzliche Informationen wie der Clouddienstname und die Region erfasst. „Szenario“ bezeichnet im Kontext des Azure-Portals das, was Sie dem Recovery Services-Tresor hinzufügen möchten. Die Richtlinie gibt die Häufigkeit und den Zeitpunkt für die Erstellung von Wiederherstellungspunkten an. Darüber hinaus enthält die Richtlinie die Beibehaltungsdauer für die Wiederherstellungspunkte.

1. Falls Sie bereits über einen geöffneten Recovery Services-Tresor verfügen, können Sie mit Schritt 2 fortfahren. Wenn kein Recovery Services-Tresor geöffnet ist, öffnen Sie das [Azure-Portal](https://portal.azure.com/), und klicken Sie im Hubmenü auf **Weitere Dienste**.

   * Geben Sie in der Liste mit den Ressourcen **Recovery Services**ein.
   * Wenn Sie mit der Eingabe beginnen, wird die Liste anhand Ihrer Eingaben gefiltert. Klicken Sie auf **Recovery Services-Tresore**, wenn der Eintrag angezeigt wird.

     ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

     Die Liste mit den Recovery Services-Tresoren wird angezeigt. Wenn in Ihrem Abonnement keine Tresore vorhanden sind, ist diese Liste leer.

    ![Ansicht der Liste mit den Recovery Services-Tresoren](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   * Wählen Sie in der Liste mit den Recovery Services-Tresoren einen Tresor aus, um das Dashboard zu öffnen.

     Das Blatt „Einstellungen“ und das Tresordashboard für den ausgewählte Tresor werden geöffnet.

     ![Tresorblatt öffnen](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
2. Klicken Sie im Menü des Tresordashboards auf **Sicherung**, um das Blatt „Sicherung“ zu öffnen.

    ![Blatt „Sicherung“ öffnen](./media/backup-azure-arm-vms-prepare/backup-button.png)

    Die Blätter „Sicherung“ und „Sicherungsziel“ werden geöffnet.

    ![Blatt „Szenario“ öffnen](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

3. Legen Sie auf dem Blatt für das Sicherungsziel die Option **Wo wird Ihre Workload ausgeführt?** auf „Azure“ und die Option **Was möchten Sie sichern?** auf „Virtueller Computer“ fest, und klicken Sie anschließend auf **OK**.

    Dadurch wird die VM-Erweiterung im Tresor registriert. Das Blatt „Sicherungsziel“ wird geschlossen, und das Blatt **Sicherungsrichtlinie** wird geöffnet.

    ![Blatt „Szenario“ öffnen](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
4. Wählen Sie auf dem Blatt „Sicherungsrichtlinie“ die Sicherungsrichtlinie aus, die Sie auf den Tresor anwenden möchten.

    ![Sicherungsrichtlinie auswählen](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

    Die Details zur Standardrichtlinie werden unter dem Dropdownmenü aufgeführt. Wählen Sie im Dropdownmenü die Option **Neu erstellen** aus, wenn Sie eine neue Richtlinie erstellen möchten. Eine Anleitung zum Definieren einer Sicherungsrichtlinie finden Sie unter [Definieren einer Sicherungsrichtlinie](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Klicken Sie auf **OK**, um die Sicherungsrichtlinie mit dem Tresor zu verknüpfen.

    Das Blatt „Sicherungsrichtlinie“ wird geschlossen, und das Blatt **Virtuelle Computer auswählen** wird geöffnet.
5. Wählen Sie auf dem Blatt **Virtuelle Computer auswählen** die virtuellen Computer aus, die der angegebenen Richtlinie zugeordnet werden sollen, und klicken Sie auf **OK**.

    ![Workload auswählen](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

    Der ausgewählte virtuelle Computer wird überprüft. Falls die erwarteten virtuellen Computer nicht angezeigt werden, sollten Sie überprüfen, ob sie sich am gleichen Azure-Standort wie der Recovery Services-Tresor befinden und nicht bereits in einem anderen Tresor geschützt werden. Der Standort des Recovery Services-Tresors wird auf dem Tresordashboard angezeigt.

6. Nachdem Sie nun alle Einstellungen für den Tresor definiert haben, klicken Sie auf dem Blatt „Sicherung“ auf **Sicherung aktivieren**. Die Richtlinie wird im Tresor und auf den VMs bereitgestellt. Dadurch wird nicht der erste Wiederherstellungspunkt für den virtuellen Computer erstellt.

    ![Sicherung aktivieren](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Nachdem die Sicherung erfolgreich aktiviert wurde, wird die Sicherungsrichtlinie nach dem Zeitplan ausgeführt. Wenn Sie einen bedarfsgesteuerten Sicherungsauftrag generieren möchten, um die virtuellen Computer jetzt zu sichern, finden Sie weitere Informationen unter [Auslösen des Sicherungsauftrags](./backup-azure-arm-vms.md#triggering-the-backup-job).

Wenn Sie Probleme beim Registrieren des virtuellen Computers haben, prüfen Sie die folgenden Informationen zum Installieren des VM-Agents und zu Netzwerkverbindungen. Die folgenden Informationen sind wahrscheinlich nicht erforderlich, wenn Sie in Azure erstellte virtuelle Computer schützen möchten. Wenn Sie jedoch Ihren virtuellen Computer zu Azure migriert haben, ist es wichtig, dass Sie den VM-Agent ordnungsgemäß installiert haben und dass Ihr virtueller Computer mit dem virtuellen Netzwerk kommunizieren kann.

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>Installieren des VM-Agents auf dem virtuellen Computer
Der Azure VM-Agent muss auf dem virtuellen Azure-Computer installiert werden, damit die Sicherungserweiterung funktioniert. Wenn Ihr virtueller Computer aus dem Azure-Katalog erstellt wurde, ist der VM-Agent auf dem virtuellen Computer bereits vorhanden. Diese Informationen gelten für Szenarien, in denen Sie *keinen* virtuellen Computer verwenden, der über den Azure-Katalog erstellt wurde (wenn Sie also beispielsweise einen virtuellen Computer aus einem lokalen Rechenzentrum migriert haben). In diesem Fall muss der VM-Agent installiert werden, um den virtuellen Computer zu schützen. Erfahren Sie mehr über den [VM-Agent](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux).

Falls beim Sichern des virtuellen Azure-Computers Probleme auftreten, müssen Sie sicherstellen, dass der Azure VM-Agent auf dem virtuellen Computer korrekt installiert ist (s. Tabelle unten). Die folgende Tabelle enthält weitere Informationen zum VM-Agent für virtuelle Windows- und Linux-Computer.

| **Vorgang** | **Windows** | **Linux** |
| --- | --- | --- |
| Installieren des VM-Agent |Laden Sie den [Agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)herunter, und installieren Sie ihn. Zum Durchführen der Installation benötigen Sie Administratorberechtigungen. |<li> Installieren Sie den neuesten [Linux-Agent](../virtual-machines/virtual-machines-linux-agent-user-guide.md). Zum Durchführen der Installation benötigen Sie Administratorberechtigungen. Es wird empfohlen, den Agent aus dem Repository Ihrer Distribution zu installieren. Es wird **nicht empfohlen**, den Linux-VM-Agent direkt von GitHub zu installieren.  |
| Aktualisieren des VM-Agents |Das Aktualisieren des VM-Agents ist so einfach wie das Neuinstallieren der [Binärdateien für den VM-Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br>Stellen Sie sicher, dass kein Sicherungsvorgang ausgeführt wird, während der VM-Agent aktualisiert wird. |Folgen Sie den Anweisungen unter [Aktualisieren des Linux-VM-Agents](../virtual-machines/virtual-machines-linux-update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Es wird empfohlen, den Agent aus dem Repository Ihrer Distribution zu aktualisieren. Es wird **nicht empfohlen**, den Linux-VM-Agent direkt von GitHub zu aktualisieren.<br>Stellen Sie sicher, dass kein Sicherungsvorgang ausgeführt wird, während der VM-Agent aktualisiert wird. |
| Überprüfen der VM-Agent-Installation |<li>Navigieren Sie auf dem virtuellen Azure-Computer zum Ordner *C:\WindowsAzure\Packages*. <li>Dieser Ordner enthält die Datei "WaAppAgent.exe".<li> Klicken Sie mit der rechten Maustaste auf die Datei, wechseln Sie zu **Eigenschaften**, und wählen Sie dann die Registerkarte **Details** aus. Im Feld mit der Produktversion sollte 2.6.1198.718 oder eine höhere Version angegeben sein. |N/V |

### <a name="backup-extension"></a>Backup-Erweiterung
Sobald der VM-Agent auf dem virtuellen Computer installiert wurde, installiert der Azure Backup-Dienst die Sicherungserweiterung für den VM-Agent. Der Azure Backup-Dienst installiert nahtlos Upgrades und Patches für die Sicherungserweiterung.

Die Sicherungserweiterung wird vom Sicherungsdienst installiert, unabhängig davon, ob der virtuelle Computer ausgeführt wird oder nicht. Bei einem ausgeführten virtuellen Computer besteht zudem die größte Chance auf einen anwendungskonsistenten Wiederherstellungspunkt. Der Azure Backup-Dienst setzt die Sicherung des virtuellen Computers jedoch auch dann fort, wenn dieser ausgeschaltet wurde und die Erweiterung nicht installiert werden konnte. Dies wird als Offline-VM bezeichnet. In diesem Fall ist der Wiederherstellungspunkt *absturzkonsistent*.

## <a name="network-connectivity"></a>Netzwerkverbindung
Um die VM-Momentaufnahmen zu verwalten, benötigt die Sicherungserweiterung eine Verbindung mit den öffentlichen Azure-IP-Adressen. Ohne die richtige Internetverbindung tritt bei diesen HTTP-Anforderungen des virtuellen Computers ein Timeout auf, und der Sicherungsvorgang funktioniert nicht. Wenn in Ihrer Bereitstellung Zugriffseinschränkungen aktiv sind (z.B. über eine Netzwerksicherheitsgruppe [NSG]), wählen Sie eine der folgenden Optionen für die Bereitstellung eines freien Pfades für den Sicherungsdatenverkehr:

* [Whitelist der IP-Bereiche der Azure-Rechenzentren:](http://www.microsoft.com/en-us/download/details.aspx?id=41653) In diesem Artikel finden Sie Anweisungen zum Erstellen einer Whitelist der IP-Adressen.
* Stellen Sie einen HTTP-Proxyserver zum Weiterleiten des Datenverkehrs bereit.

Bei der Entscheidung, welche Option Sie verwenden, müssen Sie die Kompromisse zwischen Verwaltbarkeit, differenzierter Kontrolle und Kosten abwägen.

| Option | Vorteile | Nachteile |
| --- | --- | --- |
| Whitelist der IP-Bereiche |Keine zusätzlichen Kosten.<br><br>Verwenden Sie zum Öffnen des Zugriffs in einer Netzwerksicherheitsgruppe das <i>Set-AzureNetworkSecurityRule</i>-Cmdlet. |Komplexe Verwaltung, da sich die betroffenen IP-Bereiche im Laufe der Zeit ändern.<br><br>Ermöglicht den Zugriff auf Azure insgesamt, nicht nur auf Storage. |
| HTTP-Proxy |Feinsteuerung im Proxy über Speicher-URLs ist zulässig.<br>Zentraler Punkt für Internetzugriff auf virtuelle Computer.<br>Unterliegt keinen Azure-IP-Adressänderungen. |Zusätzliche Kosten für die Ausführung eines virtuellen Computers mit Proxysoftware. |

### <a name="whitelist-the-azure-datacenter-ip-ranges"></a>Whitelist der IP-Bereiche der Azure-Rechenzentren
Ausführliche Informationen und Anweisungen zur Aufnahme der IP-Bereiche der Azure-Rechenzentren in eine Whitelist finden Sie auf der [Azure-Website](http://www.microsoft.com/en-us/download/details.aspx?id=41653) .

### <a name="using-an-http-proxy-for-vm-backups"></a>Verwenden eines HTTP-Proxys für die Sicherung von virtuellen Computern
Beim Sichern eines virtuellen Computers werden die Befehle für die Momentaufnahmenverwaltung von der Sicherungserweiterung auf der VM per HTTPS-API an Azure Storage gesendet. Leiten Sie den Verkehr der Sicherungserweiterung über den HTTP-Proxy, da dies die einzige Komponente ist, die für den Zugriff auf das öffentliche Internet konfiguriert ist.

> [!NOTE]
> Für die zu verwendende Proxysoftware gibt es keine Empfehlung. Stellen Sie sicher, dass Sie einen Proxy wählen, der mit den unten stehenden Konfigurationsschritten kompatibel ist.
>
>

Die folgende Beispielabbildung zeigt die drei Konfigurationsschritte, die zur Verwendung einen HTTP-Proxys erforderlich sind:

* „App VM“ leitet den gesamten HTTP-Datenverkehr für das öffentliche Internet über „Proxy VM“.
* „Proxy VM“ lässt eingehenden Datenverkehr von virtuellen Computern in das virtuelle Netzwerk zu.
* Die Netzwerksicherheitsgruppe (NSG) mit dem Namen „NSF-lockdown“ benötigt eine Sicherheitsregel, die von „Proxy VM“ ausgehenden Internetdatenverkehr zulässt.

![NSG mit HTTP-Proxybereitstellung – Diagramm](./media/backup-azure-vms-prepare/nsg-with-http-proxy.png)

Um einen HTTP-Proxy zur Kommunikation mit dem öffentlichen Internet zu verwenden, gehen Sie folgendermaßen vor:

#### <a name="step-1-configure-outgoing-network-connections"></a>Schritt 1: Ausgehende Netzwerkverbindungen konfigurieren 
###### <a name="for-windows-machines"></a>Für Windows-Computer
Damit wird die Proxyserverkonfiguration für das lokale Systemkonto eingerichtet.

1. Laden Sie [PsExec](https://technet.microsoft.com/sysinternals/bb897553)
2. Führen Sie den folgenden Befehl in einer Eingabeaufforderung mit erhöhten Rechten aus.

     ```
     psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
     ```
     Er öffnet das Internet Explorer-Fenster.
3. Wechseln Sie zu „Extras -> Internetoptionen -> Verbindungen -> LAN-Einstellungen“.
4. Überprüfen Sie die Proxyeinstellungen für das Systemkonto. Legen Sie Proxy-IP und Port fest.
5. Schließen Sie Internet Explorer.

Mit diesem Befehl wird eine computerweite Proxykonfiguration eingerichtet und für den gesamten ausgehenden HTTP/HTTPS-Datenverkehr verwendet.

Wenn Sie einen Proxyserver auf einem aktuellen Benutzerkonto (keinem lokalen Systemkonto) eingerichtet haben, verwenden Sie das folgende Skript, um die Einstellungen dem SYSTEMKONTO zuzuweisen:

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Wenn das Proxyserverprotokoll den Eintrag „(407) Proxyauthentifizierung erforderlich“ enthält, überprüfen Sie, ob Ihre Authentifizierung ordnungsgemäß eingerichtet ist.
>
>

###### <a name="for-linux-machines"></a>Für Linux-Computer
Fügen Sie der Datei ```/etc/environment``` die folgende Zeile hinzu:

```
http_proxy=http://<proxy IP>:<proxy port>
```

Fügen Sie der Datei ```/etc/waagent.conf``` die folgenden Zeilen hinzu:

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-the-proxy-server"></a>Schritt 2: Zulassen von eingehenden Verbindungen auf dem Proxyserver:
1. Öffnen Sie auf dem Proxyserver die Windows-Firewall. Die einfachste Möglichkeit zum Zugriff auf die Firewall besteht darin, die Windows-Firewall mit erweiterter Sicherheit zu suchen.

    ![Firewall öffnen](./media/backup-azure-vms-prepare/firewall-01.png)
2. Klicken Sie im Dialogfeld „Windows-Firewall“ mit der rechten Maustaste auf **Eingangsregeln**, und klicken Sie auf **Neue Regel**.

    ![Neue Regel erstellen](./media/backup-azure-vms-prepare/firewall-02.png)
3. Wählen Sie im **Assistenten für neue eingehende Regel** als **Regeltyp** die Option **Benutzerdefiniert**, und klicken Sie auf **Weiter**.
4. Wählen Sie auf der Seite zum Auswählen des **Programms** die Option **Alle Programme**, und klicken Sie auf **Weiter**.
5. Geben Sie auf der Seite **Protokoll und Ports** die folgenden Informationen ein, und klicken Sie auf **Weiter**:

    ![Neue Regel erstellen](./media/backup-azure-vms-prepare/firewall-03.png)

   * Für *Protokolltyp* wählen Sie *TCP*.
   * Für *Lokaler Port* wählen Sie *Bestimmte Ports* und geben im Feld darunter den konfigurierten ```<Proxy Port>``` an.
   * Für *Remoteport* wählen Sie *Alle Ports*.

     Klicken Sie sich im Assistenten bis zum Ende durch, und geben Sie dieser Regel einen Namen.

#### <a name="step-3-add-an-exception-rule-to-the-nsg"></a>Schritt 3: Hinzufügen einer Ausnahmeregel zur NSG:
Geben Sie an einer Azure PowerShell-Eingabeaufforderung den folgenden Befehl ein:

Der folgende Befehl fügt der NSG eine Ausnahme hinzu. Diese Ausnahme lässt TCP-Datenverkehr von allen Ports unter 10.0.0.5 an alle Internetadressen über Port 80 (HTTP) oder 443 (HTTPS) zu. Falls Sie einen bestimmten Port im öffentlichen Internet benötigen, müssen Sie diesen Port auch zu ```-DestinationPortRange``` hinzufügen.

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```


*In diesen Schritten werden spezifische Namen und Werte für dieses Beispiel verwendet. Verwenden Sie die Namen und Werte für Ihre Bereitstellung beim Eingeben bzw. Ausschneiden und Einfügen von Informationen in Ihren Code.*

Da Sie jetzt sichergestellt haben, dass Sie über eine Netzwerkverbindung verfügen, können Sie den virtuellen Computer sichern. Weitere Informationen finden Sie unter [Sichern von mit Resource Manager bereitgestellten virtuellen Computern](backup-azure-arm-vms.md).

## <a name="questions"></a>Fragen?
Wenn Sie Fragen haben oder Anregungen zu gewünschten Funktionen mitteilen möchten, [senden Sie uns Ihr Feedback](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Nächste Schritte
Ihre Umgebung ist jetzt für das Sichern Ihres virtuellen Computers vorbereitet. Der nächste logische Schritt besteht darin, eine Sicherung zu erstellen. Der Planungsartikel enthält ausführlichere Informationen zum Sichern virtueller Computer.

* [Sichern virtueller Computer](backup-azure-vms.md)
* [Planen der Sicherungsinfrastruktur für virtuelle Computer](backup-azure-vms-introduction.md)
* [Verwalten der Sicherung virtueller Computer](backup-azure-manage-vms.md)

