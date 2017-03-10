---
title: "Bewährte Sicherheitsmethoden für IaaS-Workloads in Azure | Microsoft Docs"
description: " Die Workloadmigration zu Azure IaaS bietet die Gelegenheit, die vorhandenen Designs neu zu überdenkens "
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 02c5b7d2-a77f-4e7f-9a1e-40247c57e7e2
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2017
ms.author: barclayn
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: 774feff39bee6f34b1fb292f130d8240ec070c81
ms.lasthandoff: 03/07/2017




---

# <a name="security-best-practices-for-iaas-workloads-in-azure"></a>Bewährte Sicherheitsmethoden für IaaS-Workloads in Azure

Als Sie damit begonnen haben, darüber nachzudenken, Workloads zu Azure IaaS zu verschieben, kamen Ihnen einige Überlegungen wahrscheinlich bereits bekannt vor. Möglicherweise verfügen Sie bereits über Erfahrung mit der Absicherung von virtuellen Umgebungen. Beim Verschieben nach Azure IaaS können Sie Ihre Expertise zur Absicherung virtueller Umgebungen anwenden und außerdem eine Reihe neuer Optionen für die Absicherung Ihres IT-Bestands nutzen.

Bevor wir beginnen, möchten wir folgenden Hinweis geben: Es ist nicht unbedingt zu erwarten, dass die lokalen Ressourcen eins zu eins nach Azure migriert werden können. Die neuen Herausforderungen und Möglichkeiten bieten die Gelegenheit, die vorhandenen Designs, Tools und Prozesse noch einmal zu überdenken.




![Zuständigkeitsbereiche](./media/azure-security-iaas/sec-cloudstack-new.png)

>[!NOTE]
> Ihre Zuständigkeit für die Sicherheit basiert auf dem Typ des Clouddiensts. In obigem Diagramm sind die Informationen zur Zuständigkeitsverteilung für Microsoft und für Sie zusammengefasst.

## <a name="best-practices"></a>Bewährte Methoden
Wir werden einige der in Azure verfügbaren Optionen erläutern, die Ihnen die Erfüllung der Sicherheitsanforderungen Ihres Unternehmens erleichtern können. Dabei müssen wir die verschiedenen Workloadtypen und deren möglicherweise unterschiedliche Sicherheitsanforderungen im Hinterkopf behalten. Keine dieser bewährten Methoden wäre alleine dafür geeignet, Ihre Systeme abzusichern. Wie immer im Bereich Sicherheit müssen Sie die geeigneten Optionen auswählen und prüfen, wie sich die einzelnen Lösungen gegenseitig ergänzen können, indem sie die von den anderen Lösungen hinterlassenen Lücken decken.

### <a name="use-privileged-access-workstations-paw"></a>Verwenden von Privileged Access Workstations (PAW)

Häufig werden Unternehmen Opfer von Cyberangriffen, weil die Administratoren für die Ausführung von Aktionen Konten mit erhöhten Rechten verwenden. In der Regel tun sie dies nicht in böser Absicht, sondern weil es ihnen die vorhandene Konfiguration und die vorhandenen Prozesse erlauben. Die meisten dieser Benutzer verstehen das Risiko aus der konzeptionellen Perspektive, entscheiden sich aber trotzdem für Schritte, die sie selbst auch für riskant halten würden.

Tätigkeiten wie das Abrufen von E-Mails oder das Navigieren im Internet scheinen relativ ungefährlich zu sein, aber Konten mit erhöhten Rechten sind dabei der Gefahr ausgesetzt, dass bösartige Akteure über Navigationsaktivitäten, speziell konzipierte E-Mails oder sonstige Techniken in das Unternehmen eindringen. Bei allen Azure-Administrationsaufgaben wird daher stark empfohlen, abgesicherte Verwaltungsarbeitsstationen einzusetzen und so das Risiko einer versehentlichen Kompromittierung zu reduzieren.

