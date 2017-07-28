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
ms.date: 06/16/2017
ms.author: terrylan
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 506c23179d09e2e22065c8ba2bc85a341bb0ea09
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017


---
# <a name="azure-security-center-frequently-asked-questions-faq"></a>Azure Security Center – Häufig gestellte Fragen 
Hier werden häufig gestellte Fragen zu Azure Security Center beantwortet. Azure Security Center ist ein Dienst, der Sie aufgrund von größerer Transparenz und besserer Kontrolle der Sicherheit Ihrer Microsoft Azure-Ressourcen dabei unterstützt, Bedrohungen zu verhindern, zu erkennen und darauf zu reagieren.

> [!NOTE]
> Ab Anfang Juni 2017 verwendet Security Center den Microsoft Monitoring Agent zum Sammeln und Speichern von Daten. Weitere Informationen finden Sie unter [Azure Security Center – Plattformmigration](security-center-platform-migration.md). Die Informationen in diesem Artikel stellen Security Center-Funktionen nach dem Umstieg auf den Microsoft Monitoring Agent vor.
>
>

## <a name="general-questions"></a>Allgemeine Fragen
### <a name="what-is-azure-security-center"></a>Was ist Azure Security Center?
Azure Security Center unterstützt Sie beim Verhindern, Erkennen und Beheben von Bedrohungen durch größere Transparenz und bessere Kontrolle über die Sicherheit Ihrer Azure-Ressourcen. Es bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Sektrum an Sicherheitslösungen verwendet werden.

