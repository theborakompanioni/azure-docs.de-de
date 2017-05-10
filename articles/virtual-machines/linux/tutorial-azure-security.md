---
title: "Überwachen der Sicherheit von Linux-VMs mit dem Azure Security Center | Microsoft-Dokumentation"
description: "Tutorial: Überwachen der Sicherheit von VMs mit dem Azure Security Center"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/01/2017
ms.author: nepeters
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 4c680ee63e1c2d8e858c725adc42bbcbc49e4045
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017

---
# <a name="monitor-vm-security-with-the-azure-security-center"></a>Überwachen der VM-Sicherheit mit dem Azure Security Center

Mithilfe des Azure Security Centers können Sie Einblick in die Konfiguration von Azure-Ressourcen in Bezug auf Sicherheitsmaßnahmen erhalten. Darüber hinaus bietet es eine integrierte Sicherheitsüberwachung, die Bedrohungen erkennen kann, die sonst unbemerkt bleiben würden. Dieses Tutorial enthält eine kurze Übersicht über das Azure Security Center und beschreibt, wie es mit virtuellen Azure-Computern verwendet wird.   

## <a name="security-center-overview"></a>Übersicht über das Security Center

Mit dem Azure Security Center können Sie potenzielle Probleme in Bezug auf die VM-Konfiguration und gezielte Sicherheitsbedrohungen feststellen. Hierzu zählen beispielsweise die Feststellung von VMs mit fehlenden Netzwerksicherheitsgruppen, unverschlüsselte Datenträger und Brute-Force-RDP-Angriffe. Diese Informationen werden auf dem Azure Security Center-Dashboard in übersichtlichen Diagrammen angezeigt.

Das Azure Security Center-Dashboard kann aufgerufen werden, indem Sie im linken Navigationsbereich des Azure-Portals auf **Security Center** klicken. Das Dashboard enthält eine allgemeine Ansicht der Ressourcenintegrität, Sicherheitswarnungen und Konfigurationsempfehlungen. In dieser Ansicht können Sie die Sicherheitsintegrität Ihrer Azure-Umgebung sehen, nach einer bestimmten Anzahl von aktuellen Empfehlungen suchen und den aktuellen Status von Bedrohungswarnungen anzeigen. Jedes dieser übergeordneten Diagramme kann erweitert werden, sodass ein bestimmter Bereich ausführlicher dargestellt werden kann.

![ASC-Dashboard](./media/tutorial-azure-security/asc-dash.png)

Azure Security Center geht über die reine Datenermittlung hinaus, da es Empfehlungen für erkannte Probleme bereitstellt. Wenn eine VM beispielsweise ohne eine verbundene Netzwerksicherheitsgruppe bereitgestellt wurde, wird eine Empfehlung mit Fehlerbehebungsschritten erstellt. Diese Empfehlungen sorgen auch für die Automatisierung der Wiederherstellung, ohne den Kontext des Azure Security Centers verlassen zu müssen.  

![Recommendations](./media/tutorial-azure-security/recommendations.png)

## <a name="configure-data-collection"></a>Konfigurieren der Datensammlung

Bevor Sie Einblick in VM-Sicherheitskonfigurationen erhalten können, muss die Azure Security Center-Datensammlung konfiguriert werden. Die umfasst das Aktivieren der Datensammlung und Erstellen eines Azure-Speicherkontos zum Speichern der gesammelten Daten. 

1. Klicken Sie auf dem Azure Security Center-Dashboard auf **Sicherheitsrichtlinie**, und wählen Sie Ihr Abonnement aus. 
2. Wählen Sie unter **Datensammlung** die Option *Ein* aus.
3. Klicken Sie auf **Speicherkonto auswählen**, und erstellen Sie ein neues Speicherkonto. Wählen Sie nach Abschluss des Vorgangs **OK**.
4. Klicken Sie auf dem Blatt **Sicherheitsrichtlinie** auf **Speichern**. 

