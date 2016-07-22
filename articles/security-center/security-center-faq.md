<properties
   pageTitle="Häufig gestellte Fragen (FAQ) zu Azure Security Center | Microsoft Azure"
   description="Dieses FAQ-Dokument beantwortet Fragen zu Azure Security Center."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/05/2016"
   ms.author="terrylan"/>

# Azure Security Center – Häufig gestellte Fragen

Hier werden häufig gestellte Fragen zu Azure Security Center beantwortet. Azure Security Center ist ein Dienst, der Sie durch größere Transparenz und bessere Kontrolle über die Sicherheit Ihrer Azure-Ressourcen dabei unterstützt, Bedrohungen zu verhindern, zu erkennen und darauf zu reagieren.

> [AZURE.NOTE] Die Informationen in diesem Dokument gelten für die Vorschauversion von Azure Security Center.

## Allgemeine Fragen

### Was ist Azure Security Center?
Azure Security Center unterstützt Sie beim Verhindern, Erkennen und Beheben von Bedrohungen durch größere Transparenz und bessere Kontrolle über die Sicherheit Ihrer Azure-Ressourcen. Es bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Sektrum an Sicherheitslösungen verwendet werden.

### Wie erhalte ich Azure Security Center?
Azure Security Center wird mit Ihrem Microsoft Azure-Abonnement aktiviert und über das [Azure-Portal](https://azure.microsoft.com/features/azure-portal/) aufgerufen. ([Melden Sie sich beim Portal an](https://portal.azure.com), wählen Sie **Durchsuchen** aus, und führen Sie einen Bildlauf zum **Security Center** aus.)

## Abrechnung

### Wie funktioniert die Abrechnung für Azure Security Center?
Informationen hierzu finden Sie unter [Security Center – Preise](https://azure.microsoft.com/pricing/details/security-center/).

## Datensammlung

Security Center sammelt Daten von Ihren virtuellen Computern, um den Sicherheitsstatus zu bewerten, Sicherheitsempfehlungen bereitzustellen und vor Bedrohungen zu warnen. Beim ersten Zugriff auf Security Center wird die Datensammlung für alle virtuellen Computer in Ihrem Abonnement aktiviert. Die Datensammlung wird zwar empfohlen, kann aber in der Security Center-Richtlinie [deaktiviert](#how-do-i-disable-data-collection) werden.

### Wie deaktiviere ich Datensammlung?

Die Option **Datensammlung** kann für ein Abonnement jederzeit in der Sicherheitsrichtlinie deaktiviert werden. ([Melden Sie sich beim Azure-Portal an](https://portal.azure.com), und wählen Sie **Durchsuchen** > **Security Center** > **Sicherheitsrichtlinie** aus.) Wenn Sie ein Abonnement auswählen, wird ein neues Blatt geöffnet, auf dem Sie die Option **Datensammlung** deaktivieren können. Wählen Sie im oberen Menüband die Option **Agents löschen** aus, um die Agents von vorhandenen virtuellen Computern zu entfernen.

> [AZURE.NOTE] Sicherheitsrichtlinien können auf der Ebene des Azure-Abonnements und der Ressourcengruppe festgelegt werden. Zum Deaktivieren der Datensammlung muss allerdings ein Abonnement ausgewählt werden.

### Wie aktiviere ich die Datensammlung?
Sie können Datensammlung für Ihre Azure-Abonnements in der Sicherheitsrichtlinie aktivieren. Um die Datensammlung zu aktivieren, [melden Sie sich beim Azure-Portal an](https://portal.azure.com), und wählen Sie **Durchsuchen** > **Security Center** > **Richtlinie** aus. Legen Sie **Datensammlung** auf **Ein** fest, und konfigurieren Sie die Speicherkonten, in denen Sie die gesammelten Daten anzeigen möchten (siehe die Frage [Wo werden meine Daten gespeichert?](#where-is-my-data-stored)). Wenn **Datensammlung** aktiviert ist, werden automatisch Sicherheitskonfigurations- und Ereignisinformationen von allen virtuellen Computern gesammelt, die im Abonnement unterstützt werden.

> [AZURE.NOTE] Sicherheitsrichtlinien können auf der Ebene des Azure-Abonnements und der Ressourcengruppe festgelegt werden, die Konfiguration von Datensammlung erfolgt jedoch nur auf Abonnementebene.

### Was passiert, wenn die Datensammlung aktiviert wird?
Datensammlung wird über den Azure-Überwachungs-Agent und die Azure-Erweiterung für Sicherheitsüberwachung aktiviert. Die Azure-Erweiterung für die Sicherheitsüberwachung sucht nach verschiedenen sicherheitsrelevanten Konfigurationen und sendet diese in [ETW-Ablaufverfolgungen](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (Event Tracing for Windows, Ereignisablaufverfolgung für Windows). Außerdem erstellt das Betriebssystem Einträge im Ereignisprotokoll. Der Azure-Überwachungs-Agent liest Ereignisprotokolleinträge und ETW-Ablaufverfolgungen und kopiert diese zur Analyse in Ihr Speicherkonto. Dies ist das Speicherkonto, das Sie in der Sicherheitsrichtlinie konfiguriert haben. Weitere Informationen über das Speicherkonto finden Sie in der Farge [Wo werden meine Daten gespeichert?](#where-is-my-data-stored)

### Wirkt sich der Überwachungs-Agent oder die Erweiterung für Sicherheitsüberwachung auf die Leistung meiner Server aus?
Der Agent und die Erweiterung beanspruchen eine äußerst geringe Menge von Systemressourcen und sollten nur eine geringe Auswirkung auf die Leistung haben.

### Wo werden meine Daten gespeichert?
Wählen Sie für jede Region, in der Sie virtuelle Computer ausführen, ein Speicherkonto, in dem Daten dieser virtuellen Computer gespeichert werden. Dies macht es einfach für Sie, Daten aus Datenschutz- und Datenhoheitszwecken im selben geografischen Gebiet zu speichern. Das Speicherkonto für ein Abonnement wählen Sie in der Sicherheitsrichtlinie aus. ([Melden Sie sich beim Azure-Portal an](https://portal.azure.com), und wählen Sie **Durchsuchen** > **Security Center** > **Sicherheitsrichtlinie** aus.) Wenn Sie auf ein Abonnement klicken, wird ein neues Blatt geöffnet. Klicken Sie auf **Speicherkonten wählen**, um eine Region auszuwählen.

> [AZURE.NOTE] Sicherheitsrichtlinien können auf der Ebene von Azure-Abonnement und Ressourcengruppe festgelegt werden. Die Auswahl einer Region für Ihr Speicherkonten erfolgt jedoch nur auf Abonnementebene.

Weitere Informationen zu Azure-Speicher und zu Speicherkonten finden Sie unter [Speicherdokumentation](https://azure.microsoft.com/documentation/services/storage/) und [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md).

## Verwenden von Azure Security Center

### Was ist eine Sicherheitsrichtlinie?
In einer Sicherheitsrichtlinie wird der Satz von Sicherheitsmechanismen definiert, die für Ressourcen in dem angegebenen Abonnement oder der angegebenen Ressourcengruppe zu empfehlen sind. In Azure Security Center definieren Sie Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen entsprechend den Sicherheitserfordernissen Ihres Unternehmens sowie dem Typ der Anwendungen oder der Vertraulichkeit der Daten in jedem Abonnement.

Beispielsweise haben Ressourcen, die für Entwicklungs- oder Testzwecke verwendet werden, möglicherweise Sicherheitsanforderungen, die sich von denen unterscheiden, die für Produktionsanwendungen verwendet werden. Ähnlich kann für Anwendungen mit reglementierten Daten, etwa personenbezogene Informationen (Personally Identifiable Information), eine höhere Sicherheitsstufe erforderlich sein. Die Sicherheitsempfehlungen und -überwachung werden entsprechend den in Azure Security Center aktivierten Sicherheitsrichtlinien umgesetzt. Weitere Informationen zu Sicherheitsrichtlinien finden Sie unter [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md).

> [AZURE.NOTE] Bei einem Konflikt zwischen der Richtlinie auf Abonnementebene und der Richtlinie auf Ressourcengruppenebene hat die Richtlinie auf Ressourcengruppenebene Vorrang.

### Wie kann ich eine Sicherheitsrichtlinie ändern?
Sicherheitsrichtlinien können für jedes Abonnement oder jede Ressourcengruppe konfiguriert werden. Um eine Sicherheitsrichtlinie auf Abonnementebene oder Ressourcengruppenebene zu ändern, müssen Sie der Besitzer des Abonnements oder ein Mitwirkender sein.

Weitere Informationen dazu, wie eine Sicherheitsrichtlinie konfiguriert wird, finden Sie unter [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md).

### Was ist eine Sicherheitsempfehlung?
Azure Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Werden mögliche Sicherheitsrisiken festgestellt, werden Empfehlungen erstellt. Entsprechend den Empfehlungen werden Sie durch den Prozess des Konfigurierens des erforderlichen Sicherheitsmechanismus geführt. Beispiele sind:

- Bereitstellung von Antischadsoftware, um bösartige Software zu erkennen und zu entfernen
- Konfigurieren von [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md) und Regeln, um Datenverkehr zu virtuellen Computern zu steuern
- Bereitstellung einer Web Application Firewall als Schutz vor Angriffen, die auf Ihre Webanwendungen abzielen
- Bereitstellen von fehlenden Systemupdates
- Behandeln von Betriebssystemkonfigurationen, die nicht den empfohlenen Basisregeln entsprechen

Hier sind nur Empfehlungen aufgeführt, die in den Sicherheitsrichtlinien aktiviert sind.

### Wie kann ich den aktuellen Sicherheitsstatus meiner Azure-Ressourcen anzeigen?
Auf dem Blatt **Security Center** wird auf der Kachel **Ressourcenintegrität** der Gesamtsicherheitsstatus Ihrer Umgebung aufgegliedert nach virtuellen Computern, Webanwendungen und weiteren Ressourcen gezeigt. Jede Ressource hat einen Indikator, der kennzeichnet, ob irgendwelche möglichen Sicherheitsrisiken festgestellt wurden. Wenn Sie auf die Kachel „Ressourcenintegrität“ klicken, werden Ihre Ressourcen angezeigt, und es wird gekennzeichnet, wo Aufmerksamkeit erforderlich ist oder Probleme vorhanden sein können.

### Wodurch wird eine Sicherheitswarnung ausgelöst?
Azure Security Center erfasst, analysiert und kombiniert automatisch Protokolldaten von Ihren Azure-Ressourcen, vom Netzwerk und von Partnerlösungen wie Antischadsoftware und Firewalls. Wurden Bedrohungen erkannt, wird eine Sicherheitswarnung erstellt. Beispiele für eine Erkennung sind:

- Gefährdete virtuelle Computer, die mit bekannten bösartigen IP-Adressen kommunizieren
- Erweiterte Schadsoftware, die mithilfe der Windows-Fehlerberichterstattung erkannt wurde
- Brute-Force-Angriffe gegen virtuelle Computer
- Sicherheitswarnungen von integrierten Partnersicherheitslösungen wie Antischadsoftware oder Web Application Firewalls

### Wo liegt bei der Bedrohungserkennung und Ausgabe von Warnungen der Unterschied zwischen Microsoft Security Response Center und Azure Security Center?
Das Microsoft Security Response Center (MSRC) führt eine selektive Sicherheitsüberwachung im Azure-Netzwerk und in der Infrastruktur durch und empfängt Threat Intelligence-Daten und Missbrauchsmeldungen von Drittanbietern. Wenn MSRC erkennt, dass eine Partei unrechtmäßig oder unbefugt auf Kundendaten zugreift oder dass der Kunde Azure nicht entsprechend den Bestimmungen für den Umgang mit vertraulichen Informationen (Acceptable Use) nutzt, wird der Kunde durch einen Manager für Sicherheitsincidents benachrichtigt. Die Benachrichtigung erfolgt in der Regel durch Senden einer E-Mail an die im Azure Security Center angegebenen Sicherheitskontakte oder an den Besitzer des Azure-Abonnements, falls kein Sicherheitskontakt angegeben wurde.

Das Security Center ist ein Azure-Dienst, der die Azure-Umgebung des Kunden ständig überwacht und Analysefunktionen anwendet, um eine Vielzahl von potenziell schädlichen Aktivitäten automatisch zu erkennen. Diese Erkennungen werden als Sicherheitshinweise im Security Center-Dashboard angegeben.

### Wie werden Berechtigungen in Azure Security Center behandelt?
Azure Security Center unterstützt rollenbasierten Zugriff. Weitere Informationen zur rollenbasierten Zugriffssteuerung (RBAC) in Azure finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Active Directory](../active-directory/role-based-access-control-configure.md).

Wenn ein Benutzer Azure Security Center öffnet, werden nur Empfehlungen und Warnungen angezeigt, die sich auf die Ressourcen beziehen, auf die der Benutzer Zugriff hat. Dies bedeutet, dass ein Benutzer nur Elemente sieht, die sich auf Ressourcen beziehen, für die ihm für das Abonnement oder die Ressourcengruppe, zu der eine Ressource gehört, die Rolle „Besitzer“, „Mitwirkender“ oder „Leser“ zugeordnet ist.

Damit Sie eine Sicherheitsrichtlinie bearbeiten können, müssen Sie ein Besitzer oder Mitwirkender des Abonnements sein.

## Virtual Machines

### Welche Typen von virtuellen Maschinen werden unterstützt?
Die Überwachung der Sicherheitsintegrität und Empfehlungen sind für virtuelle Computer (VMs) verfügbar, die mit dem [klassischen Bereitstellungsmodell oder mit dem Resource Manager-Bereitstellungsmodell](../azure-classic-rm.md) erstellt wurden. Dies gilt auch für VMs, die Teil von Azure Service Fabric-Clustern sind.

Unterstützte Windows-VMs:

- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2

Unterstützte Linux-VMs:

- Ubuntu-Versionen 12.04, 14.04, 15.10, 16.04
- Debian-Versionen 7, 8
- CentOS-Versionen 6.\*, 7.*
- Red Hat Enterprise Linux-Versionen (RHEL) 6.\*, 7.*
- SUSE Linux Enterprise Server-Versionen (SLES) 11.\*, 12.*

VMs, die in einem Clouddienst ausgeführt werden, werden ebenfalls unterstützt. Es werden nur Clouddienst-Webrollen und -Workerrollen überwacht, die in Produktionsslots ausgeführt werden. Weitere Informationen zum Clouddienst finden Sie unter [Übersicht über Cloud Services](../cloud-services/cloud-services-choose-me.md).

Die Security Center-Empfehlung [Systemupdates anwenden](security-center-recommendations.md) besagt, dass Sie für VMs fehlende Sicherheitsupdates und kritische Updates des Systems bereitstellen sollten. Für VMs in Clouddiensten wird in der Empfehlung **Betriebssystemversion aktualisieren** darauf hingewiesen, dass Sie das Betriebssystem auf die aktuelle Version aktualisieren sollten.

<!---HONumber=AcomDC_0706_2016-->