### <a name="how-do-i-get-azure-security-center"></a>Wie erhalte ich Azure Security Center?
Azure Security Center wird mit Ihrem Microsoft Azure-Abonnement aktiviert und über das [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)aufgerufen. ([Melden Sie sich beim Portal an](https://portal.azure.com), wählen Sie **Durchsuchen** aus, und führen Sie einen Bildlauf zu **Security Center** aus.)  

## <a name="billing"></a>Abrechnung
### <a name="how-does-billing-work-for-azure-security-center"></a>Wie funktioniert die Abrechnung für Azure Security Center?
Security Center wird in zwei Tarifen angeboten:

Der **Free-Tarif** bietet einen Einblick in den Sicherheitszustand Ihrer Azure-Ressourcen, grundlegende Sicherheitsrichtlinien, Empfehlungen zur Sicherheit und die Integration in Sicherheitsprodukte und -Dienste von Partnern.

Mit dem **Standard-Tarif** werden erweiterte Funktionen zur Bedrohungserkennung hinzugefügt, darunter Informationen zu Bedrohungen, Verhaltensanalysen, Erkennung von Anomalien, Sicherheitsincidents und Berichte zur Zuordnung von Bedrohungen. Der Standard-Tarif ist für die ersten 60 Tage kostenlos. Wenn Sie den Dienst über diese 60 Tage hinaus verwenden, beginnen wir automatisch mit der Berechnung des Diensts.  Wenn Sie ein Upgrade durchführen möchten, wählen Sie in der [Sicherheitsrichtlinie](security-center-policies.md#set-security-policies)die Tarifoption aus. Weitere Informationen finden Sie unter [Security Center-Preise](security-center-pricing.md).

## <a name="permissions"></a>Berechtigungen
Azure Security Center verwendet die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](../active-directory/role-based-access-control-configure.md). Diese stellt [integrierte Rollen](../active-directory/role-based-access-built-in-roles.md) bereit, die Benutzern, Gruppen und Diensten in Azure zugewiesen werden können.

Security Center bewertet die Konfiguration Ihrer Ressourcen, um die Sicherheitsprobleme und Sicherheitsrisiken zu identifizieren. In Security Center werden Ihnen nur dann Informationen zu einer Ressource angezeigt, wenn Ihnen für das Abonnement oder die Ressourcengruppe, der eine Ressource angehört, die Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ zugewiesen ist.

Informationen zu Rollen und zulässigen Aktionen in Security Center finden Sie unter [Permissions in Azure Security Center](security-center-permissions.md) (Berechtigungen in Azure Security Center).

## <a name="data-collection"></a>Datensammlung
Security Center sammelt Daten von Ihren virtuellen Computern, um den Sicherheitsstatus zu bewerten, Sicherheitsempfehlungen bereitzustellen und vor Bedrohungen zu warnen. Beim ersten Zugriff auf Security Center wird die Datensammlung für alle virtuellen Computer in Ihrem Abonnement aktiviert. Sie können die Datensammlung auch in der Security Center-Richtlinie aktivieren.

### <a name="how-do-i-disable-data-collection"></a>Wie deaktiviere ich Datensammlung?
Wenn Sie den Free-Tarif für Azure Security Center verwenden, können Sie die Datensammlung von virtuellen Computern jederzeit deaktivieren. Die Datensammlung ist für Abonnements des Standard-Tarifs erforderlich. Die Datensammlung kann für ein Abonnement in der Sicherheitsrichtlinie deaktiviert werden. ([Melden Sie sich beim Azure-Portal an](https://portal.azure.com), und wählen Sie nacheinander **Durchsuchen**, **Security Center** und **Richtlinie** aus.)  Wenn Sie ein Abonnement auswählen, wird ein neues Blatt geöffnet, auf dem Sie die Option **Datensammlung** deaktivieren können.

### <a name="how-do-i-enable-data-collection"></a>Wie aktiviere ich die Datensammlung?
Sie können die Datensammlung für Ihr Azure-Abonnement in der Sicherheitsrichtlinie aktivieren. Zum Aktivieren der Datensammlung [melden Sie sich beim Azure-Portal an](https://portal.azure.com), und wählen Sie nacheinander **Durchsuchen**, **Security Center** und **Richtlinie** aus. Legen Sie **Datensammlung** auf **Ein** fest.

### <a name="what-happens-when-data-collection-is-enabled"></a>Was passiert, wenn die Datensammlung aktiviert wird?
Wenn die Datensammlung aktiviert ist, wird der Microsoft Monitoring Agent automatisch auf allen bestehenden und neu unterstützten virtuellen Computern bereitgestellt, die im Abonnement bereitgestellt werden.

### <a name="does-the-monitoring-agent-impact-the-performance-of-my-servers"></a>Wirkt sich der Monitoring Agent auf die Leistung der Server aus?
Der Agent beansprucht eine äußerst geringe Menge von Systemressourcen und sollten nur eine geringe Auswirkung auf die Leistung haben. Weitere Informationen zu Auswirkungen auf die Leistung, zum Agent und zur Erweiterung finden Sie unter [Planungs- und Betriebshandbuch](security-center-planning-and-operations-guide.md#data-collection-and-storage).

### <a name="where-is-my-data-stored"></a>Wo werden meine Daten gespeichert?
Die von diesem Agent gesammelten Daten werden entweder in einem vorhandenen Log Analytics-Arbeitsbereich, der mit Ihrem Abonnement verknüpft ist, oder in einem neuen Arbeitsbereich gespeichert. Weitere Informationen finden Sie unter [Datensicherheit](security-center-data-security.md).

## <a name="using-azure-security-center"></a>Verwenden von Azure Security Center
### <a name="what-is-a-security-policy"></a>Was ist eine Sicherheitsrichtlinie?
In einer Sicherheitsrichtlinie werden die Sicherheitsmechanismen definiert, die für Ressourcen in dem angegebenen Abonnement zu empfehlen sind. In Azure Security Center definieren Sie Richtlinien für Ihre Azure-Abonnements entsprechend den Sicherheitserfordernissen Ihres Unternehmens sowie dem Typ der Anwendungen oder der Vertraulichkeit der Daten in jedem Abonnement.

Die Sicherheitsempfehlungen und -überwachung werden entsprechend den in Azure Security Center aktivierten Sicherheitsrichtlinien umgesetzt. Weitere Informationen zu Sicherheitsrichtlinien finden Sie unter [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md).

### <a name="who-can-modify-a-security-policy"></a>Wie kann ich eine Sicherheitsrichtlinie ändern?
Damit Sie eine Sicherheitsrichtlinie ändern können, müssen Sie Systemadministrator oder Besitzer bzw. Mitwirkender dieses Abonnements sein.

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
Auf dem Blatt **Security Center Overview** (Security Center – Übersicht) wird der Gesamtsicherheitsstatus Ihrer Umgebung aufgegliedert nach Compute, Netzwerk, Speicher und Daten und Anwendungen angezeigt. Jeder Ressourcentyp hat einen Indikator, der kennzeichnet, ob irgendwelche möglichen Sicherheitsrisiken festgestellt wurden. Wenn Sie auf die einzelnen Kacheln klicken, wird eine Liste mit von Security Center erkannten Sicherheitsproblemen sowie ein Bestand an Ressourcen in Ihrem Abonnement angezeigt.

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
* Azure-Speicherkonto
* Azure-Web-Apps (in [App Service-Umgebung](../app-service/app-service-app-service-environments-readme.md))
* Partnerlösungen, die in Ihr Azure-Abonnement integriert sind, z.B. die Firewall einer Webanwendung auf virtuellen Computern und in einer [App Service-Umgebung](../app-service/app-service-app-service-environments-readme.md)

## <a name="virtual-machines"></a>Virtual Machines
### <a name="what-types-of-virtual-machines-are-supported"></a>Welche Typen von virtuellen Maschinen werden unterstützt?
Die Überwachung und entsprechende Empfehlungen sind für virtuelle Computer (VMs) verfügbar, die mit dem [klassischen Bereitstellungsmodell oder mit dem Resource Manager-Bereitstellungsmodell](../azure-classic-rm.md)erstellt wurden.

Eine Liste mit unterstützten Plattformen finden Sie unter [Supported platforms in Azure Security Center (Unterstützte Plattformen in Azure Security Center)](security-center-os-coverage.md).

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Warum erkennt Azure Security Center die Antischadsoftware nicht, die auf meiner Azure-VM ausgeführt wird?
Azure Security Center erkennt Antischadsoftware, die über Azure-Erweiterungen installiert wurde. Security Center kann z.B. Antischadsoftware nicht erkennen, die auf einem Image vorinstalliert war, das Sie bereitgestellt haben, oder Antischadsoftware, die Sie auf Ihren virtuellen Computern über Ihre eigenen Prozesse installiert haben (z.B. Systeme für die Konfigurationsverwaltung).

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Warum erhalte ich die Meldung „Fehlende Überprüfungsdaten“ für meinen virtuellen Computer?
Diese Meldung wird angezeigt, wenn keine Überprüfungsdaten für einen virtuellen Computer vorhanden sind. Es kann einige Zeit dauern (weniger als eine Stunde), um die Überprüfungsdaten aufzufüllen, nachdem die Datensammlung in Azure Security Center aktiviert wurde. Nach dem ersten Auffüllen der Überprüfungsdaten erhalten Sie diese Meldung möglicherweise, weil überhaupt keine oder keine aktuellen Überprüfungsdaten vorliegen. Überprüfungen werden für beendete virtuelle Computer nicht aufgefüllt. Diese Meldung kann auch angezeigt werden, wenn die Überprüfungsdaten (gemäß der Aufbewahrungsrichtlinie für den Windows-Agenten, die einen Standardwert von 30 Tagen vorgibt) nicht vor kurzem aufgefüllt wurden.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Warum erhalte ich die Meldung „VM-Agent fehlt“?
Der VM-Agent muss auf virtuellen Computern installiert sein, um die Datensammlung zu ermöglichen. Der VM-Agent wird standardmäßig für virtuelle Maschinen installiert, die vom Azure Marketplace bereitgestellt werden. Informationen zum Installieren des VM-Agents auf anderen virtuellen Computern finden Sie im Blogbeitrag [VM Agent and Extensions](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/)(VM-Agent und Erweiterungen).

