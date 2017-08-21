---
title: "Unterstützung mehrerer Mandanten für die VMware-VM-Replikation in Azure (CSP-Programm) | Microsoft-Dokumentation"
description: "Beschreibt, wie Azure Site Recovery in einer Umgebung mit mehreren Mandanten bereitgestellt wird, um Replikation, Failover und Wiederherstellung von lokalen VMware-VMs in Azure über das CSP-Programm im Azure-Portal zu orchestrieren."
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: manayar
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 97edbe67c25036dc1156f0f0ca5431a617d7a004
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017


---
# <a name="multi-tenant-support-in-azure-site-recovery-for-replicating-vmware-virtual-machines-to-azure-through-csp"></a>Unterstützung mehrerer Mandanten in Azure Site Recovery für die Replikation von VMware-VMs in Azure über das CSP-Programm

Azure Site Recovery unterstützt für Mandantenabonnements Umgebungen mit mehreren Mandanten. Unterstützt wird außerdem die Mehrmandantenfähigkeit für Mandantenabonnements, die über das CSP-Programm (Cloud Solution Provider, Cloud-Lösungsanbieter) erstellt und verwaltet werden. In diesem Artikel wird das Implementieren und Verwalten von VMware-zu-Azure-Szenarien mit mehreren Mandanten detailliert behandelt. Das Erstellen und Verwalten von Mandantenabonnements über das CSP-Programm wird ebenfalls beschrieben.

Diese Anleitung bezieht sich umfassend auf die vorhandene Dokumentation zum Replizieren von VMware-VMs in Azure. Weitere Informationen finden Sie unter [Replizieren von virtuellen VMware-Computern in Azure mithilfe von Site Recovery](site-recovery-vmware-to-azure.md).

## <a name="multi-tenant-environments"></a>Umgebungen mit mehreren Mandanten
Es gibt drei wesentliche Modelle mit mehreren Mandanten:

* **Shared Hosting Services Provider (SHSP, Anbieter von gemeinsam genutzten Hostingdiensten)**: Der Partner besitzt die physische Infrastruktur und arbeitet mit gemeinsam genutzten Ressourcen (vCenter, Rechenzentren, physischem Speicher usw.), um die VMs mehrerer Mandanten in derselben Infrastruktur zu hosten. Der Partner kann die Verwaltung der Notfallwiederherstellung als verwalteten Dienst bereitstellen, oder der Mandant kann die Notfallwiederherstellung als Self-Service-Lösung betreiben.

* **Dedicated Hosting Services Provider (DHSP, Anbieter dedizierter Hostingdienste)**: Der Partner besitzt die physische Infrastruktur, wobei jedoch dedizierte Ressourcen (mehrere vCenter-Server, physische Datenspeicher usw.) verwendet werden, um die VMs der einzelnen Mandanten in getrennter Infrastruktur zu hosten. Der Partner kann die Verwaltung der Notfallwiederherstellung als verwalteten Dienst bereitstellen, oder der Mandant kann diese als Self-Service-Lösung betreiben.

* **Managed Services Provider (MSP, Anbieter verwalteter Dienste)**: Der Kunde besitzt die physische Infrastruktur, in der die VMs gehostet werden, während der Partner für die Aktivierung und Verwaltung der Notfallwiederherstellung zuständig ist.

## <a name="shared-hosting-multi-tenant-guidance"></a>Anleitung für SHSP mit mehreren Mandanten
In dieser Abschnitt wird das SHSP-Szenario ausführlich behandelt. Die anderen beiden Szenarien sind eine Teilmenge des SHSP-Szenarios und folgen denselben Prinzipien. Die Unterschiede werden am Ende der SHSP-Anleitung beschrieben.

Die grundlegende Anforderung in einem Szenario mit mehreren Mandanten ist deren Isolation. Ein Mandant darf nicht wahrnehmen, was ein anderer Mandant hostet. In einer von einem Partner verwalteten Umgebung ist diese Anforderung nicht so wichtig wie für eine Self-Service-Umgebung, in der sie kritisch sein kann. In dieser Anleitung wird davon ausgegangen, dass die Mandantenisolation erforderlich ist.

Die Architektur ist im folgenden Diagramm dargestellt:

![SHSP mit einem vCenter](./media/site-recovery-multi-tenant-support-vmware-using-csp/shared-hosting-scenario.png)  
**SHSP-Szenario mit einem vCenter**

