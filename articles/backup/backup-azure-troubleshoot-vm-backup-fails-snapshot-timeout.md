---
title: "Fehler bei der Azure-VM-Sicherung: Keine Kommunikation mit dem VM-Agent zum Abrufen des Momentaufnahmestatus möglich. Zeitüberschreitung bei Momentaufnahme-Teilvorgang für VM. | Microsoft Docs"
description: "Hier erfahren Sie mehr zu den Symptomen und Ursachen von Azure-VM-Sicherungsfehlern sowie Lösungen für diese Fehler, die im Zusammenhang mit dem Problem „Die Kommunikation mit dem VM-Agent im Hinblick auf den Status der Momentaufnahme war nicht möglich.“ auftreten. Fehler „Zeitüberschreitung bei Momentaufnahme-Teilvorgang für VM“"
services: backup
documentationcenter: 
author: genlin
manager: cfreeman
editor: 
ms.assetid: 4b02ffa4-c48e-45f6-8363-73d536be4639
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: jimpark; markgal;genli
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 87387e4f182214fa0c34a6a1358c6cc2648be906


---
# <a name="azure-vm-backup-fails-could-not-communicate-with-the-vm-agent-for-snapshot-status---snapshot-vm-sub-task-timed-out"></a>Fehler bei der Azure-VM-Sicherung: Keine Kommunikation mit dem VM-Agent zum Abrufen des Momentaufnahmestatus möglich. Zeitüberschreitung bei Momentaufnahme-Teilvorgang für VM.
## <a name="summary"></a>Zusammenfassung
Nach dem Registrieren und Planen eines virtuellen Computers (Virtual Machine, VM) für die Azure-Sicherung initiiert der Azure Backup-Dienst den Sicherungsauftrag zur geplanten Zeit, indem er mit der Sicherungserweiterung auf dem virtuellen Computer zum Erstellen einer Momentaufnahme kommuniziert. Es gibt Umstände, die die Auslösung der Momentaufnahme verhindern, was dann zu einem Sicherungsfehler führt. Dieser Artikel enthält Schritte zum Beheben von Problemen im Zusammenhang mit Azure-VM-Sicherungsfehlern, die infolge einer Zeitüberschreitung bei der Momentaufnahme auftreten.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Symptom
Bei Microsoft Azure Backup für IaaS-VMs (Infrastructure as a Service) tritt ein Fehler auf, und im [Azure-Portal](https://portal.azure.com/) wird in den Auftragsfehlerdetails die folgende Fehlermeldung zurückgegeben:

**Keine Kommunikation mit dem VM-Agent zum Abrufen des Momentaufnahmestatus möglich. Zeitüberschreitung bei Momentaufnahme-Teilvorgang für VM.**

## <a name="cause"></a>Ursache
Es gibt vier häufige Ursachen für diesen Fehler:

* Der virtuelle Computer verfügt nicht über Internetzugriff.
* Der auf dem virtuellen Computer installierte Microsoft Azure-VM-Agent ist veraltet (virtuelle Linux-Computer).
* Die Sicherungserweiterung wird nicht aktualisiert oder geladen.
* Der Momentaufnahmestatus kann nicht abgerufen werden, oder die Momentaufnahmen können nicht erstellt werden.

## <a name="cause-1-the-vm-does-not-have-internet-access"></a>Ursache 1: Der virtuelle Computer verfügt nicht über Internetzugriff.
Gemäß der Bereitstellungsanforderung verfügt der virtuelle Computers nicht über Internetzugriff, oder es liegen Einschränkungen vor, die den Zugriff auf die Azure-Infrastruktur verhindern.

Die Sicherungserweiterung muss eine Verbindung mit den öffentlichen Azure-IP-Adressen herstellen können, damit sie ordnungsgemäß funktioniert. Das liegt daran, dass sie Befehle an einen Azure Storage-Endpunkt (HTTP-URL) sendet, um die Momentaufnahmen des virtuellen Computers zu verwalten. Wenn die Erweiterung keinen Zugriff auf das öffentliche Internet hat, schlägt die Sicherung fehl.

### <a name="solution"></a>Lösung
Verwenden Sie in diesem Szenario eine der folgenden Methoden, um das Problem zu beheben:

* Aufnehmen der IP-Bereiche des Azure-Rechenzentrums in eine Positivliste
* Erstellen eines Pfads für HTTP-Datenverkehr

### <a name="to-whitelist-the-azure-datacenter-ip-ranges"></a>So nehmen Sie die IP-Bereiche des Azure-Rechenzentrums in eine Positivliste auf
1. Beschaffen Sie sich die [Liste mit den IP-Adressen des Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) , die auf der Positivliste stehen sollen.
2. Heben Sie die Blockierung für die IP-Adressen mit dem Cmdlet „New-NetRoute“ auf. Führen Sie dieses Cmdlet auf dem virtuellen Azure-Computer in einem PowerShell-Fenster mit erhöhten Rechten aus (als Administrator).
3. Fügen Sie der Netzwerksicherheitsgruppe (NSG) (falls vorhanden) Regeln für den Zugriff auf die IP-Adressen hinzu.

### <a name="to-create-a-path-for-http-traffic-to-flow"></a>So erstellen Sie einen Pfad für HTTP-Datenverkehr
1. Wenn Netzwerkeinschränkungen bestehen (beispielsweise eine NSG), sollte ein HTTP-Proxyserver zum Weiterleiten des Datenverkehrs bereitgestellt werden.
2. Falls Sie eine Netzwerksicherheitsgruppe eingerichtet haben, fügen Sie ihr Regeln für den Zugriff auf das Internet über den HTTP-Proxy hinzu.

Erfahren Sie, wie Sie [einen HTTP-Proxy für die Sicherung von virtuellen Computern einrichten](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

## <a name="cause-2-the-microsoft-azure-vm-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Ursache 2: Der auf dem virtuellen Computer installierte Microsoft Azure-VM-Agent ist veraltet (virtuelle Linux-Computer).
### <a name="solution"></a>Lösung
Die meisten agent- oder erweiterungsbezogenen Fehler bei virtuellen Linux-Computern werden durch Probleme verursacht, die einen veralteten VM-Agent betreffen. Zur Problembehandlung werden im Allgemeinen zunächst die folgenden Schritte ausgeführt:

1. [Installieren Sie den neuesten Azure-VM-Agent](https://github.com/Azure/WALinuxAgent).
2. Stellen Sie sicher, dass der Azure-Agent auf dem virtuellen Computer ausgeführt wird. Führen Sie zu diesem Zweck den folgenden Befehl aus:     ```ps -e```
   
    Wenn dieser Prozess nicht ausgeführt wird, verwenden Sie die folgenden Befehle, um ihn neu zu starten.
   
    Für Ubuntu:     ```service walinuxagent start```
   
    Für andere Distributionen:     ```service waagent start
   ```
3. [Configure the auto restart agent](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Run a new test backup. If the failures persist, please collect logs from the following folders for further debugging.
   
    We require the following logs from the customer’s VM:
   
   * /var/lib/waagent/*.xml
   * /var/log/waagent.log
   * /var/log/azure/*

If we require verbose logging for waagent, follow these steps to enable this:

1. In the /etc/waagent.conf file, locate the following line:
   
    Enable verbose logging (y|n)
2. Change the **Logs.Verbose** value from n to y.
3. Save the change, and then restart waagent by following the previous steps in this section.

## Cause 3: The backup extension fails to update or load
If extensions cannot be loaded, then Backup fails because a snapshot cannot be taken.

### Solution
For Windows guests:

1. Verify that the iaasvmprovider service is enabled and has a startup type of automatic.
2. If this is not the configuration, enable the service to determine whether the next backup succeeds.

For Linux guests:

The latest version of VMSnapshot Linux (extension used by backup) is 1.0.91.0

If the backup extension still fails to update or load, you can force the VMSnapshot extension to be reloaded by uninstalling the extension. The next backup attempt will reload the extension.

### To uninstall the extension
1. Go to the [Azure portal](https://portal.azure.com/).
2. Locate the particular VM that has backup problems.
3. Click **Settings**.
4. Click **Extensions**.
5. Click **Vmsnapshot Extension**.
6. Click **uninstall**.

This will cause the extension to be reinstalled during the next backup.

## Cause 4: The snapshots status cannot be retrieved or the snapshots cannot be taken
VM backup relies on issuing snapshot command to underlying storage. The backup can fail because it has no access to storage or because of a delay in snapshot task execution.

### Solution
The following conditions can cause snapshot task failure:

| Cause | Solution |
| --- | --- |
| VMs that have Microsoft SQL Server Backup configured. By default, VM Backup runs a VSS Full backup on Windows VMs. On VMs that are running SQL Server-based servers and on which SQL Server Backup is configured, snapshot execution delays may occur. |Set following registry key if you are experiencing backup failures because of snapshot issues.<br><br>[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE" |
| VM status is reported incorrectly because the VM is shut down in RDP. If you shut down the virtual machine in RDP, check the portal to determine whether that VM status is reflected correctly. |If it’s not, shut down the VM in the portal by using the ”Shutdown” option in the VM dashboard. |
| Many VMs from the same cloud service are configured to back up at the same time. |It’s a best practice to spread out the VMs from the same cloud service to have different backup schedules. |
| The VM is running at high CPU or memory usage. |If the VM is running at high CPU usage (more than 90 percent) or high memory usage, the snapshot task is queued and delayed and eventually times out. In this situation, try on-demand backup. |
| The VM cannot get host/fabric address from DHCP. |DHCP must be enabled inside the guest for IaaS VM Backup to work.  If the VM cannot get host/fabric address from DHCP response 245, then it cannot download ir run any extensions. If you need a static private IP, you should configure it through the platform. The DHCP option inside the VM should be left enabled. View more information about [Setting a Static Internal Private IP](../virtual-network/virtual-networks-reserved-private-ip.md). |




<!--HONumber=Nov16_HO3-->


