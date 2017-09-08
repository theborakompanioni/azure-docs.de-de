---
title: "Bewährte Sicherheitsmethoden für IaaS-Workloads in Azure | Microsoft Docs"
description: " Die Workloadmigration zu Azure IaaS ist eine gute Gelegenheit zur Neubewertung unserer Designs. "
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
ms.date: 08/29/2017
ms.author: barclayn
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: f93211d289553b7a8afbe8c17fa4847f3d4585a8
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="security-best-practices-for-iaas-workloads-in-azure"></a>Bewährte Sicherheitsmethoden für IaaS-Workloads in Azure

Als Sie die Verschiebung von Workloads in Azure IaaS (Infrastructure-as-a-Service) in Betracht gezogen haben, kamen Ihnen einige Überlegungen wahrscheinlich bereits bekannt vor. Unter Umständen sind Sie bereits mit dem Schutz virtueller Umgebungen vertraut. Bei der Verschiebung nach Azure IaaS können Sie auf Ihr Know-how im Zusammenhang mit dem Schutz virtueller Umgebungen zurückgreifen und eine Reihe neuer Optionen für den Schutz Ihrer Ressourcen nutzen.

Zunächst ein Hinweis: Es ist nicht unbedingt zu erwarten, dass sich lokale Ressourcen 1:1 nach Azure migrieren lassen. Die neuen Herausforderungen und Möglichkeiten sind eine gute Gelegenheit, um die vorhandenen Designs, Tools und Prozesse neu zu bewerten.

Ihre Zuständigkeit für die Sicherheit basiert auf dem Typ des Clouddiensts. Das folgende Diagramm enthält eine Zusammenfassung der Zuständigkeitsverteilung zwischen Microsoft und Ihnen:


![Zuständigkeitsbereiche](./media/azure-security-iaas/sec-cloudstack-new.png)


Wir erläutern einige der in Azure verfügbaren Optionen, die Sie bei der Erfüllung der Sicherheitsanforderungen Ihrer Organisation unterstützen. Bedenken Sie, dass für unterschiedliche Arten von Workloads unterschiedliche Sicherheitsanforderungen gelten können. Keine dieser bewährten Methoden wäre alleine dafür geeignet, Ihre Systeme abzusichern. Genau wie bei anderen Sicherheitsaspekten müssen Sie die geeigneten Optionen auswählen und prüfen, wie sich die einzelnen Lösungen gegenseitig ergänzen können.

## <a name="use-privileged-access-workstations"></a>Verwenden von Privileged Access Workstations

Häufig werden Organisationen Opfer von Cyberangriffen, weil Administratoren bei der Ausführung von Aktionen Konten mit erhöhten Rechten verwenden. In der Regel tun sie dies nicht in böser Absicht, sondern weil es ihnen die vorhandene Konfiguration und die vorhandenen Prozesse erlauben. Die meisten dieser Benutzer sind sich des Risikos dieser Aktionen grundsätzlich bewusst, das hält sie aber nicht davon ab, sie trotzdem auszuführen.

Tätigkeiten wie das Abrufen von E-Mails oder das Surfen im Internet erscheinen zunächst recht harmlos. Doch sie stellen möglicherweise Konten mit erhöhten Rechten böswilligen Akteuren zur Verfügung, die Ihr System gefährden können. Aktivitäten im Browser, speziell gestaltete E-Mails oder andere Methoden können genutzt werden, um sich Zugriff auf Ihr Unternehmenssystem zu verschaffen. Daher empfehlen wir bei allen Azure-Administrationsaufgaben dringend die Verwendung sicherer Verwaltungsarbeitsstationen. Diese bieten eine Möglichkeit, das Risiko einer versehentlichen Gefährdung des Systems zu verringern.

