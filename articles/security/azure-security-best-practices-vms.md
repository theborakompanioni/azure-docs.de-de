---
title: "Bewährte Methoden für die Sicherheit virtueller Computer in Azure | Microsoft-Dokumentation"
description: "Dieser Artikel enthält eine Auflistung von bewährten Sicherheitsmethoden, die in den virtuellen Computern in Azure verwendet werden."
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
ms.sourcegitcommit: cea53acc33347b9e6178645f225770936788f807
ms.openlocfilehash: d6c5ea3e44b6121377d7d51f2bb9c878f9f933c5
ms.lasthandoff: 03/03/2017


---
# <a name="azure-virtual-machine-security-best-practices"></a>Bewährte Methoden für die Sicherheit virtueller Computer in Azure

In den meisten IaaS-Szenarios (Infrastruktur als Dienst) sind [Virtuelle Computer](https://docs.microsoft.com/en-us/azure/virtual-machines/) die Hauptworkload für Organisationen, die Cloud Computing verwenden. Dies ist in [hybrid scenarios (Hybridszenarios)](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) vorherrschend, in denen Organisationen die Workloads langsam in die Cloud migrieren möchten. Befolgen Sie das Szenario [general security considerations for IaaS (allgemeinen Sicherheitshinweise für IaaS)](https://social.technet.microsoft.com/wiki/contents/articles/3808.security-considerations-for-infrastructure-as-a-service-iaas.aspx) und stellen Sie sicher, dass Sie die bewährten Sicherheitsmethoden auf alle Ihre VMs in Azure angewendet haben.

In diesem Artikel werden die bewährten Methoden für die Azure-VMs beschrieben. Diese empfohlenen Vorgehensweisen sind aus unseren Erfahrungen mit Azure-VMs und den Erfahrungen von Kunden wie Ihnen abgeleitet. 

Für jede empfohlene Vorgehensweise erklären wir:

- Wobei es bei der bewährten Methode geht
- Warum Sie die bewährte Methode nutzen sollten
- Was die Folge sein könnte, wenn Sie die bewährte Methode nicht aktivieren
- Mögliche Alternativen zur bewährten Methode
- Wie Sie erfahren können, wie Sie die empfohlenen Vorgehensweisen aktivieren

Dieser Artikel zu den bewährten Methoden für die virtuellen Azure-Computer basiert auf einer Konsensmeinung und den Fähigkeiten und Funktionssätzen der Azure-Plattform, wie sie zum Erstellungszeitpunkt dieses Artikels existierten. Meinungen und Technologien ändern sich im Laufe der Zeit. Dieser Artikel wird daher regelmäßig aktualisiert, um diese Änderungen widerzuspiegeln.

Bewährte Methoden zu Azure-VMs, die in diesem Artikel beschrieben werden:

- Authentifizierung für virtuelle Computer und Access Control
- Verfügbarkeit für virtuelle Computer und Netzwerkzugriff
- Schützen von ruhenden Daten in Azure-VMs durch Erzwingen der Verschlüsselung
- Verwalten von Aktualisierungen für virtuelle Computer
- Verwalten des Sicherheitsstatus Ihres virtuellen Computers
- Überwachen der Leistung des virtuellen Computers

## <a name="virtual-machine-authentication-and-access-control"></a>Authentifizierung für virtuelle Computer und Access Control

Der erste Schritt zum Schutz Ihres virtuellen Computers ist es, sicherzustellen, dass nur autorisierte Benutzer neue VMs bereitstellen können. Sie können [Ressourcen-Manager-Richtlinien](../azure-resource-manager/resource-manager-policy.md) verwenden, um Konventionen für Ressourcen in Ihrer Organisation einzurichten, benutzerdefinierte Richtlinien zu erstellen und diese Richtlinien auf Ressourcen, wie z.B. [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) anzuwenden. Die VMs, die zu dieser Ressourcengruppe gehören, erben diese Richtlinien. Obgleich dies die empfohlene Vorgehensweise zum Verwalten von Ressourcen ist (einschließlich VMs), die über unterschiedliche Anforderungen verfügen, und sich in verschiedenen Ressourcengruppen befinden, können Sie auch den einzelnen Zugriff auf virtuelle Computer steuern, indem Sie die [rollenbasierte Zugriffssteuerung (RBAC)](../active-directory/role-based-access-control-configure.md) verwenden.

Durch Aktivieren von Azure Resource Manager-Richtlinien und RBAC zum Steuern des VM-Zugriffs, erweitern Sie die allgemeine Sicherheit Ihres VM. Wir empfehlen Ihnen, eng miteinander verknüpfte VMs mit demselben Lebenszyklus in derselben Ressourcengruppe abzulegen. Ressourcengruppen ermöglichen Ihnen das Bereitstellen und Überwachen von Ressourcen und das Zusammenfassen von Abrechnungskosten nach Ressourcengruppe. Verwenden Sie den Ansatz der [geringsten Rechte](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models) zum Aktivieren des Zugriffs für Benutzer für die Bereitstellung von VMs, und verwenden Sie die folgenden in Azure integrierten Rollen, wenn Sie Benutzern Berechtigungen zuweisen möchten:

- [Mitwirkender von virtuellen Computern](../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor): kann virtuelle Computer verwalten, jedoch nicht das virtuelle Netzwerk, mit dem sie verbunden sind.
- [Mitwirkender von klassischen virtuellen Computern](../active-directory/role-based-access-built-in-roles.md#classic-virtual-machine-contributor): kann klassische virtuelle Computer verwalten, jedoch nicht das virtuelle Netzwerk oder Speicherkonto, mit dem sie verbunden sind.
- [Sicherheits-Manager](../active-directory/role-based-access-built-in-roles.md#security-manager): kann Sicherheitskomponenten, Sicherheitsrichtlinien und virtuelle Maschinen verwalten.
- [DevTest Labs-Benutzer](../active-directory/role-based-access-built-in-roles.md#devtest-labs-user): kann alles anzeigen sowie virtuelle Maschinen verbinden, starten, neu starten und herunterfahren.

Vermeiden Sie es, Konten und Kennwörter für mehrere Administratoren freizugeben oder Kennwörter für mehrere Benutzerkonten und Dienste zu verwenden. Dies gilt besonders für soziale Medien oder andere nicht administrative Aktivitäten. Im Idealfall sollten Sie [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)-Vorlagen verwenden, um die Bereitstellung Ihrer VMs zu sichern. Mithilfe dieses Ansatzes können Sie die Bereitstellungsoptionen erhöhen und Sicherheitseinstellungen in der Bereitstellung erzwingen.

Organisationen, die keine Datenzugriffssteuerung mithilfe von Funktionen wie RBAC erzwingen, erteilen Ihren Benutzern möglicherweise mehr Berechtigungen als erforderlich. Dies kann direkt zur Kompromittierung der Daten führen, da Sie Zugriff auf Daten erhalten, auf die Sie von vorneherein keinen Zugriff haben sollten.
 

## <a name="virtual-machine-availability-and-network-access"></a>Verfügbarkeit für virtuelle Computer und Netzwerkzugriff

Wenn Ihre VMs kritische Anwendungen ausführen, die eine hohe Verfügbarkeit erfordern, wird empfohlen, dass Sie mehrere virtuelle Computer verwenden.  Erstellen Sie für eine höhere Verfügbarkeit mindestens zwei VMs in der [Verfügbarkeitsgruppe](../virtual-machines/virtual-machines-windows-infrastructure-availability-sets-guidelines.md). Azure [Load Balancer](../load-balancer/load-balancer-overview.md) erfordert auch, dass VMs mit Lastenausgleich derselben Verfügbarkeitsgruppe angehören. Wenn diese VMs Zugriff über das Internet haben müssen, müssen Sie einen [Load Balancer mit Internetzugriff](../load-balancer/load-balancer-internet-overview.md) konfigurieren.

Bei VMs, die im Internet verfügbar sind, ist es wichtig, sicherzustellen, dass Sie [den Netzwerkdatenverkehr mit Netzwerksicherheitsgruppen steuern](../virtual-network/virtual-networks-nsg.md).  Da NSGs auf Subnetze angewendet werden können, können Sie die Anzahl der NSGs minimieren, indem Sie Ihre Ressourcen nach Subnetz gruppieren und die NSGs auf Subnetze anwenden. Das Internet wird eine Ebene der Netzwerkisolation erstellen, die durch die richtige Konfiguration von [Netzwerksicherheits](../best-practices-network-security.md)-Funktionen in Azure erreicht werden kann.  

Sie können auch die Just-in-Time-VM-Zugriffsfunktion von Azure Security Center verwenden, um zu steuern, wer und für wie lange er Remotezugriff auf einen bestimmten virtuellen Computer haben kann. Sehen Sie sich das Video unten für Weitere Informationen zur Verwendung dieser Funktion an:


<iframe src="https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Security-Center-Just-in-Time-VM-Access/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

Organisationen, die keine Netzwerkzugriffsbeschränkung auf VMs mit Internetzugriff erzwingen, werden Sicherheitsrisiken, z.B. RDP Brute-Force-Angriff ausgesetzt. 

## <a name="protect-data-at-rest-in-azure-vms-by-enforcing-encryption"></a>Schützen von ruhenden Daten in Azure-VMs durch Erzwingen der Verschlüsselung

Heutzutage ist die [Verschlüsselung von ruhenden Daten](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) ein obligatorischer Schritt in Richtung Datenschutz, Konformität und Datenhoheit. [Azure Disk Encryption](../security/azure-security-disk-encryption.md) ermöglicht IT-Administratoren, die Datenträger virtueller Windows- und Linux-IaaS-Computer (VMs) zu verschlüsseln. Bei der Azure Disk Encryption werden die Branchenstandardfunktion BitLocker von Windows und die Funktion DM-Crypt von Linux verwendet, um Volumeverschlüsselung für das Betriebssystem und die Datenträger bereitzustellen.

Sie können Azure Disk Encryption verwenden, um den Schutz Ihrer Daten zu verbessern und damit die Sicherheits- und Compliance-Anforderungen Ihrer Organisation zu erfüllen. Organisationen sollten auch die Verwendung von Verschlüsselungen in Betracht ziehen, um das Risiko des unberechtigten Datenzugriffs zu reduzieren. Es empfiehlt sich auch, Laufwerke zu verschlüsseln, bevor sie mit sensiblen Daten beschrieben werden. 

Stellen Sie sicher, dass Sie das Datenvolumen Ihrer VM verschlüsseln, um die ruhenden Daten in Ihrem Azure-Speicherkonto zu schützen. Schützen Sie die Verschlüsselungsschlüssel und geheimen Schlüssel durch die Verwendung von [Azure Key Vault](https://azure.microsoft.com/en-us/documentation/articles/key-vault-whatis/). 

Organisationen, die keine Datenverschlüsselung erzwingen, sehen sich mehr Datenintegritätsproblemen gegenüber, beispielsweise, dass böswillige oder nicht autorisierte Benutzer Daten stehlen und dass kompromittierte Konten unautorisierte Zugriffe auf unverschlüsselte Daten erhalten. Neben den Risiken müssen Unternehmen den Branchenbestimmungen entsprechen und beweisen, dass sie gewissenhaft sind und die richtigen Sicherheitsprotokolle verwenden, um die Datensicherheit zu verbessern.

Erfahren Sie mehr über die Azure Disk Encryption, indem Sie den Artikel [Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer](azure-security-disk-encryption.md) lesen.


## <a name="manage-virtual-machine-updates"></a>Verwalten von Aktualisierungen für virtuelle Computer

Azure bewirkt nicht, dass Windows-Updates auf virtuelle Microsoft Azure-Computer angewendet werden, da diese Computer durch den Benutzer verwaltet werden sollen. Dies ist genau bei wie alle lokalen Computern. Kunden wird jedoch empfohlen, die automatische Windows Update-Einstellung aktiviert zu lassen. Eine andere Möglichkeit besteht darin, einen [Windows Server Update Services (WSUS)](https://technet.microsoft.com/windowsserver/bb332157.aspx)-Server oder ein anderes geeignetes Update Verwaltungsprodukt entweder auf einem anderen virtuellen Azure-Computer oder in lokalen bereitzustellen. WSUS und Windows-Update halten VMs auf dem neuesten Stand. Außerdem ist es ratsam, ein Scan-Produkt zu verwenden, um zu überprüfen, ob alle IaaS-VMs auf dem neuesten Stand sind.

Von Azure bereitgestellte vorhandene Images werden regelmäßig aktualisiert, um die neueste Runde von Windows-Updates einzuschließen. Es gibt jedoch keine Garantie, dass die Images zum Zeitpunkt der Bereitstellung aktuell sein werden. Es ist möglich, dass eine geringfügige Verzögerung (nicht mehr als ein paar Wochen) aus öffentlichen Versionen vorliegen kann. Das Suchen und Installieren aller Windows-Updates muss der erste Schritt für jede Bereitstellung sein. Dies ist beim Bereitstellen von Images, die Sie bereitstellen oder von Ihrer eigenen Bibliothek verwenden, besonders wichtig. Images, die als Bestandteil des Katalogs von Windows Azure bereitgestellt werden, haben standardmäßig automatische Windows Updates aktiviert.

Organisationen, die keine Richtlinien zu Softwareaktualisierungen erzwingen, sind mehr Bedrohungen ausgesetzt, die bekannte Sicherheitsrisiken ausnutzen, die bereits behoben wurden. Neben den Risiken müssen Unternehmen den Branchenbestimmungen entsprechen und beweisen, dass sie gewissenhaft sind und die richtigen Sicherheitsprotokolle verwenden, um die Sicherheit von deren Workload in der Cloud zu verbessern.
Hervorzuheben ist, dass es viele Ähnlichkeiten zwischen den bewährten Methoden für Softwareupdates in einem traditionellen Datencenter und in Azure IaaS gibt. Deshalb wird empfohlen, dass Sie Azure-VMs in Ihre aktuellen Richtlinien für Softwareupdates einbeziehen.

## <a name="manage-virtual-machine-secure-posture"></a>Verwalten des Sicherheitsstatus des virtuellen Computers

Cyberbedrohungen werden entwickelt, und das Schützen Ihrer VMs erfordert eine umfangreichere Überwachungsfunktion, die schnell Bedrohungen erkennen, Warnungen auslösen und falsch positive Werte verringern kann. Der Sicherheitsstatus für diese Art der Workload umfasst alle Sicherheitsaspekte des virtuellen Computers, von der Updateverwaltung bis zum sicheren Netzwerkzugriff, während der Überwachung von Bedrohungen, die versuchen, nicht autorisierten Zugriff auf Ihre Ressourcen zu erhalten.

Sie können [Azure Security Center](../security-center/security-center-intro.md) zum Überwachen des Sicherheitsstatus in [Windows](../security-center/security-center-virtual-machine.md) und [Linux-VMs](../security-center/security-center-linux-virtual-machine.md) verwenden. Sie können die folgenden Funktionen im Azure Security Center zum Überwachen Ihrer VMs nutzen:

- Sicherheitseinstellungen des Betriebssystems mit den empfohlenen Konfigurationsregeln
- Systemsicherheit und fehlende kritische Updates
- Empfehlungen zur Endpoint Protection (Antimalware)
- Überprüfung der Datenträgerverschlüsselung
- Sicherheitsrisikobewertung und -lösung
- Bedrohungserkennung

Security Center kann Bedrohungen aktiv überwachen, und diese Bedrohungen unter Sicherheitswarnungen stellen. In einer einzigen Ansicht namens *Sicherheitsvorfall* werden Bedrohungen aggregiert, die korreliert sind. Sie können sich das untenstehende Video ansehen, um zu verstehen, wie Security Center Ihnen beim Identifizieren potenzieller Bedrohungen in Ihren VMs in Azure helfen kann.

<iframe src="https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Security-Center-in-Incident-Response/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

Organisationen, die keinen Sicherheitsstatus für ihre VMs erzwingen, haben keine Kenntnis von potenziellen Versuchen, die Sicherheitskontrollen zu umgehen, die vorhanden sind.

## <a name="monitoring-virtual-machine-performance"></a>Überwachen der Leistung des virtuellen Computers

Der Missbrauch von Ressourcen kann auch ein Problem sein, wenn Sie Prozesse in einem VM haben, die mehr Ressourcen beanspruchen als sie sollten. Ein virtueller Computer mit einem Leistungsproblem kann zu einer Dienstunterbrechung führen, der gegen eine der Sicherheitsprinzipale verstößt: Verfügbarkeit. Aus diesem Grund ist es zwingend erforderlich den VM-Zugriff zu überwachen, nicht nur reaktiv (während ein Problem stattfindet), sondern auch durch das Durchführen eines Grundwerts während der normalen Betriebsstunden.

[Azure-Diagnoseprotokolle](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) kann Sie bei der Überwachung der Ressourcen Ihres virtuellen Computers unterstützen und potenzielle Probleme, die die Leistung und Verfügbarkeit beeinträchtigen, identifizieren. Die Azure-Diagnoseerweiterung stellt Überwachungs- und Diagnosefunktionen auf einem Windows-basierten virtuellen Azure-Computer bereit. Sie können diese Funktionen auf dem virtuellen Computer nutzen, indem Sie die Erweiterung in die Azure-Ressourcen-Manager-[Vorlage](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md) einbinden. Sie können auch [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-metrics.md) verwenden, um einen Einblick in den Zustand Ihrer Ressourcen zu erhalten.

Organisationen, die die Leistung des virtuellen Computers nicht überwachen, ist es nicht möglich zu bestimmen, ob bestimmte Änderungen in Leistungsmustern Teil der normalen Nutzung sind oder ob ein nicht ordnungsgemäßer Vorgang stattfindet, der mehr Ressourcen als normal verwendet. Die Anomalie deutet auf einen potenziellen Angriff aus einer externen Ressource oder auf einen gefährdeten Prozess auf diesem virtuellen Computer hin. 
