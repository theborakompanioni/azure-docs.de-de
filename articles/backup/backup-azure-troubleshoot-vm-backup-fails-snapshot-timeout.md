---
title: "Beheben von Azure Backup-Fehlern – Zeitüberschreitung bei Momentaufnahme-Teilvorgang für VM | Microsoft-Dokumentation"
description: "Fehlerbezogene Symptome, Ursachen und Lösungen von Azure Backup-Fehlern: Keine Kommunikation mit dem VM-Agent zum Abrufen des Momentaufnahmestatus möglich. Zeitüberschreitung bei Momentaufnahme-Teilvorgang für VM."
services: backup
documentationcenter: 
author: genlin
manager: cshepard
editor: 
ms.assetid: 4b02ffa4-c48e-45f6-8363-73d536be4639
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: genli;markgal;
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: d7924d8aade1ea582faa0f319f8c1d16d5461fbc
ms.lasthandoff: 04/03/2017

---

# <a name="troubleshoot-azure-backup-failure-snapshot-vm-sub-task-timed-out"></a>Beheben von Azure Backup-Fehlern – Zeitüberschreitung bei Momentaufnahme-Teilvorgang für VM.
## <a name="summary"></a>Zusammenfassung
Nachdem Sie eine VM für den Azure Backup-Dienst registriert und geplant haben, wird der Auftrag von Backup initiiert, indem die Kommunikation mit der VM-Sicherungserweiterung durchgeführt wird, um eine Zeitpunkt-Momentaufnahme zu erstellen. Eine von vier Bedingungen kann verhindern, dass die Momentaufnahme ausgelöst wird, und dies kann wiederum zu einem Backup-Fehler führen. Dieser Artikel enthält Schritte zum Beheben von Problemen im Zusammenhang mit Backup-Fehlern, die infolge einer Zeitüberschreitung bei Momentaufnahmen auftreten.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Symptom
Für Azure Backup für eine IaaS-VM (Infrastructure as a Service) tritt ein Fehler auf, und im [Azure-Portal](https://portal.azure.com/) wird in den Fehlerdetails des Auftrags die folgende Fehlermeldung zurückgegeben: „Keine Kommunikation mit dem VM-Agent zum Abrufen des Momentaufnahmestatus möglich. Zeitüberschreitung bei Momentaufnahme-Teilvorgang für VM.“

## <a name="cause-1-the-vm-has-no-internet-access"></a>Ursache 1: Kein Internetzugriff für die VM
Gemäß der Bereitstellungsanforderung verfügt der virtuelle Computer nicht über Internetzugriff, oder es liegen Einschränkungen vor, die den Zugriff auf die Azure-Infrastruktur verhindern.

Die Sicherungserweiterung muss eine Verbindung mit den öffentlichen Azure-IP-Adressen herstellen können, damit sie richtig funktioniert. Die Erweiterung sendet Befehle an einen Azure Storage-Endpunkt (HTTP-URL), um die Momentaufnahmen des virtuellen Computers zu verwalten. Wenn die Erweiterung keinen Zugriff auf das öffentliche Internet hat, tritt für Backup irgendwann ein Fehler auf.

### <a name="solution"></a>Lösung
Probieren Sie eine der hier angegebenen Methoden aus, um dieses Problem zu lösen.
#### <a name="allow-access-to-the-azure-datacenter-ip-ranges"></a>Zulassen des Zugriffs auf die IP-Bereiche für das Azure-Datencenter

1. Beschaffen Sie sich die [Liste mit den IP-Adressen des Azure-Datencenters](https://www.microsoft.com/download/details.aspx?id=41653), um den Zugriff zuzulassen.
2. Entsperren Sie die IPs, indem Sie das **New-NetRoute**-Cmdlet auf der Azure-VM in einem PowerShell-Fenster mit erhöhten Rechten ausführen. Führen Sie das Cmdlet als Administrator aus.
3. Fügen Sie der Netzwerksicherheitsgruppe, falls vorhanden, Regeln hinzu, um den Zugriff auf die IP-Adressen zuzulassen.

#### <a name="create-a-path-for-http-traffic-to-flow"></a>Erstellen eines Pfads für HTTP-Datenverkehr

1. Wenn Netzwerkeinschränkungen bestehen (beispielsweise in Form einer Netzwerksicherheitsgruppe), ist es ratsam, einen HTTP-Proxyserver zum Weiterleiten des Datenverkehrs bereitzustellen.
2. Fügen Sie der Netzwerksicherheitsgruppe, falls vorhanden, Regeln hinzu, um den Zugriff auf das Internet über den HTTP-Proxyserver zuzulassen.

Informationen dazu, wie Sie einen HTTP-Proxy für VM-Sicherungen einrichten, finden Sie unter [Vorbereiten der Umgebung für die Sicherung virtueller Azure-Computer](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

## <a name="cause-2-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Ursache 2: Der auf dem virtuellen Computer installierte Agent ist veraltet (virtuelle Linux-Computer).

### <a name="solution"></a>Lösung
Die meisten Fehler im Zusammenhang mit Agents oder Erweiterungen bei virtuellen Linux-Computern werden durch Probleme verursacht, die einen veralteten VM-Agent betreffen. Befolgen Sie diese allgemeinen Richtlinien, um dieses Problem zu beheben:

1. Folgen Sie den Anweisungen unter [Aktualisieren des Linux-VM-Agents ](../virtual-machines/linux/update-agent.md).

 >[!NOTE]
 >Wir *empfehlen dringend*, den Agent ausschließlich über ein Verteilungsrepository zu aktualisieren. Wir raten davon ab, den Agent-Code direkt von GitHub herunterzuladen und die Aktualisierung durchzuführen. Falls der aktuelle Agent nicht zur Verteilung zur Verfügung steht, können Sie sich an den zuständigen Support wenden, um Informationen zur Installation zu erhalten. Eine Prüfung auf den aktuellen Agent können Sie auf der Seite für den [Windows Azure-Linux-Agent](https://github.com/Azure/WALinuxAgent/releases) im GitHub-Repository durchführen.

2. Stellen Sie mit dem folgenden Befehl sicher, dass der Azure-Agent auf der VM ausgeführt wird: `ps -e`

 Wenn der Prozess nicht ausgeführt wird, können Sie ihn mit den folgenden Befehlen neu starten:

 * Für Ubuntu: `service walinuxagent start`
 * Für andere Distributionen: `service waagent start`

3. [Konfigurieren Sie den Agent für den automatischen Neustart](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Führen Sie eine neue Testsicherung aus. Beschaffen Sie sich die folgenden Protokolle von der VM des Kunden, falls der Fehler weiterhin besteht:

   * /var/lib/waagent/*.xml
   * /var/log/waagent.log
   * /var/log/azure/*

Führen Sie die folgenden Schritte aus, falls die ausführliche Protokollierung für waagent erforderlich ist:

1. Suchen Sie in der Datei „/etc/waagent.conf“ nach der folgenden Zeile: **Enable verbose logging (y|n)**
2. Ändern Sie den Wert für **Logs.Verbose** von *n* in *y*.
3. Speichern Sie die Änderung, und starten Sie waagent neu, indem Sie die zuvor aufgeführten Schritte in diesem Abschnitt ausführen.

## <a name="cause-3-the-backup-extension-fails-to-update-or-load"></a>Ursache 3: Die Sicherungserweiterung wird nicht aktualisiert oder geladen.
Wenn Erweiterungen nicht geladen werden können, schlägt die Sicherung fehl, da keine Momentaufnahme erstellt werden kann.

### <a name="solution"></a>Lösung

Für Windows-Gäste:

Stellen Sie sicher, dass der iaasvmprovider-Dienst aktiviert und der *automatische* Start dafür festgelegt ist. Falls der Dienst nicht auf diese Weise konfiguriert ist, sollten Sie ihn aktivieren, damit Sie bestimmen können, ob die nächste Sicherung erfolgreich ist.

Für Linux-Gäste:

Die neueste Version von VMSnapshot für Linux (von Backup verwendete Erweiterung) lautet 1.0.91.0.

Kann die Sicherungserweiterung immer noch nicht aktualisiert oder geladen werden, können Sie das erneute Laden der VMSnapshot-Erweiterung erzwingen, indem Sie die Erweiterung deinstallieren. Beim nächsten Sicherungsversuch wird die Erweiterung neu geladen.

Gehen Sie wie folgt vor, um die Erweiterung zu deinstallieren:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/).
2. Suchen Sie nach der VM mit den Sicherungsproblemen.
3. Klicken Sie auf **Einstellungen**.
4. Klicken Sie auf **Erweiterungen**.
5. Klicken Sie auf **Vmsnapshot Extension** (Vmsnapshot-Erweiterung).
6. Klicken Sie auf **Deinstallieren**.  

Hiermit wird bewirkt, dass die Erweiterung während der nächsten Sicherung neu installiert wird.

## <a name="cause-4-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Ursache 4: Der Momentaufnahmestatus kann nicht abgerufen werden, oder die Momentaufnahme kann nicht erstellt werden.
Die VM-Sicherung basiert auf dem Ausführen eines Momentaufnahmenbefehls für das zugrunde liegende Speicherkonto. Die Sicherung kann fehlschlagen, weil kein Zugriff auf das Speicherkonto besteht oder weil die Ausführung der Momentaufnahmenaufgabe verzögert wird.

### <a name="solution"></a>Lösung
Die folgenden Umstände können zu Fehlern bei Momentaufnahmeaufgaben führen:

| Ursache | Lösung |
| --- | --- |
| Für die VM ist die SQL Server-Sicherung konfiguriert. | Standardmäßig wird bei der Sicherung virtueller Computer eine vollständige VSS-Sicherung auf virtuellen Windows-Computern ausgeführt. Auf virtuellen Computern, auf denen SQL Server-basierte Server ausgeführt werden und auf denen die SQL Server-Sicherung konfiguriert ist, können Verzögerungen bei der Ausführung von Momentaufnahmen auftreten.<br><br>Legen Sie den folgenden Registrierungsschlüssel fest, wenn aufgrund eines Problems mit der Momentaufnahme ein Backup-Fehler auftritt:<br><br>**[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE"** |
| Der VM-Status wird falsch gemeldet, da der virtuelle Computer im RDP heruntergefahren ist. | Wenn Sie die VM im Remotedesktopprotokoll (RDP) herunterfahren, können Sie im Portal ermitteln, ob der VM-Status korrekt ist. Falls nicht, fahren Sie den virtuellen Computer im Portal mithilfe der Option **Herunterfahren** auf dem VM-Dashboard herunter. |
| Viele virtuelle Computer desselben Clouddiensts sind so konfiguriert, dass die Sicherung zur selben Zeit durchgeführt wird. | Eine bewährte Methode besteht darin, für die virtuellen Computer eines Clouddiensts unterschiedliche Sicherungszeitpläne zu verwenden. |
| Der virtuelle Computer wird bei hoher CPU-/Arbeitsspeicherauslastung ausgeführt. | Wird der virtuelle Computer bei hoher CPU-Auslastung (über 90 Prozent) oder hoher Arbeitsspeicherauslastung ausgeführt, wird die Momentaufnahmenaufgabe der Warteschlange hinzugefügt und verzögert, bis schließlich eine Zeitüberschreitung auftritt. Versuchen Sie es in diesem Fall mit einer bedarfsgesteuerten Sicherung. |
| Der virtuelle Computer kann die Host-/Fabric-Adresse nicht aus DHCP abrufen. | Für die VM-Sicherung mithilfe von IaaS muss im Gastbetriebssystem die DHCP-Option aktiviert sein.  Wenn der virtuelle Computer die Host-/Fabric-Adresse nicht aus DHCP-Antwort 245 abrufen kann, können keine Erweiterungen heruntergeladen oder ausgeführt werden. Wenn Sie eine statische private IP-Adresse benötigen, sollten Sie diese über die Plattform konfigurieren. Die DHCP-Option innerhalb des virtuellen Computers sollte aktiviert bleiben. Weitere Informationen finden Sie unter [Festlegen einer statischen internen privaten IP-Adresse](../virtual-network/virtual-networks-reserved-private-ip.md). |

