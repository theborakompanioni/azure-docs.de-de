---
title: "Vorbereiten der Umgebung für die Sicherung virtueller Azure-Computer | Microsoft Docs"
description: "Vergewissern Sie sich, dass Ihre Umgebung für die Sicherung virtueller Computer in Azure vorbereitet ist."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonn
editor: 
keywords: Sicherung; Sichern;
ms.assetid: 238ab93b-8acc-4262-81b7-ce930f76a662
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/25/2017
ms.author: markgal;trinadhk;
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 045dfff9a439674c220a962907d2fe91f2fe0b35
ms.lasthandoff: 04/26/2017


---
# <a name="prepare-your-environment-to-back-up-azure-virtual-machines"></a>Vorbereiten der Umgebung für die Sicherung virtueller Azure-Computer
> [!div class="op_single_selector"]
> * [Resource Manager-Modell](backup-azure-arm-vms-prepare.md)
> * [Klassisches Modell](backup-azure-vms-prepare.md)
>
>

Bevor Sie einen virtuellen Azure-Computer sichern können, müssen drei Bedingungen erfüllt sein.

* Sie müssen einen Sicherungstresor erstellen oder einen vorhandenen Sicherungstresor angeben. Dieser muss sich *in der gleichen Region wie Ihr virtueller Computer* befinden.
* Richten Sie die Netzwerkkonnektivität zwischen den öffentlichen Azure-Internetadressen und den Endpunkten des Azure-Speichers ein.
* Installieren Sie den VM-Agent auf dem virtuellen Computer.

Wenn Sie wissen, dass diese Bedingungen in Ihrer Umgebung bereits erfüllt sind, fahren Sie mit dem Artikel [Sichern von virtuellen Azure-Computern](backup-azure-vms.md)fort. Andernfalls führt dieser Artikel Sie durch die Schritte zum Vorbereiten Ihrer Umgebung für das Sichern einer Azure-VM.

