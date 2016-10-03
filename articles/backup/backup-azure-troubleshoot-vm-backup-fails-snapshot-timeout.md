<properties
   pageTitle="Fehler bei der Azure-VM-Sicherung: Keine Kommunikation mit dem VM-Agent zum Abrufen des Momentaufnahmestatus möglich. Zeitüberschreitung bei Momentaufnahme-Teilvorgang für VM. | Microsoft Azure"
   description="Hier erfahren Sie mehr zu den Symptomen und Ursachen von Azure-VM-Sicherungsfehlern sowie Lösungen für diese Fehler, die im Zusammenhang mit dem Problem „Die Kommunikation mit dem VM-Agent im Hinblick auf den Status der Momentaufnahme war nicht möglich.“ auftreten. Fehler „Zeitüberschreitung bei Momentaufnahme-Teilvorgang für VM“"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="jwhit"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/14/2016"
    ms.author="jimpark; markgal;genli"/>

# Fehler bei der Azure-VM-Sicherung: Keine Kommunikation mit dem VM-Agent zum Abrufen des Momentaufnahmestatus möglich. Zeitüberschreitung bei Momentaufnahme-Teilvorgang für VM.

## Zusammenfassung

Nach dem Registrieren und Planen eines virtuellen Computers (Virtual Machine, VM) für die Azure-Sicherung initiiert der Azure Backup-Dienst den Sicherungsauftrag zur geplanten Zeit, indem er mit der Sicherungserweiterung auf dem virtuellen Computer zum Erstellen einer Momentaufnahme kommuniziert. Es gibt Umstände, die die Auslösung der Momentaufnahme verhindern, was dann zu einem Sicherungsfehler führt. Dieser Artikel enthält Schritte zum Beheben von Problemen im Zusammenhang mit Azure-VM-Sicherungsfehlern, die infolge einer Zeitüberschreitung bei der Momentaufnahme auftreten.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## Symptom

Bei Microsoft Azure Backup für IaaS-VMs (Infrastructure as a Service) tritt ein Fehler auf, und im Azure-Portal wird in den Auftragsfehlerdetails die folgende Fehlermeldung zurückgegeben:

**Keine Kommunikation mit dem VM-Agent zum Abrufen des Momentaufnahmestatus möglich. Zeitüberschreitung bei Momentaufnahme-Teilvorgang für VM.**

## Ursache
Es gibt vier häufige Ursachen für diesen Fehler:

- Der virtuelle Computer verfügt nicht über Internetzugriff.
- Der auf dem virtuellen Computer installierte Microsoft Azure-VM-Agent ist veraltet (virtuelle Linux-Computer).
- Die Sicherungserweiterung wird nicht aktualisiert oder geladen.
- Der Momentaufnahmestatus kann nicht abgerufen werden, oder die Momentaufnahmen können nicht erstellt werden.

## Ursache 1: Der virtuelle Computer verfügt nicht über Internetzugriff.
Gemäß der Bereitstellungsanforderung verfügt der virtuelle Computers nicht über Internetzugriff, oder es liegen Einschränkungen vor, die den Zugriff auf die Azure-Infrastruktur verhindern.

Die Sicherungserweiterung muss eine Verbindung mit den öffentlichen Azure-IP-Adressen herstellen können, damit sie ordnungsgemäß funktioniert. Das liegt daran, dass sie Befehle an einen Azure Storage-Endpunkt (HTTP-URL) sendet, um die Momentaufnahmen des virtuellen Computers zu verwalten. Wenn die Erweiterung keinen Zugriff auf das öffentliche Internet hat, schlägt die Sicherung fehl.

### Lösung
Verwenden Sie in diesem Szenario eine der folgenden Methoden, um das Problem zu beheben:

- Aufnehmen der IP-Bereiche des Azure-Rechenzentrums in eine Positivliste
- Erstellen eines Pfads für HTTP-Datenverkehr

### So nehmen Sie die IP-Bereiche des Azure-Rechenzentrums in eine Positivliste auf

