---
title: 'Hybrid Runbook Worker: Ein Runbookauftrag wird mit dem Status „Angehalten“ beendet | Microsoft Docs'
description: Symptome, Ursachen und Lösungsmöglichkeiten für Abbruchfehler bei Hybrid Runbook Worker-Aufträgen.
services: automation
documentationcenter: ''
author: mgoedtel
manager: jwhit
editor: tysonn

ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/17/2016
ms.author: magoedte

---
# Hybrid Runbook Worker: Ein Runbookauftrag wird mit dem Status „Angehalten“ beendet
## Zusammenfassung
Kurz nachdem Sie versucht haben, Ihr Runbook dreimal auszuführen, wird es angehalten. Bestimmte Bedingungen können eine erfolgreiche Ausführung des Runbooks verhindern. Die Fehlermeldung enthält jedoch keine zusätzlichen Angaben zu den Gründen für den Abbruch. In diesem Artikel finden Sie Anweisungen, wie Sie Probleme bei der Runbookausführung mit Hybrid Runbook Worker lösen können.

Falls Sie Ihr Azure-Problem mit den Informationen in diesem Artikel nicht beheben konnten, suchen Sie in den Azure-Foren zu [MSDN und Stack Overflow](https://azure.microsoft.com/support/forums/). Sie können Ihr Problem in diesen Foren veröffentlichen oder es auf Twitter an [@AzureSupport](https://twitter.com/AzureSupport) senden. Darüber hinaus können Sie eine Azure-Supportanfrage stellen, indem Sie auf der [Website des Azure-Supports](https://azure.microsoft.com/support/options/) die Option **Support erhalten** auswählen.

## Symptom
Bei der Ausführung eines Runbooks tritt ein Fehler auf. Folgende Fehlermeldung wird ausgegeben: „Die Auftragsaktion ‚Aktivieren‘ kann nicht ausgeführt werden, weil der Prozess unerwartet beendet wurde. Es wurde 3 Mal versucht, die Auftragsaktion auszuführen.“

## Ursache
Es gibt mehrere mögliche Ursachen für den Fehler:

1. Der Hybrid Worker befindet sich hinter einem Proxy oder einer Firewall.
2. Der Computer, auf dem der Hybrid Worker ausgeführt wird, erfüllt die [Mindestanforderungen](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-requirements) an die Hardware nicht.
3. Die Runbooks können nicht mit lokalen Ressourcen authentifiziert werden.

## Ursache 1: Der Hybrid Runbook Worker befindet sich hinter einem Proxy oder einer Firewall
Der Computer, auf dem der Hybrid Runbook Worker ausgeführt wird, befindet sich hinter einer Firewall oder hinter einem Proxyserver, und ein ausgehender Netzwerkzugriff ist möglicherweise nicht zulässig oder nicht ordnungsgemäß konfiguriert.

### Lösung
Stellen Sie sicher, dass der Computer über einen ausgehenden Zugriff auf „*.cloudapp.net“ über die Ports 443, 9354 und 30000-30199 verfügt.

## Ursache 2: Der Computer erfüllt die Mindestanforderungen an die Hardware nicht
Computer, auf denen der Hybrid Runbook Worker ausgeführt werden soll, müssen die Mindestanforderungen an die Hardware erfüllen, damit sie als Host für dieses Feature infrage kommen. Andernfalls kommt es – je nach Ressourcennutzung durch andere Hintergrundprozesse und möglichen Konflikten bei der Runbookausführung – zu einer übermäßigen Auslastung des Computers. Dies verursacht Verzögerungen und Timeouts bei Runbookaufträgen.

### Lösung
Vergewissern Sie sich zuerst, dass der für die Ausführung des Hybrid Runbook Worker-Features vorgesehene Computer die Hardwaremindestanforderungen erfüllt. Wenn dies der Fall ist, überwachen Sie die CPU- und Arbeitsspeicherauslastung, um Zusammenhänge zwischen der Leistung der Hybrid Runbook Worker-Prozesse und Windows zu erkennen. Falls die Leistungsgrenzen des Arbeitsspeichers oder der CPU fast erreicht werden, deutet dies u.U. darauf hin, dass Sie ein Prozessorupgrade oder weitere Prozessoren oder mehr Speicher benötigen, um den Ressourcenengpass zu beseitigen und den Fehler zu beheben. Alternativ können Sie auch eine andere Computeressource auswählen, die die Mindestanforderungen erfüllt. Bei entsprechend großem Workload skalieren Sie sie nach Bedarf.

## Ursache 3: Die Runbooks können nicht mit lokalen Ressourcen authentifiziert werden.
### Lösung
Prüfen Sie, ob im Ereignisprotokoll **Microsoft-SMA** ein entsprechendes Ereignis mit der Beschreibung *Win32-Prozess wurde mit dem Code [4294967295] beendet* vorhanden ist. Die Ursache dieses Fehlers liegt darin, dass Sie in Ihren Runbooks die Authentifizierung nicht konfiguriert haben oder dass Sie die „Ausführen als“-Anmeldeinformationen für die Hybrid Worker-Gruppe nicht angegeben haben. Überprüfen Sie die [Runbookberechtigungen](automation-hybrid-runbook-worker.md#runbook-permissions), und vergewissern Sie sich, dass Sie die Authentifizierung für Ihre Runbooks ordnungsgemäß konfiguriert haben.

<!---HONumber=AcomDC_0817_2016-->