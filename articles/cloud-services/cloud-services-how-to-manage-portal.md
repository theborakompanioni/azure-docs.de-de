---
title: "Allgemeine Verwaltungsaufgaben für Clouddienste | Microsoft-Dokumentation"
description: "Hier erfahren Sie mehr über die Verwaltung von Clouddiensten im Azure-Portal. In diesen Beispielen wird das Azure-Portal verwendet."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: cb218ad9-77d4-4149-83db-71159c00767e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/27/2016
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: a8f1bf660c44f7716767d3244a7d6e7f7acf8a83
ms.lasthandoff: 04/27/2017


---
# <a name="how-to-manage-cloud-services"></a>Verwalten von Clouddiensten
> [!div class="op_single_selector"]
> * [Azure-Portal](cloud-services-how-to-manage-portal.md)
> * [Klassisches Azure-Portal](cloud-services-how-to-manage.md)
>
>

Ihr Clouddienst wird im Bereich **Clouddienste (klassisch)** des Azure-Portals verwaltet. Dieser Artikel beschreibt einige häufig verwendete Aktionen, die Sie beim Verwalten Ihrer Clouddienste ausführen würden. Dazu zählen Aktualisieren, Löschen, Skalieren und das Heraufstufen einer Bereitstellung zur Produktion.