Wie Sie im vorigen Diagramm sehen können, hat jeder Kunde einen eigenen Verwaltungsserver. Diese Konfiguration beschränkt den Mandantenzugriff auf mandantenspezifische VMs und ermöglicht die Isolation von Mandanten. In einem VMware-VM-Replikationsszenario wird der Konfigurationsserver zum Verwalten von Konten verwendet, um VMs zu ermitteln und Agents zu installieren. Wir befolgen dieselben Prinzipien für Umgebungen mit mehreren Mandanten, wobei die VM-Ermittlung durch die vCenter-Zugriffssteuerung zusätzlich eingeschränkt wird.

Die Anforderung der Isolation von Daten erfordert, dass sämtliche sensiblen Infrastrukturinformationen (z.B. Anmeldeinformationen für den Zugriff) den Mandanten nicht angezeigt werden. Aus diesem Grund wird empfohlen, dass alle Komponenten des Verwaltungsservers unter der alleinigen Kontrolle des Partners verbleiben. Der Verwaltungsserver hat die folgenden Komponenten:
* Konfigurationsserver
* Prozessserver
* Masterzielserver 

Ein horizontal skalierter Prozessserver ist auch unter Kontrolle des Partners.

### <a name="every-cs-in-the-multi-tenant-scenario-uses-two-accounts"></a>Jeder Konfigurationsserver im Szenario mit mehreren Mandanten verwendet zwei Konten:

- **vCenter-Zugriffskonto**: Dieses Konto dient zum Ermitteln von Mandanten-VMs. Ihm sind vCenter-Zugriffsberechtigungen zugewiesen (wie im nächsten Abschnitt beschrieben). Um versehentliche Zugriffslecks zu vermeiden, wird empfohlen, dass Partner diese Anmeldeinformationen selbst in das Konfigurationstool eingeben.

- **VM-Zugriffskonto**: Dieses Konto dient zum Installieren des Mobilitäts-Agents auf den VMs des Mandanten per automatischem Push. Dies ist normalerweise ein Domänenkonto, das ein Mandant einem Partner ggf. bereitstellt oder das der Partner direkt verwalten kann. Wenn der Mandant die Details dem Partner nicht direkt mitteilen möchte, kann ihm erlaubt werden, die Anmeldeinformationen im Rahmen eines zeitlich begrenzten Zugriffs auf den Konfigurationsserver einzugeben. Alternativ kann der Mandant die Mobilitäts-Agents mithilfe des Partners manuell installieren.

### <a name="requirements-for-a-vcenter-access-account"></a>Anforderungen für das vCenter-Zugriffskonto

Wie im vorherigen Abschnitt erwähnt, müssen Sie den Konfigurationsserver mit einem Konto installieren, dem eine besondere Rolle zugewiesen ist. Diese Rollenzuweisung muss für das vCenter-Zugriffskonto für jedes vCenter-Objekt erfolgen und darf nicht an die untergeordneten Objekte weitergegeben werden. Diese Konfiguration stellt die Mandantenisolation sicher, da die Weitergabe von Zugangsdaten zu einem versehentlichen Zugriff auf andere Objekte führen kann.

![Die Option „An untergeordnete Objekte weitergeben“](./media/site-recovery-multi-tenant-support-vmware-using-csp/assign-permissions-without-propagation.png)

Die Alternative besteht darin, das Benutzerkonto und die Rolle dem Rechenzentrumsobjekt zuzuweisen und diese an die untergeordneten Objekte weiterzugeben. Weisen Sie anschließend dem Konto die Rolle *Kein Zugriff* für alle Objekte zu (z.B. VMs anderer Mandanten), auf die ein bestimmter Mandant keinen Zugriff haben soll. Diese Konfiguration ist mühsam und ermöglicht die versehentliche Preisgabe von Zugangsdaten, da jedem neuen untergeordneten Objekt auch automatisch die vom übergeordneten Objekt vererbten Zugangsrechte erteilt werden. Aus diesem Grund wird empfohlen, die erste Methode zu verwenden.

Das Zugriffsverfahren des vCenter-Kontos ist wie folgt:

1. Erstellen Sie eine neue Rolle durch Klonen der vordefinierten Rolle *Schreibgeschützt*, und geben Sie ihr einen passenden Namen (z.B. „Azure_Site_Recovery“ wie in diesem Beispiel).

2. Weisen Sie dieser Rolle die folgenden Berechtigungen zu:

    * **Datenspeicher**: Speicherplatz zuordnen, Datenspeicher durchsuchen, Low-Level-Dateivorgänge, Datei entfernen, Dateien virtueller Computer aktualisieren

    * **Netzwerk**: Netzwerk zuweisen

    * **Ressource** Zuweisen der VM zu einem Ressourcenpool, ausgeschaltete VM migrieren, eingeschaltete VM migrieren

    * **Aufgaben**: Aufgabe erstellen, Aufgabe aktualisieren

    * **VM**: 
        * Konfiguration > Alle
        * Interaktion > Frage beantworten, Geräteverbindung, CD-Medien konfigurieren, Diskettenmedien konfigurieren, Ausschalten, Einschalten, VMware-Tools installieren
        * Bestandsliste > Aus vorhandenen erstellen, Neu erstellen, Registrieren, Registrierung aufheben
        * Bereitstellung > Download virtueller Maschinen zulassen, Upload von Dateien virtueller Maschinen zulassen
        * Snapshot-Manager > Snapshots entfernen

    ![Das Dialogfeld „Rolle bearbeiten“](./media/site-recovery-multi-tenant-support-vmware-using-csp/edit-role-permissions.png)

3. Weisen Sie dem vCenter-Konto (das im Konfigurationsserver des Mandanten verwendet wird) für verschiedene Objekte Zugriffsebenen wie folgt zu:

>| Objekt | Rolle | Anmerkungen |
>| --- | --- | --- |
>| vCenter | Schreibgeschützt | Nur erforderlich, um vCenter den Zugriff zum Verwalten verschiedener Objekten zu gewähren. Sie können diese Berechtigung aufheben, wenn das Konto keinem Mandanten zur Verfügung gestellt wird oder nicht für Verwaltungsvorgänge in vCenter verwendet wird. |
>| Datacenter | Azure_Site_Recovery |  |
>| Host und Hostcluster | Azure_Site_Recovery | Stellt erneut sicher, dass der Zugriff auf Objektebene erfolgt, sodass es nur Zugriff auf Hosts gibt, die vor dem Failover und nach dem Failback über Mandanten-VMs verfügen. |
>| Datenspeicher und Datenspeichercluster | Azure_Site_Recovery | Siehe oben. |
>| Netzwerk | Azure_Site_Recovery |  |
>| Verwaltungsserver | Azure_Site_Recovery | Schließt den Zugriff auf alle Komponenten (Konfigurationsserver, Prozessserver und Masterzielserver) ein, wenn sich beliebige davon außerhalb des Konfigurationsservercomputers befinden. |
>| Mandanten-VMs | Azure_Site_Recovery | Stellt sicher, dass alle neuen VMs eines bestimmten Mandanten auch diesen Zugriff erhalten, da sie andernfalls nicht im Azure-Portal ermittelt werden können. |

Der vCenter-Kontozugriff ist nun eingerichtet. Mit diesem Schritt ist die Anforderung an die Mindestberechtigungen zum Durchführen von Failbackvorgängen erfüllt. Diese Zugriffsberechtigungen können auch mit Ihren vorhandenen Richtlinien verwendet werden. Ändern Sie Ihre vorhandenen Berechtigungen lediglich so, dass die unter Punkt 2 zuvor genannten Rollenberechtigungen hinzugefügt werden.

Zum Einschränken von Vorgängen für die Notfallwiederherstellung bis zum Failoverstatus, d.h. ohne Failbackfunktionen, befolgen Sie das vorherige Verfahren. Doch anstatt dem vCenter-Zugriffskonto die Rolle *Azure_Site_Recovery* zuzuweisen, weisen Sie ihm lediglich die Rolle *Schreibgeschützt* zu. Dieser Berechtigungssatz lässt die Replikation und ein Failover von VMs, aber kein Failback zu. Alles andere im vorhergehenden Prozess bleibt unverändert. Berechtigungen werden weiterhin nur auf Objektebene zugewiesen und nicht an untergeordnete Objekte weitergegeben, um die Mandantenisolation sicherzustellen und die VM-Ermittlung einzuschränken.

