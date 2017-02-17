---
title: "Unterstützung mehrerer Mandanten für die VMware-VM-Replikation in Azure (CSP-Programm) | Microsoft-Dokumentation"
description: "Beschreibt, wie Azure Site Recovery in einer Umgebung mit mehreren Mandanten bereitgestellt wird, um Replikation, Failover und Wiederherstellung von lokalen VMware-VMs in Azure über das CSP-Programm im Azure-Portal zu orchestrieren."
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: jwhit
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: manayar
translationtype: Human Translation
ms.sourcegitcommit: 3b606aa6dc3b84ed80cd3cc5452bbe1da6c79a8b
ms.openlocfilehash: ed484afc59bbf48490e3ff4389e8e28c71a5e471


---
# <a name="multi-tenant-support-in-azure-site-recovery-for-replicating-vmware-virtual-machines-to-azure-through-the-csp-program"></a>Unterstützung mehrerer Mandanten in Azure Site Recovery für die Replikation von VMware-VMs in Azure über das CSP-Programm

Azure Site Recovery unterstützt für Mandantenabonnements Umgebungen mit mehreren Mandanten. Mehrinstanzenfähigkeit wird auch für Mandantenabonnements unterstützt, die über das CSP-Programm erstellt und verwaltet werden. In diesem Artikel wird das Implementieren und Verwalten von VMware-zu-Azure-Szenarien mit mehreren Mandanten detailliert behandelt. Das Erstellen und Verwalten von Mandantenabonnements über CSP wird ebenfalls beschrieben.

Diese Anleitung bezieht sich umfassend auf die vorhandene Dokumentation zum Replizieren von VMware-VMs in Azure. Diese Anleitung sollte zusammen mit dieser [Dokumentation](site-recovery-vmware-to-azure.md) verwendet werden.

## <a name="multi-tenant-environments"></a>Umgebungen mit mehreren Mandanten
Es gibt drei wesentliche Modelle mit mehreren Mandanten:

1.    **Shared Hosting Services Provider (SHSP, Anbieter von gemeinsam genutzten Hostingdiensten)**: Hier besitzt der Partner die physische Infrastruktur und arbeitet mit gemeinsam genutzten Ressourcen (vCenter, Rechenzentren, physischem Speicher usw.), um die VMs mehrerer Mandanten in derselben Infrastruktur zu hosten. Die Verwaltung der Notfallwiederherstellung kann vom Partner als verwalteter Dienst bereitgestellt werden oder als Self-Service-Lösung in die Zuständigkeit des Mandanten fallen.
2.    **Dedicated Hosting Services Provider (DHSP, Anbieter dedizierter Hostingdienste)**: Hier besitzt der Partner die physische Infrastruktur, wobei jedoch dedizierte Ressourcen (mehrere vCenter, physische Datenspeicher usw.) verwendet werden, um die VMs der einzelnen Mandanten in getrennter Infrastruktur zu hosten. Die Verwaltung der Notfallwiederherstellung kann wiederum vom Partner oder als Self-Service-Lösung vom Mandanten verwaltet werden.
3.    **Managed Services Provider (MSP, Anbieter verwalteter Dienste)**: Hier besitzt der Kunde die physische Infrastruktur, in der die VMs gehostet werden, während die Partner für die Aktivierung und Verwaltung der Notfallwiederherstellung zuständig sind.

## <a name="shared-hosting-multi-tenant-guidance"></a>Anleitung für SHSP mit mehreren Mandanten
In dieser Anleitung wird das SHSP-Szenario ausführlich behandelt. Die anderen beiden Szenarien sind eine Teilmenge des SHSP-Szenarios und folgen denselben Prinzipien. Die Unterschiede werden am Ende der SHSP-Anleitung beschrieben.

Die grundlegende Anforderung in einem Szenario mit mehreren Mandanten ist die Isolation der verschiedenen Mandanten, sodass ein Mandant nicht feststellen kann, was ein anderer Mandant hostet. In einer vollständig vom Partner verwalteten Umgebung ist diese Anforderung nicht so wichtig wie für eine Self-Service-Umgebung, in der sie kritisch sein kann. In dieser Anleitung wird davon ausgegangen, dass die Mandantenisolation erforderlich ist.

Die Architektur sieht wie folgt aus:

![Architektur beim SHPS-Modell](./media/site-recovery-multi-tenant-support-vmware-using-csp/shared-hosting-scenario.png)

**Abbildung 1: SHSP-Szenario mit einem vCenter**

Wie Sie in dieser Abbildung erkennen können, hat jeder Kunde einen eigenen Verwaltungsserver. Dies erfolgt zum Beschränken des Mandantenzugriffs auf mandantenspezifische VMs zum Ermöglichen der Isolation von Mandanten. Im VMware-VM-Replikationsszenario wird der Konfigurationsserver zum Verwalten von Konten verwendet, um VMs zu ermitteln und Agents zu installieren. Wir befolgen dieselben Prinzipien für Umgebungen mit mehreren Mandanten, wobei die VM-Ermittlung durch die vCenter-Zugriffssteuerung zusätzlich eingeschränkt wird.

Die Anforderung der Isolation von Daten erfordert, dass sämtliche sensiblen Infrastrukturinformationen (z.B. Anmeldeinformationen für den Zugriff) den Mandanten nicht angezeigt werden. Aus diesem Grund wird empfohlen, dass alle Komponenten des Verwaltungsservers (Konfigurationsserver, Prozessserver und Masterzielserver) der ausschließlichen Kontrolle des Partners unterliegen. Dies schließt Prozessserver mit horizontaler Skalierung ein.

### <a name="every-cs-under-the-multi-tenant-scenario-uses-two-accounts"></a>Jeder Konfigurationsserver im Szenario mit mehreren Mandanten verwendet zwei Konten:

- **vCenter-Zugriffskonto**: Dieses Konto dient zur Ermittlung der Mandanten-VMs und verfügt über die zugewiesenen vCenter-Zugriffsberechtigungen (die im nächsten Abschnitt beschrieben werden). Es wird empfohlen, dass der Partner diese Anmeldeinformationen im Konfigurationstool eingibt, um die versehentliche Preisgabe von Zugangsdaten zu vermeiden.
- **VM-Zugriffskonto**: Dieses Konto dient zum Installieren des Mobilitäts-Agents auf den VMs des Mandanten per automatischem Push. Dies ist normalerweise ein Domänenkonto, das ein Mandant dem Partner ggf. bereitstellt oder das auch vom Partner direkt verwaltet werden kann. Wenn der Mandant die Details dem Partner nicht direkt mitteilen möchte, kann ihm erlaubt werden, die Anmeldeinformationen im Rahmen eines zeitlich begrenzten Zugriffs auf den Konfigurationsserver einzugeben. Alternativ kann der Mandant die Mobilitäts-Agents mithilfe des Partners manuell installieren.

### <a name="requirements-for-vcenter-access-account"></a>Anforderungen für das vCenter-Zugriffskonto

Wie im vorherigen Abschnitt erläutert, muss der Konfigurationsserver mit einem Konto konfiguriert werden, dem eine spezielle Rolle zugewiesen ist. Beachten Sie, dass diese Rollenzuweisung für das vCenter-Zugriffskonto für jedes vCenter-Objekt erfolgen muss und nicht an die untergeordneten Objekte weitergegeben werden darf. Dies geschieht, um die Mandantenisolation sicherzustellen, da die Weitergabe von Zugangsdaten zu einem versehentlichen Zugriff auf andere Objekte führen kann.

![Berechtigungen ohne Weitergabe](./media/site-recovery-multi-tenant-support-vmware-using-csp/assign-permissions-without-propagation.png)

Die Alternative ist das Zuweisen des Benutzerkontos und der Rolle zum Objekt „Datacenter“ und die Weitergabe an untergeordnete Objekte. Danach muss diesem Konto die Berechtigung „Kein Zugriff“ für alle Objekte (z.B. VMs anderer Mandanten) erteilt werden, auf die ein bestimmter Mandant keinen Zugriff haben soll. Dies ist einerseits mühsam und ermöglicht andererseits die versehentliche Preisgabe von Zugangsdaten, da jedem neu erstellten untergeordneten Objekt auch automatisch die vom übergeordneten Objekt vererbten Zugangsrechte erteilt werden. Es wird daher empfohlen, den ersten Ansatz zu befolgen.

Das Zugriffsverfahren des vCenter-Kontos ist wie folgt:

1.    Erstellen Sie eine neue Rolle durch Klonen der vordefinierten Rolle „Schreibgeschützt“, und geben Sie ihr einen passenden Namen (z. B. „Azure_Site_Recovery“ wie in diesem Beispiel).
2.    Weisen Sie dieser Rolle die folgenden Berechtigungen zu:
 *    Datenspeicher -> Speicherplatz zuordnen, Datenspeicher durchsuchen, Low-Level-Dateivorgänge, Datei entfernen, Dateien virtueller Computer aktualisieren
 *    Netzwerk -> Netzwerk zuweisen
 *    Ressource -> Zuweisen der VM zu einem Ressourcenpool, ausgeschaltete VM migrieren, eingeschaltete VM migrieren
 *    Tasks (Aufgaben) -> Create task (Aufgabe erstellen), update task (Aufgabe aktualisieren)
 *    Virtueller Computer -> Konfiguration
 *    Virtueller Computer -> Interagieren -> Frage beantworten, Geräteverbindung, CD-Medien konfigurieren, Diskettenmedien konfigurieren, Ausschalten, Einschalten, VMware-Tools installieren
 *    Virtueller Computer -> Inventar -> Erstellen, Registrieren, Registrierung aufheben
 *    Virtueller Computer -> Bereitstellung -> Download virtueller Computer zulassen, Upload von Dateien virtueller Computer zulassen
 *    Virtueller Computer -> Momentaufnahmen -> Momentaufnahmen entfernen

    ![Berechtigungen für Rollen](./media/site-recovery-multi-tenant-support-vmware-using-csp/edit-role-permissions.png)

3.    Weisen Sie dem vCenter-Konto (das im Konfigurationsserver des Mandanten verwendet wird) für verschiedene Objekte Zugriffsebenen wie folgt zu:

| **Object** | **Rolle** | **Hinweise:** |
| --- | --- | --- |
| vCenter | Schreibgeschützt | Dies ist nur erforderlich, um vCenter den Zugriff zum Verwalten verschiedener Objekten zu gewähren. Diese Berechtigung kann aufgehoben werden, wenn das Konto keinem Mandanten zur Verfügung gestellt wird oder nicht für Verwaltungsvorgänge in vCenter verwendet wird. |
| Datacenter | Azure_Site_Recovery |  |
| Host und Hostcluster | Azure_Site_Recovery | Stellen Sie erneut sicher, dass der Zugriff auf Objektebene erfolgt, sodass es nur einen Zugriff auf die Hosts gibt, die vor dem Failover und nach dem Failback über Mandanten-VMs verfügen. |
| Datenspeicher und Datenspeichercluster | Azure_Site_Recovery | Wie oben |
| Netzwerk | Azure_Site_Recovery |  |
| Verwaltungsserver | Azure_Site_Recovery | Dies schließt den Zugriff auf alle Komponenten (Konfigurationsserver, Prozessserver und Masterzielserver) ein, wenn sich beliebige davon außerhalb des Konfigurationsservercomputers befinden. |
| Mandanten-VMs | Azure_Site_Recovery | Stellen Sie sicher, dass alle neuen VMs eines bestimmten Mandanten auch diesen Zugriff erhalten, da sie andernfalls nicht im Azure-Portal ermittelt werden können. |

Der vCenter-Kontozugriff ist nun eingerichtet. Dadurch ist die Anforderung an die Mindestberechtigungen zum Durchführen von Failbackvorgängen erfüllt. Beachten Sie, dass diese Zugriffsberechtigungen auch mit Ihren vorhandenen Richtlinien verwendet werden können. Ändern Sie Ihre vorhandenen Berechtigungen lediglich so, dass die unter Punkt 2 oben genannten Rollenberechtigungen hinzugefügt werden.

Zum Einschränken von Vorgängen für die Notfallwiederherstellung bis zum Failoverstatus, d.h. ohne Failbackfunktionen, befolgen Sie das vorherige Verfahren. Doch anstatt dem vCenter-Zugriffskonto die Rolle „Azure_Site_Recovery“ zuzuweisen, weisen Sie ihm lediglich die Rolle „Schreibgeschützt“ zu. Dieser Berechtigungssatz lässt die Replikation und ein Failover von VMs, aber kein Failback zu. Alles andere im obigen Prozess bleibt unverändert. Berechtigungen werden weiterhin nur auf Objektebene zugewiesen und nicht an untergeordnete Objekte weitergegeben, um die Mandantenisolation sicherzustellen und die VM-Ermittlung einzuschränken.

