---
title: "Befolgen von Azure Security Center-Empfehlungen zum Erhöhen der Sicherheit | Microsoft-Dokumentation"
description: " Erfahren Sie, wie mithilfe von Sicherheitsrichtlinien und Empfehlungen in Azure Security Center Sicherheitsangriffe abgewehrt werden können. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2017
ms.author: terrylan
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 36dd149812a0e8c8ca8ef2bac09c1920ec15d3ba
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="use-azure-security-center-recommendations-to-enhance-security"></a>Befolgen von Azure Security Center-Empfehlungen zum Erhöhen der Sicherheit
Sie können die Wahrscheinlichkeit eines Sicherheitsvorfalls verringern, indem Sie eine Sicherheitsrichtlinie und die Azure Security Center-Empfehlungen umsetzen. In diesem Artikel wird erläutert, wie mithilfe von Sicherheitsrichtlinien und Empfehlungen in Security Center Sicherheitsangriffe abgewehrt werden können.

> [!NOTE]
> Dieser Artikel setzt auf den Rollen und Konzepten auf, die im [Planungs- und Betriebshandbuch](security-center-planning-and-operations-guide.md) für Security Center vorgestellt wurden. Es empfiehlt sich, einen Blick in das Planungshandbuch zu werfen, ehe Sie fortfahren.
>
>

## <a name="managing-security-recommendations"></a>Umsetzen von Sicherheitsempfehlungen
In einer Sicherheitsrichtlinie werden die Sicherheitsmechanismen definiert, die für Ressourcen im angegebenen Abonnement oder in der angegebenen Ressourcengruppe zu empfehlen sind. In Security Center definieren Sie Richtlinien auf Grundlage der Sicherheitsanforderungen Ihres Unternehmens. Weitere Informationen finden Sie unter [Festlegen von Sicherheitsrichtlinien in Security Center](security-center-policies.md).

Sicherheitsrichtlinien für Ressourcengruppen werden von der Abonnementebene geerbt.

![Vererbung von Sicherheitsrichtlinien][1]

Falls Sie benutzerdefinierte Richtlinien für bestimmte Ressourcengruppen benötigen, können Sie die Vererbung in der Ressourcengruppe deaktivieren. Legen Sie zum Deaktivieren auf dem Blatt „Sicherheitsrichtlinie“ die Einstellung „Vererbung“ auf „Eindeutig“ fest, und passen Sie die Elemente an, für die Security Center Empfehlungen zeigt.

Wenn Sie also etwa über Workloads verfügen, für die die Richtlinie „Transparent Data Encryption“ für SQL-Datenbank nicht benötigt wird, können Sie die Richtlinie auf Abonnementebene deaktivieren und nur in den Ressourcengruppen aktivieren, in denen sie benötigt wird.

> [!NOTE]
> Bei einem Konflikt zwischen der Richtlinie auf Abonnementebene und der Richtlinie auf Ressourcengruppenebene hat die Richtlinie auf Ressourcengruppenebene Vorrang.
>
>

Security Center analysiert den Sicherheitsstatus Ihrer Azure-Ressourcen. Wenn Security Center potenzielle Sicherheitsrisiken identifiziert, werden basierend auf den in der Sicherheitsrichtlinie festgelegten Elementen Empfehlungen erstellt. Entsprechend den Empfehlungen werden Sie durch den Prozess der Konfiguration der erforderlichen Sicherheitsmaßnahmen geführt.

Aktuelle Empfehlungen in Security Center konzentrieren sich auf Systemupdates, die Betriebssystemkonfiguration, Netzwerksicherheitsgruppen für Subnetze und virtuelle Computer (VMs), die SQL-Datenbanküberwachung, TDE für SQL-Datenbank und Firewalls von Webanwendungen. Die neuesten Security Center-Empfehlungen finden Sie unter [Umsetzen von Sicherheitsempfehlungen in Security Center](security-center-recommendations.md).

