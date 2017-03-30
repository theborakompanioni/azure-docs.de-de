---
title: "Bewährte Methoden für die Sicherheit virtueller Azure-Computer | Microsoft-Dokumentation"
description: "Dieser Artikel enthält eine Reihe bewährter Sicherheitsmethoden für virtuelle Computer in Azure."
services: security
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 5e757abe-16f6-41d5-b1be-e647100036d8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/02/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 1d010dd85ccf2dd708a7740eb8399fc06a603574
ms.lasthandoff: 03/22/2017


---
# <a name="best-practices-for-azure-vm-security"></a>Bewährte Methoden für die Sicherheit virtueller Azure-Computer

In den meisten IaaS-Szenarien (Infrastructure-as-a-Service, Infrastruktur als Dienst) stellen [virtuelle Azure-Computer](https://docs.microsoft.com/en-us/azure/virtual-machines/) (Virtual Machines, VMs) die Hauptworkload für Organisationen dar, die Cloud Computing verwenden. Dies gilt besonders in [Hybridszenarien](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx), in denen Organisationen Workloads nach und nach in die Cloud migrieren möchten. Orientieren Sie sich in solchen Szenarien an den [allgemeinen Sicherheitsaspekten für IaaS](https://social.technet.microsoft.com/wiki/contents/articles/3808.security-considerations-for-infrastructure-as-a-service-iaas.aspx), und wenden Sie bewährte Sicherheitsmethoden für alle Ihre virtuellen Computer an.

In diesem Artikel werden verschiedene bewährte Sicherheitsmethoden für virtuelle Computer besprochen, die sich jeweils direkt von den Erfahrungen unserer Kunden sowie von unseren eigenen Erfahrungen mit virtuellen Computern ableiten.

Die bewährten Methoden basieren auf einer gemeinsamen Linie und eignen sich für aktuelle Funktionen und Features der Azure-Plattform. Da sich Meinungen und Technologien im Laufe der Zeit verändern können, wird dieser Artikel voraussichtlich regelmäßig aktualisiert, um diesen Veränderungen Rechnung zu tragen.

In diesem Artikel wird für jede bewährte Methode Folgendes erläutert:

* Wobei es bei der bewährten Methode geht
* Warum es ratsam ist, sie anzuwenden
* Wie Sie sie anwenden können
* Was passieren kann, wenn Sie sie nicht anwenden
* Mögliche Alternativen zu der bewährten Methode

In dem Artikel werden folgende bewährten Sicherheitsmethoden für virtuelle Computer behandelt:

* Authentifizierung und Zugriffssteuerung für virtuelle Computer
* Verfügbarkeit und Netzwerkzugriff für virtuelle Computer
* Schutz von ruhenden Daten in virtuellen Computern durch Erzwingen der Verschlüsselung
* Verwalten Ihrer Updates für virtuelle Computer
* Verwalten des Sicherheitsstatus Ihrer virtuellen Computer
* Überwachen der Leistung virtueller Computer

## <a name="vm-authentication-and-access-control"></a>Authentifizierung und Zugriffssteuerung für virtuelle Computer

Der erste Schritt zum Schutz Ihres virtuellen Computers ist es, sicherzustellen, dass nur autorisierte Benutzer neue VMs bereitstellen können. Mithilfe von [Azure Resource Manager-Richtlinien](../azure-resource-manager/resource-manager-policy.md) können Sie Konventionen für Ressourcen in Ihrer Organisation einrichten, benutzerdefinierte Richtlinien erstellen und diese Richtlinien auf Ressourcen (beispielsweise [Ressourcengruppen](../azure-resource-manager/resource-group-overview.md)) anwenden.

Virtuelle Computer, die einer Ressourcengruppe angehören, erben natürlich deren Richtlinien. Diese Herangehensweise wird zwar für die Verwaltung virtueller Computer empfohlen, Sie können den Zugriff auf einzelne VM-Richtlinien jedoch auch mithilfe der [rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC)](../active-directory/role-based-access-control-configure.md) steuern.

Wenn Sie Resource Manager-Richtlinien und RBAC zum Steuern des Zugriffs auf virtuelle Computer aktivieren, verbessern Sie dadurch insgesamt die Sicherheit von virtuellen Computern. Es empfiehlt sich, virtuelle Computer mit gleichem Lebenszyklus in der gleichen Ressourcengruppe zusammenzufassen. Ressourcengruppen ermöglichen die Bereitstellung und Überwachung Ihrer Ressourcen sowie die Zusammenfassung der Abrechnungskosten für Ihre Ressourcen. Verwenden Sie den [Ansatz der geringsten Rechte](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models), wenn Sie Benutzern Zugriff auf virtuelle Computer gewähren und ihnen die Einrichtung virtueller Computer ermöglichen. Planen Sie außerdem die Verwendung der folgenden integrierten Azure-Rollen, wenn Sie den Benutzern Berechtigungen zuweisen:

- [Mitwirkender für virtuelle Computer:](../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor) Kann virtuelle Computer verwalten, aber nicht das virtuelle Netzwerk oder Speicherkonto, mit dem sie verbunden sind.
- [Mitwirkender für klassische virtuelle Computer:](../active-directory/role-based-access-built-in-roles.md#classic-virtual-machine-contributor) Kann virtuelle Computer verwalten, die mit dem klassischen Bereitstellungsmodell erstellt wurden, aber nicht das virtuelle Netzwerk oder Speicherkonto, mit dem sie verbunden sind.
- [Sicherheits-Manager:](../active-directory/role-based-access-built-in-roles.md#security-manager) Kann Sicherheitskomponenten, Sicherheitsrichtlinien und virtuelle Computer verwalten.
- [DevTest Labs-Benutzer:](../active-directory/role-based-access-built-in-roles.md#devtest-labs-user) Kann alles anzeigen sowie virtuelle Computer verbinden, starten, neu starten und herunterfahren.

Administratoren dürfen Konten und Kennwörter nicht gemeinsam verwenden, und Kennwörter dürfen nicht für mehrere Benutzerkonten oder Dienste verwendet werden. Das gilt insbesondere für Kennwörter für soziale Medien oder andere nicht administrative Aktivitäten. Zur sicheren Einrichtung Ihrer virtuellen Computer sollten Sie im Idealfall [Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md) verwenden. Mit diesem Ansatz können Sie die Sicherheit Ihrer Bereitstellungsoptionen verbessern und Sicherheitseinstellungen in der gesamten Bereitstellung erzwingen.

Organisationen, die keine Datenzugriffssteuerung mit Funktionen wie RBAC erzwingen, gewähren ihren Benutzern unter Umständen mehr Berechtigungen als erforderlich. Bei nicht angemessenem Benutzerzugriff auf bestimmte Daten besteht die Gefahr einer direkten Kompromittierung dieser Daten.

## <a name="vm-availability-and-network-access"></a>Verfügbarkeit und Netzwerkzugriff für virtuelle Computer

Falls Ihr virtueller Computer kritische Anwendungen ausführt, die eine hohe Verfügbarkeit erfordern, empfiehlt sich die Verwendung mehrerer virtueller Computer. Erstellen Sie für eine höhere Verfügbarkeit mindestens zwei VMs in der [Verfügbarkeitsgruppe](../virtual-machines/virtual-machines-windows-infrastructure-availability-sets-guidelines.md).

[Azure Load Balancer](../load-balancer/load-balancer-overview.md) setzt außerdem voraus, dass virtuelle Computer mit Lastenausgleich der gleichen Verfügbarkeitsgruppe angehören. Falls auf diese virtuellen Computer über das Internet zugegriffen werden muss, müssen Sie einen [Load Balancer mit Internetzugriff](../load-balancer/load-balancer-internet-overview.md) konfigurieren.

Bei virtuellen Computern, die über das Internet verfügbar gemacht werden, muss der [Netzwerkdatenverkehr mit Netzwerksicherheitsgruppen (NSGs) gesteuert](../virtual-network/virtual-networks-nsg.md) werden. Da NSGs auf Subnetze angewendet werden können, können Sie die Anzahl von NSGs verringern, indem Sie Ihre Ressourcen nach Subnetz gruppieren und die NSGs anschließend auf die Subnetze anwenden. Das Ziel besteht in der Schaffung einer Netzwerkisolationsebene durch ordnungsgemäße Konfiguration der [Netzwerksicherheitsfunktionen](../best-practices-network-security.md) in Azure.

Sie können auch das Just-in-Time-Zugriffsfeature für virtuelle Computer aus dem Azure Security Center verwenden, um zu steuern, wer wie lange Remotezugriff auf einen bestimmten virtuellen Computer haben soll.

Organisationen, die keine Netzwerkzugriffsbeschränkung für virtuelle Computer mit Internetzugriff erzwingen, setzen sich Sicherheitsrisiken aus. Hierzu zählen beispielsweise RDP-Brute-Force-Angriffe.

## <a name="protect-data-at-rest-in-your-vms-by-enforcing-encryption"></a>Schutz von ruhenden Daten in virtuellen Computern durch Erzwingen der Verschlüsselung

Die [Verschlüsselung ruhender Daten](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) ist eine obligatorische Maßnahme für Datenschutz, Compliance und Datenhoheit. Mit [Azure Disk Encryption](../security/azure-security-disk-encryption.md) können IT-Administratoren die Datenträger virtueller IaaS-Computer unter Windows oder Linux verschlüsseln. Disk Encryption kombiniert die Branchenstandardfeatures BitLocker (Windows) und dm-crypt (Linux), um Volumeverschlüsselung für das Betriebssystem und die Datenträger bereitzustellen.

Mit Disk Encryption können Sie den Schutz Ihrer Daten verbessern, um die Sicherheits- und Complianceanforderungen Ihrer Organisation zu erfüllen. Die Verwendung der Verschlüsselung empfiehlt sich für Organisationen zudem, um das Risiko unberechtigter Datenzugriffe zu reduzieren. Des Weiteren empfehlen wir eine Verschlüsselung Ihrer Laufwerke, bevor diese mit sensiblen Daten beschrieben werden.

Verschlüsseln Sie die Datenvolumen Ihrer virtuellen Computer, um die ruhenden Daten in Ihrem Azure-Speicherkonto zu schützen. Schützen Sie die Verschlüsselungsschlüssel und das Geheimnis mithilfe von [Azure Key Vault](https://azure.microsoft.com/en-us/documentation/articles/key-vault-whatis/).

Organisationen, die keine Datenverschlüsselung erzwingen, sind anfälliger für Datenintegritätsprobleme. So können beispielsweise nicht autorisierte Benutzer Daten aus kompromittierten Konten entwenden oder sich unberechtigt Zugang zu unverschlüsselten Daten verschaffen. Unternehmen setzen sich jedoch nicht nur diesen Risiken aus, sondern müssen zur Erfüllung von Branchenbestimmungen auch nachweisen, dass sie gewissenhaft mit Daten umgehen und geeignete Sicherheitsmaßnahmen ergreifen, um die Datensicherheit zu verbessern.

Weitere Informationen zu Disk Encryption finden Sie unter [Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer](azure-security-disk-encryption.md).


## <a name="manage-your-vm-updates"></a>Verwalten Ihrer Updates für virtuelle Computer

Da bei virtuellen Azure-Computern (wie bei allen lokalen virtuellen Computern) eine Verwaltung durch die Benutzer vorgesehen ist, werden Windows-Updates von Azure nicht mittels Push an die virtuellen Computer übertragen. Es empfiehlt sich jedoch, die Einstellung für automatische Windows-Updates aktiviert zu lassen. Alternativ können Sie [Windows Server Update Services (WSUS)](https://technet.microsoft.com/windowsserver/bb332157.aspx) oder ein anderes geeignetes Updateverwaltungsprodukt bereitstellen (entweder auf einem anderen virtuellen Computer oder lokal). Sowohl WSUS als auch Windows Update halten virtuelle Computer auf dem neuesten Stand. Außerdem sollten Sie sich mithilfe eines Überprüfungsprodukts vergewissern, dass alle Ihre virtuellen IaaS-Computer auf dem neuesten Stand sind.

Vorhandene Images, die von Azure bereitgestellt werden, werden regelmäßig mit den neuesten Windows-Updates aktualisiert. Es gibt jedoch keine Garantie, dass die Images zum Zeitpunkt der Bereitstellung auf dem neuesten Stand sind. Im Vergleich zu öffentlichen Versionen ist eine geringfügige Verzögerung von wenigen Wochen möglich. Bei jeder Bereitstellung sollten als erstes alle Windows-Updates gesucht und installiert werden. Dies ist besonders wichtig, wenn Sie Images bereitstellen, die von Ihnen selbst oder aus Ihrer eigenen Bibliothek stammen. Über den Azure Marketplace bereitgestellte Images werden standardmäßig automatisch aktualisiert.

Organisationen, die keine Softwareupdaterichtlinien erzwingen, sind anfälliger für Angreifer, die sich bekannte, bereits korrigierte Sicherheitslücken zunutze machen. Unternehmen setzen sich jedoch nicht nur diesen Bedrohungen aus, sondern müssen zur Erfüllung von Branchenbestimmungen auch nachweisen, dass sie gewissenhaft arbeiten und geeignete Sicherheitsmaßnahmen ergreifen, um die Sicherheit ihrer Workload in der Cloud zu gewährleisten.

Wichtig: Die bewährten Softwareupdatemethoden für herkömmliche Datencenter und Azure-IaaS sind sich in vielen Punkten ähnlich. Aus diesem Grund sollten Sie Ihre aktuellen Softwareupdaterichtlinien evaluieren und virtuelle Computer mit einbeziehen.

## <a name="manage-your-vm-security-posture"></a>Verwalten des Sicherheitsstatus Ihrer virtuellen Computer

Cyberbedrohungen entwickeln sich stetig weiter. Der Schutz Ihrer virtuellen Computer erfordert daher umfangreiche Überwachungsfunktionen, die Bedrohungen schnell erkennen, nicht autorisierte Zugriffe auf Ihre Ressourcen verhindern, Warnungen auslösen und falsch positive Ergebnisse verringern. Der Sicherheitsstatus für eine solche Workload umfasst sämtliche Sicherheitsaspekte des virtuellen Computers – von der Updateverwaltung bis hin zu sicherem Netzwerkzugriff.

Den Sicherheitsstatus Ihrer [virtuellen Windows-Computer](../security-center/security-center-virtual-machine.md) und [virtuellen Linux-Computer](../security-center/security-center-linux-virtual-machine.md) können Sie mithilfe von [Azure Security Center](../security-center/security-center-intro.md) überwachen. Zum Schutz Ihrer virtuellen Computer stehen Ihnen im Azure Security Center folgende Funktionen zur Verfügung:

* Anwenden von Sicherheitseinstellungen des Betriebssystems mit empfohlenen Konfigurationsregeln
* Ermitteln und Herunterladen sicherheitsrelevanter und wichtiger Updates, die möglicherweise noch fehlen
* Bereitstellen von Empfehlungen zum Schutz von Endpunkten vor Schadsoftware
* Überprüfen der Datenträgerverschlüsselung
* Bewerten und Beseitigen von Sicherheitsrisiken
* Erkennen von Bedrohungen

Security Center kann aktiv nach Bedrohungen suchen, und potenzielle Bedrohungen werden unter **Sicherheitswarnungen** angezeigt. Korrelierte Bedrohungen werden in einer zentralen Ansicht namens **Sicherheitsvorfall** aggregiert.

Im folgenden Video erfahren Sie, wie Security Center Sie beim Identifizieren potenzieller Bedrohungen für Ihre virtuellen Computer in Azure unterstützt:

<iframe src="https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Security-Center-in-Incident-Response/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

Organisationen, die für ihre virtuellen Computer kein hohes Maß an Sicherheit erzwingen, bleiben potenzielle Vorfälle, bei denen nicht autorisierte Benutzer versuchen, die eingerichteten Sicherheitskontrollen zu umgehen, verborgen.

## <a name="monitor-vm-performance"></a>Überwachen der Leistung virtueller Computer

Ressourcenmissbrauch kann problematisch sein, wenn Prozesse von virtuellen Computern mehr Ressourcen beanspruchen als sie sollten. Leistungsprobleme bei einem virtuellen Computer können zu einer Dienstunterbrechung führen und gegen das Sicherheitsprinzip der Verfügbarkeit verstoßen. Aus diesem Grund muss der Zugriff auf virtuelle Computer nicht nur reaktiv (also wenn bereits ein Problem auftritt), sondern auch proaktiv anhand einer im Normalbetrieb ermittelten Baseline überwacht werden.

Durch die Analyse der [Azure-Diagnoseprotokolldateien](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) können Sie die Ressourcen Ihrer virtuellen Computer überwachen und potenzielle Probleme erkennen, die unter Umständen die Leistung und Verfügbarkeit beeinträchtigen. Die Azure-Diagnoseerweiterung stellt Überwachungs- und Diagnosefunktionen auf virtuellen Windows-Computern bereit. Diese Funktionen können Sie aktivieren, indem Sie die Erweiterung in die [Azure Resource Manager-Vorlage](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md) einbeziehen.

Sie können auch [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-metrics.md) verwenden, um sich über den Zustand Ihrer Ressourcen zu informieren.

Organisationen, die die Leistung virtueller Computer nicht überwachen, können nicht ermitteln, ob bestimmte Veränderungen bei Leistungsmustern normal sind. Falls der virtuelle Computer mehr Ressourcen beansprucht als normal, kann dies auf einen potenziellen Angriff über eine externe Ressource oder auf die Ausführung eines kompromittierten Prozesses auf diesem virtuellen Computer hindeuten.