Privileged Access Workstations (PAWs) bieten für sensible Aufgaben ein dediziertes Betriebssystem, das vor Internetangriffen und Bedrohungsüberträgern geschützt ist. Die Trennung solcher sensitiven Aufgaben und Konten von den täglich verwendeten Arbeitsstationen und Geräten bietet einen sehr starken Schutz vor Phishing-Angriffen, vor Sicherheitsrisiken für das Betriebssystem und die Anwendungen, verschiedenen Identitätswechselangriffen und dem Diebstahl von Anmeldeinformationen z.B. mithilfe von Keyloggern, Pass-the-Hash und Pass-the-Ticket.

Der PAW-Ansatz ist eine Erweiterung der gut bewährten und empfohlenen Methode, bei den Administratoren das Admin- und das Benutzerkonto voneinander zu trennen. Bei dieser Methode wird ein persönlich zugewiesenes Administratorkonto verwendet, das von dem Standardbenutzerkonto des Benutzers getrennt ist. PAW baut auf dieser Kontotrennungsmethode auf, indem für solche sensiblen Konten eine vertrauenswürdige Arbeitsstation bereitgestellt wird.

Weitere Informationen über Priviledged Access Workstations und Hinweise zur Implementierung von PAWs finden Sie hier:

- [Privileged Access Workstations](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations)

### <a name="use-multifactor-authentication"></a>Verwenden Sie eine mehrstufige Authentifizierung

In der Vergangenheit wurde für die Prüfung des Zugriffs auf Unternehmensdaten Ihr Netzwerkperimeter verwendet. In einer von Cloud und Mobilität geprägten Welt erfolgt die Steuerung über die Identität: Sie wird dazu verwendet, den IaaS-Zugriff von allen Geräten aus zu steuern und um Sichtbarkeit und Einblick darüber zu erlangen, wo und wie Ihre Daten verwendet werden. Die digitale Identität Ihrer Azure-Benutzer zu schützen, ist die Grundlage für den Schutz Ihrer Abonnements vor Identitätsdiebstahl und sonstiger Cyberkriminalität.

Einer der nützlichsten Schritte, die Sie zur Kontoabsicherung unternehmen können, ist die Aktivierung der zweistufigen Authentifizierung . Die zweistufige Authentifizierung ist eine Authentifizierung mit mehr als nur dem Kennwort. Der zweite Faktor ist ein zusätzlich zum Kennwort verwendeter Faktor.  Dadurch wird das Risiko des Zugriffs durch eine Person gesenkt, die sich das Kennwort einer anderen Person beschaffen konnte.

Azure Multi-Factor Authentication hilft beim Schutz des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren. Das Programm bietet eine sichere Authentifizierung über eine Reihe einfacher Überprüfungsoptionen – Telefonanruf, SMS oder Benachrichtigung in einer mobilen App. Benutzer können die bevorzugte Methode wählen.

Die einfachste Verwendungsart der [Azure MFA](../multi-factor-authentication/multi-factor-authentication.md) ist die mobile App „Microsoft Authenticator“. Diese kann auf Mobilgeräten unter Windows, iOS und Android verwendet werden. Mit der jüngsten Version von Windows 10 und mit Ihrem in Azure AD integrierten lokalen Active Directory kann [Microsoft Hello for Business](../active-directory/active-directory-azureadjoin-passport-deployment.md) für das reibungslose einmalige Anmelden bei Azure-Ressourcen verwendet werden. In diesem Fall wird das Windows 10-Gerät als zweiter Authentifizierungsfaktor verwendet.


Im Fall von Azure lässt sich die zweistufige Authentifizierung am einfachsten durch die Verwendung von Azure Multi-Factor Authentication (MFA) aktivieren. [Azure MFA](../multi-factor-authentication/multi-factor-authentication.md) verfügt über eine Anwendung, die auf Ihren mobilen Geräten verwendet werden kann und per Telefonanruf, SMS oder in der App generierter Code funktioniert. Azure MFA kann außerdem in Ihr lokales Verzeichnis integriert werden.