Weitere Informationen zum Skalieren Ihres Clouddiensts finden Sie [hier](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Aktualisieren einer Clouddienstrolle oder -bereitstellung
Wenn Sie den Anwendungscode für den Clouddienst aktualisieren müssen, verwenden Sie die Option **Aktualisieren** auf dem Blatt für Clouddienste. Sie können eine oder alle Rollen aktualisieren. Zum Aktualisieren können Sie ein neues Dienstpaket oder eine Dienstkonfigurationsdatei hochladen.

1. Wählen Sie im [Azure-Portal][Azure portal] den Clouddienst aus, den Sie aktualisieren möchten. Dieser Schritt öffnet das Blatt für die Clouddienstinstanz.
2. Klicken Sie auf dem Blatt auf die Schaltfläche **Aktualisieren** .

    ![Schaltfläche „Aktualisieren“](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Aktualisieren Sie die Bereitstellung mit einer neuen Dienstpaketdatei (.cspkg) und Dienstkonfigurationsdatei (.cscfg).

    ![Bereitstellung aktualisieren](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **Optional** können Sie die Bereitstellungsbezeichnung und das Speicherkonto aktualisieren.
5. Wenn Rollen nur eine Rolleninstanz haben, aktivieren Sie das Kontrollkästchen **Auch dann bereitstellen, wenn für mindestens eine Rolle nur eine Instanz vorhanden ist** , damit das Upgrade fortgesetzt werden kann.

    Azure kann während des Updates eines Clouddiensts nur dann eine Dienstverfügbarkeit von 99,95 Prozent garantieren, wenn jede Rolle mindestens zwei Rolleninstanzen (virtuelle Computer) hat. Mit zwei Rolleninstanzen kann ein virtueller Computer Clientanforderungen verarbeiten, während der andere aktualisiert wird.

6. Aktivieren Sie **Bereitstellung starten** , damit das Update angewendet wird, nachdem der Upload des Pakets beendet wurde.
7. Klicken Sie auf **OK** , um mit der Aktualisierung des Diensts zu beginnen.

## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Austauschen von Bereitstellungen zum Heraufstufen einer Bereitstellung zur Produktion
Wenn Sie beschließen, eine neue Version eines Clouddiensts bereitzustellen, stellen Sie die neue Version in der Clouddienst-Stagingumgebung bereit, und testen Sie sie dort. Tauschen Sie mit **Austauschen** die URLs aus, mit denen die beiden Bereitstellungen adressiert werden, und stufen Sie eine neue Version zur Produktion herauf.

Sie können Bereitstellungen über die Seite **Cloud-Dienste** oder über das Dashboard austauschen.

1. Wählen Sie im [Azure-Portal][Azure portal] den Clouddienst aus, den Sie aktualisieren möchten. Dieser Schritt öffnet das Blatt für die Clouddienstinstanz.
2. Klicken Sie auf dem Blatt auf die Schaltfläche **Austauschen** .

    ![Austauschen von Cloud-Diensten](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Die folgende Bestätigungsaufforderung wird geöffnet.

    ![Austauschen von Cloud-Diensten](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Klicken Sie nach der Verifizierung der Bereitstellungsinformationen auf **OK** , um die Bereitstellungen auszutauschen.

    Der Austausch der Bereitstellungen erfolgt schnell, da sich nur die virtuellen IP-Adressen für die Bereitstellungen ändern.

    Um Rechenkosten zu sparen, können Sie Ihre Stagingbereitstellung löschen, nachdem Sie verifiziert haben, dass die Produktionsbereitstellung wie erwartet funktioniert.

### <a name="common-questions-about-swapping-deployments"></a>Häufig gestellte Fragen zum Austauschen der Bereitstellungen

**Was sind die Voraussetzungen zum Austauschen von Bereitstellungen?**

Es gibt zwei wichtige Voraussetzungen für einen erfolgreichen Austausch von Bereitstellungen:

- Wenn Sie eine statische IP-Adresse für Ihren Produktionsslot verwenden möchten, müssen Sie auch eine für den Stagingslot reservieren. Andernfalls tritt bei dem Austausch ein Fehler auf.

- Alle Instanzen Ihrer Rollen müssen ausgeführt werden, bevor Sie den Austausch durchführen können. Sie können den Status Ihrer Instanzen im Azure-Portal auf dem Blatt „Übersicht“ oder mit dem [Befehl Get-AzureRole in Windows PowerShell](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) überprüfen.

Beachten Sie, dass Updates von Gastbetriebssystemen und Dienstreparaturvorgänge auch dazu führen können, dass beim Austausch von Bereitstellungen ein Fehler auftritt. Weitere Informationen finden Sie unter [Behandeln von Problemen mit der Clouddienstbereitstellung](cloud-services-troubleshoot-deployment-problems.md).

**Führt ein Austausch zu einer Ausfallzeit für die Anwendung? Wie sollte ich dabei vorgehen?**

Wie im vorherigen Abschnitt beschrieben, erfolgt der Austausch von Bereitstellungen in der Regel sehr schnell, da es sich nur um eine Konfigurationsänderung im Azure Load Balancer handelt. In einigen Fällen kann er jedoch zehn oder mehr Sekunden dauern und zu vorübergehenden Verbindungsausfällen führen. Um die Auswirkungen auf Ihre Kunden zu minimieren, ist es empfehlenswert, [Clientwiederholungslogik](../best-practices-retry-general.md) zu implementieren.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Verknüpfen einer Ressource mit einem Clouddienst
Das Azure-Portal verknüpft Ressourcen nicht miteinander wie das aktuelle klassische Azure-Portal. Stellen Sie stattdessen zusätzliche Ressourcen in der gleichen Ressourcengruppe bereit, die auch vom Clouddienst verwendet wird.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Löschen von Bereitstellungen und eines Clouddiensts
Bevor Sie einen Cloud-Dienst löschen können, müssen Sie die einzelnen bestehenden Bereitstellungen löschen.

Um Rechenkosten zu sparen, können Sie Ihre Stagingbereitstellung löschen, nachdem Sie verifiziert haben, dass die Produktionsbereitstellung wie erwartet funktioniert. Für angehaltene bereitgestellte Rolleninstanzen werden Ihnen Rechenkosten in Rechnung gestellt.

Gehen Sie folgendermaßen vor, um eine Bereitstellung oder Ihren Cloud-Dienst zu löschen.

1. Wählen Sie im [Azure-Portal][Azure portal] den Clouddienst aus, den Sie löschen möchten. Dieser Schritt öffnet das Blatt für die Clouddienstinstanz.
2. Klicken Sie auf dem Blatt auf die Schaltfläche **Löschen** .

    ![Austauschen von Cloud-Diensten](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Sie können den gesamten Clouddienst löschen, indem Sie **Clouddienst und seine Bereitstellungen** aktivieren, oder wählen Sie entweder **Produktionsbereitstellung** oder **Stagingbereitstellung** aus.

    ![Austauschen von Cloud-Diensten](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Klicken Sie unten auf die Schaltfläche **Löschen** .
5. Klicken Sie zum Löschen des Cloud-Diensts auf **Cloud-Dienst löschen**. Klicken Sie dann an der Bestätigungsaufforderung auf **Ja**.

> [!NOTE]
> Wenn ein Clouddienst gelöscht wird, und die ausführliche Überwachung konfiguriert ist, müssen Sie die Daten manuell aus Ihrem Speicherkonto löschen. Informationen zum Speicherort der Metriktabellen finden Sie in [diesem](cloud-services-how-to-monitor.md) Artikel.
>
>

[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Nächste Schritte
* [Allgemeine Konfiguration Ihres Clouddiensts](cloud-services-how-to-configure-portal.md)
* Weitere Informationen zum [Bereitstellen eines Clouddiensts](cloud-services-how-to-create-deploy-portal.md)
* [Konfigurieren eines benutzerdefinierten Domänennamens](cloud-services-custom-domain-name-portal.md)
* Konfigurieren von [SSL-Zertifikaten](cloud-services-configure-ssl-certificate-portal.md)