Wenn dieser Vorgang abgeschlossen ist, wird der Azure Security Center-Datensammlungs-Agent auf allen virtuellen Computern installiert, und die Datensammlung beginnt. 

## <a name="configure-security-policy"></a>Konfigurieren der Sicherheitsrichtlinie

Eine Sicherheitsrichtlinie definiert die Sicherheitsrichtlinienelemente, für die Daten gesammelt und Empfehlungen bereitgestellt werden. Standardmäßig werden Azure-Ressourcen anhand aller Richtlinienelemente bewertet. Einzelne Richtlinienelemente können global für alle Azure-Ressourcen oder pro Ressourcengruppe deaktiviert werden. Diese Konfiguration bietet Ihnen die Möglichkeit, unterschiedliche Sicherheitsrichtlinien auf verschiedene Sätze von Azure-Ressourcen anzuwenden. Ausführliche Informationen zu Azure Security Center-Sicherheitsrichtlinien finden Sie unter [Festlegen von Sicherheitsrichtlinien im Azure Security Center](../../security-center/security-center-policies.md). 

So konfigurieren Sie eine Sicherheitsrichtlinie für alle Azure-Ressourcen

1. Klicken Sie auf dem Azure Security Center-Dashboard auf **Sicherheitsrichtlinie**, und wählen Sie dann Ihr Abonnement aus. 
2. Klicken Sie auf **Präventionsrichtlinie**.
3. Aktivieren oder deaktivieren Sie die Richtlinienelemente, die auf alle Azure-Ressourcen angewendet werden müssen.
4. Klicken Sie nach Abschluss des Vorgangs auf **OK**.
5. Klicken Sie auf dem Blatt **Sicherheitsrichtlinie** auf **Speichern**. 

Um eine Richtlinie für eine bestimmte Ressourcengruppe zu konfigurieren, führen Sie dieselben Schritte aus. Wählen Sie jedoch anstelle des Abonnements auf dem Blatt „Sicherheitsrichtlinie“ eine Ressourcengruppe aus. Wählen Sie bei der Konfiguration der Richtlinie *Eindeutig* unter **Vererbung** aus. Wenn Sie die Datensammlung für eine bestimmte Ressourcengruppe deaktivieren möchten, kann diese Konfiguration auch hier vorgenommen werden.

Im folgenden Beispiel wurde eine eindeutige Richtlinie für die Ressourcengruppe mit dem Namen *myResoureGroup* erstellt. In dieser Richtlinie wurden sowohl die Empfehlungen für die Datenträgerverschlüsselung als auch jene für die Web Application Firewall deaktiviert.