Privileged Access Workstations (PAWs) bieten für sensible Aufgaben ein dediziertes Betriebssystem, das vor Internetangriffen und Bedrohungsvektoren geschützt ist. Das Trennen dieser sensiblen Aufgaben und Konten von Arbeitsstationen und Geräten für die tagtägliche Nutzung sorgt für hohen Schutz. Diese Trennung schränkt die Auswirkungen von Phishingangriffen, Schwachstellen in Anwendungen und im Betriebssystem, verschiedenartigen Angriffen durch Identitätswechsel und des Diebstahls von Anmeldeinformationen ein. (Protokollierung von Tastaturanschlägen, Pass-the-Hash und Pass-the-Ticket)

Der PAW-Ansatz ist eine Erweiterung der bewährten und empfohlenen Methode, ein individuell zugewiesenes Administratorkonto zu verwenden. Das Administratorkonto ist von einem Standardbenutzerkonto getrennt. Eine PAW bietet eine vertrauenswürdige Arbeitsstation für diese sensiblen Konten.

Weitere Informationen und eine Implementierungsanleitung finden Sie unter [Privileged Access Workstations](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations).

## <a name="use-multi-factor-authentication"></a>Verwenden von Multi-Factor Authentication

In der Vergangenheit wurde für die Prüfung des Zugriffs auf Unternehmensdaten Ihr Netzwerkperimeter verwendet. In einer von der Cloud und von Mobilgeräten geprägten Welt fungiert die Identität als Steuerungsebene: Sie wird verwendet, um den Zugriff eines beliebigen Geräts auf IaaS-Dienste zu steuern. Außerdem sorgt sie für Transparenz und gibt Aufschluss darüber, wo und wie Ihre Daten verwendet werden. Die digitale Identität Ihrer Azure-Benutzer zu schützen, ist die Grundlage für den Schutz Ihrer Abonnements vor Identitätsdiebstahl und sonstiger Cyberkriminalität.

Einer der hilfreichsten Schritte, die Sie zum Schutz eines Kontos unternehmen können, ist die Aktivierung der zweistufigen Authentifizierung. Bei der zweistufigen Authentifizierung wird zusätzlich zu einem Kennwort noch eine weitere Komponente verwendet. Dadurch verringert sich das Risiko des Zugriffs durch eine Person, die sich das Kennwort einer anderen Person beschafft hat.

[Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) trägt zum Schutz des Zugriffs auf Daten und Anwendungen bei und bietet gleichzeitig ein einfaches Anmeldeverfahren für Benutzer. Sie ermöglicht eine sichere Authentifizierung mit verschiedenen komfortablen Überprüfungsoptionen: Telefonanruf, SMS oder Benachrichtigung in einer mobilen App. Benutzer können die von ihnen bevorzugte Methode wählen.

Am einfachsten lässt sich Multi-Factor Authentication über die mobile App Microsoft Authenticator nutzen. Diese kann auf Mobilgeräten unter Windows, iOS und Android verwendet werden. Mit der neuesten Version von Windows 10 und der Integration der lokalen Active Directory-Instanz in Azure Active Directory (Azure AD) kann [Windows Hello for Business](../active-directory/active-directory-azureadjoin-passport-deployment.md) für reibungsloses einmaliges Anmelden bei Azure-Ressourcen verwendet werden. In diesem Fall wird das Windows 10-Gerät als zweiter Authentifizierungsfaktor verwendet.

Für Konten, die Ihre Azure-Abonnements verwaltet, sowie für Konten, die sich bei virtuellen Computern anmelden können, bietet Multi-Factor Authentication ein höheres Maß an Sicherheit als nur die Verwendung eines Kennworts. Andere Arten der zweistufigen Authentifizierung sind unter Umständen ebenso gut geeignet, aber ggf. schwieriger bereitzustellen, sofern sie nicht bereits in der Produktion zum Einsatz kommen.

Der folgende Screenshot zeigt einige der für Azure Multi-Factor Authentication verfügbaren Authentifizierungsoptionen:

![Optionen für Multi-Factor Authentication](./media/azure-security-iaas/mfa-options.png)

