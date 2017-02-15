---
title: "Azure Security Center – Häufig gestellte Fragen (FAQ) | Microsoft Docs"
description: Dieses FAQ-Dokument beantwortet Fragen zu Azure Security Center.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 0135732e95279f2e717334d3dd39902b56b0aa90
ms.openlocfilehash: 30a327f59b8149f41c3b5206e0b0c2fc859934a0


---
# <a name="azure-security-center-frequently-asked-questions-faq"></a>Azure Security Center – Häufig gestellte Fragen 
Hier werden häufig gestellte Fragen zu Azure Security Center beantwortet. Azure Security Center ist ein Dienst, der Sie aufgrund von größerer Transparenz und besserer Kontrolle der Sicherheit Ihrer Microsoft Azure-Ressourcen dabei unterstützt, Bedrohungen zu verhindern, zu erkennen und darauf zu reagieren.

## <a name="general-questions"></a>Allgemeine Fragen
### <a name="what-is-azure-security-center"></a>Was ist Azure Security Center?
Azure Security Center unterstützt Sie beim Verhindern, Erkennen und Beheben von Bedrohungen durch größere Transparenz und bessere Kontrolle über die Sicherheit Ihrer Azure-Ressourcen. Es bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Sektrum an Sicherheitslösungen verwendet werden.