## <a name="other-multi-tenant-environments"></a>Andere Umgebungen mit mehreren Mandanten

Bislang wurde das Einrichten einer Umgebung für mehrere Mandanten für eine SHPS-Lösung detailliert beschrieben. Die anderen beiden wichtigen Lösungen heißen DHPS und MSP. Die jeweilige Architektur wird nachstehend gezeigt:

### <a name="dedicated-hosting-solution"></a>Dedizierte Hostinglösung

![Architektur beim SHPS-Modell](./media/site-recovery-multi-tenant-support-vmware-using-csp/dedicated-hosting-scenario.png)

**Abbildung 2: Dediziertes Hostingszenario mit mehreren vCentern**

Der Unterschied bei der Architektur besteht darin, dass die Infrastruktur jedes Mandanten nur für den jeweiligen Mandanten bereitgestellt wird. Der hostenden Anbieter muss weiterhin die CSP-Schritte ausführen, die für eine gemeinsam genutzte Hostinglösung beschrieben wurden. Er muss sich jedoch nicht um die Mandantenisolation kümmern, da Mandanten mithilfe getrennter vCenter isoliert werden. Die CSP-Bereitstellung bleibt unverändert.

### <a name="managed-service-solution"></a>MSP-Lösung

![Architektur beim SHPS-Modell](./media/site-recovery-multi-tenant-support-vmware-using-csp/managed-service-scenario.png)

**Abbildung 3: MSP-Szenario mit mehreren vCentern**

Der Unterschied bei der Architektur besteht darin, dass die Infrastruktur jedes Mandanten auch physisch von anderen Mandanten getrennt ist. Dieses Szenario liegt in der Regel vor, wenn der Mandant die Infrastruktur besitzt und lediglich möchte, dass ein Lösungsanbieter die Notfallwiederherstellung verwaltet. Der Partner muss wiederum die CSP-Schritte ausführen, die für eine gemeinsam genutzte Hostinglösung beschrieben wurden. Er muss sich jedoch nicht um die Mandantenisolation kümmern, da Mandanten mithilfe unterschiedlicher Infrastrukturen physisch isoliert sind. Die CSP-Bereitstellung bleibt unverändert.