Für die Konten, mit denen Ihre Azure-Abonnements verwaltet werden, sollten Sie MFA verwenden; für Konten, die sich bei Virtual Machines anmelden können, sollten Sie MFA verwenden, wann immer dies möglich ist. MFA bietet für diese Konten ein viel höheres Sicherheitsniveau als nur die Verwendung eines Kennworts. Andere Arten der zweistufigen Authentifizierung können genauso gut geeignet sein, können aber vielleicht schwieriger bereitzustellen sein, wenn sie nicht bereits verwendet werden.

Auf dem folgenden Screenshot sind einige der für Azure MFA verfügbaren Authentifizierungsoptionen zu sehen.

![MFA-Optionen](./media/azure-security-iaas/mfa-options.png)

### <a name="limit-and-constrain-administrative-access"></a>Begrenzen und Beschränken des Administratorzugangs

Es ist extrem wichtig, die Verwaltungskonten für Ihr Azure-Abonnement abzusichern. Wenn auch nur eines dieser Konten kompromittiert würde, verlieren de facto alle anderen Schritte, die möglicherweise für die Gewährleistung der Vertraulichkeit und Integrität Ihrer Daten unternommen wurden, ihren Wert. Wie sich kürzlich durch den [Edward-Snowden-Leak](https://en.wikipedia.org/wiki/Edward_Snowden) von geheimen Informationen zeigte, sind interne Angriffe in allen Organisationen eine riesige Sicherheitsbedrohung.

Über Administratorrechte verfügende Personen sollten normalerweise anhand von der folgenden oder ähnlicher Kriterien geprüft werden:

- Führt die Person Aufgaben aus, für die privilegierte Verwaltungsrechte notwendig sind?
- Wie oft werden die Aufgaben ausgeführt?
- Gibt es spezifische Gründe dafür, dass die Aufgaben nicht von einem anderen Administrator für die Person ausgeführt werden können?
- Dokumentieren Sie alle sonstigen bekannten Alternativen zur Vergabe privilegierter Rechte, und notieren Sie zu jeder dieser Alternativen, weshalb sie nicht akzeptabel ist

Mit Just-in-Time-Verwaltung kann verhindert werden, dass unnötige Konten mit erhöhten Rechten auch zu Zeiten vorhanden sind, in denen solche Rechte nicht benötigt werden. Die erhöhten Rechte werden für einen begrenzten Zeitraum vergeben, damit die Administratoren ihre Aufgaben erledigen können. Bei Schichtende oder nach der Erledigung einer Aufgabe werden diese Rechte wieder entzogen.

Mit [PIM](../active-directory/active-directory-privileged-identity-management-configure.md) können Sie den Zugriff in Ihrem Unternehmen verwalten, überwachen und steuern. PIM unterstützt Sie dabei, sich über die von den Personen in Ihrem Unternehmen durchgeführten Aktionen bewusst zu bleiben. Weiterhin wird durch das Konzept eines berechtigten Administrators eine Just-in-Time-Verwaltung in Azure AD eingeführt. Bei einem berechtigten Administrator handelt es sich um eine Person, die über ein Konto verfügt, an das potenziell Administratorrechte vergeben werden können. Diese Benutzertypen können einen Aktivierungsprozess durchlaufen und sich für eine begrenzte Zeit Administratorrechte geben lassen.


### <a name="use-devtest-labs"></a>Verwenden von DevTest Labs

Unternehmen, die Azur in Labor- und Entwicklungsumgebungen einsetzen, können mit DevTest Labs agiler testen und entwickeln, da die von der Hardwarebeschaffung verursachten Verzögerungen wegfallen. Leider besteht aber das Risiko, dass der Administrator durch mangelnde Vertrautheit mit Azure oder den Wunsch, die Einführung von Azure zu beschleunigen, die Rechte zu großzügig vergibt. Dadurch kann das Unternehmen unabsichtlich internen Angriffen ausgesetzt werden. Es kann Benutzer geben, die über einen wesentlich umfangreicheren Zugriff verfügen, als sie es sollten.

In Azure bieten wir deswegen jetzt einen Dienst namens [DevTest Labs](../devtest-lab/devtest-lab-overview.md). DevTest Labs verwendet die [rollenbasierte Zugriffssteuerung von Azure](../active-directory/role-based-access-control-what-is.md) (Role based access control; RBAC). Mit RBAC können Sie die Aufgaben in Ihrem Team anhand von Rollen aufteilen, denen jeweils nur das für die Aufgabenausführung notwendige Zugriffsniveau gewährt wird. RBAC beinhaltet vordefinierte Rollen (Eigentümer, Laborbenutzer und Mitwirkende). Diese Rollen können sogar dazu verwendet werden, Rechte an externe Partner zu vergeben. Damit wird die Zusammenarbeit durch diese Rollen erheblich vereinfacht.

Da DevTest Labs RBAC verwendet, ist es möglich, zusätzlich [individuell angepasste Rollen](../devtest-lab/devtest-lab-grant-user-permissions-to-specific-lab-policies.md) zu vergeben. DevTest Labs vereinfacht nicht nur die Berechtigungsverwaltung, sondern auch den Prozess der Umgebungsbereitstellung und den Umgang mit den anderen üblichen Herausforderungen, mit denen sich Teams konfrontiert sehen, die an Entwicklungs- und Testumgebungen arbeiten. DevTest Labs setzt einige Vorbereitung voraus, erleichtert Ihrem Team langfristig aber die Arbeit.

Hier einige der wesentlichen Azure DevTest Labs-Features:

- Administrative Kontrolle über die den Benutzern zur Verfügung stehenden Optionen. Dinge, wie die zulässigen VM-Größen, die maximale Anzahl an VMs und die Zeitpunkte, zu denen diese hoch- und herunterfahren werden, können zentral vom Administrator verwaltet werden.
- Automatisierte Erstellung von Laborumgebungen
- Kostenverfolgung
- Vereinfachte Verteilung von VMs für die temporäre Zusammenarbeit
- Self-Service, mit dem die Benutzer ihre Labore mithilfe von Vorlagen bereitstellen können
- Verbrauchsverwaltung und -begrenzung

![Erstellen eines Entwicklungs-/Testlabs](./media/azure-security-iaas/devtestlabs.png)

Für die Verwendung von DevTest Labs fallen keine zusätzlichen Kosten an. Das Erstellen von Laboren, die Konfiguration von Richtlinien, Vorlagen und Artefakte sind gratis. Sie bezahlen nur für die Azure-Ressourcen, die Sie in den Laboren verwenden, beispielsweise virtuelle Computer, Speicherkonten und virtuelle Netzwerke.



### <a name="control-and-limit-endpoint-access"></a>Prüfen und Begrenzen des Endpunktzugriffs

Labore oder Produktionssysteme in Azure zu hosten, bedeutet, dass der Zugriff auf Ihre Systeme über das Internet möglich sein muss. Standardmäßig ist bei einem virtuellen Windows-Computer der RDP-Port für den Zugriff aus dem Internet geöffnet. Bei virtuellen Linux-Computern gibt es einen geöffneten SSH-Port. Dies bedeutet, dass für die Minimierung des Risikos von unberechtigten Zugriffen Maßnahmen ergriffen werden müssen, um das „Risiko an den Endpunkten zu begrenzen“.

Es gibt Technologien in Azure, die Sie bei der Begrenzung des Zugriffs auf diese administrativen Endpunkte unterstützen können. In Azure können Sie Netzwerksicherheitsgruppen ([NSG](../virtual-network/virtual-networks-nsg.md)) verwenden. Wenn Sie Resource Manager für die Bereitstellung verwenden, wird mit NSGs der Zugriff von allen Netzwerken aus auf lediglich die Verwaltungsendpunkte beschränkt (RDP oder SSH).  Wenn Sie über NSGs nachdenken, sollten Sie auch über Zugriffssteuerungslisten (access control lists; ACLs) nachdenken. Mit ACL können Sie die Netzwerkkommunikation zwischen verschiedenen Segmenten Ihrer Azure-Netzwerke genau steuern. Dies ist vergleichbar mit dem Erstellen von Netzwerken in DMZ oder mit dem Erstellen anderer isolierter Netzwerke. ACL inspizieren zwar nicht den Datenverkehr, unterstützen aber die Netzwerksegmentierung.


In Azure können Sie von Ihrem lokalen Netzwerk aus ein [Standort-zu-Standort-VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) konfigurieren und so de facto ihr lokales Netzwerk in der Cloud fortsetzen.  Dies ist eine weitere Verwendungsmöglichkeit für NSGs, denn Sie könnten die NSGs auch dahingehend ändern, dass der Zugriff nur aus dem lokalen Netzwerk erlaubt ist und für die Verwaltung zunächst eine VPN-Verbindung mit dem Azure-Netzwerk hergestellt werden muss.

Am attraktivsten ist die Standort-zu-Standort-VPN-Option vielleicht in Fällen, in denen Sie Produktionssysteme hosten, die eng in Ihre lokalen Azure-Ressourcen integriert sind.

Alternativ können Sie zum Verwalten von Systemen, die nicht auf lokale Ressourcen zugreifen müssen, die Option [Punkt-zu-Standort](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md) verwenden. Diese Systeme können sich isoliert in ihrem eigenen virtuellen Azure-Netzwerk befinden, und die Administratoren könnten von ihrer Verwaltungsarbeitsstation aus eine VPN-Verbindung zu der von Azure gehosteten Umgebung herstellen.

>[!NOTE]
Mit beiden VPN-Optionen können Sie die ACLs in den NSGs neu konfigurieren, um den Zugriff auf die Verwaltungsendpunkte aus dem Internet zu verbieten.

Eine weitere überlegenswerte Option ist die Bereitstellung eines [Remotedesktopgateways](../multi-factor-authentication/multi-factor-authentication-get-started-server-rdg.md). Mit der Bereitstellung eines solchen Remotedesktopgateways können Sie eine abgesicherte HTTPS-Verbindung zu Remotedesktopservern herstellen und diese Verbindungen gleichzeitig präziser steuern.

Hier einige Features, auf die Sie Zugriff hätten:

- Administratoroptionen zur Verbindungsbeschränkung auf spezifische Systeme
- Smartcard-Authentifizierung oder Azure MFA
- Steuerung der Systeme, mit denen eine Verbindung über das Gateway möglich ist
- Umleitungssteuerung für Geräte und Datenträger

### <a name="use-a-key-management-solution"></a>Verwendung einer Schlüsselverwaltungslösung

Eine sichere Schlüsselverwaltung ist essenziell, um Daten in der Cloud zu schützen. Mit [Azure Key Vault](../key-vault/key-vault-whatis.md) können Sie Verschlüsselungsschlüssel und geheime Daten wie Kennwörter in Hardwaresicherheitsmodulen (HSM) sicher speichern. Zur Steigerung der Sicherheit können Sie Schlüssel in HSMs importieren oder in diesen generieren.

Wenn Sie sich dafür entscheiden, verarbeitet Microsoft Ihre Schlüssel in FIPS 140-2 Level 2-überprüften HSMs (Hardware und Firmware). Überwachen Sie die Verwendung Ihrer Schlüssel mithilfe der Azure-Protokollierung. Übergeben Sie Protokolle an Azure oder Ihr SIEM-Tool zur weiteren Analyse und zum Erkennen von Bedrohungen.

Jeder Benutzer mit einem Azure-Abonnement kann Schlüsseltresore erstellen und verwenden. Wenngleich Key Vault insbesondere Entwicklern und Sicherheitsadministratoren Vorteile bietet, kann es durch einen Organisationsadministrator implementiert und verwaltet werden, der für die Verwaltung der Azure-Dienste verantwortlich ist.


### <a name="encrypt-virtual-disks-and-disk-storage"></a>Verschlüsseln virtueller Datenträger

[Azure Disk Encryption](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0) begegnet der Bedrohung durch Datendiebstahl und dem Risiko eines unberechtigten Zugriffs durch das Verschieben eines Datenträgers. Der Datenträger könnte an ein anderes System angeschlossen sein und so die sonstigen Sicherheitsprüfungen umgehen. In Windows wird für die Verschlüsselung von Betriebssystemen und Datenlaufwerken [BitLocker](https://technet.microsoft.com/library/hh831713 ) verwendet, in Linux DM-Crypt. Zur Verwaltung der Verschlüsselungsschlüssel lässt sich Azure Disk Encryption in Key Vault integrieren. Es ist für Standard-VMs und VMs mit Premiumspeicher verfügbar.

Weitere Informationen finden Sie im Artikel über [Azure Disk Encryption in Windows- und Linux-IaaS-VMs](azure-security-disk-encryption.md).

[Azure Storage Service Encryption](../storage/storage-service-encryption.md) schützt Ihre ruhenden Daten. Es wird auf Speicherkontoebene aktiviert und verschlüsselt die Daten, wenn sie in unsere Rechenzentren geschrieben werden. Beim Zugriff werden die Daten automatisch entschlüsselt. Sie unterstützt die folgenden Szenarien:

- Verschlüsselung von Block-, Anfüge- und Seitenblobs.
- Verschlüsselung von lokalen archivierten VHDs und Vorlagen, die in Azure eingebracht werden.
- Verschlüsselung der zu Grunde liegenden Betriebssystemdatenträger und sonstigen Datenträger für IaaS-VMs, die Sie mit Ihren VHDs erstellt haben.

Bevor Sie mit der Verschlüsselung von Azure Storage fortfahren, beachten Sie bitte die folgenden beiden wichtigen Einschränkungen:

- Für klassische Speicherkonten steht es nicht zu Verfügung.
- Es werden nur Daten verschlüsselt, die nach der Aktivierung der Verschlüsselung geschrieben werden.

### <a name="use-a-centralized-security-management-system"></a>Verwenden eines zentralisierten Sicherheitsverwaltungssystems

Ihre Server müssen auf Patches, Konfigurationen, Ereignisse und Aktivitäten überwacht werden, die als Sicherheitsgefahren angesehen werden können. Um diese Probleme zu lösen, können Sie [Security Center](https://azure.microsoft.com/services/security-center/) und die [Operations Management Suite für Sicherheit und Compliance](https://azure.microsoft.com/services/security-center/) verwenden. Beide Optionen gehen über die Konfiguration innerhalb des Betriebssystems hinaus und ermöglichen es auch, die Konfiguration der zugrunde liegenden Infrastruktur wie z.B. die Netzwerkkonfiguration und die Verwendung von virtuellen Geräten zu überwachen.

### <a name="operating-system-management-best-practices"></a>Bewährte Methoden für die Betriebssystemverwaltung

Bei einer IaaS-Bereitstellung sind Sie genauso für die Verwaltung der von Ihnen bereitgestellten Systeme verantwortlich wie für die Server und Arbeitsstationen in Ihrer Umgebung. Dies bedeutet, dass Patching, Verstärkung, Rechtevergabe und alle sonstigen mit der Wartung Ihres Systems verbundenen Aktivitäten immer noch in Ihrem Verantwortungsbereich liegen.  Für Systeme, die eng in ihre lokale Ressourcen integriert sind, sollten Sie dieselben Tools und Verfahren verwenden, die auch lokal zum Einsatz kommen, z.B. Antivirus, Antischadsoftware, Patching und Backup.

**Härtung:** In Azure IaaS sollten alle virtuellen Computer so gehärtet werden, dass nur für von den installierten Anwendungen benötigten Dienstendpunkte verfügbar sind. Bei virtuellen Windows-Computern sollten Sie sich an die von Microsoft als Baselines für die Security Compliance Manager-Lösung veröffentlichten Empfehlungen halten.

[Security Compliance Manager (SCM):](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx) Wir haben kürzlich Version 4.0 veröffentlicht. Es handelt sich dabei um ein frei verfügbares Tool, mit dem Sie Ihre Desktopcomputer, traditionellen Rechenzentren und ihre private und öffentliche Cloud schnell mit Gruppenrichtlinien und System Center Configuration Manager konfigurieren und verwalten können.

SCM bietet bereitstellungsfähige Richtlinien und getestete DCM-Konfigurationspakete. Diese Baselines stützen sich auf die Empfehlungen des [Microsoft-Leitfadens zum Thema Sicherheit](https://technet.microsoft.com/en-us/library/cc184906.aspx) und auf die bewährten Branchenmethoden für die Verwaltung abweichender Konfigurationen, Adresskonformitätsanforderungen und zur Reduzierung von Sicherheitsbedrohungen.

SCM können Sie für den Import der aktuellen Konfiguration Ihrer Computer verwenden. Hierfür stehen zwei Methoden zur Verfügung: Import von Active Directory-basierten Gruppenrichtlinien und Import der Konfiguration eines „Golden-Master-Referenzcomputers“ mithilfe von [LocalGPO](https://blogs.technet.microsoft.com/secguide/2016/01/21/lgpo-exe-local-group-policy-object-utility-v1-0/), um die lokale Gruppenrichtlinie zu sichern und im Anschluss daran in SCM zu importieren.

Vergleichen Sie Ihre Standards mit den bewährten Branchenmethoden, passen Sie Ihre Standards an, und erstellen Sie neue Richtlinien und DCM-Konfigurationspakete.
Es wurden für alle unterstützten Betriebssysteme Baselines veröffentlicht, darunter Windows 10 Anniversary Update und Windows Server 2016.


**Installieren und Verwalten von Antischadsoftware**

Für Umgebungen, die von der Produktionsumgebung getrennt gehostet werden, gibt es eine Antischadsoftware-Erweiterung, die zum Schutz Ihrer virtuellen Computer und Clouddienste verwendet werden kann und sich in [Azure Security Center](../security-center/security-center-intro.md) integrieren lässt.


[Microsoft Antimalware](azure-security-antimalware.md) umfasst Features wie Echtzeitschutz, geplante Überprüfungen, Malwareproblembehandlung, Signaturupdates, Modulupdates, Beispielberichte, Sammlungen von Ausschlussereignissen und [PowerShell-Unterstützung](https://msdn.microsoft.com/library/dn771715.aspx).

![Azure Antimalware](./media/azure-security-iaas/azantimalware.png)

**Installieren Sie die aktuellsten Sicherheitsupdates:** Zu den ersten von den Kunden in Azure verschobenen Workloads zählen Labore und Systeme mit externer Anbindung. Wenn Sie virtuelle Computer in Azure hosten, die wiederum Anwendungen oder Dienste hosten, die im Internet zugänglich gemacht werden müssen, müssen Sie beim Patching aufmerksam sein. Denken Sie daran, dass dies über ein Patching des Betriebssystems hinausgeht. Ungepatchte Sicherheitsrisiken in Drittanbieter-Anwendungen können ebenfalls zu Problemen führen, die mit einer guten Patchverwaltung leicht hätten vermieden werden können.

**Bereitstellen und Testen einer Sicherungslösung**

Genau wie Sicherheitsupdates müssen auch Sicherungen so behandelt werden wie alle anderen Vorgänge. Dies gilt für alle Systeme in Ihrer Produktionsumgebung, die sich bis in die Cloud erstreckt. Test- und Entwicklungssysteme müssen Sicherungsstrategien verfolgen, die dieselben Wiederherstellungsfunktionalitäten bieten, an die sich die Benutzer auf Basis ihrer Erfahrung mit lokalen Umgebungen gewöhnt haben.

In Azure übernommene Workloads sollten sich wenn möglich in die vorhandenen Sicherheitslösungen integrieren lassen, oder Sie können [Azure Backup](../backup/backup-azure-arm-vms.md) verwenden, um sich bei der Erfüllung ihrer Sicherungsanforderungen unterstützen zu lassen.


### <a name="monitor"></a>Überwachen

[Security Center](../security-center/security-center-intro.md) wertet ständig den Sicherheitsstatus Ihrer Azure-Ressourcen aus, um potenzielle Sicherheitsrisiken zu erkennen. Anhand einer Liste mit Empfehlungen werden Sie durch den Prozess des Konfigurierens der erforderlichen Sicherheitsmechanismen geführt.

Beispiele:

- Bereitstellung von Antischadsoftware, um Schadsoftware zu erkennen und zu entfernen
- Konfigurieren von Netzwerksicherheitsgruppen und Regeln, um Datenverkehr zu virtuellen Maschinen zu steuern
- Bereitstellung von Web Application Firewalls als Schutz vor Angriffen, die auf Ihre Webanwendungen abzielen
- Bereitstellen von fehlenden Systemupdates
- Behandeln von Betriebssystemkonfigurationen, die nicht den empfohlenen Basisregeln entsprechen

In der Abbildung unten Sie sehen einige der Optionen, die Sie in Security Center aktivieren können.

![Azure Security Center-Richtlinien](./media/azure-security-iaas/security-center-policies.png)

 Die [Operations Management Suite](../operations-management-suite/operations-management-suite-overview.md) ist eine cloudbasierte IT-Verwaltungslösung von Microsoft, die Ihnen das Verwalten und Schützen Ihrer lokalen und cloudbasierten Infrastruktur erleichtert. Da OMS als cloudbasierter Dienst implementiert wird, kann die Lösung schnell und mit minimalen Investitionen in Infrastrukturressourcen bereitgestellt werden.

Neue Features werden automatisch bereitgestellt, sodass Sie Kosten für die laufende Wartung und Upgrades sparen. OMS lässt sich auch in System Center Operations Manager integrieren.  OMS besteht aus verschiedenen Komponenten, die Sie bei der besseren Verwaltung Ihrer Azure-Workloads unterstützen. Zu diesen Komponenten zählt ein [Sicherheits- und Compliancemodul](../operations-management-suite/oms-security-getting-started.md).

Mit den Sicherheits- und Compliancefeatures in OMS können Sie Informationen über Ihre Ressourcen einsehen. Diese Informationen sind in fünf Hauptkategorien untergliedert:

- Sicherheitsdomänen: In diesem Bereich können Sie die Sicherheitsdatensätze in Abhängigkeit der Zeit näher untersuchen. Außerdem können Sie die Bereiche Bewertung von Malware, Updatebewertung, Netzwerksicherheit, Informationen zu Identität und Zugriff und Computer mit Sicherheitsereignissen nutzen und schnell auf das Azure Security Center-Dashboard zugreifen.
- Relevante Probleme: Mit dieser Option können Sie schnell identifizieren, wie viele aktive Probleme vorhanden sind und welchen Schweregrad sie jeweils haben.
- Ermittlungen (Vorschau): Ermöglicht die Identifizierung von Angriffsmustern, indem Sicherheitswarnungen visualisiert werden, wenn sie für Ihre Ressourcen auftreten.
- Informationen zu Bedrohungen: Ermöglicht die Identifizierung von Angriffsmustern, indem die Gesamtzahl der Server mit ausgehendem schädlichem IP-Datenverkehr, der Bedrohungstyp und eine Karte mit den Ursprungsorten dieser IP-Adressen visualisiert werden.
- Allgemeine Sicherheitsabfragen: Diese Option liefert Ihnen eine Liste mit den häufigsten Sicherheitsabfragen, die Sie zum Überwachen der Umgebung verwenden können. Wenn Sie in eine dieser Abfragen klicken, wird das Blatt „Suche“ mit den Abfrageergebnissen geöffnet.

Der folgende Screenshot zeigt ein Beispiel für die Art von Informationen, die von OMS angezeigt werden können.

![OMS-Sicherheitsbaselines](./media/azure-security-iaas/oms-security-baseline.png)



## <a name="next-steps"></a>Nächste Schritte


* [Blog des Azure-Sicherheitsteams](https://blogs.msdn.microsoft.com/azuresecurity/)
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)
* [Sicherheit in Azure – bewährte Methoden und Muster](security-best-practices-and-patterns.md)

