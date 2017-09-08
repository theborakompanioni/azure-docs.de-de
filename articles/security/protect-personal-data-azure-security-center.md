---
title: "Schützen personenbezogener Daten mit Azure Security Center | Microsoft-Dokumentation"
description: "Schützen personenbezogener Daten mit Azure Security Center"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: 7456da29aa07372156f2b9c08ab83626dab7cc45
ms.openlocfilehash: 8738ffc13734437f4c61cf7be76590e5d73adf42
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="protect-personal-data-from-breaches-and-attacks-azure-security-center"></a>Schützen personenbezogener Daten vor Sicherheitsverletzungen und Angriffen: Azure Security Center

Dieser Artikel enthält Informationen zur Verwendung von Azure Security Center zum Schützen personenbezogener Daten vor Sicherheitsverletzungen und Angriffen.

## <a name="scenario"></a>Szenario 

Ein großes Kreuzfahrtunternehmen mit Hauptsitz in den USA expandiert und bietet Routen im Mittelmeer und der Ostsee sowie zu den Britischen Inseln an. Zu diesem Zweck hat das Unternehmen mehrere kleinere Kreuzfahrtunternehmen in Italien, Deutschland, Dänemark und Großbritannien erworben.

Das Unternehmen verwendet Microsoft Azure zum Speichern von Unternehmensdaten in der Cloud. Dies umfasst personenbezogene Informationen, z.B. Namen, Adressen, Telefonnummern und Kreditkarteninformationen. Außerdem gehören dazu Informationen der Personalverwaltung, beispielsweise:

- Adressen
- Telefonnummern
- Steueridentifikationsnummern
- Medizinische Informationen

Das Kreuzfahrtunternehmen verwaltet zudem eine umfangreiche Datenbank für Mitglieder des Bonus- und Treueprogramms. Unternehmensmitarbeiter greifen in den externen Geschäftsstellen des Unternehmens auf das Netzwerk zu. Und Reiseveranstalter weltweit haben Zugriff auf bestimmte Unternehmensressourcen.
Personenbezogene Daten werden über das Netzwerk zwischen diesen Standorten und dem Microsoft-Rechenzentrum übertragen.

## <a name="problem-statement"></a>Problembeschreibung

Das Unternehmen sorgt sich um die Bedrohung durch Angriffe auf seine Azure-Ressourcen. Die Offenlegung der personenbezogenen Daten von Kunden und Mitarbeitern durch nicht autorisierte Personen soll verhindert werden. Das Unternehmen benötigt Leitlinien zur Prävention und zur Reaktion/Korrektur sowie eine effektive Möglichkeit zur kontinuierlichen Überwachung der Sicherheit seiner Cloudressourcen.
Es benötigt einen starken Schutz vor den heutigen ausgefeilten und organisierten Methoden der Angreifer.

## <a name="company-goal"></a>Unternehmensziel

Ein Ziel des Unternehmens besteht darin, den Datenschutz personenbezogener Daten von Kunden und Mitarbeitern durch den Schutz der Daten vor Bedrohungen sicherzustellen. Ein weiteres Unternehmensziel ist die sofortige Reaktion auf Anzeichen einer Sicherheitsverletzung, um die Auswirkungen zu minimieren. Voraussetzung hierfür ist eine Möglichkeit, den aktuellen Sicherheitsstatus zu bewerten und anfällige Konfigurationen zu identifizieren und zu korrigieren.

## <a name="solutions"></a>Lösungen

Microsoft Azure Security Center (ASC) bietet eine Lösung zur integrierten Sicherheitsüberwachung und Richtlinienverwaltung. Dieser Dienst stellt benutzerfreundliche und effektive Funktionen zum Schutz vor Bedrohungen und die entsprechenden Reaktionen bereit.

### <a name="prevention"></a>Prävention

Mit ASC können Sie Sicherheitsverletzungen vorbeugen, da Sie Sicherheitsrichtlinien festlegen, Just-in-Time-Zugriff gewähren und Sicherheitsempfehlungen implementieren können.

In einer Sicherheitsrichtlinie werden die Sicherheitsmechanismen definiert, die für Ressourcen in dem angegebenen Abonnement zu empfehlen sind. Über den Just-in-Time-Zugriff kann eingehender Datenverkehr für Ihre virtuellen Azure-Computer gesperrt und so die Gefährdung durch Angriffe reduziert werden. Sicherheitsempfehlungen werden in ASC nach dem Analysieren des Sicherheitsstatus der Azure-Ressourcen erstellt.