1. Beschaffen Sie sich die [Liste mit den IP-Adressen des Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653), die auf der Positivliste stehen sollen.
2. Heben Sie die Blockierung für die IP-Adressen mit dem Cmdlet „New-NetRoute“ auf. Führen Sie dieses Cmdlet auf dem virtuellen Azure-Computer in einem PowerShell-Fenster mit erhöhten Rechten aus (als Administrator).
3. Fügen Sie der Netzwerksicherheitsgruppe (NSG) (falls vorhanden) Regeln für den Zugriff auf die IP-Adressen hinzu.

### So erstellen Sie einen Pfad für HTTP-Datenverkehr

1. Wenn Netzwerkeinschränkungen bestehen (beispielsweise eine NSG), sollte ein HTTP-Proxyserver zum Weiterleiten des Datenverkehrs bereitgestellt werden.
2. Falls Sie eine Netzwerksicherheitsgruppe eingerichtet haben, fügen Sie ihr Regeln für den Zugriff auf das Internet über den HTTP-Proxy hinzu.

Erfahren Sie, wie Sie [einen HTTP-Proxy für die Sicherung von virtuellen Computern einrichten](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

## Ursache 2: Der auf dem virtuellen Computer installierte Microsoft Azure-VM-Agent ist veraltet (virtuelle Linux-Computer).

### Lösung
Die meisten agent- oder erweiterungsbezogenen Fehler bei virtuellen Linux-Computern werden durch Probleme verursacht, die einen veralteten VM-Agent betreffen. Zur Problembehandlung werden im Allgemeinen zunächst die folgenden Schritte ausgeführt:

1. [Installieren Sie den neuesten Azure-VM-Agent](https://github.com/Azure/WALinuxAgent).
2. Stellen Sie sicher, dass der Azure-Agent auf dem virtuellen Computer ausgeführt wird. Führen Sie zu diesem Zweck den folgenden Befehl aus: ```ps -e```

    Wenn dieser Prozess nicht ausgeführt wird, verwenden Sie die folgenden Befehle, um ihn neu zu starten.

    Für Ubuntu: ```service walinuxagent start```

    Für andere Distributionen: ```service waagent start
```

3. [Konfigurieren Sie den Agent für den automatischen Neustart](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).

4. Führen Sie eine neue Testsicherung aus. Wenn die Fehler weiterhin auftreten, verwenden Sie die Protokolle aus den folgenden Ordnern für weitere Debuggingschritte.

    Wir benötigen die folgenden Protokolle von den virtuellen Computern des Kunden:

    - /var/lib/waagent/*.xml
    - /var/log/waagent.log
    - /var/log/azure/*

Ist die ausführliche Protokollierung für waagent erforderlich, führen Sie zu ihrer Aktivierung die folgenden Schritte aus:

1. Suchen Sie in der Datei „/etc/waagent.conf“ die folgende Zeile:

    Enable verbose logging (y|n)

2. Ändern Sie den Wert für **Logs.Verbose** von „n“ in „y“.
3. Speichern Sie die Änderung, und starten Sie waagent neu, indem Sie die zuvor aufgeführten Schritte in diesem Abschnitt ausführen.

## Ursache 3: Die Sicherungserweiterung wird nicht aktualisiert oder geladen.
Wenn Erweiterungen nicht geladen werden können, schlägt die Sicherung fehl, da keine Momentaufnahme erstellt werden kann.

### Lösung
Für Windows-Gäste:

1. Stellen Sie sicher, dass der iaasvmprovider-Dienst aktiviert und dafür der automatische Start festgelegt ist.
2. Weicht die Konfiguration davon ab, aktivieren Sie den Dienst, um zu ermitteln, ob die nächste Sicherung erfolgreich ist.

Für Linux-Gäste:

Die neueste Version von VMSnapshot Linux (die für die Sicherung verwendete Erweiterung) lautet 1.0.91.0.

Kann die Sicherungserweiterung immer noch nicht aktualisiert oder geladen werden, können Sie das erneute Laden der VMSnapshot-Erweiterung erzwingen, indem Sie die Erweiterung deinstallieren. Beim nächsten Sicherungsversuch wird die Erweiterung neu geladen.

### So deinstallieren Sie die Erweiterung

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/).
2. Suchen Sie nach dem bestimmten virtuellen Computer, auf dem Sicherungsprobleme auftreten.
3. Klicken Sie auf **Einstellungen**.
4. Klicken Sie auf **Erweiterungen**.
5. Klicken Sie auf **Vmsnapshot Extension** (Vmsnapshot-Erweiterung).
6. Klicken Sie auf **Deinstallieren**.

Dadurch wird die Erweiterung während der nächsten Sicherung neu installiert.

## Ursache 4: Der Momentaufnahmestatus kann nicht abgerufen werden, oder die Momentaufnahmen können nicht erstellt werden.
Bei der VM-Sicherung werden Momentaufnahmenbefehle an den zugrunde liegenden Speicher ausgegeben. Bei der Sicherung kann ein Fehler auftreten, weil kein Zugriff auf den Speicher besteht oder weil eine Verzögerung bei der Ausführung von Momentaufnahmeaufgaben auftritt.

### Lösung
Die folgenden Umstände können zu Fehlern bei Momentaufnahmeaufgaben führen:

| Ursache | Lösung |
| ----- | ----- |
| Virtuelle Computer, auf denen die Microsoft SQL Server-Sicherung konfiguriert ist. Standardmäßig wird bei der Sicherung virtueller Computer eine vollständige VSS-Sicherung auf virtuellen Windows-Computern ausgeführt. Auf virtuellen Computern, auf denen SQL Server-basierte Server ausgeführt werden und auf denen SQL Server-Sicherung konfiguriert ist, können Verzögerungen bei der Ausführung von Momentaufnahmen auftreten. | Legen Sie den folgenden Registrierungsschlüssel fest, wenn aufgrund von Momentaufnahmeproblemen Fehler bei der Sicherung auftreten:<br><br>[HKEY\_LOCAL\_MACHINE\\SOFTWARE\\MICROSOFT\\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE" |
| Der VM-Status wird falsch gemeldet, da der virtuelle Computer im RDP heruntergefahren ist. Wenn Sie den virtuellen Computer im RDP heruntergefahren haben, sollten Sie im Portal überprüfen, ob der VM-Status richtig wiedergegeben wird. | Falls nicht, fahren Sie den virtuellen Computer im Portal mithilfe der Option „Herunterfahren“ auf dem VM-Dashboard herunter. |
| Viele virtuelle Computer desselben Clouddiensts sind so konfiguriert, dass die Sicherung zur selben Zeit durchgeführt wird. | Eine bewährte Methode besteht darin, für die virtuellen Computer eines Clouddiensts unterschiedliche Sicherungszeitpläne zu verwenden. |
| Der virtuelle Computer wird bei hoher CPU-/Arbeitsspeicherauslastung ausgeführt. | Wird der virtuelle Computer bei hoher CPU-Auslastung (über 90 Prozent) oder hoher Arbeitsspeicherauslastung ausgeführt, wird die Momentaufnahmeaufgabe der Warteschlange hinzugefügt und verzögert, bis schließlich eine Zeitüberschreitung auftritt. Versuchen Sie es in diesem Fall mit bedarfsgesteuerten Sicherungen. |
|Der virtuelle Computer kann keine Host-/Fabric-Adresse aus DHCP abrufen.|Für die VM-Sicherung mithilfe von IaaS muss im Gastbetriebssystem die DHCP-Option aktiviert sein. Wenn der virtuelle Computer die Host-/Fabric-Adresse nicht aus DHCP-Antwort 245 abrufen kann, können keine Erweiterungen heruntergeladen oder ausgeführt werden. Wenn Sie eine statische private IP-Adresse benötigen, sollten Sie diese über die Plattform konfigurieren. Die DHCP-Option innerhalb des virtuellen Computers sollte aktiviert bleiben. Lesen Sie weitere Informationen zum [Festlegen einer statischen internen privaten IP-Adresse](../virtual-network/virtual-networks-reserved-private-ip.md).|

<!---HONumber=AcomDC_0921_2016-->