## <a name="csp-program-overview"></a>Übersicht über das CSP-Programm
Das Microsoft Cloud Solution Provider-[Programm](https://partner.microsoft.com/en-US/cloud-solution-provider) (CSP) ermöglicht ein besseres Zusammenspiel mit Partnern beim Anbieten sämtlicher Microsoft-Clouddienste einschließlich Office&365;, EMS und Microsoft Azure. Es ermöglicht unseren Partnern eine lückenlose Zuständigkeit für die Beziehung mit Kunden und die Position als Hauptansprechpartner. Über das CSP-Programm kann ein Partner Azure-Abonnements für Kunden bereitstellen und diese Abonnements mit ihren eigenen angepassten Angeboten mit einem Mehrwert kombinieren.

Im Fall von Azure Site Recovery können Partner die gesamte Notfallwiederherstellungslösung für Kunden direkt über das CSP-Programm verwalten. Sie können auch im Rahmen dieses Programms die Azure Site Recovery-Umgebungen einrichten und Kunden ermöglichen, ihre eigenen Anforderungen an die Notfallwiederherstellung in einem Self-Service-Modell zu erfüllen. In beiden Szenarien ist der Partner das Bindeglied zwischen Azure Site Recovery und Endkunden. Der Partner kümmert sich um die Beziehung mit den Kunden und stellt ihnen die Nutzung von Azure Site Recovery in Rechnung.

## <a name="creating-and-managing-tenant-accounts"></a>Erstellen und Verwalten von Mandantenkonten

### <a name="step-0-prerequisite-check"></a>Vorbereitender Schritt: Überprüfung der Voraussetzungen

Die VM-Voraussetzungen sind identisch mit der Beschreibung in der [Dokumentation](site-recovery-vmware-to-azure.md) zu Azure Site Recovery. Zusätzlich zu diesen Voraussetzungen müssen die oben genannten Zugriffssteuerungen vorhanden sein, ehe mit der Mandantenverwaltung im CSP-Programm fortgefahren wird. Erstellen Sie für jeden Mandanten einen separaten Verwaltungsserver, der mit den VMs des Mandanten und dem vCenter des Partners kommunizieren kann. Nur der Partner verfügt über Zugriffsrechte für diesen Server.

### <a name="step-1-create-tenant-account"></a>Schritt 1: Erstellen des Mandantenkontos

1.    Melden Sie sich über [Partnercenter](https://partnercenter.microsoft.com/) bei Ihrem CSP-Konto an. Wählen Sie links im Menü „Dashboard“ die Option „Kunden“ aus.

    ![CSP-Dashboard](./media/site-recovery-multi-tenant-support-vmware-using-csp/csp-dashboard-display.png)

2.    Klicken Sie auf der Seite, die geöffnet wird, auf „Kunden hinzufügen“.

    ![Hinzufügen von Kunden](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-new-customer.png)

3.    Füllen Sie auf der Seite „Neuer Kunde“ alle Kontodetails für den Mandanten aus, und klicken Sie dann auf „Weiter -> Abonnements“.

    ![Ausfüllen von Details](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-add-filled.png)

4.    Scrollen Sie auf der Seite zur Auswahl von Abonnements nach unten, um das Abonnement „Microsoft Azure“ hinzuzufügen. Andere Abonnements können jetzt oder zu einem späteren Zeitpunkt hinzugefügt werden.

    ![Abonnement hinzufügen](./media/site-recovery-multi-tenant-support-vmware-using-csp/azure-subscription-selection.png)

5.    Fahren Sie fort, und überprüfen Sie auf der nächsten Seite alle Details, die für den Mandanten eingegeben wurden. Klicken Sie anschließend auf die Schaltfläche „Senden“.

    ![Kundenzusammenfassung](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-summary-page.png)

6.    Nach der Erstellung des Kunden wird eine Bestätigungsseite mit den Details des Standardkontos und dem Kennwort für dieses Abonnement eingeblendet. Speichern Sie die Informationen, und ändern Sie das Kennwort später nach der Anmeldung beim Azure-Portal den Anforderungen entsprechend. Diese Informationen können wie vorhanden mit dem Mandanten gemeinsam genutzt werden. Bei Bedarf kann auch ein gesondertes Konto erstellt und freigegeben werden.

### <a name="step-2-access-tenant-account"></a>Schritt 2: Zugreifen auf das Mandantenkonto

1.    Wie in Schritt 1 beschrieben, können Sie in Ihrem Dashboard auf der Seite „Kunden“ auf das Abonnement des Mandanten zugreifen. Navigieren Sie hierhin, und klicken Sie auf den Namen des Mandantenkontos, das Sie gerade erstellt haben.
2.    Daraufhin wird der Abschnitt „Abonnements“ des Mandantenkontos geöffnet, in dem Sie die vorhandenen Abonnements des Kontos überwachen und bei Bedarf weitere Abonnements hinzufügen können. Wählen Sie zum Verwalten der Notfallwiederherstellungsvorgänge des Mandanten rechts auf der Seite „Alle Ressourcen (Azure-Portal)“ aus.

    ![Alle Ressourcen](./media/site-recovery-multi-tenant-support-vmware-using-csp/all-resources-select.png)

3.    Durch Klicken auf die Schaltfläche „Alle Ressourcen“ erhalten Sie Zugriff auf die Azure-Abonnements des Mandanten. Dieselben Informationen können Sie auch im Azure Active Directory (AAD) überprüfen, das rechts oben im Azure-Portal angezeigt wird.

    ![AAD-Administrator](./media/site-recovery-multi-tenant-support-vmware-using-csp/aad-admin-display.png)

Sie können nun alle Azure Site Recovery-Vorgänge für den Mandanten über das Azure-Portal ausführen und die Notfallwiederherstellungsvorgänge verwalten. Der zuvor beschriebene Prozess muss jedes Mal befolgt werden, um über das CSP-Programm für die verwaltete Notfallwiederherstellung auf das Abonnement des Mandanten zuzugreifen.

### <a name="step-3-deploy-resources-to-tenant-subscription"></a>Schritt 3: Bereitstellen von Ressourcen für das Abonnement des Mandanten
1.    Erstellen Sie im Azure-Portal eine Ressourcengruppe, und stellen Sie einen Recovery Services-Tresor wie üblich bereit. Laden Sie den Tresorregistrierungsschlüssel herunter.
2.    Registrieren Sie den Konfigurationsserver des Mandanten mithilfe des Tresorregistrierungsschlüssels.
3.    Geben Sie die Anmeldeinformationen für die beiden Zugriffskonten, das vCenter-Zugriffskonto und VM-Zugriffskonto, ein.

    ![Konfigurieren von Konten](./media/site-recovery-multi-tenant-support-vmware-using-csp/config-server-account-display.png)

### <a name="step-4-register-site-recovery-infrastructure-to-recovery-services-vault"></a>Schritt 4: Registrieren von Site Recovery-Infrastruktur im Recovery Services-Tresor
1.    Öffnen Sie das Azure-Portal, und registrieren Sie im zuvor erstellten Tresor den vCenter-Server beim im vorherigen Schritt registrierten Konfigurationsserver. Verwenden Sie für diesen Zweck das vCenter-Zugriffskonto.
2.    Beenden Sie den Prozess zur Vorbereitung der Infrastruktur für Site Recovery wie üblich.
3.    Die VMs können jetzt repliziert werden. Vergewissern Sie sich, dass nur die VMs des Mandanten auf dem Blatt zur VM-Auswahl unter der Option „Replizieren“ angezeigt werden.

    ![Mandanten-VMs](./media/site-recovery-multi-tenant-support-vmware-using-csp/tenant-vm-display.png)

### <a name="step-5-assign-tenant-access-to-subscription"></a>Schritt 5: Zuweisen des Mandantenzugriffs auf das Abonnement

Bei einer Self-Service-Lösung für die Notfallwiederherstellung müssen dem Mandanten die Kontodetails gemäß Punkt 6 von Schritt 1 bereitgestellt werden. Dies muss erfolgen, nachdem der Partner die Infrastruktur für die Notfallwiederherstellung eingerichtet hat. Unabhängig von der Art der Notfallwiederherstellung (verwaltet oder Self-Service) darf der Partner nur über das CSP-Portal auf die Mandantenabonnements zugreifen und den Tresor einrichten sowie die Infrastruktur des Partners in den Mandantenabonnements registrieren.

Ein Partner kann auch über das CSP-Portal dem Mandantenabonnement einen neuen Benutzer wie folgt hinzufügen:

1.    Wechseln Sie zur CSP-Abonnementseite eines bestimmten Mandanten, und wählen Sie die Option „Benutzer und Lizenzen“ aus.

    ![Benutzer und Lizenzen](./media/site-recovery-multi-tenant-support-vmware-using-csp/users-and-licences.png)

    Sie können nun einen neuen Benutzer erstellen, indem Sie die relevanten Details eingeben und Berechtigungen auswählen. Sie können auch die Liste der Benutzer mithilfe einer CSV-Datei hochladen.
2.    Nachdem die Benutzer erstellt wurden, navigieren Sie zurück zum Azure-Portal und wählen auf dem Blatt „Abonnement“ das entsprechende Abonnement aus.
3.    Wählen Sie auf dem geöffneten neuen Blatt „Zugriffssteuerung (IAM)“ aus, und klicken Sie auf „+Hinzufügen“, um einen Benutzer mit der entsprechenden Zugriffsebene hinzuzufügen. Die über das CSP-Portal erstellten Benutzer werden automatisch auf dem Blatt angezeigt, das nach dem Klicken auf eine Zugriffsebene geöffnet wird.

    ![Benutzerabonnement](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-user-subscription.png)

    Für die meisten Verwaltungsvorgänge ist die Rolle „Mitwirkender“ ausreichend. Ein Benutzer mit dieser Zugriffsebene kann fast alle Aufgaben für ein Abonnement ausführen, jedoch keine Zugriffsebenen ändern (wofür die Zugriffsebene „Besitzer“ erforderlich ist). Sie können die Zugriffsebenen nach Bedarf auch optimieren.



<!--HONumber=Feb17_HO3-->


