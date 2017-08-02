---
title: Just-In-Time-VM-Zugriff in Azure Security Center | Microsoft-Dokumentation
description: "In diesem Dokument wird beschrieben, wie Sie mit Just-In-Time-VM-Zugriff in Azure Security Center den Zugriff auf die virtuellen Azure-Computer steuern können."
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
ms.date: 07/19/2017
ms.author: terrylan
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: ad47c198b98f235b5a75fed6c403493a33594429
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>Verwalten des Zugriffs auf virtuelle Computer mithilfe des Just-In-Time-Features

Mit Just-In-Time-VM-Zugriff kann eingehender Datenverkehr auf den Azure-VMs gesperrt werden, um die Gefährdung durch Angriffe zu reduzieren und bei Bedarf einfachen Zugriff auf Verbindungen mit virtuellen Computern bereitzustellen.

> [!NOTE]
> Das Just-In-Time-Feature befindet sich in der Vorschauphase und ist im Standard-Tarif von Security Center verfügbar.  Weitere Informationen zu den Tarifen von Security Center finden Sie unter [Preise](security-center-pricing.md).
>
>

## <a name="attack-scenario"></a>Angriffsszenario

Bei Brute-Force-Angriffen wird im Allgemeinen versucht, über Verwaltungsports Zugriff auf einen virtuellen Computer zu erlangen. Im Erfolgsfall kann ein Angreifer die Kontrolle über den virtuellen Computer erlangen und in Ihrer Umgebung Fuß fassen.

Eine Möglichkeit, die Gefährdung durch Brute-Force-Angriffe zu verringern, ist das Beschränken der Zeitspanne, in der ein Port geöffnet ist. Verwaltungsports müssen nicht jederzeit geöffnet sein. Sie müssen nur geöffnet sein, während eine Verbindung mit dem virtuellen Computer besteht, z.B. zum Ausführen von Verwaltungs- oder Wartungsaufgaben. Wenn Just-In-Time aktiviert ist, verwendet das Security Center [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md)-Regeln (NSG), die den Zugriff auf Verwaltungsports beschränken, um sie vor Angriffen zu schützen.

![Just-In-Time-Szenario][1]

## <a name="how-does-just-in-time-access-work"></a>Funktionsweise des Just-In-Time-Zugriffs

Wenn Just-In-Time aktiviert ist, sperrt das Security Center durch das Erstellen einer NSG-Regel eingehenden Datenverkehr auf den Azure-VMs. Sie wählen die Ports auf dem virtuellen Computer aus, für die eingehender Datenverkehr gesperrt wird. Diese Ports werden durch die Just-In-Time-Lösung gesteuert.

Wenn ein Benutzer auf einen virtuellen Computer zugreift, überprüft das Security Center, ob der Benutzer über Berechtigungen der [rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC)](../active-directory/role-based-access-control-configure.md) verfügt, die Schreibzugriff für die Azure-Ressource bereitstellen. Wenn er über Schreibberechtigungen verfügt, wird die Anforderung genehmigt, und die Netzwerksicherheitsgruppen (NSGs) werden vom Security Center automatisch so konfiguriert, dass für die angegebene Zeitspanne eingehender Datenverkehr an den Verwaltungsports zugelassen wird. Nach Ablauf dieser Zeitspanne stellt das Security Center die vorherigen Status der NSGs wieder her.

> [!NOTE]
> Der durch das Security Center gesteuerte Just-In-Time-Zugriff auf virtuelle Computer unterstützt derzeit nur virtuelle Computer, die über Azure Resource Manager bereitgestellt wurden. Weitere Informationen über das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell finden Sie unter [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md).
>
>

## <a name="using-just-in-time-access"></a>Verwenden von Just-In-Time-Zugriff

Die Kachel **JIT-VM-Zugriff** im Blatt **Security Center** zeigt die Anzahl der für Just-In-Time-Zugriff konfigurierten virtuellen Computer und die Anzahl der in der letzten Woche genehmigten Zugriffsanforderungen an.