#### <a name="how-do-i-set-security-policies-in-asc"></a>Wie lege ich Sicherheitsrichtlinien in ASC fest?

Sie können Sicherheitsrichtlinien für jedes Abonnement konfigurieren. Damit Sie eine Sicherheitsrichtlinie ändern können, müssen Sie ein Besitzer oder Mitwirkender dieses Abonnements sein. Führen Sie im Azure-Portal folgende Schritte aus:

1. Wählen Sie im ASC-Dashboard die Option **Richtlinie** aus.

2. Wählen Sie das Abonnement aus, für das Sie die Richtlinie aktivieren möchten.

3. Wählen Sie **Präventionsrichtlinie** aus, um Richtlinien auf Abonnementebene zu konfigurieren. **Datensammlung für virtuelle Computer** muss auf **Ein** festgelegt sein.

4. Wählen Sie für **Präventionsrichtlinie** die Option **Ein** aus, um die Sicherheitsempfehlungen zu aktivieren, die für das Abonnement relevant sind.

![](media/protect-personal-data-azure-security-center/prevention-policy.png)

Ausführlichere Anweisungen und eine Erläuterung der einzelnen Richtlinienempfehlungen, die aktiviert werden können, finden Sie unter [Festlegen von Sicherheitsrichtlinien in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-policies#set-security-policies).

#### <a name="how-do-i-configure-just-in-time-access-jit"></a>Wie konfiguriere ich den Just-In-Time-Zugriff (JIT)?

Wenn JIT aktiviert ist, sperrt das Security Center durch das Erstellen einer NSG-Regel eingehenden Datenverkehr auf den virtuellen Azure-Computern. Sie wählen die Ports auf dem virtuellen Computer aus, für die eingehender Datenverkehr gesperrt wird. Führen Sie folgende Schritte aus, um den JIT-Zugriff zu verwenden:

1. Wählen Sie auf dem ASC-Blatt die Kachel **JIT-VM-Zugriff** aus.

2. Wählen Sie die Registerkarte **Empfohlen** aus.

3. Wählen Sie unter **VMs** die virtuellen Computer aus, die Sie aktivieren möchten. Hierdurch wird der virtuelle Computer mit einem Häkchen markiert. 
4. Wählen Sie **JIT auf VMs aktivieren** aus.
5. Wählen Sie **Speichern** aus.

Dann können Sie die Standardports sehen, deren Aktivierung in ASC für JIT empfohlen wird. Sie können auch einen neuen Port hinzufügen und konfigurieren, für den Sie die Just-In-Time-Lösung aktivieren möchten. Auf der Kachel **JIT-VM-Zugriff** im Security Center wird die Anzahl der für den JIT-Zugriff konfigurierten virtuellen Computer angezeigt. Zudem wird die Anzahl der genehmigten in der letzten Woche vorgenommenen Zugriffsanforderungen angezeigt.

![](media/protect-personal-data-azure-security-center/jit-vm.png)

Anweisungen hierzu und weitere Informationen zum Just-in-Time-Zugriff finden Sie unter [Verwalten des Zugriffs auf virtuelle Computer mithilfe des Just-In-Time-Features](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).

#### <a name="how-do-i-implement-asc-security-recommendations"></a>Wie implementiere ich ASC-Sicherheitsempfehlungen?

Wenn Security Center potenzielle Sicherheitsrisiken identifiziert, werden Empfehlungen erstellt. Entsprechend den Empfehlungen werden Sie durch den Prozess der Konfiguration des erforderlichen Sicherheitsmechanismus geführt. 
1. Wählen Sie im ASC-Dashboard die Kachel **Empfehlungen** aus.

2. Überprüfen Sie die Empfehlungen, die in einem Tabellenformat angezeigt werden, in dem jede Zeile eine bestimmte Empfehlung darstellt.

3. Um Empfehlungen zu filtern, wählen Sie **Filter** und dann die gewünschten Werte für den Schweregrad und Status aus.

4. Um eine Empfehlung zu verwerfen, die nicht anwendbar ist, können Sie mit der rechten Maustaste darauf klicken und **Verwerfen** auswählen.

5. Bestimmen Sie, welche Empfehlung zuerst angewendet werden soll.

6. Wenden Sie die Empfehlungen in der Reihenfolge ihrer Priorität an.

Eine Liste der möglichen Empfehlungen und exemplarische Vorgehensweisen zum Anwenden finden Sie unter [Verwalten von Sicherheitsempfehlungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

### <a name="detection-and-response"></a>Erkennung und Reaktion

Erkennung und Reaktion gehören zusammen, da nach der Erkennung einer Bedrohung möglichst schnell eine Reaktion erfolgen soll.
Bei der ASC-Bedrohungserkennung werden automatisch Sicherheitsinformationen von Ihren Azure-Ressourcen, aus dem Netzwerk und von verbundenen Partnerlösungen gesammelt. Die ASC-Erkennungsalgorithmen können schnell aktualisiert werden, wenn Angreifer neue und immer anspruchsvollere Exploit-Verfahren nutzen. Ausführlichere Informationen zur Funktionsweise der ASC-Bedrohungserkennung finden Sie unter [Azure Security Center-Erkennungsfunktionen](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities).

#### <a name="how-do-i-manage-and-respond-to-security-alerts"></a>Wie behandle ich Sicherheitswarnungen und reagiere darauf?

Eine Liste mit priorisierten Sicherheitswarnungen wird im Security Center zusammen mit den Informationen angezeigt, die Sie zum schnellen Untersuchen des Problems benötigen. Security Center umfasst außerdem Empfehlungen zur Reaktion auf einen Angriff. Führen Sie folgende Schritte aus, um Ihre Sicherheitswarnungen zu verwalten:

1. Wählen Sie im ASC-Dashboard die Kachel **Sicherheitswarnungen** aus. Auf dieser Kachel werden Details für die einzelnen Warnungen angezeigt.

2. Wählen Sie zum Filtern von Warnungen basierend auf Datum, Status und Schweregrad die Option **Filter** und dann die gewünschten Werte aus.

3. Um auf eine Warnung zu reagieren, wählen Sie sie aus, und überprüfen Sie die Informationen. Wählen Sie dann die Ressource aus, die angegriffen wurde.

4. Im Feld **Beschreibung** werden Details angezeigt, einschließlich der empfohlenen Korrekturmaßnahme.

![](media/protect-personal-data-azure-security-center/security-alerts.png)

Ausführlichere Informationen zur Reaktion auf Sicherheitswarnungen finden Sie unter [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).

Als weitere Hilfe beim Untersuchen von Sicherheitswarnungen kann das Unternehmen mithilfe der [Azure-Protokollintegration](https://aka.ms/AzLog) ASC-Warnungen mit seiner eigenen SIEM-Lösung integrieren.

#### <a name="how-do-i-manage-security-incidents"></a>Wie verwalte ich Sicherheitsincidents?

In ASC ist ein Sicherheitsincident eine Aggregation aller Warnungen für eine Ressource, die Kill Chain-Mustern entsprechen. Bei einem Vorfall wird die Liste mit den dazugehörigen Warnungen eingeblendet, damit Sie mehr Informationen zu den einzelnen Fällen einsehen können. Incidents werden auf der Kachel und dem Blatt „Sicherheitswarnungen“ angezeigt.

Führen Sie folgende Schritte aus, um Sicherheitsincidents zu überprüfen und zu verwalten:

1. Wählen Sie die Kachel **Sicherheitswarnungen** aus. Wenn ein Sicherheitsincident erkannt wird, wird er im Diagramm für die Sicherheitswarnungen angezeigt. Er ist mit einem Symbol gekennzeichnet, das sich von denen anderer Warnungen unterscheidet.

2. Wählen Sie den Sicherheitsincident aus, um weitere Details dazu anzuzeigen. Die weiteren Details umfassen eine vollständige Beschreibung, den Schweregrad, den aktuellen Status, die angegriffene Ressource, die Schritte zur Behebung für den Incident und die Warnungen, die in diesem Incident enthalten sind.

Sie können Filter festlegen, um **nur Incidents**, **nur Warnungen** oder **beide** anzuzeigen.

#### <a name="how-do-i-access-the-threat-intelligence-report"></a>Wie greife ich auf den Bericht mit Bedrohungsinformationen zu?

ASC analysiert Informationen aus mehreren Quellen, um Bedrohungen zu identifizieren. Um die dafür zuständigen Teams bei der Untersuchung und Beseitigung von Bedrohungen zu unterstützen, steht in Security Center ein Bericht mit Bedrohungsinformationen zur Verfügung.

Security Center verfügt über drei Arten von Bedrohungsberichten, die abhängig vom jeweiligen Angriff variieren können.
Folgende Berichte stehen zur Verfügung:

- Aktivitätsgruppenbericht: Enthält detaillierte Informationen zu Angreifern sowie zu ihren Zielen und Taktiken.

- Kampagnenbericht: Konzentriert sich auf Details zu bestimmten Angriffskampagnen.

- Zusammenfassender Bedrohungsbericht: Enthält alle Elemente der beiden vorherigen Berichte.

Diese Informationen sind hilfreich bei der Reaktion auf Incidents, um im Rahmen einer laufenden Untersuchung die Angriffsquelle, die Beweggründe des Angreifers und eine geeignete Vorgehensweise zur Beseitigung des Problems zu ermitteln.

1. Führen Sie folgende Schritte aus, um auf den Bericht mit Bedrohungsinformationen zuzugreifen:

2. Wählen Sie im ASC-Dashboard die Kachel **Sicherheitswarnungen** aus.

3. Wählen Sie die Sicherheitswarnung aus, für die Sie weitere Informationen erhalten möchten.

4. Klicken Sie im Feld **Berichte** auf den Link zum Bericht mit Bedrohungsinformationen.

5. Dadurch wird die PDF-Datei geöffnet, die Sie herunterladen können.

![](media/protect-personal-data-azure-security-center/security-alerts-suspicious-process.png)

Weitere Informationen zum ASC-Bericht mit Bedrohungsinformationen finden Sie unter [Azure Security Center-Bericht mit Bedrohungsinformationen](https://docs.microsoft.com/azure/security-center/security-center-threat-report).

### <a name="assessment"></a>Bewertung

Zur Unterstützung beim Testen, Bewerten und Auswerten des Sicherheitsstatus bietet ASC eine integrierte Sicherheitsrisikobewertung mit Qualys-Cloud-Agents als Teil der Komponente für Empfehlungen für virtuelle Computer.

Der Qualys-Cloud-Agent meldet Daten zu Sicherheitsrisiken an die Qualys-Verwaltungsplattform, die dann Überwachungsdaten zu Sicherheitsrisiken und zur Integrität an ASC zurücksendet. Die Empfehlung, eine Lösung zur Sicherheitsrisikobewertung hinzuzufügen, wird im ASC-Dashboard auf dem Blatt **Empfehlungen** angezeigt.

Nachdem die Lösung zur Sicherheitsrisikobewertung auf dem virtuellen Zielcomputer installiert wurde, überprüft Security Center den virtuellen Computer, um system- und anwendungsbezogene Sicherheitsrisiken zu ermitteln und zu identifizieren. Ermittelte Probleme werden unter der Option **VM-Empfehlungen** angezeigt.

#### <a name="how-do-i-implement-a-vulnerability-assessment-solution"></a>Wie implementiere ich eine Lösung zur Sicherheitsrisikobewertung? 

Wenn auf einem virtuellen Computer noch keine Lösung zur integrierten Sicherheitsrisikobewertung bereitgestellt wird, wird in Security Center deren Installation empfohlen.

1. Wählen Sie im ASC-Dashboard auf dem Blatt **Empfehlungen** die Option **Lösung zur Sicherheitsrisikobewertung hinzufügen** aus.

2. Wählen Sie die virtuellen Computer aus, auf denen die Lösung zur Sicherheitsrisikobewertung hinzugefügt werden soll.

3. Klicken Sie auf **Auf [Anzahl der] VMs installieren**.

4. Wählen Sie im Azure Marketplace eine Partnerlösung oder unter **Vorhandene Lösung verwenden** die Option **Qualys** aus.

5. Sie können die Einstellungen für automatische Updates auf dem Blatt **Partnerlösungen** aktivieren bzw. deaktivieren.

Weitere Anweisungen zum Implementieren einer Lösung zur Sicherheitsrisikobewertung finden Sie unter [Sicherheitsrisikobewertung in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations).

## <a name="next-steps"></a>Nächste Schritte

- [Schnellstarthandbuch zu Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-get-started)

- [Einführung in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

- [Integrieren von Azure Security Center-Warnungen mithilfe der Azure-Protokollintegration](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration)

- [Boost Azure Security Center with Integrated Vulnerability Assessment](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/16/boost-azure-security-center-with-integrated-vulnerability-assessment/) (Verbessern von Azure Security Center mit integrierter Sicherheitsrisikobewertung)

