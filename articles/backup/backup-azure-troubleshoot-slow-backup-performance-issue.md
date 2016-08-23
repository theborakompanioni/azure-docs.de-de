<properties
   pageTitle="Problembehandlung bei langsamer Sicherung von Dateien und Ordnern in Azure Backup | Microsoft Azure"
   description="Enthält Informationen zu Problembehandlungsschritten, mit denen Sie die Ursache von Azure Backup-Leistungsproblemen diagnostizieren können."
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="jimpark"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/20/2016"
    ms.author="genli"/>

# Problembehandlung bei langsamer Sicherung von Dateien und Ordnern in Azure Backup

Dieser Artikel enthält Informationen zu Problembehandlungsschritten, mit denen Sie eine niedrige Geschwindigkeit beim Sichern von Dateien und Ordnern mit Azure Backup diagnostizieren können. Wenn Sie den Azure Backup-Agent zum Sichern von Dateien verwenden, kann der Sicherungsprozess länger als erwartet dauern. Diese Verzögerung kann eine oder mehrere der folgenden Ursachen haben:

-	[Es treten Leistungsengpässe auf dem Computer auf, der gesichert wird.](#cause1)
-	[Azure Backup wird durch einen anderen Prozess oder Antivirensoftware beeinträchtigt.](#cause2)
-	[Der Backup-Agent wird auf einem virtuellen Azure-Computer (VM) ausgeführt.](#cause3)
-	[Sie sichern eine große Zahl von Dateien (mehrere Millionen).](#cause4)

Bevor Sie mit der Problembehandlung beginnen, empfehlen wir Ihnen, den [aktuellen Azure Backup-Agent](http://aka.ms/azurebackup_agent) herunterzuladen und zu installieren. Wir stellen häufig Updates für den Backup-Agent zur Verfügung, um verschiedene Probleme zu beheben, Funktionen hinzuzufügen und die Leistung zu verbessern.

Wir empfehlen Ihnen auch dringend den Artikel [Azure Backup-Dienst – FAQ](backup-azure-backup-faq.md), damit Sie häufig auftretende Konfigurationsprobleme vermeiden können.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>
## Ursache: Leistungsengpässe auf dem Computer

Leistungsengpässe auf dem Computer, der gesichert wird, können Verzögerungen verursachen. Die Fähigkeit des Computers, vom Datenträger zu lesen oder darauf zu schreiben, und die verfügbare Bandbreite zum Senden von Daten über das Netzwerk können beispielsweise für Engpässe sorgen.

Zum Erkennen dieser Engpässe bietet Windows ein integriertes Tool, den [Systemmonitor](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon).

Hier sind einige Leistungsindikatoren und Bereiche angegeben, die bei der Diagnose von Engpässen, um optimale Sicherungen zu erzielen, hilfreich sein können.

| Indikator | Status |
|---|---|
|Logical Disk(Physical Disk)--%idle | • 100% Leerlauf bis 50% Leerlauf = fehlerfrei</br>• 49% Leerlauf bis 20% Leerlauf = Warnung oder überwachen</br>• 19% Leerlauf bis 0% Leerlauf = kritisch oder außerhalb der Spezifikation|
| Logical Disk(Physical Disk)--%Avg. Disk Sec Read or Write | • 0,001ms bis 0,015 ms = fehlerfrei</br>• 0,015ms bis 0,025ms = Warnung oder überwachen</br>• 0,026ms oder mehr = kritisch oder außerhalb der Spezifikation|
| Logical Disk(Physical Disk)--Current Disk Queue Length (für alle Instanzen) | 80 Anforderungen über einen Zeitraum von mehr als 6 Minuten |
| Memory--Pool Non Paged Bytes|• Weniger als 60% des Pools verbraucht = fehlerfrei<br>• 61% - 80% des Pools verbraucht = Warnung oder überwachen</br>• Mehr als 80% des Pools verbraucht = kritisch oder außerhalb der Spezifikation|
| Memory--Pool Paged Bytes |• Weniger als 60% des Pools verbraucht = fehlerfrei</br>• 61% - 80% des Pools verbraucht = Warnung oder überwachen</br>• Mehr als 80% des Pools verbraucht = kritisch oder außerhalb der Spezifikation|
| Memory--Available Megabytes| • 50% freier Arbeitsspeicher oder mehr = fehlerfrei</br>• 25% freier Arbeitsspeicher = überwachen</br>• 10% freier Arbeitsspeicher verfügbar = Warnung</br>• Weniger als 100MB oder 5% freier Arbeitsspeicher verfügbar = kritisch oder außerhalb der Spezifikation|
|Processor--\\%Processor Time (alle Instanzen)|• Weniger als 60% verbraucht = fehlerfrei</br>• 61% - 90% verbraucht = überwachen oder Vorsicht</br>• 91% - 100% verbraucht = kritisch|


> [AZURE.NOTE] Wenn Sie feststellen, dass die Infrastruktur die Ursache ist, empfehlen wir, die Datenträger in regelmäßigen Abständen für eine bessere Leistung zu defragmentieren.

<a id="cause2"></a>
## Ursache: Azure Backup wird durch einen anderen Prozess oder Antivirensoftware beeinträchtigt

Es sind mehrere Fälle bekannt, in denen sich andere Prozesse innerhalb des Windows-Systems negativ auf die Leistung des Azure Backup-Agent-Prozesses ausgewirkt haben. Falls Sie beispielsweise sowohl den Azure Backup-Agent als auch ein anderes Programm zum Sichern der Daten verwenden, oder Antivirensoftware ausgeführt wird, und die zu sichernden Dateien mit einer Sperre versehen wurden, können die mehrfachen Sperren der Dateien zu Konflikten führen. In diesem Fall kann bei der Sicherung ein Fehler auftreten, oder der Auftrag kann länger als erwartet dauern.

Bei diesem Szenario deaktivieren Sie am besten das andere Sicherungsprogramm, um zu testen, ob sich die Sicherungsdauer des Azure Backup-Agents ändert. Wenn Sie sicherstellen, dass nicht mehrere Sicherungsaufträge gleichzeitig ausgeführt werden, reicht dies normalerweise aus, um eine gegenseitige Behinderung zu vermeiden.

Für Antivirenprogramme empfehlen wir Ihnen, die folgenden Dateien und Speicherorte auszuschließen:

- „C:\\Programme\\Microsoft Azure Recovery Services Agent\\bin\\cbengine.exe“
- „C:\\Programme\\Microsoft Azure Recovery Services Agent\\“
- Scratchverzeichnis (falls Sie nicht den Standardspeicherort verwenden).

<a id="cause3"></a>
## Ursache: Der Backup-Agent wird auf einem virtuellen Azure-Computer ausgeführt

Wenn Sie den Backup-Agent auf einem virtuellen Computer ausführen, ist die Leistung niedriger als bei Ausführung auf einem physischen Computer. Dies ist aufgrund der IOPS-Beschränkungen zu erwarten. Sie können die Leistung aber optimieren, indem Sie die Datenlaufwerke, die gesichert werden, auf Azure Storage Premium umstellen. Wir arbeiten an der Behebung dieses Problems, und die Lösung wird Teil einer zukünftigen Version sein.

<a id="cause4"></a>
## Ursache: Sichern einer großen Zahl von Dateien (mehrere Millionen)

Das Verschieben einer großen Datenmenge dauert länger als das Verschieben einer kleineren Datenmenge . In einigen Fällen hängt die Sicherungszeit nicht nur von der Datenmenge ab, sondern auch von der Anzahl der Dateien oder Ordner. Dies ist insbesondere dann der Fall, wenn Millionen von kleinen Dateien (ein paar Bytes bis wenige Kilobytes) gesichert werden.

Dieses Verhalten tritt auf, weil Azure simultan Ihre Dateien katalogisiert, während Sie die Daten sichern und in Azure verschieben. In einigen seltenen Fällen kann die Katalogisierung länger als erwartet dauern.

Anhand der folgenden Indikatoren können Sie den Engpass identifizieren und dann die entsprechenden nächsten Schritte ausführen:

- **Auf der Benutzeroberfläche wird der Status der Datenübertragung angezeigt**. Die Daten werden immer noch übertragen. Die Netzwerkbandbreite oder die Datenmenge könnte möglicherweise Verzögerungen verursachen.

- **Auf der Benutzeroberfläche wird der Status der Datenübertragung nicht angezeigt**. Öffnen Sie die Protokolle unter „C:\\Microsoft Azure Recovery Services Agent\\Temp“, und suchen Sie in den Protokollen nach dem Eintrag „FileProvider::EndData“. Dieser Eintrag verdeutlicht, dass die Datenübertragung abgeschlossen ist und der Katalogisierungsvorgang durchgeführt wird. Brechen Sie die Sicherungsaufträge nicht ab. Warten Sie stattdessen noch ein wenig, bis die Katalogisierung beendet ist. Wenden Sie sich an den [Azure-Support](https://portal.azure.com/#create/Microsoft.Support), wenn das Problem weiterhin besteht.

<!---HONumber=AcomDC_0810_2016-->