## <a name="other-multi-tenant-environments"></a>Andere Umgebungen mit mehreren Mandanten

Im vorherigen Abschnitt wurde das Einrichten einer Umgebung für mehrere Mandanten für eine SHPS-Lösung detailliert beschrieben. Die anderen beiden wichtigen Lösungen heißen DHPS und MSP. Die Architektur dieser Lösungen wird in den folgenden Abschnitten beschrieben.

### <a name="dedicated-hosting-solution"></a>Dedizierte Hostinglösung (DHPS)

Wie im folgenden Diagramm dargestellt, ist der architektonische Unterschied bei einer dedizierten Hostinglösung, dass die Infrastruktur jedes Mandanten allein für den jeweiligen Mandanten eingerichtet ist. Da Mandanten mithilfe getrennter vCenter isoliert werden, muss der Hostinganbieter weiterhin die CSP-Schritte ausführen, die für eine gemeinsam genutzte Hostinglösung beschrieben wurden. Er muss sich jedoch nicht um die Mandantenisolation kümmern. Das CSP-Setup bleibt unverändert.

![Architektur beim SHPS-Modell](./media/site-recovery-multi-tenant-support-vmware-using-csp/dedicated-hosting-scenario.png)  
**Dediziertes Hostingszenario mit mehreren vCentern**

### <a name="managed-service-solution"></a>MSP-Lösung (Managed Services Provider, Managed Services Provider)

Wie im folgenden Diagramm dargestellt, ist der architektonische Unterschiede bei einer MSP-Lösung, dass die Infrastruktur jedes Mandanten auch physisch von der Infrastruktur anderen Mandanten getrennt ist. Dieses Szenario liegt in der Regel vor, wenn der Mandant die Infrastruktur besitzt und lediglich möchte, dass sich ein Lösungsanbieter um die Notfallwiederherstellung kümmert. Da Mandanten wiederum mithilfe unterschiedlicher Infrastrukturen physisch isoliert sind, muss der Partner wiederum die CSP-Schritte ausführen, die für eine gemeinsam genutzte Hostinglösung beschrieben wurden. Er muss sich jedoch nicht um die Mandantenisolation kümmern. Die CSP-Bereitstellung bleibt unverändert.

![Architektur beim SHPS-Modell](./media/site-recovery-multi-tenant-support-vmware-using-csp/managed-service-scenario.png)  
**MSP-Szenario mit mehreren vCentern**