## <a name="scenario"></a>Szenario
Dieses Szenario veranschaulicht die Nutzung von Security Center zum Eindämmen der Wahrscheinlichkeit eines erheblichen Sicherheitsvorfalls, indem Security Center-Empfehlungen befolgt und Maßnahmen ergriffen werden. In diesem Szenario werden das fiktive Unternehmen Contoso und die im [Planungs- und Betriebshandbuch](security-center-planning-and-operations-guide.md#security-roles-and-access-controls) vorgestellten Rollen verwendet. Die Rollen stellen Einzelpersonen und Teams dar, die Security Center für verschiedene sicherheitsbezogene Aufgaben verwenden können. Die Rollen lauten:

![Rollen im Szenario][2]

Contoso hat vor Kurzem einige seiner lokalen Ressourcen nach Azure migriert. Contoso möchte Schutzmaßnahmen umsetzen und aufrechterhalten, die die Angriffsfläche seiner Ressourcen in der Cloud verkleinern.

## <a name="recommended-solution"></a>Empfohlene Lösung
Eine Lösung besteht darin, Security Center verwenden, um Sicherheitsrisiken zu verhindern und zu erkennen. Contoso hat im Rahmen seines Azure-Abonnements Zugriff auf Security Center. Der [Free-Tarif](security-center-pricing.md) von Security Center ist automatisch für alle Azure-Abonnements aktiviert. Die Datensammlung ist für alle VMs im Abonnement aktiviert.

David aus der IT-Sicherheitsabteilung von Contoso konfiguriert mithilfe von Security Center eine **Sicherheitsrichtlinie**. Security Center analysiert den Sicherheitsstatus der Azure-Ressourcen von Contoso. Wenn Security Center potenzielle Sicherheitsrisiken ermittelt, werden basierend auf den in der Sicherheitsrichtlinie festgelegten Elementen **Empfehlungen** erstellt.

Jeff, Zuständiger für eine Cloudwordkload, ist gemäß den Sicherheitsrichtlinien von Contoso verantwortlich für die Umsetzung und Aufrechterhaltung von Schutzmaßnahmen. Jeff kann die von Security Center erstellten Empfehlungen überwachen, um Schutzmaßnahmen anzuwenden. Entsprechend den Empfehlungen wird Jeff durch den Prozess der Konfiguration der erforderlichen Sicherheitselemente geführt.

Damit Jeff die Schutzmaßnahmen umsetzen und aufrechterhalten sowie Sicherheitsrisiken beseitigen kann, muss er wie folgt vorgehen:

- Überwachen der von Security Center bereitgestellten Sicherheitsempfehlungen
- Auswerten von Sicherheitsempfehlungen und entscheiden, ob sie umgesetzt werden sollen oder nicht
- Umsetzen von Sicherheitsempfehlungen

Wir wollen nun Jeffs Schritten folgen, um zu prüfen, wie er Security Center-Empfehlungen befolgt, um zum Beseitigen von Sicherheitsrisiken Maßnahmen zu konfigurieren.

## <a name="how-to-implement-this-solution"></a>Implementieren dieser Lösung
Jeff meldet sich beim [Azure-Portal](https://azure.microsoft.com/features/azure-portal/) an und öffnet die Security Center-Konsole. Im Rahmen seiner täglichen Überwachungsaktivitäten prüft er, ob Sicherheitsempfehlungen vorliegen, indem er die folgenden Schritte ausführt:

1. Jeff wählt die Kachel **Empfehlungen** aus, um das Blatt **Empfehlungen** zu öffnen.
   ![Auswählen der Kachel „Empfehlungen“][3]
2. Jeff überprüft die Liste der Empfehlungen. Er sieht, dass Security Center die Liste der Empfehlungen nach Priorität (von der höchsten zur niedrigsten) bereitstellt. Er beschließt, sich mit der Empfehlung mit der Priorität „Hoch“ in der Liste zu beschäftigen. Er wählt auf dem Blatt **Empfehlungen** die Option **Endpoint Protection installieren** aus.
3. Das Blatt **Endpoint Protection installieren** wird mit einer Liste virtueller Computer geöffnet, für die keine Antischadsoftware aktiviert ist. Jeff prüft die Liste der VMs. Er wählt alle VMs und dann **Auf 3 VMs installieren** aus.
   ![Endpoint Protection installieren][4]
4. Das Blatt **Endpoint Protection auswählen** wird geöffnet und stellt Jeff zwei Antischadsoftwarelösungen zur Auswahl. Jeff wählt die Lösung **Microsoft Antimalware** aus.
5. Weitere Informationen zur Antischadsoftware-Lösung werden angezeigt. Jeff klickt auf **Erstellen**.
   ![Microsoft Antimalware][5]
6. Jeff gibt die erforderlichen Konfigurationseinstellungen auf dem Blatt **Installieren** ein und klickt auf **OK**.

[Microsoft Antimalware](../security/azure-security-antimalware.md) ist jetzt auf den ausgewählten VMs aktiv.

Jeff geht die Empfehlungen mit hoher und mittlerer Priorität weiter durch und trifft Entscheidungen zu ihrer Umsetzung. Jeff konsultiert den Artikel [Umsetzen von Sicherheitsempfehlungen](security-center-recommendations.md), um die Empfehlungen und die Auswirkungen bei ihrer Umsetzung zu verstehen.

Jeff erfährt, dass [Microsoft Security Response Center (MSRC)](../security/azure-security-response-center.md) eine selektive Sicherheitsüberwachung im Azure-Netzwerk und in der Infrastruktur durchführt und Bedrohungsanalysendaten und Missbrauchsmeldungen von Drittanbietern empfängt. Nachdem Jeff Sicherheitskontaktinformationen zum Azure-Abonnement von Contoso bereitgestellt hat, wird Contoso von Microsoft kontaktiert, sobald das MSRC feststellt, dass auf die Kundendaten von Contoso ein rechtswidriger oder nicht autorisierter Zugriff erfolgt ist. Sehen wir uns an, wie Jeff die Empfehlung **Sicherheitskontaktinformationen bereitstellen** umsetzt (eine Empfehlung mit dem Schweregrad „Mittel“ in der Liste der zuvor genannten Empfehlungen).

1. Jeff wählt auf dem Blatt **Empfehlungen** die Option **Sicherheitskontaktinformationen bereitstellen** aus, das mit dem Blatt **Sicherheitskontaktinformationen bereitstellen** geöffnet wird.
2. Jeff wählt das Azure-Abonnement aus, für das er Kontaktinformationen angeben möchten. Ein zweites Blatt **Details für Sicherheitskontakt angeben** wird geöffnet.
   ![Sicherheitskontaktinformationen][6]
3. Auf dem zweiten Blatt **Details für Sicherheitskontakt angeben** gibt Jeff Folgendes ein:

  - die durch Kommas getrennten E-Mail-Adressen der Sicherheitskontakte (die Anzahl der E-Mail-Adressen ist hier unbegrenzt)
  - eine Telefonnummer eines Sicherheitskontakts

4. Jeff aktiviert auch die Option **E-Mails zu Warnungen an mich senden**, um E-Mails zu Warnungen mit hohem Schweregrad zu erhalten.
5. Jeff klickt auf **OK**, um die Sicherheitskontaktinformationen für das Abonnement von Contoso zu übernehmen.

Schließlich überprüft Jeff die Empfehlung mit niedriger Priorität **Betriebssystem-Sicherheitsrisiken beheben** und stellt fest, dass diese Empfehlung nicht umsetzbar ist. Er möchte die Empfehlung verwerfen. Jeff klickt auf die drei Punkte, die auf der rechten Seite angezeigt werden, und dann auf **Verwerfen**.
   ![Empfehlung verwerfen][7]

## <a name="conclusion"></a>Zusammenfassung
Das Überwachen von Empfehlungen in Security Center kann helfen, Sicherheitsrisiken zu beseitigen, ehe ein Angriff erfolgt. Sie können einen Sicherheitsvorfall verhindern, indem Sie mithilfe von Sicherheitsrichtlinien in Security Center Schutzmaßnahmen umsetzen und aufrechterhalten.

<!--Image references-->
[1]: ./media/security-center-using-recommendations/security-center-policy-inheritance.png
[2]: ./media/security-center-using-recommendations/scenario-roles.png
[3]: ./media/security-center-using-recommendations/select-recommendations-tile.png
[4]: ./media/security-center-using-recommendations/install-endpoint-protection.png
[5]:./media/security-center-using-recommendations/microsoft-antimalware.png
[6]: ./media/security-center-using-recommendations/provide-security-contact-details.png
[7]: ./media/security-center-using-recommendations/dismiss-recommendation.png

