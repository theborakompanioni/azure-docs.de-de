<properties
   pageTitle="Azure Security Center-Datensicherheit | Microsoft Azure"
   description="In diesem Dokument wird erläutert, wie Daten in Azure Security Center verwaltet und geschützt werden."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="yurid"/>

# Azure Security Center-Datensicherheit
Azure Security Center erfasst und verarbeitet Daten zu Ihren Azure-Ressourcen (z.B. Konfigurationsinformationen, Metadaten, Ereignisprotokolle, Absturzabbilddateien und mehr), um Kunden bei der Vermeidung, Erkennung und Behandlung von Bedrohungen zu unterstützen. Wir unternehmen große Anstrengungen, um für den Datenschutz und die Sicherheit dieser Daten zu sorgen. Microsoft hält strenge Compliance- und Sicherheitsrichtlinien ein – angefangen bei der Codierung bis hin zum Betreiben von Diensten.

In diesem Artikel wird erläutert, wie Daten in Azure Security Center verwaltet und geschützt werden.

## Datenquellen
Azure Security Center analysiert Daten aus den folgenden Quellen:

- Azure Services: Liest Informationen zur Konfiguration von Azure-Diensten, die Sie bereitgestellt haben, indem mit dem Ressourcenanbieter des Diensts kommuniziert wird.
- Netzwerk-Datenverkehr: Liest Stichproben von Netzwerk-Datenverkehr-Metadaten aus der Infrastruktur von Microsoft, z.B. Quelle/Ziel, IP/Port, Paketgröße und Netzwerkprotokoll.
- Partnerlösungen: Erfasst Sicherheitswarnungen von integrierten Partnerlösungen, z.B. Firewalls und Antischadsoftware-Lösungen. Diese Daten werden in Azure Security Center-Speicher gespeichert, der sich derzeit in den USA befindet.
- Ihre virtuellen Computer: Azure Security Center kann Konfigurationsinformationen und Informationen zu Sicherheitsereignissen, z.B. Windows-Ereignis- und -Überwachungsprotokolle, IIS-Protokolle, Syslog-Nachrichten und Absturzabbilddateien, von Ihren virtuellen Computern mithilfe von Agents für die Datensammlung erfassen. Unten im Abschnitt „Verwalten der Datensammlung“ finden Sie hierzu weitere Informationen.

Außerdem werden Informationen zu Sicherheitswarnungen, Empfehlungen und zum Sicherheitsintegritätsstatus in Azure Security Center-Speicher gespeichert, der sich derzeit in den USA befindet. Diese Informationen können auch verwandte Konfigurationsinformationen und Sicherheitsereignisse umfassen, die je nach Bedarf von Ihren virtuellen Computern erfasst werden, um für Sie die Sicherheitswarnung, die Empfehlung oder den Sicherheitsintegritätsstatus bereitzustellen.

## Datenschutz
**Trennung von Daten:** Daten werden für jede Komponente des Diensts logisch getrennt verwaltet. Sämtliche Daten werden nach Organisation gekennzeichnet. Dieser Kennzeichnung wird während des gesamten Datenlebenszyklus beibehalten und auf jeder Ebene des Diensts erzwungen. Außerdem werden Daten, die von Ihren virtuellen Computern erfasst werden, in Ihren Speicherkonten gespeichert.