### <a name="how-do-i-get-azure-security-center"></a>Wie erhalte ich Azure Security Center?
Azure Security Center wird mit Ihrem Microsoft Azure-Abonnement aktiviert und über das [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)aufgerufen. ([Melden Sie sich beim Portal an](https://portal.azure.com), wählen Sie **Durchsuchen** aus, und führen Sie einen Bildlauf zu **Security Center** aus.)  

## <a name="billing"></a>Abrechnung
### <a name="how-does-billing-work-for-azure-security-center"></a>Wie funktioniert die Abrechnung für Azure Security Center?
Security Center wird in zwei Tarifen angeboten: Free und Standard.

Mit dem Free-Tarif können Sie Sicherheitsrichtlinien festlegen und Sicherheitshinweise, Incidents und Empfehlungen empfangen, die Ihnen beim Konfigurieren der erforderlichen Steuerelemente helfen. Mit dem Free-Tarif können Sie zudem den Sicherheitsstatus Ihrer Azure-Ressourcen und der in Ihrem Azure-Abonnement integrierten Partnerlösungen überwachen.

Der Standard-Tarif bietet die Funktionen des Free-Tarifs und zusätzlich erweiterte Erkennungsfunktionen: Informationen zu Bedrohungen, Verhaltensanalysen, Absturzanalysen und Anomalieerkennung. Eine kostenlose 90-Tage-Testversion des Standard-Tarifs ist verfügbar. Wenn Sie ein Upgrade durchführen möchten, wählen Sie in der [Sicherheitsrichtlinie](security-center-policies.md#set-security-policies-for-subscriptions)die Tarifoption aus. Weitere Informationen finden Sie unter [Security Center-Preise](security-center-pricing.md).

## <a name="permissions"></a>Berechtigungen
Azure Security Center verwendet die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](../active-directory/role-based-access-control-configure.md). Diese stellt [integrierte Rollen](../active-directory/role-based-access-built-in-roles.md) bereit, die Benutzern, Gruppen und Diensten in Azure zugewiesen werden können.

Security Center bewertet die Konfiguration Ihrer Ressourcen, um die Sicherheitsprobleme und Sicherheitsrisiken zu identifizieren. In Security Center werden Ihnen nur dann Informationen zu einer Ressource angezeigt, wenn Ihnen für das Abonnement oder die Ressourcengruppe, der eine Ressource angehört, die Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ zugewiesen ist.

Informationen zu Rollen und zulässigen Aktionen in Security Center finden Sie unter [Permissions in Azure Security Center](security-center-permissions.md) (Berechtigungen in Azure Security Center).

## <a name="data-collection"></a>Datensammlung
Security Center sammelt Daten von Ihren virtuellen Computern, um den Sicherheitsstatus zu bewerten, Sicherheitsempfehlungen bereitzustellen und vor Bedrohungen zu warnen. Beim ersten Zugriff auf Security Center wird die Datensammlung für alle virtuellen Computer in Ihrem Abonnement aktiviert. Die Datensammlung wird zwar empfohlen, kann aber in der Security Center-Richtlinie [deaktiviert](#how-do-i-disable-data-collection) werden.

### <a name="how-do-i-disable-data-collection"></a>Wie deaktiviere ich Datensammlung?
Die **Datensammlung** kann für ein Abonnement jederzeit in der Sicherheitsrichtlinie deaktiviert werden. ([Melden Sie sich beim Azure-Portal an](https://portal.azure.com), und wählen Sie nacheinander **Durchsuchen**, **Security Center** und **Richtlinie** aus.)  Wenn Sie ein Abonnement auswählen, wird ein neues Blatt geöffnet, auf dem Sie die Option **Datensammlung** deaktivieren können. Wählen Sie im oberen Menüband die Option **Agents löschen**, um die Agents von vorhandenen virtuellen Computern zu entfernen.

> [!NOTE]
> Sicherheitsrichtlinien können auf der Ebene des Azure-Abonnements und der Ressourcengruppe festgelegt werden. Zum Deaktivieren der Datensammlung muss allerdings ein Abonnement ausgewählt werden.
>
>

### <a name="how-do-i-enable-data-collection"></a>Wie aktiviere ich die Datensammlung?
Sie können die Datensammlung für Ihr Azure-Abonnement in der Sicherheitsrichtlinie aktivieren. Um die Datensammlung zu aktivieren, [melden Sie sich beim Azure-Portal an](https://portal.azure.com), und wählen Sie nacheinander **Durchsuchen**, **Security Center** und **Richtlinie** aus. Legen Sie **Datensammlung** auf **Ein** fest, und konfigurieren Sie die Speicherkonten, in denen Sie die Daten gesammelt werden sollen (siehe die Frage [Wo werden meine Daten gespeichert?](#where-is-my-data-stored)). Wenn **Datensammlung** aktiviert ist, werden automatisch Sicherheitskonfigurations- und Ereignisinformationen von allen unterstützten virtuellen Computern im Abonnement gesammelt.

> [!NOTE]
> Sicherheitsrichtlinien können auf der Ebene des Azure-Abonnements und der Ressourcengruppe festgelegt werden, die Konfiguration der Datensammlung erfolgt jedoch nur auf Abonnementebene.
>
>

### <a name="what-happens-when-data-collection-is-enabled"></a>Was passiert, wenn die Datensammlung aktiviert wird?
Datensammlung wird über den Azure-Überwachungs-Agent und die Azure-Erweiterung für Sicherheitsüberwachung aktiviert. Die Azure-Erweiterung für die Sicherheitsüberwachung sucht nach verschiedenen sicherheitsrelevanten Konfigurationen und sendet diese in [Ereignisablaufverfolgungen für Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW, Event Tracing for Windows). Außerdem erstellt das Betriebssystem Einträge im Ereignisprotokoll.  Der Azure-Überwachungs-Agent liest Ereignisprotokolleinträge und ETW-Ablaufverfolgungen und kopiert diese zur Analyse in Ihr Speicherkonto.  Dies ist das Speicherkonto, das Sie in der Sicherheitsrichtlinie konfiguriert haben. Weitere Informationen über das Speicherkonto finden Sie in der Farge[Wo werden meine Daten gespeichert?](#where-is-my-data-stored)

### <a name="does-the-monitoring-agent-or-security-monitoring-extension-impact-the-performance-of-my-servers"></a>Wirkt sich der Überwachungs-Agent oder die Erweiterung für Sicherheitsüberwachung auf die Leistung meiner Server aus?
Der Agent und die Erweiterung beanspruchen eine äußerst geringe Menge von Systemressourcen und sollten nur eine geringe Auswirkung auf die Leistung haben. Weitere Informationen zu Auswirkungen auf die Leistung, zum Agent und zur Erweiterung finden Sie unter [Planungs- und Betriebshandbuch](security-center-planning-and-operations-guide.md#data-collection-and-storage).

### <a name="where-is-my-data-stored"></a>Wo werden meine Daten gespeichert?
Wählen Sie für jede Region, in der Sie virtuelle Computer ausführen, ein Speicherkonto, in dem Daten dieser virtuellen Computer gespeichert werden. Dies macht es einfach für Sie, Daten aus Datenschutz- und Datenhoheitszwecken im selben geografischen Gebiet zu speichern. Das Speicherkonto für ein Abonnement wählen Sie in der Sicherheitsrichtlinie aus. ([Melden Sie sich beim Azure-Portal an](https://portal.azure.com), und wählen Sie nacheinander **Durchsuchen**, **Security Center** und **Richtlinie** aus.) Wenn Sie ein Abonnement auswählen, wird ein neues Blatt geöffnet. Klicken Sie auf **Speicherkonten wählen**, um eine Region auszuwählen.

> [!NOTE]
> Sicherheitsrichtlinien können auf der Ebene des Azure-Abonnements und der Ressourcengruppe festgelegt werden. Die Auswahl einer Region für Ihr Speicherkonto erfolgt jedoch nur auf Abonnementebene.
>
>

Weitere Informationen zu Azure-Speicher und zu Speicherkonten finden Sie unter [Speicherdokumentation](https://azure.microsoft.com/documentation/services/storage/) und [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md).

## <a name="using-azure-security-center"></a>Verwenden von Azure Security Center
### <a name="what-is-a-security-policy"></a>Was ist eine Sicherheitsrichtlinie?
In einer Sicherheitsrichtlinie werden die Sicherheitsmechanismen definiert, die für Ressourcen im angegebenen Abonnement oder in der angegebenen Ressourcengruppe zu empfehlen sind. In Azure Security Center definieren Sie Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen entsprechend den Sicherheitserfordernissen Ihres Unternehmens sowie dem Typ der Anwendungen oder der Vertraulichkeit der Daten in jedem Abonnement.

Beispielsweise haben Ressourcen, die für Entwicklungs- oder Testzwecke verwendet werden, möglicherweise Sicherheitsanforderungen, die sich von denen unterscheiden, die für Produktionsanwendungen verwendet werden. Ähnlich kann für Anwendungen mit reglementierten Daten, etwa personenbezogene Informationen (Personally Identifiable Information), eine höhere Sicherheitsstufe erforderlich sein. Die Sicherheitsempfehlungen und -überwachung werden entsprechend den in Azure Security Center aktivierten Sicherheitsrichtlinien umgesetzt. Weitere Informationen zu Sicherheitsrichtlinien finden Sie unter [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md).

> [!NOTE]
> Bei einem Konflikt zwischen der Richtlinie auf Abonnementebene und der Richtlinie auf Ressourcengruppenebene hat die Richtlinie auf Ressourcengruppenebene Vorrang.
>
>

### <a name="who-can-modify-a-security-policy"></a>Wie kann ich eine Sicherheitsrichtlinie ändern?
Sicherheitsrichtlinien können für jedes Abonnement oder jede Ressourcengruppe konfiguriert werden. Um eine Sicherheitsrichtlinie auf Abonnementebene oder Ressourcengruppenebene zu ändern, müssen Sie der Besitzer oder ein Mitwirkender des Abonnements sein.

Weitere Informationen dazu, wie eine Sicherheitsrichtlinie konfiguriert wird, finden Sie unter [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Was ist eine Sicherheitsempfehlung?
Azure Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Werden mögliche Sicherheitsrisiken festgestellt, werden Empfehlungen erstellt. Entsprechend den Empfehlungen werden Sie durch den Prozess des Konfigurierens des erforderlichen Sicherheitsmechanismus geführt. Beispiele sind:

* Bereitstellung von Antischadsoftware, um bösartige Software zu erkennen und zu entfernen
* Konfigurieren von [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md) und Regeln, um Datenverkehr zu virtuellen Computern zu steuern
* Bereitstellung einer Web Application Firewall als Schutz vor Angriffen, die auf Ihre Webanwendungen abzielen
* Bereitstellen von fehlenden Systemupdates
* Behandeln von Betriebssystemkonfigurationen, die nicht den empfohlenen Basisregeln entsprechen

Hier sind nur Empfehlungen aufgeführt, die in den Sicherheitsrichtlinien aktiviert sind.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Wie kann ich den aktuellen Sicherheitsstatus meiner Azure-Ressourcen anzeigen?
Auf dem Blatt **Security Center** wird auf der Kachel **Ressourcenintegrität** der Gesamtsicherheitsstatus Ihrer Umgebung aufgegliedert nach virtuellen Computern, Webanwendungen und weiteren Ressourcen angezeigt. Jede Ressource hat einen Indikator, der kennzeichnet, ob irgendwelche möglichen Sicherheitsrisiken festgestellt wurden. Wenn Sie auf die Kachel „Ressourcenintegrität“ klicken, werden Ihre Ressourcen angezeigt, und es wird gekennzeichnet, wo Aufmerksamkeit erforderlich ist oder Probleme vorhanden sein können.

### <a name="what-triggers-a-security-alert"></a>Wodurch wird eine Sicherheitswarnung ausgelöst?
Azure Security Center erfasst, analysiert und kombiniert automatisch Protokolldaten von Ihren Azure-Ressourcen, vom Netzwerk und von Partnerlösungen wie Antischadsoftware und Firewalls. Wurden Bedrohungen erkannt, wird eine Sicherheitswarnung erstellt. Beispiele für eine Erkennung sind:

* Gefährdete virtuelle Computer, die mit bekannten bösartigen IP-Adressen kommunizieren
* Erweiterte Schadsoftware, die mithilfe der Windows-Fehlerberichterstattung erkannt wurde
* Brute-Force-Angriffe gegen virtuelle Computer
* Sicherheitswarnungen von integrierten Partnersicherheitslösungen wie Antischadsoftware oder Web Application Firewalls

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Wo liegt bei der Bedrohungserkennung und Ausgabe von Warnungen der Unterschied zwischen Microsoft Security Response Center und Azure Security Center?
Das Microsoft Security Response Center (MSRC) führt eine selektive Sicherheitsüberwachung im Azure-Netzwerk und in der Infrastruktur durch und empfängt Threat Intelligence-Daten und Missbrauchsmeldungen von Drittanbietern. Wenn MSRC erkennt, dass eine Partei unrechtmäßig oder unbefugt auf Kundendaten zugreift oder dass der Kunde Azure nicht entsprechend den Bestimmungen für den Umgang mit vertraulichen Informationen (Acceptable Use) nutzt, wird der Kunde durch einen Manager für Sicherheitsincidents benachrichtigt. Die Benachrichtigung erfolgt in der Regel durch das Senden einer E-Mail an die im Azure Security Center angegebenen Sicherheitskontakte oder an den Besitzer des Azure-Abonnements, falls kein Sicherheitskontakt angegeben wurde.

Das Security Center ist ein Azure-Dienst, der die Azure-Umgebung des Kunden ständig überwacht und Analysefunktionen anwendet, um eine Vielzahl von potenziell schädlichen Aktivitäten automatisch zu erkennen. Diese Erkennungen werden als Sicherheitshinweise im Security Center-Dashboard angegeben.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Welche Azure-Ressourcen werden von Azure Security Center überwacht?
Azure Security Center überwacht die folgenden Azure-Ressourcen:

* Virtuelle Computer (Virtual Machines, VMs) (auch [Clouddienste](../cloud-services/cloud-services-choose-me.md))
* Virtuelle Azure-Netzwerke
* Azure SQL-Dienst
* Partnerlösungen, die in Ihr Azure-Abonnement integriert sind, z.B. die Firewall einer Webanwendung auf virtuellen Computern und in einer [App Service-Umgebung](../app-service/app-service-app-service-environments-readme.md)

## <a name="virtual-machines"></a>Virtual Machines
### <a name="what-types-of-virtual-machines-are-supported"></a>Welche Typen von virtuellen Maschinen werden unterstützt?
Die Überwachung der Sicherheitsintegrität und entsprechende Empfehlungen sind für virtuelle Computer (VMs) verfügbar, die mit dem [klassischen Bereitstellungsmodell oder mit dem Resource Manager-Bereitstellungsmodell](../azure-classic-rm.md)erstellt wurden.

Unterstützte Windows-VMs:

* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2

Unterstützte Linux-VMs:

* Ubuntu-Versionen 12.04, 14.04, 16.04, 16.10
* Debian-Versionen 7, 8
* CentOS-Versionen 6.\*, 7.*
* Red Hat Enterprise Linux-Versionen (RHEL) 6.\*, 7.*
* SUSE Linux Enterprise Server-Versionen (SLES) 11.\*, 12.*
* Oracle Linux-Versionen 6.\*, 7.*

VMs, die in einem Clouddienst ausgeführt werden, werden ebenfalls unterstützt. Es werden nur Clouddienst-Webrollen und -Workerrollen überwacht, die in Produktionsslots ausgeführt werden. Weitere Informationen zum Clouddienst finden Sie unter [Sollte ich mich für Clouddienste oder für eine andere Lösung entscheiden?](../cloud-services/cloud-services-choose-me.md).

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Warum erkennt Azure Security Center die Antischadsoftware nicht, die auf meiner Azure-VM ausgeführt wird?
Azure Security Center erkennt nur Antischadsoftware, die über Azure-Erweiterungen installiert wurde. Security Center kann z.B. Antischadsoftware nicht erkennen, die auf einem Image vorinstalliert war, das Sie bereitgestellt haben, oder Antischadsoftware, die Sie auf Ihren virtuellen Computern über Ihre eigenen Prozesse installiert haben (z.B. Systeme für die Konfigurationsverwaltung).

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Warum erhalte ich die Meldung „Fehlende Überprüfungsdaten“ für meinen virtuellen Computer?
Es kann einige Zeit dauern (weniger als eine Stunde), um die Überprüfungsdaten aufzufüllen, nachdem die Datensammlung in Azure Security Center aktiviert wurde. Überprüfungen werden für virtuelle Computer, die beendet wurden, nicht aufgefüllt.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Warum erhalte ich die Meldung „VM-Agent fehlt“?
Der VM-Agent muss auf virtuellen Computern installiert sein, um die Datensammlung zu ermöglichen. Der VM-Agent wird standardmäßig für virtuelle Maschinen installiert, die vom Azure Marketplace bereitgestellt werden. Informationen zum Installieren des VM-Agents auf anderen virtuellen Computern finden Sie im Blogbeitrag [VM Agent and Extensions](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/)(VM-Agent und Erweiterungen).



<!--HONumber=Dec16_HO2-->


