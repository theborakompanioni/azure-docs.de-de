<properties
   pageTitle="Problembehandlung bei langsamer Sicherung von Dateien und Ordnern in Azure Backup | Microsoft Azure"
   description="Enthält Informationen zu Problembehandlungsschritten, mit denen Sie das Azure Backup-Leistungsproblem diagnostizieren und die Ursache eingrenzen können."
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="markgal"
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

Dieser Artikel enthält Informationen zu Problembehandlungsschritten, mit denen Sie eine niedrige Geschwindigkeit beim Sichern von Dateien und Ordnern mit Azure Backup diagnostizieren und die Ursache eingrenzen können. Wenn Sie den Azure Backup-Agent zum Sichern von Dateien verwenden, kann der Sicherungsprozess länger als erwartet dauern. Dieses Problem kann eine oder mehrere der folgenden Ursachen haben:

-	[Leistungsengpässe auf dem Computer, der gesichert wird](#cause1).
-	[Azure Backup wird durch einen anderen Prozess oder Antivirensoftware beeinträchtigt](#cause2).
-	[Der Backup-Agent wird auf einem virtuellen Azure-Computer (VM) ausgeführt](#cause3).
-	[Sie versuchen, eine große Zahl von Dateien zu sichern (mehrere Millionen)](#cause4).

Im nächsten Abschnitt wird beschrieben, wie Sie die genaue Ursache des Problems ermitteln können.

Bevor Sie mit der Problembehandlung beginnen, empfehlen wir Ihnen, den [aktuellen Azure Backup-Agent](http://aka.ms/azurebackup_agent) herunterzuladen und zu installieren. Wir stellen häufig Updates für den Backup-Agent zur Verfügung, um verschiedene Probleme zu beheben, Funktionen hinzuzufügen und die Leistung zu verbessern.

Wir legen Ihnen auch den Artikel [Azure Backup-Dienst – FAQ](backup-azure-backup-faq.md) ans Herz, damit Sie häufig auftretende Konfigurationsprobleme vermeiden können.

## Schritte zur Problembehandlung
<a id="cause1"></a>
## Ursache 1: Langsame Sicherung aufgrund von Leistungsengpässen auf dem zu sichernden Computer

### Lösung

Auf dem Computer, der gesichert werden soll, bestehen unter Umständen Engpässe, die zu Verzögerungen führen können. Beispiele hierfür sind die Fähigkeit des Computers, vom Datenträger zu lesen oder darauf zu schreiben, Bandbreiten zum Senden von Daten über das Netzwerk usw.

Windows verfügt über das integrierte Tool [Performance Monitor](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Leistungsüberwachung), um diese Engpässe zu erkennen. In der Tabelle unten sind die optimalen Leistungsindikatoren und Bereiche für Sicherungen angegeben.

Hier sind einige Leistungsindikatoren und Bereiche angegeben, die bei der Diagnose von Engpässen hilfreich sein können.

| Indikator | Status |
|---|---|
|Logical Disk(Physical Disk)--%idle | • 100% Leerlauf bis 50% Leerlauf = Fehlerfrei</br>• 49% Leerlauf bis 20% Leerlauf = Warnung oder Überwachen</br>• 19% Leerlauf bis 0% Leerlauf = Kritisch oder außerhalb der Spezifikation|
| Logical Disk(Physical Disk)--%Avg. Disk Sec Read or Write | • 0,001ms bis 0,015ms = Fehlerfrei</br>• 0,015ms bis 0,025 = Warnung oder Überwachen</br>• 0,026ms oder länger = Kritisch oder außerhalb der Spezifikation|
| Logical Disk(Physical Disk)--Current Disk Queue Length (für alle Instanzen) | 80 Anforderungen über einen Zeitraum von mehr als 6 Minuten |
| Memory--Pool Non Paged Bytes|• Weniger als 60% des Pools verbraucht = Fehlerfrei<br>• 61% - 80% des Pools verbraucht = Warnung oder Überwachen</br>• Mehr als 80% des Pools verbraucht = Kritisch oder außerhalb der Spezifikation|
| Memory--Pool Paged Bytes |• Weniger als 60% des Pools verbraucht = Fehlerfrei</br>• 61% - 80% des Pools verbraucht = Warnung oder Überwachen</br>• Mehr als 80% des Pools verbraucht = Kritisch oder außerhalb der Spezifikation|
| Memory--Available Megabytes| • 50% freier Arbeitsspeicher oder mehr = Fehlerfrei</br>• 25% freier Arbeitsspeicher = Überwachen</br>• 10% freier Arbeitsspeicher = Warnung</br>• Weniger als 100 MB oder 5% freier Arbeitsspeicher = Kritisch oder außerhalb der Spezifikation|
|Processor--\\%Processor Time (alle Instanzen)|• Weniger als 60% verbraucht = Fehlerfrei</br>• 61% - 90% verbraucht = Überwachen oder Vorsicht</br>• 91% - 100% verbraucht = Kritisch|


> [AZURE.NOTE] Falls die Infrastruktur als mögliche Ursache ermittelt wird, ist es ratsam, die geschützten Datenträger regelmäßig zu defragmentieren, um eine bessere Leistung zu erzielen.

<a id="cause2"></a>
## Ursache 2: Azure Backup wird durch einen anderen Prozess oder Antivirensoftware beeinträchtigt

Es sind mehrere Fälle bekannt, in denen sich andere Prozesse innerhalb des Windows-Systems negativ auf die Leistung des Azure Backup-Agent-Prozesses ausgewirkt haben. Falls Sie beispielsweise sowohl den Azure Backup-Agent als auch ein anderes Programm zum Sichern der Daten verwenden oder Antivirensoftware ausgeführt wird und die zu sichernden Dateien mit einer Sperre versehen wurden, können die mehrfachen Sperren der Dateien zu Konflikten führen. In diesem Fall kann bei der Sicherung ein Fehler auftreten, oder der Auftrag kann länger als erwartet dauern.

### Lösung

Bei diesem Szenario deaktivieren Sie am besten das andere Sicherungsprogramm, um zu testen, ob sich die Sicherungsdauer des Azure Backup-Agents ändert. Wenn Sie sicherstellen, dass nicht mehrere Sicherungsaufträge gleichzeitig ausgeführt werden, reicht dies normalerweise aus, um eine gegenseitige Behinderung zu vermeiden.

Für Antivirenprogramme empfehlen wir Ihnen, die folgenden Dateien und Speicherorte auszuschließen:

- Schließen Sie „C:\\Programme\\Microsoft Azure Recovery Services Agent\\bin\\cbengine.exe“ als Prozess aus.
- Schließen Sie die Ordner unter „C:\\Programme\\Microsoft Azure Recovery Services Agent\\“ aus.
- Schließen Sie das Scratchverzeichnis aus (falls Sie nicht den obigen Standardspeicherort verwenden).

<a id="cause3"></a>
## Ursache 3: Der Backup-Agent wird auf einem virtuellen Azure-Computer (VM) ausgeführt

### Lösung

Wenn Sie den Backup-Agent auf einem virtuellen Computer ausführen, ist die Leistung langsamer als bei Ausführung auf einem physischen Computer. Dies ist aufgrund der IOPS-Beschränkungen zu erwarten. Sie können die Leistung aber optimieren, indem Sie die Datenlaufwerke, die gesichert werden, auf Storage Premium umstellen. Wir arbeiten an der Behebung dieses Problems, und die Lösung wird Teil einer zukünftigen Version sein.

<a id="cause4"></a>
## Ursache 4: Sichern einer großen Zahl von Dateien (mehrere Millionen)

### Lösung

Es ist davon auszugehen, dass das Verschieben einer großen Datenmenge länger als das Verschieben einer kleineren Datenmenge dauert. Aber in einigen Fällen hängt die Dauer des Sicherungsvorgangs nicht nur von der Größe der Daten ab, sondern auch von der Anzahl von Dateien oder Ordnern. Dies gilt besonders, wenn Millionen von kleinen Dateien (einige Byte bis zu einigen Kilobyte) gesichert werden.

Dieses Verhalten tritt auf, weil beim Sichern der Daten und Verschieben der Daten nach Azure die Dateien gleichzeitig katalogisiert werden. In einigen selten Fällen kann der Katalogisierungsvorgang länger dauern.

Führen Sie die unten angegebenen Schritte aus, um den Engpass zu verstehen, und führen Sie dann entsprechend die nächsten Schritte aus:

a. **Benutzeroberfläche zeigt Status zur übertragenen Datenmenge an**: In diesem Fall werden die Daten weiterhin übertragen, und die Netzwerkbandbreite oder die Größe der Daten ist unter Umständen für die Verzögerungen verantwortlich.

b. **Benutzeroberfläche zeigt keinen Status an**: Öffnen Sie die Protokolle unter „C:\\Microsoft Azure Recovery Services Agent\\Temp“, und suchen Sie darin nach dem Eintrag „FileProvider::EndData“. Dieser Eintrag verdeutlicht, dass die Datenübertragung abgeschlossen ist und der Katalogisierungsvorgang durchgeführt wird. Brechen Sie die Sicherungsaufträge nicht ab, sondern warten Sie, bis die Katalogisierung abgeschlossen ist. Wenden Sie sich an den [Azure-Support](https://portal.azure.com/#create/Microsoft.Support), wenn das Problem weiterhin besteht.

<!---HONumber=AcomDC_0720_2016-->