![Eindeutige Richtlinie](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a>Anzeigen der Integrität der VM-Konfiguration

Sobald die Datensammlung aktiviert und eine Sicherheitsrichtlinie konfiguriert wurde, stellt das Azure Security Center Warnungen und Empfehlungen bereit. Bei der Bereitstellung von VMs wird der Datensammlungs-Agent installiert, während das Azure Security Center mit Daten dieser neuen VMs aufgefüllt wird. Ausführliche Informationen zur Integrität der VM-Konfiguration finden Sie unter [Schützen von virtuellen Computern im Azure Security Center](../../security-center/security-center-virtual-machine-recommendations.md). 

Bei der Datensammlung wird die Ressourcenintegrität für jede VM und die zugehörige Azure-Ressource aggregiert und in einem übersichtlichen Diagramm dargestellt. Um die Ressourcenintegrität anzuzeigen, kehren Sie zum Azure Security Center-Dashboard zurück. Klicken Sie unter **Ressourcensicherheitsstatus** auf **Berechnen**. Klicken Sie abschließend auf dem Blatt **Berechnen** auf **Virtuelle Computer**. Diese Ansicht enthält eine Zusammenfassung des Konfigurationsstatus aller VMs.

![Rechenintegrität](./media/tutorial-azure-security/compute-health.png)

Beim Auswählen der einzelnen VMs werden alle Empfehlungen für die jeweilige VM angezeigt. Empfehlungen werden im nächsten Abschnitt dieses Tutorials erläutert.

## <a name="remediate-configuration-issues"></a>Beheben von Konfigurationsproblemen

Sobald das Azure Security Center mit dem Auffüllen der Konfigurationsdaten beginnt, werden Empfehlungen für die konfigurierte Sicherheitsrichtlinie bereitgestellt. Wenn eine VM beispielsweise ohne eine zugeordnete Netzwerksicherheitsgruppe konfiguriert wurde, wird eine Empfehlung zur Erstellung einer solchen Gruppe bereitgestellt. So zeigen Sie eine Liste aller Empfehlungen an 

1. Klicken Sie auf dem Azure Security Center-Dashboard auf **Empfehlungen**.
3. Wählen Sie eine bestimmte Empfehlung aus. Daraufhin wird ein Blatt mit einer Liste von allen Ressourcen geöffnet, für die die Empfehlung gilt.
4. Wählen Sie eine bestimmte Ressource aus, die behandelt werden soll.
5. Führen Sie die Anweisungen auf dem Bildschirm zur Fehlerbehebung durch. 

In vielen Fällen stellt das Azure Security Center Schritte zur Umsetzung der Empfehlung vor, ohne den Azure Security Center-Kontext verlassen zu müssen. Im folgenden Beispiel wurde z.B. eine Netzwerksicherheitsgruppe mit einer uneingeschränkten eingehenden Regel erkannt. Über diese Empfehlung kann die Schaltfläche **Eingehende Regeln bearbeiten** ausgewählt werden, mit der die entsprechende Benutzeroberfläche zur Änderung der Regel aufgerufen wird. 

![Empfehlungen](./media/tutorial-azure-security/remediation.png)

Nachdem Empfehlungen umgesetzt wurden, werden sie als gelöst markiert. 

## <a name="view-detected-threats"></a>Anzeigen erkannter Bedrohungen

Neben den Empfehlungen zur Ressourcenkonfiguration bietet das Azure Security Center auch Warnungen zur Bedrohungserkennung. Die Sicherheitswarnfunktion aggregiert die von den einzelnen VMs gesammelten Daten, Azure-Netzwerkprotokolle und verbundenen Partnerlösungen, um Sicherheitsrisiken für Azure-Ressourcen zu erkennen. Ausführliche Informationen zu Azure Security Center-Bedrohungserkennungsfunktionen finden Sie unter [Azure Security Center-Erkennungsfunktionen](../../security-center/security-center-detection-capabilities.md).

Für die Sicherheitswarnfunktion muss der Azure Security Center-Tarif von *Free* auf *Standard* erhöht werden. Im Anschluss wird eine 30-tägige **kostenlose Testversion** zur Verfügung gestellt. So ändern Sie den Tarif  

1. Klicken Sie auf dem Azure Security Center-Dashboard auf **Sicherheitsrichtlinie**, und wählen Sie dann Ihr Abonnement aus.
2. Klicken Sie auf **Tarif**.
3. Wählen Sie den neuen Tarif aus, und klicken Sie auf **Auswählen**.
5. Klicken Sie auf dem Blatt **Sicherheitsrichtlinie** auf **Speichern**. 

Nach der Aktivierung wird das Diagramm zu den Sicherheitswarnungen aufgefüllt, sobald Sicherheitsbedrohungen erkannt werden.

![Sicherheitswarnungen](./media/tutorial-azure-security/security-alerts.png)

Wählen Sie eine Warnung aus, um Informationen wie eine Beschreibung der Bedrohung, Zeitpunkt der Erkennung, Bedrohungsversuche und die empfohlene Maßnahme zur Fehlerbehebung anzuzeigen. In diesem Beispiel wurde ein RDP-Brute-Force-Angriff mit 294 fehlgeschlagenen RDP-Versuchen erkannt und eine empfohlene Lösung bereitgestellt.

![RDP-Angriff](./media/tutorial-azure-security/rdp-attack.png)