## <a name="csp-program-overview"></a>Übersicht über das CSP-Programm
Das [CSP-Programm](https://partner.microsoft.com/en-US/cloud-solution-provider) fördert ein besseres Zusammenspiel aller Partner beim Anbieten von Microsoft Cloud Services, einschließlich Office 365, Enterprise Mobility Suite und Microsoft Azure. Das CSP-Programm ermöglicht unseren Partnern eine lückenlose Zuständigkeit für die Beziehung mit Kunden und ihre Position als Hauptansprechpartner. Partner können Azure-Abonnements für Kunden bereitstellen und diese Abonnements mit ihren eigenen angepassten Angeboten mit einem Mehrwert kombinieren.

Mit Azure Site Recovery können Partner die vollständige Lösung für Notfallwiederherstellung für Kunden direkt im CSP-Programm verwalten. Oder sie können im CSP-Programm Site Recovery-Umgebungen einrichten und Kunden ihre eigenen Anforderungen an die Notfallwiederherstellung in Self-Service-Manier erfüllen lassen. In beiden Szenarien fungieren Partner als Verbindungsglied zwischen Site Recovery und ihren Kunden. Partner sind für die Kundenbeziehung zuständig und stellen Kunden die Nutzung von Site Recovery in Rechnung.

## <a name="create-and-manage-tenant-accounts"></a>Erstellen und Verwalten von Mandantenkonten

### <a name="step-0-prerequisite-check"></a>Vorbereitender Schritt: Überprüfung der Voraussetzungen

Die VM-Voraussetzungen sind identisch mit der Beschreibung in der [Dokumentation](site-recovery-vmware-to-azure.md) zu Azure Site Recovery. Zusätzlich zu diesen Voraussetzungen müssen die oben genannten Zugriffssteuerungen vorhanden sein, ehe Sie mit der Mandantenverwaltung im CSP-Programm fortfahren. Erstellen Sie für jeden Mandanten einen separaten Verwaltungsserver, der mit den VMs des Mandanten und dem vCenter des Partners kommunizieren kann. Nur der Partner verfügt über Zugriffsrechte für diesen Server.

### <a name="step-1-create-a-tenant-account"></a>Schritt 1: Erstellen eines Mandantenkontos

1. Melden Sie im [Microsoft Partner Center](https://partnercenter.microsoft.com/) bei Ihrem CSP-Konto an. 
 
2. Klicken Sie im Menü **Dashboard** auf **Kunden**.

    ![Der Microsoft Partner Center-Link „Kunden“](./media/site-recovery-multi-tenant-support-vmware-using-csp/csp-dashboard-display.png)

3. Klicken Sie auf der Seite, die geöffnet wird, auf **Kunden hinzufügen**.

    ![Die Schaltfläche „Kunden hinzufügen“](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-new-customer.png)

4. Füllen Sie auf der Seite **Neuer Kunde** alle Kontoinformationen für den Mandanten aus, und klicken Sie dann auf **Weiter: Abonnements**.

    ![Die Seite „Kontoinformationen“](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-add-filled.png)

5. Aktivieren Sie auf der Seite zur Auswahl des Abonnements das Kontrollkästchen**Microsoft Azure**. Sie können weitere Abonnements sofort oder zu einem späteren Zeitpunkt hinzufügen.

    ![Das Kontrollkästchen „Microsoft Azure-Abonnement“](./media/site-recovery-multi-tenant-support-vmware-using-csp/azure-subscription-selection.png)

6. Bestätigen Sie auf der Seite **Überprüfung** die Mandantendetails, und klicken Sie dann auf **Senden**.

    ![Die Seite „Überprüfen“](./media/site-recovery-multi-tenant-support-vmware-using-csp/customer-summary-page.png)  

    Nachdem Sie das Mandantenkonto erstellt haben, wird eine Bestätigungsseite mit den Details des Standardkontos und dem Kennwort dieses Abonnements angezeigt. 

7. Speichern Sie die Informationen, und ändern Sie das Kennwort später auf der Anmeldungsseite des Azure-Portals den Anforderungen entsprechend.  
 
    Sie können diese Informationen dem Mandanten unverändert mitteilen oder bei Bedarf ein getrenntes Konto erstellen und freigeben.

### <a name="step-2-access-the-tenant-account"></a>Schritt 2: Zugreifen auf das Mandantenkonto

Sie können über das Microsoft Partner Center-Dashboard auf das Abonnement des Mandanten zugreifen, wie unter „Schritt 1: Erstellen eines Mandantenkontos“ beschrieben. 

1. Wechseln Sie zur Seite **Kunden**, und klicken Sie dann auf den Namen des Mandantenkontos.

2. Auf der Seite **Abonnements** des Mandantenkontos können Sie die vorhandenen Abonnements des Kontos überwachen und bei Bedarf weitere Abonnements hinzufügen. Wählen Sie zum Verwalten der Vorgänge zur Notfallwiederherstellung des Mandanten **Alle Ressourcen (Azure-Portal)** aus.

    ![Der Link „Alle Ressourcen“](./media/site-recovery-multi-tenant-support-vmware-using-csp/all-resources-select.png)  
    
    Durch Klicken auf **Alle Ressourcen** wird Ihnen Zugriff auf die Azure-Abonnements des Mandanten gewährt. Sie können den Zugriff überprüfen, indem Sie rechts oben im Azure-Portal auf den Link „Azure Active Directory“ klicken.

    ![Der Link „Azure Active Directory“](./media/site-recovery-multi-tenant-support-vmware-using-csp/aad-admin-display.png)

Sie können nun alle Standortwiederherstellungsvorgänge für den Mandanten über das Azure-Portal ausführen und die Notfallwiederherstellungsvorgänge verwalten. Um für die verwaltete Notfallwiederherstellung über das CSP-Programm auf das Mandantenabonnement zugreifen zu können, befolgen Sie den zuvor beschriebenen Prozess.

### <a name="step-3-deploy-resources-to-the-tenant-subscription"></a>Schritt 3: Bereitstellen von Ressourcen für das Abonnement des Mandanten
1. Erstellen Sie im Azure-Portal eine Ressourcengruppe, und stellen Sie dann einen Recovery Services-Tresor wie üblich bereit. 
 
2. Laden Sie den Tresorregistrierungsschlüssel herunter.

3. Registrieren Sie den Konfigurationsserver des Mandanten mithilfe des Tresorregistrierungsschlüssels.

4. Geben Sie die Anmeldeinformationen für die beiden Zugriffskonten (das vCenter-Zugriffskonto und VM-Zugriffskonto) ein.

    ![Verwalten von Konfigurationsserverkonten](./media/site-recovery-multi-tenant-support-vmware-using-csp/config-server-account-display.png)

### <a name="step-4-register-site-recovery-infrastructure-to-the-recovery-services-vault"></a>Schritt 4: Registrieren der Site Recovery-Infrastruktur im Recovery Services-Tresor
1. Registrieren Sie im Azure-Portal für den Tresor, den Sie zuvor erstellt haben, den vCenter-Server beim Konfigurationsserver, den Sie unter „Schritt 3: Bereitstellen von Ressourcen für das Mandantenabonnement“ registriert haben. Verwenden Sie für diesen Zweck das vCenter-Zugriffskonto.
2. Beenden Sie den Prozess zur Vorbereitung der Infrastruktur für Site Recovery wie üblich.
3. Die VMs können jetzt repliziert werden. Vergewissern Sie sich, dass nur die VMs des Mandanten auf dem Blatt **Virtuelle Computer auswählen** unter der Option **Replizieren** angezeigt werden.

    ![Liste der VMs des Mandanten auf dem Blatt „Virtuelle Computer auswählen“](./media/site-recovery-multi-tenant-support-vmware-using-csp/tenant-vm-display.png)

### <a name="step-5-assign-tenant-access-to-the-subscription"></a>Schritt 5: Zuweisen des Mandantenzugriffs auf das Abonnement

Geben Sie für die Self-Service-Notfallwiederherstellung dem Mandanten die Kontodetails entsprechend Schritt 6 unter „Schritt 1: Erstellen eines Mandantenkontos“ an. Führen Sie diese Aktion aus, nachdem der Partner die Infrastruktur für die Notfallwiederherstellung eingerichtet hat. Unabhängig davon, ob der Typ der Notfallwiederherstellung „Verwaltet“ oder „Self-Service“ ist, müssen Partner im CSP-Portal auf Abonnements des Mandanten zugreifen. Sie richten den Tresor in ihrem Besitz ein und registrieren Infrastruktur für die Abonnements des Mandanten.

Partner können auch über das CSP-Portal dem Mandantenabonnement einen neuen Benutzer wie folgt hinzufügen:

1. Wechseln Sie zur CSP-Abonnementseite des Mandanten, und wählen Sie die Option **Benutzer und Lizenzen** aus.

    ![Der CSP-Seite mit dem Abonnement des Mandanten](./media/site-recovery-multi-tenant-support-vmware-using-csp/users-and-licences.png)

    Sie können nun einen neuen Benutzer erstellen, indem Sie die relevanten Details eingeben und Berechtigungen auswählen. Sie können auch die Liste der Benutzer mithilfe einer CSV-Datei hochladen.

2. Nachdem ein neuer Benutzer erstellt wurde, navigieren Sie zurück zum Azure-Portal und wählen auf dem Blatt **Abonnement** das entsprechende Abonnement aus.

3. Wählen Sie auf dem geöffneten Blatt **Zugriffssteuerung (IAM)** aus, und klicken Sie auf **+Hinzufügen**, um einen Benutzer mit der entsprechenden Zugriffsebene hinzuzufügen.      
    Die über das CSP-Portal erstellten Benutzer werden automatisch auf dem Blatt angezeigt, das nach dem Klicken auf eine Zugriffsebene geöffnet wird.

    ![Hinzufügen eines Benutzers](./media/site-recovery-multi-tenant-support-vmware-using-csp/add-user-subscription.png)

    Für die meisten Verwaltungsvorgänge ist die Rolle *Mitwirkender* ausreichend. Benutzer mit dieser Zugriffsebene können fast alle Aufgaben für ein Abonnement ausführen, jedoch keine Zugriffsebenen ändern (wofür die Zugriffsebene *Besitzer* erforderlich ist). Sie können die Zugriffsebenen nach Bedarf auch optimieren.