**Datenzugriff:** Um Sicherheitsempfehlungen bereitstellen und potenzielle Sicherheitsrisiken untersuchen zu können, greifen Mitarbeiter von Microsoft unter Umständen auf Informationen zu, die von den Azure-Diensten erfasst oder analysiert wurden (beispielsweise Absturzabbilddateien). Absturzabbilddateien und Prozesserstellungsereignisse können versehentlich Kundendaten oder persönliche Daten von Ihren virtuellen Computern enthalten. Wir halten uns an die [Microsoft Online Services-Bedingungen](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) und die [Datenschutzerklärung](https://www.microsoft.com/privacystatement/de-DE/OnlineServices/Default.aspx), in denen festgelegt ist, dass Microsoft keine Kundendaten oder daraus abgeleiteten Informationen zu Werbezwecken oder anderen kommerziellen Zwecken verwendet. Wir verwenden Kundendaten nur, wenn dies für die Bereitstellung Ihrer Azure-Dienste erforderlich ist. Dies gilt auch für Zwecke, die mit der Bereitstellung dieser Dienste kompatibel sind. Alle Rechte an den Kundendaten verbleiben bei Ihnen.

**Datennutzung:** Microsoft nutzt mandantenübergreifende Muster und Informationen zu Bedrohungen (Threat Intelligence), um die Funktionen für Prävention und Erkennung zu verbessern. Dies erfolgt in Übereinstimmung mit den in unserer [Datenschutzerklärung](https://www.microsoft.com/privacystatement/de-DE/OnlineServices/Default.aspx) beschriebenen Datenschutzzusagen.

**Datenspeicherort:** Ein Speicherkonto wird für jede Region angegeben, in der virtuelle Computer ausgeführt werden. So können Sie Daten in derselben Region speichern, in der auch der virtuelle Computer angeordnet ist, von dem die Daten erfasst werden. Diese Daten, z.B. Absturzabbilddateien, werden in Ihrem Speicherkonto dauerhaft gespeichert. Der Dienst speichert auch Informationen zu Sicherheitswarnungen, z.B. Warnungen von integrierten Partnerlösungen, Empfehlungen und den Sicherheitsintegritätsstatus, in Azure Security Center-Speicher, der sich derzeit in den USA befindet.

## Verwalten der Datensammlung von virtuellen Computern

Wenn Sie sich für die Aktivierung von Azure Security Center entscheiden, wird die Datensammlung für alle Ihre Abonnements aktiviert. Sie können die Datensammlung im Azure Security Center-Dashboard im Abschnitt „Sicherheitsrichtlinie“ deaktivieren. Wenn die Datensammlung aktiviert ist, stellt Azure Security Center den Azure-Überwachungs-Agent auf allen vorhandenen unterstützten VMs sowie auf allen neuen VMs bereit, die erstellt werden. Die Sicherheitsüberwachungserweiterung von Azure sucht in ETW-Ablaufverfolgungen (Event Tracing for Windows, [Ereignisablaufverfolgung für Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx)) nach verschiedenen sicherheitsbezogenen Konfigurationen und Ereignissen. Außerdem löst das Betriebssystem während der Ausführung des Computers Ereignisprotokollereignisse aus. Beispiele für Daten dieser Art: Betriebssystemtyp und -version, Betriebssystemprotokolle (Windows-Ereignisprotokolle), ausgeführte Prozesse, Computername, IP-Adressen, angemeldeter Benutzer und Mandanten-ID. Der Azure-Überwachungs-Agent liest Ereignisprotokolleinträge und ETW-Ablaufverfolgungen und kopiert diese zur Analyse in Ihr Speicherkonto.

Ein Speicherkonto wird für jede Region angegeben, in der Sie virtuelle Computer ausführen. In den Speicherkonten werden die Daten gespeichert, die von virtuellen Computern in derselben Region erfasst werden. Dies macht es einfach für Sie, Daten aus Datenschutz- und Datenhoheitszwecken im selben geografischen Gebiet zu speichern. Sie können Speicherkonten für jede Region im Azure Security Center-Dashboard im Abschnitt „Sicherheitsrichtlinie“ konfigurieren.

Darüber hinaus kopiert der Azure-Überwachungs-Agent Absturzabbilddateien in Ihr Speicherkonto. Azure Security Center erfasst kurzlebige Kopien Ihrer Absturzabbilddateien und analysiert sie, um nach Spuren von Exploitversuchen und erfolgreichen Kompromittierungen zu suchen. Azure Security Center führt diese Analyse in derselben geografischen Region durch, in der sich auch das Speicherkonto befindet, und löscht die kurzlebigen Kopien nach Abschluss der Analyse.

Die Datensammlung von virtuellen Computern kann jederzeit deaktiviert werden. Dadurch werden sämtliche Überwachungs-Agents entfernt, die von Azure Security Center zuvor installiert wurden.


## Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Daten in Azure Security Center verwaltet und geschützt werden. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Quellen:

- [Planungs- und Betriebshandbuch für Azure Security Center:](security-center-planning-and-operations-guide.md) Hier erfahren Sie, wie Sie die Einführung von Azure Security Center planen, und erhalten grundlegende Informationen zu Entwurfsaspekten.
- [Überwachen der Sicherheitsintegrität in Azure Security Center:](security-center-monitoring.md) Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center:](security-center-managing-and-responding-alerts.md) Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
- [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Hier erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
- [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.

<!---HONumber=AcomDC_0817_2016-->