##<a name="supported-operating-system-for-backup"></a>Unterstütztes Betriebssystem für die Sicherung
 * **Linux**: Azure Backup unterstützt eine [Liste von Verteilungen, die von Azure unterstützt werden](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ausnahme: CoreOS Linux. _Andere Bring-Your-Own-Linux-Distributionen sollten ebenfalls funktionieren, sofern der VM-Agent auf dem virtuellen Computer verfügbar ist und Python unterstützt wird. Wir empfehlen diese Distributionen jedoch nicht für die Sicherung._
 * **Windows Server**: Versionen, die älter als Windows Server 2008 R2 sind, werden nicht unterstützt.


## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Einschränkungen beim Sichern und Wiederherstellen eines virtuellen Computers
> [!NOTE]
> Azure verfügt über zwei Bereitstellungsmodelle zum Erstellen und Verwenden von Ressourcen: [Resource Manager-Modell und klassisches Modell](../azure-resource-manager/resource-manager-deployment-model.md). Die folgende Liste enthält die Einschränkungen bei der Bereitstellung über das klassische Modell.
>
>

* Die Sicherung von virtuellen Computern mit mehr als 16 Datenträgern wird nicht unterstützt.
* Die Sicherung von virtuellen Computern mit einer reservierten IP-Adresse und ohne definierten Endpunkt wird nicht unterstützt.
* Im Netzwerk bereitgestellte und an den virtuellen Computer angefügte Laufwerke werden nicht in die Sicherungsdaten einbezogen.
* Das Ersetzen eines vorhandenen virtuellen Computers während der Wiederherstellung wird nicht unterstützt. Löschen Sie zuerst den vorhandenen virtuellen Computer und alle zugeordneten Datenträger, und stellen Sie dann die Daten aus der Sicherung wieder her.
* Regionsübergreifende Sicherungs- und Wiederherstellungsvorgänge werden nicht unterstützt.
* Die Sicherung von virtuellen Computern mithilfe des Azure Backup-Diensts wird in allen öffentlichen Azure-Regionen unterstützt (Informationen hierzu finden Sie in der [Liste der Azure-Regionen](https://azure.microsoft.com/regions/#services) ). Wenn die gewünschte Region derzeit nicht unterstützt wird, wird sie bei der Erstellung des Tresors in der Dropdownliste nicht angezeigt.
* Die Sicherung virtueller Computer mithilfe des Azure Backup-Diensts wird nur für bestimmte Betriebssystemversionen unterstützt:
* Das Wiederherstellen eines virtuellen Domänencontrollercomputers, der Teil einer Konfiguration mit mehreren Domänencontrollern ist, wird nur über PowerShell unterstützt. Weitere Informationen hierzu finden Sie unter [Wiederherstellen von Multi-DC-Domänencontrollern](backup-azure-restore-vms.md#restoring-domain-controller-vms).
* Das Wiederherstellen virtueller Computer mit den folgenden besonderen Netzwerkkonfigurationen wird nur über PowerShell unterstützt. Virtuelle Computer, die mit dem Wiederherstellungsworkflow der Benutzeroberfläche erstellt werden, weisen diese Netzwerkkonfigurationen nach dem Abschluss des Wiederherstellungsvorgangs nicht auf. Weitere Informationen finden Sie unter [Wiederherstellen von VMs mit speziellen Netzwerkkonfigurationen](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations).
  * Virtuelle Computer unter Load Balancer-Konfiguration (intern und extern)
  * Virtuelle Computer mit mehreren reservierten IP-Adressen
  * Virtuelle Computer mit mehreren Netzwerkadaptern

## <a name="create-a-backup-vault-for-a-vm"></a>Erstellen eines Sicherungstresors für einen virtuellen Computer
Bei einem Sicherungstresor handelt es sich um eine Entität, in der alle Sicherungen und Wiederherstellungspunkte gespeichert werden, die im Laufe der Zeit erstellt wurden. Der Sicherungstresor enthält auch die Sicherungsrichtlinien, die auf die zu sichernden virtuellen Computer angewendet werden.

> [!IMPORTANT]
> Ab März 2017 können im klassischen Portal keine Sicherungstresore mehr erstellt werden. Vorhandene Sicherungstresore werden weiterhin unterstützt, und [mithilfe von Azure PowerShell können neue Sicherungstresore erstellt werden](./backup-client-automation-classic.md#create-a-backup-vault). Microsoft empfiehlt jedoch das Erstellen von Recovery Services-Tresoren für alle Bereitstellungen, da zukünftige Verbesserungen nur für Recovery Services-Tresore gelten.


In der Abbildung sind die Beziehungen zwischen den verschiedenen Azure Backup-Entitäten dargestellt:     ![Azure Backup-Entitäten und Beziehungen](./media/backup-azure-vms-prepare/vault-policy-vm.png)



## <a name="network-connectivity"></a>Netzwerkverbindung
Um die VM-Momentaufnahmen zu verwalten, benötigt die Sicherungserweiterung eine Verbindung mit den öffentlichen Azure-IP-Adressen. Ohne die richtige Internetverbindung tritt bei diesen HTTP-Anforderungen des virtuellen Computers ein Timeout auf, und der Sicherungsvorgang funktioniert nicht. Wenn in Ihrer Bereitstellung Zugriffseinschränkungen aktiv sind (z.B. über eine Netzwerksicherheitsgruppe [NSG]), wählen Sie eine der folgenden Optionen für die Bereitstellung eines freien Pfades für den Sicherungsdatenverkehr:

* [Whitelist der IP-Bereiche der Azure-Rechenzentren:](http://www.microsoft.com/en-us/download/details.aspx?id=41653) In diesem Artikel finden Sie Anweisungen zum Erstellen einer Whitelist der IP-Adressen.
* Stellen Sie einen HTTP-Proxyserver zum Weiterleiten des Datenverkehrs bereit.

Bei der Entscheidung, welche Option Sie verwenden, müssen Sie die Kompromisse zwischen Verwaltbarkeit, differenzierter Kontrolle und Kosten abwägen.

| Option | Vorteile | Nachteile |
| --- | --- | --- |
| Whitelist der IP-Bereiche |Keine zusätzlichen Kosten.<br><br>Verwenden Sie zum Öffnen des Zugriffs in einer Netzwerksicherheitsgruppe das <i>Set-AzureNetworkSecurityRule</i>-Cmdlet. |Komplexe Verwaltung, da sich die betroffenen IP-Bereiche im Laufe der Zeit ändern.<br><br>Ermöglicht den Zugriff auf Azure insgesamt, nicht nur auf Storage. |
| HTTP-Proxy |Feinsteuerung im Proxy über Speicher-URLs ist zulässig. Um eine präzise Kontrolle im Proxy zu ermöglichen, muss das URL-Muster „https://\*.blob.core.windows.net/\*“ in der Whitelist enthalten sein. Um nur das von der VM verwendete Speicherkonto der Whitelist hinzuzufügen, muss das URL-Muster „https://\<storageAccount\>.blob.core.windows.net/\*“ der Whitelist hinzugefügt werden. <br>Zentraler Punkt für Internetzugriff auf virtuelle Computer.<br>Unterliegt keinen Azure-IP-Adressänderungen. |Zusätzliche Kosten für die Ausführung eines virtuellen Computers mit Proxysoftware. |

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
2. Klicken Sie im Dialogfeld „Windows-Firewall“ mit der rechten Maustaste auf **Eingangsregeln**, und klicken Sie auf **Neue Regel...**.

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

*Stellen Sie sicher, dass Sie die Namen im Beispiel durch die Details Ihrer Bereitstellung ersetzen.*

## <a name="vm-agent"></a>VM-Agent
Bevor Sie einen virtuellen Azure-Computer sichern können, müssen Sie zunächst sicherstellen, dass der Azure VM-Agent auf dem virtuellen Computer ordnungsgemäß installiert ist. Da es sich beim VM-Agent zum Zeitpunkt der Erstellung des virtuellen Computers um eine optionale Komponente handelt, müssen Sie sicherstellen, dass das Kontrollkästchen für den VM-Agent aktiviert ist, bevor der virtuelle Computer bereitgestellt wird.

### <a name="manual-installation-and-update"></a>Manuelle Installation und Aktualisierung
Der VM-Agent ist auf virtuellen Computern, die über den Azure-Katalog erstellt werden, bereits vorhanden. Auf virtuellen Computern, die aus lokalen Datencentern migriert werden, ist der VM-Agent jedoch nicht installiert. Für diese virtuellen Computer muss der VM-Agent explizit installiert werden. Erfahren Sie mehr über das [Installieren des VM-Agents auf einem vorhandenen virtuellen Computer](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

| **Vorgang** | **Windows** | **Linux** |
| --- | --- | --- |
| Installieren des VM-Agents |<li>Laden Sie den [Agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)herunter, und installieren Sie ihn. Zum Durchführen der Installation benötigen Sie Administratorberechtigungen. <li>[Aktualisieren Sie die VM-Eigenschaft](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) , um anzugeben, dass der Agent installiert wurde. |<li> Installieren Sie den neuesten [Linux-Agent](https://github.com/Azure/WALinuxAgent) aus GitHub. Zum Durchführen der Installation benötigen Sie Administratorberechtigungen. <li> [Aktualisieren Sie die VM-Eigenschaft](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) , um anzugeben, dass der Agent installiert wurde. |
| Aktualisieren des VM-Agents |Das Aktualisieren des VM-Agents ist so einfach wie das Neuinstallieren der [Binärdateien für den VM-Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br><br>Stellen Sie sicher, dass kein Sicherungsvorgang ausgeführt wird, während der VM-Agent aktualisiert wird. |Folgen Sie den Anweisungen unter [Aktualisieren des Linux-VM-Agents ](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). <br><br>Stellen Sie sicher, dass kein Sicherungsvorgang ausgeführt wird, während der VM-Agent aktualisiert wird. |
| Überprüfen der VM-Agent-Installation |<li>Navigieren Sie auf dem virtuellen Azure-Computer zum Ordner *C:\WindowsAzure\Packages*. <li>Dieser Ordner enthält die Datei "WaAppAgent.exe".<li> Klicken Sie mit der rechten Maustaste auf die Datei, wechseln Sie zu **Eigenschaften**, und wählen Sie dann die Registerkarte **Details** aus. Im Feld mit der Produktversion sollte 2.6.1198.718 oder eine höhere Version angegeben sein. |– |

Erfahren Sie mehr über den [VM-Agent](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) und seine [Installation](https://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/).

### <a name="backup-extension"></a>Backup-Erweiterung
Um den virtuellen Computer zu sichern, wird mit dem Azure Backup-Dienst eine Erweiterung für den VM-Agent installiert. Der Azure Backup-Dienst installiert nahtlos und ohne zusätzlichen Benutzereingriff Upgrades und Patches für die Backup-Erweiterung.

Die Sicherungserweiterung wird installiert, wenn der virtuelle Computer (Virtual Machine, VM) ausgeführt wird. Ein ausgeführter virtueller Computer bietet zudem die größte Chance auf einen anwendungskonsistenten Wiederherstellungspunkt. Der Azure Backup-Dienst setzt die Sicherung des virtuellen Computers jedoch auch dann fort, wenn der virtuelle Computer ausgeschaltet wurde und die Erweiterung nicht installiert werden konnte (auch als „Offline-VM“ bezeichnet). In diesem Fall ist der Wiederherstellungspunkt *absturzkonsistent* , wie oben beschrieben.

## <a name="questions"></a>Fragen?
Wenn Sie Fragen haben oder Anregungen zu gewünschten Funktionen mitteilen möchten, [senden Sie uns Ihr Feedback](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Nächste Schritte
Ihre Umgebung ist jetzt für das Sichern Ihres virtuellen Computers vorbereitet. Der nächste logische Schritt besteht darin, eine Sicherung zu erstellen. Der Planungsartikel enthält ausführlichere Informationen zum Sichern virtueller Computer.

* [Sichern virtueller Computer](backup-azure-vms.md)
* [Planen der Sicherungsinfrastruktur für virtuelle Computer](backup-azure-vms-introduction.md)
* [Verwalten der Sicherung virtueller Computer](backup-azure-manage-vms.md)