## <a name="limit-and-constrain-administrative-access"></a>Begrenzen und Beschränken des Administratorzugriffs

Es ist extrem wichtig, die Verwaltungskonten für Ihr Azure-Abonnement abzusichern. Wenn auch nur eines dieser Konten kompromittiert wird, verlieren alle anderen Schritte, die möglicherweise zur Gewährleistung der Vertraulichkeit und Integrität Ihrer Daten unternommen wurden, ihre Wirkung. Wie kürzlich von [Edward Snowden ](https://en.wikipedia.org/wiki/Edward_Snowden) gezeigt, sind interne Angriffe in allen Organisationen eine riesige Sicherheitsbedrohung.

Im Anschluss finden Sie Beispielkriterien zur Beurteilung von Personen mit Administratorrechten:

- Führt die Person Aufgaben aus, für die privilegierte Verwaltungsrechte notwendig sind?
- Wie oft werden die Aufgaben ausgeführt?
- Gibt es einen spezifischen Grund dafür, dass die Aufgaben nicht von einem anderen Administrator für die Person ausgeführt werden können?

Dokumentieren Sie alle sonstigen bekannten Alternativen zur Vergabe privilegierter Rechte, und notieren Sie zu jeder dieser Alternativen, weshalb sie nicht akzeptabel ist

Die Just-in-Time-Verwaltung sorgt dafür, dass keine unnötigen Konten mit erhöhten Rechten vorhanden sind, wenn solche Rechte gerade nicht benötigt werden. Erhöhte Rechte werden Konten für einen begrenzten Zeitraum gewährt, damit die Administratoren ihre Aufgaben erledigen können. Bei Schichtende oder nach Erledigung einer Aufgabe werden die Rechte dann wieder entzogen.

Mithilfe von [Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md) können Sie den Zugriff in Ihrer Organisation verwalten, überwachen und steuern. Dadurch sind Sie stets über die Aktionen informiert, die Personen in Ihrer Organisation durchführen. Durch das Konzept berechtigter Administratoren wird außerdem eine Just-in-Time-Verwaltung in Azure AD eingeführt. Bei einem berechtigten Administrator handelt es sich um eine Person, die über ein Konto verfügt, an das potenziell Administratorrechte vergeben werden können. Diese Benutzertypen können einen Aktivierungsprozess durchlaufen und sich für eine begrenzte Zeit Administratorrechte geben lassen.


## <a name="use-devtest-labs"></a>Verwenden von DevTest Labs

Organisationen, die Azure in Lab- und Entwicklungsumgebungen einsetzen, können agiler testen und entwickeln, da die von der Hardwarebeschaffung verursachten Verzögerungen wegfallen. Mangelnde Vertrautheit mit Azure oder der Wunsch, die Einführung von Azure zu beschleunigen, kann jedoch leider dazu führen, dass der Administrator die Rechte zu großzügig vergibt. Dadurch kann die Organisation für interne Angriffe anfällig werden. Einige Benutzer verfügen unter Umständen über wesentlich umfangreichere Zugriffsrechte als beabsichtigt.

Der Dienst [Azure DevTest Labs](../devtest-lab/devtest-lab-overview.md) verwendet die [rollenbasierte Zugriffssteuerung von Azure](../active-directory/role-based-access-control-what-is.md) (Role Based Access Control, RBAC). Mit RBAC können Sie die Aufgaben in Ihrem Team in Rollen aufteilen, denen jeweils nur das für die Aufgabenausführung notwendige Zugriffsniveau gewährt wird. RBAC beinhaltet vordefinierte Rollen (Besitzer, Lab-Benutzer und Mitwirkender). Mit diesen Rollen können Sie sogar Rechte für externe Partner gewähren, was die Zusammenarbeit erheblich vereinfacht.

Da DevTest Labs RBAC verwendet, können zusätzlich [benutzerdefinierte Rollen](../devtest-lab/devtest-lab-grant-user-permissions-to-specific-lab-policies.md) erstellt werden. DevTest Labs vereinfacht nicht nur die Berechtigungsverwaltung, sondern auch den Prozess der Umgebungsbereitstellung. Darüber hinaus unterstützt Sie der Dienst bei der Bewältigung anderer typischer Herausforderungen von Teams, die an Entwicklungs- und Testumgebungen arbeiten. DevTest Labs erfordert zwar eine gewisse Vorbereitung, erleichtert Ihrem Team aber langfristig die Arbeit.

Azure DevTest Labs bietet unter anderem folgende Features:

- Administrative Kontrolle über die den Benutzern zur Verfügung stehenden Optionen. Aspekte wie zulässige VM-Größen, die maximale Anzahl von virtuellen Computern und die Zeiten, zu denen virtuelle Computer hoch- und herunterfahren werden, können vom Administrator zentral verwaltet werden.
- Automatisierte Erstellung von Lab-Umgebungen
- Kostenüberwachung
- Vereinfachte Verteilung virtueller Computer für vorübergehende Zusammenarbeit
- Self-Service, mit dem Benutzer ihre Labs mithilfe von Vorlagen bereitstellen können
- Verbrauchsverwaltung und -begrenzung

![Erstellen eines Labs mithilfe von DevTest Labs](./media/azure-security-iaas/devtestlabs.png)

Für die Verwendung von DevTest Labs fallen keine zusätzlichen Kosten an. Die Erstellung von Labs, Richtlinien, Vorlagen und Artefakten ist kostenlos. Sie bezahlen nur für die Azure-Ressourcen, die Sie in Ihren Labs verwenden (also etwa für virtuelle Computer, Speicherkonten und virtuelle Netzwerke).



## <a name="control-and-limit-endpoint-access"></a>Steuern und Begrenzen des Endpunktzugriffs

Labore oder Produktionssysteme in Azure zu hosten, bedeutet, dass der Zugriff auf Ihre Systeme über das Internet möglich sein muss. Standardmäßig ist bei einem neuen virtuellen Windows-Computer der RDP-Port für den Zugriff aus dem Internet geöffnet. Bei virtuellen Linux-Computern ist der SSH-Port geöffnet. Um das Risiko unberechtigter Zugriffe zu minimieren, ist eine Begrenzung der verfügbar gemachten Endpunkte erforderlich.

Azure bietet Technologien, die Sie bei der Begrenzung des Zugriffs auf diese administrativen Endpunkte unterstützen. In Azure können Sie [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md) (NSGs) verwenden. Wenn Sie für die Bereitstellung Azure Resource Manager verwenden, wird mit NSGs der Zugriff aus allen Netzwerken auf die Verwaltungsendpunkte (RDP oder SSH) beschränkt. Wenn Sie über NSGs nachdenken, sollten Sie auch über Zugriffssteuerungslisten (access control lists; ACLs) nachdenken. Mit ACL können Sie die Netzwerkkommunikation zwischen verschiedenen Segmenten Ihrer Azure-Netzwerke genau steuern. Das ist vergleichbar mit dem Erstellen von Netzwerken in Umkreisnetzwerken oder anderen isolierten Netzwerken. ACLs untersuchen zwar nicht den Datenverkehr, helfen aber bei der Netzwerksegmentierung.


In Azure können Sie ein [Site-to-Site-VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) über Ihr lokales Netzwerk konfigurieren. Ein Site-to-Site-VPN erweitert Ihr lokales Netzwerk in die Cloud. Hierbei handelt es sich um eine weitere Verwendungsmöglichkeit für NSGs. Diese können so angepasst werden, dass der Zugriff nur über das lokale Netzwerk möglich ist. Anschließend können Sie festlegen, dass zur Verwaltung zunächst eine VPN-Verbindung mit dem Azure-Netzwerk hergestellt werden muss.

Am attraktivsten ist die Site-to-Site-VPN-Option häufig in Fällen, in denen Sie Produktionssysteme hosten, die eng mit Ihren lokalen Azure-Ressourcen verknüpft sind.

Alternativ können Sie zum Verwalten von Systemen, die nicht auf lokale Ressourcen zugreifen müssen, die Option [Point-zu-Site](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md) verwenden. Diese Systeme können in ihrem eigenen virtuellen Azure-Netzwerk isoliert werden. Administratoren können über ihre Verwaltungsarbeitsstation eine VPN-Verbindung mit der in Azure gehosteten Umgebung herstellen.

>[!NOTE]
>Mit beiden VPN-Optionen können Sie die ACLs für die NSGs neu konfigurieren, um den Internetzugriff auf Verwaltungsendpunkte zu unterbinden.

Eine weitere überlegenswerte Option ist die Bereitstellung eines [Remotedesktopgateways](../multi-factor-authentication/multi-factor-authentication-get-started-server-rdg.md). Mit dieser Bereitstellung können Sie eine sichere HTTPS-Verbindung mit Remotedesktopservern herstellen und diese Verbindungen gleichzeitig präziser steuern.

Zur Verfügung stehende Features:

- Administratoroptionen zur Verbindungsbeschränkung auf spezifische Systeme
- Smartcardauthentifizierung oder Azure Multi-Factor Authentication
- Steuerung der Systeme, mit denen eine Verbindung über das Gateway möglich ist
- Umleitungssteuerung für Geräte und Datenträger

## <a name="use-a-key-management-solution"></a>Verwendung einer Schlüsselverwaltungslösung

Eine sichere Schlüsselverwaltung ist essenziell, um Daten in der Cloud zu schützen. Mit [Azure Key Vault](../key-vault/key-vault-whatis.md) können Sie Verschlüsselungsschlüssel und geheime Daten wie Kennwörter in Hardwaresicherheitsmodulen (HSM) sicher speichern. Zur Steigerung der Sicherheit können Sie Schlüssel in HSMs importieren oder in diesen generieren.

Microsoft verarbeitet Ihre Schlüssel in mit FIPS 140-2 (Level 2) überprüften HSMs (Hardware und Firmware). Überwachen und überprüfen Sie die Verwendung Ihrer Schlüssel mithilfe der Azure-Protokollierung: Übermitteln Sie Protokolle zur weiteren Analyse und Bedrohungserkennung an Azure oder an Ihr eigenes SIEM-System (Security Information and Event Management).

Jeder Benutzer mit einem Azure-Abonnement kann Schlüsseltresore erstellen und verwenden. Von Key Vault profitieren zwar insbesondere Entwickler und Sicherheitsadministratoren, die Lösung kann jedoch durch einen Administrator implementiert und verwaltet werden, der für die Verwaltung von Azure-Diensten in einer Organisation zuständig ist.


## <a name="encrypt-virtual-disks-and-disk-storage"></a>Verschlüsseln von virtuellen Datenträgern und Datenträgerspeicher

[Azure Disk Encryption](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0) wirkt der Gefahr von Datendiebstahl und der Offenlegung von Daten entgegen, die durch unberechtigten Zugriff mittels Datenträgerverschiebung möglich wären. Der Datenträger kann an ein anderes System angefügt werden, um andere Sicherheitskontrollen zu umgehen. Unter Windows werden Betriebssystem- und Datenlaufwerke mithilfe von [BitLocker](https://technet.microsoft.com/library/hh831713) verschlüsselt. Unter Linux wird dafür DM-Crypt verwendet. Azure Disk Encryption arbeitet bei der Steuerung und Verwaltung der Verschlüsselungsschlüssel mit Key Vault zusammen. Die Lösung steht für virtuelle Standardcomputer und für virtuelle Computer mit Storage Premium zur Verfügung.

Weitere Informationen finden Sie unter [Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer](azure-security-disk-encryption.md).

[Azure Storage Service Encryption](../storage/common/storage-service-encryption.md) schützt Ihre ruhenden Daten. Die Aktivierung erfolgt auf der Speicherkontoebene. Die Lösung verschlüsselt Daten beim Schreiben in unsere Datencenter und entschlüsselt sie automatisch, wenn Sie darauf zugreifen. Sie unterstützt die folgenden Szenarien:

- Verschlüsselung von Block-, Anfüge- und Seitenblobs
- Verschlüsselung archivierter VHDs und Vorlagen, die aus der lokalen Umgebung in Azure eingebracht werden
- Verschlüsselung der zugrunde liegenden Betriebssystemdatenträger und sonstiger Datenträger für virtuelle IaaS-Computer, die Sie mit Ihren VHDs erstellt haben

Beachten Sie die beiden folgenden Einschränkungen, bevor Sie mit Azure Storage Encryption fortfahren:

- Für klassische Speicherkonten steht es nicht zu Verfügung.
- Es werden nur Daten verschlüsselt, die nach der Aktivierung der Verschlüsselung geschrieben werden.

## <a name="use-a-centralized-security-management-system"></a>Verwenden eines zentralen Sicherheitsverwaltungssystems

Ihre Server müssen auf Patches, Konfigurationen, Ereignisse und Aktivitäten überwacht werden, die möglicherweise ein Sicherheitsrisiko darstellen. Zur Beseitigung dieser Risiken können Sie [Security Center](https://azure.microsoft.com/services/security-center/) und die [Operations Management Suite-Lösung für Sicherheit und Überwachung](https://azure.microsoft.com/services/security-center/) verwenden. Diese beiden Optionen gehen über die Konfiguration im Betriebssystem hinaus. Darüber hinaus ermöglichen sie die Überwachung der Konfiguration der zugrunde liegenden Infrastruktur (etwa die Netzwerkkonfiguration und die Verwendung virtueller Geräte).

## <a name="manage-operating-systems"></a>Verwalten von Betriebssystemen

Bei einer IaaS-Bereitstellung sind Sie auch weiterhin für die Verwaltung der von Ihnen bereitgestellten Systeme zuständig – genau wie bei allen anderen Servern und Arbeitsstationen in Ihrer Umgebung. Patching, Härtung, Rechtezuweisung und alle anderen Aktivitäten in Verbindung mit der Wartung Ihres Systems fallen weiterhin in Ihren Zuständigkeitsbereich. Bei Systemen, die eng mit Ihren lokalen Ressourcen verknüpft sind, empfiehlt es sich unter Umständen, für Aspekte wie Antivirus, Antischadsoftware, Patching und Backup die gleichen Tools und Verfahren zu verwenden wie in der lokalen Umgebung.

### <a name="harden-systems"></a>Härten von Systemen
Alle virtuellen Computer in Azure IaaS müssen so gehärtet werden, dass sie nur Dienstendpunkte verfügbar machen, die für die installierten Anwendungen benötigt werden. Halten Sie sich bei virtuellen Windows-Computern an die Empfehlungen, die Microsoft als Baselines für die [Security Compliance Manager-Lösung](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx) veröffentlicht.

Security Compliance Manager ist ein kostenloses Tool. Mit diesem Tool können Sie Ihre Desktopcomputer, Ihre herkömmlichen Datencenter und Ihre private und öffentliche Cloud schnell mithilfe von Gruppenrichtlinien und System Center Configuration Manager konfigurieren und verwalten.

Security Compliance Manager bietet bereitstellungsfähige Richtlinien und getestete Konfigurationspakete für die Verwaltung gewünschter Konfigurationen. Die Baselines basieren auf den Empfehlungen des [Microsoft-Leitfadens zum Thema Sicherheit](https://technet.microsoft.com/en-us/library/cc184906.aspx) und auf den bewährten Methoden der Branche. Sie unterstützen Sie bei der Verwaltung von Konfigurationsabweichungen, bei der Erfüllung von Complianceanforderungen und bei der Beseitigung von Sicherheitsbedrohungen.

Mit Security Compliance Manager können Sie die aktuelle Konfiguration Ihrer Computer importieren. Hierfür stehen zwei Methoden zur Verfügung: Zum einen können Sie Active Directory-basierte Gruppenrichtlinien importieren. Zum anderen können Sie die Konfiguration eines „Golden-Master-Referenzcomputers“ importieren und dabei mithilfe des [LocalGPO-Tools](https://blogs.technet.microsoft.com/secguide/2016/01/21/lgpo-exe-local-group-policy-object-utility-v1-0/) die lokale Gruppenrichtlinie sichern. Die lokale Gruppenrichtlinie kann anschließend in Security Compliance Manager importiert werden.

Vergleichen Sie Ihre Standards mit den bewährten Methoden der Branche, passen Sie Ihre Standards an, und erstellen Sie neue Richtlinien und Konfigurationspakete für die Verwaltung gewünschter Konfigurationen. Es wurden für alle unterstützten Betriebssysteme Baselines veröffentlicht, darunter Windows 10 Anniversary Update und Windows Server 2016.


### <a name="install-and-manage-antimalware"></a>Installieren und Verwalten von Antischadsoftware

Für Umgebungen, die getrennt von Ihrer Produktionsumgebung gehostet werden, können Sie eine Antischadsoftware-Erweiterung verwenden, um den Schutz Ihrer virtuellen Computer und Clouddienste zu verbessern. Diese wird in [Azure Security Center](../security-center/security-center-intro.md) integriert.


[Microsoft Antimalware](azure-security-antimalware.md) umfasst Features wie Echtzeitschutz, geplante Überprüfungen, Malwareproblembehandlung, Signaturupdates, Modulupdates, Beispielberichte, Sammlungen von Ausschlussereignissen und [PowerShell-Unterstützung](https://msdn.microsoft.com/library/dn771715.aspx).

![Azure Antimalware](./media/azure-security-iaas/azantimalware.png)

### <a name="install-the-latest-security-updates"></a>Installieren der neuesten Sicherheitsupdates
Zu den Workloads, die von Kunden als erste in Azure verschoben werden, zählen unter anderem Labs und Systeme mit externer Verbindung. Wenn Ihre in Azure gehosteten virtuellen Computer Anwendungen oder Dienste hosten, die über das Internet zugänglich sein sollen, müssen Sie beim Patchen aufmerksam sein. Beschränken Sie sich beim Patchen nicht nur auf das Betriebssystem. Ungepatchte Sicherheitsrisiken in Drittanbieteranwendungen können ebenfalls zu Problemen führen, die mit einer guten Patchverwaltung vermeidbar sind.

### <a name="deploy-and-test-a-backup-solution"></a>Bereitstellen und Testen einer Sicherungslösung

Sicherungen müssen ebenso wie Sicherheitsupdates auf die gleiche Weise behandelt werden wie alle anderen Vorgänge. Dies gilt für alle Systeme in Ihrer Produktionsumgebung, die sich bis in die Cloud erstreckt. Für Test- und Entwicklungssysteme müssen Sicherungsstrategien mit Wiederherstellungsfunktionen verwendet werden, die sich an den Erfahrungen orientieren, die Benutzer bereits mit lokalen Umgebungen gemacht haben.

In Azure verschobene Workloads sollten sich möglichst in die vorhandenen Sicherheitslösungen integrieren lassen. Alternativ können Sie sich bei der Erfüllung ihrer Sicherungsanforderungen von [Azure Backup](../backup/backup-azure-arm-vms.md) unterstützen lassen.


## <a name="monitor"></a>Überwachen

[Security Center](../security-center/security-center-intro.md) wertet ständig den Sicherheitsstatus Ihrer Azure-Ressourcen aus, um potenzielle Sicherheitsrisiken zu erkennen. Anhand einer Liste mit Empfehlungen werden Sie durch den Prozess des Konfigurierens der erforderlichen Sicherheitsmechanismen geführt.

Beispiele:

- Bereitstellen von Antischadsoftware, um Schadsoftware zu erkennen und zu entfernen
- Konfigurieren von Netzwerksicherheitsgruppen und Regeln, um den Datenverkehr für virtuelle Computer zu steuern
- Bereitstellen von Web Application Firewalls zur Abwehr von Angriffen auf Ihre Webanwendungen
- Bereitstellen fehlender Systemupdates
- Behandeln von Betriebssystemkonfigurationen, die nicht den empfohlenen Baselines entsprechen

Die folgende Abbildung zeigt einige der Optionen, die Sie in Security Center aktivieren können:

![Azure Security Center-Richtlinien](./media/azure-security-iaas/security-center-policies.png)

[Operations Management Suite](../operations-management-suite/operations-management-suite-overview.md) ist eine cloudbasierte IT-Verwaltungslösung von Microsoft, die das Verwalten und Schützen Ihrer lokalen und cloudbasierten Infrastruktur erleichtert. Da Operations Management Suite als cloudbasierter Dienst implementiert wird, kann die Lösung schnell und mit minimalen Investitionen in Infrastrukturressourcen bereitgestellt werden.

Neue Features werden automatisch bereitgestellt, sodass Sie laufende Wartungs- und Upgradekosten sparen. Operations Management Suite lässt sich auch mit System Center Operations Manager kombinieren. Es besteht aus verschiedenen Komponenten, die Sie bei der besseren Verwaltung Ihrer Azure-Workloads unterstützen. Zu diesen Komponenten zählt auch ein [Sicherheits- und Compliancemodul](../operations-management-suite/oms-security-getting-started.md).

Mithilfe der Sicherheits- und Compliancefeatures von Operations Management Suite können Sie Informationen zu Ihren Ressourcen anzeigen. Die Informationen sind in vier Hauptkategorien unterteilt:

- **Sicherheitsdomänen:** Mit dieser Option können Sie Sicherheitsdatensätze über einen längeren Zeitraum untersuchen. Außerdem können Sie auf die Bewertung von Malware, auf die Updatebewertung, auf Informationen zur Netzwerksicherheit, auf Informationen zu Identität und Zugriff sowie auf Computer mit Sicherheitsereignissen zugreifen. Darüber hinaus können Sie schnell auf das Azure Security Center-Dashboard zugreifen.
- **Relevante Probleme:** Mit dieser Option können Sie schnell die Anzahl aktiver Probleme und den jeweiligen Schweregrad ermitteln.
- **Ermittlungen (Vorschau):** Mit dieser Option können Sie Angriffsmuster ermitteln, indem Sie auftretende Sicherheitswarnungen für Ihre Ressourcen visualisieren.
- **Informationen zu Bedrohungen:** Mit dieser Option können Sie Angriffsmuster ermitteln, indem Sie die Gesamtzahl von Servern mit ausgehendem schädlichem IP-Datenverkehr, den Bedrohungstyp und eine Karte mit den Ursprungsorten dieser IP-Adressen visualisieren.
- **Allgemeine Sicherheitsabfragen:** Diese Option liefert eine Liste mit den gängigsten Sicherheitsabfragen, die Sie zum Überwachen der Umgebung verwenden können. Wenn Sie auf eine dieser Abfragen klicken, wird das Blatt **Suche** mit den Ergebnissen für die entsprechende Abfrage geöffnet.

Der folgende Screenshot zeigt ein Beispiel der Informationen, die in Operations Management Suite angezeigt werden können:

![Sicherheitsbaselines für Operations Management Suite](./media/azure-security-iaas/oms-security-baseline.png)

## <a name="next-steps"></a>Nächste Schritte

* [Blog des Azure-Sicherheitsteams](https://blogs.msdn.microsoft.com/azuresecurity/)
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)
* [Sicherheit in Azure – bewährte Methoden und Muster](security-best-practices-and-patterns.md)

