---
title: Azure Security Center-Datensicherheit | Microsoft Docs
description: "In diesem Dokument wird erläutert, wie Daten in Azure Security Center verwaltet und geschützt werden."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 33f2c9f4-21aa-4f0c-9e5e-4cd1223e39d7
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/06/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 9852981e530cd147c2d34ac2ede251b58a167a0a
ms.openlocfilehash: 5c030f463b21284c15752cf95aa1f9a75f17ffb0


---
# <a name="azure-security-center-data-security"></a>Azure Security Center-Datensicherheit
Azure Security Center erfasst und verarbeitet sicherheitsbezogene Daten (einschließlich Konfigurationsinformationen, Metadaten, Ereignisprotokolle, Absturzabbilddateien und Ähnliches), um Kunden bei der Vermeidung, Erkennung und Behandlung von Bedrohungen zu unterstützen. Microsoft hält strenge Compliance- und Sicherheitsrichtlinien ein – angefangen bei der Codierung bis hin zum Betreiben von Diensten.

In diesem Artikel wird erläutert, wie Daten in Azure Security Center verwaltet und geschützt werden.


## <a name="data-sources"></a>Datenquellen
Azure Security Center analysiert Daten aus den folgenden Quellen, um über den Sicherheitsstatus zu informieren, Sicherheitslücken zu identifizieren, Gegenmaßnahmen zu empfehlen und aktive Bedrohungen zu erkennen:

- Azure Services: Verwendet Informationen zur Konfiguration von Azure-Diensten, die Sie bereitgestellt haben. Hierzu wird mit dem Ressourcenanbieter des Diensts kommuniziert.
- Netzwerkdatenverkehr: Verwendet Metadatenstichproben des Netzwerkdatenverkehrs aus der Infrastruktur von Microsoft wie etwa Quelle/Ziel, IP/Port, Paketgröße und Netzwerkprotokoll.
- Partnerlösungen: Verwendet Sicherheitswarnungen von integrierten Partnerlösungen (beispielsweise Firewalls und Antischadsoftwarelösungen).
- Ihre virtuellen Computer: Verwendet Konfigurationsinformationen und Informationen zu Sicherheitsereignissen – beispielsweise Windows-Ereignis- und Überwachungsprotokolle, IIS-Protokolle, Syslog-Nachrichten und Absturzabbilddateien von Ihren virtuellen Computern.


## <a name="data-protection"></a>Datenschutz
**Trennung von Daten:**Daten werden für jede Komponente des Diensts logisch getrennt verwaltet. Sämtliche Daten werden nach Organisation gekennzeichnet. Dieser Kennzeichnung wird während des gesamten Datenlebenszyklus beibehalten und auf jeder Ebene des Diensts erzwungen.

**Datenzugriff:** Bei der Bereitstellung von Sicherheitsempfehlungen sowie bei der Untersuchung potenzieller Sicherheitsrisiken greifen Mitarbeiter von Microsoft unter Umständen auf Informationen zu, die von Azure-Diensten erfasst oder analysiert wurden. Hierzu zählen etwa Absturzabbilddateien, Prozesserstellungsereignisse, Momentaufnahmen von VM-Datenträgern und Artefakte. Diese können ggf. Kundendaten oder persönliche Informationen von Ihren virtuellen Computern enthalten. Wir halten uns an die [Microsoft Online Services-Bedingungen und Datenschutzerklärung](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31). Darin ist festgelegt, dass Microsoft keine Kundendaten oder daraus abgeleiteten Informationen zu Werbezwecken oder anderen kommerziellen Zwecken verwendet. Wir verwenden Kundendaten nur, wenn dies für die Bereitstellung Ihrer Azure-Dienste erforderlich ist. Dies gilt auch für Zwecke, die mit der Bereitstellung dieser Dienste kompatibel sind. Alle Rechte an den Kundendaten verbleiben bei Ihnen.

**Datennutzung:** Microsoft nutzt mandantenübergreifende Muster und Informationen zu Bedrohungen (Threat Intelligence), um die Funktionen für Prävention und Erkennung zu verbessern. Dies erfolgt in Übereinstimmung mit den in unserer [Datenschutzerklärung](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx) beschriebenen Datenschutzzusagen.