Wählen Sie die Kachel **JIT-VM-Zugriff** aus. Anschließend wird das Blatt **JIT-VM-Zugriff** geöffnet.

![Kachel „JIT-VM-Zugriff“][2]

Das Blatt **JIT-VM-Zugriff** enthält Informationen zum Status Ihrer virtuellen Computer:

- **Konfiguriert** – Virtuelle Computer, die für die Unterstützung von Just-In-Time-Zugriff konfiguriert wurden. Die dargestellten Daten betreffen die letzte Woche und enthalten für jeden virtuellen Computer die Anzahl der genehmigten Anforderungen, Datum und Uhrzeit des letzten Zugriffs sowie den letzten Benutzer.
- **Empfohlen** – Virtuelle Computer, die Just-In-Time-VM-Zugriff unterstützen, jedoch nicht entsprechend konfiguriert wurden. Es wird empfohlen, die Just-In-Time-VM-Zugriffssteuerung für diese virtuellen Computer zu aktivieren. Siehe [Verwalten des Zugriffs auf virtuelle Computer mithilfe von Just-In-Time](#enable-just-in-time-vm-access).
- **Keine Empfehlung** – Mögliche Gründe, dass ein virtueller Computer nicht empfohlen wird:
  - Fehlende NSG – Die Just-In-Time-Lösung erfordert, dass eine NSG festgelegt ist.
  - Klassischer virtueller Computer – Der durch das Security Center gesteuerte Just-In-Time-VM-Zugriff unterstützt derzeit nur virtuelle Computer, die über Azure Resource Manager bereitgestellt wurden. Klassische Bereitstellungen werden durch die Just-In Time-Lösung nicht unterstützt.
  - Andere – Ein virtueller Computer fällt in diese Kategorie, wenn die Just-In-Time-Lösung in der Sicherheitsrichtlinie des Abonnements oder der Ressourcengruppe deaktiviert ist oder wenn der virtuelle Computer über keine öffentliche IP-Adresse und über keine NSG verfügt.

## <a name="configuring-a-just-in-time-access-policy"></a>Konfigurieren einer Just-In-Time-Zugriffsrichtlinie

So wählen Sie den virtuellen Computer aus, den Sie aktivieren möchten:

1. Wählen Sie auf dem Blatt **JIT-VM-Zugriff** die Registerkarte**Empfohlen** aus.

  ![Just-In-Time-Zugriff aktivieren][3]

2. Wählen Sie unter **VMs** die virtuellen Computer aus, die Sie aktivieren möchten. Hierdurch wird der virtuelle Computer mit einem Häkchen markiert.
3. Wählen Sie **JIT auf VMs aktivieren** aus.
4. Wählen Sie **Speichern**aus.

### <a name="default-ports"></a>Standardports

Sie können die Standardports anzeigen, für die das Security Center die Aktivierung von Just-In-Time empfiehlt.

1. Wählen Sie auf dem Blatt **JIT-VM-Zugriff** die Registerkarte**Empfohlen** aus.

  ![Standardportnummern anzeigen][6]

2. Wählen Sie unter **VMs** einen virtuellen Computer aus. Hierdurch wird der virtuelle Computer mit einem Häkchen markiert und das Blatt **Konfiguration des JIT-VM-Zugriffs** geöffnet. Auf diesem Blatt werden die Standardports angezeigt.

### <a name="add-ports"></a>Hinzufügen von Ports

Auf dem Blatt **Konfiguration des JIT-VM-Zugriffs** können Sie auch einen neuen Port, für den Sie die Just-In-Time-Lösung aktivieren möchten, hinzufügen und konfigurieren.

1. Klicken Sie auf dem Blatt **Konfiguration des JIT-VM-Zugriffs** auf **Hinzufügen**. Daraufhin wird das Blatt **Portkonfiguration hinzufügen** geöffnet.

  ![Portkonfiguration][7]

2. Auf dem Blatt **Portkonfiguration hinzufügen** geben Sie den Port, den Protokolltyp, zulässige Quell-IP-Adressen und die maximale Anforderungszeit an.

  Zulässige Quell-IP-Adressen sind die IP-Adressbereiche, auf die der Zugriff zulässig ist, wenn die Anforderung genehmigt wurde.

  Die maximale Anforderungszeit ist das maximale Zeitfenster, in dem ein bestimmter Port geöffnet sein kann.

3. Klicken Sie auf **OK**.

## <a name="requesting-access-to-a-vm"></a>Anfordern des Zugriffs auf einen virtuellen Computer

So fordern Sie Zugriff auf einen virtuellen Computer an:

1. Wählen Sie auf dem Blatt **JIT-VM-Zugriff** die Registerkarte**Konfiguriert** aus.
2. Wählen Sie unter **VMs** die virtuellen Computer aus, für die Sie den Zugriff aktivieren möchten. Hierdurch wird der virtuelle Computer mit einem Häkchen markiert.
3. Wählen Sie **Zugriff anfordern** aus. Daraufhin wird das Blatt **Zugriff anfordern** geöffnet.

  ![Zugriff auf einen virtuellen Computer anfordern][4]

4. Auf dem Blatt **Zugriff anfordern** können Sie für jeden virtuellen Computer die zu öffnenden Ports zusammen mit der Quell-IP-Adresse, für die der Port geöffnet wird, und dem Zeitfenster, in dem der Port geöffnet wird, konfigurieren,. Sie können nur Zugriff auf die in der Just-In-Time-Richtlinie konfigurierten Ports anfordern. Für jeden Port gilt eine maximal zulässige Zeitspanne, die aus der Just-In-Time-Richtlinie abgeleitet ist.
5. Wählen Sie **Ports öffnen** aus.

## <a name="editing-a-just-in-time-access-policy"></a>Bearbeiten einer Just-In-Time-Zugriffsrichtlinie

Sie können die vorhandene Just-In-Time-Richtlinie eines virtuellen Computers ändern, indem Sie einen neuen Port, der für diesen virtuellen Computer geöffnet werden soll, hinzufügen und konfigurieren. Oder Sie ändern einen beliebigen anderen Parameter eines bereits geschützten Ports.

Zum Bearbeiten einer vorhandenen Just-In-Time-Richtlinie eines virtuellen Computers wird die Registerkarte **Konfiguriert** verwendet:

1. Wählen Sie unter **VMs** einen virtuellen Computer aus, dem ein Port hinzugefügt werden soll, indem Sie auf die drei Punkte in der Zeile für den betreffenden virtuellen Computer klicken. Dadurch wird ein Menü geöffnet.
2. Wählen Sie im Menü **Bearbeiten** aus. Daraufhin wird das Blatt **Konfiguration des JIT-VM-Zugriffs** geöffnet.

  ![Richtlinie bearbeiten][8]

3. Auf dem Blatt **Konfiguration des JIT-VM-Zugriffs** können Sie die vorhandenen Einstellungen eines bereits geschützten Ports bearbeiten, indem Sie auf den Port klicken, oder Sie können **Hinzufügen** auswählen. Daraufhin wird das Blatt **Portkonfiguration hinzufügen** geöffnet.

  ![Port hinzufügen][7]

4. Geben Sie auf dem Blatt **Portkonfiguration hinzufügen** den Port, den Protokolltyp, zulässige Quell-IP-Adressen und die maximale Anforderungszeit an.
5. Klicken Sie auf **OK**.
6. Wählen Sie **Speichern**aus.

## <a name="auditing-just-in-time-access-activity"></a>Überwachen der Just-In-Time-Zugriffsaktivität

Mit der Protokollsuche erhalten Sie Einblicke in VM-Aktivitäten. So zeigen Sie Protokolle an:

1. Wählen Sie auf dem Blatt **JIT-VM-Zugriff** die Registerkarte**Konfiguriert** aus.
2. Wählen Sie unter **VMs** einen virtuellen Computer aus, zu dem Informationen angezeigt werden sollen, indem Sie auf die drei Punkte in der Zeile für den betreffenden virtuellen Computer klicken. Dadurch wird ein Menü geöffnet.
3. Wählen Sie im Menü **Aktivitätsprotokoll** aus. Daraufhin wird das Blatt **Aktivitätsprotokoll** geöffnet.

![Aktivitätsprotokoll auswählen][9]

Das Blatt **Aktivitätsprotokoll** bietet eine gefilterte Ansicht der früheren Vorgänge für diesen virtuellen Computer zusammen mit der Uhrzeit, dem Datum und dem Abonnement.

![Aktivitätsprotokoll anzeigen][5]

Sie können die Protokollinformationen herunterladen, indem Sie **Klicken Sie hier, um alle Elemente als CSV herunterzuladen** auswählen.

Ändern Sie die Filter, und wählen Sie **Anwenden** aus, um eine Suche und ein Protokoll zu erstellen.

## <a name="using-just-in-time-vm-access-via-powershell"></a>Verwenden von Just-In-Time-VM-Zugriff mit PowerShell

Um die Just-In-Time-Lösung mit PowerShell zu verwenden, stellen Sie sicher, dass Sie über die [aktuelle](/powershell/azure/install-azurerm-ps) Version von Azure PowerShell verfügen.
Sobald Sie über diese verfügen, müssen Sie das [aktuelle](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.10) Azure Security Center aus dem PowerShell-Katalog installieren.

### <a name="configuring-a-just-in-time-policy-for-a-vm"></a>Konfigurieren einer Just-In-Time-Richtlinie für einen virtuellen Computer

Zum Konfigurieren einer Just-In-Time-Richtlinie für einen bestimmten virtuellen Computer müssen Sie in der PowerShell-Sitzung den folgenden Befehl ausführen: Set-ASCJITAccessPolicy.
Weitere Informationen finden Sie in der Cmdlet-Dokumentation.

### <a name="requesting-access-to-a-vm"></a>Anfordern des Zugriffs auf einen virtuellen Computer

Für den Zugriff auf einen bestimmten virtuellen Computer, der durch die Just-In-Time-Lösung geschützt wird, müssen Sie in der PowerShell-Sitzung den folgenden Befehl ausführen: Invoke-ASCJITAccess.
Weitere Informationen finden Sie in der Cmdlet-Dokumentation.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde Ihnen vermittelt, wie Sie mit Just-In-Time-VM-Zugriff im Security Center den Zugriff auf die virtuellen Azure-Computer steuern können.

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

- [Festlegen von Sicherheitsrichtlinien:](security-center-policies.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verwalten von Sicherheitsempfehlungen:](security-center-recommendations.md) Erfahren Sie, wie Empfehlungen Ihnen beim Schutz Ihrer Azure-Ressourcen helfen.
- [Überwachen der Sicherheitsintegrität:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Verwalten von und Reagieren auf Sicherheitswarnungen:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
- [Überwachen von Partnerlösungen:](security-center-partner-solutions.md) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
- [Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](https://blogs.msdn.microsoft.com/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.


<!--Image references-->
[1]: ./media/security-center-just-in-time/just-in-time-scenario.png
[2]: ./media/security-center-just-in-time/just-in-time.png
[3]: ./media/security-center-just-in-time/enable-just-in-time-access.png
[4]: ./media/security-center-just-in-time/request-access-to-a-vm.png
[5]: ./media/security-center-just-in-time/activity-log.png
[6]: ./media/security-center-just-in-time/default-ports.png
[7]: ./media/security-center-just-in-time/add-a-port.png
[8]: ./media/security-center-just-in-time/edit-policy.png
[9]: ./media/security-center-just-in-time/select-activity-log.png