## <a name="data-location"></a>Speicherort der Daten
**Ihre Speicherkonten:** Ein Speicherkonto wird für jede Region angegeben, in der virtuelle Computer ausgeführt werden. So können Sie Daten in derselben Region speichern, in der auch der virtuelle Computer angeordnet ist, von dem die Daten erfasst werden. Diese Daten, z.B. Absturzabbilddateien, werden in Ihrem Speicherkonto dauerhaft gespeichert. Momentaufnahmen von VM-Datenträgern werden im gleichen Speicherkonto gespeichert wie der VM-Datenträger.

**Azure Security Center-Speicher:** Informationen zu Sicherheitswarnungen (einschließlich Partnerwarnungen, Empfehlungen und Informationen zum Sicherheitsintegritätsstatus) werden zentral gespeichert (aktuell in den USA). Diese Informationen können auch verwandte Konfigurationsinformationen und Sicherheitsereignisse umfassen, die je nach Bedarf von Ihren virtuellen Computern erfasst werden, um für Sie die Sicherheitswarnung, die Empfehlung oder den Sicherheitsintegritätsstatus bereitzustellen.

Azure Security Center erfasst kurzlebige Kopien Ihrer Absturzabbilddateien und analysiert sie, um nach Spuren von Exploitversuchen und erfolgreichen Kompromittierungen zu suchen. Azure Security Center führt diese Analyse in dem geografischen Raum durch, in dem sich auch der Arbeitsbereich befindet, und löscht die kurzlebigen Kopien nach Abschluss der Analyse.

Computerartefakte werden zentral in der Region gespeichert, in der sich auch der virtuelle Computer befindet.


## <a name="managing-data-collection-from-virtual-machines"></a>Verwalten der Datensammlung von virtuellen Computern
Wenn Sie sich für die Aktivierung von Azure Security Center entscheiden, wird die Datensammlung für alle Ihre Abonnements aktiviert. Sie können die Datensammlung auch in Azure Security Center im Abschnitt „Sicherheitsrichtlinie“ aktivieren. Wenn die Datensammlung aktiviert ist, stellt Azure Security Center den Azure-Überwachungs-Agent auf allen vorhandenen unterstützten VMs sowie auf allen neuen VMs bereit, die erstellt werden. Die Sicherheitsüberwachungserweiterung von Azure sucht in ETW-Ablaufverfolgungen (Event Tracing for Windows, [Ereignisablaufverfolgung für Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx)) nach verschiedenen sicherheitsbezogenen Konfigurationen und Ereignissen. Außerdem löst das Betriebssystem während der Ausführung des Computers Ereignisprotokollereignisse aus. Beispiele für Daten dieser Art: Betriebssystemtyp und -version, Betriebssystemprotokolle (Windows-Ereignisprotokolle), ausgeführte Prozesse, Computername, IP-Adressen, angemeldeter Benutzer und Mandanten-ID. Der Azure-Überwachungs-Agent liest Ereignisprotokolleinträge und ETW-Ablaufverfolgungen und kopiert diese zur Analyse in Ihr Speicherkonto.

Die Datensammlung von virtuellen Computern kann jederzeit deaktiviert werden. Dadurch werden sämtliche Überwachungs-Agents entfernt, die von Azure Security Center zuvor installiert wurden. Momentaufnahmen von VM-Datenträgern sowie die Artefaktsammlung sind auch bei deaktivierter Datensammlung aktiviert.


## <a name="see-also"></a>Siehe auch
In diesem Dokument haben Sie erfahren, wie Daten in Azure Security Center verwaltet und geschützt werden. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Quellen:

* [Planungs- und Betriebshandbuch für Azure Security Center:](security-center-planning-and-operations-guide.md) Hier erfahren Sie, wie Sie die Einführung von Azure Security Center planen, und erhalten grundlegende Informationen zu Entwurfsaspekten.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md) : Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) : Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md) : Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.



<!--HONumber=Feb17_HO1-->


